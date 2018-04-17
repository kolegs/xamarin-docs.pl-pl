---
title: Uwierzytelnianie dostępu do usług sieci Web
description: W tym przewodniku objaśniono sposób integrowania usługi uwierzytelniania do aplikacji platformy Xamarin.Forms, aby użytkownicy mogli udostępniać wewnętrznej bazie danych podczas tylko mieli dostęp do swoich własnych danych. Tematy obejmują usługi REST, za pomocą składnika Xamarin.Auth do uwierzytelniania OAuth dostawców tożsamości, przy użyciu uwierzytelniania podstawowego i za pomocą wbudowanego uwierzytelniania za pomocą mechanizmów oferowanych przez różnych dostawców.
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: bc34cf265885708fa6392936a8dbc9d82796e2fd
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="authenticating-access-to-web-services"></a>Uwierzytelnianie dostępu do usług sieci Web

_W tym przewodniku objaśniono sposób integrowania usługi uwierzytelniania do aplikacji platformy Xamarin.Forms, aby użytkownicy mogli udostępniać wewnętrznej bazie danych podczas tylko mieli dostęp do swoich własnych danych. Tematy obejmują usługi REST, za pomocą składnika Xamarin.Auth do uwierzytelniania OAuth dostawców tożsamości, przy użyciu uwierzytelniania podstawowego i za pomocą wbudowanego uwierzytelniania za pomocą mechanizmów oferowanych przez różnych dostawców._

## <a name="authenticating-a-restful-web-servicerestmd"></a>[Uwierzytelnianie usługi sieci RESTful Web](rest.md)

HTTP obsługuje kilka mechanizmów uwierzytelniania w celu kontroli dostępu do zasobów. Uwierzytelnianie podstawowe zapewnia dostęp do zasobów tylko do klientów mających prawidłowe poświadczenia. W tym artykule przedstawiono sposób użycia uwierzytelniania podstawowego do ochrony dostępu do zasobów usługi sieci web RESTful.

## <a name="authenticating-users-with-an-identity-provideroauthmd"></a>[Uwierzytelnianie użytkowników przy użyciu dostawcy tożsamości](oauth.md)

Xamarin.Auth jest SDK i platform do uwierzytelniania użytkowników i przechowywania ich kont. Obejmuje on wystawców uwierzytelnienia OAuth, które zapewniają obsługę służący do konsumowania dostawców tożsamości, takich jak Google, Microsoft, Facebook i Twitter. W tym artykule opisano sposób użycia Xamarin.Auth do zarządzania procesem uwierzytelniania w aplikacji platformy Xamarin.Forms.

## <a name="authenticating-users-with-azure-mobile-appsazuremd"></a>[Uwierzytelniania użytkowników przy użyciu usługi Azure Mobile Apps](azure.md)

Aplikacje mobilne platformy Azure korzystają z różnych dostawców tożsamości zewnętrznych do obsługi uwierzytelniania i autoryzacji użytkowników aplikacji. Uprawnienia można następnie ustawić w tabelach, aby ograniczyć dostęp tylko do uwierzytelnionych użytkowników. W tym artykule opisano sposób korzystania z usługi Azure Mobile Apps do zarządzania procesem uwierzytelniania w aplikacji platformy Xamarin.Forms.

## <a name="authenticating-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Uwierzytelnianie użytkowników w usłudze Azure Active Directory B2C](azure-ad-b2c.md)

Usługa Azure Active Directory B2C jest chmury rozwiązania do zarządzania tożsamościami internetowych i aplikacji dla urządzeń przenośnych. W tym artykule przedstawiono sposób korzystania z biblioteki uwierzytelniania firmy Microsoft (MSAL) i Azure Active Directory B2C integrowania zarządzania tożsamością użytkowników aplikacji platformy Xamarin.Forms.

## <a name="integrating-azure-active-directory-b2c-with-azure-mobile-appsazure-ad-b2c-mobile-appmd"></a>[Integrowanie usługi Azure Active Directory B2C z usługą Azure Mobile Apps](azure-ad-b2c-mobile-app.md)

Usługa Azure Active Directory B2C może służyć do zarządzania procesem uwierzytelniania usługi Azure Mobile Apps. Z tej metody możliwości zarządzania tożsamościami jest całkowicie zdefiniowane w chmurze i może być modyfikowany bez zmieniania kodu aplikacji mobilnej. W tym artykule przedstawiono sposób użycia usługi Azure Active Directory B2C, aby zapewnić uwierzytelnianie i autoryzację do wystąpienia usługi Azure Mobile Apps platformy Xamarin.Forms.

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do usług sieci Web](~/cross-platform/data-cloud/web-services/index.md)
- [Asynchroniczna pomoc techniczna — omówienie](~/cross-platform/platform/async.md)
