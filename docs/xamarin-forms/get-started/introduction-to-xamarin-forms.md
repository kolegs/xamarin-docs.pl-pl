---
title: Wprowadzenie do platformy Xamarin.Forms
description: Obsługujący wiele platform natywnie kopii abstrakcji zestawu narzędzi interfejsu użytkownika, który umożliwia deweloperom tworzenie interfejsów użytkownika, które mogą być współużytkowane przez system Android, iOS i platformy uniwersalnej systemu Windows jest platformy Xamarin.Forms. Interfejsy użytkownika są renderowane przy użyciu kontrolki natywne platformy docelowej, umożliwiając aplikacji platformy Xamarin.Forms zachować odpowiedni wyglądu i działania dotyczące każdej platformy. Ten artykuł zawiera wprowadzenie do platformy Xamarin.Forms oraz sposób rozpocząć pisanie aplikacji z nim.
ms.prod: xamarin
ms.assetid: f619595f-3ee7-439b-a1bc-d13e5106e6e9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: c2e37de65cf7be461543704b67249dfa9833dba8
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/23/2018
---
# <a name="an-introduction-to-xamarinforms"></a>Wprowadzenie do platformy Xamarin.Forms

_Obsługujący wiele platform natywnie kopii abstrakcji zestawu narzędzi interfejsu użytkownika, który umożliwia deweloperom tworzenie interfejsów użytkownika, które mogą być współużytkowane przez Android, iOS, Windows i platformy uniwersalnej systemu Windows jest platformy Xamarin.Forms. Interfejsy użytkownika są renderowane przy użyciu kontrolki natywne platformy docelowej, umożliwiając aplikacji platformy Xamarin.Forms zachować odpowiedni wyglądu i działania dotyczące każdej platformy. Ten artykuł zawiera wprowadzenie do platformy Xamarin.Forms oraz sposób rozpocząć pisanie aplikacji z nim._

<a name="Overview" />

## <a name="overview"></a>Omówienie

Platformy Xamarin.Forms to platforma, która umożliwia deweloperom szybkie tworzenie międzyplatformowego interfejsu użytkownika. Udostępnia abstrakcję własnego interfejsu użytkownika, który będzie renderowany przy użyciu kontrolki natywne dla systemu iOS, Android lub platformy uniwersalnej systemu Windows (UWP). Oznacza to, że aplikacje można udostępniać duża część ich kod interfejsu użytkownika i nadal zachować natywnego wyglądu i działania platformy docelowej.

Umożliwia szybkie tworzenie prototypów aplikacji, które można rozwijać, wraz z upływem czasu złożonych aplikacji platformy Xamarin.Forms. Ponieważ aplikacje platformy Xamarin.Forms natywnych aplikacji, nie ma ograniczenia innych narzędzi, takich jak sandboxing przeglądarki, ograniczone interfejsów API lub pogorszenie wydajności. Aplikacje napisane przy użyciu platformy Xamarin.Forms będą mogli korzystać z dowolnej z interfejsu API lub funkcji podstawowej platformy, takich jak (ale nie wyłącznie) CoreMotion, PassKit i StoreKit w systemie iOS; NFC i usług Google Play w systemie Android; i Kafelki w systemie Windows. Ponadto istnieje możliwość tworzenia aplikacji, które mają elementy interfejsu użytkownika utworzone za pomocą platformy Xamarin.Forms, podczas gdy inne elementy są tworzone przy użyciu natywnych narzędzi interfejsu użytkownika.

Aplikacje platformy Xamarin.Forms jest zaprojektowana w taki sam sposób jak tradycyjne aplikacje i platform. Najbardziej typowym podejściem jest użycie [przenośnych bibliotek](~/cross-platform/app-fundamentals/pcl.md) lub [udostępnionych projektów](~/cross-platform/app-fundamentals/shared-projects.md) house udostępnionego kodu i tworzenie aplikacji określonych platform, które będą korzystać z udostępnionego kodu.

Istnieją dwie metody do tworzenia interfejsów użytkownika w platformy Xamarin.Forms. Pierwszy metoda polega na utworzeniu UI z kodu źródłowego C#. Druga metoda jest użycie *Extensible Application Markup Language* interfejsy deklaratywne język używany do opisania użytkownika (XAML). Aby uzyskać więcej informacji na temat języka XAML, zobacz [podstawy XAML](~/xamarin-forms/xaml/xaml-basics/index.md).

W tym artykule omówiono podstawowe informacje dotyczące struktury platformy Xamarin.Forms i obejmuje następujące tematy:

-  [Badanie aplikacji platformy Xamarin.Forms](#Examining_A_Xamarin.Forms_Application).
-  [Jak używać platformy Xamarin.Forms stron i kontrolek](#Views_and_Layouts).
-  [Jak używać wyświetlana jest lista danych](#Lists_in_Xamarin.Forms).
-  [Jak skonfigurować wiązania z danymi](#Data_Binding).
-  [Jak przechodzić między stronami](#Navigation).
-  [Następne kroki](#Next_Steps).

<a name="Examining_A_Xamarin_Forms_Application" />

### <a name="examining-a-xamarinforms-application"></a>Badanie aplikacji platformy Xamarin.Forms

W programie Visual Studio for Mac i Visual Studio domyślnego szablonu aplikacji platformy Xamarin.Forms tworzy najprostszym platformy Xamarin.Forms rozwiązanie to możliwe, który jest wyświetlany tekst. Po uruchomieniu aplikacji powinien wyglądać podobnie do poniższej zrzuty ekranu:

[![](introduction-to-xamarin-forms-images/image05-sml.png "Domyślna aplikacji platformy Xamarin.Forms")](introduction-to-xamarin-forms-images/image05.png#lightbox "domyślnej aplikacji platformy Xamarin.Forms")

Każdy ekran na zrzutach ekranu odpowiada *strony* w platformy Xamarin.Forms. A [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) reprezentuje *działania* w systemie Android, *kontrolera widoku* w systemie iOS, czy też *strony* w uniwersalnych systemu Windows Platformy Uniwersalnej. Tworzy wystąpienie przykładowe zrzuty ekranu powyżej [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) obiektu i użyty do wyświetlenia [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/).

Aby zmaksymalizować ponowne użycie kodu uruchamiania, aplikacje platformy Xamarin.Forms mają jedną klasę o nazwie `App` odpowiada dla pierwszego wystąpienia [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) który będzie wyświetlany. Przykład `App` klasy widać w poniższym kodzie:

```csharp
public class App : Application
{
  public App ()
  {
    MainPage = new ContentPage {
      Content =  new Label
      {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
      }
    };
  }
}
```

Ten kod tworzy nową [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) obiekt, który spowoduje wyświetlenie pojedynczego [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wyśrodkowany zarówno w pionie i w poziomie na stronie.

<a name="Launching_the_Initial_Xamarin_Forms_Page_on_Each_Platform" />

### <a name="launching-the-initial-xamarinforms-page-on-each-platform"></a>Uruchamianie stronę początkową platformy Xamarin.Forms na każdej platformie

Aby użyć tego [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) wewnątrz aplikacji, każda aplikacja platformy musi zainicjować framework platformy Xamarin.Forms i zapewnić wystąpienie [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) jako uruchamiania. W tym kroku inicjowania różni się w zależności od platformy i została szczegółowo opisana w poniższych sekcjach.

<a name="Launching_in_iOS" />

#### <a name="ios"></a>iOS

Aby uruchomić stronę początkową platformy Xamarin.Forms w systemie iOS, zawiera projekt platformy `AppDelegate` klasy, która dziedziczy `Xamarin.Forms.Platform.iOS.FormsApplicationDelegate` klasy, jak pokazano w poniższym przykładzie:

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
      global::Xamarin.Forms.Forms.Init ();
      LoadApplication (new App ());
      return base.FinishedLaunching (app, options);
    }
}
```

`FinishedLoading` Zastąpienie inicjuje framework platformy Xamarin.Forms przez wywołanie metody `Init` metody. Powoduje to implementacja specyficzne dla systemu iOS platformy Xamarin.Forms do załadowania w aplikacji, zanim kontroler widoku głównego jest ustawiony przez wywołanie `LoadApplication` metody.

<a name="Launching_in_Android" />

#### <a name="android"></a>Android

Aby uruchomić stronę początkową platformy Xamarin.Forms w systemie Android, projekt platformy zawiera kod, który tworzy `Activity` z `MainLauncher` atrybutu z inherting działania z `FormsApplicationActivity` klasy, jak pokazano w poniższym przykładzie:

```csharp
namespace HelloXamarinFormsWorld.Android
{
    [Activity(Label = "HelloXamarinFormsWorld", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication (new App ());
        }
    }
}
```

`OnCreate` Zastąpienie inicjuje framework platformy Xamarin.Forms przez wywołanie metody `Init` metody. Powoduje to implementacja specyficzne dla systemu Android platformy Xamarin.Forms, aby można było załadować aplikacji przed załadowaniem aplikacji platformy Xamarin.Forms.

#### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

W aplikacjach systemu Windows platformy Uniwersalnej `Init` metodę, która inicjuje framework platformy Xamarin.Forms jest wywoływany z `App` klasy:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Powoduje to implementacja specyficzne dla platformy uniwersalnej systemu Windows platformy Xamarin.Forms do załadowania w aplikacji. Strona początkowa platformy Xamarin.Forms jest uruchamiana przez `MainPage` klasy, jak pokazano w poniższym przykładzie:

```csharp
public partial class MainPage
{
    public MainPage()
    {
      this.InitializeComponent();
      this.LoadApplication(new HelloXamarinFormsWorld.App());
    }
}
```

Ładowania aplikacji platformy Xamarin.Forms z `LoadApplication` metody.

<a name="Views_and_Layouts" />

### <a name="views-and-layouts"></a>Widoki i układy

Istnieją cztery grupy formantu używany do tworzenia interfejsu użytkownika aplikacji platformy Xamarin.Forms.

1. **Strony** — stron platformy Xamarin.Forms reprezentują ekranów i platform aplikacji mobilnej. Aby uzyskać więcej informacji na temat stron, zobacz [stron platformy Xamarin.Forms](~/xamarin-forms/user-interface/controls/pages.md).
1. **Układy** — układów platformy Xamarin.Forms są używane do tworzenia widoków w struktury logicznej kontenerów. Aby uzyskać więcej informacji na temat układów, zobacz [układów platformy Xamarin.Forms](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Widoki** — widoki platformy Xamarin.Forms są formanty wyświetlane w interfejsie użytkownika, takie jak etykiety, przycisków i pola tekstowe wpis. Aby uzyskać więcej informacji o widokach, zobacz [widoków platformy Xamarin.Forms](~/xamarin-forms/user-interface/controls/views.md).
1. **Komórki** — platformy Xamarin.Forms komórek są specjalne elementy używany do elementów na liście i opisano, w jaki sposób ma być rysowany każdy element na liście. Aby uzyskać więcej informacji na temat komórek, zobacz [komórek platformy Xamarin.Forms](~/xamarin-forms/user-interface/controls/cells.md).

W czasie wykonywania każdego formantu zostanie zamapowane na jej odpowiednik macierzystego, czyli, co będzie renderowany.

Formanty znajdują się wewnątrz układu. [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) Klasy, która implementuje układu często używane, będzie teraz zbadana.

<a name="StackLayout" />

#### <a name="stacklayout"></a>StackLayout

[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) Upraszcza programowanie wieloplatformowych aplikacji przez automatyczne rozmieszczanie formantów na ekranie, niezależnie od rozmiaru ekranu. Każdego elementu podrzędnego jest rozmieszczanych jeden po drugim, albo w poziomie lub pionie w kolejności zostały one dodane. Ilość miejsca na `StackLayout` będzie zależy sposób [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) i [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) właściwości są ustawione, ale domyślnie `StackLayout` podejmie próbę użycia cały ekran.

Poniższy kod XAML przedstawia przykład użycia [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) ułożyć trzy [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) kontrolki:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample1" Padding="20">
  <StackLayout Spacing="10">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

W poniższym przykładzie kodu pokazano równoważne kodu C#:

```csharp
public class StackLayoutExample : ContentPage
{
    public StackLayoutExample()
    {
        Padding = new Thickness(20);
        var red = new Label
        {
            Text = "Stop", BackgroundColor = Color.Red, FontSize = 20
        };
        var yellow = new Label
        {
            Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20
        };
        var green = new Label
        {
            Text = "Go", BackgroundColor = Color.Green, FontSize = 20
        };

        Content = new StackLayout
        {
            Spacing = 10,
            Children = { red, yellow, green }
        };
    }
}
```

Domyślnie [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) zakłada orientacji pionowej, jak pokazano na poniższych zrzutach ekranu:

[![](introduction-to-xamarin-forms-images/image09-sml.png "Pionowy StackLayout")](introduction-to-xamarin-forms-images/image09.png#lightbox "StackLayout pionowe")

Orientacja [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) można zmienić na orientację poziomą, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample2" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

W poniższym przykładzie kodu pokazano równoważne kodu C#:

```csharp
public class StackLayoutExample: ContentPage
{
    public StackLayoutExample()
    {
        // Code that creates labels removed for clarity
        Content = new StackLayout
        {
            Spacing = 10,
            VerticalOptions = LayoutOptions.End,
            Orientation = StackOrientation.Horizontal,
            HorizontalOptions = LayoutOptions.Start,
            Children = { red, yellow, green }
        };
    }
}
```

Poniższe zrzuty ekranu pokazują Wynikowy układ:

[![](introduction-to-xamarin-forms-images/image10-sml.png "Poziomy StackLayout")](introduction-to-xamarin-forms-images/image10.png#lightbox "StackLayout pozioma")

Rozmiar kontrolki można ustawić za pomocą `HeightRequest` i `WidthRequest` właściwości, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample3" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" WidthRequest="100" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" WidthRequest="100" />
    <Label Text="Go" BackgroundColor="Green" Font="20" WidthRequest="200" />
  </StackLayout>
</ContentPage>
```

W poniższym przykładzie kodu pokazano równoważne kodu C#:

```csharp
var red = new Label
{
    Text = "Stop", BackgroundColor = Color.Red, FontSize = 20, WidthRequest = 100
};
var yellow = new Label
{
    Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20, WidthRequest = 100
};
var green = new Label
{
    Text = "Go", BackgroundColor = Color.Green, FontSize = 20, WidthRequest = 200
};

Content = new StackLayout
{
    Spacing = 10,
    VerticalOptions = LayoutOptions.End,
    Orientation = StackOrientation.Horizontal,
    HorizontalOptions = LayoutOptions.Start,
    Children = { red, yellow, green }
};
```

Poniższe zrzuty ekranu pokazują Wynikowy układ:

[![](introduction-to-xamarin-forms-images/image11-sml.png "Poziomy StackLayout z LayoutOptions")](introduction-to-xamarin-forms-images/image11.png#lightbox "poziome StackLayout z LayoutOptions")

Aby uzyskać więcej informacji na temat [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) , zobacz [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

<a name="Lists_in_Xamarin_Forms" />

## <a name="lists-in-xamarinforms"></a>Listy w platformy Xamarin.Forms

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Kontroli jest odpowiedzialny za wyświetlanie na ekranie — każdego elementu w kolekcji elementów `ListView` będzie znajdować się w jednej komórce. Domyślnie `ListView` użyje wbudowane [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) szablon i renderowania pojedynczy wiersz tekstu.

Poniższy przykładowy kod przedstawia prostą [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) przykład:

```csharp
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = new string []
{
    "Buy pears", "Buy oranges", "Buy mangos", "Buy apples", "Buy bananas"
};
Content = new StackLayout
{
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = { listView }
};
```

Poniższy zrzut ekranu przedstawia powstałe w ten sposób [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/):

 ![](introduction-to-xamarin-forms-images/image13.png "Element ListView")

Aby uzyskać więcej informacji na temat [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) sterowania, zobacz [ListView](~/xamarin-forms/user-interface/listview/index.md).

<a name="Binding_to_a_Custom_Class" />

### <a name="binding-to-a-custom-class"></a>Powiązanie z niestandardowej klasy

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Formant może również wyświetlać niestandardowe obiekty przy użyciu domyślnego [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) szablonu.

Poniższy kod przedstawia przykład `TodoItem` klasy:

```csharp
public class TodoItem
{
    public string Name { get; set; }
    public bool Done { get; set; }
}
```

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Mogą być umieszczane kontroli, jak pokazano w poniższym przykładzie:

```csharp
listView.ItemsSource = new TodoItem [] {
    new TodoItem { Name = "Buy pears" },
    new TodoItem { Name = "Buy oranges", Done=true} ,
    new TodoItem { Name = "Buy mangos" },
    new TodoItem { Name = "Buy apples", Done=true },
    new TodoItem { Name = "Buy bananas", Done=true }
};
```

Można ustawić, które można utworzyć powiązania `TodoItem` właściwość jest wyświetlana przez [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), jak pokazano w poniższym przykładzie:

```csharp
listView.ItemTemplate = new DataTemplate(typeof(TextCell));
listView.ItemTemplate.SetBinding(TextCell.TextProperty, "Name");
```

Spowoduje to utworzenie powiązania, który określa ścieżkę do `TodoItem.Name` właściwości oraz spowoduje poprzednio wyświetlanej zrzut ekranu.

Aby uzyskać więcej informacji na temat powiązanie klasy niestandardowej, zobacz [źródeł danych elementu ListView](~/xamarin-forms/user-interface/listview/data-and-databinding.md).

<a name="Selecting_an_Item_in_a_ListView" />

### <a name="selecting-an-item-in-a-listview"></a>Wybranie elementu w elemencie ListView

Odpowiedzieć użytkownik dotknięcie komórki w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) zdarzenia powinien zostać obsłużony, jak pokazano w poniższym przykładzie:

```csharp
listView.ItemSelected += async (sender, e) => {
    await DisplayAlert("Tapped!", e.SelectedItem + " was tapped.", "OK");
};
```

Gdy jest zawarty w [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) metoda może służyć do Otwórz nową stronę z wbudowanych nawigacji Wstecz. [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) Zdarzeń mogą uzyskiwać dostęp do obiektu, który został skojarzony z komórki za pomocą [ `e.SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem/) właściwość powiązać go z nową stronę i wyświetlenia nowego przy użyciu strony `PushAsync`, jako pokazano w poniższym przykładzie:

```csharp
listView.ItemSelected += async (sender, e) => {
    var todoItem = (TodoItem)e.SelectedItem;
    var todoPage = new TodoItemPage(todoItem); // so the new page shows correct data
    await Navigation.PushAsync(todoPage);
};
```

Każdej z platform implementuje wbudowanych nawigacji Wstecz w jego własnej sposób. Aby uzyskać więcej informacji, zobacz [nawigacji](#Navigation).

Aby uzyskać więcej informacji na temat [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) zaznaczenia, zobacz [interakcyjności ListView](~/xamarin-forms/user-interface/listview/interactivity.md).

<a name="Customizing_the_appearance_of_a_cell" />

### <a name="customizing-the-appearance-of-a-cell"></a>Dostosowywanie wyglądu komórki

Można dostosować wygląd komórek przez podklasy [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) klasy, a ustawienie typu tej klasy do [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) właściwość [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/).

Komórki pokazano na poniższym zrzucie ekranu składa się z jednego [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) i dwa [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) kontrolki:

 ![](introduction-to-xamarin-forms-images/image14.png "Wygląd komórek niestandardowego elementu ListView")

Aby utworzyć ten niestandardowy układ [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) klasy powinny być podklasą klasy, jak pokazano w poniższym przykładzie kodu:

```csharp
class EmployeeCell : ViewCell
{
    public EmployeeCell()
    {
        var image = new Image
        {
            HorizontalOptions = LayoutOptions.Start
        };
        image.SetBinding(Image.SourceProperty, new Binding("ImageUri"));
        image.WidthRequest = image.HeightRequest = 40;

        var nameLayout = CreateNameLayout();
        var viewLayout = new StackLayout()
        {
           Orientation = StackOrientation.Horizontal,
           Children = { image, nameLayout }
        };
        View = viewLayout;
    }

    static StackLayout CreateNameLayout()
    {
        var nameLabel = new Label
        {
            HorizontalOptions= LayoutOptions.FillAndExpand
        };
        nameLabel.SetBinding(Label.TextProperty, "DisplayName");

        var twitterLabel = new Label
        {
           HorizontalOptions = LayoutOptions.FillAndExpand,
           Font = Fonts.Twitter
        };
        twitterLabel.SetBinding(Label.TextProperty, "Twitter");

        var nameLayout = new StackLayout()
        {
           HorizontalOptions = LayoutOptions.StartAndExpand,
           Orientation = StackOrientation.Vertical,
           Children = { nameLabel, twitterLabel }
        };
        return nameLayout;
    }
}
```

Kod wykonuje następujące zadania:

-  Dodaje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) kontroli i wiąże go do `ImageUri` właściwość `Employee` obiektu. Aby uzyskać więcej informacji na temat wiązania danych, zobacz [wiązania z danymi](#Data_Binding).
-  Tworzy [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) w orientacji pionowej do przechowywania dwa [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) kontrolki. `Label` Formantów powiązanych z `DisplayName` i `Twitter` właściwości `Employee` obiektu.
-  Tworzy [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) który będzie hostem istniejące [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) i `StackLayout`. Zorganizuje go przy użyciu orientacji poziomej elementy podrzędne.

Po utworzeniu niestandardowego komórki może być zużyte przez [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kontroli, opakowując go w [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), jak pokazano w poniższym przykładzie:

```csharp
List<Employee> myListOfEmployeeObjects = GetAListOfAllEmployees();
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = myListOfEmployeeObjects;
listView.ItemTemplate = new DataTemplate(typeof(EmployeeCell));
```

Ten kod będzie udostępniać `List` z `Employee` do [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Każda komórka będzie renderowany przy użyciu `EmployeeCell` klasy. `ListView` Przekazywać `Employee` do obiektu `EmployeeCell` jako jego [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/).

Aby uzyskać więcej informacji na temat Dostosowywanie wyglądu komórek, zobacz [wygląd komórek](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="Using_XAML_to_Create_and_Customize_A_List" />

### <a name="using-xaml-to-create-and-customize-a-list"></a>Aby tworzyć i dostosowywać listę przy użyciu kodu XAML

Odpowiednik XAML [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) w poprzedniej sekcji przedstawiono w poniższym przykładzie:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:XamarinFormsXamlSample;assembly=XamarinFormsXamlSample"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="listView" IsVisible="false" ItemsSource="{x:Static local:App.Employees}" ItemSelected="EmployeeListOnItemSelected">
    <ListView.ItemTemplate>
      <DataTemplate>
        <ViewCell>
          <ViewCell.View>
            <StackLayout Orientation="Horizontal">
              <Image Source="{Binding ImageUri}" WidthRequest="40" HeightRequest="40" />
              <StackLayout Orientation="Vertical" HorizontalOptions="StartAndExpand">
                <Label Text="{Binding DisplayName}" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Twitter}" Font="{x:Static constants:Fonts.Twitter}"/>
              </StackLayout>
            </StackLayout>
          </ViewCell.View>
        </ViewCell>
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

Definiuje to XAML [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) zawierający [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Źródło danych `ListView` ustawione za pośrednictwem [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) atrybutu. Układ każdego wiersza w `ItemsSource` jest zdefiniowany w [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) elementu.

<a name="Data_Binding" />

## <a name="data-binding"></a>Powiązanie danych

Powiązanie danych łączy dwa obiekty o nazwie *źródła* i *docelowej*. *Źródła* obiektu zawiera dane. *Docelowej* obiektu będzie korzystać z (i często jest wyświetlany) dane z obiektem źródłowym. Na przykład [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) (*docelowej* obiektu) często powiąże jego [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) właściwość publiczną `string` właściwości w *źródła* obiektu. Na poniższym diagramie przedstawiono związek powiązania:

![](introduction-to-xamarin-forms-images/data-binding.png "Powiązanie danych")

Największą zaletą powiązanie danych jest, że nie masz już martwić się o synchronizowania danych między widokami i źródła danych. Zmiany w *źródła* obiektu są automatycznie przypisany do *docelowej* obiektów wewnętrznych przez platformę, powiązania i zmian w obiekcie docelowym może być opcjonalnie wypychana wstecz do *źródła* obiektu.

Ustanawianie danych powiązania jest procesem dwóch krok:

- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Właściwość *docelowej* obiektu musi być ustawiona na *źródła*.
- Należy ustanowić powiązanie między *docelowej* i *źródła*. W języku XAML, jest to realizowane za pośrednictwem [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) — rozszerzenie znaczników. W języku C#, jest to osiągane przez [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) metody.

Aby uzyskać więcej informacji na temat wiązania danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

### <a name="xaml"></a>XAML

Poniższy kod przedstawia przykład wykonywania powiązanie danych w języku XAML:

```xaml
<Entry Text="{Binding FirstName}" ... />
```

Powiązanie między [ `Entry.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) właściwości i `FirstName` właściwość *źródła* obiektu zostanie nawiązane. Zmiany wprowadzone w `Entry` formant będzie automatycznie propagowane do `employeeToDisplay` obiektu. Podobnie jeśli zmiany zostały wprowadzone do `employeeToDisplay.FirstName` właściwości, aparat wiązania platformy Xamarin.Forms będzie również zaktualizować zawartość `Entry` formantu. Jest to nazywane *Wiązanie dwukierunkowe*. Aby powiązanie dwukierunkowe z pracy, musi implementować klasę modelu `INotifyPropertyChanged` interfejsu.

Mimo że [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) właściwość `EmployeeDetailPage` klasy można ustawić w języku XAML, w tym miejscu jest ustawiona w pliku kodu powiązanego do wystąpienia `Employee` obiektu:

```csharp
public EmployeeDetailPage(Employee employee)
{
    InitializeComponent();
    this.BindingContext = employee;
}
```

Gdy [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) właściwości każdego *docelowej* obiektu można ustawić niezależnie, nie jest to konieczne. `BindingContext` to specjalne właściwości, która jest dziedziczona przez wszystkie elementy podrzędne. W związku z tym, kiedy `BindingContext` na [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) ma ustawioną wartość `Employee` wystąpienia wszystkich elementów podrzędnych `ContentPage` tej samej `BindingContext`i można powiązać właściwości publicznej `Employee`obiektu.

### <a name="c35"></a>C&#35;

Poniższy kod przedstawia przykład wykonywania powiązanie danych w języku C#:

```csharp
public EmployeeDetailPage(Employee employeeToDisplay)
{
    this.BindingContext = employeeToDisplay;
    var firstName = new Entry()
    {
        HorizontalOptions = LayoutOptions.FillAndExpand
    };
    firstName.SetBinding(Entry.TextProperty, "FirstName");
    ...
}
```

[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Konstruktor jest przekazywany wystąpienia `Employee` obiektu i zestawy [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) do obiektu, aby powiązać. [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Formantu zostanie uruchomiony i powiązanie między [ `Entry.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) właściwości i `FirstName` właściwość *źródła* obiektu jest ustawiony. Zmiany wprowadzone w `Entry` formant będzie automatycznie propagowane do `employeeToDisplay` obiektu. Podobnie jeśli zmiany zostały wprowadzone do `employeeToDisplay.FirstName` właściwości, aparat wiązania platformy Xamarin.Forms będzie również zaktualizować zawartość `Entry` formantu. Jest to nazywane *Wiązanie dwukierunkowe*. Aby powiązanie dwukierunkowe z pracy, musi implementować klasę modelu `INotifyPropertyChanged` interfejsu.

`SetBinding` Metoda przyjmuje dwa parametry. Pierwszy parametr określa informacje o typie powiązania. Drugi parametr jest używany do dostarczania informacji dotyczących co chcesz powiązać lub temat wiązania. W większości przypadków drugi parametr jest tylko ciąg zawierający nazwę właściwości [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Następująca składnia jest używane dla wiązania `BindingContext` bezpośrednio:

```csharp
someLabel.SetBinding(Label.TextProperty, new Binding("."));
```

Składni z kropkami informuje platformy Xamarin.Forms do użycia [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) jako źródło danych, zamiast właściwość `BindingContext`. Jest to przydatne podczas `BindingContext` jest typem prostym, takich jak `string` lub `int`.

<a name="INotifyPropertyChanged" />

### <a name="property-change-notification"></a>Powiadomienia o zmianie właściwości

Domyślnie *docelowej* obiektu tylko otrzymuje wartość *źródła* obiektu podczas tworzenia powiązania. Aby zachować UI synchronizowane ze źródłem danych, musi być sposobem powiadomienia *docelowej* obiektu podczas *źródła* obiekt został zmieniony. Ten mechanizm jest zapewniana przez `INotifyPropertyChanged` interfejsu. Implementacja interfejsu zapewni powiadomienia do wszystkich formantów powiązanych z danymi, po zmianie wartości właściwości podstawowej.

Obiekty, które implementują `INotifyPropertyChanged` musi wygenerować `PropertyChanged` zdarzeń po zaktualizowaniu jednej z ich właściwości z nową wartością, jak pokazano w poniższym przykładzie:

```csharp
public class MyObject : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    string _firstName;
    public string FirstName
    {
        get { return _firstName; }
        set
        {
            if (value.Equals(_firstName, StringComparison.Ordinal))
            {
                // Nothing to do - the value hasn't changed;
                return;
            }
            _firstName = value;
            OnPropertyChanged();

        }
    }

    void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        var handler = PropertyChanged;
        if (handler != null)
        {
            handler(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Gdy `MyObject.FirstName` zmiany właściwości `OnPropertyChanged` wywoływana jest metoda, która zostanie podniesiony `PropertyChanged` zdarzeń. Aby uniknąć niepotrzebnych zdarzeń wyzwalania, `PropertyChanged` zdarzenie nie jest wywoływane, jeśli nie powoduje zmiany wartości właściwości.

Należy pamiętać, że w `OnPropertyChanged` metody `propertyName` parametru jest adorned z `CallerMemberName` atrybutu. Gwarantuje to, że jeśli `OnPropertyChanged` metoda jest wywoływana z `null` wartość `CallerMemberName` atrybutu zapewnią nazwę metody, która wywołana `OnPropertyChanged`.

<a name="Navigation" />

## <a name="navigation"></a>Nawigacji

Platformy Xamarin.Forms udostępnia wiele zastosowań nawigacji innej strony, w zależności od [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) wpisz używane. Aby uzyskać [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) wystąpień są obydwoma środowiskami nawigacji:

- [Nawigacja hierarchiczna](#Hierarchical_Navigation)
- [Modalne nawigacji](#Modal_Navigation)

[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) i [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) klasy zapewnić alternatywne nawigacji środowiska. Aby uzyskać więcej informacji, zobacz [nawigacji](~/xamarin-forms/app-fundamentals/navigation/index.md).

<a name="Hierarchical_Navigation" />

### <a name="hierarchical-navigation"></a>Hierarchiczna nawigacji

[ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Klasa udostępnia środowisko hierarchiczna nawigacji, gdy użytkownik jest powinni poruszać się po stronach przodu i do tyłu, zgodnie z potrzebami. Klasa implementuje nawigacji jako ostatni na, wytworzenia stos [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) obiektów.

W hierarchicznej nawigacji [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) klasa jest używana do nawigowania w stosie [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) obiektów. Aby przenieść się z jednej strony, aplikacji przeprowadzi wypychanie nową stronę na stosie nawigacji, gdy stanie się stroną aktywną. Aby wrócić do poprzedniej strony, aplikacja będzie pop bieżącej strony z stos nawigacji, a nowa strona najwyższego poziomu staje się stroną aktywną.

Pierwsza strona dodane stos nawigacji jest określany jako *głównego* strony aplikacji i poniższy przykład kodu pokazuje, jak to zrobić:

```csharp
public App ()
{
    MainPage = new NavigationPage(new EmployeeListPage());
}
```

Aby przejść do `LoginPage`, należy wywołać [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) metoda [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) właściwości bieżącej strony jako wykazały w poniższym przykładzie kodu:

```csharp
await Navigation.PushAsync(new LoginPage());
```

Powoduje to, że nowe `LoginPage` obiekt, aby zostać przeniesiony na stosie nawigacji, gdzie staje się stroną aktywną.

Aktywna strona może zostać zdjęte ze stosu ze stosu nawigacji, naciskając klawisz *ponownie* znajdującego się na urządzeniu, niezależnie od tego czy to fizyczne przycisk na urządzeniu lub na ekranie przycisku. Aby programowo powrócić do poprzedniej strony `LoginPage` wystąpienia należy wywołać [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) metody, jak pokazano w poniższym przykładzie:

```csharp
await Navigation.PopAsync();
```

Aby uzyskać więcej informacji na temat hierarchiczna nawigacji, zobacz [hierarchiczna nawigacji](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Modal_Navigation" />

### <a name="modal-navigation"></a>Modalne nawigacji

Platformy Xamarin.Forms zapewnia obsługę modalne stron. Modalne strony zachęca użytkowników do ukończenia zadania niezależne, który nie może być opuszczeniu do czasu ukończenia zadania lub anulowane.

Modalne strony może być dowolny z [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) typy obsługiwanych przez platformy Xamarin.Forms. Aby wyświetlić stronę modalne aplikacji przeprowadzi wypychanie go na stosie nawigacji, gdy stanie się aktywna strona. Aby powrócić do poprzedniej strony aplikacji będzie pop bieżącej strony z stos nawigacji, a nowa strona najwyższego poziomu staje się stroną aktywną.

Modalne nawigacji metody są udostępniane przez [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) właściwości na dowolnym [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) typów pochodnych. [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Udostępnia również właściwość [ `ModalStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) właściwości, z którego można uzyskać modalne stron w stosie nawigacji. Jednak nie pojęcie wykonywania modalne stosem lub wyświetlanie do strony głównej modalne nawigacji. To dlatego te operacje nie są powszechnie obsługiwane na platformach podstawowej.

> [!NOTE]
> A [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wystąpienie nie jest wymagane do wykonywania Nawigacja strony modalne.

Przejdź w trybie modalnym do `LoginPage` należy wywołać [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) metoda [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) właściwości bieżącej strony, jak w poniższym przykładzie kod wykazały :

```csharp
await Navigation.PushModalAsync(new LoginPage());
```

Powoduje to `LoginPage` wystąpienia wypychana na stosie nawigacji, gdzie staje się stroną aktywną.

Aktywna strona może zostać zdjęte ze stosu ze stosu nawigacji, naciskając klawisz *ponownie* znajdującego się na urządzeniu, niezależnie od tego czy to fizyczne przycisk na urządzeniu lub na ekranie przycisku. Aby programowo powrócić do oryginalnej strony `LoginPage` wystąpienia należy wywołać [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) metody, jak pokazano w poniższym przykładzie:

```csharp
await Navigation.PopModalAsync();
```

Powoduje to `LoginPage` wystąpienia, aby były usuwane z stos nawigacji z nowej strony najwyższego poziomu staje się stroną aktywną.

Aby uzyskać więcej informacji na temat modalne nawigacji, zobacz [modalne stron](~/xamarin-forms/app-fundamentals/navigation/modal.md).

<a name="Next_Steps" />

## <a name="next-steps"></a>Następne kroki

W tym artykule wprowadzające należy włączyć na uruchomienie pisania aplikacji platformy Xamarin.Forms. Sugerowane następne kroki obejmują informacje o następujące funkcje:

- Szablony formantu umożliwiają łatwe motywu i ponowna motywu stron aplikacji w czasie wykonywania. Aby uzyskać więcej informacji, zobacz [szablonów kontrolki](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).
- Szablony danych zapewniają możliwość definiowania prezentację danych na obsługiwanych formantów. Aby uzyskać więcej informacji, zobacz [szablony danych](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).
- Udostępniony kod mogą uzyskiwać dostęp do funkcji natywnego za pośrednictwem [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) klasy. Aby uzyskać więcej informacji, zobacz [podczas uzyskiwania dostępu do funkcji natywnych z DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).
- Platformy Xamarin.Forms zawiera proste usługą obsługi wiadomości do wysyłania i odbierania wiadomości, więc zmniejszenie sprzężenia między klasami. Aby uzyskać więcej informacji, zobacz [Publikuj i Subskrybuj z MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).
- Każdej strony układu i kontroli jest inaczej renderowane na każdej platformie za pomocą `Renderer` klasy, która z kolei tworzy kontrolkę natywnego rozmieszcza ją na ekranie i dodaje zachowanie określona w kodzie udostępnionego. Deweloperzy mogą implementować własne niestandardowe `Renderer` klas, aby dostosować wygląd i/lub zachowanie formantu. Aby uzyskać więcej informacji, zobacz [niestandardowe moduły renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).
- Efekty również umożliwić kontrolki natywne na każdej z platform do dostosowania. Efekty są tworzone w projektach specyficzne dla platformy przez podklasy [ `PlatformEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/) kontroli i są używane przez dołączenie do odpowiedniej kontroli platformy Xamarin.Forms. Aby uzyskać więcej informacji, zobacz [efekty](~/xamarin-forms/app-fundamentals/effects/index.md).

Tworzenie aplikacji mobilnych za pomocą platformy Xamarin.Forms, książki przez Petzold Charlesa jest również dobrym miejscem, aby dowiedzieć się więcej na temat platformy Xamarin.Forms. Aby uzyskać więcej informacji, zobacz [tworzenia aplikacji mobilnych za pomocą platformy Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="summary"></a>Podsumowanie

W tym artykule podać wprowadzenie do platformy Xamarin.Forms oraz sposób rozpocząć pisanie aplikacji z nim. Obsługujący wiele platform natywnie kopii abstrakcji zestawu narzędzi interfejsu użytkownika, który umożliwia deweloperom tworzenie interfejsów użytkownika, które mogą być współużytkowane przez system Android, iOS i platformy uniwersalnej systemu Windows jest platformy Xamarin.Forms. Interfejsy użytkownika są renderowane przy użyciu kontrolki natywne platformy docelowej, umożliwiając aplikacji platformy Xamarin.Forms zachować odpowiedni wyglądu i działania dotyczące każdej platformy.


## <a name="related-links"></a>Linki pokrewne

- [XAML — podstawy](~/xamarin-forms/xaml/xaml-basics/index.md)
- [Dokumentacja kontrolek](~/xamarin-forms/user-interface/controls/index.md)
- [Interfejs użytkownika](~/xamarin-forms/user-interface/index.md)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Wprowadzenie — przykłady](https://developer.xamarin.com/samples/xamarin-forms/GettingStarted/)
- [Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
- [Bezpłatne Learning Self-Guided (klip wideo)](https://university.xamarin.com/self-guided)
- [Witaj, iOS platformy Xamarin.Forms skoroszytu](https://developer.xamarin.com/workbooks/xamarin-forms/getting-started/GettingStartedWithXamarinForms-ios.workbook)
