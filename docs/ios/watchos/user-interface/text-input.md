---
title: Praca z wprowadzanie tekstu
ms.topic: article
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 170131a2449b37acfa411eeca54f7aa921b0d9e4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-text-input"></a>Praca z wprowadzanie tekstu

Apple Watch nie zapewnia klawiatury dla użytkownikom wprowadzanie tekstu, jednak obsługuje niektóre alternatyw czujki przyjaznego:

- Wybieranie z uprzednio zdefiniowanej listy opcji tekstu
- Dyktowania Siri
- Wybieranie emoji
- Bazgroły rozpoznawania pisma litery według liter (zostanie wprowadzony w watchOS 3).

Symulator nie obsługuje obecnie dyktowania, ale możesz nadal przetestować wprowadzanie tekstu kontrolera inne opcje, takie jak bazgrołów, jak pokazano poniżej:

![](text-input-images/textinput-sml.png "Testowanie opcji bazgrołów")

Aby zaakceptować wprowadzanie tekstu w aplikacji watch:

1. Utwórz wstępnie zdefiniowanych opcji tablicy ciągów.
2. Wywołanie `PresentTextInputController` z tablicy, czy zezwalać emoji lub nie i `Action` jest to po zakończeniu operacji użytkownika.
3. Tego działania zakończenia testu dla wyniku wprowadzania i podejmij odpowiednie działania w aplikacji (prawdopodobnie ustawienie wartości tekstu etykiety).

Poniższy fragment kodu przedstawia trzy wstępnie zdefiniowane opcje dla użytkownika:

```csharp
var suggest = new string[] {"Get groceries", "Buy gas", "Post letter"};

PresentTextInputController (suggest, WatchKit.WKTextInputMode.AllowEmoji, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
    // this only works if result is a text response (Plain or AllowEmoji)
        enteredText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (enteredText);
        // do something, such as myLabel.SetText(enteredText);
    }
});
```

`WKTextInputMode` Wyliczenie ma trzy wartości:

- Zwykły
- AllowEmoji
- AllowAnimatedEmoji

## <a name="plain"></a>Zwykły

Jeśli ustawiono tryb zwykły, użytkownik może wybrać:

- Dyktowania,
- Bazgroły, lub
- z listy wstępnie zdefiniowanych dostarczającego aplikacji.

[![](text-input-images/plain-scribble-sml.png "Dyktowania, bazgrołów, lub z uprzednio zdefiniowanej listy, który dostarcza aplikacji")](text-input-images/plain-scribble.png#lightbox)

Wynik jest zawsze zwrócony jako `NSObject` mogą być rzutowane na `string`.

## <a name="emoji"></a>Emoji

Istnieją dwa typy emoji:

- Regularne emoji Unicode
- Animowany obrazów

Gdy użytkownik wybierze Unicode emoji, jest zwracana jako ciąg.

Jeśli wybrano emoji animowany obraz `result` w celu wykonania procedury obsługi będzie zawierać `NSData` obiekt, który zawiera emoji `UIImage`.

## <a name="accepting-dictation-only"></a>Akceptowanie dyktowania tylko

Do użytkownika bezpośrednio na ekranie dyktowania bez żadnych sugestie (lub opcją bazgrołów):

- Przekaż się pusta tablica listy sugestie i
- Ustaw `WatchKit.WKTextInputMode.Plain`.

```csharp
PresentTextInputController (new string[0], WatchKit.WKTextInputMode.Plain, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
        dictatedText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (dictatedText);
        // do something, such as myLabel.SetText(dictatedText);
    }
});
```

Mówiąc użytkownika na ekranie czujki zostanie wyświetlony następujący ekran, zawierającej tekst jest rozpoznawany (na przykład "To jest test"):

![](text-input-images/dictation.png "Mówiąc użytkownika ekranu czujki Wyświetla tekst jako przyjmuje się")

Po naciśnięciu **gotowe** zostanie zwrócony tekst przycisku.



## <a name="related-links"></a>Linki pokrewne

- [Firmy Apple doc tekstu i etykiety](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [Wprowadzenie do systemu watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
