---
title: Wyjątek przekazywanie w Xamarin.iOS
description: Ten dokument zawiera opis sposobu pracy z natywnych i zarządzanych wyjątków w aplikacji platformy Xamarin.iOS. Zawarto informacje problemów, które mogą wystąpić i rozwiązanie tych problemów.
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/05/2017
ms.openlocfilehash: dcf1074aacb6d139d107dac01fa86f459831d5f9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786746"
---
# <a name="exception-marshaling-in-xamarinios"></a>Wyjątek przekazywanie w Xamarin.iOS

_Xamarin.iOS zawiera nowych zdarzeń, aby pomóc odpowiedzieć na wyjątki, szczególnie w kodzie natywnym._

Zarówno kod zarządzany, jak i języka Objective C jest obsługiwane wyjątki czasu wykonywania (try/catch/finally — klauzule).

Jednak ich implementacji są różne, co oznacza, że bibliotek środowiska uruchomieniowego (Mono środowiska uruchomieniowego i bibliotek środowiska uruchomieniowego języka Objective-C) mają problemy w celu obsługi wyjątków, a następnie uruchom kod napisany w innych językach.

W tym dokumencie opisano problemy, które mogą wystąpić i możliwych rozwiązań.

Przykładowy projekt, zawiera także [organizowanie wyjątek](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling), które mogą służyć do testowania scenariuszy i ich rozwiązania.

## <a name="problem"></a>Problem

Problem występuje, gdy wyjątek zostanie zgłoszony podczas odwijanie ramki stosu napotkano, który nie jest zgodny z typem wyjątku, który został zgłoszony.

Typowym przykładem to dla platformy Xamarin.iOS lub Xamarin.Mac jest podczas natywnego interfejsu API zgłasza wyjątek języka Objective-C, a następnie ten wyjątek języka Objective-C jakiś sposób musi obsługiwane, gdy proces odwijaniem stosu osiągnie ramki zarządzane.

Domyślne działanie ma nic nie rób. Powyższej przykładowej oznacza to, dzięki czemu ramki unwind zarządzanego środowiska wykonawczego języka Objective-C. Stanowi to problem, ponieważ środowisko uruchomieniowe języka Objective-C wiadomo, jak unwind ramki zarządzane; na przykład go nie będzie wykonywać żadnych `catch` lub `finally` klauzule w tej ramce.

### <a name="broken-code"></a>Uszkodzony kod

Rozważmy poniższy przykład kodu:

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

Zgłosi NSInvalidArgumentException Objective-C w kodzie natywnym:

    NSInvalidArgumentException *** setObjectForKey: key cannot be nil

I ślad stosu będzie podobny do następującego:

    0   CoreFoundation          __exceptionPreprocess + 194
    1   libobjc.A.dylib         objc_exception_throw + 52
    2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
    3   libobjc.A.dylib         objc_msgSend + 102
    4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
    5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
    6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()

Ramki 0-3 są ramek natywnych i unwinder stosu w języku Objective C runtime _można_ unwind te ramki. W szczególności wykonania dowolnego języka Objective-C `@catch` lub `@finally` klauzul.

Jednak unwinder stosu w języku Objective C jest _nie_ stanie poprawnie rozwinięcia ramki zarządzane (ramki 4 – 6), w tym ramki będzie można oddzielić, ale logiki zarządzanym wyjątku nie zostanie wykonany.

Co oznacza, że zwykle nie jest możliwe do wychwytywania tych wyjątków w następujący sposób:

```csharp
try {
    var dict = new NSMutableDictionary ();
    dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero);
} catch (Exception ex) {
    Console.WriteLine (ex);
} finally {
    Console.WriteLine ("finally");
}
```

To dlatego unwinder stosu w języku Objective C nie może określić dotyczących zarządzanej `catch` klauzul i nie będzie `finally` klauzuli można wykonać.

Jeśli w powyższym przykładzie kodu _jest_ skuteczne, to, że Objective-C ma metodę bycia powiadamianym o nieobsługiwanych wyjątków języka Objective-C, [`NSSetUncaughtExceptionHandler`][2], która Xamarin.iOS i użyj Xamarin.Mac, a w tym momencie próbuje przekonwertować wszelkie wyjątki Objective-C zarządzanych wyjątków.

## <a name="scenarios"></a>Scenariusze

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>Scenariusz 1 - przechwytywanie wyjątków w języku Objective C z obsługi catch zarządzanych

W poniższym scenariuszu, istnieje możliwość przechwytywanie wyjątków w języku Objective C za pomocą zarządzanych `catch` programów obsługi:

1. Języka Objective C jest zwracany wyjątek.
2. Środowisko uruchomieniowe języka Objective-C przeprowadzi stosu (ale nie unwind), wyszukiwanie natywny `@catch` obsługi, która może obsłużyć wyjątek.
3. Środowisko uruchomieniowe języka Objective-C nie odnaleziono żadnych `@catch` programów obsługi, wywołania `NSGetUncaughtExceptionHandler`i wywołuje program obsługi instalowane przez Xamarin.iOS/Xamarin.Mac.
4. Obsługi Xamarin.iOS/Xamarin.Mac's konwertować wyjątek języka Objective-C zarządzanym wyjątku i zgłoś go. Od języka Objective C runtime nie unwind stosu (tylko udał go), bieżącej ramki jest taki sam, gdzie został zgłoszony wyjątek języka Objective-C.

Inny problem występuje w tym miejscu, ponieważ Mono środowiska uruchomieniowego wiadomo, jak poprawnie unwind ramki Objective-C.

Wywołanego wywołania zwrotnego Xamarin.iOS nieprzechwyconego Objective-C wyjątków stos jest następujący:

     0 libxamarin-debug.dylib   exception_handler(exc=name: "NSInvalidArgumentException" - reason: "*** setObjectForKey: key cannot be nil")
     1 CoreFoundation           __handleUncaughtException + 809
     2 libobjc.A.dylib          _objc_terminate() + 100
     3 libc++abi.dylib          std::__terminate(void (*)()) + 14
     4 libc++abi.dylib          __cxa_throw + 122
     5 libobjc.A.dylib          objc_exception_throw + 337
     6 CoreFoundation           -[__NSDictionaryM setObject:forKey:] + 1015
     7 libxamarin-debug.dylib   xamarin_dyn_objc_msgSend + 102
     8 TestApp                  ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
     9 TestApp                  Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr) [0x00000]
    10 TestApp                  ExceptionMarshaling.Exceptions.ThrowObjectiveCException () [0x00013]

W tym miejscu tylko ramki zarządzane są ramki 8-10, ale zarządzanych wyjątku w przedziale od 0. Oznacza to, że Mono środowiska uruchomieniowego musi unwind ramek natywnych 0-7, co powoduje, że problem odpowiednikiem problem opisany: mimo że Mono środowiska uruchomieniowego będzie unwind ramek natywnych, nie będzie wykonania dowolnego języka Objective-C `@catch` lub `@finally` klauzule .

Przykład kodu:

```objc
-(id) setObject: (id) object forKey: (id) key
{
    @try {
        if (key == nil)
            [NSException raise: @"NSInvalidArgumentException"];
    } @finally {
        NSLog (@"This will not be executed");
    }
}
```

I `@finally` klauzuli nie zostanie wykonany, ponieważ Mono środowisko uruchomieniowe, które cofa tej ramki nie ma informacji na ten temat.

Odmiana to ma throw zarządzanym wyjątku w kod zarządzany, a następnie rozwinięcia za pośrednictwem ramek natywnych, aby uzyskać pierwszy zarządzanych `catch` klauzuli:

```csharp
class AppDelegate : UIApplicationDelegate {
    public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
    {
        throw new Exception ("An exception");
    }
    static void Main (string [] args)
    {
        try {
            UIApplication.Main (args, null, typeof (AppDelegate));
        } catch (Exception ex) {
            Console.WriteLine ("Managed exception caught.");
        }
    }
}
```

Zarządzanej `UIApplication:Main` metoda wywoła natywnego `UIApplicationMain` — metoda i iOS będzie zajmować dużo wykonywania kodu natywnego przed ostatecznie wywołaniem zarządzanej `AppDelegate:FinishedLaunching` metody nadal wiele ramek natywnych na stosie, gdy jest zarządzanym wyjątku wyjątek:

     0: TestApp                 ExceptionMarshaling.IOS.AppDelegate:FinishedLaunching (UIKit.UIApplication,Foundation.NSDictionary)
     1: TestApp                 (wrapper runtime-invoke) <Module>:runtime_invoke_bool__this___object_object (object,intptr,intptr,intptr) 
     2: libmonosgen-2.0.dylib   mono_jit_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
     3: libmonosgen-2.0.dylib   do_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
     4: libmonosgen-2.0.dylib   mono_runtime_invoke [inlined] mono_runtime_invoke_checked(method=<unavailable>, obj=<unavailable>, params=<unavailable>, error=0xbff45758)
     5: libmonosgen-2.0.dylib   mono_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>)
     6: libxamarin-debug.dylib  xamarin_invoke_trampoline(type=<unavailable>, self=<unavailable>, sel="application:didFinishLaunchingWithOptions:", iterator=<unavailable>), context=<unavailable>)
     7: libxamarin-debug.dylib  xamarin_arch_trampoline(state=0xbff45ad4)
     8: libxamarin-debug.dylib  xamarin_i386_common_trampoline
     9: UIKit                   -[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:]
    10: UIKit                   -[UIApplication _callInitializationDelegatesForMainScene:transitionContext:]
    11: UIKit                   -[UIApplication _runWithMainScene:transitionContext:completion:]
    12: UIKit                   __84-[UIApplication _handleApplicationActivationWithScene:transitionContext:completion:]_block_invoke.3124
    13: UIKit                   -[UIApplication workspaceDidEndTransaction:]
    14: FrontBoardServices      __37-[FBSWorkspace clientEndTransaction:]_block_invoke_2
    15: FrontBoardServices      __40-[FBSWorkspace _performDelegateCallOut:]_block_invoke
    16: FrontBoardServices      __FBSSERIALQUEUE_IS_CALLING_OUT_TO_A_BLOCK__
    17: FrontBoardServices      -[FBSSerialQueue _performNext]
    18: FrontBoardServices      -[FBSSerialQueue _performNextFromRunLoopSource]
    19: FrontBoardServices      FBSSerialQueueRunLoopSourceHandler
    20: CoreFoundation          __CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__
    21: CoreFoundation          __CFRunLoopDoSources0
    22: CoreFoundation          __CFRunLoopRun
    23: CoreFoundation          CFRunLoopRunSpecific
    24: CoreFoundation          CFRunLoopRunInMode
    25: UIKit                   -[UIApplication _run]
    26: UIKit                   UIApplicationMain
    27: TestApp                 (wrapper managed-to-native) UIKit.UIApplication:UIApplicationMain (int,string[],intptr,intptr)
    28: TestApp                 UIKit.UIApplication:Main (string[],intptr,intptr)
    29: TestApp                 UIKit.UIApplication:Main (string[],string,string)
    30: TestApp                 ExceptionMarshaling.IOS.Application:Main (string[])

Ramki 0 1 i 27 30 są zarządzane, są wszystkie te między macierzystego. Jeśli Mono cofa się za pośrednictwem tych ramki, Objective-C `@catch` lub `@finally` klauzule zostaną wykonane.

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>Scenariusz 2: nie można przechwytywać wyjątki języka Objective C

W poniższym scenariuszu jest _nie_ umożliwia przechwytywanie wyjątków w języku Objective C za pomocą managed `catch` obsługi ponieważ Objective-C wyjątek został obsłużony w inny sposób:

1. Języka Objective C jest zwracany wyjątek.
2. Środowisko uruchomieniowe języka Objective-C przeprowadzi stosu (ale nie unwind), wyszukiwanie natywny `@catch` obsługi, która może obsłużyć wyjątek.
3. Środowisko uruchomieniowe języka Objective-C znajduje `@catch` obsługi, cofa stosu i rozpoczyna wykonywanie `@catch` obsługi.

W tym scenariuszu często znajduje się w aplikacji platformy Xamarin.iOS, ponieważ w głównym wątku jest zazwyczaj kod następująco:

``` objective-c
void UIApplicationMain ()
{
    @try {
        while (true) {
            ExecuteRunLoop ();
        }
    } @catch (NSException *ex) {
        NSLog (@"An unhandled exception occured: %@", exc);
        abort ();
    }
}

```

To oznacza, że w głównym wątku nigdy nie jest naprawdę nieobsługiwany wyjątek języka Objective-C, a w związku z tym naszej wywołania zwrotnego, które konwertuje Objective-C wyjątków zarządzanych wyjątków nigdy nie jest wywoływana.

Dotyczy to również dość często podczas debugowania aplikacji Xamarin.Mac we wcześniejszej wersji macOS niż Xamarin.Mac obsługuje ponieważ sprawdzania większość obiektów interfejsu użytkownika w debugerze spróbuje pobrać właściwości, które odpowiadają na selektorów, które nie istnieją na wykonywanie (platformy ponieważ Xamarin.Mac obsługuje wyższą wersję macOS). Wywoływanie takie selektorów zgłosi `NSInvalidArgumentException` ("Nierozpoznany selektora przesyłany do..."), co ostatecznie powoduje, że proces awarii.

Podsumowując, mając środowiska uruchomieniowego języka Objective-C lub ramki unwind Mono środowiska uruchomieniowego, które nie programowane w dojścia może prowadzić do niezdefiniowanego zachowania, takie jak awarie, przecieki pamięci i innych typów zachowania nieprzewidywalna (mis).

## <a name="solution"></a>Rozwiązanie

Xamarin.iOS 10 i Xamarin.Mac 2.10 Dodaliśmy obsługę przechwytywanie wyjątków zarządzanych i Objective-C na krawędzi zarządzane natywne oraz konwertowanie tego wyjątku do innego typu.

W pseudo-kodzie wygląda mniej więcej tak:

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

P/Invoke do objc_msgSend zostaje zatrzymana, a ta metoda jest wywoływana zamiast tego:

``` objective-c
void
xamarin_dyn_objc_msgSend (id obj, SEL sel)
{
    @try {
        objc_msgSend (obj, sel);
    } @catch (NSException *ex) {
        convert_to_and_throw_managed_exception (ex);
    }
}
```

I odbywa się podobnie w przypadku odwrotnej (przekazywanie zarządzanych wyjątków do wyjątków języka Objective-C).

Przechwytywanie wyjątków w granicach zarządzanych native nie jest koszt wolne, więc go jest nie zawsze domyślnie:

- Xamarin.iOS/tvOS: przechwytywaniu wyjątków języka Objective C jest włączone w symulatorze.
- Xamarin.watchOS: przechwytywaniu są wymuszane we wszystkich przypadkach, ponieważ umożliwienie unwind środowiska uruchomieniowego języka Objective-C zarządzane ramki będzie mylić moduł garbage collector i albo spowodować zawieszenie lub awarii.
- Xamarin.Mac: przechwytywaniu wyjątków języka Objective C jest włączone dla kompilacji debugowania.

[Flagi czas kompilacji](#build_time_flags) sekcji opisano sposób włączania zatrzymania, gdy nie jest włączona domyślnie.

## <a name="events"></a>Zdarzenia

Istnieją dwa nowe zdarzenia, które są generowane, gdy wyjątek zostanie przechwycona: `Runtime.MarshalManagedException` i `Runtime.MarshalObjectiveCException`.

Zarówno zdarzenia są przekazywane `EventArgs` obiekt, który zawiera pierwotny wyjątek zgłoszony ( `Exception` właściwości) i `ExceptionMode` właściwość, aby określić, jak powinny być przekazywane wyjątek.

`ExceptionMode` Właściwość można zmienić w zdarzeniu programu obsługi, aby zmienić zachowanie zgodnie z żadnych niestandardowych przetwarzania wykonywane w obsłudze. Przykładem może być przerwanie procesu w przypadku niektórych wyjątek.

Zmiana `ExceptionMode` właściwość ma zastosowanie do jednego zdarzenia, nie ma wpływu na wszystkie wyjątki przechwycone w przyszłości.

Dostępne są następujące metody:

- `Default`: Wartość domyślna zależy od platformy. Jest `ThrowObjectiveCException` Jeśli wykaz Globalny jest w trybie współpracy (watchOS) i `UnwindNativeCode` w przeciwnym razie (z systemem iOS / watchOS / macOS). Wartość domyślna mogą ulec zmianie w przyszłości.
- `UnwindNativeCode`: To jest poprzednie działanie (niezdefiniowany). To nie jest dostępna podczas w trybie współpracy przy użyciu GC (która jest jedyną opcją watchOS; w związku z tym nie jest prawidłową opcją na watchOS), ale jest opcją domyślną dla innych platform.
- `ThrowObjectiveCException`: Konwertowanie zarządzanym wyjątku wyjątek języka Objective C i zgłoszenie wyjątku języka Objective-C. Jest to domyślna na watchOS.
- `Abort`: Przerwanie procesu.
- `Disable`: Wyłącza przechwytywaniu wyjątków, aby go nie ma sensu można ustawić tej wartości w obsłudze zdarzeń, ale po wywołaniu zdarzenia jest za późno je wyłączyć. W każdym przypadku jeśli ustawiona, jej będą działały jak `UnwindNativeCode`.

Dla organizowanie wyjątków języka Objective-C do kodu zarządzanego, dostępne są następujące tryby:

- `Default`: Wartość domyślna zależy od platformy. Jest `ThrowManagedException` Jeśli wykaz Globalny jest w trybie współpracy (watchOS) i `UnwindManagedCode` w przeciwnym razie (z systemem iOS / systemu tvOS / macOS). Wartość domyślna mogą ulec zmianie w przyszłości.
- `UnwindManagedCode`: To jest poprzednie działanie (niezdefiniowany). To nie jest dostępna podczas w trybie współpracy przy użyciu GC (który jest ważny tylko tryb GC watchOS; w związku z tym nie jest prawidłową opcją na watchOS), ale jest ustawieniem domyślnym dla innych platform.
- `ThrowManagedException`: Konwertowanie wyjątków języka Objective-C zarządzanym wyjątku i zgłoszenie wyjątku zarządzanych. Jest to domyślna na watchOS.
- `Abort`: Przerwanie procesu.
- `Disable`: Wyłącza przechwytywaniu wyjątków, więc go nie ma sensu można ustawić wartość to w przypadku obsługi, ale raz zdarzenie jest zgłaszane, jest za późno je wyłączyć. W każdym przypadku jeśli ustawiona, jej spowoduje przerwanie procesu.

Tak aby zobaczyć za każdym razem, gdy wyjątek jest przekazywane, można to zrobić:

``` csharp
Runtime.MarshalManagedException += (object sender, MarshalManagedExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling managed exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
    
};
Runtime.MarshalObjectiveCException += (object sender, MarshalObjectiveCExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling Objective-C exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
};
```

<a name="build_time_flags" />

## <a name="build-time-flags"></a>Flagi czas kompilacji

Można przekazać następujących opcji, aby **mtouch** (na potrzeby aplikacji platformy Xamarin.iOS) i **mmp** (dla aplikacji Xamarin.Mac), który ustalić, czy przechwycenie wyjątku jest włączona i określić domyślną akcję, która Jeśli wystąpią:

- `--marshal-managed-exceptions=`
  - `default`
  - `unwindnativecode`
  - `throwobjectivecexception`
  - `abort`
  - `disable`

- `--marshal-objectivec-exceptions=`
  - `default`
  - `unwindmanagedcode`
  - `throwmanagedexception`
  - `abort`
  - `disable`

Z wyjątkiem `disable`, te wartości są takie same jak `ExceptionMode` wartości, które są przekazywane do `MarshalManagedException` i `MarshalObjectiveCException` zdarzenia.

`disable` Opcji spowoduje _przede wszystkim_ wyłączyć zatrzymania, ale możemy nadal będzie przechwytywać z wyjątków podczas nie dodaje żadnych narzut na wykonanie. Zdarzenia kierowania nadal są zgłaszane dla tych wyjątków z domyślnym trybem jest domyślnym trybem wykonywania platformy.

## <a name="limitations"></a>Ograniczenia

Firma Microsoft tylko przechwycić P/Invokes do `objc_msgSend` rodziny funkcji podczas próby przechwytywanie wyjątków w języku Objective-C. Oznacza to, że P/Invoke do innej funkcji C, który następnie wszelkie wyjątki Objective-C, będą nadal działać w starym i niezdefiniowane zachowanie (to może zostać poprawione w przyszłości).

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc


## <a name="related-links"></a>Linki pokrewne

- [Kierowanie parametrów wyjątek (przykład)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
