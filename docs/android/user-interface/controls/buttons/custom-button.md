---
title: Przycisk niestandardowy
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: e6fc3fe4c3cb89d74188557615f58cc8e34f5991
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="custom-button"></a>Przycisk niestandardowy

W tej sekcji utworzysz przycisk z niestandardowego obrazu zamiast tekstu, za pomocą [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) elementu widget oraz plik XML, który definiuje trzy różne obrazów dla różnych stanów przycisku. Gdy przycisk jest naciśnięty, pojawi się krótką wiadomość.

Kliknij prawym przyciskiem myszy i pobrać trzy obrazy poniżej, a następnie skopiuj je do **obiektów drawable/zasoby** katalogu projektu. Te będą służyć do różnych stanów przycisku.

 [![Zielony Android ikona normalnego stanu](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [ ![Android pomarańczowy ikona stanu ukierunkowanych](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [ ![Android żółta ikona stanu naciśniętego](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

Utwórz nowy plik w **obiektów drawable/zasoby** katalog o nazwie **android_button.xml**. Wstaw następujący kod XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/android_pressed"
          android:state_pressed="true" />
    <item android:drawable="@drawable/android_focused"
          android:state_focused="true" />
    <item android:drawable="@drawable/android_normal" />
</selector>
```

Definiuje pojedynczego zasobu obiektów drawable, co spowoduje zmianę jego obrazu, w oparciu o bieżący stan przycisku. Pierwszy `<item>` definiuje **android_pressed.png** jako obraz po naciśnięciu przycisku (zostanie został aktywowany); druga `<item>` definiuje **android_focused.png** jako obraz po przycisk ma fokus (gdy przycisk zostanie wyróżniona, za pomocą urządzenia wskazującego lub konsoli kierunkowej); a trzeci `<item>` definiuje **android_normal.png** jako obraz normalnym stanie (po naciśnięciu ani fokus). Ten plik XML teraz reprezentuje pojedynczy zasób obiektów drawable i gdy odwołuje się [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) jego tła obrazu wyświetlanego zmieni na podstawie tych trzech stanów.


> [!NOTE]
> Kolejność `<item>` elementów jest ważna. Po odwołaniu tym obiektów drawable `<item>`jest przesunięta w kolejności do określenia, która jest odpowiednia dla bieżącego stanu przycisku.
> Ponieważ ostatni obraz "normal", jest tylko dotyczy sytuacji, gdy warunki `android:state_pressed` i `android:state_focused` zarówno oszacowania wartość false.

Otwórz **Resources/layout/Main.axml** plik i dodać [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) elementu:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

`android:background` Atrybut określa obiektów drawable zasobów do użycia na potrzeby tło przycisku (które, po zapisaniu na **Resources/drawable/android.xml**, jest określany jako `@drawable/android`). Zastępuje obraz tła normalne przycisków w całym systemie. Aby obiektów drawable zmienić jego obrazu na podstawie stanu przycisku należy zastosować obraz do tła.

Aby czymś po naciśnięciu przycisku, Dodaj następujący kod na końcu [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/) metody:

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

To przechwytuje [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) z układu, następnie dodaje [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) komunikat, który będzie wyświetlany po [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) zostanie kliknięty.

Teraz uruchom aplikację.


*Części tej stronie są zmiany na podstawie pracy utworzone i udostępnione przez Android Otwórz projekt źródłowy i używane zgodnie z warunki opisane w*
[*Creative Commons 2.5 autorstwa licencji* ](http://creativecommons.org/licenses/by/2.5/).
