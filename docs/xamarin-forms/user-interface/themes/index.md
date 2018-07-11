---
title: Motywy zestawu narzędzi Xamarin.Forms
description: W tym artykule przedstawiono motywy Xamarin.Forms zdefiniowanie określonych wystąpień visual dla widoków standardowych.
ms.prod: xamarin
ms.assetid: 3DFB7C55-69F6-4980-A501-588719143482
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 0f49eeba072d6aeb7ead40d5d56d4af9e9bf5e27
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38814710"
---
# <a name="xamarinforms-themes"></a>Motywy zestawu narzędzi Xamarin.Forms

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji zapoznawczej")

Motywy Xamarin.Forms zostały ogłoszeniem na konferencji rozwijających 2016 i są dostępne w wersji zapoznawczej dla klientów i spróbuj przekazać opinię.

Motyw jest dodawany do aplikacji platformy Xamarin.Forms, umieszczając **Xamarin.Forms.Theme.Base** pakietu Nuget, a także dodatkowe pakiet, który definiuje określone motyw (np. Xamarin.Forms.Theme.Light) lub inne można zdefiniować motyw lokalne dla aplikacji.

Zapoznaj się [motyw jasny](light.md) i [motyw ciemny](dark.md) strony, aby uzyskać instrukcje dotyczące sposobu dodawania ich do aplikacji lub zapoznaj się z [przykład niestandardowy motyw](custom.md).

**Ważne:** należy również wykonać kroki, aby [ładować zestawy motywu (poniżej)](#loadtheme) przez dodanie niektórych schematyczny kod służący do systemu iOS `AppDelegate` i Android `MainActivity`. To zostanie on ulepszony w wersji zapoznawczej w przyszłości.


## <a name="control-appearance"></a>Wygląd formantu

[Światła](light.md) i [ciemny](dark.md) motywy zarówno zdefiniować określone wygląd dla standardowych kontrolek. Po dodaniu motyw do słownika zasobów aplikacji zmieni się wygląd formantów standardowych.

Następujący kod XAML przedstawia niektóre formanty standardowe:

```xaml
<StackLayout Padding="40">
    <Label Text="Regular label" />
    <Entry Placeholder="type here" />
    <Button Text="OK" />
    <BoxView Color="Yellow" />
    <Switch />
</StackLayout>
```

Te zrzuty ekranu Pokaż te kontrolki:

* Nie zastosowano motyw
* Motyw jasny (tylko drobne różnice, by o bez motywu)
* Motyw ciemny

![](images/standard-none-sml.png "Formanty bez tworzenia motywów") ![](images/standard-light-sml.png "formantów z motywu jasny") ![](images/standard-dark-sml.png "formantów z ciemnego motywu")

<a name="styleclass" />

## <a name="styleclass"></a>StyleClass

`StyleClass` Właściwość umożliwia wygląd widoku, aby można zmienić zgodnie z definicją, dostarczone przez motywu.

[Światła](light.md) i [ciemny](dark.md) motywy zarówno zdefiniować trzech różnych wystąpień dla `BoxView`: `HorizontalRule`, `Circle`, i `Rounded`. Ten kod przedstawia trzy różne `BoxView`s przy użyciu innego stylu klasy stosowane:

```xaml
<StackLayout Padding="40">
    <BoxView StyleClass="HorizontalRule" />
    <BoxView StyleClass="Circle" />
    <BoxView StyleClass="Rounded" />
</StackLayout>
```

Renderuje to jasny i ciemny w następujący sposób:

![](images/boxview-light-sml.png "BoxView z StyleClass motywu jasny") ![](images/boxview-dark-sml.png "BoxView z StyleClass ciemny motyw")

<a name="builtin" />

## <a name="built-in-classes"></a>Wbudowane klasy

Oprócz automatycznie style wspólnej kontroluje światła i motywów ciemny obecnie obsługuje następujące klasy, które mogą być stosowane przez ustawienie `StyleClass` tych kontrolek:

**BoxView**

* HorizontalRule
* Okrąg
* Zaokrąglone

**Obraz**

* Okrąg
* Zaokrąglone
* Miniatura

**Przycisk**

* Domyślny
* Podstawowy
* Powodzenie
* Informacje o
* Ostrzeżenie
* Zagrożenia
* Łącze
* Małe
* Duże

**Etykieta**

* nagłówek
* Subheader
* Treść
* Łącze
* Odwrotne


## <a name="troubleshooting"></a>Rozwiązywanie problemów

<a name="loadtheme" />

### <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Nie można załadować pliku lub zestawu "Xamarin.Forms.Theme.Light" lub jednej z jego zależności

W wersji zapoznawczej motywów nie można załadować w czasie wykonywania. Dodaj kod pokazany poniżej do odpowiednie projekty, aby naprawić ten błąd.

**iOS**

W **AppDelegate.cs** Dodaj następujące wiersze po `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

W **MainActivity.cs** Dodaj następujące wiersze po `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```


## <a name="related-links"></a>Linki pokrewne

- [Przykładowe ThemesDemo](https://github.com/xamarin/xamarin-forms-samples/tree/master/Themes/ThemesDemo)
