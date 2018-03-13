---
title: Ulepszenia wyszukiwania aplikacji
description: "W tym artykule omówiono ulepszenia Apple wprowadziła aplikacji wyszukiwania w iOS 10 oraz sposób ich wdrażania w Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: af124c2ae0390c5321e9dd34158c7b53b33b2c48
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="app-search-enhancements"></a>Ulepszenia wyszukiwania aplikacji

_W tym artykule omówiono ulepszenia Apple wprowadziła aplikacji wyszukiwania w iOS 10 oraz sposób ich wdrażania w Xamarin.iOS._

W systemie iOS 10 Apple wprowadziła kilka ulepszeń aplikacji wyszukiwania, takie jak połączeń bezpośrednich Crowdsourced, wyszukiwania w aplikacji, kontynuacji Search i wizualizacja wyników weryfikacji. W tym artykule opisano wdrażanie tych funkcji w aplikacji platformy Xamarin.iOS.

## <a name="about-app-search-enhancements"></a>Temat udoskonaleń dotyczących aplikacji wyszukiwania

Spotlight Core w systemie iOS 10 udostępnia kilka rozszerzeń takich jak do wyszukiwania aplikacji:

- **Popularne z Linkiem bezpośrednim Crowdsourced (o prywatności różnicowa)** — zapewnia sposób promowania zawartość aplikacji z linkiem bezpośrednim w wynikach wyszukiwania.
- **Wyszukiwanie w aplikacji** — Użyj nowych `CSSearchQuery` klasy, aby zapewnić możliwość wyszukiwania uwagi w aplikacji podobnie jak działają aplikacje poczty, wiadomości i notatki.
- **Wyszukaj kontynuacji** — umożliwia użytkownikowi rozpocząć wyszukiwanie Spotlight lub Safari, a następnie otwórz aplikację i kontynuować tego wyszukiwania.
- **Wizualizacja wyników weryfikacji** -firmy Apple [aplikacji wyszukiwania interfejsu API sprawdzania poprawności narzędzie](https://search.developer.apple.com/appsearch-validation-tool) podczas preforming testy są obecnie wyświetlane wizualną reprezentację znaczników witryny sieci Web, a następnie połączenie bezpośrednie.
- **Komunikat aplikacji do udostępniania obrazu** — umożliwia popularnych obrazów w aplikacji dostępne w celu udostępniania w komunikatach (za pośrednictwem rozszerzenia aplikacji wiadomości) do jego wyświetlenia na wyszukiwanie Spotlight.

Poniższe sekcje opisano te tematy bardziej szczegółowo.

## <a name="crowdsourced-deep-link-popularity"></a>Popularne Crowdsourced Linkiem

iOS 10 udostępnia mechanizm do liczby częstotliwość czy popularnych linków bezpośrednich do aplikacji zostaną wykonane przez użytkownika i używa tych informacji do poprawy klasyfikację aplikację do zawartości w wynikach wyszukiwania przy zapewnieniu ochrony tożsamości użytkownika przy użyciu  *Prywatność różnicowej*.

Podczas aplikacji używających `NSUserActivity` obiektów do zapewniają linków bezpośrednich adresów URL i mają `EligibleForPublicIndexing` ustawioną właściwość `true`, iOS 10 przesyła podzbiór *różnicowej prywatności skrótów* do serwerów firmy Apple. Te informacje jest następnie używany do promowania popularnych zawartości w aplikacji w wynikach wyszukiwania.

Aby uzyskać więcej informacji dotyczących wdrażania połączeń bezpośrednich w aplikacji platformy Xamarin.iOS, zobacz nasze [wyszukiwania za pomocą NSUserActivity](~/ios/platform/search/nsuseractivity.md) dokumentacji.

## <a name="in-app-searching"></a>Wyszukiwanie w aplikacji

Zaimplementowanie nowe [CSSearchQuery](https://developer.apple.com/reference/corespotlight/cssearchquery) klasy, aplikację można podać wyszukiwania i dopasowania technologii reguły można znaleźć zawartości wewnątrz, bez konieczności opuszczania aplikacji (podobnie jak poczta, wiadomości i notatki aplikacji użytkownika w centrum uwagi Praca).

Zazwyczaj aplikacje obsługujące `CSSearchQuery` nie należy do obsługi własnych, indeksu wyszukiwania. 

## <a name="search-continuation"></a>Kontynuacji Search

W systemie iOS 9, Firma Apple wprowadziła interfejsy API wyszukiwania (takich jak Core Spotlight `NSUserActivity` oraz znaczników sieci web) umożliwia bezpośrednie preferencjom zawartości wewnątrz aplikacji, aby umożliwić użytkownikom wyszukiwanie zawartości przy użyciu interfejsów wyszukiwanie Spotlight i Safari. Zobacz nasze [nowe interfejsy API wyszukiwania](~/ios/platform/search/index.md) dokumentację, aby uzyskać więcej szczegółowych informacji.

W systemie iOS firmy Apple 10 bazując na tej funkcji przez użytkownika rozpocząć wyszukiwanie Spotlight lub Safari, a następnie kontynuować wyszukiwanie, po otwarciu aplikacji. 

Aby zaimplementować tę funkcję, należy edytować aplikacji `Info.plist` plików, dodawanie `CoreSpotlightContinuation` klucza typu **logiczna** i ustaw jej wartość `YES`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](app-search-enhancements-images/search01.png "Edytowanie CoreSpotlightContinuation w pliku Info.plist")](app-search-enhancements-images/search01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](app-search-enhancements-images/searchw01.png "Edytowanie CoreSpotlightContinuation w pliku Info.plist")](app-search-enhancements-images/search01.png#lightbox)

-----

Aby odpowiedzieć na kontynuowanie wynik wyszukiwania użytkownika (`NSUserActivity`), Edytuj `AppDelegate.cs` pliku i zastąpić `ContinueUserActivity` — metoda. Na przykład:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchQuery.ContinuationActionType) {
            var search = userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString);
            // Continue user's search here...
        }
        break;
    }

    return true;
}
```

Ten kod jest szuka typ akcji kontynuacji kwerendy (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`), następnie odczytuje użytkownika bieżącego zapytania z `NSUserActivity` słownik informacje użytkownika klasy (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`). W tym miejscu aplikacji należy podjąć działania, aby kontynuować wyszukiwanie użytkownika.

Aby uzyskać więcej informacji na temat pracy z wyszukiwania w aplikacji platformy Xamarin.iOS, zobacz nasze [wyszukiwania z wyróżnionym Core](~/ios/platform/search/corespotlight.md) dokumentacji.

## <a name="visualization-of-validation-results"></a>Wizualizacja wyników weryfikacji

Firmy Apple [aplikacji wyszukiwania interfejsu API sprawdzania poprawności narzędzie](https://search.developer.apple.com/appsearch-validation-tool) wizualną reprezentację znaczników witryny sieci Web i połączeń bezpośrednich są obecnie wyświetlane (znaczników, taki jak zdefiniowana w tym [Schema.org](http://schema.org/)) podczas preforming testy.

Przy użyciu narzędzia do sprawdzania poprawności, deweloper można zauważyć, że informacje, że Applebot przeszukiwarkę sieci Web ma indeksowanym dla lokacji, takie jak tytuł, opis, adres URL i inne obsługiwane elementy.

Aby uzyskać więcej informacji na temat pracy z znaczników sieci Web, zobacz nasze [działanie z sieci Web znaczników](~/ios/platform/search/web-markup.md) dokumentacji.

## <a name="message-app-image-sharing"></a>Komunikat obraz aplikacji do udostępniania

Jeśli rozszerzenie aplikacji komunikat zawiera obrazy udostępniania w wiadomości, aby zezwolić użytkownikowi na wyszukiwanie Spotlight dla popularnych obrazów zawartych w wiadomości, bez konieczności opuszczania aplikacji można skonfigurować rozszerzenia.

Aby włączyć tę funkcję, wykonaj następujące czynności:

1. Tworzenie rozszerzenia komunikatów aplikacji.
2. Dodaj `com.apple.developer.associated-domains` do uprawnień dla aplikacji i zawiera listę domen sieci web, które obsługuje obrazy rozszerzenia komunikatów w aplikacji do udostępniania. Dla każdej domeny, określ `spotlight-image-search` usługi.
3. Dodaj `apple-app-site-association` plików do witryny sieci Web, który jest hostem obrazów. Ten plik zawiera słownik na potrzeby `spotlight-image-search` usługi i zawiera identyfikator aplikacji, które jest prefiksem Identyfikatora zespołu lub identyfikator aplikacji następuje identyfikator pakietu. Plik może zawierać maksymalnie 500 ścieżek i wzorce, które będą indeksowane według Spotlight i uwzględniane podczas wyszukiwania popularnych obrazu. Aby uzyskać więcej informacji, zobacz firmy Apple [tworzenie i przekazywanie pliku skojarzenia](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4) dokumentacji.
4. Zezwalaj na Applebot do przeszukiwania witryn sieci Web. Zobacz firmy Apple [o Applebot](https://support.apple.com/en-us/HT204683) dokumentacji.

Zobacz nasze [integracji aplikacji komunikat](~/ios/platform/message-app-integration/index.md) dokumentację, aby uzyskać więcej szczegółowych informacji.

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego ulepszenia Apple wprowadziła aplikacji wyszukiwania w iOS 10 oraz sposób ich wdrażania w Xamarin.iOS.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
