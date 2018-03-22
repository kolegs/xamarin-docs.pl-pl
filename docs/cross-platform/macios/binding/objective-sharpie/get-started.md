---
title: Wprowadzenie
ms.topic: article
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 53e64acd8e64c9a8151b2c55045db4e308f97531
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="getting-started"></a>Wprowadzenie

> [!IMPORTANT]
> Celu Sharpie to narzędzie dla deweloperów programu Xamarin doświadczenie z zaawansowanej wiedzy na temat języka Objective-C (i przez rozszerzenie, C). Przed podjęciem próby powiązania biblioteka języka Objective-C powinien mieć stałe wiedzę na temat sposobu tworzenia natywnej biblioteki w wierszu polecenia (i dobrą znajomością działania natywnej biblioteki).

<a name="installing" />

## <a name="installing-objective-sharpie"></a>Instalowanie celu Sharpie

Sharpie celu obecnie jest to samodzielne narzędzie wiersza polecenia dla systemu Mac OS X 10.10 i nowszych i jest _nie pełni obsługiwane produktu Xamarin_. Można stosować tylko przez deweloperów zaawansowane ułatwiających tworzenie projektu powiązanie 3rd stronie Biblioteka języka Objective-C.

Celu Sharpie można pobrać jako standardowa Instalator pakietu OS X.
Uruchom Instalatora i wykonaj wszystkie wyświetlanymi w Kreatorze instalacji:

- **Bieżąca wersja: 3.4**
  - [Pobieranie najnowszej wersji](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [Forum anonsu](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> Użyj `sharpie update` polecenia aktualizacji do najnowszej wersji.

## <a name="basic-walkthrough"></a>Podstawowe wskazówki

Sharpie celu jest narzędzia wiersza polecenia, pod warunkiem Xamarin, które pomaga w tworzeniu definicje wymagane powiązać 3 biblioteka języka Objective-C strona C#.
Nawet w przypadku używania Sharpie cel, deweloper *będzie* konieczne zmodyfikowanie wygenerowanego po zakończeniu Sharpie cel, aby rozwiązać problemy, które nie można obsłużyć automatycznie przez narzędzie.

Jeśli to możliwe, cel Sharpie będzie dodawać adnotacje interfejsów API, z którą ma pewne wątpliwości dotyczące poprawnie powiązać (wiele konstrukcji w kodzie natywnym są niejednoznaczne).
Adnotacje będą wyświetlane jako [ `[Verify]` atrybutów](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md).

Dane wyjściowe Sharpie celem jest para pliki - [ `ApiDefinition.cs` i `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) — można utworzyć projekt powiązania, który kompiluje do biblioteki można używać w aplikacji platformy Xamarin.

> [!IMPORTANT]
> Sharpie celu pochodzi z jednym **głównych** reguły dla prawidłowego użycia: należy bezwzględnie przekazać go clang poprawne argumenty wiersza polecenia kompilatora w celu zapewnienia prawidłowego analizy. Jest to spowodowane Sharpie cel analizy fazy jest po prostu narzędzie [zaimplementowana przed libtooling clang interfejsu API](http://clang.llvm.org/docs/LibTooling.html).

Oznacza to, że cel Sharpie ma pełną moc Clang (kompilator C/Objective-C/C++, faktycznie kompilowany natywnej biblioteki, który będzie powiązać) i wszystkie jego wewnętrznego wiedzy pliki nagłówkowe dla powiązania.
Zamiast tłumaczenia przeanalizowany [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree) do kodu obiektu Sharpie cel tłumaczy AST do języka C# powiązania "szkieletu" odpowiedniego dla danych wejściowych do `bmac` i `btouch` narzędzia powiązania Xamarin.

Jeśli Sharpie cel błędy się podczas analizowania oznacza to, że clang błędami się podczas próby utworzenia wyrażenie AST w fazie jego analizowanie i należy dowiedzieć się, dlaczego.

**NOWY!** w wersji 3.0 prób adresów niektóre tego złożoności dzięki obsłudze projektów Xcode bezpośrednio. Jeśli natywnej biblioteki jest prawidłowy projekt Xcode, cel Sharpie można ocenić projektu dla określonego obiektu docelowego i konfigurację, aby ustalić pliki niezbędne nagłówek wejściowy i flagi kompilatora.

Jeśli żaden projekt Xcode nie jest dostępna, należy znać więcej projektu przez wydedukowania plików poprawny nagłówek wejściowy, ścieżki wyszukiwania plików nagłówka i inne flagi kompilatora niezbędne. Jest zdawać sobie sprawę flagi kompilatora użytą do skompilowania natywnej biblioteki są takie same, które muszą być przekazywane do Sharpie cel. Jest to bardziej ręczny proces, a, który wymaga nieco znajomość kompilowanie kodu natywnego w wierszu polecenia z łańcuchem narzędzi Clang.

**NOWY!** wersja 3.0 wprowadza również narzędzia do łatwego wiązania [programu CocoaPods](https://cocoapods.org) za pośrednictwem `sharpie pod` polecenia.
Jeśli interesuje Cię biblioteka jest dostępna jako CocoaPod, firma Microsoft zaleca się, że rozpoczyna się od próby powiązania CocoaPod z celem Sharpie (w przeciwieństwie próby powiązania źródła bezpośrednio).
