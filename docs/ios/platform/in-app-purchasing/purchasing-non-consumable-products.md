---
title: "Zakup produktów z systemem innym niż niestandardowe"
ms.topic: article
ms.prod: xamarin
ms.assetid: 635D9CA2-6BCA-53E1-7B10-968029AA3493
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 43aceca2eb86616fd6c9f51eb10c680e8cee9800
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="purchasing-non-consumable-products"></a>Zakup produktów z systemem innym niż niestandardowe

Produkty jednoznacznie składnika są "właścicielem" klienta. Oczekuje się, że będzie zawsze mają do nich dostępu, nawet wtedy, gdy ich urządzenie zostanie utracone kradzieży lub zakupu nowego. Są one przydatne w przypadku książki, magazine problemy, gier, filtry fotografii, "pro funkcji", np. Po zakupieniu użytkownika produktu niestandardowe nie mają nigdy nie opłacać go ponownie. Kod umożliwi przypadkowo spróbuj, StoreKit wyświetli komunikat, który już został kupiony.

## <a name="non-consumable-products-sample"></a>Przykładowe produkty z systemem innym niż niestandardowe

[Kod InAppPurchaseSample](https://developer.xamarin.com/samples/monotouch/StoreKit/) zawiera projekt o nazwie *NonConsumables*. Przykładowy kod przedstawia sposób wdrożenia przy użyciu filtrów fotograficznych jako przykład produkty z systemem innym niż niestandardowe. Po dokonaniu zakupu filtru można użyć jej do zdjęcia wielokrotnie. Nigdy nie należy go ponownie zakupu.   
   
   
   
 Proces zakupu jest wyświetlany w tej serii zrzuty ekranu — **kupić** przycisk staje się przycisk aktywacji funkcji:   
   
   
   
 [![](purchasing-non-consumable-products-images/image34.png "Proces zakupu jest wyświetlany w tej serii zrzuty ekranu")](purchasing-non-consumable-products-images/image34.png#lightbox)   
   
   
   
 Proces zakupu jest taka sama jak eksploatacyjny produktu. Najważniejsza różnica polega na w sposób śledzenia zakupu w kodzie aplikacji. W tym przykładzie przycisk Kup jest tylko dostępne w przypadku produktu nie zostały zakupiła, w przeciwnym razie przycisku uaktywnia funkcję samej siebie.   
   
   
   

Poniższy diagram przedstawia interakcje między klasami i serwerem sklepu z aplikacjami w celu dokonania zakupu produktu z systemem innym niż niestandardowe:   
   
   
   
 [![](purchasing-non-consumable-products-images/image35.png "Interakcje między klasami i serwerem sklepu z aplikacjami w celu dokonania produktu z systemem innym niż niestandardowe zakupu")](purchasing-non-consumable-products-images/image35.png#lightbox)   
   
   
   
 Najważniejsza różnica w przykładzie eksploatacyjny jest, że po zakończeniu zakupu interfejsu użytkownika zostało zaktualizowane do zapobiec ponownie zakupu. W tym przykładzie powiadomienia o pomyślnym transakcji aktualizacje interfejsu użytkownika, aby **kupić** przycisku jest konwertowany na przycisku, który uaktywnia się funkcję.

## <a name="re-purchasing-non-consumable-products"></a>Ponownie zakupu produktów z systemem innym niż niestandardowe

Kod powinien zwykle ukryć lub zmień przeznaczenie przycisk zakupu, gdy produkt został pomyślnie kupiony, aby uniemożliwić użytkownikowi próby ponownie zakupu produktu. Przykładowa aplikacja robi to poprzez zmianę **kupić** przycisku do przycisku, który sprawia, że Filtr fotograficzny przykład pracy.   
   
   
   
 Istnieją sytuacje, gdy aplikacja nie wiadomo, czy produktu z systemem innym niż niestandardowe już została zakupiona:

-  Jeśli aplikacja zostanie usunięta i ponownie zainstalowane na urządzeniu, wszystkie rekordy zakupu zostanie usunięty (chyba że / dopóki użytkownik wykona przywracania kopii zapasowej). 
-  Jeśli użytkownik ma aplikacji zainstalowanych na urządzeniach (co najmniej dwa) i dokonuje zakupu w jednym z urządzeń. Inne urządzenia będą nadal Pokaż można kupić produktu. 
-  Jeśli klient próbuje ponownie kupić produktu z systemem innym niż niestandardowe w takich sytuacjach, w sklepie App Store spełniają produktu ponownie bez dodatkowych opłat. Interfejs użytkownika będzie wyświetlana przeprowadzić zakupu (na przykład, zostanie wyświetlony alert potwierdzenie i identyfikator Apple ID będzie wymagane) jednak użytkownik następnie zostanie wyświetlony komunikat o udzielanie porad ich, że produkt został już zakupiony.  
   
   
   
 Ścieżka kodu, w tym scenariuszu jest dokładnie taka sama regularne zakupu, są tylko różnice:

-  Użytkownik nie zostały naliczone opłaty ponownie dla produktu.
-  `SKPaymentTransaction` Obiekt przekazany do aplikacji będzie mieć `OriginalTransaction` właściwość, która odwołuje się do transakcji, który został wygenerowany, gdy początkowo zostało zakupione w ramach produktu. 
-  Aplikacje, które sprzedawać produkty jednoznacznie składnika musi implementować też w StoreKit **przywrócić** funkcji, aby ułatwić użytkownikom pobrać istniejących zakupów. 
