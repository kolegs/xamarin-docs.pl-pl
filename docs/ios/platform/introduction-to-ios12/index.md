---
title: Wprowadzenie do systemu iOS 12
description: Ten dokument zawiera ogólny opis niektórych interfejsów API systemu iOS 12, dla których Xamarin (wersja zapoznawcza) wersja zawiera powiązania C#.
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/08/2018
ms.openlocfilehash: 865a06e9fa430e195ce4ea3c6088785d9513dbf6
ms.sourcegitcommit: cfb72be633e335147d156af3ef9527151b9e31d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030707"
---
# <a name="introduction-to-ios-12"></a>Wprowadzenie do systemu iOS 12

![Wersja zapoznawcza](~/media/shared/preview.png)

> [!WARNING]
> 12 pomocy technicznej platformy Xamarin dla systemu iOS jest obecnie w wersji zapoznawczej, co oznacza, że może ona zawierać błędy, nie jest pełną funkcją, i mogą ulec zmianie. Na użytek go tylko do eksperymentowania.

> [!NOTE]
> - Przegląd [wprowadzenie](get-started.md) wskazówki, aby uzyskać instrukcje na temat zacznij tworzyć aplikacje dla systemu iOS 12 za pomocą platformy Xamarin.
> - Aby uzyskać więcej informacji, przeczytaj platformy Xamarin w wersji zapoznawczej [wersji wpis w blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-3/).

Ten dokument zawiera ogólny opis niektórych interfejsów API systemu iOS 12, dla których Xamarin (wersja zapoznawcza) wersja zawiera powiązania C#.

## <a name="arkit-2"></a>ARKit 2

ARKit to środowisko rzeczywistości rozszerzonej, dołączone do systemu iOS. ARKit 2 umożliwia wielu użytkownikom na interakcję ze sobą w scenie rzeczywistości rozszerzonej sprawia, że można utrwalić obiekty w przestrzeni i powrócić do nich w późniejszym czasie i zapewnia rozpoznawania obrazu w formacie 2D i śledzenie oraz 3D obiekt rozpoznawania. iOS 12 zapewnia również szybkie wygląd AR, możliwość renderowania usdz AR modeli w twoich aplikacjach.

## <a name="siri-shortcuts"></a>Skróty programu Siri

Używanie programu Siri skróty umożliwiają deweloperom głębiej Zintegruj swoje aplikacje za pomocą Siri. Skróty Siri użytkowników można użyć polecenia głosowe do otwierania zawartości lub uruchamiać zadania w swoich aplikacjach. Używanie programu Siri dowiesz się, gdy niektóre skróty są bardziej prawdopodobne do użycia i sugerować je do użytkownika za pośrednictwem powiadomień.

## <a name="core-ml-2"></a>Podstawowe ML 2

Core ML 2 zmniejsza rozmiar aplikacji za pomocą zaokrąglania modelu i elastyczne modele, zwiększa wydajność aplikacji przy użyciu nowego interfejsu API prognoz usługi batch i używa niestandardowych modeli do obsługi postępu w usłudze machine learning.

## <a name="notification-improvements"></a>Ulepszenia powiadomień

W systemie iOS 12 pogrupowanych powiadomienia umożliwiają obecne powiadomień użytkownika w aplikacji lub grupowanie powiązanych wątku. Tekst podsumowania może służyć w celu udostępniania dodatkowych informacji na temat grupy powiadomień.

Rozszerzenia zawartości powiadomienia w systemie iOS 12 pozwalają niestandardowych interfejsów użytkownika i akcje dynamiczne. Te funkcje umożliwiają bardziej rozbudowane, lepiej dopasowane środowisko w powiadomienia użytkownika.

## <a name="natural-language-framework"></a>Framework języka naturalnego

Struktury języka naturalnego umożliwia aplikacjom wykonywanie różnych rodzajów analizy języka. Na przykład może służyć do identyfikowania części mowy i określić język, reprezentowane przez blok tekstu.

## <a name="carplay"></a>CarPlay

W systemie iOS 12 aplikacji innych firm może dostarczać map i instrukcjami nawigacji Włącz, wyłącz w CarPlay przy użyciu nowej struktury CarPlay.

## <a name="automatic-strong-passwords"></a>Automatyczne silne hasła

iOS 12 będzie zasugerować i przechowywania nazw i silne hasła dla aplikacji zawierających ekran tworzenia konta. Sugerowane hasła mogą być generowane na podstawie korzystający z domyślnego formatu 20 znaków lub na podstawie reguł hasło określone dla deweloperów. Ta funkcja sprawia, że użycie skojarzone domeny i określone typy zawartości na nową nazwę użytkownika i nowe pola hasło.

## <a name="autofill-credential-provider-extensions"></a>Autowypełnianie poświadczenia dostawcy rozszerzeń

Z systemem iOS 12 aplikacje Menedżera haseł innych firm może zapewnić rozszerzenie podać nazwę użytkownika i hasło wartości do pól logowania.

## <a name="healthkit-updates"></a>Uprawnienie HealthKit aktualizacji

wprowadzono systemu iOS 11.3 [medyczna](https://www.apple.com/healthcare/health-records/), co pozwala użytkownikom na pobieranie ich kondycję rejestrowanie informacji z różnych instytucji ochrony zdrowia i wyświetlić je na swoich urządzeniach z systemem iOS. iOS 12 dodaje interfejsów API, które umożliwiają aplikacjom innych firm bezpieczny dostęp do tych danych.

## <a name="imessage-app-presentation-contexts"></a>iMessage kontekstów prezentacji aplikacji

W systemie iOS 12 iMessage aplikacje obsługują kontekstów prezentacji, dzięki czemu aplikacje do uruchamiania jako aplikację iMessage normalnym lub w kontekście zdjęcia lub wideo efekt.

## <a name="related-links"></a>Linki pokrewne

- [Get Ready dla systemu iOS 12 (Apple)](https://developer.apple.com/ios/)
- [12 systemu iOS w wersji zapoznawczej (Apple)](https://www.apple.com/ios/ios-12-preview/)
- Xamarin (wersja zapoznawcza) [wersji wpis w blogu](https://releases.xamarin.com/preview-release-xcode-10-beta-3/)
