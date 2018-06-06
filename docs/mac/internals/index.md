---
title: Pod maską w Xamarin.Mac
description: Ten dokument łącza do różnych prowadnic, które opisują przebiega Xamarin.Mac. Omówiono w nim połączone dokumenty wcześniejsze czas kompilacji, architektura Xamarin.Mac i Xamarin.Mac rejestratora.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: c940252a675c38247d2c5bb374b9c30237222bda
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792492"
---
# <a name="under-the-hood-in-xamarinmac"></a>Pod maską w Xamarin.Mac

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Wyprzedzeniem o czas kompilacji (drzewa obiektów aplikacji)](aot.md)

Wyprzedzeniem czasu (drzewa obiektów aplikacji) kompilacji to technika zaawansowanych optymalizacji dla poprawy wydajności uruchamiania. Jednak wpływa to również na czas kompilacji, rozmiar aplikacji i wykonywanie programu w sposób głęboki, więc warto zrozumienia sposobu jego działania.

## <a name="mac-architecturearchitecturemd"></a>[Architektura Mac](architecture.md)

Relacja w Xamarin.Mac Objective-C, w tym pojęcia, takie jak kompilacji, selektorów, rejestratorów, uruchamianie aplikacji i generatora.

## <a name="xamarinmac-registrarregistrarmd"></a>[Rejestrator Xamarin.Mac](registrar.md)

Xamarin.Mac stanowi pomost między świecie zarządzany i środowiska uruchomieniowego w Cocoa, umożliwiając klasy zarządzane wywołać klasy niezarządzane Objective-C i można wywołać z powrotem po wystąpieniu zdarzenia. Pracy wymaganej w celu przeprowadzić to "Magia" jest obsługiwany przez rejestratora, ale zrozumieć, co się dzieje "kulisy", czasami mogą być pomocne.
