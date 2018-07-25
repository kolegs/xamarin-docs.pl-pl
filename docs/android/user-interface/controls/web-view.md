---
title: Widok sieci Web
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8d7b0e1abc8eb11bf812a111764b9cccfb41e041
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241178"
---
# <a name="web-view"></a>Widok sieci Web

[`WebView`](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) Umożliwia utworzenie okna w celu wyświetlania stron sieci web (lub nawet utworzyć pełną przeglądarki). W tym samouczku utworzymy prostą [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) , można przeglądać i nawigować stron sieci web.

Utwórz nowy projekt o nazwie **HelloWebView**.

Otwórz **Resources/Layout/Main.axml** i Wstaw następujący:

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Ponieważ ta aplikacja będzie dostęp do Internetu, należy dodać odpowiednie uprawnienia do programu Android pliku manifestu. Otwórz właściwości projektu, aby określić uprawnienia, które aplikacja wymaga do działania. Włącz `INTERNET` uprawnienie, jak pokazano poniżej:

![Ustawianie uprawnień internetowych w manifestu systemu Android](web-view-images/01-set-internet-permissions.png)

Teraz Otwórz **MainActivity.cs** i dodawanie przy użyciu dyrektywy dla aparatu Webkit:

```csharp
using Android.Webkit;
```

W górnej części `MainActivity` klasy, Zadeklaruj [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) obiektu:

```csharp
WebView web_view;
```

Gdy **WebView** jest pytania, aby załadować adresu URL, go domyślnie oddeleguje żądanie domyślnej przeglądarki. Zapewnienie **WebView** ładowania adresu URL (zamiast domyślnej przeglądarki), należy najpierw podklasy `Android.Webkit.WebViewClient` i zastąpić `ShouldOverriderUrlLoading` metody. Wystąpienie tym niestandardowe `WebViewClient` jest dostarczany w celu `WebView`. Aby to zrobić, Dodaj następujące zagnieżdżone `HelloWebViewClient` klasę `MainActivity`:

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

Gdy `ShouldOverrideUrlLoading` zwraca `false`, sygnalizuje systemu Android, bieżący `WebView` wystąpienia obsługi żądania, i są wymagane żadne dalsze akcje. 

Jeśli poziom interfejsu API, 24 lub nowszym, użyj przeciążenia `ShouldOverrideUrlLoading` przyjmującej `IWebResourceRequest` dla drugiego argumentu zamiast `string`:

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

Następnie użyj poniższego kodu, aby uzyskać [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) metody:

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

Inicjuje to element członkowski [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) plikiem z [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/) układ i umożliwia JavaScript dla [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) z [ `JavaScriptEnabled` ](https://developer.xamarin.com/api/property/Android.Webkit.WebSettings.JavaScriptEnabled/) 
 `= true` (zobacz [wywołać C\# poziomu języka JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript) przepisu, aby uzyskać informacje o tym, jak wywoływać C\# funkcji z języka JavaScript). Na koniec początkowej strony sieci web jest ładowany z [ `LoadUrl(String)` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/%2fM%2fLoadUrl).

Skompiluj i uruchom aplikację. Powinny zostać wyświetlone aplikację przeglądarki prostą stronę sieci web, jak pokazano na poniższym zrzucie ekranu:

[![Przykład aplikacji wyświetlanie element WebView](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

Do obsługi **ponownie** naciśnięcie klawisza przycisk, dodaj następującą instrukcję using:

```csharp
using Android.Views;
```

Następnie dodaj następujące metody w ramach `HelloWebView` działania:

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

To [ `OnKeyDown(int, KeyEvent)` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnKeyDown/(Android.Views.Keycode%2cAndroid.Views.KeyEvent)) metody wywołania zwrotnego będzie wywoływana zawsze wtedy, gdy przycisk jest wciśnięty, gdy działanie jest uruchomiona. Warunek wewnątrz używa [ `KeyEvent` ](https://developer.xamarin.com/api/type/Android.Views.KeyEvent/) do sprawdzenia, czy klawisz naciśnięty jest **ponownie** przycisku oraz tego, czy [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) może faktycznie Przejdź wstecz (jeśli ma on historii). Jeśli obie są prawdziwe, a następnie [ `GoBack()` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.GoBack/) wywoływana jest metoda, która spowoduje przejście wstecz jeden krok [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) historii. Zwracanie `true` wskazuje obsługiwania zdarzenia. Jeśli ten warunek nie jest spełniony, zdarzenia są wysyłane do systemu.

Uruchom ponownie aplikację. Teraz można skorzystaj z linków i poruszanie się wstecz w historii strony:

[![Przykładowe zrzuty ekranów przycisk Wstecz w akcji](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)


*Części tej strony są modyfikacje oparte na pracy, utworzone i udostępnione przez Android Open Source Project i używane zgodnie z postanowieniami*
[*2.5 autorstwa licencjiCreativeCommons* ](http://creativecommons.org/licenses/by/2.5/).


## <a name="related-links"></a>Linki pokrewne

- [Wywoływanie C# z języka JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView)
- [KeyEvent](https://developer.xamarin.com/api/type/Android.Webkit.WebView/Client)
