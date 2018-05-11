---
title: Historia wersji
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 9a29b131af706b9dedc808a156cdfaffa4173882
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="release-history"></a>Historia wersji

## <a name="34-october-11-2017"></a>3.4 (11 października 2017 r.)

[Pobierz v3.4.0](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Obsługa Xcode 9: 11, system macOS 10.13, systemu tvOS 11, iOS i watchOS 4
* Teraz należy ustalić problemów z SIMD i tgmath
* Telemetrii została całkowicie usunięta

## <a name="33-august-3-2016"></a>3.3 (3 sierpnia 2016)

[Pobierz v3.3.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Obsługa Xcode 8 w wersji Beta 4, iOS 10, macOS 10.12, systemu tvOS 10 oraz watchOS 3.
* Zaktualizowany do najnowszej Clang głównego kompilacji (2016-08-02)
* [Utrwalanie opcji przesyłania danych telemetrycznych](https://twitter.com/Symbiatch/status/760373403878559744) z `sharpie pod bind` do `sharpie bind`.

## <a name="32-june-14-2016"></a>3.2 (14 czerwca 2016 r.)

[Pobierz v3.2.3](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Obsługa Xcode 8 Beta 1, 10 systemu iOS, system macOS 10.12 systemu tvOS 10 oraz watchOS 3.

## <a name="31-may-31-2016"></a>3.1 (31 maj 2016)

[Pobierz v3.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* Obsługa programu CocoaPods 1.0
* Tworzenie pierwszej natywny znacznie poprawia niezawodność powiązania programu CocoaPods `.framework` , a następnie powiązanie
* Skopiuj programu CocoaPods `.framework` i powiązanie definicji do `Binding` katalogu, aby ułatwić integrację z projektów powiązania Xamarin.iOS i Xamarin.Mac

## <a name="30-october-5-2015"></a>3.0 (5 października 2015)

[Pobierz v3.0.8](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Obsługa nowych funkcji języka Objective-C tym lekkie ogólne i dopuszczanie wartości null, wprowadzonego w programie Xcode 7
* Obsługa najnowsze iOS i Mac zestawów SDK.
* Projekt Xcode, kompilowania i analizowania obsługi! Teraz można przekazać pełne projektu Xcode do Sharpie cel i najlepiej będzie wykonywał Aby dowiedzieć się, co zrobić (np. `sharpie bind Project.xcodeproj -sdk ios`).
* [Programu CocoaPods](https://cocoapods.org) obsługuje! Teraz można skonfigurować, kompilacji i powiązać bezpośrednio z celem Sharpie programu CocoaPods (np. `sharpie pod init ios AFNetworking && sharpie pod bind`).
* Podczas tworzenia wiązania struktur, moduły Clang zostanie włączona, jeśli platformę je obsługuje, co powoduje najbardziej poprawną analizę platformy, ponieważ struktura Framework jest definiowana za pomocą jego `module.modulemap`.
* Dla projektów Xcode, które kompilują produktu framework należy przeanalizować tego produktu, zamiast cele produktu pośredniego jako nieprzeznaczony cele w projekcie Xcode nadal obowiązują niejednoznaczności, które nie mogą zostać automatycznie rozwiązane.

## <a name="216-march-17-2015"></a>2.1.6 (17 marca 2015)

[Pobierz v2.1.6](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* Powiązanie wyrażenia stałej operator binarny: lewa strona wyrażenia został niepoprawnie zamieniona na po prawej stronie (np. `1 << 0` został niepoprawnie powiązany jako `0 << 1`). Dzięki użyciu Adam Kemp dla po raz pierwszy to!
* Rozwiązano problem z `NSInteger` i `NSUInteger` związany jako `int` i `uint` zamiast `nint` i `nuint` na i386; `-DNS_BUILD_32_LIKE_64` teraz jest przekazywana do Clang, aby podczas analizowania `objc/NSObjCRuntime.h` działać zgodnie z oczekiwaniami na i386.
* Architektura domyślne dla systemu Mac OS X SDK (np. `-sdk macosx10.10`) jest teraz x86_64 zamiast i386, więc `-arch` można pominąć, chyba że wymagane jest zastąpienie domyślny.

## <a name="210-march-15-2015"></a>2.1.0 (15 marca 2015)

[Pobierz v2.1.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxc #27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): Upewnij się, `using ObjCRuntime;` jest generowany po `ArgumentSemantic` jest używany.
* [bxc #27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): Upewnij się, `using System.Runtime.InteropServices;` jest generowany po `DllImport` jest używany.
* [bxc #27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): domyślna `DllImport` do ładowania symboli z `__Internal`.
* [bxc #27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): Pomiń zadeklarowany do przodu deklaracje kontenera Objective-C.
* [bxc #27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): powiązania protokołu typy z jednym kwalifikacji jako konkretnych interfejsów (`id<Foo>` jako `Foo` zamiast `Foundation.NSObject<Foo>`).
* [bxc #28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): powiązać `UInt32`, `UInt64`, i `Int64` literałów jako `Int32` można porzucić `u` i/lub `uL` sufiksy po wartości bezpiecznie można umieścić w `Int32`.
* [bxc #28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): Usuń wyliczenia mapowania nazw podczas oryginalnej nazwy natywnego rozpoczyna się od `k` prefiks.
* `sizeof` Wyrażenia języka C, którego typ argumentu nie jest zmapowana na typ pierwotny C# zostanie obliczone w Clang i powiązany jako literał całkowity, aby uniknąć generowania nieprawidłowy C#.
* Popraw składnię języka Objective C dla właściwości, których typ to blok (kod języka Objective C jest wyświetlana w komentarzach powyżej deklaracji powiązanej).
* Powiązanie zbutwiałe typy jako ich oryginalny typ (`int[]` decays do `int*` podczas analizy semantyczne w Clang, ale powiązać go jako oryginalny zapisane jako `int[]` zamiast).

Dzięki znacznie Dave Dunkin wiele z usterek stałym w tej wersji punkt raportowania!

## <a name="200-march-9-2015"></a>2.0.0: 9 marca 2015 roku.

[Pobierz v2.0.0](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

Celu Sharpie 2.0 jest głównej wersji, która oferuje udoskonalone sterowników na podstawie Clang i analizatora oraz nowy aparat wiązania na podstawie NRefactory. Podanie tych składników ulepszone _znacznie_ lepsze powiązań, szczególnie wokół powiązania typu. Wprowadzono wiele innych ulepszeń, które są wewnętrzne Sharpie cel, która umożliwia uzyskanie wiele funkcji widoczny dla użytkownika w przyszłych wersjach.

Celu Sharpie 2.0 jest oparta na Clang 3.6.1.

### <a name="type-binding-improvements"></a>Ulepszenia powiązania typu

* Bloki Objective-C są teraz obsługiwane. W tym anonimowego/wbudowanego bloków i bloki o nazwie za pośrednictwem `typedef`. Bloki anonimowe zostanie powiązany jako `System.Action` lub `System.Func` deleguje podczas nazwanego bloki zostanie powiązany jako silnej nazwy `delegate` typów.

* Brak ulepszone heurystyki nazewnictwa dla anonimowego wyliczenia, które są od razu poprzedzone `typedef` rozpoznawania na typ całkowity wbudowanej funkcji, takich jak `long` lub `int`.

* Wskaźniki C są teraz powiązane języka C# `unsafe` wskaźniki zamiast `System.IntPtr`. Powoduje to więcej jasności powiązanie dla Kiedy warto włączyć parametry wskaźnika do `out` lub `ref` parametrów. Nie jest możliwe zawsze rozpoznać, czy parametr powinien być `out` lub `ref`, więc wskaźnika są przechowywane w powiązaniu, aby umożliwić łatwiejsze inspekcji.

* Wyjątek do powyższej powiązania wskaźnika jest po napotkaniu rangę 2 wskaźnik do obiektu języka Objective-C jako parametr. W takich przypadkach Konwencji jest dominujący i będzie można powiązać parametr jako `out` (np. `NSError **error` → `out NSError error`).

### <a name="verify-attribute"></a>Sprawdź atrybut

Dostępne są często, że powiązania utworzonego przez Sharpie cel teraz adnotacją w `[Verify]` atrybutu. Te atrybuty wskazują, że należy _Sprawdź_ czy Sharpie cel została poprawny element porównując powiązania z oryginalnej deklaracji Objective-C-C (która będzie dostępna w komentarz powyżej deklaracji powiązanej).

Weryfikacja jest _zalecane_ dla wszystkich deklaracji powiązania, ale najprawdopodobniej _wymagane_ dla deklaracji opatrzoną `[Verify]` atrybutu. Jest tak, ponieważ w wielu sytuacjach, nie ma wystarczającej ilości metadanych w oryginalny kod źródłowy natywnej do wywnioskowania jak najlepiej utworzyć wiązanie. Konieczne może odwoływać się do dokumentacji lub komentarze w kodzie wewnątrz pliki nagłówkowe najlepszych decyzji powiązania.

Zobacz [Sprawdź atrybuty](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) dokumentację, aby uzyskać więcej szczegółowych informacji.

### <a name="other-notable-improvements"></a>Inne ulepszenia zauważalne

* `using` instrukcje teraz są generowane na podstawie typów powiązany. Na przykład jeśli `NSURL` została powiązana `using Foundation;` również będzie można wygenerować instrukcji.

* `struct` i `union` deklaracje będzie teraz powiązać, przy użyciu `[FieldOffset]` lewy dla Unii.

* Wartości wyliczenia z inicjatorami wyrażenie stałe zostanie teraz powiązany prawidłowo; pełne wyrażenie jest przetłumaczony na język C#.

* Metody ze zmienną liczbą argumentów i bloki są teraz powiązane.

* Struktury są teraz obsługiwane za pośrednictwem `-framework` opcji. Zobacz dokumentację [powiązanie natywnego struktur](http://developer.xamarin.com/guides/ios/advanced_topics/binding_objective-c/objective_sharpie/#frameworks) więcej szczegółów.

* Kodu źródłowego języka Objective-C będzie wykrywane automatycznie, której należy, eliminując konieczność do przekazania `-ObjC` lub `-xobjective-c` do Clang ręcznie.

* Clang użycie modułu (`@import`) jest wykrywane automatycznie, który należy wyeliminować konieczność przekazywania `-fmodules` do Clang ręcznie dla bibliotek, które używają nowa funkcja obsługi modułu w Clang.

* Interfejsu API Unified Xamarin jest teraz cel wiązania domyślne; Użyj `-classic` opcji pod kątem 32-bitowych tylko klasycznego interfejsu API.

### <a name="notable-bug-fixes"></a>Ważne poprawki błędów

* Usuń `instancetype` powiązania używanego w kategorii Objective-C
* Pełni nazwy kategorii Objective-C
* Prefiks protokoły języka Objective-C z `I` (np. `INSCopying` zamiast `NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35: 21 grudnia 2014 r.

[Pobierz v1.1.35](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

Niewielkie poprawki błędów.

## <a name="111-december-15-2014"></a>1.1.1: 15 grudnia 2014 r.

[Pobierz v1.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 został pierwszej wersji głównych po półtora roku użytku wewnętrznego i rozwój w Xamarin po początkowej wersji zapoznawczej Sharpie cel w kwietnia 2013. Ta wersja jest pierwszą osobą, która zazwyczaj wziąć pod uwagę stabilny i można go użyć w wielu różnych natywnych bibliotek, oferujący funkcje nowego zaplecza Clang.

