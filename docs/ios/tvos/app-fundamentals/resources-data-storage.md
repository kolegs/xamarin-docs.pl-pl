---
title: Magazyn danych i zasoby
description: "W tym artykule omówiono pracy z zasobami i magazynu danych w aplikacji Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: e72c013516de5bcf97e2e9f58a7a4f5cd87c730b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="resources-and-data-storage"></a>Magazyn danych i zasoby

_W tym artykule omówiono pracy z zasobami i magazynu danych w aplikacji Xamarin.tvOS._

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>systemu tvOS ograniczenia zasobów

W odróżnieniu od urządzeń z systemem iOS nowe Apple TV zapewnia bardzo ograniczonym trwałego magazynu lokalnego dla aplikacji systemu tvOS ani danych. Bardzo małe elementów (takie jak preferencje użytkownika), aplikację systemu tvOS nadal ma dostęp do `NSUserDefaults` z [limit 500 KB danych](https://forums.developer.apple.com/message/50696#50696). Jednak jeśli Xamarin.tvOS aplikacji musi być zachowany większej ilości informacji, musi przechowywanie i pobieranie danych z [iCloud](#iCloud-Data-Storage).

Ponadto systemu tvOS ogranicza rozmiar aplikacji Apple TV, 200 MB. Jeśli aplikacja wymaga zasobów poza tym rozmiar, muszą być spakowane i ładowane przy użyciu [zasobów na żądanie](#On-Demand-Resources) (maksymalnie dodatkowo 2 GB). Biorąc pod uwagę następujące ograniczenia, jest krytyczny, że poprawnie czas pobierania dodatkowe zasoby, aby zapewnić najlepsze środowisko dla użytkowników aplikacji. Aby uzyskać więcej informacji, zobacz firmy Apple [przewodnik zasobów na żądanie](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>Trwałe pliki do pobrania

Każda aplikacja systemu tvOS podano katalogu tymczasowego pamięci podręcznej, w którym jej dodatkowych zasobów i zasoby są pobierane do. Ten katalog zostaną zachowane, jak aplikacja nadal działa. Jednak jak musi zwolnić miejsce dla innych aplikacji lub obciążenie systemu Apple TV, można usunąć tej pamięci podręcznej.

W związku z tym Twoja aplikacja nie bazuje na wcześniej pobranej zawartości, są dostępne przy następnym uruchomieniu go. Xamarin.tvOS aplikacji należy zawsze Sprawdzaj istnienie wymaganych zasobów i pobrać je zgodnie z potrzebami.

> [!IMPORTANT]
> Gdy masz możliwość pobrania innych zasobów i zasobów, zgodnie z wymaganiami firmy Apple ostrzega przed wykorzystywanie całego miejsca w pamięci podręcznej aplikacji, ponieważ może spowodować nieprzewidywalne skutki.




<a name="Managing-Resources" />

## <a name="managing-resources"></a>Zarządzanie zasobami

Jak już wspomniano, ze względu na ograniczone, trwałe magazynu informacji dostępnych dla aplikacji systemu tvOS, ograniczenia te wymagają, należy dokładnie zaplanować utworzyć środowisko użytkowników dla aplikacji Xamarin.tvOS.

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>Magazyn danych usługi iCloud

Ponieważ magazyn na Apple TV jest ograniczona, nie tylko słowo istnieje bardzo ograniczony magazynu trwałego, lokalne, aplikacja ma żadnej gwarancji, że wszystkie informacje, które wcześniej pobrano będą dostępne przy następnym uruchomieniu.

W związku z tym aplikacji Xamarin.tvOS musi przechowywać wszystkie dane użytkownika w magazynie danych w usłudze iCloud. Apple są dostępne dwie opcje przechowywania danych w ramach usługi iCloud dla aplikacji systemu tvOS:

- **iCloud klucz-wartość (magazynie, wartości KLUCZY magazynu)** — mała sztuk danych (mniej niż 1 MB), czy aplikacji może wymagać (takie jak preferencje użytkownika), można użyć magazynie wartości KLUCZY magazynu usługi iCloud. iCloud magazynie wartości KLUCZY danych jest automatycznie synchronizowane z chmury i wszystkich urządzeń użytkownika z tej samej aplikacji. Aby uzyskać więcej informacji zobacz [magazynu kluczy i wartości](~/ios/data-cloud/introduction-to-icloud.md) sekcji naszych [wprowadzenie do usługi iCloud](~/ios/data-cloud/introduction-to-icloud.md) dokumentu lub firmy Apple [projektowanie pod kątem danych klucz-wartość w ramach usługi iCloud](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7) dokumentacja.
- **CloudKit** — w przypadku magazynu większy rodzajów informacji (więcej niż 1 MB), za pomocą architektury CloudKit firmy Apple. W przeciwieństwie do usługi iCloud magazynu magazynie wartości KLUCZY CloudKit danych może być udostępniana między wszyscy użytkownicy aplikacji (a także są prywatne do jednego użytkownika). Tworzą więcej informacji, zobacz nasze [wprowadzenie do CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) dokumentacji lub firmy Apple [CloudKit Szybki Start](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987).

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>Zasoby na żądanie

Zasoby na żądanie zawierają zawartości aplikacji i zasobów (niezależnie od pakietu aplikacji), które hostowanej ze sklepu App Store i pobrać wymagane przez aplikację. Maksymalnie dodatkowo 2 GB danych mogą być przekazywane przy użyciu zasobów na żądanie. Umożliwiają one pobierania początkową aplikacji może być mniejszy (aplikacje systemu tvOS są ograniczone do maksymalnie 200 MB), zapewniając sformatowanego zasobów zgodnie z potrzebami.

Gdy aplikacja systemu tvOS żąda zasobów na żądanie, system automatycznie zarządza pobierania i przechowywania tej zawartości do katalogu pamięci podręcznej aplikacji. Aplikacja musi czekać dla tej zawartości do pobrania i udostępnione, aby kontynuować.

Te zasoby mogą nadal być buforowane na Apple TV w całym przeprowadzający wielu aplikacji, w związku z tym prędkości uruchomienia cyklu. Aplikacji nie można jednak używać wcześniej pobranej zawartości są dostępne przy następnym uruchomieniu go. Zobacz [pobiera inne niż stałe](#Non-Persistent-Downloads) sekcji powyżej, aby uzyskać więcej informacji.

Xcode umożliwia tworzenie pakiety skojarzone z podać Tag zasobu powiązanej zawartości (np. wszystkie zasoby na poziom 2). Później aplikacja będzie żądać zasobów na żądanie, określając ten Tag zasobu. Aplikacji powinni przedstawić interfejsu użytkownika dla użytkownika, podając tej zawartości jest pobierany. Aby uzyskać więcej informacji, zobacz firmy Apple [przewodnik zasobów na żądanie](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

> [!IMPORTANT]
> Należy zachować ostrożność, uzyskanie kompromisu między ile razy aplikacja ma pobrać zasobów na żądanie i rozmiaru poszczególnych plików do pobrania. Użytkownik może stać się sfrustrowani z aplikacją, gra jest stale przerwania można pobrać nowej zawartości lub jeśli pojedynczy pobierania ma zbyt dużo czasu.




<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego rozmiar, zasobów i danych magazynu ograniczenia nakładane na aplikacji Xamarin.tvOS przez system systemu tvOS. Ma on przedstawiony opcje obejść te ograniczenia i sugestii tworzenia niezwykłych użytkowników dla aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
