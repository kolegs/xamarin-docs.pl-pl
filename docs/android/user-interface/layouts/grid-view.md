---
title: GridView
ms.topic: article
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: c28296af43f0091443eda0364fc0c28a938a7760
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="gridview"></a>GridView

[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) jest [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) wyświetlającą elementy w dwuwymiarowa, którą można przewijać w siatce. Elementy siatki automatycznie są wstawiane do układu przy użyciu [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/).

W tym samouczku utworzysz siatka obraz miniatury. Gdy element jest wybrany, zostanie wyświetlony komunikat powiadomienia wyskakującego położenie obrazu.

Utwórz nowy projekt o nazwie **HelloGridView**.

Niektóre fotografie chcesz używać, Znajdź lub [pobrać te przykładowe obrazy](http://developer.android.com/shareables/sample_images.zip). Dodaj pliki do projektu **zasobów/Drawable** katalogu. W **właściwości** okno, ustawić akcji kompilacji dla każdej z nich **AndroidResource**.

Otwórz **Resources/Layout/Main.axml** pliku i Wstaw następujący:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gridview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:columnWidth="90dp"
    android:numColumns="auto_fit"
    android:verticalSpacing="10dp"
    android:horizontalSpacing="10dp"
    android:stretchMode="columnWidth"
    android:gravity="center"
/>
```

To [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) spowoduje wypełnienie cały ekran. Atrybuty są raczej self objaśniający. Aby uzyskać więcej informacji o prawidłowych atrybutów, zobacz [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) odwołania.

Otwórz `HelloGridView.cs` i Wstaw następujący kod do [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metody:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    var gridview = FindViewById<GridView> (Resource.Id.gridview);
    gridview.Adapter = new ImageAdapter (this);

    gridview.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

Po **Main.axml** ustawiono układu dla widoku zawartości [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) przechwycony z Układem o [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/). [ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/) Właściwości jest następnie używana do ustawiania niestandardowych karty (`ImageAdapter`) jako źródło dla wszystkich elementów do wyświetlenia w siatce. `ImageAdapter` Jest tworzony w następnym kroku.

Coś zrobić po kliknięciu elementu w siatce, subskrybuje anonimowe delegata [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) zdarzeń.
Dzięki niemu [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) wyświetlający indeks (liczony od zera) wybranego elementu (w przypadku rzeczywistych, pozycja może posłużyć do uzyskania pełnego obrazu o rozmiarze inne zadanie). Należy pamiętać, że Java stylu odbiornika klasy mogą być używane zamiast zdarzenia platformy .NET.

Utwórz nową klasę o nazwie `ImageAdapter` tego podklasy [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
        context = c;
    }

    public override int Count {
        get { return thumbIds.Length; }
    }

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
        ImageView imageView;

        if (convertView == null) {  // if it's not recycled, initialize some attributes
            imageView = new ImageView (context);
            imageView.LayoutParameters = new GridView.LayoutParams (85, 85);
            imageView.SetScaleType (ImageView.ScaleType.CenterCrop);
            imageView.SetPadding (8, 8, 8, 8);
        } else {
            imageView = (ImageView)convertView;
        }

        imageView.SetImageResource (thumbIds[position]);
        return imageView;
    }

    // references to our images
    int[] thumbIds = {
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7
    };
}
```

Po pierwsze, to implementuje niektórych wymaganych metod dziedziczone z [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/). Konstruktor i [ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) właściwości nie wymaga wyjaśnień. Zwykle [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/) powinien zwrócić rzeczywistego obiektu w określonej pozycji na karcie, ale w tym przykładzie zostanie zignorowany. Podobnie [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/) powinien zwrócić identyfikator wiersza elementu, ale nie jest wymagana w tym miejscu.

Pierwsza metoda niezbędne jest [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/).
Ta metoda tworzy nowy [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) dla każdego obrazu dodane do `ImageAdapter`. Gdy jest to, [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) jest przekazywana, która zwykle jest obiektem odtwarzania (co najmniej po tym wywołaniu raz), więc jest to sprawdzenie, czy obiekt nie ma wartości null. Jeśli go *jest* null, [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) jest wystąpienie zostało utworzone i skonfigurowane odpowiednie właściwości w celu przedstawienia obrazu:

- [`LayoutParams`](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) Ustawia wysokość i szerokość dla widoku&mdash;, bez względu na rozmiar obiektów drawable każdego obrazu jest zmiany rozmiaru i przycięte mieści się w tych wymiarów, zależnie od potrzeb.

- [`SetScaleType()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetScaleType/) deklaruje, że obrazy powinien przycięte kierunku Centrum (Jeśli to konieczne).

- [`SetPadding(int, int, int, int)`](https://developer.xamarin.com/api/member/Android.Views.View.SetPadding/) Określa uzupełnienie dla wszystkich stron. (Należy pamiętać, że jeśli obrazów różnych współczynników proporcji, następnie mniej dopełnienie spowoduje, że dla więcej przycinanie obrazu, jeśli nie pasuje do ImageView wymiary).

Jeśli [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) przekazany do [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) jest *nie* wartości null, a następnie lokalnej [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) jest inicjowany z odtworzenie [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) obiektu.

Na koniec [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) metody `position` całkowitą przekazany do metody jest używana do wybierania obrazu z `thumbIds` tablicy, która jest ustawiona jako zasób obrazu [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).

Pozostało jest określenie `thumbIds` tablicę obiektów drawable zasobów.

Uruchom aplikację. Układ siatki powinien wyglądać następująco:

[![Zrzut ekranu widoku GridView wyświetlanie obrazów 15](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png)

Spróbuj eksperymentowanie z zachowania [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) i [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) elementów, zmieniając ich właściwości. Na przykład zamiast [ `LayoutParams` ](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) spróbuj użyć [ `SetAdjustViewBounds()` ](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetAdjustViewBounds/).

<a name="References" />

## <a name="references"></a>Odwołania

-   [`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).

*Części tej stronie są zmiany na podstawie pracy utworzone i udostępnione przez Android Otwórz projekt źródłowy i używane zgodnie z warunki opisane w*
[*Creative Commons 2.5 autorstwa licencji* ](http://creativecommons.org/licenses/by/2.5/).
