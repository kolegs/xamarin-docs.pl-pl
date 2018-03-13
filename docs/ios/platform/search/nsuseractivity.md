---
title: Wyszukiwania z NSUserActivity
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 8376ce2ccff6732fa0c89d6030b9af36d29c5085
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="search-with-nsuseractivity"></a>Wyszukiwania z NSUserActivity

`NSUserActivity` został wprowadzony w systemie iOS 8 i służy do zapewnienia danych przekazaniem.
Umożliwia tworzenie w określonych części aplikacji, które można następnie przekazać do innego wystąpienia aplikacji uruchomionej na urządzeniu iOS różnych działań. Urządzenie odbierające następnie nadal uruchomione na urządzeniu poprzedniej pobrania się po prawej, gdy użytkownik przerwał pracę działania. Aby uzyskać więcej informacji na temat używania przekazaniem, zobacz nasze [wprowadzenie do przekazaniem](~/ios/platform/handoff.md) dokumentacji.

Jesteś nowym użytkownikiem systemu iOS 9, `NSUserActivity` może być indeksowana (zarówno publiczne i prywatne) i przeszukiwane przez wyszukiwanie Spotlight i Safari. Oznaczając `NSUserActivity` jako można wyszukiwać i dodawanie metadanych można indeksować, działanie może być wymieniona w wynikach wyszukiwania na urządzeniu z systemem iOS.

[![](nsuseractivity-images/apphistory01.png "Informacje o historii aplikacji")](nsuseractivity-images/apphistory01.png#lightbox)

Jeśli użytkownik wybierze wynik wyszukiwania, który należy do działania z aplikacji, aplikacja zostanie uruchomiona i działania opisanego przez `NSUserActivity` zostanie ponownie uruchomiony i widoczne dla użytkownika.

Następujące właściwości `NSUserActivity` są używane do obsługi aplikacji wyszukiwania:

 - `EligibleForHandoff` — Jeśli `true`, to działanie może być używane w ramach operacji przekazaniem.
 - `EligibleForSearch` — Jeśli `true`, to działanie zostanie dodane do indeksu na urządzeniu i w wynikach wyszukiwania.
 - `EligibleForPublicIndexing` — Jeśli `true`, to działanie zostanie dodane do indeksu w chmurze firmy Apple i widoczne dla użytkowników (za pośrednictwem wyszukiwania), które nie został jeszcze zainstalowany aplikacji na urządzeniu z systemem iOS. Zobacz [publicznego indeksowanie wyszukiwania](#Public-Search-Indexing) sekcji poniżej, aby uzyskać więcej informacji.
 - `Title` — Zawiera tytuł dla Twojego działania i jest wyświetlana w wynikach wyszukiwania. Tekst tytułu się również wyszukać użytkowników.
 - `Keywords` — Jest tablicą ciągów używanych do opisywania Twoich działaniach, która zostanie indeksowane i wykonane wyszukiwanie przez użytkownika końcowego.
 - `ContentAttributeSet` — Jest `CSSearchableItemAttributeSet` umożliwia dalsze działanie szczegółowo opisano i podaj bogatej zawartości w wynikach wyszukiwania.
 - `ExpirationDate` — Jeśli chcesz, aby tylko być widoczny przez daną datę działanie, musisz podać tutaj tej daty.
 - `WebpageURL` — Jeśli działania można wyświetlić w sieci web lub aplikacji obsługuje linków bezpośrednich przez przeglądarkę Safari, możesz ustawić łącze, aby przejść w tym miejscu.

## <a name="nsuseractivity-quickstart"></a>NSUserActivity Szybki Start

Wykonaj te instrukcje, aby zaimplementować wyszukiwanie `NSUserActivity` w aplikacji:

- [Tworzenie identyfikatorów typ działania](#creatingtypeid)
- [Tworzenie działania](#createactivity)
- [Odpowiada na działania](#respondactivity)
- [Indeksowanie wyszukiwania publiczny](#indexing)

Brak niektórych [dodatkowe korzyści](#benefits) przy użyciu `NSUserActivity` dokonanie wyszukiwanie zawartości.

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>Tworzenie identyfikatorów typ działania

Przed utworzeniem działanie wyszukiwania, musisz utworzyć _identyfikator typu działania_ można go zidentyfikować. Identyfikator typu działania jest ciągiem krótkim dodane do `NSUserActivityTypes` aplikacji **Info.plist** plik używany do jednoznacznego identyfikowania danego typu działania użytkownika. W tablicy dla każdego działania aplikacja obsługująca i przedstawia do wyszukiwania aplikacji będzie zawierać jeden wpis. 

Apple sugeruje, aby uniknąć kolizji przy użyciu notacji wstecznego DNS stylu dla identyfikatora typu działania. Na przykład: `com.company-name.appname.activity` dla określonej aplikacji na podstawie działań lub `com.company-name.activity` działań, które można uruchamiać wielu aplikacjom.

Identyfikator typu działania jest używany podczas tworzenia `NSUserActivity` wystąpienia, aby zidentyfikować typu działania. Gdy działanie jest nadal w wyniku użytkownika, wybierając polecenie wynik wyszukiwania, typ działania (wraz z identyfikator zespołu aplikacji) określa aplikacji, które można uruchomić, aby kontynuować działanie.

Aby utworzyć wymagane identyfikatory typu działań do obsługi tego zachowania, należy edytować **Info.plist** plików i przejdź do **źródła** widoku. Dodaj `NSUserActivityTypes` klucza i tworzenia identyfikatorów w następującym formacie:

[![](nsuseractivity-images/type01.png "Klucz NSUserActivityTypes i wymagane identyfikatory w edytorze plist")](nsuseractivity-images/type01.png#lightbox)

W powyższym przykładzie utworzyliśmy jeden nowy identyfikator typu działania dla działania wyszukiwania (`com.xamarin.platform`). Podczas tworzenia własnych aplikacji, Zastąp zawartość `NSUserActivityTypes` tablicy o identyfikatorach typu działania specyficzne dla działania aplikacji obsługuje.

<a name="createactivity" />

## <a name="creating-an-activity"></a>Tworzenie działania

Poniżej przedstawiono przykład tworzenia działanie wyszukiwania indeksu na urządzeniu hostowanej:

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.BecomeCurrent();
```

Firma Microsoft można dodać więcej szczegółów, ustawiając `ContentAttributeSet` właściwość naszych `NSUserActivity` w następujący sposób:

[![](nsuseractivity-images/apphistory02.png "Dodawanie szczegółów wyszukiwania przegląd")](nsuseractivity-images/apphistory02.png#lightbox)

Za pomocą `ContentAttributeSet` można tworzyć wyniki wyszukiwania sformatowanego, które zachęcić użytkownika końcowego do interakcji z użytkownikiem.

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>Odpowiada na działania

Aby odpowiedzieć na użytkownika, wybierając w wynikach wyszukiwania (`NSUserActivity`) dla aplikacji, należy edytować **AppDelegate.cs** plików i zastąpić `ContinueUserActivity` — metoda. Na przykład:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

Należy pamiętać, że jest używany do odpowiadania na żądania przekazaniem tego samego metodę zastępującą. Teraz, gdy użytkownik kliknie łącze z naszej aplikacji w wynikach wyszukiwania Spotlight, aplikacji zostanie przesunięte na pierwszy plan (lub pracy, jeśli nie jest jeszcze uruchomiona) i zawartości, nawigacji lub funkcji reprezentowany przez łącze będą wyświetlane:

[![](nsuseractivity-images/apphistory03.png "Przywróć stan poprzedniego wyszukiwania")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>Indeksowanie wyszukiwania publiczny

Jak widzieliśmy wcześniej systemu iOS 9 ułatwia wyszukiwania dostęp do zawartości aplikacji i funkcji, które już odnaleziony i użycia na urządzeniu iOS danego użytkownika. Z publicznego indeksowania systemu iOS 9 udostępnia sposób dla użytkowników, który nie zostało odnalezione zawartości lub funkcji jeszcze (lub który jeszcze nie zainstalowano aplikacji) można pobrać uzyskanych w wyszukiwaniach ich zbyt.

Indeksowanie publicznych działa w następujący sposób:

1. Po utworzeniu działania aplikacji Oznacz go jako publiczne.
2. Publicznych działań są wysyłane do firmy Apple i indeksowane w chmurze.
3. Jak więcej użytkowników aplikacji na urządzeniu i korzystać z określonych funkcji lub zawartości reprezentowany przez działanie, zwiększa się rangę.
4. Popularne publicznego wyniki będą dostępne dla innych użytkowników, nawet jeśli nie ma zainstalowanej aplikacji.
5. Wyniki te publiczne będą widoczne w przez wyszukiwanie Spotlight oraz Safari (Jeśli działanie zawiera adres URL).

Firma Microsoft podjąć działanie wyszukiwania prywatne, które utworzyliśmy powyżej i rozwiń go jako publiczny:

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.EligibleForPublicIndexing = true;
activity.BecomeCurrent();
```

Tak, ponieważ została ustawiona działanie dla publicznego indeksowania przez ustawienie `EligibleForPublicIndexing = true`, nie oznacza, że zostanie ona automatycznie dodana do indeksu chmury publicznej firmy Apple. Najpierw muszą być spełnione następujące warunki:

1. Musi występować w wynikach wyszukiwania, a wybrany na podstawie wielu użytkowników. Wyniki pozostaną prywatne, do czasu spełnienia próg zaangażowania działania.
2. Inicjowanie obsługi aplikacji zapobiega wszystkie dane specyficzne dla użytkownika indeksowane i opublikowany.

<a name="benefits" />

## <a name="additional-benefits"></a>Dodatkowe korzyści

Przyjmując wyszukiwania aplikacji za pośrednictwem `NSUserActivity` w aplikacji, możesz również uzyskać następujące funkcje:

- **Programowi handoff** — ponieważ wyszukiwania aplikacji jest udostępnianie zawartości, nawigacji i/lub funkcji za pomocą ten sam mechanizm jako przekazaniem (`NSUserActivity`), można łatwo użytkownicy aplikacji do uruchamiania działania na jednym urządzeniu i kontynuować go na innym.
- **Używanie programu Siri sugestie** — wraz z sugestie standardowe sugestie Siri zwykle sprawia, że, uaktywnia z aplikacji można automatycznie zasugerowany.
- **Przypomnienia inteligentne Siri** -użytkownicy będą mogli poprosić Siri przypominający, o działaniach związanych z aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Przewodnik programowania w języku wyszukiwania aplikacji](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [Wpis w blogu & próbki](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
