---
title: "Ogólny podklasy NSObject"
ms.topic: article
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 76c35cfb993bade324b25f86e75db7eb028a7399
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="generic-subclasses-of-nsobject"></a>Ogólny podklasy NSObject

## <a name="using-generics-with-nsobjects"></a>Użycie typów ogólnych z NSObjects

Uruchamianie Xamarin.iOS 7.2.1 można używać typów ogólnych w podklasy `NSObject` (na przykład [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/)).

Można teraz utworzyć klas rodzajowych podobne do następującego:

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

Ponieważ obiekty tego podklasy `NSObject` zostały zarejestrowane ze środowiskiem uruchomieniowym języka Objective-C istnieją pewne ograniczenia dotyczące możliwości programu ogólnego podklasy `NSObject` typów.
    
## <a name="considerations-for-generic-subclasses-of-nsobject"></a>Zagadnienia ogólne podklas NSObject

Ten dokument zawiera szczegóły ograniczenia w ograniczoną obsługę ogólnego podklasy `NSObjects` wprowadzone w systemie Xamarin.iOS 7.2.1.
    
### <a name="generic-type-arguments-in-member-signatures"></a>Argumenty typu ogólnego w podpisach elementu członkowskiego

Musi mieć wszystkie argumenty typu ogólnego w podpisie elementu członkowskiego ujawniony dla języka Objective-C `NSObject` ograniczenia.

**Dobrym**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Przyczyna**: parametr typu ogólnego jest `NSObject`, więc podpis selektora dla `myMethod:` może być bezpiecznie ujawniony dla języka Objective-C (zawsze będzie `NSObject` ani jej podklasy jej).

**Zły**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Przyczyna**: nie można utworzyć podpisu języka Objective C dla eksportowanych elementów członkowskich, które kod języka Objective C można wywołać, ponieważ podpis czy różnią się zależnie od typu dokładnego typu ogólnego jest `T`.

**Dobrym**:

```csharp
class Generic<T> : NSObject
{
    T storage;

    [Export ("myMethod:")]
    public void MyMethod (NSObject value)
    {
    }
}
```

**Przyczyna**: istnieje możliwość mogła argumentów typu ogólnego, dopóki nie przyjmują część podpisu wyeksportowanego elementu członkowskiego.

**Dobrym**:

```csharp
class Generic<T, U> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
        Console.WriteLine (typeof (U));
    }
}
```

**Przyczyna**: `T` wyeksportowane parametru w języku Objective C `MyMethod` jest ograniczona do `NSObject`, nieograniczonego typu `U` nie jest częścią podpisu.
    
### <a name="instantiations-of-generic-types-from-objective-c"></a>Wystąpień typów ogólnych z języka Objective C

Tworzenie wystąpień typów ogólnych z języka Objective C jest niedozwolone. Ten błąd zazwyczaj występuje podczas typu zarządzanego jest używany w xib.

Należy wziąć pod uwagę definicji klasy, który udostępnia konstruktora przyjmującego `IntPtr` (Xamarin.iOS sposób tworzenia obiektu C# z natywnego wystąpienia Objective-C):
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

Konstrukcja powyżej jest poprawnie, w czasie wykonywania, to spowoduje zgłoszenie wyjątku w języku Objective-C będzie próbować utworzyć wystąpienie.

To jest spowodowany Objective-C nie ma żadnych koncepcji typów ogólnych i nie można określić dokładną typu ogólnego, aby utworzyć.

Ten problem można pracować wokół tworząc specjalne podklasą typu ogólnego.   Na przykład:
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

Obecnie nie istnieje żadne niejednoznaczności, klasa `GenericUIView` mogą być używane w xibs.

## <a name="no-support-for-generic-methods"></a>Metody ogólne nie są obsługiwane

### <a name="generic-methods-are-not-allowed"></a>Metody ogólne nie są dozwolone.

Następujący kod nie zostanie skompilowany:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**Przyczyna**: jest to niedozwolone, ponieważ Xamarin.iOS nie może określić, którego typu użyć argumentu typu `T` po wywołaniu metody z języka Objective-C.

Alternatywą jest utworzenie specjalne metody i eksportowanie który zamiast tego:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyUIViewMethod (UIView argument)
    {
        MyMethod<UIView> (argument);
    }
    public void MyMethod<T> (T argument)
    {
    }
}
```

### <a name="no-exported-static-members-allowed"></a>Nie wyeksportowanego statyczne elementy członkowskie dozwolone

Nie mogą uwidaczniać statycznych elementów członkowskich do języka Objective-C, jeśli znajduje się wewnątrz podklasą ogólnego `NSObject`.

Przykład to nieobsługiwany scenariusz:

```csharp
class Generic<T> : NSObject where T : NSObject
{
    [Export ("myMethod:")]
    public static void MyMethod ()
    {
    }

    [Export ("myProperty")]
    public static T MyProperty { get; set; }
}
```

**Przyczyna:** podobnie jak metody rodzajowe potrzeb środowiska wykonawczego platformy Xamarin.iOS mogli wiedzieć, jaki typ można użyć argumentu typu ogólnego T.

Dla wystąpienia elementów członkowskich wystąpienia samej siebie jest używana (ponieważ nigdy nie będzie wystąpienia ogólny<T>, zawsze będzie ogólny<SomeSpecificClass>), ale dla statycznych elementów członkowskich tych informacji nie ma.

Należy pamiętać, że dotyczy to nawet wtedy, gdy członka nie używa argument typu T w dowolny sposób.

W takim przypadku alternatywą jest tworzenie podklasy specjalne:

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIVIewProperty {
        get { return MyProperty; }
        set { MyProperty = value; }
    }
}

class Generic<T> : NSObject where T : NSObject
{
    public static void MyMethod () {}
    public static T MyProperty { get; set; }
}
```

### <a name="requires-new-static-registrar"></a>Wymaga nowy statyczny rejestratora

Obsługa ogólne wymaga nowej [rejestracji](~/ios/internals/registrar.md).

Jeśli spróbujesz użyć starego systemu starszych rejestracji wyświetli ostrzeżenia po napotkaniu typów ogólnych (dodatkowo aby nie generować prawidłowego kodu, co powoduje niezdefiniowane zachowanie).
    
## <a name="performance"></a>Wydajność

Rejestrator statycznych nie można rozpoznać wyeksportowanego elementu członkowskiego typu ogólnego w czasie kompilacji, jak zwykle, musi być wyszukiwane w czasie wykonywania. Oznacza to, że wywoływanie metody z języka Objective-C ma przebiegać wolniej niż wywoływanie elementów członkowskich klas nierodzajową.

