---
title: "Wygląd listy"
description: "Dostosowywanie przy użyciu nagłówków, stopek grup i komórek o zmiennej wysokości w widokach listy."
ms.topic: article
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 4e849cc034b8c77b84d1be8cb31cc1206f203ef6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="list-appearance"></a>Wygląd listy

`ListView` zawiera opcje umożliwiające sterowanie prezentacji listy ogólnej, oprócz podstawowych `ViewCell`s. Dostępne są następujące opcje:

- [**Grupowanie** ](#Grouping) &ndash; grupować elementy w elemencie ListView ułatwiające nawigację i ulepszone organizacji.
- [**Nagłówki i stopki** ](#Headers_and_Footers) &ndash; zawierają informacje na początku i na końcu widoku, który jest przewijane wraz z innych elementów.
- [**Wiersz separatorów** ](#Row_Separators) &ndash; wyświetlanie lub ukrywanie linii separatora między elementami.
- [**Zmienna wysokość wierszy** ](#Row_Heights) &ndash; domyślnie wszystkie wiersze są tego samego wysokość, ale można go zmienić umożliwia wiersze z różnych wysokości mają być wyświetlane.

<a name="Grouping" />

## <a name="grouping"></a>Grupowanie
Często dużych zestawów danych może być trudno obsługiwać, gdy przedstawione w sposób ciągły przewijanej listy. Włączanie grupowania może poprawić środowisko użytkownika w tych przypadkach lepszego organizowania zawartości i aktywowanie formantów specyficzne dla platformy, które ułatwić nawigacyjnego danych.

Po aktywowaniu grupowania dla `ListView`, dodawany jest wiersz nagłówka dla każdej grupy.

Aby włączyć grupowanie:

- Utwórz listę list (Lista grup, każda grupa jest lista elementów).
- Ustaw `ListView`w `ItemsSource` do tej listy.
- Ustaw `IsGroupingEnabled` na wartość true.
- Ustaw [ `GroupDisplayBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) można powiązać z właściwością grupy, który jest używany jako tytuł grupy.
- [Opcjonalnie] Ustaw [ `GroupShortNameBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) można powiązać z właściwością grupy, który jest używany jako krótką nazwę grupy. Krótka nazwa jest używana dla listy szybkiego dostępu (kolumna rigt serwerowe w systemie iOS, siatki kafelka w Windows Phone).

Rozpocznij od utworzenia klasy dla grup:

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

W powyższym kodzie `All` jest na liście, które będzie podane do naszej ListView jako źródło powiązania. `Title` i `ShortName` właściwości, które będą używane dla nagłówków.

Na tym etapie `All` jest wyświetlana pusta lista. Dodaj Konstruktor statyczny, dzięki czemu listy zostaną wypełnione na uruchamianie programu:

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

W powyższym kodzie firma Microsoft może także wywołać `Add` elementów `groups`, które są wystąpienia typu `PageTypeGroup`. Jest to możliwe, ponieważ `PageTypeGroup` dziedziczy `List<PageModel>`. To jest przykład listy list wzorzec opisanymi powyżej.

Oto XAML, aby wyświetlić listę pogrupowanych:

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

Należy pamiętać, że mamy:

- Ustaw `GroupShortNameBinding` do `ShortName` właściwość zdefiniowana w klasie naszej grupy
- Ustaw `GroupDisplayBinding` do `Title` właściwość zdefiniowana w klasie naszej grupy
- Ustaw `IsGroupingEnabled` na wartość true
- Zmienione `ListView`w `ItemsSource` do listy grupowanych

### <a name="customizing-grouping"></a>Dostosowywanie grupowania
Teraz, gdy firma Microsoft przedstawiono sposób wykonania podstawowych grupowania w elemencie ListView, zobaczmy, jak dostosować wyświetlanie nagłówków grup.

Podobnie jak `ListView` ma `ItemTemplate` określających sposób wyświetlania wierszy `ListView` ma `GroupHeaderTemplate`. Jest to przykład ListView z góry za pomocą szablonu nagłówka grupy niestandardowe:

![](customizing-list-appearance-images/grouping-depth.png "Element ListView z dostosowanych GroupHeaderTemplate")


Poniżej przedstawiono sposób wykonania tego projektu w języku XAML:

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
            <!-- End Group Header Customization
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

<a name="Headers_and_Footers" />

## <a name="headers-and-footers"></a>Nagłówki i stopki
Istnieje możliwość ListView do prezentowania nagłówku i stopce, które przewiń elementami listy. Nagłówku i stopce może być ciągi tekstu lub bardziej skomplikowane układu. Należy pamiętać, że to jest oddzielony od [sekcji grup](#Grouping).

Można ustawić `Header` i/lub `Footer` prosty ciąg lub można ustawić je do bardziej złożonych układu.
Dostępne są także `HeaderTemplate` i `FooterTemplate` właściwości, które umożliwiają tworzenie bardziej złożonych układów nagłówku i stopce tego powiązania danych pomocy technicznej.

Aby utworzyć prosty nagłówek/stopka, ustaw wartość właściwości w nagłówku lub stopce strony na tekst, który chcesz wyświetlić. W kodzie:

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

W języku XAML:

```xaml
<ListView  x:Name="HeaderList"  Header="Header" Footer="Footer"></ListView>
```

![](customizing-list-appearance-images/header-default.png "Element ListView o nagłówku i stopce")

Aby utworzyć dostosowany nagłówku i stopce, zdefiniuj widoków w nagłówku i stopce:

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

![](customizing-list-appearance-images/header-custom.png "Element ListView z dostosowanych nagłówków i stopek")

<a name="Row_Separators" />

## <a name="row-separators"></a>Separatory wierszy
Są wyświetlane linie separatora między `ListView` elementy domyślnie w systemach iOS i Android. Windows Phone nie obsługuje separator wierszy, na tym platform wskazówki dotyczące interfejsu użytkownika. Jeśli chcesz ukryć separator wierszy w systemach iOS i Android, ustaw `SeparatorVisibility` właściwość z elementu ListView. Opcje `SeparatorVisibility` są:

* **Domyślna** — przedstawia linii separatora w systemach iOS i Android.
* **Brak** -ukrywa separatora na wszystkich platformach.

Domyślną widoczność:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "Element ListView z separatorami domyślnego wiersza")

Brak:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "Element ListView bez separatory wierszy")

Można również ustawić kolor linii separatora za pośrednictwem `SeparatorColor` właściwości:

C#:

```csharp
SepratorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "Element ListView z separatorami zielony wiersza")

> [!NOTE]
> Ustawienie tych właściwości w systemie Android po załadowaniu `ListView` wiąże się zmniejszenie wydajności dużych.

<a name="Row_Heights" />

## <a name="row-heights"></a>Wysokość wierszy
Domyślnie wszystkie wiersze w elemencie ListView mają taką samą wysokość. Element ListView ma dwie właściwości, których można użyć, aby zmienić to zachowanie:

- `HasUnevenRows` &ndash; `true`/`false` wartość, wiersze mają różne wysokości Jeśli ustawiono `true`. Domyślnie `false`.
- `RowHeight` &ndash; Ustawia wysokość każdego wiersza, kiedy `HasUnevenRows` jest `false`.

Wysokość wszystkich wierszy można ustawić przez ustawienie `RowHeight` właściwość `ListView`.

### <a name="custom-fixed-row-height"></a>Niestandardowe stałą wysokość wiersza

C#:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "Element ListView o stałej wysokości wiersza")


### <a name="uneven-rows"></a>Nierówna wierszy

Jeśli chcesz poszczególnych wierszy mają różną wysokość, możesz ustawić `HasUnevenRows` właściwości `true`.
Należy zauważyć, że wysokości wierszy nie należy ręcznie ustawić raz `HasUnevenRows` została ustawiona jako `true`, ponieważ wysokości są automatycznie obliczane platformy Xamarin.Forms.


C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "Element ListView z nierówna wierszy")

### <a name="runtime-resizing-of-rows"></a>Środowisko uruchomieniowe zmiana rozmiaru wierszy

Poszczególne `ListView` wierszy można programowo zmienić rozmiar w czasie wykonywania, pod warunkiem, że `HasUnevenRows` właściwość jest ustawiona na `true`. [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) Metody aktualizacji rozmiar komórki, nawet wtedy, gdy nie jest widoczne, jak pokazano w poniższym przykładzie:

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

`OnImageTapped` Program obsługi zdarzeń jest wykonywane w odpowiedzi na [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) w komórce trwa dotknięciu i zwiększa rozmiar `Image` wyświetlany w komórce, dzięki czemu będzie mógł wyświetlić.

![](customizing-list-appearance-images/dynamic-row-resizing.png "Element ListView z zmiana rozmiaru wierszy środowiska wykonawczego")

Należy pamiętać, że strong możliwości obniżenia wydajności w przypadku nadmiernego obciążenia jest tej funkcji.



## <a name="related-links"></a>Linki pokrewne

- [Grupowanie (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Widok niestandardowy moduł renderowania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Dynamiczna zmiana rozmiaru wierszy (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [informacje o wersji 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [informacje o wersji 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
