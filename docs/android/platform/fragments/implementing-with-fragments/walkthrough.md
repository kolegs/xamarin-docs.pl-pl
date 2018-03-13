---
title: "Wskazówki"
ms.topic: article
ms.prod: xamarin
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: e5c058f173f64efe4a5c777872e9ea67120115f0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough"></a>Wskazówki

W poniższych krokach Podstawowa aplikacja zostanie utworzona z fragmentów. Pierwszym krokiem jest Utwórz nowe platformy Xamarin.Android dla systemu android.

## <a name="1-create-a-project"></a>1. Tworzenie projektu

Utwórz nowy projekt platformy Xamarin.Android o nazwie **FragmentSample**. **Minimum Android** wersja powinna być ustawiona do systemu Android w wersji 3.1 lub nowszej, jak pokazano na poniższej ilustracji:

[![Ustawienie wersji Minimum Android](walkthrough-images/00.png)](walkthrough-images/00.png#lightbox)


## <a name="2-create-the-mainactivity"></a>2. Utwórz MainActivity

Następnie należy utworzyć `MainActivity` klasy. Jest to uruchomienia działania aplikacji. To działanie będzie obsługiwać jeden lub oba fragmentów, w zależności od wielkości ekranu. `MainActivity` Spowoduje to zrobić, podczas ładowania pliku układu, który jest odpowiedni dla urządzenia.

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```

> [!NOTE]
> `Note:` Podklasy fragmentu musi mieć domyślnego publicznego konstruktora nie argumentów.

## <a name="3-create-the-layout-files"></a>3. Tworzenie plików układu

Dwóch różnych rozmiarów ekranu wymaga dwóch różnych układu plików. Więc warto utworzyć nowego folderu, **zasobów/układu-Large**i Utwórz nowy układ o nazwie **activity_main.axml**. Firma Microsoft będzie także zmienić nazwę pliku układu domyślne jako **Resources/Layout/activity_main.axml**. Po wprowadzeniu tych zmian foldery układu powinien wyglądać na poniższym zrzucie ekranu:

[![Zrzut ekranu przedstawiający układu folderów w środowisku IDE](walkthrough-images/01.png)](walkthrough-images/01.png#lightbox)


Wszystkie urządzenia będą obciążenia i użycia do pliku układu **zasobów/układ**.
Jest bardzo prosty układ wyświetli `TitlesFragment`:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_width="fill_parent"
           android:layout_height="fill_parent" />
</LinearLayout>
```

W przypadku urządzeń mających duży ekran Android zostanie załadowany do pliku układu **zasobów/układu-Large**. Zawartość układu dla tabletów wygląda następująco:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_weight="1"
           android:layout_width="0px"
           android:layout_height="match_parent" />
 <FrameLayout android:id="@+id/details"
              android:layout_weight="1"
              android:layout_width="0px"
              android:layout_height="match_parent" />
</LinearLayout>
```

Plik układu dla większych ekranów różni się nieznacznie. Nie tylko `TitlesFragment` wyświetlane w tym pliku układu, ale `FrameLayout` jest dodawany do prawej strony, obok fragmentu. Na większych monitorach `DetailsFragment` dodać programistycznie do `MainActivity` użytkownik wybrał odtwarzania. Później wyjaśniamy bardziej szczegółowo jak to zrobić.

Android 3.2 wprowadzono nowy sposób, aby określić układów ekranu głównego. Te nowe kwalifikatory Określ ilość miejsca musi układ, a nie rozmiaru ekranu. Jeśli tej aplikacji zostało przeznaczone do uruchamiania tylko dla systemu Android 3.2 lub nowszego, możemy utworzyć **zasobów/układu sw600dp** folderu (zamiast folderu **zasobów/układu — duże**) dla pliku układu  **activity_main.axml**. Czy można załadować pliku zasobów przez wszystkie urządzenia, które szerokość minimalna ekranu 600 pikseli niezależny od gęstości. Jednak, ponieważ ta aplikacja jest ustawiona na docelowej 3.1 dla systemu Android lub wyższym, używa starszej kwalifikator zasobów.

## <a name="4-create-the-titlesfragment"></a>4. Utwórz TitlesFragment

`TitlesFragment` będzie Wyświetla tytuły różnych odtwarza, więc warto dodać nowy fragment do projektu o nazwie `TitlesFragment`:

[![Dodawanie nowego fragmentu do projektu TitlesFragment](walkthrough-images/02.png)](walkthrough-images/02.png#lightbox)

Po `TitlesFragment` został dodany, będziemy zmieniać klasy tak, aby dziedziczyła ona z `Android.App.ListFragment`. `ListFragment` jest typem specjalne fragmentu, która obejmuje funkcjonalność listy.
`TitlesFragment` zostanie również zastąpić `OnActivityCreated` (innej metody cyklu życia fragmentu) i podaj `Adapter` który `ListFragment` będzie używany do wypełnienia listy:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
   base.OnActivityCreated(savedInstanceState);
   var adapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemChecked, Shakespeare.Titles);
   ListAdapter = adapter;
   if (savedInstanceState != null)
   {
       _currentPlayId = savedInstanceState.GetInt("current_play_id", 0);
   }
   var detailsFrame = Activity.FindViewById<View>(Resource.Id.details);
   _isDualPane = detailsFrame != null && detailsFrame.Visibility == ViewStates.Visible;
   if (_isDualPane)
   {
       ListView.ChoiceMode = (int) ChoiceMode.Single;
       ShowDetails(_currentPlayId);
   }
}
```

Jak wcześniej wspomniano, naszej aplikacji ma dwa układów `MainActivity`. Kod w `OnActivityCreated` wykrywa obecność `FrameLayout` i określa, który plik układu został załadowany. Jeśli `FrameLayout` istnieje w układzie, a następnie `_isDualPane` flaga jest ustawiona na `true`. `_isDualPane` Flaga jest wykorzystywana w innym miejscu w działaniu, w szczególności przez `ShowDetails` metody. `ShowDetails` Metoda zostaną opisane bardziej szczegółowo poniżej.

`TitlesFragment` Lista i musi odpowiadać na wybór użytkownika na liście. Aby to zrobić, `TitlesFragment` spowoduje zastąpienie metody `OnListItemClick`. Wewnątrz `OnListItemClick`, nowy `DetailsFragment` zostanie utworzona i wyświetlane w `FrameLayout`, programowo. Odpowiedni kod wewnątrz `TitlesFragment` jest:

```csharp
public override void OnListItemClick(ListView l, View v, int position, long id)
{
   ShowDetails(position);
}
private void ShowDetails(int playId)
{
   _currentPlayId = playId;
   if (_isDualPane)
   {
       // We can display everything in place with fragments.
       // Have the list highlight this item and show the data.
       ListView.SetItemChecked(playId, true);
       // Check what fragment is shown, replace if needed.
       var details = FragmentManager.FindFragmentById(Resource.Id.details) as DetailsFragment;
       if (details == null || details.ShownPlayId != playId)
       {
           // Make new fragment to show this selection.
           details = DetailsFragment.NewInstance(playId);
           // Execute a transaction, replacing any existing
           // fragment with this one inside the frame.
           var ft = FragmentManager.BeginTransaction();
           ft.Replace(Resource.Id.details, details);
           ft.SetTransition(FragmentTransaction.TransitFragmentFade);
           ft.Commit();
       }
   }
   else
   {
       // Otherwise we need to launch a new Activity to display
       // the dialog fragment with selected text.
       var intent = new Intent();
       intent.SetClass(Activity, typeof (DetailsActivity));
       intent.PutExtra("current_play_id", playId);
       StartActivity(intent);
   }
}
```

Kod z urządzenia określa sposób formatowania i wyświetlania oferty z wybranych play. W przypadku tabletów `_isDualPane` będzie można ustawić flagi `true`, i co oferty będą wyświetlane obok `TitlesFragment`. Jeśli wybrane play `id` nie jest wyświetlana, następnie nowy `DetailsFragment` jest utworzony, a następnie ładowane do `FrameLayout` w działaniu. W przypadku innych urządzeń, które nie mają dużym &ndash; telefonach, na przykład &ndash; `isDualPane` zostanie ustawiona do `false` tak nowy `DetailsActivity` zostanie uruchomiony.


## <a name="5-create-the-detailsactivity"></a>5. Utwórz DetailsActivity

`DetailsActivity` Wyświetla `DetailsFragment` mniejszych urządzeń. Aby to sprawdzić, najpierw dodamy nowe działanie do projektu o nazwie `DetailsActivity`. `DetailsActivity` jest bardzo proste działanie. Spowoduje utworzenie i następnie udostępnić nową `DetailsFragment` odtwarzania `id` wysłanej:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("current_play_id", 0);

       var details = DetailsFragment.NewInstance(index); // DetailsFragment.NewInstance is a factory method to create a Details Fragment
       var fragmentTransaction = FragmentManager.BeginTransaction();
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

Należy zauważyć, że plik układu nie jest ładowana dla `DetailsActivity`. Zamiast tego `DetailsFragment` jest ładowany do widoku głównego działania. Ten widok główny mają specjalne identyfikator `Android.Resource.Id.Content`. Nowy `DetailFragment` jest tworzony i następnie dodać do tego widoku głównego wewnątrz `FragmentTransaction` tworzone przez działania `FragmentManager`.


## <a name="6-create-the-detailsfragment"></a>6. Utwórz DetailsFragment

Teraz możemy dodać inny fragment do aplikacji o nazwie `DetailsFragment`. Ten fragment wyświetli ofertę z wybranych play. Poniższy kod przedstawia pełną `DetailsFragment`:

```csharp
internal class DetailsFragment : Fragment
{
   public static DetailsFragment NewInstance(int playId)
   {
       var detailsFrag = new DetailsFragment {Arguments = new Bundle()};
       detailsFrag.Arguments.PutInt("current_play_id", playId);
       return detailsFrag;
   }
   public int ShownPlayId
   {
       get { return Arguments.GetInt("current_play_id", 0); }
   }
   public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
   {
       if (container == null)
       {
           // Currently in a layout without a container, so no reason to create our view.
           return null;
       }
       var scroller = new ScrollView(Activity);
       var text = new TextView(Activity);
       var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
       text.SetPadding(padding, padding, padding, padding);
       text.TextSize = 24;
       text.Text = Shakespeare.Dialogue[ShownPlayId];
       scroller.AddView(text);
       return scroller;
   }
}
```

Aby `DetailsFragment` działał poprawnie, musi mieć indeks play, który wybrano w `TitlesFragment`. Istnieje wiele sposobów, aby zapewnić tę wartość na `DetailsFragment`; w tym przykładzie odtwarzania `Id` jest umieszczany w pakiecie i że pakiet jest przechowywany z właściwością argumentów wystąpienia `DetailsFragment`. Właściwość `ShownPlayId` jest udostępniana jako udogodnienie &ndash; będzie on używany przez wystąpienia `DetailsFragment` do pobierania tej wartości z pakietu.

`OnCreateView` jest wywoływane, gdy fragmentu powinien być rysowany interfejs użytkownika i powinien zwrócić `Android.Views.View` obiektu. W większości przypadków jest to `View` zwiększony z istniejącego pliku układu. W przypadku powyższy przykład fragment utworzy programowo widoku, który będzie używany do wyświetlenia.

Gratulacje! Teraz po utworzeniu aplikacji korzystającej z fragmentów do upraszczają Programowanie w rozmiarach.

W [następnej sekcji](supporting-pre-honeycomb.md), zamierzamy rozszerzyć tej aplikacji, dzięki czemu będzie ona działać na urządzeniach Android wstępne 3.0.

