---
title: Transport Layer Security (TLS) 1.2
description: W tym dokumencie opisano sposób włączania protokołu TLS 1.2 dla projektów platformy Xamarin.iOS, Xamarin.Android i platformy Xamarin.Mac. Pokazuje, jak to zrobić w programie Visual Studio 2017 i Visual Studio dla komputerów Mac.
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 88cbce6dbfee4e7aa1a0711d6da74f6f12abd4b7
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780643"
---
# <a name="transport-layer-security-tls-12"></a>Transport Layer Security (TLS) 1.2

Za pomocą najnowszej wersji [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) ważne jest, aby upewnić się, komunikacja sieciowa aplikacji są bezpieczne.

> [!WARNING]
> **Kwietnia 2018 r.** — ze względu na wyższy poziom bezpieczeństwa wymagania, w tym zgodności ze standardami PCI głównych dostawców rozwiązań w chmurze i serwery sieci web powinny polegająca na wyłączeniu obsługi protokołu TLS w wersji wcześniejszej niż 1.2.  Projekty Xamarin utworzone w poprzednich wersjach programu Visual Studio domyślnie używać starszych wersji protokołu TLS.
>
> Aby zapewnić aplikacji kontynuować pracę z tymi serwerami i usługami, **należy zaktualizować swoje projekty Xamarin, aby użyć poniższych ustawień, a następnie ponownie utwórz i Wdróż ponownie swoje aplikacje** dla użytkowników.

Projekty muszą odwoływać się do **System.Net.Http** zestawu i skonfigurowane, jak pokazano poniżej.

## <a name="update-xamarinandroid-to-tls-12"></a>Aktualizacja platformy Xamarin.Android protokołu TLS 1.2

Aktualizacja **Implementacja klasy HttpClient** i **implementacji protokołów SSL/TLS** opcji, aby włączyć zabezpieczenia protokołu TLS 1.2.

> [!NOTE]
> Wymaga systemu Android 5.0 lub nowszym.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Te ustawienia można znaleźć w **właściwości projektu > Opcje systemu Android** i klikając **zaawansowane** przycisku:

[![Konfigurowanie HttpClient i TLS w programie Visual Studio](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Te ustawienia można znaleźć w **opcje projektu > kompilacji > kompilacji dla systemu Android** karty:

[![Konfigurowanie HttpClient i TLS w programie Visual Studio dla komputerów Mac](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-xamarinios-to-tls-12"></a>Aktualizowanie interfejsu Xamarin.iOS protokołu TLS 1.2

Aktualizacja **Implementacja klasy HttpClient** opcję, aby włączyć zabezpieczenia TSL 1.2.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

To ustawienie można znaleźć w **właściwości projektu > kompilacja systemu iOS**:

[![Konfigurowanie HttpClient i TLS w programie Visual Studio](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

To ustawienie można znaleźć w **opcje projektu > kompilacji > kompilacja systemu iOS** karty:

[![Konfigurowanie HttpClient w programie Visual Studio dla komputerów Mac](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-xamarinmac-to-tls-12"></a>Aktualizacja platformy Xamarin.Mac protokołu TLS 1.2

W programie Visual Studio dla komputerów Mac, aby włączyć protokół TLS 1.2 w aplikacji platformy Xamarin.Mac, zaktualizuj **Implementacja klasy HttpClient** opcji **opcje projektu > kompilacji > kompilacji Mac.**:

[![Konfigurowanie HttpClient w programie Visual Studio dla komputerów Mac](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

> [!WARNING]
> Nadchodząca wersja platformy Xamarin.Mac 4.8 będzie obsługiwała tylko system macOS w wersji 10.9 lub wyższej.
> Poprzednie wersje platformy Xamarin.Mac obsługiwały system macOS w wersji 10.7 lub wyższej, ale starsze wersje systemu macOS nie posiadają odpowiedniej infrastruktury TLS do obsługi protokołu TLS 1.2. W przypadku systemu macOS 10.7 lub macOS 10.8 należy użyć platformy Xamarin.Mac w wersji 4.6 lub wcześniejszej.

## <a name="alternative-configuration-options"></a>Opcje konfiguracji alternatywnej

W tej sekcji omówiono alternatywy dla protokołu TLS 1.2, obsługiwane konfiguracje, przedstawionych powyżej.
Deweloperzy aplikacji należy rozważyć następujących alternatyw, tylko jeśli rozumieją ryzyko przy użyciu różnych poziomów obsługi protokołu TLS.

### <a name="httpclient-implementation"></a>Implementacja klasy HttpClient

Deweloperów platformy Xamarin zawsze były w stanie używać macierzystych klas sieciowych w kodzie, jednak istnieje również opcja, która określa, które stosu sieciowego jest używany przez `HttpClient` klasy. Dzięki temu znanego interfejsu API platformy .NET, zawierającej szybkość i zabezpieczeń zalety platformy natywnej.

Dostępne są następujące opcje:

- **Stos zarządzany** — funkcje sieciowe dostarczane przez narzędzie Mono lub
- **Natywne stosu** — różne sieć interfejsów API dostarczonych przez podstawowych platform (Android, iOS i macOS).

Stos zarządzany zapewnia najwyższy poziom zgodności z istniejącego kodu .NET, jednak może być niższa i powoduje zwiększenie rozmiaru pliku wykonywalnego.

Natywne opcje można szybciej i ma lepsze zabezpieczenia (w tym protokołu TLS 1.2), ale mogą nie zapewnić wszystkich funkcji i opcji `HttpClient` klasy.

### <a name="ssltls-implementation-android"></a>Implementacja protokołu SSL/TLS (Android)

Opcje projektu dla systemu android pozwalają również wybrać co do implementacji protokołów SSL/TLS do obsługi:

- **Zarządzane narzędzie mono** — protokół TLS 1.1 w systemie Android
- **Natywne** — protokół TLS 1.2 w systemie Android.

Nowe projekty Xamarin domyślne dla natywnych implementacji, który obsługuje protokół TLS 1.2, (co jest zalecane dla wszystkich projektów), jednak możesz przełączyć się ponownie do kodu zarządzanego gdy wymagane ze względu na zgodność.

> [!IMPORTANT]
> **Mono/zarządzane** opcja została [usunięte z systemami iOS i Mac](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/) opcje projektu.
>
> Opcja natywnych jest zawsze używana w systemach iOS i Mac platform.

## <a name="platform-specific-details"></a>Szczegóły specyficzne dla platformy

Podsumowanie powyżej opisano ustawienia na poziomie projektu Implementacja klasy HttpClient i protokoły SSL/TLS w projektach platformy Xamarin. Implementacja klasy HttpClient można również ustawić dynamicznie w kodzie. Zapoznaj się z tych wskazówek specyficznych dla platformy, aby uzyskać więcej informacji:

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS i Mac**](~/cross-platform/macios/http-stack.md)

## <a name="summary"></a>Podsumowanie

Aplikacje powinny używać zabezpieczeń TLS (Transport Layer) 1.2, tam gdzie to możliwe.
Należy zaktualizować ustawienia w istniejących aplikacji zgodnie z instrukcjami w tym artykule, a następnie stopniowo ponownie utwórz i Wdróż ponownie do klientów.

## <a name="related-links"></a>Linki pokrewne

- [Zabezpieczenia transportu aplikacji](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Cykl Xamarin 9 (lutego 2017 r.)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Informacje o wersji platformy mono 4.8 — Obsługa protokołu TLS 1.2](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [Protokołu BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient HttpClientHandler oraz wyjaśniono WebRequestHandler](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
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
