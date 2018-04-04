---
title: Błędy osadzanie .NET
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 64caaf6610d9f9193a686d91b4731cd4d4953fa6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="em0xxx-binding-error-messages"></a>EM0xxx: komunikaty o błędach powiązania

Np. Parametry, środowiska

<!-- 0xxx: the generator itself, e.g. parameters, environment -->
<h3><a name="EM0000"/>EM0000: Wystąpił nieoczekiwany błąd - wypełnij raport o usterce w https://github.com/mono/Embeddinator-4000/issues</h3>

Wystąpił nieoczekiwany błąd. Sprawdź [pliku problemu](https://github.com/mono/Embeddinator-4000/issues) jak najwięcej informacji jak to możliwe, w tym:

* Pełnej kompilacji dzienników z maksymalnej szczegółowości
* Minimalny przypadek testowy, który występuje błąd
* Wszystkie informacje w wersji

Najprostszym sposobem, aby uzyskać informacje o wersji dokładnie jest użycie **Xamarin Studio** menu **Xamarin Studio** elementu **Pokaż szczegóły** przycisk i kopiowania/wklejania wersji informacje (można użyć **kopiowanie informacji** przycisku).

<h3><a name="EM0001"/>EM0001: Nie można utworzyć katalogu wyjściowego `X`</h3>

Podana nazwa katalogu przez `-o=DIR` nie istnieje i nie można go utworzyć. Może być nieprawidłowa nazwa dla systemu plików.

<h3><a name="EM0002"/>EM0002: Opcja `X` nie jest obsługiwane</h3>

Narzędzie nie obsługuje opcji `X`. Istnieje możliwość, że inna wersja narzędzia obsługuje go lub go nie ma zastosowania w tym środowisku.

<h3><a name="EM0003"/>EM0003: Platforma `X` jest nieprawidłowy.</h3>

Narzędzie nie obsługuje platformy `X`. Istnieje możliwość, że inna wersja narzędzia obsługuje go lub go nie ma zastosowania w tym środowisku.

<h3><a name="EM0004"/>EM0004: Element docelowy `X` jest nieprawidłowy.</h3>

Narzędzie nie obsługuje docelowej `X`. Istnieje możliwość, że inna wersja narzędzia obsługuje go lub go nie ma zastosowania w tym środowisku.

<h3><a name="EM0005"/>EM0005: Element docelowy kompilacji `X` jest nieprawidłowy.</h3>

Narzędzie nie obsługuje docelowej kompilacja `X`. Istnieje możliwość, że inna wersja narzędzia obsługuje go lub go nie ma zastosowania w tym środowisku.

<h3><a name="EM0006"/>EM0006: Nie można odnaleźć lokalizacji Xcode.</h3>

Narzędzia nie można znaleźć, w obecnie zaznaczonej Xcode lokalizacji przy użyciu `xcode-select -p` polecenia. Sprawdź, czy to polecenie zakończy się pomyślnie i zwraca poprawnej lokalizacji Xcode.

<h3><a name="EM0007"/>EM0007: Nie można pobrać wersji zestawu sdk dla {sdk}.</h3>

Narzędzia nie można uzyskać za pomocą wersji zestawu SDK `xcrun --show-sdk-version --sdk {sdk}` polecenia. Sprawdź, czy to polecenie zakończy się pomyślnie i zwraca informacje o wersji zestawu SDK.

<h3><a name="EM0008"/>EM0008: Architektura {arch} jest nieprawidłowy dla {platformy}. Są prawidłowe architektury {platformy}: {architektury}.</h3>

Architektura w komunikacie o błędzie jest nieprawidłowa dla platformy docelowej. Upewnij się, że opcja--abi została przekazana nieprawidłowa architektura.

<h3><a name="EM0009"/>EM0009: Funkcja `X` nie jest obecnie zaimplementowana przez generator</h3>

Jest to znany problem, który mamy zamierzają naprawić w przyszłym wydaniu generatora. Udziały są powitalnej.

<h3><a name="EM0010"/>EM0010: Nie można scalić struktury {simulatorFramework} i {deviceFramework}, ponieważ plik "{}" istnieje zarówno w elemencie.</h3>

Narzędzia nie można scalić struktur wymienionych w komunikacie o błędzie, ponieważ jest plikiem wspólnej między nimi.

Może to wskazywać na błąd w Embeddinator 4000; Raport o usterce w pliku [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) z przypadkiem testowym.

<h3><a name="EM0011"/>EM0011: Zestaw `X` nie istnieje.</h3>

Narzędzia nie można odnaleźć zestawu `X` określonych w argumentach.

<h3><a name="EM0012"/>EM0012: Nazwa zestawu `X` nie jest unikatowy.</h3>

Więcej niż jednym zestawie dostarczony mają nazwę tej samej, wewnętrzny i nie będzie możliwe odróżnić je w czasie wykonywania.

Najbardziej prawdopodobną przyczyną jest to, że zestaw jest określona więcej niż raz w argumenty wiersza polecenia. Jednak nie przechowuje nadal zmienioną nazwę zestawu, oryginalną nazwę i wiele kopii współdziała.

<h3><a name="EM0013"/>EM0013: Nie można znaleźć zestawu "X", "Y" odwołuje się.</h3>

Narzędzia nie można odnaleźć zestawu "X" odwołuje się zestaw "Y". Upewnij się, wszystkie zestawy występujące w odwołaniach znajdują się w tym samym katalogu co zestaw może być powiązane.

<h3><a name="EM0014"/>EM0014: Nie można odnaleźć {produktu} ({produktu} {min_version} jest wymagane).</h3>

Nie można odnaleźć zależności wymienionych w komunikacie o błędzie w systemie.

<h3><a name="EM0015"/>EM0015: Nie można odnaleźć prawidłowej wersji {produktu} ({Wersja} odnaleziona, ale co najmniej {min_version} jest wymagane).</h3>

Zależność wspomnianego błąd komunikat został znaleziony w systemie, ale jest zbyt stary. Zaktualizuj do nowszej wersji.

<h3><a name="EM0016">EM0016: Nie można utworzyć łącza symbolicznego '{Plik}' -> "{target}": błąd {number}</h3>

Nie można utworzyć łącza symbolicznego, wymienione w komunikacie o błędzie.

<h3><a name="EM0026"/>Nie można EM0026 przeanalizować argument wiersza polecenia "A": *</h3>

Składnia dla opcji wiersza polecenia `A` nie może być analizowana przez narzędzie. Prawdopodobnie niepoprawne, skontaktuj się z dokumentacją lub pomocy poprawną składnię.

<h3><a name="EM0099"/>EM0099: Błąd wewnętrzny *. Raport o usterce z przypadkiem testowym pliku (https://github.com/mono/Embeddinator-4000/issues).</h3>

Ten komunikat o błędzie jest zgłaszany, gdy wewnętrzne sprawdzenie spójności w Embeddinator 4000 nie powiodło się.

To wskazuje na usterkę w Embeddinator 4000; Raport o usterce w pliku [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) z przypadkiem testowym.


<!-- 1xxx: code processing -->

# <a name="em1xxx-code-processing"></a>EM1xxx: Przetwarzanie kodu

<h3><a name="EM1010"/>Typ `T` nie jest generowany, ponieważ `X` nie są obsługiwane.</h3>

To **ostrzeżenie** który typ `T` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ używa `X`, funkcji, która nie jest obsługiwana.

Uwaga: Obsługiwane funkcje rozpoczyna się do nowych wersji narzędzia.


<h3><a name="EM1011"/>Typ `T` nie jest generowany, ponieważ brakuje w nim odpowiednikiem macierzystego.</h3>

To **ostrzeżenie** który typ `T` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ używa go Uwidacznianie coś z programu .NET framework, który nie ma odpowiednika w macierzystym platformy.


<h3><a name="EM1011"/>Typ `T` nie jest generowany, ponieważ brakuje w nim kierowania kodu za pomocą natywnego odpowiednik.</h3>

Jest to **ostrzeżenie** który typ `T` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ używa go Uwidacznianie coś z programu .NET framework, która wymaga bardzo przekazywanie.

Uwaga: Jest to coś, co może pobrać obsługiwane z pewnymi ograniczeniami w przyszłej wersji narzędzia.


<h3><a name="EM1020"/>Konstruktor `C` nie jest generowany ze względu na typ parametru `T` nie jest obsługiwane.</h3>

Jest to **ostrzeżenie** który konstruktora `C` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ parametr typu `T` nie jest obsługiwana.

Powinien być typu Ostrzeżenie wcześniej, podając więcej informacji, dlaczego `T` nie jest obsługiwane.

Uwaga: Obsługiwane funkcje rozpoczyna się do nowych wersji narzędzia.


<h3><a name="EM1021"/>Konstruktor `C` zawiera wartości domyślne, dla których nie otoki jest generowany.</h3>

Jest to **ostrzeżenie** który domyślne parametry konstruktora `C` nie generują żadnych dodatkowego kodu. Najczęstszą przyczyną jest, że istniejącą metodę już ma tę samą sygnaturę. Np. w środowisku .net jest możliwe:

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

W takich przypadkach tylko dwa wygenerowany `init` selektorów zostanie utworzony, zarówno wywołanie mono, ale będą istnieć nie otoki dla późniejszej.


<h3><a name="EM1030"/>Metoda `M` nie jest generowany, ponieważ typ zwracany `T` nie jest obsługiwane.</h3>

Jest **ostrzeżenie** który metody `M` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ jest typem zwracanym `T` nie jest obsługiwana.

Powinien być typu Ostrzeżenie wcześniej, podając więcej informacji, dlaczego `T` nie jest obsługiwane.

Uwaga: Obsługiwane funkcje rozpoczyna się do nowych wersji narzędzia.


<h3><a name="EM1031"/>Metoda `M` nie jest generowany ze względu na typ parametru `T` nie jest obsługiwane.</h3>

To **ostrzeżenie** który metody `M` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ parametr typu `T` nie jest obsługiwana.

Powinien być typu Ostrzeżenie wcześniej, podając więcej informacji, dlaczego `T` nie jest obsługiwane.

Uwaga: Obsługiwane funkcje rozpoczyna się do nowych wersji narzędzia.


<h3><a name="EM1032"/>Metoda `M` zawiera wartości domyślne, dla których nie otoki jest generowany.</h3>

Jest to **ostrzeżenie** który domyślne parametry metody `M` nie generują żadnych dodatkowego kodu. Najczęstszą przyczyną jest, że istniejącą metodę już ma tę samą sygnaturę. Np. w środowisku .net jest możliwe:

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

W takich przypadkach tylko dwa wygenerowany `increment` selektorów zostanie utworzony, zarówno wywołanie mono, ale będą istnieć nie otoki dla późniejszej.


<h3><a name="EM1033"/>Metoda `M` nie jest generowany, ponieważ inna metoda przedstawia operator o przyjaznej nazwie.</h3>

Jest to **ostrzeżenie** która metoda `M` nie jest generowany, ponieważ inna metoda przedstawia operator o przyjaznej nazwie. (https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx)


<h3><a name="EM1034"/>Metody rozszerzenia `M` nie jest generowany wewnątrz kategorii, ponieważ nie można utworzyć na typ pierwotny `T`. To normalne, statycznej metody został wygenerowany.</h3>

Jest to **ostrzeżenie** tej metody rozszerzenia na primivite wpisz (np. `System.Int32`) został znaleziony. W ObjC nie jest możliwe utworzenie kategorii na typ pierwotny. Zamiast tego generatora powoduje wygenerowanie to normalne, statycznej metody.



<h3><a name="EM1040"/>Właściwość `P` nie jest generowany ze względu na typ parametru `T` nie jest obsługiwane.</h3>

To **ostrzeżenie** który właściwość `P` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ narażonych typu `T` nie jest obsługiwane.

Powinien być typu Ostrzeżenie wcześniej, podając więcej informacji, dlaczego `T` nie jest obsługiwane.

Uwaga: Obsługiwane funkcje rozpoczyna się do nowych wersji narzędzia.

<h3><a name="EM1041"/>Właściwości indeksowanych na `T` nie jest generowany, ponieważ wiele właściwości indeksowane nie są obsługiwane.</h3>

Jest to **ostrzeżenie** który właściwości indeksowanych na `T` będą ignorowane (tj. nic nie zostanie wygenerowany) wielu właściwości indeksowane nie są obsługiwane.


<h3><a name="EM1050"/>Pole `F` nie jest generowany ze względu na typ pola `T` nie jest obsługiwane.</h3>

To **ostrzeżenie** który pole `F` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ narażonych typu `T` nie jest obsługiwane.

Powinien być typu Ostrzeżenie wcześniej, podając więcej informacji, dlaczego `T` nie jest obsługiwane.

Uwaga: Obsługiwane funkcje rozpoczyna się do nowych wersji narzędzia.

<h3><a name="EM1051"/>Element `E` jest generowany w zamian jako `F` , ponieważ jego nazwa powoduje konflikt z ważne selektor języka objective-c.</h3>

Jest to **ostrzeżenie** który element `E` zostanie wygenerowany, zamiast niego jako `F` , ponieważ jego nazwa powoduje konflikt z ważne selektor języka objective-c.

Selektory na [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) ma istotne znaczenie w języku objective c i musi zostać zastąpiona uważnie.

Uwaga: Lista zastrzeżone selektorów rozpoczyna się do nowych wersji narzędzia.


<!-- 2xxx: code generation -->

# <a name="em2xxx-code-generation"></a>EM2xxx: Generowanie kodu


<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
