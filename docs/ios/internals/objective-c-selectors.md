---
title: Selektory Objective-C w Xamarin.iOS
description: Ten dokument omówiono sposób interakcji z selektorów Objective-C w języku C#. Przedstawiono sposób wywołania i Uwagi techniczne, które należy wziąć pod uwagę, gdy taka selektorów.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 25276851879ba828361d3236cbf7896cf748588c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787045"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Selektory Objective-C w Xamarin.iOS

Języka Objective C jest oparty na *selektorów*. Selektora jest komunikat, który można wysłać do obiektu lub *klasy*. [Xamarin.iOS](~/ios/internals/api-design/index.md) mapy wystąpienia selektorów, które metody wystąpienia i klasy selektorów, które metody statyczne.

W przeciwieństwie do normalnej funkcji języka C (i jak funkcji Członkowskich C++), nie można bezpośrednio wywołać przy użyciu selektora [P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/).
(*Odłóż*: teoretycznie można użyć P/Invoke dla funkcji Członkowskich C++-virtual, ale musisz martwić się o poszczególnych kompilatora przekręcona nazwa, czyli świat słabe lepiej jest ignorowane.) Zamiast tego selektory są wysyłane do klasy Objective-C lub wystąpienia, za pomocą [ `objc_msgSend` funkcja](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend).

Może się okazać [tego przewodnika pomocne w wiadomości Objective-C](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html) przydatne.

<a name="Example" />

## <a name="example"></a>Przykład

Załóżmy, że chcesz wywołać [-[NSString sizeWithFont:forWidth:lineBreakMode:]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:) selektora.
Deklaracja (z dokumentacji firmy Apple) jest:

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  Typ zwracany jest *CGSize* dla interfejsu API Unified.
-  *Czcionki* parametr jest [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (i pochodnych (pośrednio) typu [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ) i w związku z tym jest mapowany na [System.IntPtr](https://developer.xamarin.com/api/type/System.IntPtr/) .
-  *Szerokość* parametru *CGFloat* , jest mapowany na *nfloat*.
-  *LineBreakMode* parametru *UILineBreakMode* , jest już powiązane w Xamarin.iOS jako [wyliczenie UILineBreakMode](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/) .


Zebranie wszystkich elementów i chcemy deklaracji objc_msgSend, który odpowiada:

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

Musimy Zadeklaruj ją:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Po zadeklarowaniu, możemy go wywołać, gdy mamy odpowiednie parametry:

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

Zwrócona wartość była struktury, która była mniejsza niż 8 bajtów (takich jak starszej `SizeF` używane przed przełączeniem do interfejsów API Unified) powyżej kodu powinny zostać uruchomione w symulatorze, ale awaria na urządzeniu. Tak, aby wywołać selektora, która zwraca wartość mniejsza niż 8 bitów w rozmiarze, w związku z tym firma Microsoft *również* musi zadeklarować `objc_msgSend_stret()` funkcji:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Następnie staje się wywołania:

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

Wywoływanie selektora ma trzy kroki:

1.  Pobierz element docelowy selektora.
1.  Pobierz nazwę selektora.
1.  Wywołanie objc_msgSend() z odpowiednią liczbą argumentów.


<a name="Selector_Targets" />

### <a name="selector-targets"></a>Selektor obiektów docelowych

Element docelowy selektora jest wystąpienia obiektu lub klasy Objective-C. Jeśli element docelowy jest wystąpieniem i pochodzi od typu Xamarin.iOS powiązane, użyj [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) właściwości.

Jeśli obiektem docelowym jest klasa, użyj [ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) Aby pobrać odwołanie do wystąpienia klasy, a następnie użyj [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) właściwości.


<a name="Selector_Names" />

### <a name="selector-names"></a>Selektor nazwy

Selektor nazw znajdują się w dokumentacji firmy Apple. Na przykład [metody rozszerzenia UIKit NSString](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html) obejmują [sizeWithFont:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:) i [sizeWithFont:forWidth:lineBreakMode:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:). Dwukropki osadzonych i końcowe są ważne, a Nazwa selektora.

Po utworzeniu nazwa selektora, możesz utworzyć [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) wystąpienia dla niego.


<a name="Calling_objc_msgSend()" />

### <a name="calling-objcmsgsend"></a>Wywoływanie objc_msgSend()

 `objc_msgSend()` Służy do wysyłania wiadomości (selektor) do obiektu. Tej rodziny funkcji ma co najmniej dwóch wymaganych argumentów: element docelowy selektora (wystąpień lub klasy obsługi), sam selektor i następnie żadnych argumentów, które są wymagane dla określonego selektora. Wystąpienie i selektor argumenty muszą być `System.IntPtr`, a wszystkie pozostałe argumenty musi odpowiadać typowi selektor oczekuje, np. `nint` dla `int`, lub `System.IntPtr` dla wszystkich `NSObject`-typów pochodnych. Użyj [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) właściwości w celu uzyskania `IntPtr` dla języka Objective-C wystąpienia typu.

Niestety, jest więcej niż jednym `objc_msgSend()` funkcji.

Użyj [ `objc_msgSend_stret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) dla selektorów, które zwrócić struktury.
Aby zapewnić "sekcja", na ARM ten zawiera wszystkie zwracane typy, które są *nie* wyliczenie lub typy wbudowane C (char, short, int, long, float, double). Na x86 (symulatora) musi on zostać można używać dla wszystkich struktury większy niż rozmiar 8 bajtów. (CGSize — używane w przykładzie powyżej — jest dokładnie 8 bajtów i w związku z tym nie używa `objc_msgSend_stret()` w symulatorze.)

Użyj [ `objc_msgSend_fpret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret) dla selektorów zwracające wartość zmiennoprzecinkową polecenie x86 tylko wartości. Ta funkcja nie trzeba używać na ARM; Zamiast tego należy użyć `objc_msgSend()`.

Głównym [objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend) funkcja jest używana dla wszystkich innych selektorów.

Gdy uznasz, które `objc_msgSend()` funkcje należy wywołać (i może być więcej niż jedną, np. dla symulator i urządzenia), można użyć jako normalny [ `[DllImport]` ](https://developer.xamarin.com/api/type/System.Runtime.InteropServices.DllImportAttribute/) metodę, aby zadeklarować funkcji dla nowszej wywołania.

Zestaw predefiniowanych `objc_msgSend()` deklaracje znajdują się w [ `ObjCRuntime.Messaging` ](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/).


<a name="ugly" />

## <a name="the-ugly"></a>Fałszywych

Objective-C ma trzy typy z `objc_msgSend` metody: jeden dla wywołań regularnych: jeden dla wywołań, które zwracają wartości zmiennoprzecinkowych (tylko x86) i jeden dla wywołań, które zwracają wartości struktury. Obejmują one sufiks `_stret` w `ObjCRuntime.Messaging`.

Jeśli są wywołania metody, która zwróci niektórych struktur (zasady są opisane poniżej), należy wywołać metodę z wartością zwracaną jako pierwszego parametru jako poza wartością, jak to:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Rzeczy pobrać ugly tutaj i dlatego kiedy należy używać dla reguły _stret_ różni się na X86 i ARM, jeśli chcesz, aby powiązania do pracy na symulatorze i urządzenie, należy dodać kodu, który wygląda następująco:

```csharp
if (Runtime.Arch == Arch.DEVICE){
    PointF ret;

    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);

    return ret;
} else
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
```

### <a name="using-the-objcmsgsendstret-method"></a>Przy użyciu objc\_msgSend\_stret — metoda

Kiedy należy używać dla reguły [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) są podobne dla **ARM**:

-  Wszystkie wartości typu, który nie jest wyliczeniem ani dowolnego typu podstawowego dla wyliczenia (int, byte, short, long, double, float).


Kiedy należy używać dla reguły [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) są podobne dla **X86**:

-  Wszystkie wartości typu, który nie jest wyliczeniem ani dowolnego typu podstawowego dla wyliczenia (int, byte, short, long, double, float) i którego mają macierzysty rozmiar jest większy niż 8 bajtów.


### <a name="creating-your-own-signatures"></a>Tworzenie własnych podpisów.

Następujące [gist](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) może służyć do tworzenia własnych podpisów, jeśli jest to wymagane.



## <a name="related-links"></a>Linki pokrewne

- [Przykład selektorów.](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
