---
title: Gdzie można znaleźć Moje informacje o wersji i dzienniki?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: a669daf5361e35305182922cdcb7c6a1fb92db47
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>Gdzie można znaleźć Moje informacje o wersji i dzienniki?

## <a name="outline"></a>Kontur

- [Informacje o wersji](#version-information)
    - Informacje o wersji systemu Windows
    - Informacje o wersji adresów MAC
    - Narzędzia kompilacji zestawu SDK systemu android narzędzia, narzędzi platformy
- [Dzienniki IDE i Instalatora](#ide-and-installer-logs)
    - [Dzienniki systemu Windows](#windows-logs)
        - Xamarin Studio
        - Program Xamarin dla Visual Studio
        - Instalator uniwersalnego Xamarin
        - Poszczególne `.msi` instalatorów, pełne dzienniki
        - Visual Studio uruchamianie pełnych dzienników
    - [Dzienniki Mac](#mac-logs)
        - Tworzenie hosta
    - Visual Studio for Mac
        - Xamarin Studio
        - Instalator platformy Xamarin
- [Dane wyjściowe kompilacji pełne](#verbose-build-output-logs)
- [Debugowanie dzienniki dla aplikacji platformy Xamarin.Android i Xamarin.iOS](#debug-logs-for-xamarin-apps)
    - Android `adb` dzienniki logcat
    - symulatora systemu iOS logowania (Mac)
    - urządzenia z systemem iOS logowania (Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />Informacje o wersji

Jest to zazwyczaj najlepiej, aby wysłać kopię wszystkich informacji z **kopiowanie informacji** przycisków. W przeciwnym razie będzie często należy zażądać dodatkowych informacji. Na przykład poziomy interfejsu API systemu Android zainstalowane wersje systemu operacyjnego, Xcode w wersji i .NET w wersji może składać się ważne podczas pomaga rozwiązać problem.

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Informacje o wersji systemu Windows

#### <a name="xamarin-studio"></a>Xamarin Studio

**Pomoc > o > Pokaż szczegóły > Kopiowanie informacji [przycisk]**

#### <a name="visual-studio"></a>Visual Studio

**Pomoc > Microsoft Visual Studio > informacje o kopii [przycisk]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Informacje o wersji adresów MAC

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Visual Studio > o Visual Studio > Pokaż szczegóły > Kopiowanie informacji [przycisk]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Narzędzia kompilacji zestawu SDK systemu android narzędzia, narzędzi platformy

Otwórz narzędzie Android SDK Manager, a wykonanie zrzutu górnej **narzędzia** sekcji.

![](https://kb.xamarin.com/customer/portal/attachments/337323 "Zrzut ekranu programu Android SDK Manager > folder Narzędzia")

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Narzędzia > Otwórz Menedżera zestawu SDK systemu Android**

#### <a name="visual-studio"></a>Visual Studio

Wybierz ikonę narzędzi SDK Manager:

![](https://kb.xamarin.com/customer/portal/attachments/420238 "Uruchom Android SDK Manager ikony paska narzędzi w programie Visual Studio")

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />Dzienniki IDE i Instalatora

Dla każdej lokalizacji dziennika należy do pliku zip i dołączyć folder cały dziennik.

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Dzienniki systemu Windows

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Visual Studio Tools dla platformy Xamarin

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[Jak uzyskać dzienniki instalacji programu Visual Studio](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> Instalator "Uniwersalne" Xamarin

`%LOCALAPPDATA%\Xamarin\Universal`

Są to dzienniki z `XamarinInstaller.exe` Instalatora.

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />Poszczególne `.msi` instalatorów, pełne dzienniki

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

Odwołanie: [opcji wiersza polecenia](http://msdn.microsoft.com/en-us/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Visual Studio uruchamianie pełnych dzienników

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Odwołanie:  [ /log (devenv.exe)](http://msdn.microsoft.com/en-us/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Dzienniki Mac

Możesz wybrać **Przejdź > Przejdź do folderu** menu elementu wyszukiwania, a następnie skopiuj i Wklej żadnego z tych ścieżek w oknie dialogowym.

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio for Mac

`~/Library/Logs/VisualStudio/7.0` (ta liczba ulec zmianie w zależności od używanej wersji)

Ten folder można również otworzyć za pomocą "Help -> Otwórz katalog dziennika".

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (ta liczba ulec zmianie w zależności od używanej wersji)

Ten folder można również otworzyć za pomocą "Help -> Otwórz katalog dziennika".

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />Instalator "Uniwersalne" Xamarin

`~/Library/Logs/XamarinInstaller/Universal`

Są to dzienniki z `XamarinInstaller.dmg` Instalatora.

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Host kompilacji usługi Xamarin

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />Dane wyjściowe kompilacji pełne

1.  Włącz [wyjście diagnostyczne MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).

2.  Dla aplikacji systemu iOS, należy również włączyć **mtouch pełne dane wyjściowe** przez dodanie `-v -v -v -v` w obszarze **właściwości projektu > kompilacji systemu iOS > Ogólne [tab] > dodatkowe opcje > mtouch dodatkowe argumenty**.

    **Visual Studio (Windows)**

    [![](https://kb.xamarin.com/customer/portal/attachments/337319 "Wprowadź cztery dash v do pola wejściowego mtouch dodatkowe argumenty")](https://kb.xamarin.com/customer/portal/attachments/337319)

    **Visual Studio for Mac**

    [![](https://kb.xamarin.com/customer/portal/attachments/337316 "Wprowadź cztery dash v do pola wejściowego mtouch dodatkowe argumenty")](https://kb.xamarin.com/customer/portal/attachments/337316)

3.  Czyszczenie i skompiluj ponownie projekt.

4.  Skopiuj i wklej dane wyjściowe kompilacji z IDE do pliku tekstowego.
     - Visual Studio (z systemem Windows): **Widok > wyjściowy > Pokaż dane wyjściowe z: kompilacji**
     - Visual Studio dla komputerów Mac: **Widok > konsole > błędy > dane wyjściowe kompilacji [tab]**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Debugowanie dzienniki dla aplikacji platformy Xamarin.Android i Xamarin.iOS

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Widok > konsole > dane wyjściowe aplikacji**

![](https://kb.xamarin.com/customer/portal/attachments/337290 "Ikona Konsola danych wyjściowych aplikacji w programie Visual Studio dla komputerów Mac")


(Należy pamiętać, że ten element menu zostaną wyświetlone dopiero po uruchomieniu aplikacji).

### <a name="visual-studio"></a>Visual Studio

**Widok > wyjściowy > Pokaż dane wyjściowe z: debugowanie**

![](https://kb.xamarin.com/customer/portal/attachments/337292 "Konsola danych wyjściowych przedstawiający opcji debugowania w programie Visual Studio")

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Android [ `adb` ](http://developer.android.com/tools/help/adb.html) dzienniki logcat

Po uruchomieniu `adb` polecenia, Dołącz Wstecz **android_logcat.txt** pliku z pulpitu. W instrukcjach przyjęto założenie, że tylko jeden podłączonych urządzeń.

Zobacz też [dzienników debugowania systemu Android](~/android/deploy-test/debugging/android-debug-log.md) strony.

#### <a name="visual-studio"></a>Visual Studio

1.  **Narzędzia > Android > Wiersz polecenia Adb systemu Android**
2.  Czyszczenie dziennika: `adb logcat -c`
3.  Odtwórz problem.
4.  Dane wyjściowe w dzienniku: `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1.  **Narzędzia > Otwórz wiersz polecenia z zestawu SDK systemu Android**
2.  Czyszczenie dziennika: `adb logcat -c`
3.  Odtwórz problem.
4.  Dane wyjściowe w dzienniku: `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />symulatora systemu iOS logowania (Mac)

*   Aby uzyskać dostęp do dziennika systemu, zaznacz **Debuguj > Otwórz dziennik systemu...**  w aplikacji symulatora systemu iOS.

    ![](https://kb.xamarin.com/customer/portal/attachments/382617 "Debugowanie menu przedstawiający opcji Otwórz dziennik systemu")

*   Aby wyświetlić raporty awarii w symulatorze, otwórz Console.app i przejdź do `~/Library/Logs > DiagnosticReports`.

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />urządzenia z systemem iOS logowania (Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Widok > konsole > iOS dziennika urządzenia**

#### <a name="xcode"></a>Xcode 

**Okno > urządzenia > ${DeviceName}**

Raporty o awarii są dostępne w obszarze **Wyświetl dzienniki urządzenia** przycisku. W dzienniku systemu urządzenie pojawi się w dolnej części okna w obszarze strzałkę ujawnienie <img alt="Disclosure arrow" src="https://kb.xamarin.com/customer/portal/attachments/382618" style="width: 15px; height: 12px;" />.

#### <a name="xcode-5"></a>Xcode 5

**Okno > organizatora > urządzeń [tab] > ${DeviceName}**
