---
title: Rozwiązywanie problemów
description: Ten artykuł zawiera kilka porad dotyczących rozwiązywania problemów do pracy z systemu tvOS 10 w aplikacjach Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8875e658ead17820655a2401079627875c14958b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>Rozwiązywanie problemów

_Ten artykuł zawiera kilka porad dotyczących rozwiązywania problemów do pracy z systemu tvOS 10 w aplikacjach Xamarin.tvOS._

W poniższych sekcjach wymieniono znane problemy występujące podczas korzystania z systemu tvOS 10 z Xamarin.tvOS i rozwiązanie tych problemów:

- [App Store](#App-Store)
- [Zgodność binarną](#Binary-Compatibility)
- [Protokół HTTP CFNetwork](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [CoreImage](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store" />

## <a name="app-store"></a>Sklep z aplikacjami

Znane problemy:

 - Podczas testowania zakupy w aplikacji środowisko piaskownicy, w oknie dialogowym uwierzytelniania może pojawić się dwa razy.
 - Podczas testowania zakupy w aplikacji z zawartości hostowanej w środowisko piaskownicy, wyświetli się okno dialogowe hasła za każdym razem, gdy aplikacja jest przesunięte na pierwszy plan do momentu ukończenia pobierania zawartości.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Zgodność binarną

Znane problemy:

 - Wywoływanie `NSObject.ValueForKey` będzie `null` klucza spowoduje wygenerowanie wyjątku.
 - Odwołanie do czcionki według nazwy podczas wywoływania metody `UIFont.WithName` spowoduje awarię.
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

## <a name="coreimage"></a>CoreImage

`CIImageProcessor` Interfejs API obsługuje teraz liczba dowolnego obrazu wejściowego. `CIImageProcessor` Interfejs API, który został uwzględniony w wersji beta systemu tvOS 10 1 zostaną usunięte.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

Po zakończeniu operacji programowi Handoff `UserInfo` właściwość `NSUserActivity` obiekt może być pusty. Jawnie wywołać `BecomeCurrent` NSUserActivity' obiektu jako bieżącego rozwiązania.

<a name="UIKit" />

## <a name="uikit"></a>UIKit

Znane problemy:

 - Zmienia wygląd tła `UINavigationBar`, `UITabBar` lub `UIToolBar` może spowodować w przebiegu układu rozwiązywać nowego wyglądu. Próba zmodyfikowania te wystąpienia, wewnątrz `LayoutSubviews`, `UpdateConstraints`, `WillLayoutSubviews` lub `DidUpdateSubviews` zdarzeń może spowodować w pętli nieskończonej układu.
 - W przypadku wywoływania systemu tvOS 10, `RemoveGestureRecognizer` metody `UIView` obiektu jawnie anuluje wszystkie aparat rozpoznawania gestów w toku.
 - Przedstawioną kontrolerów widoku teraz mogą wpływać na wygląd paska stanu.
 - deweloperom wywołanie wymaga systemu tvOS 10 `base.AwakeFromNib` podczas tworzenie podklas `UIViewController` i zastępowanie `AwakeFromNib` metody.
 - Aplikacje z niestandardowego `UIView` podklas, które zastępują `LayoutSubviews` i dirty układu przed wywołaniem `base.LayoutSubviews` mogą wyzwalać Pętla nieskończona układu w systemu tvOS 10.
 - Zasoby specyficzne dla kierunku lub flippable obrazów są nie Przerzucanie po przypisaniu do `UIButton` obiektów.





## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [What's new in systemu tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
