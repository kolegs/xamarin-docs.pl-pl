---
title: Aplikacje mobilne Microsoft Azure
description: Ten dokument prowadzi do prowadnice, które opisują sposób tworzenia aplikacji platformy Xamarin, który jest podłączony do platformy Azure. Praca z składnik Azure Xamarin, użytkowników i powiadomień wypychanych zawarto informacje.
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
author: conceptdev
ms.author: crdun
ms.date: 04/02/2017
ms.openlocfilehash: baa687bfb3b2e8306e70e83b6a6ee54595110860
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780633"
---
# <a name="microsoft-azure-mobile-apps"></a>Aplikacje mobilne Microsoft Azure

_Przykłady i kod pobiera dokumentację portalu Azure._

<!--
NOTE TO AUTHORS: this page is referenced from
http://azure.microsoft.com/develop/mobile/xamarin/
as https://developer.xamarin.com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started  http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data   http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push   http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs  http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data    http://go.microsoft.com/fwlink/p/?LinkId=331330
-->


Łącza te są dostępne w dokumentacji platformy Xamarin [Azure Mobile Apps](https://docs.microsoft.com/azure/app-service-mobile/) witryny sieci Web.
Dodawanie funkcji platformy Azure do aplikacji platformy Xamarin, pobierając [klienta Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Praca z składnik Xamarin Azure

Dokumentacja ogólna [Praca z biblioteki klienta Xamarin (składnik)](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library) wykonywać różne zadania związane z usługą Azure Mobile Apps. Ta strona zawiera wiele przykładowych fragmentów kodu, bez szczegółowe wyjaśnienia i przykłady dostępnych dla każdej z wymienionych poniżej artykułów wskazówki.

## <a name="getting-started"></a>Wprowadzenie

Ten artykuł zawiera instrukcje krok po kroku, aby uzyskać pierwszej aplikacji platformy Xamarin Azure do pracy.
Obejmuje ona tworzenie nowej aplikacji mobilnej Azure w portalu, a następnie pobierania i uruchamiania aplikacji wstępnie skonfigurowane.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

-  [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

-  [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>Rozpoczynanie pracy z użytkownikami

Zapewnia pełne instrukcje dotyczące konfigurowania i kodowania ekran logowania za pomocą usług Azure Mobile Services. Dostawców uwierzytelniania obsługiwanych obejmują firmy Microsoft, Google, Facebook i Twitter.

-  [iOS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>Autoryzowanie użytkowników w skryptach

Niektóre przykładowy kod Javascript zapleczy

-  [TODO.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>Wprowadzenie do wypychania

Wykonaj instrukcje, aby skonfigurować powiadomienia wypychane w witrynach sieci Web firmy Apple i Google, a następnie wysłać powiadomienie wypychane z usługi Azure Mobile Services do urządzenia.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)


## <a name="get-started-with-notification-hubs"></a>Rozpoczynanie pracy z usługą Notification Hubs

Wykonaj instrukcje, aby skonfigurować powiadomienia wypychane na witryny sieci Web firmy Apple i Google, konfigurowanie Centrum powiadomień Azure, a następnie generowanie powiadomień wypychanych do urządzeń.

-  [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
-  [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)



## <a name="related-links"></a>Linki pokrewne

- [GettingStarted (przykład)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (przykład)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (przykład)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (przykład)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs (przykład)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Ścieżka szkoleniowa aplikacje mobilne platformy Azure](https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->
