---
title: Szybki Start platformy Xamarin.Forms
ms.topic: article
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: 7ce674ea38bc847bc9064a5a61113900a45b991d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinforms-quickstart"></a>Szybki Start platformy Xamarin.Forms

W tym przewodniku pokazano, jak utworzyć aplikację konwertujący alfanumeryczne numer wprowadzony przez użytkownika do numeru telefonu liczbowych i kod, który odwołuje. Końcowe aplikacji jest pokazany poniżej:

[![](quickstart-images/intro-app-examples-sml.png "Aplikacja Phoneword")](quickstart-images/intro-app-examples.png "Phoneword aplikacji")

Utwórz aplikację Phoneword w następujący sposób:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. W **Start** ekranu, uruchom program Visual Studio. Spowoduje to otwarcie strony początkowej:

  ![](quickstart-images/vs/start-page.png "Visual Studio")

1. W programie Visual Studio, kliknij przycisk **Tworzenie nowego projektu...**  do utworzenia nowego projektu:

  ![](quickstart-images/vs/new-solution.png "Nowy projekt")

1. W **nowy projekt** okna dialogowego, kliknij przycisk **wieloplatformowych**, wybierz pozycję **Cross Platform aplikacji (platformy Xamarin.Forms)** szablonu, ustaw nazwę nazwy i rozwiązanie `Phoneword`, Wybierz odpowiednią lokalizację projektu i kliknij przycisk **OK** przycisk:

  ![](quickstart-images/vs/new-project.png "Szablony projektów i Platform")

1. W **nowej aplikacji dla wielu Platform** okna dialogowego, kliknij przycisk **pusta aplikacja**, wybierz pozycję **platformy Xamarin.Forms** technologii interfejsu użytkownika, zaznacz **.NET Standard** jako Strategii udostępniania kodu, a następnie kliknij przycisk **OK** przycisk:

  ![](quickstart-images/vs/new-app.png "Nowa aplikacja i Platform")

1. W **Eksploratora rozwiązań**w **Phoneword** projektu, kliknij dwukrotnie **MainPage.xaml** go otworzyć:

  ![](quickstart-images/vs/open-mainpage-xaml.png "Otwórz MainPage.xaml")

1. W **MainPage.xaml**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod definiuje deklaratywnie interfejsu użytkownika dla strony:

        <?xml version="1.0" encoding="UTF-8"?>
        <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           x:Class="Phoneword.MainPage">
            <ContentPage.Padding>
                <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS" Value="20, 40, 20, 20" />
                    <On Platform="Android, WinPhone, Windows" Value="20" />
                </OnPlatform>
            </ContentPage.Padding>
            <StackLayout>
              <Label Text="Enter a Phoneword:" />
              <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
              <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
              <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
            </StackLayout>
        </ContentPage>

  Zapisać zmiany w **MainPage.xaml** naciskając **CTRL + S**i zamknij plik.

1. W **Eksploratora rozwiązań**, rozwiń węzeł **MainPage.xaml** i kliknij dwukrotnie **MainPage.xaml.cs** go otworzyć:

  ![](quickstart-images/vs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

1. W **MainPage.xaml.cs**, usuń cały kod szablonu i zastąp go następującym kodem. `OnTranslate` i `OnCall` metod, które będą wykonywane w odpowiedzi na **Przetłumacz** i **wywołać** przycisków w momencie kliknięcia odpowiednio w interfejsie użytkownika:

        using System;
        using Xamarin.Forms;

        namespace Phoneword
        {
            public partial class MainPage : ContentPage
            {
                string translatedNumber;

                public MainPage ()
                {
                    InitializeComponent ();
                }

                void OnTranslate (object sender, EventArgs e)
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

                async void OnCall (object sender, EventArgs e)
                {
                    if (await this.DisplayAlert (
                            "Dial a Number",
                            "Would you like to call " + translatedNumber + "?",
                            "Yes",
                            "No")) {
                        var dialer = DependencyService.Get<IDialer> ();
                        if (dialer != null)
                            dialer.Dial (translatedNumber);
                    }
                }
            }
        }

  > [!NOTE]
> **Uwaga**: próba skompilowania aplikacji, w tym momencie będą powodować błędy, które zostanie rozwiązany później.

  Zapisać zmiany w **MainPage.xaml.cs** naciskając **CTRL + S**i zamknij plik.

1. W **Eksploratora rozwiązań**, rozwiń węzeł **App.xaml** i kliknij dwukrotnie **App.xaml.cs** go otworzyć:

  ![](quickstart-images/vs/open-app-class.png "Open App.xaml.cs")

1. W **App.xaml.cs**, usuń cały kod szablonu i zastąp go następującym kodem. `App` Ustawia konstruktora `MainPage` klasy jako strony, który będzie wyświetlany podczas uruchamiania aplikacji:

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

                protected override void OnStart()
                {
                    // Handle when your app starts
                }

                protected override void OnSleep()
                {
                    // Handle when your app sleeps
                }

                protected override void OnResume()
                {
                    // Handle when your app resumes
                }
            }
        }

  Zapisać zmiany w **App.xaml.cs** naciskając **CTRL + S**i zamknij plik.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword** projekt i wybierz **Dodaj > Nowy element...** :

  ![](quickstart-images/vs/add-new-item.png "Dodaj nowy element")

1. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Kod > klasy**, Nazwa nowego pliku **PhoneTranslator**i kliknij przycisk **Dodaj** przycisku :

  ![](quickstart-images/vs/add-translator-class.png "Dodaj nową klasę")

1. W **PhoneTranslator.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod będzie tłumaczenie wyrazu phone na numer telefonu:

        using System.Text;

        namespace Core
        {
            public static class PhonewordTranslator
            {
                public static string ToNumber(string raw)
                {
                    if (string.IsNullOrWhiteSpace(raw))
                        return null;

                    raw = raw.ToUpperInvariant();

                    var newNumber = new StringBuilder();
                    foreach (var c in raw)
                    {
                        if (" -0123456789".Contains(c))
                            newNumber.Append(c);
                        else
                        {
                            var result = TranslateToNumber(c);
                            if (result != null)
                                newNumber.Append(result);
                            // Bad character?
                            else
                                return null;
                        }
                    }
                    return newNumber.ToString();
                }

                static bool Contains(this string keyString, char c)
                {
                    return keyString.IndexOf(c) >= 0;
                }

                static readonly string[] digits = {
                    "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
                };

                static int? TranslateToNumber(char c)
                {
                    for (int i = 0; i < digits.Length; i++)
                    {
                        if (digits[i].Contains(c))
                            return 2 + i;
                    }
                    return null;
                }
            }
        }

  Zapisać zmiany w **PhoneTranslator.cs** naciskając **CTRL + S**i zamknij plik.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword** projekt i wybierz **Dodaj > Nowy element...** :

  ![](quickstart-images/vs/add-new-item.png "Dodaj nowy element")

1. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Kod > interfejs**, Nazwa nowego pliku **IDialer**i kliknij przycisk **Dodaj** przycisk:

  ![](quickstart-images/vs/add-idialer-interface.png "Dodaj nowy interfejs")

1. W **IDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod definiuje `Dial` metodę, która musi zostać wdrożone na każdej z platform wybierz numer telefonu przetłumaczonego:

        namespace Phoneword
        {
            public interface IDialer
            {
                bool Dial(string number);
            }
        }

  Zapisać zmiany w **IDialer.cs** naciskając **CTRL + S**i zamknij plik.

  > [!NOTE]
> Typowy kod aplikacji jest teraz ukończona. Specyficzne dla platformy phone telefon kod będzie teraz zaimplementowana jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.iOS** projekt i wybierz **Dodaj > Nowy element...** :

  ![](quickstart-images/vs/add-new-item-ios.png "Dodaj nowy element")

1. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Apple > Kod > klasy**, Nazwa nowego pliku **PhoneDialer.Document**i kliknij przycisk **Dodaj** przycisk:

  ![](quickstart-images/vs/new-phone-dialer-ios.png "Dodaj nową klasę")

1. W **PhoneDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod tworzy <code>Dial</code> metoda, która będzie używana na platformie iOS wybierania numeru telefonu przetłumaczonego:

        using Foundation;
        using Phoneword.iOS;
        using UIKit;
        using Xamarin.Forms;

        [assembly: Dependency(typeof(PhoneDialer))]
        namespace Phoneword.iOS
        {
            public class PhoneDialer : IDialer
            {
                public bool Dial(string number)
                {
                    return UIApplication.SharedApplication.OpenUrl (
                        new NSUrl ("tel:" + number));
                }
            }
        }

  Zapisać zmiany w **PhoneDialer.cs** naciskając **CTRL + S**i zamknij plik.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.Android** projekt i wybierz **Dodaj > Nowy element...** :

  ![](quickstart-images/vs/add-new-item-android.png "Dodaj nowy element")

1. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Android > klasy**, Nazwa nowego pliku **PhoneDialer.Document**i kliknij przycisk **Dodaj** przycisk:

  ![](quickstart-images/vs/new-phone-dialer-android.png "Dodaj nową klasę")

1. W **PhoneDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod tworzy `Dial` metoda, która będzie używana na platformie Android do wybierania numeru telefonu przetłumaczonego:

        using Android.Content;
        using Android.Telephony;
        using Phoneword.Droid;
        using System.Linq;
        using Xamarin.Forms;
        using Uri = Android.Net.Uri;

        [assembly: Dependency(typeof(PhoneDialer))]
        namespace Phoneword.Droid
        {
            public class PhoneDialer : IDialer
            {
                public bool Dial(string number)
                {
                    var context = MainActivity.Instance;
                    if (context == null)
                        return false;

                    var intent = new Intent (Intent.ActionCall);
                    intent.SetData (Uri.Parse ("tel:" + number));

                    if (IsIntentAvailable (context, intent)) {
                        context.StartActivity (intent);
                        return true;
                    }

                    return false;
                }

                public static bool IsIntentAvailable(Context context, Intent intent)
                {
                    var packageManager = context.PackageManager;

                    var list = packageManager.QueryIntentServices (intent, 0)
                        .Union (packageManager.QueryIntentActivities (intent, 0));

                    if (list.Any ())
                        return true;

                    var manager = TelephonyManager.FromContext (context);
                    return manager.PhoneType != PhoneType.None;
                }
            }
        }

  Zapisać zmiany w **PhoneDialer.cs** naciskając **CTRL + S**i zamknij plik.

1. W **Eksploratora rozwiązań**w **Phoneword.Android** projektu, kliknij dwukrotnie **MainActivity.cs** go otworzyć, usuń cały kod szablonu i zastąpić go ciągiem następujący kod:

        using Android.App;
        using Android.Content.PM;
        using Android.OS;

        namespace Phoneword.Droid
        {
            [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
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

  Zapisać zmiany w **MainActivity.cs** naciskając **CTRL + S**i zamknij plik.

1. W **Eksploratora rozwiązań**w **Phoneword.Android** projektu, kliknij dwukrotnie **właściwości** i wybierz **manifestu systemu Android** karty:

  ![](quickstart-images/vs/android-manifest.png "Otwórz manifestu systemu Android")

1. W **wymagane uprawnienia** Włącz opcję **CALL_PHONE** uprawnienia. Dzięki temu aplikacji uprawnienia do telefonicznie:

  ![](quickstart-images/vs/android-manifest-changed.png "Włącz uprawnień CallPhone")

  Zapisz zmiany w manifeście, naciskając klawisz **CTRL + S**i zamknij plik.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.UWP** projekt i wybierz **Dodaj > Nowy element...** :

  ![](quickstart-images/vs/add-new-item-uwp.png "Dodaj nowy element")

1. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Kod > klasy**, Nazwa nowego pliku **PhoneDialer.Document**i kliknij przycisk **Dodaj** przycisk:

  ![](quickstart-images/vs/new-phone-dialer-uwp.png "Dodaj nową klasę")

1. W **PhoneDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod tworzy `Dial` — metoda i metody pomocnicze, które będą używane na platformy uniwersalnej systemu Windows do wybierania numeru telefonu przetłumaczonego:

        using Phoneword.UWP;
        using System;
        using System.Threading.Tasks;
        using Windows.ApplicationModel.Calls;
        using Windows.UI.Popups;
        using Xamarin.Forms;

        [assembly: Dependency(typeof(PhoneDialer))]
        namespace Phoneword.UWP
        {
            public class PhoneDialer : IDialer
            {
                bool dialled = false;

                public bool Dial(string number)
                {
                    DialNumber(number);
                    return dialled;
                }

                async Task DialNumber(string number)
                {
                    var phoneLine = await GetDefaultPhoneLineAsync();
                    if (phoneLine != null)
                    {
                        phoneLine.Dial(number, number);
                        dialled = true;
                    }
                    else
                    {
                        var dialog = new MessageDialog("No line found to place the call");
                        await dialog.ShowAsync();
                        dialled = false;
                    }
                }

                async Task<PhoneLine> GetDefaultPhoneLineAsync()
                {
                    var phoneCallStore = await PhoneCallManager.RequestStoreAsync();
                    var lineId = await phoneCallStore.GetDefaultLineAsync();
                    return await PhoneLine.FromIdAsync(lineId);
                }
            }
        }

  Zapisać zmiany w **PhoneDialer.cs** naciskając **CTRL + S**i zamknij plik.

1. W **Eksploratora rozwiązań**w **Phoneword.UWP** projektu, kliknij prawym przyciskiem myszy **odwołania**i wybierz **Dodawanie odwołania...** :

  ![](quickstart-images/vs/uwp-add-reference.png "Dodaj odwołanie")

1. W **Menedżera odwołań** okno dialogowe, wybierz opcję **uniwersalnych systemu Windows > Rozszerzenia > systemu Windows Mobile rozszerzeń dla platformy uniwersalnej systemu Windows**i kliknij przycisk **OK** przycisk:

  ![](quickstart-images/vs/uwp-add-reference-extensions.png "Dodawanie rozszerzeń przenośnych systemu Windows dla platformy uniwersalnej systemu Windows")

1. W **Eksploratora rozwiązań**w **Phoneword.UWP** projektu, kliknij dwukrotnie **Package.appxmanifest**:

  ![](quickstart-images/vs/uwp-manifest.png "Otwórz plik manifestu platformy uniwersalnej systemu Windows")

1. W **możliwości** pozycję Włącz **rozmowy telefonicznej** możliwości. Dzięki temu aplikacji uprawnienia do telefonicznie:

  ![](quickstart-images/vs/uwp-manifest-changed.png "Włącz możliwość połączenia telefonicznego")

  Zapisz zmiany w manifeście, naciskając klawisz **CTRL + S**i zamknij plik.

1. W programie Visual Studio, wybierz **kompilacji > Kompiluj rozwiązanie** elementu menu (lub naciśnij klawisz **CTRL + SHIFT + B**). Aplikacja zostanie utworzona i zostanie wyświetlony komunikat Powodzenie, na pasku stanu programu Visual Studio:

  ![](quickstart-images/vs/build-successful.png "Pomyślnie kompilacji")

  Jeśli wystąpią błędy, powtórz poprzednie kroki i usuń ewentualne błędy, dopóki aplikacja tworzy się pomyślnie.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.UWP** projekt i wybierz **Ustaw jako projekt startowy**:

  ![](quickstart-images/vs/uwp-set-as-startup-project.png "Ustaw jako projekt startowy")

1. Na pasku narzędzi programu Visual Studio, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Odtwórz) do uruchamiania aplikacji:

  ![](quickstart-images/vs/start.png "Pasek narzędzi Visual Studio")
  ![](quickstart-images/vs/phone-result-uwp.png "Phoneword aplikacji platformy uniwersalnej systemu Windows")

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.Android** projekt i wybierz **Ustaw jako projekt startowy**.
1. Na pasku narzędzi programu Visual Studio, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Play), aby uruchomić aplikację w emulatorze systemu Android.
1. Jeśli masz urządzenie z systemem iOS i spełniać wymagania systemowe Mac do tworzenia aplikacji platformy Xamarin.Forms, użyj technika podobne do wdrożenia aplikacji na urządzeniu z systemem iOS. Można również wdrożyć aplikację, aby [zdalnego symulatora systemu iOS](~/tools/ios-simulator.md).

  Uwaga: połączenia telefoniczne nie są obsługiwane na wszystkich symulatorów.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom program Visual Studio dla komputerów Mac, a na stronie początkowej kliknij **nowy projekt...**  do utworzenia nowego projektu:

  ![](quickstart-images/xs/new-solution.png "Nowe rozwiązanie")

1. W **wybierz szablon dla nowego projektu** okna dialogowego, kliknij przycisk **Multiplatform > aplikacji**, wybierz pozycję **pusta aplikacja formularzy** szablonu, a następnie kliknij przycisk **dalej** przycisk:

  ![](quickstart-images/xs/choose-template.png "Wybieranie szablonu")

1. W **skonfigurować aplikację formularzy** okna dialogowego, nazwę nowej aplikacji `Phoneword`, upewnij się, że **Użyj przenośnej biblioteki klas** przycisk radiowy zostanie wybrany, upewnij się, że **Użyj XAML dla użytkownika Interfejs pliki** pole wyboru jest zaznaczone, a następnie kliknij przycisk **dalej** przycisk:

  ![](quickstart-images/xs/configure-app.png "Konfigurowanie aplikacji formularzy")

1. W **Konfigurowanie nowej aplikacji formularzy** okno dialogowe, pozostaw ustawioną nazwy rozwiązania i projektu `Phoneword`, wybierz odpowiednią lokalizację projektu i kliknij przycisk **Utwórz** przycisk, aby utworzyć Projekt:

  ![](quickstart-images/xs/configure-project.png "Konfigurowanie projektu formularzy")

1. W **konsoli rozwiązania**, wybierz pozycję **Phoneword** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

  ![](quickstart-images/xs/add-new-file.png "Dodaj nowy plik")

1. W **nowy plik** okno dialogowe, wybierz **Formularze > Xaml wartość ContentPage formularze**, Nazwa nowego pliku **MainPage**i kliknij przycisk **nowy** przycisku. Spowoduje to dodanie stronę o nazwie **MainPage** do projektu:

  ![](quickstart-images/xs/add-mainpage-class.png "Dodaj nowy wartość ContentPage")

1. W **konsoli rozwiązania**, kliknij dwukrotnie **MainPage.xaml** go otworzyć:

  ![](quickstart-images/xs/open-mainpage-xaml.png "Otwórz MainPage.xaml")

1. W **MainPage.xaml**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod definiuje deklaratywnie interfejsu użytkownika dla strony:

        <?xml version="1.0" encoding="UTF-8"?>
        <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           x:Class="Phoneword.MainPage">
            <ContentPage.Padding>
                <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS" Value="20, 40, 20, 20" />
                    <On Platform="Android, WinPhone, Windows" Value="20" />
                </OnPlatform>
            </ContentPage.Padding>
            <StackLayout>
              <Label Text="Enter a Phoneword:" />
              <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
              <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
              <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
            </StackLayout>
        </ContentPage>

  Zapisać zmiany w **MainPage.xaml** , wybierając **Plik > Zapisz** (lub naciskając klawisz **&#8984; + S**) i zamknij plik.

1. W **konsoli rozwiązania**, kliknij dwukrotnie **MainPage.xaml.cs** go otworzyć:

  ![](quickstart-images/xs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

1. W **MainPage.xaml.cs**, usuń cały kod szablonu i zastąp go następującym kodem. `OnTranslate` i `OnCall` metod, które będą wykonywane w odpowiedzi na **Przetłumacz** i **wywołać** przycisków w momencie kliknięcia odpowiednio w interfejsie użytkownika:

        using System;
        using Xamarin.Forms;

        namespace Phoneword
        {
            public partial class MainPage : ContentPage
            {
                string translatedNumber;

                public MainPage ()
                {
                    InitializeComponent ();
                }

                void OnTranslate (object sender, EventArgs e)
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

                async void OnCall (object sender, EventArgs e)
                {
                    if (await this.DisplayAlert (
                            "Dial a Number",
                            "Would you like to call " + translatedNumber + "?",
                            "Yes",
                            "No")) {
                        var dialer = DependencyService.Get<IDialer> ();
                        if (dialer != null)
                            dialer.Dial (translatedNumber);
                    }
                }
            }
        }

  > [!NOTE]
> **Uwaga**: próba skompilowania aplikacji, w tym momencie będą powodować błędy, które zostanie rozwiązany później.

  Zapisać zmiany w **MainPage.xaml.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz **&#8984; + S**) i zamknij plik.

1. W **konsoli rozwiązania**, kliknij dwukrotnie **App.xaml.cs** go otworzyć:

  ![](quickstart-images/xs/open-app-class.png "Open App.xaml.cs")

1. W **App.xaml.cs**, usuń cały kod szablonu i zastąp go następującym kodem. `App` Ustawia konstruktora `MainPage` klasy jako strony, który będzie wyświetlany podczas uruchamiania aplikacji:

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

                protected override void OnStart()
                {
                    // Handle when your app starts
                }

                protected override void OnSleep()
                {
                    // Handle when your app sleeps
                }

                protected override void OnResume()
                {
                    // Handle when your app resumes
                }
            }
        }

  Zapisać zmiany w **Phoneword.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz **&#8984; + S**) i zamknij plik.

1. W **konsoli rozwiązania**, wybierz pozycję **Phoneword** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

  ![](quickstart-images/xs/add-new-translator-file.png "Dodaj nowy plik")

1. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > pustą klasę**, podaj nazwę nowego pliku **PhoneTranslator**i kliknij przycisk **nowy** przycisk:

  ![](quickstart-images/xs/add-translator-class.png "Dodaj nową klasę")

1. W **PhoneTranslator.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod będzie tłumaczenie wyrazu phone na numer telefonu:

        using System.Text;

        namespace Core
        {
            public static class PhonewordTranslator
            {
                public static string ToNumber(string raw)
                {
                    if (string.IsNullOrWhiteSpace(raw))
                        return null;

                    raw = raw.ToUpperInvariant();

                    var newNumber = new StringBuilder();
                    foreach (var c in raw)
                    {
                        if (" -0123456789".Contains(c))
                            newNumber.Append(c);
                        else
                        {
                            var result = TranslateToNumber(c);
                            if (result != null)
                                newNumber.Append(result);
                            // Bad character?
                            else
                                return null;
                        }
                    }
                    return newNumber.ToString();
                }

                static bool Contains(this string keyString, char c)
                {
                    return keyString.IndexOf(c) >= 0;
                }

                static readonly string[] digits = {
                    "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
                };

                static int? TranslateToNumber(char c)
                {
                    for (int i = 0; i < digits.Length; i++)
                    {
                        if (digits[i].Contains(c))
                            return 2 + i;
                    }
                    return null;
                }
            }
        }

  Zapisać zmiany w **PhoneTranslator.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz **&#8984; + S**) i zamknij plik.

1. W **konsoli rozwiązania**, wybierz pozycję **Phoneword** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

  ![](quickstart-images/xs/add-new-interface.png "Dodaj nowy plik")

1. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > pustego interfejsu**, podaj nazwę nowego pliku **IDialer**i kliknij przycisk **nowy** przycisk:

  ![](quickstart-images/xs/add-idialer-interface.png "Dodaj nowy interfejs")

1. W **IDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod definiuje `Dial` metodę, która musi zostać wdrożone na każdej z platform wybierz numer telefonu przetłumaczonego:

        namespace Phoneword
        {
            public interface IDialer
            {
                bool Dial(string number);
            }
        }

  Zapisać zmiany w **IDialer.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz **&#8984; + S**) i zamknij plik.

  > [!NOTE]
> Typowy kod aplikacji jest teraz ukończona. Specyficzne dla platformy phone telefon kod będzie teraz zaimplementowana jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

1. W **konsoli rozwiązania**, wybierz pozycję **Phoneword.iOS** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

  ![](quickstart-images/xs/add-new-file-ios.png "Dodaj nowy plik")

1. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > pustą klasę**, podaj nazwę nowego pliku **PhoneDialer.Document**i kliknij przycisk **nowy** przycisk:

  ![](quickstart-images/xs/new-phonedialer-ios.png "Dodaj nową klasę")

1. W **PhoneDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod tworzy `Dial` metoda, która będzie używana na platformie iOS wybierania numeru telefonu przetłumaczonego:

        using Foundation;
        using Phoneword.iOS;
        using UIKit;
        using Xamarin.Forms;

        [assembly: Dependency(typeof(PhoneDialer))]
        namespace Phoneword.iOS
        {
            public class PhoneDialer : IDialer
            {
                public bool Dial(string number)
                {
                    return UIApplication.SharedApplication.OpenUrl (
                        new NSUrl ("tel:" + number));
                }
            }
        }

  Zapisać zmiany w **PhoneDialer.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz **&#8984; + S**) i zamknij plik.

1. W **konsoli rozwiązania**, wybierz pozycję **Phoneword.Droid** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

  ![](quickstart-images/xs/add-new-file-android.png "Dodaj nowy plik")

1. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > pustą klasę**, podaj nazwę nowego pliku **PhoneDialer.Document**i kliknij przycisk **nowy** przycisk:

  ![](quickstart-images/xs/new-phonedialer-android.png "Dodaj nową klasę")

1. W **PhoneDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod tworzy `Dial` metoda, która będzie używana na platformie Android do wybierania numeru telefonu przetłumaczonego:

        using Android.Content;
        using Android.Telephony;
        using Phoneword.Droid;
        using System.Linq;
        using Xamarin.Forms;
        using Uri = Android.Net.Uri;

        [assembly: Dependency(typeof(PhoneDialer))]
        namespace Phoneword.Droid
        {
            public class PhoneDialer : IDialer
            {
                public bool Dial(string number)
                {
                    var context = MainActivity.Instance;
                    if (context == null)
                        return false;

                    var intent = new Intent (Intent.ActionCall);
                    intent.SetData (Uri.Parse ("tel:" + number));

                    if (IsIntentAvailable (context, intent)) {
                        context.StartActivity (intent);
                        return true;
                    }

                    return false;
                }

                public static bool IsIntentAvailable(Context context, Intent intent)
                {
                    var packageManager = context.PackageManager;

                    var list = packageManager.QueryIntentServices (intent, 0)
                        .Union (packageManager.QueryIntentActivities (intent, 0));

                    if (list.Any ())
                        return true;

                    var manager = TelephonyManager.FromContext (context);
                    return manager.PhoneType != PhoneType.None;
                }
            }
        }

  Zapisać zmiany w **PhoneDialer.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz **&#8984; + S**) i zamknij plik.

1. W **konsoli rozwiązania**w **Phoneword.Droid** projektu, kliknij dwukrotnie **MainActivity.cs** go otworzyć, Usuń wszystkie kod szablonu i zastąp go następującym kodem Kod:

        using Android.App;
        using Android.Content.PM;
        using Android.OS;

        namespace Phoneword.Droid
        {
            [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
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

  Zapisać zmiany w **MainActivity.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz **&#8984; + S**) i zamknij plik.

1. W **konsoli rozwiązania**, rozwiń węzeł **właściwości** folder i kliknij dwukrotnie **AndroidManifest.xml** pliku:

  ![](quickstart-images/xs/android-manifest.png "Otwórz manifestu systemu Android")

1. W **wymagane uprawnienia** Włącz opcję **CallPhone** uprawnienia. Dzięki temu aplikacji uprawnienia do telefonicznie:

  ![](quickstart-images/xs/android-manifest-changed.png "Włącz uprawnień CallPhone")

  Zapisać zmiany w **AndroidManifest.xml** , wybierając **Plik > Zapisz** (lub naciskając klawisz **&#8984; + S**) i zamknij plik.

1. W **konsoli rozwiązania**, Usuń **PhonewordPage** klasę z **Phoneword** projektu. Ta strona została dodana automatycznie, gdy projekt został utworzony i nie jest już wymagane.
1. W programie Visual Studio dla komputerów Mac, wybierz **kompilacji > kompilacji wszystkich** elementu menu (lub naciśnij klawisz **&#8984; + B**). Aplikacja zostanie utworzona i komunikat informujący będą wyświetlane w Visual Studio for Mac paska narzędzi.

  ![](quickstart-images/xs/build-successful.png "Pomyślnie kompilacji")

1. Jeśli wystąpią błędy, powtórz poprzednie kroki i usuń ewentualne błędy, dopóki aplikacja tworzy się pomyślnie.
1. W programie Visual Studio dla komputerów Mac narzędzi, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Odtwórz) do uruchamiania aplikacji w narzędziu iOS Simulator:

  ![](quickstart-images/xs/start.png "Programu Visual Studio for Mac narzędzi")
  ![](quickstart-images/xs/phoneword-result-ios.png "symulatora systemu iOS")

  Uwaga: połączenia telefoniczne nie są obsługiwane w narzędziu iOS Simulator.

1. W **konsoli rozwiązania**, wybierz pozycję **Phoneword.Droid** projektu, kliknij prawym przyciskiem myszy i wybierz **Ustaw jako projekt startowy**:

  ![](quickstart-images/xs/set-startup-project.png "Ustaw jako projekt startowy")

1. W programie Visual Studio dla komputerów Mac narzędzi, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Play), aby uruchomić aplikację w emulatorze systemu Android:

  ![](quickstart-images/xs/phoneword-result-android.png "Emulator systemu android")

  Uwaga: połączenia telefoniczne nie są obsługiwane w systemie Android emulatorów.

-----

Gratulujemy kończenie aplikacji platformy Xamarin.Forms. [Następnego tematu](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md) w tym przewodniku monitoruje czynności wykonane w tym przewodniku w celu uzyskania informacji na temat podstawowe informacje na temat tworzenia aplikacji za pomocą platformy Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [Uzyskiwanie dostępu do natywnego funkcji za pośrednictwem DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
