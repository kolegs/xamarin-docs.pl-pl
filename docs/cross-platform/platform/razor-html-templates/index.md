---
title: Tworzenie widoki HTML przy użyciu szablonów Razor
description: " Używanie pełnego ekranu strony sieci Web do renderowania elementów HTML może być prosty i efektywny sposób renderowania złożone formatowanie w sposób bezpieczny dla wielu platform, zwłaszcza, jeśli masz już HTML, Javascript i CSS z projektu witryny sieci Web."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: asb3993
ms.author: amburns
ms.date: 07/24/2018
ms.openlocfilehash: 7e569aaddef912d9534e98f2f987ad5dfca8a5a6
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270135"
---
# <a name="building-html-views-using-razor-templates"></a>Tworzenie widoki HTML przy użyciu szablonów Razor

W świecie programowania aplikacji mobilnych na zazwyczaj termin "aplikacja hybrydowa" odwołuje się do aplikacji, która przedstawia niektóre lub wszystkie jego ekrany jako strony HTML wewnątrz formantu przeglądarki sieci web hostowanej.

Istnieją pewne środowiskach programistycznych, które pozwalają kompilowania aplikacji mobilnej całkowicie w kodzie HTML i Javascript, jednak te aplikacje mogą występować z problemów z wydajnością podczas próby wykonania złożone przetwarzanie lub efektów interfejsu użytkownika i są również są ograniczone do platformy Funkcje, których mogą uzyskiwać dostęp.

Xamarin oferuje zalety obu rozwiązań, szczególnie w przypadku, gdy przy użyciu aparatu tworzenia szablonów Razor HTML. Za pomocą platformy Xamarin masz swobodę tworzenia dla wielu platform oparte na szablonach HTML widoki, które Wykorzystaj języki Javascript i CSS, ale także mieć pełny dostęp do podstawowej platformy interfejsów API i szybkie przetwarzanie, przy użyciu języka C#.

W tym dokumencie wyjaśniono, jak używać Razor aparatu tworzenia szablonów kompilacji widoki HTML + Javascript + CSS, które można stosować w przypadku platform urządzeń przenośnych za pomocą platformy Xamarin.

## <a name="using-web-views-programmatically"></a>Programowe korzystanie z widoków w sieci Web

Zanim firma Dowiedz się więcej o Razor w tej sekcji omówiono sposób używania widoki sieci web do wyświetlania zawartości HTML bezpośrednio — w szczególności zawartość HTML, który jest generowany w obrębie aplikacji.

Środowisko Xamarin zapewnia pełny dostęp do podstawowych interfejsów API platformy dla systemów iOS i Android, dzięki czemu można łatwo utworzyć i wyświetlić HTML przy użyciu języka C#. Poniżej przedstawiono podstawową składnię dla każdej platformy.

### <a name="ios"></a>iOS

Wyświetlanie kodu HTML w formancie UIWebView w rozszerzeniu Xamarin.iOS pobiera również zaledwie kilku wierszy kodu:

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

Zobacz [iOS UIWebView](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) przepisy, aby uzyskać więcej informacji na temat korzystania z formantu UIWebView.

### <a name="android"></a>Android

Wyświetlanie kodu HTML w kontrolce WebView, korzystając z platformy Xamarin.Android odbywa się w kilku wierszach kodu:

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);

// enable Javascript execution in your html view so you can provide "alerts" and other js
webView.SetWebChromeClient(new WebChromeClient());

var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

Zobacz [aplikacja WebView systemu Android](http://docs.xamarin.com/recipes/android/controls/webview/) przepisy, aby uzyskać więcej informacji na temat korzystania z kontrolki WebView.

### <a name="specifying-the-base-directory"></a>Określanie podstawowego katalogu

Na obu platformach ma parametr, który określa katalog podstawowy dla stron HTML. Jest to lokalizacja w systemie plików urządzenia, który jest używany do rozpoznawania względnych odwołania do zasobów, takich jak obrazy i pliki CSS. Na przykład takich jak tagi

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

odwołują się do tych plików: **style.css**, **monkey.jpg** i **jscript.js**. Ustawienie katalog podstawowy informuje widoku sieci web, w którym znajdują się te pliki, dzięki czemu mogą być ładowane do strony.

#### <a name="ios"></a>iOS

Dane wyjściowe szablonu jest renderowany w systemie iOS przy użyciu następującego kodu C#:

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

Katalog podstawowy jest określony jako `NSBundle.MainBundle.BundleUrl` która odnosi się do katalogu, w którym aplikacja jest zainstalowana w. Wszystkie pliki w **zasobów** folderu są kopiowane do tej lokalizacji, takich jak **style.css** pliku pokazano poniżej:

 ![rozwiązanie iPhoneHybrid](images/image1_240x163.png)

Akcja kompilacji dla wszystkich plików zawartości statycznej powinny być **BundleResource**:

 ![Akcja kompilacji projektu dla systemu iOS: BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

System android wymaga również podstawowego katalogu, które zostaną przekazane jako parametr gdy ciągach html są wyświetlane w widoku sieci web.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

Specjalny ciąg **file:///android_asset/** odnosi się do folderu elementów zawartości systemu Android w swojej aplikacji, pokazane tutaj zawierający **style.css** pliku.

 ![Rozwiązanie AndroidHybrid](images/image3_240x167.png)

Akcja kompilacji dla wszystkich plików zawartości statycznej powinny być **AndroidAsset**.

 ![Akcja kompilacji projektu dla systemu android: AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>Wywoływanie C# z kodu HTML i Javascript

Po załadowaniu strony html w widoku sieci web traktuje łącza i formularze jak w przypadku załadowania strony z serwera. Oznacza to, że jeśli użytkownik kliknie łącze lub przesyła formularz widoku sieci web będzie podejmować próby przejdź do określonego celu.

Jeśli link do zewnętrznego serwera (na przykład google.com) widoku sieci web będzie podejmować próby załadować zewnętrznej witryny sieci Web (przy założeniu, że istnieje połączenie z Internetem).

```html
<a href="http://google.com/">Google</a>
```

Jeśli łącze jest względna widoku sieci web będzie podejmować próby załadowania tej zawartości z katalogu podstawowego. Oczywiście braku połączenia sieciowego jest wymagane, aby to zrobić, ponieważ zawartość jest przechowywana w aplikacji na urządzeniu.

```html
<a href="somepage.html">Local content</a>
```

Akcje formularza wykonaj tę samą regułę.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

Użytkownik nie chce hostować serwer sieci web na komputerze klienckim; jednak można użyć tych samych technik komunikacji serwera zatrudnionych we wzorcach elastyczne współczesnych do wywołania usługi za pośrednictwem HTTP GET i obsługują odpowiedzi asynchronicznie przez emitowanie kodu Javascript (lub wywołania języka Javascript są już hostowane w widoku sieci web). Dzięki temu można łatwo przekazywania danych z kodu HTML do kodu C# dla przetwarzania, a następnie Wyświetl wyniki z powrotem na stronie HTML.

Systemów iOS i Android mechanizm umożliwiający kod aplikacji, aby przechwytywać tych zdarzeń nawigacji, aby kod aplikacji może odpowiadać (jeśli jest to wymagane). Ta funkcja jest podstawą budowania hybrydowych aplikacji, ponieważ umożliwia on kodu natywnego, wchodzić w interakcje przy użyciu widoku sieci web.

#### <a name="ios"></a>iOS

Zdarzenie ShouldStartLoad w widoku sieci web w systemie iOS może zostać zastąpiona w celu kodu aplikacji do obsługi żądania nawigacji (np. Kliknij link). Wszystkie informacje podane parametry metody

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

a następnie przypisać program obsługi zdarzeń:

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

W systemie Android po prostu podklasy WebViewClient, a następnie zaimplementuj kod, aby odpowiadać na żądania nawigacji.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, IWebResourceRequest request) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

a następnie ustaw klienta w widoku sieci web:

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>Wywołania języka Javascript za pomocą języka C#

Oprócz informuje widok sieci web do załadowania strony HTML, kod w języku C# można również uruchomić w ramach aktualnie wyświetlaną stronę Javascript. Całe bloki kodu języka Javascript można tworzyć przy użyciu języka C# ciągów i wykonywane lub tworzyć wywołania metody w kodzie Javascript jest już dostępny na stronie za pośrednictwem `script` tagów.

#### <a name="android"></a>Android

Utwórz kod Javascript do wykonania, a następnie poprzedzić go "javascript:" i poinformuj widok internetowy, aby załadować ten ciąg:

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

Widoki sieci web dla systemu iOS zapewnia metody do wywołania języka Javascript:

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>Podsumowanie

W tej sekcji wprowadził funkcje kontrolki widoku sieci web w systemach Android i iOS, które umożliwiają nam tworzyć hybrydowe aplikacje za pomocą platformy Xamarin, w tym:

-  Możliwość ładowania HTML z ciągów generowane w kodzie
-  Możliwość przywołania pliki lokalne (CSS, Javascript, obrazy lub inne pliki HTML)
-  Możliwość Przechwytywanie żądań nawigacji w kodzie języka C#
-  Możliwość wywołania języka Javascript z kodu C#.


Następna sekcja wprowadza Razor, dzięki czemu można łatwo utworzyć HTML do użycia w aplikacjach hybrydowych.

## <a name="what-is-razor"></a>Co to jest Razor?

Razor jest aparat tworzenia szablonów, która została wprowadzona ze wzorca ASP.NET MVC pierwotnie można uruchomić na serwerze i generują kod HTML do przeglądarki sieci web użytkownika.

Aparatu tworzenia szablonów Razor rozszerza standardowe składni języka HTML w języku C#, dzięki czemu mogą express układ i łatwo zastosować arkuszy stylów CSS i Javascript. Szablonu można odwoływać się do klasy modelu, który może być dowolnego typu niestandardowego i którego właściwości są dostępne bezpośrednio z szablonu. Jednym z jego głównymi zaletami jest możliwość łatwego mieszać składni HTML i języka C#.

Szablony razor nie są ograniczone do użycia po stronie serwera, również mogły one zostać uwzględnione w aplikacjach platformy Xamarin. Za pomocą szablonów Razor, wraz z możliwością pracować programowo z widoki sieci web umożliwia aplikacjom zaawansowane hybrydowe dla wielu platform można tworzyć za pomocą platformy Xamarin.

### <a name="razor-template-basics"></a>Podstawy szablon razor

Ma pliki szablonów razor **.cshtml** rozszerzenie pliku. Mogą one dodane do projektu Xamarin z sekcji Tworzenie szablonów tekstowych w **nowy plik** okno dialogowe:

 ![Nowy plik — szablon Razor](images/image5_400x201.png)

Prosty szablon Razor ( **RazorView.cshtml**) znajdują się poniżej.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

Zwróć uwagę, następujące różnice względem zwykły plik HTML:

-  `@` Symbol ma specjalne znaczenie w szablony Razor — wskazuje, to następujące wyrażenie języka C# ma zostać obliczone.
- `@model` dyrektywa jest zawsze wyświetlany jako pierwszy wiersz pliku szablonu Razor.
-  `@model` Dyrektywy powinien zostać przeprowadzony według typu. W tym przykładzie prostego ciągu jest przekazywany do szablonu, ale może to być wszystkie klasy niestandardowej.
-  Gdy `@Model` o której mowa w szablonie, zapewnia odwołanie do obiektu przekazanych do szablonu, gdy zostanie wygenerowany (w tym przykładzie będzie ciąg).
-  IDE automatycznie wygeneruje klasy częściowej dla szablonów (pliki z **.cshtml** rozszerzenia). Ten kod jest widoczny, ale nie powinny być edytowane.
 ![RazorView.cshtml](images/image6_125x34.png) RazorView Period z nazwą pliku szablonu .cshtml nazwie klasy częściowej. Jest to nazwę, która jest używana do odwoływania się do szablonu w kodzie języka C#.
- `@using` instrukcji może być również uwzględniane w górnej części szablon Razor, aby uwzględnić dodatkowe przestrzenie nazw.


Następnie można wygenerować końcowych danych wyjściowych HTML przy użyciu następującego kodu C#. Należy pamiętać, że określamy modelu, który ma być ciągiem "Hello World", które zostaną uwzględnione w danych wyjściowych szablonu renderowany.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

Oto dane wyjściowe pokazane w widoku sieci web w systemie iOS Simulator i emulatora systemu Android:

 ![Witaj Świecie](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>Więcej składni Razor

W tej sekcji, którą spróbujemy wprowadzenie niektórych podstawowych składni Razor, aby pomóc Ci rozpocząć pracę przy użyciu go. Przykłady w tej sekcji wypełnić następujące klasy z danymi i wyświetl ją przy użyciu Razor:

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

Wszystkie przykłady, użyj następującego kodu inicjowania danych

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>Wyświetlanie właściwości modelu

Jeśli model jest klasy przy użyciu właściwości, łatwo występuje do nich w szablonie Razor jak pokazano w tym przykładzie szablonie:

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

Może to być renderowany na ciąg przy użyciu następującego kodu:

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

Końcowych danych wyjściowych jest tutaj wyświetlane w widoku sieci web w systemie iOS Simulator i emulatora systemu Android:

 ![Rupert](images/image8_516x160.png)

#### <a name="c-statements"></a>Instrukcje języka C#

Bardziej złożone C# mogą być zawarte w szablonie, takich jak aktualizacje właściwości modelu i obliczenia wieku, w tym przykładzie:

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

Można napisać złożonych wyrażeń języka C# jednowierszowego (np. formatowanie ery), przez umieszczenie kodu za pomocą `@()`.

Wiele instrukcji języka C# mogą być zapisywane przez otaczającego je za pomocą `@{}`.

#### <a name="if-else-statements"></a>If-else — instrukcje

Programowanie gałęzi można wyrazić za pomocą `@if` jak pokazano w tym przykładzie szablonu.

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

Tworzenie pętli konstrukcji, takich jak `foreach` mogą być również dodawane. `@` Na zmiennej pętli można użyć prefiksu ( `@food` w tym przypadku) do jej renderowania w formacie HTML.

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

Dane wyjściowe powyżej szablon jest wyświetlany, uruchomionej w systemie iOS Simulator i emulatora systemu Android:

 ![Małp X Rupert](images/image9_520x277.png)

Ta sekcja ma omówione podstawy używania szablony Razor do renderowania proste widokach tylko do odczytu. W następnej sekcji objaśniono sposób tworzenia bardziej szczegółowy aplikacji przy użyciu Razor, które akceptują dane wejściowe użytkownika i współpraca między języka Javascript w widoku HTML i języka C#.

## <a name="using-razor-templates-with-xamarin"></a>Za pomocą szablonów Razor za pomocą platformy Xamarin

W tej sekcji wyjaśniono, jak za pomocą kompilacji własnych aplikacji hybrydowych przy użyciu szablonów rozwiązań w programie Visual Studio dla komputerów Mac. Dostępne są trzy szablony **Plik > Nowy > rozwiązania...**  okna:

- **Android > aplikacji > Aplikacja WebView systemu Android**
- **iOS > aplikacji > Aplikacja WebView**
- **Projekt składnika ASP.NET MVC**



**Nowe rozwiązanie** okna wygląda następująco dla telefonu iPhone i Android projektów — opis rozwiązania po prawej stronie wyróżnia Obsługa aparatu tworzenia szablonów Razor.

 ![Tworzenie urządzenia iPhone i rozwiązania dla systemu Android](images/image13_1139x959.png)

Należy zauważyć, że można łatwo dodawać **.cshtml** szablon Razor *wszelkie* istniejący projekt Xamarin, nie jest konieczne używanie szablonów rozwiązań. projekty systemu iOS nie wymagają scenorysu do użycia zarówno: Razor po prostu programowo dodać kontrolkę UIWebView w dowolnym widoku i można renderować szablony Razor całego kodu C#.

Poniżej przedstawiono zawartość domyślnego szablonu rozwiązania dla telefonu iPhone i projekty systemu Android:

 ![urządzenia iPhone i Android szablonów](images/image10_428x310.png)

Szablony umożliwiają infrastruktury aplikacji gotowa do pracy, aby załadować szablon Razor za pomocą obiektu modelu danych, przetwarzać dane wejściowe użytkownika i przekazuje użytkownika przy użyciu języka Javascript.

Ważne elementy rozwiązania są:

-  Zawartość statyczna, takie jak **style.css** pliku.
-  Pliki szablonów cshtml razor, takich jak **RazorView.cshtml** .
-  Klasy, które odwołują się szablony Razor takich jak modeli **ExampleModel.cs** .
-  Klasy specyficzne dla platformy, która tworzy widok sieci web i renderuje szablonu, takie jak `MainActivity` w systemie Android i `iPhoneHybridViewController` w systemie iOS.


W poniższej sekcji omówiono, jak działają projekty.

### <a name="static-content"></a>Zawartość statyczna

Zawartość statyczna obejmuje arkuszy stylów CSS, obrazów, plików Javascript lub innej zawartości, które mogą być połączone z lub odwołuje się plik HTML, są wyświetlane w widoku sieci web.

Projekty szablonu włączenia arkusza stylów minimalny do pokazują, jak dodać zawartość statyczną w swojej aplikacji hybrydowych. Arkusz stylów CSS o której mowa w szablonie następująco:

```html
<link rel="stylesheet" href="style.css" />
```

Możesz dodać dowolną arkusza stylów i pliki Javascript, potrzebujesz, w tym struktur, takich jak JQuery.

### <a name="razor-cshtml-templates"></a>Cshtml razor szablonów

Szablon zawiera Razor **.cshtml** pliku, który ma wstępnie napisany kod, aby pomóc przekazywania danych między HTML/Javascript i C#. Umożliwi to kompilacja, zaawansowane hybrydowe aplikacje, które nie tylko wyświetlanie tylko do odczytu danych z modelu, ale również akceptuje dane wejściowe użytkownika w kodzie HTML i przekaż go z powrotem do kodu C# w celu ich przetwarzania lub magazynowania.

#### <a name="rendering-the-template"></a>Renderowanie szablonu

Wywoływanie `GenerateString` na podstawie szablonu powoduje wyświetlenie kodu HTML gotowy do wyświetlenia w widoku sieci web. Jeśli szablon korzysta z modelu, a następnie powinien być dostarczony przed renderowaniem. Na tym diagramie przedstawiono sposób renderowania działania — nie rozwiązywania zasobów statycznych w widoku sieci web w czasie wykonywania, aby znaleźć określone pliki przy użyciu podanej podstawowego katalogu.

 ![Schemat blokowy razor](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>Wywoływanie kodu w języku C# z szablonu

Komunikacja z widoku sieci web renderowanych wywołań zwrotnych do języka C# odbywa się przez ustawienie adresu URL dla widoku internetowego, a następnie przechwytuje żądania w języku C# do obsługi natywnych żądania bez ponownego ładowania widoku sieci web.

Przykładem są widoczne w sposób obsługi przez RazorView przycisku. Ten przycisk ma poniższy kod HTML:

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

`InvokeCSharpWithFormValues` Funkcji Javascript odczytuje wszystkie wartości z formularza HTML i zestawy `location.href` widoku sieci web:

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

To próbuje przejdź do widoku sieci web do adresu URL ze schematem niestandardowym (np.) `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

Podczas widoku internetowego natywnych przetwarzania tego żądania nawigacji, mamy możliwość jego przechwycenia. W systemie iOS odbywa się dzięki obsłudze UIWebView HandleShouldStartLoad zdarzeń. W systemie Android możemy po prostu podklasy WebViewClient używanych w formularzu, a następnie zastąpić ShouldOverrideUrlLoading.

Funkcje wewnętrzne tych dwóch interceptory nawigacji jest zasadniczo taki sam.

Sprawdź najpierw, adres URL, do którego widoku strony sieci web próbuje załadować, a jeśli nie zaczyna się od schematem niestandardowym (`hybrid:`), Zezwalaj nawigacji występują w zwykły sposób.

Dla schematu niestandardowego adresu URL, wszystko w adresie URL między schemat i "?" jest to nazwa metody do obsłużenia (w tym przypadku "UpdateLabel"). Wszystko, co w ciągu zapytania jest traktowany jako parametry w celu wywołania metody:

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` w tym przykładzie jest skraca manipulowanie ciągami w parametrze pola tekstowego (as "C# jest wyświetlany komunikat" do ciągu), a następnie wywołuje powrót do widoku sieci web.

Po obsłudze adres URL, metoda przerywa nawigacji, aby widok sieci web nie jest podejmowana próba zakończenia, przechodząc do niestandardowego adresu URL.

#### <a name="manipulating-the-template-from-c"></a>Manipulowanie szablonu za pomocą języka C#

Komunikacja renderowany widok sieci web HTML za pomocą języka C# odbywa się przez wywołanie metody Javascript w widoku sieci web. W systemach iOS, odbywa się przez wywołanie metody `EvaluateJavascript` na UIWebView:

```csharp
webView.EvaluateJavascript (js);
```

W systemie Android Javascript może być wywoływany w widoku sieci web przez ładowanie Javascript jako adres URL przy użyciu `"javascript:"` schemat adresu URL:

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>Tworzenie aplikacji prawdziwie hybrydowe

Te szablony nie należy używać natywnych kontrolek na każdej platformie — cały ekran jest wypełniony przy użyciu widoku jednej sieci web.

HTML może być bardzo przydatne na potrzeby tworzenia prototypów i wyświetlanie rodzaje elementów sieci web najlepiej jest na przykład tekstu sformatowanego i układu dynamicznego. Na przykład, jednak nie wszystkie zadania są odpowiednie do kodu HTML i Javascript — przewijanie za długo listy danych, wykonuje lepiej przy użyciu natywnych kontrolek interfejsu użytkownika, takich jak (UITableView w systemie iOS) lub ListView w systemie Android.

Widoki sieci web w szablonie łatwo można rozszerzyć za pomocą kontrolek specyficzne dla platformy — po prostu edycji **MainStoryboard.storyboard** w narzędzia iOS designer lub **Resources/layout/Main.axml** w systemie Android.

### <a name="razortodo-sample"></a>Przykładowe RazorTodo

[RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) repozytorium zawiera dwa oddzielne rozwiązania do pokazania różnic między aplikacją całkowicie oparte na HTML i aplikacji, która łączy HTML z natywne kontrolki:

-  **RazorTodo** -aplikacji opartej na całkowicie HTML, przy użyciu szablonów Razor.
-  **RazorNativeTodo** — są używane kontrolki widoku listy natywne dla systemów iOS i Android, ale wyświetla ekran edycji z językami HTML oraz Razor.


Uruchom te aplikacje platformy Xamarin dla systemów iOS i Android przy użyciu biblioteki klas przenośnych (PCLs), aby udostępnić wspólny kod, takich jak klasy bazy danych i modelu. Razor **.cshtml** szablony również mogą być zawarte w aplikacji PCL, dzięki czemu są łatwo współdzielone między platformami.

Obie aplikacje przykładowe dołączyć API zamiany tekstu na mowę z platformy natywnej wykazania, że aplikacje hybrydowe za pomocą platformy Xamarin nadal mieć dostęp do podstawowych funkcji z widoków opartych na szablonie HTML w silniku Razor i udostępnianie w serwisie Twitter.

**RazorTodo** aplikacja używa szablony HTML Razor w widokach listy i edycji. Oznacza to, że możemy ją tworzyć aplikację prawie całkowicie w udostępnionej biblioteki klas przenośnych (łącznie z bazą danych i **.cshtml** szablony Razor). Poniższe zrzuty ekranu Pokaż aplikacje iOS i Android.

 ![RazorTodo](images/Both_700x290.png)

**RazorNativeTodo** aplikacja używa szablonu HTML w silniku Razor dla widoku edycji, ale implementuje natywnych przewijanej listy na każdej platformie. To zapewnia szereg korzyści, w tym:

-  Wydajność — natywne kontrolki przewijania używają wirtualizacji, aby zapewnić, szybkie i bezproblemowe przewijanie nawet w przypadku list bardzo dużo danych.
-  Natywne środowisko — elementy interfejsu użytkownika specyficznymi dla platformy łatwo są włączone, takie jak obsługa przewijania fast indeksu, w systemach iOS i Android.


Zaletą tworzenia aplikacji hybrydowych za pomocą platformy Xamarin jest można uruchomić interfejs użytkownika całkowicie oparte na HTML (np. pierwszy przykład) i następnie dodać funkcje specyficzne dla platformy, gdy jest to wymagane (jako drugi przedstawia przykładowy). Ekrany natywnych listy i HTML w silniku Razor edycja ekranów w obu systemach iOS i Android zostały wymienione poniżej.

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>Podsumowanie

Ten artykuł ma opisano funkcje dostępne kontrolki widoku sieci web w systemach iOS i Android, które ułatwiają tworzenie aplikacji hybrydowych.

Omówiono następnie aparatu tworzenia szablonów Razor i składnię, które można łatwo generują kod HTML w aplikacji platformy Xamarin przy użyciu. **cshtml** pliki szablonów Razor. Także opisane programu Visual Studio dla komputerów Mac, szablony rozwiązań, które pozwalają na szybkie wprowadzenie do tworzenia aplikacji hybrydowych za pomocą platformy Xamarin.

Na koniec wprowadzono próbki RazorTodo, które pokazują, jak połączyć widoki sieci web za pomocą natywne interfejsy użytkownika i interfejsów API.

### <a name="related-links"></a>Linki pokrewne

- [Przykładowe RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3 — aparat widoku Razor (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor (Microsoft)](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
