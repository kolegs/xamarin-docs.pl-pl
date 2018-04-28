---
title: Wygląd komórek
description: Zapoznaj się z opcjami prezentacji danych wykorzystując wygodę ListView.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 37ecc76d9774b3f375af92f2a00c6c687358f065
ms.sourcegitcommit: a69439ad4c9fd0abe759143687d3b23582573d90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/28/2018
---
# <a name="cell-appearance"></a>Wygląd komórek

Element ListView przedstawia przewijanej listy, które można dostosować za pośrednictwem `ViewCell`s. `ViewCells` Służy do wyświetlania tekstu i obrazów, wskazujący stan PRAWDA/FAŁSZ i odbierania danych wejściowych użytkownika.

Istnieją dwa podejścia do uzyskiwania odpowiedni z komórek ListView wygląd:

- **[Dostosowywanie komórek wbudowanych](#Built_in_Cells)**  &ndash; łatwiejszą implementacją i zwiększyć wydajność kosztem dostosowywalności.
- **[Tworzenie niestandardowych komórek](#customcells)**  &ndash; więcej kontrolę nad wynik końcowy, ale ma potencjalnych problemów z wydajnością, jeśli nie zaimplementowano poprawnie.

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>Wbudowane w komórkach
Platformy Xamarin.Forms jest dostarczany z wbudowanych komórek, które działają w wielu aplikacjach prosty:

- **TextCell** &ndash; do wyświetlania tekstu
- **ImageCell** &ndash; do wyświetlania obrazu na tekst.

Dwa dodatkowe komórki, [ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell) i [ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell) są dostępne, ale często nie są one używane przez `ListView`. Zobacz [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) uzyskać więcej informacji o tych komórek.

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) jest komórki do wyświetlania tekstu, opcjonalnie z drugiego wiersza jako tekst szczegółów.

TextCells są renderowane jako kontrolki natywne w czasie wykonywania, więc wydajność w porównaniu do niestandardowego jest bardzo dobre `ViewCell`. TextCells są można dostosowywać, co pozwala na ustawienie:

- `Text` &ndash; tekst, który jest widoczny w pierwszym wierszu w dużej czcionki.
- `Detail` &ndash; tekst, który jest wyświetlany poniżej pierwszy wiersz w mniejszej czcionki.
- `TextColor` &ndash; kolor tekstu.
- `DetailColor` &ndash; kolor tekstu szczegółów

![](customizing-cell-appearance-images/text-cell-default.png "Przykład TextCell domyślne")

![](customizing-cell-appearance-images/text-cell-custom.png "Przykład TextCell dostosowane")

<a name="ImageCell" />

### <a name="imagecell"></a>ImageCell

[`ImageCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/), takich jak `TextCell`, można używać do wyświetlania tekstu i dodatkowej szczegółów tekstu i oferuje doskonałą wydajność przy użyciu kontrolki natywne każdej platformy. `ImageCell` różni się od `TextCell` w tym Wyświetla obraz w lewo tekstu.

`ImageCell` jest przydatne, gdy konieczne jest wyświetlenie listy danych z elementem visual, takie jak listy kontaktów lub filmy. ImageCells są można dostosowywać, co pozwala na ustawienie:

- `Text` &ndash; tekst, który jest widoczny w pierwszym wierszu w dużej czcionki
- `Detail` &ndash; tekst, który jest wyświetlany poniżej pierwszy wiersz w mniejszej czcionki
- `TextColor` &ndash; kolor tekstu
- `DetailColor` &ndash; kolor tekstu szczegółów
- `ImageSource` &ndash; obraz do wyświetlenia obok tekstu

![](customizing-cell-appearance-images/image-cell-default.png "Przykład ImageCell domyślne")

![](customizing-cell-appearance-images/image-cell-custom.png "Przykład ImageCell dostosowane")

<a name="customcells" />

## <a name="custom-cells"></a>Niestandardowe komórek
Gdy wbudowane komórki nie zawierają wymagane układu, niestandardowe komórek zaimplementowana wymagane układu. Na przykład można prezentować komórki dwie etykiety, które mają ta sama waga. A `TextCell` będzie za mało ponieważ `TextCell` ma jednej etykiety, która jest mniejsza. Większość dostosowania komórki dodać dodatkowych danych tylko do odczytu (na przykład dodatkowe etykiety, obrazy i inne informacje wyświetlania).

Wszystkie niestandardowe komórki musi pochodzić od [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), tej samej klasy podstawowej, że wszystkie komórki wbudowane typy użycia.

Platformy Xamarin.Forms 2 wprowadzono nowy [zachowanie buforowania](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy) na `ListView` kontroli, którą można ustawić w celu usprawnienia przewijania w przypadku niektórych typów niestandardowych komórek.

To jest przykład komórki niestandardowych:

![](customizing-cell-appearance-images/custom-cell.png "Przykład niestandardowych komórki")

### <a name="xaml"></a>XAML
Kod XAML, aby utworzyć układ powyżej znajduje się poniżej:

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

Wiele zadań XAML powyżej. Ta funkcja pozwala podzielić go:

- Niestandardowe komórki jest zagnieżdżona `DataTemplate`, który znajduje się wewnątrz `ListView.ItemTemplate`. Jest to ten sam proces jako przy użyciu innych komórki.
- `ViewCell` jest to typ niestandardowy komórki. Podrzędne `DataTemplate` elementu musi być lub pochodzić od typu `ViewCell`.
- Zwróć uwagę, że wewnątrz `ViewCell`, układ jest zarządzana przez `StackLayout`. Ten układ pozwala nam dostosować kolor tła. Należy pamiętać, że żadnej właściwości `StackLayout` czyli powiązania mogą być powiązane w komórce niestandardowych, mimo że nie jest wyświetlany w tym miejscu.

### <a name="cnum"></a>C&num;

Określenie niestandardowych komórki w języku C# jest nieco pełniejszą niż równoważne XAML. Spójrzmy:

Najpierw należy zdefiniować klasę niestandardowych komórki z `ViewCell` jako klasy podstawowej:

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

W Konstruktorze użytkownika dla strony z `ListView`, Ustaw element ListView `ItemTemplate` właściwości na nowy `DataTemplate`:

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

Należy pamiętać, że Konstruktor `DataTemplate` przyjmuje typu. Typeof operator pobiera typ CLR dla `CustomCell`.

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>Powiązanie zmiany kontekstu

Gdy powiązanie z typem niestandardowych komórki [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wystąpienia kontrolek interfejsu użytkownika, wyświetlanie `BindableProperty` należy użyć wartości [ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) należy przesłonić, aby ustawić danych mają być wyświetlane w każdej komórki, a nie konstruktora komórki, jak pokazano w poniższym przykładzie:

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

[ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) Zastąpienie zostanie wywołana podczas [ `BindingContextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.BindingContextChanged/) generowane zdarzenie, w odpowiedzi na wartość [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) zmiany właściwości. W związku z tym, kiedy `BindingContext` zmienia kontrolek interfejsu użytkownika, wyświetlanie [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wartości należy ustawić ich danych. Należy pamiętać, że `BindingContext` powinny być sprawdzane pod kątem `null` wartość, jak można ją skonfigurować przez platformy Xamarin.Forms dla wyrzucanie elementów bezużytecznych, co z kolei spowoduje `OnBindingContextChanged` zastąpienia wywoływane.

Alternatywnie można powiązać z kontrolek interfejsu użytkownika [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wystąpienia, aby wyświetlić ich wartości, które wymaga, aby zastąpić `OnBindingContextChanged` metody.

> [!NOTE]
> W przypadku przesłaniania `OnBindingContextChanged`, upewnij się, że klasa podstawowa `OnBindingContextChanged` metoda jest wywoływana, aby odbierać zarejestrowanych delegatów `BindingContextChanged` zdarzeń.

W języku XAML typ komórki niestandardowego powiązania z danymi można osiągnąć jak pokazano w poniższym przykładzie:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Powoduje to powiązanie `Name`, `Age`, i `Location` właściwości w `CustomCell` wystąpienie do `Name`, `Age`, i `Location` właściwości każdego obiektu w kolekcji źródłowej.

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

W systemach iOS i Android Jeśli [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) jest odtwarzania elementów i niestandardowych komórki używa niestandardowego modułu renderowania, niestandardowego modułu renderowania należy poprawnie zaimplementować powiadomienia o zmianie właściwości. Po komórki są ponownie ich wartości właściwości zostaną zmienione po zaktualizowaniu niż dostępna komórki, kontekst powiązania z `PropertyChanged` zdarzeń zgłaszanych. Aby uzyskać więcej informacji, zobacz [Dostosowywanie ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Aby uzyskać więcej informacji na temat odtwarzania komórki, zobacz [strategii buforowania](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

## <a name="related-links"></a>Linki pokrewne

- [Wbudowane w komórkach (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Niestandardowe komórek (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Zmienić kontekstu powiązania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
