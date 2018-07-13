---
title: EffectBehavior wielokrotnego użytku
description: Zachowania są przydatne do dodawania efektu do formantu, usuwanie efektu płytkę kocioł obsługi kodu z plików z kodem. W tym artykule przedstawiono, można dodać efekt do formantu za pomocą zachowania zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 1ce7eda6f556041cbffc3793b00e8e2cba44d3d0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995784"
---
# <a name="reusable-effectbehavior"></a>EffectBehavior wielokrotnego użytku

_Zachowania są przydatne do dodawania efektu do formantu, usuwanie efektu płytkę kocioł obsługi kodu z plików z kodem. W tym artykule przedstawiono, można dodać efekt do formantu za pomocą zachowania zestawu narzędzi Xamarin.Forms._

## <a name="overview"></a>Omówienie

`EffectBehavior` Klasa jest wielokrotnego użytku niestandardowe zachowanie zestawu narzędzi Xamarin.Forms, który dodaje [ `Effect` ](xref:Xamarin.Forms.Effect) wystąpienie kontrolki, gdy zachowanie jest dołączony do formantu i usuwa `Effect` wystąpienie, gdy zachowanie jest odłączenie od formantu.

Następujące właściwości zachowania musi być równa zachowanie:

- **Grupa** — wartość [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) atrybutu dla klasy efekt.
- **Nazwa** — wartość [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) atrybutu dla klasy efekt.

Aby uzyskać więcej informacji na temat skutków, zobacz [efekty](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="creating-the-behavior"></a>Tworzenie zachowanie

`EffectBehavior` Klasa pochodzi od [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) klasy, gdzie `T` jest [ `View` ](xref:Xamarin.Forms.View). Oznacza to, że `EffectBehavior` klasy mogą być dołączane do dowolnej kontrolki zestawu narzędzi Xamarin.Forms.

### <a name="implementing-bindable-properties"></a>Implementowanie właściwości możliwej do wiązania

`EffectBehavior` Klasy definiuje dwa [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) wystąpień, które są używane do dodawania [ `Effect` ](xref:Xamarin.Forms.Effect) do kontroli, gdy zachowanie jest dołączony do formantu. Te właściwości są wyświetlane w następującym przykładzie kodu:

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

Gdy `EffectBehavior` zużyciu `Group` właściwość powinna być ustawiona na wartość [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) atrybutu dla efektu. Ponadto `Name` właściwość powinna być ustawiona na wartość [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) atrybutu dla klasy efekt.

### <a name="implementing-the-overrides"></a>Implementowanie zastąpienia

`EffectBehavior` Klasy zastąpienia [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) i [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) metody [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) klasy, jak pokazano w poniższym kodzie przykład:

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

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Metoda wykonuje instalację, wywołując `AddEffect` jest metoda w kontrolce dołączonych jako parametr. [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Metoda wykonuje oczyszczania, wywołując `RemoveEffect` jest metoda w kontrolce dołączonych jako parametr.

### <a name="implementing-the-behavior-functionality"></a>Implementowanie funkcji zachowanie

Zachowanie ma na celu Dodaj [ `Effect` ](xref:Xamarin.Forms.Effect) zdefiniowane w `Group` i `Name` właściwości do kontrolki, gdy zachowanie jest dołączony do formantu, a następnie usuń `Effect` po zachowanie odłączenie od formantu. W poniższym przykładzie kodu pokazano podstawowe funkcje zachowanie:

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

`AddEffect` Metoda jest wykonywana w odpowiedzi na `EffectBehavior` dołączony do formantu, a odbiera kontrolki dołączonych jako parametr. Metoda następnie dodaje efekt pobrane z formantem [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekcji. `RemoveEffect` Metoda jest wykonywana w odpowiedzi na `EffectBehavior` odłączany od formantu który odbiera kontrolki dołączonych jako parametr. Metoda następnie usuwa efekt w formancie [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekcji.

`GetEffect` Metoda używa [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) metodę, która pobierze [ `Effect` ](xref:Xamarin.Forms.Effect). Efekt znajduje się za pośrednictwem składa się z `Group` i `Name` wartości właściwości. Jeśli platforma nie zapewnia efekt, `Effect.Resolve` metoda zwróci innej niż`null` wartości.

## <a name="consuming-the-behavior"></a>Korzystanie z zachowaniem

`EffectBehavior` Klasy mogą być dołączane do [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) kolekcji kontroli, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

Równoważny kod C# pokazano w poniższym przykładzie kodu:

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

`Group` i `Name` właściwości zachowania są ustawione na wartości [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) i [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) atrybutów dla klasy efekt, w każdym specyficzne dla platformy Projekt.

W czasie wykonywania, gdy zachowanie jest dołączony do [ `Label` ](xref:Xamarin.Forms.Label) kontroli `Xamarin.LabelShadowEffect` zostaną dodane do formantu [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekcji. Skutkuje to cienia do tekstu wyświetlanego przez dodawany `Label` kontrolować, jak pokazano na poniższych zrzutach ekranu:

![](effect-behavior-images/screenshots.png "Przykładową aplikację przy użyciu EffectsBehavior")

Zaletą tego zachowania do dodawania i usuwania skutków od formantów jest, że kod obsługi efekt kocioł tablicy mogą być usunięte z plików z kodem.

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, aby dodać efekt do formantu za pomocą zachowania. `EffectBehavior` Klasa jest wielokrotnego użytku niestandardowe zachowanie zestawu narzędzi Xamarin.Forms, który dodaje [ `Effect` ](xref:Xamarin.Forms.Effect) wystąpienie kontrolki, gdy zachowanie jest dołączony do formantu i usuwa `Effect` wystąpienie, gdy zachowanie jest odłączenie od formantu.


## <a name="related-links"></a>Linki pokrewne

- [Efekty](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Zachowanie efekt (przykład)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [Behavior](xref:Xamarin.Forms.Behavior)
- [Zachowanie<T>](xref:Xamarin.Forms.Behavior`1)
