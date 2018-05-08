---
title: Szybki Start platformy Xamarin.Forms
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2018
ms.openlocfilehash: 6a0107ec11955f99c62a6f59f9bf82291dee9224
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinforms-quickstart"></a>Szybki Start platformy Xamarin.Forms

W tym przewodniku pokazano, jak utworzyć aplikację konwertujący alfanumeryczne numer wprowadzony przez użytkownika do numeru telefonu liczbowych i kod, który odwołuje. Końcowe aplikacji jest pokazany poniżej:

[![](quickstart-images/intro-app-examples-sml.png "Aplikacja Phoneword")](quickstart-images/intro-app-examples.png#lightbox "Phoneword aplikacji")

Utwórz aplikację Phoneword w następujący sposób:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. W **Start** ekranu, uruchom program Visual Studio. Spowoduje to otwarcie strony początkowej:

    ![](quickstart-images/vs/start-page.png "Visual Studio")

2. W programie Visual Studio, kliknij przycisk **Tworzenie nowego projektu...**  do utworzenia nowego projektu:

    ![](quickstart-images/vs/new-solution.png "Nowy projekt")

3. W **nowy projekt** okna dialogowego, kliknij przycisk **wieloplatformowych**, wybierz pozycję **aplikacji mobilnej (platformy Xamarin.Forms)** szablonu, ustaw nazwę nazwy i rozwiązanie `Phoneword`, wybierz odpowiednią lokalizację projektu i kliknij przycisk **OK** przycisk:

    ![](quickstart-images/vs/new-project.w157.png "Szablony projektów i Platform")

4. W **nowej aplikacji dla wielu Platform** okna dialogowego, kliknij przycisk **pusta aplikacja**, wybierz pozycję **.NET Standard** strategii udostępniania kodu, a następnie kliknij przycisk **OK** przycisk:

    ![](quickstart-images/vs/new-app.png "Nowa aplikacja i Platform")

5. W **Eksploratora rozwiązań**w **Phoneword** projektu, kliknij dwukrotnie **MainPage.xaml** go otworzyć:

    ![](quickstart-images/vs/open-mainpage-xaml.png "Otwórz MainPage.xaml")

6. W **MainPage.xaml**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod definiuje deklaratywnie interfejsu użytkownika dla strony:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    Zapisać zmiany w **MainPage.xaml** naciskając **CTRL + S**i zamknij plik.

7. W **Eksploratora rozwiązań**, rozwiń węzeł **MainPage.xaml** i kliknij dwukrotnie **MainPage.xaml.cs** go otworzyć:

    ![](quickstart-images/vs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

8. W **MainPage.xaml.cs**, usuń cały kod szablonu i zastąp go następującym kodem. `OnTranslate` i `OnCall` metod, które będą wykonywane w odpowiedzi na **Przetłumacz** i **wywołać** przycisków w momencie kliknięcia odpowiednio w interfejsie użytkownika:

    ```csharp
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
    ```

    > [!NOTE]
    > Próba skompilowania aplikacji, w tym momencie będą powodować błędy, które zostanie rozwiązany później.

    Zapisać zmiany w **MainPage.xaml.cs** naciskając **CTRL + S**i zamknij plik.

9. W **Eksploratora rozwiązań**, rozwiń węzeł **App.xaml** i kliknij dwukrotnie **App.xaml.cs** go otworzyć:

    ![](quickstart-images/vs/open-app-class.png "Otwórz App.xaml.cs")

10. W **App.xaml.cs**, usuń cały kod szablonu i zastąp go następującym kodem. `App` Ustawia konstruktora `MainPage` klasy jako strony, który będzie wyświetlany podczas uruchamiania aplikacji:

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
    ```

    Zapisać zmiany w **App.xaml.cs** naciskając **CTRL + S**i zamknij plik.

11. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword** projekt i wybierz **Dodaj > Nowy element...** :

    ![](quickstart-images/vs/add-new-item.png "Dodaj nowy element")

12. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Kod > klasy**, Nazwa nowego pliku **PhoneTranslator**i kliknij przycisk **Dodaj** przycisku :

    ![](quickstart-images/vs/add-translator-class.w157.png "Dodaj nową klasę")

13. W **PhoneTranslator.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod będzie tłumaczenie wyrazu phone na numer telefonu:

    ```csharp
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
    ```

    Zapisać zmiany w **PhoneTranslator.cs** naciskając **CTRL + S**i zamknij plik.

14. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword** projekt i wybierz **Dodaj > Nowy element...** :

    ![](quickstart-images/vs/add-new-item.png "Dodaj nowy element")

15. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Kod > interfejs**, Nazwa nowego pliku **IDialer**i kliknij przycisk **Dodaj** przycisk:

    ![](quickstart-images/vs/add-idialer-interface.w157.png "Dodaj nowy interfejs")

16. W **IDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod definiuje `Dial` metodę, która musi zostać wdrożone na każdej z platform wybierz numer telefonu przetłumaczonego:

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```

    Zapisać zmiany w **IDialer.cs** naciskając **CTRL + S**i zamknij plik.

    > [!NOTE]
    > Typowy kod aplikacji jest teraz ukończona. Specyficzne dla platformy phone telefon kod będzie teraz zaimplementowana jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

17. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.iOS** projekt i wybierz **Dodaj > Nowy element...** :

    ![](quickstart-images/vs/add-new-item-ios.png "Dodaj nowy element")

18. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Kod > klasy**, Nazwa nowego pliku **PhoneDialer.Document**i kliknij przycisk **Dodaj** przycisk:

    ![](quickstart-images/vs/new-phone-dialer-ios.w157.png "Dodaj nową klasę")

19. W **PhoneDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod tworzy <code>Dial</code> metoda, która będzie używana na platformie iOS wybierania numeru telefonu przetłumaczonego:

    ```csharp
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
    ```

    Zapisać zmiany w **PhoneDialer.cs** naciskając **CTRL + S**i zamknij plik.

20. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.Android** projekt i wybierz **Dodaj > Nowy element...** :

    ![](quickstart-images/vs/add-new-item-android.png "Dodaj nowy element")

21. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Android > klasy**, Nazwa nowego pliku **PhoneDialer.Document**i kliknij przycisk **Dodaj** przycisk:

    ![](quickstart-images/vs/new-phone-dialer-android.w157.png "Dodaj nową klasę")

22. W **PhoneDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod tworzy `Dial` metoda, która będzie używana na platformie Android do wybierania numeru telefonu przetłumaczonego:

    ```csharp
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
    ```

    Zapisać zmiany w **PhoneDialer.cs** naciskając **CTRL + S**i zamknij plik.

23. W **Eksploratora rozwiązań**w **Phoneword.Android** projektu, kliknij dwukrotnie **MainActivity.cs** go otworzyć, usuń cały kod szablonu i zastąpić go ciągiem następujący kod:

    ```csharp
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
    ```

    Zapisać zmiany w **MainActivity.cs** naciskając **CTRL + S**i zamknij plik.

24. W **Eksploratora rozwiązań**w **Phoneword.Android** projektu, kliknij dwukrotnie **właściwości** i wybierz **manifestu systemu Android** karty:

    ![](quickstart-images/vs/android-manifest.png "Otwórz manifestu systemu Android")

25. W **wymagane uprawnienia** Włącz opcję **CALL_PHONE** uprawnienia. Dzięki temu aplikacji uprawnienia do telefonicznie:

    ![](quickstart-images/vs/android-manifest-changed.png "Włącz uprawnień CallPhone")

    Zapisz zmiany w manifeście, naciskając klawisz **CTRL + S**i zamknij plik.

26. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.UWP** projekt i wybierz **Dodaj > Nowy element...** :

    ![](quickstart-images/vs/add-new-item-uwp.png "Dodaj nowy element")

27. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Kod > klasy**, Nazwa nowego pliku **PhoneDialer.Document**i kliknij przycisk **Dodaj** przycisk:

    ![](quickstart-images/vs/new-phone-dialer-uwp.w157.png "Dodaj nową klasę")

28. W **PhoneDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod tworzy `Dial` — metoda i metody pomocnicze, które będą używane na platformy uniwersalnej systemu Windows do wybierania numeru telefonu przetłumaczonego:

    ```csharp
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
    ```

    Zapisać zmiany w **PhoneDialer.cs** naciskając **CTRL + S**i zamknij plik.

29. W **Eksploratora rozwiązań**w **Phoneword.UWP** projektu, kliknij prawym przyciskiem myszy **odwołania**i wybierz **Dodawanie odwołania...** :

    ![](quickstart-images/vs/uwp-add-reference.png "Dodaj odwołanie")

30. W **Menedżera odwołań** okno dialogowe, wybierz opcję **uniwersalnych systemu Windows > Rozszerzenia > systemu Windows Mobile rozszerzeń dla platformy uniwersalnej systemu Windows**i kliknij przycisk **OK** przycisk:

    ![](quickstart-images/vs/uwp-add-reference-extensions.png "Dodawanie rozszerzeń przenośnych systemu Windows dla platformy uniwersalnej systemu Windows")

31. W **Eksploratora rozwiązań**w **Phoneword.UWP** projektu, kliknij dwukrotnie **Package.appxmanifest**:

    ![](quickstart-images/vs/uwp-manifest.png "Otwórz plik manifestu platformy uniwersalnej systemu Windows")

31. W **możliwości** pozycję Włącz **rozmowy telefonicznej** możliwości. Dzięki temu aplikacji uprawnienia do telefonicznie:

    ![](quickstart-images/vs/uwp-manifest-changed.png "Włącz możliwość połączenia telefonicznego")

    Zapisz zmiany w manifeście, naciskając klawisz **CTRL + S**i zamknij plik.

32. W programie Visual Studio, wybierz **kompilacji > Kompiluj rozwiązanie** elementu menu (lub naciśnij klawisz **CTRL + SHIFT + B**). Aplikacja zostanie utworzona i zostanie wyświetlony komunikat Powodzenie, na pasku stanu programu Visual Studio:

    ![](quickstart-images/vs/build-successful.png "Pomyślnie kompilacji")

    Jeśli wystąpią błędy, powtórz poprzednie kroki i usuń ewentualne błędy, dopóki aplikacja tworzy się pomyślnie.

33. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.UWP** projekt i wybierz **Ustaw jako projekt startowy**:

    ![](quickstart-images/vs/uwp-set-as-startup-project.png "Ustaw jako projekt startowy")

34. Na pasku narzędzi programu Visual Studio, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Odtwórz) do uruchamiania aplikacji:

    ![](quickstart-images/vs/start.png "Pasek narzędzi Visual Studio")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword aplikacji platformy uniwersalnej systemu Windows")

35. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.Android** projekt i wybierz **Ustaw jako projekt startowy**.
36. Na pasku narzędzi programu Visual Studio, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Play), aby uruchomić aplikację w emulatorze systemu Android.
37. Jeśli masz urządzenie z systemem iOS i spełniać wymagania systemowe Mac do tworzenia aplikacji platformy Xamarin.Forms, użyj technika podobne do wdrożenia aplikacji na urządzeniu z systemem iOS. Można również wdrożyć aplikację, aby [zdalnego symulatora systemu iOS](~/tools/ios-simulator.md).

    Uwaga: połączenia telefoniczne nie są obsługiwane na wszystkich symulatorów.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom program Visual Studio dla komputerów Mac, a na stronie początkowej kliknij **nowy projekt...**  do utworzenia nowego projektu:

    ![](quickstart-images/xs/new-solution.png "Nowe rozwiązanie")

2. W **wybierz szablon dla nowego projektu** okna dialogowego, kliknij przycisk **Multiplatform > aplikacji**, wybierz pozycję **pusta aplikacja formularzy** szablonu, a następnie kliknij przycisk **dalej** przycisk:

    ![](quickstart-images/xs/choose-template.png "Wybieranie szablonu")

3. W **skonfigurować aplikację formularzy** okna dialogowego, nazwę nowej aplikacji `Phoneword`, upewnij się, że **Użyj przenośnej biblioteki klas** przycisk radiowy zostanie wybrany, upewnij się, że **Użyj XAML dla użytkownika Interfejs pliki** pole wyboru jest zaznaczone, a następnie kliknij przycisk **dalej** przycisk:

    ![](quickstart-images/xs/configure-app.png "Konfigurowanie aplikacji formularzy")

4. W **Konfigurowanie nowej aplikacji formularzy** okno dialogowe, pozostaw ustawioną nazwy rozwiązania i projektu `Phoneword`, wybierz odpowiednią lokalizację projektu i kliknij przycisk **Utwórz** przycisk, aby utworzyć Projekt:

    ![](quickstart-images/xs/configure-project.png "Konfigurowanie projektu formularzy")

5. W **konsoli rozwiązania**, wybierz pozycję **Phoneword** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

    ![](quickstart-images/xs/add-new-file.png "Dodaj nowy plik")

6. W **nowy plik** okno dialogowe, wybierz **Formularze > Xaml wartość ContentPage formularze**, Nazwa nowego pliku **MainPage**i kliknij przycisk **nowy** przycisku. Spowoduje to dodanie stronę o nazwie **MainPage** do projektu:

    ![](quickstart-images/xs/add-mainpage-class.png "Dodaj nowy wartość ContentPage")

7. W **konsoli rozwiązania**, kliknij dwukrotnie **MainPage.xaml** go otworzyć:

    ![](quickstart-images/xs/open-mainpage-xaml.png "Otwórz MainPage.xaml")

8. W **MainPage.xaml**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod definiuje deklaratywnie interfejsu użytkownika dla strony:

    ```xaml
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
    ```

    Zapisać zmiany w **MainPage.xaml** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

9. W **konsoli rozwiązania**, kliknij dwukrotnie **MainPage.xaml.cs** go otworzyć:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

10. W **MainPage.xaml.cs**, usuń cały kod szablonu i zastąp go następującym kodem. `OnTranslate` i `OnCall` metod, które będą wykonywane w odpowiedzi na **Przetłumacz** i **wywołać** przycisków w momencie kliknięcia odpowiednio w interfejsie użytkownika:

    ```csharp
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
    ```

    > [!NOTE]
    > Próba skompilowania aplikacji, w tym momencie będą powodować błędy, które zostanie rozwiązany później.

    Zapisać zmiany w **MainPage.xaml.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

11. W **konsoli rozwiązania**, kliknij dwukrotnie **App.xaml.cs** go otworzyć:

    ![](quickstart-images/xs/open-app-class.png "Otwórz App.xaml.cs")

12. W **App.xaml.cs**, usuń cały kod szablonu i zastąp go następującym kodem. `App` Ustawia konstruktora `MainPage` klasy jako strony, który będzie wyświetlany podczas uruchamiania aplikacji:

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
    ```

    Zapisać zmiany w **Phoneword.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

13. W **konsoli rozwiązania**, wybierz pozycję **Phoneword** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

    ![](quickstart-images/xs/add-new-translator-file.png "Dodaj nowy plik")

14. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > pustą klasę**, podaj nazwę nowego pliku **PhoneTranslator**i kliknij przycisk **nowy** przycisk:

    ![](quickstart-images/xs/add-translator-class.png "Dodaj nową klasę")

15. W **PhoneTranslator.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod będzie tłumaczenie wyrazu phone na numer telefonu:

    ```csharp
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
    ```

    Zapisać zmiany w **PhoneTranslator.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

16. W **konsoli rozwiązania**, wybierz pozycję **Phoneword** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

    ![](quickstart-images/xs/add-new-interface.png "Dodaj nowy plik")

17. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > pustego interfejsu**, podaj nazwę nowego pliku **IDialer**i kliknij przycisk **nowy** przycisk:

    ![](quickstart-images/xs/add-idialer-interface.png "Dodaj nowy interfejs")

18. W **IDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod definiuje `Dial` metodę, która musi zostać wdrożone na każdej z platform wybierz numer telefonu przetłumaczonego:

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```
    Zapisać zmiany w **IDialer.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

    > [!NOTE]
    > Typowy kod aplikacji jest teraz ukończona. Specyficzne dla platformy phone telefon kod będzie teraz zaimplementowana jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

19. W **konsoli rozwiązania**, wybierz pozycję **Phoneword.iOS** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

    ![](quickstart-images/xs/add-new-file-ios.png "Dodaj nowy plik")

20. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > pustą klasę**, podaj nazwę nowego pliku **PhoneDialer.Document**i kliknij przycisk **nowy** przycisk:

    ![](quickstart-images/xs/new-phonedialer-ios.png "Dodaj nową klasę")

21. W **PhoneDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod tworzy `Dial` metoda, która będzie używana na platformie iOS wybierania numeru telefonu przetłumaczonego:

    ```csharp
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
    ```

    Zapisać zmiany w **PhoneDialer.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

22. W **konsoli rozwiązania**, wybierz pozycję **Phoneword.Droid** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

    ![](quickstart-images/xs/add-new-file-android.png "Dodaj nowy plik")

23. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > pustą klasę**, podaj nazwę nowego pliku **PhoneDialer.Document**i kliknij przycisk **nowy** przycisk:

    ![](quickstart-images/xs/new-phonedialer-android.png "Dodaj nową klasę")

24. W **PhoneDialer.cs**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod tworzy `Dial` metoda, która będzie używana na platformie Android do wybierania numeru telefonu przetłumaczonego:

    ```csharp
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
    ```

    Zapisać zmiany w **PhoneDialer.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

25. W **konsoli rozwiązania**w **Phoneword.Droid** projektu, kliknij dwukrotnie **MainActivity.cs** go otworzyć, Usuń wszystkie kod szablonu i zastąp go następującym kodem Kod:

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MyTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
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

    Zapisać zmiany w **MainActivity.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

    > [!NOTE]
    > Przykładowy kod używa `Theme="@style/MainTheme"` ponieważ jest ona oparta na starszej szablonu. Możesz sprawdzić nazwę poprawne stylu w **Phoneword/Droid/Resources/values/styles.xml** Jeśli wystąpi błąd kompilatora możesz uzyskać nazwę motywu.

26. W **konsoli rozwiązania**, rozwiń węzeł **właściwości** folder i kliknij dwukrotnie **AndroidManifest.xml** pliku:

    ![](quickstart-images/xs/android-manifest.png "Otwórz manifestu systemu Android")

27. W **wymagane uprawnienia** Włącz opcję **CallPhone** uprawnienia. Dzięki temu aplikacji uprawnienia do telefonicznie:

    ![](quickstart-images/xs/android-manifest-changed.png "Włącz uprawnień CallPhone")

    Zapisać zmiany w **AndroidManifest.xml** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

28. W **konsoli rozwiązania**, Usuń **PhonewordPage** klasę z **Phoneword** projektu. Ta strona została dodana automatycznie, gdy projekt został utworzony i nie jest już wymagane.
29. W programie Visual Studio dla komputerów Mac, wybierz **kompilacji > kompilacji wszystkich** elementu menu (lub naciśnij klawisz  **&#8984; + B**). Aplikacja zostanie utworzona i komunikat informujący będą wyświetlane w Visual Studio for Mac paska narzędzi.

    ![](quickstart-images/xs/build-successful.png "Pomyślnie kompilacji")

30. Jeśli wystąpią błędy, powtórz poprzednie kroki i usuń ewentualne błędy, dopóki aplikacja tworzy się pomyślnie.
31. W programie Visual Studio dla komputerów Mac narzędzi, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Odtwórz) do uruchamiania aplikacji w narzędziu iOS Simulator:

    ![](quickstart-images/xs/start.png "Programu Visual Studio for Mac narzędzi")
    ![](quickstart-images/xs/phoneword-result-ios.png "symulatora systemu iOS")

    Uwaga: połączenia telefoniczne nie są obsługiwane w narzędziu iOS Simulator.

32. W **konsoli rozwiązania**, wybierz pozycję **Phoneword.Droid** projektu, kliknij prawym przyciskiem myszy i wybierz **Ustaw jako projekt startowy**:

    ![](quickstart-images/xs/set-startup-project.png "Ustaw jako projekt startowy")

33. W programie Visual Studio dla komputerów Mac narzędzi, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Play), aby uruchomić aplikację w emulatorze systemu Android:

    ![](quickstart-images/xs/phoneword-result-android.png "Emulator systemu android")

    Uwaga: połączenia telefoniczne nie są obsługiwane w systemie Android emulatorów.

-----

Gratulujemy kończenie aplikacji platformy Xamarin.Forms. [Następnego tematu](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md) w tym przewodniku monitoruje czynności wykonane w tym przewodniku w celu uzyskania informacji na temat podstawowe informacje na temat tworzenia aplikacji za pomocą platformy Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [Uzyskiwanie dostępu do natywnego funkcji za pośrednictwem DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
