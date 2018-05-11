---
title: zagadnienia dotyczące platformy 64-32-bitowej
description: Zagadnienia dotyczące korzystających z architektury 32-bitowe i 64-bitowe w aplikacji
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 223da6b490e09b2fa27ab3bbf8fa123b5fa8070c
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="3264-bit-platform-considerations"></a>zagadnienia dotyczące platformy 64-32-bitowej

IOS i macOS w przeszłości ma obsługiwane zarówno wersje 32- i 64-bitowych aplikacji, Apple stopniowo ma przestarzały obsługa 32-bitowego.

Począwszy od systemu iOS 11, aplikacje 32-bitowe nie będą uruchamiane, a [wszystkich przesłanych do sklepu z aplikacjami musi obsługiwać 64-bitowych](https://developer.apple.com/news/?id=06282017b).

Począwszy od stycznia 2018, [nowych aplikacji przesłane do Mac App Store musi obsługiwać 64-bitowych](https://developer.apple.com/news/?id=06282017a), a istniejące aplikacje muszą zostać zaktualizowane przez 2018 czerwca.

Klasycznego interfejsu API na platformie Xamarin (`XamMac.dll` i `monotouch.dll`) obsługuje tylko 32-bitowych aplikacji. Jednak używać nowej aplikacji platformy Xamarin.iOS i Xamarin.Mac [Unified API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` i `Xamarin.Mac`) domyślnie, a tym samym docelowym 32 i 64-bitowy, w razie potrzeby.

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Włączanie 64-bitowych wersjach systemu aplikacji platformy Xamarin.iOS

> [!WARNING]
> W tej sekcji dołączono względów historycznych i ułatwia przenoszenie starszych projektów platformy Xamarin.iOS do interfejsu API Unified i obsługuje 64-bitowych. Wszystkie nowe projekty Xamarin.iOS użyje Unified API i obiekt docelowy 64-bitowych domyślnie.

Dla platformy Xamarin.iOS aplikacji dla urządzeń przenośnych, które zostały przekonwertowane do interfejsu API Unified deweloperzy ręcznie zaktualizować ustawienia kompilacji do docelowego 64-bitowe:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. W **konsoli rozwiązania**, kliknij dwukrotnie plik projektu aplikacji, aby otworzyć **opcje projektu** okna.
2. Wybierz **kompilacji systemu iOS**.
3. Dla telefonu iPhone symulator w **obsługiwane architektury** listy rozwijanej wybierz opcję **x86\_64** lub **i386 + x86\_64**:

   [![Ustawienie obsługiwane architektury x86\_64 lub i386 + x86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. Fizyczne urządzenia, wybierz jedną z dostępnych **ARM64** kombinacje:

   [![Ustawienie architektury obsługiwane na jedną z kombinacji ARM64](Images/Image02.png "ustawienie obsługiwane architektury do jednego z kombinacji ARM64")](Images/Image02-large.png#lightbox)

5. Kliknij przycisk **OK**.
6. Wykonaj czystą kompilację.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt aplikacji i wybierz **właściwości**.
2. Wybierz **kompilacji systemu iOS**.
3. Dla telefonu iPhone symulator, należy ustawić **obsługiwane architektury** albo **x86\_64** lub **i386 + x86\_64**: 

   [![Ustawienie obsługiwane architektury x86_64 lub i386 + x86\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. Fizyczne urządzenia, wybierz jedną z dostępnych **ARM64** kombinacje:
    
   [![Ustawienie architektury obsługiwane na jedną z kombinacji ARM64](Images/VS01.png "ustawienie obsługiwane architektury do jednego z kombinacji ARM64")](Images/VS01-large.png#lightbox)

5. Zapisz zmiany.
6. Wykonaj czystą kompilację.

-----

ARMv7s jest obsługiwana tylko przez procesor A6 objęte iPhone 5 (lub nowszego). ARMv7 kodu jest szybsze i mniejsza niż ARMv6, tylko współpracuje z iPhone 3GS lub nowszym i jest wymagane przez firmę Apple, jeśli tabletów iPad lub iOS minimalna wersja 5.0. ARMv6 działa na wszystkich urządzeniach, ale nie jest już obsługiwana przez kompilator wysłane z Xcode 4.5 lub nowszy. 

ARM64 jest wymagana do obsługi systemu iOS 8 iPhone 6 lub innych urządzeniach 64-bitowych i wymagane przez firmę Apple podczas przesyłania nowych lub aktualizowania zamykanie aplikacji w sklepie iTunes.

Aby przyjrzeć kompleksowe możliwości różnych urządzeń z systemem iOS, zapoznaj się z firmy Apple [zgodność urządzeń](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) dokumentu.

### <a name="64-bit-and-binary-size-increases"></a>zwiększa rozmiar 64-bitowe i binarne

Podczas przejścia firmy Apple z 32-bitowej do 64-bitowy, iOS aplikacje należy uruchomić na sprzęcie zarówno 32-bitowe i 64-bitowych. W związku z tym na platformie Xamarin Unified API umożliwia deweloperom docelowych jednocześnie.

Przeznaczonych dla architektury zarówno 32-bitowe i 64-bitowe znacznie zwiększyć rozmiar aplikacji. Jednak to tak umożliwi nowych urządzeń do uruchomienia zoptymalizowanego kodu przerywania obsługi starszych urządzeń.

> [!IMPORTANT]
> Jeśli zostanie wyświetlony następujący komunikat o błędzie podczas przesyłania aplikacji systemu iOS do iTunes App Store _"ostrzeżenie ITMS-9000: Brak Obsługa 64-bitowego. Uruchamianie nowego iOS 1 lutego 2015 r. aplikacji przekazany do sklepu z aplikacjami musi zawierać Obsługa 64-bitowego i zostać skompilowane z systemem iOS 8 SDK uwzględnione w programie Xcode 6 lub nowszej. Aby włączyć 64-bitowe w projekcie, zaleca się przy użyciu domyślnego Xcode ustawienie architektury"standardowy" kompilacji do kompilacji jedną wartość binarną z kodem zarówno 32-bitowe i 64-bitowych. "_ Musisz przełączyć obsługiwane architektury na jeden z dostępnych **ARM64** kombinacja (jak pokazano powyżej), skompiluj ponownie i Prześlij ponownie.

## <a name="mac"></a>Mac

> [!IMPORTANT]
> Począwszy od stycznia 2018 wszystkich nowych aplikacji Mac przesłane do Mac App Store musi obsługiwać 64-bitowych. Istniejące aplikacje sklepu Mac App Store i ich aktualizacji musi obsługiwać 64-bitowych począwszy od czerwca 2018. Zobacz [announcment firmy Apple](https://developer.apple.com/news/?id=06282017a) i [przewodnik, który opisuje sposób aktualizacji do 64-bitowej aplikacji Xamarin.Mac](~/cross-platform/macios/32-and-64/mac-64-bit.md).

Większość nowoczesnych komputerów Mac obsługuje zarówno 32-bitowe i 64-bitowych aplikacji.   MacOS 10.6 (śniegu Leopard) był ostatni systemu operacyjnego w systemach 32-bitowych.   Większość komputerów Mac wydane od czasu 2010 obsługuje obu systemów.

W przeciwieństwie do systemu iOS są wiele nowych struktur wprowadzone w nowszych wersjach macOS tylko obsługiwane w trybie 64-bitowych (CloudKit, EventKit, GameController, LocalAuthentication, MediaLibrary, MultipeerConnectivity, NotificationCenter, GLKit, SpriteKit, społecznych, i MapKit, między innymi).

Unified API umożliwiają deweloperom wybierz, jakiego rodzaju aplikacji, którego chce utworzyć: 32-bitowy lub 64-bitowej.

**aplikacje 32-bitowe** na komputerach Mac 32-bitowe i 64-bitowych, mieć maksymalnie 32-bitowy przestrzeń adresową, a następnie wymagają, że wszystkie biblioteki są 32-bitowy.

Zwykle będą używać trybu tego, czy masz zależności 32-bitowe, które nie działają w trybie 64-bitowym, jeśli mają mniejsze pobierania, czy istnieją nie zwiększenia wydajności podczas przenoszenia do 64-bitowej.

Ten tryb jest ograniczanie, ponieważ nie można używać wielu platform dostępne w macOS Mavericks i macOS Yosemite.

**aplikacje 64-bitowe** zostanie uruchomiony tylko na urządzeniach 64-bitowych Mac.

Dla komputerów Mac to jest preferowanym trybem operacji w trybie 64-bitowym obsługuje obecnie większość komputerów Mac w użyciu, a użytkownik ma dostęp do pełnego zestawu struktury dostarczonymi przez firmę Apple.

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Włączanie 64-bitowych kompilacje Xamarin.Mac aplikacji

Informacji o tworzeniu aplikacji 64-bitowych, przy użyciu Xamarin.Mac, zobacz [Unified Xamarin.Mac aktualizowanie aplikacji na 64-bit](~/cross-platform/macios/32-and-64/mac-64-bit.md) przewodnik.

## <a name="related-links"></a>Linki pokrewne

- [Klasycznym vs różnice Unified API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
