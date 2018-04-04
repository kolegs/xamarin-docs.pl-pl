---
title: Przekaż do sklepu Mac App Store
description: Ten przewodnik przeprowadzi Cię przez przekazywania do sklepu Mac App Store aplikacji Xamarin.Mac dla publikacji.
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 4ddbbdc32ecb2de4f9dcb89162bc38803fa7159e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="upload-to-mac-app-store"></a>Przekaż do sklepu Mac App Store

_Ten przewodnik przeprowadzi Cię przez przekazywania do sklepu Mac App Store aplikacji Xamarin.Mac dla publikacji._

Aplikacje są przesyłane do zatwierdzenia Mac App Store za pomocą [iTunes Connect](http://itunesconnect.apple.com/).

1. Wybierz **macOS aplikacji** utworzyć: 

    [![](uploading-images/image65.png "iTunes Connect")](uploading-images/image65.png#lightbox)

2. Wprowadź nazwę aplikacji i inne szczegóły. Deweloper można wybrać tylko z istniejącego **identyfikator pakietu** który został utworzony wcześniej: 

    [![](uploading-images/image66.png "Wybranie Identyfikatora pakietu")](uploading-images/image66.png#lightbox)

3. Wybierz datę dostępność i ceny. Niezależnie od daty dostępności wybiera dewelopera aplikację tylko staną się dostępne do sprzedaży po jej zatwierdzeniu. Tę wartość można ustawić daleko w przyszłości, jeśli deweloper chce większą kontrolę nad Data faktyczną dostępność: 

    [![](uploading-images/image67.png "Ustawianie dostępną datę i cen")](uploading-images/image67.png#lightbox)

4. Wprowadź informacje o aplikacji, kategorii Sklepu z aplikacjami, które należy w tym: 

    [![](uploading-images/image68.png "Wprowadzanie informacji o aplikacji")](uploading-images/image68.png#lightbox) 

    Wybierz klasyfikacje, które mają zastosowanie: 

    [![](uploading-images/image69.png "Ustawianie klasyfikacji aplikacji")](uploading-images/image69.png#lightbox) 

    Opis, słowa kluczowe i skontaktuj się z adresów URL: 

    [![](uploading-images/image70.png "Edytowanie opisu słów kluczowych i skontaktuj się z adresów URL")](uploading-images/image70.png#lightbox) 

    Informacje kontaktowe i wskazówki dotyczące osób dokonujących przeglądu sklepu z aplikacjami: 

    [![](uploading-images/image71.png "Edytowanie informacji kontaktowych i porady osoby dokonujące przeglądu sklepu z aplikacjami")](uploading-images/image71.png#lightbox) 

    I na koniec zrzuty ekranu: 

    [![](uploading-images/image72.png "Dodawanie wymaganych zrzuty ekranu")](uploading-images/image72.png#lightbox) 

    Zrzuty ekranu powinna mieć format JPG, TIF lub PNG, 1280 x 800, 1440 x 900, 2880 x 1800 lub 2560 x 1600 rozmiar pikseli. Naciśnij klawisz **zapisać** aby zakończyć.

5. Informacje o aplikacji są pokazywane do przeglądu. Kliknij przycisk **Wyświetl szczegóły** zmiany stanu: 

    [![](uploading-images/image73.png "Wyświetlanie szczegółów aplikacji")](uploading-images/image73.png#lightbox)

6. W widoku szczegółów kliknij przycisk Gotowe do przekazania binarne, aby przesłać plik pakietu aplikacji: 

    [![](uploading-images/image74.png "Wybieranie gotowe do przekazania binarne")](uploading-images/image74.png#lightbox)

7. Odpowiedź na pytanie kryptografii: 

    [![](uploading-images/image75.png "Udzielenie odpowiedzi na pytanie kryptografii")](uploading-images/image75.png#lightbox)

8. Lokacji będzie powiadomienia, gdy będzie gotowy do akceptowania plik pakietu aplikacji: 

    [![](uploading-images/image76.png "Powiadomienie akceptacji")](uploading-images/image76.png#lightbox)

9. Uruchom moduł ładujący aplikacji i upewnij się, aby zalogować się Apple ID.
Wybierz **dostarczania aplikacji** aby kontynuować: 

    [![](uploading-images/image77.png "Interfejs moduł ładujący aplikacji")](uploading-images/image77.png#lightbox)

10. Wybierz z listy aplikacji w **gotowe do przekazania binarne** stanu i kliknij przycisk **dalej**: 

    [![](uploading-images/image78.png "Wybieranie aplikacji do ładowania")](uploading-images/image78.png#lightbox)

11. Przejrzyj metadanych aplikacji i kliknij przycisk **wybierz...**  można znaleźć pliku pakietu: 

    [![](uploading-images/image79.png "Przeglądanie metadanych aplikacji")](uploading-images/image79.png#lightbox)

12. Znajdź plik pakietu, który został utworzony w programie Visual Studio dla komputerów Mac za pomocą konfiguracji kompilacji sklepu z aplikacjami: 

    [![](uploading-images/image80.png "Wybieranie plików do przekazania")](uploading-images/image80.png#lightbox)

13. Naciśnij klawisz **wysyłania**: 

    [![](uploading-images/image81.png "Wysyłanie aplikacji")](uploading-images/image81.png#lightbox)

14. Pakiet zostanie zweryfikowany, a zgłoszone żadne błędy. Usuń te błędy i ponownie przekazać. Po pomyślnym zakończeniu przekazywania, aplikacja będzie automatycznie przesłanie do przeglądu przez zespół sklepu z aplikacjami: 

    [![](uploading-images/image82.png "Przykład przekazywania błędów")](uploading-images/image82.png#lightbox)

Jeśli aplikacja została zatwierdzona, będzie dostępny do pobrania ani zakupu ze sklepu Mac App Store.

## <a name="related-links"></a>Linki pokrewne

- [Instalacja](~//mac/get-started/installation.md)
- [Witaj, przykładowe Mac](~//mac/get-started/hello-mac.md)
- [Dystrybucję aplikacji ze sklepu Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Przewodnik narzędzi: Podpisywania aplikacji kodu](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Identyfikator deweloperów i strażnika](https://developer.apple.com/resources/developer-id/)
