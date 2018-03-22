---
title: "Możliwości płatności firmy Apple"
description: "Dodawanie funkcji do aplikacji często wymaga dodatkowej konfiguracji inicjowania obsługi administracyjnej. W tym przewodniku opisano ustawienia wymagane dla Apple Pay możliwości."
ms.topic: article
ms.prod: xamarin
ms.assetid: 735CC916-16A4-471B-87F7-0535E24288D7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 655e9fc81d7079c355998f0da7b41ea7cc778c3f
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="apple-pay-capabilities"></a>Możliwości płatności firmy Apple

_Dodawanie funkcji do aplikacji często wymaga dodatkowej konfiguracji inicjowania obsługi administracyjnej. W tym przewodniku opisano ustawienia wymagane dla Apple Pay możliwości._

Apple Pay umożliwia użytkownikom opłacać towarów fizycznych za pośrednictwem urządzeń z systemem iOS. Ta sekcja zawiera opis sposobu tworzenia wszystkie niezbędne składniki wymagane do firmy Apple płatności w Centrum deweloperów firmy Apple.

Podczas inicjowania obsługi administracyjnej nowej aplikacji za pośrednictwem Centrum deweloperów, istnieją trzy kroki, które należy podjąć:

1.  Utwórz handlowe identyfikatora.
2.  Utwórz identyfikator aplikacji dzięki możliwości zastosowania płatności i Dodaj sprzedawcy do niej.
3.  Wygeneruj certyfikat dla identyfikatora handlowych.

Poniższe kroki przeprowadzi Cię przez proces tworzenia powyższych elementów:

<a name="merchantid" />

## <a name="create-merchant-id"></a>Utwórz identyfikator handlowe

Identyfikator sprzedawcy umożliwia Apple Pay może akceptować płatności, a są przekazywane do firmy PassKit `PaymentRequest` — metoda i używane w Apple Pay uprawnienia:

1.  Przejdź do [Centrum deweloperów firmy Apple](https://developer.apple.com/account/) i przejdź do sekcji certyfikatów, identyfikator i profili: 
 
    ![Wybór identyfikator sprzedawcy Centrum deweloperów](apple-pay-capabilities-images/image57.png)

2.  W obszarze **identyfikatory**, wybierz pozycję **identyfikatorów sprzedawcy**, a następnie wybierz  **+**  do utworzenia nowego sprzedawcy Identyfikatora:  

3.  Wypełnij formularz, przedstawione poniżej, nowy opis i identyfikator. Opis sprawia, że identyfikator do zidentyfikowania użytkownika i będzie można zmienić później. Identyfikator musi być unikatowa dla Ciebie i musi rozpoczynać się od ciągu `merchant`. Apple zaleca się, że identyfikator być w następującym formacie: `merchant.com.[Your-App-Name]`:
   
    ![Nowy identyfikator sprzedawcy szczegóły](apple-pay-capabilities-images/image58.png)

4.  Potwierdź szczegóły, a **zarejestrować** Identyfikatora: 
    
    ![Handlowe identyfikator potwierdzenia](apple-pay-capabilities-images/image59.png)

<a name="appid" />

## <a name="create-an-app-id-with-the-apple-pay-capability-that-includes-the-merchant-id"></a>Utwórz identyfikator aplikacji z możliwością Apple Pay, która zawiera identyfikator sprzedawcy

1.  W [Centrum deweloperów](https://developer.apple.com/account/) kliknij **identyfikatorów aplikacji** w obszarze **identyfikatory**: 
    
    ![Wybierz identyfikator aplikacji w Centrum deweloperów](apple-pay-capabilities-images/image6.png)

2.  Wybierz  **+**  przycisk, aby dodać nowy identyfikator aplikacji: 
   
    ![Dodaj nowy przycisk Identyfikator aplikacji](apple-pay-capabilities-images/image27.png)

3.  Wprowadź nazwę dla Identyfikatora aplikacji i nadaj mu jawny identyfikator aplikacji:    
   
    ![Ekran szczegółów identyfikator aplikacji ](apple-pay-capabilities-images/image35.png)

4.  W obszarze usług aplikacji wybierz Apple Pay:    
  
    ![Płatności Apple usług aplikacji](apple-pay-capabilities-images/image36.png)

5.  Wybierz **kontynuować** , a następnie **zarejestrować**. Należy pamiętać, że na ekranie potwierdzenia Apple Pay wyświetli przy konfigurowalne zaznaczone, przy użyciu symbolu żółty: 
   
    ![Ekran potwierdzenia dla płatności firmy Apple](apple-pay-capabilities-images/image37.png)

6.  Wróć do listy identyfikatorów aplikacji i wybierz ten, który właśnie utworzony:  
   
    ![Edytuj identyfikator aplikacji](apple-pay-capabilities-images/image38.png)

7.  Przewiń w dół w tej sekcji rozwinięte, a następnie kliknij przycisk **Edytuj**.
8.  Przewiń w dół na liście, aby zwrócić firmy Apple, a następnie kliknij przycisk **Edytuj** przycisk:  
    
    ![Edytuj szczegóły firmy Apple należy zwrócić identyfikator aplikacji](apple-pay-capabilities-images/image39.png)

9.  Wybierz identyfikator sprzedawcy z tym Identyfikatorem aplikacji, a następnie kliknij przycisk **Kontynuuj**:  
    
    ![Wybierz identyfikator sprzedawcy do użycia dla Identyfikatora aplikacji](apple-pay-capabilities-images/image40.png)

10. Potwierdź identyfikator sprzedawcy przypisania, a następnie naciśnij klawisz **przypisać**:  
    
    ![Ekran potwierdzenia](apple-pay-capabilities-images/image41.png)

Ten identyfikator aplikacji można teraz używać wygenerować lub ponownie wygenerować nowy profil inicjowania obsługi administracyjnej, zgodnie z opisem w [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnik. 

<a name="certificate" />

## <a name="create-a-certificate-for-your-merchant-id"></a>Utwórz certyfikat dla Identyfikatora handlowe

Certyfikat jest wymagany przez firmę Apple do szyfrowania poufnych danych skojarzony z transakcją. Każdy identyfikator handlowych, utworzony musi mieć własny certyfikat. 

Aby utworzyć certyfikat, wykonaj następujące czynności:

1.  Wybierz identyfikator handlowych, utworzony wcześniej, a następnie naciśnij klawisz **Edytuj**: 
    
    ![Identyfikator sprzedawcy okno dialogowe Edycja](apple-pay-capabilities-images/image42.png)

2.  Na ekranie Ustawienia Identyfikatora sprzedawcy z systemem iOS kliknij **Tworzenie certyfikatu**: 
   
    ![Tworzenie certyfikatu przetwarzania płatności](apple-pay-capabilities-images/image43.png)

3.  Odpowiedzi na następujące pytania: 

    ![adres, jeśli płatności będą przetwarzane wyłącznie w Chinach](apple-pay-capabilities-images/image44.png)

4.  W tym momencie pojawi się monit, aby utworzyć _żądania podpisania certyfikatu_: 

    ![Tworzenie żądania podpisania certyfikatu](apple-pay-capabilities-images/image45.png)
    
    > [!IMPORTANT]
    > Jeśli używasz dostawcy płatności Apple Pay, takie JudoPay lub Stripe, ich może udostępnić CSR prawidłowo sformatowane, którego można użyć w tym momencie. Informacje dotyczące żądania to znajduje się na [JudoPay](https://www.judopay.com/docs/version-52/apple-pay/getting-started/#create-an-apple-pay-certificate) i [Stripe](https://stripe.com/docs/apple-pay/apps#csr) witryny. Aby utworzyć własną CSR, wykonaj kroki 5 – 8 poniżej. Po utworzeniu CSR, przejdź do kroku 9.

5.  Otwórz aplikację dostęp łańcucha kluczy, a następnie przejdź do **dostęp łańcucha kluczy > Asystent certyfikatu > zażądać certyfikatu od urzędu certyfikacji:** 

     ![Utwórz znajdujący się na komputerze Mac przy użyciu łańcucha kluczy](apple-pay-capabilities-images/image46.png)

6.  Wprowadź adres e-mail, wprowadź nazwę klucza prywatnego, pozostaw pusty, wybierz adres E-mail urzędu certyfikacji **Zapisz na dysku** i wybrać **Pozwól mi Określ pary kluczy informacje**:

     ![Okno dialogowe informacje certyfikatu](apple-pay-capabilities-images/image47.png)

7.  Zapisz obsługi w dogodnej lokalizacji: 

     ![Zapisywanie CSR do komputera lokalnego](apple-pay-capabilities-images/image48.png)

8.  Na ekranie informacje pary klucza ustaw **rozmiar klucza** do **256 bitów** i **algorytm** do **ECC** i kliknij przycisk **Kontynuuj**:

     ![Wprowadź pary kluczy informacje w oknie dialogowym](apple-pay-capabilities-images/image49.png)

9.  W Centrum deweloperów, kliknij polecenie **Kontynuuj** do przekazania obsługi: 

     ![Przygotowywanie do przekazania renderowanie po stronie klienta do Centrum deweloperów](apple-pay-capabilities-images/image50.png)

10. Kliknij pozycję **Choose File… (Wybierz plik...)** Aby wybrać CSR i naciśnij klawisz **Kontynuuj** można przekazać go do portalu dla deweloperów: 

     ![Przekaż renderowanie po stronie klienta do Centrum deweloperów](apple-pay-capabilities-images/image51.png)

11. Wygenerowany certyfikat go pobrać, a następnie kliknij dwukrotnie, aby zainstalować go do Twojego łańcucha kluczy.

Aby uzyskać więcej informacji na temat używania Apple Pay można skorzystać z następującymi wskazówkami:

*   [Wprowadzenie do płatności firmy Apple](~/ios/platform/apple-pay.md)

## <a name="next-steps"></a>Następne kroki
 
Poniższa lista zawiera dodatkowe kroki, które muszą być podjęte:

* Użyj nazw framework w aplikacji.
* Dodawanie uprawnień wymaganych do aplikacji. Informacje dotyczące uprawnień wymaganych i sposobu dodawania ich została szczegółowo opisana w [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.
* W aplikacji **iOS podpisywania pakietu**, upewnij się, że **uprawnień niestandardowych** ustawiono **Entitlements.plist**. Jest to _nie_ tworzy domyślne ustawienie dla debugowania i symulatora systemu iOS.

Jeśli wystąpią problemy związane z usługi aplikacji, zapoznaj się [Rozwiązywanie problemów](~/ios/deploy-test/provisioning/capabilities/index.md) głównym przewodniku.