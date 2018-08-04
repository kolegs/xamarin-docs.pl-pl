---
title: Zdalne powiadomienia przy użyciu usługi Firebase Cloud Messaging
description: Ten przewodnik zawiera instrukcje krok po kroku wyjaśnienie sposobu przy użyciu usługi Firebase Cloud Messaging zdalne powiadomienia (nazywany także powiadomienia wypychane) w aplikacji platformy Xamarin.Android. Przykład ilustruje sposób implementacji różnych zajęciach, które są wymagane do komunikacji przy użyciu usługi Firebase Cloud Messaging (FCM), zawiera przykłady sposobu konfigurowania manifestu systemu Android do uzyskiwania dostępu do usługi FCM i pokazuje transmisji komunikatów przy użyciu usługi Firebase Konsola.
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: 36ac1be1274ff90d573aa53e5c86ae0a97709505
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514430"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Zdalne powiadomienia przy użyciu usługi Firebase Cloud Messaging

_Ten przewodnik zawiera instrukcje krok po kroku wyjaśnienie sposobu przy użyciu usługi Firebase Cloud Messaging zdalne powiadomienia (nazywany także powiadomienia wypychane) w aplikacji platformy Xamarin.Android. Przykład ilustruje sposób implementacji różnych zajęciach, które są wymagane do komunikacji przy użyciu usługi Firebase Cloud Messaging (FCM), zawiera przykłady sposobu konfigurowania manifestu systemu Android do uzyskiwania dostępu do usługi FCM i pokazuje transmisji komunikatów przy użyciu usługi Firebase Konsola._

## <a name="fcm-notifications-overview"></a>Omówienie powiadomień usługi FCM

W tym przewodniku o nazwie podstawowej aplikacji **FCMClient** zostanie utworzone w celu zilustrowania podstawowych systemów przesyłania wiadomości FCM. **FCMClient** sprawdza obecność usług Google Play, odbiera tokenów rejestracji z usługi FCM, wyświetla zdalne powiadomienia wysyłane z poziomu konsoli usługi Firebase i subskrybowanie tematu wiadomości:

[![Zrzut ekranu aplikacji](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

Będzie można eksplorować tylko następujące obszary:

1.  Powiadomienia w tle

2.  Temat wiadomości

3.  Powiadomienia na pierwszym planie

Z tego instruktażu będą stopniowo dodawania funkcjonalności do **FCMClient** i uruchomienie go na urządzenie lub emulator, aby zrozumieć sposób jej interakcji z usługą FCM. Rejestrowanie będą używane do Monitor działającej aplikacji transakcji z serwerami usługi FCM i odbywa się w sposób powiadomienia są generowane na podstawie wiadomości FCM, które możesz wprowadzić do powiadomienia konsoli Firebase graficznego interfejsu użytkownika.

## <a name="requirements"></a>Wymagania


Będzie on pomocny zapoznać się z [różnego rodzaju wiadomości](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) mogą być wysyłane przez usługi Firebase Cloud Messaging. Obciążenie komunikatu określi, jak aplikacja kliencka będzie odbierania i przetwarzania wiadomości.

Zanim będzie można kontynuować z tego przewodnika, należy uzyskać poświadczenia niezbędne do korzystania z serwerów usługi FCM firmy Google; Ten proces opisano w [usługi Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#setup_fcm).
W szczególności, musisz pobrać **google-services.json** plik do użycia z przykładowym kodem, przedstawione w tym przewodniku. Jeśli użytkownik nie utworzył projektu w konsoli Firebase (lub jeśli nie została jeszcze pobrana **google-services.json** pliku), zobacz [usługi Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).

Aby uruchomić aplikację w przykładzie, konieczne będzie testu w systemie Android urządzenie lub emulator, który jest compatibile przy użyciu usługi Firebase. Usługi firebase Cloud Messaging obsługuje klientów z systemem Android 4.0 lub nowszy, a te urządzenia również musi mieć zainstalowaną aplikację Google Play Store (Google Play Services 9.2.1 lub nowszy jest wymagany). Jeśli nie masz jeszcze aplikacji Google Play Store zainstalowana na urządzeniu, odwiedź stronę [sklepu Google Play](https://support.google.com/googleplay) witryny sieci web, aby pobrać i zainstalować go. Alternatywnie można użyć emulatora zestawu SDK systemu Android za pomocą usługi Google Play zainstalowany, a nie na urządzeniu testowym (nie trzeba zainstalować Google Play Store, jeśli używasz emulatora systemu Android SDK).

## <a name="start-an-app-project"></a>Rozpocznij projekt aplikacji

Aby rozpocząć, Utwórz nowy pusty projekt platformy Xamarin.Android o nazwie **FCMClient**. Jeśli użytkownik nie jest zaznajomiony z tworzeniem projekty platformy Xamarin.Android, zobacz [Witaj, Android](~/android/get-started/hello-android/hello-android-quickstart.md).
Po utworzeniu nowej aplikacji, następnym krokiem jest ustawiona na nazwę pakietu i zainstalować kilka pakietów NuGet, które będą używane do komunikacji z usługą FCM.

### <a name="set-the-package-name"></a>Ustaw nazwę pakietu

W [usługi Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md), możesz określić nazwę pakietu aplikacji z obsługą usługi FCM. Ta nazwa pakietu służy również jako [ *identyfikator aplikacji* ](./firebase-cloud-messaging.md#fcm-in-action-app-id) skojarzony z [klucz interfejsu API](firebase-cloud-messaging.md#fcm-in-action-api-key). Skonfiguruj aplikację, aby użyć tej nazwy pakietu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Otwórz właściwości **FCMClient** projektu.

2.  W **manifestu systemu Android** Ustaw nazwę pakietu.

W poniższym przykładzie ustawiono nazwy pakietu `com.xamarin.fcmexample`:

[![Ustawianie nazwy pakietu](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

Gdy aktualizujesz **manifestu systemu Android**, sprawdź także, upewnij się, że `Internet` udzielone uprawnienie.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Otwórz właściwości **FCMClient** projektu.

2.  W **aplikacji dla systemu Android** Ustaw nazwę pakietu.

W poniższym przykładzie ustawiono nazwy pakietu `com.xamarin.fcmexample`:

[![Ustawianie nazwy pakietu](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

Gdy aktualizujesz **manifestu systemu Android**, sprawdź także, upewnij się, że `INTERNET` udzielone uprawnienie (w obszarze **wymagane uprawnienia**).

-----

> [!IMPORTANT]
> Aplikacja kliencka nie będzie można otrzymywać tokenu rejestracji usługi FCM Jeśli nie jest to nazwa pakietu *dokładnie* jest zgodna z nazwą pakietu, który został wprowadzony w konsoli Firebase.

### <a name="add-the-xamarin-google-play-services-base-package"></a>Dodawanie pakietu programu Xamarin Google Play usługi podstawowej

Ponieważ usługi Firebase Cloud Messaging zależy od usługi Google Play, [Xamarin usług Google Play - Base](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) pakietu NuGet musi zostać dodany do projektu Xamarin.Android. Potrzebna jest wersja 29.0.0.2 lub nowszej.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  W programie Visual Studio, kliknij prawym przyciskiem myszy **odwołania > Zarządzaj pakietami NuGet...** .

2.  Kliknij przycisk **Przeglądaj** kartę i wyszukaj **Xamarin.GooglePlayServices.Base**.

3.  Ten pakiet do zainstalowania **FCMClient** projektu:

    [![Instalowanie bazy usług Google Play](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  W programie Visual Studio dla komputerów Mac, kliknij prawym przyciskiem myszy **pakiety > Dodawanie pakietów...** .

2.  Wyszukaj **Xamarin.GooglePlayServices.Base**.

3.  Ten pakiet do zainstalowania **FCMClient** projektu:

    [![Instalowanie bazy usług Google Play](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

Jeśli wystąpi błąd podczas instalacji pakietu NuGet, Zamknij **FCMClient** projektu, otwórz go ponownie i ponów próbę instalacji NuGet.

Po zainstalowaniu **Xamarin.GooglePlayServices.Base**, wszystkie niezbędne zależności również są zainstalowane. Edytuj **MainActivity.cs** i dodaj następującą `using` instrukcji:

```csharp
using Android.Gms.Common;
```

Ta instrukcja sprawia, że `GoogleApiAvailability` klasy w **Xamarin.GooglePlayServices.Base** dostępne **FCMClient** kodu.
`GoogleApiAvailability` Umożliwia sprawdzanie obecności usług Google Play.

### <a name="add-the-xamarin-firebase-messaging-package"></a>Dodaj pakiet platformy Xamarin usługi Firebase Messaging

Aby odbierać komunikaty z usługi FCM, [Xamarin Firebase - Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) pakietu NuGet musi zostać dodany do projektu aplikacji. Bez tego pakietu aplikacji systemu Android nie może odbierać wiadomości z serwerów usługi FCM.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  W programie Visual Studio, kliknij prawym przyciskiem myszy **odwołania > Zarządzaj pakietami NuGet...** .

2. Wyszukaj **Xamarin.Firebase.Messaging**.

3.  Ten pakiet do zainstalowania **FCMClient** projektu:

    [![Instalacji Xamarin Firebase komunikatów](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  W programie Visual Studio dla komputerów Mac, kliknij prawym przyciskiem myszy **pakiety > Dodawanie pakietów...** .

2.  Sprawdź **Pokaż pakiety w wersji wstępnej** i wyszukaj **Xamarin.Firebase.Messaging**.

3.  Ten pakiet do zainstalowania **FCMClient** projektu:

    [![Instalacji Xamarin Firebase komunikatów](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----

Po zainstalowaniu **Xamarin.Firebase.Messaging**, wszystkie niezbędne zależności również są zainstalowane.

Następnie Edytuj **MainActivity.cs** i dodaj następującą `using` instrukcji:

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

Pierwsze dwie instrukcje Tworzenie typów w **Xamarin.Firebase.Messaging** dostępne dla pakietu NuGet **FCMClient** kodu. **Android.Util** dodaje funkcje rejestrowania, która będzie służyć do obserwowania transakcje FMS.

### <a name="add-googleplayservices-json"></a>Dodaj plik JSON usługi Google

Następnym krokiem jest dodanie **google-services.json** plik do katalogu głównego projektu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Kopiuj **google-services.json** do folderu projektu.

2.  Dodaj **google-services.json** projektu aplikacji (kliknij **Pokaż wszystkie pliki** w **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **google-services.json**, a następnie wybierz **Include w projekcie**).

3.  Wybierz **google-services.json** w **Eksploratora rozwiązań** okna.

4.  W **właściwości** Ustaw okienku **Build Action** do **GoogleServicesJson** (Jeśli **GoogleServicesJson** akcji kompilacji nie jest wyświetlany, Zapisz i Zamknij rozwiązanie, a następnie otwórz go ponownie):

    [![Ustawienie akcji kompilacji GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Kopiuj **google-services.json** do folderu projektu.

2.  Dodaj **google-services.json** projektu aplikacji.

3.  Right-click **google-services.json**.

4.  Ustaw **Akcja kompilacji** do **GoogleServicesJson**:

    [![Ustawienie akcji kompilacji GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)

-----

Gdy **google-services.json** zostanie dodany do projektu (i **GoogleServicesJson** Akcja kompilacji jest ustawiona), proces kompilacji wyodrębnia identyfikator klienta i [klucz interfejsu API](./firebase-cloud-messaging.md#fcm-in-action-api-key) a następnie dodaje te poświadczenia scalone/wygenerowane **AndroidManifest.xml** które znajdują się w **obj/Debug/android/AndroidManifest.xml**. Ten proces scalania automatycznie dodaje wszystkie uprawnienia i inne elementy usługi FCM, które są potrzebne do połączenia z serwerami usługi FCM.


## <a name="check-for-google-play-services-and-create-a-notification-channel"></a>Sprawdź dostępność usług Google Play i tworzenia kanału powiadomień

Google zaleca się, że aplikacje dla systemu Android sprawdzić obecność Google Play Services APK przed uzyskaniem dostępu do funkcji usług Google Play (Aby uzyskać więcej informacji, zobacz [sprawdzać dostępność usług Google Play](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)).

Najpierw zostanie utworzony wstępny układ interfejsu użytkownika aplikacji. Edytuj **Resources/layout/Main.axml** i zastąp jego zawartość następujący kod XML:

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

To `TextView` będzie używany do wyświetlania komunikatów, które wskazują, czy zainstalowano usług Google Play. Czy zapisać zmiany **Main.axml**.


Edytuj **MainActivity.cs** i dodaj następujące zmienne wystąpienia `MainActivity` klasy:

```csharp
public class MainActivity : AppCompatActivity
{
    static readonly string TAG = "MainActivity";

    internal static readonly string CHANNEL_ID = "my_notification_channel";
    internal static readonly int NOTIFICATION_ID = 100;

    TextView msgText;
```

Zmienne `CHANNEL_ID` i `NOTIFICATION_ID` będzie używany w metodzie [ `CreateNotificationChannel` ](#create-notification-channel-code) , zostaną dodane do `MainActivity` później w tym przewodniku.


W poniższym przykładzie `OnCreate` metoda zweryfikuje, usługi Google Play jest dostępna, zanim aplikacja próbuje użyć usługi FCM.
Dodaj następującą metodę do `MainActivity` klasy:

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

Ten kod sprawdza, czy urządzenie, aby sprawdzić, czy zainstalowano program Google Play Services APK. Jeśli nie jest zainstalowany, zostanie wyświetlony komunikat w `TextBox` która instruuje użytkownika, aby pobrać plik APK z Google Play Store (lub ją włączyć w ustawieniach systemu urządzenia).

<a name="create-notification-channel-code"></a>Aplikacje, które są uruchomione w systemie Android 8.0 (poziom interfejsu API 26) lub nowszym należy utworzyć [ _kanału powiadomień_ ](~/android/app-fundamentals/notifications/local-notifications.md) publikowania ich powiadomienia.  Dodaj następującą metodę do `MainActivity` klasy, które spowoduje utworzenie kanału powiadomień (jeśli jest to konieczne):

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var channel = new NotificationChannel(MyFirebaseMessagingService.CHANNEL_ID,
                                          "FCM Notifications",
                                          NotificationImportance.Default)
                  {

                      Description = "Firebase Cloud Messages appear in this channel"
                  };

    var notificationManager = (NotificationManager)GetSystemService(Android.Content.Context.NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Zastąp `OnCreate` metoda następującym kodem:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();

    CreateNotificationChannel();
}
```

`IsPlayServicesAvailable` nazywa się na końcu `OnCreate` , aby sprawdzić usług Google Play, uruchamia się w każdym uruchomieniu aplikacji. Metoda `CreateNotificationChannel` jest wywoływana, aby upewnić się, że kanał powiadomień, istnieje dla urządzeń z systemem Android 8 lub nowszy. Jeśli aplikacja ma `OnResume` metody powinny wywoływać `IsPlayServicesAvailable` z `OnResume` także. Całkowicie ponownie skompilować i uruchomić aplikację. Jeśli wszystko jest prawidłowo skonfigurowany, powinien wyświetlić się ekran, który wygląda jak poniższy zrzut ekranu:

[![Aplikacja oznacza, że usługi Google Play jest dostępna](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

Jeśli nie otrzymasz ten wynik, sprawdź, czy Google Play Services APK jest zainstalowany na urządzeniu z systemem (Aby uzyskać więcej informacji, zobacz [ustawienie zapasowej usług Google Play](https://developers.google.com/android/guides/setup)).
Sprawdź także, że dodano **Xamarin.Google.Play.Services.Base** pakietu usługi **FCMClient** projektu opisany wcześniej.


## <a name="add-the-instance-id-receiver"></a>Dodawanie odbiornika identyfikator wystąpienia

Następnym krokiem jest dodanie to usługa, która rozszerza `FirebaseInstanceIdService` do obsługi tworzenia, obracanie i aktualizowanie [tokenów rejestracji Firebase](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token). `FirebaseInstanceIdService` Usługa jest wymagana przez FCM można było wysyłać komunikaty do urządzenia. Gdy `FirebaseInstanceIdService` aplikacji będą automatycznie otrzymywać wiadomości FCM i wyświetlane jako powiadomienia zawsze wtedy, gdy aplikacja jest backgrounded dodaniu do aplikacji klienckiej usługi.

### <a name="declare-the-receiver-in-the-android-manifest"></a>Zadeklaruj odbiorcy w manifestu systemu Android

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

Poniższy kod XML wykonuje następujące czynności:

-   Deklaruje `FirebaseInstanceIdReceiver` implementację, która zapewnia [Unikatowy identyfikator](https://developers.google.com/instance-id/) dla każdego wystąpienia aplikacji. Odbiornik również uwierzytelnia i autoryzuje akcji.

-   Deklaruje wewnętrzny `FirebaseInstanceIdInternalReceiver` wdrożenia, który jest używany do bezpiecznego uruchamiania usługi.

-   [Identyfikatora aplikacji](./firebase-cloud-messaging.md#fcm-in-action-app-id) są przechowywane w **google-services.json** pliku, który był [dodane do projektu](#add-googleplayservices-json). Powiązań platformy Xamarin.Android Firebase zastąpi token `${applicationId}` identyfikatorem aplikacji; żaden dodatkowy kod nie jest wymagane przez aplikację kliencką do Podaj identyfikator aplikacji.

`FirebaseInstanceIdReceiver` Jest `WakefulBroadcastReceiver` , która otrzymuje `FirebaseInstanceId` i `FirebaseMessaging` zdarzenia i przekazuje je do klasy, która pochodzi z `FirebaseInstanceIdService`.

### <a name="implement-the-firebase-instance-id-service"></a>Wdrożenie usługi Identyfikatora wystąpienia usługi Firebase

Pracę z usługą FCM Trwa rejestrowanie aplikacji jest obsługiwany przez niestandardowy `FirebaseInstanceIdService` usługi przez Ciebie.
`FirebaseInstanceIdService` wykonuje następujące czynności:

1.  Używa [interfejsu API Instance ID](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) do generowania tokenów zabezpieczających, które autoryzowania aplikacji klienckiej dostęp do usługi FCM i serwera aplikacji. W zamian aplikacja otrzymuje [tokenu rejestracji](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token) z usługą FCM.

2.  Przekazuje tokenu rejestracji na serwerze aplikacji, jeśli wymaga serwera aplikacji.

Dodaj nowy plik o nazwie **MyFirebaseIIDService.cs** i zastąp jego kod szablonu poniższym kodem:

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

Ta usługa implementuje `OnTokenRefresh` metodę, która jest wywoływana, gdy token rejestracji jest początkowo tworzone lub zmienione. Gdy `OnTokenRefresh` działa, pobiera najnowszy token z `FirebaseInstanceId.Instance.Token` właściwości (która jest aktualizowana asynchronicznie przez FCM). W tym przykładzie odświeżenia tokenu jest rejestrowane, dzięki czemu mogą być wyświetlane w oknie danych wyjściowych:

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` jest wywoływane rzadko: służy do aktualizacji tokenu w następujących okolicznościach:

-   Gdy aplikacja jest zainstalowane lub odinstalowane.

-   Gdy użytkownik usuwa dane aplikacji.

-   Gdy aplikacja na partycje powoduje usunięcie identyfikatora wystąpienia.

-   Gdy zabezpieczenia tokenu bezpieczeństwo zostało naruszone.

Według firmy Google [identyfikator wystąpienia](https://developers.google.com/instance-id/guides/android-implementation) dokumentacji usługi Identyfikatora wystąpienia usługi FCM będzie żądać jego token okresowo (zazwyczaj co 6 miesięcy) odświeżania aplikacji.

`OnTokenRefresh` Ponadto wywołuje `SendRegistrationToAppServer` do skojarzenia użytkownika rejestracji tokenu przy użyciu konta po stronie serwera (jeśli istnieje) obsługiwany przez aplikację:

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Ponieważ ta implementacja zależy od projektu serwera aplikacji, treści metody pusty znajduje się w tym przykładzie. Jeśli Twój serwer aplikacji wymaga informacji o rejestracji usługi FCM, zmodyfikuj `SendRegistrationToAppServer` skojarzyć token identyfikator wystąpienia usługi FCM użytkownika przy użyciu dowolnego konta po stronie serwera obsługiwanego przez aplikację. (Zwróć uwagę, że token jest nieprzezroczysta dla aplikacji klienckiej).

Gdy token jest wysyłany do serwera aplikacji `SendRegistrationToAppServer` należy zachować wartość logiczną, aby wskazać, czy token został wysłany na serwer. Jeśli ta wartość nie jest spełniony, `SendRegistrationToAppServer` wysyła ten token do serwera aplikacji &ndash; w przeciwnym razie token został już wysłany do poprzedniego wywołania serwera aplikacji. W niektórych przypadkach (np. to `FCMClient` przykład), serwer aplikacji nie wymaga tokenu; w związku z tym, ta metoda nie jest wymagane dla tego przykładu.

## <a name="implement-client-app-code"></a>Implementowanie kodem aplikacji klienta

Skoro usług odbiorcy znajdują się w miejscu, można napisać kod aplikacji klienta z zalet tych usług. W poniższych sekcjach, zostanie on dodany do Interfejsu użytkownika do logowania się tokenu rejestracji (nazywane również *tokenu Identyfikatora wystąpienia*), a więcej kodu jest dodawany do `MainActivity` do wyświetlania `Intent` informacji, gdy aplikacja jest uruchamiana z powiadomienie dotyczące urządzenia:

[![Zaloguj się przycisk Token dodany do ekranu aplikacji](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokes"></a>Tokes dziennika

Kodzie dodanym w tym kroku jest przeznaczona tylko dla celów demonstracyjnych &ndash; musi nie trzeba rejestrować tokenów rejestracji aplikacji klienta produkcyjnego. Edytuj **Resources/layout/Main.axml** i dodaj następującą `Button` deklaracji natychmiast po `TextView` elementu:

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

Dodaj następujący kod na końcu `MainActivity.OnCreate` metody:

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

Ten kod rejestruje bieżącego tokenu w oknie danych wyjściowych po **dziennika tokenu** naciśnięcia przycisku.

### <a name="handle-notification-intents"></a>Obsługa powiadomień intencji

Kiedy użytkownik naciska powiadomienie stamtąd wystawione **FCMClient**, wszelkie dane, towarzyszący powiadomienia komunikat jest udostępniony w `Intent` dodatki. Edytuj **MainActivity.cs** i Dodaj następujący kod na górze `OnCreate` — metoda (przed wywołaniem do `IsPlayServicesAvailable`):

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

Uruchamianie aplikacji `Intent` jest wywoływane, gdy użytkownik wybiera komunikatu powiadomienia, aby ten kod będzie logować się wszystkie odpowiednie dane `Intent` w oknie danych wyjściowych. Jeśli jest to inny `Intent` musi być uruchamiane, `click_action` pola komunikatu powiadomienia musi być równa, `Intent` (Uruchom okno `Intent` jest używany, gdy nie `click_action` jest określony).


## <a name="background-notifications"></a>Powiadomienia w tle

Kompilowanie i uruchamianie **FCMClient** aplikacji. **Dziennika tokenu** wyświetlany przycisk:

[![Przycisk Token dziennik jest wyświetlany.](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

Naciśnij pozycję **dziennika tokenu** przycisku. Komunikat, jak pokazano poniżej, powinny być wyświetlane w oknie danych wyjściowych środowiska IDE:

[![Token Identyfikatora wystąpienia wyświetlane w oknie danych wyjściowych](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

Długi ciąg z etykietą **tokenu** tokenu Identyfikatora wystąpienia, zostaną wklejone do konsoli usługi Firebase &ndash; zaznacz i skopiuj następujący ciąg do Schowka. Jeśli nie ma tokenu Identyfikatora wystąpienia, Dodaj następujący wiersz na początku `OnCreate` metodę, aby sprawdzić, czy **google-services.json** był analizowany poprawnie:

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

`google_app_id` Wartość rejestrowane w oknie danych wyjściowych powinna odpowiadać `mobilesdk_app_id` zarejestrowana w **google-services.json**.

### <a name="send-a-message"></a>Wyślij wiadomość

Zaloguj się do [konsoli Firebase](https://console.firebase.google.com), wybierz swój projekt, kliknij przycisk **powiadomienia**i kliknij przycisk **wysłać Twój pierwszy komunikat**:

[![Twój pierwszy komunikat przycisk Wyślij](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

Na **redagowania wiadomości** strony, wprowadź tekst wiadomości i wybierz **pojedynczego urządzenia**. Skopiuj token Identyfikatora wystąpienia z okna danych wyjściowych środowiska IDE i wklej go do **tokenu rejestracji usługi FCM** konsoli Firebase pola:

[![Okno dialogowe tworzenia](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Na urządzeniu Android (lub emulator) w tle aplikacji, naciskając Android **Przegląd** przycisk i dotknięcie ekranu głównego. Gdy urządzenie jest gotowe, kliknij przycisk **WYSYŁANIA komunikatu** w konsoli Firebase:

[![Wysyłanie komunikatu, przycisk](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

Gdy **Przejrzyj komunikat** zostanie wyświetlone okno dialogowe, kliknij przycisk **WYSYŁANIA**.
Ikona powiadomienia, powinna zostać wyświetlona w obszarze powiadomień urządzenia (lub w emulatorze):

[![Wyświetlana jest ikona powiadomień](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

Otwórz ikonę powiadomienia, aby wyświetlić wiadomość. Komunikat powiadomienia należy dokładnie, co zostało wpisane w **tekst komunikatu** konsoli Firebase pola:

[![Komunikat z powiadomieniem jest wyświetlana na urządzeniu](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

Naciśnij ikonę powiadomienia, aby uruchomić **FCMClient** aplikacji. `Intent` Dodatki wysyłane do **FCMClient** są wyświetlane w oknie danych wyjściowych środowiska IDE:

[![Dodatki intencji listy z klucza, identyfikator komunikatu i klucz Zwiń](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

W tym przykładzie **z** klucz ma wartość numeru projektu Firebase aplikacji (w tym przykładzie `41590732`), a **collapse_key** jest równa jego nazwy pakietu ( **COM.xamarin.fcmexample**).
Jeśli nie otrzymasz komunikat, spróbuj usunąć **FCMClient** aplikacji na urządzeniu (lub w emulatorze) i powtórz powyższe kroki.


> [!NOTE]
> Jeśli użytkownik force Zamknij aplikację, usługi FCM przestanie dostarczanie powiadomień. System android uniemożliwia transmisji tła nieodwracalnie lub niepotrzebnie uruchamianie składniki zatrzymania aplikacji. (Aby uzyskać więcej informacji na temat tego zachowania, zobacz [uruchomić formanty na zatrzymania aplikacji](https://developer.android.com/about/versions/android-3.1.html#launchcontrols).) Z tego powodu konieczne jest o ręczne odinstalowanie aplikacji za każdym razem, możesz go uruchomić i zatrzymać go z sesji debugowania &ndash; wymusza FCM do generowania nowego tokenu, aby komunikaty będą w dalszym ciągu odbioru.

### <a name="add-a-custom-default-notification-icon"></a>Dodaj ikonę powiadomienia niestandardowe domyślne

W poprzednim przykładzie ikonę powiadomienia ustawiono na ikonę aplikacji. Następujący kod XML konfiguruje niestandardowy domyślną ikonę powiadomienia. Android wyświetla tę ikonę niestandardowe domyślne dla wszystkich wiadomości powiadomień, gdzie ikonę powiadomienia nie jest jawnie określona.

Aby dodać ikonę powiadomienia niestandardowe domyślne, należy dodać ikonę tak, aby **zasobów/drawable** katalogu, Edytuj **AndroidManifest.xml**i Wstaw następujący `<meta-data>` elementu do `<application>`sekcji:

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

W tym przykładzie ikonę powiadomienia, znajduje się w **zasobów/drawable/ic\_stat\_ic\_notification.png** będzie służyć jako niestandardowy domyślną ikonę powiadomienia. Jeśli ikona domyślna niestandardowych nie jest skonfigurowany w **AndroidManifest.xml** i ikona nie jest ustawiony w ładunek powiadomienia, system Android używa ikony aplikacji jako ikonę powiadomienia (co przedstawiono na zrzucie ekranu ikonę powiadomienia, które są powyżej).

## <a name="handle-topic-messages"></a>Obsługa komunikatów w temacie

Kod napisany w ten sposób znacznie obsługuje tokenów rejestracji i dodaje funkcjonalność zdalne powiadomienia do aplikacji. W następnym przykładzie dodano kod, który będzie nasłuchiwać pod kątem *tematu wiadomości* i przekazuje je do użytkownika jako zdalne powiadomienia. Temat wiadomości są wiadomości FCM, które są wysyłane do jednego lub więcej urządzeń, które subskrybują do określonego tematu. Aby uzyskać więcej informacji na temat tematu wiadomości zobacz [tematu wiadomości](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).

### <a name="subscribe-to-a-topic"></a>Subskrybowanie tematu

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

Dodaje plik XML **Subskrybowanie powiadomień** przycisku do układu.
Edytuj **MainActivity.cs** i Dodaj następujący kod na końcu `OnCreate` metody:

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

Ten kod lokalizuje **Subskrybowanie powiadomień** przycisku w układzie i przypisuje jej obsługi kliknij do kodu, który wywołuje `FirebaseMessaging.Instance.SubscribeToTopic`, przekazując tematu subskrybowanego _wiadomości_. Kiedy użytkownik naciska **Subskrybuj** przycisku aplikacja subskrybuje _wiadomości_ tematu. W poniższej sekcji _wiadomości_ tematu wiadomości będą wysyłane powiadomienia konsoli Firebase graficznego interfejsu użytkownika.

### <a name="send-a-topic-message"></a>Wyślij wiadomość do tematu

Odinstaluj aplikację, skompilować go ponownie i uruchom go ponownie. Kliknij przycisk **Subskrybowanie powiadomień o** przycisku:

[![Subskrybowanie powiadomień przycisku](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

Jeśli aplikacja zasubskrybował pomyślnie, powinien zostać wyświetlony **temacie synchronizacja zakończyła się pomyślnie** w środowisku IDE okno danych wyjściowych:

[![Okno danych wyjściowych pokazuje komunikat synchronizacji zakończyło się pomyślnie tematu](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

Wykonaj następujące kroki, aby wysłać komunikat do tematu:

1.  W konsoli Firebase kliknij **nowy komunikat**.

2.  Na **redagowania wiadomości** strony, wprowadź tekst wiadomości i wybierz **tematu**.

3.  W **tematu** menu rozwijanego wybierz wbudowanego tematu **wiadomości**:

    [![Wybranie tematu wiadomości](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4.  Na urządzeniu Android (lub emulator) w tle aplikacji, naciskając Android **Przegląd** przycisk i dotknięcie ekranu głównego.

5.  Gdy urządzenie jest gotowe, kliknij przycisk **WYSYŁANIA komunikatu** w konsoli Firebase.

6.  Sprawdź okno danych wyjściowych środowiska IDE, aby zobaczyć **/tematów/wiadomości** w danych wyjściowych dzienników:

    [![Zostanie wyświetlony komunikat z /topic/news](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

Jeśli ten komunikat pojawia się w oknie danych wyjściowych, ikonę powiadomienia powinien pojawić się w obszarze powiadomień na urządzeniu z systemem Android. Otwórz ikonę powiadomienia, aby wyświetlić komunikat tematu:

[![Jako powiadomienie pojawi się komunikat tematu](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

Jeśli nie otrzymasz komunikat, spróbuj usunąć **FCMClient** aplikacji na urządzeniu (lub w emulatorze) i powtórz powyższe kroki.

## <a name="foreground-notifications"></a>Powiadomienia na pierwszym planie

Aby otrzymywać powiadomienia w aplikacjach foregrounded, należy zaimplementować `FirebaseMessagingService`. Ta usługa jest również wymagany do odbierania danych ładunków i wysyłania wiadomości nadrzędnego. W poniższych przykładach pokazano, jak wdrożyć usługę, która rozszerza `FirebaseMessagingService` &ndash; wynikowa aplikacja będzie obsługiwać zdalne powiadomienia, gdy jest uruchomiona na pierwszym planie.

### <a name="implement-firebasemessagingservice"></a>Implementowanie FirebaseMessagingService

`FirebaseMessagingService` Usługa jest odpowiedzialna za odbieranie i przetwarzanie komunikatów z usługi Firebase. Każda aplikacja musi podklasy danego typu i zastąpienie `OnMessageReceived` można przetworzyć komunikatu przychodzącego. Gdy aplikacja działa na pierwszym planie, `OnMessageReceived` wywołania zwrotnego zawsze będzie obsługiwać wiadomości.

> [!NOTE]
> Aplikacje mają tylko 10 sekund, w którym do obsługi wiadomości przychodzące usługi Firebase Cloud. Wszelkie prace, który trwa dłużej, niż jest to powinny być zaplanowane wykonanie tła, takich jak przy użyciu biblioteki [harmonogram zadań dla systemu Android](~/android/platform/android-job-scheduler.md) lub [Dyspozytor zadań rozwiązania Firebase](~/android/platform/firebase-job-dispatcher.md).

Dodaj nowy plik o nazwie **MyFirebaseMessagingService.cs** i zastąp jego kod szablonu poniższym kodem:

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

Należy pamiętać, że `MESSAGING_EVENT` intencji filtru musi być zadeklarowana tak, aby nowe wiadomości FCM są kierowane do `MyFirebaseMessagingService`:

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

Gdy aplikacja kliencka odbiera wiadomości z usługi FCM, `OnMessageReceived` wyodrębnia zawartość komunikatu z przekazanym `RemoteMessage` obiektu przez wywołanie jego `GetNotification` metody. Następnie rejestruje ono zawartości wiadomości, dzięki czemu mogą być wyświetlane w oknie danych wyjściowych środowiska IDE:

```csharp
var body = message.GetNotification().Body;
Log.Debug(TAG, "Notification Message Body: " + body);
```

> [!NOTE]
> Jeśli ustawisz punktów przerwania w `FirebaseMessagingService`, sesja debugowania może lub nie mogą występować te punkty przerwania, ze względu na sposób FCM dostarcza komunikaty.


### <a name="send-another-message"></a>Wyślij kolejną wiadomość

Odinstaluj aplikację, skompilować go ponownie, ponownie uruchom i wykonaj następujące kroki, aby wysłać kolejną wiadomość:

1.  W konsoli Firebase kliknij **nowy komunikat**.

2.  Na **redagowania wiadomości** strony, wprowadź tekst wiadomości i wybierz **pojedynczego urządzenia**.

3.  Skopiuj ciąg tokenu z okna danych wyjściowych środowiska IDE i wklej go do **tokenu rejestracji usługi FCM** pole konsoli Firebase tak jak poprzednio.

4.  Upewnij się, że aplikacja jest uruchomiona na pierwszym planie, a następnie kliknij przycisk **WYSYŁANIA komunikatu** w konsoli Firebase:

    [![Wysyłanie kolejną wiadomość z konsoli](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5.  Gdy **Przejrzyj komunikat** zostanie wyświetlone okno dialogowe, kliknij przycisk **WYSYŁANIA**.

6.  Przychodzący komunikat jest rejestrowany w oknie danych wyjściowych środowiska IDE:

    [![Treść komunikatu wypisywane w oknie danych wyjściowych](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)


### <a name="add-a-local-notification-sender"></a>Dodawanie nadawcy lokalnego powiadomienia

W tym przykładzie pozostały przychodzący komunikat FCM zostaną przekonwertowane na lokalnego powiadomienia, który jest uruchamiany, gdy aplikacja jest uruchomiona na pierwszym planie. Edytuj **MyFirebaseMessageService.cs** i dodaj następującą `using` instrukcji:

```csharp
using FCMClient;
using System.Collections.Generic;
```

Dodaj następującą metodę do `MyFirebaseMessagingService`:

<a name="sendnotification-method"></a>
```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (var key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }

    var pendingIntent = PendingIntent.GetActivity(this,
                                                  MainActivity.NOTIFICATION_ID,
                                                  intent,
                                                  PendingIntentFlags.OneShot);

    var notificationBuilder = new  NotificationCompat.Builder(this, MainActivity.CHANNEL_ID)
                              .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
                              .SetContentTitle("FCM Message")
                              .SetContentText(messageBody)
                              .SetAutoCancel(true)
                              .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(MainActivity.NOTIFICATION_ID, notificationBuilder.Build());
}
```

Aby rozróżnić tego powiadomienia z poziomu powiadomień tła, ten kod oznacza powiadomień za pomocą ikony, która różni się od ikony aplikacji. Dodaj plik [ic\_stat\_ic\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) do **zasobów/drawable** i dołączyć go w **FCMClient** projektu .

`SendNotification` Metoda używa ` NotificationCompat.Builder` do utworzenia powiadomień, a `NotificationManagerCompat` służy do uruchamiania powiadomienia. Powiadomienie zawiera `PendingIntent` która pozwala użytkownikowi Otwórz aplikację i wyświetlić zawartość ciągu przekazany do `messageBody`. Aby uzyskać więcej informacji na temat `NotificationCompat.Builder`, zobacz [powiadomień lokalnych](~/android/app-fundamentals/notifications/local-notifications.md).

Wywołaj `SendNotification` metody na końcu `OnMessageReceived` metody:

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    Log.Debug(TAG, "From: " + message.From);

    var body = message.GetNotification().Body;
    Log.Debug(TAG, "Notification Message Body: " + body);
    SendNotification(body, message.Data);
}
```

W wyniku tych zmian `SendNotification` będą uruchamiane przy każdym otrzyma powiadomienie, gdy aplikacja działa na pierwszym planie i w obszarze powiadomień zostanie wyświetlone powiadomienie.

Gdy aplikacja jest w tle, [ładunku komunikatu](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) określają sposób obsługi wiadomości:

* **Powiadomienie** &ndash; wiadomości zostaną wysłane do **zasobnikiem systemowym**. Lokalne powiadomienie będą tam wyświetlane. Kiedy użytkownik naciska powiadomienie spowoduje uruchomienie aplikacji.
* **Dane** &ndash; komunikaty będą obsługiwane przez `OnMessageReceived`.
* **Zarówno** &ndash; wiadomości, które mają ładunku zarówno powiadomień, jak i dane będą dostarczane na pasku zadań. Po uruchomieniu aplikacji ładunek danych pojawią się w `Extras` z `Intent` który został użyty do uruchomienia aplikacji.

W tym przykładzie, jeśli aplikacja jest backgrounded `SendNotification` zostanie uruchomiony, jeśli wiadomość ma ładunku danych. W przeciwnym razie zostanie uruchomiony powiadomienie tła (przedstawione wcześniej w tym przewodniku).

### <a name="send-the-last-message"></a>Wysyłanie komunikatu ostatniej

Odinstaluj aplikację, skompilować go ponownie, uruchom ją ponownie, a następnie wykonaj następujące kroki, aby wysłać komunikatu ostatniej:

1.  W konsoli Firebase kliknij **nowy komunikat**.

2.  Na **redagowania wiadomości** strony, wprowadź tekst wiadomości i wybierz **pojedynczego urządzenia**.

3.  Skopiuj ciąg tokenu z okna danych wyjściowych środowiska IDE i wklej go do **tokenu rejestracji usługi FCM** pole konsoli Firebase tak jak poprzednio.

4.  Upewnij się, że aplikacja jest uruchomiona na pierwszym planie, a następnie kliknij przycisk **WYSYŁANIA komunikatu** w konsoli Firebase:

    [![Trwa wysyłanie komunikatu pierwszego planu](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

Tym razem komunikat, który zarejestrowano w oknie danych wyjściowych również jest spakowany w nowe powiadomienie &ndash; ikonę powiadomienia pojawi się na pasku powiadomień, gdy aplikacja jest uruchomiona na pierwszym planie:

[![Ikona powiadomienia dla komunikatu pierwszego planu](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

Po otwarciu powiadomienia, powinien zostać wyświetlony ostatni komunikat, który została wysłana z powiadomienia konsoli Firebase graficznego interfejsu użytkownika:

[![Pierwszy plan powiadomień wyświetlane z ikoną pierwszego planu](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)


## <a name="disconnecting-from-fcm"></a>Trwa rozłączanie z usługą FCM

Aby anulować subskrypcję tematu, należy wywołać [UnsubscribeFromTopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29) metody [FirebaseMessaging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging) klasy. Na przykład, aby anulować subskrypcję _wiadomości_ tematu, subskrypcji wcześniej **Anuluj subskrypcję** przycisk może zostać dodany do układu z następującym kodem procedury obsługi:

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

Aby wyrejestrować urządzenie z usługą FCM całkowicie, Usuń identyfikator wystąpienia poprzez wywołanie [DeleteInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29) metody [FirebaseInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) klasy. Na przykład:

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

Usuwa wywołanie tej metody z Identyfikatorem wystąpienia i skojarzonych z nim danych. W rezultacie okresowe wysyłanie danych FCM na urządzeniu jest zatrzymywana.


## <a name="troubleshooting"></a>Rozwiązywanie problemów

Poniższy opis problemy i rozwiązania, które mogą wystąpić w przypadku korzystania z usługi Firebase Cloud Messaging przy użyciu platformy Xamarin.Android.

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp nie został zainicjowany.

W niektórych przypadkach może zostać wyświetlony ten komunikat o błędzie:

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

Jest to znany problem, który można obejść przez czyszczenie rozwiązania i ponownie skompilować projekt (**kompilacji > czyste rozwiązanie**, **kompilacji > Kompiluj rozwiązanie**). Aby uzyskać więcej informacji, zobacz ten [dyskusjach prowadzonych na forum](https://forums.xamarin.com/discussion/96263/default-firebaseapp-is-not-initialized-in-this-process).


## <a name="summary"></a>Podsumowanie

W tym przewodniku szczegółowe kroki dotyczące implementowania usługi Firebase Cloud Messaging zdalne powiadomienia w aplikacji platformy Xamarin.Android. On opisany sposób zainstalować wymagane pakiety, które są wymagane do komunikacji z usługą FCM i jego wyjaśniono, jak skonfigurować manifestu systemu Android do uzyskiwania dostępu do serwerów usługi FCM. Ona udostępniana przykładowy kod, który ilustruje sposób sprawdzić obecność usług Google Play. Wykazać, jak zaimplementować odbiornik usługi Identyfikatora wystąpienia negocjuje z usługą FCM dla tokenu rejestracji i wyjaśniono, jak ten kod tworzy tła powiadomienia, gdy aplikacja jest backgrounded. Jego wyjaśniono sposób subskrybowania tematu wiadomości, a ona udostępniana przykładem implementacji usługi odbiornika wiadomości, która umożliwia odbieranie i wyświetlać powiadomienia zdalne, gdy aplikacja jest uruchomiona na pierwszym planie.


## <a name="related-links"></a>Linki pokrewne

- [FCMNotifications (przykład)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Usługa Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [Temat wiadomości FCM](https://firebase.google.com/docs/cloud-messaging/concept-options)
