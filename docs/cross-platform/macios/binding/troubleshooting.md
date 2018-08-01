---
title: Powiązanie, rozwiązywania problemów
description: W tym przewodniku opisano, co należy zrobić, jeśli masz trudności z powiązań biblioteki języka Objective-C. W szczególności omówiono w nim brakuje powiązań wyjątków argumentów podczas przekazywania o wartości null do powiązania i raportowania usterek.
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: aaceada84b151856506ede66907274e2457c23d4
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854808"
---
# <a name="binding-troubleshooting"></a>Powiązanie, rozwiązywania problemów

Kilka porad dotyczących rozwiązywania problemów z powiązań do systemu macOS (wcześniej znane jako OS X) interfejsów API na platformie Xamarin.Mac.

## <a name="missing-bindings"></a>Brak powiązania

Gdy Xamarin.Mac obejmuje wiele interfejsów API firmy Apple, czasami może być konieczne wywoływanie interfejsu API niektóre firmy Apple, który nie ma powiązania jeszcze. W innych przypadkach potrzebne do wywoływania innej Objective-C-C jej poza zakresem powiązań platformy Xamarin.Mac.

Jeśli masz do czynienia z interfejsem API firmy Apple, pierwszym krokiem jest pozwala dowiedzieć się, że w przypadku osiągnięcia części interfejsu API, nie mamy pokrycia dla jeszcze platformy Xamarin. [Zgłoś usterkę](#reporting-bugs) zauważyć brak interfejsu API. Używamy raporty od klientów, aby określić priorytety interfejsów API, które będziemy pracować dalej. Ponadto, jeśli masz licencję Business lub Enterprise, a ten brak powiązanie blokuje postęp, również postępuj zgodnie z instrukcjami w [pomocy technicznej](http://xamarin.com/support) do pliku biletu. Nie gwarantujemy powiązania, ale w niektórych przypadkach firma Microsoft może pomóc dzieło.

Po powiadomieniu Xamarin (jeśli dotyczy) Twojej brakujące powiązania, następnym krokiem jest należy wziąć pod uwagę powiązanie samodzielnie. Mamy pełnego przewodnika dotyczącego [tutaj](~/cross-platform/macios/binding/overview.md) i dokumentacji nieoficjalny [tutaj](http://brendanzagaeski.appspot.com/xamarin/0002.html) zawijania powiązań języka Objective-C ręcznie. W przypadku wywołania interfejsu API języka C, można użyć C# w mechanizmu P/Invoke, dokumentacja jest [tutaj](http://www.mono-project.com/docs/advanced/pinvoke/).

Jeśli zdecydujesz się praca w powiązaniu samodzielnie, należy pamiętać, że pomyłki w powiązaniu może tworzyć różnego rodzaju interesujące awarie w macierzystym środowisku uruchomieniowym. W szczególności trzeba zwrócić szczególną uwagę że podpisu w języku C# odpowiadają podpisowi natywne w liczbę argumentów, a rozmiar każdego argumentu. Niewykonanie tej czynności może spowodować uszkodzenie pamięci i/lub stosu i można w przyszłości awarii od razu lub w pewnym momencie dowolnego lub uszkodzenie danych.

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>Argument wyjątki podczas przekazywania o wartości null do powiązania

Natomiast Xamarin działa w celu zapewnienia wysokiej jakości i dobrze przetestowanych powiązania dla interfejsów API firmy Apple, czasami pomyłki i usterek dostawy w. Najbardziej typowym problemem, które możesz napotkać to interfejs API zgłaszanie `ArgumentNullException` gdy są przekazywane w wartości null podczas podstawowy interfejs API akceptuje `nil`. Pliki nagłówkowe natywnych, często Definiowanie interfejsu API nie dostarczać wystarczających informacji, interfejsów API, zaakceptuj wartość zero i, w którym będzie ulec awarii w przypadku przekazania.

Jeśli napotkasz przypadek w przypadku, gdy w `null` zgłasza `ArgumentNullException` , ale Twoim zdaniem powinny działać, wykonaj następujące kroki:

1. Sprawdź dokumentację firmy Apple i/lub przykłady, aby zobaczyć, czy możesz znaleźć dowód, który przyjmuje `nil`. Jeśli masz doświadczenia w języku Objective-C, można napisać program mały test, w celu zweryfikowania.
2. [Zgłoś usterkę](#reporting-bugs).
3. Można obejść usterki? Jeśli należy unikać wywoływania interfejsu API za pomocą `null`, proste sprawdzanie wartości null w całym wywołania można łatwo obejść.
4. Przekazywanie w wartości null, aby wyłączyć lub wyłączyć niektóre funkcje wymagają jednak niektóre interfejsy API. W takich przypadkach można obejść ten problem, przenosząc przeglądarkę zestawu (zobacz [Wyszukiwanie elementu członkowskiego języka C# dla danego selektor](~/mac/app-fundamentals/mac-apis.md#finding_selector)), kopiowanie, powiązanie i usunięcie sprawdzanie wartości null. Upewnij się, Zgłoś usterkę (krok 2), jeśli to zrobisz, jak Twoje skopiowany powiązanie nie będziesz otrzymywać aktualizacje i poprawki, które mamy w platformy Xamarin.Mac i należy to uwzględnić krótkoterminowe obejście.

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>Raportowania błędów

Twoja opinia jest dla nas ważna. Jeśli znajdziesz jakiekolwiek problemy z platformy Xamarin.Mac:

- Sprawdź [fora platformy Xamarin.Mac](https://forums.xamarin.com/categories/mac)
- Wyszukiwanie [repozytorium problem](https://github.com/xamarin/xamarin-macios/issues) 
- Przed przełączeniem na problemy usługi GitHub, był monitorowana problemy dotyczące platformy Xamarin [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Wyszukaj istnieje, dopasowanie problemów.
- Jeśli nie można odnaleźć pasującego problem, zgłoś nowy problem w [repozytorium problem usługi GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

Problemy usługi GitHub są wszystkie publiczne. Nie jest możliwe ukryć, komentarze i załączniki. 

Dołącz jak najwięcej jedną z następujących możliwości:

- Prosty przykład odtworzenia problemu. Jest to **bezcenne** gdzie to możliwe. 
- Pełen ślad stosu pojawienia się awarii.
- Kod C# wokół tej awarii. 

## <a name="related-links"></a>Linki pokrewne

- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
