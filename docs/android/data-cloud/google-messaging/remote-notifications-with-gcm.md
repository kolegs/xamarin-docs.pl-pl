---
title: Powiadomienia zdalnego przy użyciu usługi Google Cloud Messaging
description: Ten przewodnik zawiera szczegółowe informacje dotyczące wykonania zdalnego powiadomienia (nazywanych również powiadomienia wypychane) za pomocą usługi Google Cloud Messaging w aplikacji platformy Xamarin.Android. Zawiera opis różnych klas, które należy zaimplementować do komunikowania się z usługi Google Cloud Messaging (GCM), wyjaśniono, jak ustawić uprawnień w manifestu systemu Android do uzyskiwania dostępu do usługi GCM i pokazuje, wiadomości na trasie przykładowy program test.
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: f4a1451cb848f4da1f595c15d946f4e05292900d
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Powiadomienia zdalnego przy użyciu usługi Google Cloud Messaging

_Ten przewodnik zawiera szczegółowe informacje dotyczące wykonania zdalnego powiadomienia (nazywanych również powiadomienia wypychane) za pomocą usługi Google Cloud Messaging w aplikacji platformy Xamarin.Android. Zawiera opis różnych klas, które należy zaimplementować do komunikowania się z usługi Google Cloud Messaging (GCM), wyjaśniono, jak ustawić uprawnień w manifestu systemu Android do uzyskiwania dostępu do usługi GCM i pokazuje, wiadomości na trasie przykładowy program test._

> [!NOTE]
> GCM została zastąpiona [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM serwera i klienta interfejsów API [są przestarzałe](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) i nie będzie dostępna tak szybko, jak 11 kwietnia 2019.

## <a name="gcm-notifications-overview"></a>Omówienie powiadomienia usługi GCM

W tym przewodniku, utworzymy aplikację platformy Xamarin.Android korzystającą z usługi Google Cloud Messaging (GCM) w celu wykonania zdalnego powiadomienia (znanej także jako *powiadomienia wypychane*). Wdrożymy będzie różne opcje odbiornika usługi i korzystających z usługi GCM dla zdalnego wiadomości i przetestujemy naszych wdrożenia przy użyciu wiersza polecenia programu, która symuluje serwer aplikacji. 

Zauważ, że Firebase Cloud Messaging (FCM) to nowa wersja usługi GCM &ndash; Google zdecydowanie zaleca przy użyciu FCM zamiast usługi GCM. Jeśli aktualnie używasz usługi GCM, zaleca się uaktualnienie do FCM. Aby uzyskać więcej informacji o FCM, zobacz [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

Zanim będzie można kontynuować z tym przewodnikiem, należy uzyskać poświadczenia niezbędne do korzystania z serwerów usługi GCM firmy Google; Ten proces zostało wyjaśnione w dokumencie [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md). W szczególności należy *klucz interfejsu API* i *identyfikator nadawcy* do wstawienia do przykładowy kod przedstawiony w tym przewodniku. 

Aby utworzyć aplikację klienta z obsługą usługi GCM Xamarin.Android użyjemy następujące czynności:

1.  Instalowanie dodatkowych pakietów wymagana do komunikacji z serwerami usługi GCM.
2.  Konfigurowanie aplikacji uprawnień dostępu do serwerów usługi GCM.
3.  Zaimplementuj kod, aby sprawdzić obecność usług Google Play. 
4.  Implementuje konwersji usługi rejestracji i negocjuje z usługi GCM dla tokenu rejestracji.
5.  Wdrożenie usługi odbiornika identyfikator wystąpienia, która nasłuchuje aktualizacji tokenu rejestracji z usługi GCM.
6.  Wdrożenie usługi odbiornika usługi GCM, która odbiera komunikaty zdalnego z serwera aplikacji za pomocą usługi GCM.

Ta aplikacja będzie używać nowej funkcji usługi GCM, znany jako *tematu wiadomości*. W temacie wiadomości, serwera aplikacji wysyła komunikat do tematu, a nie na listę poszczególnych urządzeń. Urządzenia, które subskrypcji danego tematu mogą odbierać komunikaty tematu powiadomień wypychanych. Aby uzyskać więcej informacji na temat usługi GCM tematu wiadomości Zobacz firmy Google [implementacja tematu wiadomości](https://developers.google.com/cloud-messaging/topic-messaging). 

Gdy aplikacja kliencka będzie gotowe, wdrożymy wiersza polecenia języka C# aplikacją, która wyśle powiadomienie wypychane do aplikacji klienta za pośrednictwem usługi GCM. 

## <a name="walkthrough"></a>Wskazówki

Aby rozpocząć, Utwórz nowe puste rozwiązanie o nazwie **RemoteNotifications**. Następnie możemy dodać nowy projekt dla systemu Android do tego rozwiązania opartego na **aplikacji systemu Android** szablonu. Umożliwia wywołanie tego projektu **ClientApp**. (Jeśli nie masz doświadczenia w obsłudze tworzenia projektów platformy Xamarin.Android, zobacz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).) **ClientApp** projektu będzie zawierać kod platformy Xamarin.Android aplikacji klienckiej, która odbiera powiadomienia zdalnego za pośrednictwem usługi GCM. 

### <a name="add-required-packages"></a>Dodawanie wymaganych pakietów

Przed możemy wdrożyć naszego kodu aplikacji klienta, możemy należy zainstalować kilka pakietów, które będą używane do komunikacji z usługą GCM. Ponadto firma Microsoft należy dodać aplikacji ze sklepu Google Play na urządzeniu w naszym nie jest już zainstalowany.

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Dodaj pakiet Xamarin Google Play usługi GCM

Aby odbierać komunikaty z usługi Google Cloud Messaging [usług Google Play](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/) framework musi być obecny na urządzeniu. Bez tej platformy aplikacji systemu Android nie mogą odbierać komunikaty z serwerów usługi GCM. Usług Google Play działa w tle podczas urządzeniu z systemem Android jest włączona, ciche nasłuchiwania dla komunikatów z usługi GCM. Po nadejściu tych wiadomości, usług Google Play konwertuje komunikaty na intencje i następnie wysyła te opcje do aplikacji, które zostały zarejestrowane dla nich. 

W programie Visual Studio, kliknij prawym przyciskiem myszy **odwołania > Zarządzaj pakietami NuGet...** ; w programie Visual Studio for Mac, kliknij prawym przyciskiem myszy **pakietów > Dodawanie pakietów...** . Wyszukaj **Xamarin usług Google Play - GCM** i instalowania tego pakietu do **ClientApp** projektu: 

[![Instalowanie usług Google Play](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

Po zainstalowaniu **Xamarin usług Google Play - GCM**, **Xamarin usług Google Play - Base** jest instalowana automatycznie. Jeśli wystąpi błąd, zmień projektu *Minimum Android do docelowego* inne niż ustawienie na wartość **skompilować przy użyciu zestawu SDK w wersji** i spróbuj ponownie przeprowadzić instalację NuGet. 

Następnie Edytuj **MainActivity.cs** i dodaj następującą `using` instrukcji:

```csharp
using Android.Gms.Common;
using Android.Util;
```

Dzięki temu typów w pakiecie Google Play Services GMS dostępne do naszego kodu i dodaje nowe funkcje rejestrowania, który będzie używany do śledzenia naszych transakcji z GMS. 

#### <a name="google-play-store"></a>Sklepu Google Play

Do odbierania wiadomości z usługi GCM, aplikacji ze sklepu Google Play musi być zainstalowany na urządzeniu. (Przy każdej aplikacji ze sklepu Google Play na urządzeniu ze sklepu Google Play jest również instalowany, więc istnieje duże prawdopodobieństwo, że jest już zainstalowany na urządzeniu testu.) Bez Google Play aplikacji systemu Android nie mogą otrzymywać wiadomości usługi GCM. Jeśli nie masz jeszcze aplikacji ze sklepu Google Play zainstalowanej na urządzeniu, odwiedź stronę [Google Play](https://support.google.com/googleplay) witryny sieci web, aby pobrać i zainstalować ze sklepu Google Play. 

Alternatywnie można użyć emulatora systemu Android z systemem Android 2,2 lub nowszym zamiast urządzenie testowe (nie trzeba zainstalować ze sklepu Google Play na emulatorze systemu Android). Jednak jeśli używasz emulatora, sieci Wi-Fi musi być używana do łączenia usługi GCM i należy otworzyć kilka portów w zaporze sieci Wi-Fi zgodnie z opisem w dalszej części tego przewodnika. 

### <a name="set-the-package-name"></a>Ustaw nazwę pakietu

W [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md), możemy określić nazwy pakietu dla aplikacji z obsługą usługi GCM (Ta nazwa pakietu służy również jako *identyfikator aplikacji* skojarzonego z naszych klucz interfejsu API i identyfikator nadawcy). Teraz Otwórz właściwości **ClientApp** projektu, a następnie ustaw nazwę pakietu ten ciąg. W tym przykładzie mamy ustawioną nazwę pakietu `com.xamarin.gcmexample`:

[![Ustawienie nazwy pakietu](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

Należy pamiętać, że aplikacja kliencka będzie nie może odebrać tokenu rejestracji z usługi GCM, jeśli nie jest to nazwa pakietu *dokładnie* zgodna z nazwą pakietu, które możemy wprowadzone w konsoli dewelopera Google. 

### <a name="add-permissions-to-the-android-manifest"></a>Dodaj uprawnienia do manifestu systemu Android

Aplikacji systemu Android musi mieć następujące uprawnienia przed może odbierać powiadomienia z Google Cloud Messaging skonfigurować: 

-   `com.google.android.c2dm.permission.RECEIVE` &ndash; Udziela uprawnień do naszej aplikacji do rejestrowania i odbieranie komunikatów z usługi Google Cloud Messaging. (Co oznacza `c2dm` oznacza? Oznacza to _chmury do Device Messaging_, który jest obecnie przestarzały poprzednik usługi GCM. 
    Nadal używa usługi GCM `c2dm` w wielu ciągów jego uprawnień.) 

-   `android.permission.WAKE_LOCK` &ndash; (Opcjonalnie) Uniemożliwia urządzeniu Procesora z przejście w tryb uśpienia podczas nasłuchiwania dla wiadomości. 

-   `android.permission.INTERNET` &ndash; Daje dostęp do Internetu dzięki aplikacji klienckiej mogą komunikować się z usługą GCM. 

-   *nazwa_pakietu* `.permission.C2D_MESSAGE` &ndash; rejestruje aplikacji systemu Android i prosi o uprawnienie do odbierania wyłącznie C2D wszystkie komunikaty (w chmurze do urządzenia). *Nazwa_pakietu* prefiks jest taka sama jak swój identyfikator aplikacji. 

Zaplanujemy tych uprawnień w manifestu systemu Android. Umożliwia edytowanie **AndroidManifest.xml** i Zastąp zawartość XML następujące: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" 
    package="YOUR_PACKAGE_NAME" 
    android:versionCode="1" 
    android:versionName="1.0" 
    android:installLocation="auto">
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" />
    <permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" 
                android:protectionLevel="signature" />
    <application android:label="ClientApp" android:icon="@drawable/Icon">
    </application>
</manifest>
```

W powyższym kodzie XML, zmień *YOUR_PACKAGE_NAME* do nazwy pakietu dla projekt aplikacji klienta. Na przykład `com.xamarin.gcmexample`. 

### <a name="check-for-google-play-services"></a>Sprawdź, czy usługi Google Play

W ramach tego przewodnika tworzymy aplikacji kości bez systemu operacyjnego za pomocą jednej `TextView` w interfejsie użytkownika. Ta aplikacja nie oznacza bezpośrednio interakcji z usługą GCM. Zamiast tego będzie obejrzeć w oknie danych wyjściowych, aby zobaczyć, jak naszej aplikacji uzgodnień z usługą GCM, i sprawdzimy na pasku powiadomień dla nowych powiadomień w miarę ich występowania. 

Najpierw utwórz układ obszaru komunikatu. Edytuj **Resources.layout.Main.axml** i Zastąp zawartość XML następujące: 

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

Zapisz **Main.axml** i zamknij go.

Po uruchomieniu aplikacji klienta, chcemy, aby weryfikować dostępność usług Google Play przed możemy próbę skontaktowania się z usługi GCM. Edytuj **MainActivity.cs** i Zastąp ``count`` wystąpienia deklaracji zmiennej następujące deklaracje zmiennych wystąpienie: 

```csharp
TextView msgText;
```

Następnie dodaj następującą metodę do **MainActivity** klasy: 

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
            msgText.Text = "Sorry, this device is not supported";
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

Ten kod sprawdza urządzenie, aby sprawdzić, czy zainstalowano plik APK usług Google Play. Jeśli nie jest zainstalowany, zostanie wyświetlony komunikat w obszarze wiadomości, która sprawia, że użytkownikowi na pobieranie ze sklepu Google Play APK (lub ją włączyć w ustawieniach systemu urządzenia). Ponieważ chcemy uruchomić ten test, po uruchomieniu aplikacji klienta, dodamy wywołanie tej metody na końcu `OnCreate`. 


Następnie zastąp `OnCreate` metodę z następującym kodem:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

Ten kod sprawdza obecność Google Play Services APK i zapisuje wynik w obszarze wiadomości. 

Załóżmy całkowicie ponownie skompilować i uruchomić aplikację. Powinien zostać wyświetlony ekran, która wygląda podobnie Poniższy zrzut ekranu: 

[![Usług Google Play jest dostępna](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

Jeśli nie tego wyniku, sprawdź, czy plik APK usług Google Play jest zainstalowany na urządzeniu oraz że **Xamarin usług Google Play - GCM** pakietu zostanie dodany do Twojej **ClientApp** projekt zgodnie z objaśnieniem wcześniej. Jeśli zostanie wyświetlony błąd kompilacji, spróbuj czyszczenia rozwiązania i ponownie skompilowanie projektu. 

Firma Microsoft będzie dalej, napisz kod umożliwiający skontaktuj się z usługą GCM i wrócić tokenu rejestracji.

### <a name="register-with-gcm"></a>Rejestracji w usłudze GCM

Aby aplikacja może odbierać powiadomienia zdalne z serwera aplikacji, musi rejestracji w usłudze GCM i wrócić tokenu rejestracji. Praca rejestrowania aplikacji w usłudze GCM jest obsługiwany przez `IntentService` który utworzymy. Nasze `IntentService` wykonuje następujące czynności: 

1.  Używa [InstanceID](https://developers.google.com/instance-id/) interfejsu API do generowania tokenów zabezpieczających, które zezwalają na aplikacji klienta dostępu do serwera aplikacji. W zamian możemy odzyskać rejestracji tokenu z usługi GCM.

2.  Przekazuje token rejestracji do serwerów aplikacji (Jeśli aplikacja serwera wymaga).

3.  Subskrybuje jednego lub kilku kanałów temat powiadomienia.

Po wdrożymy to `IntentService`, przetestujemy go, aby zobaczyć, jeśli firma Microsoft odzyskać rejestracji tokenu z usługi GCM.

Dodaj nowy plik o nazwie **RegistrationIntentService.cs** i Zastąp kod szablonu następującym kodem:


```csharp
using System;
using Android.App;
using Android.Content;
using Android.Util;
using Android.Gms.Gcm;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false)]
    class RegistrationIntentService : IntentService
    {
        static object locker = new object();

        public RegistrationIntentService() : base("RegistrationIntentService") { }

        protected override void OnHandleIntent (Intent intent)
        {
            try
            {
                Log.Info ("RegistrationIntentService", "Calling InstanceID.GetToken");
                lock (locker)
                {
                    var instanceID = InstanceID.GetInstance (this);
                    var token = instanceID.GetToken (
                        "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);

                    Log.Info ("RegistrationIntentService", "GCM Registration Token: " + token);
                    SendRegistrationToAppServer (token);
                    Subscribe (token);
                }
            }
            catch (Exception e)
            {
                Log.Debug("RegistrationIntentService", "Failed to get a registration token");
                return;
            }
        }

        void SendRegistrationToAppServer (string token)
        {
            // Add custom implementation here as needed.
        }

        void Subscribe (string token)
        {
            var pubSub = GcmPubSub.GetInstance(this);
            pubSub.Subscribe(token, "/topics/global", null);
        }
    }
}
```

W powyższym przykładowym kodzie zmienić *YOUR_SENDER_ID* do Identyfikatora nadawcy dla projektu aplikacji klienta. Aby uzyskać identyfikator nadawcy dla projektu: 

1.  Zaloguj się do [Google Cloud Console](https://console.cloud.google.com/) i wybierz nazwę projektu z ściągania menu rozwijane. W **projektu informacji** kliknij okienko, która jest wyświetlana dla projektu, **przejdź do ustawień projektu**:

    [![Wybieranie XamarinGCM projektu](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2.  Na **ustawienia** Znajdź pozycję **numer projektu** &ndash; to jest identyfikator nadawcy dla projektu:

    [![Wyświetlany numer projektu](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

Chcemy uruchomić naszych `RegistrationIntentService` podczas uruchamiania aplikacji. Edytuj **MainActivity.cs** i zmodyfikuj `OnCreate` metody, aby naszych `RegistrationIntentService` jest uruchamiany po musimy sprawdzić obecność usług Google Play: 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView(Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    if (IsPlayServicesAvailable ())
    {
        var intent = new Intent (this, typeof (RegistrationIntentService));
        StartService (intent);
    }
}
```

Teraz Spójrzmy w każdej sekcji `RegistrationIntentService` zrozumieć, jak to działa. 

Po pierwsze, możemy adnotacji naszych `RegistrationIntentService` następującym atrybutem, aby wskazać, że nasza usługa nie ma zostać utworzone przez system: 

```csharp
[Service (Exported = false)]
```

`RegistrationIntentService` Konstruktor nazwy wątku roboczego *RegistrationIntentService* ułatwia debugowanie. 

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

Podstawowe funkcje `RegistrationIntentService` znajduje się w `OnHandleIntent` metody. Przejdźmy ten kod, aby zobaczyć, jak rejestruje aplikacji z usługą GCM.


##### <a name="request-a-registration-token"></a>Żądanie tokenu rejestracji

`OnHandleIntent` najpierw wywołuje firmy Google [InstanceID.GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) metoda dla żądania tokenu rejestracji z usługi GCM. Firma Microsoft zawijany ten kod w `lock` do ochrony przed możliwością wiele opcji rejestracji wykonywane równocześnie &ndash; `lock` gwarantuje, że te opcje są przetwarzane sekwencyjnie. Jeśli firma Microsoft nie można pobrać tokenu rejestracji, jest zgłaszany wyjątek i rejestrujemy wystąpił błąd. Jeśli rejestracja zakończy się powodzeniem, `token` ma ustawioną wartość tokenu rejestracji dotarliśmy powrót z usługi GCM: 

```csharp
static object locker = new object ();
...
try
{
    lock (locker)
    {
        var instanceID = InstanceID.GetInstance (this);
        var token = instanceID.GetToken (
            "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);
        ...
    }
}
catch (Exception e)
{
    Log.Debug ...
```

##### <a name="forward-the-registration-token-to-the-app-server"></a>Przekazywania tokenu rejestracji do serwerów aplikacji

Jeśli uzyskujemy token rejestracji (to znaczy nie został zgłoszony wyjątek), nazywamy `SendRegistrationToAppServer` do skojarzenia użytkownika rejestracji tokenu przy użyciu konta po stronie serwera (jeśli istnieją) obsługiwany przez naszą aplikację. Ponieważ ta implementacja zależy od projektu serwera aplikacji, empty — metoda podano tutaj: 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

W niektórych przypadkach serwer aplikacji musi tokenu rejestracji użytkownika; w takim przypadku można pominąć tej metody. Gdy tokenu rejestracji są wysyłane do serwerów aplikacji `SendRegistrationToAppServer` należy zachować wartość boolean wskazująca, czy token został wysłany do serwera. Jeśli ta właściwość ma wartość FAŁSZ, `SendRegistrationToAppServer` wysyła ten token do serwera aplikacji &ndash; w przeciwnym razie token już został wysłany do serwerów aplikacji w poprzednie wywołanie. 


##### <a name="subscribe-to-the-notification-topic"></a>Subskrybowanie powiadomień tematu

Następnie nazywamy naszych `Subscribe` metody, aby wskazać do usługi GCM interesujące do subskrybowania powiadomień tematu. W `Subscribe`, nazywamy [GcmPubSub.Subscribe](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmPubSub.html#subscribe&#40;java.lang.String,%20java.lang.String,%20android.os.Bundle&#41;) interfejsu API do subskrybowania aplikacji klienta do wszystkich wiadomości w obszarze `/topics/global`:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

Serwer aplikacji może wysyłać powiadomienia do `/topics/global` jeśli pracujemy do ich odbierania. Należy pamiętać, że nazwa tematu w obszarze `/topics` może być dowolna, należy tak długo, jak serwerów aplikacji i aplikacja kliencka uzgodnić tych nazw. (W tym miejscu Wybraliśmy nazwę `global` aby wskazać, że firma Microsoft ma odbierać wiadomości we wszystkich tematach obsługiwanej przez serwer aplikacji.) 

Aby uzyskać informacje na temat GCM po stronie serwera do obsługi komunikatów, zobacz firmy Google [wysyłanie komunikatów do tematów](https://developers.google.com/cloud-messaging/topic-messaging). 


#### <a name="implement-an-instance-id-listener-service"></a>Implementacja usługi odbiornika identyfikator wystąpienia

Tokenów rejestracji są unikatowe i bezpieczne; Jednak aplikacja kliencka (lub GCM) może być konieczne odświeżenie tokenu rejestracji w przypadku ponownej instalacji aplikacji lub problem z zabezpieczeniami. Z tego powodu musimy zaimplementować `InstanceIdListenerService` który odpowiada na żądania tokenu odświeżania usługi GCM. 

Dodaj nowy plik o nazwie **InstanceIdListenerService.cs** i Zastąp kod szablonu następującym kodem: 

```csharp
using Android.App;
using Android.Content;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
    class MyInstanceIDListenerService : InstanceIDListenerService
    {
        public override void OnTokenRefresh()
        {
            var intent = new Intent (this, typeof (RegistrationIntentService));
            StartService (intent);
        }
    }
}
```

Dodawanie adnotacji `InstanceIdListenerService` następującym atrybutem oznacza, że usługa ma nie zostać utworzone przez system i może odbierać tokenu rejestracji usługi GCM (nazywane również *identyfikator wystąpienia*) Odśwież żądania: 

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

`OnTokenRefresh` Rozpoczyna się w naszej usługi metody `RegistrationIntentService` tak, aby można go przechwycić nowy token rejestracji.


#### <a name="test-registration-with-gcm"></a>Test rejestracji usługi GCM

Załóżmy całkowicie ponownie skompilować i uruchomić aplikację. Jeśli pojawi się pomyślnie tokenu rejestracji z usługi GCM, tokenu rejestracji powinien zostać wyświetlony w oknie danych wyjściowych. Na przykład: 

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>Obsługa wiadomości podrzędne 

Kod wdrożonych dotychczasowych jest tylko kod "Konfiguracja"; sprawdza, czy usług Google Play jest zainstalowany i negocjuje z usługi GCM i serwerów aplikacji, aby przygotować odbieranie powiadomień zdalnej aplikacji klienta. Jednak mamy jeszcze implementowania kodu faktycznie odbierający i przetwarzający komunikaty powiadomień podrzędne. Aby to zrobić, musimy zaimplementować *usługi odbiornika usługi GCM*. Ta usługa odbiera komunikaty tematu z serwera aplikacji i lokalnie emituje je jako powiadomienia. Po wdrożymy tę usługę, utworzymy program test do wysyłania komunikatów do usługi GCM, dzięki czemu możemy stwierdzić, jeśli implementacja naszej działa prawidłowo. 


#### <a name="add-a-notification-icon"></a>Dodawanie ikony powiadomień

Dodajmy najpierw mała ikona, która będzie wyświetlana w obszarze powiadomień, po uruchomieniu naszych powiadomień. Możesz skopiować [ta ikona](remote-notifications-with-gcm-images/ic-stat-ic-notification.png) do projektu lub Utwórz własną ikon niestandardowych. Firma Microsoft będzie Nazwij plik ikony **ic_stat_button_click.png** i skopiuj go do **obiektów drawable/zasoby** folderu. Pamiętaj, aby użyć **Dodaj > istniejący element...**  Aby dołączyć ten plik ikony do projektu.


#### <a name="implement-a-gcm-listener-service"></a>Implementacja usługi odbiornika usługi GCM

Dodaj nowy plik o nazwie **GcmListenerService.cs** i Zastąp kod szablonu następującym kodem:

```csharp
using Android.App;
using Android.Content;
using Android.OS;
using Android.Gms.Gcm;
using Android.Util;

namespace ClientApp
{
    [Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
    public class MyGcmListenerService : GcmListenerService
    {
        public override void OnMessageReceived (string from, Bundle data)
        {
            var message = data.GetString ("message");
            Log.Debug ("MyGcmListenerService", "From:    " + from);
            Log.Debug ("MyGcmListenerService", "Message: " + message);
            SendNotification (message);
        }

        void SendNotification (string message)
        {
            var intent = new Intent (this, typeof(MainActivity));
            intent.AddFlags (ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity (this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new Notification.Builder(this)
                .SetSmallIcon (Resource.Drawable.ic_stat_ic_notification)
                .SetContentTitle ("GCM Message")
                .SetContentText (message)
                .SetAutoCancel (true)
                .SetContentIntent (pendingIntent);

            var notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
            notificationManager.Notify (0, notificationBuilder.Build());
        }
    }
}
```

Spójrzmy na każdej sekcji naszych `GcmListenerService` zrozumieć, jak to działa. 

Po pierwsze, możemy adnotacji `GcmListenerService` z atrybutem oznacza, że ta usługa nie ma zostać utworzone przez system i przeprowadzamy konwersji filtru, aby wskazać, że odbiera komunikaty GCM: 

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

Gdy `GcmListenerService` odbiera komunikat z usługi GCM, `OnMessageReceived` wywołania metody. Ta metoda wyodrębnia zawartość komunikatu z przekazany do `Bundle`, rejestruje zawartość komunikatu (tak, aby firma Microsoft mogą je wyświetlać w oknie danych wyjściowych) i wywołania `SendNotification` można uruchomić lokalnego powiadomienia o zawartości odebranego komunikatu: 

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

`SendNotification` Używa metody `Notification.Builder` można utworzyć powiadomienia, a następnie używa `NotificationManager` można uruchomić powiadomienia. Ostatecznie to konwertuje komunikatu powiadomienia zdalnego lokalnego powiadomienia będą widoczne dla użytkownika.
Aby uzyskać więcej informacji o korzystaniu z `Notification.Builder` i `NotificationManager`, zobacz [lokalnego powiadomienia](~/android/app-fundamentals/notifications/local-notifications.md).


#### <a name="declare-the-receiver-in-the-manifest"></a>Deklarowanie odbiornika w manifeście

Zanim firma Microsoft może odbierać komunikaty z usługi GCM, firma Microsoft musi zadeklarować odbiornika usługi GCM w manifestu systemu Android. Umożliwia edytowanie **AndroidManifest.xml** i Zastąp `<application>` sekcji z następującego kodu XML: 

```xml
<application android:label="RemoteNotifications" android:icon="@drawable/Icon">
    <receiver android:name="com.google.android.gms.gcm.GcmReceiver" 
              android:exported="true" 
              android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="YOUR_PACKAGE_NAME" />
        </intent-filter>
    </receiver>
</application>
```

W powyższym kodzie XML, zmień *YOUR_PACKAGE_NAME* do nazwy pakietu dla projekt aplikacji klienta. W naszym przykładzie wskazówki nazwa pakietu jest `com.xamarin.gcmexample`. 

Zobaczmy, czego poszczególnych ustawień w tym pliku XML:

|Ustawienie|Opis|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|Deklaruje, że aplikacji implementuje odbiornika usługi GCM, która przechwytuje i przetwarza wiadomości przychodzących powiadomień wypychanych.|
|`com.google.android.c2dm.permission.SEND`|Deklaruje tylko serwery usługi GCM może wysyłać komunikaty bezpośrednio do aplikacji.|
|`com.google.android.c2dm.intent.RECEIVE`|Filtr konwersji reklamy, jaki aplikacji obsługuje emisji komunikatów z usługi GCM.|
|`com.google.android.c2dm.intent.REGISTRATION`|Filtr konwersji reklamy, jaki aplikacji obsługuje nowe opcje rejestracji (to znaczy wdrożonych wystąpienia Identyfikatora odbiornika usługi).|

Alternatywnie można dekoracji `GcmListenerService` z tych atrybutów, a nie określając je w kodzie XML; w tym miejscu możemy określić je w **AndroidManifest.xml** tak, aby przykłady kodu są łatwiejsze do wykonania. 


### <a name="create-a-message-sender-to-test-the-app"></a>Tworzenie nadawcy wiadomości do testowania aplikacji

Ta funkcja pozwala dodać projekt aplikacji konsoli pulpitu C# do rozwiązania i nadaj mu **MessageSender**. Aby symulować serwer aplikacji użyjemy tej aplikacji konsoli &ndash; wysyła komunikaty powiadomień do **ClientApp** za pośrednictwem usługi GCM. 


#### <a name="add-the-jsonnet-package"></a>Dodaj pakiet Json.NET

W tej aplikacji konsoli tworzymy ładunek JSON, który zawiera komunikat powiadomienia, którą chcemy, aby wysłać do aplikacji klienckiej. Użyjemy **Json.NET** pakietu w **MessageSender** ułatwiające tworzenie obiektu JSON, wymagane przez usługi GCM. W programie Visual Studio, kliknij prawym przyciskiem myszy **odwołania > Zarządzaj pakietami NuGet...** ; w programie Visual Studio for Mac, kliknij prawym przyciskiem myszy **pakietów > Dodawanie pakietów...** . 

Umożliwia wyszukiwanie **Json.NET** pakietu, a następnie zainstaluj go w projekcie: 

[![Instalowanie pakietu struktury Json.NET](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)


#### <a name="add-a-reference-to-systemnethttp"></a>Dodaj odwołanie do System.Net.Http

Ponadto musisz dodać odwołanie do `System.Net.Http` tak, aby firma Microsoft może utworzyć wystąpienia `HttpClient` do wysyłania wiadomości testu do usługi GCM. W **MessageSender** projektu, kliknij prawym przyciskiem myszy **odwołania > Dodaj odwołanie** i przewiń w dół do momentu wyświetlenia **System.Net.Http**. Umieść znacznik wyboru obok pozycji **System.Net.Http** i kliknij przycisk **OK**. 


#### <a name="implement-code-that-sends-a-test-message"></a>Zaimplementuj kod, który wysyła wiadomość testową

W **MessageSender**, Edytuj **Program.cs** i Zastąp zawartość następującym kodem:

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json.Linq;

namespace MessageSender
{
    class MessageSender
    {
        public const string API_KEY = "YOUR_API_KEY";
        public const string MESSAGE = "Hello, Xamarin!";

        static void Main (string[] args)
        {
            var jGcmData = new JObject();
            var jData = new JObject();

            jData.Add ("message", MESSAGE);
            jGcmData.Add ("to", "/topics/global");
            jGcmData.Add ("data", jData);

            var url = new Uri ("https://gcm-http.googleapis.com/gcm/send");
            try
            {
                using (var client = new HttpClient())
                {
                    client.DefaultRequestHeaders.Accept.Add(
                        new MediaTypeWithQualityHeaderValue("application/json"));

                    client.DefaultRequestHeaders.TryAddWithoutValidation (
                        "Authorization", "key=" + API_KEY);

                    Task.WaitAll(client.PostAsync (url,
                        new StringContent(jGcmData.ToString(), Encoding.Default, "application/json"))
                            .ContinueWith(response =>
                            {
                                Console.WriteLine(response);
                                Console.WriteLine("Message sent: check the client device notification tray.");
                            }));
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Unable to send GCM message:");
                Console.Error.WriteLine(e.StackTrace);
            }
        }
    }
}
```

W powyższym kodzie zmienić *YOUR_API_KEY* do klucza interfejsu API dla projekt aplikacji klienta. 

Serwer aplikacji test wysyła komunikat formacie JSON do usługi GCM:

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

Usługi GCM, przekazuje z kolei tej wiadomości do aplikacji klienta. Utworzymy **MessageSender** i Otwórz okno konsoli, gdzie możemy uruchomić go z wiersza polecenia.



### <a name="try-it"></a>Wypróbuj!

Teraz już wszystko gotowe do testowania aplikacji klienta. Jeśli używasz emulatora lub jeśli urządzenie komunikuje się z usługą GCM za pośrednictwem sieci Wi-Fi, należy otworzyć następujące porty TCP na zaporze komunikatów GCM mogą uzyskać za pomocą: 5228, 5229 i 5230.

Uruchom aplikację klienta i obejrzeć w oknie danych wyjściowych. Po `RegistrationIntentService` pomyślnie otrzymuje rejestracji tokenu z usługi GCM, w oknie danych wyjściowych powinien być wyświetlany token z dziennika dane wyjściowe podobne do następujących:

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

W tym momencie aplikacji klienckiej jest gotowa do odbierania komunikatu powiadomienia zdalnego. W wierszu polecenia Uruchom **MessageSender.exe** programu można wysłać komunikatu powiadomienia "Hello, Xamarin" do aplikacji klienckiej.
Jeśli nie ma jeszcze skompilowany **MessageSender** projekt, zrób to teraz.

Aby uruchomić **MessageSender.exe** w programie Visual Studio, otwórz wiersz polecenia, zmień **MessageSender/bin/Debug** katalogu, a następnie uruchom polecenie bezpośrednio:

```cmd
MessageSender.exe
```

Aby uruchomić **MessageSender.exe** w programie Visual Studio dla komputerów Mac, otwórz sesję terminala, zmień **MessageSender/bin/Debug** katalogu i mono używany do uruchamiania **MessageSender.exe** 

```bash
mono MessageSender.exe
```

Może potrwać na minutę na propagację za pośrednictwem usługi GCM i powrót do aplikacji klienckiej wiadomości. Jeśli wiadomość zostanie odebrana pomyślnie, możemy powinny być widoczne dane wyjściowe podobne do następujących w oknie danych wyjściowych: 

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

Ponadto należy zauważyć, że ikona powiadomienia o nowym okazało się na pasku powiadomień: 

[![Na urządzeniu pojawi się ikona Notiication](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

Po otwarciu na pasku powiadomień na wyświetlanie powiadomień, powinny być widoczne naszych zdalnego powiadomień:

[![Zostanie wyświetlony komunikat powiadomienia](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

Gratulacje, aplikację otrzymała jej pierwszego powiadomienia zdalnego!

Należy pamiętać, że komunikaty GCM nie otrzyma Jeśli aplikacja jest zatrzymany w życie. Aby wznowić powiadomienia po zatrzymaniu force, aplikacja musi być ręcznie ponownie uruchomić. Aby uzyskać więcej informacji o tych zasadach systemu Android, zobacz [uruchamianie formantów w aplikacjach zatrzymania](https://developer.android.com/about/versions/android-3.1.html#launchcontrols) i to [post przepełnienie stosu](http://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267). 

 
## <a name="summary"></a>Podsumowanie

W tym przewodniku szczegółowe kroki dla wykonania zdalnego powiadomienia w aplikacji platformy Xamarin.Android. On opisany sposób instalowania dodatkowych pakietów potrzebne do komunikacji usługi GCM, a jego wyjaśniono sposób konfigurowania aplikacji uprawnień dostępu do serwerów usługi GCM. Zapewniała przykładowy kod, która ilustruje sposób sprawdzić obecność usług Google Play, implementowania rejestracji usługi konwersji oraz usługi odbiornika identyfikator wystąpienia, który negocjuje z usługi GCM dla tokenu rejestracji i implementowania odbiornika usługi GCM Usługa, która odbiera i przetwarza komunikaty powiadomień dotyczących zdalnego. Na koniec zaimplementowano program testu w wierszu polecenia, aby wysłać powiadomienia testowe do naszej aplikacji klienckich za pośrednictwem usługi GCM. 


## <a name="related-links"></a>Linki pokrewne

- [RemoteNotifications GCM (przykład)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications)
- [Usługa Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
