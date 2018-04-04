---
title: Powiadomienia
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 1a681c2bda941d8fe015a8d4da8b99f4d85e441b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="notifications"></a>Powiadomienia

Obejrzyj aplikacji można otrzymywać powiadomienia, jeśli aplikacji systemu iOS zawierające je obsługuje. Brak obsługi wbudowanych powiadomień, w przeciwnym *muszą* Aby dodać obsługę powiadomień dodatkowe opisane poniżej, ale jeśli chcesz dostosować zachowanie powiadomień i wyglądu odczytać na.

Zapoznaj się [powiadomień systemu iOS](~/ios/platform/user-notifications/deprecated/index.md) doc, aby uzyskać więcej informacji na temat dodawania obsługi powiadomień do aplikacji systemu iOS w rozwiązaniu.

## <a name="creating-notification-controllers"></a>Tworzenie kontrolerów powiadomień

Dla scenorysu kontrolery powiadomienia mają specjalny typ segue wyzwalania je. Przeciągnięcie nowy **Kontroler interfejsu powiadomień** na scenorysu będzie automatycznie mieć segue, dołączyć:

![](notifications-images/notification-storyboard1.png "Nowy kontroler interfejsu powiadomień z segue dołączony")

Gdy segue powiadomienia wybrano można edytować jej właściwości:

![](notifications-images/notification-storyboard2.png "Wybrane segue powiadomienia.")

Po dostosowaniu kontroler może wyglądać tak jak ten przykład z WatchKitCatalog:

![](notifications-images/notifications-segue.png "Właściwości powiadomień")


Istnieją dwa typy powiadomień:

- **Wygląd Short** -nieprzewijalna widok statyczny zdefiniowane przez system.

- **Wygląd Long** — przewijanego, można dostosować widoku zdefiniowane przez użytkownika! Można określić prostszy, statycznej wersji i bardziej złożonej wersji dynamicznych.

### <a name="short-look-notification-controller"></a>Kontroler krótkiej wygląd powiadomienia

Wygląd krótkiej interfejsu użytkownika składa się z tylko ikonę aplikacji, nazwy aplikacji i ciągu tytuł powiadomienia.

Jeśli użytkownik nie ignoruje powiadomienia, system automatycznie nastąpi przełączenie do powiadomień long wygląd, zawierający więcej informacji.


### <a name="long-look-notification-controller"></a>Long — wygląd kontrolera powiadomień

System operacyjny decyduje o tym, czy mają być wyświetlane statyczny lub dynamiczny widok na podstawie różnych czynników. Musi udostępniają interfejs statyczne i opcjonalnie mogą również obejmować dynamicznego interfejsu powiadomień.

#### <a name="static"></a>Static

Widok statyczny powinien być proste i szybkie do wyświetlenia.

![](notifications-images/notification-static.png "Widok statyczny")

#### <a name="dynamic"></a>Dynamic

Widok dynamiczny można wyświetlić więcej danych i udostępniać więcej interakcji.

![](notifications-images/notification-dynamic.png "Widok dynamiczny")


## <a name="generating-notifications"></a>Generowanie powiadomień

Powiadomienia mogą pochodzić z serwera zdalnego ([usługi powiadomień wypychanych firmy Apple](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html), lub APNS) lub mogą być generowane lokalnie w aplikacji systemu iOS.

Zapoznaj się [iOS wskazówki powiadomienia](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) przykład sposób generowania powiadomień lokalnych i [WatchNotifications próbki](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/) na przykład pracy.

Lokalnego powiadomienia musi mieć `AlertTitle` wyświetlana na Apple Watch - `AlertTitle` ciągu jest wyświetlana w interfejsie wygląd krótkiej. Zarówno `AlertTitle` i `AlertBody` są wyświetlane na liście powiadomień i `AlertBody` jest wyświetlana w interfejsie wygląd Long.

Ten zrzut ekranu przedstawia `AlertTitle` będzie wyświetlany na liście powiadomień i `AlertBody` wyświetlane w interfejsie wygląd Long (przy użyciu [przykładowy kod](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)):

![](notifications-images/watch-notificationslist-sml.png "Ten zrzut ekranu przedstawia AlertTitle będzie wyświetlany na liście powiadomień") ![ ] (notifications-images/watch-notificationcontroller-sml.png "AlertBody wyświetlane w interfejsie wygląd Long")

## <a name="testing-notifications"></a>Testowanie powiadomień

Powiadomienia (lokalne i zdalne) można tylko prawidłowo sprawdzenia na urządzeniu, jednak może być symulowane, za pomocą **JSON** pliku w symulatorze systemu iOS.

### <a name="testing-on-apple-watch"></a>Testowanie na Apple Watch

Podczas testowania powiadomienia w Apple Watch, należy pamiętać, że [dokumentacji firmy Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) następujące stany:

> Po odebraniu jednej z aplikacji lokalnego lub zdalnego powiadomienia na telefonie iPhone użytkownika, iOS decyduje o tym, czy mają być wyświetlane powiadomienia na telefonie iPhone lub Apple Watch.

Jest to alluding na fakt, że iOS decyduje o tym, czy powiadomienie pojawi się na telefonie iPhone lub czujki. Jeśli sparowanego iPhone jest aktywne po otrzymaniu powiadomienia, powiadomienie jest prawdopodobnie będzie wyświetlana na telefonie iPhone i *nie* kierowane do czujki.

Upewnij się, że na czujki pojawi się powiadomienie, Wyłącz ekran iPhone (naciśnięcie przycisku zasilania raz) lub pozwolić na jego przejście w stan uśpienia. Jeśli sparowanego czujki jest w zasięgu, zasilania i jest on zużyte na nadgarstka, powiadomienia będą kierowane istnieje i są wyświetlane na czujki (wraz z niewielkie).

### <a name="testing-on-the-ios-simulator"></a>Testowanie w symulatorze systemu iOS

Możesz *musi* Podaj ładunek JSON testu podczas testowania trybu powiadomień w symulatorze systemu iOS. Ustawianie ścieżki **argumenty wykonywania niestandardowe** okna w programie Visual Studio dla komputerów Mac.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac zostanie wyświetlony dodatkowe opcje rozszerzenia wyrażenie kontrolne jest ustawiony jako **projekt startowy**.
Kliknij prawym przyciskiem myszy na projekt rozszerzenia czujki i wybierz polecenie **Uruchom z > Parametry niestandardowe...** :
    
[![](notifications-images/runwith-customparams-sml.png "Uruchomione z właściwości niestandardowych")](notifications-images/runwith-customparams.png#lightbox)
    
Spowoduje to otwarcie **argumenty wykonywania** okna, który zawiera **WatchKit** kartę. Wybierz **powiadomień** i podaj ładunek JSON, naciśnij klawisz **Execute** można uruchomić aplikacji czujki w symulatorze:
    
[![](notifications-images/runwith-execargs-sml.png "Wybierz domyślny ładunek powiadomienia")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Można ustawić ładunek powiadomienia testu w programie Visual Studio kliknij prawym przyciskiem myszy na rozszerzeniu czujki, aby edytować **właściwości projektu**. Przejdź do **debugowania** sekcji, a następnie wybierz plik JSON powiadomienia z listy (automatycznie wyświetlone zostaną wszystkie pliki JSON dołączony do projektu).
    
[![](notifications-images/runwith-execargs-sml-vs.png "Wybierz plik JSON powiadomienia")](notifications-images/runwith-execargs-vs.png#lightbox)

Gdy jest rozszerzenie czujki **projekt startowy**, Visual Studio będą wyświetlane dodatkowe opcje, jak pokazano poniżej. Wybierz jedną z **powiadomień** opcji, aby uruchomić aplikację czujki w **powiadomień** trybu (przy użyciu pliku JSON, zaznaczona w oknie właściwości):
    
![](notifications-images/runwith-vs.png "Menu urządzenia")

-----

Domyślny kontroler powiadomień wygląda następująco podczas testowania w symulatorze na domyślny ładunek JSON plik:

![](notifications-images/notification-debug-sml.png "Przykład powiadomień")

Istnieje również możliwość użycia [wiersza polecenia](~/ios/watchos/troubleshooting.md#command_line) można uruchomić symulatora systemu iOS.

### <a name="example-notification-payload"></a>Przykładowy ładunek powiadomienia

W [katalogu zestawu czujki](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) przykład jest plik przykładowy ładunek JSON **NotificationPayload.json** (wymienione poniżej).

```csharp
{
    "aps": {
        "alert": "Test message content",
        "title": "Optional title",
        "category": "myCategory"
        },

        "WatchKit Simulator Actions": [
        {
            "title": "First Button",
            "identifier": "firstButtonAction"
        }
        ],

        "customKey": "Use this file to define a testing payload for your notifications. The aps dictionary specifies the category, alert text and title. The WatchKit Simulator Actions array can provide info for one or more action buttons in addition to the standard Dismiss button. Any other top level keys are custom payload. If you have multiple such JSON files in your project, you'll be able to choose between them in when selecting to debug the notification interface of your Watch App."
    }
```



## <a name="related-links"></a>Linki pokrewne

- [WatchNotifications (lokalnego powiadomienia) (przykład)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)
- [WatchKitCatalog (przykład)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Dokumentacja czujki zestaw powiadomień firmy Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
