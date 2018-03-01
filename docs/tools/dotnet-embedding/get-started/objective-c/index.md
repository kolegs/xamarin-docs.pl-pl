---
title: "Wprowadzenie do języka Objective C"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 832ad2e6d462b39df7fa4a45739e97f7a7d6e5b5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-objective-c"></a>Wprowadzenie do języka Objective C

Jest to na stronie początkowej dla języka Objective-C, która obejmuje podstawy dla wszystkich obsługiwanych platformach.


## <a name="requirements"></a>Wymagania

Aby Objective-C za pomocą osadzanie .NET należy systemem Mac:

* System macOS 10.12 (Sierra) lub nowszy
* Xcode 8.3.2 lub nowszy
* [Mono 5.0](http://www.mono-project.com/download/)

Można zainstalować [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) do edycji i kompilowania kodu C#.


Uwagi:

* Starsze niż system macOS Xcode i Mono _może_ działać, ale są zastosowaniem i nieobsługiwane;
* Generowanie kodu może odbywać się w systemie Windows, ale to tylko możliwe do skompilowania go na komputerze Mac, w którym zainstalowano Xcode;


## <a name="installation"></a>Instalacja

Następnym krokiem jest, aby pobrać i zainstalować osadzanie .NET opartym na systemie

* [Package](https://dl.xamarin.com/embeddinator/Xamarin.Embeddinator-4000-0.2.0.79.pkg)
* [Uwagi do wersji](https://github.com/mono/Embeddinator-4000/tree/master/docs/releases)

Alternatywą jest możliwe utworzenie z naszych [repozytorium git](https://github.com/mono/Embeddinator-4000/tree/objc), zobacz [przyczyniając się](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) dokumentu, aby uzyskać instrukcje.

Instalator jest Instalator pkg standardowe na podstawie:

![Instalator wprowadzenie](images/install1.png)
![Instalator zainstalować typu](images/install2.png)
![podsumowanie Instalatora](images/install3.png)

Po zainstalowaniu za pomocą Instalatora, po uruchomieniu nową sesję terminala można użyć `objcgen` polecenia.
W przeciwnym razie będzie można uruchamiać narzędzie za pośrednictwem ścieżki bezwzględne: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`.

## <a name="platforms"></a>Platformy

Języka Objective C jest językiem, który jest najczęściej używana do pisania aplikacji dla macOS, iOS, systemu tvOS i watchOS; i embeddinator obsługuje wszystkie tych platform. Praca z każdej z platform oznacza niektóre podstawowe różnice i objaśniono [tutaj](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[Tworzenie aplikacji macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) jest najprostszym, ponieważ nie obejmuje tyle dodatkowe czynności, takie jak Ustawianie tożsamości, profile provisining, symulatorów i urządzeń. Zachęcamy rozpoczynać dokumentu macOS przed dla systemu iOS.

### <a name="ios--tvos"></a>iOS / systemu tvOS

Upewnij się, że są już skonfigurowane do opracowywania aplikacji systemu iOS przed podjęciem próby go utworzyć za pomocą embeddinator. [Instrukcjami](~/tools/dotnet-embedding/get-started/objective-c/ios.md) założono, że masz już pomyślnie skompilowane i wdrożyć aplikację systemu iOS z komputera.

Obsługę systemu tvOS jest odpowiednikiem jak działa z systemem iOS, używając tylko projekty systemu tvOS w IDEs (Visual Studio i środowiska Xcode) zamiast projektów dla systemu iOS.

> [!NOTE]
> Uwaga: Obsługa watchOS będzie dostępna w przyszłym wydaniu i jest bardzo podobny do systemu iOS/tvOS.


## <a name="further-reading"></a>Dalsze informacje

* [Osadzanie .NET funkcje specyficzne dla języka Objective C](~/tools/dotnet-embedding/objective-c/index.md)
* [Najlepsze rozwiązania dla języka Objective C](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [Ograniczenia osadzania .NET](~/tools/dotnet-embedding/limitations.md)
* [Współtworzenie projekt open source](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Kody błędów wraz z opisami](~/tools/dotnet-embedding/errors.md)
* [Platformy docelowe](~/tools/dotnet-embedding/objective-c/platforms.md)


## <a name="related-links"></a>Linki pokrewne

- [Przykładowe pogody (z systemem iOS & macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
