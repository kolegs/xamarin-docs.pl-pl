---
title: "Przy użyciu TestFlight"
description: "TestFlight teraz jest własnością firmy Apple, a jest to podstawowy sposób do wersji beta testowania aplikacji platformy Xamarin.iOS. Ten artykuł przeprowadzi Cię przez wszystkie kroki procesu TestFlight — z przekazywania aplikacji, do pracy z iTunes Connect."
ms.topic: article
ms.prod: xamarin
ms.assetid: BA880768-2BC8-41E4-B57E-A56F8EED4690
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 8de7b91e5854e5c660788cdca055860b2ba0139e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="using-testflight"></a>Przy użyciu TestFlight

_TestFlight teraz jest własnością firmy Apple, a jest to podstawowy sposób do wersji beta testowania aplikacji platformy Xamarin.iOS. Ten artykuł przeprowadzi Cię przez wszystkie kroki procesu TestFlight — z przekazywania aplikacji, do pracy z iTunes Connect._

Testowania wersji beta jest integralną częścią cyklu tworzenia oprogramowania, a istnieje wiele aplikacji dla wielu platform oferty może usprawnić ten proces, takich jak [HockeyApp](http://hockeyapp.net/features/), [Applause](http://www.applause.com/mobile-app-testing)i oczywiście Google Play natywnych aplikacji w wersji Beta testowania dla aplikacji systemu Android. Ten dokument koncentruje się na TestFlight firmy Apple.

TestFlight usługa testowania wersji beta firmy Apple dla aplikacji systemu iOS i jest dostępna tylko za pośrednictwem [iTunes Connect](https://itunesconnect.apple.com/). Jest dostępna dla aplikacji systemu iOS 8.0 lub nowszym. TestFlight umożliwia użytkownikom wewnętrznych i zewnętrznych testowania wersji beta, a z powodu Beta Przegląd aplikacji w przypadku drugiego nagłówka, zapewnia znacznie łatwiejsze procesu w ostatnim zapoznania się z nimi podczas publikowania do sklepu z aplikacjami.


Wcześniej plik binarny został wygenerowany w programie Visual Studio dla komputerów Mac i przekazać do witryny sieci Web TestFlightApp w celu dystrybucji do testerów. Nowy proces istnieje wiele ulepszeń zezwalające wysokiej jakości, ma również przetestowanej aplikacji w sklepie z aplikacjami. Na przykład:


- Przegląd aplikacji w wersji Beta, wymaganych do testowania zewnętrznej zapewnia wyższy prawdopodobieństwo pomyślnego dla użytkownika końcowego przejrzeć aplikacji sklepu, jak wymaga zgodność z zasadami firmy Apple.
- Przed przesłaniem, aplikacja musi być zarejestrowany w usłudze iTunes Connect. Dzięki temu, że nie będzie istnieć niezgodność profilów aprowizacji, nazwy i certyfikaty.
- Aplikacja TestFlight jest teraz aplikacji iOS prawdziwe, aby szybciej działa.
- Po zakończeniu testowania wersji beta procesu przenoszenia aplikacji, aby przejrzeć jest szybkie i wydajne; Wystarczy jedno kliknięcie przycisku.

## <a name="requirements"></a>Wymagania

Tylko te aplikacje, które są iOS 8.0 lub nowszej można przetestować za pośrednictwem TestFlight.

Wszystkie testerów należy przetestować aplikację na co najmniej urządzenia z systemem iOS 8. Jednak najlepszym rozwiązaniem decyduje, którego powinien zostać przetestowany aplikacji we wszystkich wersjach systemu iOS

## <a name="provisioning"></a>Inicjowanie obsługi administracyjnej

Aby przetestować kompilacji z TestFlight, musisz utworzyć *profil dystrybucji sklepu z aplikacjami* z nowego uprawnienia w wersji beta. To uprawnienie umożliwia testowania wersji beta za pośrednictwem TestFlight oraz wszelkie **nowe** profil dystrybucji sklepu z aplikacjami automatycznie zawiera to uprawnienie. Możesz wykonać instrukcje krok po kroku w [tworzenia profilu dystrybucji](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) przewodniku, aby wygenerować nowy profil.

Można potwierdzić, że profil dystrybucji zawiera uprawnienie beta podczas [weryfikacji kompilacji w środowisku Xcode](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md), jak pokazano poniżej:

[ ![](testflight-images/validate-build.png "Przesyłanie aplikacji do firmy Apple")](testflight-images/validate-build.png)


## <a name="testflight-workflow"></a>TestFlight przepływu pracy

Poniższy przepływ pracy zawiera opis czynności, aby rozpocząć korzystanie z TestFlight wersji Beta testowania aplikacji:

1. W przypadku nowych aplikacji, utworzyć [iTunes rekordu Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) .
2. [Archiwum i opublikuj](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) iTunes Połącz przez aplikację.
3. Zarządzanie testowania wersji Beta:
    - Dodawanie metadanych.
    - Dodaj użytkowników wewnętrznych:
        - Maksymalna liczba 25 użytkowników.
    - Dodawanie użytkowników zewnętrznych:
        - Maksymalna liczba 1000 użytkowników.
        - Wymaga przeglądu testu w wersji beta, który wymaga zgodności z wytycznymi firmy Apple.
4. Odbieraj opinie od użytkowników, obsługiwanie go i wróć do kroku 2.

## <a name="create-an-itunes-connect-record"></a>Utwórz rekord Connect iTunes

1.  Zaloguj się do [iTunes portalu Connect](https://itunesconnect.apple.com/) przy użyciu poświadczeń deweloperów firmy Apple.
2.  Wybierz **Moje aplikacje**:

    [ ![](testflight-images/my-apps.png "Wybierz Moje aplikacje")](testflight-images/my-apps.png)


3.  Na **Moje aplikacje** ekranu, kliknij pozycję  **+**  przycisk w lewym górnym rogu ekranu, aby dodać nową aplikację. Jeśli masz konta dewelopera Mac i z systemem iOS, pojawi się monit, aby wybrać nowy typ aplikacji w tym miejscu.

Użytkownik zobaczy **nowych aplikacji dla systemu iOS** przesyłanie okna, które musi zawierać dokładnie takie same informacje jak Info.plist aplikacji

Aby uzyskać więcej informacji o tworzeniu nowego iTunes rekordu Connect dotyczą [tworzenia iTunes rekordu Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) przewodnik.



###  <a name="completing-the-new-ios-app-submission-form"></a>Kończenie nowego formularza przesyłania aplikacji dla systemu iOS

Formularz powinien odzwierciedlać dokładnie informacji w pliku Info.plist aplikacji, jak przedstawiono poniżej:

[ ![](testflight-images/infoplist.png "Info.plist aplikacji") ](testflight-images/infoplist.png) 
 [ ![ ] (testflight-images/newiosapp.png "formularza na iTunes Connect")](testflight-images/newiosapp.png)

-  **Nazwa** — opisową nazwę używanej podczas konfigurowania pakietu aplikacji. Musi to być dokładna **Nazwa aplikacji** wpisu w Twojej `Info.plist`.
-  **Podstawowy język** — podstawowy język w aplikacji. Jest to zwykle niezależnie od języka mowy użytkownik.
-  **Identyfikator pakietu** — Lista identyfikatorów aplikacji menu rozwijane utworzony na swoje konto dewelopera.
    *   **Sufiks Identyfikatora pakietu** — Jeśli wybrano symbol wieloznaczny identyfikator pakietu (kończąca się * w naszym przykładzie powyżej), dodatkowe pojawi się okno sygnalizowania sufiksu Identyfikatora pakietu. W przykładzie **identyfikator pakietu** jest `mobi.chkn.*`, sufiks jest **widok strony**. Te składają się **identyfikator pakietu** w naszym `Info.plist`.
-  **Wersja** — numer wersji aplikacji przekazywanego. To jest wybierany przez dewelopera.
-  **Jednostka SKU** — jednostka SKU jest unikatowy identyfikator dla aplikacji, które nie będą widoczne dla użytkowników. Go można traktować w podobny sposób jak identyfikatora produktu. W powyższym przykładzie wybrano I datę oraz numer wersji dla tej daty.


## <a name="upload-your-app"></a>Przekaż aplikację

Po utworzeniu iTunes rekordu Connect można przekazać nowych kompilacji. Należy pamiętać, że kompilacje muszą mieć nowe uprawnienia w wersji beta.

Najpierw należy utworzyć użytkownika [final dystrybucyjnego](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) w środowisku IDE, następnie [przesyłania aplikacji do firmy Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) przez moduł ładujący aplikacji lub funkcji archiwum w środowisku Xcode.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

###  <a name="create-an-archive"></a>Utwórz archiwum

 Tworzenie pliku binarnego w programie Visual Studio dla komputerów Mac, musisz użyć _archiwum_ funkcji. Kliknij prawym przyciskiem myszy projekt i wybierz **archiwum publikowania**, jak pokazano poniżej:

 [ ![](testflight-images/new-archive.png "Wybierz archiwum do publikowania")](testflight-images/new-archive.png)


 Zapoznaj się [tworzenia Distributable](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) przewodnika, aby uzyskać więcej informacji.

###  <a name="sign-and-distribute-your-app"></a>Zaloguj się i dystrybucja aplikacji

 Utworzenie archiwum zostanie automatycznie otwarte **widoku archiwa**, wyświetlanie wszystkich zarchiwizowanych projektów pogrupowane według rozwiązania. Aby zarejestrować aplikację i przygotowania go do dystrybucji, wybierz **logowania i rozpowszechnij...** , przedstawiono poniżej:

[ ![](testflight-images/archive-view.png "Utworzenie archiwum spowoduje automatyczne otwarcie widoku archiwa")](testflight-images/archive-view.png)

 Spowoduje to otwarcie Kreatora publikacji. Wybierz **sklepu z aplikacjami** kanału dystrybucji, aby utworzyć pakiet i otwórz moduł ładujący aplikacji. Na ekranie profilu inicjowania obsługi administracyjnej wybierz Twojej tożsamości podpisywania i profilu inicjowania obsługi administracyjnej lub ponowne podpisanie z inną tożsamością. Sprawdź szczegóły pakietu, a następnie kliknij przycisk **publikowania** można zapisać użytkownika `.ipa`

[ ![](testflight-images/group.png "Wybierz tożsamość podpisywania i profil inicjowania obsługi administracyjnej lub ponowne podpisanie z inną tożsamością")](testflight-images/group.png)

 Zapoznaj się [przesyłania aplikacji do firmy Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) sekcji, aby uzyskać więcej informacji na temat tych kroków.

###  <a name="submitting-your-build"></a>Przesyłanie kompilacji
 Kreatora publikacji zostanie otwarty program ładujący aplikacji wszystkie możesz Przekaż kompilację do iTunes Connect. Wybierz **dostarczania aplikacji** opcji i przekazać `.ipa` pliku utworzone powyżej. Moduł ładujący aplikacji będzie sprawdzania poprawności i przekaż kompilację do iTunes Connect.

 Zapoznaj się [przesyłania aplikacji do firmy Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) sekcji, aby uzyskać więcej informacji na temat tych kroków.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

###  <a name="building-your-final-distributable"></a>Tworzenie sieci final dystrybucyjnego
 Ponieważ dodatek plug-in Xamarin dla Visual Studio nie obsługuje archiwizacji aplikacji platformy Xamarin.iOS do publikowania w sklepie z aplikacjami, dostępne są dwie opcje publikowania aplikacji systemu iOS w programie Visual Studio. Są to:

1. Przekaż plik IPA utworzone za pomocą polecenia kompilacji IPA ad hoc.
1. Przekaż zip `.app` pakietu.

 [Tworzenia Distributable](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) przewodnik zawiera instrukcje dla obu tych opcji.

###  <a name="submitting-your-build"></a>Przesyłanie kompilacji
 Aby przesłać aplikacji do firmy Apple, należy przenieść z hosta usługi kompilacji i korzystać program ładujący aplikacji, który jest instalowany jako część programu Xcode. Aby uzyskać więcej informacji dotyczących uzyskiwania dostępu do modułu ładującego aplikacji, zobacz do firmy Apple [moduł ładujący aplikacji dostępu](http://help.apple.com/itc/apploader/#/apdATD1E927-D1E1A1303-D1E927A1126) przewodnik.

Po otwarciu, wybierz **dostarczania aplikacji** opcji i przekazać zip lub `.ipa` pliku utworzone powyżej. Moduł ładujący aplikacji będzie sprawdzania poprawności i przekaż kompilację do iTunes Connect.

 Zapoznaj się [przesyłania aplikacji do firmy Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) sekcji, aby uzyskać więcej informacji na temat tych kroków.

-----


[Publikowania do sklepu z aplikacjami](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) wszystkie powyższe kroki bardziej szczegółowo opisano, zapoznaj się to na pełniejsze wyglądu w procesie przesyłania sklepu z aplikacjami.

Po powrocie do **Moje aplikacje** sekcji z iTunes Connect, należy odnaleźć czy aplikacji został pomyślnie przekazany. W tym momencie możesz są teraz gotowe do wykonania niektórych testów Beta!

## <a name="manage-beta-testing"></a>Zarządzanie testowania wersji Beta

### <a name="add-metadata"></a>Dodawanie metadanych

Aby rozpocząć korzystanie z TestFlight, przejdź do **wersja wstępna** kartę aplikacji. Trzy karty przedstawiający listę kompilacji, testerów wewnętrznych i zewnętrznych testerów powinny być widoczne, jak przedstawiono poniżej:

[ ![](testflight-images/app-uploaded.png "Karty kompilacji, testerów wewnętrznych i zewnętrznych testerów")](testflight-images/app-uploaded.png)

Aby dodać metadane do aplikacji, kliknij numer kompilacji, a następnie TestFlight:

[ ![](testflight-images/metadata.png "Dodawanie metadanych")](testflight-images/metadata.png)

W obszarze **informacji testu**, można zapewnić testerów istotne informacje dotyczące aplikacji, na przykład:

- Co do testowania
- Opis aplikacji.
- Adres URL Marketing — zapewni informacje o aplikacji, które dodajesz.
- Adres URL zasad ochrony prywatności — Adres URL zawierający informacje dotyczące zasady zachowania poufności informacji w firmie.
- Adres E-mail opinii.

Należy pamiętać, że te metadane **nie jest** wymagane dla testerów wewnętrznych, ale **jest** wymagane dla testerów zewnętrznych.

<a name="beta-testing" />

### <a name="enable-beta-testing"></a>Włącz testowania wersji Beta

Gdy wszystko będzie gotowe rozpocząć testowanie aplikacji, Włącz **TestFlight Beta testowanie** przełącznika dla danej wersji:

[ ![](testflight-images/turn-on-testing.png "Włącz przełącznik TestFlight testowanie wersji Beta")](testflight-images/turn-on-testing.png)

Jest aktywny dla każdej kompilacji **60 dni** od daty włączone na przełączniku TestFlight Beta. Widoczny pozostało dla każdej kompilacji, na ile dni **informacji testu** strony:

[ ![](testflight-images/daysleft.png "Strona informacji o testu")](testflight-images/daysleft.png)

Testowanie można wyłączyć w dowolnym momencie.

### <a name="internal-testers"></a>Wewnętrzny testerów

Wewnętrzny testerów są członkami zespół deweloperów, którzy zostali przypisani jedną z następujących ról w iTunes Connect:

-  **Administrator** — administratora jest odpowiedzialny za dodawanie i zarządzanie nimi nowych użytkowników w iTunes Connect.
-  **Prawne** — Agent zespołu jest tylko administrator przypisania roli prawnych. Umożliwia do podpisania Umowy.
-  **Techniczne** — techniczne użytkownik może zmienić większość właściwości dotyczących aplikacji. Na przykład edytować informacje o aplikacji, przekazać dane binarne i wysłać aplikację do przeglądu.

Maksymalnie 25 elementy Członkowskie mogą być udostępniane każdej kompilacji.

Aby dodać testerów, przejdź do **użytkownikami i rolami** na ekranie głównym iTunes połączenia:

[ ![](testflight-images/users-and-roles.png "Użytkownicy i role na ekranie głównym iTunes połączenia")](testflight-images/users-and-roles.png)

Istniejące iTunes Połącz użytkowników będą wyświetlane na liście. Aby je zaznaczyć, kliknij jego nazwę, Włącz **wewnętrzny Tester** przełącznik, a następnie kliknij przycisk **zapisać**:

[ ![](testflight-images/internal-tester.png "Włącz przełącznik wewnętrzny Tester")](testflight-images/internal-tester.png)

Aby dodać użytkownika, który nie znajduje się na liście, wybierz opcję  **+**  znajdujący się obok *użytkowników*i podaj pierwszy adres nazwę, nazwisko i adres e-mail do utworzenia konta. Użytkownik musi potwierdzić swój adres e-mail, aby uaktywnić konto:

[ ![](testflight-images/add-new-user.png "Dodawanie użytkownika")](testflight-images/add-new-user.png)

Jeśli **Moje aplikacje > wersja wstępna > wewnętrzny testerów**, znajduje się teraz użytkowników, które zostały dodane do testów wewnętrznych TestFlight beta:

[ ![](testflight-images/select-users.png "Listę użytkowników, które zostały dodane do testów wewnętrznych TestFlight w wersji beta")](testflight-images/select-users.png)

Możesz zaprosić te testerów zaznaczyć jego nazwę, a następnie klikając polecenie **zaprosić** przycisku. Otrzymają wiadomość e-mail z zaproszeniem do testowania aplikacji.

Można wyświetlić stan ich zaproszenia w kolumnie Stan strony testerów wewnętrznego:

[ ![](testflight-images/status-added.png "Stan zaproszenia")](testflight-images/status-added.png)


### <a name="external-testers"></a>Zewnętrzne testerów

Zanim wyświetli zewnętrznych testerów do wersji beta testowanie aplikacji, go musi przechodzić przez Przegląd aplikacji w wersji Beta i, w związku z tym musi być zgodna z [wytyczne przeglądu sklepu aplikacji](https://developer.apple.com/app-store/review/guidelines/).

Aby przesłać aplikacji do przeglądu, kliknij przycisk **przesłać dla wersji Beta aplikacji przejrzyj** tekst obok kompilacji, jak pokazano na poniższej ilustracji:

[ ![](testflight-images/beta-app-review.png "Przedstawia Przegląd aplikacji w wersji Beta")](testflight-images/beta-app-review.png)

Dla aplikacji do przekazania przeglądu należy wprowadzić wszystkie wymagane metadane na stronie informacje o wersji Beta TestFlight.

Można teraz uruchomić przygotować zaproszeń do skorzystania z, a następnie dodaj do 2000 testerów zewnętrznych za pomocą karty zewnętrznej testerów, wprowadzając ich poczty e-mail, imię i nazwisko, jak pokazano na poniższym zrzucie ekranu. Wiadomości e-mail, którą można wprowadzić musi być swój identyfikator firmy Apple; jest to po prostu wiadomości e-mail, którą otrzyma zaproszenie na.

[ ![](testflight-images/add-external.png "Zaproś testerów")](testflight-images/add-external.png)

Jeśli masz dużą liczbę testerów zewnętrznych, możesz użyć **Importuj plik** łącze, aby zaimportować `CSV` pliku o następującym formacie w jednym wierszu:

``` 
first name, last name, email address
```

Zewnętrzne testerów można również dodać do różnych grup, aby chronić Twoje testerów uporządkowane.

Po wprowadzeniu szczegóły testerów zewnętrznych, kliknij przycisk **Dodaj** i Potwierdź, że posiadasz wyrażenia zgody na Poproś użytkowników:

[ ![](testflight-images/confirm-consent.png "Upewnij się, że masz użytkowników, zgody zaproszenie")](testflight-images/confirm-consent.png)

Dopiero po pomyślnym Przegląd aplikacji w wersji Beta będzie można wysłać zaproszenia do zewnętrznego testerów. W tym momencie tekst w polu **zewnętrznych** na kompilacji strony zmieni się na **wysyłania zaprasza**. Kliknij tutaj, aby wysłać zaproszenia do wszystkich testerów się, że masz już dodany.

Jeśli aplikacja została odrzucona, należy rozwiązać problemy z wyświetlany w **Centrum**i ponownie prześlij całą zaktualizowane dane binarne do przeglądu.

## <a name="as-a-beta-tester"></a>Jako Beta Tester

Po zaprosić Twojej tester, otrzymają wiadomość e-mail, podobnie jak na poniższym zrzucie ekranu:

[ ![](testflight-images/tester-email.png "Przykład zaproszenia e-mail")](testflight-images/tester-email.png)

Po kliknięciu **Otwórz w TestFlight** przycisk aplikacji będą otwierane w aplikacji TestFlight lub jeśli jeszcze nie została pobrana, zostanie bezpośrednio do sklepu z aplikacjami i zezwolić im na jej pobranie.

Raz Twojej aplikacji zostanie otwarty w programie TestFlight, jego zostaną wyświetlone szczegółowe informacje o do testowania i wyświetli monit o tester instalowania aplikacji do ich iOS 8.0 (lub nowszym) urządzenia:

[ ![](testflight-images/install-app.png "TestFlight zostaną wyświetlone szczegółowe informacje o do testowania")](testflight-images/install-app.png)

Kompilacje test zostanie wskazany na ekranu urządzenia przez pomarańczowy kropka poprzedzających nazwę aplikacji.

Testerów można przekazać opinię za pośrednictwem aplikacji TestFlight i spowoduje zmniejszenie tych informacji na adres e-mail podany w metadanych.

### <a name="beta-testing-complete"></a>Zakończenie testowania wersji beta

Po zakończeniu testowania wersji beta można teraz przesyłania aplikacji do sklepu z aplikacjami przeglądu przez firmę Apple. Ten proces odbywa się bardzo straightforwardly w iTunes Connect, klikając **przesyłania do przeglądu** przycisku, jak przedstawiono poniżej:

[ ![](testflight-images/submit-for-review.png "Kliknij przycisk przesyłania dla przycisku przeglądu")](testflight-images/submit-for-review.png)

## <a name="summary"></a>Podsumowanie

W tym artykule przeglądał sposobu korzystania z firmy Apple TestFlight Beta testowanie za pomocą programu iTunes Connect. Go opisano sposób przekazywania nowej kompilacji do iTunes Connect i jak z zaproszeniem dla wewnętrznych i zewnętrznych testerów Beta do używania aplikacji.

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie rekordu Connect iTunes](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Publikowanie w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Inicjowanie obsługi aplikacji do sklepu z aplikacjami dystrybucji](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#provisioning)
- [Przy użyciu TestFlight firmy Apple w wersji Beta](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/BetaTestingTheApp.html#//apple_ref/doc/uid/TP40011225-CH35-SW2)
