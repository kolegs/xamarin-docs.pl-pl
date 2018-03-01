---
title: Zdalnego powiadomienia z Firebase Cloud Messaging
description: "Ten przewodnik zawiera szczegółowe informacje dotyczące wykonania zdalnego powiadomienia (nazywanych również powiadomienia wypychane) za pomocą Firebase Cloud Messaging w aplikacji platformy Xamarin.Android. Przedstawia wdrożenie różnych klas, które są potrzebne do komunikacji z wiadomości chmury Firebase (FCM), przykłady sposobu konfigurowania manifestu systemu Android do uzyskiwania dostępu do FCM i prezentuje działanie podrzędne wiadomości przy użyciu Firebase W konsoli."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 4e5bf2b24845fa008c6f97a6d55e18a51bc82164
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Zdalnego powiadomienia z Firebase Cloud Messaging

_Ten przewodnik zawiera szczegółowe informacje dotyczące wykonania zdalnego powiadomienia (nazywanych również powiadomienia wypychane) za pomocą Firebase Cloud Messaging w aplikacji platformy Xamarin.Android. Przedstawia wdrożenie różnych klas, które są potrzebne do komunikacji z wiadomości chmury Firebase (FCM), przykłady sposobu konfigurowania manifestu systemu Android do uzyskiwania dostępu do FCM i prezentuje działanie podrzędne wiadomości przy użyciu Firebase W konsoli._

## <a name="fcm-notifications-overview"></a>Omówienie powiadomienia FCM

W tym przewodniku Podstawowa aplikacja o nazwie **FCMClient** zostanie utworzone w celu zilustrowania essentials FCM obsługi komunikatów. **FCMClient** sprawdza obecność usług Google Play, odbiera tokenów rejestracji z FCM Wyświetla zdalnego powiadomień, które zostanie wysłana z konsoli Firebase i subskrybuje tematu wiadomości:

[![Zrzut ekranu aplikacji](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png)

Następujące obszary zostaną przedstawione:

1.  Powiadomienia tła

2.  Temat wiadomości

3.  Powiadomienia pierwszego planu

W tym przewodniku będzie stopniowego dodawania funkcji do **FCMClient** i uruchom go na urządzeniu lub emulatorze, aby zrozumieć, jak współdziała z FCM. Rejestrowanie umożliwia Monitor transakcje aplikacji na żywo z serwerów FCM i odbywa się w sposób powiadomienia będą generowane z komunikatami FCM wejścia Firebase konsoli powiadomienia graficznego interfejsu użytkownika. 

## <a name="requirements"></a>Wymagania

Zanim będzie można kontynuować z tym przewodnikiem, należy uzyskać poświadczenia niezbędne do korzystania z serwerów FCM firmy Google; Ten proces zostało wyjaśnione w dokumencie [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). W szczególności należy pobrać **google services.json** pliku do użycia z przykładowy kod przedstawiony w tym przewodniku. Jeśli użytkownik jeszcze nie utworzono projekt w konsoli Firebase (lub jeśli nie została jeszcze pobrana **google services.json** pliku), zobacz [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

Aby uruchomić przykładową aplikację, konieczne będzie Android testu urządzenia lub emulatora, który jest compatibile z Firebase. Firebase Cloud Messaging obsługuje klientów z systemem Android 2.3 (pierniki) lub nowszy, a te urządzenia również musi mieć zainstalowanej aplikacji ze sklepu Google Play (Google Play Services 9.2.1 wymagana jest lub nowsza). Jeśli nie masz jeszcze aplikacji ze sklepu Google Play zainstalowanej na urządzeniu, odwiedź stronę [Google Play](https://support.google.com/googleplay) witryny sieci web, aby pobrać i zainstalować go. Alternatywnie możesz użyć emulatora Android SDK, z systemem Android 2.3 lub nowszym zamiast urządzenie testowe (nie trzeba zainstalować sklepu Google Play, jeśli używasz emulatora Android SDK). 

## <a name="start-an-app-project"></a>Projekt aplikacji

Aby rozpocząć, Utwórz nowy pusty projekt platformy Xamarin.Android o nazwie **FCMClient**. Jeśli nie masz doświadczenia w tworzeniu projektów platformy Xamarin.Android, zobacz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md). Po utworzeniu nowej aplikacji, następnym krokiem jest ustawiona na nazwę pakietu i zainstalować kilka pakietów NuGet, które będą używane do komunikacji z FCM. 

### <a name="set-the-package-name"></a>Ustaw nazwę pakietu

W [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md), określone nazwy pakietu dla aplikacji z obsługą FCM. Ta nazwa pakietu służy również jako *identyfikator aplikacji* skojarzonego z kluczem interfejsu API. Skonfiguruj aplikację, aby użyć tej nazwy pakietu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Otwórz właściwości **FCMClient** projektu. 

2.  W **manifestu systemu Android** Ustaw nazwę pakietu. 

W poniższym przykładzie ustawiono nazwę pakietu `com.xamarin.fcmexample`: 

[![Ustawienie nazwy pakietu](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png)

Podczas aktualizowania **manifestu systemu Android**, także Sprawdź, upewnij się, że `Internet` udzielone uprawnienie. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Otwórz właściwości **FCMClient** projektu. 

2.  W **aplikacji systemu Android** Ustaw nazwę pakietu. 

W poniższym przykładzie ustawiono nazwę pakietu `com.xamarin.fcmexample`: 

[![Ustawienie nazwy pakietu](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png)

Podczas aktualizowania **manifestu systemu Android**, także Sprawdź, upewnij się, że `INTERNET` udzielone uprawnienie (w obszarze **wymagane uprawnienia**). 

-----

Należy pamiętać, że aplikacja kliencka będzie nie może odebrać tokenu rejestracji z FCM, jeśli nie jest to nazwa pakietu *dokładnie* pasuje do nazwy pakietu, którą wprowadzono w konsoli Firebase. 

### <a name="add-the-xamarin-google-play-services-base-package"></a>Dodaj pakiet podstawowy usługi Xamarin Google Play

Ponieważ Firebase Cloud Messaging zależy od usług Google Play, [Xamarin usług Google Play - Base](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) pakietu NuGet musi zostać dodany do projektu platformy Xamarin.Android. Konieczne będzie wersji 29.0.0.2 lub nowszej.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  W programie Visual Studio, kliknij prawym przyciskiem myszy **odwołania > Zarządzaj pakietami NuGet...** . 

2.  Kliknij przycisk **Przeglądaj** karcie i wyszukaj **Xamarin.GooglePlayServices.Base**. 

3.  Ten pakiet do zainstalowania **FCMClient** projektu: 

    [ ![Instalowanie Base usług Google Play](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  W programie Visual Studio for Mac, kliknij prawym przyciskiem myszy **pakietów > Dodawanie pakietów...** . 

2.  Wyszukaj **Xamarin.GooglePlayServices.Base**. 

3.  Ten pakiet do zainstalowania **FCMClient** projektu: 

    [ ![Instalowanie Base usług Google Play](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png)

-----

Jeśli wystąpi błąd podczas instalowania NuGet, Zamknij **FCMClient** projektu, otwórz go ponownie i ponów próbę instalacji NuGet. 

Po zainstalowaniu **Xamarin.GooglePlayServices.Base**, również są zainstalowane wszystkie wymagane zależności. Edytuj **MainActivity.cs** i dodaj następującą `using` instrukcji: 

```csharp
using Android.Gms.Common;
```

Ta instrukcja sprawia, że `GoogleApiAvailability` klasy w **Xamarin.GooglePlayServices.Base** dostępne dla **FCMClient** kodu. 
`GoogleApiAvailability` Służy do sprawdzania obecności usług Google Play. 

### <a name="add-the-xamarin-firebase-messaging-package"></a>Dodaj Xamarin Firebase pakietu do obsługi komunikatów

Aby odbierać komunikaty z FCM, [Xamarin Firebase - wiadomości](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) pakietu NuGet musi zostać dodany do projektu aplikacji. Bez tego pakietu aplikacji systemu Android nie mogą otrzymywać wiadomości FCM serwerów.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  W programie Visual Studio, kliknij prawym przyciskiem myszy **odwołania > Zarządzaj pakietami NuGet...** . 

2.  Sprawdź **Uwzględnij wersję wstępną** i wyszukaj **Xamarin.Firebase.Messaging**. 

3.  Ten pakiet do zainstalowania **FCMClient** projektu: 

    [ ![Instalowanie obsługi komunikatów Xamarin Firebase](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  W programie Visual Studio for Mac, kliknij prawym przyciskiem myszy **pakietów > Dodawanie pakietów...** . 

2.  Sprawdź **Pokaż pakiety wersji wstępnej** i wyszukaj **Xamarin.Firebase.Messaging**. 

3.  Ten pakiet do zainstalowania **FCMClient** projektu: 

    [ ![Instalowanie obsługi komunikatów Xamarin Firebase](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png)

-----
 
Po zainstalowaniu **Xamarin.Firebase.Messaging**, również są zainstalowane wszystkie wymagane zależności.

Następnie Edytuj **MainActivity.cs** i dodaj następującą `using` instrukcji:

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

Pierwsze dwie instrukcje Tworzenie typów w **Xamarin.Firebase.Messaging** dostępne dla pakietu NuGet **FCMClient** kodu. **Android.Util** dodaje funkcje rejestrowania, która będzie służyć do przestrzegania transakcje z FMS. 


### <a name="add-the-google-services-json-file"></a>Dodaj plik JSON usługi Google

Następnym krokiem jest dodanie **google services.json** plik do katalogu głównego projektu: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Kopiuj **google services.json** do folderu projektu.

2.  Dodaj **google services.json** do projektu aplikacji (kliknij **Pokaż wszystkie pliki** w **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **google-services.json**, a następnie wybierz pozycję **Include w projekcie**). 

3.  Wybierz **google services.json** w **Eksploratora rozwiązań** okna.

4.  W **właściwości** ustawić okienku **Akcja kompilacji** do **GoogleServicesJson** (Jeśli **GoogleServicesJson** akcji kompilacji nie jest wyświetlany, Zapisz i Zamknij rozwiązanie, a następnie otwórz go ponownie):

    [![Ustawienie akcji kompilacji GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Kopiuj **google services.json** do folderu projektu.

2.  Dodaj **google services.json** do projektu aplikacji. 

3.  Right-click **google-services.json**.

4.  Ustaw **Akcja kompilacji** do **GoogleServicesJson**: 

    [![Ustawienie akcji kompilacji GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png)
 
-----
 

Gdy **google services.json** zostanie dodany do projektu (i **GoogleServicesJson** Akcja kompilowania została ustawiona), proces kompilacji wyodrębnia klienta identyfikator i klucz API, a następnie dodaje te poświadczenia do scalonych / wygenerowany **AndroidManifest.xml** który znajduje się w **obj/Debug/android/AndroidManifest.xml**. Ten proces scalania automatycznie doda żadnych uprawnień i inne elementy FCM, które są wymagane do połączenia z serwerem FCM. 


## <a name="check-for-google-play-services"></a>Sprawdź, czy usługi Google Play

Najpierw zostanie utworzona początkowa układ interfejsu użytkownika aplikacji. Edytuj **Resources/layout/Main.axml** i zastąp jego zawartość XML następujące: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">
    <TextView
        android:text=" "
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/msgText"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:padding="10dp" />
</LinearLayout>
```

To `TextView` będzie używany do wyświetlania komunikatów, które wskazują, czy zainstalowano usług Google Play. Zapisać zmiany w **Main.axml**. Edytuj **MainActivity.cs** i dodaj następujące wystąpienia deklaracji zmiennej do `MainActivity` klasy: 

```csharp
TextView msgText;
```

Google zaleca się, że aplikacje dla systemu Android sprawdzić obecność plik APK usług Google Play przed uzyskaniem dostępu do funkcji usług Google Play (Aby uzyskać więcej informacji, zobacz [Sprawdź usługi Google Play](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)). W poniższym przykładzie `OnCreate` metody zweryfikuje usług Google Play jest dostępna, zanim aplikacja próbuje użyć usługi FCM. Dodaj następującą metodę do `MainActivity` klasy: 

```csharp
public bool IsPlayServicesAvailable ()
{
    int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable (this);
    if (resultCode != ConnectionResult.Success)
    {
        if (GoogleApiAvailability.Instance.IsUserResolvableError (resultCode))
            msgText.Text = GoogleApiAvailability.Instance.GetErrorString (resultCode);
        else
        {
            msgText.Text = "This device is not supported";
            Finish ();
        }
        return false;
    }
    else
    {
        msgText.Text = "Google Play Services is available.";
        return true;
    }
}
```

Ten kod sprawdza urządzenie, aby sprawdzić, czy zainstalowano plik APK usług Google Play. Jeśli nie jest zainstalowany, zostanie wyświetlony komunikat w `TextBox` który powoduje, że użytkownik pobierania APK ze sklepu Google Play (lub ją włączyć w ustawieniach systemu urządzenia). 

Zastąp `OnCreate` metodę z następującym kodem: 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);
    IsPlayServicesAvailable ();
}
```

`IsPlayServicesAvailable` jest wywoływana po zakończeniu `OnCreate` , aby sprawdzić usług Google Play uruchamia się w każdym uruchomieniu aplikacji. Jeśli aplikacja ma `OnResume` metody powinny wywoływać `IsPlayServicesAvailable` z `OnResume` również. Całkowicie ponownie skompilować i uruchomić aplikację. Jeśli jest prawidłowo skonfigurowane, powinien zostać wyświetlony ekran, która wygląda podobnie Poniższy zrzut ekranu: 

[![Aplikacja wskazuje, że usług Google Play jest dostępny](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png)

Jeśli nie tego wyniku, sprawdź, czy plik APK usług Google Play jest zainstalowany na urządzeniu (Aby uzyskać więcej informacji, zobacz [ustawienie zapasową usług Google Play](https://developers.google.com/android/guides/setup)). Sprawdź również, że dodano **Xamarin.Google.Play.Services.Base** pakiet do Twojej **FCMClient** projekt zgodnie z opisem.


## <a name="add-the-instance-id-receiver"></a>Dodaj odbiornika identyfikator wystąpienia

Następnym krokiem jest dodanie usługi, rozszerzający `FirebaseInstanceIdService` do obsługi tworzenia, obracanie i aktualizowanie Firebase tokenów rejestracji. (Aby uzyskać więcej informacji o tokenów rejestracji, zobacz [rejestracji FCM](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).) `FirebaseInstanceIdService` Usługa jest wymagana przez FCM można było wysłać wiadomości do urządzenia. Gdy `FirebaseInstanceIdService` aplikacji zostanie automatycznie komunikaty FCM i wyświetla je jako powiadomienia, gdy aplikacja jest backgrounded dodaniu do aplikacji klienckich usługi. 

### <a name="declare-the-receiver-in-the-android-manifest"></a>Deklarowanie odbiornika w manifestu systemu Android

Edytuj **AndroidManifest.xml** i Wstaw następujący `<receiver>` elementy do `<application>` sekcji: 

```xml
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver"
    android:exported="false" />
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
    </intent-filter>
</receiver>
```

Plik XML wykonuje następujące czynności:

-   Deklaruje `FirebaseInstanceIdReceiver` implementację, która zapewnia [Unikatowy identyfikator](https://developers.google.com/instance-id/) dla każdego wystąpienia aplikacji. Odbiornik również uwierzytelnia i autoryzuje akcje. 

-   Deklaruje wewnętrznego `FirebaseInstanceIdInternalReceiver` implementację, która służy do uruchamiania usług w bezpieczny sposób.

`FirebaseInstanceIdReceiver` Jest `WakefulBroadcastReceiver` odbierająca `FirebaseInstanceId` i `FirebaseMessaging` zdarzeń i dostarcza je do klasy, która pochodzi z `FirebaseInstanceIdService`. 

### <a name="implement-the-firebase-instance-id-service"></a>Wdrożenie usługi Identyfikatora wystąpienia Firebase

Praca rejestrowania aplikacji za pomocą FCM jest obsługiwany przez niestandardowego `FirebaseInstanceIdService` usługi podane. 
`FirebaseInstanceIdService` wykonuje następujące czynności: 

1.  Używa [interfejsu API Instance ID](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) do generowania tokenów zabezpieczeń, które zezwalają na aplikacji klienckiej dostęp do FCM i serwerów aplikacji. W zamian aplikacji otrzymuje w odpowiedzi rejestracji tokenu z FCM. 

2.  Przekazuje token rejestracji do serwerów aplikacji, jeśli aplikacja serwera wymaga.

Dodaj nowy plik o nazwie **MyFirebaseIIDService.cs** i zastąp jego kod szablonu następującym kodem: 

```csharp
using System;
using Android.App;
using Firebase.Iid;
using Android.Util;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class MyFirebaseIIDService : FirebaseInstanceIdService
    {
        const string TAG = "MyFirebaseIIDService";
        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }
        void SendRegistrationToServer(string token)
        {
            // Add custom implementation, as needed.
        }
    }
}
```

Ta usługa implementuje `OnTokenRefresh` metodę, która jest wywoływana, gdy token rejestracji jest początkowo utworzony lub zmienione. Gdy `OnTokenRefresh` działa, pobierania najnowszej tokenu z `FirebaseInstanceId.Instance.Token` właściwości (która jest aktualizowana asynchronicznie przez FCM). W tym przykładzie odświeżenia tokenu jest rejestrowane, aby mógł on zostać wyświetlony w oknie danych wyjściowych: 

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` wywoływana jest rzadko: pozwala zaktualizować token w następujących okolicznościach: 

-   Gdy aplikacja jest instalowania lub odinstalowywania.

-   Gdy użytkownik usuwa dane aplikacji. 

-   Gdy aplikacja Usuwa identyfikator wystąpienia.

-   Gdy naruszono bezpieczeństwo tokenu.

Zgodnie z firmy Google [identyfikator wystąpienia](https://developers.google.com/instance-id/guides/android-implementation) żądanie zostanie obsługi identyfikator wystąpienia FCM dokumentacji, że aplikacja, Odśwież jego token okresowo (zazwyczaj co 6 miesięcy). 

`OnTokenRefresh` wymaga także `SendRegistrationToAppServer` do skojarzenia użytkownika rejestracji tokenu przy użyciu konta po stronie serwera (jeśli istnieją) obsługiwany przez aplikację: 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Ponieważ ta implementacja zależy od projektu serwera aplikacji, treści metody pusty znajduje się w tym przykładzie. Jeśli aplikacja serwera wymaga informacji rejestracyjnych FCM, zmodyfikuj `SendRegistrationToAppServer` do skojarzenia FCM wystąpienia Identyfikatora tokenu z żadnym kontem po stronie serwera obsługiwane przez aplikację. (Należy pamiętać, że token jest nieprzezroczysta dla aplikacji klienckiej). 

Jeśli token jest wysyłany do serwerów aplikacji `SendRegistrationToAppServer` należy zachować wartość boolean wskazująca, czy token został wysłany do serwera. Jeśli ta właściwość ma wartość FAŁSZ, `SendRegistrationToAppServer` wysyła ten token do serwera aplikacji &ndash; w przeciwnym razie token już został wysłany do serwerów aplikacji w poprzednie wywołanie. W niektórych przypadkach (takich jak ta `FCMClient` przykładzie), serwer aplikacji musi tokenu; w związku z tym ta metoda nie jest wymagana w tym przykładzie. 

## <a name="implement-client-app-code"></a>Wdrożenie klienta aplikacji kodu

Teraz, usługi odbiornika znajdują się w miejscu, można zapisać kodu aplikacji klienta korzystać z tych usług. W poniższych sekcjach, zostanie on dodany do interfejsu użytkownika logowania tokenu rejestracji (nazywane również *token Identyfikatora wystąpienia*), i więcej kodu jest dodawana do `MainActivity` Aby wyświetlić `Intent` informacje, gdy aplikacja jest uruchamiana z powiadomienie: 

[![Przycisk Token dziennik dodane do ekranu aplikacji](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png)

### <a name="log-tokens"></a>Tokeny dziennika

Kodzie dodanym w tym kroku jest przeznaczony tylko do celów demonstracyjnych &ndash; aplikację klienta produkcyjnego czy nie ma potrzeby dziennika tokenów rejestracji. Edytuj **Resources/layout/Main.axml** i dodaj następującą `Button` deklaracji natychmiast po `TextView` elementu: 

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

Edytuj **MainActivity.cs** i dodaj następujące deklaracji elementu członkowskiego do `MainActivity` klasy:

```csharp
const string TAG = "MainActivity";
```

Dodaj następujący kod na końcu `OnCreate` metody: 

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

Ten kod rejestruje tokenu bieżącego w oknie danych wyjściowych podczas **dziennika tokenu** dotknięciu jest przycisk. 

### <a name="handle-notification-intents"></a>Obsługa intencje powiadomień

Po naciśnięciu powiadomienia wystawiony na podstawie **FCMClient**, wszelkie dane towarzyszące powiadomienie komunikat jest udostępniony w `Intent` dodatki. Edytuj **MainActivity.cs** i Dodaj następujący kod do góry `OnCreate` — metoda (przed wywołaniem do `IsPlayServicesAvailable`): 

```csharp
if (Intent.Extras != null)
{
    foreach (var key in Intent.Extras.KeySet())
    {
        var value = Intent.Extras.GetString(key);
        Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
    }
}
```

Uruchamianie aplikacji `Intent` jest wywoływane po naciśnięciu komunikatu powiadomienia, dlatego ten kod będzie rejestrować wszystkie odpowiednie dane `Intent` w oknie danych wyjściowych. Jeśli inne `Intent` musi być uruchamiane, `click_action` pola komunikatu powiadomienia musi mieć ustawioną który `Intent` (Uruchom okno `Intent` jest używany, gdy nie `click_action` jest określony). 


## <a name="background-notifications"></a>Powiadomienia tła

Tworzenie i uruchamianie **FCMClient** aplikacji. **Dziennika tokenu** wyświetlany przycisk:

[![Przycisk Token dziennik jest wyświetlany.](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png)

Wybierz **dziennika tokenu** przycisku. W oknie danych wyjściowych IDE powinien zostać wyświetlony komunikat, podobne do następujących: 

[![Token Identyfikatora wystąpienia wyświetlane w oknie danych wyjściowych](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png)

Długość ciągu etykietą **tokenu** jest token Identyfikatora wystąpienia zostanie wklejona do konsoli Firebase &ndash; zaznacz i skopiuj ten ciąg do Schowka. Jeśli nie ma tokenu Identyfikatora wystąpienia, Dodaj następujący wiersz do góry `OnCreate` metody do sprawdzenia, czy **google services.json** poprawnie przeanalizować:

```csharp
Log.Debug(TAG, "google app id: " + Resource.String.google_app_id);
```

`google_app_id` Rejestrowane w oknie danych wyjściowych wartość powinna być zgodna `mobilesdk_app_id` wartość rejestrowane w **google services.json**. 

### <a name="send-a-message"></a>Wysyłanie wiadomości

Zaloguj się do [konsoli Firebase](https://console.firebase.google.com)wybierz projekt, kliknij przycisk **powiadomienia**i kliknij przycisk **wysłać SWÓJ pierwszy komunikat**: 

[![Wyślij przycisk swój pierwszy komunikat](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png)

Na **wiadomości wysyłanych** , wprowadź tekst wiadomości i wybrać opcję **jednego urządzenia**. Skopiuj token Identyfikatora wystąpienia w oknie danych wyjściowych IDE i wklej ją do **tokenu rejestracji FCM** pole konsoli Firebase: 

[![Okno dialogowe tworzenia](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png)

Na urządzeniu z systemem Android (lub emulator) w tle aplikacji, naciskając Android **omówienie** przycisk i dotknięcie ekranu głównego. Gdy urządzenie jest gotowe, kliknij przycisk **WYSYŁANIA komunikatu** w konsoli Firebase: 

[![Wyślij wiadomość przycisku](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png)

Gdy **Przejrzyj komunikat** zostanie wyświetlone okno dialogowe, kliknij przycisk **WYSYŁANIA**.
Ikona powiadomienia powinny być wyświetlane w obszarze powiadomień urządzenia (lub w emulatorze): 

[![Jest wyświetlana ikona powiadomienia](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png)

Otwórz ikonę powiadomienia, aby wyświetlić wiadomość. Komunikat powiadomienia należy dokładnie, co zostało wpisane w **tekst wiadomości** pole konsoli Firebase: 

[![Na urządzeniu zostanie wyświetlony komunikat powiadomienia](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png)

Naciśnij ikonę powiadomienia, aby powrócić do **FCMClient** aplikacji. `Intent` Dodatki wysyłane do **FCMClient** są wyświetlane w oknie danych wyjściowych IDE: 

[![Listy dodatki konwersji z kluczy, identyfikator komunikatu i klucz Zwiń](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png)

W tym przykładzie **z** klucza jest ustawiona wartość numeru projektu Firebase aplikacji (w tym przykładzie `41590732`) i **collapse_key** jest ustawiona na nazwę pakietu ( **COM.xamarin.fcmexample**). Jeśli nie zostanie wyświetlony komunikat, spróbuj usunąć **FCMClient** aplikacji na urządzenia (lub w emulatorze) i powtórz powyższe kroki. 


> [!NOTE]
> **Uwaga:** Jeśli możesz force zamknięcie aplikacji, FCM przestanie dostarczanie powiadomień. Android uniemożliwia transmisji tła przypadkowo lub niepotrzebnie uruchamianie składniki zatrzymania aplikacji. (Aby uzyskać więcej informacji dotyczących tego zachowania, zobacz [uruchamianie formantów w aplikacjach zatrzymania](https://developer.android.com/about/versions/android-3.1.html#launchcontrols).) Z tego powodu konieczne jest, aby ręcznie odinstalować aplikację za każdym razem, uruchomić i zatrzymać go z sesji debugowania &ndash; wymusza FCM, aby wygenerować nowy token, dzięki czemu będzie można odebrać wiadomości.

### <a name="add-a-custom-default-notification-icon"></a>Dodaj niestandardowy domyślnej ikony powiadomień

W poprzednim przykładzie ikonę powiadomienia ma ustawioną ikonę aplikacji. Następujący kod XML konfiguruje niestandardowe domyślnej ikony dla powiadomień. Android Wyświetla ta ikona niestandardowe domyślne wszystkich komunikatów powiadomień, w którym ikonę powiadomienia nie jest jawnie ustawiona. 

Aby dodać niestandardowe domyślnej ikony powiadomień, dodać ikonę tak, aby **obiektów drawable/zasoby** katalogu, Edytuj **AndroidManifest.xml**i Wstaw następujący `<meta-data>` element do `<application>`sekcji: 

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

W tym przykładzie ikonę powiadomienia który znajduje się w **zasobów/obiektów drawable/MF\_stat\_MF\_notification.png** będzie używany jako niestandardowy domyślną ikonę powiadomienia. Jeśli nie skonfigurowano niestandardowe domyślnej ikony w **AndroidManifest.xml** i bez ikony są w ładunku powiadomienia, Android korzysta ikona aplikacji jako ikonę powiadomienia (jak pokazano powyżej powiadomień ikony dla zrzut ekranu). 

## <a name="handle-topic-messages"></a>Obsługa komunikatów w temacie

Kod napisany w tym samym zakresie obsługuje tokenów rejestracji i dodaje funkcjonalność zdalnego powiadomienia do aplikacji. Kolejnym przykładzie dodaje kod, który nasłuchuje *tematu wiadomości* i przekazuje je do użytkownika jako zdalny powiadomienia. Temat wiadomości są FCM wiadomości, które są wysyłane do jednego lub więcej urządzeń, które subskrypcji określonego tematu. Aby uzyskać więcej informacji na temat tematu wiadomości, zobacz [tematu wiadomości](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

### <a name="subscribe-to-a-topic"></a>Subskrypcja tematu

Edytuj **Resources/layout/Main.axml** i dodaj następującą `Button` deklaracji bezpośrednio po poprzednim `Button` elementu:

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

Dodaje plik XML **subskrybować powiadomienia** przycisku do układu.
Edytuj **MainActivity.cs** i Dodaj następujący kod na końcu `OnCreate` metody: 

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

Ten kod lokalizuje **subskrybować powiadomienia** przycisk w układzie i przypisuje jej obsługi kliknięcia kod, który wywołuje `FirebaseMessaging.Instance.SubscribeToTopic`, przekazując subskrybowanego temat _wiadomości_. Po naciśnięciu **Subskrybuj** przycisku subskrybuje aplikacji _wiadomości_ tematu. W poniższej sekcji _wiadomości_ temat wiadomość zostanie wysłana z Firebase konsoli powiadomienia graficznego interfejsu użytkownika. 

### <a name="send-a-topic-message"></a>Wyślij wiadomość tematu

Odinstaluj aplikację, odbuduj go i uruchom go ponownie. Kliknij przycisk **Subskrybowanie powiadomień o** przycisk:

[![Subskrybowanie powiadomień przycisku](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png)

Jeśli aplikacja subskrybowanych pomyślnie, powinien zostać wyświetlony **tematu synchronizacji zakończyło się pomyślnie** w IDE okna wyjściowego: 

[![Okno danych wyjściowych zawiera temat komunikat synchronizacji zakończyło się pomyślnie](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png)

Aby wysłać wiadomość tematu, wykonaj następujące kroki:

1.  Kliknij w konsoli Firebase **nowy komunikat**. 

2.  Na **wiadomości wysyłanych** , wprowadź tekst wiadomości i wybrać opcję **tematu**. 

3.  W **tematu** menu rozwijanego wybierz wbudowanego tematu **wiadomości**: 

    [ ![Wybranie tematu wiadomości](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png)

4.  Na urządzeniu z systemem Android (lub emulator) w tle aplikacji, naciskając Android **omówienie** przycisk i dotknięcie ekranu głównego. 

5.  Gdy urządzenie jest gotowe, kliknij przycisk **WYSYŁANIA komunikatu** w konsoli Firebase. 

6.  Sprawdź okno danych wyjściowych IDE, aby wyświetlić **/tematy/wiadomości** w danych wyjściowych dziennika: 

    [ ![Zostanie wyświetlony komunikat z /topic/news](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png)

Jeśli ten komunikat jest widoczna w oknie danych wyjściowych, ikonę powiadomienia powinien również zostać wyświetlony w obszarze powiadomień na urządzeniu z systemem Android. Otwórz ikonę powiadomienia, aby wyświetlić komunikat tematu: 

[![Komunikat tematu jest wyświetlany jako powiadomienie](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png)

Jeśli nie zostanie wyświetlony komunikat, spróbuj usunąć **FCMClient** aplikacji na urządzenia (lub w emulatorze) i powtórz powyższe kroki. 

## <a name="foreground-notifications"></a>Powiadomienia pierwszego planu

Aby otrzymywać powiadomienia w aplikacji foregrounded, musisz zaimplementować `FirebaseMessagingService`. Ta usługa jest również wymagany do odbierania ładunków danych i wysyłania wiadomości nadrzędnego. Poniższe przykłady przedstawiają sposób zaimplementować to usługa, która rozszerza `FirebaseMessagingService` &ndash; wynikowej aplikacji będzie można obsługiwać powiadomień zdalnego jest uruchomiona na pierwszym planie. 

### <a name="implement-firebasemessagingservice"></a>Implementowanie FirebaseMessagingService

`FirebaseMessagingService` Usługa obejmuje `OnMessageReceived` metodę, która zapisu do obsługi komunikatów przychodzących. Należy pamiętać, że `OnMessageReceived` jest wywoływana komunikatów powiadomień *tylko* gdy aplikacja jest uruchomiona na pierwszym planie. Gdy aplikacja jest uruchomiona w tle, zostanie wyświetlone automatycznie generowanych powiadomienie (jak zostało to pokazane we wcześniejszej części tego przewodnika). 

Dodaj nowy plik o nazwie **MyFirebaseMessagingService.cs** i zastąp jego kod szablonu następującym kodem: 

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Media;
using Android.Util;
using Firebase.Messaging;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    {
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
        }
    }
}
```

Należy pamiętać, że `MESSAGING_EVENT` konwersji filtru musi być zadeklarowana tak, aby nowe komunikaty FCM są kierowane do `MyFirebaseMessagingService`: 

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

Gdy aplikacja kliencka odbiera wiadomości z FCM, `OnMessageReceived` wyodrębnia zawartość komunikatu z przekazany do `RemoteMessage` obiektu przez wywołanie jego `GetNotification` metody. Następnie rejestruje treści wiadomości, aby mógł on zostać wyświetlony w oknie danych wyjściowych IDE: 

```csharp
Log.Debug(TAG, "From: " + message.From);
Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
```

> [!NOTE]
> **Uwaga:** Jeśli Ustaw punkty przerwania w `FirebaseMessagingService`, sesji debugowania mogą lub nie napotkać te punkty przerwania z powodu jak FCM dostarcza wiadomości.
 

### <a name="send-another-message"></a>Wyślij kolejną wiadomość

Odinstalowanie aplikacji, skompilować go ponownie, ponownie uruchom i wykonaj następujące kroki, aby wysłać kolejną wiadomość:

1.  Kliknij w konsoli Firebase **nowy komunikat**. 

2.  Na **wiadomości wysyłanych** , wprowadź tekst wiadomości i wybrać opcję **jednego urządzenia**. 

3.  Skopiuj ciąg tokenu w oknie danych wyjściowych IDE i wklej ją do **tokenu rejestracji FCM** pole konsoli Firebase jak poprzednio. 

4.  Upewnij się, że aplikacja jest uruchomiona na pierwszym planie, a następnie kliknij przycisk **WYSYŁANIA komunikatu** w konsoli Firebase: 

    [ ![Wysyłanie kolejną wiadomość z konsoli](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png)

5.  Gdy **Przejrzyj komunikat** zostanie wyświetlone okno dialogowe, kliknij przycisk **WYSYŁANIA**.

6.  Komunikat przychodzący jest rejestrowany w oknie danych wyjściowych IDE:

    [ ![Treść komunikatu wypisywane w oknie danych wyjściowych](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png)


### <a name="add-a-local-notifications-sender"></a>Dodawanie nadawcy lokalnego powiadomienia

W tym przykładzie pozostałych komunikat przychodzący FCM zostanie skonwertowana na lokalnego powiadomienia, który jest uruchamiany, gdy aplikacja jest uruchomiona na pierwszym planie. Edytuj **MyFirebaseMessageService.cs** i dodaj następującą `using` instrukcji: 

```csharp
using FCMClient;
using System.Collections.Generic;
```

Dodaj następującą metodę do `MyFirebaseMessagingService`: 

```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (string key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }
    var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

    var notificationBuilder = new Notification.Builder(this)
        .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
        .SetContentTitle("FCM Message")
        .SetContentText(messageBody)
        .SetAutoCancel(true)
        .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManager.FromContext(this);
    notificationManager.Notify(0, notificationBuilder.Build());
}
```

Odróżnienie tego powiadomienia z tła powiadomienia, ten kod oznacza powiadomienia za pomocą ikony, która różni się od ikona applicatiion. Dodaj [MF\_stat\_MF\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) do **obiektów drawable/zasoby** i uwzględnić go w **FCMClient** projektu. 

`SendNotification` Używa metody `Notification.Builder` do utworzenia powiadomień, a `NotificationManager` jest używane do uruchomienia powiadomienia. Powiadomienie zawiera `PendingIntent` która umożliwi użytkownikowi Otwórz aplikację i wyświetlić zawartość ciąg przekazany do `messageBody`. W zależności od wersji Android, która ma być docelowa z aplikacją, warto użyć `NotificationCompat.Builder` zamiast tego. Aby uzyskać więcej informacji na temat `Notification.Builder`, zobacz [lokalnego powiadomienia](~/android/app-fundamentals/notifications/local-notifications.md). 

Wywołanie `SendNotification` metody na końcu `OnMessageReceived` metody: 

```csharp
SendNotification(message.GetNotification().Body, message.Data);
```

W wyniku tych zmian `SendNotification` będą uruchamiane przy każdym otrzyma powiadomienie, gdy aplikacja działa na pierwszym planie, a powiadomienie jest wyświetlane w obszarze powiadomień. Jeśli aplikacja jest backgrounded, `SendNotification` nie zostaną uruchomione, a zamiast tego zostanie uruchomiona powiadomienie tła (przedstawione wcześniej w tym przewodniku). 

### <a name="send-the-last-message"></a>Wyślij ostatniego komunikatu

Odinstalowanie aplikacji, skompilować go ponownie, uruchom go ponownie, a następnie do wysłania ostatniego komunikatu, wykonaj następujące kroki:

1.  Kliknij w konsoli Firebase **nowy komunikat**. 

2.  Na **wiadomości wysyłanych** , wprowadź tekst wiadomości i wybrać opcję **jednego urządzenia**. 

3.  Skopiuj ciąg tokenu w oknie danych wyjściowych IDE i wklej ją do **tokenu rejestracji FCM** pole konsoli Firebase jak poprzednio. 

4.  Upewnij się, że aplikacja jest uruchomiona na pierwszym planie, a następnie kliknij przycisk **WYSYŁANIA komunikatu** w konsoli Firebase: 

    [ ![Wysyłanie komunikatu pierwszego planu](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png)

Teraz, komunikat, który został zarejestrowany w oknie danych wyjściowych również jest spakowany w nowe powiadomienie &ndash; ikony powiadomień zostanie wyświetlona na pasku powiadomień, gdy aplikacja jest uruchomiona na pierwszym planie: 

[![Ikony powiadomień dla wiadomości pierwszego planu](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png)

Po otwarciu powiadomienia, powinny pojawić ostatnią wiadomością wysłaną z Firebase konsoli powiadomienia graficznego interfejsu użytkownika: 

[![Powiadomienie pierwszego planu pokazano ikoną pierwszego planu](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png)

 
## <a name="troubleshooting"></a>Rozwiązywanie problemów

Następujące problemy opisane i rozwiązania, które mogą wystąpić podczas z platformy Xamarin.Android przy użyciu Firebase Cloud Messaging.

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp nie został zainicjowany.

W niektórych przypadkach może wystąpić ten komunikat o błędzie:

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

Jest to znany problem, który można obejść przez czyszczenie rozwiązania i ponownie skompilować projekt (**kompilacji > Wyczyść rozwiązanie**, **kompilacji > Kompiluj ponownie rozwiązanie**). Aby uzyskać więcej informacji, zobacz [dyskusjach prowadzonych na forum](https://forums.xamarin.com/discussion/96263/default-firebaseapp-is-not-initialized-in-this-process).


## <a name="summary"></a>Podsumowanie

W tym przewodniku szczegółowe kroki dotyczące implementowania Firebase Cloud Messaging zdalnego powiadomienia w aplikacji platformy Xamarin.Android. On opisany sposób instalowania wymaganych pakietów potrzebne do komunikacji FCM, a jego wyjaśniono sposób konfigurowania manifestu systemu Android do uzyskiwania dostępu do serwerów FCM. Zapewniała przykładowy kod, który ilustruje sposób sprawdzić obecność usług Google Play. Wykazać, sposobu implementacji usługi odbiornika Identyfikatora wystąpienia, który negocjuje z FCM dla tokenu rejestracji i wyjaśniono, jak ten kod tworzy tła powiadomienia, gdy aplikacja jest backgrounded. Go wyjaśniono sposób subskrybowania tematu wiadomości i zapewniała przykładem implementacji usługi odbiornika wiadomości, który jest używany do pobierania i wyświetlać powiadomienia zdalnego, gdy aplikacja jest uruchomiona na pierwszym planie. 


## <a name="related-links"></a>Linki pokrewne

- [FCMNotifications (przykład)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
