---
title: Klasa aplikacji platformy Xamarin.Forms
description: W tym artykule opisano funkcje domyślną klasę aplikacji, który zawiera właściwości, aby ustawić stronę początkową dla aplikacji, i trwałych słownik na potrzeby przechowywania wartości prostego i zachowuje zmiany stanu cyklu życia.
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 6de4380f2ce2d19df4ff912b7c86b75ca9e7821b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38999032"
---
# <a name="xamarinforms-app-class"></a>Klasa aplikacji platformy Xamarin.Forms

`Application` Klasy bazowej oferuje następujące funkcje, które są widoczne w domyślnej projektów `App` podklasy:

* A `MainPage` właściwość, która to lokalizacja ustawić stronę początkową dla aplikacji.
* Trwałe [ `Properties` słownika](#Properties_Dictionary) do przechowywania wartości prostego i zachowuje zmiany stanu cyklu życia.
* Statyczna `Current` właściwość, która zawiera odwołanie do bieżącego obiektu aplikacji.

Dodatkowo uwidacznia [metody cyklu życia](~/xamarin-forms/app-fundamentals/app-lifecycle.md) takich jak `OnStart`, `OnSleep`, i `OnResume` oraz zdarzeń modalne nawigacji.

W zależności od szablonu wybrano, `App` klasy można zdefiniować w jeden z dwóch sposobów:

* **C#**, lub
* **XAML I C#**

Aby utworzyć **aplikacji** przy użyciu XAML, wartość domyślna **aplikacji** klasy muszą zostać zastąpione XAML **aplikacji** klasy i skojarzone związanym z kodem, jak pokazano w poniższym przykładzie kodu:

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

A także ustawienie [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) właściwości związane z kodem również wywołać `InitializeComponent` metodę, aby załadować i przeanalizować skojarzone XAML.

## <a name="mainpage-property"></a>Właściwość MainPage

`MainPage` Właściwość `Application` klasy ustawia strony głównej aplikacji.

Na przykład można utworzyć logikę w swojej `App` klasy można wyświetlić różne strony, w zależności od tego, czy użytkownik jest zalogowany lub nie.

`MainPage` Właściwość powinna być ustawiona `App` konstruktora

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

`Application` Podklasy ma statycznych `Properties` słownika, która może służyć do przechowywania danych, w szczególności do użycia w `OnStart`, `OnSleep`, i `OnResume` metody. To są dostępne z dowolnego miejsca w kodzie zestawu narzędzi Xamarin.Forms przy użyciu `Application.Current.Properties`.

`Properties` Słownika używa `string` klucza i magazyny `object` wartość.

Na przykład można ustawić trwałych `"id"` właściwość w dowolnym miejscu w kodzie (po wybraniu elementu na stronie `OnDisappearing` metodę, lub w `OnSleep` metoda) następująco:

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

W `OnStart` lub `OnResume` metody tej wartości można następnie użyć do odtworzenia środowisko użytkownika w jakiś sposób. `Properties` Magazynów słownika `object`s, więc należy rzutować wartość przed jego użyciem.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

Zawsze sprawdzić obecność klucza przed uzyskaniem dostępu do go w celu uniemożliwienia nieoczekiwanych błędów.

> [!NOTE]
> `Properties` Słownika może wykonywać serializację tylko typy pierwotne magazynu. Próba zapisu innych typów (takich jak `List<string>`) może zakończyć się niepowodzeniem dyskretnie.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>Stan trwały

`Properties` Słownika jest automatycznie zapisywanych na urządzeniu.
Dodany do słownika danych będą dostępne, gdy aplikacja zwraca z tła, lub nawet w przypadku, po ponownym uruchomieniu.

Zestaw narzędzi Xamarin.Forms 1.4 wprowadzono kolejną metodę na `Application` , klasa — `SavePropertiesAsync()` — może być wywoływana pozostać aktywne `Properties` słownika. To, aby możliwe było zapisać właściwości po ważne aktualizacje, a nie ich nie wprowadzenie serializacji się ze względu na awarię lub zamykanych przez system operacyjny o podwyższonym ryzyku.

Można znaleźć odwołania do przy użyciu `Properties` słownika **tworzenia aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms** rozdziały książki [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), i [20 ](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf), a w skojarzonej [przykłady](https://github.com/xamarin/xamarin-forms-book-preview-2).



## <a name="the-application-class"></a>Klasa aplikacji

Kompletna `Application` Implementacja klasy znajdują się poniżej odwołania:

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

Ta klasa jest tworzone w każdym projekcie specyficzne dla platformy i przekazywane do `LoadApplication` metody, czyli gdzie `MainPage` jest ładowany i wyświetlane użytkownikowi.
Kod dla każdej platformy jest wyświetlany w poniższych sekcjach. Najnowsze szablony rozwiązania Xamarin.Forms już zawierają ten kod, wstępnie skonfigurowane dla twojej aplikacji.


### <a name="ios-project"></a>Projekt systemu iOS

IOS `AppDelegate` klasa dziedziczy `FormsApplicationDelegate`. Powinno:

* Wywołaj `LoadApplication` z wystąpieniem `App` klasy.

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

### <a name="android-project"></a>Projekt dla systemu android

Android `MainActivity` dziedziczy `FormsAppCompatActivity`. W `OnCreate` zastąpienia `LoadApplication` metoda jest wywoływana z wystąpieniem `App` klasy.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity : FormsAppCompatActivity
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="universal-windows-project-uwp-for-windows-10"></a>Projekt Universal Windows (UWP) dla systemu Windows 10

Zobacz [projektów Windows instalacji](~/xamarin-forms/platform/windows/installation/index.md) informacje na temat obsługi platformy uniwersalnej systemu Windows w interfejsie Xamarin.Forms.

Strona główna projektu platformy uniwersalnej systemu Windows powinien dziedziczyć `WindowsPage`:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

Konstrukcja codebehind języka C# musi wywołać `LoadApplication` do utworzenia wystąpienia usługi Xamarin.Forms `App`. Należy pamiętać, że dobrze jawnie użyć przestrzeni nazw aplikacji, aby zakwalifikować `App` ponieważ aplikacji platformy uniwersalnej systemu Windows również mają własne `App` klasy niezwiązanych ze sobą do zestawu narzędzi Xamarin.Forms.

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

Należy pamiętać, że `Forms.Init()` musi zostać wywołana **App.xaml.cs** wokół wiersz 63.
