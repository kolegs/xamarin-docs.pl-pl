---
title: Usługi danych i usługi w chmurze
description: Można korzystać z aplikacji platformy Xamarin.Forms implementowane przy użyciu różnych technologii usług sieci web, a w tym przewodniku zbada, jak to zrobić.
ms.prod: xamarin
ms.assetid: 0601D9D0-C8D2-4C3B-A749-A340BDBF64A4ß
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 6e67820fa83ddea46f934b4eaedde2c6334f9cc6
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31045058"
---
# <a name="data--cloud-services"></a>Usługi danych i usługi w chmurze

_Można korzystać z aplikacji platformy Xamarin.Forms implementowane przy użyciu różnych technologii usług sieci web, a w tym przewodniku zbada, jak to zrobić._

Aby obejrzeć wprowadzenie do korzystania z usług sieci web i platform na platformie Xamarin, zobacz [wprowadzenie do usługi sieci Web](~/cross-platform/data-cloud/web-services/index.md).

## <a name="understanding-the-samplexamarin-formsdata-cloudwalkthroughmd"></a>[Omówienie przykładu](~/xamarin-forms/data-cloud/walkthrough.md)

Ten artykuł zawiera wskazówki przykładowej aplikacji platformy Xamarin.Forms, który demonstruje sposób komunikowania się z usługami sieci web inną. Tematy obejmują Anatomia aplikacji, strony i modelu danych i wywoływanie operacji usługi sieci web.

## <a name="consuming-web-servicesxamarin-formsdata-cloudconsumingindexmd"></a>[Korzystanie z usług internetowych](~/xamarin-forms/data-cloud/consuming/index.md)

W tym przewodniku pokazano, jak łączyć się z usługami innej witryny sieci web w celu zapewnienia tworzenia, odczytu, aktualizacji i usuwania (CRUD) funkcje do aplikacji platformy Xamarin.Forms. Tematy obejmują komunikacji z [usługami ASMX](consuming/asmx.md), [usługi WCF](consuming/wcf.md), [usługi REST](consuming/rest.md), i [Azure Mobile Apps](consuming/azure.md).

## <a name="authenticating-access-to-web-servicesxamarin-formsdata-cloudauthenticationindexmd"></a>[Uwierzytelnianie dostępu do usług internetowych](~/xamarin-forms/data-cloud/authentication/index.md)

W tym przewodniku objaśniono sposób integrowania usługi uwierzytelniania do aplikacji platformy Xamarin.Forms, aby użytkownicy mogli udostępniać wewnętrznej bazie danych podczas tylko mieli dostęp do swoich własnych danych. Tematy zawierają [przy użyciu uwierzytelniania podstawowego, za pomocą usługi REST](authentication/rest.md), [do uwierzytelniania OAuth dostawców tożsamości za pomocą składnika Xamarin.Auth](authentication/oauth.md)i przy użyciu wbudowanych uwierzytelniania mechanizmy oferowane przez [Azure Mobile Apps](authentication/azure.md).

## <a name="synchronizing-data-with-web-servicessyncindexmd"></a>[Synchronizowanie danych z usługami internetowymi](sync/index.md)

W tym artykule opisano sposób dodawania funkcji synchronizacji w trybie offline do aplikacji platformy Xamarin.Forms. Synchronizacja w trybie offline umożliwia użytkownikom na interakcję z aplikacji mobilnej, przeglądanie, dodawanie lub modyfikowanie danych, nawet jeśli nie ma połączenia sieciowego. Zmiany są przechowywane w lokalnej bazie danych, a gdy urządzenie jest w trybie online, zmiany można synchronizować z usługą sieci web.

## <a name="sending-push-notificationspush-notificationsindexmd"></a>[Wysyłanie powiadomień push](push-notifications/index.md)

W tym artykule przedstawiono sposób dodawania powiadomienia wypychane do aplikacji platformy Xamarin.Forms. Centra powiadomień Azure zapewnia infrastrukturę skalowalne wypychanych umożliwiający wysyłanie powiadomień wypychanych przenośnych z poziomu dowolnego zaplecza na dowolną platformę mobilną, dzięki czemu na złożoność używanego w wewnętrznej bazie danych o do komunikowania się z różnych systemów powiadomień platformy.

## <a name="storing-files-in-the-cloudstorageindexmd"></a>[Przechowywanie plików w chmurze](storage/index.md)

W tym artykule przedstawiono, jak za pomocą platformy Xamarin.Forms do przechowywania tekstu i danych binarnych w usłudze Azure Storage oraz sposobu uzyskiwania dostępu do danych. Magazyn Azure to rozwiązanie magazynu w chmurze skalowalne może służyć do przechowywania danych niestrukturalnych i strukturalnych.

## <a name="searching-data-in-the-cloudsearchindexmd"></a>[Wyszukiwanie danych w chmurze](search/index.md)

W tym artykule przedstawiono sposób korzystania z biblioteki usługi Microsoft Azure wyszukiwania Integrowanie usługi Azure Search w aplikacji platformy Xamarin.Forms. Wyszukiwanie Azure to usługa w chmurze, który zapewnia indeksowanie i badania możliwości przekazywane dane. Spowoduje to usunięcie wymagania dotyczące infrastruktury i złożoności algorytmu wyszukiwania tradycyjnie skojarzone z wdrożeniem funkcji wyszukiwania w aplikacji.

## <a name="storing-data-in-a-document-databasecosmosdbindexmd"></a>[Zapisywanie danych w bazie danych dokumentów](cosmosdb/index.md)

W tym przewodniku przedstawiono sposób użycia rozwiązania Cosmos Azure DB .NET Standard biblioteki klienta do integracji z bazą danych dokumentów bazy danych Azure rozwiązania Cosmos w aplikacji platformy Xamarin.Forms. Baza danych dokumentów bazy danych Azure rozwiązania Cosmos jest bazą danych NoSQL, który zapewnia małe opóźnienia dostępu do dokumentów JSON, oferty usługi szybkie, wysokiej dostępności i skalowalności bazy danych dla aplikacji, które wymagają bezproblemowego skalowania i globalnej replikacji.

## <a name="adding-intelligence-with-cognitive-servicescognitive-servicesindexmd"></a>[Dodawanie analizy za pomocą usług Cognitive Services](cognitive-services/index.md)

W tym przewodniku wyjaśniono, jak korzystać z niektórych interfejsów API usługi kognitywnych firmy Microsoft w aplikacji platformy Xamarin.Forms. Zestaw interfejsów API, zestawy SDK i usług dostępnych dla deweloperów można wprowadzić do swoich aplikacji inteligentne, dodając funkcje, takie jak rozpoznawanie twarzy, rozpoznawanie mowy i zrozumienia języka są usługi kognitywnych.
