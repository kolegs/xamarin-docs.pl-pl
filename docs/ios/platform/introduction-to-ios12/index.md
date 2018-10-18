---
title: Wprowadzenie do systemu iOS 12
description: Ten dokument zawiera ogólny opis niektórych interfejsów API systemu iOS 12, dla których Xamarin (wersja zapoznawcza) wersja zawiera powiązania C#.
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/08/2018
ms.openlocfilehash: 81375e8c66e5504604d0d4cb3be34afd58f4269d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615151"
---
# <a name="introduction-to-ios-12"></a>Wprowadzenie do systemu iOS 12

Ten dokument zawiera ogólny opis niektórych interfejsów API systemu iOS 12, dla których Xamarin (wersja zapoznawcza) wersja zawiera powiązania C#.

Aby rozpocząć tworzenie aplikacji dla systemu iOS 12 za pomocą platformy Xamarin, zobacz [— wprowadzenie](get-started.md)

## <a name="arkit-2arkit2md"></a>[ARKit 2](arkit2.md)

ARKit to środowisko rzeczywistości rozszerzonej, dołączone do systemu iOS. ARKit 2 umożliwia wielu użytkownikom na interakcję ze sobą w scenie rzeczywistości rozszerzonej sprawia, że można utrwalić obiekty w przestrzeni i powrócić do nich w późniejszym czasie i zapewnia rozpoznawania obrazu w formacie 2D i śledzenie oraz 3D obiekt rozpoznawania. iOS 12 zapewnia również szybkie wygląd AR, możliwość renderowania usdz AR modeli w twoich aplikacjach.

## <a name="siri-shortcutssiri-shortcutsmd"></a>[Skróty programu Siri](siri-shortcuts.md)

Używanie programu Siri skróty umożliwiają deweloperom głębiej Zintegruj swoje aplikacje za pomocą Siri. Skróty Siri użytkowników za pomocą poleceń głosowych do otwierania zawartości lub inicjowania zadań w tle lub inicjują oni te same czynności, za pomocą skrótów, które Siri sugeruje na ekranie blokady.

## <a name="core-ml-2coremlmd"></a>[Podstawowe ML 2](coreml.md)

Core ML 2 zmniejsza rozmiar aplikacji za pomocą zaokrąglania modelu i elastyczne modele, zwiększa wydajność aplikacji przy użyciu nowego interfejsu API prognoz usługi batch i używa niestandardowych modeli do obsługi postępu w usłudze machine learning.

## <a name="notification-improvementsnotificationsindexmd"></a>[Ulepszenia powiadomień](notifications/index.md)

W systemie iOS 12 pogrupowanych powiadomienia umożliwiają obecne powiadomień użytkownika w aplikacji lub grupowanie powiązanych wątku. Tekst podsumowania zawiera dalsze informacje na temat grupy powiadomień.

Rozszerzenia zawartości powiadomienia w systemie iOS 12 pozwalają niestandardowych interfejsów użytkownika i dynamiczna akcji przycisków.

## <a name="natural-language-frameworknatural-languagemd"></a>[Framework języka naturalnego](natural-language.md)

Struktury języka naturalnego umożliwia aplikacjom wykonywanie różnych rodzajów analizy języka. Na przykład go identyfikować części mowy i określić język, reprezentowane przez blok tekstu.

## <a name="vision-framework"></a>Struktury Vision

Struktury Vision obejmuje wykrywanie twarzy ulepszone, który może wykrywać twarze w różnych położeniach. Ponadto żądania zmiany można wybrać określoną poprawkę algorytm framework wizji.

## <a name="photo-and-video-apis"></a>Zdjęcia i interfejsów API klipów wideo

W systemie iOS 12 segmentacji pionowa interfejsu API zwraca otoczki efekty pionowa — liniowy maski wyznacza na pierwszym planie z tło obrazu pionowa, która jest przydatne przy tworzeniu różne efekty obrazu. iOS 12 daje ponadto możliwość użycia dane głębokości z aparatu TrueDepth dla efekty wideo w czasie rzeczywistym.

## <a name="passwords"></a>Hasła

iOS 12 ułatwia użytkownikom i deweloperom pracę przy użyciu haseł:

- Automatycznego wypełniania haseł i automatyczne silne hasła umożliwiać do automatycznego generowania, przechowywania i używać silnych haseł w aplikacjach systemu iOS podczas rejestracji w usłudze i logowania do aplikacji.
- Automatyczne uzupełnianie kodu zabezpieczeń umożliwia uwierzytelnianie za pomocą wiadomości SMS kody bez konieczności ręcznego wycinanie i wklejanie lub zapamiętywania.
- `ASWebAuthenticationSession` Klasa upraszcza proces pracy z usługami uwierzytelniania federacyjnego.
- Dostawca poświadczeń automatyczne uzupełnianie rozszerzenia umożliwiają aplikacji innych firm hasło o podanie nazwy użytkownika i hasła do pól logowania.

## <a name="healthkit-updates"></a>Uprawnienie HealthKit aktualizacji

wprowadzono systemu iOS 11.3 [medyczna](https://www.apple.com/healthcare/health-records/), co pozwala użytkownikom na pobieranie ich kondycję rejestrowanie informacji z różnych instytucji ochrony zdrowia i wyświetlić je na swoich urządzeniach z systemem iOS. iOS 12 dodaje interfejsów API, które umożliwiają aplikacjom innych firm bezpieczny dostęp do tych danych.

## <a name="imessage-app-presentation-contexts"></a>iMessage kontekstów prezentacji aplikacji

W systemie iOS 12 iMessage aplikacje obsługują kontekstów prezentacji, dzięki czemu aplikacje do uruchamiania jako aplikację iMessage normalnym lub w kontekście zdjęcia lub wideo efekt.

## <a name="network-framework"></a>Struktura sieci

Podstawowy stos sieci struktura sieci `URLSession` interfejsów API w powszechnie stosowana w aplikacjach systemu iOS jest teraz dostępny jako framework autonomicznych, dzięki czemu łatwiej jest pracować z protokołu TCP, UDP, TLS i IPv4 i IPv6.

## <a name="carplay"></a>CarPlay

W systemie iOS 12 aplikacji innych firm może dostarczać map i instrukcjami nawigacji Włącz, wyłącz w CarPlay przy użyciu nowej struktury CarPlay.

## <a name="deprecations"></a>Deprecations

Z systemem iOS 12 Apple jest przestarzała:

- OpenGL ES [zachęcanie do deweloperów](https://developer.apple.com/ios/whats-new/) podczas wdrażania systemu operacyjnego.
- [`UIWebView`](https://developer.xamarin.com/api/type/UIKit.UIWebView/), [uzyskać `WKWebView` ](https://developer.apple.com/documentation/webkit/wkwebview?language=objc).

## <a name="related-links"></a>Linki pokrewne

- [Get Ready dla systemu iOS 12 (Apple)](https://developer.apple.com/ios/)
