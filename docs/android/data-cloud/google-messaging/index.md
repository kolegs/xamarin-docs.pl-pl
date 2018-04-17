---
title: Google Messaging
description: Ta sekcja zawiera przewodniki dotyczące sposobu wdrożenia aplikacji platformy Xamarin.Android przy użyciu usług obsługi wiadomości Google.
ms.prod: xamarin
ms.assetid: 85E8DF92-D160-4763-A7D3-458B4C31635F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: cf1eaec3dfee7c3457a4614147c9b5564843b2a7
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="google-messaging"></a>Google Messaging

_Ta sekcja zawiera przewodniki dotyczące sposobu wdrożenia aplikacji platformy Xamarin.Android przy użyciu usług obsługi wiadomości Google._

## <a name="firebase-cloud-messagingfirebase-cloud-messagingmd"></a>[Usługa Firebase Cloud Messaging](firebase-cloud-messaging.md)

Firebase Cloud Messaging (FCM) to usługa, która umożliwia wysyłanie komunikatów między aplikacjami mobilnymi i aplikacji serwera. FCM jest następcą Google Cloud Messaging firmy Google. Ten artykuł zawiera omówienie działania FCM i zapewnia szczegółową procedurę uzyskania poświadczeń, dzięki czemu aplikacja może używać FCM usług.

## <a name="remote-notifications-with-firebase-cloud-messagingremote-notifications-with-fcmmd"></a>[Zdalnego powiadomienia z Firebase Cloud Messaging](remote-notifications-with-fcm.md)

Ten przewodnik zawiera szczegółowe informacje dotyczące wykonania zdalnego powiadomienia (nazywanych również powiadomienia wypychane) za pomocą Firebase Cloud Messaging w aplikacji platformy Xamarin.Android. Przedstawia wdrożenie różnych klas, które są potrzebne do komunikacji z wiadomości chmury Firebase (FCM), przykłady sposobu konfigurowania manifestu systemu Android do uzyskiwania dostępu do FCM i prezentuje działanie podrzędne wiadomości przy użyciu Firebase W konsoli.

## <a name="google-cloud-messaginggoogle-cloud-messagingmd"></a>[Usługa Google Cloud Messaging](google-cloud-messaging.md)

Ta sekcja zawiera ogólne omówienie jak Google Cloud Messaging (GCM) tras wiadomości między aplikacji i serwera aplikacji, i zapewnia szczegółową procedurę uzyskania poświadczeń, dzięki czemu aplikacja może używać usługi GCM. (Należy pamiętać, że usługi GCM została zastąpiona FCM).

> [!NOTE]
> GCM została zastąpiona [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM serwera i klienta interfejsów API [są przestarzałe](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) i nie będzie dostępna tak szybko, jak 11 kwietnia 2019.

## <a name="remote-notifications-with-google-cloud-messagingremote-notifications-with-gcmmd"></a>[Powiadomienia zdalnego przy użyciu usługi Google Cloud Messaging](remote-notifications-with-gcm.md)

Ta sekcja zawiera szczegółowe informacje dotyczące implementować zdalnego powiadomień platformy Xamarin.Android przy użyciu usługi Google Cloud Messaging.
Wyjaśniono różne składniki, których należy użyć do włączenia usługi Google Cloud Messaging wiadomości w aplikacji systemu Android.


