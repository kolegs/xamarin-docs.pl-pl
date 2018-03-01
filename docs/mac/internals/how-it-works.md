---
title: "Jak działa Xamarin.Mac"
description: "W tym dokumencie opisano wewnętrzne działanie Xamarin.Mac. W szczególności wygląda konstruktorów, zarządzania pamięcią, wcześniejsze kompilacji czasu i rejestratora."
ms.topic: article
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/25/2017
ms.openlocfilehash: 7329e8ddb5b86adcf6e1efaa805149012be8853c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="how-xamarinmac-works"></a>Jak działa Xamarin.Mac

W większości przypadków dewelopera nigdy nie będzie trzeba się martwić o wewnętrznej "magicznych" z Xamarin.Mac, jednak o nierównej zrozumienia sposobu rzeczy działa pod maską pomoże w interpretowanie istniejąca dokumentacja z obiektyw C# i debugowanie problemów podczas zanim jeszcze wystąpią.

W Xamarin.Mac, aplikacja pomost między dwoma względem: Brak środowiska uruchomieniowego języka Objective-C, na podstawie zawierający wystąpienia klas natywnych (`NSString`, `NSApplication`itp) i środowiska uruchomieniowego języka C# zawierającą wystąpienia klas zarządzanych (`System.String`, `HttpClient`itp). Między tymi dwoma względem Xamarin.Mac tworzy mostek dwukierunkowej, aplikację można wywoływać metod (selektorów) w języku Objective-C (takie jak `NSApplication.Init`) i języka Objective C można wywoływać aplikacji C# metod Wstecz (np. metody obiektu delegowanego aplikacji). Ogólnie rzecz biorąc, wywołania w języku Objective C są niewidocznie obsługiwane za pośrednictwem **P/Invoke** i niektóre kod środowiska uruchomieniowego zawiera Xamarin.

<a name="exposing-classes" />

## <a name="exposing-c-classes--methods-to-objective-c"></a>Udostępnianie klas C# / metody Objective-C

Jednak dla języka Objective-C do wywołania zwrotnego do aplikacji w języku C# obiektów, muszą być uwidaczniane w języku Objective C można zrozumieć sposób. Odbywa się za pośrednictwem `Register` i `Export` atrybutów. Wykonaj poniższy przykład:

```csharp
[Register ("MyClass")]
public class MyClass : NSObject
{
   [Export ("init")]
   public MyClass ()
   {
   }

   [Export ("run")]
   public void Run ()
   {
   }
}
```

W tym przykładzie środowiska uruchomieniowego języka Objective-C będzie teraz wiedzieć o klasy o nazwie `MyClass` z selektorów o nazwie `init` i `run`.

W większości przypadków jest szczegółów implementacji dewelopera można zignorować, ponieważ większość wywołania zwrotne aplikacja odbiera będzie albo za pomocą metod zastąpiona na `base` klasy (takich jak `AppDelegate`, `Delegates`, `DataSources`) lub na  **Akcje** przekazany do interfejsów API. We wszystkich tych przypadkach `Export` atrybuty nie są niezbędne w kodzie języka C#.

## <a name="constructor-runthrough"></a>Runthrough — Konstruktor

W wielu przypadkach dewelopera należy do udostępnienia aplikacji C# klasy konstruowania interfejsu API do środowiska wykonawczego języka Objective-C, aby go można wdrożyć z takich miejsc, takich jak wywołanego scenorysu lub XIB plików. Oto pięć konstruktorów najczęściej używane w aplikacjach Xamarin.Mac:

```csharp
// Called when created from unmanaged code
public CustomView (IntPtr handle) : base (handle)
{
   Initialize ();
}

// Called when created directly from a XIB file
[Export ("initWithCoder:")]
public CustomView (NSCoder coder) : base (coder)
{
   Initialize ();
}

// Called from C# to instance NSView with a Frame (initWithFrame)
public CustomView (CGRect frame) : base (frame)
{
}

// Called from C# to instance NSView without setting the frame (init)
public CustomView () : base ()
{
}

// This is a special case constructor that you call on a derived class when the derived called has an [Export] constructor.
// For example, if you call init on NSString then you don’t want to call init on NSObject.
public CustomView () : base (NSObjectFlag.Empty)
{
}
```

Ogólnie rzecz biorąc, należy pozostawić dewelopera `IntPtr` i `NSCoder` konstruktorów, które są generowane podczas tworzenia niektórych typów, takich jak niestandardowe `NSViews` samodzielnie. Jeśli po usunięciu go Xamarin.Mac musi wywołać jeden z tych konstruktorów w odpowiedzi na żądanie środowiska uruchomieniowego języka Objective-C, aplikacja ulegnie awarii wewnątrz kodu natywnego i może być trudne do ustalenia dokładnie problem.

## <a name="memory-management-and-cycles"></a>Zarządzanie pamięcią i cykle

Zarządzanie pamięcią w Xamarin.Mac jest bardzo podobne do Xamarin.iOS na wiele sposobów. Jest również temat złożonych, jeden poza zakres tego dokumentu. Przeczytaj [pamięci i najlepsze rozwiązania w zakresie wydajności](~/cross-platform/deploy-test/memory-perf-best-practices.md).

## <a name="ahead-of-time-compilation"></a>Korzystania z czasu kompilacji

Zazwyczaj aplikacji .NET nie skompiluj do kodu maszynowego, jeśli zostały one utworzone, zamiast tego skompiluj do pośredniego warstwy o nazwie kodu IL, który pobiera _Just In Time_ (JIT) skompilowany kod komputera po uruchomieniu aplikacji.

Czas trwania mono czasu wykonywania kompilacji JIT ten kod maszynowy może zmniejszyć uruchomienie aplikacji Xamarin.Mac przez maksymalnie 20%, ponieważ zajmuje trochę czasu konieczne będzie generowany kod maszyny.

Ze względu na ograniczenia narzucone przez firmę Apple iOS kompilacja JIT kodu IL jest niedostępny dla platformy Xamarin.iOS. W związku z tym wszystkie aplikacji platformy Xamarin.iOS są pełne _Ahead Of Time_ (drzewa obiektów aplikacji) skompilowana do kodu maszyny podczas cyklu kompilacji.

Nowością w Xamarin.Mac jest możliwość drzewa obiektów aplikacji kodu IL podczas cyklu kompilacji aplikacji, tak samo, jak można platformy Xamarin.iOS. Używa Xamarin.Mac _hybrydowego_ podejście drzewa obiektów aplikacji, który kompiluje się większość potrzebnych kod maszynowy, ale umożliwia kompilowanie trampolines potrzebne i elastyczność w dalszym ciągu obsługuje Reflection.Emit środowiska uruchomieniowego (i użycia innych przypadków, które obecnie działa na Xamarin.Mac).

Istnieją dwa główne obszary, którym drzewa obiektów aplikacji może pomóc w aplikacji Xamarin.Mac:

- **Lepsze dzienniki awarii "native"** — Jeśli aplikacja Xamarin.Mac ulega awarii w kodzie natywnym, która jest często dochodzi nieprawidłowe wywołania do interfejsów API Cocoa (takie jak wysyłanie `null` do metody, która nie obsługuje on) natywny awarii dzienniki z JIT ramki są trudne do analizy. Ponieważ ramki JIT nie ma informacji o debugowaniu, będzie wiele wierszy z przesunięciem szesnastkowych i nie clue została co dzieje. Drzewa obiektów aplikacji generuje ramki "rzeczywistym" o nazwie i dane śledzenia są znacznie łatwiejsze do odczytu. Oznacza to również, Xamarin.Mac aplikacja będzie wchodzić w lepiej z natywnych narzędzi takich jak **lldb** i **instrumentów**.
- **Uruchamianie lepszej wydajności w czasie** — w przypadku dużych aplikacji Xamarin.Mac, za pomocą wielu uruchamiania po raz drugi, JIT kompilowanie cały kod może zająć dużo czasu. Drzewa obiektów aplikacji działa ta na początku.

### <a name="enabling-aot-compilation"></a>Włączanie kompilacji drzewa obiektów aplikacji

Drzewa obiektów aplikacji jest włączone w Xamarin.Mac przez dwukrotne kliknięcie **Nazwa projektu** w **Eksploratora rozwiązań**, nawigacyjnego do **kompilacji Mac** i dodawanie `--aot:[options]` do  **Mmp dodatkowe argumenty:** pola (gdzie `[options]` jest jeden lub więcej opcji, aby kontrolować typ drzewa obiektów aplikacji, zobacz poniżej). Na przykład:

![Dodawanie drzewa obiektów aplikacji do mmp dodatkowe argumenty](how-it-works-images/aot01.png "drzewa obiektów dodanie aplikacji do mmp dodatkowych argumentów")

> [!IMPORTANT]
> OSTRZEŻENIE! Włączenie drzewa obiektów aplikacji kompilacji znacznie zwiększa czas kompilacji, czasami do kilku minut, ale go może skrócić czas uruchamiania aplikacji o około 20%. W związku z tym kompilacji drzewa obiektów aplikacji powinna być włączona tylko na **wersji** kompilacje Xamarin.Mac aplikacji.

### <a name="aot-compilation-options"></a>Opcje kompilacji drzewa obiektów aplikacji

Istnieje kilka różnych opcji, które można dostosować podczas włączania kompilacji drzewa obiektów aplikacji w aplikacji Xamarin.Mac:

- `none` -Brak kompilacji drzewa obiektów aplikacji. To jest ustawienie domyślne.
- `all` -Drzewa obiektów aplikacji kompiluje wszystkie zestawy w MonoBundle.
- `core` -Kompiluje drzewa obiektów aplikacji `Xamarin.Mac`, `System` i `mscorlib` zestawów.
- `sdk` -Kompiluje drzewa obiektów aplikacji `Xamarin.Mac` i zestawy bibliotek klasy podstawowej (BCL).
- `|hybrid` — Dodawanie do jednej z powyższych opcji umożliwia hybrydowego drzewa obiektów aplikacji, który pozwala na usuwanie IL, ale będzie powodować już kompilować razy.
- `+` -Zawiera jeden dla do kompilacji drzewa obiektów aplikacji.
- `-` — Usuwa pojedynczy plik z kompilacji drzewa obiektów aplikacji.

Na przykład `--aot:all,-MyAssembly.dll` umożliwiłyby kompilacji drzewa obiektów aplikacji na wszystkich zestawów w MonoBundle _z wyjątkiem_ `MyAssembly.dll` i `--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll` umożliwiłyby hybrydowej, zawiera kod drzewa obiektów aplikacji `MyOtherAssembly.dll` , z wyłączeniem `mscorlib.dll`.

## <a name="partial-static-registrar"></a>Częściowe rejestratora statyczne

Podczas opracowywania aplikacji Xamarin.Mac, minimalizując czas między wykonanie zmiany i testowanie go może stać się ważne spotkania programowanie terminy. Strategie takie jak modularyzacji z codebases i testów jednostkowych może pomóc zmniejszyć czasy kompilacji, zgodnie z ich zmniejszyć liczbę razy, że aplikacja będzie wymagać kosztowne ponownej pełnej kompilacji.

Ponadto i nowych do Xamarin.Mac, _częściowe rejestratora statycznych_ (jak pioneered przez Xamarin.iOS) może znacznie zmniejszyć czasy uruchamiania aplikacji Xamarin.Mac w **debugowania** konfiguracji. Zrozumienie, jak przy użyciu częściowego rejestratora statyczne mogą ściśnięte limit prawie poprawy 5 x w uruchamiania debugowania potrwa nieco Historia jest rejestratora, co to jest różnica między statyczna i dynamicznie, co ta wersja "static częściowej" nie.

### <a name="about-the-registrar"></a>Temat Rejestratora

Pod maską żadnych Xamarin.Mac aplikacji znajduje się framework Cocoa z firmy Apple i środowiska uruchomieniowego języka Objective-C. Tworzenie mostka między tym "natywny world" i "world zarządzanego" języka C# jest podstawowym obowiązkiem Xamarin.Mac. Część tego zadania jest obsługiwany przez rejestrator, w którym jest wykonywana wewnątrz `NSApplication.Init ()` metody. Jest to jeden powód, która wymaga użycia interfejsów API Cocoa w Xamarin.Mac `NSApplication.Init` zostać wywołane najpierw.

Zadanie rejestratora jest poinformowanie środowiska uruchomieniowego języka Objective-C o istnieniu aplikacji C# klas pochodzących od klasy, takich jak `NSApplicationDelegate`, `NSView`, `NSWindow`, i `NSObject`. Wymaga to skanowanie wszystkich typów w aplikacji, aby określić, co wymaga rejestracji i jakie elementy dla każdego typu raportu.

Ten tryb skanowania może odbywać się albo **dynamicznie**, podczas uruchamiania aplikacji za pomocą odbicia, lub **statycznie**, krokiem czasu kompilacji. Podczas pobierania typu rejestracji dewelopera należy pamiętać o następujących czynności:

- Statyczne rejestracji znacząco zmniejsza czas uruchamiania, ale może to spowolnić kompilacje razy znacznie (zazwyczaj więcej niż double czas kompilacji debugowania). Będzie to wartość domyślna dla **wersji** konfiguracji kompilacji.
- Dynamiczne rejestracji opóźnia tej pracy do czasu uruchomienia aplikacji, z pominięciem generowania kodu, ale dodatkowej pracy mogą tworzyć zauważalne Wstrzymaj (co najmniej dwie sekundy) w przypadku uruchamiania aplikacji. To zauważyć szczególnie w kompilacjach do debugowania konfiguracji, które domyślnie dynamicznej rejestracji i którego odbicia jest mniejsza.

Częściowe rejestracji statycznych, wprowadzony po raz pierwszy w pkt 8.13 Xamarin.iOS, zapewnia dewelopera najlepsze cechy obu opcji. Przez wstępnie obliczanie informacje rejestracyjne każdego elementu w `Xamarin.Mac.dll` i wysyłanie tych informacji z Xamarin.Mac w bibliotece statycznej (który tylko musi być połączony w czasie kompilacji), firma Microsoft usunęła większość czasu odbicia dynamiczny Rejestrator podczas nie wpływających na czas kompilacji.

### <a name="enabling-the-partial-static-registrar"></a>Włączanie częściowe rejestratora statyczne

Częściowe rejestratora statycznego jest włączona w Xamarin.Mac przez dwukrotne kliknięcie **Nazwa projektu** w **Eksploratora rozwiązań**, nawigacyjnego do **kompilacji Mac** i dodawanie `--registrar:static` do **mmp dodatkowe argumenty:** pola. Na przykład:

![Dodawanie częściowe rejestratora statyczny do mmp dodatkowe argumenty](how-it-works-images/psr01.png "Dodawanie częściowe rejestratora statyczny do mmp dodatkowe argumenty")

## <a name="additional-resources"></a>Dodatkowe zasoby

Poniżej przedstawiono niektóre szczegółowe wyjaśnienia sposobu działania wewnętrznie:

- [Objective-C Selectors](~/ios/internals/objective-c-selectors.md)
- [Rejestratora](~/ios/internals/registrar.md)
- [Interfejsu API Unified Xamarin dla systemów iOS i OS X](~/cross-platform/macios/unified/index.md)
- [Podstawowe informacje na temat Theading](~/ios/app-fundamentals/threading.md)
- [Obiekty delegowane, protokołów i zdarzenia](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [— informacje `newrefcount`](~/ios/internals/newrefcount.md)

