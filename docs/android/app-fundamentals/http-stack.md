---
title: Stos HttpClient i selektor implementacji protokołów SSL/TLS dla systemu Android
description: Selektory HttpClient stosu i implementacji protokołów SSL/TLS określają HttpClient i SSL/TLS implementację, które będą używane przez aplikacje platformy Xamarin.Android.
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/20/2018
ms.openlocfilehash: 765c51346ac63a00838fec52bde87b38091e2dd9
ms.sourcegitcommit: a4c2a63ba76b839cda99e4474e7ab46fe307cd39
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34689477"
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>Stos HttpClient i selektor implementacji protokołów SSL/TLS dla systemu Android

Selektory HttpClient stosu i implementacji protokołów SSL/TLS określają HttpClient i SSL/TLS implementację, które będą używane przez aplikacje platformy Xamarin.Android.

Projekty musi odwoływać się **System.Net.Http** zestawu.

> [!WARNING]
> **Kwietnia, 2018** — ze względu na wyższy poziom bezpieczeństwa wymagania, w tym zgodności PCI główne dostawców w chmurze i serwery sieci web powinny zaprzestać obsługi protokołu TLS w wersji wcześniejszej niż 1.2.  Projekty platformy Xamarin utworzone w poprzednich wersjach programu Visual Studio domyślnie używać starszych wersji protokołu TLS.
>
> W celu zapewnienia aplikacji kontynuować pracę z tych serwerów i usług, **należy zaktualizować swoje projekty Xamarin z `Android HttpClient` i `Native TLS 1.2` ustawienia pokazano poniżej, następnie ponowne utworzenie i ponownie wdróż aplikacje** do użytkownika Użytkownicy.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Konfiguracja platformy Xamarin.Android HttpClient znajduje się w **opcje projektu > Opcje Android**, następnie kliknij przycisk **zaawansowane opcje** przycisku.

Poniżej przedstawiono zalecane ustawienia dla protokołu TLS 1.2 pomocy technicznej:

[![Android opcje programu Visual Studio](http-stack-images/android-win-sml.png)](http-stack-images/android-win.png#lightbox)


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Konfiguracja platformy Xamarin.Android HttpClient znajduje się w **opcje projektu > kompilacji > kompilacji Android** ustawienia i kliknij pozycję **ogólne** kartę.

Poniżej przedstawiono zalecane ustawienia dla protokołu TLS 1.2 pomocy technicznej:

[![Visual Studio for Mac opcje systemu Android](http-stack-images/android-mac-sml.png)](http-stack-images/android-mac.png#lightbox)

-----

## <a name="alternative-configuration-options"></a>Opcje konfiguracji alternatywnej

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler jest nowy program obsługi, który deleguje do kodu macierzystego języka Java/OS zamiast wykonania wszystko w kodzie zarządzanym.
**Jest to zalecana opcja.**

#### <a name="pros"></a>Specjaliści

- Użyj natywnego interfejsu API lepszą wydajność i mniejszego rozmiaru pliku wykonywalnego.
- Obsługę najnowszych standardów, np. TLS 1.2.

#### <a name="cons"></a>Cons

- Wymaga systemu Android 5.0 lub nowszej.
- Niektóre funkcje HttpClient/opcje nie są dostępne.

### <a name="managed-httpclienthandler"></a>Zarządzane (HttpClientHandler)

Zarządzanego programu obsługi jest w pełni zarządzanego programu obsługi HttpClient dostarczanego z poprzednich wersji platformy Xamarin.Android.

#### <a name="pros"></a>Specjaliści

- Jest najbardziej zgodna (funkcje) z MS .NET i starsze wersje platformy Xamarin.

#### <a name="cons"></a>Cons

- Nie jest on pełni zintegrowany z systemem operacyjnym (np.) tylko protokołu TLS 1.0).
- Jest zwykle znacznie wolniejsze (np.) szyfrowanie) niż natywnego interfejsu API.
- Wymaga to więcej kodu zarządzanego Tworzenie dużych aplikacji.



### <a name="choosing-a-handler"></a>Wybieranie obsługi

Wybór między `AndroidClientHandler` i `HttpClientHandler` zależy od wymagań aplikacji. `AndroidClientHandler` Zaleca się najbardziej aktualne obsługi zabezpieczeń, np.

-   Wymagane jest, że obsługa protokołu TLS 1.2 +.
-   Aplikacja jest przeznaczonych dla systemu Android 5.0 (interfejs API 21) lub nowszym.
-   Należy protokołu TLS 1.2 + obsługę `HttpClient`.
-   Nie ma potrzeby obsługę protokołu TLS 1.2 + `WebClient`.

`HttpClientHandler` jest to dobre rozwiązanie, jeśli potrzebujesz protokołu TLS 1.2 + obsługują, ale musi obsługiwać systemu android w wersjach wcześniejszych niż Android 5.0. Jest również dobrym rozwiązaniem, jeśli potrzebujesz protokołu TLS 1.2 + obsługę `WebClient`.

Począwszy od platformy Xamarin.Android 8.3, `HttpClientHandler` wartość domyślna to wytaczania SSL (`btls`) jako podstawowy dostawca protokołu TLS. Dostawca wytaczania SSL TLS oferuje następujące korzyści:

-   Obsługuje ona protokołu TLS 1.2 +.
-   Obsługuje ona wszystkie wersje systemu Android.
-   Zapewnia Obsługa protokołu TLS 1.2 + zarówno `HttpClient` i `WebClient`.

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

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Pole kombi implementacji protokołu TLS/SSL w programie Visual Studio](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

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

- `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; Tej zmiennej środowiskowej deklaruje domyślnie `HttpMessageHandler` używanego przez aplikację. Na przykład:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

- `XA_TLS_PROVIDER` &ndash; Tej zmiennej środowiskowej zostanie zadeklarować biblioteki TLS, które będą używane, albo `btls`, `legacy`, lub `default` (która jest taka sama jak pominięcie tej zmiennej):

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

Tej zmiennej środowiskowej została ustawiona przez dodanie _pliku środowiska_ do projektu. Środowisko plik jest plikiem zwykły tekst sformatowany Unix z akcją kompilacji **AndroidEnvironment**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Zrzut ekranu przedstawiający AndroidEnvironment akcji kompilacji w programie Visual Studio.](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Zrzut ekranu przedstawiający AndroidEnvironment kompilacji akcji w programie Visual Studio dla komputerów Mac.](http-stack-images/tls03-xs.png)

-----

Zobacz [środowiska platformy Xamarin.Android](~/android/deploy-test/environment.md) przewodnika, aby uzyskać więcej informacji o zmiennych środowiskowych i platformy Xamarin.Android.


## <a name="related-links"></a>Linki pokrewne

- [Protokół Transport Layer Security (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
