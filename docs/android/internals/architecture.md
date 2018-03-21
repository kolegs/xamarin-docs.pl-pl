---
title: Architektura
ms.topic: article
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9834da444032622cc3547e7c99ca3de0e41bb603
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2018
---
# <a name="architecture"></a>Architektura

Uruchamianie aplikacji platformy Xamarin.Android w środowisku wykonywania Mono.
To wykonanie środowiska działa po stronie by-side z maszyną wirtualną środowiska uruchomieniowego systemu Android (GRAFIKA). Zarówno środowiska wykonawcze działają jądra systemu Linux i ujawnia różnych interfejsach API do kodu użytkownika, dzięki której deweloperzy mogą korzystać z systemu źródłowego. Mono środowiska uruchomieniowego jest napisany w języku C.

Można używać [systemu](http://msdn.microsoft.com/en-us/library/system.aspx), [System.IO](http://msdn.microsoft.com/en-us/library/system.io.aspx), [System.Net](http://msdn.microsoft.com/en-us/library/system.net.aspx) i pozostałe .NET klasy bibliotek, które mają dostęp do podstawowych funkcji systemu operacyjnego Linux.

W systemie Android, większość funkcji systemu, takie jak Audio, grafiki, OpenGL i telefonii nie są dostępne bezpośrednio do natywnych aplikacji, są dostępne tylko za pośrednictwem systemu Android API języka Java Runtime znajdującej się w jednym z [Java](https://developer.xamarin.com/api/namespace/Java.Lang/). * przestrzenie nazw lub [Android](https://developer.xamarin.com/api/namespace/Android/). * przestrzeni nazw. Architektura jest około następująco:

[![Diagram Mono i GRAFIK powyżej jądra i poniżej .NET/Java + powiązania](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Deweloperom platformy Xamarin.Android dostęp do różnych funkcji w systemie operacyjnym, przez wywoływanie interfejsów API architektury .NET, który wiedzieli, (dla niskiego poziomu dostępu) lub przy użyciu klas w przestrzenie nazw systemu Android, która zapewnia mostka do interfejsów API języka Java, które są dostępne w środowisko wykonawcze systemu Android.

Więcej informacji na sposób komunikacji klasy systemu Android z klasy systemu Android środowiska uruchomieniowego [projekt interfejsu API](~/android/internals/api-design.md) dokumentu.


## <a name="application-packages"></a>Pakiety aplikacji

Pakiety aplikacji systemu android są kontenerami ZIP z *apk* rozszerzenia pliku. Pakiety aplikacji platformy Xamarin.Android mają taką samą strukturę i układu jako normalne pakiety systemu Android, z następującymi dodatkami:

-   Zestawy aplikacji (zawierający IL) są *przechowywane* nieskompresowanych w *zestawy* folderu. Podczas uruchamiania w wersji kompilacji *apk* jest *mmap()* ed w proces i zestawy są ładowane z pamięci. Pozwala to na szybsze uruchamianie aplikacji, jak zestawy nie są do wyodrębnienia przed jej wykonaniem. - *Uwaga:* informacji o lokalizacji zestawu, takich jak [Assembly.Location](https://developer.xamarin.com/api/property/System.Reflection.Assembly.Location/) i [Assembly.CodeBase](https://developer.xamarin.com/api/property/System.Reflection.Assembly.CodeBase/)
    *nie może opierać się* w wersji kompilacji. Nie są dostępne jako wpisy różne systemu plików i mają nie można używać lokalizacji.


-   Natywnej biblioteki zawierające Mono środowiska uruchomieniowego znajdują się w obrębie *apk* . Aplikacji platformy Xamarin.Android musi zawierać natywnych bibliotek na potrzeby docelowe architektury systemu Android, np. *armeabi* , *armeabi v7a* , *x86* . Aplikacji platformy Xamarin.Android nie można uruchomić na platformie, chyba że zawiera on bibliotek odpowiednie środowiska uruchomieniowego.


Również zawierać aplikacji platformy Xamarin.Android *Android wywoływane otoki* umożliwia systemu Android do wywołania wewnątrz kodu zarządzanego.



## <a name="android-callable-wrappers"></a>Wywoływane otoki dla systemu android

- **Wywoływane otoki android** są [JNI](http://en.wikipedia.org/wiki/Java_Native_Interface) mostek, które są używane w dowolnej chwili Android środowiska uruchomieniowego trzeba wywołanie kodu zarządzanego. Metody jak wirtualne są wywoływane otoki android może zostać zastąpiona, a można zaimplementować interfejsów Java. Zobacz [Omówienie integracji Java](~/android/platform/java-integration/index.md) doc, aby uzyskać więcej informacji.


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>Wywoływane otoki zarządzane

Wywoływane otoki zarządzane są Mostek JNI, które są używane w dowolnej chwili kod zarządzany musi wywołać kodu dla systemu Android oraz zapewnić wsparcie dla Zastępowanie metody wirtualne i wdrażanie interfejsów Java. Całą [Android](https://developer.xamarin.com/api/namespace/Android/). * i powiązane przestrzenie nazw są zarządzane wywoływane otoki wygenerowane za pomocą [powiązania JAR](~/android/platform/binding-java-library/index.md).
Wywoływane otoki zarządzane są odpowiedzialne za konwersji między typami zarządzane i Android i wywoływanie metody podstawowej platformy systemu Android za pośrednictwem JNI.

Każdy utworzony wywoływana otoka zarządzanych blokad Java odwołania globalny, który jest dostępny za pośrednictwem [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) właściwości. Odwołania do globalnego służą do zapewniania mapowania między wystąpieniami Java i zarządzanych wystąpień. Globalne odwołania są ograniczone zasobu: emulatory Zezwalaj tylko 2000 globalne odwołania do istnieje w czasie, gdy sprzęt większości umożliwia ponad 52,000 globalne odwołania do istnieje w czasie.

Aby sprawdzić, kiedy globalne odwołania są tworzone i niszczone, można ustawić [debug.mono.log](~/android/troubleshooting/index.md) właściwości systemu, aby zawierała [gref](~/android/troubleshooting/index.md).

Odwołania do globalnego może zostać jawnie zwolnione przez wywołanie metody [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) na zarządzany otok można wywołać. To spowoduje usunięcie mapowania między wystąpieniem Java i zarządzane wystąpienia i umożliwić wystąpienia Java, które mają być zbierane. Jeśli ponownie dostęp do wystąpienia Java z kodu zarządzanego, zostanie utworzony nowy wywoływana otoka zarządzanych dla niego.

Należy zachować ostrożność podczas usuwanie zarządzanego wywoływane otoki, jeśli wystąpienie mogą przypadkowo udostępniać wątków jako usuwanie wystąpienia będzie miało wpływ na odwołania z innych wątków. Maksymalna bezpieczeństwa, tylko `Dispose()` wystąpień, które zostały przydzielone za pośrednictwem `new` *lub* z metody która będzie *wiedzieć* zawsze przydzielić nowych wystąpień i wystąpień nie pamięci podręcznej, które mogą spowodować wystąpienie przypadkowe udostępnianie między wątkami.



## <a name="managed-callable-wrapper-subclasses"></a>Wywoływana otoka podklasy zarządzane

Wywoływana otoka zarządzanych podklasy są miejsca zamieszkania może całą logikę specyficzne dla aplikacji "interesujące". Obejmują one niestandardowe [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) podklasy (takich jak [działania Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) typu w domyślnym szablonie projektu). (W szczególności są to dowolne *Java.Lang.Object* podklas, które są *nie* zawierają [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) atrybutów niestandardowych lub [ RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) jest *false*, co jest ustawieniem domyślnym.)

Jak zarządzać wywoływane otoki zarządzane podklasy wywoływana otoka również zawierać odwołania do globalnej, dostępny za pośrednictwem [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) właściwości. Tak samo jak z zarządzanego wywoływane otoki, odwołań do globalnych można jawnie zwolniona przez wywołanie metody [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/).
W przeciwieństwie do zarządzanego wywoływane otoki *szczególną uwagę na to* należy podjąć w celu usuwania wystąpień, jako *Dispose()*używać wystąpienia spowoduje przerwanie mapowanie między wystąpienia Java (wystąpienia Wywoływana otoka systemu android) i zarządzanego obiektu.


### <a name="java-activation"></a>Aktywacja Java

Gdy [Android wywoływana otoka](~/android/platform/java-integration/android-callable-wrappers.md) (Aktywnej) jest tworzony za pomocą języka Java, Konstruktor Aktywnej spowoduje, że odpowiedni konstruktor C# do wywołania. Na przykład Aktywnej dla *MainActivity* będzie zawierać Konstruktor domyślny, który wywoła *MainActivity*przez konstruktora domyślnego. (Jest to zrobić za pomocą *TypeManager.Activate()* wywołania konstruktorów Aktywnej.)

Istnieje jeden innych sygnatury konstruktora konsekwencji: *(IntPtr, JniHandleOwnership)* konstruktora. *(IntPtr, JniHandleOwnership)* Konstruktor jest wywoływane zawsze, gdy obiekt Java jest widoczne dla kodu zarządzanego i zarządzane wywoływana otoka musi zostać zbudowana w celu zarządzania dojście JNI. Zwykle odbywa się automatycznie.

Istnieją dwa scenariusze, w którym *(IntPtr, JniHandleOwnership)* Konstruktor musi zostać dostarczone ręcznie na podklasy zarządzane wywoływana otoka:

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) jest podklasą klasy. *Aplikacji* jest specjalne; wartość domyślna *awaryjności* będzie konstruktora *nigdy nie* można wywołać i [(IntPtr, JniHandleOwnership) zamiast tego należy podać — Konstruktor ](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Wywołanie metody wirtualnej z konstruktora klasy podstawowej.


Należy pamiętać, że [2] jest leaky abstrakcji. W języku Java, jak w języku C# wywołań metody wirtualne z konstruktorem zawsze wywołania najdalszych pochodnych implementacji metody. Na przykład [TextView (kontekstu, AttributeSet, int) konstruktora](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/) wywołuje metodę wirtualną [TextView.getDefaultMovementMethod()](http://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()), która jest powiązana jako [ Właściwość TextView.DefaultMovementMethod](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/).
W związku z tym jeśli typem [LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) zostały (1) [podklasy TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [zastąpienia TextView.DefaultMovementMethod](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)i (3) [aktywować wystąpienia tego za pomocą XML,](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) przesłoniętych *DefaultMovementMethod* właściwości będzie można wywołać przed konstruktora Aktywnej mieli możliwość wykonania i wystąpiłyby przed konstruktora C# mieli możliwość wykonania.

Jest to obsługiwane przez instancji LogTextBox za pośrednictwem [LogTextView (IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) konstruktora, jeśli wystąpienie LogTextBox Aktywnej po raz pierwszy najedzie zarządzanego kodu, a następnie wywoływania [ LogTextBox (kontekstu, IAttributeSet, int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) Konstruktor *na tym samym wystąpieniu* po wykonaniem konstruktora Aktywnej.

Kolejność zdarzeń:

1.  Układ XML jest ładowany do [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41).

2.  Android tworzy wykres obiektu układu i tworzy wystąpienie klasy *monodroid.apidemo.LogTextBox* , Aktywnej dla *LogTextBox* .

3.  *Monodroid.apidemo.LogTextBox* konstruktora [android.widget.TextView](http://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) konstruktora.

4.  *TextView* wywołuje konstruktor *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* .

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* wywołuje *LogTextBox.n_getDefaultMovementMethod()* , który wywołuje *TextView.n_GetDefaultMovementMethod()* , które wywołuje [Java.Lang.Object.GetObject&lt;TextView&gt; (JniHandleOwnership.DoNotTransfer obsługi)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* wystąpienie sprawdza, czy istnieje już odpowiedniego C# *obsługi* . Jeśli istnieje, jest zwracana. W tym scenariuszu nie istnieje, dlatego *obiekt.GetObject&lt;T&gt;()* należy go utworzyć.

7.  *Obiekt.GetObject&lt;T&gt;()* szuka *LogTextBox (IntPtr, JniHandleOwneship)* konstruktora, wywołuje ona, tworzy mapowanie między *obsługi* i utworzone wystąpienie i zwraca utworzone wystąpienie.

8.  *TextView.n_GetDefaultMovementMethod()* wywołuje *LogTextBox.DefaultMovementMethod* metoda pobierająca właściwości.

9.  Zwraca sterowania *android.widget.TextView* konstruktora, który kończy działanie.

10. *Monodroid.apidemo.LogTextBox* wykonaniem konstruktora wywoływania *TypeManager.Activate()* .

11. *LogTextBox (kontekstu, IAttributeSet, int)* konstruktora *dla tego samego wystąpienia utworzone w [7]* .

12. ...


Jeśli (IntPtr, JniHandleOwnership) nie można odnaleźć konstruktora, a następnie [System.MissingMethodException](https://developer.xamarin.com/api/type/System.MissingMethodException/) zostanie wygenerowany.


<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>Przedwczesne Dispose() wywołania

Brak mapowania między dojście JNI i odpowiednie wystąpienia C#. Java.Lang.Object.Dispose() dzieli to mapowanie. Dojście JNI wprowadzenie kodu zarządzanego po mapowanie zostało przerwane, prawdopodobnie aktywacji Java i *(IntPtr, JniHandleOwnership)* Konstruktor będą sprawdzane pod kątem i wywoływane. Jeśli nie istnieje konstruktor, wyjątek zostanie zgłoszony.

Na przykład podane następujące podklasy Wraper można wywołać zarządzane:

```csharp
class ManagedValue : Java.Lang.Object {

    public string Value {get; private set;}

    public ManagedValue (string value)
    {
        Value = value;
    }

    public override string ToString ()
    {
        return string.Format ("[Managed: Value={0}]", Value);
    }
}
```

Jeśli firma Microsoft tworzy wystąpienie metody Dispose() i spowodować zarządzane wywoływana otoka zostać ponownie utworzone:

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

Program będzie die:

```shell
E/mono    ( 2906): Unhandled Exception: System.NotSupportedException: Unable to activate instance of type Scratch.PrematureDispose.ManagedValue from native handle 4051c8c8 --->
System.MissingMethodException: No constructor found for Scratch.PrematureDispose.ManagedValue::.ctor(System.IntPtr, Android.Runtime.JniHandleOwnership)
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateProxy (System.Type type, IntPtr handle, JniHandleOwnership transfer) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   --- End of inner exception stack trace ---
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object.GetObject (IntPtr handle, JniHandleOwnership transfer, System.Type type) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object._GetObject[IJavaObject] (IntPtr handle, JniHandleOwnership transfer) [0x00000
```

Jeśli podklasy zawierają *(IntPtr, JniHandleOwnership)* konstruktora, a następnie *nowe* będzie można utworzyć wystąpienia typu. W związku z tym wystąpienie pojawi się "utratę" wszystkie dane wystąpienia, ponieważ jest nowe wystąpienie. (Należy pamiętać, że ma wartość null).

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Tylko *Dispose()* z zarządzanych podklasy wywoływana otoka, gdy wiesz, że obiekt Java nie będą już używane lub podklasy nie zawiera żadnych danych wystąpienia i a *(IntPtr, JniHandleOwnership)* podano konstruktora.



## <a name="application-startup"></a>Uruchamianie aplikacji

Gdy działanie, usługa itp. jest uruchamiana, Android najpierw sprawdza, czy jest już procesu uruchomionego na potrzeby hostowania działanie/service/etc. Jeśli takie nie istnieje żaden proces, a następnie zostanie utworzony nowy proces, [AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-intro.html) odczytu, a typ określony w [ /manifest/application/@android:name ](http://developer.android.com/guide/topics/manifest/application-element.html#nm) jest utworzyć i załadować atrybutu. Następnie, wszystkie typy określone przez [ /manifest/application/provider/@android:name ](http://developer.android.com/guide/topics/manifest/provider-element.html#nm) wartości atrybutów są tworzone i mieć ich [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/) wywołana metoda. Punkty zaczepienia Xamarin.Android do tego przez dodanie *mono. MonoRuntimeProvider* *ContentProvider* do pliku AndroidManifest.xml podczas procesu kompilacji. *Mono. MonoRuntimeProvider.attachInfo()* metoda jest odpowiedzialna za ładowania Mono środowiska uruchomieniowego do procesu.
Wszelkie próby użycia Mono przed tym punktem zakończy się niepowodzeniem. ( *Uwaga*: jest to dlaczego typy, które podklasy [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) wymaga podania [(IntPtr, JniHandleOwnership) konstruktora](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103), ponieważ wystąpienie aplikacji jest utworzone przed zainicjowaniem Mono.)

Po zakończeniu procesu inicjowania `AndroidManifest.xml` konsultacji można znaleźć nazwy klasy działania/service/itp. Aby uruchomić. Na przykład [ /manifest/application/activity/@android:name atrybutu](http://developer.android.com/guide/topics/manifest/activity-element.html#nm) służy do określania nazwy działania, aby załadować. Dla działań, ten typ musi dziedziczyć [android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/).
Określony typ jest załadowany za pośrednictwem [Class.forName()](http://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (który wymaga typu Java typ, dlatego Android wywoływane otoki), następnie wystąpienia. Tworzenie wystąpienia Android wywoływana otoka wyzwoli tworzenia wystąpienia odpowiedniego typu C#. Następnie wywoła android [Activity.onCreate(Bundle)](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , który spowoduje, że odpowiednie [Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) do wywołania, i jest wyłączony na szczepy.
