---
title: Implementowanie kontrolki HybridWebView
description: W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla formantu niestandardowego HybridWebView, który pokazuje, jak poprawić formantów web specyficzne dla platformy umożliwiający kodu C# do wywołania języka JavaScript.
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 4715685fdf417ba2d08f9ae3d36d6e691fa701fa
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242462"
---
# <a name="implementing-a-hybridwebview"></a>Implementowanie kontrolki HybridWebView

_Kontrolek interfejsu użytkownika niestandardowego zestawu narzędzi Xamarin.Forms powinien pochodzić od klasy widoku, która służy do umieszczania, układy i formanty na ekranie. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla formantu niestandardowego HybridWebView, który pokazuje, jak poprawić formantów web specyficzne dla platformy umożliwiający kodu C# do wywołania języka JavaScript._

Każdego widoku interfejsu Xamarin.Forms ma towarzyszący modułu renderowania dla każdej platformy, która tworzy wystąpienie kontrolki natywne. Gdy [ `View` ](xref:Xamarin.Forms.View) jest renderowany przez aplikację platformy Xamarin.Forms w systemie iOS, `ViewRenderer` tworzenia wystąpienia klasy, która z kolei tworzy macierzystej `UIView` kontroli. Na platformie Android `ViewRenderer` tworzy wystąpienie klasy `View` kontroli. Na Universal Windows Platform (platformy UWP), `ViewRenderer` klasy tworzy macierzystej `FrameworkElement` kontroli. Aby uzyskać więcej informacji na temat renderowania i klasy natywnych kontrolek, mapowane kontrolek zestawu narzędzi Xamarin.Forms, zobacz [natywne kontrolki i klasy podstawowej renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono relację między [ `View` ](xref:Xamarin.Forms.View) odpowiedniego natywne kontrolki, które ją implementują i:

![](hybridwebview-images/view-classes.png "Relacja między widok klasy i jej implementującej klasy natywne")

Proces renderowania może służyć do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `View` ](xref:Xamarin.Forms.View) na każdej platformie. Proces ten jest w następujący sposób:

1. [Tworzenie](#Creating_the_HybridWebView) `HybridWebView` kontrolki niestandardowej.
1. [Używanie](#Consuming_the_HybridWebView) `HybridWebView`z zestawu narzędzi Xamarin.Forms.
1. [Tworzenie](#Creating_the_Custom_Renderer_on_each_Platform) niestandardowego modułu renderowania dla `HybridWebView` na każdej platformie.

Każdy element zostaną teraz dokładniej omówione w implementacji `HybridWebView` modułu renderowania, zwiększającym formantów web specyficzne dla platformy umożliwiający kodu C# do wywołania języka JavaScript. `HybridWebView` Wystąpienie zostanie użyte do wyświetlenia strony HTML, która prosi użytkownika o podanie swojego imienia. Następnie, gdy użytkownik kliknie przycisk HTML, funkcja języka JavaScript wywoła C# `Action` który wyświetla wyskakujące okienko zawierające nazwę użytkownika.

Aby uzyskać więcej informacji o procesie dla wywołania języka C# poziomu języka JavaScript, zobacz [wywoływania C# z kodu JavaScript](#Invoking_C_from_JavaScript). Aby uzyskać więcej informacji na temat strony HTML, zobacz [tworzenia strony sieci Web](#Creating_the_Web_Page).

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>Tworzenie HybridWebView

`HybridWebView` Kontrolki niestandardowej mogą być tworzone przez podklasy [ `View` ](xref:Xamarin.Forms.View) klasy, jak pokazano w poniższym przykładzie kodu:

```csharp
public class HybridWebView : View
{
  Action<string> action;
  public static readonly BindableProperty UriProperty = BindableProperty.Create (
    propertyName: "Uri",
    returnType: typeof(string),
    declaringType: typeof(HybridWebView),
    defaultValue: default(string));

  public string Uri {
    get { return (string)GetValue (UriProperty); }
    set { SetValue (UriProperty, value); }
  }

  public void RegisterAction (Action<string> callback)
  {
    action = callback;
  }

  public void Cleanup ()
  {
    action = null;
  }

  public void InvokeAction (string data)
  {
    if (action == null || data == null) {
      return;
    }
    action.Invoke (data);
  }
}
```

`HybridWebView` Formant niestandardowy jest tworzony w projekcie biblioteki .NET Standard i definiuje następujący interfejs API dla formantu:

- A `Uri` właściwość, która określa adres strony sieci web do załadowania.
- A `RegisterAction` metodę, która rejestruje `Action` za pomocą kontrolki. Zarejestrowanych akcji zostanie wywołany poziomu języka JavaScript zawarty w pliku HTML, do których odwołuje się za pośrednictwem `Uri` właściwości.
- A `CleanUp` metodę, która usuwa odwołanie do zarejestrowanej `Action`.
- `InvokeAction` Metody, który wywołuje zarejestrowaną `Action`. Ta metoda zostanie wywołana z niestandardowego modułu renderowania w każdym projekcie specyficzne dla platformy.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>Korzystanie z HybridWebView

`HybridWebView` Formantu niestandardowego może być przywoływany w XAML w projekcie biblioteki .NET Standard deklarowanie przestrzeni nazw dla lokalizacji i używając prefiksu przestrzeni nazw w formancie niestandardowym. Poniższy kod przedstawia przykładowy sposób, w jaki `HybridWebView` kontrolki niestandardowej mogą być wykorzystane przez strony XAML:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             x:Class="CustomRenderer.HybridWebViewPage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <local:HybridWebView x:Name="hybridWebView" Uri="index.html"
          HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
    </ContentPage.Content>
</ContentPage>
```

`local` Prefiks przestrzeni nazw może mieć nazwę niczego. Jednak `clr-namespace` i `assembly` wartości muszą być zgodne szczegóły kontrolki niestandardowej. Po zadeklarowaniu przestrzeń nazw prefiks jest używany do odwołania kontrolki niestandardowej.

Poniższy kod przedstawia przykładowy sposób, w jaki `HybridWebView` kontrolki niestandardowej mogą być wykorzystane przez strony C#:

```csharp
public class HybridWebViewPageCS : ContentPage
{
  public HybridWebViewPageCS ()
  {
    var hybridWebView = new HybridWebView {
      Uri = "index.html",
      HorizontalOptions = LayoutOptions.FillAndExpand,
      VerticalOptions = LayoutOptions.FillAndExpand
    };
    ...
    Padding = new Thickness (0, 20, 0, 0);
    Content = hybridWebView;
  }
}
```

`HybridWebView` Wystąpienie zostanie użyte, aby wyświetlić formant web natywnych na każdej platformie. Ma ona `Uri` właściwość jest ustawiona na plik HTML, przechowywane w każdym projekcie specyficzne dla platformy i który będzie wyświetlany przez kontrolki natywne sieci web. Kod HTML renderowany prosi użytkownika o podanie swojego imienia, za pomocą funkcji języka JavaScript, wywoływania C# `Action` w odpowiedzi na kliknięcia przycisku kodu HTML.

`HybridWebViewPage` Rejestruje akcji wywoływanej poziomu języka JavaScript, jak pokazano w poniższym przykładzie kodu:

```csharp
public partial class HybridWebViewPage : ContentPage
{
  public HybridWebViewPage ()
  {
    ...
    hybridWebView.RegisterAction (data => DisplayAlert ("Alert", "Hello " + data, "OK"));
  }
}
```

Ta akcja wymaga [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) metodę w celu wyświetlenia modalne okno podręczne, które przedstawia nazwę wprowadzone na stronie HTML wyświetlanego przez `HybridWebView` wystąpienia.

Teraz można dodać niestandardowego modułu renderowania do każdego projektu aplikacji, aby zwiększyć kontrolkach internetowych specyficzne dla platformy, umożliwiając kod C# można wywołać z języka JavaScript.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Tworzenie niestandardowego modułu renderowania na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania jest następująca:

1. Utwórz podklasę `ViewRenderer<T1,T2>` klasę, która renderuje kontrolki niestandardowej. Pierwszy argument typu należy kontrolka niestandardowa jest modułu renderowania, w tym przypadku `HybridWebView`. Drugi argument typu powinien być natywne formant, który będzie implementowany widoku niestandardowego.
1. Zastąp `OnElementChanged` metodę, która renderuje niestandardowe logiki kontrola i zapisz go dostosować. Ta metoda jest wywoływana, gdy zostanie utworzony odpowiedni formant niestandardowego zestawu narzędzi Xamarin.Forms.
1. Dodaj `ExportRenderer` atrybutów do klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania formantu niestandardowego zestawu narzędzi Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania przy użyciu zestawu narzędzi Xamarin.Forms.

> [!NOTE]
> W przypadku większości elementów zestawu narzędzi Xamarin.Forms jest opcjonalny w celu zapewnienia niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej kontrolki będą używane. Jednakże, niestandardowe programy renderujące są wymagane w każdym projekcie platformy podczas renderowania [widoku](xref:Xamarin.Forms.View) elementu.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](hybridwebview-images/solution-structure.png "Obowiązki projektu programu renderującego niestandardowych HybridWebView")

`HybridWebView` Formantu niestandardowego jest renderowany przez klasy renderowania specyficzne dla platformy, które wynikają z `ViewRenderer` klasy dla każdej platformy. Skutkuje to każda `HybridWebView` kontrolek niestandardowych, które są renderowane przy użyciu kontrolki specyficzne dla platformy sieci web, jak pokazano na poniższych zrzutach ekranu:

![](hybridwebview-images/screenshots.png "HybridWebView na każdej platformie")

`ViewRenderer` Klasy ujawnia `OnElementChanged` metody, która jest wywoływana podczas tworzenia formantu niestandardowego zestawu narzędzi Xamarin.Forms do renderowania odpowiedni formant natywnych sieci web. Ta metoda przyjmuje `ElementChangedEventArgs` parametr, który zawiera `OldElement` i `NewElement` właściwości. Te właściwości reprezentują element zestawu narzędzi Xamarin.Forms, modułu renderowania *został* dołączyć i element zestawu narzędzi Xamarin.Forms, modułu renderowania *jest* dołączone do, odpowiednio. W przykładowej aplikacji `OldElement` właściwość będzie miała `null` i `NewElement` właściwość będzie zawierać odwołanie do `HybridWebView` wystąpienia.

Zastąpione wersję `OnElementChanged` metody, w każdej klasie renderowania specyficzne dla platformy, to miejsce, do wykonania podczas tworzenia wystąpienia natywnych sieci web kontroli i dostosowywania. `SetNativeControl` Metoda powinna służyć formantu macierzystych internetowej, a ta metoda również spowoduje przypisanie odwołania kontrolki do `Control` właściwości. Ponadto można uzyskać odwołanie do formantu Xamarin.Forms, który jest renderowany przy użyciu `Element` właściwości.

W niektórych sytuacjach `OnElementChanged` metoda może być wywoływana wiele razy. W związku z tym aby zapobiec przeciekom pamięci, należy uważać podczas tworzenia wystąpienia nowego formantu natywnych. W poniższym przykładzie kodu pokazano podejście do użycia podczas tworzenia wystąpienia nowego formantu natywne w niestandardowego modułu renderowania:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Nowy formant natywnych były tworzone tylko raz, gdy `Control` właściwość `null`. Kontrolki powinny być konfigurowane tylko i subskrypcję procedury obsługi zdarzeń, gdy niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms. Podobnie programy obsługi zdarzeń, które zostały zasubskrybowane przez powinna składać się wyłącznie usuwania subskrypcji, gdy element renderer jest dołączony do zmiany. Przyjęcie tego podejścia ma na celu tworzenie wydajnych niestandardowego modułu renderowania, nie odczuwają przecieków pamięci.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje modułu renderowania za pomocą zestawu narzędzi Xamarin.Forms. Ten atrybut przyjmuje dwa parametry — Nazwa typu formantu niestandardowego zestawu narzędzi Xamarin.Forms renderowanego i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono strukturę strony sieci web ładowane przez każdy formant natywnych sieci web, proces wywoływania C# poziomu języka JavaScript i wdrażania tego w każdej klasie niestandardowego modułu renderowania specyficzne dla platformy.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Tworzenie strony sieci Web

Poniższy przykład kodu pokazuje strony sieci web, która będzie wyświetlana przez `HybridWebView` kontrolek niestandardowych:

```html
<html>
<body>
<script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
<h1>HybridWebView Test</h1>
<br/>
Enter name: <input type="text" id="name">
<br/>
<br/>
<button type="button" onclick="javascript:invokeCSCode($('#name').val());">Invoke C# Code</button>
<br/>
<p id="result">Result:</p>
<script type="text/javascript">
function log(str)
{
    $('#result').text($('#result').text() + " " + str);
}

function invokeCSCode(data) {
    try {
        log("Sending Data:" + data);
        invokeCSharpAction(data);
    }
    catch (err){
          log(err);
    }
}
</script>
</body>
</html>
```

Strony sieci web umożliwia użytkownikowi podanie swojego imienia w `input` element i zapewnia `button` element, który będzie wywoływać kod C# po kliknięciu. Proces na osiągnięcie tego celu jest w następujący sposób:

- Gdy użytkownik kliknie `button` elementu `invokeCSCode` funkcja JavaScript jest wywoływana z wartością `input` element został przekazany do funkcji.
- `invokeCSCode` Wywołaniach funkcji `log` funkcję, aby wyświetlić dane, wysyła go do języka C# `Action`. Następnie wywołuje `invokeCSharpAction` metody do wywołania języka C# `Action`, przekazując parametru odebranego z `input` elementu.

`invokeCSharpAction` Funkcji języka JavaScript nie jest zdefiniowana na stronie sieci web, a także zostaną dodane do niego każdego niestandardowego modułu renderowania.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>Wywoływanie C# poziomu języka JavaScript

Proces dla wywołania języka C# poziomu języka JavaScript jest identyczny w każdej z platform:

- Niestandardowego modułu renderowania tworzy formant macierzystych internetowej i ładuje plik HTML określonego przez `HybridWebView.Uri` właściwości.
- Po załadowaniu strony sieci web niestandardowego modułu renderowania wprowadza `invokeCSharpAction` funkcji JavaScript do strony sieci web.
- Gdy użytkownik wprowadza imię i nazwisko ich i klika pozycję w kodzie HTML `button` elementu `invokeCSCode` zostanie wywołana funkcja, która z kolei wywołuje `invokeCSharpAction` funkcji.
- `invokeCSharpAction` Funkcja wywołuje metodę w przypadku niestandardowego modułu renderowania, które z kolei wywołuje `HybridWebView.InvokeAction` metody.
- `HybridWebView.InvokeAction` Metoda wywołuje zarejestrowaną `Action`.

W poniższych sekcjach zostaną omówiono sposób implementacji tego procesu na każdej platformie.

### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie niestandardowego modułu renderowania w systemie iOS

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy iOS:

```csharp
[assembly: ExportRenderer (typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.iOS
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, WKWebView>, IWKScriptMessageHandler
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.webkit.messageHandlers.invokeAction.postMessage(data);}";
        WKUserContentController userController;

        protected override void OnElementChanged (ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                userController = new WKUserContentController ();
                var script = new WKUserScript (new NSString (JavaScriptFunction), WKUserScriptInjectionTime.AtDocumentEnd, false);
                userController.AddUserScript (script);
                userController.AddScriptMessageHandler (this, "invokeAction");

                var config = new WKWebViewConfiguration { UserContentController = userController };
                var webView = new WKWebView (Frame, config);
                SetNativeControl (webView);
            }
            if (e.OldElement != null) {
                userController.RemoveAllUserScripts ();
                userController.RemoveScriptMessageHandler ("invokeAction");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup ();
            }
            if (e.NewElement != null) {
                string fileName = Path.Combine (NSBundle.MainBundle.BundlePath, string.Format ("Content/{0}", Element.Uri));
                Control.LoadRequest (new NSUrlRequest (new NSUrl (fileName, false)));
            }
        }

        public void DidReceiveScriptMessage (WKUserContentController userContentController, WKScriptMessage message)
        {
            Element.InvokeAction (message.Body.ToString ());
        }
    }
}
```

`HybridWebViewRenderer` Klasy ładowania strony sieci web, określone w `HybridWebView.Uri` właściwości do macierzystej [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) kontroli i `invokeCSharpAction` funkcji języka JavaScript są wstrzykiwane do strony sieci web. Po użytkownik wprowadza imię i nazwisko ich i klika pozycję HTML `button` elementu `invokeCSharpAction` funkcja JavaScript jest wykonywana, za pomocą `DidReceiveScriptMessage` metoda wywoływana po otrzymaniu wiadomości ze strony internetowej. Z kolei ta metoda wywołuje `HybridWebView.InvokeAction` metody, która będzie wywoływać zarejestrowanych akcji do wyświetlenia w oknie podręcznym.

Ta funkcja odbywa się w następujący sposób:

- Pod warunkiem, że `Control` właściwość `null`, przeprowadzane są następujące operacje:
  - A [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) tworzone jest wystąpienie, które umożliwia opublikowanie wiadomości i wstawianie skrypty użytkownika do strony sieci web.
  - A [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) iniekcję tworzone jest wystąpienie `invokeCSharpAction` funkcji JavaScript do strony sieci web po załadowaniu strony sieci web.
  - [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/) Metoda dodaje [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) wystąpienia kontrolerowi zawartości.
  - [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/) Metoda dodaje program obsługi komunikatów skryptu o nazwie `invokeAction` do [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) wystąpienia, co spowoduje, że funkcja JavaScript `window.webkit.messageHandlers.invokeAction.postMessage(data)` należy zdefiniować we wszystkich ramki we wszystkich widokach w sieci web, które będą używane `WKUserContentController` wystąpienia.
  - A [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/) tworzone jest wystąpienie, za pomocą [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) wystąpienia zostanie ustawiony jako kontroler zawartości.
  - A [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) sterowania zostanie uruchomiony i `SetNativeControl` metoda jest wywoływana, aby przypisać odwołanie do `WKWebView` kontrolę `Control` właściwości.
- Pod warunkiem, że niestandardowego modułu renderowania jest podłączony do nowego elementu zestawu narzędzi Xamarin.Forms:
  - [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/) Metoda ładuje plik HTML, który jest określony przez `HybridWebView.Uri` właściwości. Kod ten określa, czy plik jest przechowywany w `Content` folderu projektu. Po wyświetleniu strony sieci web `invokeCSharpAction` funkcji języka JavaScript, które będą wstrzyknięte do strony sieci web.
- Gdy element modułu renderowania jest dołączony do zmiany:
  - Zasoby są zwalniane.

> [!NOTE]
> `WKWebView` Klasy jest obsługiwana tylko w systemie iOS 8 lub nowszy.

### <a name="creating-the-custom-renderer-on-android"></a>Tworzenie niestandardowego modułu renderowania w systemie Android

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy systemu Android:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Android.Webkit.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
        Context _context;

        public HybridWebViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                var webView = new Android.Webkit.WebView(_context);
                webView.Settings.JavaScriptEnabled = true;
                SetNativeControl(webView);
            }
            if (e.OldElement != null)
            {
                Control.RemoveJavascriptInterface("jsBridge");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup();
            }
            if (e.NewElement != null)
            {
                Control.AddJavascriptInterface(new JSBridge(this), "jsBridge");
                Control.LoadUrl(string.Format("file:///android_asset/Content/{0}", Element.Uri));
                InjectJS(JavaScriptFunction);
            }
        }

        void InjectJS(string script)
        {
            if (Control != null)
            {
                Control.LoadUrl(string.Format("javascript: {0}", script));
            }
        }
    }
}
```

`HybridWebViewRenderer` Klasy ładowania strony sieci web, określone w `HybridWebView.Uri` właściwości do macierzystej [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) kontroli i `invokeCSharpAction` funkcji języka JavaScript są wstrzykiwane do strony sieci web po załadowaniu strony sieci web za pomocą `InjectJS` metody. Po użytkownik wprowadza imię i nazwisko ich i klika pozycję HTML `button` elementu `invokeCSharpAction` funkcja JavaScript jest wykonywana. Ta funkcja odbywa się w następujący sposób:

- Pod warunkiem, że `Control` właściwość `null`, przeprowadzane są następujące operacje:
  - Natywny [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) tworzone jest wystąpienie i języka JavaScript jest włączone w formancie.
  - `SetNativeControl` Metoda jest wywoływana, aby przypisać odwołania do macierzystych [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) kontrolę `Control` właściwości.
- Pod warunkiem, że niestandardowego modułu renderowania jest podłączony do nowego elementu zestawu narzędzi Xamarin.Forms:
  - [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) Metoda wprowadza nową `JSBridge` wystąpienia w ramce głównej kontekstu JavaScript WebView, nadając mu nazwę `jsBridge`. Dzięki temu metody `JSBridge` klasy można uzyskać dostęp z poziomu języka JavaScript.
  - [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) Metoda ładuje plik HTML, który jest określony przez `HybridWebView.Uri` właściwości. Kod ten określa, czy plik jest przechowywany w `Content` folderu projektu.
  - `InjectJS` Metoda jest wywoływana, aby wstawić `invokeCSharpAction` funkcji JavaScript do strony sieci web.
- Gdy element modułu renderowania jest dołączony do zmiany:
  - Zasoby są zwalniane.

Gdy `invokeCSharpAction` funkcji języka JavaScript jest wykonywana, z kolei wywołuje `JSBridge.InvokeAction` metody, która jest wyświetlana w poniższym przykładzie kodu:

```csharp
public class JSBridge : Java.Lang.Object
{
  readonly WeakReference<HybridWebViewRenderer> hybridWebViewRenderer;

  public JSBridge (HybridWebViewRenderer hybridRenderer)
  {
    hybridWebViewRenderer = new WeakReference <HybridWebViewRenderer> (hybridRenderer);
  }

  [JavascriptInterface]
  [Export ("invokeAction")]
  public void InvokeAction (string data)
  {
    HybridWebViewRenderer hybridRenderer;

    if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget (out hybridRenderer)) {
      hybridRenderer.Element.InvokeAction (data);
    }
  }
}
```

Klasa musi pochodzić od klasy `Java.Lang.Object`, i metod, które są dostępne w kodzie JavaScript musi posiadać `[JavascriptInterface]` i `[Export]` atrybutów. W związku z tym, gdy `invokeCSharpAction` funkcji języka JavaScript są wstrzykiwane do strony sieci web i jest wykonywany, będzie on wywołać `JSBridge.InvokeAction` metodę z powodu trwa ozdobione `[JavascriptInterface]` i `[Export("invokeAction")]` atrybutów. Z kolei `InvokeAction` wywołuje metodę `HybridWebView.InvokeAction` metody, które będą wywoływane zarejestrowanych akcji do wyświetlenia w oknie podręcznym.

> [!NOTE]
> Projekty używające `[Export]` atrybutu musi zawierać odwołanie do `Mono.Android.Export`, lub spowoduje wystąpienie błędu kompilatora.

Należy pamiętać, że `JSBridge` klasa przechowuje `WeakReference` do `HybridWebViewRenderer` klasy. Pozwoli to uniknąć, tworząc odwołanie cykliczne między dwoma klasami. Aby uzyskać więcej informacji, zobacz [słabe odwołania](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx) w witrynie MSDN.

> [!IMPORTANT]
> W systemie Android Oreo upewnij się, że ustawia manifestu systemu Android **docelowej wersji systemu Android** do **automatyczne**. W przeciwnym razie uruchomienie tego kodu spowoduje błąd komunikat "invokeCSharpAction nie zdefiniowano".

### <a name="creating-the-custom-renderer-on-uwp"></a>Tworzenie niestandardowego modułu renderowania na platformy uniwersalnej systemu Windows

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy uniwersalnej systemu Windows:

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.UWP
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Windows.UI.Xaml.Controls.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.external.notify(data);}";

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                SetNativeControl(new Windows.UI.Xaml.Controls.WebView());
            }
            if (e.OldElement != null)
            {
                Control.NavigationCompleted -= OnWebViewNavigationCompleted;
                Control.ScriptNotify -= OnWebViewScriptNotify;
            }
            if (e.NewElement != null)
            {
                Control.NavigationCompleted += OnWebViewNavigationCompleted;
                Control.ScriptNotify += OnWebViewScriptNotify;
                Control.Source = new Uri(string.Format("ms-appx-web:///Content//{0}", Element.Uri));
            }
        }

        async void OnWebViewNavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
        {
            if (args.IsSuccess)
            {
                // Inject JS script
                await Control.InvokeScriptAsync("eval", new[] { JavaScriptFunction });
            }
        }

        void OnWebViewScriptNotify(object sender, NotifyEventArgs e)
        {
            Element.InvokeAction(e.Value);
        }
    }
}
```

`HybridWebViewRenderer` Klasy ładowania strony sieci web, określone w `HybridWebView.Uri` właściwości do macierzystej `WebView` kontrolki i `invokeCSharpAction` funkcji języka JavaScript są wstrzykiwane do strony sieci web po załadowaniu strony sieci web za pomocą `WebView.InvokeScriptAsync` metody. Po użytkownik wprowadza imię i nazwisko ich i klika pozycję HTML `button` elementu `invokeCSharpAction` funkcja JavaScript jest wykonywana, za pomocą `OnWebViewScriptNotify` metoda wywoływana po otrzymaniu powiadomienia na stronie sieci web. Z kolei ta metoda wywołuje `HybridWebView.InvokeAction` metody, która będzie wywoływać zarejestrowanych akcji do wyświetlenia w oknie podręcznym.

Ta funkcja odbywa się w następujący sposób:

- Pod warunkiem, że `Control` właściwość `null`, przeprowadzane są następujące operacje:
  - `SetNativeControl` Metoda jest wywoływana w celu utworzenia wystąpienia nowego native `WebView` kontroli i odwołania do niej przypisać `Control` właściwości.
- Pod warunkiem, że niestandardowego modułu renderowania jest podłączony do nowego elementu zestawu narzędzi Xamarin.Forms:
  - Programy obsługi zdarzeń dla `NavigationCompleted` i `ScriptNotify` zdarzenia są rejestrowane. `NavigationCompleted` Zdarzenie uruchamiane w przypadku obu natywnych `WebView` kontroli zakończył ładowanie bieżącej zawartości lub Jeśli nawigacja nie powiodła się. `ScriptNotify` Zdarzenia generowane, gdy zawartość w natywnym `WebView` kontroli używa języka JavaScript do przekazania parametrów do aplikacji. Generowane strony sieci web `ScriptNotify` zdarzeń przez wywołanie metody `window.external.notify` podczas przekazywania `string` parametru.
  - `WebView.Source` Właściwość jest ustawiona na identyfikator URI pliku HTML, który jest określony przez `HybridWebView.Uri` właściwości. W kodzie założono, że plik jest przechowywany w `Content` folderu projektu. Po wyświetleniu strony sieci web `NavigationCompleted` będą wyzwalać zdarzenia i `OnWebViewNavigationCompleted` zostanie wywołana metoda. `invokeCSharpAction` Będą następnie wstrzyknięte funkcji języka JavaScript do strony sieci web za pomocą `WebView.InvokeScriptAsync` metody, pod warunkiem, że nawigacja została ukończona pomyślnie.
- Gdy element modułu renderowania jest dołączony do zmiany:
  - Zdarzenia są usuwania subskrypcji.

## <a name="summary"></a>Podsumowanie

Ten artykuł ma pokazano sposób tworzenia niestandardowego modułu renderowania dla `HybridWebView` formant niestandardowy, który pokazuje, jak poprawić formantów web specyficzne dla platformy umożliwiający kodu C# do wywołania języka JavaScript.


## <a name="related-links"></a>Linki pokrewne

- [CustomRendererHybridWebView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [Wywoływanie C# z języka JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
