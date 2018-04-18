---
title: Osadzanie błędów .NET
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/11/2018
ms.openlocfilehash: f9563768a5a2fba31da3eb343b093c9bfaea3069
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="net-embedding-errors"></a>Osadzanie błędów .NET

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: Komunikaty o błędach powiązanie

Np. Parametry, środowiska

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000" />

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: Wystąpił nieoczekiwany błąd - wypełnij raport o usterce w https://github.com/mono/Embeddinator-4000/issues

Wystąpił nieoczekiwany błąd. Sprawdź [pliku problemu](https://github.com/mono/Embeddinator-4000/issues) jak najwięcej informacji jak to możliwe, w tym:

* Pełnej kompilacji dzienników z maksymalnej szczegółowości
* Minimalny przypadek testowy, który występuje błąd
* Wszystkie informacje w wersji

Najprostszym sposobem, aby uzyskać informacje o wersji dokładnie jest użycie **Xamarin Studio** menu **Xamarin Studio** elementu **Pokaż szczegóły** przycisk i kopiowania/wklejania wersji informacje (można użyć **kopiowanie informacji** przycisku).

<a name="EM0001" />

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: Nie można utworzyć katalogu wyjściowego `X`

Podana nazwa katalogu przez `-o=DIR` nie istnieje i nie można go utworzyć. Może być nieprawidłowa nazwa dla systemu plików.

<a name="EM0002" />

### <a name="em0002-option-x-is-not-supported"></a>EM0002: Opcja `X` nie jest obsługiwane

Narzędzie nie obsługuje opcji `X`. Istnieje możliwość, że inna wersja narzędzia obsługuje go lub go nie ma zastosowania w tym środowisku.

<a name="EM0003" />

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: Platforma `X` jest nieprawidłowy.

Narzędzie nie obsługuje platformy `X`. Istnieje możliwość, że inna wersja narzędzia obsługuje go lub go nie ma zastosowania w tym środowisku.

<a name="EM0004" />

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: Element docelowy `X` jest nieprawidłowy.

Narzędzie nie obsługuje docelowej `X`. Istnieje możliwość, że inna wersja narzędzia obsługuje go lub go nie ma zastosowania w tym środowisku.

<a name="EM0005" />

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: Element docelowy kompilacji `X` jest nieprawidłowy.

Narzędzie nie obsługuje docelowej kompilacja `X`. Istnieje możliwość, że inna wersja narzędzia obsługuje go lub go nie ma zastosowania w tym środowisku.

<a name="EM0006" />

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: Nie można odnaleźć lokalizacji Xcode.

Narzędzia nie można znaleźć, w obecnie zaznaczonej Xcode lokalizacji przy użyciu `xcode-select -p` polecenia. Sprawdź, czy to polecenie zakończy się pomyślnie i zwraca poprawnej lokalizacji Xcode.

<a name="EM0007" />

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: Nie można pobrać wersji zestawu sdk dla {sdk}.

Narzędzia nie można uzyskać za pomocą wersji zestawu SDK `xcrun --show-sdk-version --sdk {sdk}` polecenia. Sprawdź, czy to polecenie zakończy się pomyślnie i zwraca informacje o wersji zestawu SDK.

<a name="EM0008" />

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: Architektura {arch} jest nieprawidłowy dla {platformy}. Są prawidłowe architektury {platformy}: {architektury}.

Architektura w komunikacie o błędzie jest nieprawidłowa dla platformy docelowej. Upewnij się, że opcja--abi została przekazana nieprawidłowa architektura.

<a name="EM0009" />

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: Funkcja `X` nie jest obecnie zaimplementowana przez generator

Jest to znany problem, który mamy zamierzają naprawić w przyszłym wydaniu generatora. Udziały są powitalnej.

<a name="EM0010" />

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: Nie można scalić struktury {simulatorFramework} i {deviceFramework}, ponieważ plik "{}" istnieje zarówno w elemencie.

Narzędzia nie można scalić struktur wymienionych w komunikacie o błędzie, ponieważ jest plikiem wspólnej między nimi.

Może to wskazywać na błąd w Embeddinator 4000; Raport o usterce w pliku [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) z przypadkiem testowym.

<a name="EM0011" />

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011: Zestaw `X` nie istnieje.

Narzędzia nie można odnaleźć zestawu `X` określonych w argumentach.

<a name="EM0012" />

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: Nazwa zestawu `X` nie jest unikatowy.

Więcej niż jednym zestawie dostarczony mają nazwę tej samej, wewnętrzny i nie będzie możliwe odróżnić je w czasie wykonywania.

Najbardziej prawdopodobną przyczyną jest to, że zestaw jest określona więcej niż raz w argumenty wiersza polecenia. Jednak nie przechowuje nadal zmienioną nazwę zestawu, oryginalną nazwę i wiele kopii współdziała.

<a name="EM0013" />

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013: Nie można znaleźć zestawu "X", "Y" odwołuje się.

Narzędzia nie można odnaleźć zestawu "X" odwołuje się zestaw "Y". Upewnij się, wszystkie zestawy występujące w odwołaniach znajdują się w tym samym katalogu co zestaw może być powiązane.

<a name="EM0014" />

### <a name="em0014-could-not-find-product-product-minversion-is-required"></a>EM0014: Nie można odnaleźć {produktu} ({produktu} {min_version} jest wymagane).

Nie można odnaleźć zależności wymienionych w komunikacie o błędzie w systemie.

<a name="EM0015" />

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-minversion-is-required"></a>EM0015: Nie można odnaleźć prawidłowej wersji {produktu} ({Wersja} odnaleziona, ale co najmniej {min_version} jest wymagane).

Zależność wspomnianego błąd komunikat został znaleziony w systemie, ale jest zbyt stary. Zaktualizuj do nowszej wersji.

<a name="EM0016" />

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016: Nie można utworzyć łącza symbolicznego '{Plik}' -> "{target}": błąd {number}

Nie można utworzyć łącza symbolicznego, wymienione w komunikacie o błędzie.

<a name="EM0026" />

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>Nie można EM0026 przeanalizować argument wiersza polecenia "A": *

Składnia dla opcji wiersza polecenia `A` nie może być analizowana przez narzędzie. Prawdopodobnie niepoprawne, skontaktuj się z dokumentacją lub pomocy poprawną składnię.

<a name="EM0099" />

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099: Błąd wewnętrzny *. Raport o usterce z przypadkiem testowym pliku (https://github.com/mono/Embeddinator-4000/issues).

Ten komunikat o błędzie jest zgłaszany, gdy wewnętrzne sprawdzenie spójności w Embeddinator 4000 nie powiodło się.

To wskazuje na usterkę w Embeddinator 4000; Raport o usterce w pliku [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) z przypadkiem testowym.

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: Przetwarzanie kodu

<a name="EM1010" />

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: Wpisz `T` nie jest generowany, ponieważ `X` nie są obsługiwane.

To **ostrzeżenie** który typ `T` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ używa `X`, funkcji, która nie jest obsługiwana.

Uwaga: Obsługiwane funkcje rozpoczyna się do nowych wersji narzędzia.

<a name="EM1011" />

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: Wpisz `T` nie jest generowany, ponieważ brakuje w nim kierowania kodu za pomocą natywnego odpowiednik.

Jest to **ostrzeżenie** który typ `T` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ ujawnia on coś z programu .NET framework, która wymaga bardzo kierowania.

Uwaga: Jest to coś, co może pobrać obsługiwany z pewnymi ograniczeniami w przyszłej wersji narzędzia.

<a name="EM1020" />

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: Konstruktor `C` nie jest generowany ze względu na typ parametru `T` nie jest obsługiwane.

Jest to **ostrzeżenie** który konstruktora `C` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ parametr typu `T` nie jest obsługiwana.

Powinien być typu Ostrzeżenie wcześniej, podając więcej informacji, dlaczego `T` nie jest obsługiwane.

Uwaga: Obsługiwane funkcje rozpoczyna się do nowych wersji narzędzia.

<a name="EM1021" />

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: Konstruktor `C` zawiera wartości domyślne, dla których nie otoki jest generowany.

Jest to **ostrzeżenie** który domyślne parametry konstruktora `C` nie generują żadnych dodatkowego kodu. Najczęstszą przyczyną jest, że istniejącą metodę już ma tę samą sygnaturę. Np. w środowisku .net jest możliwe:

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

W takich przypadkach tylko dwa wygenerowany `init` selektorów zostanie utworzony, zarówno wywołanie mono, ale będą istnieć nie otoki dla późniejszej.

<a name="EM1030" />

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: Metoda `M` nie jest generowany, ponieważ typ zwracany `T` nie jest obsługiwane.

Jest **ostrzeżenie** który metody `M` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ jest typem zwracanym `T` nie jest obsługiwana.

Powinien być typu Ostrzeżenie wcześniej, podając więcej informacji, dlaczego `T` nie jest obsługiwane.

Uwaga: Obsługiwane funkcje rozpoczyna się do nowych wersji narzędzia.

<a name="EM1031" />

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: Metoda `M` nie jest generowany ze względu na typ parametru `T` nie jest obsługiwane.

To **ostrzeżenie** który metody `M` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ parametr typu `T` nie jest obsługiwana.

Powinien być typu Ostrzeżenie wcześniej, podając więcej informacji, dlaczego `T` nie jest obsługiwane.

Uwaga: Obsługiwane funkcje rozpoczyna się do nowych wersji narzędzia.

<a name="EM1032" />

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: Metoda `M` zawiera wartości domyślne, dla których nie otoki jest generowany.

Jest to **ostrzeżenie** który domyślne parametry metody `M` nie generują żadnych dodatkowego kodu. Najczęstszą przyczyną jest, że istniejącą metodę już ma tę samą sygnaturę. Np. w środowisku .net jest możliwe:

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

W takich przypadkach tylko dwa wygenerowany `increment` selektorów zostanie utworzony, zarówno wywołanie mono, ale będą istnieć nie otoki dla późniejszej.

<a name="EM1033" />

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: Metoda `M` nie jest generowany, ponieważ inna metoda przedstawia operator o przyjaznej nazwie.

Jest to **ostrzeżenie** która metoda `M` nie jest generowany, ponieważ inna metoda przedstawia operator o przyjaznej nazwie. (https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx)

<a name="EM1034" />

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: Metody rozszerzenia `M` nie jest generowany wewnątrz kategorii, ponieważ nie można utworzyć na typ pierwotny `T`. To normalne, statycznej metody został wygenerowany.

Jest to **ostrzeżenie** tej metody rozszerzenia na primivite wpisz (np. `System.Int32`) został znaleziony. W ObjC nie jest możliwe utworzenie kategorii na typ pierwotny. Zamiast tego generatora powoduje wygenerowanie to normalne, statycznej metody.

<a name="EM1040" />

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: Właściwość `P` nie jest generowany ze względu na typ parametru `T` nie jest obsługiwane.

To **ostrzeżenie** który właściwość `P` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ narażonych typu `T` nie jest obsługiwane.

Powinien być typu Ostrzeżenie wcześniej, podając więcej informacji, dlaczego `T` nie jest obsługiwane.

Uwaga: Obsługiwane funkcje rozpoczyna się do nowych wersji narzędzia.

<a name="EM1041" />

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041: Właściwości indeksowanych na `T` nie jest generowany, ponieważ wiele właściwości indeksowane nie są obsługiwane.

Jest to **ostrzeżenie** który właściwości indeksowanych na `T` będą ignorowane (tj. nic nie zostanie wygenerowany) wielu właściwości indeksowane nie są obsługiwane.

<a name="EM1050" />

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: Pole `F` nie jest generowany ze względu na typ pola `T` nie jest obsługiwane.

To **ostrzeżenie** który pole `F` będą ignorowane (tj. nic nie zostanie wygenerowany) ponieważ narażonych typu `T` nie jest obsługiwane.

Powinien być typu Ostrzeżenie wcześniej, podając więcej informacji, dlaczego `T` nie jest obsługiwane.

Uwaga: Obsługiwane funkcje rozpoczyna się do nowych wersji narzędzia.

<a name="EM1051" />

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: Element `E` jest generowany w zamian jako `F` , ponieważ jego nazwa powoduje konflikt z ważne selektor języka objective-c.

Jest to **ostrzeżenie** który element `E` zostanie wygenerowany, zamiast niego jako `F` , ponieważ jego nazwa powoduje konflikt z ważne selektor języka objective-c.

Selektory na [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) ma istotne znaczenie w języku objective c i musi zostać zastąpiona uważnie.

Uwaga: Lista zastrzeżone selektorów rozpoczyna się do nowych wersji narzędzia.

<a name="EM1052" />

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: Element `E` nie jest generowana jego nazwa powoduje konflikt z innymi elementami na tej samej klasy.

Jest to **ostrzeżenie** elementu `E` nie jest generowany, ponieważ jego nazwa powoduje konflikt z innymi elementami na tej samej klasy.

<a name="EM1053" />

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: Docelowa `E` nie jest obsługiwana dla platformy Xamarin.iOS i Xamarin.Mac. Tylko `framework` opcji jest uznawany za obsługiwany i powinna być używana.

Jest to **ostrzeżenie** kierowanych `E` jest uznawany za obsługiwany dla platformy Xamarin.iOS i Xamarin.Mac zastosowań. 

Użycie biblioteki Embeddinator statyczne lub dynamiczne mogą wymagać dodatkowej pracy kroków lub ulepszeń i należy unikać w większości przypadków użycia.

Rozważ usunięcie z `--target` przebieg lub parametr `--target=framework` zamiast tego.

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: Generowanie kodu

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
