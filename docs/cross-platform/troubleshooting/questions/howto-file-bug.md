---
title: Kiedy i jak należy I pliku raport o usterce?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: asb3993
ms.author: amburns
ms.openlocfilehash: d5471195b5051725b41b8f28b3959db362297669
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Kiedy i jak należy I pliku raport o usterce?


Plik usterki w śledzenia usterek Bugzilla Xamarin w tym miejscu: [ https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all ](https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all).

## <a name="file-a-bug-if"></a>Plik usterki, jeśli...


Masz zestaw kroków, które uważasz, że inżynierów Xamarin będzie można użyć do odtworzenia problemu, który jest spowodowany przez Xamarin.

LUB

Widoczne objawy ten problem, można opisać dokładnie, zwłaszcza, jeśli można również opisać pewnych okolicznościach dokładne związane z problemem. <sup> [[1]](#note-1)</sup>


## <a name="best-practices-to-help-xamarin-address-bugs-quickly-and-efficiently"></a>Najważniejsze wskazówki ułatwiające Xamarin adresu usterki szybkie i skuteczne


1. <a name="ref-1" />Wyszukiwanie [Bugzilla](https://bugzilla.xamarin.com/query.cgi?format=specific&amp;bug_status=__all__) i sieci web dla istniejących usterek raportów lub sugestie dotyczące użycia, które może rozwiązać problem bezpośrednio.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />Postępuj zgodnie z [usterki zapisywania wskazówki](https://bugzilla.xamarin.com/page.cgi?id=bug-writing.html) opisujący problem wyraźnie i zwięzłym, jak to możliwe, łącznie z opisem co się stało i został prawidłowo niepożądane.

1. <a name="ref-3" />Obejmują wszystkie dane śledzenia stosu istotne, tekst komunikatu o błędzie lub wszystkie dzienniki awarii. <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />Zanotuj wszelkie istotne komunikaty o błędach w załączników zrzut ekranu jako zwykły tekst zbyt.

1. <a name="ref-5" />To mały, niezależny przypadek testowy, która powoduje wygenerowanie błędu z jako mały kod, jak to możliwe.  Jeśli nie można odtworzyć problem z całkiem nowego projektu (utworzone przy użyciu jednej z wbudowanych szablonów), proszę zip projekt, który demonstruje problemu i dołączenie go do raportu o usterce.  Przykładowy projekt upewnij się, wystarczy przed dołączeniem go. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />Opisz środowisko, w którym ten błąd napotkano, łącznie z systemu operacyjnego i [wersji programu Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) i wszelkie zależności.

---

## <a name="additional-details"></a>Dodatkowe szczegóły

1. <a name="note-1" />[*^*](#ref-1) W idealnym przypadku opis "widoczne objawy" Uwzględnij za mało informacji, aby innych klientów można potwierdzić, czy występuje ten sam problem (tego samego komunikaty o błędach, tym samym degradacji wydajności, tej samej śladu stosu z awarii, _itp._ ). "Konkretnych okoliczności" dobrym przykładem będzie Jeśli można powiedzieć wyglądać mniej więcej tak: "I zwykle problem 75 procent trafień czasu, ale zmiana ta rzecz następnie można uniknąć problem całkowicie". Inny przykład podobne "dokładne okoliczności" to, czy powrót do poprzedniej wersji programu Xamarin subskrypcji zatrzymuje problem.

1. <a name="note-2" />[*^*](#ref-2) Jak można oczekiwać fragmentów tekstu błędu (lub dowolny tekst opisowy jednoznacznie) są zazwyczaj najlepszym terminy wyszukiwania. Jeśli istniejący raport błędów jest niekompletna, to Zapraszamy dodawanie szczegółów lub pliku nowy, lepiej usterek — raport.

1. <a name="note-3" />[*^*](#ref-3) Następne pytanie dobrej jest, czy ten sam problem został zgłoszony dla dowolnego języka Java, Objective-C lub Swift aplikacji. Jeśli tak, problem jest najprawdopodobniej część Android lub iOS sam zamiast część Xamarin.

1. <a name="note-4" />[*^*](#ref-4) Kilka przykładów informacje obejmują:

    1. Dla błędów występujących podczas kompilowania projektu, podaj pełną [danych wyjściowych kompilacji diagnostycznych](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) na raport o usterce.
    
    1. Błędów występujących podczas kompilowania lub debugowania projektu systemu iOS w programie Visual Studio, uruchom _Pomoc > Xamarin > Zip dzienniki_ po naciśnięcie błąd i obejmują wynikowy plik zip na raport o usterce.
    
    1. Dla wyjątków lub awarie w aplikacjach systemu Android i iOS, podaj odpowiednie "[debugowania dzienniki dla aplikacji platformy Xamarin.Android i Xamarin.iOS](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)."

1. <a name="note-5" />[*^*](#ref-5) Jeśli to możliwe dla konkretnego problemu, co doskonałą opcją jest odtworzyć problem, dodając niewielkiej liczby plików z oryginalnego rozwiązania do zupełnie nowego rozwiązania. Zespół Xamarin często będą mogli do badania problemów, nawet w przypadku większych przypadków testowych (przy założeniu, że wyraźnie opisano kroki umożliwiające odtworzenie), ale umożliwiają łatwiejsze przypadków testowych najlepiej szansy szybkie rozwiązanie usterki.


1. <a name="note-6" />[*^*](#ref-6) Jeśli jest _nie_ możliwe do odtworzenia problemu przez dodanie małą liczbę plików do zupełnie nowego rozwiązania, następnie można zip w i dołączyć folder całego rozwiązania pełnej aplikacji. Usuń `bin`, `obj`, `Components`, i `packages` folderów Aby zip plik mniejszy. (IDE i proces kompilacji będzie zazwyczaj przywrócić lub ponownie Utwórz zawartość tych folderów zgodnie z potrzebami.) Można również usunąć tyle kodu i zasobów plików z projektu jak, tak długo, jak wynikowy rozwiązanie nadal pokazuje oryginalnego problemu.

