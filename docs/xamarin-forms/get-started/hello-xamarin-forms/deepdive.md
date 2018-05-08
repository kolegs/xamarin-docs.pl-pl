---
title: Informacje temat dokładnego platformy Xamarin.Forms
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: d97aa580-1eb9-48b3-b15b-0d7421ea7ae
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: ae4f2198e42ab404cabe148108a24ef2219bcf6b
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinforms-deep-dive"></a>Informacje temat dokładnego platformy Xamarin.Forms

W [szybkiego startu platformy Xamarin.Forms](~/xamarin-forms/get-started/hello-xamarin-forms/quickstart.md), Phoneword aplikacja została skompilowana. W tym artykule opisano, co został utworzony w celu uzyskania informacji na temat podstawowe informacje dotyczące sposobu działania aplikacji platformy Xamarin.Forms.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Wprowadzenie do programu Visual Studio

Program Visual Studio jest zaawansowanym IDE firmy Microsoft. Zawiera funkcje pełni zintegrowane wizualnego projektanta, Edytor tekstu, wraz z narzędzia do refaktoryzacji, przeglądarkę zestawu i integracji kodu źródłowego. Ten artykuł skupia się na temat używania niektórych podstawowych funkcji programu Visual Studio z platformą Xamarin wtyczki.

Program Visual Studio umożliwia organizowanie kodu w *rozwiązań* i *projekty*. Rozwiązanie to kontener, który może zawierać jeden lub więcej projektów. Projekt może być aplikacji, biblioteki obsługi aplikacji testu i więcej. Aplikacja Phoneword składa się z jedno rozwiązanie zawierające projekty czterech, jak pokazano na poniższym zrzucie ekranu.

![](deepdive-images/vs/solution.png "Eksplorator rozwiązań programu Visual Studio")

Projekty są:

- Phoneword — ten projekt jest .NET Standard projektu biblioteki, który przechowuje wszystkie udostępnione kodu i udostępnionych interfejsu użytkownika.
- Phoneword.Android — ten projekt zawiera określonego kodu dla systemu Android i jest punkt wejścia dla aplikacji systemu Android.
- Phoneword.iOS — ten projekt zawiera z systemem iOS kod i jest punkt wejścia dla aplikacji systemu iOS.
- Phoneword.UWP — ten projekt zawiera kod systemu Windows platformy Uniwersalnej i jest punkt wejścia dla aplikacji platformy uniwersalnej systemu Windows.

## <a name="anatomy-of-a-xamarinforms-application"></a>Anatomia aplikacji platformy Xamarin.Forms

Poniższy zrzut ekranu przedstawia zawartość Phoneword .NET Standard projektu biblioteki w programie Visual Studio:

![](deepdive-images/vs/net-standard-project.png "Zawartość projektu standardowego .NET Phoneword")

Projekt posiada **zależności** węzła, który zawiera **NuGet** i **SDK** węzłów. **NuGet** węzeł zawiera pakiet NuGet platformy Xamarin.Forms, który został dodany do projektu i **SDK** zawiera węzeł `NETStandard.Library` metapackage, który odwołuje się do pełnego zestawu pakietów NuGet definiującą .NET Standard.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Wprowadzenie do programu Visual Studio dla komputerów Mac

Visual Studio for Mac jest bezpłatne, open source IDE podobne do programu Visual Studio. Zawiera funkcje pełni zintegrowane wizualnego projektanta, Edytor tekstu, wraz z narzędzia do refaktoryzacji, przeglądarkę zestawu i integracji kodu źródłowego. Aby uzyskać więcej informacji na temat programu Visual Studio dla komputerów Mac, zobacz [wprowadzenie do programu Visual Studio for Mac](/visualstudio/mac/).

Visual Studio for Mac następuje rozwiązanie Visual Studio kodu do organizowania *rozwiązań* i *projekty*. Rozwiązanie to kontener, który może zawierać jeden lub więcej projektów. Projekt może być aplikacji, biblioteki obsługi aplikacji testu i więcej. Aplikacja Phoneword składa się z jedno rozwiązanie zawierające projekty trzy, jak pokazano na poniższym zrzucie ekranu.

![](deepdive-images/xs/solution.png "Visual Studio for Mac okienko rozwiązania")

Projekty są:

- Phoneword — ten projekt jest projektu biblioteki (PCL) klas przenośnych, który przechowuje wszystkie udostępnione kodu i udostępnionych interfejsu użytkownika.
- Phoneword.Droid — ten projekt zawiera określonego kodu dla systemu Android i jest punkt wejścia dla aplikacji systemu Android.
- Phoneword.iOS — ten projekt zawiera z systemem iOS kod i jest punkt wejścia dla aplikacji systemu iOS.

## <a name="anatomy-of-a-xamarinforms-application"></a>Anatomia aplikacji platformy Xamarin.Forms

Poniższy zrzut ekranu przedstawia zawartość Phoneword PCL projektu w programie Visual Studio dla komputerów Mac:

![](deepdive-images/xs/pcl-project.png "Zawartość projektu Phoneword PCL")

Projekt składa się z trzech folderów:

- **Odwołania** — zawiera zestawy wymagane, aby skompilować i uruchomić aplikację. Rozwijanie folderu obsługującym podzestaw przenośny .NET wykazuje odwołania do zestawów platformy .NET, takie jak [systemu](http://msdn.microsoft.com/library/system%28v=vs.110%29.aspx), System.Core, i [System.Xml](http://msdn.microsoft.com/library/system.xml%28v=vs.110%29.aspx). Rozszerzanie **z pakietów** folderu ujawnia odwołania do zestawów platformy Xamarin.Forms.
- **Pakiety** — domach katalogu pakietów [NuGet](https://www.nuget.org) pakiety, które ułatwiają korzystanie z bibliotek innych firm w aplikacji. Te pakiety może zostać zaktualizowana do najnowszej wersji, prawym przyciskiem myszy folder, a następnie wybierając opcję aktualizacji w menu podręcznym.
- **Właściwości** — zawiera **AssemblyInfo.cs**, plik metadanych zestawu .NET. Jest dobrym rozwiązaniem, aby wypełnić ten plik podstawowych informacji o aplikacji. Aby uzyskać więcej informacji dotyczących tego pliku, zobacz [klasy AssemblyInfo](http://msdn.microsoft.com/library/microsoft.visualbasic.applicationservices.assemblyinfo(v=vs.110).aspx) w witrynie MSDN.

-----

Projekt zawiera również liczbę plików:

- **App.XAML** — kod znaczników XAML dla `App` klasy, która definiuje słownik zasobów dla aplikacji.
- **App.XAML.cs** — CodeBehind dla `App` klasy, która odpowiada za tworzenie wystąpień pierwszej strony, który będzie wyświetlany przez aplikację na każdej platformie oraz na potrzeby obsługi zdarzeń cyklu życia aplikacji.
- **IDialer.cs** — `IDialer` interfejs, który określa, że `Dial` metoda musi być dostarczona przez wszystkie klasy implementującej.
- **MainPage.xaml** — kod znaczników XAML dla `MainPage` klasy, która definiuje interfejsu użytkownika dla strony wyświetlany po uruchomieniu aplikacji.
- **MainPage.xaml.cs** — CodeBehind dla `MainPage` klasy, która zawiera logiki biznesowej, które jest wykonywane, gdy użytkownik użyje strony.
- **Packages.config** — plik XML (Visual Studio for Mac tylko), który zawiera informacje używane przez projekt, aby śledzić wymagane pakiety i ich wersje pakietów NuGet. Zarówno program Visual Studio dla komputerów Mac, jak i programu Visual Studio można skonfigurować na automatyczne przywracanie wszelkich brakujących pakietów NuGet podczas udostępniania kodu źródłowego z innymi użytkownikami. Zawartość tego pliku jest kontrolowany przez Menedżera pakietów NuGet i nie powinny być edytowane ręcznie.
- **PhoneTranslator.cs** — logiki biznesowej, które jest odpowiedzialny za konwertowanie phone word na numer telefonu, który jest wywoływany z **MainPage.xaml.cs**.

Aby uzyskać więcej informacji na temat Anatomia aplikacji platformy Xamarin.iOS, zobacz [Anatomia aplikacji platformy Xamarin.iOS](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy). Aby uzyskać więcej informacji na temat Anatomia aplikacji platformy Xamarin.Android, zobacz [Anatomia aplikacji platformy Xamarin.Android](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Architektura i podstawowe informacje dotyczące aplikacji

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aplikacji platformy Xamarin.Forms została zaprojektowana w taki sam sposób jak tradycyjnych aplikacji i platform. Udostępniony kod jest zwykle umieszczane w bibliotece .NET Standard, a aplikacje specyficzne dla platformy zużyć udostępnionego kodu. Na poniższym diagramie przedstawiono omówienie tej relacji dla aplikacji Phoneword:

![](deepdive-images/vs/architecture.png "Architektura Phoneword")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aplikacji platformy Xamarin.Forms została zaprojektowana w taki sam sposób jak tradycyjnych aplikacji i platform. Udostępniony kod jest zwykle umieszczane w przenośnych klasy biblioteki PCL (), a specyficzne dla platformy aplikacji korzystać z udostępnionego kodu. Na poniższym diagramie przedstawiono omówienie tej relacji dla aplikacji Phoneword:

![](deepdive-images/xs/architecture.png "Architektura Phoneword")

Aby uzyskać więcej informacji o PCLs, zobacz [wprowadzenie do bibliotek klas przenośnych](~/cross-platform/app-fundamentals/pcl.md).

-----

Aby zmaksymalizować ponowne użycie kodu uruchamiania, aplikacje platformy Xamarin.Forms mają jedną klasę o nazwie `App` jest odpowiedzialny za tworzenie wystąpień pierwszej strony, który będzie wyświetlany przez aplikację na każdej platformie, jak pokazano w poniższym przykładzie:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
namespace Phoneword
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new MainPage();
        }
        ...
    }
}
```

Ten kod ustawia `MainPage` właściwość `App` klasę, aby nowe wystąpienie klasy [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) klasy. Ponadto [ `XamlCompilation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/) atrybutu włącza kompilatora XAML, dzięki czemu XAML jest kompilowany bezpośrednio do język pośredni. Aby uzyskać więcej informacji, zobacz [kompilacji XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Uruchamianie aplikacji na każdej platformie

### <a name="ios"></a>iOS

Aby uruchomić stronę początkową platformy Xamarin.Forms w systemie iOS, zawiera projekt Phoneword.iOS `AppDelegate` klasy, która dziedziczy `FormsApplicationDelegate` klasy, jak pokazano w poniższym przykładzie:

```csharp
namespace Phoneword.iOS
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init ();
            LoadApplication (new App ());
            return base.FinishedLaunching (app, options);
        }
    }
}
```

`FinishedLaunching` Zastąpienie inicjuje framework platformy Xamarin.Forms przez wywołanie metody `Init` metody. Powoduje to implementacja specyficzne dla systemu iOS platformy Xamarin.Forms do załadowania w aplikacji, zanim kontroler widoku głównego jest ustawiony przez wywołanie `LoadApplication` metody.

### <a name="android"></a>Android

Aby uruchomić stronę początkową platformy Xamarin.Forms w systemie Android, Phoneword.Droid projekt zawiera kod, który tworzy `Activity` z `MainLauncher` atrybutu z działaniem dziedziczących `FormsApplicationActivity` klasy, jak pokazano w poniższym przykładzie kodu:

```csharp
namespace Phoneword.Droid
{
    [Activity(Label = "Phoneword",
              Icon = "@drawable/icon",
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        internal static MainActivity Instance { get; private set; }

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            Instance = this;
            global::Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication(new App());
        }
    }
}
```

`OnCreate` Zastąpienie inicjuje framework platformy Xamarin.Forms przez wywołanie metody `Init` metody. Powoduje to implementacja specyficzne dla systemu Android platformy Xamarin.Forms, aby można było załadować aplikacji przed załadowaniem aplikacji platformy Xamarin.Forms. Ponadto `MainActivity` klasy zawiera odwołanie do samej siebie w `Instance` właściwości. `Instance` Właściwości jest znana jako lokalny kontekst i jest wywoływany przez `PhoneDialer` klasy.

## <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

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
namespace Phoneword.UWP
{
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.LoadApplication(new Phoneword.App());
        }
    }
}
```

Ładowania aplikacji platformy Xamarin.Forms z `LoadApplication` metody.

> [!NOTE]
> Uniwersalnych aplikacji platformy systemu Windows (UWP) może być utworzonego za pomocą platformy Xamarin.Forms, ale tylko w systemie Windows za pomocą programu Visual Studio.

## <a name="user-interface"></a>Interfejs użytkownika

Istnieją cztery grupy formantu używany do tworzenia interfejsu użytkownika aplikacji platformy Xamarin.Forms.

1. **Strony** — stron platformy Xamarin.Forms reprezentują ekranów i platform aplikacji mobilnej. Używa aplikacji Phoneword [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) klasy do wyświetlenia na jednym ekranie. Aby uzyskać więcej informacji na temat stron, zobacz [stron platformy Xamarin.Forms](~/xamarin-forms/user-interface/controls/pages.md).
1. **Układy** — układów platformy Xamarin.Forms są używane do tworzenia widoków w struktury logicznej kontenerów. Używa aplikacji Phoneword [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) klasy rozmieszczanie formantów w poziomie stosu. Aby uzyskać więcej informacji na temat układów, zobacz [układów platformy Xamarin.Forms](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Widoki** — widoki platformy Xamarin.Forms są formanty wyświetlane w interfejsie użytkownika, takie jak etykiety, przycisków i pola tekstowe wpis. Używa aplikacji Phoneword [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), i [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) kontrolki. Aby uzyskać więcej informacji o widokach, zobacz [widoków platformy Xamarin.Forms](~/xamarin-forms/user-interface/controls/views.md).
1. **Komórki** — platformy Xamarin.Forms komórek są specjalne elementy używany do elementów na liście i opisano, w jaki sposób ma być rysowany każdy element na liście. Phoneword, który nie powoduje aplikacji używać żadnych komórek. Aby uzyskać więcej informacji na temat komórek, zobacz [komórek platformy Xamarin.Forms](~/xamarin-forms/user-interface/controls/cells.md).

W czasie wykonywania każdego formantu zostanie zamapowane na jej odpowiednik macierzystego, czyli, co będzie renderowany.

Po uruchomieniu aplikacji Phoneword na dowolnej platformie Wyświetla jednym ekranie umożliwiająca [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) w platformy Xamarin.Forms. A `Page` reprezentuje *grupie widoków* w systemie Android *kontrolera widoku* w systemie iOS, czy też *strony* na platformy uniwersalnej systemu Windows. Aplikacja Phoneword również tworzy [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) obiekt, który reprezentuje `MainPage` klasy, których znaczników XAML jest wyświetlana w poniższym przykładzie kodu:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                         xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                         x:Class="Phoneword.MainPage">
        ...
        <StackLayout>
            <Label Text="Enter a Phoneword:" />
            <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
            <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
            <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
</ContentPage>
```

`MainPage` Klasy używa [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) formantu automatyczne rozmieszczanie formantów na ekranie, niezależnie od rozmiaru ekranu. Każdy element podrzędny jest rozmieszczanych jeden po drugim pionie w kolejności, w której są dodawane. `StackLayout` Formant zawiera [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) formantu do wyświetlania tekstu na stronie [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) formantu, aby akceptować dane wejściowe użytkownika tekstową, a dwa [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) formanty używane do wykonywania kodu w odpowiedzi na zdarzenia touch.

Aby uzyskać więcej informacji na temat XAML w platformy Xamarin.Forms, zobacz [podstawy XAML platformy Xamarin.Forms](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="responding-to-user-interaction"></a>Odpowiada na żądania interakcji z użytkownikiem

Obiekt zdefiniowany w języku XAML mogą wyzwalać zdarzenia jest obsługiwany w pliku CodeBehind. Poniższy kod przedstawia przykład `OnTranslate` w CodeBehind dla metody `MainPage` klasy, która jest wykonywana w odpowiedzi na [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) zdarzenie wywołujące na *Przetłumacz* przycisk.

```csharp
void OnTranslate(object sender, EventArgs e)
{
    translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
    if (!string.IsNullOrWhiteSpace (translatedNumber)) {
        callButton.IsEnabled = true;
        callButton.Text = "Call " + translatedNumber;
    } else {
        callButton.IsEnabled = false;
        callButton.Text = "Call";
    }
}
```

`OnTranslate` Metoda tłumaczy phoneword do odpowiedniego numeru telefonu, a w odpowiedzi, ustawia właściwości przycisk wywołania. Plik CodeBehind dla klasy XAML można uzyskać dostępu do obiektu zdefiniowany w języku XAML, przy użyciu nazwy przypisanej do niego z `x:Name` atrybutu. Wartość przypisana do tego atrybutu ma te same zasady, jak C# zmienne, musi rozpoczynać się od litery lub podkreślenia i zawierać nie spacji osadzonych.

Okablowanie przycisku Przetłumacz `OnTranslate` metody występuje w kodzie XAML dla `MainPage` klasy:

```xaml
<Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Dodatkowe założenia Phoneword

Phoneword aplikacji dla platformy Xamarin.Forms wprowadziła kilka pojęć, które nie zostały omówione w tym artykule. Pojęcia te obejmują:

- Włączanie i wyłączanie przycisków. A [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) może być włączony lub wyłączony, zmieniając jej [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) właściwości. Na przykład poniższy kod przykładowy wyłącza `callButton`:

    ```csharp
    callButton.IsEnabled = false;
    ```

- Wyświetlanie okna dialogowego alertu. Gdy użytkownik naciśnie wywołanie **przycisk** pokazuje aplikacji Phoneword *okna dialogowego alertu* z opcją Umieść lub anulować wywołania. [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/System.String/) Metoda służy do tworzenia okna dialogowego, jak pokazano w poniższym przykładzie:

    ```csharp
    await this.DisplayAlert (
            "Dial a Number",
            "Would you like to call " + translatedNumber + "?",
            "Yes",
            "No");
    ```

- Uzyskiwanie dostępu do natywnego funkcji za pośrednictwem [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) klasy. Używa aplikacji Phoneword `DependencyService` klasę, aby rozwiązać `IDialer` interfejs do implementacji wybierania numeru telefonu specyficzne dla platformy, jak pokazano w poniższym przykładzie kodu z projektu Phoneword:

    ```csharp
    async void OnCall (object sender, EventArgs e)
    {
        ...
        var dialer = DependencyService.Get<IDialer> ();
        ...
    }
    ```

  Aby uzyskać więcej informacji na temat [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) , zobacz [podczas uzyskiwania dostępu do funkcji natywnego za pośrednictwem DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

- Umieszczenie połączenie telefoniczne z adresem URL. Aplikacja Phoneword używa `OpenURL` można uruchomić aplikacji phone systemu. Adres URL składa się z `tel:` prefiks następuje numer telefonu ma być wywoływana, jak pokazano w poniższym przykładzie kodu z projektu iOS:

    ```csharp
    return UIApplication.SharedApplication.OpenUrl (new NSUrl ("tel:" + number));
    ```

- Dostosowywanie układu platformy. [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Klasa umożliwia deweloperom dostosowywać układ aplikacji i funkcje na podstawie na platformie, jak pokazano w poniższym przykładzie kodu, który używa innego [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)wartości na różnych platformach, aby poprawnie wyświetlić stronę:

    ```xaml
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms" ... >
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        ...
    </ContentPage>
    ```

  Aby uzyskać więcej informacji na temat ulepszeń platformy, zobacz [klasę urządzeń](~/xamarin-forms/platform/device.md).

## <a name="testing-and-deployment"></a>Testowanie i wdrażanie

Visual Studio for Mac i Visual Studio oferują wiele opcji testowania i wdrażania aplikacji. Debugowanie aplikacji jest typowe część cyklu programistycznym z aplikacji i ułatwia diagnozowanie problemów z kodem. Aby uzyskać więcej informacji, zobacz [Ustaw punkt przerwania](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/), [kod za pomocą kroku](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/), i [dane wyjściowe do okna dziennika](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/).

Symulatorów są dobrym miejscem do rozpoczęcia wdrażania i testowania aplikacji i funkcji funkcja przydatna przy testowaniu aplikacji. Jednak użytkownicy nie będą korzystać końcowego aplikacji w symulatorze, dzięki aplikacji powinny być badane na urządzeniach prawdziwe, wczesne i często. Aby uzyskać więcej informacji dotyczących obsługi urządzenia z systemem iOS, zobacz [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md). Aby uzyskać więcej informacji dotyczących obsługi urządzenia z systemem Android, zobacz [ustawić się urządzenia na potrzeby programowania](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="summary"></a>Podsumowanie

Ten artykuł zawiera zbadać podstawowe informacje na temat tworzenia aplikacji za pomocą platformy Xamarin.Forms. Tematy zawarte Anatomia aplikacji platformy Xamarin.Forms, architektura i podstawowe informacje na temat aplikacji oraz interfejsu użytkownika.

W następnej sekcji tego przewodnika aplikacji zostanie rozszerzony uwzględnienie wielu ekranów, aby zapoznać się z bardziej zaawansowanych architektura platformy Xamarin.Forms i pojęcia.
