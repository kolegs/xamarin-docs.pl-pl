---
title: Xamarin.Android Performance
description: "Istnieje wiele technik zwiększającą wydajność aplikacji skompilowanej za pomocą platformy Xamarin.Android. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywana przez Procesora i ilości pamięci używanej przez aplikację. W tym artykule opisano i omówiono te techniki."
ms.topic: article
ms.prod: xamarin
ms.assetid: dc2e27f2-7f71-4d57-9cf9-165528276613
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3871955f723d1b3aec6245bba0502ca4f955d64c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-performance"></a>Xamarin.Android Performance

_Istnieje wiele technik zwiększającą wydajność aplikacji skompilowanej za pomocą platformy Xamarin.Android. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywana przez Procesora i ilości pamięci używanej przez aplikację. W tym artykule opisano i omówiono te techniki._

## <a name="performance-overview"></a>Wydajności — omówienie

Niską wydajnością przedstawia na wiele sposobów. Go aplikacja prawdopodobnie nie odpowiada, może spowodować wolne przewijanie i może zmniejszyć czas pracy baterii. Jednak optymalizacji wydajności wymaga więcej niż tylko wdrażania wydajność kodu. Środowisko użytkownika wydajność aplikacji, należy również rozważyć. Na przykład zapewniając wykonanie operacji bez blokowania użytkownika wykonywanie innych działań może pomóc ulepszyć środowisko użytkownika.

Istnieje szereg technik zwiększenie wydajności i obserwowaną wydajność aplikacji skompilowanej za pomocą platformy Xamarin.Android. Obejmują one:

- [Optymalizowanie hierarchii układu](#optimizelayout)
- [Optymalizacja widoki List](#optimizelistviews)
- [Usuwanie programów obsługi zdarzeń w przypadku działań](#removeeventhandlers)
- [Limit czasu działania usługi](#limitservices)
- [Zwolnić zasoby przy powiadomieniu](#releaseresources)
- [Zwolnij zasoby interfejsu użytkownika jest ukryty.](#releaseresourcesuihidden)
- [Optymalizacja zasoby obrazów](#optimizeimages)
- [Usuwa zasoby nieużywane obrazów](#disposeimages)
- [Unikaj zmiennoprzecinkowe operacje arytmetyczne](#avoidfloats)
- [Zamknąć okna dialogowe](#dismissdialogs)


> [!NOTE]
> Przed przeczytaniem tego artykułu warto najpierw przeczytać artykuł [wydajności i Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md), omówiono w nim-platforma określonych technik w celu poprawy użycie pamięci i wydajność aplikacji utworzony za pomocą platformy Xamarin.

<a name="optimizelayout" />

## <a name="optimize-layout-hierarchies"></a>Optymalizowanie hierarchii układu

Każdy układ dodanych do aplikacji wymaga inicjowania, układu i rysunku. Przebieg układu może być kosztowne podczas zagnieżdżania [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) wystąpienia używające `weight` parametru, ponieważ każdego elementu podrzędnego będą mierzone dwa razy. Za pomocą wystąpień zagnieżdżonych `LinearLayout` może prowadzić do hierarchii dokładnego widoku, co może spowodować niską wydajność układów, które są zwiększony wiele razy, takie jak w [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/). W związku z tym jest ważne, że takie układów są zoptymalizowane pod, wydajności zostanie następnie mnożony korzyści.

Rozważmy na przykład [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) wiersza widoku listy, który zawiera ikonę, tytuł i opis. `LinearLayout` Będzie zawierać [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) i pionowym `LinearLayout` zawiera dwa [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) wystąpień:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="0dip"
        android:layout_weight="1"
        android:layout_height="fill_parent">
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:gravity="center_vertical"
            android:text="Mei tempor iuvaret ad." />
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:singleLine="true"
            android:ellipsize="marquee"
            android:text="Lorem ipsum dolor sit amet." />
    </LinearLayout>
</LinearLayout>
```

Ten układ jest poziomy 3 głębokość, a niepotrzebne, gdy zwiększony dla każdego [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) wiersza. Jednak może zostać ulepszony przez spłaszczanie układ, jak pokazano w poniższym przykładzie:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_alignParentTop="true"
        android:layout_alignParentBottom="true"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <TextView
        android:id="@+id/secondLine"
        android:layout_width="fill_parent"
        android:layout_height="25dip"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:singleLine="true"
        android:ellipsize="marquee"
        android:text="Lorem ipsum dolor sit amet." />
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:layout_above="@id/secondLine"
        android:layout_alignWithParentIfMissing="true"
        android:gravity="center_vertical"
        android:text="Mei tempor iuvaret ad." />
</RelativeLayout>
```

Poprzednie hierarchii poziomu 3 została zmniejszona do hierarchii poziom 2 i jeden [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) została zastąpiona dwoma [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) wystąpień. Będzie można uzyskać do znacznego zwiększenia wydajności podczas pompowania układu dla każdego [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) wiersza.

<a name="optimizelistviews" />

## <a name="optimize-list-views"></a>Optymalizacja widoki List

Użytkownicy oczekują przewijanie płynne i szybkie ładować [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) wystąpień. Jednak przewijanie wydajności mogą występować podczas każdego wiersza w widoku listy zawiera widok głęboko zagnieżdżone hierarchie lub wierszy widok listy zawiera złożone układów. Istnieją jednak techniki, których można użyć w celu uniknięcia słaby `ListView` wydajności:

- Ponowne użycie widoków wiersza, aby uzyskać więcej informacji, zobacz [ponowne użycie widoków wiersza](#reuserowviews).
- Spłaszczanie układów, jeśli jest to możliwe.
- Zawartość wiersza pamięci podręcznej, które są pobierane z usługi sieci web.
- Unikaj skalowanie obrazu.

Zbiorczo te techniki może pomóc zapewnić [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) wystąpień sprawnie przewijania.

<a name="reuserowviews" />

### <a name="reuse-row-views"></a>Ponowne użycie wiersza widoków

Podczas wyświetlania setki wierszy w [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/), byłoby odpady pamięci do utworzenia setki [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) obiektów, gdy tylko niewielką liczbę ich są wyświetlane na ekranie naraz. Zamiast tego tylko `View` obiekty widoczne w wierszach na ekranie mogą zostać załadowane do pamięci z **zawartości** ładowany w tych ponownie obiektów. Zapobiega to tworzeniu wystąpienia elementu setki dodatkowe obiekty, oszczędzając czas i pamięci.

W związku z tym gdy wiersz zniknie z ekranu jego widoku można umieścić w kolejce do ponownego użycia, jak pokazano w poniższym przykładzie:

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

Jako użytkownik przewija widok, [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) wywołania `GetView` należy przesłonić, aby zażądać nowych widoków do wyświetlenia — Jeśli jest dostępna przekazaniem nieużywane widoku w `convertView` parametru. Jeśli ta wartość jest `null` , a następnie kod tworzy nową [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) wystąpienia, w przeciwnym razie `convertView` można zresetować właściwości i ponownie.

Aby uzyskać więcej informacji, zobacz [wiersza widoku ponownego użycia](~/android/user-interface/layouts/list-view/populating.md) w [podczas wypełniania elementu ListView z danymi](~/android/user-interface/layouts/list-view/populating.md).

<a name="removeeventhandlers" />

## <a name="remove-event-handlers-in-activities"></a>Usuwanie programów obsługi zdarzeń w przypadku działań

Gdy działanie zostanie zniszczony w środowisku wykonawczym systemu Android, nadal można aktywności w środowisku wykonawczym Mono. W związku z tym usunąć obsługi zdarzeń do zewnętrznych obiektów w `Activity.OnPause` aby zapobiec środowiska uruchomieniowego utrzymywanie odwołania do działania, który został zlikwidowany.

W działaniu należy zadeklarować zawierający program(y) obsługi zdarzenia na poziomie klasy:

```csharp
EventHandler<UpdatingEventArgs> service1UpdateHandler;
```

Następnie implementuje obsługi w ramach działania, takie jak w `OnResume`:

```csharp
service1UpdateHandler = (object s, UpdatingEventArgs args) => {
    this.RunOnUiThread (() => {
        this.updateStatusText1.Text = args.Message;
    });
};
App.Current.Service1.Updated += service1UpdateHandler;
```

Gdy działanie jest kończona uruchomiona, `OnPause` jest wywoływana. W `OnPause` implementacji, Usuń obsługi w następujący sposób:

```csharp
App.Current.Service1.Updated -= service1UpdateHandler;
```

<a name="limitservices" />

## <a name="limit-the-lifespan-of-services"></a>Limit czasu działania usługi

Po uruchomieniu usługi dla systemu Android zapewnia uruchamianie procesu usługi. Dzięki temu proces kosztowne, ponieważ jego pamięci nie może być stronicowana ani używane w innych miejscach. Pozostawienie Usługa uruchomiona, gdy nie jest to wymagane w związku z tym zwiększa ryzyko aplikacji wykazujących pogorszenie wydajności z powodu ograniczeń pamięci. Może również wprowadzać przełączanie aplikacji mniej wydajne, ponieważ zmniejsza liczbę procesów, które mogą buforować systemu Android.

Czas działania usługi mogą być ograniczone za pomocą `IntentService`, która kończy się po został obsłużony celem, który je zainicjował.

<a name="releaseresources" />

## <a name="release-resources-when-notified"></a>Zwolnić zasoby przy powiadomieniu

Podczas cyklu życia aplikacji [ `OnTrimMemory` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnTrimMemory/p/Android.Content.TrimMemory/) wywołania zwrotnego zawiera powiadomienie, gdy jest za mało pamięci urządzenia. To wywołanie zwrotne powinny zostać wdrożone do nasłuchiwania poziomu powiadomień pamięci:

- [`TrimMemoryRunningModerate`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryRunningModerate/) — Aplikacja *może* do zwolnienia niektórych niepotrzebnych zasobów.
- [`TrimMemoryRunningLow`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryRunningLow/) — Aplikacja *powinien* wersji niepotrzebne zasobów.
- [`TrimMemoryRunningCritical`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryRunningCritical/) — Aplikacja *powinien* zlecenia może tyle procesów niekrytyczne.

Ponadto podczas procesu aplikacji są buforowane, poziomu powiadomień pamięci może być odbierany przez [ `OnTrimMemory` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnTrimMemory/p/Android.Content.TrimMemory/) wywołania zwrotnego:

- [`TrimMemoryBackground`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryBackground/) — zwolnić zasoby, które mogą być szybkie i skuteczne odbudować po powrocie do aplikacji.
- [`TrimMemoryModerate`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryModerate/) — zwolnienie zasobów pomaga zachować inne procesy buforowane w celu zapewnienia lepszej ogólnej wydajności systemu.
- [`TrimMemoryComplete`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryComplete/) — Proces aplikacji wkrótce zostanie zakończony, jeśli większej ilości pamięci nie jest wkrótce.

Powiadomienia powinien odpowiedział na zwalniając zasobów na podstawie odebranego poziomu.

<a name="releaseresourcesuihidden" />

## <a name="release-resources-when-the-user-interface-is-hidden"></a>Zwolnij zasoby interfejsu użytkownika jest ukryty.

Zwalnia zasoby używane przez interfejs użytkownika aplikacji, gdy użytkownik przechodzi do innej aplikacji, ponieważ może znacznie zwiększyć pojemność dla systemu Android dla pamięci podręcznej procesów, które z kolei może mieć wpływ na jakość środowiska użytkownika.

Aby otrzymać powiadomienie, gdy użytkownik opuszcza interfejsu użytkownika, należy zaimplementować [ `OnTrimMemory` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnTrimMemory/p/Android.Content.TrimMemory/) wywołania zwrotnego w `Activity` klas i nasłuchiwania [ `TrimMemoryUiHidden` ](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryUiHidden/) poziomie, co oznacza, że interfejs użytkownika ukryty. To powiadomienie zostanie odebrana tylko wtedy, gdy *wszystkie* składniki interfejsu użytkownika aplikacji będą ukryte przez użytkownika. Udostępnia zasoby interfejsu użytkownika po odebraniu tego powiadomienia zapewnia, że jeśli użytkownik przejdzie ponownie w innej aktywności w aplikacji, zasoby interfejsu użytkownika są nadal dostępne na wznowienie działania.

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optymalizacja zasoby obrazów

Obrazy są niektóre z najdroższych zasobów korzystających z aplikacji, a często są przechwytywane w wysokiej rozdzielczości. W związku z tym podczas wyświetlania obrazu Wyświetl ją z rozdzielczością wymagane na ekranie urządzenia. Jeśli obraz jest o rozdzielczości wyższej niż ekran, powinien być skalowany w dół.

Aby uzyskać więcej informacji, zobacz [optymalizacji zasobów obrazu](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) w [wydajności i Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md) przewodnik.

<a name="disposeimages" />

## <a name="dispose-of-unused-image-resources"></a>Usuwa zasoby nieużywane obrazów

Aby zapisać zużycie pamięci, jest dobrze do usuwania zasobów dużych obrazów, które nie są już potrzebne. Jest jednak należy się upewnić, że obrazy są poprawnie usuwane. Zamiast jawnego `.Dispose()` wywołania, możesz korzystać z [przy użyciu](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-statement) instrukcje, aby zapewnić prawidłowe korzystanie z `IDisposable` obiektów. 

Na przykład [mapy bitowej](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) klasa implementuje `IDisposable`. Zawijanie tworzenia wystąpienia elementu `BitMap` obiektu w `using` bloku gwarantuje, że jego zostaną usunięte prawidłowo przy zamykaniu z bloku:

```csharp
using (Bitmap smallPic = BitmapFactory.DecodeByteArray(smallImageByte, 0, smallImageByte.Length))
{
    // Use the smallPic bit map here
}
```

Aby uzyskać więcej informacji na temat zwalniania jednorazowe zasoby, zobacz [wersji interfejsu IDisposable zasobów](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).  


<a name="avoidfloats" />

## <a name="avoid-floating-point-arithmetic"></a>Unikaj zmiennoprzecinkowe operacje arytmetyczne

Na urządzeniach z systemem Android zmiennoprzecinkowe arytmetyczne wynosi około 2 x jest mniejsza niż liczba całkowita arytmetyczne. W związku z tym Zamień zmiennoprzecinkowe arytmetyczne liczbę całkowitą arytmetyczne, jeśli to możliwe. Jednak nie ma różnic czas wykonania między `float` i `double` arytmetycznego na ostatnie sprzętu.

> [!NOTE]
> Nawet w przypadku arytmetyczne całkowitą sprzętu Brak niektórych procesorach dzielenia możliwości. W związku z tym operacji dzielenia i modulo całkowitą często są wykonywane w oprogramowaniu.

<a name="dismissdialogs" />

## <a name="dismiss-dialogs"></a>Zamknąć okna dialogowe

Korzystając z [ `ProgressDialog` ](https://developer.xamarin.com/api/type/Android.App.ProgressDialog/) klasy (lub wszystkie okna dialogowego lub alertu), zamiast wywoływać metodę [ `Hide` ](https://developer.xamarin.com/api/member/Android.App.Dialog.Hide()/) wywołanie metody po zakończeniu okna dialogowego cel [ `Dismiss` ](https://developer.xamarin.com/api/member/Android.App.Dialog.Dismiss()/) metody. W przeciwnym razie okno dialogowe będzie nadal aktywności i będzie wyciek działania, przytrzymując odwołanie do niej.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano i opisem techniki zwiększania wydajności aplikacji skompilowanej za pomocą platformy Xamarin.Android. Zbiorczo te techniki znacznie zmniejszyć ilość pracy wykonywana przez Procesora i ilości pamięci używanej przez aplikację.


## <a name="related-links"></a>Linki pokrewne

- [Wydajność i Platform](~/cross-platform/deploy-test/memory-perf-best-practices.md)
