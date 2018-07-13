---
title: Ścieżka powiązania zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak uzyskać dostęp do właściwości podrzędnych i elementy członkowskie kolekcji z właściwością ścieżki klasy wiązania za pomocą powiązania danych zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 887a20f1791a190c182e6d179cfabb46c6e0eb48
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998950"
---
# <a name="xamarinforms-binding-path"></a>Ścieżka powiązania zestawu narzędzi Xamarin.Forms

We wszystkich poprzednich przykładach wiązania danych [ `Path` ](xref:Xamarin.Forms.Binding.Path) właściwość `Binding` klasy (lub [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path) właściwość `Binding` — rozszerzenie znaczników) został ustawiony do jednej właściwości. Jest możliwe ustawienie `Path` do *właściwości podrzędnych* (właściwość właściwości), lub do elementu członkowskiego kolekcji.

Załóżmy, że strona zawiera `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

`Time` Właściwość `TimePicker` typu `TimeSpan`, ale być może chcesz utworzyć powiązania danych, który odwołuje się do `TotalSeconds` właściwość, która `TimeSpan` wartość. Oto powiązanie danych:

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

`Time` Właściwość jest typu `TimeSpan`, który ma `TotalSeconds` właściwości. `Time` i `TotalSeconds` właściwości są simply połączonych kropką. Elementy w `Path` ciągu zawsze odwołuje się do właściwości, a nie typy tych właściwości.

Przykład i kilka innych są wyświetlane w **odmiany ścieżki** strony:

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

W drugim `Label`, źródło wiążące się sama strona. `Content` Właściwość jest typu `StackLayout`, który ma `Children` właściwości typu `IList<View>`, który ma `Count` właściwości wskazującą liczbę elementów podrzędnych.

## <a name="paths-with-indexers"></a>Ścieżki przy użyciu indeksatorów

Powiązanie w trzecim `Label` w **odmiany ścieżki** strony odwołania [ `CultureInfo` ](xref:System.Globalization.CultureInfo) klasy w `System.Globalization` przestrzeni nazw:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

Źródło jest ustawiona na statyczną `CultureInfo.CurrentCulture` właściwość, która jest obiektem typu `CultureInfo`. Czy klasa definiuje właściwość o nazwie `DateTimeFormat` typu [ `DateTimeFormatInfo` ](xref:System.Globalization.DateTimeFormatInfo) zawierający `DayNames` kolekcji. Indeks wybiera czwarty element.

Czwarty `Label` wykonuje coś podobnego, ale dla kultury skojarzony (Francja). `Source` Powiązania zostaje ustalona `CultureInfo` obiektu za pomocą konstruktora:

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

Zobacz [przekazywania argumentów konstruktora](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) Aby uzyskać więcej informacji na temat określania argumentów konstruktora w XAML.

Na koniec, ostatni przykład jest podobny do drugiego, z tą różnicą, że odwołuje się do jednego z elementów podrzędnych elementu `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Ten podrzędny jest `Label`, który ma `Text` właściwości typu `String`, który ma `Length` właściwości. Pierwszy `Label` raporty `TimeSpan` w `TimePicker`, więc po zmianie tekstu końcowe `Label` zmieni się także.

W tym miejscu jest uruchomiony na wszystkich trzech platformach program:

[![Ścieżka odmiany](binding-path-images/pathvariations-small.png "odmiany ścieżki")](binding-path-images/pathvariations-large.png#lightbox "odmiany ścieżki")

## <a name="debugging-complex-paths"></a>Debugowanie złożonych ścieżek

Definicje skomplikowanej ścieżki mogą być trudne do konstruowania: musisz znać typ każdej właściwości podrzędnych lub typ elementów w kolekcji, aby poprawnie dodać następną właściwość podrzędnych, ale same typy nie są wyświetlane w ścieżce. Jedna z technik dobre jest do zbudowania ścieżkę stopniowo i spójrz na wyniki pośrednie. Ten przykład ostatniego można uruchomić bez `Path` definicji wszystkie:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

Który jest wyświetlany typ źródło wiążące lub `DataBindingDemos.PathVariationsPage`. Znasz `PathVariationsPage` pochodzi od klasy `ContentPage`, więc posiada `Content` właściwości:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

Typ `Content` właściwości są teraz czytelne jako `Xamarin.Forms.StackLayout`. Dodaj `Children` właściwości `Path` a typem `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, która jest klasą wewnętrznych zestawu narzędzi Xamarin.Forms, ale oczywiście typem kolekcji. Dodawanie indeksu, a typem `Xamarin.Forms.Label`. Kontynuuj w ten sposób.

Zgodnie z zestawu narzędzi Xamarin.Forms przetwarza ścieżka powiązania, instaluje `PropertyChanged` obsługi dla dowolnego obiektu w ścieżce, która implementuje `INotifyPropertyChanged` interfejsu. Na przykład powiązanie końcowego reaguje na zmianę w pierwszym `Label` ponieważ `Text` zmiany właściwości.

Jeśli właściwość w ścieżce powiązania nie implementuje `INotifyPropertyChanged`, wszelkie zmiany tej właściwości zostanie zignorowany. Niektóre zmiany całkowicie może unieważnić ścieżka powiązania, dzięki czemu tej techniki należy używać tylko wtedy, gdy parametry i właściwości podrzędnych nigdy nie stanie się nieprawidłowy.



## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
