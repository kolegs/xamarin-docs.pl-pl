---
title: Fragmenty wskazówki — część 2
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 58291388d375a4fd9273c8e0cd46db3799966766
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="fragments-walkthrough-ndash-landscape"></a>Fragmenty wskazówki &ndash; orientacji poziomej

[Wskazówki fragmenty &ndash; część 1](./walkthrough.md) przedstawiono sposób tworzenia i używania fragmentów w aplikacji systemu Android, którego celem jest mniejszych ekranach na telefonie. Następnym krokiem w ramach tego przewodnika jest aby zmodyfikować aplikację, aby skorzystać z bardzo poziome miejsca na tablecie &ndash; będzie jedno działanie, które będzie zawsze listy odtwarzania ( `TitlesFragment`) i `PlayQuoteFragment` będą dodawane dynamicznie do działania w odpowiedzi na wybór dokonany przez użytkownika:

[![Aplikacji uruchomionej na tablecie](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

Telefony, które działają w trybie krajobraz będzie również korzystać ze to rozszerzenie:

[![Aplikacji uruchomionej na telefonie z systemem Android w trybie krajobraz](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>Aktualizowanie aplikacji do obsługi orientacji poziomej

Następujące modyfikacje zostaną zależą od pracy, które zostało wykonane w [wskazówki fragmenty - telefonu](./walkthrough.md)

1. Utwórz układ alternatywny do wyświetlenia zarówno `TitlesFragment` i `PlayQuoteFragment`.
1. Aktualizacja `TitlesFragment` wykryć, czy urządzenia są wyświetlane jednocześnie obu fragmentach i odpowiednio zmień zachowanie.
1. Aktualizacja `PlayQuoteActivity` zamknięcia, gdy urządzenie jest w trybie krajobraz.

## <a name="1-create-an-alternate-layout"></a>1. Utwórz układ alternatywnej

Po utworzeniu działania głównego na urządzeniu z systemem Android, Android podejmie decyzję, którego układu, aby załadować oparte na orientacji urządzenia. Domyślnie zapewni Android **Resources/layout/activity_main.axml** pliku układu. W przypadku urządzeń, które są ładowane w trybie krajobraz zapewni Android **Resources/layout-land/activity_main.axml** pliku układu. Przewodnik na [Android zasobów](/xamarin/android/app-fundamentals/resources-in-android) zawiera więcej informacji na temat sposobu Android decyduje o tym, jakie zasobów plików do załadowania aplikacji.

Utwórz układ alternatywny którego element docelowy **pozioma** orientacji, wykonując kroki opisane w [alternatywne układy](/xamarin/android/user-interface/android-designer/alternative-layout-views) przewodnik. To należy dodać nowy plik zasobu układu do projektu, **Resources/layout/activity_main.axml**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Alternatywne układu w Eksploratorze rozwiązań](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Układ alternatywne rozwiązanie konsoli](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

Po utworzeniu układu alternatywne, Edytuj źródło pliku **Resources/layout-land/activity_main.axml** , aby było to XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/two_fragments_layout"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_weight="1"
        android:layout_width="0px"
        android:layout_height="match_parent" />

    <FrameLayout android:id="@+id/playquote_container"
            android:layout_weight="1"
            android:layout_width="0px"
            android:layout_height="match_parent"
             />
</LinearLayout>
```

Widok główny działania jest podany identyfikator zasobu `two_fragments_layout` i ma dwa widoki podrzędne, `fragment` i `FrameLayout`. Gdy `fragment` statycznie załadowanym `FrameLayout` działa jako "zastępczy", który zostanie zastąpiony w czasie wykonywania przez `PlayQuoteFragment`. Zawsze nowe play jest zaznaczona w `TitlesFragment`, `playquote_container` zostaną zaktualizowane o nowe wystąpienie klasy `PlayQuoteFragment`.

Zajmie wszystkich podrzędnych widoków pełna wysokość jego elementu nadrzędnego. Szerokość każdego widok podrzędny jest kontrolowany przez `android:layout_weight` i `android:layout_width` atrybutów. W tym przykładzie każdy widok podrzędny będą zajmować 50% szerokość zapewniają przez element nadrzędny. Zobacz [dokumentów firmy Google na LinearLayout](https://developer.android.com/guide/topics/ui/layout/linear.html) szczegółowe informacje o _wagi układu_.

## <a name="2-changes-to-titlesfragment"></a>2. Zmiany TitlesFragment

Po utworzeniu alternatywny układu jest niezbędne do aktualizacji `TitlesFragment`. Gdy aplikacja są wyświetlane dwa fragmenty na jedno działanie, następnie `TitlesFragment` powinny zostać załadowane `PlayQuoteFragment` w działania nadrzędnego. W przeciwnym razie `TitlesFragment` należy uruchomić `PlayQuoteActivity` który host `PlayQuoteFragment`. Flaga wartości logicznej pomoże `TitlesFragment` określają zachowanie, które należy go używać. Ta flaga zostaną zainicjowane w `OnActivityCreated` metody.

Najpierw Dodaj zmienną instance w górnej części `TitlesFragment` klasy:

```csharp
bool showingTwoFragments;
```

Następnie dodaj poniższy fragment kodu do `OnActivityCreated` można zainicjować zmiennej: 

```csharp
var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
showingTwoFragments = quoteContainer != null &&
                      quoteContainer.Visibility == ViewStates.Visible;
if (showingTwoFragments)
{
    ListView.ChoiceMode = ChoiceMode.Single;
    ShowPlayQuote(selectedPlayId);
}
```

Jeśli urządzenie działa w trybie krajobraz, a następnie `FrameLayout` o identyfikatorze zasobu `playquote_container` będą widoczne na ekranie, `showingTwoFragments` zostanie zainicjowana w celu `true`. Jeśli urządzenie działa w trybie portret następnie `playquote_container` nie będzie na ekranie, więc `showingTwoFragments` będzie `false`.

`ShowPlayQuote` — Metoda będzie konieczne zmiany sposobu jej wyświetlania oferty &ndash; fragmentu lub Uruchom nowe działanie.  Aktualizacja `ShowPlayQuote` metodę, aby załadować fragmentu przy wyświetlaniu dwa fragmenty, w przeciwnym razie należy uruchomić działania:

```csharp
void ShowPlayQuote(int playId)
{
    selectedPlayId = playId;
    if (showingTwoFragments)
    {
        ListView.SetItemChecked(selectedPlayId, true);

        var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

        if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
        {
            var container = Activity.FindViewById(Resource.Id.playquote_container);
            var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

            FragmentTransaction ft = FragmentManager.BeginTransaction();
            ft.Replace(Resource.Id.playquote_container, quoteFrag);
            ft.Commit();
        }
    }
    else
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

Jeśli użytkownik wybrał play, który jest inny niż ten, który jest aktualnie wyświetlany w `PlayQuoteFragment`, następnie nowy `PlayQuoteFragment` jest tworzony i zastąpi zawartość `playquote_container` w kontekście `FragmentTransaction`.

### <a name="complete-code-for-titlesfragment"></a>Kompletny kod dla TitlesFragment

Po zakończeniu wszystkich wcześniejszych zmian do `TitlesFragment`, pełnej klasy powinna być zgodna ten kod:

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;
    bool showingTwoFragments;

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }


        var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
        showingTwoFragments = quoteContainer != null &&
                                quoteContainer.Visibility == ViewStates.Visible;
        if (showingTwoFragments)
        {
            ListView.ChoiceMode = ChoiceMode.Single;
            ShowPlayQuote(selectedPlayId);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        selectedPlayId = playId;
        if (showingTwoFragments)
        {
            ListView.SetItemChecked(selectedPlayId, true);

            var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

            if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
            {
                var container = Activity.FindViewById(Resource.Id.playquote_container);
                var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

                FragmentTransaction ft = FragmentManager.BeginTransaction();
                ft.Replace(Resource.Id.playquote_container, quoteFrag);
                ft.AddToBackStack(null);
                ft.SetTransition(FragmentTransit.FragmentFade);
                ft.Commit();
            }
        }
        else
        {
            var intent = new Intent(Activity, typeof(PlayQuoteActivity));
            intent.PutExtra("current_play_id", playId);
            StartActivity(intent);
        }
    }
}
```

## <a name="3-changes-to-playquoteactivity"></a>3. Zmiany PlayQuoteActivity

Brak szczegółów końcowego automatyzującą: `PlayQuoteActivity` nie jest konieczne, gdy urządzenie jest w trybie krajobraz. Jeśli urządzenie jest w trybie krajobraz `PlayQuoteActivity` nie powinny być widoczne. Aktualizacja `OnCreate` metody `PlayQuoteActivity` tak, aby zamknie się. Ten kod jest z ostateczną wersją `PlayQuoteActivity.OnCreate`:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    base.OnCreate(savedInstanceState);

    if (Resources.Configuration.Orientation == Android.Content.Res.Orientation.Landscape)
    {
        Finish();
    }

    var playId = Intent.Extras.GetInt("current_play_id", 0);
    var playQuoteFrag = PlayQuoteFragment.NewInstance(playId);
    FragmentManager.BeginTransaction()
                    .Add(Android.Resource.Id.Content, playQuoteFrag)
                    .Commit();
}
```

Ta modyfikacja dodaje wyboru dla orientacji urządzenia. Jeśli jest ona w trybie krajobraz `PlayQuoteActivity` spowoduje zamknięcie się.

## <a name="4-run-the-application"></a>4. Uruchamianie aplikacji

Gdy te zmiany zostaną ukończone, uruchom aplikację, obracania urządzenia na poziomą trybu (Jeśli to konieczne), a następnie wybierz odtwarzania. Oferty powinna być wyświetlana na tej samej ekranu w formie listy odtwarzania:

[![Aplikacji uruchomionej na telefonie z systemem Android w trybie krajobraz](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Aplikacji uruchomionej na tablet z systemem Android](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
