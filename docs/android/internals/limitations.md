---
title: Vs platformy Xamarin.Android. Desktop — różnice w środowisku wykonawczym Mono
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: b1302bcf8d6835cac356d96b538d134891648420
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436768"
---
# <a name="limitations"></a>Ograniczenia

Ponieważ aplikacje w systemie Android wymaga generowania Java typy serwera proxy podczas procesu kompilacji, nie jest możliwe generowanie całego kodu w czasie wykonywania.

Ograniczenia platformy Xamarin.Android w porównaniu do pulpitu Mono są to:


## <a name="limited-dynamic-language-support"></a>Obsługa ograniczona języka dynamicznego

 [Wywoływane otoki android](~/android/platform/java-integration/android-callable-wrappers.md) są potrzebne w dowolnym momencie Android środowiska uruchomieniowego trzeba wywołanie kodu zarządzanego. Wywoływane otoki dla systemu android są generowane w czasie kompilacji, na podstawie statycznej analizy kodu IL. Wynikiem tego: możesz *nie* Użyj dynamicznych języków (IronPython, IronRuby itp.) w żadnym scenariuszu gdzie podklasy typów Java jest wymagana (w tym podklasy pośredniego), jako nie istnieje sposób wyodrębniania tych typów dynamicznych w czasie kompilacji do generowania niezbędne wywoływane otoki dla systemu Android.


## <a name="limited-java-generation-support"></a>Obsługa generowania ograniczone Java

[Wywoływane otoki android](~/android/platform/java-integration/android-callable-wrappers.md) muszą być generowane w kolejności dla kodu języka Java do wywoływania z kodu zarządzanego. *Domyślnie*, Android wywoływane otoki będzie zawierać tylko (niektórych) zadeklarowane konstruktory i metody, które zastąpią metodą wirtualną Java (tzn. ma [ `RegisterAttribute` ](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)) lub implementować (metoda interfejsu Java Interfejs zawiera `Attribute`).
  
Przed wydaniem 4.1 żadnych dodatkowych metod może być zadeklarowany. W wersji 4.1 [ `Export` i `ExportField` atrybutów niestandardowych, które można zadeklarować metody Java i pól w Android wywoływana otoka](~/android/platform/java-integration/working-with-jni.md).

### <a name="missing-constructors"></a>Brak konstruktorów

Konstruktory pozostają trudne, chyba że [ `ExportAttribute` ](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute) jest używany. Algorytm generowania konstruktorów Android wywoływana otoka jest, czy będzie obliczanie konstruktora Java, jeśli:

1. Brak mapowania Java dla wszystkich typów parametru
2. Klasa podstawowa deklaruje konstruktora tego samego &ndash; jest to wymagane, ponieważ Android wywoływana otoka *musi* wywołać odpowiedniego konstruktora klasy podstawowej; można nie argumentów domyślnych (ponieważ nie ma łatwego sposobu na określenia, jakie wartości powinny być używane w języku Java).

Rozważmy na przykład następującej klasy:

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

Podczas tego doskonale logicznej wygląda wynikowy Android wywoływana otoka *w kompilacjach wydania* nie będą zawierać konstruktora domyślnego. W związku z tym jeśli próba uruchomienia tej usługi (np. [ `Context.StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)), zakończy się niepowodzeniem:

```shell
E/AndroidRuntime(31766): FATAL EXCEPTION: main
E/AndroidRuntime(31766): java.lang.RuntimeException: Unable to instantiate service example.MyIntentService: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2347)
E/AndroidRuntime(31766):        at android.app.ActivityThread.access$1600(ActivityThread.java:130)
E/AndroidRuntime(31766):        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1277)
E/AndroidRuntime(31766):        at android.os.Handler.dispatchMessage(Handler.java:99)
E/AndroidRuntime(31766):        at android.os.Looper.loop(Looper.java:137)
E/AndroidRuntime(31766):        at android.app.ActivityThread.main(ActivityThread.java:4745)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invokeNative(Native Method)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invoke(Method.java:511)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:786)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:553)
E/AndroidRuntime(31766):        at dalvik.system.NativeStart.main(Native Method)
E/AndroidRuntime(31766): Caused by: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at java.lang.Class.newInstanceImpl(Native Method)
E/AndroidRuntime(31766):        at java.lang.Class.newInstance(Class.java:1319)
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2344)
E/AndroidRuntime(31766):        ... 10 more
```

Należy zadeklarować konstruktora domyślnego, za pomocą adorn `ExportAttribute`i ustaw [ `ExportAttribute.SuperStringArgument` ](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/): 

```csharp
[Service]
class MyIntentService : IntentService {
    [Export (SuperArgumentsString = "\"value\"")]
    public MyIntentService (): base("value")
    {
    }

    // ...
}
```


### <a name="generic-c-classes"></a>Klasy ogólne C#

Klasy ogólne C# są obsługiwane tylko częściowo. Istnieją następujące ograniczenia:


-   Typy ogólne nie mogą używać `[Export]` lub `[ExportField`]. Takie próby wygeneruje `XA4207` błędu.

    ```csharp
    public abstract class Parcelable<T> : Java.Lang.Object, IParcelable
    {
        // Invalid; generates XA4207
        [ExportField ("CREATOR")]
        public static IParcelableCreator CreateCreator ()
        {
            ...
    }
    ```

-   Metody ogólne nie mogą używać `[Export]` lub `[ExportField]`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        
        // Invalid; generates XA4207
        [Export]
        public static void Method<T>(T value)
        {
            ...
        }
    }
    ```

-   `[ExportField]` Nie można używać metod, które zwraca `void`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        // Invalid; generates XA4208
        [ExportField ("CREATOR")]
        public static void CreateSomething ()
        {
        }
    }
    ```

-   Wystąpień typów ogólnych _nie mogą_ można utworzyć na podstawie kodu języka Java.
    Mogą one tylko bezpiecznie utworzone z kodu zarządzanego:

    ```csharp
    [Activity (Label="Die!", MainLauncher=true)]
    public class BadGenericActivity<T> : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);
        }
    }
    ```


## <a name="partial-java-generics-support"></a>Częściowe pytania ogólne

Java, obsługiwanych typów ogólnych powiązanie jest ograniczona. Szczególnie, Pozostało elementów członkowskich w klasie ogólnego wystąpienia, która pochodzi z innej klasy ogólnej (z systemem innym niż wystąpienia) udostępniony jako Java.Lang.Object. Na przykład [Android.Content.Intent.GetParcelableExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.GetParcelableExtra/p/System.String/) metoda zwraca Java.Lang.Object. Jest to spowodowane wymazanej ogólne Java.
Mamy kilka klas, które nie mają zastosowania tego ograniczenia, ale są skorygowane ręcznie.


## <a name="related-links"></a>Linki pokrewne

- [Wywoływane otoki systemu Android](~/android/platform/java-integration/android-callable-wrappers.md)
- [Praca z JNI](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)
- [SuperString](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/)
- [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)
