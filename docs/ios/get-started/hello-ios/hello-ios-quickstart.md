---
title: Witaj, iOS
description: Ten przewodnik dwuczęściową opisuje sposób tworzenia podstawowej aplikacji platformy Xamarin.iOS przy użyciu programu Visual Studio dla komputerów Mac lub Visual Studio i zrozumienia podstaw dotyczących tworzenia aplikacji systemu iOS za pomocą platformy Xamarin. Spowoduje to wprowadzenie narzędzi, pojęcia i kroki wymagane do tworzenia i wdrażania aplikacji platformy Xamarin.iOS.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: D3868F3A-4EED-BDDF-45AA-665102C39634
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/23/2017
ms.openlocfilehash: dc9b86845dc91c7fb8ec3a88a5862e5e9f6de18d
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2018
---
# <a name="helloios-quickstart"></a>Hello.iOS Szybki Start

W tym przewodniku opisano sposób tworzenia aplikacji, który tłumaczy alfanumeryczne numer wprowadzony przez użytkownika do numeru telefonu liczbowego, a następnie wywołuje ten numer. Końcowe aplikacji wygląda następująco:

 [![](hello-ios-quickstart-images/image1.png "Aplikacja szybkiego startu Hello.iOS")](hello-ios-quickstart-images/image1.png#lightbox)


<a name="Requirements" />

## <a name="requirements"></a>Wymagania

Tworzenie aplikacji systemu iOS za pomocą platformy Xamarin wymaga:

-  Mac macOS uruchomionych Sierra (10.12) lub nowszy.
-  Zainstalowana najnowsza wersja Xcode i iOS SDK z [sklepu z aplikacjami](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) .

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Xamarin.iOS współpracuje z poniższych konfiguracji:

-  Najnowsza wersja programu Visual Studio dla komputerów Mac, pasujące specyfikacje powyżej.

[Przewodnik instalacji Mac Xamarin.iOS](~/ios/get-started/installation/mac.md) jest dostępny, aby uzyskać instrukcje krok po kroku instalacji

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.iOS współpracuje z poniższych konfiguracji:

-  Najnowszą wersję programu Visual Studio 2015 lub 2017 Professional lub wyższej w systemie Windows 7 lub nowszym, łączyć się z hosta kompilacji Mac, który pasuje do specyfikacji powyżej.

[Przewodnik instalacji systemu Windows Xamarin.iOS](~/ios/get-started/installation/windows/index.md) jest dostępny, aby uzyskać instrukcje krok po kroku instalacji.

-----

Przed rozpoczęciem pracy należy pobrać [ustawić ikony aplikacji platformy Xamarin](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac wskazówki

Ten przewodnik opisuje sposób tworzenia aplikacji o nazwie Phoneword, który tłumaczy numer telefonu alfanumeryczne pod numer telefonu liczbowych.

1. Uruchom program Visual Studio for Mac z **aplikacji** folderu lub **Spotlight** można wyświetlić ekranu uruchamiania:

  ![](hello-ios-quickstart-images/image2new.png "Ekran startowy")

Na ekranie uruchamiania kliknij **nowy projekt...**  do tworzenia nowego rozwiązania Xamarin.iOS:

![](hello-ios-quickstart-images/image3new.png "rozwiązania dla systemu iOS")


2. Z **okno dialogowe nowego rozwiązania**, wybierz **systemu iOS > aplikacji > Aplikacja pojedynczego widoku** szablonu, zapewniając, że C# jest zaznaczona. Kliknij przycisk **dalej**:

  ![](hello-ios-quickstart-images/image4new.png "Wybierz aplikacja z jednym widokiem")

3. Konfigurowanie aplikacji. Nadaj **nazwa** `Phoneword_iOS`i pozostawić wszystkie inne jako domyślny. Kliknij przycisk **dalej**:

  ![](hello-ios-quickstart-images/image5new.png "Wprowadź nazwę aplikacji")

4. Pozostaw projektu i rozwiązania nazwę w postaci. Wybierz lokalizację projektu w tym miejscu lub zachować go jako domyślny:

  ![](hello-ios-quickstart-images/image6new.png "Wybierz lokalizację projektu")

5. Kliknij przycisk **Utwórz** aby **rozwiązania**.

6. Otwórz **Main.storyboard** pliku przez dwukrotne kliknięcie w **konsoli rozwiązania**. Dzięki temu można wizualnie do tworzenia interfejsu użytkownika:

  ![](hello-ios-quickstart-images/image7new.png "Projektant dla systemu iOS")

  Należy pamiętać, że _klasy wielkości_ są domyślnie włączone. Zapoznaj się [Unified Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) przewodnika, aby dowiedzieć się więcej o nich.

8. W **konsoli przybornika**, wpisz "etykieta" na pasku wyszukiwania i przeciągnij **etykiety** na powierzchnię projektu (obszar na środku):

  ![](hello-ios-quickstart-images/image8new.png "Przeciągnij etykietę na powierzchni projektowej obszaru w Centrum")

  > [!NOTE]
  > Można wyświetlić **konsoli właściwości** lub **przybornika** w dowolnym momencie, przechodząc do **Widok > konsole**.

9. Uchwyty z *przeciąganie formanty* (kółka wokół formantu) i szersze etykietę:

  ![](hello-ios-quickstart-images/image9.png "Szersze etykietę")


10. Z **etykiety** zaznaczone na powierzchni projektu, użyj **konsoli właściwości** zmienić **tekst** właściwość **etykiety** do "Enter Phoneword: "

  ![](hello-ios-quickstart-images/image10.png "Etykieta do wprowadzania Phoneword zestawu")

11. Wyszukaj "pola tekstowego" wewnątrz przeciągania przybornika **pola tekstowego** z **przybornika** na projekt surface i umieść go w obszarze **etykiety**. Dopasuj szerokość do **pola tekstowego** jest szerokość **etykiety**:

  ![](hello-ios-quickstart-images/image12new.png "Wprowadź pola tekstowego Szerokość etykiety")


12. Z **pola tekstowego** zaznaczone na powierzchni projektu, zmień **pola tekstowego**w **nazwa** właściwości w sekcji tożsamości **konsoli właściwości** do `PhoneNumberText`i zmień **tekst** dla właściwości "1-855-XAMARIN":

  ![](hello-ios-quickstart-images/image13new.png "Zmień właściwości Title 1-855-XAMARIN")


13. Przeciągnij **przycisk** z **przybornika** na projekt surface i umieść go w obszarze **pola tekstowego**. Dostosuj szerokość więc **przycisk** szerokość całego **pola tekstowego** i **etykiety**:

  ![](hello-ios-quickstart-images/image14new.png "Dostosuj szerokość, tak aby szerokie, jak pole tekstowe, etykieta przycisku")


14. Z **przycisk** zaznaczone na powierzchni projektu, zmień **nazwa** właściwości w **tożsamości** sekcji **konsoli właściwości** do `TranslateButton`. Zmień **tytuł** dla właściwości "Przetłumacz":

  ![](hello-ios-quickstart-images/image15new.png "Zmień właściwości Title Przetłumacz")


15. Powtórz dwa kroki powyżej i przeciągnij **przycisk** z **przybornika** na projekt powierzchni i umieść go w katalogu pierwszej **przycisk**. Dostosuj szerokość więc **przycisk** szerokości jako pierwszy **przycisk**:

  ![](hello-ios-quickstart-images/image16new.png "Dostosuj szerokość tak szerokie, jak przycisku pierwszej jest przycisk")


16. Z drugiej **przycisk** zaznaczone na powierzchni projektu, zmień **nazwa** właściwości w **tożsamości** sekcji **konsoli właściwości**do `CallButton`. Zmień **tytuł** dla właściwości "Wywołanie":

  ![](hello-ios-quickstart-images/image17new.png "Zmień właściwości Title do wywołania")

  Zapisz zmiany, przechodząc do **Plik > Zapisz** lub naciskając klawisz **⌘ + s**.


17. Logikę musi ma zostać dodany do aplikacji do tłumaczenia numerów telefonów z alfanumeryczne liczbowych. Dodaj nowy plik do projektu, klikając prawym przyciskiem myszy **Phoneword_iOS** projektu w **konsoli rozwiązania** i wybierając polecenie **Dodaj > Nowy plik...**  lub naciskając klawisz **⌘ + n**:

  ![](hello-ios-quickstart-images/image18.png "Dodaj nowy plik do projektu")


18. W **nowy plik** okno dialogowe, wybierz opcję **ogólne > pustą klasę** i nazwę nowego pliku `PhoneTranslator`:

  ![](hello-ios-quickstart-images/image19.png "Wybierz klasę pusty i nazwę nowego pliku PhoneTranslator")


19. Tworzy nową, pustą C# klasy dla nas. Usuń kod szablonu i zastąp go następującym kodem:

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword_iOS
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

    Zapisz **PhoneTranslator.cs** plik i zamknij go.

20. Dodaj kod, aby okablować interfejsu użytkownika. W tym kliknij dwukrotnie na **ViewController.cs** w **konsoli rozwiązania** go otworzyć:

  ![](hello-ios-quickstart-images/image20new.png "Dodaj kod, aby okablować interfejsu użytkownika")


21. Rozpocznij od okablowania się `TranslateButton`. W **ViewController** klasy, Znajdź `ViewDidLoad` — metoda i Dodaj następujący kod pod `base.ViewDidLoad()` wywołania:

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(
            PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call ", UIControlState.Normal);
            CallButton.Enabled = false;
        } else {
            CallButton.SetTitle ("Call " + translatedNumber,
                UIControlState.Normal);
            CallButton.Enabled = true;
        }
    };
    ```

    Obejmują `using Phoneword_iOS;` Jeśli przestrzeń nazw plików jest inna.

22. Dodaj kod, który odpowiada użytkownikowi naciśnięcie przycisku drugi nosi nazwę `CallButton`. Umieść następujący kod poniżej kod `TranslateButton` i Dodaj `using Foundation;` na początku pliku:

    ```csharp
        CallButton.TouchUpInside += (object sender, EventArgs e) => {
            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl ("tel:" + translatedNumber);

            // ...otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl (url)) {
                var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                PresentViewController (alert, true, null);
            }
        };
    ```


23. Zapisz zmiany i późniejszego kompilowania aplikacji, wybierając **kompilacji > kompilacji wszystkich** lub naciskając klawisz **⌘ + B**.  Jeśli aplikacja kompiluje, w górnej części IDE pojawi się komunikat Powodzenie:

  ![](hello-ios-quickstart-images/image21.png "Komunikat z potwierdzeniem pojawi się w górnej części IDE")

  Jeśli wystąpią błędy, poprzednich kroków i usuń ewentualne błędy, dopóki aplikacja tworzy się pomyślnie.

27. Na koniec przetestować aplikację w **symulatora systemu iOS**. W górnym lewym rogu IDE, wybierz **debugowania** z pierwszej listy rozwijanej, i **iPhone 8 oraz iOS x.x** z drugiej listy rozwijanej i kliknij **Start** (trójkątny przycisku podobny przycisk Odtwórz):

  ![](hello-ios-quickstart-images/image27new.png "Naciśnij przycisk Start")

  > [!NOTE]
  > W chwili obecnej, ze względu na wymagania firmy Apple, może być konieczne certyfikatu deweloperskiego lub *tożsamości podpisywania* do kompilacji kodu dla urządzenie lub symulator. Postępuj zgodnie z instrukcjami [Inicjowanie obsługi administracyjnej urządzeń przewodnik](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) tej konfiguracji.

28. Spowoduje to uruchomienie aplikacji w narzędziu iOS Simulator:

  ![](hello-ios-quickstart-images/image28.png "Aplikacja była uruchomiona w symulatorze systemu iOS")

  Połączenia telefoniczne nie są obsługiwane w narzędziu iOS Simulator; Zamiast tego okna dialogowego alertu zostanie wyświetlony podczas próby połączenia:

  ![](hello-ios-quickstart-images/image29.png "Okna dialogowego alertu podczas próby połączenia")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-walkthrough"></a>Visual Studio Walkthrough

Ten przewodnik opisuje sposób tworzenia aplikacji o nazwie Phoneword, który tłumaczy numer telefonu alfanumeryczne pod numer telefonu liczbowych.

**Uwaga**: w tym przewodniku zastosowano Visual Studio Enterprise 2017 na maszynie wirtualnej systemu Windows 10. Konfiguracji może różnić się od tego, tak długo, jak spełnia wymagania powyżej, ale należy pamiętać, że niektóre zrzuty ekranu mogą różnić się do konfiguracji.

> [!NOTE]
> Przed kontynuowaniem w tym przewodniku, należy nawiązano już połączenie do komputera Mac w programie Visual Studio. Jest to spowodowane Xamarin.iOS zależy od firmy Apple narzędzi, aby skompilować i uruchomić projektanta i aplikacje dla systemu iOS. Aby pobrać, postępuj zgodnie z instrukcjami [pary Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) przewodnik.

1. Uruchom program Visual Studio z **Start** menu:

  ![](hello-ios-quickstart-images/image001-.png "Ekranu startowego")

  W polu wyszukiwania w obszarze **nowe rozwiązanie** wprowadź _jednej aplikacji widoku_i wybierz **pojedynczego widoku aplikacji (iPhone)** do tworzenia nowego rozwiązania Xamarin.iOS:

  ![](hello-ios-quickstart-images/image002-.png "Dodaj aplikację pojedynczego widoku")


2. Nazwij projekt i rozwiązanie `Phoneword`, jak pokazano poniżej:

  ![](hello-ios-quickstart-images/vs-image3.png "Nazwa PhonewordiOS projektu i nowych Phoneword rozwiązania")


3. Naciśnij klawisz **OK** do utworzenia nowego projektu

4. Upewnij się, że Xamarin Mac Agent ikony na pasku narzędzi jest zielony.

    ![Potwierdź, że Xamarin Mac Agent ikony na pasku narzędzi jest zielony](hello-ios-quickstart-images/vs-image4.png)

    Jeśli nie, oznacza to, że istnieje połączenie z hostem kompilacji Mac, postępuj zgodnie z instrukcjami [przewodnik konfiguracji](~/ios/get-started/installation/windows/connecting-to-mac/index.md) połączenia się.


5. Otwórz **Main.storyboard** pliku w systemie iOS projektanta przez dwukrotne kliknięcie w **Eksploratora rozwiązań**:

  ![](hello-ios-quickstart-images/vs-image7.png "Projektant dla systemu iOS")

6. Otwórz **przybornika** karcie, wpisz "etykieta" na pasku wyszukiwania i przeciągnij **etykiety** na powierzchnię projektu (obszar na środku):

  ![](hello-ios-quickstart-images/vs-image8.png "Przeciągnij etykietę na powierzchni projektowej obszaru w Centrum")


7. Następnie uchwyty z *przeciąganie formanty* i szersze etykietę:

  ![](hello-ios-quickstart-images/vs-image9.png "Szersze etykietę")


8. Z **etykiety** zaznaczone na powierzchni projektu, użyj **okna właściwości** zmienić **tekst** właściwość **etykiety** do "Enter Phoneword: "

  ![](hello-ios-quickstart-images/vs-image10.png "Zmień wartość właściwości tekst etykiety "Wprowadzić Phoneword"")

  > [!NOTE]
  > Można wyświetlić **właściwości** lub **przybornika** w dowolnym momencie, przechodząc do **widoku** menu.


9. Wyszukaj "pola tekstowego" wewnątrz przeciągania przybornika **pola tekstowego** z **przybornika** na projekt surface i umieść go w obszarze **etykiety**. Dopasuj szerokość do **pola tekstowego** jest szerokość **etykiety**:

  ![](hello-ios-quickstart-images/vs-image12.png "Dostosuj szerokość, dopóki pole tekstowe jest tej samej szerokości jako etykieta")


10. Z **pola tekstowego** zaznaczone na powierzchni projektu, zmień **pola tekstowego**w **nazwa** właściwości w sekcji tożsamości **właściwości**do `PhoneNumberText`i zmień **tekst** dla właściwości "1-855-XAMARIN":

  ![](hello-ios-quickstart-images/vs-image13.png "Zmień właściwości Text 1-855-XAMARIN")


11. Przeciągnij **przycisk** z **przybornika** na projekt surface i umieść go w obszarze **pola tekstowego**. Dostosuj szerokość więc **przycisk** szerokość całego **pola tekstowego** i **etykiety**:

  ![](hello-ios-quickstart-images/vs-image14.png "Dostosuj szerokość, tak aby szerokie, jak pole tekstowe, etykieta przycisku")


12. Z **przycisk** zaznaczone na powierzchni projektu, zmień **nazwa** właściwości w **tożsamości** sekcji **właściwości** do `TranslateButton`. Zmień **tytuł** dla właściwości "Przetłumacz":

  ![](hello-ios-quickstart-images/vs-image15.png "Zmień właściwości Title Przetłumacz")


13. Powtórz dwa poprzednie kroki i przeciągnij **przycisk** z **przybornika** na projekt powierzchni i umieść go w katalogu pierwszej **przycisk**. Dostosuj szerokość więc **przycisk** szerokości jako pierwszy **przycisk**:

  ![](hello-ios-quickstart-images/vs-image16.png "Dostosuj szerokość tak szerokie, jak przycisku pierwszej jest przycisk")


14. Z drugiej **przycisk** zaznaczone na powierzchni projektu, zmień **nazwa** właściwości w **tożsamości** sekcji **właściwości** do `CallButton`. Zmień **tytuł** dla właściwości "Wywołanie":

  ![](hello-ios-quickstart-images/vs-image17.png "Zmień właściwości Title do wywołania")

  Zapisz zmiany, przechodząc do **Plik > Zapisz wszystko** lub naciskając klawisz **Ctrl + s**.

15. Dodawanie kodu do tłumaczenia numerów telefonów z alfanumeryczne liczbowych. W tym celu należy najpierw dodać nowy plik do projektu przez kliknięcie prawym przyciskiem myszy **Phoneword** projektu w **Eksploratora rozwiązań** i wybierając polecenie **Dodaj > Nowy element...**  lub naciskając klawisz **Ctrl + Shift + A**:

  ![](hello-ios-quickstart-images/vs-image18.png "Dodawanie kodu do tłumaczenia numerów telefonów z alfanumeryczne numeryczne")


16. W **nowy plik** okno dialogowe, wybierz opcję **Apple > klasy** i nazwę nowego pliku `PhoneTranslator`:

  ![](hello-ios-quickstart-images/vs-image19.png "Dodaj nową klasę o nazwie PhoneTranslator")

  > [!IMPORTANT]
  > Upewnij się, że wybrano szablon "class", który ma C# w ikony. W przeciwnym razie nie można się odwoływać ta nowa klasa.


17. Spowoduje to utworzenie nowej klasy C#. Usuń kod szablonu i zastąp go następującym kodem:

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

  Zapisz **PhoneTranslator.cs** plik i zamknij go.

18. Kliknij dwukrotnie **ViewController.cs** w **Eksploratora rozwiązań** go otworzyć, dzięki czemu można dodać logikę do dojścia do interakcji z przycisków:

  ![](hello-ios-quickstart-images/vs-image20.png "Logika dodane do obsługi interakcji z przycisków")


19. Rozpocznij od okablowania się `TranslateButton`. W **ViewController** klasy, Znajdź `ViewDidLoad` metody. Dodaj następujący kod przycisk wewnątrz `ViewDidLoad`, podrzędne `base.ViewDidLoad()` wywołania:

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {

        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call", UIControlState.Normal);
            CallButton.Enabled = false;
            }
        else {
            CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
            CallButton.Enabled = true;
            }
    };
    ```
    Obejmują `using Phoneword;` Jeśli przestrzeń nazw plików jest inna.

20. Dodaj kod, który odpowiada użytkownikowi naciśnięcie przycisku drugi nosi nazwę `CallButton`. Umieść następujący kod poniżej kod `TranslateButton` i Dodaj `using Foundation;` na początku pliku:

    ```csharp
    CallButton.TouchUpInside += (object sender, EventArgs e) => {
        var url = new NSUrl ("tel:" + translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app,
            // otherwise show an alert dialog

        if (!UIApplication.SharedApplication.OpenUrl (url)) {
                        var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                        alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        PresentViewController (alert, true, null);
                    }
    };
    ```


21. Zapisz zmiany i późniejszego kompilowania aplikacji, wybierając **kompilacji > Kompiluj rozwiązanie** lub naciskając klawisz **Ctrl + Shift + B**.  Jeśli aplikacja kompiluje, komunikat z potwierdzeniem będą wyświetlane w dolnej części IDE:

  ![](hello-ios-quickstart-images/vs-image21.png "Komunikat z potwierdzeniem będą wyświetlane w dolnej części IDE")

  Jeśli wystąpią błędy, poprzednich kroków i usuń ewentualne błędy, dopóki aplikacja tworzy się pomyślnie.

22. Na koniec przetestować aplikację w **zdalny symulatora systemu iOS**. Na pasku narzędzi IDE, wybierz **debugowania** i **iPhone 8 oraz iOS x.x** z listy rozwijanej menu, a następnie naciśnij klawisz **Start** (zielonym trójkątem podobny przycisk Odtwórz):

  ![](hello-ios-quickstart-images/vs-image27.png "Naciśnij przycisk Start")

23. Spowoduje to uruchomienie aplikacji w narzędziu iOS Simulator:

  ![](hello-ios-quickstart-images/vs-image28.png "Aplikacja była uruchomiona w symulatorze systemu iOS")

  Połączenia telefoniczne nie są obsługiwane w narzędziu iOS Simulator; Zamiast tego okna dialogowego alertu zostaną wyświetlone podczas próby połączenia:

  ![](hello-ios-quickstart-images/vs-image29.png "Wyświetli okna dialogowego alertu podczas próby połączenia")

-----

Gratulujemy Kończenie pierwszej aplikacji platformy Xamarin.iOS!

Teraz nadszedł czas na dissect narzędzia i umiejętności wyświetlane w tym przewodniku w [Hello, iOS nowości](~/ios/get-started/hello-ios/hello-ios-deepdive.md).


## <a name="related-links"></a>Linki pokrewne

- [Ikony aplikacji platformy Xamarin i uruchamianie obrazów (przykład)](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)
- [Witaj, iOS (przykład)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [System iOS w portalu inicjowania obsługi](https://developer.apple.com/ios/manage/overview/index.action)
