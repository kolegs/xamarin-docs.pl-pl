---
title: Omówienie
description: Szczegóły dotyczące działania proces wiązania
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: d1a90934cf7a9a832172f32ed95cf3e254e04385
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="overview"></a>Omówienie

_Szczegóły dotyczące działania proces wiązania_

Powiązanie biblioteki języka Objective-C do użycia z programem Xamarin przyjmuje trzy kroki:

1. Zapis C# "Definicji interfejsu API" do opisywania, jak natywnego interfejsu API jest widoczna w .NET oraz sposobu mapowania do podstawowej Objective-C. Jest to realizowane przy użyciu standardowego języka C# konstrukcje, takich jak `interface` i powiązanie różnych **atrybuty** (zobacz ten [prosty przykład](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. Raz napisano "Definicja interfejsu API" w języku C#, skompiluj go do produkcji w zestawie "powiązanie". Można to zrobić na [ **wiersza polecenia** ](#commandline) lub przy użyciu [ **powiązania projektu** ](#bindingproject) w Visual Studio for Mac lub Visual Studio.

3. Tego zestawu "powiązanie" jest dodawane do projektu aplikacji platformy Xamarin, aby dostęp do funkcji natywnego przy użyciu interfejsu API zdefiniowany.
  Projekt powiązania jest całkowicie niezależna od projektów aplikacji.

**Uwaga:** krok 1 można zautomatyzować przy pomocy [ **Sharpie cel**](#objectivesharpie). Sprawdza, czy interfejs API języka Objective-C i generuje proponowanych C# "Definicji interfejsu API." Można dostosować plików utworzonych przez Sharpie cel i używać ich w projekcie powiązania (lub w wierszu polecenia) można utworzyć zestawu z powiązania. Celu Sharpie nie tworzy wiązania samodzielnie, jest zadaniem opcjonalnym składnikiem większego procesu.

Można także znaleźć więcej szczegółowych informacji technicznych o [jej działania](#howitworks), które zawierają informacje pomocne podczas zapisu powiązania.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Powiązania wiersza polecenia

Można użyć `btouch-native` dla platformy Xamarin.iOS (lub `bmac-native` Jeśli używasz Xamarin.Mac) do tworzenia powiązań bezpośrednio. Działa on przez przekazanie definicji interfejsu API języka C#, które zostały utworzone ręcznie (lub przy użyciu Sharpie cel) do narzędzia wiersza polecenia (`btouch-native` dla systemu iOS lub `bmac-native` dla komputerów Mac).


Ogólna składnia wywoływania tych narzędzi jest:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Powyższe polecenia spowoduje wygenerowanie pliku `cocos2d.dll` w bieżącym katalogu i będzie zawierać pełni powiązanej bibliotece, których można użyć w projekcie. Jest to narzędzie, które używa programu Visual Studio for Mac można tworzyć powiązania, jeśli projekt powiązania (opisane [poniżej](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>Powiązania projektu

Projekt powiązanie dla komputerów Mac lub Visual Studio (Visual Studio obsługuje tylko powiązania z systemem iOS) można utworzyć w programie Visual Studio oraz ułatwia do edycji i definicje interfejsu API dla powiązania (w przeciwieństwie przy użyciu wiersza polecenia) kompilacji.

Wykonaj to [Wprowadzenie — przewodnik](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) na temat sposobu tworzenia i używania powiązania projektu do tworzenia powiązania.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Sharpie celu

Celu Sharpie to narzędzie wiersza polecenia osobnego, pomaga w początkowej etapy tworzenia powiązania. Powiązanie nie jest tworzony przez samego siebie, a nie tworzy automatycznie początkowego kroku generowania definicji interfejsu API dla docelowej natywnej biblioteki.

Odczyt [Sharpie cel docs](~/cross-platform/macios/binding/objective-sharpie/index.md) informacje na temat przeanalizować natywnych bibliotek, natywnego struktur i programu CocoaPods do definicji interfejsu API, które mogą być wbudowane w powiązania.

<a name="howitworks" />

## <a name="how-binding-works"></a>Jak działa powiązania

Istnieje możliwość użycia [[zarejestrować]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) atrybutu, [[eksportowanie]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) atrybutu, i [ręcznego wywołania selektor języka Objective-C](~/ios/internals/objective-c-selectors.md) razem powiązać ręcznie nowych (wcześniej typy Objective-C niepowiązanego).

Po pierwsze Znajdź typ, który chcesz powiązać. Omówienie celów (i prostota), firma Microsoft będzie powiązać [NSEnumerator](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) typu (który już został powiązany w [Foundation.NSEnumerator](https://developer.xamarin.com/api/type/Foundation.NSEnumerator/); poniżej implementacji jest tak na przykład celów).

Po drugie należy utworzyć typ języka C#. Firma Microsoft będzie prawdopodobnie chcesz umieścić to w przestrzeni nazw; ponieważ Objective-C nie obsługuje przestrzeni nazw, musisz użyć `[Register]` atrybutu, aby zmienić nazwę typu, która Xamarin.iOS zarejestruje się w języku Objective C runtime. Typ języka C# również musi dziedziczyć z [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

Trzecie, zapoznaj się z dokumentacją Objective-C i Utwórz [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) wystąpień dla każdego selektora chcesz użyć. Umieść je w treści klasy:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

Czwarty danego typu będzie musiał podać konstruktorów. Możesz *musi* łańcucha z wywołania konstruktora do konstruktora klasy podstawowej. `[Export]` Kod języka Objective-C, aby wywoływać konstruktorów o nazwie określonej selektora zezwolenie na atrybuty:

```csharp
[Export("init")]
public NSEnumerator()
    : base(NSObjectFlag.Empty)
{
    Handle = Messaging.IntPtr_objc_msgSend(this.Handle, selInit.Handle);
}
```

```csharp
// This constructor must be present so that Xamarin.iOS
// can create instances of your type from Objective-C code.
public NSEnumerator(IntPtr handle)
    : base(handle)
{
}
```

Piąte zapewniają metody selektory zadeklarowany w kroku 3. Te użyje `objc_msgSend()` do wywołania selektor na obiekt natywny. Zwróć uwagę na użycie [Runtime.GetNSObject()](https://developer.xamarin.com/api/member/ObjCRuntime.Runtime.GetNSObject/(System.IntPtr)) przekonwertować `IntPtr` do odpowiedniego typu `NSObject` podkategorii typu. Jeśli chcesz, aby metodę można wywołać za pomocą kodu języka Objective-C, element członkowski *musi* można **wirtualnego**.

```csharp
[Export("nextObject")]
public virtual NSObject NextObject()
{
    return Runtime.GetNSObject(
        Messaging.IntPtr_objc_msgSend(this.Handle, selNextObject.Handle));
}
```

```csharp
// Note that for properties, [Export] goes on the get/set method:
public virtual NSArray AllObjects {
    [Export("allObjects")]
    get {
        return (NSArray) Runtime.GetNSObject(
            Messaging.IntPtr_objc_msgSend(this.Handle, selAllObjects.Handle));
    }
}
```

Wprowadzanie wszystkich elementów:

```csharp
using System;
using Foundation;
using ObjCRuntime;

namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        static Selector selInit       = new Selector("init");
        static Selector selAllObjects = new Selector("allObjects");
        static Selector selNextObject = new Selector("nextObject");

        [Export("init")]
        public NSEnumerator()
            : base(NSObjectFlag.Empty)
        {
            Handle = Messaging.IntPtr_objc_msgSend(this.Handle,
                selInit.Handle);
        }

        public NSEnumerator(IntPtr handle)
            : base(handle)
        {
        }

        [Export("nextObject")]
        public virtual NSObject NextObject()
        {
            return Runtime.GetNSObject(
                Messaging.IntPtr_objc_msgSend(this.Handle,
                    selNextObject.Handle));
        }

        // Note that for properties, [Export] goes on the get/set method:
        public virtual NSArray AllObjects {
            [Export("allObjects")]
            get {
                return (NSArray) Runtime.GetNSObject(
                    Messaging.IntPtr_objc_msgSend(this.Handle,
                        selAllObjects.Handle));
            }
        }
    }
}
```

