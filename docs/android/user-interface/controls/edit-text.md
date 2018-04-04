---
title: Edytowanie tekstu
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d6be8ae1587742a8c2a37b22b2da3701187dde2a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="edit-text"></a>Edytowanie tekstu

W tej sekcji utworzysz pola tekstowego do danych wprowadzonych przez użytkownika, za pomocą [typu części EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) elementu widget. Tekst wprowadzony w polu klucza "Wprowadź" będzie wyświetlany tekst w komunikacie powiadomienia wyskakującego.

Otwórz <code>Resources\layout\main.xml</code> plik i dodać [typu części EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) elementu (wewnątrz [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<pre><code class=" syntax brush-C#">&lt;EditText
    android:id="@+id/edittext"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"/&gt;</code></pre>
```

Do zrobienia czegoś tekst, który użytkownik wpisuje, Dodaj następujący kod na końcu [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/) metody:

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
 e.Handled = false;
 if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) {
  Toast.MakeText (this, edittext.Text, ToastLength.Short).Show ();
  e.Handled = true;
         }
};
```

To przechwytuje [ `EditText` ](https://developer.xamarin.com/api/type/Android.Widget.EditText/) element na podstawie układu i zestawy [ `KeyPress` ](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) obsługi, który definiuje akcję, która ma zostać wykonane po naciśnięciu klawisza, gdy widżetu ma fokus. W tym przypadku metoda zdefiniowano w celu nasłuchiwania klawisza Enter (po naciśnięciu), a następnie wyskakujące [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) wiadomości z tekstem, który został wprowadzony. [ `Handled` ](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) Właściwość powinna zawsze być `true` Jeśli zdarzenie został obsłużony, dzięki czemu zdarzenia nie bąbelków w górę (co spowoduje powrót karetki w polu tekstowym).

Uruchom aplikację.

*Części tej stronie są zmiany na podstawie pracy utworzone i* [ *współużytkowane przez Android Otwórz projekt źródłowy* ](http://code.google.com/policies.html) *i używać zgodnie z warunki opisane w* [ *Licencji autorstwa creative Commons 2.5* ](http://creativecommons.org/licenses/by/2.5/) *. W tym samouczku jest oparta na* [ *Android rzeczy formularza samouczek* ](http://developer.android.com/resources/tutorials/views/hello-formstuff.html) *.*



## <a name="related-links"></a>Linki pokrewne

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
