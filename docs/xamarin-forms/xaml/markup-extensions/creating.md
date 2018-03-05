---
title: "Tworzenie rozszerzeń znaczników XAML"
description: "Definiowanie własnych niestandardowych rozszerzeń znaczników XAML"
ms.topic: article
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: eb7189226e4f5d7eb2b55bf61728e65db44ba57b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="creating-xaml-markup-extensions"></a>Tworzenie rozszerzeń znaczników XAML

Na poziomie programowe rozszerzenie znaczników w XAML jest klasa implementująca [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) lub [ `IMarkupExtension<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension%3CT%3E/) interfejsu. Można eksplorować kodu źródłowego rozszerzeń znaczników standardowe opisane poniżej w [ **wyrażenia MarkupExtension** katalogu](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) repozytorium GitHub platformy Xamarin.Forms. 

Istnieje również możliwość zdefiniowania własnych niestandardowych rozszerzeń znaczników XAML przez pochodny `IMarkupExtension` lub `IMarkupExtension<T>`. Formularz ogólny rozszerzenie znaczników uzyska wartość określonego typu. Jest to w przypadku kilku platformy Xamarin.Forms rozszerzenia znaczników:

- `TypeExtension` pochodną `IMarkupExtension<Type>`
- `ArrayExtension` pochodną `IMarkupExtension<Array>`
- `DynamicResourceExtension` pochodną `IMarkupExtension<DynamicResource>`
- `BindingExtension` pochodną `IMarkupExtension<BindingBase>`
- `ConstraintExpression` pochodną `IMarkupExtension<Constraint>`

Dwa `IMarkupExtension` interfejsy zdefiniować tylko jedną metodę, o nazwie `ProvideValue`: 

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

Ponieważ `IMarkupExtension<T>` pochodną `IMarkupExtension` i zawiera `new` — słowo kluczowe na `ProvideValue`, zawiera `ProvideValue` metody.

Bardzo często rozszerzenia znaczników XAML definiują właściwości, które składają się do wartości zwracanej. (Wyjątek oczywiste `NullExtension`, w którym `ProvideValue` po prostu zwraca `null`.) `ProvideValue` Metoda ma jeden argument typu `IServiceProvider` które zostaną dokładniej omówione w dalszej części tego artykułu.

## <a name="a-markup-extension-for-specifying-color"></a>Rozszerzenie znacznika służący do określania kolorów

Następujące rozszerzenie znaczników w XAML pozwala utworzyć `Color` wartość przy użyciu składników odcień, nasycenie i jasność. Definiuje czterech właściwości czterech składników koloru, w tym alfa składnik, który jest ustawiana na 1. Klasa pochodzi z `IMarkupExtension<Color>` aby wskazać `Color` zwrócić wartość:

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

Ponieważ `IMarkupExtension<T>` pochodzi z `IMarkupExtension`, klasa musi zawierać dwa `ProvideValue` metody, która zwraca `Color` i drugą, która zwraca `object`, ale druga metoda można po prostu Wywołaj metodę pierwszy.

**Pokaz kolor HSL** strony zawiera różne sposoby, które `HslColorExtension` może występować w pliku XAML, aby określić kolor `BoxView`:

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

Zwróć uwagę, że w przypadku `HslColorExtension` jest XML tag, czterech właściwości są ustawione jako atrybuty, ale jeśli wydaje się między nawiasy klamrowe, czterech właściwości są rozdzielane przecinkami bez znaków cudzysłowu. Wartości domyślne `H`, `S`, i `L` 0 i wartość domyślną `A` ma wartość 1, więc te właściwości można pominąć, jeśli mają ustawione wartości domyślne. Ostatnim przykładzie pokazano przykład gdzie jasność jest 0, co zwykle powoduje czarne, ale kanału alfa wynosi 0,5, tak, aby go jest niewidoczny pół szary, biały tle strony:

[![Pokaz kolor HSL](creating-images/hslcolordemo-small.png "pokaz kolor HSL")](creating-images/hslcolordemo-large.png "pokaz kolor HSL")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>Rozszerzenie znaczników, aby uzyskać dostęp do map bitowych

Argument `ProvideValue` jest obiekt, który implementuje [ `IServiceProvider` ](https://developer.xamarin.com/api/type/System.IServiceProvider/) interfejs, który jest zdefiniowany w ramach platformy .NET `System` przestrzeni nazw. Ten interfejs jest jeden element członkowski, metodę o nazwie `GetService` z `Type` argumentu. 

`ImageResourceExtension` Klasy pokazano poniżej przedstawia jedną wykorzystanie `IServiceProvider` i `GetService` uzyskanie `IXmlLineInfoProvider` obiekt, który może dostarczyć informacji wiersza i znaku wskazującą, w których wykryto określony błąd. W takim przypadku wyjątek zgłoszony podczas `Source` nie ustawiono właściwości:

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

`ImageResourceExtension` jest przydatne, gdy plik XAML wymaga dostępu do pliku obrazu, przechowywane jako osadzony zasób w projekcie przenośnej biblioteki klas. Używa `Source` właściwości do wywoływania statycznych `ImageSource.FromResource` metody. Ta metoda wymaga nazwy zasobu w pełni kwalifikowaną, która składa się z nazwy zestawu, nazwę folderu i nazwę pliku, oddzielone kropkami. `ImageResourceExtension` Nie muszą zestawu Nazwa części ponieważ uzyskuje nazwę zestawu przy użyciu odbicia i dołącza go do `Source` właściwości. Niezależnie od tego `ImageSource.FromResource` musi być wywoływana z zestawu, który zawiera mapa bitowa, co oznacza, że to rozszerzenie zasobu XAML nie może być częścią zewnętrznej biblioteki obrazów nie znajdują się również w tej bibliotece. (Zobacz [ **obrazów osadzonych** ](~/xamarin-forms/user-interface/images.md#embedded_images) artykułu, aby uzyskać więcej informacji na temat uzyskiwania dostępu do mapy bitowe przechowywane jako zasoby osadzone.) 

Mimo że `ImageResourceExtension` wymaga `Source` można ustawić dla właściwości `Source` właściwości jako właściwość content klasy określonej w atrybucie. Oznacza to, że `Source=` można pominąć części wyrażenia w nawiasach klamrowych. W **pokaz zasobu obrazu** strony, `Image` dwa obrazy przy użyciu nazwy folderu i nazwę pliku, oddzielone kropkami pobrać elementy:

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

Oto programu uruchomionego na wszystkich platformach trzy:

[![Pokaz zasobu obrazu](creating-images/imageresourcedemo-small.png "obrazu pokaz zasobów")](creating-images/imageresourcedemo-large.png "obrazu pokaz zasobów")

## <a name="service-providers"></a>Dostawcy usług

Za pomocą `IServiceProvider` argument `ProvideValue`, rozszerzenia znaczników XAML mogą korzystać z przydatne informacje o pliku XAML, w którym są one używane. Do użycia, ale `IServiceProvider` argument pomyślnie, musisz wiedzieć, jakiego rodzaju usług są dostępne w szczególności kontekstach. Najlepszym sposobem zawierają opis tej funkcji jest poprzez analizę kodu źródłowego istniejących rozszerzeń znaczników XAML w [ **wyrażenia MarkupExtension** folderu](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) w repozytorium platformy Xamarin.Forms w witrynie GitHub. Należy pamiętać, że niektóre typy usług są wewnętrzne platformy Xamarin.Forms.

W niektórych rozszerzeń znaczników XAML tej usługi może być przydatna:

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget` Interfejs definiuje dwie właściwości `TargetObject` i `TargetProperty`. Jeśli te informacje są uzyskiwane w `ImageResourceExtension` klasy `TargetObject` jest `Image` i `TargetProperty` jest `BindableProperty` obiekt do `Source` właściwość `Image`. Jest to właściwość ustawiono rozszerzenie znaczników w XAML.

`GetService` Wywołania z argumentem `typeof(IProvideValueTarget)` faktycznie zwraca obiekt typu `SimpleValueTargetProvider`, która jest zdefiniowana w `Xamarin.Forms.Xaml.Internals` przestrzeni nazw. Jeśli rzutowania wartość zwracaną `GetService` dla tego typu, można także przejść do `ParentObjects` właściwość, która jest tablica zawierająca `Image` elementu, `Grid` nadrzędnej i `ImageResourceDemoPage` nadrzędny `Grid`.

## <a name="conclusion"></a>Wniosek

Rozszerzenia znaczników XAML odgrywać istotną rolę w języku XAML, rozszerzając możliwości można ustawić atrybutów z różnych źródeł. Ponadto jeśli istniejące rozszerzenia znaczników XAML nie zawiera dokładnie należy, można również napisać własny. 


## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia znaczników (przykład)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Rozdział rozszerzeń znaczników XAML z książki platformy Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
