---
title: Implementowanie HybridWebView
description: W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania HybridWebView kontrolki niestandardowej, która pokazuje, jak poprawić formanty specyficzne dla platformy sieci web umożliwia kodu C# do wywołania z poziomu języka JavaScript.
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: d2cce7598fde4cf59a91940161e605860847623e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241302"
---
# <a name="implementing-a-hybridwebview"></a>Implementowanie HybridWebView

_Formanty interfejsu użytkownika platformy Xamarin.Forms powinien pochodzić od klasy widoku, który służy do umieszczania układy i kontroli na ekranie. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania HybridWebView kontrolki niestandardowej, która pokazuje, jak poprawić formanty specyficzne dla platformy sieci web umożliwia kodu C# do wywołania z poziomu języka JavaScript._

Każdy widok platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu. Gdy [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) jest renderowany przez aplikację platformy Xamarin.Forms w systemie iOS, `ViewRenderer` tworzenia wystąpienia klasy, która z kolei tworzy natywny `UIView` formantu. Na platformie Android `ViewRenderer` tworzy wystąpienie klasy `View` formantu. W systemie Windows platformy Uniwersalnej, `ViewRenderer` natywny tworzy wystąpienie klasy `FrameworkElement` formantu. Aby uzyskać więcej informacji na temat klasy macierzystego formantu, mapowane na formanty platformy Xamarin.Forms i renderowania, zobacz [renderowania klasy podstawowej i kontrolki natywne](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono związek między [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) i odpowiednie natywnego formantów, które implementuje ona:

![](hybridwebview-images/view-classes.png "Relacja między jego implementującej klasy natywnej i klasy widoku")

Proces renderowania może służyć do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) na każdej z platform. Proces ten wygląda następująco:

1. [Utwórz](#Creating_the_HybridWebView) `HybridWebView` kontrolki niestandardowej.
1. [Korzystać z](#Consuming_the_HybridWebView) `HybridWebView`z platformy Xamarin.Forms.
1. [Utwórz](#Creating_the_Custom_Renderer_on_each_Platform) niestandardowego modułu renderowania dla `HybridWebView` na każdej z platform.

Każdy element zostanie teraz omówiony z kolei do zaimplementowania `HybridWebView` renderowania zwiększającym formanty specyficzne dla platformy sieci web umożliwia kodu C# do wywołania z poziomu języka JavaScript. `HybridWebView` Wystąpienie zostanie użyte do wyświetlenia strony HTML z prośbą o wprowadzenie ich nazwy. Następnie, gdy użytkownik kliknie przycisk HTML, funkcja JavaScript wywoła C# `Action` który wyświetla okno podręczne zawierające nazwę użytkownika.

Aby uzyskać więcej informacji na temat procesu dla wywoływanie C# z poziomu języka JavaScript, zobacz [wywoływania C# z poziomu języka JavaScript](#Invoking_C_from_JavaScript). Aby uzyskać więcej informacji na temat strony HTML, zobacz [tworzenia strony sieci Web](#Creating_the_Web_Page).

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>Tworzenie HybridWebView

`HybridWebView` Kontrolki niestandardowej mogą być tworzone przez podklasy [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) klasy, jak pokazano w poniższym przykładzie:

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

`HybridWebView` Formant niestandardowy jest tworzony w .NET Standard projektu biblioteki i definiuje następujące interfejsu API dla formantu:

- A `Uri` właściwości, który określa adres strony sieci web do załadowania.
- A `RegisterAction` metodę, która rejestruje `Action` z formantem. Zarejestrowanych akcji zostanie wywołany z poziomu języka JavaScript zawarty w pliku HTML, do których odwołuje się za pośrednictwem `Uri` właściwości.
- A `CleanUp` metodę, która usuwa odwołanie do zarejestrowaną `Action`.
- `InvokeAction` Metodę, która wywołuje zarejestrowaną `Action`. Ta metoda zostanie wywołana z niestandardowego modułu renderowania w każdym projekcie specyficzne dla platformy.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>Korzystanie z HybridWebView

`HybridWebView` Formant niestandardowy może być przywoływany w języku XAML w .NET Standard projektu biblioteki deklarowanie przestrzeni nazw dla lokalizacji, a następnie użyć prefiksu przestrzeni nazw na kontrolki niestandardowej. Poniższy kod przedstawia przykład sposobu `HybridWebView` kontrolki niestandardowej, może być zużyte przez strony XAML:

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

`local` Prefiks przestrzeni nazw może mieć nazwę żadnych czynności. Jednak `clr-namespace` i `assembly` wartości muszą być zgodne szczegóły kontrolki niestandardowej. Po zadeklarowaniu obszaru nazw prefiks jest używany do odwołania kontrolki niestandardowej.

Poniższy kod przedstawia przykład sposobu `HybridWebView` kontrolki niestandardowej mogą być używane przez stronę C#:

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

`HybridWebView` Wystąpienie zostanie użyte do wyświetlania formantu natywnego sieci web na każdej z platform. Ma ona `Uri` właściwość jest ustawiona na plik HTML, który jest przechowywany w każdym projekcie specyficzne dla platformy i który będzie wyświetlany przez formant natywnego sieci web. Renderowany kod HTML z monitem o wprowadzić odpowiednią nazwę funkcji JavaScript, wywoływania C# `Action` w odpowiedzi na kliknięcie przycisku kodu HTML.

`HybridWebViewPage` Rejestruje akcji do wywołania z poziomu języka JavaScript, jak pokazano w poniższym przykładzie:

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

Ta akcja wymaga [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) metodę w celu wyświetlenia modalne okno wyskakujące przedstawia informacje o nazwie wprowadzone na stronie HTML wyświetlanego przez `HybridWebView` wystąpienia.

Teraz można dodać niestandardowego modułu renderowania do każdego projektu aplikacji w celu zwiększenia formanty specyficzne dla platformy sieci web, zezwalając kodu C# do wywołania z poziomu języka JavaScript.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Tworzenie niestandardowego modułu renderowania na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania wygląda następująco:

1. Utwórz podklasę `ViewRenderer<T1,T2>` klasy, która renderuje kontrolkę niestandardową. Pierwszy argument typu powinien być kontrolki niestandardowej jest mechanizm renderujący, w tym przypadku `HybridWebView`. Drugi argument typu powinny być macierzystego formantu, który będzie implementowany widok niestandardowy.
1. Zastąpienie `OnElementChanged` metodę, która renderuje niestandardowej logiki kontroli i zapisu, aby dostosować go. Ta metoda jest wywoływana po utworzeniu odpowiednich platformy Xamarin.Forms kontrolki niestandardowej.
1. Dodaj `ExportRenderer` atrybutu klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania Kontrolki niestandardowe platformy Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania z platformy Xamarin.Forms.

> [!NOTE]
> W przypadku większości elementów platformy Xamarin.Forms jest opcjonalne zapewnić niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej formantu będzie używany. Jednak niestandardowe moduły renderowania są wymagane w każdym projekcie platformy podczas renderowania [widoku](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) elementu.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](hybridwebview-images/solution-structure.png "Obowiązki HybridWebView niestandardowe renderowania projektu:")

`HybridWebView` Kontrolki niestandardowej jest renderowany przez klasy renderowania specyficzne dla platformy, które wynikają z `ViewRenderer` klasy dla każdej platformy. Powoduje to w każdym `HybridWebView` kontrolki niestandardowej renderowanego kontroli specyficzne dla platformy sieci web, jak pokazano na poniższych zrzutach ekranu:

![](hybridwebview-images/screenshots.png "HybridWebView na każdej platformie")

`ViewRenderer` Klasy ujawnia `OnElementChanged` metodę, która jest wywoływana po utworzeniu platformy Xamarin.Forms formantu niestandardowego do renderowania kontrolki na odpowiednich natywnego sieci web. Ta metoda przyjmuje `ElementChangedEventArgs` parametr, który zawiera `OldElement` i `NewElement` właściwości. Te właściwości reprezentuje elementu platformy Xamarin.Forms który renderującego *został* dołączyć i elementu platformy Xamarin.Forms który renderującego *jest* dołączony do, odpowiednio. W przykładowej aplikacji `OldElement` właściwość będzie `null` i `NewElement` właściwości będzie zawierać odwołanie do `HybridWebView` wystąpienia.

Zastąpiona wersja `OnElementChanged` metody w każdej klasie renderowania specyficzne dla platformy jest miejscem do wykonania podczas tworzenia wystąpienia web macierzystego formantu i dostosowywania. `SetNativeControl` Metodę formantu natywnego sieci web, a ta metoda będzie również przypisać odwołania formantu do `Control` właściwości. Ponadto można uzyskać odwołanie do formantu platformy Xamarin.Forms, który jest renderowany przy użyciu `Element` właściwości.

W niektórych sytuacjach `OnElementChanged` metoda może być wywołana wiele razy. W związku z tym aby zapobiec przecieki pamięci, należy uważać podczas tworzenia wystąpienia nowego macierzystego formantu. Podejście do użycia podczas tworzenia wystąpienia nowego macierzystego formantu w niestandardowego modułu renderowania pokazano w poniższym przykładzie kodu:

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

Nowe macierzystego formantu były tworzone tylko raz, jeśli `Control` jest właściwość `null`. Kontrolki tylko należy skonfigurować i subskrypcję procedury obsługi zdarzeń, gdy niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms. Podobnie programy obsługi zdarzeń, które zostały subskrybuje tylko należy anulować w, gdy element renderującego jest dołączony do zmiany. Przyjmowanie takie podejście pomoże utworzyć wydajności renderowania niestandardowych, które nie występują z przecieki pamięci.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje mechanizm renderujący platformy Xamarin.Forms. Atrybut przyjmuje dwa parametry — Nazwa typu formantu niestandardowego platformy Xamarin.Forms renderowanego i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono struktury strony sieci web załadowanych przez każdego formantu w macierzystym sieci web, proces wywoływanie C# z poziomu języka JavaScript i wykonania w każdej klasie niestandardowego modułu renderowania specyficzne dla platformy.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Tworzenie strony sieci Web

Poniższy przykład kodu pokazuje strony sieci web, który będzie wyświetlany przez `HybridWebView` kontrolki niestandardowej:

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

Strony sieci web umożliwia użytkownikom wprowadzanie nazwy na `input` element i udostępnia `button` element, który wywoła kodu C# po kliknięciu. Proces na osiągnięcie tego celu jest następujący:

- Gdy użytkownik kliknie `button` elementu `invokeCSCode` funkcja JavaScript jest wywoływana z wartością `input` element przekazywany do funkcji.
- `invokeCSCode` Wywołania funkcji `log` funkcji, aby wyświetlić dane wysyła go do języka C# `Action`. Następnie wywołuje `invokeCSharpAction` metody do wywołania języka C# `Action`, przekazywanie parametru otrzymanych od `input` elementu.

`invokeCSharpAction` Funkcji JavaScript nie jest zdefiniowana na stronie sieci web i zostaną dodane do niego każdego niestandardowego modułu renderowania.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>Wywoływanie C# z poziomu języka JavaScript

Proces wywoływania C# z poziomu języka JavaScript jest identyczny na każdej platformie:

- Niestandardowego modułu renderowania tworzy kontrolkę natywnego sieci web i ładuje plik HTML, określony przez `HybridWebView.Uri` właściwości.
- Po załadowaniu strony sieci web niestandardowego modułu renderowania injects `invokeCSharpAction` funkcji JavaScript do strony sieci web.
- Gdy użytkownik wprowadza nazwę ich i kliknie HTML `button` elementu `invokeCSCode` jest wywoływana funkcja, która z kolei wywołuje `invokeCSharpAction` funkcji.
- `invokeCSharpAction` Funkcja wywołuje metodę w niestandardowego modułu renderowania, który z kolei wywołuje `HybridWebView.InvokeAction` metody.
- `HybridWebView.InvokeAction` Metoda wywołuje zarejestrowaną `Action`.

W poniższych sekcjach zostaną omówiono sposób implementacji tego procesu na każdej z platform.

### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie modułu renderowania niestandardowe w systemie iOS

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

`HybridWebViewRenderer` Klasy wyświetla stronę sieci web określonego w `HybridWebView.Uri` właściwości do natywny [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) kontroli i `invokeCSharpAction` funkcji JavaScript jest wstrzykiwane do strony sieci web. Gdy użytkownik wprowadza nazwę ich i klika pozycję HTML `button` elementu `invokeCSharpAction` funkcja JavaScript jest wykonywana, z `DidReceiveScriptMessage` metoda wywoływana po otrzymaniu wiadomości ze strony sieci web. Z kolei, ta metoda wywołuje `HybridWebView.InvokeAction` metodę, która wywoła zarejestrowanych akcji do wyświetlenia w oknie podręcznym.

Ta funkcja jest osiągnięte w następujący sposób:

- Pod warunkiem że `Control` właściwość jest `null`, przeprowadzane są następujące operacje:
  - A [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) jest tworzone wystąpienie, który umożliwia przesyłanie komunikatów i wstrzykiwania skryptów użytkownika do strony sieci web.
  - A [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) iniekcję jest tworzone wystąpienie `invokeCSharpAction` funkcji JavaScript do strony sieci web po załadowaniu strony sieci web.
  - [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/) Dodaje metody [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) wystąpienia kontrolerowi zawartości.
  - [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/) Metoda dodaje program obsługi komunikatów skryptu o nazwie `invokeAction` do [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) wystąpienia, która spowoduje, że funkcja JavaScript `window.webkit.messageHandlers.invokeAction.postMessage(data)` do zdefiniowania we wszystkich ramki we wszystkich widokach sieci web, które będą korzystać z `WKUserContentController` wystąpienia.
  - A [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/) jest tworzone wystąpienie, z [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) wystąpienia ustawiany jako kontroler zawartości.
  - A [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) formantu zostanie uruchomiony oraz `SetNativeControl` metoda jest wywoływana można przypisać odwołania do `WKWebView` formant `Control` właściwości.
- Pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms:
  - [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/) Metody ładuje plik HTML, który jest określony przez `HybridWebView.Uri` właściwości. Kod określa, że plik jest przechowywany w `Content` folderu projektu. Po wyświetleniu strony sieci web `invokeCSharpAction` funkcji JavaScript, które zostaną dodane do strony sieci web.
- Gdy element renderującego jest dołączony do zmiany:
  - Zasoby zostały zwolnione.

> [!NOTE]
> `WKWebView` Klasa jest obsługiwana tylko w systemie iOS 8 lub nowszy.

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

`HybridWebViewRenderer` Klasy wyświetla stronę sieci web określonego w `HybridWebView.Uri` właściwości do natywny [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) kontroli i `invokeCSharpAction` funkcji JavaScript jest wstrzykiwane do strony sieci web po stronie sieci web został załadowany, z `InjectJS` metody. Gdy użytkownik wprowadza nazwę ich i klika pozycję HTML `button` elementu `invokeCSharpAction` funkcja JavaScript jest wykonywana. Ta funkcja jest osiągnięte w następujący sposób:

- Pod warunkiem że `Control` właściwość jest `null`, przeprowadzane są następujące operacje:
  - Natywny [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) jest tworzone wystąpienie i JavaScript jest włączone w formancie.
  - `SetNativeControl` Metoda jest wywoływana, aby przypisać odwołanie do natywnego [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) formant `Control` właściwości.
- Pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms:
  - [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) Metody injects nowy `JSBridge` wystąpienie w ramce głównej kontekstu JavaScript widoku sieci Web, nadając mu nazwę `jsBridge`. Dzięki temu metod w `JSBridge` klasy można uzyskać dostęp z poziomu języka JavaScript.
  - [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) Metody ładuje plik HTML, który jest określony przez `HybridWebView.Uri` właściwości. Kod określa, że plik jest przechowywany w `Content` folderu projektu.
  - `InjectJS` Iniekcję wywoływana jest metoda `invokeCSharpAction` funkcji JavaScript do strony sieci web.
- Gdy element renderującego jest dołączony do zmiany:
  - Zasoby zostały zwolnione.

Gdy `invokeCSharpAction` funkcji JavaScript jest wykonywana, z kolei wywołuje `JSBridge.InvokeAction` metodę, która jest wyświetlana w poniższym przykładzie:

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

Klasa musi pochodzić od `Java.Lang.Object`, oraz metody, które są widoczne dla JavaScript musi być dekorowane za `[JavascriptInterface]` i `[Export]` atrybutów. W związku z tym, kiedy `invokeCSharpAction` funkcji JavaScript jest wstrzykiwane do strony sieci web i jest wykonywana, zostanie wywołany `JSBridge.InvokeAction` metody z powodu trwa ozdobione `[JavascriptInterface]` i `[Export("invokeAction")]` atrybutów. Z kolei `InvokeAction` wywołuje metodę `HybridWebView.InvokeAction` metody, które będą wywoływane zarejestrowanych akcji do wyświetlenia w oknie podręcznym.

> [!NOTE]
> Projekty używające `[Export]` atrybutu musi zawierać odwołanie do `Mono.Android.Export`, lub spowoduje błąd kompilatora.

Należy pamiętać, że `JSBridge` obsługuje klasy `WeakReference` do `HybridWebViewRenderer` klasy. Pozwoli to uniknąć, tworząc odwołanie cykliczne między dwiema klasami. Aby uzyskać więcej informacji, zobacz [słabe odwołania](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx) w witrynie MSDN.

> [!IMPORTANT]
> W systemie Android Oreo upewnij się, że ustawia manifestu systemu Android **wersji docelowej Android** do **automatyczne**. W przeciwnym razie uruchomienie tego kodu spowoduje błąd komunikat "invokeCSharpAction nie zdefiniowano".

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

`HybridWebViewRenderer` Klasy wyświetla stronę sieci web określonego w `HybridWebView.Uri` właściwości do natywny `WebView` kontroli i `invokeCSharpAction` funkcji JavaScript jest wstrzykiwane do strony sieci web po stronie sieci web został załadowany, z `WebView.InvokeScriptAsync` — metoda. Gdy użytkownik wprowadza nazwę ich i klika pozycję HTML `button` elementu `invokeCSharpAction` funkcja JavaScript jest wykonywana, z `OnWebViewScriptNotify` metoda wywoływana po otrzymaniu powiadomienia z strony sieci web. Z kolei, ta metoda wywołuje `HybridWebView.InvokeAction` metodę, która wywoła zarejestrowanych akcji do wyświetlenia w oknie podręcznym.

Ta funkcja jest osiągnięte w następujący sposób:

- Pod warunkiem że `Control` właściwość jest `null`, przeprowadzane są następujące operacje:
  - `SetNativeControl` Metoda jest wywoływana można utworzyć nowego natywny `WebView` kontroli i przypisać odwołanie do niej do `Control` właściwości.
- Pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms:
  - Programy obsługi zdarzeń dla `NavigationCompleted` i `ScriptNotify` są rejestrowane zdarzenia. `NavigationCompleted` Zdarzenie uruchamiane w przypadku obu natywnego `WebView` kontroli zakończył ładowanie bieżącej zawartości lub gdy nawigacja nie powiodła się. `ScriptNotify` Zdarzenia generowane, gdy zawartość w natywnym `WebView` kontroli przekazywane ciąg do aplikacji za pomocą kodu JavaScript. Strony sieci web uruchamiany `ScriptNotify` zdarzenia przez wywołanie metody `window.external.notify` podczas `string` parametru.
  - `WebView.Source` Właściwość jest ustawiona na identyfikator URI pliku HTML, który jest określony przez `HybridWebView.Uri` właściwości. Kod przyjęto założenie, że plik jest przechowywany w `Content` folderu projektu. Po wyświetleniu strony sieci web `NavigationCompleted` będą wyzwalać zdarzeń i `OnWebViewNavigationCompleted` można wywołać metody. `invokeCSharpAction` Funkcji JavaScript zostaną następnie dodane do strony sieci web z `WebView.InvokeScriptAsync` metody, pod warunkiem, że nawigacji zakończyła się pomyślnie.
- Gdy element renderującego jest dołączony do zmiany:
  - Zdarzenia są anulować w.

## <a name="summary"></a>Podsumowanie

Ten artykuł ma przedstawiono sposób tworzenia niestandardowego modułu renderowania dla `HybridWebView` formantu niestandardowego, który demonstruje sposób zwiększenia formanty specyficzne dla platformy sieci web umożliwia kodu C# do wywołania z poziomu języka JavaScript.


## <a name="related-links"></a>Linki pokrewne

- [CustomRendererHybridWebView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [Wywołanie C# z poziomu języka JavaScript](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript/)
