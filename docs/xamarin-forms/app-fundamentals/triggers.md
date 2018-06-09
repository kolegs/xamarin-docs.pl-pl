---
title: Wyzwalacze platformy Xamarin.Forms
description: W tym artykule opisano sposób użycia platformy Xamarin.Forms wyzwalaczy na odpowiadanie na zmiany interfejsu użytkownika za pomocą języka XAML. Wyzwalacze umożliwiają express akcji deklaratywnie w języku XAML, które zmienić wygląd kontrolki oparte na zdarzeniach lub zmiany właściwości.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: b28ebb8845b7eae0d818e1279b4d6eaef4ad5b8b
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241438"
---
# <a name="xamarinforms-triggers"></a>Wyzwalacze platformy Xamarin.Forms

Wyzwalacze umożliwiają express akcji deklaratywnie w języku XAML, które zmienić wygląd kontrolki oparte na zdarzeniach lub zmiany właściwości.

Można przypisać wyzwalacz bezpośrednio do formantu, lub dodaj je do słownika zasobów strony lub na poziomie aplikacji ma zostać zastosowany do wielu formantów.

Istnieją cztery typy wyzwalaczy:

* [Wyzwalacza właściwości](#property) -występuje, gdy właściwość w formancie jest równa określonej wartości.

* [Wyzwalacz danych](#data) — wiązanie do wyzwalania na podstawie właściwości inny formant danych używa.

* [Wyzwalacz zdarzenia](#event) -występuje, gdy wystąpi zdarzenie w formancie.

* [Wyzwalacz Multi](#multi) — umożliwia wielu warunki wyzwalania ustawić przed wywołaniem metody akcji.

<a name="property" />

## <a name="property-triggers"></a>Wyzwalacze właściwości

Proste wyzwalacza, może zostać wyrażona wyłącznie w języku XAML, dodawanie `Trigger` elementu do formantu wyzwala kolekcji.
W tym przykładzie pokazano wyzwalacz, który zmienia `Entry` kolor tła, gdy odbierze fokus:

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

Ważne części deklaracji wyzwalacza są:

* **Element TargetType** — typ formantu, której wyzwalacz dotyczy.

* **Właściwość** -właściwości formantu, który jest monitorowany.

* **Wartość** -wartość w przypadku monitorowanych właściwości, która powoduje trigger, aby aktywować.

* **Metoda ustawiająca** — Kolekcja `Setter` elementy mogą zostać dodane, a po spełnieniu warunku wyzwalania. Należy określić `Property` i `Value` można ustawić.

* **Akcji EnterActions i ExitActions** (tego nie pokazano) — są zapisywane w kodzie i mogą być używane w uzupełnieniu do (lub zamiast) `Setter` elementów. Są one [opisanych poniżej](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Stosowanie wyzwalacz za pomocą stylu

Wyzwalacze można również dodać do `Style` deklaracji w formancie, na stronie lub aplikacji `ResourceDictionary`. W tym przykładzie deklaruje stylu niejawne (ie. nie `Key` ustawiono) co oznacza, że będą stosowane do wszystkich `Entry` kontrolki na stronie.

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

Wyzwalacze danych używać powiązania danych do monitorowania inny formant powoduje `Setter`s, aby jest wywoływana. Zamiast `Property` atrybutu w wyzwalaczu właściwość, ustaw `Binding` atrybut do monitorowania dla określonej wartości.

W poniższym przykładzie jest używana składnia wiązania danych `{Binding Source={x:Reference entry}, Path=Text.Length}` czyli jak możemy odwoływać się do właściwości inny formant. Gdy długość `entry` wynosi zero, wyzwalacz został aktywowany. W tym przykładzie wyzwalacza wyłącza przycisk, gdy wartość wejściowa jest pusta.

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

Porada: podczas obliczania `Path=Text.Length` zawsze podawać wartości domyślnej dla docelowej właściwości (np.) `Text=""`), ponieważ w przeciwnym razie będzie `null` i wyzwalacz nie działa jak oczekujesz.

Oprócz określenia `Setter`s można też podać [ `EnterActions` i `ExitActions` ](#enterexit).

<a name="event" />

## <a name="event-triggers"></a>Wyzwalacze zdarzeń

`EventTrigger` Element wymaga tylko `Event` właściwości, takie jak `"Clicked"` w poniższym przykładzie.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Należy zauważyć, że istnieją nie `Setter` elementów, ale raczej odwołania do klasy zdefiniowanej przez `local:NumericValidationTriggerAction` co wymaga `xmlns:local` na stronie użytkownika XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Ta sama klasa implementuje `TriggerAction` co oznacza, że powinien udostępniać zastąpienie `Invoke` metodę, która jest wywoływana, gdy wystąpi zdarzenie wyzwalacza.

Wykonanie akcji wyzwalacza następujące czynności:

* Implementowanie ogólnego `TriggerAction<T>` klasy z parametru ogólnego o typ wyzwalacza zostaną zastosowane dla formantu. Można użyć nadklas, takich jak `VisualElement` można zapisać wyzwalacza akcje, które obsługuje wiele różnych mechanizmów, lub określ typ formantu, takich jak `Entry`.

* Zastąpienie `Invoke` metody — to jest wywoływana, gdy są spełnione kryteria wyzwalacza.

* Opcjonalnie udostępniają właściwości, które można ustawić w języku XAML, gdy jest zadeklarowany jako wyzwalacz (takich jak `Anchor`, `Scale`, i `Length` w tym przykładzie).

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

Właściwości udostępnianych przez akcję wyzwalacza można ustawić w deklaracji XAML w następujący sposób:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Należy zachować ostrożność podczas udostępniania Wyzwalacze w `ResourceDictionary`, jedno wystąpienie będzie współdzielona przez formanty każdy stan, który skonfigurowano na raz będzie dotyczyć wszystkich.

Należy pamiętać, że wyzwalacze zdarzeń nie obsługują `EnterActions` i `ExitActions` [opisanych poniżej](#enterexit).    

<a name="multi" />

## <a name="multi-triggers"></a>Obsługa wielu wyzwalaczy

A `MultiTrigger` wygląda podobnie do `Trigger` lub `DataTrigger` z wyjątkiem może istnieć więcej niż jednego warunku. Wszystkie warunki należy spełnić przed `Setter`s są wyzwalane.

Oto przykład wyzwalacza dla przycisku, który jest powiązany z dwóch różnych komponentów (`email` i `phone`):

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

`Conditions` Kolekcji może także zawierać `PropertyCondition` elementów, takich jak to:

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>Tworzenie wyzwalacza multi "Wymagaj wszystkie"

Wyzwalacz multi tylko aktualizuje kontroli, gdy są spełnione wszystkie warunki. Testowanie "wszystkie pola długości to zero" (na przykład strony logowania, gdy wszystkie dane wejściowe muszą być zakończone) jest trudnych, ponieważ ma warunek "gdzie Text.Length > 0", ale to nie można wyrazić w języku XAML.

Można to zrobić z `IValueConverter`. Kod konwertera poniżej transformacje `Text.Length` powiązanie do `bool` wskazująca, czy pole jest puste:


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

Aby użyć tego konwertera w wyzwalaczu multi, najpierw dodaj go do słownika zasobów strony (wraz z niestandardowego `xmlns:local` definicję przestrzeni nazw):

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

Poniżej przedstawiono XAML. Należy uwzględnić następujące różnice w pierwszym przykładzie wyzwalacza multi:

* Przycisk ma `IsEnabled="false"` ustawieniem domyślnym.
* Warunki wyzwalania multi umożliwia włączanie konwerter `Text.Length` wartość na wartość logiczną.
* Jeśli wszystkie warunki są `true`, metoda ustawiająca sprawia, że przycisk `IsEnabled` właściwości `true`.

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

Te zrzuty ekranu pokazują różnicę między dwa multi wyzwalacza przykładach. W górnej części ekrany tekst wejściowy tylko w jednej `Entry` jest wystarczająca, aby umożliwić **zapisać** przycisku.
W dolnej części ekrany **logowania** przycisk pozostaje nieaktywne, dopóki oba pola zawierają dane.


![](triggers-images/multi-requireall.png "Przykłady multiTrigger")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>Akcji EnterActions i ExitActions

Innym sposobem implementacji zmian, gdy wystąpi wyzwalacza jest przez dodanie `EnterActions` i `ExitActions` kolekcje i określanie `TriggerAction<T>` implementacji.

Możesz podać *zarówno* `EnterActions` i `ExitActions` oraz `Setter`s w wyzwalaczu, ale należy pamiętać, że `Setter`s są nazywane natychmiast (nie należy czekać do `EnterAction` lub `ExitAction` do Wykonaj). Alternatywnie można wykonywać wszystkie elementy w kodzie i nie używać `Setter`s wcale.

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

Jak zawsze, gdy klasa jest przywoływany w XAML przestrzeni nazw powinny deklarować takich jak `xmlns:local` w sposób pokazany poniżej:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

`FadeTriggerAction` Kodu przedstawiono poniżej:

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

Uwaga: `EnterActions` i `ExitActions` są ignorowane w **wyzwalacze zdarzeń**.



## <a name="related-links"></a>Linki pokrewne

- [Przykładowe wyzwalacze](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/)
