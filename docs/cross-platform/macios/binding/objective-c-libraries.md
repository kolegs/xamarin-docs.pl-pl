---
title: Tworzenie powiązań bibliotek języka Objective-C
description: Ten dokument zawiera ogólne omówienie sposobu tworzenia opisujących sposób powiązania zdarzeń, metody, niestandardowe formanty i kod języka Objective-C, powiązania C#.
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 4c414e0e863f44045473a248576a3612b1719559
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854834"
---
# <a name="binding-objective-c-libraries"></a>Tworzenie powiązań bibliotek języka Objective-C

Podczas pracy z platformy Xamarin.iOS i Xamarin.Mac, mogą wystąpić przypadki, w której chcesz używać innej biblioteki języka Objective-C. W takiej sytuacji można użyć projekty powiązań platformy Xamarin, można utworzyć powiązania C# w natywnych bibliotekach języka Objective-C. Projekt używa tych samych narzędzi, które są używane do dostosowania systemów iOS i Mac interfejsów API dla języka C#.

W tym dokumencie opisano, jak powiązać interfejsów API języka Objective-C, jeśli dokonywane jest wiązanie tylko interfejsy API języka C, w tym celu należy użyć standardowego mechanizmu .NET [framework P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/).
Szczegółowe informacje na temat będą statycznie łączyć biblioteki C są dostępne na [Konsolidacja bibliotek natywnych](~/ios/platform/native-interop.md) strony.

Zobacz nasze Pomocnika [powiązanie Podręcznik typy](~/cross-platform/macios/binding/binding-types-reference.md).
Ponadto, jeśli chcesz dowiedzieć się więcej na temat zachodzących kulisy, Sprawdź nasze [powiązanie Przegląd](~/cross-platform/macios/binding/overview.md) strony.

Mogą być wbudowane powiązania dla systemów iOS i Mac bibliotek.
Ta strona zawiera opis sposobu działa w systemie iOS powiązania, ale powiązania Mac są bardzo podobne.

**Przykładowy kod dla systemu iOS**

Możesz użyć [iOS przykładowe powiązania](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) projekt, aby eksperymentować z powiązaniami.

<a name="Getting_Started" />

## <a name="getting-started"></a>Wprowadzenie

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Najprostszym sposobem utworzenia powiązania jest utworzenie projektu powiązanie interfejsu Xamarin.iOS.
Można to zrobić w programie Visual Studio dla komputerów Mac, wybierając typ projektu **dla systemu iOS > biblioteki > Biblioteka powiązań**:

[![](objective-c-libraries-images/00-sml.png "W tym celu z programu Visual Studio dla komputerów Mac Wybieranie typu projektu biblioteki, biblioteka powiązań systemu iOS")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Najprostszym sposobem utworzenia powiązania jest utworzenie projektu powiązanie interfejsu Xamarin.iOS.
Można to zrobić w programie Visual Studio na Windows, wybierając typ projektu **Visual C# > iOS > Biblioteka powiązań (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "Biblioteka powiązań systemu iOS w system iOS")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> Uwaga: Powiązanie projekty dla **Xamarin.Mac** są obsługiwane tylko w programie Visual Studio dla komputerów Mac.

-----

Wygenerowany projekt zawiera małe szablon, który można edytować, zawiera dwa pliki: `ApiDefinition.cs` i `StructsAndEnums.cs`.

`ApiDefinition.cs` Jest, gdzie będą definiować kontrakt interfejsu API jest plik, który opisuje, jak podstawowego interfejsu API języka Objective-C jest odwzorowane w języku C#. Składnia i zawartość tego pliku są głównym temat dyskusję na temat tego dokumentu i jej zawartość są ograniczone do interfejsów języka C# i C# delegata deklaracji. `StructsAndEnums.cs` Plik jest plikiem, w którym wprowadza inne definicje, które są wymagane przez interfejsy i delegaty. Obejmuje to wartości wyliczenia i struktury, które mogą używać kodu.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>Powiązanie interfejsu API

Wykonywania kompleksowe powiązania, należy zrozumieć definicji interfejsu API języka Objective-C i zapoznać się z wytycznymi dotyczącymi .NET Framework projektu.

Aby powiązać biblioteki, które zazwyczaj rozpocznie się przy użyciu pliku definicji interfejsu API. Plik definicji interfejsu API jest tylko plik źródłowy C# zawierającą interfejsów języka C#, adnotacjami za pomocą kliku atrybutów, które ułatwiają wprowadzenie metodyki powiązanie.  Ten plik jest definiuje kontrakt między C# i języka Objective-C jest.

Na przykład to prosta pliku interfejsu api w bibliotece:

```csharp
using Foundation;

namespace Cocos2D {
  [BaseType (typeof (NSObject))]
  interface Camera {
    [Static, Export ("getZEye")]
    nfloat ZEye { get; }

    [Export ("restore")]
    void Restore ();

    [Export ("locate")]
    void Locate ();

    [Export ("setEyeX:eyeY:eyeZ:")]
    void SetEyeXYZ (nfloat x, nfloat y, nfloat z);

    [Export ("setMode:")]
    void SetMode (CameraMode mode);
  }
}
```

Powyższe Przykładowa aplikacja definiuje klasę o nazwie `Cocos2D.Camera` który pochodzi od klasy `NSObject` typ podstawowy (ten typ pochodzi z `Foundation.NSObject`) i definiujący właściwości statycznej (`ZEye`), dwie metody, które przyjmują żadnych argumentów i metody, które przyjmuje trzy argumenty.

Szczegółowe omówienie formatu pliku interfejsu API i atrybuty, które można użyć zostało omówione w [plik definicji interfejsu API](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) poniższej sekcji.

Aby uzyskać pełną powiązania, będzie zazwyczaj dotyczy cztery składniki:

-  Plik definicji interfejsu API (`ApiDefinition.cs` w szablonie).
-  Opcjonalnie: wszystkie typy wyliczeniowe, typy, struktury wymagane przez plik definicji interfejsu API (`StructsAndEnums.cs` w szablonie).
-  Opcjonalnie: dodatkowych źródeł, które mogą rozwiń wygenerowanego powiązania lub podać więcej przyjazna interfejsem API C# (C# pliki, które dodajesz do projektu).
-  Natywną bibliotekę dokonywane jest wiązanie.

Ten wykres przedstawia zależności między plikami:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "Ten wykres przedstawia zależności między plikami")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

Plik definicji interfejsu API będzie zawierać tylko definicji przestrzeni nazw i interfejs (przy użyciu żadnych składowych, które mogą zawierać interfejs) i nie powinna zawierać klas, wyliczeń, delegatów lub struktury. Plik definicji interfejsu API to jedynie Umowa, która będzie służyć do generowania interfejsu API.

Wprowadzania dodatkowego kodu, że chcesz muszą wyliczenia lub klasy pomocnicze powinna być hostowana w oddzielnym pliku, w przykładzie powyżej "CameraMode" jest wartością wyliczenia, która nie istnieje w pliku CS i powinna być hostowana w oddzielnym pliku, na przykład `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

`APIDefinition.cs` Plików jest połączony z `StructsAndEnum` klasy i są używane do generowania powiązania podstawowej biblioteki. Możesz użyć wynikowej biblioteki jako — jest, ale zazwyczaj można dostrajanie wynikowy biblioteki, aby dodać niektóre funkcje języka C# dla użytkowników. Przykłady: Implementowanie `ToString()` metody dostarczy indeksatory C#, Dodaj niejawne konwersje do i z niektórych typach natywnych lub silnie typizowane wersje niektórych metod. Te ulepszenia są przechowywane w dodatkowych plików języka C#. Tylko dodać pliki C# do projektu i zostaną uwzględnione w ramach tego procesu kompilacji.

Spowoduje to pokazanie, jak możesz również zaimplementować kod w swojej `Extra.cs` pliku. Należy zauważyć, że będziesz używać klas częściowych jak rozszerzyć tych klas częściowych, które są generowane na podstawie kombinacji `ApiDefinition.cs` i `StructsAndEnums.cs` podstawowe powiązania:

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

Tworzenie biblioteki dadzą natywnych wiązania.

Aby wykonać to powiązanie, należy dodać bibliotekę natywną do projektu.  Można to zrobić, dodając natywnej biblioteki do projektu, przeciągając i upuszczając bibliotekę natywną z Findera do projektu w Eksploratorze rozwiązań lub kliknij prawym przyciskiem myszy projekt i wybierając polecenie **Dodaj**  >  **Dodaj pliki** do wybierz bibliotekę natywną.
Bibliotek natywnych, zgodnie z Konwencją, Rozpocznij od słowa "lib" i kończy się rozszerzeniem ".a". Gdy to zrobisz, program Visual Studio for Mac doda dwa pliki: plik .a i automatycznie wypełnione C# plik zawierający informacje o zawiera bibliotekę natywną:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "Bibliotek natywnych, zgodnie z Konwencją rozpoczynać się lib programu word i kończyć się znakiem .a rozszerzenia")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

Zawartość `libMagicChord.linkwith.cs` plik zawiera informacje o używaniu tej biblioteki i powoduje, że środowiska IDE, aby utworzyć pakiet te dane binarne do wynikowego pliku DLL:

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

Szczegółowe informacje o sposobie używania pełna [ `[LinkWith]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute) atrybutu są udokumentowane w artykule [powiązanie Podręcznik typy](~/cross-platform/macios/binding/binding-types-reference.md).

Teraz podczas kompilowania projektu będzie na końcu `MagicChords.dll` pliku, który zawiera bibliotekę natywną i powiązania. Można rozpowszechniać tego projektu, lub użyj wynikowy DLL dla innych deweloperów dla ich własnych.

Czasami może się okazać konieczne kilku wartości wyliczenia, definicje delegata lub innych typów. Należy umieszczać w plik definicji interfejsu API, jak jest to jedynie kontraktu

<a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>Plik definicji interfejsu API

Plik definicji interfejsu API składa się z wielu interfejsach. Interfejsy w definicji interfejsu API zostanie włączony w deklaracji klasy, a musi być dekorowane za pomocą [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) atrybutu, aby określić klasę bazową dla klasy.

Możesz może zastanawiać, dlaczego firma Microsoft nie użyto klas zamiast interfejsów dla definicję kontraktu. Firma Microsoft pobrane interfejsów, ponieważ mogliśmy zapisać kontraktu dla metody, bez konieczności dostarczania treści metody w pliku definicji interfejsu API lub konieczności dostarczania treści, które musiały zgłoszą wyjątek lub zwraca zrozumiałą wartość.

Jednak ponieważ używamy interfejsu jako szkielet do generowania klasy mieliśmy odwołać się do dekoracji różne części kontraktu, za pomocą atrybutów powiązania dysku.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>Metody wiązania

Najprostsza powiązanie, które można zrobić jest tworzenia powiązania metody. Po prostu deklaruje metodę w interfejsie konwencji nazewnictwa C# i dekoracji metody [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atrybutu. [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Atrybut jest tego, jakie łącza swoją nazwę języka C# o nazwie języka Objective-C w środowisku uruchomieniowym platformy Xamarin.iOS. Wartość parametru [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atrybut jest nazwą selektor języka Objective-C. Kilka przykładów:

```csharp
// A method, that takes no arguments
[Export ("refresh")]
void Refresh ();

// A method that takes two arguments and return the result
[Export ("add:and:")]
nint Add (nint a, nint b);

// A method that takes a string
[Export ("draw:atColumn:andRow:")]
void Draw (string text, nint column, nint row);
```

Powyższe przykłady pokazują, jak można powiązać metody wystąpienia. Aby powiązać metody statyczne, należy użyć [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) atrybut następująco:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

Jest to wymagane, ponieważ kontrakt jest częścią interfejsu i interfejsy mają nie pojęcie deklaracji wystąpienia statycznej, więc ponownie odwołać się do atrybutów. Jeśli chcesz ukryć konkretną metodę z wiązania można dekoracji metody [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atrybutu.

`btouch-native` Polecenia wprowadzi sprawdza, czy parametry odwołania nie mieć wartości null. Jeśli chcesz zezwolić na wartości null dla określonego parametru, użyj [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) atrybutu w parametrze następująco:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

Podczas eksportowania typu odwołania z [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) słów kluczowych, można również określić semantyki alokacji. Jest to konieczne upewnić się, że żadne dane nie następuje przeciek.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>Wiązanie właściwości

Podobnie jak metod, właściwości języka Objective-C są powiązane za pomocą [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atrybut i mapy bezpośrednio do właściwości języka C#. Podobnie jak metod, właściwości mogą być dekorowane za pomocą [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) i [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atrybutów.

Kiedy używasz [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atrybutu dla właściwości, w obszarze natywnych btouch obejmuje faktycznie wiąże dwóch metod: getter i setter. Nazwa która należy podać, aby wyeksportować **basename** i metody ustawiającej jest obliczana przez poprzedzenie jej słowo "set" Włączanie pierwszą literę **basename** na wielkie litery oraz wprowadzania selektor zająć argument. Oznacza to, że `[Export ("label")]` stosowane na właściwość faktycznie wiąże "label" i "setLabel:" metod języka Objective-C.

Czasami właściwości języka Objective-C nie oparte na wzorcu opisanych powyżej i nazwa jest ręcznie zastąpione. W takich przypadkach można kontrolować sposób, że powiązanie jest generowany przy użyciu [ `[Bind]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAttribute) atrybutu na pobierającą czy ustawiającą, na przykład:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

Następnie tworzy powiązania "isMenuVisible" i "setMenuVisible:". Opcjonalnie właściwości mogą być powiązane przy użyciu następującej składni:

```csharp
[Category, BaseType(typeof(UIView))]
interface UIView_MyIn
{
  [Export ("name")]
  string Name();

  [Export("setName:")]
  void SetName(string name);
}
```

Gdzie metodę getter i setter jawnie są zdefiniowane jako w `name` i `setName` powiązania powyżej.

Oprócz obsługi statycznej właściwości, za pomocą [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute), można dekoracji właściwości statycznej wątku za pomocą [ `[IsThreadStatic]` ](~/cross-platform/macios/binding/binding-types-reference.md#IsThreadStaticAttribute), na przykład:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

Tak samo, jak metody umożliwiają niektóre parametry do [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute), można zastosować [ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) do właściwości, aby wskazać, że wartość null jest prawidłową wartość dla właściwości, na przykład:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

[ `[NullAllowed]` ](~/cross-platform/macios/binding/binding-types-reference.md#NullAllowedAttribute) Bezpośrednio na metody ustawiającej można również określić parametr:

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>Zastrzeżenia powiązania Kontrolki niestandardowe

Podczas konfigurowania wiązania dla kontrolki niestandardowej, należy rozważyć następujące zastrzeżenia:

1. **Wiązanie właściwości muszą być statyczne** — podczas definiowania powiązania właściwości [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute) należy użyć atrybutu.
 2. **Nazwy właściwości musi dokładnie pasować** — nazwa używana do powiązania właściwość musi dokładnie pasować nazwę właściwości kontrolki niestandardowej.
3. **Typy właściwości muszą być zgodne** — typ zmiennej, używana do powiązania właściwość musi być zgodny z typem właściwości w formancie niestandardowym.
4. **Punkty przerwania i metody pobierającej/ustawiającej** — punkty przerwania są umieszczane w metodzie pobierającej lub metod ustawiających właściwości nigdy nie zostanie osiągnięty.
5. **Obserwuj wywołania zwrotne** — konieczne będzie otrzymywać powiadomienia o zmianach w wartości właściwości kontrolek niestandardowych za pomocą wywołania zwrotne obserwacji.

Nieprzestrzeganie dowolnego z powyższych zastrzeżenia wymienionych może spowodować powiązań, w trybie dyskretnym niepowodzeniem w czasie wykonywania.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Wzorzec mutable języka Objective-C i właściwości

Struktury języka Objective-C za pomocą idiom gdzie niektóre klasy są niezmienne z podklasy modyfikowalna. Na przykład `NSString` jest niezmienialny wersji, podczas gdy `NSMutableString` podklasą klasy, która umożliwia mutacji.

W tych klas jest częściej można zobaczyć niezmienialne klasy bazowej, zawierają właściwości, za pomocą metody pobierającej, ale nie metody ustawiającej. I mutable wersji wprowadzenie metody ustawiającej. Ponieważ nie jest to naprawdę możliwe w języku C#, mieliśmy do mapowania tego idiomu na idiom, która będzie działać w języku C#.

Metodą, to jest mapowany do języka C# jest dodawanie zarówno getter i setter dla klasy bazowej, ale Oflagowanie metody ustawiającej z [ `[NotImplemented]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotImplementedAttribute) atrybutu.

Następnie na podklasy modyfikowalny, należy użyć [ `[Override]` ](~/cross-platform/macios/binding/binding-types-reference.md#OverrideAttribute) atrybutu dla właściwości, aby upewnić się, czy właściwość jest faktycznie zastępowanie zachowania obiektu nadrzędnego.

Przykład:

```csharp
[BaseType (typeof (NSObject))]
interface MyTree {
    string Name { get; [NotImplemented] set; }
}

[BaseType (typeof (MyTree))]
interface MyMutableTree {
    [Override]
    string Name { get; set; }
}
```

<a name="Binding_Constructors" />

### <a name="binding-constructors"></a>Konstruktory powiązania

`btouch-native` Narzędzie automatycznie wygeneruje fours konstruktorów w klasie dla danej klasy `Foo`, generuje ona:

-  `Foo ()`: konstruktora domyślnego (maps do konstruktora "init" Objective-C)
-  `Foo (NSCoder)`: Konstruktor używany podczas deserializacji pliku NIB (mapuje Objective-C "initWithCoder:" konstruktora).
-  `Foo (IntPtr handle)`: konstruktor do tworzenia opartych na uchwyt, wywoływane przez środowisko uruchomieniowe po środowisko wykonawcze musi uwidaczniać obiektu zarządzanego z niezarządzanej obiektu.
-  `Foo (NSEmptyFlag)`: jest on używany przez klasy pochodne Aby uniknąć podwójnego inicjowania.

Dla konstruktorów zdefiniowanych przez użytkownika muszą zostać zadeklarowane za pomocą następujący podpis wewnątrz definicji interfejsu: one musi zwracać `IntPtr` wartość i nazwa metody powinna być konstruktora. Na przykład aby powiązać `initWithFrame:` konstruktora, jest to, czy użyć:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

<a name="Binding_Protocols" />

### <a name="binding-protocols"></a>Protokoły powiązania

Zgodnie z opisem w dokumencie projekt interfejsu API w sekcji [Omawiając modeli i protokoły](~/ios/internals/api-design/index.md#Models), Xamarin.iOS mapuje protokołów języka Objective-C do klas, które zostały oznaczone za pomocą [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) atrybut. Służy to zazwyczaj podczas implementowania klas delegata języka Objective-C.

Istotną różnicą między zwykłej klasy powiązanej i klasy delegata jest, czy klasa obiektu delegowanego może mieć jedną lub więcej metod opcjonalne.

Na przykład rozważmy `UIKit` klasy `UIAccelerometerDelegate`, jest to, jak jest powiązana w rozszerzeniu Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

Ponieważ jest to metoda opcjonalny w definicji dla `UIAccelerometerDelegate` nie ma nic innego celu. Ale jeśli było wymaganej metody przy użyciu protokołu, należy dodać [ `[Abstract]` ](~/cross-platform/macios/binding/binding-types-reference.md#AbstractAttribute) atrybutu do metody. Spowoduje to wymuszenie od użytkownika wprowadzenia w życie faktycznie treści metody.

Ogólnie rzecz biorąc protokoły są używane w klasach, które odpowiada na komunikaty. Zazwyczaj jest to wykonywane w języku Objective-C, przypisując do właściwości "delegata" wystąpienie obiektu, który odpowiada metod w protokole.

Konwencja w rozszerzeniu Xamarin.iOS jest zarówno języka Objective-C luźno powiązane styl każdy wystąpienie `NSObject` można przypisać do obiektu delegowanego i udostępniają również silnie typizowaną wersję. Z tego powodu firma Microsoft zwykle zapewnia zarówno `Delegate` właściwość, która jest silnie typizowane i `WeakDelegate` luźno z typem. Firma Microsoft zwykle powiązać typowaniem luźnym wersja, która [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute), i użyjemy [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) atrybutu, aby zapewnić silnie typizowaną wersję.

Spowoduje to pokazanie, jak firma Microsoft powiązane `UIAccelerometer` klasy:

```csharp
[BaseType (typeof (NSObject))]
interface UIAccelerometer {
        [Static] [Export ("sharedAccelerometer")]
        UIAccelerometer SharedAccelerometer { get; }

        [Export ("updateInterval")]
        double UpdateInterval { get; set; }

        [Wrap ("WeakDelegate")]
        UIAccelerometerDelegate Delegate { get; set; }

        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }
}
```

<a name="iOS7ProtocolSupport" />

**Nowość w wersji 7.0 MonoTouch**

Począwszy od MonoTouch 7.0 nowych i ulepszonych Protokół powiązania funkcji zostały włączone.  Ta obsługa nowych sprawia, że łatwiejszy w obsłudze języka Objective-C idiomy wdrażanie jednego lub więcej protokołów w danej klasy.

Dla każdej definicji protokołu `MyProtocol` w języku Objective-C, jest teraz `IMyProtocol` interfejs, który zawiera listę wymaganych metod z protokołem, a także klasę rozszerzenia, która udostępnia wszystkie metody opcjonalne.  Powyżej, w połączeniu z nowego pomocy technicznej w programie Xamarin Studio, Edytor umożliwia deweloperom implementują metody protokołu bez konieczności używania oddzielnych podklasy klasy abstrakcyjnej modelu poprzedniego.

Dowolna definicja, która zawiera [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) atrybutu powoduje wygenerowanie trzy klasy pomocnicze, które znacząco poprawić sposób, że użytkownik korzysta z protokołów:

```csharp
    // Full method implementation, contains all methods
    class MyProtocol : IMyProtocol {
        public void Say (string msg);
        public void Listen (string msg);
    }

    // Interface that contains only the required methods
    interface IMyProtocol: INativeObject, IDisposable {
        [Export ("say:")]
        void Say (string msg);
    }

    // Extension methods
    static class IMyProtocol_Extensions {
        public static void Optional (this IMyProtocol this, string msg);
        }
    }
```

**Implementacja klasy** zapewnia pełną klasa abstrakcyjna, można zastąpić poszczególnych metod i Uzyskaj pełną bezpieczeństwa.  Jednak ze względu na C# nie obsługuje wielokrotne dziedziczenie, istnieją scenariusze, w którym może musi mieć różne klasy bazowej, ale nadal chcesz zaimplementować interfejs, który jest

Wygenerowany **definicji interfejsu** pochodzą.  Jest to interfejs, który ma wszystkie wymagane metody z protokołem.  Umożliwia to deweloperom, które implementują Protokół usługi jedynie zaimplementowanie interfejsu.  Środowisko uruchomieniowe rejestruje automatycznie typ jako przyjęcie protokołu.

Zwróć uwagę, że interfejs tylko Wyświetla listę wymaganych metod i udostępniać metod opcjonalne.  Oznacza to, otrzyma pełny typ sprawdzania pod kątem wymaganych metod klas, które przyjęły protokołu, ale będzie musiał poważniejsze słabe wpisując (ręcznie przy użyciu [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atrybuty i podpis dopasowania) dla opcjonalnego metody protokołu.

Ułatwia ona korzystanie z interfejsu API, który korzysta z protokołów, narzędzie powiązania również generuje klasy metody rozszerzenia, która udostępnia wszystkie metody opcjonalne.  Oznacza to, że tak długo, jak korzystanie z interfejsu API, można traktować protokołów jako posiadające wszystkich metod.

Jeśli chcesz używać definicje protokołu w interfejsie API, należy napisać szkielet pustych interfejsów w swojej definicji interfejsu API.  Jeśli chcesz użyć Mój_protokół w interfejsie API, należy to zrobić:

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}

interface IMyProtocol {}

[BaseType (typeof(NSObject))]
interface MyTool {
    [Export ("getProtocol")]
    IMyProtocol GetProtocol ();
}
```

Powyżej jest niezbędne, ponieważ w czasie wiązania `IMyProtocol` czy nie istnieje, to znaczy Dlaczego należy podać pusty interfejs.

#### <a name="adopting-protocol-generated-interfaces"></a>Przyjęcie generowanych przez protokół interfejsów

Zawsze, gdy można implementować jeden z interfejsów, generowany dla protokołów, w następujący sposób:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Implementację metody interfejsu automatycznie pobiera wyeksportowane o nazwie poprawne, więc jest równoważny następującemu wyrażeniu:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Nie ma znaczenia, jeśli interfejs jest implementowany jawnie lub niejawnie.

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>Powiązanie rozszerzenia klas

W języku Objective C jest możliwe rozszerzenie klasy za pomocą nowych metod podobne duchu metody rozszerzenia języka C# w. Jeśli występuje jeden z tych metod, można użyć [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) atrybutu do metody jako odbiorca komunikatu języka Objective-C.

Na przykład w rozszerzeniu Xamarin.iOS, firma Microsoft powiązane metody rozszerzenia, które są zdefiniowane na `NSString` podczas `UIKit` są importowane jako metody `NSStringDrawingExtensions`, podobnie do następującego:

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

<a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>Powiązanie z listami argumentów języka Objective-C

Języka Objective-C obsługuje ze zmienną liczbą argumentów. Na przykład:

```objc
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

Do wywołania tej metody za pomocą języka C# należy utworzyć podpis następująco:

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

To deklaruje metodę jako wewnętrzne, ukrywanie powyższe wywołanie interfejsu API od użytkowników, ale uwidaczniania go do biblioteki. Następnie można napisać metodę następująco:

```csharp
public void AppendWorkers(params Worker[] workers)
{
    if (workers == null)
         throw new ArgumentNullException ("workers");

    var pNativeArr = Marshal.AllocHGlobal(workers.Length * IntPtr.Size);
    for (int i = 1; i < workers.Length; ++i)
        Marshal.WriteIntPtr (pNativeArr, (i - 1) * IntPtr.Size, workers[i].Handle);

    // Null termination
    Marshal.WriteIntPtr (pNativeArr, (workers.Length - 1) * IntPtr.Size, IntPtr.Zero);

    // the signature for this method has gone from (IntPtr, IntPtr) to (Worker, IntPtr)
    WorkerManager.AppendWorkers(workers[0], pNativeArr);
    Marshal.FreeHGlobal(pNativeArr);
}
```

<a name="Binding_Fields" />

### <a name="binding-fields"></a>Powiązywanie pól

Czasami ma dostęp do pola publiczne, które zostały zadeklarowane w bibliotece.

Zazwyczaj te pola zawierają wartości ciągów lub liczb całkowitych, które muszą być przywoływane. Są często używane jako ciąg, który reprezentuje określonego powiadomień, a klucze w słowniku.

Aby powiązać pola, Dodaj właściwość do pliku definicji interfejsu i dekoracji właściwość o [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) atrybutu. Ten atrybut przyjmuje jeden parametr: nazwa C symbolu do wyszukiwania. Na przykład:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

Aby zawijać różnych pól w klasie statycznej, która nie pochodzi od `NSObject`, możesz użyć [ `[Static]` ](~/cross-platform/macios/binding/binding-types-reference.md#StaticAttribute_Class) atrybut w klasie, takich jak to:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

Powyższe wygeneruje `LonelyClass` nie jest pochodny od `NSObject` i będzie zawierać powiązanie z `NSSomeEventNotification` 
 `NSString` widoczne jako `NSString`.

[ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) Atrybut można stosować do następujących typów danych:

-  `NSString` odwołania (tylko właściwości tylko do odczytu)
-  `NSArray` odwołania (tylko właściwości tylko do odczytu)
-  32-bitowe liczby całkowite (`System.Int32`)
-  64-bitowe liczby całkowite (`System.Int64`)
-  32-bitowe wartości zmiennoprzecinkowe (`System.Single`)
-  64-bitowych wartości zmiennoprzecinkowe (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

Oprócz nazwy natywnego pola można określić nazwy biblioteki, w którym znajduje się pole, przekazując nazwę biblioteki:

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

W przypadku łączenia statycznie nie ma żadnej biblioteki do powiązania, więc należy użyć `__Internal` nazwy:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>Typy wyliczeniowe powiązania

Możesz dodać `enum` bezpośrednio na Twoje powiązanie plików sprawia, że łatwiej z nich korzystać w definicji interfejsu API — bez korzystania z innego źródła pliku (musi zostać skompilowany w końcowym projektu i powiązania).

Przykład:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

Istnieje również możliwość utworzenia własnych typów wyliczeniowych, aby zastąpić `NSString` stałe. W tym przypadku będzie generator **automatycznie** utworzyć metody, które można przekonwertować wartości wyliczenia i stałe NSString dla Ciebie.

Przykład:

```csharp
enum NSRunLoopMode {

    [DefaultEnumValue]
    [Field ("NSDefaultRunLoopMode")]
    Default,

    [Field ("NSRunLoopCommonModes")]
    Common,

    [Field (null)]
    Other = 1000
}

interface MyType {
    [Export ("performForMode:")]
    void Perform (NSString mode);

    [Wrap ("Perform (mode.GetConstant ())")]
    void Perform (NSRunLoopMode mode);
}
```

W powyższym przykładzie można podjąć decyzję o dekoracji `void Perform (NSString mode);` z [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atrybutu. Spowoduje **Ukryj** interfejs API oparty na stałej, od klientów powiązania.

Jednak ograniczenie Tworzenie podklasy typ jako wrażeń interfejs API korzysta alternatywnych [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) atrybutu. Te wygenerowane metody nie są `virtual`, czyli możesz nie być w stanie w celu pominięcia go — które mogą lub nie, jest dobrym wyborem.

Alternatywą jest oryginalne, Oznacz `NSString`— na podstawie definicji jako `[Protected]`. Dzięki temu będzie można utworzyć podklas do pracy, gdy jest to wymagane, a wersji wrap'ed będzie nadal działać i wywołać metodę przesłonięta.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>Powiązanie `NSValue`, `NSNumber`, i `NSString` typowi lepiej

[ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) Atrybut umożliwia tworzenie powiązań `NSNumber`, `NSValue` i `NSString`(wyliczenia) na bardziej precyzyjne typy języka C#. Ten atrybut może służyć do tworzenia, lepsze, bardziej precyzyjne interfejsu API platformy .NET za pośrednictwem natywnych interfejsów API.

Można dekorowania metod (na zwracaną wartość), parametry i właściwości z [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute). Jedynym ograniczeniem jest to, że Twoje elementu członkowskiego **nie** znajdować się wewnątrz [ `[Protocol]` ](~/cross-platform/macios/binding/binding-types-reference.md#ProtocolAttribute) lub [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) interfejsu.

Na przykład:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

Czy dane wyjściowe:

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

Wewnętrznie zrobimy `bool?`  <->  `NSNumber` i `CGRect`  <->  `NSValue` konwersji.

[`[BindAs]`](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) obsługuje również tablic `NSNumber` `NSValue` i `NSString`(wyliczenia).

Na przykład:

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

Czy dane wyjściowe:

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll` jest `NSString` kopii typu wyliczeniowego, firma Microsoft powoduje pobranie po prawej stronie `NSString` wartości i obsługi konwersji typu.

Zobacz [ `[BindAs]` ](~/cross-platform/macios/binding/binding-types-reference.md#BindAsAttribute) dokumentacji, aby sprawdzić obsługiwane konwersji typów.

<a name="Binding_Notifications" />

### <a name="binding-notifications"></a>Powiązanie powiadomienia

Powiadomienia są komunikaty, które są wysyłane do `NSNotificationCenter.DefaultCenter` i są używane jako mechanizm wysyłać komunikaty z jednej części aplikacji do innej. Deweloperzy subskrybują powiadomienia zwykle przy użyciu [NSNotificationCenter](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/)firmy [AddObserver](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/M/AddObserver/) metody. Gdy aplikacja wysyła komunikat do Centrum powiadomień, zwykle zawiera przechowywana w ładunku [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) słownika. Tego słownika jest słabo wpisane i uzyskiwanie informacji z niej jest podatne, błąd, ponieważ użytkownicy zazwyczaj należy przeczytać dokumentację klucze, które są dostępne w słowniku i typy wartości, które mogą być przechowywane w słowniku. Obecność czasami kluczy jest używany jako wartość logiczną, a także.

Generator powiązanie interfejsu Xamarin.iOS zapewnia obsługę dla deweloperów powiązać powiadomienia. Aby to zrobić, należy ustawić [ `[Notification]` ](~/cross-platform/macios/binding/binding-types-reference.md#NotificationAttribute) atrybutu dla właściwości, która również zostały oznaczone [ `[Field]` ](~/cross-platform/macios/binding/binding-types-reference.md#FieldAttribute) właściwości (może być publicznym lub prywatnym).

Ten atrybut może być używana bez argumentów dla powiadomień, które zawierają ładunek lub można określić `System.Type` , odwołuje się do innego interfejsu w definicji interfejsu API, zwykle o nazwie kończą się ciągiem "EventArgs". Generator spowoduje wyłączenie interfejsu na klasę tej podklasy `EventArgs` i będzie zawierać wszystkie właściwości tam wymienione. [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) Atrybut powinien być używany w klasie, EventArgs, aby wyświetlić nazwę klucza, używany do wyszukiwania słownik języka Objective-C, które można pobrać wartości.

Na przykład:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Powyższy kod generuje klasę zagnieżdżoną `MyClass.Notifications` przy użyciu następujących metod:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Użytkownicy Twój kod może łatwo subskrybować opublikowane w usłudze powiadomień [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) przy użyciu kodu w następujący sposób:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Wartością zwróconą przez `ObserveDidStart` można łatwo przestać otrzymywać powiadomienia, takie jak to:

```csharp
token.Dispose ();
```

Można też wywołać [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject/) i przekazać token. Jeśli powiadomienie zawiera parametry, należy określić obiekt pomocnika `EventArgs` interfejsu następująco:

```csharp
interface MyClass {
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}

// The helper EventArgs declaration
interface MyScreenChangedEventArgs {
    [Export ("ScreenXKey")]
    nint ScreenX { get; set; }

    [Export ("ScreenYKey")]
    nint ScreenY { get; set; }

    [Export ("DidGoOffKey")]
    [ProbePresence]
    bool DidGoOff { get; }
}
```

Powyższe wygeneruje `MyScreenChangedEventArgs` klasy `ScreenX` i `ScreenY` właściwości, które powoduje pobranie danych z [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) słownika przy użyciu kluczy "ScreenXKey" i "ScreenYKey" odpowiednio i zastosować odpowiednie konwersje. `[ProbePresence]` Atrybut jest używany dla generatora sondowania, jeśli klucz jest ustawiany `UserInfo`, zamiast próbować wyodrębnianie wartości. To jest używana w przypadkach, gdzie obecności klawisz jest wartością (zwykle w przypadku wartości logicznych).

Umożliwia pisanie kodu w następujący sposób:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

<a name="Binding_Categories" />

### <a name="binding-categories"></a>Kategorie powiązania

Kategorie są mechanizm języka Objective-C, używanej do rozszerzania zestaw metod i właściwości dostępne w klasie.   W praktyce, są one używane do albo rozszerzyć funkcjonalność klasy bazowej (na przykład `NSObject`) po określonym środowiskiem konsolidowana (na przykład `UIKit`), dzięki czemu ich metod dostępna, ale tylko wtedy, gdy nowa struktura jest połączony w.   W niektórych przypadkach są one używane do organizowania funkcji w klasie według funkcji.   Są one podobne duchu metody rozszerzenia języka C#. Jest to, jak kategoria będzie wyglądać w cel — C:

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Powyższy przykład jeśli znalezione na bibliotekę będzie rozszerzać wystąpienia `UIView` przy użyciu metody `makeBackgroundRed`.

Aby powiązać te, można użyć [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) atrybutu w definicji interfejsu.  Korzystając z [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) atrybutu znaczenie [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) atrybut zmieni się w celu określenia klasy bazowej, aby rozszerzyć typ do rozszerzenia.

Poniższej przedstawiono sposób, w jaki `UIView` rozszerzenia są powiązane i przekształcane w metodach rozszerzeń C#:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Powyższe utworzy `MyUIViewExtension` klasę, która zawiera `MakeBackgroundRed` — metoda rozszerzenia.  Oznacza to, że teraz można wywołać "MakeBackgroundRed" na dowolnym `UIView` podklasy, co zapewnia taką samą funkcjonalność jak Objective-C. W niektórych przypadkach kategorie są używane, nie można rozszerzyć klasę systemu, ale do organizowania funkcji wyłącznie w celach dekoracji.  Jak to:

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

Chociaż można używać [ `[Category]` ](~/cross-platform/macios/binding/binding-types-reference.md#CategoryAttribute) atrybut również tego stylu dekoracji deklaracji, może także wystarczy dodać ich wszystkich do definicji klasy.  Oba te może osiągnąć takie same:

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Twitter {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Facebook {
    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

Jest to po prostu krótszy w takich przypadkach można scalić kategorie:

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);

    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

<a name="Binding_Blocks" />

### <a name="binding-blocks"></a>Bloki powiązania

Bloki są nową konstrukcję wprowadzone przez firmę Apple, aby przenieść funkcjonalności odpowiednikiem metod anonimowych C# do Objective-C. Na przykład `NSSet` klasa udostępnia teraz tej metody:

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

Opis powyżej deklaruje metodę o nazwie `enumerateObjectsUsingBlock:` przyjmującą jeden argument o nazwie `block`. Ten blok jest podobna do metody anonimowe C#, w tym obsługę do przechwytywania bieżącego środowiska ("to" wskaźnik, dostęp do zmiennych lokalnych i parametrów). W powyższej metody `NSSet` wywołuje blok z dwoma parametrami `NSObject` ( `id obj` części) i wskaźnik na wartość logiczną ( `BOOL *stop`) części.

Aby powiązać tego rodzaju interfejsu API za pomocą btouch, musisz pierwszym zdeklarowaniu blok podpisu typu C# delegowanie, a następnie Odwołaj się do niego z interfejsu API punktu wejścia, takich jak to:

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

A teraz kodzie można wywołać funkcji za pomocą języka C#:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

Można również używać wyrażeń lambda, jeśli użytkownik sobie tego życzy:

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

<a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>Metody asynchroniczne

Generator powiązań można włączyć klasy metody do metody async przyjaznego (metody, które zwracają Task lub Task&lt;T&gt;).

Możesz użyć [ `[Async]` ](~/cross-platform/macios/binding/binding-types-reference.md#AsyncAttribute) atrybutu metody, które zwracają void i którego ostatni argument jest wywołanie zwrotne.  Po zastosowaniu tego do metody generator powiązań wygeneruje wersję tej metody z sufiksem `Async`.  Jeśli wywołanie zwrotne nie przyjmuje żadnych parametrów, zwracaną wartością będzie `Task`, jeśli wywołania zwrotnego przyjmuje parametr, wynik będzie `Task<T>`.  Jeśli wywołanie zwrotne przyjmuje wiele parametrów, należy ustawić `ResultType` lub `ResultTypeName` określić odpowiednią nazwę wygenerowany typ, w której będą przechowywane wszystkie właściwości.

Przykład:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

Powyższy kod wygeneruje zarówno funkcję LoadFile metody, a także:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Dzięki czemu są ujawniane Silne typy parametrów NSDictionary słabe

W wielu miejscach w interfejsie API języka Objective-C, parametry są przekazywane jako ze słabą kontrolą typów `NSDictionary` interfejsów API za pomocą określonych kluczy i wartości, ale te są podatne (możesz przekazać nieprawidłowe klucze i uzyskać bez ostrzeżeń; możesz przekazać nieprawidłowe wartości i uzyskać nie ostrzeżenia) i frustrujące Aby użyć jako wymagają wielu rund do dokumentacji, aby wyszukać możliwe nazwy klucza i wartości.

To rozwiązanie jest silnie typizowaną wersję udostępniający jednoznacznym wersji interfejsu API i w tle mapy różnych podstawowych klucze i wartości.

Tak na przykład, jeśli interfejs API języka Objective-C zaakceptowane `NSDictionary` i jest udokumentowany jako biorąc klucz `XyzVolumeKey` który bierze `NSNumber` wartością woluminu od 0,0 do 1,0 i `XyzCaptionKey` przyjmującej ciągu, określ, czy Twoi użytkownicy musieli nieuprzywilejowany interfejsu API który wygląda następująco:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

`Volume` Właściwość jest zdefiniowana jako dopuszczającego wartość null float, zgodnie z Konwencją w języku Objective-C nie wymaga słowników jak wartości, więc istnieją scenariusze, w którym wartość nie może być ustawione.

Aby to zrobić, należy wykonać kilka czynności:

* Utwórz silnie typizowaną klasę tej podklasy [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) i zapewnia różnych metod pobierających i ustawiających dla każdej właściwości.
* Zadeklaruj przeciążenia dla metod, biorąc `NSDictionary` podjęcie nowej wersji silnie typizowane.

Można utworzyć silnie typizowanej klasy albo ręcznie lub użyć generator, które wykonają tę pracę za Ciebie.  Najpierw omówimy, jak to zrobić ręcznie, aby zrozumieć, co się dzieje, a następnie automatyczne podejście.

Należy utworzyć plik obsługi dla tego, przechodzi do Twojej umowy interfejsu API.  Jest to, trzeba napisać do utworzenia klasy XyzOptions:

```csharp
public class XyzOptions : DictionaryContainer {
# if !COREBUILD
    public XyzOptions () : base (new NSMutableDictionary ()) {}
    public XyzOptions (NSDictionary dictionary) : base (dictionary){}

    public nfloat? Volume {
       get { return GetFloatValue (XyzOptionsKeys.VolumeKey); }
       set { SetNumberValue (XyzOptionsKeys.VolumeKey, value); }
    }
    public string Caption {
       get { return GetStringValue (XyzOptionsKeys.CaptionKey); }
       set { SetStringValue (XyzOptionsKeys.CaptionKey, value); }
    }
# endif
}
```

Następnie powinien udostępniać metodą otoki, która wydobywa informacje dotyczące interfejsu API wysokiego poziomu, na podstawie interfejs API niskiego poziomu.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Jeśli Twój interfejs API nie musi zostać zastąpione, interfejs API oparty na NSDictionary bezpiecznie można ukryć za pomocą [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atrybutu.

Jak widać, używamy [ `[Wrap]` ](~/cross-platform/macios/binding/binding-types-reference.md#WrapAttribute) atrybutu udostępnienie nowego punktu wejścia interfejsu API, a firma Microsoft surface przy użyciu naszego silnie typizowaną `XyzOptions` klasy.  Metoda otoki umożliwia również o wartości null do przekazania.

Teraz jedną rzecz, którą firma Microsoft nie mogą zawierać jest gdzie `XyzOptionsKeys` pochodzą wartości.  Zwykle będzie grupie klucze, które interfejs API udostępnia w klasie statycznej, takich jak `XyzOptionsKeys`, podobnie do następującego:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

Przyjrzyjmy się automatyczna obsługa tworzenia silnie typizowane słowników.  Pozwala to uniknąć mnóstwo standardowej, a bezpośrednio w Twojej kontrakt interfejsu API, zamiast korzystać z zewnętrznego pliku, można zdefiniować słownika.

Aby utworzyć słownik silnie typizowane, oznacza wprowadzenie interfejsu w interfejsie API i dekoracji ją za pomocą [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) atrybutu.  Oznacza to, generator, należy utworzyć klasę z taką samą nazwę jak Twoje interfejs, który będzie dziedziczyć `DictionaryContainer` i zapewni silne typizowane metody dostępu dla niego.

[ `[StrongDictionary]` ](~/cross-platform/macios/binding/binding-types-reference.md#StrongDictionary) Atrybut przyjmuje jeden parametr, która jest nazwą klasy statycznej, która zawiera klucze słownika.  Następnie każda właściwość interfejsu staną się silnie typizowane metody dostępu.  Domyślnie kod użyje nazwy właściwości z sufiksem "Key" w klasie statycznej do tworzenia metody dostępu.

Oznacza to, czy tworzenie swojej silnie typizowane metody dostępu nie wymaga już zewnętrznego pliku, ani konieczności ręcznego tworzenia metod pobierających i ustawiających dla każdej właściwości ani konieczności wyszukiwania kluczy ręcznie samodzielnie.

Jest to, jak będzie wyglądać cały wiązania:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
[StrongDictionary ("XyzOptionKeys")]
interface XyzOptions {
    nfloat Volume { get; set; }
    string Caption { get; set; }
}

[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

W razie potrzeby odwoływać się w Twojej `XyzOption` członków innego pola (oznacza to nie nazwa właściwości z sufiksem `Key`), można dekoracji właściwość o [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atrybutu o nazwie, chcesz użyć.

<a name="Type_mappings" />

## <a name="type-mappings"></a>Mapowanie typu

W tej sekcji opisano, jak typy języka Objective-C są mapowane na typy języka C#.

<a name="Simple_Types" />

### <a name="simple-types"></a>Typy proste

W poniższej tabeli przedstawiono, jak powinny być mapowane typy z języka Objective-C i świata CocoaTouch świat platformy Xamarin.iOS:

|Nazwa typu języka Objective-C|Typ, ujednoliconego interfejsu API Xamarin.iOS|
|---|---|
|`BOOL`, `GLboolean`|`bool`|
|`NSInteger`|`nint`|
|`NSUInteger`|`nuint`|
|`CFTimeInterval` / `NSTimeInterval`|`double`|
|`NSString` ([więcej informacji na temat wiązania NSString](~/ios/internals/api-design/nsstring.md))|`string`|
|`char *`|`string` (Zobacz również: [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring))|
|`CGRect`|`CGRect`|
|`CGPoint`|`CGPoint`|
|`CGSize`|`CGSize`|
|`CGFloat`, `GLfloat`|`nfloat`|
|Typy CoreFoundation (`CF*`)|`CoreFoundation.CF*`|
|`GLint`|`nint`|
|`GLfloat`|`nfloat`|
|Typy Foundation (`NS*`)|`Foundation.NS*`|
|`id`|`Foundation`.`NSObject`|
|`NSGlyph`|`nint`|
|`NSSize`|`CGSize`|
|`NSTextAlignment`|`UITextAlignment`|
|`SEL`|`ObjCRuntime.Selector`|
|`dispatch_queue_t`|`CoreFoundation.DispatchQueue`|
|`CFTimeInterval`|`double`|
|`CFIndex`|`nint`|
|`NSGlyph`|`nuint`|

<a name="Arrays" />

### <a name="arrays"></a>Tablice

Środowisko uruchomieniowe platformy Xamarin.iOS automatycznie zajmie się konwertowanie na tablice języka C# do `NSArrays` i wykonywania konwersji, dlatego na przykład urojone Metoda języka Objective-C, która zwraca `NSArray` z `UIViews`:

```csharp
// Get the peer views - untyped
- (NSArray *)getPeerViews ();

// Set the views for this container
- (void) setViews:(NSArray *) views
```

Jest powiązany następująco:

```csharp
[Export ("getPeerViews")]
UIView [] GetPeerViews ();

[Export ("setViews:")]
void SetViews (UIView [] views);
```

Chodzi o to, aby użyć silnie typizowane C# macierzy, dzięki czemu będzie IDE w celu zapewnienia prawidłowego kodu zakończenia z rzeczywisty typ bez zmuszania użytkownika do odgadnięcia lub Wyszukaj dokumentację Aby dowiedzieć się, rzeczywisty typ obiektów znajdujących się w tablicy.

W przypadkach, w których nie można śledzić w dół rzeczywisty typ najbardziej pochodnej znajdujących się w tablicy, można użyć `NSObject []` jako wartość zwracaną.

<a name="Selectors" />

### <a name="selectors"></a>Selektory

Selektory są wyświetlane w interfejsie API języka Objective-C jako specjalny typ `SEL`. Podczas tworzenia powiązania z selektorem, mapującej typu, który ma `ObjCRuntime.Selector`.  Zazwyczaj selektory są widoczne w interfejsie API zarówno obiektu, obiekt docelowy i selektora do wywołania w obiekcie docelowym. Oba te po prostu zapewnienie odnosi się do delegowanego C#: coś, który hermetyzuje zarówno metody do wywołania, jak i obiekt, który można wywołać metody w.

Jest to, jak wygląda powiązania:

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

I to, jak metoda wykorzystania w aplikacji:

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        b.SetTarget (this, new Selector ("print"));
    }

    [Export ("print")]
    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

Aby wprowadzić powiązanie wrażeń deweloperów języka C#, zazwyczaj podaje się metody, która przyjmuje `NSAction` parametr, który pozwala delegaty C# i wyrażenia lambda, które ma być używany zamiast `Target+Selector`. W tym będzie najczęściej Ukryj `SetTarget` metody, oznaczając flagą ją za pomocą [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atrybutu, a następnie może narazić nowej metody pomocnika, takich jak to:

```csharp
// API.cs
interface Button {
   [Export ("setTarget:selector:"), Internal]
   void SetTarget (NSObject target, Selector sel);
}

// Extensions.cs
public partial class Button {
     public void SetTarget (NSAction callback)
     {
         SetTarget (new NSActionDispatcher (callback), NSActionDispatcher.Selector);
     }
}
```

Teraz kod użytkownika może być zapisany jako:

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        // First Style
        b.SetTarget (ThePrintMethod);

        // Lambda style
        b.SetTarget (() => {  /* print here */ });
    }

    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

<a name="Strings" />

### <a name="strings"></a>Ciągi

Kiedy są powiązania metody, która przyjmuje `NSString`, można zastąpić, przy użyciu języka C# typu string, jak i typy zwracane i parametry.

Tylko wówczas, gdy możesz chcieć użyć `NSString` jest bezpośrednio, gdy ten ciąg jest używany jako token. Aby uzyskać więcej informacji na temat ciągów i `NSString`, przeczytaj artykuł [projekt interfejsu API na NSString](~/ios/internals/api-design/nsstring.md) dokumentu.

W rzadkich przypadkach interfejs API może narazić ciąg notacji języka C (`char *`) zamiast ciągiem języka Objective-C (`NSString *`). W takich przypadkach można dodawać adnotacje do parametru o [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) atrybutu.

<a name="outref_parameters" />

### <a name="outref-parameters"></a>out / ref parametrów

Niektóre interfejsy API zwracają wartości w swoich parametrów lub przekazania parametrów poprzez odniesienie.

Przeważnie podpisie wygląda następująco:

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

Pierwszy przykład przedstawia typowe idiom języka Objective-C, aby zwrócić kody błędów, wskaźnik do `NSError` wskaźnik jest przekazywany, a po powrocie wartość jest ustawiona.   Druga metoda pokazuje, jak metoda języka Objective-C może przyjmować obiektu i modyfikowania jego zawartości.   Jest to przebiegu przez odwołanie, a nie wartość czyste dane wyjściowe.

Twoje powiązanie będzie wyglądać następująco:

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

<a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>Atrybuty zarządzania pamięci

Kiedy używasz [ `[Export]` ](~/cross-platform/macios/binding/binding-types-reference.md#ExportAttribute) atrybutu i przechodzi dane, które zostaną zachowane w metodzie wywoływanej, można określić semantyki argumentu przez przekazanie jej jako drugi parametr, na przykład:

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

Flaga będzie powyższych wartości jako posiadające semantykę "Zachowaj". Semantyka dostępne są następujące:

-  Przypisz
-  Kopiuj
-  Zachować

<a name="Style_Guidelines" />

### <a name="style-guidelines"></a>Wskazówki dotyczące stylu

<a name="Using_[Internal]" />

#### <a name="using-internal"></a>Za pomocą [wewnętrznych]

Możesz użyć [ `[Internal]` ](~/cross-platform/macios/binding/binding-types-reference.md#InternalAttribute) atrybutu, aby ukryć metodę z publicznego interfejsu API. Można to zrobić w przypadku gdy narażonych interfejs API jest zbyt niskiego poziomu, i chcesz zapewnić wysokiego poziomu implementacji w oddzielnym pliku, w oparciu o tę metodę.

Umożliwia także to napotkasz ograniczenia w generatorze powiązania, na przykład niektórych zaawansowanych scenariuszach może ujawnić typy, które nie są powiązane i chcesz powiązać w własny sposób, gdy chcesz zabalit do tych typów samodzielnie własny sposób.

<a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>Programy obsługi zdarzeń i wywołania zwrotne

Klasy języka Objective-C zwykle powiadomienia można rozgłaszać lub zażądać informacji, wysyłając wiadomość na klasy delegata (delegat języka Objective-C).

Ten model, podczas gdy w pełni obsługiwana i udostępniane przez rozszerzenia Xamarin.iOS czasami może być kłopotliwe. Rozszerzenie Xamarin.iOS uwidacznia wzorzec zdarzeń języka C# i systemu wywołanie zwrotne metody w klasie, która może służyć w takich sytuacjach. Dzięki temu kod do uruchomienia:

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

Generator powiązania jest w stanie skrócenie wpisywanie wymagane do mapowania wzorzec języka Objective-C na wzorcu języka C#.

Uruchamianie przy użyciu rozszerzenia Xamarin.iOS 1.4 umożliwi również poinstruować generatora Aby utworzyć powiązania dla określonych delegatów języka Objective-C i udostępnić delegata zdarzenia języka C# oraz właściwości na typ hosta.

Istnieją dwie klasy zaangażowanych w ramach tego procesu, klasy hosta, która będzie jest ten, który obecnie emituje zdarzenia, a następnie wysyła je na `Delegate` lub `WeakDelegate` i klasa rzeczywiste delegata.

Biorąc pod uwagę następujące ustawienia:

```csharp
[BaseType (typeof (NSObject))]
interface MyClass {
    [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
    NSObject WeakDelegate { get; set; }

    [Wrap ("WeakDelegate")][NullAllowed]
    MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
    [Export ("loaded:bytes:")]
    void Loaded (MyClass sender, int bytes);
}
```

Aby zawijać klasy należy:

-  W klasie hosta, należy dodać do usługi [`[BaseType]`](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute)  
   Deklaracja typu, który działa jako jego delegata i nazwę języka C#, która zostanie udostępniane. W powyższym przykładzie są `typeof (MyClassDelegate)` i `WeakDelegate` odpowiednio.
-  W klasie delegata w każdej metody, która ma więcej niż dwóch parametrów należy określić typ, który ma być automatycznie wygenerowanej klasy EventArgs.

Generator powiązanie nie jest ograniczona do zawijania tylko miejsce docelowe pojedyncze zdarzenie, jest to możliwe, że niektóre klasy języka Objective-C, aby emitować wiadomości do więcej niż jeden delegować, więc będzie musiał podać tablic do obsługi tej konfiguracji. Większość ustawień nie będą potrzebne, ale generator jest gotowy do obsługi tych przypadkach.

Następnie kod wynikowy będzie:

```csharp
[BaseType (typeof (NSObject),
    Delegates=new string [] {"WeakDelegate"},
    Events=new Type [] { typeof (MyClassDelegate) })]
interface MyClass {
        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }

        [Wrap ("WeakDelegate")][NullAllowed]
        MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
        [Export ("loaded:bytes:"), EventArgs ("MyClassLoaded")]
        void Loaded (MyClass sender, int bytes);
}
```

`EventArgs` Służy do określania nazwy `EventArgs` klasy do wygenerowania. Należy użyć jednego na podpisu (w tym przykładzie `EventArgs` będzie zawierać `With` właściwość typu nint).

O powyższych definicjach generator generuje następujące zdarzenie w wygenerowanym MyClass:

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

Więc można teraz użyć kodu następująco:

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

Wywołania zwrotne są tak samo jak wywołania zdarzenia, różnica jest to, że zamiast wielu subskrybentów potencjalnych (na przykład wiele metod można dołączyć do `Clicked` zdarzeń lub `DownloadFinished` zdarzeń) wywołań zwrotnych, może mieć tylko pojedynczego subskrybenta.

Proces jest identyczny, jedyną różnicą jest to, że zamiast uwidaczniania nazwę `EventArgs` klasy, które zostaną wygenerowane, EventArgs jest faktycznie używany do nazywania wynikowa nazwa delegat C#.

Jeśli metoda w klasie delegata zwróci wartość, generator powiązań będą Mapuj do metody delegata w klasie nadrzędnej, a nie zdarzenia. W takich przypadkach należy podać wartość domyślną, który powinien być zwrócony przez metodę, jeśli użytkownik nie Podłączanie delegata. Możesz to zrobić za pomocą [ `[DefaultValue]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) lub [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) atrybutów.

[`[DefaultValue]`](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueAttribute) zostanie trwale zakodowana wartość zwracana, gdy [ `[DefaultValueFromArgument]` ](~/cross-platform/macios/binding/binding-types-reference.md#DefaultValueFromArgumentAttribute) służy do określania, które argument wejściowy zostaną zwrócone.

<a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>Wyliczenia i typy podstawowe

Możesz też przywołać wyliczenia lub typów podstawowych, które nie są bezpośrednio obsługiwane przez system btouch definicji interfejsu. Aby to zrobić, umieść swoje wyliczeń i podstawowych typów w oddzielnym pliku, a następnie Dołącz to stanowiącego część jednej dodatkowych plików, które umożliwiają btouch.

<a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>Łączenie zależności

Są wiązane interfejsów API, które nie są częścią Twojej aplikacji, należy się upewnić, że plik wykonywalny jest połączony z tych bibliotek.

Należy poinformować Xamarin.iOS sposobu łączenia bibliotek, można to zrobić przez zmianę konfiguracji kompilacji do wywołania `mtouch` polecenia za pomocą pewne dodatkowe argumenty, określające, jak połączyć nowe biblioteki za pomocą kompilacji "-gcc_flags" opcji następuje ciąg w cudzysłowach zawierający wszystkich dodatkowych bibliotek, które są wymagane dla programu, takie jak to:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

Powyższy przykład połączy `libMyLibrary.a`, `libSystemLibrary.dylib` i `CFNetwork` biblioteki framework do końcowego pliku wykonywalnego.

Lub możesz skorzystać z poziomu zestawu [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), który można osadzić w plikach kontraktu (takie jak `AssemblyInfo.cs`).
Kiedy używasz [ `[LinkWithAttribute]` ](~/cross-platform/macios/binding/binding-types-reference.md#LinkWithAttribute), konieczne będzie mieć swoje dostępnym w momencie upewnij powiązania, zgodnie z tym nastąpi osadzenie natywnej biblioteki za pomocą aplikacji natywnej biblioteki. Na przykład:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

Możesz się zastanawiać, dlaczego potrzebujesz `-force_load` polecenia i przyczynę jest czy ObjC — Flaga, chociaż kompiluje kod, nie zachowuje metadane wymagane do obsługi kategorii (eliminacji nieużywany kod konsolidatora/kompilatora paski) zestawu wymagany w czasie wykonywania dla platformy Xamarin.iOS.

<a name="Assisted_References" />

## <a name="assisted-references"></a>Asystowana odwołania

Niektóre przejściowy obiektów, takich jak arkusze akcji i pola alertu są kłopotliwe do śledzenia dla deweloperów i generator powiązań może pomóc trochę tutaj.

Na przykład jeśli masz klasę, która wykazało, że wiadomość, a następnie generowane `Done` zdarzenia będą tradycyjnego obsługi to:

```csharp
class Demo {
    MessageBox box;

    void ShowError (string msg)
    {
        box = new MessageBox (msg);
        box.Done += { box = null; ... };
    }
}
```

W powyższym scenariuszu deweloper musi zapewnić odwołanie do obiektu, wykona i albo przeciek lub aktywnie wyczyść odwołanie do pola na jego własnej.  Obsługuje kod powiązania, generator śledzenia odwołania dla Ciebie i usuń zaznaczenie go po wywołaniu metody specjalne powyższy kod więc będzie:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

Zwróć uwagę, jak go jest już konieczne zapewnienie działa ze zmienną lokalną, i czy nie jest konieczne usunięcie odwołania, gdy obiekt jest niszczony zmiennej w wystąpieniu usługi.

Aby móc korzystać z tego, klasa powinny mieć właściwość zdarzenia w [ `[BaseType]` ](~/cross-platform/macios/binding/binding-types-reference.md#BaseTypeAttribute) deklaracji, a także `KeepUntilRef` zmienna jest ustawiana na nazwę metody, która jest wywoływana, gdy obiekt zakończy pracę, takie jak to:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

<a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>Dziedziczenie protokołów

Począwszy od platformy Xamarin.iOS v3.2, firma Microsoft obsługuje dziedziczenie z protokołów, które zostały oznaczone za pomocą [ `[Model]` ](~/cross-platform/macios/binding/binding-types-reference.md#ModelAttribute) właściwości. Jest to przydatne w przypadku niektórych wzorców interfejsu API, tak jak w `MapKit` gdzie `MKOverlay` protokołu, dziedziczy `MKAnnotation` protokołu i przyjmie liczby klas, które dziedziczą z `NSObject`.

W przeszłości wprowadziliśmy kopiowanie protokołu do każdego wdrożenia, ale w takich przypadkach teraz możemy spowodować `MKShape` klasa dziedziczy `MKOverlay` protokołu i wygeneruje wymaganych metod automatycznie.

## <a name="related-links"></a>Linki pokrewne

- [Przykład powiązania](https://developer.xamarin.com/samples/BindingSample/)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Usługa Xamarin University kurs: Tworzenie biblioteki powiązań języka Objective-C za pomocą narzędzie Objective Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
