---
title: 'Witaj, Android: Szczegółowe informacje'
description: W tym przewodniku legalną dwuczęściową możesz utworzyć swoją pierwszą aplikację platformy Xamarin.Android i poznać podstawy tworzenia aplikacji dla systemu Android za pomocą platformy Xamarin. Po drodze zostaną wprowadzone do narzędzi, koncepcje i kroki wymagane do kompilowania i wdrażania aplikacji platformy Xamarin.Android.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: EF0E110B-20EA-43F6-9476-1A0F41AFD298
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: 3aa70469c5916a22a22d7857c62a4b46c1637124
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242423"
---
# <a name="hello-android-deep-dive"></a>Witaj, Android: Szczegółowe informacje

_W tym przewodniku legalną dwuczęściową możesz utworzyć swoją pierwszą aplikację platformy Xamarin.Android i poznać podstawy tworzenia aplikacji dla systemu Android za pomocą platformy Xamarin. Po drodze zostaną wprowadzone do narzędzi, koncepcje i kroki wymagane do kompilowania i wdrażania aplikacji platformy Xamarin.Android._

## <a name="hello-android-deep-dive"></a>Witaj, szczegółowe dane dla systemu Android

W [Witaj, przewodniku Szybki Start w systemie Android](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md), skompilowane i uruchomione swoją pierwszą aplikację platformy Xamarin.Android. Teraz nadszedł czas na programowanie lepiej zrozumieć, jak dla systemu Android działanie aplikacji, dzięki czemu można tworzyć bardziej zaawansowanych programów. Ten przewodnik Przegląd czynności, które miały w Witaj, przewodniku dla systemu Android, dzięki czemu można zrozumieć, co zostało zrobione i rozpocząć tworzenie powinieneś rozumieć podstawy tworzenia aplikacji dla systemu Android.

Ten przewodnik będzie w ogóle na następujące tematy:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **Wprowadzenie do programu Visual Studio** &ndash; wprowadzenie do programu Visual Studio i utworzenie nowej aplikacji platformy Xamarin.Android.

-   **Anatomia aplikacji platformy Xamarin.Android** — samouczek podstawowe części aplikacji platformy Xamarin.Android.

-   **Podstawy aplikacji i podstawy architektury** &ndash; wprowadzenie do działań, manifestu systemu Android i ogólne wersję opracowywania aplikacji systemu Android.

-   **Interfejs użytkownika (UI)** &ndash; Tworzenie interfejsów użytkownika za pomocą narzędzia Android Designer.

-   **Działania i cyklem życia aktywności** &ndash; wprowadzenie do cyklu życia aktywności i okablowania interfejsu użytkownika w kodzie.

-   **Testowanie, wdrażanie i poprawek** &ndash; wykonania aplikacji za pomocą wskazówki dotyczące testowania, wdrażania, generowanie kompozycji i inne.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   **Wprowadzenie do programu Visual Studio dla komputerów Mac** &ndash; wprowadzenie do programu Xamarin Studio i utworzenie nowej aplikacji platformy Xamarin.Android.

-   **Anatomia aplikacji platformy Xamarin.Android** &ndash; samouczek podstawowe części aplikacji platformy Xamarin.Android.

-   **Podstawy aplikacji i podstawy architektury** &ndash; wprowadzenie do działań, manifestu systemu Android i ogólne wersję opracowywania aplikacji systemu Android.

-   **Interfejs użytkownika (UI)** &ndash; Tworzenie interfejsów użytkownika za pomocą narzędzia Android Designer.

-   **Działania i cyklem życia aktywności** &ndash; wprowadzenie do cyklu życia aktywności i okablowania interfejsu użytkownika w kodzie.

-   **Testowanie, wdrażanie i poprawek** &ndash; wykonania aplikacji za pomocą wskazówki dotyczące testowania, wdrażania, generowanie kompozycji i inne.

-----


Ten przewodnik ułatwia rozwój umiejętności i wiedzę, które są wymagane do tworzenia jednoekranową aplikację dla systemu Android. Po zakończeniu pracy przez nią, musisz zrozumieć różne części aplikacji platformy Xamarin.Android i jak one współdziałają ze sobą.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Wprowadzenie do programu Visual Studio

Visual Studio to zaawansowane środowisko IDE firmy Microsoft. Zawiera funkcje w pełni zintegrowane wizualnego projektanta, edytora tekstu, który zawiera refaktoryzacji, przeglądarka zestawów, integracji kodu źródłowego i inne narzędzia. W tym przewodniku dowiesz się, niektóre z podstawowych funkcji programu Visual Studio za pomocą platformy Xamarin wtyczki.

Program Visual Studio umożliwia organizowanie kodu w _rozwiązania_ i _projektów_. To rozwiązanie jest kontenerem, który może zawierać jeden lub więcej projektów. Projekt może być aplikacji (np. dla systemu iOS lub Android), biblioteka pomocnicze, aplikacja testowa i. W **Phoneword** aplikacji, możesz dodać nowy projekt dla systemu Android przy użyciu **aplikacji dla systemu Android** szablon **Phoneword** rozwiązanie utworzone w [Witaj, Android](~/android/get-started/hello-android/hello-android-quickstart.md) przewodnik. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Wprowadzenie do programu Visual Studio dla komputerów Mac

Program Visual Studio for Mac to bezpłatne, typu open-source środowisko IDE podobne do programu Visual Studio. Zawiera funkcje, w pełni zintegrowane wizualnego projektanta, edytora tekstów, pełną za pomocą narzędzi refaktoryzacji, przeglądarka zestawów i integracji kodu źródłowego. W tym przewodniku dowiesz się używać niektórych podstawowych programu Visual Studio dla komputerów Mac funkcji. Jeśli jesteś nowym użytkownikiem programu Visual Studio dla komputerów Mac, warto zapoznaj się z bardziej szczegółowym [wprowadzenie do programu Visual Studio dla komputerów Mac](https://docs.microsoft.com/visualstudio/mac/).

Program Visual Studio for Mac następuje rozwiązanie programu Visual Studio kod do organizowania _rozwiązania_ i _projektów_. To rozwiązanie jest kontenerem, który może zawierać jeden lub więcej projektów. Projekt może być aplikacji (np. dla systemu iOS lub Android), biblioteka pomocnicze, aplikacja testowa i. W **Phoneword** aplikacji, możesz dodać nowy projekt dla systemu Android przy użyciu **aplikacji dla systemu Android** szablon **Phoneword** rozwiązanie utworzone w [Witaj, Android](~/android/get-started/hello-android/hello-android-quickstart.md) przewodnik.

-----

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinandroid-application"></a>Anatomia aplikacji platformy Xamarin.Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Poniższy zrzut ekranu przedstawiono zawartość tego rozwiązania. Jest to Eksploratorze rozwiązań, który zawiera strukturę katalogów oraz ich wszystkie pliki skojarzone z rozwiązania:

[![Eksplorator rozwiązań](hello-android-deepdive-images/vs/02-solution-structure-sml.png)](hello-android-deepdive-images/vs/02-solution-structure.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Poniższy zrzut ekranu przedstawiono zawartość tego rozwiązania. Jest to konsoli rozwiązania, który zawiera strukturę katalogów oraz ich wszystkie pliki skojarzone z rozwiązaniem:

[![Konsola rozwiązania](hello-android-deepdive-images/xs/02-solution-structure-sml.png)](hello-android-deepdive-images/xs/02-solution-structure.png#lightbox)

-----

To rozwiązanie o nazwie **Phoneword** został utworzony projekt dla systemu Android i **Phoneword** została umieszczona wewnątrz niej.

Przyjrzyj się elementy wewnątrz projektu, aby zobaczyć każdego folderu i jego przeznaczenia:

-   **Właściwości** &ndash; zawiera [AndroidManifest.xml](~/android/platform/android-manifest.md) pliku, który w tym artykule opisano wszystkie wymagania dotyczące aplikacji platformy Xamarin.Android, w tym nazwa, numer wersji i uprawnienia. **Właściwości** folder zawiera także wszystkie [AssemblyInfo.cs](xref:Microsoft.VisualBasic.ApplicationServices.AssemblyInfo), plik metadanych zestawu .NET. Jest dobrym rozwiązaniem, aby wypełnić ten plik z podstawowych informacji o aplikacji.

-   **Odwołania** &ndash; zawiera zestawy wymagany do kompilowania i uruchamiania aplikacji. Po rozwinięciu pola odwołania do katalogu, zobaczysz odwołania do zestawów .NET takich jak [systemu](xref:System), System.Core, i [System.Xml](xref:System.Xml), a także odwołania do zestawu Mono.Android platformy Xamarin.


-   **Zasoby** &ndash; zawiera pliki wymagane przez aplikację do uruchamiania w tym czcionki, lokalne pliki danych i plików tekstowych. Pliki zawarte w tym miejscu są dostępne za pośrednictwem wygenerowany `Assets` klasy. Aby uzyskać więcej informacji na temat elementów zawartości systemu Android, zobacz Xamarin [elementów zawartości przy użyciu systemu Android](~/android/app-fundamentals/resources-in-android/android-assets.md) przewodnik.

-   **Zasoby** &ndash; zawiera zasoby aplikacji, takich jak ciągi, obrazy i układów. Dostęp do tych zasobów w kodzie za pomocą wygenerowany `Resource` klasy. [Zasoby systemu Android](~/android/app-fundamentals/resources-in-android/index.md) przewodnik zawiera szczegółowe informacje o **zasobów** katalogu. Szablon aplikacji również zawiera zwięzły Przewodnik po do zasobów w **AboutResources.txt** pliku.

### <a name="resources"></a>Resources

**Zasobów** katalog zawiera cztery foldery o nazwie **drawable**, **układ**, **mipmappingu** i **wartości**, a także plik o nazwie **Resource.designer.cs**.

Elementy są podsumowane w poniższej tabeli:

-   **drawable** &ndash; DOM drawable katalogi [drawable zasobów](http://developer.android.com/guide/topics/resources/drawable-resource.html) takich jak obrazy i map bitowych.

-   **mipmappingu** &ndash; katalogu mipmappingu przechowywane są pliki drawable dla uruchamiania różnych gęstości ikonę. W szablonie domyślnego katalogu drawable przechowuje plik ikony aplikacji **Icon.png**.


-   **Układ** &ndash; zawiera katalogu układu _pliki projektanta systemu Android_ (axml) definiują interfejs użytkownika dla wszystkich ekranów i działań. Ten szablon tworzy układ domyślny, o nazwie **Main.axml**.

-   **wartości** &ndash; ten katalog zawiera wszystkie pliki XML, które przechowują prostych wartości, takich jak ciągi, liczby całkowite i kolorów. Ten szablon tworzy plik do przechowywania wartości ciągu o nazwie **Strings.xml**.

-   **Resource.Designer.cs** &ndash; znany także jako `Resource` klasy, ten plik jest częściową klasą, która zawiera unikatowe identyfikatory, które są przypisane do każdego zasobu. Ona automatycznie jest tworzony za pomocą narzędzi platformy Xamarin.Android i zostanie ponownie wygenerowany w razie. Ten plik nie należy ręcznie edytować, zgodnie z projektami interfejsu Xamarin.Android zastąpią wszelkie zmiany dokonane ręcznie do niego.


## <a name="app-fundamentals-and-architecture-basics"></a>Podstawy aplikacji i podstawowe informacje dotyczące architektury

Aplikacje systemu android bez pojedynczego punktu wejścia; oznacza to nie ma żadnych jednego wiersza kodu w aplikacji, która wywołuje systemu operacyjnego, aby uruchomić aplikację. Zamiast tego aplikacja zostaje uruchomiona, gdy Android występuje jeden z jej klas, w tym czasie system Android ładowany do pamięci procesu całej aplikacji.

Ta unikatowa funkcja systemu android mogą być bardzo przydatne podczas projektowania skomplikowane aplikacje lub wchodzącymi w interakcje z systemem Android. Jednak te opcje również upewnić Android złożonych zajmujące się podstawowy scenariusz, takich jak **Phoneword** aplikacji. Z tego powodu Eksplorowanie architektury dla systemu Android zostanie podzielona na dwie. Ten przewodnik dissects aplikacji korzystającej z najbardziej typowych punkt wejścia dla aplikacji systemu Android: pierwszy ekran. W [Witaj, Android (wiele ekranów)](~/android/get-started/hello-android-multiscreen/index.md), złożoności pełnego architektury dla systemu Android zbadano zgodnie z opisem są różne sposoby, aby uruchomić aplikację.


### <a name="phoneword-scenario---starting-with-an-activity"></a>Scenariusz Phoneword — począwszy od działania

Po otwarciu **Phoneword** aplikacji po raz pierwszy w emulatora lub urządzenia, system operacyjny tworzy pierwszy *działania*. Działanie to specjalne klasa systemu Android, która odnosi się do ekranu pojedynczej aplikacji i jest odpowiedzialna za rysowania i zapewniająca interfejsu użytkownika. Gdy system Android tworzy pierwsze działanie aplikacji, ładuje całej aplikacji:

[![Działanie obciążenia](hello-android-deepdive-images/01-activity-load-sml.png)](hello-android-deepdive-images/01-activity-load.png#lightbox)

Ponieważ nie ma żadnych progresji liniowej za pośrednictwem aplikacji systemu Android (mogą uruchamiać aplikację z kilku punktów), systemu Android ma unikatowy sposób rejestrowanie informacji o klasy i pliki składa się aplikacja. W **Phoneword** przykładzie wszystkie części, które tworzą aplikacji zostały zarejestrowane przy użyciu specjalnych pliku XML o nazwie **manifestu systemu Android**. Rola **manifestu systemu Android** do śledzenia zawartość aplikacji, właściwości oraz uprawnień i ujawnienia ich do systemu operacyjnego Android. Można potraktować **Phoneword** aplikacji jako jedno działanie (ekran) i kolekcję plików zasobów i pomocnika, które są powiązane ze sobą przy użyciu pliku manifestu systemu Android, jak pokazano na poniższym diagramie:

[![Pomocnicy zasobów](hello-android-deepdive-images/02-resources-helpers-sml.png)](hello-android-deepdive-images/02-resources-helpers.png#lightbox)

Kilka następnych sekcji zbadać relacje między różnymi składnikami **Phoneword** aplikacji; należy udostępnić lepszego zrozumienia na powyższym diagramie. Tej prezentacji rozpoczyna się od interfejsu użytkownika, jak omówiono w nim pliki projektanta i układ systemu Android.


## <a name="user-interface"></a>Interfejs użytkownika

`Main.axml` jest to plik układu interfejsu użytkownika dla pierwszego ekranu w aplikacji. Axml wskazuje, że jest to plik projektanta systemu Android (oznacza AXML *XML dla systemu Android*). Nazwa *Main* dowolnej z punktu widzenia systemu Android &ndash; plik układu może mieć inną nazwę. Po otwarciu **Main.axml** w IDE, wyświetlenie go Edytor visual plików układ systemu Android o nazwie *narzędzia Android Designer*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Narzędzia android Designer](hello-android-deepdive-images/vs/03-android-designer-sml.png "narzędzia Android Designer")](hello-android-deepdive-images/vs/03-android-designer.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Narzędzia android Designer](hello-android-deepdive-images/xs/03-android-designer-sml.png)](hello-android-deepdive-images/xs/03-android-designer.png#lightbox)

-----

W **Phoneword** aplikacji **TranslateButton**jego identyfikator jest ustawiony na `@+id/TranslateButton`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Ustawienia identyfikatora TranslateButton](hello-android-deepdive-images/vs/04-translatebutton-sml.png "TranslateButton ustawienia identyfikatora")](hello-android-deepdive-images/vs/04-translatebutton.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Ustawienia identyfikatora TranslateButton](hello-android-deepdive-images/xs/04-translatebutton-sml.png)](hello-android-deepdive-images/xs/04-translatebutton.png#lightbox)

-----

Po ustawieniu `id` właściwość **TranslateButton**, mapuje narzędzia Android Designer **TranslateButton** kontrolę `Resource` klasy i przypisuje mu *zasobów Identyfikator* z `TranslateButton`. To mapowanie Wizualna kontrola klasy sprawia, że możliwe do lokalizowania i korzystania **TranslateButton** i inne kontrolki w kodzie aplikacji. Zostanie to opisane bardziej szczegółowo, gdy możesz rozbić kod, który obsługuje formanty. Wszystko, czego potrzebujesz, aby dowiedzieć się teraz jest, że reprezentacja kodu kontrolki jest połączony z wizualnej reprezentacji formantu w Projektancie za pośrednictwem `id` właściwości.


### <a name="source-view"></a>Widok źródła

Wszystko, co jest zdefiniowana na powierzchni projektowej jest tłumaczony na plik XML dla platformy Xamarin.Android użyć. Narzędzia Android Designer udostępnia widok źródła, który zawiera kod XML, który został wygenerowany z projektanta wizualnego. Możesz wyświetlić plik XML, przełączając się **źródła** panelu w lewym dolnym rogu widoku projektanta, jak pokazano na poniższym zrzucie ekranu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Widok projektanta źródła](hello-android-deepdive-images/vs/05-source-view-sml.png "projektanta widoku źródła")](hello-android-deepdive-images/vs/05-source-view.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Widok projektanta źródła](hello-android-deepdive-images/xs/05-source-view-sml.png)](hello-android-deepdive-images/xs/05-source-view.png#lightbox)

-----

Ten kod źródłowy XML może zawierać **tekstu (duży)**, **zwykły tekst**, a dwa **przycisk** elementów. Więcej informacji na temat Przewodnik po przykładzie projektanta systemu Android, można znaleźć w Xamarin Android [Przegląd projektanta](~/android/user-interface/android-designer/index.md) przewodnik.

Narzędzia i pojęcia visual część interfejsu użytkownika zostały teraz objęte. Następnie nadszedł czas, aby przejść do kodu, który obsługuje interfejs użytkownika, ponieważ zbadano działań i cyklem życia aktywności.


## <a name="activities-and-the-activity-lifecycle"></a>Działania i cyklem życia aktywności

`Activity` Klasy zawiera kod, który obsługuje interfejs użytkownika.
Działanie jest odpowiedzialny za reagowanie na interakcję z użytkownikiem i tworzenie dynamiki.
Ta sekcja wprowadza `Activity` klasy, w tym artykule omówiono cykl życia aktywności i dissects kod, który obsługuje interfejs użytkownika w **Phoneword** aplikacji.


### <a name="activity-class"></a>Klasa Activity

**Phoneword** aplikacja ma tylko jednego ekranu (działanie). Klasa, która napędza ekranu nosi nazwę `MainActivity` starszy i mieszkał w **MainActivity.cs** pliku. Nazwa `MainActivity` ma specjalnego znaczenia w systemie Android &ndash; nazwę pierwszego działania w aplikacji, mimo że Konwencji `MainActivity`, systemu Android nie istotne, jeśli jest on nazwany coś innego.

Po otwarciu **MainActivity.cs**, możesz zobaczyć, że `MainActivity` klasa jest *podklasy* z `Activity` klasy i że działanie jest powiązany z [działania](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) atrybutu:

```csharp
[Activity (Label = "Phone Word", MainLauncher = true)]
public class MainActivity : Activity
{
  ...
}
```

`Activity` Atrybut rejestruje działanie z manifestu systemu Android; dzięki temu wiadomo, że ta klasa jest częścią systemu Android **Phoneword** aplikacji zarządzanych przez tego manifestu. `Label` Właściwość ustawia tekst, który będzie wyświetlany w górnej części ekranu.

`MainLauncher` Właściwość zawiera informacje dla systemu Android, aby wyświetlić to działanie podczas uruchamiania aplikacji. Ta właściwość staje się ważne, jak dodać więcej działań (ekrany) do aplikacji, jak wyjaśniono w [Witaj, Android (wiele ekranów)](~/android/get-started/hello-android-multiscreen/index.md) przewodnik.

Teraz, gdy podstawy `MainActivity` zostały omówione, nadszedł czas na Dowiedz się więcej na kod działania przez wprowadzenie _cykl życia aktywności_.

### <a name="activity-lifecycle"></a>Cykl życia aktywności

W systemie Android działania przechodzą przez różne etapy cyklu życia, w zależności od ich interakcji z użytkownikiem. Można tworzyć działania, wprowadzenie i wstrzymania, wznowienia i niszczone i tak dalej. `Activity` Klasa zawiera metody, które system wywołuje w określonych punktach w cyklu życia ekranu. Na poniższym diagramie przedstawiono typowy okres ważności działania, a także niektóre odpowiadającej metody cyklu życia:

[![Cykl życia aktywności](hello-android-deepdive-images/04-lifecycle-sml.png)](hello-android-deepdive-images/04-lifecycle.png#lightbox)

Przez zastąpienie `Activity` metod cyklu życia, można kontrolować sposób działania ładuje, jak zachowuje się z użytkownikiem i nawet co się dzieje po znika ono z ekranu urządzenia. Na przykład można zastąpić metody cyklu życia w diagramie powyżej, aby wykonać niektóre ważne zadania:

-   **OnCreate** &ndash; tworzy widoków, inicjuje zmienne i wykonuje inne zadania przygotowywania, które musi zostać wykonane, zanim użytkownik będzie widział działania. Ta metoda jest wywoływana tylko raz, gdy działanie jest ładowany do pamięci. 

-   **OnResume** &ndash; wykonuje wszystkie zadania, które musi się odbyć za każdym razem, gdy działanie wraca do ekranu urządzenia. 

-   **OnPause** &ndash; wykonuje wszystkie zadania, które musi się odbyć za każdym razem, gdy działanie pozostawia ekranu urządzenia.


Po dodaniu niestandardowego kodu do metody cyklu życia w `Activity`, możesz *zastąpienia* tej metody cyklu życia *implementację podstawową*. Wybierz tę wartość do istniejącej metody cyklu życia, (który ma już dołączone do niego kodu) i rozszerzanie tej metody przy użyciu własnego kodu. Możesz wywołać implementację podstawową z wewnątrz swojej metody, aby upewnić się, że oryginalny kod jest uruchamiany zanim nowy kod. Na przykład przedstawiono w następnej sekcji. 

Cykl życia aktywności jest ważne i złożone częścią systemu Android. Jeśli chcesz dowiedzieć się więcej na temat działania po zakończeniu _wprowadzenie_ serii, przeczytaj [cykl życia aktywności](~/android/app-fundamentals/activity-lifecycle/index.md) przewodnik. W tym przewodniku dalej koncentruje się na pierwszym etapie cyklu życia aktywności `OnCreate`.


### <a name="oncreate"></a>OnCreate

Android wywołania `Activity`firmy `OnCreate` metody podczas tworzenia działania (przed ekranu są prezentowane użytkownikowi). Można zastąpić `OnCreate` cyklu życia metodę, aby tworzyć widoki i Przygotuj swoją aktywność w celu spełnienia użytkownika:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);
    // Additional setup code will go here
}
```

W **Phoneword** aplikacji, w pierwszej kolejności należy zrobić w `OnCreate` jest załadować interfejsu użytkownika tworzone za pomocą projektanta systemu Android. Aby załadować interfejsu użytkownika, wywołaj `SetContentView` i przekaż go *zasobu Nazwa układu* pliku układu: `Main.axml`. Układ znajduje się w `Resource.Layout.Main`:

```csharp
SetContentView (Resource.Layout.Main);
```

Gdy `MainActivity` rozpoczyna się, tworzy widok, który jest oparty na zawartość **Main.axml** pliku. Należy pamiętać, że nazwa pliku układu pasuje do nazwy działania &ndash; *Main*axml jest układ *Main*działania. Nie jest to wymagane z punktu widzenia systemu Android, ale po rozpoczęciu dodawania większej liczby ekranów do aplikacji, znajdziesz, że Konwencja nazewnictwa ułatwia pasuje do pliku kod do pliku układu.

Po plik układu jest gotowa, możesz rozpocząć wyszukiwania kontrolki.
Aby wyszukiwać kontrolkę, należy wywołać `FindViewById` i przekaż identyfikator zasobu formantu:

```csharp
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
```

Teraz, gdy odwołania do formantów w pliku układu można rozpocząć programowanie ich odpowiedzi na interakcję z użytkownikiem.


### <a name="responding-to-user-interaction"></a>Reagowanie na interakcję z użytkownikiem

W systemie Android `Click` zdarzeń będzie nasłuchiwać pod kątem touch użytkownika. W tej aplikacji `Click` zdarzeń odbywa się za pomocą wyrażenia lambda, ale delegata lub program obsługi zdarzeń o nazwie można także użyć. Końcowe **TranslateButton** kodu przypominających następujące czynności: 

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

## <a name="testing-deployment-and-finishing-touches"></a>Testowanie, wdrażanie i poprawek

Visual Studio dla komputerów Mac i Visual Studio oferują wiele opcji testowania i wdrażania aplikacji. W tej sekcji omówiono opcje debugowania, pokazuje testowania aplikacji na urządzeniu i wprowadza narzędzia służące do tworzenia aplikacji niestandardowej ikony gęstości inny ekran.


### <a name="debugging-tools"></a>Narzędzia debugowania

Problemów w kodzie aplikacji może być trudny do zdiagnozowania. Aby ułatwić diagnozowanie problemów z złożonym kodzie, można [Ustaw punkt przerwania](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [kod za pomocą kroku](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code), lub [informacji wyjściowych w oknie dziennika](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).


### <a name="deploy-to-a-device"></a>Wdrażanie na urządzeniu

Emulator jest dobry początek, wdrażania i testowania aplikacji, ale użytkownicy nie będą używać ostatecznej aplikacji w emulatorze. Jest dobrą praktyką w celu przetestowania aplikacji na rzeczywistym urządzeniu wcześnie i często.

Zanim urządzenie z systemem Android można używać do testowania aplikacji, należy skonfigurować do tworzenia aplikacji. [Ustaw się urządzenia na potrzeby programowania](~/android/get-started/installation/set-up-device-for-development.md) przewodnik zawiera szczegółowe instrukcje dotyczące przygotowywanie urządzenia do tworzenia aplikacji.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po skonfigurowaniu urządzenia można wdrożyć do niego podłączenie go, wybierając je z **wybierz urządzenie** okien dialogowych i uruchamiania aplikacji:

![Urządzenia wybierz opcję debugowania](hello-android-deepdive-images/vs/06-select-device.png "urządzenia wybierz opcję debugowania")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po skonfigurowaniu urządzenia można wdrożyć do niego podłączenie ją, naciskając klawisz **Start (odtwarzanie)**, wybierając je z **wybierz urządzenie** okien dialogowych i naciskając klawisz **OK**:

[![Urządzenia wybierz opcję debugowania](hello-android-deepdive-images/xs/06-select-device-sml.png)](hello-android-deepdive-images/xs/06-select-device.png#lightbox)

-----

Spowoduje to uruchomienie aplikacji na urządzeniu:

[![Wprowadź Phoneword](hello-android-deepdive-images/05-enter-phoneword-sml.png)](hello-android-deepdive-images/05-enter-phoneword.png#lightbox)


### <a name="set-icons-for-different-screen-densities"></a>Ustawienie ikon dla innego ekranu gęstości

Urządzenia z systemem android są dostępne w różnych rozmiarów ekranu i rozwiązania, a nie wszystkie obrazy wyglądają dobrze na wszystkich ekranach. Na przykład poniżej przedstawiono zrzut ekranu przedstawiający mała gęstość ikony o wysokiej gęstości 5 Nexus. Zwróć uwagę, jak rozmyty go jest porównywany z otaczającego ikon:

[![Ikona rozmyte](hello-android-deepdive-images/06-blurry-icon-sml.png)](hello-android-deepdive-images/06-blurry-icon.png#lightbox)

Aby to uwzględniać, jest dobrym rozwiązaniem jest dodanie ikon różne rozwiązania **zasobów** folderu. Android zawiera różne wersje **mipmappingu** folder, aby obsłużyć ikony uruchamianie różnych gęstości *mdpi* dla nośnika, *hdpi* wysokiego, i  *xhdpi*, *xxhdpi*, *xxxhdpi* dla ekranów o dużej gęstości. Ikony o różnych rozmiarach, są przechowywane w odpowiednim **mipmappingu -** folderów:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![folderach mipmap](hello-android-deepdive-images/vs/07-mipmap-folders.png "folderach mipmap")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Folderach Mipmap](hello-android-deepdive-images/xs/07-mipmap-folders-sml.png)](hello-android-deepdive-images/xs/07-mipmap-folders.png#lightbox)

-----

Android wybierze ikonę z odpowiedniej gęstości:

[![Ikony w odpowiedniej gęstości](hello-android-deepdive-images/07-appropriate-density-sml.png)](hello-android-deepdive-images/07-appropriate-density.png#lightbox)

### <a name="generate-custom-icons"></a>Generowanie niestandardowych ikon

Wszyscy nie ma dostępnych do tworzenia niestandardowych ikon i uruchamianie obrazów, które aplikacja potrzebuje wyróżniały się projektanta. Oto kilka innych sposobów generowania kompozycji aplikacji niestandardowej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   [Android Studio zasobów](http://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; generator opartego na sieci web, w przeglądarce dla wszystkich typów ikon dla systemu Android, wraz z łączami do innych narzędzi społeczności przydatne. Działa najlepiej w przeglądarce Google Chrome.

-   Program Visual Studio &ndash; służy to do tworzenia proste ikony, ustaw dla swojej aplikacji bezpośrednio w środowisku IDE.

-   [Glyphish](http://www.glyphish.com/) &ndash; wbudowanych ikony o wysokiej jakości zestawy za darmo pobierania i możliwości zakupu.

-   [Fiverr](http://www.fiverr.com/) &ndash; wybierz z wielu projektantów do tworzenia ikony ustawiane, zaczynając od 5 USD. Może być hit lub miss, ale odpowiedni zasób, jeśli potrzebujesz ikony przeznaczony na bieżąco.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   [Android Studio zasobów](http://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; generator opartego na sieci web, w przeglądarce dla wszystkich typów ikon dla systemu Android, wraz z łączami do innych narzędzi społeczności przydatne. Działa najlepiej w przeglądarce Google Chrome.

-   [Rysunek 3](https://itunes.apple.com/us/app/sketch/id852320343?mt=12) &ndash; szkicu jest aplikacja dla komputerów Mac dotyczące projektowania interfejsów użytkownika, ikon i nie tylko. To jest aplikacja, który został użyty do projektowania zestawu ikon aplikacji platformy Xamarin i uruchamianie obrazów. Rysunek 3 jest dostępna w Store aplikacji, a koszt wynosi około 80 $. Możesz wypróbować bezpłatną [narzędzie szkic](http://bohemiancoding.com/sketch/tool/) także.

-   [Pixelmator](http://www.pixelmator.com/) &ndash; wszechstronny obraz, edytowanie aplikacji dla komputerów Mac, który koszt wynosi około 30 USD.

-   [Glyphish](http://www.glyphish.com/) &ndash; wbudowanych ikony o wysokiej jakości zestawy za darmo pobierania i możliwości zakupu.

-   [Fiverr](http://www.fiverr.com/) &ndash; wybierz z wielu projektantów do tworzenia ikony ustawiane, zaczynając od 5 USD. Może być hit lub miss, ale odpowiedni zasób, jeśli potrzebujesz ikony przeznaczony na bieżąco.

-----

Aby uzyskać więcej informacji na temat rozmiarów ikonę i wymagań dotyczą [zasoby systemu Android](~/android/app-fundamentals/resources-in-android/index.md) przewodnik.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="adding-google-play-services-packages"></a>Dodawanie sklepu Google Play Services pakietów

_Usługi Google Play_ to zestaw bibliotek dodatku, dzięki której deweloperzy systemu Android móc korzystać z najnowszych funkcji od firmy Google, takich jak Google Maps, Google Cloud Messaging i rozliczenia w aplikacji.
Wcześniej powiązania z wszystkie biblioteki usług Google Play nie podał platformy Xamarin w postaci jednego pakietu &ndash; począwszy od programu Visual Studio dla komputerów Mac, okno dialogowe nowego projektu jest dostępne do wybierania, które pakiety usług Google Play, które mają zostać objęte Twoja aplikacja.

Aby dodać co najmniej jedną bibliotekę usługi usługi Google Play, kliknij prawym przyciskiem myszy **pakietów** węzeł w drzewie projektu i kliknij przycisk **Dodaj usługę Google Play...** :

[![Dodaj usługę Google Play](hello-android-deepdive-images/xs/08-add-google-play-services-sml.png)](hello-android-deepdive-images/xs/08-add-google-play-services.png#lightbox)

Gdy **Dodawanie usług Google Play** zobaczy okno dialogowe, wybierz pakiety (rozszerzeń nuget), które chcesz dodać do projektu:

[![Wybierz pakiety](hello-android-deepdive-images/xs/09-add-dialog-sml.png)](hello-android-deepdive-images/xs/09-add-dialog.png#lightbox)

Po wybierz usługę i kliknięciu **Dodaj pakiet**, Visual Studio for Mac pobiera i instaluje pakiet, możesz wybrać, a także wszelkie zależne pakiety usług Google Play, które są wymagane. W niektórych przypadkach może zostać wyświetlony **akceptacja licencji** okno dialogowe, które wymaga, aby **Akceptuj** przed zainstalowanych pakietów:

[![Akceptacja licencji](hello-android-deepdive-images/xs/10-license-acceptance-sml.png)](hello-android-deepdive-images/xs/10-license-acceptance.png#lightbox)

-----

## <a name="summary"></a>Podsumowanie

Gratulacje! Teraz masz pełny opis składniki aplikacji platformy Xamarin.Android, a także narzędzia wymagane do jego utworzenia.

W następnym samouczku z _wprowadzenie_ serii, będzie rozszerzyć aplikację w celu obsługi wielu ekranów, gdy eksplorujesz pojęcia i bardziej zaawansowanych architektury dla systemu Android.
