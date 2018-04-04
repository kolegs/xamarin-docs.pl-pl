---
title: Zmiany w sieci Web w systemie iOS 11
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/12/2016
ms.openlocfilehash: 5cbf1d2f6c7b8a110cb65cad81df18f9f0568fda
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="web-changes-in-ios-11"></a>Zmiany w sieci Web w systemie iOS 11

iOS 11 wprowadziła nową wersję przeglądarki sieci web Safari — Safari 11.0 — w tym zmiany WebKit i SafariServices. Ten przewodnik opisuje te zmiany.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` został wprowadzony w systemie iOS 9 jako opcję wyświetlania zawartości sieci web lub uwierzytelniania użytkowników z aplikacji. Więcej informacji na temat funkcji można znaleźć w [widoki sieci Web](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) przewodnik.

iOS 11 wprowadziła styl aktualizacji do kontrolera widoku Safari, przyznawanie użytkownikom sprawniejsze między aplikacją i sieci web. Na przykład usunięcie paska zapewnia teraz kontroler widoku działania przeglądarki w aplikacji, a nie mini przeglądarki Safari adresu. Można również dostosować schemat kolorów, aby zmieścić się przy użyciu schemat kolorów aplikacji przez ustawienie `preferredBarTintColor` i `PreferredControlTintColor` właściwości:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

Poniższy fragment kodu renderuje paski purpurowy i białe, wyświetlane na poniższej ilustracji:

![Paski SFSafariViewController renderowane w purpurowy i biały](web-images/image1.png)

Przycisk Odrzuć przedstawionych w kontroler widoku Safari można także zmienić ustawienie `DismissButtonStyle` właściwości albo `Done`, `Close`, lub `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Odrzuć zmiany tekstu przycisku](web-images/image2.png)

Ta wartość może zostać zmieniona podczas `SFSafariViewController` jest przedstawiony.


W zależności od zawartości wyświetlanej wewnątrz kontrolera widoku Safari może być konieczne do zapewnienia, że gdy użytkownik przewija nie Zwiń paski menu. Ta opcja jest włączona, ustawiając nowe `BarCollapsedEnabled` właściwości `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Pasek zwijanie wyłączone](web-images/image3.png)

Apple ma również aktualizacje na prywatność w kontroler widoku Safari w systemie iOS 11. Teraz przeglądania danych, takich jak pliki cookie i lokalny magazyn istnieć tylko na podstawie dla aplikacji, a nie we wszystkich wystąpieniach kontrolera widoku Safari. Dzięki temu działań przeglądania użytkownika prywatnych w Twojej aplikacji.

Dodatkowe funkcje takie jak przeciągnij i upuść obsługę adresów URL i obsługę `window.open()` również zostały dodane do `SFSafariViewController` w systemie iOS 11. Można znaleźć więcej informacji na temat nowych funkcji w [dokumentacji SFSafariViewController firmy Apple](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).


## <a name="webkit"></a>WebKit

`WKWebView` został wprowadzony w ramach usługi WebKit iOS 8 jako sposób wyświetlania zawartości sieci web do użytkownika. Jest znacznie więcej można dostosowywać niż `SFSafariViewController`, pozwala na tworzenie własnego nawigacji i interfejs użytkownika.

Apple wprowadziła trzy główne ulepszenia `WKWebView` z systemem iOS 11: 

- Możliwość zarządzania plikami cookie
- Filtrowanie zawartości
- Ładowanie zasobów niestandardowych. 

Plik cookie zarządzania odbywa się za pośrednictwem nowej [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) klasy, która służy do dodawania i usuwania plików cookie, można pobrać wszystkich plików cookie przechowywany w WKWebView i obserwować pliku cookie, zapisać zmiany.

Zawartości Filtrowanie umożliwia zarządzanie typu zawartości, która będzie widoczna użytkownika, umożliwiając upewnij się, że jest bezpieczne, rodziny przyjazny, a w razie potrzeby, dostępne tylko do wybranej grupy użytkowników. To jest implementowane za pomocą nowej [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) klasy, podając pary wyzwalacze i akcje w formacie JSON. Więcej informacji na temat tych wyzwalacze i akcje znajdują się w firmy Apple [zasady blokowania zawartości](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) przewodnik.

iOS 11 umożliwia teraz dostosowanie `WKWebView` z zasobów niestandardowych ładowania dla zawartości sieci web. To jest implementowane za pośrednictwem `IWKUrlSchemeHandler` interfejsu, co pozwala obsługiwać schematy adresów URL, niebędących macierzysty Kit sieci Web. Ten interfejs jest rozpoczęcie i zakończenie metody, która musi zostać wdrożona:

```csharp
public class MyHandler : NSObject, IWKUrlSchemeHandler {

    [Export("webView:startURLSchemeTask:")]
    public void StartUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        
        // Implement a IWKUrlSchemeTask here
        var response = new NSUrlResponse(urlSchemeTask.Request.Url, "text/html", ContentLength, null);
        urlSchemeTask.DidReceiveResponse(response);
        urlSchemeTask.DidReceiveData(someData);
        urlSchemeTask.DidFinish();
    }

    [Export("webView:stopURLSchemeTask:")]
    public void StopUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        throw new NotImplementedException();
    }

}
``` 

Gdy program obsługi został zaimplementowany, użyj jej do ustawienia `SetUrlSchemeHandler` właściwość `WKWebViewConfiguration`. Następnie należy załadować adres URL elementu, który używa schematu niestandardowego:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

