---
title: Microsoft Azure Mobile Apps
description: "Przykłady i kod pobiera dokumentację portalu Azure."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: fb3c26b7d090ca42328c61192c794dec1544d1d3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure Mobile Apps

_Przykłady i kod pobiera dokumentację portalu Azure._

<!--
NOTE TO AUTHORS: this page is referenced from
http://azure.microsoft.com/en-us/develop/mobile/xamarin/
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


Łącza te są dostępne w dokumentacji platformy Xamarin [Azure Mobile Apps](https://azure.microsoft.com/en-us/documentation/services/app-service/mobile/) witryny sieci Web.
Dodawanie funkcji platformy Azure do platformy Xamarin aplikacja jest tak proste, jak pobieranie [klienta Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Praca z składnik Xamarin Azure

Dokumentacja ogólna [Praca z biblioteki klienta Xamarin (składnik)](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/) wykonywać różne zadania związane z usługą Azure Mobile Apps. Ta strona zawiera wiele przykładowych fragmentów kodu, bez szczegółowe wyjaśnienia i przykłady dostępnych dla każdej z wymienionych poniżej artykułów wskazówki.

## <a name="getting-started"></a>Wprowadzenie

Ten artykuł zawiera instrukcje krok po kroku, aby uzyskać pierwszej aplikacji platformy Xamarin Azure do pracy.
Obejmuje ona tworzenie nowej aplikacji mobilnej Azure w portalu, a następnie pobierania i uruchamiania aplikacji wstępnie skonfigurowane.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-forms-get-started/)

## <a name="validate-modify-and-augment-data-in-scripts"></a>Sprawdzanie poprawności, modyfikowania i rozszerzyć danych w skryptach

Pokazuje, jak dodać skryptów po stronie serwera do tabel danych usługi Azure Mobile Services do zaimplementowania weryfikacji po stronie serwera i inne funkcje.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)


## <a name="add-paging-to-data"></a>Dodaj stronicowania danych

Prosty przykład stronicowania dużych zestawów danych przy użyciu Skip() i Take().

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)


## <a name="get-started-with-users"></a>Rozpoczynanie pracy z użytkownikami

Zapewnia pełne instrukcje dotyczące konfigurowania i kodowania ekran logowania za pomocą usług Azure Mobile Services. Dostawców uwierzytelniania obsługiwanych obejmują firmy Microsoft, Google, Facebook i Twitter.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>Autoryzowanie użytkowników w skryptach

Niektóre przykładowy kod Javascript zapleczy

-  [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>Wprowadzenie do wypychania

Wykonaj instrukcje, aby skonfigurować powiadomienia wypychane w witrynach sieci Web firmy Apple i Google, a następnie wysłać powiadomienie wypychane z usługi Azure Mobile Services do urządzenia.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-push/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-push/)


## <a name="get-started-with-notification-hubs"></a>Rozpoczynanie pracy z usługą Notification Hubs

Wykonaj instrukcje, aby skonfigurować powiadomienia wypychane na witryny sieci Web firmy Apple i Google, konfigurowanie Centrum powiadomień Azure, a następnie generowanie powiadomień wypychanych do urządzeń.

-  [iOS](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-ios-get-started/)
-  [Android](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-android-get-started/)



## <a name="related-links"></a>Linki pokrewne

- [GettingStarted (przykład)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (przykład)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (przykład)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (przykład)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [ValidateModifyData (przykład)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
- [NotificationHubs (przykład)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure przykład PCL (przez @paulbatum) (przykład)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Ścieżka szkoleniowa aplikacje mobilne platformy Azure](https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/)
