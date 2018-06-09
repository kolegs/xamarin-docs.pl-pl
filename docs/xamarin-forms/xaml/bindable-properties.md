---
title: Właściwości
description: Ten artykuł zawiera wprowadzenie do właściwości i pokazuje, jak utworzyć i używać ich.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 5e39e8eb3d7ffb3ed33ea2a585d8d367302e9baa
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245979"
---
# <a name="bindable-properties"></a>Właściwości

_W platformy Xamarin.Forms funkcji wspólnego języka środowiska uruchomieniowego (języka wspólnego CLR) właściwości zostanie przedłużony właściwości. Właściwości możliwej do wiązania jest specjalnym rodzajem właściwości, gdy wartość właściwości jest śledzony przez system właściwości platformy Xamarin.Forms. Ten artykuł zawiera wprowadzenie do właściwości i pokazuje, jak utworzyć i używać ich._

## <a name="overview"></a>Omówienie

Właściwości rozszerzenia funkcji właściwość CLR przez tworzenie kopii właściwości o [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) typu zamiast kopii właściwość z polem. Właściwości ma na celu zapewnić systemu właściwości, który obsługuje powiązanie danych, style, szablony i wartościami ustawionymi przez relacji nadrzędny podrzędny. Ponadto można powiązać właściwości można podać wartości domyślne, sprawdzania poprawności wartości właściwości i wywołania zwrotne, które monitorują zmiany właściwości.

Właściwości powinny być implementowane jako właściwości do obsługi co najmniej jeden z następujących funkcji:

- Działając jako prawidłowy *docelowej* właściwości do wiązania danych.
- Ustawienie właściwości za pośrednictwem [styl](~/xamarin-forms/user-interface/styles/index.md).
- Udostępnia domyślną wartość właściwości, która jest inna niż domyślna dla typu właściwości.
- Sprawdzanie poprawności wartości właściwości.
- Monitorowanie zmian właściwości.

Przykłady właściwości platformy Xamarin.Forms [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/), [ `Button.BorderRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/), i [ `StackLayout.Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/). Każda właściwość powiązania ma odpowiadające mu `public static readonly` właściwości typu [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) które będzie widoczne na tej samej klasy, która jest identyfikator właściwości możliwej do wiązania. Na przykład odpowiednie właściwości możliwej do wiązania identyfikator `Label.Text` właściwość jest [ `Label.TextProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Label.TextProperty/).

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>Tworzenie i korzystanie z właściwości możliwej do wiązania

Proces tworzenia właściwości możliwej do wiązania jest następujący:

1. Utwórz [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wystąpienia jednego z [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) przeciążenia metody.
1. Zdefiniuj metody dostępu właściwości dla [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wystąpienia.

Należy pamiętać, że wszystkie [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wystąpienia należy utworzyć w wątku interfejsu użytkownika. Oznacza to, że tylko kod uruchamiany w wątku interfejsu użytkownika można pobrać lub ustawić wartość właściwości możliwej do wiązania. Jednak `BindableProperty` wystąpienia można uzyskać z innych wątków kierowanie w wątku interfejsu użytkownika z [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) metody.

### <a name="creating-a-property"></a>Tworzenie właściwości

Aby utworzyć `BindableProperty` wystąpienia, klasa zawierająca musi pochodzić od [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) klasy. Jednak `BindableObject` klasy jest wysoka w hierarchii klas, dlatego większość klas używane dla właściwości powiązania obsługi funkcji interfejsu użytkownika.

Można powiązać właściwości mogą być tworzone przez zadeklarowanie `public static readonly` właściwości typu [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/). Właściwości możliwej do wiązania powinien mieć ustawioną wartość jednego z [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) przeciążenia metody. Deklaracja powinna mieścić się w treści [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) klasy, ale poza żadnych definicji elementu członkowskiego.

Co najmniej identyfikator należy określić podczas tworzenia [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/), wraz z następującymi parametrami:

- Nazwa [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/).
- Typ właściwości.
- Typ będący właścicielem obiektu.
- Wartość domyślna właściwości. Dzięki temu, że właściwość zawsze zwraca wartość określonego domyślnego, gdy jest unset i może być inna niż wartość domyślna dla typu właściwości. Wartość domyślna będzie przywrócone po [ `ClearValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.ClearValue/p/Xamarin.Forms.BindableProperty/) metoda jest wywoływana we właściwościach powiązania.

Poniższy kod przedstawia przykład właściwości możliwej do wiązania, identyfikatora i wartości czterech wymaganych parametrów:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

Spowoduje to utworzenie [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wystąpienia o nazwie `EventName`, typu `string`. Właściwość jest własnością `EventToCommandBehavior` klasy, a ma wartość domyślną `null`. Konwencję nazewnictwa dla właściwości jest, że identyfikator właściwości powiązania musi odpowiadać nazwie właściwości określony w `Create` metody z dołączonym "Property". W związku z tym w powyższym przykładzie identyfikator właściwości możliwej do wiązania jest `EventNameProperty`.

Opcjonalnie tworząc [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wystąpienie następujące parametry można określić:

- Tryb powiązania. Służy do określania kierunku, w którym rozpropaguje zmiany wartości właściwości. W domyślnym trybie powiązanie, zmiany zostaną przeniesione z *źródła* do *docelowej*.
- Delegat weryfikacji, który zostanie wywołany, gdy wartość właściwości jest ustawiona. Aby uzyskać więcej informacji, zobacz [wywołania zwrotne walidacji](#validation).
- Delegat zmiany właściwości, które będą wywoływane, gdy zmieniono wartość właściwości. Aby uzyskać więcej informacji, zobacz [wykrywanie zmian właściwości](#propertychanges).
- Właściwość zmiana delegata, który zostanie wywołany, gdy zmieni się wartość właściwości. Ten delegat ma takiego samego podpisu jak obiekt delegowany zmiany właściwości.
- Delegat wartość coerce, które będą wywoływane, gdy zmieniono wartość właściwości. Aby uzyskać więcej informacji, zobacz [wymuszone wywołania zwrotne wartość](#coerce).
- A `Func` używany do inicjowania wartości domyślnej właściwości. Aby uzyskać więcej informacji, zobacz [tworzenie wartości domyślnej z Func](#defaultfunc).

### <a name="creating-accessors"></a>Tworzenie metody dostępu

Metod dostępu do właściwości są wymagane na potrzeby dostępu do właściwości powiązania składni właściwości. `Get` Akcesor powinien zwrócić wartość, która jest zawarta w odpowiedniej właściwości możliwej do wiązania. Można to osiągnąć poprzez wywołanie [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) metody, przekazując identyfikator właściwości możliwej do wiązania, na którym ma być pobrana wartość, a następnie Rzutowanie na wymagany typ wyniku. `Set` Dostępu należy ustawić wartość może być powiązana odpowiednia właściwość. Można to osiągnąć poprzez wywołanie [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody, przekazując identyfikator właściwości możliwej do wiązania, dla których należy ustawić wartość i wartość do ustawienia.

Poniższy przykład kodu pokazuje metody dostępu dla `EventName` właściwości możliwej do wiązania:

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>Korzystanie z właściwości możliwej do wiązania

Po utworzeniu właściwości możliwej do wiązania, mogą być używane z XAML lub kodu. W języku XAML jest to osiągane przez deklarowanie przestrzeni nazw z prefiksem z deklaracji przestrzeni nazw wskazujący nazwę przestrzeni nazw CLR i opcjonalnie nazwy zestawu. Aby uzyskać więcej informacji, zobacz [przestrzeń nazw XAML](~/xamarin-forms/xaml/namespaces.md).

Poniższy przykład kodu pokazuje przestrzeni nazw XAML dla niestandardowego typu, który zawiera właściwości możliwej do wiązania, który jest zdefiniowany w tym samym zestawie co kod aplikacji, która odwołuje się do niestandardowego typu:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Deklaracja przestrzeni nazw jest używana podczas ustawiania `EventName` właściwości możliwej do wiązania, jako wykazały w poniższym przykładzie kodu XAML:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

W poniższym przykładzie kodu pokazano równoważne kodu C#:

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  ...
});
```

<a name="advanced" />

## <a name="advanced-scenarios"></a>Scenariusze zaawansowane

Podczas tworzenia [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wystąpienia, istnieje wiele parametrów opcjonalnych, które można ustawić na potrzeby scenariuszy z zaawansowanych właściwości możliwej do wiązania. Ta sekcja opisuje tych scenariuszy.

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>Wykrywanie zmian właściwości

A `static` zmienić właściwości wywołania zwrotnego metody mogą być rejestrowane w właściwości możliwej do wiązania, określając `propertyChanged` parametr [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) metody. Metody wywołania zwrotnego określony zostanie wywołany, gdy wartość właściwości możliwej do wiązania.

Poniższy kod przedstawia przykład sposobu `EventName` rejestrów właściwości możliwej do wiązania `OnEventNameChanged` metodę jako metoda wywołania zwrotnego z zmienić właściwości:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create (
    "EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
...

static void OnEventNameChanged (BindableObject bindable, object oldValue, object newValue)
{
  // Property changed implementation goes here
}
```

W metodzie zmienić właściwości wywołania zwrotnego [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) parametr jest używany do określenia, które wystąpienie klasy będący właścicielem zgłosił zmianę i wartości dwa `object` reprezentują parametry starej i nowej wartości właściwości możliwej do wiązania.

<a name="validation" />

### <a name="validation-callbacks"></a>Wywołania zwrotne walidacji

A `static` weryfikacji metody wywołania zwrotnego może być zarejestrowany z właściwości możliwej do wiązania, określając `validateValue` parametr [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) metody. Metody wywołania zwrotnego określony zostanie wywołany, gdy wartość właściwości możliwej do wiązania.

Poniższy kod przedstawia przykład sposobu `Angle` rejestrów właściwości możliwej do wiązania `IsValidValue` metodę jako metoda wywołania zwrotnego sprawdzania poprawności:

```csharp
public static readonly BindableProperty AngleProperty =
  BindableProperty.Create ("Angle", typeof(double), typeof(HomePage), 0.0, validateValue: IsValidValue);
...

static bool IsValidValue (BindableObject view, object value)
{
  double result;
  bool isDouble = double.TryParse (value.ToString (), out result);
  return (result >= 0 && result <= 360);
}
```

Wywołania zwrotne walidacji są dostarczane z wartością i powinien zwrócić `true` czy wartość jest nieprawidłowa dla właściwości, w przeciwnym razie `false`. Wystąpił wyjątek zostanie wygenerowany, jeśli wywołanie zwrotne weryfikacji zwraca `false`, który powinien zostać obsłużony przez dewelopera. Typowy sposób użycia metody wywołania zwrotnego sprawdzania poprawności jest ograniczający wartości liczb całkowitych lub na symulacyjnych, gdy właściwość powiązania jest ustawiona. Na przykład `IsValidValue` metoda sprawdza, czy wartość właściwości jest `double` z zakresu od 0 do 360.

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>Coerce — wartość wywołań zwrotnych

A `static` wymuszone wartość metody wywołania zwrotnego może być zarejestrowany w usłudze właściwości możliwej do wiązania, określając `coerceValue` parametr [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) metody. Metody wywołania zwrotnego określony zostanie wywołany, gdy wartość właściwości możliwej do wiązania.

Przekształcić wartość wywołań zwrotnych są używane do wymuszenia ponownego oszacowania można powiązać właściwości po zmianie wartości właściwości. Na przykład wywołanie zwrotne wartość coerce można zapewnić wartość jedną właściwość powiązania nie jest większa niż wartość właściwości możliwej do wiązania innego.

Poniższy kod przedstawia przykład sposobu `Angle` rejestrów właściwości możliwej do wiązania `CoerceAngle` metodę jako metoda wywołania zwrotnego wartość coerce:

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle) {
    input = homePage.MaximumAngle;
  }

  return input;
}
```

`CoerceAngle` Metoda sprawdza wartość `MaximumAngle` właściwości oraz jeśli `Angle` jest większa niż wartość właściwości, przekształca wynik dane wartości `MaximumAngle` wartości właściwości.

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>Tworzenie wartości domyślnej z Func

A `Func` można zainicjować wartość domyślnej właściwości możliwej do wiązania, jak pokazano w poniższym przykładzie:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator` Ustawiono parametr `Func` który wywołuje [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) metodę, aby zwrócić `double` reprezentujący nazwanego rozmiar czcionki, który jest używany w [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) na platformie macierzystego.

## <a name="summary"></a>Podsumowanie

W tym artykule podane wprowadzenie do właściwości, a także przedstawiono sposób tworzenia i korzystać z nich. Właściwości możliwej do wiązania jest specjalnym rodzajem właściwości, gdy wartość właściwości jest śledzony przez system właściwości platformy Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [Przestrzeń nazw XAML](~/xamarin-forms/xaml/namespaces.md)
- [Zdarzenia do zachowania polecenia (na przykład)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Wywołanie zwrotne weryfikacji (przykład)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [Coerce — wartość wywołania zwrotnego (przykład)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
