---
title: "Profilowanie aplikacji platformy Xamarin.iOS z dokumentów"
description: "Jak korzystać z aplikacji platformy Xamarin.iOS na urządzeniu lub w symulatorze dokumentów."
ms.topic: article
ms.prod: xamarin
ms.assetid: 70A8CAC8-20C2-655B-37C3-ACF9EA7874D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 9fec6fca0bf7930a59a6927557786a3ed7a412da
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="profiling-xamarinios-applications-with-instruments"></a>Profilowanie aplikacji platformy Xamarin.iOS z dokumentów

_Jak korzystać z aplikacji platformy Xamarin.iOS na urządzeniu lub w symulatorze dokumentów._

Xcode **instrumentów** to narzędzie, które może służyć do do profilu aplikacji platformy Xamarin.iOS na urządzeniu lub w symulatorze. Mono używa jej Just in Time model, aby skompilować kod i dokumentów nie interpretacji tego typu danych również, więc może być trudne do pracy z danymi wyjściowymi symulatora aplikacjach korzystających z instrumentów.
Z powodu tego błędu w tym przewodniku będzie skoncentrować się na jak interpretować dane wyjściowe dokumentów, w tym dokumencie przy użyciu projektanta aplikacji.

## <a name="requirements"></a>Wymagania

Instrumentów środowiska Xcode działa tylko na komputerach Mac.

## <a name="opening-the-instruments-app"></a>Otwieranie dokumentów aplikacji

Wybierz urządzenie i uruchamianie aplikacji dokumentów:

1.  Otwórz projekt platformy Xamarin.iOS w programie Visual Studio dla komputerów Mac.
2.  Wybierz **Debug | iPhone** konfiguracji.
3.  Podłącz urządzenie z systemem iOS do komputera.
4.  W **Uruchom** menu, wybierz opcję **przekazać do urządzenia** . Aplikacja zostanie teraz utworzony i przekazane do urządzenia.
5.  W **narzędzia** menu, wybierz opcję **uruchamianie instrumentów**.


Dokumenty teraz otworzyć i wyświetlić to okno dialogowe:

 [ ![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png "Wybieranie szablonów profilowania")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments1.png)

Kliknij, aby wybrać **alokacji** szablonu. Inne szablony są prawidłowe, jednak w tym artykule omówiono tylko **alokacji** szablonu profilu.

Następnie wybierz urządzenie i aplikacji przy użyciu menu w górnej części okna:

[ ![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png "Wybierz urządzenia i aplikacji")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments2.png)

Urządzenia z systemem iOS powinna być wybrana w menu u góry okna i należy wybrać aplikację do profilowania obok niej (**MemoryDemo** na zrzucie ekranu powyżej).

Jeśli urządzenie nie jest wymieniony w obszarze w menu, sprawdź **konsoli** w programie Visual Studio for Mac komunikaty o błędach, które mogą być wyświetlane, gdy aplikacja jest wdrożona na urządzeniu. Upewnij się również, czy urządzenia zostały udostępnione do tworzenia aplikacji za pomocą organizatora Xcode.

Kliknij przycisk **wybierz** przycisk i następnym ekranie powinien zostać wyświetlony:

[ ![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png "Interfejs profilowania")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments3.png)

Kliknij przycisk rekordu (czerwone kółko w lewym górnym) do uruchomienia profilowania.

Poniższy zrzut ekranu przedstawia przykład profilowanie przy użyciu **instrumentów**:

[ ![](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png "Przykład profilowania za pomocą dokumentów")](using-instruments-to-detect-native-leaks-using-markheap-images/instruments4.png)

## <a name="summary"></a>Podsumowanie

W tym przewodniku pokazano, jak uruchomić instrumentów środowiska Xcode do monitorowania aplikacji systemu iOS z poziomu programu Visual Studio dla komputerów Mac. Przejdź do [wskazówki instrumentów](~/ios/deploy-test/walkthrough-apples-instrument.md) na przykład jak zdiagnozować problem pamięci za pomocą dokumentów.

## <a name="related-links"></a>Linki pokrewne

- [Wskazówki dokumentów](~/ios/deploy-test/walkthrough-apples-instrument.md)
- [Xamarin.iOS wyrzucanie elementów bezużytecznych](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/)
