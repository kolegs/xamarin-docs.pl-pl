---
title: Widok sieci Web
description: Przedstawia lokalnej lub zawartość sieci web i dokumenty.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: a96c57b66e5debbbb7318c22e33a21eb9b998395
ms.sourcegitcommit: 271d3f7ea4abfcf87734d2c747a68cb8114d743c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2018
---
# <a name="webview"></a>Widok sieci Web

[Widok sieci Web](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) jest widok, aby wyświetlić sieci web, jak i HTML zawartości w aplikacji. W odróżnieniu od `OpenUri`, który przyjmuje w przeglądarce sieci web na urządzeniu, użytkownik `WebView` Wyświetla zawartość HTML w aplikacji.

Ten przewodnik składa się z następujących sekcji:

- **[Zawartość](#Content)**  &ndash; widoku sieci Web obsługuje różne źródła zawartości, w tym osadzonego kodu HTML, stron sieci web i ciągi HTML.
- **[Nawigacji](#Navigation)**  &ndash; widoku sieci Web obsługuje Nawigacja do konkretnej strony i cofnięcie.
- **[Zdarzenia](#Events)**  &ndash; nasłuchiwania i odpowiadania na akcje wykonywane przez użytkownika w widoku sieci Web.
- **[Wydajność](#Performance)**  &ndash; Dowiedz się więcej na temat charakterystyki wydajności widoku sieci Web na każdej z platform.
- **[Uprawnienia](#Permissions)**  &ndash; Dowiedz się, jak ustawić uprawnienia, aby działały widoku sieci Web w aplikacji.
- **[Układ](#Layout)**  &ndash; niektórych bardzo szczególne wymagania dla jego układ ma widoku sieci Web. Dowiedz się, jak i upewnij się, że widok sieci Web wyświetla prawidłowo:

![](webview-images/in-app-browser.png "W przeglądarce aplikacji")

## <a name="content"></a>Zawartość

Widok sieci Web są obsługiwane następujące typy zawartości:

- HTML i CSS witryn sieci Web &ndash; widoku sieci Web zawiera pełną obsługę witryn sieci Web napisane przy użyciu kodu HTML i CSS, w tym obsługi języka JavaScript.
- Dokumenty &ndash; ponieważ widoku sieci Web jest wdrażane za pomocą natywnego składników na każdej platformie, widoku sieci Web jest w stanie przedstawiający dokumenty, które są widoczne na każdej z platform. Oznacza to, że pliki PDF pracy z systemem iOS i Android, ale nie Windows Phone.
- Ciągi HTML &ndash; WebView mogą być prezentowane ciągi kodu HTML z pamięci.
- Lokalne pliki &ndash; widoku sieci Web może ona powodować dowolne z powyższych typów zawartości osadzone w aplikacji.

> [!NOTE]
> `WebView` w systemach Windows i Windows Phone nie obsługuje Silverlight, Flash lub żadnych formantów ActiveX, nawet jeśli są one obsługiwane przez program Internet Explorer na tej platformie.

### <a name="websites"></a>Witryny internetowe

Aby wyświetlić witrynę sieci Web z Internetu, należy ustawić `WebView`w [ `Source` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebViewSource/) właściwości ciągu adresu URL:

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> Adresy URL musi być w pełni sformułowane z protokołem określony (tj. musi mieć "http://" lub "https://" dołączany na jego początku).

#### <a name="ios-and-ats"></a>iOS i ATS

Od wersji 9 dla systemu iOS zezwala tylko aplikację do komunikacji z serwerami, które implementują najlepszych rozwiązań zabezpieczeń domyślnie. Wartości musi być ustawiona w `Info.plist` aby umożliwić komunikację z serwerami niezabezpieczonych.

> [!NOTE]
> Jeśli aplikacja wymaga połączenia niezabezpieczonego witryny sieci Web, należy zawsze wprowadź domenę jako wyjątków za pomocą `NSExceptionDomains` zamiast wyłączania ATS całkowicie przy użyciu `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` należy używać tylko extreme sytuacji awaryjnych.

Poniżej pokazano, jak włączyć określonej domeny (w tym wielkość xamarin.com) do obejścia ATS wymagania:

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

Jest najlepszym rozwiązaniem, aby włączyć tylko niektóre domeny pominąć ATS, co umożliwia używanie podczas korzystających z dodatkowych zabezpieczeń dostępnych w niezaufanych domenach zaufanych witryn. Poniżej przedstawiono mniej bezpieczne metody wyłączenie ATS aplikacji:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

Zobacz [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md) uzyskać więcej informacji o tej nowej funkcji w systemie iOS 9.

### <a name="html-strings"></a>Ciągi HTML

Jeśli mają być przedstawiane w ciągu dynamicznie zdefiniowane w kodzie HTML, musisz utworzyć wystąpienia [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "Ciąg HTML wyświetlania widoku sieci Web")

W powyższym kodzie `@` służy do oznaczania HTML jako ciąg literału, co oznacza wszystkie znaki ucieczki zwykle są ignorowane.

### <a name="local-html-content"></a>Zawartość lokalna HTML

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

Należy pamiętać, że czcionek określone w powyższej CSS będzie musiał można dostosować dla każdej platformy jako nie każda platforma ma te same czcionki.

Do wyświetlania lokalnej zawartości przy użyciu `WebView`, należy otworzyć plik HTML, jak każdy inny, a następnie Załaduj zawartość jako ciąg do `Html` właściwość `HtmlWebViewSource`. Aby uzyskać więcej informacji na otwieranie plików, zobacz [Praca z plikami](~/xamarin-forms/app-fundamentals/files.md).

Poniższe zrzuty ekranu pokazują wynik wyświetlanie zawartości lokalnej na każdej platformie:

![](webview-images/local-content.png "Widok sieci Web wyświetlanie zawartości lokalnej")

Mimo że załadowano pierwszej strony, `WebView` nie ma informacji o skąd pochodzą HTML. To jest problem podczas pracy nad stron, które odwołują się do zasobów lokalnych. Po którym może się tak dziać przykładami łącze lokalnych stron, które do każdej drugiej strony, strony sprawia, że korzystanie z oddzielnych plików JavaScript lub strona zawiera linki do arkusza stylów CSS.  

Aby rozwiązać ten problem, należy przekazać do `WebView` gdzie można znaleźć plików w systemie plików. To zrobić przez ustawienie `BaseUrl` właściwość `HtmlWebViewSource` używane przez `WebView`.

Różni się system plików na poszczególnych systemów operacyjnych, należy określić ten adres URL na każdej z platform. Przedstawia platformy Xamarin.Forms `DependencyService` dla rozpoznawania zależności w czasie wykonywania na każdej z platform.

Aby użyć `DependencyService`, najpierw zdefiniować interfejs, który można stosować na każdej platformie:

```csharp
public interface IBaseUrl { string Get(); }
```

Należy pamiętać, że dopóki interfejs jest wykonywane na każdej platformie, aplikacja nie zostanie uruchomiony. Upewnij się, należy ustawić w projektu wspólnego `BaseUrl` przy użyciu `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Następnie należy dostarczyć implementacje interfejsu dla każdej platformy.

#### <a name="ios"></a>iOS

W systemach iOS, zawartość sieci web powinien znajdować się w katalogu głównym projektu lub **zasobów** katalogu z akcją kompilacji *BundleResource*, jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/ios-vs.png "Lokalne pliki w systemie iOS")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](webview-images/ios-xs.png "Lokalne pliki w systemie iOS")

-----

`BaseUrl` Powinien być ustawiony na ścieżkę głównym pakietu:

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

W systemie Android, należy umieścić w folderze Zasoby z akcją kompilacji kodu HTML, CSS i obrazy *AndroidAsset* jak pokazano poniżej:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/android-vs.png "Lokalne pliki w systemie Android")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](webview-images/android-xs.png "Lokalne pliki w systemie Android")

-----

W systemie Android `BaseUrl` powinien być ustawiony na `"file:///android_asset/"`:

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

W systemie Android, pliki w **zasoby** folderu można także uzyskać dostęp za pośrednictwem bieżącego kontekstu Android jest udostępniana przez `MainActivity.Instance` właściwości:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="windows-phone"></a>Windows Phone

Na Windows Phone, umieść HTML, CSS i obrazów w katalogu głównym projektu z akcją kompilacji ustawioną *zawartości* jak pokazano poniżej:

![](webview-images/windows-vs.png "Lokalne pliki na Windows Phone")

Na Windows Phone `BaseUrl` powinien być ustawiony na `""`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Windows))]
namespace WorkingWithWebview.Windows {
  public class BaseUrl_Windows : IBaseUrl {
    public string Get() {
      return "";
    }
  }
}
```

#### <a name="windows-runtime-and-universal-windows-platform"></a>Środowisko wykonawcze systemu Windows i platforma uniwersalna systemu Windows

W przypadku projektów środowiska wykonawczego systemu Windows i Windows platformy Uniwersalnej, umieść HTML, CSS i obrazów w katalogu głównym projektu z akcją kompilacji ustawioną *zawartości*.

`BaseUrl` Powinien być ustawiony na `"ms-appx-web:///"`:

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

## <a name="navigation"></a>Nawigacji

Widok sieci Web obsługuje nawigację kilka metod i właściwości, które są dostępne:

- **GoForward()** &ndash; Jeśli `CanGoForward` ma wartość true, wywołania `GoForward` przechodzi dalej do następnej strony odwiedzone.
- **GoBack()** &ndash; Jeśli `CanGoBack` ma wartość true, wywołania `GoBack` spowoduje przejście do ostatniej strony odwiedzone.
- **CanGoBack** &ndash; `true` Jeśli istnieją strony, aby wrócić do `false` Jeśli przeglądarka jest pod adresem URL początkowy.
- **CanGoForward** &ndash; `true` Jeśli użytkownik ma nawigacji Wstecz i można przejście do strony, która została już odwiedzone.

W ramach stron `WebView` nie obsługuje wielodotyku gestów. Ważne jest, aby upewnić się, tej zawartości jest zoptymalizowany pod kątem mobile i pojawia się bez konieczności powiększania.

Często w przypadku aplikacji do wyświetlenia łącza w `WebView`, zamiast przeglądarki na urządzeniu. W takiej sytuacji jest Zezwalaj na normalne nawigacji, ale podczas trafień użytkownika kopii, gdy są one początkowy łącze, aplikacji należy powrócić do widoku normalnego aplikacji.

Użyj właściwości i metod wbudowanych nawigacji, aby włączyć ten scenariusz.

Rozpocznij od utworzenia strony widoku przeglądarki:

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

W naszym związane z kodem:

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

To już wszystko!

![](webview-images/in-app-browser.png "Przyciski nawigacji w widoku sieci Web")

## <a name="events"></a>Zdarzenia

Widok sieci Web wywołuje dwa zdarzenia w celu reagowania na zmiany w stanie:

- **Nawigowanie po** &ndash; Zdarzenie wywoływane, gdy widoku sieci Web rozpocznie się podczas ładowania nowej strony.
- **Przejście** &ndash; Zdarzenie wywoływane, gdy strona zostanie załadowany i nawigacji została zatrzymana.

Jeśli przewidujesz przy użyciu zająć dużo czasu ładowania strony sieci Web, rozważ użycie tych zdarzeń do zaimplementowania wskaźnika stanu. Na przykład:

Nasze XAML:

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
        isVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```
Nasze uchwyty dwa zdarzeń:

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

Powoduje to następujące dane wyjściowe (ładowania):

![](webview-images/loading-start.png "Przykład zdarzeń nawigacji widoku sieci Web")

Zakończono ładowanie:

![](webview-images/loading-end.png "Przejście przykład zdarzeń, widok sieci Web")

## <a name="performance"></a>Wydajność

Najnowsze funkcje jak już wspomniano wszystkich popularnych przeglądarkach przyjmuje technologii, takich jak przyspieszane renderowania i kompilacja kodu JavaScript. Niestety, ze względu na ograniczenia zabezpieczeń większość tych korzyści nie były dostępne w iOS-equaivalent z `WebView`, `UIWebView`. Platformy Xamarin.Forms `WebView` używa `UIWebView`. Jeśli jest to problem, musisz zapisać niestandardowego modułu renderowania, które używa `WKWebView`, który obsługuje szybsze przeglądania. Należy pamiętać, że `WKWebView` jest obsługiwana tylko w systemie iOS 8 i nowszych.

Widok sieci Web w systemie Android, domyślnie jest około tak szybko, jak przeglądarki wbudowanej.

[Widoku sieci Web platformy uniwersalnej systemu Windows](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) używa aparatu renderowania Microsoft Edge. Komputery stacjonarne i tablet urządzeń powinna zostać wyświetlona wydajności tej samej jako przy użyciu samej przeglądarki Edge.

`WebBrowser` Formantu Windows Phone 8 i Windows Phone 8.1 ma nie obsługują HTML5 najnowsze funkcje i często może zawierać pogorszenie wydajności. Należy pamiętać o jak lokacje będą wyświetlane w Windows Phone `WebView`. Nie jest wystarczająca do testowania w programie Internet Explorer.

## <a name="permissions"></a>Uprawnienia

Aby `WebView` działała, należy się upewnić, że uprawnienia zostały ustawione dla każdej platformy. Należy pamiętać, że na niektórych platformach `WebView` będzie działać w trybie debugowania, a nie wbudowanych w wydaniu. To już niektóre uprawnienia, jak uzyskać dostęp do Internetu w systemie Android są ustawione domyślnie w programie Visual Studio dla komputerów Mac w trybie debugowania.

- **Windows Phone 8.0** &ndash; wymaga `ID_CAP_WEBBROWSERCOMPONENT` formantu i `ID_CAP_NETWORKING` dostęp do Internetu.
- **Windows Phone 8.1 i platformy uniwersalnej systemu Windows** &ndash; wymaga możliwości Internet (klient i serwer), podczas wyświetlania zawartości w sieci.
- **Android** &ndash; wymaga `INTERNET` tylko wtedy, gdy wyświetlanie zawartości z sieci. Zawartość lokalna wymaga żadne specjalne uprawnienia.
- **iOS** &ndash; wymaga żadne specjalne uprawnienia.

## <a name="layout"></a>Układ

W przeciwieństwie do większości innych widoków platformy Xamarin.Forms `WebView` wymaga, aby `HeightRequest` i `WidthRequest` są określone, gdy jest zawarty w StackLayout lub RelativeLayout. Aby określić te właściwości, w przeciwnym przypadku `WebView` nie będzie zwracał.

W poniższych przykładach pokazano układy, które powodują powstanie pracę, renderowanie `WebView`s:

StackLayout z WidthRequest & HeightRequest:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

RelativeLayout z WidthRequest & HeightRequest:

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

Siatka *bez* WidthRequest & HeightRequest. Siatka jest jednym z kilku układy, które nie wymagają określenia żądanej wysokości i szerokości.:

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


## <a name="related-links"></a>Linki pokrewne

- [Praca z widoku sieci Web (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [Widok sieci Web (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
