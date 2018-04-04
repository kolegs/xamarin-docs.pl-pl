---
title: Zachowania dołączone
description: Klasy statyczne z co najmniej jednej właściwości dołączonych będą dołączone zachowania. W tym artykule przedstawiono sposób tworzenia i zużywać przyłączonych zachowań.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: af891ad1ff1389d5a48c6c47ba1914b8d4dfc20f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="attached-behaviors"></a>Zachowania dołączone

_Klasy statyczne z co najmniej jednej właściwości dołączonych będą dołączone zachowania. W tym artykule przedstawiono sposób tworzenia i zużywać przyłączonych zachowań._

## <a name="overview"></a>Omówienie

Dołączona właściwość jest specjalnym rodzajem właściwości możliwej do wiązania. Są zdefiniowane w jedną klasę, ale dołączone do innych obiektów, i są one rozpoznawalną w języku XAML jako atrybuty, które zawiera klasy i nazwę właściwości oddzielone kropką.

Dołączona właściwość można zdefiniować `propertyChanged` delegata, która zostanie wykonana po zmianie wartości właściwości, np. gdy właściwość jest ustawiona w formancie. Gdy `propertyChanged` wykonuje delegata, został przekazany odwołanie do sterowania, na którym jest dołączany i parametrów, które zawierają starej i nowej wartości właściwości. Ten delegat może służyć do dodania nowych funkcji do kontroli, podłączonego do właściwości przez manipulowanie odwołania przekazywane w następujący sposób:

1. `propertyChanged` Delegata rzutuje odwołania formantu, który otrzymuje się [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), do typu formantu, który jest zaprojektowana, aby zwiększyć.
1. `propertyChanged` Delegata modyfikuje właściwości formantu, metody wywołania formantu lub rejestrów obsługi zdarzeń dla zdarzenia udostępnianych przez formant do implementacji podstawowych funkcji zachowanie.

Wystąpił problem z przyłączonych zachowań jest zostały zdefiniowane w `static` klasy, z `static` właściwości i metody. Utrudnia można utworzyć dołączonego zachowania, które mają stan. Ponadto zachowania platformy Xamarin.Forms zastąpił zachowania dołączone jako preferowane podejście do budowy zachowanie. Aby uzyskać więcej informacji na temat platformy Xamarin.Forms zachowania, zobacz [zachowania platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md) i [zachowania wielokrotnego użytku](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

## <a name="creating-an-attached-behavior"></a>Tworzenie dołączone zachowanie

Aplikacja przykładowa prezentuje `NumericValidationBehavior`, który prezentuje wartości wprowadzonej przez użytkownika do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kontrolować na czerwono, jeśli nie jest `double`. W poniższym przykładzie kodu pokazano zachowanie:

```csharp
public static class NumericValidationBehavior
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached (
            "AttachBehavior",
            typeof(bool),
            typeof(NumericValidationBehavior),
            false,
            propertyChanged:OnAttachBehaviorChanged);

    public static bool GetAttachBehavior (BindableObject view)
    {
        return (bool)view.GetValue (AttachBehaviorProperty);
    }

    public static void SetAttachBehavior (BindableObject view, bool value)
    {
        view.SetValue (AttachBehaviorProperty, value);
    }

    static void OnAttachBehaviorChanged (BindableObject view, object oldValue, object newValue)
    {
        var entry = view as Entry;
        if (entry == null) {
            return;
        }

        bool attachBehavior = (bool)newValue;
        if (attachBehavior) {
            entry.TextChanged += OnEntryTextChanged;
        } else {
            entry.TextChanged -= OnEntryTextChanged;
        }
    }

    static void OnEntryTextChanged (object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior` Klasa zawiera dołączona właściwość o nazwie `AttachBehavior` z `static` pobierającej i ustawiającej, który kontroluje, dodawanie lub usuwanie zachowania do formantu, z którą zostanie dołączony. To dołączona właściwość rejestrów `OnAttachBehaviorChanged` — metoda, która zostanie wykonana po zmianie wartości właściwości. Ta metoda rejestruje lub cofnąć rejestruje program obsługi zdarzeń dla [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) zdarzenia na podstawie wartości z `AttachBehavior` dołączona właściwość. Do podstawowych funkcji zachowanie jest zapewniana przez `OnEntryTextChanged` metodę, która analizuje wartość wprowadzona w [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) przez użytkownika i zestawy `TextColor` właściwości czerwony, jeśli wartość nie jest `double`.

## <a name="consuming-an-attached-behavior"></a>Wykorzystywanie dołączone zachowanie

`NumericValidationBehavior` Klasy mogą być używane przez dodanie `AttachBehavior` dołączona właściwość do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kontroli, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

Odpowiednik [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) w języku C# przedstawiono w poniższym przykładzie:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

W czasie wykonywania zachowanie będzie odpowiadać interakcji z formantem, zgodnie z implementacją zachowanie. Poniższe zrzuty ekranu pokazują dołączone zachowanie odpowiada na nieprawidłowe dane wejściowe:

[![](attached-images/screenshots-sml.png "Przykładowa aplikacja z zachowaniem dołączonych")](attached-images/screenshots.png#lightbox "Przykładowa aplikacja z zachowaniem dołączone")

> [!NOTE]
> Zachowania dołączone są przeznaczone dla typu formantu określonego (lub superklasą, które można stosować do wielu formantów), a tylko powinny one być dodane do kontroli zgodne. Próby dołączenia do formantu niezgodne zachowanie spowoduje zachowanie nieznany i zależy od implementacji zachowanie.

### <a name="removing-an-attached-behavior-from-a-control"></a>Usuwanie dołączonych zachowanie za pomocą formantu

`NumericValidationBehavior` Można usunąć klasy z formantu przez ustawienie `AttachBehavior` dołączona właściwość do `false`, jak to pokazano w poniższym przykładzie kodu XAML:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

Odpowiednik [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) w języku C# przedstawiono w poniższym przykładzie:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

W czasie wykonywania `OnAttachBehaviorChanged` metoda będzie wykonywane, kiedy wartość `AttachBehavior` ustawiono dołączona właściwość `false`. `OnAttachBehaviorChanged` Metody następnie zarejestruje usuwania programu obsługi zdarzeń dla [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) zdarzeń, zapewniając, że zachowanie nie jest uruchomione jako użytkownik wchodzi w interakcję z formantem.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób tworzenia i zużywać przyłączonych zachowań. Zachowania dołączone są `static` klas z co najmniej jednego z dołączonych właściwości.


## <a name="related-links"></a>Linki pokrewne

- [Dołączone zachowania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
