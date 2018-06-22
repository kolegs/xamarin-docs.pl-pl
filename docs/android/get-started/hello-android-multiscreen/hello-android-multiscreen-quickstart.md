---
title: 'Witaj, Android Wieloekranowy: Szybki Start'
description: W tym przewodniku dwuczęściową rozszerza Phoneword aplikacji do obsługi drugi ekranu. Na bieżąco podstawowe bloki konstrukcyjne aplikacji systemu Android są wprowadzane z bardziej zgłębić temat w architekturze systemu Android.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: ED99584A-BA3B-429A-AEE5-CF3CB0116762
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/30/2018
ms.openlocfilehash: d8f909ab522b5bbf08a2b666fd4f64340e60b3e5
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436934"
---
# <a name="hello-android-multiscreen-quickstart"></a>Witaj, Android Wieloekranowy: Szybki Start

_W tym przewodniku dwuczęściową rozszerza Phoneword aplikacji do obsługi drugi ekranu. Na bieżąco podstawowe bloki konstrukcyjne aplikacji systemu Android są wprowadzane z bardziej zgłębić temat w architekturze systemu Android._

## <a name="hello-android-multiscreen-quickstart"></a>Witaj, Android Wieloekranowy Szybki Start

W tym przewodniku części wskazówki dodasz drugi ekranu do [Phoneword](https://developer.xamarin.com/samples/monodroid/Phoneword/) aplikacji do rejestrowania historii numerów translacji przy użyciu aplikacji. [Końcowego aplikacji](https://developer.xamarin.com/samples/monodroid/PhonewordMultiscreen/) będzie mieć drugi ekranu, który wyświetla numery, które zostały "tłumaczone", jak pokazano na zrzucie ekranu po prawej stronie:

[![Zrzuty ekranu aplikacji przykład](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

Towarzyszącego [nowości](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md) sprawdza co został utworzony i zawiera omówienie architektury, nawigacji i innych nowych pojęć Android napotkał na bieżąco.


## <a name="requirements"></a>Wymagania

Ponieważ w tym przewodniku przejmuje where [Hello, Android](~/android/get-started/hello-android/index.md) pozostawia off, wymaga wprowadzenia [Hello, Android szybkiego startu](~/android/get-started/hello-android/hello-android-quickstart.md).
Jeśli chcesz przejść bezpośrednio do poniższe wskazówki, możesz pobrać ukończoną wersję [Phoneword](https://developer.xamarin.com/samples/monodroid/Phoneword/) (od Hello szybkiego startu dla systemu Android) i użyj go, aby uruchomić przewodnika.

## <a name="walkthrough"></a>Wskazówki

W tym przewodniku zostanie dodana **historii tłumaczenia** ekranu do **Phoneword** aplikacji.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Uruchamianie przez otwarcie **Phoneword** aplikacji w Visual Studio i edytowanie **Main.axml** plik z **Eksploratora rozwiązań**.

### <a name="updating-the-layout"></a>Aktualizowanie układu

Z **przybornika**, przeciągnij **przycisk** na projekt surface i umieść go poniżej **TranslatedPhoneWord** TextView. W **właściwości** okienka, zmień przycisk **identyfikator** do `@+id/TranslationHistoryButton` 

[![Przeciągnij nowy przycisk](hello-android-multiscreen-quickstart-images/vs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/vs/02-new-button.png#lightbox)

Ustaw **tekst** właściwość przycisk, aby `@string/translationHistory`. Android projektanta zinterpretuje to dosłownie, ale użytkownik chce wprowadź kilka zmian, tak aby tekst przycisku zostaną wyświetlone poprawnie:

[![Ustawianie tekstu przycisku historii tłumaczenia](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string-sml.png)](hello-android-multiscreen-quickstart-images/vs/03-translation-history-string.png#lightbox)

Rozwiń węzeł **wartości** węźle **zasobów** folderu w **Eksploratora rozwiązań** i kliknij dwukrotnie plik zasobów ciągu **Strings.xml**:

[![Open Strings.xml](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/04-strings-resources-file.png#lightbox)

Dodaj `translationHistory` ciągu nazwy i wartości do **Strings.xml** plik i zapisać go:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

**Historii tłumaczenia** tekst przycisku należy zaktualizować, aby odzwierciedlić nową wartość ciągu:

[![Przycisk odzwierciedla nową wartość ciągu](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png)](hello-android-multiscreen-quickstart-images/vs/05-new-string-value.png#lightbox)

Z **historii tłumaczenia** przycisk jest zaznaczony na powierzchni projektu, Znajdź `enabled` w **właściwości** okienko i ustaw dla niego wartość `false` wyłączenie przycisku. To spowoduje, że przycisk, aby stać się ciemniejszego na powierzchni projektu:

[![Wyłącz przycisk Historia tłumaczenia](hello-android-multiscreen-quickstart-images/vs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/vs/06-enabled-false.png#lightbox)

### <a name="creating-the-second-activity"></a>Tworzenie drugiego działania

Utworzyć drugi działanie zasilania drugi ekranu. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Phoneword** projekt i wybierz pozycję **Dodaj > Nowy element...** :

[![Dodaj nowy plik](hello-android-multiscreen-quickstart-images/vs/07-add-new-file-sml.png)](hello-android-multiscreen-quickstart-images/vs/07-add-new-file.png#lightbox)

W **Dodaj nowy element** okno dialogowe, wybierz **Visual C# > działania** i nazwij plik działania **TranslationHistoryActivity.cs**.

Zastąp kod szablonu w **TranslationHistoryActivity.cs** następującym kodem:

```csharp
using System;
using System.Collections.Generic;
using Android.App;
using Android.OS;
using Android.Widget;
namespace Phoneword
{
    [Activity(Label = "@string/translationHistory")]
    public class TranslationHistoryActivity : ListActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            // Create your application here
            var phoneNumbers = Intent.Extras.GetStringArrayList("phone_numbers") ?? new string[0];
            this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
        }
    }
}
```

W tej klasie, tworzysz `ListActivity` i wypełnianie jej programowo, więc nie trzeba utworzyć nowy plik układu dla tego działania. To jest omówiona bardziej szczegółowo w [Hello, Android Wieloekranowy nowości](~/android/get-started/hello-android/hello-android-deepdive.md).

### <a name="adding-translation-history-code"></a>Dodawanie kodu historii tłumaczenia

Numery telefonów, (tj. użytkownik został przetłumaczony na pierwszym ekranie) zbiera dane tej aplikacji i przekazuje je do drugiego ekranu. Numery telefonów są przechowywane jako lista ciągów. Obsługa list (i lokalizacji docelowych, które są używane później), Dodaj następujący `using` dyrektywy na początku **MainActivity.cs**:

```csharp
using System.Collections.Generic;
using Android.Content;
```

Następnie utwórz pustą listę, która może zostać wypełniony z numerami telefonów.
`MainActivity` Klasa będzie wyglądać następująco:

```csharp
[Activity(Label = "Phoneword", MainLauncher = true)]
public class MainActivity : Activity
{
    static readonly List<string> phoneNumbers = new List<string>();
    ...// OnCreate, etc.
}
```

W `MainActivity` klasy, Dodaj następujący kod, aby zarejestrować **historii tłumaczenia** przycisk (umieść ten wiersz po `translateButton` deklaracja):

```csharp
Button translationHistoryButton = FindViewById<Button> (Resource.Id.TranslationHistoryButton);
```

Dodaj następujący kod na końcu `OnCreate` metodę okablować się **historii tłumaczenia** przycisk:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", phoneNumbers);
    StartActivity(intent);
};
```

Aktualizacja **Przetłumacz** przycisk, aby dodać numer telefonu do listy `phoneNumbers`. `Click` Obsługę `TranslateHistoryButton` powinien przypominać następujący kod:

```csharp
// Add code to translate number
string translatedNumber = string.Empty;
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = "";
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
        phoneNumbers.Add(translatedNumber);
        translationHistoryButton.Enabled = true;
    }
};
```

Zapisz i tworzenia aplikacji, aby upewnić się, że nie ma żadnych błędów.

### <a name="running-the-app"></a>Aplikację

Wdróż aplikację na emulator lub urządzenie. Poniższe zrzuty ekranu przedstawiają działanie **Phoneword** aplikacji:

[![Przykład zrzuty ekranu](hello-android-multiscreen-quickstart-images/screenshot-sml.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Uruchamianie przez otwarcie **Phoneword** projektu programu Visual Studio for Mac i edytowanie **Main.axml** plik z **konsoli rozwiązania**.

### <a name="updating-the-layout"></a>Aktualizowanie układu

Z **przybornika**, przeciągnij **przycisk** na projekt surface i umieść go poniżej **TranslatedPhoneWord** TextView. W **właściwości** konsoli, zmień przycisk **identyfikator** do `@+id/TranslationHistoryButton` 

[![Przeciągnij nowy przycisk](hello-android-multiscreen-quickstart-images/xs/02-new-button-sml.png)](hello-android-multiscreen-quickstart-images/xs/02-new-button.png#lightbox)

Ustaw **tekst** właściwość przycisk, aby `@string/translationHistory`. Android projektanta zinterpretuje to dosłownie, ale użytkownik chce wprowadź kilka zmian, tak aby tekst przycisku zostaną wyświetlone poprawnie:

[![Ustawianie tekstu przycisku historii tłumaczenia](hello-android-multiscreen-quickstart-images/xs/03-call-history-string-sml.png)](hello-android-multiscreen-quickstart-images/xs/03-call-history-string.png#lightbox)


Rozwiń węzeł **wartości** węźle **zasobów** folderu w **konsoli rozwiązania** i kliknij dwukrotnie plik zasobów ciągu **Strings.xml**:

[![Otwórz ciągów](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file-sml.png)](hello-android-multiscreen-quickstart-images/xs/04-strings-resources-file.png#lightbox)


Dodaj `translationHistory` ciągu nazwy i wartości do **Strings.xml** plik i zapisać go:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Translation History</string>
    <string name="ApplicationName">Phoneword</string>
</resources>
```

**Historii tłumaczenia** tekst przycisku należy zaktualizować, aby odzwierciedlić nową wartość ciągu:

[![Przycisk odzwierciedla nową wartość ciągu](hello-android-multiscreen-quickstart-images/xs/05-new-string-value-sml.png)](hello-android-multiscreen-quickstart-images/xs/05-new-string-value.png#lightbox)


Z **historii tłumaczenia** przycisk wybranego na powierzchnię projektu, otwórz **zachowanie** karcie w **konsoli właściwości** i kliknij dwukrotnie **włączone**  pole wyboru, aby wyłączyć przycisku. To spowoduje, że przycisk, aby stać się ciemniejszego na powierzchni projektu:

[![Wyłącz przycisk Historia tłumaczenia](hello-android-multiscreen-quickstart-images/xs/06-enabled-false-sml.png)](hello-android-multiscreen-quickstart-images/xs/06-enabled-false.png#lightbox)

### <a name="creating-the-second-activity"></a>Tworzenie drugiego działania

Utworzyć drugi działanie zasilania drugi ekranu. W **konsoli rozwiązania**, kliknij szarego koło zębate ikonę **Phoneword** projekt i wybierz pozycję **Dodaj > Nowy plik...** :

Z **nowy plik** okno dialogowe, wybierz **Android > działania**, nazwa działania `TranslationHistoryActivity`, następnie kliknij przycisk **Dodaj**.

Zastąp kod szablonu w `TranslationHistoryActivity` następującym kodem:

```csharp
using System;
using System.Collections.Generic;
using Android.App;
using Android.OS;
using Android.Widget;
namespace Phoneword
{
    [Activity(Label = "@string/translationHistory")]
    public class TranslationHistoryActivity : ListActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            // Create your application here
            var phoneNumbers = Intent.Extras.GetStringArrayList("phone_numbers") ?? new string[0];
            this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
        }
    }
}
```

W tej klasie `ListActivity` jest tworzone i wypełniane programowo, więc nie trzeba utworzyć nowy plik układu dla tego działania. Jest to wyjaśnić bardziej szczegółowo w [Hello, Android Wieloekranowy nowości](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md).

### <a name="adding-translation-history-code"></a>Dodawanie kodu historii tłumaczenia

Numery telefonów, (tj. użytkownik został przetłumaczony na pierwszym ekranie) zbiera dane tej aplikacji i przekazuje je do drugiego ekranu. Numery telefonów są przechowywane jako lista ciągów. Obsługa list (i lokalizacji docelowych, które są używane później), Dodaj następujący `using` dyrektywy na początku **MainActivity.cs**:

```csharp
using System.Collections.Generic;
using Android.Content;
```

Następnie utwórz pustą listę, która może zostać wypełniony z numerami telefonów. `MainActivity` Klasa będzie wyglądać następująco:

```csharp
[Activity(Label = "Phoneword", MainLauncher = true)]
public class MainActivity : Activity
{
    static readonly List<string> phoneNumbers = new List<string>();
    ...// OnCreate, etc.
}
```

W `MainActivity` klasy, Dodaj następujący kod, aby zarejestrować **historii TranslationHistory** przycisk (umieść ten wiersz po `TranslationHistoryButton` deklaracja):

```csharp
Button translationHistoryButton = FindViewById<Button> (Resource.Id.TranslationHistoryButton);
```

Dodaj następujący kod na końcu `OnCreate` metodę okablować się **historii tłumaczenia** przycisk:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", phoneNumbers);
    StartActivity(intent);
};
```

Aktualizacja **Przetłumacz** przycisk, aby dodać numer telefonu do listy `phoneNumbers`. `Click` Obsługę `TranslateHistoryButton` powinien przypominać następujący kod:

```csharp
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = "";
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
        phoneNumbers.Add(translatedNumber);
        translationHistoryButton.Enabled = true;
    }
};
```

### <a name="running-the-app"></a>Aplikację

Wdróż aplikację na emulator lub urządzenie. Poniższe zrzuty ekranu przedstawiają działanie **Phoneword** aplikacji:

[![Przykład zrzuty ekranu](hello-android-multiscreen-quickstart-images/screenshot.png)](hello-android-multiscreen-quickstart-images/screenshot.png#lightbox)

-----

Gratulujemy Kończenie pierwszej aplikacji platformy Xamarin.Android wielu ekranu! Teraz nadszedł czas na dissect narzędzia i umiejętności właśnie znasz &ndash; maksymalnie jest następnie [Hello, Android Wieloekranowy nowości](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-deepdive.md).


## <a name="related-links"></a>Linki pokrewne

- [Ikony aplikacji platformy Xamarin i ekrany uruchamiania (ZIP)](https://github.com/xamarin/monodroid-samples/blob/master/Phoneword/Resources/XamarinAndroidIcons.zip?raw=true)
- [Phoneword (przykład)](https://developer.xamarin.com/samples/monodroid/Phoneword)
- [PhonewordMultiscreen (przykład)](https://developer.xamarin.com/samples/monodroid/PhonewordMultiscreen)
