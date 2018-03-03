---
title: Zabezpieczenia transportu aplikacji
description: "Zabezpieczenia transportu aplikacji (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 60858e05e222725f05eb67bd7aaa4e56d2ff3880
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="app-transport-security"></a>Zabezpieczenia transportu aplikacji

_Zabezpieczenia transportu aplikacji (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji._

W tym artykule przedstawiono zmiany zabezpieczeń, które zabezpieczeń transportu aplikacji wymusza na aplikację systemu iOS 9 i [to dla projektów Xamarin.iOS](#xamarinsupport), będzie ona obejmować [opcje konfiguracji ATS](#config) i będzie ona obejmować porady [Wypisz ATS](#optout) ATS, jeśli jest to wymagane. Ponieważ ATS jest domyślnie włączona, wszystkie niezabezpieczonego połączenia internetowe zgłosi wyjątek w aplikacji systemu iOS 9 (chyba że jawnie już dozwolone).


## <a name="about-app-transport-security"></a>Zabezpieczenia transportu aplikacji — informacje

Jak już wspomniano, ATS zapewnia wszystkie komunikację z Internetem w systemie iOS 9 i OS X El Capitan odpowiadają bezpieczne połączenie najlepszych rozwiązań, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych bezpośrednio przy użyciu aplikacji lub biblioteki, która jest Korzystanie z programu.

W przypadku istniejących aplikacji, wdrożenie `HTTPS` protokołu, jeśli to możliwe. Dla nowej aplikacji platformy Xamarin.iOS, należy używać `HTTPS` wyłącznie podczas komunikowania się z zasobami sieci internet. Ponadto wysokiego poziomu komunikacji interfejsu API musi być zaszyfrowany za pomocą protokołu TLS w wersji 1.2 z utajnienie.

Wszystkie połączenia z [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) lub [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) użyje ATS domyślnie aplikacje dla systemu iOS 9 i OS X 10.11 (El Capitan).

## <a name="default-ats-behavior"></a>Domyślne zachowanie ATS

Ponieważ ATS jest domyślnie włączona w aplikacji skompilowany dla systemu iOS 9 i OS X 10.11 (El Capitan), wszystkie połączenia przy użyciu [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) lub [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) będzie pracował Wymagania dotyczące zabezpieczeń ATS. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.

### <a name="ats-connection-requirements"></a>Wymagania dotyczące połączenia ATS

ATS będzie wymuszać z następującymi wymaganiami dla wszystkich połączeń z Internetem:

- Wszystkie połączenia szyfrów muszą używać utajnienie. Zobacz listę zaakceptowane szyfrów poniżej.
- Protokół zabezpieczeń TLS (Transport Layer) musi być w wersji 1.2 lub nowszej.
- Należy użyć co najmniej SHA256 linii papilarnych z 2048 bitów lub większej klucza RSA, lub 256-bitową albo większa klucza Elliptic krzywej (ECC) dla wszystkich certyfikatów.

Ponownie ponieważ ATS jest domyślnie włączone w systemie iOS 9, wszelkie próby nawiązania połączenia, które nie spełniają tych wymagań spowoduje wyjątek. 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>ATS szyfrów zgodne

Następujący typ szyfrowania utajnienie są akceptowane przez ATS zabezpieczone komunikację z Internetem:

- `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA`
- `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`

Aby uzyskać więcej informacji na temat pracy z klasy komunikacji internetowe z systemem iOS, zobacz firmy Apple [odwołania do klasy NSURLConnection](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755), [odwołania CFURL](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) lub [NSURLSession odwołania do klasy ](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Obsługa ATS w Xamarin.iOS

Ponieważ ATS jest domyślnie włączone w systemu iOS 9 i OS X El Capitan, jeśli w aplikacji platformy Xamarin.iOS lub wszystkie biblioteki lub usługi, której używa nawiązuje połączenie z Internetem, musisz podjąć inną akcję lub połączenia spowoduje wyjątek.

Dla istniejącej aplikacji, Apple sugeruje obsługiwanych `HTTPS` protokołu tak szybko, jak to możliwe. Jeśli albo nie ponieważ łączysz się 3rd strony usługi sieci web, która nie obsługuje `HTTPS` lub w przypadku obsługi `HTTPS` byłoby niepraktyczne, użytkownik może zrezygnować z ATS. Zobacz [Opting poza ATS](#Opting-Out-of-ATS) sekcji poniżej, aby uzyskać więcej informacji.

Dla nowej aplikacji platformy Xamarin.iOS, należy użyć `HTTPS` wyłącznie podczas komunikowania się z zasobami sieci internet. Ponownie, może się zdarzyć, (na przykład przy użyciu 3 Usługa sieci web firmy) gdy nie jest to możliwe, i należy zrezygnować z ATS.

Ponadto ATS wymusza będą szyfrowane przy użyciu protokołu TLS w wersji 1.2 z utajnienie wysokiego poziomu komunikacji interfejsu API. Zobacz [wymagań dotyczących połączenia ATS](#ATS-Connection-Requirements) i [szyfrów zgodne ATS](#ATS-Compatible-Ciphers) sekcje powyżej, aby uzyskać więcej informacji.

Podczas nie może być zapoznać się z protokołem TLS ([Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)) jest następnika do protokołu SSL ([Secure Socket Layer](https://en.wikipedia.org/wiki/Transport_Layer_Security)) i zapewnia zbiór protokołów kryptograficznych, aby wymusić zabezpieczeń przez połączenia sieciowe.

Poziom TLS jest kontrolowany przez usługę sieci web, które są wykorzystywanie i dlatego jest poza kontrolą aplikacji. Zarówno `HttpClient` i `ModernHttpClient` automatycznie powinny używać najwyższy poziom szyfrowania TLS obsługiwanej przez serwer.

W zależności od serwera, że trwa konwersacja (zwłaszcza, jeśli jest usługą strona 3), może być konieczne wyłączenie utajnienie lub wybierz niższej TLS. Zobacz [Konfigurowanie opcji ATS](#Configuring-ATS-Options) sekcji poniżej, aby uzyskać więcej informacji.

> [!IMPORTANT]
> **Uwaga:** zabezpieczeń transportu aplikacji nie ma zastosowania do przy użyciu aplikacji Xamarin **implementacje zarządzane HTTPClient**. Dotyczy on połączeń za pomocą CFNetwork **implementacje HTTPClient** lub **NSURLSession HTTPClient implementacje** tylko.

### <a name="setting-the-httpclient-implementation"></a>Ustawienie wdrażania HTTPClient

Aby ustawić implementacji HTTPClient używany przez aplikację systemu iOS, kliknij dwukrotnie **projektu** w **Eksploratora rozwiązań** otworzyć **opcje projektu**. Przejdź do **kompilacji systemu iOS** i wybierz typ klienta żądaną w obszarze **implementacji HttpClient** listy rozwijanej:

![](ats-images/client01.png "Ustawienie opcji kompilacji systemu iOS")


#### <a name="managed-handler"></a>Zarządzanego programu obsługi

Obsługa zarządzanego jest pełni zarządzanego programu obsługi HttpClient, który został wysłany w poprzednich wersjach platformy Xamarin.iOS i jest domyślny program obsługi.

Zalety:

- Jest najbardziej zgodna z platformy Microsoft .NET i starszej wersji programu Xamarin.

Zalety:

- Nie jest on pełni zintegrowany z systemem iOS (np. jest ograniczone do protokołu TLS 1.0).
- Jest zwykle znacznie mniejsza niż macierzystych interfejsów API.
- Go wymaga więcej kodu zarządzanego i tworzy aplikacje większy.

#### <a name="cfnetwork-handler"></a>Program obsługi CFNetwork

Obsługi CFNetwork na podstawie opiera się na natywnego `CFNetwork` framework.

Zalety:

- Używa natywnego interfejsu API lepszą wydajność i mniejsze rozmiary pliku wykonywalnego.
- Dodaje obsługę nowszych standardy, takie jak protokół TLS 1.2.

Zalety:

- Wymaga systemu iOS 6 lub nowszej.
- Nie jest dostępna z watchOS.
- Niektóre funkcje HttpClient i opcje nie są dostępne.

#### <a name="nsurlsession-handler"></a>Program obsługi NSUrlSession

Obsługi NSUrlSession na podstawie opiera się na natywnego `NSUrlSession` interfejsu API.

Zalety:

- Używa natywnego interfejsu API lepszą wydajność i mniejsze rozmiary pliku wykonywalnego.
- Dodaje obsługę nowszych standardy, takie jak protokół TLS 1.2.

Zalety:

- Wymaga systemu iOS 7 lub nowszy.
- Niektóre funkcje HttpClient i opcje nie są dostępne. 

## <a name="diagnosing-ats-issues"></a>Diagnozowanie problemów ATS

Podczas próby połączenia z Internetem bezpośrednio lub z widoku sieci web w systemie iOS 9, może być błąd pojawia się w postaci:

> Zabezpieczenia transportu aplikacji zablokował obciążenia zasobu HTTP (http://www.-the-blocked-domain.com) jako zwykły tekst, ponieważ jest niebezpieczne. Wyjątki tymczasowego można skonfigurować za pomocą pliku Info.plist aplikacji.

W iOS9 zabezpieczeń transportu aplikacji (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji. Ponadto ATS wymaga komunikacji przy użyciu `HTTPS` protokołu i być szyfrowana przy użyciu protokołu TLS w wersji 1.2 z utajnienie wysokiego poziomu komunikacji interfejsu API.

Ponieważ ATS jest domyślnie włączona w aplikacji skompilowany dla systemu iOS 9 i OS X 10.11 (El Capitan), wszystkie połączenia przy użyciu `NSURLConnection`, `CFURL` lub `NSURLSession` podlegają ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.

Udostępnia również Apple [TLSTool Przykładowa aplikacja](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) mogą być kompilowane (lub opcjonalnie przekodowane Xamarin i C#) i służy do diagnozowania problemów ATS/TLS. Zobacz [Opting poza ATS](#Opting-Out_of_ATS) sekcji poniżej, aby uzyskać informacje na temat rozwiązywania tego problemu.


<a name="config" />

## <a name="configuring-ats-options"></a>Konfigurowanie opcji ATS

Można skonfigurować kilka funkcji ATS przez ustawienie wartości kluczy określonych w Twojej aplikacji **Info.plist** pliku. Następujące klucze są dostępne do kontrolowania ATS (_wcięcia, które pokazują, jak są zagnieżdżone_):

```csharp
NSAppTransportSecurity
    NSAllowsArbitraryLoads
    NSAllowsArbitraryLoadsInWebContent
    NSExceptionDomains
    <domain-name-for-exception-as-string>
        NSExceptionMinimumTLSVersion
        NSExceptionRequiresForwardSecrecy
        NSExceptionAllowsInsecureHTTPLoads
        NSRequiresCertificateTransparency
        NSIncludesSubdomains
        NSThirdPartyExceptionMinimumTLSVersion
        NSThirdPartyExceptionRequiresForwardSecrecy
        NSThirdPartyExceptionAllowsInsecureHTTPLoads
```

Każdy klucz ma następujący typ i znaczenie:

- **NSAppTransportSecurity** (`Dictionary`) — zawiera wszystkie ustawienia kluczy i wartości ATS.
- **NSAllowsArbitraryLoads** (`Boolean`) — Jeśli `YES` ATS zostanie wyłączony dla wszystkich domen **nie** na liście `NSExceptionDomains`. Dla listy domen ustawieniami zabezpieczeń będzie używany.
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean`) — Jeśli `YES` umożliwi stron sieci web można prawidłowo załadować podczas zabezpieczeń transportu firmy Apple (ATS) jest nadal chroniona w pozostałej części aplikacji.
- **NSExceptionDomains** (`Dictionary`) — zbiór domeny czy i ustawienia zabezpieczeń, które mają zostać użyte ATS dla danej domeny.
- **< Domain-name-for-exception-as-string >** (`Dictionary`) — zbiór wyjątki dla danej domeny (np.) `www.xamarin.com`).
- **NSExceptionMinimumTLSVersion** (`String`)-minimalną wersję protokołu TLS jako `TLSv1.0`, `TLSv1.1` lub `TLSv1.2` (jest to wartość domyślna).
- **NSExceptionRequiresForwardSecrecy** (`Boolean`) — Jeśli `NO` domeny bez szyfrowania za pomocą zabezpieczeń do przodu. Wartość domyślna to `YES`.
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`) — Jeśli `NO` (ustawienie domyślne) cała komunikacja z tej domeny musi być w `HTTPS` protokołu.
- **NSRequiresCertificateTransparency** (`Boolean`) — Jeśli `YES` domeny Secure Sockets Layer (SSL) musi zawierać prawidłowy przezroczystość danych. Wartość domyślna to `NO`.
- **NSIncludesSubdomains** (`Boolean`) — Jeśli `YES` ustawienia te zastępują wszystkich domen podrzędnych domeny. Wartość domyślna to `NO`.
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`)-TLS, wersja używany, gdy domena jest 3 usługa firmy poza kontrolą dewelopera.
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`) — Jeśli `YES` utajnienie wymaga 3 domeny firmy.
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`) — Jeśli `YES` ATS umożliwi z systemem innym niż bezpieczna komunikacja z 3 stron domenami.

<a name="optout" />

### <a name="opting-out-of-ats"></a>Rezygnacja poza ATS

Gdy wysokiej sugeruje Apple przy użyciu `HTTPS` protokołu i bezpieczna komunikacja z Internetem na podstawie informacji, może być razy, które nie jest to zawsze możliwe. Na przykład, jeśli są komunikowania się z 3 Usługa sieci web firmy lub przy użyciu Internetu dostarczane reklam w aplikacji.

Jeśli w aplikacji platformy Xamarin.iOS żądanie do niezabezpieczonych domeny należy się upewnić, poniższe zmiany do swojej aplikacji **Info.plist** pliku spowoduje wyłączenie ustawienia domyślne zabezpieczeń, które egzekwuje ATS danej domeny:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>www.the-domain-name.com</key>
        <dict>
            <key>NSExceptionMinimumTLSVersion</key>
            <string>TLSv1.0</string>
            <key>NSExceptionRequiresForwardSecrecy</key>
            <false/>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <true/>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>
```

W programie Visual Studio for Mac, kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań**, przełącz się do **źródła** wyświetlanie i dodawanie kluczy powyżej:

[ ![](ats-images/ats01.png "Widok źródła w pliku Info.plist")](ats-images/ats01.png)


Jeśli Twoja aplikacja powinna ładowanie i wyświetlanie zawartości sieci web z niezabezpieczonego witryn, dodaj następującą wartość do aplikacji **Info.plist** pliku, aby poprawnie załadowany zabezpieczeń transportu firmy Apple (ATS) jest nadal chroniona pozostałe strony sieci web aplikacji:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

Opcjonalnie wprowadź następujące zmiany do swojej aplikacji **Info.plist** plik, aby całkowicie wyłączyć ATS dla wszystkich domen i komunikacja z Internetem:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

W programie Visual Studio for Mac, kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań**, przełącz się do **źródła** wyświetlanie i dodawanie kluczy powyżej:

[ ![](ats-images/ats02.png "Widok źródła w pliku Info.plist")](ats-images/ats02.png)

> [!IMPORTANT]
> **Uwaga:** Jeśli aplikacja wymaga połączenia niezabezpieczonego witryny sieci Web, należy **zawsze** wprowadź domenę jako wyjątków za pomocą `NSExceptionDomains` zamiast wyłączania ATS całkowicie przy użyciu `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` należy używać tylko extreme sytuacji awaryjnych.




Ponownie, wyłączenie ATS powinien _tylko_ służyć w ostateczności przełączanie do bezpiecznego połączenia jest niedostępna lub niepraktyczne.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

Ten artykuł zawiera wprowadzono zabezpieczeń transportu aplikacji (ATS) i opisano sposób wymusza bezpieczną komunikację z Internetem. Po pierwsze firma Microsoft objęte zmiany, które wymaga ATS dla aplikacji platformy Xamarin.iOS systemem iOS 9. Następnie możemy objęte kontrolowanie funkcji ATS i opcje. Ponadto firma Microsoft objęte Rezygnacja z ATS w aplikacji platformy Xamarin.iOS.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
