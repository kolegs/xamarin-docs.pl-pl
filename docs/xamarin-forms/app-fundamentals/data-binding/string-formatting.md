---
title: Formatowanie ciągu zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak za pomocą powiązania danych zestawu narzędzi Xamarin.FOrms formatowania i Wyświetl obiekty jako ciągi. Jest to osiągane przez ustawienie StringFormat powiązania do standardowego ciągu formatowania .NET z symbolem zastępczym.
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: a876e81c67b6ec61a2cb29143cb001a7d6160032
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998150"
---
# <a name="xamarinforms-string-formatting"></a>Formatowanie ciągu zestawu narzędzi Xamarin.Forms

Czasami wygodne jest wyświetlanie ciąg reprezentujący obiekt lub wartość za pomocą powiązania danych. Na przykład możesz chcieć użyć `Label` do wyświetlenia bieżącej wartości `Slider`. W tym powiązaniu danych `Slider` jest źródło i miejsce docelowe jest `Text` właściwość `Label`.

Podczas wyświetlania ciągów w kodzie, najbardziej zaawansowanych narzędzi jest statyczną [ `String.Format` ](xref:System.String.Format(System.String,System.Object)) metody. Ciąg formatowania zawiera formatowanie specyficzne dla różnych typów obiektów kodów, i może zawierać inne teksty wraz z wartościami formatowana. Zobacz [typy formatowania na platformie .NET](/dotnet/standard/base-types/formatting-types/) artykuł, aby uzyskać więcej informacji na temat ciąg formatowania.

## <a name="the-stringformat-property"></a>Właściwość StringFormat

Ten obiekt jest przenoszone do powiązania danych: możesz ustawić [ `StringFormat` ](xref:Xamarin.Forms.BindingBase.StringFormat) właściwość `Binding` (lub [ `StringFormat` ](xref:Xamarin.Forms.Xaml.BindingExtension.StringFormat) właściwość `Binding` — rozszerzenie znaczników) do .NET Standard formatowanie ciągu z jednego symbolu zastępczego:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

Należy zauważyć, że ciąg formatowania sterujący jest ujęty w znaki pojedynczego cudzysłowu (apostrof), ułatwiające analizatora XAML, należy unikać znaku nawiasów klamrowych jako inne rozszerzenie znaczników w XAML. W przeciwnym razie, ciąg bez znaków pojedynczego cudzysłowu jest ten sam ciąg, który zostanie wykorzystany, aby wyświetlić wartość zmiennoprzecinkowa w wywołaniu `String.Format`. Specyfikacja formatowania `F2` powoduje, że wartości, które mają być wyświetlane z dwóch miejsc po przecinku.

`StringFormat` Właściwość sens tylko wtedy, gdy właściwość docelowa jest typu `string`, i jest w trybie powiązania `OneWay` lub `TwoWay`. Dla powiązania dwukierunkowego `StringFormat` ma zastosowanie tylko dla wartości, przekazując ze źródła do docelowego.

Jak można zauważyć w następnym artykule na [ścieżka wiązania](binding-path.md), powiązań danych, może stać się dość skomplikowane i zawiłe. Podczas debugowania tych powiązań danych, możesz dodać `Label` w pliku XAML z `StringFormat` do wyświetlania niektórych wyników pośrednich. Nawet jeśli jest używana tylko do wyświetlania typu obiektu, które mogą być przydatne.

**Formatowanie ciągów** strona przedstawia kilka zastosowań `StringFormat` właściwości:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="DataBindingDemos.StringFormattingPage"
             Title="String Formatting">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Blue" />
                <Setter Property="HeightRequest" Value="2" />
                <Setter Property="Margin" Value="0, 5" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <Slider x:Name="slider" />
        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='The slider value is {0:F2}'}" />

        <BoxView />

        <TimePicker x:Name="timePicker" />
        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time,
                              StringFormat='The TimeSpan is {0:c}'}" />

        <BoxView />

        <Entry x:Name="entry" />
        <Label Text="{Binding Source={x:Reference entry},
                              Path=Text,
                              StringFormat='The Entry text is &quot;{0}&quot;'}" />

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:DateTime.Now}">
            <Label Text="{Binding}" />
            <Label Text="{Binding Path=Ticks,
                                  StringFormat='{0:N0} ticks since 1/1/1'}" />
            <Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
            <Label Text="{Binding StringFormat='The long date is {0:D}'}" />
        </StackLayout>

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:Math.PI}">
            <Label Text="{Binding}" />
            <Label Text="{Binding StringFormat='PI to 4 decimal points = {0:F4}'}" />
            <Label Text="{Binding StringFormat='PI in scientific notation = {0:E7}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Wiązania na `Slider` i `TimePicker` pokazują użycie specyfikacji formatu dla `double` i `TimeSpan` typy danych. `StringFormat` Wyświetlającym tekst od `Entry` widok pokazuje, jak określić podwójnego cudzysłowu w ciągu formatowania przy użyciu `&quot;` jednostka w kodzie HTML.

Następna sekcja w pliku XAML `StackLayout` z `BindingContext` równa `x:Static` rozszerzenie znaczników, który odwołuje się do statycznego `DateTime.Now` właściwości. Pierwsze wiązanie nie ma właściwości:

```xaml
<Label Text="{Binding}" />
```

To po prostu wyświetla `DateTime` wartość `BindingContext` przy użyciu domyślnego formatowania. Wyświetla drugie powiązanie `Ticks` właściwość `DateTime`, podczas wyświetlania dwa powiązania `DateTime` z określonego formatu. Zwróć uwagę, to `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

Jeśli zachodzi potrzeba wyświetlenia nawiasów klamrowych po lewej stronie lub prawy nawias ciągu formatowania, po prostu użyć parę z nich.

Ostatnie zestawy sekcji `BindingContext` wartość `Math.PI` i wyświetla je przy użyciu domyślnego formatowania i dwa różne typy formatowania liczbowego.

W tym miejscu jest uruchomiony na wszystkich trzech platformach program:

[![Ciąg formatowania](string-formatting-images/stringformatting-small.png "ciągu formatowania")](string-formatting-images/stringformatting-large.png#lightbox "ciągu formatowania")

## <a name="viewmodels-and-string-formatting"></a>Modele widoków i formatowanie ciągu

Jeśli używasz `Label` i `StringFormat` do wyświetlenia wartości widoku, który jest również celem ViewModel, można zdefiniować powiązania z widoku, który `Label` lub ViewModel do `Label`. Ogólnie rzecz biorąc druga metoda jest najlepszy, ponieważ weryfikuje, czy powiązania między widokiem a ViewModel działają.

To podejście jest wyświetlany w **lepsze selektor kolorów** próbki, która używa tego samego ViewModel jako **prosty selektor kolorów** program wyświetlany w [ **powiązanie tryb** ](binding-mode.md) artykułu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.BetterColorSelectorPage"
             Title="Better Color Selector">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Hue}" />
            <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

            <Slider Value="{Binding Saturation}" />
            <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

            <Slider Value="{Binding Luminosity}" />
            <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

Obecnie dostępne są trzy pary `Slider` i `Label` elementy, które są powiązane z takie same źródła właściwości `HslColorViewModel` obiektu. Jedyną różnicą jest to, że `Label` ma `StringFormat` właściwość do wyświetlenia każdego `Slider` wartość.

[![Lepiej kolor selektor](string-formatting-images/bettercolorselector-small.png "lepiej kolor selektor")](string-formatting-images/bettercolorselector-large.png#lightbox "lepiej kolor selektora")

Możesz się zastanawiać jak można wyświetlić wartości RGB (czerwony, zielony, niebieski) format tradycyjnych dwóch cyfr szesnastkowych. Te wartości całkowite nie są bezpośrednio dostępne `Color` struktury. Jedno rozwiązanie będzie do obliczania wartości całkowitych, które składniki kolorów w ramach ViewModel i narażają je jako właściwości. Następnie można sformatować za pomocą `X2` specyfikacji formatowania.

Innym rozwiązaniem jest bardziej ogólny: możesz napisać *powiązania konwertera wartości* zgodnie z opisem w artykule nowsze [ **konwertery wartości powiązania**](converters.md).

Następnego artykułu, jednak Eksploruje [ **ścieżka wiązania** ](binding-path.md) bardziej szczegółowo i pokazują, jak służy do odwoływania się właściwości podrzędnych i elementów w kolekcji.


## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
