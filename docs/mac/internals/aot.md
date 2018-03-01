---
title: "Xamarin.Mac wcześniejsze czasu kompilacji"
description: "Korzystania z wady i zalety kompilacji czasu (drzewa obiektów aplikacji) i uwagi"
ms.topic: article
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: f9ace41380987472b6957c94719e6ae9b6f7995d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinmac-ahead-of-time-compilation"></a>Xamarin.Mac wcześniejsze czasu kompilacji

## <a name="overview"></a>Omówienie

Wyprzedzeniem czasu (drzewa obiektów aplikacji) kompilacji to technika zaawansowanych optymalizacji dla poprawy wydajności uruchamiania. Jednak wpływa to również na czas kompilacji, rozmiar aplikacji i wykonanie programu głęboki sposoby. Aby poznać skutków ubocznych, który nakłada, chcemy się nieco Poznaj kompilacji i wykonywania aplikacji.

Kod napisany w językach zarządzanych, takich jak C# i F # jest kompilowana do reprezentacji pośredniej o nazwie IL. Ta IL przechowywane w zestawach z biblioteki i program, jest stosunkowo compact i przenośnych, między architekturami procesorów. IL, jest jednak tylko pośredni Ustaw instrukcji i w pewnym momencie, które będzie można przetłumaczyć jej na kod maszynowy specyficznych dla procesora IL.

Istnieją dwa punkty, w których możesz zrobić to przetwarzanie:

- **Tylko w time (JIT)** — podczas uruchamiania oraz wykonywanie aplikacji ma być kompilowana w pamięci, aby kod maszynowy IL.
- **Czas (drzewa obiektów aplikacji) z wyprzedzeniem** — podczas kompilacji IL jest skompilowany i zapisywany do natywnych bibliotek i przechowywane w ramach Twojego pakietu aplikacji.

Każda opcja charakteryzuje się pewną liczbę zalety i wady i zalety:

- **JIT**
  - **Czas uruchamiania** — kompilacji JIT musi zostać wykonane podczas uruchamiania. W przypadku większości aplikacji jest rzędu 100ms, ale dla dużych aplikacji teraz, może być znacznie więcej.
  - **Wykonanie** — jako JIT kodu mogą być zoptymalizowany pod kątem określonej używanego procesora, można wygenerować kodu nieco większą. W większości aplikacji jest kilka punktów procentowych szybciej co najwyżej.
- **AOT**
  - **Czas uruchamiania** — ładowania wstępnie skompilowanym dylibs jest znacznie szybsze niż zestawy JIT.
  - **Miejsce na dysku** — te dylibs może jednak zająć znaczną ilość miejsca na dysku. W zależności od tego, które zestawy są AOTed, można dwukrotnie lub więcej rozmiar części kodu aplikacji.
  - **Czas kompilacji** — kompilacja drzewa obiektów aplikacji jest znacznie niższej tego JIT i będzie spowolnić kompilacje przy jej użyciu. To spowolnienie mogą należeć do zakresu od sekund maksymalnie kilka minut lub więcej, w zależności od rozmiaru i liczby zestawów skompilowany.
  - **Zaciemnienie** — jako IL, który jest znacznie łatwiejsze do odtworzenia niż kod maszynowy, nie jest zawsze wymagane mogą być usunięte ułatwiające zasłaniają poufnych kodu. Wymaga to "Hybrydowe" opcji opisano poniżej.

## <a name="enabling-aot"></a>Włączanie drzewa obiektów aplikacji

Opcje drzewa obiektów aplikacji zostaną dodane do okienka Mac kompilacji w przyszłej aktualizacji. Do tego czasu włączenie drzewa obiektów aplikacji wymaga przekazywanie argumentów wiersza polecenia za pomocą pola "mmp dodatkowych argumentów" w Mac kompilacji. Dostępne są następujące opcje:


      --aot[=VALUE]          Specify assemblies that should be AOT compiled
                               - none - No AOT (default)
                               - all - Every assembly in MonoBundle
                               - core - Xamarin.Mac, System, mscorlib
                               - sdk - Xamarin.Mac.dll and BCL assemblies
                               - |hybrid after option enables hybrid AOT which
                               allows IL stripping but is slower (only valid
                               for 'all')
                                - Individual files can be included for AOT via +
                               FileName.dll and excluded via -FileName.dll

                               Examples:
                                 --aot:all,-MyAssembly.dll
                                 --aot:core,+MyOtherAssembly.dll,-mscorlib.dll



## <a name="hybrid-aot"></a>Hybrydowe drzewa obiektów aplikacji

Podczas wykonywania aplikacji macOS środowiska uruchomieniowego domyślnie używa kod maszynowy załadowany z natywnych bibliotek, utworzonego przez kompilacji drzewa obiektów aplikacji. Istnieją jednak pewne kwestie kodu, takie jak trampolines, gdzie kompilacji JIT może spowodować znacznie więcej zoptymalizowane wyników. Wymaga to IL zarządzanych zestawów, które mają być dostępne. W systemach iOS aplikacje są ograniczone z jakimkolwiek użyciem kompilacji JIT; tych sekcji kodu zostały skompilowane również drzewa obiektów aplikacji.

Opcja hybrydowa instruuje kompilator, aby kompilacji obu tych sekcji (na przykład iOS), ale także do założono, że IL nie będą dostępne w czasie wykonywania. Następnie mogą być usunięte tego IL po kompilacji. Jak wspomniano powyżej, środowisko uruchomieniowe będą zmuszeni do użycia mniej zoptymalizowane procedur w niektórych miejscach.

## <a name="further-considerations"></a>Dodatkowe uwagi

Ujemna skutków skalowalność drzewa obiektów aplikacji rozmiary i liczbę zestawów przetworzone. Pełny [platformy docelowej](~/mac/platform/target-framework.md) przykład zawierający znacznie większe Base klasy biblioteki (BCL) niż Nowoczesny i w związku z tym drzewa obiektów aplikacji będzie trwać znacznie dłużej i tworzenia pakietów większych. To jest rozliczana przez platformę pełne docelową niezgodności z konsolidację, które usuwa nieużywane kodu. Rozważ przeniesienie nowoczesnych i włączanie konsolidację, aby uzyskać najlepsze wyniki przez aplikację.

Dodatkową korzyścią drzewa obiektów aplikacji zawiera ulepszone interakcji z natywnego debugowanie i profilowania toolchains. Ponieważ większość bazy kodu będzie można skompilować wcześniejsze, będzie mieć nazwy funkcji i symboli, które są łatwiejsze do odczytania wewnątrz raporty awarii macierzystego, profilowania i debugowanie. Funkcje JIT wygenerowany nie mają te nazwy i często wyświetlane jako nienazwane przesunięcia szesnastkowych, które są bardzo trudne do rozwiązania.
