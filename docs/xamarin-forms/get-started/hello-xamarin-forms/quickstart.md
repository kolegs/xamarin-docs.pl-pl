---
title: Przewodnik Szybki Start platformy Xamarin.Forms
description: W tym artykule wyjaśniono, jak utworzyć aplikację konwertujący alfanumeryczne podany numer telefonu przez użytkownika do numeru telefonu liczbowych i wywołująca numer.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: 5b5f8c80e49d66ed3bd8b008c975d1cfeda93ed4
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38832387"
---
# <a name="xamarinforms-quickstart"></a>Przewodnik Szybki Start platformy Xamarin.Forms

Ten przewodnik przedstawia sposób tworzenia aplikacji konwertujący alfanumeryczne podany numer telefonu przez użytkownika do numeru telefonu liczbowych i wywołująca numer. Poniżej przedstawiono końcowy aplikacji:

[![](quickstart-images/intro-app-examples-sml.png "Aplikacja Phoneword")](quickstart-images/intro-app-examples.png#lightbox "Phoneword aplikacji")

Utwórz aplikację Phoneword w następujący sposób:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. W **Start** ekranu, uruchom program Visual Studio. Spowoduje to otwarcie strony startowej:

    ![](quickstart-images/vs/start-page.png "Visual Studio")

2. W programie Visual Studio, kliknij przycisk **Utwórz nowy projekt...**  do utworzenia nowego projektu:

    ![](quickstart-images/vs/new-solution.png "Nowy projekt")

3. W **nowy projekt** okno dialogowe, kliknij przycisk **dla wielu Platform**, wybierz opcję **aplikacja mobilna (Xamarin.Forms)** szablonu, ustaw nazwę na **Phoneword**, wybierz odpowiednią lokalizację dla projektu i kliknij przycisk **OK** przycisku:

    ![](quickstart-images/vs/new-project.w157.png "Szablony projektu dla wielu Platform")

    > [!NOTE]
    > Niepowodzenie nazwij rozwiązanie **Phoneword** spowoduje dużo błędów kompilacji.

4. W **nowej aplikacji platformy Cross** okno dialogowe, kliknij przycisk **pusta aplikacja**, wybierz opcję **.NET Standard** strategia udostępniania kodu, a następnie kliknij przycisk **OK** przycisk:

    ![](quickstart-images/vs/new-app.png "Nowa aplikacja dla wielu Platform")

5. W **Eksploratora rozwiązań**w **Phoneword** projektu, kliknij dwukrotnie **MainPage.xaml** aby je otworzyć:

    ![](quickstart-images/vs/open-mainpage-xaml.png "Otwórz plik MainPage.xaml")

6. W **MainPage.xaml**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod definiuje sposób deklaratywny interfejsu użytkownika dla strony:

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

    Czy zapisać zmiany **MainPage.xaml** , naciskając klawisz **CTRL + S**i zamknij plik.

7. W **Eksploratora rozwiązań**, rozwiń węzeł **MainPage.xaml** i kliknij dwukrotnie **MainPage.xaml.cs** aby je otworzyć:

    ![](quickstart-images/vs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

8. W **MainPage.xaml.cs**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. `OnTranslate` i `OnCall` metodami metody będą wykonywane w odpowiedzi na **Translate** i **wywołania** przycisków w momencie kliknięcia odpowiednio w interfejsie użytkownika:

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
                translatedNumber = PhonewordTranslator.ToNumber (phoneNumberText.Text);
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
    > Próby utworzenia aplikacji na tym etapie będą powodować błędy, które zostanie rozwiązany później.

    Czy zapisać zmiany **MainPage.xaml.cs** , naciskając klawisz **CTRL + S**i zamknij plik.

9. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword** projektu, a następnie wybierz **Dodaj > Nowy element...** :

    ![](quickstart-images/vs/add-new-item.png "Dodaj nowy element")

10. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Kod > klasa**, nadaj nowemu plikowi **PhoneTranslator**i kliknij przycisk **Dodaj** przycisku :

    ![](quickstart-images/vs/add-translator-class.w157.png "Dodaj nową klasę")

11. W **PhoneTranslator.cs**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod będzie tłumaczenie wyrazu telefonu na numer telefonu:

    ```csharp
    using System.Text;

    namespace Phoneword
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

    Czy zapisać zmiany **PhoneTranslator.cs** , naciskając klawisz **CTRL + S**i zamknij plik.

12. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword** projektu, a następnie wybierz **Dodaj > Nowy element...** :

    ![](quickstart-images/vs/add-new-item.png "Dodaj nowy element")

13. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Kod > interfejs**, nadaj nowemu plikowi **IDialer**i kliknij przycisk **Dodaj** przycisku:

    ![](quickstart-images/vs/add-idialer-interface.w157.png "Dodawanie nowego interfejsu")

14. W **IDialer.cs**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod definiuje `Dial` metodę, która musi zostać wdrożone na poszczególnych platform, aby wybrać numer telefonu tłumaczenia:

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```

    Czy zapisać zmiany **IDialer.cs** , naciskając klawisz **CTRL + S**i zamknij plik.

    > [!NOTE]
    > Typowy kod dla aplikacji została zakończona. Kod telefon phone specyficzne dla platformy zostaną teraz zaimplementowane jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

15. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.iOS** projektu, a następnie wybierz **Dodaj > Nowy element...** :

    ![](quickstart-images/vs/add-new-item-ios.png "Dodaj nowy element")

16. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Apple > Kod > klasa**, nadaj nowemu plikowi **PhoneDialer.Document**i kliknij przycisk **Dodaj** przycisku:

    ![](quickstart-images/vs/new-phone-dialer-ios.w157.png "Dodaj nową klasę")

17. W **PhoneDialer.cs**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod tworzy <code>Dial</code> metodę, która będzie używana na platformie systemu iOS na numer telefonu tłumaczenia:

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

    Czy zapisać zmiany **PhoneDialer.cs** , naciskając klawisz **CTRL + S**i zamknij plik.

18. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.Android** projektu, a następnie wybierz **Dodaj > Nowy element...** :

    ![](quickstart-images/vs/add-new-item-android.png "Dodaj nowy element")

19. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Android > klasa**, nadaj nowemu plikowi **PhoneDialer.Document**i kliknij przycisk **Dodaj** przycisku:

    ![](quickstart-images/vs/new-phone-dialer-android.w157.png "Dodaj nową klasę")

20. W **PhoneDialer.cs**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod tworzy `Dial` metodę, która będzie używana na platformie systemu Android na numer telefonu tłumaczenia:

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

                var intent = new Intent (Intent.ActionDial);
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

    Czy zapisać zmiany **PhoneDialer.cs** , naciskając klawisz **CTRL + S**i zamknij plik.

21. W **Eksploratora rozwiązań**w **Phoneword.Android** projektu, kliknij dwukrotnie **MainActivity.cs** aby go otworzyć, należy usunąć wszystkie kod szablonu i zastąp go wartością Poniższy kod:

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true,
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

    Czy zapisać zmiany **MainActivity.cs** , naciskając klawisz **CTRL + S**i zamknij plik.

22. W **Eksploratora rozwiązań**w **Phoneword.Android** projektu, kliknij dwukrotnie **właściwości** i wybierz **manifestu systemu Android** karty:

    ![](quickstart-images/vs/android-manifest.png "Otwórz manifestu systemu Android")

23. W **wymagane uprawnienia** Włącz **CALL_PHONE** uprawnień. Daje to uprawnień aplikacji, aby umieścić połączenia telefonicznego:

    ![](quickstart-images/vs/android-manifest-changed.png "Włącz uprawnienia CallPhone")

    Zapisz zmiany w manifeście, naciskając klawisz **CTRL + S**i zamknij plik.

24. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.UWP** projektu, a następnie wybierz **Dodaj > Nowy element...** :

    ![](quickstart-images/vs/add-new-item-uwp.png "Dodaj nowy element")

25. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Kod > klasa**, nadaj nowemu plikowi **PhoneDialer.Document**i kliknij przycisk **Dodaj** przycisku:

    ![](quickstart-images/vs/new-phone-dialer-uwp.w157.png "Dodaj nową klasę")

26. W **PhoneDialer.cs**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod tworzy `Dial` metoda i metody pomocnicze, które będą używane na platformie Universal Windows numer telefonu tłumaczenia:

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

    Czy zapisać zmiany **PhoneDialer.cs** , naciskając klawisz **CTRL + S**i zamknij plik.

27. W **Eksploratora rozwiązań**w **Phoneword.UWP** projektu, kliknij prawym przyciskiem myszy **odwołania**i wybierz **Dodaj odwołanie...** :

    ![](quickstart-images/vs/uwp-add-reference.png "Dodawanie odwołania")

28. W **Menadżer odwołań** okno dialogowe, wybierz opcję **Universal Windows > Rozszerzenia > Windows Mobile rozszerzeń dla platformy uniwersalnej systemu Windows**i kliknij przycisk **OK** przycisku:

    ![](quickstart-images/vs/uwp-add-reference-extensions.png "Dodawanie rozszerzeń przenośnych Windows dla platformy uniwersalnej systemu Windows")

29. W **Eksploratora rozwiązań**w **Phoneword.UWP** projektu, kliknij dwukrotnie **Package.appxmanifest**:

    ![](quickstart-images/vs/uwp-manifest.png "Otwórz Manifest platformy uniwersalnej systemu Windows")

30. W **możliwości** strony, należy włączyć **połączeń telefonicznych** możliwości. Daje to uprawnień aplikacji, aby umieścić połączenia telefonicznego:

    ![](quickstart-images/vs/uwp-manifest-changed.png "Włącz możliwość połączenia telefonicznego")

    Zapisz zmiany w manifeście, naciskając klawisz **CTRL + S**i zamknij plik.

31. W programie Visual Studio, wybierz **kompilacji > Kompiluj rozwiązanie** elementu menu (lub naciśnij **CTRL + SHIFT + B**). Aplikacja zostanie skompilowana i na pasku stanu programu Visual Studio pojawi się komunikat o powodzeniu:

    ![](quickstart-images/vs/build-successful.png "Kompilacja powiodła się")

    Jeśli występują błędy, powtórz poprzednie kroki i Popraw wszelkie błędy, aż aplikacja zostanie pomyślnie skompilowana.

32. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.UWP** projektu, a następnie wybierz **Ustaw jako projekt startowy**:

    ![](quickstart-images/vs/uwp-set-as-startup-project.png "Ustaw jako projekt startowy")

33. Na pasku narzędzi programu Visual Studio naciśnij klawisz **Start** (przycisk trójkątna przypominającą przycisk Odtwórz) do uruchomienia aplikacji:

    ![](quickstart-images/vs/start.png "Narzędzi programu Visual Studio")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword aplikacji platformy uniwersalnej systemu Windows")

34. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.Android** projektu, a następnie wybierz **Ustaw jako projekt startowy**.
35. Na pasku narzędzi programu Visual Studio naciśnij klawisz **Start** (przycisk trójkątna przypominającą przycisk Odtwórz) do uruchomienia aplikacji w emulatorze systemu Android.
36. Jeśli masz urządzenie z systemem iOS i spełniać wymagania systemowe Mac dla programowania na platformie Xamarin.Forms, podobne techniki należy używać do wdrażania aplikacji na urządzeniu z systemem iOS. Alternatywnie: Wdrażanie aplikacji na [zdalny symulator systemu iOS](~/tools/ios-simulator.md).

    Uwaga: połączenia telefoniczne nie są obsługiwane na wszystkich symulatorów.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom program Visual Studio dla komputerów Mac, a następnie na stronie początkowej kliknij **nowy projekt...**  do utworzenia nowego projektu:

    ![](quickstart-images/xs/new-solution.png "Nowe rozwiązanie")

2. W **wybierz szablon dla nowego projektu** okno dialogowe, kliknij przycisk **dla wielu platform > App**, wybierz opcję **pusta aplikacja formularzy** szablonu, a następnie kliknij przycisk **dalej** przycisku:

    ![](quickstart-images/xs/choose-template.png "Wybierz szablon")

3. W **Konfiguruj aplikację formularzy** okno dialogowe, nazwa nowej aplikacji **Phoneword**, upewnij się, że **Użyj .NET Standard** przycisku radiowego jest zaznaczone, a następnie kliknij przycisk **Dalej** przycisku:

    ![](quickstart-images/xs/configure-app.png "Konfigurowanie aplikacji formularzy")

4. W **Konfigurowanie nowej aplikacji formularzy** okno dialogowe, pozostaw wartość nazwy rozwiązania i projektu **Phoneword**, a następnie wybierz odpowiednią lokalizację dla projektu i kliknij przycisk **Utwórz**przycisk, aby utworzyć projekt:

    ![](quickstart-images/xs/configure-project.png "Konfigurowanie projektu formularzy")

    > [!NOTE]
    > Kończy się niepowodzeniem nadać nazwę rozwiązania i projektu **Phoneword** spowoduje dużo błędów kompilacji.

5. W **konsoli rozwiązania**, kliknij dwukrotnie **MainPage.xaml** aby je otworzyć:

    ![](quickstart-images/xs/open-mainpage-xaml.png "Otwórz plik MainPage.xaml")

6. W **MainPage.xaml**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod definiuje sposób deklaratywny interfejsu użytkownika dla strony:

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

    Czy zapisać zmiany **MainPage.xaml** , wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**) i zamknij plik.

7. W **konsoli rozwiązania**, kliknij dwukrotnie **MainPage.xaml.cs** aby je otworzyć:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

8. W **MainPage.xaml.cs**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. `OnTranslate` i `OnCall` metodami metody będą wykonywane w odpowiedzi na **Translate** i **wywołania** przycisków w momencie kliknięcia odpowiednio w interfejsie użytkownika:

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
                translatedNumber = PhonewordTranslator.ToNumber (phoneNumberText.Text);
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
    > Próby utworzenia aplikacji na tym etapie będą powodować błędy, które zostanie rozwiązany później.

    Czy zapisać zmiany **MainPage.xaml.cs** , wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**) i zamknij plik.

9. W **konsoli rozwiązania**, wybierz opcję **Phoneword** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

    ![](quickstart-images/xs/add-new-translator-file.png "Dodaj nowy plik")

10. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > Pusta klasa**, nadaj nowemu plikowi **PhoneTranslator**i kliknij przycisk **New** przycisku:

    ![](quickstart-images/xs/add-translator-class.png "Dodaj nową klasę")

11. W **PhoneTranslator.cs**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod będzie tłumaczenie wyrazu telefonu na numer telefonu:

    ```csharp
    using System.Text;

    namespace Phoneword
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

    Czy zapisać zmiany **PhoneTranslator.cs** , wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**) i zamknij plik.

12. W **konsoli rozwiązania**, wybierz opcję **Phoneword** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

    ![](quickstart-images/xs/add-new-interface.png "Dodaj nowy plik")

13. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > pusty interfejs**, nadaj nowemu plikowi **IDialer**i kliknij przycisk **New** przycisku:

    ![](quickstart-images/xs/add-idialer-interface.png "Dodawanie nowego interfejsu")

14. W **IDialer.cs**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod definiuje `Dial` metodę, która musi zostać wdrożone na poszczególnych platform, aby wybrać numer telefonu tłumaczenia:

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```
    Czy zapisać zmiany **IDialer.cs** , wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**) i zamknij plik.

    > [!NOTE]
    > Typowy kod dla aplikacji została zakończona. Kod telefon phone specyficzne dla platformy zostaną teraz zaimplementowane jako [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

15. W **konsoli rozwiązania**, wybierz opcję **Phoneword.iOS** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

    ![](quickstart-images/xs/add-new-file-ios.png "Dodaj nowy plik")

16. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > Pusta klasa**, nadaj nowemu plikowi **PhoneDialer.Document**i kliknij przycisk **New** przycisku:

    ![](quickstart-images/xs/new-phonedialer-ios.png "Dodaj nową klasę")

17. W **PhoneDialer.cs**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod tworzy `Dial` metodę, która będzie używana na platformie systemu iOS na numer telefonu tłumaczenia:

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

    Czy zapisać zmiany **PhoneDialer.cs** , wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**) i zamknij plik.

18. W **konsoli rozwiązania**, wybierz opcję **Phoneword.Droid** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

    ![](quickstart-images/xs/add-new-file-android.png "Dodaj nowy plik")

19. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > Pusta klasa**, nadaj nowemu plikowi **PhoneDialer.Document**i kliknij przycisk **New** przycisku:

    ![](quickstart-images/xs/new-phonedialer-android.png "Dodaj nową klasę")

20. W **PhoneDialer.cs**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod tworzy `Dial` metodę, która będzie używana na platformie systemu Android na numer telefonu tłumaczenia:

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

                var intent = new Intent (Intent.ActionDial);
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

    Czy zapisać zmiany **PhoneDialer.cs** , wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**) i zamknij plik.

21. W **konsoli rozwiązania**w **Phoneword.Droid** projektu, kliknij dwukrotnie **MainActivity.cs** aby go otworzyć, Usuń wszystkie kod szablonu i zastąp go następującym kodem :

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true,
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

    Czy zapisać zmiany **MainActivity.cs** , wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**) i zamknij plik.

22. W **konsoli rozwiązania**, rozwiń węzeł **właściwości** folder i kliknij dwukrotnie plik **AndroidManifest.xml** pliku:

    ![](quickstart-images/xs/android-manifest.png "Otwórz manifestu systemu Android")

23. W **wymagane uprawnienia** Włącz **CallPhone** uprawnień. Daje to uprawnień aplikacji, aby umieścić połączenia telefonicznego:

    ![](quickstart-images/xs/android-manifest-changed.png "Włącz uprawnienia CallPhone")

    Czy zapisać zmiany **AndroidManifest.xml** , wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**) i zamknij plik.

24. W programie Visual Studio dla komputerów Mac, wybierz **kompilacji > Tworzenie wszystkich** elementu menu (lub naciśnij  **&#8984; + B**). Aplikacja zostanie skompilowana i w programie Visual Studio dla komputerów Mac narzędzi pojawi się komunikat o powodzeniu.

    ![](quickstart-images/xs/build-successful.png "Kompilacja powiodła się")

25. Jeśli występują błędy, powtórz poprzednie kroki i Popraw wszelkie błędy, aż aplikacja zostanie pomyślnie skompilowana.
26. W programie Visual Studio dla komputerów Mac paska narzędzi, naciśnij klawisz **Start** (przycisk trójkątna przypominającą przycisk Odtwórz) do uruchomienia aplikacji w symulatorze systemu iOS:

    ![](quickstart-images/xs/start.png "Program Visual Studio for Mac narzędzi")
    ![](quickstart-images/xs/phoneword-result-ios.png "symulatora systemu iOS")

    Uwaga: połączenia telefoniczne nie są obsługiwane w narzędziu iOS Simulator.

27. W **konsoli rozwiązania**, wybierz opcję **Phoneword.Droid** projektu, kliknij prawym przyciskiem myszy i wybierz **Ustaw jako projekt startowy**:

    ![](quickstart-images/xs/set-startup-project.png "Ustaw jako projekt startowy")

28. W programie Visual Studio dla komputerów Mac paska narzędzi, naciśnij klawisz **Start** (przycisk trójkątna przypominającą przycisk Odtwórz) do uruchomienia aplikacji w emulatorze systemu Android:

    ![](quickstart-images/xs/phoneword-result-android.png "Emulator systemu android")

    Uwaga: połączenia telefoniczne nie są obsługiwane w przypadku emulatorów systemu Android.

-----

Gratulacje z okazji kończenie aplikacji platformy Xamarin.Forms. [Następnego tematu](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md) w tym przewodniku przeglądy czynności, które zostały wykonane w tym przewodniku, aby poznać podstawy tworzenia aplikacji przy użyciu zestawu narzędzi Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [Dostęp do natywnych funkcji za pośrednictwem DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
