---
title: Notifications in Xamarin.Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: bc39faa37adae660a7751313d0d573237fadce94
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="notifications-in-xamarinandroid"></a>Notifications in Xamarin.Android


## <a name="overview"></a>Omówienie

W tej sekcji opisano sposób implementować powiadomień platformy Xamarin.Android. Zawiera opis różnych elementów interfejsu użytkownika dla systemu Android powiadomień, a w tym artykule omówiono interfejsu API użytkownika związane z tworzenie i wyświetlanie powiadomienie.


## <a name="sections"></a>Sekcje

### <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Lokalnego powiadomienia w systemie Android](local-notifications.md)

W tej sekcji opisano sposób wykonania lokalnego powiadomienia o platformie Xamarin.Android. Opisuje różne elementy interfejsu użytkownika powiadomień systemu Android oraz omówimy interfejsu API użytkownika związane z tworzenie i wyświetlanie powiadomienie. 

### <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[Wskazówki — przy użyciu lokalnego powiadomienia o platformie Xamarin.Android](local-notifications-walkthrough.md)  
 
W tym przewodniku opisano sposób użycia lokalnego powiadomienia w aplikacji platformy Xamarin.Android. Jednak przedstawia podstawy tworzenia i publikowania powiadomienia. Gdy użytkownik kliknie powiadomienie w szufladzie powiadomień uruchamiany drugi działania. 


## <a name="for-further-reading"></a>Dalsze informacje

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; Firebase Cloud Messaging (FCM) to usługa, która umożliwia wysyłanie komunikatów między aplikacjami mobilnymi i aplikacji serwera. Firebase Cloud Messaging może służyć do zaimplementowania zdalnego powiadomienia (nazywanych również powiadomienia wypychane) w aplikacji platformy Xamarin.Android.

[Powiadomienia](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; Android Developer ten temat jest wyczerpujący powiadomień systemu Android. Obejmuje on projekt, w sekcji dotyczącej zagadnień, która ułatwia projektowanie powiadomienia, że są one zgodne z wytycznymi interfejsu użytkownika dla systemu Android. Podano więcej informacje o preserviing nawigacji, podczas uruchamiania działania oraz wyjaśniono sposób wyświetlania postępu w powiadomień i kontrolować odtwarzanie multimediów na ekranie blokady. 

[NotificationListenerService](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) &ndash; ten Android usługi umożliwia aplikacji do nasłuchiwania (i interakcyjnie) zamieszczony wszystkie powiadomienia na urządzeniu z systemem Android, nie tylko powiadomienia w aplikacji jest zarejestrowany do odbierania. Należy pamiętać, że użytkownik musi jawnie udzielić uprawnienia do aplikacji, aby być w stanie nasłuchiwania powiadomienia na urządzeniu.





## <a name="related-links"></a>Linki pokrewne

- [Lokalnego powiadomienia (na przykład)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Zdalne powiadomienia (na przykład)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications/)
