---
title: "Wyszukiwania za pomocą znacznika sieci Web"
description: "Dodawanie wyniki wyszukiwania opartych na sieci web, które można połączyć aplikację."
ms.topic: article
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 63bc1f0ed13fe65b36e95978da9ccc2ea8d4481c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="search-with-web-markup"></a>Wyszukiwania za pomocą znacznika sieci Web

W przypadku aplikacji, które zapewniają dostęp do zawartości za pośrednictwem witryny sieci web (nie tylko z poziomu aplikacji), zawartość sieci web może być oznaczony specjalne łącza, który będzie przeszukiwana przez firmę Apple oraz podaj połączeń bezpośrednich do aplikacji na urządzeniu z systemem iOS 9 użytkownika.

Jeśli już obsługuje aplikację systemu iOS przenośnych połączeń bezpośrednich i witryny sieci Web przedstawione głębokiego łącza do zawartości w aplikacji, Apple _Applebot_ przeszukiwarkę sieci web zostanie indeksu tej zawartości i automatycznie dodać je do ich indeksu chmury:

[ ![](web-markup-images/webmarkup01.png "Omówienie indeksu chmury")](web-markup-images/webmarkup01.png)

Apple będzie powierzchni te wyniki w przez wyszukiwanie Spotlight i Safari wyników wyszukiwania.
Jeśli użytkownik naciska na jeden z tych wyników (i mają zainstalowany aplikacji), a następnie zostaną wykonane do zawartości w aplikacji:

[ ![](web-markup-images/webmarkup02.png "Bezpośrednie połączenie z witryny w wynikach wyszukiwania")](web-markup-images/webmarkup02.png)

## <a name="enabling-web-content-indexing"></a>Włączanie indeksowania zawartości sieci Web

Istnieją cztery kroki wymagane do upewnij, że zawartość aplikacji wyszukiwanie przy użyciu znaczników sieci Web:

1. Upewnij się, że Apple mogą odnaleźć i indeksu aplikacji witryny sieci Web, definiując go jako **Obsługa** lub **Marketing** witryny programu iTunes Connect.
2. Upewnij się, że aplikacji witryny sieci Web zawiera kod znaczników wymagane do zaimplementowania przenośnych połączeń bezpośrednich. Znajduje się w sekcjach poniżej, aby uzyskać więcej informacji.
3. Włącz link bezpośredni do obsługi w aplikacji systemu iOS.
4. Dodaj kod znaczników dla danych strukturalnych udostępniane przez witryny sieci Web aplikacji zapewniają bogaty i atrakcyjne wynik dla użytkownika końcowego. Gdy ten krok nie jest ściśle wymagane, jest zalecane przez firmę Apple.

Poniższe sekcje będą przekazywane te kroki szczegółowo.

## <a name="make-your-apps-website-discoverable"></a>Stał się wykrywalny aplikacji witryny sieci Web

Najprostszym sposobem, aby znaleźć aplikacji witryny sieci Web firmy Apple ma być używany jako **Obsługa** lub **Marketing** witryny sieci Web podczas przesyłania aplikacji do firmy Apple za pomocą programu iTunes Connect.

## <a name="using-smart-app-banners"></a>Przy użyciu transparentach inteligentne aplikacji

Podaj inteligentne transparent aplikacji w witrynie sieci Web do prezentowania wyczyść łącze do aplikacji. Jeśli aplikacja nie jest jeszcze zainstalowany, Safari automatycznie monitują użytkownika do zainstalowania aplikacji. W przeciwnym razie można wybrać używanie **widoku** łącze, aby uruchomić aplikację w witrynie sieci Web. Na przykład aby utworzyć inteligentne transparent aplikacji, można użyć poniższego kodu:

```xml
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

Aby uzyskać więcej informacji, zobacz firmy Apple [wspieranie aplikacji za pomocą inteligentne transparentach aplikacji](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html) dokumentacji.

## <a name="using-universal-links"></a>Przy użyciu łącza uniwersalnych

Nowość w systemach iOS 9, uniwersalnych łącza zapewniają lepszą alternatywę do inteligentnego transparentach aplikacji lub istniejące niestandardowe schematy adresów URL podając następujące:

- **Unikatowy** — ten sam adres URL nie może być żądane przez więcej niż jednej witryny sieci Web.
- **Zabezpieczanie** — certyfikat z podpisem jest wymagany dla witryny sieci Web, zapewniający witryny sieci Web jest właścicielem przez Ciebie i licencjobiorcę połączone z aplikacji.
- **Elastyczne** — użytkownika końcowego można kontrolować, czy adres URL uruchamia witryny sieci Web lub aplikacji.
- **Uniwersalny** — ten sam adres URL może służyć do definiowania zawartości zarówno witryny sieci Web i aplikacji.

## <a name="using-twitter-cards"></a>Korzystanie z kart Twitter

Możesz podać głębokiego łącza do zawartości aplikacji za pomocą karty w usłudze Twitter. Na przykład:

```xml
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

Aby uzyskać więcej informacji, zobacz w serwisie Twitter [Twitter protokołu karty](http://dev.twitter.com/cards/mobile) dokumentacji.

## <a name="using-facebook-app-links"></a>Przy użyciu łącza aplikacji usługi Facebook

Możesz podać głębokiego łącza do zawartości aplikacji przy użyciu łącza aplikacji Facebook. Na przykład:

```xml
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

Aby uzyskać więcej informacji, zobacz w serwisie Facebook [łącza aplikacji](http://applinks.org) dokumentacji.

## <a name="opening-deep-links"></a>Otwieranie linków bezpośrednich

Należy dodać obsługę otwieranie i wyświetlanie linków bezpośrednich w aplikacji platformy Xamarin.iOS. Edytuj **AppDelegate.cs** plików i zastąpić `OpenURL` można obsłużyć formatu niestandardowego adresu URL. Na przykład:

```csharp
public override bool OpenUrl (UIApplication application, NSUrl url, string sourceApplication, NSObject annotation)
{

    // Handling a URL in the form http://company.com/appname/?123
    try {
        var components = new NSUrlComponents(url,true);
        var path = components.Path;
        var query = components.Query;

        // Is this a known format?
        if (path == "/appname") {
            // Display the view controller for the content
            // specified in query (123)
            return ContentViewController.LoadContent(query);
        }
    } catch {
        // Ignore issue for now
    }

    return false;
}
```

W powyższym kodzie czekamy na adres URL zawierający `/appname` i przekazując wartość `query` (`123` w tym przykładzie) do kontrolera widoku niestandardowego w naszej aplikacji do wyświetlenia żądanej zawartości dla użytkownika.

## <a name="providing-rich-results-with-structured-data"></a>Zapewnianie sformatowanego wyników z danych strukturalnych

W tym znaczników danych strukturalnych zapewnia wyniki wyszukiwania sformatowanego dla użytkownika końcowego, która wykracza poza po prostu tytuł i opis. Obejmują obrazy i określonych danych aplikacji (na przykład klasyfikacje) oraz akcji do wyników za pomocą znacznika danych strukturalnych.

Zaawansowana wyniki są zaangażowania więcej i może zwiększyć z klasyfikacji w chmurze na podstawie indeksu wyszukiwania przez zachęcając więcej użytkownikom na interakcję z nimi.

Jedną z opcji zapewniające znaczników danych strukturalnych jest przy użyciu interfejsu Open Graph. Na przykład:

```xml
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

Aby uzyskać więcej informacji, zobacz [interfejsu Open Graph](http://ogp.me) witryny sieci Web.

Inny wspólnej dla znacznika danych strukturalnych jest jego schema.org Mikrodane formatem. Na przykład:

```xml
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
    <span itemprop="ratingValue">4** stars -
    <span itemprop="reviewCount">255** reviews


```

Te same informacje może być reprezentowany w formacie JSON-LD schema.org firmy:

```xml
<script type="application/ld+json">
    "@content":"http://schema.org",
    "@type":"AggregateRating",
    "ratingValue":"4",
    "reviewCount":"255"
</script>
```

Poniżej przedstawiono przykład metadane z witryny sieci Web udostępnia zaawansowane wyszukiwanie dla użytkownika końcowego:

[ ![](web-markup-images/deeplink01.png "Wyniki za pomocą znacznika danych strukturalnych wyszukiwania RTF")](web-markup-images/deeplink01.png)

Apple obecnie obsługuje następujące typy schematu z schema.org:

 - AggregateRating
 - ImageObject
 - InteractionCount
 - Oferty
 - Organizacja
 - PriceRange
 - Przepisu
 - SearchAction

Aby uzyskać więcej informacji o tych typach schematu, zobacz [schema.org](http://schema.org).

## <a name="providing-actions-with-structured-data"></a>Zapewnianie akcji z danych strukturalnych

Określone typy danych strukturalnych umożliwi wynik wyszukiwania jako umożliwiająca wykonanie akcji przez użytkownika końcowego. Obecnie obsługiwane są następujące akcje:

 - Wybieranie numeru telefonu.
 - Pobieranie kierunek mapy do określonego adresu.
 - Odtwarzanie plików audio i wideo.

Na przykład definiowanie akcji, aby wybrać numer telefonu może wyglądać następujące czynności:

```xml
<div itemscope itemtype="http://schema.org/Organization">
    <span itemprop="telephone">(408) 555-1212**


```

Gdy ten wyniki wyszukiwania będzie wyświetlana dla użytkownika końcowego, ikona małych telefonu będzie wyświetlany w wyniku. Jeśli użytkownik naciska ikony, zostanie wywołana z określoną liczbą.

Poniższy kod HTML dodać akcję do odtwarzania pliku dźwiękowego z wyników wyszukiwania:

```xml
<div itemscope itemtype="http://schema.org/AudioObject">
    <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**


```

Na koniec poniższy kod HTML dodać akcję, aby uzyskać wskazówki z wyników wyszukiwania:

```xml
<div itemscope itemtype="http://schema.org/PostalAddress">
    <span itemprop="streetAddress">1 Infinite Loop**
    <span itemprop="addressLocality">Cupertino**
    <span itemprop="addressRegion">CA**
    <span itemprop="postalCode">95014**


```

Aby uzyskać więcej informacji, zobacz firmy Apple [witryny dewelopera aplikacji wyszukiwania](http://developer.apple.com/ios/search/).



## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Przewodnik programowania w języku wyszukiwania aplikacji](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
