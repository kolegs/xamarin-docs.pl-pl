---
title: "Wdrażanie i testowanie"
description: "Stabilizacji i wskazówki dotyczące wdrażania"
ms.topic: article
ms.prod: xamarin
ms.assetid: 92B35AB1-7AB7-3D3B-DB31-CC971E0B43AE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 0c152c389a6aa62512882863cd2830b436587475
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="deployment-and-testing"></a>Wdrażanie i testowanie

W tej sekcji omówiono tematy wykorzystywane do testowania aplikacji oraz do rozpowszechniania. Tematy w tym miejscu zawierają elementów, takich jak narzędzia używane do debugowania, wdrażania, testerów i sposób publikowania aplikacji do sklepu z aplikacjami.


##  <a name="app-distributioniosdeploy-testapp-distributionindexmd"></a>[Dystrybucja aplikacji](~/ios/deploy-test/app-distribution/index.md)

W tym artykule przedstawiono sposób konfigurowania, tworzenie i publikowanie aplikacji platformy Xamarin.iOS dystrybucji za pośrednictwem różnych różne sposoby, w tym:

- [Dystrybucji sklepu z aplikacjami](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Dystrybucji wewnętrznych (Enterprise)](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Ad Hoc dystrybucji](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)

##  <a name="ipa-deploymentiosdeploy-testapp-distributionipa-supportmd"></a>[Wdrożenie IPA](~/ios/deploy-test/app-distribution/ipa-support.md)

Ad Hoc i wdrożeń w przedsiębiorstwie umożliwiają deweloperom tworzenie pakietów, które mogą być dystrybuowane do testowania lub użytkownikom wewnętrzne firmy. Tym dokumencie opisano sposób tworzenia IPA, które można synchronizować z urządzeniem z systemem iOS za pomocą programu iTunes.

## <a name="provisioningprovisioningindexmd"></a>[Inicjowanie obsługi administracyjnej](provisioning/index.md)

Ten zestaw przewodniki obejmuje podpisywania kodu i inicjowania obsługi administracyjnej essentials, takich jak praca z listy właściwości oraz sposobu udostępniania aplikacji dla usług aplikacji. 

## <a name="wireless-deploymentwireless-deploymentmd"></a>[Wdrażanie bezprzewodowej](wireless-deployment.md)

 Xcode 9 wprowadzono możliwość wdrażania na urządzeniu z systemem iOS lub Apple TV za pośrednictwem sieci, zamiast do sprzętowych urządzeń za każdym razem, gdy chcesz wdrożyć, a debugowanie aplikacji. Ta funkcja jest obecnie w przeglądzie.

##  <a name="testflightiosdeploy-testtestflightmd"></a>[TestFlight](~/ios/deploy-test/testflight.md)

TestFlight teraz jest własnością firmy Apple, a jest to podstawowy sposób do wersji beta testowania aplikacji platformy Xamarin.iOS. Ten artykuł przeprowadzi Cię przez wszystkie kroki procesu TestFlight — z przekazywania aplikacji, do pracy z iTunes Connect.

##  <a name="debugging-in-xamariniosiosdeploy-testdebugging-in-xamarin-iosmd"></a>[Debugowanie w Xamarin.iOS](~/ios/deploy-test/debugging-in-xamarin-ios.md)

Visual Studio i programu Visual Studio for Mac IDEs obejmuje obsługę debugowania aplikacji platformy Xamarin.iOS zarówno w symulatorze systemu iOS, jak i na urządzeniach z systemem iOS. W tym artykule przedstawiono, jak używać debugera, jak również konfigurowanie różnych opcji, który go obsługuje.


##  <a name="touchunitiosdeploy-testtouchunitmd"></a>[Touch.Unit](~/ios/deploy-test/touch.unit.md)

Ten dokument zawiera opis sposobu tworzenia testów jednostkowych dla projektów platformy Xamarin.iOS.
Testowanie jednostkowe na platformie Xamarin.iOS odbywa się przy użyciu platformy Touch.Unit, która obejmuje zarówno dla systemu iOS test runner, a także zmodyfikowanej wersji [NUnitLite](http://www.nunitlite.com/) platforma, która udostępnia zestaw znanych interfejsów API do pisania testów jednostkowych.



##  <a name="using-instruments-to-detect-native-leaks-using-markheapiosdeploy-testusing-instruments-to-detect-native-leaks-using-markheapmd"></a>[Za pomocą dokumentów, aby wykryć przecieki macierzystym, przy użyciu MarkHeap](~/ios/deploy-test/using-instruments-to-detect-native-leaks-using-markheap.md)

W tym artykule opisano sposób korzystania z dokumentów na dowolnym urządzeniu z systemem iOS i dowolnej aplikacji platformy Xamarin.iOS. Analizuje także jak do profilu aplikacji w symulatorze.



##  <a name="walkthrough---using-apples-instrument-tooliosdeploy-testwalkthrough-apples-instrumentmd"></a>[Wskazówki — za pomocą narzędzia Instrumentacji firmy Apple](~/ios/deploy-test/walkthrough-apples-instrument.md)

W tym artykule przedstawiono sposób użycia narzędzia dokumentów firmy Apple do diagnozowania problemów pamięci w aplikacji systemu iOS utworzonej za pomocą platformy Xamarin. Go pokazano, jak uruchomić dokumentów, wykonać migawki sterty i analizować wzrostu pamięci. Widoczny jest również sposób używać dokumentów do wyświetlania i wskazanie dokładne wierszy kodu, które przyczyną problemu pamięci.

##  <a name="linking-on-ioslinkermd"></a>[Łączenie w systemie iOS](linker.md)

Wyjaśniono, jak konsolidator pracuje nad najmniejszą pakiet aplikacji to możliwe, oraz upewnij się, aby zmodyfikować jego ustawienia i użycia.

##  <a name="xamarinios-performanceperformancemd"></a>[Xamarin.iOS Performance](performance.md)

Istnieje wiele technik zwiększającą wydajność aplikacji skompilowanej za pomocą platformy Xamarin.iOS. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywana przez Procesora i ilości pamięci używanej przez aplikację.

##  <a name="mtouchmtouchmd"></a>[mtouch](mtouch.md)

Uwagi i informacje o mtouch.exe, narzędzia wiersza polecenia, która tworzy projektu do użytku aplikacji przez system iOS.

## <a name="ios-build-mechanicsios-build-mechanicsmd"></a>[Mechanika kompilacji systemu iOS](ios-build-mechanics.md)

Ten przewodnik opisuje sposób czasu aplikacji i sposób użycia metod, które można zastosować dla przyspieszają kompilacje dla wszystkich konfiguracji kompilacji.