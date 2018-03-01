---
title: RadioButton
ms.topic: article
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: c4f4909813f5c82a49ec51278b3b50cc36a8e17b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="radiobutton"></a>RadioButton

W tej sekcji utworzysz dwa wzajemnie wykluczającymi się przycisków (Włączenie co wyłączenie innych), za pomocą [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) i [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) elementy widget. Po naciśnięciu przycisku radiowego, albo, zostanie wyświetlony komunikat powiadomienia wyskakującego.


Otwórz **Resources/layout/Main.axml** plik i dodać dwie [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s zagnieżdżona w [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) (wewnątrz [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<RadioGroup
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"
  android:orientation="vertical">
  <RadioButton android:id="@+id/radio_red"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Red" />
  <RadioButton android:id="@+id/radio_blue"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Blue" />
</RadioGroup>
```

Ważne jest, że [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s są pogrupowane według [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) element, aby wybrać nie więcej niż jedną naraz. Istotą takiej logiki jest realizowane automatycznie przez Android system. Gdy dla jednego [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) w ramach grupy jest zaznaczona, wszystkie pozostałe są automatycznie wyłączone.

Coś zrobić po każdym [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) jest zaznaczone, należy zapisać program obsługi zdarzeń:

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

Po pierwsze nadawcy, który jest przekazywany w jest rzutowane na element RadioButton.
Następnie [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) komunikat wyświetlany tekst przycisku radiowego wybrane.

Teraz, u dołu [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) metody, Dodaj następujący kod:

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

To przechwytuje poszczególnych [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s z układu i dodaje handlerto nowo utworzone zdarzenie.

Uruchom aplikację.

**Porada:** Jeśli musisz zmienić stan samodzielnie (takie jak czas ładowania zapisanego [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)), użyj [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) metody ustawiającej lub [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/) metody.

*Części tej stronie są zmiany na podstawie pracy utworzone i udostępnione przez Android Otwórz projekt źródłowy i używane zgodnie z warunki opisane w*
[*Creative Commons 2.5 autorstwa licencji* ](http://creativecommons.org/licenses/by/2.5/). 
