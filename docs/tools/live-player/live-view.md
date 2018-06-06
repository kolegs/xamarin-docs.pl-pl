---
redirect_url: /xamarin/tools/live-player/
title: XAML na żywo podglądu
description: Ten dokument omówiono sposób za pomocą platformy Xamarin Live odtwarzacza live preview strony XAML, wprowadzić zmiany w pliku XAML i zobaczyć zmiany pojawiają się teraz na urządzeniu.
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
author: topgenorth
ms.author: toopge
ms.date: 12/21/2017
ms.openlocfilehash: cc68044342fca84e62e3b17770170e1d7a23f677
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793705"
---
# <a name="xaml-live-previewing"></a>XAML na żywo podglądu

Jedną z zalet Xamarin Live Player jest możliwość live preview strony XAML, wprowadzić zmiany w kodzie w programie Visual Studio i zobaczyć zmiany pojawiają się teraz na urządzeniu. Podgląd na żywo można wprowadzić na urządzenia z systemem iOS lub Android lub w symulatorze lub emulator. W tym przewodniku pokazano, jak funkcja podglądu na żywo umożliwia wyświetlanie poszczególnych ekranach XAML.

## <a name="requirements"></a>Wymagania

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Komputera z uruchomionym systemem Windows 7 lub nowszy.
2. Visual Studio 2017 wersji 15,4 lub nowszym oraz **Mobile development z platformą .NET** obciążenia zainstalowane.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac OS X 10.11 macOS 10.12 lub nowszego.
2. Program Visual Studio dla komputerów Mac 7.2 lub nowszego. Firma Microsoft zaleca najnowszej wersji.

-----

<a name="deploydevice" />

## <a name="deploying-to-device"></a>Wdrażanie do urządzenia

Zanim użyjesz Xamarin Live Player z urządzenia z systemem iOS lub Android, musisz pobrać aplikację platformy Xamarin Player na żywo i Sparuj go do programu Visual Studio, zgodnie z opisem w [zainstalować](~/tools/live-player/install.md) przewodnik. Po zostały pomyślnie łączyć się urządzenia do programu Visual Studio, możesz rozpocząć na żywo podglądu strony XAML. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Otwórz stronę XAML, który ma zostać Podgląd na żywo w edytorze programu Visual Studio 2017:

    ![](live-view-images/vs-image1.png)

2. Ustaw dla konfiguracji urządzenia **Debug | iPhone** dla systemu iOS lub **debugowania** dla systemu Android, a następnie wybierz urządzenie Player na żywo z listy:

    ![](live-view-images/vs-image2.png)

3. Do uruchomienia tej strony XAML jako widok na żywo na urządzeniu, wybierz **Narzędzia > Xamarin Live Player > Live uruchom bieżący widok** na pasku menu:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Otwórz stronę XAML, która ma zostać Podgląd na żywo w programie Visual Studio dla edytora Mac:

    ![](live-view-images/image1.png)

2. Ustaw dla konfiguracji urządzenia **Debug | iPhone** dla systemu iOS lub **debugowania** dla systemu Android, a następnie wybierz urządzenie Player na żywo z listy:

    ![](live-view-images/image2.png)

3. Aby uruchomić tę stronę XAML jako widok na żywo na urządzeniu, wybierz **Uruchom > Live uruchom bieżący widok** na pasku menu:

    ![](live-view-images/image3.png)

-----

## <a name="deploying-to-android-emulator"></a>Wdrażanie na emulatorze systemu Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Otwórz stronę XAML, który ma zostać Podgląd na żywo w edytorze programu Visual Studio 2017:

    ![](live-view-images/vs-image1.png)

2. Ustaw dla konfiguracji urządzenia **debugowania** dla systemów Android i wybierz urządzenie Player na żywo z listy:

    ![](live-view-images/vs-image4.png)

3. Aby uruchomić tę stronę XAML jako widok na żywo w emulatorze systemu Android, zaznacz **Narzędzia > Xamarin Live Player > Live uruchom bieżący widok** na pasku menu:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otwórz stronę XAML, która ma zostać Podgląd na żywo w programie Visual Studio dla edytora Mac:

    ![](live-view-images/image7.png)

2. Ustaw dla konfiguracji urządzenia **debugowania** dla systemów Android i wybierz urządzenie Player na żywo z listy:

    ![](live-view-images/image6.png)

3. Aby uruchomić tę stronę XAML jako widok na żywo na urządzeniu, wybierz polecenie Uruchom > Live uruchom bieżący widok na pasku menu:

    ![](live-view-images/image3.png)

-----

## <a name="deploying-to-ios-simulator"></a>Wdrażanie do symulatora systemu iOS

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Obecnie nie jest obsługiwane dla przy użyciu podglądu XAML na żywo w symulatorze iOS zdalny w systemie Windows. Zamiast tego należy [wdrożenia na urządzeniu](#deploydevice).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otwórz stronę XAML, która ma zostać Podgląd na żywo w programie Visual Studio dla edytora Mac:

    ![](live-view-images/image1.png)

2. Ustaw dla konfiguracji urządzenia **Debug | iPhoneSimulator** dla systemów iOS i wybierz symulatora systemu iOS z listy:

    ![](live-view-images/image2.png)

3. Wybierz **Uruchom > Live uruchom bieżący widok** na pasku menu, do uruchomienia symulatora i wyświetlania strony XAML:

    ![](live-view-images/image4.png)

4. Po uruchomieniu symulatorze, możesz rozpocząć edytowanie XAML i wyświetlić podgląd pojawiają się na żywo:

    ![](live-view-images/image5.png)  

-----

## <a name="related-links"></a>Linki pokrewne

- [Omówienie odtwarzacza na żywo Xamarin](https://xamarin.com/live)
- [wpis w blogu](https://blog.xamarin.com/live-player/)
- [Przykłady na żywo Player Xamarin](~/tools/live-player/samples.md)
