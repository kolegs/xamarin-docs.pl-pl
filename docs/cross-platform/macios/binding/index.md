---
title: Powiązanie Objective-C
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: fef2826f536042dc9be830a4c0dc358658c359d9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="binding-objective-c"></a>Powiązanie Objective-C

Ta sekcja zawiera szereg dokumenty, które obejmują tworzenie powiązania z bibliotek języka Objective-C, więc można wywołać z utworzonych za pomocą platformy Xamarin.iOS lub Xamarin.Mac aplikacji C#.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Omówienie](~/cross-platform/macios/binding/overview.md)

Ten dokument zawiera niektóre funkcje wewnętrzne sposób następuje powiązanie. Jest zaawansowane dokument pewne informacje techniczne.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Tworzenie powiązań bibliotek języka Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)

Ten dokument zawiera opis procesu używane do tworzenia powiązań C# interfejsów API języka Objective C i sposobie mapowania idioms w języku Objective C idioms używane w programie .NET.
Są wiązane tylko interfejsy API C, należy użyć standardowego mechanizmu .NET dla tego framework P/Invoke.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Przewodnik po definicji powiązania](~/cross-platform/macios/binding/binding-types-reference.md)

Jest to przewodnik odwołania, który opisuje wszystkie atrybuty, które są dostępne dla autorów powiązania do procesu tworzenia powiązania.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Narzędzie Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Celu Sharpie to narzędzie wiersza polecenia ułatwiające bootstrap przebiegu pierwszy powiązania. Działa on za analizowanie plików nagłówka natywnej biblioteki do mapowania publiczny interfejs API do [powiązania definicji](~/cross-platform/macios/binding/objective-c-libraries.md) (proces można również wykonać ręcznie).

## <a name="ios"></a>iOS

[Strony powiązanie iOS](~/ios/platform/binding-objective-c/index.md) łącza z powrotem do tych wspólnych zasobów powiązanie dodatkowo do poniższych przykładów.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[Wskazówki: Powiązywanie biblioteka języka Objective C](~/ios/platform/binding-objective-c/walkthrough.md)

Ten artykuł zawiera przewodnik krok po kroku, tworzenia powiązania projektu przy użyciu typu open source [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) projektu języka Objective-C, na przykład. Biblioteka InfColorPicker udostępnia kontroler widoku wielokrotnego użytku, który zezwala użytkownikowi na wybranie koloru oparta na jej reprezentacji HSB zaznaczeniu kolor bardziej przyjazny dla użytkownika. Sharpie celu będzie służyć do pomocy w procesie powiązania.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Przykłady powiązania](https://github.com/mono/monotouch-bindings)

Kolekcja powiązań innych firm, które mogą być używane odwołanie podczas tworzenia nowego powiązania projektów.

## <a name="mac"></a>Mac

W przeszłości [powiązania Mac](~/mac/platform/binding.md) było bardzo ręczny proces. Obecnie [do pobrania Podgląd](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) obsługi komputerów Mac powiązania projektu w przyszłej wersji programu Visual Studio dla komputerów Mac.



## <a name="related-links"></a>Linki pokrewne

- [iOS powiązania](~/ios/platform/binding-objective-c/index.md)
- [Powiązanie Mac](~/mac/platform/binding.md)
