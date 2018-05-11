---
title: Instalowanie platformy Xamarin w programie Visual Studio 2017 r.
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: 8aad42717d2408f97d40f5d244d797727ea12588
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="installing-xamarin-in-visual-studio-2017"></a>Instalowanie platformy Xamarin w programie Visual Studio 2017 r.

<a name="requirements" />

## <a name="requirements"></a>Wymagania

Poniżej przedstawiono wymagania dotyczące instalowania Xamarin w programie Visual Studio 2017:

1. Windows 7 lub nowszy.

2. Visual Studio 2017 r (Community, Professional lub Enterprise).

3. Xamarin dla Visual Studio.

Aby uzyskać więcej informacji na temat wymagania wstępne dotyczące instalowania i używania Xamarin, zobacz [wymagania systemowe](~/cross-platform/get-started/requirements.md).

<a name="installation" />

## <a name="installation"></a>Instalacja

Xamarin można zainstalować w ramach nowej instalacji programu Visual Studio 2017 r.
Aby to osiągnąć, wykonaj następujące kroki:

1. Pobieranie programu Visual Studio Community 2017 Visual Studio Professional i Visual Studio Enterprise z [programu Visual Studio](https://www.visualstudio.com/vs/) strony (do pobrania łącza znajdują się u dołu).

2. Kliknij dwukrotnie pobrany pakiet, aby rozpocząć instalację.

3. Wybierz **Mobile development z platformą .NET** obciążenia na ekranie instalacji: 

    [![Tworzenie przenośnych za pomocą wyboru .NET na ekranie obciążeń](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png#lightbox)

4. Gdy **Mobile development z platformą .NET** jest zaznaczone, mieć przyjrzeć się **Podsumowanie** panelu po prawej stronie. W tym miejscu możesz wyłączyć opcje wdrażania przenośnych, których nie chcesz zainstalować. Domyślnie instalowane są wszystkie opcje pokazano na poniższym zrzucie ekranu (**skoroszytów Xamarin**, **profilera Xamarin**, **symulatora zdalny Xamarin**,  **Zestaw android NDK**, **zestawu SDK systemu Android**, **Java SE Development Kit**, **Emulator systemu Google Android**, **F # Obsługa**, i **Intel HAXM**):

    ![Panel Podsumowanie listę opcji Xamarin, aby zainstalować](windows-images/02-summary.png)

5. Gdy wszystko będzie gotowe do rozpoczęcia instalacji programu Visual Studio 2017 kliknij **zainstalować** przycisk w prawym dolnym rogu:

    ![Lokalizacja instalacji przycisku](windows-images/03-click-install.png)

   W zależności od wersji programu Visual Studio 2017 instalujesz proces instalacji może potrwać bardzo długo. Paski postępu służy do monitorowania instalacji:

    ![Zrzut ekranu pasków postępu podczas instalacji](windows-images/04-progress-bars.png)

6. Po zakończeniu instalacji programu Visual Studio 2017 kliknij **uruchamianie** przycisk, aby uruchomić program Visual Studio:

    ![Lokalizacja przycisk uruchamiania](windows-images/05-launch.png)

<a name="vs2017" />

### <a name="adding-xamarin-to-visual-studio-2017"></a>Dodawanie Xamarin do programu Visual Studio 2017 r.

Jeśli jest już zainstalowany program Visual Studio 2017, ponownie uruchamiając Instalatora programu Visual Studio 2017 do modyfikowania obciążeń można dodać Xamarin (zobacz [zmodyfikować Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) szczegółowe informacje). Następnie wykonaj kroki wymienione powyżej do zainstalowania programu Xamarin.

Aby uzyskać więcej informacji o pobranie i zainstalowanie programu Visual Studio 2017, zobacz [zainstalować program Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


### <a name="verifying-installation"></a>Weryfikowanie instalacji

W programie Visual Studio 2017 r, można sprawdzić, Xamarin jest zainstalowana, klikając **pomocy** menu. Jeśli zainstalowano program Xamarin, powinny pojawić się **Xamarin** element menu, jak pokazano w tym zrzut ekranu:

![Element menu Xamarin wyświetlany w menu Pomoc](windows-images/12-xamarin-menu-item.png)

Jeśli używasz starszej wersji programu Visual Studio, możesz kliknąć **Pomoc > Microsoft Visual Studio** i przewiń listę zainstalowanych produktów, aby zobaczyć, czy jest zainstalowany program Xamarin:

![Visual Studio zainstalowanych produktów ekranu](windows-images/13-xamarin-is-installed.png)

Aby uzyskać więcej informacji na temat lokalizowania informacji o wersji, zobacz [gdzie mogę znaleźć Moje informacje o wersji i dzienniki?](~/cross-platform/troubleshooting/questions/version-logs.md)

<a name="nextsteps" />

## <a name="next-steps"></a>Następne kroki

Instalowanie platformy Xamarin w programie Visual Studio 2017 umożliwia uruchamianie pisanie kodu dla aplikacji, ale wymagają dodatkowej konfiguracji umożliwiające tworzenie i wdrażanie aplikacji w symulatorze emulatora i urządzenia. Można znaleźć w następujących przewodnikach, aby zakończyć instalację i rozpocząć tworzenie aplikacji międzyplatformowego.

### <a name="ios"></a>iOS

Aby uzyskać szczegółowe informacje, zobacz [Xamarin.iOS instalowania w systemie Windows](~/ios/get-started/installation/windows/index.md) przewodnik. 

1. [Zainstaluj program Visual Studio dla komputerów Mac](https://docs.microsoft.com/visualstudio/mac/installation)
2. [Połączenie programu Visual Studio na hoście z systemem Mac kompilacji](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
3. [Konfiguracja dewelopera z systemem iOS](~/ios/get-started/installation/device-provisioning/index.md) — wymagany do uruchamiania aplikacji na urządzeniu
5. [Zdalny symulatora systemu iOS](~/tools/ios-simulator.md)
6. [Wprowadzenie do rozwiązania Xamarin.iOS dla programu Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

### <a name="android"></a>Android

Aby uzyskać szczegółowe informacje, zobacz [instalowanie platformy Xamarin.Android w systemie Windows](~/android/get-started/installation/windows.md) przewodnik.

1. [Xamarin.Android Configuration](~/android/get-started/installation/windows.md#configuration)
2. [Za pomocą platformy Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Emulator zestawu SDK systemu Android](~/android/get-started/installation/android-emulator/index.md)
4. [Konfigurowanie urządzenia na potrzeby programowania](~/android/get-started/installation/set-up-device-for-development.md)
