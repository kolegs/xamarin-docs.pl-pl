---
title: Dołączone właściwości
description: Ten artykuł zawiera wprowadzenie do dołączonych właściwości i pokazuje, jak utworzyć i korzystać z nich.
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 981e59fe3ba8c63d0f6c6a067ceb9f338a02da8f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997335"
---
# <a name="attached-properties"></a>Dołączone właściwości

_Dołączona właściwość to specjalny rodzaj właściwości możliwej do wiązania, zdefiniowane w jedną klasę, ale dołączone do innych obiektów i rozpoznawalną w XAML jako atrybutu, który zawiera klasę i nazwę właściwości oddzielony kropką. Ten artykuł zawiera wprowadzenie do dołączonych właściwości i pokazuje, jak utworzyć i korzystać z nich._

## <a name="overview"></a>Omówienie

Dołączone właściwości Włącz obiekt można przypisać wartości dla właściwości, która nie określa własnej klasy. Na przykład podrzędnych, których można używać elementów dołączonych właściwości, aby poinformować ich element nadrzędny, jak są przedstawiane w interfejsie użytkownika. [ `Grid` ](xref:Xamarin.Forms.Grid) Kontrolka zezwala na wiersza i kolumny można określić, ustawiając element podrzędny `Grid.Row` i `Grid.Column` dołączonych właściwości. `Grid.Row` i `Grid.Column` są dołączone właściwości, ponieważ są ustawiane w przypadku elementów, które są elementami podrzędnymi `Grid`, a nie na `Grid` sam.

Właściwości możliwe do wiązania powinny zostać wdrożone jako dołączone właściwości w następujących scenariuszach:

- Gdy istnieje potrzeba umożliwienia mechanizm ustawienie właściwości dostępnych dla klas w innych niż Definiowanie klasy.
- Gdy klasa reprezentuje to usługa, która musi można łatwo integrować z innych klas.

Aby uzyskać więcej informacji na temat właściwości możliwej do wiązania, zobacz [właściwości możliwej do wiązania](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="creating-and-consuming-an-attached-property"></a>Tworzenie i korzystanie z dołączoną właściwość

Proces tworzenia dołączoną właściwość jest następująca:

1. Tworzenie [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) wystąpienie z jednym z [ `CreateAttached` ](xref:Xamarin.Forms.BindableProperty.CreateAttached*) przeciążenia metody.
1. Podaj `static` `Get` *PropertyName* i `Set` *PropertyName* metod jako Akcesory dla dołączonych właściwości.

### <a name="creating-a-property"></a>Tworzenie właściwości

Podczas tworzenia dołączoną właściwość do użycia na inne typy, klasy, w którym zostanie utworzona właściwość nie musi pochodzić od [ `BindableObject` ](xref:Xamarin.Forms.BindableObject). Jednak *docelowej* właściwość metody dostępu powinny być lub pochodzić od, [ `BindableObject` ](xref:Xamarin.Forms.BindableObject).

Dołączona właściwość mogą być tworzone przez zadeklarowanie `public static readonly` właściwości typu [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty). Właściwości możliwej do wiązania powinna być równa zwrócona wartość jednego z [ `BindableProperty.CreateAttached` ](xref:Xamarin.Forms.BindableProperty.CreateAttached(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) przeciążenia metody. Deklaracja powinna być w treści klasy będącej właścicielem, ale poza żadnych definicji elementu członkowskiego.

Poniższy kod przedstawia przykład dołączoną właściwość:

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

Spowoduje to utworzenie dołączoną właściwość o nazwie `HasShadow`, typu `bool`. Właściwość jest własnością `ShadowEffect` klasy, a wartość domyślna `false`. Konwencje nazewnictwa w przypadku dołączonych właściwości jest, że identyfikator dołączonej właściwości musi odpowiadać nazwa właściwości określone w `CreateAttached` metody, z dołączoną "Property". W związku z tym, w powyższym przykładzie identyfikator dołączoną właściwość jest `HasShadowProperty`.

Aby uzyskać więcej informacji o tworzeniu właściwości może być powiązana, w tym parametry, które można określić podczas tworzenia, zobacz [tworzenia i używania właściwości możliwej do wiązania](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property).

### <a name="creating-accessors"></a>Tworzenie metod dostępu

Statyczne `Get` *PropertyName* i `Set` *PropertyName* metody są wymagane jako Akcesory dla dołączonych właściwości, w przeciwnym razie system właściwość będzie mógł użyć dołączona właściwość. `Get` *PropertyName* dostępu powinny odpowiadać następujący podpis:

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get` *PropertyName* akcesor powinna zwrócić wartość, która jest zawarta w odpowiednich `BindableProperty` pole dołączona właściwość. Można to osiągnąć przez wywołanie metody [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) metody, przekazując identyfikatora właściwości możliwej do wiązania, na którym ma zostać pobrana wartość, a następnie rzutowania wartość wynikowa na wymagany typ.

`Set` *PropertyName* dostępu powinny odpowiadać następujący podpis:

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set` *PropertyName* dostępu należy określić wartość odpowiadającego `BindableProperty` pole dołączona właściwość. Można to osiągnąć przez wywołanie metody [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) metody, przekazując identyfikatora właściwości możliwej do wiązania, dla której chcesz ustawić wartość i wartość do ustawienia.

Dla obu metod dostępu *docelowej* obiekt powinien być lub pochodzić od, [ `BindableObject` ](xref:Xamarin.Forms.BindableObject).

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

### <a name="consuming-an-attached-property"></a>Korzystanie z dołączoną właściwość

Po utworzeniu dołączonej właściwości mogą być używane z XAML lub kodu. W XAML jest to osiągane przez zadeklarowanie przestrzeni nazw z prefiksem, za pomocą deklaracji przestrzeni nazw, wskazującą nazwę przestrzeni nazw środowiska uruchomieniowego języka wspólnego (CLR) i opcjonalnie nazwy zestawu. Aby uzyskać więcej informacji, zobacz [przestrzeni nazw XAML](~/xamarin-forms/xaml/namespaces.md).

Poniższy przykład kodu demonstruje typ niestandardowy, który zawiera dołączoną właściwość, która jest zdefiniowana w ramach tego samego zestawu jako kod aplikacji, który odwołuje się do niestandardowego typu przestrzeni nazw XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

Deklaracja przestrzeni nazw jest następnie używany, gdy ustawienie dołączonych właściwości w formancie określonych pokazano w poniższym przykładzie kodu XAML:

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

Równoważny kod C# pokazano w poniższym przykładzie kodu:

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consuming-an-attached-property-with-a-style"></a>Korzystanie z dołączoną właściwość stylu

Dołączone właściwości można dodać również kontrolkę, styl. Ilustruje poniższy przykład kodu XAML *jawne* styl, który używa `HasShadow` dołączonych właściwości, które mogą być stosowane do [ `Label` ](xref:Xamarin.Forms.Label) kontrolki:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

[ `Style` ](xref:Xamarin.Forms.Style) Mogą być stosowane do [ `Label` ](xref:Xamarin.Forms.Label) , ustawiając jego [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości `Style` przy użyciu `StaticResource`— rozszerzenie znaczników, jak pokazano w poniższym przykładzie kodu:

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

Aby uzyskać więcej informacji na temat style zobacz [style](~/xamarin-forms/user-interface/styles/index.md).

## <a name="advanced-scenarios"></a>Scenariusze zaawansowane

Podczas tworzenia dołączoną właściwość, istnieje kilka parametrów opcjonalnych, które można ustawić właściwości dołączone zaawansowanych scenariuszy. Obejmuje to wykrywanie zmian właściwości sprawdzania poprawności wartości właściwości i wartości właściwości coercing —. Aby uzyskać więcej informacji, zobacz [zaawansowane scenariusze](~/xamarin-forms/xaml/bindable-properties.md#advanced).

## <a name="summary"></a>Podsumowanie

W tym artykule podano wprowadzenie dołączone właściwości, a następnie pokazano, jak utworzyć i korzystać z nich. Dołączona właściwość to specjalny rodzaj właściwości możliwej do wiązania, zdefiniowane w jedną klasę, ale dołączone do innych obiektów i rozpoznawalny w XAML jako atrybuty, które zawierają klasy i nazwy właściwości oddzielony kropką.


## <a name="related-links"></a>Linki pokrewne

- [Właściwości możliwe do wiązania](~/xamarin-forms/xaml/bindable-properties.md)
- [Przestrzeń nazw XAML](~/xamarin-forms/xaml/namespaces.md)
- [Efektem cienia (przykład)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](xref:Xamarin.Forms.BindableProperty)
- [BindableObject](xref:Xamarin.Forms.BindableObject)
