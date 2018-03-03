---
title: Transport Layer Security (TLS)
description: "Włączanie obsługi protokołu TLS 1.2 dla projektów platformy Xamarin w systemach Android, iOS i Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/10/2017
ms.openlocfilehash: 5237ed35116e5f8983df579d0ab68363996fb06f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="transport-layer-security-tls"></a>Transport Layer Security (TLS)

_Włączanie obsługi protokołu TLS 1.2 dla projektów platformy Xamarin w systemach Android, iOS i Mac_

Za pomocą najnowszej wersji [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) ważne jest zapewnienie komunikacji sieciowej aplikacji są bezpieczne.

> [!NOTE]
> Xamarin zwalnia od [lutego 2017](https://releases.xamarin.com/stable-release-cycle-9/) domyślnie używają protokołu TLS 1.2 w nowych projektach.

Obsługa protokołu TLS 1.2 jest teraz dostępna w:

* Mono 4.8 (obejmuje [Obsługa protokołu TLS 1.2](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support))
* Xamarin.iOS
* Xamarin.Mac
* Xamarin.Android (wymaga systemu Android 5.0 lub nowszej)

Projekty musi odwoływać się **System.Net.Http** zestawu. 

## <a name="updating-to-tls-12"></a>Aktualizowanie protokołu TLS 1.2

W tej sekcji opisano niektóre opcje konfiguracji sieci w programie Xamarin projektów, więc można zaktualizować Twojego _istniejących_ aplikacji, aby móc korzystać z bardziej bezpiecznego protokołu.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Te ustawienia można znaleźć w **opcje projektu > Opcje Android** , a następnie klikając **zaawansowane** przycisk: 

[![Skonfiguruj HttpClient i TLS w programie Visual Studio](transport-layer-security-images/properties-vs-sml.png)](transport-layer-security-images/properties-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
Te ustawienia można znaleźć w **właściwości projektu > opcji kompilacji > Zaawansowane** karty:

[![Skonfiguruj HttpClient i TLS w Visual Studio i Xamarin Studio dla komputerów Mac](transport-layer-security-images/properties-xs-sml.png)](transport-layer-security-images/properties-xs.png)

-----


### <a name="httpclient-implementation"></a>Implementacja HttpClient

Deweloperów programu Xamarin zawsze były w stanie używać macierzystych klas sieci w ich kodu, jednak istnieje również opcja, która określa, które stos sieciowy jest używany przez `HttpClient` klasy. Zapewnia to znanych interfejs API .NET, który ma szybkość i zabezpieczeń zalety natywnych platformy.

Dostępne są następujące opcje:

- **Zarządzany stos** — funkcje sieciowe dostarczane Mono lub
- **Stos natywnego** — różne sieci interfejsów API dostarczonych przez podstawowej platformy (Android, iOS i macOS).

Zarządzany stos zapewnia najwyższy poziom zgodności z istniejącego kodu platformy .NET, jednak może trwać dłużej, a powoduje zwiększenie rozmiaru pliku wykonywalnego.

Natywne opcje może być szybsze i lepsze zabezpieczenia (w tym protokołu TLS 1.2), ale mogą nie zapewnić wszystkich funkcji i opcji `HttpClient` klasy.


### <a name="ssltls-implementation"></a>Implementacja protokołu SSL/TLS

Opcje projektu pozwalają również wybrać życie SSL/TLS do obsługi:

- **Mono/zarządzane** — protokół TLS 1.1 w systemie Android, TLS 1.0 w systemach iOS i macOS.
- **Natywny** — protokół TLS 1.2 na zarówno dla systemu Android, iOS i macOS.

Domyślnie nowe projekty Xamarin implementacji natywnej, który obsługuje protokół TLS 1.2, (co jest zalecane dla wszystkich projektów), jednak można przełączyć do kodu zarządzanego gdy wymagane ze względu na zgodność.

> [!IMPORTANT]
> **Mono/zarządzane** opcja zostanie usunięta w [przyszłych wydaniach](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/).
>
> Natywny opcja jest zalecana.

# <a name="platform-specific-details"></a>Szczegóły poszczególnych platform

Podsumowanie powyżej opisano ustawienia poziomu projektu implementacji HttpClient i SSL/TLS w projektach Xamarin. Implementacja HttpClient można również ustawić dynamicznie w kodzie, a w systemie iOS są dwa natywne opcje do wyboru.

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS i Mac**](~/cross-platform/macios/http-stack.md)


# <a name="summary"></a>Podsumowanie

Aplikacje powinny używać zabezpieczeń TLS (Transport Layer) 1.2, gdy jest to możliwe.
Nowe aplikacje teraz domyślnie ta konfiguracja, jednak może być konieczne zaktualizowanie ustawień w istniejących aplikacji zgodnie z instrukcjami w tym artykule.

## <a name="related-links"></a>Linki pokrewne

- [Zabezpieczenia transportu aplikacji](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Cykl Xamarin 9 (lutego 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Informacje o wersji mono 4.8 — Obsługa protokołu TLS 1.2](http://www.mono-project.com/docs/about-monohttps://developer.xamarin.com/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler i WebRequestHandler wyjaśniono](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/en-us/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [Klient HTTP (przykład)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
