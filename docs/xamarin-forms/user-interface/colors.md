---
title: Kolory w interfejsie Xamarin.Forms
description: Zestaw narzędzi Xamarin.Forms zapewnia elastyczne Color class dla wielu platform. W tym artykule opisano funkcje udostępniane przez Color class i jak z niej korzystać.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 1017f108d6808155cac84e98a811a30d09afa134
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986086"
---
# <a name="colors-in-xamarinforms"></a>Kolory w interfejsie Xamarin.Forms

_Zestaw narzędzi Xamarin.Forms zapewnia elastyczne Color class dla wielu platform._

W tym artykule przedstawiono różne sposoby `Color` klasa może być używana w interfejsie Xamarin.Forms.

`Color` Klasa udostępnia wiele metod do tworzenia wystąpienia kolorów

-  **Nazwane kolory** — zbiór typowych o nazwie kolorów, w tym `Red`, `Green`, i `Blue`.
-  **FromHex** — podobnie jak składnią używaną w języku HTML, na przykład "00FF00" wartość ciągu. Składnik alfa jest opcjonalnie można określić jako pierwszą pary znaków ("CC00FF00").
-  **FromHsla** -odcień, nasycenie i jasność `double` wartościami opcjonalna wartość alfa (0.0 do 1.0).
-  **FromRgb** -czerwony, zielony i niebieski `int` wartości (0 – 255).
-  **FromRgba** -czerwony, zielony, niebieski i alfa `int` wartości (0 – 255).
-  **FromUint** — ustawianie pojedynczego `double` wartość reprezentującą informację o **argb**.

Poniżej przedstawiono niektóre przykładowe kolorów, do `BackgroundColor` niektóre etykiet przy użyciu różnych odmian dozwolonych składni:

```csharp
var red    = new Label { Text = "Red",   BackgroundColor = Color.Red };
var orange = new Label { Text = "Orange",BackgroundColor = Color.FromHex("FF6A00") };
var yellow = new Label { Text = "Yellow",BackgroundColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
var green  = new Label { Text = "Green", BackgroundColor = Color.FromRgb (38, 127, 0) };
var blue   = new Label { Text = "Blue",  BackgroundColor = Color.FromRgba(0, 38, 255, 255) };
var indigo = new Label { Text = "Indigo",BackgroundColor = Color.FromRgb (0, 72, 255) };
var violet = new Label { Text = "Violet",BackgroundColor = Color.FromHsla(0.82, 1, 0.25, 1) };

var transparent = new Label { Text = "Transparent",BackgroundColor = Color.Transparent };
var @default = new Label    { Text = "Default",    BackgroundColor = Color.Default };
var accent = new Label      { Text = "Accent",     BackgroundColor = Color.Accent };
```

Te kolory są wyświetlane na każdej z platform. Zwróć uwagę, ostateczny kolor - `Accent` -blue-ish kolor dla systemów iOS i Android; ta wartość jest definiowana przez zestaw narzędzi Xamarin.Forms.

 [![Pokaz kolor](colors-images/colors-sml.png "pokaz kolor")](colors-images/colors.png#lightbox "pokaz kolorów")

## <a name="colordefault"></a>Color.Default

Użyj `Default` Ustaw (lub ponownie ustawić) wartość koloru domyślną platformy (zrozumienie, że jest to inny kolor podstawowy na każdej platformie dla każdej właściwości).

Deweloperzy mogą używać tej wartości można ustawić `Color` właściwość ale powinny **nie** zapytania tego wystąpienia na jego składnik wartości RGB (one wszystko jest gotowe do -1).

## <a name="colortransparent"></a>Color.Transparent

Ustaw kolor, aby wyczyścić.

## <a name="coloraccent"></a>Color.Accent

W systemach iOS i Android tego wystąpienia jest równa kontrastowy kolor, który jest widoczny na domyślne tło, ale nie jest taki sam jak domyślny kolor tekstu.

## <a name="additional-methods"></a>Dodatkowe metody

`Color` wystąpienia obejmują dodatkowe metody, które mogą służyć do tworzenia nowych kolorów:

-  **AddLuminosity** — zwraca nowy kolor, modyfikując jasność przez podany delta.
-  **WithHue** — zwraca nowy kolor, zastępując odcień dostarczona wartość.
-  **WithLuminosity** — zwraca nowy kolor, zastępując jasność dostarczona wartość.
-  **WithSaturation** — zwraca nowy kolor, zastępując nasycenie dostarczona wartość.
-  **MultiplyAlpha** — zwraca nowy kolor, modyfikując alfa przemnożenie przez podana wartość alfa.

## <a name="implicit-conversions"></a>Niejawne konwersje

Niejawna konwersja między elementem `Xamarin.Forms.Color` i `System.Drawing.Color` typy mogą być wykonywane:

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

Ten fragment kodu używa `Device.RuntimePlatform` właściwość selektywnie Ustawianie koloru `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>Przy użyciu XAML

Kolorów można również łatwo przywoływać w XAML przy użyciu nazw zdefiniowany kolor lub reprezentacje szesnastkowy, pokazano poniżej:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> Korzystając z kompilacji XAML, nazw kolorów jest rozróżniana wielkość liter i mogą być zapisywane małymi literami. Aby uzyskać więcej informacji na temat kompilacji XAML, zobacz [kompilacji XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="summary"></a>Podsumowanie

Xamarin.Forms `Color` klasa jest używana do tworzenia odwołań obsługujących platformy kolorów. Może służyć w współużytkowanym kodem i XAML.


## <a name="related-links"></a>Linki pokrewne

- [ColorsSample](https://developer.xamarin.com/samples/WorkingWithColors)
- [Selektor możliwej do wiązania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
