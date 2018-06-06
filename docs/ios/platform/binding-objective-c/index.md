---
title: Powiązanie bibliotek systemu iOS
description: Ten dokument zawiera opis sposobu tworzenia C# powiązania z kodu języka Objective-C, co umożliwia stosowanie natywnych bibliotek i programu CocoaPods w aplikacji platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b054595568a34616a01f2c3f3c7d85f968c3f1fa
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787174"
---
# <a name="binding-ios-libraries"></a>Powiązanie bibliotek systemu iOS

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

## <a name="xamarin-university-lightning-lecture"></a>Wykładu Lightning Xamarin University

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS powiązania w języku C/C++ za [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie powiązań języka Objective-C](~/cross-platform/macios/binding/index.md)
- [Powiązanie Mac](~/mac/platform/binding.md)
