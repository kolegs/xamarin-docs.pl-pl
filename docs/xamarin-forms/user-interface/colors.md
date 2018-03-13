---
title: Kolory
description: "Platformy Xamarin.Forms zapewnia elastyczne klasy kolorów i platform."
ms.topic: article
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 9c40d99d80766c5d32a76e424e902bd1b7d141ba
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="colors"></a>Kolory

_Platformy Xamarin.Forms zapewnia elastyczne klasy kolorów i platform._

W tym artykule przedstawiono różne sposoby `Color` klasa może być używana w platformy Xamarin.Forms.

`Color` Klasa dostarcza metody do tworzenia wystąpienia kolorów

-  **O nazwie kolory** -zbiór typowych o nazwie kolorów, w tym `Red`, `Green`, i `Blue`.
-  **FromHex** — podobnie jak składni stosowanej w języku HTML, np "00FF00" wartość ciągu. Alfa jest opcjonalnie można określić jako pierwszy parę znaków ("CC00FF00").
-  **FromHsla** -odcień, nasycenie i jasność `double` wartości z opcjonalną wartość alfa (0.0 do 1.0).
-  **FromRgb** -czerwony, zielony i niebieski `int` wartości (0 – 255).
-  **FromRgba** -czerwony, zielony, niebieski i alfa `int` wartości (0 – 255).
-  **FromUint** -ustawić jeden `double` reprezentujący wartość **argb**.

Poniżej przedstawiono niektóre przykładowe kolorów, `BackgroundColor` niektóre etykiet przy użyciu różnych odmian dozwolonych składni:

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

Kolorów te są wyświetlane na każdej z platform. Zwróć uwagę, kolor końcowy - `Accent` -blue-ish kolor dla systemów iOS i Android; ta wartość jest definiowana przez platformy Xamarin.Forms. Na Windows Phone `Accent` pokazuje kolorem czerwonym *ponieważ jest to kolor akcentu wybrane przez użytkownika dla tego urządzenia*; zmiany tej wartości w zależności od preferencji użytkownika.

 [![Pokaz kolor](colors-images/colors-sml.png "pokaz kolor")](colors-images/colors.png#lightbox "pokaz kolorów")

## <a name="colordefault"></a>Color.Default

Użyj `Default` Ustaw (lub ponownie ustawić) wartości koloru domyślną platformy (opis, że jest to inny kolor podstawowy na każdej z platform dla każdej właściwości).

Deweloperzy mogą używać tę wartość można ustawić `Color` właściwości ale powinny **nie** zapytanie tego wystąpienia dla jego składnika wartości RGB (one wszystko jest gotowe do -1).

## <a name="colortransparent"></a>Color.Transparent

Ustaw kolor, aby wyczyścić.

## <a name="coloraccent"></a>Color.Accent

Windows Phone jest kolor dopełniający wybierany przez użytkownika. Dobrym aplikacji Windows Phone umożliwia jako część ich stylów udostępnienie natywnego wyglądu i działania.

W systemach iOS i Android tego wystąpienia wynosi kontrastowy kolor, który jest widoczny na domyślne tło, ale nie jest taka sama jak domyślny kolor tekstu.

## <a name="additional-methods"></a>Dodatkowe metody

`Color` wystąpienia obejmują dodatkowe metody, które mogą służyć do tworzenia nowych kolorów:

-  **AddLuminosity** — zwraca nowy kolor, modyfikując jasność przez podany różnicowe.
-  **WithHue** — zwraca nowy kolor, zastępując odcień dostarczona wartość.
-  **WithLuminosity** — zwraca nowy kolor, zastępując jasność dostarczona wartość.
-  **WithSaturation** — zwraca nowy kolor, zastępując nasycenie dostarczona wartość.
-  **MultiplyAlpha** — zwraca nowy kolor, modyfikując alfa mnożenie go przez podana wartość alfa.

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

Następujący fragment kodu używa `Device.RuntimePlatform` właściwości można selektywnie ustawić kolor `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>Korzystanie z języka XAML

Kolory można też łatwo odwoływać się w języku XAML, przy użyciu nazw koloru zdefiniowanego lub reprezentacje Hex, pokazano poniżej:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

## <a name="summary"></a>Podsumowanie

Platformy Xamarin.Forms `Color` klasa jest używana do tworzenia odwołań kolor obsługujący platformy. Może służyć w udostępnionym kodu i języka XAML.


## <a name="related-links"></a>Linki pokrewne

- [ColorsSample](https://developer.xamarin.com/samples/WorkingWithColors)
- [Selektor powiązania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
