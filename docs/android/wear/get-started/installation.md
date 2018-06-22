---
title: 'Instalowanie i konfigurowanie nosić onXamarin.Android systemu operacyjnego '
description: W tym artykule przedstawiono kroki instalacji i elementy konfiguracji wymagane w celu przygotowania komputera i urządzeń dla systemu Android nosić programowanie. Na koniec tego artykułu będziesz mieć działającego instalacji platformy Xamarin.Android zużycia zintegrowana z programu Visual Studio dla komputerów Mac i/lub programu Microsoft Visual Studio, a będzie gotowa rozpocząć tworzenie pierwszej aplikacji platformy Xamarin.Android zużycia.
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 14162663c518fdd1324f2b0340592fbae491d112
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436508"
---
# <a name="setup-and-installation"></a>Instalacja i Konfiguracja

_W tym artykule przedstawiono kroki instalacji i elementy konfiguracji wymagane w celu przygotowania komputera i urządzeń dla systemu Android nosić programowanie. Na koniec tego artykułu będziesz mieć działającego instalacji platformy Xamarin.Android zużycia zintegrowana z programu Visual Studio dla komputerów Mac i/lub programu Microsoft Visual Studio, a będzie gotowa rozpocząć tworzenie pierwszej aplikacji platformy Xamarin.Android zużycia._

## <a name="requirements"></a>Wymagania

Do tworzenia aplikacji opartych na platformie Xamarin Android nosić niezbędne jest, następujące elementy:

-   **Visual Studio lub Visual Studio for Mac** &ndash; możesz Jeśli używasz programu Visual Studio, Visual Studio 2015 Professional lub nowszego jest wymagany.

-   **Xamarin.Android** &ndash; Xamarin.Android 4.17 lub nowszej, musi być zainstalowana i skonfigurowana przy użyciu programu Visual Studio lub Visual Studio dla komputerów Mac.

-   **Zestaw SDK systemu android** — zestaw SDK systemu Android (interfejs API 21) 5.0.1 lub nowszym należy zainstalować za pomocą Menedżera zestawu SDK systemu Android.

-   **Java Developer Kit** &ndash; tworzenie aplikacji dla systemu Xamarin Android wymaga [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) tworzenie poziom interfejsu API 24 lub większa (JDK 1.8 obsługuje również poziomy interfejsu API starszych niż 24).

Można nadal używać [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Jeśli tworzenie specjalnie dla interfejsu API na poziomie 23 lub starszym.

> [!IMPORTANT]
> Xamarin.Android nie obsługuje JDK 9.

## <a name="installation"></a>Instalacja

Po zainstalowaniu platformy Xamarin.Android, dzięki czemu możesz przystąpić do tworzenia i testowania aplikacji systemu Android nosić wykonaj następujące czynności: 

1.  Instalowanie wymaganych zestawów Android SDK i narzędzia.
2.  Skonfiguruj urządzenie testowe.
3.  Tworzenie pierwszej aplikacji platformy systemu Android zużycia.

Te kroki są opisane w poniższych sekcjach.


### <a name="install-android-sdk-and-tools"></a>Zainstaluj zestaw Android SDK i narzędzia 

Uruchom **Android SDK Manager**: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Jak uruchomić narzędzie Android SDK Manager w programie Visual Studio](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Jak uruchomić narzędzie Android SDK Manager w programie Visual Studio dla komputerów Mac](installation-images/xs/sdk-menu.png)

-----


Upewnij się, że masz następujące zestaw Android SDK i narzędzia są zainstalowane:

* Android SDK Tools v 24.0.0 lub nowszej, a
* Android 4.4W (API20), lub
* Android 5.0.1 (API21) lub nowszej.

Jeśli nie masz najnowszej zestawu SDK i narzędzia są zainstalowane, Pobierz wymagane narzędzia zestawu SDK *i* bitów interfejsu API (może być konieczne przewiń z bitowego je znaleźć &ndash; wybór interfejsu API są wyświetlane poniżej): 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Zrzut ekranu przedstawiający przykładowy SDK Manager włączenie Android 5.0.1 składników](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Zrzut ekranu przedstawiający przykładowy SDK Manager włączenie Android 4.4 i 5.0.1 składników](installation-images/xs/sdk-select.png)

-----


## <a name="configuration"></a>Konfiguracja

Przed użyciem testowanie aplikacji, należy skonfigurować Android nosić emulatora lub rzeczywistego urządzenia Android zużycia. 


### <a name="android-wear-emulator"></a>Emulatora systemu android zużycie

Przed użyciem emulatora systemu Android nosić, należy skonfigurować przy użyciu systemu Android nosić Android Virtual Device (AVD) **Menedżera emulatora Google**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Jak można uruchomić Menedżera emulatora systemu Android w programie Visual Studio](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Jak można uruchomić Menedżera emulatora systemu Android w programie Visual Studio dla komputerów Mac](installation-images/xs/emulator-menu.png)

-----

Aby uzyskać więcej informacji na temat konfigurowania emulatora systemu Android nosić zobacz [debugowania nosić systemu Android na emulatorze](~/android/wear/deploy-test/debug-on-emulator.md).


### <a name="android-wear-device"></a>Urządzenia z systemem android zużycie

Jeśli masz urządzenie Android nosić, takich jak Android Smartwatch nosić można debugować aplikacji na tym urządzeniu, a nie przy użyciu emulatora. Aby uzyskać informacje dotyczące programowania za pomocą urządzenia zużycia, zobacz [debugowania na urządzeniu nosić](~/android/wear/deploy-test/debug-on-device.md).


## <a name="create-your-first-android-wear-app"></a>Tworzenie pierwszej aplikacji platformy systemu Android zużycie

Postępuj zgodnie z [nosić Witaj,](~/android/wear/get-started/hello-wear.md) instrukcjami, aby utworzyć pierwszą aplikację czujki.


## <a name="packaging-your-app"></a>Pakowanie aplikacji

Aplikacje systemu android zużycia zawsze są dystrybuowane wraz z aplikację telefonów z systemem Android pomocnika. 

Po dodaniu aplikacji systemu Android nosić jako odwołanie do głównej aplikacji systemu Android automatycznie zakłada się, że projekt programu Android nosić i wygeneruje wszystkie niezbędne XML i metadanych dla Ciebie. Ponadto zostanie Sprawdź, czy numery pakietu i wersję odpowiadają, można łatwo wysłać aplikacji do witryny Google Play. 

Aby dowiedzieć się więcej na temat tworzenia pakietu aplikacji zużycia, zobacz [Praca z opakowania](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>Linki pokrewne

- [SkeletonWear (przykład)](https://developer.xamarin.com/samples/SkeletonWear/)
