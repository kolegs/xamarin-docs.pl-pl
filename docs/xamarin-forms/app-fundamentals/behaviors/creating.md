---
title: Zachowania zestawu narzędzi Xamarin.Forms
description: Zachowania zestawu narzędzi Xamarin.Forms są tworzone przez wynikających z zachowania lub zachowanie<T> klasy. W tym artykule przedstawiono sposób tworzenia i wykorzystywania zachowania zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 7e057567ec0bb72e9bcc016d4a9fef3af78a3ea1
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998898"
---
# <a name="xamarinforms-behaviors"></a>Zachowania zestawu narzędzi Xamarin.Forms

_Zachowania zestawu narzędzi Xamarin.Forms są tworzone przez wynikających z zachowania lub zachowanie<T> klasy. W tym artykule przedstawiono sposób tworzenia i wykorzystywania zachowania zestawu narzędzi Xamarin.Forms._

## <a name="overview"></a>Omówienie

Proces tworzenia zachowania zestawu narzędzi Xamarin.Forms jest następująca:

1. Utwórz klasę, która dziedziczy z [ `Behavior` ](xref:Xamarin.Forms.Behavior) lub [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) klasy, gdzie `T` jest typu formantu, którego powinien dotyczyć zachowanie.
1. Zastąp [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) metodę w celu konieczności przeprowadzania konfiguracji.
1. Zastąp [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) metodę w celu czyszczenia wymagane.
1. Implementuje podstawowe funkcje zachowanie.

Skutkuje to struktura pokazano w poniższym przykładzie kodu:

```csharp
public class CustomBehavior : Behavior<View>
{
    protected override void OnAttachedTo (View bindable)
    {
        base.OnAttachedTo (bindable);
        // Perform setup
    }

    protected override void OnDetachingFrom (View bindable)
    {
        base.OnDetachingFrom (bindable);
        // Perform clean up
    }

    // Behavior implementation
}
```

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Metody jest uruchamiany natychmiast po, zachowaniem jest dołączony do formantu. Ta metoda otrzymuje odwołanie do formantu, który jest dołączony i może służyć do rejestrowania procedury obsługi zdarzeń lub wykonywać inne ustawienia, które są wymagane do obsługi funkcji zachowanie. Na przykład można subskrybować zdarzenie na kontrolce. Zachowanie funkcji następnie mogą być realizowane w program obsługi zdarzeń dla zdarzenia.

[ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Metody jest wywoływane, gdy zachowanie zostanie usunięty z kontroli. Ta metoda otrzymuje odwołanie do formantu, który jest dołączony i jest używany do wykonywania oczyszczania wymagane. Na przykład można anulować subskrypcję ze zdarzenia kontroli, aby zapobiec przeciekom pamięci.

To zachowanie może być bezpiecznie spożywane, dołączając ją do [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) zbiór właściwej opcji kontroli.

## <a name="creating-a-xamarinforms-behavior"></a>Tworzenie zachowania zestawu narzędzi Xamarin.Forms

Przykładowa aplikacja demonstruje `NumericValidationBehavior`, wyróżnia wartości wprowadzonej przez użytkownika do [ `Entry` ](xref:Xamarin.Forms.Entry) kontrolować na czerwono, jeśli nie jest `double`. W poniższym przykładzie kodu pokazano zachowanie:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    protected override void OnAttachedTo(Entry entry)
    {
        entry.TextChanged += OnEntryTextChanged;
        base.OnAttachedTo(entry);
    }

    protected override void OnDetachingFrom(Entry entry)
    {
        entry.TextChanged -= OnEntryTextChanged;
        base.OnDetachingFrom(entry);
    }

    void OnEntryTextChanged(object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior` Pochodzi od klasy [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) klasy, gdzie `T` jest [ `Entry` ](xref:Xamarin.Forms.Entry). [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Metoda rejestruje zdarzenia obsługi dla [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) zdarzenia z [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) metoda wyrejestrowywanie `TextChanged`przecieków zdarzenie, aby zapobiec pamięci. Podstawowe funkcje zachowanie odbywa się przy `OnEntryTextChanged` metody, która analizuje wartości wprowadzonej przez użytkownika do `Entry`i ustawia [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) właściwości na czerwony, jeśli wartość nie jest `double`.

> [!NOTE]
> Zestaw narzędzi Xamarin.Forms nie ustawia `BindingContext` zachowania, ponieważ zachowania może być udostępniane i zastosować do wielu kontrolek przy użyciu stylów.

## <a name="consuming-a-xamarinforms-behavior"></a>Korzystanie z zachowania zestawu narzędzi Xamarin.Forms

Każdy formant Xamarin.Forms ma [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) kolekcji, do którego można dodać co najmniej jeden zachowań, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

Odpowiednik [ `Entry` ](xref:Xamarin.Forms.Entry) w języku C# pokazano w poniższym przykładzie kodu:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

W czasie wykonywania zachowanie będzie odpowiadać na interakcję z kontrolki, zgodnie z implementacji zachowania. Poniższe zrzuty ekranu pokazują to zachowanie, które odpowiada na nieprawidłowe dane wejściowe:

[![](creating-images/screenshots-sml.png "Przykładowa aplikacja z zachowaniem Xamarin.Forms")](creating-images/screenshots.png#lightbox "Przykładowa aplikacja z działaniem zestawu narzędzi Xamarin.Forms")

> [!NOTE]
> Zachowania są przeznaczone dla typu określonego (lub superklasą, które można zastosować do wielu formantów), a tylko powinny one być dodane do kontroli zgodne. Próby dołączenia zachowania do niezgodnych kontrolki spowoduje wyjątek.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Korzystanie z zachowania zestawu narzędzi Xamarin.Forms przy użyciu stylu

Zachowania mogą być również używane przez style jawne lub niejawne. Jednak tworzenie stylu, który ustawia [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) właściwości kontrolki nie jest możliwe, ponieważ właściwość jest tylko do odczytu. To rozwiązanie jest dodać dołączoną właściwość do klasy zachowanie, który kontroluje, dodając i usuwając zachowanie. Proces przebiega w następujący sposób:

1. Dodaj dołączonej właściwości do klasy zachowanie, która będzie służyć do kontrolowania, dodawania lub usuwania zachowanie do kontroli, do którego zostanie zachowanie dołączone. Upewnij się, że dołączona właściwość rejestruje `propertyChanged` delegata, która zostanie wykonana po zmianie wartości właściwości.
1. Utwórz `static` metodę getter i setter dla dołączonych właściwości.
1. Implementowanie logiki `propertyChanged` delegata do dodawania i usuwania zachowanie.

Poniższy przykład kodu pokazuje dołączona właściwość, która kontroluje, dodając i usuwając `NumericValidationBehavior`:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached ("AttachBehavior", typeof(bool), typeof(NumericValidationBehavior), false, propertyChanged: OnAttachBehaviorChanged);

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
            entry.Behaviors.Add (new NumericValidationBehavior ());
        } else {
            var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
            if (toRemove != null) {
                entry.Behaviors.Remove (toRemove);
            }
        }
    }
    ...
}
```

`NumericValidationBehavior` Klasa zawiera dołączoną właściwość o nazwie `AttachBehavior` z `static` metodę getter i setter, który kontroluje, dodawania lub usuwania zachowanie do kontroli, do którego zostanie dołączony. To dołączonych właściwości rejestrów `OnAttachBehaviorChanged` metody, która zostanie wykonana po zmianie wartości właściwości. Ta metoda dodaje lub usuwa zachowania kontrolki, w oparciu o wartość `AttachBehavior` dołączona właściwość.

Poniższy kod przedstawia przykład *jawne* stylu dla `NumericValidationBehavior` , który używa `AttachBehavior` dołączonych właściwości i mogą być stosowane do [ `Entry` ](xref:Xamarin.Forms.Entry) kontrolki:

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

[ `Style` ](xref:Xamarin.Forms.Style) Mogą być stosowane do [ `Entry` ](xref:Xamarin.Forms.Entry) kontroli przez ustawienie jego [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości `Style` przy użyciu `StaticResource` — rozszerzenie znaczników, jak pokazano w poniższym przykładzie kodu:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Aby uzyskać więcej informacji na temat style zobacz [style](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> Podczas dodawania, może być powiązana właściwości w celu zachowania, które zostaje ustawiona lub zapytania w XAML, jeśli utworzysz zachowania mają stan one nie mogą być udostępniane między kontrolkami w `Style` w `ResourceDictionary`.

### <a name="removing-a-behavior-from-a-control"></a>Usuwanie zachowanie kontrolki

[ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Metoda uruchamiane, gdy zachowanie zostanie usunięty z formantu i służy do przeprowadzania wymagane czyszczenia takich jak anulowanie subskrypcji zdarzeń, aby zapobiec przeciek pamięci. Jednak zachowania nie są niejawnie usuwane z formantów chyba że formantu [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) kolekcji jest modyfikowany przez `Remove` lub `Clear` metody. Poniższy przykład kodu demonstruje, usuwanie określone zachowanie kontrolki `Behaviors` kolekcji:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Alternatywnie formantu [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) można wyczyścić kolekcji, jak pokazano w poniższym przykładzie kodu:

```csharp
entry.Behaviors.Clear();
```

Ponadto należy pamiętać, że zachowania nie są niejawnie usuwane z formantów podczas strony są zdjęte ze stosu nawigacji ze stosu. Zamiast tego one muszą być jawnie usunięte przed stron wychodzące z zakresu.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób tworzenia i wykorzystywania zachowania zestawu narzędzi Xamarin.Forms. Zachowania zestawu narzędzi Xamarin.Forms są tworzone przez pochodząca od [ `Behavior` ](xref:Xamarin.Forms.Behavior) lub [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) klasy.


## <a name="related-links"></a>Linki pokrewne

- [Zachowania zestawu narzędzi Xamarin.Forms (przykład)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Zachowania zestawu narzędzi Xamarin.Forms stosowane przy użyciu stylu (przykład)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [Behavior](xref:Xamarin.Forms.Behavior)
- [Zachowanie<T>](xref:Xamarin.Forms.Behavior`1)
