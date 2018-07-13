---
title: Architektura
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: e6a30247c13deab871bf230aba53b9963981fd02
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997403"
---
# <a name="architecture"></a>Architektura

Platforma Xamarin.Android aplikacje są uruchamiane w ramach środowisko wykonawcze Mono.
To wykonanie środowiska uruchomienia po stronie by-side z maszyną wirtualną środowiska uruchomieniowego dla systemu Android (GRAFIKA). Obydwa środowiska uruchomieniowe uruchamiane w systemie jądra systemu Linux i różne interfejsy API do kodu użytkownika, który umożliwia deweloperom dostęp do bazowego systemu. Środowisko uruchomieniowe Mono został napisany w języku C.

Mogą być przy użyciu [systemu](xref:System), [System.IO](xref:System.IO), [przestrzeni nazw System.Net](xref:System.Net) i pozostałej części programu .NET klasy biblioteki, aby dostęp do podstawowych obiektów systemu operacyjnego Linux.

W systemie Android, większość obiektów systemu, takie jak Audio, grafiki, OpenGL i telefonii nie są dostępne bezpośrednio do aplikacji natywnych, są dostępne tylko za pośrednictwem środowiska uruchomieniowego Java interfejsy API systemu Android znajdujących się w jednym z [Java](https://developer.xamarin.com/api/namespace/Java.Lang/). * przestrzenie nazw lub [Android](https://developer.xamarin.com/api/namespace/Android/). * przestrzeni nazw. Architektura jest mniej więcej następująco:

[![Diagram przedstawiający Mono i GRAFIKI powyżej jądra, jak i poniżej .NET i Java i powiązania](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Deweloperom platformy Xamarin.Android dostęp do różnych funkcji w systemie operacyjnym, poprzez wywoływanie interfejsów API platformy .NET, które znają (Aby uzyskać dostęp do niskiego poziomu) lub przy użyciu klas ujawnione w przestrzenie nazw systemu Android, które pozwala przejść do interfejsów API języka Java, które są udostępniane przez środowisko wykonawcze systemu Android.

Aby uzyskać więcej informacji na temat sposobu klas systemu Android komunikować się z klas w środowisku uruchomieniowym systemu Android zobacz [projekt interfejsu API](~/android/internals/api-design.md) dokumentu.


## <a name="application-packages"></a>Pakiety aplikacji

Pakiety aplikacji dla systemu android są ZIP kontenerów przy użyciu *.apk* rozszerzenie pliku. Pakiety aplikacji platformy Xamarin.Android mają taką samą strukturę oraz układ jako normalny pakietów dla systemu Android, z następującymi dodatkami:

-   Zestawy aplikacji (zawierający IL) są *przechowywane* bez kompresji w ramach *zestawy* folderu. Podczas procesu kompilacji uruchamiania w wersji *.apk* jest *mmap()* ed do procesu i zestawy są ładowane z pamięci. Pozwala to na szybsze uruchamianie aplikacji, ponieważ zestawy nie są do wyodrębnienia przed wykonywania. - *Uwaga:* informacji o lokalizacji zestawu, takie jak [Assembly.Location](xref:System.Reflection.Assembly.Location) i [Assembly.CodeBase](xref:System.Reflection.Assembly.CodeBase)
    *nie mogą być powoływane* w wersji kompilacje. Nie istnieją one jako odrębne filesystem wpisy i mają nie można używać lokalizacji.


-   Natywnych bibliotek środowiska uruchomieniowego Mono zawierający znajdują się w obrębie *.apk* . Aplikacji platformy Xamarin.Android musi zawierać bibliotek natywnych dla żądanego docelowych architektur systemu Android, np. *armeabi* , *armeabi v7a* , *x86* . Platforma Xamarin.Android nie mogą być uruchomione na platformie, chyba że zawiera on biblioteki odpowiedniego środowiska uruchomieniowego.


Zawierają również aplikacji platformy Xamarin.Android *wywoływane otoki systemu Android* umożliwia systemu Android, aby wywoływać kod zarządzany.



## <a name="android-callable-wrappers"></a>Wywoływane otoki systemu android

- **Wywoływane otoki systemu android** są [interfejsem JNI](http://en.wikipedia.org/wiki/Java_Native_Interface) most, które są używane w dowolnym momencie, w środowisku uruchomieniowym systemu Android musi wywołać kodu zarządzanego. Wywoływane otoki systemu android są jak wirtualne metody może zostać zastąpiona, a następnie można zaimplementować interfejsów języka Java. Zobacz [Omówienie integracji Java](~/android/platform/java-integration/index.md) dokumentu, aby uzyskać więcej informacji.


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>Wywoływane otoki zarządzane

Zarządzane wywoływanych otok są Mostek interfejsem JNI, które są używane w dowolnym momencie, kod zarządzany musi wywołać kodu dla systemu Android oraz zapewnić wsparcie dla zastępowanie metod wirtualnych i wdrażanie interfejsów języka Java. Cały [Android](https://developer.xamarin.com/api/namespace/Android/). * i pokrewne obszary nazw są zarządzane wywoływane otoki wygenerowane za pomocą [powiązania JAR](~/android/platform/binding-java-library/index.md).
Zarządzane wywoływanych otok są zobowiązani do konwersji między typami zarządzanymi i dla systemu Android i wywoływanie metody podstawowej platformy systemu Android za pośrednictwem interfejsem JNI.

Każdy utworzony zarządzanych otoka wywoływana przechowuje odwołania globalnego Java, który jest dostępny za pośrednictwem [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) właściwości. Odwołania do globalnego jest używana do udostępniania mapowania między wystąpieniami Java i wystąpienia zarządzanego. Globalne odwołania są ograniczone zasobów: emulatorów Zezwalaj tylko 2000 globalnego odwołania istnieje w czasie, podczas gdy większość sprzęt umożliwia ponad 52,000 globalnego odwołania istnieje w danym momencie.

Aby sprawdzić, kiedy globalne odwołania są tworzone i niszczone, można ustawić [debug.mono.log](~/android/troubleshooting/index.md) właściwości systemu, aby zawierała [gref](~/android/troubleshooting/index.md).

Odwołania do globalnego może być jawnie zwolniony przez wywołanie metody [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) na zarządzany otok wywoływanych. Spowoduje to usunięcie mapowania między wystąpieniem Java i wystąpienia zarządzanego i zezwolić na wystąpienie Java, które mają być zbierane. Jeśli wystąpienie Java odbywa się ponownie z kodu zarządzanego, nową otokę wywoływaną zarządzane zostaną utworzone dla niego.

Należy zachować ostrożność podczas usuwanie zarządzanych otok wywoływanych w przypadku wystąpienia, które mogą być przypadkowo współużytkowane między wątkami jako disposing wystąpienie będzie mieć wpływ na odwołania z innych wątków. Pod kątem maksymalnego bezpieczeństwa, tylko `Dispose()` wystąpień, które zostały przydzielone za pomocą `new` *lub* metod zestawu *znać* zawsze przydzielać nowe wystąpienia, a nie pamięci podręcznej wystąpień, które mogą spowodować wystąpienie przypadkowe udostępnianie między wątkami.



## <a name="managed-callable-wrapper-subclasses"></a>Zarządzanych otoka wywoływana podklasy

Podklasy zarządzanych otoka wywoływana to, gdzie wszystkie "interesujący" logikę specyficzną dla aplikacji może na żywo. Obejmują one niestandardowe [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) podklasy (takie jak [działania Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) typu w domyślnym szablonie projektu). (W szczególności są to dowolne *Java.Lang.Object* podklas, które są *nie* zawierają [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) atrybutów niestandardowych lub [ RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) jest *false*, co jest ustawieniem domyślnym.)

Jak zarządzane wywoływanych otok, zarządzanych otoka wywoływana podklasy również zawierać odwołania do globalnej, dostępne za pośrednictwem [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) właściwości. Tak samo, jak za pomocą zarządzanych otok wywoływanych, globalne odwołania można jawnie uwolniony przez wywołanie [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/).
W przeciwieństwie do zarządzanych otok wywoływanych *szczególną uwagę na* należy podjąć usuwania wystąpień, jak *Dispose()* ing wystąpienia spowoduje przerwanie mapowanie między wystąpienia Java (wystąpienia Otoka wywoływana z systemem android) i wystąpienia zarządzanego.


### <a name="java-activation"></a>Aktywacja języka Java

Gdy [otoka wywoływana z systemem Android](~/android/platform/java-integration/android-callable-wrappers.md) (Aktywnej) jest tworzony w języku Java, Konstruktor Aktywnej spowoduje, że odpowiedni konstruktor C# do wywołania. Na przykład Aktywnej dla *MainActivity* będzie zawierać konstruktora domyślnego, który będzie wywoływał *MainActivity*firmy domyślnego konstruktora. (Jest to realizowane *TypeManager.Activate()* wywoływać konstruktorów Aktywnej.)

Istnieje jeden innych sygnatury konstruktora konsekwencji: *(pola IntPtr, JniHandleOwnership)* konstruktora. *(Pola IntPtr, JniHandleOwnership)* Konstruktor jest wywoływana zawsze wtedy, gdy obiekt Java jest uwidaczniany w kodzie zarządzanym i zarządzanych otoka wywoływana musi zostać zbudowana w celu zarządzania interfejsem JNI dojście. Zwykle odbywa się automatycznie.

Istnieją dwa scenariusze, w którym *(pola IntPtr, JniHandleOwnership)* konstruktora musi zostać dostarczone ręcznie na podklasę zarządzanych otoka wywoływana:

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) jest podklasą klasy. *Aplikacji* jest specjalne; wartość domyślna *awaryjności* zostanie Konstruktor *nigdy nie* wywołana i [(pola IntPtr, JniHandleOwnership) zamiast tego należy podać konstruktora ](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Wywołanie wirtualnej metody z konstruktora klasy bazowej.


Należy pamiętać, że (2) jest abstrakcyjną leaky. W języku Java, tak jak w języku C# wywołań metod wirtualnych z konstruktorem zawsze wywołuje najbardziej pochodnej implementacji metody. Na przykład [Konstruktor TextView (kontekście AttributeSet, int)](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/) wywołuje metodę wirtualną [TextView.getDefaultMovementMethod()](http://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()), która jest powiązana jako [ Właściwość TextView.DefaultMovementMethod](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/).
W związku z tym jeśli typ [LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) zostałby (1) [podklasy TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [zastąpienia TextView.DefaultMovementMethod](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)i (3) [aktywować wystąpienia, za pomocą XML,](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) zastąpione *DefaultMovementMethod* właściwość będzie można wywołać konstruktora Aktywnej mieli Państwo możliwość wykonywania i może wystąpić, zanim Konstruktor C# mieli Państwo możliwość wykonywania.

Jest to obsługiwane przez instancji LogTextBox za pośrednictwem [LogTextView (pola IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) konstruktora, gdy po raz pierwszy najedzie wystąpienia LogTextBox Aktywnej zarządzanego kodu, a następnie wywoływania [ LogTextBox (kontekście IAttributeSet, int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) Konstruktor *na tym samym wystąpieniu* po wykonaniem konstruktora Aktywnej.

Kolejność zdarzeń:

1.  Układ XML jest ładowany do [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41).

2.  Android tworzy wykres obiektu układu i tworzy wystąpienie *monodroid.apidemo.LogTextBox* , Aktywnej dla *LogTextBox* .

3.  *Monodroid.apidemo.LogTextBox* Konstruktor wykonuje [android.widget.TextView](http://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) konstruktora.

4.  *TextView* wywołuje konstruktor *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* .

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* wywołuje *LogTextBox.n_getDefaultMovementMethod()* , który wywołuje *TextView.n_GetDefaultMovementMethod()* , który wywołuje [Java.Lang.Object.GetObject&lt;TextView&gt; (JniHandleOwnership.DoNotTransfer obsłużyć)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* sprawdza, czy ma już odpowiedniego języka C# wystąpienie *obsługi* . Jeśli istnieje, jest zwracany. W tym scenariuszu nie istnieje, więc *obiekt.GetObject&lt;T&gt;()* należy go utworzyć.

7.  *Obiekt.GetObject&lt;T&gt;()* szuka *LogTextBox (pola IntPtr, JniHandleOwneship)* konstruktora, wywołuje on, tworzy mapowanie między *obsługi* i utworzone wystąpienie i zwraca utworzone wystąpienie.

8.  *TextView.n_GetDefaultMovementMethod()* wywołuje *LogTextBox.DefaultMovementMethod* metoda pobierająca właściwości.

9.  Formant powraca do *android.widget.TextView* konstruktora, który kończy działanie.

10. *Monodroid.apidemo.LogTextBox* wykonaniem konstruktora, wywołując *TypeManager.Activate()* .

11. *LogTextBox (kontekście IAttributeSet, int)* Konstruktor wykonuje *w jednym wystąpieniu, które są tworzone w (7)* .

12. Jeśli (pola IntPtr, JniHandleOwnership) nie można odnaleźć konstruktora, a następnie System.MissingMethodException](xref:System.MissingMethodException) zostanie zgłoszony.

<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>Wywołania metody Dispose() przedwczesne

Istnieje mapowanie między interfejsem JNI dojścia i odpowiednie wystąpienia języka C#. Java.Lang.Object.Dispose() przerywa to mapowanie. Jeśli uchwyt interfejsem JNI wprowadzi kod zarządzany, po mapowanie zostało przerwane, wygląda aktywacji Java i *(pola IntPtr, JniHandleOwnership)* Konstruktor będą sprawdzane pod kątem i wywołana. Jeśli Konstruktor nie istnieje, wyjątek zostanie zgłoszony.

Na przykład biorąc pod uwagę następujące podklasy zarządzanego wywoływaną Wraper:

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

Jeśli firma Microsoft Utwórz wystąpienie metody Dispose() i spowodować zarządzanych otoka wywoływana zostać ponownie utworzone:

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

Program będzie zdechną:

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

Jeśli zawiera podklasy *(pola IntPtr, JniHandleOwnership)* konstruktora, a następnie *nowe* zostanie utworzone wystąpienie tego typu. W wyniku wystąpienia pojawi się na "utrać" wszystkie dane wystąpienia, jak jest nowe wystąpienie. (Zwróć uwagę, że ma wartość null).

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Tylko *Dispose()* z zarządzanych otoka wywoływana podklasy, gdy wiesz, że obiekt języka Java nie będą już używane lub podklasy nie zawiera żadnych danych wystąpienia i a *(pola IntPtr, JniHandleOwnership)* podano konstruktora.



## <a name="application-startup"></a>Uruchamianie aplikacji

Podczas działania, usługa itp. zostanie uruchomiony, systemu Android zostanie najpierw sprawdzić, czy jest już proces uruchomiony do hostowania działania/service/itd. Jeśli nie ma takiego procesu istnieje, zostanie utworzony nowy proces, [AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-intro.html) odczytu, a typ określony w [ /manifest/application/@android:name ](http://developer.android.com/guide/topics/manifest/application-element.html#nm) atrybutu jest ładowany i uruchomiony. Następnie, wszystkie typy określone przez [ /manifest/application/provider/@android:name ](http://developer.android.com/guide/topics/manifest/provider-element.html#nm) wartości atrybutów są tworzone i mieć ich [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/) wywołana metoda. Punkty zaczepienia platformy Xamarin.Android do tego, dodając *mono. MonoRuntimeProvider* *obiektu ContentProvider* do pliku AndroidManifest.xml w procesie kompilacji. *Mono. MonoRuntimeProvider.attachInfo()* metoda jest odpowiedzialna za ładowanie środowiska uruchomieniowego Mono w procesie.
Każda próba ich używać platformy Mono, przed tym punkcie zakończy się niepowodzeniem. ( *Uwaga*: dlatego typów z podklasami [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) należy podać [(pola IntPtr, JniHandleOwnership) Konstruktor](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103), jak to wystąpienie aplikacji utworzone przed Mono mogą być zainicjowane.)

Po zakończeniu procesu inicjowania `AndroidManifest.xml` konsultacji można znaleźć nazwy klasy działania/service/itp., aby uruchomić. Na przykład [ /manifest/application/activity/@android:name atrybut](http://developer.android.com/guide/topics/manifest/activity-element.html#nm) służy do określania nazwy działania do załadowania. Dla działań, ten typ musi dziedziczyć [android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/).
Określony typ jest ładowany za pośrednictwem [Class.forName()](http://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (który wymaga, aby typ języka Java typu, dlatego wywoływane otoki systemu Android), następnie tworzone jest wystąpienie. Tworzenia wystąpienia otoka wywoływana z systemem Android spowoduje wyzwolenie tworzenia wystąpienia odpowiedniego typu C#. Następnie wywoła android [Activity.onCreate(Bundle)](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , który spowoduje, że odpowiednie [Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) do wywołania, i wszystko jest gotowe zawodach.
