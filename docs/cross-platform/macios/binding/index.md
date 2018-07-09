---
title: Powiązania Objective-C
description: Ten dokument zawiera łącza do różnych przewodniki, które zawierają opis sposobu tworzenia, dzięki czemu deweloperzy mogą korzystać z bibliotek standardowych w aplikacjach platformy Xamarin dla kodu języka Objective-C, powiązania C#.
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 3f1e1ce324e849c0c939d936eb6ee1470cf24a3b
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855159"
---
# <a name="binding-objective-c"></a>Powiązania Objective-C

Ta sekcja zawiera szereg dokumentów, które obejmują tworzenie powiązań bibliotek języka Objective-C, dzięki czemu można wywołać z C# aplikacje utworzone przy użyciu platformy Xamarin.iOS i Xamarin.Mac.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Omówienie](~/cross-platform/macios/binding/overview.md)

Ten dokument zawiera niektóre funkcje wewnętrzne, jak odbywa się powiązania. Jest to zaawansowane dokument, niektóre informacje techniczne.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Tworzenie powiązań bibliotek języka Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)

W tym dokumencie opisano proces, który został użyty do utworzenia powiązania C# interfejsów API języka Objective-C i sposobie mapowania idiomy w języku Objective-C idiomy używane na platformie .NET.
Jeśli dokonywane jest wiązanie tylko interfejsy API języka C, należy użyć standardowego mechanizmu .NET w tym celu framework P/Invoke.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Podręcznik informacyjny definicji powiązania](~/cross-platform/macios/binding/binding-types-reference.md)

Jest to przewodnik, który opisuje wszystkie atrybuty, które są dostępne dla autorów powiązania do procesu tworzenia wiązania.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Narzędzie Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Narzędzie Objective Sharpie to narzędzie wiersza polecenia ułatwiające bootstrap w pierwszym przebiegu powiązania. Działa przez analizowanie plików nagłówka natywną bibliotekę do mapowania publicznego interfejsu API do [powiązanie definicji](~/cross-platform/macios/binding/objective-c-libraries.md) (proces, można również wykonać ręcznie).

## <a name="ios"></a>iOS

[Strony powiązanie z systemem iOS](~/ios/platform/binding-objective-c/index.md) linkiem do tych wspólnych zasobów powiązania dodatkowo do poniższych przykładach.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[Wskazówki: Powiązywanie biblioteki języka Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)

Ten artykuł zawiera instrukcje krok po kroku przewodnik tworzenia powiązań projektu za pomocą "open source" [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) projektu języka Objective-C jako przykład. Biblioteka InfColorPicker zawiera kontroler widoku wielokrotnego użytku, która pozwala użytkownikowi na wybranie kolor oparty na jego reprezentację HSB, dzięki czemu wybór koloru jest bardziej przyjazny dla użytkownika. Narzędzie Objective Sharpie będzie służyć do pomocy w procesie powiązania.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Przykłady powiązania](https://github.com/mono/monotouch-bindings)

Kolekcja powiązania innych firm, które mogą być używane odwołanie podczas tworzenia nowego powiązania projektów.

## <a name="mac"></a>Mac

W przeszłości [powiązania Mac](~/mac/platform/binding.md) było bardzo ręczny proces. Ma obecnie [do pobrania w wersji zapoznawczej](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) obsługi komputerów Mac powiązania projektu w przyszłej wersji programu Visual Studio dla komputerów Mac.



## <a name="related-links"></a>Linki pokrewne

- [iOS powiązania](~/ios/platform/binding-objective-c/index.md)
- [Powiązania komputerów Mac](~/mac/platform/binding.md)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
