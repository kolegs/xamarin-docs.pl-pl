---
title: Touch ID w Xamarin.iOS
description: Ten dokument zawiera opis sposobu użycia funkcji Touch ID technologii uwierzytelniania biometrycznego odcisku palca firmy Apple, w aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: b5db80d280d7ad3c43a438d5caae57fbd1928896
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788592"
---
# <a name="touch-id-in-xamarinios"></a>Touch ID w Xamarin.iOS

Touch ID została wprowadzona w systemie iOS 7 jako metody uwierzytelniania użytkownika — podobnie jak kod dostępu. Jednak została ograniczona do odblokowywania urządzenia, przy użyciu sklepu z aplikacjami, za pomocą programu iTunes i iCloud łańcucha kluczy tylko uwierzytelniania.

Istnieją teraz dwa sposoby użycia funkcji Touch ID jako mechanizmu uwierzytelniania w aplikacji systemu iOS 8 przy użyciu lokalnego interfejsu API uwierzytelniania. Obecnie nie jest możliwe zdalne uwierzytelnianie za pomocą uwierzytelniania lokalnego.

Aby w pełni zrozumieć funkcji Touch ID i jego wartość, możemy Eksploruj łańcucha kluczy usług i danych użytkownika znaczenie tych nowych zmian. Dostęp do pęku kluczy również zostały rozszerzone na w systemie iOS 8 przy użyciu nowej funkcji listy kontroli dostępu (ACL).

## <a name="keychain--secure-enclave"></a>Łańcucha kluczy & bezpiecznego enklawę

Łańcucha kluczy jest dużej bazy danych, zapewniając bezpiecznego magazynu dla hasła, klucze, certyfikaty i informacje o jeden poszczególnych identyfikatora firmy Apple. W systemie iOS 8 aplikacji jest zawsze ma dostęp do elementów własny unikatowy łańcucha kluczy, a nie może uzyskać dostępu do elementów łańcucha kluczy innych aplikacji. To różni się od OS X łańcucha kluczy w przypadku odblokować za pomocą jednego hasła, dzięki czemu dowolnej aplikacji obsługującej łańcucha kluczy usługi, użyj łańcuchu kluczy. W tym artykule firma Microsoft będzie skupić się na działanie łańcucha kluczy w systemie iOS 8.

Łańcucha kluczy jest specjalne bazy danych, w której każdy wiersz jest nazywany _elementu łańcucha kluczy_. Każdy element jest opisane przez atrybuty łańcucha kluczy i składa się z zaszyfrowanych wartości. Aby zezwolić na efektywne wykorzystanie łańcucha kluczy, jest zoptymalizowana pod kątem małych elementów, lub _kluczy tajnych_.
Każdy element łańcucha kluczy jest chroniony przez kod dostępu użytkowników i urządzeń Unikatowy klucz tajny. Elementy łańcucha kluczy powinny być chronione, nawet wtedy, gdy użytkownicy nie używa swoich urządzeń. Ten sposób jest implementowany w systemie iOS zezwalając tylko elementy staną się dostępne, gdy urządzenie zostanie odblokowane, gdy urządzenie jest zablokowane stają się niedostępne. Może być również przechowywane w zaszyfrowanej kopii zapasowej. Jednym z kluczowych funkcji usługi łańcucha kluczy jest wymusić kontrolę dostępu; aplikacja ma dostęp do jego część łańcucha kluczy, a nie będzie można uruchamiać wszystkie inne aplikacje. Na poniższym diagramie przedstawiono sposób komunikowania się z łańcucha kluczy:

[![](touchid-images/image1.png "Ten diagram przedstawia sposób komunikowania się z łańcucha kluczy")](touchid-images/image1.png#lightbox)

### <a name="secure-enclave"></a>Bezpieczne enklawę

Łańcucha kluczy nie może odszyfrować elementu łańcucha kluczy siebie. Zamiast tego jest wykonywane *Secure enklawę*. Bezpieczne enklawę jest wspólnej procesora w ramach mikroukładu A7, która jest odpowiedzialna za określanie pomyślnego dopasowania z odcisków palców z czujnika funkcji Touch ID przed zarejestrowanych drukowania. Zostanie następnie odszyfrować elementu łańcucha kluczy i przywrócenie odszyfrowany klucz tajny łańcuchu kluczy.

### <a name="working-with-keychain"></a>Praca z łańcucha kluczy

Najpierw aplikacji powinien zapytania do łańcucha kluczy, aby zobaczyć, czy istnieje hasła. Jeśli nie istnieje, może być konieczne monitować o hasło, więc użytkownik nie jest stale poproszony. Jeśli hasło musi zostać zaktualizowany, monit o podanie nowego hasła i przekaż zaktualizowanej wartości do łańcucha kluczy.

> [!NOTE]
> Po pobraniu przy użyciu klucza tajnego z łańcucha kluczy, wszystkie odwołania do danych powinien wyczyszczone z pamięci. Nigdy nie przypisany do zmiennej globalnej.

## <a name="keychain-acl-and-touch-id"></a>Listy kontroli dostępu łańcucha kluczy i Touch ID

Listy kontroli dostępu jest nowy atrybut elementu łańcucha kluczy w systemie iOS 8, który opisuje informacje dotyczące co musi stanie, aby zezwolić na określonej operacji występuje. Może to być w formie wyświetlanie okna dialogowego alertu i żądania kodu dostępu. Listy kontroli dostępu umożliwia ustawienie ułatwień dostępu i uwierzytelniania dla łańcucha elementów. Na poniższym diagramie przedstawiono, jak to nowy atrybut wiąże się przy użyciu reszty elementu łańcucha kluczy:

[![](touchid-images/image2.png "Ten diagram przedstawia, jak to nowy atrybut wiąże się przy użyciu reszty elementu łańcucha kluczy")](touchid-images/image2.png#lightbox)

Począwszy od systemu iOS 8, jest teraz nowe zasady obecności użytkownika `SecAccessControl`, który jest wymuszana przez bezpieczny enklawę na telefonie iPhone 5s lub nowszym. Firma Microsoft jest widoczna w poniższej tabeli, podobnie jak konfiguracja urządzenia mogą mieć wpływ oceny zasad:

|Konfiguracja urządzenia|Ocena zasad|Mechanizm tworzenia kopii zapasowej|
|--- |--- |--- |
|Urządzenia bez kodu dostępu|Brak dostępu|Brak|
|Urządzenia z kodu dostępu|Wymaga kodu dostępu|Brak|
|Urządzenia z Touch ID|Preferuje Touch ID|Umożliwia kodu dostępu|

Wszystkie operacje wewnątrz enklawę Secure można relacją wzajemnego zaufania. Oznacza to, że firma Microsoft można używać funkcji Touch ID Wynik uwierzytelniania do autoryzowania odszyfrowywania elementu łańcucha kluczy. Secure enklawę zachowanie licznik nieudanych dopasowania funkcji Touch ID, w których przypadku użytkownik będzie musiał przywrócić za pomocą kodu dostępu.
Nowa struktura w systemie iOS 8, nazywany _uwierzytelniania lokalnych_, obsługuje ten proces uwierzytelniania w urządzeniu. Przeanalizujemy to w następnej sekcji.

## <a name="local-authentication"></a>Uwierzytelnianie lokalnych

Jak możemy ustanowić w poprzedniej sekcji, aplikacje mogą używać uwierzytelniania lokalnych do uwierzytelnienia użytkownika zgodnie z zasadami zabezpieczeń, który nie został skonfigurowany na urządzeniu.

Obecnie interfejs API udostępnia tylko dwie możliwości: po pierwsze, ułatwia on istniejące usługi łańcucha kluczy za pomocą nowego łańcucha kluczy listy kontroli dostępu (ACL). Dane łańcucha kluczy można odblokować za pomocą pomyślne uwierzytelnienie użytkowników linii papilarnych.

Po drugie LocalAuthentication udostępnia dwie metody uwierzytelniania aplikacji lokalnie. Programiści powinni używać `CanEvaluatePolicy` ustalenie, czy urządzenie jest w stanie akceptowania funkcji Touch ID, a następnie `EvaluatePolicy` rozpocząć operację uwierzytelniania.

Gdy obie możliwości zapewniają uwierzytelnianie lokalne, nie udostępniają mechanizm dla aplikacji lub użytkownik, do uwierzytelniania na serwerze zdalnym.
Uwierzytelniania lokalnego zawiera nowy interfejs użytkownika standardowego w celu uwierzytelnienia. W przypadku funkcji Touch ID jest widok alertów z dwóch przycisków, jak pokazano poniżej. Jeden przycisk Anuluj i używać rezerwowej metodę uwierzytelniania — kod dostępu. Istnieje również niestandardowy komunikat, który musi być ustawiona. Dobrym rozwiązaniem na potrzeby wyjaśnić użytkownikowi, dlaczego wymagane jest uwierzytelnienie funkcji Touch ID jest.

[![](touchid-images/image12.png "Alert uwierzytelniania funkcji Touch ID")](touchid-images/image12.png#lightbox)

### <a name="with-keychain-services"></a>Z usługami łańcucha kluczy

Analizujemy nieco wcześniej jak element łańcucha kluczy jest odszyfrować, sprawdź naszego kodu dostępu przy użyciu bezpiecznego enklawę. W systemie iOS 8 firma Microsoft umożliwia uwierzytelnianie lokalnych żądania weryfikacji funkcji Touch ID w połączeniu z list kontroli dostępu do funkcji, która udostępnia implementację mechanizm rezerwowy lub hasło.
Aby użyć listy ACL możemy powinien być używany `SecAccessControl` zasad, a następnie zaznaczając stan urządzeniami przy użyciu `SecAccessible.WhenPasscodeSetThisDeviceOnly` lub `SecAccessible.WhenUnlocked`.

#### <a name="considerations-with-acl"></a>Zagadnienia dotyczące z list kontroli dostępu

Istnieje wiele sposobów możemy należy mieć na uwadze przy użyciu list kontroli dostępu łańcucha kluczy, a niektóre z nich wymieniono poniżej:

-   Używać tylko z aplikacji na pierwszym planie — można wywołać żadnej operacji łańcucha kluczy w wątku w tle, które wywołanie zakończy się niepowodzeniem.
-   Dodawanie i aktualizowanie elementów łańcucha kluczy mogą wymagać uwierzytelniania.
-   Jeśli żądanie zwróci wiele dopasowań w łańcuchu kluczy, może być wymagane uwierzytelnianie.
-   Listy ACL chronione elementy są tylko do urządzeń i dlatego nie zsynchronizowane lub kopii zapasowej.

### <a name="using-local-authentication-without-keychain-services"></a>Przy użyciu uwierzytelniania lokalnego bez usług łańcucha kluczy

Uwierzytelnianie lokalnych został utworzony jako sposób do zbierania poświadczeń, takich jak dostępu lub funkcji Touch ID i do pracy z enklawę zabezpieczenia, aby zakończyć uwierzytelnianie użytkownika. Go traktować jako mostka pomiędzy aplikacją i enklawę bezpieczny, który może nigdy nie bezpośrednio komunikować się ze sobą. Może również służyć do oceny zasad aplikacji.

W tym celu aplikację wywołuje oceny zasad wewnątrz uwierzytelnianie lokalne, które uruchamia wykonywanie operacji wewnątrz enklawę Secure. Można wykorzystać do zapewnienia uwierzytelniania do aplikacji, bez bezpośrednio zapytań/uzyskiwania dostępu do bezpiecznego enklawę.

[![](touchid-images/image13a.png "Przy użyciu uwierzytelniania lokalnego bez usług łańcucha kluczy")](touchid-images/image13a.png#lightbox)

W aplikacji przy użyciu uwierzytelniania lokalnego zapewnia prostą metodę wdrażania weryfikacji użytkownika, na przykład w celu odblokowania funkcji wyłącznie dla oczu właściciel urządzenia, takich jak bankowość aplikacji lub do kontroli rodzicielskiej wsparcia dla poszczególnych aplikacja. Można również użyć jej jako sposób rozszerzenia uwierzytelniania, która już istnieje — informacje do zabezpieczenia, takich jak użytkownicy, ale one również chce mieć opcje.

Zabezpieczenia uwierzytelniania lokalnego różni się od łańcuchu kluczy. Na przykład korzystając z łańcucha kluczy, zaufanie jest od systemu operacyjnego i enklawę bezpieczny. Przy użyciu uwierzytelniania lokalnego jest pomiędzy aplikacją i systemie operacyjnym, co oznacza, że tylko mają dostęp do wyników enklawę Secure nie Secure enklawę sam.

W zakresie zabezpieczeń, jest również bardzo ważne, aby dowiedzieć się, że istnieje **dostępu** palców zarejestrowanych lub linii papilarnych obrazów. Secure enklawę jest właścicielem tych informacji, a więc innego składnika systemu ma do niego dostęp.

Aby używać funkcji Touch ID bez łańcucha kluczy dzięki wykorzystaniu lokalnego interfejsu API uwierzytelniania, istnieje kilka funkcji, które możemy użyć. Te szczegóły są przedstawione poniżej:

*   `CanEvaluatePolicy` — To po prostu warto sprawdzić, czy urządzenie jest w stanie akceptowania użycia funkcji Touch ID.
*   `EvaluatePolicy` — To rozpoczyna operację uwierzytelniania i wyświetla interfejsu użytkownika i zwraca `true` lub `false` odpowiedzi.
*   `DeviceOwnerAuthenticationWithBiometrics` — To zasad, który może służyć do wyświetlenia na ekranie funkcji Touch ID. Warto zauważyć, że nie istnieje kod dostępu rezerwowy mechanizm w tym miejscu, zamiast tego należy zaimplementować ten powrotu do aplikacji, aby zezwolić użytkownikom na pominięcie uwierzytelniania funkcji Touch ID.

Istnieje kilka wyjaśnienia dotyczące przy użyciu uwierzytelniania lokalnego, które są wymienione poniżej:

*   Podobnie jak w przypadku łańcucha kluczy, można go można uruchomić tylko na pierwszym planie. Wywoływanie wątku w tle spowoduje jego awarię.
*   Należy pamiętać, że oceny zasad może zakończyć się niepowodzeniem. Przycisk kod dostępu musi zostać zaimplementowane jako bazowy.
*   Należy podać `localizedReason` wyjaśniający, dlaczego jest wymagane uwierzytelnianie. Ułatwia to tworzenie relacji zaufania z użytkownikiem.

Następnie w poniższej sekcji przedstawiono sposób implementacji interfejsu API, biorąc pod uwagę następujące zastrzeżenia.

## <a name="adding-touch-id-to-your-application"></a>Dodawanie funkcji Touch ID do swojej aplikacji

W poprzednich sekcjach analizujemy teorii za dostępu i uwierzytelniania za pomocą uwierzytelniania lokalnego i łańcucha kluczy. Firma Microsoft będzie teraz Przyjrzyjmy się sposób integrowania funkcji Touch ID w aplikacji.

### <a name="walkthrough"></a>Wskazówki

Dlatego Przyjrzyjmy się dodanie niektórych uwierzytelniania Touch ID do naszej aplikacji. W tym przewodniku zamierzamy użyj [tabeli scenorysu](https://developer.xamarin.com/samples/StoryboardTable/) próbki. Chcemy upewnić się, że tylko właściciel urządzenia można dodać elementu do tej listy, nie chcemy zajmowały go miejsca, umożliwiając każdy Dodawanie elementu!

1. Pobierz przykład i uruchom go w programie Visual Studio dla komputerów Mac.
2. Kliknij dwukrotnie `MainStoryboard.Storyboard` otworzyć próbki w systemie iOS projektanta. Z tej próbki chcemy dodać nowy ekran do naszej aplikacji, która będzie określać uwierzytelniania. To zostanie umieszczona przed bieżącą `MasterViewController`.
3. Przeciągnij nowy **kontrolera widoku** z **przybornika** do **powierzchni projektowej**. Ustaw jako **główny kontroler widoku** przez **Ctrl + przeciągnij** z **kontrolera nawigacji**:

    [![](touchid-images/image4.png "Ustaw kontroler widoku głównego")](touchid-images/image4.png#lightbox)
4.  Nazwa nowego kontrolera widoku `AuthenticationViewController`.
5. Następnie przeciągnij przycisk i umieść ją na `AuthenticationViewController`. To wywołanie `AuthenticateButton`i nadaj mu tekst `Add a Chore`.
6. Utwórz zdarzenia na `AuthenticateButton` o nazwie `AuthenticateMe`.
7. Utwórz ręcznie segue z `AuthenticationViewController` klikając pasek czarny u dołu i **Ctrl i przeciągnij** na pasku do `MasterViewController` i wybierając polecenie **wypychania** (lub **Pokaż** Jeśli przy użyciu klasy wielkości):

    [![](touchid-images/image5.png "Przeciągnij z paska MasterViewController i wybierając wypychania lub Pokaż")](touchid-images/image6.png#lightbox)
8. Kliknij nowo utworzony segue i nadaj mu identyfikator `AuthenticationSegue`, jak pokazano poniżej:

    [![](touchid-images/image7.png "Ustaw identyfikator segue AuthenticationSegue")](touchid-images/image7.png#lightbox)
9. Dodaj następujący kod do `AuthenticationViewController`:

    ```csharp
    partial void AuthenticateMe (UIButton sender)
    {
        var context = new LAContext();
        NSError AuthError;
        var myReason = new NSString("To add a new chore");

        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError)){
            var replyHandler = new LAContextReplyHandler((success, error) => {
                this.InvokeOnMainThread(()=> {
                    if(success)
                    {
                        Console.WriteLine("You logged in!");
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        // Show fallback mechanism here
                    }
                });
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, myReason, replyHandler);
        };
    }
    ```

Jest to całego kodu, należy wdrożyć funkcję Touch ID uwierzytelniania za pomocą uwierzytelniania lokalnego. Wyróżnione wiersze w poniższym obrazie Pokaż uwierzytelnianie lokalne:

[![](touchid-images/image8.png "Wyróżnione wiersze zawierają uwierzytelnianie lokalne")](touchid-images/image8.png#lightbox)

Najpierw należy do ustalenia, czy urządzenie jest w stanie akceptowania funkcji Touch ID do wprowadzania, za pomocą `CanEvaluatePolicy` i przekazywanie w zasadach `DeviceOwnerAuthenticationWithBiometrics`. Jeśli to PRAWDA, można wyświetlić interfejsu użytkownika funkcji Touch ID przy użyciu `EvaluatePolicy`. Istnieją trzy informacje mamy do przekazania do `EvaluatePolicy` — same zasady, ciąg wyjaśniający, dlaczego wymagane jest uwierzytelnienie i obsługi odpowiedzi. Program obsługi odpowiedzi informuje aplikacji, co powinno to w przypadku uwierzytelniania powiodła się czy nie. Oto bliżej w odpowiedzi program obsługi:

[![](touchid-images/image9.png "Program obsługi odpowiedzi")](touchid-images/image9.png#lightbox)

Program obsługi odpowiedzi określono typu `LAContextReplyHandler`, który przyjmuje parametry Powodzenie — `bool` wartości i `NSError` o nazwie `error`. Jeśli powiedzie się, to gdy faktycznie zostaną wykonane niezależnie od jest ona chcemy uwierzytelniania — w takim przypadku wyświetlania ekranu, który Daj nam doda nowe kwestii. Należy pamiętać, jest jeden ostrzeżenia lokalnego uwierzytelniania, należy uruchomić na pierwszym planie, dlatego upewnij się `InvokeOnMainThread`:

[![](touchid-images/image10.png "InvokeOnMainThread jest używany do uwierzytelniania lokalnego")](touchid-images/image10.png#lightbox)

Ponadto podczas uwierzytelniania przebiegło pomyślnie, chcemy przejście do `MasterViewController`. `PerformSegue` Metody można użyć w tym celu:

[![](touchid-images/image11.png "Wywołaj metodę PerformSegue przechodzenia do MasterViewController")](touchid-images/image11.png#lightbox)

## <a name="summary"></a>Podsumowanie
W tym przewodniku analizujemy łańcucha kluczy i jak działa w systemie iOS. Możemy również zbadane łańcucha kluczy listy ACL, a to zmiany w systemie iOS. Wybraliśmy dalej, poszukaj w ramach uwierzytelniania lokalnych, co nowego w systemie iOS 8 i następnie przeglądał Implementowanie uwierzytelniania funkcji Touch ID w naszej aplikacji.

## <a name="related-links"></a>Linki pokrewne

- [Touch ID próbki](https://developer.xamarin.com/samples/StoryboardTable_LocalAuthentication)
- [Przykładowe WWDC łańcucha kluczy](https://developer.xamarin.com/samples/KeychainTouchID/)
- [Łańcucha kluczy (przykład)](https://developer.xamarin.com/samples/Keychain/)
