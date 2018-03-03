---
title: Sharpie celu
description: "Ta sekcja zawiera wprowadzenie do Sharpie cel, narzędzie wiersza polecenia platformy Xamarin w pozwala zautomatyzować proces tworzenia powiązania do biblioteki języka Objective C"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 02eebb7d8f579a207b6777771dbea223d30211cc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="objective-sharpie"></a>Sharpie celu

_Ta sekcja zawiera wprowadzenie do Sharpie cel, narzędzie wiersza polecenia platformy Xamarin w pozwala zautomatyzować proces tworzenia powiązania do biblioteki języka Objective C_

<style type="text/css"> niebieski .terminal {kolorów: rgb(10,96,254);} .terminal zielony {kolorów: rgb(12,156,26);} amarantowy .terminal {kolorów: rgb(152,12,103);} </style>

- [Omówienie](#overview) & [historii](#history)
- [Wprowadzenie](get-started.md)
- [Narzędzia i polecenia](tools.md)
- [Funkcje](platform/index.md)
- [Przykłady](examples/index.md)
- [Pełny przewodnik](~/ios/platform/binding-objective-c/walkthrough.md)
- [Historia wersji](releases.md)

#<a name="overview"></a>Omówienie

Celu Sharpie to narzędzie wiersza polecenia ułatwiające bootstrap przebiegu pierwszy powiązania.
Działa on za analizowanie plików nagłówka natywnej biblioteki do mapowania publiczny interfejs API do [powiązania definicji](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) (proces został ręcznie wcześniej).

Sharpie celu używa Clang analizy pliki nagłówkowe, więc jest powiązanie w pełnych i dokładne. Może to znacznie skrócić czas i nakładu pracy potrzebnego do tworzenia powiązań jakości.

> [!IMPORTANT]
> **Ostrzeżenie:** celu Sharpie to narzędzie dla deweloperów programu Xamarin doświadczenie z zaawansowanej wiedzy na temat języka Objective-C (i przez rozszerzenie, C). Przed podjęciem próby powiązania biblioteka języka Objective-C powinien mieć stałe wiedzę na temat sposobu tworzenia natywnej biblioteki w wierszu polecenia (i dobrą znajomością działania natywnej biblioteki).



#<a name="history"></a>Historia

Firma Microsoft rozwijającymi i za pomocą Sharpie cel wewnętrznie na platformie Xamarin dla ostatnich trzech lat. Jako dowód do potęgi Sharpie cel interfejsy API wprowadzone w Xamarin.iOS i Xamarin.Mac począwszy od zestawu iOS 8, Mac OS X 10.10, i watchOS 2.0 zostały zainicjowano z Sharpie cel. Xamarin zależy od silnie Sharpie cel wewnętrznie do tworzenia własnych produktów.

Jednak Sharpie celem jest bardzo zaawansowane narzędzia, która wymaga zaawansowanej wiedzy na temat języka Objective-C C, jak używać clang kompilator w wierszu polecenia i Podsumowując zazwyczaj jak natywnych bibliotek. Z powodu tego paska wysoka, możemy mieli świadomość o graficzny interfejs użytkownika kreator służy do ustawiania niewłaściwy oczekiwań i w efekcie Sharpie celem jest obecnie dostępna tylko jako narzędzie wiersza polecenia.



## <a name="related-links"></a>Linki pokrewne

- [Pobieranie Sharpie celu](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
- [Wskazówki: Powiązywanie biblioteka języka Objective C](~/ios/platform/binding-objective-c/walkthrough.md)
- [Tworzenie powiązań bibliotek języka Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Szczegóły powiązania](~/cross-platform/macios/binding/overview.md)
- [Powiązanie typy Podręcznik](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin dla deweloperów języka Objective C](~/ios/get-started/objective-c-developers/index.md)
