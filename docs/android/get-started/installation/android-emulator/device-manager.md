---
title: Zarządzanie urządzeniami wirtualnego przy użyciu Menedżera urządzeń systemu Android
description: W tym artykule wyjaśniono, jak utworzyć i skonfigurować Android urządzeń wirtualnych (urządzeń Avd), które emulują fizycznego urządzenia z systemem Android przy użyciu Menedżera urządzeń systemu Android. Te urządzenia wirtualnego umożliwia uruchamianie i testowanie aplikacji bez konieczności zależą od urządzenia fizycznego.
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: a7c1aeafd94d7e2639617cda13312ee8a09e2c94
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935331"
---
# <a name="managing-virtual-devices-with-the-android-device-manager"></a>Zarządzanie urządzeniami wirtualnego przy użyciu Menedżera urządzeń systemu Android

_W tym artykule wyjaśniono, jak utworzyć i skonfigurować Android urządzeń wirtualnych (urządzeń Avd), które emulują fizycznego urządzenia z systemem Android przy użyciu Menedżera urządzeń systemu Android. Te urządzenia wirtualnego umożliwia uruchamianie i testowanie aplikacji bez konieczności zależą od urządzenia fizycznego._

## <a name="overview"></a>Omówienie

Po upewnieniu się, że jest włączona przyspieszanie sprzętowe (zgodnie z opisem w [przyspieszanie sprzętowe wydajności emulatora](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), następnym krokiem jest użycie _Menedżera urządzeń Android_ (zwaną także _Menedżera urządzeń Xamarin Android_) do tworzenia urządzeń wirtualnych, które służą do testowania i debugowania aplikacji.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W tym artykule opisano sposób tworzenia, zduplikowany, dostosowywanie i uruchomić urządzenia wirtualne z systemem Android przy użyciu Menedżera urządzeń systemu Android.

[![Zrzut ekranu Menedżera urządzeń systemu Android w zakładce urządzenia](device-manager-images/win/01-devices-dialog-sml.png)](device-manager-images/win/01-devices-dialog.png#lightbox)

Za pomocą Menedżera urządzeń systemu Android, aby utworzyć i skonfigurować _urządzeń wirtualnych z systemem Android_ (urządzeń Avd), które działają w [emulatora systemu Android](~/android/deploy-test/debugging/debug-on-emulator.md).
Każdy AVD jest konfiguracji emulatora, która symuluje fizycznego urządzenia z systemem Android. Pozwala na uruchamianie i testowanie aplikacji w różnych konfiguracji, które symulować różne fizycznego urządzenia z systemem Android.

## <a name="requirements"></a>Wymagania

Aby użyć Menedżera urządzeń systemu Android, potrzebne następujące elementy:

-   Wymagany jest program Visual Studio 2017 wersji 15.7 lub nowszej. Obsługiwane są wersje Visual Studio Community, Professional i Enterprise.

-   Visual Studio Tools dla platformy Xamarin wersji 4.9 lub nowszej.

-   Musi być zainstalowany zestaw SDK systemu Android (zobacz [Definiowanie zestawu Android SDK dla platformy Xamarin.Android](~/android/get-started/installation/android-sdk.md)) i narzędzia zestawu SDK w wersji 26.1.1 lub nowszym należy zainstalować zgodnie z objaśnieniem w następnej sekcji. Należy zainstalować zestaw SDK systemu Android w następującej lokalizacji (Jeśli nie jest już zainstalowany): **C:\\Program Files (x86)\\Android\\zestawu sdk systemu android**.


## <a name="launching-the-device-manager"></a>Uruchamianie Menedżera urządzeń

Uruchamianie Menedżera urządzeń systemu Android z **narzędzia** menu, klikając **Narzędzia > Android > Menedżera urządzeń Android**:

[![Uruchamianie z menu Narzędzia](device-manager-images/win/04-tools-menu-sml.png)](device-manager-images/win/04-tools-menu.png#lightbox)

Jeśli po uruchomieniu następujących okna dialogowego błędu, zobacz [Rozwiązywanie problemów z systemem Android Emulator](~/android/get-started/installation/android-emulator/troubleshooting.md) instrukcje obejście problemu:

![Błąd wystąpienia zestawu SDK systemu android](device-manager-images/win/32-sdk-error.png)

Przed użyciem Menedżera urządzeń systemu Android, należy zainstalować wersję narzędzia zestawu SDK systemu Android 26.1.1 lub nowszym. Jeśli zestaw SDK systemu Android narzędzi 26.1.1 lub nowszy nie jest zainstalowany, to okno dialogowe błędu może zostać wyświetlony po uruchomieniu:

![Okno dialogowe android SDK wystąpienia błędu](device-manager-images/win/02-sdk-instance-error.png)

Jeśli widzisz tego okna dialogowego błędu, kliknij przycisk **Otwórz SDK Manager** aby otworzyć narzędzie Android SDK Manager. W systemie Android SDK Manager kliknij przycisk **narzędzia** karcie i zainstalowane następujące składniki:

-   **Narzędzia zestawu SDK systemu android 26.1.1** lub nowszy 
-   **Android SDK narzędzi platformy 27.0.1** lub nowszy  
-   **Android SDK — narzędzia kompilacji 27.0.3** lub nowszy

Te pakiety powinny być wyświetlane z **zainstalowana** stanu, jak pokazano na poniższym zrzucie ekranu:

[![Instalowanie narzędzia zestawu SDK systemu Android 27.0](device-manager-images/win/03-sdk-tools-sml.png)](device-manager-images/win/03-sdk-tools.png#lightbox)

Po zainstalowaniu tych pakietów można zamknąć SDK Manager i ponownie uruchom Menedżera urządzeń systemu Android.

## <a name="main-screen"></a>Ekran główny

Uruchamianie Menedżera urządzeń Android przedstawia informacje ekranu, który wyświetla wszystkie aktualnie skonfigurowane urządzenia wirtualnego. Dla każdego urządzenia **nazwa**, **systemu operacyjnego** (poziom interfejsu API systemu Android), **Procesora**, **pamięci** rozmiar i rozdzielczość ekranu są wyświetlane:

[![Lista zainstalowanych urządzeń i ich parametry](device-manager-images/win/05-installed-list-sml.png)](device-manager-images/win/05-installed-list.png#lightbox)

Po kliknięciu urządzenie na liście **Start** przycisk pojawia się po prawej stronie. Możesz kliknąć **Start** przycisk, aby uruchomić emulatora do tego urządzenia wirtualnego:

[![Przycisk Start obrazu urządzenia](device-manager-images/win/06-start-button-sml.png)](device-manager-images/win/06-start-button.png#lightbox)

Po uruchomieniu emulatora z wybranego urządzenia wirtualnego **Start** przycisku zmienia się na **zatrzymać** przycisku, który umożliwia zatrzymanie emulator:

[![Zatrzymaj przycisku uruchomionych urządzenia](device-manager-images/win/07-stop-button-sml.png)](device-manager-images/win/07-stop-button.png#lightbox)

### <a name="new-device"></a>Nowe urządzenie

Aby utworzyć nowe urządzenie, kliknij przycisk **nowy** przycisk (znajdujący się po prawej stronie górnej części ekranu):

[![Nowy przycisk służący do tworzenia nowego urządzenia](device-manager-images/win/08-new-button-sml.png)](device-manager-images/win/08-new-button.png#lightbox)

Kliknięcie przycisku **nowy** uruchamia **nowe urządzenie** ekranu:

[![Nowe urządzenie ekranu Menedżera urządzeń](device-manager-images/win/09-new-device-editor-sml.png)](device-manager-images/win/09-new-device-editor.png#lightbox)

Aby skonfigurować nowe urządzenie w **nowe urządzenie** ekranu, wykonaj następujące kroki:

1. Wybierz urządzenie fizyczne emulować klikając **urządzenia** menu rozwijanego:

    [![Menu rozwijane urządzenia](device-manager-images/win/10-device-menu-sml.png)](device-manager-images/win/10-device-menu.png#lightbox)

2. Wybierz obraz systemu do użycia z tym urządzeniem wirtualnym, klikając **obrazu systemu** menu rozwijanego. W tym menu znajdują się obrazy Menedżera urządzeń zainstalowany system w obszarze **zainstalowana**. **Pobierz** sekcja zawiera obrazy manager urządzenia systemu, które są aktualnie niedostępne na komputerze deweloperskim, ale można automatycznie zainstalować:

    [![Menu rozwijane obrazu systemu](device-manager-images/win/11-system-image-menu-sml.png)](device-manager-images/win/11-system-image-menu.png#lightbox)

3. Nadaj nazwę nowego urządzenia. W poniższym przykładzie, nowego urządzenia o nazwie **węzła 5 interfejsu API 25**:

    [![Nazewnictwo nowego urządzenia](device-manager-images/win/12-device-name-sml.png)](device-manager-images/win/12-device-name.png#lightbox)

4. Edytuj wszystkie właściwości, które należy zmodyfikować. Aby zmienić właściwości, zobacz [edycji systemu Android właściwości urządzenia wirtualnego](~/android/get-started/installation/android-emulator/device-properties.md).

5. Dodaj wszelkie dodatkowe właściwości, które należy jawnie ustawić. **Nowe urządzenie** ekranie są wyświetlane tylko właściwości najbardziej powszechnie zmodyfikowane, ale możesz kliknąć **Dodaj właściwość** menu rozwijanego (w lewym dolnym rogu) można dodać dodatkowe właściwości. W poniższym przykładzie `hw.lcd.backlight` dodawana właściwość:

    [![Dodawanie menu rozwijanego właściwości](device-manager-images/win/13-add-property-menu-sml.png)](device-manager-images/win/13-add-property-menu.png#lightbox)

6. Kliknij przycisk **Utwórz** przycisk (prawym dolnym rogu), aby utworzyć nowe urządzenie:

    ![Tworzenie przycisków](device-manager-images/win/14-create-button.png)

7. Może spowodować, że **akceptacji licencji** ekranu. Kliknij przycisk **Akceptuj** Jeśli akceptujesz postanowienia licencyjne:

    ![Ekran akceptacji licencji](device-manager-images/win/15-license-acceptance.png)

8. Menedżer urządzeń systemu Android dodaje nowe urządzenie na liście zainstalowane urządzenia wirtualnego podczas wyświetlania **tworzenie** wskaźnik postępu podczas tworzenia urządzenia:

    [![Postęp tworzenia wskaźnika](device-manager-images/win/16-creating-the-device-sml.png)](device-manager-images/win/16-creating-the-device.png#lightbox)

9. Po zakończeniu procesu tworzenia nowego urządzenia jest wyświetlane na liście zainstalowanych urządzeń wirtualnych z **Start** przycisk Gotowe do uruchomienia:

   [![Nowo utworzona urządzenia, gotowy do uruchomienia](device-manager-images/win/17-created-device-sml.png)](device-manager-images/win/17-created-device.png#lightbox)


### <a name="edit-device"></a>Edytowanie urządzenia

Aby edytować istniejące urządzenie wirtualne, wybierz urządzenie, a następnie kliknij przycisk **Edytuj** przycisk (znajdujący się w prawym górnym rogu ekranu):

[![Przycisk modyfikowania nowe urządzenie Edytuj](device-manager-images/win/19-edit-button-sml.png)](device-manager-images/win/19-edit-button.png#lightbox)

Kliknięcie przycisku **Edytuj** uruchamia edytor urządzenia dla wybranego urządzenia wirtualnego:

[![Ekran urządzenia edytora](device-manager-images/win/20-device-editor-sml.png)](device-manager-images/win/20-device-editor.png#lightbox)

**Edytor urządzenia** ekranie są wyświetlane właściwości urządzenia wirtualnego w pierwszej kolumnie z odpowiednimi wartościami każdej właściwości w drugiej kolumnie. Po wybraniu właściwości szczegółowy opis tej właściwości jest wyświetlana po prawej stronie.

Na przykład w poniższym zrzucie ekranu `hw.lcd.density` właściwość został zmieniony z **420** do **240**:

[![Przykład edycji urządzenia](device-manager-images/win/21-device-editing-sml.png)](device-manager-images/win/21-device-editing.png#lightbox)

Po dokonaniu zmiany niezbędną konfigurację, kliknij przycisk **zapisać** przycisku.
Aby uzyskać więcej informacji na temat zmiany właściwości urządzenia wirtualnego, zobacz [edycji systemu Android właściwości urządzenia wirtualnego](~/android/get-started/installation/android-emulator/device-properties.md).

 
### <a name="additional-options"></a>Dodatkowe opcje

Dostępne są dodatkowe opcje Praca z urządzeniami &hellip; menu w prawym górnym rogu:

[![Lokalizacja dodatkowe opcje menu](device-manager-images/win/22-overflow-menu-sml.png)](device-manager-images/win/22-overflow-menu.png#lightbox)

Dodatkowe opcje menu zawiera następujące elementy:

-   **Zduplikowana i edytować** &ndash; duplikaty aktualnie wybrane urządzenie i otwarcie go w **nowe urządzenie** ekranu z różnych unikatową nazwę. Na przykład wybranie **VisualStudio_android 23_x86_phone** i klikając **zduplikowany i edytować** dołącza do nazwy licznika:

    [![Duplikat i Edycja ekranu](device-manager-images/win/23-dupe-and-edit-sml.png)](device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **Wyświetlanie w Eksploratorze** &ndash; powoduje otwarcie okna Eksploratora Windows w folderze, który zawiera pliki dla urządzenia wirtualnego. Na przykład wybranie **węzła 5 X API 25** i klikając **ujawnić w Eksploratorze** powoduje otwarcie okna, podobnie do następującej:

    [![Kliknięcie przycisku ujawniania w Eksploratorze wyników](device-manager-images/win/24-reveal-in-explorer-sml.png)](device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **Resetowanie do ustawień fabrycznych** &ndash; resetuje wybranego urządzenia do domyślnych ustawień, wymazywanie użytkownika zmian stanu wewnętrznego urządzenia została uruchomiona (to również usuwa bieżące [szybkie rozruchu](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot) migawki Jeśli istnieją). Ta zmiana nie zmienia zmiany wprowadzone do urządzenia wirtualnego podczas tworzenia i edytowania. Pojawi się okno dialogowe z monitu, że resetowania nie można cofnąć. Kliknij przycisk **czyszczenie danych użytkownika** o potwierdzenie resetowania.

-   **Usuń** &ndash; powoduje trwałe usunięcie wybranego urządzenia wirtualnego.
    Pojawi się okno dialogowe z monitu, że usunięcie urządzenia nie można cofnąć. Kliknij przycisk **usunąć** Jeśli masz pewność, że chcesz usunąć urządzenie.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W tym artykule opisano sposób tworzenia, zduplikowany, dostosowywanie i uruchomić urządzenia wirtualne z systemem Android przy użyciu Menedżera urządzeń systemu Android.

[![Zrzut ekranu Menedżera urządzeń systemu Android w zakładce urządzenia](device-manager-images/mac/01-devices-dialog-sml.png)](device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Ten przewodnik dotyczy tylko programu Visual Studio dla komputerów Mac.
Program Xamarin Studio jest niezgodny przy użyciu Menedżera urządzeń systemu Android.

Za pomocą Menedżera urządzeń systemu Android, aby utworzyć i skonfigurować *urządzeń wirtualnych z systemem Android* (urządzeń Avd), które działają w [emulatora systemu Android](~/android/deploy-test/debugging/debug-on-emulator.md).
Każdy AVD jest konfiguracji emulatora, która symuluje fizycznego urządzenia z systemem Android. Pozwala na uruchamianie i testowanie aplikacji w różnych konfiguracji, które symulować różne fizycznego urządzenia z systemem Android.

## <a name="requirements"></a>Wymagania

- Program Visual Studio dla komputerów Mac w wersji 7.5 lub nowszej.

- Android SDK 8.0 (26 interfejsu API) lub nowszym należy zainstalować za pomocą Menedżera zestawu SDK systemu Android.


## <a name="launching-the-device-manager"></a>Uruchamianie Menedżera urządzeń

Uruchamianie Menedżera urządzeń systemu Android, klikając **Narzędzia > Menedżera urządzeń**:

[![Uruchamianie z menu Narzędzia](device-manager-images/mac/16-tools-menu-sml.png)](device-manager-images/mac/16-tools-menu.png#lightbox)

Przed użyciem Menedżera urządzeń systemu Android, należy zainstalować wersję narzędzia zestawu SDK systemu Android 26.0.2 lub nowszym. Jeśli zestaw SDK systemu Android narzędzi 26.0.2 lub nowszy nie jest zainstalowany, zostanie wyświetlony po uruchomieniu tego okna dialogowego błędu:

![Okno dialogowe android SDK wystąpienia błędu](device-manager-images/mac/02-sdk-instance-error.png)

Jeśli widzisz tego okna dialogowego błędu, kliknij przycisk **OK** aby otworzyć narzędzie Android SDK Manager. W systemie Android SDK Manager kliknij przycisk **narzędzia** karcie i zainstalowane następujące składniki:

-   **Narzędzia zestawu SDK systemu android 26.0.2** lub nowszy 
-   **Android SDK narzędzi platformy 26.0.0** lub nowszy 
-   **Android SDK — narzędzia kompilacji 26.0.0** lub nowszy

Te pakiety powinny być wyświetlane z **zainstalowana** stanu, jak pokazano na poniższym zrzucie ekranu:

[![Instalowanie narzędzia zestawu SDK systemu Android 26.0](device-manager-images/mac/03-sdk-tools-sml.png)](device-manager-images/mac/03-sdk-tools.png#lightbox)

## <a name="main-screen"></a>Ekran główny

Uruchamianie Menedżera urządzeń Android przedstawia informacje ekranu, który wyświetla wszystkie aktualnie skonfigurowane urządzenia wirtualnego. Dla każdego urządzenia **nazwa**, **obrazu systemu** (poziom interfejsu API systemu Android), **Procesora**, **pamięci** rozmiar i rozdzielczość ekranu są wyświetlane:

[![Lista zainstalowanych urządzeń i ich parametry](device-manager-images/mac/05-devices-list-sml.png)](device-manager-images/mac/05-devices-list.png#lightbox)

Kliknij przycisk **odtwarzanie** przycisk, aby uruchomić emulatora z urządzeniem wirtualnym wybranych przez użytkownika:

[![Przycisk Start obrazu urządzenia](device-manager-images/mac/06-start-button-sml.png)](device-manager-images/mac/06-start-button.png#lightbox)

Po emulator rozpoczyna się od wybranego urządzenia wirtualnego **odtwarzanie** przycisku zmienia się na **zatrzymać** przycisku, który umożliwia zatrzymanie emulator:

[![Zatrzymaj przycisku uruchomionych urządzenia](device-manager-images/mac/07-stop-button-sml.png)](device-manager-images/mac/07-stop-button.png#lightbox)

### <a name="new-device"></a>Nowe urządzenie

Aby utworzyć nowe urządzenie, kliknij przycisk **nowe urządzenie** przycisk (znajdujący się po prawej stronie górnej części ekranu):

[![Nowy przycisk służący do tworzenia nowego urządzenia](device-manager-images/mac/08-new-button-sml.png)](device-manager-images/mac/08-new-button.png#lightbox)

Kliknięcie przycisku **nowe urządzenie** uruchamia **nowe urządzenie** ekranu:

[![Nowe urządzenie ekranu Menedżera urządzeń](device-manager-images/mac/09-new-device-editor-sml.png)](device-manager-images/mac/09-new-device-editor.png#lightbox)

Wykonaj następujące kroki, aby skonfigurować nowe urządzenie w **nowe urządzenie** ekranu:

1. Wybierz urządzenie fizyczne emulować klikając **urządzenia** menu rozwijanego:

    [![Menu rozwijane urządzenia](device-manager-images/mac/10-device-menu-sml.png)](device-manager-images/mac/10-device-menu.png#lightbox)

2. Wybierz obraz systemu do użycia z tym urządzeniem wirtualnym, klikając **obrazu systemu** menu rozwijanego. W tym menu znajdują się obrazy Menedżera urządzeń zainstalowany system w obszarze **zainstalowana**. **Pobierz** sekcja (jeśli są pokazywane) zawiera obrazy manager urządzenia systemu, które są aktualnie niedostępne na komputerze deweloperskim, ale można automatycznie zainstalować:

    [![Menu rozwijane obrazu systemu](device-manager-images/mac/11-system-image-menu-sml.png)](device-manager-images/mac/11-system-image-menu.png#lightbox)

3. Nadaj nazwę nowego urządzenia. W poniższym przykładzie, nowego urządzenia o nazwie **węzła 5 X API 25**:

    [![Nazewnictwo nowego urządzenia](device-manager-images/mac/12-device-name-sml.png)](device-manager-images/mac/12-device-name.png#lightbox)

4. Edytuj wszystkie właściwości, które należy zmodyfikować. Aby zmienić właściwości, zobacz [edycji systemu Android właściwości urządzenia wirtualnego](~/android/get-started/installation/android-emulator/device-properties.md).

5. Dodaj wszelkie dodatkowe właściwości, które należy jawnie ustawić. **Nowe urządzenie** ekranie są wyświetlane tylko właściwości najbardziej powszechnie zmodyfikowane, ale możesz kliknąć **Dodaj właściwość** menu rozwijanego (w lewym dolnym rogu) można dodać dodatkowe właściwości:

    [![Dodawanie menu rozwijanego właściwości](device-manager-images/mac/13-add-property-menu-sml.png)](device-manager-images/mac/13-add-property-menu.png#lightbox)

6. Możesz również kliknąć **niestandardowy** do definiowania nowych właściwości dla urządzenia:

    ![Tworzenie przycisków](device-manager-images/mac/14-custom-button.png)

7. Kliknij przycisk **Utwórz** przycisk (prawym dolnym rogu), aby utworzyć nowe urządzenie:

    ![Tworzenie przycisków](device-manager-images/mac/15-create-button.png)

8. Może spowodować, że **akceptacji licencji** ekranu. Kliknij przycisk **Akceptuj** Jeśli akceptujesz postanowienia licencyjne.

9. Menedżer urządzeń systemu Android dodaje nowe urządzenie na liście zainstalowane urządzenia wirtualnego podczas wyświetlania **tworzenie** wskaźnik postępu podczas tworzenia urządzenia:

    [![Postęp tworzenia wskaźnika](device-manager-images/mac/17-creating-the-device-sml.png)](device-manager-images/mac/17-creating-the-device.png#lightbox)

10. Po zakończeniu procesu tworzenia nowego urządzenia jest wyświetlany na liście urządzeń z **odtwarzanie** przycisk Gotowe do uruchomienia:

   [![Nowo utworzona urządzenia, gotowy do uruchomienia](device-manager-images/mac/18-created-device-sml.png)](device-manager-images/mac/18-created-device.png#lightbox)


### <a name="edit-device"></a>Edytowanie urządzenia

Aby edytować istniejące urządzenie wirtualne, wybierz **dodatkowe opcje** menu rozwijanego (koło zębate ikonę) i wybierz **Edytuj**:
 
[![Edytuj zaznaczenia menu modyfikowania nowego urządzenia](device-manager-images/mac/19-edit-button-sml.png)](device-manager-images/mac/19-edit-button.png#lightbox)

Kliknięcie przycisku **Edytuj** uruchamia edytor urządzenia dla wybranego urządzenia wirtualnego:
 
[![Ekran urządzenia edytora](device-manager-images/mac/20-device-editor-sml.png)](device-manager-images/mac/20-device-editor.png#lightbox)

**Edytor urządzenia** ekranie są wyświetlane właściwości urządzenia wirtualnego w pierwszej kolumnie z odpowiednimi wartościami każdej właściwości w drugiej kolumnie. Po wybraniu właściwości szczegółowy opis tej właściwości jest wyświetlana po prawej stronie.

Na przykład w poniższym zrzucie ekranu `hw.lcd.density` właściwości została zmieniona z **320** do **240** i `hw.ramSize` jest zmieniana na **768**:
 
[![Przykład edycji urządzenia](device-manager-images/mac/21-device-editing-sml.png)](device-manager-images/mac/21-device-editing.png#lightbox)

Po dokonaniu zmiany niezbędną konfigurację, kliknij przycisk **zapisać** przycisku.
Aby uzyskać więcej informacji na temat zmiany właściwości urządzenia wirtualnego, zobacz [edycji systemu Android właściwości urządzenia wirtualnego](~/android/get-started/installation/android-emulator/device-properties.md).

 
### <a name="additional-options"></a>Dodatkowe opcje

Dodatkowe opcje do pracy z urządzeniem są dostępne z menu rozwijanego znajdujący się na lewo od **odtwarzanie** przycisk:

[![Lokalizacja dodatkowe opcje menu](device-manager-images/mac/22-overflow-menu-sml.png)](device-manager-images/mac/22-overflow-menu.png#lightbox)

Dodatkowe opcje menu zawiera następujące elementy:

-   **Edytuj** &ndash; otwiera urządzenia obecnie zaznaczonego w edytorze urządzenia, zgodnie z wcześniejszym opisem.

-   **Zduplikowana i edytować** &ndash; duplikaty aktualnie wybrane urządzenie i otwarcie go w **nowe urządzenie** ekranu z różnych unikatową nazwę.
    Na przykład wybranie **węzła 5 X API 25** i klikając **zduplikowany i edytować** dołącza do nazwy licznika:

    [![Duplikat i Edycja ekranu](device-manager-images/mac/23-dupe-and-edit-sml.png)](device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **Wyświetlanie w wyszukiwarce** &ndash; powoduje otwarcie okna wyszukiwania macOS w folderze, który zawiera pliki dla urządzenia wirtualnego. Na przykład wybranie **węzła 5 X API 25** i klikając **ujawnić w wyszukiwarce** powoduje otwarcie okna, podobnie do następującej:

    [![Kliknięcie przycisku ujawniania w Eksploratorze wyników](device-manager-images/mac/24-reveal-in-finder-sml.png)](device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **Resetowanie do ustawień fabrycznych** &ndash; resetuje wybranego urządzenia do domyślnych ustawień, wymazywanie użytkownika zmian stanu wewnętrznego urządzenia została uruchomiona. Ta zmiana nie zmienia zmiany wprowadzone do urządzenia wirtualnego podczas tworzenia i edytowania. Pojawi się okno dialogowe z monitu, że resetowania nie można cofnąć. Kliknij przycisk **czyszczenie danych użytkownika** o potwierdzenie resetowania.

-   **Usuń** &ndash; powoduje trwałe usunięcie wybranego urządzenia wirtualnego.
    Pojawi się okno dialogowe z monitu, że usunięcie urządzenia nie można cofnąć. Kliknij przycisk **usunąć** Jeśli masz pewność, że chcesz usunąć urządzenie.

-----

## <a name="troubleshooting"></a>Rozwiązywanie problemów

W poniższych sekcjach opisano sposób zdiagnozować i rozwiązać problemy, które mogą wystąpić podczas konfigurowania urządzeń wirtualnych przy użyciu Menedżera urządzeń systemu Android.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-in-non-standard-location"></a>Zestaw SDK systemu android w lokalizacji niestandardowej

Zazwyczaj zestaw SDK systemu Android jest instalowany w następującej lokalizacji:

**C:\\Program Files (x86)\\Android\\zestawu sdk systemu android**

Jeśli zestaw SDK nie jest zainstalowany w tej lokalizacji, błąd ten może wystąpić podczas uruchamiania Menedżera urządzeń systemu Android:

![Błąd wystąpienia zestawu SDK systemu android](troubleshooting-images/win/01-sdk-error.png)

Aby obejść ten problem, wykonaj następujące czynności:

1. Na pulpicie systemu Windows, przejdź do **C:\\użytkowników\\*username*\\AppData\\mobilny\\XamarinDeviceManager**:

    ![Lokalizacja pliku dziennika Menedżera urządzeń systemu android](troubleshooting-images/win/02-log-files.png)

2. Kliknij dwukrotnie, aby otworzyć jeden z plików dziennika i Znajdź **ścieżkę pliku konfiguracyjnego**. Na przykład:

    [![Ścieżka pliku konfiguracji w pliku dziennika](troubleshooting-images/win/03-config-file-path-sml.png)](troubleshooting-images/win/03-config-file-path.png#lightbox)

3. Przejdź do tej lokalizacji i kliknij dwukrotnie **user.config** go otworzyć. 

4. W **user.config**, zlokalizuj **&lt;ustawienia użytkownika&gt;** element i Dodaj **AndroidSdkPath** do niej atrybut. Ten atrybut ustawiony na ścieżkę zainstalowanym zestawu SDK systemu Android na komputerze, a następnie zapisz plik. Na przykład **&lt;ustawienia użytkownika&gt;** będzie wyglądać podobnie do następującej, jeśli zainstalowano zestaw SDK systemu Android w **C:\\programy\\Android\\zestawuSDK**:
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

Po wprowadzeniu tej zmiany do **user.config**, można uruchomić Menedżera urządzeń systemu Android.

### <a name="snapshot-disables-wifi-on-android-oreo"></a>Migawki wyłącza sieci Wi-Fi na Oreo systemu Android

Jeśli masz AVD skonfigurowano dla systemu Android Oreo symulowane dostępu do sieci Wi-Fi, ponowne uruchomienie AVD po migawki może spowodować dostępu do sieci Wi-Fi ma zostać wyłączona.

Aby obejść ten problem

1. Wybierz AVD w Menedżerze urządzeń systemu Android.

2. W menu opcji dodatkowych kliknij **ujawnić w Eksploratorze**.

3. Przejdź do **migawki > default_boot**.

4. Usuń **snapshot.pb** pliku:

    ![Lokalizacja pliku snapshot.pb](troubleshooting-images/win/05-delete-snapshot.png)

5. Uruchom ponownie AVD. 

Po wprowadzeniu tych zmian, AVD zostanie uruchomiony ponownie w stanie, który umożliwia sieci Wi-Fi do pracy ponownie.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="snapshot-disables-wifi-on-android-oreo"></a>Migawki wyłącza sieci Wi-Fi na Oreo systemu Android

Jeśli masz AVD skonfigurowano dla systemu Android Oreo symulowane dostępu do sieci Wi-Fi, ponowne uruchomienie AVD po migawki może spowodować dostępu do sieci Wi-Fi ma zostać wyłączona.

Aby obejść ten problem

1. Wybierz AVD w Menedżerze urządzeń systemu Android.

2. W menu opcji dodatkowych kliknij **ujawnić w wyszukiwarce**.

3. Przejdź do **migawki > default_boot**.

4. Usuń **snapshot.pb** pliku:

    [![Lokalizacja pliku snapshot.pb](troubleshooting-images/mac/02-delete-snapshot-sml.png)](troubleshooting-images/mac/02-delete-snapshot.png#lightbox)

5. Uruchom ponownie AVD. 

Po wprowadzeniu tych zmian, AVD zostanie uruchomiony ponownie w stanie, który umożliwia sieci Wi-Fi do pracy ponownie.

-----

### <a name="generating-a-bug-report"></a>Generowanie raport o usterce

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Jeśli napotkasz problem przy użyciu systemu Android Menedżera urządzeń którego nie można rozwiązać przy użyciu powyższego wskazówki dotyczące rozwiązywania problemów, klikając prawym przyciskiem myszy pasek tytułu i wybierając pliku raport o usterce **wygenerować raport o usterce**:

[![Lokalizacja elementu menu dla składanie raport o usterce](troubleshooting-images/win/04-bug-report-sml.png)](troubleshooting-images/win/04-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Jeśli napotkasz problem przy użyciu systemu Android Menedżera urządzeń którego nie można rozwiązać przy użyciu powyższego wskazówki dotyczące rozwiązywania problemów, klikając pliku raport o usterce **Pomoc > Generowanie raportu o usterce**:

[![Lokalizacja elementu menu dla składanie raport o usterce](troubleshooting-images/mac/01-bug-report-sml.png)](troubleshooting-images/mac/01-bug-report.png#lightbox)


-----

## <a name="summary"></a>Podsumowanie

W tym przewodniku wprowadzono Menedżera urządzeń Android dostępne w programie Visual Studio dla systemów Mac i Visual Studio Tools dla platformy Xamarin. Go wyjaśniono zasadnicze funkcje, takie jak uruchamianie i zatrzymywanie emulatora systemu Android, wybierając wirtualnego urządzenia z systemem Android (AVD) do uruchomienia, tworzenie nowych wirtualnych urządzeń i jak edytować urządzenia wirtualnego. Wyjaśniono jej edytowanie właściwości profilu sprzętu dla dalszego dostosowania i zapewniała wskazówki dotyczące rozwiązywania problemów dla typowych problemów.


## <a name="related-links"></a>Linki pokrewne

- [Zmiany zestawu narzędzi Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Debugowanie na emulatorze systemu Android](~/android/deploy-test/debugging/debug-on-emulator.md)
- [Informacje o wersji (Google) narzędzia zestawu SDK](https://developer.android.com/studio/releases/sdk-tools)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
