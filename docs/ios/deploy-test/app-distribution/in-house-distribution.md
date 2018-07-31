---
title: Dystrybucji dla aplikacji platformy Xamarin.iOS
description: Ten dokument zawiera krótkie omówienie dystrybucji aplikacji w firmie członka Apple Enterprise Developer Program.
ms.prod: xamarin
ms.assetid: 9466E51E-303E-466E-85D7-D0525E16BB37
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 1dff0e614943805930cf7d838110c4a42eee6f48
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353207"
---
# <a name="in-house-distribution-for-xamarinios-apps"></a>Dystrybucji dla aplikacji platformy Xamarin.iOS

_Ten dokument zawiera krótkie omówienie dystrybucji aplikacji w firmie członka Apple Enterprise Developer Program._

Po aplikacji platformy Xamarin.iOS został opracowany, następnym krokiem w cyklu życia tworzenia oprogramowania jest Rozsyłaj swoją aplikację dla użytkowników. Zastrzeżonych aplikacji można rozpowszechniać *wewnętrznych* (nazywanymi wcześniej przedsiębiorstwa) za pośrednictwem **Apple Developer Enterprise Program**, która zapewnia następujące korzyści:

- Aplikacja nie musi być przesłana do przeglądu przez firmę Apple.
- Nie ma ograniczeń do liczby urządzeń, na których można wdrożyć aplikację
    - Należy zauważyć, że Apple sprawia, że oczywiste, że aplikacje wewnętrzne są tylko do użytku wewnętrznego.

Jest również pamiętać, że Enterprise Program:

- Nie zapewnia dostępu do usługi iTunes Connect dystrybucji lub testowania (w tym usługi TestFlight).
- Koszt członkostwa jest 299 USD rocznie.

Wszystkie aplikacje nadal muszą być podpisane przez firmę Apple.

<a name="testing" />

## <a name="testing-your-application"></a>Testowanie aplikacji

Testowanie aplikacji odbywa się przy użyciu dystrybucji Ad Hoc. Aby uzyskać więcej informacji na temat testowania, postępuj zgodnie z instrukcjami w [dystrybucji Ad-Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) przewodnik. Należy pamiętać, że można tylko przetestować na maksymalnie 100 urządzeń.

<a name="setup" />

## <a name="getting-set-up-for-distribution"></a>Pobieranie zestawu w górę do dystrybucji

Podobnie jak w przypadku innych programów dla deweloperów firmy Apple, w ramach programu Apple Developer Enterprise tylko administratorzy zespołu agenci mogą tworzyć i dystrybucji certyfikatów i profilów aprowizacji.

Certyfikaty programu Apple Developer Enterprise Program jest ważny przez 3 lata, a profile aprowizacji wygaśnie po roku.

Ważne jest, aby pamiętać, że nie można odnawiać wygasłych certyfikatów i zamiast tego trzeba będzie zastąpić wygasły certyfikat z nową, zgodnie z opisem [poniżej](#certificate).

<a name="certificate" />

## <a name="creating-a-distribution-certificate"></a>Tworzenie certyfikatu dystrybucji

1. Przejdź do *certyfikaty, identyfikatory i profile* sekcji Apple Developer Member Center.
2. W obszarze *certyfikaty*, wybierz opcję **produkcji**.
3. Kliknij przycisk **+** przycisk, aby utworzyć nowy certyfikat.
4. W obszarze *produkcji* nagłówka, wybierz **wewnętrznego i tymczasowego**:

   [![](in-house-distribution-images/createcertmanually01.png "Wybierz wewnętrznego i tymczasowego")](in-house-distribution-images/createcertmanually01.png#lightbox)

5. Kliknij przycisk Kontynuuj i postępuj zgodnie z instrukcjami, aby utworzyć żądanie podpisania certyfikatu za pośrednictwem dostępu łańcucha kluczy:

   [![](in-house-distribution-images/createcertmanually02.png "Utwórz żądanie za pośrednictwem dostępu łańcucha kluczy do podpisania certyfikatu")](in-house-distribution-images/createcertmanually02.png#lightbox)

6. Po utworzeniu pliku CSR, zgodnie z instrukcjami, kliknij przycisk Kontynuuj, a następnie przekaż plik CSR, do witryny Member Center przeznaczonej:

   [![](in-house-distribution-images/createcertmanually03.png "Przekaż plik CSR do witryny Member Center")](in-house-distribution-images/createcertmanually03.png#lightbox)

7. Kliknij przycisk Generuj do utworzenia certyfikatu.
8. Pobierz certyfikat ukończone, a następnie kliknij dwukrotnie plik, aby go zainstalować.
9. W tym momencie należy zainstalować certyfikat na komputerze, ale może być konieczne odświeżenie profilów, aby upewnić się, że są one widoczne w środowisku Xcode.

Alternatywnie istnieje możliwość żądania certyfikatu przy użyciu okna dialogowego preferencji w środowisku Xcode. Aby to zrobić, wykonaj następujące czynności:

1. Wybierz zespół, a następnie kliknij przycisk *Wyświetl szczegóły*:

    [![](in-house-distribution-images/selectteam.png "Wybierz zespół")](in-house-distribution-images/selectteam.png#lightbox)

2. Następnie kliknij przycisk **Utwórz** znajdujący się obok **dystrybucji certyfikatu systemu iOS**:

   [![](in-house-distribution-images/selectcert.png "Tworzenie certyfikatu dystrybucji dla systemu iOS")](in-house-distribution-images/selectcert.png#lightbox)

2.   Następnie kliknij przycisk **plus (+)** i wybrać **iOS App Store**:

   [![](in-house-distribution-images/selectcert.png "Wybierz z systemem iOS App Store")](in-house-distribution-images/selectcert.png#lightbox)

<a name="profile" />

## <a name="creating-a-distribution-provisioning-profile"></a>Tworzenie profilu aprowizacji dystrybucji

<a name="appid" />

### <a name="creating-an-app-id"></a>Tworzenie Identyfikatora aplikacji

Jak z żadnych innych profilu aprowizacji możesz utworzyć identyfikator aplikacji będą musieli identyfikacji aplikacji, które można wydawać na urządzeniu użytkownika. Jeśli nie zostało jeszcze utworzone to, wykonaj poniższe kroki, aby utworzyć:


1. W [Centrum deweloperów firmy Apple](https://developer.apple.com/account/overview.action) wskaż *certyfikatu, identyfikatory i profile* sekcji. Wybierz **identyfikatory aplikacji** w obszarze **identyfikatory**.
2. Kliknij przycisk **+** przycisk, a następnie podaj **nazwa** której będzie je zidentyfikować w portalu.
3. Prefiks aplikacji powinny być już ustawiona jako swój identyfikator zespołu i nie można jej zmienić. Wybierz jawne lub identyfikator aplikacji z symbolami wieloznacznymi, a następnie wprowadź identyfikator pakietu w formacie odwrotnego DNS, takich jak: **jawne**: com. [nazwa_domeny]. [ Nazwa aplikacji] **symboli wieloznacznych**: com. [nazwa_domeny]. *
4. Wybierz dowolne [App Services](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services) wymaganych przez aplikację.
5. Kliknij przycisk **Kontynuuj** przycisk i zgodnie z instrukcjami wyświetlanymi na ekranie, aby utworzyć nowy identyfikator aplikacji.

Po utworzeniu wymagane składniki potrzebne do tworzenia profilu dystrybucji, wykonaj poniższe kroki, aby go utworzyć:

1. Wróć do portalu aprowizacji firmy Apple i wybierz **aprowizacji** > **dystrybucji**:

   [![](in-house-distribution-images/distribute01.png "Wybierz opcję aprowizacji > dystrybucji")](in-house-distribution-images/distribute01.png#lightbox)

2. Kliknij przycisk **+** przycisk, a następnie wybierz typ profilu dystrybucji, który ma zostać utworzony jako **wewnętrznych**:

   [![](in-house-distribution-images/distribute02.png "Utwórz profil dystrybucji wewnętrznej")](in-house-distribution-images/distribute02.png#lightbox)

3. Kliknij przycisk **Kontynuuj** i wybrać identyfikator aplikacji z listy rozwijanej, który chcesz utworzyć profil dystrybucji:

   [![](in-house-distribution-images/distribute03.png "Wybierz identyfikator aplikacji z listy rozwijanej")](in-house-distribution-images/distribute03.png#lightbox)

4. Kliknij przycisk **Kontynuuj** przycisku i wybierz opcję dystrybucji certyfikatu wymagane do podpisania aplikacji:

   [![](in-house-distribution-images/distribute04.png "Wybierz opcję dystrybucji certyfikatu wymagany do podpisywania aplikacji")](in-house-distribution-images/distribute04.png#lightbox)

6. Kliknij przycisk **Kontynuuj** przycisk, a następnie wprowadź **nazwa** dla nowego profilu dystrybucji:

   [![](in-house-distribution-images/distribute06.png "Wprowadź nazwę dla nowego profilu dystrybucji")](in-house-distribution-images/distribute06.png#lightbox)

7. Kliknij przycisk **Generuj** przycisk, aby utworzyć nowy profil i zakończyć proces.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 Masz Zamknij program Visual Studio dla komputerów Mac i Odśwież listę dostępnych tożsamości podpisywania i profilów aprowizacji środowiska Xcode (zgodnie z instrukcjami w [żądania tożsamości podpisywania](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) sekcji) przed nową Profil dystrybucji jest dostępny w programie Visual Studio dla komputerów Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Może być konieczne Zamknij program Visual Studio i mieć Xcode (dla systemu Mac hosta usługi kompilacji), Odśwież swoją listę dostępnych tożsamości podpisywania i inicjowania obsługi profilów (zgodnie z instrukcjami w [żądania tożsamości podpisywania](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) sekcji) zanim nowy profil dystrybucji jest dostępna w programie Visual Studio.

-----

<a name="inhouse" />

## <a name="distributing-your-app-in-house"></a>Dystrybuowanie aplikacji we własnym zakresie

Przy użyciu programu Apple Developer Enterprise licencjobiorcy jest osoba, która jest odpowiedzialny za dystrybucję aplikacji i przestrzega [wytycznych](http://adcdownload.apple.com/Documentation/License_Agreements__Apple_Developer_Enterprise_Program/Apple_Developer_Program_Enterprise_Agreement_20150608.pdf) ustawione przez firmę Apple.

Aplikację można rozpowszechniać bezpiecznie przy użyciu różnych środków, takich jak:

- Lokalnie za pomocą programu iTunes
- Serwer zarządzania urządzeniami Przenośnymi
- Serwer wewnętrzny, bezpieczne sieci web
- Adres e-mail

Do dystrybucji aplikacji w jeden z następujących sposobów musi najpierw utworzyć plik IPA, jak wyjaśniono w następnej sekcji.


### <a name="creating-an-ipa-for-in-house-deployment"></a>Tworzenie archiwum IPA wewnętrznych wdrożenia

Po zainicjowaniu obsługi administracyjnej, aplikacji mogą być umieszczone w plik znany jako *IPA*. Jest to plik zip, który zawiera aplikację, a także dodatkowe metadane i ikony. IPA służy do dodawania aplikacji lokalnie w programie iTunes, dzięki czemu mogą być synchronizowane bezpośrednio do urządzenia, który znajduje się w profilu inicjowania obsługi administracyjnej.

Aby uzyskać więcej informacji na temat tworzenia Zobacz IPA [Obsługa IPA](~/ios/deploy-test/app-distribution/ipa-support.md) przewodnik.


## <a name="summary"></a>Podsumowanie

W tym artykule zapewniła krótkie omówienie we własnym zakresie dystrybuowanie aplikacji platformy Xamarin.iOS.

## <a name="related-links"></a>Linki pokrewne

- [Dystrybucja w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Dystrybucja Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Plik iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Obsługa IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
