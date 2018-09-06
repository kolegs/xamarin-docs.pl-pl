---
title: Publikowanie aplikacji platformy Xamarin.iOS Store aplikacji
description: W tym dokumencie opisano, jak konfigurować, tworzenie i publikowanie aplikacji platformy Xamarin.iOS dla dystrybucji w Store aplikacji.
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 7560f66acc3a3ea683e75be2ae85f908036e008c
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780668"
---
# <a name="publishing-xamarinios-apps-to-the-app-store"></a>Publikowanie aplikacji platformy Xamarin.iOS Store aplikacji

Aby opublikować aplikację, aby [App Store](https://www.apple.com/ios/app-store/), Deweloper aplikacji musi najpierw Prześlij — wraz z zrzuty ekranu, opis, ikony i inne informacje — do firmy Apple do przeglądu. Po zatwierdzeniu aplikacji Apple umieszcza je w Store App, gdzie użytkownicy mogą je kupić i zainstalować go bezpośrednio ze swoich urządzeń z systemem iOS.

W tym przewodniku opisano kroki, które trzeba wykonać, aby przygotować aplikację do Store aplikacji i wyślij je do firmy Apple do przeglądu. W szczególności opisano:

> [!div class="checklist"]
> - Następujące wytyczne przeglądu App Store
> - Konfigurowanie Identyfikatora aplikacji i uprawnienia
> - Ikona App Store i ikony aplikacji
> - Konfigurowanie App Store, profil inicjowania obsługi administracyjnej
> - Aktualizowanie **wersji** Konfiguracja kompilacji
> - Konfigurowanie aplikacji w usłudze iTunes Connect
> - Kompilowanie aplikacji i przesłania go do firmy Apple

> [!IMPORTANT]
> Apple [wskazał](https://developer.apple.com/news/?id=05072018a) , począwszy od lipca 2018 r., wszystkie aplikacje i aktualizacje przesłane Store aplikacji musi utworzone za pomocą narzędzia iOS 11 SDK i [musi obsługiwać ekranu telefonu iPhone X](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md).

## <a name="app-store-guidelines"></a>Wytyczne dotyczące App Store

Przed przesłaniem aplikacji do publikacji w Store aplikacji, upewnij się, że spełnia standardy określone przez firmy Apple [wytyczne dotyczące sklepu App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html).
Podczas przesyłania aplikacji do aplikacji App Store firmy Apple przegląda ją, aby upewnić się, że spełnia on tych wymagań. Jeśli nie, Firma Apple odrzuci — i konieczne będzie wspominane problemami i Prześlij ponownie.
Dlatego jest dobry pomysł, aby zapoznać się możliwie jak najszybciej w procesie tworzenia wytycznych.

Kilka rzeczy, aby Zwróć uwagę na podczas przesyłania aplikacji:

1. Upewnij się, że opis aplikacji odpowiada jego działanie.
2. Test, który umożliwia aplikacji nie ulec awarii w ramach normalnego użycia. Obejmuje to obciążenie na każdym urządzeniu z systemem iOS, które obsługuje.

Ponadto zapoznaj się z [zasoby związane z App Store](https://developer.apple.com/app-store/resources/) zapewniająca firmy Apple.

## <a name="set-up-an-app-id-and-entitlements"></a>Konfigurowanie Identyfikatora aplikacji i uprawnienia

Każda aplikacja dla systemu iOS ma unikatowy identyfikator aplikacji, który ma skojarzony zestaw usług aplikacji o nazwie *uprawnień*. Uprawnień aplikacjom wykonywanie różnych czynności takich jak otrzymać powiadomienie wypychane, dostęp do funkcji systemu iOS, takich jak zestaw HealthKit i nie tylko.

Aby utworzyć identyfikator aplikacji i wybierz dowolne wymagane uprawnienia, odwiedź stronę [portalu Apple Developer Portal](https://developer.apple.com/account/) i wykonaj następujące czynności:

1. W **certyfikaty, identyfikatory i profile** zaznacz **identyfikatory > identyfikatory aplikacji**.
2. Kliknij przycisk **+** przycisk, a następnie podaj **nazwa** i **identyfikator pakietu** nowej aplikacji.
3. Przewiń do dołu ekranu i wybrać dowolną **App Services** które będzie wymagane przez aplikację platformy Xamarin.iOS. App Service są opisane w [Praca z funkcjami w rozszerzeniu Xamarin.iOS](~/ios/deploy-test/provisioning/capabilities/index.md) przewodnik.
4. Kliknij przycisk **Kontynuuj** przycisk, a następnie postępuj zgodnie z wyświetlanymi instrukcjami, aby utworzyć nowy identyfikator aplikacji.

Oprócz Wybieranie i konfigurowanie usług aplikacji, podczas definiowania Identyfikatora aplikacji, należy skonfigurować identyfikator aplikacji oraz uprawnień do projektu Xamarin.iOS, edytując **Info.plist** i  **Plik Entitlements.plist** plików. Aby uzyskać więcej informacji, zapoznaj się [Praca z uprawnieniami w rozszerzeniu Xamarin.iOS](~/ios/deploy-test/provisioning/entitlements.md) przewodnik, który opisuje sposób tworzenia **plik Entitlements.plist** plików i znaczenie różnych ustawień uprawnień zawiera on.

## <a name="include-an-app-store-icon"></a>Obejmują ikonę App Store

Podczas przesyłania aplikacji do firmy Apple, należy się upewnić, że zawiera on katalog zasobów, który zawiera ikonę App Store. Aby dowiedzieć się, jak to zrobić, Przyjrzyj się [App Store ikony w rozszerzeniu Xamarin.iOS](~/ios/app-fundamentals/images-icons/app-store-icon.md) przewodnik.

## <a name="set-the-apps-icons-and-launch-screens"></a>Ustawienie ikon aplikacji i ekrany uruchamiania

Dla firmy Apple udostępnić aplikację dla systemu iOS na Store aplikacji musi mieć odpowiednie ikon i ekranów uruchamiania dla wszystkich urządzeń z systemem iOS, na którym można uruchomić. Aby uzyskać więcej informacji o konfigurowaniu ikony aplikacji i ekrany uruchamiania przeczytaj następujące przewodniki:

- [Ikony aplikacji w rozszerzeniu Xamarin.iOS](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Uruchom ekrany dla aplikacji platformy Xamarin.iOS](~/ios/app-fundamentals/images-icons/launch-screens.md)

## <a name="create-and-install-an-app-store-provisioning-profile"></a>Tworzenie i instalowanie programu App Store, profil inicjowania obsługi administracyjnej

dla systemu iOS używa *profilów aprowizacji* do kontrolowania sposobu kompilacji aplikacji można wdrożyć. Są to pliki, które zawierają informacje o certyfikat użyty do podpisania aplikacji, identyfikator aplikacji i zainstalowaną aplikację. Do tworzenia i dystrybucji ad-hoc profilu inicjowania obsługi administracyjnej zawiera również listę dozwolonych urządzeń, na których można wdrożyć aplikację. Jednak w celu dystrybucji App Store tylko informacji o certyfikatach i identyfikator aplikacji są uwzględniane tylko przy użyciu mechanizmu dystrybucji publiczny jest Store aplikacji.

Aby utworzyć i zainstalować App Store, profil inicjowania obsługi administracyjnej, wykonaj następujące kroki:

1. Zaloguj się do [portalu Apple Developer Portal](https://developer.apple.com/account/).
2. W **certyfikaty, identyfikatory i profile**, wybierz opcję **profilów aprowizacji > dystrybucji**.
3. Kliknij przycisk **+** przycisku Wybierz **App Store**i kliknij przycisk **Kontynuuj**.
4. Wybierz swoją aplikację **Identyfikatora aplikacji** z listy i kliknij przycisk **Kontynuuj**.
5. Wybierz certyfikat podpisywania, a następnie kliknij przycisk **Kontynuuj**.
6. Wprowadź **nazwa profilu** i kliknij przycisk **Kontynuuj** do generowania profilu.
7. Użyj platformy Xamarin [Zarządzanie kontem Apple](~/cross-platform/macios/apple-account-management.md) narzędzi, aby pobrać nowo utworzony inicjowania obsługi profilu na komputerze Mac. Jeśli masz na komputerze Mac, możesz również pobrać profilu inicjowania obsługi administracyjnej bezpośrednio z portalu dla deweloperów firmy Apple i dwukrotnie kliknij go, aby zainstalować.

Aby uzyskać szczegółowe instrukcje, zobacz [tworzenia profilu dystrybucji](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) i [wybór profilu dystrybucji w projekcie platformy Xamarin.iOS](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile).

## <a name="update-the-release-build-configuration"></a>Aktualizowanie konfiguracji kompilacji wydania

Automatyczne konfigurowanie nowe projekty Xamarin.iOS **debugowania** i **wersji** _konfiguracje kompilacji_. Aby poprawnie skonfigurować **wersji** kompilacji, wykonaj następujące kroki:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Z **konsoli rozwiązania**, otwórz **Info.plist**. Wybierz **Aprowizacja ręczna**. Zapisz i zamknij plik.
2. Kliknij prawym przyciskiem myszy **Nazwa projektu** w **konsoli rozwiązania**, wybierz opcję **opcje**i przejdź do **kompilacja systemu iOS** kartę.
3. Ustaw **konfiguracji** do **wersji** i **platformy** do **iPhone**.
4. Aby skompilować z określonym zestawem SDK systemu iOS, wybierz go z **wersja zestawu SDK** listy. W przeciwnym razie pozostaw tę wartość na **domyślne**.
5. Łączenie zmniejsza całkowity rozmiar aplikacji przy obcięcie się nieużywany kod. W większości przypadków **zachowanie konsolidatora** należy ustawić wartość domyślną **Połącz tylko zestawy SDK struktury**. W niektórych sytuacjach takich jak podczas korzystania z niektórych bibliotek innych firm, może być konieczne Ustaw tę wartość na **nie łącz** aby upewnić się, wymagane kod nie zostanie usunięta. Aby uzyskać więcej informacji, zobacz [aplikacji Xamarin.iOS łączenie](~/ios/deploy-test/linker.md) przewodnik.
6. Sprawdź **obrazów PNG zoptymalizować** dodatkowo zmniejszyć rozmiar aplikacji.
7. Debugowanie powinno _nie_ można włączyć, ponieważ spowoduje to, że kompilacja niepotrzebnie.
8. Dla systemu iOS 11, wybierz jedną z architektury urządzenia, które obsługuje **ARM64**. Aby uzyskać więcej informacji na temat kompilacji dla urządzeń z systemem iOS w 64-bitowych, zobacz **Włączanie 64-bitowych kompilacji dla aplikacji platformy Xamarin.iOS** części [uwagi dotyczące platform 64-32-bitowych](~/cross-platform/macios/32-and-64/index.md) dokumentacji.
9. Możesz też chcieć użyć **LLVM** kompilatora do kompilowania kodu na mniejsze i szybsze. Jednak ta opcja zwiększa czasy kompilacji.
10. Odpowiednio do potrzeb swojej aplikacji, możesz również chcieć dostosować typ **wyrzucania elementów bezużytecznych** są używane i skonfigurowane pod kątem **internacjonalizacji**.

    Po ustawieniu opcji opisanych powyżej, ustawienia kompilacji powinien wyglądać mniej więcej tak:

    ![ustawienia kompilacji systemu iOS](publishing-to-the-app-store-images/build-m157.png "ustawienia kompilacji systemu iOS")

    Ponadto zapoznaj się z tego [mechanika kompilacji dla systemu iOS](~/ios/deploy-test/ios-build-mechanics.md) przewodniku dalej w tym artykule opisano ustawienia kompilacji.

11. Przejdź do **podpisywanie pakietu systemu iOS** kartę. Jeśli opcje, w tym miejscu nie są edytowalne, upewnij się, że **ręcznego inicjowania obsługi administracyjnej** wybrano **Info.plist** pliku.
12. Upewnij się, że **konfiguracji** jest ustawiona na **wersji** i **platformy** ustawiono **iPhone**.
13. Ustaw **tożsamość do podpisywania** do **dystrybucja (automatycznie)**.
14. Dla **profilu aprowizacji**, wybierz pozycję App Store, profil inicjowania obsługi administracyjnej [utworzonego powyżej](#create-and-install-an-app-store-provisioning-profile).

    Pakiet projektu, opcje podpisywania powinna teraz wyglądać podobnie do następującej:

    ![Podpisywanie pakietu systemu iOS](publishing-to-the-app-store-images/bundleSigning-m157.png "podpisywanie pakietu systemu iOS")

15. Kliknij przycisk **OK** można zapisać zmian we właściwościach projektu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Upewnij się, że program Visual Studio 2017 został [sparowano z hostem kompilacji Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Kliknij prawym przyciskiem myszy **Nazwa projektu** w **Eksploratora rozwiązań**, wybierz opcję **właściwości**.
3. Przejdź do **kompilacja systemu iOS** kartę i i ustaw **konfiguracji** do **wersji** i **platformy** do **iPhone**.
4. Aby skompilować z określonym zestawem SDK systemu iOS, wybierz go z **wersja zestawu SDK** listy. W przeciwnym razie pozostaw tę wartość na **domyślne**.
5. Łączenie zmniejsza całkowity rozmiar aplikacji przy obcięcie się nieużywany kod. W większości przypadków **zachowanie konsolidatora** należy ustawić wartość domyślną **Połącz tylko zestawy SDK struktury**. W niektórych sytuacjach takich jak podczas korzystania z niektórych bibliotek innych firm, może być konieczne Ustaw tę wartość na **nie łącz** aby upewnić się, wymagane kod nie zostanie usunięta. Aby uzyskać więcej informacji, zobacz [aplikacji Xamarin.iOS łączenie](~/ios/deploy-test/linker.md) przewodnik.
6. Sprawdź **obrazów PNG zoptymalizować** dodatkowo zmniejszyć rozmiar aplikacji.
7. Debugowanie nie powinna być włączona, ponieważ spowoduje to, że kompilacja niepotrzebnie.
8. Dla systemu iOS 11, wybierz jedną z architektury urządzenia, które obsługuje **ARM64**. Aby uzyskać więcej informacji na temat kompilacji dla urządzeń z systemem iOS w 64-bitowych, zobacz **Włączanie 64-bitowych kompilacji dla aplikacji platformy Xamarin.iOS** części [uwagi dotyczące platform 64-32-bitowych](~/cross-platform/macios/32-and-64/index.md) dokumentacji.
9. Możesz też chcieć użyć **LLVM** kompilatora do kompilowania kodu na mniejsze i szybsze. Jednak ta opcja zwiększa czasy kompilacji.
10. Odpowiednio do potrzeb swojej aplikacji, możesz również chcieć dostosować typ **wyrzucania elementów bezużytecznych** są używane i skonfigurowane pod kątem **internacjonalizacji**.

    Po ustawieniu opcji opisanych powyżej, ustawienia kompilacji powinien wyglądać mniej więcej tak:

    ![ustawienia kompilacji systemu iOS](publishing-to-the-app-store-images/build-w157.png "ustawienia kompilacji systemu iOS")

    Ponadto zapoznaj się z tego [mechanika kompilacji dla systemu iOS](~/ios/deploy-test/ios-build-mechanics.md) przewodniku dalej w tym artykule opisano ustawienia kompilacji.

11. Przejdź do **podpisywanie pakietu systemu iOS** kartę. Upewnij się, że **konfiguracji** jest ustawiona na **wersji**, **platformy** jest ustawiona na **iPhone**oraz że **ręcznego inicjowania obsługi administracyjnej.**  jest zaznaczone.
12. Ustaw **tożsamość do podpisywania** do **dystrybucja (automatycznie)**.
13. Dla **profilu aprowizacji**, wybierz pozycję App Store, profil inicjowania obsługi administracyjnej [utworzonego powyżej](#create-and-install-an-app-store-provisioning-profile).

    Pakiet projektu, opcje podpisywania powinna teraz wyglądać podobnie do następującej:

    ![Ustawienia podpisywanie zbiorów systemu iOS](publishing-to-the-app-store-images/bundleSigning-w157.png "ustawienia podpisywanie zbiorów systemu iOS")

14. Przejdź do **opcje IPA systemu iOS** kartę.
15. Upewnij się, że **konfiguracji** jest ustawiona na **wersji** i **platformy** ustawiono **iPhone**.
16. Sprawdź **kompilacji iTunes pakiet archiwum (IPA)** pola wyboru. To ustawienie spowoduje, że każdy **wersji** kompilacji (ponieważ jest wybrana konfiguracja), aby wygenerować plik IPA. Ten plik można przesłać do firmy Apple do wydania w Store aplikacji.

    > [!NOTE]
    > **Metadane programu iTunes** i **iTunesArtwork** nie są niezbędne dla wersji App Store. Aby uzyskać więcej informacji, zapoznaj się [plik iTunesMetadata.plist w aplikacji platformy Xamarin.iOS](~/ios/deploy-test/app-distribution/itunesmetadata.md) i [iTunes kompozycji](~/ios/app-fundamentals/images-icons/app-icons.md#itunes-artwork).

17. Aby określić, nazwa_pliku IPA, która różni się od nazwy projektu Xamarin.iOS, wprowadź go w **nazwy pakietu** pola.

    ![Ustawienia podpisywanie zbiorów systemu iOS](publishing-to-the-app-store-images/ipaOptions-w157.png "ustawienia podpisywanie zbiorów systemu iOS")

18. Zapisz konfigurację kompilacji i zamknij go.

-----

## <a name="configure-your-app-in-itunes-connect"></a>Konfigurowanie aplikacji w usłudze iTunes Connect

[iTunes Connect](https://itunesconnect.apple.com) to pakiet sieci web narzędzia do zarządzania aplikacji systemu iOS na Store aplikacji. Aplikacja platformy Xamarin.iOS muszą zostać prawidłowo skonfigurowane w usłudze iTunes Connect zanim może być przesyłany do firmy Apple w celu przeglądu i wydanej w dniu Store aplikacji.

Aby dowiedzieć się, jak to zrobić, przeczytaj [skonfigurowanie aplikacji w usłudze iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) przewodnik.

## <a name="build-and-submit-your-app"></a>Tworzenie i przesyłanie aplikacji

Prawidłowo skonfigurowane ustawienia kompilacji i iTunes Connect oczekujące na przesłanych informacji można utworzyć aplikację i go przesłać do firmy Apple.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. W programie Visual Studio dla komputerów Mac, wybierz **wersji** konfigurację kompilacji i urządzenia (nie simulator) do których chcesz utworzyć.

    ![Tworzenie konfiguracji i platformy](publishing-to-the-app-store-images/chooseConfig-m157.png "wybór Konfiguracja i platforma kompilacji")

2. Z **kompilacji** menu, wybierz opcję **archiwum dla publikowania**.
3. Po utworzeniu archiwum **archiwa** zostanie wyświetlony widok:

    ![Wyświetl archiwa](publishing-to-the-app-store-images/archives-m157.png "Wyświetl archiwa")

    > [!NOTE]
    > Domyślnie **archiwa** widoku wyświetlane są tylko archiwa otwartego rozwiązania. Aby zobaczyć wszystkie rozwiązania, które mają archiwów, sprawdź **Pokaż wszystkie archiwa** pola wyboru. To dobry pomysł, aby zachować starych archiwów, aby informacje o debugowaniu, które są umieszczane może być używane w celu symbolizacji raporty o awariach, jeśli to konieczne.

4. Kliknij przycisk **podpisywanie i dystrybucja...**  aby otworzyć Kreatora publikacji.
5. Wybierz **App Store** kanał dystrybucji. Kliknij przycisk **Dalej**.

    ![Wybór kanału dystrybucji](publishing-to-the-app-store-images/distChannel-m157.png "Wybór kanału dystrybucji")

6. W **profil inicjowania obsługi administracyjnej** okna, wybierz opcję tożsamości podpisywania, aplikacji i profilu inicjowania obsługi administracyjnej. Kliknij przycisk **Dalej**.

    ![Wybieranie profilu aprowizacji](publishing-to-the-app-store-images/provProfileSelect-m157.png "Wybieranie profilu aprowizacji")

7. Sprawdź szczegóły pakietu, a następnie kliknij przycisk **Publikuj** można zapisać pliku IPA aplikacji:

    ![Weryfikacja szczegółów aplikacji](publishing-to-the-app-store-images/publish-m157.png "weryfikacji szczegółów aplikacji")

8. Po zapisaniu swoje IPA aplikacja jest gotowa do przesłania do usługi iTunes Connect.

    ![Gotowy do przesłania](publishing-to-the-app-store-images/readyToGo-m157.png "gotowości do przesłania")

9. Kliknij przycisk **Otwórz program Application Loader** i zaloguj się (należy pamiętać, że musisz [utworzyć hasło specyficzny dla aplikacji](https://support.apple.com/ht204397) dla identyfikatora Apple ID).

    > [!NOTE]
    > Aby uzyskać więcej informacji o tym narzędziu, Przyjrzyj się [dokumentacji firmy Apple dotyczące program Application Loader](https://help.apple.com/itc/apploader/#/apdS673accdb).

10. Wybierz **dostarczanie aplikacji** i kliknij przycisk **wybierz** przycisku:

    ![Wybierz dostarczanie aplikacji](publishing-to-the-app-store-images/publishvs01.png "wybierz dostarczanie aplikacji")

11. Wybierz plik IPA utworzonego powyżej, a następnie kliknij przycisk **OK** przycisku.
12. Program Application Loader, zostanie przeprowadzona Weryfikacja pliku:

    ![Na ekranie weryfikacji](publishing-to-the-app-store-images/publishvs02.png "ekranu sprawdzania poprawności")

13. Kliknij przycisk **dalej** przycisk i aplikacja zostanie zweryfikowana względem Store aplikacji:

    ![Trwa sprawdzanie poprawności względem Store App](publishing-to-the-app-store-images/publishvs03.png "sprawdzanie poprawności względem Store aplikacji")

14. Kliknij przycisk **wysyłania** przycisk, aby przesłać aplikację do firmy Apple do przeglądu.
15. Program Application Loader informuje, kiedy plik został pomyślnie przekazany.

    > [!NOTE]
    > Apple może odrzucić aplikacji za pomocą **iTunesMetadata.plist** zawarte w pliku IPA, zostaje wyświetlony błąd podobny do następującego:
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > Obejście tego problemu z tym błędem, Przyjrzyj się [ten wpis na forum Xamarin](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!NOTE]
> Program Visual Studio 2017 nie obsługuje obecnie **archiwum dla publikowania** znaleziono przepływu pracy w programie Visual Studio dla komputerów Mac.

1. Upewnij się, że program Visual Studio 2017 został [sparowano z hostem kompilacji Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Wybierz **wersji** z programu Visual Studio 2017 **konfiguracje rozwiązania** listy rozwijanej i **iPhone** z **platformy rozwiązania** Lista rozwijana.

    ![Tworzenie konfiguracji i platformy](publishing-to-the-app-store-images/chooseConfig-w157.png "wybór Konfiguracja i platforma kompilacji")

3. Skompiluj projekt. Spowoduje to utworzenie pliku IPA.

    > [!NOTE]
    > [Aktualizowanie konfiguracji kompilacji wydania](#update-the-release-build-configuration) części tego dokumentu skonfigurowane ustawienia kompilacji aplikacji, aby utworzyć plik IPA dla każdego **wersji** kompilacji.

4. Aby znaleźć plik IPA na komputerze Windows, kliknij prawym przyciskiem myszy nazwę projektu rozszerzenia Xamarin.iOS w programie Visual Studio 2017 **Eksploratora rozwiązań** i wybierz polecenie **Otwórz Folder w Eksploratorze plików**. Następnie w Windows po prostu otworzyć **Eksploratora plików**, przejdź do **iPhone/bin/Release** podkatalogu. Jeśli nie masz [dostosowanego lokalizacją danych wyjściowych pliku IPA](#customize-the-ipa-location), należy go w tym katalogu.
5. Aby zamiast tego wyświetlić plik IPA na hostem kompilacji komputera Mac, kliknij prawym przyciskiem myszy nazwę projektu rozszerzenia Xamarin.iOS w Visual Studio 2017 **Eksploratora rozwiązań** (na Windows) i wybierz **Pokaż plik IPA na serwerze kompilacji**. Spowoduje to otwarcie **wyszukiwania** okno na hostem kompilacji komputera Mac przy użyciu pliku IPA wybrane.
6. Otwórz na hostem kompilacji Mac. **uruchamiania aplikacji**. W programie Xcode, wybierz **Xcode > Otwórz narzędzie dla deweloperów > Uruchamianie aplikacji**.

    > [!NOTE]
    > Aby uzyskać więcej informacji o tym narzędziu, Przyjrzyj się [dokumentacji firmy Apple dotyczące program Application Loader](https://help.apple.com/itc/apploader/#/apdS673accdb).

7. Zaloguj się w module uruchamiania aplikacji (należy pamiętać, że musisz [utworzyć hasło specyficzny dla aplikacji](https://support.apple.com/ht204397) dla identyfikatora Apple ID).
8. Wybierz **dostarczanie aplikacji** i kliknij przycisk **wybierz** przycisku:

    ![Wybierz dostarczanie aplikacji ](publishing-to-the-app-store-images/publishvs01.png "wybierz dostarczanie aplikacji")

9. Wybierz plik IPA utworzonego powyżej, a następnie kliknij przycisk **OK**.
10. Program Application Loader, zostanie przeprowadzona Weryfikacja pliku:

    ![Na ekranie weryfikacji](publishing-to-the-app-store-images/publishvs02.png "ekranu sprawdzania poprawności")

11. Kliknij przycisk **dalej** przycisk i aplikacja zostanie zweryfikowana względem Store aplikacji:

    ![Trwa sprawdzanie poprawności względem Store App](publishing-to-the-app-store-images/publishvs03.png "sprawdzanie poprawności względem Store aplikacji")

12. Kliknij przycisk **wysyłania** przycisk, aby przesłać aplikację do firmy Apple do przeglądu.
13. Program Application Loader informuje, kiedy plik został pomyślnie przekazany.

    > [!NOTE]
    > Apple może odrzucić aplikacji za pomocą **iTunesMetadata.plist** zawarte w pliku IPA, zostaje wyświetlony błąd podobny do następującego:
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > Obejście tego problemu z tym błędem, Przyjrzyj się [ten wpis na forum Xamarin](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1).

-----

## <a name="itunes-connect-status"></a>iTunes Connect stanu

Aby wyświetlić stan aplikacji przesłanych informacji, zaloguj się do usługi iTunes Connect, a następnie wybierz aplikację. Początkowy stan powinien być **oczekiwania do recenzji**, chociaż może tymczasowo odczytu **przekazywanie Odebrano** podczas, gdy jest przetwarzany.

![Oczekiwanie na przegląd](publishing-to-the-app-store-images/image21.png "oczekiwania do przeglądu")

## <a name="tips-and-tricks"></a>Porady i wskazówki

### <a name="customize-the-ipa-location"></a>Dostosowywanie lokalizacji IPA

**MSBuild** właściwości `IpaPackageDir`, pozwala na dostosowywanie lokalizacji danych wyjściowych pliku IPA. Jeśli `IpaPackageDir` jest ustawiony w niestandardowej lokalizacji pliku IPA zostaną umieszczone w tej lokalizacji, zamiast domyślnego podkatalogu oznaczony sygnaturą czasową. Może to być przydatne w przypadku tworzenia automatycznych kompilacji, które korzystają z określonej ścieżki do katalogu działają poprawnie, takich jak używane dla kompilacji ciągłej integracji (CI).

Istnieje kilka możliwych sposobach korzystania z nowej właściwości. Na przykład, służący do wypełniania wyjściowego pliku IPA do starego katalog domyślny (tak jak Xamarin.iOS 9.6 i niższy), można ustawić `IpaPackageDir` właściwości `$(OutputPath)` przy użyciu jednej z poniższych metod. Oba podejścia są zgodne z wszystkie kompilacje ujednoliconego interfejsu API Xamarin.iOS, w tym kompilacje IDE, a także kompilacji z wiersza polecenia, które używają **msbuild** lub **mdtool**:

- Pierwsza opcja jest ustawiona `IpaPackageDir` właściwość w ramach `<PropertyGroup>` element **MSBuild** pliku. Na przykład można dodać następujące `<PropertyGroup>` do dolnej części pliku csproj projektu aplikacji systemu iOS (tuż przed zamykającym `</Project>` znaczników):

    ```xml
    <PropertyGroup>
      <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- Lepszym rozwiązaniem jest dodanie `<IpaPackageDir>` element w dół istniejące `<PropertyGroup>` , który odpowiada konfiguracji używane do tworzenia pliku IPA. Jest to lepsze, ponieważ przygotuje projektu dla zgodności w przyszłości planowane ustawienie na stronie właściwości projektu opcje IPA systemu iOS. Jeśli obecnie używasz `Release|iPhone` konfigurację, aby utworzyć plik IPA, grupy pełną zaktualizowana wartość właściwości może wyglądać podobnie do poniższego:

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone'">
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

Alternatywne technika **msbuild** kompilacji z wiersza polecenia jest dodanie `/p:` argument wiersza polecenia, aby ustawić `IpaPackageDir` właściwości. W takim przypadku należy pamiętać, że **msbuild** nie rozwija `$()` wyrażenia przekazanego w wierszu polecenia, więc nie jest możliwe użycie `$(OutputPath)` składni. Zamiast tego należy podać pełną ścieżkę.

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Lub następujące czynności na komputerze Mac:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

Przy użyciu Twojej dystrybucji kompilacja utworzona i archiwizowane, teraz można przystąpić do przesłania zgłoszenia do usługi iTunes Connect.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób konfigurowania, tworzenie i przesyłanie aplikacji systemu iOS w wersji na Store aplikacji.

## <a name="related-links"></a>Linki pokrewne

- [Portal dla deweloperów firmy Apple (Apple)](https://developer.apple.com/account/)
- [iTunes Connect (Apple)](https://itunesconnect.apple.com)
- [Wytyczne dotyczące App Store (Apple)](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Typowe odrzuceń aplikacji (Apple)](https://developer.apple.com/app-store/review/rejections/)
- [Praca z funkcjami w rozszerzeniu Xamarin.iOS](~/ios/deploy-test/provisioning/capabilities/index.md)
- [Praca z uprawnieniami w rozszerzeniu Xamarin.iOS](~/ios/deploy-test/provisioning/entitlements.md)
- [Konfigurowanie aplikacji w usłudze iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Ikony aplikacji w rozszerzeniu Xamarin.iOS](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Uruchom ekrany dla aplikacji platformy Xamarin.iOS](~/ios/app-fundamentals/images-icons/launch-screens.md)
- [Dokumentacja modułu ładującego aplikacji (Apple)](https://help.apple.com/itc/apploader/#/apdS673accdb)
