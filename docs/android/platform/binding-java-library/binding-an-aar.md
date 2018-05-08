---
title: Powiązanie. AAR
description: Ten przewodnik zawiera instrukcje krok po kroku do tworzenia biblioteki powiązania Xamarin.Android Java z systemem Android. Plik AAR.
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/11/2018
ms.openlocfilehash: 54708a7cfd071f77968991c9fe4e52938697c9bb
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="binding-an-aar"></a>Powiązanie. AAR

_Ten przewodnik zawiera instrukcje krok po kroku do tworzenia biblioteki powiązania Xamarin.Android Java z systemem Android. Plik AAR._


## <a name="overview"></a>Omówienie

*Android archiwum (. AAR)* plik jest format pliku dla biblioteki systemu Android.
. Plik AAR jest. Archiwum ZIP zawiera następujące:

-   Skompilowany kod języka Java
-   Identyfikatory zasobów
-   Zasoby
-   Meta-data (na przykład deklaracje działania, uprawnienia)

W tym przewodniku możemy czynności w podstawy tworzenia biblioteki powiązania z jednym. Plik AAR. Omówienie Java biblioteki powiązanie ogólnie (za pomocą przykładowego kodu podstawowych), zobacz [powiązanie biblioteka języka Java](~/android/platform/binding-java-library/index.md).


> [!IMPORTANT]
> Powiązanie projekt może zawierać tylko jeden. Plik AAR. Jeśli. Zależności AAR drugiej. AAR, a następnie te zależności powinny być zawarte w projekcie własnych powiązania i zawiera odwołania do. Zobacz [usterek 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573).


## <a name="walkthrough"></a>Wskazówki

Utworzymy biblioteki powiązań, na przykład plik archiwum systemu Android, który został utworzony w programie Android Studio [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true). To. Zawiera AAR `TextCounter` klasy z metod statycznych, które liczbę samogłoski i spółgłoski w ciągu. Ponadto **textanalyzer.aar** zawiera zasób obrazu, aby wyświetlić wyniki inwentaryzacji.

Do tworzenia biblioteki powiązania z użyjemy następujące kroki. Plik AAR:

1. Utwórz nowy projekt języka Java powiązania biblioteki.

2. Dodaj jeden. AAR plik do projektu. Powiązanie projekt może zawierać tylko jeden. AAR.

3. Ustawić akcji kompilacji odpowiednie. Plik AAR.

4. Wybierz pozycję platformy docelowej. Obsługuje AAR.

5. Tworzenie biblioteki powiązania.

Po utworzyliśmy biblioteki powiązań, firma Microsoft polega na opracowywaniu małych aplikacji systemu Android, które monituje użytkownika dla wywołania ciągu tekstowego. Metody AAR do analizowania tekstu, pobiera obraz z. AAR i wyświetla wyniki wraz z obrazem.

Przykładowa aplikacja będą uzyskiwać dostęp do `TextCounter` klasy **textanalyzer.aar**:

```java
package com.xamarin.textcounter;

public class TextCounter
{
    ...
    public static int numVowels (String text) { ... };
    ...
    public static int numConsonants (String text) { ... };
    ...
}
```

Ponadto, to przykładowa aplikacja będzie pobierania i wyświetlania zasób obrazu, który jest spakowany w **textanalyzer.aar**:

[![Obraz małp Xamarin](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

Ten zasób obrazu znajduje się w **res/drawable/monkey.png** w **textanalyzer.aar**.



### <a name="creating-the-bindings-library"></a>Tworzenie biblioteki powiązania

Przed rozpoczęciem poniższe kroki, Pobierz przykład [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android archiwum:

1.  Tworzenie nowego projektu biblioteki powiązania począwszy od szablonu biblioteki systemu Android powiązania. Można użyć programu Visual Studio for Mac lub Visual Studio (na zrzutach ekranu poniżej Pokaż Visual Studio, ale programu Visual Studio for Mac jest bardzo podobny). Nazwa rozwiązania **AarBinding**:

    [![Utwórz projekt AarBindings](binding-an-aar-images/01-new-bindings-library-vs-sml.w157.png)](binding-an-aar-images/01-new-bindings-library-vs.w157.png#lightbox)

2.  Szablon zawiera **Jars** folder, w którym można dodać użytkownika. AAR(s) do projektu biblioteki powiązania. Kliknij prawym przyciskiem myszy **Jars** i wybierz polecenie **Dodaj > istniejący element**:

    [![Dodaj istniejący element](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)


3.  Przejdź do **textanalyzer.aar** wcześniej pobrany plik, zaznacz go i kliknij **Dodaj**:

    [![Dodaj textanalayzer.aar](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)


4.  Sprawdź, czy **textanalyzer.aar** został pomyślnie dodany do projektu:

    [![Dodano plik textanalyzer.aar](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5.  Ustawić akcji kompilacji dla **textanalyzer.aar** do `LibraryProjectZip`. W programie Visual Studio for Mac, kliknij prawym przyciskiem myszy **textanalyzer.aar** można ustawić akcji kompilacji. W programie Visual Studio, Akcja kompilacji można ustawić w **właściwości** okienko):

    [![Ustawienie akcji kompilacji textanalyzer.aar LibraryProjectZip](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6.  Otwórz właściwości, aby skonfigurować projekt *platformy docelowej*. Jeśli. AAR używa żadnych API systemu Android, ustaw docelową platformę na poziom interfejsu API. AAR oczekuje. (Aby uzyskać więcej informacji o ustawianiu platformy docelowej i poziomy interfejsu API systemu Android w ogólności, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md).)

    Ustaw element docelowy poziom interfejsu API biblioteki powiązania. W tym przykładzie firma Microsoft mogą używać najnowszej platformy poziom interfejsu API (interfejs API na poziomie 23), ponieważ naszych **textanalyzer** interfejsów API systemu Android nie ma zależności:

    [![Ustawienie poziomu docelowego 23 interfejsu API](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7.  Tworzenie biblioteki powiązania. Projekt biblioteki powiązania należy pomyślnie kompilacji i utworzenie danych wyjściowych. Biblioteki DLL w następującej lokalizacji: **AarBinding/bin/Debug/AarBinding.dll**



### <a name="using-the-bindings-library"></a>Za pomocą biblioteki powiązania

Aby korzystać z tego. Biblioteki DLL w aplikacji platformy Xamarin.Android, należy najpierw dodać odwołanie do biblioteki powiązania. Wykonaj następujące kroki:

1.  Trwa tworzenie aplikacji w tym samym rozwiązaniu jako bibliotekę powiązania w celu uproszczenia tego przewodnika. (Aplikacji, który wykorzystuje biblioteki powiązania można również znajdują się w różnych rozwiązaniach.) Utwórz nową aplikację platformy Xamarin.Android: kliknij rozwiązanie prawym przyciskiem myszy i wybierz **Dodawanie nowego projektu**. Nazwa nowego projektu **BindingTest**:

    [![Utwórz nowy projekt BindingTest](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2.  Kliknij prawym przyciskiem myszy **odwołania** węzła **BindingTest** projekt i wybierz **Dodawanie odwołania...** :

    [![Kliknij przycisk Dodaj odwołanie](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3.  Wybierz **AarBinding** wcześniej utworzony projekt i kliknij przycisk **OK**:

    [![Sprawdź AAR powiązania projektu](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4.  Otwórz **odwołania** węzła **BindingTest** projektu, aby sprawdzić, czy **AarBinding** występuje odwołanie:

    [![AarBinding znajduje się w obszarze odwołań](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)


Jeśli chcesz wyświetlić zawartość projektu biblioteki powiązanie, możesz kliknąć dwukrotnie odwołania, aby otworzyć go w **przeglądarki obiektów**. Można wyświetlić zawartość zamapowanych `Com.Xamarin.Textcounter` przestrzeni nazw (mapowane za pomocą języka Java `com.xamarin.textanalyzezr` pakietu) i można wyświetlić członków `TextCounter` klasy:

[![Wyświetlanie w przeglądarce obiektów](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

Zrzut ekranu powyżej prezentuje dwa `TextAnalyzer` metody wywołujące przykładową aplikację: `NumConsonants` (który opakowuje źródłowy Java `numConsonants` — metoda), i `NumVowels` (który opakowuje źródłowy Java `numVowels` — metoda).



### <a name="accessing-aar-types"></a>Uzyskiwanie dostępu do. Typy AAR

Dodanie odwołania do aplikacji wskazujące powiązanie biblioteki są dostępne typy Java w. AAR jako użytkownik może uzyskiwać dostęp do typów C# (dzięki użyciu C# otoki). Kod aplikacji C# może wywołać `TextAnalyzer` metod, jak pokazano w poniższym przykładzie:

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

W powyższym przykładzie dzwonimy metod statycznych `TextCounter` klasy. Można również utworzyć wystąpienia klasy i wywołanie metody wystąpienia. Na przykład jeśli użytkownika. AAR opakowuje klasy o nazwie `Employee` mający metody wystąpienia `buildFullName`, można utworzyć wystąpienia `MyClass` i używać go jak pokazano poniżej:

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

Następujące kroki, aby go monit o wprowadzenie tekstu, używa Dodaj kod do aplikacji `TextCounter` do analizowania tekstu, a następnie wyświetla wyniki.

Zastąp **BindingTest** układu (**Main.axml**) za pomocą następujących XML. Ten układ ma `EditText` dwóch przycisków za inicjowanie samogłosek i Spółgłoska liczby i wprowadzania tekstu:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation             ="vertical"
    android:layout_width            ="fill_parent"
    android:layout_height           ="fill_parent" >
    <TextView
        android:text                ="Text to analyze:"
        android:textSize            ="24dp"
        android:layout_marginTop    ="30dp"
        android:layout_gravity      ="center"
        android:layout_width        ="wrap_content"
        android:layout_height       ="wrap_content" />
    <EditText
        android:id                  ="@+id/input"
        android:text                ="I can use my .AAR file from C#!"
        android:layout_marginTop    ="10dp"
        android:layout_gravity      ="center"
        android:layout_width        ="300dp"
        android:layout_height       ="wrap_content"/>
    <Button
        android:id                  ="@+id/vowels"
        android:layout_marginTop    ="30dp"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Vowels" />
    <Button
        android:id                  ="@+id/consonants"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Consonants" />
</LinearLayout>
```

Zastąp zawartość **MainActivity.cs** następującym kodem. Jak pokazano w tym przykładzie, wywołanie procedury obsługi zdarzeń przycisk opakowana `TextCounter` metod, które znajdują się w. AAR i użyj wyskakujące powiadomienia do wyświetlenia wyników. Powiadomienie `using` instrukcji dla przestrzeni nazw powiązania biblioteki (w tym przypadku `Com.Xamarin.Textcounter`):

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using Android.Views.InputMethods;
using Com.Xamarin.Textcounter;

namespace BindingTest
{
    [Activity(Label = "BindingTest", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        InputMethodManager imm;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            imm = (InputMethodManager)GetSystemService(Context.InputMethodService);

            var vowelsBtn = FindViewById<Button>(Resource.Id.vowels);
            var consonBtn = FindViewById<Button>(Resource.Id.consonants);
            var edittext = FindViewById<EditText>(Resource.Id.input);
            edittext.InputType = Android.Text.InputTypes.TextVariationPassword;

            edittext.KeyPress += (sender, e) =>
            {
                imm.HideSoftInputFromWindow(edittext.WindowToken, HideSoftInputFlags.NotAlways);
                e.Handled = true;
            };

            vowelsBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumVowels(edittext.Text);
                string msg = count + " vowels found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

            consonBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumConsonants(edittext.Text);
                string msg = count + " consonants found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

        }
    }
}
```

Kompilowanie i uruchamianie **BindingTest** projektu. Aplikacja uruchomi i przedstawia zrzut ekranu po lewej stronie ( `EditText` jest inicjowany z tekstem, ale naciśnięciu można go zmienić). Po naciśnięciu przycisku **SAMOGŁOSKI liczba**, wyskakujące Wyświetla liczbę samogłoski, jak pokazano na prawo od:

[![Zrzuty ekranu uruchamiania BindingTest](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

Spróbuj nacisnąć pozycję **SPÓŁGŁOSKI liczba** przycisku. Ponadto można zmodyfikować wiersz tekstu i naciśnij pozycję tych przycisków ponownie w celu zbadania różnych samogłosek Spółgłoska liczby i.



### <a name="accessing-aar-resources"></a>Uzyskiwanie dostępu do. Zasoby AAR

Xamarin narzędzi scaleń **R** dane. AAR w swojej aplikacji **zasobów** klasy. W związku z tym są dostępne. Zasoby AAR z układu (i z kodem) w taki sam sposób, jak może uzyskać dostępu do zasobów, które znajdują się w **zasobów** ścieżkę projektu.

Aby uzyskać dostęp do zasobu obrazu, należy użyć **Resource.Drawable** nazwę obrazu spakowane wewnątrz. AAR. Na przykład możesz odwoływać się do **image.png** w. Plik AAR za pomocą `@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

Można także przejść do układów zasobów, które znajdują się w. AAR. Aby to zrobić, należy użyć **Resource.Layout** nazwę układu umieszczone wewnątrz. AAR. Na przykład:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

**Textanalyzer.aar** przykład zawierający plik obrazu, który znajduje się w **res/drawable/monkey.png**. Umożliwia dostęp do tego zasobu obrazu i używać go w naszym przykładzie aplikacji:

Edytuj **BindingTest** układu (**Main.axml**) i Dodaj `ImageView` na końcu `LinearLayout` kontenera. To `ImageView` Wyświetla obraz znaleźć pod adresem **@drawable/monkey**; ten obraz zostaną załadowane z sekcji zasobów **textanalyzer.aar**:

```xml
    ...
    <ImageView
        android:src                 ="@drawable/monkey"
        android:layout_marginTop    ="40dp"
        android:layout_width        ="200dp"
        android:layout_height       ="200dp"
        android:layout_gravity      ="center" />

</LinearLayout>
```

Kompilowanie i uruchamianie **BindingTest** projektu. Aplikacja uruchomi i przedstawia zrzut ekranu po lewej stronie &ndash; po naciśnięciu przycisku **SPÓŁGŁOSKI liczba**, wyniki są wyświetlane, jak pokazano na prawo od:

[![Wyświetlanie liczby zgodne BindingTest](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)


Gratulacje! Biblioteka języka Java pomyślnie zostały powiązane. AAR!



## <a name="summary"></a>Podsumowanie

W tym przewodniku, utworzyliśmy biblioteki powiązania. Plik AAR, dodać biblioteki powiązania do aplikacji minimalnego testu i uruchomiono aplikację w celu zweryfikowania, że naszego kodu C# można wywołać kodu języka Java znajdującej się w. Plik AAR.
Ponadto firma Microsoft rozszerzony aplikacji dostępu i wyświetla zasób obrazu, który znajduje się w. Plik AAR.


## <a name="related-links"></a>Linki pokrewne

- [Kompilowanie biblioteki powiązania Java (klip wideo)](https://university.xamarin.com/classes#10090)
- [Tworzenie powiązań plików JAR](~/android/platform/binding-java-library/binding-a-jar.md)
- [Tworzenie powiązań biblioteki języka Java](~/android/platform/binding-java-library/index.md)
- [AarBinding (przykład)](https://developer.xamarin.com/samples/monodroid/JavaIntegration/AarBinding)
- [Błąd 44573-do-jednego projektu nie można powiązać wiele plików AAR](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
