---
title: "Powiązanie bibliotek systemu iOS"
description: "Porady: tworzenie natywnych bibliotek systemu iOS (i programu CocoaPods) dostępny w aplikacji platformy Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3afe1a03299e600502d49b1db039af4c6642e131
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="binding-ios-libraries"></a>Powiązanie bibliotek systemu iOS

_Porady: tworzenie natywnych bibliotek systemu iOS (i programu CocoaPods) dostępny w aplikacji platformy Xamarin._

Skorzystaj z poniższych linków, aby dowiedzieć się więcej o powiązanie bibliotek języka Objective C i programu CocoaPods Xamarin.iOS i Xamarin.Mac:

- [**Omówienie** ](~/cross-platform/macios/binding/overview.md) -
  opisano, jak działa powiązania.
- [**Powiązanie bibliotek języka Objective-C** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  instrukcje dotyczące sposobu powiązać bibliotek języka Objective-C do użycia w projektach Xamarin.
- [**Typ definicji podręcznik** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  opisano wszystkie atrybuty, które są dostępne dla autorów powiązania do procesu tworzenia powiązania.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Narzędzie Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Celu Sharpie to narzędzie wiersza polecenia ułatwiające bootstrap przebiegu pierwszy powiązania.
Działa on za analizowanie plików nagłówka natywnej biblioteki do mapowania publiczny interfejs API do [powiązania definicji](~/cross-platform/macios/binding/objective-c-libraries.md) (proces w przeciwnym razie jest wykonywane ręcznie). Celu Sharpie nie tworzy powiązanie samodzielnie, ale może pomóc ułatwiające rozpoczęcie pracy!

Celu 3.0 Sharpie wprowadzono możliwość powiązać bezpośrednio programu Cocoapods!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[Wskazówki — wiązanie iOS biblioteka języka Objective C](walkthrough.md)

Ta strona zawiera przewodnik krok po kroku tworzenia projektu powiązania z systemem iOS przy użyciu typu open source [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker) projektu języka Objective-C, na przykład. **InfColorPicker** biblioteka zawiera kontroler widoku wielokrotnego użytku, który zezwala użytkownikowi na wybranie koloru oparta na jej reprezentacji HSB zaznaczeniu kolor bardziej przyjazny dla użytkownika.
Sharpie celu będzie służyć do pomocy w procesie powiązania.



## <a name="related-links"></a>Linki pokrewne

- [Tworzenie powiązań języka Objective-C](~/cross-platform/macios/binding/index.md)
- [Powiązanie Mac](~/mac/platform/binding.md)
