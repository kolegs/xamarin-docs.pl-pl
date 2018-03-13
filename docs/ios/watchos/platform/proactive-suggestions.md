---
title: Sugestie aktywne
description: "W tym artykule przedstawiono sposób używanie sugestie aktywne w aplikacji watchOS 3 do zaangażowania dysku przez system aktywnego automatycznie przekazać pomocne informacje do użytkownika."
ms.topic: article
ms.prod: xamarin
ms.assetid: 10CC9F16-963C-44F1-8B98-F09FB2310DFF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: f9711cc39662a7e77d926551a0d2b49363d8ec4d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="proactive-suggestions"></a>Sugestie aktywne

_W tym artykule przedstawiono sposób używanie sugestie aktywne w aplikacji watchOS 3 do zaangażowania dysku przez system aktywnego automatycznie przekazać pomocne informacje do użytkownika._


Jesteś nowym użytkownikiem watchOS 3, aktywne sugestie obecny wiadomości sposoby użytkownikom współpracować z aplikacji platformy Xamarin.iOS przez aktywne obecny przydatne informacje automatycznie dla użytkownika w odpowiednim czasie.


## <a name="about-proactive-suggestions"></a>O sugestie aktywne

Jesteś nowym użytkownikiem watchOS 3, `NSUserActivity` obejmuje `MapItem` właściwość, która umożliwia aplikacji dostarczanie informacji o lokalizacji, które mogą być używane w innych kontekstach. Na przykład, jeśli hoteli aplikacji wyświetlane monitoruje i udostępnia `MapItem` lokalizacji użytkownika przełączona do aplikacji map, lokalizacji hoteli tylko wyświetlana będzie dostępna.

Aplikacja udostępnia tę funkcję do systemu przy użyciu kolekcji technologii, takich jak `NSUserActivity`, MapKit, Media Player i UIKit. Ponadto zapewniając obsługę aktywnego sugestię aplikacji, pobiera lepsza integracja Siri bezpłatnie.

## <a name="location-based-suggestions"></a>Lokalizacja na podstawie sugestii

Jesteś nowym użytkownikiem watchOS 3, `NSUserActivity` klasa zawiera `MapItem` właściwość, która umożliwia deweloperowi Podaj informacje o lokalizacji, które mogą być używane w innych kontekstach. Na przykład jeśli aplikacja wyświetla restauracji recenzji, deweloper można ustawić `MapItem` właściwości do lokalizacji restauracji, użytkownik jest wyświetlana w aplikacji. Jeśli użytkownik zmienia się na aplikacji mapy, restauracji lokalizacji jest automatycznie dostępny.

Jeśli aplikacja obsługuje wyszukiwania aplikacji, można użyć nowych składników adres `CSSearchableItemAttributesSet` klasę, aby określić lokalizacje, które użytkownik może odwiedzić. Przez ustawienie `MapItem` właściwości, inne właściwości są wypełniane automatycznie.

Oprócz ustawienia `Latitude` i `Longitude` adresu właściwości składnika, zalecane jest, że aplikacja podać `NamedLocation` i `PhoneNumbers` właściwości, więc Siri mogą inicjować wywołania do lokalizacji.

## <a name="contextual-siri-reminders"></a>Przypomnienia kontekstowe Siri

Umożliwia użytkownikowi używaj Siri umożliwia szybkie przypomnieniami Aby wyświetlić zawartość się, że są obecnie wyświetlane w aplikacji w późniejszym terminie. Na przykład, jeśli zostały one wyświetlanie przeglądu restauracji w aplikacji, ich można wywołać Siri i wypowiedz *"Przypomnij mi na ten temat gdy pojawia się Strona główna".* Używanie programu Siri może wygenerować monitu z łączem do przeglądu w aplikacji.

## <a name="implementing-proactive-suggestions"></a>Implementowanie sugestie aktywne

Dodawanie aktywnego sugestię wsparcie dla aplikacji platformy Xamarin.iOS jest zwykle tak proste, jak implementacja kilka interfejsów API lub rozszerzanie na kilka interfejsów API, które aplikacji może już być implementacja.

Aktywne sugestie dotyczące pracy z aplikacjami przede wszystkim na trzy sposoby:

- **`NSUserActivity`** -Pomaga zrozumieć, jakie informacje na ekranie użytkownika aktualnie jest praca z systemu.
- **Sugestie lokalizacji** — Jeśli aplikacja udostępnia lub wykorzystuje lokalizacji na podstawie informacji, te interfejsu API rozszerzenia oferta nowe sposoby udostępniania tych informacji w aplikacjach.

I jest obsługiwany w aplikacji przez wykonania następujących czynności:

- **Kontekstowe przypomnienia Siri** — w systemie iOS 10, `NSUserActivity` został rozszerzony w celu Zezwalaj na Siri szybko przypomnieniami Aby wyświetlić zawartość są obecnie wyświetlane w aplikacji w późniejszym terminie.
- **Sugestie lokalizacji** -iOS 10 zwiększa `NSUserActivity` do przechwytywania lokalizacje wyświetlić wewnątrz aplikacji i podwyższenie w wielu miejscach w systemie.
- **Kontekstowe żądań Siri**  -  `NSUserActivity` udostępnia kontekst do informacji przedstawionych wewnątrz aplikacji na używanie programu Siri, dzięki czemu użytkownik może uzyskać wskazówek lub być wywołanie wywoływanie Siri z poziomu aplikacji.

Wszystkie te funkcje mają wspólną rzecz, wszyscy korzystają z `NSUserActivity` w postaci jednego lub innym celu zapewnienia ich funkcjonalności. 

## <a name="nsuseractivity"></a>NSUserActivity

Jak wspomniano powyżej, `NSUserActivity` pomaga zrozumieć, jakie informacje na ekranie użytkownika aktualnie jest praca z systemu. `NSUserActivity` jest lekki buforowania informacji o stanie mechanizm przechwytywania aktywności użytkownika w trakcie nawigowania aplikacji. Na przykład przejrzenie restauracji aplikacji:

[![](proactive-suggestions-images/activity02.png "Restauracja aplikacji")](proactive-suggestions-images/activity02.png#lightbox)

Następujące zależności między:

1. Jako użytkownik współdziała z aplikacją, `NSUserActivity` jest tworzony, aby odtworzyć stanu aplikacji później.
2. Jeśli użytkownik wyszukuje restauracji, nastąpią tego samego wzorca tworzenia działań.
3. I ponownie, gdy użytkownik wyświetla wynik. W tym ostatnim przypadku użytkownik jest wyświetlanie lokalizacji i w systemie iOS 10, system jest bardziej pamiętać o pewnych pojęć (np. interakcje lokalizacji lub komunikacji).

Wykonaj bliższe spojrzenie na ekranie ostatnich:

[![](proactive-suggestions-images/activity03.png "Ładunek NSUserActivity")](proactive-suggestions-images/activity03.png#lightbox)

W tym miejscu aplikacji jest utworzenie `NSUserActivity` i zostały wprowadzone informacje, aby odtworzyć stan później. Aplikacja była dostępna również niektóre metadane, takie jak nazwa i adres lokalizacji. Z tego działania utworzone aplikacji pozwala dowiedzieć się, że reprezentuje jej bieżący stan użytkownika systemu iOS.

Aplikacja następnie decyduje, w przypadku działania anonsowane bezprzewodową dla przekazaniem, zapisany jako wartości tymczasowej lokalizacji masz sugestie lub dodane do indeksu Spotlight na urządzeniu do wyświetlania w wynikach wyszukiwania.

Aby uzyskać więcej informacji, wyszukaj przekazaniem i uwagi, zobacz nasze [wprowadzenie do programowi Handoff](~/ios/platform/handoff.md) i [systemu iOS 9 nowe interfejsy API wyszukiwania](~/ios/platform/search/index.md) przewodników.

### <a name="creating-an-activity"></a>Tworzenie działania

Przed utworzeniem działania, identyfikator typu działania będą potrzebne do utworzenia, aby zidentyfikować go. Identyfikator typu działania jest ciągiem krótkim dodane do `NSUserActivityTypes` aplikacji `Info.plist` plik używany do jednoznacznego identyfikowania danego typu działania użytkownika. W tablicy dla każdego działania aplikacja obsługująca i przedstawia do wyszukiwania aplikacji będzie zawierać jeden wpis. Zobacz nasze [Tworzenie odwołania do działania typu identyfikatory](~/ios/platform/search/nsuseractivity.md) więcej szczegółów.

Szukaj na przykład działanie:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Inform system of Activity
activity.BecomeCurrent();
```

Nowe działanie jest tworzony przy użyciu identyfikatora typu działania. Następnie niektóre metadane Definiowanie działanie jest tworzone ten stan można przywrócić w późniejszym terminie. Następnie działanie jest podany tytuł odzwierciedlający i dołączone do informacji o użytkowniku. Ponadto niektóre funkcje są włączone i działania są wysyłane do systemu.

Powyższy kod można wzbogacić dalsze uwzględniać metadanych, które dostarcza kontekst do działania, wprowadzając następujące zmiany:

```csharp
...

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Inform system of Activity
activity.BecomeCurrent();
```

Jeśli dewelopera ma witryny sieci Web, która jest w stanie wyświetlać te same informacje co aplikacja, aplikacja może zawierać adres URL i zawartości mogą być wyświetlane na innych urządzeniach, które nie mają zainstalowanej (za pośrednictwem przekazaniem) aplikacji:

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>Przywracanie działania

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

Upewnij się, to ten sam identyfikator typu działania (`com.xamarin.platform`) jako działania utworzone powyżej. Aplikacja korzysta z informacjami przechowywanymi w `NSUserActivity` do przywrócenia stanu, do których użytkownik przerwał pracę.

### <a name="benefits-of-creating-an-activity"></a>Korzyści wynikające z tworzenia działania

Przy minimalnej liczbie kod przedstawiony powyżej aplikacja jest teraz możliwe korzystanie z trzech nowych funkcji systemu iOS 10:

- **Handoff**
- **Zwracanie przez wyszukiwanie Spotlight**
- **Przypomnienia kontekstowe Siri**

Poniższa sekcja będzie Spójrz na włączenie dwóch innych nowych funkcji systemu iOS 10:

- **Sugestie dotyczące lokalizacji**
- **Używanie programu Siri kontekstowe żądań**

### <a name="location-based-suggestions"></a>Lokalizacja na podstawie sugestii 

Zająć przykład restauracji aplikacji wyszukiwania powyżej. Jeśli została ona zaimplementowana `NSUserActivity` i poprawnie wypełnione wszystkie metadane i atrybuty, użytkownik będzie mógł wykonywać następujące czynności:

1. Znajdź restauracji w aplikacji, które chciałby spełnia friend w.
4. Jeśli użytkownik zmienia się na aplikacji mapy, restauracji adres jest sugerowany automatycznie jako miejsca docelowego.
5. Działa to nawet w przypadku aplikacji firm 3 (obsługujące `NSUserActivity`), więc użytkownik może przełączać się do Udostępnianie innych aplikacji i adres restauracji jest sugerowany automatycznie jako miejsce docelowe istnieje również.
6. Zapewnia także kontekstu do Siri, więc użytkownik może wywołać Siri w restauracji aplikacji i poproś *"Get kierunki..."* i używanie programu Siri udostępni instrukcje restauracji przegląda użytkownika.

Jedyną operacją wszystkie powyższe funkcje ma wspólną, wszystkie wskazuje, gdzie sugestii pierwotnie pochodzi z. W przypadku powyższym przykładzie jest aplikacji przejrzyj fikcyjne restauracji.

Aby włączyć tę funkcję dla aplikacji za pośrednictwem kilku niewielkie modyfikacje i dodatki do istniejącej struktury została rozszerzona watchOS 3:

- `NSUserActivity` ma dodatkowe pola do przechwytywania informacji o lokalizacji, które są wyświetlane w aplikacji.
- Wprowadzono kilka dodatków MapKit i CoreSpotlight do przechwytywania lokalizacji.
- Funkcje świadomi lokalizacji dodano Siri, mapy, wielozadaniowości i inne aplikacje w systemie.

Aby zaimplementować lokalizacji na podstawie sugestii, rozpoczyna się od przedstawionych powyżej tego samego kodu działania:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");

// Inform system of Activity
activity.BecomeCurrent();
```

Jeśli aplikacja używa MapKit, jest tak proste, jak dodawanie bieżącej mapy `MKMapItem` do działania:

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

Jeśli aplikacja nie jest używany MapKit, jego przyjmują wyszukiwania aplikacji i określ następujące nowe atrybuty do lokalizacji:

```csharp
// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
...

attributes.NamedLocation = "Apple Inc.";
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

Spójrz na powyższy kod szczegółowo. Po pierwsze nazwę lokalizacji jest wymagany w każdym wystąpieniu:

```csharp
attributes.NamedLocation = "Apple Inc.";
```

Następnie na podstawie opisu w wymagany dla tekstu tekst na podstawie wystąpień (na przykład klawiatura QuickType):

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Współrzędne geograficzne są opcjonalne, ale upewnij się, że użytkownik jest kierowany do dokładnej lokalizacji aplikacji jest pożądane, aby wysłać je do:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Ustawiając numerów telefonów, aplikacji można uzyskać dostępu do Siri, więc użytkownik może wywołać Siri z aplikacji mówiąc wyglądać mniej więcej tak, * "Zadzwoń do tego miejsca":

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Na koniec aplikacja może wskazywać, czy wystąpienie jest odpowiedni dla nawigacji i telefony:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```
## <a name="activities-best-practices"></a>Najlepsze rozwiązania w zakresie działań

Podczas pracy z działaniami, Apple sugeruje następujące najlepsze rozwiązania:

- Użyj `NeedsSave` aktualizacji opóźnieniem ładunku.
- Upewnij się, aby zachować silne odwołanie do bieżącego działania.
- Transfer tylko małych ładunków, które obejmują tylko wystarczających informacji do przywrócenia stanu.
- Zapewnić działanie identyfikatory typów unikatowa i opisowa przy użyciu notacji wstecznego DNS, aby je określić. 

## <a name="consuming-location-suggestions"></a>Korzystanie z sugestii dotyczących lokalizacji

Następnej sekcji omówiono korzystanie z sugestii lokalizacji, które pochodzą z innymi częściami systemu (na przykład aplikacja mapy) lub innych 3 aplikacji firmy.

## <a name="routing-apps-and-locations-suggestions"></a>Aplikacje routingu i sugestie dotyczące lokalizacji

W tej sekcji zostanie Spójrz na korzystanie z sugestii lokalizacji bezpośrednio z poziomu aplikacji routingu. Dla aplikacji routingu dodać tę funkcję, deweloper będzie korzystać z istniejących `MKDirectionsRequest` framework w następujący sposób:

- Wspieranie aplikacji wielozadaniowości.
- Aby zarejestrować aplikację jako aplikację routingu.
- Do obsługi uruchamiania aplikacji z MapKit `MKDirectionsRequest` obiektu.
- Nadaj watchOS dowiedzieć się zasugerować aplikacji oparte na stopnia zaangażowania użytkowników.

Po uruchomieniu aplikacji z MapKit `MKDirectionsRequest` obiekt go powinien automatycznie uruchomić nadanie instrukcje użytkownika żądanej lokalizacji lub istnieje interfejs użytkownika, który ułatwia użytkownika rozpocząć pobieranie instrukcjami. Na przykład:


```csharp
using System;
using Foundation;
using UIKit;
using MapKit;
using CoreLocation;

namespace MonkeyChat
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate, IUISplitViewControllerDelegate
    {
        ...
        
        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            if (MKDirectionsRequest.IsDirectionsRequestUrl (url)) {
                var request = new MKDirectionsRequest (url);
                var coordinate = request.Destination?.Placemark.Location?.Coordinate;
                var address = request.Destination.Placemark.AddressDictionary;
                if (coordinate.IsValid()) {
                    var geocoder = new CLGeocoder ();
                    geocoder.GeocodeAddress (address, (place, err) => {
                        // Handle the display of the address

                    });
                }
            }

            return true;
        }
    }       
}
```

Spójrz na ten kod szczegółowo. Testów go wyświetlić, jeśli jest to żądanie prawidłowe miejsce docelowe:

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

Jeśli istnieje, a następnie tworzy `MKDirectionsRequest` z adresu URL:

```csharp
var request = new MKDirectionsRequest(url);
```

Nowość w watchOS 3 aplikacji mogą być wysyłane adres, który nie ma współrzędne geograficzną, w tym Przyczyna deweloper musi kodowania adresu:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="summary"></a>Podsumowanie

Ten artykuł ma pasuje do aktywnego sugestie i pokazano, jak dewelopera można używać ich do dysku ruch do aplikacji platformy Xamarin.iOS watchOS. Objęte krok, aby zaimplementować sugestie aktywne, a przedstawione wskazówki dotyczące użycia.


## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu watchOS](https://developer.xamarin.com/samples/watchos/all/)
- [Przewodnik programowania w języku SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
