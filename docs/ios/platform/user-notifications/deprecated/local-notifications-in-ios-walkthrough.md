---
title: "Wskazówki — przy użyciu lokalnego powiadomienia w Xamarin.iOS"
description: "W tej sekcji zostanie omówiony sposób użycia lokalnego powiadomienia w aplikacji platformy Xamarin.iOS. Będzie on pokazują podstawy tworzenia i publikowania powiadomienie, które będą wyskakujące alert, gdy odebrana w aplikacji."
ms.topic: article
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 72a66fae7a1d4554643c1b52796256cc0b3dbd31
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>Wskazówki — przy użyciu lokalnego powiadomienia w Xamarin.iOS

_W tej sekcji zostanie omówiony sposób użycia lokalnego powiadomienia w aplikacji platformy Xamarin.iOS. Będzie on pokazują podstawy tworzenia i publikowania powiadomienie, które będą wyskakujące alert, gdy odebrana w aplikacji._

> [!IMPORTANT]
> **Uwaga:** informacje w tej sekcji dotyczą systemu iOS 9 i wcześniejszych, pozostał tutaj do obsługi starszych wersji systemu iOS. Dla systemu iOS 10 i nowsze, zobacz [Framework powiadomienia użytkownika — przewodnik](~/ios/platform/user-notifications/index.md) do obsługi w pliku lokalnym i zdalnym powiadomienia na urządzeniu z systemem iOS.

## <a name="walkthrough"></a>Wskazówki

Umożliwiają tworzenie prostej aplikacji, który będzie informować o lokalnego powiadomienia w akcji. Ta aplikacja będzie zawierać pojedynczy przycisk na nim. Po kliknięciu przycisku na przycisku, utworzy lokalnego powiadomienia. Po określonym przedziale czasu, zostanie wyświetlone powiadomienia są wyświetlane.


1. W programie Visual Studio dla komputerów Mac, Utwórz nowe rozwiązanie iOS pojedynczego widoku i nadaj mu `Notifications`.
1. Otwórz `Main.storyboard` pliku, a następnie przeciągnij przycisk Widok. Nazwa przycisku **przycisk**i nadaj mu tytuł **dodać powiadomienia**. Można również ustawić niektórych [ograniczenia](~/ios/user-interface/designer/designer-auto-layout.md) na przycisk w tym momencie: 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "Ustawienie ograniczeń na przycisku")
1. Edytuj `ViewController` klasy, a następnie dodaj poniższe programu obsługi zdarzeń do metody ViewDidLoad:

    ```csharp
    button.TouchUpInside += (sender, e) =>
    {
        // create the notification
        var notification = new UILocalNotification();

        // set the fire date (the date time in which it will fire)
        notification.FireDate = NSDate.FromTimeIntervalSinceNow(60);

        // configure the alert
        notification.AlertAction = "View Alert";
        notification.AlertBody = "Your one minute alert has fired!";

        // modify the badge
        notification.ApplicationIconBadgeNumber = 1;

        // set the sound to be the default sound
        notification.SoundName = UILocalNotification.DefaultSoundName;

        // schedule it
        UIApplication.SharedApplication.ScheduleLocalNotification(notification);
    };
    ```

    Ten kod będzie utworzyć powiadomienie, które używa dźwięk, ustawia wartość wskaźnika ikona 1 i wyświetla alert dla użytkownika.

1. Następnie przeprowadź edycję pliku `AppDelegate.cs`, najpierw dodaj następujący kod do `FinishedLaunching` metody. Firma Microsoft ma sprawdzenie, czy urządzenie ma zainstalowany system iOS 8, jeśli więc mamy **wymagane** poprosić o uprawnienia użytkownika do odbierania powiadomień:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
            var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
                UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
            );

            application.RegisterUserNotificationSettings (notificationSettings);
        }
    ```

1. Nadal `AppDelegate.cs`, dodaj następującą metodę, która będzie wywoływana po otrzymaniu powiadomienia:

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

1. Musimy obsługę w przypadku, gdy powiadomienia została uruchomiona z powodu lokalnego powiadomienia. Edytowanie metody `FinishedLaunching` w `AppDelegate` uwzględnienie poniższy fragment kodu:


    ```csharp
    // check for a notification

    if (launchOptions != null)
    {
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
    }

    ```

1. Na koniec uruchom aplikację. W systemie iOS 8 pojawi się monit umożliwia powiadomienia. Kliknij przycisk **OK** , a następnie kliknij przycisk **dodać powiadomienia** przycisku. Po krótkiej przerwie powinna zostać wyświetlona okna dialogowego alertu, jak pokazano na poniższych zrzutach ekranu:

    ![](local-notifications-in-ios-walkthrough-images/image0.png "Potwierdzenie umożliwia wysyłanie powiadomień") ![ ] (local-notifications-in-ios-walkthrough-images/image1.png "przycisku Dodaj powiadomień") ![ ] (local-notifications-in-ios-walkthrough-images/image2.png "okna dialogowego alertu powiadomień")

## <a name="summary"></a>Podsumowanie

W tym przewodniku pokazano, jak za pomocą różnych interfejsu API do tworzenia i publikowania powiadomienia w systemie iOS. Konieczne jest również wykazanie aktualizacji ikona aplikacji ze wskaźnika do przekazywania opinii określonych niektórych aplikacji do użytkownika.


## <a name="related-links"></a>Linki pokrewne

- [Lokalnego powiadomienia (na przykład)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Lokalne i przewodnik programowania w języku powiadomień wypychanych](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
