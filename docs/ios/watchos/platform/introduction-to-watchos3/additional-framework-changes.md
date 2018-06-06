---
title: Zmiany struktury dodatkowe watchOS 3
description: W tym dokumencie opisano różne zmiany framework wprowadzone w systemie watchOS 3 i jak pracować z nimi w programie Xamarin. Omówiono podstawowe dane, Core ruchu Foundation, HealthKit, HomeKit, PassKit i UIKit.
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: af44096928c5e543ac99df3faec9f2e9215f666d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791518"
---
# <a name="additional-watchos-3-frameworks-changes"></a>Zmiany struktury dodatkowe watchOS 3

_W tym artykule omówiono dodatkowych, drobne zmiany lub ulepszenia istniejących struktur dla watchOS 3._

Oprócz najważniejszych zmian w systemach iOS firmy Apple wprowadził zmiany i usprawnienia wielu istniejących struktur w watchOS 3.


## <a name="core-data"></a>Podstawowe dane

Następujące ulepszenia wprowadzono framework Core danych dla czujki OS 3:

- Główny [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) obiektów obsługuje równoczesnych powodujący błąd i pobieranie bez serializacji.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) klasy przechowuje pulę magazynów danych SQLite.
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) obiektów z magazynów danych SQLite w obsługi trybu dziennika odnowy nowej generacji zapytania funkcji, których zarządzanego obiektu kontekstów (MOC) można przypiąć do wersji określonej bazy danych dla przyszłych pobierania i Błąd transakcji.
- Przy użyciu wysokiego poziomu `NSPersistenceContainer` do odwołania `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) i innych danych podstawowych konfiguracji zasobów.
- Dodano kilka nowych metod wygody `NSManagedObject` ułatwianie wykonywania pobrań i utworzyć podklasy.

Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania Framework danych podstawowych](https://developer.apple.com/reference/coredata).


## <a name="core-motion"></a>Podstawowe ruchu

Następujące ulepszenia wprowadzono framework Core ruchu dla czujki OS 3:

- Nowe zdarzenie ruchu urządzenie używa przyspieszeniomierza i Żyroskop do dostarczania aktualizacji ruchu i orientacji. zarejestrować aplikację aplikacji dla tej aktualizacji (stawkami maksymalnie 100Hz).
- Nowe zdarzenie Pedometer umożliwia szybkie, w czasie rzeczywistym powiadomienia po zatrzymaniu i przywraca uruchomiona. Użyj [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer) do rejestrowania zdarzeń pedometer pierwszym planie lub w tle.


## <a name="foundation"></a>Foundation

Następujące ulepszenia wprowadzono struktury Foundation dla czujki 3 systemu operacyjnego:

- Użyj nowych [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) klasie, aby obliczeń Interwał daty i czasu, takie jak czas trwania, porównanie odstępach czasu i testowania dla interwału przecięcia.
- Dodano kilka nowych właściwości do [NSLocal](https://developer.apple.com/reference/foundation/nslocale) klasę, aby uzyskać informacje dotyczące lokalnej i format wyświetlania dostępne.
- Użyj nowych [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) klasy do konwersji między różne jednostki z miary (jednostka miary) lub wykonywania obliczeń na wartościach w różnych UOMs.
- Użyj nowych [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) klasy do formatowania zlokalizowanych pomiarów do wyświetlania dla użytkownika końcowego.
- Użyj nowych [NSUnit](https://developer.apple.com/reference/foundation/nsunit) i [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) klasy reprezentujący określonego UOMs.


## <a name="healthkit"></a>HealthKit

Następujące ulepszenia wprowadzono ramach HealthKit czujki 3 systemu operacyjnego:

- Użyj nowych [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) klasę, aby określić `ActivityType` i `LocationType` z ćwiczeń.
- Nowy [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) i `WheelchairUse` metody [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) klasy zostały dodane do pracy z dla niepełnosprawnych powiązane dane kondycji.
- Dodano nowe klucze metadanych dla typów pogody (takich jak `HKWeatherConditionClear` i `HKWeatherConditionCloudy`) i typy ćwiczeń (takich jak `HKWorkoutActivityTypeFlexibility` i `HKWorkoutActivityTypeWheelchairRunPace`) zostały dodane.


## <a name="homekit"></a>HomeKit

Następujące ulepszenia wprowadzono ramach HomeKit czujki 3 systemu operacyjnego:

- Dodano możliwość wyświetlania i interakcyjnie HomeKit połączenia IP aparaty fotograficzne.
- Dodano kilka nowych usług i właściwości.
- Dodano więcej kontekstu i konfiguracji akcesoriów podstawowe usługi i usług łącza.


## <a name="passkit"></a>PassKit

Następujące ulepszenia wprowadzono ramach PassKit czujki 3 systemu operacyjnego:

- Rozwija framework do obsługi bezpiecznego, w aplikacji płatności na Apple Watch zarówno towarów fizycznych, jak i usług.
- Obecnie dostępne są następujące klasy: [PKPayment](https://developer.apple.com/reference/passkit/pkpayment), [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod), [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) i [PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

Następujące ulepszenia wprowadzono ramach UIKit czujki 3 systemu operacyjnego:

- Do obsługi typu dynamicznego w etykietach, korzystać z nowych pól tekstowych i pól tekstowych `PreferredFontForTextStyle` metody `UIFont` klasy.
- `ColorWithDisplayP3` Metody został dodany do obsługuje szeroką koloru.


## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie (przykład)](https://developer.xamarin.com/samples/monotouch/WatchKit/)
- [What's new in watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
