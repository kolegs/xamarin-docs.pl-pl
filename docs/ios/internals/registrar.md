---
title: Typ rejestratora
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 9d4d2e5f3e1c2dc1233b466c00dd60340552fed0
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="type-registrar"></a>Typ rejestratora

W tym dokumencie opisano system rejestracji typu, używany przez platformy Xamarin.iOS.

## <a name="registration-of-managed-classes-and-methods"></a>Rejestracja zarządzanej klasy i metody

Podczas uruchamiania zarejestruje Xamarin.iOS:

  - Klasy z [[zarejestrować]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) atrybut klasy Objective-C.
  - Klasy z [[kategorii]](https://developer.xamarin.com/api/type/CRuntime.CategoryAttribute) atrybut jako kategorie Objective-C.
  - Interfejsy o [[Protocol]](https://developer.xamarin.com/api/type/Foundation.ProtocolAttribute/) atrybut jako protokoły, Objective-C.

w poszczególnych członków przypadków z [[eksportowanie]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) atrybutu zostaną wyeksportowane do Objective-C. Dzięki temu klasy zarządzane jako tworzone i zarządzane metod do wywoływania z języka Objective C i sposób metody i właściwości są połączone między world C# i Objective-C, co.

Bardzo prosty przykład jest klasa AppDelegate, które każda aplikacja ma. Zostanie przypominają, że zarządzane metody Main ma wiersz podobny do tego:

    UIApplication.Main (args, null, "AppDelegate");

Ta wartość informuje środowiska uruchomieniowego języka Objective C można utworzyć typu o nazwie "AppDelegate" jako klasa delegata aplikacji.  Ta klasa środowiska uruchomieniowego języka Objective-C dowiedzieć się, jak można utworzyć wystąpienia klasy "AppDelegate" napisane w języku C#, musi być zarejestrowana.

Środowiska wykonawczego platformy Xamarin.iOS zajmie się rejestracji i wewnętrznie, rejestracja może odbywać się całkowicie w czasie wykonywania (rejestracja dynamicznych) lub można zrobić w czasie kompilacji (rejestracja statyczne).  Dynamiczne metody obejmuje za pomocą odbicia przy uruchamianiu, aby znaleźć wszystkie klasy i metody do rejestracji i przekazać te do środowiska wykonawczego języka Objective-C.  Metody statyczne przeprowadzający zestawy używane przez aplikację w czasie kompilacji.  Klasy i metody zarejestrować się w języku Objective-C, a następnie generuje mapa, która jest osadzony w Twojej wartość binarną.  Następnie podczas uruchamiania, możemy Zarejestruj mapy środowiska uruchomieniowego języka Objective-C.

### <a name="categories"></a>Kategorie

Uruchamianie z Xamarin.iOS 8.10 będzie można utworzyć kategorii Objective-C przy użyciu składni języka C#.

Jest to realizowane za pomocą atrybutu [kategorii], określenie typu rozszerzenie jako argument atrybutu.
Poniższy przykład będzie na przykład rozszerzyć NSString:

    [Category (typeof (NSString))]

Każda metoda kategorii jest przy użyciu normalnego mechanizmu eksportowania metod do języka Objective-C, za pomocą atrybutu [eksportu]:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Wszystkie metody zarządzanych rozszerzeń musi być statyczna, ale można utworzyć metody wystąpienia Objective-C przy użyciu standardowej składni metody rozszerzenia w języku C#:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

i pierwszy argument dla metody rozszerzenia będą wystąpienia, na którym została wywołana metoda.

Pełny przykład:

    [Category (typeof (NSString))]
    public static class MyStringCategory
    {
        [Export ("toUpper")]
        static string ToUpper (this NSString self)
        {
            return self.ToString ().ToUpper ();
        }
    }

W tym przykładzie spowoduje dodanie metody wystąpienia toUpper natywnej do klasy NSString, która może być wywoływany z Objective-C.

    [Category (typeof (UIViewController))]
    public static class MyViewControllerCategory
    {
        [Export ("shouldAutoRotate")]
        static bool GlobalRotate ()
        {
            return true;
        }
    }

### <a name="protocols"></a>protokoły

Począwszy od platformy Xamarin.iOS 8.10 interfejsów z atrybutem [Protocol] zostaną wyeksportowane do języka Objective-C jako protokołów.

Przykład:

    [Protocol ("MyProtocol")]
    interface IMyProtocol
    {
        [Export ("method")]
        void Method ();
    }

    class MyClass : IMyProtocol
    {
        void Method ()
        {
        }
    }

To zostaną wyeksportowane do języka Objective-C jako protokół (Mój_protokół) i klasy (MyClass), który implementuje ten protokół.

 **Dynamiczne rejestracji**

jest używany dla kompilacji w symulatorze, jako jego przyspiesza cyklu kompilacji/debug.  Jest to wynik wyeliminowanie kroki generuje mapowania klasy i kompilacji tej tabeli mapy do uruchamiania Twojej aplikacji za każdym razem musisz uruchomić aplikację i zamiast tego Uruchom ogólnego przeznaczenia jest używana zawsze.  Komputer stacjonarny ma wystarczająco dużo możliwość wykonywania środowiska uruchomieniowego skanowanie klas szybko, więc wydajności jest nigdy nie wystąpił problem.

 **Statyczne rejestracji**

jest przeznaczony dla kompilacji urządzenia jako urządzenia przenośne są wolniej niż komputerów stacjonarnych i wykonywania środowiska uruchomieniowego skanowanie jest powolne.  Ponieważ urządzenia kompilacje zawsze jest konieczne do utworzenia nowego pliku binarnego, Cykl kompilacji/debug nie ma wpływu na tworzenie mapowania rejestracji.

## <a name="new-registration-system"></a>Nowy System rejestracji

Począwszy od wersji stabilnej 6.2.6 i wersja beta 6.3.4 dodaliśmy nowe rejestratora statycznych. W 7.2.1 wersji wprowadziliśmy rejestratora nowy domyślny.

Nowy system rejestracji oferuje następujące nowe funkcje:

- Kompiluj czas wykrywania błędów programisty:
    - Dwie klasy rejestrowany o takiej samej nazwie.
    - Więcej niż jednej metody wyeksportowane odpowiedzieć na ten sam selektor



- Można usunąć nieużywane kodu natywnego
    - Nowy system rejestracji doda silne odwołania do kodu, używany w bibliotekach statycznych, umożliwiając natywnego konsolidator, aby usunąć nieużywane kodu natywnego z wynikowego pliku binarnego.
      W powiązania przykładowych na platformie Xamarin większość aplikacji stać się co najmniej 300k mniejsze.

- Obsługa ogólnego podklasy NSObject. Zobacz [ogólne NSObject](~/ios/internals/api-design/nsobject-generics.md) Aby uzyskać więcej informacji. Ponadto nowy system rejestracji będzie przechwytywać nieobsługiwane konstrukcje ogólnych, które wcześniej spowodowałaby losowe działania w czasie wykonywania.

Oto kilka przykładów przechwycony przez nowe registar błędy:

Eksportowanie ten sam selektor więcej niż raz w tej samej klasy.

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo:")]
    void Foo (NSString str);
    [Export ("foo:")]
    void Foo (string str)
}
```

Eksportowanie więcej niż jedną klasę zarządzany o tej samej nazwie Objective-C.

```csharp
[Register ("Class")]
class MyClass : NSObject {}

[Register ("Class")]
class YourClass : NSObject {}
```

Eksportowanie metody ogólne.

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo")]
    void Foo<T> () {}
}
```



Należy pamiętać o nowe rejestratora w kilku kwestiach:
- Niektóre innej biblioteki muszą zostać zaktualizowane do pracy z nowym systemem rejestracji, zobacz sekcję [wymagane modyfikacje poniżej](#required_modifications) więcej szczegółów.
- Wadą krótkoterminowa interfejsu jest również, że Clang, należy użyć, jeśli jest używany w ramach kont (jest to ponieważ nagłówka accounts.h firmy Apple mogą być kompilowane tylko przez Clang). Dodaj <code>--compiler:clang</code> do mtouch dodatkowe argumenty do użycia Clang, jeśli Twoje korzystania z Xcode 4.6 lub starszy (Xamarin.iOS automatycznie wybierze Clang w programie Xcode 5.0 lub nowszej.)

    <li>Jeśli środowisko Xcode 4.6 (lub starszym) jest używany, GCC / G ++ musi zostać wybrany, gdy wyeksportowanego typu nazwy zawierają znaki spoza zestawu ascii (jest to wersja Clang zostały wydane z wersją Xcode 4.6 obsługują znaki spoza zestawu ascii w identyfikatorach kod języka Objective-C). Dodaj <code>--compiler:gcc</code> do mtouch dodatkowe argumenty do użycia GCC.


## <a name="selecting-a-registrar"></a>Wybieranie rejestratora

Możesz wybrać inny Rejestrator przez dodanie jedną z następujących opcji do argumentów dodatkowe mtouch projektu iOS opcje kompilacji:

-  `--registrar:static` : domyślne dla kompilacji urządzenia
-  `--registrar:dynamic` : domyślne dla kompilacji w symulatorze
-  `--registrar:legacystatic` : domyślne dla urządzenia kompilacje do Xamarin.iOS 7.2.1
-  `--registrar:legacydynamic` : domyślne dla symulatora kompilacje do Xamarin.iOS 7.2.1


## <a name="shortcomings-in-the-old-registration-system"></a>Braki w starym systemie rejestracji

Poprzedni system rejestracji ma następujące wady:

-  Nie można było (natywna) statycznych odwołań do języka Objective-C klasy i metody w bibliotekach natywnego innych firm, co oznaczało, że nie poprosimy natywnego konsolidator, aby usunąć kodu natywnego innych firm, które faktycznie nie został użyty (ponieważ wszystkie elementy zostaną usunięte). Jest to spowodowane "-force_load libNative.a" każde powiązanie innych firm musiała zrobić (lub równoważne ForceLoad = true w atrybucie [LinkWith]).
-  Można wyeksportować dwa typy zarządzane o takiej samej nazwie Objective-C, bez ostrzeżenia. Scenariusz rzadkich było utworzenie dwóch klas AppDelegate (w różnych przestrzeniach nazw). W czasie wykonywania jest losowy która z nich została pobrana (w rzeczywistości, który go różna uruchamia aplikację, która nie została jeszcze odbudować - którego wykonano bardzo puzzling i irytujące środowisko debugowania).
-  Można wyeksportować dwie metody o tej samej sygnaturze Objective-C. Jeszcze ponownie której będzie można wywołać z języka Objective-C został losowe (ale ten problem nie został jako wspólne, jak poprzedni, przede wszystkim, ponieważ jedynym sposobem faktycznie wystąpić tej usterki zostało Aby przesłonić metodę zarządzanych unlucky).
-  Zbiór metod, który został wyeksportowany jest nieco inne między kompilacjami dynamiczną i statyczną.
-  Nie działa prawidłowo podczas eksportowania klasy ogólne (dokładne życie ogólnego wykonywane w czasie wykonywania jest losowe skutecznie co powoduje zachowanie nieokreślonej).


 <a name="required_modifications" />


## <a name="new-registrar-required-changes-to-bindings"></a>Nowe rejestratora: Zmiany wymagane w celu powiązania


Istniejących powiązań Objective-C może muszą zostać zaktualizowane do pracy z nowego registar.

Poniżej przedstawiono listę zmian, które trzeba wykonać te czynności.

### <a name="protocols-must-have-the-protocol-attribute"></a>Protokoły musi mieć atrybut [Protocol]

Implementacje protokołów teraz musi mieć atrybut [Protocol] zastosowanych do nich.  Nie zrobisz, spowoduje to błąd macierzysty konsolidatora podobny do tego:

```csharp
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>Selektory musi mieć prawidłową liczbę parametrów

Wszystkie selektory musi wskazywać poprawnie liczbą parametrów.  Poprzednio te błędy zostały zignorowane i może powodować problemy w czasie wykonywania.

Krótko mówiąc liczba dwukropki musi być zgodna z liczbą parametrów.

Na przykład:

-  Brak parametrów: "foo"
-  Jeden parametr: "foo:"
-  Dwa parametry: "foo:parameterName2:"


Niepoprawne używa są następujące:

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>Użyj parametru IsVariadic w pliku eksportu

Funkcje ze zmienną liczbą argumentów musi to powiedzieć w atrybucie ich eksportu (jest to spowodowane selektor jest niewłaściwy o liczbie argumentów, tj. wskaż 2. z góry naruszenia):

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>Należy połączyć istniejące symboli

Nie można powiązać klas, które nie istnieją w natywnej biblioteki.

Próba powiązania klas nie istnieje, zostanie wyświetlony błąd macierzysty konsolidatora.

Zazwyczaj dzieje się tak, gdy powiązanie zarania przez pewien czas i kod macierzysty został zmodyfikowany w tym samym czasie, aby określonej klasy natywnej albo został usunięty lub zmieniono jego nazwę, podczas gdy powiązanie nie zostały zaktualizowane.

