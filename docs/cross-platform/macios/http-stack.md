---
title: HttpClient i selektor implementacji protokołów SSL/TLS dla systemu iOS/macOS
description: Stos HttpClient i protokoły SSL/TLS selektor implementacji określa implementacji klasy HttpClient i protokoły SSL/TLS, który będzie używany przez aplikację na platformie Xamarin iOS, tvOS lub macOS.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: fd48c7148aadd8d156544113e2d719295294bf40
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780629"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>HttpClient i protokoły SSL/TLS selektor implementacji dla systemu iOS/macOS

**Selektor implementacji klasy HttpClient** dla platformy Xamarin.iOS, Xamarin.tvOS i Xamarin.Mac Określa, które `HttpClient` wdrożenia do użycia. Możesz przełączyć się do implementację, która korzysta z systemem iOS, tvOS lub macOS transportów natywnego (`NSUrlSession` lub `CFNetwork`, w zależności od systemu operacyjnego). Wzrost jest protokół TLS 1.2 — Pomoc techniczna, mniejsze pliki binarne i szybsze pobieranie; minusem jest to, że wymaga ona pętlę zdarzeń, należy uruchomić dla asynchronicznych operacji do wykonania.

Projekty muszą odwoływać się do **System.Net.Http** zestawu.

> [!WARNING]
> **Kwietnia 2018 r.** — ze względu na wyższy poziom bezpieczeństwa wymagania, w tym zgodności ze standardami PCI głównych dostawców rozwiązań w chmurze i serwery sieci web powinny polegająca na wyłączeniu obsługi protokołu TLS w wersji wcześniejszej niż 1.2.  Projekty Xamarin utworzone w poprzednich wersjach programu Visual Studio domyślnie używać starszych wersji protokołu TLS.
>
> W celu zapewnienia aplikacji kontynuować pracę z tymi serwerami i usługami, **należy zaktualizować swoje projekty Xamarin dzięki `NSUrlSession` ustawienie pokazano poniżej, następnie ponownie utwórz i Wdróż ponownie swoje aplikacje** dla użytkowników.

### <a name="selecting-an-httpclient-stack"></a>Wybieranie stos HttpClient

Aby dostosować `HttpClient` używany przez aplikację:

1. Kliknij dwukrotnie **Nazwa projektu** w **Eksploratora rozwiązań** aby otworzyć aplet Opcje projektu.
2. Przełącz się do **kompilacji** ustawienia projektu (na przykład **kompilacja systemu iOS** dla aplikacji platformy Xamarin.iOS).
3. Z **Implementacja klasy HttpClient** listy rozwijanej wybierz `HttpClient` wpisać jako jeden z następujących: **sesji NSUrlSession** (zalecane), **CFNetwork**, lub  **Zarządzane**.

[![Implementacja klasy HttpClient wybierać zarządzane CFNetwork i sesji NSUrlSession](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> Obsługa protokołu TLS 1.2 `NSUrlSession` opcja jest zalecana.

### <a name="nsurlsession"></a>NSUrlSession

`NSURLSession`— Na podstawie programu obsługi opiera się na natywnym `NSURLSession` framework dostępnych w systemie iOS 7 i nowszych. 
**To ustawienie zalecane.**

#### <a name="pros"></a>Specjaliści

- Aby uzyskać lepszą wydajność i mniejszy rozmiar pliku wykonywalnego używa natywnych interfejsów API.
- Wsparcie dla najnowszych standardów, takich jak protokół TLS 1.2.

#### <a name="cons"></a>Wady

- Wymaga systemu iOS 7 lub nowszy.
- Niektóre `HttpClient` funkcji/opcje nie są dostępne.

### <a name="cfnetwork"></a>CFNetwork

`CFNetwork`— Na podstawie programu obsługi opiera się na natywnym `CFNetwork` framework dostępnych w systemie iOS 6 i nowsze.

#### <a name="pros"></a>Specjaliści

- Aby uzyskać lepszą wydajność i mniejszy rozmiar pliku wykonywalnego używa natywnych interfejsów API.
- Obsługa nowszej standardów, takich jak protokół TLS 1.2.

#### <a name="cons"></a>Wady

- Wymaga systemu iOS 6 lub nowszej.
- Nie jest dostępna na systemu watchOS.
- Niektóre funkcje HttpClient/opcje nie są dostępne.

### <a name="managed"></a>Zarządzane

Obsługa zarządzanych jest w pełni zarządzanego programu obsługi HttpClient, dostarczanego z poprzedniej wersji programu Xamarin.

#### <a name="pros"></a>Specjaliści

- Ma ona najbardziej zgodne skonfigurowaną przy użyciu programu Microsoft .NET i starsze wersje platformy Xamarin.

#### <a name="cons"></a>Wady

- Go nie jest w pełni zintegrowany z systemów operacyjnych firmy Apple i jest ograniczona do protokołu TLS 1.0. Nie można nawiązać zabezpieczyć serwery sieci web lub usług w chmurze w przyszłości.
- On zazwyczaj znacznie wolniejsze w elementów, takich jak szyfrowanie niż natywnych interfejsów API.
- Wymaga ona więcej kodu zarządzanego, w związku z tym utworzenie dystrybucyjny większej aplikacji.

### <a name="programmatically-setting-the-httpmessagehandler"></a>Programowe Ustawianie klasa HttpMessageHandler

Oprócz skonfigurowania całego projektu pokazanych powyżej, można również utworzyć wystąpienie `HttpClient` i wstawiać żądaną `HttpMessageHandler` za pośrednictwem konstruktora, jak pokazano w tych fragmentach kodu:

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

Dzięki temu można użyć innego `HttpMessageHandler` z deklaracją w **opcje projektu** okna dialogowego.

## <a name="ssltls-implementation"></a>Implementacja protokołu SSL/TLS

Protokół SSL (Secure Socket Layer) i jego następca TLS (Transport Layer Security) oferują obsługę protokołu HTTP i innych połączeń sieciowych za pośrednictwem `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS lub dla platformy Xamarin.Mac `System.Net.Security.SslStream` implementacji wywoła firmy Apple natywnych implementacji protokołów SSL/TLS, zamiast korzystać z zarządzaną implementację udostępniane przez narzędzie Mono. Implementacja natywnego firmy Apple obsługuje protokół TLS 1.2.

> [!WARNING]
> Nadchodząca wersja platformy Xamarin.Mac 4.8 będzie obsługiwała tylko system macOS w wersji 10.9 lub wyższej.
> Poprzednie wersje platformy Xamarin.Mac obsługiwały system macOS w wersji 10.7 lub wyższej, ale starsze wersje systemu macOS nie posiadają odpowiedniej infrastruktury TLS do obsługi protokołu TLS 1.2. W przypadku systemu macOS 10.7 lub macOS 10.8 należy użyć platformy Xamarin.Mac w wersji 4.6 lub wcześniejszej.

## <a name="app-transport-security"></a>Zabezpieczenia transportu aplikacji

Firmy Apple _App Transport Security_ (ATS) wymusza nawiązywać bezpieczne połączenia między zasobami Internetu (na przykład serwer zaplecza aplikacji) i aplikacji. ATS gwarantuje, że cała komunikacja z Internetem odpowiadają bezpieczne połączenie najlepszych rozwiązań, zapobiegając w ten sposób przypadkowego ujawnienia poufnych informacji, bezpośrednio lub za pomocą aplikacji bibliotekę, która go używa.

Ponieważ ATS jest włączone domyślnie aplikacje są kompilowane dla systemu iOS 9, tvOS 9 i OS X 10.11 (El Capitan) i nowszych, wszystkie połączenia za pomocą `NSUrlConnection`, `CFUrl` lub `NSUrlSession` podlegają ATS wymogi bezpieczeństwa. Jeśli połączenia nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.

Na podstawie wybranych opcji stos HttpClient i implementacji protokołów SSL/TLS, konieczne może być wprowadzać modyfikacje do aplikacji, aby działać poprawnie z ATS.

Aby dowiedzieć się więcej na temat ATS, zobacz nasze [App Transport Security guide](~/ios/app-fundamentals/ats.md).

## <a name="known-issues"></a>Znane problemy

W tej sekcji opisano znane problemy z obsługą protokołu TLS w rozszerzeniu Xamarin.iOS.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>Nie można załadować z powodu błędu "Żądana wartość, których nie można znaleźć AppleTLS" projektu

Xamarin.iOS 9,8 wprowadzone pewne nowe ustawienia zawarte **.csproj** pliku dla aplikacji platformy Xamarin.iOS. Te zmiany mogą powodować problemy, podczas projektu jest otwierany przy użyciu starszych wersji rozszerzenia Xamarin.iOS. Poniższy zrzut ekranu przedstawia przykład komunikat o błędzie, który może być wyświetlany w tym scenariuszu:

![Zrzut ekranu przedstawiający wystąpił błąd podczas próby załadowania projektu, żądana wartość starszej wersji, nie można odnaleźć](http-stack-images/tlserror-xs.png)

Ten błąd jest spowodowany przez wprowadzenie `MtouchTlsProvider` ustawienie do pliku projektu w 9,8 platformy Xamarin.iOS. Jeśli nie jest można zaktualizować do 9,8 Xamarin.iOS (lub nowszej), aby ręcznie edytować jest około pracy **.csproj** pliku aplikacji, Usuń `MtouchTlsprovider` elementu, a następnie zapisz plik projektu zmienione.

Poniższy fragment kodu jest przykład tego, co `MtouchTlsProvider` ustawienie może wyglądać jak wewnątrz **.csproj** pliku:

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>Linki pokrewne

- [Protokół Transport Layer Security (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [Zabezpieczenia transportu aplikacji](~/ios/app-fundamentals/ats.md)
