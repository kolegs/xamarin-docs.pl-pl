---
title: Układy z kartami z elementów nadrzędnych.
description: Ten przewodnik zawiera wprowadzenie oraz wyjaśniono, jak utworzyć interfejs użytkownika z kartami w aplikacji platformy Xamarin.Android przy użyciu interfejsów API elementów nadrzędnych.
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 3e96ce2064391d585943f4d79453f8b4f8c6f583
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="tabbed-layouts-with-the-actionbar"></a>Układy z kartami z elementów nadrzędnych.

_Ten przewodnik zawiera wprowadzenie oraz wyjaśniono, jak utworzyć interfejs użytkownika z kartami w aplikacji platformy Xamarin.Android przy użyciu interfejsów API elementów nadrzędnych._


## <a name="overview"></a>Omówienie

Na pasku akcji to wzorzec Android interfejsu użytkownika, który zapewnia spójny interfejs użytkownika dla najważniejsze funkcje takie jak karty, tożsamość aplikacji, menu i wyszukiwania. 3.0 dla systemu Android (interfejs API na poziomie 11) Google wprowadzony interfejsów API elementów nadrzędnych na platformy Android. Interfejsy API elementów nadrzędnych wprowadzenie motywów interfejsu użytkownika, aby zapewnić spójny wygląd i zachowanie i klasy, która pozwala na interfejsy użytkownika z kartami. W tym przewodniku opisano sposób Dodawanie kart na pasku akcji do aplikacji platformy Xamarin.Android. Również zawiera omówienie sposobu korzystania z wersji biblioteki obsługi systemu Android 7 na znaki tabulacji elementów nadrzędnych Poprawka usterki systemu do aplikacji platformy Xamarin.Android przeznaczonych dla systemu Android 2.1 Android 2.3. 

Należy pamiętać, że `Toolbar` to składnik paska nowszej i bardziej ogólną akcję należy użyć zamiast z `ActionBar` (`Toolbar` zaprojektowano tak, aby zastąpić `ActionBar`). Aby uzyskać więcej informacji, zobacz [narzędzi](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="requirements"></a>Wymagania

Dowolnej aplikacji platformy Xamarin.Android, która dotyczy poziom interfejsu API 11 (Android 3.0) lub nowszym ma dostęp do interfejsów API elementów nadrzędnych jako część macierzystych interfejsów API systemu Android. 

Niektóre z interfejsów API elementów nadrzędnych wstecz systemie poziom interfejsu API 7 (Android 2.1) i są dostępne za pośrednictwem [w wersji 7 AppCompat biblioteki](http://developer.android.com/tools/support-library/features.html#v7-appcompat), który ma zostać udostępnione aplikacji platformy Xamarin.Android przy użyciu [Xamarin Android Obsługa biblioteki — w wersji 7 ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) pakietu.



## <a name="introducing-tabs-in-the-actionbar"></a>Wprowadzenie do karty w elementów nadrzędnych.

Na pasku akcji próbuje wyświetlania wszystkich jego kart jednocześnie i jednakowe wszystkich kartach rozmiaru na szerokość najszerszego etykieta karty. Jest to zilustrowane Poniższy zrzut ekranu: 

![Zrzut ekranu z elementów nadrzędnych z wszystkich kart o wielkości równej pokazano](with-action-bar-images/image1.png)

Gdy elementów nadrzędnych. nie można wyświetlić wszystkich kartach, skonfiguruje kartach w widoku przewijanej w poziomie. Użytkownik może szybko przesuń w lewo lub w prawo, aby wyświetlić pozostałe karty. Ten zrzut ekranu ze sklepu Google Play przedstawia przykład: 

![Zrzut ekranu kart w poziomie przewijanego widoku](with-action-bar-images/image2.png)

Każda karta na pasku akcji powinna być skojarzona z [ *fragmentu*](~/android/platform/fragments/index.md). Po wybraniu karty aplikacja wyświetli fragmentu, która jest skojarzona z kartą. Elementów nadrzędnych nie odpowiada za wyświetlanie odpowiedni fragment do użytkownika. Zamiast tego elementów nadrzędnych powiadomi o zmianach stanu na karcie za pośrednictwem klasy, która implementuje interfejs ActionBar.ITabListener aplikacji. Ten interfejs zawiera trzy metody wywołania zwrotnego, które Android wywoła po zmianie stanu karty: 

-  **OnTabSelected** — ta metoda jest wywoływana, gdy użytkownik wybierze karcie. Fragment powinna zostać wyświetlona.

-  **OnTabReselected** — ta metoda jest wywoływana, gdy karcie jest zaznaczona, ale jest ponownie wybrane przez użytkownika. To wywołanie zwrotne jest zwykle używane do odświeżania lub zaktualizowania wyświetlanych fragmentu.

-  **OnTabUnselected** — ta metoda jest wywoływana, gdy użytkownik wybierze innej karty. To wywołanie zwrotne służy do zapisania stanu w fragmencie wyświetlanych przed znika.

Opakowuje Xamarin.Android `ActionBar.ITabListener` ze zdarzeniami w `ActionBar.Tab` klasy. Aplikacje mogą przypisać procedury obsługi zdarzeń do przynajmniej jednej z tych zdarzeń. Istnieją trzy zdarzenia (po jednej dla każdej metody w `ActionBar.ITabListener`), który zostanie podniesiony kartę pasek akcji: 

-  TabSelected
-  TabReselected
-  TabUnselected



### <a name="adding-tabs-to-the-actionbar"></a>Dodawanie kart do elementów nadrzędnych.

Elementów nadrzędnych natywnej do 3.0 dla systemu Android (interfejs API na poziomie 11) lub nowszym i jest dostępny do aplikacji platformy Xamarin.Android przeznaczonego dla tego interfejsu API co najmniej. 

Poniższe kroki przedstawiono sposób dodawania elementów nadrzędnych karty na działanie systemu Android: 

1. W `OnCreate` metody działania &ndash; *przed inicjowanie wszystkie elementy widget interfejsu użytkownika* &ndash; aplikacja musi ustawić `NavigationMode` na `ActionBar` do `ActionBar.NavigationModeTabs` opisane w tym kodu fragment kodu:

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. Tworzenie nowego przy użyciu karty `ActionBar.NewTab()`.

3. Przypisz programów obsługi zdarzeń lub podać niestandardowy `ActionBar.ITabListener` wdrożenia, który będzie odpowiadał na zdarzenia, które są wywoływane, gdy użytkownik wchodzi w interakcję z kartami elementów nadrzędnych.

4. Dodaj kartę, który został utworzony w poprzednim kroku, aby `ActionBar`.


Następujący kod jest przykładem Dodawanie kart do aplikacji, która używa obsługi zdarzeń do reagowania na zmiany stanu przy użyciu następujące kroki: 

```csharp
protected override void OnCreate(Bundle bundle)
{
    ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
    SetContentView(Resource.Layout.Main);

    ActionBar.Tab tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab1_text));
    tab.SetIcon(Resource.Drawable.tab1_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);

    tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab2_text));
    tab.SetIcon(Resource.Drawable.tab2_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);
}
```


#### <a name="event-handlers-vs-actionbaritablistener"></a>Vs programów obsługi zdarzeń ActionBar.ITabListener

Aplikacje powinny używać programów obsługi zdarzeń i `ActionBar.ITabListener` dla różnych scenariuszy. Programy obsługi zdarzeń oferują pewne składni wygody; one Zapisz z konieczności utworzenia klasy i implementować `ActionBar.ITabListener`. Ten wygody pochodzić przy koszcie &ndash; Xamarin.Android wykonuje przekształcenie to dla Ciebie, jedna klasa do tworzenia i wdrażania `ActionBar.ITabListener` dla Ciebie. Jest to poprawnie, gdy aplikacja ma ograniczoną liczbę kart. 

Podczas pracy nad wielu kartach lub udostępniania często używane funkcje między kartami elementów nadrzędnych, może być skuteczniejsza pod względem pamięci i wydajność, aby utworzyć niestandardowe klasy, która implementuje `ActionBar.ITabListener`i udostępniania jednego wystąpienia klasy. Zmniejszy to liczbę GREF w używanej aplikacji platformy Xamarin.Android. 



### <a name="backwards-compatibility-for-older-devices"></a>Zapewnienia zgodności dla starszych urządzeń

[Biblioteki obsługi systemu Android w wersji 7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) wstecz porty karty elementów nadrzędnych 2.1 systemu Android (interfejs API na poziomie 7). Karty są dostępne w aplikacji platformy Xamarin.Android, gdy ten składnik został dodany do projektu.

Aby użyć elementów nadrzędnych, działanie musi podklasy `ActionBarActivity` i Użyj motywu AppCompat, jak pokazano w poniższy fragment kodu:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

Działanie może uzyskać odwołania do jego elementów nadrzędnych z `ActionBarActivity.SupportingActionBar` właściwości. Poniższy fragment kodu przedstawiono przykład konfigurowania elementów nadrzędnych w działaniu:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity : ActionBarActivity, ActionBar.ITabListener
{
    static readonly string Tag = "ActionBarTabsSupport";

    public void OnTabReselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Optionally refresh/update the displayed tab.
        Log.Debug(Tag, "The tab {0} was re-selected.", tab.Text);
    }

    public void OnTabSelected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Display the fragment the user should see
        Log.Debug(Tag, "The tab {0} has been selected.", tab.Text);
    }

    public void OnTabUnselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Save any state in the displayed fragment.
        Log.Debug(Tag, "The tab {0} as been unselected.", tab.Text);
    }

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SupportActionBar.NavigationMode = ActionBar.NavigationModeTabs;
        SetContentView(Resource.Layout.Main);
    }

    void AddTabToActionBar(int labelResourceId, int iconResourceId)
    {
        ActionBar.Tab tab = SupportActionBar.NewTab()
                                            .SetText(labelResourceId)
                                            .SetIcon(iconResourceId)
                                            .SetTabListener(this);
        SupportActionBar.AddTab(tab);
    }
}
```


## <a name="summary"></a>Podsumowanie

W tym przewodniku omówiono sposób tworzenia interfejsu użytkownika z kartami w platformy Xamarin.Android przy użyciu elementów nadrzędnych. Firma Microsoft objętych usługą, jak dodać karty do elementów nadrzędnych i sposobu działania interakcji z kartę zdarzenia za pośrednictwem `ActionBar.ITabListener` interfejsu. Widzieliśmy również, jak biblioteki obsługi systemu Android w wersji 7 AppCompat pakietu backports elementów nadrzędnych karty do starszych wersji systemu android. 


## <a name="related-links"></a>Linki pokrewne

- [ActionBarTabs (przykład)](https://developer.xamarin.com/samples/monodroid/UserInterface/ActionBarTabs/)
- [Pasek narzędzi](~/android/user-interface/controls/tool-bar/index.md)
- [Fragmenty](~/android/platform/fragments/index.md)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](http://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [Wzorzec pasku akcji](http://developer.android.com/design/patterns/actionbar.html)
- [AppCompat systemu android w wersji 7](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Pakiet NuGet AppCompat w wersji 7 biblioteki obsługi platformy Xamarin.Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
