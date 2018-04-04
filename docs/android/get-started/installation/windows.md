---
title: Instalacja systemu Windows
description: W tym przewodniku opisano kroki dotyczące instalowania platformy Xamarin.Android dla programu Visual Studio w systemie Windows oraz wyjaśniono sposób konfigurowania platformy Xamarin.Android tworzenie pierwszej aplikacji platformy Xamarin.Android.
ms.prod: xamarin
ms.assetid: 2BE4D5AD-D468-B177-8F96-837D084E7DE1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 1cd9a4977aad3f3bd8d8a4e51871698a54f75eb8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="windows-installation"></a>Instalacja systemu Windows

_W tym przewodniku opisano kroki dotyczące instalowania platformy Xamarin.Android dla programu Visual Studio w systemie Windows oraz wyjaśniono sposób konfigurowania platformy Xamarin.Android tworzenie pierwszej aplikacji platformy Xamarin.Android._


## <a name="overview"></a>Omówienie

Ponieważ Xamarin jest teraz dostępna w ramach wszystkich wersji programu Visual Studio bez dodatkowych kosztów i nie wymaga oddzielnej licencji, Instalator programu Visual Studio można użyć, aby pobrać i zainstalować narzędzia platformy Xamarin.Android.
(Ręcznej instalacji i licencji kroki, które były wymagane w przypadku wcześniejszych wersji platformy Xamarin.Android nie są już wymagane.) W tym przewodniku przedstawiono poniżej:

-   Jak skonfigurować niestandardowe lokalizacje Java Development Kit, zestaw SDK systemu Android i zestawu Android NDK.

-   Jak uruchomić narzędzie Android SDK Manager w celu pobierania i instalacji dodatkowe składniki zestawu SDK systemu Android.

-   Jak przygotować urządzenie z systemem Android lub w emulatorze do debugowania i testowania.

-   Jak utworzyć pierwszy projekt aplikacji platformy Xamarin.Android.

Na koniec tego przewodnika, będzie dostępna działająca instalacji platformy Xamarin.Android zintegrowane z programu Visual Studio i będzie można rozpocząć tworzenie pierwszej aplikacji platformy Xamarin.Android.

## <a name="installation"></a>Instalacja

Aby uzyskać szczegółowe informacje na temat instalowania Xamarin do użytku z programem Visual Studio w systemie Windows, temacie [Windows zainstaluj](~/cross-platform/get-started/installation/windows.md) przewodnik.


## <a name="configuration"></a>Konfiguracja

Umożliwia tworzenie aplikacji platformy Xamarin.Android korzysta z Java Development Kit (JDK) i zestaw SDK systemu Android. Podczas instalacji Instalator programu Visual Studio umieszcza je w ich lokalizacje domyślne, a konfiguruje środowisko projektowe o konfiguracji odpowiednią ścieżkę. Można wyświetlać i zmieniać te lokalizacje, klikając **Narzędzia > Opcje > Xamarin > Ustawienia systemu Android**:

![Okno dialogowe Ustawienia zrzut ekranu programu Xamarin Android](windows-images/07-settings.png)

Dla większości użytkowników tych domyślnych lokalizacji będzie działać bez dalszych zmian. Jednak możesz skonfigurować niestandardowe lokalizacje dla tych narzędzi Visual Studio (na przykład po zainstalowaniu Java JDK, zestaw SDK systemu Android lub zestawu NDK w innej lokalizacji). Kliknij przycisk **zmienić** obok ścieżki, która ma zostać zmieniony, następnie przejdź do nowej lokalizacji.

Używa platformy Xamarin.Android [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), który jest wymagany, jeśli są rozwijającym się na poziomie interfejsu API, 24 lub większa (JDK 8 obsługuje również poziomy interfejsu API starszych niż 24). Można nadal używać [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Jeśli tworzenie specjalnie dla interfejsu API na poziomie 23 lub starszym.

> [!IMPORTANT]
> Xamarin.Android nie obsługuje JDK 9.


### <a name="android-sdk-manager"></a>Menedżer zestawów SDK dla systemu Android

Android używa wielu ustawień poziom interfejsu API systemu Android do określania zgodności aplikacji w różnych wersjach systemu android (Aby uzyskać więcej informacji na temat poziomów interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md)).
W zależności od tego, jakie ma być docelowa poziomy interfejsu API systemu Android należy pobrać i zainstalować dodatkowe składniki zestawu SDK systemu Android. Ponadto może być konieczne zainstalowanie opcjonalnych narzędzi i obrazy emulatora w zestawie SDK systemu Android. Aby to zrobić, użyj **Android SDK Manager**. Możesz uruchomić **Android SDK Manager** klikając **Narzędzia > Android > Android SDK Manager**:

[![Jak uruchomić narzędzie Android SDK Manager](windows-images/08-sdk-manager-sml.png)](windows-images/08-sdk-manager.png#lightbox)

Domyślnie program Visual Studio instaluje Google Android SDK Manager:

![Zrzut ekranu przykładzie systemu Google Android SDK Manager](windows-images/09-google-sdk-manager.png)

Google Android SDK Manager służy do instalowania wersje pakietu narzędzi dla systemu Android SDK do wersji 25.2.3. Jednak należy użyć nowszą wersję pakietu Narzędzia zestawu SDK systemu Android, należy zainstalować dodatek Xamarin Android SDK Manager dla programu Visual Studio (dostępne w witrynie Marketplace programu Visual Studio). Jest to konieczne, ponieważ firmy Google autonomiczny zestaw SDK Manager została uznana za przestarzałą w wersji 25.2.3 pakietu Narzędzia zestawu SDK systemu Android. 

Aby uzyskać więcej informacji o korzystaniu z platformy Xamarin Android SDK Manager, zobacz [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md).


### <a name="android-emulator"></a>Emulator systemu android

Jeśli nie masz urządzenia z systemem Android fizycznego na potrzeby testowania można użyć emulatora systemu Android do testowania aplikacji. Aby uzyskać więcej informacji na temat emulator systemu Google Android, zobacz [emulatora Android SDK](~/android/deploy-test/debugging/android-sdk-emulator/index.md).

Emulator systemu Google Android używa firmy Intel HAXM (sprzętu przyspieszony menedżera wykonywania), które mogą powodować konflikt z technologii wirtualizacji, używany przez inne emulatory. Są trzy technologie wirtualizacji główne:

-   **Funkcja Hyper-V** (wykorzystywane przez Visual Studio Emulator dla systemów Android i Windows Phone emulator) 

-   **Pole wirtualnego** (wykorzystywane przez Genymotion)

-   **Intel HAXM** (wykorzystywane przez emulator Google Android SDK) 

Ponieważ Procesora komputera programowanie może obsługiwać tylko jeden technologii wirtualizacji jednocześnie, najlepiej jest tylko jedna używany na komputerze dewelopera.

<a name="device" />

### <a name="android-device"></a>Urządzenia z systemem android

Jeśli masz urządzenie z systemem Android fizyczne na potrzeby testowania, jest odpowiedni moment, aby skonfigurować go do użytku programowanie. Zobacz [ustawić się urządzenia na potrzeby programowania](~/android/get-started/installation/set-up-device-for-development.md) Aby skonfigurować urządzenia z systemem Android do tworzenia aplikacji, następnie podłącz je do komputera służący do uruchamiania i debugowanie aplikacji platformy Xamarin.Android.


## <a name="create-an-application"></a>Tworzenie aplikacji

Teraz, gdy zainstalowano platformy Xamarin.Android, można uruchomić programu Visual Studio Utwórz nowy projekt. Kliknij przycisk **Plik > Nowy > Projekt** aby rozpocząć tworzenie aplikacji:

![Tworzenie nowego projektu](windows-images/10-new-project.png)

W **nowy projekt** okno dialogowe, wybierz opcję **Android** w obszarze **szablony** i kliknij przycisk **pusta aplikacja (Android)** w okienku po prawej stronie. Wprowadź nazwę dla aplikacji (na poniższym zrzucie ekranu, aplikacja jest wywoływana **moja_aplikacja**), następnie kliknij przycisk **OK**:

[![Zrzut ekranu nowego projektu dialogowym Tworzenie pusta aplikacja systemu Android](windows-images/11-first-app-sml.png)](windows-images/11-first-app.png#lightbox)

To już wszystko! Teraz można przystąpić do tworzenia aplikacji systemu Android za pomocą platformy Xamarin.Android!


## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób konfigurowania i instalowania platformy Xamarin.Android w systemie Windows, jak (opcjonalnie) Konfigurowanie programu Visual Studio z niestandardowych Java JDK i zestawu SDK systemu Android lokalizacje instalacji, jak uruchomić SDK Manager, aby zainstalować dodatkowe zestawu SDK systemu Android składniki, sposobu konfigurowania urządzenia z systemem Android lub w emulatorze i jak rozpocząć tworzenie pierwszej aplikacji.

Następnym krokiem jest przyjrzeć [Hello, Android](~/android/get-started/hello-android/index.md) samouczkami, aby dowiedzieć się, jak utworzyć pracy aplikacji platformy Xamarin.Android.


## <a name="related-links"></a>Linki pokrewne

- [Pobierz program Visual Studio](https://www.visualstudio.com/vs/)
- [Instalowanie narzędzi Visual Studio Tools dla platformy Xamarin](~/cross-platform/get-started/installation/windows.md)
- [Wymagania systemowe](~/cross-platform/get-started/requirements.md)
- [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md)
- [Emulator zestawu SDK systemu Android](~/android/get-started/installation/android-emulator/index.md)
- [Konfigurowanie urządzeń środowiska deweloperskiego](~/android/get-started/installation/set-up-device-for-development.md)
