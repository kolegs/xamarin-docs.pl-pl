---
title: "Menedżer urządzeń Xamarin Android"
description: "Menedżer urządzeń Android Xamarin, obecnie w wersji zapoznawczej, zastępuje starszy Menedżer urządzeń firmy Google. W tym przewodniku wyjaśniono, jak utworzyć i skonfigurować Android urządzeń wirtualnych (urządzeń Avd), które emulują urządzeń z systemem Android przy użyciu Menedżera urządzeń Android Xamarin. Te urządzenia wirtualnego umożliwia uruchamianie i testowanie aplikacji bez konieczności zależą od urządzenia fizycznego."
ms.topic: article
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 75dc51b5372ae4d8a322c29e39a585a547ab1963
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2018
---
# <a name="xamarin-android-device-manager"></a>Menedżer urządzeń Xamarin Android

_Menedżer urządzeń Android Xamarin, obecnie w wersji zapoznawczej, zastępuje starszy Menedżer urządzeń firmy Google. W tym przewodniku wyjaśniono, jak utworzyć i skonfigurować Android urządzeń wirtualnych (urządzeń Avd), które emulują urządzeń z systemem Android przy użyciu Menedżera urządzeń Android Xamarin. Te urządzenia wirtualnego umożliwia uruchamianie i testowanie aplikacji bez konieczności zależą od urządzenia fizycznego._

![Obecnie w wersji zapoznawczej](~/media/shared/preview.png)

 
## <a name="overview"></a>Omówienie

Po upewnieniu się, że jest włączona przyspieszanie sprzętowe (zgodnie z opisem w [przyspieszanie sprzętowe](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), następnym krokiem jest utworzenie urządzeń wirtualnych do użycia na potrzeby testowania i debugowania aplikacji. Menedżer urządzeń Xamarin Android służy do tworzenia urządzeń wirtualnych do użycia przez emulatora Android SDK.

Dlaczego chcesz za pomocą Menedżera urządzeń Xamarin Android, zamiast [Menedżera urządzeń Google](~/android/get-started/installation/android-emulator/google-emulator-manager.md)?
Począwszy od wersji narzędzia zestawu SDK systemu Android 26.0.1 Firma Google usunęła obsługę interfejsu użytkownika na podstawie AVD i zestawu SDK menedżerowie na rzecz ich nowych narzędzi interfejsu wiersza polecenia (interfejsu wiersza polecenia). Z powodu tej zmiany, należy użyć [Xamarin SDK Manager](~/android/get-started/installation/android-sdk.md) i Xamarin Android Menedżera urządzeń podczas aktualizacji do narzędzia zestawu SDK systemu Android 26.0.1 i nowsze (co jest niezbędne do rozwoju Oreo 8.0 dla systemu Android).


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W tym przewodniku opisano sposób instalacji i za pomocą Menedżera urządzeń Android Xamarin dla Visual Studio w systemie Windows (lub [dla komputerów Mac](?tabs=vsmac)):

[![Zrzut ekranu Menedżera urządzeń Android Xamarin w zakładce urządzenia](xamarin-device-manager-images/win/01-devices-dialog-sml.png)](xamarin-device-manager-images/win/01-devices-dialog.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W tym przewodniku wyjaśniono, jak zainstalować i używać Menedżera urządzeń Android Xamarin dla Visual Studio dla komputerów Mac (lub [dla systemu Windows](?tabs=vswin)):

[![Zrzut ekranu Menedżera urządzeń Android Xamarin w zakładce urządzenia](xamarin-device-manager-images/mac/01-devices-dialog-sml.png)](xamarin-device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Ten przewodnik dotyczy tylko programu Visual Studio dla komputerów Mac.
Program Xamarin Studio jest niezgodny przy użyciu Menedżera urządzeń Android Xamarin.

-----

Za pomocą Menedżera urządzenia Android Xamarin, aby utworzyć i skonfigurować *urządzeń wirtualnych z systemem Android* (urządzeń Avd) uruchomionymi [emulatora Android SDK](~/android/deploy-test/debugging/android-sdk-emulator/index.md).
Każdy AVD jest konfiguracji emulatora, która symuluje fizycznego urządzenia z systemem Android. Pozwala na uruchamianie i testowanie aplikacji w różnych konfiguracji, które symulować różne fizycznego urządzenia z systemem Android. Menedżer urządzeń Xamarin Android zastępuje autonomiczny firmy Google Menedżera AVD, (która jest przestarzała).

W tym przewodniku dowiesz się, jak zainstalować i uruchomić Menedżera urządzeń systemu Android. Dowiesz się sposobu tworzenia, zduplikowany, dostosowywanie i uruchomić urządzenia wirtualne. W tym przewodniku wyjaśniono również, jak skonfigurować właściwości dla każdego urządzenia wirtualnego (na przykład poziom interfejsu API, Procesora, pamięci i rozpoznawania), aby włączyć lub wyłączyć symulowane czujników, takich jak przyspieszeniomierza, GPS orientację i czujnik oświetlenia oraz konfigurowanie odpowiedniego typu sprzętu Akceleracja używany przez urządzenie wirtualne.

## <a name="requirements"></a>Wymagania

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby użyć Menedżera urządzeń Xamarin Android, potrzebne następujące elementy:

-   Wymagany jest program Visual Studio 2017 wersji 15.5 lub nowszej. Program Visual Studio Community edition i wyższe jest obsługiwany.

-   Xamarin dla Visual Studio w wersji 4.8 lub nowszej. Aby uzyskać informacje o aktualizowaniu Xamarin, zobacz [zmienić kanału aktualizacji](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/).

-   Najnowszą wersję [Instalator Menedżera urządzeń Xamarin](https://go.microsoft.com/fwlink/?linkid=865528) dla systemu Windows.

-   **Zestaw SDK systemu android** &ndash; musi być zainstalowany zestaw SDK systemu Android (zobacz [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md)), a musi być zainstalowana wersja narzędzia zestawu SDK 26.0, zgodnie z objaśnieniem w następnej sekcji. Należy zainstalować zestaw SDK systemu Android w następującej lokalizacji (Jeśli nie jest już zainstalowany): **C:\\Program Files (x86)\\Android\\zestawu sdk systemu android**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Program Visual Studio dla komputerów Mac 7.4 lub nowszej.

-   Najnowszą wersję [Instalator Menedżera urządzeń Xamarin](https://go.microsoft.com/fwlink/?linkid=865527) dla macOS.

-   **Zestaw SDK systemu android** &ndash; Android SDK 8.0 (26 interfejsu API) lub nowszym należy zainstalować za pomocą Menedżera zestawu SDK.

-----

 
## <a name="installing-the-device-manager"></a>Instalowanie Menedżera urządzeń

Aby zainstalować Menedżera urządzeń Xamarin Android, wykonaj następujące kroki:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Pobierz [Instalator Menedżera urządzeń Xamarin](https://go.microsoft.com/fwlink/?linkid=865528) dla systemu Windows.

2. Kliknij dwukrotnie **Xamarin.DeviceManager.msi** i postępuj zgodnie z instrukcjami instalacji: 

    ![Kreator konfiguracji Menedżera urządzeń Xamarin Android](xamarin-device-manager-images/win/30-installer.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Pobierz [Instalator Menedżera urządzeń Xamarin](https://go.microsoft.com/fwlink/?linkid=865527) dla macOS.

2. Kliknij dwukrotnie **AndroidDevices.pkg** i postępuj zgodnie z instrukcjami instalacji: 

    [![Kreator konfiguracji Menedżera urządzeń Xamarin Android](xamarin-device-manager-images/mac/30-installer-sml.png)](xamarin-device-manager-images/mac/30-installer.png#lightbox)

-----

 
## <a name="launching-the-device-manager"></a>Uruchamianie Menedżera urządzeń

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W programie Visual Studio 15,6 wersji zapoznawczej 3 i nowszych, można uruchomić Menedżera urządzeń Xamarin Android z **narzędzia** menu. Jeśli używasz programu Visual Studio 15,6 Preview 3 lub później, uruchom Menedżera urządzeń, klikając **Narzędzia > Android Emulator Manager**:

[![Uruchamianie z menu Narzędzia](xamarin-device-manager-images/win/04-tools-menu-sml.png)](xamarin-device-manager-images/win/04-tools-menu.png#lightbox)

Jeśli używasz starszej wersji programu Visual Studio z systemu Windows musi być uruchamiany Menedżer urządzeń Xamarin Android **Start** menu.

![Xamarin Menedżera urządzeń systemu Android w start menu](xamarin-device-manager-images/win/31-start-menu.png)

Kliknij prawym przyciskiem myszy **Menedżera urządzeń Xamarin Android** i wybierz **więcej > Uruchom jako administrator**. Jeśli po uruchomieniu następujących okna dialogowego błędu, zobacz [Rozwiązywanie problemów](#troubleshooting) sekcji instrukcje obejście problemu:

![Błąd wystąpienia zestawu SDK systemu android](xamarin-device-manager-images/win/32-sdk-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W programie Visual Studio for Mac 7.6 Preview 3 (obecnie w kanału alfa) lub nowszym, można uruchomić Menedżera urządzeń Xamarin Android wybierając **Narzędzia > Menedżera emulatora**:

[![Uruchamianie z menu Narzędzia](xamarin-device-manager-images/mac/16-tools-menu-sml.png)](xamarin-device-manager-images/mac/16-tools-menu.png#lightbox)

Jeśli używasz starszej wersji programu Visual Studio dla komputerów Mac, Menedżer urządzeń Xamarin Android musi być uruchamiany niezależnie. Zlokalizuj **urządzeń z systemem Android** w **aplikacji** folder i kliknij dwukrotnie, aby uruchomić go:

[![Menedżer urządzeń Xamarin Android lokalizacji w wyszukiwaniu](xamarin-device-manager-images/mac/31-location-in-finder-sml.png)](xamarin-device-manager-images/mac/31-location-in-finder.png#lightbox)


-----

Przed użyciem Menedżera urządzeń systemu Android, należy zainstalować wersję narzędzia zestawu SDK systemu Android 26.0.0 lub nowszym. Jeśli zestaw SDK systemu Android narzędzi 26.0.0 lub nowszy nie jest zainstalowany, zostanie wyświetlony po uruchomieniu tego okna dialogowego błędu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Okno dialogowe android SDK wystąpienia błędu](xamarin-device-manager-images/win/02-sdk-instance-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Okno dialogowe android SDK wystąpienia błędu](xamarin-device-manager-images/mac/02-sdk-instance-error.png)

-----

Jeśli widzisz tego okna dialogowego błędu, kliknij przycisk **OK** aby otworzyć narzędzie Android SDK Manager. W systemie Android SDK Manager kliknij przycisk **narzędzia** karcie i zainstaluj **narzędzia zestawu SDK systemu Android 26.0.2** lub nowszym, **Android SDK narzędzi platformy 26.0.0** lub nowszym i  **Android SDK — narzędzia kompilacji 26.0.0** (lub nowsze):


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Instalowanie narzędzia zestawu SDK systemu Android 26.0](xamarin-device-manager-images/win/03-sdk-tools-sml.png)](xamarin-device-manager-images/win/03-sdk-tools.png#lightbox)

Po zainstalowaniu tych pakietów można zamknąć SDK Manager i ponownie uruchom Menedżera urządzeń systemu Android.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Instalowanie narzędzia zestawu SDK systemu Android 26.0](xamarin-device-manager-images/mac/03-sdk-tools-sml.png)](xamarin-device-manager-images/mac/03-sdk-tools.png#lightbox)

-----

 
## <a name="main-screen"></a>Ekran główny

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Uruchamianie Menedżera urządzeń Android przedstawia informacje ekranu, który wyświetla wszystkie aktualnie skonfigurowane urządzenia wirtualnego. Dla każdego urządzenia **nazwa**, **systemu operacyjnego** (poziom interfejsu API systemu Android), **Procesora**, **pamięci** rozmiar i rozdzielczość ekranu są wyświetlane:

[![Lista zainstalowanych urządzeń i ich parametry](xamarin-device-manager-images/win/05-installed-list-sml.png)](xamarin-device-manager-images/win/05-installed-list.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Uruchamianie Menedżera urządzeń Android przedstawia informacje ekranu, który wyświetla wszystkie aktualnie skonfigurowane urządzenia wirtualnego. Dla każdego urządzenia **nazwa**, **obrazu systemu** (poziom interfejsu API systemu Android), **Procesora**, **pamięci** rozmiar i rozdzielczość ekranu są wyświetlane:

[![Lista zainstalowanych urządzeń i ich parametry](xamarin-device-manager-images/mac/05-devices-list-sml.png)](xamarin-device-manager-images/mac/05-devices-list.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po kliknięciu urządzenie na liście **Start** przycisk pojawia się po prawej stronie. Możesz kliknąć **Start** przycisk, aby uruchomić emulatora do tego urządzenia wirtualnego:

[![Przycisk Start obrazu urządzenia](xamarin-device-manager-images/win/06-start-button-sml.png)](xamarin-device-manager-images/win/06-start-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Kliknij przycisk **odtwarzanie** przycisk, aby uruchomić emulatora z urządzeniem wirtualnym wybranych przez użytkownika:
 
[![Przycisk Start obrazu urządzenia](xamarin-device-manager-images/mac/06-start-button-sml.png)](xamarin-device-manager-images/mac/06-start-button.png#lightbox)
 
-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po uruchomieniu emulatora z wybranego urządzenia wirtualnego **Start** przycisku zmienia się na **zatrzymać** przycisku, który umożliwia zatrzymanie emulator:

[![Zatrzymaj przycisku uruchomionych urządzenia](xamarin-device-manager-images/win/07-stop-button-sml.png)](xamarin-device-manager-images/win/07-stop-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po emulator rozpoczyna się od wybranego urządzenia wirtualnego **odtwarzanie** przycisku zmienia się na **zatrzymać** przycisku, który umożliwia zatrzymanie emulator:
 
[![Zatrzymaj przycisku uruchomionych urządzenia](xamarin-device-manager-images/mac/07-stop-button-sml.png)](xamarin-device-manager-images/mac/07-stop-button.png#lightbox)
 
-----

 
### <a name="new-device"></a>Nowe urządzenie

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby utworzyć nowe urządzenie, kliknij przycisk **nowy** przycisk (znajdujący się po prawej stronie górnej części ekranu):

[![Nowy przycisk służący do tworzenia nowego urządzenia](xamarin-device-manager-images/win/08-new-button-sml.png)](xamarin-device-manager-images/win/08-new-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby utworzyć nowe urządzenie, kliknij przycisk **nowe urządzenie** przycisk (znajdujący się po prawej stronie górnej części ekranu):
 
[![Nowy przycisk służący do tworzenia nowego urządzenia](xamarin-device-manager-images/mac/08-new-button-sml.png)](xamarin-device-manager-images/mac/08-new-button.png#lightbox)
 
-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kliknięcie przycisku **nowy** uruchamia **nowe urządzenie** ekranu:

[![Nowe urządzenie ekranu Menedżera urządzeń](xamarin-device-manager-images/win/09-new-device-editor-sml.png)](xamarin-device-manager-images/win/09-new-device-editor.png#lightbox)

Aby skonfigurować nowe urządzenie w **nowe urządzenie** ekranu, wykonaj następujące kroki:

1. Wybierz urządzenie fizyczne emulować klikając **urządzenia** menu rozwijanego:

    [![Menu rozwijane urządzenia](xamarin-device-manager-images/win/10-device-menu-sml.png)](xamarin-device-manager-images/win/10-device-menu.png#lightbox)

2. Wybierz obraz systemu do użycia z tym urządzeniem wirtualnym, klikając **obrazu systemu** menu rozwijanego. W tym menu znajdują się obrazy zainstalowany system w obszarze **zainstalowana**. **Pobierz** sekcji przedstawiono obrazów systemu, które są aktualnie niedostępne na komputerze deweloperskim, ale mogą być instalowane automatycznie:

    [![Menu rozwijane obrazu systemu](xamarin-device-manager-images/win/11-system-image-menu-sml.png)](xamarin-device-manager-images/win/11-system-image-menu.png#lightbox)

3. Nadaj nazwę nowego urządzenia. W poniższym przykładzie, nowego urządzenia o nazwie **węzła 5 interfejsu API 25**:

    [![Nazewnictwo nowego urządzenia](xamarin-device-manager-images/win/12-device-name-sml.png)](xamarin-device-manager-images/win/12-device-name.png#lightbox)

4. Edytuj wszystkie właściwości, które należy zmodyfikować. Aby zmienić właściwości, zobacz [właściwości profilu](#properties) dalszej części tego przewodnika.

5. Dodaj wszelkie dodatkowe właściwości, które należy jawnie ustawić. **Nowe urządzenie** ekranie są wyświetlane tylko właściwości najbardziej powszechnie zmodyfikowane, ale możesz kliknąć **Dodaj właściwość** menu rozwijanego (w lewym dolnym rogu) można dodać dodatkowe właściwości. W poniższym przykładzie `hw.lcd.backlight` dodawana właściwość:

    [![Dodawanie menu rozwijanego właściwości](xamarin-device-manager-images/win/13-add-property-menu-sml.png)](xamarin-device-manager-images/win/13-add-property-menu.png#lightbox)

6. Kliknij przycisk **Utwórz** przycisk (prawym dolnym rogu), aby utworzyć nowe urządzenie:

    ![Tworzenie przycisków](xamarin-device-manager-images/win/14-create-button.png)

7. Może spowodować, że **akceptacji licencji** ekranu. Kliknij przycisk **Akceptuj** Jeśli akceptujesz postanowienia licencyjne:

    ![Ekran akceptacji licencji](xamarin-device-manager-images/win/15-license-acceptance.png)

8. Menedżer urządzeń Android dodaje nowe urządzenie na liście zainstalowanych urządzeń wirtualnych z **tworzenie** wskaźnik postępu podczas tworzenia urządzenia:

    [![Postęp tworzenia wskaźnika](xamarin-device-manager-images/win/16-creating-the-device-sml.png)](xamarin-device-manager-images/win/16-creating-the-device.png#lightbox)

9. Po zakończeniu procesu tworzenia nowego urządzenia jest wyświetlane na liście zainstalowanych urządzeń wirtualnych z **Start** przycisk Gotowe do uruchomienia:

   [![Nowo utworzona urządzenia, gotowy do uruchomienia](xamarin-device-manager-images/win/17-created-device-sml.png)](xamarin-device-manager-images/win/17-created-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Kliknięcie przycisku **nowe urządzenie** uruchamia **nowe urządzenie** ekranu:

[![Nowe urządzenie ekranu Menedżera urządzeń](xamarin-device-manager-images/mac/09-new-device-editor-sml.png)](xamarin-device-manager-images/mac/09-new-device-editor.png#lightbox)

Wykonaj następujące kroki, aby skonfigurować nowe urządzenie w **nowe urządzenie** ekranu:

1. Wybierz urządzenie fizyczne emulować klikając **urządzenia** menu rozwijanego:

    [![Menu rozwijane urządzenia](xamarin-device-manager-images/mac/10-device-menu-sml.png)](xamarin-device-manager-images/mac/10-device-menu.png#lightbox)

2. Wybierz obraz systemu do użycia z tym urządzeniem wirtualnym, klikając **obrazu systemu** menu rozwijanego. W tym menu znajdują się obrazy zainstalowany system w obszarze **zainstalowana**. **Pobierz** sekcja (jeśli są pokazywane) zawiera obrazy systemu, które są aktualnie niedostępne na komputerze deweloperskim, ale mogą być instalowane automatycznie:

    [![Menu rozwijane obrazu systemu](xamarin-device-manager-images/mac/11-system-image-menu-sml.png)](xamarin-device-manager-images/mac/11-system-image-menu.png#lightbox)

3. Nadaj nazwę nowego urządzenia. W poniższym przykładzie, nowego urządzenia o nazwie **węzła 5 X API 25**:

    [![Nazewnictwo nowego urządzenia](xamarin-device-manager-images/mac/12-device-name-sml.png)](xamarin-device-manager-images/mac/12-device-name.png#lightbox)

4. Edytuj wszystkie właściwości, które należy zmodyfikować. Aby zmienić właściwości, zobacz [właściwości profilu](#properties) dalszej części tego przewodnika.

5. Dodaj wszelkie dodatkowe właściwości, które należy jawnie ustawić. **Nowe urządzenie** ekranie są wyświetlane tylko właściwości najbardziej powszechnie zmodyfikowane, ale możesz kliknąć **Dodaj właściwość** menu rozwijanego (w lewym dolnym rogu) można dodać dodatkowe właściwości:

    [![Dodawanie menu rozwijanego właściwości](xamarin-device-manager-images/mac/13-add-property-menu-sml.png)](xamarin-device-manager-images/mac/13-add-property-menu.png#lightbox)

6. Możesz również kliknąć **niestandardowy** do definiowania nowych właściwości dla urządzenia:

    ![Tworzenie przycisków](xamarin-device-manager-images/mac/14-custom-button.png)

7. Kliknij przycisk **Utwórz** przycisk (prawym dolnym rogu), aby utworzyć nowe urządzenie:

    ![Tworzenie przycisków](xamarin-device-manager-images/mac/15-create-button.png)

8. Może spowodować, że **akceptacji licencji** ekranu. Kliknij przycisk **Akceptuj** Jeśli akceptujesz postanowienia licencyjne.

9. Podczas tworzenia urządzenia, Menedżer urządzeń Android dodaje nowe urządzenie na liście urządzeń z **tworzenie** wskaźnik postępu:

    [![Postęp tworzenia wskaźnika](xamarin-device-manager-images/mac/17-creating-the-device-sml.png)](xamarin-device-manager-images/mac/17-creating-the-device.png#lightbox)

10. Po zakończeniu procesu tworzenia nowego urządzenia jest wyświetlany na liście urządzeń z **odtwarzanie** przycisk Gotowe do uruchomienia:

   [![Nowo utworzona urządzenia, gotowy do uruchomienia](xamarin-device-manager-images/mac/18-created-device-sml.png)](xamarin-device-manager-images/mac/18-created-device.png#lightbox)

-----


<a name="device-edit" />


### <a name="edit-device"></a>Edytowanie urządzenia

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby edytować istniejące urządzenie wirtualne, wybierz urządzenie, a następnie kliknij przycisk **Edytuj** przycisk (znajdujący się w prawym górnym rogu ekranu):

[![Przycisk modyfikowania nowe urządzenie Edytuj](xamarin-device-manager-images/win/19-edit-button-sml.png)](xamarin-device-manager-images/win/19-edit-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby edytować istniejące urządzenie wirtualne, wybierz **dodatkowe opcje** menu rozwijanego (koło zębate ikonę) i wybierz **Edytuj**:
 
[![Edytuj zaznaczenia menu modyfikowania nowego urządzenia](xamarin-device-manager-images/mac/19-edit-button-sml.png)](xamarin-device-manager-images/mac/19-edit-button.png#lightbox)
 
-----

Kliknięcie przycisku **Edytuj** uruchamia edytor urządzenia dla wybranego urządzenia wirtualnego:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Ekran urządzenia edytora](xamarin-device-manager-images/win/20-device-editor-sml.png)](xamarin-device-manager-images/win/20-device-editor.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
[![Ekran urządzenia edytora](xamarin-device-manager-images/mac/20-device-editor-sml.png)](xamarin-device-manager-images/mac/20-device-editor.png#lightbox)
 
-----

**Edytor urządzenia** ekranie są wyświetlane właściwości urządzenia wirtualnego w pierwszej kolumnie z odpowiednimi wartościami każdej właściwości w drugiej kolumnie. Po wybraniu właściwości szczegółowy opis tej właściwości jest wyświetlana po prawej stronie.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Na przykład w poniższym zrzucie ekranu `hw.lcd.density` właściwość został zmieniony z **420** do **240**:

[![Przykład edycji urządzenia](xamarin-device-manager-images/win/21-device-editing-sml.png)](xamarin-device-manager-images/win/21-device-editing.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Na przykład w poniższym zrzucie ekranu `hw.lcd.density` właściwości została zmieniona z **320** do **240** i `hw.ramSize` jest zmieniana na **768**:
 
[![Przykład edycji urządzenia](xamarin-device-manager-images/mac/21-device-editing-sml.png)](xamarin-device-manager-images/mac/21-device-editing.png#lightbox)
 
-----

Po dokonaniu zmiany niezbędną konfigurację, kliknij przycisk **zapisać** przycisku.
Aby uzyskać więcej informacji na temat zmiany właściwości urządzenia wirtualnego, zobacz [właściwości profilu](#properties) dalszej części tego przewodnika.


 
### <a name="additional-options"></a>Dodatkowe opcje

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dostępne są dodatkowe opcje Praca z urządzeniami &hellip; menu w prawym górnym rogu:

[![Lokalizacja dodatkowe opcje menu](xamarin-device-manager-images/win/22-overflow-menu-sml.png)](xamarin-device-manager-images/win/22-overflow-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Dodatkowe opcje do pracy z urządzeniem są dostępne z menu rozwijanego znajdujący się na lewo od **odtwarzanie** przycisk:

[![Lokalizacja dodatkowe opcje menu](xamarin-device-manager-images/mac/22-overflow-menu-sml.png)](xamarin-device-manager-images/mac/22-overflow-menu.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dodatkowe opcje menu zawiera następujące elementy:

-   **Zduplikowana i edytować** &ndash; duplikaty aktualnie wybrane urządzenie i otwarcie go w **nowe urządzenie** ekranu z różnych unikatową nazwę. Na przykład wybranie **VisualStudio_android 23_x86_phone** i klikając **zduplikowany i edytować** dołącza do nazwy licznika:

    [![Duplikat i Edycja ekranu](xamarin-device-manager-images/win/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **Wyświetlanie w Eksploratorze** &ndash; powoduje otwarcie okna Eksploratora Windows w folderze, który zawiera pliki dla urządzenia wirtualnego. Na przykład wybranie **węzła 5 X API 25** i klikając **ujawnić w Eksploratorze** powoduje otwarcie okna, podobnie do następującej:

    [![Kliknięcie przycisku ujawniania w Eksploratorze wyników](xamarin-device-manager-images/win/24-reveal-in-explorer-sml.png)](xamarin-device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **Resetowanie do ustawień fabrycznych** &ndash; resetuje wybranego urządzenia do domyślnych ustawień, wymazywanie użytkownika zmian stanu wewnętrznego urządzenia została uruchomiona. Ta zmiana nie zmienia zmiany wprowadzone do urządzenia wirtualnego podczas tworzenia i edytowania. Pojawi się okno dialogowe z monitu, że resetowania nie można cofnąć. Kliknij przycisk **czyszczenie danych użytkownika** o potwierdzenie resetowania.

-   **Usuń** &ndash; powoduje trwałe usunięcie wybranego urządzenia wirtualnego.
    Pojawi się okno dialogowe z monitu, że usunięcie urządzenia nie można cofnąć. Kliknij przycisk **usunąć** Jeśli masz pewność, że chcesz usunąć urządzenie.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Dodatkowe opcje menu zawiera następujące elementy:

-   **Edytuj** &ndash; otwiera urządzenia obecnie zaznaczonego w edytorze urządzenia, zgodnie z wcześniejszym opisem w [Edytuj urządzenia](#device-edit).

-   **Zduplikowana i edytować** &ndash; duplikaty aktualnie wybrane urządzenie i otwarcie go w **nowe urządzenie** ekranu z różnych unikatową nazwę.
    Na przykład wybranie **węzła 5 X API 25** i klikając **zduplikowany i edytować** dołącza do nazwy licznika:

    [![Duplikat i Edycja ekranu](xamarin-device-manager-images/mac/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **Wyświetlanie w wyszukiwarce** &ndash; powoduje otwarcie okna wyszukiwania macOS w folderze, który zawiera pliki dla urządzenia wirtualnego. Na przykład wybranie **węzła 5 X API 25** i klikając **ujawnić w wyszukiwarce** powoduje otwarcie okna, podobnie do następującej:

    [![Kliknięcie przycisku ujawniania w Eksploratorze wyników](xamarin-device-manager-images/mac/24-reveal-in-finder-sml.png)](xamarin-device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **Resetowanie do ustawień fabrycznych** &ndash; resetuje wybranego urządzenia do domyślnych ustawień, wymazywanie użytkownika zmian stanu wewnętrznego urządzenia została uruchomiona. Ta zmiana nie zmienia zmiany wprowadzone do urządzenia wirtualnego podczas tworzenia i edytowania. Pojawi się okno dialogowe z monitu, że resetowania nie można cofnąć. Kliknij przycisk **czyszczenie danych użytkownika** o potwierdzenie resetowania.

-   **Usuń** &ndash; powoduje trwałe usunięcie wybranego urządzenia wirtualnego.
    Zostanie wyświetlone okno dialogowe z monitu, że usunięcie urządzenia nie można cofnąć. " Kliknij przycisk **usunąć** Jeśli masz pewność, że chcesz usunąć urządzenie.

-----

<a name="properties" />
 

## <a name="profile-properties"></a>Właściwości profilu

**Nowe urządzenie** i **Edytuj urządzenia** ekrany listy właściwości urządzenia wirtualnego w pierwszej kolumnie z odpowiednimi wartościami każdej właściwości w drugiej kolumnie. Po wybraniu właściwości szczegółowy opis tej właściwości jest wyświetlana po prawej stronie. Można zmodyfikować jego *właściwości profilu sprzętu* i jego *AVD właściwości*.
Właściwości profilu sprzętu (takich jak `hw.ramSize` i `hw.accelerometer`) opisano charakterystyki fizycznej emulowanej urządzenia. Te właściwości obejmują rozmiaru ekranu, ilość dostępnej pamięci RAM, czy występuje przyspieszeniomierza. AVD właściwości określ operacji AVD, po uruchomieniu. Na przykład aby określić, jak AVD używa karty graficznej komputer deweloperski do renderowania można skonfigurować AVD właściwości.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Możesz zmienić właściwości, korzystając z poniższych wskazówek:

-   Aby zmienić właściwości typu boolean, kliknij znacznik wyboru z prawej strony właściwości typu boolean:

    ![Zmiana właściwości typu boolean](xamarin-device-manager-images/win/25-boolean-value.png)

-   Aby zmienić *wyliczenia* właściwość (wyliczany), kliknij strzałkę w dół na prawo od właściwości i wybierz nową wartość.

    ![Zmiana właściwości wyliczenia](xamarin-device-manager-images/win/27-enum-value.png)

-   Aby zmienić właściwość ciąg lub liczba całkowita, kliknij dwukrotnie bieżące ustawienie ciąg lub liczba całkowita w kolumnie wartość i wprowadź nową wartość.

    ![Zmiana właściwością liczby całkowitej.](xamarin-device-manager-images/win/26-integer-value.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Możesz zmienić właściwości, korzystając z poniższych wskazówek:

-   Aby zmienić właściwości typu boolean, kliknij znacznik wyboru z prawej strony właściwości typu boolean:

    ![Zmiana właściwości typu boolean](xamarin-device-manager-images/mac/25-boolean-value.png)

-   Aby zmienić *wyliczenia* właściwość (wyliczany), kliknij menu rozwijane z prawej strony właściwości i wybierz nową wartość.

    ![Zmiana właściwości wyliczenia](xamarin-device-manager-images/mac/27-enum-value.png)

-   Aby zmienić właściwość ciąg lub liczba całkowita, kliknij dwukrotnie bieżące ustawienie ciąg lub liczba całkowita w kolumnie wartość i wprowadź nową wartość.

    ![Zmiana właściwością liczby całkowitej.](xamarin-device-manager-images/mac/26-integer-value.png)


-----

W poniższej tabeli przedstawiono szczegółowy opis właściwości wymienionych w **nowe urządzenie** i **Edytor urządzenia** ekrany:

[!include[](~/android/includes/emulator-properties.md)]

Aby uzyskać więcej informacji na temat tych właściwości, zobacz [właściwości profilu sprzętu](https://developer.android.com/studio/run/managing-avds.html#hpproperties).


<a name="troubleshooting" />

## <a name="troubleshooting"></a>Rozwiązywanie problemów

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Poniżej opisano typowe problemy Menedżera urządzeń Xamarin Android i rozwiązania:

### <a name="android-sdk-in-non-standard-location"></a>Zestaw SDK systemu android w lokalizacji niestandardowej

Zazwyczaj zestaw SDK systemu Android jest instalowany w następującej lokalizacji:

**C:\\Program Files (x86)\\Android\\zestawu sdk systemu android**

Jeśli zestaw SDK nie jest zainstalowany w tej lokalizacji, błąd ten może wystąpić po uruchomieniu:

![Błąd wystąpienia zestawu SDK systemu android](xamarin-device-manager-images/win/32-sdk-error.png)

Aby obejść ten problem, wykonaj następujące czynności:

1. Na pulpicie systemu Windows, przejdź do **C:\\użytkowników\\*username*\\AppData\\mobilny\\XamarinDeviceManager**:

    ![Lokalizacja pliku dziennika Xamarin Menedżera urządzeń](xamarin-device-manager-images/win/33-log-files.png)

2. Kliknij dwukrotnie, aby otworzyć jeden z plików dziennika i Znajdź **ścieżkę pliku konfiguracyjnego**. Na przykład:

    [![Ścieżka pliku konfiguracji w pliku dziennika](xamarin-device-manager-images/win/34-config-file-path-sml.png)](xamarin-device-manager-images/win/34-config-file-path.png#lightbox)

3. Przejdź do tej lokalizacji i kliknij dwukrotnie **user.config** go otworzyć. 

4. W **user.config**, zlokalizuj  **&lt;ustawienia użytkownika&gt;**  element i Dodaj **AndroidSdkPath** do niej atrybut. Ten atrybut ustawiony na ścieżkę zainstalowanym zestawu SDK systemu Android na komputerze, a następnie zapisz plik. Na przykład  **&lt;ustawienia użytkownika&gt;**  będzie wyglądać podobnie do następującej, jeśli zainstalowano zestaw SDK systemu Android w **C:\\programy\\Android\\zestawuSDK**:
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

Po wprowadzeniu tej zmiany do **user.config**, można uruchomić Menedżera urządzeń Xamarin Android.

### <a name="generating-a-bug-report"></a>Generowanie raport o usterce

Jeśli napotkasz problem przy użyciu platformy Xamarin Android Menedżera urządzeń którego nie można rozwiązać przy użyciu powyższego wskazówki dotyczące rozwiązywania problemów, klikając prawym przyciskiem myszy pasek tytułu i wybierając pliku raport o usterce **wygenerować raport o usterce**:

![Lokalizacja elementu menu dla składanie raport o usterce](xamarin-device-manager-images/win/35-bug-report.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Obecnie nie istnieją żadne znane problemy/obejścia dla platformy Xamarin Android Menedżera urządzeń w programie Visual Studio dla komputerów Mac. 

### <a name="generating-a-bug-report"></a>Generowanie raport o usterce

Jeśli napotkasz problem, klikając pliku raport o usterce **Pomoc > Generowanie raportu o usterce**:

![Lokalizacja elementu menu dla składanie raport o usterce](xamarin-device-manager-images/mac/35-bug-report.png)

-----

 
 
## <a name="summary"></a>Podsumowanie

W tym przewodniku wprowadzono dostępne w programie Visual Studio Menedżera urządzeń Android Xamarin dla systemów Mac i Xamarin dla Visual Studio. Go wyjaśniono zasadnicze funkcje, takie jak uruchamianie i zatrzymywanie emulatora systemu Android, wybierając wirtualnego urządzenia z systemem Android (AVD) do uruchomienia, tworzenie nowych wirtualnych urządzeń i jak edytować urządzenia wirtualnego. On również wyjaśniono sposób edycji właściwości profilu sprzętu dla dalszego dostosowania.


## <a name="related-links"></a>Linki pokrewne

- [Zmiany zestawu narzędzi Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Debugowanie za pomocą emulatora Android SDK](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
- [Informacje o wersji (Google) narzędzia zestawu SDK](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
