---
title: FlexLayout platformy Xamarin.Forms
description: Użyj FlexLayout układania lub zawijania Kolekcja widoków podrzędnych.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 7585138cd6c33c2a5dc537ba28101a84e1c4b7ae
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="the-xamarinforms-flexlayout"></a>FlexLayout platformy Xamarin.Forms

_Użyj FlexLayout układania lub zawijania Kolekcja widoków podrzędnych._

Platformy Xamarin.Forms [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout) stanowi nowość w wersji 3.0 lub nowszej platformy Xamarin.Forms. Opiera się na CSS [elastyczne modułu układu pole](http://www.w3.org/TR/css-flexbox-1/), powszechnie znane jako _flex układu_ lub _pola elastycznego_, więc wywołuje się, ponieważ zawiera on wiele opcji elastyczne rozmieszczenia elementów podrzędnych w układzie.

`FlexLayout` jest podobny do platformy Xamarin.Forms [ `StackLayout` ](~/xamarin-forms/user-interface/layouts/stack-layout.md) w tym go rozmieścić elementy podrzędne w poziomie i w pionie na stosie. Jednak `FlexLayout` jest również możliwość zawijania jego elementów podrzędnych, jeśli jest zbyt duża, aby zmieścić ją w pojedynczym wierszu lub kolumnie, a także ma wiele opcji orientacja wyrównania i dostosowania do różnych rozmiarów ekranu.

`FlexLayout` pochodną [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) i dziedziczy [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) właściwości typu `IList<View>`.

`FlexLayout` Definiuje właściwości publicznej sześciu i pięć dołączone właściwości powiązania, które mają wpływ na rozmiar, orientację i wyrównanie jego elementów podrzędnych. (Jeśli nie masz doświadczenia w obsłudze dołączone właściwości możliwej do wiązania, zapoznaj się z artykułem  **[dołączone właściwości](~/xamarin-forms/xaml/attached-properties.md)**.) Te właściwości są szczegółowo opisane w sekcjach poniżej na **[właściwości szczegółowo](#bindable-properties)** i  **[dołączone właściwości powiązania szczegółowo](#attached-properties)**. Jednak w tym artykule rozpoczyna się z sekcją w przypadku niektórych **[typowe scenariusze użycia](#common-scenarios)** z `FlexLayout` opisujący wiele z tych właściwości więcej nieformalnego. Na końcu tego artykułu, zobaczysz jak połączyć `FlexLayout` z [arkusze stylów CSS](~/xamarin-forms/user-interface/styles/css/index.md).

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>Typowe scenariusze użycia

**[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** przykładowy program zawiera kilka stron demonstate, że niektóre typowe zastosowania `FlexLayout` i umożliwia eksperymentów z jego właściwości.

### <a name="using-flexlayout-for-a-simple-stack"></a>Przy użyciu FlexLayout proste stosu

**Stosu proste** strony pokazuje, jak `FlexLayout` można podstawić `StackLayout` , ale z prostszych znaczników. Wszystkie elementy w tym przykładzie jest zdefiniowana w strony XAML. `FlexLayout` Zawiera cztery elementy podrzędne:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.SimpleStackPage"
             Title="Simple Stack">

    <FlexLayout Direction="Column"
                AlignItems="Center"
                JustifyContent="SpaceEvenly">

        <Label Text="FlexLayout in Action"
               FontSize="Large" />

        <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />

        <Button Text="Do-Nothing Button" />

        <Label Text="Another Label" />
    </FlexLayout>
</ContentPage>
```

Oto tej strony z systemem iOS, Android i platformy uniwersalnej systemu Windows:

[![Prosty stos strony](flex-layout-images/SimpleStack.png "uproszczonych stos strony")](flex-layout-images/SimpleStack-Large.png#lightbox)

Trzy właściwości `FlexLayout` są wyświetlane w **SimpleStackPage.xaml** pliku:

- [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) Właściwość jest ustawiona na wartość [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection) wyliczenia. Wartość domyślna to `Row`. Ustawienie właściwości `Column` powoduje, że elementy podrzędne `FlexLayout` być rozmieszczone w jednej kolumnie elementów.

    Gdy elementy w `FlexLayout` ułożone w kolumnie `FlexLayout` ma pionowym _osi głównej_ i poziomym _między osi_.

- [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) Właściwość jest typu [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems) i określa sposób wyrównania elementów na osi poprzecznej. `Center` Opcja powoduje, że każdy element, aby wyśrodkowany w poziomie.

    W przypadku używania `StackLayout` zamiast `FlexLayout` dla tego zadania, czy Centrum wszystkie elementy, przypisując `HorizontalOptions` właściwości każdy element, aby `Center`. `HorizontalOptions` Właściwość nie ma wpływu na elementy podrzędne `FlexLayout`, ale pojedynczą `AlignItems` właściwości następuje to w ramach tego samego celu. Jeśli potrzebujesz, możesz użyć `AlignSelf` dołączonych właściwości możliwej do wiązania, aby zastąpić `AlignItems` właściwości dla poszczególnych elementów:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    Z tą zmianą, ta `Label` znajduje się w lewej krawędzi `FlexLayout` przypadku kolejność czytania od lewej do prawej.

- [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) Właściwość jest typu [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify)i określa sposób rozmieszczenia elementów na osi głównej. `SpaceEvenly` Opcja przydziela wszystkie pozostałe miejsce pionowy równomiernie w poszczególnych elementów i powyżej pierwszy element i poniżej ostatniego elementu.

    W przypadku używania `StackLayout`, trzeba przypisać `VerticalOptions` właściwości każdy element, aby `CenterAndExpand` osiągnąć ten sam efekt. Ale `CenterAndExpand` opcja będzie zajmować dwa razy więcej miejsca na dysku między każdym z elementów niż przed pierwszym elementem i po ostatnim elemencie. Można naśladować `CenterAndExpand` opcji `VerticalOptions` przez ustawienie `JustifyContent` właściwość `FlexLayout` do `SpaceAround`.

Te `FlexLayout` właściwości omówiono bardziej szczegółowo w sekcji **[właściwości szczegółowo](#bindable-properties)** poniżej.

### <a name="using-flexlayout-for-wrapping-items"></a>Przy użyciu FlexLayout zawijania elementów

**Zawijania fotografii** strony **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** w przykładzie pokazano, jak `FlexLayout` może zawijać się z jego elementów podrzędnych do dodatkowych wierszy lub kolumn. Tworzy plik XAML `FlexLayout` i przypisuje dwóch właściwości:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.PhotoWrappingPage"
             Title="Photo Wrapping">
    <Grid>
        <ScrollView>
            <FlexLayout x:Name="flexLayout"
                        Wrap="Wrap"
                        JustifyContent="SpaceAround" />
        </ScrollView>

        <ActivityIndicator x:Name="activityIndicator"
                           IsRunning="True"
                           VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

`Direction` Właściwości tego `FlexLayout` nie jest ustawiona, więc domyślne ustawienie `Row`, co oznacza, że elementy podrzędne są wyświetlane w wierszach i głównych osi jest poziomy.

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) Właściwość jest typu wyliczenia [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap). Jeśli istnieje zbyt wiele elementów, aby zmieścić w wierszu, to ustawienie właściwości powoduje elementy, które mają być zawijany do następnego wiersza.

Zwróć uwagę, że `FlexLayout` jest elementem podrzędnym `ScrollView`. Jeśli istnieje za dużo wierszy mieści się na stronie, a następnie `ScrollView` ma wartość domyślną `Orientation` właściwość `Vertical` i umożliwia przewijanie w pionie.

`JustifyContent` Właściwość przydziela pozostałe miejsce na osi głównej (osi poziomej), aby każdy element jest ujęta w tej samej wielkość odstępu.

Plik CodeBehind uzyskuje dostęp do kolekcji zdjęć próbki i dodaje je do `Children` Kolekcja `FlexLayout`:

```csharp
public partial class PhotoWrappingPage : ContentPage
{
    // Class for deserializing JSON list of sample bitmaps
    [DataContract]
    class ImageList
    {
        [DataMember(Name = "photos")]
        public List<string> Photos = null;
    }

    public PhotoWrappingPage ()
    {
        InitializeComponent ();

        LoadBitmapCollection();
    }

    async void LoadBitmapCollection()
    {
        int imageDimension = Device.RuntimePlatform == Device.iOS ||
                             Device.RuntimePlatform == Device.Android ? 240 : 120;

        string urlSuffix = String.Format("?width={0}&height={0}&mode=max", imageDimension);

        using (WebClient webClient = new WebClient())
        {
            try
            {
                // Download the list of stock photos
                Uri uri = new Uri("http://docs.xamarin.com/demo/stock.json");
                byte[] data = await webClient.DownloadDataTaskAsync(uri);

                // Convert to a Stream object
                using (Stream stream = new MemoryStream(data))
                {
                    // Deserialize the JSON into an ImageList object
                    var jsonSerializer = new DataContractJsonSerializer(typeof(ImageList));
                    ImageList imageList = (ImageList)jsonSerializer.ReadObject(stream);

                    // Create an Image object for each bitmap
                    foreach (string filepath in imageList.Photos)
                    {
                        Image image = new Image
                        {
                            Source = ImageSource.FromUri(new Uri(filepath + urlSuffix))
                        };
                        flexLayout.Children.Add(image);
                    }
                }
            }
            catch
            {
                flexLayout.Children.Add(new Label
                {
                    Text = "Cannot access list of bitmap files"
                });
            }
        }

        activityIndicator.IsRunning = false;
        activityIndicator.IsVisible = false;
    }
}
```

Oto program działająca na platformach trzy, stopniowo przewijane od góry do dołu:

[![Strona zawijania fotografii](flex-layout-images/PhotoWrapping.png "strony zawijania zdjęcia")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>Układ strony z FlexLayout

Brak układu standardowego w projekcie sieci web o nazwie [ _grail Stolicą_ ](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) ponieważ jest on formacie układu, który jest pożądana, ale często trudne do sprawę z bezą. Układ składa się z nagłówka u góry strony i stopki u dołu, zarówno rozszerzanie pełnej szerokości strony. Zajmujące Centrum strony jest głównym zawartości, ale często z kolumnowy menu z lewej strony zawartości i dodatkowe informacje (nazywane również _Odłóż_ obszaru) po prawej stronie. [Sekcja 5.4.1 specyfikacji CSS elastyczny Układ pola](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) w tym artykule opisano, jak można uzyskać układu grail Stolicą z pola elastycznego.

**Grail Stolicą układu** strony **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** pokazano prosty implementacji tego układu przy użyciu jednej `FlexLayout` zagnieżdżone w innym. Obszary po lewej i prawej obszaru zawartości, ponieważ ta strona jest przeznaczona dla telefonu w trybie portret, są tylko 50 pikseli szerokości:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.HolyGrailLayoutPage"
             Title="Holy Grail Layout">

    <FlexLayout Direction="Column">

        <!-- Header -->
        <Label Text="HEADER"
               FontSize="Large"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center" />

        <!-- Body -->
        <FlexLayout FlexLayout.Grow="1">

            <!-- Content -->
            <Label Text="CONTENT"
                   FontSize="Large"
                   BackgroundColor="Gray"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center"
                   FlexLayout.Grow="1" />

            <!-- Navigation items-->
            <BoxView FlexLayout.Basis="50"
                     FlexLayout.Order="-1"
                     Color="Blue" />

            <!-- Aside items -->
            <BoxView FlexLayout.Basis="50"
                     Color="Green" />

        </FlexLayout>

        <!-- Footer -->
        <Label Text="FOOTER"
               FontSize="Large"
               BackgroundColor="Pink"
               HorizontalTextAlignment="Center" />
    </FlexLayout>
</ContentPage>
```

W tym miejscu jest uruchomiona na trzy platformach:

[![Strona układu Stolicą Grail](flex-layout-images/HolyGrailLayout.png "Grail Stolicą strony układu")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

Obszary nawigacji i zarezerwuj są renderowane z `BoxView` po lewej i prawej stronie.

Pierwszy `FlexLayout` w kodzie XAML plik ma osi pionowej głównego i zawiera trzy elementy podrzędne ułożone w kolumnie. Są to nagłówek, w treści strony i stopki. Zagnieżdżone `FlexLayout` ma osi poziomej głównego z trzema elementami podrzędnymi rozmieszczone w wierszu.

Trzy dołączonych właściwości przedstawiono w tym programie:

- `Order` Dołączona właściwość powiązania jest ustawiony na pierwszym `BoxView`. Ta właściwość jest liczbą całkowitą o wartości domyślnej 0. Ta właściwość służy do zmiany kolejności układu. Zwykle deweloperzy preferowane zawartości strony w znaczniku przed elementy nawigacji i Odłóż elementów. Ustawienie `Order` właściwości pierwszego `BoxView` wartość mniejsza niż jego elementów równorzędnych powoduje on wyświetlany jako pierwszy element w wierszu. Podobnie, sprawdź, czy element jest wyświetlany ostatni przez ustawienie `Order` właściwości do wartości większej niż jego elementów równorzędnych.

- `Basis` Dołączona właściwość powiązania jest ustawiona na dwa `BoxView` elementów nadanie im szerokość 50 pikseli. Ta właściwość jest typu `FlexBasis`, struktury, który definiuje właściwość statyczna typu `FlexBasis` o nazwie `Auto`, co jest ustawieniem domyślnym. Można użyć `Basis` Aby określić rozmiar pikseli lub wartość procentowa, która wskazuje, ile miejsca elementu zajmuje na osi głównej. Jest to _podstawy_ ponieważ określa on rozmiar elementu, który stanowi podstawę wszystkie kolejne układu.

- `Grow` Właściwość jest ustawiona na zagnieżdżony `Layout` i na `Label` podrzędnych reprezentujący zawartości. Ta właściwość jest typu `float` i ma wartość domyślną równą 0. Gdy są ustawione na wartość dodatnią, wszystkie pozostałe miejsce na osi głównej jest przydzielony do tego elementu i elementów równorzędnych o wartości dodatnie `Grow`. Miejsce jest przydzielane proporcjonalnie do wartości, przypominające specyfikacji gwiazdki w `Grid`.

    Pierwszy `Grow` dołączona właściwość jest ustawiona na zagnieżdżony `FlexLayout`, wskazujące tego `FlexLayout` jest zajmować wszystkie nieużywane miejsce pionowy w zewnętrznego `FlexLayout`. Drugi `Grow` dołączona właściwość jest ustawiona na `Label` reprezentujący zawartości, co oznacza, że ta zawartość zajmować wszystkie nieużywane miejsce poziomy w wewnętrznej `FlexLayout`.

    Istnieje również podobne `Shrink` dołączonych właściwości możliwej do wiązania, którego można użyć, gdy rozmiar przekracza rozmiar elementów podrzędnych `FlexLayout` , ale zawijania nie jest wymagana.

### <a name="catalog-items-with-flexlayout"></a>Elementy katalogu z FlexLayout

**Elementy katalogu** strony **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** próbki jest podobny do [przykład 1 w sekcji 1.1 specyfikacji pole układu Flex CSS](http://www.w3.org/TR/css-flexbox-1/#overview)z tą różnicą, że wyświetla poziomie przewijanego serii obrazów i opisy trzy małpy:

[![Katalog elementy stronicowy](flex-layout-images/CatalogItems.png "katalogu elementy stronicowy")](flex-layout-images/CatalogItems-Large.png#lightbox)

Każdy z trzech małpy jest `FlexLayout` zawartych w `Frame` podano jawne wysokości i szerokości i która jest również podrzędny większego `FlexLayout`. W tym pliku XAML, większość właściwości `FlexLayout` elementy podrzędne są określone w style wszystkie oprócz jednego jest niejawne stylu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CatalogItemsPage"
             Title="Catalog Items">
    <ContentPage.Resources>
        <Style TargetType="Frame">
            <Setter Property="BackgroundColor" Value="LightYellow" />
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="Margin" Value="10" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="Margin" Value="0, 4" />
        </Style>

        <Style x:Key="headerLabel" TargetType="Label">
            <Setter Property="Margin" Value="0, 8" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="Blue" />
        </Style>

        <Style TargetType="Image">
            <Setter Property="FlexLayout.Order" Value="-1" />
            <Setter Property="FlexLayout.AlignSelf" Value="Center" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="White" />
            <Setter Property="BackgroundColor" Value="Green" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}"
                           WidthRequest="240"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Niejawne styl `Image` obejmuje ustawienia dwóch dołączone można powiązać właściwości `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

`Order` Ustawienie &ndash;1 spowoduje, że `Image` element ma być wyświetlany najpierw w każdym z zagnieżdżone `FlexLayout` widoków niezależnie od jego położenie w kolekcji elementów podrzędnych. `AlignSelf` Właściwość `Center` powoduje, że `Image` do wyśrodkowany w `FlexLayout`. Przesłania ustawienie `AlignItems` właściwość, która ma wartość domyślną z `Stretch`, co oznacza że `Label` i `Button` elementy podrzędne są rozciągany w celu pełnej szerokości wprowadzonej `FlexLayout`.

W każdej z trzech `FlexLayout` wyświetla pusty `Label` poprzedza `Button`, ale ma `Grow` ustawienie 1. Oznacza to, że wszystkie bardzo pionowy miejsce jest przydzielane do tego pustego `Label`, które skutecznie wypchnięcia `Button` w dół.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>Właściwości szczegółowo

Teraz, przedstawiono niektóre typowe zastosowania `FlexLayout`, właściwości `FlexLayout` może być eksplorowany bardziej szczegółowo. 
`FlexLayout` Definiuje sześć właściwości, które można ustawić `FlexLayout` siebie, kodu lub XAML do sterowania orientatin i wyrównania. (Jednej z tych właściwości [ `Position` ](xref:Xamarin.Forms.FlexLayout.Position), nie jest uwzględnione w tym artykule.)

Możesz eksperymentować z pięciu pozostałe właściwości możliwej do wiązania za pomocą **eksperymentu** strony **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** próbki. Ta strona służy do dodawania lub usuwania elementów podrzędnych z `FlexLayout` i ustawić kombinacji pięć właściwości. Wszystkie elementy podrzędne elementu `FlexLayout` są `Label` widoków różnych kolorów i rozmiary, z `Text` właściwość liczba odpowiadająca jej położenie w `Children` kolekcji.

Gdy program uruchamiania, pięć `Picker` widoków wyświetlić domyślne wartości tych pięciu `FlexLayout` właściwości. `FlexLayout` w dolnej części ekranu zawiera trzy elementy podrzędne:

[![Strona eksperymentu: Domyślna](flex-layout-images/ExperimentDefault.png "domyślna strona eksperymentu -")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Każdy z `Label` widoków ma szare tło pokazujący miejsce przydzielone do tego `Label` w `FlexLayout`. Tło `FlexLayout` jest niebieski Alicji. Z wyjątkiem małego margines w lewo i w prawo go zajmuje obszar całego dołu strony.

<a name="direction" />

### <a name="the-direction-property"></a>Właściwość kierunku

[ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) Właściwość jest typu [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection), wyliczenie z czterech członków:

- `Column`
- `ColumnReverse` (lub "kolumny reverse" w języku XAML)
- `Row`, wartość domyślna
- `RowReverse` (lub "wiersza reverse" w języku XAML)

W języku XAML możesz określić wartość tej właściwości przy użyciu nazwy elementu członkowskiego wyliczenia w małe litery, wielkie litery, lub wielkich i małych liter, lub można użyć dwóch dodatkowe ciągi wyświetlane w nawiasach, które są takie same jak wskaźniki CSS. (Parametry "kolumny wstecznego" i "wiersza wstecznego" są zdefiniowane w [ `FlexDirectionTypeConverter` ](xref:Xamarin.Forms.FlexDirectionTypeConverter) klasa używana przez XAML parser.)

Oto **eksperymentu** strona wyświetlająca (od lewej do prawej), `Row` kierunku, `Column` kierunek i `ColumnReverse` kierunek:

[![Strony eksperymentu: Kierunek](flex-layout-images/ExperimentDirection.png "stronie eksperymentu - kierunku")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

Zwróć uwagę, że dla `Reverse` opcje elementy są liczone od prawej lub dolnej.

<a name="wrap" />

### <a name="the-wrap-property"></a>Właściwość zawijania

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) Właściwość jest typu [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap), wyliczenie z trzech elementów członkowskich:

- `NoWrap`, wartość domyślna
- `Wrap`
- `Reverse` (lub "wrap-reverse" w języku XAML)

Od lewej do prawej, Pokaż te ekrany `NoWrap`, `Wrap` i `Reverse` opcje 12 elementów podrzędnych:

[![Stronie eksperymentu: Zawijaj](flex-layout-images/ExperimentWrap.png "stronie eksperymentu — zawijanie")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Gdy `Wrap` właściwość jest ustawiona na `NoWrap` osi głównej jest ograniczone (tak jak ten program) i osi głównej nie jest szerokość lub wysokość, do wszystkich elementów podrzędnych, `FlexLayout` próbuje zmniejszyć elementy, jak zrzut ekranu z systemem iOS Pokazuje. Można kontrolować shrinkness elementów mających [ `Shrink` ](#shrink) dołączonych właściwości możliwej do wiązania.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>Właściwość JustifyContent

[ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) Właściwość jest typu [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify), wyliczenie z sześciu członków:

- `Start` (lub "flex-start" w języku XAML), wartość domyślna
- `Center`
- `End` (lub "flex-end" w języku XAML)
- `SpaceBetween` (lub "miejsca między" w języku XAML)
- `SpaceAround` (lub "miejsca obejścia" w języku XAML)
- `SpaceEvenly`

Ta właściwość określa, jak elementy są rozmieszczone na osi głównej, która jest osi poziomej w tym przykładzie:

[![Strona eksperymentu: Justify zawartości](flex-layout-images/ExperimentJustifyContent.png "stronie eksperymentu - Justify zawartości")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

Wszystkie trzy zrzuty ekranu `Wrap` właściwość jest ustawiona na `Wrap`. `Start` Domyślny jest wyświetlany w obszarze poprzedni zrzut ekranu dla systemu Android. W tym miejscu wartość iOS zrzut ekranu pokazuje `Center` opcja: wszystkie elementy są przenoszone do Centrum. Trzy pozostałe opcje rozpoczynające się od słowa `Space` przydzielić dodatkowe miejsce nie jest zajmowany przez elementy. `SpaceBetween` przydziela miejsce równomiernie w poszczególnych elementów. `SpaceAround` naraża równa każdego elementu ilości wolnego miejsca podczas `SpaceEvenly` naraża równa odstęp między każdym z elementów i przed pierwszym elementem i po ostatnim elemencie w wierszu.

<a name="align-items" />

### <a name="the-alignitems-property"></a>Właściwość AlignItems

[ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) Właściwość jest typu [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems), wyliczenie z czterech członków:

- `Stretch`, wartość domyślna
- `Center`
- `Start` (lub "flex-start" w języku XAML)
- `End` (lub "flex-end" w języku XAML)

Jest to jeden z dwóch właściwości (inne trwa [ `AlignContent` ](#align-content)) który wskazuje sposób wyrównania elementów podrzędnych na osi poprzecznej. W każdym wierszu elementy podrzędne są rozciągnięty (jak pokazano na poprzednim zrzucie ekranu) lub wyrównane początkową, center lub koniec każdego elementu, jak pokazano na poniższych zrzutach ekranu trzy:

[![Strona eksperymentu: Dopasuj elementów](flex-layout-images/ExperimentAlignItems.png "stronie eksperymentu - wyrównywanie elementów")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

Zrzut ekranu z systemem iOS wierzchołki wszystkie elementy podrzędne są wyrównane. Android zrzuty ekranu elementy są wyśrodkowane w pionie na podstawie w elemencie podrzędnym najwyższego. Zrzut ekranu platformy uniwersalnej systemu Windows dołu wszystkie elementy są wyrównane.

Dla żadnego elementu `AlignItems` ustawienie może zostać zastąpiona przez [ `AlignSelf` ](#align-self) dołączonych właściwości możliwej do wiązania.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>Właściwość AlignContent

[ `AlignContent` ](xref:Xamarin.Forms.FlexLayout.AlignContent) Właściwość jest typu [ `FlexAlignContent` ](xref:Xamarin.Forms.FlexAlignContent), wyliczenie z siedmiu członków:

- `Stretch`, wartość domyślna
- `Center`
- `Start` (lub "flex-start" w języku XAML)
- `End` (lub "flex-end" w języku XAML)
- `SpaceBetween` (lub "miejsca między" w języku XAML)
- `SpaceAround` (lub "miejsca obejścia" w języku XAML)
- `SpaceEvenly`

Podobnie jak `AlignItems`, `AlignContent` właściwość również wyrównuje elementów podrzędnych na osi poprzecznej, ale wpływa na całą wierszy lub kolumn:

[![Strona eksperymentu: Wyrównaj zawartość](flex-layout-images/ExperimentAlignContent.png "stronie eksperymentu - Align zawartości")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

W iOS screnshot oba wiersze znajdowały się u góry; Zrzut ekranu dla systemu Android, są one w Centrum; i na zrzucie ekranu platformy uniwersalnej systemu Windows są one na dole. Wiersze mogą również rozmieszczonych w na różne sposoby:

[![Strona eksperymentu: Dopasuj zawartości 2](flex-layout-images/ExperimentAlignContent2.png "stronie eksperymentu - Align zawartości 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent` Nie obowiązuje, gdy istnieje tylko jeden wiersz lub kolumnę.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>Dołączone właściwości powiązania szczegółowo

`FlexLayout` Definiuje właściwości dołączonych pięć. Te właściwości są ustawione na elementy podrzędne `FlexLayout` i dotyczą tylko tego konkretnego podrzędnych.

<a name="align-self" />

### <a name="the-alignself-property"></a>Właściwość AlignSelf

[ `AlignSelf` ](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) Jest dołączona właściwość można powiązać typu [ `FlexAlignSelf` ](xref:Xamarin.Forms.FlexAlignContent), wyliczenie z pięciu członków:

- `Auto`, wartość domyślna
- `Stretch`
- `Center`
- `Start` (lub "flex-start" w języku XAML)
- `End` (lub "flex-end" w języku XAML)

Dla wszystkich poszczególnych podrzędnych z `FlexLayout`, ta właściwość pierwszeństwo przez ustawieniem skonfigurowanym [ `AlignItems` ](#align-items) ustawiona w `FlexLayout` samej siebie. Domyślne ustawienie `Auto` oznacza korzystanie z `AlignItems` ustawienie.

Dla `Label` elementu o nazwie `label` (lub przykład), można ustawić `AlignSelf` właściwości w kodzie w następujący sposób:

```csharp
FlexAlign.SetAlignSelf(label, FlexAlignSelf.Center);
```

Zwróć uwagę, że nie istnieje żadne odwołanie do `FlexLayout` nadrzędny `Label`. W języku XAML właściwość następująco:

```xaml
<Label ... FlexAlign.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Właściwości Order

[ `Order` ](xref:Xamarin.Forms.FlexLayout.OrderProperty) Właściwość jest typu `int`. Wartość domyślna to 0.

`Order` Właściwości umożliwia zmianę kolejności który elementów podrzędnych `FlexLayout` ułożone. Zazwyczaj dzieci `FlexLayout` ułożone jest tej samej kolejności, w jakiej występują w `Children` kolekcji. Można zastąpić to zamówienie, ustawiając `Order` dołączona właściwość może być powiązana wartość niezerową liczbą całkowitą na jeden lub więcej elementów podrzędnych. `FlexLayout` Następnie Rozmieszcza elementy podrzędne w oparciu o ustawieniu `Order` właściwości w każdej podrzędnej, ale elementów podrzędnych o takim samym `Order` ustawienia są rozmieszczone w kolejności ich występowania w `Children` kolekcji.

### <a name="the-basis-property"></a>Właściwość podstawy

[ `Basis` ](xref:Xamarin.Forms.FlexLayout.BasisProperty) Dołączona właściwość można powiązać wskazuje ilość miejsca, który jest przydzielony do elementu podrzędnego `FlexLayout` na osi głównej. Określony rozmiar przez `Basis` rozmiar osi głównego elementu nadrzędnego jest właściwość `FlexLayout`. W związku z tym `Basis` wskazuje szerokość elementu podrzędnego, gdy elementy podrzędne są rozmieszczone w lub wysokość wierszy, gdy elementy podrzędne są rozmieszczone w kolumnach.

`Basis` Właściwość jest typu [ `FlexBasis` ](xref:Xamarin.Forms.FlexBasis), strukturę. Rozmiar można określić w jednostkach niezależnych od urządzenia lub jako procent rozmiaru `FlexLayout`. Wartość domyślna `Basis` właściwość ma właściwości statycznej `FlexBasis.Auto`, co oznacza, że podrzędne żądanej przez szerokość lub wysokość jest używany.

W kodzie, można ustawić `Basis` właściwość `Label` o nazwie `label` do 40 jednostek niezależnych od urządzenia następująco:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

Drugi argument `FlexBasis` nosi nazwę konstruktora `isRelative` i wskazuje, czy rozmiar jest względna (`true`) lub bezwzględną (`false`). Argument ma wartość domyślną `false`, więc można także użyć poniższego kodu:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Niejawna konwersja z `float` do `FlexBasis` jest zdefiniowane, więc może ona jeszcze bardziej uprościć:

```csharp
FlexLayout.SetBasis(label, 40);
```

Ustawienie rozmiaru na 25% `FlexLayout` nadrzędnego następująco:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

Ta wartość ułamkową musi należeć do zakresu od 0 do 1.

W języku XAML można użyć numeru rozmiar w jednostkach niezależnych od urządzenia:

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

Lub możesz określić wartość procentową z zakresu od 0 do 100%:

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

**Eksperymentu podstawy** strony **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** próbki pozwala na wypróbowanie `Basis` właściwości. Na stronie są wyświetlane kolumny opakowana pięć `Label` elementów ze zmieniającymi się kolory tła i pierwszego planu. Dwa `Slider` elementy umożliwiają określenie `Basis` wartości drugim i czwartym `Label`:

[![Podstawy eksperymentu strony](flex-layout-images/BasisExperiment.png "podstawę eksperymentu strony")](flex-layout-images/BasisExperiment-Large.png#lightbox)

Zrzut ekranu dla systemu iOS po lewej stronie przedstawia dwa `Label` elementy są podane wysokość w jednostkach niezależnych od urządzenia. Android ekranu pokazuje ich podane są wysokości będących część całkowitej wysokości `FlexLayout`. Jeśli `Basis` jest ustawiona na 100%, wysokość jest elementem podrzędnym `FlexLayout`, a także być zawijany do następnej kolumnie i zajmują cały wysokość tej kolumny, jak pokazano na zrzucie ekranu platformy uniwersalnej systemu Windows: wygląda, jakby pięć elementy podrzędne są rozmieszczone w wierszu , ale zostaną faktycznie ułożone w pięciu kolumn.

### <a name="the-grow-property"></a>Zwiększ właściwości

[ `Grow` ](xref:Xamarin.Forms.FlexLayout.GrowProperty) Jest dołączona właściwość można powiązać typu `int`. Wartość domyślna to 0, a wartość musi być większa lub równa 0.

`Grow` Właściwości pełni rolę, podczas gdy `Wrap` właściwość jest ustawiona na `NoWrap` i wierszy podrzędnych ma łączna szerokość mniejszą niż szerokość `FlexLayout`, lub kolumn podrzędnych ma wysokości krótszy niż `FlexLayout`. `Grow` Właściwość wskazuje, jak rozdzielić pozostałe miejsce między elementami podrzędnymi.

W **wzrostu eksperymentu** strony pięć `Label` kolory naprzemiennych elementów ułożone w kolumnie i dwa `Slider` elementy umożliwiają dostosowanie `Grow` właściwość drugim i czwartym `Label`. Zrzut ekranu dla systemu iOS na lewo przedstawiono domyślne `Grow` właściwości 0:

[![Na stronie eksperymentu Zwiększaj](flex-layout-images/GrowExperiment.png "stronie eksperymentu wzrostu")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Jeśli zostanie podany, wszelkie jeden element podrzędny dodatnią `Grow` wartości, a następnie dzieckiem zajmuje całe pozostałe miejsce, zgodnie z systemem Android zrzut ekranu przedstawia. To miejsce także przydzielanie między co najmniej dwa elementy podrzędne. Zrzut ekranu platformy uniwersalnej systemu Windows `Grow` właściwości drugiego `Label` ma ustawioną wartość 0,5, podczas `Grow` właściwości czwartego `Label` jest w wersji 1.5, co daje czwarty `Label` trzy razy więcej miejsca pozostałość jako drugi `Label`.

Jak widok podrzędny używa tego miejsca zależy od typu danego elementu podrzędnego. Aby uzyskać `Label`, tekst może być umieszczony wewnątrz obszaru całkowitą `Label` przy użyciu właściwości `HorizontalTextAlignment` i `VerticalTextAlignment`.

<a name="shrink" />

### <a name="the-shrink-property"></a>Właściwość zmniejszania

[ `Shrink` ](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) Jest dołączona właściwość można powiązać typu `int`. Wartość domyślna to 1, a wartość musi być większa lub równa 0.

`Shrink` Właściwości pełni rolę podczas `Wrap` właściwość jest ustawiona na `NoWrap` i łączna szerokość wiersza elementów podrzędnych jest większa niż szerokość `FlexLayout`, albo jest większa niż wysokość agregacji pojedynczej kolumny elementów podrzędnych wysokość `FlexLayout`. Zwykle `FlexLayout` przez constricting ich rozmiary wyświetli te elementy podrzędne. `Shrink` Właściwości można wskazać dzieci, które otrzymują odpowiedni priorytet w wyświetlane w pełnej wielkości.

**Zmniejszyć eksperymentu** tworzy stronę `FlexLayout` z pojedynczy wiersz pięć `Label` elementy podrzędne, które wymagają więcej miejsca niż `FlexLayout` szerokości. Zrzut ekranu dla systemu iOS po lewej stronie zawiera wszystkie `Label` elementy z wartościami domyślnymi 1:

[![Zmniejszanie eksperymentu strony](flex-layout-images/ShrinkExperiment.png "zmniejszania eksperymentu strony")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Zrzut ekranu dla systemu Android `Shrink` wartość dla drugiego `Label` jest równa 0, a `Label` jest wyświetlany w pełnej szerokości. Ponadto czwarty `Label` podano `Shrink` wartość większa niż jeden, a jego został zmniejszony. Zrzut ekranu platformy uniwersalnej systemu Windows zawiera zarówno `Label` elementy podane są `Shrink` wartość 0, aby zezwolić im na być wyświetlany w ich pełnego rozmiaru, jeśli to możliwe.

Można jednocześnie ustawić `Grow` i `Shrink` wartości, aby zmieścił się w sytuacjach, gdy rozmiarów agregacji podrzędnych może czasami mniejsze niż lub czasami jest większy niż rozmiar `FlexLayout`.

## <a name="css-styling-with-flexlayout"></a>Style CSS z FlexLayout

Można użyć [stylów CSS](~/xamarin-forms/user-interface/styles/css/index.md) funkcja wprowadzona w systemie platformy Xamarin.Forms 3.0 w połączeniu z `FlexLayout`. **Elementy katalogu CSS** strony **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** próbki duplikaty układ **elementy katalogu** strony, ale z CSS Arkusz stylów dla wielu style:

[![Strona elementów wykazu CSS](flex-layout-images/CssCatalogItems.png "strona elementów wykazu CSS")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

Oryginalna **CatalogItemsPage.xaml** plik ma pięć `Style` definicje w jego `Resources` sekcji z 15 `Setter` obiektów. W **CssCatalogItemsPage.xaml** pliku, która została zmniejszona do dwóch `Style` definicji tylko cztery `Setter` obiektów. Te style uzupełnić arkusza stylów CSS dla właściwości, które funkcja stylów CSS platformy Xamarin.Forms obecnie nie obsługuje:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CssCatalogItemsPage"
             Title="CSS Catalog Items">
    <ContentPage.Resources>
        <StyleSheet Source="CatalogItemsStyles.css" />

        <Style TargetType="Frame">
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey" StyleClass="header" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey" StyleClass="header" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey" StyleClass="header" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

W pierwszym wierszu odwołuje się do arkusza stylów CSS `Resources` sekcji:

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

Zauważ również, że obejmuje dwa elementy w każdym z trzech elementów `StyleClass` ustawienia:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

Te dotyczą selektorów w **CatalogItemsStyles.css** arkusza stylów:

```css
frame {
    width: 300;
    height: 480;
    background-color: lightyellow;
    margin: 10;
}

label {
    margin: 4 0;
}

label.header {
    margin: 8 0;
    font-size: large;
    color: blue;
}

label.empty {
    flex-grow: 1;
}

image {
    height: 180;
    order: -1;
    align-self: center;
}

button {
    font-size: large;
    color: white;
    background-color: green;
}
```

Kilka `FlexLayout` odwołujesz dołączonych właściwości. W `label.empty` selektora, zobaczysz `flex-grow` atrybut, który style pustą `Label` zapewnienie puste miejsce powyżej `Button`. `image` Selektor zawiera `order` atrybutu i `align-self` atrybutów, które odpowiadają `FlexLayout` dołączone właściwości.

Widzisz, że właściwości można ustawić bezpośrednio na `FlexLayout` i dołączonych właściwości można ustawić na elementy podrzędne `FlexLayout`. Alternatywnie można ustawić te właściwości pośrednio przy użyciu tradycyjnych style opartych na języku XAML lub style CSS. Ważne jest znać i zrozumienie tych właściwości. Te właściwości są, co sprawia, że `FlexLayout` naprawdę elastyczne. 

## <a name="flexlayout-with-xamarinuniversity"></a>FlexLayout z Xamarin.University

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Platformy Xamarin.Forms 3.0 Flex układ przez [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Linki pokrewne

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
