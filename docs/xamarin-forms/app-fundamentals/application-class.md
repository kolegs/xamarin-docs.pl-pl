---
title: Klasa aplikacji
description: "Funkcje domyślną klasę aplikacji, które mogą być C# lub języka XAML"
ms.topic: article
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: c383808d443685c1561113e418aed62467f1d5bd
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="app-class"></a>Klasa aplikacji

`Application` Klasy podstawowej oferuje następujące funkcje, które są widoczne w domyślnej projekty `App` podklasy:

* A `MainPage` właściwość, która jest służące do ustawienia strony początkowej aplikacji.
* Trwałe [ `Properties` słownika](#Properties_Dictionary) do przechowywania wartości prostego zachowuje zmiany stanu cyklu życia.
* Statycznego `Current` właściwość, która zawiera odwołanie do bieżącego obiektu aplikacji.

Jeśli udostępnia również [metody cyklu życia](~/xamarin-forms/app-fundamentals/app-lifecycle.md) takich jak `OnStart`, `OnSleep`, i `OnResume` oraz zdarzenia modalne nawigacji.

W zależności od tego, jaki szablon wybrano, `App` klasy można zdefiniować w jeden z dwóch sposobów:

* **C#**, lub
* **XAML I C#**

Aby utworzyć **aplikacji** przy użyciu języka XAML, domyślnie **aplikacji** klasy muszą zostać zastąpione XAML **aplikacji** klasy i skojarzone związane z kodem, jak pokazano w poniższym przykładzie kodu:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

Poniższy przykład kodu pokazuje skojarzone kodem:

```csharp
public partial class App : Application
{
    public App ()
    {
        InitializeComponent ();
        MainPage = new HomePage ();
    }
    ...
}
```

Oraz ustawienie [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) właściwość CodeBehind musi także wywołać metodę `InitializeComponent` metodę, aby załadować i przeanalizować skojarzone XAML.

## <a name="mainpage-property"></a>Właściwość MainPage

`MainPage` Właściwość `Application` klasy ustawia strony głównej aplikacji.

Na przykład można utworzyć logikę w Twojej `App` klasę, aby wyświetlić stronę różne w zależności od tego, czy użytkownik jest zalogowany lub nie.

`MainPage` Właściwość powinna być ustawiona `App` konstruktora,

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }
}
```

<a name="Properties_Dictionary" />

## <a name="properties-dictionary"></a>Słownik właściwości

`Application` Podklasy ma statycznych `Properties` słownik, który może służyć do przechowywania danych, w szczególności do użycia w `OnStart`, `OnSleep`, i `OnResume` metody. To są dostępne z dowolnego miejsca, w za pomocą kodu platformy Xamarin.Forms `Application.Current.Properties`.

`Properties` Używa słownika `string` klucza i magazyny `object` wartość.

Na przykład można ustawić trwałych `"id"` właściwości w dowolnym miejscu w kodzie (po wybraniu elementu na stronie `OnDisappearing` metody, lub w `OnSleep` metody), takich jak to:

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

W `OnStart` lub `OnResume` metody następnie tej wartości można użyć do odtworzenia środowisko użytkownika w inny sposób. `Properties` Magazynów słownika `object`s, więc należy rzutować jego wartości przed jego użyciem.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

Zawsze sprawdzaj obecności klucza przed uzyskaniem dostępu do go w celu uniemożliwienia nieoczekiwane błędy.

> [!NOTE]
> `Properties` Słownika może serializować tylko typy pierwotne dla magazynu. Próba zapisu innych typów (takich jak `List<string>`) może nie w trybie dyskretnym.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>Trwałość

`Properties` Słownik jest automatycznie zapisywany na urządzeniu.
Dodane do słownika danych będą dostępne po powrocie z aplikacji w tle lub nawet po ponownym uruchomieniu.

Platformy Xamarin.Forms 1.4 wprowadzić dodatkową metodę na `Application` klasy - `SavePropertiesAsync()` — które można wywołać pozostać aktywne `Properties` słownika. To pozwala na zapisywanie właściwości po ważne aktualizacje, a nie ich nie pobierania serializacji się z powodu awarii lub zostanie przerwany przez system operacyjny ryzyka.

Można znaleźć odniesienia do przy użyciu `Properties` słownika **tworzenia aplikacji mobilnych za pomocą platformy Xamarin.Forms** książki rozdziałach [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), i [20 ](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)i w skojarzonym [przykłady](https://github.com/xamarin/xamarin-forms-book-preview-2).



## <a name="the-application-class"></a>Klasa aplikacji

Kompletna `Application` Implementacja klasy są wyświetlane poniżej dla odwołania:

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }

    protected override void OnStart()
    {
        // Handle when your app starts
        Debug.WriteLine ("OnStart");
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Debug.WriteLine ("OnSleep");
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
        Debug.WriteLine ("OnResume");
    }
}

```

Ta klasa jest tworzone w każdym projekcie specyficzne dla platformy i przekazane do `LoadApplication` metodę, która jest, gdy `MainPage` jest załadowane, a wyświetlana użytkownikowi.
W poniższych sekcjach przedstawiono kod dla każdej platformy. Najnowsze szablony rozwiązań platformy Xamarin.Forms zawiera już ten kod, wstępnie skonfigurowane dla aplikacji.


### <a name="ios-project"></a>iOS projektu

IOS `AppDelegate` teraz dziedziczy klasa `FormsApplicationDelegate`. Powinno:

* Wywołanie `LoadApplication` z wystąpieniem `App` klasy.

* Zawsze zwraca `base.FinishedLaunching (app, options);`.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="android-project"></a>Projekt systemu android

Android `MainActivity` teraz dziedziczy `FormsApplicationActivity`. W `OnCreate` zastąpienia `LoadApplication` metoda jest wywoływana przy użyciu wystąpienia `App` klasy.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

> [!NOTE]
> Istnieje nowsza [ `FormsAppCompatActivity` ](~/xamarin-forms/platform/android/appcompat.md) podstawowa klasa, która może służyć do lepszej obsługi projektowania w systemie Android materiału.
> Będzie to domyślny szablon dla systemu Android w przyszłości, ale można wykonać [tych instrukcji](~/xamarin-forms/platform/android/appcompat.md) do zaktualizowania istniejącej aplikacji systemu Android.


### <a name="windows-phone-project"></a>Windows Phone projektu

Strona główna w projekcie (w oparciu o program Silverlight) Windows Phone powinny dziedziczyć `FormsApplicationPage`. Oznacza to, języka XAML i C# dla `MainPage` odwołania `FormsApplicationPage` klasy, jak pokazano.

XAML używa niestandardowej przestrzeni nazw, aby element główny odzwierciedla `FormsApplicationPage` klasy:

```xaml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

C# dziedziczy `FormsApplicationPage` klasy i wywołania `LoadApplication` można utworzyć wystąpienia użytkownika platformy Xamarin.Forms `App`. Należy pamiętać, że dobrze jest jawnie używać nazw aplikacji do kwalifikowania `App` ponieważ aplikacje Windows Phone również mają swoje własne `App` klasy niezwiązanych ze sobą platformy Xamarin.Forms.

```csharp
public partial class MainPage :
    global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3, use the correct namespace
    }
 }
```

### <a name="windows-81-project"></a>Projekt Windows 8.1

Strona główna w [Windows 8.1 (w oparciu o WinRT)](~/xamarin-forms/platform/windows/installation/tablet.md) projekty powinny dziedziczyć `WindowsPage`. Oznacza to, że XAML dla `MainPage` odwołania `WindowsPage` klasy, jak pokazano:

XAML używa niestandardowej przestrzeni nazw, aby element główny odzwierciedla `FormsApplicationPage` klasy:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
   ...>
</forms:WindowsPage>
```

Należy wywołać konstrukcji został C# `LoadApplication` można utworzyć wystąpienia użytkownika platformy Xamarin.Forms `App`. Należy pamiętać, że dobrze jest jawnie używać nazw aplikacji do kwalifikowania `App` ponieważ aplikacji platformy uniwersalnej systemu Windows również mają swoje własne `App` klasy niezwiązanych ze sobą platformy Xamarin.Forms.

```csharp
public partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();
        LoadApplication(new YOUR_APP_NAMESPACE.App());
    }
 }
```

Należy pamiętać, że `Forms.Init()` musi zostać wywołana **App.xaml.cs** około 65 wiersza.

### <a name="universal-windows-project-uwp-for-windows-10"></a>Uniwersalny projekt systemu Windows (UWP) dla systemu Windows 10

[Uniwersalnych projektów systemu Windows](~/xamarin-forms/platform/windows/installation/universal.md) pomocy technicznej w platformy Xamarin.Forms jest obecnie w przeglądzie.

Należy dziedziczyć po stronie głównej w projekcie platformy UWP `WindowsPage`. Oznacza to, języka XAML i C# dla `MainPage` odwołania `FormsApplicationPage` klasy, jak pokazano.

XAML używa niestandardowej przestrzeni nazw, aby element główny odzwierciedla `FormsApplicationPage` klasy:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

Należy wywołać konstrukcji został C# `LoadApplication` można utworzyć wystąpienia użytkownika platformy Xamarin.Forms `App`. Należy pamiętać, że dobrze jest jawnie używać nazw aplikacji do kwalifikowania `App` ponieważ aplikacji platformy uniwersalnej systemu Windows również mają swoje własne `App` klasy niezwiązanych ze sobą platformy Xamarin.Forms.

```csharp
public sealed partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();

        LoadApplication(new YOUR_NAMESPACE.App());
    }
 }
```

Należy pamiętać, że `Forms.Init()` musi zostać wywołana **App.xaml.cs** wokół wiersza 63.
