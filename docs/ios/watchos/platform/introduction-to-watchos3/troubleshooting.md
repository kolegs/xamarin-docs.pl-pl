---
title: watchOS 3 Rozwiązywanie problemów
description: Ten dokument zawiera kilka porad dotyczących rozwiązywania problemów przydatne podczas pracy z watchOS 3 w Xamarin. Wskazówki dotyczą działań, Apple Pay odświeżania w tle, NSURLConnection, ochrony prywatności i więcej.
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 0aca2c96533e17e4aeb2f57d38a87d39f700fb45
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791031"
---
# <a name="watchos-3-troubleshooting"></a>watchOS 3 Rozwiązywanie problemów

_Ten artykuł zawiera kilka porad dotyczących rozwiązywania problemów do pracy z watchOS 3 w aplikacjach Apple Watch Xamarin._

Ta strona zawiera listę znane problemy, które mogą wystąpić, gdy przy użyciu watchOS 3 dzięki platformie Xamarin i rozwiązanie tych problemów.

## <a name="activities"></a>Kategoria Activities

Działanie udostępnianie działał prawidłowo wszystkie pary obserwowanie Apple musi być uruchomiona watchOS 3.

Znane problemy:

- Czasami podczas odpowiadania na działanie udostępnianie powiadomień nie powiedzie się.
- Odpowiadanie na powiadomienie udostępnianie działanie z następującym komunikatem może zakończyć się niepowodzeniem.
- Kontekstowe tekst powyżej działania udostępnianie powiadomienie będzie nieprawidłowa.

## <a name="apple-pay"></a>Płatności firmy Apple

Znane problemy:

- Jeśli data wygaśnięcia niepoprawne lub kod efektywna jest wprowadzana dla nowego numeru płatności w Apple Pay, po naciśnięcie **dalej** uruchomionego procesu ulegnie awarii.
- Zakupy w aplikacji Apple Pay wymaganie numeru PIN może ulec awarii.

## <a name="auto-mac-unlock"></a>Odblokowywanie automatyczne Mac

Używając watchOS 3 w wersji beta 2 (lub nowszego) oraz system macOS Sierra beta 2 (lub nowszego), jeśli uwierzytelnianie dwuskładnikowe jest włączone dla użytkownika iCloud konta, za pomocą ich Apple Watch można automatycznie odblokować ich komputerów Mac.

## <a name="background-refresh"></a>Odświeżania w tle

Naruszania zasobów systemowych spowoduje awarię aplikacji watchOS 3 o następujących kodach wyjątek:

- **0xc51bad01** -aplikacji używane zbyt dużo czasu Procesora.
- **0xc51bad02** -aplikacji używane zbyt dużo czasu tablicy.
- **0xc51bad03** — aplikacja nie ma wystarczającej ilości środowiska uruchomieniowego do ukończenia bieżącego zadania.

## <a name="clock"></a>zegar

Komplikacji z nowo zainstalowane aplikacje Apple Watch może wyświetlane jako puste. Ponowny rozruch Apple Watch, aby rozwiązać ten problem.

## <a name="connectivity"></a>Łączność

Znane problemy:

- watchOS nie wyświetli monit o uprawnienia dostępu do chronionych danych przez użytkownika na Apple Watch. Udziel dostępu iPhone aplikacji przed użyciem danych w aplikacji czujki.
- Apple Watch można wprowadzić stan, w którym nie wszystkie transmisje WatchConnectivity, ponowny rozruch Apple Watch, aby rozwiązać problem.

## <a name="notifications"></a>Powiadomienia

Jeśli załącznik nośnika jest zbyt duży, zostanie wyświetlone na telefonie iPhone użytkownika, ale nie ich Apple Watch.

## <a name="nsurlconnection"></a>NSURLConnection

Wszelkie `NSURLConnection` połączeń przy użyciu starszych protokołów TLS zakończy się niepowodzeniem. W przypadku wszystkich połączeń SSL/TLS szyfrowania symetrycznego RC4 teraz jest domyślnie wyłączona. Ponadto transportu zabezpieczyć interfejs API nie obsługuje protokołów SSLv3 i zaleca się, że aplikacja zatrzymać tak szybko, jak to możliwe przy użyciu algorytmu SHA-1 i 3DES kryptografii.

Począwszy od watchOS 3 zabezpieczenia połączeń SSL/TLS jest wymuszany wyłącznie przez firmę Apple. Usługi i aplikacje należy zaktualizować serwery sieci web, aby używać najnowszej wersji protokołu TLS.

## <a name="nsurlsession"></a>NSURLSession

Począwszy od watchOS 3 `HTTPBodyStream` właściwość `NSMutableURLRequest` klasy musi mieć ustawioną nieotwarte strumienia od `NSURLConnection` i `NSURLSession` teraz ściśle wymusić to wymaganie.

## <a name="privacy"></a>Ochrona prywatności

Znane problemy:

Podczas pracy z `https://` adresy URL zarówno `NSURLSession` i `NSURLConnection` już obsługiwać mechanizmów szyfrowania RC4 podczas uzgadniania TLS. Następujące kody błędów mogą być generowane:

- **-co najmniej 1200-98** — `NSURLErrorSecurityConnectionFailed` i SecureTransport błędów.
- **-1200 [3:-9824]** -obciążenia http nie powiodło się.
- **-1200**  -  `NSURLConnection` zakończenie z błędem.

Począwszy od watchOS 3 zabezpieczenia połączeń SSL/TLS jest wymuszany wyłącznie przez firmę Apple. Usługi i aplikacje należy zaktualizować serwery sieci web, aby używać najnowszej wersji protokołu TLS. Zobacz [NSURLConnection](#NSURLConnection) powyżej Aby uzyskać więcej informacji.

## <a name="snapshots"></a>Migawki

WatchKit aplikacje, które nie przyjmuje nowych `HandelBackgroundTask` interfejsu API nie będziesz już otrzymywać okresowe aktualizacje w watchOS 3. 

## <a name="watchkit"></a>WatchKit

SpriteKit i SceneKit sceny zostanie wstrzymana, gdy aplikacja wprowadza tła w watchOS dokowania.

## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu watchOS](https://developer.xamarin.com/samples/watchos/all/)
- [What's new in watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
