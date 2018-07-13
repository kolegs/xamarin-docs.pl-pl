---
title: Powiązania Basic zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak używać zestawu narzędzi Xamarin.Forms wiązania danych, łączenia parze właściwości pomiędzy dwoma obiektami, co najmniej jednym z nich jest zazwyczaj obiekt interfejsu użytkownika. Te dwa obiekty są nazywane źródłowe i docelowe.
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 16d1970b5e9d8f9c2b7c8be875c81136525c4fb7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998075"
---
# <a name="xamarinforms-basic-bindings"></a>Powiązania Basic zestawu narzędzi Xamarin.Forms

Powiązanie danych zestawu narzędzi Xamarin.Forms łączy parze właściwości pomiędzy dwoma obiektami, co najmniej jeden z nich jest zazwyczaj obiekt interfejsu użytkownika. Te dwa obiekty są nazywane *docelowej* i *źródła*:

- *Docelowej* znajduje się obiekt (i właściwości) w zestawie jest powiązanie danych.
- *Źródła* jest przywoływany przez powiązanie danych obiektu (i właściwości).

Czasami może być nieco mylące wykonywania tego rozróżnienia: W najprostszym przypadku przepływu danych ze źródła do docelowego, co oznacza, że właściwość docelowa jest wartość od wartości właściwości source. Jednak w niektórych przypadkach można również przepływ danych z obiektu docelowego do źródła lub w obu kierunkach. Aby uniknąć nieporozumień, należy pamiętać o tym, której celem jest zawsze obiektu, na którym powiązanie danych jest ustawiona, nawet wtedy, gdy jej jest udostępnienie danych, a nie odbiera dane.

## <a name="bindings-with-a-binding-context"></a>Tylko powiązania z kontekstu powiązania

Chociaż powiązań danych zazwyczaj są określone w całości XAML, jest istotne, aby zobaczyć, powiązań danych w kodzie. **Podstawowe powiązanie kodu** strona zawiera plik XAML z `Label` i `Slider`:

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

`Slider` Jest ustawiony w zakresie od 0 do 360. Celem tego programu jest obracanie `Label` przez operacje `Slider`.

Bez powiązania danych, należy ustawić `ValueChanged` zdarzenia `Slider` do obsługi zdarzeń, który uzyskuje dostęp do `Value` właściwość `Slider` i ustawia tę wartość na `Rotation` właściwość `Label`. Powiązanie danych automatyzuje zadania; Program obsługi zdarzeń i kod, znajdujący się w nim nie są już potrzebne.

Możesz określić powiązanie wystąpienia każdej klasy, która pochodzi od klasy [ `BindableObject` ](xref:Xamarin.Forms.BindableObject), która obejmuje `Element`, `VisualElement`, `View`, i `View` pochodnych.  Wiązanie jest zawsze ustawiony w obiekcie docelowym. Powiązanie odwołuje się do obiektu źródłowego. Aby ustawić powiązanie danych, należy zastosować następujące dwa elementy członkowskie klasy docelowej:

- [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Właściwość określa obiektu źródłowego.
- [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) Metody Określa właściwość target i source — właściwość.

W tym przykładzie `Label` jest cel wiążący i `Slider` jest źródło wiążące. Zmiany w `Slider` źródła mają wpływ na obrót `Label` docelowej. Przepływy danych ze źródła do docelowego.

`SetBinding` Metody zdefiniowanej przez `BindableObject` ma argument typu [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) z którego [ `Binding` ](xref:Xamarin.Forms.Binding) pochodzi z klasy, ale istnieją inne `SetBinding` metody zdefiniowane przez [ `BindableObjectExtensions` ](xref:Xamarin.Forms.BindableObjectExtensions) klasy. Plik związany z kodem w **podstawowe powiązanie kodu** próbki prostsze [ `SetBinding` ](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) metody rozszerzenia z tej klasy.

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

`Label` Obiekt jest cel wiążący, dlatego ten typ obiektu, na którym ta właściwość jest ustawiona i jest wywoływana metoda. `BindingContext` Właściwość wskazuje źródło powiązania, w którym jest `Slider`.

`SetBinding` Metoda jest wywoływana w elemencie docelowym powiązania, ale określa zarówno właściwość docelowa, jak i właściwość source. Właściwość docelowa jest określony jako `BindableProperty` obiektu: `Label.RotationProperty`. Właściwość źródło jest określone jako ciąg i wskazuje `Value` właściwość `Slider`.

`SetBinding` Metoda, co spowoduje wyświetlenie jedną z najważniejszych zasad powiązań danych:

*Właściwość docelowa musi być wspierana przez właściwości możliwej do wiązania.*

Ta reguła oznacza, że obiekt docelowy musi być wystąpieniem klasy, która pochodzi od klasy `BindableObject`. Zobacz [ **właściwości możliwej do wiązania** ](~/xamarin-forms/xaml/bindable-properties.md) artykuł, aby zapoznać się z omówieniem możliwej do wiązania obiektów i właściwości możliwej do wiązania.

Nie ma takich reguły dla właściwości source, która jest określana jako ciąg. Wewnętrznie odbicie umożliwia dostęp do właściwości rzeczywistych. W tym konkretnym przypadku jednak `Value` właściwość jest także wspierana przez właściwości możliwej do wiązania.

Kod może być nieco uproszczona: `RotationProperty` właściwości możliwej do wiązania jest definiowany przez `VisualElement`i dziedziczone przez `Label` i `ContentPage` , więc nazwa klasy nie jest wymagane w `SetBinding` wywołania:

```csharp
label.SetBinding(RotationProperty, "Value");
```

Łącznie z nazwą klasy jest dobrym przypomnienie obiektu docelowego.

Jak można manipulować `Slider`, `Label` obraca się w związku z tym:

[![Kod Basice powiązanie](basic-bindings-images/basiccodebinding-small.png "podstawowy kod powiązania")](basic-bindings-images/basiccodebinding-large.png#lightbox "podstawowy kod powiązania")

**Podstawowe powiązanie Xaml** strony jest taka sama jak **podstawowe powiązanie kodu** z tą różnicą, że definiuje powiązanie danych w XAML:

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

Podobnie jak w przypadku kodu, wiązanie danych jest ustawiona na obiekt docelowy, który jest `Label`. Dwa rozszerzeń struktury znaczników XAML są zaangażowane. Poniżej przedstawiono rozpoznawalny za ograniczniki nawiasów klamrowych:

- `x:Reference` — Rozszerzenie znaczników musi odwoływać się do obiektu źródłowego, który jest `Slider` o nazwie `slider`.
- `Binding` Łączy rozszerzenia znaczników `Rotation` właściwość `Label` do `Value` właściwość `Slider`.

Zapoznaj się z artykułem [rozszerzeń struktury znaczników XAML](~/xamarin-forms/xaml/markup-extensions/index.md) Aby uzyskać więcej informacji na temat rozszerzeń struktury znaczników XAML. `x:Reference` — Rozszerzenie znaczników jest obsługiwana przez [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension) klasy; `Binding` jest obsługiwana przez [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension) klasy. Jako plik XML wskazują prefiksy przestrzeni nazw, `x:Reference` jest częścią specyfikacji XAML 2009, podczas gdy `Binding` jest częścią zestawu narzędzi Xamarin.Forms. Należy zauważyć, że znaki cudzysłowu są wyświetlane w nawiasy klamrowe.

Można łatwo zapomnieć `x:Reference` — rozszerzenie znaczników podczas ustawiania `BindingContext`. Powszechne jest wprowadzanie przez pomyłkę ustaw właściwość bezpośrednio na nazwę źródło wiążące następująco:

```xaml
BindingContext="slider"
```

Ale nie jest to odpowiednie. Ustawia tego znacznika `BindingContext` właściwość `string` obiektu, którego znaki pisowni "suwaka"!

Należy zauważyć, że właściwość source jest określony za pomocą [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path) właściwość `BindingExtension`, który odpowiada za pomocą [ `Path` ](xref:Xamarin.Forms.Binding.Path) właściwość [ `Binding` ](xref:Xamarin.Forms.Binding) klasy.

Znaczniki wyświetlane na **podstawowe powiązania w XAML** strony można uprościć: rozszerzeń struktury znaczników XAML, takich jak `x:Reference` i `Binding` może mieć *zawartości właściwość* zdefiniowane atrybuty, które w XAML rozszerzenia znaczników oznacza, że nazwa właściwości nie musi występować. `Name` Właściwość jest właściwość content `x:Reference`i `Path` właściwość jest właściwość content `Binding`, co oznacza, że mogą być one wyeliminowane z wyrażeń:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>Powiązania bez kontekstu powiązania

`BindingContext` Właściwość jest ważnym elementem powiązań danych, ale nie zawsze jest konieczne. Zamiast tego można określić obiekt źródłowy w `SetBinding` wywołania lub `Binding` — rozszerzenie znaczników.

Jest to zaprezentowane w **powiązanie kodu zamiast** próbki. Plik XAML jest podobne do **podstawowe powiązanie kodu** przykładowy, chyba że `Slider` jest zdefiniowany dla formantu `Scale` właściwość `Label`. Z tego powodu `Slider` ustawiono na szeroką gamę &ndash;2 do 2:

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

Plik związany z kodem ustawia powiązanie z [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) metody zdefiniowanej przez `BindableObject`. Argument jest [Konstruktor](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) dla [ `Binding` ](xref:Xamarin.Forms.Binding) klasy:

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

`Binding` Konstruktor ma 6 parametrów, więc `source` parametr jest określony za pomocą argumentu nazwanego. Argument jest `slider` obiektu.

Program został uruchomiony, może być nieco Zaskakujące:

[![Powiązanie alternatywnej kodu](basic-bindings-images/alternativecodebinding-small.png "powiązania alternatywnej kodu")](basic-bindings-images/alternativecodebinding-large.png#lightbox "powiązania alternatywnej kodu")

Na ekranie dla systemu iOS po lewej stronie zostaną wyświetlone wyglądu ekranu najpierw zostanie wyświetlona. Gdzie jest `Label`?

Problem jest to, że `Slider` ma początkowa wartość 0. Powoduje to, że `Scale` właściwość `Label` również należy ustawić na wartość 0, zastępując wartość domyślną 1. Skutkuje to `Label` są początkowo niewidoczne. Jak wykazać, zrzuty ekranu dla systemów Android i platformy uniwersalnej Windows (UWP), można manipulować `Slider` się `Label` pojawiają się ponownie, ale jego początkowego znikanie jest niejasna.

Dowiesz się w [następnego artykułu](binding-mode.md) sposób uniknąć tego problemu przez inicjowanie `Slider` z wartością domyślną `Scale` właściwości.

**Powiązania XAML alternatywna** strona zawiera równoważne powiązania w XAML:

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

Teraz `Binding` — rozszerzenie znaczników ma dwie właściwości ustawione, `Source` i `Path`, oddzielone przecinkami. One może znajdować się w tym samym wierszu, jeśli użytkownik sobie tego życzy:

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source` Właściwość jest ustawiona na osadzonych `x:Reference` rozszerzenia znaczników, które w przeciwnym razie ma tej samej składni jako ustawienie `BindingContext`. Zwróć uwagę, że znaki cudzysłowu są wyświetlane w obrębie nawiasów klamrowych i że dwie właściwości muszą być rozdzielone przecinkami.

Właściwość content `Binding` — rozszerzenie znaczników jest `Path`, ale `Path=` część rozszerzenia znaczników mogą zostać usunięte, jeśli pierwsza właściwość w wyrażeniu. Aby wyeliminować `Path=` strony, należy zamienić dwie właściwości:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

Mimo że rozszerzeń struktury znaczników XAML są zwykle rozdzielone nawiasów klamrowych, również mogą być wyrażone jako elementy obiektu:

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

Teraz `Source` i `Path` właściwości są regularnie atrybuty XAML: wartości są wyświetlane w cudzysłowach i atrybuty nie są rozdzielone przecinkami. `x:Reference` — Rozszerzenie znaczników może również stać się element obiektu:

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

Ta składnia nie jest wspólnego, ale czasami jest konieczne w przypadku złożonych obiektów.

Ustaw przykładów pokazanych w do tej pory `BindingContext` właściwości i `Source` właściwość `Binding` do `x:Reference` — rozszerzenie znaczników można odwoływać się do innego widoku, na stronie. Te dwie właściwości są typu `Object`, i można je ustawić do dowolnego obiektu, który zawiera właściwości, które są odpowiednie na potrzeby powiązania źródeł.

W artykułach z wyprzedzeniem, dowiesz się, że można skonfigurować `BindingContext` lub `Source` właściwości `x:Static` — rozszerzenie znaczników odwoływać się do wartości pola, lub właściwość statyczna lub `StaticResource` — rozszerzenie znaczników do odwołują się do obiektu, przechowywane w Słownik zasobów lub bezpośrednio do obiektu, który jest zazwyczaj (ale nie zawsze) wystąpienia ViewModel.

`BindingContext` Właściwość można ustawić `Binding` obiektu, aby `Source` i `Path` właściwości `Binding` Definiowanie kontekstu powiązania.

## <a name="binding-context-inheritance"></a>Dziedziczenie kontekstu powiązania

W tym artykule, wiesz, że można określić za pomocą obiektu źródłowego `BindingContext` właściwości lub `Source` właściwość `Binding` obiektu. Jeśli obie są skonfigurowane, `Source` właściwość `Binding` ma pierwszeństwo przed `BindingContext`.

`BindingContext` Właściwość ma bardzo ważną cechą:

*Ustawienie `BindingContext` właściwość jest dziedziczona przez drzewo wizualne.*

Jak można zauważyć, może to być bardzo przydatne dla uproszczenia wyrażenia wiązania, a w niektórych przypadkach &mdash; szczególnie w scenariuszach Model-View-ViewModel (MVVM) &mdash; niezbędne jest.

**Dziedziczenia kontekstu powiązania** przykładowe dane stanowią prosty pokaz dziedziczenia kontekstu powiązania:

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

`BindingContext` Właściwość `StackLayout` ustawiono `slider` obiektu. Ten kontekst powiązania jest dziedziczona przez `Label` i `BoxView`, z którego mają ich `Rotation` właściwości ustawione na `Value` właściwość `Slider`:

[![Powiązanie dziedziczenia kontekstu](basic-bindings-images/bindingcontextinheritance-small.png "dziedziczenia kontekstu powiązania")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "powiązanie kontekstu dziedziczenia")

W [następnego artykułu](binding-mode.md), zobaczysz sposób, w jaki *tryb powiązania* można zmienić przepływ danych między obiekty źródłowe i docelowe.


## <a name="related-links"></a>Linki pokrewne

- [Prezentacja powiązania danych (przykład)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Rozdział dotyczący powiązania danych z książki Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
