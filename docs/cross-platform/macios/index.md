---
title: iOS i Mac
description: "W tej sekcji możemy obejmuje strategii współużytkowanie kodu platformy Xamarin.iOS i Xamarin.Mac projektów."
ms.topic: article
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 1491e6ec36a9ced9460e083769b2148386d1d518
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="ios-and-mac"></a>iOS i Mac

_W tej sekcji możemy obejmuje strategii współużytkowanie kodu platformy Xamarin.iOS i Xamarin.Mac projektów._

## <a name="code-sharing"></a>Udostępnianie kodu

Dla elementów kodu, które mają żadnych elementów interfejsu użytkownika w najlepszy sposób, aby udostępnić kod między systemów iOS i Mac jest nadal używana [przenośnej biblioteki klas](~/cross-platform/app-fundamentals/pcl.md).

Kod, który ma wykonania dodatkowych czynności interfejsu użytkownika i jeszcze chcesz udostępnić, należy użyć [udostępnionych projektów](~/cross-platform/app-fundamentals/shared-projects.md) umożliwiają umieść kod, aby udostępnić w jednym projekcie i został skompilowany z komputerów Mac i z systemem iOS, gdy odwołuje się do.

##  <a name="unified-apiunifiedindexmd"></a>[Ujednolicony interfejs API](unified/index.md)

Ujednolicone interfejsu API dla systemów iOS i Mac projektów używa tej samej przestrzeni nazw dla struktur, tym samym pliku kodu może służyć zarówno platformach do łatwego udostępniania kodu. Umożliwia również zarówno 32- i 64 bitowej kompilacji. Interfejsu API Unified został domyślnego szablonu od początku 2015 i jest zalecany dla wszystkich nowych projektów - *tylko* projektów Unified API może zostać przesłane do sklepu z aplikacjami.

### <a name="classic-apis"></a>Interfejsy API klasycznego

> [!NOTE]
> **Amortyzacja klasycznego profilu:** miarę dodawania nowych platform w Xamarin.iOS zostanie użyty stopniowo zastąpić funkcji z klasycznym profilu (monotouch.dll). Na przykład opcja z systemem innym niż NRC (liczba nowych ref) została usunięta. NRC zawsze została włączona dla wszystkich ujednoliconego aplikacji (tj. z systemem innym niż NRC nigdy nie był opcję) i ma nie znanych problemów. Przyszłych wydaniach usunie opcję użycia Boehm jako moduł garbage collector. Było również opcję Nigdy nie są dostępne do ujednoliconego aplikacji. Całkowite usunięcie klasycznego Obsługa zaplanowano spadek 2016 w wersji 10.0 platformy Xamarin.iOS.

API Xamarin.Mac i Xamarin.iOS (z systemem innym niż Unified) oryginalnego wprowadzone udostępniania kodu trudniejsze ponieważ struktury natywnego albo `MonoTouch.` lub `MonoMac.` prefiksy przestrzeni nazw.  Podaliśmy pusty przestrzeni nazw, który umożliwia deweloperom udostępnianie kodu dodając `using` instrukcje, które odwołują się oba MonoMac MonoTouch obszary nazw i z tego samego pliku, ale został nieco fałszywych. Klasycznego interfejsu API tylko powinno być kontynuowane do użycia w starszej wersji aplikacji, które są dystrybuowane wewnętrznie (uaktualnianie do interfejsu API Unified jest zalecane).


### <a name="updating-from-classic-to-the-unified-api"></a>Aktualizowanie ze środowiska klasycznego do ujednoliconego interfejsu API

Brak szczegółowych instrukcji dotyczących dowolnej aplikacji z klasycznego do interfejsu API Unified aktualizacji.

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Tworzenie powiązań bibliotek języka Objective-C](binding/index.md)

Xamarin pozwala dostosować natywnych bibliotek do aplikacji z powiązaniami. W tej sekcji opisano:

- jak działają powiązania
- jak ręcznie utworzyć projekt powiązania, który pozwala wprowadzić kod języka Objective-C do platformy Xamarin, i
- jak używać naszych **Sharpie cel** narzędzie w celu automatyzacji procesu.

## <a name="native-referencesnative-referencesmd"></a>[Odwołania natywne](native-references.md)



##  <a name="macios-native-typesnativetypesmd"></a>[Typy natywne Mac/z systemem iOS](nativetypes.md)

Aby obsługiwać 32 i 64-bitowy kod przezroczysty z C# i F #, wprowadzono nowe typy danych.   Dowiedz się więcej o je tutaj.

##  <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[Tworzenie 32- i 64 bitowej aplikacji](32-and-64/index.md)

Co należy wiedzieć, do obsługi 32- i 64 bitowych aplikacji.

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[Praca z typami natywnymi w aplikacjach międzyplatformowych](native-types-cross-platform.md)

W tym artykule omówiono za pomocą nowego systemu iOS Unified API natywnych typów (`nint`, `nuint`, `nfloat`) w aplikacji i platform, gdy kod jest współużytkowany z urządzeń z systemem innym niż z systemem iOS, takich jak Android i Windows Phone w systemach operacyjnych.
Zapewnia wgląd w stosowania natywnych typów, a udostępnia kilka możliwych rozwiązań przypadkach gdy nowy typ musi być stosowana z kodem i platform.


## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[Stos HttpClient i selektor implementacji protokołów SSL/TLS](http-stack.md)

Selektor stosu HttpClient steruje życie HttpClient do użycia w aplikacji platformy Xamarin.iOS, Xamarin.tvOS i Xamarin.Mac. Teraz można przełączyć do implementację, która używa dla systemu iOS, systemu tvOS firmy lub transportów natywnego znaków OS x (`NSUrlSession` lub `CFNetwork` w zależności od systemu operacyjnego).

Protokół SSL (Secure Socket Layer) i jej następcą, TLS (Transport Layer Security), zapewniają obsługę dla protokołu HTTP i innych połączeń sieciowych za pośrednictwem `System.Net.Security.SslStream`. Nowa opcja kompilacji implementacji protokołów SSL/TLS zmienia między własnych Mono stosu protokołu TLS, a obsługiwane przez stos protokołu TLS firmy Apple w Mac i z systemem iOS.
