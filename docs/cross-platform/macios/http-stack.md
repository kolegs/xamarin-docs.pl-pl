---
title: "Stos HttpClient i selektor implementacji protokołu SSL/TLS dla systemu iOS/macOS"
description: "Stos HttpClient i selektor implementacji protokołu SSL/TLS określa implementacji HttpClient i SSL/TLS, który będzie używany przez aplikację systemu iOS, systemu tvOS lub macOS Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 06/12/2017
ms.openlocfilehash: eff096b1dca15b9b11038a599987f632bca2352f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-iosmacos"></a>Stos HttpClient i selektor implementacji protokołu SSL/TLS dla systemu iOS/macOS

## <a name="httpclient-stack-selector"></a>Selektor HttpClient stosu

Dostępna dla platformy Xamarin.iOS Xamarin.tvOS i Xamarin.Mac: która kontroluje `HttpClient` wdrożenia do użycia. Wartość domyślna jest nadal HttpClient, który jest obsługiwany przez `HttpWebRequest`, a teraz można opcjonalnie przełączyć implementację, która korzysta z systemem iOS, systemu tvOS lub macOS transportów natywnego (`NSUrlSession` lub `CFNetwork` w zależności od systemu operacyjnego). Odwróć mniejsze pliki binarne i szybsze pobieranie, wadą interfejsu jest wymaganie pętli zdarzenia należy uruchomić na można wykonać operacji asynchronicznej.

Projekty musi odwoływać się **System.Net.Http** zestawu.

<a name="Selecting-a-HttpClient-Stack" />

### <a name="selecting-a-httpclient-stack"></a>Wybieranie stosu HttpClient

Aby dostosować HttpClient używany przez aplikację:

1. Kliknij dwukrotnie **Nazwa projektu** w **Eksploratora rozwiązań** aby otworzyć aplet Opcje projektu.
2. Przełącz się do **kompilacji** ustawienia projektu (na przykład **kompilacji systemu iOS** dla aplikacji platformy Xamarin.iOS).
3. Z **implementacji HttpClient** listy rozwijanej wybierz HttpClient wpisać jako jeden z następujących: **zarządzane**, **CFNetwork** lub **NSUrlSession**.

[![Wybierz implementacji HttpClient z zarządzanego, CFNetwork lub NSUrlSession](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

<a name="Managed" />

### <a name="managed-default"></a>Zarządzane (ustawienie domyślne)

Obsługa zarządzanego jest pełni zarządzanego programu obsługi HttpClient, która została wysłana z poprzedniej wersji programu Xamarin.

#### <a name="pros"></a>Zalety:

 - Składa się z zestawu Microsoft .NET i starsze wersje Xamarin funkcji najbardziej zgodne.

#### <a name="cons"></a>Zalety:

 - Nie jest w pełni zintegrowana z systemów operacyjnych firmy Apple, a jest ograniczone do protokołu TLS 1.0.
 - Zwykle znacznie mniejsza w takich elementów, jak szyfrowanie macierzystych interfejsów API go.
 - Wymaga to więcej zarządzanego kodu, co powoduje utworzenie dystrybucyjnego większych aplikacji.

<a name="CFNetwork" />

### <a name="cfnetwork"></a>CFNetwork

Program obsługi na podstawie CFNetwork opiera się na natywnego `CFNetwork` framework dostępnych w systemie iOS 6 i nowsze.

#### <a name="pros"></a>Zalety:

 - Lepszą wydajność i mniejsze wykonywalnego używa macierzystych interfejsów API.
 - Obsługiwane przez nowszą standardy, takie jak protokół TLS 1.2.

#### <a name="cons"></a>Zalety:

 - Wymaga systemu iOS 6 lub nowszej.
 - Nie jest dostępna na watchOS.
 - Niektóre funkcje HttpClient/opcje nie są dostępne.

<a name="NSUrlSession" />

### <a name="nsurlsession"></a>NSUrlSession

Program obsługi na podstawie NSURLSession opiera się na natywnego `NSURLSession` framework dostępnych w systemie iOS 7 i nowszych.

#### <a name="pros"></a>Zalety:

 - Lepszą wydajność i mniejsze wykonywalnego używa macierzystych interfejsów API.
 - Obsługa najnowszych standardów, takie jak protokół TLS 1.2.

#### <a name="cons"></a>Zalety:

 - Wymaga systemu iOS 7 lub nowszy.
 - Niektóre funkcje HttpClient/opcje nie są dostępne.

### <a name="programmatically-setting-the-httpmessagehandler"></a>Programowo ustawienie klasa HttpMessageHandler

Oprócz przedstawionych powyżej konfiguracji na poziomie projektu można również utworzyć wystąpienie `HttpClient` i wstrzyknąć żądaną `HttpMessageHandler` za pośrednictwem konstruktora, jak pokazano w tych fragmentów kodu:

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

<a name="New-SSL-TLS-implementation-build-option" />
<a name="Selecting-a-SSL-TLS-implementation" />
<a name="Apple-TLS" />

## <a name="ssltls-implementation-build"></a>Implementacja protokołu SSL/TLS kompilacji

Protokół SSL (Secure Socket Layer) i jej następcą, TLS (Transport Layer Security), zapewniają obsługę dla protokołu HTTP i innych połączeń sieciowych za pośrednictwem `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS lub jego Xamarin.Mac `System.Net.Security.SslStream` implementacji wywoła implementacji native SSL/TLS firmy Apple zamiast zarządzaną implementację podał Mono. Firmy Apple implementacji native obsługuje protokół TLS 1.2.

<a name="Mono" />
> [!WARNING]
> **Mono/zarządzane** TLS dostawcy jest ograniczony do SSL v3 i TLS w wersji 1. Ten dostawca TLS została wycofana i nie jest już dostępna dla aplikacji platformy Xamarin.iOS. 

<a name="App-Transport-Security" />

## <a name="app-transport-security"></a>Zabezpieczenia transportu aplikacji

Firmy Apple _zabezpieczeń transportu aplikacji_ (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji. ATS zapewnia cała komunikacja z Internetem odpowiadają bezpieczne połączenie najlepszych rozwiązań, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych bezpośrednio przy użyciu aplikacji lub biblioteki, która zajmuje go.

Ponieważ ATS jest domyślnie włączone w aplikacje dla systemu iOS 9, systemu tvOS 9 i OS X 10.11 (El Capitan) i nowszych, wszystkie połączenia przy użyciu `NSUrlConnection`, `CFUrl` lub `NSUrlSession` podlegają ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.

Na podstawie wybranych opcji HttpClient stosu i implementacji protokołów SSL/TLS, może być konieczne wprowadzenie modyfikacji aplikację, aby działać poprawnie przy ATS.

Aby dowiedzieć się więcej na temat ATS, zobacz nasze [przewodnik dotyczący zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md).

## <a name="known-issues"></a>Znane problemy

W tej sekcji opisano znane problemy z obsługą protokołu TLS w platformy Xamarin.iOS.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>Nie można załadować z powodu błędu "Żądana wartość, którego nie można znaleźć AppleTLS" projektu

Xamarin.iOS 9,8 wprowadzone niektóre nowe ustawienia zawarte **.csproj** pliku dla aplikacji platformy Xamarin.iOS. Te zmiany może spowodować problemy, po otwarciu projektu ze starszymi wersjami platformy Xamarin.iOS. Poniższy zrzut ekranu jest przykładem mogą być wyświetlane w tym scenariuszu komunikat o błędzie:

![Zrzut ekranu przedstawiający wystąpił błąd podczas próby załadowania projektu, żądana wartość starszych nie znaleziono](http-stack-images/tlserror-xs.png)

Ten błąd jest spowodowany przez wprowadzenie `MtouchTlsProvider` ustawienie do pliku projektu w 9,8 platformy Xamarin.iOS. Jeśli nie jest można zaktualizować do 9,8 Xamarin.iOS (lub nowszej), aby ręcznie edytować jest około pracy **.csproj** pliku aplikacji, Usuń `MtouchTlsprovider` elementu, a następnie zapisz plik projektu zmienione.

Poniższy fragment jest przykładem co `MtouchTlsProvider` ustawienie może wyglądać jak wewnątrz **.csproj** pliku:

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>Linki pokrewne

- [Protokół Transport Layer Security (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [Zabezpieczenia transportu aplikacji](~/ios/app-fundamentals/ats.md)
