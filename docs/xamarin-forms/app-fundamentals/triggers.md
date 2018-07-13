---
title: Wyzwalacze zestawu narzędzi Xamarin.Forms
description: W tym artykule wyjaśniono, jak reagować na zmiany w interfejsie użytkownika przy użyciu XAML przy użyciu zestawu narzędzi Xamarin.Forms wyzwalaczy. Wyzwalacze umożliwiają express akcji deklaratywnie w XAML wygląd formantów na podstawie zdarzeń lub zmiany właściwości.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 954a0967e034e0321964e12ca0725ae2a85e3bc6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995540"
---
# <a name="xamarinforms-triggers"></a>Wyzwalacze zestawu narzędzi Xamarin.Forms

Wyzwalacze umożliwiają express akcji deklaratywnie w XAML wygląd formantów na podstawie zdarzeń lub zmiany właściwości.

Możesz przypisać wyzwalacza bezpośrednio z kontrolką lub dodać go do słownika zasobów strony lub na poziomie aplikacji mają być stosowane do wielu kontrolek.

Istnieją cztery typy wyzwalaczy:

* [Wyzwalacz właściwości](#property) -występuje, gdy właściwość na formancie jest równa określonej wartości.

* [Wyzwalacz danych](#data) — używa danych powiązania do wyzwalania na podstawie właściwości innej kontrolki.

* [Wyzwalacz zdarzenia](#event) -występuje, gdy wystąpi zdarzenie na formancie.

* [Wiele wyzwalaczy](#multi) — zezwala na wiele warunków wyzwalacza, należy ustawić przed akcją.

<a name="property" />

## <a name="property-triggers"></a>Wyzwalacze właściwości

Proste wyzwalacza mogą być wyrażone w wyłącznie w XAML, dodając `Trigger` elementu do kontrolki wyzwala zbieranie.
Wyzwalacz, który zmienia się w tym przykładzie `Entry` kolor tła, po odebraniu koncentracji uwagi:

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
             Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Ważne elementy tego wyzwalacza deklaracji są następujące:

* **TargetType** — typ formantu dotyczy wyzwalacza.

* **Właściwość** — właściwości formantu, który jest monitorowany.

* **Wartość** -wartości, jeśli występuje on dla właściwości monitorowanych, który powoduje, że wyzwalacz, aby aktywować.

* **Metoda ustawiająca** — Kolekcja `Setter` elementów może zostać dodany, a po spełnieniu warunku wyzwalania. Należy określić `Property` i `Value` do ustawienia.

* **EnterActions i ExitActions** (niewyświetlany) — są zapisywane w kodzie i mogą być używane w uzupełnieniu do (lub zamiast) `Setter` elementów. Są one [opisanych poniżej](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Stosowanie wyzwalacz za pomocą stylu

Wyzwalacze można dodać do `Style` deklaracji w kontrolce, na stronie lub aplikacji `ResourceDictionary`. W tym przykładzie deklaruje stylu niejawnego (tj. nie `Key` ustawiono) co oznacza, że zostaną zastosowane do wszystkich `Entry` formantów na stronie.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

<a name="data" />

## <a name="data-triggers"></a>Wyzwalacze danych

Wyzwalacze danych używać powiązanie danych do monitorowania innej kontrolki, aby spowodować `Setter`s, aby zostać wywołana. Zamiast `Property` atrybutu w wyzwalaczu właściwość, ustaw `Binding` atrybutu do monitorowania dla określonej wartości.

W poniższym przykładzie użyto składnia wiązania danych `{Binding Source={x:Reference entry}, Path=Text.Length}` czyli jak nazywamy inna kontrolka właściwości. Jeśli długość `entry` wynosi zero, wyzwalacz zostanie aktywowany. W tym przykładzie wyzwalacza wyłącza przycisk, gdy wartość wejściowa jest pusta.

```xaml
<!-- the x:Name is referenced below in DataTrigger-->
<!-- tip: make sure to set the Text="" (or some other default) -->
<Entry x:Name="entry"
       Text=""
       Placeholder="required field" />

<Button x:Name="button" Text="Save"
        FontSize="Large"
        HorizontalOptions="Center">
    <Button.Triggers>
        <DataTrigger TargetType="Button"
                     Binding="{Binding Source={x:Reference entry},
                                       Path=Text.Length}"
                     Value="0">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </Button.Triggers>
</Button>
```

Porada: podczas obliczania `Path=Text.Length` zawsze dostarczaj wartość domyślną dla właściwości docelowej (np.) `Text=""`), ponieważ w przeciwnym razie będzie `null` i wyzwalacz nie będzie działać, takie jak oczekujesz.

Oprócz określenia `Setter`s można też podać [ `EnterActions` i `ExitActions` ](#enterexit).

<a name="event" />

## <a name="event-triggers"></a>Wyzwalacze zdarzeń

`EventTrigger` Element wymaga tylko `Event` właściwości, takie jak `"Clicked"` w poniższym przykładzie.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Należy zauważyć, że istnieją nie `Setter` elementów, ale raczej odwołanie do klasy zdefiniowanej przez `local:NumericValidationTriggerAction` wymagająca `xmlns:local` deklaruje się w stronę użytkownika XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Ta sama klasa implementuje `TriggerAction` co oznacza, że powinno zapewniać zastąpienie `Invoke` metodę, która jest wywoływana zawsze wtedy, gdy wystąpi zdarzenie wyzwalacza.

Wykonanie akcji wyzwalacza należy:

* Implementowanie ogólnego `TriggerAction<T>` klasy za pomocą parametru ogólnego odpowiedni dla typu wyzwalacza zostaną zastosowane do formantu. Można użyć superklas, takich jak `VisualElement` do zapisania Wyzwalaj akcje, które współpracuje z różnymi formantów lub określ typ kontrolki, takie jak `Entry`.

* Zastąp `Invoke` metody — jest to zawsze wtedy, gdy są spełnione kryteria wyzwalacza.

* Opcjonalnie ujawnić właściwości, które można ustawiać w XAML, gdy wyzwalacz jest zadeklarowany (takie jak `Anchor`, `Scale`, i `Length` w tym przykładzie).

```csharp
public class NumericValidationTriggerAction : TriggerAction<Entry>
{
    protected override void Invoke (Entry entry)
    {
        double result;
        bool isValid = Double.TryParse (entry.Text, out result);
        entry.TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Właściwości ujawnione przez akcję wyzwalacza można ustawić w deklaracji XAML w następujący sposób:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Należy zachować ostrożność podczas udostępniania wyzwalaczy w `ResourceDictionary`, jedno wystąpienie będzie współdzielona przez formanty, stan, który został skonfigurowany po dotyczą ich wszystkich.

Należy pamiętać, że nie obsługują wyzwalacze zdarzeń `EnterActions` i `ExitActions` [opisanych poniżej](#enterexit).    

<a name="multi" />

## <a name="multi-triggers"></a>Wiele wyzwalaczy

A `MultiTrigger` wygląda podobnie do `Trigger` lub `DataTrigger` z wyjątkiem może istnieć więcej niż jeden warunek. Wszystkie warunki muszą być spełnione przed `Setter`s są wyzwalane.

Oto przykład wyzwalacza dla przycisku, który wiąże się na dwa różne dane wejściowe (`email` i `phone`):

```xaml
<MultiTrigger TargetType="Button">
    <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference email},
                                   Path=Text.Length}"
                               Value="0" />
        <BindingCondition Binding="{Binding Source={x:Reference phone},
                                   Path=Text.Length}"
                               Value="0" />
    </MultiTrigger.Conditions>

  <Setter Property="IsEnabled" Value="False" />
    <!-- multiple Setter elements are allowed -->
</MultiTrigger>
```

`Conditions` Kolekcji mogą również zawierać `PropertyCondition` elementów, takich jak to:

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>Tworzenie wyzwalacza multi "wymagane są wszystkie"

Wyzwalacz multi tylko zaktualizowanie kontrolki, gdy są spełnione wszystkie warunki. Testowanie dla "wszystkich długości pola są zero" (na przykład strony logowania, gdzie wszystkie dane wejściowe muszą być kompletne) jest trudne, ponieważ mają warunek "gdzie Text.Length > 0", ale to nie może być wyrażona w XAML.

Można to zrobić za pomocą `IValueConverter`. Kod konwerter poniżej przekształcenia `Text.Length` powiązania do `bool` oznacza to, czy pole jest puste:


```csharp
public class MultiTriggerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        if ((int)value > 0) // length > 0 ?
            return true;            // some data has been entered
        else
            return false;            // input is empty
    }

    public object ConvertBack(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        throw new NotSupportedException ();
    }
}
```

Aby użyć tego konwertera w wyzwalaczu multi, go najpierw dodać do strony słownika zasobów (wraz z niestandardowego `xmlns:local` definicję przestrzeni nazw):

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

Poniżej przedstawiono XAML. Zwróć uwagę na następujące różnice w pierwszym przykładzie wyzwalacza multi:

* Przycisk ma `IsEnabled="false"` domyślnie.
* Warunki wyzwalania multi używać konwertera, aby włączyć `Text.Length` wartość na wartość logiczną.
* Jeśli wszystkie warunki są `true`, metody ustawiającej sprawia, że przycisk `IsEnabled` właściwość `true`.

```xaml
<Entry x:Name="user" Text="" Placeholder="user name" />

<Entry x:Name="pwd" Text="" Placeholder="password" />

<Button x:Name="loginButton" Text="Login"
        FontSize="Large"
        HorizontalOptions="Center"
        IsEnabled="false">
  <Button.Triggers>
    <MultiTrigger TargetType="Button">
      <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference user},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
        <BindingCondition Binding="{Binding Source={x:Reference pwd},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
      </MultiTrigger.Conditions>
      <Setter Property="IsEnabled" Value="True" />
    </MultiTrigger>
  </Button.Triggers>
</Button>
```

Te zrzuty ekranu pokazują różnicę między dwóch multi wyzwalacza przykładach. W górnej części ekrany wprowadzania tekstu tylko w jednej `Entry` jest wystarczający, aby umożliwić **Zapisz** przycisku.
W dolnej części ekrany **logowania** przycisk pozostaje nieaktywna, dopóki oba pola zawierają dane.


![](triggers-images/multi-requireall.png "Przykłady multiTrigger")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions i ExitActions

Innym sposobem wdrożenia zmiany w przypadku wyzwalacza jest przez dodanie `EnterActions` i `ExitActions` kolekcji i określając `TriggerAction<T>` implementacji.

Możesz podać *zarówno* `EnterActions` i `ExitActions` także `Setter`s w wyzwalaczu, ale należy pamiętać, który `Setter`s są nazywane natychmiast (nie oczekują `EnterAction` lub `ExitAction` do Zakończenie). Można również wykonywać wszystkie elementy w kodzie i nie używać `Setter`s w ogóle.

```xaml
<Entry Placeholder="enter job title">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="Entry.IsFocused" Value="True">
            <Trigger.EnterActions>
                <local:FadeTriggerAction StartsFrom="0"" />
            </Trigger.EnterActions>

            <Trigger.ExitActions>
                <local:FadeTriggerAction StartsFrom="1" />
            </Trigger.ExitActions>
                        <!-- You can use both Enter/Exit and Setter together if required -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Jak zawsze, gdy klasa jest przywoływany w XAML należy zadeklarować przestrzeni nazw takich jak `xmlns:local` jak pokazano poniżej:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

`FadeTriggerAction` Kodu jest pokazany poniżej:

```csharp
public class FadeTriggerAction : TriggerAction<VisualElement>
{
    public FadeTriggerAction() {}

    public int StartsFrom { set; get; }

    protected override void Invoke (VisualElement visual)
    {
            visual.Animate("", new Animation( (d)=>{
                var val = StartsFrom==1 ? d : 1-d;
                visual.BackgroundColor = Color.FromRgb(1, val, 1);

            }),
            length:1000, // milliseconds
            easing: Easing.Linear);
    }
}
```

Uwaga: `EnterActions` i `ExitActions` są ignorowane na **wyzwalaczy zdarzeń**.



## <a name="related-links"></a>Linki pokrewne

- [Przykładowe wyzwalaczy](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](xref:Xamarin.Forms.TriggerAction`1)
