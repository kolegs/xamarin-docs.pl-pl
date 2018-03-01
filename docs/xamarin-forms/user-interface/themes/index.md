---
title: Motywy
ms.topic: article
ms.prod: xamarin
ms.assetid: 3DFB7C55-69F6-4980-A501-588719143482
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: a62d6c0cb9b6c41ebf3f2a6e4bd350f9ace986f6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="themes"></a>Motywy

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji zapoznawczej")

Kompozycje platformy Xamarin.Forms zostały ogłoszenia na Evolve 2016 i są dostępne jako podglądu dla klientów i spróbuj przekazać opinię.

Motyw zostanie dodany do aplikacji platformy Xamarin.Forms przy tym **Xamarin.Forms.Theme.Base** pakietu Nuget, oraz dodatkowe pakietu, który definiuje motyw określonych (np.) Xamarin.Forms.Theme.Light) lub #else motyw lokalnego można zdefiniować dla aplikacji.

Zapoznaj się [motywu jasny](light.md) i [ciemny motyw](dark.md) strony, aby uzyskać instrukcje dotyczące sposobu dodawania ich do aplikacji lub zapoznaj się z [motyw niestandardowy przykład](custom.md).

**Ważne:** należy również wykonać kroki, aby [ładowanie zestawów motywu (poniżej)](#loadtheme) przez dodanie niektórych schematyczny kod służący do systemu iOS `AppDelegate` i Android `MainActivity`. Spowoduje to można zwiększyć w przyszłości zapoznawczą.


## <a name="control-appearance"></a>Wygląd formantu

[Światła](light.md) i [ciemny](dark.md) motywów zarówno zdefiniuj określonych wygląd dla standardowych formantów. Po dodaniu motyw do słownika zasobów aplikacji zmieni się wygląd standardowych kontrolek.

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

Te zrzuty ekranu pokazują tych kontrolek z:

* Nie zastosowano motyw
* Motywu jasny (tylko niewielkie różnice mającej bez motywu)
* Ciemny motyw

![](images/standard-none-sml.png "Formanty bez tworzenia motywów") ![ ] (images/standard-light-sml.png "formantów z motywu jasny") ![ ] (images/standard-dark-sml.png "formantów z ciemnego motywu")

<a name="styleclass" />

## <a name="styleclass"></a>StyleClass

`StyleClass` Właściwość umożliwia wygląd widoku można zmienić zgodnie z definicji podał motywu.

[Światła](light.md) i [ciemny](dark.md) motywów zarówno zdefiniować trzy różne typy wyglądu dla `BoxView`: `HorizontalRule`, `Circle`, i `Rounded`. Ten kod przedstawia trzy różne `BoxView`s z klasami inny styl stosowane:

```xaml
<StackLayout Padding="40">
    <BoxView StyleClass="HorizontalRule" />
    <BoxView StyleClass="Circle" />
    <BoxView StyleClass="Rounded" />
</StackLayout>
```

To powoduje jasny i ciemny w następujący sposób:

![](images/boxview-light-sml.png "BoxView z StyleClass motywu jasny") ![ ] (images/boxview-dark-sml.png "BoxView z StyleClass ciemny motyw")

<a name="builtin" />

## <a name="built-in-classes"></a>Wbudowanych klas

Oprócz automatycznie style typowe kontrolki jasnym i ciemny motywów obsługuje obecnie następujących klas, które mogą być stosowane przez ustawienie `StyleClass` tych kontrolek:

**BoxView**

* HorizontalRule
* koło
* Zaokrąglone

**Obraz**

* koło
* Zaokrąglone
* Miniatur

**Przycisk**

* Domyślny
* podstawowy
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
* Treści
* Łącze
* Odwrotność


## <a name="troubleshooting"></a>Rozwiązywanie problemów

<a name="loadtheme"/>

### <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Nie można załadować pliku lub zestawu 'Xamarin.Forms.Theme.Light' lub jednej z jego zależności

W wersji zapoznawczej kompozycji nie można załadować w czasie wykonywania. Dodaj kod przedstawione poniżej do odpowiednich projektów, aby naprawić ten błąd.

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
