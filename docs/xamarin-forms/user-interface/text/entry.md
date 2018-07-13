---
title: Wpis zestawu narzędzi Xamarin.Forms
description: W tym artykule opisano sposób użycia klasy wpisu zestawu narzędzi Xamarin.Forms do akceptowania jednego wiersza tekstu lub wprowadź hasło w aplikacji.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 95afdfde878759d4a598e200d16fe6fb1fa2005e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998249"
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

### <a name="keyboards"></a>Klawiatury

Klawiatura, które są prezentowane podczas interakcji z `Entry` można ustawić programowo za pośrednictwem `Keyboard` właściwości.

Opcje typu klawiatury są następujące:

- **Domyślne** &ndash; klawiatury domyślne
- **Porozmawiaj** &ndash; używane dla badań & miejsca gdzie przydają się emoji
- **Adres e-mail** &ndash; używane podczas wprowadzania adresu e-mail
- **Liczbowe** &ndash; używane podczas wprowadzania liczb
- **Telefon** &ndash; używane podczas wprowadzania numerów telefonów
- **Adres URL** &ndash; używane do wprowadzania ścieżki do plików i adresy witryn sieci web

Brak [przykładzie każdy klawiatury](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) w naszej sekcji przepisy.

### <a name="enabling-and-disabling-spell-checking"></a>Włączanie i wyłączanie sprawdzania pisowni

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Formantów właściwości tego, czy sprawdzanie pisowni jest włączone. Domyślnie ustawiono właściwość `true`. Wprowadzania tekstu, są oznaczone błędnie napisanych wyrazów.

Jednak w niektórych scenariuszach wpis tekstu, na przykład wprowadzając nazwę użytkownika, sprawdzanie pisowni zapewnia środowisko ujemne, a tym samym powinny być wyłączone przez ustawienie [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) właściwości `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Gdy [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) właściwość jest ustawiona na `false`, a klawiatury niestandardowej nie jest używana, moduł sprawdzania pisowni natywny zostanie wyłączona. Jednak jeśli [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) ma został zestaw, który wyłącza pisowni kontroli, takie jak [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` właściwość jest ignorowana. W związku z tym, aby włączyć sprawdzanie pisowni, w którym nie można użyć właściwości `Keyboard` wyłączają jawnie.

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

- [TextChanged](xref:Xamarin.Forms.Entry.TextChanged) &ndash; wywoływane, gdy tekst zostanie zmieniony we wpisie. Zawiera tekst, przed zmianą i po niej.
- [Ukończono](xref:Xamarin.Forms.Entry.Completed) &ndash; wywoływane, gdy użytkownik zakończyła danych wejściowych, naciskając klawisz ENTER na klawiaturze.

### <a name="completed"></a>Zakończone

`Completed` Zdarzeń służy do reagowania na ukończenie interakcji z wpisem. `Completed` jest wywoływane, gdy użytkownik zakończy dane wejściowe z polem, wprowadzając klawisz ENTER na klawiaturze. Program obsługi zdarzenia jest program obsługi zdarzeń generycznych, biorąc nadawcy i `EventArgs`:

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
