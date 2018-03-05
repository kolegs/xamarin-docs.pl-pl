---
title: "Podstawowe powiązania"
description: "Powiązanie danych obiektów docelowych, źródeł i kontekstu powiązania"
ms.topic: article
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 500ad02d79cea79f59b1aca91b0312c9a9d6bac3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="basic-bindings"></a>Podstawowe powiązania

Powiązanie danych platformy Xamarin.Forms łączy pary właściwości między dwoma obiektami, zwykle jest co najmniej jeden obiekt interfejsu użytkownika. Te dwa obiekty są nazywane *docelowej* i *źródła*:

- *Docelowej* jest obiekt (i właściwości) dla zestawu jest powiązanie danych.
- *Źródła* jest odwołuje się powiązanie danych obiektu (i właściwości).

Ta różnica czasami może być nieco mylące: ogólnie rzecz biorąc, dane przepływają ze źródła do obiektu docelowego, co oznacza, że właściwość target jest wartość od wartości właściwości source. Jednak w niektórych przypadkach można również przepływu danych z obiektu docelowego w źródle lub w obu kierunkach. Aby uniknąć pomyłek, należy pamiętać, której celem jest zawsze obiektu, na którym wiązania danych została ustawiona, nawet jeśli jego jest dostarcza dane zamiast odbierania danych.

## <a name="bindings-with-a-binding-context"></a>Powiązania z kontekstem powiązania

Chociaż zazwyczaj określono powiązania danych wyłącznie w języku XAML, jest istotne, aby zobaczyć powiązania danych w kodzie. **Podstawowe powiązanie kodu** strona zawiera plik XAML z `Label` i `Slider`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicCodeBindingPage"
             Title="Basic Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="48"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Slider` Ma wartość z zakresu od 0 do 360. Celem tego programu jest obracanie `Label` przez operacje `Slider`.

Bez powiązania danych, należy ustawić `ValueChanged` zdarzenie `Slider` obsługi zdarzeń, który uzyskuje dostęp do `Value` właściwość `Slider` i ustawia tę wartość na `Rotation` właściwość `Label`. Powiązanie danych automatyzuje zadania; Program obsługi zdarzeń i kod w jego nie są już wymagane. 

Powiązanie można ustawić na wystąpienie dowolnej klasy, która jest pochodną [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), która obejmuje `Element`, `VisualElement`, `View`, i `View` pochodnych.  Powiązanie jest zawsze ustawiony na obiekcie docelowym. Powiązanie odwołuje się do obiektu źródłowego. Aby ustawić wiązania danych, użyj następujących dwóch członków klasy docelowej:

- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Właściwość określa obiekt źródłowy.
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Metody Określa właściwość target i source — właściwość.

W tym przykładzie `Label` jest cel wiązania i `Slider` jest źródłem powiązania. Zmiany w `Slider` źródła wpływa na obrót `Label` docelowej. Przepływy danych ze źródła do obiektu docelowego.

`SetBinding` Metody zdefiniowanej przez `BindableObject` wymaga argumentu typu [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) z którego [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) pochodzi z klasy, ale ma innych `SetBinding` metody zdefiniowane przez [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) klasy. Plik CodeBehind **podstawowe powiązanie kodu** w przykładzie użyto prostsze [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) — metoda rozszerzenia z tej klasy.

```csharp
public partial class BasicCodeBindingPage : ContentPage
{
    public BasicCodeBindingPage()
    {
        InitializeComponent();

        label.BindingContext = slider;
        label.SetBinding(Label.RotationProperty, "Value");
    }
}
```

`Label` Obiekt jest cel wiązania w taki sposób, aby obiektu, na którym ta właściwość jest ustawiona i na którym ma zostać wywołana metoda. `BindingContext` Właściwość wskazuje źródło powiązania, które jest `Slider`.

`SetBinding` Metoda jest wywoływana w celu powiązania, ale Określa właściwość target i właściwości source. Właściwość docelowa jest określony jako `BindableProperty` obiekt: `Label.RotationProperty`. Właściwość źródła jest określony jako ciąg i wskazuje `Value` właściwość `Slider`. 

`SetBinding` Metody ujawnia jedną z najważniejszych zasad powiązania danych:

*Właściwość target musi był zabezpieczony za pomocą właściwości możliwej do wiązania.*

Ta reguła oznacza, że obiekt docelowy muszą być wystąpienia klasy, która jest pochodną `BindableObject`. Zobacz [ **właściwości** ](~/xamarin-forms/xaml/bindable-properties.md) artykuł, aby zapoznać się z omówieniem można powiązać obiekty i właściwości.

Brak nie takie zasady dla właściwości source, który jest określony jako ciąg. Wewnętrznie odbicia umożliwia dostęp do rzeczywistej właściwości. W tym przypadku jednak `Value` właściwość nie jest również obsługiwana przez właściwości możliwej do wiązania.

Kod może być nieco uproszczony: `RotationProperty` właściwości możliwej do wiązania jest definiowana za pomocą `VisualElement`i dziedziczone przez `Label` i `ContentPage` również to nazwa klasy nie jest wymagane w `SetBinding` wywołania:

```csharp
label.SetBinding(RotationProperty, "Value");
```

Jednak łącznie z nazwą klasy jest dobrym monitu obiektu docelowego.

Jak można manipulować `Slider`, `Label` obraca odpowiednio:

[![Kod Basice powiązanie](basic-bindings-images/basiccodebinding-small.png "powiązania podstawowy kod")](basic-bindings-images/basiccodebinding-large.png "kod podstawowe powiązanie")

**Podstawowe powiązanie Xaml** strona jest taki sam jak **podstawowe powiązanie kodu** z tą różnicą, że definiuje on powiązania danych w języku XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicXamlBindingPage"
             Title="Basic XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Tak jak w przypadku kodu, wiązania danych jest ustawiona na obiekt docelowy, który jest `Label`. Uczestniczą dwa rozszerzenia znaczników XAML. Są to natychmiast rozpoznawanym przez ograniczniki nawias klamrowy:

- `x:Reference` — Rozszerzenie znaczników jest wymagany do odwołuje się do obiektu źródłowego, który jest `Slider` o nazwie `slider`.
- `Binding` Łącza rozszerzeń znaczników `Rotation` właściwość `Label` do `Value` właściwość `Slider`. 

Zapoznaj się z artykułem [rozszerzenia znaczników XAML](~/xamarin-forms/xaml/markup-extensions/index.md) Aby uzyskać więcej informacji na temat rozszerzeń znaczników XAML. `x:Reference` — Rozszerzenie znaczników jest obsługiwana przez [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) klasy; `Binding` jest obsługiwana przez [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) klasy. Jako plik XML wskazać prefiksy przestrzeni nazw, `x:Reference` jest częścią specyfikacji języka XAML 2009, podczas gdy `Binding` wchodzi w skład platformy Xamarin.Forms. Należy zauważyć, że nie znaki cudzysłowu są wyświetlane w nawiasach klamrowych.

Ułatwia zapomnij `x:Reference` — rozszerzenie znaczników podczas ustawiania `BindingContext`. Jest to często przez pomyłkę ustaw właściwość bezpośrednio na nazwę źródła powiązanie następująco:

```xaml
BindingContext="slider"
```

Ale są nieprawidłowe. Ustawia tego znacznika `BindingContext` właściwości `string` obiektu, którego znaki pisowni "suwaka"!

Należy zauważyć, że właściwość source zostanie określony z [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) właściwość `BindingExtension`, która odpowiada [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) właściwość [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) klasy.

Kod znaczników wyświetlany na **podstawowe powiązanie XAML** strony można uprościć: rozszerzenia znaczników XAML, takich jak `x:Reference` i `Binding` może mieć *właściwości content* zdefiniowanych atrybutów, które dla XAML rozszerzenia znaczników oznacza, że nazwa właściwości nie muszą się pojawić. `Name` Właściwość jest właściwość content `x:Reference`i `Path` właściwość jest właściwość content `Binding`, co oznacza, że mogą zostać usunięte z wyrażeń:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>Powiązania bez kontekstu powiązania

`BindingContext` Właściwości jest istotnym elementem powiązań danych, ale nie zawsze jest konieczne. Zamiast tego można określić obiektu źródłowego w `SetBinding` wywołania lub `Binding` — rozszerzenie znaczników.

To jest przedstawiona w **zamiast kodu — wiązanie** próbki. Plik XAML jest podobny do **podstawowe powiązanie kodu** przykładowe z wyjątkiem `Slider` zdefiniowano w celu sterowania `Scale` właściwość `Label`. Z tego powodu `Slider` jest ustawiony dla zakresu &ndash;2 do 2:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeCodeBindingPage"
             Title="Alternative Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Plik CodeBehind ustawia powiązanie z [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) metody zdefiniowanej przez `BindableObject`. Argument jest [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) dla [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) klasy:

```csharp
public partial class AlternativeCodeBindingPage : ContentPage
{
    public AlternativeCodeBindingPage()
    {
        InitializeComponent();

        label.SetBinding(Label.ScaleProperty, new Binding("Value", source: slider));
    }
}
```

`Binding` Konstruktor ma następujące parametry 6, więc `source` parametr zostanie określony z argumentem. Argument jest `slider` obiektu. 

Uruchomiony ten program może być nieco zaskakująco:

[![Powiązanie alternatywnego kodu](basic-bindings-images/alternativecodebinding-small.png "powiązania alternatywnego kodu")](basic-bindings-images/alternativecodebinding-large.png "powiązania alternatywnego kodu")

Na ekranie systemu iOS po lewej stronie zostaną wyświetlone wyglądu ekranu najpierw zostanie wyświetlona strona. Gdzie jest `Label`? 

Jest to problem, że `Slider` ma początkowej wartości 0. Powoduje to `Scale` właściwość `Label` należy również ustawić 0, zastępując wartość domyślną 1. Powoduje to `Label` są początkowo niewidoczne. Zgodnie z systemami Android i Windows platformy Uniwersalnej zrzuty ekranu pokazują, można manipulować `Slider` dokonanie `Label` pojawiają się ponownie, ale jego początkowej likwidacji jest niejasna.

Dowiesz się w [kolejnym artykule](binding-mode.md) sposób uniknąć tego problemu przez inicjowanie `Slider` z wartością domyślną `Scale` właściwości.

**Powiązania XAML alternatywna** strony zawiera równoważne powiązanie wyłącznie w języku XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeXamlBindingPage"
             Title="Alternative XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="{Binding Source={x:Reference slider},
                               Path=Value}" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Teraz `Binding` — rozszerzenie znaczników ma dwie właściwości ustawione, `Source` i `Path`, rozdzielone przecinkami. One może występować w jednym wierszu, jeśli wolisz:

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source` Ustawiono właściwość do osadzonego `x:Reference` rozszerzeniu znacznika, które w przeciwnym razie ma takiej samej składni jak ustawienie `BindingContext`. Zwróć uwagę, czy znaki cudzysłowu są wyświetlane w nawiasach klamrowych, i że dwie właściwości muszą być oddzielone przecinkami.

Właściwość content `Binding` — rozszerzenie znaczników jest `Path`, ale `Path=` część rozszerzenia znaczników tylko mogą zostać usunięte, jeśli jest pierwszą właściwością w wyrażeniu. Aby wyeliminować `Path=` części, trzeba wymienić dwie właściwości:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

Chociaż rozszerzenia znaczników XAML są zwykle rozdzielone nawiasy klamrowe, mogą również być wyrażone jako elementy obiektu:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Source="{x:Reference slider}"
                 Path="Value" />
    </Label.Scale>
</Label>
``` 

Teraz `Source` i `Path` właściwości są prawidłowe atrybuty XAML: znaki cudzysłowu są wyświetlane wartości i atrybuty nie są oddzielone przecinkami. `x:Reference` — Rozszerzenie znaczników może stać się również element obiektu:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Path="Value">
            <Binding.Source>
                <x:Reference Name="slider" />
            </Binding.Source>
        </Binding>
    </Label.Scale>
</Label>
```

Ta składnia nie jest typowe, ale czasami jest to konieczne w przypadku obiektów złożonych.

W przykładach pokazano wykonanej do tej pory ustawiana `BindingContext` właściwości i `Source` właściwość `Binding` do `x:Reference` — rozszerzenie znaczników do innego widoku na stronie odwołania. Te dwie właściwości są typu `Object`, i może należeć do dowolnego obiektu, który zawiera właściwości, które są odpowiednie dla powiązania źródła. 

W artykułach z wyprzedzeniem, dowiesz się, że można ustawić `BindingContext` lub `Source` właściwości `x:Static` — rozszerzenie znaczników odwoływać się do wartości właściwości statycznej lub pola, lub `StaticResource` — rozszerzenie znaczników do odwołuje się do obiektu przechowywane w Słownik zasobów lub bezpośrednio do obiektu, który jest zazwyczaj (ale nie zawsze) wystąpienia ViewModel. 

`BindingContext` Można również ustawić właściwość `Binding` obiekt, aby `Source` i `Path` właściwości `Binding` Definiowanie kontekstu powiązania.

## <a name="binding-context-inheritance"></a>Dziedziczenie kontekstu powiązania

W tym artykule przedstawiono czy można określić za pomocą obiektu źródłowego `BindingContext` właściwości lub `Source` właściwość `Binding` obiektu. Jeśli są ustawione, `Source` właściwość `Binding` ma pierwszeństwo przed `BindingContext`.

`BindingContext` Właściwość ma bardzo istotne cechy:

*Ustawienie `BindingContext` właściwość jest dziedziczona przez drzewa wizualnego.*

Jak można zauważyć, może to być bardzo przydatne, dla uproszczenia wyrażenia wiązania, a w niektórych przypadkach &mdash; szczególnie w scenariuszach Model-View-ViewModel (MVVM) &mdash; niezbędne jest.

**Dziedziczenia kontekstu powiązania** próbka jest prosty pokaz z dziedziczenia kontekstu powiązania:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BindingContextInheritancePage"
             Title="BindingContext Inheritance">
    <StackLayout Padding="10">

        <StackLayout VerticalOptions="FillAndExpand"
                     BindingContext="{x:Reference slider}">
            
            <Label Text="TEXT"
                   FontSize="80"
                   HorizontalOptions="Center"
                   VerticalOptions="EndAndExpand"
                   Rotation="{Binding Value}" />

            <BoxView Color="#800000FF"
                     WidthRequest="180"
                     HeightRequest="40"
                     HorizontalOptions="Center"
                     VerticalOptions="StartAndExpand"
                     Rotation="{Binding Value}" />
        </StackLayout>

        <Slider x:Name="slider" 
                Maximum="360" />
        
    </StackLayout>
</ContentPage>
```

`BindingContext` Właściwość `StackLayout` ustawiono `slider` obiektu. Ten kontekst powiązania jest dziedziczona przez oba `Label` i `BoxView`, zarówno z których ich `Rotation` właściwości `Value` właściwości `Slider`: 

[![Powiązanie dziedziczenia kontekstu](basic-bindings-images/bindingcontextinheritance-small.png "powiązanie dziedziczenia kontekstu")](basic-bindings-images/bindingcontextinheritance-large.png "powiązanie dziedziczenia kontekstu")

W [kolejnym artykule](binding-mode.md), zobaczysz jak *tryb wiązania* można zmienić przepływ danych między obiektami źródłowe i docelowe.


## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
