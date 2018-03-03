---
title: Microsoft Azure
description: "Pobiera dokumentacji i przykładowego kodu na platformie Azure."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/09/2017
ms.openlocfilehash: 55213de8d2c93349598bcc9b10232535f09bc3d6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-azure"></a>Microsoft Azure

_Pobiera dokumentacji i przykładowego kodu na platformie Azure._

[ ![](images/evolve-mikej-azure-sml.png "Funkcje usługi aplikacji platformy Azure są łatwe do dodania do aplikacji platformy Xamarin, łącznie z magazynu danych w chmurze i powiadomień wypychanych i platform")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Rozwija 2016: Tworzenie połączonych aplikacji za pomocą platformy Azure i Xamarin](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Połączona usług w programie Visual Studio dla komputerów Mac

Nowy [usług połączonych](connected-services.md) funkcji programu Visual Studio for Mac ułatwia deweloperom szybkie i łatwe dodawanie funkcji platformy Azure do aplikacji dla urządzeń przenośnych z w środowisku IDE. Obecnie dostępne do testowania kanału alfa.


## <a name="azure-app-services"></a>Usługi aplikacji Azure

Kolekcja [dokumentacji usługi Azure Mobile Apps](~/cross-platform/data-cloud/mobile-apps.md) która prowadzi użytkownika przez proces wdrażania [klienta Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).
Xamarin oferuje również Azure Messaging dzaj pakietami nuget [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) i [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) pomagających w realizacji powiadomień wypychanych na różnych platformach.

Konfigurowanie aplikacji na [portal usługi aplikacji Azure](https://portal.azure.com/) dostępu do aplikacji mobilnych, interfejsów API sieci Web, magazynu i wiele innych. Dowiedz się więcej o [jak usługi aplikacji są różne](http://azure.microsoft.com/en-us/updates/whats-new-with-azure-app-service/) i obejrzeć w [tych klipów wideo firmy Microsoft](http://azure.microsoft.com/en-us/campaigns/azure-march-announcement/).

## <a name="active-directory-authentication"></a>Uwierzytelnianie usługi Active Directory

[Usługa Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md) może służyć do logowania użytkowników w aplikacji platformy Xamarin za pośrednictwem [Xamarin.Auth składnika](https://www.nuget.org/packages/Xamarin.Auth/).
Następnie dodatkowe usługi, takie jak usługi Office 365 mają dostęp do aplikacji.

## <a name="webapi"></a>WebAPI

Interfejs API sieci Web firmy Microsoft uwidacznia interfejs REST przypominającej, które mogą być łatwo używane przez aplikacje platformy Xamarin.
Użytkownik może łatwo rozkręcenia [witryny sieci Web Azure](https://trywebsites.azurewebsites.net/) i tworzenie aplikacji na podstawie WebAPI do nawiązania połączenia aplikacji platformy Xamarin.


###  <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[Wprowadzenie do usługi sieci Web](~/cross-platform/data-cloud/web-services/index.md)

Ten samouczek przedstawia sposób integrowania REST, WCF i SOAP sieci web technologii usług z aplikacjami mobilnymi Xamarin. Go sprawdza różnych implementacji usługi, ocenia dostępne narzędzia i biblioteki w celu ich integracji i zawiera przykładowe wzorce służący do konsumowania danych usługi. Ponadto zapewnia ogólne omówienie tworzenia usługi RESTful sieci web do użytku z aplikacjami mobilnymi Xamarin.

## <a name="samples"></a>Przykłady

Oprócz [przykłady dokumentacji](https://github.com/xamarin/mobile-samples/tree/master/Azure), następujące aplikacje pełną pokazują różne funkcje platformy Azure, włączyć do aplikacji platformy Xamarin:

- [Sport](https://github.com/xamarin/Sport) — przyjazne Ligi sportowych śledzenia aplikacji, który używa danych pamięci masowej i wypychania powiadomień.
- [Chwil](https://github.com/pierceboggan/Moments) — błyskawicznych fotografii udostępniania, który używa usługi Azure Storage dla obrazów.
- [Xamarin CRM](https://github.com/xamarin/app-crm) — używa interfejsu API sieci Web zaplecza.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) — usługi Azure Mobile Apps.

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) — przykład dla [serii architektura](https://www.microsoft.com/net/learn/architecture) z książek elektronicznych.
- [MyDriving](https://azure.microsoft.com/en-us/campaigns/mydriving/) — Azure + IoT próbkowania Build 2016.


## <a name="related-links"></a>Linki pokrewne

- [Azure przykład PCL (przez @paulbatum) (przykład)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure portal](http://azure.microsoft.com/)
- [Klienta mobilnego programu Xamarin (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
