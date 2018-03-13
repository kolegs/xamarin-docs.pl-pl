---
title: Szybki Start Wieloekranowy platformy Xamarin.Forms
ms.topic: article
ms.prod: xamarin
ms.assetid: 255d93b9-518c-4e5d-a9cd-4dd8a7945a7f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: 5457cb3a80208311bac66a0232afd9cb4dc02c87
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="xamarinforms-multiscreen-quickstart"></a>Szybki Start Wieloekranowy platformy Xamarin.Forms

Ta opcja szybkiego startu pokazuje, jak rozszerzyć Phoneword przez dodanie drugiego ekranu do śledzenia historii połączeń dla aplikacji. Końcowe aplikacji jest pokazany poniżej:

[![](quickstart-images/intro-app-examples-sml.png "Aplikacja Phoneword")](quickstart-images/intro-app-examples.png#lightbox "Phoneword aplikacji")

Rozszerzanie aplikacji Phoneword w następujący sposób:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Uruchom program Visual Studio. Na stronie początkowej kliknij **Otwórz projekt...** , a następnie w **Otwórz projekt** oknie dialogowym Wybierz plik rozwiązania dla projektu Phoneword:

  ![](quickstart-images/vs/open-solution.png "Otwórz projekt")

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword** projekt i wybierz **Dodaj > Nowy element...** :

  ![](quickstart-images/vs/add-new-item.png "Dodaj nowy element")

1. W **Dodaj nowy element** okno dialogowe, wybierz opcję **elementów Visual C# > platformy Xamarin.Forms > strony zawartości**, nazwę nowego elementu **CallHistoryPage**i kliknij przycisk **Dodaj**  przycisku. Spowoduje to dodanie stronę o nazwie **CallHistoryPage** do projektu:

  ![](quickstart-images/vs/add-callhistorypage-class.png "Szablony projektu platformy Xamarin.Forms")

1. W **CallHistoryPage.xaml**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod definiuje deklaratywnie interfejsu użytkownika dla strony:

        <?xml version="1.0" encoding="UTF-8"?>
        <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           x:Class="Phoneword.CallHistoryPage"
                           Title="Call History">
            <ContentPage.Padding>
                <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS" Value="20, 40, 20, 20" />
                    <On Platform="Android, WinPhone, Windows" Value="20" />
                </OnPlatform>
            </ContentPage.Padding>
            <StackLayout>
              <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
            </StackLayout>
        </ContentPage>

  Zapisać zmiany w **CallHistoryPage.xaml** naciskając **CTRL + S**i zamknij plik.

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **App.xaml.cs** go otworzyć:

  ![](quickstart-images/vs/open-app-class.png "Open App.xaml.cs")

1. W **App.xaml.cs**, zaimportować `System.Collections.Generic` przestrzeni nazw, Dodaj deklaracja `PhoneNumbers` właściwości inicjowania właściwości w `App` Konstruktor i zainicjować [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) właściwość jako [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/). `PhoneNumbers` Kolekcji będzie służyć do przechowywania listy każdy numer telefonu przetłumaczony przez aplikację:

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

  Zapisać zmiany w **App.xaml.cs** naciskając **CTRL + S**i zamknij plik.

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **MainPage.xaml** go otworzyć:

  ![](quickstart-images/vs/open-mainpage-xaml.png "Otwórz MainPage.xaml")

1. W **MainPage.xaml**, Dodaj [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) sterowania na końcu [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) formantu. Przycisk będzie służyć do przejdź do strony Historia wywołań:

        <StackLayout VerticalOptions="FillAndExpand"
                     HorizontalOptions="FillAndExpand"
                     Orientation="Vertical"
                     Spacing="15">
          ...
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
          <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
                  Clicked="OnCallHistory" />
        </StackLayout>

  Zapisać zmiany w **MainPage.xaml** naciskając **CTRL + S**i zamknij plik.

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **MainPage.xaml.cs** go otworzyć:

  ![](quickstart-images/vs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

1. W **MainPage.xaml.cs**, Dodaj `OnCallHistory` metoda obsługi zdarzeń i zmodyfikuj `OnCall` metoda obsługi zdarzeń, aby dodać numer telefonu przetłumaczonego `App.PhoneNumbers` kolekcji i Włącz `callHistoryButton`, pod warunkiem że `dialer` zmienna nie jest `null`:

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

  Zapisać zmiany w **MainPage.xaml.cs** naciskając **CTRL + S**i zamknij plik.

1. W programie Visual Studio, wybierz **kompilacji > Kompiluj rozwiązanie** elementu menu (lub naciśnij klawisz **CTRL + SHIFT + B**). Aplikacja zostanie utworzona i zostanie wyświetlony komunikat Powodzenie, na pasku stanu programu Visual Studio:

  ![](quickstart-images/vs/build-successful.png "Pomyślnie kompilacji")

  Jeśli wystąpią błędy, powtórz poprzednie kroki i usuń ewentualne błędy, dopóki aplikacja tworzy się pomyślnie.

1. Na pasku narzędzi programu Visual Studio, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Odtwórz) do uruchamiania aplikacji:

  ![](quickstart-images/vs/start.png "Pasek narzędzi Visual Studio")
  ![](quickstart-images/vs/phone-result-uwp.png "Phoneword aplikacji platformy uniwersalnej systemu Windows")

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword.Droid** projekt i wybierz **Ustaw jako projekt startowy**.
1. Na pasku narzędzi programu Visual Studio, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Play), aby uruchomić aplikację w emulatorze systemu Android.
1. Jeśli masz urządzenie z systemem iOS i spełniać wymagania systemowe Mac do tworzenia aplikacji platformy Xamarin.Forms, użyj technika podobne do wdrożenia aplikacji na urządzeniu z systemem iOS. Można również wdrożyć aplikację, aby [zdalnego symulatora systemu iOS](~/tools/ios-simulator.md).

  Uwaga: połączenia telefoniczne nie są obsługiwane na wszystkich symulatorów.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom program Visual Studio dla komputerów Mac. Na stronie początkowej kliknij **Otwórz...** , a w oknie dialogowym Wybierz plik rozwiązania dla projektu Phoneword:

  ![](quickstart-images/xs/open-solution.png "Otwórz rozwiązanie")

1. W **konsoli rozwiązania**, wybierz pozycję **Phoneword** projektu, kliknij prawym przyciskiem myszy i wybierz **Dodaj > Nowy plik...** :

  ![](quickstart-images/xs/add-new-file.png "Dodaj nowy plik")

1. W **nowy plik** okno dialogowe, wybierz opcję **Formularze > Xaml wartość ContentPage formularze**, Nazwa nowego pliku **CallHistoryPage**i kliknij przycisk **nowy** przycisk. Spowoduje to dodanie stronę o nazwie **CallHistoryPage** do projektu:

  ![](quickstart-images/xs/add-callhistorypage-class.png "Dodaj wartość ContentPage formularzy")

1. W **konsoli rozwiązania**, kliknij dwukrotnie **CallHistoryPage.xaml** go otworzyć:

  ![](quickstart-images/xs/open-callhistorypage-xaml.png "Otwórz CallHistoryPage.xaml")

1. W **CallHistoryPage.xaml**, usuń cały kod szablonu i zastąp go następującym kodem. Ten kod definiuje deklaratywnie interfejsu użytkownika dla strony:

        <?xml version="1.0" encoding="UTF-8"?>
        <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           x:Class="Phoneword.CallHistoryPage"
                           Title="Call History">
            <ContentPage.Padding>
                <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS" Value="20, 40, 20, 20" />
                    <On Platform="Android, WinPhone, Windows" Value="20" />
                </OnPlatform>
            </ContentPage.Padding>
            <StackLayout>
              <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
            </StackLayout>
        </ContentPage>      

  Zapisać zmiany w **CallHistoryPage.xaml** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

1. W **konsoli rozwiązania**, kliknij dwukrotnie **App.xaml.cs** go otworzyć:

  ![](quickstart-images/xs/open-app-class.png "Open App.xaml.cs")

1. W **App.xaml.cs**, zaimportować `System.Collections.Generic` przestrzeni nazw, Dodaj deklaracja `PhoneNumbers` właściwości inicjowania właściwości w `App` Konstruktor i zainicjować [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) właściwość jako [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/). `PhoneNumbers` Kolekcji będzie służyć do przechowywania listy każdy numer telefonu przetłumaczony przez aplikację:

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

  Zapisać zmiany w **App.xaml.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

1. W **konsoli rozwiązania**, kliknij dwukrotnie **MainPage.xaml** go otworzyć:

  ![](quickstart-images/xs/open-mainpage-xaml.png "Otwórz MainPage.xaml")

1. W **MainPage.xaml**, Dodaj [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) sterowania na końcu [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) formantu. Przycisk będzie służyć do przejdź do strony Historia wywołań:

        <StackLayout VerticalOptions="FillAndExpand"
                     HorizontalOptions="FillAndExpand"
                     Orientation="Vertical"
                     Spacing="15">
          ...
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
          <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
                  Clicked="OnCallHistory" />
        </StackLayout>

  Zapisać zmiany w **MainPage.xaml** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

1. W **konsoli rozwiązania**, kliknij dwukrotnie **MainPage.xaml.cs** go otworzyć:

  ![](quickstart-images/xs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

1. W **MainPage.xaml.cs**, Dodaj `OnCallHistory` metoda obsługi zdarzeń i zmodyfikuj `OnCall` metoda obsługi zdarzeń, aby dodać numer telefonu przetłumaczonego `App.PhoneNumbers` kolekcji i Włącz `callHistoryButton`, pod warunkiem że `dialer` zmienna nie jest `null`:

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

  Zapisać zmiany w **MainPage.xaml.cs** , wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**) i zamknij plik.

1. W programie Visual Studio dla komputerów Mac, wybierz **kompilacji > kompilacji wszystkich** elementu menu (lub naciśnij klawisz  **&#8984; + B**). Aplikacja zostanie utworzona i komunikat informujący będą wyświetlane w Visual Studio for Mac narzędzi:

  ![](quickstart-images/xs/build-successful.png "Pomyślnie kompilacji")

  Jeśli wystąpią błędy, powtórz poprzednie kroki i usuń ewentualne błędy, dopóki aplikacja tworzy się pomyślnie.

1. W programie Visual Studio dla komputerów Mac narzędzi, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Odtwórz) do uruchamiania aplikacji w narzędziu iOS Simulator:

  ![](quickstart-images/xs/start.png "Programu Visual Studio for Mac narzędzi")
  ![](quickstart-images/xs/phone-result-ios.png "symulatora systemu iOS")

  Uwaga: połączenia telefoniczne nie są obsługiwane w narzędziu iOS Simulator.

1. W **konsoli rozwiązania**, wybierz pozycję **Phoneword.Droid** projektu, kliknij prawym przyciskiem myszy i wybierz **Ustaw jako projekt startowy**:

  ![](quickstart-images/xs/set-startup-project.png "Ustaw jako uruchomienia projektu")

1. W programie Visual Studio dla komputerów Mac narzędzi, naciśnij klawisz **Start** (przycisk trójkątny podobny przycisk Play), aby uruchomić aplikację w emulatorze systemu Android:

  ![](quickstart-images/xs/phone-result-android.png "Emulator systemu android")

  Uwaga: połączenia telefoniczne nie są obsługiwane w systemie Android emulatorów.

-----

Gratulujemy Kończenie Wieloekranowy aplikacji platformy Xamarin.Forms. [Następnego tematu](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/deepdive.md) w tym przewodniku monitoruje czynności wykonane w tym przewodniku w celu opracowywania zrozumienia Nawigacja strony i powiązania danych przy użyciu platformy Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [Phoneword (przykład)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
- [PhonewordMultiscreen (przykład)](https://developer.xamarin.com/samples/xamarin-forms/PhonewordMultiscreen/)
