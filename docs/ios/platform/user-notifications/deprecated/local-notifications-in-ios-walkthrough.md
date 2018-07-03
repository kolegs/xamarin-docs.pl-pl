---
title: 'Przewodnik: Używanie powiadomień lokalnych w rozszerzeniu Xamarin.iOS'
description: W tej sekcji omówiono sposób użycia lokalnego powiadomienia w aplikacji platformy Xamarin.iOS. Go przedstawiono podstawowe informacje dotyczące tworzenia i publikowania powiadomienie, które wyświetli alert, gdy odebrana przez aplikację.
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: cf1e44ba4176922234fc1b6b9bfe5c463611cc7b
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403432"
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>Przewodnik: Używanie powiadomień lokalnych w rozszerzeniu Xamarin.iOS

_W tej sekcji omówiono sposób użycia lokalnego powiadomienia w aplikacji platformy Xamarin.iOS. Go przedstawiono podstawowe informacje dotyczące tworzenia i publikowania powiadomienie, które wyświetli alert, gdy odebrana przez aplikację._

> [!IMPORTANT]
> Informacje przedstawione w tej sekcji odnoszą się do systemu iOS 9 i wcześniejszych, pozostał w tym miejscu umożliwiają starszymi wersjami systemu iOS. Dla systemu iOS 10 i nowszych wersji, zobacz [przewodnik struktura powiadomień użytkownika](~/ios/platform/user-notifications/index.md) do obsługi zarówno lokalnych, jak i zdalne powiadomienia na urządzeniu z systemem iOS.

## <a name="walkthrough"></a>Przewodnik

Pozwól, Utwórz prostą aplikację, która będzie wyświetlana powiadomień lokalnych w działaniu. Ta aplikacja będzie mieć jeden przycisk. Po kliknięciu przycisku utworzy lokalnego powiadomienia. Po określonym przedziale czasu, zobaczymy powiadomienia są wyświetlane.


1. W programie Visual Studio dla komputerów Mac, Utwórz nowe rozwiązanie dla systemu iOS pojedynczy widok i wywołać go `Notifications`.
1. Otwórz `Main.storyboard` pliku, a następnie przeciągnij przycisk do widoku. Nazwij przycisk **przycisk**i nadaj jej tytuł **Dodaj powiadomienie**. Możesz również ustawić niektóre [ograniczenia](~/ios/user-interface/designer/designer-auto-layout.md) do przycisku, w tym momencie: 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "Ustawienie pewne ograniczenia na przycisku")
1. Edytuj `ViewController` klasę i dodaj następującą obsługę zdarzeń do metody ViewDidLoad:

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

    Ten kod utworzy powiadomienie, które używa dźwięk, ustawia wartość wskaźnika ikony na 1 i wyświetla alert dla użytkownika.

1. Następnie edytuj plik `AppDelegate.cs`, najpierw należy dodać następujący kod do `FinishedLaunching` metody. Firma Microsoft ma sprawdzenie, jeśli urządzenie ma zainstalowany system iOS 8, jeśli więc jesteśmy **wymagane** poproś o zezwolenie użytkownika otrzymywać powiadomienia:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
            var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
                UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
            );

            application.RegisterUserNotificationSettings (notificationSettings);
        }
    ```

1. Nadal w `AppDelegate.cs`, dodaj następującą metodę, która będzie wywoływana po otrzymaniu powiadomienia:

    ```csharp
    public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
            {
                // show an alert
                UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
                okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

                UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(okayAlertController, true, null);

                // reset our badge
                UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
            }

    ```

1. Potrzebujemy obsłużyć przypadek, w którym powiadomienia została uruchomiona z powodu lokalnego powiadomienia. Edytuj metodę `FinishedLaunching` w `AppDelegate` obejmujący następujący fragment kodu:


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

1. Na koniec uruchom aplikację. W systemie iOS 8 użytkownik zostanie wyświetlony monit Zezwalaj na powiadomienia. Kliknij przycisk **OK** a następnie kliknij przycisk **Dodaj powiadomienie** przycisku. Po krótkiej przerwie powinny zostać wyświetlone okna dialogowego alertu, jak pokazano na poniższych zrzutach ekranu:

    ![](local-notifications-in-ios-walkthrough-images/image0.png "Trwa Potwierdzanie możliwość wysyłania powiadomień") ![ ] (local-notifications-in-ios-walkthrough-images/image1.png "przycisku Dodaj powiadomienie") ![ ] (local-notifications-in-ios-walkthrough-images/image2.png "dialog powiadomień o alertach")

## <a name="summary"></a>Podsumowanie

W tym instruktażu pokazano, jak używać różnych interfejsu API do tworzenia i publikowania powiadomienia w systemie iOS. Program ten pokazała również sposób aktualizacji ikona aplikacji za pomocą wskaźnika opinie niektórych aplikacji określonego użytkownika.


## <a name="related-links"></a>Linki pokrewne

- [Powiadomienia lokalne (przykład)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Lokalne i przewodnik programowania powiadomień wypychanych](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
