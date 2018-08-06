---
title: Powiadomienia wypychane w systemie iOS
description: Ten dokument zawiera opis sposobu pracy z powiadomień wypychanych w systemie iOS 9 i wcześniejszych. Zawarto informacje certyfikatów, w zarejestrowani powiadomienia bramy Service (APNS, Apple Push) i inne.
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7bb2a250b9d3cc0c8df02f432330f9fe1dc58f94
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788670"
---
# <a name="push-notifications-in-ios"></a>Powiadomienia wypychane w systemie iOS

> [!IMPORTANT]
> Informacje przedstawione w tej sekcji dotyczą systemu iOS 9 i wcześniejszych, pozostał tutaj do obsługi starszych wersji systemu iOS. Dla systemu iOS 10 i nowsze, zobacz [Framework powiadomienia użytkownika — przewodnik](~/ios/platform/user-notifications/index.md) do obsługi w pliku lokalnym i zdalnym powiadomienia na urządzeniu z systemem iOS.

Powiadomienia wypychane powinny być przechowywane krótki i zawierać tylko za mało danych do powiadomienie aplikacji mobilnej, że kontaktowej aplikacji serwera aktualizacji. Na przykład momencie nadejścia nowych wiadomości e-mail, aplikacja serwera czy tylko powiadamiać aplikacji mobilnej, które Odebrano nowe wiadomości e-mail. Powiadomienia nie będzie zawierał nowych wiadomości e-mail, do samej siebie. Aplikacji mobilnej może następnie pobrać nowe wiadomości e-mail z serwera, gdy właściwe

W Centrum wypychania jest powiadomienia w systemie iOS *Apple Push Notification bramy Service (APNS)*. Jest to usługa dostarczonymi przez firmę Apple, odpowiedzialną za powiadomienia routingu z serwera aplikacji na urządzeniach z systemem iOS.
Na poniższym obrazie przedstawiono topologię powiadomień wypychanych dla systemu iOS: ![](remote-notifications-in-ios-images/image4.png "ten obraz ilustracja topologii powiadomień wypychanych dla systemu iOS")

Zdalne powiadomień, się są ciągów, które są zgodne z formatem w formacie JSON i protokoły określone w [ładunek powiadomienia](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) sekcji [lokalnych i wypychanych Podręcznik programowania powiadomień](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)w [dokumentacja dla deweloperów systemu iOS](https://developer.apple.com/devcenter/ios/index.action).

Apple przechowuje dwóch środowisk APNS: *piaskownicy* i *produkcji* środowiska. Środowisko piaskownicy jest przeznaczony do testowania w fazie projektowania i znajduje się w temacie `gateway.sandbox.push.apple.com` na TCP port 2195. W środowisku produkcyjnym jest przeznaczona do użycia w aplikacjach, które zostały wdrożone oraz znajduje się w temacie `gateway.push.apple.com` na TCP port 2195.

## <a name="requirements"></a>Wymagania

Powiadomienia wypychane muszą przestrzegać następujących reguł, które są definiowane przez architekturę APNS:

-  **256 bajtów Limit wiadomości** — rozmiar cały komunikat powiadomienia, nie może przekraczać 256 bajtów.
-  **Brak potwierdzenia otrzymania** -APNS nie zapewnia nadawcy z zgłaszającego komunikat go do określonego adresata powiadomienia. Jeśli urządzenie jest nieosiągalne, wiele kolejnych powiadomienia są wysyłane wszystkie powiadomienia z wyjątkiem ostatniego zostaną utracone. Tylko najnowsze powiadomień są dostarczane do urządzenia.
-  **Każda aplikacja wymaga bezpiecznego certyfikatu** -komunikacji z usługą APNS musi odbywać się za pośrednictwem protokołu SSL.


## <a name="creating-and-using-certificates"></a>Tworzenie i używanie certyfikatów

Każdy z tych środowisk wspomniano w poprzedniej sekcji wymagają własny certyfikat. W tej sekcji opisano, jak utworzyć certyfikat, skojarzyć go z profilu inicjowania obsługi administracyjnej, a następnie pobierać certyfikat wymiany informacji osobistych do użycia z PushSharp.

1.  Aby utworzyć certyfikatów przejdź do systemu iOS portalu inicjowania obsługi w witrynie sieci Web firmy Apple, jak pokazano na poniższym zrzucie ekranu (Uwaga identyfikatorów aplikacji element menu po lewej stronie):

    [![](remote-notifications-in-ios-images/image5new.png "IOS portalu inicjowania obsługi w witrynie sieci Web jabłek")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  Następnie przejdź do sekcji identyfikatorów aplikacji i Utwórz nowy identyfikator aplikacji, jak pokazano na poniższym zrzucie ekranu:

    [![](remote-notifications-in-ios-images/image6new.png "Przejdź do sekcji identyfikatorów aplikacji i Utwórz nowy identyfikator aplikacji")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  Po kliknięciu **+** przycisku, będzie można wprowadzić opis i identyfikator pakietu dla identyfikator aplikacji, jak pokazano w następnym zrzut ekranu:

    [![](remote-notifications-in-ios-images/image7new.png "Wprowadź opis oraz identyfikator pakietu dla Identyfikatora aplikacji")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. Upewnij się wybrać **jawny identyfikator aplikacji** i czy identyfikator pakietu nie kończą się `*` . Spowoduje to utworzenie identyfikatora, który jest dobra dla wielu aplikacji, a certyfikaty powiadomień wypychanych muszą mieć dla pojedynczej aplikacji.

1. W obszarze usług aplikacji, wybierz **powiadomień wypychanych**:

    [![](remote-notifications-in-ios-images/image8new.png "Wybierz powiadomienia wypychane")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. I naciśnij klawisz **przesyłania** do Potwierdź rejestrację nowy identyfikator aplikacji:

    [![](remote-notifications-in-ios-images/image9new.png "Potwierdź rejestrację nowego Identyfikatora aplikacji")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  Następnie należy utworzyć certyfikat dla identyfikatora aplikacji. W obszarze nawigacji po lewej stronie, przejdź do **certyfikaty > wszystkie** i wybierz `+` przycisku, jak pokazano na poniższym zrzucie ekranu:

    [![](remote-notifications-in-ios-images/image10new.png "Utwórz certyfikat dla Identyfikatora aplikacji")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  Wybierz, czy chcesz użyć certyfikatu rozwoju lub produkcji:

    [![](remote-notifications-in-ios-images/image11new.png "Wybierz certyfikat rozwoju lub produkcji")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. A następnie wybierz nowy identyfikator aplikacji, które właśnie utworzyliśmy:

    [![](remote-notifications-in-ios-images/image12new.png "Wybierz właśnie utworzony nowy identyfikator aplikacji")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  Spowoduje to wyświetlenie instrukcji, które przeprowadzi użytkownika przez proces tworzenia *żądania podpisania certyfikatu* przy użyciu **dostęp łańcucha kluczy** aplikacji opartym na systemie

7.  Teraz, certyfikatu został utworzony, należy użyć jako część procesu kompilacji do podpisania aplikacji, dzięki czemu może zarejestrować się przy użyciu usługi APNs. Wymaga to tworzenie i instalowanie profilu inicjowania obsługi administracyjnej, który używa certyfikatu.

8.  Aby utworzyć profil inicjowania obsługi administracyjnej programowania, przejdź do **profile inicjowania obsługi** sekcji, a następnie wykonaj kroki, aby utworzyć, za pomocą identyfikatora aplikacji właśnie utworzyliśmy.

9.  Po utworzeniu profilu inicjowania obsługi administracyjnej otwarcie **organizatora Xcode** i go odświeżyć. Jeśli utworzony profil inicjowania obsługi administracyjnej nie jest widoczna, może być konieczne pobranie profilu z portalu inicjowania obsługi systemu iOS i zaimportować go ręcznie. Poniższy zrzut ekranu przedstawia przykład organizatora z profilem należy dodać:

    [![](remote-notifications-in-ios-images/image13new.png "Ten zrzut ekranu przedstawia przykład organizatora z profilem należy dodać")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  Na tym etapie należy skonfigurować projektu platformy Xamarin.iOS do nowo utworzonej profilu inicjowania obsługi administracyjnej. Można to zrobić w **opcje projektu** okna dialogowego, w obszarze **iOS podpisywania pakietu** kartę jako przedstawiający na poniższym zrzucie ekranu:

    [![](remote-notifications-in-ios-images/image11.png "Konfigurowanie projektu platformy Xamarin.iOS do nowo utworzonej profilu inicjowania obsługi administracyjnej")](remote-notifications-in-ios-images/image11.png#lightbox)



W tym momencie aplikacja jest skonfigurowana do pracy z powiadomień wypychanych. Istnieją jednak nadal kilka wymaga większej liczby kroków z certyfikatem. Ten certyfikat jest w formacie DER, który nie jest zgodny z PushSharp, która wymaga certyfikatów wymiany informacji osobistych (PKCS12). Aby przekonwertować certyfikat, aby była używana przez PushSharp, wykonaj następujące kroki końcowego:

1.  **Pobierz plik certyfikatu** — logowanie do portalu inicjowania obsługi systemu iOS wybierz kartę certyfikaty, wybierz certyfikat skojarzony z poprawnego profilu inicjowania obsługi administracyjnej i wybierz opcję **Pobierz** .
1.  **Otwórz dostęp łańcucha kluczy** -to jest aplikacja jest interfejsem Graficznym do systemu zarządzania hasłami w OS X.
1.  **Zaimportuj certyfikat** — Jeśli posiadający świadectwa nie jest już zainstalowany, **pliku... Importowanie elementów** z menu dostęp łańcucha kluczy. Przejdź do certyfikatu, który wyeksportowany powyżej i zaznacz go.
1.  **Wyeksportuj certyfikat** — rozwiń certyfikat, więc skojarzony klucz prywatny jest widoczny, kliknij prawym przyciskiem myszy na kluczu i wybrać eksportu. Pojawi się monit dla pliku, a hasło do eksportowanego pliku.

Na tym etapie firma Microsoft są wykonywane za pomocą certyfikatów. Firma Microsoft utworzeniu certyfikatu, który będzie używany do podpisywania aplikacji systemu iOS i przekonwertować tego certyfikatu do formatu, który może być używany z PushSharp w aplikacji serwera. Następny Przyjrzyjmy się sposób interakcji aplikacji systemu iOS przy użyciu usługi APNS.

## <a name="registering-with-apns"></a>Rejestrowanie przy użyciu usługi APNS

Przed iOS aplikacji może odbierać powiadomienia zdalnego należy zarejestrować przy użyciu usługi APNS. APNS zostanie wygenerowania tokenu unikatowych urządzeń i która powrót do aplikacji dla systemu iOS. Aplikacje systemu iOS następnie otrzymuje token urządzenia i zarejestrować się z serwerem aplikacji. Po wszystkich dzieje się tak, rejestracja została zakończona, a serwer aplikacji może wypychanie powiadomień do urządzeń przenośnych.

Teoretycznie token urządzenia mogą ulec zmianie zawsze aplikacji systemu iOS rejestruje przy użyciu usługi APNS, ale w praktyce jest to nie zdarzyć, że często. Do optymalizacji aplikacji może buforować najnowszych token urządzenia i aktualizować tylko serwera aplikacji podczas jej zmienić. Na poniższym diagramie przedstawiono proces rejestracji i uzyskania tokena urządzenia:

 ![](remote-notifications-in-ios-images/image12.png "Ten diagram przedstawia proces rejestracji i uzyskania tokena urządzenia")

Rejestracja przy użyciu usługi APNS jest obsługiwany w `FinishedLaunching` metody klasy delegata aplikacji przez wywołanie metody `RegisterForRemoteNotificationTypes` na bieżącej `UIApplication` obiektu. Podczas rejestruje aplikacji systemu iOS przy użyciu usługi APNS, on także określić, jakie typy powiadomień zdalnego może zostać wyświetlony. Te typy powiadomień zdalnego są zadeklarowane w wyliczeniu `UIRemoteNotificationType`. Poniższy fragment kodu przedstawiono przykładowy sposób aplikacji systemu iOS można zarejestrować do odbierania zdalnego alertu i wskaźnika powiadomień:

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

Żądanie rejestracji usługi APN jest wykonywany w tle — po odebraniu odpowiedzi z systemem iOS będzie Wywołaj metodę `RegisteredForRemoteNotifications` w `AppDelegate` klasy i przekaż token zarejestrowanym urządzeniem. Token będzie znajdować się w `NSData` obiektu. Poniższy fragment kodu przedstawia sposób pobrać token urządzenia APNS, że podane:

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

Jeśli rejestracja nie powiedzie się z jakiegoś powodu (takie jak urządzenie nie jest podłączone do Internetu), iOS wywoła `FailedToRegisterForRemoteNotifications` klasa delegata w aplikacji. Poniższy fragment kodu przedstawia sposób wyświetlenia alertu użytkownikowi informująca, których nie powiodła się rejestracja:

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>Celów Token urządzenia

Tokeny urządzeń będzie wygaśnie lub zmieniać wraz z upływem czasu. Ze względu na fakt ten aplikacji serwera należy wykonać niektóre czyszczenia dom i przeczyścić tokeny te wygasły lub zmienione. Gdy aplikacja wysyła jako powiadomienie wypychane do urządzenia, które ma wygasły token, APNS zostanie zarejestrować i zapisać wygasły token. Serwery mogą następnie zapytania usługi APNS, aby dowiedzieć się, jakie tokeny wygasły.

Usługi APNS używany do zapewnienia *opinii usługi* -punkt końcowy HTTPS, który przeprowadza uwierzytelnianie za pomocą certyfikatu, który został utworzony w celu wysłania wypychania powiadomień i wysyła dane o jakie tokeny wygasły. Ma to przestarzała przez firmę Apple i usunięte.

Zamiast tego jest nowy kod stanu HTTP dla przypadku, gdy wcześniej został zgłoszony przez usługę opinii:

> 410 - token urządzenia nie jest już aktywne dla tematu.

Ponadto nowe `timestamp` będzie klucz danych JSON w treści odpowiedzi:

> Jeśli wartość w: Nagłówek stanu jest 410, wartość tego klucza jest czas ostatniego jaką APNs potwierdza, że token urządzenia nie był już prawidłowe dla tematu.
>
> Zatrzymaj wypychanie powiadomień, dopóki urządzenie rejestruje tokenu z sygnaturą czasową nowsze przy pomocy dostawcy usługi.

## <a name="summary"></a>Podsumowanie

W tej sekcji przedstawiono podstawowe pojęcia otaczającego powiadomień wypychanych w systemie iOS. Wyjaśniono jej roli z powiadomień bramy Service (APNS, Apple Push). Następnie objętych usługą, tworzenia i używania certyfikatów zabezpieczeń, które są niezbędne do APNS. Na koniec tego dokumentu została zakończona z omówione użycia serwerów aplikacji *opinii usługi* można zatrzymać śledzenia wygasł tokenów urządzeń.


## <a name="related-links"></a>Linki pokrewne

- [Powiadomienia - prezentacja powiadomień lokalnych i zdalnych (przykład)](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [Lokalne i wypychanie powiadomień dla deweloperów](https://developer.apple.com/notifications/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
