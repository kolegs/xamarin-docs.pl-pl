---
title: Publikowanie do sklepu z aplikacjami firmy Apple TV
description: Ten dokument zawiera opis publikowania aplikacji do sklepu z aplikacjami firmy Apple TV. Go w tym artykule omówiono sposób konfigurowania, obsługi administracyjnej, tworzenie i przesyłanie aplikacji systemu tvOS, utworzony za pomocą platformy Xamarin.
ms.prod: xamarin
ms.assetid: 52448C93-DC19-40FA-BF8C-608AE680FF49
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: ac905caaf0bdefe7f0c5502be0bd63102ca5a813
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789307"
---
# <a name="publishing-to-the-apple-tv-app-store"></a>Publikowanie do sklepu z aplikacjami firmy Apple TV

W kolejności dystrybuować aplikacje na wszystkich urządzeniach Apple TV, Firma Apple wymaga aplikacji, które mają być opublikowane za pośrednictwem *sklepu z aplikacjami firmy Apple TV*, co pojedynczej lokalizacji zakupów dla aplikacji systemu tvOS sklepu z aplikacjami. Deweloperzy wielu typów aplikacji można kapitalizacji na ogromną Powodzenie ten pojedynczy punkt dystrybucji. Sklep App Store firmy Apple TV to gotowe rozwiązanie, oferty deweloperzy aplikacji systemów dystrybucji i płatności.

Proces przesyłania aplikacji do sklepu z aplikacjami firmy Apple TV obejmuje:

1. Tworzenie *identyfikator aplikacji* i wybierając *uprawnień*.
2. Tworzenie *dystrybucji profil inicjowania obsługi administracyjnej.*
3. Tworzenie aplikacji za pomocą tego profilu.
4. Przesyłanie aplikacji poprzez *iTunes Connect*.


W tym artykule omówimy wszystkie kroki niezbędne do obsługi administracyjnej, tworzenie i przesyłanie aplikacji do sklepu z aplikacjami firmy Apple TV dystrybucji.

<a name="Before_you_Submit" />

## <a name="before-you-submit-an-application"></a>Przed wysłaniem aplikacji

Po przesłaniu aplikacji dla publikacji do sklepu z aplikacjami firmy Apple TV, przechodzi ona przez proces oceny przez firmę Apple, aby mieć pewność, że spełnia on wymagania firmy Apple wytyczne dotyczące jakości i zawartości. Jeśli aplikacja nie spełniają te wytyczne, Apple odrzuci go po tym czasie konieczne będzie adresów — niezgodność odnosiło się przez firmę Apple, a następnie prześlij ponownie.
W związku z tym należy pozostawić najlepszy sposób sprawia, że za pośrednictwem firmy Apple, przejrzyj zapoznawanie się z tymi wytycznymi, a próby dostosowanie aplikacji do nich. Wytycznymi firmy Apple są dostępne pod adresem [wytyczne przeglądu sklepu aplikacji](https://developer.apple.com/appstore/resources/approval/guidelines.html) i [przygotowanie Your przesyłanie aplikacji dla nowej Apple TV](https://developer.apple.com/tvos/submit/).

Jest kilka rzeczy, które należy obserwować podczas przesyłania aplikacji:

1. Upewnij się, że opis aplikacji jest zgodny funkcjonalności aplikacji.
2. Test, który nie awarii aplikacji w obszarze normalnego użycia. W tym użycie na każdym urządzeniu Apple TV, która jest obsługiwana.


Apple zachowuje również listę porady przesyłanie sklepu z aplikacjami firmy Apple TV. Można znaleźć w [dystrybucji ze sklepu App store](https://developer.apple.com/appstore/resources/submission/tips.html).

<a name="Configuring_your_Application_in_iTunes_Connect" />

## <a name="configuring-your-application-in-itunes-connect"></a>Konfigurowanie aplikacji w iTunes Connect

[Połącz iTunes](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) to pakiet narzędzi sieci web, między innymi, zarządzanie aplikacjami systemu tvOS ze sklepu Apple TV App Store. Aplikacja Xamarin.tvOS musi być prawidłowo skonfigurowane i w iTunes Connect przed może zostać przesłane do firmy Apple do przeglądu, a ostatecznie zwalniane sprzedaży lub jako bezpłatną aplikację w sklepie z aplikacjami firmy Apple TV.

Wykonaj następujące czynności:

1. Sprawdź, czy odpowiednie umowy w miejscu i aktualności w **umów podatku i bankowości** sekcji iTunes Połącz z wersji aplikacji systemu iOS bezpłatnie lub do sprzedaży.
2. Utwórz nową **iTunes rekordu Connect** dla aplikacji i określ jej **Nazwa wyświetlana** (jak w sklepie z aplikacjami firmy Apple TV).
3. Wybierz **cena sprzedaży** lub określ bezpłatnie wydane aplikacji.
4. Podaj **ikonę aplikacji sklepu** (duże ikony) i zrzuty ekranu aplikacji akcji, na urządzeniach Apple TV, obsługuje on. Zobacz nasze [Praca z obrazów i ikon](~/ios/tvos/app-fundamentals/icons-images.md) przewodnika, aby uzyskać więcej informacji.
5. Podaj wyczyść zwięzły **opis** aplikacji w tym jej funkcje i korzyści dla użytkownika końcowego.
6. Podaj **kategorii**, **pod kategorii**, i **słowa kluczowe** pomóc użytkownikowi postrzegać twoją aplikację w sklepie z aplikacjami firmy Apple TV.
7. Podaj **skontaktuj się z** i **Obsługa** adresów URL do witryny sieci Web wymagane przez firmę Apple.
8. Ustaw aplikacji **klasyfikacji**, który jest używany przez kontroli rodzicielskiej ze sklepu Apple TV App Store.
9. Skonfigurować opcjonalne technologii sklepu z aplikacjami, takie jak **Game Center** i **zakupu w aplikacji**.

Aby uzyskać więcej informacji, zobacz nasze [konfigurowanie Twojego systemu tvOS aplikacji w iTunes Connect](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) dokumentacji.

<a name="Preparing_for_App_Store_Distribution" />

## <a name="preparing-for-app-store-distribution"></a>Przygotowywanie do dystrybucji sklepu z aplikacjami

Aby opublikować aplikację do sklepu z aplikacjami firmy Apple TV, należy najpierw skompiluj go do dystrybucji, który obejmuje wiele kroków. Poniższe sekcje obejmuje wszystko, co jest wymagane w celu przygotowania aplikacji Xamarin.tvOS dla publikacji tak, aby mogą być tworzone i przesłać je do sklepu Apple TV aplikacji do przeglądu i wersji.

<a name="Provisioning_for_Application_Services" />

### <a name="provisioning-for-application-services"></a>Inicjowanie obsługi administracyjnej dla usług aplikacji

Apple zawiera specjalne usług aplikacji, nazywane również uprawnienia, które można aktywować aplikacji systemu tvOS, podczas tworzenia Unikatowy identyfikator dla tego zaznaczenia. Czy korzystasz z uprawnień niestandardowych lub nie, nadal musisz utworzyć unikatowy identyfikator dla aplikacji Xamarin.tvOS, zanim będzie można opublikować w sklepie Apple TV aplikacji.

Tworzenie Identyfikatora aplikacji i opcjonalnie wybranie uprawnień obejmuje następujące czynności przy użyciu firmy Apple iOS opartych na sieci web portalu inicjowania obsługi:

1. Wybierz **inicjowania obsługi administracyjnej** > **programowanie**.
2. Kliknij przycisk **+** przycisk i podaj **nazwa** i **identyfikator pakietu** nowej aplikacji.
3. Przewiń do dołu ekranu i wybrać dowolną **usługi aplikacji** które będzie wymagane przez aplikację Xamarin.tvOS.
4. Kliknij przycisk **Kontynuuj** przycisk i następujących wyświetlanymi instrukcjami, aby utworzyć nowy identyfikator aplikacji.

Oprócz wybierania i konfigurowania wymaganych usług aplikacji, podczas definiowania Identyfikatora aplikacji, należy również skonfigurować identyfikator aplikacji i uprawnień w projekcie Xamarin.tvOS, edytując zarówno `Info.plist` i `Entitlements.plist` plików.

Wykonaj następujące czynności w programie Visual Studio dla komputerów Mac:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Info.plist` plik, aby otworzyć do edycji.
2. W **systemu tvOS aplikacji docelowej** , wprowadź nazwę aplikacji i wprowadź **identyfikator pakietu** utworzony podczas definiowania identyfikator aplikacji.
3. Zapisać zmiany w `Info.plist` pliku.
4. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Entitlements.plist` plik, aby otworzyć do edycji.
5. Wybierz i skonfiguruj uprawnienia wymagane dla Ciebie Xamarin.tvOS aplikacji, aby odpowiadały instalacji wykonywane powyżej, gdy zdefiniowane identyfikator aplikacji.
6. Zapisać zmiany w `Entitlements.plist` pliku.

Aby uzyskać szczegółowe instrukcje, zobacz nasze [inicjowania obsługi administracyjnej dla usług aplikacji](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) dokumentacji. Podczas tego dokumentu została napisana dla systemu iOS, te same kroki służą do udostępniania aplikacji Xamarin.tvOS.

<a name="Setting_the_Apps_Icons_and_Launch_Screens" />

### <a name="setting-the-apps-icons-launch-image-and-top-shelf-image"></a>Ustawienie ikon aplikacji, uruchom obrazu i górnej półki obrazu

Dla aplikacji systemu tvOS zostać zaakceptowana przez firmę Apple do włączenia w sklepie z aplikacjami firmy Apple TV wymaga prawidłowego ikony, uruchamiania i obrazy górnej półki dla wszystkich urządzeń Apple TV, które będą działały na. Będzie konieczne Dodaj wymagane zasoby obrazu, który zostanie skompilowany w `Assets.car` pliku i zawartych w pakiecie aplikacji Xamarin.tvOS, przed przesłaniem do iTunes Connect.

Aby uzyskać szczegółowe instrukcje, zobacz nasze [Praca z obrazów i ikon](~/ios/tvos/app-fundamentals/icons-images.md) dokumentacji.

<a name="Creating_and_Installing_a_Distribution_Profile" />

### <a name="creating-and-installing-a-distribution-profile"></a>Tworzenie i instalowanie profilu dystrybucji

używa systemu tvOS *profile aprowizacji* do kontrolowania wdrażanie kompilacji określonej aplikacji. Są to pliki, które zawierają informacje o certyfikacie używanym do podpisania aplikacji, *identyfikator aplikacji*, i zainstalowaną aplikację. Do tworzenia i dystrybucji ad-hoc profilu inicjowania obsługi administracyjnej zawiera listę dozwolonych urządzeń, do których można wdrożyć aplikację. Jednak dystrybucji sklepu z aplikacjami firmy Apple TV, tylko informacje identyfikator certyfikatu i aplikacji są uwzględnione, ponieważ jest tylko mechanizm dystrybucji publicznych za pośrednictwem sklepu z aplikacjami firmy Apple TV.

Udostępnianie obejmuje następujące czynności przy użyciu firmy Apple iOS opartego na sieci web portalu inicjowania obsługi:

1.  Wybierz **inicjowania obsługi administracyjnej** > **dystrybucji**.
2.  Kliknij przycisk **+** przycisk i wybierz typ profilu dystrybucji, który ma zostać utworzony jako **sklepu z aplikacjami firmy Apple TV**.
3.  Wybierz **identyfikator aplikacji** z listy rozwijanej, który chcesz utworzyć profil dystrybucji.
4.  Wybierz certyfikat wymagany do podpisywania aplikacji.
5.  Wprowadź **nazwa** nowego **profil dystrybucji** i generować profilu.
6.  Odśwież listę dostępnych profilów w programie Xcode.
7.  Wybierz profil w programie Visual Studio dla inicjowania obsługi administracyjnej dystrybucji **sklepu z aplikacjami** _Konfiguracja kompilacji_.

Aby uzyskać szczegółowe instrukcje, zobacz [tworzenia profilu dystrybucji](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#creatingprofile) i [wybór profilu dystrybucji w projekcie platformy Xamarin.iOS](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile). Ponownie oba te dokumenty są specyficzne dla systemu iOS, ale ta sama metoda jest używana dla aplikacji systemu tvOS.


<a name="Setting_the_Build_Configuration_for_your_Application" />

### <a name="setting-the-build-configuration-for-your-application"></a>Ustawienie konfiguracji kompilacji dla aplikacji

Domyślnie podczas tworzenia nowej aplikacji Xamarin.tvOS _konfiguracje kompilacji_ są tworzone automatycznie dla obu **debugowania** i **wersji** wdrożenia. Przed wykonaniem końcowego kompilacji, który będzie można być przesyłanie aplikacji do firmy Apple, istnieje kilka zmian, które należy wprowadzić w bazie **wersji** konfiguracji.

Wykonaj następujące czynności:

1. Kliknij prawym przyciskiem myszy **Nazwa projektu** w **Eksploratora rozwiązań** i wybieranie **opcje** aby je otworzyć do edycji.
2. Jeśli ma być przeznaczona dla określonej wersji systemu tvOS, wybierz go w obszarze **systemu tvOS kompilacji** > **system iOS w wersji zestawu SDK**. W wersji zapoznawczej obsługę systemu tvOS, pozostaw tę wartość ustawiono **domyślne**.
3. Łączenie zmniejszyć całkowity rozmiar aplikacji dystrybucyjnego przez usuwanie limit nieużywane klasy metod, właściwości, itd. i w większości przypadków należy pozostawić wartość domyślna wynosząca **tylko łącze framework SDK**. W niektórych sytuacjach, na przykład podczas przy użyciu określonych niektórych 3 bibliotek strony, może być wymuszono Ustaw tę wartość na **nie zawierają łącza** do zachowania elementu potrzebne przed usunięciem.
4. Aby wysłać aplikacji Xamarin.tvOS, należy używać kompilatora optymalizującego LLVM. Upewnij się, że **Użyj LLVM optymalizacji kompilatora** zaznaczono pole w obszarze **wersji** konfiguracji.
5. Apple wymagane również, że aplikacje systemu tvOS użycia kodu bitowego. Ponownie w obszarze **wersji** konfiguracji, Dodaj `--bitcode=asmonly` do **mtouch dodatkowe argumenty** pole.
6. **Pliki obrazów PNG Optymalizuj dla systemu iOS** powinno być zaznaczone pole wyboru, ponieważ zapewni to dodatkowe zmniejszenie rozmiaru elementu dostarczanego aplikacji.
7. Debugowanie powinno *nie* być włączone, ponieważ spowoduje to, że kompilacji niepotrzebnie większy.


<a name="Building_and_Submitting_the_Distributable" />

## <a name="building-and-submitting-the-distributable"></a>Tworzenie i przesyłanie dystrybucyjnego

Z aplikacją Xamarin.tvOS prawidłowo skonfigurowane możesz teraz przystąpić do czy kompilacji końcowej dystrybucji, która będzie można przesyłania należy do firmy Apple do przeglądu i wersji.

#### <a name="build-your-archive"></a>Tworzenie archiwum

1. Wybierz **wersji | Urządzenie** konfiguracji w programie Visual Studio dla komputerów Mac:

    ![](app-store-publishing-images/buildxs01new.png "Wybierz konfigurację wydania")
2. Z **kompilacji** menu, wybierz opcję **archiwum publikowania**:

    [![](app-store-publishing-images/buildxs02new.png "Wybierz archiwum do publikowania")](app-store-publishing-images/buildxs02new.png#lightbox)
3. Po utworzeniu archiwum **archiwa** zostanie wyświetlony widok:

    [![](app-store-publishing-images/buildxs03new.png "Wyświetl archiwa")](app-store-publishing-images/buildxs03new.png#lightbox)

### <a name="sign-and-distribute-your-app"></a>Zaloguj się i dystrybucja aplikacji

Zawsze skompilować aplikację dla archiwum, zostanie on automatycznie otwarty *widoku archiwa*, wyświetlanie wszystkich projektów zarchiwizowane; pogrupowane według rozwiązania. Domyślnie ten widok przedstawia tylko bieżące, otwórz rozwiązanie. Aby wyświetlić wszystkie rozwiązania, których archiwów, kliknij na **Pokaż wszystkie archiwa** opcji.

Zalecane jest aby archiwa wdrażanie u klientów (wdrożenia sklepu z aplikacjami lub Enterprise) były przechowywane, tak, aby informacje o debugowaniu, który jest generowany można symbolized w późniejszym terminie.

Aby zarejestrować aplikację i przygotowania go do dystrybucji:

1. Wybierz **utworzyć i dystrybuować...** , ilustrowane poniżej:

    [![](app-store-publishing-images/buildxs04new.png ", Wybierz theSign i rozpowszechnij...")](app-store-publishing-images/buildxs04new.png#lightbox)
2. Spowoduje to otwarcie Kreatora publikacji. Wybierz **sklepu z aplikacjami** kanału dystrybucji, aby utworzyć pakiet i otwórz moduł ładujący aplikacji:

    [![](app-store-publishing-images/distribute01.png "Wybierz kanał dystrybucji sklepu z aplikacjami")](app-store-publishing-images/distribute01.png#lightbox)
3. Na ekranie profilu inicjowania obsługi administracyjnej Wybierz tożsamość podpisywania i odpowiadający profil inicjowania obsługi administracyjnej lub ponownie podpisać tożsamość:

    [![](app-store-publishing-images/distribute02.png "Wybierz tożsamość podpisywania i odpowiadający profil inicjowania obsługi administracyjnej")](app-store-publishing-images/distribute02.png#lightbox)
4. Sprawdź szczegóły pakietu, a następnie kliknij przycisk **publikowania** zapisać Twoje `.ipa` pakietu:

    [![](app-store-publishing-images/distribute03.png "Sprawdź szczegóły pakietu")](app-store-publishing-images/distribute03.png#lightbox)
5. Raz z `.ipa` zostało zapisane, że aplikacja jest gotowa do przekazania do programu iTunes Połącz przez moduł ładujący aplikacji:

    [![](app-store-publishing-images/distribute04.png "Przekazane do programu iTunes Połącz przez moduł ładujący aplikacji")](app-store-publishing-images/distribute04.png#lightbox)

Z dystrybucją kompilacja utworzona i archiwizowane, możesz teraz przystąpić do przesyłania aplikacji do iTunes Connect.

<a name="Submitting_Your_App_to_Apple" />

## <a name="submitting-your-app-to-apple"></a>Przesyłanie aplikacji do firmy Apple

Ukończono budowanie dystrybucji można przystąpić do przesyłania aplikacji systemu iOS do firmy Apple w celu przeglądu i wersję ze sklepu App Store.


Archiwum przepływu pracy w programie Visual Studio for Mac zostanie automatycznie otwarta w moduł ładujący aplikacji, po zapisaniu `.ipa`:

2. Wybierz *dostarczania aplikacji* i kliknij przycisk *wybierz* przycisk:

    [![](app-store-publishing-images/publishvs01.png "Wybierz pozycję świadczenia aplikacji")](app-store-publishing-images/publishvs01.png#lightbox)

3. Wybierz plik zip lub IPA utworzone powyżej i kliknij przycisk **OK** przycisku.
4. Moduł ładujący aplikacji będzie sprawdzanie poprawności pliku:

    [![](app-store-publishing-images/publishvs02.png "Moduł ładujący aplikacji ekranu sprawdzania poprawności")](app-store-publishing-images/publishvs02.png#lightbox)
5. Kliknij przycisk *dalej* przycisk i aplikacja zostanie zweryfikowana względem sklepu z aplikacjami:

    [![](app-store-publishing-images/publishvs03.png "Aplikacja jest weryfikowana pod kątem Sklepu z aplikacjami")](app-store-publishing-images/publishvs03.png#lightbox)
6. Kliknij przycisk **wysyłania** przycisk, aby wysłać aplikacji do firmy Apple do przeglądu.
7. Moduł ładujący aplikacji informuje, kiedy plik został pomyślnie przekazany.

<a name="iTunes_Connect_Status" />

### <a name="itunes-connect-status"></a>Stan połączenia iTunes

Zaloguj się do programu iTunes Connect, wybierz aplikację z listy dostępnych aplikacji stanu iTunes Connect powinna zostać wyświetlona, że jest **oczekiwanie na przegląd** (tymczasowo może odczytać **przekazać Odebrano** podczas przetwarzania):

[![](app-store-publishing-images/image21.png "Stan w programach iTunes połączyć przedstawiający oczekiwania do przeglądu")](app-store-publishing-images/image21.png#lightbox)

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli występują problemy dotyczące przesyłania Xamarin.tvOS aplikacji do sklepu z aplikacjami firmy Apple TV, zobacz nasze [Rozwiązywanie problemów](~/ios/tvos/troubleshooting.md) przewodnik. Zawiera kilka znanych problemów, które można napotkać i sposobu rozwiązania tych problemów Xamarin.tvOS.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawione przewodnik krok po kroku, konfigurowanie, tworzenie i przesyłanie aplikacji do sklepu z aplikacjami firmy Apple TV publikacji. Najpierw go opisane kroki niezbędne do tworzenia i zainstalować dystrybucji profil inicjowania obsługi administracyjnej. Następnie udał za pośrednictwem Utwórz kompilację dystrybucji przy użyciu programu Visual Studio for Mac. Na koniec on wyświetlał sposobu używania programu iTunes Connect i narzędzie archiwum Xcode do przesyłania aplikacji do sklepu z aplikacjami firmy Apple TV.


## <a name="related-links"></a>Linki pokrewne

- [Praca z obrazami i ikonami](~/ios/tvos/app-fundamentals/icons-images.md)
- [Przygotowanie Your przesyłania aplikacji do nowego Apple TV](https://developer.apple.com/tvos/submit/)
- [Wskazówki dotyczące przesyłania sklepu z aplikacjami](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Typowe odrzuceń aplikacji](https://developer.apple.com/app-store/review/rejections/)
- [Wytyczne dotyczące sklepu App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
