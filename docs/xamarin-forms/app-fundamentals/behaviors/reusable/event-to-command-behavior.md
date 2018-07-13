---
title: EventToCommandBehavior wielokrotnego użytku
description: Zachowania może służyć do skojarzenia poleceń z kontrolkami, które nie zostały zaprojektowane do interakcji z poleceniami. W tym artykule przedstawiono, za pomocą zachowania zestawu narzędzi Xamarin.Forms do wywołania polecenia, gdy zdarzenie zostanie wyzwolony.
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 3151179b6ff6d26b74a87ded747310646b304603
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996324"
---
# <a name="reusable-eventtocommandbehavior"></a>EventToCommandBehavior wielokrotnego użytku

_Zachowania może służyć do skojarzenia poleceń z kontrolkami, które nie zostały zaprojektowane do interakcji z poleceniami. W tym artykule przedstawiono, za pomocą zachowania zestawu narzędzi Xamarin.Forms do wywołania polecenia, gdy zdarzenie zostanie wyzwolony._

## <a name="overview"></a>Omówienie

`EventToCommandBehavior` Klasa jest wielokrotnego użytku niestandardowe zachowanie zestawu narzędzi Xamarin.Forms, który wykonuje polecenia w odpowiedzi na *wszelkie* inicjowanie zdarzeń. Domyślnie argumenty zdarzeń dla zdarzenia zostaną przekazane do polecenia i może być opcjonalnie przekonwertowane przez [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) implementacji.

Następujące właściwości zachowania musi być równa zachowanie:

- **EventName** — Nazwa zdarzenia nasłuchuje zachowanie.
- **Polecenie** — **ICommand** do wykonania. Zachowanie spodziewa się znaleźć `ICommand` wystąpienia na [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) dołączonych kontrolki, które mogą być dziedziczone z elementu nadrzędnego.

Można również ustawić następujące właściwości zachowanie opcjonalne:

- **CommandParameter** — `object` które zostaną przekazane do polecenia.
- **Konwerter** — [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) implementację, która zmieni format danych argumentu zdarzenia są przekazywane między *źródła* i *docelowej*przez mechanizm wiązania.

## <a name="creating-the-behavior"></a>Tworzenie zachowanie

`EventToCommandBehavior` Klasa pochodzi od `BehaviorBase<T>` klasy, która z kolei pochodzi od klasy [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) klasy. Celem `BehaviorBase<T>` klasa ma na celu dostarczenie klasę bazową dla dowolnego zachowania zestawu narzędzi Xamarin.Forms, które wymagają [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) zachowania, należy ustawić dołączonych kontroli. Dzięki temu działanie można powiązać, a wykonywanie `ICommand` określony przez `Command` właściwości, gdy jest używane działanie.

`BehaviorBase<T>` Klasa udostępnia możliwym do zastąpienia [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) metodę, która ustawia [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) zachowanie i którą można przesłonić [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))metodę, która czyści `BindingContext`. Ponadto klasa przechowuje odwołanie do dołączonych formantu w `AssociatedObject` właściwości.

### <a name="implementing-bindable-properties"></a>Implementowanie właściwości możliwej do wiązania

`EventToCommandBehavior` Klasa definiuje cztery [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) polecenie jest zdefiniowane w jedno, które są wykonywane przez użytkownika, gdy zdarzenie zostanie wyzwolony. Te właściwości są wyświetlane w następującym przykładzie kodu:

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

Gdy `EventToCommandBehavior` zużyciu klasy `Command` właściwość powinna być danymi powiązanymi `ICommand` do wykonania w odpowiedzi na inicjowanie zdarzeń, która jest zdefiniowana w `EventName` właściwości. Zachowanie będzie poszukiwać `ICommand` na [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) dołączonych formantu.

Domyślnie argumenty zdarzeń dla zdarzenia zostaną przekazane do polecenia. Te dane mogą być opcjonalnie konwertowane, ponieważ jest przekazywana między *źródła* i *docelowej* przez aparat powiązania, określając [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) implementacji jako `Converter` wartości właściwości. Alternatywnie parametru może być przekazywany do polecenia, określając `CommandParameter` wartości właściwości.

### <a name="implementing-the-overrides"></a>Implementowanie zastąpienia

`EventToCommandBehavior` Klasy zastąpienia [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) i [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) metody `BehaviorBase<T>` klasy, jak pokazano w poniższym przykładzie kodu:

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

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Metoda wykonuje instalację, wywołując `RegisterEvent` metody, przekazując wartość `EventName` właściwości jako parametr. [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Metoda wykonuje oczyszczania, wywołując `DeregisterEvent` metody, przekazując wartość `EventName` właściwości jako parametr.

### <a name="implementing-the-behavior-functionality"></a>Implementowanie funkcji zachowanie

Zachowanie ma na celu wykonania polecenia zdefiniowanego przez `Command` właściwości w odpowiedzi na inicjowanie zdarzeń, który jest definiowany przez `EventName` właściwości. W poniższym przykładzie kodu pokazano podstawowe funkcje zachowanie:

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

`RegisterEvent` Metoda jest wykonywana w odpowiedzi na `EventToCommandBehavior` dołączony do formantu który otrzymuje wartość `EventName` właściwości jako parametr. Metoda następnie próbuje zlokalizować zdarzenie zdefiniowane w `EventName` właściwość sterowanie dołączone. Pod warunkiem, że zdarzenia można znaleźć, `OnEvent` metody jest zarejestrowany jako metody obsługi zdarzenia.

`OnEvent` Metoda jest wykonywana w odpowiedzi na inicjowanie zdarzeń, która jest zdefiniowana w `EventName` właściwości. Pod warunkiem, że `Command` właściwości odwołuje się do prawidłowego `ICommand`, metoda próbuje pobrać parametr do przekazania do `ICommand` w następujący sposób:

- Jeśli `CommandParameter` właściwość definiuje parametru, zostanie pobrana.
- W przeciwnym razie, jeśli `Converter` właściwość definiuje [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) implementacji konwertera jest wykonywany i konwertuje dane argumentu zdarzenia, ponieważ jest przekazywana między *źródła* i *docelowej* przez mechanizm wiązania.
- W przeciwnym razie argumenty zdarzeń są zakłada się, że parametr.

Dane powiązane `ICommand` następnie jest wykonywany, przekazując parametru do polecenia, pod warunkiem, że [ `CanExecute` ](xref:Xamarin.Forms.Command.CanExecute(System.Object)) metoda zwraca `true`.

Chociaż nie jest wyświetlany w tym miejscu `EventToCommandBehavior` obejmuje również `DeregisterEvent` metodę, która jest wykonywana przez [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) metody. `DeregisterEvent` Metoda jest używana do lokalizowania i wyrejestrować zdarzenia, zdefiniowany w `EventName` właściwości, aby oczyścić wszelkie potencjalne pamięci, przecieków.

## <a name="consuming-the-behavior"></a>Korzystanie z zachowaniem

`EventToCommandBehavior` Klasy mogą być dołączane do [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) kolekcji kontroli, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ListView ItemsSource="{Binding People}">
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}"
                                  Converter="{StaticResource SelectedItemConverter}" />
  </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

Równoważny kod C# pokazano w poniższym przykładzie kodu:

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

`Command` Właściwość zachowanie jest dane powiązane z `OutputAgeCommand` właściwość ViewModel skojarzonego, podczas gdy `Converter` właściwość jest ustawiona na `SelectedItemConverter` wystąpienia, która zwraca wartość [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem)z [ `ListView` ](xref:Xamarin.Forms.ListView) z [ `SelectedItemChangedEventArgs` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs).

W czasie wykonywania zachowanie będzie odpowiadać na interakcję z kontrolką. Po wybraniu elementu w [ `ListView` ](xref:Xamarin.Forms.ListView), [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) nastąpi zdarzenie, które będą wykonywane `OutputAgeCommand` w ViewModel. Z kolei spowoduje to zaktualizowanie ViewModel `SelectedItemText` właściwości, [ `Label` ](xref:Xamarin.Forms.Label) wiąże pozycji, jak pokazano na poniższych zrzutach ekranu:

[![](event-to-command-behavior-images/screenshots-sml.png "Przykładowa aplikacja z EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "przykładowej aplikacji przy użyciu EventToCommandBehavior")

Zaletą tego zachowania do wykonania polecenia, gdy zdarzenie zostanie wyzwolony, to, że polecenia mogą być skojarzone z kontrolkami, które nie zostały przeznaczone do interakcji z poleceniami. Ponadto spowoduje to usunięcie kod obsługi zdarzeń płytkę kocioł z plików z kodem.

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, za pomocą zachowania zestawu narzędzi Xamarin.Forms do wywołania polecenia, gdy zdarzenie zostanie wyzwolony. Zachowania może służyć do skojarzenia poleceń z kontrolkami, które nie zostały zaprojektowane do interakcji z poleceniami.


## <a name="related-links"></a>Linki pokrewne

- [Zachowanie EventToCommand (przykład)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Behavior](xref:Xamarin.Forms.Behavior)
- [Zachowanie<T>](xref:Xamarin.Forms.Behavior`1)
