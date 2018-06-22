---
title: Powiadomienia w Xamarin.iOS
description: W tej sekcji przedstawiono sposób wdrożenia lokalnego powiadomienia w platformy Xamarin.iOS. Zostanie wyjaśnić różnych elementów interfejsu użytkownika powiadomienie z systemem iOS i omówienia interfejsu API użytkownika związane z tworzenie i wyświetlanie powiadomienie.
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: d4dd759f52e52f417441f69e6417fab536daf6d3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30777915"
---
# <a name="notifications-in-xamarinios"></a>Powiadomienia w Xamarin.iOS

_W tej sekcji przedstawiono sposób wdrożenia lokalnego powiadomienia w platformy Xamarin.iOS. Zostanie wyjaśnić różnych elementów interfejsu użytkownika powiadomienie z systemem iOS i omówienia interfejsu API użytkownika związane z tworzenie i wyświetlanie powiadomienie._

> [!IMPORTANT]
> Informacje przedstawione w tej sekcji dotyczą systemu iOS 9 i wcześniejszych, pozostał tutaj do obsługi starszych wersji systemu iOS. Dla systemu iOS 10 i nowsze, zobacz [Framework powiadomienia użytkownika — przewodnik](~/ios/platform/user-notifications/index.md) do obsługi w pliku lokalnym i zdalnym powiadomienia na urządzeniu z systemem iOS.

iOS ma wskazać użytkownikowi, że otrzymał powiadomienie na trzy sposoby:

-  **Dźwięku lub wibrację** -iOS można odtwarzać dźwięk do powiadamiania użytkowników. Jeśli dźwięk jest wyłączona, można skonfigurować urządzenie do Włącz wibrację.
-  **Alerty** — można wyświetlić okna dialogowego na ekranie z informacjami o powiadomienia.
-  **Identyfikatory** — powiadomienie zostanie opublikowana, można wyświetlić wiele (oznaczonego jako) ikonę aplikacji.


udostępnia również iOS *Centrum powiadomień* który spowoduje wyświetlenie wszystkich powiadomień, zarówno lokalnych, jak i zdalnych, dla użytkownika. Użytkownicy mogą uzyskiwać dostęp do tego, szybko przesuwając w dół od górnej krawędzi ekranu:

 ![](local-notifications-in-ios-images/image13.png "Centrum powiadomień")

## <a name="creating-local-notifications-in-ios"></a>Tworzenie lokalnego powiadomienia w systemie iOS

iOS sprawia, że dość proste do tworzenia i obsługi lokalnego powiadomienia.
Po pierwsze iOS 8 wymaga aplikacjom Pytaj o zgodę użytkownika na wyświetlanie powiadomień. Dodaj następujący kod do aplikacji przed podjęciem próby wysłania powiadomienia lokalnych — przykładowe dołączone umieszcza je w **AppDelegate**w **FinishedLaunching** metody.

```csharp
var settings = UIUserNotificationSettings.GetSettingsForTypes(
  UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound
  , null);
UIApplication.SharedApplication.RegisterUserNotificationSettings (settings);
```

  [![](local-notifications-in-ios-images/image0-sml.png "Potwierdzenie umożliwia wysyłanie powiadomień lokalnych")](local-notifications-in-ios-images/image0.png#lightbox)

Można zaplanować lokalnego powiadomienia o utworzeniu `UILocalNotification` obiekt, ustaw `FireDate`i Zaplanuj go za pomocą `ScheduleLocalNotification` metoda `UIApplication.SharedApplication` obiektu. Poniższy fragment kodu będzie pokazują, jak zaplanować powiadomienie, które będą wyzwalać minutę w przyszłości i wyświetl alert z następującym komunikatem:

```csharp
UILocalNotification notification = new UILocalNotification();
NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

Poniższy zrzut ekranu pokazują, jak wygląda ten alert:

  [![](local-notifications-in-ios-images/image2-sml.png "Przykład alertu")](local-notifications-in-ios-images/image2.png#lightbox)

Należy pamiętać, że jeśli użytkownik wybrał opcję *zezwala* powiadomienia nie będzie wyświetlany.

Jeśli chcesz zastosować wskaźnika do ikony aplikacji z liczbą, możesz ustawić jak pokazano w poniższym kodzie wiersza:

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

W kolejności play dźwięk ikona, ustaw właściwość SoundName powiadomienie, jak pokazano w poniższy fragment kodu:

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Na Apple Human Interface Guidelines Jeśli powiadomienie odtwarza dźwięk, jego powinny również towarzyszyć wskaźnika lub alert, aby pomóc użytkownikowi w poznaniu aplikacji, która wygenerowała alert. Ponadto jeśli dźwięk jest dłuższy niż 30 sekund, iOS zostanie odtworzona domyślnie dźwięku zamiast tego.

> [!IMPORTANT]
> Jest to błąd w symulatorze systemu iOS, które będą wyzwalać powiadomienia delegata dwa razy. Ten problem nie powinien wystąpić przy uruchamianiu aplikacji na urządzeniu.

## <a name="handling-notifications"></a>Obsługiwanie powiadomień

aplikacje systemu iOS obsługi zdalnych i lokalnych powiadomień w prawie dokładnie taki sam sposób. Gdy aplikacja jest uruchomiona, `ReceivedLocalNotification` metody lub `ReceivedRemoteNotification` będzie można wywołać metody w klasie AppDelegate i informacji powiadomień zostanie przekazany jako parametr.

Aplikacja może obsłużyć powiadomienie na różne sposoby. Na przykład aplikacji może po prostu wyświetl alert w celu odnotowania użytkowników o niektórych zdarzeń. Lub powiadomienia mogą być wykorzystane do wyświetlenia alertu dla użytkownika, który proces zakończy pracę, takich jak pliki synchronizowanie z serwerem.

Poniższy kod przedstawia sposób obsługi lokalnego powiadomienia i wyświetl alert i zresetuj numer od zera:

```csharp
 public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
        {
            // show an alert
            UIAlertController okayAlertController = UIAlertController.Create (notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
            okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
            viewController.PresentViewController (okayAlertController, true, null);

            // reset our badge
            UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
        }
```

Jeśli aplikacja nie jest uruchomiona, iOS będzie odtwarzać dźwięk i/lub zaktualizować wskaźnika ikona zgodnie z wymaganiami. Po uruchomieniu aplikacji skojarzonych z tym alertem, aplikacja uruchomi zostanie wywołana metoda FinishedLaunching na delegata aplikacji i powiadomień informacje zostaną przekazane w za pomocą parametru opcje. Jeśli opcje słownik zawiera klucz `UIApplication.LaunchOptionsLocalNotificationKey`, a następnie AppDelegate wie, że aplikacja została uruchomiona z lokalnego powiadomienia. Poniższy fragment kodu przedstawia ten proces:

```csharp
// check for a local notification
if (options.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
 {
     var localNotification = options[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
     if (localNotification != null)
     {
        UIAlertController okayAlertController = UIAlertController.Create (localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
        viewController.PresentViewController (okayAlertController, true, null);

         // reset our badge
         UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
     }
 }
```

Jeśli zamiast tego zawiera słownik opcje zdalnego powiadomienie `LaunchOptionsRemoteNotificationKey`, i czy wynikowej wartości dla tego klucza `NSDictionary` obiektu o ładunek powiadomienia zdalnego. Ładunek powiadomienia za pomocą alertu, badge i dźwięku kluczy można wyodrębnić. Poniższy fragment kodu przedstawia sposób otrzymywać powiadomień zdalnego:

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>Podsumowanie

W tej sekcji pokazano, jak utworzyć i opublikować powiadomienie w platformy Xamarin.iOS. Pokaż, jak aplikacji mogą reagować na wysyłanie powiadomień przez zastąpienie `ReceivedLocalNotification` metody lub `ReceivedRemoteNotification` metoda `AppDelegate`.


## <a name="related-links"></a>Linki pokrewne

- [Lokalnego powiadomienia (na przykład)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Lokalne i wypychanie powiadomień dla deweloperów](https://developer.apple.com/notifications/)
- [Lokalnych i wypychanych Podręcznik programowania powiadomień](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
