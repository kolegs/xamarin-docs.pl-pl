---
title: Zestaw narzędzi Xamarin.Forms w projektach natywnego platformy Xamarin
description: W tym artykule wyjaśniono, jak używać klasy pochodnej ContentPage strony, dodawanych bezpośrednio do natywnych projektów platformy Xamarin i jak przechodzić między nimi.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: 65bb3fa070c082fa6c6c489e326a870a80fb9502
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997526"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Zestaw narzędzi Xamarin.Forms w projektach natywnego platformy Xamarin

_Formularze natywne umożliwiają pochodzące z zestawu narzędzi Xamarin.Forms ContentPage stron, które mają być wykorzystywane przez natywne projekty Xamarin.iOS, Xamarin.Android i Windows platformy Uniwersalnej. Natywnych projektów mogą wykorzystywać pochodne ContentPage stron, które bezpośrednio są dodawane do projektu lub z biblioteki .NET Standard, biblioteki .NET Standard lub projektu udostępnionego. W tym artykule opisano sposób korzystania z klasy pochodnej ContentPage strony, dodawanych bezpośrednio do natywnych projektów oraz przechodzenie między nimi._

Zazwyczaj aplikację platformy Xamarin.Forms zawiera jedną lub więcej stron, które wynikają z [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), te strony są współdzielone przez wszystkie platformy w projekcie biblioteki .NET Standard lub projektu udostępnionego. Jednak zezwala na formularze natywne `ContentPage`-pochodnych stronach mają zostać dodane bezpośrednio do natywnych aplikacji platformy Xamarin.iOS, Xamarin.Android i platformy uniwersalnej systemu Windows. W porównaniu do konieczności natywnego projektu korzystać `ContentPage`-pochodnych stron z projektu biblioteki .NET Standard lub projektu udostępnionego lub zalet Dodawanie stron bezpośrednio do natywnych projektów jest, można rozszerzyć za pomocą natywnego widoków stron. Widoki natywne następnie może mieć nazwę w XAML z `x:Name` i do którego istnieje odwołanie związanym z kodem. Aby uzyskać więcej informacji na temat widoki natywne zobacz [widoki natywne](~/xamarin-forms/platform/native-views/index.md).

Proces, co umożliwia korzystanie z zestawu narzędzi Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych strony w natywnego projektu jest następująca:

1. Dodaj pakiet Xamarin.Forms NuGet do natywnego projektu.
1. Dodaj [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych strony i wszystkimi zależnościami do natywnego projektu.
1. Wywołaj `Forms.Init` metody.
1. Skonstruować wystąpienia [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych strony i przekonwertować go na odpowiedni typ macierzysty przy użyciu jednej z następujących metod rozszerzenia: `CreateViewController` dla systemów iOS, `CreateFragment` lub `CreateSupportFragment` dla systemów Android, lub `CreateFrameworkElement` dla platformy uniwersalnej systemu Windows.
1. Przejdź do reprezentacji typu natywnego [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych strony za pomocą nawigacji natywnego interfejsu API.

Xamarin.Forms musi zostać zainicjowany przez wywołanie metody `Forms.Init` metoda przed natywnego projektu można skonstruować [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych strony. Kiedy w tym celu przede wszystkim wybór zależy od po najwygodniejsza przepływu aplikacji — może być wykonana przy uruchamianiu aplikacji lub po prostu przed `ContentPage`— strona pochodnej jest konstruowany. W tym artykule i towarzyszące przykładowych aplikacji `Forms.Init` metoda jest wywoływana przy uruchamianiu aplikacji.

> [!NOTE]
> **NativeForms** przykładowe rozwiązanie aplikacji nie zawiera żadnych projektów zestawu narzędzi Xamarin.Forms. Zamiast tego składa się z projektem platformy Xamarin.iOS projekcie platformy Xamarin.Android i projektu platformy uniwersalnej systemu Windows. Każdy projekt jest natywnego projektu, który używa formularze natywne korzystanie z [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych stronach. Jednak nie ma powodu Dlaczego nie można używać natywnych projektów `ContentPage`-pochodnych stronach projekt biblioteki .NET Standard lub projektu udostępnionego.

Korzystając z formularze natywne, Xamarin.Forms funkcje, takie jak [ `DependencyService` ](xref:Xamarin.Forms.DependencyService), [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter), a aparat powiązania danych, cała praca nadal.

## <a name="ios"></a>iOS

W systemach iOS `FinishedLaunching` zastąpić w `AppDelegate` klasy zwykle jest używana do wykonywania aplikacji uruchamiania zadań związanych z usługą. Jest ona wywoływana po aplikacja została uruchomiona i zwykle jest zastępowany skonfigurować okno główne i wyświetlić kontrolera. Poniższy kod przedstawia przykład `AppDelegate` klasy w przykładowej aplikacji:

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

- Zestaw narzędzi Xamarin.Forms jest inicjowany przez wywołanie `Forms.Init` metody.
- Odwołanie do `AppDelegate` klasy jest przechowywana w `static` `Instance` pola. Jest to mechanizm dla innych klas do wywołania metody zdefiniowane w `AppDelegate` klasy.
- `UIWindow`, Czyli kontener główny dla widoków w aplikacji natywnych dla systemów iOS, jest tworzony.
- `PhonewordPage` Klasy, która jest rozwiązanie Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych strony zdefiniowane w XAML, jest tworzony i konwertowana na `UIViewController` przy użyciu `CreateViewController` — metoda rozszerzenia.
- `Title` Właściwość `UIViewController` ma wartość, która będzie wyświetlana na `UINavigationBar`.
- Element `UINavigationController` jest tworzona dla zarządzania Nawigacja hierarchiczna. `UINavigationController` Klasa zarządza stos kontrolerów widoku i `UIViewController` przekazany do konstruktora zostaną wyświetlone początkowo po `UINavigationController` jest ładowany.
- `UINavigationController` Wystąpienia jest ustawiony jako najwyższego poziomu `UIViewController` dla `UIWindow`i `UIWindow` jest ustawiony jako klucza oknem aplikacji i jest widoczny.

Gdy `FinishedLaunching` metoda jest wykonywana, sprawdzanie interfejsu użytkownika zdefiniowane w interfejsie Xamarin.Forms `PhonewordPage` klasy pojawi się, jak pokazano na poniższym zrzucie ekranu:

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png#lightbox "PhonewordPage systemu iOS")

Interakcja z interfejsem użytkownika, na przykład, naciskając [ `Button` ](xref:Xamarin.Forms.Button), spowodują obsługi zdarzeń w `PhonewordPage` wykonywania związanym z kodem. Na przykład, gdy użytkownik naciśnie **historii połączeń** przycisku następującą obsługę zdarzeń jest wykonywane:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToCallHistoryPage();
}
```

`static` `AppDelegate.Instance` Pole umożliwia `AppDelegate.NavigateToCallHistoryPage` metoda do wywołania, który jest pokazany w poniższym przykładzie kodu:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateViewController();
    callHistoryPage.Title = "Call History";
    _navigation.PushViewController(callHistoryPage, true);
}
```

`NavigateToCallHistoryPage` Metoda konwertuje Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych strony, aby `UIViewController` z `CreateViewController` — metoda rozszerzenia i zestawy `Title` właściwość `UIViewController`. `UIViewController` Jest następnie wypychane na `UINavigationController` przez `PushViewController` metody. W związku z tym, interfejs użytkownika zdefiniowane w interfejsie Xamarin.Forms `CallHistoryPage` klasy pojawi się, jak pokazano na poniższym zrzucie ekranu:

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png#lightbox "CallHistoryPage systemu iOS")

Gdy `CallHistoryPage` jest wyświetlany, naciskając tylnej strzałki zostanie wyświetlone `UIViewController` dla `CallHistoryPage` klasy z `UINavigationController`, zwracanie użytkownikowi `UIViewController` dla `PhonewordPage` klasy.

## <a name="android"></a>Android

W systemie Android `OnCreate` zastąpić w `MainActivity` klasy zwykle jest używana do wykonywania aplikacji uruchamiania zadań związanych z usługą. Poniższy kod przedstawia przykład `MainActivity` klasy w przykładowej aplikacji:

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

- Zestaw narzędzi Xamarin.Forms jest inicjowany przez wywołanie `Forms.Init` metody.
- Odwołanie do `MainActivity` klasy jest przechowywana w `static` `Instance` pola. Jest to mechanizm dla innych klas do wywołania metody zdefiniowane w `MainActivity` klasy.
- `Activity` Content ma wartość z zasobu układu. W przykładowej aplikacji, układ składa się z `LinearLayout` zawierający `Toolbar`, a `FrameLayout` do działania jako kontener fragmentu.
- `Toolbar` Są pobierane i Ustaw jako pasek akcji dla `Activity`, a tytuł paska akcji jest ustawiona.
- `PhonewordPage` Klasy, która jest rozwiązanie Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych strony zdefiniowane w XAML, jest tworzony i konwertowana na `Fragment` przy użyciu `CreateFragment` — metoda rozszerzenia.
- `FragmentManager` Klasy tworzy i zatwierdzeń transakcji, która zastępuje `FrameLayout` wystąpienia z `Fragment` dla `PhonewordPage` klasy.

Aby uzyskać więcej informacji na temat fragmentów, zobacz [fragmenty](~/android/platform/fragments/index.md).

> [!NOTE]
> Oprócz `CreateFragment` metody rozszerzenia Xamarin.Forms obejmuje również `CreateSupportFragment` metody. `CreateFragment` Metoda tworzy `Android.App.Fragment` , mogą być używane w aplikacjach przeznaczonych dla interfejsu API 11 lub nowszym. `CreateSupportFragment` Metoda tworzy `Android.Support.V4.App.Fragment` które mogą być używane w aplikacjach przeznaczonych dla wersji interfejsu API przed 11.

Gdy `OnCreate` metoda jest wykonywana, sprawdzanie interfejsu użytkownika zdefiniowane w interfejsie Xamarin.Forms `PhonewordPage` klasy pojawi się, jak pokazano na poniższym zrzucie ekranu:

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png#lightbox "PhonewordPage dla systemu Android")

Interakcja z interfejsem użytkownika, na przykład, naciskając [ `Button` ](xref:Xamarin.Forms.Button), spowodują obsługi zdarzeń w `PhonewordPage` wykonywania związanym z kodem. Na przykład, gdy użytkownik naciśnie **historii połączeń** przycisku następującą obsługę zdarzeń jest wykonywane:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainActivity.Instance` Pole umożliwia `MainActivity.NavigateToCallHistoryPage` metoda do wywołania, który jest pokazany w poniższym przykładzie kodu:

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

`NavigateToCallHistoryPage` Metoda konwertuje Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych strony, aby `Fragment` z `CreateFragment` metodę rozszerzenia i dodaje `Fragment` do fragmentu kopię stosu. W związku z tym, interfejs użytkownika zdefiniowane w interfejsie Xamarin.Forms `CallHistoryPage` pojawi się, jak pokazano na poniższym zrzucie ekranu:

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png#lightbox "CallHistoryPage dla systemu Android")

Gdy `CallHistoryPage` jest wyświetlany, naciskając tylnej strzałki zostanie wyświetlone `Fragment` dla `CallHistoryPage` ze stosu wstecz fragmentu, zwracanie użytkownikowi `Fragment` dla `PhonewordPage` klasy.

### <a name="enabling-back-navigation-support"></a>Włączanie obsługi przeglądania do tyłu

`FragmentManager` Klasa ma `BackStackChanged` zdarzenie, dla którego jest uruchamiany w każdym przypadku, gdy zmienia się zawartość stosu wstecz fragmentu. `OnCreate` Method in Class metoda `MainActivity` klasa zawiera program obsługi zdarzeń anonimowego dla tego zdarzenia:

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

Ta procedura obsługi zdarzeń wyświetla przycisku Wstecz na pasku akcji, pod warunkiem, że istnieje co najmniej jeden `Fragment` wystąpień na fragment kopię stosu. Odpowiedzi na naciśnięcie przycisku Wstecz jest obsługiwany przez `OnOptionsItemSelected` zastąpienia:

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

`OnOptionsItemSelected` Zastąpienie jest wywoływane, gdy zaznaczono element menu opcji. Ta implementacja POP bieżącego fragmentu z fragmentu stosu wstecz, pod warunkiem, że został wybrany przycisk Wstecz wiąże się z co najmniej jeden `Fragment` wystąpień na fragment kopię stosu.

### <a name="multiple-activities"></a>Wiele działań

Gdy aplikacja składa się z wielu działań [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych stronach można ją osadzić w każdej z tych czynności. W tym scenariuszu `Forms.Init` metoda musi zostać wywołany tylko w `OnCreate` zastąpienia pierwszego `Activity` która osadza rozwiązanie Xamarin.Forms `ContentPage`. Jednak to ma wpływ następujące:

- Wartość `Xamarin.Forms.Color.Accent` zostaną pobrane z `Activity` wywołaniu `Forms.Init` metody.
- Wartość `Xamarin.Forms.Application.Current` zostaną skojarzone z `Activity` wywołaniu `Forms.Init` metody.

### <a name="choosing-a-file"></a>Wybieranie pliku

Podczas osadzania [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych strona używająca [ `WebView` ](xref:Xamarin.Forms.WebView) , potrzebuje pomocy technicznej HTML "Wybierz plik" przycisk, `Activity` trzeba zastąpić `OnActivityResult` Metoda:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>Platforma UWP

Na platformie UWP, natywnych `App` klasy zwykle jest używana do wykonywania aplikacji uruchamiania zadań związanych z usługą. Xamarin.Forms jest zwykle inicjowana, w aplikacjach platformy UWP Xamarin.Forms w `OnLaunched` zastąpienia w natywnym `App` klasy w celu przekazania `LaunchActivatedEventArgs` argument `Forms.Init` metody. Z tego powodu natywnych aplikacji platformy uniwersalnej systemu Windows korzystających z zestawu narzędzi Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)— strona pochodnej najłatwiej można wywołać `Forms.Init` metody z `App.OnLaunched` metody.

Domyślnie natywnych `App` klasy uruchomień `MainPage` klasy jako pierwszej strony aplikacji. Poniższy kod przedstawia przykład `MainPage` klasy w przykładowej aplikacji:

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

- Buforowanie jest włączone dla tej strony, czemu nową `MainPage` nie jest tworzony, gdy użytkownik przechodzi do strony.
- Odwołanie do `MainPage` klasy jest przechowywana w `static` `Instance` pola. Jest to mechanizm dla innych klas do wywołania metody zdefiniowane w `MainPage` klasy.
- `PhonewordPage` Klasy, która jest rozwiązanie Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych strony zdefiniowane w XAML, jest tworzony i konwertowana na `FrameworkElement` przy użyciu `CreateFrameworkElement` metodę rozszerzenia, a następnie ustaw jako zawartość `MainPage` klasy.

Gdy `MainPage` konstruktor został wykonany, interfejs użytkownika zdefiniowane w interfejsie Xamarin.Forms `PhonewordPage` klasy pojawi się, jak pokazano na poniższym zrzucie ekranu:

[![](native-forms-images/uwp-phonewordpage.png "PhonewordPage platformy uniwersalnej systemu Windows")](native-forms-images/uwp-phonewordpage-large.png#lightbox "PhonewordPage platformy uniwersalnej systemu Windows")

Interakcja z interfejsem użytkownika, na przykład, naciskając [ `Button` ](xref:Xamarin.Forms.Button), spowodują obsługi zdarzeń w `PhonewordPage` wykonywania związanym z kodem. Na przykład, gdy użytkownik naciśnie **historii połączeń** przycisku następującą obsługę zdarzeń jest wykonywane:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    Phoneword.UWP.MainPage.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainPage.Instance` Pole umożliwia `MainPage.NavigateToCallHistoryPage` metoda do wywołania, który jest pokazany w poniższym przykładzie kodu:

```csharp
public void NavigateToCallHistoryPage()
{
    this.Frame.Navigate(new CallHistoryPage());
}
```

Nawigacja w platformy uniwersalnej systemu Windows jest zwykle wykonywany z `Frame.Navigate` metody, która przyjmuje `Page` argumentu. Definiuje zestaw narzędzi Xamarin.Forms `Frame.Navigate` metody rozszerzenia, która przyjmuje [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych wystąpienie strony. W związku z tym, kiedy `NavigateToCallHistoryPage` metoda wykonuje interfejsu użytkownika zdefiniowane w interfejsie Xamarin.Forms `CallHistoryPage` pojawi się, jak pokazano na poniższym zrzucie ekranu:

[![](native-forms-images/uwp-callhistorypage.png "CallHistoryPage platformy uniwersalnej systemu Windows")](native-forms-images/uwp-callhistorypage-large.png#lightbox "CallHistoryPage platformy uniwersalnej systemu Windows")

Gdy `CallHistoryPage` jest wyświetlany, naciskając tylnej strzałki zostanie wyświetlone `FrameworkElement` dla `CallHistoryPage` ze stosu wstecz w aplikacji, zwracanie użytkownikowi `FrameworkElement` dla `PhonewordPage` klasy.

### <a name="enabling-back-navigation-support"></a>Włączanie obsługi przeglądania do tyłu

Na platformie UWP aplikacje, należy włączyć przeglądania do tyłu dla sprzętu i oprogramowania wstecz przycisków, różnych czynników formularzy innego urządzenia. Można to zrobić, rejestrując zdarzenia obsługi dla `BackRequested` zdarzenie, które mogą być wykonywane w `OnLaunched` metody w natywnym `App` klasy:

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

Po uruchomieniu aplikacji `GetForCurrentView` metoda pobiera `SystemNavigationManager` obiekt skojarzony z bieżącym widokiem, a następnie rejestruje zdarzenia obsługi dla `BackRequested` zdarzeń. Aplikacja otrzymuje tylko to zdarzenie jest aplikacją pierwszego planu i w odpowiedzi, wywołuje `OnBackRequested` program obsługi zdarzeń:

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

`OnBackRequested` Wywołań obsługi zdarzeń `GoBack` metody ramki głównej aplikacji i zestawy `BackRequestedEventArgs.Handled` właściwości `true` do oznaczenia zdarzeń jako obsługiwane. Nie można oznaczyć zdarzenia jako obsłużony, może spowodować systemu przechodzenia poza aplikacji (w systemach z rodziny urządzeń przenośnych) lub ignorowanie zdarzeń (na rodzina urządzeń pulpitu).

Aplikacja zależy od wybranego systemu przycisku Wstecz na telefonie, ale pozwala wybrać, czy ma być wyświetlana na przycisku Wstecz na pasku tytułu na komputerach. Jest to osiągane przez ustawienie `AppViewBackButtonVisibility` jedną z właściwości `AppViewBackButtonVisibility` wartości wyliczenia:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated` Obsługi zdarzeń, który jest wykonywany w odpowiedzi na `Navigated` zdarzenie, aktualizuje widoczność tytułu paska przycisku Wstecz, gdy występuje nawigacji na stronie. Dzięki temu przycisku Wstecz w pasku tytułu jest widoczna w przypadku, gdy stos Wstecz w aplikacji nie jest pusta lub usunięte z paska tytułu, jeśli stos Wstecz w aplikacji jest pusty.

Aby uzyskać więcej informacji na temat obsługi przeglądania do tyłu na platformy uniwersalnej systemu Windows, zobacz [historii nawigacji i do tyłu nawigacji dla aplikacji platformy UWP](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="summary"></a>Podsumowanie

Formularze natywne Zezwalaj na platformie Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-pochodnych stron, które mają być używane przez natywnych projektów platformy Xamarin.iOS, Xamarin.Android i uniwersalnej platformy Windows (UWP). Natywnych projektów mogą wykorzystywać `ContentPage`-pochodnych stron, które bezpośrednio są dodawane do projektu albo projekt biblioteki .NET Standard bądź projektu udostępnionego. W tym artykule wyjaśniono, jak korzystać z `ContentPage`-pochodnych strony, dodawanych bezpośrednio do natywnych projektów i jak przechodzić między nimi.


## <a name="related-links"></a>Linki pokrewne

- [NativeForms (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [Widoki natywne](~/xamarin-forms/platform/native-views/index.md)
