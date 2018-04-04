---
title: Kulisy
description: Rzut oka na przebiega Xamarin.Mac
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 74721e880bb0d3ada3f3940a4074d06f55601c0e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="under-the-hood"></a>Kulisy

_Rzut oka na przebiega Xamarin.Mac_

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Wyprzedzeniem o czas kompilacji (drzewa obiektów aplikacji)](aot.md)

Wyprzedzeniem czasu (drzewa obiektów aplikacji) kompilacji to technika zaawansowanych optymalizacji dla poprawy wydajności uruchamiania. Jednak wpływa to również na czas kompilacji, rozmiar aplikacji i wykonywanie programu w sposób głęboki, więc warto zrozumienia sposobu jego działania.

## <a name="mac-architecturearchitecturemd"></a>[Architektura Mac](architecture.md)

Relacja w Xamarin.Mac Objective-C, w tym pojęcia, takie jak kompilacji, selektorów, rejestratorów, uruchamianie aplikacji i generatora.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac registrar](registrar.md)

Xamarin.Mac stanowi pomost między świecie zarządzany i środowiska uruchomieniowego w Cocoa, umożliwiając klasy zarządzane wywołać klasy niezarządzane Objective-C i można wywołać z powrotem po wystąpieniu zdarzenia. Pracy wymaganej w celu przeprowadzić to "Magia" jest obsługiwany przez rejestratora, ale zrozumieć, co się dzieje "kulisy", czasami mogą być pomocne.
