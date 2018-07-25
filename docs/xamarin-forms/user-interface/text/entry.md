---
title: Wpis zestawu narzędzi Xamarin.Forms
description: W tym artykule opisano sposób użycia klasy wpisu zestawu narzędzi Xamarin.Forms do akceptowania jednego wiersza tekstu lub wprowadź hasło w aplikacji.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/16/2018
ms.openlocfilehash: 272887f0abb0785f959c542e65789d7645a583f1
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241228"
---
# <a name="xamarinforms-entry"></a>Wpis zestawu narzędzi Xamarin.Forms

_Jednego wiersza tekstu lub hasła, dane wejściowe_

Xamarin.Forms `Entry` służy do wprowadzania tekstu jednowierszowego. `Entry`, Takiej jak `Editor` wyświetlanie, obsługuje wiele typów klawiatury. Ponadto `Entry` mogą być używane jako pole hasła.

## <a name="display-customization"></a>Dostosowywanie ekranu

### <a name="setting-and-reading-text"></a>Ustawianie i odczytywanie tekstu

`Entry`, Podobnie jak inne widoki przedstawiania tekstu, udostępnia `Text` właściwości. Ta właściwość umożliwia ustawianie i odczytywanie tekstu przedstawiony przez `Entry`. W poniższym przykładzie pokazano ustawienie `Text` właściwości w XAML:

```xaml
<Entry Text="I am an Entry" />
```

W języku C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Aby odczytać tekstu, dostęp do `Text` właściwości w języku C#:

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> Szerokość `Entry` można zdefiniować, ustawiając jego `WidthRequest` właściwości. Nie są zależne od szerokość `Entry` definiowanego na podstawie wartości jego `Text` właściwości.

### <a name="limiting-input-length"></a>Ograniczenie długości danych wejściowych

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Właściwość może służyć do ograniczania długości danych wejściowych, które są dozwolone dla [ `Entry` ](xref:Xamarin.Forms.Entry). Ta właściwość powinna być ustawiona na dodatnią liczbą całkowitą:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) właściwości wartość 0 oznacza, że może być brak danych wejściowych, a wartość `int.MaxValue`, co jest wartością domyślną dla [ `Entry` ](xref:Xamarin.Forms.Entry), wskazuje, że istnieje nie skuteczne limit liczby znaków, które mogą być wprowadzane.

### <a name="customizing-the-keyboard"></a>Dostosowywanie klawiatury

Klawiatura, które są prezentowane podczas interakcji z [ `Entry` ](xref:Xamarin.Forms.Entry) można ustawić programowo za pośrednictwem [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) właściwości do jednej z następujących właściwości z [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) klasy:

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) — używany do badań i miejsc, w którym emoji są przydatne.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) — Klawiatura domyślne.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) — używany podczas wprowadzania adresu e-mail.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) — używany podczas wprowadzania liczb.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) — używany podczas wprowadzania tekstu, bez jakichkolwiek [ `KeyboardFlags` ](xref:Xamarin.Forms.KeyboardFlags) określony.
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) — używany podczas wprowadzania numerów telefonów.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) — używany podczas wprowadzania tekstu.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) — używany do wprowadzania ścieżki do plików i adresy witryn sieci web.

Można to zrobić w XAML w następujący sposób:

```xaml
<Entry Keyboard="Chat" />
```

Jest równoważny kod C#:

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
```

Przykłady każdego klawiatury można znaleźć w naszej [przepisy](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) repozytorium.

[ `Keyboard` ](xref:Xamarin.Forms.Keyboard) Ma również klasy [ `Create` ](xref:Xamarin.Forms.Keyboard.Create*) metoda fabryki, który może służyć do dostosowywania klawiatury, określając zachowanie wielkości liter, Sprawdź pisownię i sugestii. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) wartości wyliczenia podaną jako argumenty do metody, przy użyciu dostosowanych `Keyboard` zwracanego. `KeyboardFlags` Wyliczenie zawiera następujące wartości:

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) — żadne funkcje nie są dodawane do klawiatury.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) — Wskazuje, że pierwszą literę pierwszy wyraz każdego wpisanego zdania będą automatycznie wpisać wielkimi literami.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) — Wskazuje tego sprawdzania pisowni zostanie przeprowadzone wpisanego tekstu.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) — Wskazuje słowo uzupełnienia będą oferowane na wpisanego tekstu.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) — Wskazuje, że pierwszą literę każdego wyrazu będzie automatycznie wpisać wielkimi literami.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) — Wskazuje, że każdy znak zostanie automatycznie wpisać wielkimi literami.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) — Wskazuje, nastąpi nie automatyczne wielkie litery.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) — Wskazuje, czy sprawdzanie pisowni, zakończenia słowa i zdania wielkość liter wystąpią na wprowadzony tekst.

Poniższy przykład kodu XAML pokazuje, jak dostosować domyślny [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) oferują uzupełnienia programu word i korzystaj każdego wprowadzonego znaku:

```xaml
<Entry Placeholder="Enter text here">
    <Entry.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Entry.Keyboard>
</Entry>
```

Jest równoważny kod C#:

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

#### <a name="customizing-the-return-key"></a>Dostosowywanie klawisz ENTER

Wygląd klawisz ENTER na klawiaturze nietrwałego, która jest wyświetlana, gdy [ `Entry` ](xref:Xamarin.Forms.Entry) ma fokus, może zostać dostosowane przez ustawienie [ `ReturnType` ](xref:Xamarin.Forms.Entry.ReturnType) właściwość z wartością [ `ReturnType` ](xref:Xamarin.Forms.ReturnType) wyliczenia:

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) — Wskazuje, że nie określonego klucza zwracany jest wymagane i czy używany będzie domyślny platformy.
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) — Wskazuje "Gotowego" klawisz ENTER.
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) — Wskazuje "Idź" klawisz ENTER.
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) — Wskazuje "Dalej" klawisz ENTER.
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) — Wskazuje "Wyszukaj" klawisz ENTER.
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) — Wskazuje "Send" klawisz ENTER.

W poniższym przykładzie XAML pokazuje, jak ustawić klawisz ENTER:

```xaml
<Entry ReturnType="Send" />
```

Jest równoważny kod C#:

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> Dokładny wygląd klawisz ENTER, zależy od platformy. W systemach iOS klawisz return jest przycisk oparte na tekście. W systemach Android i Universal Windows Platform, klawisz return jest oparte na ikonę przycisku.

Po naciśnięciu klawisza return [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) generowane zdarzenia i wszystkie `ICommand` określony przez [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand) właściwość jest wykonywany. Ponadto wszelkie `object` określony przez [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter) właściwości, które zostaną przekazane do `ICommand` jako parametr. Aby uzyskać więcej informacji na temat poleceń, zobacz [interfejs polecenia](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

### <a name="enabling-and-disabling-spell-checking"></a>Włączanie i wyłączanie sprawdzania pisowni

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Formantów właściwości tego, czy sprawdzanie pisowni jest włączone. Domyślnie ustawiono właściwość `true`. Wprowadzania tekstu, są oznaczone błędnie napisanych wyrazów.

Jednak w niektórych scenariuszach wpis tekstu, na przykład wprowadzając nazwę użytkownika, sprawdzanie pisowni zapewnia ujemny i powinny być wyłączone przez ustawienie [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) właściwości `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Gdy [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) właściwość jest ustawiona na `false`, a klawiatury niestandardowej nie jest używana, moduł sprawdzania pisowni natywny zostanie wyłączona. Jednak jeśli [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) ma został zestaw, który wyłącza pisowni kontroli, takie jak [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` właściwość jest ignorowana. W związku z tym, aby włączyć sprawdzanie pisowni, w którym nie można użyć właściwości `Keyboard` wyłączają jawnie.

### <a name="enabling-and-disabling-text-prediction"></a>Włączanie i wyłączanie funkcja podpowiadania tekstu

[ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) Formantów właściwości czy funkcja podpowiadania tekstu i automatyczna korekta tekstu jest włączona. Domyślnie ustawiono właściwość `true`. Wprowadzania tekstu, są prezentowane prognozy programu word.

Jednak w niektórych scenariuszach wprowadzania tekstu, takich jak wprowadzenie nazwy użytkownika, funkcja podpowiadania tekstu i Tekst automatyczny poprawianiem zapewnia ujemny i powinny być wyłączone przez ustawienie [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) właściwości `false`:

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> Gdy [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) właściwość jest ustawiona na `false`, a klawiatury niestandardowej nie jest używana funkcja podpowiadania tekstu i automatyczne korekty tekstu jest wyłączona. Jednak jeśli [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) ustawił tego funkcja podpowiadania tekstu wyłącza `IsTextPredictionEnabled` właściwość jest ignorowana. W związku z tym, aby umożliwić funkcja podpowiadania tekstu dla, w którym nie można użyć właściwości `Keyboard` wyłączają jawnie.

### <a name="placeholders"></a>Symbole zastępcze

`Entry` można ustawić tak, aby pokazywać tekst symbolu zastępczego, gdy nie przechowuje dane wejściowe użytkownika. W praktyce jest to często postrzegane w formularzach wyjaśnienie zawartość, która jest odpowiednia dla danego pola. Kolor tekstu symbolu zastępczego nie można dostosować i będą takie same, niezależnie od wartości `TextColor` ustawienie. Jeśli projekt odwołuje się do koloru niestandardowego symbolu zastępczego, musisz przełączyć się na [niestandardowego modułu renderowania](). Spowoduje to utworzenie następujących `Entry` "USERNAME" jako symbol zastępczy w XAML:

```xaml
<Entry Placeholder="Username" />
```

W języku C#:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "Przykład symbolu zastępczego wpisu")

### <a name="password-fields"></a>Pola hasła

`Entry` udostępnia `IsPassword` właściwości. Gdy `IsPassword` jest `true`, zawartość pola zostanie wyświetlone jako czarne kółka:

W XAML:

```xaml
<Entry IsPassword="true" />
```

W języku C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "Przykład IsPassword wpisu")

Symbole zastępcze mogą być używane z wystąpieniami `Entry` skonfigurowanych jako pola hasła:

W XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

W języku C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "Przykład symbolu zastępczego i IsPassword wpisu")

### <a name="colors"></a>Kolory

Wpis można ustawić tło niestandardowe i kolorów tekstu za pomocą następujących właściwości możliwej do wiązania:

- **TextColor** &ndash; Ustawia kolor tekstu.
- **BackgroundColor** &ndash; Ustawia kolor wyświetlany za tekstem.

Szczególną jest niezbędne do zapewnienia, że kolory będzie można używać na każdej platformie. Ponieważ każdej z platform ma różne wartości domyślne kolory tekstu i tła, często musisz ustawić zarówno, jeśli zostanie ustawiona.

Użyj poniższego kodu, aby ustawić kolor tekstu wpisu:

W XAML:

```xaml
<Entry TextColor="Green" />
```

W języku C#:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![](entry-images/textcolor.png "Przykład TextColor wpisu")

Należy zauważyć, że symbol zastępczy nie dotyczy określonego `TextColor`.

Aby ustawić kolor tła w XAML:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

W języku C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "Przykład BackgroundColor wpisu")

Uważaj upewnić się, że wybrane kolory tła i tekstu można używać na każdej platformie i nie może zasłaniać dowolny tekst symbolu zastępczego.

## <a name="events-and-interactivity"></a>Zdarzenia i interakcyjność

Wpis przedstawia dwa zdarzenia:

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) &ndash; wywoływane, gdy tekst zostanie zmieniony we wpisie. Zawiera tekst, przed zmianą i po niej.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed) &ndash; wywoływane, gdy użytkownik zakończyła danych wejściowych, naciskając klawisz ENTER na klawiaturze.

### <a name="completed"></a>Zakończone

`Completed` Zdarzeń służy do reagowania na ukończenie interakcji z wpisem. `Completed` jest wywoływane, gdy użytkownik zakończy dane wejściowe z polem, naciskając klawisz ENTER na klawiaturze. Program obsługi zdarzenia jest program obsługi zdarzeń generycznych, biorąc nadawcy i `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

Ukończone zdarzenie może być subskrybowana w XAML:

```xaml
<Entry Completed="Entry_Completed" />
```

i C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

Po [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) wszystkie zdarzenia generowane `ICommand` określony przez [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand) właściwość jest wykonywane, za pomocą `object` określony przez [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter) przekazywana do właściwości `ICommand`.

### <a name="textchanged"></a>Textchanged.

`TextChanged` Zdarzeń służy do reagowania na zmiany w treści pola.

`TextChanged` jest wywoływane zawsze, gdy `Text` z `Entry` zmiany. Program obsługi zdarzenia przyjmuje wystąpienie klasy `TextChangedEventArgs`. `TextChangedEventArgs` zapewnia dostęp do starej i nowej wartości z `Entry` `Text` za pośrednictwem `OldTextValue` i `NewTextValue` właściwości:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` Zdarzeń może być subskrybowana w XAML:

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

i C#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```


## <a name="related-links"></a>Linki pokrewne

- [Tekst (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Wpis interfejsu API](xref:Xamarin.Forms.Entry)
