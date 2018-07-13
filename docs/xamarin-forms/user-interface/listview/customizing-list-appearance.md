---
title: Dostosowywanie wyglądu ListView
description: W tym artykule wyjaśniono, jak dostosować ListViews w aplikacjach Xamarin.Forms przy użyciu nagłówki, stopki, grup i komórek o zmiennej wysokości.
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 1326a1326b4a88459e4e0a01ef590e770e3a88c0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997351"
---
# <a name="customizing-listview-appearance"></a>Dostosowywanie wyglądu ListView

`ListView` zawiera opcje sterujące prezentacji listy ogólnej, oprócz podstawowych `ViewCell`s. Opcje obejmują:

- [**Grupowanie** ](#Grouping) &ndash; grupować elementy w ListView dla zapewnienia łatwiejszej nawigacji i ulepszone organizacji.
- [**Nagłówki i stopki** ](#Headers_and_Footers) &ndash; wyświetlania informacji na początku i końcu widoku, który przewija z innymi elementami.
- [**Wiersz separatory** ](#Row_Separators) &ndash; Pokaż lub Ukryj linie separator między elementami.
- [**Zmienna wysokość wierszy** ](#Row_Heights) &ndash; domyślnie wszystkie wiersze są sama wysokość, ale można to zmienić umożliwia wiersze z różnymi wysokości, które mają być wyświetlane.

<a name="Grouping" />

## <a name="grouping"></a>Grupowanie
Często dużych zestawów danych może stać się niewydolna umieszczeniem w sposób ciągły przewijanej listy. Włączania grupowania zwiększenie komfortu użytkowników w takich przypadkach lepiej organizowania zawartości i aktywowanie formantów specyficzne dla platformy, które ułatwić po danych.

Po aktywowaniu grupowanie dla `ListView`, wiersz nagłówka jest dodawany dla każdej grupy.

Aby włączyć grupowania:

- Utwórz listę list (Lista grup, każda grupa jest lista elementów).
- Ustaw `ListView`firmy `ItemsSource` do tej listy.
- Ustaw `IsGroupingEnabled` na wartość true.
- Ustaw [ `GroupDisplayBinding` ](xref:Xamarin.Forms.ListView.GroupDisplayBinding) można powiązać właściwości grupy, który jest używany jako tytuł grupy.
- [Opcjonalnie] Ustaw [ `GroupShortNameBinding` ](xref:Xamarin.Forms.ListView.GroupShortNameBinding) można powiązać właściwości grupy, który jest używany jako krótka nazwa grupy. Krótka nazwa jest używana dla listy szybkiego dostępu (po prawej stronie kolumny w systemie iOS).

Należy rozpocząć od tworzenia klasy dla grup:

```csharp
public class PageTypeGroup : List<PageModel>
    {
        public string Title { get; set; }
        public string ShortName { get; set; } //will be used for jump lists
        public string Subtitle { get; set; }
        private PageTypeGroup(string title, string shortName)
        {
            Title = title;
            ShortName = shortName;
        }

        public static IList<PageTypeGroup> All { private set; get; }
    }
```

W powyższym kodzie `All` listę, do której zostanie przydzielony do naszych ListView jako źródło wiążące. `Title` i `ShortName` są właściwościami, które będą używane dla nagłówków.

Na tym etapie `All` jest pusta lista. Dodaj Konstruktor statyczny, tak aby lista zostanie wypełniona przy uruchamianiu programu:

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alfa", "A"){
                new PageModel("Amelia", "Cedar", new switchCellPage(),""),
                new PageModel("Alfie", "Spruce", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ava", "Pine", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Archie", "Maple", new switchCellPage(), "grapefruit.jpg")
            },
            new PageTypeGroup ("Bravo", "B"){
                new PageModel("Brooke", "Lumia", new switchCellPage(),""),
                new PageModel("Bobby", "Xperia", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Bella", "Desire", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ben", "Chocolate", new switchCellPage(), "grapefruit.jpg")
            }
        }
        All = Groups; //set the publicly accessible list
}
```

W powyższym kodzie możemy również wywołać `Add` elementów `groups`, które są wystąpieniami typu `PageTypeGroup`. Jest to możliwe, ponieważ `PageTypeGroup` dziedziczy `List<PageModel>`. Jest to przykład listę list wzorzec przedstawionych powyżej.

Oto XAML do wyświetlania pogrupowaną listę:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage"
    <ContentPage.Content>
        <ListView  x:Name="GroupedView"
        GroupDisplayBinding="{Binding Title}"
        GroupShortNameBinding="{Binding ShortName}"
        IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                     Detail="{Binding Subtitle}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Wyniki będą następujące:

![](customizing-list-appearance-images/grouping-depth.png "Przykład grupowania ListView")

Pamiętaj, że mamy:

- Ustaw `GroupShortNameBinding` do `ShortName` właściwości zdefiniowanej w klasie naszej grupy
- Ustaw `GroupDisplayBinding` do `Title` właściwości zdefiniowanej w klasie naszej grupy
- Ustaw `IsGroupingEnabled` na wartość true
- Zmienione `ListView`firmy `ItemsSource` na pogrupowaną listę

### <a name="customizing-grouping"></a>Dostosowywanie grupowania

Jeśli grupa została włączona na liście, można także dostosowywać nagłówek grupy.

Podobnie jak `ListView` ma `ItemTemplate` do definiowania sposobu wyświetlania wierszy `ListView` ma `GroupHeaderTemplate`.

Przykład dostosowania nagłówek grupy w XAML jest następujący:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage">
    <ContentPage.Content>
        <ListView x:Name="GroupedView"
         GroupDisplayBinding="{Binding Title}"
         GroupShortNameBinding="{Binding ShortName}"
         IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding Subtitle}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.ItemTemplate>
            <!-- Group Header Customization-->
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding ShortName}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            <!-- End Group Header Customization -->
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

<a name="Headers_and_Footers" />

## <a name="headers-and-footers"></a>Nagłówki i stopki
Istnieje możliwość ListView przedstawić w nagłówku i stopce, które można przewijać z elementami listy. Nagłówek i stopka może być ciągów tekstu lub bardziej skomplikowanych układ. Należy pamiętać, że to różni się od [sekcji grup](#Grouping).

Możesz ustawić `Header` i/lub `Footer` prosty ciąg wartości lub można je ustawić do bardziej złożonych układu.
Dostępne są także `HeaderTemplate` i `FooterTemplate` właściwości, które umożliwiają tworzenie bardziej złożonych układów dla nagłówku i stopce to powiązanie danych pomocy technicznej.

Aby utworzyć proste nagłówek/stopkę, ustaw wartość właściwości nagłówka lub stopki na tekst, który chcesz wyświetlić. W kodzie:

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

W XAML:

```xaml
<ListView  x:Name="HeaderList"  Header="Header" Footer="Footer"></ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView przy użyciu nagłówek i stopka")

Aby utworzyć dostosowany nagłówek i stopka, należy określić widoki nagłówku i stopce:

```xaml
<ListView.Header>
    <StackLayout Orientation="Horizontal">
        <Label Text="Header"
        TextColor="Olive"
        BackgroundColor="Red" />
    </StackLayout>
</ListView.Header>
<ListView.Footer>
    <StackLayout Orientation="Horizontal">
        <Label Text="Footer"
        TextColor="Gray"
        BackgroundColor="Blue" />
    </StackLayout>
</ListView.Footer>
```

![](customizing-list-appearance-images/header-custom.png "ListView przy użyciu dostosowanych nagłówek i stopka")

<a name="Row_Separators" />

## <a name="row-separators"></a>Separatory wierszy
Separator wierszy są wyświetlane między `ListView` elementy domyślnie w systemach iOS i Android. Jeśli chcesz użyć ukryć linii separatora w systemach iOS i Android, ustaw `SeparatorVisibility` właściwość swoje ListView. Opcje `SeparatorVisibility` są:

* **Domyślne** — pokazuje linii separatora w systemach iOS i Android.
* **Brak** -ukrywa separator na wszystkich platformach.

Widoczność domyślne:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "ListView z separatorami domyślny wiersz")

Brak:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView bez separatory wierszy")

Można również ustawić kolor linii separatora za pośrednictwem `SeparatorColor` właściwości:

C#:

```csharp
SepratorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView przy użyciu separatory wierszy zielony")

> [!NOTE]
> Ustawienie tych właściwości w systemie Android po załadowaniu `ListView` powoduje spadek wydajności.

<a name="Row_Heights" />

## <a name="row-heights"></a>Wysokość wierszy
Domyślnie wszystkie wiersze w ListView mają taką samą wysokość. ListView ma dwie właściwości, których można użyć, aby zmienić to zachowanie:

- `HasUnevenRows` &ndash; `true`/`false` wartość, wiersze mają różnej wysokości, jeśli ustawiono `true`. Wartość domyślna to `false`.
- `RowHeight` &ndash; Ustawia wysokość każdego wiersza, kiedy `HasUnevenRows` jest `false`.

Można ustawić wysokości wszystkich wierszy, ustawiając `RowHeight` właściwość `ListView`.

### <a name="custom-fixed-row-height"></a>Niestandardowe stałą wysokość wiersza

C#:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView przy użyciu stałą wysokość wiersza")


### <a name="uneven-rows"></a>Nierównomierny wierszy

Jeśli chcesz poszczególne wiersze mają różnej wysokości, możesz ustawić `HasUnevenRows` właściwość `true`.
Należy zauważyć, że wysokość wierszy nie należy ręcznie ustawić raz `HasUnevenRows` został ustawiony na `true`, ponieważ wysokości są automatycznie obliczane zestawu narzędzi Xamarin.Forms.


C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView nierównomierny wierszy")

### <a name="runtime-resizing-of-rows"></a>Środowisko uruchomieniowe zmiana rozmiaru wierszy

Poszczególne `ListView` wierszy można programowo zmienić rozmiar w czasie wykonywania, pod warunkiem, że `HasUnevenRows` właściwość jest ustawiona na `true`. [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) Metoda aktualizuje rozmiar komórki, nawet wtedy, gdy nie są obecnie widoczne, jak pokazano w poniższym przykładzie kodu:

```csharp
void OnImageTapped (object sender, EventArgs args)
{
    var image = sender as Image;
    var viewCell = image.Parent.Parent as ViewCell;

    if (image.HeightRequest < 250) {
        image.HeightRequest = image.Height + 100;
        viewCell.ForceUpdateSize ();
    }
}
```

`OnImageTapped` Programu obsługi zdarzeń jest wykonywane w odpowiedzi na [ `Image` ](xref:Xamarin.Forms.Image) w komórce jest nacisnął i zwiększa rozmiar `Image` wyświetlana w komórce, dzięki czemu będzie mógł wyświetlić.

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView przy użyciu rozmiaru wiersza środowiska uruchomieniowego")

Należy pamiętać, że istnieje duże prawdopodobieństwo spadek wydajności, jeśli ta funkcja jest nadmiernie obciążany.



## <a name="related-links"></a>Linki pokrewne

- [Grupowanie (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Widok niestandardowy moduł renderowania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Dynamiczna zmiana rozmiaru wierszy (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [informacje o wersji 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [informacje o wersji 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
