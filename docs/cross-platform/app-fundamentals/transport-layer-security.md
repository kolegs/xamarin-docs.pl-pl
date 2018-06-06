---
title: Transport Layer Security (TLS) 1.2
description: W tym dokumencie opisano sposób włączania opcji protokołu TLS 1.2 dla platformy Xamarin.iOS, Xamarin.Android i Xamarin.Mac projektów. Pokazuje, jak to zrobić w Visual Studio 2017 i Visual Studio dla komputerów Mac.
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 6f27d7713f2fe6426fa28f268b8e97838893aa76
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781400"
---
# <a name="transport-layer-security-tls-12"></a>Transport Layer Security (TLS) 1.2

Za pomocą najnowszej wersji [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) ważne jest zapewnienie komunikacji sieciowej aplikacji są bezpieczne.

> [!WARNING]
> **Kwietnia, 2018** — ze względu na wyższy poziom bezpieczeństwa wymagania, w tym zgodności PCI główne dostawców w chmurze i serwery sieci web powinny zaprzestać obsługi protokołu TLS w wersji wcześniejszej niż 1.2.  Projekty platformy Xamarin utworzone w poprzednich wersjach programu Visual Studio domyślnie używać starszych wersji protokołu TLS.
>
> W celu zapewnienia aplikacji kontynuować pracę z tych serwerów i usług, **należy zaktualizować swoje projekty Xamarin, aby użyć poniższych ustawień, a następnie ponowne utworzenie i ponownie wdróż aplikacje** dla użytkowników.

Projekty musi odwoływać się **System.Net.Http** zestawu i skonfigurowany, jak pokazano poniżej.

## <a name="update-android-to-tls-12"></a>Aktualizacja dla systemu Android protokołu TLS 1.2

Aktualizacja **implementacji HttpClient** i **implementacji protokołów SSL/TLS** opcje, aby umożliwić zabezpieczeń protokołu TLS 1.2.

> [!NOTE]
> Wymaga systemu Android 5.0 lub nowszej.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Te ustawienia można znaleźć w **właściwości projektu > Opcje Android** , a następnie klikając **zaawansowane** przycisk:

[![Skonfiguruj HttpClient i TLS w programie Visual Studio](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Te ustawienia można znaleźć w **opcje projektu > kompilacji > Android kompilacji** karty:

[![Skonfiguruj HttpClient i TLS w programie Visual Studio dla komputerów Mac](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-ios-to-tls-12"></a>Aktualizacja dla systemu iOS do protokołu TLS 1.2

Aktualizacja **implementacji HttpClient** opcję, aby włączyć TSL 1.2 zabezpieczeń.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

To ustawienie znajduje się w **właściwości projektu > iOS kompilacji**:

[![Skonfiguruj HttpClient i TLS w programie Visual Studio](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

To ustawienie znajduje się w **opcje projektu > kompilacji > iOS kompilacji** karty:

[![Skonfiguruj HttpClient w programie Visual Studio dla komputerów Mac](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-macos-to-tls-12-in-visual-studio-for-mac"></a>Zaktualizuj system macOS do protokołu TLS 1.2 w programie Visual Studio dla komputerów Mac

Aktualizacja **implementacji HttpClient** opcji w **opcje projektu > kompilacji > kompilacji Mac** kartę, aby włączyć TSL 1.2 zabezpieczeń:

[![Skonfiguruj HttpClient w programie Visual Studio dla komputerów Mac](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

## <a name="alternative-configuration-options"></a>Opcje konfiguracji alternatywnej

W tej sekcji omówiono alternatywy dla protokołu TLS 1.2, obsługiwane konfiguracje przedstawionych powyżej.
Deweloperzy aplikacji tylko należy rozważyć te możliwości, jeśli rozumieją ryzykiem przy użyciu różnych poziomów obsługę protokołu TLS.

### <a name="httpclient-implementation"></a>Implementacja HttpClient

Deweloperów programu Xamarin zawsze były w stanie używać macierzystych klas sieci w ich kodu, jednak istnieje również opcja, która określa, które stos sieciowy jest używany przez `HttpClient` klasy. Zapewnia to znanych interfejs API .NET, który ma szybkość i zabezpieczeń zalety natywnych platformy.

Dostępne są następujące opcje:

- **Zarządzany stos** — funkcje sieciowe dostarczane Mono lub
- **Stos natywnego** — różne sieci interfejsów API dostarczonych przez podstawowej platformy (Android, iOS i macOS).

Zarządzany stos zapewnia najwyższy poziom zgodności z istniejącego kodu platformy .NET, jednak może trwać dłużej, a powoduje zwiększenie rozmiaru pliku wykonywalnego.

Natywne opcje może być szybsze i lepsze zabezpieczenia (w tym protokołu TLS 1.2), ale mogą nie zapewnić wszystkich funkcji i opcji `HttpClient` klasy.

### <a name="ssltls-implementation-android"></a>Implementacja protokołu SSL/TLS (Android)

Opcje projektu systemu android pozwalają również wybrać życie SSL/TLS do obsługi:

- **Mono/zarządzanych** — protokół TLS 1.1 w systemie Android
- **Natywny** — protokół TLS 1.2 w systemie Android.

Domyślnie nowe projekty Xamarin implementacji natywnej, który obsługuje protokół TLS 1.2, (co jest zalecane dla wszystkich projektów), jednak można przełączyć do kodu zarządzanego gdy wymagane ze względu na zgodność.

> [!IMPORTANT]
> **Mono/zarządzane** opcja została [usunięte z systemem iOS i Mac](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/) opcje projektu.
>
> Opcja natywnych jest zawsze używana w systemach iOS i Mac platform.

## <a name="platform-specific-details"></a>Szczegóły poszczególnych platform

Podsumowanie powyżej opisano ustawienia poziomu projektu implementacji HttpClient i SSL/TLS w projektach Xamarin. Implementacja HttpClient można również ustawić dynamicznie w kodzie. Te przewodniki specyficzne dla platformy, aby uzyskać więcej informacji można znaleźć:

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS i Mac**](~/cross-platform/macios/http-stack.md)


## <a name="summary"></a>Podsumowanie

Aplikacje powinny używać zabezpieczeń TLS (Transport Layer) 1.2, gdy jest to możliwe.
Należy zaktualizować ustawienia w istniejących aplikacji zgodnie z instrukcjami w tym artykule, a następnie ponowne utworzenie i ponownie Wdróż na klientach.

## <a name="related-links"></a>Linki pokrewne

- [Zabezpieczenia transportu aplikacji](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Cykl Xamarin 9 (lutego 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Informacje o wersji mono 4.8 — Obsługa protokołu TLS 1.2](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler i WebRequestHandler wyjaśniono](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [Klient HTTP (przykład)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
