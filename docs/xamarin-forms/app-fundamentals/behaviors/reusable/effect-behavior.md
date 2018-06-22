---
title: EffectBehavior wielokrotnego użytku
description: Zachowania są przydatne podejście do dodawania efekt do formantu, usunięcie efekt płytkę kocioł obsługi kodu z plików z kodem. W tym artykule przedstawiono Dodawanie efektu do formantu przy użyciu zachowanie platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: a1612d1e87f0e05c859babd93fd03ac9a5736b47
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30785058"
---
# <a name="reusable-effectbehavior"></a>EffectBehavior wielokrotnego użytku

_Zachowania są przydatne podejście do dodawania efekt do formantu, usunięcie efekt płytkę kocioł obsługi kodu z plików z kodem. W tym artykule przedstawiono Dodawanie efektu do formantu przy użyciu zachowanie platformy Xamarin.Forms._

## <a name="overview"></a>Omówienie

`EffectBehavior` Klasy jest wielokrotnego użytku platformy Xamarin.Forms zachowanie niestandardowych, które dodaje [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) wystąpienie do kontrolowania, czy zachowanie jest dołączona do formantu i usuwa `Effect` wystąpienia, gdy jest to zachowanie odłączyć od formantu.

Następujące właściwości zachowanie musi mieć ustawioną zachowanie:

- **Grupa** — wartość [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) atrybutu dla klasy efekt.
- **Nazwa** — wartość [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) atrybutu dla klasy efekt.

Aby uzyskać więcej informacji na temat efektów, zobacz [efekty](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="creating-the-behavior"></a>Tworzenie zachowania

`EffectBehavior` Pochodną klasy [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) klasy, których `T` jest [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/). Oznacza to, że `EffectBehavior` klasa może zostać dołączony do sterowania wszystkie platformy Xamarin.Forms.

### <a name="implementing-bindable-properties"></a>Implementowanie właściwości

`EffectBehavior` Klasa definiuje dwie [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wystąpień, które są używane do dodawania [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) do formantu, gdy działanie jest dołączony do formantu. Te właściwości są wyświetlane w poniższym przykładzie kodu:

```csharp
public class EffectBehavior : Behavior<View>
{
  public static readonly BindableProperty GroupProperty =
    BindableProperty.Create ("Group", typeof(string), typeof(EffectBehavior), null);
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(EffectBehavior), null);

  public string Group {
    get { return (string)GetValue (GroupProperty); }
    set { SetValue (GroupProperty, value); }
  }

  public string Name {
    get { return(string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }
  ...
}
```

Gdy `EffectBehavior` jest używany, `Group` właściwości powinien mieć ustawioną wartość [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) atrybutu dla efektu. Ponadto `Name` właściwości powinien mieć ustawioną wartość [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) atrybutu dla klasy efekt.

### <a name="implementing-the-overrides"></a>Implementowanie zastąpienia

`EffectBehavior` Klasy zastąpienia [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) i [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) metody [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) klasy, jak pokazano w poniższym kodzie przykład:

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  protected override void OnAttachedTo (BindableObject bindable)
  {
    base.OnAttachedTo (bindable);
    AddEffect (bindable as View);
  }

  protected override void OnDetachingFrom (BindableObject bindable)
  {
    RemoveEffect (bindable as View);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Metoda wykonuje instalację przez wywołanie metody `AddEffect` metoda w formancie dołączone jako parametr. [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Metoda przeprowadza oczyszczania wywołując `RemoveEffect` jest metoda w formancie dołączone jako parametr.

### <a name="implementing-the-behavior-functionality"></a>Implementowanie funkcji zachowanie

Zachowanie służy do dodawania [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) zdefiniowane w `Group` i `Name` właściwości do formantu, gdy działanie jest dołączony do formantu, a następnie usuń `Effect` przypadku zachowanie odłączyć od formantu. Podstawowe funkcje zachowanie pokazano w poniższym przykładzie kodu:

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  void AddEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Add (GetEffect ());
    }
  }

  void RemoveEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Remove (GetEffect ());
    }
  }

  Effect GetEffect ()
  {
    if (!string.IsNullOrWhiteSpace (Group) && !string.IsNullOrWhiteSpace (Name)) {
      return Effect.Resolve (string.Format ("{0}.{1}", Group, Name));
    }
    return null;
  }
}
```

`AddEffect` Metoda jest wykonywana w odpowiedzi na `EffectBehavior` dołączany do formantu który odbiera formantu dołączone jako parametr. Metoda Dodaje efekt pobrane do formantu [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekcji. `RemoveEffect` Metoda jest wykonywana w odpowiedzi na `EffectBehavior` odłączany od formantu który odbiera formantu dołączone jako parametr. Metoda następnie usuwa efekt formantu [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekcji.

`GetEffect` Używa metody [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) metoda pobierania [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/). Efekt znajduje się za pośrednictwem złączeniem `Group` i `Name` wartości właściwości. Jeśli platforma nie zapewnia efekt, `Effect.Resolve` metoda zwróci niż`null` wartości.

## <a name="consuming-the-behavior"></a>Korzystanie z zachowaniem

`EffectBehavior` Klasa może zostać dołączony do [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) kolekcji kontroli, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

W poniższym przykładzie kodu pokazano równoważne kodu C#:

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};
label.Behaviors.Add (new EffectBehavior {
  Group = "Xamarin",
  Name = "LabelShadowEffect"
});
```

`Group` i `Name` właściwości zachowania są ustawione na wartości [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) i [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) atrybuty dla klasy obowiązywać w każdej specyficzne dla platformy Projekt.

W czasie wykonywania, gdy działanie jest dołączony do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) kontroli, `Xamarin.LabelShadowEffect` zostanie dodany do formantu [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekcji. Powoduje to cienia dodawany do tekstu wyświetlanego przez `Label` kontroli, jak pokazano na poniższych zrzutach ekranu:

![](effect-behavior-images/screenshots.png "Przykładowa aplikacja z EffectsBehavior")

Zaletą używania tego zachowania do dodawania i usuwania skutków od formantów jest, czy kod obsługi efekt kocioł tablicy można usunąć z plików z kodem.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono Dodawanie efektu do formantu przy użyciu zachowanie. `EffectBehavior` Klasy jest wielokrotnego użytku platformy Xamarin.Forms zachowanie niestandardowych, które dodaje [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) wystąpienie do kontrolowania, czy zachowanie jest dołączona do formantu i usuwa `Effect` wystąpienia, gdy jest to zachowanie odłączyć od formantu.


## <a name="related-links"></a>Linki pokrewne

- [Efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Efekt zachowanie (przykład)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [Behavior](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Zachowanie<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
