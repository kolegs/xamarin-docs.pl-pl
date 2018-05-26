---
title: Testowanie, optymalizacja i wdrażanie aplikacji platformy Xamarin.Android
description: Ta sekcja zawiera przewodniki, które opisują sposób przetestować aplikację, zoptymalizować jego wydajność, przygotować ją do wersji, podpisz go za pomocą certyfikatu i opublikować go do sklepu z aplikacjami
ms.prod: xamarin
ms.assetid: 568C0B85-EFF3-AF6F-5605-95055193D367
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: ce86590b2d05f3b9141d1a5ba42df9274544f9ae
ms.sourcegitcommit: 8f14a067528067a34521452297080c98b4c1d1af
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/25/2018
---
# <a name="deployment-and-testing"></a>Wdrażanie i testowanie

Ta sekcja zawiera przewodniki, które opisują sposób przetestować aplikację, zoptymalizować jego wydajność, przygotować ją do wersji, podpisz go za pomocą certyfikatu i opublikować go do sklepu z aplikacjami.


##  <a name="application-package-sizesapp-package-sizemd"></a>[Rozmiar pakietu aplikacji](app-package-size.md)

W tym artykule sprawdza elementów składowych pakiet aplikacji platformy Xamarin.Android i skojarzone strategie, których można użyć do wdrożenia pakietu wydajne podczas debugowania i wydania etapy tworzenia.

##  <a name="building-appsbuilding-appsindexmd"></a>[Tworzenie aplikacji](building-apps/index.md)

W tej sekcji opisano sposób procesu kompilacji działa i wyjaśniono sposób zastosowania APKs specyficzne dla interfejsu ABI.

##  <a name="command-line-emulatorcommand-line-emulatormd"></a>[Emulator wiersza polecenia](command-line-emulator.md)

W tym artykule krótko dotyka uruchamianie emulatora przy użyciu wiersza polecenia.

## <a name="debuggingandroiddeploy-testdebuggingindexmd"></a>[Debugowanie](~/android/deploy-test/debugging/index.md)

Przewodniki w sekcji ułatwiają debugowanie aplikacji przy użyciu systemu Android emulatorów, urządzenia Android rzeczywistego i dziennik debugowania.

##  <a name="setting-the-debuggable-attributeandroiddeploy-testdebuggable-attributemd"></a>[Ustawienie atrybutu możliwością debugowania](~/android/deploy-test/debuggable-attribute.md)

W tym artykule wyjaśniono, jak ustawić atrybut debugowalny, tak że narzędzi takich jak `adb` może komunikować się z maszyny wirtualnej Java.

##  <a name="environmentenvironmentmd"></a>[Środowisko](environment.md)

W tym artykule opisano środowiska wykonawczego platformy Xamarin.Android i właściwości systemu Android, wpływające na wykonywanie programów.

##  <a name="gdbgdbmd"></a>[GDB](gdb.md)

W tym artykule opisano sposób użycia `gdb` do debugowania aplikacji platformy Xamarin.Android.

##  <a name="installing-a-system-appinstall-system-appmd"></a>[Instalowanie aplikacji przez System](install-system-app.md)

W tym przewodniku objaśniono sposób instalowania aplikacji platformy Xamarin.Android jako aplikację systemu na urządzeniu z systemem Android lub w ramach niestandardowych pamięci ROM.

##  <a name="linking-on-androidlinkermd"></a>[Łączenie w systemie Android](linker.md)

W tym artykule omówiono proces łączenia używane przez platformy Xamarin.Android, aby zmniejszyć rozmiar końcowy aplikacji. Opisuje różne poziomy połączeń, które mogą być wykonywane i zawiera pewne wskazówki i rozwiązania tego problemu można ograniczyć błędów, które mogą być wynikiem przy użyciu konsolidator.

## <a name="xamarinandroid-performanceandroiddeploy-testperformancemd"></a>[Xamarin.Android Performance](~/android/deploy-test/performance.md)

Istnieje wiele technik zwiększającą wydajność aplikacji skompilowanej za pomocą platformy Xamarin.Android. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywana przez Procesora i ilości pamięci używanej przez aplikację.

## <a name="profiling-android-appsandroiddeploy-testprofilingmd"></a>[Profilowanie aplikacji systemu Android](~/android/deploy-test/profiling.md)

W tym przewodniku objaśniono sposób użycia profiler narzędzi do sprawdzania wydajności i użycie pamięci przez aplikację systemu Android.


## <a name="preparing-an-application-for-releaseandroiddeploy-testrelease-prepindexmd"></a>[Przygotowywanie aplikacji dla wersji](~/android/deploy-test/release-prep/index.md)

Po ukończeniu kodowany i przetestować aplikację, jest potrzebne do przygotowania pakietu do dystrybucji. Pierwszym zadaniem w celu przygotowania tego pakietu jest do skompilowania aplikacji dla wersji głównie pociąga za sobą Ustawianie atrybutów niektórych aplikacji.

## <a name="signing-the-android-application-packageandroiddeploy-testsigningindexmd"></a>[Podpisywanie pakietu aplikacji systemu Android](~/android/deploy-test/signing/index.md)

Dowiedz się, jak utworzyć Android tożsamość podpisywania, Utwórz nowy certyfikat podpisywania dla aplikacji systemu Android i podpisywanie aplikacji przy użyciu certyfikatu podpisywania. Ponadto, w tym temacie wyjaśniono, jak wyeksportować aplikację dla dysku *ad hoc* dystrybucji. Wynikowa APK mogą być ładowane bezpośrednio do urządzenia z systemem Android bez pośrednictwa sklepu z aplikacjami.

## <a name="publishing-an-applicationandroiddeploy-testpublishingindexmd"></a>[Publikowanie aplikacji](~/android/deploy-test/publishing/index.md)

Ta seria artykułów wyjaśniono publicznego dystrybucji aplikacji utworzonych za pomocą platformy Xamarin.Android. Dystrybucji może odbywać się za pośrednictwem kanałów, takich jak wiadomości e-mail, serwera sieci web prywatne, Google Play lub sklepu z aplikacjami firmy Amazon dla systemu Android.
