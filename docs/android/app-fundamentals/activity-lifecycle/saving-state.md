---
title: Wskazówki — Zapisywanie stanu działania
description: Firma Microsoft ma objętych teorii za Zapisywanie stanu w przewodniku cyklu życia działanie; Teraz przejdźmy przykładem.
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: e282eeb8732bd5294da4ec4e3fe337e81107c8f3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---saving-the-activity-state"></a>Wskazówki — Zapisywanie stanu działania

_Firma Microsoft ma objętych teorii za Zapisywanie stanu w przewodniku cyklu życia działanie; Teraz przejdźmy przykładem._

## <a name="activity-state-walkthrough"></a>Wskazówki stanu działania

Umożliwia otwarcie **ActivityLifecycle_Start** projektu (w [ActivityLifecycle](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle) przykładowe), skompiluj go i uruchom go. Jest to bardzo proste projekt, który ma dwa działania, aby zademonstrować cyklu życia działania i jak są nazywane różnych metod cyklu życia. Po uruchomieniu aplikacji na ekranie `MainActivity` wyświetleniem: 

[![Działania A ekranu](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>Wyświetlanie stanu przejścia

Każda metoda w tym przykładzie zapisuje w oknie danych wyjściowych aplikacji IDE do wskazywania stanu działania. (Aby otworzyć okno danych wyjściowych w programie Visual Studio, wpisz **CTRL-ALT-O**; aby otworzyć okno danych wyjściowych w programie Visual Studio for Mac, kliknij przycisk **Widok > konsole > danych wyjściowych aplikacji**.)

Po pierwszym uruchomieniu aplikacji, w oknie danych wyjściowych wyświetlane zmiany stanu z *działania A*: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Po kliknięciu pozycji **Start B działania** przycisku, widzimy *działania A* Wstrzymaj i Zatrzymaj podczas *B działania* przechodzi przez jego zmiany stanu: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

W związku z tym *działania B* pracy i wyświetlić zamiast *działania A*: 

[![Ekran działania B](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

Po kliknięciu pozycji **ponownie** przycisku *działania B* zostanie zniszczony i *działania A* wznawiania: 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>Dodawanie liczników kliknij

Następnie wdrożymy Zmień aplikacji, tak aby mamy zlicza i wyświetla liczbę razy kliknięciu przycisku. Po pierwsze możemy dodać `_counter` zmienna wystąpienia na `MainActivity`:

```csharp
int _counter = 0;
```

Następnie umożliwia edytowanie **Resource/layout/Main.axml** układ pliku i Dodaj nową `clickButton` wyświetlający, ile razy użytkownik kliknął przycisk. Powstałe w ten sposób **Main.axml** powinien przypominać następujący: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/myButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/mybutton_text" />
    <Button
        android:id="@+id/clickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/counterbutton_text" />
</LinearLayout>
```

Teraz Dodaj następujący kod na końcu [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metody w `MainActivity` &ndash; ten kod uchwytów kliknij zdarzenia z `clickButton`:

```csharp
var clickbutton = FindViewById<Button> (Resource.Id.clickButton);
clickbutton.Text = Resources.GetString (
    Resource.String.counterbutton_text, _counter);
clickbutton.Click += (object sender, System.EventArgs e) =>
{
    _counter++;
    clickbutton.Text = Resources.GetString (
        Resource.String.counterbutton_text, _counter);
} ;
```

Gdy budujemy i ponownie uruchom aplikację, nowe pojawia się przycisk zwiększa i wyświetla wartość `_counter` przy każdym kliknięciu:

[![Dodaj liczba touch](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

Jednak gdy firma Microsoft obracania urządzenia do tryb poziomo, ta liczba jest utracone:

[![Obracanie na poziomą Ustawia liczbę wstecz od zera.](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

Badanie danych wyjściowych aplikacji, widzimy, który *działania A* została wstrzymana, zatrzymana, zniszczona, odtworzone, ponowne uruchomienie, a następnie wznowiona podczas obrotu pionowy tryb poziomo: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Ponieważ *działania A* jest zniszczony i utworzony ponownie ponownie, gdy urządzenie jest obracany, jego stan wystąpienia zostaną utracone. Następnie dodamy kod, aby zapisać i przywrócenia stanu wystąpienia.

### <a name="adding-code-to-preserve-instance-state"></a>Dodawanie kodu do Zachowaj stan wystąpienia

Umożliwia dodanie metody `MainActivity` można zapisać stan wystąpienia. Przed *działania A* jest niszczone, Android automatycznie wywołuje [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) i przekazuje w [pakietu](https://developer.xamarin.com/api/type/Android.OS.Bundle/) możemy służy do przechowywania naszych stan wystąpienia. Teraz użyj go, aby zapisać naszych liczba kliknij jako wartość całkowitą:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

Gdy *działania A* jest ponowne utworzenie i wznowione, Android przekazuje to `Bundle` do naszych `OnCreate` — metoda. Teraz Dodaj kod, aby `OnCreate` do przywrócenia `_counter` wartość z zakresu od przekazany do `Bundle`. Dodaj następujący kod bezpośrednio przed wierszem gdzie `clickbutton` jest zdefiniowana: 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

Tworzenie i ponownie uruchom aplikację, a następnie kliknij przycisk drugi kilka razy. Firma Microsoft obracania urządzenia do tryb poziomo, zostaną zachowane licznik!

[![Obracanie ekranu pokazuje liczbę cztery zachowane](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)


Spójrzmy w oknie danych wyjściowych, aby zobaczyć, co się stało:
    
```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - Saving instance state
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - Recovered instance state
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
``` 

Przed [OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) wywołano metodę nowe `OnSaveInstanceState` wywołano metodę, aby zapisać `_counter` wartość w `Bundle`. Android przekazany to `Bundle` do nas po jej wywołaniu naszych `OnCreate` — metoda i my udało się użyć, aby przywrócić `_counter` wartość na miejscu.


## <a name="summary"></a>Podsumowanie

W tym walkthough użyliśmy naszych wiedzy na temat cyklu życia działania w celu zachowania danych o stanie. 



## <a name="related-links"></a>Linki pokrewne

- [ActivityLifecycle (przykład)](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)
- [Cykl życia aktywności](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Działanie systemu android](https://developer.xamarin.com/api/type/Android.App.Activity/)
