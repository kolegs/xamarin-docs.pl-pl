---
title: Zmiany w sieci Web w systemie iOS 11
description: W tym dokumencie omówiono zmiany wprowadzone do aparatu WebKit i framework usług Safari w systemie iOS 11. Go w tym artykule opisano sposób pracy z style w SFSafariViewController aktualizacji i nowych funkcji w WKWebView.
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/12/2017
ms.openlocfilehash: 00587e3b49e953b780a49623f081ae798e81fa61
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351460"
---
# <a name="web-changes-in-ios-11"></a>Zmiany w sieci Web w systemie iOS 11

System iOS 11 wprowadzono nową wersję przeglądarki sieci web Safari — Safari 11.0 — w tym zmiany aparatu WebKit i SafariServices. Ten przewodnik opisuje te zmiany.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` została wprowadzona w systemie iOS 9 jako opcję wyświetlania zawartości sieci web lub uwierzytelniania użytkowników z Twojej aplikacji. Więcej informacji na temat funkcji można znaleźć w [widoki sieci Web](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) przewodnik.

System iOS 11 wprowadził styl aktualizacji do kontrolera widoku przeglądarki Safari, przyznawanie użytkownikom, aby usprawnić środowisko między aplikacją i sieci web. Na przykład usunięcie zawiera teraz kontroler widoku działania przeglądarki w aplikacjach, zamiast mini przeglądarki Safari na pasku adresu. Można również dostosować schemat kolorów, aby dopasować się przy użyciu schematu kolorów aplikacji przez ustawienie `preferredBarTintColor` i `PreferredControlTintColor` właściwości:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

Poniższy fragment kodu powoduje wyświetlenie pasków purpurowy i białe, wyświetlane na poniższej ilustracji:

![Paski SFSafariViewController renderowane w purpurowy i kolor biały](web-images/image1.png)

Przycisk Odrzuć znajdujące się w kontroler widoku przeglądarki Safari można także zmienić przez ustawienie `DismissButtonStyle` właściwości albo `Done`, `Close`, lub `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Odrzuć zmiany tekstu przycisku](web-images/image2.png)

Ta wartość może zostać zmieniona podczas `SFSafariViewController` zostanie wyświetlony.


W zależności od zawartości, która jest wyświetlana w kontroler widoku przeglądarki Safari może być konieczne upewnić się, że paski menu nie Zwiń, gdy użytkownik przewija. Ta opcja jest włączona, ustawiając nową `BarCollapsedEnabled` właściwości `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Pasek zwijanie wyłączone](web-images/image3.png)

Apple wprowadziła również aktualizacje prywatności w kontroler widoku przeglądarki Safari w systemie iOS 11. Teraz przeglądanie danych, takich jak pliki cookie i lokalny magazyn istnieją tylko na poszczególnych aplikacji, a nie we wszystkich wystąpieniach kontrolera widoku przeglądarki Safari. Dzięki temu działań przeglądania użytkownika prywatnych w aplikacji.

Dodatkowe funkcje takie jak przeciągnij i upuść obsługę adresów URL i obsługa `window.open()` również zostały dodane do `SFSafariViewController` w systemie iOS 11. Można znaleźć więcej informacji na temat tych nowych funkcji w [dokumentacji SFSafariViewController firmy Apple](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).


## <a name="webkit"></a>Aparatu WebKit

`WKWebView` został wprowadzony w ramach aparatu WebKit w systemie iOS 8 jako sposób wyświetlania zawartości sieci web do użytkownika. Jest znacznie szersze niż `SFSafariViewController`, co pozwala na tworzenie własnych nawigacji i interfejsu użytkownika.

Apple wprowadziła trzech głównych ulepszenia `WKWebView` z systemem iOS 11: 

- Możliwość zarządzania plikami cookie
- Filtrowanie zawartości
- Ładowanie zasobów niestandardowych. 

Zarządzanie plików cookie odbywa się za pośrednictwem nowego [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) klasy, która pozwala na dodawanie i usuwanie plików cookie, aby uzyskać wszystkie pliki cookie przechowywane w WKWebView i obserwować plików cookie, zapisać zmiany.

Zawartość, filtrowanie, umożliwia zarządzanie typu zawartości, która będzie widoczna dla użytkowników, dzięki czemu możesz się upewnić, że jest przyjaznym bezpieczne, rodziny i, jeśli to konieczne, dostępne tylko do wybranej grupy użytkowników. Ten sposób jest implementowany przy użyciu nowego [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) klasy, podając pary wyzwalaczy i akcji w formacie JSON. Więcej informacji na temat tych wyzwalaczy i akcji można znaleźć w firmy Apple [zasady blokowania zawartości](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) przewodnik.

System iOS 11 umożliwia teraz dostosowywanie `WKWebView` przy użyciu zasobów niestandardowych ładowania dla zawartości sieci web. Ten sposób jest implementowany za pośrednictwem `IWKUrlSchemeHandler` interfejsu, co pozwala obsługiwać schematy adresów URL, które nie są natywne zestawu sieci Web. Ten interfejs jest uruchamianie i zatrzymywanie metodę, która musi zostać wdrożone:

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

Po zaimplementowaniu program obsługi, użyj go, aby ustawić `SetUrlSchemeHandler` właściwość `WKWebViewConfiguration`. Następnie należy załadować adres URL elementu, który używa niestandardowego schematu:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

