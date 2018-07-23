---
title: 'Witaj, Android: Przewodnik Szybki Start'
description: W tym przewodniku legalną dwuczęściową spowoduje tworzenie pierwszej aplikacji platformy Xamarin.Android (programu Visual Studio lub Visual Studio dla komputerów Mac) i poznać podstawy tworzenia aplikacji dla systemu Android za pomocą platformy Xamarin. Po drodze zostaną wprowadzone do narzędzi, koncepcje i kroki wymagane do kompilowania i wdrażania aplikacji platformy Xamarin.Android.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 44007FA1-3ABC-4935-BF52-4613AF0553A6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/20/2018
ms.openlocfilehash: beb90587e0d720de7770056c8b51264099edecdc
ms.sourcegitcommit: fb55eba393e43bcc9e9d1fef9ef1f1310e99f620
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/21/2018
ms.locfileid: "39189024"
---
# <a name="hello-android-quickstart"></a>Witaj, Android: Przewodnik Szybki Start

_W tym przewodniku legalną dwuczęściową spowoduje tworzenie pierwszej aplikacji platformy Xamarin.Android (programu Visual Studio lub Visual Studio dla komputerów Mac) i poznać podstawy tworzenia aplikacji dla systemu Android za pomocą platformy Xamarin. Po drodze zostaną wprowadzone do narzędzi, koncepcje i kroki wymagane do kompilowania i wdrażania aplikacji platformy Xamarin.Android._

## <a name="hello-android-quickstart"></a>Witaj, przewodniku Szybki Start w systemie Android

W tym instruktażu utworzysz aplikację, która tłumaczy alfanumeryczne podany numer telefonu (przez użytkownika) pod numer telefonu liczbowych i wyświetlić numery telefonów liczbowych do użytkownika. Końcowy aplikacji wygląda następująco:

[![Zrzut ekranu aplikacji po jej ukończeniu](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)


## <a name="requirements"></a>Wymagania

Aby korzystać z tego przewodnika, potrzebne następujące elementy:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Windows 7 lub nowszy.

-   Visual Studio 2015 Professional lub nowszego.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Najnowszą wersję programu Visual Studio dla komputerów Mac.

-   OS X Yosemite lub nowszy.

-----

W tym przewodniku przyjęto założenie, że najnowszej wersji interfejsu Xamarin.Android jest zainstalowana i uruchomiona na wybraną platformę. Przewodnik dotyczący instalowania platformy Xamarin.Android, zapoznaj się [platformy Xamarin.android](~/android/get-started/installation/index.md) przewodników.
Przed rozpoczęciem pracy, Pobierz i Rozpakuj [ikony aplikacji platformy Xamarin i ekrany uruchamiania](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true) zestawu.

## <a name="configuring-emulators"></a>Konfigurowanie emulatorów

Jeśli używasz emulatora systemu Android, firma Microsoft zaleca, aby skonfigurować emulator, aby używał przyspieszania sprzętowego. Instrukcje dotyczące konfigurowania przyspieszanie sprzętowe są dostępne w [przyspieszanie sprzętowe wydajności emulatora](~/android/get-started/installation/android-emulator/hardware-acceleration.md).


## <a name="walkthrough"></a>Przewodnik

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Uruchom program Visual Studio.  Kliknij przycisk **Plik > Nowy > Projekt** Aby utworzyć nowy projekt.

W **nowy projekt** okno dialogowe, kliknij przycisk **aplikacji dla systemu Android** szablonu.
Nazwa nowego projektu `Phoneword`. Kliknij przycisk **OK**:

[![Nowy projekt jest Phoneword](hello-android-quickstart-images/vs/01-new-project-name-w157-sml.png)](hello-android-quickstart-images/vs/01-new-project-name-w157.png#lightbox)

W **nową aplikację systemu Android** okno dialogowe, kliknij przycisk **pusta aplikacja** i kliknij przycisk **OK** do utworzenia nowego projektu:

[![Wybierz szablon pusta aplikacja](hello-android-quickstart-images/vs/02-blank-app-w157-sml.png)](hello-android-quickstart-images/vs/02-blank-app-w157.png#lightbox)

### <a name="creating-the-layout"></a>Tworzenie układu

Po utworzeniu nowego projektu, rozwiń węzeł **zasobów** folder i następnie **układ** folderu w **Eksploratora rozwiązań**.
Kliknij dwukrotnie **activity_main.axml** aby otworzyć go w narzędzia Android Designer. Jest to plik układu ekranu aplikacji:

[![Otwórz main.axml działania](hello-android-quickstart-images/vs/04-open-layout-sml.png)](hello-android-quickstart-images/vs/04-open-layout.png#lightbox)

Z **przybornika** (obszar po lewej stronie), wprowadź `text` w polu wyszukiwania, a następnie przeciągnij **tekstu (duży)** widżetów na powierzchnię projektową (obszar na środku):

[![Dodaj widget duże pole tekstowe](hello-android-quickstart-images/vs/04-large-text-sml.png)](hello-android-quickstart-images/vs/04-large-text.png#lightbox)

Za pomocą **tekstu (duży)** formant zaznaczony na powierzchni projektowej, użyj **właściwości** okienko, aby zmienić `text` właściwość **tekstu (duży)** widgetu w celu `Enter a Phoneword:` jak pokazano poniżej:

[![Ustaw właściwości duże pole tekstowe](hello-android-quickstart-images/vs/05-enter-a-phoneword-sml.png)](hello-android-quickstart-images/vs/05-enter-a-phoneword.png#lightbox)

Przeciągnij **zwykły tekst** widgetu z **przybornika** projektu powierzchni i umieść ją poniżej **tekstu (duży)** elementu widget:

[![Dodaj widget w postaci zwykłego tekstu](hello-android-quickstart-images/vs/06-plain-text-sml.png)](hello-android-quickstart-images/vs/06-plain-text.png#lightbox)

Za pomocą **zwykły tekst** wybranego na powierzchni projektowej, użyj elementu widget **właściwości** okienko, aby zmienić `id` właściwość **zwykły tekst** widgetu w celu `@+id/PhoneNumberText`i zmień `text` właściwości `1-855-XAMARIN`:

[![Ustaw właściwości w postaci zwykłego tekstu](hello-android-quickstart-images/vs/07-add-properties-sml.png)](hello-android-quickstart-images/vs/07-add-properties.png#lightbox)

Przeciągnij **przycisk** z **przybornika** projektu powierzchni i umieść ją poniżej **zwykły tekst** elementu widget:

[![Przeciągnij tłumaczenie przycisku w projekcie](hello-android-quickstart-images/vs/08-drag-button-sml.png)](hello-android-quickstart-images/vs/08-drag-button.png#lightbox)

Za pomocą **przycisk** zaznaczone na powierzchni projektowej, użyj **właściwości** okienko, aby zmienić `id` właściwość **przycisk** do `@+id/TranslateButton` i zmień `text` właściwości `Translate`:

[![Zestaw translacji właściwości przycisku](hello-android-quickstart-images/vs/09-translate-button-sml.png)](hello-android-quickstart-images/vs/09-translate-button.png#lightbox)

Przeciągnij **TextView** z **przybornika** projektu powierzchni i umieść ją w obszarze **przycisk** elementu widget. Ustaw `id` właściwość **TextView** do `@+id/TranslatedPhoneWord` i zmień `text` na pusty ciąg:

[![Ustaw właściwości w widoku tekstu.](hello-android-quickstart-images/vs/10-textview-properties-sml.png)](hello-android-quickstart-images/vs/10-textview-properties.png#lightbox)    

Zapisz swoją pracę, naciskając klawisz **CTRL + S**.

### <a name="writing-translation-code"></a>Pisanie kodu tłumaczenia

Następnym krokiem jest dodawanie kodu do translacji numery telefonów z alfanumeryczne na liczbowy. Dodaj nowy plik do projektu, klikając prawym przyciskiem myszy **Phoneword** projektu w **Eksploratora rozwiązań** okienka i wybierając **Dodaj > Nowy element...**  jak pokazano poniżej:

[![Dodaj nowy element](hello-android-quickstart-images/vs/12-add-new-item-sml.png)](hello-android-quickstart-images/vs/12-add-new-item.png#lightbox)

W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# > Kod > plik kodu** i nadaj nowemu plikowi kodu **PhoneTranslator.cs**:

[![Add PhoneTranslator.cs](hello-android-quickstart-images/vs/14-add-class-sml-w157.png)](hello-android-quickstart-images/vs/14-add-class-w157.png#lightbox)

Spowoduje to utworzenie nowego pustego klasy C#. Wstaw następujący kod do tego pliku:

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

Czy zapisać zmiany **PhoneTranslator.cs** pliku, klikając **Plik > Zapisz** (lub naciskając **CTRL + S**), zamknij plik.

### <a name="wiring-up-the-interface"></a>Podłączanie do interfejsu

Następnym krokiem jest dodanie kodu w celu podłączenia do interfejsu użytkownika, wstawiając zapasowy kod z gałęzią `MainActivity` klasy. Rozpocznij od skonfigurowania okablowania się **Translate** przycisku. W `MainActivity` klasy, Znajdź `OnCreate` metody. Następnym krokiem jest dodanie kod przycisku wewnątrz `OnCreate`poniżej `base.OnCreate(bundle)` i `SetContentView
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

Pobierz odwołanie do formantów, które zostały utworzone w pliku układu za pomocą narzędzia Android Designer. Dodaj następujący kod wewnątrz `OnCreate` po wywołaniu metody `SetContentView`:

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

Dodaj kod, który reaguje na naciśnięcie przez użytkownika z **Translate** przycisku.
Dodaj następujący kod do `OnCreate` metody (wiersze dodane w poprzednim kroku):

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

Zapisz swoją pracę, wybierając **Plik > Zapisz wszystko** (lub naciskając **CTRL-SHIFT-S**) i utworzyć aplikację, wybierając **kompilacji > Kompiluj rozwiązanie** (lub naciskając klawisz **CTRL-SHIFT-B**). 

Jeśli występują błędy, przechodzą przez poprzednie kroki i Popraw wszelkie błędy, aż aplikacja zostanie pomyślnie skompilowana. Jeśli błąd kompilacji takich jak _zasób nie istnieje w bieżącym kontekście_, upewnij się, że nazwa przestrzeni nazw w **MainActivity.cs** jest zgodna z nazwą projektu (`Phoneword`) a następnie całkowicie ponownie skompiluj rozwiązanie. Jeśli nadal występują błędy kompilacji, sprawdź, czy zainstalowano najnowsze aktualizacje produktu Xamarin.Android.

### <a name="setting-the-label-and-app-icon"></a>Ustawienie etykiety i ikona aplikacji

Teraz masz działającą aplikację &ndash; nadszedł czas, aby dodać poprawek! W **MainActivity.cs**, Edytuj `Label` dla `MainActivity`. `Label` Jest systemu Android wyświetla w górnej części ekranu, aby powiadomić użytkowników, w którym znajdują się w aplikacji.
W górnej części `MainActivity` klasy, zmienić `Label` do `Phone Word` jak pokazano poniżej:

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

Teraz nadszedł czas, aby ustawić ikonę aplikacji. Domyślnie program Visual Studio zapewni ikona domyślna dla projektu. Teraz usunąć te pliki z rozwiązania i zastąpić inną ikonę. Rozwiń **zasobów** folderu w **konsoli rozwiązania**. Należy zauważyć, że istnieje pięć folderów, które mają prefiks **mipmappingu -**, i że każdy z tych folderów zawiera pojedynczy **Icon.png** pliku:

[![folderach mipmap i Icon.png plików](hello-android-quickstart-images/vs/21-mipmap-folders-sml.png)](hello-android-quickstart-images/vs/21-mipmap-folders.png#lightbox)

Należy usunąć te pliki ikon z projektu. Kliknij prawym przyciskiem myszy na każdym z **Icon.png** plików, a następnie wybierz **Usuń** z menu kontekstowego:
   
[![Usuń domyślną Icon.png](hello-android-quickstart-images/vs/21-delete-icon-sml.png)](hello-android-quickstart-images/vs/21-delete-icon.png#lightbox)
   
Kliknij pozycję **Usuń** przycisk w oknie dialogowym.

Następnie Pobierz i Rozpakuj [zestawu ikon aplikacji Xamarin](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Ten plik zip zawiera ikony dla aplikacji. Każda ikona jest wizualnie identyczne, ale w różnych rozdzielczościach jest poprawnie renderowany na różnych urządzeniach przy użyciu innego ekranu gęstości.  Zestaw plików muszą zostać skopiowane do projektu Xamarin.Android. W programie Visual Studio w **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **hdpi mipmappingu** i wybierz polecenie **Dodaj > istniejące elementy**:

[![Dodaj pliki](hello-android-quickstart-images/vs/22-add-files-sml.png)](hello-android-quickstart-images/vs/22-add-files.png#lightbox)

Z poziomu okna dialogowego wyboru przejdź do katalogu rozpakowany Xamarin AdApp ikon, a następnie otwórz **hdpi mipmappingu** folderu. Wybierz **Icon.png** i kliknij przycisk **Dodaj**.

Powtórz te kroki dla każdego z **mipmappingu -** folderów do zawartości **mipmappingu -** ikony aplikacji Xamarin foldery są kopiowane do ich odpowiedników **mipmappingu -** folderów w **Phoneword** projektu.

Po wszystkie ikony są kopiowane do projektu Xamarin.Android, otwórz **opcje projektu** okno dialogowe, klikając prawym przyciskiem myszy nad projektem w **konsoli rozwiązania**. Wybierz **kompilacji > Aplikacja dla systemu Android** i wybierz **@mipmap/icon** z **ikony aplikacji** pole kombi:

[![Ustawienie ikony projektu](hello-android-quickstart-images/vs/25-set-project-icon-sml.png)](hello-android-quickstart-images/vs/25-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>Uruchamianie aplikacji

Na koniec przetestować aplikację, uruchamiając go na urządzeniu z systemem Android lub w emulatorze i tłumaczenie Phoneword:

[![Zrzut ekranu aplikacji po jej ukończeniu](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)



# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Uruchom program Visual Studio dla komputerów Mac z **aplikacje** folderu lub **Spotlight**. 

Kliknij przycisk **nowe rozwiązanie...**  Aby utworzyć nowy projekt.

W **wybierz szablon dla nowego projektu** okno dialogowe, kliknij przycisk **Android > aplikacji** i wybierz **aplikacji dla systemu Android** szablonu. Kliknij przycisk **Dalej**.

[![Wybierz szablon aplikacji dla systemu Android](hello-android-quickstart-images/xs/03-choose-template-sml.png)](hello-android-quickstart-images/xs/03-choose-template.png#lightbox)

W **Konfiguruj aplikację systemu Android** okno dialogowe, nazwa nowej aplikacji `Phoneword` i kliknij przycisk **dalej**.

[![Konfigurowanie aplikacji dla systemu Android](hello-android-quickstart-images/xs/04-configure-android-app-sml.png)](hello-android-quickstart-images/xs/04-configure-android-app.png#lightbox)

W **Konfiguruj aplikację systemu Android** okno dialogowe, pozostaw wartość nazwy rozwiązania i projektu `Phoneword` i kliknij przycisk **Utwórz** do tworzenia projektu.

### <a name="creating-the-layout"></a>Tworzenie układu

Po utworzeniu nowego projektu, rozwiń węzeł **zasobów** folder i następnie **układ** folderu w **rozwiązania** konsoli.
Kliknij dwukrotnie **Main.axml** aby otworzyć go w narzędzia Android Designer. Jest to plik układu ekranu, gdy zostanie on wyświetlony w Projektancie dla systemu Android:

[![Otwórz Main.axml](hello-android-quickstart-images/xs/05-open-layout-sml.png)](hello-android-quickstart-images/xs/05-open-layout.png#lightbox)

Wybierz **Hello World, kliknij mnie!** **Przycisk** na powierzchni projektowej i naciśnij klawisz **Usuń** klawisz, aby go usunąć. 

Z **przybornika** (obszar po prawej stronie), wprowadź `text` w polu wyszukiwania, a następnie przeciągnij **tekstu (duży)** widżetów na powierzchnię projektową (obszar na środku):

[![Dodaj widget duże pole tekstowe](hello-android-quickstart-images/xs/06-large-text-sml.png)](hello-android-quickstart-images/xs/06-large-text.png#lightbox)

Za pomocą **tekstu (duży)** widżet wybrane na powierzchni projektowej, można użyć **właściwości** konsoli, aby zmienić `Text` właściwość **tekstu (duży)** widgetu w celu `Enter a Phoneword:` jak pokazano poniżej:

[![Ustaw duże pole tekstowe właściwości elementu widget](hello-android-quickstart-images/xs/07-enter-a-phoneword-sml.png)](hello-android-quickstart-images/xs/07-enter-a-phoneword.png#lightbox)

Następnie przeciągnij **zwykły tekst** widgetu z **przybornika** projektu powierzchni i umieść ją poniżej **tekstu (duży)** elementu widget. Zwróć uwagę, że można użyć pola wyszukiwania do lokalizowania elementów widget według nazwy:

[![Dodaj widget w postaci zwykłego tekstu](hello-android-quickstart-images/xs/08-plain-text-sml.png)](hello-android-quickstart-images/xs/08-plain-text.png#lightbox)

Za pomocą **zwykły tekst** widżet wybrane na powierzchni projektowej, można użyć **właściwości** konsoli, aby zmienić `Id` właściwość **zwykły tekst** widgetu w celu `@+id/PhoneNumberText` i zmień `Text` właściwości `1-855-XAMARIN`:

[![Właściwości elementu widget zwykły tekst](hello-android-quickstart-images/xs/09-add-properties-sml.png)](hello-android-quickstart-images/xs/09-add-properties.png#lightbox)

Przeciągnij **przycisk** z **przybornika** projektu powierzchni i umieść ją poniżej **zwykły tekst** elementu widget:

[![Dodawanie przycisku](hello-android-quickstart-images/xs/10-drag-button-sml.png)](hello-android-quickstart-images/xs/10-drag-button.png#lightbox)

Za pomocą **przycisk** zaznaczone na powierzchni projektowej, możesz użyć **właściwości** konsoli, aby zmienić `Id` właściwość **przycisk** do `@+id/TranslateButton` i Zmień `Text` właściwości `Translate`:

[![Skonfiguruj jako przycisk translate](hello-android-quickstart-images/xs/11-translate-button-sml.png)](hello-android-quickstart-images/xs/11-translate-button.png#lightbox)

Przeciągnij **TextView** z **przybornika** projektu powierzchni i umieść ją w obszarze **przycisk** elementu widget. Za pomocą **TextView** zaznaczone, ustaw `id` właściwość **TextView** do `@+id/TranslatedPhoneWord` i zmień `text` na pusty ciąg:

[![Ustaw właściwości w widoku tekstu.](hello-android-quickstart-images/xs/12-textview-properties-sml.png)](hello-android-quickstart-images/xs/12-textview-properties.png#lightbox)    

Zapisz swoją pracę, naciskając klawisz  **&#8984; + S**.

### <a name="writing-translation-code"></a>Pisanie kodu tłumaczenia

Teraz Dodaj działał kod służący do tłumaczenia numery telefonów z alfanumeryczne na liczbowy. Dodaj nowy plik do projektu, klikając ikonę koła zębatego obok **Phoneword** projektu w **rozwiązania** konsoli i wybierając **Dodaj > Nowy plik...** :

[![Dodaj nowy plik do projektu](hello-android-quickstart-images/xs/14-add-new-file-sml.png)](hello-android-quickstart-images/xs/14-add-new-file.png#lightbox)

W **nowy plik** okno dialogowe, wybierz opcję **ogólne > Pusta klasa**, nadaj nowemu plikowi **PhoneTranslator**i kliknij przycisk **New**. Spowoduje to utworzenie nowego pustego klasy C# dla nas.

Usuń cały kod szablonu w nowej klasie i zastąp go następującym kodem:

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

Czy zapisać zmiany **PhoneTranslator.cs** pliku, wybierając **Plik > Zapisz** (lub naciskając  **&#8984; + S**), zamknij plik. Upewnij się, że nie ma żadnych błędów kompilacji, ponownie skompilować rozwiązanie.

### <a name="wiring-up-the-interface"></a>Podłączanie do interfejsu

Następnym krokiem jest dodanie kodu w celu podłączenia do interfejsu użytkownika, dodając kod zapasowego do `MainActivity` klasy.
Kliknij dwukrotnie **MainActivity.cs** w **konsoli rozwiązania** aby go otworzyć.

Rozpocznij, dodając procedurę obsługi zdarzeń do **Translate** przycisku. W `MainActivity` klasy, Znajdź `OnCreate` metody. Dodaj kod przycisku wewnątrz `OnCreate`poniżej `base.OnCreate(bundle)` i `SetContentView (Resource.Layout.Main)` wywołania. Usuń wszystkie istniejące przycisk kodu obsługującego (czyli kodu, który odwołuje się do `Resource.Id.myButton` i tworzy program obsługi kliknięcie) tak, aby `OnCreate` metody jest podobny do następującego:

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

Następnie odwołanie jest wymagane do formantów, które zostały utworzone w pliku układu przy użyciu narzędzia Android Designer. Dodaj następujący kod wewnątrz `OnCreate` — metoda (po wywołaniu `SetContentView`):

```csharp
// Get our UI controls from the loaded layout
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
```

Dodaj kod, który reaguje na naciśnięcie przez użytkownika z **Translate** przycisku, dodając następujący kod do `OnCreate` metody (wiersze dodane w poprzednim kroku):

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

Zapisz swoją pracę i skompilować aplikację, wybierając **kompilacji > Tworzenie wszystkich** (lub naciskając  **&#8984; + B**). Jeśli aplikacja kompiluje, zostanie wyświetlony komunikat o powodzeniu, w górnej części programu Visual Studio dla komputerów Mac:

Jeśli występują błędy, przechodzą przez poprzednie kroki i Popraw wszelkie błędy, aż aplikacja zostanie pomyślnie skompilowana. Jeśli błąd kompilacji takich jak _zasób nie istnieje w bieżącym kontekście_, upewnij się, że nazwa przestrzeni nazw w **MainActivity.cs** jest zgodna z nazwą projektu (`Phoneword`) a następnie całkowicie ponownie skompiluj rozwiązanie. Jeśli nadal występują błędy kompilacji, sprawdź, czy zainstalowano najnowsze platformy Xamarin.Android, jak i aktualizacje programu Visual Studio dla komputerów Macintosh.

### <a name="setting-the-label-and-app-icon"></a>Ustawienie etykiety i ikona aplikacji

Skoro masz działającą aplikację, nadszedł czas na wprowadzanie końcowych poprawek! Rozpocznij, edytując `Label` dla `MainActivity`.
`Label` Jest systemu Android wyświetla w górnej części ekranu, aby powiadomić użytkowników, w którym znajdują się w aplikacji. W górnej części `MainActivity` klasy, zmienić `Label` do `Phone Word` jak pokazano poniżej:

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

Teraz nadszedł czas, aby ustawić ikonę aplikacji. Domyślnie program Visual Studio for Mac zapewni ikona domyślna dla projektu. Teraz usunąć te pliki z rozwiązania i zastąpić inną ikonę. Rozwiń **zasobów** folderu w **konsoli rozwiązania**. Należy zauważyć, że istnieje pięć folderów, które mają prefiks **mipmappingu -**, i że każdy z tych folderów zawiera pojedynczy **Icon.png** pliku:

[![folderach mipmap i Icon.png plików](hello-android-quickstart-images/xs/23-mipmap-folders-sml.png)](hello-android-quickstart-images/xs/23-mipmap-folders.png#lightbox)

Należy usunąć te pliki ikon z projektu. Kliknij prawym przyciskiem myszy na każdym z **Icon.png** plików, a następnie wybierz **Usuń** z menu kontekstowego:

[![Usuń domyślną Icon.png](hello-android-quickstart-images/xs/23-delete-icon-sml.png)](hello-android-quickstart-images/xs/23-delete-icon.png#lightbox)

Kliknij pozycję **Usuń** przycisk w oknie dialogowym.

Następnie Pobierz i Rozpakuj [zestawu ikon aplikacji Xamarin](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true). Ten plik zip zawiera ikony dla aplikacji. Każda ikona jest wizualnie identyczne, ale w różnych rozdzielczościach jest poprawnie renderowany na różnych urządzeniach przy użyciu innego ekranu gęstości.  Zestaw plików muszą zostać skopiowane do projektu Xamarin.Android. W programie Visual Studio dla komputerów Mac w **konsoli rozwiązania**, kliknij prawym przyciskiem myszy **hdpi mipmappingu** i wybierz polecenie **Dodaj > Dodaj pliki**:

[![Dodaj pliki](hello-android-quickstart-images/xs/24-add-files-sml.png)](hello-android-quickstart-images/xs/24-add-files.png#lightbox)

Z poziomu okna dialogowego wyboru przejdź do katalogu rozpakowany Xamarin AdApp ikon, a następnie otwórz **hdpi mipmappingu** folderu. Wybierz **Icon.png** i kliknij przycisk **Otwórz**.

W **Dodaj plik do folderu** okno dialogowe, wybierz opcję **skopiuj plik do katalogu** i kliknij przycisk **OK**:

[![Skopiuj plik do okna dialogowego katalogu](hello-android-quickstart-images/xs/26-copy-to-directory-sml.png)](hello-android-quickstart-images/xs/26-copy-to-directory.png#lightbox)

Powtórz te kroki dla każdego z **mipmappingu -** folderów do zawartości **mipmappingu -** ikony aplikacji Xamarin foldery są kopiowane do ich odpowiedników **mipmappingu -** folderów w **Phoneword** projektu.

Po wszystkie ikony są kopiowane do projektu Xamarin.Android, otwórz **opcje projektu** okno dialogowe, klikając prawym przyciskiem myszy nad projektem w **konsoli rozwiązania**. Wybierz **kompilacji > Aplikacja dla systemu Android** i wybierz **@mipmap/icon** z **ikony aplikacji** pole kombi:

[![Ustawienie ikony projektu](hello-android-quickstart-images/xs/28-set-project-icon-sml.png)](hello-android-quickstart-images/xs/28-set-project-icon.png#lightbox)

### <a name="running-the-app"></a>Uruchamianie aplikacji

Na koniec przetestować aplikację, uruchamiając go na urządzeniu z systemem Android lub w emulatorze i tłumaczenie Phoneword:

[![Zrzut ekranu aplikacji po jej ukończeniu](hello-android-quickstart-images/intro-app-examples-sml.png)](hello-android-quickstart-images/intro-app-examples.png#lightbox)

-----

Gratulujemy Kończenie pierwszej aplikacji platformy Xamarin.Android!
Teraz nadszedł czas na przeanalizujemy narzędzi i umiejętności, które zostały właśnie zaprezentowano. Zapasowa jest następnie [Witaj, Android Deep Dive](~/android/get-started/hello-android/hello-android-deepdive.md).


## <a name="related-links"></a>Linki pokrewne

- [Ikony aplikacji platformy Xamarin Android (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (przykład)](https://developer.xamarin.com/samples/monodroid/Phoneword)
