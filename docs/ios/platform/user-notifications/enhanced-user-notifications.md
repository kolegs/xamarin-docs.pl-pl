---
title: Powiadomienia użytkowników
description: W tym artykule opisano wszystkich sposobów powiadomienia użytkowników zostały rozszerzone iOS 10 i sposób ich użycia w aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 9fd3ff17dc9af3fd30a7d5b31e8cea7ff8669a51
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="enhanced-user-notifications"></a>Powiadomienia użytkowników

_W tym artykule opisano wszystkich sposobów powiadomienia użytkowników zostały rozszerzone iOS 10 i sposób ich użycia w aplikacji platformy Xamarin.iOS._

Jesteś nowym użytkownikiem iOS 10, powiadomienie użytkownika pozwala dostarczania i obsługi powiadomień lokalnych i zdalnych. Przy użyciu tego framework, aplikacji lub rozszerzenia aplikacji można zaplanować dostarczania lokalnego powiadomienia, określając zestaw warunków, takich jak lokalizacji lub godzinę.

## <a name="about-user-notifications"></a>Powiadomienia użytkowników — informacje

Jak już wspomniano, nowej struktury powiadomienie użytkownika umożliwia dostarczanie i obsługi powiadomień lokalnych i zdalnych. Przy użyciu tego framework, aplikacji lub rozszerzenia aplikacji można zaplanować dostarczania lokalnego powiadomienia, określając zestaw warunków, takich jak lokalizacji lub godzinę.

Ponadto, aplikacji lub rozszerzenia można odbierać (i potencjalnie modyfikować) powiadomień lokalnych i zdalnych jako zostaną dostarczone do urządzeń z systemem iOS przez użytkownika.

Nowa struktura interfejsu użytkownika powiadomienie użytkownika umożliwia aplikacji lub rozszerzenia aplikacji do dostosowywania wyglądu powiadomień lokalnych i zdalnych, kiedy mają być przedstawiane dla użytkownika.

Ta struktura zapewnia następujące sposoby, że aplikację można dostarczać powiadomienia dla użytkownika:

- **Alerty Visual** — gdy przedstawia powiadomienia w dół od górnej krawędzi ekranu w formie transparentu.
- **Dźwięk i drgań** — może być skojarzony z powiadomienie.
- **Badging ikona aplikacji** — gdy ikona aplikacji wyświetla wskaźnika wskazująca, że nowa zawartość jest dostępna, takie jak liczba nieprzeczytanych wiadomości.

Ponadto w zależności od bieżącego kontekstu użytkownika, istnieją różne sposoby, które zostanie wyświetlone powiadomienie:

- Jeśli urządzenie zostanie odblokowane, powiadomienia będą wycofać z górnej części ekranu jako transparent.
- Jeśli urządzenie jest zablokowane, powiadomienia będą wyświetlane na ekranie blokady użytkownika.
- Jeśli użytkownik ma pominięte powiadomienie, mogą otworzyć Centrum powiadomień i wyświetlić wszystkie dostępne, powiadomienia oczekiwania.

Aplikacji platformy Xamarin.iOS ma dwa typy powiadomień użytkownika, który jest w stanie wysyłać:

- **Lokalnego powiadomienia** — te są wysyłane przez aplikacje są instalowane lokalnie na urządzeniach użytkowników.
- **Powiadomienia zdalnego** -są wysyłane z zdalnej one wyzwalacza update tła zawartości aplikacji lub serwera i albo widoczne dla użytkownika.

### <a name="about-local-notifications"></a>Informacji o powiadomieniach lokalnego

Lokalnego powiadomienia wysyłających aplikację systemu iOS mają następujące funkcje i atrybuty:

- Są one wysyłane przez aplikacje, które znajdują się lokalnie na urządzeniu użytkownika. 
- Są one można skonfigurować do używania w czasie lub pod kątem lokalizacji na podstawie wyzwalaczy. 
- Aplikacja planuje powiadomienia o urządzeniu użytkownika i jest wyświetlany po spełnieniu warunku wyzwalacza.
- Gdy użytkownik użyje powiadomienie, aplikacja będzie otrzymywać wywołania zwrotnego.

Niektóre przykłady lokalnego powiadomienia obejmują:

- Alerty kalendarza
- Przypomnienie alerty
- Wyzwalacze pamiętać lokalizacji

Aby uzyskać więcej informacji, zobacz firmy Apple [lokalnego i zdalnego Podręcznik programowania powiadomień](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) dokumentacji.

### <a name="about-remote-notifications"></a>Informacji o powiadomieniach zdalnego

Powiadomienia zdalnego wysyłających aplikację systemu iOS mają następujące funkcje i atrybuty:

- Aplikacja zawiera składnik po stronie serwera, w którym się komunikuje.
- Apple Push Notification Service (APNs) służy do przesyłania najlepszą jakość dostarczania powiadomień zdalnego do urządzenia użytkownika z serwerów działających w chmurze dewelopera.
- Gdy aplikacja odbiera powiadomienia zdalnego będzie wyświetlana użytkownikowi.
- Gdy użytkownik użyje powiadomienia, aplikacja będzie otrzymywać wywołania zwrotnego.

Niektóre powiadomienia zdalnego należą:

- Alerty wiadomości
- Sportowych aktualizacji
- Wiadomości błyskawiczne obsługi wiadomości

Istnieją dwa typy powiadomień zdalnego dostępne dla aplikacji systemu iOS:

- **Skierowany do użytkownika** — te są wyświetlane dla użytkownika na urządzeniu.
- **Aktualizacje dyskretnej** — te udostępniają mechanizm aktualizacji zawartości aplikacji systemu iOS w tle. Po odebraniu dyskretnej aktualizacji aplikacji może dotrzeć do serwerów Usuń rozwijana najnowsza zawartość.

Aby uzyskać więcej informacji, zobacz firmy Apple [lokalnego i zdalnego Podręcznik programowania powiadomień](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) dokumentacji.

### <a name="about-the-existing-notifications-api"></a>O istniejących powiadomienia interfejsu API

Przed iOS 10 użyje aplikacji systemu iOS `UIApplication` zarejestrować powiadomienia w systemie i można zaplanować, jak powinno być wyzwalane powiadomienia (albo przez czas lub lokalizacji).

Istnieje kilka problem, który deweloper może wystąpić podczas pracy z istniejących powiadomień interfejsu API:

- Znaleziono różnych wywołań zwrotnych wymagane dla lokalnego lub zdalnego powiadomienia, co może prowadzić do duplikacji kodu.
- Aplikacja ograniczył kontroli powiadomienia po miał zostało zaplanowane w systemie.
- Znaleziono różne poziomy wsparcia dla wszystkich platform istniejących firmy Apple.

### <a name="about-the-new-user-notification-framework"></a>O nowej struktury powiadomienie użytkownika

Z systemem iOS 10, Apple wprowadziła nowe struktury powiadomienie użytkownika, która zastępuje istniejącą `UIApplication` metody wymienionych powyżej.

Powiadomienie użytkownika framework zapewnia następujące korzyści:

- Znanych interfejs API, który zawiera funkcję parzystości z wcześniejszych metod, dzięki czemu można łatwo do kodu portu w ramach istniejącego.
- Zawiera rozwinięte zestaw opcji zawartości umożliwia bardziej zaawansowane funkcje powiadomień do wysłania do użytkownika.
- Pliku lokalnym i zdalnym powiadomienia są obsługiwane przez tego samego kodu i wywołania zwrotne.
- Upraszcza proces obsługi wywołania zwrotne, które są wysyłane do aplikacji, gdy użytkownik użyje powiadomienie.
- Ulepszone zarządzanie zarówno oczekujące i dostarczonego powiadomień, w tym możliwość usunąć lub zaktualizować powiadomienia.
- Dodaje możliwość czy prezentacji powiadomienia w aplikacji.
- Dodaje możliwość planowania i obsługi powiadomień z wewnątrz rozszerzeń aplikacji.
- Dodaje nowy punkt rozszerzenia dla powiadomień, się. 

Nowej struktury powiadomienie użytkownika zapewnia ujednolicone powiadomienie interfejsu API w wielu platform że Apple obsługuje, w tym: 

- **iOS** — pełna pomocy technicznej do zarządzania i zaplanować powiadomienia.
- **systemu tvOS** -dodaje zdolność do wskaźnika aplikacji ikony dla powiadomień lokalnych i zdalnych.
- **watchOS** — dodaje możliwość przesyłania dalej powiadomień użytkownika sparowanego urządzenia z systemem iOS do ich Apple Watch i umożliwia aplikacjom czujki celu lokalnego powiadomienia bezpośrednio na czujki, sama.

Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania Framework UserNotifications](https://developer.apple.com/reference/usernotifications) i [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui) dokumentacji.

## <a name="preparing-for-notification-delivery"></a>Przygotowywanie do dostarczania powiadomień o zapytaniach

Przed iOS aplikacja może wysyłać powiadomienia użytkownika, którego aplikacja musi być zarejestrowana w systemie, a ponieważ powiadomienie jest przerwania dla użytkownika, aplikacja musi jawne żądanie zgodę przed wysłaniem ich.

Istnieją trzy różne poziomy żądania powiadomień, które użytkownik może zatwierdzić dla aplikacji:

- Wyświetla transparentu.
- Alerty dźwięku.
- Badging ikonę aplikacji.

Ponadto te poziomy zatwierdzenia musi żądał i ustawić dla powiadomień lokalnych i zdalnych.

Powinno się żądać uprawnień powiadomień, jak najszybciej po uruchomieniu aplikacji, dodając następujący kod, aby `FinishedLaunching` metody `AppDelegate` i typ powiadomień odpowiednie ustawienia (`UNAuthorizationOptions`):

```csharp
using UserNotifications;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    return true;
}
```

Ponadto użytkownik zawsze może zmienić uprawnień powiadomień dla aplikacji, w dowolnej chwili za pomocą **ustawienia** aplikacji na urządzeniu. Aplikacja ma sprawdzać dostępność uprawnień żądany powiadomienia użytkownika przed rozpoczęciem prezentacji powiadomienia za pomocą następującego kodu:

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
}); 
``` 

### <a name="configuring-the-remote-notifications-environment"></a>Konfigurowanie środowiska programu zdalnego powiadomienia

Nowość w systemach iOS 10, deweloper musi informuje system operacyjny środowiska Push Notification są uruchomione jako rozwoju lub produkcji. Nie można podać te informacje może spowodować aplikacji odrzucane podczas przesyłania do iTune sklepu z aplikacjami z powiadomienie podobny do następującego:

> Brak uprawnień powiadomień wypychanych - aplikacji obejmuje interfejs API dla firmy Apple Push Notification service, ale `aps-environment` podpis aplikacji brakuje uprawnień.

Aby zapewnić wymagane uprawnienia, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij dwukrotnie `Entitlements.plist` w pliku **konsoli rozwiązania** go otworzyć do edycji.
2. Przełącz się do **źródła** widoku: 

    [![](enhanced-user-notifications-images/setup01.png "Wyświetl źródło")](enhanced-user-notifications-images/setup01.png#lightbox)
3. Kliknij przycisk **+** przycisk, aby dodać nowy klucz.
4. Wprowadź `aps-environment` dla **właściwości**, pozostaw **typu** jako `String` , a następnie wprowadź albo `development` lub `production` dla **wartość**: 

    [![](enhanced-user-notifications-images/setup02.png "Właściwość punktach dostępu do środowiska")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Zapisz zmiany w pliku.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij dwukrotnie `Entitlements.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
3. Kliknij przycisk **+** przycisk, aby dodać nowy klucz.
4. Wprowadź `aps-environment` dla **właściwości**, pozostaw **typu** jako `String` , a następnie wprowadź albo `development` lub `production` dla **wartość**: 

    [![](enhanced-user-notifications-images/setup02w.png "Właściwość punktach dostępu do środowiska")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Zapisz zmiany w pliku.

-----

### <a name="registering-for-remote-notifications"></a>Rejestrowanie na potrzeby zdalnego powiadomień

Jeśli aplikacja będzie wysyłania i odbierania powiadomień zdalnego, nadal trzeba będzie wykonać _tokenu rejestracji_ przy użyciu istniejącego `UIApplication` interfejsu API. Rejestracja wymaga podłączenia urządzenia do aktywnego połączenia dostępu do sieci usługi APNs, które wygeneruje niezbędne token, który zostanie wysłany do aplikacji. Aplikacja potrzebuje do przesyłania dalej ten token do aplikacji po stronie serwera dewelopera zarejestrować powiadomień zdalnego:

[![](enhanced-user-notifications-images/token01.png "Token rejestracji — omówienie")](enhanced-user-notifications-images/token01.png#lightbox)

Aby zainicjować wymagana rejestracja, należy użyć następującego kodu:

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

Token, który są wysyłane do aplikacji po stronie serwera dewelopera potrzebne do uwzględnienia jako część ładunek powiadomienia tego get w wysłanych z serwera do usługi APNs, podczas wysyłania powiadomień zdalnego:

[![](enhanced-user-notifications-images/token02.png "Token wchodzącego w skład ładunek powiadomienia")](enhanced-user-notifications-images/token02.png#lightbox)

Token działa jako klucz, który wiąże ze sobą, powiadomienia i otwarty lub Odpowiedz na powiadomienie aplikacji.

Aby uzyskać więcej informacji, zobacz firmy Apple [lokalnego i zdalnego Podręcznik programowania powiadomień](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) dokumentacji.

## <a name="notification-delivery"></a>Dostarczanie powiadomień

Przy użyciu aplikacji pełni zarejestrowano i zażąda wymagane uprawnienia przyznane przez użytkownika aplikacja jest teraz gotowy do wysyłania i odbierania powiadomień. 

### <a name="providing-notification-content"></a>Zawarto powiadomień

Nowe w systemach iOS 10, wszystkie powiadomienia zawierają zarówno **tytuł** i **Subtitle** który zawsze będzie wyświetlane z **treści** zawartości powiadomienia. Nowe, jest również możliwość dodawania **załączników nośnika** zawartości powiadomienia.

Aby utworzyć zawartość lokalnego powiadomienia, należy użyć poniższego kodu:

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

Zdalne powiadomień proces jest podobny:

```csharp
{
    "aps":{
        "alert":{
            "title":"Notification Title",
            "subtitle":"Notification Subtitle",
            "body":"This is the message body of the notification."
        },
        "badge":1
    }
}
```

### <a name="scheduling-when-a-notification-is-sent"></a>Planowanie podczas powiadomienia są wysyłane

Z zawartością powiadomień utworzone, aplikacja musi Zaplanuj, kiedy zostanie wyświetlone powiadomienie do użytkownika przez ustawienie *wyzwalacza*. iOS 10 udostępnia cztery różne typy wyzwalaczy:

- **Powiadomienie wypychane** — jest używane wyłącznie w przypadku zdalnego powiadomienia i jest wyzwalane, gdy pakiet APN wysyła powiadomienie do aplikacji uruchomionej na urządzeniu.
- **Interwał czasu** — umożliwia lokalnego powiadomienia do zaplanowania od czasu rozpoczęcia interwału teraz i niektóre przyszłych punkt końcowy. Na przykład:`var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`
- **Data kalendarza** — umożliwia lokalnego powiadomienia o planowanych dla określonej daty i godziny.
- **Oparte na lokalizacji** — umożliwia lokalnego powiadomienia do zaplanowania, jeśli urządzenie z systemem iOS jest wprowadzanie lub opuszczenie określonej lokalizacji geograficznej lub jest w danym bliskości wszystkie sygnały Bluetooth.

W przypadku lokalnego powiadomienia jest gotowy, aplikacja musi wywołać `Add` metody `UNUserNotificationCenter` obiektu można zaplanować jego wyświetlana użytkownikowi. Zdalnego powiadomienia o aplikacji po stronie serwera wysyła ładunek powiadomienia usługi APNs, który z kolei wysyła pakiet do urządzenia użytkownika.

Łącząc wszystkie elementy, przykładowe lokalnego powiadomienia o może wyglądać tak:

```csharp
using UserNotifications;
...

var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);

var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

## <a name="handling-foreground-app-notifications"></a>Obsługa powiadomień dotyczących aplikacji pierwszego planu

Nowość w systemach iOS 10, aplikacja może obsłużyć powiadomienia inaczej gdy działa na pierwszym planie, a powiadomienie zostanie wywołany. Zapewniając `UNUserNotificationCenterDelegate` i wdrażanie `UserNotificationCenter` metody, aplikacja może zająć ponad odpowiedzialność za wyświetlanie powiadomienia. Na przykład:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        #region Constructors
        public UserNotificationCenterDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void WillPresentNotification (UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
        {
            // Do something with the notification
            Console.WriteLine ("Active Notification: {0}", notification);

            // Tell system to display the notification anyway or use
            // `None` to say we have handled the display locally.
            completionHandler (UNNotificationPresentationOptions.Alert);
        }
        #endregion
    }
}
```

Ten kod jest po prostu wypisywanie zawartości `UNNotification` do danych wyjściowych aplikacji i prosząc systemu, aby wyświetlić Alert standardowe powiadomienia. 

Jeśli aplikacja do wyświetlania powiadomień, się, gdy na pierwszym planie i nie używać domyślnych ustawień systemowych, przekazać `None` do programu obsługi zakończenia. Przykład:

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

Z tego kodu w miejscu, należy otworzyć `AppDelegate.cs` pliku do edycji i zmień `FinishedLaunching` metodę wyglądać podobnie do następującego:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    // Watch for notifications while the app is active
    UNUserNotificationCenter.Current.Delegate = new UserNotificationCenterDelegate ();

    return true;
}
```

Ten kod jest Dołączanie niestandardowego `UNUserNotificationCenterDelegate` z góry do bieżącego `UNUserNotificationCenter` , aplikacja może obsłużyć powiadomienia, gdy jest aktywny i na pierwszym planie.

## <a name="notification-management"></a>Zarządzanie powiadomień

Nowość w systemach iOS 10, zarządzania powiadomień zapewnia dostęp zarówno oczekujące i dostarczonego powiadomień i dodaje możliwość usunięcia, zaktualizować lub podwyższyć poziom te powiadomienia.

Jest ważnym elementem zarządzania powiadomień _identyfikator żądania_ który została przypisana do powiadomienia, gdy została utworzona, a zaplanowane w systemie. Zdalne powiadomień jest przypisany przez nowy `apps-collapse-id` pole w nagłówku żądania HTTP.

Identyfikator żądania jest używane w celu wybrania powiadomienie, które chce wykonać zarządzania powiadomień w aplikacji.

### <a name="removing-notifications"></a>Usunięcie powiadomienia

Aby usunąć oczekujące powiadomienie z systemu, należy użyć poniższego kodu:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

Aby usunąć już dostarczonego powiadomienia, należy użyć poniższego kodu:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>Aktualizowanie istniejących powiadomień

Aby zaktualizować istniejące powiadomień, Utwórz nowe powiadomienie z odpowiednie parametry zmodyfikowane (np. nowe czas wyzwalacz) i dodaj go do sieci z tego samego identyfikatora żądania jako powiadomienie, które ma zostać zmodyfikowana. Przykład:


```csharp
using UserNotifications;
...

// Rebuild notification
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

// New trigger time
var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger (10, false);

// ID of Notification to be updated
var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

// Add to system to modify existing Notification
UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

Już dostarczonego powiadomienia o istniejących powiadomień będzie pobrać zaktualizowany i awansowane na początku listy na ekranach blokady i głównej i w Centrum powiadomień, jeśli już zostało odczytane przez użytkownika.

## <a name="working-with-notification-actions"></a>Praca z działań powiadomień

W systemie iOS 10 powiadomienia, które są dostarczane do użytkownika nie są statyczne i podać kilka sposobów, które użytkownik może współdziałać z nimi (z wbudowanych do akcji niestandardowych).

Istnieją trzy typy działań, które aplikację systemu iOS mogą odpowiadać na:

- **Akcja domyślna** — jest to, gdy użytkownik naciska powiadomienie, aby otworzyć aplikację i wyświetlić szczegóły danego powiadomienia.
- **Niestandardowe akcje** -te zostały dodane w systemie iOS 8 i umożliwiają szybkie dla użytkownika wykonać zadanie niestandardowe bezpośrednio z powiadomienia bez konieczności uruchamiania aplikacji. Może być przedstawiony jako listę przycisków z tytułami można dostosować lub wejściowego pola tekstowego, który można uruchomić w tle (gdzie aplikacja jest podawana mała ilość czasu do spełnienia żądania) lub pierwszego planu (gdy aplikacja jest uruchamiana na pierwszym planie fu lfill żądania). Niestandardowe akcje są dostępne zarówno dla systemu iOS, jak i watchOS.
- **Odrzuć akcji** — ta akcja jest wysyłane do aplikacji, gdy użytkownik odrzuci powiadomienie o danym.

### <a name="creating-custom-actions"></a>Tworzenie niestandardowe akcje

Aby utworzyć i zarejestrować się w systemie akcji niestandardowej, należy użyć poniższego kodu:

```csharp
// Create action
var actionID = "reply";
var title = "Reply";
var action = UNNotificationAction.FromIdentifier (actionID, title, UNNotificationActionOptions.None);

// Create category
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);
    
// Register category
var categories = new UNNotificationCategory [] { category };
UNUserNotificationCenter.Current.SetNotificationCategories (new NSSet<UNNotificationCategory>(categories)); 
```

Podczas tworzenia nowego `UNNotificationAction`, jest przypisany unikatowy identyfikator i tytuł, który będzie wyświetlany na przycisku. Domyślnie akcji zostanie utworzona jako akcję tła, jednak mogą być dostarczane opcje, aby dostosować zachowanie akcji (na przykład ustawienie go jako akcja pierwszego planu).

Wszystkie akcje utworzone muszą być powiązane z kategorią. Podczas tworzenia nowego `UNNotificationCategory`, jest przypisany unikatowy identyfikator, listę działań może przeprowadzać, listy identyfikatorów zamiar zapewniające więcej informacji na temat celem akcji w kategorii i niektóre opcje, aby kontrolować zachowanie kategorii.

Na koniec wszystkich kategorii są rejestrowane przy użyciu systemu `SetNotificationCategories` metody.

### <a name="presenting-custom-actions"></a>Przedstawienie akcji niestandardowych

Po utworzeniu zestawu akcji niestandardowych i kategorie i zarejestrowany w systemie, może być prezentowana z lokalnego lub zdalnego powiadomienia.

Dla zdalnego powiadomienia, należy ustawić `category` w zdalnym ładunek powiadomienia, który pasuje do jednej z kategorii utworzone powyżej. Na przykład:

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

Lokalne powiadomień, należy ustawić `CategoryIdentifier` właściwość `UNMutableNotificationContent` obiektu. Na przykład:

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

Ponownie ten identyfikator musi być zgodny z jednym z kategorii, które zostało utworzone powyżej.

### <a name="handling-dismiss-actions"></a>Obsługa odrzucić akcje

Jak już wspomniano akcję odrzucić mogą być wysyłane do aplikacji, gdy użytkownik odrzuci powiadomienie. Ponieważ nie jest to standardowy akcji, opcja należy można ustawić po utworzeniu kategorii. Na przykład:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>Obsługa akcji odpowiedzi

Gdy użytkownik użyje akcji niestandardowych i kategorie, które zostały utworzone powyżej, aplikacja musi wykonać żądanego zadania. Jest to zrobić, podając `UNUserNotificationCenterDelegate` i wdrażanie `UserNotificationCenter` metody. Na przykład:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        ...

        #region Override Methods
        public override void DidReceiveNotificationResponse (UNUserNotificationCenter center, UNNotificationResponse response, Action completionHandler)
        {
            // Take action based on Action ID
            switch (response.ActionIdentifier) {
            case "reply":
                // Do something
                break;
            default:
                // Take action based on identifier
                if (response.IsDefaultAction) {
                    // Handle default action...
                } else if (response.IsDismissAction) {
                    // Handle dismiss action
                }
                break;
            }

            // Inform caller it has been handled
            completionHandler();
        }
        #endregion
    }
}
```

Przekazany w `UNNotificationResponse` klasa ma `ActionIdentifier` właściwość, która może być domyślna akcja lub odrzucić akcji. Użyj `response.Notification.Request.Identifier` do testowania jakiekolwiek akcje niestandardowe.

`UserText` Właściwość przechowuje wartość pola tekstowego dowolnego użytkownika. `Notification` Właściwość przechowuje źródłowego powiadomienie, które obejmują żądania z wyzwalaczem i powiadomień zawartości. Aplikację można zdecydować, jeśli było lokalnego lub zdalnego powiadomień na podstawie typu wyzwalacza.

## <a name="working-with-service-extensions"></a>Praca z rozszerzenia usługi

Podczas pracy ze zdalnego powiadomienia _rozszerzenia usługi_ sposób, aby włączyć szyfrowanie na całej trasie wewnątrz ładunek powiadomienia. Rozszerzenia usługi są interfejsu użytkownika rozszerzeniem (dostępne w systemie iOS 10) uruchomionymi w tle z głównym celem rozbudować lub wymiana widocznej zawartości powiadomienia, zanim są prezentowane użytkownika. 

[![](enhanced-user-notifications-images/extension01.png "Omówienie rozszerzenia usługi")](enhanced-user-notifications-images/extension01.png#lightbox)

Rozszerzenia usługi są przeznaczone do szybkiego uruchamiania oraz podano tylko krótkim czas wykonywania przez system. W przypadku, gdy rozszerzenie usługi nie powiedzie się jej zadania w przydzielonym czasie, rezerwowy metoda zostanie wywołana. W przypadku niepowodzenia powrotu oryginalnego powiadomień zawartości będzie widoczny dla użytkownika.

Niektóre zastosowania potencjalnych rozszerzeń usługi obejmują:

- Udostępnianie end-to-end szyfrowania zawartości zdalnego powiadomień.
- Dodawanie załączników do zdalnego powiadomienia wzbogacić je.

### <a name="implementing-a-service-extension"></a>Wdrażanie rozszerzenia usługi

Aby zaimplementować rozszerzenia usługi w aplikacji platformy Xamarin.iOS, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otwórz rozwiązanie aplikacji w programie Visual Studio dla komputerów Mac.
2. Kliknij prawym przyciskiem myszy nazwę rozwiązania w **konsoli rozwiązania** i wybierz **Dodaj** > **Dodawanie nowego projektu**.
3. Wybierz **iOS** > **rozszerzenia** > **rozszerzenia usługi powiadomień** i kliknij przycisk **dalej** przycisk: 

    [![](enhanced-user-notifications-images/extension02.png "Wybierz rozszerzenia usługi powiadomień")](enhanced-user-notifications-images/extension02.png#lightbox)
4. Wprowadź **nazwa** rozszerzenia i kliknij **dalej** przycisk: 

    [![](enhanced-user-notifications-images/extension03.png "Wprowadź nazwę rozszerzenia")](enhanced-user-notifications-images/extension03.png#lightbox)
5. Dostosuj **Nazwa projektu** i/lub **Nazwa rozwiązania** wymagane, a następnie kliknij przycisk **Utwórz** przycisk: 

    [![](enhanced-user-notifications-images/extension04.png "Zmień nazwę projektu i/lub nazwa rozwiązania")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Otwórz rozwiązanie aplikacji w programie Visual Studio.
2. Kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksploratora rozwiązań** i wybierz **Dodaj** > **Dodawanie nowego projektu**.
3. Wybierz **iOS** > **rozszerzenia** > **rozszerzenia usługi powiadomień**: 

    [![](enhanced-user-notifications-images/extension01w.png "Wybierz rozszerzenia usługi powiadomień")](enhanced-user-notifications-images/extension01w.png#lightbox)
4. Wprowadź **nazwa** rozszerzenia i kliknij **OK** przycisku.

-----

> [!IMPORTANT]
> Identyfikator pakietu rozszerzenia usługi powinno być zgodne, identyfikator pakietu aplikacji głównej z `.appnameserviceextension` dołączany na końcu. Na przykład jeśli identyfikator pakietu aplikacji głównej `com.xamarin.monkeynotify`, rozszerzenie usługi powinien mieć identyfikator pakietu `com.xamarin.monkeynotify.monkeynotifyserviceextension`. To automatycznie należy ustawić, gdy rozszerzenie jest dodane do rozwiązania. 

Brak jednej klasy głównym w rozszerzeniu usługi powiadomień, który będzie musiał zostać zmodyfikowane w celu zapewnienie wymaganej funkcjonalności. Na przykład:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyChatServiceExtension
{
    [Register ("NotificationService")]
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Computed Properties
        public Action<UNNotificationContent> ContentHandler { get; set; }
        public UNMutableNotificationContent BestAttemptContent { get; set; }
        #endregion

        #region Constructors
        protected NotificationService (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            ContentHandler = contentHandler;
            BestAttemptContent = (UNMutableNotificationContent)request.Content.MutableCopy ();

            // Modify the notification content here...
            BestAttemptContent.Title = $"{BestAttemptContent.Title}[modified]";

            ContentHandler (BestAttemptContent);
        }

        public override void TimeWillExpire ()
        {
            // Called just before the extension will be terminated by the system.
            // Use this as an opportunity to deliver your "best attempt" at modified content, otherwise the original push payload will be used.

            ContentHandler (BestAttemptContent);
        }
        #endregion
    }
}
```

Pierwsza metoda `DidReceiveNotificationRequest`, zostanie przekazany identyfikator powiadomienia, a także powiadomień zawartości za pośrednictwem `request` obiektu. Przekazany w `contentHandler` musi być wywołane w celu przedstawienia powiadomienie do użytkownika.

Druga metoda `TimeWillExpire`, zostanie wywołana tuż przed czas ma wyczerpania rozszerzenia usługi do przetworzenia żądania. Jeśli rozszerzenie usługi nie powiedzie się wywołać `contentHandler` w wyznaczonym czasie, oryginalną zawartość będzie widoczny dla użytkownika.

### <a name="triggering-a-service-extension"></a>Wyzwolenie rozszerzenia usługi

Z rozszerzeniem usługi tworzone i dostarczane z aplikacją mogą być wyzwalane przez zmodyfikowanie zdalnego ładunek powiadomienia wysyłane do urządzenia. Na przykład:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

Nowe `mutable-content` klucza określa rozszerzenie usługi będzie musiał można uruchomić, aby zaktualizować zawartość zdalnego powiadomień. `encrypted-content` Klucz przechowuje zaszyfrowane dane, które rozszerzenia usługi można odszyfrować przed rozpoczęciem prezentacji dla użytkownika.

Spójrz na poniższym przykładzie rozszerzenie usługi:

```csharp
using UserNotification;

namespace myApp {
    public class NotificationService : UNNotificationServiceExtension {
    
        public override void DidReceiveNotificationRequest(UNNotificationRequest request, contentHandler) {
            // Decrypt payload
            var decryptedBody = Decrypt(Request.Content.UserInfo["encrypted-content"]);
            
            // Modify Notification body
            var newContent = new UNMutableNotificationContent();
            newContent.Body = decryptedBody;
            
            // Present to user
            contentHandler(newContent);
        }
        
        public override void TimeWillExpire() {
            // Handle out-of-time fallback event
            ...
        }
        
    }
}
```

Ten kod odszyfrowuje zaszyfrowaną zawartość z `encrypted-content` klucza, tworzy nową `UNMutableNotificationContent`, ustawia `Body` właściwości do odszyfrowania zawartości i używa `contentHandler` do prezentowania powiadomienie do użytkownika.

## <a name="summary"></a>Podsumowanie

W tym artykule ma opisano wszystkich sposobów powiadomienia użytkowników zostały rozszerzone przez system iOS 10. On przedstawiony nowej struktury powiadomienie użytkownika i jak z niego korzystać w aplikacji platformy Xamarin.iOS lub rozszerzenie aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Odwołanie UserNotifications Framework](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Podręcznik programowania powiadomień lokalnych i zdalnych](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
