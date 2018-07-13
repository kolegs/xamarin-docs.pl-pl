---
title: Właściwości możliwe do wiązania
description: Ten artykuł zawiera wprowadzenie do właściwości możliwej do wiązania i pokazuje, jak utworzyć i korzystać z nich.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 241579d51d1f0af84655f439bad3adb879404e91
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995391"
---
# <a name="bindable-properties"></a>Właściwości możliwe do wiązania

_W interfejsie Xamarin.Forms funkcje wspólne właściwości środowiska uruchomieniowego (języka wspólnego CLR) języka jest rozszerzany, które można powiązać właściwości. Właściwości możliwej do wiązania to specjalny rodzaj właściwości, których wartość właściwości jest śledzona przez system właściwości zestawu narzędzi Xamarin.Forms. Ten artykuł zawiera wprowadzenie do właściwości możliwej do wiązania i pokazuje, jak utworzyć i korzystać z nich._

## <a name="overview"></a>Omówienie

Właściwości możliwej do wiązania rozszerzenia CLR właściwości funkcji dzięki tworzeniu kopii właściwość o [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) typu, zamiast tworzenia kopii właściwość z polem. Właściwości możliwe do wiązania ma na celu stanowią system właściwości, który obsługuje powiązanie danych, style i szablony, a wartości ustawione przy użyciu relacji nadrzędny podrzędny. Ponadto właściwości możliwej do wiązania można podać wartości domyślne, sprawdzanie poprawności wartości właściwości i wywołania zwrotne, które monitorują zmiany właściwości.

Właściwości powinny być zrealizowane jako możliwej do wiązania właściwości do obsługi co najmniej jeden z następujących funkcji:

- Działający jako prawidłowy *docelowej* właściwość do wiązania danych.
- Ustawienie właściwości, za pośrednictwem [styl](~/xamarin-forms/user-interface/styles/index.md).
- Udostępnia wartość właściwości domyślnej, która jest inna niż domyślna dla typu właściwości.
- Sprawdzanie poprawności wartości właściwości.
- Monitorowanie zmian właściwości.

Przykłady zestawu narzędzi Xamarin.Forms właściwości możliwej do wiązania [ `Label.Text` ](xref:Xamarin.Forms.Label.Text), [ `Button.BorderRadius` ](xref:Xamarin.Forms.Button.BorderRadius), i [ `StackLayout.Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation). Każdej możliwej do wiązania właściwości ma odpowiadające mu `public static readonly` właściwości typu [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) które będzie widoczne na tej samej klasy, która jest identyfikator właściwości możliwej do wiązania. Na przykład, odpowiedni identyfikator właściwości możliwej do wiązania dla `Label.Text` właściwość [ `Label.TextProperty` ](xref:Xamarin.Forms.Label.TextProperty).

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>Tworzenie i korzystanie z właściwości możliwej do wiązania

Proces tworzenia właściwość może być powiązana jest następująca:

1. Tworzenie [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) wystąpienie z jednym z [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create*) przeciążenia metody.
1. Zdefiniuj metody dostępu właściwości, dla [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) wystąpienia.

Należy pamiętać, że wszystkie [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) wystąpienia muszą być utworzone w wątku interfejsu użytkownika. Oznacza to, że kod, który jest uruchamiany na wątku interfejsu użytkownika można uzyskać lub ustawić wartość właściwości możliwej do wiązania. Jednak `BindableProperty` wystąpień są dostępne z innych wątków przez kierowanie do wątku interfejsu użytkownika przy użyciu [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) metody.

### <a name="creating-a-property"></a>Tworzenie właściwości

Aby utworzyć `BindableProperty` wystąpienia, klasa zawierająca musi pochodzić od klasy [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) klasy. Jednak `BindableObject` klasa jest najwyższym poziomie w hierarchii klas, dzięki czemu większość klas używane dla właściwości możliwej do wiązania obsługę funkcji interfejsu użytkownika.

Właściwości możliwej do wiązania, mogą być tworzone przez zadeklarowanie `public static readonly` właściwości typu [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty). Właściwości możliwej do wiązania powinna być równa zwrócona wartość jednego z [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) przeciążenia metody. Deklaracja powinna mieścić się w treści [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) klasy, ale poza żadnych definicji elementu członkowskiego.

Jako minimum, należy określić identyfikator, podczas tworzenia [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty), wraz z następującymi parametrami:

- Nazwa [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty).
- Typ właściwości.
- Typ obiekt-właściciel.
- Wartość domyślna dla właściwości. Daje to gwarancję, że właściwość zawsze zwraca wartość określonego domyślnego, jest usunięta, gdy może on być inny niż wartość domyślna dla typu właściwości. Wartość domyślna będzie przywrócone po [ `ClearValue` ](xref:Xamarin.Forms.BindableObject.ClearValue(Xamarin.Forms.BindableProperty)) metoda jest wywoływana w właściwości możliwej do wiązania.

Poniższy kod przedstawia przykład właściwości możliwej do wiązania, przy użyciu identyfikatora i wartości dla czterech wymagane parametry:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

Spowoduje to utworzenie [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) wystąpienia o nazwie `EventName`, typu `string`. Właściwość jest własnością `EventToCommandBehavior` klasy, a wartość domyślna `null`. Konwencji nazewnictwa, które można powiązać właściwości jest, że identyfikator właściwości możliwej do wiązania musi odpowiadać nazwa właściwości określone w `Create` metody, z dołączoną "Property". W związku z tym, w powyższym przykładzie identyfikator właściwości możliwej do wiązania jest `EventNameProperty`.

Opcjonalnie podczas tworzenia [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) wystąpienia następujące parametry można określić:

- Tryb powiązania. Służy do określania kierunku, w którym rozpropaguje zmiany wartości właściwości. W domyślnym trybie powiązania, zmiany zostaną przeniesione z *źródła* do *docelowej*.
- Delegat weryfikacji, który zostanie wywołany, gdy ustawiono wartość właściwości. Aby uzyskać więcej informacji, zobacz [wywołania zwrotne weryfikacji](#validation).
- Delegat zmiany właściwości, który zostanie wywołany, gdy zmieniono wartość właściwości. Aby uzyskać więcej informacji, zobacz [wykrywanie zmian właściwości](#propertychanges).
- Zmiana właściwości delegata, który zostanie wywołany, gdy zmieni się wartość właściwości. Ten delegat ma taki sam podpis, jak obiekt delegowany zmiany właściwości.
- Delegat wartość coerce, który zostanie wywołany, gdy zmieniono wartość właściwości. Aby uzyskać więcej informacji, zobacz [wymuszone wywołania zwrotne wartości](#coerce).
- Element `Func` używany do inicjowania wartości domyślnej właściwości. Aby uzyskać więcej informacji, zobacz [tworzenia wartości domyślnej z Func](#defaultfunc).

### <a name="creating-accessors"></a>Tworzenie metod dostępu

Akcesory właściwości są wymagane na potrzeby dostępu do właściwości możliwej do wiązania składni właściwości. `Get` Akcesor powinna zwrócić wartość, która jest zawarta w odpowiedniej właściwości możliwej do wiązania. Można to osiągnąć przez wywołanie metody [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) metody, przekazując identyfikatora właściwości możliwej do wiązania, na którym ma zostać pobrana wartość, a następnie rzutowanie wynik na wymagany typ. `Set` Dostępu należy określić wartość odpowiedniej właściwości możliwej do wiązania. Można to osiągnąć przez wywołanie metody [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metody, przekazując identyfikatora właściwości możliwej do wiązania, dla której chcesz ustawić wartość i wartość do ustawienia.

Poniższy przykład kodu pokazuje metody dostępu dla `EventName` właściwości możliwej do wiązania:

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>Korzystanie z właściwości możliwej do wiązania

Po utworzeniu właściwości możliwej do wiązania, mogą być używane z XAML lub kodu. W XAML jest to osiągane przez zadeklarowanie przestrzeni nazw z prefiksem, za pomocą deklaracji przestrzeni nazw, wskazującą nazwę przestrzeni nazw CLR i, opcjonalnie, nazwę zestawu. Aby uzyskać więcej informacji, zobacz [przestrzeni nazw XAML](~/xamarin-forms/xaml/namespaces.md).

Poniższy przykład kodu demonstruje typ niestandardowy, który zawiera właściwości możliwej do wiązania, która jest zdefiniowana w ramach tego samego zestawu jako kod aplikacji, który odwołuje się do niestandardowego typu przestrzeni nazw XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Deklaracja przestrzeni nazw jest używana podczas ustawiania `EventName` właściwości możliwej do wiązania, jakie wykazano w poniższym przykładzie kodu XAML:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

Równoważny kod C# pokazano w poniższym przykładzie kodu:

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  ...
});
```

<a name="advanced" />

## <a name="advanced-scenarios"></a>Scenariusze zaawansowane

Podczas tworzenia [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) wystąpienia, istnieje wiele parametrów opcjonalnych, które można ustawić właściwości możliwej do wiązania zaawansowanych scenariuszy. W tej sekcji przedstawiono tych scenariuszy.

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>Wykrywanie zmian właściwości

A `static` metody wywołania zwrotnego z zmiany właściwości mogą być rejestrowane za pomocą właściwości możliwej do wiązania, określając `propertyChanged` parametr [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) metody. Po zmianie wartości właściwości możliwej do wiązania, zostanie wywołany metodą określonego wywołania zwrotnego.

Poniższy kod przedstawia przykładowy sposób, w jaki `EventName` rejestrów właściwości możliwej do wiązania `OnEventNameChanged` metodę jako metodę wywołania zwrotnego z zmiany właściwości:

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

W przypadku zmiany właściwości Metoda wywołania zwrotnego [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) parametr jest używany do określenia, które wystąpienie klasy będącej właścicielem zgłosił zmianę i wartości dwóch `object` stare i nowe wartości reprezentują parametry właściwości możliwej do wiązania.

<a name="validation" />

### <a name="validation-callbacks"></a>Wywołania zwrotne weryfikacji

A `static` metody wywołania zwrotnego weryfikacji można zarejestrować za pomocą właściwości możliwej do wiązania, określając `validateValue` parametr [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) metody. Metodą określonego wywołania zwrotnego zostanie wywołany, gdy ustawiono wartość właściwości możliwej do wiązania.

Poniższy kod przedstawia przykładowy sposób, w jaki `Angle` rejestrów właściwości możliwej do wiązania `IsValidValue` metodę jako metodę wywołania zwrotnego weryfikacji:

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

Wywołania zwrotne sprawdzania poprawności są dostarczane z wartością i powinna zwrócić `true` czy wartość jest nieprawidłowa dla właściwości, w przeciwnym razie `false`. Wyjątek zostanie wygenerowany, jeśli funkcja zwraca wywołanie zwrotne weryfikacji `false`, powinno zostać obsłużone przez dewelopera. Typowym zastosowaniem metody wywołania zwrotnego weryfikacji jest ograniczając wartości liczb całkowitych lub wartości podwójnej precyzji, jeśli ustawiono właściwość może być powiązana. Na przykład `IsValidValue` metoda sprawdza, czy wartość właściwości jest `double` w zakresie od 0 do 360.

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>Wywołania zwrotne wartości wymuszonych

A `static` coerce — wartość metody wywołania zwrotnego można zarejestrować za pomocą właściwości możliwej do wiązania, określając `coerceValue` parametr [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) metody. Po zmianie wartości właściwości możliwej do wiązania, zostanie wywołany metodą określonego wywołania zwrotnego.

Coerce — wartość wywołania zwrotne służą do wymuszenia ponownej oceny, które można powiązać właściwości po zmianie wartości właściwości. Na przykład wywołanie zwrotne wartość coerce można upewnij się, że wartość jednej właściwości możliwej do wiązania nie większa niż wartość innej właściwości możliwej do wiązania.

Poniższy kod przedstawia przykładowy sposób, w jaki `Angle` rejestrów właściwości możliwej do wiązania `CoerceAngle` metodę jako metodę wywołania zwrotnego wartość coerce:

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

`CoerceAngle` Metoda sprawdza wartość `MaximumAngle` właściwości i, jeśli `Angle` wartość właściwości jest większa niż, przekształca wynik dane wartość `MaximumAngle` wartości właściwości.

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>Tworzenie wartości domyślnej z Func

Element `Func` może służyć do inicjacji wartości domyślnej właściwości możliwej do wiązania, jak pokazano w poniższym przykładzie kodu:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator` Parametr ma wartość `Func` wywołującej [ `Device.GetNamedSize` ](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) metodę, aby zwrócić `double` reprezentujący nazwane rozmiar czcionki, która jest używana na [ `Label` ](xref:Xamarin.Forms.Label) na platformy natywnej.

## <a name="summary"></a>Podsumowanie

W tym artykule podano zapoznać się z wprowadzeniem do właściwości możliwej do wiązania i pokazano, jak utworzyć i korzystać z nich. Właściwości możliwej do wiązania to specjalny rodzaj właściwości, których wartość właściwości jest śledzona przez system właściwości zestawu narzędzi Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [Przestrzeń nazw XAML](~/xamarin-forms/xaml/namespaces.md)
- [Zdarzenia na zachowanie polecenia (przykład)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Wywołanie zwrotne weryfikacji (przykład)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [Coerce — wartość wywołania zwrotnego (przykład)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](xref:Xamarin.Forms.BindableProperty)
- [BindableObject](xref:Xamarin.Forms.BindableObject)
