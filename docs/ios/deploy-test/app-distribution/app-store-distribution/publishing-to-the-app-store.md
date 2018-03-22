---
title: Publikowanie do sklepu z aplikacjami
description: "W tym artykule przedstawiono sposób konfigurowania, tworzenie i publikowanie aplikacji platformy Xamarin.iOS dystrybucji za pośrednictwem sklepu z aplikacjami. Zawiera przewodnik krok po kroku, który obejmuje sposobu przygotowania aplikacji do dystrybucji, jak za pomocą narzędzi firmy Apple do przesyłania aplikacji do przeglądu i, finally, jak opublikować aplikację do sklepu z aplikacjami."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: ef8fafb923dcad936ce0a049e715cdd163ea7222
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="publishing-to-the-app-store"></a>Publikowanie do sklepu z aplikacjami

_W tym artykule przedstawiono sposób konfigurowania, tworzenie i publikowanie aplikacji platformy Xamarin.iOS dystrybucji za pośrednictwem sklepu z aplikacjami. Zawiera przewodnik krok po kroku, który obejmuje sposobu przygotowania aplikacji do dystrybucji, jak za pomocą narzędzi firmy Apple do przesyłania aplikacji do przeglądu i, finally, jak opublikować aplikację do sklepu z aplikacjami._

W kolejności dystrybuować aplikacje na wszystkich urządzeniach z systemem iOS, Firma Apple wymaga aplikacji, które mają być opublikowane za pośrednictwem *sklepu z aplikacjami*, co pojedynczej lokalizacji zakupów w aplikacjach systemu iOS ze sklepu App Store. Z ponad 500 000 aplikacji w sklepie deweloperzy wiele typów aplikacji kapitalizacji na ogromną Powodzenie ten pojedynczy punkt dystrybucji. Sklep App Store to gotowe rozwiązanie, oferty deweloperzy aplikacji systemów dystrybucji i płatności.

Proces przesyłania aplikacji do sklepu z aplikacjami obejmuje:

1. Tworzenie **identyfikator aplikacji** i wybierając **uprawnień**.
2. Tworzenie **dystrybucji profil inicjowania obsługi administracyjnej**.
3. Za pomocą tego profilu, aby skompilować aplikację.
4. Przesyłanie aplikacji za pośrednictwem **iTunes Connect**.


W tym artykule opisano wszystkie kroki niezbędne do udostępnienia, tworzenie i przesyłanie aplikacji do sklepu z aplikacjami dystrybucji.

## <a name="before-you-submit-an-application"></a>Przed wysłaniem aplikacji

Po przesłaniu aplikacji dla publikacji do sklepu z aplikacjami przechodzi ona przez proces oceny przez firmę Apple, aby mieć pewność, że spełnia on wymagania firmy Apple wytyczne dotyczące jakości i zawartości. Jeśli aplikacja nie spełniają te wytyczne, Apple odrzuci go po tym czasie konieczne będzie adresów — niezgodność odnosiło się przez firmę Apple, a następnie prześlij ponownie.
W związku z tym należy pozostawić najlepszy sposób sprawia, że za pośrednictwem firmy Apple, przejrzyj zapoznawanie się z tymi wytycznymi, a próby dostosowanie aplikacji do nich. Wytycznymi firmy Apple są dostępne pod adresem: [wytyczne przeglądu sklepu aplikacji](https://developer.apple.com/appstore/resources/approval/guidelines.html).

Jest kilka rzeczy, które należy obserwować podczas przesyłania aplikacji:

1. Upewnij się, że opis aplikacji jest zgodny funkcjonalności aplikacji.
2. Test, który aplikacja nie awarii w obszarze normalnego użycia. W tym użycie na każdym urządzeniu z systemem iOS, która jest obsługiwana.


Apple zachowuje również listę porady przesyłanie sklepu z aplikacjami. Można znaleźć w [dystrybucji ze sklepu App store](https://developer.apple.com/appstore/resources/submission/tips.html).

## <a name="configuring-your-application-in-itunes-connect"></a>Konfigurowanie aplikacji w iTunes Connect

[Połącz iTunes](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) to pakiet narzędzi sieci web, między innymi, zarządzanie aplikacji systemu iOS ze sklepu App Store. Aplikacja platformy Xamarin.iOS musi być prawidłowo skonfigurowane i w iTunes Connect przed może zostać przesłane do firmy Apple do przeglądu, a ostatecznie zwalniane sprzedaży lub jako bezpłatną aplikację w sklepie z aplikacjami.

Wykonaj następujące czynności:

1. Sprawdź, czy odpowiednie umowy w miejscu i aktualności w **umów podatku i bankowości** sekcji iTunes Połącz z wersji aplikacji systemu iOS bezpłatnie lub do sprzedaży.
2. Utwórz nową **iTunes rekordu Connect** dla aplikacji i określ jej **Nazwa wyświetlana** (taki jak pokazano w sklepie App Store).
3. Wybierz **cena sprzedaży** lub określ bezpłatnie wydane aplikacji.
5. Podaj wyczyść zwięzły **opis** aplikacji, łącznie z jej funkcje i korzyści dla użytkownika końcowego.
6. Podaj **kategorii**, **pod kategorii**, i **słowa kluczowe** pomóc użytkownikowi postrzegać twoją aplikację w sklepie z aplikacjami.
7. Podaj **skontaktuj się z** i **Obsługa** adresów URL do witryny sieci Web wymagane przez firmę Apple.
8. Ustaw aplikacji **klasyfikacji**, który jest używany przez kontroli rodzicielskiej ze sklepu App Store.
9. Skonfigurować opcjonalne technologii sklepu z aplikacjami, takie jak **Game Center** i **zakupu w aplikacji**.

Aby uzyskać więcej informacji, zobacz nasze [Konfigurowanie aplikacji w iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) dokumentacji.

## <a name="preparing-for-app-store-distribution"></a>Przygotowywanie do dystrybucji sklepu z aplikacjami

Aby opublikować aplikację do sklepu z aplikacjami, należy najpierw skompiluj go do dystrybucji, który obejmuje wiele kroków. Poniższe sekcje obejmuje wszystko, co jest wymagane w celu przygotowania aplikacji platformy Xamarin.iOS dla publikacji tak, aby mogą być tworzone i przesłać je do sklepu z aplikacjami dla przeglądu i wersji.

### <a name="provisioning-for-application-services"></a>Inicjowanie obsługi administracyjnej dla usług aplikacji

Apple zawiera specjalne usług aplikacji, nazywane również uprawnienia, które można aktywować aplikacji systemu iOS, podczas tworzenia Unikatowy identyfikator dla tego zaznaczenia. Czy korzystasz z uprawnień niestandardowych lub nie, nadal musisz utworzyć unikatowy identyfikator dla aplikacji platformy Xamarin.iOS, zanim będzie można opublikować w sklepie z aplikacjami.

Tworzenie Identyfikatora aplikacji i opcjonalnie wybranie uprawnień obejmuje następujące czynności przy użyciu firmy Apple iOS opartych na sieci web portalu inicjowania obsługi:

1. W **certyfikaty, identyfikatory & profile** sekcji wybierz **identyfikatory** > **identyfikator aplikacji**.
2. Kliknij przycisk  **+**  przycisk i podaj **nazwa** i **identyfikator pakietu** nowej aplikacji.
3. Przewiń do dołu ekranu i wybrać dowolną **usługi aplikacji** które będzie wymagane przez aplikację platformy Xamarin.iOS.
4. Kliknij przycisk **Kontynuuj** przycisk i następujących wyświetlanymi instrukcjami, aby utworzyć nowy identyfikator aplikacji.

Oprócz wybierania i konfigurowania wymaganych usług aplikacji, podczas definiowania Identyfikatora aplikacji, również należy skonfigurować identyfikator aplikacji i uprawnień w projekcie platformy Xamarin.iOS edytując zarówno `Info.plist` i `Entitlements.plist` plików.

Wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Info.plist` plik, aby otworzyć do edycji.
2. W **iOS aplikacji docelowej** , wprowadź nazwę aplikacji i wprowadź **identyfikator pakietu** utworzony podczas definiowania identyfikator aplikacji.
3. Zapisać zmiany w `Info.plist` pliku.
4. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Entitlements.plist` plik, aby otworzyć do edycji.
5. Wybierz i skonfiguruj uprawnień wymaganej aplikacji platformy Xamarin.iOS, aby odpowiadały instalacji wykonywane powyżej, gdy zdefiniowane identyfikator aplikacji.
6. Zapisać zmiany w `Entitlements.plist` pliku.

Aby uzyskać szczegółowe instrukcje, zobacz nasze [inicjowania obsługi administracyjnej dla usług aplikacji](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) dokumentacji.

### <a name="setting-the-store-icons"></a>Ustawienie ikon magazynu

Ikony sklepu z aplikacjami powinny teraz być dostarczona przez katalogu zasobów. Aby dodać ikony sklepu z aplikacjami, w pierwszej kolejności zlokalizować **AppIcon** obraz ustawiony **Assets.xcassets** pliku projektu.

Ikona wymagany w katalogu zasobów o nazwie **sklepu z aplikacjami** i powinna być **1024 x 1024** rozmiar. Apple ma już wspomniano, czy ikona magazynu aplikacji w katalogu zasobów nie może być przezroczysty, ani zawierać kanału alfa.

Aby uzyskać informacje na temat ustawiania ikony magazynu odwoływać się do [ikonę przechowywania aplikacji](~/ios/app-fundamentals/images-icons/app-store-icon.md) przewodnik.

### <a name="setting-the-apps-icons-and-launch-screens"></a>Ustawienie aplikacji ikon i ekranów uruchamiania

Dla aplikacji systemu iOS, aby akceptowane przez firmę Apple do włączenia w sklepie z aplikacjami wymaga prawidłowego ikon i uruchamiania ekrany dla wszystkich systemu IOS, urządzenia, które będą działały na. Ikony aplikacji są dodawane do projektów w katalogu zasobów za pomocą **AppIcon** obraz ustawiony **Assets.xcassets** pliku. Ekrany uruchamiania są dodawane do scenorysu.

Aby uzyskać szczegółowe instrukcje dotyczące tworzenia aplikacji ikon i ekranów uruchamiania, zobacz [ikona aplikacji](~/ios/app-fundamentals/images-icons/app-icons.md) i [uruchamianie ekrany](~/ios/app-fundamentals/images-icons/launch-screens.md) przewodników.

### <a name="creating-and-installing-a-distribution-profile"></a>Tworzenie i instalowanie profilu dystrybucji

iOS używa *profile aprowizacji* do kontrolowania wdrażanie kompilacji określonej aplikacji. Są to pliki, które zawierają informacje o certyfikacie używanym do podpisania aplikacji, *identyfikator aplikacji*, i zainstalowaną aplikację. Do tworzenia i dystrybucji ad-hoc profilu inicjowania obsługi administracyjnej zawiera listę dozwolonych urządzeń, do których można wdrożyć aplikację. Jednak dystrybucji sklepu z aplikacjami, tylko informacje identyfikator certyfikatu i aplikacji są uwzględnione, ponieważ jest tylko mechanizm dystrybucji publicznych za pośrednictwem sklepu z aplikacjami.

Udostępnianie obejmuje następujące czynności przy użyciu firmy Apple iOS opartego na sieci web portalu inicjowania obsługi:

1.  Wybierz **inicjowania obsługi administracyjnej** > **dystrybucji**.
2.  Kliknij przycisk  **+**  przycisk i wybierz typ profilu dystrybucji, który ma zostać utworzony jako **sklepu z aplikacjami**.
3.  Wybierz **identyfikator aplikacji** z listy rozwijanej, który chcesz utworzyć profil dystrybucji.
4.  Wybierz prawidłowy certyfikat produkcyjny (dystrybucja) do podpisania aplikacji.
5.  Wprowadź **nazwa** nowego **profil dystrybucji** i generować profilu.
6.  Dla komputerów Mac, otwórz xcode i przejdź do **Preferencje > [wybierz identyfikator Apple ID] > Wyświetl szczegóły...** . Pobierz wszystkie dostępne profile w środowisku Xcode na komputerach Mac.
7.  Zwracany do środowiska IDE, a w obszarze **iOS podpisywania pakietu** opcji wybierz profil inicjowania obsługi administracyjnej dystrybucji do prawidłowego _kompilacji konfiguracji_ (będzie **sklepu z aplikacjami**lub **wersji**).

Aby uzyskać szczegółowe instrukcje, zobacz [tworzenia profilu dystrybucji](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) i [wybór profilu dystrybucji w projekcie platformy Xamarin.iOS](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile).

## <a name="setting-the-build-configuration-for-your-application"></a>Ustawienie konfiguracji kompilacji dla aplikacji

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy **Nazwa projektu** w **konsoli rozwiązania** i wybieranie **opcji** aby je otworzyć do edycji.
2. Wybierz **kompilacji systemu iOS** i wybierz **wersji | iPhone** z **konfiguracji** listy rozwijanej:

    ![](publishing-to-the-app-store-images/configurevs01.png "Wybierz z listy rozwijanej konfiguracji sklepu z aplikacjami")

3. Jeśli masz ma być przeznaczona dla wersji platformy systemów iOS, możesz wybrać je z **zestawu SDK w wersji** listy rozwijanej else Pozostaw domyślne ustawienie tej wartości.
4. Łączenie zmniejszyć całkowity rozmiar Twojej aplikacji dystrybucyjnego przez usuwanie limit nieużywane klasy metod, właściwości, itd. i w większości przypadków należy pozostawić wartość domyślna wynosząca **tylko zestawy SDK łącze**. W niektórych sytuacjach, na przykład podczas przy użyciu określonych niektórych 3 bibliotek firm, może można wymusić ta wartość **nie zawierają łącza** aby zapobiec wymagane elementy zostaną usunięte. Aby uzyskać więcej informacji, zapoznaj się [mechanika kompilacji systemu iOS](~/ios/deploy-test/ios-build-mechanics.md) przewodnik.
5. **Pliki obrazów PNG Optymalizuj dla systemu iOS** powinno być zaznaczone pole wyboru, ponieważ zapewni to dodatkowe zmniejszenie rozmiaru elementu dostarczanego aplikacji.
6. Debugowanie powinno _nie_ być włączone, ponieważ spowoduje to, że kompilacji niepotrzebnie.
8. Dla systemu iOS 11, musisz wybrać jedną z architektur urządzenia, które obsługuje **ARM64**. Aby uzyskać więcej informacji na temat tworzenia dla urządzeń z systemem iOS 64-bitowym, zobacz **Włączanie 64 bitowej kompilacje dla aplikacji platformy Xamarin.iOS** sekcji [zagadnień dotyczących platformy 32/x 64](~/cross-platform/macios/32-and-64/index.md) dokumentacji.
9. Możesz opcjonalnie użyć **LLVM** kompilatora, co powoduje szybsze i mniejsze kod, jednak będzie trwało dłużej do skompilowania.
10. Oparte na potrzeby aplikacji, możesz również dostosować typ **wyrzucanie elementów bezużytecznych** używane i ustawienia **internacjonalizacji**.
11. Zapisz zmiany w konfiguracji kompilacji.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Domyślnie podczas tworzenia nowej aplikacji platformy Xamarin.iOS w programie Visual Studio _konfiguracje kompilacji_ są tworzone automatycznie dla obu **Ad Hoc** i **sklepu z aplikacjami** wdrożenia. Przed wykonaniem końcowego kompilacji aplikacji, która będzie można przesyłania należy do firmy Apple, istnieje kilka zmianami, które należy wprowadzić w konfiguracji podstawowej.

Wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy **Nazwa projektu** w **Eksploratora rozwiązań** i wybieranie **właściwości** aby je otworzyć do edycji.
2. Wybierz **kompilacji systemu iOS** i **sklepu AppStore** (lub **wersji** Jeśli sklepu z aplikacjami nie jest dostępna) z **konfiguracji** listy rozwijanej:

    ![](publishing-to-the-app-store-images/configurevs01.png "Wybierz z listy rozwijanej konfiguracji sklepu z aplikacjami")

3. Jeśli masz ma być przeznaczona dla wersji platformy systemów iOS, możesz wybrać je z **zestawu SDK w wersji** listy rozwijanej else Pozostaw domyślne ustawienie tej wartości.
4. Łączenie zmniejszyć całkowity rozmiar Twojej aplikacji dystrybucyjnego przez usuwanie limit nieużywane klasy metod, właściwości, itd. i w większości przypadków należy pozostawić wartość domyślna wynosząca **tylko zestawy SDK łącze**. W niektórych sytuacjach, na przykład podczas przy użyciu określonych niektórych 3 bibliotek firm, może można wymusić ta wartość **nie zawierają łącza** aby zapobiec wymagane elementy zostaną usunięte. Aby uzyskać więcej informacji, zapoznaj się [mechanika kompilacji systemu iOS](~/ios/deploy-test/ios-build-mechanics.md) przewodnik.
5. **Pliki obrazów PNG Optymalizuj dla systemu iOS** powinno być zaznaczone pole wyboru, ponieważ zapewni to dodatkowe zmniejszenie rozmiaru elementu dostarczanego aplikacji.
6. Debugowanie powinno _nie_ być włączone, ponieważ spowoduje to, że kompilacji niepotrzebnie.
7. Polecenie **zaawansowane** karty:

    ![](publishing-to-the-app-store-images/configurevs02.png "Karta Zaawansowane")

8. Jeśli aplikacja platformy Xamarin.iOS jest przeznaczona dla urządzeń z systemem iOS z systemem iOS 8 i 64 bitowych, należy wybrać jedną z architektur urządzenia, które obsługuje **ARM64**. Aby uzyskać więcej informacji na temat tworzenia dla urządzeń z systemem iOS 64-bitowym, zobacz **Włączanie 64 bitowej kompilacje dla aplikacji platformy Xamarin.iOS** sekcji [zagadnień dotyczących platformy 32/x 64](~/cross-platform/macios/32-and-64/index.md) dokumentacji.
9. Możesz opcjonalnie użyć **LLVM** kompilatora, co powoduje szybsze i mniejsze kod, jednak będzie trwało dłużej do skompilowania.
10. Oparte na potrzeby aplikacji, możesz również dostosować typ **wyrzucanie elementów bezużytecznych** używane i ustawienia **internacjonalizacji**.
11. Zapisz zmiany w konfiguracji kompilacji.

-----

## <a name="building-and-submitting-the-distributable"></a>Tworzenie i przesyłanie dystrybucyjnego

Z aplikacji platformy Xamarin.iOS prawidłowo skonfigurowane można teraz przystąpić do kompilacji końcowej dystrybucji, która będzie można przesyłania należy do firmy Apple do przeglądu i wersji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="build-your-archive"></a>Tworzenie archiwum

1. Wybierz **wersji | Urządzenie** konfiguracji w programie Visual Studio dla komputerów Mac:

    ![](publishing-to-the-app-store-images/buildxs01new.png "Wybierz wersji | Konfiguracja urządzenia")
1. Z **kompilacji** menu, wybierz opcję **archiwum publikowania**:

    ![](publishing-to-the-app-store-images/buildxs02new.png "Wybierz archiwum do publikowania")

1. Po utworzeniu archiwum **archiwa** zostanie wyświetlony widok:

    ![](publishing-to-the-app-store-images/buildxs03new.png "Zostanie wyświetlony widok archiwa")


> [!NOTE]
> Podczas stary _sklepu z aplikacjami_ i _Ad Hoc_ konfiguracje teraz zostały usunięte ze wszystkich programu Visual Studio for Mac szablonów projektów, może się okazać, że starsze projekty nadal zawierać te konfiguracje. Jeśli jest to możliwe, można nadal używać **sklepu | Urządzenie** konfiguracji w kroku 1 z listy powyżej.

### <a name="sign-and-distribute-your-app"></a>Zaloguj się i dystrybucja aplikacji

 Zawsze skompilować aplikację dla archiwum, zostanie on automatycznie otwarty **widoku archiwa**, wyświetlanie wszystkich projektów zarchiwizowane; pogrupowane według rozwiązania. Domyślnie ten widok przedstawia tylko bieżące, otwórz rozwiązanie. Aby wyświetlić wszystkie rozwiązania, których archiwów, kliknij na **Pokaż wszystkie archiwa** opcji.

 Zalecane jest aby archiwa wdrażanie u klientów (wdrożenia sklepu z aplikacjami lub Enterprise) były przechowywane, tak, aby informacje o debugowaniu, który jest generowany można symbolized w późniejszym terminie.

 Aby zarejestrować aplikację i przygotowania go do dystrybucji:


1. Wybierz **utworzyć i dystrybuować...** , ilustrowane poniżej:

    ![](publishing-to-the-app-store-images/buildxs04new.png "Wybierz znak i dystrybucja...")

1. Spowoduje to otwarcie Kreatora publikacji. Wybierz **sklepu z aplikacjami** kanału dystrybucji, aby utworzyć pakiet i otwórz moduł ładujący aplikacji:

    ![](publishing-to-the-app-store-images/distribute01.png "Moduł ładujący otwartych aplikacji")

1. Na ekranie profilu inicjowania obsługi administracyjnej Wybierz tożsamość podpisywania i odpowiadający profil inicjowania obsługi administracyjnej lub ponownie podpisać tożsamość:

    ![](publishing-to-the-app-store-images/distribute02.png "Wybierz tożsamość podpisywania i odpowiadający profil inicjowania obsługi administracyjnej")

1. Sprawdź szczegóły pakietu, a następnie kliknij przycisk **publikowania** zapisać Twoje `.ipa` pakietu:

    ![](publishing-to-the-app-store-images/distribute03.png "Sprawdź informacje szczegółowe dotyczące pakietu")

1. Raz z `.ipa` zostało zapisane, że aplikacja jest gotowa do przekazania do programu iTunes Połącz przez moduł ładujący aplikacji:

    ![](publishing-to-the-app-store-images/distribute04.png "Na ekranie publikacji zakończyło się pomyślnie.")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dodatek plug-in Xamarin dla Visual Studio nie obsługuje obecnie Archiwizacja przepływu pracy w przypadku publikowania aplikacji systemu iOS do sklepu z aplikacjami. W związku z tym masz przekazywania IPA utworzony za pośrednictwem **kompilacji Ad hoc IPA** polecenia, które jest opisane poniżej.


## <a name="build-an-ipa"></a>Tworzenie IPA

 W tej sekcji opisano tworzenie IPA podobny do przepływu pracy przy użyciu Ad Hoc lub dystrybucji przedsiębiorstwa. Jednak zostanie podpisana, przy użyciu sklepu z aplikacjami profil, który został utworzony wcześniej inicjowania obsługi administracyjnej.

 Wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu platformy Xamarin.iOS i wybierz **właściwości** aby je otworzyć do edycji:

    ![](publishing-to-the-app-store-images/imagevs01.png "Wybierz polecenie Właściwości")
1. Wybierz **iOS podpisywania pakietu** i zmień profilu inicjowania obsługi administracyjnej do sklepu z aplikacjami profil inicjowania obsługi administracyjnej:

    ![](publishing-to-the-app-store-images/ipa01.png "Wybierz iOS podpisywania pakietu i zmień profilu inicjowania obsługi administracyjnej do sklepu z aplikacjami profil inicjowania obsługi administracyjnej")
1. Wybierz **iOS opcje IPA** i wybierz **Ad Hoc** z **konfiguracji** listy rozwijanej (jeśli Ad Hoc nie jest wyświetlana, wybierz **wersji** Zamiast tego):

    ![](publishing-to-the-app-store-images/imagevs02.png "Wybierz z listy rozwijanej konfiguracji Ad Hoc")

1. Opcjonalnie można określić **nazwy pakietu** dla IPA, jeśli nie określono będzie mieć taką samą nazwę jak projektu platformy Xamarin.iOS.
1. Zapisz zmiany we właściwościach projektu.
1. Wybierz **Ad Hoc** z programu Visual Studio dla komputerów Mac **kompilacji konfiguracji** listy rozwijanej:

    ![](publishing-to-the-app-store-images/imagevs05.png "Wybierz z listy rozwijanej konfiguracji kompilacji Ad Hoc")
1. Skompiluj projekt, aby utworzyć pakiet IPA.
1. Wbudowane IPA `Bin`  >  _iOS Device_  >  `Ad Hoc` folderu:

    ![](publishing-to-the-app-store-images/imagevs06.png "IPA wyświetlany w Eksploratorze plików")

-----


## <a name="customizing-the-ipa-location"></a>Dostosowywanie lokalizacji IPA

Nowy **MSBuild** właściwości `IpaPackageDir` został dodany do ułatwiają dostosowywanie `.ipa` pliku lokalizacji wyjściowej. Jeśli `IpaPackageDir` jest ustawiona w lokalizacji niestandardowej, `.ipa` pliku zostaną umieszczone w tej lokalizacji, zamiast domyślnej podkatalogu oznaczony znacznikiem czasowym. Może to być przydatne, gdy tworzenie zautomatyzowanych kompilacje, które korzystają z określonej ścieżki do katalogu działał prawidłowo, takich jak używany dla kompilacji ciągłej integracji (CI).

Istnieje kilka sposobów możliwych do użycia nowej właściwości:

Na przykład, aby dane wyjściowe `.ipa` pliku starego domyślny katalog (tak jak Xamarin.iOS 9.6 i niżej), można ustawić `IpaPackageDir` właściwości `$(OutputPath)` przy użyciu jednej z poniższych metod. W obu przypadkach efekt są zgodne z wszystkich kompilacjach Xamarin.iOS interfejsu API Unified kompilacje IDE w tym również używających kompilacji wiersza polecenia `xbuild`, `msbuild`, lub `mdtool`:

  - Pierwsza opcja jest skonfigurowanie `IpaPackageDir` właściwości w `<PropertyGroup>` element **MSBuild** pliku. Na przykład można dodać następujące `<PropertyGroup>` do dołu projekt aplikacji systemu iOS `.csproj` pliku (tylko przed tagiem zamykającym `</Project>` tagu):

      ```xml
        <PropertyGroup>
            <IpaPackageDir>$(OutputPath)</IpaPackageDir>
        </PropertyGroup>
      ```
  - Lepszym rozwiązaniem jest dodanie `<IpaPackageDir>` element w dół istniejącej `<PropertyGroup>` odpowiadający konfiguracji, używany do tworzenia `.ipa` pliku. Jest to lepszą, ponieważ przygotuje projektu dla zgodności w przyszłości planowane ustawienia na stronie właściwości projektu iOS IPA opcje. Jeśli obecnie używasz `Release|iPhone` Konfiguracja kompilacji `.ipa` pliku, grupa pełną zaktualizowane właściwości mogą wyglądać podobnie do następujących:

      ```xml
      <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' )
        <Optimize>true</Optimize>
        <OutputPath>bin\iPhone\Release</OutputPath>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <ConsolePause>false</ConsolePause>
        <CodesignKey>iPhone Developer</CodesignKey>
        <MtouchUseSGen>true</MtouchUseSGen>
        <MtouchUseRefCounting>true</MtouchUseRefCounting>
        <MtouchFloat32>true</MtouchFloat32>
        <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
        <MtouchLink>SdkOnly</MtouchLink>
        <MtouchArch>;ARMv7, ARM64</MtouchArch>
        <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
        <MtouchTlsProvider>Default</MtouchTlsProvider>
        <PlatformTarget>x86&</PlatformTarget>
        <BuildIpa>true</BuildIpa>
        <IpaPackageDir>$(OutputPath</IpaPackageDir>
      </PropertyGroup>
      ```
Alternatywne technika `msbuild` lub `xbuild` kompilacji wiersza polecenia jest dodanie `/p:` argument wiersza polecenia, aby ustawić `IpaPackageDir` właściwości. W takim przypadku należy pamiętać, że `msbuild` nie zwiększa `$()` wyrażenia przekazano w wierszu polecenia, więc nie można użyć `$(OutputPath)` składni. Zamiast tego Podaj pełną nazwę ścieżki. W mono `xbuild` polecenie Rozwiń `$()` wyrażenia, ale jest nadal lepiej używać pełnej nazwy ścieżki, ponieważ `xbuild` po pewnym czasie zostaną wycofane uzyskać [wersji i platform `msbuild` ](http://www.mono-project.com/docs/about-mono/releases/4.4.0/#msbuild-preview-for-os-x)w przyszłych wersjach. Pełny przykład, który korzysta z tej metody może wyglądać podobnie jak poniżej w systemie Windows:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Lub następujące dla komputerów Mac:

```bash
xbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

Z dystrybucją kompilacja utworzona i archiwizowane, możesz teraz przystąpić do przesyłania aplikacji do iTunes Connect.

### <a name="automatically-copy-app-bundles-back-to-windows"></a>Automatycznie skopiować .app pakietów systemu Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="submitting-your-app-to-apple"></a>Przesyłanie aplikacji do firmy Apple

> [!NOTE]
> Ostatnio zmieniła się z jego proces weryfikacji w aplikacjach systemu iOS firmy Apple i może odrzucić aplikacji za pomocą `iTunesMetadata.plist` objęte IPA. Jeśli wystąpi błąd `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`rozwiązanie opisane [tutaj](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1) powinno rozwiązać problem.

Ukończono budowanie dystrybucji można przystąpić do przesyłania aplikacji systemu iOS do firmy Apple w celu przeglądu i wersję ze sklepu App Store.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 Wykonaj następujące czynności:

1. Uruchom **Xcode**.
2. Z **okna** menu wybierz opcję **organizatora**.
3. Polecenie **archiwa** i wybierz utworzony wcześniej archiwum:

    ![](publishing-to-the-app-store-images/publishxs01.png "Wybierz archiwum do przesyłania")
4. Polecenie **sprawdzania poprawności...**  przycisku.
5. Wybierz konto do weryfikacji, a następnie kliknij przycisk **wybierz** przycisk:

    ![](publishing-to-the-app-store-images/publishxs02.png "Wybierz konto do weryfikacji")
6. Kliknij przycisk **weryfikacji** przycisk:

    ![](publishing-to-the-app-store-images/publishxs03.png "Okno dialogowe podsumowania pliku")
7. Jeśli wystąpiły problemy z pakietu, zostaną one wyświetlone.
8. Rozwiąż wszelkie problemy i skompiluj ponownie archiwum w programie Visual Studio dla komputerów Mac.
9. Polecenie **przesyłania...**  przycisku.
10. Wybierz konto do weryfikacji, a następnie kliknij przycisk **wybierz** przycisk:

    ![](publishing-to-the-app-store-images/publishxs04.png "Wybierz konto do weryfikacji")
11. Kliknij przycisk **przesyłania** przycisk:

    ![](publishing-to-the-app-store-images/publishxs05.png "Okno dialogowe podsumowania pliku")
12. Xcode informuje, po zakończeniu przekazywania pliku do programu iTunes Connect.


Archiwum przepływu pracy w programie Visual Studio for Mac zostanie automatycznie otwarta w moduł ładujący aplikacji, po zapisaniu _.ipa_:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Przesyłanie aplikacji do firmy Apple do przeglądu odbywa się przy użyciu aplikacji moduł ładujący aplikacji. Te kroki należy wykonać w Mac hosta kompilacji. Istnieje możliwość pobrania kopii modułu ładującego aplikacji z [tutaj](https://itunesconnect.apple.com/apploader/ApplicationLoader_3.0.dmg).

-----

1. Wybierz *dostarczania aplikacji* i kliknij przycisk *wybierz* przycisk:

    [![](publishing-to-the-app-store-images/publishvs01.png "Wybierz pozycję świadczenia aplikacji")](publishing-to-the-app-store-images/publishvs01.png#lightbox)

2. Wybierz plik zip lub IPA utworzone powyżej i kliknij przycisk **OK** przycisku.

3. Moduł ładujący aplikacji będzie sprawdzanie poprawności pliku:

    [![](publishing-to-the-app-store-images/publishvs02.png "Na ekranie sprawdzania poprawności")](publishing-to-the-app-store-images/publishvs02.png#lightbox)
4. Kliknij przycisk *dalej* przycisk i aplikacja zostanie zweryfikowana względem sklepu z aplikacjami:

    [![](publishing-to-the-app-store-images/publishvs03.png "Sprawdzanie poprawności względem sklepu z aplikacjami")](publishing-to-the-app-store-images/publishvs03.png#lightbox)
5. Kliknij przycisk **wysyłania** przycisk, aby wysłać aplikacji do firmy Apple do przeglądu.
6. Moduł ładujący aplikacji informuje, kiedy plik został pomyślnie przekazany.

## <a name="itunes-connect-status"></a>Stan połączenia iTunes

Zaloguj się do programu iTunes Connect, wybierz aplikację z listy dostępnych aplikacji stanu iTunes Connect powinna zostać wyświetlona, że jest **oczekiwanie na przegląd** (tymczasowo może odczytać **przekazać Odebrano** podczas przetwarzania):

[![](publishing-to-the-app-store-images/image21.png "Stan w programach iTunes Connect powinna zostać wyświetlona oczekuje do przeglądu")](publishing-to-the-app-store-images/image21.png#lightbox)

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawione przewodnik krok po kroku, konfigurowanie, tworzenie i przesyłanie aplikacji do sklepu z aplikacjami publikacji. Najpierw go opisane kroki niezbędne do tworzenia i zainstalować dystrybucji profil inicjowania obsługi administracyjnej. Następnie udał za pośrednictwem jak używać programu Visual Studio i Visual Studio for Mac do tworzenia kompilacji dystrybucji. Na koniec on wyświetlał sposobu używania programu iTunes Connect i narzędzia do przesyłania aplikacji do sklepu z aplikacjami.


## <a name="related-links"></a>Linki pokrewne

- [Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md)
- [Podręcznik przepływu pracy programowania aplikacji systemu iOS: dystrybucja aplikacji](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [Wskazówki dotyczące przesyłania sklepu z aplikacjami](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Typowe odrzuceń aplikacji](https://developer.apple.com/app-store/review/rejections/)
- [Wytyczne dotyczące sklepu App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
