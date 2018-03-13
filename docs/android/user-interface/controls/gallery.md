---
title: Galerii
ms.topic: article
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: da6815a073d93379c8564f3ff91023deb20b0d55
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="gallery"></a>Galerii

[`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) jest używany do wyświetlania elementów w poziomie przewijanej listy widżet układ i umieszcza środek widoku bieżącego zaznaczenia.

> [!IMPORTANT]
> Tego elementu widget została uznana za przestarzałą w systemie Android 4.1 (poziom interfejsu API 16). 

W tym samouczku będziesz Galeria fotografii, a następnie Wyświetl komunikat powiadomienia wyskakującego każdym razem, gdy wybrano elementu galerii.

Po `Main.axml` ustawiono układu dla widoku zawartości `Gallery` przechwycony z Układem o [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/).
[ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/) Właściwości jest następnie używana do ustawiania niestandardowych karty ( `ImageAdapter`) jako źródło dla wszystkich elementów, które mają być wyświetlane w dallery. `ImageAdapter` Jest tworzony w następnym kroku.

Coś zrobić po kliknięciu elementu w galerii, subskrybuje anonimowe delegata [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) zdarzeń. Dzięki niemu [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) wyświetlający indeks (liczony od zera) elementu theselected (w przypadku rzeczywistych, pozycja może posłużyć do uzyskania pełnego obrazu o rozmiarze inne zadanie).

Po pierwsze, istnieje kilka zmiennych Członkowskich, w tym tablicę identyfikatorów, które odwołują się obrazy zapisany w katalogu zasobów obiektów drawable (**obiektów drawable/zasoby**).

Następnie jest konstruktor klasy, gdzie [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/) dla `ImageAdapter` wystąpienia jest zdefiniowana i zapisywane w lokalnym pola.
Następnie to implementuje niektórych wymaganych metod odziedziczone [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).
Konstruktor i [ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) właściwości nie wymaga wyjaśnień. Zwykle [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/p/System.Int32/) powinien zwrócić rzeczywistego obiektu w określonej pozycji na karcie, ale w tym przykładzie zostanie zignorowany. Podobnie [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/p/System.Int32/) powinien zwrócić identyfikator wiersza elementu, ale nie jest wymagana w tym miejscu.

Metoda wykonuje pracę do zastosowania do obrazu [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) które zostaną osadzone w [ `Gallery` ](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) w ramach tej metody, element członkowski [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/) jest Pozwala utworzyć nowy [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).
[ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) Jest przygotowana przez zastosowanie obrazu z lokalnym tablicę obiektów drawable zasobów, ustawienie [ `Gallery.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.Gallery+LayoutParams/) wysokość i szerokość obrazu ustawienie Skaluj do rozmiaru [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) wymiarów i ostatecznie ustawienie tła do użycia atrybutu styleable nabyte w konstruktorze.

Zobacz [ `ImageView.ScaleType` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView+ScaleType/) dla innego obrazu opcje skalowania.

## <a name="walkthrough"></a>Wskazówki

Utwórz nowy projekt o nazwie *HelloGallery*.

![Zrzut ekranu przedstawiający nowy projekt dla systemu Android w oknie dialogowym nowego rozwiązania](gallery-images/hellogallery1.png)

Niektóre fotografie chcesz używać, Znajdź lub [pobrać te przykładowe obrazy](http://developer.android.com/shareables/sample_images.zip).
Dodaj pliki do projektu **zasobów/Drawable** katalogu. W **właściwości** okno, ustawić akcji kompilacji dla każdej z nich **AndroidResource**.

Otwórz **Resources/Layout/Main.axml** i Wstaw następujący:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

Otwórz `MainActivity.cs` i Wstaw następujący kod do [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metody:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    Gallery gallery = (Gallery) FindViewById<Gallery>(Resource.Id.gallery);

    gallery.Adapter = new ImageAdapter (this);

    gallery.ItemClick += delegate (object sender, Android.Widget.AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

Utwórz nową klasę o nazwie `ImageAdapter` tego podklasy [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
          context = c;
    }

    public override int Count { get { return thumbIds.Length; } }

    public override Java.Lang.Object GetItem (int position)
    {
          return null;
    }

    public override long GetItemId (int position)
    {
          return 0;
    }

    // create a new ImageView for each item referenced by the Adapter
    public override View GetView (int position, View convertView, ViewGroup parent)
    {
          ImageView i = new ImageView (context);

          i.SetImageResource (thumbIds[position]);
          i.LayoutParameters = new Gallery.LayoutParams (150, 100);
          i.SetScaleType (ImageView.ScaleType.FitXy);

          return i;
    }

    // references to our images
    int[] thumbIds = {
            Resource.Drawable.sample_1,
            Resource.Drawable.sample_2,
            Resource.Drawable.sample_3,
            Resource.Drawable.sample_4,
            Resource.Drawable.sample_5,
            Resource.Drawable.sample_6,
            Resource.Drawable.sample_7
     };
}

```

Uruchom aplikację. Powinien on wyglądać na poniższym zrzucie ekranu:

![Zrzut ekranu HelloGallery wyświetlanie obrazów próbki](gallery-images/hellogallery3.png)



## <a name="references"></a>Odwołania

-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
-   [`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/)
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)

*Części tej stronie są zmiany na podstawie pracy utworzone i udostępnione przez Android Otwórz projekt źródłowy i używane zgodnie z warunki opisane w*
[*Creative Commons 2.5 autorstwa licencji* ](http://creativecommons.org/licenses/by/2.5/).


