---
title: Kiedy i w jaki sposób mogę powinien pliku raport o usterce?
description: W tym dokumencie opisano, kiedy, gdzie i w jaki sposób na raport o usterce. Zapewnia również raport o usterce, najlepsze rozwiązania umożliwiające engineers, aby jak najlepiej zdiagnozować problem.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: conceptdev
ms.author: crdun
ms.date: 08/01/2018
ms.openlocfilehash: f20740ff1e16187be3d3703b3da07329f6f52daf
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514341"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Kiedy i w jaki sposób mogę powinien pliku raport o usterce?

> [!TIP]
> Użyj **Zgłoś problem** element menu w programie Visual Studio &ndash; spowoduje to wysłanie informacji diagnostycznych oraz raport o usterce, aby pomóc rozwiązać ten problem.
>
> Istnieją szczegółowe instrukcje dotyczące [programu Visual Studio 2017](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017) i [programu Visual Studio dla komputerów Mac](https://docs.microsoft.com/visualstudio/mac/report-a-problem).
>
> Możesz wyszukać istniejące raporty na [społeczności deweloperów programu Visual Studio](https://developercommunity.visualstudio.com/) witryny sieci Web.

## <a name="file-a-bug-if"></a>Zgłoś usterkę, jeśli...

Masz zestaw kroków Twoim zdaniem, inżynierów będzie można użyć do odtworzenia problemu.

LUB

Wystarczy opisać widoczne objawy problemu, należy dokładnie, zwłaszcza, jeśli może również opisywać niektórych konkretnych okoliczności związane z problemem. <sup> [[1]](#note-1)</sup>

## <a name="best-practices-to-help-address-bugs-quickly-and-efficiently"></a>Najważniejsze wskazówki ułatwiające adres błędy szybciej i wydajniej

1. <a name="ref-1" />Wyszukiwanie [społeczności deweloperów programu Visual Studio](https://developercommunity.visualstudio.com/) i sieci web dla istniejących usterek, raportów lub sugestie użycia, które mogą rozwiązać problem bezpośrednio.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />Opisz problem wyraźnie i zwięźle, jak to możliwe, łącznie z opisem co się stało i był oczekiwany efekt.

1. <a name="ref-3" />Obejmują wszystkie ślady stosu istotne, tekst komunikatu o błędzie lub wszystkie dzienniki awarii (Jeśli używasz **Zgłoś problem** funkcji, mogą to być uwzględniane automatycznie). <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />Zapisz wszystkie ważne komunikaty o błędach w zrzucie ekranu załączniki jako zwykły tekst zbyt.

1. <a name="ref-5" />Obejmują małe, niezależne przypadek testowy, który powoduje wygenerowanie błędu przy użyciu niewielkiej ilości kodu, jak to możliwe.  Jeśli nie można odtworzyć problem z zupełnie nowym projektem (utworzone przy użyciu jednego z wbudowanych szablonów), proszę zip projektu, który pokazuje problem i dołączyć go do raportu o usterce.  Należy przykładowy projekt tak proste, jak to możliwe, przed dołączeniem go. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />Opisz środowisko, gdzie Napotkano błąd, w tym system operacyjny i [wersji programu Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) oraz wszystkie zależności.

## <a name="additional-details"></a>Dodatkowe szczegóły

1. <a name="note-1" />[*^*](#ref-1) Najlepiej opis "widoczne objawy" powinien zawierać wystarczającą ilość szczegółów, dzięki czemu pozostali klienci powinni sprawdzić, czy są one widoczne ten sam problem (tego samego komunikaty o błędach, tym samym obniżenie wydajności, tym samym ślad stosu pochodzący z awarii, _itp._ ). "Konkretnych okoliczności", dobrym przykładem będzie jeśli mogą mówić mniej więcej tak: "zwykle osiągnę problem 75% czasu, ale zmiana to jedno następnie można uniknąć tego problemu całkowicie". Inny przykład podobne "dokładne okoliczności" jest, jeśli zmiany na starszą wersję do poprzedniej wersji programu Xamarin nie będzie możliwy problem.

1. <a name="note-2" />[*^*](#ref-2) Jak można oczekiwać fragmenty tekstu błędu (lub inny tekst opisowy jednoznacznie) są zazwyczaj na najlepsze terminy wyszukiwania. Jeśli istniejący raport o usterce jest ukończona, zachęcamy do Dodaj szczegóły lub pliku nową, lepiej usterek — raport.

1. <a name="note-3" />[*^*](#ref-3) Innym dobrym pytaniem jest, czy ten sam problem został zgłoszony dla dowolnego języka Java, Objective-C lub Swift aplikacji. Jeśli tak, problem jest najprawdopodobniej częścią systemu Android lub iOS sam, a nie częścią platformy Xamarin.

1. <a name="note-4" />[*^*](#ref-4) Kilka przykładów informacje do uwzględnienia:

    1. Dla błędów występujących podczas kompilowania projektu, należy podać pełną [dane wyjściowe kompilacji diagnostycznych](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) na raport o usterce.

    1. Błędy występujące podczas kompilowania lub debugowania projektu systemu iOS w programie Visual Studio, uruchom **Pomoc > Xamarin > Zip dzienniki** po osiągnięcia błędu oraz zawierać wynikowy plik zip na raport o usterce.

    1. Wyjątki lub awarie w aplikacjach systemu Android lub iOS, podaj odpowiednią [debugowania dzienniki dla aplikacji platformy Xamarin.Android i Xamarin.iOS](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps).

1. <a name="note-5" />[*^*](#ref-5) Jeśli to możliwe dla konkretnego problemu, jedną z opcji jest odtworzyć problem, dodając niewielką liczbę plików z oryginalnego rozwiązania do zupełnie nowego rozwiązania. Zespół platformy Xamarin często będzie do badania problemów, nawet w przypadku większych przypadki testowe (przy założeniu, że wyraźnie opisano kroki prowadzące do odtworzenia), ale najlepiej szansy szybko rozwiązać błąd prostsze zapewniają przypadków testowych.

1. <a name="note-6" />[*^*](#ref-6) Jeśli jest _nie_ można odtworzyć problem, dodając niewielką liczbę plików do zupełnie nowego rozwiązania, następnie można zip i Dołącz folder całego rozwiązania dla swojej aplikacji pełny. Usuń `bin`, `obj`, `Components`, i `packages` foldery, aby wprowadzić zip plik mniejszy. (Środowiska IDE i procesem kompilacji będzie zazwyczaj przywracania lub odtworzyć zawartość tych folderów, zgodnie z potrzebami). Można również usunąć dowolną liczbę kodu i zasobów plików z projektu, jak chcesz, tak długo, jak długo wynikowy rozwiązanie nadal pokazuje oryginalnego problemu.
