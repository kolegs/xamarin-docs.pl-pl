---
title: Rozszerzony opis zestawu narzędzi Xamarin.Forms
description: W tym artykule sprawdza podstawy tworzenia aplikacji przy użyciu zestawu narzędzi Xamarin.Forms. Omawiane tematy zawarte Anatomia aplikacji platformy Xamarin.Forms, architektury i podstawowe informacje dotyczące aplikacji i interfejsu użytkownika.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: d97aa580-1eb9-48b3-b15b-0d7421ea7ae
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: 7eff7f4413b533caadcf2aa8b5eed8c4ab65449d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242228"
---
# <a name="xamarinforms-deep-dive"></a>Rozszerzony opis zestawu narzędzi Xamarin.Forms

W [szybkiego startu zestawu narzędzi Xamarin.Forms](~/xamarin-forms/get-started/hello-xamarin-forms/quickstart.md), aplikacja Phoneword została skompilowana. W tym artykule opisano, jakie została zbudowana w celu uzyskania informacji na temat podstawowe informacje dotyczące sposobu działania aplikacji platformy Xamarin.Forms.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Wprowadzenie do programu Visual Studio

Visual Studio to zaawansowane środowisko IDE firmy Microsoft. Zawiera funkcje, w pełni zintegrowane wizualnego projektanta, edytora tekstów, pełną za pomocą narzędzi refaktoryzacji, przeglądarka zestawów i integracji kodu źródłowego. Ten artykuł koncentruje się na temat korzystania z niektórych podstawowych funkcji programu Visual Studio za pomocą platformy Xamarin wtyczki.

Program Visual Studio umożliwia organizowanie kodu w *rozwiązania* i *projektów*. To rozwiązanie jest kontenerem, który może zawierać jeden lub więcej projektów. Projekt może być aplikacja, obsługi biblioteki, aplikacja testowa i. Aplikacja Phoneword składa się z jednego rozwiązania zawierającego czterema projektami, jak pokazano na poniższym zrzucie ekranu.

![](deepdive-images/vs/solution.png "Eksplorator rozwiązań programu Visual Studio")

Projekty są:

- Phoneword — ten projekt jest projekt biblioteki .NET Standard, który przechowuje wszystkie wspólny kod i Współdzielony interfejs użytkownika.
- Phoneword.Android — ten projekt zawiera kodu dla systemu Android określonych i jest punktem wejścia dla aplikacji systemu Android.
- Phoneword.iOS — ten projekt zawiera konkretnego kodu dla systemu iOS i jest punkt wejścia dla aplikacji dla systemu iOS.
- Phoneword.UWP — ten projekt zawiera konkretny kod uniwersalnej platformy Windows (UWP) i jest punktem wejścia dla aplikacji platformy uniwersalnej systemu Windows.

## <a name="anatomy-of-a-xamarinforms-application"></a>Anatomia aplikacji platformy Xamarin.Forms

Poniższy zrzut ekranu przedstawia zawartość Phoneword .NET Standard projektu biblioteki w programie Visual Studio:

![](deepdive-images/vs/net-standard-project.png "Zawartość standardowy projekt .NET Phoneword")

Projekt ma **zależności** węzeł, który zawiera **NuGet** i **SDK** węzłów. **NuGet** węzeł zawiera pakiet Xamarin.Forms NuGet, który został dodany do projektu, a **SDK** zawiera węzeł `NETStandard.Library` meta Microsoft.aspnetcore.all, który odwołuje się do pełnego zestawu pakietów NuGet definiują .NET Standard.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Wprowadzenie do programu Visual Studio dla komputerów Mac

Program Visual Studio for Mac to bezpłatne, typu open-source środowisko IDE podobne do programu Visual Studio. Zawiera funkcje, w pełni zintegrowane wizualnego projektanta, edytora tekstów, pełną za pomocą narzędzi refaktoryzacji, przeglądarka zestawów i integracji kodu źródłowego. Aby uzyskać więcej informacji na temat programu Visual Studio for Mac zobacz [wprowadzenie do programu Visual Studio dla komputerów Mac](/visualstudio/mac/).

Program Visual Studio for Mac następuje rozwiązanie programu Visual Studio kod do organizowania *rozwiązania* i *projektów*. To rozwiązanie jest kontenerem, który może zawierać jeden lub więcej projektów. Projekt może być aplikacja, obsługi biblioteki, aplikacja testowa i. Aplikacja Phoneword składa się z jednego rozwiązania, zawierający trzy projekty, jak pokazano na poniższym zrzucie ekranu.

![](deepdive-images/xs/solution.png "Program Visual Studio for Mac okienko rozwiązania")

Projekty są:

- Phoneword — ten projekt jest projekt biblioteki .NET Standard, który przechowuje wszystkie wspólny kod i Współdzielony interfejs użytkownika.
- Phoneword.Droid — ten projekt zawiera kodu dla systemu Android określonych i jest punktem wejścia dla aplikacji systemu Android.
- Phoneword.iOS — ten projekt zawiera konkretnego kodu dla systemu iOS i jest punkt wejścia dla aplikacji systemu iOS.

## <a name="anatomy-of-a-xamarinforms-application"></a>Anatomia aplikacji platformy Xamarin.Forms

Poniższy zrzut ekranu przedstawia zawartość Phoneword .NET Standard projektu biblioteki w programie Visual Studio dla komputerów Mac:

![](deepdive-images/xs/library-project.png "Zawartość projektu biblioteki standardowej .NET Phoneword")

Projekt ma **zależności** węzeł, który zawiera **NuGet** i **SDK** węzłów. **NuGet** węzeł zawiera pakiet Xamarin.Forms NuGet, który został dodany do projektu, a **SDK** zawiera węzeł `NETStandard.Library` meta Microsoft.aspnetcore.all, który odwołuje się do pełnego zestawu pakietów NuGet definiują .NET Standard.

-----

Projekt zawiera również liczbę plików:

- **App.XAML** — kod znaczników dla XAML `App` klasy, która definiuje słownik zasobów dla aplikacji.
- **App.XAML.cs** — CodeBehind dla `App` klasy, która jest odpowiedzialny za utworzenie wystąpienia pierwsza strona, która będzie wyświetlana przez aplikację na każdej platformie i obsługa zdarzeń cyklu życia aplikacji.
- **IDialer.cs** — `IDialer` interfejs, który określa, że `Dial` musi być podana metoda przez wszystkie klasy implementującej.
- **MainPage.xaml** — kod znaczników dla XAML `MainPage` klasy, która definiuje interfejsu użytkownika dla strony wyświetlane po uruchomieniu aplikacji.
- **MainPage.xaml.cs** — CodeBehind dla `MainPage` klasy, która zawiera logikę biznesową, która jest wykonywany, gdy użytkownik wchodzi w interakcję ze stroną.
- **PhoneTranslator.cs** — logikę biznesową, która jest odpowiedzialna za konwertowanie word telefonu na numer telefonu, który jest wywoływany z **MainPage.xaml.cs**.

Aby uzyskać więcej informacji o strukturze aplikacji platformy Xamarin.iOS, zobacz [Anatomia aplikacji platformy Xamarin.iOS](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application). Aby uzyskać więcej informacji o strukturze aplikacji platformy Xamarin.Android, zobacz [Anatomia aplikacji platformy Xamarin.Android](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Architektura i podstawy aplikacji

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aplikacja Xamarin.Forms została zaprojektowana w taki sam sposób jak tradycyjnych aplikacji dla wielu platform. Udostępnionego kodu źródłowego zwykle jest umieszczany w biblioteki .NET Standard, a aplikacje specyficzne dla platformy zużyć udostępnionego kodu. Na poniższym diagramie przedstawiono omówienie tej relacji dla aplikacji Phoneword:

![](deepdive-images/vs/architecture.png "Architektura Phoneword")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aplikacja Xamarin.Forms została zaprojektowana w taki sam sposób jak tradycyjnych aplikacji dla wielu platform. Udostępnionego kodu źródłowego zwykle jest umieszczany w biblioteki .NET Standard, a aplikacje specyficzne dla platformy zużyć udostępnionego kodu. Na poniższym diagramie przedstawiono omówienie tej relacji dla aplikacji Phoneword:

![](deepdive-images/xs/architecture.png "Architektura Phoneword")

-----

Aby zmaksymalizować ponowne użycie kodu uruchamiania, aplikacje Xamarin.Forms mają jedną klasę o nazwie `App` odpowiada podczas tworzenia wystąpienia pierwsza strona, która będzie wyświetlana przez aplikację na każdej platformie, jak pokazano w poniższym przykładzie kodu:

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

Ten kod ustawia `MainPage` właściwość `App` nowe wystąpienie klasy [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) klasy. Ponadto [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) atrybut włącza kompilator XAML, dzięki czemu XAML jest skompilowany bezpośrednio w języku pośrednim. Aby uzyskać więcej informacji, zobacz [kompilacji XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Trwa uruchamianie aplikacji na każdej platformie

### <a name="ios"></a>iOS

Aby uruchomić stronę początkową Xamarin.Forms w systemie iOS, zawiera projekt Phoneword.iOS `AppDelegate` klasy dziedziczącej z klasy `FormsApplicationDelegate` klasy, jak pokazano w poniższym przykładzie kodu:

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

`FinishedLaunching` Zastąpienie jest inicjowana w ramach zestawu narzędzi Xamarin.Forms, wywołując `Init` metody. Powoduje to, że implementacja specyficzne dla systemu iOS zestawu narzędzi Xamarin.Forms do załadowania w aplikacji, zanim kontroler widoku głównego jest ustawiana przez wywołanie metody `LoadApplication` metody.

### <a name="android"></a>Android

Aby uruchomić stronę początkową Xamarin.Forms w systemie Android, projekt Phoneword.Droid zawiera kod, który tworzy `Activity` z `MainLauncher` atrybutu za pomocą działania dziedziczenie z `FormsAppCompatActivity` klasy, jak pokazano w poniższym przykładzie kodu:

```csharp
namespace Phoneword.Droid
{
    [Activity(Label = "Phoneword", 
              Icon = "@mipmap/icon", 
              Theme = "@style/MainTheme", 
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        internal static MainActivity Instance { get; private set; }

        protected override void OnCreate(Bundle bundle)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(bundle);
            Instance = this;
            global::Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication(new App());
        }
    }
}
```

`OnCreate` Zastąpienie jest inicjowana w ramach zestawu narzędzi Xamarin.Forms, wywołując `Init` metody. Powoduje to implementacji specyficznych dla systemu Android Xamarin.Forms, aby można było załadować aplikację przed załadowaniem aplikacji platformy Xamarin.Forms. Ponadto `MainActivity` klasa przechowuje odwołania do samego siebie w `Instance` właściwości. `Instance` Właściwość jest znany jako kontekstu lokalnego i jest wywoływany przez `PhoneDialer` klasy.

## <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

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

Jest załadowana w aplikacji platformy Xamarin.Forms `LoadApplication` metody.

> [!NOTE]
> Uniwersalnych aplikacji dla platformy Windows (UWP) może być utworzone przy użyciu zestawu narzędzi Xamarin.Forms, ale tylko za pomocą programu Visual Studio na Windows.

## <a name="user-interface"></a>Interfejs użytkownika

Istnieją cztery grupy w główną kontrolką użyty do utworzenia interfejsu użytkownika aplikacji platformy Xamarin.Forms.

1. **Strony** — strony zestawu narzędzi Xamarin.Forms reprezentują ekrany aplikacji mobilnych dla wielu platform. Aplikacja Phoneword używa [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) klasy do wyświetlenia na jednym ekranie. Aby uzyskać więcej informacji dotyczących stron, zobacz [strony zestawu narzędzi Xamarin.Forms](~/xamarin-forms/user-interface/controls/pages.md).
1. **Układy** — układy platformy Xamarin.Forms to kontenery, które umożliwiają tworzenie widoków w logicznej struktury. Aplikacja Phoneword używa [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) klasy, aby rozmieścić formanty w stosie poziomej. Aby uzyskać więcej informacji na temat układów, zobacz [układy platformy Xamarin.Forms](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Widoki** — Xamarin.Forms widoki są formanty wyświetlane w interfejsie użytkownika, takie jak etykiety, przyciski i pola wprowadzania tekstu. Aplikacja Phoneword używa [ `Label` ](xref:Xamarin.Forms.Label), [ `Entry` ](xref:Xamarin.Forms.Entry), i [ `Button` ](xref:Xamarin.Forms.Button) kontrolki. Aby uzyskać więcej informacji o widokach, zobacz [widoków Xamarin.Forms](~/xamarin-forms/user-interface/controls/views.md).
1. **Komórki** — Xamarin.Forms komórek są specjalne elementy używane na potrzeby elementów na liście i opisano, jak ma być rysowany każdy element na liście. Użyj Phoneword, których aplikacja nie wprowadzać żadnych komórek. Aby uzyskać więcej informacji na temat komórek, zobacz [komórki zestawu narzędzi Xamarin.Forms](~/xamarin-forms/user-interface/controls/cells.md).

W czasie wykonywania każdy formant zostanie zamapowane do równoważnik natywnych to, co będzie renderowana.

Po uruchomieniu aplikacji Phoneword na dowolnej platformie wyświetla pojedynczy ekran, który odpowiada [ `Page` ](xref:Xamarin.Forms.Page) w interfejsie Xamarin.Forms. A `Page` reprezentuje *grupie widoków* w systemie Android, *kontrolera widoku* w systemie iOS, czy też *strony* na platformie Universal Windows. Również tworzy wystąpienie aplikacji Phoneword [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) obiekt, który reprezentuje `MainPage` klasy, w których znaczników XAML jest wyświetlana w poniższym przykładzie kodu:

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

`MainPage` Klasy używa [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) formantu, aby automatycznie rozmieścić formanty na ekranie, niezależnie od rozmiaru ekranu. Każdy element podrzędny jest pozycjonowane jedna po drugiej w pionie w kolejności, w której są dodawane. `StackLayout` Kontrolka zawiera [ `Label` ](xref:Xamarin.Forms.Label) sterowania do wyświetlania tekstu na stronie [ `Entry` ](xref:Xamarin.Forms.Entry) formantu, aby akceptować dane wejściowe użytkownika tekstowych, a dwa [ `Button` ](xref:Xamarin.Forms.Button) formantów służących do wykonywania kodu w odpowiedzi na zdarzenia dotykowe.

Aby uzyskać więcej informacji na temat XAML w interfejsie Xamarin.Forms, zobacz [podstawy XAML zestawu narzędzi Xamarin.Forms](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="responding-to-user-interaction"></a>Reagowanie na interakcję z użytkownikiem

Obiekt zdefiniowany w XAML może wywołać zdarzenie, które odbywa się w pliku związanym z kodem. Poniższy kod przedstawia przykład `OnTranslate` metody w CodeBehind dla `MainPage` klasy, która jest wykonywana w odpowiedzi na [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) zdarzenie na *Translate* przycisk.

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

`OnTranslate` Metoda tłumaczy phoneword w odpowiadającym im numerem telefonu, a w odpowiedzi, ustawia właściwości przycisku wywołania. Plik CodeBehind dla klasy XAML ma dostęp do obiektu, zdefiniowane w XAML przy użyciu nazwy, przypisane do niego za pomocą `x:Name` atrybutu. Wartość przypisana do tego atrybutu ma te same reguły jako języka C# zmienne, w tym jego musi zaczynać się literą lub znakiem podkreślenia i może zawierać spacji osadzonych.

Okablowanie przycisku translate `OnTranslate` metoda występuje w znaczniku XAML dla `MainPage` klasy:

```xaml
<Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Dodatkowe założenia Phoneword

Aplikacja Phoneword dla platformy Xamarin.Forms wprowadziła kilka koncepcji, które nie zostały omówione w tym artykule. Te pojęcia obejmują:

- Włączanie i wyłączanie przycisków. A [ `Button` ](xref:Xamarin.Forms.Button) mogą być włączane lub wyłączane, zmieniając jego [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) właściwości. Na przykład, poniższy kod przykładowy wyłącza `callButton`:

    ```csharp
    callButton.IsEnabled = false;
    ```

- Wyświetlanie okna dialogowego alertu. Gdy użytkownik naciśnie wywołanie **przycisk** pokazuje aplikacji Phoneword *okna dialogowego alertu* z możliwością umieścić lub anulować wywołanie. [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) Metoda jest używana do tworzenia okna dialogowego, jak pokazano w poniższym przykładzie kodu:

    ```csharp
    await this.DisplayAlert (
            "Dial a Number",
            "Would you like to call " + translatedNumber + "?",
            "Yes",
            "No");
    ```

- Dostęp do natywnych funkcji za pośrednictwem [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) klasy. Aplikacja Phoneword używa `DependencyService` klasy, aby rozwiązać `IDialer` współpracować w celu implementacji wybierania numeru telefonu specyficzne dla platformy, jak pokazano w poniższym przykładzie kodu z projektu Phoneword:

    ```csharp
    async void OnCall (object sender, EventArgs e)
    {
        ...
        var dialer = DependencyService.Get<IDialer> ();
        ...
    }
    ```

  Aby uzyskać więcej informacji na temat [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) klasy, zobacz [dostęp do natywnych funkcji za pośrednictwem DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

- Wprowadzenie do rozmowy telefonicznej z adresem URL. Aplikacja Phoneword używa `OpenURL` można uruchomić aplikację systemu. Adres URL składa się z `tel:` prefiks następuje numer telefonu do wywołania, jak pokazano w poniższym przykładzie kodu z projektu systemu iOS:

    ```csharp
    return UIApplication.SharedApplication.OpenUrl (new NSUrl ("tel:" + number));
    ```

- Dostosowywanie układu platformy. [ `Device` ](xref:Xamarin.Forms.Device) Klasy pozwala deweloperom na dostosowywanie układu aplikacji i funkcjonalności na poszczególnych platform, jak pokazano w poniższym przykładzie kodu, który używa innego [ `Padding` ](xref:Xamarin.Forms.Layout.Padding)wartości na różnych platformach, aby poprawnie wyświetlić każdą stronę:

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

  Aby uzyskać więcej informacji na temat ulepszeń platformy, zobacz [klasę urządzeń pamięci](~/xamarin-forms/platform/device.md).

## <a name="testing-and-deployment"></a>Testowanie i wdrażanie

Visual Studio dla komputerów Mac i Visual Studio oferują wiele opcji testowania i wdrażania aplikacji. Debugowanie aplikacji jest wspólne część cyklu życia tworzenia aplikacji i ułatwia diagnozowanie problemów z kodem. Aby uzyskać więcej informacji, zobacz [Ustaw punkt przerwania](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [kod za pomocą kroku](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code), i [informacji wyjściowych w oknie dziennika](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

Symulatorów są dobrym miejscem do rozpoczęcia wdrażania i testowania aplikacji, a funkcji przydatnych funkcji do testowania aplikacji. Jednak użytkownicy nie zużyje końcowy aplikacji w symulatorze, więc powinien zostać przetestowany aplikacje na prawdziwych urządzeniach, wcześnie i często. Aby uzyskać więcej informacji na temat inicjowania obsługi urządzeń z systemem iOS, zobacz [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md). Aby uzyskać więcej informacji na temat inicjowania obsługi urządzeń z systemem Android, zobacz [Ustaw się urządzenia na potrzeby programowania](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="summary"></a>Podsumowanie

Ten artykuł ma zbadać podstawy tworzenia aplikacji przy użyciu zestawu narzędzi Xamarin.Forms. Omawiane tematy zawarte Anatomia aplikacji platformy Xamarin.Forms, architektury i podstawowe informacje dotyczące aplikacji i interfejsu użytkownika.

W następnej części tego przewodnika, aplikacja zostanie rozszerzony obejmujący wiele ekranów, aby zapoznać się z bardziej zaawansowanego zestawu narzędzi Xamarin.Forms architektury i pojęcia.
