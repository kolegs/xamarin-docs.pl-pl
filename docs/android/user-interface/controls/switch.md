---
title: Przełącznik
description: Sposób użycia elementu widget przełącznika w aplikacji platformy Xamarin.Android
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/29/2018
ms.openlocfilehash: e3bcce48a675a9ba3d1d41f93babc7fcb26448c8
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403279"
---
# <a name="switch"></a>Przełącznik

`Switch` Widżet (pokazana poniżej) umożliwia użytkownikowi przełączanie między dwoma stanami, jak na przykład lub WYŁĄCZONY. `Switch` Wartość domyślna to OFF. Poniżej przedstawiono widżetu w jego ON i OFF stany:

[![Zrzuty ekranu przedstawiające elementu widget przełącznika i Włącz stanów](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>Tworzenie przełącznika

Do utworzenia przełącznika, po prostu zadeklarować `Switch` elementu w pliku XML w następujący sposób:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

Tworzy przełącznik podstawowe, jak pokazano poniżej:

[![Zrzut ekranu przedstawiający aplikacja demonstracyjna, wyświetlanie przełącznik w stanie OFF](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>Zmiana wartości domyślnych

Tekstu wyświetlanego przez kontrolkę dla ON i OFF stanów i wartości domyślne są konfigurowane. Na przykład aby przełącznika, domyślnie włączone i odczytać NO/YES zamiast OFF/ON, możemy ustawić `checked`, `textOn`, i `textOff` atrybutów w następujący kod XML.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>Zapewnianie tytuł

`Switch` Widżet obsługuje również, w tym etykietę tekstową, ustawiając `text` atrybutu w następujący sposób:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Ten kod znaczników generuje Poniższy zrzut ekranu w czasie wykonywania:

[![Zrzut ekranu przedstawiający aplikacja demonstracyjna, przy czym tekst poziomo widżet przełącznika](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

Gdy `Switch`firmy zmiany wartości zgłasza `CheckedChange` zdarzeń.
Na przykład w poniższym kodzie możemy przechwytywania tego zdarzenia i przedstawić `Toast` na podstawie elementu widget komunikatem `isChecked` wartość `Switch`, która jest przekazywana do programu obsługi zdarzeń w ramach `CompoundButton.CheckedChangeEventArg` argumentu.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>Linki pokrewne

- [SwitchDemo (przykład)](https://developer.xamarin.com/samples/monodroid/SwitchDemo/)
- [Karta Układ samouczek](~/android/user-interface/layouts/tab-layout/index.md)
