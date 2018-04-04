---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8323c456a97a033e19374a4bd3ea7468ecafa608
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="togglebutton"></a>ToggleButton

W tej sekcji utworzysz przycisk używany specjalnie z myślą o przełączania się między dwoma stanami przy użyciu [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) elementu widget. Tego elementu widget jest doskonałym sposobem przycisków radiowych, jeśli masz dwie proste stanów, które wzajemnie się wykluczają ("on" i "off", na przykład). Android 4.0 (poziom interfejsu API 14) wprowadzono zamiast przycisku przełącznika, znany jako [ `Switch` ](https://developer.xamarin.com/api/type/Android.Widget.Switch/).

Przykład **ToggleButton** można wyświetlić w pary po lewej stronie obrazów, gdy pary prawej obrazów stanowi przykład **przełącznika**:

![Przykłady przełączników i ToggleButtons zarówno włączać i wyłączać stanów](toggle-button-images/togglebutton-switch.png)  

Aplikacja używa kontroli polega na styl. Oba elementy widget działają tak samo.

Otwórz **Resources/layout/Main.axml** plik i dodać [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) elementu (wewnątrz [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

Coś zrobić po zmianie stanu, Dodaj następujący kod na końcu [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) metody:

```csharp
ToggleButton togglebutton = FindViewById<ToggleButton>(Resource.Id.togglebutton);

togglebutton.Click += (o, e) => {
    // Perform action on clicks
    if (togglebutton.Checked)
        Toast.MakeText(this, "Checked", ToastLength.Short).Show ();
    else
        Toast.MakeText(this, "Not checked", ToastLength.Short).Show ();
};
```

To przechwytuje [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) element z układu i obsługuje zdarzenie kliknięcia, który definiuje akcję do wykonania po kliknięciu przycisku. W tym przykładzie metoda sprawdza stan przycisku nowe, a następnie wyświetla [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) komunikat, który wskazuje bieżący stan.

Zwróć uwagę, że [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) dojść własny stan przełączać się między zaznaczony i niezaznaczony, więc jest po prostu poproś.

Uruchom aplikację.


**Porada:** Jeśli musisz zmienić stan samodzielnie (takie jak czas ładowania zapisanego [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)), użyj [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) metody ustawiającej lub [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/) metody.


## <a name="related-links"></a>Linki pokrewne

- [ToggleButton](http://developer.android.com/reference/android/widget/ToggleButton.html)
- [Przełącznik](http://developer.android.com/reference/android/widget/Switch.html)
