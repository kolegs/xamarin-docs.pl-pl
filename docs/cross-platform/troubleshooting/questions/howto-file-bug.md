---
title: Kiedy i w jaki sposób mogę powinien pliku raport o usterce?
description: W tym dokumencie opisano, kiedy, gdzie i w jaki sposób na raport o usterce. Zapewnia również raport o usterce, najlepsze rozwiązania umożliwiające engineers, aby jak najlepiej zdiagnozować problem.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
ms.openlocfilehash: b70fe29a79e099f1141c1295d907b48afaa2c3c7
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351609"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Kiedy i w jaki sposób mogę powinien pliku raport o usterce?

Zgłaszanie błędów w śledzenia usterek Bugzilla Xamarin tutaj: [ https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all ](https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all).

## <a name="file-a-bug-if"></a>Zgłoś usterkę, jeśli...

Masz zestaw kroków, które Twoim zdaniem inżynierów platformy Xamarin będzie można użyć do odtworzenia problemu, który jest spowodowany przez środowisko Xamarin.

LUB

Wystarczy opisać widoczne objawy problemu, należy dokładnie, zwłaszcza, jeśli może również opisywać niektórych konkretnych okoliczności związane z problemem. <sup> [[1]](#note-1)</sup>


## <a name="best-practices-to-help-xamarin-address-bugs-quickly-and-efficiently"></a>Najważniejsze wskazówki ułatwiające Xamarin adres błędy szybciej i wydajniej


1. <a name="ref-1" />Wyszukiwanie [Bugzilla](https://bugzilla.xamarin.com/query.cgi?format=specific&amp;bug_status=__all__) i sieci web dla istniejących usterek, raportów lub sugestie użycia, które mogą rozwiązać problem bezpośrednio.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />Postępuj zgodnie z [Błąd zapisywania wskazówki](https://bugzilla.xamarin.com/page.cgi?id=bug-writing.html) opisujący problem wyraźnie i zwięźle, jak to możliwe, łącznie z opisem co się stało i została prawidłowo do wykonania.

1. <a name="ref-3" />Obejmują wszystkie ślady stosu istotne, tekst komunikatu o błędzie lub wszystkie dzienniki awarii. <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />Zapisz wszystkie ważne komunikaty o błędach w zrzucie ekranu załączniki jako zwykły tekst zbyt.

1. <a name="ref-5" />Obejmują małe, niezależne przypadek testowy, który powoduje wygenerowanie błędu przy użyciu niewielkiej ilości kodu, jak to możliwe.  Jeśli nie można odtworzyć problem z zupełnie nowym projektem (utworzone przy użyciu jednego z wbudowanych szablonów), proszę zip projektu, który pokazuje problem i dołączyć go do raportu o usterce.  Należy przykładowy projekt tak proste, jak to możliwe, przed dołączeniem go. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />Opisz środowisko, gdzie Napotkano błąd, w tym system operacyjny i [wersji programu Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) oraz wszystkie zależności.

---

## <a name="additional-details"></a>Dodatkowe szczegóły

1. <a name="note-1" />[*^*](#ref-1) Najlepiej opis "widoczne objawy" powinien zawierać wystarczającą ilość szczegółów, dzięki czemu pozostali klienci powinni sprawdzić, czy są one widoczne ten sam problem (tego samego komunikaty o błędach, tym samym obniżenie wydajności, tym samym ślad stosu pochodzący z awarii, _itp._ ). "Konkretnych okoliczności", dobrym przykładem będzie jeśli mogą mówić mniej więcej tak: "zwykle osiągnę problem 75% czasu, ale zmiana to jedno następnie można uniknąć tego problemu całkowicie". Inny przykład podobne "dokładne okoliczności" jest, jeśli zmiany na starszą wersję do poprzedniej wersji programu Xamarin nie będzie możliwy problem.

1. <a name="note-2" />[*^*](#ref-2) Jak można oczekiwać fragmenty tekstu błędu (lub inny tekst opisowy jednoznacznie) są zazwyczaj na najlepsze terminy wyszukiwania. Jeśli istniejący raport o usterce jest ukończona, zachęcamy do Dodaj szczegóły lub pliku nową, lepiej usterek — raport.

1. <a name="note-3" />[*^*](#ref-3) Innym dobrym pytaniem jest, czy ten sam problem został zgłoszony dla dowolnego języka Java, Objective-C lub Swift aplikacji. Jeśli tak, problem jest najprawdopodobniej częścią systemu Android lub iOS sam, a nie częścią platformy Xamarin.

1. <a name="note-4" />[*^*](#ref-4) Kilka przykładów informacje do uwzględnienia:

    1. Dla błędów występujących podczas kompilowania projektu, należy podać pełną [dane wyjściowe kompilacji diagnostycznych](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) na raport o usterce.
    
    1. Błędy występujące podczas kompilowania lub debugowania projektu systemu iOS w programie Visual Studio, uruchom _Pomoc > Xamarin > Zip dzienniki_ po osiągnięcia błędu oraz zawierać wynikowy plik zip na raport o usterce.
    
    1. Wyjątki lub awarie w aplikacjach systemu Android lub iOS, podaj odpowiednią "[debugowania dzienniki dla aplikacji platformy Xamarin.Android i Xamarin.iOS](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)."

1. <a name="note-5" />[*^*](#ref-5) Jeśli to możliwe dla konkretnego problemu, jedną z opcji doskonałą jest odtworzyć problem, dodając niewielką liczbę plików z oryginalnego rozwiązania do zupełnie nowego rozwiązania. Zespół platformy Xamarin często będzie do badania problemów, nawet w przypadku większych przypadki testowe (przy założeniu, że wyraźnie opisano kroki prowadzące do odtworzenia), ale najlepiej szansy szybko rozwiązać błąd prostsze zapewniają przypadków testowych.


1. <a name="note-6" />[*^*](#ref-6) Jeśli jest _nie_ można odtworzyć problem, dodając niewielką liczbę plików do zupełnie nowego rozwiązania, następnie można zip i Dołącz folder całego rozwiązania dla swojej aplikacji pełny. Usuń `bin`, `obj`, `Components`, i `packages` foldery, aby wprowadzić zip plik mniejszy. (Środowiska IDE i procesem kompilacji będzie zazwyczaj przywracania lub odtworzyć zawartość tych folderów, zgodnie z potrzebami). Można również usunąć dowolną liczbę kodu i zasobów plików z projektu, jak chcesz, tak długo, jak długo wynikowy rozwiązanie nadal pokazuje oryginalnego problemu.

