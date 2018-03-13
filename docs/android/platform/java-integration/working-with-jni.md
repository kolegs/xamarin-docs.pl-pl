---
title: Praca z JNI
description: "Xamarin.Android umożliwia pisanie aplikacji systemu Android w języku C# zamiast Java. Kilka zestawów są dostarczane z platformy Xamarin.Android, które zapewniają powiązania dla bibliotek języka Java, w tym Mono.Android.dll i Mono.Android.GoogleMaps.dll. Powiązania nie są dostarczane dla każdego możliwe biblioteka języka Java i powiązania, które znajdują się nie może powiązać każdy typ Java i element członkowski. Aby użyć niepowiązanych typów języka Java i elementów członkowskich, Java interfejsu macierzystego (JNI) mogą być używane. W tym artykule przedstawiono metodę zastosowania JNI wchodzić w interakcje z typów języka Java i elementów członkowskich z aplikacji platformy Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: e9a6f44637b77bf53c3cab00ac5051e6a2f27386
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="working-with-jni"></a>Praca z JNI

_Xamarin.Android umożliwia pisanie aplikacji systemu Android w języku C# zamiast Java. Kilka zestawów są dostarczane z platformy Xamarin.Android, które zapewniają powiązania dla bibliotek języka Java, w tym Mono.Android.dll i Mono.Android.GoogleMaps.dll. Powiązania nie są dostarczane dla każdego możliwe biblioteka języka Java i powiązania, które znajdują się nie może powiązać każdy typ Java i element członkowski. Aby użyć niepowiązanych typów języka Java i elementów członkowskich, Java interfejsu macierzystego (JNI) mogą być używane. W tym artykule przedstawiono metodę zastosowania JNI wchodzić w interakcje z typów języka Java i elementów członkowskich z aplikacji platformy Xamarin.Android._


## <a name="overview"></a>Omówienie

Nie zawsze jest konieczne lub można utworzyć zarządzane można wywołać otoki (MCW) do wywołania kodu języka Java. W wielu przypadkach "wbudowany" JNI jest doskonale akceptowalne i użyteczne do użytku jednorazowe niezwiązanego członków Java. Często jest łatwiejsze umożliwia wywołanie jednej metody w klasie Java niż aby wygenerować powiązanie JAR cały JNI.

Udostępnia Xamarin.Android `Mono.Android.dll` zestawu, który umożliwia powiązanie dla systemu Android w `android.jar` biblioteki. Typy i składniki nie występują w ramach `Mono.Android.dll` i typów nie występują w ramach `android.jar` mogą być używane przez ręczne ich powiązania. Aby powiązać Java typów i członków, należy użyć **natywnego interfejsu Java** (**JNI**) do wyszukiwania typów, Odczyt i zapis pól i wywołania metody.

Jest bardzo podobny do interfejsu API JNI platformie Xamarin.Android `System.Reflection` interfejsu API programu .NET: Umożliwia ono odszukać typy i składniki według nazwy, Odczyt i zapis wartości pól wywołania metod i inne. Można użyć JNI i `Android.Runtime.RegisterAttribute` atrybutu niestandardowego, aby zadeklarować metody wirtualne, które mogą zostać powiązane do obsługi zastępowanie. Interfejsy można powiązać, dzięki czemu mogą one zostać wdrożone w języku C#.

W tym dokumencie opisano:

-  Jak JNI odwołuje się do typów.
-  Jak wyszukiwanie, Odczyt i zapis pól.
-  Jak wyszukiwanie i wywołania metody.
-  Sposób ujawniać metod wirtualnych, aby umożliwić zastępowanie z kodu zarządzanego.
-  Jak uwidacznia interfejsów.



## <a name="requirements"></a>Wymagania

JNI, jak za pośrednictwem [przestrzeni nazw Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/), jest dostępna w każdej wersji platformy Xamarin.Android.
Aby powiązać typów języka Java i interfejsów, należy użyć Xamarin.Android 4.0 lub nowszy.


## <a name="managed-callable-wrappers"></a>Wywoływane otoki zarządzane

A **zarządzane wywoływana otoka** (**MCW**) jest *powiązania* dla klasy Java lub interfejsu, który opakowuje się wszystkie JNI maszyny, aby kod klienta C# nie musisz martwić się o podstawowy złożoność JNI. Większość `Mono.Android.dll` składa się z zarządzanym wywoływane otoki.

Wywoływane otoki zarządzanych służyć do dwóch celów:

1.  Hermetyzuj Użyj JNI tak, aby kod klienta nie musi wiedzieć o podstawowej złożoności.
1.  Umożliwiają podrzędna Klasa Java typów i implementować interfejsów Java.

Pierwszy celem jest wyłącznie dla wygody i hermetyzacji złożoności, aby konsumentów miały proste, zarządzany zestaw klas do użycia. Wymaga to użycia różnych [JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) członków zgodnie z opisem w dalszej części tego artykułu. Należy pamiętać, że zarządzane wywoływane otoki nie są niezbędne &ndash; "wbudowany" Użyj JNI doskonale zaakceptować i jest przydatne w przypadku jednorazowe użycie niepowiązanej członków Java. Podrzędna classing i interfejs wdrażania wymaga użycia zarządzanych wywoływane otoki.



## <a name="android-callable-wrappers"></a>Wywoływane otoki dla systemu android

Android wywoływane otoki (Aktywnej) są wymagane, gdy wykonawczym systemu Android (GRAFIKA) musi wywoływać kod zarządzany; te otoki są wymagane, ponieważ nie można zarejestrować klasy ozdobione w czasie wykonywania.
(W szczególności [DefineClass](http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) JNI funkcja nie jest obsługiwana przez środowisko wykonawcze systemu Android. Wywoływane otoki dla systemu android w związku z tym składają się na brak obsługi rejestracji typu środowiska uruchomieniowego.)

Zawsze, gdy kodu dla systemu Android musi wykonać wirtualny lub interfejsu metodę, która jest zastąpiona lub zaimplementowano w kodzie zarządzanym, Xamarin.Android podać serwer proxy Java, aby ta metoda pobiera wysyłane do odpowiedniego typu zarządzanego. Te typy proxy Java są kodu języka Java mają "w tej samej" klasy podstawowej i listy interfejsu Java, jak typ zarządzany, wdrażanie tego samego konstruktorów i typ deklarujący wszelkich przesłoniętych klasy podstawowej i metod interfejsu.

Wywoływane otoki dla systemu android są generowane przez **monodroid.exe** programu podczas [proces kompilacji](~/android/deploy-test/building-apps/build-process.md)i są generowane dla wszystkich typów dziedziczących (bezpośrednio lub pośrednio) [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/).



### <a name="implementing-interfaces"></a>Implementowanie interfejsów

Czasami podczas może muszą implementować interfejs dla systemu Android (takich jak [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/)).

Wszystkie Android klasy i interfejsy rozszerzenia [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) interfejsu; w związku z tym wszystkie typy Android musi implementować `IJavaObject`.
Xamarin.Android wykorzystuje fakt &ndash; używa `IJavaObject` zapewnienie Android przy użyciu serwera proxy Java (Android wywoływana otoka) dla danego typu zarządzanego. Ponieważ **monodroid.exe** wyszukuje tylko `Java.Lang.Object` podklasy (który musi zaimplementować interfejs `IJavaObject`), tworzenie podklas `Java.Lang.Object` zapewnia sposób implementacji interfejsów w kodzie zarządzanym. Na przykład:

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {
    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig) {
        // implementation goes here...
    }
    public void OnLowMemory () {
        // implementation goes here...
    }
}
```


### <a name="implementation-details"></a>Szczegóły implementacji

*W dalszej części tego artykułu zawiera szczegóły implementacji mogą ulec zmianie bez uprzedzenia* (i przedstawionej tutaj tylko dlatego, deweloperzy mogą być zastanawiasz się, jak to, co dzieje się kulisy).

Na przykład podać następujące źródło C#:

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

**Mandroid.exe** program wygeneruje następujące wywoływana otoka Android:

```java
package mono.samples.helloWorld;

public class HelloAndroid extends android.app.Activity {
    static final String __md_methods;
    static {
        __md_methods =
            "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" +
            "";
        mono.android.Runtime.register (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                HelloAndroid.class,
                __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                "", this, new java.lang.Object[] { });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Zwróć uwagę, czy klasa podstawowa jest zachowywana i deklaracje metody natywnej są udostępniane dla każdej metody, która zostanie zastąpiona w kodzie zarządzanym.



### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute i ExportFieldAttribute

Zazwyczaj Xamarin.Android automatycznie generuje kod języka Java, który obejmuje Aktywnej; tej generacji jest oparta na nazwy klasy i metody, gdy klasa pochodzi od klasy Java i zastępuje istniejące metody Java. Jednak w niektórych scenariuszach generowanie kodu nie jest odpowiedni, przedstawione poniżej:

-   Obsługa systemu android akcji nazwy w układzie atrybutów xml, na przykład [android: onClick](https://developer.xamarin.com/api/member/Android.Views.View+IOnClickListener.OnClick/p/Android.Views.View/) atrybutu XML. Jeśli nie zostanie określony, nadmuchany wystąpienia widoku próby odszukania metody Java.

-   [Java.io.Serializable](http://developer.android.com/reference/java/io/Serializable.html) wymaga interfejsu `readObject` i `writeObject` metody. Ponieważ nie są członkami tego interfejsu, naszych odpowiedniej implementacji zarządzanego nie ujawnia tych metod kodu języka Java.

-   [Android.os.Parcelable](https://developer.xamarin.com/api/type/Android.Os.Parcelable/) interfejsu oczekuje, że klasa implementacji muszą mieć pola statycznego `CREATOR` typu `Parcelable.Creator`. Wygenerowany kod w języku Java wymaga niektórych jawnej pola. W naszym scenariuszu standard nie istnieje sposób do pola danych wyjściowych w kodzie języka Java z kodu zarządzanego.


Ponieważ generowanie kodu nie zapewnia rozwiązanie, aby wygenerować dowolne metody Java z dowolnych nazw, począwszy od platformy Xamarin.Android 4.2, [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/) i [ExportFieldAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportFieldAttribute/) zostały wprowadzono oferowanie rozwiązanie powyższych scenariuszy. Oba atrybuty znajdują się w `Java.Interop` przestrzeni nazw:

-   `ExportAttribute` &ndash; Określa nazwę metody i jego typów oczekiwanego wyjątku (Aby podać jawne "zgłasza wyjątek" w języku Java). Gdy jest używany dla metody, metoda "wyeksportuje" metody Java, która generuje kod wysyłki na zarządzaną metodę odpowiedniego wywołania JNI. Może być używany z `android:onClick` i `java.io.Serializable`.

-   `ExportFieldAttribute` &ndash; Określa nazwę pola. Znajduje się on na metodę, która działa jako inicjator pola. Może być używany z `android.os.Parcelable`.

[ExportAttribute](https://developer.xamarin.com/samples/monodroid/ExportAttribute/) przykładowy projekt ilustruje sposób używania tych atrybutów.


#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>Rozwiązywanie problemów z ExportAttribute i ExportFieldAttribute

-   OPAKOWYWANIE kończy się niepowodzeniem z powodu braku **Mono.Android.Export.dll** &ndash; Jeśli użyto `ExportAttribute` lub `ExportFieldAttribute` w niektórych metod, w tym kod lub zależnej biblioteki, musisz dodać  **Mono.Android.Export.dll**. Ten zestaw jest izolowana do obsługi wywołania zwrotnego kodu w języku Java. Jest oddzielony od **Mono.Android.dll** jako dodatkowe rozmiar dodaje do aplikacji.

-   W kompilacji wydania `MissingMethodException` występuje dla metod eksportu &ndash; kompilacji w wersji `MissingMethodException` występuje dla metod eksportu. (Tego problemu w najnowszej wersji platformy Xamarin.Android.)



### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` i `ExportFieldAttribute` zapewniać funkcje że Java można użyć kodu w czasie wykonywania. Ten kod w czasie wykonywania uzyskuje dostęp do kodu zarządzanego za pośrednictwem wygenerowane metody JNI regulowane przez te atrybuty. W związku z tym nie istnieje metoda Java istniejących wiążąca zarządzaną metodę; w związku z tym metoda Java są generowane na podstawie sygnatury metody zarządzanych.

Ten przypadek nie jest jednak pełni decydującym. Głównie jest to istotne w niektórych zaawansowanych mapowań między typy zarządzane i typów języka Java, takich jak:

-  InputStream
-  OutputStream
-  XmlPullParser
-  XmlResourceParser

Typy, takich jak te są potrzebne dla wyeksportowanych metod `ExportParameterAttribute` należy jawnie odpowiadającego mu parametru lub typ zwracany wartość.



### <a name="annotation-attribute"></a>Atrybut adnotacji

W Xamarin.Android 4.2, możemy przekonwertować `IAnnotation` typów wdrożenia w atrybuty (System.Attribute) oraz dodano obsługę Generowanie adnotacji w języku Java otoki.

Oznacza to kierunkową następujące zmiany:

-   Generator powiązanie generuje `Java.Lang.DeprecatedAttribute` z `java.Lang.Deprecated` (gdy powinien być `[Obsolete]` w kodzie zarządzanym).

-   Nie oznacza to, czy istniejące `Java.Lang.Deprecated` będzie znikają klasy. Te obiekty opartych na języku Java można nadal używane jako zwykły obiektów języka Java (jeśli takie istnieje). Będzie `Deprecated` i `DeprecatedAttribute` klasy.

-   `Java.Lang.DeprecatedAttribute` Klasa jest oznaczona jako `[Annotation]` . Po niestandardowy atrybut, który jest odziedziczone to `[Annotation]` atrybutu, zadanie programu msbuild wygeneruje adnotację Java dla tego atrybutu niestandardowego (@Deprecated) w systemie Android otoki można wywołać (Aktywnej).

-   Adnotacje, zostanie wygenerowany na klas, metod i wyeksportować pola (co to metoda w kod zarządzany).

Jeśli klasa zawierająca (adnotacjami samej klasy lub klasa, która zawiera członków adnotacjami) nie jest zarejestrowany, całą źródłową Klasa Java nie jest generowany, łącznie z adnotacjami. Dla metod, można określić `ExportAttribute` do get, metoda jawnie wygenerowany i opatrzone adnotacjami. Ponadto nie jest funkcja "Generuj" definicję klasy Java adnotacji. Innymi słowy zdefiniuj atrybut niestandardowy zarządzany niektórych adnotację, należy dodać inną bibliotekę JAR zawierający odpowiadającą klasę języka Java w adnotacji. Dodawanie plik źródłowy Java, który definiuje typ adnotacji jest niewystarczająca. Kompilator języka Java nie działa w taki sam sposób jak **stanie**.

Ponadto obowiązują następujące ograniczenia:

-   Ten proces konwersji nie należy wziąć pod uwagę `@Target` adnotacja dla typu adnotacji do tej pory.

-   Atrybuty na właściwość nie działa. Zamiast tego użyj atrybutów dla metody ustawiającej lub metody pobierającej właściwość.



## <a name="class-binding"></a>Powiązanie klasy

Powiązanie klasy oznacza zapisywania zarządzanych wywoływana otoka uprościć wywołania typ źródłowy Java.

Powiązanie metod wirtualnych i abstrakcyjny zezwala na zastępowanie w języku C# wymaga platformy Xamarin.Android 4.0. Jednak dowolnej wersji platformy Xamarin.Android można powiązać metody niewirtualnej, statycznej metody lub metody wirtualne bez obsługi zastąpień.

Powiązanie zwykle zawiera następujące elementy:

-  A [JNI dojście do typu Java związany](#_Looking_up_Java_Types).

-  [JNI pola nazwy i właściwości dla każdego pola powiązane](#_Instance_Fields).

-  [Identyfikatory metody JNI i metody dla każdego powiązany — metoda](#_Instance_Methods).

-  Jeśli wymagana jest podrzędna classing, typ musi mieć [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) atrybut niestandardowy w deklaracji typu z [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) ustawioną `true`.



### <a name="declaring-type-handle"></a>Dojście typu deklarującego

Metody wyszukiwania pola i metody wymagają odwołujących się do swojego typu deklarującego odwołania do obiektu. Według Konwencji jest to przechowywać w formacie `class_ref` pola:

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

Zobacz [odwołania do typu JNI](#_JNI_Type_References) sekcji szczegółowe informacje o `CLASS` tokenu.


### <a name="binding-fields"></a>Powiązywanie pól

Java pola są widoczne jako właściwości języka C#, na przykład pole Java [java.lang.System.in](http://developer.android.com/reference/java/lang/System.html#in) jest powiązany jako właściwość C# [Java.Lang.JavaSystem.In](https://developer.xamarin.com/api/property/Java.Lang.JavaSystem.In/).
Ponadto ponieważ JNI rozróżnia pola statyczne i pól wystąpień, różnych metod można użyć podczas implementowania właściwości.

Pole powiązania obejmuje zestawy trzech metod:

1.  *Pobranie identyfikatora pola* metody. *Pobranie identyfikatora pola* metoda jest odpowiedzialna za zwracanie dojście pola, którego *pobrać wartości pola* i *ustaw wartość pola* użyje metody. Uzyskiwanie identyfikatora pola wymaga, wiedząc, wpisz nazwę pola, deklarowanie i [podpis typu JNI](#_JNI_Type_Signatures) pola.

1.  *Pobrać wartości pola* metody. Te metody wymagają dojścia pola i są odpowiedzialne za odczytywanie wartości pola w języku Java.
    Metoda do użycia zależy od typu pola.

1.  *Ustaw wartość pola* metody. Te metody wymagają dojścia pola i są odpowiedzialne za zapisywanie wartości pola w języku Java. Metoda do użycia zależy od typu pola.


 [Pola statyczne](#_Static_Fields) użyj [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/), `JNIEnv.GetStatic*Field`, i [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/) metody.

 [Wystąpienie pola](#_Instance_Fields) użyj [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/), `JNIEnv.Get*Field`, i [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/) metody.

Na przykład właściwość statyczna `JavaSystem.In` można zaimplementować jako:

```csharp
static IntPtr in_jfieldID;
public static System.IO.Stream In
{
    get {
        if (in_jfieldId == IntPtr.Zero)
            in_jfieldId = JNIEnv.GetStaticFieldID (class_ref, "in", "Ljava/io/InputStream;");
        IntPtr __ret = JNIEnv.GetStaticObjectField (class_ref, in_jfieldId);
        return InputStreamInvoker.FromJniHandle (__ret, JniHandleOwnership.TransferLocalRef);
    }
}
```

Uwaga: Firma Microsoft korzysta z [InputStreamInvoker.FromJniHandle](https://developer.xamarin.com/api/member/Android.Runtime.InputStreamInvoker.FromJniHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) można przekonwertować odwołania JNI do `System.IO.Stream` korzystania z wystąpienia i firma Microsoft `JniHandleOwnership.TransferLocalRef` ponieważ [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) Zwraca lokalnego odwołania.

Duża liczba [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) typy mają `FromJniHandle` metod, które umożliwia konwersję JNI odwołanie do odpowiedniego typu.



### <a name="method-binding"></a>Powiązanie — metoda

Metody języka Java są widoczne jako metody C#, a właściwości języka C#. Na przykład metoda Java [java.lang.Runtime.runFinalizersOnExit](http://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) metody jest powiązany jako [Java.Lang.Runtime.RunFinalizersOnExit](https://developer.xamarin.com/api/member/Java.Lang.Runtime.RunFinalizersOnExit/) metody i [java.lang.Object.getClass ](http://developer.android.com/reference/java/lang/Object.html#getClass) metody jest powiązany jako [Java.Lang.Object.Class](https://developer.xamarin.com/api/property/Java.Lang.Object.Class/) właściwości.

Wywołanie metody jest procesem dwuetapowym:

1.  *Pobierz identyfikator metody* dla metody do wywołania. *Pobierz identyfikator metody* metoda jest odpowiedzialna za zwracanie dojścia metody, który będzie używany przez metodę wywołania metody. Uzyskiwanie identyfikatora — metoda wymaga, wiedząc, wpisz nazwę metody, deklarowanie i [podpis typu JNI](#_JNI_Type_Signatures) metody.

1.  Wywołanie metody.

Tak jak w przypadku pola, metody, które umożliwia uzyskanie identyfikatora — metoda i wywołanie metody różnią się metody statyczne i metody wystąpienia.

[Metody statyczne](#_Static_Methods_1) użyj [JNIEnv.GetStaticMethodID()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/) odszukać parametru id — metoda i używanie `JNIEnv.CallStatic*Method` rodziny metod do wywołania.

[Wystąpienie metody](#_Instance_Methods) użyj [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/) odszukać parametru id — metoda i używanie `JNIEnv.Call*Method` i `JNIEnv.CallNonvirtual*Method` rodzin metod do wywołania.

Metoda powiązania jest potencjalnie więcej niż tylko wywołania metody. Powiązanie — metoda również obejmuje metodę do zastąpienia (dla metody abstrakcyjnej i innej niż końcowa) lub zaimplementowana (dla metody interfejsu). [Interfejsy obsługi dziedziczenia](#_Supporting_Inheritance,_Interfaces_1) sekcji omówiono złożoności obsługi metody wirtualne i metod interfejsu.

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>Metody statyczne

Powiązanie statycznej metody polega na użyciu `JNIEnv.GetStaticMethodID` można uzyskać dojścia metody, przy użyciu odpowiednich `JNIEnv.CallStatic*Method` metody, w zależności od typu zwracanego metody. Poniżej przedstawiono przykład powiązania dla [Runtime.getRuntime](http://developer.android.com/reference/java/lang/Runtime.html#getRuntime()) metody:

```csharp
static IntPtr id_getRuntime;

[Register ("getRuntime", "()Ljava/lang/Runtime;", "")]
public static Java.Lang.Runtime GetRuntime ()
{
    if (id_getRuntime == IntPtr.Zero)
        id_getRuntime = JNIEnv.GetStaticMethodID (class_ref,
                "getRuntime", "()Ljava/lang/Runtime;");

    return Java.Lang.Object.GetObject<Java.Lang.Runtime> (
            JNIEnv.CallStaticObjectMethod  (class_ref, id_getRuntime),
            JniHandleOwnership.TransferLocalRef);
}
```

Należy pamiętać, że dojście metody są przechowywane w polu statycznym, `id_getRuntime`. To zoptymalizować wydajność, dojścia metody nie trzeba można przeszukiwać w każdym wywołaniu. Nie jest konieczne w celu buforowania dojścia metody w ten sposób. Po uzyskaniu dojścia metody [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) służy do wywoływania metody. `JNIEnv.CallStaticObjectMethod` Zwraca `IntPtr` zawierającą dojście zwrócone wystąpienie Java.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) służy do konwertowania dojście Java na wystąpienie silnie typizowany obiekt.



#### <a name="non-virtual-instance-method-binding"></a>Wirtualnego wystąpienia powiązania — metoda

Powiązanie `final` metody wystąpienia lub metodą wystąpienia, które nie wymagają zastępowanie, polega na użyciu `JNIEnv.GetMethodID` można uzyskać dojścia metody, przy użyciu odpowiednich `JNIEnv.Call*Method` metody, w zależności od typu zwracanego metody. Poniżej przedstawiono przykład powiązania dla `Object.Class` właściwości:

```csharp
static IntPtr id_getClass;
public Java.Lang.Class Class {
    get {
        if (id_getClass == IntPtr.Zero)
            id_getClass = JNIEnv.GetMethodID (class_ref, "getClass", "()Ljava/lang/Class;");
        return Java.Lang.Object.GetObject<Java.Lang.Class> (
                JNIEnv.CallObjectMethod (Handle, id_getClass),
                JniHandleOwnership.TransferLocalRef);
    }
}
```

Należy pamiętać, że dojście metody są przechowywane w polu statycznym, `id_getClass`.
To zoptymalizować wydajność, dojścia metody nie trzeba można przeszukiwać w każdym wywołaniu. Nie jest konieczne w celu buforowania dojścia metody w ten sposób. Po uzyskaniu dojścia metody [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) służy do wywoływania metody. `JNIEnv.CallStaticObjectMethod` Zwraca `IntPtr` zawierającą dojście zwrócone wystąpienie Java.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) służy do konwertowania dojście Java na wystąpienie silnie typizowany obiekt.


### <a name="binding-constructors"></a>Powiązanie konstruktorów

Konstruktory są Java metod o tej nazwie `"<init>"`. Tak jak w przypadku metody wystąpienia Java, `JNIEnv.GetMethodID` jest używana do wyszukiwania dojście konstruktora. W przeciwieństwie do metod Java [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/) metody są używane do wywołania, uchwyt metody konstruktora. Wartość zwracana `JNIEnv.NewObject` jest JNI lokalnego odwołania:


```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

Zwykle powiązanie klasy będzie podklasy [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/).
Podczas tworzenie podklas `Java.Lang.Object`, dodatkowe semantycznego wejścia play: `Java.Lang.Object` wystąpienie obsługuje globalnych odwołania do wystąpienia Java za pomocą `Java.Lang.Object.Handle` właściwości.

1.  `Java.Lang.Object` Domyślnego konstruktora przyzna wystąpienia Java.

1.  Jeśli typ ma `RegisterAttribute` , i `RegisterAttribute.DoNotGenerateAcw` jest `true` , następnie wystąpienia `RegisterAttribute.Name` typ jest tworzony za pomocą jego domyślny konstruktor.

1.  W przeciwnym razie [Android wywoływana otoka](~/android/platform/java-integration/android-callable-wrappers.md) (Aktywnej) odpowiadający `this.GetType` zostanie uruchomiony za pośrednictwem jego domyślny konstruktor. Wywoływane otoki dla systemu android są generowane podczas tworzenia pakietu dla każdego `Java.Lang.Object` podklasy, dla którego `RegisterAttribute.DoNotGenerateAcw` nie jest ustawiony na `true`.

Dla typów, które są klasy nie powiązań, jest to oczekiwana semantyczne: Tworzenie wystąpień `Mono.Samples.HelloWorld.HelloAndroid` C# wystąpienia konstruować Java `mono.samples.helloworld.HelloAndroid` wystąpienia, czyli wygenerowanego wywoływana otoka systemu Android.

Dla powiązania klas może to być poprawne zachowanie, jeśli typ Java zawiera konstruktora domyślnego i/lub brak innych konstruktora musi być wywoływane. W przeciwnym razie konstruktora należy podać który wykonuje następujące czynności:

1.  Wywoływanie [Java.Lang.Object (IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) zamiast domyślnej `Java.Lang.Object` konstruktora. Jest to niezbędne, aby uniknąć tworzenia nowego wystąpienia Java.

1.  Sprawdź wartość [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) przed utworzeniem wystąpienia Java. `Object.Handle` Właściwość będzie mieć wartości inne niż `IntPtr.Zero` Android wywoływana otoka został skonstruowany w kodzie języka Java i powiązanie klasy jest budowany zawiera utworzone wystąpienie wywoływana otoka systemu Android. Na przykład, gdy Android tworzy `mono.samples.helloworld.HelloAndroid` wystąpienia, Android wywoływana otoka zostanie utworzony, w pierwszej i Java `HelloAndroid` Konstruktor spowoduje utworzenie wystąpienia odpowiadającego `Mono.Samples.HelloWorld.HelloAndroid` typu z `Object.Handle` właściwości ustawiany przed wykonaniem konstruktora wystąpienia Java.

1.  Jeśli typ bieżącego środowiska uruchomieniowego nie jest taka sama jak deklarujący typu, a następnie wystąpienia odpowiednie dla systemu Android wywoływana otoka muszą być tworzone i użyj [Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) do przechowywania zwracane przez dojście [ JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/).

1.  Jeśli typ bieżącego środowiska uruchomieniowego jest taki sam jak typ deklarujący, następnie wywołać konstruktora, Java i użyj [Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) do przechowywania zwracane przez dojście `JNIEnv.NewInstance` .


Rozważmy na przykład [java.lang.Integer(int)](http://developer.android.com/reference/java/lang/Integer.html#Integer(int)) konstruktora. To jest powiązany jako:

```csharp
// Cache the constructor's method handle for later use
static IntPtr id_ctor_I;

// Need [Register] for subclassing
// RegisterAttribute.Name is always ".ctor"
// RegisterAttribute.Signature is tye JNI type signature of constructor
// RegisterAttribute.Connector is ignored; use ""
[Register (".ctor", "(I)V", "")]
public Integer (int value)
    // 1. Prevent Object default constructor execution
    : base (IntPtr.Zero, JniHandleOwnership.DoNotTransfer)
{
    // 2. Don't allocate Java instance if already allocated
    if (Handle != IntPtr.Zero)
        return;

    // 3. Derived type? Create Android Callable Wrapper
    if (GetType () != typeof (Integer)) {
        SetHandle (
                Android.Runtime.JNIEnv.CreateInstance (GetType (), "(I)V", new JValue (value)),
                JniHandleOwnership.TransferLocalRef);
        return;
    }

    // 4. Declaring type: lookup &amp; cache method id...
    if (id_ctor_I == IntPtr.Zero)
        id_ctor_I = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
    // ...then create the Java instance and store
    SetHandle (
            JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value)),
            JniHandleOwnership.TransferLocalRef);
}
```

[JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/) metody są pomocnicy do wykonania `JNIEnv.FindClass`, `JNIEnv.GetMethodID`, `JNIEnv.NewObject`, i `JNIEnv.DeleteGlobalReference` na wartość zwracana z `JNIEnv.FindClass`. Zobacz następną sekcję, aby uzyskać szczegółowe informacje.

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>Obsługi dziedziczenia — interfejsy

Tworzenie podklas typu Java lub implementowania interfejsu Java wymaga wygenerowania [Android wywoływane otoki](~/android/platform/java-integration/android-callable-wrappers.md) (ACWs) generowane dla każdego `Java.Lang.Object` podklasy podczas procesu tworzenia pakietów. Generowanie Aktywnej są kontrolowane poprzez [Android.Runtime.RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) atrybutu niestandardowego.

Dla typów C# `[Register]` konstruktora atrybutów niestandardowych wymaga jednego argumentu: [JNI uproszczone odwołanie do typu](#_Simplified_Type_References_1) for Java odpowiedniego typu. Dzięki temu, zapewniając różnych nazw między Java i C#.

Starszą niż Xamarin.Android 4.0 `[Register]` atrybutu niestandardowego nie jest dostępny do "alias" istniejących typów języka Java. Jest to spowodowane procesu tworzenia Aktywnej wygenerowanie ACWs dla każdego `Java.Lang.Object` podklasy napotkano.

Xamarin.Android 4.0 wprowadzono [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) właściwości. Ta właściwość powoduje, że proces generowania Aktywnej *pominąć* adnotacjami typu, umożliwiając deklaracji nowego zarządzanego wywoływane otoki który ACWs generowane w czasie tworzenia pakietu nie spowoduje. Dzięki temu powiązanie istniejących typów języka Java. Dla wystąpienia, należy wziąć pod uwagę następujące prostą klasę Java `Adder`, która zawiera jedną metodę `add`, który dodaje do liczb całkowitych i zwraca wynik:

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

`Adder` Typ może zostać powiązany jako:

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public partial class Adder : Java.Lang.Object {
    static IntPtr class_ref = JNIEnv.FindClass ( "mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }
}
partial class ManagedAdder : Adder {
}
```

W tym miejscu `Adder` typ języka C# *aliasy* `Adder` typu Java. `[Register]` Atrybut służy do określania nazwy JNI `mono.android.test.Adder` typu Java i `DoNotGenerateAcw` właściwość jest używana do Aktywnej generation powstrzymanie. Spowoduje to wygenerowanie Aktywnej dla `ManagedAdder` typ, który prawidłowo podklasy `mono.android.test.Adder` typu. Jeśli `RegisterAttribute.DoNotGenerateAcw` nie użyto właściwości, a następnie proces kompilacji platformy Xamarin.Android będzie mieć wygenerował nowy `mono.android.test.Adder` typu Java. To może spowodować błędy kompilacji jako `mono.android.test.Adder` typu będą obecne dwukrotnie w dwóch osobnych plikach.



### <a name="binding-virtual-methods"></a>Metody wirtualne powiązania

`ManagedAdder` podklasy Java `Adder` typu, ale nie ma zastosowanie szczególnie: C# `Adder` typu nie definiuje żadnych metod wirtualnych, więc `ManagedAdder` nie może zastąpić żadnych czynności.

Powiązanie `virtual` metody, aby zezwolić na zastąpienie przez podklasy wymaga kilka kwestii, które muszą być przeprowadzone, które kwalifikują się do następujących dwóch kategorii:

1.  **Powiązanie — metoda**

1.  **Rejestracja — metoda**


#### <a name="method-binding"></a>Powiązanie — metoda

Powiązanie metoda wymaga dodatkowo dwóch członków pomocy technicznej dla C# `Adder` definicji: `ThresholdType`, i `ThresholdClass`.

##### <a name="thresholdtype"></a>ThresholdType

`ThresholdType` Właściwość zwraca typ bieżącego powiązania:

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType` jest używany podczas wiązania metody do określania, kiedy należy wykonywać wirtualnego a metody niewirtualnej wysyłania. Należy zawsze zwracają `System.Type` wystąpienia, który odpowiada C# typ deklarujący.

##### <a name="thresholdclass"></a>ThresholdClass

`ThresholdClass` Właściwość zwraca odwołania do klasy JNI dla powiązania typu:

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` jest używany w powiązanie metody podczas wywoływania metody-virtual.

#### <a name="binding-implementation"></a>Implementacja powiązania

Implementacja metody powiązania jest odpowiedzialny za środowiska uruchomieniowego wywołanie metody Java. Zawiera także `[Register]` deklaracji atrybutu niestandardowego, która jest częścią rejestracji — metoda i zostanie dokładnie omówione w sekcji Metoda rejestracji:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }
}
```

`id_add` Pole zawiera identyfikator metody dla metody Java do wywołania. `id_add` Wartości są uzyskiwane z `JNIEnv.GetMethodID`, który wymaga deklarujący klasy (`class_ref`), nazwę metody Java (`"add"`) i podpis JNI metody (`"(II)I"`).

Po uzyskaniu ID metody `GetType` jest porównywany `ThresholdType` ustalenie, czy konieczne jest wirtualna ani wirtualna z systemem innym niż wysyłania. Wirtualne wysyłania jest wymagany, gdy `GetType` odpowiada `ThresholdType`, jako `Handle` może odwoływać się do podklasy przydzielone Java, która zastępuje metodę.

Gdy `GetType` nie jest zgodny `ThresholdType`, `Adder` zostały podklasy (np. przez `ManagedAdder`) i `Adder.Add` implementacji zostanie wywołany tylko, jeśli wywoływane podklasy `base.Add`. Jest to w przypadku wysyłania-virtual, czyli gdzie `ThresholdClass` polega na. `ThresholdClass` Określa, która Klasa Java zapewni implementacja metody do wywołania.



#### <a name="method-registration"></a>Rejestracja — metoda

Załóżmy, zostały zaktualizowane `ManagedAdder` definicji, które zastępują `Adder.Add` metody:

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

Odwołania, który `Adder.Add` miał `[Register]` atrybutu niestandardowego:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

`[Register]` Konstruktora atrybutów niestandardowych akceptuje trzy wartości:

1.  Nazwa metody Java `"add"` w takim przypadku.

1.  Podpis typu metody, JNI `"(II)I"` w takim przypadku.

1.  *Metody łącznika* , `GetAddHandler` w takim przypadku.
    Metody łącznika zostanie dokładnie omówione później.


Pierwsze dwa parametry Pozwól, aby wygenerować deklaracji metody, aby przesłonić metodę proces generowania Aktywnej. Wynikowa Aktywnej zawiera niektóre z następującym kodem:

```csharp
public class ManagedAdder extends mono.android.test.Adder {
    static final String __md_methods;
    static {
        __md_methods = "n_add:(II)I:GetAddHandler\n" +
            "";
        mono.android.Runtime.register (...);
    }
    @Override
    public int add (int p0, int p1) {
        return n_add (p0, p1);
    }
    private native int n_add (int p0, int p1);
    // ...
}
```

Należy pamiętać, że `@Override` metoda jest zadeklarowana, który deleguje do `n_`-prefiksem metody o tej samej nazwie. To upewnij się, że gdy wywołuje kodu języka Java `ManagedAdder.add`, `ManagedAdder.n_add` zostanie wywołany, które umożliwią zastępowanie C# `ManagedAdder.Add` metody do wykonania.

W związku z tym najważniejszych pytanie: jak jest `ManagedAdder.n_add` argumentów podłączono do `ManagedAdder.Add`?

Java `native` metody zostały zarejestrowane ze środowiskiem uruchomieniowym Java (Android środowiska wykonawczego) za pośrednictwem [funkcja JNI RegisterNatives](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734).
`RegisterNatives` Pobiera tablicę struktury zawierające nazwę metody Java podpis typu JNI i wskaźnik funkcji do wywołania następuje [JNI konwencji wywoływania](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715).
Wskaźnik funkcji musi być funkcja, która przyjmuje dwa argumenty wskaźnika parametrów metody. Java `ManagedAdder.n_add` metoda musi być implementowana przez funkcję, która ma następujące prototypu C:

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android nie ujawnia `RegisterNatives` metody. Zamiast tego Aktywnej i MCW razem zawierają informacje niezbędne do wywołania `RegisterNatives`: Aktywnej zawiera nazwę metody i podpis typu JNI, tylko brakujący element jest wskaźnik funkcji, aby przyłączyć.

Jest to, gdy *metody łącznika* polega na. Trzeci `[Register]` parametr atrybut niestandardowy jest nazwą metody zdefiniowanej w zarejestrowany typ lub klasę podstawową zarejestrowanego typu, który akceptuje Brak parametrów i zwraca `System.Delegate`. Zwrócona `System.Delegate` z kolei odwołuje się do metody, która ma poprawnej sygnatury funkcji JNI. Na koniec metoda łącznika zwraca delegata *musi* prowadzić do katalogu głównego, aby wykaz Globalny nie zbieraj, jak obiekt delegowany jest świadczona języka Java.

```csharp
#pragma warning disable 0169
static Delegate cb_add;
// This method must match the third parameter of the [Register]
// custom attribute, must be static, must return System.Delegate,
// and must accept no parameters.
static Delegate GetAddHandler ()
{
    if (cb_add == null)
        cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
    return cb_add;
}
// This method is registered with JNI.
static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
{
    Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
    return __this.Add (a, b);
}
#pragma warning restore 0169
```

`GetAddHandler` Metoda tworzy `Func<IntPtr, IntPtr, int, int,
int>` delegata, który odwołuje się do `n_Add` następnie wywołuje metodę, [JNINativeWrapper.CreateDelegate](https://developer.xamarin.com/api/member/Android.Runtime.JNINativeWrapper.CreateDelegate/).
`JNINativeWrapper.CreateDelegate` opakowuje podanej metody w bloku try/catch, tak aby wszystkie nieobsługiwane wyjątki są obsługiwane i spowoduje zwiększenie [AndroidEvent.UnhandledExceptionRaiser](https://developer.xamarin.com/api/event/Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser/) zdarzeń. Delegat wynikowe są przechowywane w statycznych `cb_add` zmiennej, dzięki czemu GC nie spowoduje zwolnienia obiektu delegowanego.

Na koniec `n_Add` metoda jest odpowiedzialna za organizowanie parametry JNI odpowiednie typy zarządzane, a następnie wywołaj metodę delegowania.

Uwaga: Używaj `JniHandleOwnership.DoNotTransfer` podczas uzyskiwania MCW za pośrednictwem wystąpienia Java. Traktowanie ich jako lokalnego odwołania (i w związku z tym wywoływania `JNIEnv.DeleteLocalRef`) spowoduje przerwanie zarządzane —&gt; Java -&gt; zarządzane przejścia stosu.



### <a name="complete-adder-binding"></a>Zakończenie metoda dodająca powiązania

Pełną zarządzane powiązania dla `mono.android.tests.Adder` jest typu:

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public class Adder : Java.Lang.Object {

    static IntPtr class_ref = JNIEnv.FindClass ("mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    protected override Type ThresholdType {
        get {return typeof (Adder);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

#region Add
    static IntPtr id_add;

    [Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }

#pragma warning disable 0169
    static Delegate cb_add;
    static Delegate GetAddHandler ()
    {
        if (cb_add == null)
            cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
        return cb_add;
    }

    static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
    {
        Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
        return __this.Add (a, b);
    }
#pragma warning restore 0169
#endregion
}
```



### <a name="restrictions"></a>Ograniczenia

Podczas zapisywania typu, które spełniają następujące kryteria:

1.  Podklasy `Java.Lang.Object`

1.  Ma `[Register]` atrybutu niestandardowego

1.  `RegisterAttribute.DoNotGenerateAcw` jest `true`


Następnie interakcji GC typ *nie mogą* ma wszystkie pola, które mogą odwoływać się do `Java.Lang.Object` lub `Java.Lang.Object` podklasy w czasie wykonywania. Na przykład pola typu `System.Object` i dowolny typ interfejsu nie są dozwolone. Typy, które nie mogą odwoływać się `Java.Lang.Object` wystąpienia są dozwolone, takich jak `System.String` i `List<int>`. To ograniczenie jest zapobiec kolekcji przedwczesny obiektu przez wykaz Globalny.

Jeśli typ musi zawierać pole wystąpienia, które mogą odwoływać się do `Java.Lang.Object` wystąpienie musi być typ pola `System.WeakReference` lub `GCHandle`.



## <a name="binding-abstract-methods"></a>Powiązanie metody abstrakcyjne

Powiązanie `abstract` metod jest w przeważającej mierze identyczny jak powiązania metodach wirtualnych. Istnieją tylko dwa różnice:

1.  Abstrakcyjna metoda jest abstrakcyjna. Nadal zachowuje `[Register]` atrybut i skojarzone metody rejestracji powiązanie — metoda po prostu zostanie przeniesiony do `Invoker` typu.

1.  Niż `abstract` `Invoker` tworzony jest typ, który podklasy typ abstrakcyjny. `Invoker` Typu przesłonięcie metody wszystkie abstrakcyjne zadeklarowana w klasie podstawowej i implementacji przesłoniętych znajduje implementacji metody powiązanie, mimo że można zignorować w przypadku wysyłania-virtual.


Załóżmy na przykład, że powyższe `mono.android.test.Adder.add` zostały metody `abstract`. Zmienić powiązanie C#, aby `Adder.Add` zostały abstract i nowy `AdderInvoker` typu byłoby zdefiniowane, które wykonane `Adder.Add`:

```csharp
partial class Adder {
    [Register ("add", "(II)I", "GetAddHandler")]
    public abstract int Add (int a, int b);

    // The Method Registration machinery is identical to the
    // virtual method case...
}

partial class AdderInvoker : Adder {
    public AdderInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    static IntPtr id_add;
    public override int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
    }
}
```

`Invoker` Typu jest wymagane tylko podczas uzyskiwania JNI odwołania do wystąpienia utworzone w języku Java.


## <a name="binding-interfaces"></a>Powiązanie interfejsów

Interfejsy powiązania jest podobny do wiązania klasy zawierające metody wirtualne, ale wiele charakterystyki różnią się w sposób niewielkie (i nie tak niewielkie). Należy rozważyć [deklaracji interfejsu Java](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14):

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

Interfejs powiązania ma dwie części: definicja interfejsu C# i definicji wywołujący dla interfejsu.



### <a name="interface-definition"></a>Definicja interfejsu

Definicja interfejsu C#, należy spełnić następujące wymagania:

-   Definicja interfejsu muszą mieć `[Register]` atrybutu niestandardowego.

-   Interfejs musi rozszerzać `IJavaObject interface`.
    Błąd w tym celu uniemożliwi ACWs: dziedziczenie z interfejsu Java.

-   Każda metoda interfejsu musi zawierać `[Register]` atrybut określający z odpowiednią nazwą metody Java, podpisu JNI i metody łącznika.

-   Metoda łącznika również określić typ, który metody łącznika może znajdować się na.

Podczas tworzenia wiązania `abstract` i `virtual` metody, metoda łącznik będzie przeszukiwana w hierarchii dziedziczenia rejestrowanego typu. Interfejsy nie mogą mieć żadnych metod zawierających treści, więc to nie zadziała, w związku z tym wymagania, które można określić typu wskazujący, gdzie znajduje się metody łącznika. Typ jest określany w ciągu metody łącznika po dwukropku `':'`, a musi być kwalifikowana nazwa typu zestawu zawierającego typ element wywołujący.

Deklaracje metody interfejsu są tłumaczenia odpowiedniego języka Java metoda przy użyciu *zgodne* typów. Typy wbudowane Java, niezgodne typy są odpowiednie C# typy, np. Java `int` C# `int`. Dla typów odwołań obsługiwany typ jest typem, który może zapewnić dojścia JNI odpowiedniego typu Java.

Elementy członkowskie interfejsu nie będzie można bezpośrednio wywoływany przez Java &ndash; wywołania będzie udziału za pośrednictwem typu wywołujący &ndash; , niektóre dużą elastyczność jest dozwolone.

Interfejs Java postęp może być [zadeklarowany w języku C# jako](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83):

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Powiadomienie powyżej, że firma Microsoft mapy Java `int[]` parametr [JavaArray&lt;int&gt;](https://developer.xamarin.com/api/type/Android.Runtime.JavaArray%601/).
Nie jest to konieczne: Firma Microsoft może mieć powiązany go C# `int[]`, lub `IList<int>`, lub coś jeszcze całkowicie. Wybrano dowolnego typu, `Invoker` musi mieć możliwość umożliwiło Java `int[]` typu dla wywołania.


### <a name="invoker-definition"></a>Element wywołujący definicji

`Invoker` Definicji typu musi dziedziczyć `Java.Lang.Object`, zaimplementuj interfejs odpowiednie i podaj wszystkie metody połączenia, do którego odwołuje się definicja interfejsu. Istnieje jeden więcej sugestii, która różni się od powiązanie klasy: `class_ref` identyfikatory pól i metody powinny należeć do wystąpienia, nie statycznych elementów członkowskich.

Przyczyna preferowanie elementów członkowskich wystąpień związana z `JNIEnv.GetMethodID` zachowanie w środowisku wykonawczym systemu Android. (Może to być również zachowanie Java; nie została przetestowana.) `JNIEnv.GetMethodID` zwraca wartość null, podczas wyszukiwania metodę, która pochodzi z zaimplementowanym interfejsem i nie zadeklarowana interfejsu. Należy wziąć pod uwagę [java.util.SortedMap&lt;K, V&gt; ](http://developer.android.com/reference/java/util/SortedMap.html) interfejsu Java, która implementuje [java.util.Map&lt;K, V&gt; ](http://developer.android.com/reference/java/util/Map.html) interfejsu. Mapa [wyczyść](http://developer.android.com/reference/java/util/Map.html#clear()) metody, w związku z tym pozornie rozsądnym `Invoker` definicja SortedMap może być:

```csharp
// Fails at runtime. DO NOT FOLLOW
partial class ISortedMapInvoker : Java.Lang.Object, ISortedMap {
    static IntPtr class_ref = JNIEnv.FindClass ("java/util/SortedMap");
    static IntPtr id_clear;
    public void Clear()
    {
        if (id_clear == IntPtr.Zero)
            id_clear = JNIEnv.GetMethodID(class_ref, "clear", "()V");
        JNIEnv.CallVoidMethod(Handle, id_clear);
    }
     // ...
}
```

Powyższe będzie działać, ponieważ `JNIEnv.GetMethodID` zwróci `null` podczas wyszukiwania `Map.clear` formę `SortedMap` wystąpienie klasy.

Istnieją dwa rozwiązania to: śledzenie który interfejs każdej metody pochodzi z i mieć `class_ref` dla każdego interfejsu, lub zachować wszystko jako elementów członkowskich wystąpień i wyszukiwania metody w typie klas pochodnych większość, nie typem interfejsu. Drugie odbywa się **Mono.Android.dll**.

Definicja wywołujący ma sześć sekcje: konstruktora, `Dispose` metody `ThresholdType` i `ThresholdClass` członków, `GetObject` metodę, implementacja metody interfejsu i implementacja metody łącznika.



#### <a name="constructor"></a>Konstruktor

Konstruktor musi wyszukać klasa środowiska uruchomieniowego wystąpienia wywoływanego i przechowywane w wystąpieniu klasy środowiska uruchomieniowego `class_ref` pola:

```csharp
partial class IAdderProgressInvoker {
    IntPtr class_ref;
    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref   = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }
}
```

Uwaga: `Handle` właściwości należy użyć w treści konstruktora i nie `handle` parametru na Android 4.0 `handle` parametr może być nieprawidłowy, po zakończeniu konstruktora podstawowego wykonywania.


#### <a name="dispose-method"></a>Dispose — Metoda

`Dispose` — Metoda wymaga zwolnienia odwołania do globalnego przydzielone w Konstruktorze:

```csharp
partial class IAdderProgressInvoker {
    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }
}
```


#### <a name="thresholdtype-and-thresholdclass"></a>ThresholdType i ThresholdClass

`ThresholdType` i `ThresholdClass` elementów członkowskich są takie same, co znaleziono w powiązaniu klasy:

```csharp
partial class IAdderProgressInvoker {
    protected override Type ThresholdType {
        get {
            return typeof (IAdderProgressInvoker);
        }
    }
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```


#### <a name="getobject-method"></a>GetObject — Metoda

Statycznego `GetObject` metody jest wymagany do obsługi [Extensions.JavaCast&lt;T&gt;()](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/):

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```


#### <a name="interface-methods"></a>Metody interfejsu

Co metoda interfejsu musi mieć implementacji, które wywołuje odpowiedniej metody Java za pomocą JNI:

```csharp
partial class IAdderProgressInvoker {
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd", "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd, new JValue (JNIEnv.ToJniHandle (values)), new JValue (currentIndex), new JValue (currentSum));
    }
}
```



#### <a name="connector-methods"></a>Metody łącznika

Metody łącznika i infrastrukturę obsługującą są odpowiedzialne za organizowanie parametry JNI odpowiednie typy C#. Java `int[]` parametr zostanie przekazany jako JNI `jintArray`, który jest `IntPtr` w języku C#. `IntPtr` Muszą być przekazywane do `JavaArray<int>` aby zapewnić obsługę wywoływania interfejsu C#:

```csharp
partial class IAdderProgressInvoker {
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
}
```

Jeśli `int[]` byłoby preferowaną za pośrednictwem `JavaList<int>`, następnie [JNIEnv.GetArray()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetArray/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership%2cSystem.Type)) można zamiast tego użyć:

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

Zauważ, że `JNIEnv.GetArray` kopiuje całą macierz między maszynami wirtualnymi, więc dla dużych tablic może to spowodować partii dodano wykorzystania GC.


### <a name="complete-invoker-definition"></a>Definicja ukończenia wywołującego

[Ukończenia definicji IAdderProgressInvoker](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88):

```csharp
class IAdderProgressInvoker : Java.Lang.Object, IAdderProgress {

    IntPtr class_ref;

    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }

    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }

    protected override Type ThresholdType {
        get {return typeof (IAdderProgressInvoker);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }

#region OnAdd
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd",
                    "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd,
                new JValue (JNIEnv.ToJniHandle (values)),
                new JValue (currentIndex),
new JValue (currentSum));
    }

#pragma warning disable 0169
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
#pragma warning restore 0169
#endregion
}
```



## <a name="jni-object-references"></a>Odwołania do obiektów JNI

Zwraca wiele metod JNIEnv *JNI* *odwołania do obiektu*, które są podobne do `GCHandle`s. JNI udostępnia trzy typy odwołań do obiektów: odwołania lokalne, globalne odwołania i słabe odwołania globalnego. Wszystkie trzy są reprezentowane jako `System.IntPtr`, *, ale* (zgodnie z sekcji typy funkcji JNI) nie wszystkie `IntPtr`s zwrócony z `JNIEnv` metody są odwołania. Na przykład [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/) zwraca `IntPtr`, ale nie zwraca odwołanie do obiektu, zwraca `jmethodID`. Zapoznaj się [dokumentację funkcji JNI](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html) szczegółowe informacje.

Odwołania lokalne są tworzone przez *większość* metod tworzenia odwołania.
Android umożliwia tylko ograniczonej liczby lokalnego odwołania do istnieje w danym momencie, zwykle 512. Można usunąć odwołania lokalnych za pomocą [JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/).
W odróżnieniu od JNI nie odwołują się metody JNIEnv odwołującego zwracany obiekt zwracać lokalnego odwołań; [JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/) zwraca *globalne* odwołania. Zdecydowanie zaleca się usunąć odwołania lokalne tak szybko jak to możliwe, prawdopodobnie tworząc [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) wokół obiektu i określając `JniHandleOwnership.TransferLocalRef` do [Java.Lang.Object (IntPtr. Obsługa JniHandleOwnership transfer)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) konstruktora.

Globalne odwołania są tworzone przez [JNIEnv.NewGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewGlobalRef/) i [JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/).
Może zostać zniszczone z [JNIEnv.DeleteGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteGlobalRef/).
Emulatory mają limit 2000 oczekujących odwołań globalnego, gdy urządzenia sprzętowe mają limit około 52,000 odwołań do globalnych.

Słabe odwołania globalnych są dostępne tylko w Android wersja 2.2 (Froyo) lub nowszym. Słabe odwołania globalnych można usunąć z [JNIEnv.DeleteWeakGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteWeakGlobalRef/(System.IntPtr)).


### <a name="dealing-with-jni-local-references"></a>Zajmujących się JNI odwołania lokalne

[JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/), [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/), [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/), [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/)i [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) metody zwracają `IntPtr` zawierającą JNI lokalnego odwołania do obiektu Java lub `IntPtr.Zero` Jeśli zwrócony Java `null`. Z powodu ograniczonej liczby odwołania lokalne, które mogą być oczekujących na po (zapisów 512), jest pożądane, aby upewnić się, że odwołania są usuwane w odpowiednim czasie. Istnieją trzy sposoby, które można przetwarzać odwołania lokalne: jawne usuwanie, tworzenie `Java.Lang.Object` wystąpienie do przechowywania ich i użycie `Java.Lang.Object.GetObject<T>()` do tworzenia zarządzanego wywoływana otoka wokół nich.



### <a name="explicitly-deleting-local-references"></a>Jawne usuwanie odwołania lokalne

[JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) służy do usuwania odwołania lokalne. Po usunięciu lokalnego odwołania nie może być używane, więc należy uważać, aby upewnić się, że `JNIEnv.DeleteLocalRef` jest ostatnim zadaniem, które odbywa się za pomocą lokalnego odwołania.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```



### <a name="wrapping-with-javalangobject"></a>Zawijanie z Java.Lang.Object

`Java.Lang.Object` udostępnia [Java.Lang.Object (uchwyt IntPtr, JniHandleOwnership transfer)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) konstruktora, który może służyć do opakowywania odwołania do istniejących JNI. [JniHandleOwnership](https://developer.xamarin.com/api/type/Android.Runtime.JniHandleOwnership/) parametr określa sposób `IntPtr` parametr powinien być traktowany:

-   [JniHandleOwnership.DoNotTransfer](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.DoNotTransfer/) &ndash; utworzony `Java.Lang.Object` wystąpienia utworzy nowe odwołanie globalnych z `handle` parametru i `handle` niezmieniona.
    Element wywołujący jest odpowiedzialny zwalnianie `handle` , jeśli to konieczne.

-   [JniHandleOwnership.TransferLocalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; utworzony `Java.Lang.Object` wystąpienia utworzy nowe odwołanie globalnych z `handle` parametru i `handle` jest usuwany z [JNIEnv.DeleteLocalRef ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) . Obiekt wywołujący nie należy zwolnić `handle` i nie mogą używać `handle` po zakończeniu konstruktora wykonywania.

-   [JniHandleOwnership.TransferGlobalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; utworzony `Java.Lang.Object` wystąpienie ma przejąć na własność `handle` parametru. Obiekt wywołujący nie należy zwolnić `handle` .


Ponieważ metody wywołania metody JNI zwracają lokalny system plików refs, `JniHandleOwnership.TransferLocalRef` jest zwykle używane:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

Utworzone odwołanie globalne nie zostanie zwolniona dopóki `Java.Lang.Object` wystąpienie jest bezużytecznych. Jeśli jesteś w stanie, usuwanie wystąpienia zwolnić odwołania globalnej, przyspieszanie wyrzucania:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```



### <a name="using-javalangobjectgetobjectlttgt"></a>Przy użyciu Java.Lang.Object.GetObject&lt;T&gt;)

`Java.Lang.Object` udostępnia [Java.Lang.Object.GetObject&lt;T&gt;(dojście IntPtr JniHandleOwnership transfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) metodę, która może służyć do tworzenia zarządzanego wywoływana otoka określonego typu.

Typ `T` należy spełnić następujące wymagania:

1.  `T` musi być typem referencyjnym.

1.  `T` musi implementować `IJavaObject` interfejsu.

1.  Jeśli `T` nie jest klasy lub interfejsu abstrakcyjnego, następnie `T` podać konstruktora z typami parametrów `(IntPtr,
    JniHandleOwnership)` .

1.  Jeśli `T` jest klasą abstrakcyjną lub interfejsu, Brak *musi* można *wywołujący* dostępne dla `T` . Element wywołujący jest typu nieabstrakcyjnej, która dziedziczy `T` lub implementuje `T` , i ma taką samą nazwę jak `T` z sufiksem wywołujący. Na przykład, jeśli T jest interfejsem `Java.Lang.IRunnable` , następnie typu `Java.Lang.IRunnableInvoker` musi istnieć i musi zawierać wymaganych `(IntPtr,
    JniHandleOwnership)` konstruktora.


Ponieważ metody wywołania metody JNI zwracają lokalny system plików refs, `JniHandleOwnership.TransferLocalRef` jest zwykle używane:

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Wyszukanie typami Java

Do wyszukiwania, pole lub metoda JNI, typ deklarujący pola lub metody musi można najpierw wyszukiwane. [Android.Runtime.JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)) metoda jest używana do wyszukiwania typów języka Java. Parametr ciągu ma *uproszczone odwołanie do typu* lub *pełny typ odwołania* dla typu Java. Zobacz [odwołania do typu JNI sekcji](#_JNI_Type_References) szczegółowe informacje o uproszczonym i pełny typ odwołania.

Uwaga: w odróżnieniu od każdego innych `JNIEnv` metodę, która zwraca wystąpienia obiektu `FindClass` zwraca odwołania globalnej lokalnego odwołania.

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>Pola wystąpienia

Pola są manipulować za pośrednictwem *pola identyfikatorów*. Identyfikatory pola są otrzymywane za pośrednictwem [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/), co wymaga klasy zdefiniowanej w nazwę pola, pole i [podpis typu JNI](#JNI_Type_Signatures) pola.

Identyfikatory pola zbędna, nie ma zostać zwolniony i są prawidłowe, tak długo, jak jest ładowany z odpowiadającym typem Java. (Android nie obsługuje obecnie klasy zwalnianie.)

Istnieją dwa zestawy metody pól wystąpień: jeden do odczytywania pól wystąpień i jeden dla zapisywania pól wystąpień. Wszystkie zestawy metod wymagają Identyfikatora pola do odczytu lub zapisu wartości pola.


### <a name="reading-instance-field-values"></a>Odczytywania wartości pola wystąpienia

Zbiór metod do odczytywania wartości pola wystąpienia jest zgodny ze wzorcem nazewnictwa:

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```
gdzie `*` jest typ pola:

-   [JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/) &ndash; odczytać wartości wszystkie pola wystąpienia, który nie jest typem wbudowanej funkcji, takich jak `java.lang.Object` , tablic i typów interfejsów. Wartość zwracana jest JNI lokalnego odwołania.

-   [JNIEnv.GetBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetBooleanField/) &ndash; odczytać wartość `bool` wystąpienia pól.

-   [JNIEnv.GetByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetByteField/) &ndash; odczytać wartość `sbyte` wystąpienia pól.

-   [JNIEnv.GetCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetCharField/) &ndash; odczytać wartość `char` wystąpienia pól.

-   [JNIEnv.GetShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetShortField/) &ndash; odczytać wartość `short` wystąpienia pól.

-   [JNIEnv.GetIntField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetIntField/) &ndash; odczytać wartość `int` wystąpienia pól.

-   [JNIEnv.GetLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetLongField/) &ndash; odczytać wartość `long` wystąpienia pól.

-   [JNIEnv.GetFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFloatField/) &ndash; odczytać wartość `float` wystąpienia pól.

-   [JNIEnv.GetDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetDoubleField/) &ndash; odczytać wartość `double` wystąpienia pól.





### <a name="writing-instance-field-values"></a>Zapisywanie wartości pola wystąpienia

Zbiór metod zapisywania wartości pola wystąpienia jest zgodny ze wzorcem nazewnictwa:

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

gdzie *typu* jest typ pola:

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash; zapisu wartości wszystkie pola, które nie są typu wbudowanej funkcji, takich jak `java.lang.Object` , tablic i typów interfejsów. `IntPtr` Wartość może być JNI lokalnego odwołania, JNI odwołania globalnej, JNI słabe odwołanie globalnych, lub `IntPtr.Zero` (dla `null` ).

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash; zapisu wartości `bool` wystąpienia pól.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash; zapisu wartości `sbyte` wystąpienia pól.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Char)) &ndash; zapisu wartości `char` wystąpienia pól.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash; zapisu wartości `short` wystąpienia pól.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash; zapisu wartości `int` wystąpienia pól.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash; zapisu wartości `long` wystąpienia pól.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Single)) &ndash; zapisu wartości `float` wystąpienia pól.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Double)) &ndash; zapisu wartości `double` wystąpienia pól.


<a name="_Static_Fields" />

## <a name="static-fields"></a>Pola statyczne

Pola statyczne są manipulować za pośrednictwem *pola identyfikatorów*. Identyfikatory pola są otrzymywane za pośrednictwem [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFieldID/), co wymaga klasy zdefiniowanej w nazwę pola, pole i [podpis typu JNI](#JNI_Type_Signatures) pola.

Identyfikatory pola zbędna, nie ma zostać zwolniony i są prawidłowe, tak długo, jak jest ładowany z odpowiadającym typem Java. (Android nie obsługuje obecnie klasy zwalnianie.)

Istnieją dwa zestawy metody pola statyczne: jeden do odczytywania pól wystąpień i jeden dla zapisywania pól wystąpień. Wszystkie zestawy metod wymagają Identyfikatora pola do odczytu lub zapisu wartości pola.


### <a name="reading-static-field-values"></a>Odczytywanie wartości pola statycznego

Zbiór metod do odczytywania wartości pola statycznego jest zgodny ze wzorcem nazewnictwa:

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

gdzie `*` jest typ pola:

-   [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) &ndash; odczytać wartości pola statycznego, która nie jest typu wbudowanej funkcji, takich jak `java.lang.Object` , tablic i typów interfejsów. Wartość zwracana jest JNI lokalnego odwołania.

-   [JNIEnv.GetStaticBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticBooleanField/) &ndash; odczytać wartość `bool` pola statyczne.

-   [JNIEnv.GetStaticByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticByteField/) &ndash; odczytać wartość `sbyte` pola statyczne.

-   [JNIEnv.GetStaticCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticCharField/) &ndash; odczytać wartość `char` pola statyczne.

-   [JNIEnv.GetStaticShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticShortField/) &ndash; odczytać wartość `short` pola statyczne.

-   [JNIEnv.GetStaticLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticLongField/) &ndash; odczytać wartość `long` pola statyczne.

-   [JNIEnv.GetStaticFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFloatField/) &ndash; odczytać wartość `float` pola statyczne.

-   [JNIEnv.GetStaticDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticDoubleField/) &ndash; odczytać wartość `double` pola statyczne.



### <a name="writing-static-field-values"></a>Zapisywanie wartości pola statycznego

Zbiór metod zapisywania wartości pola statycznego jest zgodny ze wzorcem nazewnictwa:

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

gdzie *typu* jest typ pola:

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash; zapisu wartości pola statycznego, która nie jest typu wbudowanej funkcji, takich jak `java.lang.Object` , tablic i typów interfejsów. `IntPtr` Wartość może być JNI lokalnego odwołania, JNI odwołania globalnej, JNI słabe odwołanie globalnych, lub `IntPtr.Zero` (dla `null` ).

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash; zapisu wartości `bool` pola statyczne.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash; zapisu wartości `sbyte` pola statyczne.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Char)) &ndash; zapisu wartości `char` pola statyczne.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash; zapisu wartości `short` pola statyczne.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash; zapisu wartości `int` pola statyczne.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash; zapisu wartości `long` pola statyczne.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Single)) &ndash; zapisu wartości `float` pola statyczne.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Double)) &ndash; zapisu wartości `double` pola statyczne.


<a name="_Instance_Methods" />

## <a name="instance-methods"></a>Wystąpienie metody

Wystąpienie metody są wywoływane za pośrednictwem *metody identyfikatorów*. Identyfikatory metody są otrzymywane za pośrednictwem [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/), który wymaga typu zdefiniowanego w nazwie metody, metody i [podpis typu JNI](#JNI_Type_Signatures) metody.

Metoda identyfikatorów zbędna, nie ma zostać zwolniony i są prawidłowe, tak długo, jak jest ładowany z odpowiadającym typem Java. (Android nie obsługuje obecnie klasy zwalnianie.)

Istnieją dwa zestawy metod do wywoływania metody: jeden dla praktycznie wywoływanie metod i jeden dla-niemal wywoływanie metod. Obie metody wymagają Identyfikatora metodę można wywołać metody i niewirtualną wywołania wymaga również określić implementację klasy, która powinna być wywoływana.

Metody interfejsu można przeszukiwać tylko w obrębie typu deklarującego; Nie można przeszukiwać metody, które pochodzą z interfejsów extended dziedziczone. Zobacz nowsze interfejsy powiązania / section implementacji wywołujący, aby uzyskać więcej informacji.

Każda metoda zadeklarowana w klasie lub dowolnej klasy podstawowej lub zaimplementowanego interfejsu można wyszukiwać.


### <a name="virtual-method-invocation"></a>Wywołanie metody wirtualnej

Zbiór metod wywoływania metod praktycznie zgodny ze wzorcem nazewnictwa:

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

gdzie `*` jest zwracany typ metody.

-   [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/) &ndash; wywołania metody, która zwraca typ innych niż wbudowane, takie jak `java.lang.Object` stałych i interfejsy. Wartość zwracana jest JNI lokalnego odwołania.

-   [JNIEnv.CallBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallBooleanMethod/) &ndash; wywołania metody, która zwraca `bool` wartość.

-   [JNIEnv.CallByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallByteMethod/) &ndash; wywołania metody, która zwraca `sbyte` wartość.

-   [JNIEnv.CallCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallCharMethod/) &ndash; wywołania metody, która zwraca `char` wartość.

-   [JNIEnv.CallShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallShortMethod/) &ndash; wywołania metody, która zwraca `short` wartość.

-   [JNIEnv.CallLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash; wywołania metody, która zwraca `long` wartość.

-   [JNIEnv.CallFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallFloatMethod/) &ndash; wywołania metody, która zwraca `float` wartość.

-   [JNIEnv.CallDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallDoubleMethod/) &ndash; wywołania metody, która zwraca `double` wartość.



### <a name="non-virtual-method-invocation"></a>Wywołanie metody niewirtualnej

Zbiór metod wywoływania metod-niemal zgodny ze wzorcem nazewnictwa:

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

gdzie `*` jest zwracany typ metody. Wywołanie metody niewirtualne jest zwykle używany do wywołania metody podstawowej wirtualnej metody.

-   [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/) &ndash; Non praktycznie wywołania metody, która zwraca typ innych niż wbudowane, takie jak `java.lang.Object` stałych i interfejsy. Wartość zwracana jest JNI lokalnego odwołania.

-   [JNIEnv.CallNonvirtualBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod/) &ndash; Non praktycznie wywołania metody, która zwraca `bool` wartość.

-   [JNIEnv.CallNonvirtualByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualByteMethod/) &ndash; Non praktycznie wywołania metody, która zwraca `sbyte` wartość.

-   [JNIEnv.CallNonvirtualCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualCharMethod/) &ndash; Non praktycznie wywołania metody, która zwraca `char` wartość.

-   [JNIEnv.CallNonvirtualShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualShortMethod/) &ndash; Non praktycznie wywołania metody, która zwraca `short` wartość.

-   [JNIEnv.CallNonvirtualLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualLongMethod/) &ndash; Non praktycznie wywołania metody, która zwraca `long` wartość.

-   [JNIEnv.CallNonvirtualFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualFloatMethod/) &ndash; Non praktycznie wywołania metody, która zwraca `float` wartość.

-   [JNIEnv.CallNonvirtualDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod/) &ndash; Non praktycznie wywołania metody, która zwraca `double` wartość.


<a name="_Static_Methods" />

## <a name="static-methods"></a>Metody statyczne

Metody statyczne są wywoływane za pośrednictwem *metody identyfikatorów*. Identyfikatory metody są otrzymywane za pośrednictwem [JNIEnv.GetStaticMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/), który wymaga typu zdefiniowanego w nazwie metody, metody i [podpis typu JNI](#JNI_Type_Signatures) metody.

Metoda identyfikatorów zbędna, nie ma zostać zwolniony i są prawidłowe, tak długo, jak jest ładowany z odpowiadającym typem Java. (Android nie obsługuje obecnie klasy zwalnianie.)



### <a name="static-method-invocation"></a>Wywołanie metody statycznej

Zbiór metod wywoływania metod praktycznie zgodny ze wzorcem nazewnictwa:

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

gdzie `*` jest zwracany typ metody.

-   [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) &ndash; wywołania metody statycznej, która zwraca typ innych niż wbudowane, takie jak `java.lang.Object` stałych i interfejsy. Wartość zwracana jest JNI lokalnego odwołania.

-   [JNIEnv.CallStaticBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticBooleanMethod/) &ndash; wywołania metody statycznej, która zwraca `bool` wartość.

-   [JNIEnv.CallStaticByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticByteMethod/) &ndash; wywołania metody statycznej, która zwraca `sbyte` wartość.

-   [JNIEnv.CallStaticCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticCharMethod/) &ndash; wywołania metody statycznej, która zwraca `char` wartość.

-   [JNIEnv.CallStaticShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticShortMethod/) &ndash; wywołania metody statycznej, która zwraca `short` wartość.

-   [JNIEnv.CallStaticLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash; wywołania metody statycznej, która zwraca `long` wartość.

-   [JNIEnv.CallStaticFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticFloatMethod/) &ndash; wywołania metody statycznej, która zwraca `float` wartość.

-   [JNIEnv.CallStaticDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticDoubleMethod/) &ndash; wywołania metody statycznej, która zwraca `double` wartość.


<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>Podpisy typu JNI

[Podpisy typu JNI](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) są [odwołania do typu JNI](#_JNI_Type_References) (choć nie uproszczony typu odwołania), z wyjątkiem metod. W przypadku metod podpis typu JNI jest nawias otwierający `'('`, a następnie odwołania do typu dla wszystkich parametrów typów połączonych ze sobą (z Brak oddzielający przecinków lub jakikolwiek inny), a następnie zamykający nawias okrągły `')'`, następuje odwołanie do typu JNI zwracany typ metody.

Na przykład podana metoda Java:

```java
long f(int n, String s, int[] array);
```

Podpis typu JNI mogą być następujące:

```csharp
(ILjava/lang/String;[I)J
```

Ogólnie rzecz biorąc, *silnie* zalecane jest używanie `javap` polecenie w celu ustalenia JNI podpisów. Na przykład JNI typu podpis [java.lang.Thread.State.valueOf(String)](http://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) metoda jest "(Ljava/lang/ciąg;) stan$ Ljava/lang/wątku;", podczas gdy JNI wpisz podpis [ java.lang.Thread.State.values](http://developer.android.com/reference/java/lang/Thread.State.html#values) metoda jest "() [Stan$ Ljava/lang/wątku;". Zwróć uwagę na końcu średnikami; te *są* częścią podpis typu JNI.

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>Odwołania do typu JNI

Odwołania do typu JNI różnią się od odwołania do typu Java. Nie można użyć w pełni kwalifikowane nazwy typów języka Java, takich jak `java.lang.String` z JNI, zamiast niej należy użyć odmiany JNI `"java/lang/String"` lub `"Ljava/lang/String;"`, w zależności od kontekstu; zobacz poniżej, aby uzyskać szczegółowe informacje.
Istnieją cztery typy JNI odwołania do typu:

-  **built-in**
-  **uproszczony**
-  **Typ**
-  **Tablica**


### <a name="built-in-type-references"></a>Odwołania do typu wbudowanego

Typ wbudowany odwołania są pojedynczy znak używany do wartości wbudowane typy referencyjne. Mapowanie wygląda następująco:

-  `"B"` Aby uzyskać `sbyte` .
-  `"S"` Aby uzyskać `short` .
-  `"I"` Aby uzyskać `int` .
-  `"J"` Aby uzyskać `long` .
-  `"F"` Aby uzyskać `float` .
-  `"D"` Aby uzyskać `double` .
-  `"C"` Aby uzyskać `char` .
-  `"Z"` Aby uzyskać `bool` .
-  `"V"` dla `void` metody zwracanych typów.


<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>Uproszczony typu odwołania

Odwołania do typu uproszczony można używać tylko w [JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)).
Istnieją dwa sposoby pochodzi z typu uproszczone odwołanie:

1.  W pełni kwalifikowaną nazwę języka Java, Zastąp co `'.'` w obrębie nazwę pakietu i przed nazwą typu z `'/'` i co `'.'` w nazwę typu z `'$'` .

1.  Dane wyjściowe do odczytu `'unzip -l android.jar | grep JavaName'` .


Jedną z dwóch spowoduje typu Java [java.lang.Thread.State](http://developer.android.com/reference/java/lang/Thread.State.html) mapowane na odwołanie do typu uproszczony `java/lang/Thread$State`.


### <a name="type-references"></a>Typ odwołania

Odwołanie do typu jest odwołanie do typu wbudowanego lub typu uproszczone odwołanie z `'L'` prefiks i `';'` sufiks. Dla typu Java [java.lang.String](http://developer.android.com/reference/java/lang/String.html), jest odwołanie do typu uproszczony `"java/lang/String"`, gdy znajduje się odwołanie do typu `"Ljava/lang/String;"`.

Odwołania do typu są używane z odwołania do typu tablicy i podpisy JNI.

Dodatkowy sposób uzyskać odwołania typu jest odczytując dane wyjściowe `'javap -s -classpath android.jar fully.qualified.Java.Name'`.
W zależności od typu których to dotyczy, możesz użyć deklaracji konstruktora lub metody zwracany typ można określić nazwy JNI. Na przykład:

```shell
$ javap -classpath android.jar -s java.lang.Thread.State
Compiled from "Thread.java"
```

```java
public final class java.lang.Thread$State extends java.lang.Enum{
public static final java.lang.Thread$State NEW;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State RUNNABLE;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State BLOCKED;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TIMED_WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TERMINATED;
  Signature: Ljava/lang/Thread$State;
public static java.lang.Thread$State[] values();
  Signature: ()[Ljava/lang/Thread$State;
public static java.lang.Thread$State valueOf(java.lang.String);
  Signature: (Ljava/lang/String;)Ljava/lang/Thread$State;
static {};
  Signature: ()V
}
```

`Thread.State` jest typem wyliczenia Java, więc używamy podpis `valueOf` metodę, aby określić, że odwołanie do typu jest stan$ Ljava/lang/wątku;.



### <a name="array-type-references"></a>Odwołania do typu tablicy

Odwołania do typu tablicy są `'['` prefiksem odwołania do typu JNI.
Nie można użyć odwołania do typu uproszczony, określając tablic.

Na przykład `int[]` jest `"[I"`, `int[][]` jest `"[[I"`, i `java.lang.Object[]` jest `"[Ljava/lang/Object;"`.



## <a name="java-generics-and-type-erasure"></a>Typy ogólne Java i usuwania typów

*Większość* czasu, przez JNI, ogólne Java *nie istnieją*.
Istnieją pewne "zagnieceń", ale te zagnieceń znajdują się w sposób interakcji Java z ogólnymi nie w sposób JNI wyszukuje i wywołuje ogólnych elementów członkowskich.

Gdy interakcji za pośrednictwem JNI jest brak różnicy w typie ogólnym lub elementu członkowskiego i typu nieogólnego lub elementu członkowskiego. Na przykład typ ogólny [java.lang.Class&lt;T&gt; ](http://developer.android.com/reference/java/lang/Class.html) jest również typu ogólnego "nieprzetworzonej" `java.lang.Class`, które mają tego samego odwołania typu uproszczony, `"java/lang/Class"`.


## <a name="java-native-interface-support"></a>Obsługa natywnego interfejsu Java

[Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) jest zarządzany otok dla Jave interfejsu macierzystego (JNI). Funkcje JNI są zadeklarowane w [natywnego specyfikacja interfejsu Java](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html), ale metody została zmieniona na usuwanie jawnych `JNIEnv*` parametru i `IntPtr` jest używany zamiast `jobject`, `jclass`, `jmethodID`itp. Rozważmy na przykład [funkcja JNI NewObject](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517):

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

To jest ujawniona jako [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/) metody:

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

Tłumaczenie między dwoma wywołań jest rozsądnych prosta. W języku C należy:

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

C# równoważne mogą być następujące:

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

Po utworzeniu wystąpienia obiektu języka Java, przechowywać w formacie typu IntPtr, prawdopodobnie należy do Zrób coś z nim. Można użyć metody JNIEnv, takie jak [ <span class="external">JNIEnv.CallVoidMethod()</span> ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallVoidMethod/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/) Aby to zrobić, ale jeśli istnieje już analogiczny otoki C#, a następnie należy utworzyć otoka przez odwołanie JNI. Możesz zrobić to za pomocą [Extensions.JavaCast <t>()</t> ](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/) — metoda rozszerzenia:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

Można również użyć [Java.Lang.Object.GetObject <t>()</t> ](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) metody:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

Ponadto wszystkie funkcje JNI zostały zmodyfikowane przez usunięcie `JNIEnv*` parametru w każdej funkcji JNI.


## <a name="summary"></a>Podsumowanie

Zajmowanie się bezpośrednio JNI jest olbrzymich środowisko, które należy unikać za wszelką cenę. Niestety nie zawsze jest zbędnego; miejmy nadzieję, że ten przewodnik zawiera niektóre pomocy naciśnięcie niezwiązanego przypadków Java z Mono dla systemu Android.


## <a name="related-links"></a>Linki pokrewne

- [SanityTests (przykład)](https://developer.xamarin.com/samples/SanityTests/)
- [Specyfikacja interfejsu macierzystego języka Java](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Funkcje interfejsu macierzystego języka Java](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
