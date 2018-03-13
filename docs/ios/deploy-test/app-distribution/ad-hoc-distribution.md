---
title: Dystrybucji Ad Hoc
description: "Ten dokument zawiera omówienie techniki dystrybucji Ad Hoc, które są używane przede wszystkim do testowania aplikacji platformy Xamarin.iOS przy użyciu szerokiej grupy osób."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3B621CAD-103C-478A-97C3-829015F48D1A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 3d01130989336ada855e936a6597b517fab5ee69
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="ad-hoc-distribution"></a>Dystrybucji Ad Hoc

_Ten dokument zawiera omówienie techniki dystrybucji Ad Hoc, które są używane przede wszystkim do testowania aplikacji platformy Xamarin.iOS przy użyciu szerokiej grupy osób._

Aplikacji platformy Xamarin.iOS został opracowany, następnym krokiem w cyklu tworzenia oprogramowania po dystrybucję aplikacji dla użytkowników do testowania.

iTunes Connect jest jedną z opcji zarządzania testowania aplikacji i jest opisany bardziej w [TestFlight](~/ios/deploy-test/testflight.md) przewodnik. Jednak członkami Apple Developer Enterprise Program nie mają dostępu do programu iTunes połączyć, dlatego *Ad Hoc* dystrybucji jest najlepszą metodą testowania tych aplikacji.

Aplikacje platformy Xamarin.iOS mogą być testowane użytkownika za pośrednictwem *ad hoc* dystrybucji, który jest dostępny zarówno w programie dla deweloperów firmy Apple, jak i Apple Developer Enterprise Program i zezwala na maksymalnie 100 urządzeń z systemem iOS do sprawdzenia.

Ad hoc dystrybucji daje możliwość nie wymaga zatwierdzenia sklepu z aplikacjami i można ją zainstalować bezprzewodową z serwera sieci web lub za pomocą programu iTunes. Jednakże ograniczone do **100** urządzeń rocznie członkostwa rozwoju i dystrybucji, oraz te należy dodać ręcznie w Centrum użytkowników według ich UDID. Aby uzyskać więcej informacji na temat dodawania urządzeń, odwiedź stronę [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#adddevice) przewodnik.

Ad hoc dystrybucji wymaga, aby aplikacje były udostępniane za pomocą Ad Hoc *profil inicjowania obsługi administracyjnej* zawierającego kod podpisywania informacje, a także tożsamości aplikacji i urządzeń, które mogą zainstalować aplikację.

Ten przewodnik zawiera informacji na temat inicjowania obsługi administracyjnej dla dystrybucji Ad Hoc i informacje na temat sposobu dystrybucji aplikacji platformy Xamarin.iOS.

<a name="setup" />

## <a name="setting-up-for-distribution"></a>Ustawienia dla dystrybucji

Nawet wtedy, gdy planujesz wydanie aplikacji platformy Xamarin.iOS wewnętrznych wdrażania dla celów testowych, należy utworzyć Ad Hoc dystrybucji profil udostępniania właściwe dla niego. Ten profil temu aplikacja może być podpisane cyfrowo dla wersji, dzięki czemu można zainstalować na urządzeniu z systemem iOS.

Następnej sekcji opisano sposób uzyskać certyfikat dystrybucji i profil inicjowania obsługi administracyjnej dystrybucji.

> [!NOTE]
>  Uwaga: Tylko agentów zespołu i Administratorzy mogą tworzyć certyfikaty dystrybucji i profile inicjowania obsługi.

<a name="createcertificate" />

## <a name="create-a-distribution-certificate"></a>Utwórz certyfikat z dystrybucji


1. Przejdź do *certyfikaty, identyfikatory & profile* sekcji Apple Developer Member Center.
2. W obszarze *certyfikaty*, wybierz pozycję **produkcji**.
3. Kliknij przycisk  **+**  przycisk, aby utworzyć nowy certyfikat.
4. W obszarze *produkcji* nagłówek, wybierz **wewnętrznych i Ad Hoc**, lub **sklepu z aplikacjami i Ad Hoc**w oparciu o członkostwo w programie:

  [![](ad-hoc-distribution-images/cert-first-small.png "Wybierz wewnętrznych i Ad Hoc lub sklepu z aplikacjami i Ad Hoc")](ad-hoc-distribution-images/cert-first-large.png#lightbox)

5. Kliknij przycisk Kontynuuj i postępuj zgodnie z instrukcjami, aby utworzyć żądanie podpisania certyfikatu za pośrednictwem dostępu łańcucha kluczy:

  [![](ad-hoc-distribution-images/createcertmanually02.png "Utwórz certyfikat podpisywania żądania za pośrednictwem dostępu łańcucha kluczy")](ad-hoc-distribution-images/createcertmanually02.png#lightbox)

6. Po utworzeniu obsługi zgodnie z instrukcjami, kliknij przycisk Kontynuuj i przekazać obsługi klienta do witryny Member Center:

  [![](ad-hoc-distribution-images/createcertmanually03.png "Przekaż do witryny Member Center obsługi klienta")](ad-hoc-distribution-images/createcertmanually03.png#lightbox)

7. Kliknij przycisk Wygeneruj, aby utworzyć certyfikat.
8. Na koniec Pobierz certyfikat ukończone i kliknij go dwukrotnie, aby go zainstalować.
9. W tym momencie certyfikatu należy zainstalować na komputerze, ale może być konieczne [Odśwież profile](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) aby upewnić się, że są one widoczne w programie Xcode.

Alternatywnie jest możliwość żądania certyfikatu za pomocą okna dialogowego preferencji w środowisku Xcode. Aby to zrobić, wykonaj następujące czynności:

1.   Wybierz zespół, a następnie kliknij przycisk **zarządzanie certyfikatami...** : [ ![ ] (ad-hoc-distribution-images/selectteam.png "Wybranie zespołu")](ad-hoc-distribution-images/selectteam.png#lightbox)

2.   Następnie kliknij przycisk **plus (+)** i wybrać **iOS App Store**: [ ![ ] (ad-hoc-distribution-images/selectcert.png "wybranie iOS App Store")](ad-hoc-distribution-images/selectcert.png#lightbox)

<a name="createprofile" />

## <a name="create-a-distribution-provisioning-profile"></a>Utwórz profil inicjowania obsługi administracyjnej dystrybucji

<a name="createappid" />

### <a name="create-an-app-id"></a>Utwórz identyfikator aplikacji
Jako wszystkie inne inicjowania obsługi profilu możesz utworzyć identyfikator aplikacji będą musieli zidentyfikować aplikację, która będzie dystrybuowana do urządzenia użytkownika. Jeśli to nie zostało już utworzone, wykonaj poniższe kroki, aby utworzyć:


1. W [Centrum deweloperów firmy Apple](https://developer.apple.com/account/overview.action) przejdź do *certyfikatu, identyfikatory i profile* sekcji. Wybierz **identyfikatorów aplikacji** w obszarze **identyfikatory**.
2. Kliknij przycisk  **+**  przycisk i podaj **nazwa** której znajdą w portalu.
3. Prefiks aplikacji powinna być już ustawiona jako Identyfikatora zespołu i nie można zmienić. Wybierz jawne lub identyfikator aplikacji symboli wieloznacznych i wprowadź identyfikator pakietu w formacie wstecznego DNS, takich jak:
    - **Jawne**: `com.[DomainName].[AppName]`
    - **Symbol wieloznaczny**: `com.[DomainName].*`
4. Wybierz dowolne [usługi aplikacji](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) aplikację wymagającej.
5. Kliknij przycisk **Kontynuuj** przycisk i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie można utworzyć nowego identyfikatora aplikacji.

Po utworzeniu wymagane składniki potrzebne do tworzenia profilu dystrybucji, wykonaj poniższe kroki, aby go utworzyć:

1. Powróć do portalu inicjowania obsługi administracyjnej firmy Apple i wybierz **inicjowania obsługi administracyjnej > dystrybucji**: [ ![ ] (ad-hoc-distribution-images/distribute01.png "wybierz udostępniania > dystrybucji")](ad-hoc-distribution-images/distribute01.png#lightbox)

2. Kliknij przycisk  **+**  przycisk i wybierz typ profilu dystrybucji, który ma zostać utworzony jako **Ad Hoc**:

    [![](ad-hoc-distribution-images/distribute02.png "Utwórz typ dystrybucji Ad Hoc")](ad-hoc-distribution-images/distribute02.png#lightbox)

3. Kliknij przycisk **Kontynuuj** i wybrać z listy rozwijanej, która ma zostać utworzony profil dystrybucji dla Identyfikatora aplikacji:

    [![](ad-hoc-distribution-images/distribute03.png "Wybierz z listy rozwijanej identyfikator aplikacji")](ad-hoc-distribution-images/distribute03.png#lightbox)

4. Kliknij przycisk **Kontynuuj** przycisk i wybierz opcję dystrybucji certyfikatu wymagane do podpisania aplikacji:

    [![](ad-hoc-distribution-images/distribute04.png "Wybierz opcję dystrybucji certyfikatu wymagane do podpisania aplikacji")](ad-hoc-distribution-images/distribute04.png#lightbox)

6. Kliknij przycisk **Kontynuuj** przycisk, a następnie wprowadź **nazwa** nowego profilu dystrybucji:

    [![](ad-hoc-distribution-images/distribute06.png "Wprowadź nazwę nowego profilu dystrybucji")](ad-hoc-distribution-images/distribute06.png#lightbox)

7. Kliknij przycisk **Generuj** przycisk, aby utworzyć nowy profil i zakończenia procesu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Być może trzeba zamknąć program Visual Studio dla komputerów Mac i ma Xcode Odśwież swoją listę dostępnych tożsamości podpisywania i profile inicjowania obsługi (zgodnie z instrukcjami w [pobieranie profilów i certyfikatów w programie Xcode](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) sekcji) zanim nowy profil dystrybucji jest dostępna w programie Visual Studio dla komputerów Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Być może trzeba zamknąć program Visual Studio i ma Xcode (na hosta usługi kompilacji Mac), Odśwież swoją listę dostępnych tożsamości podpisywania i profile inicjowania obsługi (zgodnie z instrukcjami w [pobieranie profilów i certyfikatów w programie Xcode](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)sekcji) zanim nowy profil dystrybucji jest dostępna w programie Visual Studio.

-----

<a name="selectprofile" />

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Wybieranie profilu dystrybucji w projekcie platformy Xamarin.iOS

Gdy wszystko będzie gotowe do końcowego kompilacji aplikacji platformy Xamarin.iOS, wybierz profil dystrybucji, który został utworzony wcześniej.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 W programie Visual Studio dla komputerów Mac wykonaj następujące czynności:

1. Kliknij dwukrotnie nazwę projektu w **Eksploratora rozwiązań** go otworzyć do edycji.
2. Wybierz **iOS podpisywania pakietu** i typ kompilacji z **konfiguracji** listy rozwijanej:

    ![](ad-hoc-distribution-images/releasexs01.png "Wybierz typ kompilacji z listy rozwijanej konfiguracji")
3. W większości przypadków **tożsamość podpisywania** i **profilu inicjowania obsługi administracyjnej** może pozostać wartościami domyślnymi **automatyczne** i wybrać poprawny Visual Studio dla komputerów Mac profil na podstawie identyfikatora pakietu w pliku Info.plist:

    ![](ad-hoc-distribution-images/releasexs02.png "Tożsamość podpisywania i profilu inicjowania obsługi administracyjnej ustawione wartości domyślne automatyczny")
4. W razie potrzeby wybierz tożsamość podpisywania i profil dystrybucji (jeden utworzone powyżej) z listy rozwijane:

    ![](ad-hoc-distribution-images/releasexs03.png "Wybierz tożsamość podpisywania i profil dystrybucji")
5. Kliknij przycisk **OK** przycisk, aby zapisać zmiany.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
 W programie Visual Studio wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierz **właściwości** go otworzyć do edycji.
2. Wybierz **iOS podpisywania pakietu** i typ kompilacji z **konfiguracji** listy rozwijanej:

    ![](ad-hoc-distribution-images/releasevs01.png "Wybierz typ kompilacji z listy rozwijanej konfiguracji")
3. W większości przypadków **tożsamość podpisywania** i **profilu inicjowania obsługi administracyjnej** może pozostać wartościami domyślnymi **automatyczne** i Visual Studio wybierze poprawnego profilu oparte na identyfikator pakietu w pliku Info.plist:

    ![](ad-hoc-distribution-images/releasevs02.png "Tożsamość podpisywania i profilu inicjowania obsługi administracyjnej ustawione wartości domyślne automatyczny")
4. W razie potrzeby wybierz tożsamość podpisywania i profil dystrybucji (jeden utworzone powyżej) z listy rozwijane:

    ![](ad-hoc-distribution-images/releasevs03.png "Wybierz tożsamość podpisywania i profil dystrybucji")
5. Zapisać zmiany w oknie właściwości projektu.

-----

<a name="adhoc" />

## <a name="ad-hoc-distribution"></a>Ad Hoc dystrybucji

Gdy [TestFlight](~/ios/deploy-test/testflight.md) testuje popularnych środków w wersji beta i dystrybucji, jest częścią iTunes Connect i w związku z tym jest niedostępny do elementów członkowskich **Apple Developer Enterprise Program**.

Ad Hoc dystrybucji umożliwia deweloperom beta aplikacje testu przy użyciu różnych urządzeń, łącząc iTunes nie jest opcją. Ad Hoc działa w sposób podobny do dystrybucji wewnętrznych i wymaga IPA ma zostać utworzony, który można dystrybuować bezprzewodową, albo ręcznie za pomocą programu iTunes.

<a name="IPA_Creation" />

### <a name="ipa-support-for-ad-hoc-deployment"></a>Obsługa wdrażania usługi Ad Hoc IPA

Po zainicjowaniu obsługi, aplikacje można spakować do pliku, znany jako *IPA*. Jest to plik zip, który zawiera aplikację, a także dodatkowe metadane i ikon. IPA umożliwia dodawanie aplikacji lokalnie do programu iTunes, dzięki czemu można synchronizować bezpośrednio do urządzenia, który znajduje się w profilu inicjowania obsługi administracyjnej.

Aby uzyskać więcej informacji na temat tworzenia IPA, zobacz [Obsługa IPA](~/ios/deploy-test/app-distribution/ipa-support.md) przewodnik.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono mechanizmów dystrybucji Ad Hoc, które są wymagane do testowania aplikacji platformy Xamarin.iOS.


## <a name="related-links"></a>Linki pokrewne

- [Dystrybucja w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Dystrybucji wewnętrznych](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Plik iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Obsługa IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
