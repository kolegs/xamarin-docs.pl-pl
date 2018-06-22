---
title: Widok sieci Web
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2baf7dae71ce7607c629b570ad25f477dec66c17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765311"
---
# <a name="web-view"></a>Widok sieci Web

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) Pozwala utworzyć własne okna do wyświetlania stron sieci web (lub nawet utworzyć pełną przeglądarki). W tym samouczku, utworzysz prostą [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) który można wyświetlać i przejdź do strony sieci web.

Utwórz nowy projekt o nazwie **HelloWebView**.

Otwórz **Resources/Layout/Main.axml** i Wstaw następujący:

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Ponieważ ta aplikacja uzyska dostęp do Internetu, należy dodać odpowiednie uprawnienia do Android pliku manifestu. Otwórz właściwości projektu, aby określić uprawnienia aplikacji wymaga do działania. Włącz `INTERNET` uprawnienia, jak pokazano poniżej:

![Ustawianie uprawnień internetowych w manifestu systemu Android](web-view-images/01-set-internet-permissions.png)

Teraz otworzyć **MainActivity.cs** i Dodaj using dyrektywy dla Webkit:

```csharp
using Android.Webkit;
```

W górnej części `MainActivity` klasy, Zadeklaruj [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) obiektu:

```csharp
WebView web_view;
```

Gdy **WebView** jest żądanie załadowania adresu URL, jego będzie domyślnie delegowane żądanie do domyślnej przeglądarki. Aby **widoku sieci Web** załadować adres URL (zamiast domyślnej przeglądarki), należy najpierw podklasy `Android.Webkit.WebViewClient` i zastąpić `ShouldOverriderUrlLoading` — metoda. Wystąpienie tego niestandardowych `WebViewClient` został dostarczony do `WebView`. W tym celu należy dodać następujące zagnieżdżone `HelloWebViewClient` klasy wewnątrz `MainActivity`:

```csharp
public class HelloWebViewClient : WebViewClient
{
    public override bool ShouldOverrideUrlLoading (WebView view, string url)
    {
        view.LoadUrl(url);
        return false;
    }
}
```

Gdy `ShouldOverrideUrlLoading` zwraca `false`, sygnały go w systemie Android który bieżącego `WebView` wystąpienie obsługi żądania, a nie są wymagane nie dalsze akcje. 

Jeśli ma być przeznaczona dla poziom interfejsu API, 24 lub nowszym, użyj przeciążenia `ShouldOverrideUrlLoading` pobierającej `IWebResourceRequest` jako drugi argument, a nie `string`:

```csharp
public class HelloWebViewClient : WebViewClient
{
    // For API level 24 and later
    public override bool ShouldOverrideUrlLoading (WebView view, IWebResourceRequest request)
    {
        view.LoadUrl(request.Url.ToString());
        return false;
    }
}
```

Następnie należy użyć poniższego kodu do [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) metody:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    web_view = FindViewById<WebView> (Resource.Id.webview);
    web_view.Settings.JavaScriptEnabled = true;
    web_view.SetWebViewClient(new HelloWebViewClient());
    web_view.LoadUrl ("https://www.xamarin.com/university");
}
```

Inicjuje to element członkowski [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) z elementem z [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) układ i umożliwia JavaScript dla [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) z [ `JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (zobacz [wywołać C\# z poziomu języka JavaScript](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript) przepisu informacji o sposobie wywołania C\# funkcji z kodu JavaScript). Na koniec początkowej strony sieci web jest ładowany z [ `LoadUrl(String)` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl).

Skompiluj i uruchom aplikację. Jak pokazano na poniższym zrzucie ekranu powinny być widoczne aplikacji viewer prostą stronę sieci web:

[![Przykład wyświetlania widoku sieci Web aplikacji](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

Do obsługi **ponownie** klucza naciśnięcie przycisku, dodaj następującą instrukcję using:

```csharp
using Android.Views;
```

Następnie dodaj następującą metodę wewnątrz `HelloWebView` działania:

```csharp
public override bool OnKeyDown (Android.Views.Keycode keyCode, Android.Views.KeyEvent e)
{
    if (keyCode == Keycode.Back && web_view.CanGoBack ())
    {
        web_view.GoBack ();
        return true;
    }
    return base.OnKeyDown (keyCode, e);
}
```

To [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent)) przy każdym naciśnięciu przycisku, gdy działanie zostanie wywołana metoda wywołania zwrotnego. Warunek wewnątrz używa [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) do sprawdzenia, czy naciśnięty klawisz jest **ponownie** przycisk oraz tego, czy [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) jest w rzeczywistości stanie Przechodzenie wstecz (Jeśli w przeszłości). Jeśli oba warunki są spełnione, a następnie [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/) wywoływana jest metoda, która spowoduje przejście wstecz jeden krok w [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) historii. Zwracanie `true` wskazuje, że zdarzenie zostało obsłużone. Jeśli ten warunek nie jest spełniony, zdarzenia są wysyłane do systemu.

Uruchom ponownie aplikację. Teraz można skorzystaj z linków do nawigowania wstecz w historii strony:

[![Przykład zrzuty ekranu przycisku Wstecz w akcji](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*Części tej stronie są zmiany na podstawie pracy utworzone i udostępnione przez Android Otwórz projekt źródłowy i używane zgodnie z warunki opisane w*
[*Creative Commons 2.5 autorstwa licencji* ](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>Linki pokrewne

- [Wywołanie C# z poziomu języka JavaScript](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
