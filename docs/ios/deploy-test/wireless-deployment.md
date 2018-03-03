---
title: "Wdrażanie bezprzewodowej"
description: "Ta funkcja podglądu umożliwia wdrożenia dla systemu iOS lub urządzeń Apple TV za pośrednictwem połączenia sieciowego"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AB4C5A9-4FBB-4DCB-BD72-0022D5439E65
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/09/2018
ms.openlocfilehash: 11961a21a7c4188c505c822a35531036fd953405
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="wireless-deployment"></a>Wdrażanie bezprzewodowej

_Ta funkcja podglądu umożliwia wdrożenia dla systemu iOS lub urządzeń Apple TV za pośrednictwem połączenia sieciowego_

![Wersja zapoznawcza](~/media/shared/preview.png)

Ważnym elementem developer przepływu pracy jest wdrażana na urządzeniu. Xcode 9 wprowadzono możliwość wdrażania na urządzeniu z systemem iOS lub Apple TV za pośrednictwem sieci, zamiast do sprzętowych urządzeń za każdym razem, gdy chcesz wdrożyć, a debugowanie aplikacji. Ta funkcja został wprowadzony w programie Visual Studio w wersji systemów Mac i Visual Studio 15,6, który jest obecnie w wersji zapoznawczej.

Ten przewodnik zawiera szczegóły parę i wdrażanie do urządzenia za pośrednictwem sieci.

## <a name="requirements"></a>Wymagania

Wdrażanie bezprzewodowej jest dostępna jako **Podgląd** funkcji w Visual Studio dla komputerów Mac i Visual Studio.


Przy użyciu wdrażania sieci bezprzewodowej, należy dysponować następującymi elementami:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- macOS 10.12.4
- Najnowszej wersji wstępnej programu Visual Studio dla komputerów Mac 
    - Aby zainstalować ten przełącznik, aby [kanału alfa lub Beta](https://docs.microsoft.com/en-us/visualstudio/mac/update) w programie Visual Studio dla komputerów Mac.
- Xcode 9.0 lub nowszej
- Urządzenia z systemem iOS 11.0 lub systemu tvOS 11.0 lub nowszy

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- R [wersji zapoznawczej](https://www.visualstudio.com/vs/preview/) programu Visual Studio
- Urządzenia z systemem iOS 11.0 lub systemu tvOS 11.0 lub nowszy

Na hoście kompilacji Mac należy zainstalować następujące składniki:

- macOS 10.12.4
- Visual Studio for Mac w wersji zapoznawczej
    - Aby zainstalować przełącznik, aby [kanału alfa lub Beta](https://docs.microsoft.com/en-us/visualstudio/mac/update) w programie Visual Studio dla komputerów Mac.
- Xcode 9.0 lub nowszej

-----

## <a name="connecting-a-device"></a>Podłączanie urządzenia

Aby wdrożyć i bezprzewodowo debugowania na twoim urządzeniu, musi skojarzyć urządzenie z systemem iOS lub Apple TV z Xcode opartym na systemie Po łączyć, można wybrać go z listy docelowej urządzeń w programie Visual Studio. 

Następujący proces parowania wystarcza tylko nastąpić raz na urządzenie. Xcode, zachowują ustawienia połączenia.

<a name="pair" />

### <a name="pairing-an-ios-device-with-xcode"></a>Parowanie urządzenia z systemem iOS z Xcode

1. Otwórz środowisko Xcode i przejdź do **okna > urządzenia i symulatorów**.
2. Podłącz urządzenie z systemem iOS do komputera Mac za pomocą kabla lightning. Należy wybrać opcję **zaufania na tym komputerze** na urządzeniu.
3. Wybierz urządzenie, a następnie wybierz **Połącz za pośrednictwem sieci** pole wyboru, aby skojarzyć urządzenia: ![urządzenia i symulatora okno Połącz przy użyciu opcji sieci](wireless-deployment-images/image2.png)

### <a name="pairing-an-apple-tv-with-xcode"></a>Parowanie Apple TV z Xcode

1. Upewnij się, Telewizora Mac i Apple są podłączone do tej samej sieci.

2. Otwórz środowisko Xcode i przejdź do **okna > urządzenia i symulatorów**.

3. Na Apple TV, przejdź do **Ustawienia > element zdalny i urządzenia > zdalnej aplikacji i urządzeń**.

4. Wybierz Apple TV w **odnalezionej** obszaru w środowisku Xcode, a następnie wprowadź kod weryfikacyjny wyświetlany w Apple TV.

5. Kliknij przycisk **Connect** przycisku. Podczas pomyślnie parowania obok Apple TV pojawi się ikona połączenia sieciowego.

## <a name="deploy-to-a-device"></a>Wdrażanie do urządzenia

Gdy urządzenie jest podłączone bezprzewodowo i gotowe do zastosowania w przypadku wdrażania jest wyświetlane na liście urządzeń docelowych tak, jakby urządzenia zostały połączone za pośrednictwem portu USB.

Aby przetestować na urządzeniu fizycznym, urządzenie musi być [elastycznie](~/ios/get-started/installation/device-provisioning/index.md). Upewnij się to zrobić przed przystąpieniem do wdrożenia na urządzeniu. 

Do wdrożenia na urządzeniu z systemem iOS lub systemu tvOS, wykonaj następujące kroki:

1. Upewnij się, że wdrożenia urządzenia maszyny i obiekt docelowy znajdują się w tej samej sieci bezprzewodowej. 

2. Wybierz urządzenie z listy urządzeń docelowych i uruchomić aplikację.

2. Jeśli urządzenie jest zablokowane, zostanie wyświetlony monit do odblokowywania urządzenia. Po odblokowaniu urządzenia aplikacja jest wdrażana na urządzeniu.

Debugowanie bezprzewodowego jest automatycznie włączone po wdrożeniu sieci bezprzewodowej, aby można było użyć wcześniej Ustaw punkty przerwania i kontynuować debugowanie przepływu pracy, jak będzie zawsze gotowe.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

1. Upewnij się, że urządzenie z systemem iOS lub Apple TV, są połączone z tej samej sieci co Twoje Mac.

2. Jeśli urządzenia nie są wyświetlane w programie Visual Studio, sprawdź, czy w środowisku Xcode **urządzenia i symulatorów** okna. 

    * Jeśli środowisko Xcode **nie** pokazu urządzenie podłączone, spróbuj [pary](#pair) ponownie urządzenie.

    * Jeśli środowisko Xcode pokazuj urządzenie jako podłączone, ponowne uruchomienie programu Visual Studio i na urządzeniu.

3. Jeśli nie zostało to jeszcze zrobione, konieczne będzie [udostępniania](~/ios/get-started/installation/device-provisioning/index.md) urządzenia.

4. Jeśli masz problemy z tej funkcji, które nie mogą być ustalone w poprzednich krokach pliku problemu w [społeczność deweloperów](https://developercommunity.visualstudio.com/spaces/41/index.html).

## <a name="related-links"></a>Linki pokrewne

- [Para urządzenia bezprzewodowe z Xcode](https://help.apple.com/xcode/mac/9.0/index.html?localePath=en.lproj#/devbc48d1bad)
