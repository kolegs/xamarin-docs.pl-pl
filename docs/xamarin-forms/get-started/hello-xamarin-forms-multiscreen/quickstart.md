---
title: Krótkie wprowadzenie do zestawu narzędzi Xamarin.Forms (wiele ekranów)
description: W tym artykule wyjaśniono, jak rozszerzyć aplikację Phoneword, dodając drugi ekran, aby śledzić historię wywołań dla aplikacji.
ms.prod: quickstart
ms.assetid: 255d93b9-518c-4e5d-a9cd-4dd8a7945a7f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: a4e27f1810a16b5d13838d2e2c1067950586fab3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996184"
---
# <a name="xamarinforms-multiscreen-quickstart"></a>Krótkie wprowadzenie do zestawu narzędzi Xamarin.Forms (wiele ekranów)

Ten przewodnik Szybki Start pokazano, jak rozszerzyć aplikację Phoneword, dodając drugi ekran, aby śledzić historię wywołań dla aplikacji. Poniżej przedstawiono końcowy aplikacji:

[![](quickstart-images/intro-app-examples-sml.png "Aplikacja Phoneword")](quickstart-images/intro-app-examples.png#lightbox "Phoneword aplikacji")

Rozszerzanie aplikacji Phoneword w następujący sposób:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Uruchom program Visual Studio. Na stronie początkowej kliknij **Otwórz projekt...** , a następnie w **Otwórz projekt** okno dialogowe, wybierz plik rozwiązania dla projektu Phoneword:

    ![](quickstart-images/vs/open-solution.png "Otwórz projekt")

2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword** projektu, a następnie wybierz **Dodaj > Nowy element...** :

    ![](quickstart-images/vs/add-new-item.png "Dodaj nowy element")

3. W **Dodaj nowy element** okno dialogowe, wybierz opcję **elementy Visual C# > Xamarin.Forms > strony zawartości**, nadaj nazwę nowego elementu **CallHistoryPage**i kliknij przycisk **Dodaj**  przycisku. Spowoduje to dodanie stronę o nazwie **CallHistoryPage** do projektu:

    ![](quickstart-images/vs/add-callhistorypage-class.png "Szablony projektu Xamarin.Forms")

4. W **CallHistoryPage.xaml**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod definiuje sposób deklaratywny interfejsu użytkownika dla strony:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>
    ```

    Czy zapisać zmiany **CallHistoryPage.xaml** , naciskając klawisz **CTRL + S**i zamknij plik.

5. W **Eksploratora rozwiązań**, kliknij dwukrotnie **App.xaml.cs** pliku w udostępnionej **Phoneword** projektu, aby otworzyć go:

    ![](quickstart-images/vs/open-app-class.png "Otwórz plik App.xaml.cs")

6. W **App.xaml.cs**, zaimportuj `System.Collections.Generic` przestrzeni nazw, dodawanie deklaracji `PhoneNumbers` właściwości inicjowania właściwości w `App` Konstruktor i zainicjować [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) właściwości [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). `PhoneNumbers` Bezużytecznych zostanie użyty do przechowywania listy każdy numer telefonu przetłumaczone przez aplikację:

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public static IList<string> PhoneNumbers { get; set; }

            public App()
            {
                InitializeComponent();
                PhoneNumbers = new List<string>();
                MainPage = new NavigationPage(new MainPage());
            }
            ...
        }
    }
    ```

    Czy zapisać zmiany **App.xaml.cs** , naciskając klawisz **CTRL + S**i zamknij plik.

7. W **Eksploratora rozwiązań**, kliknij dwukrotnie **MainPage.xaml** pliku w udostępnionej **Phoneword** projektu, aby otworzyć go:

    ![](quickstart-images/vs/open-mainpage-xaml.png "Otwórz plik MainPage.xaml")

8. W **MainPage.xaml**, Dodaj [ `Button` ](xref:Xamarin.Forms.Button) kontroli na końcu [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) kontroli. Przycisk będzie służyć do przejdź do strony historii wywołań:

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    Czy zapisać zmiany **MainPage.xaml** , naciskając klawisz **CTRL + S**i zamknij plik.

9. W **Eksploratora rozwiązań**, kliknij dwukrotnie **MainPage.xaml.cs** aby je otworzyć:

    ![](quickstart-images/vs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

10. W **MainPage.xaml.cs**, Dodaj `OnCallHistory` metody obsługi zdarzeń i zmodyfikuj `OnCall` metody obsługi zdarzeń, aby dodać numer telefonu przetłumaczone `App.PhoneNumbers` kolekcji i włączyć `callHistoryButton`, pod warunkiem, że `dialer` zmienna nie jest `null`:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            ...

            async void OnCall(object sender, EventArgs e)
            {
                ...
                if (dialer != null) {
                    App.PhoneNumbers.Add (translatedNumber);
                    callHistoryButton.IsEnabled = true;
                    dialer.Dial (translatedNumber);
                }
                ...
            }

            async void OnCallHistory(object sender, EventArgs e)
            {
                await Navigation.PushAsync (new CallHistoryPage ());
            }
        }
    }
    ```

    Czy zapisać zmiany **MainPage.xaml.cs** , naciskając klawisz **CTRL + S**i zamknij plik.

11. W programie Visual Studio, wybierz **kompilacji > Kompiluj rozwiązanie** elementu menu (lub naciśnij **CTRL + SHIFT + B**). Aplikacja zostanie skompilowana i na pasku stanu programu Visual Studio pojawi się komunikat o powodzeniu:

    ![](quickstart-images/vs/build-successful.png "Kompilacja powiodła się")

    Jeśli występują błędy, powtórz poprzednie kroki i Popraw wszelkie błędy, aż aplikacja zostanie pomyślnie skompilowana.

12. Na pasku narzędzi programu Visual Studio naciśnij klawisz **Start** (przycisk trójkątna przypominającą przycisk Odtwórz) do uruchomienia aplikacji:

    ![](quickstart-images/vs/start.png "Narzędzi programu Visual Studio")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword aplikacji platformy uniwersalnej systemu Windows")

13. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.Droid** projektu, a następnie wybierz **Ustaw jako projekt startowy**.
14. Na pasku narzędzi programu Visual Studio naciśnij klawisz **Start** (przycisk trójkątna przypominającą przycisk Odtwórz) do uruchomienia aplikacji w emulatorze systemu Android.
15. Jeśli masz urządzenie z systemem iOS i spełniać wymagania systemowe Mac dla programowania na platformie Xamarin.Forms, podobne techniki należy używać do wdrażania aplikacji na urządzeniu z systemem iOS. Alternatywnie: Wdrażanie aplikacji na [zdalny symulator systemu iOS](~/tools/ios-simulator.md).

    Uwaga: połączenia telefoniczne nie są obsługiwane na wszystkich symulatorów.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom program Visual Studio dla komputerów Mac. Na stronie początkowej kliknij **Otwórz...** i w oknie dialogowym Wybierz plik rozwiązania dla projektu Phoneword:

    ![](quickstart-images/xs/open-solution.png "Otwórz rozwiązanie")

2. W **konsoli rozwiązania**, wybierz opcję **Phoneword** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

    ![](quickstart-images/xs/add-new-file.png "Dodaj nowy plik")

3. W **nowy plik** okno dialogowe, wybierz opcję **formularzy > Forms ContentPage Xaml**, nadaj nowemu plikowi **CallHistoryPage**i kliknij przycisk **New** przycisk. Spowoduje to dodanie stronę o nazwie **CallHistoryPage** do projektu:

    ![](quickstart-images/xs/add-callhistorypage-class.png "Dodaj Forms ContentPage")

4. W **konsoli rozwiązania**, kliknij dwukrotnie **CallHistoryPage.xaml** aby je otworzyć:

    ![](quickstart-images/xs/open-callhistorypage-xaml.png "Otwórz CallHistoryPage.xaml")

5. W **CallHistoryPage.xaml**, Usuń wszystkie kod szablonu i zastąp go następującym kodem. Ten kod definiuje sposób deklaratywny interfejsu użytkownika dla strony:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>      
    ```

    Czy zapisać zmiany **CallHistoryPage.xaml** , wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**) i zamknij plik.

6. W **konsoli rozwiązania**, kliknij dwukrotnie **App.xaml.cs** aby je otworzyć:

    ![](quickstart-images/xs/open-app-class.png "Otwórz plik App.xaml.cs")

7. W **App.xaml.cs**, zaimportuj `System.Collections.Generic` przestrzeni nazw, dodawanie deklaracji `PhoneNumbers` właściwości inicjowania właściwości w `App` Konstruktor i zainicjować [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) właściwości [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). `PhoneNumbers` Bezużytecznych zostanie użyty do przechowywania listy każdy numer telefonu przetłumaczone przez aplikację:

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public static IList<string> PhoneNumbers { get; set; }

            public App()
            {
                InitializeComponent();
                PhoneNumbers = new List<string>();
                MainPage = new NavigationPage(new MainPage());
            }
            ...
        }
    }
    ```

    Czy zapisać zmiany **App.xaml.cs** , wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**) i zamknij plik.

8. W **konsoli rozwiązania**, kliknij dwukrotnie **MainPage.xaml** aby je otworzyć:

    ![](quickstart-images/xs/open-mainpage-xaml.png "Otwórz plik MainPage.xaml")

9. W **MainPage.xaml**, Dodaj [ `Button` ](xref:Xamarin.Forms.Button) kontroli na końcu [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) kontroli. Przycisk będzie służyć do przejdź do strony historii wywołań:

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    Czy zapisać zmiany **MainPage.xaml** , wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**) i zamknij plik.

10. W **konsoli rozwiązania**, kliknij dwukrotnie **MainPage.xaml.cs** aby je otworzyć:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

11. W **MainPage.xaml.cs**, Dodaj `OnCallHistory` metody obsługi zdarzeń i zmodyfikuj `OnCall` metody obsługi zdarzeń, aby dodać numer telefonu przetłumaczone `App.PhoneNumbers` kolekcji i włączyć `callHistoryButton`, pod warunkiem, że `dialer` zmienna nie jest `null`:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            ...

            async void OnCall(object sender, EventArgs e)
            {
                ...
                if (dialer != null) {
                    App.PhoneNumbers.Add (translatedNumber);
                    callHistoryButton.IsEnabled = true;
                    dialer.Dial (translatedNumber);
                }
                ...
            }

            async void OnCallHistory(object sender, EventArgs e)
            {
                await Navigation.PushAsync (new CallHistoryPage ());
            }
        }
    }
    ```

    Czy zapisać zmiany **MainPage.xaml.cs** , wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**) i zamknij plik.

12. W programie Visual Studio dla komputerów Mac, wybierz **kompilacji > Tworzenie wszystkich** elementu menu (lub naciśnij  **&#8984; + B**). Aplikacja zostanie skompilowana i w programie Visual Studio dla komputerów Mac narzędzi pojawi się komunikat o powodzeniu:

    ![](quickstart-images/xs/build-successful.png "Kompilacja powiodła się")

    Jeśli występują błędy, powtórz poprzednie kroki i Popraw wszelkie błędy, aż aplikacja zostanie pomyślnie skompilowana.

13. W programie Visual Studio dla komputerów Mac paska narzędzi, naciśnij klawisz **Start** (przycisk trójkątna przypominającą przycisk Odtwórz) do uruchomienia aplikacji w symulatorze systemu iOS:

    ![](quickstart-images/xs/start.png "Program Visual Studio for Mac narzędzi")
    ![](quickstart-images/xs/phone-result-ios.png "symulatora systemu iOS")

    Uwaga: połączenia telefoniczne nie są obsługiwane w narzędziu iOS Simulator.

14. W **konsoli rozwiązania**, wybierz opcję **Phoneword.Droid** projektu, kliknij prawym przyciskiem myszy i wybierz **Ustaw jako projekt startowy**:

    ![](quickstart-images/xs/set-startup-project.png "Projekt jako startowy")

15. W programie Visual Studio dla komputerów Mac paska narzędzi, naciśnij klawisz **Start** (przycisk trójkątna przypominającą przycisk Odtwórz) do uruchomienia aplikacji w emulatorze systemu Android:

    ![](quickstart-images/xs/phone-result-android.png "Emulator systemu android")

    Uwaga: połączenia telefoniczne nie są obsługiwane w przypadku emulatorów systemu Android.

-----

Gratulacje z okazji Kończenie aplikację platformy Xamarin.Forms (wiele ekranów). [Następnego tematu](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/deepdive.md) w tym przewodniku przeglądy czynności, które zostały wykonane w tym przewodniku do opracowywania zrozumienia nawigowania po stronach i powiązania danych za pomocą zestawu narzędzi Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [Phoneword (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
- [PhonewordMultiscreen (przykład)](https://developer.xamarin.com/samples/xamarin-forms/PhonewordMultiscreen/)
