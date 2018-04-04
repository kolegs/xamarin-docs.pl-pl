---
title: Dodawanie formatowanie specyficzne dla systemu iOS
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 280ca523d3e3b4f5037d626cc5fd0bd5b31d0e8b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="adding-ios-specific-formatting"></a>Dodawanie formatowanie specyficzne dla systemu iOS

Aby ustawić specyficzne dla systemu iOS formatowania jest utworzenie [niestandardowego modułu renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) formantu i style specyficzne dla platformy zestaw kolorów dla każdej platformy.

Inne opcje, aby kontrolować sposób wyglądu aplikacji dla systemu iOS platformy Xamarin.Forms obejmują:

* Konfigurowanie opcji w wyświetlania [ **Info.plist**](#info-plist)
* Ustawianie stylów formantu za pośrednictwem [ `UIAppearance` interfejsu API](#uiappearance)

Poniżej opisano czynności następujących alternatyw.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Customizing Info.plist

**Info.plist** pliku pozwala skonfigurować niektóre aspekty renderering aplikacji systemu iOS, takie jak jak (i czy) na pasku stanu jest wyświetlany.

Na przykład [próbki Todo](https://developer.xamarin.com/samples/xamarin-forms/Todo/) używa następujący kod do ustawienia nawigacji pasek koloru i koloru tekstu na wszystkich platformach:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Wynik jest wyświetlany we fragmencie ekranu poniżej. Zwróć uwagę, czy czarny elementy paska stanu (to nie można ustawić w ramach platformy Xamarin.Forms ponieważ jest funkcją specyficzne dla platformy).

![](theme-images/status-default-sml.png "iOS motywów")

W idealnym przypadku na pasku stanu może być także białe — coś możemy wykonywać bezpośrednio w projekcie systemu iOS. Dodaj następujące wpisy do **Info.plist** Aby wymusić na pasku stanu biały:

![](theme-images/info-plist.png "iOS Info.plist Entries")

lub Edytuj odpowiadający mu **Info.plist** plik bezpośrednio do obejmują:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Teraz po uruchomieniu aplikacji na pasku nawigacji jest zielony i jego tekstu jest białe (ze względu na platformy Xamarin.Forms formatowania) *i* tekst na pasku stanu jest również białe dzięki konfiguracji specyficznych dla systemu iOS:

![](theme-images/status-white-sml.png "iOS motywów")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance interfejsu API

[ `UIAppearance` Interfejsu API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) może być używany do ustawiania właściwości visual na wiele formantów z systemem iOS *bez* konieczności tworzenia [niestandardowego modułu renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

Dodawanie pojedynczy wiersz kodu w celu **AppDelegate.cs** `FinishedLaunching` metody styl możesz wszystkie formanty, używając danego typu ich `Appearance` właściwości. Poniższy kod zawiera dwa przykłady — globally Ustawianie stylów na karcie pasek i kontrolowanie:

**AppDelegate.cs** w projekcie systemu iOS

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

Domyślnie wybrana karta ikony paska w [ `TabbedPage` ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) byłoby niebieski:

![](theme-images/tabbar-default.png "IOS domyślna ikona paska kartę w TabbedPage")

Aby zmienić to zachowanie, ustaw `UITabBar.Appearance` właściwości:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Powoduje to, że wybrana karta jako zielony:

![](theme-images/tabbar-custom.png "Zielony iOS ikony paska kartę w TabbedPage")

Przy użyciu tego interfejsu API umożliwia dostosowanie wyglądu platformy Xamarin.Forms `TabbedPage` w systemie iOS z kodem bardzo mało. Zapoznaj się [przepisu dostosować karty](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/) Aby uzyskać więcej informacji na temat używania niestandardowego modułu renderowania można ustawić określonej czcionki dla karty.

### <a name="uiswitch"></a>UISwitch

`Switch` Formant jest inny przykład, który można łatwo stylem:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Te przechwytywania ekranu dwóch wyświetlić domyślny `UISwitch` formantu po lewej i dostosowaną wersję (ustawienie `Appearance`) po prawej stronie w [próbki Todo](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "Domyślny kolor UISwitch") ![ ] (theme-images/switch-custom.png "dostosowane UISwitch kolorów")

### <a name="other-controls"></a>Inne formanty

Wiele formantów interfejsu użytkownika dla systemu iOS mogą mieć ich domyślne kolory i inne atrybuty ustawić za pomocą [ `UIAppearance` interfejsu API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## <a name="related-links"></a>Linki pokrewne

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Dostosuj karty](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/)
