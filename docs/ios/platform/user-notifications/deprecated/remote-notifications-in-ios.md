---
title: Powiadomienia wypychane w systemie iOS
description: W tym dokumencie opisano sposób pracy za pomocą powiadomień wypychanych w systemie iOS 9 lub wcześniejszej. Omówiono w nim certyfikaty, rejestrowanie w usłudze Apple Push powiadomienia Gateway Service (APNS) i nie tylko.
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f11f5d1cbde0f5eae27215af8eb6544be46c0206
ms.sourcegitcommit: 7ffbecf4a44c204a3fce2a7fb6a3f815ac6ffa21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/28/2018
ms.locfileid: "39654818"
---
# <a name="push-notifications-in-ios"></a>Powiadomienia wypychane w systemie iOS

> [!IMPORTANT]
> Informacje przedstawione w tej sekcji odnoszą się do systemu iOS 9 i wcześniejszych, pozostał w tym miejscu umożliwiają starszymi wersjami systemu iOS. Dla systemu iOS 10 i nowszych wersji, zobacz [przewodnik struktura powiadomień użytkownika](~/ios/platform/user-notifications/index.md) do obsługi zarówno lokalnych, jak i zdalne powiadomienia na urządzeniu z systemem iOS.

Powiadomień wypychanych powinny znajdować się krótki opis i zawierać tylko wystarczającej ilości danych w celu powiadomienia aplikacji mobilnej, jego należy skontaktować się z aplikacji serwera aktualizacji. Na przykład po nadejściu nowej wiadomości e-mail, aplikacji serwera będzie tylko powiadomienie aplikacji mobilnej, nowy adres e-mail jest już dostępna. Powiadomienie nie będzie zawierał nowy adres e-mail, sam. Aplikacji mobilnej będzie następnie pobieranie nowych wiadomości e-mail z serwera podczas odpowiednie

W środku wypychanych powiadomień w systemie iOS jest *Apple Push Notification Gateway Service (APNS)*. To jest usługa dostarczonymi przez firmę Apple, który jest odpowiedzialny za routing powiadomienia z serwera aplikacji do urządzeń z systemem iOS.
Na poniższym obrazie przedstawiono topologię powiadomień wypychanych dla systemu iOS: ![](remote-notifications-in-ios-images/image4.png "ten obraz ilustracja topologii powiadomień wypychanych dla systemu iOS")

Zdalne powiadomienia, samodzielnie to ciągi zgodne z formatem w formacie JSON i protokoły określone w [ładunek powiadomienia](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) części [lokalnych i wypychanych Podręcznik programowania powiadomień](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)w [dokumentacji dla deweloperów systemu iOS](https://developer.apple.com/devcenter/ios/index.action).

Apple obsługuje dwa środowiska usługi APNS: *piaskownicy* i *produkcji* środowiska. W środowisku piaskownicy jest przeznaczona do testowania w fazie opracowywania i znajduje się w temacie `gateway.sandbox.push.apple.com` na porcie TCP 2195. W środowisku produkcyjnym jest przeznaczona do użycia w aplikacjach, które zostały wdrożone oraz można znaleźć w folderze `gateway.push.apple.com` na porcie TCP 2195.

## <a name="requirements"></a>Wymagania

Powiadomienie wypychane narzucają następujące reguły, które są definiowane przez architekturę usługi APNS:

-  **256 bajtów Limit wiadomości** — rozmiar cały komunikat powiadomienia, nie może przekraczać 256 bajtów.
-  **Brak potwierdzenia otrzymania** -APNS nie zapewnia nadawcy, o który komunikat dotarło do określonego adresata powiadomień. Jeśli urządzenie jest nieosiągalne, wiele kolejnych powiadomienia będą wysyłane wszystkie powiadomienia z wyjątkiem najnowszych, ostatnio zostaną utracone. Najbardziej aktualne powiadomienia będą dostarczane na urządzeniu.
-  **Każda aplikacja wymaga bezpiecznego certyfikatu** — komunikacja z usługą APNS musi odbywać się za pośrednictwem protokołu SSL.


## <a name="creating-and-using-certificates"></a>Tworzenie i używanie certyfikatów

Każdego z wymienionych w poprzedniej sekcji środowisk wymaga ich własnych certyfikatów. W tej sekcji opisano, jak utworzyć certyfikat, skojarzyć go z profilem inicjowania obsługi administracyjnej i następnie uzyskać certyfikat wymiany informacji osobistych do użycia z usługą PushSharp.

1.  Aby utworzyć certyfikaty przejdź do ustawień systemu iOS portalu inicjowania obsługi w witrynie internetowej firmy Apple, jak pokazano na poniższym zrzucie ekranu (Uwaga identyfikatory aplikacji element menu po lewej stronie):

    [![](remote-notifications-in-ios-images/image5new.png "Portal Aprowizowania w witrynie sieci Web jabłek dla systemu iOS")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  Następnie przejdź do sekcji identyfikatory aplikacji i Utwórz nowy identyfikator aplikacji, jak pokazano na poniższym zrzucie ekranu:

    [![](remote-notifications-in-ios-images/image6new.png "Przejdź do sekcji identyfikatory aplikacji i Utwórz nowy identyfikator aplikacji")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  Po kliknięciu **+** przycisku będzie można wprowadzić opis i identyfikator pakietu dla Identyfikatora aplikacji, jak pokazano w następnym zrzucie ekranu:

    [![](remote-notifications-in-ios-images/image7new.png "Wprowadź opis i identyfikator pakietu dla Identyfikatora aplikacji")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. Upewnij się, że wybrano **jawny identyfikator aplikacji** oraz że identyfikator pakietu nie kończy się znakiem `*` . Spowoduje to utworzenie identyfikator, który jest dobra dla wielu aplikacji i certyfikatów usługi wypychania powiadomień musi być dla pojedynczej aplikacji.

1. W obszarze App Services wybierz **powiadomień wypychanych**:

    [![](remote-notifications-in-ios-images/image8new.png "Wybierz powiadomienia wypychane")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. I naciśnij klawisz **przesyłania** o potwierdzenie rejestracji nowego Identyfikatora aplikacji:

    [![](remote-notifications-in-ios-images/image9new.png "Potwierdź rejestrację nowego Identyfikatora aplikacji")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  Następnie należy utworzyć certyfikat dla identyfikatora aplikacji. W obszarze nawigacji po lewej stronie ekranu, przejdź do **certyfikaty > wszystkie** i wybierz `+` przycisk, jak pokazano na poniższym zrzucie ekranu:

    [![](remote-notifications-in-ios-images/image10new.png "Utwórz certyfikat dla Identyfikatora aplikacji")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  Wybierz, czy chcesz użyć certyfikatu rozwoju i produkcji:

    [![](remote-notifications-in-ios-images/image11new.png "Wybierz certyfikat programowania lub produkcji")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. A następnie wybierz nowy identyfikator aplikacji, który właśnie utworzyliśmy:

    [![](remote-notifications-in-ios-images/image12new.png "Wybierz właśnie utworzony nowy identyfikator aplikacji")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  Spowoduje to wyświetlenie instrukcji, które przeprowadzi użytkownika przez proces tworzenia *żądanie podpisania certyfikatu* przy użyciu **dostęp do pęku kluczy** aplikacji na komputerze Mac.

7.  Teraz, gdy certyfikat został utworzony, należy użyć jako część procesu kompilacji do podpisania aplikacji, dzięki czemu może zarejestrować się za pomocą usługi APNs. Wymaga to tworzenie i instalowanie profilu aprowizacji, który używa certyfikatu.

8.  Aby utworzyć rozwoju, profil inicjowania obsługi administracyjnej, przejdź do **profilów aprowizacji** sekcji, a następnie wykonaj kroki, aby utworzyć, przy użyciu identyfikatora aplikacji, które właśnie utworzyliśmy.

9.  Po utworzeniu profilu inicjowania obsługi administracyjnej otwierają **organizatora Xcode** i ich odświeżanie. Jeśli utworzono profil aprowizacji nie jest widoczna, może być konieczne pobranie profilu z portalu aprowizacji systemu iOS i zaimportować go ręcznie. Poniższy zrzut ekranu przedstawia przykład organizatora przy użyciu profilu aprowizacji, dodane:  
    [![](remote-notifications-in-ios-images/image13new.png "Ten zrzut ekranu przedstawia przykład organizatora przy użyciu profilu aprowizacji, dodane")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  W tym momencie należy skonfigurować projekt rozszerzenia Xamarin.iOS do korzystania z nowo utworzoną profilu inicjowania obsługi administracyjnej. Można to zrobić w **opcje projektu** okna dialogowego, w obszarze **podpisywanie pakietu systemu iOS** karty, jak pokazano na poniższym zrzucie ekranu:  
    [![](remote-notifications-in-ios-images/image11.png "Konfigurowanie projektu Xamarin.iOS skorzystać z tej nowo utworzony profil inicjowania obsługi administracyjnej")](remote-notifications-in-ios-images/image11.png#lightbox)

Na tym etapie aplikacja jest skonfigurowana do pracy za pomocą powiadomień push. Istnieją jednak jeszcze kilka kroków wymaganych przy użyciu certyfikatu. Ten certyfikat jest w formacie DER, który nie jest zgodny z PushSharp, która wymaga certyfikatów wymiany informacji osobistych (PKCS12). Aby przekonwertować certyfikatu, czemu będzie ona używana przez PushSharp, wykonaj następujące kroki końcowego:

1.  **Pobierz plik certyfikatu** — Zaloguj się do portalu inicjowania obsługi dla systemu iOS wybrana karta Certyfikaty, zaznacz certyfikat skojarzony z poprawnego profilu inicjowania obsługi administracyjnej i wybierz opcję **Pobierz** .
1.  **Otwórz dostęp do pęku kluczy** — jest to aplikacja interfejs graficzny interfejs użytkownika do systemu zarządzania hasłami w systemie OS X.
1.  **Zaimportuj certyfikat** — Jeśli posiadający świadectwa nie jest już zainstalowany, **pliku... Importowanie elementów** menu dostęp do pęku kluczy. Przejdź do certyfikatu, który wyeksportowany powyżej i zaznacz go.
1.  **Wyeksportuj certyfikat** — rozwiń certyfikat, więc skojarzony klucz prywatny jest widoczna, kliknij prawym przyciskiem myszy na kluczu i wybrana opcja eksportowania. Użytkownik jest monitowany dla nazwy pliku i hasło dla eksportowanego pliku.

W tym momencie możemy są wykonywane przy użyciu certyfikatów. Firma Microsoft utworzono certyfikat, który będzie używany do podpisywania aplikacji systemu iOS i przekonwertować tego certyfikatu do formatu, który może być używany z PushSharp w ramach aplikacji serwera. Dalej Przyjrzyjmy się sposobu interakcji aplikacji systemu iOS z usługą APNS.

## <a name="registering-with-apns"></a>Rejestrowanie za pomocą usługi APNS

Przed systemu iOS aplikacja może odbierać zdalne powiadomienia, które należy zarejestrować za pomocą usługi APNS. APNS wygeneruje token unikatowych urządzeń i, wróć do aplikacji dla systemu iOS. Aplikacji dla systemu iOS zostaną następnie podjąć tokenu urządzenia i zarejestrować się z serwerem aplikacji. Po wszystkich dzieje się tak, rejestracja jest zakończona, a serwer aplikacji może wysyłać powiadomienia wypychane do urządzeń przenośnych.

Teoretycznie tokenu urządzenia mogą ulec zmianie każdorazowo aplikacji systemu iOS rejestruje się za pomocą usługi APNS, jednak w praktyce nie jest to realizowane, często. Optymalizacji aplikacji może buforować najnowszych tokenu urządzenia i aktualizować tylko na serwerze aplikacji po jej zmianie. Na poniższym diagramie przedstawiono proces rejestracji i uzyskiwanie tokenu urządzenia:

 ![](remote-notifications-in-ios-images/image12.png "Ten diagram przedstawia proces rejestracji i uzyskiwanie tokenu urządzenia")

Rejestracja za pomocą usługi APNS jest obsługiwana w `FinishedLaunching` metody klasy delegata aplikacji przez wywołanie metody `RegisterForRemoteNotificationTypes` na bieżącym `UIApplication` obiektu. W przypadku aplikacji systemu iOS rejestruje się za pomocą usługi APNS, on również określać, jakie rodzaje zdalne powiadomienia, jego chcesz otrzymywać. Te typy powiadomień zdalnego są deklarowane w wyliczeniu `UIRemoteNotificationType`. Poniższy fragment kodu znajduje się przykład jak aplikacji systemu iOS można zarejestrować do odbierania zdalnego alert i powiadomienia:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
    var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

    UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications ();
} else {
    UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
    UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
}
```

Żądanie rejestracji usługi APNS jest wykonywany w tle — po otrzymaniu odpowiedzi z systemem iOS wywoła metodę `RegisteredForRemoteNotifications` w `AppDelegate` klasy i przekazać token zarejestrowanego urządzenia. Token będzie znajdować się w `NSData` obiektu. Poniższy fragment kodu przedstawia sposób pobierania tokenu urządzenia tej usługi APNS, pod warunkiem:

```csharp
public override void RegisteredForRemoteNotifications (
UIApplication application, NSData deviceToken)
{
    // Get current device token
    var DeviceToken = deviceToken.Description;
    if (!string.IsNullOrWhiteSpace(DeviceToken)) {
        DeviceToken = DeviceToken.Trim('<').Trim('>');
    }

    // Get previous device token
    var oldDeviceToken = NSUserDefaults.StandardUserDefaults.StringForKey("PushDeviceToken");

    // Has the token changed?
    if (string.IsNullOrEmpty(oldDeviceToken) || !oldDeviceToken.Equals(DeviceToken))
    {
        //TODO: Put your own logic here to notify your server that the device token has changed/been created!
    }

    // Save new device token
    NSUserDefaults.StandardUserDefaults.SetString(DeviceToken, "PushDeviceToken");
}
```

Jeśli rejestracja nie powiedzie się z jakiegoś powodu (takie jak urządzenie nie jest połączone z Internetem), z systemem iOS wywoła `FailedToRegisterForRemoteNotifications` aplikacji klasa obiektu delegowanego. Poniższy fragment kodu przedstawia sposób wyświetlania alertu do użytkownika informacją, które rejestracja nie powiodła się:

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>Celów tokenu urządzenia

Tokenów urządzeń będą wygasać lub ulegać wraz z upływem czasu. Ze względu na fakt ten aplikacje serwera należy wykonać niektóre czyszczenia dom i czyszczenie te tokeny wygasły lub zostały zmienione. Gdy aplikacja wysyła powiadomienie wypychane na urządzeniu, zawierający tokenu wygasłe, APNS zostanie zarejestrować i zapisać wygasły token. Serwery mogą następnie utworzyć zapytanie względem usługi APNS, aby dowiedzieć się, jakie tokeny wygasły.

Usługi APNS używany do zapewnienia *usługi opinii* — punkt końcowy HTTPS, który przeprowadza uwierzytelnianie za pomocą certyfikatu, który został utworzony w celu wysyłania wypychanych powiadomień i wysyła dane o jakie tokeny wygasły. To została zastąpiona przez firmę Apple i usunięte.

Zamiast tego jest nowy kod stanu HTTP dla przypadek, który wcześniej został zgłoszony przez usługę opinii:

> 410 - token urządzenia nie jest już aktywna dla tematu.

Ponadto nowe `timestamp` będzie klucza danych JSON w treści odpowiedzi:

> Jeśli wartość: Nagłówek stanu jest 410, wartość tego klucza jest czas ostatniego, jaką APNs stwierdzono, że token urządzenia już nie jest prawidłowy dla tematu.
>
> Zatrzymaj wypychanie powiadomień do momentu urządzenia rejestruje token z sygnaturą czasową nowsze od dostawcy.

## <a name="summary"></a>Podsumowanie

W tej sekcji wprowadzenie podstawowych pojęć związanych z powiadomień wypychanych w systemie iOS. Wyjaśniono jej roli programu Apple Push Notification bramy Service (APNS). Następnie objętych usługą, tworzenia i używania certyfikatów zabezpieczeń, które są istotne dla usługi APNS. Na końcu tego dokumentu zakończeniu do dyskusji na temat użycia serwerów aplikacji *informacji zwrotnych usług* Aby zatrzymać śledzenie wygasło tokenów urządzeń.


## <a name="related-links"></a>Linki pokrewne

- [Powiadomienia — prezentacja lokalne i zdalne powiadomienia (przykład)](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [Lokalne i powiadomienia wypychane dla deweloperów](https://developer.apple.com/notifications/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
