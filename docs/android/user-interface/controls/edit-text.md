---
title: Edytowanie tekstu
description: Jak za pomocą widżetu typu części EditText akceptują dane wejściowe użytkownika.
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/09/2018
ms.openlocfilehash: bb2cb13472e7e17eb1b0438ed67033f2b04defe2
ms.sourcegitcommit: b6f3e55d4f3dcdc505abc8dc9241cff0bb5bd154
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780662"
---
# <a name="edit-text"></a>Edytowanie tekstu

W tej sekcji użyjesz [typu części EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) widżet, aby utworzyć pole tekstowe dla danych wejściowych użytkownika. Po tekst został wprowadzony w pole, **Enter** klucz będzie wyświetlany tekst w komunikacie powiadomień wyskakujących.

Otwórz **Resources/layout/activity_main.axml** i Dodaj [typu części EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) elementu zawierającego układu. Poniższy przykład **activity_main.axml** ma `EditText` który dodano do `LinearLayout`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <EditText
        android:id="@+id/edittext"
        android:layout_width="match_parent"
        android:imeOptions="actionGo"
        android:inputType="text"
        android:layout_height="wrap_content" />
</LinearLayout>
```

W tym przykładzie kodu `EditText` atrybut `android:imeOptions` ustawiono `actionGo`. To ustawienie zmienia domyślny [gotowe](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_DONE) akcji [Przejdź](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_GO) akcji tak, aby naciskając **Enter** klucza wyzwalaczy `KeyPress` obsługi danych wejściowych.
(Zazwyczaj `actionGo` jest używany, aby **Enter** klucza powoduje otwarcie obiekt docelowy adresu URL, który został wpisany w.)

Aby obsługiwać wprowadzanie tekstu użytkownika, Dodaj następujący kod na końcu [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/) method in Class metoda **MainActivity.cs**:

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
    e.Handled = false;
    if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) 
    {
        Toast.MakeText(this, edittext.Text, ToastLength.Short).Show();
        e.Handled = true;
    }
};
```

Ponadto, Dodaj następujący kod `using` instrukcji na górze **MainActivity.cs** Jeśli nie jest jeszcze obecna:

```csharp
using Android.Views;
```

Ten przykład kodu zwiększa [typu części EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) elementu z układu i dodaje [KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) program obsługi, który definiuje akcję, która ma zostać wykonane po naciśnięciu klawisza, gdy widget jest ustawiony fokus. W tym przypadku metoda jest określona do nasłuchiwania pod kątem **Enter** klucza (w przypadku dotknięcie), a następnie wyskakujące [wyskakujące](https://developer.xamarin.com/api/type/Android.Widget.Toast/) wiadomości z tekstem, który został wprowadzony. Należy pamiętać, że [Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) właściwość powinna zawsze być `true` Jeśli zdarzenie zostało obsłużone. Jest to konieczne uniemożliwić Propagacja zdarzenia w górę (co spowoduje powrót karetki, w polu tekstowym).

Uruchom aplikację, a następnie wprowadź jakiś tekst w polu tekstowym. Po naciśnięciu klawisza **Enter** klucza wyskakujące zostanie wyświetlony pokazany po prawej stronie:

[![Przykłady wprowadzania tekstu do typu części EditText](edit-text-images/edit-text-sml.png)](edit-text-images/edit-text.png#lightbox)

*Części tej strony są modyfikacje oparte na Praca utworzona i* [ *współużytkowane przez Android Open Source Project* ](http://code.google.com/policies.html) *i wykorzystywane zgodnie z postanowieniami* [ *Licencji uznania autorstwa creative Commons 2.5* ](http://creativecommons.org/licenses/by/2.5/) *. Ten samouczek opiera się na* [ *samouczek dla systemu Android formularza rzeczy* ](http://developer.android.com/resources/tutorials/views/hello-formstuff.html) *.*


## <a name="related-links"></a>Linki pokrewne

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
