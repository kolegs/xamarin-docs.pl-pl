---
title: Aktualizowanie aplikacji w tle
ms.topic: article
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f4a18bf8f35d1a6c615c819ea90433d1eb123422
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="updating-an-application-in-the-background"></a>Aktualizowanie aplikacji w tle

Odświeżania w tle jest proces wznawiania pracy aplikacji, która jest wstrzymana lub nie działa i aktualizowanie jej z nową zawartość. dla systemu iOS zawiera trzy opcje odświeżanie zawartości w tle:

1.  *Monitorowanie region* i *znaczne usługi zmiany lokalizacji* -API lokalizacji wyzwalacza tła aktualizacji na podstawie zmian w lokalizacji użytkownika. Te interfejsy API pozwala na własne ryzyko Odśwież zawartość w aplikacjach systemem lokalizacji iOS 6, których nie są dostępne inne opcje.
1.  *Pobieranie (iOS 7 +) w tle* -danych czasowych podejście do odświeżania *niekrytyczne* zawartość, która aktualizuje *często* .
1.  *Powiadomienia zdalne (iOS 7 +)* — aplikacje, które odbierać powiadomienia wypychane można używać powiadomień do wyzwolenia zawartości operacji odświeżania w tle. Ta metoda może posłużyć do zaktualizowania z *ważne, zależne od czasu* zawartość, która aktualizuje *sporadycznie* .


W poniższych częściach omówiono podstawowe informacje dotyczące tych opcji.

## <a name="region-monitoring-and-significant-location-changes"></a>Monitorowanie regionu i lokalizacji znaczących zmian

iOS udostępnia dwa interfejsy API lokalizacji, z backgrounding możliwości:

1.  *Monitorowanie region* jest proces konfigurowania regionów z granic i wznawiania pracy urządzenia, gdy użytkownik wprowadza lub opuszcza obszar. Regiony są cykliczne i może być o różnych rozmiarach. Gdy użytkownik przecina granicę regionu, urządzenie wzbudzania do obsługi zdarzeń, zazwyczaj przez wyzwalania powiadomienie lub przeprowadzony zadania po raz pierwszy. Monitorowanie region wymaga GPS i zwiększa baterii i użycia danych.
1.  *Znaczne usługi zmiany lokalizacji* jest dostępna dla urządzeń z komórkowej radia opcja prostszy, oszczędzania energii. Aplikacja nasłuchuje lokalizacji znaczących zmian otrzyma powiadomienie po wieże komórki zmienia urządzenia. Ta usługa służy do wznawiania aplikacji wstrzymane lub został zakończony i zapewnia możliwość sprawdzenia dla nowej zawartości w tle. Operacje w tle wynosi około 10 sekund, chyba że łączyć się z [zadania w tle](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md) .


Aplikacja nie jest konieczne lokalizacji `UIBackgroundMode` użycia tych lokalizacji interfejsów API. Ponieważ iOS nie śledzi typy zadań, które można uruchomić, gdy urządzenie jest wybudzany za zmiany w lokalizacji użytkownika, interfejsy API stanowią obejście aktualizowania zawartości w tle w systemie iOS 6. *Należy pamiętać, wyzwalania aktualizacje w tle z interfejsami API zestawu na podstawie lokalizacji narysuje na zasoby urządzenia czy może mylić użytkowników, którzy nie zrozumienie, dlaczego aplikacja wymaga dostępu do ich lokalizacji*. Należy zachować ostrożność podczas implementowania monitorowania Region lub znaczących zmian lokalizacji przetwarzania w aplikacjach, które nie są już przy użyciu lokalizacji interfejsów API w tle.

Aplikacje za pomocą lokalizacji monitorowania dla przetwarzania w tle uwidaczniać luka w systemie iOS 6: wymagania aplikacji nie mieści się w kategorii konieczne tła, ma ograniczony backgrounding opcje. Wraz z wprowadzeniem dwóch nowych interfejsów API *pobrać tła* i *zdalnego powiadomienia*, system iOS 7 (lub większą) zapewnia możliwości backgrounding więcej aplikacji. W dwóch następnych sekcjach przedstawiono te nowe interfejsy API.

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>Pobieranie w tle (iOS 7 lub nowszej)

W systemie iOS 6 aplikacji, wprowadzając na pierwszym planie potrzebne czasu potrzebna na załadowanie nowej zawartości krótko przedstawiający użytkowników z zawartością, którą one już widoczne. Pobieranie w tle umożliwia aplikacjom ładowanie nowych danych *przed* użytkownik uruchamia aplikację i zapewnić użytkownikowi z najbardziej aktualne materiały.

Aby zaimplementować pobieranie w tle, należy edytować *Info.plist* i sprawdź **Włącz tryby tła** i **pobieranie w tle** pola wyboru:

 [![](updating-an-application-in-the-background-images/fetch.png "Dokonaj edycji Info.plist i sprawdź pole wyboru Włącz tryby tła i pobieranie w tle")](updating-an-application-in-the-background-images/fetch.png#lightbox)

Następnie w `AppDelegate`, Zastąp `FinishedLaunching` metodę, aby ustawić interwał pobierania minimalnej. W tym przykładzie nie możemy udostępnić systemu operacyjnego określić, jak często pobrać nową zawartość:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

Na koniec wykonaj pobranie przez zastąpienie `PerformFetch` metody w `AppDelegate`i przekazując *obsługi uzupełniania*. Obsługa uzupełniania jest delegata, który przyjmuje `UIBackgroundFetchResult`:

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

Gdy skończymy aktualizowania zawartości, nie możemy udostępnić wiedzieć, wywołując Obsługa uzupełniania o odpowiedni stan systemu operacyjnego. iOS oferują trzy opcje stanu ukończenia obsługi:

1.  `UIBackgroundFetchResult.NewData` -Wywoływany w przypadku nowej zawartości została pobrana, a aplikacja została zaktualizowana.
1.  `UIBackgroundFetchResult.NoData` -Wywoływana, gdy wykonano kroki pobierania dla nowej zawartości, ale nie jest dostępna żadna zawartość.
1.  `UIBackgroundFetchResult.Failed` -Przydatne w przypadku obsługi błędów, jest to podczas pobierania nie może przejść.


Aplikacje przy użyciu pobieranie w tle można wykonywać wywołania można zaktualizować interfejsu użytkownika w tle. Gdy użytkownik otwiera aplikację, interfejs użytkownika zostanie aktualny i wyświetlanie nowej zawartości. Spowoduje to również aktualizację migawki przełącznik aplikacji aplikacji, użytkownik może wyświetlić, gdy aplikacja ma nową zawartość.

> [!IMPORTANT]
> **Uwaga**: raz `PerformFetch` jest wywoływana, aplikacja musi około 30 sekund zaczną działać poza pobierania nową zawartość i Wywołaj bloku obsługi zakończenia. Jeśli to trwa za długo, aplikacja zostanie zakończona. Należy rozważyć użycie tła pobrać z _Usługa transferu w tle_ podczas pobierania multimediów lub innych dużych plików.


### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

W powyższym przykładowym kodzie, nie możemy udostępnić systemu operacyjnego określić, jak często pobrać nową zawartość przez ustawienie interwału pobierania minimalnej `BackgroundFetchIntervalMinimum`. iOS oferują trzy opcje interwału pobierania:

1.  `BackgroundFetchIntervalNever` -Wskaż system nigdy nie pobrać nową zawartość. Umożliwia wyłączanie pobieranie w niektórych sytuacjach, na przykład gdy użytkownik nie jest zalogowany. Jest to wartość domyślna interwału pobierania. 
1.  `BackgroundFetchIntervalMinimum` — Niech system zdecyduj, jak często można pobrać na podstawie wzorców użytkownika, czas pracy baterii, potrzeb innych aplikacji i danych użycia.
1.  `BackgroundFetchIntervalCustom` — Jeśli wiadomo, jak często jest aktualizowana zawartość aplikacji, można określić interwał "uśpienia" po każdej operacji, w którym aplikacja nie będzie mógł pobieranie nową zawartość. Po skonfigurowaniu tego interwału system określi, gdy można pobrać zawartości.


Zarówno `BackgroundFetchIntervalMinimum` i `BackgroundFetchIntervalCustom` polegać na systemie można zaplanować pobrań. Ten interwał jest dynamiczny, dostosowania do potrzeb urządzenia, a także zwyczaje danego użytkownika. Na przykład jeśli jeden użytkownik sprawdza aplikacji codziennie i innym kontroli co godzinę, z systemem iOS zapewni zawartość jest aktualny dla obu użytkowników każdym otwarciu aplikacji.

Pobieranie w tle należy używać dla aplikacji, które często aktualizacji z zawartością niekrytyczne. W przypadku aplikacji o aktualizacje krytyczne można użyć zdalnego powiadomienia. Zdalnego powiadomienia są oparte na pobieranie w tle i udostępniania tej procedury obsługi zakończenia. Firma Microsoft będzie Poznaj zdalnego powiadomienia dalej.

 <a name="remote_notifications" />


## <a name="remote-notifications-ios-7-and-greater"></a>Powiadomienia zdalne (iOS 7 lub nowszej)

Powiadomienia wypychane są JSON komunikatów wysłanych z dostawcą urządzenia poprzez *Apple Push Notification service (APN)*.

W systemie iOS 6 przychodzących powiadomień wypychanych informuje system, aby ostrzec użytkownika, który coś interesującego miało miejsce w aplikacji. Kliknięcie powiadomienia ściąga aplikacji ze stanu wstrzymania lub został zakończony, a aplikacja będzie rozpocząć uaktualnianie zawartości. System iOS 7 (lub większą) rozszerza powiadomień wypychanych zwykłej, zapewniając aplikacji w taki sposób, aby zaktualizować zawartość w tle *przed* powiadomienia użytkownika, dzięki czemu użytkownik może otworzyć aplikację i udostępniana nowej zawartości natychmiast.

Aby zaimplementować zdalnego powiadomienia, należy edytować *Info.plist* i sprawdź **Włącz tryby tła** i **zdalnego powiadomienia** pola wyboru:

 [![](updating-an-application-in-the-background-images/remote.png "Włącz tryby tła i zdalnego powiadomienia ustawioną tryb tła")](updating-an-application-in-the-background-images/remote.png#lightbox)

Następnie należy ustawić `content-available` flagi na powiadomienia wypychane do 1. Umożliwia to aplikacji wiedzieć, aby pobrać nową zawartość przed wyświetleniem alertu:

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

W *AppDelegate*, Zastąp `DidReceiveRemoteNotification` metody sprawdzić ładunek powiadomienia dostępnej zawartości, a następnie wywołać odpowiednie zakończenia bloku obsługi:

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

Powiadomienia zdalnego należy używać rzadkim aktualizacji z zawartością, która ma podstawowe znaczenie dla funkcjonalności aplikacji. Aby uzyskać więcej informacji na powiadomienia zdalnego, zobacz Xamarin [powiadomień wypychanych w systemie iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md) przewodnik.

> [!IMPORTANT]
> **Uwaga**: ponieważ mechanizm aktualizacji w powiadomieniach zdalnego opiera się na pobieranie w tle, aplikacja musi rozpocząć wyłączyć pobieranie nowej zawartości i Wywołaj bloku obsługi uzupełniania w ciągu 30 sekund od otrzymania powiadomienia lub z systemem iOS zostanie zakończyć działanie aplikacji. Należy wziąć pod uwagę parowanie zdalnego powiadomienia z _Usługa transferu w tle_ podczas pobierania multimediów lub innych dużych plików w tle.


### <a name="silent-remote-notifications"></a>Dyskretnej zdalnego powiadomienia

Powiadomienia zdalnego to prosty sposób aplikacji, aktualizacji i rozpocząć wyłączyć pobieranie nowej zawartości, ale istnieją przypadki, w którym nie musimy powiadomić użytkownika, które nastąpiły zmiany. Na przykład jeśli użytkownik flagi synchronizowanie pliku, nie należy powiadomić ich za każdym razem, gdy plik aktualizacji. Synchronizowanie pliku nie jest zdarzeniem zaskakująco ani nie wymaga natychmiastowej uwagi użytkownika. Użytkownicy oczekują tylko plików powinny być zawsze aktualne, gdy zostanie otwarta.

W przypadkach, takich jak poprzednim iOS umożliwia wysyłanie powiadomień wypychanych do wysłania w trybie dyskretnym — to znaczy bez alert. Aby włączyć regularnych powiadomień do jednej dyskretnej, po prostu usunięcie alertu z ładunek powiadomienia:

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>Limity szybkości

Największych różnica między normalne i dyskretnej powiadomienia z punktu widzenia developer to dyskretnej wypchnięć szybkość ograniczone. APNs zostanie opóźnione dostarczania wypchnięć w trybie dyskretnym na urządzeniu, jeśli szybkość wypychania pobiera zbyt duże. To, aby upewnić się, że aplikacje nie opróżnienia zasoby urządzenia z zbyt wiele dyskretnej powiadomienia.

Jednak APN umożliwi dyskretnej powiadomienia "potwierdzeń" obok normalne powiadomienia zdalne lub keep-alive odpowiedzi. Ponieważ regularne powiadomienia nie są ograniczone szybkość, ich mogą zostać wykorzystane do zmuszenia przechowywanych w górę dyskretnej powiadomienia z APNs na urządzeniu, jak pokazano na poniższym diagramie:

 [![](updating-an-application-in-the-background-images/silent.png "Powiadomienia regularnego może służyć do wypychać przechowywanych dyskretnej powiadomienia za pomocą usługi APNs do urządzenia, jak pokazano na tym diagramie")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> **Uwaga**: Apple zachęca deweloperów do wysyłania powiadomień wypychanych w trybie dyskretnym, zawsze, gdy aplikacja wymaga i umożliwiają APN zaplanować ich dostarczania.


W tej sekcji możemy zostały objęte różne opcje odświeżanie zawartości w tle do uruchamiania zadań, które nie pasują do kategorii konieczne tła. Teraz zobaczmy, niektóre z tych interfejsów API w akcji.

 [Następnie: Część 4 - iOS Backgrounding — wskazówki](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
