---
title: Pisanie reakcji aplikacji
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: b8c113b67b3fbfa57ca86c72e11ddeb0e4e1a9ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763504"
---
# <a name="writing-responsive-applications"></a>Pisanie reakcji aplikacji

Jeden z kluczy utrzymywanie reakcji graficznego interfejsu użytkownika jest długotrwałych zadań wątku w tle, nie zablokowane graficznego interfejsu użytkownika. Załóżmy, że chcemy do obliczania wartości do wyświetlenia dla użytkownika, ale ten 5 sekund w celu obliczenia przyjmuje wartość:

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        SlowMethod ();
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

To będzie działać, ale aplikacja "zawiesza się" 5 sekund podczas obliczania wartości. W tym czasie aplikacja nie będzie odpowiadać interakcji z użytkownikiem. Aby uzyskać obejścia tego problemu, chcemy czy naszych obliczeń wątku w tle:

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

Teraz możemy obliczenia wartości wątku w tle, więc naszych GUI pozostaje odpowiadać podczas obliczania. Jednak po zakończeniu obliczania naszych awarii aplikacji, pozostawiając to w dzienniku:

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

Jest to spowodowane należy zaktualizować graficznego interfejsu użytkownika z wątku graficznego interfejsu użytkownika. Naszego kodu aktualizuje graficznego interfejsu użytkownika z puli wątków, co powoduje awarię aplikacji. Musimy obliczenia naszych wartości na wątku w tle, ale następnie wykonaj naszej aktualizacji w wątku graficznego interfejsu użytkownika, który jest obsługiwany z [Activity.RunOnUIThread](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)):

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        RunOnUiThread (() => textview.Text = "Method Complete");
    }
}
```

Ten kod działa zgodnie z oczekiwaniami. Tym graficznym Interfejsie pozostaje reakcji i poprawnie aktualizowany po złożonych obliczeń.

Należy zauważyć, że ta metoda nie jest używany po prostu do obliczenia wartości kosztowne. Może służyć do długotrwałych zadań, które może odbywać się w tle, takie jak wywołania usługi sieci web lub pobieranie danych z Internetu.
