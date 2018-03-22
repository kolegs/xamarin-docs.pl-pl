---
title: Dystrybucji sklepu z aplikacjami
description: "W tym dokumencie opisano wymagania dotyczące dystrybucji do sklepu z aplikacjami firmy Apple."
ms.topic: article
ms.prod: xamarin
ms.assetid: B07E2C1F-A6DF-43CB-BFB0-0252A5558467
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: 25c4cb980f77880ae690916ec45be3cd12a3cf10
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="app-store-distribution"></a>Dystrybucji sklepu z aplikacjami

_W tym dokumencie opisano wymagania dotyczące dystrybucji do sklepu z aplikacjami firmy Apple._

Aplikacji platformy Xamarin.iOS został opracowany, następnym krokiem w cyklu tworzenia oprogramowania po dystrybucję aplikacji użytkownikom za pomocą programu iTunes App Store. Jest to najczęściej dystrybucji aplikacji. Przez publikowanie aplikacji w sklepie z aplikacjami firmy Apple, może on dostępne dla konsumentów na całym świecie.

> [!IMPORTANT]
> Jest **ważne** pamiętać, że program iTunes Connect, a w związku z tym publikowanie aplikacji do sklepu z aplikacjami, możesz **musi** być częścią albo indywidualnych lub organizacji Program deweloperów firmy Apple. Nie można wykonaj kroki opisane na tej stronie, jeśli jesteś członkiem Apple Developer **Enterprise** Program.

Dystrybucja aplikacji — tak jak w przypadku tworzenia aplikacji, — wymaga, aby aplikacje były udostępniane za pomocą odpowiedniej *profil inicjowania obsługi administracyjnej*. Profile inicjowania obsługi administracyjnej są pliki zawierające kod podpisywania informacje, a także tożsamości aplikacji i mechanizmu dystrybucji zamierzone. Zawierają także informacje o jakie urządzenia można wdrożyć aplikację do dystrybucji - App Store.

<a name="provisioning" />

## <a name="provisioning-an-app-for-app-store-distribution"></a>Inicjowanie obsługi aplikacji do sklepu z aplikacjami dystrybucji

Niezależnie od tego, jak zamierzasz wersji aplikacji platformy Xamarin.iOS, musisz utworzyć *profil inicjowania obsługi administracyjnej dystrybucji* właściwe dla niego. Ten profil umożliwia aplikacji jest podpisany cyfrowo w wersji, dzięki czemu można zainstalować na urządzeniu z systemem iOS. Podobnie jak projektowanie, profil inicjowania obsługi administracyjnej, profil dystrybucji będzie zawierać następujące informacje:

- Identyfikator aplikacji
- Certyfikat dystrybucji

Można wybrać takie same **identyfikator aplikacji** i **urządzeń** używana na potrzeby programowania profil inicjowania obsługi administracyjnej, ale jeśli nie masz jeszcze jeden, należy utworzyć certyfikat dystrybucji do identyfikowania użytkownika organizacja podczas przesyłania aplikacji do sklepu z aplikacjami. W poniższej sekcji opisano kroki dotyczące sposobu tworzenia certyfikatu dystrybucji.

> [!NOTE]
> Tylko agentów zespołu i Administratorzy, można utworzyć dystrybucji certyfikatów oraz profile inicjowania obsługi.

<a name="creatingcertificate" />

## <a name="creating-a-distribution-certificate"></a>Tworzenie certyfikatu dystrybucji

1. Przejdź do *certyfikaty, identyfikatory & profile* sekcji Apple Developer Member Center.
2. W obszarze *certyfikaty*, wybierz pozycję **produkcji**.
3. Kliknij przycisk  **+**  przycisk, aby utworzyć nowy certyfikat.
4. W obszarze *produkcji* nagłówek, wybierz **sklepu z aplikacjami i Ad Hoc**:

    [![](images/createcertmanually01.png "Wybierz sklepu z aplikacjami i Ad Hoc")](images/createcertmanually01.png#lightbox)
5. Kliknij przycisk **Kontynuuj**, a następnie postępuj zgodnie z instrukcjami, aby utworzyć żądanie podpisania certyfikatu za pośrednictwem dostępu łańcucha kluczy:

    [![](images/createcertmanually02.png "Utwórz certyfikat podpisywania żądania za pośrednictwem dostępu łańcucha kluczy")](images/createcertmanually02.png#lightbox)
6. Po utworzeniu obsługi zgodnie z instrukcjami, kliknij przycisk **Kontynuuj**i przekazać do witryny Member Center obsługi:

    [![](images/createcertmanually03.png "Przekaż do witryny Member Center obsługi klienta")](images/createcertmanually03.png#lightbox)

7. Kliknij przycisk **Generuj** można utworzyć certyfikatu.
8. Na koniec **Pobierz** gotowy certyfikat i kliknij dwukrotnie plik go zainstalować.
9. W tym momencie certyfikatu należy zainstalować na komputerze, ale może być konieczne [Odśwież profile](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download), aby upewnić się, że są one widoczne w programie Xcode.

Alternatywnie jest możliwość żądania certyfikatu za pomocą okna dialogowego preferencji w środowisku Xcode. Aby to zrobić, wykonaj następujące czynności:

1.   Wybierz zespół, a następnie kliknij przycisk **zarządzanie certyfikatami...** : [ ![ ] (images/selectteam.png "Wybierz zespołu i Wyświetl szczegóły")](images/selectteam.png#lightbox)

2.   Następnie kliknij przycisk **Utwórz** znajdujący się obok **dystrybucji certyfikat iOS**: [ ![ ] (images/selectcert.png "Tworzenie dystrybucji certyfikatu systemu iOS")](images/selectcert.png#lightbox)

3.   W zależności od swoich uprawnień zespołu tożsamości podpisywania zostanie wygenerowany, jak pokazano poniżej, lub należy czekać do momentu agenta zespołu lub administratora zatwierdza ją: [ ![ ] (images/generated.png "tożsamości podpisywania zostanie wygenerowany i wyświetlane okno dialogowe")](images/generated.png#lightbox)


<a name="creatingprofile" />

## <a name="creating-a-distribution-profile"></a>Tworzenie profilu dystrybucji

<a name="creatingappid" />

### <a name="creating-an-app-id"></a>Tworzenie Identyfikatora aplikacji

Jako wszystkie inne inicjowania obsługi profilu możesz utworzyć identyfikator aplikacji jest wymagane do identyfikowania aplikacji, które przekazujesz do urządzenia użytkownika. Jeśli to nie zostało już utworzone, wykonaj poniższe kroki, aby utworzyć:


1. W [Centrum deweloperów firmy Apple](https://developer.apple.com/account/overview.action) przejdź do *certyfikatu, identyfikatory i profile* sekcji. Wybierz **identyfikatorów aplikacji** w obszarze **identyfikatory**.
2. Kliknij przycisk  **+**  przycisk i podaj **nazwa** której znajdą w portalu.
3. Prefiks aplikacji powinna być już ustawiona jako Identyfikatora zespołu i nie można zmienić. Wybierz jawne lub identyfikator aplikacji symboli wieloznacznych i wprowadź identyfikator pakietu w formacie wstecznego DNS, takich jak:
    - **Jawne**: modelu com. [nazwa_domeny]. [ Nazwa aplikacji]
    - **Wildcard**:com.[DomainName].*
4. Wybierz dowolne [usługi aplikacji](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) aplikację wymagającej.
5. Kliknij przycisk **Kontynuuj** przycisk i zgodnie z instrukcjami wyświetlanymi na ekranie można utworzyć nowego identyfikatora aplikacji.


### <a name="creating-a-provisioning-profile"></a>Tworzenie profilu inicjowania obsługi administracyjnej

Po utworzeniu wymagane składniki potrzebne do tworzenia profilu dystrybucji, wykonaj poniższe kroki, aby go utworzyć:

1. Powróć do portalu inicjowania obsługi administracyjnej firmy Apple i wybierz **inicjowania obsługi administracyjnej** > **dystrybucji**:

    [![](images/distribute01.png "Inicjowanie obsługi administracyjnej RSelect > dystrybucji")](images/distribute01.png#lightbox)

2. Kliknij przycisk  **+**  przycisk i wybierz typ profilu dystrybucji, który ma zostać utworzony jako **sklepu z aplikacjami**:

    [![](images/distribute02.png "Utwórz profil dystrybucji sklepu z aplikacjami")](images/distribute02.png#lightbox)

3. Kliknij przycisk **Kontynuuj** i wybrać z listy rozwijanej, która ma zostać utworzony profil dystrybucji dla Identyfikatora aplikacji:

    [![](images/distribute03.png "Wybierz z listy rozwijanej identyfikator aplikacji")](images/distribute03.png#lightbox)

4. Kliknij przycisk **Kontynuuj** i wybrać certyfikat wymagany do podpisywania aplikacji:

    [![](images/distribute04.png "Wybierz certyfikat wymagany do podpisywania aplikacji")](images/distribute04.png#lightbox)

5. Kliknij przycisk **Kontynuuj** i wybrać urządzenia z systemem iOS, które aplikacji platformy Xamarin.iOS będzie mogła działać:

    [![](images/distribute05.png "Wybierz iOS urządzenia, aplikacja będzie mogła działać")](images/distribute05.png#lightbox)

6. Kliknij przycisk **Kontynuuj** przycisk, a następnie wprowadź **nazwa** nowego profilu dystrybucji:

    [![](images/distribute06.png "Wprowadź nazwę nowego profilu dystrybucji")](images/distribute06.png#lightbox)

7. Kliknij przycisk **Generuj** przycisk, aby utworzyć nowy profil i zakończenia procesu.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 Być może trzeba zamknąć program Visual Studio dla komputerów Mac i ma Xcode Odśwież swoją listę dostępnych tożsamości podpisywania i profile inicjowania obsługi (zgodnie z instrukcjami w [żąda tożsamości podpisywania](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) sekcji) przed nowy Profil dystrybucji jest dostępny w programie Visual Studio dla komputerów Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 Być może trzeba zamknąć program Visual Studio i ma Xcode (na hosta usługi kompilacji Mac), Odśwież listę dostępnych tożsamości podpisywania i profile inicjowania obsługi (zgodnie z instrukcjami w [żąda tożsamości podpisywania](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) sekcji) zanim nowy profil dystrybucji jest dostępna w programie Visual Studio.

-----

<a name="selectprofile" />

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Wybieranie profilu dystrybucji w projekcie platformy Xamarin.iOS

Gdy wszystko będzie gotowe do końcowego kompilacji aplikacji platformy Xamarin.iOS do sprzedaży w sklepie iTunes aplikację, wybierz profil dystrybucji, który został utworzony wcześniej.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 W programie Visual Studio dla komputerów Mac wykonaj następujące czynności:

1. Kliknij dwukrotnie nazwę projektu w **Eksploratora rozwiązań** go otworzyć do edycji.
2. Wybierz **iOS podpisywania pakietu** i **wersji | iPhone** z **konfiguracji** listy rozwijanej:

    ![](images/releasexs01.png "Wybierz wersji | urządzenia iPhone z listy rozwijanej konfiguracji")
3. W większości przypadków **tożsamość podpisywania** i **profilu inicjowania obsługi administracyjnej** może pozostać ich wartości domyślne **automatyczne** i wybrać poprawny Visual Studio dla komputerów Mac profil na podstawie identyfikatora pakietu w pliku Info.plist:

    ![](images/releasexs02.png "Tożsamość podpisywania i profilu inicjowania obsługi administracyjnej ustawione wartości domyślne automatyczny")
4. W razie potrzeby wybierz tożsamość podpisywania i profil dystrybucji (jeden utworzone powyżej) z listy rozwijane:

    ![](images/releasexs03.png "Wybierz tożsamość podpisywania i profile dystrybucji")
5. Kliknij przycisk **OK** przycisk, aby zapisać zmiany.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 W programie Visual Studio wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierz **właściwości** go otworzyć do edycji.
2. Wybierz **iOS podpisywania pakietu** i **wersji | iPhone** z **konfiguracji** listy rozwijanej:

    ![](images/releasevs01.png "Wybierz wersji | urządzenia iPhone z listy rozwijanej konfiguracji")
3. W większości przypadków **tożsamość podpisywania** i **profilu inicjowania obsługi administracyjnej** może pozostać ich wartości domyślne **automatyczne** i Visual Studio wybierze poprawnego profilu na podstawie identyfikatora pakietu w pliku Info.plist

    ![](images/releasevs02.png "Tożsamość podpisywania i profilu inicjowania obsługi administracyjnej ustawione wartości domyślne automatyczny")
4. W razie potrzeby wybierz tożsamość podpisywania i profil dystrybucji (jeden utworzone powyżej) z listy rozwijane:

    ![](images/releasevs03.png "Wybierz tożsamość podpisywania i profil dystrybucji")
5. Zapisać zmiany w oknie właściwości projektu.

-----

<a name="itunesconnect" />

## <a name="configuring-your-application-in-itunes-connect"></a>Konfigurowanie aplikacji w iTunes Connect

Gdy aplikacja jest pomyślnie zostały udostępnione, następnym krokiem jest skonfigurowanie aplikacji w [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa), która jest pakiet sieci web na podstawie narzędzi, między innymi, zarządzanie aplikacjami systemu iOS w sklepie z aplikacjami.

Aplikacja platformy Xamarin.iOS musi być prawidłowo skonfigurowane i w iTunes Connect przed może zostać przesłane do firmy Apple do przeglądu, a ostatecznie zwalniane sprzedaży lub jako bezpłatną aplikację w sklepie z aplikacjami.

Aby uzyskać więcej informacji, zobacz nasze [Konfigurowanie aplikacji w iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) dokumentacji.

<a name="submitting" />

## <a name="submitting-an-app-to-itunes-connect"></a>Przesyłanie aplikacji iTunes Connect

Po aplikacji jest podpisany przy użyciu profilu inicjowania obsługi administracyjnej dystrybucji i utworzeniu aplikacji w iTunes Connect, plik binarny aplikacji jest przekazywane do firmy Apple do przeglądu. Po pomyślnym przeglądu przez firmę Apple staje się dostępna w sklepie z aplikacjami.

Aby uzyskać więcej informacji na temat publikowania aplikacji do sklepu z aplikacjami, zobacz [publikowania do sklepu z aplikacjami](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

<a name="windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Automatycznie skopiować .app pakietów systemu Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="summary"></a>Podsumowanie

W tym artykule opisano najważniejsze składniki w celu przygotowania aplikacji platformy Xamarin.iOS do dystrybucji, w sklepie z aplikacjami.

## <a name="related-links"></a>Linki pokrewne

- [Konfigurowanie aplikacji w iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publikowanie w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Dystrybucji wewnętrznych](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Dystrybucja Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Plik iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Obsługa IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
