---
title: Selektory języka Objective-C w rozszerzeniu Xamarin.iOS
description: W tym dokumencie omówiono sposób interakcji z selektory języka Objective-C za pomocą języka C#. Przedstawiono sposób wywołania selektory i zagadnienia techniczne, które należy wziąć pod uwagę podczas wykonywania czynności.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/12/2017
ms.openlocfilehash: 3083770fd2874eca317585b6bf949f3efe56f879
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351733"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Selektory języka Objective-C w rozszerzeniu Xamarin.iOS

Język Objective-C opiera się na *selektory*. Selektor jest komunikat, który można wysłać do obiektu lub *klasy*. [Xamarin.iOS](~/ios/internals/api-design/index.md) mapy wystąpienia selektory metody wystąpienia i klasy selektory do metod statycznych.

W przeciwieństwie do normalnej funkcji języka C (i takich jak funkcji składowych języka C++), nie można bezpośrednio wywoływać za pomocą selektora [P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/).
(*Specjalnie*: teoretycznie można użyć metody P/Invoke wirtualnych funkcji składowych języka C++, ale będziesz potrzebować martwić się o poszczególnych kompilatora przekręcaniu nazwy, czyli świata ból lepiej jest ignorowane.) Zamiast tego selektory są wysyłane do klasy języka Objective-C lub przy użyciu [ `objc_msgSend` funkcja](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend).

Może się okazać [tego przewodnika pomocne komunikaty języka Objective-C](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html) przydatne.

<a name="Example" />

## <a name="example"></a>Przykład

Załóżmy, że chcesz wywołać [-[NSString sizeWithFont:forWidth:lineBreakMode:]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:) selektora.
Deklaracja (z dokumentacji firmy Apple) jest:

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  Typ zwracany jest *CGSize* do ujednoliconego interfejsu API.
-  *Czcionki* parametr jest [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (i typ (pośrednio) pochodzący od [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ), a w związku z tym jest mapowany na [System.IntPtr](xref:System.IntPtr) .
-  *Szerokość* parametru *CGFloat* , jest mapowany na *nfloat*.
-  *LineBreakMode* parametru *UILineBreakMode* , jest już powiązane w rozszerzeniu Xamarin.iOS jako [wyliczenie UILineBreakMode](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/) .


Zebranie wszystkich i chcemy, aby deklaracji objc_msgSend, który odpowiada:

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

Firma Microsoft będzie należy zadeklarować ją:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Po zadeklarowaniu, firma Microsoft wywoływać go, gdy będziemy już mieć odpowiednie parametry:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode);
```

Zwrócona wartość była to struktura, która była mniejsza niż 8 bajtów (takich jak starszej wersji `SizeF` używane przed przełączeniem do interfejsów API Unified) powyższy kod powinny zostać uruchomione w symulatorze, ale które uległy awarii na urządzeniu. Tak, aby wywołać selektor, która nie zwraca wartości mniejszej niż 8 bitów rozmiar, w związku z tym firma Microsoft *również* musi zadeklarować `objc_msgSend_stret()` funkcji:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Następnie stałyby wywołania:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode);
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode);
```


<a name="Invoking_a_Selector" />

## <a name="invoking-a-selector"></a>Wywoływanie selektora

Wywoływanie selektora składa się z trzech kroków:

1.  Pobierz element docelowy selektora.
1.  Pobierz nazwę selektora.
1.  Wywołaj objc_msgSend() z odpowiednimi argumentami.


<a name="Selector_Targets" />

### <a name="selector-targets"></a>Selektor obiektów docelowych

Obiekt docelowy selektor jest wystąpienie obiektu lub klasą języka Objective-C. Jeśli element docelowy jest wystąpieniem i pochodzi z typem powiązanej Xamarin.iOS, użyj [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) właściwości.

Jeśli element docelowy jest klasą, użyj [ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) Aby pobrać odwołanie do wystąpienia klasy, a następnie użyj [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) właściwości.


<a name="Selector_Names" />

### <a name="selector-names"></a>Selektor nazwy

Selektor nazw są wymienione w ramach dokumentacji firmy Apple. Na przykład [metody rozszerzenia UIKit NSString](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html) obejmują [sizeWithFont:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:) i [sizeWithFont:forWidth:lineBreakMode:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:). W dwukropki osadzone i końcowe są ważne i są częścią nazwy selektora.

Po utworzeniu nazwy selektora, można utworzyć [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) wystąpienia dla niego.


<a name="Calling_objc_msgSend()" />

### <a name="calling-objcmsgsend"></a>Wywoływanie objc_msgSend()

 `objc_msgSend()` Służy do wysyłania wiadomości (selektor) do obiektu. Tej rodziny funkcji ma co najmniej dwa argumenty wymagane: docelowy selektor (wystąpienia lub klasy obsługi), sam selektor i następnie żadnych argumentów, które są wymagane dla określonego selektora. Wystąpienia i selektor argumenty muszą być `System.IntPtr`, a wszystkie pozostałe argumenty musi być zgodny typ oczekuje selektor, np. `nint` dla `int`, lub `System.IntPtr` dla wszystkich `NSObject`— typy pochodne. Użyj [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) właściwości w celu uzyskania `IntPtr` dla wystąpienia typu języka Objective-C.

Niestety, jest więcej niż jeden `objc_msgSend()` funkcji.

Użyj [ `objc_msgSend_stret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) na selektorów, które zwraca strukturę.
Aby zachować "interesująca", na ARM to obejmuje wszystkie zwracane typy, które są *nie* wyliczenia lub typy wbudowane C (char, short, int, long, float, double). Na x86 (symulator) to musi być używane dla wszystkich struktur większe niż 8 bajtów. (CGSize — użytych w przykładzie powyżej — 8 bajtów dokładnie i dlatego nie za pomocą `objc_msgSend_stret()` w symulatorze.)

Użyj [ `objc_msgSend_fpret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret) dla selektorów, które zwraca zmiennoprzecinkową polecenie x86 tylko wartości. Ta funkcja nie trzeba było używać na ARM; Zamiast tego należy użyć `objc_msgSend()`.

Głównym [objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend) funkcja jest używana dla wszystkich innych selektorów.

Po podjęciu, który `objc_msgSend()` funkcji potrzebne do wywoływania (i może być więcej niż jeden, np. dla symulator i urządzenia), można użyć jako normalny [ `[DllImport]` ](xref:System.Runtime.InteropServices.DllImportAttribute) metodę, aby zadeklarować funkcję dla nowszych wywołania.

Zestaw wstępnie przygotowanych `objc_msgSend()` deklaracje znajdują się w [ `ObjCRuntime.Messaging` ](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/).


<a name="ugly" />

## <a name="the-ugly"></a>Fałszywych

Języka Objective-C ma trzy typy z `objc_msgSend` metody: jeden dla wywołania regularnych: jeden dla wywołań, które zwracają wartości zmiennoprzecinkowe (tylko x86) i jeden dla wywołań, które zwracają wartości struktury. Obejmują one sufiks `_stret` w `ObjCRuntime.Messaging`.

Jeśli to wywołanie metody, która zwróci niektórych struktur (zasady są opisane poniżej), należy wywołać metody z wartością zwracaną jako pierwszy parametr, poza wartością następująco:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Elementy Pobierz nieładnego tutaj i dlatego kiedy należy używać dla reguły _stret_ różni się na X86 i ARM, jeśli chcesz, aby powiązań do pracy w symulatorze i urządzenia, należy dodać kod, który wygląda w następujący sposób:

```csharp
if (Runtime.Arch == Arch.DEVICE){
    PointF ret;

    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);

    return ret;
} else
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
```

### <a name="using-the-objcmsgsendstret-method"></a>Za pomocą objc\_msgSend\_stret — metoda

Reguły dotyczące używania [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) są podobne do tego zadania dla **ARM**:

-  Wartość dowolnego typu, który nie jest wyliczeniem lub dowolnego typu podstawowego dla wyliczenia (int, byte, short, long, double, float).


Reguły dotyczące używania [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) są podobne do tego zadania dla **X86**:

-  Wartość dowolnego typu, który nie jest wyliczeniem lub dowolnego typu podstawowego dla wyliczenia (int, byte, short, long, double, float) i którego macierzysty rozmiar jest większy niż 8 bajtów.


### <a name="creating-your-own-signatures"></a>Tworzenie własnych podpisów.

Następujące [gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) może służyć do tworzenia własnych podpisów, jeśli jest to wymagane.



## <a name="related-links"></a>Linki pokrewne

- [Przykład selektorów](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
