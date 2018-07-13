---
title: Dołączone zachowania
description: Dołączone zachowania są statyczne klasy z jedną lub więcej właściwości dołączone. W tym artykule przedstawiono sposób tworzenia i wykorzystywania dołączone zachowania.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 2c9bd9ad4e7572b9eae6f0073da8a2c8f1e7c9fc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995349"
---
# <a name="attached-behaviors"></a>Dołączone zachowania

_Dołączone zachowania są statyczne klasy z jedną lub więcej właściwości dołączone. W tym artykule przedstawiono sposób tworzenia i wykorzystywania dołączone zachowania._

## <a name="overview"></a>Omówienie

Dołączona właściwość to specjalny rodzaj właściwości możliwej do wiązania. Są one zdefiniowane w jedną klasę, ale dołączone do innych obiektów i są rozpoznawalne w XAML jako atrybuty, które zawierają klasy i nazwy właściwości oddzielony kropką.

Dołączona właściwość można zdefiniować `propertyChanged` delegata, która zostanie wykonana po zmianie wartości właściwości, np. gdy właściwość jest ustawiona na kontrolce. Gdy `propertyChanged` wykonuje delegata, został przekazany, odwołanie do kontrolki, na którym jest dołączany i parametry, które zawierają stare i nowe wartości właściwości. Ten delegat może służyć do dodawania nowych funkcji do kontroli, dołączonego do właściwości poprzez manipulowanie odwołania, który jest przekazywany, w następujący sposób:

1. `propertyChanged` Delegata rzutuje odwołania kontrolki, które są odbierane w postaci [ `BindableObject` ](xref:Xamarin.Forms.BindableObject), typ formantu zachowanie jest przeznaczone do zwiększenia.
1. `propertyChanged` Delegata modyfikuje właściwości formantu, metody wywołania formantu lub procedury obsługi zdarzeń rejestrów dla zdarzeń udostępnianych przez formant implementuje podstawowe funkcje zachowanie.

Wystąpił problem z dołączone zachowania to, że są one zdefiniowane w `static` klasy, za pomocą `static` właściwości i metody. Utrudnia to utworzyć dołączone zachowania, które mają stan. Ponadto zachowania zestawu narzędzi Xamarin.Forms zastąpiono dołączone zachowania jako preferowane podejście do konstruowania zachowanie. Aby uzyskać więcej informacji na temat zachowania zestawu narzędzi Xamarin.Forms, zobacz [zachowania zestawu narzędzi Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md) i [zachowania wielokrotnego użytku, do](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

## <a name="creating-an-attached-behavior"></a>Tworzenie dołączone zachowania

Przykładowa aplikacja demonstruje `NumericValidationBehavior`, wyróżnia wartości wprowadzonej przez użytkownika do [ `Entry` ](xref:Xamarin.Forms.Entry) kontrolować na czerwono, jeśli nie jest `double`. W poniższym przykładzie kodu pokazano zachowanie:

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

`NumericValidationBehavior` Klasa zawiera dołączoną właściwość o nazwie `AttachBehavior` z `static` metodę getter i setter, który kontroluje, dodawania lub usuwania zachowanie do kontroli, do którego zostanie dołączony. To dołączonych właściwości rejestrów `OnAttachBehaviorChanged` metody, która zostanie wykonana po zmianie wartości właściwości. Ta metoda rejestruje lub cofnąć rejestruje zdarzenia obsługi dla [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) , oparte na zdarzeniach na wartości `AttachBehavior` dołączona właściwość. Podstawowe funkcje zachowanie odbywa się przy `OnEntryTextChanged` metody, która analizuje wartość zawierana [ `Entry` ](xref:Xamarin.Forms.Entry) przez użytkownika, a następnie ustawia `TextColor` właściwości na czerwony, jeśli wartość nie jest `double`.

## <a name="consuming-an-attached-behavior"></a>Korzystanie z dołączone zachowania

`NumericValidationBehavior` Klasy mogą być używane przez dodanie `AttachBehavior` dołączonych właściwości [ `Entry` ](xref:Xamarin.Forms.Entry) kontrolować, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

Odpowiednik [ `Entry` ](xref:Xamarin.Forms.Entry) w języku C# pokazano w poniższym przykładzie kodu:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

W czasie wykonywania zachowanie będzie odpowiadać na interakcję z kontrolki, zgodnie z implementacji zachowania. Poniższe zrzuty ekranu pokazują dołączone zachowania, które odpowiada na nieprawidłowe dane wejściowe:

[![](attached-images/screenshots-sml.png "Przykładowa aplikacja z zachowaniem dołączonych")](attached-images/screenshots.png#lightbox "Przykładowa aplikacja z zachowaniem dołączone")

> [!NOTE]
> Dołączone zachowania zostały napisane dla typu określonego (lub superklasą, które można zastosować do wielu formantów), a tylko powinny one być dodane do kontroli zgodne. Próby dołączenia zachowania do niezgodnych kontrolki spowoduje zachowanie nieznany i jest zależna od implementacji zachowania.

### <a name="removing-an-attached-behavior-from-a-control"></a>Usuwanie dołączonych zachowanie kontrolki

`NumericValidationBehavior` Można usunąć klasy z formantu przez ustawienie `AttachBehavior` dołączonych właściwości `false`, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

Odpowiednik [ `Entry` ](xref:Xamarin.Forms.Entry) w języku C# pokazano w poniższym przykładzie kodu:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

W czasie wykonywania `OnAttachBehaviorChanged` metoda będzie wykonywane, kiedy wartość `AttachBehavior` dołączonej właściwości jest równa `false`. `OnAttachBehaviorChanged` Metoda zostanie następnie wyrejestrowywać program obsługi zdarzeń dla [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) zdarzeń, zapewniając, że zachowanie nie jest wykonywane jako użytkownik wchodzi w interakcję z kontrolką.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób tworzenia i wykorzystywania dołączone zachowania. Dołączone zachowania są `static` klas z jedną lub więcej właściwości dołączone.


## <a name="related-links"></a>Linki pokrewne

- [Dołączone zachowania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
