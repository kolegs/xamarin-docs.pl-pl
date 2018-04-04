---
title: Ciąg formatowania
description: Użyj powiązania danych, aby sformatować i wyświetlić obiekty jako ciągi
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 4e143f650c3cde7577def1a95e53b207608a088a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="string-formatting"></a>Ciąg formatowania

Czasami jest łatwe w użyciu powiązania danych, aby wyświetlić reprezentację ciągu obiektu lub wartości. Na przykład możesz chcieć użyć `Label` Aby wyświetlić bieżącą wartość `Slider`. W tym powiązaniu danych `Slider` jest źródłem, a element docelowy jest `Text` właściwość `Label`.

Podczas wyświetlania ciągów w kodzie, najbardziej zaawansowanych narzędzi jest statycznych [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) metody. Ciąg formatowania zawiera kody właściwe dla różnych typów obiektów formatowania i może zawierać inne tekst wraz z wartościami formatowania. Zobacz [typy formatowania w .NET](/dotnet/standard/base-types/formatting-types/) artykułu, aby uzyskać więcej informacji o ciągu formatowania.

## <a name="the-stringformat-property"></a>StringFormat — właściwość

Tej funkcji są przenoszone do powiązania danych: możesz ustawić [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) właściwość `Binding` (lub [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.StringFormat/) właściwość `Binding` — rozszerzenie znaczników) do standardowa .NET formatowania ciągu z jednego symbolu zastępczego:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

Należy zauważyć, że ciąg formatowania rozdzielane znaków pojedynczego cudzysłowu (apostrof), aby uniknąć traktowanie nawiasów klamrowych jako inne rozszerzenie znaczników w XAML analizatora języka XAML. W przeciwnym razie który ciąg bez znaków pojedynczego cudzysłowu jest ten sam ciąg ma zostać użyty do wyświetlenia wartości zmiennoprzecinkowej w wywołaniu `String.Format`. Specyfikacja formatowania `F2` powoduje, że wartości mają być wyświetlane z dwóch miejsc po przecinku.

`StringFormat` Właściwości sens tylko wtedy, gdy właściwość target jest typu `string`, i jest w trybie powiązania `OneWay` lub `TwoWay`. Dla powiązania dwukierunkowego `StringFormat` dotyczy tylko dla wartości przekazywanie ze źródła do obiektu docelowego.

Jak można zauważyć w artykule na [ścieżka wiązania](binding-path.md), powiązania danych może być dosyć złożona i zwichrowanych. Podczas debugowania tych powiązań danych, można dodać `Label` w pliku XAML z `StringFormat` wyświetlić niektórych wyników pośrednich. Nawet jeśli można używać tylko do wyświetlania typu obiektu, które mogą być przydatne.

**Ciągu formatowania** strony przedstawiono kilka zastosowań `StringFormat` właściwości:

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

Wiązania na `Slider` i `TimePicker` Pokaż korzystania ze specyfikacji formatu dla `double` i `TimeSpan` typy danych. `StringFormat` Wyświetlającym tekst od `Entry` widoku pokazano, jak określić podwójnego cudzysłowu w ciągu formatowania przy użyciu `&quot;` jednostki HTML.

Jest w następnej sekcji w pliku XAML `StackLayout` z `BindingContext` ustawioną `x:Static` rozszerzenie znaczników, który odwołuje się do statycznego `DateTime.Now` właściwości. Pierwsze wiązanie nie ma właściwości:

```xaml
<Label Text="{Binding}" />
```

Spowoduje to wyświetlenie po prostu `DateTime` wartość `BindingContext` z formatowaniem domyślne. Wyświetla powiązania drugi `Ticks` właściwość `DateTime`, podczas wyświetlania dwa powiązania `DateTime` z określonego formatu. Zwróć uwagę, to `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

Jeśli konieczne jest wyświetlenie lewej lub prawej nawiasy klamrowe w ciągu formatowania, po prostu użyć parę z nich.

Ostatni zestawy sekcji `BindingContext` wartość `Math.PI` i wyświetla je z domyślnego formatowania i dwa różne typy formatowanie liczbowe.

Oto programu uruchomionego na wszystkich platformach trzy:

[![Ciąg formatowania](string-formatting-images/stringformatting-small.png "ciągu formatowania")](string-formatting-images/stringformatting-large.png#lightbox "ciągu formatowania")

## <a name="viewmodels-and-string-formatting"></a>ViewModels i ciągu formatowania

Jeśli używasz `Label` i `StringFormat` do wyświetlania wartości widoku, który jest również cel ViewModel, można zdefiniować powiązania z widoku `Label` lub ViewModel do `Label`. Ogólnie rzecz biorąc drugi metoda jest najbardziej przydatna, ponieważ sprawdza powiązania między widokiem i ViewModel pracy.

Takie podejście jest wyświetlany w obszarze **lepsze selektor kolorów** próbki, która używa tego samego ViewModel jako **prosty selektor kolorów** program wyświetlany w [ **powiązanie tryb** ](binding-mode.md) artykułu:

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

Obecnie dostępne są trzy pary `Slider` i `Label` elementów, które są powiązane do tego samego źródła właściwości w `HslColorViewModel` obiektu. Jedyną różnicą jest to, że `Label` ma `StringFormat` właściwości do wyświetlenia każdego `Slider` wartość.

[![Kolor lepiej selektora](string-formatting-images/bettercolorselector-small.png "lepiej kolor selektora")](string-formatting-images/bettercolorselector-large.png#lightbox "lepiej kolor selektora")

Możesz może się zastanawiać, jak można wyświetlić wartości RGB (czerwony, zielony, niebieski) format tradycyjnych dwóch cyfr szesnastkowych. Te wartości całkowite nie są bezpośrednio dostępne `Color` struktury. Jedno rozwiązanie będzie można obliczyć wartości będące liczbami całkowitymi składniki kolorów w ViewModel i ujawniać je jako właściwości. Następnie można sformatować przy użyciu `X2` specyfikacji formatowania.

Innym rozwiązaniem jest bardziej ogólne: można napisać *konwerter wartości powiązania* zgodnie z opisem w artykule nowsze [ **konwertery wartości powiązania**](converters.md).

Kolejnym artykule jednak Eksploruje [ **ścieżka wiązania** ](binding-path.md) bardziej szczegółowo i pokazać, jak umożliwia on odwołania podrzędne właściwości i elementów w kolekcji.


## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
