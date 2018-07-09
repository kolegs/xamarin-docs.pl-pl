---
title: Tworzenie powiązań z narzędzie Objective Sharpie
description: Ta sekcja zawiera wprowadzenie do Objective Sharpie, Xamarin narzędzie wiersza polecenia, służące do automatyzowania procesu tworzenia wiązania do biblioteki języka Objective-C
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 53fcbbc408ae147405a3285d9391457051d6e16e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854801"
---
# <a name="creating-bindings-with-objective-sharpie"></a>Tworzenie powiązań z narzędzie Objective Sharpie

_Ta sekcja zawiera wprowadzenie do Objective Sharpie, Xamarin narzędzie wiersza polecenia, służące do automatyzowania procesu tworzenia wiązania do biblioteki języka Objective-C_

- [Omówienie](#overview) & [historii](#history)
- [Wprowadzenie](get-started.md)
- [Narzędzia i polecenia](tools.md)
- [Funkcje](platform/index.md)
- [Przykłady](examples/index.md)
- [Szczegółowy przewodnik](~/ios/platform/binding-objective-c/walkthrough.md)
- [Historia wersji](releases.md)

## <a name="overview"></a>Omówienie

Narzędzie Objective Sharpie to narzędzie wiersza polecenia ułatwiające bootstrap w pierwszym przebiegu powiązania.
Działa przez analizowanie plików nagłówka natywną bibliotekę do mapowania publicznego interfejsu API do [powiązanie definicji](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) (proces, która wcześniej była przeprowadzana ręcznie).

Narzędzie Objective Sharpie używa Clang parsuj pliki nagłówka, powiązanie jest dokładne i możliwie dokładny. Może to znacznie skrócić czas i nakład pracy wymagany do utworzenia powiązania jakości.

> [!IMPORTANT]
> Narzędzie Objective Sharpie to narzędzie dla doświadczonych deweloperów platformy Xamarin przy użyciu zaawansowanej wiedzy Objective-c (a, C). Przed podjęciem próby powiązania do biblioteki języka Objective-C powinny mieć stałych wiedzę na temat sposobu tworzenia natywną bibliotekę w wierszu polecenia (i dobre zrozumienie sposobu działania bibliotekę natywną).

## <a name="history"></a>Historia

Firma Microsoft ewoluują i przy użyciu Objective Sharpie wewnętrznie w Xamarin przez ostatnie trzy lata. Świadectwo siły spotykającej do potęgi równej Objective Sharpie interfejsów API, wprowadzona w Xamarin.iOS i Xamarin.Mac od system iOS 8, Mac OS X 10.10 firma i watchOS 2.0 zostały załadować wyłącznie przy użyciu Objective Sharpie. Xamarin rolę odgrywa Objective Sharpie wewnętrznie do tworzenia swoich własnych produktów.

Jednak Objective Sharpie jest bardzo zaawansowane narzędzie, które wymaga zaawansowanej wiedzy na temat języka Objective-C i C, jak używać kompilatora clang w wierszu polecenia i bibliotek natywnych zazwyczaj jak zostały zbudowane. Ze względu na ten pasek wysoka, firma Microsoft uznało, posiadające graficznym interfejsem użytkownika Kreator ustawia niewłaściwego oczekiwania, a w efekcie Objective Sharpie jest obecnie dostępna wyłącznie jako narzędzie wiersza polecenia.

## <a name="related-links"></a>Linki pokrewne

- [Objective Sharpie pobierania](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
- [Wskazówki: Powiązywanie biblioteki języka Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)
- [Tworzenie powiązań bibliotek języka Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Szczegóły powiązania](~/cross-platform/macios/binding/overview.md)
- [Podręcznik informacyjny typów powiązań](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin dla deweloperów języka Objective-C](~/ios/get-started/objective-c-developers/index.md)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
