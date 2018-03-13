---
title: Podstawy aplikacji
description: Podstawowe koncepcje aplikacji
ms.topic: article
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/21/2017
ms.openlocfilehash: f6c0bb75327f0c14d314a0e7ad9d114fd17c2a57
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="application-fundamentals"></a>Podstawy aplikacji

Ta sekcja zawiera przewodnik dla niektórych zadań rzeczy częściej lub pojęcia deweloperzy muszą znać podczas tworzenia aplikacji platformy Xamarin.iOS (dawniej MonoTouch).

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[Zabezpieczenia transportu aplikacji](~/ios/app-fundamentals/ats.md)

W tym artykule przedstawiono zmiany zabezpieczeń, które zabezpieczeń transportu aplikacji wymusza na aplikację systemu iOS 9 i jego znaczenia dla projektów Xamarin.iOS i opisano opcje konfiguracji ATS omówiono sposób Wypisz ATS, jeśli jest to wymagane. Ponieważ ATS jest domyślnie włączona, wszystkie niezabezpieczonego połączenia internetowe zgłosi wyjątek w aplikacji systemu iOS 9 (chyba że jawnie już dozwolone).


## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Uruchamianie procesów w tle](~/ios/app-fundamentals/backgrounding/index.md)

Tło przetwarzania lub backgrounding jest proces umożliwienie aplikacji wykonać zadania w tle, gdy inna aplikacja jest uruchomiona na pierwszym planie. W tym przewodniku stanowi wprowadzenie do przetwarzania w systemie iOS w tle.


## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[Zdarzenia, protokołów i Delegaty](~/ios/app-fundamentals/delegates-protocols-and-events.md)

Ten artykuł przedstawia informacje o technologii klucza iOS używany do odbierania wywołania zwrotne i wypełnianie formantów interfejsu użytkownika z danymi. Technologie te są zdarzenia, protokołów i obiektów delegowanych; w tym artykule opisano, co każdego z nich jest i jak są używane w języku C#. Pokazuje, jak Xamarin.iOS używa kontrolki z systemem iOS do udostępnienia znanych zdarzenia platformy .NET, jak również, jak Xamarin.iOS zapewnia obsługę pojęcia języka Objective-C, takie jak protokoły i delegatów (Objective-C delegatów nie należy mylić z delegatów C#). Ten artykuł zawiera również przykłady, które pokazują, jak protokoły są używane zarówno jako podstawa dla języka Objective-C delegatów i w scenariuszach niebędącego delegatem.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[Wątkowość](~/ios/app-fundamentals/threading.md)

W tym artykule omówiono wątków w aplikacji platformy Xamarin.iOS i nieco zawiera informacje o puli wątków .NET, reakcji aplikacji i wyrzucanie elementów bezużytecznych.&nbsp;

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md)

W tym artykule sprawdza sposób używania obrazów w Xamarin.iOS, zarówno obrazów do obsługi aplikacji (takich jak ikony, ładowania obrazów, itp.), jak i obrazów w aplikacji (takich jak obrazy stosowana do formantów). Obejmuje ona również, jak dołączyć do nich obrazów za pomocą programu Visual Studio dla komputerów Mac, a także sposoby interakcji z obrazami z kodu.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[Praca z listy właściwości](~/ios/app-fundamentals/index.md)

Tym dokumencie przedstawiono Visual Studio dla edytora listy (.plist) graficznego i zaawansowane właściwości dla komputerów Mac do pracy z Info.plist i Entitlements.plist. Zastosowano ustawienie ikon i uruchamiania obrazy dla aplikacji systemu iOS i pokazuje możliwości określania aplikacji (uprawnień) z w programie Visual Studio dla komputerów Mac.

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[Praca w systemie plików](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS można użyć tej samej klasy System.IO do pracy z plików i katalogów w systemie iOS, który ma zostać użyty w dowolnej aplikacji .NET. Pomimo znanych klasy i metody, iOS implementuje pewne ograniczenia na plikach, które można utworzyć lub uzyskać dostępu do i udostępnia funkcje specjalne określonych katalogów. W tym artykule opisano te ograniczenia i funkcje oraz pokazano, jak działa dostęp do plików w aplikacji platformy Xamarin.iOS.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[Tworzenie aplikacji systemu iOS w kodzie](~/ios/app-fundamentals/ios-code-only.md)

W tym artykule sprawdza sposób tworzenia aplikacji systemu iOS w całości kodu przy użyciu programu Visual Studio i Visual Studio dla komputerów Mac. Widoczny jest sposób uruchomienia z pusty szablon projektu do tworzenia przez utworzenie hierarchii widoków z UIKit ekran aplikacji w kontrolerze. Następnie zawarto informacje, jak tworzyć widoki niestandardowe, które mogą być ładowane w kontrolerze.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[Praca z domyślnych ustawień użytkownika](~/ios/app-fundamentals/user-defaults.md)

`NSUserDefaults` Klasa udostępnia sposób dla systemu iOS aplikacje i rozszerzenia programowo interakcję z systemowe domyślny System. Przy użyciu ustawienia domyślne systemu, użytkownik może konfigurować zachowanie aplikacji lub style w celu spełnienia preferencji (oparte na projekt aplikacji). Na przykład, aby przedstawiać dane w programie vs metryki angielskie pomiary, lub wybierz motyw danego interfejsu użytkownika.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[Dotyk](~/ios/app-fundamentals/touch/index.md)

Ekranów dotykowych na wielu urządzeniach współczesnych Zezwalaj użytkownikom na interakcję z urządzeniami, szybkie i skuteczne jak fizyczną i intuicyjne. Ta interakcja nie jest ograniczona tylko w celu wykrywania touch proste — można również gesty. Na przykład gestu powiększanie gestem uszczypnięcia jest bardzo typowym przykładem tego — punkty zaciskające części ekranu z dwoma palcami, które użytkownik może powiększanie lub pomniejszanie. W tym przewodniku sprawdza touch i gestów w systemie iOS.

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[Praca z zabezpieczenia i prywatność](~/ios/app-fundamentals/security-privacy.md)

Apple wprowadziła kilka ulepszeń zarówno zabezpieczeń i prywatności w systemie iOS 10 (i nowsze), ułatwiające developer poprawy zabezpieczeń aplikacji i zapewnienia zachowania poufności użytkownika końcowego. W tym artykule opisano wdrażanie tych funkcji w aplikacji platformy Xamarin.iOS.

##  <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[Lokalizacja](~/ios/app-fundamentals/localization/index.md)

Ten przewodnik obejmuje Dodawanie kodowań do aplikacji platformy Xamarin.iOS w celu obsługi różnych języków.