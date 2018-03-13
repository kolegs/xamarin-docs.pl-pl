---
title: "Stos HttpClient i selektor implementacji protokołów SSL/TLS dla systemu Android"
description: "Selektory HttpClient stosu i implementacji protokołów SSL/TLS określają HttpClient i SSL/TLS implementację, które będą używane przez aplikacje platformy Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: 5c63bda11a57c0f27efa1db6f0455b25f7da531b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>Stos HttpClient i selektor implementacji protokołów SSL/TLS dla systemu Android

_Selektory HttpClient stosu i implementacji protokołów SSL/TLS określają HttpClient i SSL/TLS implementację, które będą używane przez aplikacje platformy Xamarin.Android._

## <a name="overview"></a>Omówienie

Xamarin.Android zawiera dwa pola kombi określające ustawienia protokołu TLS dla aplikacji systemu Android. Jedno pole kombi będzie zidentyfikowanie `HttpMessageHandler` będą używane podczas tworzenia wystąpienia `HttpClient` obiektu, podczas gdy druga identyfikuje życie TLS będą używane przez żądania sieci web.

> [!NOTE]
> Projekty musi odwoływać się **System.Net.Http** zestawu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ustawienia dla stosu HttpClient znajdują się w opcji projektu w projekcie platformy Xamarin.Android. Polecenie **Android opcje** , a następnie kliknij na **zaawansowane opcje** przycisku. Spowoduje to wyświetlenie **zaawansowane opcje Android** okna dialogowego, który ma pola kombi dwa, jeden dla implementacji HttpClient i jeden dla implementacji protokołów SSL/TLS:


[![Android opcje programu Visual Studio](http-stack-images/tls07-vs-sml.png)](http-stack-images/tls07-vs.png#lightbox)

## <a name="httpclient-stack-selector"></a>Selektor HttpClient stosu

Ta opcja projektu określa, które `HttpMessageHandler` implementacji będą używane przy każdym `HttpClient` wystąpienie obiektu. Domyślnie jest to zarządzanej `HttpClientHandler`.

[![Pole kombi implementacji HttpClient systemu android w programie Visual Studio](http-stack-images/tls04-vs-sml.png)](http-stack-images/tls04-vs.png#lightbox) 


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Ustawienia dla stosu HttpClient znajdują się w opcji projektu w projekcie platformy Xamarin.Android. Polecenie **kompilacji > kompilacji systemu Android** ustawienia i kliknij pozycję **ogólne** karty:

[![Visual Studio for Mac opcje systemu Android](http-stack-images/tls07-xs-sml.png)](http-stack-images/tls07-xs.png#lightbox)

## <a name="httpclient-stack-selector"></a>Selektor HttpClient stosu

Ta opcja projektu określa, które `HttpMessageHandler` implementacji będą używane przy każdym `HttpClient` wystąpienie obiektu. Domyślnie jest to zarządzanej `HttpClientHandler`.

![Pole kombi implementacji HttpClient systemu android w programie Visual Studio dla komputerów Mac](http-stack-images/tls04-xs.png )

-----

### <a name="managed-httpclienthandler"></a>Zarządzane (HttpClientHandler)

Zarządzanego programu obsługi jest w pełni zarządzanego programu obsługi HttpClient dostarczanego z poprzednich wersji platformy Xamarin.Android.

#### <a name="pros"></a>Zalety:

- Jest najbardziej zgodna (funkcje) z MS .NET i starsze wersje platformy Xamarin.

#### <a name="cons"></a>Zalety:

- Nie jest on pełni zintegrowany z systemem operacyjnym (np.) tylko protokołu TLS 1.0).
- Jest zwykle znacznie wolniejsze (np.) szyfrowanie) niż natywnego interfejsu API.
- Wymaga to więcej kodu zarządzanego Tworzenie dużych aplikacji.

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler jest nowy program obsługi, który deleguje do kodu macierzystego języka Java/OS zamiast wykonania wszystko w kodzie zarządzanym.

#### <a name="pros"></a>Zalety:

- Użyj natywnego interfejsu API lepszą wydajność i mniejszego rozmiaru pliku wykonywalnego.
- Obsługę najnowszych standardów, np. TLS 1.2.

#### <a name="cons"></a>Zalety:

- Wymaga systemu Android 5.0 lub nowszej.
- Niektóre funkcje HttpClient/opcje nie są dostępne.

### <a name="choosing-a-handler"></a>Wybieranie obsługi

Wybór między `AndroidClientHandler` i `HttpClientHandler` zależy od wymagań aplikacji. `AndroidClientHandler` jest dobrym rozwiązaniem, jeśli wszystkie następujące zastosowania:

-   Wymagane jest, że obsługa protokołu TLS 1.2 +.
-   Aplikacja jest przeznaczonych dla systemu Android 5.0 (interfejs API 21) lub nowszym.
-   Należy protokołu TLS 1.2 + obsługę `HttpClient`.
-   Nie ma potrzeby obsługę protokołu TLS 1.2 + `WebClient`.

`HttpClientHandler` jest to dobre rozwiązanie, jeśli potrzebujesz protokołu TLS 1.2 + obsługują, ale musi obsługiwać systemu android w wersjach wcześniejszych niż Android 5.0. Jest również dobrym rozwiązaniem, jeśli potrzebujesz protokołu TLS 1.2 + obsługę `WebClient`.

Począwszy od platformy Xamarin.Android 8.3, `HttpClientHandler` wartość domyślna to wytaczania SSL (`btls`) jako podstawowy dostawca protokołu TLS. Dostawca wytaczania SSL TLS oferuje następujące korzyści:

-   Obsługuje ona protokołu TLS 1.2.
-   Obsługuje ona wszystkie wersje systemu Android.
-   Zapewnia obsługę protokołu TLS 1.2 dla obu `HttpClient` i `WebClient`.

Przy użyciu wytaczania SSL jako podstawowy dostawca TLS wadą jest to, że może zwiększyć rozmiar wynikowy APK (dodaje dodatkowe rozmiar APK na obsługiwanych ABI około 1MB).

Począwszy od platformy Xamarin.Android 8.3, domyślny dostawca TLS jest wytaczania SSL (`btls`). Jeśli nie chcesz używać wytaczania protokołu SSL, można przywrócić historycznych zarządzaną implementację SSL przez ustawienie `$(AndroidTlsProvider)` właściwości `legacy` (Aby uzyskać więcej informacji na temat ustawienie właściwości kompilacji, zobacz [procesu kompilacji](~/android/deploy-test/building-apps/build-process.md)).


### <a name="programatically-using-androidclienthandler"></a>Programowo przy użyciu `AndroidClientHandler`

`Xamarin.Android.Net.AndroidClientHandler` Jest `HttpMessageHandler` implementacji specjalnie dla platformy Xamarin.Android.
Wystąpienia tej klasy będzie używać natywnego `java.net.URLConnection` wdrożenia dla wszystkich połączeń HTTP. Zapewni to teoretycznie wzrost wydajności HTTP i mniejszych APK.

Następujący fragment kodu przedstawiono przykładowy sposób jawnie dla pojedynczego wystąpienia `HttpClient` klasy:

```csharp
// Android 5.0 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
> Podstawowym urządzeniu z systemem Android muszą obsługiwać protokół TLS 1.2 (tj. System android 5.0 i nowszych)


## <a name="ssltls-implementation-build-option"></a>Opcja kompilacji implementacji protokołów SSL/TLS

Ta opcja projektu określa, jakie podstawowej biblioteki TLS będą używane przez wszystkie żądania sieci web, zarówno `HttpClient` i `WebRequest`. Domyślnie wybrany jest protokół TLS 1.2:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Pole kombi implementacji protokołu TLS/SSL w programie Visual Studio](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Pole kombi implementacji protokołu TLS/SSL w programie Visual Studio dla komputerów Mac](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png#lightbox)

-----

Na przykład:

```csharp
var client = new HttpClient();
```

Jeśli implementacja HttpClient została ustawiona w **zarządzane** i ustawiono implementacji protokołu TLS **natywnego protokołu TLS 1.2 +**, a następnie `client` obiektu automatycznie użyje zarządzanej `HttpClientHandler` i Protokołu TLS 1.2 (udostępniony przez bibliotekę BoringSSL) jego żądań HTTP.

Jednak jeśli **implementacji HttpClient** ustawiono `AndroidHttpClient`, następnie wszystkie `HttpClient` obiektów użyje podstawowej Klasa Java `java.net.URLConnection` i będą mieć wpływu na **implementacji protokołu TLS/SSL** wartość. `WebRequest` obiekty użyje biblioteki BoringSSL.

## <a name="other-ways-to-control-ssltls-configuration"></a>Inne sposoby kontrolowania konfiguracji SSL/TLS

Istnieją trzy sposoby czy aplikacji platformy Xamarin.Android można kontrolować ustawienia TLS:

1. Wybierz bibliotekę HttpClient implementacji i domyślne TLS w opcji projektu.
2. Programowo przy użyciu `Xamarin.Android.Net.AndroidClientHandler`.
3. Zadeklaruj zmienne środowiskowe (opcjonalnie).

Z trzech opcji Zalecanym podejściem jest użycie opcji projektu platformy Xamarin.Android w celu zadeklarowania domyślnie `HttpMessageHandler` i protokołu TLS dla całej aplikacji. Następnie, w razie potrzeby programowo wystąpienia `Xamarin.Android.Net.AndroidClientHandler` obiektów. Te opcje są opisane powyżej.

Trzecia opcja &ndash; korzystanie ze zmiennych środowiskowych &ndash; opisanej poniżej.

### <a name="declare-environment-variables"></a>Deklarowanie zmiennych środowiskowych

Istnieją dwie zmienne środowiskowe, które są powiązane z zastosowaniem protokołu TLS w Xamarin.Android:

-   `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; Tej zmiennej środowiskowej deklaruje domyślnie `HttpMessageHandler` używanego przez aplikację. Na przykład:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

-   `XA_TLS_PROVIDER` &ndash; Tej zmiennej środowiskowej zostanie zadeklarować biblioteki TLS, które będą używane, albo `btls`, `legacy`, lub `default` (która jest taka sama jak pominięcie tej zmiennej):

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

Tej zmiennej środowiskowej została ustawiona przez dodanie _pliku środowiska_ do projektu. Środowisko plik jest plikiem zwykły tekst sformatowany Unix z akcją kompilacji **AndroidEnvironment**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Zrzut ekranu przedstawiający AndroidEnvironment akcji kompilacji w programie Visual Studio.](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Zrzut ekranu przedstawiający AndroidEnvironment kompilacji akcji w programie Visual Studio dla komputerów Mac.](http-stack-images/tls03-xs.png)

-----

Zobacz [środowiska platformy Xamarin.Android](~/android/deploy-test/environment.md) przewodnika, aby uzyskać więcej informacji o zmiennych środowiskowych i platformy Xamarin.Android.


## <a name="related-links"></a>Linki pokrewne

- [Protokół Transport Layer Security (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
