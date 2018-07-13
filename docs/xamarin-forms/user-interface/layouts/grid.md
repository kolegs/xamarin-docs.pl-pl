---
title: Siatka zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak korzystać z zestawu narzędzi Xamarin.Forms siatki klasy do prezentowania widoków w siatkach, które posiadają wierszy i kolumn.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 01dd59d5e94b473316b03f9035d38305fad42880
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994505"
---
# <a name="xamarinforms-grid"></a>Siatka zestawu narzędzi Xamarin.Forms

[`Grid`](xref:Xamarin.Forms.Grid) obsługuje rozmieszczanie widoków w wiersze i kolumny. Rozmiary proporcjonalna lub bezwzględny rozmiarów można ustawić wierszy i kolumn. `Grid` Układu nie powinny być mylone z tradycyjnych tabel i nie jest przeznaczona do prezentowania danych tabelarycznych. `Grid` nie ma koncepcji wierszy, kolumny lub komórki formatowania. W przeciwieństwie do tabel HTML `Grid` czysto jest przeznaczona do układania zawartości.

[![](grid-images/layouts-sml.png "Układy platformy Xamarin.Forms")](grid-images/layouts.png#lightbox "układy platformy Xamarin.Forms")

W tym artykule omówiono:

- **[Cel](#Purpose)**  &ndash; typowe zastosowania dla `Grid`.
- **[Użycie](#Usage)**  &ndash; sposób używania `Grid` do osiągnięcia żądanego projektu.
  - **[Wiersze i kolumny](#Rows_and_Columns)**  &ndash; Określ wierszy i kolumn `Grid`.
  - **[Wprowadzenie do widoków](#Placing_Views)**  &ndash; dodawać widoki do siatki w określonych wierszy i kolumn.
  - **[Odstępy](#Spacing)**  &ndash; skonfigurować spacji między wierszami i kolumnami.
  - **[Zakresy](#Spans)**  &ndash; skonfigurować elementy, aby rozciągać się na kilka wierszy lub kolumn.

![](grid-images/grid.png "Eksploracja siatki")

## <a name="purpose"></a>Cel

`Grid` można rozmieścić widoków w siatce. Jest to przydatne w wielu przypadkach:

- Rozmieszczanie przycisków w aplikacji Kalkulator
- Rozmieszczenie przyciski/wyborów w siatce, takich jak dla systemu iOS lub Android ekranów głównych
- Rozmieszczanie widoków, aby były one taki sam rozmiar w jednym wymiarze (na przykład w niektóre paski narzędzi)

## <a name="usage"></a>Użycie

W przeciwieństwie do tradycyjnych tabel `Grid` nie są rozpoznawane przez liczbę i rozmiar wierszy i kolumn z zawartości. Zamiast tego `Grid` ma `RowDefinitions` i `ColumnDefinitions` kolekcji. Przytrzymaj te definicje liczby wierszy i kolumn zostanie wyświetlone. Widoki są dodawane do `Grid` przy użyciu określonych wiersza i kolumny indeksów, które wiersze i kolumny widoku powinny być umieszczone w zidentyfikować który.

<a name="Rows_and_Columns" />

### <a name="rows-and-columns"></a>Wiersze i kolumny

Informacje wiersza i kolumny są przechowywane w `Grid`firmy `RowDefinitions`  &  `ColumnDefinitions` właściwości, które są każdego kolekcjami z [ `RowDefinition` ](xref:Xamarin.Forms.RowDefinition) i [ `ColumnDefinition` ](xref:Xamarin.Forms.ColumnDefinition)obiektów, odpowiednio. `RowDefinition` ma tylko jedną właściwość `Height`, i `ColumnDefinition` ma tylko jedną właściwość `Width`. Opcje wysokość i szerokość są następujące:

- **Automatyczne** &ndash; automatycznie rozmiary w celu dopasowania do zawartości w wierszu lub kolumnie. Określony jako [ `GridUnitType.Auto` ](xref:Xamarin.Forms.GridUnitType) w języku C# lub jako `Auto` w XAML.
- **Proportional(*)** &ndash; rozmiaru wierszy i kolumn w stosunku do pozostałego miejsca. Określana jako wartość i `GridUnitType.Star` w języku C# i jako `#*` w XAML, za pomocą `#` trwa odpowiednią wartość. Określenie jednego wiersza/kolumny z `*` spowoduje, że jej w celu wypełnienia dostępnego miejsca.
- **Bezwzględny** &ndash; rozmiarów kolumnami i wierszami przy użyciu stałych, określone wartości szerokości i wysokości. Określana jako wartość i `GridUnitType.Absolute` w języku C# i jako `#` w XAML, za pomocą `#` trwa odpowiednią wartość.

> [!NOTE]
> Wartości szerokości kolumn są ustawione jako "*" Domyślnie w interfejsie Xamarin.Forms, co zapewnia, że kolumna wypełni dostępne miejsce.

Należy wziąć pod uwagę aplikację, która potrzebuje trzema wierszami i kolumnami. Dolny wiersz musi być dokładnie 200px wysokości i górny wiersz musi być dwa razy większa przedtem jako środkowym rzędzie. Kolumna po lewej stronie musi być dostatecznie szerokie, aby dopasować zawartość i prawa kolumna musi wypełnić pozostałe miejsce.

W XAML:

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

Aby umieścić widoków w `Grid` , musisz dodać je jako elementy podrzędne do siatki, a następnie określ, które wiersze i kolumny są członkami w.

W XAML, użyj `Grid.Row` i `Grid.Column` o poszczególnych poszczególnych widokach, do określania położenia. Należy pamiętać, że `Grid.Row` i `Grid.Column` Określ lokalizację, w oparciu o listy liczony od zera, wierszy i kolumn. Oznacza to, że w siatkę 4 x 4 lewej górnej komórki jest (0,0) i prawej dolnej komórki jest (3,3).

`Grid` Pokazane poniżej zawiera cztery komórki:

![](grid-images/label-grid.png "Siatka z czterech widoków")

W XAML:

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

Powyższy kod tworzy siatka z czterech etykiet, dwóch kolumn i dwa wiersze. Należy pamiętać, że każda etykieta będzie miał taki sam rozmiar i że wiersze rozwinie się używać całe dostępne miejsce.

W powyższym przykładzie widoki są dodawane do [ `Grid.Children` ](xref:Xamarin.Forms.Grid.Children) przy użyciu kolekcji [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/) przeciążenia, które określa lewym i górnym argumenty. Korzystając z [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) przeciążenia, które określa lewej, po prawej stronie, górnej i dolnej argumentów, podczas gdy po lewej stronie i górnym argumenty zawsze będzie odnosił się do komórek w [ `Grid` ](xref:Xamarin.Forms.Grid), po prawej stronie i argumenty dolnej może wydawać się odwoływać się do komórek, które wykraczają poza `Grid`. Jest to spowodowane prawy argument zawsze musi być większa niż argument po lewej stronie, a argument dolnej zawsze musi być większa niż top argument. W poniższym przykładzie pokazano równoważny kod przy użyciu zarówno `Add` przeciążenia:

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

`Grid` zawiera właściwości, aby kontrolować odstępy między wierszami i kolumnami.  Następujące właściwości są dostępne do dostosowywania `Grid`:

- **ColumnSpacing** &ndash; ilość miejsca między kolumnami.
- **RowSpacing** &ndash; ilość miejsca między wierszami.

Określa następujące XAML `Grid` z dwiema kolumnami, jeden wiersz i 5 pikseli odstępy między kolumnami:

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

Często, podczas pracy z siatki, istnieje element, który powinien zajmować więcej niż jeden wiersz lub kolumnę. Należy wziąć pod uwagę aplikacji prosty kalkulator:

![](grid-images/calculator.png "Calulator aplikacji")

Zwróć uwagę, przycisk 0 obejmuje dwie kolumny, podobnie jak na wbudowane kalkulatory dla każdej platformy. Jest to realizowane przy użyciu `ColumnSpan` właściwość, która określa liczbę kolumn na element powinien zajmować. XAML dla tego przycisku:

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

W języku C#:

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

Należy pamiętać, ten kod w metody statyczne `Grid` klasy są używane do wykonywania zmian pozycjonowania, łącznie ze zmianami `ColumnSpan` i `RowSpan`. Należy pamiętać, że w przeciwieństwie do innych właściwości, które można ustawić w dowolnym momencie, właściwości, które można ustawić przy użyciu metod statycznych musi już być również w siatce przed ich zmiany.

Pełne XAML dla powyższych aplikacji Kalkulator jest następująca:

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

Zauważ, że zarówno etykiety w górnej części siatki i zero przycisk są occuping więcej niż jedną kolumnę. Mimo że podobny układ można osiągnąć przy użyciu zagnieżdżonych siatki `ColumnSpan`  &  `RowSpan` podejście jest prostsze.

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

- [Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms 17 rozdziałów](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [Siatka](xref:Xamarin.Forms.Grid)
- [Układ (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Przykład BusinessTumble (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
