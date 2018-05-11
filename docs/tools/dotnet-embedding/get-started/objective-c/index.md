---
title: Wprowadzenie do języka Objective C
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 0c2d92f52000bbc6d9d4ea3b07112795aa98bd0a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="getting-started-with-objective-c"></a>Wprowadzenie do języka Objective C

Jest to na stronie początkowej dla języka Objective-C, która obejmuje podstawy dla wszystkich obsługiwanych platformach.

## <a name="requirements"></a>Wymagania

Aby używać osadzanie .NET Objective-C, potrzebne są systemem Mac:

* System macOS 10.12 (Sierra) lub nowszy
* Xcode 8.3.2 lub nowszy
* [Mono 5.0](http://www.mono-project.com/download/)

Można zainstalować [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) do edycji i kompilowania kodu C#.

> [!NOTE]
> * Starsze niż system macOS Xcode i Mono _może_ działać, ale są zastosowaniem i nieobsługiwane
> * Generowanie kodu może odbywać się w systemie Windows, ale to tylko możliwe do skompilowania go na komputerze Mac, w którym zainstalowano Xcode

## <a name="installing-net-embedding-from-nuget"></a>Instalowanie .NET osadzanie z NuGet

Postępuj zgodnie z następującymi [instrukcje](~/tools/dotnet-embedding/get-started/install/install.md) do instalowania i konfigurowania osadzanie .NET projektu.

Wywołanie polecenia przykładowe ma na liście [macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) i [iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md) wprowadzenie przewodników.

## <a name="platforms"></a>Platformy

Języka Objective C jest językiem, który jest najczęściej używana do pisania aplikacji dla macOS, iOS, systemu tvOS i watchOS; osadzanie .NET obsługuje wszystkie tych platform. Praca z każdej z platform oznacza niektóre [podstawowe różnice i te zostały omówione w tym miejscu](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[Tworzenie aplikacji macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) jest najprostszym, ponieważ nie obejmuje tyle dodatkowe czynności, takie jak Ustawianie tożsamości, profile provisining, symulatorów i urządzeń. Zachęcamy rozpoczynać dokumentu macOS przed dla systemu iOS.

### <a name="ios--tvos"></a>iOS / systemu tvOS

Upewnij się, są już skonfigurowane do opracowywania aplikacji systemu iOS przed podjęciem próby go utworzyć za pomocą osadzanie .NET. [Instrukcjami](~/tools/dotnet-embedding/get-started/objective-c/ios.md) założono, że masz już pomyślnie skompilowane i wdrożyć aplikację systemu iOS z komputera.

Obsługę systemu tvOS jest odpowiednikiem jak działa z systemem iOS, używając tylko projekty systemu tvOS w IDEs (Visual Studio i środowiska Xcode) zamiast projektów dla systemu iOS.

> [!NOTE]
> Obsługa watchOS będą dostępne w przyszłych wydaniach i jest bardzo podobny do systemu iOS/tvOS.

## <a name="further-reading"></a>Dalsze informacje

* [Osadzanie .NET funkcje specyficzne dla języka Objective C](~/tools/dotnet-embedding/objective-c/index.md)
* [Najlepsze rozwiązania dla języka Objective C](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [Ograniczenia osadzania .NET](~/tools/dotnet-embedding/limitations.md)
* [Współtworzenie projekt open source](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Kody błędów wraz z opisami](~/tools/dotnet-embedding/errors.md)
* [Platformy docelowe](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe pogody (z systemem iOS & macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
