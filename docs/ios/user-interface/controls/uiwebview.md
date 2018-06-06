---
title: Widoki sieci Web platformy Xamarin.iOS
description: W tym dokumencie opisano różne sposoby aplikacji platformy Xamarin.iOS można wyświetlać zawartość sieci web. Zawarto informacje UIWebView, WKWebView SFSafariViewController, Safari i zabezpieczeń transportu aplikacji.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f720eae68415ab9efe021e53c9da4875209cd221
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790499"
---
# <a name="web-views-in-xamarinios"></a>Widoki sieci Web platformy Xamarin.iOS

W okresie istnienia systemu iOS firmy Apple wydała na wiele sposobów dla deweloperów aplikacji włączenie funkcji widoku sieci web w swoich aplikacjach. Większość użytkowników korzystania z wbudowanych przeglądarki sieci web Safari na urządzeniu z systemem iOS i w związku z tym spodziewać, że funkcje widoku sieci web z innych aplikacji są zgodne z tym środowiskiem. Oczekiwane tego samego gestów do pracy, wydajność, aby być na par oraz funkcje takie same.

W tym artykule, przeanalizujemy każdego z trzech widoków sieci Web dostarczonymi przez firmę Apple: `UIWebView`, `WKWebview`, i `SFSafariViewController`, ich podobieństwa i różnic oraz sposobu ich użycia. 

iOS 11 wprowadzono nowe zmiany zarówno `WKWebView` i `SFSafariViewController`. Aby uzyskać więcej informacji o tych zobacz [sieci Web zmiany w przewodniku iOS 11](~/ios/platform/introduction-to-ios11/web.md) przewodnik.

## <a name="uiwebview"></a>UIWebView

`UIWebView` jest firmy Apple starszych sposób dostarczania zawartości sieci web w aplikacji. Została wydana w systemie iOS 2.0 i jest przestarzała w wersji 8.0 lub nowszej.

Jeśli planujesz obsługuje wersje systemu iOS starszych niż 8.0, trzeba będzie użyć `UIWebView`. Ze względu na fakt, że `UIWebView` mniej zoptymalizowana pod kątem wydajności niż rozwiązań alternatywnych, zaleca się że należy sprawdzić wersji systemu iOS. Jeśli go 8.0 lub nowszym, za pomocą opcji opisane poniżej utworzy lepsze środowisko pracy użytkownika.
 
Aby dodać UIWebView do aplikacji platformy Xamarin.iOS, użyj następującego kodu:
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

Daje to poniższe widoku sieci web:

[![](uiwebview-images/webview.png "Efekt ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

Aby uzyskać więcej informacji na temat używania `UIWebView`, dotyczą następujących przepisami:


- [Ładowanie strony sieci Web](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_a_web_page/)
- [Ładowanie zawartości lokalnej](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_local_content/)
- [Ładowanie dokumentów sieci Web](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_non-web_documents/)

## <a name="wkwebview"></a>WKWebView

`WKWebView` został wprowadzony w iOS 8 Zezwalanie aplikacji deweloperom implementuje interfejs podobny do przenośnych Safari przeglądania sieci web. Jest to z powodu, w części fakt który `WKWebView` używa aparatu Nitro Javascript, tego samego silnika używane przez przenośnych Safari. `WKWebView` zawsze należy używać over UIWebView były możliwe ze względu na [zwiększenie wydajności](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), wbudowane gesty, przyjazny dla użytkownika i łatwość interakcji między strony sieci web i aplikacji.
  
`WKWebView` można dodać do aplikacji w niemal identyczny sposób UIWebView, jednak Deweloper masz większą kontrolę nad interfejsu użytkownika/UX i funkcje. Tworzenie i wyświetlanie obiektu widoku sieci web spowoduje wyświetlenie żądanej strony, jednak można kontrolować sposób prezentowania widoku, jak można przejść użytkownik i jak użytkownik opuszcza widoku.  

Poniższy kod może służyć do uruchamiania `WKWebView` w aplikacji platformy Xamarin.iOS:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

Daje to poniższe widoku sieci web:

[![](uiwebview-images/wkwebview.png "Przykładowy widok sieci web bez ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

Należy zauważyć, że `WKWebView` znajduje się w przestrzeni nazw WebKit, dlatego trzeba dodać, to użycie dyrektywy do góry klasy.

`WKWebView` można również w ramach Xamarin.Mac aplikacji, a w związku z tym warto rozważyć użycie go w przypadku tworzenia aplikacji systemu Mac/iOS między platformami.

[Obsługi JavaScript alerty](https://developer.xamarin.com/recipes/ios/content_controls/web_view/handle_javascript_alerts/) przepisu zawiera także informacje o użyciu WKWebView Javascript

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` najnowsze sposób dostarczanie zawartości sieci web z aplikacji i jest dostępny w systemie iOS 9 i nowszych. W odróżnieniu od `UIWebView` lub `WKWebView`, `SFSafariViewController` kontroler widoku i dlatego nie można używać z innymi widokami. Powinni przedstawić `SFSafariViewController` jako nowego kontrolera widoku w taki sam sposób czy przedstawienie dowolnego kontrolera widoku.
 
 `SFSafariViewController` jest zasadniczo "mini safari" można ją osadzić w aplikacji. Podobnie jak WKWebView go używa tego samego silnika Javascript Nitro, ale również zapewnia szeroką gamę dodatkowych funkcji Safari, takie jak automatyczne uzupełnianie, Czytelnik i możliwość udostępnianie plików cookie i danych mobilnych Safari. Interakcja między użytkownikiem i `SFSafariViewController` nie jest dostępna dla aplikacji. Aplikacja nie mają dostęp do funkcji Safari domyślne.
 
Ponadto domyślnie implementuje **gotowe** przycisk umożliwiający użytkownikowi łatwy powrót do aplikacji i do przodu i Wstecz przycisków nawigacji, umożliwiając użytkownikowi nawigowania w stosie stron sieci web. Ponadto zapewnia także użytkownika przy użyciu adresu paska nadanie im spokój umysłu, które znajdują się w oczekiwanej strony sieci web. Na pasku adresu nie zezwala użytkownikom na zmianę adresu url. 

Tych implementacji nie można zmienić, dlatego `SFSafariViewController` nadaje się do użycia jako domyślnej przeglądarki, jeśli chce przedstawić strony sieci Web bez potrzeby dostosowywania aplikacji.

Poniższy kod może służyć do uruchamiania `SFSafariViewController` w aplikacji platformy Xamarin.iOS:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Daje to poniższe widoku sieci web:

[![](uiwebview-images/sfsafariviewcontroller.png "Przykładowy widok sieci web z SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Istnieje również możliwość otwarcia aplikacji mobilnej Safari z aplikacji, za pomocą poniższy kod:

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

Daje to poniższe widoku sieci web:

[![](uiwebview-images/safari.png "Strony sieci web w programie Safari")](uiwebview-images/safari.png#lightbox)

Nawigowanie po użytkowników aplikacji Safari ogólnie zawsze należy unikać. Większość użytkowników nie będzie oczekiwać nawigacji poza aplikacji, więc Jeśli opuścisz aplikacji użytkownikom może nigdy nie należy zwracać, zasadniczo skasowanie zaangażowania.

ulepszenia systemu iOS 9 Zezwalaj użytkownikowi na łatwy powrót do aplikacji za pomocą przycisku Wstecz, który znajduje się w lewym górnym rogu strony Safari.

## <a name="app-transport-security"></a>Zabezpieczenia transportu aplikacji

Zabezpieczenia transportu aplikacji, lub *ATS* została wprowadzona przez firmę Apple w systemie iOS 9, aby upewnić się, że cała komunikacja z Internetem odpowiadają bezpieczne połączenie najlepszych rozwiązań.

Aby uzyskać więcej informacji dotyczących ATS, łącznie ze sposobem ją wdrożyć w aplikacji, zapoznaj się [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md) przewodnik.

## <a name="related-links"></a>Linki pokrewne

- [WebViews (przykład)](https://developer.xamarin.com/samples/monotouch/WebView/)
