---
title: "Powiązanie bibliotek języka Objective C"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: d1c4c46b62b95d70dd2832c96ffd2686163990a5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="binding-objective-c-libraries"></a>Powiązanie bibliotek języka Objective C

Podczas pracy z Xamarin.iOS lub Xamarin.Mac, mogą występować w przypadkach, w której chcesz korzystać z biblioteki języka Objective-C innych firm. W takiej sytuacji Xamarin powiązania projektów służy do tworzenia powiązanie C# do natywnych bibliotek języka Objective-C. Projekt używa tych samych narzędzi, których używamy w celu przełączenia systemów iOS i Mac interfejsów API języka C#.

W tym dokumencie opisano sposób powiązania interfejsów API języka Objective-C, są wiązane tylko interfejsy API C, w tym celu należy używać standardowego mechanizmu .NET [framework P/Invoke](http://mono-project.com/Dllimport).
Więcej informacji na temat statycznie łączyć biblioteki C są dostępne na [łączenie natywnych bibliotek](~/ios/platform/native-interop.md) strony.

Zobacz nasze Pomocnika [powiązanie Podręcznik typy](~/cross-platform/macios/binding/binding-types-reference.md).
Ponadto, jeśli chcesz dowiedzieć się więcej na temat zachodzących pod maską, sprawdź naszych [— wiązanie omówienie](~/cross-platform/macios/binding/overview.md) strony.

Powiązania mogą być tworzone dla systemu iOS i Mac bibliotek.
Ta strona zawiera opis sposobu pracy w systemie iOS powiązanie, jednak powiązania Mac są bardzo podobne.

**Przykładowy kod dla systemu iOS**

Można użyć [iOS powiązania przykładowych](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) projektu, aby wypróbować wiązania.

<a name="Getting_Started" />

## <a name="getting-started"></a>Wprowadzenie

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Najprostszym sposobem tworzenia powiązania jest tworzenie projektu platformy Xamarin.iOS powiązania.
Można to zrobić w programie Visual Studio dla komputerów Mac, wybierając typ projektu **systemu iOS > Biblioteka > biblioteki powiązania**:

[![](objective-c-libraries-images/00-sml.png "W tym celu z programu Visual Studio dla komputerów Mac Wybieranie typu projektu iOS biblioteki powiązania biblioteki")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Najprostszym sposobem tworzenia powiązania jest tworzenie projektu platformy Xamarin.iOS powiązania.
Można to zrobić w programie Visual Studio w systemie Windows, wybierając typ projektu **Visual C# > iOS > biblioteki powiązania (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "iOS powiązania biblioteki z systemem iOS")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> Uwaga: Powiązania projektów dla **Xamarin.Mac** są obsługiwane tylko w programie Visual Studio dla komputerów Mac.

-----

Wygenerowanego projektu zawiera małe szablon, który można edytować, zawiera dwa pliki: `ApiDefinition.cs` i `StructsAndEnums.cs`.

`ApiDefinition.cs` Miejscu należy określić kontrakt interfejsu API jest plik, który opisuje sposób podstawowej interfejsu API języka Objective C jest zaprojektowana w języku C#. Składnia i zawartość tego pliku są tematem głównym dyskusji tego dokumentu i jej zawartość są ograniczone do interfejsów języka C# i deklaracje delegatów C#. `StructsAndEnums.cs` Plik jest plikiem, w którym będzie wprowadzić inne definicje, które są wymagane przez interfejsów i delegatów. W tym wartości wyliczenia i struktury, którzy mogą korzystać z kodu.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>Powiązanie z interfejsu API

Wykonywania kompleksowe powiązania, należy zrozumieć definicji interfejsu API języka Objective C i zapoznać się z wytycznymi dla platformy .NET Framework projektu.

Powiązanie biblioteki będzie zazwyczaj rozpoczyna się przy użyciu pliku definicji interfejsu API. Plik definicji interfejsu API jest jedynie C# pliku źródłowego, który zawiera powiązanie C# interfejsów, które zostały opatrzoną kilka atrybutów, które pomagają dysku.  Ten plik znajduje się co definiuje kontrakt między C# i języka Objective C jest.

Na przykład to prosta plik interfejsu api dla biblioteki:

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

Powyższym przykładzie zdefiniowano klasę o nazwie `Cocos2D.Camera` która pochodzi z `NSObject` typ podstawowy (ten typ pochodzi z `Foundation.NSObject`) i który definiuje właściwość statyczna (`ZEye`), dwie metody, które przyjmują bez argumentów i metody, które przyjmuje trzy argumenty.

Szczegółowe omówienie formatu pliku interfejsu API i atrybutów, których można użyć jest objęte [plik definicji interfejsu API](~/cross-platform/macios/binding/objective-c-libraries.md) poniższej sekcji.

Aby uzyskać pełną powiązanie, będzie zwykle dotyczy cztery składniki:

-  Plik definicji interfejsu API (`ApiDefinition.cs` w szablonie).
-  Opcjonalnie: wszystkie wyliczenia, typów, wymagane przez plik definicji interfejsu API struktury (`StructsAndEnums.cs` w szablonie).
-  Opcjonalnie: dodatkowe źródeł, które może rozwinąć wygenerowanego powiązanie lub podać więcej C# przyjazną interfejsu API (C# pliki, które można dodać do projektu).
-  Natywnej biblioteki dokonywane jest wiązanie.

Ten wykres pokazuje relację między plikami:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "Ten wykres pokazuje relację między plikami")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

Plik definicji interfejsu API: będzie zawierać tylko obszary nazw i interfejs definicji (żadnych elementów członkowskich, które interfejs może zawierać) i nie powinna zawierać klas, wyliczeń, delegatów lub struktury. Plik definicji interfejsu API jest jedynie kontraktu, która będzie służyć do generowania interfejsu API.

Dodatkowy kod czy muszą być wyliczenia lub klasy pomocnicze powinna być obsługiwana w osobnym pliku, w przykładzie przedstawionym powyżej "CameraMode" jest wartością wyliczenia, która nie istnieje w pliku CS i powinna być obsługiwana w osobnym pliku, na przykład `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

`APIDefinition.cs` Pliku jest połączona z `StructsAndEnum` klasy i są używane do generowania powiązanie core biblioteki. Można użyć wynikowa Biblioteka jako — jest, ale zazwyczaj można dostosować wynikowy biblioteki, aby dodać niektóre funkcje C# dla użytkowników. Oto kilka przykładów implementacji `ToString()` metody dostarczy indeksatory C#, Dodaj niejawne konwersje do i z niektórych typów natywnych lub silnie typizowane wersje niektórych metod. Te ulepszenia są przechowywane w dodatkowe pliki C#. Tylko dodać pliki C# do projektu i zostaną uwzględnione w procesie kompilacji.

Oznacza to, jak może wdrożyć kod w Twojej `Extra.cs` pliku. Zwróć uwagę, że będziesz używać klas częściowych jak te rozszerzyć klasy częściowe, które są generowane z kombinacji `ApiDefinition.cs` i `StructsAndEnums.cs` podstawowe powiązania:

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

Kompilowanie biblioteki utworzy natywnego wiązania.

Aby ukończyć tego powiązania, należy dodać natywnej biblioteki do projektu.  Można to zrobić przez dodanie do projektu, poprzez przeciągnięcie i upuszczenie natywnej biblioteki z wyszukiwania na projekt w Eksploratorze rozwiązań lub natywnej biblioteki prawym przyciskiem myszy projekt i wybierając pozycję **Dodaj**  >  **Dodaj pliki** wybierz natywnej biblioteki.
Natywnych bibliotek Konwencja rozpoczynać się od słowa "lib" i kończy się rozszerzeniem ".a". Gdy to zrobisz, Visual Studio for Mac doda dwa pliki: `.a` plików i automatycznie wypełnione C# plik zawierający informacje o natywnej biblioteki zawiera:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "Natywnych bibliotek Konwencja rozpoczynać word lib i kończyć .a rozszerzenia")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

Zawartość `libMagicChord.linkwith.cs` plik zawiera informacje o używaniu tej biblioteki i instruuje środowiskiem IDE, aby ten plik binarny tworzenia pakietów w wynikowym pliku DLL:

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

Szczegółowe informacje o sposobie używania atrybutu LinkWith są udokumentowane w naszym [powiązanie Podręcznik typy](~/cross-platform/macios/binding/binding-types-reference.md).

Teraz podczas kompilowania projektu zostanie na końcu `MagicChords.dll` pliku, który zawiera powiązanie i natywnej biblioteki. Można rozpowszechniać tego projektu, lub użyj wynikowa Biblioteka DLL, aby inni deweloperzy dla ich własnych.

Czasami może się okazać konieczne kilka wartości wyliczenia i definicji delegata oraz innych typów. Nie umieszczaj w pliku definicji interfejsu API, jak jest to tylko kontrakt

 <a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>Plik definicji interfejsu API

Plik definicji interfejsu API składa się z wielu interfejsów. Interfejsy w definicji interfejsu API zostanie włączone w deklaracji klasy, a musi być dekorowane za [[BaseType]](~/cross-platform/macios/binding/binding-types-reference.md) atrybutu, aby określić klasę podstawową dla klasy.

Użytkownik może się zastanawiać, dlaczego firma Microsoft nie używa klasy zamiast interfejsów dla definicję kontraktu. Firma Microsoft pobrane interfejsów, ponieważ dozwolone firmie Microsoft w celu zapisu kontrakt dla metody bez konieczności podanie treści metody w pliku definicji interfejsu API i konieczności podanie treści, która było zgłosić wyjątek, lub zwróć odpowiednią wartość.

Jednak ponieważ interfejsu jako szkielet jest używany do generowania klasy było odwołać się do dekoracji różnych części umowy z atrybutami do powiązania.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>Metody wiązania

Najprostsza powiązania, które może wykonywać jest powiązać metodę. Po prostu zadeklarować metody w interfejsie o konwencje nazewnictwa C# i dekoracji metody [[eksportowanie]](~/cross-platform/macios/binding/binding-types-reference.md) atrybutu. Atrybut [eksportu] jest co łączy nazwę C# o nazwie Objective-C w środowisku uruchomieniowym platformy Xamarin.iOS. Parametr atrybut eksportu jest nazwą selektor języka Objective-C, kilka przykładów:

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

Powyższe przykłady pokazują, jak można powiązać metody wystąpienia. Aby powiązać metody statyczne, należy użyć `[Static]` atrybutu następująco:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

Jest to wymagane, ponieważ kontrakt jest częścią interfejs i interfejsy mają nie pojęcie deklaracji wystąpienia vs statyczne, więc ponownie odwołać się do atrybutów. Jeśli chcesz ukryć konkretnej metody z powiązania można dekoracji metody [[wewnętrznego]](~/cross-platform/macios/binding/binding-types-reference.md) atrybutu.

`btouch-native` Polecenia wprowadzi sprawdza, czy parametry odwołania nie mieć wartości null. Jeśli chcesz zezwolić na wartości null dla określonego parametru, użyj [[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md) atrybutu w parametrze, jak to:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

Podczas eksportowania z typem referencyjnym `[Export]` — słowo kluczowe można również określić semantykę alokacji. Jest to niezbędne do zapewnienia przeciek żadne dane.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>Właściwości powiązania

Podobnie jak metody, właściwości języka Objective C jest powiązana za pomocą [[eksportowanie]](~/cross-platform/macios/binding/binding-types-reference.md) atrybut i mapy bezpośrednio do właściwości języka C#. Podobnie jak metody, właściwości mogą być dekorowane za [[Static]](~/cross-platform/macios/binding/binding-types-reference.md) i [[wewnętrznego]](~/cross-platform/macios/binding/binding-types-reference.md) atrybutów.

Jeśli używasz `[Export]` atrybutu dla właściwości, w obszarze btouch obejmuje natywne faktycznie wiąże dwóch metod: metody pobierającej i ustawiającej. Nazwę można wyeksportować jest **nazwę bazową** i metoda setter jest obliczana przez poprzedzenie jej "set", włączanie pierwszą literę słowa **nazwę bazową** do wielkimi literami i podejmowanie selektora potrwać argument. Oznacza to, że `[Export ("label")]` zastosowane na właściwość faktycznie wiąże "label" i "setLabel:" metod Objective-C.

Czasami właściwości Objective-C nie wykonuj wzorzec opisane powyżej, a nazwa to ręcznie zastąpione. W takich przypadkach można kontrolować sposób, że powiązanie jest generowany przy użyciu `[Bind]` atrybutu metoda pobierająca lub ustawiająca, na przykład:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

Wówczas wiąże "isMenuVisible" i "setMenuVisible:". Opcjonalnie właściwość może być powiązana, używając następującej składni:

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

Gdzie pobierającej i ustawiającej jawnie są zdefiniowane jako w `name` i `setName` powiązania powyżej.

Oprócz obsługi statycznej właściwości, za pomocą `[Static]`, można dekoracji właściwości statyczne dla wątku z [[IsThreadStatic]](~/cross-platform/macios/binding/binding-types-reference.md), na przykład:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

Podobnie jak metody umożliwiają niektórych parametrów do [[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md), można zastosować [[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md) do właściwości, aby wskazać, że wartość null jest prawidłową wartością dla właściwości, na przykład:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

[[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md) bezpośrednio na metodę ustawiającą można również określić parametr:

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>Ostrzeżenia powiązania Kontrolki niestandardowe

Podczas konfigurowania wiązania dla kontrolki niestandardowej, należy rozważyć następujące ostrzeżenia:

1. **Powiązanie właściwości muszą być statyczne** — podczas definiowania powiązania właściwości `Static` atrybut musi być używany.
2. **Nazwy właściwości muszą być zgodne** — nazwa używana do powiązania właściwość musi odpowiadać nazwie właściwości w kontrolki niestandardowej dokładnie.
3. **Typy właściwości muszą być zgodne** — typ zmiennej używanych do wiązania właściwości musi odpowiadać typowi właściwości w kontrolki niestandardowej dokładnie.
4. **Punkty przerwania i metody pobierającej/ustawiającej** — umieścić punkty przerwania w metodzie pobierającej lub metody ustawiające właściwości nigdy nie zostanie uruchomiona.
5. **Wywołania zwrotne obserwować** -konieczne będzie otrzymywać powiadomienia o zmianach w wartości właściwości niestandardowych kontrolek za pomocą wywołania zwrotne obserwacji.

Nieprzestrzeganie wszelkie ostrzeżenia wymienionych powyżej może spowodować powiązanie dyskretnie niepowodzeniem w czasie wykonywania.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Wzorzec modyfikowalną Objective-C i właściwości

Struktury języka Objective-C za pomocą idiom gdzie niektórych klas są niezmienne z modyfikowalną podklasy.   Na przykład `NSString` jest niezmienialny wersji, podczas gdy `NSMutableString` podklasą klasy, która umożliwia mutacji.

W tych klas jest częściej można zobaczyć niezmienne klasy podstawowej zawierają właściwości z metody pobierającej, ale metody ustawiającej.   I mutable wersji wprowadzenie metody ustawiającej.   Ponieważ nie jest to naprawdę możliwe w języku C#, było mapować tej idiom do idiom, który będzie działać w języku C#.

Sposób ten jest mapowany na język C# jest dodanie metody pobierającej i ustawiającej dla klasy podstawowej, ale Flagowanie metody ustawiającej z `[NotImplemented]` atrybutu.

Następnie na podklasy modyfikowalna, należy użyć `[Override]` atrybutu dla właściwości, aby upewnić się, że właściwość jest rzeczywiście zastępowanie zachowania elementu nadrzędnego.

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

### <a name="binding-constructors"></a>Powiązanie konstruktorów

**Btouch native** narzędzie automatycznie wygeneruje fours konstruktorów w klasie, dla danej klasy `Foo`, generuje:

-  `Foo ()`: Konstruktor domyślny (maps do konstruktora "init" Objective-C)
-  `Foo (NSCoder)`: Konstruktor używany podczas deserializacji obiektu NIB plików (mapuje Objective-C "initWithCoder:" Konstruktor).
-  `Foo (IntPtr handle)`: Konstruktor tworzonej na podstawie dojścia wywoływanie odbywa się przez środowisko uruchomieniowe gdy środowiska uruchomieniowego musi ujawniać obiektu zarządzanego z niezarządzanej obiektu.
-  `Foo (NSEmptyFlag)`: Aby uniknąć podwójnego inicjowania jest używany przez klas pochodnych.

Dla konstruktorów zdefiniowanych przez użytkownika, muszą być deklarowane przy użyciu następujących podpisu wewnątrz definicji interfejsu: one musi zwracać `IntPtr` wartość i nazwa metody powinna być konstruktora. Na przykład aby powiązać `initWithFrame:` konstruktora, jest to, czy użyć:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

 <a name="Binding_Protocols" />

### <a name="binding-protocols"></a>Powiązania protokołów

Zgodnie z opisem w dokumencie projekt interfejsu API w sekcji [dyskutować modeli i protokoły](~/ios/internals/api-design/index.md), Xamarin.iOS mapuje protokołów Objective-C do klasy, które zostały oznaczone stanem [[Model]](~/cross-platform/macios/binding/binding-types-reference.md) atrybutu. Służy to zazwyczaj podczas implementowania Objective-C klasy obiektów delegowanych.

Istotną różnicą między zwykłej klasy powiązane i klasa obiektu delegowanego jest klasa delegata może być co najmniej jedną metodę opcjonalne.

Na przykład rozważmy `UIKit` klasy `UIAccelerometerDelegate`, jest to, jak jest powiązana w Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

Ponieważ jest to metoda opcjonalne w definicji dla `UIAccelerometerDelegate` nie ma nic innego celu. Ale jeśli wystąpił wymaganej metody protokołu, należy dodać [[abstrakcyjny]](~/cross-platform/macios/binding/binding-types-reference.md) atrybut do metody. Spowoduje to wymuszenie od użytkownika implementacji faktycznie treści metody.

Ogólnie rzecz biorąc protokoły są używane w klasach, które odpowiada na komunikaty. Jest to zazwyczaj wykonywane w języku Objective C przez przypisanie do właściwości "delegowanie" wystąpienia obiektu, który odpowiada metod w protokole.

Konwencja w Xamarin.iOS jest do obsługi zarówno Objective-C słabo sprzężona stylu, gdy dowolne wystąpienie `NSObject` można przypisać do delegata i aby również Uwidacznianie silnie typizowaną wersję. Z tego powodu zwykle udostępniamy silnie typizowany właściwość "Delegowanie" i "WeakDelegate" słabo wpisany. Firma Microsoft zazwyczaj powiązać słabo typizowana wersji z eksportu i używamy [[zawijać]](~/cross-platform/macios/binding/binding-types-reference.md) atrybutu zapewnienie silnie typizowaną wersję.

Oznacza to, jak firma Microsoft powiązany `UIAccelerometer` klasy:

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

Począwszy od MonoTouch 7.0 nowych i ulepszonych protokołu, powiązania funkcji zostały włączone.  Ta nowa funkcja obsługi uproszczono do używania języka Objective-C idioms przyjmowania jednego lub więcej protokołów w danej klasy.

Dla każdej definicji protokołu `MyProtocol` w języku Objective-C, jest teraz `IMyProtocol` interfejs, który zawiera wszystkie wymagane metody z protokołu, a także rozszerzenie klasy, która zawiera wszystkie metody opcjonalne.  Powyżej, łączyć nowa funkcja obsługi w programie Xamarin Studio Edytor umożliwia deweloperom wdrażanie metod protokołu bez konieczności użycia osobnych podklasy poprzedniej klas abstrakcyjnych modelu.

Żadnych definicji, który zawiera `[Protocol]` atrybutu powoduje wygenerowanie trzech klas pomocniczych, które znacząco poprawić sposób korzystać z protokołów:

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

**Implementacja klasy** zapewnia pełną klasa abstrakcyjna, można zastąpić poszczególnych metod i Pobierz pełny typ bezpieczeństwa.  Istnieją z powodu C# nie obsługuje dziedziczenia wiele scenariuszy, w którym może musi mieć różne klasy podstawowej, ale nadal chcesz zaimplementować interfejsu, gdzie

Wygenerowany **definicji interfejsu** polega na.  Jest to interfejs, który zawiera wszystkie wymagane metody z protokołu.  Dzięki temu deweloperzy, których chcesz wdrożyć jedynie implementować interfejs użytkownika protokołu.  Środowisko uruchomieniowe automatycznie zarejestruje typ jako przyjmowanie protokołu.

Należy zauważyć, że interfejs tylko wymieniono wymagane metody a ujawniać metod opcjonalne.  Oznacza to uzyska pełny typ sprawdzania pod kątem wymaganych metod klasy, które przyjmują protokołu, ale ma odwołać się do wpisywania słabe (ręcznie przy użyciu atrybutów eksportu dopasowanie i podpis) dla metod protokołu opcjonalne.

Aby uprościć jego użycie interfejsu API, który używa protokołów, narzędzie powiązanie również utworzy klasę — metoda rozszerzenia, która udostępnia wszystkie metody opcjonalne.  Oznacza to, że tak długo, jak zużywają interfejsu API, można traktować jako zawierający wszystkie metody protokołów.

Jeśli chcesz używać protokołu definicji interfejsu API, musisz zapisać szkielet pustych interfejsów w definicji interfejsu API.  Jeśli chcesz użyć Mój_protokół w interfejsie API, będzie potrzebny w tym celu:

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

Powyższe jest potrzebna, ponieważ w czasie wiązania `IMyProtocol` czy nie istnieje, to znaczy Dlaczego należy podać pustego interfejsu.

#### <a name="adopting-protocol-generated-interfaces"></a>Przyjmowanie generowanych przez protokół interfejsów

Zawsze, gdy wdrożenie jest jednego z interfejsów wygenerowany dla protokołów, w następujący sposób:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Implementację metody interfejsu automatycznie pobiera wyeksportowane o nazwie poprawnej, więc jest to równoważne:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Nie ma znaczenia, jeśli interfejs zostanie zaimplementowana jawnie lub niejawnie.

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>Powiązanie rozszerzenia klas

<!--In Objective-C it is possible to extend classes with new methods,
similar in spirit to C#'s extension methods. When one of these methods
is present, you can use the `[Target]` attribute to flag the first
parameter of a method as being the receiver of the Objective-C
message.

For example, in Xamarin.iOS we bound the extension methods that are defined on
`NSString` when `UIKit` is imported as methods in the `UIView`, like this:

```csharp
[BaseType (typeof (UIResponder))]
interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

-->

W języku Objective C jest możliwe rozszerzenie klasy z nowych metod podobne duchu C# dla metody rozszerzenia. Jeśli występuje jeden z tych metod, możesz użyć `BaseType` Flaga metodę jako odbiorcy wiadomości języka Objective C dla atrybutu.

Na przykład w Xamarin.iOS, możemy powiązane metody rozszerzenia, które są zdefiniowane w `NSString` podczas `UIKit` zostanie zaimportowana jako metody `NSStringDrawingExtensions`, podobnie do następującej:

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

 <a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>Powiązanie listy argumentów Objective-C

Objective-C obsługuje argumentów ze zmienną liczbą argumentów, możesz użyć następujących techniki opisane przez Zach Gris na [ten wpis](http://forums.monotouch.net/yaf_postst311_SOLVED-Binding-ObjectiveC-Argument-Lists.aspx).

Komunikat Objective-C wygląda następująco:

```csharp
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

Aby wywołać tę metodę w języku C# można utworzyć podpisu następująco:

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

Deklaruje to metoda jako wewnętrzne, ukrywanie powyżej interfejsu API od użytkowników, ale ujawnienie go do biblioteki. Następnie można napisać metodę następująco:

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

Czasami można uzyskać dostępu do pola publiczne, które zostały zgłoszone w bibliotece.

Zazwyczaj te pola zawiera musi odwoływać się wartości ciągów lub liczb całkowitych. Są często używane jako parametry, które reprezentują określonych powiadomień, a klucze w słowniku.

Aby powiązać pole, Dodaj właściwość do pliku definicji interfejsu i dekoracji właściwości o [[Field]](~/cross-platform/macios/binding/binding-types-reference.md) atrybutu. Ten atrybut przyjmuje jeden parametr: nazwa C symbolu do wyszukiwania. Na przykład:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

Jeśli ma być zawijany różnych pól w klasie statycznej, który nie pochodzi od `NSObject`, można użyć `[Static]` atrybutu dla klasy, jak to:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

Powyższe wygeneruje `LonelyClass` który nie pochodzi od `NSObject` i będzie zawierać powiązanie z `NSSomeEventNotification` 
 `NSString` udostępniony jako `NSString`.

`[Field]` Atrybut można stosować do następujących typów danych:

-  `NSString` odwołania (tylko właściwości tylko do odczytu)
-  `NSArray` odwołania (tylko właściwości tylko do odczytu)
-  wskazówki 32-bitowych (`System.Int32`)
-  wskazówki 64-bitowych (`System.Int64`)
-  32-bitowych liczb zmiennoprzecinkowych (`System.Single`)
-  64-bitowych liczb zmiennoprzecinkowych (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

Oprócz nazwy natywnego pola można określić nazwy biblioteki pole lokalizacji, przekazując nazwę biblioteki:

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

W przypadku łączenia statycznie nie żadnej biblioteki do powiązania, dlatego należy użyć `__Internal` nazwy:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>Typy wyliczeniowe powiązania

Możesz dodać `enum` bezpośrednio w powiązaniu z plików ułatwia ich używać wewnątrz definicji interfejsu API - bez korzystania z innego źródła pliku (musi być skompilowany w powiązaniach i ostatecznego projektu).

Przykład:

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

Istnieje również możliwość utworzenia własnych wyliczenia, aby zastąpić `NSString` stałe. W takim przypadku zostanie generatora **automatycznie** Tworzenie metod do konwertowania wartości wyliczenia i stałe NSString dla Ciebie.

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

W powyższym przykładzie można podjąć decyzję o dekoracji `void Perform (NSString mode);` z `[Internal]` atrybutu. Spowoduje to **Ukryj** interfejsu API na podstawie stała od użytkowników powiązania.

Jednak powoduje ograniczenie tworzenie podklas typ jako wrażeń zastosowań alternatywnych interfejsu API `[Wrap]` atrybutu. Te metody wygenerowanego nie są `virtual`, tj. Możesz nie mieć możliwość zastąpienia ich — które mogą lub nie, być dobrym rozwiązaniem.

Alternatywą jest oryginalne, Oznacz `NSString`— na podstawie definicji jako `[Protected]`. Umożliwi to podklasy do pracy, gdy jest to wymagane, a wersja wrap'ed będą nadal działać i Wywołaj przesłoniętej metody.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>Powiązanie NSValue, NSNumber i NSString lepsze typem

[[BindAs]](~/cross-platform/macios/binding/binding-types-reference.md) atrybutu umożliwia powiązanie `NSNumber`, `NSValue` i `NSString`(Typy wyliczeniowe) do dokładniejsze typów C#. Ten atrybut służy do tworzenia lepsze, bardziej precyzyjne interfejs API .NET za pośrednictwem natywnego interfejsu API.

Można dekoracji metody (dla wartości zwracanej), parametrów i właściwości, do których [[BindAs]](~/cross-platform/macios/binding/binding-types-reference.md). Jedynym ograniczeniem jest to, że elementów członkowskich **nie mogą** znajdować się wewnątrz `[Protocol]` lub `[Model]` interfejsu.

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

Wewnętrznie wykonamy `bool?`  <->  `NSNumber` i `CGRect`  <->  `NSValue` konwersji.

[[BindAs] ](~/cross-platform/macios/binding/binding-types-reference.md) obsługuje również tablice `NSNumber` `NSValue` i `NSString`(Typy wyliczeniowe).

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

`CAScroll` jest `NSString` kopii wyliczenia, możemy spowoduje pobranie prawo `NSString` wartość i obsługiwać konwersji typu.

Zobacz [dokumentacji [BindAs]](~/cross-platform/macios/binding/binding-types-reference.md) Zobacz obsługiwane typy konwersji.

 <a name="Binding_Notifications" />

### <a name="binding-notifications"></a>Powiązanie powiadomienia

Powiadomienia są komunikaty, które są wysyłane do `NSNotificationCenter.DefaultCenter` i są używane jako mechanizm wysyłać wiadomości z jednej części aplikacji do innej. Deweloperzy subskrybować powiadomienia zwykle używanie [NSNotificationCenter](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/)w [AddObserver](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/M/AddObserver/) metody. Gdy aplikacja ogłasza komunikatu do Centrum powiadomień, zwykle zawiera przechowywana w ładunku [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) słownika. Ten słownik lekko został wpisany i uzyskiwanie informacji z jego błędu podatnych na błędy, ponieważ użytkownicy muszą się zazwyczaj do odczytu w dokumentacji klucze, które są dostępne w słowniku i typy wartości, które mogą być przechowywane w słowniku. Obecność klucze czasami jest używany jako wartość logiczną, a także.

Generator powiązania Xamarin.iOS zapewnia obsługę dla deweloperów można powiązać powiadomienia. Aby to zrobić, należy ustawić [[powiadomień]](~/cross-platform/macios/binding/binding-types-reference.md) atrybutu dla właściwości, która również zostały oznaczone [[Field]](~/cross-platform/macios/binding/binding-types-reference.md) właściwości (może to być publicznych lub prywatnych).

Ten atrybut może być używany bez argumentów dla powiadomień, które zawiera ładunek lub można określić `System.Type` który odwołuje się do innego interfejsu w definicji interfejsu API, zwykle o nazwie kończą się ciągiem "EventArgs". Generator spowoduje wyłączenie interfejsu w klasie tej podklasy `EventArgs` i będzie zawierać wszystkie właściwości, na liście. `[Export]` Atrybut powinien być używany w klasie, EventArgs, aby wyświetlić listę nazwę używaną do odszukania słownika języka Objective C można pobrać wartości klucza.

Na przykład:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Powyższy kod wygeneruje klasy zagnieżdżonej `MyClass.Notifications` z następujących metod:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Użytkownicy kodu łatwo można następnie Subskrybowanie powiadomień o publikowanego [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) przy użyciu kodu w następujący sposób:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Zwracana wartość z `ObserveDidStart` można łatwo zrezygnować z otrzymywania powiadomień, w następujący sposób:

```csharp
token.Dispose ();
```

Lub możesz wywołać [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject/) i przekaż token. Jeśli powiadomienia zawiera parametry, należy określić obiekt pomocnika `EventArgs` interfejsu następująco:

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

Powyższe wygeneruje `MyScreenChangedEventArgs` klasy z `ScreenX` i `ScreenY` właściwości, które spowoduje pobranie danych z [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) słownika przy użyciu kluczy "ScreenXKey" i "ScreenYKey" odpowiednio i zastosowanie odpowiednich konwersji. `[ProbePresence]` Atrybut jest używany dla generatora do sondowania, jeśli klucz jest ustawiany `UserInfo`, zamiast wyodrębniania wartości. Służy to do przypadku obecności klucza wartość (zazwyczaj dla wartości logicznych).

Dzięki temu można napisać kod następująco:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

 <a name="Binding_Categories" />

### <a name="binding-categories"></a>Powiązanie kategorii

Kategorie są mechanizm Objective-C, używany do rozszerzania zestaw metod i właściwości dostępne w klasie.   W praktyce, są one używane do albo rozszerzyć funkcjonalność klasy podstawowej (na przykład `NSObject`) podczas konsolidowana określonej platformy (na przykład `UIKit`), co ich metod dostępne, ale tylko wtedy, gdy jest dołączane nowej struktury.   W niektórych przypadkach są one używane do organizowania funkcji w klasie według funkcji.   Są one podobne w duchu do metody rozszerzenia C#. Jest to, jak kategoria będzie wyglądać w celu C:

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Powyższy przykład jeśli znaleziono na biblioteki może rozszerzać wystąpienia `UIView` z metodą `makeBackgroundRed`.

Aby powiązać te, można użyć `[Category]` atrybutu w definicji interfejsu.  Jeśli za pomocą kategorii atrybutu, znaczenie `[BaseType]` atrybutu zmieni się z używany do określenia klasy podstawowej, aby rozszerzyć, aby być typ do rozszerzenia.

Poniżej przedstawiono sposób `UIView` rozszerzenia są powiązane i przekształcane w metodach rozszerzeń C#:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Powyższe utworzy `MyUIViewExtension` klasę, która zawiera `MakeBackgroundRed` — metoda rozszerzenia.  Oznacza to, że można teraz wywołać "MakeBackgroundRed" na dowolnym `UIView` podklasy, umożliwiając te same funkcje jak Objective-C. W niektórych przypadkach kategorie są używane, nie można rozszerzyć klasy systemu, ale do organizowania funkcji wyłącznie w celach decoration.  Jak to:

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

Chociaż można używać `Category` atrybutu również tego stylu decoration deklaracji, można także po prostu dodać je wszystkie do definicji klasy.  Czy oba te osiągnięcia takie same:

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

Jest krótszy tylko w tych przypadkach można scalić kategorie:

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

Bloki są nową konstrukcję wprowadzone przez firmę Apple, aby wyświetlić funkcjonalności odpowiednikiem metod anonimowych C# w celu Objective-C. Na przykład `NSSet` klasy udostępnia teraz tej metody:

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

Opis powyżej deklaruje metody o nazwie "*enumerateObjectsUsingBlock:*" pobierającej jeden argument o nazwie *bloku*. Ten blok jest podobna do metody anonimowej C#, w tym obsługuje przechwytywania w bieżącym środowisku ("this" wskaźnik, dostęp do zmiennych lokalnych i parametrów). W powyższym metody `NSSet` wywołuje bloku z dwoma parametrami `NSObject` (część "identyfikator obj") i wskaźnik na wartość logiczną ("BOOL * Zatrzymaj") części.

Aby powiązać tego rodzaju interfejsu API z btouch, musisz najpierw zadeklarować podpis typu bloku, jak C# delegować i odwołania z interfejsu API punktu wejścia, jak to:

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

A teraz kodzie można wywołać funkcji w języku C#:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

Jeśli wolisz, można także użyć wyrażenia lambda:

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

 <a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>Metod asynchronicznych

Generator powiązania można włączyć klasy metod do metod asynchronicznych przyjaznego (metody, które zwracają Task lub Task&lt;T&gt;).

Można użyć `[Async]` atrybutu metody, które zwracają void i których ostatni argument jest wywołaniem zwrotnym.  Po zastosowaniu to metody generator powiązanie wygeneruje wersja tej metody z sufiksem `Async`.  Jeśli wywołanie zwrotne nie przyjmuje żadnych parametrów, wartość zwracana będzie `Task`, jeśli wywołanie zwrotne przyjmuje parametr, wynikiem będzie `Task<T>`.  Jeśli wywołanie zwrotne przyjmuje wiele parametrów, należy ustawić `ResultType` lub `ResultTypeName` określić odpowiednią nazwę wygenerowanego typu, w której będą przechowywane wszystkie właściwości.

Przykład:

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

Powyższy kod zostanie wygenerowany zarówno funkcję LoadFile metody, jak również:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Udostępniając Silne typy parametrów słabego NSDictionary

W wielu miejscach w interfejsie API języka Objective-C, parametry są przekazywane jako lekko wpisany `NSDictionary` interfejsów API za pomocą określonych kluczy i wartości, ale te są podatne na (przekazać nieprawidłowe klucze i pobrać żadnych ostrzeżeń; Przekaż nieprawidłowe wartości i uzyskiwanie żadnych ostrzeżeń) błąd i irytujące Aby użyć, ponieważ wymagają one wiele rund dokumentacji w celu wyszukania możliwe nazwy klucza i wartości.

Rozwiązanie jest zapewnienie silnie typizowaną wersję, która udostępnia silnie typizowaną wersję interfejsu API i w tle mapy różnych podstawowej klucze i wartości.

Tak na przykład, jeśli interfejsu API języka Objective-C `NSDictionary` i jest udokumentowany jako biorąc klucza "XyzVolumeKey", która przyjmuje `NSNumber` z woluminu wartość z zakresu od 0,0 do 1.0 i "XyzCaptionKey" pobierający ciąg byłaby wskazana swoim użytkownikom nieuprzywilejowany Interfejs API, który wygląda następująco:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

`Volume` Właściwość jest zdefiniowana jako wartości null typu float, zgodnie z Konwencją w języku Objective C nie wymaga słowników ma wartość, więc istnieje scenariuszy, w której wartość nie może być ustawiona.

Aby to zrobić, należy wykonać kilka czynności:

* Utwórz klasę jednoznacznie tego podklasy [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) i zapewnia różne ustawiających i pobierających dla każdej właściwości.
* Zadeklaruj przeciążenia dla metody, biorąc `NSDictionary` zostały jednoznacznie nowej wersji.

Utwórz silnie typizowanej klasy albo ręcznie lub za pomocą generatora wykonywać pracę za Ciebie.  Najpierw zapoznać się jak zrobić to ręcznie, aby zrozumieć, co się dzieje, a następnie automatyczne rozwiązanie.

Należy utworzyć plik obsługi dla tego, nie przechodzi w dany kontrakt interfejsu API.  Jest to musiałaby zapisu można utworzyć klasy XyzOptions:

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

Następnie należy Podaj metodę otoki powierzchni interfejsu API wysokiego poziomu na interfejs API niskiego poziomu.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Jeśli Twój interfejs API nie musi zostać zastąpione, można bezpiecznie ukryć interfejsu API programu NSDictionary przy użyciu [wewnętrzne](~/cross-platform/macios/binding/binding-types-reference.md) atrybutu.

Jak widać, używamy `[Wrap]` atrybutu udostępnienie nowego punktu wejścia interfejsu API i firma Microsoft za pomocą naszych silnie typizowanej klasy XyzOptions powierzchni.
Metoda otoki umożliwia również null do przekazania.

Teraz, firma Microsoft nie mogą zawierać jest to, gdzie `XyzOptionsKeys` pochodzeniu wartości.  Klucze powierzchni interfejsu API w klasie statycznej, takich jak XyzOptionsKeys, jak to będzie zwykle grupy:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

Daj nam Spójrz na automatyczną obsługę tworzenia jednoznacznie słowników.  Dzięki temu można uniknąć dużo standardowej, a bezpośrednio w umowy interfejsu API, zamiast pliku zewnętrznego, można zdefiniować słownika.

Aby utworzyć silnie typizowanego słownika, wprowadzenie interfejsu w interfejsie API i dekoracji go przy użyciu [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md) atrybutu.  Informuje generatora, należy utworzyć klasę z taką samą nazwę jak interfejsu, który będzie dziedziczyć `DictionaryContainer` i zapewni silne typizowane metody dostępu dla niego.

`StrongDictionary` Atrybut przyjmuje jeden parametr, który jest nazwa klasy statycznej, która zawiera klucze słownika.  Następnie każdej właściwości interfejsu staną się silnie typizowane metody dostępu.  Domyślnie kod użyje nazwy właściwości wraz z sufiksem "Key" w klasie statycznej można utworzyć metody dostępu.

Oznacza to, że tworzenia Twojej silnie typizowane metody dostępu nie wymaga już pliku zewnętrznego, ani ręcznie tworzyć ustawiających dla każdej właściwości i metody pobierające, ani konieczności wyszukania klucze ręcznie samodzielnie.

Jest to, jak będzie wyglądać całego wiązania:

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

W razie potrzeby można odwoływać się w sieci `XyzOption` członków innego pola (to znaczy nie nazwę właściwości wraz z sufiksem `Key`), można dekoracji właściwości o `Export` atrybutu o nazwie, która ma być używany.

 <a name="Type_mappings" />

## <a name="type-mappings"></a>Mapowanie typu

W tej sekcji omówiono sposób Objective-C typy są mapowane na typach C#.

<a name="Simple_Types" />

### <a name="simple-types"></a>Typy proste

W poniższej tabeli przedstawiono, jak powinna mapowania typów z języka Objective C i CocoaTouch world World Xamarin.iOS:

<table border="1" cellpadding="1" cellspacing="1" width="80%">
      <caption> Mapowanie typu </caption>
      <tbody>
        <tr>
          <td>
Nazwa typu Objective-C </td>
          <td>
Typ ujednoliconego interfejsu API platformy Xamarin.iOS </td>
        </tr>
        <tr>
          <td>
BOOL, GLboolean </td>
          <td>
bool </td>
        </tr>
        <tr>
          <td>
NSInteger </td>
          <td>
nint </td>
        </tr>
        <tr>
          <td>
NSUInteger </td>
          <td>
nuint </td>
        </tr>
        <tr>
          <td>
CFTimeInterval / NSTimeInterval </td>
          <td>
double </td>
        </tr>
        <tr>
          <td>
NSString (<a href="~/ios/internals/api-design/nsstring.md">więcej informacji na temat wiązania NSString</a>) </td>
          <td>
string </td>
        </tr>
        <tr>
          <td>
char * </td>
          <td>
            <a href="~/cross-platform/macios/binding/binding-types-reference.md#plainstring"> [PlainString] </a> ciągu </td>
        </tr>
        <tr>
          <td>
CGRect </td>
          <td>
CGRect </td>
        </tr>
        <tr>
          <td>
CGPoint </td>
          <td>
CGPoint </td>
        </tr>
        <tr>
          <td>
CGSize </td>
          <td>
CGSize </td>
        </tr>
        <tr>
          <td>
CGFloat, GLfloat </td>
          <td>
nfloat </td>
        </tr>
        <tr>
          <td>
Typy CoreFoundation (CF *) </td>
          <td>
CoreFoundation.CF* </td>
        </tr>
        <tr>
          <td>
GLint </td>
          <td>
nint </td>
        </tr>
        <tr>
          <td>
GLfloat </td>
          <td>
nfloat </td>
        </tr>
        <tr>
          <td>
Typy Foundation (NS *) </td>
          <td>
Foundation.NS* </td>
        </tr>
        <tr>
          <td>
identyfikator </td>
          <td>
Foundation.NSObject </td>
        </tr>
        <tr>
          <td>
NSGlyph </td>
          <td>
nint </td>
        </tr>
        <tr>
          <td>
NSSize </td>
          <td>
CGSize </td>
        </tr>
        <tr>
          <td>
NSTextAlignment </td>
          <td>
UITextAlignment </td>
        </tr>
        <tr>
          <td>
SEL </td>
          <td>
ObjCRuntime.Selector </td>
        </tr>
        <tr>
          <td>
dispatch_queue_t </td>
          <td>
CoreFoundation.DispatchQueue </td>
        </tr>
        <tr>
          <td>
CFTimeInterval </td>
          <td>
double </td>
        </tr>
        <tr>
          <td>
CFIndex </td>
          <td>
nint </td>
        </tr>
        <tr>
          <td>
NSGlyph </td>
          <td>
nuint </td>
        </tr>
      </tbody>
    </table>

 <a name="Arrays" />

### <a name="arrays"></a>Tablice

Środowisko uruchomieniowe platformy Xamarin.iOS automatycznie zajmuje się konwertowanie tablic C# do `NSArrays` i wykonywania konwersji, tak na przykład urojony metody Objective-C, która zwraca `NSArray` z `UIViews`:

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

Będzie używać silnie typizowaną tablicę C#, ponieważ dzięki temu IDE w celu zapewnienia prawidłowego kodu zakończenia z rzeczywisty typ bez wymuszania użytkownika do odgadnięcia lub wyszukania dokumentacji, aby dowiedzieć się, rzeczywisty typ obiektów zawartych w tablicy.

W przypadkach, gdy nie można śledzić w dół rzeczywisty typ najdalszych pochodnych zawartych w tablicy można użyć `NSObject []` jako wartości zwracane.

 <a name="Selectors" />

### <a name="selectors"></a>Selektorów.

Selektory są wyświetlane w interfejsie API języka Objective-C jako specjalny typ "Zaznaczenia". Podczas tworzenia wiązania selektora, czy mapowanie typu `ObjCRuntime.Selector`.  Zwykle selektory są widoczne w interfejsie API zarówno obiektu, obiekt docelowy i selektora do wywołania w obiekcie docelowym. Zapewnianie oba te zasadniczo odpowiada delegata C#: coś, który hermetyzuje zarówno metoda do wywołania, jak i obiektów można wywołać metody w.

To jest powiązanie wygląda następująco:

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

I jest to, jak metoda wykorzystania w aplikacji:

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

Aby powiązanie wrażeń dla deweloperów języka C#, zazwyczaj będzie dostarczana metody pobierającej `NSAction` parametr, który umożliwia delegatów C# i wyrażeń lambda, można użyć zamiast `Target+Selector`. W tym celu będzie zazwyczaj ukryć metodę "SetTarget" przez flagowanie go z atrybutem "Internal" i następnie może narazić nowej metody pomocnika, jak to:

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

Więc kodu użytkownika może być zapisany jako:

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

Gdy są powiązanie metody pobierającej `NSString`, można zastąpić, że C# typu ciąg, zwracać na typy i parametry.

Tylko wówczas, gdy ma być `NSString` jest bezpośrednio, gdy ten ciąg jest używany jako tokenu. Aby uzyskać więcej informacji na temat ciągów i `NSString`, przeczytaj [projekt interfejsu API w NSString](~/ios/internals/api-design/nsstring.md) dokumentu.

W rzadkich przypadkach interfejs API może narazić ciąg notacji języka C (`char *`) zamiast ciągu języka Objective-C (`NSString *`). W takich przypadkach może dodawać adnotacje do parametru z [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) atrybutu.

 <a name="outref_parameters" />

### <a name="outref-parameters"></a>out / ref parametrów

Niektóre funkcje interfejsu API zwracają wartości w ich parametrów lub przekazania parametrów przez odwołanie.

Zwykle podpis wygląda następująco:

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

W pierwszym przykładzie pokazano, wspólnej idiom Objective-C do zwrócenia kody błędów, wskaźnik do `NSError` wskaźnika jest przekazywana, a po powrocie wartość jest ustawiona.   Druga metoda pokazuje, jak metoda Objective-C może pobrać obiektu i zmodyfikuj jego zawartość.   Będzie to przekazywanym przez odwołanie, a nie wartość czysty danych wyjściowych.

Twoje powiązanie będzie wyglądać następująco:

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

 <a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>Atrybuty zarządzania pamięci

Jeśli używasz `[Export]` atrybutu i przekazywane dane, które zostaną zachowane w nazwie metody, można określić semantykę argumentu, przekazując go jako drugiego parametru, na przykład:

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

Flaga będzie powyższych wartości jako mający semantyki "Zachowaj". Semantyka dostępne są następujące:

-  Przypisz:
-  Copy:
-  Zachowaj:

 <a name="Style_Guidelines" />

### <a name="style-guidelines"></a>Styl wskazówki

 <a name="Using_[Internal]" />

#### <a name="using-internal"></a>Za pomocą [wewnętrznych]

Można użyć [[wewnętrznego]](~/cross-platform/macios/binding/binding-types-reference.md) atrybutu, aby ukryć metodę z publiczny interfejs API. Można to zrobić w przypadku gdy narażonych interfejs API jest zbyt niski poziom i chcesz wysokiego poziomu implementacji w osobnym pliku na podstawie tej metody.

Umożliwia także to po uruchomieniu do ograniczenia w generatorze powiązanie, na przykład niektóre zaawansowane scenariusze może narazić typy, które nie są powiązane i chcesz powiązać w sposób własne i ma być zawijany samodzielnie tych typów w sposób własne.

 <a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>Programy obsługi zdarzeń i wywołania zwrotne

Klasy języka Objective-C zwykle emisji powiadomienia lub żądają informacji wysyłając wiadomość na klasie delegata (delegat Objective-C).

Ten model, podczas gdy w pełni obsługiwane i udostępniane przez Xamarin.iOS czasami można skomplikowane. Xamarin.iOS przedstawia wzorzec zdarzeń C# i systemu wywołania zwrotnego metody w klasie, która może być używana w takich sytuacjach. Dzięki temu kod takie do uruchomienia:

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

Generator powiązanie jest w stanie skrócenie czasu wpisywanie wymagane mapować wzorzec Objective-C do wzorca C#.

Uruchamianie z Xamarin.iOS 1.4 będzie można także skonfigurować generator do tworzenia powiązań dla określonych delegatów Objective-C i uwidocznić delegata jako C# zdarzeń i właściwości w typie hosta.

Istnieją dwie klasy związane z tego procesu, klasy hosta, który jest obecnie emituje zdarzeń i wysyła ich na `Delegate` lub `WeakDelegate` i klasa rzeczywistego obiektu delegowanego.

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

-  W klasie hosta, należy dodać do Twojego `[BaseType]` deklaracji typu działający jako swojego delegata i C# nazwę, która jest widoczne. W naszym przykładzie powyżej te są "typeof (MyClassDelegate)" i "WeakDelegate" odpowiednio.
-  W klasie delegata na każda metoda ma więcej niż dwóch parametrów należy określić typ, który ma być używany dla automatycznie wygenerowana klasa EventArgs.

Generator powiązania nie jest ograniczona do zawijania tylko pojedyncze zdarzenie docelowego, możliwe jest tego delegata niektórych klas języka Objective-C do wysyłania komunikatów do więcej niż jeden, więc będzie musiał podać tablic do obsługi tej instalacji. Większość ustawień nie będą potrzebne, ale generatora jest gotowe do obsługi tych przypadkach.

Wynikowy kod będzie:

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

`EventArgs` Służy do określania nazwy `EventArgs` klasy do wygenerowania. Należy używać po jednym dla każdego podpisu (w tym przykładzie `EventArgs` będzie zawierać właściwość "Z" nint typu).

Z definicjami powyżej generatora utworzy następujące zdarzenie w wygenerowanym MyClass:

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

Teraz można więc użyć kodu następująco:

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

Wywołania zwrotne, są tak samo jak wywołań zdarzeń, różnica jest to, że zamiast wielu subskrybentów potencjalnych (na przykład wiele metod można Podłączanie do zdarzeń "kliknięto element" lub "Pobierz Zakończono" zdarzenie) wywołań zwrotnych, może mieć tylko jednego abonenta.

Proces jest taki sam, jedyną różnicą jest to, że zamiast udostępnianie nazwę klasy EventArgs, który zostanie wygenerowany, EventArgs faktycznie jest używany do nazywania wynikowa Nazwa delegata C#.

Jeśli metoda w klasie obiektów delegowanych zwróci wartość, generator powiązanie przypisze to do metody delegata w klasie nadrzędnej zamiast zdarzenia. W takich sytuacjach należy podać wartość domyślną, która ma zostać zwrócony przez metodę, jeśli użytkownik nie Podłączanie delegata. Można to zrobić przy użyciu `[DefaultValue]` lub `[DefaultValueFromArgument]` atrybutów.

DefaultValue będzie kodowania zwracanej wartości, podczas gdy `[DefaultValueFromArgument]` służy do określania, które argument wejściowy zostaną zwrócone.

 <a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>Wyliczeń i typy podstawowe

Można także odwoływać wyliczenia lub typów podstawowych, które nie są bezpośrednio obsługiwane przez system definicji interfejsu btouch. W tym celu należy umieścić Twoje wyliczeń i typy podstawowe w osobnym pliku i uwzględniać to w ramach dodatkowe pliki, które świadczą btouch.

 <a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>Łączenie zależności

Są wiązane interfejsów API, które nie są częścią aplikacji, należy się upewnić, że pliku wykonywalnego jest połączony z tych bibliotek.

Należy poinformować Xamarin.iOS jak połączyć bibliotek, można to zrobić przez zmianę konfiguracji kompilacji, aby wywołać polecenie mtouch z niektórych argumentów dodatkowe kompilacji, określające, jak połączyć się z bibliotekami nowe, za pomocą "-gcc_flags" opcji, a następnie w ciągu w cudzysłowie, który zawiera wszystkich dodatkowych bibliotek, które są wymagane dla programu, jak to:

```csharp
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

Powyższy przykład połączy `libMyLibrary.a`, `libSystemLibrary.dylib` i `CFNetwork` framework biblioteki w końcowym pliku wykonywalnego.

Lub możesz korzystać z poziomu zestawu `LinkWithAttribute`, który można osadzić w plikach kontrakt (takie jak `AssemblyInfo.cs`). Jeśli używasz `LinkWithAttribute`, musisz mieć natywnej biblioteki dostępny w czasie wprowadzania powiązania, ponieważ spowoduje to osadzić natywnej biblioteki z aplikacji. Na przykład:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

Może być zastanawiasz się, dlaczego potrzebujesz polecenia "force_load" i że flagą ObjC — mimo że kompiluje kod w, nie zachowuje metadane wymagane do obsługi kategorii (eliminacji martwy kod kompilatora/konsolidatora taśmy), które należy to z faktu w czasie wykonywania dla platformy Xamarin.iOS.

 <a name="Assisted_References" />

## <a name="assisted-references"></a>Asystowaną odwołań

Niektóre obiekty przejściowy, takich jak arkusze akcji i pola alertu są skomplikowane do śledzenia dla deweloperów i generator powiązanie pomaga trochę tutaj.

Na przykład, jeśli klasa, która wykazało wiadomość, a następnie wygenerowany "Gotowe" zdarzenia, tradycyjny sposób obsługi to będzie następująca:

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

W powyższym scenariuszu deweloper musi zachować odwołania do obiektu siebie i albo przeciek lub aktywnie wyczyść odwołania pola na jego własnej.  Powiązanie kodu generatora obsługuje śledzenia odwołania dla Ciebie i wyczyść go po wywołaniu metody specjalnej powyższy kod więc będzie:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

Zwróć uwagę, jak nie jest już muszą być zmiennej w wystąpieniu, że działa on za pomocą zmiennych lokalnych i czy nie jest konieczne usunięcie odwołania, gdy obiekt zaniku.

Aby móc korzystać z tego, klasy powinny mieć właściwość zdarzenia w `[BaseType]` deklaracji, a także `KeepUntilRef` zmienna jest ustawiona na nazwę metody, która jest wywołana po zakończeniu pracy, podobny do tego obiektu:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

 <a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>Dziedziczenie protokołów

Począwszy od platformy Xamarin.iOS wersja 3.2 lub, firma Microsoft obsługuje dziedziczenia z protokołów, które zostały oznaczone `[Model]` właściwości. Jest to przydatne w pewnych wzorców interfejsu API, takich jak w `MapKit` gdzie `MKOverlay` protokołu, dziedziczy `MKAnnotation` protokołu i jest stosowane przez liczbę klas, które dziedziczą z `NSObject`.

W przeszłości firma Microsoft wymaga kopiowanie co implementacji protokołu, ale w tych przypadkach firma Microsoft może już `MKShape` klasa dziedziczy `MKOverlay` protokołu i wygeneruje wymaganych metod automatycznie.

### <a name="related-links"></a>Linki pokrewne

- [Przykładowe powiązania](https://developer.xamarin.com/samples/BindingSample/)
