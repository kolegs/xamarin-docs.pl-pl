---
title: "Wskazówki — tworzenie z kartami interfejsu użytkownika z TabHost"
description: "Ten artykuł przeprowadzi tworzenie z kartami interfejsu użytkownika w platformy Xamarin.Android przy użyciu interfejsu API TabHost."
ms.topic: article
ms.prod: xamarin
ms.assetid: AD6E2173-974E-477C-940F-0CAB5E53326D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2dd397e824ce7735be4421c3f258852de3f77ecb
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="walkthrough---creating-a-tabbed-ui-with-tabhost"></a>Wskazówki — tworzenie z kartami interfejsu użytkownika z TabHost

_Ten artykuł przeprowadzi tworzenie z kartami interfejsu użytkownika w platformy Xamarin.Android przy użyciu interfejsu API TabHost._

> [!NOTE]
> `TabHost` to interfejs API stare, która została zastąpiona przez firmę Google. Deweloperzy są zachęcani do kompilacji z kartami aplikacji przy użyciu [elementów nadrzędnych](~/android/user-interface/controls/action-bar.md). `ActionBar` Jest dostępna we wszystkich wersji systemu android. Została wprowadzona w 3.0 dla systemu Android (interfejs API na poziomie 11), a była powrotem przenoszone do 2.2 systemu Android (interfejs API na poziomie 8) i Android 2.3 (interfejs API na poziomie 10) w [w wersji 7 AppCompat biblioteki](http://developer.android.com/tools/support-library/features.html#v7-appcompat), które są dostępne dla platformy Xamarin.Android przy użyciu [Xamarin Biblioteka obsługi systemu android - 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) pakietu.

Ten artykuł przeprowadzi tworzenie z kartami interfejsu użytkownika w platformy Xamarin.Android przy użyciu `TabHost` interfejsu API. Jest to starsza interfejsu API, która jest dostępna we wszystkich wersjach systemu android. W tym przykładzie aplikacja zostanie utworzona z trzema kartami z logiką dla każdej karty są hermetyzowane w działaniu.
Poniższy zrzut ekranu znajduje się przykład aplikacji, która zostanie utworzona:

![Zrzut ekranu aplikacji z wieloma kartami](creating-a-tabbed-ui-images/image02.png)


## <a name="creating-the-application"></a>Tworzenie aplikacji

Pobierz i Rozpakuj [TabHostWalkthrough](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/).
Ten projekt służy jako punkt początkowy dla aplikacji i zawiera kilka obrazów. Należy zbadać tego projektu, pojawi się już utworzyliśmy zasobów obiektów drawable dla ikon kartę.

Pierwszy teraz zaktualizować plik układu **Resources/Layout/Main.axml** który będzie hostem kart. Edytuj **Resources/Layout/Main.axml** i Wstaw następujący kod XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TabHost xmlns:android="http://schemas.android.com/apk/res/android"
         android:id="@android:id/tabhost"
         android:layout_width="fill_parent"
         android:layout_height="fill_parent">
    <LinearLayout
            android:orientation="vertical"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:padding="5dp">
        <TabWidget
                android:id="@android:id/tabs"
                android:layout_width="fill_parent"
                android:layout_height="wrap_content" />
        <FrameLayout
                android:id="@android:id/tabcontent"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:padding="5dp" />
    </LinearLayout>
</TabHost>
```

Poniższy zrzut ekranu przedstawia układ w Projektancie Xamarin:

[![Zrzut ekranu przedstawiający układu TabHost w Projektancie Xamarin](creating-a-tabbed-ui-images/image04-sml.png)](creating-a-tabbed-ui-images/image04.png#lightbox)

TabHost musi mieć dwa widoki podrzędnych w nim: `TabWidget` i `FrameLayout`. Położenie `TabWidget` i `FrameLayout` pionie wewnątrz `TabHost`, `LinearLayout` jest używany. FrameLayout jest, gdzie dla każdej karty należy wstawić zawartość, która jest pusta ponieważ `TabHost` zostanie automatycznie osadzić każde działanie w czasie wykonywania. Istnieje kilka reguł, które muszą być spełnione po przejściu do tworzenia układu dla interfejsów użytkownika z kartami:

-   `TabHost` Musi mieć identyfikator `@android:id/tabhost`.

-   `TabWidget` Musi mieć identyfikator `@android:id/tabs`.

-   `FrameLayout` Musi mieć identyfikator `@android:id/tabcontent`.

-   `TabHost` wymaga działalności, która zarządza odziedziczone `TabActivity`. W związku z tym należy do podklasy `TabActivity` tutaj &ndash; zwykłej aktywności nie będą działać.

Przeprowadź edycję pliku **MainActivity.cs** , aby klasa `MainActivity` podklasy `TabActivity` pokazane na poniższy fragment kodu:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/ic_launcher")]
public class MainActivity : TabActivity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
    }
}
```

Utwórz cztery osobnych klas działania w projekcie: `MyScheduleActivity`, `SessionsActivity`, `SpeakersActivity`, i `WhatsOnActivity`. Każde działanie spowoduje, że interfejsu użytkownika karty. Teraz tych działań będzie możliwe skrótowa, która wyświetla `TextView` z prostego komunikatu. Edytuj kod w poszczególnych działań zawiera następujące `OnCreate` implementacji:

```csharp
[Activity]
public class MyScheduleActivity : Activity
{
    protected override void OnCreate (Bundle savedInstanceState)
    {
        base.OnCreate (savedInstanceState);
        TextView textview = new TextView (this);
        textview.Text = "This is the My Schedule tab";
        SetContentView (textview);
    }
}
```

Zwróć uwagę, że powyższe kodu nie używa pliku układu. Po prostu tworzy `TextView` z tekstem i który ustawia `TextView` jako widok zawartości. Zduplikowana to dla każdej z pozostałych działań trzy.

Firma Microsoft będzie następnie przypisać ikony do każdej karty. Każda karta wymaga dwóch ikon &ndash; jeden dla wybranego stanu i jeden dla stanu niezaznaczone. Przykład ikony te dwie różne można wyświetlić w następujące dwa obrazy (niezbędne ikony dla tej aplikacji zostały już dodane do projektu przykładowe):

![Zrzut ekranu przedstawiający wybrany stan i stan niezaznaczone ikony](creating-a-tabbed-ui-images/tab-icons.png)


Firma Microsoft przypisze obiektów drawable zasobów kart ikona, definiując *Drawablelistystan*. Stan listy drawables są specjalne zasobów obiektów drawable, które są zdefiniowane w pliku XML i pozwalają określić różne obrazów, które są specyficzne dla stanu tego elementu. W tym przykładzie istnieje jeden obraz, który jest używany po wybraniu karty, a druga jest używany, gdy karcie nie jest zaznaczona. Aby zaoszczędzić czas, niezbędne drawables stan listy zostały dodane do projektu. Poniższa lista zawiera pliki i XML każdego zawiera:

-   `ic_tab_my_schedule.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

-   `ic_tab_sessions.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_sessions_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_sessions_unselected"/>
    </selector>
    ```

-   `ic_tab_speakers.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_speakers_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_speakers_unselected"/>
    </selector>
    ```

-   `ic_tab_whats_on.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

Karty są dodawane do `TabHost` programowo, który jest bardzo powtarzających się zadań. Aby ułatwić z tym, dodaj następującą metodę do klasy `MainActivity`:

```csharp
private void CreateTab(Type activityType, string tag, string label, int drawableId )
{
    var intent = new Intent(this, activityType);
    intent.AddFlags(ActivityFlags.NewTask);

    var spec = TabHost.NewTabSpec(tag);
    var drawableIcon = Resources.GetDrawable(drawableId);
    spec.SetIndicator(label, drawableIcon);
    spec.SetContent(intent);

    TabHost.AddTab(spec);
}
```

Każda karta `TabHost` jest reprezentowany przez wystąpienie elementu `TabHost.TabSpec` klasy. To wystąpienie zawiera meta dane niezbędne do renderowania kartę, w szczególności:

-   **Tekst i ikona** &ndash; mają być wyświetlane w `TabWidget`.

-   **Karta zawartość** &ndash; może to być albo `Activity` lub `View` i nie jest wyświetlany po wybraniu karty.

-   **Unikatowy Tag** &ndash; każdej karcie musi mieć unikatowy tag przypisane do niej.

Możemy dodać `TabHost.TabSpec` wystąpienia dla każdej karty w naszej aplikacji. Umożliwia to zrobić w następnym kroku.

Zaktualizuj metodę `OnCreate` w `MainActivity` tak, aby podobny do następującego kodu:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateTab(typeof(WhatsOnActivity), "whats_on", "What's On", Resource.Drawable.ic_tab_whats_on);
    CreateTab(typeof(SpeakersActivity), "speakers", "Speakers", Resource.Drawable.ic_tab_speakers);
    CreateTab(typeof(SessionsActivity), "sessions", "Sessions", Resource.Drawable.ic_tab_sessions);
    CreateTab(typeof(MyScheduleActivity), "my_schedule", "My Schedule", Resource.Drawable.ic_tab_my_schedule);
}
```

Uruchom aplikację. Aplikacja powinna przypominać zrzucie ekranu pokazano na początku tego przewodnika.

To już wszystko! Został utworzony z kartami aplikacji, która zapewnia łatwy sposób przejdź do różnych części aplikacji.



## <a name="summary"></a>Podsumowanie

W tym rozdziale omówiono układów z kartami i zapoznaje proces tworzenia aplikacji z kartami. Instruktaż przedstawiono sposób użycia `TabActivity` można zwiększyć układ pliku tego hosting `TabHost` i `TabWidget`. `TabHost` Następnie wypełniono Kolekcja `TabHost.TabSpec` obiektów, które mają być używane przez `TabHost` w czasie wykonywania można utworzyć wystąpienia działania, które będą używane na każdej karcie.



## <a name="related-links"></a>Linki pokrewne

- [TabHostWalkthrough (przykład)](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Pakiet NuGet AppCompat w wersji 7 biblioteki obsługi systemu android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Biblioteka appcompat w wersji 7](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
