---
title: Wysyłanie powiadomień wypychanych z usługi Azure Mobile Apps
description: W tym artykule opisano sposób użycia usługi Azure Notification Hubs można wysyłać powiadomienia wypychane z wystąpienia usługi Azure Mobile Apps do aplikacji platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: A1EF400F-73F4-43E9-A0C3-1569A0F34A3B
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: d5bf0e614ef3777bc956e66c0b737bfb8a5b9e0c
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243769"
---
# <a name="sending-push-notifications-from-azure-mobile-apps"></a>Wysyłanie powiadomień wypychanych z usługi Azure Mobile Apps

_Centra powiadomień Azure zapewnia infrastrukturę skalowalne wypychanych umożliwiający wysyłanie powiadomień wypychanych przenośnych z poziomu dowolnego zaplecza na dowolną platformę mobilną, dzięki czemu na złożoność używanego w wewnętrznej bazie danych o do komunikowania się z różnych systemów powiadomień platformy. W tym artykule opisano sposób użycia usługi Azure Notification Hubs można wysyłać powiadomienia wypychane z wystąpienia usługi Azure Mobile Apps do aplikacji platformy Xamarin.Forms._

> [!VIDEO https://youtube.com/embed/le2lDY22xwM]

**Azure wypychanie przez Centrum powiadomień i platformy Xamarin.Forms, [Xamarin University](https://university.xamarin.com/)**

Powiadomienie wypychane jest używany do dostarczania informacji, takich jak wiadomości z systemem zaplecza do aplikacji na urządzeniu przenośnym w celu zwiększenia aplikacji zaangażowania i użycia. Powiadomienia mogą być wysyłane w dowolnym momencie, nawet wtedy, gdy użytkownik nie jest aktywnie używa aplikacji docelowej.

Systemy zaplecza wysyłać powiadomienia wypychane do urządzeń przenośnych za pomocą systemów powiadomień platformy (PNS), jak pokazano na poniższym diagramie:

[![](azure-images/pns.png "Systemy powiadomień platformy")](azure-images/pns-large.png#lightbox "systemów powiadomień platformy")

System wewnętrznej bazy danych do wysyłania powiadomień wypychanych, kontaktuje się PNS specyficzne dla platformy, aby wysłać powiadomienie do wystąpienia aplikacji klienta. To znacznie zwiększa złożoność wewnętrznej bazy danych powiadomień wypychanych i platform są wymagane, ponieważ wewnętrznej bazy danych musi używać każdego specyficzne dla platformy systemu powiadomień platformy interfejsu API i protokół.

Centra powiadomień Azure eliminuje to złożoność przez zapewniając abstrakcyjność szczegółów różnych systemów powiadomień platformy, dzięki czemu powiadomienia i platform do wysłania przy użyciu jednego wywołania interfejsu API, jak pokazano na poniższym diagramie:

[![](azure-images/notification-hub.png)](azure-images/notification-hub-large.png#lightbox)

Aby wysyłać powiadomienia wypychane, kontakty tylko system Azure Centrum powiadomień, który z kolei komunikuje się z różnych systemów powiadomień platformy, w związku z tym zmniejszenie złożoności wewnętrznej bazy danych zaplecza kodu tego wysyła powiadomienia wypychane.

Aplikacje mobilne platformy Azure ma wbudowaną obsługę powiadomień wypychanych przy użyciu usługi notification hubs. Wysyłanie powiadomień wypychanych z wystąpieniem usługi Azure Mobile Apps do aplikacji platformy Xamarin.Forms proces wygląda następująco:

1. Rejestruje aplikację platformy Xamarin.Forms z systemu powiadomień platformy, która zwraca uchwyt.
1. Wystąpienie usługi Azure Mobile Apps wysyła powiadomienie do jego Centrum powiadomień Azure, określający dojście urządzenia docelowe.
1. Centrum powiadomień Azure wysyła powiadomienie do odpowiedniego systemu powiadomień platformy dla tego urządzenia.
1. System powiadomień platformy wysyła powiadomienie do określonego urządzenia.
1. Aplikacji platformy Xamarin.Forms przetwarza powiadomienia i wyświetla je.

Aplikacja przykładowa prezentuje aplikacji zarządzającej listą, którego dane są przechowywane w wystąpieniu usługi Azure Mobile Apps. Za każdym razem, gdy nowy element jest dodawany do wystąpienia usługi Azure Mobile Apps, powiadomienia wypychane są wysyłane do aplikacji platformy Xamarin.Forms. Poniższe zrzuty ekranu pokazują każdej z platform wyświetlanie powiadomień wypychanych Odebrano:

[![](azure-images/screenshots.png "Przykładowa aplikacja odbierania powiadomień wypychanych")](azure-images/screenshots-large.png#lightbox "Przykładowa aplikacja odbierania powiadomień wypychanych")

Aby uzyskać więcej informacji na temat usługi Azure Notification Hubs, zobacz [usługi Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/) i [Dodawanie powiadomień wypychanych do aplikacji platformy Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push/).

## <a name="azure-and-platform-notification-system-setup"></a>Azure i konfiguracji systemu powiadomień platformy

Integrowanie Centrum powiadomień Azure wystąpienia usługi Azure Mobile Apps proces wygląda następująco:

1. Utwórz wystąpienie usługi Azure Mobile Apps. Aby uzyskać więcej informacji, zobacz [korzystanie z aplikacji mobilnej Azure](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Konfigurowanie Centrum powiadomień. Aby uzyskać więcej informacji, zobacz [Konfigurowanie Centrum powiadomień](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#create-hub).
1. Aktualizuj wystąpienie usługi Azure Mobile Apps do wysyłania powiadomień wypychanych. Aby uzyskać więcej informacji, zobacz [zaktualizować projekt serwera do wysyłania powiadomień wypychanych](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#update-the-server-project-to-send-push-notifications).
1. Zarejestruj się w każdym systemu powiadomień platformy.
1. Konfigurowanie Centrum powiadomień do komunikowania się z każdym systemu powiadomień platformy.

Poniższe sekcje zawierają dodatkowe instrukcje dotyczące konfiguracji dla każdej platformy.

### <a name="ios"></a>iOS

Aby użyć Apple Push Notification Service (APNS) z Centrum powiadomień Azure przeprowadza się następujące dodatkowe czynności:

1. Wygeneruj certyfikat podpisywania żądania certyfikatu wypychania w narzędziu Keychain Access. Aby uzyskać więcej informacji, zobacz [Generowanie pliku żądania podpisania certyfikatu dla certyfikatu wypychania](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#generate-the-certificate-signing-request-file-for-the-push-certificate) w Centrum dokumentacji platformy Azure.
1. Zarejestrować aplikację platformy Xamarin.Forms do obsługi powiadomień wypychanych w Centrum deweloperów firmy Apple. Aby uzyskać więcej informacji, zobacz [zarejestrować aplikację dla powiadomień wypychanych](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-app-for-push-notifications) w Centrum dokumentacji platformy Azure.
1. Utwórz wypychania powiadomień włączone profilu inicjowania obsługi administracyjnej dla aplikacji platformy Xamarin.Forms w Centrum deweloperów firmy Apple. Aby uzyskać więcej informacji, zobacz [Utwórz profil inicjowania obsługi administracyjnej dla aplikacji](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#create-a-provisioning-profile-for-the-app) w Centrum dokumentacji platformy Azure.
1. Konfigurowanie Centrum powiadomień do komunikowania się z usługą APNS. Aby uzyskać więcej informacji, zobacz [Konfigurowanie Centrum powiadomień dla APNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-apns).
1. Konfigurowanie aplikacji platformy Xamarin.Forms do użycia nowego Identyfikatora aplikacji i profilu inicjowania obsługi administracyjnej. Aby uzyskać więcej informacji, zobacz [Konfigurowanie projektu systemu iOS w programie Xamarin Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-xamarin-studio) lub [Konfigurowanie projektu systemu iOS w programie Visual Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-visual-studio) w Centrum dokumentacji platformy Azure.

### <a name="android"></a>Android

Aby użyć Firebase Cloud Messaging (FCM) z Centrum powiadomień Azure przeprowadza się następujące dodatkowe czynności:

1. Zarejestruj się w celu FCM. Klucz interfejsu API serwera i identyfikator klienta są automatycznie generowane i pakowane w `google-services.json` pliku, który zostanie pobrana. Aby uzyskać więcej informacji, zobacz [włączyć Firebase Cloud Messaging (FCM)](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#enable-firebase-cloud-messaging-fcm).
1. Konfigurowanie Centrum powiadomień do komunikowania się z FCM. Aby uzyskać więcej informacji, zobacz [Konfiguruj powrotem aplikacje mobilne kończyć do wysyłania żądań wypychanych przy użyciu FCM](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm).

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Aby używać usługi powiadomień systemu Windows (WNS) z Centrum powiadomień Azure przeprowadza się następujące dodatkowe czynności:

1. Zarejestruj usługi powiadomień systemu Windows (WNS). Aby uzyskać więcej informacji, zobacz [rejestrowanie aplikacji systemu Windows dla powiadomień wypychanych z usługą WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-windows-app-for-push-notifications-with-wns) w Centrum dokumentacji platformy Azure.
1. Konfigurowanie Centrum powiadomień do komunikowania się z usługą WNS. Aby uzyskać więcej informacji, zobacz [Konfigurowanie Centrum powiadomień w przypadku usługi WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-wns) w Centrum dokumentacji platformy Azure.

## <a name="adding-push-notification-support-to-the-xamarinforms-application"></a>Dodawanie obsługi powiadomień wypychanych do aplikacji platformy Xamarin.Forms

W poniższych sekcjach omówiono wdrożenia wymagane w każdym projekcie specyficzne dla platformy do obsługi powiadomień wypychanych.

### <a name="ios"></a>iOS

Proces wdrażania obsługi powiadomień wypychanych w aplikacji systemu iOS jest następująca:

1. Zarejestruj z Notification Service (APNS, Apple Push) w `AppDelegate.FinishedLaunching` metody. Aby uzyskać więcej informacji, zobacz [rejestracji w systemie firmy Apple Push Notification](#ios_register).
1. Implementowanie `AppDelegate.RegisteredForRemoteNotifications` można obsłużyć odpowiedzi dotyczącej rejestracji. Aby uzyskać więcej informacji, zobacz [obsługi odpowiedzi dotyczącej rejestracji](#ios_registration_response).
1. Implementowanie `AppDelegate.DidReceiveRemoteNotification` metody w celu przetworzenia przychodzących powiadomień wypychanych. Aby uzyskać więcej informacji, zobacz [przetwarzania przychodzących powiadomień wypychanych](#ios_process_incoming).

<a name="ios_register" />

### <a name="registering-with-the-apple-push-notification-service"></a>Rejestrowania za pomocą usługi Apple Push Notification Service

Przed aplikacji systemu iOS może odbierać powiadomienia wypychane, zarejestruj z Apple Push Notification Service (APNS), co spowoduje wygenerowania tokenu unikatowych urządzeń i przywrócić go do aplikacji. Rejestracja jest wywoływana w `FinishedLaunching` zastąpienia w `AppDelegate` klasy:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    ...
    var settings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert, new NSSet());

    UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications();
    ...
}
```

Rejestruje aplikacji systemu iOS w usłudze APNS musi określać typy powiadomień wypychanych, który chcesz otrzymywać. `RegisterUserNotificationSettings` Metoda rejestruje typy powiadomień, aplikacja może odbierać, z `RegisterForRemoteNotifications` metoda rejestrowania, aby odbierać powiadomienia wypychane z usługi APNS.

> [!NOTE]
> Nie można wywołać `RegisterUserNotificationSettings` metody spowoduje powiadomień wypychanych jest w trybie dyskretnym odebranych przez aplikację.

<a name="ios_registration_response" />

### <a name="handling-the-registration-response"></a>Obsługa odpowiedzi dotyczącej rejestracji

Żądanie rejestracji APN przebiega w tle. Po odebraniu odpowiedzi z systemem iOS wywoła `RegisteredForRemoteNotifications` zastąpienia w `AppDelegate` klasy:

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyAPNS}
    };

    // Register for push with the Azure mobile app
    Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
    push.RegisterAsync(deviceToken, templates);
}
```

Ta metoda tworzy szablon wiadomości prostego powiadomienia w formacie JSON i rejestruje urządzenia, aby otrzymywać powiadomienia szablonu z Centrum powiadomień.

> [!NOTE]
> `FailedToRegisterForRemoteNotifications` Zastąpienie powinny być implementowane w celu obsługi sytuacji takich jak brak połączenia z siecią. Jest to ważne, ponieważ użytkownicy mogą uruchamiać aplikacji podczas w trybie offline.

<a name="ios_process_incoming" />

### <a name="processing-incoming-push-notifications"></a>Przetwarzanie przychodzących powiadomień wypychanych

`DidReceiveRemoteNotification` Zastąpienia w `AppDelegate` klasa jest używana do przetwarzania przychodzących powiadomień wypychanych, gdy aplikacja jest uruchomiona i jest wywoływane, gdy otrzyma powiadomienie:

```csharp
public override void DidReceiveRemoteNotification(
    UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
    NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

    string alert = string.Empty;
    if (aps.ContainsKey(new NSString("alert")))
        alert = (aps[new NSString("alert")] as NSString).ToString();

    // Show alert
    if (!string.IsNullOrEmpty(alert))
    {
      var notificationAlert = UIAlertController.Create("Notification", alert, UIAlertControllerStyle.Alert);
      notificationAlert.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Cancel, null));
      UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(notificationAlert, true, null);
    }
}
```

`userInfo` Słownik zawiera `aps` klucza, którego wartość jest `alert` słownik z pozostałych danych powiadomień. Ten słownik jest pobierana, z `string` komunikatu powiadomienia są wyświetlane w oknie dialogowym.

> [!NOTE]
> Jeśli aplikacja nie jest uruchomiony po odebraniu powiadomienia wypychane, aplikacja zostanie uruchomiona, ale `DidReceiveRemoteNotification` — metoda nie będzie przetwarzać powiadomienia. Zamiast tego należy pobrać ładunek powiadomienia i odpowiednio odpowiadać z `WillFinishLaunching` lub `FinishedLaunching` zastąpienia.

Aby uzyskać więcej informacji na temat APNS, zobacz [powiadomień wypychanych w systemie iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md).

### <a name="android"></a>Android

Proces wdrażania obsługi powiadomień wypychanych w aplikacji systemu Android wygląda następująco:

1. Dodaj [Xamarin.Firebase.Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet pakietów do projektu systemu Android, a ustawiona wersja docelowa aplikacji systemu Android 7.0 lub nowszy.
1. Dodaj `google-services.json` , pobrany plik z poziomu konsoli Firebase do katalogu głównego projektu systemu Android i jego Akcja kompilacji ustawioną **GoogleServicesJson**. Aby uzyskać więcej informacji, zobacz [Dodaj plik JSON usługi Google](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).
1. Rejestru z wiadomości chmury Firebase (FCM) przez zadeklarowanie odbiornika w manifestu systemu Android plików oraz ich realizacji przez `FirebaseRegistrationService.OnTokenRefresh` metody. Aby uzyskać więcej informacji, zobacz [rejestrowania za pomocą Firebase Cloud Messaging](#android_register_fcm).
1. Zarejestruj w Centrum powiadomień Azure `AzureNotificationHubService.RegisterAsync` metody. Aby uzyskać więcej informacji, zobacz [rejestrowanie w Centrum powiadomień Azure](#android_register_azure).
1. Implementowanie `FirebaseNotificationService.OnMessageReceived` metody w celu przetworzenia przychodzących powiadomień wypychanych. Aby uzyskać więcej informacji, zobacz [wyświetlania zawartości powiadomienia wypychane](#android_displaying_notification).

Aby uzyskać więcej informacji na temat Firebase Cloud Messaging, zobacz [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) i [zdalnego powiadomienia z Firebase Cloud Messaging](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

<a name="android_register_fcm" />

#### <a name="registering-with-firebase-cloud-messaging"></a>Rejestrowanie w Firebase Cloud Messaging

Przed aplikacji systemu Android może odbierać powiadomienia wypychane, należy zarejestrować z FCM, co spowoduje wygenerować token rejestracji i przywrócić go do aplikacji. Aby uzyskać więcej informacji na temat tokenów rejestracji, zobacz [rejestracji FCM](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#registration).

Jest to osiągane przez:

- Deklarowanie odbiornika w manifestu systemu Android. Aby uzyskać więcej informacji, zobacz [deklarowanie odbiornika w manifestu systemu Android](#declaring_a_receiver).
- Wdrażanie usługi Firebase identyfikator wystąpienia. Aby uzyskać więcej informacji, zobacz [implementowania usługi Identyfikatora wystąpienia Firebase](#implementing-firebase-instance-id-service).

<a name="declaring_a_receiver" />

##### <a name="declaring-the-receiver-in-the-android-manifest"></a>Deklarowanie odbiornika w manifestu systemu Android

Edytuj **AndroidManifest.xml** i Wstaw następujący `<receiver>` elementy do `<application>` elementu:

```xml
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
  <intent-filter>
    <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
    <category android:name="${applicationId}" />
  </intent-filter>
</receiver>
```

Plik XML wykonuje następujące operacje:

- Deklaruje wewnętrznego `FirebaseInstanceIdInternalReceiver` implementację, która służy do uruchamiania usług w bezpieczny sposób.
- Deklaruje `FirebaseInstanceIdReceiver` implementację, która zawiera unikatowy identyfikator dla każdego wystąpienia aplikacji. Odbiornik również uwierzytelnia i autoryzuje akcje.

`FirebaseInstanceIdReceiver` Odbiera `FirebaseInstanceId` i `FirebaseMessaging` zdarzeń i dostarcza je do klasy, która jest pochodną `FirebasesInstanceIdService`.

<a name="implementing-firebase-instance-id-service" />

##### <a name="implementing-the-firebase-instance-id-service"></a>Wdrażanie usługi Identyfikatora wystąpienia Firebase

Trwa rejestrowanie aplikacji z FCM jest to osiągane przez wyprowadzanie klasy z `FirebaseInstanceIdService` klasy. Ta klasa jest odpowiedzialny za wygenerowanie tokenów zabezpieczających, które zezwalają na dostęp FCM aplikacji klienta. W przykładowej aplikacji `FirebaseRegistrationService` pochodną klasy `FirebaseInstanceIdService` klasy i przedstawiono w poniższym przykładzie:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    const string TAG = "FirebaseRegistrationService";

    public override void OnTokenRefresh()
    {
        var refreshedToken = FirebaseInstanceId.Instance.Token;
        Log.Debug(TAG, "Refreshed token: " + refreshedToken);
        SendRegistrationTokenToAzureNotificationHub(refreshedToken);
    }

    void SendRegistrationTokenToAzureNotificationHub(string token)
    {
        // Update notification hub registration
        Task.Run(async () =>
        {
            await AzureNotificationHubService.RegisterAsync(TodoItemManager.DefaultManager.CurrentClient.GetPush(), token);
        });
    }
}
```

`OnTokenRefresh` Metoda jest wywoływana, gdy aplikacja odbiera token rejestracji z FCM. Metoda pobiera tokenu z `FirebaseInstanceId.Instance.Token` właściwość, która asynchronicznie jest aktualizowana przez FCM. `OnTokenRefresh` Wywoływana jest metoda rzadko, ponieważ token jest aktualizowana tylko, gdy jest zainstalowany lub odinstalowany, gdy użytkownik usuwa dane aplikacji, gdy aplikacja Usuwa identyfikator wystąpienia aplikacji, lub gdy został zabezpieczeń tokenu naruszenie bezpieczeństwa. Ponadto usługa FCM identyfikator wystąpienia żądania, czy aplikacja odświeża token jego okresowo, zazwyczaj co 6 miesięcy.

`OnTokenRefresh` Również wywołuje metodę `SendRegistrationTokenToAzureNotificationHub` metodę, która jest używana do skojarzenia z Centrum powiadomień Azure tokenu rejestracji użytkownika.

<a name="android_register_azure" />

#### <a name="registering-with-the-azure-notification-hub"></a>Rejestrowanie w Centrum powiadomień Azure

`AzureNotificationHubService` Klasa udostępnia `RegisterAsync` metody, co umożliwi skojarzenie tokenu rejestracji użytkownika w Centrum powiadomień Azure. Poniższy kod przedstawia przykład `RegisterAsync` metody, który jest wywoływany przez `FirebaseRegistrationService` klasy po zmianie tokenu rejestracji:

```csharp
public class AzureNotificationHubService
{
    const string TAG = "AzureNotificationHubService";

    public static async Task RegisterAsync(Push push, string token)
    {
        try
        {
            const string templateBody = "{\"data\":{\"message\":\"$(messageParam)\"}}";
            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBody}
            };

            await push.RegisterAsync(token, templates);
            Log.Info("Push Installation Id: ", push.InstallationId.ToString());
        }
        catch (Exception ex)
        {
            Log.Error(TAG, "Could not register with Notification Hub: " + ex.Message);
        }
    }
}
```

Ta metoda tworzy szablonu powiadomienia prostego komunikatu jako JSON i rejestruje odbierać powiadomienia szablonu z Centrum powiadomień przy użyciu tokenu rejestracji Firebase. Dzięki temu, że powiadomienia wysyłane z Centrum powiadomień Azure będzie obowiązywać reprezentowany przez tokenu rejestracji urządzenia.

<a name="android_displaying_notification" />

#### <a name="displaying-the-contents-of-a-push-notification"></a>Wyświetlanie zawartości powiadomień wypychanych

Wyświetlanie zawartości powiadomień wypychanych jest to osiągane przez wyprowadzanie klasy z `FirebaseMessagingService` klasy. Ta klasa zawiera możliwym do zastąpienia `OnMessageReceived` metodę, która jest wywoływana, gdy po otrzymaniu powiadomienia z FCM, pod warunkiem, że aplikacja jest uruchomiona na pierwszym planie. W przykładowej aplikacji `FirebaseNotificationService` pochodną klasy `FirebaseMessagingService` klasy i przedstawiono w poniższym przykładzie:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseNotificationService : FirebaseMessagingService
{
    const string TAG = "FirebaseNotificationService";

    public override void OnMessageReceived(RemoteMessage message)
    {
        Log.Debug(TAG, "From: " + message.From);

        // Pull message body out of the template
        var messageBody = message.Data["message"];
        if (string.IsNullOrWhiteSpace(messageBody))
            return;

        Log.Debug(TAG, "Notification message body: " + messageBody);
        SendNotification(messageBody);
    }

    void SendNotification(string messageBody)
    {
        var intent = new Intent(this, typeof(MainActivity));
        intent.AddFlags(ActivityFlags.ClearTop);
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

        var notificationBuilder = new NotificationCompat.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
            .SetContentTitle("New Todo Item")
            .SetContentText(messageBody)
            .SetContentIntent(pendingIntent)
            .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))
            .SetAutoCancel(true);

        var notificationManager = NotificationManager.FromContext(this);
        notificationManager.Notify(0, notificationBuilder.Build());
    }
}
```

Gdy aplikacja otrzyma powiadomienie z FCM, `OnMessageReceived` metoda wyodrębnia zawartość wiadomości i wywołuje `SendNotification` metody. Ta metoda Konwertuje zawartość komunikatu lokalnego powiadomienia, który jest uruchamiany, gdy aplikacja jest uruchomiona z powiadomień w obszarze powiadomień.

##### <a name="handling-notification-intents"></a>Obsługa powiadomień intencje

Po naciśnięciu powiadomienia żadnych danych towarzyszące powiadomienie jest udostępniany w `Intent` dodatki. Te dane można wyodrębnić z następującym kodem:

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

Uruchamianie aplikacji `Intent` jest wywoływane po naciśnięciu komunikatu powiadomienia, dlatego ten kod będzie rejestrować wszystkie odpowiednie dane `Intent` w oknie danych wyjściowych.

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Przed Windows platformy Uniwersalnej aplikacji może odbierać powiadomienia wypychane, które należy zarejestrować z usługi WNS (Windows Notification), która będzie zwracać kanału powiadomień. Rejestracja jest wywoływany przez `InitNotificationsAsync` metoda `App` klasy:

```csharp
private async Task InitNotificationsAsync()
{
    var channel = await PushNotificationChannelManager
          .CreatePushNotificationChannelForApplicationAsync();

    const string templateBodyWNS =
        "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

    JObject headers = new JObject();
    headers["X-WNS-Type"] = "wns/toast";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyWNS},
        {"headers", headers} // Needed for WNS.
    };

    await TodoItemManager.DefaultManager.CurrentClient.GetPush()
          .RegisterAsync(channel.Uri, templates);
}
```

Ta metoda pobiera kanału powiadomień wypychanych, tworzy szablon wiadomości powiadomień w formacie JSON i rejestruje urządzenia, aby otrzymywać powiadomienia szablonu z Centrum powiadomień.

`InitNotificationsAsync` Metoda jest wywoływana z `OnLaunched` zastąpienia w `App` klasy:

```csharp
protected override async void OnLaunched(LaunchActivatedEventArgs e)
{
    ...
    await InitNotificationsAsync();
}
```

Dzięki temu, że rejestracji powiadomień wypychanych jest tworzony lub odświeżyć za każdym razem, gdy aplikacja jest uruchamiana, dlatego zapewnienie kanału wypychanych usługi WNS jest zawsze aktywna.

Po otrzymaniu powiadomienia wypychane automatycznie będzie wyświetlana jako *wyskakujące* — niemodalne okno dialogowe zawierające komunikat.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia usługi Azure Notification Hubs można wysyłać powiadomienia wypychane z wystąpienia usługi Azure Mobile Apps do aplikacji platformy Xamarin.Forms. Centra powiadomień Azure zapewnia infrastrukturę skalowalne wypychanych umożliwiający wysyłanie powiadomień wypychanych przenośnych z poziomu dowolnego zaplecza na dowolną platformę mobilną, dzięki czemu na złożoność używanego w wewnętrznej bazie danych o do komunikowania się z różnych systemów powiadomień platformy.


## <a name="related-links"></a>Linki pokrewne

- [Korzystanie z aplikacji mobilnej Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)
- [Dodawanie powiadomień wypychanych do aplikacji platformy Xamarin.Forms](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/)
- [Powiadomienia wypychane w systemie iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)
- [Usługa Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [TodoAzurePush (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)
- [Klientów mobilnych Azure SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
