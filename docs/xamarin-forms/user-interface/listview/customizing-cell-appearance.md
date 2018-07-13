---
title: Dostosowywanie wyglądu komórek w ListView
description: W tym artykule przedstawiono opcje prezentacji danych w aplikacjach Xamarin.Forms przy wykorzystaniu możliwości wygody kontrolki ListView.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 7a0f55b6d8a61f52f4ef137d83c56d86149bc3c9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996259"
---
# <a name="customizing-listview-cell-appearance"></a>Dostosowywanie wyglądu komórek w ListView

ListView przedstawia przewijany list, które można dostosować za pośrednictwem `ViewCell`s. `ViewCells` może służyć do wyświetlania tekstu i obrazów, wskazujący stan PRAWDA/FAŁSZ i odbierania danych wejściowych użytkownika.

Istnieją dwa podejścia do uzyskiwania z komórki ListView wyglądu:

- **[Dostosowywanie komórek wbudowanych](#Built_in_Cells)**  &ndash; łatwiejszą implementacją i zwiększając wydajność kosztem dostosowywalności.
- **[Tworzenie niestandardowych komórek](#customcells)**  &ndash; bardziej kontrolę nad efekt, ale ma potencjalnych problemów z wydajnością, jeśli nie zaimplementowana poprawnie.

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>Wbudowane w komórkach
Zestaw narzędzi Xamarin.Forms jest dostarczany z wbudowaną komórki, które działają w przypadku wielu prostej aplikacji:

- **TextCell** &ndash; do wyświetlania tekstu
- **ImageCell** &ndash; do wyświetlania obrazu z tekstem.

Dwa dodatkowe komórki, [ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell) i [ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell) są dostępne, ale nie są one często stosowana w przypadku `ListView`. Zobacz [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) Aby uzyskać więcej informacji na temat tych komórek.

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell) jest komórka do wyświetlania tekstu, opcjonalnie wraz z drugiego wiersza jako tekst szczegółów.

TextCells są renderowane jako natywne kontrolki w czasie wykonywania, więc wydajność w porównaniu do niestandardowego jest bardzo dobra `ViewCell`. TextCells są dostosowywane, co pozwala ustawić:

- `Text` &ndash; tekst, który jest wyświetlany w pierwszym wierszu, w dużej czcionki.
- `Detail` &ndash; tekst, który jest wyświetlany poniżej pierwszy wiersz, w mniejszej czcionki.
- `TextColor` &ndash; kolor tekstu.
- `DetailColor` &ndash; kolor tekstu szczegółów

![](customizing-cell-appearance-images/text-cell-default.png "Przykład TextCell domyślne")

![](customizing-cell-appearance-images/text-cell-custom.png "Dostosowane przykład TextCell")

<a name="ImageCell" />

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell), takich jak `TextCell`, może służyć do wyświetlania tekstu i dodatkowych szczegółów tekstu i oferuje doskonałą wydajność za pomocą kontrolki natywne każdej z platform. `ImageCell` różni się od `TextCell` , wyświetla obraz po lewej stronie tekstu.

`ImageCell` jest przydatne, gdy zachodzi potrzeba wyświetlenia listy danych za pomocą zmieni się wizualny aspekt, takich jak lista kontaktów lub filmów. ImageCells są dostosowywane, co pozwala ustawić:

- `Text` &ndash; tekst, który jest wyświetlany w pierwszym wierszu, w dużej czcionki
- `Detail` &ndash; tekst, który jest wyświetlany poniżej pierwszy wiersz, w mniejszej czcionki
- `TextColor` &ndash; kolor tekstu
- `DetailColor` &ndash; kolor tekstu szczegółów
- `ImageSource` &ndash; obraz do wyświetlenia obok tekstu

![](customizing-cell-appearance-images/image-cell-default.png "Przykład ImageCell domyślne")

![](customizing-cell-appearance-images/image-cell-custom.png "Dostosowane przykład ImageCell")

<a name="customcells" />

## <a name="custom-cells"></a>Niestandardowe komórek
Gdy wbudowane komórki nie zawierają wymagane układu, niestandardowe komórek implementowane wymagane układu. Na przykład można przedstawić komórki za pomocą dwóch etykiet, które mają taki sam wagę. A `TextCell` okażą się niewystarczające ponieważ `TextCell` ma jedną etykietę, która jest mniejsza. Większość dostosowań komórki dodać dodatkowe dane tylko do odczytu (na przykład dodatkowe etykiety, obrazy i inne informacje wyświetlania).

Wszystkie komórki niestandardowe muszą pochodzić od [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), tej samej klasy bazowej, że wszystkie komórki wbudowane typy użycia.

Zestaw narzędzi Xamarin.Forms 2 wprowadzono nowe [zachowanie buforowania](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy) na `ListView` kontrolki, które można ustawić, aby poprawić wydajność w przypadku niektórych typów niestandardowych komórek.

Jest to przykład niestandardowych komórki:

![](customizing-cell-appearance-images/custom-cell.png "Przykład niestandardowych komórki")

### <a name="xaml"></a>XAML
XAML do tworzenia układu powyżej znajduje się poniżej:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="demoListView.ImageCellPage">
    <ContentPage.Content>
        <ListView  x:Name="listView">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout BackgroundColor="#eee"
                        Orientation="Vertical">
                            <StackLayout Orientation="Horizontal">
                                <Image Source="{Binding image}" />
                                <Label Text="{Binding title}"
                                TextColor="#f35e20" />
                                <Label Text="{Binding subtitle}"
                                HorizontalOptions="EndAndExpand"
                                TextColor="#503026" />
                            </StackLayout>
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Powyższe XAML znacznie robi. Możemy ją rozbić:

- Zagnieżdżona w komórce niestandardowe `DataTemplate`, który znajduje się wewnątrz `ListView.ItemTemplate`. Jest to ten sam proces, używając innych komórek.
- `ViewCell` to typ niestandardowy komórki. Elementem podrzędnym elementu `DataTemplate` element musi być lub pochodzić od typu `ViewCell`.
- Należy zauważyć, że wewnątrz `ViewCell`, układ jest zarządzana przez `StackLayout`. Ten układ pozwala nam dostosować kolor tła. Należy pamiętać, że dowolnej właściwości `StackLayout` czyli możliwej do wiązania może być powiązane w komórce niestandardowe, ale który nie został tutaj pokazany.

### <a name="cnum"></a>C&num;

Określanie niestandardowego komórki w języku C# jest nieco pełniejszą niż odpowiednik XAML. Przyjrzyjmy się:

Najpierw należy zdefiniować klasę niestandardowe komórki za pomocą `ViewCell` jako klasa bazowa:

```csharp
public class CustomCell : ViewCell
    {
        public CustomCell()
        {
            //instantiate each of our views
            var image = new Image ();
            StackLayout cellWrapper = new StackLayout ();
            StackLayout horizontalLayout = new StackLayout ();
            Label left = new Label ();
            Label right = new Label ();

            //set bindings
            left.SetBinding (Label.TextProperty, "title");
            right.SetBinding (Label.TextProperty, "subtitle");
            image.SetBinding (Image.SourceProperty, "image");

            //Set properties for desired design
            cellWrapper.BackgroundColor = Color.FromHex ("#eee");
            horizontalLayout.Orientation = StackOrientation.Horizontal;
            right.HorizontalOptions = LayoutOptions.EndAndExpand;
            left.TextColor = Color.FromHex ("#f35e20");
            right.TextColor = Color.FromHex ("503026");

            //add views to the view hierarchy
            horizontalLayout.Children.Add (image);
            horizontalLayout.Children.Add (left);
            horizontalLayout.Children.Add (right);
            cellWrapper.Children.Add (horizontalLayout);
            View = cellWrapper;
        }
    }
```

W konstruktora dla strony z `ListView`, ustaw ListView `ItemTemplate` nową właściwość `DataTemplate`:

```csharp
public partial class ImageCellPage : ContentPage
    {
        public ImageCellPage ()
        {
            InitializeComponent ();
            listView.ItemTemplate = new DataTemplate (typeof(CustomCell));
        }
    }
```

Należy pamiętać, że Konstruktor `DataTemplate` przyjmuje typu. Typeof — operator pobiera typ CLR dla `CustomCell`.

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>Zmiany kontekstu powiązania

Podczas tworzenia wiązania do typu niestandardowego komórki [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) wystąpień kontrolek interfejsu użytkownika, wyświetlając `BindableProperty` należy używać wartości [ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) należy przesłonić, aby ustawić danych, które mają być wyświetlane w Każda komórka, a nie konstruktora komórki, jak pokazano w poniższym przykładzie kodu:

```csharp
public class CustomCell : ViewCell
{
    Label nameLabel, ageLabel, locationLabel;

    public static readonly BindableProperty NameProperty =
        BindableProperty.Create ("Name", typeof(string), typeof(CustomCell), "Name");
    public static readonly BindableProperty AgeProperty =
        BindableProperty.Create ("Age", typeof(int), typeof(CustomCell), 0);
    public static readonly BindableProperty LocationProperty =
        BindableProperty.Create ("Location", typeof(string), typeof(CustomCell), "Location");

    public string Name {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null) {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

[ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) Zastąpienie zostanie wywołana kiedy [ `BindingContextChanged` ](xref:Xamarin.Forms.BindableObject.BindingContextChanged) aktywowaniu zdarzenia w odpowiedzi na wartość [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) zmiany właściwości. W związku z tym, kiedy `BindingContext` zmienia kontrolek interfejsu użytkownika, wyświetlając [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) wartości należy ustawić ich danych. Należy pamiętać, że `BindingContext` powinny być sprawdzane pod kątem `null` wartość, jak to może być ustawiona przez zestaw narzędzi Xamarin.Forms dla wyrzucania elementów bezużytecznych, co z kolei spowoduje, że `OnBindingContextChanged` zastąpienia wywoływana.

Alternatywnie można powiązać formanty interfejsu użytkownika [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) wystąpienia, aby wyświetlić ich wartości, które eliminuje konieczność, aby zastąpić `OnBindingContextChanged` metody.

> [!NOTE]
> Podczas zastępowania `OnBindingContextChanged`, upewnij się, że klasa bazowa `OnBindingContextChanged` metoda jest wywoływana, dlatego, że zarejestrowani delegaci otrzymają `BindingContextChanged` zdarzeń.

W XAML powiązanie typu niestandardowego komórki z danymi można osiągnąć jak pokazano w poniższym przykładzie kodu:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Powoduje to powiązanie `Name`, `Age`, i `Location` właściwości możliwej do wiązania w `CustomCell` wystąpienia do `Name`, `Age`, i `Location` właściwości każdego obiektu w kolekcji źródłowej.

W poniższym przykładzie kodu pokazano równoważne powiązania w języku C#:

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView {
    ItemsSource = people,
    ItemTemplate = customCell
};
```

W systemach iOS i Android Jeśli [ `ListView` ](xref:Xamarin.Forms.ListView) jest odtwarzane elementy i niestandardowe komórki używa niestandardowego modułu renderowania, niestandardowego modułu renderowania poprawnie musi implementować powiadomienie o zmianie właściwości. Gdy komórek są ponownie ich wartości właściwości ulegnie zmianie podczas aktualizowania kontekstu powiązania, dostępne komórki za pomocą `PropertyChanged` zdarzeń zgłaszanych. Aby uzyskać więcej informacji, zobacz [Dostosowywanie obiektu ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Aby uzyskać więcej informacji na temat odtwarzania komórki zobacz [strategii buforowania](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

## <a name="related-links"></a>Linki pokrewne

- [Wbudowane komórek (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Niestandardowe komórek (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Zmienić kontekstu powiązania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
