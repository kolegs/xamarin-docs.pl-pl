---
title: Instalacja systemu Windows
description: W tym przewodniku opisano kroki dotyczące instalowania platformy Xamarin.Android dla programu Visual Studio w systemie Windows oraz wyjaśniono sposób konfigurowania platformy Xamarin.Android tworzenie pierwszej aplikacji platformy Xamarin.Android.
ms.prod: xamarin
ms.assetid: 2BE4D5AD-D468-B177-8F96-837D084E7DE1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 1eb8d4ec9ad60f0f9e81676920df4d950a875088
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066445"
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

[Emulatora systemu Android](https://developer.android.com/studio/run/emulator) mogą być przydatne narzędzie do projektowania i testowania aplikacji platformy Xamarin.Android. Na przykład urządzenie fizyczne, takie jak tablet nie może być łatwo dostępne podczas tworzenia lub deweloper może być konieczne uruchomienie niektórych testów integracji na komputerach przed wykonaniem kodu.

Emulowanie urządzenia z systemem Android na komputerze obejmuje następujące składniki:

* **Emulator systemu Google Android** &ndash; jest to emulator na podstawie [QEMU](https://www.qemu.org/) tworzącą zwirtualizowanych urządzeniu z systemem na stacji roboczej dewelopera.
* **Obraz emulatora** &ndash; _obraz emulatora_ jest szablon lub specyfikacji sprzętu i systemu operacyjnego, który ma zostać Zwirtualizowana. Na przykład jeden obraz emulatora będzie zidentyfikować wymagania sprzętowe dla węzła 5 X systemem Android 7.0 z usług Google Play zainstalowane. Inny obraz emulatora może określonej tabeli 10" systemem Android 6.0.
* **Android Virtual Device (AVD)** &ndash; _Android urządzenia wirtualnego_ jest emulowane urządzenia z systemem Android utworzone na podstawie obrazu emulatora. Podczas uruchamiania i testowania aplikacji systemu Android, Xamarin.Android będzie uruchomić emulatora systemu Android, uruchamianie określonych AVD, zainstaluj plik APK, a następnie uruchom aplikację.

Do znacznej poprawy wydajności w przypadku komputerów opartych na tworzenie na x86 można osiągnąć za pomocą obrazów emulatora specjalne, które są zoptymalizowane dla x86 architektury i jeden z dwóch technologii wirtualizacji:

1. Firmy Microsoft Hyper-V &ndash; dostępne na komputerach z systemem Windows 10 kwietnia Update.
2. Firmy Intel sprzętu przyspieszyć wykonywanie Manager (HAXM) &ndash; dostępne na x86 komputery z systemem OS X, macOS lub starszej wersji systemu Windows.

Aby uzyskać więcej informacji o emulatorze systemu Android, Hyper-V i HAXM, zobacz [przyspieszanie sprzętowe wydajności emulatora](~/android/get-started/installation/android-emulator/hardware-acceleration.md) przewodnik.

> [!NOTE]
> W starszych wersjach systemu Windows HAXM nie jest zgodny z funkcją Hyper-V. W tym scenariuszu należy albo [wyłączenie funkcji Hyper-V](~/android/get-started/installation/android-emulator/troubleshooting.md#disable-hyperv) lub użycie wolniejszych obrazy emulatora, które nie mają x86 optymalizacji.


<a name="device" />

### <a name="android-device"></a>Urządzenia z systemem android

Jeśli masz urządzenie z systemem Android fizyczne na potrzeby testowania, jest odpowiedni moment, aby skonfigurować go do użytku programowanie. Zobacz [ustawić się urządzenia na potrzeby programowania](~/android/get-started/installation/set-up-device-for-development.md) Aby skonfigurować urządzenia z systemem Android do tworzenia aplikacji, następnie podłącz je do komputera służący do uruchamiania i debugowanie aplikacji platformy Xamarin.Android.


## <a name="create-an-application"></a>Tworzenie aplikacji

Teraz, gdy zainstalowano platformy Xamarin.Android, można uruchomić programu Visual Studio Utwórz nowy projekt. Kliknij przycisk **Plik > Nowy > Projekt** aby rozpocząć tworzenie aplikacji:

![Tworzenie nowego projektu](windows-images/10-new-project.png)

W **nowy projekt** okno dialogowe, wybierz opcję **Android** w obszarze **szablony** i kliknij przycisk **aplikacji systemu Android** w okienku po prawej stronie. Wprowadź nazwę dla aplikacji (na poniższym zrzucie ekranu, aplikacja jest wywoływana **moja_aplikacja**), następnie kliknij przycisk **OK**:

[![Zrzut ekranu nowego projektu dialogowym Tworzenie pusta aplikacja systemu Android](windows-images/11-first-app-sml.w157.png)](windows-images/11-first-app.w157.png#lightbox)

To już wszystko! Teraz można przystąpić do tworzenia aplikacji systemu Android za pomocą platformy Xamarin.Android!


## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób konfigurowania i instalowania platformy Xamarin.Android w systemie Windows, jak (opcjonalnie) Konfigurowanie programu Visual Studio z niestandardowych Java JDK i zestawu SDK systemu Android lokalizacje instalacji, jak uruchomić SDK Manager, aby zainstalować dodatkowe zestawu SDK systemu Android składniki, sposobu konfigurowania urządzenia z systemem Android lub w emulatorze i jak rozpocząć tworzenie pierwszej aplikacji.

Następnym krokiem jest przyjrzeć [Hello, Android](~/android/get-started/hello-android/index.md) samouczkami, aby dowiedzieć się, jak utworzyć pracy aplikacji platformy Xamarin.Android.


## <a name="related-links"></a>Linki pokrewne

- [Pobierz program Visual Studio](https://visualstudio.microsoft.com/vs/)
- [Instalowanie narzędzi Visual Studio Tools dla platformy Xamarin](~/cross-platform/get-started/installation/windows.md)
- [Wymagania systemowe](~/cross-platform/get-started/requirements.md)
- [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md)
- [Konfiguracja emulatora systemu Android](~/android/get-started/installation/android-emulator/index.md)
- [Konfigurowanie urządzeń środowiska deweloperskiego](~/android/get-started/installation/set-up-device-for-development.md)
- [Uruchamianie aplikacji na emulatorze systemu Android](https://developer.android.com/studio/run/emulator#Requirements)
