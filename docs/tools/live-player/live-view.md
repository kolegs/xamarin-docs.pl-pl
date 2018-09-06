---
redirect_url: /xamarin/tools/live-player/
title: XAML na żywo podglądu
description: W tym dokumencie omówiono sposób użycia aplikacji Xamarin Live Player Podgląd stron XAML na żywo, wprowadzić zmiany w XAML i zobaczyć zmiany, pojawiają się natychmiast na urządzeniu.
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
author: topgenorth
ms.author: toopge
ms.date: 08/08/2018
ms.openlocfilehash: 200d19aa0a13d0557e52cb90021190978838ed39
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780568"
---
# <a name="xaml-live-previewing"></a>XAML na żywo podglądu

Jedną z zalet platformy Xamarin Live Player jest możliwość Podgląd stron XAML na żywo, wprowadzić zmiany do kodu w programie Visual Studio i zobaczyć zmiany, pojawiają się natychmiast na urządzeniu. Podgląd na żywo można wprowadzić na urządzeniu z systemem Android lub na symulatorze lub w emulatorze. Ten przewodnik pokazuje, jak używać funkcji podglądu na żywo, aby wyświetlić poszczególne ekrany XAML.

## <a name="requirements"></a>Wymagania

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Maszyna z systemem Windows 7 lub nowszy.
2. Visual Studio 2017 w wersji 15.4 lub nowszym oraz **opracowywania aplikacji mobilnych przy użyciu platformy .NET** zainstalowanym obciążeniem.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac OS X 10.11, z systemem macOS 10.12 lub nowszej.
2. Program Visual Studio dla komputerów Mac 7.2 lub nowszego. Firma Microsoft zaleca najnowszej wersji.

-----

<a name="deploydevice" />

## <a name="deploying-to-device"></a>Wdrażania aplikacji na urządzeniu

Zanim użyjesz aplikacji Xamarin Live Player urządzeń z systemem Android, musisz pobrać aplikację Xamarin Live Player i Sparuj go do programu Visual Studio, zgodnie z opisem w [zainstalować](~/tools/live-player/install.md) przewodnik. Gdy zostały pomyślnie sparowane urządzenie do programu Visual Studio, możesz rozpocząć na żywo Podgląd strony XAML. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Otwórz stronę XAML, który chcesz podglądu na żywo w edytorze programu Visual Studio 2017:

    ![](live-view-images/vs-image1.png)

2. Ustaw konfigurację urządzenia **debugowania** i z listy wybierz urządzenie Live Player:

    ![](live-view-images/vs-image2.png)

3. Aby uruchomić tę stronę XAML jako widok na żywo na twoim urządzeniu, wybierz **Narzędzia > Xamarin Live Player > Live bieżącego widoku uruchamiania** na pasku menu:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Otwórz stronę XAML, która ma być podglądu na żywo w programie Visual Studio dla komputerów Mac edytora:

    ![](live-view-images/image1.png)

2. Ustaw konfigurację urządzenia **debugowania** i z listy wybierz urządzenie Live Player:

    ![](live-view-images/image2.png)

3. Aby uruchomić tę stronę XAML jako widok na żywo na twoim urządzeniu, wybierz **Uruchom > Live bieżącego widoku uruchamiania** na pasku menu:

    ![](live-view-images/image3.png)

-----

## <a name="deploying-to-android-emulator"></a>Wdrażanie do emulatora systemu Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Otwórz stronę XAML, który chcesz podglądu na żywo w edytorze programu Visual Studio 2017:

    ![](live-view-images/vs-image1.png)

2. Ustaw konfigurację urządzenia **debugowania** dla systemów Android i urządzenie Live Player, z listy wybierz pozycję:

    ![](live-view-images/vs-image4.png)

3. Aby uruchomić tę stronę XAML jako widok na żywo w emulatorze systemu Android, wybierz **Narzędzia > Xamarin Live Player > Live bieżącego widoku uruchamiania** na pasku menu:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otwórz stronę XAML, która ma być podglądu na żywo w programie Visual Studio dla komputerów Mac edytora:

    ![](live-view-images/image7.png)

2. Ustaw konfigurację urządzenia **debugowania** dla systemów Android i urządzenie Live Player, z listy wybierz pozycję:

    ![](live-view-images/image6.png)

3. Aby uruchomić tę stronę XAML jako widok na żywo na twoim urządzeniu, wybierz polecenie Uruchom > Live bieżącego widoku uruchamiania na pasku menu:

    ![](live-view-images/image3.png)

-----

## <a name="related-links"></a>Linki pokrewne

- [Xamarin Live Player Przegląd](https://xamarin.com/live)
- [Wpis w blogu](https://blog.xamarin.com/live-player/)
- [Przykłady aplikacji Xamarin Live Player](~/tools/live-player/samples.md)
