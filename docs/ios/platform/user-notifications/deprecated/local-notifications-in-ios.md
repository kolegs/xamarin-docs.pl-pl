---
title: Powiadomienia w rozszerzeniu Xamarin.iOS
description: W tej sekcji pokazano, jak wdrożyć powiadomienia lokalne na platformie Xamarin.iOS. Będzie on opisano różne elementy interfejsu użytkownika powiadomienie z systemem iOS i omówić interfejsu API użytkownika związane z tworzeniem i wyświetlanie powiadomienie.
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/13/2018
ms.openlocfilehash: ef5ce97736e60ff3a03bc0d496eadcae8c6f7e61
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038537"
---
# <a name="notifications-in-xamarinios"></a>Powiadomienia w rozszerzeniu Xamarin.iOS

> [!IMPORTANT]
> Informacje przedstawione w tej sekcji dotyczy tylko systemu iOS 9 i wcześniejszych. Dla systemu iOS 10 i nowszych wersji, zobacz [Podręcznik framework powiadomień użytkownika](~/ios/platform/user-notifications/index.md).

dla systemu iOS zawiera trzy sposoby, aby poinformować użytkownika, że otrzymał powiadomienie:

- **Dźwięk lub wibracje** — z systemem iOS można odtwarzać dźwięk do powiadamiania użytkowników. Jeśli dźwięk jest wyłączona, można skonfigurować urządzenie do wibracje.
- **Alerty** — istnieje możliwość wyświetlić okno dialogowe na ekranie z informacjami o powiadomienie.
- **Wskaźniki** — po opublikowaniu powiadomienia mogą być wyświetlane wiele (oznaczonej) na ikonie aplikacji.

dla systemu iOS zapewnia również *Centrum powiadomień* , będą wyświetlane wszystkie powiadomienia, lokalnych i zdalnych, dla użytkownika. Użytkownicy mogą uzyskiwać dostęp do tego, szybko przesuwając w dół od górnej krawędzi ekranu:

![Centrum powiadomień](local-notifications-in-ios-images/image13.png "Centrum powiadomień")

## <a name="creating-local-notifications-in-ios"></a>Tworzenie lokalnego powiadomienia w systemie iOS

iOS sprawia, że dość proste do tworzenia i obsługi powiadomień lokalnych.
Po pierwsze system iOS 8 wymaga aplikacji, poproś o zezwolenie użytkownika wyświetlić powiadomienia. Dodaj następujący kod do aplikacji przed podjęciem próby wysłania powiadomienia lokalne — [dołączony przykład](https://developer.xamarin.com/samples/monotouch/LocalNotifications/) umieszcza je w **elemencie AppDelegate**firmy **FinishedLaunching** metody.

```csharp
var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes(
    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
);
application.RegisterUserNotificationSettings(notificationSettings);
```

[![Potwierdzanie możliwość wysyłania powiadomień lokalnych](local-notifications-in-ios-images/image0-sml.png "potwierdzenie możliwość wysyłania powiadomień lokalnych")](local-notifications-in-ios-images/image0.png#lightbox)

Aby zaplanować lokalnego powiadomienia, należy utworzyć `UILocalNotification` obiektu, należy ustawić `FireDate`i Zaplanuj go za pomocą `ScheduleLocalNotification` metody `UIApplication.SharedApplication` obiektu. Poniższy fragment kodu przedstawia sposób tworzenia harmonogramu powiadomienie, które fire w przyszłości jedną minutę i wyświetl alert z komunikatem:

```csharp
UILocalNotification notification = new UILocalNotification();
NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

Poniższy zrzut ekranu pokazuje, jak wygląda ten alert:

[![](local-notifications-in-ios-images/image2-sml.png "Przedstawia przykładowy alert")](local-notifications-in-ios-images/image2.png#lightbox)

Należy pamiętać, że jeśli użytkownik wybrał opcję *zezwala na* powiadomienia, wtedy nic nie zostanie wyświetlony.

Jeśli chcesz zastosować wskaźnik na ikonę aplikacji z liczbą, można ustawić go, jak pokazano w poniższym kodzie wiersza:

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

W kolejności odtwarzania dźwięku z ikoną, ustaw właściwość SoundName powiadomienie, jak pokazano w poniższym fragmencie kodu:

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Jeśli dźwięk powiadomień jest dłuższy niż 30 sekund, z systemem iOS będzie odtwarzany domyślnie dźwięku zamiast tego.

> [!IMPORTANT]
> Istnieje błąd w symulatorze systemu iOS, które będą wyzwalać powiadomienie delegata dwa razy. Ten problem nie powinien wystąpić po uruchomieniu aplikacji na urządzeniu.

## <a name="handling-notifications"></a>Obsługa powiadomień dotyczących

aplikacje dla systemu iOS obsługują powiadomienia zdalne i lokalne w prawie dokładnie taki sam sposób. Gdy aplikacja jest uruchomiona, `ReceivedLocalNotification` metody lub `ReceivedRemoteNotification` metody `AppDelegate` klasa zostanie wywołana, i powiadomienia dotyczące zostaną przekazane jako parametr.

Aplikacja może obsługiwać powiadomienia na różne sposoby. Na przykład aplikacji może po prostu wyświetl alert w celu przypomnienia użytkownikom o niektóre zdarzenia. Lub powiadomienia mogą być używane, aby wyświetlić alert dla użytkownika, który proces zakończy działanie, takie jak synchronizowanie plików na serwerze.

Poniższy kod przedstawia sposób obsługi powiadomień lokalnych i wyświetl alert oraz zresetować numer na zero:

```csharp
public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
{
    // show an alert
    UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
    okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

    Window.RootViewController.PresentViewController(okayAlertController, true, null);

    // reset our badge
    UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
}
```

Jeśli aplikacja nie jest uruchomiona, z systemem iOS będzie odtwarzał dźwięk i/lub zaktualizować wskaźnik ikony zgodnie z wymaganiami. Gdy użytkownik uruchamia aplikację skojarzonych z tym alertem, spowoduje uruchomienie aplikacji i `FinishedLaunching` zostanie wywołana metoda obiektu delegowanego aplikacja i powiadomienia dotyczące zostaną przekazane w za pośrednictwem `launchOptions` parametru. Jeśli opcje słownik zawiera klucz `UIApplication.LaunchOptionsLocalNotificationKey`, a następnie `AppDelegate` wie, że aplikacja została uruchomiona z lokalnego powiadomienia. Poniższy fragment kodu ilustruje ten proces:

```csharp
// check for a local notification
if (launchOptions.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
{
    var localNotification = launchOptions[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
    if (localNotification != null)
    {
        UIAlertController okayAlertController = UIAlertController.Create(localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

        Window.RootViewController.PresentViewController(okayAlertController, true, null);

        // reset our badge
        UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
    }
}
```

Zdalne powiadomienia `launchOptions` będzie miał `LaunchOptionsRemoteNotificationKey` ze skojarzoną `NSDictionary` zawierającego ładunek powiadomienia zdalne. Można wyodrębnić ładunek powiadomienia za pośrednictwem `alert`, `badge`, i `sound` kluczy. Poniższy fragment kodu pokazuje, jak otrzymywać powiadomienia zdalne:

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>Podsumowanie

W tej sekcji pokazano, jak tworzyć i publikować powiadomienie w rozszerzeniu Xamarin.iOS. Pokaż, jak aplikacja może reagować na powiadomienia przez zastąpienie `ReceivedLocalNotification` metody lub `ReceivedRemoteNotification` method in Class metoda `AppDelegate`.

## <a name="related-links"></a>Linki pokrewne

- [Powiadomienia lokalne (przykład)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Lokalne i powiadomienia wypychane dla deweloperów](https://developer.apple.com/notifications/)
- [Lokalne i podręcznik programowania powiadomień wypychanych](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
