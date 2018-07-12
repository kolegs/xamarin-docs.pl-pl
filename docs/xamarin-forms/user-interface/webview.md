---
title: Aplikacja WebView zestawu narzędzi Xamarin.Forms
description: W tym artykule opisano sposób użycia klasy WebView zestawu narzędzi Xamarin.Forms do przedstawienia lokalnych lub zawartość sieci web w sieci i dokumentów dla użytkowników.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 55267dfb1439d17f09126f65973ce9e6a0247d80
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986060"
---
# <a name="xamarinforms-webview"></a>Aplikacja WebView zestawu narzędzi Xamarin.Forms

[`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) jest to widok do wyświetlania w aplikacji sieci web i zawartość HTML. W odróżnieniu od `OpenUri`, który powoduje otwarcie przeglądarki sieci web na urządzeniu, `WebView` Wyświetla zawartość HTML w aplikacji.

![](webview-images/in-app-browser.png "W przeglądarce aplikacji")

## <a name="content"></a>Zawartość

`WebView` obsługuje następujące typy zawartości:

- Języki HTML i CSS witryn sieci Web &ndash; WebView ma pełną obsługę witryn sieci Web napisane przy użyciu kodu HTML i CSS, łącznie z obsługą języka JavaScript.
- Dokumenty &ndash; ponieważ WebView jest implementowana przy użyciu składnikami macierzystymi na każdej platformie, widok sieci Web jest w stanie wyświetlanie dokumentów, które są widoczne na każdej platformie. Oznacza to, że pliki PDF działać w systemach iOS i Android.
- Ciągi HTML &ndash; WebView można wyświetlić w ciągach HTML z pamięci.
- Pliki lokalne &ndash; WebView może powodować dowolne z powyższych typów zawartości osadzone w aplikacji.

> [!NOTE]
> `WebView` na Windows nie obsługuje programu Silverlight, Flash lub dowolnej kontrolki ActiveX, nawet jeśli są one obsługiwane przez program Internet Explorer na tej platformie.

### <a name="websites"></a>Witryny internetowe

Aby wyświetlić witrynę sieci Web z Internetu, należy ustawić `WebView`firmy [ `Source` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebViewSource/) właściwość ciągu adresu URL:

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> Adresy URL musi być w pełni sformułowane przy użyciu protokołu określonego (czyli musi mieć "http://" lub "https://" dołączony do niego).

#### <a name="ios-and-ats"></a>dla systemów iOS i ATS

Od wersji 9 dla systemu iOS zezwala tylko aplikację do komunikacji z serwerami, które implementują najlepszym rozwiązaniem z zakresu zabezpieczeń domyślnie. Wartości musi być ustawiona w `Info.plist` aby umożliwić komunikację z serwerami niebezpieczne.

> [!NOTE]
> Jeśli aplikacja wymaga połączenia z witryny sieci Web niezabezpieczone, zawsze należy wprowadzić domeny jako wyjątków za pomocą `NSExceptionDomains` zamiast wyłączenie ATS całkowicie za pomocą `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` można stosować tylko w sytuacjach awaryjnych, skrajnych.

Poniżej przedstawiono sposób włączania określonej domeny (w tym przypadków strony xamarin.com) w celu obejścia wymagania ATS:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>xamarin.com</key>
            <dict>
                <key>NSIncludesSubdomains</key>
                <true/>
                <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSTemporaryExceptionMinimumTLSVersion</key>
                <string>TLSv1.1</string>
            </dict>
        </dict>
    </dict>
```

Jest najlepszym rozwiązaniem jest włączenie tylko niektóre domeny w celu obejścia ATS, co pozwala na wykorzystanie zaufanych witryn, jednocześnie korzystając z dodatkowych zabezpieczeń w domenach niezaufanych. Poniżej przedstawiono metody mniej bezpieczna opcja wyłączenia ATS dla aplikacji:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

Zobacz [App Transport Security](~/ios/app-fundamentals/ats.md) Aby uzyskać więcej informacji na temat tej nowej funkcji w systemie iOS 9.

### <a name="html-strings"></a>Ciągi HTML

Jeśli chcesz przedstawić ciąg HTML definiowany dynamicznie w kodzie, konieczne będzie utworzenie wystąpienia [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "Ciąg HTML wyświetlanie widoku sieci Web")

W powyższym kodzie `@` służy do oznaczania HTML jako ciąg literału, co oznacza wszystkie znaki ucieczki zwykle są ignorowane.

### <a name="local-html-content"></a>Lokalną zawartość HTML

Widok sieci Web można wyświetlać zawartość z kodu HTML, CSS i Javascript osadzone w aplikacji. Na przykład:

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamrin.Forms</h1>
    <p>This is an iOS web page.</p>
    <img src="XamarinLogo.png" />
  </body>
</html>
```

CSS:

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

Należy pamiętać, że czcionek określone w powyższym kodzie CSS musi być dostosowane do poszczególnych platform nie każda platforma ma te same czcionki.

Do wyświetlania lokalnych zawartości przy użyciu `WebView`, musisz otworzyć ten plik HTML, takich jak każdy inny, a następnie Załaduj zawartość jako ciąg znaków do `Html` właściwość `HtmlWebViewSource`. Aby uzyskać więcej informacji na temat otwierania plików, zobacz [Praca z plikami](~/xamarin-forms/app-fundamentals/files.md).

Poniższych zrzutach ekranu przedstawiono wynik wyświetlanie zawartości lokalnej na każdej z platform:

![](webview-images/local-content.png "Aplikacja WebView wyświetlanie zawartości lokalnej")

Mimo, że pierwsza strona został załadowany, `WebView` nie ma informacji o pochodzenie pliku HTML. To jest problem podczas pracy ze stronami odwołujące się do zasobów lokalnych. Gdy, może się zdarzyć, przykładami łącze lokalnych stron, które do każdej drugiej strony, strony sprawia, że korzystanie z oddzielnych plików JavaScript lub strony, który stanowi łącze do arkusza stylów CSS.  

Aby rozwiązać ten problem, należy przekazać `WebView` gdzie można znaleźć plików w systemie plików. To zrobić, ustawiając `BaseUrl` właściwość `HtmlWebViewSource` posługują się `WebView`.

Ponieważ system plików na wszystkich systemów operacyjnych jest inna, należy określić tego adresu URL na każdej platformie. Zestaw narzędzi Xamarin.Forms uwidacznia `DependencyService` rozpoznawania zależności w czasie wykonywania na każdej platformie.

Aby użyć `DependencyService`, należy najpierw zdefiniować interfejs, który można zaimplementować dla każdej platformy:

```csharp
public interface IBaseUrl { string Get(); }
```

Należy pamiętać, że do momentu interfejs jest wykonywane na każdej platformie, aplikacja nie uruchomi. W typowych projektu, upewnij się, należy ustawić `BaseUrl` przy użyciu `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Następnie należy podać implementacje interfejsu dla każdej platformy.

#### <a name="ios"></a>iOS

W systemach iOS, zawartości sieci web powinien znajdować się w katalogu głównym projektu lub **zasobów** katalogu przy użyciu akcji kompilacji *BundleResource*, jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/ios-vs.png "Pliki lokalne w systemie iOS")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](webview-images/ios-xs.png "Pliki lokalne w systemie iOS")

-----

`BaseUrl` Powinna być ustawiona na ścieżce głównym pakietu:

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS{
  public class BaseUrl_iOS : IBaseUrl {
    public string Get() {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

W systemie Android, należy umieścić w folderze zasobów za pomocą akcji kompilacji HTML, CSS i obrazów *AndroidAsset* jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/android-vs.png "Pliki lokalne w systemie Android")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](webview-images/android-xs.png "Pliki lokalne w systemie Android")

-----

W systemie Android `BaseUrl` powinna być równa `"file:///android_asset/"`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android {
  public class BaseUrl_Android : IBaseUrl {
    public string Get() {
      return "file:///android_asset/";
    }
  }
}
```

W systemie Android pliki **zasoby** folderu można także uzyskać dostęp za pośrednictwem bieżącego kontekstu dla systemu Android, który jest uwidaczniany przez `MainActivity.Instance` właściwości:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Dla projektów uniwersalnych platformy Windows (UWP), umieść HTML, CSS i obrazów w folderze głównym projektu za pomocą akcji kompilacji, ustaw *zawartości*.

`BaseUrl` Powinna być równa `"ms-appx-web:///"`:

```csharp
[assembly: Dependency(typeof(BaseUrl))]
namespace WorkingWithWebview.UWP
{
    public class BaseUrl : IBaseUrl
    {
        public string Get()
        {
            return "ms-appx-web:///";
        }
    }
}
```

## <a name="navigation"></a>Nawigacja

Widok sieci Web obsługuje nawigację kilka metod i właściwości udostępniane przez niego:

- **GoForward()** &ndash; Jeśli `CanGoForward` ma wartość true, wywołanie `GoForward` przechodzi do przodu, następną odwiedzoną stronę.
- **GoBack()** &ndash; Jeśli `CanGoBack` ma wartość true, wywołanie `GoBack` spowoduje przejście do ostatniej odwiedzane strony.
- **CanGoBack** &ndash; `true` w przypadku stron, które mają przejdź z powrotem do `false` Jeśli przeglądarka jest adresem URL wyjścia.
- **CanGoForward** &ndash; `true` Jeśli użytkownik ma przejście wstecz i przejść do strony, który już został odwiedzony.

Na stronach `WebView` nie obsługuje gestów dotykowych wielu. Ważne jest, aby upewnić się, że tej zawartości jest zoptymalizowane pod kątem mobile i pojawia się bez potrzeby powiększania.

To częsty problem w aplikacji, aby wyświetlić łącza w obrębie `WebView`, zamiast w przeglądarce na urządzeniu. W takiej sytuacji jest przydatne zezwolenie na normalnej nawigacji, ale podczas trafień użytkownika kopii, gdy są one już Link, aplikacja powinna powrócić do widoku normalna aplikacja.

Aby włączyć ten scenariusz, należy użyć właściwości i metod wbudowanych nawigacji.

Należy rozpocząć od tworzenia strony widoku przeglądarki:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.InAppDemo"
Title="In App Browser">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal" Padding="10,10">
                <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="backClicked" />
                <Button Text="Forward" HorizontalOptions="End" Clicked="forwardClicked" />
            </StackLayout>
            <WebView x:Name="Browser" WidthRequest="1000" HeightRequest="1000" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

W naszym związanym z kodem:

```csharp
public partial class InAppDemo : ContentPage
{
  //sets the URL for the browser in the page at creation
    public InAppDemo (string URL)
    {
        InitializeComponent ();
        Browser.Source = URL;
    }


    private void backClicked(object sender, EventArgs e)
    {
    // Check to see if there is anywhere to go back to
        if (Browser.CanGoBack) {
            Browser.GoBack ();
        } else { // If not, leave the view
            Navigation.PopAsync ();
        }
    }

    private void forwardClicked(object sender, EventArgs e)
    {
        if (Browser.CanGoForward) {
            Browser.GoForward ();
        }
    }
}
```

To wszystko!

![](webview-images/in-app-browser.png "Przyciski nawigacji w widoku sieci Web")

## <a name="events"></a>Zdarzenia

Widok sieci Web zgłasza dwa zdarzenia ułatwiające reagowanie na zmiany w stanie:

- **Nawigacja** &ndash; Zdarzenie wywoływane, gdy element WebView, który rozpoczyna się podczas ładowania nowej strony.
- **Przejście** &ndash; Zdarzenie wywoływane, gdy strona została załadowana i nawigacji została zatrzymana.

Jeśli przewidujesz używanie zająć dużo czasu do załadowania strony sieci Web, należy rozważyć użycie tych zdarzeń do zaimplementowania wskaźnik stanu. Na przykład XAML wygląda następująco:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.LoadingDemo" Title="Loading Demo">
  <ContentPage.Content>
    <StackLayout>
      <Label x:Name="LoadingLabel"
        Text="Loading..."
        HorizontalOptions="Center"
        IsVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

Programy obsługi zdarzeń dwa:

```csharp
void webOnNavigating (object sender, WebNavigatingEventArgs e)
{
    LoadingLabel.IsVisible = true;
}

void webOnEndNavigating (object sender, WebNavigatedEventArgs e)
{
    LoadingLabel.IsVisible = false;
}
```

To powoduje zwrócenie następujących danych wyjściowych (Trwa ładowanie):

![](webview-images/loading-start.png "Przykład Event przechodząc WebView")

Zakończono ładowanie:

![](webview-images/loading-end.png "Przykład nawigować Event WebView")

## <a name="performance"></a>Wydajność

Zauważyliśmy już najnowsze funkcje, wszystkich popularnych przeglądarek przyjmuje technologie, takie jak przyspieszane renderowania i kompilacja kodu JavaScript. Niestety, ze względu na ograniczenia zabezpieczeń, większość z tych udoskonaleń nie były dostępne w systemu iOS — equaivalent z `WebView`, `UIWebView`. Zestaw narzędzi Xamarin.Forms `WebView` używa `UIWebView`. Jeżeli jest to problem, należy napisać niestandardowego modułu renderowania, które używa `WKWebView`, który obsługuje szybsze przeglądanie. Należy pamiętać, że `WKWebView` jest obsługiwana tylko w systemie iOS 8 i nowszych.

Widok sieci Web w systemie Android, domyślnie jest około tak szybko, jak wbudowana przeglądarka.

[WebView platformy uniwersalnej systemu Windows](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) używa aparatu renderowania Microsoft Edge. Urządzeń stacjonarnych i tablecie powinna zostać wyświetlona ta sama wydajność jako przy użyciu samej przeglądarki Edge.

## <a name="permissions"></a>Uprawnienia

Aby `WebView` do pracy, to należy się upewnić, że uprawnienia zostały ustawione dla każdej platformy. Należy pamiętać, że na niektórych platformach `WebView` będzie działać w trybie debugowania, ale nie w przypadku, gdy stworzona z myślą o wersji. To, ponieważ niektóre uprawnienia, jak uzyskać dostęp do Internetu w systemie Android, są domyślnie przez program Visual Studio dla komputerów Mac w trybie debugowania.

- **Platformy uniwersalnej systemu Windows** &ndash; wymaga możliwości Internet (klient i serwer), podczas wyświetlania zawartości w sieci.
- **Android** &ndash; wymaga `INTERNET` tylko wtedy, gdy wyświetlanie zawartości z sieci. Zawartość lokalna wymaga żadne specjalne uprawnienia.
- **iOS** &ndash; wymaga żadne specjalne uprawnienia.

## <a name="layout"></a>Układ

W przeciwieństwie do większości innych widoków Xamarin.Forms `WebView` wymaga, aby `HeightRequest` i `WidthRequest` są określone, gdy jest zawarty w StackLayout lub RelativeLayout. Aby określić te właściwości, w przeciwnym przypadku `WebView` nie będą renderowane.

W poniższych przykładach pokazano układy rozwiązywana we współpracy, renderowanie `WebView`s:

StackLayout za pomocą WidthRequest & HeightRequest:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

RelativeLayout za pomocą WidthRequest & HeightRequest:

```xaml
<RelativeLayout>
    <Label Text="test"
        RelativeLayout.XConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=10}"
        RelativeLayout.YConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=20}" />
    <WebView Source="http://www.xamarin.com/"
        RelativeLayout.XConstraint="{ConstraintExpression Type=Constant,
                                     Constant=10}"
        RelativeLayout.YConstraint="{ConstraintExpression Type=Constant,
                                     Constant=50}"
        WidthRequest="1000" HeightRequest="1000" />
</RelativeLayout>
```

AbsoluteLayout *bez* WidthRequest & HeightRequest:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

Siatka *bez* WidthRequest & HeightRequest. Siatka jest jeden z kilku układów, które nie wymagają określania żądanej wysokości i szerokości.:

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Label Text="test" Grid.Row="0" />
    <WebView Source="http://www.xamarin.com/" Grid.Row="1" />
</Grid>
```

## <a name="invoking-javascript"></a>Wywoływanie kodu JavaScript

[ `WebView` ](xref:Xamarin.Forms.WebView) Obejmuje możliwość wywołania funkcji języka JavaScript za pomocą języka C# i zwraca wyniki do wywołującego kodu C#. Jest to realizowane przy [ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) metody, która jest wyświetlana w następującym przykładzie [WebView](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) próbki:

```csharp
var numberEntry = new Entry { Text = "5" };
var resultLabel = new Label();
var webView = new WebView();
...

int number = int.Parse(numberEntry.Text);
string result = await webView.EvaluateJavaScriptAsync($"factorial({number})");
resultLabel.Text = $"Factorial of {number} is {result}.";
```

[ `WebView.EvaluateJavaScriptAsync` ](xref:Xamarin.Forms.WebView.EvaluateJavaScriptAsync*) Metoda ocenia języka JavaScript, który jest określony jako argument i zwraca wynik w postaci `string`. W tym przykładzie `factorial` zostanie wywołana funkcja języka JavaScript, która zwraca silnię `number` w wyniku. Plik JavaScript, ta funkcja jest zdefiniowana w lokalnym pliku HTML, który [ `WebView` ](xref:Xamarin.Forms.WebView) ładuje i przedstawiono w poniższym przykładzie:

```html
<html>
<body>
<script type="text/javascript">
function factorial(num) {
        if (num === 0 || num === 1)
            return 1;
        for (var i = num - 1; i >= 1; i--) {
            num *= i;
        }
        return num;
}
</script>
</body>
</html>
```

## <a name="related-links"></a>Linki pokrewne

- [Praca z widoku sieci Web (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [Aplikacja WebView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
