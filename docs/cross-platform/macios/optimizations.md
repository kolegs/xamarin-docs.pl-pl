---
title: Tworzenie optymalizacji
description: W tym dokumencie opisano różne optymalizacje, które są stosowane w czasie kompilacji dla aplikacji Xamarin.iOS i Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: 432511a35f9f285d2c0060b5521256f834bb35ea
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351642"
---
# <a name="build-optimizations"></a>Tworzenie optymalizacji

W tym dokumencie opisano różne optymalizacje, które są stosowane w czasie kompilacji dla aplikacji Xamarin.iOS i Xamarin.Mac.

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>Usuń UIApplication.EnsureUIThread / NSApplication.EnsureUIThread

Usuwa wywołania [UIApplication.EnsureUIThread] [ 1] (dla platformy Xamarin.iOS) lub `NSApplication.EnsureUIThread` (dla platformy Xamarin.Mac).

Tego rodzaju optymalizacji zmiany następującego typu kodu:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    global::UIKit.UIApplication.EnsureUIThread ();
    // ...
}
```

do poniższej:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    // ...
}
```

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i są stosowane tylko do metod z `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Domyślnie jest ono włączone dla wersji kompilacji.

Zachowanie domyślne można przesłonić, przekazując `--optimize=[+|-]remove-uithread-checks` do mtouch/mmp.

[1]: https://developer.xamarin.com/api/member/UIKit.UIApplication.EnsureUIThread/

## <a name="inline-intptrsize"></a>IntPtr.Size wbudowane

Inlines wartości stałej z `IntPtr.Size` zależnie od platformy docelowej.

Tego rodzaju optymalizacji zmiany następującego typu kodu:

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

do następujących (podczas kompilowania dla platformy 64-bitowej):

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i są stosowane tylko do metod z `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Domyślnie jest włączone, jeśli pojedynczy architektury lub dla zestawu platform (**pliku Xamarin.iOS.dll**, **Xamarin.TVOS.dll**, **Xamarin.WatchOS.dll** lub  **Xamarin.Mac.dll**).

Jeśli przeznaczony dla wielu architektur, tego rodzaju optymalizacji utworzy różne zestawy dla wersji 32-bitowych i 64-bitowej wersji aplikacji, a obie wersje będą musiały zostać uwzględnione w aplikacji, efektywnie zwiększenie rozmiaru ostatecznej aplikacji zamiast zmniejszać go.

Zachowanie domyślne można przesłonić, przekazując `--optimize=[+|-]inline-intptr-size` do mtouch/mmp.

## <a name="inline-nsobjectisdirectbinding"></a>NSObject.IsDirectBinding wbudowane

`NSObject.IsDirectBinding` jest właściwością wystąpienia, która określa, czy jest konkretnego wystąpienia typu otoki, czy nie (typ otoki jest typ zarządzany, który mapuje do typu macierzystego; dla wystąpienia zarządzanego `UIKit.UIView` typu map do natywnych `UIView` typ - przeciwieństwo jest typu użytkownika w tym przypadku `class MyUIView : UIKit.UIView` będzie typ użytkownika).

Należy znać wartość `IsDirectBinding` podczas wywoływania w języku Objective-C, ponieważ wartość określa, którą wersję `objc_msgSend` do użycia.

Biorąc pod uwagę następujący kod:

```csharp
class UIView : NSObject {
    public virtual string SomeProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class NSUrl : NSObject {
    public virtual string SomeOtherProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class MyUIView : UIView {
}
```

Można określić w `UIView.SomeProperty` wartość `IsDirectBinding` nie jest stała i nie może być śródwierszowa:

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

Jednak jest możliwe, aby przyjrzeć się wszystkie typy w aplikacji i określić, że nie ma żadnych typów, które dziedziczą z `NSUrl`, i dlatego jest bezpieczne w tekście `IsDirectBinding` wartość stałej `true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

W szczególności, tego rodzaju optymalizacji spowoduje zmianę następującego typu kodu (jest to kod powiązania `NSUrl.AbsoluteUrl`):

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

do następujących (gdy można określić, że nie istnieją żadne podklasy `NSUrl` w aplikacji):

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i są stosowane tylko do metod z `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Jest zawsze domyślnie włączone dla platformy Xamarin.iOS i zawsze domyślnie wyłączona dla platformy Xamarin.Mac (ponieważ jest możliwe dynamicznie ładować zestawy platformie Xamarin.Mac, go nie jest możliwe ustalenie, czy konkretnej klasy nigdy nie jest podklasą klasy).

Zachowanie domyślne można przesłonić, przekazując `--optimize=[+|-]inline-isdirectbinding` do mtouch/mmp.

## <a name="inline-runtimearch"></a>Runtime.Arch wbudowane

Tego rodzaju optymalizacji zmiany następującego typu kodu:

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

do następujących (podczas tworzenia urządzenia):

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i są stosowane tylko do metod z `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Jest ona zawsze włączone domyślnie dla platformy Xamarin.iOS (nie jest dostępna dla platformy Xamarin.Mac).

Zachowanie domyślne można przesłonić, przekazując `--optimize=[+|-]inline-runtime-arch` do mtouch.

## <a name="dead-code-elimination"></a>Eliminacja nieużywany kod

Tego rodzaju optymalizacji zmiany następującego typu kodu:

```csharp
if (true) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

do:

```csharp
Console.WriteLine ("Doing this");
```

Ocenia się również porównań stałych, takich jak to:

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

i określić, że wyrażenie `8 == 8` jest zawsze wartość PRAWDA, a jej do zmniejszenia:

```csharp
Console.WriteLine ("Doing this");
```

Jest to zaawansowane optymalizacji, gdy jest używana wraz z wbudowanie optymalizacje, ponieważ można przekształcać w następujący typ kodu (jest to kod powiązania `NFCIso15693ReadMultipleBlocksConfiguration.Range`):

```csharp
NSRange ret;
if (IsDirectBinding) {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret (out ret, this.Handle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    }
} else {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret (out ret, this.SuperHandle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    }
}
return ret;
```

do tego (Tworzenie aplikacji dla urządzeń 64-bitowych, a także możliwość zapewnienia istnieją nie `NFCIso15693ReadMultipleBlocksConfiguration` podklasy w aplikacji):

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

Kompilator AOT już jest w stanie wyeliminować nieużywany kod tak, ale tego rodzaju optymalizacji odbywa się wewnątrz konsolidatora, co oznacza, że konsolidator można zobaczyć, że istnieje wiele metod, które nie są już używane i mogą zostać usunięte, więc (chyba że są używane w innych miejscach) :

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i są stosowane tylko do metod z `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Zawsze jest włączone domyślnie (jeśli konsolidator jest włączona).

Zachowanie domyślne można przesłonić, przekazując `--optimize=[+|-]dead-code-elimination` do mtouch/mmp.

## <a name="optimize-calls-to-blockliteralsetupblock"></a>Optymalizowanie wywołania BlockLiteral.SetupBlock

Środowisko uruchomieniowe Xamarin.iOS/Mac musi wiedzieć, podpis blok podczas tworzenia blokiem języka Objective-C dla zarządzanej delegować. Może to być to dość kosztowna operacja. Tego rodzaju optymalizacji obliczyć podpisu bloku w czasie kompilacji i zmodyfikuj IL, aby wywołać `SetupBlock` metodę, która zamiast tego przyjmuje podpis jako argument. W ten sposób pozwala uniknąć konieczności obliczanie podpisu w czasie wykonywania.

Testy porównawcze pokazują, że przyspiesza to wywołanie bloku współczynnik od 10 do 15.

Spowoduje przekształcenie, następujące [kodu](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211):

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlock (callback, completionHandler);
    // ...
}
```

do:

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlockImpl (callback, completionHandler, true, "v@?B");
    // ...
}
```

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i są stosowane tylko do metod z `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Włączono domyślnie podczas przy użyciu rejestratora statyczne (w rozszerzeniu Xamarin.iOS statyczne rejestratora jest domyślnie włączona dla kompilacji urządzenia, a w przypadku statycznej rejestrator jest domyślnie włączone dla wersji platformy Xamarin.Mac kompilacji).

Zachowanie domyślne można przesłonić, przekazując `--optimize=[+|-]blockliteral-setupblock` do mtouch/mmp.

## <a name="optimize-support-for-protocols"></a>Optymalizowanie obsługuje protokoły

Środowisko uruchomieniowe Xamarin.iOS/Mac potrzebuje informacji o zarządzanych protokołów implementuje języka Objective-C typów. Te informacje są przechowywane w interfejsach (i atrybuty w tych interfejsów), który nie jest bardzo wydajny format, ani nie jest przyjaznego dla konsolidatora.

Przykładem jest, że te interfejsy są przechowywane informacje na temat wszystkie składowe protokołu w `[ProtocolMember]` atrybut, który zawiera odwołania do typów parametru tych członków, między innymi. Oznacza to, że wdrożenie takiego interfejsu spowoduje, że konsolidator zachować wszystkie typy używane w tym interfejsie, nawet w przypadku członków opcjonalne aplikacji nigdy nie wywołuje ani nie implementuje.

Tego rodzaju optymalizacji spowoduje, że statyczne Rejestrator przechowywać wszystkie informacje wymagane w formacie wydajne, który używa mała ilość pamięci, która jest łatwe i Szybkie odnajdowanie w środowisku uruchomieniowym.

On również nauczy konsolidator, nie zawsze musi zachować te interfejsy ani żaden z powiązanych atrybutów.

Tego rodzaju optymalizacji wymaga zarówno konsolidator i statyczne rejestratora, do włączenia.

Xamarin.iOS tego rodzaju optymalizacji jest włączony domyślnie konsolidator i statyczne rejestratora są włączone.

Na platformy Xamarin.Mac tego rodzaju optymalizacji nigdy nie włączono domyślnie, ponieważ platformy Xamarin.Mac obsługuje ładowanie zestawy dynamiczne, a te zestawy mogą nie zostały znany w czasie kompilacji (i dlatego nie jest zoptymalizowana,).

Zachowanie domyślne można przesłonić, przekazując `--optimize=-register-protocols` do mtouch/mmp.

## <a name="remove-the-dynamic-registrar"></a>Usuwanie rejestratora dynamiczne

Środowisko uruchomieniowe platformy Xamarin.Mac i Xamarin.iOS obsługują [rejestrowanie typów zarządzanych](https://developer.xamarin.com/guides/ios/advanced_topics/registrar/) ze środowiskiem uruchomieniowym języka Objective-C. Albo można to zrobić w czasie kompilacji lub w czasie wykonywania (lub częściowo znajduje się w czasie kompilacji i rest w czasie wykonywania), ale jeśli całkowicie odbywa się w czasie kompilacji, oznacza to, że pomocnicze kod działa w czasie wykonywania, można je usunąć. Powoduje to znaczny spadek rozmiar aplikacji, w szczególności dla mniejszych aplikacje, takie jak rozszerzenia lub aplikacje systemu watchOS.

Tego rodzaju optymalizacji wymaga zarówno statyczne rejestratora i konsolidator włączyć.

Konsolidator spróbuje określić, czy jest bezpiecznie usunąć rejestratora dynamiczne i if, aby spróbować go usunąć.

Ponieważ platformy Xamarin.Mac obsługuje dynamicznie podczas ładowania zestawów w czasie wykonywania, (które nie były znane w czasie kompilacji), jest niemożliwe do należy określić w czasie kompilacji, czy jest to bezpieczne optymalizacji. Oznacza to, czy tego rodzaju optymalizacji nigdy nie jest włączone domyślnie dla aplikacji platformy Xamarin.Mac.

Zachowanie domyślne można przesłonić, przekazując `--optimize=[+|-]remove-dynamic-registrar` do mtouch/mmp.

Jeśli wartość domyślna jest zastępowany można usunąć rejestratora dynamiczne, konsolidator wyemitują ostrzeżenia, jeśli wykryje, że nie jest bezpieczne (ale dynamiczne rejestratora nadal zostaną usunięte).

## <a name="inline-runtimedynamicregistrationsupported"></a>Runtime.DynamicRegistrationSupported wbudowane

Inlines wartość z `Runtime.DynamicRegistrationSupported` ustalony w czasie kompilacji.

W przypadku usunięcia dynamiczne rejestratora (zobacz [usunąć rejestratora dynamiczne](#remove-the-dynamic-registrar) optymalizacji), jest stałą `false` wartość, w przeciwnym razie stałą `true` wartość.

Tego rodzaju optymalizacji zmiany następującego typu kodu:

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

w następujących po usunięciu dynamiczne rejestratora:

```csharp
throw new Exception ("dynamic registration is not supported");
```

w następujących rejestratora dynamiczne nie są usuwane:

```csharp
Console.WriteLine ("do something");
```

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i są stosowane tylko do metod z `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Zawsze jest włączone domyślnie (jeśli konsolidator jest włączona).

Zachowanie domyślne można przesłonić, przekazując `--optimize=[+|-]inline-dynamic-registration-supported` do mtouch/mmp.

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Precompute metody służące do tworzenia zarządzanych obiektów delegowanych bloków języka Objective-C

Kiedy języka Objective-C wywołuje selektora która przyjmuje bloku jako parametr, a następnie zarządzanego kodu został zastąpiony tej metody rozszerzenia Xamarin.iOS / środowiska uruchomieniowego platformy Xamarin.Mac potrzebne do utworzenia delegata dla tego bloku.

Uwzględni kod powiązania wygenerowany przez generator powiązań `[BlockProxy]` atrybut, który określa typ z `Create` metody, które można to zrobić.

Biorąc pod uwagę następujący kod języka Objective-C:

```objc
@interface ObjCBlockTester : NSObject {
}
-(void) classCallback: (void (^)())completionHandler;
-(void) callClassCallback;
@end

@implementation ObjCBlockTester
-(void) classCallback: (void (^)())completionHandler
{
}

-(void) callClassCallback
{
    [self classCallback: ^()
    {
        NSLog (@"called!");
    }];
}
@end
```

i następujący kod powiązania:

```csharp
[BaseType (typeof (NSObject))]
interface ObjCBlockTester
{
    [Export ("classCallback:")]
    void ClassCallback (Action completionHandler);
}
```

generator dadzą:

```csharp
[Register("ObjCBlockTester", true)]
public unsafe partial class ObjCBlockTester : NSObject {
    // unrelated code...

    [Export ("callClassCallback")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public virtual void CallClassCallback ()
    {
        if (IsDirectBinding) {
            ApiDefinition.Messaging.void_objc_msgSend (this.Handle, Selector.GetHandle ("callClassCallback"));
        } else {
            ApiDefinition.Messaging.void_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("callClassCallback"));
        }
    }
    
    [Export ("classCallback:")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public unsafe virtual void ClassCallback ([BlockProxy (typeof (Trampolines.NIDActionArity1V0))] System.Action completionHandler)
    {
        // ...
        
    }
}

static class Trampolines
{
    [UnmanagedFunctionPointerAttribute (CallingConvention.Cdecl)]
    [UserDelegateType (typeof (System.Action))]
    internal delegate void DActionArity1V0 (IntPtr block);

    static internal class SDActionArity1V0 {
        static internal readonly DActionArity1V0 Handler = Invoke;
        
        [MonoPInvokeCallback (typeof (DActionArity1V0))]
        static unsafe void Invoke (IntPtr block) {
            var descriptor = (BlockLiteral *) block;
            var del = (System.Action) (descriptor->Target);
            if (del != null)
                del (obj);
        }
    }
    
    internal class NIDActionArity1V0 {
        IntPtr blockPtr;
        DActionArity1V0 invoker;
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe NIDActionArity1V0 (BlockLiteral *block)
        {
            blockPtr = _Block_copy ((IntPtr) block);
            invoker = block->GetDelegateForBlock<DActionArity1V0> ();
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        ~NIDActionArity1V0 ()
        {
            _Block_release (blockPtr);
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe static System.Action Create (IntPtr block)
        {
            if (block == IntPtr.Zero)
                return null;
            if (BlockLiteral.IsManagedBlock (block)) {
                var existing_delegate = ((BlockLiteral *) block)->Target as System.Action;
                if (existing_delegate != null)
                    return existing_delegate;
            }
            return new NIDActionArity1V0 ((BlockLiteral *) block).Invoke;
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        unsafe void Invoke ()
        {
            invoker (blockPtr);
        }
    }
}
```

Kiedy wywołuje języka Objective-C `[ObjCBlockTester callClassCallback]`, rozszerzenia Xamarin.iOS / środowiska uruchomieniowego platformy Xamarin.Mac przyjrzymy się `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` atrybutu w parametrze. Następnie zostanie wyszukać `Create` metody w ramach tego typu i wywołanie tej metody do utworzenia delegata.

Tego rodzaju optymalizacji znajdzie `Create` metody w czasie kompilacji i statyczne rejestratora wygeneruje kod, który odwołuje się do metody w czasie wykonywania, zamiast tego za pomocą odbicia (jest to znacznie szybciej i umożliwia także konsolidator oraz atrybutu tokeny metadanych Aby usunąć odpowiedni kod środowiska uruchomieniowego, tworzenie mniejszych aplikacji).

Jeśli nie można odnaleźć mmp/mtouch `Create` metoda, następnie będzie wyświetlane ostrzeżenie MT4174/MM4174 i wyszukiwania będą wykonywane w czasie wykonywania zamiast tego.
Najbardziej prawdopodobna przyczyna ręcznie napisano kod powiązania bez wymaganych `[BlockProxy]` atrybutów.

Tego rodzaju optymalizacji wymaga statycznego rejestratora do włączenia.

Zawsze jest włączone domyślnie (tak długo, jak statycznych rejestrator jest włączony).

Zachowanie domyślne można przesłonić, przekazując `--optimize=[+|-]static-delegate-to-block-lookup` do mtouch/mmp.
