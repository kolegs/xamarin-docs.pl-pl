---
title: Wpis
description: Jednego wiersza tekstu lub hasła danych wejściowych
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 2e40effa7bc54b7b7cf73edaa882256fed521a95
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="entry"></a>Wpis

_Jednego wiersza tekstu lub hasła danych wejściowych_

Platformy Xamarin.Forms `Entry` służy do wprowadzania jednego wiersza tekstu. `Entry`, takich jak widok edytora obsługuje wiele typów klawiatury. Ponadto `Entry` mogą być używane jako pole hasła.

## <a name="display-customization"></a>Dostosowywanie ekranu

### <a name="setting-and-reading-text"></a>Ustawienie i odczytywanie tekstu

Przedstawia zapis, jak inne widoki prezentacji tekstu `Text` właściwości. `Text` można ustawić i przeczytaj tekst przedstawiony przez `Entry`. W poniższym przykładzie pokazano, ustawienie tekstu w języku XAML:

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
