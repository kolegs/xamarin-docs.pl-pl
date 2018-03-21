---
title: "Przewodnik odwołań dla typów powiązania"
ms.topic: article
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 0d58a8ab15a7b2d598aa8fd45a9b4d0c3d9e440b
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2018
---
# <a name="binding-types-reference-guide"></a>Przewodnik odwołań dla typów powiązania

W tym dokumencie opisano listę atrybutów, które służy do dodawania adnotacji kontrakt interfejsu API plików do powiązania i wygenerować kodu

Kontrakty Xamarin.iOS i interfejsu API Xamarin.Mac są zapisywane w języku C# przede wszystkim jako definicje interfejsu, które definiują sposób że kod języka Objective C jest udostępniane na język C#. Proces obejmuje zarówno deklaracjach interfejsu i definicje typu podstawowego, które mogą wymagać kontrakt interfejsu API. Aby obejrzeć wprowadzenie do typów powiązanie, zobacz nasze Przewodnik uzupełniający po [powiązanie bibliotek języka Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md).

## <a name="type-definitions"></a>Definicje typów

Składnia:

```csharp
[BaseType (typeof (BTYPE))
interface MyType [: Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

Każdego interfejsu w definicji kontrakt zawierający [ `[BaseType]` ](#BaseTypeAttribute) atrybut, który deklaruje typ podstawowy dla wygenerowanego obiektu. W powyższym deklaracji `MyType` klasy typ języka C# zostanie wygenerowany, że wywoływana wiązania na typ Objective-C `MyType`.

Jeśli określisz żadnych typów po typename (w powyższym przykładzie `Protocol1` i `Protocol2`) przy użyciu składni dziedziczenia interfejsu zawartość tych interfejsów będzie wbudowanego tak, jakby były częścią kontraktu dla `MyType`.
Sposób, który polega na powierzchni Xamarin.iOS, że typem przyjmie protokół ze śródwierszowaniem wszystkie metody i właściwości, które zostały zgłoszone w protokole do samego typu.

Poniżej przedstawiono sposób deklaracja Objective-C `UITextField` byłoby zdefiniowane w kontrakcie Xamarin.iOS:

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

Może być zapisany jako kontrakt interfejsu API języka C#:

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

Można kontrolować wielu innych aspektów generowania kodu przy zastosowaniu innych atrybutów do interfejsu, jak również konfigurowanie [ `[BaseType]` ](#BaseTypeAttribute) atrybutu.


### <a name="generating-events"></a>Generowanie zdarzeń

Jedna funkcja projektu platformy Xamarin.iOS i Xamarin.Mac interfejsu API jest firma Microsoft mapowania klasy obiektów delegowanych Objective-C C# zdarzeń i wywołania zwrotne. Użytkownicy mogą wybrać w zasadzie poszczególnych wystąpień, czy chcą, aby przyjąć wzorzec programowania Objective-C, przez przypisanie do właściwości, takie jak `Delegate` wystąpienia klasy, która implementuje różnych metod, które spowodowałoby wywołanie środowiska uruchomieniowego języka Objective-C lub przez Wybieranie języka C# — styl zdarzeń i właściwości.

Poinformuj nas, zobacz przykład sposobu korzystania z modelu Objective-C:

```csharp
bool MakeDecision ()
{
    return true;
}

void Setup ()
{
     var scrollView = new UIScrollView (myRect);
     scrollView.Delegate = new MyScrollViewDelegate ();
     ...
}

class MyScrollViewDelegate : UIScrollViewDelegate {
    public override void Scrolled (UIScrollView scrollView)
    {
        Console.WriteLine ("Scrolled");
    }

    public override bool ShouldScrollToTop (UIScrollView scrollView)
    {
        return MakeDecision ();
    }
}
```

W powyższym przykładzie widać, że Wybraliśmy zastąpić dwie metody, która powiadomienie, że zdarzenie przewijania miało miejsce, a drugi który jest wywołaniem zwrotnym, które powinien zwracać wartość logiczną poinstruowanie `scrollView` czy powinna przewiń do z góry lub nie.

Model C# pozwala użytkownikowi biblioteki nasłuchiwanie na powiadomienia za pomocą składni zdarzenia C# lub składni właściwości Podłączanie wywołania zwrotne, które mogą zwracać wartości.

Jest to, jak wygląda za pomocą wyrażenia lambda kodu C# dla tej samej funkcji:

```csharp
void Setup ()
{
    var scrollview = new UIScrollView (myRect);
    // Event connection, use += and multiple events can be connected
    scrollView.Scrolled += (sender, eventArgs) { Console.WriteLine ("Scrolled"); }

    // Property connection, use = only a single callback can be used
    scrollView.ShouldScrollToTop = (sv) => MakeDecision ();
}
```

Ponieważ zdarzenia nie zwracają wartości (mają zwrócony typ void) można połączyć wiele kopii. `ShouldScrollToTop` Nie jest zdarzeniem, zamiast tego jest właściwością typu `UIScrollViewCondition` który ma podpis:

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

Zwraca `bool` wartość, w tym przypadku składni lambda pozwala tylko zwrócić wartości z `MakeDecision` funkcji.

Generator powiązanie obsługuje generowanie zdarzeń i właściwości, które klasy, jak połączyć `UIScrollView` z jego `UIScrollViewDelegate` (również wywołać te klasy modelu), jest to realizowane przez dodawanie adnotacji do Twojej [ `[BaseType]` ](#BaseTypeAttribute) definicji z `Events` i `Delegates` parametrów (opisanych poniżej). Oprócz Dodawanie adnotacji do [ `[BaseType]` ](#BaseTypeAttribute) z tych parametrów konieczne jest poinformowanie generatora kilka więcej składników.

Dla zdarzeń, które przyjmują więcej niż jeden parametr (w języku Objective-C Konwencji jest pierwszym parametrem w klasie delegata wystąpienie obiektu nadawcy) należy podać nazwę, którą chcesz dla wygenerowanej `EventArgs` klasy jako. Jest to zrobić za pomocą [ `[EventArgs]` ](#EventArgsAttribute) atrybutu w deklaracji metody w klasie modelu. Na przykład:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

Wygeneruje powyżej deklaracji `UIImagePickerImagePickedEventArgs` klasą pochodzącą z `EventArgs` pakiety obydwu tych parametrów i `UIImage` i `NSDictionary`. Generator daje to:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Następnie udostępnia następujące opcje w `UIImagePickerController` klasy:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

Metod modelu, które zwracają wartości są powiązane inaczej. Te wymagają zarówno nazwy dla wygenerowanego C# delegat (sygnatura metody), a także wartość domyślną do zwrócenia w przypadku, gdy użytkownik nie ma implementacji siebie. Na przykład `ShouldScrollToTop` definicji jest to:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

Powyższe utworzy `UIScrollViewCondition` delegować z podpisem wyświetleniem powyżej, a jeśli użytkownik nie poda implementację, zwracana wartość musi być true.

Oprócz [ `[DefaultValue]` ](#DefaultValueAttribute) atrybutu, można również użyć [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute) atrybut, który kieruje generatora, aby zwrócić wartość określony parametr w wywołaniu lub [ `[NoDefaultValue]` ](#NoDefaultValueAttribute) parametr, który nakazuje generatora, że nie ma żadnej wartości domyślnej.

<a name="BaseTypeAttribute" />

### <a name="basetypeattribute"></a>BaseTypeAttribute

Składnia:

```csharp
public class BaseTypeAttribute : Attribute {
        public BaseTypeAttribute (Type t);

        // Properties
        public Type BaseType { get; set; }
        public string Name { get; set; }
        public Type [] Events { get; set; }
        public string [] Delegates { get; set; }
        public string KeepRefUntil { get; set; }
}
```

#### <a name="basetypename"></a>BaseType.Name

Możesz użyć `Name` dla właściwości Nazwa, który zostanie powiązany tego typu w środowisku Objective-C formantu. Służy to zwykle nazwę typu C#, który jest zgodne z wytycznymi projektowania platformy .NET Framework, ale który mapuje nazwę w języku Objective-C, która nie jest zgodna z Konwencji.

Przykład w przypadku następujących możemy mapy Objective-C `NSURLConnection` typ `NSUrlConnection`, zgodnie z wytycznymi projektowania programu .NET Framework, użyj "Url" zamiast "URL":

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

Podana nazwa jest używana jako wartość dla wygenerowanej `[Register]` atrybutu w powiązaniu. Jeśli `Name` nie zostanie określony, krótka nazwa typu jest używany jako wartość `[Register]` atrybutu w wygenerowanych danych wyjściowych.

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType.Events i BaseType.Delegates

Te właściwości są używane do generowania języka C# — styl zdarzenia w wygenerowane klasy. Są one używane do łączenia danej klasy wraz ze swoją klasą delegata Objective-C. Można napotkać w wielu przypadkach, gdy klasa korzysta klasa delegata do wysyłania powiadomień i zdarzeń. Na przykład `BarcodeScanner` byłyby dodatek `BardodeScannerDelegate` klasy. `BarcodeScanner` Klasy zwykle mają `Delegate` właściwość, którą należy przypisać wystąpienia `BarcodeScannerDelegate` do, podczas gdy ta działa, możesz chcieć udostępnić użytkownikom C# — takie jak styl interfejsu zdarzenia i w takim przypadku należy użyć `Events`i `Delegates` właściwości [ `[BaseType]` ](#BaseTypeAttribute) atrybutu.

Te właściwości są zawsze wartość ze sobą i musi mieć taką samą liczbę elementów i są synchronizowane. `Delegates` Tablica zawiera jeden ciąg dla każdego słabą kontrolą delegata, który ma być zawijany, i `Events` tablica zawiera jeden typ dla poszczególnych typów, które chcesz skojarzyć z nim.

```csharp
[BaseType (typeof (NSObject),
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIAccelerometerDelegate)})]
public interface UIAccelerometer {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIAccelerometerDelegate {
}
```


#### <a name="basetypekeeprefuntil"></a>BaseType.KeepRefUntil

Jeśli ten atrybut można zastosować podczas tworzenia nowego wystąpienia tej klasy, wystąpienie obiektu będą znajdować się wokół do metody odwołuje się `KeepRefUntil` została wywołana. Jest to przydatne poprawić użyteczność swoje interfejsy API, gdy nie ma użytkownika, aby zachować odwołanie do obiektu wokół do używania w kodzie. Wartość tej właściwości jest nazwa metody w `Delegate` klasy, to trzeba używać w połączeniu z `Events` i `Delegates` również właściwości.

Poniższym przykładzie pokazano, jak jest to używane przez `UIActionSheet` w Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject), KeepRefUntil="Dismissed")]
[BaseType (typeof (UIView),
           KeepRefUntil="Dismissed",
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIActionSheetDelegate)})]
public interface UIActionSheet {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIActionSheetDelegate {
    [Export ("actionSheet:didDismissWithButtonIndex:"), EventArgs ("UIButton")]
    void Dismissed (UIActionSheet actionSheet, nint buttonIndex);
}
```


### <a name="disabledefaultctorattribute"></a>DisableDefaultCtorAttribute

Jeśli ten atrybut jest stosowany do definicji interfejsu uniemożliwi generatora produkujących konstruktora domyślnego.

Należy użyć tego atrybutu, gdy będziesz potrzebować na można zainicjować jednego z innych konstruktorów w klasie obiektu.


### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

Jeśli ten atrybut jest stosowany do definicji interfejsu go jako prywatny zostanie Flaga konstruktora domyślnego. Oznacza to, że można nadal utworzyć wystąpienie tej klasy obiektu wewnętrznie z pliku rozszerzenie, ale go po prostu nie być dostępna dla użytkowników klasy.

<a name="CategoryAttribute" />

### <a name="categoryattribute"></a>CategoryAttribute

Użyj tego atrybutu w definicji typu powiązać kategorii Objective-C i ujawniać je jako metody rozszerzenia C# dublowanego sposób Objective-C dostęp do funkcji.

Kategorie są mechanizm Objective-C, używany do rozszerzania zestaw metod i właściwości dostępne w klasie.   W praktyce, są one używane do albo rozszerzyć funkcjonalność klasy podstawowej (na przykład `NSObject`) podczas konsolidowana określonej platformy (na przykład `UIKit`), co ich metod dostępne, ale tylko wtedy, gdy jest dołączane nowej struktury.   W niektórych przypadkach są one używane do organizowania funkcji w klasie według funkcji.   Są one podobne w duchu do metody rozszerzenia C#.

Jest to, jak kategoria będzie wyglądać w celu C:

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Powyższy przykład znajduje się w bibliotece, która będzie rozszerzać wystąpienia `UIView` z metodą `makeBackgroundRed`.

Aby powiązać te, można użyć [ `[Category]` ](#CategoryAttribute) atrybutu w definicji interfejsu.   Korzystając z [ `[Category]` ](#CategoryAttribute) atrybutu znaczenie [ `[BaseType]` ](#BaseTypeAttribute) atrybut zostanie zmieniony z używany do określenia klasy podstawowej, aby rozszerzyć, aby jest typ do rozszerzenia.

Poniżej przedstawiono sposób `UIView` rozszerzenia są powiązane i przekształcane w metodach rozszerzeń C#:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Powyższe utworzy `MyUIViewExtension` klasę, która zawiera `MakeBackgroundRed` — metoda rozszerzenia.   Oznacza to, że można teraz wywołać `MakeBackgroundRed` na dowolnym `UIView` podklasy, umożliwiając te same funkcje jak Objective-C.

W niektórych przypadkach można znaleźć **statycznych** elementów członkowskich w kategoriach, takich jak w poniższym przykładzie:

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

Prowadzi to do **niepoprawne** definicji interfejsu kategorii C#:

```csharp
[Category]
[BaseType (typeof (FooObject))]
interface FooObject_Extensions {

    // Incorrect Interface definition
    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Jest nieprawidłowy ponieważ do użycia `BoolMethod` rozszerzenie wymaga wystąpienia `FooObject` , ale są powiązanie ObjC **statycznych** rozszerzenia, jest efektem ze względu na fakt sposobu implementacji metody rozszerzenia C#.

Jedynym sposobem, aby użyć powyższych definicji jest ugly następującym kodem:

```csharp
(null as FooObject).BoolMethod (range);
```

Aby tego uniknąć zaleca się użycie śródwierszowej `BoolMethod` definicji wewnątrz `FooObject` samą definicję interfejsu, to będzie można wywołać to rozszerzenie, np. jest on przeznaczony `FooObject.BoolMethod (range)`.

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Firma Microsoft wyświetli ostrzeżenie (BI1117) zawsze, gdy okaże się [ `[Static]` ](#StaticAttribute) członek wewnątrz [ `[Category]` ](#CategoryAttribute) definicji. Czy na pewno chcesz mieć [ `[Static]` ](#StaticAttribute) elementów członkowskich w Twojej [ `[Category]` ](#CategoryAttribute) definicje ostrzeżenia mogą wyłączeniu przy użyciu `[Category (allowStaticMembers: true)]` lub dekoracji elementu członkowskiego lub [ `[Category]` ](#CategoryAttribute) definicji z interfejsu [ `[Internal]` ](#InternalAttribute).

<a name="StaticAttribute_Class" />

### <a name="staticattribute"></a>StaticAttribute

Jeśli ten atrybut jest stosowany do klasy, po prostu wygeneruje klasy statycznej, który nie pochodzi od `NSObject`, więc [ `[BaseType]` ](#BaseTypeAttribute) atrybut jest ignorowany. Klasy statyczne są używane do obsługi C zmiennych publicznych, które chcesz udostępnić.

Na przykład:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

Generuje klasy C# przy użyciu następujących interfejsu API:

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>Model protokołu/definicji

Modele są zazwyczaj używane przez implementację protokołu.
Różnią się one w tym środowiska uruchomieniowego tylko zarejestruje w języku Objective C metody, które faktycznie zostały zastąpione.
W przeciwnym razie metoda nie zostanie zarejestrowana.

To zwykle oznacza, że w przypadku podklasą klasy, które zostały oznaczone stanem `ModelAttribute`, nie należy wywołać metodę podstawową.   Wywołanie tej metody spowoduje zgłoszenie wyjątku, powinny implementować zachowanie całej na podklasa użytkownika dla dowolnej metody, których można zastąpić.

<a name="AbstractAttribute" />

### <a name="abstractattribute"></a>AbstractAttribute

Domyślnie elementów członkowskich, które są częścią protokołu nie są wymagane. Dzięki temu użytkownicy mogą utworzyć podklasy `Model` obiektu jedynie wyprowadzanie z klas w języku C# i zastępowanie metody ich interesują. Czasami kontrakt Objective-C wymaga, aby użytkownik udostępnia implementację dla tej metody (te są oznaczone `@required` dyrektywy w języku Objective-C). W takich przypadkach należy Flaga tych metod z `[Abstract]` atrybutu.

`[Abstract]` Atrybut można zastosować do metody lub właściwości oraz powoduje, że generatora, aby flaga wygenerowany element członkowski jako abstrakcyjny i klasy, która ma być klasą abstrakcyjną.

Poniżej jest pobierana z Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UITableViewDataSource {
    [Export ("tableView:numberOfRowsInSection:")]
    [Abstract]
    nint RowsInSection (UITableView tableView, nint section);
}
```

<a name="DefaultValueAttribute" />

### <a name="defaultvalueattribute"></a>DefaultValueAttribute

Określa domyślną wartość zwracaną przez metodę modelu, jeśli użytkownik nie ma metody dla metody określonej w obiekt modelu

Składnia:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

Na przykład w następującej klasy urojony delegowanego dla `Camera` klasy, firma Microsoft udostępnia `ShouldUploadToServer` który może być udostępniany jako właściwość na `Camera` klasy. Jeśli użytkownik `Camera` klasy nie jawnie ustawiona wartość lambda, który odpowiada wartość PRAWDA lub FAŁSZ, wartością domyślną jest zwracany w takim przypadku będzie false, wartość określoną w `DefaultValue` atrybutu:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

Jeżeli użytkownik ustawi obsługi w klasie urojony, ta wartość zostałyby one zignorowane:

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

Zobacz też: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute).

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

Składnia:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

Ten atrybut, gdy zostanie podane dla metody, która nie zwraca wartości na klasę modelu zleca generatora, aby zwrócić wartość określonego parametru, jeśli użytkownik nie dostarczył własnej metody lub lambda.

Przykład:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

W powyższych przypadków, gdy użytkownik `NSAnimation` klasy wybrany właściwości C# zdarzenia/i nie określono `NSAnimation.ComputeAnimationCurve` do metody lub lambda, wartość zwracana będzie wartość w parametrze postępu.

Zobacz też: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [`[DefaultValue]`](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

Czasami warto nie uwidacznia zdarzeń lub delegatem właściwości klasę modelu do klasy obsługującej, dodanie tego atrybutu zleca generatora, aby uniknąć generowania metody ozdobione go.

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);

    [Export ("imagePickerController:didFinishPickingImage:"), IgnoredInDelegate)] // No event generated for this method
    void FinishedPickingImage (UIImagePickerController picker, UIImage image);
}
```

### <a name="delegatenameattribute"></a>DelegateNameAttribute

Ten atrybut jest używany w modelu metody, które zwracają wartości, aby ustawić nazwę podpisu delegata do użycia.

Przykład:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

Z powyższej definicji generatora utworzy następujące oświadczenie publicznego:

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

Ten atrybut jest stosowane do umożliwienia generatora, aby zmienić nazwę właściwości wygenerowane klasy hosta. Czasami jest przydatne w przypadku nazwę metody klasy FooDelegate sens dla klasy obiektu delegowanego, ale może wydawać się dziwne w klasie hosta jako właściwość.

Również to naprawdę przydatne (i potrzebnych) po ma dwa lub więcej metod przeciążenia, które warto zachować o nazwie, jak w klasie FooDelegate, ale chcesz udostępnić je w klasie hosta o określonej nazwie lepiej.

Przykład:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

Z powyższej definicji generatora utworzy następujące oświadczenie publicznego w klasie hosta:

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

<a name="EventArgsAttribute" />

### <a name="eventargsattribute"></a>EventArgsAttribute

Dla zdarzeń, które przyjmują więcej niż jeden parametr (w języku Objective-C Konwencji jest pierwszym parametrem w klasie delegata wystąpienie obiektu nadawcy) należy podać nazwę, którą chcesz dla wygenerowanej klasy EventArgs się. Jest to zrobić za pomocą `[EventArgs]` atrybutu w deklaracji metody w Twojej `Model` klasy.

Na przykład:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

Wygeneruje powyżej deklaracji `UIImagePickerImagePickedEventArgs` klasy, która jest pochodną EventArgs i pakiety oba parametry `UIImage` i `NSDictionary`. Generator daje to:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Następnie udostępnia następujące opcje w `UIImagePickerController` klasy:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```


### <a name="eventnameattribute"></a>EventNameAttribute

Ten atrybut jest stosowane do umożliwienia generatora, aby zmienić nazwę zdarzenia lub właściwości wygenerowane klasy. Czasami jest przydatne w przypadku nazwę metody klasy modelu ma sens dla klasy modelu, ale może wydawać się dziwne klasy pochodzące jako zdarzenie lub właściwości.

Na przykład `UIWebView` używa następujący bit z `UIWebViewDelegate`:

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

Powyżej ujawnia `LoadingFinished` co metoda w `UIWebViewDelegate`, ale `LoadFinished` jako zdarzenie, aby przyłączyć się do w `UIWebView`:

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute" />

### <a name="modelattribute"></a>ModelAttribute

Po zastosowaniu `[Model]` atrybut do definicji typu w dany kontrakt interfejsu API, środowisko uruchomieniowe wygeneruje specjalne kod, który tylko będzie powierzchni wywołań do metod w klasie, jeśli użytkownik ma zastąpić metodę w klasie. Ten atrybut jest zwykle stosowany do wszystkich interfejsów API, które otaczają Objective-C klasa delegata.

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

Określa, że metody na model nie zawiera zwracaną wartość domyślną.

To polega na ze środowiskiem uruchomieniowym języka Objective-C odpowiada `false` na żądanie środowiska uruchomieniowego języka Objective-C do określenia, jeśli określony selektor jest zaimplementowana w tej klasie.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

Zobacz też: [ `[DefaultValue]` ](#DefaultValueAttribute), [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute" />

## <a name="protocols"></a>protokoły

Pojęcia języka Objective-C protokołu nie istnieje naprawdę w języku C#. Protokoły są podobne do interfejsów języka C#, jednak ich różnią się, że nie wszystkie metody i właściwości zadeklarowany w protokole musi być implementowana przez klasę, która przyjmuje go. Zamiast tego niektóre metody i właściwości są opcjonalne.

Niektóre protokoły są zazwyczaj używane jako klasy modeli, te powinny zostać powiązany za pomocą [ `[Model]` ](#ModelAttribute) atrybutu.

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
```

Począwszy od platformy Xamarin.iOS 7.0 nowych i ulepszonych protokołu, powiązania funkcji zostały włączone.  Żadnych definicji, który zawiera `[Protocol]` atrybutu powoduje wygenerowanie trzech klas pomocniczych, które znacząco poprawić sposób korzystać z protokołów:

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

**Implementacja klasy** zapewnia pełną klasa abstrakcyjna, można zastąpić poszczególnych metod i Pobierz pełny typ bezpieczeństwa. Ale z powodu C# nie obsługuje dziedziczenie wielokrotne, istnieją scenariusze, w którym możesz wymagają różnych klasy podstawowej, ale mimo to chcesz implementować interfejsu.

Jest to, gdy wygenerowany **definicji interfejsu** polega na.  Jest to interfejs, który zawiera wszystkie wymagane metody z protokołu.  Dzięki temu deweloperzy, których chcesz wdrożyć jedynie implementować interfejs użytkownika protokołu.  Środowisko uruchomieniowe automatycznie zarejestruje typ jako przyjmowanie protokołu.

Należy zauważyć, że interfejs tylko wymieniono wymagane metody a ujawniać metod opcjonalne.   Oznacza to uzyska pełny typ sprawdzania pod kątem wymaganych metod klasy, które przyjmują protokołu, ale ma odwołać się do wpisywania słabe (ręcznie przy użyciu atrybutów eksportu dopasowanie i podpis) dla metod protokołu opcjonalne.

Aby uprościć jego użycie interfejsu API, który używa protokołów, narzędzie powiązanie również utworzy klasę — metoda rozszerzenia, która udostępnia wszystkie metody opcjonalne.   Oznacza to, że tak długo, jak zużywają interfejsu API, można traktować jako zawierający wszystkie metody protokołów.

Jeśli chcesz używać protokołu definicji interfejsu API, musisz zapisać szkielet pustych interfejsów w definicji interfejsu API.   Jeśli chcesz użyć Mój_protokół w interfejsie API, będzie potrzebny w tym celu:

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

### <a name="adopting-protocol-generated-interfaces"></a>Przyjmowanie generowanych przez protokół interfejsów

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

### <a name="protocol-inlining"></a>Protokół ze śródwierszowaniem

Gdy powiąże istniejących typów języka Objective-C, które zostały zgłoszone jako przyjmowanie protokół można wbudowanego protokołu bezpośrednio. Aby to zrobić, tylko zadeklarować sieci protokołu jako interfejs bez żadnej [ `[BaseType]` ](#BaseTypeAttribute) lista protokołu na liście interfejsach podstawowych dla wybranego interfejsu i atrybutów.

Przykład:

```csharp
interface SpeakProtocol {
    [Export ("say:")]
    void Say (string msg);
}

[BaseType (typeof (NSObject))]
interface Robot : SpeakProtocol {
    [Export ("awake")]
    bool Awake { get; set; }
}
```


## <a name="member-definitions"></a>Definicje elementu członkowskiego

Atrybuty w tej sekcji są stosowane do poszczególnych elementów członkowskich typu: właściwości i deklaracje metody.


### <a name="alignattribute"></a>AlignAttribute

Można określić wartość wyrównania dla zwracanych typów właściwości. Wskaźniki do adresów, które muszą być wyrównane na granicach niektórych podjąć pewne właściwości (w Xamarin.iOS się tak zdarzyć na przykład niektóre `GLKBaseEffect` wyrównane właściwości, które muszą być 16 bajtów). Ta właściwość służy do dekoracji metoda pobierająca i użyj wartość wyrównania. To jest zwykle używana z `OpenTK.Vector4` i `OpenTK.Matrix4` typy po zintegrowaniu z interfejsów API języka Objective-C.

Przykład:

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```


### <a name="appearanceattribute"></a>AppearanceAttribute

`[Appearance]` Atrybut jest ograniczona do systemu iOS 5, gdzie wprowadzono Menedżera wyglądu.

`[Appearance]` Atrybut można stosować do dowolnej metody lub właściwości, które udział w `UIAppearance` framework. Jeśli ten atrybut jest stosowany do metody lub właściwości w klasie, kierujące generatora powiązanie, aby utworzyć klasę jednoznacznie wygląd, która służy do określania stylu wszystkie wystąpienia tej klasy lub wystąpienia, spełniających określone kryteria.

Przykład:

```csharp
public interface UIToolbar {
    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);
}
```

Powyższe będzie generować następujący kod w UIToolbar:

```csharp
public partial class UIToolbar {
    public partial class UIToolbarAppearance : UIView.UIViewAppearance {
        public virtual void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);
        public virtual UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics)
    }
    public static new UIToolbarAppearance Appearance { get; }
    public static new UIToolbarAppearance AppearanceWhenContainedIn (params Type [] containers);
}
```

### <a name="autoreleaseattribute-xamarinios-54"></a>AutoReleaseAttribute (Xamarin.iOS 5.4)

Użyj `[AutoReleaseAttribute]` na metody i właściwości powodującą otoczenie wywołania metody do metody w `NSAutoReleasePool`.

W języku Objective C jest niektórych metod, które zwracają wartości, które są dodawane do domyślnej `NSAutoReleasePool`. Domyślnie te przejdzie z wątku `NSAutoReleasePool`, ponieważ Xamarin.iOS zachowanie odwołania do obiektów tak długo, jak znajduje się obiekt zarządzany, nie może być utrzymywać dodatkowe odwołania `NSAutoReleasePool` którego tylko pobrać opróżnione do Twojej wątku Zwraca sterowania do następnego wątku lub wróć do głównego pętli.

Ten atrybut jest stosowany na przykład na duże właściwości (na przykład `UIImage.FromFile`), która zwracałaby obiektów, które zostały dodane do domyślnej `NSAutoReleasePool`. Bez tego atrybutu będzie zachowane obrazy tak długo, jak z wątku nie zwrócił formantu do głównego pętli. Uf Twojego wątek został jakiegoś downloader tła, który jest zawsze aktywna, a oczekiwania pracy, obrazy czy nigdy nie zwolnione.

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

`[ForcedTypeAttribute]` Jest używany w celu wymuszenia utworzenia typu zarządzanego, nawet wtedy, gdy zwrócony obiekt niezarządzane jest niezgodny z typem opisanym w definicji powiązania.

Jest to przydatne, jeśli typ opisanego w nagłówku nie są zgodne zwrócony typ metody natywnej, na przykład skorzystać z następującej definicji Objective-C z `NSURLSession`:

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

Wyraźnie stwierdza, że zwróci `NSURLSessionDownloadTask` wystąpienia, ale jeszcze jej **zwraca** `NSURLSessionTask`, która jest superklasą i dlatego nie można przekonwertować na typ `NSURLSessionDownloadTask`. Ponieważ pracujemy w kontekście bezpieczne `InvalidCastException` nastąpi.

Do wykonania opis nagłówka i uniknąć `InvalidCastException`, `[ForcedTypeAttribute]` jest używany.

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

`[ForcedTypeAttribute]` Również przyjmuje wartość logiczną o nazwie `Owns` czyli `false` domyślnie `[ForcedType (owns: true)]`. Właścicielem parametru jest używana na potrzeby [zasad własność](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html) dla **Core Foundation** obiektów.

`[ForcedTypeAttribute]` Jest prawidłowa tylko dla parametrów, właściwości i wartości zwracanej.

<a name="BindAsAttribute" />

### <a name="bindasattribute"></a>BindAsAttribute

`[BindAsAttribute]` Umożliwia powiązanie `NSNumber`, `NSValue` i `NSString`(Typy wyliczeniowe) do dokładniejsze typów C#. Ten atrybut służy do tworzenia lepsze, bardziej precyzyjne interfejs API .NET za pośrednictwem natywnego interfejsu API.

Można dekoracji metody (dla wartości zwracanej), parametrów i właściwości, do których `BindAs`. Jedynym ograniczeniem jest to, że elementów członkowskich **nie mogą** znajdować się wewnątrz `[Protocol]` lub [ `[Model]` ](#ModelAttribute) interfejsu.

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

Bieżący hermetyzacji obsługiwane typy to:

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

Obsługiwane są następujące typy danych języka C# możliwość hermetyzacji z/do `NSValue`:

* CGAffineTransform
* NSRange
* CGVector
* SCNMatrix4
* CLLocationCoordinate2D
* SCNVector3
* SCNVector4
* CGPoint / PointF
* CGRect / RectangleF
* CGSize / SizeF
* UIEdgeInsets
* UIOffset
* MKCoordinateSpan
* CMTimeRange
* CMTime
* CMTimeMapping
* CATransform3D

#### <a name="nsnumber"></a>NSNumber

Obsługiwane są następujące typy danych języka C# możliwość hermetyzacji z/do `NSNumber`:

* bool
* byte
* double
* float
* short
* int
* long
* sbyte
* ushort
* uint
* ulong
* nfloat
* nint
* nuint
* Wyliczenia

#### <a name="nsstring"></a>NSString

[`[BindAs]`](#BindAsAttribute) działa w powiązaniu z [wyliczenia obsługiwanej przez stałą NSString](#enum-attributes) dzięki czemu można tworzyć lepsze interfejs API .NET, na przykład:

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

Czy dane wyjściowe:

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

Firma Microsoft będzie obsługiwać `enum`  <->  `NSString` konwersji tylko wtedy, gdy typ wyliczenia podana [ `[BindAs]` ](#BindAsAttribute) jest [przez stałą NSString](#enum-attributes).

#### <a name="arrays"></a>Tablice

[`[BindAs]`](#BindAsAttribute) obsługuje także tablice dowolnego z obsługiwanych typów może mieć następujący definicji interfejsu API, na przykład:

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

Czy dane wyjściowe:

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

`rects` Parametru będą umieszczane wewnątrz `NSArray` zawierający `NSValue` dla każdego `CGRect` i w zamian pobierze tablicę `CAScroll?` której został utworzony za pomocą wartości zwracane `NSArray` zawierający `NSStrings`.

<a name="BindAttribute" />

### <a name="bindattribute"></a>BindAttribute

`[Bind]` Atrybut ma dwa zastosowania jednej, gdy jest stosowany do metody lub deklaracja właściwości, a innym po zastosowaniu do poszczególnych metody pobierającej lub ustawiającej we właściwości.

Gdy jest używany dla metody lub właściwości, efekt `[Bind]` atrybutu jest wywołanie metody, która wywołuje określony selektor. Ale wynikowy wygenerowanego — metoda nie jest oznaczone [ `[Export]` ](#ExportAttribute) atrybutu, co oznacza, że nie mogą uczestniczyć w Zastępowanie metody. To jest zwykle używana w połączeniu z `[Target]` atrybutu implementacji metody rozszerzenia języka Objective-C.

Na przykład:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

W przypadku używania w metoda pobierająca lub ustawiająca, `[Bind]` atrybut służy do zmiany ustawień domyślnych wykryta przez generator kodu podczas generowania nazw selektor języka Objective-C pobierającej i ustawiającej dla właściwości. Domyślnie, gdy flaga właściwości o nazwie `fooBar`, generator może wygenerować `fooBar` eksportu dla metody pobierającej i `setFooBar:` dla metody ustawiającej. W niektórych przypadkach Objective-C nie jest zgodna z tę Konwencję, zwykle zmieniać ich nazwa metody pobierającej, który ma zostać `isFooBar`.
Informowanie generator tego, czy użyć tego atrybutu.

Na przykład:

```csharp
// Default behavior
[Export ("active")]
bool Active { get; set; }

// Custom naming with the Bind attribute
[Export ("visible")]
bool Visible { [Bind ("isVisible")] get; set; }
```

<a name="AsyncAttribute" />

### <a name="asyncattribute"></a>AsyncAttribute

Tylko na platformy Xamarin.iOS 6.3 i nowszych.

Ten atrybut można stosować do metod przyjmujących obsługi uzupełniania jako ostatni argument.

Można użyć `[Async]` atrybutu dla metod, w których ostatni argument jest wywołaniem zwrotnym.  Po zastosowaniu to metody generator powiązanie wygeneruje wersja tej metody z sufiksem `Async`.  Jeśli wywołanie zwrotne nie przyjmuje żadnych parametrów, wartość zwracana będzie `Task`, jeśli wywołanie zwrotne przyjmuje parametr, wynikiem będzie `Task<T>`.

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

Ta metoda asynchroniczna wygeneruje:

```csharp
Task LoadFileAsync (string file);
```

Jeśli wywołanie zwrotne przyjmuje wiele parametrów, należy ustawić `ResultType` lub `ResultTypeName` określić odpowiednią nazwę wygenerowanego typu, w której będą przechowywane wszystkie właściwości.

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

Następujące wygeneruje tej metodzie asynchronicznej, gdzie `FileLoading` zawiera właściwości dostęp zarówno do `files` i `byteCount`:

```csharp
Task<FileLoading> LoadFile (string file);
```

W przypadku ostatniego parametru wywołania zwrotnego `NSError`, następnie wygenerowanego `Async` metoda będzie sprawdzać, jeśli wartość nie jest zerowa, a jeśli tak jest, metoda asynchroniczna wygenerowanego ustawi zadania wyjątek.

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

Powyższe generuje następujące metody asynchronicznej:

```csharp
Task<string> UploadAsync (string file);
```

I w przypadku błędu, zadanie wynikowy będzie miało wyjątek ustawioną `NSErrorException` który opakowuje powstałe w ten sposób `NSError`.

#### <a name="asyncattributeresulttype"></a>AsyncAttribute.ResultType

Ta właściwość umożliwia określenie wartości zwróceniem `Task` obiektu.   Ten parametr ma typ istniejący, w związku z tym musi być zdefiniowana w jednym z podstawowych definicji interfejsu api.

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute.ResultTypeName

Ta właściwość umożliwia określenie wartości zwróceniem `Task` obiektu.   Ten parametr przyjmuje nazwę nazwę odpowiedniego typu, generator utworzy szereg właściwości, po jednej dla każdego parametru, który przyjmuje wywołania zwrotnego.

#### <a name="asyncattributemethodname"></a>AsyncAttribute.MethodName

Ta właściwość umożliwia dostosowywanie nazwę metody asynchroniczne wygenerowany.   Wartość domyślna to nazwa metody i dołączać tekstu "Async", można to zmienić to ustawienie domyślne.

### <a name="disablezerocopyattribute"></a>DisableZeroCopyAttribute

Ten atrybut jest stosowany do parametrów ciągu lub ciągu właściwości i instruuje generatora kodu, aby nie używać ciągu kopiowania zero marshaling dla tego parametru, a zamiast tego utworzyć nowe wystąpienie NSString z ciągu języka C#.
Ten atrybut jest wymagany tylko na ciągach, jeśli poinstruować generatora, aby użyć ciągu kopiowania zero organizowanie za pomocą `--zero-copy` opcji wiersza polecenia lub ustawienie atrybut poziomu zestawu `ZeroCopyStringsAttribute`.

Jest to konieczne w przypadku, gdy właściwość jest zadeklarowany w języku Objective-C za `retain` lub `assign` właściwości zamiast `copy` właściwości. Te zwykle się tak zdarzyć w bibliotekach innych firm, które zostały błędnie "zoptymalizowane" przez deweloperów. Ogólnie rzecz biorąc `retain` lub `assign` `NSString` właściwości są nieprawidłowe, ponieważ `NSMutableString` lub klas pochodnych użytkownika `NSString` może zmienić zawartość ciągi bez wiedzy o kod biblioteki, w niewielkim stopniu krytyczne aplikacja. Zwykle dzieje się z powodu optymalizacji przedwczesne.

Poniżej przedstawiono dwa takich właściwości w celu C:

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```


### <a name="disposeattribute"></a>DisposeAttribute

Po zastosowaniu `[DisposeAttribute]` do klasy, podaj fragment kodu, który zostanie dodany do `Dispose()` implementacji metody klasy.

Ponieważ `Dispose` metody jest generowana automatycznie przez `bmac-native` i `btouch-native` narzędzia, musisz użyć `[Dispose]` atrybut można wstrzyknąć kodu w wygenerowanym `Dispose` implementacji metody.

Na przykład:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute" />

### <a name="exportattribute"></a>ExportAttribute

`[Export]` Atrybut służy do Flaga metody lub właściwości, które mają być uwidaczniane do środowiska wykonawczego języka Objective-C. Ten atrybut jest udostępniana między narzędzie powiązania i rzeczywistego Xamarin.iOS i Xamarin.Mac środowisk uruchomieniowych. W przypadku metod parametr jest przekazywany dosłownego wyrażenia do wygenerowanego kodu, dla właściwości, metody pobierającej i ustawiającej eksportu są generowane, oparte na deklaracji podstawowej (zobacz sekcję dotyczącą [ `[BindAttribute]` ](#BindAttribute) informacji na temat alter działanie narzędzia powiązanie).

Składnia:

```csharp
public enum ArgumentSemantic {
    None, Assign, Copy, Retain.
}

[AttributeUsage (AttributeTargets.Method | AttributeTargets.Constructor | AttributeTargets.Property)]
public class ExportAttribute : Attribute {
    public ExportAttribute();
    public ExportAttribute (string selector);
    public ExportAttribute (string selector, ArgumentSemantic semantic);
    public string Selector { get; set; }
    public ArgumentSemantic ArgumentSemantic { get; set; }
}
```

[Selektora](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html) reprezentuje nazwę podstawowej Objective-C — metoda lub właściwość jest powiązana.

#### <a name="exportattributeargumentsemantic"></a>ExportAttribute.ArgumentSemantic

<a name="FieldAttribute" />

### <a name="fieldattribute"></a>FieldAttribute

Ten atrybut służy do udostępnienia zmiennej globalnej C jako pola, które jest załadowany na żądanie i ujawniony dla kodu C#. Zazwyczaj jest to wymagane do uzyskania wartości stałe, które są zdefiniowane w C lub Objective-C i który może być albo tokeny używane w niektórych interfejsów API lub, których wartości są nieprzezroczyste i mogą być używane jako — jest przez kod użytkownika.

Składnia:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

`symbolName` Jest symbol C, aby połączyć z. Domyślnie to zostanie załadowany z biblioteki którego nazwa jest wywnioskowany na podstawie przestrzeni nazw w którym zdefiniowano typ. Jeśli nie jest biblioteki, w których wyszukiwane symbol, należy przekazać `libraryName` parametru. Jeśli łączysz biblioteki statycznej, użyj `__Internal` jako `libraryName` parametru.

Właściwości wygenerowanego zawsze są statyczne.

Właściwości oznaczone atrybutem pól mogą być następujące:

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* Wyliczenia

Metody ustawiające nie są obsługiwane dla [wyliczenia opartym na stałe NSString](#enum-attributes), ale one mogą być ręcznie powiązane w razie potrzeby.

Przykład:

```csharp
[Static]
interface CameraEffects {
     [Field ("kCameraEffectsZoomFactorKey", "CameraLibrary")]
     NSString ZoomFactorKey { get; }
}
```

<a name="InternalAttribute" />

### <a name="internalattribute"></a>InternalAttribute

`[Internal]` Atrybut można stosować do metody lub właściwości i ma wpływ Flagowanie wygenerowanego kodu z `internal` C# — słowo kluczowe udostępnianie kodu tylko na kod w wygenerowanym zestawie. Zazwyczaj umożliwia ukrywanie interfejsów API, które są zbyt niskiego poziomu lub podaj nieoptymalne publiczny interfejs API, który chcesz zwiększyć po lub do interfejsów API, które nie są obsługiwane przez generatora i wymaga kodowania niektórych ręcznie.

Podczas projektowania powiązanie, czy zwykle Ukryj metody lub właściwości przy użyciu tego atrybutu i podaj inną nazwę metody lub właściwości i następnie w pliku C# uzupełniające pomocy technicznej, należy dodać otoki jednoznacznie, który ujawnia funkcjonalność.

Na przykład:

```csharp
[Internal]
[Export ("setValue:forKey:")]
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

Następnie w pliku obsługi może mieć kodu następująco:

```csharp
public NSObject this [NSObject idx] {
    get {
        return _GetValueForKey (idx);
    }
    set {
        _SetValueForKey (value, idx);
    }
}
```

<a name="IsThreadStaticAttribute" />

### <a name="isthreadstaticattribute"></a>IsThreadStaticAttribute

Ten atrybut flags pole zapasowe dla właściwość, która ma być oznaczony za pomocą programu .NET `[ThreadStatic]` atrybutu. Jest to przydatne, jeśli pole jest statyczna zmienna wątku.

### <a name="marshalnativeexceptions-xamarinios-606"></a>MarshalNativeExceptions (Xamarin.iOS 6.0.6)

Tego atrybutu spowoduje, że metoda pomocy technicznej natywnego (Objective-C) wyjątki.
Zamiast wywoływać metodę `objc_msgSend` bezpośrednio, wywołanie będzie przejście przez trampoline niestandardowego, który przechwytuje ObjectiveC wyjątki i marshals je na zarządzane wyjątki.

Obecnie tylko kilka `objc_msgSend` podpisy są obsługiwane (znajdziesz się, jeśli podpis nie jest obsługiwana, gdy powiązanie natywnych aplikacji, która używa powiązanie kończy się niepowodzeniem i brak monotouch_*_objc_msgSend* symbol), ale może być więcej dodane na żądanie.


### <a name="newattribute"></a>NewAttribute

Ten atrybut jest stosowany do metod i właściwości do generatora ma generować `new` — słowo kluczowe przed deklaracji.

Jest on używany w celu uniknięcia ostrzeżeń kompilatora podczas tej samej metody lub właściwości Nazwa została wprowadzona w systemie podklasą istniał już w klasie podstawowej.

<a name="NotificationAttribute" />

### <a name="notificationattribute"></a>NotificationAttribute

Ten atrybut można stosować do pola, aby mieć produktu generator pomocnika silnie typizowana Klasa powiadomienia.

Ten atrybut może być używany bez argumentów dla powiadomień, które zawiera ładunek lub można określić `System.Type` który odwołuje się do innego interfejsu w definicji interfejsu API, zwykle o nazwie kończą się ciągiem "EventArgs". Generator spowoduje wyłączenie interfejsu w klasie tej podklasy `EventArgs` i będzie zawierać wszystkie właściwości, na liście. [ `[Export]` ](#ExportAttribute) Atrybut powinien być używany w `EventArgs` klasy, aby wyświetlić listę nazwę używaną do odszukania słownika języka Objective C można pobrać wartości klucza.

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
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Użytkownicy kodu łatwo można następnie Subskrybowanie powiadomień o publikowanego [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) przy użyciu kodu w następujący sposób:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Lub wartość określonego obiektu aby przyjrzeć się. W przypadku przekazania `null` do `objectToObserve` ta metoda będzie działać podobnie jak jego elementów równorzędnych.

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

Zwracana wartość z `ObserveDidStart` można łatwo zrezygnować z otrzymywania powiadomień, w następujący sposób:

```csharp
token.Dispose ();
```

Lub możesz wywołać [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject//) i przekaż token. Jeśli powiadomienia zawiera parametry, należy określić obiekt pomocnika `EventArgs` interfejsu następująco:

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

Powyższe wygeneruje `MyScreenChangedEventArgs` klasy z `ScreenX` i `ScreenY` właściwości, które spowoduje pobranie danych z [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) słownika przy użyciu kluczy `ScreenXKey` i `ScreenYKey` odpowiednio i zastosowanie odpowiednich konwersji. `[ProbePresence]` Atrybut jest używany dla generatora do sondowania, jeśli klucz jest ustawiany `UserInfo`, zamiast wyodrębniania wartości. Służy to do przypadku obecności klucza wartość (zazwyczaj dla wartości logicznych).

Dzięki temu można napisać kod następująco:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

W niektórych przypadkach nie ma stałej skojarzone z wartością przekazany w słowniku.  Apple czasami korzysta stałe symboli publicznych, a czasami stałe typu string.  Domyślnie [ `[Export]` ](#ExportAttribute) Twojego podanych atrybut `EventArgs` klasy użyje określonej nazwy jako symboli publicznych do można przeszukiwać w czasie wykonywania.  Jeśli nie jest to, a zamiast tego powinien być wyszukiwane jako stałą typu string, a następnie przekazać `ArgumentSemantic.Assign` wartość do atrybutu eksportu.

**Nowość w 8.4 platformy Xamarin.iOS**

Czasami powiadomienia rozpocznie życia bez żadnych argumentów, więc korzystanie z [ `[Notification]` ](#NotificationAttribute) bez argumentów jest akceptowany.  Jednak czasami zostaną wprowadzone parametry powiadomienia.  Aby obsługiwać ten scenariusz, ten atrybut można zastosować więcej niż raz.

Jeśli tworzysz powiązania i chce się uniknąć dzielenia istniejący kod użytkownika, czy włączyć istniejące powiadomienie z:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

W wersji, która wyświetla atrybut powiadomień dwa razy, jak to:

```csharp
interface MyClass {
    [Notification]
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

<a name="NullAllowedAttribute" />

### <a name="nullallowedattribute"></a>NullAllowedAttribute

Jeśli to jest stosowany do właściwości oznacza flagą właściwości jako zezwalający na wartość `null` do przypisania do niej. To jest prawidłowa tylko dla typów odwołań.

Gdy ten jest stosowany do parametrów w podpisie metody, wskazuje, że określony parametr może mieć wartości null i czy nie powinny być sprawdzane na przekazywanie `null` wartości.

Jeśli typ referencyjny nie ma tego atrybutu, narzędzie powiązania wygeneruje wyboru dla wartości przypisywane przed przekazaniem go do języka Objective C i wygeneruje Sprawdź, czy zgłosi `ArgumentNullException` przypisana wartość `null`.

Na przykład:

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute" />

### <a name="overrideattribute"></a>OverrideAttribute

Użyj tego atrybutu nakazać generator powiązanie, czy powiązania dla tej konkretnej metody powinny oznaczone stanem `override` — słowo kluczowe.

### <a name="presnippetattribute"></a>PreSnippetAttribute

Ten atrybut służy do wprowadzenia kodu ma zostać wstawiony po sprawdzeniu poprawności parametrów wejściowych, ale przed jego kod wywołuje Objective-C.

Przykład:

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```

### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

Ten atrybut służy do wstrzyknięcia kodu ma zostać wstawiony przed wszystkie parametry są prawidłowe w metodzie wygenerowany.

Przykład:

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```

### <a name="postgetattribute"></a>PostGetAttribute

Powoduje, że generator powiązania do wywołania określonej właściwości z tej klasy można pobrać wartości z niego.

Ta właściwość jest zwykle używana do odświeżania pamięci podręcznej, która wskazuje do obiektów odniesienia, zachowujących wykres obiektu, do których odwołuje się. Zazwyczaj jest wyświetlane na kod, który ma operacje, takie jak dodawanie i usuwanie. Ta metoda jest używana, tak aby po elementy są dodawane lub usuwane, że można zaktualizować wewnętrznej pamięci podręcznej, aby upewnić się, że firma Microsoft utrzymują zarządzanych odwołania do obiektów, które są rzeczywiście używane. Jest to możliwe, ponieważ narzędzie powiązania generuje polem zapasowym dla wszystkich obiektów odniesienia w danym powiązaniu.

Przykład:

```csharp
[BaseType (typeof (NSObject))]
[Since (4,0)]
public interface NSOperation {
    [Export ("addDependency:")][PostGet ("Dependencies")]
    void AddDependency (NSOperation op);

    [Export ("removeDependency:")][PostGet ("Dependencies")]
    void RemoveDependency (NSOperation op);

    [Export ("dependencies")]
    NSOperation [] Dependencies { get; }
}
```

W takim przypadku `Dependencies` właściwości zostanie wywołany po dodaniu lub usunięciu zależności z `NSOperation` obiekt, zapewnienie, że mamy wykresu, który reprezentuje rzeczywisty załadowany obiektów, co uniemożliwia zarówno przecieki pamięci, jak i uszkodzenie pamięci.

### <a name="postsnippetattribute"></a>PostSnippetAttribute

Ten atrybut służy do wprowadzenia kodu źródłowego C# ma zostać wstawiony po kodzie została wywołana z nim metody Objective-C

Przykład:

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>ProxyAttribute

Ten atrybut jest stosowany do zwrócenia wartości, aby flaga je jako obiekty serwera proxy. Niektóre obiekty zwrotny serwer proxy interfejsów API języka Objective-C, które nie mogą być rozróżniane z powiązania użytkownika. Efektem tego atrybutu jest flaga obiektu jako `DirectBinding` obiektu. Scenariusz w Xamarin.Mac, można znaleźć [omówione tej usterki](https://bugzilla.novell.com/show_bug.cgi?id=670844).

### <a name="retainlistattribute"></a>RetainListAttribute

Powoduje, że generatora, aby zachować zarządzane odniesienia do parametru lub usuń wewnętrzny odwołania do parametru. To jest używany do przechowywania obiektów, do których odwołuje się.

Składnia:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Jeśli wartość `doAdd` ma wartość true, parametr jest dodawana do `__mt_{0}_var List<NSObject>;`. Gdzie `{0}` jest zastępowany danego `listName`. W klasie częściowej uzupełniające do interfejsu API musi zadeklarować tego pola zapasowego.

Na przykład zobacz [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) i [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin.iOS 6.0)

To może odnosić się do zwracanych typów, aby wskazać, czy generator powinien wywoływać `Release` obiektu przed zwróceniem. Jest to potrzebne tylko wtedy, gdy metody zawiera obiekt zachowywane (w przeciwieństwie do obiektu autoreleased, który jest najbardziej typowym scenariuszem)

Przykład:

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

Ponadto ten atrybut jest propagowana do wygenerowanego kodu tak, aby środowiska wykonawczego platformy Xamarin.iOS wie, że należy zachować obiektu na wracając do języka Objective-C z takich funkcji.


### <a name="sealedattribute"></a>SealedAttribute

Powoduje, że generatora, aby flaga wygenerowane metody jako sealed. Jeśli ten atrybut nie jest określony, wartością domyślną jest do generowania metodą wirtualną (metoda wirtualna, metoda abstrakcyjna lub zastąpienia w zależności od tego, jak są używane inne atrybuty).

<a name="StaticAttribute" />

### <a name="staticattribute"></a>StaticAttribute

Gdy `[Static]` atrybut jest stosowany do metody lub właściwości, spowoduje to wygenerowanie statycznej metody lub właściwości. Jeśli ten atrybut nie jest określony, generator tworzy wystąpienie metody lub właściwości.


### <a name="transientattribute"></a>TransientAttribute

Użyj flagi właściwości, których wartości są przejściowe, oznacza to, obiekty, które są tworzone tymczasowo przez system iOS, ale nie są długotrwałe dla tego atrybutu. Jeśli ten atrybut jest stosowany do właściwości, generator nie tworzy pole zapasowe dla tej właściwości, co oznacza, że klasa zarządzana nie przechowuje odwołania do obiektu.

<a name="WrapAttribute" />

### <a name="wrapattribute"></a>WrapAttribute

W projekcie powiązania Xamarin.iOS/Xamarin.Mac `[Wrap]` atrybutu jest stosowanego do opakowywania obiektu słabą kontrolą jednoznacznie obiektu. To jest dostarczany do gry przede wszystkim z obiektami delegata Objective-C, które zwykle są zadeklarowane jako typu `id` lub `NSObject`. Jest używany przez Xamarin.iOS i Xamarin.Mac Konwencji do udostępnienia tych źródeł danych lub delegatów jako typu `NSObject` o nazwie przy użyciu konwencji "Weak" + nazwa jest widoczna. `id delegate` Właściwość w języku Objective-C będzie widoczne jako `NSObject WeakDelegate { get; set; }` właściwość w pliku kontrakt interfejsu API.

Ale zwykle jest to wartość przypisana do tego obiektu delegowanego silnego typu, dlatego firma Microsoft surface określonego typu i Zastosuj `[Wrap]` atrybutu, oznacza to, że użytkownicy mogą być używane słabe typów, jeśli muszą pewną kontrolę szczegółowe lub potrzebują odwołać się do tric niskiego poziomu łączy, lub można użyć właściwości jednoznacznie większość pracy.

Przykład:

```csharp
[BaseType (typeof (NSObject))]
interface Demo {
     [Export ("delegate"), NullAllowed]
     NSObject WeakDelegate { get; set; }

     [Wrap ("WeakDelegate")]
     DemoDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
interface DemoDelegate {
    [Export ("doDemo")]
    void DoDemo ();
}
```

Jest to, jak użytkownik użyje słabą kontrolą wersji delegata:

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

I to jest sposób użytkownika będzie używana wersja jednoznacznie, zwróć uwagę, czy użytkownik korzysta z system typów C# w i aby zadeklarować jego celem przy użyciu słowa kluczowego override i że nie ma do ręcznie dekoracji metody `[Export]`, ponieważ robiliśmy który Praca w powiązaniu dla użytkownika:

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```

Użycie innego `[Wrap]` atrybut jest obsługi wersji silnie typizowane metody.  Na przykład:

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Elementy członkowskie, generowane przez `[Wrap]` nie są `virtual` domyślnie, jeśli potrzebujesz `virtual` można ustawić elementu członkowskiego `true` opcjonalny `isVirtual` parametru.

```csharp
[BaseType (typeof (NSObject))]
interface FooExplorer {
    [Export ("fooWithContentsOfURL:")]
    void FromUrl (NSUrl url);

    [Wrap ("FromUrl (NSUrl.FromString (url))", isVirtual: true)]
    void FromUrl (string url);
}
```

## <a name="parameter-attributes"></a>Atrybuty parametrów

W tej sekcji opisano atrybuty, które można zastosować do parametrów w definicji metody, jak również `[NullAttribute]` dotyczący właściwości jako całość.

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

Ten atrybut jest stosowany do parametrów typów w deklaracjach delegata C# do powiadamiania integratora, że parametr zagrożona odpowiada bloku Objective-C konwencji wywoływania i powinien kierować w ten sposób.

To jest zazwyczaj używana w przypadku wywołania zwrotne zdefiniowane następująco w celu C:

```csharp
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

Zobacz też: [CCallback](#CCallback).

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

Ten atrybut jest stosowany do typów parametru w deklaracji obiektu delegowanego C# do powiadamiania integratora, że parametr zagrożona odpowiada Konwencja wywoływania wskaźnika funkcji C ABI i powinien kierować w ten sposób.

To jest zazwyczaj używana w przypadku wywołania zwrotne zdefiniowane następująco w celu C:

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

Zobacz też: [BlockCallback](#BlockCallback).

### <a name="params"></a>Parametry

Można użyć `[Params]` atrybut ostatni parametr tablicy definicję metody mają generator wstrzyknąć "params" w definicji.   Dzięki temu powiązanie umożliwić łatwe następujące parametry opcjonalne.

Na przykład następującej definicji:

```csharp
[Export ("loadFiles:")]
void LoadFiles ([Params]NSUrl [] files);
```

Umożliwia skonfigurowanie następujący kod do zapisania:

```csharp
foo.LoadFiles (new NSUrl (url));
foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));
```

To ustawienie dodatkową zaletę, że nie wymaga użytkowników do utworzenia tablicy wyłącznie w celu przekazywania elementów.

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

Można użyć `[PlainString]` atrybutu przed parametrów ciągu nakazać generatora powiązanie, aby przekazać ciągu jako ciąg C, zamiast przekazywanie jako parametr `NSString`.

Korzystać z większości interfejsów API języka Objective-C `NSString` parametrów, ale kilka interfejsów API ujawnia `char *` interfejsu API przekazywania ciągów, zamiast `NSString` odmiany.
Użyj `[PlainString]` w takich przypadkach.

Na przykład następujące deklaracje języka Objective-C:

```csharp
- (void) setText: (NSString *) theText;
- (void) logMessage: (char *) message;
```

Powinny być powiązane następująco:

```csharp
[Export ("setText:")]
void SetText (string theText);

[Export ("logMessage:")]
void LogMessage ([PlainString] string theText);
```

### <a name="retainattribute"></a>RetainAttribute

Powoduje, że generatora, aby utrzymywać odwołania do określonego parametru. Generator zapewni magazynu zapasowego dla tego pola lub można określić nazwę ( `WrapName`) do przechowywania wartości w. Jest to przydatne do przechowywania odwołanie do zarządzanego obiektu przekazanego jako parametr do języka Objective-C, a po wiadomo, że Objective-C będzie przechowywać tylko kopii tego obiektu. Na przykład interfejsu API, takich jak `SetDisplay (SomeObject)` użyje tego atrybutu jest prawdopodobne, że SetDisplay można tylko wyświetlić jeden obiekt naraz. Jeśli potrzebujesz do śledzenia więcej niż jeden obiekt (na przykład w przypadku API stosu przypominającej) należy użyć `[RetainList]` atrybutu.

Składnia:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```


### <a name="retainlistattribute"></a>RetainListAttribute

Powoduje, że generatora, aby zachować zarządzane odniesienia do parametru lub usuń wewnętrzny odwołania do parametru. To jest używany do przechowywania obiektów, do których odwołuje się.

Składnia:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Jeśli wartość `doAdd` ma wartość true, parametr jest dodawana do `__mt_{0}_var List<NSObject>`. Gdzie `{0}` jest zastępowany danego `listName`. W klasie częściowej uzupełniające do interfejsu API musi zadeklarować tego pola zapasowego.

Na przykład zobacz [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) i [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)


### <a name="transientattribute"></a>TransientAttribute

Ten atrybut jest stosowany do parametrów i jest używana tylko podczas przejścia z języka Objective C dla C#.  Podczas tych przejścia różnych Objective-C `NSObject` parametry są opakowywane w zarządzanym reprezentacji obiektu.

Środowisko uruchomieniowe otrzymuje odwołanie do natywnego obiektu i utrzymywać odwołania do momentu zarządzanych ostatnie odwołanie do obiektu został usunięty i wykaz Globalny ma możliwość uruchamiania.

W niektórych przypadkach należy przechowuje odwołanie do natywnego obiektu Runtime C#.  Taka sytuacja może wystąpić, gdy podstawowy kod macierzysty został dołączony specjalnego zachowania do cyklu życia parametru.  Na przykład: destruktor dla parametru będzie wykonanie akcji oczyszczania lub usunąć niektóre cenny zasobów.

Ten atrybut informuje środowisko uruchomieniowe, które chcesz, aby obiekt można usunąć, jeśli to możliwe, wracając do języka Objective-C z zastąpić metodę.

Reguła jest prosty: Jeśli środowiska uruchomieniowego musieli tworzyć nowych reprezentacja zarządzanego z natywnego obiektu na końcu funkcji liczba Zachowaj dla obiekt natywny zostaną usunięte i zostaną usunięte z właściwością dojście obiektu zarządzanego.   Oznacza to, że jeśli pozostawiono odwołanie do obiektu zarządzanego odwołujące się stanie się bezużyteczny (wywoływanie metod na nim spowoduje zgłoszenie wyjątku).

Jeśli przekazany obiekt nie został utworzony lub został już oczekujące zarządzanych reprezentację obiektu, wymuszone dispose nie ma miejsca. 


## <a name="property-attributes"></a>Atrybuty właściwości

<a name="NotImplementedAttribute" />

### <a name="notimplementedattribute"></a>NotImplementedAttribute

Ten atrybut służy do obsługi idiom Objective-C, w którym wprowadzono w klasie podstawowej z metody pobierającej właściwości i modyfikowalną podklasy wprowadza metody ustawiającej.

Ponieważ C# nie obsługuje tego modelu, klasa podstawowa musi mieć zarówno metodę ustawiającą, jak i metody pobierającej i podklasy mogą używać [OverrideAttribute](#OverrideAttribute).

Ten atrybut jest używana tylko w metody ustawiające właściwości i jest używana do obsługi modyfikowalną idiom Objective-c.

Przykład:

```csharp
[BaseType (typeof (NSObject))]
interface MyString {
    [Export ("initWithValue:")]
    IntPtr Constructor (string value);

    [Export ("value")]
    string Value {
        get;

    [NotImplemented ("Not available on MyString, use MyMutableString to set")]
        set;
    }
}

[BaseType (typeof (MyString))]
interface MyMutableString {
    [Export ("value")]
    [Override]
    string Value { get; set; }
}
```

<a name="enum-attributes" />

## <a name="enum-attributes"></a>Enum — atrybuty

Mapowanie `NSString` prosty sposób utworzyć interfejs API .NET lepiej jest stałe wartości wyliczenia. Go:

* Umożliwia kod zakończenia ma być bardziej użyteczna, zwiększając liczbę pokazywanych **tylko** poprawne wartości dla interfejsu API;
* Dodaje typ bezpieczeństwa, nie można użyć innej `NSString` stałej w kontekście niepoprawne; i
* pozwala, aby ukryć niektóre stałe, co Pokaż krótszej listy interfejsu API bez utraty funkcji uzupełniania kodu.

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
```

Z powyższych definicji powiązania utworzy generatora `enum` się i zostanie również utworzona `*Extensions` statycznego typu zawierająca metody dwa sposoby konwersji między wartościami wyliczenia i `NSString` stałe. Oznacza to stałe pozostaje dostępna dla deweloperów, nawet jeśli nie są częścią interfejsu API.

Przykłady:

```csharp
// using the NSString constant in a different API / framework / 3rd party code
CallApiRequiringAnNSString (NSRunLoopMode.Default.GetConstant ());
```

```csharp
// converting the constants from a different API / framework / 3rd party code
var constant = CallApiReturningAnNSString ();
// back into an enum value
CallApiWithEnum (NSRunLoopModeExtensions.GetValue (constant));
```

### <a name="defaultenumvalueattribute"></a>DefaultEnumValueAttribute

Można dekoracji **jeden** wartości wyliczenia z tym atrybutem. Będzie to stała zwracanych wartości enum nie jest znany.

W powyższym przykładzie:

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

Jeśli żadna wartość enum jest oznaczone, a następnie `NotSupportedException` zostanie wygenerowany.

### <a name="errordomainattribute"></a>ErrorDomainAttribute

Kody błędów są powiązane jako wartości wyliczenia. Zazwyczaj jest błąd domeny dla nich i nie zawsze jest łatwo znaleźć zastosowanie co (lub jeśli nawet istnieje).

Ten atrybut służy do skojarzenia z wyliczenia sam błąd domeny.

Przykład:

```csharp
    [Native]
    [ErrorDomain ("AVKitErrorDomain")]
    public enum AVKitError : nint {
        None = 0,
        Unknown = -1000,
        PictureInPictureStartFailed = -1001
    }
```

Następnie można wywołać metody rozszerzenia `GetDomain` uzyskać stałej żadnych błędów.

### <a name="fieldattribute"></a>FieldAttribute

Jest to ten sam `[Field]` używany dla stałych wewnątrz typu atrybutu. On również umożliwia wewnątrz wyliczenia mapowanie wartości z określonego stałą.

A `null` wartość może być używana do określenia wartości wyliczenia, które ma zostać zwrócony, jeśli `null` `NSString` stała jest określona.

W powyższym przykładzie:

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

Jeśli nie `null` ma wartość, a następnie `ArgumentNullException` zostanie wygenerowany.

## <a name="global-attributes"></a>Atrybuty globalne

Atrybutami globalnymi albo są stosowane przy użyciu `[assembly:]` modyfikator atrybutów, takich jak [ `[LinkWithAttribute]` ](#LinkWithAttribute) lub mogą być używane wszędzie, gdzie, takie jak [ `[Lion]` ](#SinceAndLionAttributes) i [ `[Since]` ](#SinceAndLionAttributes) atrybutów.

<a name="LinkWithAttribute" />

### <a name="linkwithattribute"></a>LinkWithAttribute

Jest to atrybut poziomu zestawu, który umożliwia deweloperom Określ połączeń flagi wymagane ponowne powiązane biblioteki bez wymuszania konsumenta biblioteki ręcznie skonfigurować gcc_flags i mtouch dodatkowe argumenty przekazane do biblioteki.

Składnia:

```csharp
// In properties
[Flags]
public enum LinkTarget {
    Simulator    = 1,
    ArmV6    = 2,
    ArmV7    = 4,
    Thumb    = 8,
}

[AttributeUsage(AttributeTargets.Assembly, AllowMultiple=true)]
public class LinkWithAttribute : Attribute {
    public LinkWithAttribute ();
    public LinkWithAttribute (string libraryName);
    public LinkWithAttribute (string libraryName, LinkTarget target);
    public LinkWithAttribute (string libraryName, LinkTarget target, string linkerFlags);
    public bool ForceLoad { get; set; }
    public string Frameworks { get; set; }
    public bool IsCxx { get; set;  }
    public string LibraryName { get; }
    public string LinkerFlags { get; set; }
    public LinkTarget LinkTarget { get; set; }
    public bool NeedsGccExceptionHandling { get; set; }
    public bool SmartLink { get; set; }
    public string WeakFrameworks { get; set; }
}
```

Ten atrybut jest stosowany na poziomie zestawu, na przykład, co jest [powiązania CorePlot](https://github.com/mono/monotouch-bindings/tree/master/CorePlot) użyć:

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

Jeśli używasz `[LinkWith]` atrybutu określony `libraryName` jest osadzone w zestawie wynikowym, dzięki czemu użytkownicy mogą wydać pojedynczego pliku DLL zawierającego zarówno zależności niezarządzane, a także należy poprawnie używać flagi wiersza polecenia Biblioteka z platformy Xamarin.iOS.

Istnieje również możliwość zapewnia `libraryName`, w którym to przypadku `LinkWith` atrybut można tylko określić flagi konsolidatora dodatkowe:

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>Konstruktory LinkWithAttribute

Te konstruktorami umożliwiają określ bibliotekę do osadzenia w zestawie wynikowym, w obsługiwanych obiektów docelowych, które obsługuje biblioteki i wszystkie flagi opcjonalne biblioteki, które są niezbędne do połączenia z biblioteką i łącza z.

Należy pamiętać, że `LinkTarget` argument jest wykryta przez Xamarin.iOS i nie musi być ustawiona.

Przykłady:

```csharp
// Specify additional linker:
[assembly: LinkWith (LinkerFlags = "-sqlite3")]

// Specify library name for the constructor:
[assembly: LinkWith ("libDemo.a");

// Specify library name, and link target for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator);

// Specify only the library name, link target and linker flags for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator, SmartLink = true, ForceLoad = true, IsCxx = true);
```

#### <a name="linkwithattributeforceload"></a>LinkWithAttribute.ForceLoad

`ForceLoad` Właściwość jest używana do określania, czy `-force_load` łącze flaga jest wykorzystywana do natywnej biblioteki konsolidacji. Teraz to powinien zawsze mieć wartość PRAWDA.

#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute.Frameworks

Czy biblioteka jest powiązany ma twardych wymaganie w żadnych platform (inne niż `Foundation` i `UIKit`), należy ustawić `Frameworks` właściwości na ciąg zawierający listę rozdzielonych spacjami struktury platformy. Na przykład, jeśli są powiązanie biblioteki wymagającego `CoreGraphics` i `CoreText`, należy ustawić `Frameworks` właściwości `"CoreGraphics CoreText"`.

#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute.IsCxx

Ustaw tę właściwość na wartość true, jeśli wynikowego pliku wykonywalnego musi być kompilowana przy użyciu kompilatora C++ zamiast domyślna, czyli kompilatora C. Użycie tej biblioteki, która jest został napisany w języku C++.

#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute.LibraryName

Nazwa biblioteki niezarządzanych do pakietu. Jest to plik z rozszerzeniem ".a" i może zawierać kod obiektu dla wielu platform (na przykład, ARM i x86 dla symulator).

Zaznaczone wcześniejszych wersji platformy Xamarin.iOS `LinkTarget` właściwości w celu określenia platformy biblioteki obsługiwane, ale jest teraz automatycznie wykrywane i `LinkTarget` właściwość jest ignorowana.

#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute.LinkerFlags

`LinkerFlags` Ciąg umożliwia autorom powiązanie określić flagi konsolidatora dodatkowe potrzebne podczas łączenia natywnej biblioteki do aplikacji.

Na przykład, jeśli natywnej biblioteki wymaga libxml2 i użyciem biblioteki zlib, należy ustawić `LinkerFlags` ciąg `"-lxml2 -lz"`.

#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute.LinkTarget

Zaznaczone wcześniejszych wersji platformy Xamarin.iOS `LinkTarget` właściwości w celu określenia platformy biblioteki obsługiwane, ale jest teraz automatycznie wykrywane i `LinkTarget` właściwość jest ignorowana.

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute.NeedsGccExceptionHandling

Ustaw tę właściwość na wartość true, jeśli biblioteka, że połączenie jest ustanawiane wymaga biblioteki obsługi wyjątków GCC (gcc_eh)

#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute.SmartLink

`SmartLink` Właściwość powinna być ustawiona na true, aby umożliwić Xamarin.iOS ustalić, czy `ForceLoad` jest wymagany.

#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute.WeakFrameworks

`WeakFrameworks` Właściwości działa tak samo jak `Frameworks` właściwości, z wyjątkiem w czasie konsolidacji `-weak_framework` specyfikator jest przekazywana do gcc dla każdej z wymienionych platform.

`WeakFrameworks` Umożliwia bibliotek i aplikacjom słabo łącze względem struktury platformy, dzięki czemu można opcjonalnie korzystać, jeśli są dostępne, ale nie przyjmują twardych zależności na nich, co jest przydatne, jeśli biblioteka jest przeznaczona do dodawania dodatkowych funkcji na nowsze wersje systemu IOS. Aby uzyskać więcej informacji na słabe połączeń, zobacz dokumentację firmy Apple na [słabe łączenie](http://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html).

Nadaje słabe łączenia byłoby `Frameworks` kont, takich jak `CoreBluetooth`, `CoreImage`, `GLKit`, `NewsstandKit` i `Twitter` ponieważ są one dostępne w systemie iOS 5 tylko.

<a name="SinceAndLionAttributes" />

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) i LionAttribute (macOS)

Możesz użyć `[Since]` atrybut flagi interfejsów API jako mający wprowadzana w określonym punkcie w czasie. Atrybut powinien być używany tylko do flagi typy i metody, które mogły spowodować problem środowiska uruchomieniowego, jeśli klasy podstawowej, metoda lub właściwość nie jest dostępna.

Składnia:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

Go zazwyczaj nie powinny być stosowane do wyliczenia, ograniczenia lub nowe struktury jako tych będzie powodował błąd w czasie wykonywania, jeśli są one wykonywane na urządzeniu przy użyciu starszej wersji systemu operacyjnego.

Przykład, gdy jest stosowany do typu:

```csharp
// Type introduced with iOS 4.2
[Since (4,2)]
[BaseType (typeof (UIPrintFormatter))]
interface UIViewPrintFormatter {
    [Export ("view")]
    UIView View { get; }
}
```

Przykład, gdy jest stosowany do nowego elementu członkowskiego:

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

`[Lion]` Atrybut jest stosowany w taki sam sposób, ale dla typów wprowadzone w systemie Lion. Powód użycia `[Lion]` i bardziej szczegółowe numer wersji, który jest używany w systemie iOS jest tym iOS jest bardzo często zaktualizowany podczas główne wersje OS X występuje sporadycznie i możliwe jest łatwiejsze do zapamiętania przez ich nazwa kodowa niż według numeru wersji systemu operacyjnego

### <a name="adviceattribute"></a>AdviceAttribute

Umożliwiają deweloperom wskazówki dotyczące innych interfejsów API, które mogą być bardziej wygodne dla nich do używania za pomocą tego atrybutu.   Na przykład jeśli podasz jednoznacznie wersji interfejsu API umożliwiają tego atrybutu na atrybucie słabą kontrolą bezpośrednie deweloperowi lepsze interfejsu API.

Informacje z tego atrybutu jest wyświetlana w dokumentacji i narzędzi mogą być opracowane umożliwiają użytkownika sugestie dotyczące sposobów usprawnienia

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

Tylko dostępne w Xamarin.iOS 5.4 i nowszych.

Generator powoduje, że ten atrybut który powiązania dla tej określonej biblioteki (jeśli zastosowano z `[assembly:]`) lub kierowanie ciągu szybkiego kopiowania zero typ powinien być używany. Ten atrybut jest odpowiednikiem przekazywanie opcji wiersza polecenia `--zero-copy` do generatora.

W przypadku używania kopii zero dla ciągów, generatora skutecznie używa tych samych parametrach C# jako ciąg, który wykorzystuje Objective-C bez konieczności tworzenia nowej `NSString` obiekt i unikając kopiowania danych z ciągów C# na ciąg języka Objective-C. Jedyną wadą używania ciągów kopiowania Zero jest, że upewnij się, że wszystkie właściwości ciągu który opakowaniem wykonywanej być oznaczony jako `retain` lub `copy` ma `[DisableZeroCopy]` atrybut. To jest wymagane, ponieważ dojście do kopiowania zero ciągów został przydzielony na stosie i jest nieprawidłowa na zwrot funkcji.

Przykład:

```csharp
[ZeroCopyStrings]
[BaseType (typeof (NSObject))]
interface MyBinding {
    [Export ("name")]
    string Name { get; set; }

    [Export ("domain"), NullAllowed]
    string Domain { get; set; }

    [DisablZeroCopy]
    [Export ("someRetainedNSString")]
    string RetainedProperty { get; set; }
}

```

Można także zastosować atrybut na poziomie zestawu i będą stosowane do wszystkich typów zestawu:

```csharp
[assembly:ZeroCopyStrings]
```

## <a name="strongly-typed-dictionaries"></a>Jednoznacznie słowników

Z Xamarin.iOS 8.0 wprowadzono obsługę łatwe tworzenie klas jednoznacznie tego zawijania `NSDictionaries`.

Gdy zawsze było możliwe do użycia [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) typu danych wraz z ręcznego interfejsu API, jest teraz znacznie prostsze, w tym celu.  Aby uzyskać więcej informacji, zobacz [udostępniając typy silne](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types).

<a name="StrongDictionary" />

### <a name="strongdictionary"></a>StrongDictionary

Jeśli ten atrybut jest stosowany do interfejsu, generator utworzy klasę z taką samą nazwę jak interfejsu, która jest pochodną [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) i włącza każda właściwość zdefiniowana w interfejsie do silnie typizowanego metody pobierającej i ustawiającej słownika.

Automatycznie generuje klasę, która może zostać utworzona z istniejącego `NSDictionary` lub który został utworzony nowy.

Ten atrybut przyjmuje jeden parametr, nazwa klasy zawierającej kluczy, które są używane do uzyskania dostępu do elementów w słowniku.   Domyślnie każdej właściwości w interfejsie z atrybutem będzie wyszukać elementu członkowskiego w określonym typie dla nazwy z sufiksem "Klucz".

Na przykład:

```csharp
[StrongDictionary ("MyOptionKeys")]
interface MyOption {
    string Name { get; set; }
    nint    Age  { get; set; }
}

[Static]
interface MyOptionKeys {
    // In Objective-C this is "NSString *MYOptionNameKey;"
    [Field ("MYOptionNameKey")]
    NSString NameKey { get; }

    // In Objective-C this is "NSString *MYOptionAgeKey;"
    [Field ("MYOptionAgeKey")]
    NSString AgeKey { get; }
}

```

W przypadku powyższych `MyOption` klasy utworzy właściwości ciągu dla `Name` który użyje `MyOptionKeys.NameKey` jako klucz w słowniku, by pobrać ciąg.   I użyje `MyOptionKeys.AgeKey` jako klucz w słowniku pobrać `NSNumber` zawierającą int.

Jeśli chcesz użyć innego klucza służy atrybutu eksportu we właściwości, na przykład:

```csharp
[StrongDictionary ("MyColoringKeys")]
interface MyColoringOptions {
    [Export ("TheName")]  // Override the default which would be NameKey
    string Name { get; set; }

    [Export ("TheAge")] // Override the default which would be AgeKey
    nint    Age  { get; set; }
}

[Static]
interface MyColoringKeys {
    // In Objective-C this is "NSString *MYColoringNameKey"
    [Field ("MYColoringNameKey")]
    NSString TheName { get; }

    // In Objective-C this is "NSString *MYColoringAgeKey"
    [Field ("MYColoringAgeKey")]
    NSString TheAge { get; }
}
```

#### <a name="strong-dictionary-types"></a>Typy silne słownik

Obsługiwane są następujące typy danych w `StrongDictionary` definicji:

|Typ interfejsu C#|`NSDictionary` Typ magazynu|
|---|---|
|`bool`|`Boolean` przechowywane w `NSNumber`|
|Wartości wyliczenia|Liczba całkowita przechowywane w `NSNumber`|
|`int`|Liczba całkowita 32-bitowych przechowywane w `NSNumber`|
|`uint`|przechowywane w liczbę całkowitą bez znaku 32-bitowych `NSNumber`|
|`nint`|`NSInteger` przechowywane w `NSNumber`|
|`nuint`|`NSUInteger` przechowywane w `NSNumber`|
|`long`|64-bitowej liczby całkowitej przechowywane w `NSNumber`|
|`float`|Liczba całkowita 32-bitowych przechowywane jako `NSNumber`|
|`double`|64-bitowej liczby całkowitej przechowywane jako `NSNumber`|
|`NSObject` i podklas|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C# `Array` z `NSObject`|`NSArray`|
|C# `Array` z wyliczenia|`NSArray` zawierający `NSNumber` wartości|
