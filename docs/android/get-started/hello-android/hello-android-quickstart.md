---
title: 'Witaj, Android: Szybki Start'
description: "W tym przewodniku dwuczęściową zostanie tworzenie pierwszej aplikacji platformy Xamarin.Android (z wykorzystaniem programu Visual Studio lub Visual Studio dla komputerów Mac) i zrozumienia podstaw dotyczących tworzenia aplikacji systemu Android za pomocą platformy Xamarin. Na bieżąco należy wprowadzić narzędzi, pojęcia i kroki wymagane do tworzenia i wdrażania aplikacji platformy Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 44007FA1-3ABC-4935-BF52-4613AF0553A6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: bbe243b108be6e0060ba9067db58a9875c7b5153
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2018
---
# <a name="hello-android-quickstart"></a>Witaj, Android: Szybki Start

_W tym przewodniku dwuczęściową zostanie tworzenie pierwszej aplikacji platformy Xamarin.Android (z wykorzystaniem programu Visual Studio lub Visual Studio dla komputerów Mac) i zrozumienia podstaw dotyczących tworzenia aplikacji systemu Android za pomocą platformy Xamarin. Na bieżąco należy wprowadzić narzędzi, pojęcia i kroki wymagane do tworzenia i wdrażania aplikacji platformy Xamarin.Android._

## <a name="hello-android-quickstart"></a>Witaj, Android Szybki Start

W tym przewodniku utworzysz aplikację, który tłumaczy numer telefonu alfanumeryczne (wprowadzony przez użytkownika) do numeru telefonu liczbowych i wyświetlany dla użytkownika numeru telefonu liczbowych. Końcowe aplikacji wygląda następująco:

[![Zrzut ekranu aplikacji po jej ukończeniu](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)


## <a name="requirements"></a>Wymagania

Aby skorzystać z tego przewodnika, potrzebne następujące elementy:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Windows 7 lub nowszy.

-   Program Visual Studio Professional 2015 lub nowszego.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Najnowszą wersję programu Visual Studio dla komputerów Mac.

-   OS X Yosemite lub nowszym.

-----

W tym przewodniku zakłada, że najnowszą wersję platformy Xamarin.Android jest zainstalowana i uruchomiona na platformie wyboru. Przewodnik dotyczący instalowania Xamarin.Android, zapoznaj się [instalacji platformy Xamarin.Android](~/android/get-started/installation/index.md) przewodników.
Przed rozpoczęciem pracy, Pobierz i Rozpakuj [ikony aplikacji platformy Xamarin i uruchomić ekrany](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true) ustawiony.

## <a name="configuring-emulators"></a>Konfigurowanie emulatory

Jeśli używasz emulatora zestawu SDK systemu Android firmy Google, zaleca się skonfigurowanie emulator używany przyspieszanie sprzętowe. Instrukcje dotyczące konfigurowania przyspieszanie sprzętowe są dostępne w [przyspieszanie sprzętowe](~/android/get-started/installation/android-emulator/hardware-acceleration.md).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Jeśli używasz programu Visual Studio Emulator systemu Android, funkcji Hyper-V musi być włączona w komputerze. Aby uzyskać więcej informacji na temat konfigurowania programu Visual Studio Emulator systemu Android, zobacz [wymagania systemowe dla programu Visual Studio Emulator for Android](https://msdn.microsoft.com/en-us/library/mt228280.aspx).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-----

## <a name="walkthrough"></a>Wskazówki

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Uruchom program Visual Studio.  Kliknij przycisk **Plik > Nowy > Projekt** do utworzenia nowego projektu.

W **nowy projekt** okna dialogowego, kliknij przycisk **pusta aplikacja (Android)** szablonu.
Nazwa nowego projektu `Phoneword`. Kliknij przycisk **OK** do utworzenia nowego projektu:

[![Nowy projekt jest Phoneword](hello-android-quickstart-images/vs/02-new-project-name-sml.png)](hello-android-quickstart-images/vs/02-new-project-name.png#lightbox)

### <a name="creating-the-layout"></a>Tworzenie układu

Po utworzeniu nowego projektu, rozwiń węzeł **zasobów** folder, a następnie **układu** folderu w **Eksploratora rozwiązań**.
Kliknij dwukrotnie **Main.axml** aby otworzyć go w Projektancie systemu Android. To jest plik układu do ekranu aplikacji:

[![Otwórz Main.axml](hello-android-quickstart-images/vs/04-open-layout-sml.png)](hello-android-quickstart-images/vs/04-open-layout.png#lightbox)

Z **przybornika** (obszar po lewej stronie), wprowadź `text` do pola wyszukiwania i przeciągnij **tekst (duży)** elementu widget na powierzchnię projektu (obszar na środku):

[![Dodawanie elementu widget duży](hello-android-quickstart-images/vs/04-large-text-sml.png)](hello-android-quickstart-images/vs/04-large-text.png#lightbox)

Z **tekst (duży)** formant zaznaczony na powierzchni projektu, użyj **właściwości** okienko, aby zmienić `text` właściwość **tekst (duży)** elementu widget do `Enter a Phoneword:` w sposób pokazany poniżej:

[![Ustaw właściwości duży](hello-android-quickstart-images/vs/05-enter-a-phoneword-sml.png)](hello-android-quickstart-images/vs/05-enter-a-phoneword.png#lightbox)

Przeciągnij **zwykły tekst** widżet z **przybornika** projektu surface i umieść go pod **tekst (duży)** elementu widget:

[![Dodawanie elementu widget w postaci zwykłego tekstu](hello-android-quickstart-images/vs/06-plain-text-sml.png)](hello-android-quickstart-images/vs/06-plain-text.png#lightbox)

Z **zwykły tekst** wybranego na powierzchnię projektu, użyj elementu widget **właściwości** okienko, aby zmienić `id` właściwość **zwykły tekst** elementu widget do `@+id/PhoneNumberText`i zmień `text` właściwości `1-855-XAMARIN`:

[![Ustawianie właściwości w postaci zwykłego tekstu](hello-android-quickstart-images/vs/07-add-properties-sml.png)](hello-android-quickstart-images/vs/07-add-properties.png#lightbox)

Przeciągnij **przycisk** z **przybornika** projektu surface i umieść go pod **zwykły tekst** elementu widget:

[![Przeciągnij tłumaczenie przycisku do projektu](hello-android-quickstart-images/vs/08-drag-button-sml.png)](hello-android-quickstart-images/vs/08-drag-button.png#lightbox)

Z **przycisk** zaznaczone na powierzchni projektu, użyj **właściwości** okienko, aby zmienić `id` właściwość **przycisk** do `@+id/TranslateButton` i zmień `text` właściwości `Translate`:

[![Zestaw tłumaczenie właściwości przycisku](hello-android-quickstart-images/vs/09-translate-button-sml.png)](hello-android-quickstart-images/vs/09-translate-button.png#lightbox)

Przeciągnij **TextView** z **przybornika** projektu surface i umieść go w obszarze **przycisk** elementu widget. Ustaw `id` właściwość **TextView** do `@+id/TranslatedPhoneWord` i zmień `text` na pusty ciąg:

[![Ustaw właściwości w widoku tekstu.](hello-android-quickstart-images/vs/10-textview-properties-sml.png)](hello-android-quickstart-images/vs/10-textview-properties.png#lightbox)    

Zapisz swoją pracę, naciskając klawisz **CTRL + S**.

### <a name="writing-translation-code"></a>Pisanie kodu tłumaczenia

Następnym krokiem jest dodać kod do tłumaczenia numerów telefonów z alfanumeryczne liczbowych. Dodaj nowy plik do projektu, klikając prawym przyciskiem myszy **Phoneword** projektu w **Eksploratora rozwiązań** okienko i wybierając polecenie **Dodaj > Nowy element...**  w sposób przedstawiony poniżej:

[![Dodaj nowy element](hello-android-quickstart-images/vs/12-add-new-item-sml.png)](hello-android-quickstart-images/vs/12-add-new-item.png#lightbox)

W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > kod** i nazwę nowego pliku kodu **PhoneTranslator.cs**:

[![Add PhoneTranslator.cs](hello-android-quickstart-images/vs/14-add-class-sml.png)](hello-android-quickstart-images/vs/14-add-class.png#lightbox)

Spowoduje to utworzenie nowego pustego C# klasy. Wstaw następujący kod do tego pliku:

```csharp
using System.Text;
using System;
namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return "";
            else
                raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                {
                    newNumber.Append(c);
                }
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
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
            if ("ABC".Contains(c))
                return 2;
            else if ("DEF".Contains(c))
                return 3;
            else if ("GHI".Contains(c))
                return 4;
            else if ("JKL".Contains(c))
                return 5;
            else if ("MNO".Contains(c))
                return 6;
            else if ("PQRS".Contains(c))
                return 7;
            else if ("TUV".Contains(c))
                return 8;
            else if ("WXYZ".Contains(c))
                return 9;
            return null;
        }
    }
}
```

Zapisać zmiany w **PhoneTranslator.cs** plików przez kliknięcie przycisku **Plik > Zapisz** (lub naciskając klawisz **CTRL + S**), zamknij plik.

### <a name="wiring-up-the-interface"></a>Podłączanie interfejsu

Następnym krokiem jest Dodaj kod, aby okablować interfejsu użytkownika, podając kod zapasowego do `MainActivity` klasy. Rozpocznij od okablowania się **Przetłumacz** przycisku. W `MainActivity` klasy, Znajdź `OnCreate` metody. Następnym krokiem jest dodanie kodu przycisk wewnątrz `OnCreate`, poniżej `base.OnCreate(bundle)` i `SetContentView
(Resource.Layout.Main)` wywołania. Najpierw należy zmodyfikować kod szablonu, aby `OnCreate` metody jest podobny do następującego:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Widget;
using Android.OS;

namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // New code will go here
        }
    }
}
```

Odwołać się do formantów, które zostały utworzone w pliku układu za pomocą projektanta dla systemu Android. Dodaj następujący kod wewnątrz `OnCreate` po wywołaniu metody `SetContentView`:

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

Dodaj kod, który reaguje na naciśnięcie przez użytkownika z **Przetłumacz** przycisku.
Dodaj następujący kod do `OnCreate` — metoda (po wierszach dodanym w poprzednim kroku):

```csharp
// Add code to translate number
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    string translatedNumber = Core.PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

Zapisz swoją pracę, wybierając **Plik > Zapisz wszystko** (lub naciskając klawisz **CTRL-SHIFT-S**) i kompilowania aplikacji przez wybranie **kompilacji > Kompiluj ponownie rozwiązanie** (lub naciskając klawisz **CTRL-SHIFT-B**). 

Jeśli wystąpią błędy, poprzednich kroków i usuń ewentualne błędy, dopóki aplikacja tworzy się pomyślnie. Jeśli błąd kompilacji takich jak _zasób nie istnieje w bieżącym kontekście_, upewnij się, że nazwa przestrzeni nazw w **MainActivity.cs** jest zgodna z nazwą projektu (`Phoneword`), a następnie całkowicie ponownie skompiluj rozwiązanie. Jeśli nadal występują błędy kompilacji, sprawdź, czy zostały zainstalowane najnowsze aktualizacje platformy Xamarin.Android.

### <a name="setting-the-label-and-app-icon"></a>Ustawianie etykiety i ikona aplikacji

Teraz powinien mieć działającą aplikację &ndash; nadszedł czas, aby dodać ostateczne poprawki! W **MainActivity.cs**, Edytuj `Label` dla `MainActivity`. `Label` Jest Android wyświetli w górnej części ekranu, aby powiadomić użytkowników, gdy są w aplikacji.
W górnej części `MainActivity` klasy, zmień `Label` do `Phone Word` w sposób pokazany poniżej:

```csharp
namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        ...
    }
}
```

Teraz nadszedł czas, aby ustawić ikonę aplikacji. Domyślnie program Visual Studio zapewni domyślną ikonę dla projektu. Umożliwia usunięcie tych plików z rozwiązania i zastąp je z inną ikonę. Rozwiń węzeł **zasobów** folderu w **konsoli rozwiązania**. Są pięć folderów, które mają przedrostek **mipmap -**, i czy każdy z tych folderów zawiera jeden **Icon.png** pliku:

[![folderach mipmap i Icon.png plików](hello-android-quickstart-images/vs/21-mipmap-folders-sml.png)](hello-android-quickstart-images/vs/21-mipmap-folders.png#lightbox)

Należy usunąć te pliki ikon z projektu. Kliknij prawym przyciskiem myszy na każdym z **Icon.png** plików, a następnie wybierz **usunąć** z menu kontekstowego:
   
[![Usuwanie domyślnej Icon.png](hello-android-quickstart-images/vs/21-delete-icon-sml.png)](hello-android-quickstart-images/vs/21-delete-icon.png#lightbox)
   
Polecenie **usunąć** przycisk w oknie dialogowym.

Następnie Pobierz i Rozpakuj [ustawić ikony aplikacji platformy Xamarin](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Ten plik zip zawiera ikony dla aplikacji. Każda z ikon jest wizualnie identyczne, ale na inną rozdzielczość renderowania poprawnie na różnych urządzeniach za pomocą gęstości inny ekran.  Zestaw plików musi być skopiowany do projektu platformy Xamarin.Android. W programie Visual Studio w **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **mipmap hdpi** i wybierz polecenie **Dodaj > istniejące elementy**:

[![Dodawanie plików](hello-android-quickstart-images/vs/22-add-files-sml.png)](hello-android-quickstart-images/vs/22-add-files.png#lightbox)

Z okna dialogowego wyboru, przejdź do katalogu Xamarin AdApp ikony rozpakowane, a następnie otwórz **mipmap hdpi** folderu. Wybierz **Icon.png** i kliknij przycisk **Dodaj**.

Powtórz te kroki dla każdego z **mipmap -** folderów do zawartości **mipmap -** ikony aplikacji platformy Xamarin folderów zostają skopiowane do ich odpowiednika **mipmap -** folderów w **Phoneword** projektu.

Po skopiowaniu wszystkie ikony do projektu platformy Xamarin.Android, otwórz **opcje projektu** okno dialogowe, klikając prawym przyciskiem myszy na projekt w **konsoli rozwiązania**. Wybierz **kompilacji > aplikacji systemu Android** i wybierz  **@mipmap/icon**  z **ikona aplikacji** pola kombi:

[![Ustawienie ikona projektu](hello-android-quickstart-images/vs/25-set-project-icon-sml.png)](hello-android-quickstart-images/vs/25-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>Aplikację

Na koniec przetestować aplikację, uruchamiając go na urządzeniu z systemem Android lub w emulatorze, a translacja Phoneword:

[![Zrzut ekranu aplikacji po jej ukończeniu](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Uruchom program Visual Studio for Mac z **aplikacji** folderu lub **Spotlight**. 

Kliknij przycisk **nowe rozwiązanie...**  do utworzenia nowego projektu.

W **wybierz szablon dla nowego projektu** okna dialogowego, kliknij przycisk **Android > aplikacji** i wybierz **aplikacji systemu Android** szablonu. Kliknij przycisk **Dalej**.

[![Wybierz szablon aplikacji systemu Android](hello-android-quickstart-images/xs/03-choose-template-sml.png)](hello-android-quickstart-images/xs/03-choose-template.png#lightbox)

W **Konfigurowanie aplikacji systemu Android** okna dialogowego, nazwę nowej aplikacji `Phoneword` i kliknij przycisk **dalej**.

[![Konfigurowanie aplikacji systemu Android](hello-android-quickstart-images/xs/04-configure-android-app-sml.png)](hello-android-quickstart-images/xs/04-configure-android-app.png#lightbox)

W **Konfigurowanie nowej aplikacji systemu Android** okno dialogowe, pozostaw ustawioną nazwy rozwiązania i projektu `Phoneword` i kliknij przycisk **Utwórz** Aby utworzyć projekt.

### <a name="creating-the-layout"></a>Tworzenie układu

Po utworzeniu nowego projektu, rozwiń węzeł **zasobów** folder, a następnie **układu** folderu w **rozwiązania** konsoli.
Kliknij dwukrotnie **Main.axml** aby otworzyć go w Projektancie systemu Android. To jest plik układu dla ekranu, gdy zostanie on wyświetlony w Projektancie Android:

[![Otwórz Main.axml](hello-android-quickstart-images/xs/05-open-layout-sml.png)](hello-android-quickstart-images/xs/05-open-layout.png#lightbox)

Wybierz **mnie kliknij pozycję Witaj świecie!** **Przycisk** na powierzchni projektu i naciśnij klawisz **usunąć** klucz do usunięcia go. 

Z **przybornika** (obszar po prawej stronie), wprowadź `text` do pola wyszukiwania i przeciągnij **tekst (duży)** elementu widget na powierzchnię projektu (obszar na środku):

[![Dodawanie elementu widget duży](hello-android-quickstart-images/xs/06-large-text-sml.png)](hello-android-quickstart-images/xs/06-large-text.png#lightbox)

Z **tekst (duży)** widget wybrane na powierzchni projektu, można użyć **właściwości** konsoli, aby zmienić `Text` właściwość **tekst (duży)** elementu widget do `Enter a Phoneword:` w sposób przedstawiony poniżej:

[![Właściwości elementu widget duży](hello-android-quickstart-images/xs/07-enter-a-phoneword-sml.png)](hello-android-quickstart-images/xs/07-enter-a-phoneword.png#lightbox)

Następnie przeciągnij **zwykły tekst** widżet z **przybornika** projektu surface i umieść go pod **tekst (duży)** elementu widget. Zwróć uwagę, że można użyć pola wyszukiwania do lokalizowania elementy widget według nazwy:

[![Dodawanie elementu widget w postaci zwykłego tekstu](hello-android-quickstart-images/xs/08-plain-text-sml.png)](hello-android-quickstart-images/xs/08-plain-text.png#lightbox)

Z **zwykły tekst** widget wybrane na powierzchni projektu, można użyć **właściwości** konsoli, aby zmienić `Id` właściwość **zwykły tekst** elementu widget do `@+id/PhoneNumberText` i zmienić `Text` właściwości `1-855-XAMARIN`:

[![Właściwości elementu widget zwykłego tekstu](hello-android-quickstart-images/xs/09-add-properties-sml.png)](hello-android-quickstart-images/xs/09-add-properties.png#lightbox)

Przeciągnij **przycisk** z **przybornika** projektu surface i umieść go pod **zwykły tekst** elementu widget:

[![Dodawanie przycisku](hello-android-quickstart-images/xs/10-drag-button-sml.png)](hello-android-quickstart-images/xs/10-drag-button.png#lightbox)

Z **przycisk** zaznaczone na powierzchni projektu, można użyć **właściwości** konsoli, aby zmienić `Id` właściwość **przycisk** do `@+id/TranslateButton` i Zmień `Text` właściwości `Translate`:

[![Skonfiguruj jako przycisk Przetłumacz](hello-android-quickstart-images/xs/11-translate-button-sml.png)](hello-android-quickstart-images/xs/11-translate-button.png#lightbox)

Przeciągnij **TextView** z **przybornika** projektu surface i umieść go w obszarze **przycisk** elementu widget. Z **TextView** zaznaczone, ustaw `id` właściwość **TextView** do `@+id/TranslatedPhoneWord` i zmień `text` na pusty ciąg:

[![Ustaw właściwości w widoku tekstu.](hello-android-quickstart-images/xs/12-textview-properties-sml.png)](hello-android-quickstart-images/xs/12-textview-properties.png#lightbox)    

Zapisz swoją pracę, naciskając klawisz  **&#8984; + S**.

### <a name="writing-translation-code"></a>Pisanie kodu tłumaczenia

Teraz Dodaj kod służący do tłumaczenia numerów telefonów z alfanumeryczne liczbowych. Dodaj nowy plik do projektu, klikając ikonę Koło zębate obok **Phoneword** projektu w **rozwiązania** konsoli i wybierając polecenie **Dodaj > Nowy plik...** :

[![Dodaj nowy plik do projektu](hello-android-quickstart-images/xs/14-add-new-file-sml.png)](hello-android-quickstart-images/xs/14-add-new-file.png#lightbox)

W **nowy plik** okno dialogowe, wybierz opcję **ogólne > pustą klasę**, podaj nazwę nowego pliku **PhoneTranslator**i kliknij przycisk **nowy**. Tworzy nowy pusty C# klasy dla nas.

Usuń wszystkie kod szablonu w nowej klasy i zastąp go następującym kodem:

```csharp
using System.Text;
using System;
namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return "";
            else
                raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                {
                    newNumber.Append(c);
                }
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
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
            if ("ABC".Contains(c))
                return 2;
            else if ("DEF".Contains(c))
                return 3;
            else if ("GHI".Contains(c))
                return 4;
            else if ("JKL".Contains(c))
                return 5;
            else if ("MNO".Contains(c))
                return 6;
            else if ("PQRS".Contains(c))
                return 7;
            else if ("TUV".Contains(c))
                return 8;
            else if ("WXYZ".Contains(c))
                return 9;
            return null;
        }
    }
}
```

Zapisać zmiany w **PhoneTranslator.cs** pliku, wybierając **Plik > Zapisz** (lub naciskając klawisz  **&#8984; + S**), zamknij plik. Upewnij się, że po ponownym utworzeniu rozwiązania nie ma żadnych błędów kompilacji.

### <a name="wiring-up-the-interface"></a>Podłączanie interfejsu

Następnym krokiem jest Dodaj kod, aby okablować interfejsu użytkownika przez dodanie kodu zapasowego do `MainActivity` klasy.
Kliknij dwukrotnie **MainActivity.cs** w **konsoli rozwiązania** go otworzyć.

Rozpocznij od Dodawanie obsługi zdarzeń do **Przetłumacz** przycisku. W `MainActivity` klasy, Znajdź `OnCreate` metody. Dodaj kod przycisk wewnątrz `OnCreate`, poniżej `base.OnCreate(bundle)` i `SetContentView (Resource.Layout.Main)` wywołania. Usuń kod obsługi przycisk szablonu, aby `OnCreate` metody jest podobny do następującego:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;

namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Our code will go here
        }
    }
}
```

Następnie odwołanie jest potrzebny do formantów, które zostały utworzone w pliku układu projektanta dla systemu Android. Dodaj następujący kod wewnątrz `OnCreate` — metoda (po wywołaniu `SetContentView`):

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

Dodaj kod, który reaguje na naciśnięcie przez użytkownika z **Przetłumacz** przycisku, dodając następujący kod do `OnCreate` — metoda (po wierszach dodanym w ostatnim kroku):

```csharp
// Add code to translate number
string translatedNumber = string.Empty;

translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

Zapisz swoją pracę i kompilowania aplikacji przez wybranie **kompilacji > kompilacji wszystkich** (lub naciskając klawisz  **&#8984; + B**). Jeśli aplikacja kompiluje, otrzymasz komunikat o powodzeniu w górnej części programu Visual Studio dla komputerów Mac:

Jeśli wystąpią błędy, poprzednich kroków i usuń ewentualne błędy, dopóki aplikacja tworzy się pomyślnie. Jeśli błąd kompilacji takich jak _zasób nie istnieje w bieżącym kontekście_, upewnij się, że nazwa przestrzeni nazw w **MainActivity.cs** jest zgodna z nazwą projektu (`Phoneword`), a następnie całkowicie ponownie skompiluj rozwiązanie. Jeśli nadal występują błędy kompilacji, sprawdź, czy zainstalowano najnowsze platformy Xamarin.Android i aktualizacji programu Visual Studio for Mac.

### <a name="setting-the-label-and-app-icon"></a>Ustawianie etykiety i ikona aplikacji

Teraz, gdy masz działającą aplikację, to czas, aby dodać ostateczne poprawki! Rozpocznij od edycji `Label` dla `MainActivity`.
`Label` Jest Android wyświetli w górnej części ekranu, aby powiadomić użytkowników, gdy są w aplikacji. W górnej części `MainActivity` klasy, zmień `Label` do `Phone Word` w sposób pokazany poniżej:

```csharp
namespace Phoneword
{
    [Activity (Label = "Phone Word", MainLauncher = true)]
    public class MainActivity : Activity
    {
        ...
    }
}
```

Teraz nadszedł czas, aby ustawić ikonę aplikacji. Domyślnie program Visual Studio for Mac zapewni domyślną ikonę dla projektu. Umożliwia usunięcie tych plików z rozwiązania i zastąp je z inną ikonę. Rozwiń węzeł **zasobów** folderu w **konsoli rozwiązania**. Są pięć folderów, które mają przedrostek **mipmap -**, i czy każdy z tych folderów zawiera jeden **Icon.png** pliku:

[![folderach mipmap i Icon.png plików](hello-android-quickstart-images/xs/23-mipmap-folders-sml.png)](hello-android-quickstart-images/xs/23-mipmap-folders.png#lightbox)

Należy usunąć te pliki ikon z projektu. Kliknij prawym przyciskiem myszy na każdym z **Icon.png** plików, a następnie wybierz **Usuń** z menu kontekstowego:

[![Usuwanie domyślnej Icon.png](hello-android-quickstart-images/xs/23-delete-icon-sml.png)](hello-android-quickstart-images/xs/23-delete-icon.png#lightbox)

Polecenie **usunąć** przycisk w oknie dialogowym.

Następnie Pobierz i Rozpakuj [ustawić ikony aplikacji platformy Xamarin](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Ten plik zip zawiera ikony dla aplikacji. Każda z ikon jest wizualnie identyczne, ale na inną rozdzielczość renderowania poprawnie na różnych urządzeniach za pomocą gęstości inny ekran.  Zestaw plików musi być skopiowany do projektu platformy Xamarin.Android. W programie Visual Studio dla komputerów Mac w **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **mipmap hdpi** i wybierz polecenie **Dodaj > Dodaj pliki**:

[![Dodawanie plików](hello-android-quickstart-images/xs/24-add-files-sml.png)](hello-android-quickstart-images/xs/24-add-files.png#lightbox)

Z okna dialogowego wyboru, przejdź do katalogu Xamarin AdApp ikony rozpakowane, a następnie otwórz **mipmap hdpi** folderu. Wybierz **Icon.png** i kliknij przycisk **Otwórz**.

W **Dodaj plik do folderu** okno dialogowe, wybierz opcję **skopiuj plik do katalogu** i kliknij przycisk **OK**:

[![Skopiuj plik do katalogu okna dialogowego](hello-android-quickstart-images/xs/26-copy-to-directory-sml.png)](hello-android-quickstart-images/xs/26-copy-to-directory.png#lightbox)

Powtórz te kroki dla każdego z **mipmap -** folderów do zawartości **mipmap -** ikony aplikacji platformy Xamarin folderów zostają skopiowane do ich odpowiednika **mipmap -** folderów w **Phoneword** projektu.

Po skopiowaniu wszystkie ikony do projektu platformy Xamarin.Android, otwórz **opcje projektu** okno dialogowe, klikając prawym przyciskiem myszy na projekt w **konsoli rozwiązania**. Wybierz **kompilacji > aplikacji systemu Android** i wybierz  **@mipmap/icon**  z **ikona aplikacji** pola kombi:

[![Ustawienie ikona projektu](hello-android-quickstart-images/xs/28-set-project-icon-sml.png)](hello-android-quickstart-images/xs/28-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>Aplikację

Na koniec przetestować aplikację, uruchamiając go na urządzeniu z systemem Android lub w emulatorze, a translacja Phoneword:

[![Zrzut ekranu aplikacji po jej ukończeniu](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)

-----

Gratulujemy Kończenie pierwszej aplikacji platformy Xamarin.Android!
Teraz nadszedł czas na dissect narzędzia i umiejętności, które właśnie znasz. Następnie maksymalnie jest [Hello, Android nowości](~/android/get-started/hello-android/hello-android-deepdive.md).


## <a name="related-links"></a>Linki pokrewne

- [Ikony aplikacji dla systemu Xamarin Android (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (przykład)](https://developer.xamarin.com/samples/monodroid/Phoneword)
