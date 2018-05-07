---
title: Wyszukiwania z wyróżnionym Core
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 0074cfcdc4a80f66f64d12f8e5c0a5e81ab2b8a1
ms.sourcegitcommit: c9ebf456e1c6924956bedb13f4ea78ff09f7b1a0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/04/2018
---
# <a name="search-with-core-spotlight"></a>Wyszukiwania z wyróżnionym Core

Core Spotlight jest nowa struktura dla systemu iOS 9, która stanowi interfejs API przypominającej bazy danych, aby dodać, edytować lub usunąć łącza do zawartości w aplikacji. Elementy, które zostały dodane przy użyciu Spotlight rdzenie będą dostępne w przez wyszukiwanie Spotlight na urządzeniu z systemem iOS.

Na przykład typy zawartości, które mogą być indeksowane przy użyciu Core Spotlight przyjrzeć się firmy Apple wiadomości, poczta, kalendarz i informacje o aplikacji. Core Spotlight wszystkie one obecnie używane do udostępniania wyników wyszukiwania.

## <a name="creating-an-item"></a>Tworzenie elementu

Poniżej przedstawiono przykład tworzenia elementu i indeksowania przy użyciu Core Spotlight:

```csharp
using CoreSpotlight;
...

// Create attributes to describe an item
var attributes = new CSSearchableItemAttributeSet();
attributes.Title = "App Center Test";
attributes.ContentDescription = "Automatically test your app on 1,000 devices in the cloud.";

// Create item
var item = new CSSearchableItem ("1", "products", attributes);

// Index item
CSSearchableIndex.DefaultSearchableIndex.Index (new CSSearchableItem[]{ item }, (error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Te informacje będą widoczne w wynikach wyszukiwania w następujący:

[![](corespotlight-images/corespotlight01.png "Omówienie wyników wyszukiwanie Spotlight Core")](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>Przywrócenie elementu

Po naciśnięciu na element dodawany do wyników wyszukiwania za pomocą Core Spotlight dla aplikacji, `AppDelegate` metody `ContinueUserActivity` nosi nazwę (Ta metoda służy także do `NSUserActivity`). Na przykład:

```csharp
public override bool ContinueUserActivity (UIApplication application,
   NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchableItem.ActionType.ToString ()) {
            // Display content for searchable item...
        }
        break;
    }

    return true;
}
```

Należy zauważyć, że teraz możemy sprawdzanie występowania działania `ActivityType` z `CSSearchableItem.ActionType`.

## <a name="updating-an-item"></a>Aktualizowanie elementu

Może to być razy, gdy element indeksu utworzyliśmy z wyróżnionym Core muszą zostać zmodyfikowane, takie jak zmiana tytuł lub obraz miniatury jest wymagane. Aby wprowadzić tę zmianę, używamy tej samej metody, która była używana można początkowo utworzyć indeks.
Utwórz nową `CSSearchableItem` używa tego samego Identyfikatora, jak został użyty do utworzenia elementu, a następnie Dołącz nowy `CSSearchableItemAttributeSet` zawierających atrybuty zmodyfikowane:

[![](corespotlight-images/corespotlight02.png "Aktualizowanie elementu — omówienie")](corespotlight-images/corespotlight02.png#lightbox)

Ten element jest zapisane wyszukiwanie indeksu, istniejący element zostaje zaktualizowany przy użyciu nowych informacji.

## <a name="deleting-an-item"></a>Usuwanie elementu

Podstawowe Spotlight zapewnia wiele sposobów, aby usunąć element indeks, gdy nie jest już wymagane.

Po pierwsze można usunąć elementu za pomocą jego identyfikatora, na przykład:

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Następnie można usunąć grupy elementów indeksu według nazwy domeny. Na przykład:

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Na koniec można usunąć wszystkie elementy indeksu z następującym kodem:

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```
## <a name="additional-core-spotlight-features"></a>Podstawowe dodatkowe uwagi funkcje

Core Spotlight zawiera następujące funkcje, które ułatwia to utrzymanie indeks dokładne i aktualne:

- **Partii Obsługa aktualizacji** — Jeśli aplikacja musi utworzyć lub zmodyfikować dużą grupę indeksy w tym samym czasie, całą partię mogą być wysyłane do `Index` metody `CSSearchableIndex` klasy w jednym wywołaniu.
- **Odpowiadanie na zmiany indeksu** — przy użyciu `CSSearchableIndexDelegate` aplikacji mogą odpowiadać na zmiany i powiadomienia z możliwością przeszukiwania indeksu.
- **Stosowanie ochrony danych** — przy użyciu klasy ochrony danych, można zaimplementować zabezpieczeń na elementy dodane do indeksu wyszukiwania, przy użyciu Core Spotlight.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Przewodnik programowania w języku wyszukiwania aplikacji](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
