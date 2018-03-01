---
title: "Licencjonowanie usług Google"
ms.topic: article
ms.prod: xamarin
ms.assetid: E96BDCC3-454A-A797-5819-905E2BB1AC41
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/20/2017
ms.openlocfilehash: f1e7e36dfa1bfe122084f0525d83f06760ca1fe0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="google-licensing-services"></a>Licencjonowanie usług Google

Przed Google Play aplikacje systemu Android zależał od starsza wersja, którą kopiowania zapewniany przez rynku Google, aby upewnić się, tylko autoryzowanym użytkownikom można uruchamiać aplikacje na urządzeniach. Ograniczenia mechanizm ochrony kopiowania wprowadzone mniej niż idealne rozwiązanie do ochrony aplikacji.

Google licencjonowania nie zastępuje to starszych mechanizm ochrony kopiowania.
Licencjonowanie Google jest usługą elastyczne, bezpieczne, opartych na sieci, która aplikacji systemu Android mogą zbadać w celu określenia, czy aplikacja jest licencjonowana do uruchomienia na danym urządzeniu.

Licencjonowanie Google jest elastyczne, w tym aplikacji systemu Android mają pełną kontrolę nad czas wyszukiwania licencji, jak często, aby sprawdzić licencji i sposób obsługi odpowiedzi z serwera licencji.

Licencjonowanie Google jest bezpieczne, w tym każdego odpowiedzi jest podpisany przy użyciu pary kluczy RSA użytkowany wyłącznie między serwerem Google Play i aplikacji. Google Play zawiera klucz publiczny dla deweloperów osadzone w aplikacji systemu Android, które służy do uwierzytelniania odpowiedzi. Serwer Google Play wewnętrznie przechowuje klucz prywatny.

Aplikacja, która została zaimplementowana licencjonowania Google wysyła żądanie do usługi hostowanej przez aplikację ze sklepu Google Play na urządzeniu. Google Play wysyła następnie żądanie do serwera licencji Google, która odpowiada ze stanem licencji: 

[ ![Diagram przepływu pracy serwera licencjonowania](google-licensing-services-images/gp-licensing-service-overview.png)](google-licensing-services-images/gp-licensing-service-overview.png)

Na powyższym diagramie przedstawiono tego przepływu pracy: 

-   Aplikacja zawiera nazwę pakietu, *identyfikator jednorazowy* (kryptograficznych uwierzytelnienia) używany do weryfikowania odpowiedzi serwera i wywołanie zwrotne, które może obsłużyć odpowiedzi asynchronicznie. 

-   Google Play zawiera informacje, takie jak konto Google i urządzenia, takie jak numer firmy IMSI. 

Usługa licencjonowania Google również jest kluczowym elementem APK rozszerzenia plików, (które zostały omówione w dalszej części tego dokumentu). Pliki rozszerzeń APK wykorzystują Google licencjonowania usługi można uzyskać adresy URL plików rozszerzeń, które zostaną pobrane.

<a name="Requirements" />

## <a name="requirements"></a>Wymagania

Aplikacje, które nie zostały zakupione w ramach Google Play zostanie nie skorzystają z usług Google licencjonowania. Jeżeli Google Play nie jest zainstalowany na urządzeniu, aplikacje używające usług licencjonowania będą nadal działać normalnie na tym urządzeniu.

Google Play wymaga dostępu do Internetu dla funkcji. Aplikacja może buforować licencji w przypadku scenariuszy, w którym urządzenie nie ma dostępu do serwerów firmy Google Play licencjonowania.

Bezpłatne aplikacje wymagane tylko licencjonowania Google Jeśli aplikacja używa APK rozszerzeń plików.
