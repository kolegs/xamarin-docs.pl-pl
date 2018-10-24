---
title: FlexLayout zestawu narzędzi Xamarin.Forms
description: Na użytek FlexLayout układania lub zawijania Kolekcja widoków podrzędnych.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: a6c1b0a4e0df1c25f595ca4eb53079c74b84972e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998586"
---
# <a name="the-xamarinforms-flexlayout"></a>FlexLayout zestawu narzędzi Xamarin.Forms

_Na użytek FlexLayout układania lub zawijania Kolekcja widoków podrzędnych._

Xamarin.Forms [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout) nowego w interfejsie Xamarin.Forms w wersji 3.0 lub nowszej. Jest on oparty na CSS [elastyczne modułu Układ pola](http://www.w3.org/TR/css-flexbox-1/), powszechnie znane jako _flex układ_ lub _flex-box_, więc wywołuje się, ponieważ zawiera on wiele elastycznych opcji Rozmieść elementy podrzędne w układzie.

`FlexLayout` jest podobny do Xamarin.Forms [ `StackLayout` ](~/xamarin-forms/user-interface/layouts/stack-layout.md) się go można rozmieścić jego elementy podrzędne w poziomie i pionie na stosie. Jednak `FlexLayout` również jest zdolny do zawijania jego elementy podrzędne, jeśli jest zbyt duża, aby zmieścić ją w pojedynczym wierszu lub kolumnie, a także zawiera wiele opcji umożliwiających orientacji, wyrównanie i dostosowania do różnych rozmiarów ekranu.

`FlexLayout` pochodzi od klasy [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) i dziedziczy [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) właściwości typu `IList<View>`.

`FlexLayout` Definiuje sześć właściwości możliwej do wiązania publiczne i pięciu dołączonych właściwości możliwej do wiązania, które wpływają na rozmiar, orientacji i dostosowania jego elementy podrzędne. (Jeśli nie znasz dołączone właściwości możliwej do wiązania, zobacz artykuł  **[dołączonych właściwości](~/xamarin-forms/xaml/attached-properties.md)**.) Te właściwości są opisane szczegółowo w sekcji poniżej na **[właściwości możliwe do wiązania szczegółowo](#bindable-properties)** i  **[dołączone właściwości możliwej do wiązania szczegółowo](#attached-properties)**. Jednak w tym artykule zaczyna się od sekcji niektórych **[typowe scenariusze użycia](#common-scenarios)** z `FlexLayout` opisujący wiele z tych właściwości więcej nieformalnie. Pod koniec tego artykułu, zobaczysz jak połączyć `FlexLayout` z [arkusze stylów CSS](~/xamarin-forms/user-interface/styles/css/index.md).

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>Typowe scenariusze użycia

**[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** przykładowy program zawiera kilka stron tego przedstawiają niektóre typowe zastosowania `FlexLayout` i pozwala na eksperymentowanie z jego właściwości.

### <a name="using-flexlayout-for-a-simple-stack"></a>Za pomocą FlexLayout proste stosu

**Proste stosu** stronie pokazuje, jak `FlexLayout` można podstawić `StackLayout` przy użyciu znaczników prostsze, ale. Wszystko, co w tym przykładzie jest zdefiniowana w strony XAML. `FlexLayout` Zawiera cztery elementy podrzędne:

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

Poniżej przedstawiono tę stronę z systemem iOS, Android i platformy uniwersalnej Windows:

[![Prostą stosu strony](flex-layout-images/SimpleStack.png "prostą stosu strony")](flex-layout-images/SimpleStack-Large.png#lightbox)

Trzy właściwości `FlexLayout` są wyświetlane w **SimpleStackPage.xaml** pliku:

- [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) Właściwość jest ustawiona na wartość [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection) wyliczenia. Wartość domyślna to `Row`. Ustawienie właściwości `Column` powoduje, że elementy podrzędne `FlexLayout` być rozmieszczone w jednej kolumnie elementów.

    Gdy elementy w `FlexLayout` są rozmieszczone w kolumnie, `FlexLayout` ma pionowej _osi głównej_ i poziomy _wielu osi_.

- [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) Właściwość jest typu [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems) i określa sposób wyrównania elementów na osi poprzecznej. `Center` Opcja powoduje, że każdy element, aby wyśrodkowany w poziomie.

    W przypadku używania `StackLayout` zamiast `FlexLayout` dla tego zadania będzie centrum wszystkich elementów, przypisując `HorizontalOptions` właściwość do każdego elementu `Center`. `HorizontalOptions` Właściwości nie działa dla elementów podrzędnych `FlexLayout`, ale pojedynczej precyzji `AlignItems` właściwość w ramach tego samego celu. Jeśli zachodzi potrzeba, można użyć `AlignSelf` dołączonych właściwości możliwej do wiązania, aby zastąpić `AlignItems` właściwości dla poszczególnych elementów:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    Dzięki temu ta `Label` znajduje się w lewej krawędzi `FlexLayout` po kolejność czytania od lewej do prawej.

- [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) Właściwość jest typu [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify)i określa, jak elementy są uporządkowane na osi głównej. `SpaceEvenly` Opcja przydziela wszystkie pozostałe pionowy odstęp równo między wszystkie elementy i przed pierwszy element i ostatni element.

    W przypadku używania `StackLayout`, trzeba przypisać `VerticalOptions` właściwość do każdego elementu `CenterAndExpand` aby osiągnąć ten sam efekt. Ale `CenterAndExpand` opcja będzie zajmować dwa razy więcej miejsca na dysku między poszczególnymi elementami niż przed pierwszym i po ostatnim elemencie. Można naśladować `CenterAndExpand` opcji `VerticalOptions` , ustawiając `JustifyContent` właściwość `FlexLayout` do `SpaceAround`.

Te `FlexLayout` właściwości zostały omówione bardziej szczegółowo w sekcji **[właściwości możliwe do wiązania szczegółowo](#bindable-properties)** poniżej.

### <a name="using-flexlayout-for-wrapping-items"></a>Za pomocą FlexLayout zawijania elementów

**Zawijania zdjęcie** strony **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** w przykładzie pokazano, jak `FlexLayout` może zawijać się jego elementów podrzędnych do dodatkowych wierszy lub kolumn. Tworzy plik XAML `FlexLayout` i przypisuje dwóch właściwości:

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

`Direction` Właściwość to `FlexLayout` nie jest ustawiona, dlatego ma domyślne ustawienie `Row`, co oznacza, że elementy podrzędne są ułożone w wiersze i głównej osi jest poziomy.

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) Właściwość jest typu wyliczeniowego [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap). W przypadku zbyt wiele elementów, aby dopasować wiersz ustawienie tej właściwości powoduje elementy do zawijany do następnego wiersza.

Należy zauważyć, że `FlexLayout` jest elementem podrzędnym `ScrollView`. Jeśli istnieje zbyt wiele wierszy, aby zmieściły się na stronie, a następnie `ScrollView` ma wartość domyślną `Orientation` właściwość `Vertical` i umożliwia przewijanie w pionie.

`JustifyContent` Właściwość przydziela pozostała ilość miejsca na osi głównej (osi poziomej) tak, aby każdy element jest ujęta w tyle samo miejsca puste.

Plik związany z kodem uzyskuje dostęp do kolekcja zdjęć próbki i doda je do `Children` zbiór `FlexLayout`:

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

W tym miejscu jest uruchomiony na trzech platformach, stopniowo przewijane od góry do dołu program:

[![Na stronie zawijania zdjęcie](flex-layout-images/PhotoWrapping.png "strony zawijania zdjęć")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>Układ strony z FlexLayout

Istnieje standardowego układu w projekcie sieci web o nazwie [ _marzeniem Stolica_ ](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) ponieważ jest on format układ, który jest bardzo pożądane, ale często trudno weź pod uwagę przy użyciu doskonałej jakości. Układ składa się z nagłówkiem w górnej części strony i stopki u dołu, oba rozszerzenia na całą szerokość strony. Zajmuje środek strony jest główną zawartością, ale często z kolumnowych menu po lewej stronie zawartości i dodatkowe informacje (nazywane czasem _specjalnie_ obszaru) po prawej stronie. [Sekcja 5.4.1 specyfikacji CSS elastyczny Układ pola](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) w tym artykule opisano, jak układ marzeniem Stolica można realizować za pomocą pola elastycznego.

**Układ Marzeniem Stolica** strony **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** przykład pokazuje prosty implementację tego układu przy użyciu jednej `FlexLayout` zagnieżdżona w innej. Obszary, w lewo i prawo do obszaru zawartości, ponieważ ta strona jest przeznaczona dla telefonów w trybie pionowym, są tylko 50 pikseli szerokości:

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

W tym miejscu jest uruchomiona na trzech platformach:

[![Strona układu Marzeniem Stolica](flex-layout-images/HolyGrailLayout.png "Marzeniem Stolica stronę układu")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

Obszary nawigacji i aside są renderowane przy użyciu `BoxView` po lewej i prawej stronie.

Pierwszy `FlexLayout` w XAML pliku głównego osi pionowej, a zawiera trzy elementy podrzędne, uporządkowane według kolumny. Są to nagłówek, treść strony oraz stopkę. Zagnieżdżony `FlexLayout` ma głównej osi poziomej, z trzema elementami podrzędnymi rozmieszczone w wierszu.

Trzy właściwości możliwej do wiązania dołączone zostały przedstawione w tym programie:

- `Order` Dołączonych właściwości możliwej do wiązania jest ustawiona na pierwszym `BoxView`. Ta właściwość jest liczbą całkowitą o wartości domyślnej 0. Ta właściwość służy do zmiany kolejności układu. Ogólnie deweloperzy Preferuj zawartość wyświetlania strony w znacznikach przed elementy nawigacji i specjalnie elementów. Ustawienie `Order` właściwości pierwszego `BoxView` na wartość mniejszą niż jego elementów równorzędnych powoduje, że pojawiało się jako pierwszy element w wierszu. Podobnie, można zagwarantować, że element jest wyświetlany ostatni, ustawiając `Order` właściwości do wartości większej niż jego elementów równorzędnych.

- `Basis` Dołączonych właściwości możliwej do wiązania jest ustawiona na dwa `BoxView` elementów, aby dać im szerokość 50 pikseli. Ta właściwość jest typu `FlexBasis`, struktury, który definiuje właściwość statyczna typu `FlexBasis` o nazwie `Auto`, co jest ustawieniem domyślnym. Możesz użyć `Basis` Aby określić rozmiar w pikselach lub wartość procentowa, która wskazuje, ile miejsca elementu zajmuje się na osi głównej. Jest on nazywany _podstawy_ ponieważ określa rozmiar elementu, który stanowi podstawę wszystkie kolejne układu.

- `Grow` Właściwość jest ustawiona na zagnieżdżony `Layout` i `Label` podrzędnych reprezentujący zawartości. Ta właściwość jest typu `float` i ma wartość domyślną równą 0. Po ustawieniu na wartość dodatnią, całe pozostałe miejsce na osi głównej jest przydzielony do tego elementu i elementów równorzędnych o wartości dodatnie `Grow`. Miejsce jest przydzielane proporcjonalnie do wartości, przypominające specyfikacji gwiazdki w `Grid`.

    Pierwszy `Grow` dołączoną właściwość jest ustawiona na zagnieżdżony `FlexLayout`oznaczający że `FlexLayout` się zajmować wszystkie nieużywane miejsce pionowy w zewnętrzny blok `FlexLayout`. Drugi `Grow` dołączoną właściwość jest ustawiona na `Label` reprezentujący zawartości, co oznacza, że ta zawartość zajmą wszystkich nieużywanych przestrzeni w poziomie w ramach wewnętrzny `FlexLayout`.

    Istnieje również podobny `Shrink` dołączonych właściwości możliwej do wiązania, którego można użyć, gdy rozmiar elementów podrzędnych przekracza rozmiar `FlexLayout` , ale opakowujące aplikacje nie jest wymagana.

### <a name="catalog-items-with-flexlayout"></a>Elementy katalogu przy użyciu FlexLayout

**Elementów katalogu** strony w **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** przykład jest podobny do [przykład 1 w sekcji 1.1 specyfikacji pole układu Flex CSS](http://www.w3.org/TR/css-flexbox-1/#overview)z tą różnicą, że wyświetla poziomo przewijany serii obrazów i opisy trzy małpy:

[![Wykaz elementów strony](flex-layout-images/CatalogItems.png "wykazu elementów strony")](flex-layout-images/CatalogItems-Large.png#lightbox)

Każdy z trzech małpy jest `FlexLayout` zawarte w `Frame` otrzymuje jawne wysokości i szerokości, a który również jest elementem podrzędnym większego `FlexLayout`. W tym pliku XAML, większość właściwości `FlexLayout` elementy podrzędne są określone w stylach wszystkie oprócz jednego z nich jest styl niejawny:

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

Styl niejawny `Image` obejmuje ustawienia dwóch dołączonych możliwej do wiązania właściwości `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

`Order` Ustawienie &ndash;1 spowoduje, że `Image` element ma być wyświetlany najpierw w każdym zagnieżdżonym `FlexLayout` widokach, niezależnie od jego pozycja w kolekcji elementów podrzędnych. `AlignSelf` Właściwość `Center` powoduje, że `Image` do wyśrodkowany w ramach `FlexLayout`. Zastępuje ustawienie `AlignItems` właściwość, która ma wartość domyślną z `Stretch`, co oznacza że `Label` i `Button` elementy podrzędne są rozciągnięte do pełnej szerokości `FlexLayout`.

W ramach każdej z trzech `FlexLayout` widoków blank `Label` poprzedza `Button`, ale ma `Grow` ustawienie 1. Oznacza to, że bardzo pionowej przestrzeni jest przydzielany do to pole puste, `Label`, które skutecznie wypycha `Button` do dołu.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>Właściwości możliwe do wiązania szczegółowo

Teraz, gdy niektóre popularne aplikacje `FlexLayout`, właściwości `FlexLayout` można eksplorować bardziej szczegółowo. 
`FlexLayout` Definiuje sześć właściwości możliwej do wiązania, które można ustawić na `FlexLayout` samej usługi w kodzie lub XAML, kontrolki orientatin i wyrównania. (Jedną z tych właściwości [ `Position` ](xref:Xamarin.Forms.FlexLayout.Position), nieuwzględnione w tym artykule.)

Możesz eksperymentować z pięciu pozostałe właściwości możliwej do wiązania za pomocą **eksperymentować** strony **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** próbki. Ta strona umożliwia dodawanie lub usuwanie elementów podrzędnych z `FlexLayout` oraz ustawić kombinacje pięć właściwości możliwej do wiązania. Wszystkie obiekty podrzędne danego `FlexLayout` są `Label` widoki różne kolory i rozmiary, za pomocą `Text` właściwość ustawioną na numer odpowiadający jej położenie w `Children` kolekcji.

Po uruchomieniu programu się pięciu `Picker` widoków wyświetlić wartości domyślne tych pięciu `FlexLayout` właściwości. `FlexLayout` w dolnej części ekranu zawiera trzy elementy podrzędne:

[![Strona eksperymentu: Domyślna](flex-layout-images/ExperimentDefault.png "domyślna strona eksperymentu —")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Każdy z `Label` widoków ma szarego tła, pokazujący miejsce przydzielone do tego `Label` w ramach `FlexLayout`. Tło `FlexLayout` jest Stalowobłękitny. Zajmuje obszar dolnej całej strony, z wyjątkiem nieco margines w lewo i w prawo.

<a name="direction" />

### <a name="the-direction-property"></a>Właściwość kierunku

[ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) Właściwość jest typu [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection), wyliczenie z cztery elementy członkowskie:

- `Column`
- `ColumnReverse` (lub "kolumna reverse" w XAML)
- `Row`, wartość domyślna
- `RowReverse` (lub "wiersz reverse" w XAML)

XAML można określić wartość tej właściwości przy użyciu nazwy elementów członkowskich wyliczenia w małe litery, wielkie litery, lub wielkich i małych liter, lub użyć dwa dodatkowe ciągi wyświetlane w nawiasach, które są takie same jak wskaźniki CSS. (Ciągi "odwrotnej kolejności kolumn" i "odwrotnej kolejności wierszy" są zdefiniowane w [ `FlexDirectionTypeConverter` ](xref:Xamarin.Forms.FlexDirectionTypeConverter) klasa używana przez XAML parser.)

Oto **eksperymentu** przedstawiający (od lewej do prawej), stronę `Row` kierunku, `Column` kierunku i `ColumnReverse` kierunek:

[![Na stronie eksperymentu: Kierunek](flex-layout-images/ExperimentDirection.png "strona eksperymentu — kierunek")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

Należy zauważyć, że dla `Reverse` opcji elementy rozpoczynają się od prawej lub u dołu.

<a name="wrap" />

### <a name="the-wrap-property"></a>Właściwość Wrap

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) Właściwość jest typu [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap), wyliczenie z trzema elementami członkowskimi:

- `NoWrap`, wartość domyślna
- `Wrap`
- `Reverse` (lub "wrap-reverse" w XAML)

Od lewej do prawej, Pokaż te ekrany `NoWrap`, `Wrap` i `Reverse` opcje 12 elementów podrzędnych:

[![Na stronie eksperymentu: Zawijanie](flex-layout-images/ExperimentWrap.png "opakować strona eksperymentu —")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Gdy `Wrap` właściwość jest ustawiona na `NoWrap` głównej osi jest ograniczony (tak jak w tym programie) i osi głównej nie jest szerokość lub wysokość, aby dopasować wszystkie elementy podrzędne, `FlexLayout` próbuje zmniejszyć elementy, co zrzut ekranu z systemem iOS Pokazuje. Można kontrolować shrinkness elementów mających [ `Shrink` ](#shrink) dołączonych właściwości możliwej do wiązania.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>Właściwość JustifyContent

[ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) Właściwość jest typu [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify), wyliczenie z sześciu członków:

- `Start` (lub "flex-start" w XAML), wartość domyślna
- `Center`
- `End` (lub "flex-end" w XAML)
- `SpaceBetween` (lub "miejsca — od" w XAML)
- `SpaceAround` (lub "miejsce around" w XAML)
- `SpaceEvenly`

Ta właściwość określa, jak elementy są równe odstępy na osi głównej, która jest osi poziomej, w tym przykładzie:

[![Na stronie eksperymentu: Uzasadnienie zawartości](flex-layout-images/ExperimentJustifyContent.png "strona Experiment - Justify zawartości")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

Wszystkie trzy zrzuty ekranu `Wrap` właściwość jest ustawiona na `Wrap`. `Start` Domyślny jest wyświetlany na poprzednim zrzucie ekranu systemu Android. Na iOS zrzucie ekranu poniżej przedstawiono `Center` opcja: wszystkie elementy są przenoszone do Centrum. Trzy inne opcje rozpoczynające się od słowa `Space` przydzielić dodatkowe miejsce nie jest zajęta przez elementy. `SpaceBetween` przydziela miejsce równomiernie między elementami; `SpaceAround` umieszcza równa ilość wolnego miejsca wokół każdego elementu, gdy `SpaceEvenly` umieszcza równa miejsca między poszczególnymi elementami i przed pierwszym i po ostatnim elemencie w wierszu.

<a name="align-items" />

### <a name="the-alignitems-property"></a>Właściwość AlignItems

[ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) Właściwość jest typu [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems), wyliczenie z cztery elementy członkowskie:

- `Stretch`, wartość domyślna
- `Center`
- `Start` (lub "flex-start" w XAML)
- `End` (lub "flex-end" w XAML)

Jest to jedna z dwóch właściwości (inne są [ `AlignContent` ](#align-content)) wskazujący sposób wyrównania elementów podrzędnych na osi poprzecznej. W każdym wierszu elementy podrzędne są rozciągnięte (jak pokazano na poprzednim zrzucie ekranu) lub wyrównane na początku, center lub końcu każdego elementu, jak pokazano na poniższych zrzutach ekranu trzy:

[![Na stronie eksperymentu: Wyrównywanie elementów](flex-layout-images/ExperimentAlignItems.png "strona eksperymentu — wyrównywanie elementów")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

Zrzut ekranu z systemem iOS wierzchołki wszystkie elementy podrzędne są wyrównane. Android zrzuty ekranu elementy są wyśrodkowane w pionie oparte na najwyższy element podrzędny. Na zrzucie ekranu platformy uniwersalnej systemu Windows dołu wszystkie elementy są wyrównane.

Dla dowolnego pojedynczego elementu `AlignItems` ustawienie może zostać zastąpiona przez [ `AlignSelf` ](#align-self) dołączonych właściwości możliwej do wiązania.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>Właściwość AlignContent

[ `AlignContent` ](xref:Xamarin.Forms.FlexLayout.AlignContent) Właściwość jest typu [ `FlexAlignContent` ](xref:Xamarin.Forms.FlexAlignContent), wyliczenie z siedmiu członków:

- `Stretch`, wartość domyślna
- `Center`
- `Start` (lub "flex-start" w XAML)
- `End` (lub "flex-end" w XAML)
- `SpaceBetween` (lub "miejsca — od" w XAML)
- `SpaceAround` (lub "miejsce around" w XAML)
- `SpaceEvenly`

Podobnie jak `AlignItems`, `AlignContent` właściwości również jest wyrównywany elementów podrzędnych na osi poprzecznej, ale wpływa na całe wiersze lub kolumny:

[![Na stronie eksperymentu: Wyrównanie zawartości](flex-layout-images/ExperimentAlignContent.png "strona Experiment - Align zawartości")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

W screnshot dla systemu iOS oba wiersze znajdowały się u góry; na zrzucie ekranu systemu Android znajdują się w Centrum; i na zrzucie ekranu platformy uniwersalnej systemu Windows, są one u dołu. Wiersze mogą również rozmieszczonych w na różne sposoby:

[![Na stronie eksperymentu: Wyrównanie zawartości 2](flex-layout-images/ExperimentAlignContent2.png "strona Experiment - Align zawartości 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent` Nie obowiązuje, gdy istnieje tylko jeden wiersz lub kolumnę.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>Dołączone właściwości możliwej do wiązania szczegółowo

`FlexLayout` Definiuje pięciu dołączonych właściwości możliwej do wiązania. Te właściwości są ustawione na elementy podrzędne `FlexLayout` i odnoszą się tylko do tego określonego elementu podrzędnego.

<a name="align-self" />

### <a name="the-alignself-property"></a>Właściwość AlignSelf

[ `AlignSelf` ](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) Dołączonych właściwości możliwej do wiązania jest typu [ `FlexAlignSelf` ](xref:Xamarin.Forms.FlexAlignContent), wyliczenie z pięciu członków:

- `Auto`, wartość domyślna
- `Stretch`
- `Center`
- `Start` (lub "flex-start" w XAML)
- `End` (lub "flex-end" w XAML)

Dla wszystkich poszczególnych podrzędnych z `FlexLayout`, ta właściwość ustawiania zastąpień [ `AlignItems` ](#align-items) właściwość ustawioną na `FlexLayout` sam. Domyślne ustawienie `Auto` oznacza korzystanie z `AlignItems` ustawienie.

Dla `Label` elementu o nazwie `label` (lub przykład), możesz ustawić `AlignSelf` właściwości w kodzie w następujący sposób:

```csharp
FlexLayout.SetAlignSelf(label, FlexAlignSelf.Center);
```

Należy zauważyć, że nie ma żadnego odwołania do `FlexLayout` nadrzędnym `Label`. W XAML można ustawić właściwości następująco:

```xaml
<Label ... FlexLayout.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Właściwości Order

[ `Order` ](xref:Xamarin.Forms.FlexLayout.OrderProperty) Właściwość jest typu `int`. Wartość domyślna to 0.

`Order` Właściwości umożliwia zmianę kolejności, elementy podrzędne `FlexLayout` są uporządkowane. Zazwyczaj elementy podrzędne `FlexLayout` ułożone jest tej samej kolejności, w jakiej występują w `Children` kolekcji. Możesz zmienić tę kolejność, ustawiając `Order` dołączona właściwość może być powiązana z wartością całkowitą od zera na jeden lub więcej elementów podrzędnych. `FlexLayout` Następnie rozmieszcza jego elementy podrzędne, w oparciu o ustawienia `Order` właściwość w każdej podrzędnej, ale elementów podrzędnych o takim samym `Order` ustawienie ułożone w kolejności, w jakiej występują w `Children` kolekcji.

### <a name="the-basis-property"></a>Właściwość podstawy

[ `Basis` ](xref:Xamarin.Forms.FlexLayout.BasisProperty) Dołączonych właściwości możliwej do wiązania wskazuje ilość miejsca, który jest przydzielony do elementu podrzędnego `FlexLayout` na osi głównej. Określonego rozmiaru przez `Basis` właściwość jest rozmiar wzdłuż osi głównej nadrzędnej `FlexLayout`. W związku z tym `Basis` Określa szerokość elementu podrzędnego, gdy elementy podrzędne są ułożone w wierszach lub wysokość, gdy elementy podrzędne są ułożone w kolumnach.

`Basis` Właściwość jest typu [ `FlexBasis` ](xref:Xamarin.Forms.FlexBasis), strukturę. Można określić rozmiar w jednostkach niezależnych od urządzenia, albo lub jako procent rozmiaru `FlexLayout`. Wartość domyślna `Basis` właściwość jest właściwością statyczne `FlexBasis.Auto`, co oznacza, że elementem podrzędnym żądanej przez szerokość lub wysokość jest używany.

W kodzie, można ustawić `Basis` właściwość `Label` o nazwie `label` 40 jednostek niezależnych od urządzenia następująco:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

Drugi argument `FlexBasis` nosi nazwę Konstruktor `isRelative` i wskazuje, czy rozmiar jest względna (`true`) lub bezwzględną (`false`). Argument ma wartość domyślną `false`, dzięki czemu można również użyć następującego kodu:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Niejawna konwersja z `float` do `FlexBasis` jest zdefiniowana, więc można ją jeszcze bardziej ułatwić:

```csharp
FlexLayout.SetBasis(label, 40);
```

Można ustawić rozmiar do 25% `FlexLayout` nadrzędnego następująco:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

Część ułamkowa musi należeć do zakresu od 0 do 1.

W XAML można użyć numeru, rozmiar w jednostkach niezależnych od urządzenia:

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

Lub można określić wartość procentową z zakresu od 0 do 100%:

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

**Eksperymentować podstawy** strony **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** próbki pozwala na eksperymentowanie z `Basis` właściwości. Zostanie wyświetlona strona opakowana kolumny z pięciu `Label` elementy ze zmieniającymi się kolory pierwszego planu i tła. Dwa `Slider` elementy umożliwiają określenie `Basis` wartości drugiego i czwartego `Label`:

[![Podstawa eksperymentować strony](flex-layout-images/BasisExperiment.png "podstawę eksperymentować strony")](flex-layout-images/BasisExperiment-Large.png#lightbox)

Zrzut ekranu z systemem iOS, po lewej stronie pokazuje dwa `Label` elementy są podane wysokości w jednostkach niezależnych od urządzenia. Android ekran pokazuje podane są wysokości, które stanowi ułamek całkowita wysokość `FlexLayout`. Jeśli `Basis` równego 100%, a następnie element podrzędny jest wysokość `FlexLayout`i będzie zawijany do następnej kolumny, a zajmować cały wysokość tej kolumny, tak jak pokazano na zrzucie ekranu platformy uniwersalnej systemu Windows: wygląda tak, jakby pięć elementy podrzędne są rozmieszczane w wierszu , ale są faktycznie ułożone w pięciu kolumnach.

### <a name="the-grow-property"></a>Rozwijaj właściwości

[ `Grow` ](xref:Xamarin.Forms.FlexLayout.GrowProperty) Dołączonych właściwości możliwej do wiązania jest typu `int`. Wartość domyślna to 0, a wartość musi być większa lub równa 0.

`Grow` Właściwość odgrywa rolę, podczas gdy `Wrap` właściwość jest ustawiona na `NoWrap` i wiersz elementów podrzędnych ma łączna szerokość mniejsza niż szerokość `FlexLayout`, lub kolumnie elementów podrzędnych ma wysokości krótszy niż `FlexLayout`. `Grow` Właściwość wskazuje, jak rozdzielić pozostałe miejsce między elementami podrzędnymi.

W **rozwój eksperymentu** stronie pięciu `Label` elementów systemu przemiennej kolory są rozmieszczone w kolumnie, a dwa `Slider` elementy umożliwiają dostosowanie `Grow` właściwość drugiego i czwartego `Label`. Zrzut ekranu z systemem iOS na lewo przedstawiono domyślne `Grow` właściwości 0:

[![Na stronie eksperymentu zwiększania](flex-layout-images/GrowExperiment.png "na stronie eksperymentu rozwoju")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Jeśli wszystkie jeden element podrzędny jest dodatnią `Grow` wartości, a następnie ten podrzędny zajmuje całe pozostałe miejsce, tak jak pokazano na zrzucie ekranu systemu Android. Tego obszaru można przypisać w taki sposób, przez co najmniej dwa elementy podrzędne. Na zrzucie ekranu platformy uniwersalnej systemu Windows `Grow` właściwość drugiej `Label` wynosi 0,5, podczas `Grow` właściwość czwarty `Label` jest w wersji 1.5, co daje czwarty `Label` trzykrotnie większe pozostawieniu miejsce jako drugi `Label`.

Jak widok podrzędny korzysta z tego miejsca zależy od określonego typu elementu podrzędnego. Aby uzyskać `Label`, tekst może być umieszczony w obrębie łączny rozmiar `Label` przy użyciu właściwości `HorizontalTextAlignment` i `VerticalTextAlignment`.

<a name="shrink" />

### <a name="the-shrink-property"></a>Właściwość zmniejszania

[ `Shrink` ](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) Dołączonych właściwości możliwej do wiązania jest typu `int`. Wartość domyślna to 1, a wartość musi być większa lub równa 0.

`Shrink` Właściwość odgrywa rolę podczas `Wrap` właściwość jest ustawiona na `NoWrap` , a łączna szerokość wierszy elementów podrzędnych jest większa niż szerokość `FlexLayout`, lub agregacji wysokość elementów podrzędnych pojedynczej kolumny jest większa niż wysokość `FlexLayout`. Zwykle `FlexLayout` wyświetli te elementy podrzędne, constricting ich rozmiary. `Shrink` Właściwości można wskazać podrzędne, które otrzymują odpowiedni priorytet w są wyświetlane w ich pełną rozmiarów.

**Zmniejszania eksperymentu** tworzy stronę `FlexLayout` z pojedynczy wiersz z pięciu `Label` elementy podrzędne, które wymagają więcej miejsca niż `FlexLayout` szerokości. Zrzut ekranu z systemem iOS, po lewej stronie pokazuje wszystkie `Label` elementy z wartościami domyślnymi 1:

[![Zmniejszania eksperymentować strony](flex-layout-images/ShrinkExperiment.png "zmniejszania eksperymentować strony")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Na zrzucie ekranu systemu Android `Shrink` wartość dla drugiego `Label` jest równa 0 i który `Label` są wyświetlane w jej pełnej szerokości. Ponadto czwarty `Label` otrzymuje `Shrink` wartość większa niż jeden, a jego został zmniejszony. Zrzut ekranu platformy uniwersalnej systemu Windows zawiera zarówno `Label` elementy podane są `Shrink` wartość 0, aby umożliwić im mają być wyświetlane w ich pełnym rozmiarze, czy jest możliwe.

Można ustawić zarówno `Grow` i `Shrink` wartości do uwzględnienia w sytuacjach, gdy rozmiarów agregacji podrzędnych może czasami mniejsze niż lub czasami jest większy niż rozmiar `FlexLayout`.

## <a name="css-styling-with-flexlayout"></a>Style CSS przy użyciu FlexLayout

Możesz użyć [style CSS](~/xamarin-forms/user-interface/styles/css/index.md) funkcja wprowadzona w systemie Xamarin.Forms 3.0 w połączeniu z `FlexLayout`. **Elementów katalogu CSS** strony **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** przykładowe duplikuje układ **elementów katalogu** strony, ale z CSS Arkusz stylów dla wielu style:

[![Strona elementów wykazu z CSS](flex-layout-images/CssCatalogItems.png "CSS w katalogu elementów strony")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

Oryginalny **CatalogItemsPage.xaml** plik ma pięć `Style` definicje w jego `Resources` sekcję 15 `Setter` obiektów. W **CssCatalogItemsPage.xaml** pliku, który został zmniejszony do dwóch `Style` definicje z tylko cztery `Setter` obiektów. Style te stanowią uzupełnienie arkusza stylów CSS dla właściwości, które funkcja stylów Xamarin.Forms CSS obecnie nie obsługuje:

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

W pierwszym wierszu metody odwołuje się do arkusza stylów CSS `Resources` sekcji:

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

Zwróć uwagę, że zawierają dwa elementy w każdym z trzech elementów `StyleClass` ustawienia:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

Te dotyczą selektory w **CatalogItemsStyles.css** arkusza stylów:

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

Kilka `FlexLayout` właściwości możliwej do wiązania dołączone do których istnieją odwołania w tym miejscu. W `label.empty` selektor, zobaczysz `flex-grow` atrybut, który style pustą `Label` zapewnienie puste miejsce powyżej `Button`. `image` Zawiera selektor `order` atrybutu i `align-self` atrybutów, które odpowiadają `FlexLayout` dołączonych właściwości możliwej do wiązania.

W tym samouczku właściwości można ustawić bezpośrednio na `FlexLayout` i dołączonych właściwości możliwej do wiązania można ustawić na elementy podrzędne `FlexLayout`. Lub możesz ustawić te właściwości, które pośrednio przy użyciu tradycyjnych style oparte na XAML lub style CSS. Ważne jest wiedzieć i zrozumienie tych właściwości. Te właściwości są, co sprawia, że `FlexLayout` prawdziwie elastyczna. 

## <a name="flexlayout-with-xamarinuniversity"></a>FlexLayout z Xamarin.University

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Produkt Xamarin.Forms 3.0 Flex układu przez [platformy Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Linki pokrewne

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
