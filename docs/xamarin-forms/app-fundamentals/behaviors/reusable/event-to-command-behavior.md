---
title: "EventToCommandBehavior wielokrotnego użytku"
description: "Zachowania może służyć do skojarzenia z formantami, które nie zostały zaprojektowane do interakcji z poleceniami poleceń. W tym artykule przedstawiono wywołania polecenia, gdy zdarzenie jest generowane przy użyciu zachowanie platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: d82a1391feca9187cf2aca4394509447aeac6a18
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="reusable-eventtocommandbehavior"></a>EventToCommandBehavior wielokrotnego użytku

_Zachowania może służyć do skojarzenia z formantami, które nie zostały zaprojektowane do interakcji z poleceniami poleceń. W tym artykule przedstawiono wywołania polecenia, gdy zdarzenie jest generowane przy użyciu zachowanie platformy Xamarin.Forms._

## <a name="overview"></a>Omówienie

`EventToCommandBehavior` Klasy jest wielokrotnego użytku platformy Xamarin.Forms zachowanie niestandardowych, które wykonuje polecenie w odpowiedzi na *żadnych* inicjowanie zdarzeń. Domyślnie argumenty zdarzeń dla zdarzenia będą przekazywane do polecenia, a opcjonalnie można przekonwertować przez [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) implementacji.

Następujące właściwości zachowanie musi mieć ustawioną zachowanie:

- **EventName** — Nazwa zdarzenia nasłuchuje zachowanie.
- **Polecenie** — **ICommand** do wykonania. Zachowanie oczekuje można znaleźć `ICommand` wystąpienie na [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) dołączone kontroli, które mogą być dziedziczone z elementu nadrzędnego.

Można również ustawić następujące właściwości zachowanie opcjonalne:

- **CommandParameter** — `object` który będą przekazywane do polecenia.
- **Konwerter** — [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) implementację, która zmieni format danych argument zdarzenia jest przekazywany między *źródła* i *docelowej*przez aparat wiązania.

## <a name="creating-the-behavior"></a>Tworzenie zachowania

`EventToCommandBehavior` Pochodną klasy `BehaviorBase<T>` klasy, która z kolei jest pochodną [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) klasy. Celem `BehaviorBase<T>` klasy jest zapewnienie klasę podstawową dla dowolnej platformy Xamarin.Forms zachowania, które wymagają [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) zachowania, należy ustawić dołączonych kontroli. Zapewnia to zachowanie może powiązać i wykonać `ICommand` określonego przez `Command` właściwości, gdy jest używane przez zachowanie.

`BehaviorBase<T>` Klasa udostępnia możliwym do zastąpienia [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) metodę, która ustawia [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) zachowania i którą można przesłonić [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)metodę, która czyści `BindingContext`. Ponadto klasa zawiera odwołanie do formantu dołączonych w `AssociatedObject` właściwości.

### <a name="implementing-bindable-properties"></a>Implementowanie właściwości

`EventToCommandBehavior` Klasa definiuje cztery [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wystąpień, które są wykonywane przez użytkownika zdefiniowane polecenia, gdy generowane zdarzenie. Te właściwości są wyświetlane w poniższym przykładzie kodu:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  public static readonly BindableProperty EventNameProperty =
    BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
  public static readonly BindableProperty CommandProperty =
    BindableProperty.Create ("Command", typeof(ICommand), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty CommandParameterProperty =
    BindableProperty.Create ("CommandParameter", typeof(object), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty InputConverterProperty =
    BindableProperty.Create ("Converter", typeof(IValueConverter), typeof(EventToCommandBehavior), null);

  public string EventName { ... }
  public ICommand Command { ... }
  public object CommandParameter { ... }
  public IValueConverter Converter { ...  }
  ...
}
```

Gdy `EventToCommandBehavior` klasy jest używany, `Command` właściwości powinny być danymi powiązanymi `ICommand` ma być wykonywana w odpowiedzi na inicjowanie zdarzeń, który jest zdefiniowany w `EventName` właściwości. Zachowanie będzie poszukiwać `ICommand` na [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) dołączone formantu.

Domyślnie argumenty dla zdarzenia będą przekazywane do polecenia. Te dane mogą być opcjonalnie konwertowane, jak jest przekazywany między *źródła* i *docelowej* przez aparat wiązania, określając [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) implementacji jako `Converter` wartości właściwości. Można również parametr mogą być przekazywane do polecenia, określając `CommandParameter` wartości właściwości.

### <a name="implementing-the-overrides"></a>Implementowanie zastąpienia

`EventToCommandBehavior` Klasy zastąpienia [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) i [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) metody `BehaviorBase<T>` klasy, jak pokazano w poniższym przykładzie:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  protected override void OnAttachedTo (View bindable)
  {
    base.OnAttachedTo (bindable);
    RegisterEvent (EventName);
  }

  protected override void OnDetachingFrom (View bindable)
  {
    DeregisterEvent (EventName);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Metoda wykonuje instalację przez wywołanie metody `RegisterEvent` metody, przekazując wartość `EventName` właściwości jako parametr. [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Metoda przeprowadza oczyszczania przez wywołanie metody `DeregisterEvent` metody, przekazując wartość `EventName` właściwości jako parametr.

### <a name="implementing-the-behavior-functionality"></a>Implementowanie funkcji zachowanie

Zachowanie służy do wykonywania poleceń zdefiniowanych przez `Command` właściwości w odpowiedzi na inicjowanie zdarzeń, który jest zdefiniowany przez `EventName` właściwości. Podstawowe funkcje zachowanie pokazano w poniższym przykładzie kodu:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  void RegisterEvent (string name)
  {
    if (string.IsNullOrWhiteSpace (name)) {
      return;
    }

    EventInfo eventInfo = AssociatedObject.GetType ().GetRuntimeEvent (name);
    if (eventInfo == null) {
      throw new ArgumentException (string.Format ("EventToCommandBehavior: Can't register the '{0}' event.", EventName));
    }
    MethodInfo methodInfo = typeof(EventToCommandBehavior).GetTypeInfo ().GetDeclaredMethod ("OnEvent");
    eventHandler = methodInfo.CreateDelegate (eventInfo.EventHandlerType, this);
    eventInfo.AddEventHandler (AssociatedObject, eventHandler);
  }

  void OnEvent (object sender, object eventArgs)
  {
    if (Command == null) {
      return;
    }

    object resolvedParameter;
    if (CommandParameter != null) {
      resolvedParameter = CommandParameter;
    } else if (Converter != null) {
      resolvedParameter = Converter.Convert (eventArgs, typeof(object), null, null);
    } else {
      resolvedParameter = eventArgs;
    }       

    if (Command.CanExecute (resolvedParameter)) {
      Command.Execute (resolvedParameter);
    }
  }
  ...
}
```

`RegisterEvent` Metoda jest wykonywana w odpowiedzi na `EventToCommandBehavior` dołączany do formantu który odbiera wartość `EventName` właściwości jako parametr. Metoda następnie próbuje zlokalizować zdarzeń zdefiniowanych w `EventName` właściwości dołączonych formantu. Pod warunkiem, że zdarzenie może być umieszczony `OnEvent` metody jest zarejestrowany jako metoda obsługi dla zdarzenia.

`OnEvent` Metoda jest wykonywana w odpowiedzi na inicjowanie zdarzeń, który jest zdefiniowany w `EventName` właściwości. Pod warunkiem że `Command` właściwość odwołuje się do prawidłowej `ICommand`, metoda próbuje pobrać parametr do przekazania do `ICommand` w następujący sposób:

- Jeśli `CommandParameter` właściwość definiuje parametru, zostanie pobrana.
- W przeciwnym razie, jeśli `Converter` definiuje właściwość [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) implementacji konwertera jest wykonywany i konwertuje dane argument zdarzenia, ponieważ jest przekazywany między *źródła* i *docelowej* przez aparat wiązania.
- W przeciwnym razie argumenty zdarzeń są przyjmowane jako parametr.

Danymi powiązanymi `ICommand` następnie jest wykonywane, w parametr przekazywany do polecenia, pod warunkiem, że [ `CanExecute` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Command.CanExecute/p/System.Object/) metoda zwraca `true`.

Wprawdzie nie pokazano w tym miejscu `EventToCommandBehavior` obejmuje również `DeregisterEvent` metodę, która jest wykonywana przez [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) metody. `DeregisterEvent` Metoda jest używana do lokalizowania i wyrejestrowania zdarzeń zdefiniowanych w `EventName` właściwości do oczyszczania przecieków wszystkie potencjalne pamięci.

## <a name="consuming-the-behavior"></a>Korzystanie z zachowaniem

`EventToCommandBehavior` Klasa może zostać dołączony do [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) kolekcji kontroli, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ListView ItemsSource="{Binding People}">
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}"
                                  Converter="{StaticResource SelectedItemConverter}" />
  </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

W poniższym przykładzie kodu pokazano równoważne kodu C#:

```csharp
var listView = new ListView ();
listView.SetBinding (ItemsView<Cell>.ItemsSourceProperty, "People");
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
  Converter = new SelectedItemEventArgsToSelectedItemConverter ()
});

var selectedItemLabel = new Label ();
selectedItemLabel.SetBinding (Label.TextProperty, "SelectedItemText");
```

`Command` Właściwość zachowanie jest powiązane z danych `OutputAgeCommand` obiektu ViewModel skojarzonego, podczas `Converter` właściwość jest ustawiona na `SelectedItemConverter` wystąpienia, która zwraca [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/)z [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) z [ `SelectedItemChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SelectedItemChangedEventArgs/).

W czasie wykonywania zachowanie będzie odpowiadać interakcji z formantem. Po wybraniu elementu w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) uruchomią zdarzeń, które będą wykonywane `OutputAgeCommand` w ViewModel. Z kolei spowoduje to zaktualizowanie ViewModel `SelectedItemText` właściwości który [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wiąże do, jak pokazano na poniższych zrzutach ekranu:

[![](event-to-command-behavior-images/screenshots-sml.png "Przykładowa aplikacja z EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "Przykładowa aplikacja z EventToCommandBehavior")

Zaletą korzystania z tego zachowania do wykonania polecenia, gdy zdarzenie jest generowane, to czy polecenia może być skojarzony z formantami, które nie zostały zaprojektowane do interakcji z poleceń. Ponadto spowoduje to usunięcie tablicy kocioł kod obsługi zdarzeń z plików z kodem.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono wywołania polecenia, gdy zdarzenie jest generowane przy użyciu zachowanie platformy Xamarin.Forms. Zachowania może służyć do skojarzenia z formantami, które nie zostały zaprojektowane do interakcji z poleceniami poleceń.


## <a name="related-links"></a>Linki pokrewne

- [Zachowanie EventToCommand (przykład)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Behavior](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Zachowanie<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
