---
title: Publikowanie do sklepu z aplikacjami
description: "Ten przewodnik przeprowadzi Cię przez wdrażanie aplikacji Xamarin.Mac, za pomocą programu Visual Studio dla komputerów Mac. Go wyjaśniono, jak skonfigurować konta dewelopera Mac, przeprowadzi Cię przez proces tworzenia certyfikatów do podpisywania kodu i pokazuje, jak ich używać do tworzenia aplikacji Mac, które mogą być dystrybuowane bezpośrednio lub za pośrednictwem sklepu Mac App Store."
ms.topic: article
ms.prod: xamarin
ms.assetid: D26C5E54-EAD2-5487-264D-4263AEA1EBF2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 0846dc8bdb3ac722838faa4173f5d30233912ecc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="publishing-to-the-app-store"></a>Publikowanie do sklepu z aplikacjami

_Ten przewodnik przeprowadzi Cię przez wdrażanie aplikacji Xamarin.Mac, za pomocą programu Visual Studio dla komputerów Mac. Go wyjaśniono, jak skonfigurować konta dewelopera Mac, przeprowadzi Cię przez proces tworzenia certyfikatów do podpisywania kodu i pokazuje, jak ich używać do tworzenia aplikacji Mac, które mogą być dystrybuowane bezpośrednio lub za pośrednictwem sklepu Mac App Store._

## <a name="overview"></a>Omówienie

Xamarin.Mac aplikacje mogą być dystrybuowane na dwa sposoby:

- **Identyfikator dewelopera** — aplikacje podpisane z mogą Identyfikator dewelopera przesyłane poza sklepu z aplikacjami, ale są rozpoznawane przez strażnika i mogą instalować.
- **Mac App Store** — aplikacje muszą mieć pakiet Instalatora i Instalator i aplikacji muszą być podpisane, w celu przesłania do sklepu Mac App Store.

Tym dokumencie wyjaśniono, jak używać programu Visual Studio dla systemów Mac i Xcode, aby skonfigurować konto deweloperów firmy Apple i skonfigurować Xamarin.Mac projektu dla każdego typu wdrożenia.


## <a name="mac-developer-program"></a>Mac programie dla deweloperów

Po dołączeniu do [programie dla deweloperów Mac](https://developer.apple.com/devcenter/mac/) deweloperowi zostaną zaoferowane możliwość dołączenia jako osoba lub firmy, jak pokazano na poniższym zrzucie ekranu:

[![Portalu dla deweloperów firmy Apple](images/image1.png "portalu dla deweloperów firmy Apple")](images/image1-large.png)

Wybierz typ rejestracji poprawne dla danej sytuacji.

> [!NOTE]
> **Uwaga**: wybory dokonane w tym miejscu będzie miało wpływ na sposób niektóre ekrany są wyświetlane podczas konfigurowania konta dewelopera. Opisy i zrzuty ekranu w tym dokumencie są wykonywane z punktu widzenia **poszczególnych** konta dewelopera. W **firmy**, niektóre opcje tylko będą dostępne do **administrator Team** użytkowników.


### <a name="certificates-and-identifiersmacdeploy-testpublishing-to-the-app-storecertificates-identifiersmd"></a>[Certyfikaty i identyfikatory](~/mac/deploy-test/publishing-to-the-app-store/certificates-identifiers.md)

Ten przewodnik przeprowadzi Cię przez proces tworzenia wymagane certyfikaty i identyfikatorów, które będą wymagane do publikowania aplikacji Xamarin.Mac.


### <a name="create-provisioning-profilemacdeploy-testpublishing-to-the-app-storeprofilesmd"></a>[Utwórz profil inicjowania obsługi administracyjnej](~/mac/deploy-test/publishing-to-the-app-store/profiles.md)

Ten przewodnik przeprowadzi Cię przez proces tworzenia niezbędne profile inicjowania obsługi, które będą wymagane do publikowania aplikacji Xamarin.Mac.


### <a name="mac-app-configurationmacdeploy-testpublishing-to-the-app-storeapp-configurationmd"></a>[Konfiguracja aplikacji dla komputerów Mac](~/mac/deploy-test/publishing-to-the-app-store/app-configuration.md)

Ten przewodnik przeprowadzi Cię przez kolejne etapy konfigurowania aplikacji przez Xamarin.Mac dla publikacji.


### <a name="sign-with-developer-idmacdeploy-testpublishing-to-the-app-storesigningmd"></a>[Podpisywanie za pomocą identyfikatora dewelopera](~/mac/deploy-test/publishing-to-the-app-store/signing.md)

Ten przewodnik przeprowadzi Cię przez podpisywania aplikacji Xamarin.Mac, identyfikatorem Developer dla publikacji.


### <a name="bundle-for-mac-app-storemacdeploy-testpublishing-to-the-app-storebundlingmd"></a>[Pakiet dla sklepu Mac App Store](~/mac/deploy-test/publishing-to-the-app-store/bundling.md)

Ten przewodnik przeprowadzi Cię przez funkcję tworzenia pakietów aplikacji Xamarin.Mac dla publikacji do sklepu Mac App Store.


### <a name="upload-to-mac-app-storemacdeploy-testpublishing-to-the-app-storeuploadingmd"></a>[Przekazywanie do sklepu Mac App Store](~/mac/deploy-test/publishing-to-the-app-store/uploading.md)

Ten przewodnik przeprowadzi Cię przez przekazywania do sklepu Mac App Store aplikacji Xamarin.Mac dla publikacji.


## <a name="related-links"></a>Linki pokrewne

- [Instalacja](/visualstudio/mac/installation/)
- [Witaj, przykładowe Mac](~/mac/get-started/hello-mac.md)
- [Identyfikator deweloperów i strażnika](https://developer.apple.com/resources/developer-id/)
