---
title: Implementowanie SiriKit
description: W tym artykule opisano kroki wymagane do zaimplementowania SiriKit pomocy technicznej w aplikacji platformy Xamarin.iOS.
ms.topic: article
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: a891e5bf797742ceb1bb45bb8144fa77dec99b2c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="implementing-sirikit"></a>Implementowanie SiriKit

_W tym artykule opisano kroki wymagane do zaimplementowania SiriKit pomocy technicznej w aplikacji platformy Xamarin.iOS._



Nowość w systemach iOS 10, SiriKit umożliwia aplikacji platformy Xamarin.iOS do świadczenia usług, które są dostępne dla użytkownika za pomocą aplikacja map i używanie programu Siri na urządzeniu z systemem iOS. W tym artykule opisano kroki wymagane do wykonania SiriKit pomocy technicznej w aplikacji platformy Xamarin.iOS przez dodanie wymaganych opcji rozszerzenia, rozszerzenia interfejsu użytkownika intencje i słownika.

Używanie programu Siri działa z pojęciem **domen**, grup znać akcji dla zadania pokrewne. Każdy interakcji, która aplikacja ma z Siri muszą należeć do jednej z jego domeny znanej usługi:

- Audio i wideo telefonicznej.
- Rezerwacji jazdy.
- Zarządzanie ćwiczeń.
- Do obsługi komunikatów.
- Wyszukiwanie zdjęcia.
- Wysyłanie lub odbieranie płatności.

Gdy użytkownik wysyła żądanie Siri odwołania rozszerzenia aplikacji usług, SiriKit wysyła rozszerzenia **zamiar** obiektu, który opisuje żądanie użytkownika wraz z danymi pomocniczych. Rozszerzenie aplikacji następnie generuje odpowiednie **odpowiedzi** obiektu dla danego **zamiar**, opisujące, jak rozszerzenie może obsłużyć żądania.

Ten przewodnik przedstawia prosty przykład, w tym obsługi SiriKit do istniejącej aplikacji. Dla tego przykładu będziemy używać fałszywych aplikacji MonkeyChat:

[ ![](implementing-sirikit-images/monkeychat01.png "Ikona MonkeyChat")](implementing-sirikit-images/monkeychat01.png)

MonkeyChat zachowuje własną kontaktów w książce znajomych użytkownika, każde skojarzone z nazwą ekranu (na przykład Bobo, na przykład) i umożliwia użytkownikowi wysyłanie rozmowy tekstu do każdego przyjazne nazwy ekranu.

## <a name="extending-the-app-with-sirikit"></a>Rozszerzanie aplikacji z SiriKit

Jak pokazano w [opis pojęć SiriKit](~/ios/platform/sirikit/understanding-sirikit.md) przewodniku obejmuje trzy główne części Rozszerzanie aplikacji za pomocą SiriKit:

[ ![](implementing-sirikit-images/elements01.png "Rozszerzanie aplikacji z SiriKit diagramu")](implementing-sirikit-images/elements01.png)

Należą do nich następujące elementy:

1. **Rozszerzenie intencje** — sprawdza, czy odpowiedzi użytkowników, sprawdza, aplikacja może obsłużyć żądania i faktycznie wykonuje zadanie, aby spełnić żądania użytkownika.
2. **Rozszerzenie interfejsu użytkownika intencje** - *opcjonalnie*, zapewnia niestandardowego interfejsu użytkownika do odpowiedzi w środowisku używanie programu Siri i może obejmować aplikacje interfejsu użytkownika i znakowanie na używanie programu Siri wzbogacić przez użytkownika.
3. **Aplikacja** — zapewnia vocabularies określonego użytkownika ułatwiają używanie programu Siri w aplikacji. 

Wszystkie te elementy i kroki, aby uwzględnić je w aplikacji zostanie omówiona szczegółowo w poniższych sekcjach.


## <a name="preparing-the-app"></a>Przygotowywanie aplikacji

SiriKit jest oparty na rozszerzeniach, przed dodaniem wszystkich rozszerzeń do aplikacji, istnieją jednak kilka rzeczy, które deweloper musi wykonać ułatwiające przyjęcia SiriKit.

### <a name="moving-common-shared-code"></a>Przenoszenie typowy kod udostępnionego 

Po pierwsze, dewelopera można przenosić niektóre typowe kodu, który będzie udostępniany między aplikacją i rozszerzenia do albo _udostępnionych projektów_, _przenośnej biblioteki klas (PCLs)_ lub _macierzystego Biblioteki_.

Rozszerzenia, należy wykonać wszystkie czynności, które aplikacja nie. W warunkach MonkeyChat przykładowej aplikacji, np. wyszukiwanie kontaktów, dodawanie nowych kontaktów, wysyłanie wiadomości i pobieranie historii wiadomości.

Przenosząc to wspólny kod do projektu udostępnionego, PCL lub natywnej biblioteki ułatwia Obsługa kodu w jednym miejscu typowe i zapewnia, że rozszerzenie i aplikacji nadrzędnej zapewniają uniform uruchomień oraz funkcje dla użytkownika.

W przypadku przykładową aplikację MonkeyChat modeli danych i przetwarzania kodu, takich jak dostęp do sieci i bazy danych zostaną przeniesione do natywnej biblioteki.

Wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom program Visual Studio dla komputerów Mac i otworzyć aplikację MonkeyChat.
2. Kliknij prawym przyciskiem myszy nazwę rozwiązania w **konsoli rozwiązania** i wybierz **Dodaj** > **nowy projekt...** : 

    [ ![](implementing-sirikit-images/prep01.png "Dodawanie nowego projektu")](implementing-sirikit-images/prep01.png)
3. Wybierz **iOS** > **biblioteki** > **biblioteki klas** i kliknij przycisk **dalej** przycisk: 

    [ ![](implementing-sirikit-images/prep02.png "Wybierz biblioteki klas")](implementing-sirikit-images/prep02.png)
4. Wprowadź `MonkeyChatCommon` dla **nazwa** i kliknij przycisk **Utwórz** przycisk: 

    [ ![](implementing-sirikit-images/prep03.png "Wprowadź nazwę MonkeyChatCommon")](implementing-sirikit-images/prep03.png)
5. Kliknij prawym przyciskiem myszy **odwołania** folder główny aplikacji w **Eksploratora rozwiązań** i wybierz **odwołuje się do edycji...** . Sprawdź **MonkeyChatCommon** projekt i kliknij przycisk **OK** przycisk: 

    [ ![](implementing-sirikit-images/prep05.png "Sprawdzanie projektu MonkeyChatCommon")](implementing-sirikit-images/prep05.png)
6. W **Eksploratora rozwiązań**, przeciągnij typowy kod udostępnionych z głównej aplikacji do natywnej biblioteki.
7. W przypadku MonkeyChat, przeciągnij **DataModels** i **procesorów** foldery z głównej aplikacji do natywnej biblioteki: 

    [ ![](implementing-sirikit-images/prep06.png "Foldery DataModels i procesorów w Eksploratorze rozwiązań")](implementing-sirikit-images/prep06.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Uruchom program Visual Studio i Otwórz aplikację MonkeyChat.
2. Kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksploratora rozwiązań** i wybierz **Dodaj** > **nowy projekt...** .
3. Wybierz **Visual C#** > **projektu udostępnionego** i kliknij przycisk **dalej** przycisk: 

    [ ![](implementing-sirikit-images/prep02w.png "Wybierz biblioteki klas")](implementing-sirikit-images/prep02w.png)
4. Wprowadź `MonkeyChatCommon` dla **nazwa** i kliknij przycisk **Utwórz** przycisku.
5. Kliknij prawym przyciskiem myszy **odwołania** folder główny aplikacji w **Eksploratora rozwiązań** i wybierz **odwołuje się do edycji...** . Sprawdź **MonkeyChatCommon** projekt i kliknij przycisk **OK** przycisk: 

    [ ![](implementing-sirikit-images/prep05w.png "Sprawdzanie projektu MonkeyChatCommon")](implementing-sirikit-images/prep05w.png)
6. W **Eksploratora rozwiązań**, przeciągnij typowy kod udostępnionych z głównej aplikacji do projektu udostępnionego.
7. W przypadku MonkeyChat, przeciągnij **DataModels** i **procesorów** foldery z głównej aplikacji do natywnej biblioteki.

-----

Edytuj dowolny z plików, które zostały przeniesione do natywnej biblioteki i zmienić przestrzeni nazw, aby był zgodny z biblioteki. Na przykład zmiana `MonkeyChat` do `MonkeyChatCommon`:

```csharp
using System;
namespace MonkeyChatCommon
{
    /// <summary>
    /// A message sent from one user to another within a conversation.
    /// </summary>
    public class MonkeyMessage
    {
        public MonkeyMessage ()
        {
        }
        ...
    }
}
```

Następnie wróć do głównej aplikacji i Dodaj `using` instrukcji dla przestrzeni nazw natywnej biblioteki dowolnym aplikacja korzysta z jednej z klas, które zostały przeniesione:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using MonkeyChatCommon;

namespace MonkeyChat
{
    public partial class MasterViewController : UITableViewController
    {
        public DetailViewController DetailViewController { get; set; }

        DataSource dataSource;
        ...
    }
}
```

### <a name="architecting-the-app-for-extensions"></a>Projektowania aplikacji dla rozszerzeń

Zazwyczaj aplikacji będzie Załóż wiele opcji, a deweloper musi upewnij się, że aplikacja została zaprojektowana dla odpowiedniej liczby celem rozszerzenia.

W sytuacji, gdy aplikacja wymaga więcej niż jeden profil konwersji deweloper może umieszczenie wszystkich jego obsługa konwersji w jedno rozszerzenie zamiar lub tworzenie oddzielnych rozszerzenia zamiar dla każdego celem.

Wyboru można utworzyć oddzielne rozszerzenia zamiar dla każdego zamiar, deweloper może przechodzili duplikowania dużą ilość schematyczny kod w poszczególnych rozszerzeń i Utwórz dużą ilość procesor i pamięć.

Aby wybrać jedną z dwóch opcji, zobacz, jeśli dowolne intencje naturalnie należy razem. Na przykład aplikację, który zgłosił połączeń audio i wideo mogą zostać uwzględnione obu tych opcji w rozszerzeniu zamierzone jednym, ponieważ są one obsługi podobne zadania i może dostarczyć większości ponowne użycie kodu.

Celem lub grupy lokalizacji docelowych, które nie pasują do istniejącej grupy należy utworzyć nowe rozszerzenie zamiar w rozwiązaniu aplikacji je.


### <a name="setting-the-required-entitlements"></a>Ustawianie uprawnień wymaganych

Dowolną aplikację platformy Xamarin.iOS zawierający integracji SiriKit muszą mieć prawidłowe uprawnienia ustawione. Jeśli projektanta nie prawidłowo te wymagane uprawnienia, nie będą w stanie zainstalować i przetestować aplikację na sprzęcie rzeczywistych iOS 10 (lub nowszego), który jest także wymaganie od 10 dla systemu iOS Simulator nie obsługuje SiriKit.

Wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij dwukrotnie `Entitlements.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
2. Przełącz się do **źródła** kartę.
3. Dodaj `com.apple.developer.siri` **właściwości**ustaw **typu** do `Boolean` i **wartość** do `Yes`: 

    [ ![](implementing-sirikit-images/setup01.png "Dodaj właściwość com.apple.developer.siri")](implementing-sirikit-images/setup01.png)
4. Zapisz zmiany w pliku.
5. Kliknij dwukrotnie **pliku projektu** w **Eksploratora rozwiązań** go otworzyć do edycji.
6. Wybierz **iOS podpisywania pakietu** i upewnij się, że `Entitlements.plist` plik jest zaznaczony na **uprawnień niestandardowych** pola: 

    [ ![](implementing-sirikit-images/setup02.png "Wybierz plik Entitlements.plist w polu uprawnienia niestandardowe")](implementing-sirikit-images/setup02.png)
7. Kliknij przycisk **OK** przycisk, aby zapisać zmiany.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij dwukrotnie `Entitlements.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
3. Dodaj `com.apple.developer.siri` **właściwości**ustaw **typu** do `Boolean` i **wartość** do `Yes`: 

    [ ![](implementing-sirikit-images/setup01w.png "Dodaj właściwość com.apple.developer.siri")](implementing-sirikit-images/setup01w.png)
4. Zapisz zmiany w pliku.
5. Kliknij dwukrotnie **pliku projektu** w **Eksploratora rozwiązań** go otworzyć do edycji.
6. Wybierz **iOS podpisywania pakietu** i upewnij się, że `Entitlements.plist` plik jest zaznaczony na **uprawnień niestandardowych** pola.

-----

Po zakończeniu instalacji aplikacji `Entitlements.plist` pliku powinna wyglądać następująco (w otwarty w edytorze zewnętrznego):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.siri</key>
    <true/>
</dict>
</plist>
```

### <a name="correctly-provisioning-the-app"></a>Poprawnie Inicjowanie obsługi aplikacji

Z powodu ograniczeniami zabezpieczeń, który Apple umieścił wokół framework SiriKit, dowolną aplikację platformy Xamarin.iOS, który implementuje SiriKit _musi_ mają poprawny identyfikator aplikacji i uprawnień (patrz sekcja powyżej) i muszą być podpisane przy odpowiedniego Profil inicjowania obsługi administracyjnej.

Wykonaj następujące czynności na komputerze Mac:

1. W przeglądarce sieci web, przejdź do [http://developer.apple.com](http://developer.apple.com) i zaloguj się do swojego konta.
2. Polecenie **certyfikaty**, **identyfikatory** i **profile**.
3. Wybierz **profile inicjowania obsługi** i wybierz **identyfikatorów aplikacji**, następnie kliknij przycisk  **+**  przycisku.
4. Wprowadź **nazwa** nowego profilu.
5. Wprowadź **identyfikator pakietu** firmy Apple po jego nazewnictwa zalecenia.
6. Przewiń w dół do **usługi aplikacji** zaznacz **SiriKit** i kliknij przycisk **Kontynuuj** przycisk: 

    [ ![](implementing-sirikit-images/setup03.png "Wybierz SiriKit")](implementing-sirikit-images/setup03.png)
7. Sprawdź wszystkie ustawienia, następnie **przesyłania** identyfikator aplikacji.
8. Wybierz **profile inicjowania obsługi** > **programowanie**, kliknij przycisk  **+**  przycisku Wybierz **identyfikator Apple ID**, następnie kliknij przycisk **Kontynuuj**.
9. Kliknij przycisk Wybierz **wszystkie**, następnie kliknij przycisk **Kontynuuj**.
10. Kliknij przycisk **Zaznacz wszystko** ponownie, następnie kliknij przycisk **Kontynuuj**.
11. Wprowadź **nazwa profilu** przy użyciu firmy Apple do nazw sugestii, kliknij przycisk **Kontynuuj**.
12. Uruchom środowisko Xcode.
13. Wybierz z Xcode menu **Preferencje...**
14. Wybierz **kont**, następnie kliknij przycisk **Wyświetl szczegóły...** przycisk: 

    [ ![](implementing-sirikit-images/setup04.png "Wybierz konta")](implementing-sirikit-images/setup04.png)
15. Kliknij przycisk **Pobierz wszystkie profile** przycisk w dolnym rogu po lewej stronie: 

    [ ![](implementing-sirikit-images/setup05.png "Pobierz wszystkie profile")](implementing-sirikit-images/setup05.png)
16. Upewnij się, że **profilu inicjowania obsługi administracyjnej** utworzone powyżej został zainstalowany w środowisku Xcode.
17. Otwórz projekt, aby dodać obsługę SiriKit do programu Visual Studio dla komputerów Mac.
18. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań**.
18. Upewnij się, że **identyfikator pakietu** jest zgodna ze strukturą utworzone w portalu dla deweloperów firmy Apple powyżej: 

    [ ![](implementing-sirikit-images/setup06.png "Identyfikator pakietu")](implementing-sirikit-images/setup06.png)
18. W **Eksploratora rozwiązań**, wybierz pozycję **projektu**.
19. Kliknij prawym przyciskiem myszy projekt i wybierz **opcje**.
21. Wybierz **iOS podpisywania pakietu**, wybierz pozycję **tożsamość podpisywania** i **profilu inicjowania obsługi administracyjnej** utworzone powyżej: 

    [ ![](implementing-sirikit-images/setup07.png "Wybierz tożsamość podpisywania i profilu inicjowania obsługi administracyjnej")](implementing-sirikit-images/setup07.png)
22. Kliknij przycisk **OK** przycisk, aby zapisać zmiany.

> [!IMPORTANT]
> **Uwaga:** SiriKit testowanie działa tylko na rzeczywistą iOS 10 urządzenia sprzętowego, a nie w systemie iOS 10 symulatora. Jeśli masz problemy z zainstalowaniem SiriKit włączona aplikacji platformy Xamarin.iOS na sprzęcie prawdziwe, upewnij się, czy wymagane uprawnienia, identyfikator aplikacji, identyfikator podpisywania i profilu inicjowania obsługi administracyjnej zostały poprawnie skonfigurowane w firmy Apple Developer Portal i Visual Studio dla komputerów Mac.

### <a name="requesting-siri-authorization"></a>Żąda autoryzacji Siri

Przed aplikacji dodaje wszystkie słownictwa określonego użytkownika lub rozszerzenia intencje łączy się Siri, jego żąda autoryzacji z użytkownikowi na dostęp Siri.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Edytowanie aplikacji `Info.plist` plików, przełącz się do **źródła** wyświetlanie i dodawanie `NSSiriUsageDescription` klucza o wartości ciągu, opisujące, jak aplikacja będzie używać Siri i jakie typy danych będą wysyłane. Na przykład aplikacji MonkeyChat powiedzieć "Kontakty MonkeyChat zostaną wysłane do Siri":

[ ![](implementing-sirikit-images/request01.png "NSSiriUsageDescription w edytorze Info.plist")](implementing-sirikit-images/request01.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Edytowanie aplikacji `Info.plist` plik i dodać `NSSiriUsageDescription` klucza o wartości ciągu, opisujące, jak aplikacja będzie używać Siri i jakie typy danych będą wysyłane. Na przykład aplikacji MonkeyChat powiedzieć "Kontakty MonkeyChat zostaną wysłane do Siri":

[ ![](implementing-sirikit-images/request01w.png "NSSiriUsageDescription w edytorze Info.plist")](implementing-sirikit-images/request01w.png)

-----

Wywołanie `RequestSiriAuthorization` metody `INPreferences` klasy po pierwszym uruchomieniu aplikacji. Edytuj `AppDelegate.cs` klasy i wprowadź `FinishedLaunching` wygląd metody podobne do poniższych:


```csharp
using Intents;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{

    // Request access to Siri
    INPreferences.RequestSiriAuthorization ((INSiriAuthorizationStatus status) => {
        // Respond to returned status
        switch (status) {
        case INSiriAuthorizationStatus.Authorized:
            break;
        case INSiriAuthorizationStatus.Denied:
            break;
        case INSiriAuthorizationStatus.NotDetermined:
            break;
        case INSiriAuthorizationStatus.Restricted:
            break;
        }
    });


    return true;
}
```

Ta metoda jest wywoływana, po raz pierwszy zostanie wyświetlony alert monitowanie użytkownika o Zezwalaj aplikacji na używanie programu Siri dostępu. Komunikat, który projektanta został dodany do `NSSiriUsageDescription` powyżej będą wyświetlane w tym alercie. Jeśli użytkownik nie początkowo zezwala na dostęp, może użyć **ustawienia** aplikacji, aby udzielić dostępu do aplikacji.

W dowolnym momencie aplikacji można sprawdzić aplikacji możliwość dostępu Siri przez wywołanie metody `SiriAuthorizationStatus` metody `INPreferences` klasy.

### <a name="localization-and-siri"></a>Lokalizacja i używanie programu Siri

Na urządzeniu z systemem iOS użytkownik będzie mógł wybrać język dla Siri, który jest inny, a następnie domyślnego systemu. Podczas pracy z ilości danych, aplikacji będą musieli używać `SiriLanguageCode` metody `INPreferences` klasy można pobrać kod języka z Siri. Na przykład:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>Dodawanie użytkownika określonego słownika.

Słownik określonego użytkownika będzie zapewnienie słów ani fraz, które są unikatowe dla poszczególnych użytkowników aplikacji. Zapewnia się one w czasie wykonywania z poziomu głównego aplikacji (nie rozszerzeń aplikacji) jako uporządkowany zestaw warunków, uporządkowanych w najbardziej znaczących priorytet użycia dla użytkowników, najważniejsze postanowienia na początku listy.

Słownik określonego użytkownika muszą należeć do jednej z następujących kategorii:

- Skontaktuj się z nazwy (które nie są zarządzane przez platformę, kontaktów).
- Tagi zdjęcie.
- Nazwy albumów zdjęć.
- Nazwy ćwiczeń.

Podczas wybierania terminologii, aby zarejestrować jako słownik niestandardowy, wybierz tylko warunki który może być błędnie interpretowane przez kogoś zapoznać się z aplikacją. Nigdy nie rejestru wspólne warunki, takie jak "Moje ćwiczeń" lub "Moje Album". Na przykład aplikacja MonkeyChat zarejestruje pseudonimy skojarzone z każdego kontakt w książce adresowej użytkownika.

Aplikacja zawiera słownictwa określonego użytkownika, wywołując `SetVocabularyStrings` metody `INVocabulary` klasy i przekazując `NSOrderedSet` kolekcji z głównego aplikacji. Aplikacja zawsze powinny wywoływać `RemoveAllVocabularyStrings` metody first, aby usunąć wszelkie istniejące warunki przed dodaniem nowych. Na przykład:

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Foundation;
using Intents;

namespace MonkeyChatCommon
{
    public class MonkeyAddressBook : NSObject
    {
        #region Computed Properties
        public List<MonkeyContact> Contacts { get; set; } = new List<MonkeyContact> ();
        #endregion

        #region Constructors
        public MonkeyAddressBook ()
        {
        }
        #endregion

        #region Public Methods
        public NSOrderedSet<NSString> ContactNicknames ()
        {
            var nicknames = new NSMutableOrderedSet<NSString> ();

            // Sort contacts by the last time used
            var query = Contacts.OrderBy (contact => contact.LastCalledOn);

            // Assemble ordered list of nicknames by most used to least
            foreach (MonkeyContact contact in query) {
                nicknames.Add (new NSString (contact.ScreenName));
            }

            // Return names
            return new NSOrderedSet<NSString> (nicknames.AsSet ());
        }

        // This method MUST only be called on a background thread!
        public void UpdateUserSpecificVocabulary ()
        {
            // Clear any existing vocabulary
            INVocabulary.SharedVocabulary.RemoveAllVocabularyStrings ();

            // Register new vocabulary
            INVocabulary.SharedVocabulary.SetVocabularyStrings (ContactNicknames (), INVocabularyStringType.ContactName);
        }
        #endregion
    }
}
```

Z tego kodu w miejscu mogą nadać mu w następujący sposób:

```csharp
using System;
using System.Threading;
using UIKit;
using MonkeyChatCommon;
using Intents;

namespace MonkeyChat
{
    public partial class ViewController : UIViewController
    {
        #region AppDelegate Access
        public AppDelegate ThisApp {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do we have access to Siri?
            if (INPreferences.SiriAuthorizationStatus == INSiriAuthorizationStatus.Authorized) {
                // Yes, update Siri's vocabulary
                new Thread (() => {
                    Thread.CurrentThread.IsBackground = true;
                    ThisApp.AddressBook.UpdateUserSpecificVocabulary ();
                }).Start ();
            }
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

> [!IMPORTANT]
> **Uwaga:** Siri traktuje słownika niestandardowego jako wskazówek i będzie zawierać tyle terminologii, jak to możliwe. Jednak miejsce do słownika niestandardowego jest ograniczona, dzięki czemu należy zarejestrować _tylko_ terminologii, która może być mylące, w związku z tym zachowaniu całkowita liczba zarejestrowanych warunków do minimum.

Aby uzyskać więcej informacji, zobacz nasze [odwołanie do użytkownika określonego słownika](~/ios/platform/sirikit/understanding-sirikit.md) i firmy Apple [Określanie niestandardowych słownictwa odwołanie](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

### <a name="adding-app-specific-vocabulary"></a>Dodawanie słownictwa określonych aplikacji

Słownik określonych aplikacji definiuje słów i wyrażeń, które będzie znane do wszystkich użytkowników aplikacji, takich jak typy pojazdów lub nazwy ćwiczeń. Ponieważ są to część aplikacji, są zdefiniowane w `AppIntentVocabulary.plist` pliku jako część pakietu głównego aplikacji. Ponadto można lokalizować tych słów i wyrażeń.

Warunki słownictwa określonych aplikacji muszą należeć do jednej z następujących kategorii:

- Opcje, które wywołują.
- Nazwy ćwiczeń.

Plik słownictwa określonych aplikacji zawiera dwa klucze poziomu głównego:

- `ParameterVocabularies` **Wymagane** — definiuje niestandardowe warunki i parametry opcje odnoszą się do aplikacji.
- `IntentPhrases` **Opcjonalne** — zawiera przykład fraz przy użyciu niestandardowych warunki zdefiniowane w `ParameterVocabularies`.

Każdy wpis `ParameterVocabularies` należy określić ciąg Identyfikatora, termin i zamiar, którego dotyczy termin. Ponadto pojedynczy termin mogą być stosowane do wielu opcji.

Aby uzyskać pełną listę dozwolonych wartości i struktura wymaganego pliku, zobacz firmy Apple [referencyjnym dotyczącym formatowania pliku słownictwa aplikacji](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

Aby dodać `AppIntentVocabulary.plist` plik do projektu aplikacji, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierz **Dodaj** > **nowego pliku...**   >  **iOS**:

    [ ![](implementing-sirikit-images/plist01.png "Dodaj listę właściwości")](implementing-sirikit-images/plist01.png) 
2. Kliknij dwukrotnie `AppIntentVocabulary.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
3. Kliknij przycisk  **+**  Aby dodać klucz, należy ustawić **nazwa** do `ParameterVocabularies` i **typu** do `Array`:

    [ ![](implementing-sirikit-images/plist02.png "Ustaw nazwę ParameterVocabularies i typ do tablicy")](implementing-sirikit-images/plist02.png)
4. Rozwiń węzeł `ParameterVocabularies` i kliknij przycisk  **+**  przycisk i ustaw **typu** do `Dictionary`:

    [ ![](implementing-sirikit-images/plist03.png "Ustaw typ do słownika")](implementing-sirikit-images/plist03.png)
5. Kliknij przycisk  **+**  Aby dodać nowy klucz, należy ustawić **nazwa** do `ParameterNames` i **typu** do `Array`:

    [ ![](implementing-sirikit-images/plist04.png "Ustaw nazwę ParameterNames i typ do tablicy")](implementing-sirikit-images/plist04.png)
6. Kliknij przycisk  **+**  Aby dodać nowy klucz o **typu** z `String` i wartość jako jeden z dostępnych nazw parametrów. Na przykład `INStartWorkoutIntent.workoutName`:

    [ ![](implementing-sirikit-images/plist05.png "Klucz INStartWorkoutIntent.workoutName")](implementing-sirikit-images/plist05.png)
7. Dodaj `ParameterVocabulary` klucza `ParameterVocabularies` klucza z **typu** z `Array`:

    [ ![](implementing-sirikit-images/plist06.png "Dodaj klucz ParameterVocabulary do klucza ParameterVocabularies z typem tablicy")](implementing-sirikit-images/plist06.png)
8. Dodaj nowy klucz o **typu** z `Dictionary`:

    [ ![](implementing-sirikit-images/plist07.png "Dodaj nowy klucz typu słownika")](implementing-sirikit-images/plist07.png)
9. Dodaj `VocabularyItemIdentifier` klucza z **typu** z `String` i określ unikatowy identyfikator terminu:

    [ ![](implementing-sirikit-images/plist08.png "Dodaj klucz VocabularyItemIdentifier z typu String i określ unikatowy identyfikator")](implementing-sirikit-images/plist08.png)
10. Dodaj `VocabularyItemSynonyms` klucza z **typu** z `Array`:

    [ ![](implementing-sirikit-images/plist09.png "Dodaj klucz VocabularyItemSynonyms z typem tablicy")](implementing-sirikit-images/plist09.png)
11. Dodaj nowy klucz o **typu** z `Dictionary`:

    [ ![](implementing-sirikit-images/plist10.png "Dodaj nowy klucz typu słownika")](implementing-sirikit-images/plist10.png)
12. Dodaj `VocabularyItemPhrase` klucza z **typu** z `String` i termin definiowania aplikacji:

    [ ![](implementing-sirikit-images/plist11.png "Dodaj klucz VocabularyItemPhrase z typu String i termin definiowania aplikacji")](implementing-sirikit-images/plist11.png)
13. Dodaj `VocabularyItemPronunciation` klucza z **typu** z `String` i Fonetyczne brzmienie termin:

    [ ![](implementing-sirikit-images/plist12.png "Dodaj klucz VocabularyItemPronunciation z typu String i Fonetyczne brzmienie termin")](implementing-sirikit-images/plist12.png)
14. Dodaj `VocabularyItemExamples` klucza z **typu** z `Array`:

    [ ![](implementing-sirikit-images/plist13.png "Dodaj klucz VocabularyItemExamples z typem tablicy")](implementing-sirikit-images/plist13.png)
15. Dodaj kilka `String` klucze przykład wykorzystania termin:

    [ ![](implementing-sirikit-images/plist14.png "Dodaj kilka kluczy będących ciągami przykład wykorzystania termin")](implementing-sirikit-images/plist14.png)
16. Powtórz powyższe kroki dla wszystkich innych warunków niestandardowych aplikacji, należy zdefiniować.
17. Zwiń `ParameterVocabularies` klucza.
18. Dodaj `IntentPhrases` klucza z **typu** z `Array`:

    [ ![](implementing-sirikit-images/plist15.png "Dodaj klucz IntentPhrases z typem tablicy")](implementing-sirikit-images/plist15.png)
19. Dodaj nowy klucz o **typu** z `Dictionary`:

    [ ![](implementing-sirikit-images/plist16.png "Dodaj nowy klucz typu słownika")](implementing-sirikit-images/plist16.png)
20. Dodaj `IntentName` klucza z **typu** z `String` i konwersji na przykład:

    [ ![](implementing-sirikit-images/plist17.png "Dodaj klucz IntentName z typu String i opcje dla przykładu")](implementing-sirikit-images/plist17.png)
21. Dodaj `IntentExamples` klucza z **typu** z `Array`:

    [ ![](implementing-sirikit-images/plist18.png "Dodaj klucz IntentExamples z typem tablicy")](implementing-sirikit-images/plist18.png)
22. Dodaj kilka `String` klucze przykład wykorzystania termin:

    [ ![](implementing-sirikit-images/plist19.png "Dodaj kilka kluczy będących ciągami przykład wykorzystania termin")](implementing-sirikit-images/plist19.png)
23. Powtórz powyższe kroki, aby uzyskać wszystkie profile aplikacji, musisz podać przykład użycia programu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierz **Dodaj** > **nowego pliku...**   >  **iOS**:

    [ ![](implementing-sirikit-images/plist01w.png "Dodaj nowy Info.plist")](implementing-sirikit-images/plist01w.png) 
2. Kliknij dwukrotnie `AppIntentVocabulary.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
3. Kliknij przycisk  **+**  Aby dodać klucz, należy ustawić **nazwa** do `ParameterVocabularies` i **typu** do `Array`:

    [ ![](implementing-sirikit-images/plist02w.png "Ustaw nazwę ParameterVocabularies i typ do tablicy")](implementing-sirikit-images/plist02w.png)
4. Rozwiń węzeł `ParameterVocabularies` i kliknij przycisk  **+**  przycisk i ustaw **typu** do `Dictionary`:

    [ ![](implementing-sirikit-images/plist03w.png "Ustaw typ do słownika")](implementing-sirikit-images/plist03w.png)
5. Kliknij przycisk  **+**  Aby dodać nowy klucz, należy ustawić **nazwa** do `ParameterNames` i **typu** do `Array`:

    [ ![](implementing-sirikit-images/plist04w.png "Ustaw nazwę ParameterNames i typ do tablicy")](implementing-sirikit-images/plist04w.png)
6. Kliknij przycisk  **+**  Aby dodać nowy klucz o **typu** z `String` i wartość jako jeden z dostępnych nazw parametrów. Na przykład `INStartWorkoutIntent.workoutName`:

    [ ![](implementing-sirikit-images/plist05w.png "Klucz INStartWorkoutIntent.workoutName")](implementing-sirikit-images/plist05w.png)
7. Dodaj `ParameterVocabulary` klucza `ParameterVocabularies` klucza z **typu** z `Array`:

    [ ![](implementing-sirikit-images/plist06w.png "Dodaj klucz ParameterVocabulary do klucza ParameterVocabularies z typem tablicy")](implementing-sirikit-images/plist06w.png)
8. Dodaj nowy klucz o **typu** z `Dictionary`:

    [ ![](implementing-sirikit-images/plist07w.png "Dodaj nowy klucz typu słownika")](implementing-sirikit-images/plist07w.png)
9. Dodaj `VocabularyItemIdentifier` klucza z **typu** z `String` i określ unikatowy identyfikator terminu:

    [ ![](implementing-sirikit-images/plist08w.png "Dodaj klucz VocabularyItemIdentifier z typu String i określ unikatowy identyfikator dla warunku")](implementing-sirikit-images/plist08w.png)
10. Dodaj `VocabularyItemSynonyms` klucza z **typu** z `Array`:

    [ ![](implementing-sirikit-images/plist09w.png "Dodaj klucz VocabularyItemSynonyms z typem tablicy")](implementing-sirikit-images/plist09w.png)
11. Dodaj nowy klucz o **typu** z `Dictionary`:

    [ ![](implementing-sirikit-images/plist10w.png "Dodaj nowy klucz typu słownika")](implementing-sirikit-images/plist10w.png)
12. Dodaj `VocabularyItemPhrase` klucza z **typu** z `String` i termin definiowania aplikacji:

    [ ![](implementing-sirikit-images/plist11w.png "Dodaj klucz VocabularyItemPhrase z typu String i termin definiowania aplikacji")](implementing-sirikit-images/plist11w.png)
13. Dodaj `VocabularyItemPronunciation` klucza z **typu** z `String` i Fonetyczne brzmienie termin:

    [ ![](implementing-sirikit-images/plist12w.png "Dodaj klucz VocabularyItemPronunciation z typu String i Fonetyczne brzmienie termin")](implementing-sirikit-images/plist12w.png)
14. Dodaj `VocabularyItemExamples` klucza z **typu** z `Array`:

    [ ![](implementing-sirikit-images/plist13w.png "Dodaj klucz VocabularyItemExamples z typem tablicy")](implementing-sirikit-images/plist13w.png)
15. Dodaj kilka `String` klucze przykład wykorzystania termin:

    [ ![](implementing-sirikit-images/plist14w.png "Dodaj kilka kluczy będących ciągami przykład wykorzystania termin")](implementing-sirikit-images/plist14w.png)
16. Powtórz powyższe kroki dla wszystkich innych warunków niestandardowych aplikacji, należy zdefiniować.
17. Zwiń `ParameterVocabularies` klucza.
18. Dodaj `IntentPhrases` klucza z **typu** z `Array`:

    [ ![](implementing-sirikit-images/plist15w.png "Dodaj klucz IntentPhrases z typem tablicy")](implementing-sirikit-images/plist15w.png)
19. Dodaj nowy klucz o **typu** z `Dictionary`:

    [ ![](implementing-sirikit-images/plist16w.png "Dodaj nowy klucz typu słownika")](implementing-sirikit-images/plist16w.png)
20. Dodaj `IntentName` klucza z **typu** z `String` i konwersji na przykład:

    [ ![](implementing-sirikit-images/plist17w.png "Dodaj klucz IntentName z typu String i opcje dla przykładu")](implementing-sirikit-images/plist17w.png)
21. Dodaj `IntentExamples` klucza z **typu** z `Array`:

    [ ![](implementing-sirikit-images/plist18w.png "Dodaj klucz IntentExamples z typem tablicy")](implementing-sirikit-images/plist18w.png)
22. Dodaj kilka `String` klucze przykład wykorzystania termin:

    [ ![](implementing-sirikit-images/plist19w.png "Dodaj kilka kluczy będących ciągami przykład wykorzystania termin")](implementing-sirikit-images/plist19w.png)
23. Powtórz powyższe kroki, aby uzyskać wszystkie profile aplikacji, musisz podać przykład użycia programu.

-----

> [!IMPORTANT]
> **Uwaga:** `AppIntentVocabulary.plist` zostanie zarejestrowany z Siri na testowej urządzeń podczas rozwoju i może zająć trochę czasu na używanie programu Siri uwzględnienie słownika niestandardowego. W związku z tym tester będzie konieczne Poczekaj kilka minut przed podjęciem próby test słownictwa określonych aplikacji, gdy została zaktualizowana.

Aby uzyskać więcej informacji, zobacz nasze [określonego słownika materiały referencyjne dotyczące aplikacji](~/ios/platform/sirikit/understanding-sirikit.md) i firmy Apple [Określanie niestandardowych słownictwa odwołanie](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

## <a name="adding-an-intents-extension"></a>Dodawanie rozszerzenia lokalizacji docelowych

Teraz, gdy aplikacja został przygotowany do przyjęcia SiriKit, deweloper konieczne będzie dodanie rozszerzenia intencje (przynajmniej jeden) do rozwiązania do obsługi opcji wymaganych do integracji Siri.

Dla każdego wymaganego rozszerzenia lokalizacji docelowych wykonaj następujące czynności:

- Dodaj projekt rozszerzenia intencje do rozwiązania aplikacji platformy Xamarin.iOS.
- Skonfiguruj rozszerzenie intencje `Info.plist` pliku.
- Modyfikowanie klasy głównym rozszerzenia lokalizacji docelowych.

Aby uzyskać więcej informacji, zobacz nasze [odwołania do rozszerzenia intencje](~/ios/platform/sirikit/understanding-sirikit.md) i firmy Apple [tworzenie odwołanie do rozszerzenia intencje](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1).

### <a name="creating-the-extension"></a>Tworzenie rozszerzenia

Aby dodać rozszerzenie intencje do rozwiązania, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij prawym przyciskiem myszy **Nazwa rozwiązania** w **konsoli rozwiązania** i wybierz **Dodaj** > **Dodawanie nowego projektu...** .
2. W oknie dialogowym wybierz **iOS** > **rozszerzenia** > **rozszerzenia zamiar** i kliknij przycisk **dalej** przycisk: 

    [ ![](implementing-sirikit-images/intents05.png "Wybierz rozszerzenie konwersji")](implementing-sirikit-images/intents05.png)
3. Następnie wprowadź **nazwa** rozszerzenia opcje i kliknij **dalej** przycisk: 

    [ ![](implementing-sirikit-images/intents06.png "Wprowadź nazwę dla opcji rozszerzenia")](implementing-sirikit-images/intents06.png)
4. Na koniec kliknij **Utwórz** przycisk, aby dodać rozszerzenie jako zamiar rozwiązania aplikacji: 

    [ ![](implementing-sirikit-images/intents07.png "Dodaj rozszerzenie zamiar rozwiązania aplikacji")](implementing-sirikit-images/intents07.png)
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **odwołania** folderu nowo utworzonego rozszerzenia celem. Sprawdź nazwę wspólnego udostępniony kod biblioteki projektu (aplikacji utworzone powyżej), a następnie kliknij przycisk **OK** przycisk: 

    [ ![](implementing-sirikit-images/intents08.png "Wybierz nazwę projektu wspólnego biblioteki udostępnionej kodu")](implementing-sirikit-images/intents08.png)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij prawym przyciskiem myszy **Nazwa rozwiązania** w **Eksploratora rozwiązań** i wybierz **Dodaj** > **Dodawanie nowego projektu...** .
2. W oknie dialogowym wybierz **iOS** > **rozszerzenia** > **rozszerzenia zamiar** i kliknij przycisk **dalej** przycisk: 

    [ ![](implementing-sirikit-images/intents05w.png "Wybierz rozszerzenie konwersji")](implementing-sirikit-images/intents05w.png)
3. Następnie wprowadź **nazwa** rozszerzenia opcje i kliknij **OK** przycisku.
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **odwołania** folderu nowo utworzonego rozszerzenia celem. Sprawdź nazwę wspólnego udostępniony kod biblioteki projektu (aplikacji utworzone powyżej), a następnie kliknij przycisk **OK** przycisk: 

    [ ![](implementing-sirikit-images/intents08w.png "Wybierz nazwę projektu wspólnego biblioteki udostępnionej kodu")](implementing-sirikit-images/intents08w.png)
    
-----

Powtórz te kroki dla wielu rozszerzeń zamiar (na podstawie [projektowania aplikacji dla rozszerzeń](#Architecting-the-App-for-Extensions) powyższej sekcji) wymagających aplikacji.

### <a name="configuring-the-infoplist"></a>Konfigurowanie Info.plist

Dla każdego rozszerzenia lokalizacji docelowych, które zostały dodane do rozwiązania aplikacji muszą być skonfigurowane w `Info.plist` plików do pracy z aplikacją.

Podobnie jak wszystkie typowe rozszerzenia aplikacji aplikacja będzie mieć istniejących kluczy `NSExtension` i `NSExtensionAttributes`. Dla opcji rozszerzenia istnieją dwa nowe atrybuty, które muszą być skonfigurowane:

[ ![](implementing-sirikit-images/intents01.png "Dwa nowe atrybuty, które muszą być skonfigurowane")](implementing-sirikit-images/intents01.png)

- **IntentsSupported** — jest wymagany i składa się z tablicę nazw zamiar klasy, które chce obsługiwać z celem rozszerzenia aplikacji.
- **IntentsRestrictedWhileLocked** — jest opcjonalny klucz dla aplikacji określić zachowanie ekranu blokady rozszerzenia. Zawiera tablicę nazw zamiar klasy, które aplikacji wymaga, aby wymagać od użytkownika zalogowania się do użycia z celem rozszerzenia.

Aby skonfigurować rozszerzenie zamiar `Info.plist` pliku, kliknij go dwukrotnie **Eksploratora rozwiązań** go otworzyć do edycji. Następnie należy przełączyć się do **źródła** wyświetlić, a następnie rozwiń węzeł `NSExtension` i `NSExtensionAttributes` klucze w edytorze:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![](implementing-sirikit-images/intents02.png "Klucze NSExtension i NSExtensionAttributes w edytorze")](implementing-sirikit-images/intents02.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](implementing-sirikit-images/intents02w.png "Klucze NSExtension i NSExtensionAttributes w edytorze")](implementing-sirikit-images/intents02w.png)

-----

Rozwiń węzeł `IntentsSupported` klucza, a następnie dodaj nazwę klasy zamiar, będzie obsługiwać tego rozszerzenia. Przykład MonkeyChat aplikacji, obsługuje `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![](implementing-sirikit-images/intents09.png "Klucz INSendMessageIntent")](implementing-sirikit-images/intents09.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](implementing-sirikit-images/intents09w.png "Klucz INSendMessageIntent")](implementing-sirikit-images/intents09w.png)

-----

Jeśli aplikacja wymaga opcjonalnie, użytkownik powinien być zalogowany do urządzenia do używania danego zamiar, rozwiń węzeł `IntentRestrictedWhileLocked` klucza i dodać lokalizacji docelowych, które mają ograniczony dostęp do nazwy klasy. Przykład MonkeyChat aplikacji, użytkownik musi być zalogowany wysłać wiadomość czatu tak dodaliśmy `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![](implementing-sirikit-images/intents10.png "Dodanym kluczu INSendMessageIntent")](implementing-sirikit-images/intents10.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](implementing-sirikit-images/intents10w.png "Dodanym kluczu INSendMessageIntent")](implementing-sirikit-images/intents10w.png)

-----


Aby uzyskać pełną listę dostępnych domen zamiar Zobacz firmy Apple [odwołania domen zamiar](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Konfigurowanie klasy głównym

Następnie dewelopera należy skonfigurować klasy głównym, która działa jako główny punkt wejścia dla rozszerzenia zamiar na używanie programu Siri. Musi być podklasą klasy `INExtension` zgodny ze `IINIntentHandler` delegowanie. Na przykład:

```csharp
using System;
using System.Collections.Generic;

using Foundation;
using Intents;

namespace MonkeyChatIntents
{
    [Register ("IntentHandler")]
    public class IntentHandler : INExtension, IINSendMessageIntentHandling, IINSearchForMessagesIntentHandling, IINSetMessageAttributeIntentHandling
    {
        #region Constructors
        protected IntentHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override NSObject GetHandler (INIntent intent)
        {
            // This is the default implementation.  If you want different objects to handle different intents,
            // you can override this and return the handler you want for that particular intent.

            return this;
        }
        #endregion
        ...
    }
}
```

Brak jednej metody solitary, którego aplikacja musi implementować klasy głównym celem rozszerzenia, `GetHandler` metody. Ta metoda jest przekazywana celem przez SiriKit, a aplikacja musi zwracać **obsługi zamiar** odpowiedniego dla typu danego celem.

Ponieważ przykładową aplikację MonkeyChat obsługuje tylko jeden zamiar, aplikacja zwraca się we `GetHandler` metody. Jeśli rozszerzenie obsługi więcej niż jeden profil konwersji, deweloper czy dodać klasę dla każdego typu konwersji i zwrócić wystąpienia tutaj na podstawie `Intent` przekazywany do metody.

### <a name="handling-the-resolve-stage"></a>Obsługa etap rozwiązanie

W ramach rozwiązania jest gdzie rozszerzenia zamiar będzie wyjaśnienia i sprawdza poprawność parametrów przekazanych z Siri i zostały ustawione przez użytkownika konwersacji.

Dla każdego parametru są wysyłane z Siri, Brak `Resolve` metody. Aplikację należy zaimplementować tę metodę dla każdego parametru, że aplikacja może być konieczne używanie programu Siri na help, aby uzyskać poprawny odpowiedzi od użytkownika.

W przypadku przykładową aplikację MonkeyChat rozszerzenia zamiar będzie wymagać co najmniej jednego adresata do wysłania tej wiadomości do. Dla każdego adresata na liście rozszerzenia trzeba wykonać wyszukiwanie kontaktów, który może mieć następujące wyniki:

- Znaleziono dokładnie jeden pasujący kontakt.
- Znaleziono co najmniej dwa pasujące kontakty.
- Nie znaleziono żadnych zgodnych kontaktów.

Ponadto MonkeyChat wymaga zawartości dla treści wiadomości. Jeśli użytkownik nie ma podany to Siri musi Monituj użytkownika o zawartości.

Rozszerzenie zamiar musi obsługiwać wszystkich tych przypadkach.


```csharp
[Export ("resolveRecipientsForSearchForMessages:withCompletion:")]
public void ResolveRecipients (INSendMessageIntent intent, Action<INPersonResolutionResult []> completion)
{
    var recipients = intent.Recipients;
    // If no recipients were provided we'll need to prompt for a value.
    if (recipients.Length == 0) {
        completion (new INPersonResolutionResult [] { (INPersonResolutionResult)INPersonResolutionResult.NeedsValue });
        return;
    }

    var resolutionResults = new List<INPersonResolutionResult> ();

    foreach (var recipient in recipients) {
        var matchingContacts = new INPerson [] { recipient }; // Implement your contact matching logic here to create an array of matching contacts
        if (matchingContacts.Length > 1) {
            // We need Siri's help to ask user to pick one from the matches.
            resolutionResults.Add (INPersonResolutionResult.GetDisambiguation (matchingContacts));
        } else if (matchingContacts.Length == 1) {
            // We have exactly one matching contact
            resolutionResults.Add (INPersonResolutionResult.GetSuccess (recipient));
        } else {
            // We have no contacts matching the description provided
            resolutionResults.Add ((INPersonResolutionResult)INPersonResolutionResult.Unsupported);
        }
    }

    completion (resolutionResults.ToArray ());
}

[Export ("resolveContentForSendMessage:withCompletion:")]
public void ResolveContent (INSendMessageIntent intent, Action<INStringResolutionResult> completion)
{
    var text = intent.Content;
    if (!string.IsNullOrEmpty (text))
        completion (INStringResolutionResult.GetSuccess (text));
    else
        completion ((INStringResolutionResult)INStringResolutionResult.NeedsValue);
}
```

Aby uzyskać więcej informacji, zobacz nasze [rozwiązać etap Reference](~/ios/platform/sirikit/understanding-sirikit.md) i firmy Apple [obsługi odwołanie intencje i rozwiązywanie](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1).

### <a name="handling-the-confirm-stage"></a>Obsługa etap Potwierdź

Potwierdź etap jest gdzie rozszerzenia zamiar sprawdza, czy zawiera wszystkie informacje w odpowiedzi na żądanie użytkownika. Aplikacja chce wysłać potwierdzenie wzdłuż zostaną wszystkie pomocnicze szczegóły co to jest o wystąpić na używanie programu Siri, może być potwierdza z użytkownikiem przez to jest zamierzone akcji.

```csharp
[Export ("confirmSendMessage:completion:")]
public void ConfirmSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Verify user is authenticated and the app is ready to send a message.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Ready, userActivity);
    completion (response);
}
```

Aby uzyskać więcej informacji, zobacz nasze [potwierdzić etap Reference](~/ios/platform/sirikit/understanding-sirikit.md).

### <a name="processing-the-intent"></a>Przetwarzanie celem

To jest punkt, w którym rozszerzenia zamiar faktycznie wykonuje zadanie do zrealizowania żądania użytkownika i przekazywać wyników Siri, użytkownik może mieć świadomość.


```csharp
public void HandleSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Implement the application logic to send a message here.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, userActivity);
    completion (response);
}

public void HandleSearchForMessages (INSearchForMessagesIntent intent, Action<INSearchForMessagesIntentResponse> completion)
{
    // Implement the application logic to find a message that matches the information in the intent.
    ...

    var userActivity = new NSUserActivity (nameof (INSearchForMessagesIntent));
    var response = new INSearchForMessagesIntentResponse (INSearchForMessagesIntentResponseCode.Success, userActivity);

    // Initialize with found message's attributes
    var sender = new INPerson (new INPersonHandle ("sarah@example.com", INPersonHandleType.EmailAddress), null, "Sarah", null, null, null);
    var recipient = new INPerson (new INPersonHandle ("+1-415-555-5555", INPersonHandleType.PhoneNumber), null, "John", null, null, null);
    var message = new INMessage ("identifier", "I am so excited about SiriKit!", NSDate.Now, sender, new INPerson [] { recipient });
    response.Messages = new INMessage [] { message };
    completion (response);
}

public void HandleSetMessageAttribute (INSetMessageAttributeIntent intent, Action<INSetMessageAttributeIntentResponse> completion)
{
    // Implement the application logic to set the message attribute here.
    ...

    var userActivity = new NSUserActivity (nameof (INSetMessageAttributeIntent));
    var response = new INSetMessageAttributeIntentResponse (INSetMessageAttributeIntentResponseCode.Success, userActivity);
    completion (response);
}
```

Aby uzyskać więcej informacji, zobacz nasze [obsługi etap Reference](~/ios/platform/sirikit/understanding-sirikit.md).

## <a name="adding-an-intents-ui-extension"></a>Dodawanie intencje rozszerzenia interfejsu użytkownika

Opcjonalne rozszerzenie interfejsu użytkownika intencje przedstawia informacje o możliwość dostosowania interfejsu użytkownika aplikacji i znakowanie do obsługi Siri i upewnij użytkowników możesz podłączonej do aplikacji. Z tym rozszerzeniem aplikację można przełączyć marki, a także visual i innych informacji do zapisu.

[ ![](implementing-sirikit-images/intentsui01.png "Przykładowe dane wyjściowe intencje rozszerzenie interfejsu użytkownika")](implementing-sirikit-images/intentsui01.png)

Podobnie jak rozszerzenie intencje dewelopera wykona następny krok dla opcji rozszerzenia interfejsu użytkownika:

- Dodaj projekt intencje rozszerzenia interfejsu użytkownika do rozwiązania aplikacji platformy Xamarin.iOS.
- Skonfiguruj rozszerzenie interfejsu użytkownika intencje `Info.plist` pliku.
- Modyfikowanie klasy głównym intencje rozszerzenie interfejsu użytkownika.

Aby uzyskać więcej informacji, zobacz nasze [intencje Reference rozszerzenie interfejsu użytkownika](~/ios/platform/sirikit/understanding-sirikit.md) i firmy Apple [podając odwołanie do interfejsu niestandardowe](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1).

### <a name="creating-the-extension"></a>Tworzenie rozszerzenia

Aby dodać rozszerzenie interfejsu użytkownika intencje do rozwiązania, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij prawym przyciskiem myszy **Nazwa rozwiązania** w **konsoli rozwiązania** i wybierz **Dodaj** > **Dodawanie nowego projektu...** .
2. W oknie dialogowym wybierz **iOS** > **rozszerzenia** > **rozszerzenie interfejsu użytkownika zamiar** i kliknij przycisk **dalej** przycisk: 

    [ ![](implementing-sirikit-images/intents11.png "Wybierz rozszerzenie konwersji interfejsu użytkownika")](implementing-sirikit-images/intents11.png)
3. Następnie wprowadź **nazwa** rozszerzenia opcje i kliknij **dalej** przycisk: 

    [ ![](implementing-sirikit-images/intents12.png "Wprowadź nazwę dla opcji rozszerzenia")](implementing-sirikit-images/intents12.png)
4. Na koniec kliknij **Utwórz** przycisk, aby dodać rozszerzenie jako zamiar rozwiązania aplikacji: 

    [ ![](implementing-sirikit-images/intents13.png "Dodaj rozszerzenie zamiar rozwiązania aplikacji")](implementing-sirikit-images/intents13.png)
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **odwołania** folderu nowo utworzonego rozszerzenia celem. Sprawdź nazwę wspólnego udostępniony kod biblioteki projektu (aplikacji utworzone powyżej), a następnie kliknij przycisk **OK** przycisk: 

    [ ![](implementing-sirikit-images/intents14.png "Wybierz nazwę projektu wspólnego biblioteki udostępnionej kodu")](implementing-sirikit-images/intents14.png)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij prawym przyciskiem myszy **Nazwa rozwiązania** w **Eksploratora rozwiązań** i wybierz **Dodaj** > **Dodawanie nowego projektu...**
2. W oknie dialogowym wybierz **iOS** > **rozszerzenia** > **rozszerzenie interfejsu użytkownika zamiar** i kliknij przycisk **dalej** przycisk.
3. Następnie wprowadź **nazwa** rozszerzenia opcje i kliknij **OK** przycisku.
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **odwołania** folderu nowo utworzonego rozszerzenia celem. Sprawdź nazwę wspólnego udostępniony kod biblioteki projektu (aplikacji utworzone powyżej), a następnie kliknij przycisk **OK** przycisku.
    
-----

### <a name="configuring-the-infoplist"></a>Konfigurowanie Info.plist

Skonfiguruj rozszerzenie interfejsu użytkownika intencje `Info.plist` pliku do pracy z aplikacją.

Podobnie jak wszystkie typowe rozszerzenia aplikacji aplikacja będzie mieć istniejących kluczy `NSExtension` i `NSExtensionAttributes`. Dla opcji rozszerzenia istnieje jeden nowy atrybut, który musi być skonfigurowany:

[ ![](implementing-sirikit-images/intents03.png "Jeden nowy atrybut, który musi być skonfigurowany")](implementing-sirikit-images/intents03.png)

**IntentsSupported** jest wymagany i składa się z tablicą nazwy klas zamiar aplikacji mają być obsługiwane z celem rozszerzenia.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby skonfigurować rozszerzenie interfejsu użytkownika zamiar `Info.plist` pliku, kliknij go dwukrotnie **Eksploratora rozwiązań** go otworzyć do edycji. Następnie należy przełączyć się do **źródła** wyświetlić, a następnie rozwiń węzeł `NSExtension` i `NSExtensionAttributes` klucze w edytorze:

[ ![](implementing-sirikit-images/intents04.png "Klucze NSExtension i NSExtensionAttributes w edytorze")](implementing-sirikit-images/intents04.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby skonfigurować rozszerzenie interfejsu użytkownika zamiar `Info.plist` pliku, kliknij go dwukrotnie **Eksploratora rozwiązań** go otworzyć do edycji. Rozwiń węzeł `NSExtension` i `NSExtensionAttributes` klucze w edytorze:

[ ![](implementing-sirikit-images/intents04w.png "Klucze NSExtension tnie i NSExtensionAttributes w edytorze")](implementing-sirikit-images/intents04w.png)

-----

Rozwiń węzeł `IntentsSupported` klucza, a następnie dodaj nazwę klasy zamiar, będzie obsługiwać tego rozszerzenia. Przykład MonkeyChat aplikacji, obsługuje `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![](implementing-sirikit-images/intents15.png "Klucz INSendMessageIntent")](implementing-sirikit-images/intents15.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](implementing-sirikit-images/intents15w.png "Klucz INSendMessageIntent")](implementing-sirikit-images/intents15w.png)

-----

Aby uzyskać pełną listę dostępnych domen zamiar Zobacz firmy Apple [odwołania domen zamiar](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Konfigurowanie klasy głównym

Konfigurowanie klasy głównym, która działa jako główny punkt wejścia dla celem rozszerzenia interfejsu użytkownika na używanie programu Siri. Musi być podklasą klasy `UIViewController` zgodny ze `IINUIHostedViewController` interfejsu. Na przykład:

```csharp
using System;
using Foundation;
using CoreGraphics;
using Intents;
using IntentsUI;
using UIKit;

namespace MonkeyChatIntentsUI
{
    public partial class IntentViewController : UIViewController, IINUIHostedViewControlling
    {
        #region Constructors
        protected IntentViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }

        public override void DidReceiveMemoryWarning ()
        {
            // Releases the view if it doesn't have a superview.
            base.DidReceiveMemoryWarning ();

            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Public Methods
        [Export ("configureWithInteraction:context:completion:")]
        public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
        {
            // Do configuration here, including preparing views and calculating a desired size for presentation.

            if (completion != null)
                completion (DesiredSize ());
        }

        [Export ("desiredSize:")]
        public CGSize DesiredSize ()
        {
            return ExtensionContext.GetHostedViewMaximumAllowedSize ();
        }
        #endregion
    }
}
```

Przekazuje Siri `INInteraction` wystąpienia klasy `Configure` metody `UIViewController` wystąpienia wewnątrz zamiar rozszerzenie interfejsu użytkownika.

`INInteraction` Obiektu zawiera trzy kluczowych informacji do rozszerzenia:

1. Obiekt konwersji przetwarzane.
2. Obiekt odpowiedzi zamiar `Confirm` i `Handle` metody rozszerzenia celem.
3. Stan interakcji, który definiuje stan interakcji między aplikacją i używanie programu Siri.

`UIViewController` Wystąpienie jest klasa zasady do interakcji z Siri i ponieważ dziedziczy on z `UIViewController`, ma dostęp do wszystkich funkcji UIKit.

Gdy wywołuje Siri `Configure` metody `UIViewController` przekazaniem w kontekście widoku informujący, że kontroler widoku albo będzie obsługiwana w Siri Snippit lub karty mapy.

Używanie programu Siri również zostaną spełnione programu obsługi zakończenia, która aplikacja musi zwracać wymagany rozmiar widoku, gdy aplikacja zakończeniu jego konfigurowania.

### <a name="design-the-ui-in-ios-designer"></a>Projektowanie interfejsu użytkownika w systemie iOS projektanta

Układ interfejsu użytkownika intencje rozszerzenie interfejsu użytkownika w systemie iOS projektanta. Kliknij dwukrotnie rozszerzenia `MainInterface.storyboard` w pliku **Eksploratora rozwiązań** go otworzyć do edycji. Przeciągnij we wszystkich wymaganych elementów interfejsu użytkownika do tworzenia interfejsu użytkownika i zapisać zmiany.

> [!IMPORTANT]
> **Uwaga:** zablokowaniu można dodać elementy interaktywne, takie jak `UIButtons` lub `UITextFields` rozszerzenie interfejsu użytkownika zamiar `UIViewController`, te są ściśle zabroniony jako zamiar interfejsu użytkownika w nieinterakcyjnym, a użytkownik nie będzie obsługiwać interakcję z nimi.

### <a name="wire-up-the-user-interface"></a>Podczas transmisji w górę interfejsu użytkownika

Rozszerzenie interfejsu użytkownika intencje interfejsu użytkownika utworzone w Projektancie systemu iOS, należy edytować `UIViewController` podklasy i zastąpienie `Configure` metody w następujący sposób:

```csharp
[Export ("configureWithInteraction:context:completion:")]
public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
{
    // Do configuration here, including preparing views and calculating a desired size for presentation.
    ...

    // Return desired size
    if (completion != null)
        completion (DesiredSize ());
}

[Export ("desiredSize:")]
public CGSize DesiredSize ()
{
    return ExtensionContext.GetHostedViewMaximumAllowedSize ();
}
```

### <a name="overriding-the-default-siri-ui"></a>Zastępowanie domyślnego interfejsu użytkownika Siri

Rozszerzenie interfejsu użytkownika intencje będą zawsze wyświetlane wraz z inną zawartością Siri, taką jak ikona aplikacji i nazwy w górnej części interfejsu użytkownika lub, oparte na zamiar, przyciski (na przykład wysyłanie lub Anuluj) mogą być wyświetlane u dołu.

Istnieje kilka wystąpień, gdzie aplikacja może zastąpić informacje, które Siri są wyświetlane użytkownikowi domyślnie, takie jak wiadomości lub mapy, gdzie aplikacji można zastąpić domyślne środowisko z jednym dostosowane do aplikacji.

Jeśli rozszerzenie interfejsu użytkownika intencje musi zastąpić elementów domyślnych Siri interfejsu użytkownika, `UIViewController` podklasy należy zaimplementować `IINUIHostedViewSiriProviding` interfejsu i wyrazić zgodę na wyświetlanie elementu konkretnego interfejsu.

Dodaj następujący kod do `UIViewController` podklasy mówić Siri zamiar rozszerzenie interfejsu użytkownika jest już wyświetlana zawartość wiadomości:

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>Uwagi

Apple sugeruje, że deweloper wziąć pod uwagę podczas projektowania i implementowania rozszerzeń interfejsu użytkownika zamiar następujące zagadnienia:

- **Być świadome z użycia pamięci** — ponieważ rozszerzenia interfejsu użytkownika celem są tymczasowe i tylko przedstawiono krótki czas, system nakłada zwiększenie poziomu ograniczeń pamięci, niż są używane w pełnej aplikacji.
- **Należy wziąć pod uwagę minimalny i maksymalny rozmiar widoku** — upewnij się, że celem rozszerzenia interfejsu użytkownika wygląda dobrze na każdy typ urządzenia z systemem iOS, rozmiaru i orientacji. Ponadto wymagany rozmiar, który aplikacja wysyła do Siri może nie móc otrzymać.
- **Elastyczne i adaptacyjną wzorce układu** — ponownie, aby upewnić się, że interfejs użytkownika wygląda doskonale na każdym urządzeniu.

## <a name="summary"></a>Podsumowanie

W tym artykule został objęty SiriKit i pokazano, jak można dodać do aplikacji platformy Xamarin.iOS do świadczenia usług, które są dostępne dla użytkownika za pomocą aplikacja map i używanie programu Siri na urządzeniu z systemem iOS.




## <a name="related-links"></a>Linki pokrewne

- [Przykładowe ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Przewodnik programowania w języku SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Odwołanie do lokalizacji docelowych platformy](https://developer.apple.com/reference/intents)
- [Odwołanie do lokalizacji docelowych interfejsu użytkownika platformy](https://developer.apple.com/reference/intentsui)
- [Odwołanie do konwersji domen](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
