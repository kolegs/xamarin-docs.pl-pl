---
title: Dodawanie formatowania specyficzne dla systemu iOS
description: W tym artykule wyjaśniono, jak ustawić wygląd specyficzne dla systemu iOS bez korzystania z niestandardowego modułu renderowania zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 3b8a440617dedfbe23f869e865b3cedae21d6c5b
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241380"
---
# <a name="adding-ios-specific-formatting"></a>Dodawanie formatowania specyficzne dla systemu iOS

Jednym ze sposobów, aby ustawić specyficzne dla systemu iOS formatowania jest utworzenie [niestandardowego modułu renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) dla formantu i style właściwe dla platformy zestawu kolorów dla każdej platformy.

Inne opcje, aby kontrolować sposób wygląd aplikacji dla systemu iOS Xamarin.Forms obejmują:

* Konfigurowanie wyświetlić opcje w [ **Info.plist**](#info-plist)
* Ustawianie stylów kontroli za pośrednictwem [ `UIAppearance` interfejsu API](#uiappearance)

Te możliwości zostały podane poniżej.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Dostosowywanie pliku Info.plist

**Info.plist** pliku pozwala skonfigurować niektóre aspekty renderering aplikacji systemu iOS, takie jak jak (i czy) jest wyświetlana na pasku stanu.

Na przykład [przykładowe Todo](https://developer.xamarin.com/samples/xamarin-forms/Todo/) używa następujący kod w celu ustawienia nawigacji paska koloru i kolor tekstu na wszystkich platformach:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Wynik jest wyświetlany we fragmencie ekranu poniżej. Zauważ, że elementy paska stanu są czarny (nie można ustawić w ramach zestawu narzędzi Xamarin.Forms, ponieważ jest to funkcja specyficzne dla platformy).

![](theme-images/status-default-sml.png "Motyw systemu iOS")

Najlepiej na pasku stanu jest również biały - coś, co możemy osiągnąć bezpośrednio w projekcie dla systemu iOS. Dodaj następujące wpisy na **Info.plist** wymusić pasek stanu, aby zostać umieszczone na:

![](theme-images/info-plist.png "Wpisy w pliku Info.plist w systemie iOS")

lub edytować odpowiednich **Info.plist** plik bezpośrednio do dołączenia:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Teraz po uruchomieniu aplikacji na pasku nawigacyjnym jest zielony, a jego tekstu biały (ze względu na platformie Xamarin.Forms formatowania) *i* tekst na pasku stanu jest również białe dzięki konfiguracji specyficznych dla systemu iOS:

![](theme-images/status-white-sml.png "Motyw systemu iOS")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance interfejsu API

[ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) może służyć do ustawiania właściwości visual na wielu formantów dla systemu iOS *bez* konieczności tworzenia [niestandardowego modułu renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

Dodanie jednego wiersza kodu w celu **AppDelegate.cs** `FinishedLaunching` metody można zastosować styl do wszystkich kontrolek danego typu przy użyciu ich `Appearance` właściwości. Poniższy kod zawiera dwa przykłady — globalnie style na karcie paska, a następnie przełącz sterowania:

**AppDelegate.cs** w projekcie dla systemu iOS

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
  // tab bar
    UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
  // switch
    UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
    // required Xamarin.Forms code
    Forms.Init ();
    LoadApplication (new App ());
    return base.FinishedLaunching (app, options);
}
```

### <a name="uitabbar"></a>UITabBar

Domyślnie kartę wybraną ikonę paska w [ `TabbedPage` ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) będzie niebieski:

![](theme-images/tabbar-default.png "Domyślne dla systemu iOS ikony paska kartę w TabbedPage")

Aby zmienić to zachowanie, ustaw `UITabBar.Appearance` właściwości:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Powoduje to, że wybrany kartę, aby być w kolorze zielonym:

![](theme-images/tabbar-custom.png "Kolor zielony dla systemu iOS ikony paska kartę w TabbedPage")

Za pomocą tego interfejsu API umożliwia dostosowanie wyglądu Xamarin.Forms `TabbedPage` w systemie iOS za pomocą bardzo niewielkiej ilości kodu. Zapoznaj się [przepisu Dostosowywanie karty](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs) Aby uzyskać więcej informacji na temat korzystania z niestandardowego modułu renderowania można ustawić określonej czcionki dla karty.

### <a name="uiswitch"></a>UISwitch

`Switch` Formant jest inny przykład, w którym można łatwo różne:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Te przechwytywania ekranu dwóch wyświetlić domyślny `UISwitch` kontrolki z lewej strony oraz dostosowaną wersję (ustawienie `Appearance`) po prawej stronie w [przykładowe Todo](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "Domyślny kolor UISwitch") ![](theme-images/switch-custom.png "dostosowany kolor UISwitch")

### <a name="other-controls"></a>Inne kontrolki

Wiele kontrolek interfejsu użytkownika dla systemu iOS mogą mieć ich domyślne kolory i inne atrybuty, które można ustawić przy użyciu [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## <a name="related-links"></a>Linki pokrewne

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Dostosowywanie karty](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
