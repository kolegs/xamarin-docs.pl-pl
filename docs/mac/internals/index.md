---
title: Kulisy
description: Rzut oka na przebiega Xamarin.Mac
ms.topic: article
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 2ba3ffb421dc64bba7df1e10a40125f14365f29e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="under-the-hood"></a>Kulisy

_Rzut oka na przebiega Xamarin.Mac_

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Wyprzedzeniem o czas kompilacji (drzewa obiektów aplikacji)](aot.md)

Wyprzedzeniem czasu (drzewa obiektów aplikacji) kompilacji to technika zaawansowanych optymalizacji dla poprawy wydajności uruchamiania. Jednak wpływa to również na czas kompilacji, rozmiar aplikacji i wykonywanie programu w sposób głęboki, więc warto zrozumienia sposobu jego działania.

## <a name="mac-architecturearchitecturemd"></a>[Architektura Mac](architecture.md)

Relacja w Xamarin.Mac Objective-C, w tym pojęcia, takie jak kompilacji, selektorów, rejestratorów, uruchamianie aplikacji i generatora.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac registrar](registrar.md)

Xamarin.Mac stanowi pomost między świecie zarządzany i środowiska uruchomieniowego w Cocoa, umożliwiając klasy zarządzane wywołać klasy niezarządzane Objective-C i można wywołać z powrotem po wystąpieniu zdarzenia. Pracy wymaganej w celu przeprowadzić to "Magia" jest obsługiwany przez rejestratora, ale zrozumieć, co się dzieje "kulisy", czasami mogą być pomocne.
