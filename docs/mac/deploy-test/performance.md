---
title: Xamarin.Mac wydajności
description: Ten dokument zawiera pewne zagadnienia dotyczące wydajności dla aplikacji Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 54B07DED-FDF2-49B2-A5FB-3A9357E65922
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 273fb1980695a3dcd4a4fc2123b1ebafef1756a2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinmac-performance"></a>Xamarin.Mac wydajności

## <a name="overview"></a>Omówienie

Aplikacje Xamarin.Mac są podobne do platformy Xamarin.iOS i wiele z tym samym sugestie dotyczące wydajności są stosowane:

- [Wydajności platformy Xamarin.iOS](~/ios/deploy-test/performance.md)
- [Wydajność i platform](~/cross-platform/deploy-test/memory-perf-best-practices.md)

ale istnieje szereg macOS określonych sugestii, które mogą być pomocne.

## <a name="prefer-modern-target-framework"></a>Preferowane jest platformy docelowej modern

Dostępnych jest wiele [docelowych platform](~/mac/platform/target-framework.md) dostępne dla aplikacji Xamarin.Mac różne charakterystyki i funkcje.

Jeśli to możliwe, preferowane Nowoczesny i pracować z zależnych bibliotek, aby dodać obsługę. Tylko nowoczesnych platformy docelowej umożliwia łączenie, co może zmniejszyć znaczne rozmiary zestawu. Staje się on szczególnie ważne podczas włączania drzewa obiektów aplikacji, zgodnie z drzewa obiektów aplikacji kompilacji pełne zestawów może spowodować duże pakiety końcowego.

## <a name="enable-the-linker"></a>Włącz konsolidator

Czas uruchamiania, obciążenia i "Tylko w czasie" (JIT), skaluje nieco liniowo z rozmiarem użytkownika końcowego plików binarnych. Najprostszym sposobem, aby poprawić ten jest usuwając martwy kod z [konsolidatora](~/mac/deploy-test/linker.md).

Sugestia ta dotyczy przede wszystkim użytkownikom nowoczesnych platformy docelowej, użycie [łączenie platformy](~/mac/deploy-test/linker.md) zapewniają z nagłego ograniczoną wydajność.

## <a name="enable-aot-when-appropriate"></a>Włącz drzewa obiektów w aplikacji, gdy jest to odpowiednie

Inny aspekt wydajności uruchamiania jest kompilacji JIT zestawów do kodu maszyny. Wyprzedzeniem o czasie (drzewa obiektów aplikacji) kompilacji można znacznie skrócić czas uruchamiania, ale jest dostarczany z liczbą wady i zalety omówione w [dokumentacji drzewa obiektów aplikacji](~/mac/internals/aot.md).

## <a name="ensure-performant-delegates"></a>Sprawdź wydajność delegatów

Wiele aplikacji Xamarin.Mac są skupia widoków Cocoa takich jak `NSCollectionView`, `NSOutlineView`, lub `NSTableView`. Często te widoki są obsługiwane przez `Delegate` i `DataSource` klasy osobie Cocoa, udzielenie odpowiedzi na pytania dotyczące danych, które można wyświetlić.

Wiele z tych punktów wejścia są wywoływane często, a czasami wiele razy na sekundę podczas przewijania.

Upewnij się, że te funkcje zwracają wartości, które mają być obliczane łatwo lub informacje już w pamięci podręcznej, aby zapobiec blokuje interfejsu użytkownika.

## <a name="use-cocoa-provided-apis-for-reusing-views"></a>Przy użyciu interfejsów API dostarczonych Cocoa ponownego korzystania z widoków

Wiele widoków Cocoa, zawierające wiele widoków podrzędnych lub komórki (takie jak `NSCollectionView`, `NSOutlineView`, i `NSTableView`) podaj interfejsy API umożliwiające tworzenie i ponowne użycie widoków. Te tworzenia pul elementów udostępnionego i zapobiec problemom z wydajnością, gdy szybkie przewijanie widoków.

## <a name="use-async-and-do-not-block-the-ui"></a>Użyj asynchronicznych i nie blokują interfejsu użytkownika

Aplikacje klasyczne często przetwarzania dużych ilości danych i jest bardzo proste zablokować oczekiwanie na Operacja synchroniczna wątku interfejsu użytkownika.

Jeśli to możliwe, użyj [async](~/cross-platform/platform/async.md) i wątków, aby zapobiec blokuje interfejsu użytkownika.

Do uruchomienia dużo operacji, należy rozważyć użycie [NSProgressIndicator](https://developer.xamarin.com/samples/mac/ProgressBarExample/) lub inne opcje Apple zanotowane w [HIG](https://developer.apple.com/macos/human-interface-guidelines/indicators/progress-indicators/) do powiadamiania użytkowników.


## <a name="related-links"></a>Linki pokrewne

- [Wydajność i platform](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Wydajności platformy Xamarin.iOS](~/ios/deploy-test/performance.md)
