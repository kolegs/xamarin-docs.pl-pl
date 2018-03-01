---
title: Wprowadzanie tekstu
ms.topic: article
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: ce1014616d0cf5f6cd5228d69976dfeca546b382
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="text-input"></a>Wprowadzanie tekstu

Akceptowanie danych wejściowych użytkownika tekst jest realizowane za pomocą `UITextField` na jeden wiersz danych wejściowych i UITextView edycji tekstu wielowierszowego. Można przeciągnięcie dowolnej z tych kontrolek do ekranu i kliknij dwukrotnie, aby ustawić tekst początkowej.

Poniższe zrzuty ekranu przedstawiają ikony dla tych elementów, znajduje się w konsoli do przybornika programu Visual Studio dla komputerów Mac:

 [ ![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png)

 [ ![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png)

Po wyjściu ma nazwanych i zapisany plik scenorysu, Visual Studio for Mac spowoduje zaktualizowanie `.designer.cs` częściowej klasy można dodać kodu C#, który odwołuje się do pliku klasy formantu. Każdy formant ma swoją własną unikatową właściwości i zdarzenia, które są dostępne w kodzie C#.

 <a name="UITextField" />


## <a name="uitextfield"></a>UITextField

`UITextField` Kontroli jest najczęściej używana do akceptowania pojedynczy wiersz wprowadzania tekstu, takie jak nazwa użytkownika lub hasło. Niektóre z opcji dostępnych dla Dostosowywanie formantu są wyświetlane tutaj:

 [ ![](text-input-images/image15a.png "Właściwości UITextField")](text-input-images/image15a.png)

Poniżej opisano tych kontrolek:

-  **Symbol zastępczy** — ten krok jest opcjonalny. Jeśli ustawiona, jest wyświetlany, gdy pole tekstowe jest pusta, zwykle można wyjaśnić użytkownikowi oczekuje się, jakie dane wejściowe.
-  **Przycisk Wyczyść** — Określa, po wyświetleniu standardowe przycisk Wyczyść (szary okręgu (x)) w polu tekstowym sposób dla użytkownika szybko zwykły tekst. Może być trwale ukryty, stale widoczna lub pokazano, w zależności od tego, czy pole jest edytowany.
-  **Minimalny rozmiar czcionki** i **Dopasuj do rozmiaru** — zezwala na rozmiar czcionki, aby automatycznie korygowane dłużej tekstu i zapobiec obcięcie, ale ograniczone do nie mniejszy niż podany rozmiar.
-  **Wielkość liter** — czy wielką słów, zdań lub wszystkie dane wejściowe.
-  **Korekcja** — czy włączone jest sprawdzanie pisowni i sugestie.
-  **Klawiatura** — formanty styl klawiatury wyświetlana dla danych wejściowych, i w związku z tym jakie klucze są dostępne na klawiaturze. Dotyczy to również na klawiaturze numerycznej, dopełniania telefonicznej, wiadomości E-mail, adres URL wraz z innymi opcjami.
-  **Wygląd** — Określa styl wyglądu klawiatury i będzie albo ciemny lub jasny motywów.
-  **Zwraca klucz** — zmienić etykiety na klawisz Return w celu lepszego odzwierciedlenia, jakie działania zostaną podjęte. Obsługiwane wartości to Przejdź, sprzężenia, dalej, trasy, gotowe i wyszukiwania.
-  **Zabezpieczanie** — wskazuje, czy dane wejściowe są wyświetlane jako znaki (takie jak w przypadku wprowadzania hasła).


Jeśli UITextField o nazwie `textfield1` został dodany do ekranu przy użyciu projektanta, możesz ustawić lub zmodyfikować jego właściwości w języku C# w następujący sposób:

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

Xamarin.iOS zapewnia wyliczenia, stosownie do ułatwiają wybierz odpowiednie ustawienia, takie jak `UIKeyboardType` i `UIReturnKeyType` w powyżej fragmentu kodu.

### <a name="display-text-programmatically"></a>Wyświetl tekst programowo

Jeśli nie chcesz projektowanie ekranu przy użyciu projektanta, lub jeśli chcesz dynamicznie dodać fragment tekstu w czasie wykonywania, możesz utworzyć i wyświetlić UITextField programowo w `ViewDidLoad` metody kontrolera widoku następująco:

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />


## <a name="uitextview"></a>UITextView

`UITextView` Formantu można używać do wyświetlania tekstu tylko do odczytu lub zaakceptować wprowadzanie tekstu w wielu wierszach. Wiele z tych samych opcji jako ma `UITextField` (takich jak wielkość liter, korekty, itd.).

 [ ![](text-input-images/image16a.png "Właściwości UITextView")](text-input-images/image16a.png)

Właściwości specyficzne dla obejmują:

-  **Zachowanie** — czy tekst jest edytowalny, czy tylko do odczytu.
-  **Wykrywanie** — Detects i konwertuje podano: dane do klikania elementów takich jak numery telefonów, które mogą wyzwalać wywołanie, adresów, które stają się łączy do społeczności Maps, adresy URL, które otwarte w programie Safari lub daty i godziny, które stają się zdarzenia w kalendarzu.


Jeśli UITextView został dodany do ekranu przy użyciu projektanta, możesz ustawić lub zmodyfikować jego właściwości w następujący sposób:

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```



## <a name="related-links"></a>Linki pokrewne

- [Formanty (przykład)](https://developer.xamarin.com/samples/Controls/)
