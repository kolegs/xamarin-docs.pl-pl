---
title: Przełącznik
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0f4bfc3646f1ccd956ee8151468b3de20f6e1e2b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="switch"></a>Przełącznik

`Switch` Widget (pokazana poniżej) pozwala użytkownikowi na przełączanie się między dwoma stanami, jak na przykład lub wyłączone. `Switch` Wartość domyślna to OFF. Poniżej przedstawiono widżetu jego ON i OFF stany:

[![Element widget przełącznika i Włącz stany zrzuty ekranu](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>Tworzenie przełącznika

Aby utworzyć przełącznik, po prostu zadeklarować `Switch` elementu w języku XML w następujący sposób:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

Tworzy przełącznik podstawowe, jak pokazano poniżej:

[![Zrzut ekranu przedstawiający aplikacja demonstracyjna, wyświetlanie przełącznika w stanie wyłączone](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>Zmiana wartości domyślne

Zarówno tekstu wyświetlanego przez formant dla ON i OFF Stany i wartość domyślną, mogą być konfigurowane. Na przykład aby przełącznika domyślne on i odczytu nie/tak zamiast OFF/w, możemy ustawić `checked`, `textOn`, i `textOff` atrybutów w następujących XML.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>Udostępnia tytuł

`Switch` Elementu widget obsługuje również tym etykietę tekstową, ustawiając `text` atrybutu w następujący sposób:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Ten kod znaczników tworzy Poniższy zrzut ekranu w czasie wykonywania:

[![Zrzut ekranu przedstawiający aplikacja demonstracyjna, z tekstem w poziomie poprzedzającą widget przełącznika](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

Gdy `Switch`jego zmiany wartości zgłasza `CheckedChange` zdarzeń.
Na przykład w poniższym kodzie możemy przechwytywania tego zdarzenia i stanowi `Toast` na podstawie elementu widget komunikatem `isChecked` wartość `Switch`, która jest przekazywana jako część programu obsługi zdarzeń `CompoundButton.CheckedChangeEventArg` argumentu.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>Linki pokrewne

- [SwitchDemo (przykład)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/SwitchDemo/)
- [Samouczek układu kartę](~/android/user-interface/layouts/tab-layout/index.md)
- [Wprowadzenie Sandwich lodów](http://www.android.com/about/ice-cream-sandwich/)
- [Platformy systemu android 4.0](http://developer.android.com/sdk/android-4.0.html)
