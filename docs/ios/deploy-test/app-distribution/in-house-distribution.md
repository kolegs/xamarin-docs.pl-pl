---
title: "Dystrybucji wewnętrznych"
description: "Ten dokument zawiera krótkie omówienie dystrybucji aplikacji we własnym zakresie, jako członka Program firmy Apple Developer Enterprise."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9466E51E-303E-466E-85D7-D0525E16BB37
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4da96f3119fb46fbeb22ad3d6c68b3099f6d0698
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="in-house-distribution"></a>Dystrybucji wewnętrznych

_Ten dokument zawiera krótkie omówienie dystrybucji aplikacji we własnym zakresie, jako członka Program firmy Apple Developer Enterprise._

Aplikację platformy Xamarin.iOS został opracowany, następnym krokiem w cyklu tworzenia oprogramowania po dystrybucję aplikacji dla użytkowników. Zastrzeżonych aplikacje mogą być dystrybuowane *wewnętrznych* (wcześniej nazywane przedsiębiorstwa) za pośrednictwem **Apple Developer Enterprise Program**, która oferuje następujące korzyści:

- Aplikacja nie trzeba można przesłać do przeglądu przez firmę Apple.
- Bez ograniczeń do liczby urządzeń, na których można wdrożyć aplikację
    - Należy pamiętać, że Apple umożliwia oczywiste, że wewnętrznych aplikacji są tylko do użytku wewnętrznego.

Należy również zauważyć, że Program przedsiębiorstwa:

- Nie zapewnia dostęp do programu iTunes Connect do dystrybucji lub testowania (w tym TestFlight).
- Koszt członkostwa jest 299 USD na rok.

Wszystkie aplikacje nadal muszą być podpisane przez firmę Apple.

<a name="testing" />

## <a name="testing-your-application"></a>Testowanie aplikacji

Testowanie aplikacji jest przeprowadzane przy użyciu funkcji dystrybucji Ad Hoc. Aby uzyskać więcej informacji na temat testowania, postępuj zgodnie z instrukcjami [dystrybucji Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) przewodnik. Należy pamiętać, że tylko można przetestować na maksymalnie 100 urządzeń.

<a name="setup" />

## <a name="getting-set-up-for-distribution"></a>Pobieranie zdefiniować ustawienia dla dystrybucji

Podobnie jak w przypadku innych programów deweloperów firmy Apple, w obszarze Apple Developer Enterprise Program tylko dla administratorów zespołów i agentów można utworzyć dystrybucji certyfikatów oraz profile inicjowania obsługi.

Apple Developer Enterprise Program certyfikaty będą trwać przez 3 lata, a profile aprowizacji wygaśnie po roku.

Należy pamiętać, że nie można odnowić wygasłe certyfikaty, a zamiast tego należy zastąpić wygasłego certyfikatu nową, zgodnie z opisem [poniżej](#certificate).

<a name="certificate" />

## <a name="creating-a-distribution-certificate"></a>Tworzenie certyfikatu dystrybucji

1. Przejdź do *certyfikaty, identyfikatory & profile* sekcji Apple Developer Member Center.
2. W obszarze *certyfikaty*, wybierz pozycję **produkcji**.
3. Kliknij przycisk  **+**  przycisk, aby utworzyć nowy certyfikat.
4. W obszarze *produkcji* nagłówek, wybierz **wewnętrznych i Ad Hoc**:

   [![](in-house-distribution-images/createcertmanually01.png "Wybierz wewnętrznych i Ad Hoc")](in-house-distribution-images/createcertmanually01.png#lightbox)

5. Kliknij przycisk Kontynuuj i postępuj zgodnie z instrukcjami, aby utworzyć żądanie podpisania certyfikatu za pośrednictwem dostępu łańcucha kluczy:

   [![](in-house-distribution-images/createcertmanually02.png "Utwórz certyfikat podpisywania żądania za pośrednictwem dostępu łańcucha kluczy")](in-house-distribution-images/createcertmanually02.png#lightbox)

6. Po utworzeniu programu obsługi, zgodnie z instrukcjami, kliknij przycisk Kontynuuj i przekazać programu obsługi do witryny Member Center:

   [![](in-house-distribution-images/createcertmanually03.png "Przekaż do witryny Member Center obsługi klienta")](in-house-distribution-images/createcertmanually03.png#lightbox)

7. Kliknij przycisk Generuj utworzyć certyfikatu.
8. Pobierz certyfikat ukończone i kliknij go dwukrotnie, aby go zainstalować.
9. W tym momencie certyfikatu należy zainstalować na komputerze, ale może być konieczne odświeżenie profile, aby upewnić się, że są one widoczne w programie Xcode.

Alternatywnie jest możliwość żądania certyfikatu za pomocą okna dialogowego preferencji w środowisku Xcode. Aby to zrobić, wykonaj następujące czynności:

1. Wybierz zespół, a następnie kliknij przycisk *Wyświetl szczegóły*:

    [![](in-house-distribution-images/selectteam.png "Wybierz zespół")](in-house-distribution-images/selectteam.png#lightbox)

2. Następnie kliknij przycisk **Utwórz** znajdujący się obok **dystrybucji certyfikat iOS**:

   [![](in-house-distribution-images/selectcert.png "Utwórz certyfikat dystrybucji z systemem iOS")](in-house-distribution-images/selectcert.png#lightbox)

2.   Następnie kliknij przycisk **plus (+)** i wybrać **iOS App Store**:

   [![](in-house-distribution-images/selectcert.png "Wybierz iOS App Store")](in-house-distribution-images/selectcert.png#lightbox)

<a name="profile" />

## <a name="creating-a-distribution-provisioning-profile"></a>Tworzenie profilu inicjowania obsługi administracyjnej dystrybucji

<a name="appid" />

### <a name="creating-an-app-id"></a>Tworzenie Identyfikatora aplikacji

Jako wszystkie inne inicjowania obsługi profilu możesz utworzyć identyfikator aplikacji będą musieli zidentyfikować aplikację, która będzie rozpowszechniania do urządzenia użytkownika. Jeśli to nie zostało już utworzone, wykonaj poniższe kroki, aby utworzyć:


1. W [Centrum deweloperów firmy Apple](https://developer.apple.com/account/overview.action) przejdź do *certyfikatu, identyfikatory i profile* sekcji. Wybierz **identyfikatorów aplikacji** w obszarze **identyfikatory**.
2. Kliknij przycisk  **+**  przycisk i podaj **nazwa** której znajdą w portalu.
3. Prefiks aplikacji powinna być już ustawiona jako Identyfikatora zespołu i nie można zmienić. Wybierz opcję Explicit, lub identyfikator aplikacji symboli wieloznacznych, a następnie wprowadź identyfikator pakietu w formacie wstecznego DNS, takich jak: **Explicit**: modelu com. [nazwa_domeny]. [ Nazwa aplikacji] **symbolu wieloznacznego**: modelu com. [nazwa_domeny]. *
4. Wybierz dowolne [usługi aplikacji](~/ios/get-started/installation/device-provisioning/index.md#appservices) aplikację wymagającej.
5. Kliknij przycisk **Kontynuuj** przycisk i zgodnie z instrukcjami wyświetlanymi na ekranie można utworzyć nowego identyfikatora aplikacji.

Po utworzeniu wymagane składniki potrzebne do tworzenia profilu dystrybucji, wykonaj poniższe kroki, aby go utworzyć:

1. Powróć do portalu inicjowania obsługi administracyjnej firmy Apple i wybierz **inicjowania obsługi administracyjnej** > **dystrybucji**:

   [![](in-house-distribution-images/distribute01.png "Wybierz opcję inicjowania obsługi administracyjnej > dystrybucji")](in-house-distribution-images/distribute01.png#lightbox)

2. Kliknij przycisk  **+**  przycisk i wybierz typ profilu dystrybucji, który ma zostać utworzony jako **wewnętrznych**:

   [![](in-house-distribution-images/distribute02.png "Utwórz profil dystrybucji wewnętrznych")](in-house-distribution-images/distribute02.png#lightbox)

3. Kliknij przycisk **Kontynuuj** i wybrać z listy rozwijanej, która ma zostać utworzony profil dystrybucji dla Identyfikatora aplikacji:

   [![](in-house-distribution-images/distribute03.png "Wybierz z listy rozwijanej identyfikator aplikacji")](in-house-distribution-images/distribute03.png#lightbox)

4. Kliknij przycisk **Kontynuuj** przycisk i wybierz opcję dystrybucji certyfikatu wymagane do podpisania aplikacji:

   [![](in-house-distribution-images/distribute04.png "Wybierz opcję dystrybucji certyfikatu wymagane do podpisania aplikacji")](in-house-distribution-images/distribute04.png#lightbox)

6. Kliknij przycisk **Kontynuuj** przycisk, a następnie wprowadź **nazwa** nowego profilu dystrybucji:

   [![](in-house-distribution-images/distribute06.png "Wprowadź nazwę nowego profilu dystrybucji")](in-house-distribution-images/distribute06.png#lightbox)

7. Kliknij przycisk **Generuj** przycisk, aby utworzyć nowy profil i zakończenia procesu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 Być może trzeba zamknąć program Visual Studio dla komputerów Mac i ma Xcode, Odśwież listę dostępnych tożsamości podpisywania i profile inicjowania obsługi (zgodnie z instrukcjami w [żąda tożsamości podpisywania](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) sekcji) przed nowy Profil dystrybucji jest dostępny w programie Visual Studio dla komputerów Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Być może trzeba zamknąć program Visual Studio i ma Xcode (na hosta usługi kompilacji Mac), Odśwież swoją listę dostępnych tożsamości podpisywania i profile inicjowania obsługi (zgodnie z instrukcjami w [żąda tożsamości podpisywania](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) sekcji) zanim nowy profil dystrybucji jest dostępna w programie Visual Studio.

-----

<a name="inhouse" />

## <a name="distributing-your-app-in-house"></a>Dystrybucja aplikacji we własnym zakresie

Z firmy Apple Developer Enterprise Program Licencjobiorcy to osoby odpowiedzialne za dystrybucję aplikacji i przestrzegać [wytyczne](http://adcdownload.apple.com/Documentation/License_Agreements__Apple_Developer_Enterprise_Program/Apple_Developer_Program_Enterprise_Agreement_20150608.pdf) ustawiony przez firmę Apple.

Aplikacji mogą być dystrybuowane bezpiecznie przy użyciu różnych środków, takich jak:

- Lokalnie za pomocą programu iTunes
- Serwer zarządzania urządzeniami Przenośnymi
- Serwer wewnętrzny, bezpieczne sieci web
- Adres e-mail

Do dystrybucji aplikacji w jednym z następujących sposobów musi najpierw utworzyć plik IPA zgodnie z objaśnieniem w następnej sekcji.


### <a name="creating-an-ipa-for-in-house-deployment"></a>Trwa tworzenie IPA dla wewnętrznych wdrożenia

Po zainicjowaniu obsługi, aplikacje można spakować do pliku, znany jako *IPA*. Jest to plik zip, który zawiera aplikację, a także dodatkowe metadane i ikon. IPA umożliwia dodawanie aplikacji lokalnie do programu iTunes, dzięki czemu można synchronizować bezpośrednio do urządzenia, który znajduje się w profilu inicjowania obsługi administracyjnej.

Aby uzyskać więcej informacji na temat tworzenia Zobacz IPA [Obsługa IPA](~/ios/deploy-test/app-distribution/ipa-support.md) przewodnik.


## <a name="summary"></a>Podsumowanie

W tym artykule udzielił krótkie omówienie we własnym zakresie dystrybucja aplikacji platformy Xamarin.iOS.

## <a name="related-links"></a>Linki pokrewne

- [Dystrybucja w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Dystrybucja Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Plik iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Obsługa IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
