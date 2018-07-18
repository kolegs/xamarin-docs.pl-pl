---
title: Podstawy aplikacji platformy Xamarin.iOS
description: Ten dokument prowadzi różne przewodniki z instrukcjami, które opisują koncepcje podstawowe do tworzenia aplikacji platformy Xamarin.iOS, takie jak zabezpieczenia transportu aplikacji, uruchamianie procesów w tle, zdarzeń i wątkowości.
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/21/2017
ms.openlocfilehash: de337291554e81a2434dcc30c163f4789fc832eb
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111215"
---
# <a name="xamarinios-application-fundamentals"></a>Podstawy aplikacji platformy Xamarin.iOS

Ta sekcja zawiera wskazówki na niektóre typowe zadania rzeczy lub pojęcia, które deweloperzy muszą znać podczas opracowywania aplikacji platformy Xamarin.iOS (dawniej MonoTouch).

## <a name="accessibilityiosapp-fundamentalsaccessibilitymd"></a>[Ułatwienia dostępu](~/ios/app-fundamentals/accessibility.md)

W tym dokumencie opisano różne interfejsy API i narzędzia, które mogą służyć do pomagające w tworzeniu aplikacji, które są dostępne do wybranej liczby użytkowników, jak to możliwe.

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[Zabezpieczenia transportu aplikacji](~/ios/app-fundamentals/ats.md)

W tym artykule przedstawiono zmiany zabezpieczeń, które App Transport Security wymusza na aplikacji dla systemu iOS 9 i co to oznacza dla Twoich projektów platformy Xamarin.iOS i pokrycie ATS opcje konfiguracji opisano jak zrezygnować z ATS, jeśli jest to wymagane. Ponieważ ATS jest domyślnie włączone, wszystkie połączenia internetowe — zabezpieczanie zgłosi wyjątek w aplikacjach systemu iOS 9 (chyba że jawnie już dozwolone).

## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Uruchamianie procesów w tle](~/ios/app-fundamentals/backgrounding/index.md)

Tło przetwarzania lub uruchamianie procesów w tle polega na umożliwieniu aplikacji wykonywania zadań w tle, gdy inna aplikacja jest uruchomiona na pierwszym planie. Ten przewodnik stanowi wprowadzenie do przetwarzania w systemie iOS w tle.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[Tworzenie aplikacji systemu iOS w kodzie](~/ios/app-fundamentals/ios-code-only.md)

W tym artykule zbadamy, jak tworzyć aplikacje dla systemu iOS w całości kodu przy użyciu programu Visual Studio i Visual Studio dla komputerów Mac. Widoczny jest sposób rozpocząć od pusty szablon projektu służący do tworzenia ekranu aplikacji w kontrolerze, tworząc hierarchię widoków z UIKit. Następnie omówiono sposób tworzenia widoków niestandardowych, które mogą być ładowane w kontrolerze.

## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[Zdarzenia, protokoły i delegaci](~/ios/app-fundamentals/delegates-protocols-and-events.md)

W tym artykule przedstawiono technologie klucza dla systemu iOS, odbierać wywołania zwrotne i wypełnianie kontrolek interfejsu użytkownika z danymi. Technologie te są zdarzenia, protokoły i delegaci; w tym artykule wyjaśniono czym każdy z nich jest i jak każdy obiekt jest używany z kodu C#. Pokazuje, jak Xamarin.iOS używa formantów z systemem iOS w celu udostępnienia znanych .NET zdarzeń, a także, jak Xamarin.iOS zapewnia obsługę języka Objective-C pojęć, takich jak protokoły i delegaci (delegatów języka Objective-C nie należy mylić z delegatów w języku C#). Ten artykuł zawiera także przykłady pokazujące, jak protokoły są używane zarówno jako podstawy dla obiektów delegowanych języka Objective-C, jak i w scenariuszach Niedelegowany.

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[Praca w systemie plików](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS można użyć tych samych klas System.IO do pracy z plikami i katalogami w systemie iOS, które byłyby używane w dowolnej aplikacji platformy .NET. Niezależnie od znanych klasy i metody, z systemem iOS implementuje pewne ograniczenia na pliki, które mogą być tworzone lub uzyskać dostępu do i udostępnia także specjalne funkcje dla niektórych katalogów. W tym artykule opisano te ograniczenia i funkcje i pokazuje, jak działa dostęp do plików aplikacji platformy Xamarin.iOS.

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md)

W tym artykule zbadamy, jak używać obrazów w rozszerzeniu Xamarin.iOS zarówno obrazy support aplikacji (takich jak ikony, ładowania obrazów, itp.), jak i obrazów w aplikacji (na przykład zastosować do kontrolek obrazów). Obejmuje ona również, jak dołączać obrazy za pomocą programu Visual Studio dla komputerów Mac, a także sposoby interakcji z obrazami z kodu.

## <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[Lokalizacja](~/ios/app-fundamentals/localization/index.md)

Ten przewodnik obejmuje dodanie kodowania do aplikacji platformy Xamarin.iOS w celu obsługi różnych języków.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[Praca z listy właściwości](~/ios/app-fundamentals/index.md)

Ten dokument stanowi wprowadzenie programu Visual Studio dla komputerów Mac graficznego i zaawansowane właściwości listy (.plist) edytora do pracy z pliku Info.plist i plik Entitlements.plist. Zawiera ono Ustawienia ikon i uruchamiania obrazy dla aplikacji dla systemu iOS i pokazuje Określanie możliwości aplikacji (uprawnień) z poziomu programu Visual Studio dla komputerów Mac.

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[Praca z zabezpieczeń i prywatności](~/ios/app-fundamentals/security-privacy.md)

Apple wprowadziła kilka dodatkowych funkcji zabezpieczeń i prywatności w systemie iOS 10 (lub nowszym), które pomogą developer zwiększające bezpieczeństwo swoich aplikacji i upewnij się, ochrony prywatności użytkowników końcowych. W tym artykule opisano wdrażanie tych funkcji w aplikacji platformy Xamarin.iOS.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[Wątkowość](~/ios/app-fundamentals/threading.md)

W tym artykule omówiono wątków w aplikacji platformy Xamarin.iOS i nieco zawiera informacje o puli wątków .NET, czasu reakcji aplikacji i wyrzucania elementów bezużytecznych.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[Dotyk](~/ios/app-fundamentals/touch/index.md)

Ekranów dotykowych na wielu urządzeniach współczesnych umożliwiają szybkie i skuteczne wchodzić w interakcje z urządzeniami w sposób naturalny i intuicyjne. Ta interakcja nie jest ograniczona tylko do wykrywania touch proste — można użyć także gestów. Na przykład gestu powiększanie gestem uszczypnięcia jest bardzo typowy przykład to — uszczypnięć część ekranu przy użyciu dwóch palców, które użytkownik może powiększanie lub pomniejszanie. Ten przewodnik sprawdza, czy Dotyk i gesty w systemie iOS.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[Praca z domyślne ustawienia użytkownika](~/ios/app-fundamentals/user-defaults.md)

`NSUserDefaults` Klasy zapewnia sposób dla systemu iOS, aplikacji i rozszerzeń, aby programowo współdziałać z systemu domyślne systemowe. Za pomocą systemu ustawienia domyślne, użytkownik może konfigurować zachowanie aplikacji lub style w celu spełnienia ich Preferencje (oparte na projekt aplikacji). Na przykład, aby przedstawiać dane w vs metryki Imperialny pomiary, lub wybierz danego motyw interfejsu użytkownika.
