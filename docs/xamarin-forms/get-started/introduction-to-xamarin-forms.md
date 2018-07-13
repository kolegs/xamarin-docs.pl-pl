---
title: Wprowadzenie do zestawu narzędzi Xamarin.Forms
description: Ten artykuł zawiera wprowadzenie do zestawu narzędzi Xamarin.Forms i jak rozpocząć pisanie aplikacji z nim.
ms.prod: xamarin
ms.assetid: f619595f-3ee7-439b-a1bc-d13e5106e6e9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 95b0744cdd52ac1c3f5d7c62c18139a30400ab04
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38999034"
---
# <a name="an-introduction-to-xamarinforms"></a>Wprowadzenie do zestawu narzędzi Xamarin.Forms

_Xamarin.Forms to międzyplatformowa natywnie kopii abstrakcji zestawu narzędzi interfejsu użytkownika, który umożliwia deweloperom łatwe tworzenie interfejsów użytkownika, które mogą być współużytkowane przez systemów Android, iOS, Windows i platformy uniwersalnej Windows. Interfejsy użytkownika są renderowane przy użyciu natywnych kontrolek platformę docelową, umożliwiając aplikacjach Xamarin.Forms zachować odpowiedni wygląd i działanie dla każdej platformy. Ten artykuł zawiera wprowadzenie do zestawu narzędzi Xamarin.Forms i jak rozpocząć pisanie aplikacji z nim._

<a name="Overview" />

## <a name="overview"></a>Omówienie

Xamarin.Forms to struktura, która umożliwia deweloperom szybkie tworzenie interfejsów użytkownika dla wielu platform. Udostępnia abstrakcję własnego interfejsu użytkownika, który będzie renderowany przy użyciu kontrolki natywne w systemie iOS, Android lub Windows platformy Uniwersalnej. Oznacza to, że aplikacje mogą udostępniać dużą część ich kod interfejsu użytkownika i nadal zachować natywnego wyglądu i działania platformy docelowej.

Zestaw narzędzi Xamarin.Forms umożliwia do szybkiego prototypowania aplikacji, które można rozwijać wraz z upływem czasu złożonych aplikacji. Ponieważ aplikacje Xamarin.Forms natywnych aplikacji, nie muszą oni ograniczenia innych narzędziach programistycznych, takich jak przeglądarki piaskownicy, ograniczone interfejsów API lub niską wydajnością. Aplikacje napisane przy użyciu zestawu narzędzi Xamarin.Forms będą mogli korzystać z żadnego z interfejsów API lub funkcje podstawowej platformy, takich jak (ale nie tylko) CoreMotion PassKit i StoreKit w systemie iOS; NFC i usług Google Play w systemie Android; i kafelków na Windows. Ponadto istnieje możliwość tworzenia aplikacji, które będą mieć części interfejsu użytkownika utworzone za pomocą zestawu narzędzi Xamarin.Forms, podczas gdy inne części są tworzone przy użyciu natywnego zestawu narzędzi interfejsu użytkownika.

Aplikacje Xamarin.Forms jest zaprojektowana w taki sam sposób jak tradycyjne aplikacje dla wielu platform. Najbardziej typowym podejściem jest użycie [bibliotek przenośnych](~/cross-platform/app-fundamentals/pcl.md) lub [projekty udostępnione](~/cross-platform/app-fundamentals/shared-projects.md) house udostępnionego kodu w celu tworzenia aplikacji określonych platform, które będą korzystać z udostępnionego kodu.

Istnieją dwie metody do tworzenia interfejsów użytkownika w interfejsie Xamarin.Forms. Pierwszą techniką jest tworzenie interfejsów użytkownika wyłącznie przy użyciu kodu źródłowego języka C#. Drugi techniką jest użycie *Extensible Application Markup Language* interfejsy deklaratywnym językiem znaczników używany do opisania użytkownika (XAML). Aby uzyskać więcej informacji na temat XAML, zobacz [podstawy XAML](~/xamarin-forms/xaml/xaml-basics/index.md).

W tym artykule omówiono podstawowe informacje dotyczące Framework platformy Xamarin.Forms i obejmuje następujące tematy:

-  [Badanie aplikacji platformy Xamarin.Forms](#Examining_A_Xamarin.Forms_Application).
-  [Jak używać zestawu narzędzi Xamarin.Forms stron i kontrolek](#Views_and_Layouts).
-  [Jak używać wyświetlana lista danych,](#Lists_in_Xamarin.Forms).
-  [Jak skonfigurować powiązania danych](#Data_Binding).
-  [Jak nawigować między stronami](#Navigation).
-  [Następne kroki](#Next_Steps).

<a name="Examining_A_Xamarin_Forms_Application" />

### <a name="examining-a-xamarinforms-application"></a>Badanie aplikacji platformy Xamarin.Forms

W programie Visual Studio dla komputerów Mac i Visual Studio domyślnego szablonu aplikacji platformy Xamarin.Forms, tworzy to najprostsza rozwiązanie Xamarin.Forms jest to możliwe, którego tekst jest wyświetlany użytkownikowi. Uruchomienia aplikacji powinny wyglądać podobnie do poniższych zrzutach ekranu:

[![](introduction-to-xamarin-forms-images/image05-sml.png "Domyślna aplikacja Xamarin.Forms")](introduction-to-xamarin-forms-images/image05.png#lightbox "domyślnej aplikacji platformy Xamarin.Forms")

Odnosi się do każdego ekranu na zrzutach ekranu *strony* w interfejsie Xamarin.Forms. A [ `Page` ](xref:Xamarin.Forms.Page) reprezentuje *działania* w systemie Android, *kontrolera widoku* w systemie iOS, czy też *strony* w Windows Universal Platforma (systemu Windows UWP). Tworzy przykładowe zrzuty ekranu powyżej [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) obiektu i użyty do wyświetlenia [ `Label` ](xref:Xamarin.Forms.Label).

Aby zmaksymalizować ponowne użycie kodu startowego, aplikacje Xamarin.Forms mają jedną klasę o nazwie `App` odpowiada podczas tworzenia wystąpienia pierwszy [ `Page` ](xref:Xamarin.Forms.Page) , zostanie wyświetlony. Przykładem `App` klasy są widoczne w poniższym kodzie:

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

Ten kod tworzy nową [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) obiekt, który będzie wyświetlany pojedynczy [ `Label` ](xref:Xamarin.Forms.Label) wyśrodkowany zarówno w pionie i poziomie na stronie.

<a name="Launching_the_Initial_Xamarin_Forms_Page_on_Each_Platform" />

### <a name="launching-the-initial-xamarinforms-page-on-each-platform"></a>Uruchamianie strona początkowa Xamarin.Forms na każdej platformie

Aby użyć tej funkcji [ `Page` ](xref:Xamarin.Forms.Page) wewnątrz aplikacji, każda aplikacja platformy zainicjować w ramach zestawu narzędzi Xamarin.Forms i zapewnić wystąpienie [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) jako uruchamiania. W tym kroku inicjowania różni się w zależności od platformy i został omówiony w poniższych sekcjach.

<a name="Launching_in_iOS" />

#### <a name="ios"></a>iOS

Aby uruchomić stronę początkową Xamarin.Forms w systemie iOS, zawiera projekt platformy `AppDelegate` klasy dziedziczącej z klasy `Xamarin.Forms.Platform.iOS.FormsApplicationDelegate` klasy, jak pokazano w poniższym przykładzie kodu:

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

`FinishedLoading` Zastąpienie jest inicjowana w ramach zestawu narzędzi Xamarin.Forms, wywołując `Init` metody. Powoduje to, że implementacja specyficzne dla systemu iOS zestawu narzędzi Xamarin.Forms do załadowania w aplikacji, zanim kontroler widoku głównego jest ustawiana przez wywołanie metody `LoadApplication` metody.

<a name="Launching_in_Android" />

#### <a name="android"></a>Android

Aby uruchomić stronę początkową Xamarin.Forms w systemie Android, projekt platformy zawiera kod, który tworzy `Activity` z `MainLauncher` atrybutu z inherting działania z `FormsAppCompatActivity` klasy, jak pokazano w poniższym przykładzie kodu:

```csharp
namespace HelloXamarinFormsWorld.Android
{
    [Activity(Label = "HelloXamarinFormsWorld", Theme = "@style/MainTheme", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : FormsAppCompatActivity
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

`OnCreate` Zastąpienie jest inicjowana w ramach zestawu narzędzi Xamarin.Forms, wywołując `Init` metody. Powoduje to implementacji specyficznych dla systemu Android Xamarin.Forms, aby można było załadować aplikację przed załadowaniem aplikacji platformy Xamarin.Forms.

#### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

W aplikacjach Windows platformy Uniwersalnej `Init` z wywoływana jest metoda, która inicjuje w ramach zestawu narzędzi Xamarin.Forms `App` klasy:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Powoduje to implementacja specyficzne dla platformy uniwersalnej systemu Windows zestawu narzędzi Xamarin.Forms do załadowania w aplikacji. Strona początkowa Xamarin.Forms jest uruchamiany przez `MainPage` klasy, jak pokazano w poniższym przykładzie kodu:

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

Jest załadowana w aplikacji platformy Xamarin.Forms `LoadApplication` metody.

<a name="Views_and_Layouts" />

### <a name="views-and-layouts"></a>Widoki i układy

Istnieją cztery grupy w główną kontrolką użyty do utworzenia interfejsu użytkownika aplikacji platformy Xamarin.Forms.

1. **Strony** — strony zestawu narzędzi Xamarin.Forms reprezentują ekrany aplikacji mobilnych dla wielu platform. Aby uzyskać więcej informacji dotyczących stron, zobacz [strony zestawu narzędzi Xamarin.Forms](~/xamarin-forms/user-interface/controls/pages.md).
1. **Układy** — układy platformy Xamarin.Forms to kontenery, które umożliwiają tworzenie widoków w logicznej struktury. Aby uzyskać więcej informacji na temat układów, zobacz [układy platformy Xamarin.Forms](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Widoki** — Xamarin.Forms widoki są formanty wyświetlane w interfejsie użytkownika, takie jak etykiety, przyciski i pola wprowadzania tekstu. Aby uzyskać więcej informacji o widokach, zobacz [widoków Xamarin.Forms](~/xamarin-forms/user-interface/controls/views.md).
1. **Komórki** — Xamarin.Forms komórek są specjalne elementy używane na potrzeby elementów na liście i opisano, jak ma być rysowany każdy element na liście. Aby uzyskać więcej informacji na temat komórek, zobacz [komórki zestawu narzędzi Xamarin.Forms](~/xamarin-forms/user-interface/controls/cells.md).

W czasie wykonywania każdy formant zostanie zamapowane do równoważnik natywnych to, co będzie renderowana.

Formanty są hostowane wewnątrz układu. [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Klasy, która implementuje najczęściej używanym układem, będą teraz analizowane.

<a name="StackLayout" />

#### <a name="stacklayout"></a>StackLayout

[ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Upraszcza opracowywanie aplikacji dla wielu platform, automatyczne rozmieszczanie kontrolek na ekranie, niezależnie od rozmiaru ekranu. Każdego elementu podrzędnego jest określonym położeniem jeden po drugim, albo poziomo lub pionowo w kolejności ich dodania. Ile miejsca `StackLayout` spowoduje użycie zależy [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) i [ `VerticalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) właściwości są ustawione, ale domyślnie `StackLayout` podejmie próbę użycia cały ekran.

Poniższy kod XAML przedstawia przykład użycia [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) rozmieścić trzy [ `Label` ](xref:Xamarin.Forms.Label) kontrolki:

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

Równoważny kod C# pokazano w poniższym przykładzie kodu:

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

Domyślnie [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) zakłada orientacji pionowej, jak pokazano na poniższych zrzutach ekranu:

[![](introduction-to-xamarin-forms-images/image09-sml.png "Pionowe StackLayout")](introduction-to-xamarin-forms-images/image09.png#lightbox "StackLayout pionowy")

Orientacja [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) można ją zmienić na orientacji poziomej, jak pokazano w poniższym przykładzie kodu XAML:

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

Równoważny kod C# pokazano w poniższym przykładzie kodu:

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

Poniższych zrzutach ekranu przedstawiono wynikowe układu:

[![](introduction-to-xamarin-forms-images/image10-sml.png "Poziomy StackLayout")](introduction-to-xamarin-forms-images/image10.png#lightbox "StackLayout poziome")

Rozmiar kontrolki, można ustawić przy użyciu `HeightRequest` i `WidthRequest` właściwości, jak pokazano w poniższym przykładzie kodu XAML:

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

Równoważny kod C# pokazano w poniższym przykładzie kodu:

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

Poniższych zrzutach ekranu przedstawiono wynikowe układu:

[![](introduction-to-xamarin-forms-images/image11-sml.png "Poziomy StackLayout z LayoutOptions")](introduction-to-xamarin-forms-images/image11.png#lightbox "poziomy StackLayout z LayoutOptions")

Aby uzyskać więcej informacji na temat [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) klasy, zobacz [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

<a name="Lists_in_Xamarin_Forms" />

## <a name="lists-in-xamarinforms"></a>Listy w interfejsie Xamarin.Forms

[ `ListView` ](xref:Xamarin.Forms.ListView) Kontroli jest odpowiedzialny za wyświetlanie kolekcję elementów na ekranie — każdego elementu w `ListView` będzie znajdować się w jednej komórce. Domyślnie `ListView` użyje wbudowane [ `TextCell` ](xref:Xamarin.Forms.TextCell) szablonu i renderowania pojedynczy wiersz tekstu.

Poniższy przykład kodu pokazuje prosty [ `ListView` ](xref:Xamarin.Forms.ListView) przykładu:

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

Poniższy zrzut ekranu przedstawia, wynikowy [ `ListView` ](xref:Xamarin.Forms.ListView):

 ![](introduction-to-xamarin-forms-images/image13.png "ListView")

Aby uzyskać więcej informacji na temat [ `ListView` ](xref:Xamarin.Forms.ListView) sterowania, zobacz [ListView](~/xamarin-forms/user-interface/listview/index.md).

<a name="Binding_to_a_Custom_Class" />

### <a name="binding-to-a-custom-class"></a>Powiązanie z niestandardowej klasy

[ `ListView` ](xref:Xamarin.Forms.ListView) Kontrolki można również wyświetlić niestandardowe obiekty przy użyciu domyślnego [ `TextCell` ](xref:Xamarin.Forms.TextCell) szablonu.

Poniższy kod przedstawia przykład `TodoItem` klasy:

```csharp
public class TodoItem
{
    public string Name { get; set; }
    public bool Done { get; set; }
}
```

[ `ListView` ](xref:Xamarin.Forms.ListView) Mogą zostać wypełnione kontroli, jak pokazano w poniższym przykładzie kodu:

```csharp
listView.ItemsSource = new TodoItem [] {
    new TodoItem { Name = "Buy pears" },
    new TodoItem { Name = "Buy oranges", Done=true} ,
    new TodoItem { Name = "Buy mangos" },
    new TodoItem { Name = "Buy apples", Done=true },
    new TodoItem { Name = "Buy bananas", Done=true }
};
```

Można utworzyć powiązania do zestawu, który `TodoItem` właściwości są wyświetlane według [ `ListView` ](xref:Xamarin.Forms.ListView), jak pokazano w poniższym przykładzie kodu:

```csharp
listView.ItemTemplate = new DataTemplate(typeof(TextCell));
listView.ItemTemplate.SetBinding(TextCell.TextProperty, "Name");
```

Spowoduje to utworzenie powiązania, który określa ścieżkę do `TodoItem.Name` właściwości i spowoduje poprzednio wyświetlanej zrzut ekranu.

Aby uzyskać więcej informacji na temat powiązania niestandardowej klasy, zobacz [źródeł danych ListView](~/xamarin-forms/user-interface/listview/data-and-databinding.md).

<a name="Selecting_an_Item_in_a_ListView" />

### <a name="selecting-an-item-in-a-listview"></a>Zaznaczenie elementu w ListView

Aby odpowiedzieć na użytkownika, dotknięcie komórkę w [ `ListView` ](xref:Xamarin.Forms.ListView), [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) zdarzenia powinien zostać obsłużony, jak pokazano w poniższym przykładzie kodu:

```csharp
listView.ItemSelected += async (sender, e) => {
    await DisplayAlert("Tapped!", e.SelectedItem + " was tapped.", "OK");
};
```

Gdy [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) metoda może służyć do otwierających nową stronę wbudowanych nawigacji Wstecz. [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Zdarzeń mogą uzyskiwać dostęp do obiektu, który został skojarzony z komórki za pomocą [ `e.SelectedItem` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) właściwości powiązać go do nowej strony i wyświetlane nowe przy użyciu strony `PushAsync`, jako pokazano w poniższym przykładzie kodu:

```csharp
listView.ItemSelected += async (sender, e) => {
    var todoItem = (TodoItem)e.SelectedItem;
    var todoPage = new TodoItemPage(todoItem); // so the new page shows correct data
    await Navigation.PushAsync(todoPage);
};
```

Każdej z platform implementuje wbudowanych nawigacji Wstecz w inny sposób. Aby uzyskać więcej informacji, zobacz [nawigacji](#Navigation).

Aby uzyskać więcej informacji na temat [ `ListView` ](xref:Xamarin.Forms.ListView) wyboru, zobacz [interakcyjność ListView](~/xamarin-forms/user-interface/listview/interactivity.md).

<a name="Customizing_the_appearance_of_a_cell" />

### <a name="customizing-the-appearance-of-a-cell"></a>Dostosowywanie wyglądu komórek

Wygląd komórki, które można dostosowywać przez podklasy [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) klasy, a typ tej klasy, aby ustawienia [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) właściwość [ `ListView` ](xref:Xamarin.Forms.ListView).

Komórka pokazano na poniższym zrzucie ekranu składa się z jednego [ `Image` ](xref:Xamarin.Forms.Image) oraz dwóch [ `Label` ](xref:Xamarin.Forms.Label) kontrolki:

 ![](introduction-to-xamarin-forms-images/image14.png "Wygląd komórki ListView niestandardowe")

Aby utworzyć ten układ niestandardowy [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) klasy powinna być podklasą klasy, jak pokazano w poniższym przykładzie kodu:

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

-  Dodaje [ `Image` ](xref:Xamarin.Forms.Image) kontroli i wiąże go do `ImageUri` właściwość `Employee` obiektu. Aby uzyskać więcej informacji na temat tworzenia powiązań danych, zobacz [powiązanie danych](#Data_Binding).
-  Tworzy [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) w przypadku orientacji pionowej, aby pomieścić dwa [ `Label` ](xref:Xamarin.Forms.Label) kontrolki. `Label` Formanty są powiązane z `DisplayName` i `Twitter` właściwości `Employee` obiektu.
-  Tworzy [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) który będzie hostował istniejące [ `Image` ](xref:Xamarin.Forms.Image) i `StackLayout`. Będzie on Rozmieść jego elementy podrzędne przy użyciu orientacji poziomej.

Po utworzeniu niestandardowego komórki mogą być używane przez [ `ListView` ](xref:Xamarin.Forms.ListView) kontroli, opakowując go w [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate), jak pokazano w poniższym przykładzie kodu:

```csharp
List<Employee> myListOfEmployeeObjects = GetAListOfAllEmployees();
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = myListOfEmployeeObjects;
listView.ItemTemplate = new DataTemplate(typeof(EmployeeCell));
```

Ten kod zapewni `List` z `Employee` do [ `ListView` ](xref:Xamarin.Forms.ListView). Każda komórka będzie renderowany przy użyciu `EmployeeCell` klasy. `ListView` Przekazują `Employee` obiekt `EmployeeCell` jako jego [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext).

Aby uzyskać więcej informacji na temat Dostosowywanie wyglądu komórek, zobacz [wygląd komórki](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="Using_XAML_to_Create_and_Customize_A_List" />

### <a name="using-xaml-to-create-and-customize-a-list"></a>Tworzenie i dostosowywanie listy przy użyciu XAML

Odpowiednik XAML [ `ListView` ](xref:Xamarin.Forms.ListView) w poprzedniej sekcji pokazano w poniższym przykładzie kodu:

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

Definiuje to XAML [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) zawierający [ `ListView` ](xref:Xamarin.Forms.ListView). Źródło danych `ListView` jest ustawiony za pomocą [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) atrybutu. Układ każdego wiersza w `ItemsSource` jest zdefiniowana w [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) elementu.

<a name="Data_Binding" />

## <a name="data-binding"></a>Powiązanie danych

Powiązanie danych łączy dwa obiekty, o nazwie *źródła* i *docelowej*. *Źródła* obiekt zawiera dane. *Docelowej* obiektu spowoduje wykorzystanie (i często wyświetlany) danych z obiektu źródłowego. Na przykład [ `Label` ](xref:Xamarin.Forms.Label) (*docelowej* obiektu) będzie najczęściej powiązać jej [ `Text` ](xref:Xamarin.Forms.Label.Text) właściwość publiczną `string` właściwość *źródła* obiektu. Na poniższym diagramie przedstawiono relację powiązania:

![](introduction-to-xamarin-forms-images/data-binding.png "Powiązanie danych")

Główną zaletą powiązanie danych jest, że masz już martwić się o synchronizowanie danych między widokami i źródła danych. Zmiany w *źródła* obiektu są automatycznie przekazywane do *docelowej* obiektu w tle przez platformę, powiązania i zmian w obiekcie docelowym może być opcjonalnie odsunięte do *źródła* obiektu.

Ustanawianie danych powiązania jest procesem dwóch kroków:

- [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Właściwość *docelowej* obiektu musi być równa *źródła*.
- Należy ustanowić powiązanie między *docelowej* i *źródła*. W XAML, jest to osiągane przy użyciu [ `Binding` ](xref:Xamarin.Forms.Xaml.BindingExtension) — rozszerzenie znaczników. W języku C# jest to osiągane przez [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) metody.

Aby uzyskać więcej informacji na temat tworzenia powiązań danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

### <a name="xaml"></a>XAML

Poniższy kod przedstawia przykład wykonywania danych powiązania w XAML:

```xaml
<Entry Text="{Binding FirstName}" ... />
```

Powiązanie między [ `Entry.Text` ](xref:Xamarin.Forms.Entry.Text) właściwości i `FirstName` właściwość *źródła* obiektu zostanie nawiązane. Zmiany wprowadzone w `Entry` formant będzie automatycznie propagowane do `employeeToDisplay` obiektu. Podobnie jeśli zmiany zostały wprowadzone do `employeeToDisplay.FirstName` właściwości aparatu powiązania zestawu narzędzi Xamarin.Forms spowoduje to również zaktualizowanie zawartość `Entry` kontroli. Jest to nazywane *określają powiązanie dwukierunkowe*. Aby powiązanie dwukierunkowe do pracy, musi implementować klasy modelu `INotifyPropertyChanged` interfejsu.

Mimo że [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) właściwość `EmployeeDetailPage` klasy można ustawić w XAML, w tym miejscu jest ustawiona w pliku związanym z kodem do wystąpienia `Employee` obiektu:

```csharp
public EmployeeDetailPage(Employee employee)
{
    InitializeComponent();
    this.BindingContext = employee;
}
```

Gdy [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) właściwości każdego *docelowej* obiektu można ustawić niezależnie, nie jest to konieczne. `BindingContext` to specjalne właściwości, która jest dziedziczona przez elementy podrzędne. W związku z tym, kiedy `BindingContext` na [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) jest ustawiona na `Employee` wystąpienia, wszystkie elementy podrzędne `ContentPage` mają taką samą `BindingContext`i można powiązać z publicznymi właściwościami `Employee`obiektu.

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

[ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Konstruktor jest przekazywany wystąpienie `Employee` obiektu i zestawy [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) do obiektu, aby powiązać. [ `Entry` ](xref:Xamarin.Forms.Entry) Sterowania zostanie uruchomiony i powiązanie między [ `Entry.Text` ](xref:Xamarin.Forms.Entry.Text) właściwości i `FirstName` właściwość *źródła* obiektu jest ustawiony. Zmiany wprowadzone w `Entry` formant będzie automatycznie propagowane do `employeeToDisplay` obiektu. Podobnie jeśli zmiany zostały wprowadzone do `employeeToDisplay.FirstName` właściwości aparatu powiązania zestawu narzędzi Xamarin.Forms spowoduje to również zaktualizowanie zawartość `Entry` kontroli. Jest to nazywane *określają powiązanie dwukierunkowe*. Aby powiązanie dwukierunkowe do pracy, musi implementować klasy modelu `INotifyPropertyChanged` interfejsu.

`SetBinding` Metoda przyjmuje dwa parametry. Pierwszy parametr określa informacje o typie powiązania. Drugi parametr jest używany w celu dostarczenia informacji o czym można powiązać lub jak powiązać. W większości przypadków, drugi parametr jest po prostu ciąg przechowujący nazwę właściwości [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext). Poniższa składnia jest używana do powiązania do `BindingContext` bezpośrednio:

```csharp
someLabel.SetBinding(Label.TextProperty, new Binding("."));
```

Składni z kropkami informuje zestawu narzędzi Xamarin.Forms do użycia [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) jako źródło danych, a nie właściwością `BindingContext`. To jest przydatne, gdy `BindingContext` jest typem prostym, takich jak `string` lub `int`.

<a name="INotifyPropertyChanged" />

### <a name="property-change-notification"></a>Powiadomienie o zmianie właściwości

Domyślnie *docelowej* obiektu tylko otrzymuje wartość *źródła* obiektu podczas tworzenia powiązania. Aby zachować synchronizację ze źródłem danych interfejsu użytkownika, musi istnieć sposób powiadamiania *docelowej* obiektu podczas *źródła* obiektu został zmieniony. Ten mechanizm jest świadczona przez `INotifyPropertyChanged` interfejsu. Implementowanie interfejsu oferuje powiadomienia wszystkie formanty powiązane z danymi po zmianie wartości właściwości podstawowej.

Obiekty, które implementują `INotifyPropertyChanged` trzeba zwiększyć `PropertyChanged` zdarzenie, kiedy jedna z jego właściwości jest aktualizowany z nową wartością, jak pokazano w poniższym przykładzie kodu:

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

Gdy `MyObject.FirstName` zmiany właściwości `OnPropertyChanged` wywoływana jest metoda, która zostanie podniesiony `PropertyChanged` zdarzeń. Aby uniknąć niepotrzebnych zdarzeń wyzwalania, `PropertyChanged` zdarzeń nie jest zgłaszany, jeśli nie powoduje zmiany wartości właściwości.

Należy pamiętać, że w `OnPropertyChanged` metoda `propertyName` parametr jest powiązany z `CallerMemberName` atrybutu. Daje to gwarancję, że jeśli `OnPropertyChanged` metoda jest wywoływana z `null` wartość `CallerMemberName` atrybut zapewni nazwę metody, która wywołała `OnPropertyChanged`.

<a name="Navigation" />

## <a name="navigation"></a>Nawigacja

Zestaw narzędzi Xamarin.Forms oferuje pewną liczbę innej strony nawigacji środowisk, w zależności od [ `Page` ](xref:Xamarin.Forms.Page) wpisz używane. Aby uzyskać [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wystąpienia dostępne są dwa środowiska nawigacji:

- [Nawigacja hierarchiczna](#Hierarchical_Navigation)
- [Modalne nawigacji](#Modal_Navigation)

[ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage), [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) i [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) klasy zapewniają środowiska alternatywnych nawigacji. Aby uzyskać więcej informacji, zobacz [nawigacji](~/xamarin-forms/app-fundamentals/navigation/index.md).

<a name="Hierarchical_Navigation" />

### <a name="hierarchical-navigation"></a>Nawigacja hierarchiczna

[ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Klasa udostępnia środowisko Nawigacja hierarchiczna, gdzie użytkownik jest powinni poruszać się po stronach przodu i do tyłu, zgodnie z potrzebami. Klasa implementuje nawigacji jako ostatni na wejściu, first-out (LIFO) stos [ `Page` ](xref:Xamarin.Forms.Page) obiektów.

W obszarze nawigacji hierarchiczne [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) klasa jest używana do nawigowania w stos [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) obiektów. Aby przenieść się z jednej strony, aplikacja będzie umożliwiać wypychanie powiadomień nową stronę na stosie nawigacji, gdy stanie się stroną aktywną. Aby wrócić do poprzedniej strony, aplikacja wyświetli bieżącą stronę ze stosu nawigacji, a nowa strona najwyższego poziomu staje się stroną aktywną.

Pierwsza strona dodane do stos nawigacji jest określany jako *głównego* strony aplikacji i poniższy przykład kodu pokazuje, jak to zrobić:

```csharp
public App ()
{
    MainPage = new NavigationPage(new EmployeeListPage());
}
```

Aby przejść do `LoginPage`, należy wywołać [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) metody [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) właściwości bieżącej strony, jakie wykazano w poniższym przykładzie kodu:

```csharp
await Navigation.PushAsync(new LoginPage());
```

Powoduje to, że nowe `LoginPage` obiekt ma zostać wypchnięty na stos nawigacji, gdzie staje się stroną aktywną.

Aktywnej strony mogą zostać zdjęte ze stosu w stosie nawigacji, naciskając klawisz *ponownie* znajdujący się na urządzeniu, bez względu na to czy jest to przycisku fizycznego na urządzeniu lub przycisk na ekranie. Aby programowo powrócić do poprzedniej strony `LoginPage` wystąpienie musi zostać wywołany [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) metody, jak pokazano w poniższym przykładzie kodu:

```csharp
await Navigation.PopAsync();
```

Aby uzyskać więcej informacji na temat Nawigacja hierarchiczna zobacz [Nawigacja hierarchiczna](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Modal_Navigation" />

### <a name="modal-navigation"></a>Modalne nawigacji

Zestaw narzędzi Xamarin.Forms zapewnia obsługę strony modalne. Strony modalne zaleca użytkownikom ukończenie niezależna zadanie, które nie mogą być opuszczeniu aż zadanie jest ukończone lub anulowane.

Strony modalne może być dowolny z [ `Page` ](xref:Xamarin.Forms.Page) typy obsługiwane przez zestaw narzędzi Xamarin.Forms. Można wyświetlić strony modalne aplikacji będzie umożliwiać wypychanie powiadomień go na stosie nawigacji, gdy stanie się stroną aktywną. Aby powrócić do poprzedniej strony wyświetli aplikacji bieżącej strony ze stosu nawigacji, a nowa strona najwyższego poziomu staje się stroną aktywną.

Modalne nawigacji metody są udostępniane przez [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) właściwości na dowolnym [ `Page` ](xref:Xamarin.Forms.Page) typów pochodnych. [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Udostępnia również właściwość [ `ModalStack` ](xref:Xamarin.Forms.INavigation.ModalStack) właściwości, z którego można uzyskać strony modalne w stosie nawigacji. Jednakże nie obowiązuje koncepcja wykonywania stosem modalne lub usuwanie do strony głównej w modalnym nawigacji. Jest to spowodowane te operacje nie są powszechnie obsługiwane na platformach bazowego.

> [!NOTE]
> A [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) wystąpienia nie jest wymagane do wykonywania Nawigacja strony modalne.

Przejdź w trybie modalnym do `LoginPage` należy wywołać [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) metody [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) właściwości bieżącej strony, jakie wykazano w poniższym przykładzie kodu :

```csharp
await Navigation.PushModalAsync(new LoginPage());
```

Powoduje to, że `LoginPage` wystąpienia, które ma zostać wypchnięty na stos nawigacji, gdzie staje się stroną aktywną.

Aktywnej strony mogą zostać zdjęte ze stosu w stosie nawigacji, naciskając klawisz *ponownie* znajdujący się na urządzeniu, bez względu na to czy jest to przycisku fizycznego na urządzeniu lub przycisk na ekranie. Aby programowo powrócić do oryginalnej strony `LoginPage` wystąpienie musi zostać wywołany [ `PopModalAsync` ](xref:Xamarin.Forms.INavigation.PopModalAsync) metody, jak pokazano w poniższym przykładzie kodu:

```csharp
await Navigation.PopModalAsync();
```

Powoduje to, że `LoginPage` wystąpienia, aby były usuwane z stos nawigacji przy użyciu nowej strony najwyższego poziomu, staje się stroną aktywną.

Aby uzyskać więcej informacji na temat modalne nawigacji zobacz [strony modalne](~/xamarin-forms/app-fundamentals/navigation/modal.md).

<a name="Next_Steps" />

## <a name="next-steps"></a>Następne kroki

Ten artykuł wprowadzający powinny umożliwiać do rozpoczęcia pisania aplikacji platformy Xamarin.Forms. Proponowane następne kroki zawierają informacje o następujące funkcje:

- Szablony kontrolek umożliwiają łatwe motywu i ponownej kompozycji strony aplikacji w czasie wykonywania. Aby uzyskać więcej informacji, zobacz [szablony kontrolek](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).
- Szablony danych zapewniają możliwość definiowania prezentacji danych na obsługiwanych formantów. Aby uzyskać więcej informacji, zobacz [szablony danych](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).
- Udostępniony kod może uzyskać dostęp natywne funkcje za pośrednictwem [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) klasy. Aby uzyskać więcej informacji, zobacz [uzyskiwania dostępu do funkcji natywnych z DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).
- Zestaw narzędzi Xamarin.Forms obejmuje prostą usługę komunikatów do wysyłania i odbierania wiadomości, w związku z tym zmniejszenie sprzężenia między klasami. Aby uzyskać więcej informacji, zobacz [Publikuj i Subskrybuj przy użyciu MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).
- Każdej strony układu i kontroli jest inaczej renderowane na każdej platformie za pomocą `Renderer` klasę, która z kolei tworzy formant natywnych rozmieszcza go na ekranie i dodaje zachowanie, które określono w udostępnionego kodu. Deweloperzy mogą implementować własne niestandardowe `Renderer` klasy, aby dostosować wygląd lub zachowanie kontrolki. Aby uzyskać więcej informacji, zobacz [niestandardowe programy renderujące](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).
- Efekty również umożliwiać natywne kontrolki na każdej z platform można dostosować. Efekty są tworzone w projektach specyficzne dla platformy przez podklasy [ `PlatformEffect` ](xref:Xamarin.Forms.PlatformEffect`2) kontrolować i są używane przez dołączenie do odpowiedniej kontrolki zestawu narzędzi Xamarin.Forms. Aby uzyskać więcej informacji, zobacz [efekty](~/xamarin-forms/app-fundamentals/effects/index.md).

Alternatywnie tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms, książki, Charles Petzold jest dobrym miejscem, aby dowiedzieć się więcej na temat zestawu narzędzi Xamarin.Forms. Aby uzyskać więcej informacji, zobacz [tworzenia aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="summary"></a>Podsumowanie

W tym artykule podano wprowadzenie do zestawu narzędzi Xamarin.Forms i jak rozpocząć pisanie aplikacji z nim. Xamarin.Forms to międzyplatformowa natywnie kopii abstrakcji zestawu narzędzi interfejsu użytkownika, który umożliwia deweloperom łatwe tworzenie interfejsów użytkownika, które mogą być współużytkowane przez system Android, iOS i platformy uniwersalnej Windows. Interfejsy użytkownika są renderowane przy użyciu natywnych kontrolek platformę docelową, umożliwiając aplikacjach Xamarin.Forms zachować odpowiedni wygląd i działanie dla każdej platformy.


## <a name="related-links"></a>Linki pokrewne

- [XAML — podstawy](~/xamarin-forms/xaml/xaml-basics/index.md)
- [Dokumentacja kontrolek](~/xamarin-forms/user-interface/controls/index.md)
- [Interfejs użytkownika](~/xamarin-forms/user-interface/index.md)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Przykłady umożliwiające rozpoczęcie pracy](https://developer.xamarin.com/samples/xamarin-forms/GettingStarted/)
- [Xamarin.Forms](xref:Xamarin.Forms)
- [Bezpłatnym szkoleniom wykładów (wideo)](https://university.xamarin.com/self-guided)
- [Witaj, Xamarin.Forms dla systemu iOS skoroszytu](https://developer.xamarin.com/workbooks/xamarin-forms/getting-started/GettingStartedWithXamarinForms-ios.workbook)
