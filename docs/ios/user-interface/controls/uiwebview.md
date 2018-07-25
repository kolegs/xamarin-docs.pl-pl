---
title: Widoki sieci Web w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano różne sposoby, w aplikacji platformy Xamarin.iOS mogą wyświetlać zawartość sieci web. Omówiono w nim UIWebView, WKWebView, SFSafariViewController, Safari i zabezpieczenia transportu aplikacji.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 56b0f0e910cc95ca50d1e5e460ce71a1c8f669a2
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242043"
---
# <a name="web-views-in-xamarinios"></a>Widoki sieci Web w rozszerzeniu Xamarin.iOS

W okresie istnienia systemu iOS firmy Apple został wydany na wiele sposobów dla deweloperów aplikacji w celu włączenia funkcji widoku sieci web w swoich aplikacjach. Większość użytkowników korzystanie z wbudowanej przeglądarki Safari w sieci web, na urządzeniu z systemem iOS, a w związku z tym spodziewać, że funkcje widoku sieci web z innych aplikacji są spójne z tego środowiska. Oczekują one ten sam gestów do pracy i wydajności do włączenia par oraz funkcje takie same.

W tym artykule, przeanalizujemy każdego z trzech widoki sieci Web dostarczonymi przez firmę Apple: `UIWebView`, `WKWebview`, i `SFSafariViewController`, ich podobieństwa i różnice i jak mogą być używane. 

System iOS 11 wprowadzono nowe zmiany do obu `WKWebView` i `SFSafariViewController`. Aby uzyskać więcej informacji na ten temat, zobacz [Web zmiany w podręczniku systemu iOS 11](~/ios/platform/introduction-to-ios11/web.md) przewodnik.

## <a name="uiwebview"></a>UIWebView

`UIWebView` jest firmy Apple starszy sposób udostępniania zawartości sieci web w aplikacji. Został wydany w systemie iOS w wersji 2.0 i jest przestarzała począwszy od 8.0.

Jeśli planujesz starszych niż 8.0 obsługuje wersje systemu iOS będą musieli używać `UIWebView`. Ze względu na fakt, że `UIWebView` jest mniej zoptymalizowana pod kątem wydajności niż alternatyw, zaleca się należy sprawdzenie, czy wersja systemu IOS dla użytkownika. Jeśli go 8.0 lub nowszym, za pomocą opcji opisane poniżej spowoduje utworzenie lepsze środowisko użytkownika.
 
Aby dodać UIWebView do aplikacji platformy Xamarin.iOS, użyj następującego kodu:
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

To daje następujący widok sieci web:

[![](uiwebview-images/webview.png "Efekt ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

Aby uzyskać więcej informacji na temat korzystania z `UIWebView`, odnoszą się do następujących przepisy:


- [Ładowanie strony sieci Web](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [Ładowanie zawartości lokalnej](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [Ładowanie dokumentów sieci Web](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## <a name="wkwebview"></a>WKWebView

`WKWebView` wprowadzono w programie System iOS 8 umożliwiająca deweloperom aplikacji zaimplementować przeglądaniem podobny do mobilnych Safari interfejsu sieci web. To wynika, w części fakt, `WKWebView` korzysta z aparatu Nitro Javascript tego samego silnika, które są używane przez przeglądarkę Safari mobilnych. `WKWebView` zawsze należy używać trybu failover UIWebView były możliwe ze względu na [zwiększenie wydajności](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), wbudowane gesty, przyjazny dla użytkownika i łatwość interakcji między strony sieci web i aplikacji.
  
`WKWebView` można dodać do aplikacji w sposób niemal identyczne do UIWebView, jednak Deweloper masz większą kontrolę nad interfejs użytkownika i funkcje. Tworzenie i wyświetlanie obiektu widoku sieci web spowoduje wyświetlenie żądanej strony, jednak można kontrolować sposób prezentowania widoku, jak użytkownik może przejść i jak użytkownik opuszcza widoku.  

Poniższy kod może służyć do uruchamiania `WKWebView` w aplikacji platformy Xamarin.iOS:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

To daje następujący widok sieci web:

[![](uiwebview-images/wkwebview.png "Przykładowy widok sieci web bez ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

Ważne jest, aby pamiętać, że `WKWebView` znajduje się w przestrzeni nazw aparatu WebKit, więc trzeba dodać to użycie dyrektywy na górze klasy.

`WKWebView` można także w aplikacjach platformy Xamarin.Mac, a zatem warto rozważyć użycie go w przypadku tworzenia aplikacji Mac/systemu iOS dla wielu platform.

[Obsługi języka JavaScript alerty](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) przepisu zawiera także informacje na temat używania WKWebView za pomocą języka Javascript

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` jest najnowszą sposobem udostępnienia zawartości sieci web z aplikacji i jest dostępne w systemie iOS 9 i nowszych wersjach. W odróżnieniu od `UIWebView` lub `WKWebView`, `SFSafariViewController` jest kontroler widoku i dlatego nie może być używany z innymi widokami. Powinni przedstawić `SFSafariViewController` jako nowy kontroler widoku w taki sam sposób możesz przedstawiałoby wszystkimi kontrolerami widoku.
 
 `SFSafariViewController` jest zasadniczo "mini safari", można ją osadzić w aplikacji. Podobnie jak WKWebView go używa tego samego aparatu Javascript Nitro, ale także oferuje szeroką gamę dodatkowych funkcji Safari, takich jak automatyczne uzupełnianie, Czytelnik i możliwość udostępniania plików cookie i dane przy użyciu przenośnych przeglądarki Safari. Interakcje między użytkownikiem a `SFSafariViewController` nie jest dostępny do aplikacji. Aplikacja nie będzie dostępu do żadnych funkcji domyślnej przeglądarki Safari.
 
Ponadto domyślnie implementuje **gotowe** przycisku uprawnień umożliwiających użytkownikowi łatwy powrót do aplikacji, a kierunek przód- tył przycisków nawigacji, umożliwiając użytkownikowi przejdź przez stos stron sieci web. Ponadto zapewnia również użytkownika przy użyciu adresu paska nadanie im poczucie bezpieczeństwa, które znajdują się w oczekiwanej strony sieci web. Na pasku adresu nie zezwala użytkownikowi na zmianę adresu url. 

Tych implementacji nie można zmienić, więc `SFSafariViewController` doskonale nadaje się do użycia jako domyślnej przeglądarki, jeśli aplikacja chce, aby przedstawić strony sieci Web bez potrzeby dostosowywania.

Poniższy kod może służyć do uruchamiania `SFSafariViewController` w aplikacji platformy Xamarin.iOS:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

To daje następujący widok sieci web:

[![](uiwebview-images/sfsafariviewcontroller.png "Przykładowy widok sieci web za pomocą SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Istnieje również możliwość otworzyć aplikację mobilną Safari z poziomu aplikacji, za pomocą poniższego kodu:

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

To daje następujący widok sieci web:

[![](uiwebview-images/safari.png "Strony sieci web w przeglądarce Safari")](uiwebview-images/safari.png#lightbox)

Nawigacja użytkowników aplikacji do programu Safari ogólnie zawsze należy unikać. Większość użytkowników nie będzie oczekiwać nawigacji spoza aplikacji, więc jeśli przejdziesz do innej aplikacji, użytkownicy mogą nigdy nie należy zwracać, zasadniczo zabijanie zaangażowania.

ulepszenia systemu iOS 9 umożliwia użytkownikowi łatwy powrót do aplikacji za pomocą przycisku Wstecz, który znajduje się w lewym górnym rogu strony przeglądarki Safari.

## <a name="app-transport-security"></a>Zabezpieczenia transportu aplikacji

Zabezpieczenia transportu aplikacji, lub *ATS* została wprowadzona przez firmę Apple w systemie iOS 9, aby upewnić się, że cała komunikacja z Internetem odpowiadają bezpieczne połączenie najlepszych rozwiązań.

Aby uzyskać więcej informacji na temat ATS, jak wdrożyć ją w swojej aplikacji, w tym dotyczą [App Transport Security](~/ios/app-fundamentals/ats.md) przewodnik.

## <a name="related-links"></a>Linki pokrewne

- [Elementów Webview (przykład)](https://developer.xamarin.com/samples/monotouch/WebView/)
