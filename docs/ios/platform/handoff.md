---
title: Handoff
description: "Ten artykuł obejmuje pracy z przekazaniem w aplikacji platformy Xamarin.iOS transferu działań użytkownika między aplikacje działające na użytkownika przez inne urządzenia."
ms.topic: article
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 0b3471f607bbde6560af597b6b901e6fbd1ec0b0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="handoff"></a>Handoff

_Ten artykuł obejmuje pracy z przekazaniem w aplikacji platformy Xamarin.iOS transferu działań użytkownika między aplikacje działające na użytkownika przez inne urządzenia._

Firma Apple wprowadziła przekazaniem iOS 8 i OS X Yosemite (10.10) do typowych mechanizm umożliwiający użytkownikowi transfer działania uruchomione na jednym ze swoich urządzeń do innego urządzenia z tej samej aplikacji lub innej aplikacji obsługującej tego samego działania.

[ ![](handoff-images/handoff02.png "Przykładem operacji przekazaniem")](handoff-images/handoff02.png)

W tym artykule otrzymuje krótki przegląd umożliwiające udostępnianie informacji w aplikacji platformy Xamarin.iOS działania i framework przekazaniem szczegółowo opisano:

## <a name="about-handoff"></a>O przekazaniem

Programowi handoff (znanej także jako ciągłości) została wprowadzona przez firmy Apple w systemie iOS 8 i OS X Yosemite (10.10) jako sposób dla użytkownika uruchomić działanie na jednym urządzeniu (z systemem iOS lub Mac) i kontynuować tego samego działania na innym urządzeniu (określone przez użytkownika iClou d konta).

Programowi handoff została rozszerzona w systemie iOS 9 w celu obsługi nowych, udoskonalone funkcje wyszukiwania. Aby uzyskać więcej informacji, zobacz nasze [ulepszenia wyszukiwania](~/ios/platform/search/index.md) dokumentacji.

Na przykład użytkownik może uruchomić wiadomość e-mail na ich iPhone i bezproblemowo kontynuować wiadomości e-mail na ich Mac, biorąc pod uwagę tych samych informacji komunikat wypełnione i kursor znajduje się w tej samej lokalizacji, że ich pozostaną w systemie iOS.

Dowolnej aplikacji, które mają taki sam _identyfikator zespołu_ są uprawnione do przy użyciu programowi Handoff na kontynuowanie działań użytkownika w aplikacjach, tak długo, jak te aplikacji są dostarczane za pośrednictwem sklepu iTunes App Store lub podpisany przez dewelopera zarejestrowanych (dla komputerów Mac, Enterprise lub Ad Hoc aplikacji).

Wszelkie `NSDocument` lub `UIDocument` oparte na aplikacje mają automatycznie przekazaniem obsługują wbudowanych i wymagają minimalne zmiany dotyczące obsługi przekazaniem.

### <a name="continuing-user-activities"></a>Kontynuowanie działania użytkownika

`NSUserActivity` Klasy (wraz z niektórych niewielkie zmiany w `UIKit` i `AppKit`) obsługuje określanie działania użytkownika, który może potencjalnie być kontynuowane w innym na urządzeniach użytkownika.

Dla działania do przekazania do innego urządzenia użytkownika, za pośrednictwem musi hermetyzowany w wystąpieniu `NSUserActivity`, oznaczona jako _bieżące działanie_, ustawić jego ładunku (dane używane w celu wykonania kontynuacji) i działania muszą zostać następnie przesłane do tego urządzenia.

Przekazaniem przekazuje bez systemu operacyjnego co najmniej informacje, aby zdefiniować działanie będzie kontynuowane z większych pakietów danych synchronizowanego za pośrednictwem usługi iCloud.

Na urządzenie odbierające użytkownik otrzyma powiadomienie, że czynność jest gotowa do kontynuacji. Jeśli użytkownik zdecyduje się na kontynuowanie działania na nowe urządzenie, określonej aplikacji jest uruchamiane (jeśli jeszcze nie działa) i ładunku z `NSUserActivity` służy do ponownego uruchamiania działania.

[ ![](handoff-images/handoffinteractions.png "Omówienie kontynuowanie działania użytkownika")](handoff-images/handoffinteractions.png)

Tylko te aplikacje, które współużytkują ten sam Deweloper identyfikator zespołu a odpowiadanie na danym _typ działania_ kwalifikują się do kontynuacji. Aplikacji Określa typy działań, obsługujący w obszarze `NSUserActivityTypes` klucza z jego **Info.plist** pliku. Biorąc pod uwagę to, kontynuowanie urządzenia wybiera aplikację, aby wykonać kontynuacji na podstawie Identyfikatora zespołu, typ działania i opcjonalnie _tytuł działania_.

Aplikacja odbierająca korzysta z informacji z `NSUserActivity`w `UserInfo` słownika, aby skonfigurować interfejs użytkownika i przywrócić stan danego działania, aby przejścia bezproblemowe dla użytkownika końcowego.

Jeśli kontynuacji wymaga więcej informacji, nie mogą być wysyłane wydajnie za pomocą `NSUserActivity`, wznawianie aplikacji można wysyłać wywołania pochodzące aplikacji i ustanowienia jeden lub więcej strumieni wymagane dane. Na przykład jeśli działanie zostało edytowanie dokumentu duży z wiele obrazów, przesyłania strumieniowego będą wymagane do przekazywania informacji niezbędnych do kontynuowania działania na urządzenie odbierające. Aby uzyskać więcej informacji, zobacz [strumieni kontynuacji wspierające](#Supporting-Continuation-Streams) poniższej sekcji.

Jak wspomniano powyżej, `NSDocument` lub `UIDocument` oparte na aplikacje mają automatycznie przekazaniem obsługuje wbudowanych. Aby uzyskać więcej informacji, zobacz [obsługi przekazaniem w aplikacjach opartych na dokument](#Supporting-Handoff-in-Document-Based-Apps) poniższej sekcji.

### <a name="the-nsuseractivity-class"></a>Klasa NSUserActivity

`NSUserActivity` Klasa jest obiektu podstawowego w exchange przekazaniem i jest używany w celu hermetyzacji stanu aktywności użytkownika, który jest dostępny do kontynuacji. Aplikacja zostanie utworzenie wystąpienia kopię `NSUserActivity` dla żadnego działania obsługuje i chce nadal na innym urządzeniu. Na przykład dokumentu edytora utworzyć działanie dla każdego dokumentu aktualnie otwarte. Jednak tylko dokument przedniej (wyświetlane na wierzchu okna lub karty) jest _bieżące działanie_ i do nich dostępne do kontynuacji.

Wystąpienie `NSUserActivity` jest identyfikowany przez jego `ActivityType` i `Title` właściwości. `UserInfo` Właściwość słownik używany do przesyłania informacji o stanie działania. Ustaw `NeedsSave` właściwości `true` Jeśli do opóźnionego ładowania informacje o stanie za pośrednictwem `NSUserActivity`na delegata. Użyj `AddUserInfoEntries` metody można scalić nowych danych z innych klientów do `UserInfo` słownika, ponieważ wymagane, aby zachować stan działania.

### <a name="the-nsuseractivitydelegate-class"></a>Klasa NSUserActivityDelegate

`NSUserActivityDelegate` Jest używany do przechowywania informacji `NSUserActivity`w `UserInfo` słownika aktualnych i zsynchronizowane z bieżącym stanie działania. Gdy system potrzebuje informacji zawartych w działaniu zaktualizowania (takich jak przed kontynuacji na innym urządzeniu), wywołuje `UserActivityWillSave` metody obiektu delegowanego.

Musisz zaimplementować `UserActivityWillSave` — metoda i udostępnić wszelkich zmian do `NSUserActivity` (takich jak `UserInfo`, `Title`, itd.) aby upewnić się, że nadal odzwierciedla stan bieżącego działania. Gdy system wywołuje `UserActivityWillSave` metody, `NeedsSave` flagi zostaną wyczyszczone. Jeśli zmodyfikujesz właściwości danych działania, musisz ustawić `NeedsSave` do `true` ponownie.

Zamiast `UserActivityWillSave` metody przedstawionych powyżej, można opcjonalnie mieć `UIKit` lub `AppKit` automatycznie zarządzać aktywności użytkownika. Aby to zrobić, ustaw obiektu odpowiadającego w trybie `UserActivity` właściwości i wdrożenie `UpdateUserActivityState` metody. Zobacz [obsługi przekazaniem w obiekty odpowiadające](#Supporting-Handoff-in-Responders) sekcji poniżej, aby uzyskać więcej informacji.

### <a name="app-framework-support"></a>Obsługa platformy aplikacji

Zarówno `UIKit` (iOS) i `AppKit` (OS X) podaj wbudowaną obsługę przekazaniem w `NSDocument`, obiekt odpowiadający w trybie (`UIResponder`/`NSResponder`), a `AppDelegate` klasy. Podczas każdego systemu operacyjnego implementuje przekazaniem nieco inaczej, podstawowego mechanizmu i interfejsy API są takie same.

#### <a name="user-activities-in-document-based-apps"></a>Działań użytkownika w aplikacjach opartych na dokumentu

Na podstawie dokumentu z systemami iOS i OS X aplikacje automatycznie obsługują przekazaniem wbudowanych. Aby aktywować tę obsługę, musisz dodać `NSUbiquitousDocumentUserActivityType` klucz i wartość dla każdego `CFBundleDocumentTypes` wpis w aplikacji **Info.plist** pliku.

Jeśli ten klucz jest obecny, zarówno `NSDocument` i `UIDocument` automatycznie utworzyć `NSUserActivity` wystąpień dla dokumentów na podstawie iCloud podanego typu. Należy podać typ czynności dla każdego typu dokumentu, który obsługuje aplikacja oraz wiele typów dokumentów może używać tego samego typu działania. Zarówno `NSDocument` i `UIDocument` automatycznie wypełnić `UserInfo` właściwość `NSUserActivity` z ich `FileURL` wartość właściwości.

Na OS X `NSUserActivity` zarządza `AppKit` i skojarzone z obiektów odpowiadających automatycznie stają się bieżące działanie, gdy okno dokumentu staje się oknem głównym. W systemach iOS dla `NSUserActivity` obiekty zarządzane przez `UIKit`, musi albo wywołanie `BecomeCurrent` metoda jawnie lub dokumentu `UserActivity` ustawiona w `UIViewController` gdy aplikacja jest dostarczany na pierwszy plan.

`AppKit` automatycznie przywrócić dowolne `UserActivity` właściwości utworzonej w ten sposób na OS X. Dzieje się tak, jeśli `ContinueUserActivity` metoda zwraca `false` lub jeśli niezaimplementowana. W takiej sytuacji dokument zostanie otwarty z `OpenDocument` metody `NSDocumentController` , a następnie otrzyma `RestoreUserActivityState` wywołania metody.

Zobacz [obsługi przekazaniem w aplikacjach opartych na dokument](#Supporting-Handoff-in-Document-Based-Apps) sekcji poniżej, aby uzyskać więcej informacji.

#### <a name="user-activities-and-responders"></a>Aktywności użytkowników i obiektów odpowiadających

Zarówno `UIKit` i `AppKit` automatycznie można zarządzać aktywności użytkownika, jeśli zostanie ustawiona jako obiektu odpowiadającego w trybie `UserActivity` właściwości. Jeśli stan został zmodyfikowany, musisz ustawić `NeedsSave` właściwości obiektu odpowiadającego `UserActivity` do `true`. System automatycznie zapisze `UserActivity` na żądanie po nadaniu czasu obiektu odpowiadającego w trybie zaktualizowany stan przez wywołanie jego `UpdateUserActivityState` metody.

Jeśli wiele obiektów odpowiadających udostępniać pojedynczy `NSUserActivity` wystąpienia, którą otrzymali `UpdateUserActivityState` wywołania zwrotnego, jeśli system aktualizuje obiekt działanie użytkownika. Obiekt odpowiadający musi wywołać `AddUserInfoEntries` metodę aktualizowania `NSUserActivity`w `UserInfo` słownika, aby odzwierciedlały bieżący stan działania na tym etapie. `UserInfo` Słownik jest wyczyszczone przed każdym `UpdateUserActivityState` wywołania.

Aby usunąć skojarzenie się z działania, można ustawić obiekt odpowiadający jej `UserActivity` właściwości `null`. Gdy platforma aplikacji zarządzanych `NSUserActivity` wystąpienie nie ma więcej skojarzone obiekty odpowiadające ani dokumentów, jest automatycznie nieważne.

Zobacz [obsługi przekazaniem w obiekty odpowiadające](#Supporting-Handoff-in-Responders) sekcji poniżej, aby uzyskać więcej informacji.

#### <a name="user-activities-and-the-appdelegate"></a>Działania użytkownika i AppDelegate

Aplikacji `AppDelegate` jest jego podstawowego punktu wejścia, gdy obsługa kontynuacji przekazaniem. Gdy użytkownik odpowiada na powiadomienie programowi Handoff odpowiednie aplikacja jest uruchamiana (Jeśli nie jest jeszcze uruchomiona) i `WillContinueUserActivityWithType` metody `AppDelegate` zostanie wywołana. W tym momencie aplikacji powinien poinformować użytkownika, który uruchamia kontynuacji.

`NSUserActivity` Wystąpienia są dostarczane, gdy `AppDelegate`w `ContinueUserActivity` metoda jest wywoływana. W tym momencie należy skonfigurować interfejs użytkownika aplikacji i kontynuować danego działania.

Zobacz [implementacja przekazaniem](#Implementing-Handoff) sekcji poniżej, aby uzyskać więcej informacji.

## <a name="enabling-handoff-in-a-xamarin-app"></a>Włączanie przekazaniem w aplikacji platformy Xamarin

Ze względu na wymagania dotyczące zabezpieczeń narzuconego przez przekazaniem aplikacji platformy Xamarin.iOS, który używa framework przekazaniem muszą zostać prawidłowo skonfigurowane w witrynie Apple Developer Portal i w pliku projektu platformy Xamarin.iOS.

Wykonaj następujące czynności:

1. Zaloguj się do [portalu dla deweloperów firmy Apple](http://developer.apple.com).
2. Polecenie **certyfikaty, identyfikatory & profile**.
3. Jeśli jeszcze tego nie zrobiono, kliknij polecenie **identyfikatory** i Utwórz identyfikator aplikacji (np. `com.company.appname`), Edytuj else identyfikatora istniejącego
4. Upewnij się, że **iCloud** usługi została sprawdzona dla podanego Identyfikatora: 

    [ ![](handoff-images/provision01.png "Włączanie usługi iCloud dla podanego Identyfikatora")](handoff-images/provision01.png)
5. Zapisz zmiany.
4. Polecenie **profile inicjowania obsługi** > **programowanie** i tworzenie nowych aplikacji profilu inicjowania obsługi administracyjnej dla Ciebie aplikacji: 

    [ ![](handoff-images/provision02.png "Tworzenie nowego profilu aplikacji inicjowania obsługi administracyjnej")](handoff-images/provision02.png)
5. Pobierz i zainstaluj nowy profil inicjowania obsługi administracyjnej albo użyj Xcode, aby pobrać i zainstalować profil.
6. Edytuj opcje projektu platformy Xamarin.iOS i upewnij się, że używasz profilu inicjowania obsługi administracyjnej, który został właśnie utworzony: 

    [ ![](handoff-images/provision03.png "Wybierz utworzony właśnie profil inicjowania obsługi administracyjnej")](handoff-images/provision03.png)
7. Następnie edytuj Twojej **Info.plist** i upewnij się, że używasz identyfikator aplikacji, który został użyty do utworzenia profilu inicjowania obsługi administracyjnej: 

    [ ![](handoff-images/provision04.png "Ustaw identyfikator aplikacji")](handoff-images/provision04.png)
8. Przewiń do **tryby tła** sekcji i sprawdź następujące elementy: 

    [ ![](handoff-images/provision05.png "Włącz tryby tła wymagane")](handoff-images/provision05.png)
9. Zapisz zmiany do wszystkich plików.

Przy użyciu tych ustawień w miejscu aplikacja jest teraz gotowy dostępu do interfejsów API Framework przekazaniem. Aby uzyskać szczegółowe informacje o inicjowaniu obsługi, zobacz nasze [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) i [Inicjowanie obsługi aplikacji](~/ios/get-started/installation/device-provisioning/index.md) przewodniki.

## <a name="implementing-handoff"></a>Implementowanie przekazaniem

Działania użytkownika może być kontynuowane między aplikacji, które są podpisane za pomocą ten sam Deweloper identyfikator zespołu i obsługiwać ten sam typ działania. Implementowanie przekazaniem w aplikacji platformy Xamarin.iOS wymaga utworzenia obiektu działania użytkownika (albo w `UIKit` lub `AppKit`), aktualizować stan obiektu, aby śledzić działania i kontynuowanie działania na urządzeniu odbierania.

### <a name="identifying-user-activities"></a>Identyfikowania działań użytkownika

Pierwszy krok w przypadku implementowania przekazaniem do identyfikacji typów działań użytkownika, które obsługuje aplikację oraz wyświetlanie, która z tych działań nadają się do kontynuacji na innym urządzeniu. Na przykład: aplikacja rzeczy do zrobienia może obsługiwać edytowania elementów jako jeden _typ działania użytkownika_i obsługuje przeglądania na liście dostępne elementy, jak inny.

Aplikację można utworzyć dowolną liczbę użytkowników typów działań w razie potrzeby, jeden dla dowolnej funkcji, która zawiera aplikację. Dla każdego typu działania użytkownika aplikacja musi śledzić, gdy rozpoczyna się działanie typu i kończy się i wymagane do przechowywania informacji aktualny stan, aby kontynuować zadania na innym urządzeniu.

Działania użytkownika może być kontynuowane w dowolnej aplikacji podpisanej o tym samym identyfikatorze zespołu bez żadnych mapowanie jeden do jednego między aplikacjami wysyłania i odbierania. Na przykład danej aplikacji można utworzyć cztery różne typy działań, które są używane przez inną, poszczególne aplikacje na innym urządzeniu. Jest to często dochodzi między Mac wersję aplikacji (które może mieć wiele funkcji i funkcji) i aplikacje dla systemu iOS, który każdej aplikacji mniejsze i skupiają się na określonych zadań.

### <a name="creating-activity-type-identifiers"></a>Tworzenie identyfikatorów typ działania

_Identyfikator typu działania_ krótkich ciągów jest dodawany do `NSUserActivityTypes` aplikacji **Info.plist** plik używany do jednoznacznego identyfikowania danego typu działania użytkownika. W tablicy dla każdego działania, który obsługuje aplikacja będzie zawierać jeden wpis. Apple sugeruje, aby uniknąć kolizji przy użyciu notacji wstecznego DNS stylu dla identyfikatora typu działania. Na przykład: `com.company-name.appname.activity` dla określonej aplikacji na podstawie działań lub `com.company-name.activity` działań, które można uruchamiać wielu aplikacjom.

Identyfikator typu działania jest używany podczas tworzenia `NSUserActivity` wystąpienia, aby zidentyfikować typu działania. Gdy działanie jest nadal na innym urządzeniu, typ działania (wraz z identyfikator zespołu aplikacji) określa aplikacji, które można uruchomić, aby kontynuować działanie.

Na przykład zamierzamy utworzyć przykładową aplikację o nazwie **MonkeyBrowser** ([Pobierz tutaj](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)). Ta aplikacja wyświetli cztery karty, każda z różnych Otwórz adres URL w przeglądarce strony sieci web. Użytkownik będzie mógł kontynuować każdej z kart na urządzeniu iOS inną aplikację.

Aby utworzyć wymagane identyfikatory typu działań do obsługi tego zachowania, należy edytować **Info.plist** plików i przejdź do **źródła** widoku. Dodaj `NSUserActivityTypes` klucza i utworzyć następujące identyfikatory:

[ ![](handoff-images/type01.png "Klucz NSUserActivityTypes i wymagane identyfikatory w edytorze plist")](handoff-images/type01.png)

Utworzyliśmy cztery nowe działanie typu identyfikatory, po jednej dla każdej z kart w przykładzie **MonkeyBrowser** aplikacji. Podczas tworzenia własnych aplikacji, Zastąp zawartość `NSUserActivityTypes` tablicy o identyfikatorach typu działania specyficzne dla działania aplikacji obsługuje.

### <a name="tracking-user-activity-changes"></a>Śledzenie zmian czynności użytkownika

Gdy utworzymy nowe wystąpienie klasy `NSUserActivity` klasy, firma Microsoft będzie określać `NSUserActivityDelegate` wystąpienia śledzenie zmian stanu działania. Na przykład kod wykonaj może być używany do śledzenia zmian stanu:

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;

namespace MonkeyBrowse
{
    public class UserActivityDelegate : NSUserActivityDelegate
    {
        #region Constructors
        public UserActivityDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void UserActivityReceivedData (NSUserActivity userActivity, NSInputStream inputStream, NSOutputStream outputStream)
        {
            // Log
            Console.WriteLine ("User Activity Received Data: {0}", userActivity.Title);
        }

        public override void UserActivityWasContinued (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity Was Continued: {0}", userActivity.Title);
        }

        public override void UserActivityWillSave (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity will be Saved: {0}", userActivity.Title);
        }
        #endregion
    }
}
```

`UserActivityReceivedData` Metoda jest wywoływana, gdy strumień kontynuacji otrzymał danych z urządzenia wysyłającego. Aby uzyskać więcej informacji, zobacz [strumieni kontynuacji wspierające](#Supporting-Continuation-Streams) poniższej sekcji.

`UserActivityWasContinued` Metoda jest wywoływana, gdy inne urządzenie przejmuje działania z bieżącego urządzenia. W zależności od typu działań, takich jak dodawanie nowego elementu do listy ToDo aplikacja może musi przerwać działania na urządzeniu wysyłania.

`UserActivityWillSave` Metoda jest wywoływana przed wszelkie zmiany do działania są zapisywane i synchronizowane między urządzeniami dostępna lokalnie. Ta metoda służy do wprowadzania żadnych zmian ostatniej minuty w `UserInfo` właściwość `NSUserActivity` wystąpienia przed ich wysłaniem.

### <a name="creating-a-nsuseractivity-instance"></a>Tworzenie wystąpienia NSUserActivity

Każde działanie, które chce zapewnić możliwość dalszego na innym urządzeniu aplikacji musi być umieszczane w `NSUserActivity` wystąpienia. Aplikację można utworzyć dowolną liczbę działań, zgodnie z wymaganiami i rodzaj tych działań jest zależny od funkcji i funkcji w aplikacji. Na przykład aplikację poczty e-mail może utworzyć jedno działanie do tworzenia nowej wiadomości i drugi dla odczytywania wiadomości.

Na przykład aplikacji nowej `NSUserActivity` jest tworzony za każdym razem, gdy użytkownik wprowadza nowy adres URL w jednym widoku przeglądarki sieci web z kartami. Poniższy kod zapisuje stan danej karty:

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSUserActivity UserActivity { get; set; }
...

UserActivity = new NSUserActivity (UserActivityTab1);
UserActivity.Title = "Weather Tab";
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Tworzy nową `NSUserActivity` przy użyciu jednego z typów działań użytkownika utworzone powyżej i udostępnia tytuł zrozumiałą dla działania. Dołącza do wystąpienia `NSUserActivityDelegate` utworzone powyżej do monitorowania stanu zmiany i iOS informuje, że to działanie użytkownika jest bieżące działanie.

### <a name="populating-the-userinfo-dictionary"></a>Wypełnianie słownika informacje o użytkowniku

Jak mamy już wspomniano powyżej, `UserInfo` właściwość `NSUserActivity` jest klasa `NSDictionary` par kluczy i wartości używane do definiowania stan danego działania. Wartości przechowywane w `UserInfo` musi mieć jedną z następujących typów: `NSArray`, `NSData`, `NSDate`, `NSDictionary`, `NSNull`, `NSNumber`, `NSSet`, `NSString`, lub `NSURL`. `NSURL` wartości danych, które wskaż dokumenty iCloud zostanie automatycznie dopasowana tak, aby wskazywały dokumentów na urządzenie odbierające.

W powyższym przykładzie utworzyliśmy `NSMutableDictionary` obiektu i wypełniane jednego klucza, podając adres URL, który użytkownik został aktualnie wyświetlane na karcie danego. `AddUserInfoEntries` Zaktualizować działania z danymi, które będą używane do działania na urządzenie odbierające przywracania została użyta metoda aktywności użytkownika:

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple sugeruje zachować informacje wysyłane do barest minimum, aby upewnić się, że działanie nie są przesyłane w odpowiednim czasie do odbierania urządzenia. Jeśli większych informacji jest wymagane, jak można edytować obrazu dołączonego do dokumentu musi być wysyłane, należy użyć strumieni kontynuacji. Zobacz [strumieni kontynuacji wspierające](#Supporting-Continuation-Streams) sekcji poniżej, aby uzyskać więcej informacji.

### <a name="continuing-an-activity"></a>Kontynuowanie działania

Przekazaniem poinformuje automatycznie lokalnego z systemem iOS i OS X urządzeń, które znajdują się w fizycznej bliskości urządzenia źródłowego i zalogowaniem się do tego samego konta usługi iCloud dostępności można kontynuować działania użytkownika. Jeśli użytkownik zdecyduje się na kontynuowanie działań na nowe urządzenie, system uruchomi odpowiednich aplikacji (na podstawie Identyfikatora zespołu i typ działania) i informacje o jego `AppDelegate` kontynuacji musi nastąpić.

Najpierw `WillContinueUserActivityWithType` metoda jest wywoływana, aplikacja może poinformować użytkownika, który ma rozpocząć kontynuacji. Możemy użyć poniższego kodu w **AppDelegate.cs** pliku naszych przykładową aplikację do obsługi kontynuacji początkowy:

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSString UserActivityTab2 = new NSString ("com.xamarin.monkeybrowser.tab2");
public NSString UserActivityTab3 = new NSString ("com.xamarin.monkeybrowser.tab3");
public NSString UserActivityTab4 = new NSString ("com.xamarin.monkeybrowser.tab4");
...

public FirstViewController Tab1 { get; set; }
public SecondViewController Tab2 { get; set;}
public ThirdViewController Tab3 { get; set; }
public FourthViewController Tab4 { get; set; }
...

public override bool WillContinueUserActivity (UIApplication application, string userActivityType)
{
    // Report Activity
    Console.WriteLine ("Will Continue Activity: {0}", userActivityType);

    // Take action based on the user activity type
    switch (userActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Inform view that it's going to be modified
        Tab1.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Inform view that it's going to be modified
        Tab2.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Inform view that it's going to be modified
        Tab3.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Inform view that it's going to be modified
        Tab4.PreparingToHandoff ();
        break;
    }

    // Inform system we handled this
    return true;
}
```

W powyższym przykładzie każdy kontroler widoku rejestruje `AppDelegate` i ma publiczny `PreparingToHandoff` metodę, która wyświetla komunikat informujący użytkownika, że działanie ma być przekazane do bieżącego urządzenia i wskaźnika działania. Przykład:

```csharp
private void ShowBusy(string reason) {

    // Display reason
    BusyText.Text = reason;

    //Define Animation
    UIView.BeginAnimations("Show");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0.5f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PreparingToHandoff() {
    // Inform caller
    ShowBusy ("Continuing Activity...");
}
```

`ContinueUserActivity` z `AppDelegate` zostanie wywołana w celu faktycznie kontynuować danego działania. Ponownie z naszych przykładową aplikację:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

Publicznego `PerformHandoff` metoda każdego kontrolera widoku faktycznie preforms oddanie i przywraca działanie na bieżącego urządzenia. W przypadku przykładzie wyświetla ten sam adres URL w danej karcie, który użytkownik podczas przeglądania na innym urządzeniu. Przykład:

```csharp
private void HideBusy() {

    //Define Animation
    UIView.BeginAnimations("Hide");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PerformHandoff(NSUserActivity activity) {

    // Hide busy indicator
    HideBusy ();

    // Extract URL from dictionary
    var url = activity.UserInfo ["Url"].ToString ();

    // Display value
    URL.Text = url;

    // Display the give webpage
    WebView.LoadRequest(new NSUrlRequest(NSUrl.FromString(url)));

    // Save activity
    UserActivity = activity;
    UserActivity.BecomeCurrent ();

}
```

`ContinueUserActivity` Metoda zawiera `UIApplicationRestorationHandler` można wywołać dla dokumentu lub obiekt odpowiadający w trybie na podstawie wznawianie działania. Musisz przekazać `NSArray` lub dostępnych obiektów do obsługi przywracania po wywołaniu. Na przykład:

```csharp
completionHandler (new NSObject[]{Tab4});
```

Dla każdego obiektu przekazany jego `RestoreUserActivityState` metoda zostanie wywołana. Każdy obiekt może następnie użyć danych w `UserInfo` słownika do przywrócenia jego własnej stanu. Na przykład:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

Dla aplikacji opartych na dokument, jeśli nie implementują `ContinueUserActivity` lub metoda zwraca `false`, `UIKit` lub `AppKit` automatycznie wznowić działania. Zobacz [obsługi przekazaniem w aplikacjach opartych na dokument](#Supporting-Handoff-in-Document-Based-Apps) sekcji poniżej, aby uzyskać więcej informacji.

### <a name="failing-handoff-gracefully"></a>Niepowodzenie przekazaniem bezpiecznie

Ponieważ przekazaniem opiera się na przekazywania informacji między kolekcji słabo połączone z systemem iOS i OS X urządzenia, czasami niepowodzenia procesu transferu. Zalecane jest zaprojektowanie aplikacji bezpiecznie obsłużyć te błędy i powiadamia użytkownika o wszelkich sytuacjach nasuwające.

W przypadku awarii `DidFailToContinueUserActivitiy` metody `AppDelegate` zostanie wywołana. Na przykład:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

Należy używać podane `NSError` źródło informacji dla użytkownika o awarii.

## <a name="native-app-to-web-browser-handoff"></a>Aplikacji natywnej do przekazaniem przeglądarki sieci Web

Użytkownik może chcieć kontynuować działanie bez odpowiedniej aplikacji natywnej zainstalowanym żądane urządzenie. W niektórych sytuacjach interfejsu opartego na sieci web może zapewnienie wymaganej funkcjonalności i działania nadal może być kontynuowane. Na przykład konta e-mail przewidują interfejsu użytkownika sieci web base tworzenie i odczytywania wiadomości.

Jeśli źródłowy, natywnego aplikacja zna adres URL dla interfejsu sieci web (i wymaganej składni identyfikowania danego elementu kontynuowana) może zakodować te informacje w `WebpageURL` właściwość `NSUserActivity` wystąpienia. Jeśli urządzenie odbierające nie ma odpowiedniej aplikacji natywnej zainstalowana tak, aby obsłużyć kontynuacji, można wywołać interfejsu podany w sieci web.

## <a name="web-browser-to-native-app-handoff"></a>Przeglądarki sieci Web do natywnej aplikacji programowi Handoff

Jeśli użytkownik został przy użyciu interfejsu sieci web na urządzeniu źródłowego i natywnych aplikacji na urządzenie odbierające oświadczenia domeny część `WebpageURL` właściwości, a następnie system będzie używać tego dojścia kontynuacji aplikacji. Nowe urządzenie otrzyma `NSUserActivity` wystąpienia tego typu działania, co oznacza `BrowsingWeb` i `WebpageURL` będą zawierać adres URL został odwiedzający użytkownika, `UserInfo` słownika będzie pusty.

Aplikację do uczestnictwa w tym typie przekazaniem go oświadczenia domeny w `com.apple.developer.associated-domains` uprawnień w formacie `<service>:<fully qualified domain name>` (na przykład: `activity continuation:company.com`).

Jeśli pasuje do określonej domeny `WebpageURL` wartość właściwości, przekazaniem listę zatwierdzonych aplikacji identyfikatorów pliki do pobrania z witryny sieci Web w tej domenie. Witryny sieci Web, musisz podać listę identyfikatorów zatwierdzone w podpisany plik JSON o nazwie **apple-app lokacji — skojarzenia** (na przykład `https://company.com/apple-app-site-association`).

Ten plik JSON zawiera słownik, który określa listę identyfikatorów aplikacji w formie `<team identifier>.<bundle identifier>`. Na przykład:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

Do podpisywania pliku JSON (dzięki czemu jest on prawidłowy `Content-Type` z `application/pkcs7-mime`), użyj **terminali** aplikacji i `openssl` z certyfikat i klucz wystawiony przez urząd certyfikacji zaufany przez system iOS (zobacz [ http://support.Apple.com/kb/ht5012](http://support.apple.com/kb/ht5012) lista). Na przykład:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

`openssl` Podpisanego pliku JSON, który można umieścić w witrynie sieci Web w danych wyjściowych polecenia **apple-app lokacji — skojarzenia** adresu URL. Na przykład:

```csharp
https://example.com/apple-app-site-association.
```

Aplikacja będzie otrzymywać wszystkie działania którego `WebpageURL` domeny znajduje się w jego `com.apple.developer.associated-domains` uprawnienia. Tylko `http` i `https` protokoły są pomocy technicznej, innego protokołu zgłosi wyjątek.

## <a name="supporting-handoff-in-document-based-apps"></a>Obsługa przekazaniem w aplikacji opartych na dokumentu

Jak już wspomniano w systemach iOS i OS X aplikacji opartych na dokumentu będzie automatycznie obsługiwał programowi Handoff na podstawie iCloud dokumentów, jeśli aplikacja **Info.plist** plik zawiera `CFBundleDocumentTypes` klucza z `NSUbiquitousDocumentUserActivityType`. Na przykład:

```xml
<key>CFBundleDocumentTypes</key>
<array>
    <dict>
        <key>CFBundleTypeName</key>
        <string>NSRTFDPboardType</string>
        . . .
        <key>LSItemContentTypes</key>
        <array>
        <string>com.myCompany.rtfd</string>
        </array>
        . . .
        <key>NSUbiquitousDocumentUserActivityType</key>
        <string>com.myCompany.myEditor.editing</string>
    </dict>
</array>
```

W tym przykładzie ciąg jest desygnator aplikacji wstecznego DNS o nazwie działania dołączane. Jeśli wprowadzone w ten sposób, wpisy typu działania nie trzeba powtarzać się w `NSUserActivityTypes` tablicę **Info.plist** pliku.

Automatycznie utworzonych obiektów aktywności użytkownika (dostępnych w dokumencie `UserActivity` właściwości) może być wywoływany przez inne obiekty w aplikacji i używane w celu przywrócenia stanu na kontynuację. Na przykład aby śledzić elementu wybór i zarządzania dokumentami pozycji. Należy ustawić tego działania `NeedsSave` właściwości `true` po każdej zmianie stanu zmiany i zaktualizować `UserInfo` słownika `UpdateUserActivityState` metody.

`UserActivity` Właściwości mogą być używane z dowolnego wątku i odpowiada klucz wartość observing (KVO) protokołu, dzięki mogą być używane do synchronizowania dokumentu i wylogowywanie przesyłane z usługą iCloud. `UserActivity` Właściwość zostaną unieważnione, gdy dokument zostanie zamknięty.

Aby uzyskać więcej informacji, zobacz firmy Apple [obsługę działań użytkownika w aplikacji opartych na dokument](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) dokumentacji.

## <a name="supporting-handoff-in-responders"></a>Obsługa przekazaniem w obiektów odpowiadających

Możesz skojarzyć obiektów odpowiadających (dziedziczone z poziomu `UIResponder` w systemie iOS lub `NSResponder` na OS X) dla działań przez ustawienie ich `UserActivity` właściwości. System automatycznie zapisuje `UserActivity` właściwość we właściwym czasie wywoływania obiekt odpowiadający `UpdateUserActivityState` metody w celu dodania do obiektu aktywności użytkownika przy użyciu bieżących danych `AddUserInfoEntriesFromDictionary` metody.

## <a name="supporting-continuation-streams"></a>Obsługa kontynuacji strumieni

Może się zdarzyć, gdy ilość informacji w celu kontynuowania działania wymagane nie może wydajnie przesłanych przez początkowej ładunku przekazaniem. W takiej sytuacji aplikacja odbierająca może nawiązywać co najmniej jednego strumienia między sobą i źródłowy aplikacji na przesyłanie danych.

Źródłowy aplikacji ustawi `SupportsContinuationStreams` właściwość `NSUserActivity` wystąpienie do `true`. Na przykład:

```csharp
// Create a new user Activity to support this tab
UserActivity = new NSUserActivity (ThisApp.UserActivityTab1){
    Title = "Weather Tab",
    SupportsContinuationStreams = true
};
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Aplikacja odbierająca może wywoływać `GetContinuationStreams` metody `NSUserActivity` w jego `AppDelegate` ustanowienie strumienia. Na przykład:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

Na urządzeniu źródłowego do delegata działania użytkownika odbiera strumienie przez wywołanie jego `DidReceiveInputStream` metodę w celu zapewnienia dane żądanie na kontynuowanie działania użytkownika na wznawianie urządzenia.

Użyjesz `NSInputStream` umożliwia dostęp tylko do odczytu do transmisji danych i `NSOutputStream` zapewnić dostęp tylko do zapisu. Strumienie należy używać w czasie żądania i odpowiedzi, gdzie aplikacja odbierająca żądania większej ilości danych i zapewnia źródłowego aplikacji. Tak, aby dane zapisywane do strumienia wyjściowego urządzenia źródłowego jest do odczytu ze strumienia wejściowego na kontynuowanie urządzeniu i na odwrót.

Nawet w sytuacjach, w których wymagane są kontynuacji strumienia powinien być minimalny tyłu i do przodu komunikację między dwiema aplikacjami.

Aby uzyskać więcej informacji, zobacz firmy Apple [strumieni kontynuacji Using](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) dokumentacji.

## <a name="handoff-best-practices"></a>Najlepsze rozwiązania w zakresie przekazaniem

Pomyślnego wykonania bezproblemowe utrzymania aktywności użytkownika za pośrednictwem przekazaniem wymaga dokładne projektu ze względu na różne składniki zaangażowane. Apple sugeruje przyjmowanie następujące najlepsze rozwiązania w zakresie aplikacji przekazaniem włączone:

- Projektowanie Twojej aktywności użytkowników, aby wymagać ładunku najmniejsza możliwa do powiązania stan działanie będzie kontynuowane. Im większy ładunek, tym dłużej trwa kontynuacji, aby rozpocząć.
- Należy transfer dużych ilości danych pomyślnie kontynuację, uwzględniać, koszty związane z w koszty konfiguracji i sieci.
- Jest typowe dla dużych aplikacji Mac do tworzenia działań użytkownika, które są obsługiwane przez kilka mniejszych, specyficzne dla zadania aplikacje na urządzeniach z systemem iOS. Różnych aplikacji i wersji systemu operacyjnego należy tak zaprojektować do siebie lub bezpiecznie zakończyć się niepowodzeniem.
- Przy określaniu typów z działań, użyj notacji wstecznego DNS, aby uniknąć kolizji. Jeśli działanie jest specyficzne dla danej aplikacji, jego nazwa powinny obejmować w definicji typu (na przykład `com.myCompany.myEditor.editing`). Jeśli działanie można pracować w wielu aplikacjach, Porzuć Nazwa aplikacji z definicji (na przykład `com.myCompany.editing`).
- Jeśli Twoja aplikacja powinna zaktualizowany stan działania użytkownika (`NSUserActivity`) ustaw `NeedsSave` właściwości `true`. W odpowiednim czasie przekazaniem wywoła delegata `UserActivityWillSave` metody, aby można było wykonać aktualizację `UserInfo` słownika zgodnie z potrzebami.
- Ponieważ proces przekazaniem nie może zainicjować natychmiast na urządzenie odbierające, należy zaimplementować `AppDelegate`w `WillContinueUserActivity` i poinformować użytkownika o kontynuację się uruchomić.

## <a name="example-handoff-app"></a>Przykład programowi Handoff aplikacji

Jako przykład w aplikacji platformy Xamarin.iOS przy użyciu programowi Handoff Przedstawiliśmy [ **MonkeyBrowser** ](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/) Przykładowa aplikacja za pomocą tego przewodnika. Aplikacja ma cztery karty, które użytkownik może użyć do przeglądania sieci web, każda z typem dane działanie: pogodzie, ulubionego i kawy podziału pracy.

Na każdej z kart, gdy użytkownik wprowadzi nowy adres URL i podsłuchu **Przejdź** przycisk Nowy `NSUserActivity` jest tworzony na tej karcie, który zawiera adres URL, który użytkownik jest obecnie przeglądania:

[ ![](handoff-images/handoff01.png "Przykład programowi Handoff aplikacji")](handoff-images/handoff01.png)

Jeśli inny urządzeń użytkownika zawiera **MonkeyBrowser** aplikacja jest zainstalowana, jest zalogowaniem się do usługi iCloud przy użyciu tego samego konta użytkownika, jest w tej samej sieci, a w pobliżu urządzenia powyżej, przekazaniem działania będzie wyświetlana na stronę główną ekran (w dolnym rogu po lewej stronie):

[ ![](handoff-images/handoff02.png "Działanie przekazaniem wyświetlane na ekranie macierzystego w dolnym rogu po lewej stronie")](handoff-images/handoff02.png)

Jeśli użytkownik przeciąga w górę na ikonie przekazaniem, aplikacja zostanie uruchomiona i aktywności użytkownika określonego w `NSUserActivity` będzie dalej na nowe urządzenie:

[ ![](handoff-images/handoff03.png "Nadal aktywność użytkownika na nowe urządzenie")](handoff-images/handoff03.png)

Jeśli aktywność użytkownika została pomyślnie wysłana do innej firmy Apple urządzenia, urządzenia wysyłającego `NSUserActivity` otrzymają wywołanie `UserActivityWasContinued` metody na jego `NSUserActivityDelegate` opcję wiedzieć, że działanie użytkownika została pomyślnie przeniesiona do innego urządzenie.

## <a name="summary"></a>Podsumowanie

W tym artykule udzielił wprowadzenie do struktury programowi Handoff umożliwia kontynuowanie działania użytkownika, między wieloma użytkownika urządzeń firmy Apple. Następnie go pokazano, jak włączyć i wdrożyć przekazaniem w aplikacji platformy Xamarin.iOS. Na koniec go omówiono różne typy kontynuacje przekazaniem dostępne i przekazaniem najlepsze rozwiązania.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Przykładowe MonkeyBrowser](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [Nowości w systemie iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Przewodnik HomeKitDeveloper](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [Wskazówki dotyczące interfejsu użytkownika HomeKit](https://developer.apple.com/homekit/ui-guidelines/)
- [Odwołanie HomeKit Framework](https://developer.apple.com/library/ios/home_kit_framework_ref)
