---
title: Menedżer emulatora Google
description: Jak utworzyć i zarządzać wirtualnych urządzeń z systemem Android za pomocą Menedżera emulatora Google
ms.prod: xamarin
ms.assetid: 0C0BBEC0-C84A-4558-B905-4EF81FCD62F9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: c42ebdca44e47e29ac74a263f0d11d4d4c120586
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="google-emulator-manager"></a>Menedżer emulatora Google

Po upewnieniu się, że jest włączona przyspieszanie sprzętowe (zgodnie z opisem w [przyspieszanie sprzętowe emulatora systemu Android](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), następnym krokiem jest utworzenie urządzeń wirtualnych do użycia na potrzeby testowania i debugowania aplikacji. Można użyć starszego Menedżera emulatora Google (znanej także jako *Android Virtual Device (AVD) Manager*) do tworzenia urządzeń wirtualnych do użycia przez Emulator systemu Google Android.

> [!NOTE]
> Jeśli ma być przeznaczona dla systemu Android Oreo 8.0, należy użyć [Menedżera urządzeń Xamarin Android](~/android/get-started/installation/android-emulator/xamarin-device-manager.md) tworzenie i konfigurowanie urządzenia wirtualnego.


## <a name="installing-system-images"></a>Instalowanie systemu obrazów

W zależności od tego, który ma być docelowa poziomy interfejsu API systemu Android musisz pobrać i zainstalować obrazy systemu specyficzne dla poziom interfejsu API, które są używane przez zestaw SDK systemu Android emulator. Na każdym poziomie interfejsu API systemu Android to zbiór **x86** obrazów systemu, które będą potrzebne do pobrania i zainstalowania tworzenia urządzenia wirtualnego.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby zainstalować obrazy systemu konieczne, uruchom narzędzie Android SDK Manager (**Narzędzia > Android > Android SDK Manager**) i przewiń do poziomy interfejsu API mają być obsługiwane. Włącz znacznik wyboru obok następujące obrazy systemu na każdym poziomie interfejsu API:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby zainstalować obrazy systemu konieczne, uruchom narzędzie Android SDK Manager (**Narzędzia > SDK Manager**) i przewiń do poziomy interfejsu API mają być obsługiwane. Włącz znacznik wyboru obok następujące obrazy systemu na każdym poziomie interfejsu API:

-----

-   **Obraz systemu Atom Intel x86**
-   **Obraz systemu Intel interfejsy API Google x86 Atom**

Obraz systemu dodaje interfejsy API Google (na przykład map interfejsy API Google) do urządzenia wirtualnego. 

Na poniższym zrzucie ekranu **Atom Intel x86** obrazy zostanie zainstalowany, dzięki czemu można tworzyć urządzeń wirtualnych z systemem Android 6.0:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Wybieranie obrazów systemu Android 6.0 x86 dla emulatora systemu Android](google-emulator-manager-images/win/03-select-x86-images-sml.png)](google-emulator-manager-images/win/03-select-x86-images.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Wybieranie obrazów systemu Android 6.0 x86 dla emulatora systemu Android](google-emulator-manager-images/mac/02-select-x86-images-sml.png)](google-emulator-manager-images/mac/02-select-x86-images.png#lightbox)

-----

Jeśli tworzysz aplikacje 64-bitowych, należy zainstalować następujące obrazy systemu:

-   **Obraz systemu Atom_64 Intel x86**
-   **Obraz systemu Atom_64 Intel interfejsy API Google x86**

Te obrazy 64-bitowy system umożliwia uruchamianie aplikacji 32-bitowych. jednak 32-bitową **obrazu systemu Atom Intel x86** nieco szybciej działa w emulatorze systemu Android SDK.

Jeśli tworzysz aplikacje dla systemu Android nosić, należy zainstalować następujące obrazy systemu:

-   **Obraz systemu Atom Intel x86 zużycia dla systemu android**
-   **Obraz systemu Intel interfejsy API Google x86 Atom**

Po zainstalowaniu tych obrazów systemu, można utworzyć **x86**— na podstawie urządzeń wirtualnych z systemem Android, wybierając odpowiedni poziom interfejsu API i procesora CPU/ABI wyborów podczas konfiguracji urządzenia wirtualnego (to jest opisane w dalszej części).


## <a name="configuring-virtual-devices"></a>Konfigurowanie urządzeń wirtualnych

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Urządzenia wirtualne są skonfigurowane za pomocą **Android Emulator Manager** (zwaną także _Menedżer urządzeń wirtualnych systemu Android_ lub _Menedżera AVD_). Aby uruchomić Menedżera emulatora systemu Android w programie Visual Studio, kliknij przycisk **Android Emulator Manager** ikonę na pasku narzędzi:

[![Położenie ikony AVD](google-emulator-manager-images/win/04-avd-icon-sml.png)](google-emulator-manager-images/win/04-avd-icon.png#lightbox)

Można również uruchomić Menedżera emulatora systemu Android na pasku menu, wybierając **Narzędzia > Android > Android Emulator Manager**:

[![Lokalizacja elementu menu Menedżera emulatora systemu android](google-emulator-manager-images/win/05-avd-manager-menu-item-sml.png)](google-emulator-manager-images/win/05-avd-manager-menu-item.png#lightbox)

**Android Virtual Device (AVD) Manager** okno dialogowe wyświetla listę istniejących urządzeń wirtualnych systemu Android:

[![Menedżer urządzeń wirtualnych systemu android](google-emulator-manager-images/win/06-virtual-device-manager-sml.png)](google-emulator-manager-images/win/06-virtual-device-manager.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Urządzenia wirtualne są skonfigurowane za pomocą **Android Emulator Manager** (zwaną także _Menedżer urządzeń wirtualnych systemu Android_ lub _Menedżera AVD_). 

Można uruchomić Menedżera emulatora systemu Android na pasku menu, wybierając **Narzędzia > Google Emulator Manager**:

[![Lokalizacja elementu menu Menedżera emulatora systemu android](google-emulator-manager-images/mac/03-avd-manager-menu-item-sml.png)](google-emulator-manager-images/mac/03-avd-manager-menu-item.png#lightbox)

**Android Virtual Device (AVD) Manager** okno dialogowe wyświetla listę istniejących urządzeń wirtualnych systemu Android:

[![Menedżer urządzeń wirtualnych systemu android](google-emulator-manager-images/mac/05-virtual-device-manager-sml.png)](google-emulator-manager-images/mac/05-virtual-device-manager.png#lightbox)

-----

Można utworzyć nowych obrazów urządzenie wirtualne z innego urządzenia właściwości i poziomy interfejsu API &ndash; następnej sekcji opisano sposób tworzenia urządzeń niestandardowych definicje i urządzenia wirtualnego.


### <a name="creating-a-custom-device-definition"></a>Tworzenie definicji urządzeń niestandardowych

Aby utworzyć definicję urządzeń niestandardowych, kliknij przycisk **Utwórz...**  w **Menedżera urządzeń wirtualnych systemu Android (AVD)**. Spowoduje to otwarcie **tworzenie nowych Android Virtual Device (AVD)** okna dialogowego:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Definicja urządzeń niestandardowych oparte na węzła 6](google-emulator-manager-images/win/07-custom-device-sml.png)](google-emulator-manager-images/win/07-custom-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Definicja urządzeń niestandardowych oparte na węzła 6](google-emulator-manager-images/mac/06-custom-device-sml.png)](google-emulator-manager-images/mac/06-custom-device.png#lightbox)

-----

W tym oknie dialogowym skonfiguruj następujące opcje:

-   **Nazwa AVD** &ndash; unikatową nazwę w definicji urządzenia. Na przykład zrzucie ekranu powyżej, ma ustawioną nazwę **MyNexus**. Zauważ, że nazwa AVD nie może zawierać spacji &ndash; **OK** przycisk zostanie wyłączona, Jeśli spróbujesz użyć spacje w nazwie AVD.

-   **Urządzenie** &ndash; wybierz profil sprzętu, który chcesz emulować (na przykład **5 węzła** lub **6 węzła**).

-   **Docelowy** &ndash; wybierz poziom interfejsu API systemu Android dla urządzenia wirtualnego. To ustawienie powinno być większa lub równa minimalna wersja systemu Android w aplikacji.

-   **Procesor CPU/ABI** &ndash; wybierz **Atom Intel interfejsy API Google (x86)** tak, aby interfejsy API Google będą dostępne w definicji urządzenia.

-   **Skórki** &ndash; wybierz wygląd urządzenia wirtualnego. Na przykład zrzucie ekranu powyżej **HVGA** wybrano skórki (zrzut ekranu emulatora na końcu tego artykułu jest przykładem **HVGA** skórki).

-   **Opcje pamięci** &ndash; zazwyczaj ilość pamięci RAM ustawieniem domyślnym jest zbyt wysoka i, w systemie Windows, powoduje, że ostrzeżenia: **emulowanie pamięci RAM przekracza 768M może się nie powieść**. Dla większości użytkowników zaleca się ustawienie pamięci RAM do 768MB (jak pokazano na powyższym zrzucie ekranu). Duża ilość pamięci RAM wartości może to spowolnić emulator.

-   **Użyj hosta procesora GPU** &ndash; tej opcji powoduje, że emulator, aby użyć hosta komputera graficznego przetwarzania jednostki (GPU) w celu wykonania operacji grafiki. Firma Microsoft zaleca, aby włączyć tę opcję, aby zwiększyć wydajność emulatora. Aby uzyskać więcej informacji na temat **opcje emulacji** sekcji, zobacz [co to są migawki i użycie hosta GPU emulacji opcje używane dla?](https://android.stackexchange.com/questions/51739/what-is-snapshot-and-use-host-gpu-emulation-options-for)


Pozostałe opcje można pozostawić ustawień domyślnych. Gdy wszystko będzie gotowe, kliknij przycisk **OK** do utworzenia nowego urządzenia wirtualnego. Wyniki dla nowej konfiguracji urządzenia wirtualnego wyszczególnione w następnym oknie dialogowym:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Okno wyników po utworzeniu nowego AVD](google-emulator-manager-images/win/08-create-results.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Okno wyników po utworzeniu nowego AVD](google-emulator-manager-images/mac/07-create-results.png)

-----

Aby uzyskać szczegółowy opis właściwości konfiguracji opisanych w tym oknie dialogowym, zobacz [właściwości profilu sprzętu](https://developer.android.com/studio/run/managing-avds.html#hpproperties).
Po kliknięciu **OK**, nowa konfiguracja urządzenia jest wyświetlana na liście istniejących urządzeń wirtualnych systemu Android. Na poniższym zrzucie ekranu **MyNexus** został dodany do listy:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![MyNexus dodany do listy urządzeń](google-emulator-manager-images/win/09-added-to-list-sml.png)](google-emulator-manager-images/win/09-added-to-list.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![MyNexus dodany do listy urządzeń](google-emulator-manager-images/mac/08-added-to-list-sml.png)](google-emulator-manager-images/mac/08-added-to-list.png#lightbox)

-----

Nowe niestandardowe urządzenie wirtualne jest także dodawane do menu rozwijanego urządzenia:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![MyNexus dodany do menu rozwijanego urządzenia](google-emulator-manager-images/win/10-available-custom-device-sml.png)](google-emulator-manager-images/win/10-available-custom-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![MyNexus dodany do menu rozwijanego urządzenia](google-emulator-manager-images/mac/09-available-custom-device-sml.png)](google-emulator-manager-images/mac/09-available-custom-device.png#lightbox)

-----



### <a name="cloning-a-device-definition"></a>Klonowanie definicji urządzenia

Można wybrać istniejącą definicję urządzenia i *klonowania* go w celu utworzenia nowej definicji urządzeń niestandardowych. Jest to dobre rozwiązanie, należy używać w przypadku istniejącej definicji urządzenia wymagające tylko kilka drobne dostosowania do własnych potrzeb. **Definicje urządzenia** karcie **Android Virtual Device (AVD) Manager** Wyświetla wszystkie definicje dostępnego urządzenia:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Lista definicji dostępnego urządzenia](google-emulator-manager-images/win/11-device-definitions-sml.png)](google-emulator-manager-images/win/11-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Lista definicji dostępnego urządzenia](google-emulator-manager-images/mac/10-device-definitions-sml.png)](google-emulator-manager-images/mac/10-device-definitions.png#lightbox)

-----

Urządzenia wstępnie skonfigurowanych na tej liście nie można zmodyfikować &ndash; można edytować tylko utworzonych przez użytkownika urządzenia wirtualnego. Jest możliwe wybranie definicji urządzenie i klikając nową definicję urządzenia z definicji wstępnie skonfigurowanych urządzeń **klonowania**. Na przykład wybranie **5 węzła** definicji i klikając **klonowania** następujące okno dialogowe:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Okno dialogowe urządzenia w klonowania](google-emulator-manager-images/win/12-clone-device-sml.png)](google-emulator-manager-images/win/12-clone-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Okno dialogowe urządzenia w klonowania](google-emulator-manager-images/mac/11-clone-device-sml.png)](google-emulator-manager-images/mac/11-clone-device.png#lightbox)

-----

W następnym zrzut ekranu, nazwa została zmieniona na **węzła 5 niestandardowych** i parametrów urządzenia, które są modyfikowane w celu utwórz nową definicję urządzeń niestandardowych:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Niestandardowe węzła 5 AVD](google-emulator-manager-images/win/13-custom-nexus.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Niestandardowe węzła 5 AVD](google-emulator-manager-images/mac/12-custom-nexus-sml.png)](google-emulator-manager-images/mac/12-custom-nexus.png#lightbox)

-----

Kliknięcie przycisku **urządzenia w klonowania** tworzy nową definicję urządzenie jest teraz wyświetlany w **definicje urządzenia** listy:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Niestandardowe węzła 5 pojawia się jako nową definicję urządzenia użytkownika](google-emulator-manager-images/win/14-new-definition-sml.png)](google-emulator-manager-images/win/14-new-definition.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Niestandardowe węzła 5 pojawia się jako nową definicję urządzenia użytkownika](google-emulator-manager-images/mac/13-new-definition-sml.png)](google-emulator-manager-images/mac/13-new-definition.png#lightbox)

-----

Zauważ, że każda definicja utworzonych przez użytkownika urządzenia jest wyświetlane zielona ikona, jak pokazano powyżej. Ta nowa definicja urządzenia może służyć do tworzenia nowych AVD wybierając definicji i klikając **utworzyć AVD**. Spowoduje to wyświetlenie **tworzenie nowych Android Virtual Device (AVD)** okna dialogowego. W poniższym przykładzie nazwa **AVD\_dla\_Nexus\_5\_niestandardowy** generowane automatycznie dla nowego urządzenia wirtualnego:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Create AVD z definicji urządzenie użytkownika niestandardowego 5 węzła](google-emulator-manager-images/win/15-create-avd-sml.png)](google-emulator-manager-images/win/15-create-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Create AVD z definicji urządzenie użytkownika niestandardowego 5 węzła](google-emulator-manager-images/mac/14-create-avd-sml.png)](google-emulator-manager-images/mac/14-create-avd.png#lightbox)

-----

Po **OK** zostanie kliknięty niestandardowej konfiguracji urządzenia jest wyświetlana na liście istniejących urządzeń wirtualnych systemu Android. Ponadto został dodany do menu rozwijanego urządzenia:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nowe niestandardowe AVD dodany do menu rozwijanego urządzenia](google-emulator-manager-images/win/16-new-avd-sml.png)](google-emulator-manager-images/win/16-new-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Nowe niestandardowe AVD dodany do menu rozwijanego urządzenia](google-emulator-manager-images/mac/15-new-avd-sml.png)](google-emulator-manager-images/mac/15-new-avd.png#lightbox)

-----

