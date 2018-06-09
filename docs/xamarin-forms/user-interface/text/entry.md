---
title: Wpis platformy Xamarin.Forms
description: W tym artykule opisano sposób użycia klasy platformy Xamarin.Forms wpisu do akceptowania jednego wiersza tekstu lub wprowadź hasło w aplikacji.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: b6188b986589a56229ad2e092d4100ff3f75dbe4
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245556"
---
# <a name="xamarinforms-entry"></a>Wpis platformy Xamarin.Forms

_Jednego wiersza tekstu lub hasła danych wejściowych_

Platformy Xamarin.Forms `Entry` służy do wprowadzania jednego wiersza tekstu. `Entry`, Takiej jak `Editor` wyświetlić, obsługuje wiele typów klawiatury. Ponadto `Entry` mogą być używane jako pole hasła.

## <a name="display-customization"></a>Dostosowywanie ekranu

### <a name="setting-and-reading-text"></a>Ustawienie i odczytywanie tekstu

`Entry`, Jak inne widoki prezentacji tekst przedstawia `Text` właściwości. Tej właściwości można ustawić i przeczytaj tekst przedstawiony przez `Entry`. W poniższym przykładzie pokazano ustawienie `Text` właściwości w języku XAML:

```xaml
<Entry Text="I am an Entry" />
```

W języku C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Aby odczytać tekstu, Uzyskaj dostęp do `Text` właściwości w języku C#:

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> Szerokość `Entry` można zdefiniować ustawiając jego `WidthRequest` właściwości. Nie są zależne od szerokość `Entry` został określony na podstawie wartości z jego `Text` właściwości.

### <a name="limiting-input-length"></a>Ograniczenie długości danych wejściowych

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Właściwości może służyć do ograniczenia długości danych wejściowych, które są dozwolone dla [ `Entry` ](xref:Xamarin.Forms.Entry). Ta właściwość powinna być równa dodatnią liczbą całkowitą:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) właściwości wartość 0 oznacza, że może być brak danych wejściowych i wartość `int.MaxValue`, która jest wartością domyślną dla [ `Entry` ](xref:Xamarin.Forms.Entry), wskazuje, że istnieje nie skuteczne limit liczby znaków, które mogą być wprowadzane.

### <a name="keyboards"></a>Klawiatury

Klawiatury, które są prezentowane, gdy użytkownicy korzystają z `Entry` można ustawić programowo przy użyciu `Keyboard` właściwości.

Dostępne są następujące opcje dla typu klawiatury:

- **Domyślne** &ndash; klawiatury domyślne
- **Rozmowa** &ndash; używany do badań & miejsca gdzie emoji są przydatne
- **Wiadomości e-mail** &ndash; używane podczas wprowadzania adresu e-mail
- **Liczbowa** &ndash; używany podczas wprowadzania liczb
- **Telefon** &ndash; używane podczas wprowadzania numerów telefonów
- **Adres URL** &ndash; używane do wprowadzania ścieżki do plików i adresy witryn sieci web

Brak [przykład każdego klawiatury](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) w naszej sekcji przepisami.

### <a name="enabling-and-disabling-spell-checking"></a>Włączanie i wyłączanie sprawdzania pisowni

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Formantów właściwość określa, czy sprawdzanie pisowni jest włączone. Domyślnie ma ustawioną właściwość `true`. Wprowadzania tekstu, są oznaczone pisowni.

Jednak niektóre scenariusze wprowadzania tekstu, takie jak wprowadzić nazwę użytkownika sprawdzanie pisowni zapewnia środowisko ujemna i dlatego powinny być wyłączone przez ustawienie [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) właściwości `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Gdy [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) właściwość jest ustawiona na `false`i niestandardowe klawiatury nie jest używana, moduł sprawdzania pisowni macierzystego zostanie wyłączone. Jednak jeśli [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) ma zostały zestawu, które wyłączają pisowni sprawdzania, takich jak [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` właściwość jest ignorowana. Właściwości nie można użyć do włączenia sprawdzania pisowni `Keyboard` który jawnie wyłącza.

### <a name="placeholders"></a>Symbole zastępcze

`Entry` można ustawić tak, aby wyświetlić tekst zastępczy, gdy dane wejściowe użytkownika nie jest przechowywana. W praktyce jest to często postrzegane w formularzach wyjaśnienie zawartość, która jest odpowiednia dla danego pola. Kolor tekstu symbolu zastępczego nie można dostosować i będą takie same, niezależnie od tego `TextColor` ustawienie. Jeśli projekt odwołuje się do kolorów niestandardowych symbolu zastępczego, musisz przełączyć się na [niestandardowego modułu renderowania](). Spowoduje utworzenie następujących `Entry` "USERNAME" jako symbol zastępczy w języku XAML:

```xaml
<Entry Placeholder="Username" />
```

W języku C#:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "Przykład symbolu zastępczego wpisu")

### <a name="password-fields"></a>Pola hasła

`Entry` udostępnia `IsPassword` właściwości. Gdy `IsPassword` jest `true`, zawartość pola zostanie wyświetlone jako czarny kółka:

W języku XAML:

```xaml
<Entry IsPassword="true" />
```

W języku C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "Przykład IsPassword wpisu")

Symbole zastępcze może być używana z wystąpień `Entry` skonfigurowanych jako pola hasła:

W języku XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

W języku C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "Wpis IsPassword i przykładowe symbolu zastępczego")


### <a name="colors"></a>Kolory

Można ustawić wpisu Użyj niestandardowe tło i kolory tekstu za pomocą następujących właściwości:

- **TextColor** &ndash; Ustawia kolor tekstu.
- **BackgroundColor** &ndash; Ustawia kolor wyświetlany za tekstem.

Szczególną jest niezbędne do zapewnienia, że kolorów będzie można używać na każdej z platform. Ponieważ różne ustawienia domyślne kolory tła i tekstu dotyczy wszystkich platform, należy często Jeśli zostanie ustawiona wartość.

Aby ustawić kolor tekstu pozycji, należy użyć poniższego kodu:

W języku XAML:

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

Aby ustawić kolor tła w języku XAML:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

W języku C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "Przykład BackgroundColor wpisu")

Należy zachować ostrożność upewnić się, że wybrane kolory tła i tekstu nadają się na każdej platformie i nie może zasłaniać tekst zastępczy.

## <a name="events-and-interactivity"></a>Zdarzenia i interakcji

Wpis udostępnia dwa zdarzenia:

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) &ndash; wywoływane, gdy tekst zostanie zmieniony we wpisie. Zawiera tekst przed i po zmianie.
- [Ukończono](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) &ndash; wywoływane, gdy użytkownik zakończył dane wejściowe, naciskając klawisz return na klawiaturze.

### <a name="completed"></a>Zakończone

`Completed` Zdarzeń służy do reagowania na zakończenia interakcji z wpisu. `Completed` jest wywoływane, gdy użytkownik zakończy dane wejściowe z pola, wprowadzając klawisz return na klawiaturze. Program obsługi dla zdarzenia jest obsługi zdarzeń rodzajowych, biorąc nadawcy i `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

Zakończonego zdarzenia może być subskrybowana w języku XAML:

```xaml
<Entry Completed="Entry_Completed" />
```

i C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

### <a name="textchanged"></a>TextChanged

`TextChanged` Zdarzeń służy do reagowania na zmiany w zawartości pola.

`TextChanged` jest wywoływane zawsze, gdy `Text` z `Entry` zmiany. Program obsługi dla zdarzenia przełączy wystąpienia `TextChangedEventArgs`. `TextChangedEventArgs` zapewnia dostęp do starej i nowej wartości `Entry` `Text` za pośrednictwem `OldTextValue` i `NewTextValue` właściwości:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` Zdarzeń może być subskrybowana w języku XAML:

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
- [Wpis interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)
