---
title: "Aktualizowanie istniejącej aplikacji platformy Xamarin.Forms"
description: "Wykonaj następujące kroki, aby zaktualizować istniejącą aplikację platformy Xamarin.Forms za pomocą interfejsu API Unified i aktualizacji do wersji 1.3.1"
ms.topic: article
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: d2f14510e5968ebe24bd297365416fa8aa5a0c59
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="updating-existing-xamarinforms-apps"></a>Aktualizowanie istniejącej aplikacji platformy Xamarin.Forms

_Wykonaj następujące kroki, aby zaktualizować istniejącą aplikację platformy Xamarin.Forms za pomocą interfejsu API Unified i aktualizacji do wersji 1.3.1_

> [!IMPORTANT]
> Ponieważ platformy Xamarin.Forms 1.3.1 pierwszej wersji, który obsługuje interfejsu API Unified, aby używać najnowszej wersji w tym samym czasie jako migracja aplikacji systemu iOS do Unified należy zaktualizować całego rozwiązania. Oznacza to, że oprócz aktualizowanie projektu iOS Unified pomocy technicznej, również należy edytować kod w _wszystkie_ projektów w rozwiązaniu.

Aktualizacja jest wykonywana w dwóch krokach:

1. Przeprowadź migrację aplikacji systemu iOS do interfejsu API Unified, przy użyciu programu Visual Studio dla kompilacji dla komputerów Mac w narzędziu do migracji.

    - Narzędzie migracji automatycznie aktualizować projektu.

    - Aktualizacja dla systemu iOS native interfejsów API, zgodnie z instrukcjami, aby [aktualizacji aplikacji systemu iOS](~/cross-platform/macios/unified/updating-ios-apps.md) (w szczególności w niestandardowego modułu renderowania lub zależności usługi kodu).

2. Aktualizacji całym rozwiązaniu platformy Xamarin.Forms w wersji 1.3.

    1. Zainstaluj pakiet NuGet 1.3.1 platformy Xamarin.Forms.

    2. Aktualizacja `App` klasy w kodzie udostępnionego.

    3. Aktualizacja `AppDelegate` w projekcie systemu iOS.

    4. Aktualizacja `MainActivity` w projekcie systemu Android.

    5. Aktualizacja `MainPage` w projekcie Windows Phone.

## <a name="1-ios-app-unified-migration"></a>1. Aplikacja systemu iOS (Unified migracja)

Podczas migracji wymaga uaktualnienia do wersji 1.3, która obsługuje interfejsu API Unified platformy Xamarin.Forms. W kolejności poprawne odwołań do zestawów do utworzenia najpierw musimy zaktualizować projekt dla systemu iOS, aby za pomocą interfejsu API Unified.

### <a name="migration-tool"></a>Narzędzia do migracji

Kliknij projekt dla systemu iOS, że jest zaznaczone, następnie wybierz pozycję **projektu > migracji Xamarin.iOS Unified API...**  i zgadzam się ostrzeżenie informujące, że jest wyświetlany.

![](updating-xamarin-forms-apps-images/beta-tool1.png "Wybierz projekt > migrację do interfejsu API platformy Xamarin.iOS Unified... i zgadzam się ostrzeżenie informujące, że jest wyświetlany")

Spowoduje to automatyczne:

 - Zmień typ projektu do obsługi interfejsu API Unified 64-bitowych.
 - Zmień odwołanie framework, aby **Xamarin.iOS** (miejsce starej **monotouch** odwołania).
 - Zmień odwołania do przestrzeni nazw w kodzie, aby usunąć `MonoTouch` prefiks.
 - Aktualizacja **csproj** plik elementów docelowych poprawne kompilacji dla interfejsu API Unified.

**Wyczyść** i **kompilacji** projektu, aby upewnić się, nie ma żadnych innych błędów, aby rozwiązać problem. Żadne dalsze akcje nie powinno być wymagane. Te kroki są co omówiono bardziej szczegółowo w [docs Unified API](~/cross-platform/macios/unified/updating-ios-apps.md).

### <a name="update-native-ios-apis-if-required"></a>Zaktualizuj natywnych interfejsów API dla systemu iOS (jeśli jest to wymagane)

Po dodaniu dodatkowych iOS kodu natywnego (np. niestandardowe moduły renderowania lub zależności usług) może być konieczne poprawki dodatkowy kod ręcznego wykonania. Ponowne kompilowanie aplikacji i zapoznaj się z [iOS aktualizowania istniejących aplikacji instrukcje](~/cross-platform/macios/unified/updating-ios-apps.md) dodatkowe informacje na temat zmian, które mogą być wymagane. [Te wskazówki](~/cross-platform/macios/unified/updating-tips.md) pomoże zidentyfikować zmiany, które są wymagane.

## <a name="2-xamarinforms-131-update"></a>2. Xamarin.Forms 1.3.1 Update

Po zaktualizowaniu aplikacji systemu iOS do interfejsu API Unified reszty rozwiązanie musi zostać zaktualizowany do wersji 1.3.1 platformy Xamarin.Forms. Możliwości obejmują:

 - Aktualizowanie pakietu NuGet platformy Xamarin.Forms w każdym projekcie.
 - Zmiana kodu w celu użycia nowych platformy Xamarin.Forms `Application`, `FormsApplicationDelegate` (iOS), `FormsApplicationActivity` (Android), a `FormsApplicationPage` klasy (Windows Phone).

Poniżej opisano następujące kroki:

### <a name="21-update-nuget-in-all-projects"></a>2.1 NuGet aktualizacji we wszystkich projektach

Aktualizacja platformy Xamarin.Forms do 1.3.1 wersji wstępnej, za pomocą Menedżera pakietów NuGet dla wszystkich projektów w rozwiązaniu: PCL (jeśli istnieje), iOS, Android i Windows Phone. Zaleca się że **usunąć i ponownie dodać** pakiet NuGet platformy Xamarin.Forms aktualizacji do wersji 1.3.

**Uwaga:** jest obecnie w wersji 1.3.1 platformy Xamarin.Forms *wersji wstępnej*. Oznacza to, musisz wybrać **wstępną** opcji w NuGet za pośrednictwem (znaczników — pole w programie Visual Studio dla komputerów Mac) lub w dół listy rozwijanej w programie Visual Studio, aby wyświetlić najnowszą wersję wstępną.

> [!IMPORTANT]
> Jeśli używasz programu Visual Studio, upewnij się, że jest zainstalowana najnowsza wersja Menedżera pakietów NuGet. Starsze wersje programu NuGet w programie Visual Studio nie zainstaluje poprawnie ujednolicona wersja 1.3.1 platformy Xamarin.Forms. Przejdź do **Narzędzia > rozszerzenia i aktualizacje...**  i wybierz polecenie **zainstalowana** listy, aby sprawdzić, czy **Menedżera pakietów NuGet dla programu Visual Studio** co najmniej wersji 2.8.5. Jeśli jest to starszy polecenie **aktualizacje** listy, aby pobrać najnowszą wersję.

Po zaktualizowaniu pakietu NuGet do platformy Xamarin.Forms 1.3.1, wprowadź następujące zmiany w każdym projekcie do uaktualnienia do nowego `Xamarin.Forms.Application` klasy.

### <a name="22-portable-class-library-or-shared-project"></a>2.2 przenośnej biblioteki klas (lub udostępnionego projektu)

Zmień **App.cs** pliku, aby:

 - `App` Teraz dziedziczy klasa `Application`.
 - `MainPage` Właściwość jest ustawiona na pierwszej strony zawartości, które mają zostać wyświetlone.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

Całkowicie usunęliśmy `GetMainPage` metody, a zamiast tego ustaw `MainPage` *właściwości* na `Application` podklasy.

Nowy `Application` obsługuje również klasy podstawowej `OnStart`, `OnSleep`, i `OnResume` zastąpień w celu zarządzania cyklem życia aplikacji.

`App` Klasy są następnie przekazywane do nowego `LoadApplication` metody w każdym projekcie aplikacji, zgodnie z poniższym opisem:

### <a name="23-ios-app"></a>2.3 aplikacja systemu iOS

Zmień **AppDelegate.cs** pliku, aby:

 - Klasa dziedziczy `FormsApplicationDelegate` (zamiast `UIApplicationDelegate` wcześniej).
 - `LoadApplication` jest wywoływana z nowe wystąpienie klasy `App`.

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

### <a name="23-android-app"></a>2.3 aplikacji systemu android

Zmień **MainActivity.cs** pliku, aby:

 - Klasa dziedziczy `FormsApplicationActivity` (zamiast `FormsActivity` wcześniej).
 - `LoadApplication` jest wywoływana z nowe wystąpienie klasy `App`

```csharp
[Activity (Label = "YOURAPPNAM", Icon = "@drawable/icon", MainLauncher = true,
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

### <a name="24-windows-phone-app"></a>2.4 Windows Phone aplikacji

Należy zaktualizować **MainPage** — zarówno w pliku XAML, jak i plik codebehind.

Zmień **MainPage.xaml** pliku, aby:

 - Element główny XAML powinny być `winPhone:FormsApplicationPage`.
 - `xmlns:phone` Atrybut powinien być *zmienić* do `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

Poniżej przedstawiono przykład zaktualizowane — powinien mieć tylko do edytowania tych czynności (pozostałe atrybuty powinny być takie same):

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

Zmień **MainPage.xaml.cs** pliku, aby:

 - Klasa dziedziczy `FormsApplicationPage` (zamiast `PhoneApplicationPage` wcześniej).
 - `LoadApplication` jest wywoływana przy użyciu nowego wystąpienia platformy Xamarin.Forms `App` klasy. Może być konieczne do pełnej kwalifikacji to odwołanie, ponieważ Windows Phone ma własne `App` klasy już zdefiniowany.

```csharp
public partial class MainPage : global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3
    }
 }
```

### <a name="troubleshooting"></a>Rozwiązywanie problemów

Czasami zostanie wyświetlony błąd podobny do poniższego po zaktualizowaniu pakietu NuGet platformy Xamarin.Forms. Występuje, gdy NuGet updater nie całkowicie usunąć odwołania do starszych wersji z Twojego **csproj** plików.

>Twoje\_PROJECT.csproj: błąd: ten projekt zawiera odwołania do pakietów NuGet Brak na tym komputerze. Włącz Przywracanie pakietu NuGet, aby je pobrać.  Aby uzyskać więcej informacji, zobacz http://go.microsoft.com/fwlink/?LinkID=322105. Brak pliku jest... /.. /Packages/Xamarin.Forms.1.2.3.6257/Build/Portable-win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.TARGETS. (TWOJEJ\_PROJEKTU)

Aby usunąć te błędy, otwórz **csproj** plik w edytorze tekstów i Znajdź `<Target` elementy, które odwołują się do starszych wersji platformy Xamarin.Forms, takich jak pokazano poniżej elementu. Należy ręcznie usunąć to cały element z **csproj** plik i zapisać zmiany.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

Projekt powinien pomyślnie kompilacji, po usunięciu tych stare odwołania.

## <a name="considerations"></a>Uwagi

Następujące kwestie należy brane pod uwagę podczas konwertowania istniejącego projektu platformy Xamarin.Forms klasycznego interfejsu API na nowy interfejs API Unified Jeśli danej aplikacji zależy od składnika lub pakietu NuGet.

### <a name="components"></a>Składniki

Każdego składnika, który jest dołączony do aplikacji będzie również muszą zostać zaktualizowane do interfejsu API Unified lub wystąpi konflikt podczas kompilacji. Dla każdego składnika dołączone zastąpić bieżącą wersję za pomocą nowej wersji w magazynie składników Xamarin obsługującego interfejsu API Unified i wykonać czystą kompilację. Każdego składnika, który nie został jeszcze przekonwertowany przez autora, będą wyświetlane tylko ostrzeżenie w magazynie składników 32-bitowym.

### <a name="nuget-support"></a>Obsługa NuGet

Gdy firma Microsoft przyczyniły się zmiany NuGet do pracy z obsługą Unified API, nie została nową wersję programu NuGet, dlatego firma Microsoft ocenia jak uzyskać NuGet do rozpoznawania nowych interfejsów API.

Do tego czasu, podobnie jak składniki musisz przełączyć dowolnego pakietu NuGet, zostały uwzględnione w projekcie na wersję obsługującą interfejsy API Unified i wykonaj czystą kompilację później.

> [!IMPORTANT]
> Jeśli masz wystąpił błąd w formie _"błąd 3 nie może zawierać zarówno"monotouch.dll"i"Xamarin.iOS.dll"w tym samym projekcie platformy Xamarin.iOS —"Xamarin.iOS.dll"odwołuje się do jawnie, gdy"monotouch.dll"odwołuje się do niego" xxx, wersja = 0.0.000, Culture = neutral, PublicKeyToken = null ""_ po przekonwertowaniu aplikacji do interfejsów API Unified, jest zazwyczaj z powodu konieczności składnika lub pakietu NuGet w projekcie, który nie został jeszcze zaktualizowany do interfejsu API Unified. Należy usunąć istniejący składnik/NuGet, aktualizacja do wersji, która obsługuje interfejsy API Unified i wykonać czystą kompilację.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Włączanie 64-bitowym kompilacji aplikacji platformy Xamarin.iOS

Dla aplikacji mobilnej platformy Xamarin.iOS, który został przekonwertowany do interfejsu API Unified Deweloper nadal musi umożliwiają tworzenie aplikacji na komputerach 64-bitowych z opcji aplikacji. Zobacz **Włączanie 64 bitowej kompilacje dla aplikacji platformy Xamarin.iOS** z [zagadnień dotyczących platformy 32/x 64](~/cross-platform/macios/32-and-64/index.md#enable-64) kompilacje dokumentu, aby uzyskać szczegółowe instrukcje na temat włączania 64-bitowym.

## <a name="summary"></a>Podsumowanie

Teraz można zaktualizować aplikacji platformy Xamarin.Forms do wersji 1.3.1 i aplikacji dla systemu iOS migracji Unified API (który obsługuje architektur 64-bitowych na platformie iOS).

Jak wspomniano powyżej, jeśli aplikację platformy Xamarin.Forms zawiera kod natywny, takich jak niestandardowe moduły renderowania lub zależności usług, a następnie te mogą także aktualizowanie w celu użycia nowych typów [wprowadzone w interfejsie API Unified](~/cross-platform/macios/index.md).

## <a name="related-links"></a>Linki pokrewne

- [Aktualizowanie aplikacji dla systemu iOS](~/cross-platform/macios/unified/updating-apps.md)
- [Aktualizowanie aplikacji dla systemu iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Praca z typami natywnymi w aplikacjach międzyplatformowych](~/cross-platform/macios/native-types-cross-platform.md)
- [Aktualizowanie porady](~/cross-platform/macios/unified/updating-tips.md)
- [Klasycznym vs różnice Unified API](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
