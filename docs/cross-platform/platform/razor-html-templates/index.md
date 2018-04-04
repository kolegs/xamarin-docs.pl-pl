---
title: Tworzenie widoków HTML za pomocą szablonów Razor
description: " Do renderowania elementów HTML za pomocą pełnego ekranu strony sieci Web może być prosty i skuteczny sposób renderowania złożone formatowanie w sposób między platformami, zwłaszcza, jeśli masz już HTML, Javascript i CSS z projektu witryny sieci Web."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: fa361e48f8f7e236a3295deda2d80a02ef06b34d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="building-html-views-using-razor-templates"></a>Tworzenie widoków HTML za pomocą szablonów Razor

W świecie przenośnych programowanie termin "aplikacja hybrydowe" odnosi się zazwyczaj do aplikacji, która przedstawia niektóre lub wszystkie jego ekrany jako strony HTML w formant przeglądarki sieci web hostowanej.

Brak niektórych środowiskach programistycznych, które pozwalają kompilowania aplikacji mobilnej w całości kodu HTML i Javascript, jednak te aplikacje mogą występować z problemów z wydajnością, podczas próby wykonania złożonych obliczeń lub efekty interfejsu użytkownika i są również ograniczony do platformy Funkcje, które mogą uzyskiwać dostęp do.

Xamarin oferuje najlepsze cechy obu tych środowisk, szczególnie w przypadku, gdy przy użyciu aparatu tworzenia szablonów HTML elementu Razor. Za pomocą platformy Xamarin masz możliwość tworzenia wielu platform opartego na szablonie HTML widoki, które używać Javascript i CSS, ale również mają pełny dostęp do podstawowej platformy interfejsów API i szybkie przetwarzanie przy użyciu języka C#.

Tym dokumencie opisano sposób użycia Razor, HTML + Javascript + CSS widoków, które mogą być używane przez platformy urządzeń przenośnych za pomocą platformy Xamarin kompilacji aparatu tworzenia szablonów.

## <a name="using-web-views-programmatically"></a>Programowo przy użyciu widoków sieci Web

Zanim firma Microsoft Dowiedz się więcej o Razor w tej sekcji opisano sposób użycia widoki sieci web do wyświetlania zawartości HTML bezpośrednio — w szczególności zawartość HTML, który zostanie wygenerowany wewnątrz aplikacji.

Xamarin zapewnia pełny dostęp do podstawowej platformy interfejsów API zarówno dla systemu iOS i Android, dzięki czemu łatwiej można tworzyć i wyświetlać HTML przy użyciu języka C#. Poniżej przedstawiono podstawowe składni dla każdej platformy.

### <a name="ios"></a>iOS

Również wyświetlanie HTML w formancie UIWebView w Xamarin.iOS zajmuje zaledwie kilku wierszach kodu:

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

Zobacz [iOS UIWebView](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) przepisami, aby uzyskać więcej informacji na temat używania kontroli UIWebView.

### <a name="android"></a>Android

Wyświetlanie kodu HTML w kontrolce WebView przy użyciu platformy Xamarin.Android odbywa się w kilku wierszach kodu:

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

Zobacz [Android WebView](http://docs.xamarin.com/recipes/android/controls/webview/) przepisami, aby uzyskać więcej informacji na temat używania kontrolki WebView.

### <a name="specifying-the-base-directory"></a>Określanie podstawowego katalogu

Na obu platform istnieje parametr, który określa podstawowego katalogu dla strony HTML. Jest to lokalizacja, w systemie plików urządzenia, który jest używany do rozpoznawania względnych odwołania do zasobów, takich jak obrazy i pliki CSS. Na przykład takich jak tagi

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

odwołuje się do tych plików: **style.css**, **monkey.jpg** i **jscript.js**. Ustawienie katalogu podstawowego informuje widoku sieci web, w którym znajdują się te pliki, mogą być ładowane do strony.

#### <a name="ios"></a>iOS

Dane wyjściowe szablonu jest renderowany w systemie iOS z następującym kodem C#:

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

Katalog podstawowy jest określony jako `NSBundle.MainBundle.BundleUrl` które odwołuje się do katalogu, w którym aplikacja jest zainstalowana w. Wszystkie pliki w **zasobów** folderu są kopiowane do tej lokalizacji, takich jak **style.css** pliku pokazano poniżej:

 ![rozwiązanie iPhoneHybrid](images/image1_240x163.png)

Akcja kompilacji dla wszystkich plików zawartości statycznej powinny być **BundleResource**:

 ![Akcja kompilacji projektu iOS: BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

System android wymaga również katalogu podstawowego można przekazać jako parametru, gdy html ciągów są wyświetlane w widoku sieci web.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

Ciąg specjalne **file:///android_asset/** odwołuje się do folderu Zasoby systemu Android w aplikacji, pokazano tutaj zawierający **style.css** pliku.

 ![Rozwiązanie AndroidHybrid](images/image3_240x167.png)

Akcja kompilacji dla wszystkich plików zawartości statycznej powinny być **AndroidAsset**.

 ![Akcja kompilacji projektu systemu android: AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>Wywoływanie C# z kodu HTML i Javascript

Po załadowaniu strony html w widoku sieci web traktuje łącza i formularze będzie strony został załadowany z serwera. Oznacza to, że jeśli użytkownik kliknie łącze lub przesłaniu formularza widoku sieci web będzie podejmować próby przejdź do określonego obiektu docelowego.

Jeśli link do zewnętrznego serwera (na przykład google.com) widoku sieci web będzie podejmować próby załadowania zewnętrznej witryny sieci Web (przy założeniu, że istnieje połączenie internetowe).

```html
<a href="http://google.com/">Google</a>
```

Jeśli połączenie jest względna widoku sieci web będzie podejmować próby załadowania tej zawartości z katalogu podstawowego. Oczywiście żadne sieci połączenie nie jest wymagane, aby to zrobić, ponieważ zawartość jest przechowywana w aplikacji na urządzeniu.

```html
<a href="somepage.html">Local content</a>
```

Akcje formularza wykonaj tę samą zasadę.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

Użytkownik nie chce hosta serwera sieci web na komputerze klienckim; jednak można użyć tych samych metod komunikacji serwera stosowanych w wzorce projektowe reakcji współczesnych do wywołania usługi za pośrednictwem HTTP GET i obsługują odpowiedzi asynchronicznie przez emitowanie kodu Javascript (lub wywołania języka Javascript już umieszczony w widoku sieci web). Dzięki temu można łatwo przekazywania danych z kodu HTML do kodu C# dla przetwarzania, a następnie Wyświetl wyniki z powrotem na stronie HTML.

Zarówno dla systemu iOS i Android mechanizm umożliwiający kod aplikacji w celu przechwycenia te zdarzenia nawigacji, dzięki czemu kod aplikacji może odpowiadać (jeśli jest to wymagane). Ta funkcja jest niezwykle istotne tworzenie hybrydowych aplikacji, ponieważ umożliwia interakcję z widoku sieci web kodu natywnego.

#### <a name="ios"></a>iOS

Zdarzenia ShouldStartLoad w widoku sieci web w systemie iOS może zostać zastąpiona w celu kodu aplikacji do obsługi żądania nawigacji (na przykład w przypadku kliknięcia łącza). Parametry metody dostarczania wszelkich informacji

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

a następnie przypisać programu obsługi zdarzeń:

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

W systemie Android po prostu podklasy WebViewClient, a następnie zaimplementuj kod, aby odpowiadać na żądania nawigacji.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, string url) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

a następnie ustaw klienta w widoku sieci web:

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>Wywoływanie kodu Javascript w języku C#

Oprócz informuje widok sieci web do załadowania strony HTML, kod w języku C# można również uruchomić Javascript w aktualnie wyświetlaną stronę. Cały bloki kodu Javascript może być utworzony przy użyciu języka C# ciągów i wykonać lub można sformułować wywołań metody Javascript jest już dostępne na stronie za pośrednictwem `script` tagów.

#### <a name="android"></a>Android

Tworzenie kodu Javascript do wykonania, a następnie poprzedzić go "javascript:" oraz poinstruuj widoku sieci web, aby załadować ten ciąg:

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

Widoki sieci web dla systemu iOS umożliwiają przeznaczony do wywołania języka Javascript:

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>Podsumowanie

Ta sekcja została wprowadzona funkcje kontrolki widoku sieci web na Android i iOS, które umożliwiają nam tworzenie hybrydowych aplikacji za pomocą platformy Xamarin, w tym:

-  Możliwość ładowania z ciągów generowane w kodzie HTML
-  Możliwość odwołania lokalnych plików (CSS, Javascript, obrazy lub inne pliki HTML)
-  Zdolność do przechwycenia żądań nawigacji w kodzie języka C#
-  Możliwość wywołania języka Javascript z kodu C#.


Następna sekcja wprowadza Razor, co ułatwia tworzenie HTML do użycia w aplikacjach hybrydowego.

## <a name="what-is-razor"></a>Co to jest Razor?

Razor to aparat tworzenia szablonów, która została wprowadzona w programie ASP.NET MVC, pierwotnie do uruchamiania na serwerze i generują kod HTML do przeglądarki użytkownika.

Aparatu Razor tworzenia szablonów rozszerza standardowej składni HTML w języku C#, dzięki czemu można express układ i łatwo zastosować arkusze stylów CSS i Javascript. Szablon można odwoływać się do klasy modelu, które mogą być dowolnego typu niestandardowego i którego właściwości są dostępne bezpośrednio z szablonu. Jednym z jej główne zalety jest możliwość łatwego mieszać składni HTML i C#.

Szablony razor nie są ograniczone do użycia po stronie serwera, również mogły one zostać uwzględnione w aplikacji platformy Xamarin. Praca z widokami web programowo przy użyciu szablony Razor wraz z możliwością umożliwia zaawansowane hybrydowych i platform aplikacji ma zostać utworzony za pomocą platformy Xamarin.

### <a name="razor-template-basics"></a>Podstawy szablonu razor

Pliki szablonów razor ma **.cshtml** rozszerzenia pliku. Może być dodany do projektu Xamarin z sekcji tworzenia szablonów tekstowych w **nowy plik** okna dialogowego:

 ![Nowy plik — szablonu Razor](images/image5_400x201.png)

Proste szablonu Razor ( **RazorView.cshtml**) są wyświetlane poniżej.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

Zwróć uwagę następujące różnice zwykły plik HTML:

-  `@` Symbol ma specjalne znaczenie w szablonach Razor — go wskazuje, że poniższe wyrażenie jest C# ma zostać obliczone.
- `@model` dyrektywa jest zawsze wyświetlany jako pierwszy wiersz pliku szablonu Razor.
-  `@model` Dyrektywy powinno następować typu. W tym przykładzie prostego ciągu jest przekazywany do szablonu, ale może to być wszystkie klasy niestandardowej.
-  Gdy `@Model` jest przywoływany w szablonie zawiera odwołanie do obiektu przekazanych do szablonu po jego wygenerowaniu (w tym przykładzie będzie ciąg).
-  IDE automatycznie wygeneruje częściowej klasy szablonów (pliki z **.cshtml** rozszerzenia). Ten kod jest widoczny, ale nie powinny być zmieniane.
 ![RazorView.cshtml](images/image6_125x34.png) częściowej klasy o nazwie RazorView odpowiadające .cshtml nazwa pliku szablonu. Jest to nazwa używana do odwoływania się do szablonu w kodzie języka C#.
- `@using` instrukcje może także stanowić w górnej części szablonu Razor, aby dołączyć dodatkowe przestrzenie nazw.


Następnie można wygenerować ostateczne dane wyjściowe HTML za pomocą następującego kodu C#. Należy pamiętać, że określono modelu, który ma być ciągiem "Hello World", które zostaną uwzględnione w renderowanym danych wyjściowych.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

Poniżej przedstawiono dane wyjściowe w widoku sieci web, w systemie iOS Simulator i emulatora systemu Android:

 ![Witaj Świecie](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>Więcej składni Razor

W tej sekcji, którą spróbujemy wprowadzenie niektórych podstawowa składnia Razor, aby pomóc Ci rozpocząć za pomocą go. Przykłady w tej sekcji wypełnij następujące klasy z danymi i wyświetl ją przy użyciu Razor:

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

Wszystkie przykłady użyć poniższego kodu inicjowania danych

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>Wyświetlanie właściwości modelu

Jeśli model jest klasy przy użyciu właściwości, ich łatwo odwołuje się szablonu Razor opisane w tym przykładzie szablonie:

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

Może to być renderowana na ciąg, używając następującego kodu:

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

Ostateczne dane wyjściowe są tutaj wyświetlane w widoku sieci web w systemie iOS Simulator i emulatora systemu Android:

 ![Rupert](images/image8_516x160.png)

#### <a name="c-statements"></a>C# — instrukcje

Bardziej złożone C# można uwzględnić w szablonie, takie jak aktualizacje właściwości modelu i obliczeń wiek, w tym przykładzie:

```html
@model Monkey
<html>
    <body>
    @{
        Model.Name = "Rupert X. Monkey";
        Model.Birthday = new DateTime(2011,3,1);
    }
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    </body>
</html>
```

Jednowierszowe C# wyrażenia złożone (takie jak formatowanie wiek) można pisać przy otaczającego kodu przy użyciu `@()`.

Wiele instrukcji "C#" mogą być zapisywane przez umieszczenie ich z `@{}`.

#### <a name="if-else-statements"></a>If-else-instrukcje

Gałęzie kodu może zostać wyrażona z `@if` opisane w tym przykładzie szablon.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <p>@Model.FavoriteFoods.Count favorites</p>
    }
    </body>
</html>
```

#### <a name="loops"></a>Pętle

Zapętlenie konstrukcje, takich jak `foreach` można również dodać. `@` Na zmienna pętli for można użyć prefiksu ( `@food` w takim przypadku) do renderowania jej w formacie HTML.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <ul>
            @foreach (var food in @Model.FavoriteFoods) {
                <li>@food</li>
            }
        </ul>
    }
    </body>
</html>
```

Dane wyjściowe szablonu powyżej przedstawiono uruchomiony w systemie iOS Simulator i emulatora systemu Android:

 ![Rupert małp X](images/image9_520x277.png)

Ta sekcja ma omówione podstawy za pomocą szablonów Razor do renderowania proste widokach tylko do odczytu. W następnej sekcji objaśniono sposób tworzenia bardziej szczegółowy aplikacji przy użyciu Razor, które akceptuje dane wejściowe użytkownika i współdziałanie między Javascript w widoku HTML i C#.

## <a name="using-razor-templates-with-xamarin"></a>Za pomocą szablonów Razor za pomocą platformy Xamarin

W tej sekcji opisano sposób korzystania kompilacji hybrydowych aplikacji przy użyciu szablony rozwiązań w programie Visual Studio dla komputerów Mac. Dostępne są trzy szablony **Plik > Nowy > rozwiązania...**  okno:

- **Android > aplikacji > aplikacji systemu Android widoku sieci Web**
- **iOS > aplikacji > Aplikacja widoku sieci Web**
- **Projektu programu ASP.NET MVC**



**Nowe rozwiązanie** okno wygląda następująco iPhone i projektów w systemie Android — opis rozwiązania po prawej stronie wyróżnia obsługę tworzenia szablonów aparatu Razor.

 ![Tworzenie iPhone i rozwiązania dla systemu Android](images/image13_1139x959.png)

Należy pamiętać, że możesz łatwo dodać **.cshtml** szablonu Razor do *żadnych* istniejącego projektu Xamarin, nie jest konieczne użycie tych szablonów rozwiązania. projekty dla systemu iOS nie wymagają scenorysu używać Razor; po prostu Dodaj formant UIWebView do dowolnego widoku programowo i umożliwiający renderowanie szablony Razor całego w kodzie języka C#.

Poniżej przedstawiono zawartość rozwiązania szablonu domyślnego dla urządzenia iPhone i projektów w systemie Android:

 ![urządzenia iPhone i szablony dla systemu Android](images/image10_428x310.png)

Szablony umożliwiają infrastruktury aplikacji gotowych do przejdź do ładowania szablonu Razor z obiektu modelu danych, przetwarzania danych wejściowych użytkownika i poinformować użytkowników za pośrednictwem kodu Javascript.

Ważne części rozwiązania są:

-  Zawartość statyczna, takich jak **style.css** pliku.
-  Pliki szablonów .cshtml razor, takich jak **RazorView.cshtml** .
-  Model klasy, do których istnieją odwołania w szablony Razor, takich jak **ExampleModel.cs** .
-  Klasy specyficzne dla platformy, która tworzy widok sieci web i renderowania szablonu, takie jak `MainActivity` w systemie Android i `iPhoneHybridViewController` w systemie iOS.


W poniższej sekcji omówiono sposób działania projektów.

### <a name="static-content"></a>Zawartość statyczna

Zawartość statyczna zawiera arkusze stylów CSS, obrazów, plików Javascript lub innej zawartości, które mogą być połączone z lub odwołuje się plik HTML, będzie wyświetlany w widoku sieci web.

Projekty szablonu obejmują arkusz stylów minimalnego pokazujący sposób dodać zawartość statyczną w aplikacji hybrydowych. Arkusz stylów CSS odwołuje się do szablonu następująco:

```html
<link rel="stylesheet" href="style.css" />
```

Możesz dodać niezależnie od arkusza stylów i pliki Javascript potrzebujesz, łącznie z platform, takich jak JQuery.

### <a name="razor-cshtml-templates"></a>Cshtml razor szablonów

Szablon zawiera Razor **.cshtml** pliku, który został wcześniej zapisany kod, aby pomóc przekazywania danych między HTML/Javascript i C#. Pozwoli to kompilacji hybrydowego zaawansowane aplikacje, które nie tylko wyświetlanie tylko do odczytu danych z modelu, ale również akceptuje dane wejściowe użytkownika w kodzie HTML i przekaż go z powrotem do kodu C# dla przetwarzania lub magazynu.

#### <a name="rendering-the-template"></a>Renderowania szablonu

Wywoływanie `GenerateString` w szablonie spowoduje renderowanie kodu HTML gotowy do wyświetlenia w widoku sieci web. Jeśli szablon korzysta z modelu, a następnie należy dostarczyć przed renderowaniem. Ten diagram przedstawia sposób renderowania działania — nie który zasoby statyczne są rozpoznawane przez widok sieci web w czasie wykonywania, aby znaleźć określone pliki przy użyciu dostarczonego katalogu podstawowego.

 ![Schemat blokowy razor](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>Wywoływanie kodu C# z szablonu

Komunikacja z widoku sieci web renderowanych wywołań zwrotnych do języka C# odbywa się przez ustawienie adresu URL dla widoku sieci web, a następnie przechwytywaniu żądania w języku C# do obsługi natywnych żądania bez ponownego ładowania widoku sieci web.

Przykładem może być widoczny w sposób obsługi przez RazorView przycisku. Przycisk ma poniższy kod HTML:

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

`InvokeCSharpWithFormValues` Funkcji Javascript odczytuje wszystkie wartości z formularza HTML i zestawy `location.href` widoku sieci web:

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

To próbuje przejdź do widoku sieci web do adresu URL za schematu niestandardowego (np.) `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

Podczas widoku sieci web natywnego przetwarzania tego żądania nawigacji, będziemy mieć możliwość przechwycić go. W systemie iOS jest to realizowane przez obsługi zdarzeń HandleShouldStartLoad UIWebView. W systemie Android możemy po prostu podklasy WebViewClient używany w formularzu i zastąpić ShouldOverrideUrlLoading.

Funkcje wewnętrzne tych dwóch nawigacji interceptory jest zasadniczo taki sam.

Najpierw sprawdź adres URL, który próbuje załadować, widoku sieci web, a jeśli go nie zaczyna się od schematu niestandardowego (`hybrid:`), Zezwalaj na normalne nawigacji występują.

Dla schematu niestandardowego adresu URL, wszystkie elementy w adresie URL między systemu oraz "?" jest to nazwa metody do obsługi (w tym przypadku "UpdateLabel"). Wszystkie elementy w ciągu zapytania będą traktowane jako parametrów do wywołania metody:

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` w tym przykładzie jest skraca manipulowanie ciągami w parametrze pola tekstowego (dołączanie "C# mówi" jak ciąg), a następnie wywołuje wstecz widoku sieci web.

Po obsługi adres URL, metoda przerywa nawigacji, aby widoku sieci web nie jest podejmowana próba zakończenia, przechodząc do niestandardowy adres URL.

#### <a name="manipulating-the-template-from-c"></a>Manipulowanie szablonu w języku C#

Komunikacja z był renderowany widok HTML w sieci web w języku C# odbywa się za pomocą funkcji Javascript w widoku sieci web. W systemach iOS, odbywa się przez wywołanie metody `EvaluateJavascript` na UIWebView:

```csharp
webView.EvaluateJavascript (js);
```

W systemie Android, Javascript, może być wywoływany w widoku sieci web przez ładowanie Javascript jako adres URL przy użyciu `"javascript:"` schemat adresu URL:

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>Tworzenie aplikacji hybrydowych naprawdę

Te szablony nie należy wprowadzać użyj natywnych kontrolek na każdej platformie — cały ekran jest wypełniony widoku pojedynczej sieci web.

HTML może być doskonałe rozwiązanie dla prototypowania i wyświetlanie rodzaje elementów sieci web najlepiej jest na przykład tekstu rich text i elastyczny układ. Na przykład, jednak nie wszystkie zadania są odpowiednie do kodu HTML i Javascript — przewijanie długich list danych, wykonuje lepiej przy użyciu natywnych kontrolek interfejsu użytkownika, takie jak (UITableView w systemie iOS) lub ListView w systemie Android.

Widoki sieci web w szablonie łatwo można rozszerzyć za pomocą formantów specyficzne dla platformy — po prostu edycji **MainStoryboard.storyboard** w Projektancie systemu iOS lub **Resources/layout/Main.axml** w systemie Android.

### <a name="razortodo-sample"></a>Przykładowe RazorTodo

[RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) repozytorium zawiera dwa oddzielne rozwiązania przedstawiał różnice między aplikacją całkowicie opartej na HTML i aplikacji, która łączy HTML z kontrolki natywne:

-  **RazorTodo** -driven całkowicie HTML aplikacji za pomocą szablonów Razor.
-  **RazorNativeTodo** — używa kontrolki widok listy macierzystego dla systemów iOS i Android, ale wyświetla ekran edycji HTML i Razor.


Te aplikacje platformy Xamarin Uruchom zarówno dla systemu iOS i Android przy użyciu przenośnej biblioteki klas (PCLs), aby udostępnić typowy kod, takich jak klasy bazy danych i modelu. Razor **.cshtml** szablonów można również uwzględnić w PCL aby łatwo są one udostępniane na platformach.

Obie aplikacje przykładowe dołączyć do nich udostępniania Twitter i zamiany tekstu na mowę interfejsy API z natywnego platformy, z którego wynika, że hybrydowych aplikacji za pomocą platformy Xamarin nadal mieć dostęp do podstawowych funkcji z widoków opartych na szablonie HTML w silniku Razor.

**RazorTodo** aplikacja korzysta z szablonów HTML w silniku Razor dla widoków listy i edycji. Oznacza to, że budujemy aplikacji prawie całkowicie w udostępnionym przenośnej biblioteki klas (łącznie z bazą danych i **.cshtml** szablony Razor). Poniższe zrzuty ekranu pokazują, iOS i aplikacje dla systemu Android.

 ![RazorTodo](images/Both_700x290.png)

**RazorNativeTodo** aplikacja używa szablonu HTML w silniku Razor dla widoku edycji, ale implementuje natywnego przewijanej listy na każdej z platform. To zapewnia szereg korzyści w tym:

-  Wydajność — kontrolki przewijania natywne używają wirtualizacji, aby zapewnić, szybkie i sprawne przewijanie nawet w przypadku list bardzo dużo danych.
-  Natywnym środowiskiem — elementy interfejsu użytkownika specyficzne dla platformy są łatwo włączone, takie jak obsługa indeksu przewijanie fast w systemach iOS i Android.


Najważniejszą korzyścią z tworzenie hybrydowych aplikacji za pomocą platformy Xamarin jest można uruchomić z interfejsem użytkownika całkowicie opartej na HTML (np. przykład pierwszy) i następnie dodać funkcje specyficzne dla platformy na żądanie (jak przedstawiono przykładowe na drugi). Ekrany macierzysty listy i HTML w silniku Razor Edytuj ekrany na obu systemów iOS i Android są wyświetlane poniżej.

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>Podsumowanie

Ten artykuł zawiera wyjaśniono funkcje dostępne kontrolki widoku sieci web w systemach iOS i Android, które ułatwiają tworzenie hybrydowych aplikacji.

Omówione następnie aparatu Razor tworzenia szablonów i składnię, która może służyć do generowania HTML łatwe w użyciu aplikacji Xamarin. **cshtml** plików szablonu Razor. Dla komputerów Mac szablony rozwiązań, które umożliwiają szybkie rozpoczęcie pracy Tworzenie hybrydowych aplikacji za pomocą platformy Xamarin są także opisane programu Visual Studio.

Na koniec spowodował próbki RazorTodo, które pokazują, jak połączyć widoki sieci web z interfejsów API i interfejsów użytkownika macierzystego.

### <a name="related-links"></a>Linki pokrewne

- [RazorTodo Sample](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3 - aparatu widoku Razor (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Wprowadzenie do programowania sieci Web ASP.NET przy użyciu składni Razor (Microsoft)](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)