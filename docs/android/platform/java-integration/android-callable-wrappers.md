---
title: Wywoływane otoki dla systemu android
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: bb15c7f3a36cc7f1ed235d92e343bbae67a718b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="android-callable-wrappers"></a>Wywoływane otoki dla systemu android

Android wywoływane otoki (ACWs) są wymagane, gdy Android środowiska uruchomieniowego wywołuje kodu zarządzanego. Te otoki są wymagane, ponieważ nie można zarejestrować klasy ozdobione (Android środowiska wykonawczego) w czasie wykonywania. (W szczególności [JNI DefineClass() funkcja](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) nie jest obsługiwana przez środowisko wykonawcze systemu Android.} Wywoływane otoki dla systemu android w związku z tym składają się na brak obsługi rejestracji typu środowiska uruchomieniowego. 

*Za każdym razem, gdy* kodu dla systemu Android musi wykonać `virtual` lub metody interfejsu `overridden` lub zaimplementowano w kodzie zarządzanym, Xamarin.Android należy podać serwer proxy Java, aby ta metoda jest wysyłane do odpowiedniego typu zarządzanego. Te typy proxy Java są kodu języka Java, który ma "w tej samej" klasy podstawowej i listy interfejsu Java jako typ zarządzany, wdrażanie tego samego konstruktorów i typ deklarujący wszelkich przesłoniętych klasy podstawowej i metod interfejsu. 

Wywoływane otoki dla systemu android są generowane przez **monodroid.exe** programu podczas [proces kompilacji](~/android/deploy-test/building-apps/build-process.md): są one generowane dla wszystkich typów dziedziczących (bezpośrednio lub pośrednio) [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/). 



## <a name="android-callable-wrapper-naming"></a>Wywoływana otoka android nazewnictwa

Nazwy pakietu dla systemu Android wywoływane otoki są oparte na MD5SUM nazwa kwalifikowana zestawu typu eksportowane. Ta technika nazewnictwa umożliwia tego samego Pełna nazwa typu udostępniane przez różnych zestawów bez wprowadzania błąd tworzenia pakietów. 

Ze względu na to schemat nazewnictwa MD5SUM nie może bezpośrednio uzyskać dostęp z typów według nazwy. Na przykład następująca `adb` polecenia nie będą działać, ponieważ nazwa typu `my.ActivityType` nie jest generowany domyślnie: 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

Ponadto mogą zostać wyświetlone błędy podobnie do następującej przy próbie odwoływać się do typu o nazwie:

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

Jeśli użytkownik *czy* wymagają dostępu do typów według nazwy, mogą zadeklarować nazwę dla tego typu w deklaracji atrybutu. Na przykład, w tym miejscu jest kod, który deklaruje działania z w pełni kwalifikowaną nazwę `My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

`ActivityAttribute.Name` Właściwości można ustawić jawnie deklarować nazwy tego działania: 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Po dodaniu ustawienie właściwości `my.ActivityType` można uzyskać, sprawdzając nazwę z kodu zewnętrznego i `adb` skryptów. `Name` Atrybut można określać dla wielu różnych typów, w tym `Activity`, `Application`, `Service`, `BroadcastReceiver`, i `ContentProvider`: 

-   [ActivityAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/)
-   [ApplicationAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Name/)
-   [ServiceAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ServiceAttribute.Name/)
-   [BroadcastReceiverAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.BroadcastReceiverAttribute.Name/)
-   [ContentProviderAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.ContentProviderAttribute.Name/)

Na podstawie MD5SUM nazewnictwa Aktywnej wprowadzono 5.0 platformy Xamarin.Android. Aby uzyskać więcej informacji o nazwach atrybutów, zobacz [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/). 



## <a name="implementing-interfaces"></a>Implementowanie interfejsów

Istnieją momenty, gdy konieczne może być implementuje interfejs dla systemu Android, takie jak [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/). Ponieważ wszystkie klasy systemu Android i interfejs rozszerzają [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) interfejsu, powstaje pytanie: jak możemy wdrożyć `IJavaObject`? 

Pytanie zostało odebrane powyżej: Przyczyna wszystkie typy Android muszą implementować `IJavaObject` jest tak, aby Xamarin.Android Android wywoływana otoka zapewnienie systemu Android, np. proxy Java danego typu. Ponieważ **monodroid.exe** wyszukuje tylko `Java.Lang.Object` podklasy, i `Java.Lang.Object` implementuje `IJavaObject,` odpowiedzi są oczywiste: podklasy `Java.Lang.Object`: 

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {

    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
    {
        // implementation goes here...
    } 

    public void OnLowMemory ()
    {
        // implementation goes here...
    }
}
```


## <a name="implementation-details"></a>Szczegóły implementacji

*Pozostała część ta strona zawiera szczegóły implementacji mogą ulec zmianie bez uprzedzenia* (i przedstawionej tutaj tylko ponieważ deweloperzy będą zastanawiasz się, jak to, co dzieje się). 

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

public class HelloAndroid
    extends android.app.Activity
{
    static final String __md_methods;
    static {
        __md_methods = "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" + "";
        mono.android.Runtime.register (
            "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
            Culture=neutral, PublicKeyToken=null", HelloAndroid.class, __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
                Culture=neutral, PublicKeyToken=null", "", this, new java.lang.Object[] {  });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Zwróć uwagę, że klasa podstawowa jest zachowywana, a `native` deklaracje metody są dostępne dla każdej metody, która zostanie zastąpiona w kodzie zarządzanym. 
