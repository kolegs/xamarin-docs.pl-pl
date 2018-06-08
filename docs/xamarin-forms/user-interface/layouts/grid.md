---
title: Siatka
description: Przedstawia widoków w siatkach.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 6ff36f511c5194017afd34601fc9ea2f89b1e2d4
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848112"
---
# <a name="grid"></a>Siatka

[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) obsługuje rozmieszczanie widoków do wierszy i kolumn. Rozmiary proporcjonalne lub bezwzględną rozmiary można ustawić wierszy i kolumn. `Grid` Układu nie należy mylić z tradycyjnego tabel i nie jest przeznaczona do prezentowania danych tabelarycznych. `Grid` nie ma pojęcie wierszy, kolumny lub komórki formatowania. W przeciwieństwie do tabel HTML `Grid` służy wyłącznie do rozmieszczania zawartości.

[![](grid-images/layouts-sml.png "Układy platformy Xamarin.Forms")](grid-images/layouts.png#lightbox "układów platformy Xamarin.Forms")

W tym artykule opisano:

- **[Cel](#Purpose)**  &ndash; typowe zastosowania dla `Grid`.
- **[Użycie](#Usage)**  &ndash; sposób użycia `Grid` do osiągnięcia żądanej projektu.
  - **[Wiersze i kolumny](#Rows_and_Columns)**  &ndash; Określ wierszy i kolumn `Grid`.
  - **[Wprowadzenie do widoków](#Placing_Views)**  &ndash; dodawać widoki do siatki w określonych wierszy i kolumn.
  - **[Odstępy](#Spacing)**  &ndash; skonfigurować odstęp między wierszy i kolumn.
  - **[Zakresy](#Spans)**  &ndash; skonfigurować elementy zakresu wielu wierszy lub kolumn.

![](grid-images/grid.png "Eksploracja siatki")

## <a name="purpose"></a>Cel

`Grid` może być stosowany do tworzenia widoków w siatce. Jest to przydatne w wielu przypadkach:

- Rozmieszczanie przycisków w aplikacji Kalkulator
- Rozmieszczenie przycisków/wyborów w siatce, takich jak systemu iOS lub Android ekrany macierzystego
- Rozmieszczanie widoków, które będą miały taki sam rozmiar w jednym wymiarze (jak w niektóre paski narzędzi)

## <a name="usage"></a>Użycie

W przeciwieństwie do tradycyjnych tabel `Grid` nie są rozpoznawane liczby i rozmiarów wierszy i kolumn z zawartości. Zamiast tego `Grid` ma `RowDefinitions` i `ColumnDefinitions` kolekcji. Definicje liczby wierszy i kolumn układane będą one przechowywania. Widoki są dodawane do `Grid` z określonych wiersza i kolumny indeksów, które zidentyfikować które wiersza i kolumny widoku powinna zostać umieszczona w.

<a name="Rows_and_Columns" />

### <a name="rows-and-columns"></a>Wierszy i kolumn

Wiersz i kolumnę informacje są przechowywane w `Grid`w `RowDefinitions`  &  `ColumnDefinitions` właściwości, które są kolekcjami każdego z [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) i [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/)obiekty odpowiednio. `RowDefinition` ma tylko jedną właściwość `Height`, i `ColumnDefinition` ma tylko jedną właściwość `Width`. Opcje wysokość i szerokość są następujące:

- **Automatycznie** &ndash; automatycznie rozmiary w celu dopasowania do zawartości w wierszy lub kolumn. Określony jako [ `GridUnitType.Auto` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/) w języku C# lub jako `Auto` w języku XAML.
- **Proportional(*)** &ndash; rozmiarów kolumn i wierszy jako część pozostałe miejsce. Określona jako wartość i `GridUnitType.Star` w języku C# i jako `#*` w języku XAML, z `#` trwa żądaną wartość. Określenie jednego wiersza/kolumny z `*` spowoduje jego wypełnienia dostępnego miejsca.
- **Bezwzględny** &ndash; rozmiarów kolumn i wierszy z określonych, stałej wartości szerokości i wysokości. Określona jako wartość i `GridUnitType.Absolute` w języku C# i jako `#` w języku XAML, z `#` trwa żądaną wartość.

> [!NOTE]
> Wartości szerokości kolumn są ustawione jako "*" Domyślnie w platformy Xamarin.Forms co zapewnia, że kolumna zostanie wypełnienia dostępnego miejsca.

Należy wziąć pod uwagę aplikację, która potrzebuje trzy wiersze i dwie kolumny. Dolny wiersz musi być dokładnie 200px wysokość i górnym wierszu musi być dwa razy tak wysokie jako środkową wiersz. Kolumna po lewej stronie musi być dostatecznie szerokie, aby dopasować zawartość i prawa kolumna musi wypełnienia pozostałego miejsca.

W języku XAML:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="2*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="200" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

W języku C#:

```csharp
var grid = new Grid();
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(2, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(200)});
grid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (200) });
```

<a name="Placing_Views" />

### <a name="placing-views-in-a-grid"></a>Wprowadzenie do widoków w siatce

Aby umieścić widoków w `Grid` należy dodać je jako elementy podrzędne do siatki, a następnie określ, które wierszy i kolumn należą w.

W języku XAML, użyj `Grid.Row` i `Grid.Column` o każdym poszczególnych widoku, aby określić umieszczania. Należy pamiętać, że `Grid.Row` i `Grid.Column` Określ lokalizację opartych na listach liczony od zera, wierszy i kolumn. Oznacza to, że w siatce 4 x 4 lewej górnej komórki jest (0,0) i prawej dolnej komórki jest (3,3).

`Grid` Pokazano poniżej zawiera cztery komórek:

![](grid-images/label-grid.png "Siatkę z czterech widoków")

W języku XAML:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Label Text="Top Left" Grid.Row="0" Grid.Column="0" />
  <Label Text="Top Right" Grid.Row="0" Grid.Column="1" />
  <Label Text="Bottom Left" Grid.Row="1" Grid.Column="0" />
  <Label Text="Bottom Right" Grid.Row="1" Grid.Column="1" />
</Grid>
```

W języku C#:

```csharp
var grid = new Grid();

grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});

var topLeft = new Label { Text = "Top Left" };
var topRight = new Label { Text = "Top Right" };
var bottomLeft = new Label { Text = "Bottom Left" };
var bottomRight = new Label { Text = "Bottom Right" };

grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);
```

Powyższy kod tworzy siatki z czterech etykiet, dwóch kolumn i dwa wiersze. Należy pamiętać, że każda etykieta będą miały taki sam rozmiar i że wierszy zostanie rozwinięty w celu użycia całe dostępne miejsce.

W powyższym przykładzie widoki są dodawane do [ `Grid.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.Children/) przy użyciu kolekcji [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/) przeciążenia, które określa argumenty lewego i górnego. Korzystając z [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) przeciążenia, które określa po lewej, prawej górnej i dolnej argumentów, podczas po lewej stronie i argumentów top będzie zawsze odwołuje się do komórek w [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), prawo i argumenty dolnej może się odwoływać się do komórek znajdujących się poza `Grid`. To dlatego prawy argument zawsze musi być większa niż argument po lewej stronie, a argument dolnej zawsze musi być większa niż top argument. W poniższym przykładzie przedstawiono równoważne kodu za pomocą obu `Add` przeciążenia:

```csharp
// left, top
grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);

// left, right, top, bottom
grid.Children.Add(topLeft, 0, 1, 0, 1);
grid.Children.Add(topRight, 1, 2, 0, 1);
grid.Children.Add(bottomLeft, 0, 1, 1, 2);
grid.Children.Add(bottomRight, 1, 2, 1, 2);
```

### <a name="spacing"></a>Odstępy

`Grid` ma właściwości, aby kontrolować odstępy między wierszy i kolumn.  Następujące właściwości są dostępne dla dostosowywanie `Grid`:

- **ColumnSpacing** &ndash; ilość miejsca między kolumnami.
- **RowSpacing** &ndash; ilość miejsca między wierszami.

Określa następujące XAML `Grid` z kolumnami, jeden wiersz i 5 pikseli odstępy między kolumnami:

```xaml
<Grid ColumnSpacing="5">
  <Grid.ColumnDefinitions>
    <ColumnDefinitions Width="*" />
    <ColumnDefinitions Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

W języku C#:

```csharp
var grid = new Grid { ColumnSpacing = 5 };
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
```

### <a name="spans"></a>Zakresy

Często, podczas pracy z siatki, Brak elementu, który będzie zajmować więcej niż jeden wiersz lub kolumnę. Należy wziąć pod uwagę aplikacji prosty kalkulator:

![](grid-images/calculator.png "Calulator aplikacji")

Zwróć uwagę, przycisk 0 obejmuje dwie kolumny, podobnie jak na wbudowanych kalkulatory dla każdej platformy. Jest to realizowane przy użyciu `ColumnSpan` właściwość, która określa liczbę kolumn jako element będzie zajmować. XAML dla tego przycisku:

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

W języku C#:

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

Należy pamiętać, ten kod w metod statycznych `Grid` klasy są używane do wykonywania pozycjonowania zmiany w tym zmiany `ColumnSpan` i `RowSpan`. Należy pamiętać, że w przeciwieństwie do innych właściwości, które można ustawić w dowolnym momencie, właściwości ustawiane przy użyciu metod statycznych musi już istnieć w siatce przed ich zmiany.

Zakończenie XAML dla powyższych aplikacji Kalkulator wygląda następująco:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.CalculatorGridXAML"
Title = "Calculator - XAML"
BackgroundColor="#404040">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="plainButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#eee"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="darkerButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#ddd"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="orangeButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#E8AD00"/>
                <Setter Property="TextColor" Value="White" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <Grid x:Name="controlGrid" RowSpacing="1" ColumnSpacing="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="150" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Label Text="0" Grid.Row="0" HorizontalTextAlignment="End" VerticalTextAlignment="End" TextColor="White"
        FontSize="60" Grid.ColumnSpan="4" />
            <Button Text = "C" Grid.Row="1" Grid.Column="0"
        Style="{StaticResource darkerButton}" />
            <Button Text = "+/-" Grid.Row="1" Grid.Column="1"
        Style="{StaticResource darkerButton}" />
            <Button Text = "%" Grid.Row="1" Grid.Column="2"
        Style="{StaticResource darkerButton}" />
            <Button Text = "div" Grid.Row="1" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "7" Grid.Row="2" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "8" Grid.Row="2" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "9" Grid.Row="2" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "X" Grid.Row="2" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "4" Grid.Row="3" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "5" Grid.Row="3" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "6" Grid.Row="3" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "-" Grid.Row="3" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "1" Grid.Row="4" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "2" Grid.Row="4" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "3" Grid.Row="4" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "+" Grid.Row="4" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "0" Grid.ColumnSpan="2"
        Grid.Row="5" Grid.Column="0" Style="{StaticResource plainButton}" />
            <Button Text = "." Grid.Row="5" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "=" Grid.Row="5" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Zwróć uwagę, czy zarówno etykiety w górnej części siatki i zero przycisk są occuping więcej niż jedną kolumnę. Mimo że podobny układ mógłby zostać osiągnięty przy użyciu siatki zagnieżdżonych `ColumnSpan`  &  `RowSpan` podejście jest prostsze.

C# implementacji:

```csharp
public CalculatorGridCode ()
{
  Title = "Calculator - C#";
  BackgroundColor = Color.FromHex ("#404040");

  var plainButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#eee") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var darkerButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#ddd") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var orangeButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#E8AD00") },
      new Setter { Property = Button.TextColorProperty, Value = Color.White },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };

  var controlGrid = new Grid { RowSpacing = 1, ColumnSpacing = 1 };
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (150) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });

  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });

  var label = new Label {
    Text = "0",
    HorizontalTextAlignment = TextAlignment.End,
    VerticalTextAlignment = TextAlignment.End,
    TextColor = Color.White,
    FontSize = 60
  };
  controlGrid.Children.Add (label, 0, 0);

  Grid.SetColumnSpan (label, 4);

  controlGrid.Children.Add (new Button { Text = "C", Style = darkerButton }, 0, 1);
  controlGrid.Children.Add (new Button { Text = "+/-", Style = darkerButton }, 1, 1);
  controlGrid.Children.Add (new Button { Text = "%", Style = darkerButton }, 2, 1);
  controlGrid.Children.Add (new Button { Text = "div", Style = orangeButton }, 3, 1);
  controlGrid.Children.Add (new Button { Text = "7", Style = plainButton }, 0, 2);
  controlGrid.Children.Add (new Button { Text = "8", Style = plainButton }, 1, 2);
  controlGrid.Children.Add (new Button { Text = "9", Style = plainButton }, 2, 2);
  controlGrid.Children.Add (new Button { Text = "X", Style = orangeButton }, 3, 2);
  controlGrid.Children.Add (new Button { Text = "4", Style = plainButton }, 0, 3);
  controlGrid.Children.Add (new Button { Text = "5", Style = plainButton }, 1, 3);
  controlGrid.Children.Add (new Button { Text = "6", Style = plainButton }, 2, 3);
  controlGrid.Children.Add (new Button { Text = "-", Style = orangeButton }, 3, 3);
  controlGrid.Children.Add (new Button { Text = "1", Style = plainButton }, 0, 4);
  controlGrid.Children.Add (new Button { Text = "2", Style = plainButton }, 1, 4);
  controlGrid.Children.Add (new Button { Text = "3", Style = plainButton }, 2, 4);
  controlGrid.Children.Add (new Button { Text = "+", Style = orangeButton }, 3, 4);
  controlGrid.Children.Add (new Button { Text = ".", Style = plainButton }, 2, 5);
  controlGrid.Children.Add (new Button { Text = "=", Style = orangeButton }, 3, 5);

  var zeroButton = new Button { Text = "0", Style = plainButton };
  controlGrid.Children.Add (zeroButton, 0, 5);
  Grid.SetColumnSpan (zeroButton, 2);

  Content = controlGrid;
}
```


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie aplikacji mobilnych za pomocą platformy Xamarin.Forms, rozdział 17](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [Siatka](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)
- [Układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Przykład BusinessTumble (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
