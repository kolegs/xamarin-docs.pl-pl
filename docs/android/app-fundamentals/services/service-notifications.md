---
title: "Usługa powiadomień"
description: "W tym przewodniku opisano, jak usługi systemu Android mogą za pomocą lokalnego powiadomienia wysyłania informacji do użytkownika."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 3c06fca9c6d8c3cd91889007bd1879149771622b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="service-notifications"></a>Usługa powiadomień

_W tym przewodniku opisano, jak usługi systemu Android mogą za pomocą lokalnego powiadomienia wysyłania informacji do użytkownika._


## <a name="service-notifications-overview"></a>Omówienie usługi powiadomień

Powiadomienia usługi Zezwalaj aplikacji wyświetlić informacje dla użytkownika, nawet jeśli aplikacji systemu Android nie jest na pierwszym planie. Istnieje możliwość powiadomienie zapewnienie akcje użytkownika, takie jak wyświetlanie działanie aplikacji. W poniższym przykładzie kodu pokazano, jak usługa może wysłać powiadomienie do użytkownika:

```csharp
[Service]
public class MyService: Service 
{
    // A notification requires an id that is unique to the application.
    const int NOTIFICATION_ID = 9000;
    
    public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
    {
        // Code omitted for clarity - here is where the service would do something.
    
        // Work has finished, now dispatch anotification to let the user know.
        Notification.Builder notificationBuilder = new Notification.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_notification_small_icon)
            .SetContentTitle(Resources.GetString(Resource.String.notification_content_title))
            .SetContentText(Resources.GetString(Resource.String.notification_content_text));
        
        var notificationManager = (NotificationManager)GetSystemService(NotificationService);
        notificationManager.Notify(NOTIFICATION_ID, notificationBuilder.Build());
    }
}
```

Przykładem powiadomienia, który jest wyświetlany jest ten zrzut ekranu:

[![Ikona powiadomienia wyświetlana na pasku stanu](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

Gdy slajdów użytkownika w dół ekranu powiadomień od góry, wyświetlane jest powiadomienie pełnej:

![Notication wyświetlane w powiadomień na pasku zadań](service-notifications-images/02-fullnotification.png)


## <a name="updating-a-notification"></a>Powiadomienie o aktualizacji

Aby zaktualizować powiadomienia, usługa zostanie ponownie opublikować powiadomień przy użyciu tego samego identyfikatora powiadomienia. Android będzie wyświetlić lub zaktualizować powiadomień na pasku stanu, w razie potrzeby.

```csharp 
void UpdateNotification(string content)
{
   var notification = GetNotification(content, pendingIntent);

   NotificationManager notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
   notificationManager.Notify(NOTIFICATION_ID, notification);
}

Notification GetNotification(string content, PendingIntent intent)
{
   return new Notification.Builder(this)
           .SetContentTitle(tag)
           .SetContentText(content)
           .SetSmallIcon(Resource.Drawable.NotifyLg)
           .SetLargeIcon(BitmapFactory.DecodeResource(Resources, Resource.Drawable.Icon))
           .SetContentIntent(intent).Build();
}
```

Więcej informacji na temat powiadomień jest dostępna w [lokalnego powiadomienia](~/android/app-fundamentals/notifications/local-notifications.md) sekcji [powiadomień systemu Android](~/android/app-fundamentals/notifications/index.md) przewodnik.


## <a name="related-links"></a>Linki pokrewne

- [Lokalnego powiadomienia w systemie Android](~/android/app-fundamentals/notifications/local-notifications.md)
