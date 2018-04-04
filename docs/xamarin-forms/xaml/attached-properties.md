---
title: Dołączone właściwości
description: Dołączona właściwość jest specjalnym rodzajem właściwości możliwej do wiązania, zdefiniowany w jedną klasę, ale dołączone do innych obiektów i rozpoznawalną w języku XAML jako atrybutu, który zawiera klasę i nazwę właściwości oddzielone kropką. Ten artykuł zawiera wprowadzenie do dołączonych właściwości i pokazuje, jak utworzyć i używać ich.
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 5c903a39e5569c7ffedfff8eb8e6b0bd4071be9d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="attached-properties"></a>Dołączone właściwości

_Dołączona właściwość jest specjalnym rodzajem właściwości możliwej do wiązania, zdefiniowany w jedną klasę, ale dołączone do innych obiektów i rozpoznawalną w języku XAML jako atrybutu, który zawiera klasę i nazwę właściwości oddzielone kropką. Ten artykuł zawiera wprowadzenie do dołączonych właściwości i pokazuje, jak utworzyć i używać ich._

## <a name="overview"></a>Omówienie

Dołączony Włącz właściwości obiektu do przypisania wartości dla właściwości, które nie definiuje własnej klasy. Na przykład podrzędnych, których można używać elementów dołączonych właściwości, aby poinformować ich elementu nadrzędnego, jak są one przedstawiane w interfejsie użytkownika. [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) Kontroli umożliwia wierszy i kolumn podrzędnych należy określić przez ustawienie `Grid.Row` i `Grid.Column` dołączone właściwości. `Grid.Row` i `Grid.Column` są dołączone właściwości, ponieważ są one ustawiane dla elementów, które są elementami podrzędnymi `Grid`, a nie na `Grid` samej siebie.

Właściwości powinny być implementowane jako dołączone właściwości w następujących scenariuszach:

- Gdy istnieje potrzeba aby mechanizm ustawienie właściwości dostępne dla klasy innej niż Definiowanie klasy.
- Gdy klasa reprezentuje usługi, który ma być łatwo integrować z innych klas.

Aby uzyskać więcej informacji na temat właściwości, zobacz [właściwości](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="creating-and-consuming-an-attached-property"></a>Tworzenie i korzystanie z dołączona właściwość

Proces tworzenia dołączona właściwość wygląda następująco:

1. Utwórz [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) wystąpienia jednego z [ `CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) przeciążenia metody.
1. Podaj `static` `Get` *PropertyName* i `Set` *PropertyName* metody jako metody dostępu dla dołączona właściwość.

### <a name="creating-a-property"></a>Tworzenie właściwości

Podczas tworzenia właściwości dołączonej do użycia na inne typy, klasy, których tworzone jest właściwość nie ma pochodzić od [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/). Jednak *docelowej* właściwości dla metod dostępu powinien być lub pochodzić od, [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/).

Dołączona właściwość mogą być tworzone przez zadeklarowanie `public static readonly` właściwości typu [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/). Właściwości możliwej do wiązania powinien mieć ustawioną wartość jednego z [ `BindableProperty.CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) przeciążenia metody. Deklaracja powinna być w treści klasa będąca właścicielem, ale poza żadnych definicji elementu członkowskiego.

Poniższy kod przedstawia przykład dołączona właściwość:

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

Spowoduje to utworzenie dołączona właściwość o nazwie `HasShadow`, typu `bool`. Właściwość jest własnością `ShadowEffect` klasy, a ma wartość domyślną `false`. Konwencję nazewnictwa dołączone właściwości jest, czy identyfikator dołączona właściwość musi pasować do nazwy właściwości, określona w `CreateAttached` metody z dołączonym "Property". W związku z tym w powyższym przykładzie identyfikator dołączona właściwość jest `HasShadowProperty`.

Aby uzyskać więcej informacji o tworzeniu można powiązać właściwości, w tym parametry, które można określić podczas tworzenia, zobacz [tworzenia i używania właściwości możliwej do wiązania](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property).

### <a name="creating-accessors"></a>Tworzenie metody dostępu

Statyczne `Get` *PropertyName* i `Set` *PropertyName* metody są wymagane jako metody dostępu dla dołączona właściwość, w przeciwnym razie wartość właściwości systemu nie będzie można użyć dołączona właściwość. `Get` *PropertyName* akcesor powinny być zgodne z następującą sygnaturą:

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get` *PropertyName* akcesor powinien zwrócić wartość, która jest zawarta w polu `BindableProperty` dla dołączona właściwość pole. Można to osiągnąć poprzez wywołanie [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) metody, przekazując identyfikator właściwości możliwej do wiązania, na którym ma być pobrana wartość, a następnie rzutowanie wynikowej wartości do wymaganego typu.

`Set` *PropertyName* akcesor powinny być zgodne z następującą sygnaturą:

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set` *PropertyName* dostępu należy ustawić wartość odpowiadającego `BindableProperty` dla dołączona właściwość pole. Można to osiągnąć poprzez wywołanie [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) metody, przekazując identyfikator właściwości możliwej do wiązania, dla których należy ustawić wartość i wartość do ustawienia.

Dla obu metod dostępu *docelowej* obiektu powinien mieć lub pochodzić od, [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/).

Poniższy przykład kodu pokazuje metody dostępu dla `HasShadow` dołączona właściwość:

```csharp
public static bool GetHasShadow (BindableObject view)
{
  return (bool)view.GetValue (HasShadowProperty);
}

public static void SetHasShadow (BindableObject view, bool value)
{
  view.SetValue (HasShadowProperty, value);
}
```

### <a name="consuming-an-attached-property"></a>Wykorzystywanie dołączona właściwość

Po utworzeniu dołączona właściwość mogą być używane z XAML lub kodu. W języku XAML jest to osiągane przez deklarowanie przestrzeni nazw z prefiksem z deklaracji przestrzeni nazw wskazujący nazwę przestrzeni nazw środowiska uruchomieniowego języka wspólnego (CLR) i opcjonalnie nazwy zestawu. Aby uzyskać więcej informacji, zobacz [przestrzeń nazw XAML](~/xamarin-forms/xaml/namespaces.md).

Poniższy przykład kodu pokazuje przestrzeni nazw XAML dla niestandardowego typu, który zawiera dołączona właściwość, która jest zdefiniowana w tym samym zestawie co kod aplikacji, która odwołuje się do niestandardowego typu:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

Deklaracja przestrzeni nazw jest używane podczas ustawiania dołączona właściwość na określonego formantu, jak zostało to pokazane w poniższym przykładzie kodu XAML:

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

W poniższym przykładzie kodu pokazano równoważne kodu C#:

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consuming-an-attached-property-with-a-style"></a>Wykorzystywanie dołączona właściwość stylu

Dołączone właściwości można również dodać do formantu w stylu. Przedstawia poniższy przykładowy kod XAML *jawne* styl, który używa `HasShadow` dołączona właściwość, którą można zastosować do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) kontrolki:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Można zastosować do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) przez ustawienie jej [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości `Style` wystąpienia przy użyciu `StaticResource`— rozszerzenie znaczników, jak pokazano w poniższym przykładzie:

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

Aby uzyskać więcej informacji na temat stylów, zobacz [style](~/xamarin-forms/user-interface/styles/index.md).

## <a name="advanced-scenarios"></a>Scenariusze zaawansowane

Podczas tworzenia dołączona właściwość, istnieje wiele parametrów opcjonalnych, które można skonfigurować tak, aby włączyć właściwości dołączonych zaawansowanych scenariuszy. W tym wykrywanie zmian właściwości, sprawdzanie poprawności wartości właściwości i wartości właściwości coercing —. Aby uzyskać więcej informacji, zobacz [zaawansowane scenariusze](~/xamarin-forms/xaml/bindable-properties.md#advanced).

## <a name="summary"></a>Podsumowanie

W tym artykule podane wprowadzenie do dołączonych właściwości, a także przedstawiono sposób tworzenia i korzystać z nich. Dołączona właściwość jest specjalnym rodzajem właściwości możliwej do wiązania, zdefiniowana w jedną klasę, ale dołączone do innych obiektów i rozpoznawalną w języku XAML jako atrybuty, które zawiera klasy i nazwę właściwości oddzielone kropką.


## <a name="related-links"></a>Linki pokrewne

- [Właściwości możliwe do wiązania](~/xamarin-forms/xaml/bindable-properties.md)
- [Przestrzeń nazw XAML](~/xamarin-forms/xaml/namespaces.md)
- [Efektem cienia (przykład)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
