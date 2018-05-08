---
title: Tworzenie optymalizacji
description: W tym dokumencie opisano różne funkcje optymalizacji, które są stosowane w czasie kompilacji dla aplikacji platformy Xamarin.iOS i Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
dateupdated: 04/16/2018
ms.openlocfilehash: 42d9903e75267a9578fb320b0bc532ad4f0fd71a
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="build-optimizations"></a>Tworzenie optymalizacji

W tym dokumencie opisano różne funkcje optymalizacji, które są stosowane w czasie kompilacji dla aplikacji platformy Xamarin.iOS i Xamarin.Mac.

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>Usuń UIApplication.EnsureUIThread / NSApplication.EnsureUIThread

Usuwa wywołań [UIApplication.EnsureUIThread] [ 1] (dla platformy Xamarin.iOS) lub `NSApplication.EnsureUIThread` (dla Xamarin.Mac).

Tego rodzaju optymalizacji zmieni typ następującego kodu:

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

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i jest stosowana tylko do metod `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Domyślnie jest włączona dla wersji kompilacji.

Zachowanie domyślne można przesłonić przez przekazanie `--optimize=[+|-]remove-uithread-checks` do mtouch/mmp.

[1]: https://developer.xamarin.com/api/member/UIKit.UIApplication.EnsureUIThread/

## <a name="inline-intptrsize"></a>IntPtr.Size wbudowany

Inlines wartości stałej z `IntPtr.Size` zależnie od platformy docelowej.

Tego rodzaju optymalizacji zmieni typ następującego kodu:

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

do następujących (podczas kompilowania dla platformy 64-bitowe):

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i jest stosowana tylko do metod `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Domyślnie jest włączone, jeśli jako pojedynczy architektury docelowej lub zestawu platformy (**Xamarin.iOS.dll**, **Xamarin.TVOS.dll**, **Xamarin.WatchOS.dll** lub  **Xamarin.Mac.dll**).

Jeśli przeznaczonych dla wielu architektur, tego rodzaju optymalizacji utworzy różnych zestawów dla wersji 32-bitowych i 64-bitowej wersji aplikacji i obie wersje będą musiały zostać uwzględnione w aplikacji, efektywnie zwiększenie rozmiaru ostatecznej aplikacji zamiast zmniejszać się go.

Zachowanie domyślne można przesłonić przez przekazanie `--optimize=[+|-]inline-intptr-size` do mtouch/mmp.

## <a name="inline-nsobjectisdirectbinding"></a>NSObject.IsDirectBinding wbudowany

`NSObject.IsDirectBinding` jest właściwością wystąpienia, który określa, czy konkretne wystąpienie jest typu otoki lub nie (typ otoki jest typ zarządzany, który jest mapowany na typ macierzysty; dla wystąpienia zarządzanej `UIKit.UIView` typu map do natywnego `UIView` typ — przeciwieństwem jest typu użytkownika w tym przypadku `class MyUIView : UIKit.UIView` byłoby typu użytkownika).

Należy znać wartość `IsDirectBinding` podczas wywoływania metody w języku Objective-C, ponieważ wartość określa, która wersja `objc_msgSend` do użycia.

Podane tylko dla następującego kodu:

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

Można określić, że w `UIView.SomeProperty` wartość `IsDirectBinding` nie jest stała i nie można wbudować elementu:

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

Jednak jest możliwe przyjrzeć się wszystkie typy w aplikacji i określić, że nie ma żadnych typów, które dziedziczą z `NSUrl`, i w związku z tym jest bezpieczne w tekście `IsDirectBinding` wartość stałej `true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

W szczególności, tego rodzaju optymalizacji spowoduje zmianę następującego typu kodu (to jest kod powiązania `NSUrl.AbsoluteUrl`):

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

do następujących (jeśli można określić, czy nie występują żadne podklasy `NSUrl` w aplikacji):

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i jest stosowana tylko do metod `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Jest zawsze włączona domyślnie dla platformy Xamarin.iOS i zawsze domyślnie wyłączona dla Xamarin.Mac (można dynamicznie załadować zestawów w Xamarin.Mac, dlatego go nie jest możliwe określenie, czy określonej klasy nigdy nie jest podklasą klasy).

Zachowanie domyślne można przesłonić przez przekazanie `--optimize=[+|-]inline-isdirectbinding` do mtouch/mmp.

## <a name="inline-runtimearch"></a>Runtime.Arch wbudowany

Tego rodzaju optymalizacji zmieni typ następującego kodu:

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

do następujących (podczas budowania urządzenia):

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i jest stosowana tylko do metod `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Jest on zawsze włączone domyślnie dla platformy Xamarin.iOS (nie jest ona dostępna dla Xamarin.Mac).

Zachowanie domyślne można przesłonić przez przekazanie `--optimize=[+|-]inline-runtime-arch` do mtouch.

## <a name="dead-code-elimination"></a>Eliminacja martwy kod

Tego rodzaju optymalizacji zmieni typ następującego kodu:

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

Będzie on również ocenić, stałej porównań, jak to:

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

Jest to zaawansowane optymalizacji, gdy jest używany wraz ze śródwierszowaniem optymalizacje, ponieważ można przekształcać następujący typ kodu (to jest kod powiązania `NFCIso15693ReadMultipleBlocksConfiguration.Range`):

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

do tego (budowania urządzenia 64-bitowym, a także możliwość upewnij się, nie `NFCIso15693ReadMultipleBlocksConfiguration` podklasy w aplikacji):

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

Kompilator drzewa obiektów aplikacji już jest w stanie wyeliminować martwy kod w następujący sposób, ale ten rodzaj optymalizacji jest wykonywane wewnątrz konsolidatora, co oznacza, że konsolidator można zobaczyć, że istnieje wiele metod, które nie są już używane, a w związku z tym mogą zostać usunięte (chyba że są używane w innym miejscu) :

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i jest stosowana tylko do metod `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Jest on zawsze włączone domyślnie (przy włączonej konsolidator).

Zachowanie domyślne można przesłonić przez przekazanie `--optimize=[+|-]dead-code-elimination` do mtouch/mmp.

## <a name="optimize-calls-to-blockliteralsetupblock"></a>Optymalizacja wywołania BlockLiteral.SetupBlock

Środowisko uruchomieniowe Xamarin.iOS/Mac musi wiedzieć, podpis bloku podczas tworzenia blok języka Objective C dla zarządzanego delegowanie. Może to być dość kosztowna operacja. Tego rodzaju optymalizacji obliczyć podpis bloku w czasie kompilacji i zmodyfikować języka IL w celu wywołania `SetupBlock` metody pobierającej podpis jako argument zamiast tego. W ten sposób pozwala uniknąć konieczności obliczanie podpisu w czasie wykonywania.

Wzorce Pokaż to przyspiesza wywoływania bloku o 10 – 15.

Spowoduje przekształcenie następujące [kod](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211):

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

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i jest stosowana tylko do metod `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Jest ona włączona domyślnie po przy użyciu statycznych rejestratora (w Xamarin.iOS statycznych rejestratora jest domyślnie włączone dla kompilacji urządzenia i w Xamarin.Mac statycznych rejestratora jest domyślnie włączona dla wersji kompilacji).

Zachowanie domyślne można przesłonić przez przekazanie `--optimize=[+|-]blockliteral-setupblock` do mtouch/mmp.

## <a name="optimize-support-for-protocols"></a>Optymalizacja Obsługa protokołów

Środowisko uruchomieniowe Xamarin.iOS/Mac potrzebuje informacji o zarządzanych protokołów implementuje Objective-C typów. Te informacje są przechowywane w interfejsach (i atrybuty te interfejsy), który nie jest bardzo wydajny format, ani nie jest przyjaznym dla konsolidatora.

Przykładem jest, że te interfejsy przechowywane informacje o wszystkich członków protokołu `[ProtocolMember]` atrybut, który zawiera odwołania do typów parametrów elementu tych członków, między innymi. Oznacza to po prostu wykonania takiego interfejsu spowoduje, że konsolidator zachować wszystkie typy używane w tym interfejsie, nawet w przypadku opcjonalnych elementów członkowskich lub aplikacji w nigdy nie wywołuje implementuje.

Optymalizacja będzie statycznych rejestratora przechowywania wszelkich wymaganych informacji w formacie wydajne, który używa mała ilość pamięci, która jest bardzo proste i szybkie można znaleźć w czasie wykonywania.

On również udzieli konsolidator czy nie zawsze musi zachować te interfejsy ani żaden z powiązanych atrybutów.

Tego rodzaju optymalizacji wymaga zarówno konsolidator i statyczne rejestratora włączyć.

Na Xamarin.iOS Optymalizacja jest włączona domyślnie po zarówno konsolidator i statyczne rejestratora są włączone.

Na Xamarin.Mac Optymalizacja nigdy nie włączono domyślnie, ponieważ obsługuje Xamarin.Mac dynamicznie ładowania zestawów i te zestawy mogą nie zostały znane w czasie kompilacji (i w związku z tym nie jest zoptymalizowana).

Zachowanie domyślne można przesłonić przez przekazanie `--optimize=-register-protocols` do mtouch/mmp.

## <a name="remove-the-dynamic-registrar"></a>Usuń dynamiczne rejestratora

Zarówno Xamarin.iOS, jak i środowiska uruchomieniowego Xamarin.Mac obejmują obsługę [rejestrowanie typy zarządzane](https://developer.xamarin.com/guides/ios/advanced_topics/registrar/) ze środowiskiem uruchomieniowym języka Objective-C. Mogą to robić albo, w czasie kompilacji lub w czasie wykonywania (lub częściowo w czasie kompilacji i rest w czasie wykonywania), ale jeśli odbywa się całkowicie w czasie kompilacji, oznacza to, że kod obsługi to zrobić w czasie wykonywania mogą zostać usunięte. Powoduje to znaczące zmniejszenie rozmiaru aplikacji, w szczególności dla mniejszych aplikacje, takie jak rozszerzenia lub watchOS aplikacje.

Tego rodzaju optymalizacji wymaga zarówno statycznych rejestratora i konsolidator włączyć.

Konsolidator będzie podejmować próby określenia, czy jest bezpiecznie usunąć rejestratora dynamicznego, a jeśli tak będzie próbować usunąć go.

Ponieważ Xamarin.Mac dynamicznie obsługuje ładowanie zestawów w czasie wykonywania (które nie były znane w czasie kompilacji), istnieje możliwość określenia w czasie kompilacji, czy jest to bezpieczne optymalizacji. Oznacza to, że tego rodzaju optymalizacji nigdy nie jest włączona domyślnie w przypadku aplikacji Xamarin.Mac.

Zachowanie domyślne można przesłonić przez przekazanie `--optimize=[+|-]remove-dynamic-registrar` do mtouch/mmp.

Jeśli wartość domyślna jest zastąpiona w celu usunięcia dynamiczne rejestratora, konsolidator będzie Emituj ostrzeżenia w przypadku wykrycia, że nie jest bezpieczne (ale dynamiczne rejestratora nadal zostaną usunięte).

## <a name="inline-runtimedynamicregistrationsupported"></a>Runtime.DynamicRegistrationSupported wbudowany

Inlines wartość z `Runtime.DynamicRegistrationSupported` określone w czasie kompilacji.

Usunięcie dynamiczne rejestratora (zobacz [Usuń dynamiczne rejestratora](#remove-the-dynamic-registrar) optymalizacji), jest stałą `false` wartość, w przeciwnym razie jest stałą `true` wartość.

Tego rodzaju optymalizacji zmieni typ następującego kodu:

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

Tego rodzaju optymalizacji wymaga konsolidator, aby włączyć i jest stosowana tylko do metod `[BindingImpl (BindingImplOptions.Optimizable)]` atrybutu.

Jest on zawsze włączone domyślnie (przy włączonej konsolidator).

Zachowanie domyślne można przesłonić przez przekazanie `--optimize=[+|-]inline-dynamic-registration-supported` do mtouch/mmp.

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Precompute metody do tworzenia delegatów zarządzanych dla języka Objective-C bloków

Gdy Objective-C wywołuje selektora który przyjmuje bloku jako parametr, a następnie kod zarządzany zastąpiła tej metody, Xamarin.iOS / Xamarin.Mac runtime potrzebne do utworzenia delegata dla tego bloku.

Powiązanie kod wygenerowany przez generator powiązanie będzie zawierać `[BlockProxy]` atrybut, który określa typ do `Create` metody, które można to zrobić.

Podane następujący kod języka Objective-C:

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

generator utworzy:

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

Gdy wywołuje Objective-C `[ObjCBlockTester callClassCallback]`, Xamarin.iOS / Xamarin.Mac runtime weźmie pod uwagę `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` atrybutu w parametrze. Następnie zostanie wyszukiwanie `Create` metody dla tego typu i wywołanie tej metody można utworzyć obiektu delegowanego.

Tego rodzaju optymalizacji znajdzie `Create` metody w czasie kompilacji i statyczne rejestratora wygeneruje kod, który odwołuje się do metody w czasie wykonywania, zamiast niego przy użyciu atrybutu i odbicie (jest to znacznie szybsze i pozwala również konsolidator tokeny metadanych Aby usunąć odpowiedni kod środowiska uruchomieniowego, co aplikacja mniejszych).

Jeśli nie można odnaleźć mmp/mtouch `Create` metody, następnie będzie wyświetlane ostrzeżenie MT4174/MM4174 i wyszukiwania będzie można wykonać w czasie wykonywania zamiast tego.
Najbardziej prawdopodobną przyczyną jest zapisywany ręcznie powiązanie kodu bez wymaganych `[BlockProxy]` atrybutów.

Tego rodzaju optymalizacji wymaga statycznego rejestratora włączenia.

Jest on zawsze włączona domyślnie (o ile rejestratora statycznego jest włączona).

Zachowanie domyślne można przesłonić przez przekazanie `--optimize=[+|-]static-delegate-to-block-lookup` do mtouch/mmp.
