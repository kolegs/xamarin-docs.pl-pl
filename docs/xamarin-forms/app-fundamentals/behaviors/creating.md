---
title: Xamarin.Forms Behaviors
description: "Zachowania platformy Xamarin.Forms są tworzone przez wynikające z działania lub zachowania<T> klasy. W tym artykule przedstawiono sposób tworzenia i korzystać z platformy Xamarin.Forms zachowania."
ms.topic: article
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: c70e4c9ec49b48c3bf6ecc6a4944d992f8ae930a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms Behaviors

_Zachowania platformy Xamarin.Forms są tworzone przez wynikające z działania lub zachowania<T> klasy. W tym artykule przedstawiono sposób tworzenia i korzystać z platformy Xamarin.Forms zachowania._

## <a name="overview"></a>Omówienie

Proces tworzenia zachowanie platformy Xamarin.Forms wygląda następująco:

1. Utwórz klasę, która dziedziczy [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) lub [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) klasy, gdzie `T` jest typ kontroli, do którego należy zastosować zachowanie.
1. Zastąpienie [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) metodę, aby wykonać wszystkie wymagane ustawienia.
1. Zastąpienie [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) metodę, aby wykonać wszelkie wymagane czyszczenie.
1. Implementuje podstawowych funkcji zachowania.

Powoduje to struktura pokazano w poniższym przykładzie:

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

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Metody jest uruchamiany natychmiast po zachowanie jest dołączony do formantu. Ta metoda odbiera odwołanie do formantu, który jest podłączony i może służyć do zarejestrowania programów obsługi zdarzeń lub wykonywać inne ustawienia, które są wymagane do obsługi funkcji zachowanie. Na przykład można utworzyć subskrypcji na zdarzenie w formancie. Zachowanie funkcji mogą być następnie realizowane w programu obsługi zdarzeń dla zdarzenia.

[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Metody uruchamiane, gdy działanie zostanie usunięta z formantu. Ta metoda odbiera odwołanie do formantu, który jest dołączony, a służy do wykonywania wymaganych oczyszczania. Można na przykład subskrypcję zdarzenia, aby zapobiec przecieki pamięci.

Zachowanie mogą być następnie używane przez dołączenie go do [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) kolekcji właściwej opcji kontroli.

## <a name="creating-a-xamarinforms-behavior"></a>Tworzenie zachowanie platformy Xamarin.Forms

Aplikacja przykładowa prezentuje `NumericValidationBehavior`, który prezentuje wartości wprowadzonej przez użytkownika do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kontrolować na czerwono, jeśli nie jest `double`. W poniższym przykładzie kodu pokazano zachowanie:

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

`NumericValidationBehavior` Pochodną [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) klasy, których `T` jest [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Metoda rejestruje program obsługi zdarzeń dla [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) zdarzeń, z [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) metody do rejestrowania `TextChanged`przecieków zdarzenie, aby zapobiec pamięci. Do podstawowych funkcji zachowanie jest zapewniana przez `OnEntryTextChanged` metodę, która analizuje wartości wprowadzonej przez użytkownika do `Entry`i ustawia [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) właściwości czerwony, jeśli wartość nie jest `double`.

> [!NOTE]
> **Uwaga**: platformy Xamarin.Forms nie ustawia `BindingContext` zachowania, ponieważ zachowania może być udostępniony i stosowane do wielu formantów za pomocą stylów.

## <a name="consuming-a-xamarinforms-behavior"></a>Korzystanie z zachowaniem platformy Xamarin.Forms

Każdy kontrolkę platformy Xamarin.Forms [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) kolekcji, do której można dodać jeden lub więcej zachowania, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

Odpowiednik [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) w języku C# przedstawiono w poniższym przykładzie:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

W czasie wykonywania zachowanie będzie odpowiadać interakcji z formantem, zgodnie z implementacją zachowanie. Poniższe zrzuty ekranu pokazują zachowanie odpowiada na nieprawidłowe dane wejściowe:

[ ![](creating-images/screenshots-sml.png "Przykładowa aplikacja z zachowaniem platformy Xamarin.Forms")](creating-images/screenshots.png "Przykładowa aplikacja z zachowaniem platformy Xamarin.Forms")

> [!NOTE]
> **Uwaga**: zachowania są zapisywane dla typu formantu określonego (lub superklasą, które można stosować do wielu formantów), a tylko powinny one być dodane do kontroli zgodne. Próby dołączenia do formantu niezgodne zachowanie wystąpi wyjątek.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Korzystanie z platformy Xamarin.Forms zachowanie o stylu

Zachowania również mogą być używane przez styl jawnych ani niejawnych. Jednak tworzenie styl, który ustawia [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) właściwości formantu nie jest możliwe, ponieważ ta właściwość jest tylko do odczytu. Rozwiązanie jest można dodać właściwości dołączonej do klasy zachowanie, która kontroluje, dodawanie i usuwanie zachowanie. Proces przebiega w następujący sposób:

1. Dodaj właściwości dołączonej do klasy zachowanie, która będzie służyć do kontrolowania dodawania lub usuwania zachowania do formantu do którego zachowanie zostanie dołączony. Upewnij się, że dołączona właściwość rejestruje `propertyChanged` delegata, która zostanie wykonana po zmianie wartości właściwości.
1. Utwórz `static` metody pobierającej i ustawiającej dołączonych właściwości.
1. Wdrożyć logikę w `propertyChanged` delegata do dodawania i usuwania zachowanie.

Poniższy przykład kodu pokazuje dołączona właściwość, która kontroluje, dodawanie i usuwanie `NumericValidationBehavior`:

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

`NumericValidationBehavior` Klasa zawiera dołączona właściwość o nazwie `AttachBehavior` z `static` pobierającej i ustawiającej, który kontroluje, dodawanie lub usuwanie zachowania do formantu, z którą zostanie dołączony. To dołączona właściwość rejestrów `OnAttachBehaviorChanged` — metoda, która zostanie wykonana po zmianie wartości właściwości. Ta metoda dodaje lub usuwa zachowania do kontrolki, na podstawie wartości z `AttachBehavior` dołączona właściwość.

Poniższy kod przedstawia przykład *jawne* stylów dla `NumericValidationBehavior` używającą `AttachBehavior` dołączona właściwość i mogą być stosowane do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kontrolki:

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Można zastosować do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kontroli przez ustawienie jej [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości `Style` wystąpienia przy użyciu `StaticResource` — rozszerzenie znaczników, jak pokazano w poniższym przykładzie:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Aby uzyskać więcej informacji na temat stylów, zobacz [style](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> **Uwaga**: możliwe jest dodawanie właściwości do zachowania, ustawić lub zapytanie w języku XAML, jeśli Tworzenie zachowania mają stan ich nie mogą być udostępniane między formantami w `Style` w `ResourceDictionary`.

### <a name="removing-a-behavior-from-a-control"></a>Usuwanie zachowanie za pomocą formantu

[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Metody jest wywoływane podczas zachowanie zostanie usunięta z formantu i służy do wykonywania wymaganych oczyszczania takich jak anulowanie subskrypcji zdarzeń, aby zapobiec przeciek pamięci. Jednak zachowania nie są domyślnie usuwane z formantów chyba że formantu [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) kolekcji jest modyfikowany przez `Remove` lub `Clear` metody. Poniższy przykład kodu pokazuje, usuwając określone zachowanie z formantu `Behaviors` kolekcji:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Alternatywnie formantu [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) można wyczyścić kolekcji, jak pokazano w poniższym przykładzie:

```csharp
entry.Behaviors.Clear();
```

Ponadto należy pamiętać, że zachowania nie są niejawnie usuwane z formantów podczas strony są zdjęte ze stosu nawigacji ze stosu. Zamiast tego one musi jawnie usunięte przed stron przechodzi poza zakresem.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób tworzenia i korzystać z platformy Xamarin.Forms zachowania. Zachowania platformy Xamarin.Forms są tworzone przez pochodny [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) lub [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) klasy.


## <a name="related-links"></a>Linki pokrewne

- [Zachowanie platformy Xamarin.Forms (przykład)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Zachowanie platformy Xamarin.Forms stosowane przy użyciu stylu (przykład)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [Behavior](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Zachowanie<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
