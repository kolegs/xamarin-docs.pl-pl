---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 85c505d03e7a763b24fb176b6a94c0fe43009b79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="checkbox"></a>CheckBox

W tej sekcji utworzysz wyboru dla elementów, wybierając przy użyciu [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox) elementu widget. Po naciśnięciu pole wyboru wiadomość wyskakujące będzie wskazują bieżący stan elementu checkbox.

Otwórz **Resources/layout/Main.axml** plik i dodać [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) elementu (wewnątrz [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout)):

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

Coś zrobić po zmianie stanu, Dodaj następujący kod na końcu [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) metody:

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

To przechwytuje [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) element z układu, obsługuje zdarzenie Click, który definiuje akcji, które ma zostać wykonane po kliknięciu pola wyboru. Po kliknięciu [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) właściwości jest wywoływana w celu sprawdzenia stanu nowego pola wyboru. Jeśli zostało zaznaczone, a następnie [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) wyświetlany jest komunikat "Wybrane", w przeciwnym razie wyświetla "Nie jest zaznaczone". [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) Obsługuje własnych zmian stanu, więc należy zbadać bieżącego stanu.

Uruchom go.

**Porada:** Jeśli musisz zmienić stan samodzielnie (takie jak czas ładowania zapisanego [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference), użyj [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked) metody ustawiającej lub [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle) metody.

*Części tej stronie są zmiany na podstawie pracy utworzone i udostępnione przez Android Otwórz projekt źródłowy i używane zgodnie z warunki opisane w*
[*Creative Commons 2.5 autorstwa licencji* ](http://creativecommons.org/licenses/by/2.5/).
