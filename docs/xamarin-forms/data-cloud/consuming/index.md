---
title: Korzystanie z usług sieci Web
description: W tym przewodniku pokazano, jak łączyć się z usługami innej witryny sieci web w celu zapewnienia tworzenia, odczytu, aktualizacji i usuwania (CRUD) funkcje do aplikacji platformy Xamarin.Forms. Tematy obejmują komunikowania się z usługami ASMX, usługi WCF, usług REST i Azure Mobile Apps.
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: a4c842ea7fd37ade9be0a9cb3e3ff7e50a6d1491
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="consuming-web-services"></a>Korzystanie z usług sieci Web

_Ten przewodniku pokazano, jak łączyć się z usługami innej witryny sieci web w celu zapewnienia tworzenia, odczytu, aktualizacji i usuwania (CRUD) funkcje do aplikacji platformy Xamarin.Forms. Tematy obejmują komunikowania się z usługami ASMX, usługi WCF, usług REST i Azure Mobile Apps.

## <a name="consuming-an-aspnet-web-service-asmxxamarin-formsdata-cloudconsumingasmxmd"></a>[Korzystanie z usługi sieci Web platformy ASP.NET (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)

Usługi sieci Web platformy ASP.NET (ASMX) zapewniają możliwość tworzenia usług sieci web, który wysyła wiadomości za pośrednictwem protokołu HTTP przy użyciu protokołu dynamicznej konfiguracji hosta (SOAP, Simple Object Access Protocol). SOAP jest protokołem niezależne od platformy i języka umożliwiające tworzenie i uzyskiwanie dostępu do usług sieci web. Korzystającym z usług ASMX nie trzeba niczego wiedzieć o platformie, model obiektów lub język programowania używany do wdrażania usługi. Tylko muszą zrozumieć sposób wysyłania i odbierania wiadomości protokołu SOAP. W tym artykule pokazano, jak korzystać z usługi sieci web ASMX z aplikacji platformy Xamarin.Forms.

## <a name="consuming-a-windows-communication-foundation-wcf-web-servicexamarin-formsdata-cloudconsumingwcfmd"></a>[Korzystanie z usługi sieci Web Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/consuming/wcf.md)

WCF jest strukturą ujednoliconego firmy Microsoft do tworzenia aplikacji korzystających z usług. Umożliwia ona deweloperom tworzenie bezpieczne, niezawodne, transakcyjne i interoperacyjne aplikacji rozproszonej. Istnieją różnice między usług sieci Web platformy ASP.NET (ASMX) i WCF, ale ważne jest, aby zrozumieć, że WCF obsługuje te same możliwości, które zapewnia ASMX — wiadomości SOAP za pośrednictwem protokołu HTTP. W tym artykule pokazano, jak korzystać z usługi WCF SOAP z aplikacji platformy Xamarin.Forms.

## <a name="consuming-a-restful-web-servicexamarin-formsdata-cloudconsumingrestmd"></a>[Korzystanie z usługi sieci RESTful Web](~/xamarin-forms/data-cloud/consuming/rest.md)

Representational State (Transfer REST) jest architektury stylu do tworzenia usług sieci web. Żądania REST są wysyłane za pośrednictwem protokołu HTTP, przy użyciu tego samego polecenia HTTP, korzystających z przeglądarki sieci web do pobierania strony sieci web i wysyłać dane do serwerów. W tym artykule pokazano, jak korzystać z usługą sieci web RESTful aplikacji platformy Xamarin.Forms.

## <a name="consuming-an-azure-mobile-appxamarin-formsdata-cloudconsumingazuremd"></a>[Korzystanie z aplikacji mobilnej Azure](~/xamarin-forms/data-cloud/consuming/azure.md)

Aplikacje mobilne platformy Azure umożliwiają opracowywanie aplikacji z zapleczy skalowalne hostowanych w usłudze Azure App Service z obsługą przenośnych uwierzytelniania, synchronizacji w trybie offline i powiadomień wypychanych. W tym artykule, która ma zastosowanie wyłącznie do usługi Azure Mobile Apps, która użycia zaplecza Node.js, wyjaśniono, jak zapytania, wstawiania, aktualizowania i usuwania danych przechowywanych w tabeli w wystąpieniu usługi Azure Mobile Apps.

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do usług sieci Web](~/cross-platform/data-cloud/web-services/index.md)
- [Asynchroniczna pomoc techniczna — omówienie](~/cross-platform/platform/async.md)
