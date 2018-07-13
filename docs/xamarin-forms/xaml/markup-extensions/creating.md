---
title: Tworzenie rozszerzeń struktury znaczników XAML
description: W tym artykule wyjaśniono, jak zdefiniować własne niestandardowe rozszerzenia znaczników w XAML zestawu narzędzi Xamarin.Forms. Rozszerzenie znaczników XAML jest klasa, która implementuje interfejs IMarkupExtension IMarkupExtension.
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: d4b3d5c65ddf8be433d1f8e182774aa839f60357
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995599"
---
# <a name="creating-xaml-markup-extensions"></a>Tworzenie rozszerzeń struktury znaczników XAML

Na poziomie programowy, rozszerzenie znaczników XAML jest klasa, która implementuje [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) lub [ `IMarkupExtension<T>` ](xref:Xamarin.Forms.Xaml.IMarkupExtension`1) interfejsu. Możesz zapoznać się z kodem źródłowym rozszerzenia standardowych znaczników opisane poniżej w [ **wyrażeń MarkupExtension** katalogu](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) repozytorium GitHub platformy Xamarin.Forms.

Użytkownik może również definiować własne niestandardowe rozszerzenia znaczników w XAML przez pochodząca od `IMarkupExtension` lub `IMarkupExtension<T>`. Formularz ogólny należy użyć, jeśli rozszerzenie znaczników uzyskuje wartość określonego typu. Jest to w przypadku kilku Xamarin.Forms — rozszerzenia znaczników:

- `TypeExtension` pochodzi od klasy `IMarkupExtension<Type>`
- `ArrayExtension` pochodzi od klasy `IMarkupExtension<Array>`
- `DynamicResourceExtension` pochodzi od klasy `IMarkupExtension<DynamicResource>`
- `BindingExtension` pochodzi od klasy `IMarkupExtension<BindingBase>`
- `ConstraintExpression` pochodzi od klasy `IMarkupExtension<Constraint>`

Dwa `IMarkupExtension` interfejsy definiują tylko jedną metodę, o nazwie `ProvideValue`:

```csharp
public interface IMarkupExtension
{
    object ProvideValue(IServiceProvider serviceProvider);
}

public interface IMarkupExtension<out T> : IMarkupExtension
{
    new T ProvideValue(IServiceProvider serviceProvider);
}
```

Ponieważ `IMarkupExtension<T>` pochodzi od klasy `IMarkupExtension` i zawiera `new` słowo kluczowe w `ProvideValue`, zawiera ona `ProvideValue` metody.

Bardzo często rozszerzeń struktury znaczników XAML definiują właściwości, które przyczyniają się do wartości zwracanej. (Wyjątek oczywiste `NullExtension`, w którym `ProvideValue` po prostu zwraca `null`.) `ProvideValue` Metoda ma pojedynczy argument typu `IServiceProvider` , zostanie dokładnie omówione w dalszej części tego artykułu.

## <a name="a-markup-extension-for-specifying-color"></a>Rozszerzenie znaczników w celu określania kolorów

Następujące rozszerzenie znaczników w XAML pozwala utworzyć `Color` wartość za pomocą składników aplikacji hue, nasycenia i jasności. Definiuje cztery właściwości czterech składników koloru, w tym składnik alfa, który jest inicjowany do 1. Klasa pochodzi z `IMarkupExtension<Color>` do wskazania `Color` zwracają wartość:

```csharp
public class HslColorExtension : IMarkupExtension<Color>
{
    public double H { set; get; }

    public double S { set; get; }

    public double L { set; get; }

    public double A { set; get; } = 1.0;

    public Color ProvideValue(IServiceProvider serviceProvider)
    {
        return Color.FromHsla(H, S, L, A);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<Color>).ProvideValue(serviceProvider);
    }
}
```

Ponieważ `IMarkupExtension<T>` pochodzi od klasy `IMarkupExtension`, klasy musi zawierać dwa `ProvideValue` metody, która zwraca `Color` , a drugi, która zwraca `object`, ale druga metoda po prostu wywołać pierwszej metody.

**Pokaz kolor HSL** strona zawiera różne sposoby `HslColorExtension` może znajdować się w pliku XAML, aby określić kolor `BoxView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.HslColorDemoPage"
             Title="HSL Color Demo">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="WidthRequest" Value="80" />
                <Setter Property="HeightRequest" Value="80" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <BoxView>
            <BoxView.Color>
                <local:HslColorExtension H="0" S="1" L="0.5" A="1" />
            </BoxView.Color>
        </BoxView>

        <BoxView>
            <BoxView.Color>
                <local:HslColor H="0.33" S="1" L="0.5" />
            </BoxView.Color>
        </BoxView>

        <BoxView Color="{local:HslColorExtension H=0.67, S=1, L=0.5}" />

        <BoxView Color="{local:HslColor H=0, S=0, L=0.5}" />

        <BoxView Color="{local:HslColor A=0.5}" />
    </StackLayout>
</ContentPage>
```

Należy zauważyć, że w przypadku `HslColorExtension` jest XML tag, cztery właściwości są ustawione jako atrybuty, ale gdy się pojawi się między nawiasy klamrowe, cztery właściwości są oddzielone przecinkami, bez znaków cudzysłowu. Wartości domyślne `H`, `S`, i `L` 0 i wartość domyślną `A` wynosi 1, dzięki czemu te właściwości można pominąć, jeśli chcesz je ustawione wartości domyślne. Ostatni przykład pokazuje przykład gdzie jasność ma wartość 0, co zwykle powoduje czarny, ale kanał alfa wynosi 0,5, tak, aby go jest niewidoczna połowicznie szarego białym tle strony:

[![Pokaz kolor HSL](creating-images/hslcolordemo-small.png "pokaz kolor HSL")](creating-images/hslcolordemo-large.png#lightbox "pokaz kolor HSL")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>Rozszerzenia znaczników do uzyskiwania dostępu do mapy bitowej

Argument `ProvideValue` jest obiektem, który implementuje [ `IServiceProvider` ](xref:System.IServiceProvider) interfejs, który jest zdefiniowany w .NET `System` przestrzeni nazw. Ten interfejs ma jeden element członkowski, metodę o nazwie `GetService` z `Type` argumentu.

`ImageResourceExtension` Klasy poniżej demonstruje jedno użycie możliwe `IServiceProvider` i `GetService` uzyskać `IXmlLineInfoProvider` obiekt, który może dostarczyć informacji wierszy i znaków wskazujący, w których wykryto określony błąd. W takim wypadku zgłaszany jest wyjątek podczas `Source` nie ustawiono właściwości:

```csharp
[ContentProperty("Source")]
class ImageResourceExtension : IMarkupExtension<ImageSource>
{
    public string Source { set; get; }

    public ImageSource ProvideValue(IServiceProvider serviceProvider)
    {
        if (String.IsNullOrEmpty(Source))
        {
            IXmlLineInfoProvider lineInfoProvider = serviceProvider.GetService(typeof(IXmlLineInfoProvider)) as IXmlLineInfoProvider;
            IXmlLineInfo lineInfo = (lineInfoProvider != null) ? lineInfoProvider.XmlLineInfo : new XmlLineInfo();
            throw new XamlParseException("ImageResourceExtension requires Source property to be set", lineInfo);
        }

        string assemblyName = GetType().GetTypeInfo().Assembly.GetName().Name;

        return ImageSource.FromResource(assemblyName + "." + Source);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<ImageSource>).ProvideValue(serviceProvider);
    }
}
```

`ImageResourceExtension` jest przydatne, gdy plik XAML ma dostęp do pliku obrazu, który jest przechowywany w postaci zasobów osadzonych, w projekcie biblioteki .NET Standard. Używa ona `Source` Wywołaj statyczną właściwość `ImageSource.FromResource` metody. Ta metoda wymaga nazwę zasobu w pełni kwalifikowaną, która składa się z nazwy zestawu, nazwę folderu i nazwę pliku, oddzielone kropkami. `ImageResourceExtension` Nie potrzebujesz Nazwa zestawu część ponieważ uzyskuje nazwę zestawu przy użyciu odbicia i dołącza go do `Source` właściwości. Niezależnie od tego `ImageSource.FromResource` musi być wywołana z zestawu, który zawiera mapę bitową, co oznacza, że to rozszerzenie zasobów XAML nie może być częścią zewnętrznej biblioteki, chyba, że obrazy są również w tej bibliotece. (Zobacz [ **obrazów osadzonych** ](~/xamarin-forms/user-interface/images.md#embedded_images) artykuł, aby uzyskać więcej informacji na temat uzyskiwania dostępu do mapy bitowe przechowywane jako zasoby osadzone.)

Mimo że `ImageResourceExtension` wymaga `Source` właściwości można ustawić `Source` wskazana jest właściwość w atrybucie jako zawartości właściwości klasy. Oznacza to, że `Source=` można pominąć część wyrażenia w nawiasach klamrowych. W **pokaz zasobu obrazu** stronie `Image` elementy Pobierz dwa obrazy przy użyciu nazwy folderu i nazwę pliku, oddzielone kropkami:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.ImageResourceDemoPage"
             Title="Image Resource Demo">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Image Source="{local:ImageResource Images.SeatedMonkey.jpg}"
               Grid.Row="0" />

        <Image Source="{local:ImageResource Images.FacePalm.jpg}"
               Grid.Row="1" />

    </Grid>
</ContentPage>
```

W tym miejscu jest uruchomiony na wszystkich trzech platformach program:

[![Obraz pokaz zasobów](creating-images/imageresourcedemo-small.png "obrazu pokaz zasobów")](creating-images/imageresourcedemo-large.png#lightbox "obrazu pokaz zasobów")

## <a name="service-providers"></a>Dostawcy usług

Za pomocą `IServiceProvider` argument `ProvideValue`, rozszerzeń struktury znaczników XAML mogą uzyskać dostęp do przydatne informacje dotyczące pliku XAML, w którym są one używane. Do użycia, ale `IServiceProvider` argument pomyślnie, musisz wiedzieć, jakiego rodzaju usługi są dostępne w określonym kontekście. Najlepszym sposobem zawierają opis tej funkcji jest poprzez analizę kodu źródłowego istniejące rozszerzenia znaczników w XAML w [ **wyrażeń MarkupExtension** folderu](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) w repozytorium zestawu narzędzi Xamarin.Forms w witrynie GitHub. Należy pamiętać, że niektóre typy usług są wewnętrzne dla zestawu narzędzi Xamarin.Forms.

W niektórych rozszerzenia znaczników w XAML ta usługa może być przydatne:

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget` Interfejs definiuje dwie właściwości `TargetObject` i `TargetProperty`. Jeśli informacje te są uzyskiwane w `ImageResourceExtension` klasy `TargetObject` jest `Image` i `TargetProperty` jest `BindableProperty` dla obiektu `Source` właściwość `Image`. Jest to właściwość ustawiono XAML — rozszerzenie znaczników.

`GetService` Wywołania z argumentem `typeof(IProvideValueTarget)` rzeczywistości zwraca obiekt typu `SimpleValueTargetProvider`, który jest zdefiniowany w `Xamarin.Forms.Xaml.Internals` przestrzeni nazw. Jeśli rzutowanie zwracanej wartości z `GetService` dla tego typu, możesz także uzyskać dostęp `ParentObjects` właściwość, która jest tablicą zawierającą `Image` elementu `Grid` obiektu nadrzędnego i `ImageResourceDemoPage` nadrzędnym `Grid`.

## <a name="conclusion"></a>Wniosek

Rozszerzeń struktury znaczników XAML odgrywają kluczową rolę w XAML, rozszerzając możliwości można ustawić atrybutów z różnych źródeł. Ponadto jeśli istniejące rozszerzenia znaczników w XAML nie zawierają dokładnie co jest potrzebne, można także napisać własny.


## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia znaczników (przykład)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [XAML znaczników rozszerzenia rozdziału z książki zestawu narzędzi Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
