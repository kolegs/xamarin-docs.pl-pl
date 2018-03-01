---
title: Instalowanie platformy Xamarin w programie Visual Studio w systemie Windows
ms.topic: article
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: b68e03251b83192bdc5836af6ea54446ddaad24a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="installing-xamarin-in-visual-studio-on-windows"></a>Instalowanie platformy Xamarin w programie Visual Studio w systemie Windows

Ponieważ Xamarin jest teraz dostępna w ramach wszystkich wersji programu Visual Studio bez dodatkowych kosztów i nie wymaga oddzielnej licencji, Instalator programu Visual Studio można użyć, aby pobrać i zainstalować narzędzia Xamarin.

-   [Wymagania](#requirements)
-   [Instalacja](#installation)
-   [Dodawanie Xamarin do programu Visual Studio 2017 r.](#vs2017)
-   [Dodawanie Xamarin do programu Visual Studio 2015](#vs2015)
-   [Weryfikowanie instalacji](#verifying)
-   [Następne kroki](#nextsteps)


<a name="requirements" />

# <a name="requirements"></a>Wymagania

Poniżej przedstawiono wymagania dotyczące instalowania programu Visual Studio tools dla platformy Xamarin:

1. Windows 7 lub nowszy.

2. Visual Studio 2015 lub 2017 (Community, Professional lub Enterprise).

3. Xamarin dla Visual Studio.

Należy pamiętać, że Xamarin nie można używać z wersji Express programu Visual Studio z powodu braku obsługi wtyczki.

Aby uzyskać więcej informacji na temat wymagania wstępne dotyczące instalowania i używania Xamarin, zobacz [wymagania systemowe](~/cross-platform/get-started/requirements.md).


<a name="installation" />

# <a name="installation"></a>Instalacja

Xamarin można zainstalować w ramach nowej instalacji programu Visual Studio.
Aby to osiągnąć, wykonaj następujące kroki:

1. Pobierz program Visual Studio Community, Visual Studio Professional lub Visual Studio Enterprise z [programu Visual Studio](https://www.visualstudio.com/vs/) strony (do pobrania łącza znajdują się u dołu).

2. Kliknij dwukrotnie pobrany pakiet, aby rozpocząć instalację.

3. Wybierz **Mobile development z platformą .NET** obciążenia na ekranie instalacji: 

    [![Tworzenie przenośnych za pomocą wyboru .NET na ekranie obciążeń](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png)

4. Gdy **Mobile development z platformą .NET** jest zaznaczone, mieć przyjrzeć się **Podsumowanie** panelu po prawej stronie. W tym miejscu możesz wyłączyć opcje wdrażania przenośnych, których nie chcesz zainstalować. Domyślnie instalowane są wszystkie opcje pokazano na poniższym zrzucie ekranu (**skoroszytów Xamarin**, **profilera Xamarin**, **symulatora zdalny Xamarin**,  **Zestaw android NDK**, **zestawu SDK systemu Android**, **Java SE Development Kit**, **Emulator systemu Google Android**, **F # Obsługa**, i **Intel HAXM**):

    ![Panel Podsumowanie listę opcji Xamarin, aby zainstalować](windows-images/02-summary.png)

5. Gdy wszystko będzie gotowe do rozpoczęcia instalacji programu Visual Studio, kliknij przycisk **zainstalować** przycisk w prawym dolnym rogu:

    ![Lokalizacja instalacji przycisku](windows-images/03-click-install.png)

   W zależności od wersji programu Visual Studio są instalowane proces instalacji może trwać bardzo długo. Paski postępu służy do monitorowania instalacji:

    ![Zrzut ekranu pasków postępu podczas instalacji](windows-images/04-progress-bars.png)

6. Po zakończeniu instalacji programu Visual Studio, kliknij przycisk **uruchamianie** przycisk, aby uruchomić program Visual Studio:

    ![Lokalizacja przycisk uruchamiania](windows-images/05-launch.png)


<a name="vs2017" />

## <a name="adding-xamarin-to-visual-studio-2017"></a>Dodawanie Xamarin do programu Visual Studio 2017 r.

Jeśli jest już zainstalowany program Visual Studio 2017, ponownie uruchamiając Instalatora programu Visual Studio, aby zmodyfikować obciążeń można dodać Xamarin (zobacz [zmodyfikować Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) szczegółowe informacje). Następnie wykonaj kroki wymienione powyżej do zainstalowania programu Xamarin.

Aby uzyskać więcej informacji o pobranie i zainstalowanie programu Visual Studio 2017, zobacz [zainstalować program Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


<a name="vs2015" />

## <a name="adding-xamarin-to-visual-studio-2015"></a>Dodawanie Xamarin do programu Visual Studio 2015

Aby dodać Xamarin.Android do istniejącej instalacji programu Visual Studio 2015, użyj następujących kroków:

1. Kliknij prawym przyciskiem myszy systemu Windows **Start** i wybrać **programy i funkcje**.

2. Kliknij prawym przyciskiem myszy **programu Microsoft Visual Studio** i kliknij przycisk **zmiany**.

3. Kiedy wyświetli się okno dialogowe Instalator programu Visual Studio, kliknij przycisk **Modyfikuj** przycisku.

4. W **funkcje** karcie, przewiń w dół do **Cross Platform Mobile Development**. Kliknij pole wyboru obok pozycji **C# / .NET (Xamarin)**:

    ![Dodawanie C# / .NET Xamarin programu Visual Studio 2015](windows-images/06-add-xamarin.png)

5. Kliknij przycisk **aktualizacji** przycisk, aby dodać Xamarin dla programu Visual Studio.


<a name="verifying" />

## <a name="verifying-installation"></a>Weryfikowanie instalacji

W programie Visual Studio 2017 r, można sprawdzić, Xamarin jest zainstalowana, klikając **pomocy** menu. Jeśli zainstalowano program Xamarin, powinny pojawić się **Xamarin** element menu, jak pokazano w tym zrzut ekranu:

![Element menu Xamarin wyświetlany w menu Pomoc](windows-images/12-xamarin-menu-item.png)

Jeśli używasz starszej wersji programu Visual Studio, możesz kliknąć **Pomoc > Microsoft Visual Studio** i przewiń listę zainstalowanych produktów, aby zobaczyć, czy jest zainstalowany program Xamarin:

![Visual Studio zainstalowanych produktów ekranu](windows-images/13-xamarin-is-installed.png)

Aby uzyskać więcej informacji na temat lokalizowania informacji o wersji, zobacz [gdzie mogę znaleźć Moje informacje o wersji i dzienniki?](~/cross-platform/troubleshooting/questions/version-logs.md)

<a name="nextsteps" />

# <a name="next-steps"></a>Następne kroki

Instalowanie narzędzi Visual Studio Tools dla platformy Xamarin umożliwia uruchamianie pisanie kodu dla aplikacji, ale wymagają dodatkowej konfiguracji umożliwiające tworzenie i wdrażanie aplikacji w symulatorze emulatora i urządzenia. Można znaleźć w następujących przewodnikach, aby zakończyć instalację i rozpocząć tworzenie aplikacji międzyplatformowego.

## <a name="ios"></a>iOS

Aby uzyskać szczegółowe informacje, zobacz [Xamarin.iOS instalowania w systemie Windows](~/ios/get-started/installation/windows/index.md) przewodnik. 

1. [Zainstaluj narzędzia platformy Xamarin.iOS na komputerze Mac](~/ios/get-started/installation/windows/index.md#installation)
2. [Konfigurowanie komputerów Mac](~/ios/get-started/installation/windows/index.md#configuration)
3. [Konfiguracja dewelopera z systemem iOS](~/ios/get-started/installation/windows/index.md#developersetup) (do uruchamiania aplikacji na urządzeniu).
4. [Visual Studio nawiązywania połączenia z hostem kompilacji Mac](~/ios/get-started/installation/windows/index.md#connectingtomac)
5. [Zdalny symulatora systemu iOS](~/tools/ios-simulator.md)
6. [Wprowadzenie do platformy Xamarin.iOS dla programu Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

## <a name="android"></a>Android

Aby uzyskać szczegółowe informacje, zobacz [instalowanie platformy Xamarin.Android w systemie Windows](~/android/get-started/installation/windows.md) przewodnik.

1. [Xamarin.Android Configuration](~/android/get-started/installation/windows.md#configuration)
2. [Za pomocą platformy Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Zestaw SDK systemu android Emulator](~/android/get-started/installation/android-emulator/index.md)
4. [Konfigurowanie urządzeń środowiska deweloperskiego](~/android/get-started/installation/set-up-device-for-development.md)
