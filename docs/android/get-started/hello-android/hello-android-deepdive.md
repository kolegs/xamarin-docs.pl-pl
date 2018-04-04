---
title: 'Hello, Android: Nowości'
description: W tym przewodniku dwuczęściową możesz tworzenie pierwszej aplikacji platformy Xamarin.Android i zrozumienia podstaw dotyczących tworzenia aplikacji systemu Android za pomocą platformy Xamarin. Na bieżąco należy wprowadzić narzędzi, pojęcia i kroki wymagane do tworzenia i wdrażania aplikacji platformy Xamarin.Android.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: EF0E110B-20EA-43F6-9476-1A0F41AFD298
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: bae3e7323596cc88f2b76aceeb5a4d1df4ce2d0c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="hello-android-deep-dive"></a>Hello, Android: Nowości

_W tym przewodniku dwuczęściową możesz tworzenie pierwszej aplikacji platformy Xamarin.Android i zrozumienia podstaw dotyczących tworzenia aplikacji systemu Android za pomocą platformy Xamarin. Na bieżąco należy wprowadzić narzędzi, pojęcia i kroki wymagane do tworzenia i wdrażania aplikacji platformy Xamarin.Android._

## <a name="hello-android-deep-dive"></a>Witaj, Android nowości

W [Hello, Android szybkiego startu](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md), wbudowane i uruchomiono pierwszej aplikacji platformy Xamarin.Android. Teraz nadszedł czas na opracowanie lepiej zrozumieć, jak Android pracy aplikacji, dzięki czemu można tworzyć bardziej złożone programy. W tym przewodniku opisano kroki, które miały w Hello, wskazówki dla systemu Android, dzięki czemu można zrozumieć, jakie były i zacząć podstawowych zrozumienia projektowanie aplikacji systemu Android.

W tym przewodniku będzie touch na następujące tematy:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **Wprowadzenie do programu Visual Studio** &ndash; wprowadzenie do programu Visual Studio i tworzenie nowej aplikacji platformy Xamarin.Android.

-   **Anatomia aplikacji platformy Xamarin.Android** — samouczek podstawowych części aplikacji platformy Xamarin.Android.

-   **Podstawowe informacje na temat aplikacji oraz podstawowe informacje o architekturze** &ndash; wprowadzenie do działań, manifestu systemu Android i ogólne wersję programowanie dla systemu Android.

-   **Interfejs użytkownika (UI)** &ndash; Tworzenie interfejsów użytkownika za pomocą projektanta dla systemu Android.

-   **Działania i cyklem życia działania** &ndash; wprowadzenie do działania w cyklu życia i okablowania interfejsu użytkownika w kodzie.

-   **Testowanie, wdrożenia i ostateczne poprawki** &ndash; zakończenie aplikacji przy użyciu wskazówki dotyczące testowania, wdrożenia generowania kompozycji i więcej.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   **Wprowadzenie do programu Visual Studio dla komputerów Mac** &ndash; wprowadzenie do platformy Xamarin Studio i tworzenie nowej aplikacji platformy Xamarin.Android.

-   **Anatomia aplikacji platformy Xamarin.Android** &ndash; samouczek podstawowych części aplikacji platformy Xamarin.Android.

-   **Podstawowe informacje na temat aplikacji oraz podstawowe informacje o architekturze** &ndash; wprowadzenie do działań, manifestu systemu Android i ogólne wersję programowanie dla systemu Android.

-   **Interfejs użytkownika (UI)** &ndash; Tworzenie interfejsów użytkownika za pomocą projektanta dla systemu Android.

-   **Działania i cyklem życia działania** &ndash; wprowadzenie do działania w cyklu życia i okablowania interfejsu użytkownika w kodzie.

-   **Testowanie, wdrożenia i ostateczne poprawki** &ndash; zakończenie aplikacji przy użyciu wskazówki dotyczące testowania, wdrożenia generowania kompozycji i więcej.

-----


Ten przewodnik ułatwia tworzenie umiejętności i wiedzy wymagane do tworzenia pojedynczej ekranu aplikacji systemu Android. Po zakończeniu pracy przy jego użyciu, należy zrozumieć różne części aplikacji platformy Xamarin.Android i sposób ich dopasowania.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Wprowadzenie do programu Visual Studio

Program Visual Studio jest zaawansowanym IDE firmy Microsoft. Zawiera funkcje pełni zintegrowane wizualnego projektanta, Edytor tekstu, zawierającą refaktoryzacji narzędzia, przeglądarkę zestawu integracji kodu źródłowego i więcej. W tym przewodniku nauczysz niektóre podstawowe funkcje programu Visual Studio za pomocą wtyczki Xamarin.

Program Visual Studio umożliwia organizowanie kodu w _rozwiązań_ i _projekty_. Rozwiązanie to kontener, który może zawierać jeden lub więcej projektów. Projekt może być aplikacji (np. dla systemu iOS lub Android), biblioteka pomocnicze, aplikacja testowa i. W **Phoneword** aplikacji, należy dodać nowy projekt dla systemu Android przy użyciu **aplikacji systemu Android** szablon **Phoneword** rozwiązania utworzone w [Witaj, Android](~/android/get-started/hello-android/hello-android-quickstart.md) przewodnik. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Wprowadzenie do programu Visual Studio dla komputerów Mac

Visual Studio for Mac jest bezpłatne, open source IDE podobne do programu Visual Studio. Zawiera funkcje pełni zintegrowane wizualnego projektanta, Edytor tekstu, wraz z narzędzia do refaktoryzacji, przeglądarkę zestawu i integracji kodu źródłowego. W tym przewodniku omówiono używać niektórych podstawowych programu Visual Studio dla komputerów Mac funkcji. Jeśli jesteś nowym użytkownikiem programu Visual Studio for Mac, warto zapoznaj się z bardziej szczegółowym [wprowadzenie do programu Visual Studio dla komputerów Mac](https://docs.microsoft.com/visualstudio/mac/).

Visual Studio for Mac następuje rozwiązanie Visual Studio kodu do organizowania _rozwiązań_ i _projekty_. Rozwiązanie to kontener, który może zawierać jeden lub więcej projektów. Projekt może być aplikacji (np. dla systemu iOS lub Android), biblioteka pomocnicze, aplikacja testowa i. W **Phoneword** aplikacji, należy dodać nowy projekt dla systemu Android przy użyciu **aplikacji systemu Android** szablon **Phoneword** rozwiązania utworzone w [Witaj, Android](~/android/get-started/hello-android/hello-android-quickstart.md) przewodnik.

-----

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinandroid-application"></a>Anatomia aplikacji platformy Xamarin.Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Poniższy zrzut ekranu listy zawartości tego rozwiązania. Jest to Eksplorator rozwiązań zawiera strukturę katalogów oraz ich wszystkie pliki skojarzone z rozwiązania:

[![Eksplorator rozwiązań](hello-android-deepdive-images/vs/02-solution-structure-sml.png)](hello-android-deepdive-images/vs/02-solution-structure.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Poniższy zrzut ekranu listy zawartości tego rozwiązania. Jest to konsoli rozwiązanie zawiera strukturę katalogów oraz ich wszystkie pliki skojarzone z rozwiązania:

[![Konsola rozwiązania](hello-android-deepdive-images/xs/02-solution-structure-sml.png)](hello-android-deepdive-images/xs/02-solution-structure.png#lightbox)

-----

Rozwiązanie o nazwie **Phoneword** został utworzony i Projekt systemu Android **Phoneword** został umieszczony wewnątrz niej.

Przyjrzyj się elementy wewnątrz każdego folderu i jego celem projektu:

-   **Właściwości** &ndash; zawiera [AndroidManifest.xml](~/android/platform/android-manifest.md) pliku, który opisano wszystkie wymagania dotyczące aplikacji platformy Xamarin.Android, w tym nazwa, numer wersji i uprawnień. **Właściwości** folder zawiera również wszystkie [AssemblyInfo.cs](http://msdn.microsoft.com/en-us/library/microsoft.visualbasic.applicationservices.assemblyinfo(v=vs.110).aspx), plik metadanych zestawu .NET. Jest dobrym rozwiązaniem, aby wypełnić ten plik podstawowych informacji o aplikacji.

-   **Odwołania** &ndash; zawiera zestawy wymagane, aby skompilować i uruchomić aplikację. Po rozwinięciu katalogu odwołań, zobaczysz odwołania do zestawów platformy .NET takich jak [systemu](http://msdn.microsoft.com/en-us/library/system%28v=vs.110%29.aspx), System.Core, i [System.Xml](http://msdn.microsoft.com/en-us/library/system.xml%28v=vs.110%29.aspx), oraz odwołanie do zestawu Mono.Android na platformie Xamarin.


-   **Zasoby** &ndash; zawiera pliki wymagane przez aplikację do uruchamiania w tym czcionki, lokalne pliki danych i plików tekstowych. Pliki uwzględnione w tym miejscu są dostępne za pośrednictwem wygenerowany `Assets` klasy. Aby uzyskać więcej informacji na zasoby systemu Android, zobacz Xamarin [zasoby systemu Android przy użyciu](~/android/app-fundamentals/resources-in-android/android-assets.md) przewodnik.

-   **Zasoby** &ndash; zawiera zasoby aplikacji, takich jak ciągów, obrazy i układów. Dostęp do tych zasobów w kodzie za pomocą wygenerowany `Resource` klasy. [Android zasobów](~/android/app-fundamentals/resources-in-android/index.md) przewodnik zawiera szczegółowe informacje o **zasobów** katalogu. Szablon aplikacji zawiera również krótkie przewodnik dotyczący zasobów w **AboutResources.txt** pliku.

### <a name="resources"></a>Zasoby

**Zasobów** katalog zawiera cztery foldery o nazwie **obiektów drawable**, **układu**, **mipmap** i **wartości**, a także plik o nazwie **Resource.designer.cs**.

W poniższej tabeli przedstawiono podsumowanie elementów:

-   **obiektów drawable** &ndash; DOM katalogi obiektów drawable [zasobów obiektów drawable](http://developer.android.com/guide/topics/resources/drawable-resource.html) takich jak obrazy i bitmapy.

-   **mipmap** &ndash; katalogu mipmap przechowywane pliki obiektów drawable dla różnych uruchamiania ikona gęstości. W szablonie domyślnego katalogu obiektów drawable przechowuje plik ikony aplikacji, **Icon.png**.


-   **Układ** &ndash; układu katalogu zawiera _Android plików projektanta_ (.axml) definiującą interfejsu użytkownika dla wszystkich ekranów i działań. Szablon tworzy domyślny układ o nazwie **Main.axml**.

-   **wartości** &ndash; ten katalog zawiera wszystkie pliki XML, które przechowują wartości prosty przykład ciągów, liczb całkowitych i kolory. Szablon tworzy plik do przechowywania wartości ciągu o nazwie **Strings.xml**.

-   **Resource.Designer.cs** &ndash; znanej także jako `Resource` klasy, ten plik jest częściowej klasy, która przechowuje unikatowe identyfikatory przypisane do każdego zasobu. Automatycznie jest tworzony za pomocą narzędzi platformy Xamarin.Android, a w przypadku ponownego wygenerowania niezbędne. Ten plik nie należy ręcznie edytować, ponieważ Xamarin.Android spowoduje zastąpienie wszelkich ręcznych zmian.


## <a name="app-fundamentals-and-architecture-basics"></a>Podstawowe informacje na temat aplikacji oraz podstawowe informacje dotyczące architektury

Aplikacje systemu android nie mają jeden punkt wejścia; oznacza to to nie pojedynczy wiersz kodu w aplikacji, która wywołuje systemu operacyjnego można uruchomić aplikacji. Zamiast tego aplikacja jest uruchamiana po Android tworzy jednej z jej klas, w tym czasie Android ładowany do pamięci procesu całej aplikacji.

Ta funkcja unikatowy systemu android może być bardzo przydatne w przypadku projektowania skomplikowane aplikacji lub wchodzącymi w interakcje z systemem Android. Jednak te opcje umożliwiają Android złożonych podczas pracy nad podstawowy scenariusz, takich jak **Phoneword** aplikacji. Z tego powodu eksploracji architektury systemu Android jest podzielony na dwie. W tym przewodniku dissects aplikacji korzystającej z najbardziej typowych punkt wejścia dla aplikacji systemu Android: pierwszy ekran. W [Hello, Android Wieloekranowy](~/android/get-started/hello-android-multiscreen/index.md), złożoności pełnego architektura systemu Android są przedstawione zgodnie z opisem są różne sposoby, aby uruchomić aplikację.


### <a name="phoneword-scenario---starting-with-an-activity"></a>Scenariusz Phoneword — począwszy od działania

Po otwarciu **Phoneword** aplikacji po raz pierwszy w emulatorze lub urządzeń, system operacyjny tworzy pierwszy *działania*. Działanie jest specjalnej klasy systemu Android, umożliwiająca ekranu jednej aplikacji i jest odpowiedzialny za rysowania i włączanie interfejsu użytkownika. Gdy Android tworzy pierwsze działanie aplikacji, ładuje całej aplikacji:

[![Działanie obciążenia](hello-android-deepdive-images/01-activity-load-sml.png)](hello-android-deepdive-images/01-activity-load.png#lightbox)

Ponieważ nie ma żadnych liniowej postępu za pośrednictwem aplikacji systemu Android (można uruchomić aplikacji z kilku punktów), Android ma unikatowy sposób rejestrowanie informacji o klasy i pliki składa się aplikacja. W **Phoneword** przykładzie wszystkie części, które tworzą aplikacji są rejestrowane specjalny plik XML o nazwie **manifestu systemu Android**. Rola **manifestu systemu Android** do śledzenia zawartość aplikacji, właściwości i uprawnień i ujawnienia ich do systemu operacyjnego Android. Można potraktować **Phoneword** aplikacji jako pojedyncze działanie (ekran) i kolekcję plików zasobów i pomocnika powiązane ze sobą przy użyciu pliku manifestu systemu Android, jak pokazano na poniższym diagramie:

[![Pomocnicy zasobów](hello-android-deepdive-images/02-resources-helpers-sml.png)](hello-android-deepdive-images/02-resources-helpers.png#lightbox)

Następne sekcje kilka Eksploruj relacje między różne części **Phoneword** aplikacji; należy dostarczyć lepiej zrozumieć na powyższym diagramie. Zawarto informacje Android pliki projektanta i układ tego eksploracji rozpoczyna się przy użyciu interfejsu użytkownika.


## <a name="user-interface"></a>Interfejs użytkownika

`Main.axml` jest to plik układu interfejsu użytkownika na pierwszym ekranie w aplikacji. .axml wskazuje, że jest to plik projektanta dla systemu Android (oznacza AXML *Android XML*). Nazwa *Main* dowolnej z punktu widzenia firmy Android &ndash; pliku układu może mieć inną nazwę. Po otwarciu **Main.axml** w środowisku IDE, wyświetlenie go Edytor wizualny dla systemu Android układu plików o nazwie *Android projektanta*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Projektant android](hello-android-deepdive-images/vs/03-android-designer-sml.png "projektanta dla systemu Android")](hello-android-deepdive-images/vs/03-android-designer.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Projektant systemu android](hello-android-deepdive-images/xs/03-android-designer-sml.png)](hello-android-deepdive-images/xs/03-android-designer.png#lightbox)

-----

W **Phoneword** aplikacji, **TranslateButton**jego identyfikator jest ustawiony na `@+id/TranslateButton`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Ustawienia identyfikatora TranslateButton](hello-android-deepdive-images/vs/04-translatebutton-sml.png "TranslateButton ustawienia identyfikatora")](hello-android-deepdive-images/vs/04-translatebutton.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Ustawienia identyfikatora TranslateButton](hello-android-deepdive-images/xs/04-translatebutton-sml.png)](hello-android-deepdive-images/xs/04-translatebutton.png#lightbox)

-----

Podczas ustawiania `id` właściwość **TranslateButton**, mapy Android projektanta **TranslateButton** formant `Resource` klasy, a następnie przypisuje go *zasobów Identyfikator* z `TranslateButton`. To mapowanie visual formantu do klasy umożliwia lokalizowanie i używanie **TranslateButton** i innych kontrolek w kodzie aplikacji. Spowoduje to omówiona bardziej szczegółowo po przerwaniu od siebie kod, który obsługuje kontrolki. Wszystko co należy wiedzieć, obecnie jest kod reprezentację formant jest powiązany wizualną reprezentację formantu w Projektancie za pośrednictwem `id` właściwości.


### <a name="source-view"></a>Wyświetl źródło

Wszystkie elementy zdefiniowane na powierzchni projektowej przetłumaczyć XML dla platformy Xamarin.Android do użycia. Projektant Android udostępnia widoku źródła, który zawiera kod XML, który został wygenerowany na podstawie projektanta wizualnego. Możesz wyświetlić plik XML przy przełączanie do **źródła** panelu w lewym dolnym rogu Widok projektanta, jak pokazano na poniższym zrzucie ekranu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Widok projektanta źródła](hello-android-deepdive-images/vs/05-source-view-sml.png "widoku źródła projektanta")](hello-android-deepdive-images/vs/05-source-view.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Widok projektanta źródła](hello-android-deepdive-images/xs/05-source-view-sml.png)](hello-android-deepdive-images/xs/05-source-view.png#lightbox)

-----

Ten kod źródłowy XML powinien zawierać **tekst (duży)**, **zwykły tekst**, a dwa **przycisk** elementów. Aby uzyskać więcej informacji na temat samouczek projektanta dla systemu Android, odwoływać się do platformy Xamarin Android [Omówienie projektanta](~/android/user-interface/android-designer/index.md) przewodnik.

Teraz zostać omówione narzędzia i pojęcia dotyczące visual część interfejsu użytkownika. Następnie nadszedł czas, aby przejść do kodu, które obsługuje interfejs użytkownika jako działania i cyklem życia działania są przedstawione.


## <a name="activities-and-the-activity-lifecycle"></a>Działania i cyklem życia działania

`Activity` Klasa zawiera kod, który obsługuje interfejs użytkownika.
Działanie jest odpowiedzialny za odpowiada do interakcji z użytkownikiem i tworzenie dynamicznych użytkowników.
W tej sekcji przedstawiono `Activity` klasy, w tym artykule omówiono cyklu życia działania i dissects kod, który obsługuje interfejs użytkownika w **Phoneword** aplikacji.


### <a name="activity-class"></a>Klasa działania

**Phoneword** aplikacja ma tylko jednego ekranu (działanie). Klasa, która obsługuje ekranu jest nazywany `MainActivity` i znajduje się w **MainActivity.cs** pliku. Nazwa `MainActivity` ma specjalnego znaczenia w systemie Android &ndash; Konwencji jest nazwa pierwsze działanie w aplikacji `MainActivity`, Android nie zależy, jeśli jest on inną nazwę.

Po otwarciu **MainActivity.cs**, można stwierdzić, że `MainActivity` jest klasa *podklasy* z `Activity` klasy i że działania jest adorned z [działania](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) atrybutu:

```csharp
[Activity (Label = "Phone Word", MainLauncher = true)]
public class MainActivity : Activity
{
  ...
}
```

`Activity` Atrybut rejestruje działanie z manifestu systemu Android; dzięki temu Android wiedzieć, że ta klasa jest częścią **Phoneword** aplikacji zarządzanych przez tego manifestu. `Label` Właściwość ustawia tekst, który będzie wyświetlany w górnej części ekranu.

`MainLauncher` Właściwości informuje systemu Android do wyświetlenia tego działania, po uruchomieniu aplikacji. Ta właściwość jest ważne, jak dodać więcej działaniami (ekranami) do aplikacji, zgodnie z objaśnieniem w [Hello, Android Wieloekranowy](~/android/get-started/hello-android-multiscreen/index.md) przewodnik.

Teraz, gdy podstawy `MainActivity` zostały objęte, nadszedł czas na lepsze zapoznanie się z kodem działania dzięki zastosowaniu _cyklu życia działania_.

### <a name="activity-lifecycle"></a>Cykl życia działania

W systemie Android działania przejść przez różne etapy cyklu życia, w zależności od ich interakcji z użytkownikiem. Można tworzyć działania, wprowadzenie i wstrzymania, wznowienia i zniszczone i tak dalej. `Activity` Klasa zawiera metody, które wywołuje system w niektórych punktach w cyklu życia ekranu. Na poniższym diagramie przedstawiono typowy okres ważności działania, a także niektóre odpowiednich metod cyklu życia:

[![Cykl życia działania](hello-android-deepdive-images/04-lifecycle-sml.png)](hello-android-deepdive-images/04-lifecycle.png#lightbox)

Przez zastąpienie `Activity` cyklu życia metod, można kontrolować sposób działania ładowania jak reaguje na użytkownika i nawet co się stanie po znika ono z ekranu urządzenia. Na przykład można zastąpić metody cyklu życia w powyższym diagramie, aby wykonać kilka ważnych zadań:

-   **OnCreate** &ndash; tworzy widoki, inicjuje zmienne i wykonuje inne zadania przygotowywania, które musi zostać wykonane zanim użytkownik będzie widział działania. Ta metoda jest wywoływana tylko raz, gdy działanie jest ładowany do pamięci. 

-   **OnResume** &ndash; wykonuje wszystkie zadania, które musi wystąpić za każdym razem, gdy działanie zwraca do ekranu urządzenia. 

-   **OnPause** &ndash; wykonuje wszystkie zadania, które musi wystąpić za każdym razem, gdy działanie pozostawia ekranu urządzenia.


Po dodaniu niestandardowych kodów metody cyklu życia w `Activity`, możesz *zastąpienia* tej metody cyklu życia *Podstawowa implementacja*. Wybierz istniejące metody cyklu życia (który jest już dołączony do niej kodu), a metoda ta jest rozszerzana za pomocą własnego kodu. Możesz wywoływać implementację podstawową z wewnątrz metodę, aby upewnić się, że oryginalny kod jest uruchamiany przed nowy kod. Na przykład przedstawiono w następnej sekcji. 

Cykl życia działania jest ważne i złożone częścią systemu Android. Jeśli chcesz dowiedzieć się więcej na temat działań, po zakończeniu _wprowadzenie_ serii odczytu [cyklu życia działania](~/android/app-fundamentals/activity-lifecycle/index.md) przewodnik. W tym przewodniku dalej koncentruje się na pierwszym etapie tego cyklu działania `OnCreate`.


### <a name="oncreate"></a>OnCreate

Wywołania android `Activity`w `OnCreate` podczas tworzenia działania (przed ekranu znajduje się z użytkownikiem). Można zastąpić `OnCreate` metody cyklu życia tworzenia widoków i przygotowywania do użytkownika spełnia Twoje działania:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);
    // Additional setup code will go here
}
```

W **Phoneword** aplikacji, najpierw musisz `OnCreate` jest załadować interfejsu użytkownika utworzone w Projektancie systemu Android. Aby załadować interfejsu użytkownika, należy wywołać `SetContentView` i przekaż go *Nazwa zasobu układu* pliku układu: `Main.axml`. Układ znajduje się pod adresem `Resource.Layout.Main`:

```csharp
SetContentView (Resource.Layout.Main);
```

Gdy `MainActivity` rozpoczyna się, tworzy widok, w którym jest oparty na zawartość **Main.axml** pliku. Należy pamiętać, że nazwa pliku układu pasuje do nazwy działania &ndash; *Main*.axml jest układ *Main*działania. Nie jest to wymagane z punktu widzenia firmy Android, ale po rozpoczęciu dodać więcej ekranów do aplikacji, można znaleźć czy Konwencja nazewnictwa ułatwia pasuje do pliku kodu do pliku układu.

Po przygotowaniu do pliku układu można rozpocząć wyszukiwania kontrolki.
Aby wyszukać kontrolkę, należy wywołać `FindViewById` i podaj identyfikator zasobu formantu:

```csharp
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
```

Teraz, gdy masz odwołań do formantów w pliku układ, można uruchomić programowania im odpowiadanie na interakcji z użytkownikiem.


### <a name="responding-to-user-interaction"></a>Odpowiada na żądania interakcji z użytkownikiem

W systemie Android `Click` zdarzeń nasłuchuje touch użytkownika. W tej aplikacji `Click` zdarzenie jest obsługiwane z wyrażenia lambda, ale delegata lub obsługi zdarzenia o nazwie można zamiast tego użyć. Ostatni **TranslateButton** kod przypominających następujące: 

```csharp
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

## <a name="testing-deployment-and-finishing-touches"></a>Testowanie wdrażania i ostateczne poprawki

Zarówno program Visual Studio dla komputerów Mac, jak i programu Visual Studio oferują wiele opcji testowania i wdrażania aplikacji. W tej sekcji opisano opcje debugowania, przedstawiono testowanie aplikacji na urządzeniu i wprowadza narzędzia do tworzenia niestandardowych aplikacji ikony gęstości inny ekran.


### <a name="debugging-tools"></a>Narzędzia debugowania

Może być trudne do diagnozowania problemów w kodzie aplikacji. Aby pomóc w diagnozowaniu problemów złożonego kodu, możesz [Ustaw punkt przerwania](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/), [kod za pomocą kroku](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/), lub [dane wyjściowe do okna dziennika](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/).


### <a name="deploy-to-a-device"></a>Wdrażanie do urządzenia

Emulator stanowi dobry początek wdrażania i testowania aplikacji, ale użytkownicy nie będą korzystać z ostatecznej aplikacji w emulatorze. Dobrym rozwiązaniem do testowania aplikacji na urządzeniu rzeczywistych wczesne i często jest.

Zanim urządzenia z systemem Android można użyć do testowania aplikacji, należy skonfigurować do tworzenia aplikacji. [Ustawić się urządzenia na potrzeby programowania](~/android/get-started/installation/set-up-device-for-development.md) przewodnik zawiera szczegółowe instrukcje na przygotowanie urządzenie do programowania.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po skonfigurowaniu urządzenia, można wdrożyć do niej przez podłączenie go, wybierając ją z **wybierz urządzenie** okno dialogowe i uruchamiania aplikacji:

![Urządzenie debugowania wybierz](hello-android-deepdive-images/vs/06-select-device.png "urządzenia wybierz debugowania")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po skonfigurowaniu urządzenia, można wdrożyć do niej przez jego podłączone, naciskając klawisz **Start (Odtwórz)**, wybierając ją z **wybierz urządzenie** okno dialogowe i naciskając klawisz **OK**:

[![Wybierz opcję debugowania urządzenia](hello-android-deepdive-images/xs/06-select-device-sml.png)](hello-android-deepdive-images/xs/06-select-device.png#lightbox)

-----

Spowoduje to uruchomienie aplikacji na urządzeniu:

[![Wprowadź Phoneword](hello-android-deepdive-images/05-enter-phoneword-sml.png)](hello-android-deepdive-images/05-enter-phoneword.png#lightbox)


### <a name="set-icons-for-different-screen-densities"></a>Ustawienie ikon dla gęstości inny ekran

Urządzenia z systemem android są dostępne w różnych rozmiarów ekranu i rozwiązania, a nie wszystkie obrazy wyglądać na wszystkich ekranach. Na przykład poniżej przedstawiono zrzut ekranu o małej gęstości ikony o wysokiej gęstości 5 węzła. Zwróć uwagę, jak rozmyte ma być porównywane otaczającego ikony:

[![Ikona rozmyte](hello-android-deepdive-images/06-blurry-icon-sml.png)](hello-android-deepdive-images/06-blurry-icon.png#lightbox)

Aby to uwzględniać, jest dobrym rozwiązaniem, aby dodać ikony różne rozwiązania **zasobów** folderu. Android zawiera różne wersje **mipmap** folderu do obsługi ikony uruchamiania różnych gęstości *mdpi* dla nośnika, *hdpi* wysokiego, i  *xhdpi*, *xxhdpi*, *xxxhdpi* na ekranach o bardzo wysokiej gęstości. Ikony o różnych rozmiarach są przechowywane w odpowiedniej **mipmap -** folderów:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![folderach mipmap](hello-android-deepdive-images/vs/07-mipmap-folders.png "folderach mipmap")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Folderach Mipmap](hello-android-deepdive-images/xs/07-mipmap-folders-sml.png)](hello-android-deepdive-images/xs/07-mipmap-folders.png#lightbox)

-----

Android zostanie wybierz ikonę z odpowiedniej gęstości:

[![Ikony w odpowiedniej gęstości](hello-android-deepdive-images/07-appropriate-density-sml.png)](hello-android-deepdive-images/07-appropriate-density.png#lightbox)

### <a name="generate-custom-icons"></a>Generowanie ikon niestandardowych

Nie zawsze ma dostępne do tworzenia ikon niestandardowych i uruchamianie obrazów, które aplikacja powinna wyróżniające projektanta. Poniżej przedstawiono kilka innych sposobów generowania aplikacji niestandardowej kompozycji:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   [Android Studio zasobów](http://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; generator opartych na sieci web, w przeglądarce dla wszystkich typów ikon systemu Android, wraz z łączami do innych narzędzi przydatne społeczności. Działa najlepiej w przeglądarce Google Chrome.

-   Visual Studio &ndash; umożliwia to tworzenie prostego ikony, ustaw dla aplikacji bezpośrednio w środowisku IDE.

-   [Glyphish](http://www.glyphish.com/) &ndash; ikona wbudowane wysokiej jakości bezpłatnie ustawia pobierania i zakupu.

-   [Fiverr](http://www.fiverr.com/) &ndash; możliwość wyboru z szerokiej gamy Designer, aby utworzyć ikonę ustawiane, zaczynając od wynosi 5. Może być hit lub miss, ale dobrym zasobu, jeśli potrzebujesz ikony przeznaczony na bieżąco.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   [Android Studio zasobów](http://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; generator opartych na sieci web, w przeglądarce dla wszystkich typów ikon systemu Android, wraz z łączami do innych narzędzi przydatne społeczności. Działa najlepiej w przeglądarce Google Chrome.

-   [Rysunek 3](https://itunes.apple.com/us/app/sketch/id852320343?mt=12) &ndash; szkicu jest to aplikacja Mac za projektowanie interfejsów użytkownika, ikony i inne. To jest aplikacja, który został użyty do zaprojektowania zestaw ikony aplikacji platformy Xamarin i uruchamianie obrazów. Rysunek 3 koszty około $80 i jest dostępna w sklepie z aplikacjami. Można wypróbować wolnych [narzędzie szkicu](http://bohemiancoding.com/sketch/tool/) również.

-   [Pixelmator](http://www.pixelmator.com/) &ndash; obraz elastyczne Edytowanie aplikacji dla komputerów Mac, który koszty około 30 $.

-   [Glyphish](http://www.glyphish.com/) &ndash; ikona wbudowane wysokiej jakości bezpłatnie ustawia pobierania i zakupu.

-   [Fiverr](http://www.fiverr.com/) &ndash; możliwość wyboru z szerokiej gamy Designer, aby utworzyć ikonę ustawiane, zaczynając od wynosi 5. Może być hit lub miss, ale dobrym zasobu, jeśli potrzebujesz ikony przeznaczony na bieżąco.

-----

Aby uzyskać więcej informacji na temat rozmiarów ikonę i wymagań dotyczą [Android zasobów](~/android/app-fundamentals/resources-in-android/index.md) przewodnik.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="adding-google-play-services-packages"></a>Dodawanie Google Play Services pakietów

_Usług Google Play_ to zestaw bibliotek dodatek umożliwia Android deweloperom korzystać z najnowszych funkcji z Google, takich jak Google Maps, Google Cloud Messaging i rozliczeń w aplikacji.
Wcześniej, powiązań dla wszystkich bibliotek usług Google Play dostarczonych przez platformy Xamarin w formie pojedynczy pakiet &ndash; począwszy od programu Visual Studio dla komputerów Mac, okno dialogowe nowego projektu jest dostępna do wybierania który usług Google Play pakietów do uwzględnienia w aplikacji.

Aby dodać co najmniej jedną bibliotekę usługa Google Play, kliknij prawym przyciskiem myszy **pakiety** węzeł w drzewie projektu i kliknij przycisk **dodać usługa Google Play...** :

[![Dodawanie usługi Google Play](hello-android-deepdive-images/xs/08-add-google-play-services-sml.png)](hello-android-deepdive-images/xs/08-add-google-play-services.png#lightbox)

Gdy **Dodawanie usług Google Play** jest wyświetlone okno dialogowe, wybierz pakiety, które mają zostać dodane do projektu (nugets):

[![Wybierz pakiety](hello-android-deepdive-images/xs/09-add-dialog-sml.png)](hello-android-deepdive-images/xs/09-add-dialog.png#lightbox)

Gdy wybierz usługę i kliknij przycisk **Dodaj pakiet**, Visual Studio for Mac pobiera i instaluje pakiet i wszystkie zależne pakiety usług Google Play, które są wymagane. W niektórych przypadkach może wystąpić **akceptacji licencji** okna dialogowego, które wymaga, aby **Akceptuj** przed pakiety są zainstalowane:

[![Akceptacja licencji](hello-android-deepdive-images/xs/10-license-acceptance-sml.png)](hello-android-deepdive-images/xs/10-license-acceptance.png#lightbox)

-----

## <a name="summary"></a>Podsumowanie

Gratulacje! Teraz powinien mieć pełny opis składników aplikacji platformy Xamarin.Android, a także narzędzia niezbędne do jej utworzenia.

W następnym samouczku z _wprowadzenie_ serii, rozszerzenie aplikacji do obsługi wielu ekrany Ci poznać platformę bardziej zaawansowanych architektura systemu Android i pojęcia.
