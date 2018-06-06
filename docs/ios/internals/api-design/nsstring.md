---
title: NSString Xamarin.iOS i Xamarin.Mac
description: W tym dokumencie opisano, jak Xamarin.iOS przezroczysty konwertuje NSString obiektów C# obiektów string, jeśli tak nie jest.
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: baf36700ab4d608296a9a67e234ce613da9ca077
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786093"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>NSString Xamarin.iOS i Xamarin.Mac

Projekt platformy Xamarin.iOS i Xamarin.Mac odwołuje się do interfejsu API używany do udostępnienia natywnego typu ciąg .NET `string`, manipulowanie ciągami w języku C# i innych języków programowania .NET, a także do udostępnienia ciągu jako typ danych udostępnianych przez interfejs API, zamiast `NSString` — typ danych.

Oznacza to, że deweloperzy nie powinny zachować ciągów, które są przeznaczone do użycia dla wywołania do Xamarin.iOS & Xamarin.Mac interfejsu API (Unified) w specjalny typ (`Foundation.NSString`), można nadal używać tego Mono `System.String` dla wszystkich operacji gdy Interfejs API w Xamarin.iOS lub Xamarin.Mac wymaga parametrów, zajmuje się naszego powiązania interfejsu API przekazywanie informacji.

Na przykład właściwość "text" Objective-C w `UILabel` typu `NSString`, jest zadeklarowany w następujący sposób:

```objc
@property(nonatomic, copy) NSString *text
```

To jest widoczna w Xamarin.iOS jako:

```csharp
class UILabel {
    public string Text { get; set; }
}
```

W tle stosowania tej właściwości marshals ciągu języka C# do `NSString` i wywołuje `objc_msgSend` metody w taki sam sposób, który będzie Objective-C.

Istnieje kilka interfejsów API języka Objective-C innych firm, które nie zajmują `NSString`, ale zamiast tego używać ciągu C ("*char*"). W takich przypadkach można nadal używać typu danych string C#, ale należy użyć [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) atrybutu informują generator powiązanie tego ciągu nie powinien zostać zorganizowany jako `NSString`, ale zamiast tego w postaci ciągu C.

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>Wyjątki od reguły

Xamarin.iOS i Xamarin.Mac wprowadzono wyjątek od tej reguły. Wybór między po uwidaczniamy `string`s, i gdy wykonujemy z wyjątkiem i ujawnia `NSString`s, jest wykonywane, gdy `NSString` metody można realizacji porównanie wskaźnika zamiast zawartością porównania.

Może się tak zdarzyć, gdy interfejsów API języka Objective-C używa publicznego `NSString` stałej jako token reprezentujący niektóre akcje, zamiast rzeczywistej zawartości ciągu porównanie.

W takich przypadkach `NSString` interfejsy API są widoczne, a mniejszości interfejsów API, które mają to. Można również zauważyć, że właściwości NSString są widoczne w niektórych klas. Te `NSString` właściwości są widoczne dla elementów, takich jak powiadomienia. Te są właściwości zwykle wyglądać następująco:

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```
Powiadomienia są klucze, które są używane do `NSNotification` klasy, jeśli chcesz zarejestrować dla określonego zdarzenia są emitowane przez środowisko uruchomieniowe.

Klucze zwykle wyglądać mniej więcej tak:

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

Inne miejsce gdzie `NSString`s są widoczne w interfejsie API jest jako tokenów, które są używane jako parametry do niektórych interfejsów API w systemie iOS i OS X, które przyjmują `NSDictionary` obiektów jako parametry. Słownik zwykle zawiera `NSString` kluczy. Xamarin.iOS, przez Konwencję, nazwy tych statycznych `NSString` właściwości przez dodanie nazwy "Klucz".
