---
title: Omówienie powiązań języka Objective-C
description: Ten dokument zawiera omówienie różnych sposobów tworzenia powiązania C# dla kodu języka Objective-C, łącznie z wiersza polecenia powiązania, powiązanych projektów i Objective Sharpie. Omówiono również, jak działa powiązanie.
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: asb3993
ms.author: amburns
ms.openlocfilehash: 97d0c5b9f61d4dafe144d2b2f22df6d465cbbccb
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855276"
---
# <a name="overview-of-objective-c-bindings"></a>Omówienie powiązań języka Objective-C

_Szczegółowe informacje, jak działa proces wiązania_

Powiązanie do biblioteki języka Objective-C do użycia z platformą Xamarin przyjmuje trzy kroki:

1. Zapis C# "Definicja interfejsu API" opisujący, jak natywnych interfejsów API jest widoczna w .NET i jak jest on mapowany do bazowego Objective-C. Odbywa się przy użyciu standardowego języka C# konstrukcji, takich jak `interface` i powiązanie różnych **atrybuty** (zobacz ten [prosty przykład](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. Po napisaniu "Definicja interfejsu API" w języku C#, skompilujesz go do produkcji w zestawie "powiązania". Można to zrobić na [ **wiersza polecenia** ](#commandline) lub za pomocą [ **powiązania projektu** ](#bindingproject) w programie Visual Studio for Mac lub Visual Studio.

3. Tego zestawu "powiązania" jest dodawane do projektu aplikacji platformy Xamarin, aby mogli uzyskiwać dostęp macierzystą funkcjonalność za pomocą interfejsu API zdefiniowany.
  Projekt powiązania jest całkowicie niezależna od Twoich projektów aplikacji.

**Uwaga:** kroku 1, można zautomatyzować przy pomocy [ **Objective Sharpie**](#objectivesharpie). Sprawdza, czy interfejs API języka Objective-C i generuje proponowane C# "Definicja interfejsu API". Można dostosować plików utworzonych przez Objective Sharpie i używać ich w projekcie powiązania (lub w wierszu polecenia) do utworzenia zestawu powiązania. Narzędzie Objective Sharpie nie tworzy wiązania samodzielnie, jest zadaniem opcjonalnym składnikiem większego procesu.

Można również przeczytać więcej szczegółowych informacji technicznych o [jak to działa](#howitworks), które pomogą Ci do zapisania wiązania.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Powiązania wiersza polecenia

Możesz użyć `btouch-native` dla platformy Xamarin.iOS (lub `bmac-native` korzystania z platformy Xamarin.Mac) bezpośrednio Tworzenie powiązań. Polega ono na przekazywanie definicji interfejsu API języka C#, utworzone ręcznie (lub za pomocą Objective Sharpie) do narzędzia wiersza polecenia (`btouch-native` dla systemu iOS lub `bmac-native` dla komputerów Mac).


Ogólna składnia wywoływania tych narzędzi jest:

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Powyższe polecenie spowoduje wygenerowanie pliku `cocos2d.dll` w bieżącym katalogu i będzie zawierać pełni powiązanej bibliotekę, w której można użyć w projekcie. To narzędzie, które korzysta z programu Visual Studio dla komputerów Mac w celu utworzenia wiązania, jeśli używasz projektu powiązania (opisane [poniżej](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>Projekt powiązania

Projekt powiązania mogą być tworzone w programie Visual Studio dla komputerów Mac lub Visual Studio (Visual Studio obsługuje tylko powiązania z systemem iOS) i ułatwia to edytowanie i tworzenie definicji interfejsu API dla powiązania (a nie przy użyciu wiersza polecenia).

Postępuj zgodnie z tym [przewodnik wprowadzenie](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) na temat sposobu tworzenia i używania projektu powiązania do tworzenia powiązania.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Narzędzie Objective Sharpie

Narzędzie Objective Sharpie to narzędzie wiersza polecenia inną, oddzielną, które może ułatwić realizację początkowych etapów tworzenia powiązania. Nie tworzy powiązanie samodzielnie, zamiast automatyzuje etap początkowy generowania definicji interfejsu API dla natywnej biblioteki docelowej.

Odczyt [docs Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md) dowiesz się, jak można przeanalizować natywnych bibliotek i struktur natywnych, CocoaPods do definicje interfejsu API, które mogą być wbudowane w powiązaniach.

<a name="howitworks" />

## <a name="how-binding-works"></a>Jak działa powiązanie

Można użyć [[zarejestrować]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) atrybutu [[eksportu]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) atrybut, i [ręcznego wywołania selektor języka Objective-C](~/ios/internals/objective-c-selectors.md) ze sobą, aby samodzielnie ręcznie powiązali nowe (wcześniej niezwiązane) typy języka Objective-C.

Po pierwsze Znajdź typ, który chcesz powiązać. Dla dyskusji celów i prostotę, firma Microsoft będzie powiązać [NSEnumerator](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) typu (który jest już powiązane w [Foundation.NSEnumerator](https://developer.xamarin.com/api/type/Foundation.NSEnumerator/); Poniższa implementacja jest po prostu na przykład celów).

Po drugie musimy utworzyć typ C#. Firma Microsoft będzie prawdopodobnie zechcesz umieścić go w przestrzeni nazw; ponieważ języka Objective-C nie obsługuje przestrzenie nazw, należy użyć `[Register]` atrybutu, aby zmienić nazwę typu, który zarejestruje Xamarin.iOS ze środowiskiem uruchomieniowym języka Objective-C. Typ C# musi również dziedziczyć [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

Po trzecie, zapoznaj się z dokumentacją języka Objective-C i utworzyć [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) wystąpień dla każdego selektora chcesz użyć. Umieść je w treści klasy:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

Czwarty danego typu należy podać konstruktorów. Możesz *musi* połączyć w łańcuch z wywołania konstruktora do konstruktora klasy bazowej. `[Export]` Atrybuty zezwalać na kod języka Objective-C, aby wywoływać konstruktorów o nazwie określonej selektor:

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

Piąte zapewniają metody selektory zadeklarowane w kroku 3. Będą one używane `objc_msgSend()` do wywołania selektor na obiekt natywny. Zwróć uwagę na użycie [Runtime.GetNSObject()](https://developer.xamarin.com/api/member/ObjCRuntime.Runtime.GetNSObject/(System.IntPtr)) przekonwertować `IntPtr` do odpowiednio wpisane `NSObject` podkategorii typu. Jeśli chcesz, aby metodę można wywołać za pomocą kodu języka Objective-C, elementu członkowskiego *musi* można **wirtualnego**.

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

Łączenie wszystkiego razem:

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

## <a name="related-links"></a>Linki pokrewne

- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)