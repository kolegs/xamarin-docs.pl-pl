---
title: Xamarin.Mac - macOS Sierra Rozwiązywanie problemów
description: Ten dokument zawiera kilka porad dotyczących rozwiązywania problemów do pracy z macOS Sierra w aplikacjach Xamarin.Mac. Wskazówki dotyczą Mac App Store, Apple Pay zgodności plików binarnych, CFNetwork, CloudKit i więcej.
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/22/2016
ms.openlocfilehash: 5b2571d9562fd137257e2dd0ea2ada8f071bab92
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792332"
---
# <a name="xamarinmac---macos-sierra-troubleshooting"></a>Xamarin.Mac - macOS Sierra Rozwiązywanie problemów

_Ten artykuł zawiera kilka porad dotyczących rozwiązywania problemów do pracy z macOS Sierra w aplikacjach Xamarin.Mac._

W poniższych sekcjach wymieniono znane problemy, które mogą wystąpić, gdy przy użyciu macOS Sierra Xamarin.mac i rozwiązanie tych problemów:

- [App Store](#App-Store)
- [Apple Pay](#Apple-Pay)
- [Zgodność binarną](#Binary-Compatibility)
- [Protokół HTTP CFNetwork](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Obraz Core](#CoreImage)
- [Powiadomienia](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store" />

## <a name="app-store"></a>Sklep z aplikacjami

Znane problemy:

- Podczas testowania zakupy w aplikacji środowisko piaskownicy, w oknie dialogowym uwierzytelniania może pojawić się dwa razy.
- Podczas testowania zakupy w aplikacji z zawartości hostowanej w środowisko piaskownicy, wyświetli się okno dialogowe hasła za każdym razem, gdy aplikacja jest przesunięte na pierwszy plan do momentu ukończenia pobierania zawartości.

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Płatności firmy Apple

Jeśli wygaśnięcia niepoprawna data lub zabezpieczeń (efektywna) wprowadzeniu kodu podczas dodawania nowej karty płatności do Apple Pay karty procesu udostępniania zostanie zakończony.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Zgodność binarną

Znane problemy:

- Wywoływanie `NSObject.ValueForKey` będzie `null` klucza spowoduje wygenerowanie wyjątku.
- Zarówno `NSURLSession` i NSURLConnection` no longer RC4 cipher suites during the TLS handshake for `http://' adresów URL.
- Aplikacje może zostać zawieszona na ich zmodyfikowania geometrii superview albo `ViewWillLayoutSubviews` lub `LayoutSubviews` metody.
- W przypadku wszystkich połączeń SSL/TLS szyfrowania symetrycznego RC4 teraz jest domyślnie wyłączona. Ponadto transportu zabezpieczyć interfejs API nie obsługuje protokołów SSLv3 i zaleca się, że aplikacja zatrzymać tak szybko, jak to możliwe przy użyciu algorytmu SHA-1 i 3DES kryptografii.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>Protokół HTTP CFNetwork

`HTTPBodyStream` Właściwość `NSMutableURLRequest` klasy musi mieć ustawioną nieotwarte strumienia od `NSURLConnection` i `NSURLSession` teraz ściśle wymusić to wymaganie.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Długotrwałe operacje zwróci _"Nie ma uprawnień do zapisania pliku."_ Wystąpił błąd.

<a name="CoreImage" />

## <a name="core-image"></a>Obraz Core

`CIImageProcessor` Interfejs API obsługuje teraz liczba dowolnego obrazu wejściowego. `CIImageProcessor` Interfejs API, który został uwzględniony w macOS Sierra beta 1 zostaną usunięte.

<a name="Notifications" />

## <a name="notifications"></a>Powiadomienia

Podczas pracy z rozszerzeniami zawartości powiadomienia, kontrolery widoku nie jest poprawnie udostępnieniu i może spowodować awarię, po osiągnięciu limitu pamięci rozszerzenia.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

Po zakończeniu operacji programowi Handoff `UserInfo` właściwość `NSUserActivity` obiekt może być pusty. Jawnie wywołać `BecomeCurrent` `NSUserActivity` obiektu jako bieżącego rozwiązania.

<a name="Safari" />

## <a name="safari"></a>Safari

WebGeolocation wymaga bezpiecznego (`https://`) adres URL do pracy na iOS 10 i macOS Sierra, aby uniemożliwić złośliwym korzystaniem z danych lokalizacji.







## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla komputerów Mac](https://developer.xamarin.com/samples/mac/)
- [What's new in 10.12 X systemu operacyjnego](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
