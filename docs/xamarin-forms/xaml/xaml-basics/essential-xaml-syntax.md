---
title: "Część 2. Składnia podstawowych języka XAML"
description: "XAML przede wszystkim jest przeznaczony do tworzenia wystąpienia i Inicjowanie obiektów. Ale często złożonych obiektów, które łatwo nie może być reprezentowany jako ciągi XML musi mieć wartość właściwości, a czasami należy ustawić właściwości zdefiniowane przez klasę jednej klasy podrzędnej. Te dwie potrzeba podstawowych języka XAML składni właściwości elementów właściwości i dołączone właściwości."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 77ed7c49a901a877d822c2274263bcb8dbe19ac6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="part-2-essential-xaml-syntax"></a>Część 2. Składnia podstawowych języka XAML

_XAML przede wszystkim jest przeznaczony do tworzenia wystąpienia i Inicjowanie obiektów. Ale często złożonych obiektów, które łatwo nie może być reprezentowany jako ciągi XML musi mieć wartość właściwości, a czasami należy ustawić właściwości zdefiniowane przez klasę jednej klasy podrzędnej. Te dwie potrzeba podstawowych języka XAML składni właściwości elementów właściwości i dołączone właściwości._

## <a name="property-elements"></a>Właściwości elementów

W języku XAML jako atrybuty XML zwykle są ustawione właściwości klasy:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

Istnieje jednak alternatywny sposób ustawiania właściwości w języku XAML. Aby wypróbować to alternatywne z `TextColor`, najpierw usuń istniejącą `TextColor` ustawienia:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

Otwórz pusty element `Label` tag, dzieląc go na tagiem początkowym i końcowym:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

W ramach tych tagów Dodaj tagiem początkowym i końcowym, które składają się z nazwy klasy i nazwę właściwości oddzielona kropką:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

Ustaw wartość właściwości jako zawartość tych nowych tagów następująco:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

Te dwa sposoby określania `TextColor` właściwości są taką samą funkcję, ale nie używaj dwa sposoby dla tej właściwości, ponieważ będzie efektywnie się ustawienie właściwości dwa razy, a może być niejednoznaczna.

Przy użyciu tej nowej składni należy wprowadzić niektóre przydatne terminologią:

-  `Label` jest *object element*. Jest to obiekt platformy Xamarin.Forms wyrażonych jako XML element.
-  `Text`, `VerticalOptions`, `FontAttributes` i `FontSize` są *atrybuty właściwości*. Są one właściwości platformy Xamarin.Forms wyrażonych jako atrybuty XML.
-  W tym ostatnim fragment `TextColor` stał się *elementu właściwości*. Jest to właściwość platformy Xamarin.Forms, ale teraz jest elementem XML.


Definicja właściwości, które elementy mogą w najpierw wydają się być naruszenie składni XML, ale nie jest. Okres nie ma specjalnego znaczenia w kodzie XML. Do dekodera XML `Label.TextColor` jest po prostu elementu podrzędnego normalnego.

W języku XAML jednak ta składnia jest bardzo specjalnych. Jedna z reguł dla elementów właściwości jest nic może występować w `Label.TextColor` tagu. Wartość właściwości zawsze jest definiowany jako zawartości między początkową elementu właściwości i tagami końcowymi.

Możesz użyć składni elementu właściwości w więcej niż jedną właściwość:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center">
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

Lub za pomocą składni elementu właściwości dla wszystkich właściwości:

```xaml
<Label>
    <Label.Text>
        Hello, XAML!
    </Label.Text>
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
    <Label.VerticalOptions>
        Center
    </Label.VerticalOptions>
</Label>
```

Na początku składni elementu właściwości może się wydawać niepotrzebnych long-winded zastąpienia dla elementu stosunkowo prosta, a w tych przykładach, który na pewno jest wielkość liter.

Jednak składni elementu właściwości staje się istotny, gdy wartość właściwości jest zbyt złożone, aby być wyrażone jako prosty ciąg. W tagach właściwości elementu można utworzyć wystąpienia obiektu innego i ustawienia swoich właściwości. Na przykład można jawnie ustawić właściwość takich jak `VerticalOptions` do `LayoutOptions` wartości ustawienia właściwości:

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

Inny przykład: `Grid` ma dwie właściwości o nazwie `RowDefinitions` i `ColumnDefinitions`. Te dwie właściwości są typu `RowDefinitionCollection` i `ColumnDefinitionCollection`, które są kolekcjami elementów `RowDefinition` i `ColumnDefinition` obiektów. Należy użyć składni elementu właściwości można ustawić kolekcjach.

W tym miejscu jest na początku pliku XAML dla `GridDemoPage` klasy przedstawiający tagów element właściwości dla `RowDefinitions` i `ColumnDefinitions` kolekcje:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

Zwróć uwagę, skróconej składni służące do definiowania o rozmiarze automatycznie komórek komórek pikseli szerokości i wysokości i ustawień gwiazdki.

## <a name="attached-properties"></a>Dołączone właściwości

Właśnie w tym samouczku który `Grid` wymaga elementy na stronie właściwości `RowDefinitions` i `ColumnDefinitions` kolekcje w celu zdefiniowania wierszy i kolumn. Jednak musi również istnieć niewłaściwy dla programisty wskazać, wierszy i kolumn gdzie poszczególne elementy podrzędne elementu `Grid` znajduje się.

W tagu dla każdego elementu podrzędnego elementu `Grid` Określ wiersz i kolumnę tego elementu podrzędnego przy użyciu następujących atrybutów:

-  `Grid.Row`
-  `Grid.Column`

Domyślne wartości tych atrybutów to 0. Można również określić, czy element podrzędny obejmuje więcej niż jeden wiersz lub kolumnę z tymi atrybutami:

-  `Grid.RowSpan`
-  `Grid.ColumnSpan`

Te atrybuty mają przypisane wartości domyślne 1.

W tym miejscu jest pełny plik GridDemoPage.xaml:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>

        <Label Text="Autosized cell"
               Grid.Row="0" Grid.Column="0"
               TextColor="White"
               BackgroundColor="Blue" />

        <BoxView Color="Silver"
                 HeightRequest="0"
                 Grid.Row="0" Grid.Column="1" />

        <BoxView Color="Teal"
                 Grid.Row="1" Grid.Column="0" />

        <Label Text="Leftover space"
               Grid.Row="1" Grid.Column="1"
               TextColor="Purple"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two rows (or more if you want)"
               Grid.Row="0" Grid.Column="2" Grid.RowSpan="2"
               TextColor="Yellow"
               BackgroundColor="Blue"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two columns"
               Grid.Row="2" Grid.Column="0" Grid.ColumnSpan="2"
               TextColor="Blue"
               BackgroundColor="Yellow"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Fixed 100x100"
               Grid.Row="2" Grid.Column="2"
               TextColor="Aqua"
               BackgroundColor="Red"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

    </Grid>
</ContentPage>
```

`Grid.Row` i `Grid.Column` ustawienia 0 nie są wymagane, ale są zwykle dołączone do celów przejrzystości.

Oto, co prawdopodobnie na wszystkich platformach trzy:

[![](essential-xaml-syntax-images/griddemo.png "Układ siatki")](essential-xaml-syntax-images/griddemo-large.png#lightbox "siatki układu")

Oceny wyłącznie z składnię, te `Grid.Row`, `Grid.Column`, `Grid.RowSpan`, i `Grid.ColumnSpan` atrybuty wydają się być statycznego pola lub właściwości `Grid`, ale interesujące jest, że wystarczająco, `Grid` nie definiuje żadnych o nazwie `Row`, `Column`, `RowSpan`, lub `ColumnSpan`.

Zamiast tego `Grid` definiuje czterech właściwości o nazwie `RowProperty`, `ColumnProperty`, `RowSpanProperty`, i `ColumnSpanProperty`. Są to specjalne typy właściwości znany jako *dołączone właściwości*. Są one zdefiniowane przez `Grid` class, ale ustawiony na elementy podrzędne `Grid`.

Jeśli chcesz użyć tych dołączone właściwości w kodzie, `Grid` klasa dostarcza metody statycznej o nazwie `SetRow`, `GetColumn`, itd. W języku XAML, te dołączone właściwości są ustawione jako atrybuty elementów podrzędnych, ale `Grid` przy użyciu właściwości proste nazwy.

Dołączone właściwości są zawsze rozpoznawalną w plikach XAML jako atrybuty zawierająca klasy i nazwę właściwości oddzielone kropką. Są one nazywane *dołączone właściwości* ponieważ są one zdefiniowane przez jedną klasę (w tym przypadku `Grid`), ale dołączone do innych obiektów (w tym przypadku elementów podrzędnych `Grid`). W układzie `Grid` można przejrzeć wartości tych właściwości dołączonych wiedzieć, gdzie umieścić każdego elementu podrzędnego.

`AbsoluteLayout` Klasa definiuje dwie dołączone właściwości o nazwie `LayoutBounds` i `LayoutFlags`. Oto funkcji zmiany rozmiaru i wzorzec szachownicy realizowane przy użyciu proporcjonalne pozycjonowanie `AbsoluteLayout`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.AbsoluteDemoPage"
             Title="Absolute Demo Page">

    <AbsoluteLayout BackgroundColor="#FF8080">
        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

  </AbsoluteLayout>
</ContentPage>
```

A tutaj jest:

[![](essential-xaml-syntax-images/absolutedemo-large.png "Układ bezwzględną")](essential-xaml-syntax-images/absolutedemo-large.png#lightbox "układu bezwzględne")

Dla mniej więcej tak może pytanie mądry programu przy użyciu kodu XAML. Oczywiście powtarzania i prawidłowości `LayoutBounds` prostokąt sugeruje, że może go lepiej rozumieją w kodzie.

Na pewno jest uzasadnione dotyczą, a problem nie występuje równoważenia kodu i znaczników podczas definiowania interfejsów użytkownika. Jest łatwy do definiowania niektórych elementów wizualnych w języku XAML, a następnie dodać więcej elementów wizualnych, które może być lepiej generowane w pętli za pomocą konstruktora pliku CodeBehind.

## <a name="content-properties"></a>Właściwości zawartości

W poprzednich przykładach `StackLayout`, `Grid`, i `AbsoluteLayout` obiekty są ustawione na `Content` właściwość `ContentPage`, i elementy podrzędne tych układów są faktycznie elementów w `Children` kolekcji. Jeszcze tych `Content` i `Children` właściwości są nigdzie w pliku XAML.

Oczywiście można uwzględnić `Content` i `Children` właściwości jako właściwość elementy, takie jak w **XamlPlusCode** próbki:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout.Children>
                <Slider VerticalOptions="CenterAndExpand"
                        ValueChanged="OnSliderValueChanged" />

                <Label x:Name="valueLabel"
                       Text="A simple Label"
                       FontSize="Large"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand" />

                <Button Text="Click Me!"
                      HorizontalOptions="Center"
                      VerticalOptions="CenterAndExpand"
                      Clicked="OnButtonClicked" />
            </StackLayout.Children>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Jest rzeczywiste pytanie: Dlaczego są te elementy właściwości *nie* wymagany w pliku XAML?

Elementy zdefiniowane w platformy Xamarin.Forms do użycia w języku XAML mogą mieć jedną właściwość oflagowane w programie `ContentProperty` atrybut klasy. Jeśli możesz odszukać `ContentPage` klasy w dokumentacji platformy Xamarin.Forms, zobaczysz tego atrybutu:

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

Oznacza to, że `Content` znaczniki elementu właściwości nie są wymagane. Zawartość XML, która pojawia się między początkową i końcową `ContentPage` tagi przyjęto, że ma zostać przypisany do `Content` właściwości.

 `StackLayout`, `Grid`, `AbsoluteLayout`, i `RelativeLayout` wszystkie pochodzi od `Layout<View>`, i czy możesz odszukać `Layout<T>` w dokumentacji platformy Xamarin.Forms zobaczysz innego `ContentProperty` atrybutu:

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

Umożliwiająca zawartości układu były automatycznie dodawane do `Children` kolekcji bez jawnego `Children` znaczniki elementu właściwości.

Inne klasy również mieć `ContentProperty` atrybutu definicje. Na przykład właściwość content `Label` jest `Text`. Sprawdź w dokumentacji interfejsu API dla innych użytkowników.

## <a name="platform-differences-with-onplatform"></a>Różnice platformy z OnPlatform

W przypadku aplikacji jednej strony jest często można ustawić `Padding` właściwości na stronie, aby uniknąć zastępowania pasek stanu systemu iOS. W kodzie, można użyć `Device.RuntimePlatform` właściwości w tym celu:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

Można również wykonać podobny przy użyciu języka XAML `OnPlatform` i `On` klasy. Najpierw obejmują elementy na stronie właściwości `Padding` właściwość w górnej części strony:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

W ramach tych tagów uwzględnić `OnPlatform` tagu. `OnPlatform` jest klasą szablonową. Należy określić argument typu ogólnego, w tym przypadku `Thickness`, który jest typem `Padding` właściwości. Na szczęście Brak atrybutu XAML w szczególności, aby zdefiniować argumentów ogólnych o nazwie `x:TypeArguments`. Typ właściwości, które ustawiasz powinna odpowiadać:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">

        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`OnPlatform` ma właściwości o nazwie `Platforms` czyli `IList` z `On` obiektów. Użyj tagów element właściwości dla tej właściwości:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>

            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Teraz Dodaj `On` elementy. W przypadku każdego onem wartość `Platform` właściwości i `Value` właściwości kod znaczników dla `Thickness` właściwości:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>
                <On Platform="iOS" Value="0, 20, 0, 0" />
                <On Platform="Android" Value="0, 0, 0, 0" />
                <On Platform="UWP" Value="0, 0, 0, 0" />
            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Można uprościć tego znacznika. Właściwość content `OnPlatform` jest `Platforms`, więc można usunąć tych tagów elementu właściwości:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android" Value="0, 0, 0, 0" />
            <On Platform="UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`Platform` Właściwość `On` jest typu `IList<string>`, więc jeśli wartości są takie same, można dołączyć wielu platform:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android, UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Android i Windows są ustawione na wartość domyślną `Padding`, można usunąć tag:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Jest to standardowy sposób, aby ustawić zależny od platformy `Padding` właściwości w języku XAML. Jeśli `Value` ustawienie nie może być reprezentowany przez jednego ciągu, elementy właściwości można zdefiniować dla niego:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS">
                <On.Value>
                    0, 20, 0, 0
                </On.Value>
            </On>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

## <a name="summary"></a>Podsumowanie

Elementy właściwości i dołączone właściwości została ustanowiona znacznie podstawowa składnia języka XAML. Jednak czasami należy ustawić właściwości obiektów w sposób pośrednie, na przykład ze słownika zasobów. Takie podejście jest omówiona w następnej części, część [3. Rozszerzenia znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).



## <a name="related-links"></a>Linki pokrewne

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Part 1. Część 1. Wprowadzenie do języka XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Part 3. Część 3. Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Part 4. Część 4. Powiązania danych — podstawy](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Part 5. Z danych powiązanie z modelem MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
