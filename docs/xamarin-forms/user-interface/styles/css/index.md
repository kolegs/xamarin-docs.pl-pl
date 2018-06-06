---
title: Ustawianie stylów aplikacji platformy Xamarin.Forms przy użyciu kaskadowych arkuszy stylów (CSS)
description: Platformy Xamarin.Forms obsługuje elementy wizualne stylów przy użyciu kaskadowych arkuszy stylów (CSS).
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 76ca67f7ac8a8e27e5f502455d48874c775fc172
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794088"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>Ustawianie stylów aplikacji platformy Xamarin.Forms przy użyciu kaskadowych arkuszy stylów (CSS)

_Platformy Xamarin.Forms obsługuje elementy wizualne stylów przy użyciu kaskadowych arkuszy stylów (CSS)._

Platformy Xamarin.Forms 3.0 wprowadzono możliwość określenia stylu aplikacji za pomocą CSS. Arkusz stylów zawiera listę reguł z składające się z co najmniej jeden selektorów i blok deklaracji. Blok deklaracja zawiera listę deklaracji w nawiasach klamrowych, każdy deklaracją składające się z właściwością, dwukropek i wartości. W przypadku wielu deklaracji w bloku jako separator dodaje się średnikiem. Poniższy przykładowy kod przedstawia niektóre platformy Xamarin.Forms CSS zgodnych:

```css
^contentpage {
    background-color: lightgray;
}

#listView {
    background-color: lightgray;
}

stacklayout {
    margin: 20;
}

.mainPageTitle {
    font-style: bold;
    font-size: medium;
}

.mainPageSubtitle {
    margin-top: 15;
}

.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}

listview image {
    height: 60;
    width: 60;
}

stacklayout>image {
    height: 200;
    width: 200;
}
```

W platformy Xamarin.Forms arkusze stylów CSS są analizowane i oceniane w środowiska uruchomieniowego, a nie w czasie kompilacji i są ponownie przeanalizowany przy użyciu arkuszy stylów.

> [!NOTE]
> Obecnie wszystkie style, które jest możliwe za pomocą stylów XAML nie można wykonać z CSS. Jednak style XAML może służyć jako uzupełnienie CSS dla właściwości, które nie są obecnie obsługiwane przez platformy Xamarin.Forms. Aby uzyskać więcej informacji na temat style XAML, zobacz [style aplikacji platformy Xamarin.Forms przy użyciu stylów XAML](~/xamarin-forms/user-interface/styles/xaml/index.md).

[MonkeyAppCSS](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/) przykładowych pokazano, za pomocą CSS do określania stylu prostej aplikacji i jest wyświetlany w poniższe zrzuty ekranu:

[![Strona główna MonkeyApp z stylów CSS](css-images/MonkeyAppMainPage.png "MonkeyApp strona główna z stylów CSS")](css-images/MonkeyAppMainPage-Large.png#lightbox "MonkeyApp strona główna z stylów CSS")

[![Strona szczegółów MonkeyApp ze stylów CSS](css-images/MonkeyAppDetailPage.png "MonkeyApp strony szczegółów z stylów CSS")](css-images/MonkeyAppDetailPage-Large.png#lightbox "MonkeyApp strony szczegółów z stylów CSS")

> [!NOTE]
> Nie jest obecnie możliwe do nadawania stylu kolor tła [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) przy użyciu arkusza stylów. W związku z tym w przykładowej aplikacji [ `NavigationPage.BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) właściwość jest ustawiona w kodzie.

## <a name="consuming-a-style-sheet"></a>Korzystanie z arkusza stylów

Proces dodawania arkusza stylów do rozwiązania wygląda następująco:

1. Dodaj pusty plik CSS do projektu biblioteki .NET Standard.
1. Ustawić akcji kompilacji w pliku CSS do **EmbeddedResource**.

### <a name="loading-a-style-sheet"></a>Podczas ładowania arkusza stylów

Istnieje kilka metod, które mogą służyć do załadowania arkusza stylów.

### <a name="xaml"></a>XAML

Arkusz stylów można załadować i przeanalizować z [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) klasy przed dodaniem go do [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) strony:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    ...
</ContentPage>
```

[ `StyleSheet.Source` ](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) Właściwość określa arkusz stylów jako identyfikatora URI względną wobec lokalizacji pliku otaczającego XAML lub względem katalogu głównego projektu, jeśli identyfikator URI, który rozpoczyna się od `/`.

> [!WARNING]
> Plik kodu CSS nie będzie można załadować, jeśli jest akcja kompilacji nie jest ustawiona na **EmbeddedResource**.

Alternatywnie można załadowaniu i przeanalizowaniu z arkusza stylów [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) klasy przez ze śródwierszowaniem w `CDATA` sekcji:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet>
            <![CDATA[
            ^contentpage {
                background-color: lightgray;
            }
            ]]>
        </StyleSheet>
    </ContentPage.Resources>
    ...
</ContentPage>
```

### <a name="c"></a>C#

W języku C# arkusz stylów może być załadowany jako osadzony zasób i dodawany do [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) strony:

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        this.Resources.Add(StyleSheet.FromAssemblyResource(
            IntrospectionExtensions.GetTypeInfo(typeof(MyPage)).Assembly,
            "MyProject.Assets.styles.css"));
    }
}
```

Pierwszy argument `StyleSheet.FromAssemblyResource` metoda jest zestaw zawierający arkusz stylów, a drugi argument jest `string` reprezentujący identyfikator zasobu. Identyfikator zasobu można uzyskać z **właściwości** okna w przypadku wybrania pliku CSS.

Alternatywnie arkusza stylów mogą być ładowane z `StringReader` i dodane do [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) strony:

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        using (var reader = new StringReader("^contentpage { background-color: lightgray; }"))
        {
            this.Resources.Add(StyleSheet.FromReader(reader));
        }
    }
}
```

Argument `StyleSheet.FromReader` jest metoda `TextReader` który zapoznał arkusza stylów.

## <a name="selecting-elements-and-applying-properties"></a>Zaznaczanie elementów i stosowanie właściwości

CSS używa selektorów, aby określić, które elementy pod kątem. Style z pasującymi selektory są stosowane po kolei, w kolejności definicji. Style zdefiniowane dla określonego elementu są zawsze stosowane na końcu. Aby uzyskać więcej informacji o obsługiwanych selektorów, zobacz [odwołania selektora](#selector-reference).

CSS używa właściwości stylu wybranego elementu. Każda właściwość ma zestaw możliwych wartości, a niektóre właściwości mogą wpłynąć na dowolnego typu elementu, podczas gdy inne dotyczą grupy elementów. Aby uzyskać więcej informacji o obsługiwanych właściwości, zobacz [odwołania do właściwości](#property-reference).

### <a name="selecting-elements-by-type"></a>Zaznaczanie elementów według typu

Elementy w drzewie wizualnym można wybrać typu z wielkich liter `element` selektora:

```css
stacklayout {
    margin: 20;
}
```

Ten selektor identyfikuje wszelkie [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) elementy na stronach, które zużywają arkusza stylów i ustawia ich marginesy uniform grubości 20.

> [!NOTE]
> `element` Selektor nie będzie rozpoznawał podklas określonego typu.

### <a name="selecting-elements-by-base-class"></a>Zaznaczanie elementów przez klasę podstawową

Elementy w drzewie wizualnym można wybrać przez klasę podstawową z wielkich liter `^base` selektora:

```css
^contentpage {
    background-color: lightgray;
}
```

Ten selektor identyfikuje wszelkie [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) kolor elementów, które zużywają arkusza stylów i ustawia ich tło `lightgray`.

> [!NOTE]
> `^base` Selektora są specyficzne dla platformy Xamarin.Forms i nie jest częścią specyfikacji CSS.

### <a name="selecting-an-element-by-name"></a>Wybranie elementu według nazwy

Można wybrać poszczególne elementy w drzewie wizualnym z uwzględnieniem wielkości liter `#id` selektora:

```css
#listView {
    background-color: lightgray;
}
```

Ten selektor identyfikuje element których [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) właściwość jest ustawiona na `listView`. Jednak jeśli `StyleId` właściwość nie jest ustawiona, selektor będzie wrócić korzystania z `x:Name` elementu. W związku z tym w poniższym przykładzie XAML `#listView` określi selektora [ `ListView` ](xref:Xamarin.Forms.ListView) których `x:Name` atrybut ma ustawioną `listView`i ustawi kolor tła jego `lightgray`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView x:Name="listView" ...>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

### <a name="selecting-elements-with-a-specific-class-attribute"></a>Wybieranie elementów z atrybutem określonej klasy

Można wybrać elementy z atrybutem określonej klasy z uwzględnieniem wielkości liter `.class` selektora:

```css
.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}
```

Ustawiając można przypisać do elementu XAML klasy CSS [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) właściwość elementu Nazwa klasy CSS. W związku z tym w poniższym przykładzie XAML style zdefiniowane przez `.detailPageTitle` klasy są przypisane do pierwszej [ `Label` ](xref:Xamarin.Forms.Label), podczas gdy style zdefiniowane przez `.detailPageSubtitle` klasy są przypisane do drugiego `Label`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            <Label ... StyleClass="detailPageTitle" />
            <Label ... StyleClass="detailPageSubtitle"/>
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

### <a name="selecting-child-elements"></a>Wybieranie elementów podrzędnych

Elementy podrzędne w drzewie wizualnym można wybrać z wielkich liter `element element` selektora:

```css
listview image {
    height: 60;
    width: 60;
}
```

Ten selektor identyfikuje wszelkie [ `Image` ](xref:Xamarin.Forms.Image) elementy, które są elementami podrzędnymi [ `ListView` ](xref:Xamarin.Forms.ListView) elementów i ustawia szerokość i wysokość do 60. W związku z tym w poniższym przykładzie XAML `listview image` określi selektora [ `Image` ](xref:Xamarin.Forms.Image) jest elementem podrzędnym [ `ListView` ](xref:Xamarin.Forms.ListView)i ustawia jej wysokości i szerokości do 60.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView ...>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid>
                            ...
                            <Image ... />
                            ...
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> `element element` Selektor nie wymaga elementu podrzędnego _bezpośredniego_ element podrzędny elementu nadrzędnego — element podrzędny może mieć innego elementu nadrzędnego. Wybór występuje pod warunkiem, że określony pierwszy element jest elementem nadrzędnym.

### <a name="selecting-direct-child-elements"></a>Wybieranie bezpośrednimi elementami podrzędnymi

Bezpośrednie elementy podrzędne w drzewie wizualnym można wybrać z wielkich liter `element>element` selektora:

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

Ten selektor identyfikuje wszelkie [ `Image` ](xref:Xamarin.Forms.Image) elementy, które są bezpośrednimi elementami podrzędnymi [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) elementów i ustawia szerokość i wysokość 200. W związku z tym w poniższym przykładzie XAML `stacklayout>image` określi selektora [ `Image` ](xref:Xamarin.Forms.Image) jest bezpośrednim elementem podrzędnym elementu [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)i ustawia jej wysokości i szerokości 200.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            ...
            <Image ... />
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

> [!NOTE]
> `element>element` Selektora wymaga elementu podrzędnego _bezpośredniego_ element podrzędny elementu nadrzędnego.

## <a name="selector-reference"></a>Selektor odwołania

Następujące selektory CSS są obsługiwane przez platformy Xamarin.Forms:

|Selektor|Przykład|Opis|
|---|---|---|
|`.class`|`.header`|Wybiera wszystkie elementy z `StyleClass` Właściwość zawierająca "nagłówka". Należy pamiętać, że ten selektor uwzględniana jest wielkość liter.|
|`#id`|`#email`|Wybiera wszystkie elementy z `StyleId` ustawioną `email`. Jeśli `StyleId` nie jest ustawiona, jest powrót do `x:Name`. Przy użyciu języka XAML, `x:Name` jest preferowana przez `StyleId`. Należy pamiętać, że ten selektor uwzględniana jest wielkość liter.|
|`*`|`*`|Wybiera wszystkie elementy.|
|`element`|`label`|Wybiera wszystkie elementy typu `Label`, ale częściowej klasy nie. Należy pamiętać, że ten selektor bez uwzględniania wielkości liter.|
|`^base`|`^contentpage`|Wybiera wszystkie elementy z `ContentPage` jako klasy podstawowej, w tym `ContentPage` samej siebie. Należy zauważyć, że ten selektor bez uwzględniania wielkości liter i nie jest częścią specyfikacji CSS.|
|`element,element`|`label,button`|Wybiera wszystkie `Button` elementów i wszystkie `Label` elementy. Należy pamiętać, że ten selektor bez uwzględniania wielkości liter.|
|`element element`|`stacklayout label`|Wybiera wszystkie `Label` elementy wewnątrz `StackLayout`. Należy pamiętać, że ten selektor bez uwzględniania wielkości liter.|
|`element>element`|`stacklayout>label`|Wybiera wszystkie `Label` elementy z `StackLayout` jako bezpośrednią lokacją nadrzędną. Należy pamiętać, że ten selektor bez uwzględniania wielkości liter.|
|`element+element`|`label+entry`|Wybiera wszystkie `Entry` elementy bezpośrednio po `Label`. Należy pamiętać, że ten selektor bez uwzględniania wielkości liter.|
|`element~element`|`label~entry`|Wybiera wszystkie `Entry` poprzedzone elementy `Label`. Należy pamiętać, że ten selektor bez uwzględniania wielkości liter.|

Style z pasującymi selektory są stosowane po kolei, w kolejności definicji. Style zdefiniowane dla określonego elementu są zawsze stosowane na końcu.

> [!TIP]
> Selektory można łączyć bez ograniczeń, takich jak `StackLayout>ContentView>label.email`.

Następujące selektory są obecnie obsługiwane:

- `[attribute]`
- `@media` I `@supports`
- `:` I `::`

> [!NOTE]
> Właściwości i zastąpienia szczegółowością nie są obsługiwane.

## <a name="property-reference"></a>Odwołania do właściwości

Następujące właściwości CSS są obsługiwane przez platformy Xamarin.Forms (w **wartości** kolumny, typy są _kursywą_, gdy są literały ciągu `gray`):

|Właściwość|Informacje zawarte w tym artykule dotyczą|Wartości|Przykład|
|---|---|---|---|
|`background-color`|`VisualElement`|_Kolor_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_Ciąg_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`|_Kolor_ \| `initial`|`border-color: #9acd32;`|
|`border-width`|`Button`|_O podwójnej precyzji_ \| `initial` |`border-width: .5;`|
|`color`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`|_Kolor_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Ciąg_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Podwójna_ \| _namedsize_ \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_O podwójnej precyzji_ \| `initial` |`min-height: 250;`|
|`margin`|`View`|_Grubość_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_Grubość_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_Grubość_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_Grubość_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_Grubość_ \| `initial` |`margin-bottom: 6;`|
|`min-height`|`VisualElement`|_O podwójnej precyzji_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_O podwójnej precyzji_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_O podwójnej precyzji_ \| `initial` |`opacity: .3;`|
|`padding`|`Layout`, `Page`|_Grubość_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Layout`, `Page`|_O podwójnej precyzji_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Layout`, `Page`| _O podwójnej precyzji_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Layout`, `Page`| _O podwójnej precyzji_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Layout`, `Page`| _O podwójnej precyzji_ \| `initial` |`padding-bottom: 6;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `right` \| `center` \| `start` \| `end` \| `initial`. `left` i `right` należy unikać w środowiskach od prawej do lewej.| `text-align: right;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial `|`visibility: hidden;`|
|`width`|`VisualElement`|_O podwójnej precyzji_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` jest prawidłową wartością dla wszystkich właściwości. Czyści wartość (Resetowanie do ustawień domyślnych), która została ustawiona w innym stylu.

Obecnie obsługiwane są następujące właściwości:

- `all: initial`.
- Właściwości układu (pole lub siatki).
- Skrótowa właściwości, takie jak `font`, i `border`.

Ponadto istnieje nie `inherit` dziedziczenie wartości i dlatego nie jest obsługiwane. W związku z tym nie można na przykład ustawić `font-size` właściwości układ i oczekują wszystkie [ `Label` ](xref:Xamarin.Forms.Label) wystąpień w układzie dziedziczona wartością. Jedynym wyjątkiem jest `direction` właściwość, która ma wartość domyślną z `inherit`.

### <a name="color"></a>Kolor

Następujące `color` wartości są obsługiwane:

- `X11` [kolory](https://en.wikipedia.org/wiki/X11_color_names/), która odpowiada kolorów CSS, wstępnie zdefiniowane kolory platformy uniwersalnej systemu Windows i platformy Xamarin.Forms kolorów. Należy zauważyć, że te wartości kolorów bez uwzględniania wielkości liter.
- liczba szesnastkowa kolorów: `#rgb`, `#argb`, `#rrggbb`, `#aarrggbb`
- kolorów RGB: `rgb(255,0,0)`, `rgb(100%,0%,0%)`. Wartości są w zakresie od 0 do 255 lub 0 do 100%.
- kolory rgba: `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`. Wartość nieprzezroczystości jest w zakresie od 0,0 1.0.
- kolory HSL: `hsl(120, 100%, 50%)`. Wartość h jest z zakresu 0-360, a s i l w zakresie 0-100%.
- kolory hsla: `hsla(120, 100%, 50%, .8)`. Wartość nieprzezroczystości jest w zakresie od 0,0 1.0.

### <a name="thickness"></a>Grubość

Jeden, dwóch, trzech lub czterech `thickness` wartości są obsługiwane, oddzielonych biały znak:

- Pojedynczą wartość wskazuje uniform grubości.
- Dwie wartości wskazują grubość pionowych, a następnie poziomej.
- Trzy wartości wskazują górnej części poziomym (lewy i prawy), a następnie grubość dolnej.
- Cztery wartości oznaczają u góry, a następnie po prawej, dolnej, a następnie grubość po lewej stronie.

> [!NOTE]
> CSS `thickness` wartości różnią się od XAML [ `Thickness` ](/api/type/Xamarin.Forms.Thickness/) wartości. Na przykład w języku XAML wartość dwóch `Thickness` wskazuje grubość poziomych, a następnie pionowy, podczas wartość czterech `Thickness` wskazuje po lewej stronie góry, a następnie po prawej, dolnej grubości. Ponadto XAML `Thickness` wartości są rozdzielane przecinkami.

### <a name="namedsize"></a>NamedSize

Następujące bez uwzględniania wielkości liter `namedsize` wartości są obsługiwane:

- `default`
- `micro`
- `small`
- `medium`
- `large`

Dokładne znaczenie `namedsize` wartość jest zależny od platformy i zależne od widoku.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>CSS w platformy Xamarin.Forms z Xamarin.University

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Platformy Xamarin.Forms 3.0 CSS, przez [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Linki pokrewne

- [MonkeyAppCSS (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)
- [Określanie stylów aplikacji Xamarin.Forms przy użyciu stylów XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
