---
title: Powiązanie, rozwiązywanie problemów
description: W tym przewodniku opisano, co zrobić, jeśli masz trudności powiązanie biblioteki języka Objective-C.
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: 7ea3e3802ec2e0baf0fe8355a41e806bacabc9ac
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="binding-troubleshooting"></a>Powiązanie, rozwiązywanie problemów

Wskazówki dotyczące rozwiązywania problemów powiązania z macOS (wcześniej znane jako OS X) interfejsów API w Xamarin.Mac.

## <a name="missing-bindings"></a>Brak powiązania

Podczas Xamarin.Mac obejmuje wiele interfejsów API firmy Apple, czasami trzeba wywołać interfejs API niektóre firmy Apple, która nie ma powiązania jeszcze. W innych przypadkach należy wywołać innej Objective-C-C jego poza zakresem Xamarin.Mac powiązania.

Jeśli użytkownik ma do czynienia z interfejsu API firmy Apple, pierwszym krokiem jest let Xamarin wiedzieć, że czy osiągnięto sekcji interfejsu API, które nie są dostępne dla jeszcze pokrycia. [O usterce](#reporting-bugs) zauważyć brak interfejsu API. Używamy raporty od klientów interfejsów API, które pracujemy w następnej kolejności priorytetów. Ponadto, jeśli masz licencji firmy lub organizacji i braku powiązanie blokuje postęp, również postępuj zgodnie z instrukcjami w [Obsługa](http://xamarin.com/support) do pliku biletu. Nie można Obiecujemy, że powiązanie, ale w niektórych przypadkach firma Microsoft może ułatwiające rozpoczęcie pracy.

Po powiadomieniu Xamarin (jeśli dotyczy) z powiązania Brak, następnym krokiem jest należy wziąć pod uwagę powiązanie samodzielnie. Mamy pełny przewodnik [tutaj](~/cross-platform/macios/binding/overview.md) i części dokumentacji nieoficjalny [tutaj](http://brendanzagaeski.appspot.com/xamarin/0002.html) zawijania powiązania Objective-C ręcznie. Jeśli wywołujesz interfejs API C można użyć C# dla mechanizmu P/Invoke, dokumentacja jest [tutaj](http://www.mono-project.com/docs/advanced/pinvoke/).

Jeśli zdecydujesz się działać w powiązaniu samodzielnie, należy pamiętać, że błędów w powiązaniu może utworzyć szerokiej gamy interesujące awarii w natywnym środowisku uruchomieniowym. W szczególności należy zwrócić szczególną uwagę na zgodność podpisu w języku C# natywnego podpisu liczbą argumentów, a rozmiar każdej argumentu. Błąd w tym celu może spowodować uszkodzenie pamięci i/lub stosu i można w przyszłości awarii natychmiast, albo w pewnym momencie dowolnego lub uszkodzenie danych.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>Argument wyjątków podczas przekazywania wartości null do powiązania

Xamarin działa w celu zapewnienia wysokiej jakości i przetestowany również powiązania dla interfejsów API firmy Apple, czasami błędów i usterek dostawy w. Interfejs API jest najbardziej typowe problemy, które mogą zostać uruchomione na zgłaszanie `ArgumentNullException` gdy przekazujesz wartość null podczas akceptuje podstawowego interfejsu API `nil`. Pliki nagłówkowe macierzystego, często Definiowanie interfejsu API nie zapewniają wystarczających informacji, na którym interfejsów API zaakceptować nil i które ulegnie awarii, w przypadku przekazania.

Po uruchomieniu do sprawę w przypadku, gdy przekazując `null` zgłasza `ArgumentNullException` , ale uważasz, że powinien on działać, wykonaj następujące kroki:

1. Sprawdź w dokumentacji firmy Apple i/lub przykłady, aby zobaczyć, czy można znaleźć dowód, że akceptuje `nil`. Jeśli potrafisz Objective-C, można napisać program teście małych ją zweryfikować.
2. [O usterce](#reporting-bugs).
3. Można obejść usterki? Jeśli można uniknąć wywołanie interfejsu API z `null`, proste sprawdzanie wartości null wokół wywołania można łatwo obejść.
4. Jednak niektóre funkcje interfejsu API wymaga przekazywanie wartość null, włączyć lub wyłączyć funkcje. W takich przypadkach można obejść problem przełączając przeglądarkę zestawu (zobacz [Wyszukiwanie elementu członkowskiego C# dla danego selektora](~/mac/app-fundamentals/mac-apis.md#finding_selector)), kopiowanie powiązania i usuwanie sprawdzania wartości null. Upewnij się, o usterce (krok 2) Jeśli to zrobisz, jak skopiowany wiązania nie będziesz otrzymywać aktualizacje i poprawki, wchodzące w Xamarin.Mac i to należy traktować jako krótkoterminowa obejść.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>Raportowanie błędów

Twoja opinia jest dla nas ważna. Jeśli napotkasz jakiekolwiek problemy z Xamarin.Mac:

- Sprawdź [fora Xamarin.Mac](https://forums.xamarin.com/categories/mac)
- Wyszukiwanie [repozytorium problem](https://github.com/xamarin/xamarin-macios/issues) 
- Przed przełączeniem na problemy z GitHub, Xamarin problemy zostały śledzone na [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Wyszukaj istnieje, do dopasowania problemy.
- Jeśli nie można odnaleźć pasującego problem, plik nowy problem w [repozytorium GitHub problem](https://github.com/xamarin/xamarin-macios/issues/new).

Problemy z usługi GitHub są wszystkie publiczne. Nie jest możliwe Ukryj komentarze lub załączników. 

Dołącz tyle jedną z następujących możliwości:

- Prosty przykład odtworzenia problemu. Jest to **nieocenione** w miarę możliwości. 
- Ślad stosu pełne awarii (Crash).
- Kod C# otaczającego tej awarii. 
