---
title: Profilowanie aplikacji platformy Xamarin.iOS za pomocą narzędzia Instruments
description: W tym dokumencie opisano sposób użycia aplikacji Instruments firmy Apple do profilu aplikacji platformy Xamarin.iOS zainstalowana na urządzeniu lub symulatora.
ms.prod: xamarin
ms.assetid: 70A8CAC8-20C2-655B-37C3-ACF9EA7874D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 9b6168eba91a87af88891b9e07e3dd395301cc48
ms.sourcegitcommit: 021027b78cb2f8061b03a7c6ae59367ded32d587
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39182211"
---
# <a name="profiling-xamarinios-applications-with-instruments"></a>Profilowanie aplikacji platformy Xamarin.iOS za pomocą narzędzia Instruments

Środowisko Xcode **instrumentów** to narzędzie, który może służyć do profilowanie aplikacji platformy Xamarin.iOS na urządzeniu lub w symulatorze. Narzędzie mono używa jej Just-in-Time model, aby skompilować kod i instrumenty nie interpretacji tego rodzaju danych, więc może być trudne do pracy z danymi wyjściowymi aplikacji opartych na symulatorze, korzystających z dokumentów.
Ze względu na ten problem ten przewodnik koncentruje się na temat sposobu interpretacji danych wyjściowych dokumentów, w tym dokumencie za pomocą aplikacji dla deweloperów.

## <a name="requirements"></a>Wymagania

Instrumentów programu Xcode jest uruchamiany tylko na komputerach Mac.

## <a name="opening-the-instruments-app"></a>Otwieranie dokumentów aplikacji

Wybierz urządzenie, a następnie uruchom aplikację w instrumenty:

1. Otwórz projekt rozszerzenia Xamarin.iOS w programie Visual Studio dla komputerów Mac.
2. Wybierz **debugowanie | iPhone** konfiguracji.
3. Podłącz urządzenie z systemem iOS do komputera.
4. W **Uruchom** menu, wybierz opcję **Przekaż do urządzenia** . Aplikacja zostanie teraz utworzone i przekazane do urządzenia.
5. W **narzędzia** menu, wybierz opcję **Uruchom instrumenty**.


Dokumenty teraz otworzyć i wyświetlić to okno dialogowe:

 [![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png "Wybieranie szablonu profilowania")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png#lightbox)

Kliknij, aby wybrać **alokacje** szablonu. Inne szablony są prawidłowe, ale w tym artykule omówiono tylko **alokacje** szablon profilu.

Następnie wybierz urządzenia i aplikacji przy użyciu menu w górnej części okna:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png "Wybierz urządzenia i aplikacji")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png#lightbox)

Urządzenia z systemem iOS powinno być zaznaczone w menu u góry okna i aplikacji, które mają być profilowane, należy wybrać obok niego (**MemoryDemo** naz zrzucie ekranu).

Jeśli urządzenie nie ma na liście w obszarze menu, sprawdź **konsoli** w programie Visual Studio dla komputerów Mac, komunikaty o błędach, które mogą być wyświetlane, gdy aplikacja jest wdrożona na urządzeniu. Ponadto upewnij się, że urządzenie zostało zaaprowizowane do tworzenia aplikacji za pomocą organizatora Xcode.

Kliknij przycisk **wybierz** przycisku i następnym ekranem powinien pojawić się:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png "Interfejs profilowania")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png#lightbox)

Kliknij przycisk rekordu (czerwone kółko w lewym górnym rogu), aby rozpocząć profilowanie.

Poniższy zrzut ekranu przedstawia przykład profilowania przy użyciu **instrumentów**:

[![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png "Przykładem profilowania przy użyciu instrumenty")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png#lightbox)

## <a name="summary"></a>Podsumowanie

W przewodniku pokazano, jak uruchomić instrumentów programu Xcode do monitorowania aplikacji dla systemu iOS z poziomu programu Visual Studio dla komputerów Mac. Przejdź do [wskazówki instrumentów](~/ios/deploy-test/walkthrough-apples-instrument.md) przykładowy sposób diagnozowania problemu pamięci, za pomocą dokumentów.

## <a name="related-links"></a>Linki pokrewne

- [Przewodnik instrumenty](~/ios/deploy-test/walkthrough-apples-instrument.md)
- [Xamarin.iOS wyrzucania elementów bezużytecznych (wpis na blogu)](http://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/)
