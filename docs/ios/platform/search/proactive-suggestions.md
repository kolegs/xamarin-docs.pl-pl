---
title: Wprowadzenie do aktywnego sugestie
description: W tym artykule pokazano, jak użyj sugestii aktywnego w aplikacji platformy Xamarin.iOS zaangażowania dysku przez system aktywnego automatycznie przekazać pomocne informacje do użytkownika.
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 5b06dbf0e8e108616adb4f77910267aaa1ac71f4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-proactive-suggestions"></a>Wprowadzenie do aktywnego sugestie

_W tym artykule pokazano, jak użyj sugestii aktywnego w aplikacji platformy Xamarin.iOS zaangażowania dysku przez system aktywnego automatycznie przekazać pomocne informacje do użytkownika._

Jesteś nowym użytkownikiem iOS 10, aktywne sugestie obecny wiadomości sposoby użytkownikom współpracować z aplikacji platformy Xamarin.iOS przez aktywne obecny przydatne informacje automatycznie dla użytkownika w odpowiednim czasie.

iOS 10 przedstawia nowe sposoby pobudzenie zaangażowania do aplikacji przez system do aktywnego wyświetlane przydatne informacje automatycznie dla użytkownika w odpowiednim czasie. Tak jak w przypadku systemu iOS 9 podać możliwość dodawania wyszukiwania bezpośrednich do aplikacji przy użyciu Spotlight, przekazaniem i sugestie Siri (zobacz [nowe interfejsy API wyszukiwania](~/ios/platform/search/index.md)), z systemem iOS 10 aplikacji mogą uwidaczniać funkcje, które mogą być prezentowane użytkownikowi przez system z poziomu następujące lokalizacje:

- Przełącznik aplikacji
- Ekran blokady
- CarPlay
- Mapy
- Używanie programu Siri interakcji
- Sugestie QuickType

Aplikacja udostępnia tę funkcję do systemu przy użyciu kolekcji technologii, takich jak `NSUserActivity`, znaczników sieci web, Core Spotlight, MapKit, Media Player i UIKit. Ponadto zapewniając obsługę aktywnego sugestię aplikacji, pobiera lepsza integracja Siri bezpłatnie.

## <a name="location-based-suggestions"></a>Lokalizacja na podstawie sugestii

Jesteś nowym użytkownikiem iOS 10, `NSUserActivity` klasa zawiera `MapItem` właściwość, która umożliwia deweloperowi Podaj informacje o lokalizacji, które mogą być używane w innych kontekstach. Na przykład jeśli aplikacja wyświetla restauracji recenzji, deweloper można ustawić `MapItem` właściwości do lokalizacji restauracji, użytkownik jest wyświetlana w aplikacji. Jeśli użytkownik zmienia się na aplikacji mapy, restauracji lokalizacji jest automatycznie dostępny.

Jeśli aplikacja obsługuje wyszukiwania aplikacji, można użyć nowych składników adres `CSSearchableItemAttributesSet` klasę, aby określić lokalizacje, które użytkownik może odwiedzić. Przez ustawienie `MapItem` właściwości, inne właściwości są wypełniane automatycznie.

Oprócz ustawienia `Latitude` i `Longitude` adresu właściwości składnika, zalecane jest, że aplikacja podać `NamedLocation` i `PhoneNumbers` właściwości, więc Siri mogą inicjować wywołania do lokalizacji.

## <a name="web-markup-based-suggestions"></a>Kod znaczników sieci Web na podstawie sugestii

System iOS 9 dodane do możliwość uwzględnienia znaczników danych strukturalnych w witrynie sieci Web, który wzbogaca zawartość, która jest widoczne w wynikach wyszukiwania Spotlight oraz Safari (zobacz [wyszukiwania za pomocą znacznika sieci Web](~/ios/platform/search/web-markup.md)). iOS 10 dodaje możliwość uwzględnienia znaczników na podstawie lokalizacji (takie jak [adresu pocztowego](http://schema.org/PostalAddress) zgodnie z definicją w [Schema.org](http://schema.org/)) dodatkowo ulepszyć środowisko użytkownika. Na przykład jeśli widoków użytkowników, a lokalizacja oznaczone w witrynie, system może sugerować tej samej lokalizacji, kiedy adresat otworzy mapy.

## <a name="text-based-suggestions"></a>Tekst na podstawie sugestii

UIKit została rozszerzona w systemie iOS 10, aby uwzględnić [TextContentType](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) właściwość [UITextInputTraits](https://developer.apple.com/reference/uikit/uitextinputtraits) klasę, aby określić znaczenie semantyczne zawartości w obszarze tekstu. Dzięki tym informacjom w miejscu systemu można zwykle automatycznie wybierz odpowiedni typ klawiatury, Autokorekta sugestie dotyczące poprawy oraz aktywne integrować informacji z innych aplikacji i witryn sieci Web.

Na przykład, jeśli użytkownik wprowadza tekst w polu tekstowym oznaczone `UITextContentType.FullStreetAddress`, system może sugerować automatyczne wypełnianie pola z lokalizacją użytkownika został ostatnio wyświetlanie.

## <a name="media-based-suggestions"></a>Nośnik na podstawie sugestii

Jeśli aplikacja jest odtwarzany nośnika przy użyciu [MPPlayableContentManager](https://developer.xamarin.com/api/type/MediaPlayer.MPPlayableContentManager/) interfejsu API, iOS 10 pozwala użytkownikom na wyświetlanie albumów i odtwarzanie multimediów za pośrednictwem aplikacji na ekranie blokady.

## <a name="contextual-siri-reminders"></a>Przypomnienia kontekstowe Siri

Umożliwia użytkownikowi używaj Siri umożliwia szybkie przypomnieniami Aby wyświetlić zawartość się, że są obecnie wyświetlane w aplikacji w późniejszym terminie. Na przykład, jeśli zostały one wyświetlanie przeglądu restauracji w aplikacji, ich można wywołać Siri i wypowiedz *"Przypomnij mi na ten temat gdy pojawia się Strona główna".* Używanie programu Siri może wygenerować monitu z łączem do przeglądu w aplikacji.

## <a name="contact-based-suggestions"></a>Skontaktuj się z na podstawie sugestii

Umożliwia aplikacji kontaktów (i powiązane informacje kontaktowe) są wyświetlane w **skontaktuj się z** aplikację na urządzeniu z systemem iOS oraz wszystkie kontakty użytkowników przechowywanych w systemie.

## <a name="ride-sharing-based-suggestions"></a>Udostępnianie oparte na sugestie, które wywołują

Jeśli korzysta z aplikacji przez udostępnianie jazdy [MKDirectionsRequest](https://developer.xamarin.com/api/type/MapKit.MKDirectionsRequest/) interfejsu API, iOS 10 przedstawi go jako opcja w przełącznik aplikacji w czasie, gdy użytkownik prawdopodobnie mają jazdy. Aplikacja również musi być zarejestrowana jako aplikację sharing jazdy określając `MKDirectionsModeRideShare` dla [MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) klucza w jego `Info.plist` pliku.

Jeśli aplikacja obsługuje tylko innych udostępniania, sugestię systemu może rozpoczynać się od *"Get jazdy do..."*, jeśli są obsługiwane inne rodzaje kierunek routingu (na przykład Walking lub roweru), będą używane przez system *"Get instrukcje..."*

> [!IMPORTANT]
> [MKMapItem](https://developer.xamarin.com/api/type/MapKit.MKMapItem/) obiekt, który odbiera aplikacji nie może zawierać informacje o długości i szerokości geograficznej i wymaga wykonania geokodowania.

## <a name="implementing-proactive-suggestions"></a>Implementowanie sugestie aktywne

Dodawanie aktywnego sugestię wsparcie dla aplikacji platformy Xamarin.iOS jest zwykle tak proste, jak implementacja kilka interfejsów API lub rozszerzanie na kilka interfejsów API, które aplikacji może już być implementacja.

Aktywne sugestie dotyczące pracy z aplikacjami przede wszystkim na trzy sposoby:

- **`NSUserActivity` i Schema.org**  -  `NSUserActivity` pomaga zrozumieć, jakie informacje na ekranie użytkownika aktualnie jest praca z systemu. Schema.org dodaje możliwości podobne do stron sieci web.
- **Sugestie lokalizacji** — Jeśli aplikacja udostępnia lub wykorzystuje lokalizacji na podstawie informacji, te interfejsu API rozszerzenia oferta nowe sposoby udostępniania tych informacji w aplikacjach.
- **Sugestie aplikacji nośnika** — system podwyższyć poziom aplikacji i jego zawartości multimedialnej na podstawie kontekstu interakcji użytkownika z urządzeniem z systemem iOS.

I jest obsługiwany w aplikacji przez wykonania następujących czynności:

- **Programowi handoff**  -  `NSUserActivity` został dodany w systemie iOS 8 do obsługi przekazaniem, który umożliwia deweloperowi uruchomienia działania na jednym urządzeniu, a następnie kontynuować go na innym (zobacz [wprowadzenie do przekazaniem](~/ios/platform/handoff.md)).
- **Wyróżnione wyszukiwania** — system iOS 9 dodano możliwość wspierania zawartość aplikacji z poziomu wyników przez wyszukiwanie Spotlight za pomocą `NSUserActivity` (zobacz [wyszukiwania z wyróżnionym Core](~/ios/platform/search/corespotlight.md)).
- **Kontekstowe przypomnienia Siri** — w systemie iOS 10, `NSUserActivity` został rozszerzony w celu Zezwalaj na Siri szybko monitu do wyświetlenia zawartości, użytkownik jest aktualnie wyświetlane w aplikacji w późniejszym terminie.
- **Sugestie lokalizacji** -iOS 10 zwiększa `NSUserActivity` do przechwytywania lokalizacje wyświetlić wewnątrz aplikacji i podwyższenie w wielu miejscach w systemie.
- **Kontekstowe żądań Siri**  -  `NSUserActivity` udostępnia kontekst do informacji przedstawionych wewnątrz aplikacji na używanie programu Siri, dzięki czemu użytkownik może uzyskać wskazówek lub być wywołanie wywoływanie Siri z poziomu aplikacji.
- **Skontaktuj się z interakcje** — nowy w systemie iOS 10, `NSUserActivity` umożliwia aplikacjom komunikację można podwyższyć poziomu z karty kontaktu (w aplikacji kontaktów) jako metody alternatywnej komunikacji.

Wszystkie te funkcje mają wspólną rzecz, wszyscy korzystają z `NSUserActivity` w postaci jednego lub innym celu zapewnienia ich funkcjonalności. 

## <a name="nsuseractivity"></a>NSUserActivity

Jak wspomniano powyżej, `NSUserActivity` pomaga zrozumieć, jakie informacje na ekranie użytkownika aktualnie jest praca z systemu. `NSUserActivity` jest lekki buforowania informacji o stanie mechanizm przechwytywania aktywności użytkownika w trakcie nawigowania aplikacji. Na przykład przejrzenie aplikacji restauracji:

[![](proactive-suggestions-images/activity02.png "Stan lekki NSUserActivity mechanizm buforowania")](proactive-suggestions-images/activity02.png#lightbox)

Następujące zależności między:

1. Jako użytkownik współdziała z aplikacją, `NSUserActivity` jest tworzony, aby odtworzyć stanu aplikacji później.
2. Jeśli użytkownik wyszukuje restauracji, nastąpią tego samego wzorca tworzenia działań.
3. I ponownie, gdy użytkownik wyświetla wynik. W tym ostatnim przypadku użytkownik jest wyświetlanie lokalizacji i w systemie iOS 10, system jest bardziej pamiętać o pewnych pojęć (np. interakcje lokalizacji lub komunikacji).

Wykonaj bliższe spojrzenie na ekranie ostatnich:

[![](proactive-suggestions-images/activity03.png "Szczegóły NSUserActivity")](proactive-suggestions-images/activity03.png#lightbox)

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

Deweloper musi upewnij się, to ten sam identyfikator typu działania (`com.xamarin.platform`) jako działania utworzone powyżej. Aplikacja korzysta z informacjami przechowywanymi w `NSUserActivity` do przywrócenia stanu, do których użytkownik przerwał pracę.

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
2. Gdy użytkownik przechodzi od aplikacji przy użyciu wielozadaniowości przełącznik aplikacji, system automatycznie wyświetli sugestię (w dolnej części ekranu) Aby uzyskać instrukcje restauracji przy użyciu ich nawigacji ulubionych aplikacji.
3. Jeśli użytkownik przełącza się do aplikacji wiadomości i uruchamia, wpisując *"Umożliwia spełnia na"*, klawiatura QuickType automatycznie sugeruje wklejanie w adresie restauracji.
4. Jeśli użytkownik zmienia się na aplikacji mapy, restauracji adres jest sugerowany automatycznie jako miejsca docelowego.
5. Działa to nawet w przypadku aplikacji firm 3 (obsługujące `NSUserActivity`), więc użytkownik może przełączać się do Udostępnianie innych aplikacji i adres restauracji jest sugerowany automatycznie jako miejsce docelowe istnieje również.
6. Zapewnia także kontekstu do Siri, więc użytkownik może wywołać Siri w restauracji aplikacji i poproś *"Get kierunki..."* i używanie programu Siri udostępni instrukcje restauracji przegląda użytkownika.

Jedyną operacją wszystkie powyższe funkcje ma wspólną, wszystkie wskazuje, gdzie sugestii pierwotnie pochodzi z. W przypadku powyższym przykładzie jest aplikacji przejrzyj fikcyjne restauracji.

Aby włączyć tę funkcję dla aplikacji za pośrednictwem kilku niewielkie modyfikacje i dodatki do istniejącej struktury została rozszerzona iOS 10:

- `NSUserActivity` ma dodatkowe pola do przechwytywania informacji o lokalizacji, które są wyświetlane w aplikacji.
- Wprowadzono kilka dodatków MapKit i CoreSpotlight do przechwytywania lokalizacji.
- Funkcje świadomi lokalizacji dodano Siri, mapy, klawiatury, wielozadaniowości i inne aplikacje w systemie.

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

Następnie opis tekstowy na podstawie lokalizacji jest wymagany dla wystąpień na podstawie tekstu (na przykład klawiatura QuickType):

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Współrzędne geograficzne są opcjonalne, ale upewnij się, że użytkownik jest kierowany do dokładnej lokalizacji aplikacji jest chcą wysłać je do, więc należy włączyć:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Ustawiając numerów telefonów, aplikacji można uzyskać dostępu do Siri, więc użytkownik może wywołać Siri z aplikacji mówiąc wyglądać mniej więcej tak, *"Zadzwoń do tego miejsca"*:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Na koniec aplikacja może wskazywać, czy wystąpienie jest odpowiedni dla nawigacji i telefony:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>Implementowanie interakcje kontaktu

Nowy w systemie iOS 10, aplikacje komunikacji są ściśle zintegrowana w aplikacji kontakty z wizytówkę. Dla aplikacji, które zostały zaimplementowane interakcje kontaktu użytkownik może dodawać informacje o danej aplikacji do określonych osób w swoich kontaktów. A jeśli na przykład one kliknij przycisk szybkie działanie u góry karty, aby wysłać wiadomość, dołączone aplikacji zostaną wyświetlone jako opcję, aby wysłać komunikat.

Jeśli wybrano 3rd stron aplikacji, można zapamiętanych i widoczne jako domyślną metodą do komunikatu danej osoby, następnym razem, użytkownik będzie chciał skontaktować się z nimi.

Skontaktuj się z pomocą interakcji są implementowane za pomocą aplikacji `NSUserActivity` i nowej struktury intencje wprowadzone w systemie iOS 10. Więcej szczegółów na temat pracy z opcji, zobacz nasze [opis pojęć SiriKit](~/ios/platform/sirikit/understanding-sirikit.md) i [implementacja SiriKit](~/ios/platform/sirikit/implementing-sirikit.md) przewodników.

#### <a name="donating-interactions"></a>Rezygnacji interakcji

Spójrz na jak przekazać interakcji aplikacji:

[![](proactive-suggestions-images/activity04.png "Omówienie rezygnacji interakcji")](proactive-suggestions-images/activity04.png#lightbox)

Tworzy aplikację `INInteraction` obiekt, który zawiera **zamiar** (`INIntent`), **uczestników** i **metadanych**. **Zamiar** reprezentuje akcję użytkownika, takie jak wywołania wideo lub wysyłanie wiadomości SMS. **Uczestników** dołączyć osobom otrzymania powiadomienia. **Metadanych** definiuje dodatkowych informacji, takich jak pomyślnie wysyłania komunikatu itp.

Deweloper nigdy nie bezpośrednio tworzy wystąpienie `INIntent` lub `INIntentResponse`, używają jednej z klas określonych podrzędnych (na podstawie zadania aplikacji jest wykonywanie zadań w imieniu użytkownika) które dziedziczą z tych klas nadrzędnych. Na przykład `INSendMessageIntent` i `INSendMessageIntentResponse` do wysyłania wiadomości tekstowej. 

Gdy interakcji jest całkowicie wypełnione, wywołaj `DonateInteraction` metodę, aby poinformować system, który jest dostępny do użycia interakcji.

Gdy użytkownik wchodzi w interakcję z aplikacją z karty kontaktu, interakcji pobiera powiązany z `NSUserActivity`, który jest następnie używany do uruchamiania aplikacji:

[![](proactive-suggestions-images/activity05.png "Interakcja pobiera powiązany z NSUserActivity, który służy do uruchamiania aplikacji")](proactive-suggestions-images/activity05.png#lightbox)

Spójrz na poniższym przykładzie zamiar wysłać komunikat:

```csharp
using System;
using Foundation;
using UIKit;
using Intents;

namespace MonkeyNotification
{
    public class DonateInteraction
    {
        #region Constructors
        public DonateInteraction ()
        {
        }
        #endregion

        #region Public Methods
        public void SendMessageIntent (string text, INPerson from, INPerson[] to)
        {

            // Create App Activity
            var activity = new NSUserActivity ("com.xamarin.message");

            // Define details
            var info = new NSMutableDictionary ();
            info.Add (new NSString ("message"), new NSString (text));

            // Populate Activity
            activity.Title = "Sent MonkeyChat Message";
            activity.UserInfo = info;

            // Enable capabilities
            activity.EligibleForSearch = true;
            activity.EligibleForHandoff = true;
            activity.EligibleForPublicIndexing = true;

            // Inform system of Activity
            activity.BecomeCurrent ();

            // Create message Intent
            var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);

            // Create Intent Reaction
            var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);

            // Create interaction
            var interaction = new INInteraction (intent, response);

            // Donate interaction to the system
            interaction.DonateInteraction ((err) => {
                // Handle donation error
                ...
            });
        }
        #endregion
    }
}
```

Spojrzenie na ten kod szczegółowo, tworzy i wypełnia wystąpienia `NSUserActivity` (jak pokazano w [tworzenia działania](#Creating-an-Activity) powyższej sekcji). Następnie tworzy wystąpienie `INSendMessageIntent` (który dziedziczy z `INIntent`) i wypełnia treść wysyłanej wiadomości:

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

`INSendMessageIntentResponse` Jest tworzony i przekazany `NSUserActivity` utworzone powyżej:

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

`INInteraction` Składa się z zamiar wysłać wiadomość (`INSendMessageIntent`) i odpowiedzi (`INSendMessageIntentResponse`) utworzone:

```csharp
var interaction = new INInteraction (intent, response);
```

Na koniec system jest powiadomienie o interakcji:

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
    // Handle donation error
    ...
});
```

Obsługa uzupełniania jest przekazywany w których aplikacja może odpowiadać na pobrania powodzeniem lub niepowodzeniem.

### <a name="activities-best-practices"></a>Najlepsze rozwiązania w zakresie działań

Podczas pracy z działaniami, Apple sugeruje następujące najlepsze rozwiązania:

- Użyj `NeedsSave` aktualizacji opóźnieniem ładunku.
- Upewnij się, aby zachować silne odwołanie do bieżącego działania.
- Transfer tylko małych ładunków, które obejmują tylko wystarczających informacji do przywrócenia stanu.
- Zapewnić działanie identyfikatory typów unikatowa i opisowa przy użyciu notacji wstecznego DNS, aby je określić. 

## <a name="schemaorg"></a>Schema.org

Jak pokazano powyżej, `NSUserActivity` pomaga zrozumieć, jakie informacje na ekranie użytkownika aktualnie jest praca z systemu. Schema.org dodaje możliwości podobne do stron sieci web.

Schema.org zapewniają te same rodzaje interakcje lokalizacji witryny sieci Web. Apple zaprojektowane nowe sugestie lokalizacji do pracy równie dobrze podczas wyświetlania w programie Safari, tak jak w aplikacji natywnej.

Niektóre tła Schema.org:

- Zapewnia on standardu open web znaczników słownika.
- Jej działanie polega na tym metadanych strukturalnych na stronach sieci web.
- Istnieje ponad 500 schematy reprezentujących różne dostępne pojęcia.
- Wdrażając go w witrynie internetowej, deweloper może uzyskać niektóre korzyści wynikające ze stosowania `NSUserActivity` w natywnej aplikacji.

Schematy ułożone w drzewie, takich jak struktury, w którym określonych typów takich jak *restauracja*, dziedziczyć typy bardziej ogólne, takie jak *firm*. Aby uzyskać więcej informacji, zobacz [Schema.org](http://schema.org).

Na przykład, jeśli strona sieci web zawiera następujące dane:

```xml
<script type="application/ld+json>
{
    "@context":"http://schema.org",
    "@type":"Restaurant",
    "telephone":"(415) 781-1111",
    "url":"https://www.yanksing.com",
    "address":{
        "@type":"PostalAddress",
        "streetAddress":"101 Spear St",
        "addressLocality":"San Francisco",
        "postalCode":"94105",
        "addressRegion":"CA"
    },
    "aggregateRating":{
        "@type":"AggregateRating",
        "ratingValue":"3.5",
        "reviewCount":"2022"
    }
}
</script>
```

Jeśli użytkownik odwiedzin tej strony w programie Safari, a następnie przełączono do innej aplikacji, informacje o lokalizacji na stronie będzie przechwycony i oferowana jako sugestia lokalizacji w innych częściach systemu (jak pokazano powyżej działań).

Safari wyodrębni niczego na stronie sieci web zgodną do żadnej z następujących właściwości schematu:

- **Adresu pocztowego**
- **GeoCoordinates**
- Właściwości telefonu.

Aby uzyskać więcej informacji, zobacz nasze [wyszukiwania za pomocą znacznika sieci Web](~/ios/platform/search/web-markup.md) przewodnik.

## <a name="consuming-location-suggestions"></a>Korzystanie z sugestii dotyczących lokalizacji

Następnej sekcji omówiono korzystanie z sugestii lokalizacji, które pochodzą z innymi częściami systemu (na przykład aplikacja mapy) lub innych 3 aplikacji firmy.

Istnieją dwa sposoby głównego aplikacji, jaką może wykorzystać sugestie lokalizacji:

- Za pomocą klawiatury QuickType.
- Bezpośrednio przez wykorzystywanie informacji o lokalizacji w aplikacjach routingu.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>Lokalizacja sugestie i klawiatury QuickType

Jeśli aplikacja obsługuje adresy w formatach tekstowych na podstawie, aplikację można korzystać z sugestie lokalizacji za pośrednictwem interfejsu użytkownika QuickType. iOS 10 rozszerza QuickType interfejsu użytkownika przy użyciu następujących funkcji:

- Aplikację można dodać wskazówek dotyczących semantycznego przeznaczenia dla pól tekstowych w interfejsie użytkownika.
- Aplikacja może sugestie aktywne w aplikacji.
- Aplikacji mogą korzystać z rozszerzonego Autokorekta.

Nowe `TextContentType` właściwości formantów pola tekstowego w systemie iOS 10 umożliwia deweloperowi zdefiniuj semantycznego celem dla wartości, które użytkownik ma wprowadzić w danym polu. Na przykład:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

Czy wskaż system czy aplikacja oczekuje od użytkownika wprowadzenia pełnej ulicę w danym polu. Pozwoli to klawiatury QuickType można automatycznie udostępnić sugestie lokalizacji na klawiaturze, gdy użytkownik jest wprowadzenie wartości w tym polu.

Poniżej przedstawiono kilka typów częściej dostępne przez dewelopera w `UITextContentType` klasy statycznej:

* `Name`
* `GivenName`
* `FamilyName`
* `Location`
* `FullStreetAddress`
* `AddressCityAndState`
* `TelephoneNumber`
* `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>Aplikacje routingu i sugestie dotyczące lokalizacji

W tej sekcji zostanie Spójrz na korzystanie z sugestii lokalizacji bezpośrednio z poziomu aplikacji routingu. Dla aplikacji routingu dodać tę funkcję, deweloper będzie korzystać z istniejących `MKDirectionsRequest` framework w następujący sposób:

- Wspieranie aplikacji wielozadaniowości.
- Aby zarejestrować aplikację jako aplikację routingu.
- Do obsługi uruchamiania aplikacji z MapKit `MKDirectionsRequest` obiektu.
- Aby dać iOS dowiedzieć się zasugerować aplikacji dla użytkownika w odpowiednim czasie, na podstawie stopnia zaangażowania użytkowników.

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

Nowy w systemie iOS 10, aplikacji mogą być wysyłane adres, który nie ma współrzędne geograficzną, w tym Przyczyna deweloper musi kodowania adresu:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="media-app-suggestions"></a>Sugestie aplikacji nośnika

Jeśli aplikacja obsługuje dowolny typ nośnika, takie jak aplikacja podcast lub przesyłania strumieniowego zawartości nośnika, takich jak audio i wideo, z systemem iOS 10, można skorzystać z sugestii nośnika w systemie.

W przypadku aplikacji obsługujących nośnika z systemem iOS obsługuje następujące zachowania:

- iOS zamienia aplikacje, które użytkownik jest najczęściej używana na podstawie ich zachowania poprzedniej.
- Sugestie dotyczące aplikacji będą wyświetlane w centrum uwagi i widok dzisiaj.
- Jeśli aplikacja spełnia jednego z następujących wyzwalaczy, może towarzyszyć zwiększone sugestią ekranu blokady:
    - Po podłączenie słuchawki lub urządzenia Bluetooth nawiązuje połączenie.
    - Po pierwsze, w samochodu.
    - Po odbieranych w domu lub pracy. 

W tym proste wywołanie interfejsu API w systemie iOS 10, dewelopera umożliwia tworzenie angażujący środowisko ekranu blokady dla użytkowników aplikacji nośnika. Za pomocą `MPPlayableContentManager` klasy do zarządzania odtwarzanie multimediów, nośnik pełny formanty (na przykład przedstawiony przez aplikację utworów muzycznych) zostaną wyświetlone na ekranie blokady aplikacji.


```csharp
using System;
using MediaPlayer;
using UIKit;

namespace MonkeyPlayer
{
    public class PlayableContentDelegate : MPPlayableContentDelegate
    {
        #region Constructors
        public PlayableContentDelegate ()
        {
        }
        #endregion

        #region Override methods
        public override void InitiatePlaybackOfContentItem (MPPlayableContentManager contentManager, Foundation.NSIndexPath indexPath, Action<Foundation.NSError> completionHandler)
        {
            // Access the media item to play
            var item = LoadMediaItem (indexPath);

            // Populate the lock screen
            PopulateNowPlayingItem (item);

            // Prep item to be played
            var status = PreparePlayback (item);

            // Call completion handler
            completionHandler (null);
        }
        #endregion

        #region Public Methods
        public MPMediaItem LoadMediaItem (Foundation.NSIndexPath indexPath)
        {
            var item = new MPMediaItem ();

            // Load item from media store
            ...

            return item;
        }

        public void PopulateNowPlayingItem (MPMediaItem item)
        {
            // Get Info Center and album art
            var infoCenter = MPNowPlayingInfoCenter.DefaultCenter;
            var albumArt = (item.Artwork == null) ? new MPMediaItemArtwork (UIImage.FromFile ("MissingAlbumArt.png")) : item.Artwork;

            // Populate Info Center
            infoCenter.NowPlaying.Title = item.Title;
            infoCenter.NowPlaying.Artist = item.Artist;
            infoCenter.NowPlaying.AlbumTitle = item.AlbumTitle;
            infoCenter.NowPlaying.PlaybackDuration = item.PlaybackDuration;
            infoCenter.NowPlaying.Artwork = albumArt;
        }

        public bool PreparePlayback (MPMediaItem item)
        {
            // Prepare media item for playback
            ...

            // Return results
            return true;
        }
        #endregion
    }
}
```

## <a name="summary"></a>Podsumowanie

Ten artykuł ma pasuje do aktywnego sugestie i pokazano, jak deweloper może użyć ich do dysku ruch do aplikacji platformy Xamarin.iOS. Objęte krok, aby zaimplementować sugestie aktywne, a przedstawione wskazówki dotyczące użycia.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Przewodnik programowania w języku SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
