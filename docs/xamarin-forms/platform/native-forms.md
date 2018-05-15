---
title: Formularze natywnego
description: Formularze natywnego Zezwalaj pochodzi wartość platformy Xamarin.Forms ContentPage stron zużywanych przez projektów natywnych Xamarin.iOS, Xamarin.Android i systemu Windows platformy Uniwersalnej. Projektów natywnych może wykorzystać pochodzi wartość ContentPage stron bezpośrednio dodawane do projektu lub z biblioteki .NET Standard biblioteki standardowej .NET lub projektu udostępnionego. W tym artykule opisano, jak korzystać z uzyskanych wartość ContentPage stron bezpośrednio dodawane do projektów natywnych i jak przechodzić między nimi.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: bb7aa9a7071f9ac7bef0dce5790a3fe74302cfb4
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="native-forms"></a>Formularze natywnego

_Formularze natywnego Zezwalaj pochodzi wartość platformy Xamarin.Forms ContentPage stron zużywanych przez projektów natywnych Xamarin.iOS, Xamarin.Android i systemu Windows platformy Uniwersalnej. Projektów natywnych może wykorzystać pochodzi wartość ContentPage stron bezpośrednio dodawane do projektu lub z biblioteki .NET Standard biblioteki standardowej .NET lub projektu udostępnionego. W tym artykule opisano, jak korzystać z uzyskanych wartość ContentPage stron bezpośrednio dodawane do projektów natywnych i jak przechodzić między nimi._

Zazwyczaj aplikacji platformy Xamarin.Forms zawiera jedną lub więcej stron, które pochodzą z [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), a te strony są współużytkowane przez wszystkie platformy .NET Standard projektu biblioteki lub projektu udostępnionego. Jednak umożliwia natywnego formularzy `ContentPage`-pochodnych stron, które mają zostać dodane bezpośrednio do natywnych aplikacji platformy Xamarin.iOS, Xamarin.Android i platformy uniwersalnej systemu Windows. W porównaniu do o natywnego projektu korzystać `ContentPage`-pochodnej stron z projektu biblioteki .NET Standard lub projektu udostępnionego, zaletą Dodawanie stron bezpośrednio do projektów natywnych jest rozszerzone stron z widokami macierzystego. Widoki natywnego następnie może mieć nazwę w języku XAML z `x:Name` i do którego istnieje odwołanie z kodem. Aby uzyskać więcej informacji o widokach macierzystego, zobacz [natywnego widoków](~/xamarin-forms/platform/native-views/index.md).

Proces służący do konsumowania platformy Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnej strony natywnego projektu jest następujący:

1. Dodaj pakiet NuGet platformy Xamarin.Forms do natywnego projektu.
1. Dodaj [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodzących strony oraz wszelkie zależności do natywnego projektu.
1. Wywołanie `Forms.Init` metody.
1. Skonstruować wystąpienia [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych strony i przekonwertować go do odpowiedniego typu natywnego przy użyciu jednej z następujących metod rozszerzenia: `CreateViewController` dla systemu iOS, `CreateFragment` lub `CreateSupportFragment` dla systemu Android, lub `CreateFrameworkElement` dla platformy uniwersalnej systemu Windows.
1. Przejdź do typu macierzystego reprezentację [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych strony przy użyciu nawigacji natywnego interfejsu API.

Platformy Xamarin.Forms musi zostać zainicjowany przez wywołanie metody `Forms.Init` metoda przed natywnego projektu można konstruować [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych strony. Wybieranie, kiedy w tym celu głównie zależy od daty dogodnym w Twojej aplikacji przepływu — może być wykonana podczas uruchamiania aplikacji lub po prostu przed `ContentPage`-strony pochodny jest tworzony. W tym artykule i towarzyszące przykładowe aplikacje `Forms.Init` metoda jest wywoływana podczas uruchamiania aplikacji.

> [!NOTE]
> **NativeForms** przykładowe rozwiązanie aplikacji nie zawiera żadnych projektów platformy Xamarin.Forms. Zamiast tego składa się z projektem platformy Xamarin.iOS projektu platformy Xamarin.Android i projektu platformy uniwersalnej systemu Windows. Każdy projekt jest natywnego projektu, który używa natywnego formularzy użycie [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych stron. Jednak nie ma powodu, dlaczego nie można korzystać z projektów natywnych `ContentPage`-pochodną stron .NET Standard projektu biblioteki lub projektu udostępnionego.

Podczas korzystania z natywnej formularzy, platformy Xamarin.Forms funkcje, takie jak [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/), [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)oraz aparat wiązania danych, wszystkie nadal wykonywane.

## <a name="ios"></a>iOS

W systemach iOS `FinishedLaunching` zastąpienia w `AppDelegate` klasy zwykle jest używana do wykonywania aplikacji uruchamiania zadań związanych z. Jest ona wywoływana po aplikacja została uruchomiona i jest zwykle zastąpiona w celu skonfigurowania głównego okna i wyświetlić kontroler. Poniższy kod przedstawia przykład `AppDelegate` klasy w przykładowej aplikacji:

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static AppDelegate Instance;

    UIWindow _window;
    UINavigationController _navigation;

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        Forms.Init();

        Instance = this;
        _window = new UIWindow(UIScreen.MainScreen.Bounds);

        UINavigationBar.Appearance.SetTitleTextAttributes(new UITextAttributes
        {
            TextColor = UIColor.Black
        });

        var mainPage = new PhonewordPage().CreateViewController();
        mainPage.Title = "Phoneword";

        _navigation = new UINavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    ...
}
```

`FinishedLaunching` Metoda wykonuje następujące zadania:

- Platformy Xamarin.Forms jest inicjowany przez wywołanie `Forms.Init` metody.
- Odwołanie do `AppDelegate` klasy są przechowywane w `static` `Instance` pola. Pozwala to mechanizm umożliwiający innych klas do wywołania metody zdefiniowane w `AppDelegate` klasy.
- `UIWindow`, Która jest kontener główny dla widoków w aplikacji natywnej systemu iOS, jest tworzony.
- `PhonewordPage` Klasy, która jest platformy Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych strony zdefiniowany w języku XAML, jest konstruowane i przekonwertować `UIViewController` przy użyciu `CreateViewController` — metoda rozszerzenia.
- `Title` Właściwość `UIViewController` ma wartość, która będzie wyświetlana na `UINavigationBar`.
- A `UINavigationController` jest tworzona dla zarządzania hierarchiczna nawigacji. `UINavigationController` Klasa zarządza stosie, kontrolerów widoku i `UIViewController` przekazany do konstruktora zobaczy początkowo po `UINavigationController` jest załadowany.
- `UINavigationController` Wystąpienia jest ustawiony jako najwyższego poziomu `UIViewController` dla `UIWindow`i `UIWindow` jest ustawiony jako okno klucza dla aplikacji i stają się widoczne.

Raz `FinishedLaunching` metody zostało wykonane, interfejs użytkownika zdefiniowane w platformy Xamarin.Forms `PhonewordPage` będzie wyświetlana klasy, jak pokazano na poniższym zrzucie ekranu:

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png#lightbox "PhonewordPage systemu iOS")

Interakcja z interfejsu użytkownika, na przykład, naciskając pozycję na [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), spowoduje obsługi zdarzeń w `PhonewordPage` wykonywania związane z kodem. Na przykład, gdy użytkownik naciska **Historia wywołań** przycisku, następujące programu obsługi zdarzeń jest wykonywana:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToCallHistoryPage();
}
```

`static` `AppDelegate.Instance` Pole umożliwia `AppDelegate.NavigateToCallHistoryPage` metody do wywołania, które przedstawiono w poniższym przykładzie:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateViewController();
    callHistoryPage.Title = "Call History";
    _navigation.PushViewController(callHistoryPage, true);
}
```

`NavigateToCallHistoryPage` Metoda konwertuje platformy Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych stronę, aby `UIViewController` z `CreateViewController` — metoda rozszerzenia i zestawy `Title` właściwość `UIViewController`. `UIViewController` Następnie spoczywa na `UINavigationController` przez `PushViewController` metody. W związku z tym interfejsu użytkownika zdefiniowane w platformy Xamarin.Forms `CallHistoryPage` będzie wyświetlana klasy, jak pokazano na poniższym zrzucie ekranu:

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png#lightbox "CallHistoryPage systemu iOS")

Podczas `CallHistoryPage` jest wyświetlany, naciskając tylnej strzałkę będzie pop `UIViewController` dla `CallHistoryPage` klasę z `UINavigationController`, zwracanie użytkownikowi `UIViewController` dla `PhonewordPage` klasy.

## <a name="android"></a>Android

W systemie Android `OnCreate` zastąpienia w `MainActivity` klasy zwykle jest używana do wykonywania aplikacji uruchamiania zadań związanych z. Poniższy kod przedstawia przykład `MainActivity` klasy w przykładowej aplikacji:

```csharp
public class MainActivity : AppCompatActivity
{
    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Phoneword";

        var mainPage = new PhonewordPage().CreateFragment(this);
        FragmentManager
            .BeginTransaction()
            .Replace(Resource.Id.fragment_frame_layout, mainPage)
            .Commit();
        ...
    }
    ...
}
```

`OnCreate` Metoda wykonuje następujące zadania:

- Platformy Xamarin.Forms jest inicjowany przez wywołanie `Forms.Init` metody.
- Odwołanie do `MainActivity` klasy są przechowywane w `static` `Instance` pola. Pozwala to mechanizm umożliwiający innych klas do wywołania metody zdefiniowane w `MainActivity` klasy.
- `Activity` Zawartości znajduje się z zasobem układu. W przykładowej aplikacji, układ składa się z `LinearLayout` zawierający `Toolbar`, a `FrameLayout` do działania jako kontener fragmentu.
- `Toolbar` Jest pobrać i ustawić jako pasek akcji dla `Activity`, i ustawiono akcję tytułu paska.
- `PhonewordPage` Klasy, która jest platformy Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych strony zdefiniowany w języku XAML, jest konstruowane i przekonwertować `Fragment` przy użyciu `CreateFragment` — metoda rozszerzenia.
- `FragmentManager` Klasy tworzy i zatwierdza transakcji, który zastępuje `FrameLayout` wystąpienia z `Fragment` dla `PhonewordPage` klasy.

Aby uzyskać więcej informacji na temat fragmentów, zobacz [fragmenty](~/android/platform/fragments/index.md).

> [!NOTE]
> Oprócz `CreateFragment` metody rozszerzenia platformy Xamarin.Forms obejmuje również `CreateSupportFragment` metody. `CreateFragment` Metoda tworzy `Android.App.Fragment` które może być używany w aplikacjach, które odnoszą się do interfejsu API 11 i większa. `CreateSupportFragment` Metoda tworzy `Android.Support.V4.App.Fragment` które mogą być używane w aplikacjach, które odnoszą się do wersji interfejsu API przed 11.

Raz `OnCreate` metody zostało wykonane, interfejs użytkownika zdefiniowane w platformy Xamarin.Forms `PhonewordPage` będzie wyświetlana klasy, jak pokazano na poniższym zrzucie ekranu:

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png#lightbox "PhonewordPage systemu Android")

Interakcja z interfejsu użytkownika, na przykład, naciskając pozycję na [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), spowoduje obsługi zdarzeń w `PhonewordPage` wykonywania związane z kodem. Na przykład, gdy użytkownik naciska **Historia wywołań** przycisku, następujące programu obsługi zdarzeń jest wykonywana:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainActivity.Instance` Pole umożliwia `MainActivity.NavigateToCallHistoryPage` metody do wywołania, które przedstawiono w poniższym przykładzie:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateFragment(this);
    FragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, callHistoryPage)
        .Commit();
}
```

`NavigateToCallHistoryPage` Metoda konwertuje platformy Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych stronę, aby `Fragment` z `CreateFragment` — metoda rozszerzenia i dodaje `Fragment` fragment z powrotem stosu. W związku z tym interfejsu użytkownika zdefiniowane w platformy Xamarin.Forms `CallHistoryPage` będą wyświetlane, jak pokazano na poniższym zrzucie ekranu:

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png#lightbox "CallHistoryPage systemu Android")

Podczas `CallHistoryPage` jest wyświetlany, naciskając tylnej strzałkę będzie pop `Fragment` dla `CallHistoryPage` w stosie przechodzenia wstecz fragmentu zwracanie użytkownikowi `Fragment` dla `PhonewordPage` klasy.

### <a name="enabling-back-navigation-support"></a>Włączanie obsługi nawigacji Wstecz

`FragmentManager` Klasa ma `BackStackChanged` Zdarzenie uruchamiane przy każdej zmianie zawartości stosie przechodzenia wstecz fragmentu. `OnCreate` Metoda `MainActivity` klasa zawiera program obsługi zdarzeń anonimowego dla tego zdarzenia:

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

Ten program obsługi zdarzeń wyświetla przycisku Wstecz na pasku akcji, pod warunkiem, że istnieje co najmniej jeden `Fragment` wystąpień na fragment kopii stosu. Odpowiedź na naciśnięcie przycisku Wstecz jest obsługiwany przez `OnOptionsItemSelected` zastąpienia:

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && FragmentManager.BackStackEntryCount > 0)
    {
        FragmentManager.PopBackStack();
        return true;
    }
    return base.OnOptionsItemSelected(item);
}
```

`OnOptionsItemSelected` Zastąpienia jest wywoływana, gdy zaznaczono element menu opcji. Ta implementacja POP bieżącego fragmentu z fragmentu stosie przechodzenia wstecz, pod warunkiem, że wybrano przycisku Wstecz i co najmniej jedna `Fragment` wystąpień na fragment kopii stosu.

### <a name="multiple-activities"></a>Wiele działań

Gdy aplikacja składa się z wielu działań [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-stron pochodnej można ją osadzić w każdej z tych czynności. W tym scenariuszu `Forms.Init` metody muszą być wywoływany tylko w `OnCreate` zastąpienia pierwszego `Activity` który osadza platformy Xamarin.Forms `ContentPage`. Jednak ma wpływ następujące:

- Wartość `Xamarin.Forms.Color.Accent` zostaną pobrane z `Activity` wymagane `Forms.Init` metody.
- Wartość `Xamarin.Forms.Application.Current` zostaną skojarzone z `Activity` wymagane `Forms.Init` metody.

### <a name="choosing-a-file"></a>Wybieranie pliku

Osadź [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych strona używa [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) który wymaga obsługi języka HTML "Wybierz plik" przycisku, `Activity` , należy zastąpić `OnActivityResult` Metoda:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>Platforma UWP

Na platformy uniwersalnej systemu Windows, natywnego `App` klasy zwykle jest używana do wykonywania aplikacji uruchamiania zadań związanych z. Platformy Xamarin.Forms jest zazwyczaj zainicjowany, w aplikacji platformy Xamarin.Forms platformy uniwersalnej systemu Windows w `OnLaunched` zastąpienia w natywnym `App` klasy do przekazania `LaunchActivatedEventArgs` argument `Forms.Init` metody. Z tego powodu natywnych aplikacji platformy uniwersalnej systemu Windows, które wykorzystują platformy Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)— strona pochodnej najłatwiej można wywołać `Forms.Init` metody z `App.OnLaunched` metody.

Domyślnie natywnego `App` klasy powoduje uruchomienie `MainPage` klasy jako pierwszej strony aplikacji. Poniższy kod przedstawia przykład `MainPage` klasy w przykładowej aplikacji:

```csharp
public sealed partial class MainPage : Page
{
    public static MainPage Instance;

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        this.Content = new Phoneword.UWP.Views.PhonewordPage().CreateFrameworkElement();
    }
    ...
}
```

`MainPage` Konstruktor wykonuje następujące zadania:

- Buforowanie jest włączone dla tej strony, tak aby nowy `MainPage` nie jest tworzony, gdy użytkownik przechodzi do strony.
- Odwołanie do `MainPage` klasy są przechowywane w `static` `Instance` pola. Pozwala to mechanizm umożliwiający innych klas do wywołania metody zdefiniowane w `MainPage` klasy.
- `PhonewordPage` Klasy, która jest platformy Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych strony zdefiniowany w języku XAML, jest konstruowane i przekonwertować `FrameworkElement` przy użyciu `CreateFrameworkElement` — metoda rozszerzenia, a następnie ustawić jako zawartość `MainPage` klasy.

Raz `MainPage` Konstruktor zostało wykonane, interfejs użytkownika zdefiniowane w platformy Xamarin.Forms `PhonewordPage` będzie wyświetlana klasy, jak pokazano na poniższym zrzucie ekranu:

[![](native-forms-images/uwp-phonewordpage.png "UWP PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png#lightbox "PhonewordPage platformy uniwersalnej systemu Windows")

Interakcja z interfejsu użytkownika, na przykład, naciskając pozycję na [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), spowoduje obsługi zdarzeń w `PhonewordPage` wykonywania związane z kodem. Na przykład, gdy użytkownik naciska **Historia wywołań** przycisku, następujące programu obsługi zdarzeń jest wykonywana:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    Phoneword.UWP.MainPage.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainPage.Instance` Pole umożliwia `MainPage.NavigateToCallHistoryPage` metody do wywołania, które przedstawiono w poniższym przykładzie:

```csharp
public void NavigateToCallHistoryPage()
{
    this.Frame.Navigate(new CallHistoryPage());
}
```

Nawigacja w platformy uniwersalnej systemu Windows jest zwykle wykonywany z `Frame.Navigate` metodę, która przyjmuje `Page` argumentu. Definiuje platformy Xamarin.Forms `Frame.Navigate` — metoda rozszerzenia, która przyjmuje [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych wystąpienie strony. W związku z tym, kiedy `NavigateToCallHistoryPage` wykonuje metodę interfejsu użytkownika zdefiniowane w platformy Xamarin.Forms `CallHistoryPage` będą wyświetlane, jak pokazano na poniższym zrzucie ekranu:

[![](native-forms-images/uwp-callhistorypage.png "UWP CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png#lightbox "CallHistoryPage platformy uniwersalnej systemu Windows")

Podczas `CallHistoryPage` jest wyświetlany, naciskając tylnej strzałkę będzie pop `FrameworkElement` dla `CallHistoryPage` w stosie przechodzenia wstecz w aplikacji, zwracając użytkownikowi `FrameworkElement` dla `PhonewordPage` klasy.

### <a name="enabling-back-navigation-support"></a>Włączanie obsługi nawigacji Wstecz

Na platformy uniwersalnej systemu Windows aplikacje, należy włączyć nawigacji Wstecz dla wszystkich sprzętu i oprogramowania przyciski Wstecz, na innym urządzeniu rozmiarach. Można to osiągnąć poprzez zarejestrowanie programu obsługi zdarzeń dla `BackRequested` zdarzenie, które mogą być wykonywane w `OnLaunched` metody w natywnym `App` klasy:

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    if (rootFrame == null)
    {
        ...      
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
    }
    ...
}
```

Gdy aplikacja jest uruchamiana, `GetForCurrentView` metoda pobiera `SystemNavigationManager` obiektów skojarzonych z bieżącego widoku, a następnie rejestruje program obsługi zdarzeń dla `BackRequested` zdarzeń. Jeśli to aplikacja pierwszego planu, a w odpowiedzi, wywołuje aplikację tylko odbiera to zdarzenie `OnBackRequested` obsługi zdarzeń:

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
    }
}
```

`OnBackRequested` Wywołań obsługi zdarzeń `GoBack` metoda ramki głównej aplikacji i zestawy `BackRequestedEventArgs.Handled` właściwości `true` do zostać oznaczone jako obsługi zdarzenia. Nie można oznaczyć zdarzenia jako obsługiwane może spowodować systemu wyjściem aplikacji (w systemach z rodziny urządzeń przenośnych) lub ignorowanie zdarzeń (w rodzinie pulpitu urządzenia).

Aplikacja zależy od systemu określonego przycisku Wstecz na telefonie, ale wybiera czy wyświetlać przycisk Wstecz na pasku tytułu na urządzeniach z pulpitu. Jest to osiągane przez ustawienie `AppViewBackButtonVisibility` jedną z właściwości `AppViewBackButtonVisibility` wartości wyliczenia:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated` Program obsługi zdarzeń, które są wykonywane w odpowiedzi na `Navigated` zdarzenie wywołujące, aktualizuje widoczność paska przycisku Wstecz tytułu podczas nawigacji strony. Dzięki temu przycisku Wstecz paska tytułu jest wyświetlany w przypadku stosie przechodzenia wstecz w aplikacji nie jest pusty lub usunięte z paska tytułu, jeśli w stosie przechodzenia wstecz w aplikacji jest pusta.

Aby uzyskać więcej informacji na temat obsługi nawigacji Wstecz na platformy uniwersalnej systemu Windows, temacie [historii nawigacji i do tyłu nawigacji dla aplikacji platformy UWP](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="summary"></a>Podsumowanie

Formularze natywnego Zezwalaj platformy Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-pochodnych stron, które mają być używane przez projektów natywnych Xamarin.iOS, Xamarin.Android i systemu Windows platformy Uniwersalnej. Można korzystać z projektów natywnych `ContentPage`-pochodnych stron, które bezpośrednio dodawane do projektu lub z projektu biblioteki .NET Standard lub projektu udostępnionego. W tym artykule wyjaśniono, jak korzystać z `ContentPage`-pochodnych stron, które są bezpośrednio dodawane do projektów natywnych i jak przechodzić między nimi.


## <a name="related-links"></a>Linki pokrewne

- [NativeForms (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [Widoki natywne](~/xamarin-forms/platform/native-views/index.md)
