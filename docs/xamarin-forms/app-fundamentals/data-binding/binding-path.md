---
title: "Ścieżka powiązania"
description: "Użyj powiązania danych do właściwości podrzędnej dostępu i elementów członkowskich kolekcji"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 39d326714a6fee1abe242a7256888647784cdec3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="binding-path"></a>Ścieżka powiązania

We wszystkich poprzednich przykładach wiązania danych [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) właściwość `Binding` klasy (lub [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) właściwość `Binding` — rozszerzenie znaczników) została ustawiona do jednej właściwości. Można ustawić jest `Path` do *podrzędne właściwości* (właściwość właściwości), lub do elementu członkowskiego zbioru.

Na przykład, załóżmy, że strona zawiera `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

`Time` Właściwość `TimePicker` jest typu `TimeSpan`, ale być może chcesz utworzyć powiązania danych, który odwołuje się do `TotalSeconds` właściwości tego `TimeSpan` wartość. Powiązanie danych jest następujący:

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```
         
`Time` Właściwość jest typu `TimeSpan`, który ma `TotalSeconds` właściwości. `Time` i `TotalSeconds` właściwości są simply połączone kropką. Elementy w `Path` ciąg zawsze odwołuje się do właściwości, a nie do typy tych właściwości.

Przykład i kilka innych są wyświetlane w **zmiany ścieżki** strony:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:globe="clr-namespace:System.Globalization;assembly=mscorlib"
             x:Class="DataBindingDemos.PathVariationsPage"
             Title="Path Variations"
             x:Name="page">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="HorizontalTextAlignment" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    
    <StackLayout Margin="10, 0">
        <TimePicker x:Name="timePicker" />

        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time.TotalSeconds,
                              StringFormat='{0} total seconds'}" />

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children.Count,
                              StringFormat='There are {0} children in this StackLayout'}" />
        
        <Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                              Path=DateTimeFormat.DayNames[3],
                              StringFormat='The middle day of the week is {0}'}" />

        <Label>
            <Label.Text>
                <Binding Path="DateTimeFormat.DayNames[3]"
                         StringFormat="The middle day of the week in France is {0}">
                    <Binding.Source>
                        <globe:CultureInfo>
                            <x:Arguments>
                                <x:String>fr-FR</x:String>
                            </x:Arguments>
                        </globe:CultureInfo>
                    </Binding.Source>
                </Binding>
            </Label.Text>
        </Label>

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children[1].Text.Length,
                              StringFormat='The first Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

W ciągu sekundy `Label`, źródło powiązania jest samej strony. `Content` Właściwość jest typu `StackLayout`, który ma `Children` właściwości typu `IList<View>`, który ma `Count` właściwości wskazującą liczbę elementów podrzędnych.

## <a name="paths-with-indexers"></a>Ścieżki z indeksatorów

Powiązanie w trzecim polu `Label` w **zmiany ścieżki** odwołuje się do strony [ `CultureInfo` ](https://developer.xamarin.com/api/type/System.Globalization.CultureInfo/) klasy w `System.Globalization` przestrzeni nazw:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

Źródło ustawiono statycznych `CultureInfo.CurrentCulture` właściwość, która jest obiektem typu `CultureInfo`. Czy klasa definiuje właściwość o nazwie `DateTimeFormat` typu [ `DateTimeFormatInfo` ](https://developer.xamarin.com/api/type/System.Globalization.DateTimeFormatInfo/) zawierający `DayNames` kolekcji. Indeks wybiera czwarty element.

Czwarta `Label` jest podobny, ale dla kultury skojarzone z Francja. `Source` Ma ustawioną właściwość powiązania `CultureInfo` obiektów z konstruktorem:

```xaml
<Label>
    <Label.Text>
        <Binding Path="DateTimeFormat.DayNames[3]"
                 StringFormat="The middle day of the week in France is {0}">
            <Binding.Source>
                <globe:CultureInfo>
                    <x:Arguments>
                        <x:String>fr-FR</x:String>
                    </x:Arguments>
                </globe:CultureInfo>
            </Binding.Source>
        </Binding>
    </Label.Text>
</Label>
```

Zobacz [przekazywanie argumentów konstruktora](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) Aby uzyskać więcej informacji na temat określania argumentów konstruktora w języku XAML.

Na koniec ostatnim przykładzie jest podobny do drugiego, z wyjątkiem odwołuje się ona do jednego z elementów podrzędnych `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Ten podrzędny jest `Label`, który ma `Text` właściwości typu `String`, który ma `Length` właściwości. Pierwszy `Label` raporty `TimeSpan` w `TimePicker`, więc po zmianie tekstu ostatecznych `Label` również zmiany.

Oto programu uruchomionego na wszystkich platformach trzy:

[![Zmiany ścieżki](binding-path-images/pathvariations-small.png "zmiany ścieżki")](binding-path-images/pathvariations-large.png "zmiany ścieżki")

## <a name="debugging-complex-paths"></a>Debugging Complex Paths

Definicje złożonych ścieżka może być trudne do skonstruowania: należy znać typ każdej właściwości podrzędne lub typ elementów w kolekcji, aby prawidłowo dodać dalej właściwości podrzędnej, ale same typy nie są wyświetlane w ścieżce. Jeden dobry technika jest do zbudowania ścieżka przyrostowo i przyjrzyj wyniki pośrednie. W tym ostatnim przypadku można uruchomić bez `Path` definicji co:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

Która wyświetla typ źródła powiązanie lub `DataBindingDemos.PathVariationsPage`. Znasz `PathVariationsPage` pochodną `ContentPage`, co powoduje `Content` właściwości:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

Typ `Content` właściwości jest teraz ujawniony jako `Xamarin.Forms.StackLayout`. Dodaj `Children` właściwości `Path` a typem `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, które jest klasą wewnętrzną platformy Xamarin.Forms, ale oczywiście typem kolekcji. Dodawanie indeksu, jak i typ jest `Xamarin.Forms.Label`. Kontynuuj w ten sposób.

Jak platformy Xamarin.Forms przetwarza ścieżkę wiązania, instaluje `PropertyChanged` obsługi dowolnego obiektu w ścieżce, która implementuje `INotifyPropertyChanged` interfejsu. Na przykład powiązanie końcowego reaguje na zmianę w pierwszym `Label` ponieważ `Text` zmiany właściwości. 

Jeśli właściwość w ścieżce powiązania nie implementuje `INotifyPropertyChanged`, zmiany wprowadzone w tej właściwości zostanie zignorowany. Niektóre zmiany całkowicie może unieważnić ścieżkę wiązania, należy użyć tej techniki tylko wtedy, gdy ciąg i podrzędne właściwości nigdy nie staną się nieprawidłowe.



## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
