---
title: Debugowanie natywnych awarii
description: "Ten przewodnik opisuje sposób debugowania wyjątki, które pochodzą z języka Objective-C runtime."
ms.topic: article
ms.prod: xamarin
ms.assetid: B0C0CE31-2737-4969-8EA5-D39D3333E9C2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: d8633a3f575b51d4eeac326cc5ea418fcbf5bd20
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="debugging-a-native-crash"></a>Debugowanie natywnych awarii

## <a name="overview"></a>Omówienie

Czasami błędy programowania można przyczynę awarii w natywnym środowisku uruchomieniowym języka Objective-C. W przeciwieństwie do języka C# wyjątki te nie wskazują na określonej linii w kodzie, który można sprawdzić, aby rozwiązać. Czasami może być prosta do Znajdowanie i rozwiązywanie problemów, a innym razem one może być bardzo trudne do śledzenia. 

Umożliwia przeprowadzenie kilka przykładów rzeczywistym natywnego awarii i zapoznaj się z.

## <a name="example-1-assertion-failure"></a>Przykład 1: Błąd potwierdzenia

Oto kilka pierwszych linii awarii w aplikacji prosty test (te informacje będą w **danych wyjściowych aplikacji** konsoli):

```csharp
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.381 NSOutlineViewHottness[79111:1304993] (
    0   CoreFoundation                      0x91888343 __raiseError + 195
    1   libobjc.A.dylib                     0x9a5e6a2a objc_exception_throw + 276
    2   CoreFoundation                      0x918881ca +[NSException raise:format:arguments:] + 138
    3   Foundation                          0x950742b1 -[NSAssertionHandler handleFailureInMethod:object:file:lineNumber:description:] + 118
    4   AppKit                              0x975db476 -[NSTableView _uncachedRectHeightOfRow:] + 373
    5   AppKit                              0x975db2f8 -[_NSTableRowHeightStorage _uncachedRectHeightOfRow:] + 143
    6   AppKit                              0x975db206 -[_NSTableRowHeightStorage _cacheRowHeights] + 167
    7   AppKit                              0x975db130 -[_NSTableRowHeightStorage _createRowHeightsArray] + 226
    8   AppKit                              0x975b5851 -[_NSTableRowHeightStorage _ensureRowHeights] + 73
    9   AppKit                              0x975b5790 -[_NSTableRowHeightStorage computeTableHeightForNumberOfRows:] + 89
    10  AppKit                              0x975b4c38 -[NSTableView _totalHeightOfTableView] + 220
```

Przedrostek numery wierszy jest ślad stosu macierzystego. Od tego widać, awarii wystąpił gdzieś w `NSTableView` obsługi wysokości wierszy. Następnie `NSAssertionHandler` uruchamiany `NSException (objc_exception_throw)` i widzimy Błąd potwierdzenia:

```csharp
rowHeight error: The value must be > 0 for row 0, but the delegate
<NSOutlineView_ViewDelegate: 0xaa01860> gave -1.000
```

Przeanalizowanie to on ma bardzo wyczyść niektórych `NSOutlineViewDelegate` metoda zwraca wartość ujemną. To problemu:

```csharp
public override nfloat GetRowHeight (NSTableView tableView, nint row)
{
    return -1;
}
```

## <a name="example-2-callback-jumped-into-middle-of-nowhere"></a>Przykład 2: Wywołanie zwrotne przeskoki do środka nigdzie

```csharp
Stacktrace:

 at <unknown> <0xffffffff>
 at (wrapper managed-to-native) MonoMac.AppKit.NSApplication.NSApplicationMain (int,string[]) <IL 0x000a4, 0xffffffff>
 at MonoMac.AppKit.NSApplication.Main (string[]) [0x00041] in /Users/donblas/Programming/xamcore-master/src/AppKit/NSApplication.cs:107
 at NSOutlineViewHottness.MainClass.Main (string[]) [0x00007] in /Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/Main.cs:14
 at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr) <IL 0x00050, 0xffffffff>

Native stacktrace:

Debug info from gdb:

(lldb) command source -s 0 '/tmp/mono-gdb-commands.qrHllW'
Executing commands in '/private/tmp/mono-gdb-commands.qrHllW'.
(lldb) process attach --pid 79229
Process 79229 stopped
Executable module set to "/Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/bin/Debug/NSOutlineViewHottness.app/Contents/MacOS/NSOutlineViewHottness".
Architecture set to: i386-apple-macosx.
(lldb) thread list
Process 79229 stopped
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 thread #2: tid = 0x142790, 0x9af768d2 libsystem_kernel.dylib`kevent64 + 10, queue = 'com.apple.libdispatch-manager'
 thread #3: tid = 0x142792, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #4: tid = 0x142794, 0x9af6fa6a libsystem_kernel.dylib`semaphore_wait_trap + 10
 thread #5: tid = 0x142795, 0x9af75772 libsystem_kernel.dylib`__recvfrom + 10
 thread #6: tid = 0x142799, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #7: tid = 0x14279a, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #8: tid = 0x14279b, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #9: tid = 0x1427f8, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #10: tid = 0x1427fe, 0x9af6fa2e libsystem_kernel.dylib`mach_msg_trap + 10
(lldb) thread backtrace all
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 * frame #0: 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10
   frame #1: 0x986bfb25 libsystem_c.dylib`waitpid$UNIX2003 + 48
   frame #2: 0x028ba36d libmono-2.0.dylib`mono_handle_native_sigsegv(signal=11, ctx=0x03115fe0) + 541 at mini-exceptions.c:2323
   frame #3: 0x0290a8bb libmono-2.0.dylib`mono_arch_handle_altstack_exception(sigctx=<unavailable>, fault_addr=<unavailable>, stack_ovf=0) + 155 at exceptions-x86.c:1159
   frame #4: 0x0280b4fd libmono-2.0.dylib`mono_sigsegv_signal_handler(_dummy=<unavailable>, info=<unavailable>, context=<unavailable>) + 445 at mini.c:6861
   frame #5: 0x91ef403b libsystem_platform.dylib`_sigtramp + 43
   frame #6: 0x9a5dd0bd libobjc.A.dylib`objc_msgSend + 45
   frame #7: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #8: 0x9773ba91 AppKit`-[NSApplication sendAction:to:from:] + 548
   frame #9: 0x9773b82d AppKit`-[NSControl sendAction:to:] + 102
   frame #10: 0x97934d36 AppKit`__26-[NSCell _sendActionFrom:]_block_invoke + 176
   frame #11: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #12: 0x97787975 AppKit`-[NSCell _sendActionFrom:] + 161
   frame #13: 0x979188ea AppKit`-[NSButtonCell _sendActionFrom:] + 55
   frame #14: 0x979366e6 AppKit`__48-[NSCell trackMouse:inRect:ofView:untilMouseUp:]_block_invoke965 + 43
   frame #15: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #16: 0x977a3d00 AppKit`-[NSCell trackMouse:inRect:ofView:untilMouseUp:] + 2815
   frame #17: 0x977a2df4 AppKit`-[NSButtonCell trackMouse:inRect:ofView:untilMouseUp:] + 524
   frame #18: 0x977a233b AppKit`-[NSControl mouseDown:] + 762
   frame #19: 0x97cbc112 AppKit`-[_NSThemeWidget mouseDown:] + 378
   frame #20: 0x97d36d74 AppKit`-[NSWindow _reallySendEvent:] + 12353
   frame #21: 0x977201f9 AppKit`-[NSWindow sendEvent:] + 409
   frame #22: 0x976cdc67 AppKit`-[NSApplication sendEvent:] + 4679
   frame #23: 0x9754807c AppKit`-[NSApplication run] + 1003
```

Jest to problem, który był znacznie trudniejsze do śledzenia. Po wyświetleniu `at <unknown> <0xffffffff>` lub `MonoMac.ObjCRuntime.Runtime.GetNSObject (IntPtr ptr)` u góry śladu stosu zarządzanych sugeruje próbujemy do wykonania niektórych kodu zarządzanego z obiektem, które zostały zebrane pamięci. Pokazuje śledzenia stosu natywnego `trackMouse:inRect:ofView:untilMouseUp` do `NSCell _sendActionFrom` , możemy innym obsługi zdarzenie click próby wywołania zwrotnego do języka C# i śmierci.

Ogólnie rzecz biorąc błędy takie są trudne do śledzenia. Po dodaniu `GC.Collect(2)` do obsługi przycisk, aby ułatwić śledzenie tego problemu (wymuszanie wyrzucania elementów bezużytecznych) w celu wydawania odtworzenia. Po bisecting przykładzie przez pewien czas, usunięcie fragmentów kodu do czasu wydania zniknął, włączona, należy [tej usterki](https://bugzilla.xamarin.com/show_bug.cgi?id=23378):

```csharp
mainWindowController.Window.StandardWindowButton (NSWindowButton.CloseButton).Activated += HandleActivated;
```

`NSButton` Zwrócony przez `StandardWindowButton()` został zbieranych, mimo że zdarzenia został zarejestrowany do niego (która jest usterki). Przy próbie wywołanie tego klikając, gdy przycisk był bezużytecznych możemy awarii.

Mimo że nie był główną przyczynę tego konkretnego problemu, ślady stosu, jak to może być spowodowane podpisów niepoprawnej metody w funkcjach `[Export]`ed Objective-c. Na przykład, jeśli metoda oczekuje parametru `out string` i wpisz go w formie `string`, firma Microsoft może ulec awarii w taki sam sposób.

## <a name="example-3-callbacks-and-managed-objects"></a>Przykład 3: Wywołania zwrotne i zarządzane obiekty

Wiele interfejsów API Cocoa obejmują trwa "wywołanie zwrotne" w bibliotece po wystąpieniu zdarzenia, co daje możliwość reagować, lub gdy niektórych danych jest wymagany do wykonania określonego zadania. Chociaż można traktować głównie **delegata** i **źródła danych** wzorców, istnieje wiele interfejsów API, które działają w ten sposób. Na przykład, jeśli zastąpienie metody `NSView` i wstawić go w drzewie wizualnym, oczekują AppKit zadzwonić do Ciebie po wystąpieniu określonego zdarzenia.

W większości przypadków Xamarin.Mac zostanie poprawnie uniemożliwić docelowy obiekt zarządzany tych wywołań zwrotnych w ramach odzyskiwania pamięci podczas one nadal może być wywołanie zwrotne. Jednak rzadko usterki w powiązaniu może zakłócać to. W takim przypadku można wyświetlić nieprzyjemny awarii podobny do poniższego:

```csharp
Thread 0 Crashed:: Dispatch queue: com.apple.main-thread

0   libsystem_kernel.dylib              0x98c2f69a __pthread_kill + 10
1   libsystem_pthread.dylib             0x90341f19 pthread_kill + 101
2   libsystem_c.dylib                   0x9453feee abort + 156
3   libmonosgen-2.0.dylib               0x020bfba5 mono_handle_native_sigsegv + 757
4   libmonosgen-2.0.dylib               0x0210b812 mono_arch_handle_altstack_exception + 162
5   libmonosgen-2.0.dylib               0x0200c55e mono_sigsegv_signal_handler + 446
6   libsystem_platform.dylib            0x9513003b _sigtramp + 43
7   ???                                 0xffffffff 0 + 4294967295
8   libmonosgen-2.0.dylib               0x0200c3a0 mono_sigill_signal_handler + 48
9   com.apple.AppKit                    0x99f76041 -[NSView setFrame:] + 448
10  com.apple.AppKit                    0x9a1fd4ea -[NSToolbarView adjustToWindow:attachedToEdge:] + 198
11  com.apple.AppKit                    0x9a1fd414 -[NSToolbar _adjustViewToWindow] + 68
12  com.apple.AppKit                    0x9a01eb0d -[NSToolbar _windowWillShowToolbar] + 79

```

Ten przewodnik ułatwia śledzenie usterek, tego rodzaju jeśli one przyciąć, poprawnie Raportuj je więc ich i obejść je w przypadku korzystania z kodu do tego czasu.

### <a name="locating"></a>Lokalizowanie

Prawie w każdym przypadku z usterkami tego rodzaju głównej objaw jest natywny awarii, zwykle z polecenie podobne do następującego `mono_sigsegv_signal_handler`, lub `_sigtrap` w górnej ramki stosu. Cocoa próby wywołania zwrotnego do kodu C#, aktywując połączonego obiektu pamięci i awarii. Jednak awarii nie każdy z tych symboli jest spowodowany problemem powiązanie następująco, musisz wykonać pewne dodatkowe tu przeszukuje potwierdzić, że na tym polega problem. 

Co sprawia, że te usterki trudne do śledzenia jest tylko występowania **po** wyrzucania elementów bezużytecznych został usunięty z danego obiektu. Jeśli uważasz, że zostały trafienie jednego z tych błędów, należy gdzieś Dodaj następujący kod w sekwencji uruchamiania:

```csharp
new System.Threading.Thread (() => 
{
    while (true) {
         System.Threading.Thread.Sleep (1000);
         GC.Collect ();
    }
}).Start (); 
```

Spowoduje to wymuszenie aplikację do uruchamiania modułu zbierającego elementy bezużyteczne w ciągu sekundy. Ponownie uruchom aplikację i spróbuj odtworzyć ten błąd. Jeśli zamiast losowo jest awarii natychmiast lub spójnie, to na ścieżce prawo.

### <a name="reporting"></a>Raportowanie

Następnym krokiem jest zgłosić problem Xamarin, można naprawić powiązania w przyszłych wersjach. Jeśli jesteś firmy lub właściciela licencji enterprise otworzyć bilet w 

[http://xamarin.com/support](http://xamarin.com/support)

W przeciwnym razie Szukaj istniejący problem:

- Sprawdź [fora Xamarin.Mac](https://forums.xamarin.com/categories/mac)
- Wyszukiwanie [repozytorium problem](https://github.com/xamarin/xamarin-macios/issues)
- Przed przełączeniem na problemy z GitHub, Xamarin problemy zostały śledzone na [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Wyszukaj istnieje, do dopasowania problemy.
- Jeśli nie można odnaleźć pasującego problem, plik nowy problem w [repozytorium GitHub problem](https://github.com/xamarin/xamarin-macios/issues/new).

Problemy z usługi GitHub są wszystkie publiczne. Nie jest możliwe Ukryj komentarze lub załączników. 

Dołącz tyle jedną z następujących możliwości:

- Prosty przykład odtworzenia problemu. Jest to **nieocenione** w miarę możliwości. 
- Ślad stosu pełne awarii (Crash).
- Kod C# otaczającego tej awarii.   

### <a name="working-around"></a>Praca wokół

Po śledzenia po rozwiązaniu problemu może być prosty stosowanie poprawek problem z obejść, dopóki nie można ustalić powiązania. Celem jest, aby zapobiec obiektu (**widoku**, **delegata**, **źródła danych**) który niepoprawnie zostanie usunięty z opuszczeniem pamięci, przechowując otwarte odwołanie.

W prostych przypadkach, gdy istnieje tylko jedno wystąpienie obiektu, zmienić kod z tego:

```csharp
void AddObject ()
{
    item.View = new MyView ();
    ...
}
```

Przy użyciu zmienną statyczną, takich jak ta:

```csharp
static NSObject view;
...

void AddObject ()
{
    view = new MyView ();
    item.View = view;
    ...
}
```

W przypadkach, gdy wiele wystąpień może zostać utworzony, statycznego `HashSet` można zastosować:

```csharp
static HashSet<NSObject> collection = new HashSet<NSObject> ();
...

void AddObject ()
{
    item.View = new MyView ();
    collection.Add (item.View );
    ...
}
```

Po powiązaniu został rozwiązany i uaktualnienie do wersji Xamarin.Mac, która obejmuje poprawki, które zostały, można usunąć kod obejścia.

## <a name="exception-bubbling-and-objective-c"></a>Wyjątek propagacji i języka Objective C

Wyjątek języka C# "ucieczki" kodu zarządzanego do wywoływania metody Objective-C nigdy nie należy zezwalać. Jeśli to zrobisz, wyniki są niezdefiniowane, ale generalnie wymagają awarii. Ogólnie rzecz biorąc, jak wszystko, co możemy bąbelków zapasowej przydatnych informacji o awarii natywnych i zarządzanych ułatwić szybkie rozwiązywanie problemów związanych z.

Bez zbyt ugrzęźnięcia z przyczyn technicznych Dlaczego, konfigurowania infrastruktury wyłapywanie wyjątków zarządzanych w każdej granicy zarządzane lub natywne jest-trivially kosztowne i istnieją _partii_ przejść, które pojawiają się w wiele typowych operacji. Wiele operacji, w szczególności tych, które wymagają wątka UI musi zakończyć się szybko lub aplikacji spowoduje odtwarzanie przerywane i mają charakterystyki wydajności nie do przyjęcia. Wiele z tych wywołań zwrotnych zrobić bardzo proste rzeczy, które rzadko mają możliwość zgłaszanie, ten narzut zarówno będzie kosztowne i niepotrzebne w takich przypadkach.

W związku z tym, czy firma Microsoft nie spróbuj skonfigurować te / przechwytuje dla Ciebie. Dla miejsca, w którym kod ma nieuproszczone rzeczy (ponad Powiedz, zwracanie wartości logicznych lub prostego matematyczne), możesz spróbować samodzielnie catch. 

