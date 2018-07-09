---
title: Wprowadzenie do narzędzie Objective Sharpie
description: Ten dokument zawiera ogólne omówienie Objective Sharpie, narzędzie używane do zautomatyzowania tworzenia powiązania C# na kod języka Objective-C.
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: da8c51c4ba4df74afac950bbff867221e7307d6e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854782"
---
# <a name="getting-started-with-objective-sharpie"></a>Wprowadzenie do narzędzie Objective Sharpie

> [!IMPORTANT]
> Narzędzie Objective Sharpie to narzędzie dla doświadczonych deweloperów platformy Xamarin przy użyciu zaawansowanej wiedzy Objective-c (a, C). Przed podjęciem próby powiązania do biblioteki języka Objective-C powinny mieć stałych wiedzę na temat sposobu tworzenia natywną bibliotekę w wierszu polecenia (i dobre zrozumienie sposobu działania bibliotekę natywną).

<a name="installing" />

## <a name="installing-objective-sharpie"></a>Instalowanie narzędzie Objective Sharpie

Narzędzie Objective Sharpie jest obecnie autonomiczne narzędzie wiersza polecenia dla systemu Mac OS X 10.10 i nowszych, a _nie w pełni obsługiwana produktu Xamarin_. Można stosować tylko przez zaawansowanych deweloperów ułatwiają tworzenie projektu powiązania do 3 jednostki biblioteki języka Objective-C.

Narzędzie Objective Sharpie można pobrać jako standardowa Instalatora pakietu OS X.
Uruchom Instalatora i wszystkie postępuj zgodnie z monitami wyświetlanymi na ekranie za pomocą Kreatora instalacji:

- **Bieżąca wersja: 3.4**
  - [Pobieranie najnowszej wersji](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [Ogłoszenie na forum](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> Użyj `sharpie update` polecenia aktualizacji do najnowszej wersji.

## <a name="basic-walkthrough"></a>Podstawowe przewodnik

Narzędzie Objective Sharpie to narzędzie wiersza polecenia, dostarczone przez: Xamarin, które pomaga w tworzeniu definicji wymaganej do powiązania 3 biblioteki języka Objective-C innej firmy do języka C#.
Nawet w przypadku używania Objective Sharpie, deweloper *będzie* konieczne zmodyfikowanie wygenerowanych plików, po zakończeniu Objective Sharpie w celu rozwiązania problemów, które nie może być automatycznie obsługiwane przez narzędzie.

Jeśli to możliwe, interfejsy API za pomocą którego ma pewne wątpliwości dotyczące poprawnie powiązać zostaną dodane adnotacje Objective Sharpie (wiele konstrukcji w kodzie natywnym są niejednoznaczne).
Adnotacje będą wyświetlane jako [ `[Verify]` atrybuty](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md).

Dane wyjściowe Objective Sharpie to para plików - [ `ApiDefinition.cs` i `StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) — można utworzyć projekt powiązania, który kompiluje do biblioteki można używać w aplikacji platformy Xamarin.

> [!IMPORTANT]
> Narzędzie Objective Sharpie pochodzi z jednej **głównych** reguła właściwy sposób użycia: należy bezwzględnie udostępnić clang poprawne argumenty wiersza polecenia kompilatora w celu zapewnienia prawidłowego analizy. Jest to spowodowane Objective Sharpie, faza analizy to po prostu narzędzie [zaimplementowane względem clang libtooling API](http://clang.llvm.org/docs/LibTooling.html).

Oznacza to, że Objective Sharpie ma pełnych możliwości Clang (kompilator języka C/Objective-C/C++, który kompiluje faktycznie natywną bibliotekę, którą będzie można powiązać) i wszystkich jego wewnętrznej wiedzy pliki nagłówkowe dla wiązania.
Zamiast translacji przeanalizowanego [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree) do kodu obiektowego Objective Sharpie tłumaczy AST do języka C# powiązania "szkieletu" odpowiedniego dla danych wejściowych do `bmac` i `btouch` narzędzia powiązań platformy Xamarin.

Jeśli Objective Sharpie występuje podczas analizowania, oznacza to, że clang dotyczącego podczas jego analizowanie w fazie próby utworzenia wyrażenie AST i trzeba ustalić przyczynę.

**NOWY!** próby wersji 3.0 lub nowszej adresów niektóre tę złożoność dzięki obsłudze projektów Xcode bezpośrednio. Jeśli natywnej biblioteki jest to poprawny projekt programu Xcode, Objective Sharpie ocenić projektu dla określonego obiektu docelowego i konfigurację, aby ustalić niezbędne nagłówki danych wejściowych i flagi kompilatora.

Jeśli projekt Xcode nie jest dostępna, należy znać bardziej projektu przez wydedukowania plików poprawny nagłówek danych wejściowych, ścieżki wyszukiwania pliku nagłówka i inne flagi kompilatora niezbędne. Należy weź pod uwagę, że flagi kompilatora, używane do tworzenia natywnej biblioteki są takie same, muszą zostać przekazane do Objective Sharpie. Jest to bardziej ręczny proces, a, który wymaga znacznej liczby znajomość kompilowanie kodu natywnego w wierszu polecenia przy użyciu łańcucha narzędzi Clang.

**NOWY!** w wersji 3.0 wprowadzono również narzędzia do łatwego wiązania [CocoaPods](https://cocoapods.org) za pośrednictwem `sharpie pod` polecenia.
Jeśli interesuje Cię biblioteka jest dostępny jako CocoaPod, zalecamy rozpoczyna się od próby powiązania CocoaPod z Objective Sharpie (w przeciwieństwie do próby powiązania ze źródłem bezpośrednio).

## <a name="related-links"></a>Linki pokrewne

- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)