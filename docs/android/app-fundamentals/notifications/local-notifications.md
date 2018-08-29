---
title: Powiadomienia lokalne
description: W tej sekcji pokazano, jak wdrożyć powiadomienia lokalne na platformie Xamarin.Android. Opisano różne elementy interfejsu użytkownika dla systemu Android powiadomienia, a w tym artykule omówiono interfejs API użytkownika związane z tworzeniem i wyświetlanie powiadomienie.
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/16/2018
ms.openlocfilehash: 221fa9b70eeba2c4ca08433c627e5648470a7fac
ms.sourcegitcommit: 7ffbecf4a44c204a3fce2a7fb6a3f815ac6ffa21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/28/2018
ms.locfileid: "39514534"
---
<a name="compatibility"></a>

# <a name="local-notifications"></a>Powiadomienia lokalne

_W tej sekcji pokazano, jak wdrożyć powiadomienia lokalne na platformie Xamarin.Android. Opisano różne elementy interfejsu użytkownika dla systemu Android powiadomienia, a w tym artykule omówiono interfejs API użytkownika związane z tworzeniem i wyświetlanie powiadomienie._

## <a name="local-notifications-overview"></a>Powiadomienia lokalne — Przegląd

Android zawiera dwa obszary kontrolowane przez system wyświetlanie ikony powiadomień i powiadomienia dotyczące użytkownika. Podczas publikowania powiadomienia wyświetlana jest ikona jego *obszaru powiadomień*, jak pokazano na poniższym zrzucie ekranu:

![Przykład obszaru powiadomień na urządzeniu](local-notifications-images/01-notification-shade.png)

Aby uzyskać szczegółowe informacje o powiadomienie, użytkownik może otwierać menu powiadomień, (który rozwija każdego ikonę powiadomienia, aby wyświetlić zawartość powiadomień) i wykonywać żadnych akcji skojarzonych z powiadomienia. Poniższy ekran zrzut pokazuje *menu powiadomień* odnosi się do obszaru powiadomień wyświetlane powyżej:

[![Przykład menu powiadomień, wyświetlania trzy powiadomień](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Powiadomień systemu android należy użyć dwóch typów układy:

-   ***Układ podstawowy*** &ndash; format compact, stały prezentacji.

-   ***Układ rozwiniętej*** &ndash; format prezentacji, który będzie można rozszerzać na większy rozmiar, aby wyświetlić więcej informacji.

Każdy z tych typów układu (i jak je tworzyć) zostało wyjaśnione w poniższych sekcjach.

> [!NOTE]
> Ten przewodnik koncentruje się na [interfejsów API NotificationCompat](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) z [biblioteki obsługi systemu Android](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Te interfejsy API zapewni maksymalnie wstecznej zgodności dla systemu Android 4.0 (poziom interfejsu API 14).


### <a name="base-layout"></a>Układ podstawowy

Wszystkie powiadomienia dla systemu Android są oparte na format podstawowy układ, który co najmniej obejmuje następujące elementy:

1.  A *ikonę powiadomienia*, która reprezentuje źródłowy aplikacji lub typ powiadomienia, jeśli aplikacja obsługuje różne rodzaje powiadomienia.

2.  Powiadomienie *tytuł*, lub nazwa nadawcy, jeśli powiadomienie jest osobistą wiadomość.

3.  Komunikat powiadomienia.

4.  A *sygnatura czasowa*.

Te elementy są wyświetlane, jak pokazano na poniższym diagramie:

[![Lokalizacja elementów powiadomień](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

Układy podstawowa są ograniczone do 64 piksele niezależne od gęstość (dp) w wysokości. Domyślnie system android tworzy ten styl podstawowe powiadomienie.

Opcjonalnie powiadomienia można wyświetlić duża ikona reprezentuje zdjęcie nadawcy lub aplikacji. Duża ikona jest stosowana w powiadomienia w systemie Android 5.0 i nowszych wersjach, ikona małych powiadomienia zostanie wyświetlona jako wskaźnik myszy na ikonie na dużą:

![Zdjęcie proste powiadomienia](local-notifications-images/04-simple-notification-photo.png)

Począwszy od systemu Android 5.0, powiadomienia można również są wyświetlane na ekranie blokady:

[![Powiadomienia ekranu blokady przykład](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

Użytkownik może dwukrotnego powiadomienia ekranu blokady do odblokowywania urządzenia i przejdź do aplikacji, która pochodzi powiadomienie, lub Przesuniecie palcem, aby odrzucić powiadomienie. Aplikacje można ustawić poziom widoczności powiadomienie, aby kontrolować, co jest wyświetlany na ekranie blokady, a użytkownicy mogą wybrać, czy umożliwiające poufnej zawartości ma być wyświetlany w powiadomienia ekranu blokady.

Android 5.0 wprowadzono formatu prezentacji powiadomienia o wysokim priorytecie, o nazwie *hud*. Hud powiadomienia Przesuń w dół od górnej krawędzi ekranu na kilka sekund, a następnie retreat wykonywać kopie zapasowe w obszarze powiadomień:

[![Przykład heads-up powiadomień](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

Powiadomienia hud umożliwiają interfejsu użytkownika, aby umieścić ważne informacje w sposób widoczny dla użytkownika bez zakłócania pracy stan aktualnie uruchomione działanie systemu.

Android zawiera obsługę metadanych powiadomień, powiadomienia można sortować i wyświetlać inteligentnie. Metadane powiadomień kontroluje również, jak powiadomienia są prezentowane na ekranie blokady, jak i w formacie hud. Aplikacje można ustawić następujące typy metadanych powiadomień:

-   **Priorytet** &ndash; poziom priorytetu Określa, jak i kiedy powiadomienia są prezentowane. Na przykład w systemie Android 5.0, powiadomienia o wysokim priorytecie są wyświetlane jako hud powiadomienia.

-   **Widoczność** &ndash; Określa, ile zawartość powiadomień jest wyświetlany, gdy powiadomienie jest wyświetlane na ekranie blokady.

-   **Kategoria** &ndash; informuje system, jak obsługiwać powiadomienia w różnych okolicznościach, na przykład gdy urządzenie jest w *nie przeszkadzać* trybu.

**Uwaga:** **widoczność** i **kategorii** zostały wprowadzone w systemie Android 5.0 i są niedostępne we wcześniejszych wersjach systemu android. Począwszy od systemu Android 8.0 [kanały powiadomień](#notif-chan) są używane do kontrolowania, jak powiadomienia są prezentowane użytkownikowi.


### <a name="expanded-layouts"></a>Rozwinięty układów

Począwszy od systemu Android 4.1, powiadomienia można skonfigurować przy użyciu stylów rozwiniętej układu, które umożliwiają użytkownikom rozwinąć wysokość powiadomieniu, aby wyświetlić więcej zawartości. Na przykład poniższy przykład ilustruje powiadomienie rozszerzonej układu w trybie Umowie:

![Umowie powiadomień](local-notifications-images/07-contracted-notification.png)

Gdy to powiadomienie jest rozwinięty, co spowoduje wyświetlenie cały komunikat:

![Powiadomienie rozszerzonej](local-notifications-images/08-expanded-notification.png)

System android obsługuje trzy style rozwiniętej układu dla pojedynczego zdarzenia powiadomień:

-   ***Tekst big*** &ndash; w trybie umowie, wyświetla się fragment pierwszego wiersza komunikatu, następuje dwóch kropek. W trybie rozwiniętym Wyświetla cały komunikat (jak w powyższym przykładzie).

-   ***Skrzynka odbiorcza*** &ndash; w trybie umowie Wyświetla liczbę nowych wiadomości. W trybie rozwiniętym Wyświetla pierwszy wiadomości e-mail lub listę wiadomości w skrzynce odbiorczej.

-   ***Obraz*** &ndash; w trybie umowie wyświetla tylko tekst komunikatu. W trybie rozwiniętym Wyświetla tekst i obraz.

[Poza podstawowe powiadomienie](#beyond-the-basic-notification) (w dalszej części tego artykułu) opisano sposób tworzenia *tekst Big*, *skrzynki odbiorczej*, i *obraz* powiadomienia.

<a name="notif-chan"></a>
<a name="notification-channels"></a>
## <a name="notification-channels"></a>Kanały powiadomień

Począwszy od systemu Android 8.0 (Oreo), można użyć *kanały powiadomień* funkcję, aby utworzyć użytkownika można dostosować kanał dla każdego typu powiadomienia, które mają być wyświetlane. Kanały powiadomień umożliwiać dla Ciebie powiadomień grupy tak, aby wszystkie powiadomienia opublikowane w usłudze załączniku kanału takie samo zachowanie. Na przykład Niewykluczone, że kanał powiadomień, która jest przeznaczona dla powiadomień, które wymagają natychmiastowej uwagi i oddzielny kanał "zapewniająca" używaną dla komunikatów informacyjnych.

**YouTube** aplikacji, który został zainstalowany przy użyciu systemu Android Oreo zawiera dwie kategorie powiadomień: **pobieranie powiadomienia** i **ogólne powiadomienia**:

[![Ekrany powiadomień usługi YouTube w systemie Android Oreo](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

Każdy z tych kategorii odpowiada kanału powiadomień. Implementuje aplikację serwisu YouTube **pobieranie powiadomienia** kanału i **ogólne powiadomienia** kanału. Użytkownik może nacisnąć **pobieranie powiadomienia**, powoduje wyświetlenie na ekranie ustawień dla aplikacji Pobierz kanału powiadomień:

[![Pobierz ekranie powiadomień dla aplikacji w usłudze YouTube](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

Na tym ekranie, użytkownik może zmodyfikować zachowanie **Pobierz** powiadomień kanału, wykonując następujące czynności:

-   Ustaw poziom ważności **pilne**, **wysokiej**, **średni**, lub **niski**, który konfiguruje poziom przerwania dźwięku i visual.

-   Kropka powiadomień należy włączyć lub wyłączyć.

-   Migające światło należy włączyć lub wyłączyć.

-   Pokaż lub Ukryj powiadomienia na ekranie blokady.

-   Zastąp **nie przeszkadzać** ustawienie.

**Ogólne powiadomienia** kanał ma podobne ustawienia:

[![Ogólne powiadomienia ekranu dla aplikacji w usłudze YouTube](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

Zwróć uwagę, nie masz Pełna kontrola nad sposób kanałów powiadomień interakcji z użytkownikiem &ndash; użytkownik może modyfikować ustawienia dla dowolnego kanału powiadomień na urządzeniu, jak pokazano na zrzutach ekranu powyżej. Można jednak skonfigurować wartości domyślne, (jak zostaną opisane poniżej). Ponieważ te przykłady ilustrują, nowa funkcja kanały powiadomień umożliwia można umożliwić użytkownikom szczegółową kontrolę nad tym różnego rodzaju powiadomienia.


## <a name="notification-creation"></a>Tworzenie powiadomień

Aby utworzyć powiadomienie w systemie Android, należy użyć [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder) klasy z [Xamarin.Android.Support.v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) pakietu NuGet. Ta klasa pozwala na tworzenie i publikowanie powiadomień w starszych wersjach systemu Android. Aby uzyskać więcej informacji o korzystaniu z `NotificationCompat.Builder`, zobacz [zgodności](#compatibility) w dalszej części tego tematu.

`NotificationCompat.Builder` udostępnia metody do ustawiania różne opcje w powiadomieniu, takich jak:

-   Zawartość, w tym tytuł, tekst komunikatu i ikonę powiadomienia.

-   Styl powiadomień, takie jak *tekst Big*, *skrzynki odbiorczej*, lub *obraz* stylu.

-   Priorytet powiadomienia: minimum, niski, domyślnie wysoki lub maksymalną. W systemie Android 8.0 lub nowszym, priorytet jest ustawiany za pośrednictwem [ _kanału powiadomień_](#notification-channels).

-   Widoczność powiadomień na ekranie blokady: public, private lub klucza tajnego.

-   Metadane kategorii, które pomaga Android klasyfikować i filtrować powiadomienia.

-   Opcjonalne cel, który wskazuje działanie do uruchomienia po dotknięcie powiadomienia.

-   Identyfikator kanału powiadomień, powiadomienie zostanie opublikowany na (system Android 8.0 lub nowszy).

Po ustawieniu tych opcji w konstruktorze, musisz wygenerować obiekt powiadomień, który zawiera ustawienia. Publikowanie powiadomienia, polega na przekazaniu tego obiektu powiadomienia w celu *Menedżera powiadomień*. System android oferuje [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/) klasy, która jest odpowiedzialna za publikowanie powiadomień i wyświetlania ich do użytkownika. Odwołanie do tej klasy można uzyskać z dowolnego kontekstu, takich jak działania lub usługi.


### <a name="creating-a-notification-channel"></a>Tworzenie kanału powiadomień

Aplikacje, które są uruchomione na system Android 8.0, należy utworzyć kanał powiadomień dla ich powiadomień. Kanału powiadomień wymaga trzy następujące informacje:

* Ciąg Identyfikatora, który jest unikatowy dla pakietu, która będzie identyfikowała kanału.
* Nazwa kanału, który będzie widoczny dla użytkownika.  Nazwa musi być od jednej do 40 znaków.
* Ważność kanału.

Aplikacje będą wymagały sprawdzić wersję systemu Android, które są uruchomione.
Urządzenia z systemem w wersjach starszych niż system Android 8.0 nie powinien utworzyć kanał powiadomień. Poniższa metoda przedstawiono przykładowy sposób tworzenia kanału powiadomień w działaniu:

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

    var channelName = Resources.GetString(Resource.String.channel_name);
    var channelDescription = GetString(Resource.String.channel_description);
    var channel = new NotificationChannel(CHANNEL_ID, channelName, NotificationImportance.Default)
                  {
                      Description = channelDescription
                  };

    var notificationManager = (NotificationManager) GetSystemService(NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Kanał powiadomień należy utworzyć na każdym razem, gdy działanie jest tworzony. Dla `CreateNotificationChannel` metody powinna być wywoływana `OnCreate` sposób działania.

### <a name="creating-and-publishing-a-notification"></a>Tworzenie i publikowanie powiadomienie

Aby wygenerować powiadomienie w systemie Android, wykonaj następujące kroki:

1.  Utwórz wystąpienie `NotificationCompat.Builder` obiektu.

2.  Wywołaj różnych metod na `NotificationCompat.Builder` obiektu, aby ustawić opcje powiadamiania.

3.  Wywołaj [kompilacji](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/) metody `NotificationCompat.Builder` obiektu do utworzenia wystąpienia obiektu powiadamiania.

4.  Wywołaj [powiadamiania](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification)) metoda Menedżera powiadomień do opublikowania powiadomienia.

Należy podać co najmniej następujące informacje dotyczące każdego powiadomienia:

-   Mała ikona (24 x 24 dp rozmiar)

-   Krótki tytuł

-   Tekst powiadomienia

Poniższy przykład kodu ilustruje sposób użycia `NotificationCompat.Builder` do generowania podstawowe powiadomienie. Należy zauważyć, że `NotificationCompat.Builder` metody obsługują [tworzeniach łańcucha metody](http://en.wikipedia.org/wiki/Method_chaining); oznacza to, każda metoda zwraca obiekt konstruktora, aby można było używać wynik ostatniego wywołania metody do wywołania następnego wywołania metody:

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

W tym przykładzie nowy `NotificationCompat.Builder` obiektu o nazwie `builder` zostanie uruchomiony, oraz identyfikator kanału powiadomień ma być używany. Tytuł i treść powiadomienia są ustawiane i ikonę powiadomienia są ładowane z **Resources/drawable/ic_notification.png**. Wywołanie konstruktora powiadomień `Build` metoda tworzy obiekt powiadomień przy użyciu tych ustawień. Następnym krokiem jest wywołanie `Notify` metoda Menedżera powiadomień. Aby zlokalizować program notification manager, należy wywołać `GetSystemService`, jak pokazano powyżej.

`Notify` Metoda przyjmuje dwa parametry: identyfikator powiadomień oraz obiektu powiadamiania. Identyfikator powiadomienia jest unikatowa liczba całkowita, która identyfikuje powiadomienie do aplikacji. W tym przykładzie identyfikator powiadomień jest ustawiona na wartość zero (0); Jednak w przypadku aplikacji produkcyjnej należy podać unikatowy identyfikator każdego powiadomienia. Ponowne użycie poprzednich wartość identyfikatora w wywołaniu `Notify` powoduje, że ostatniego powiadomienia zostaną zastąpione.

Po uruchomieniu tego kodu na urządzeniu z systemem Android 5.0 generuje powiadomienie, który wygląda następująco:

![Wynik powiadomienia dla przykładowego kodu](local-notifications-images/09-hello-world.png)

W lewym dolnym rogu powiadomienia wyświetlana jest ikona powiadomień &ndash; ten obraz kółku &ldquo;i&rdquo; ma kanał alfa, więc Android można narysować szarego tła cykliczne związanych z nim. Możesz również dostarczyć ikony bez kanału alfa. Aby wyświetlić fotograficzne obrazu w postaci ikony, zobacz [duże ikony Format](#large-icon-format) w dalszej części tego tematu.

Sygnatura czasowa jest ustawiana automatycznie, ale można zastąpić to ustawienie, wywołując [SetWhen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/) metody konstruktora powiadomień. Na przykład poniższy kod ustawia bieżący czas sygnaturę czasową:

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>Włączanie dźwięk i wibracje

Jeśli chcesz, aby Twoje powiadomienie, aby również odtwarzać dźwięk, można wywołać konstruktora powiadomień [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/) metody i przekaż `NotificationDefaults.Sound` flagi:

```csharp
// Instantiate the notification builder and enable sound:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

To wywołanie `SetDefaults` spowoduje, że urządzenie odtworzyć dźwięk, gdy zostanie opublikowany powiadomienia. Jeśli chcesz, aby urządzenie wibracje zamiast odtwarzać dźwięk, można przekazać `NotificationDefaults.Vibrate` do `SetDefaults.` Jeśli chcesz, aby urządzenie odtwarzać dźwięk i wibracje urządzenia, możesz przekazać obie flagi do `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

Jeśli włączysz dźwięku bez określania odtwarzanie dźwięku, system Android używa domyślny dźwięk powiadomień systemu. Jednak można zmienić dźwięk, który będzie odtwarzany przez wywołanie konstruktora powiadomień [SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/) metody. Na przykład do odtwarzania alarmu dźwięku z powiadomienia (zamiast domyślny dźwięk powiadomienie), możesz też uzyskać identyfikator URI alarmu dźwięku z [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/) i przekazać ją do `SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

Alternatywnie można użyć dzwonek domyślne systemu dźwiękowego powiadomienia:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

Po utworzeniu obiektu powiadamiania, jest możliwość ustawienia właściwości powiadomień w obiekcie powiadomień (zamiast konfigurować je z wyprzedzeniem za pomocą `NotificationCompat.Builder` metody). Na przykład, zamiast wywoływać metodę `SetDefaults` metodę umożliwiającą włączenie wibracje na powiadomienie, można bezpośrednio modyfikować Flaga bitowa powiadomienia [domyślnie](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/) właściwości:

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

W tym przykładzie powoduje urządzenia wibracje po opublikowaniu powiadomienia.

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>Aktualizowanie powiadomienie

Jeśli chcesz zaktualizować zawartość powiadomienia po jej opublikowaniu, można ponownie użyć istniejącego `NotificationCompat.Builder` obiekt, aby utworzyć nowy obiekt powiadomień i opublikuj to powiadomienie o identyfikatorze ostatniego powiadomienia. Na przykład:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

W tym przykładzie istniejące `NotificationCompat.Builder` obiekt jest używany do utworzenia nowego obiektu powiadomienia za pomocą różnych tytuł i komunikat.
Nowy obiekt powiadomień jest opublikowana przy użyciu identyfikatora poprzednie powiadomienie i poprawka ta aktualizuje zawartość wcześniej publikowane powiadomień:

![Zaktualizowano powiadomienia](local-notifications-images/12-updated-notification.png)

Treść poprzednie powiadomienie zostanie ponownie użyty &ndash; tylko tytuł i tekst powiadomienie ulega zmianie, gdy zostanie wyświetlone powiadomienie w menu powiadomień. Tekst tytułu zmienia się z "Przykładowe powiadomienie" na "Zaktualizowano powiadomienie" i tekst komunikatu zmieni się z "Hello World! To jest Mój pierwszy powiadomienie!" Aby "Zmieniono na tę wiadomość."

Powiadomienie pozostaje widoczne, dopóki jedna trzy rzeczy sytuacji:

-   Użytkownik odrzuci powiadomienia (lub naciśnie *Wyczyść wszystko*).

-   Aplikacja nawiązuje połączenie `NotificationManager.Cancel`, przekazując identyfikator unikatowy powiadomień, która została przypisana w momencie powiadomienia została opublikowana.

-   Wywołania aplikacji `NotificationManager.CancelAll`.

Aby uzyskać więcej informacji o aktualizowaniu powiadomień systemu Android, zobacz [zmodyfikować powiadomienie](http://developer.android.com/training/notify-user/managing.html#Updating).


### <a name="starting-an-activity-from-a-notification"></a>Uruchamianie działania dla powiadomienia

W systemie Android, są często powiadomienie do skojarzenia z *akcji* &ndash; działanie, które jest uruchamiany po naciśnięciu powiadomienia przez użytkownika. To działanie może znajdować się w innej aplikacji lub nawet w przypadku innego zadania. Aby dodać akcję z powiadomieniem, należy utworzyć [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/) obiektu i skojarz `PendingIntent` powiadomień wysyłanych. A `PendingIntent` to specjalny typ cel, który umożliwia odbiorcy aplikacji do uruchomienia wstępnie zdefiniowanego fragment kodu z uprawnieniami aplikacji wysyłającej. Po naciśnięciu powiadomienia Android uruchamiania działania określonego przez `PendingIntent`.

Poniższy fragment kodu ilustruje sposób tworzenia powiadomienie z `PendingIntent` który spowoduje uruchomienie działania aplikacji źródłowy `MainActivity`:

```csharp
// Set up an intent so that tapping the notifications returns to this app:
Intent intent = new Intent (this, typeof(MainActivity));

// Create a PendingIntent; we're only using one PendingIntent (ID = 0):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    PendingIntent.GetActivity (this, pendingIntentId, intent, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including pending intent:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

Ten kod jest bardzo podobny do kodu powiadomień w poprzedniej sekcji, chyba że `PendingIntent` jest dodawany do obiektu powiadamiania. W tym przykładzie `PendingIntent` jest skojarzony z działania źródłowego aplikacji, zanim zostanie on przekazany do konstruktora powiadomień [SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/) metody. `PendingIntentFlags.OneShot` Flaga jest przekazywany do `PendingIntent.GetActivity` metody, aby `PendingIntent` jest używana tylko raz. Po uruchomieniu tego kodu, zostanie wyświetlone następujące powiadomienie:

![Powiadomienie o pierwszej akcji](local-notifications-images/10-first-action-notification.png)

Naciśnięcie tego powiadomienia, zajmuje się użytkownika do źródłowego działania.

W aplikacji produkcyjnej, aplikacja musi obsługiwać *stosu wstecz* gdy użytkownik naciśnie **ponownie** znajdującego się w działania powiadomień (Jeśli nie jesteś zaznajomiony z zadań dla systemu Android i zaplecze stosu, zobacz [ Zadania i stosu wstecz](http://developer.android.com/guide/components/tasks-and-back-stack.html)).
W większości przypadków Przejdź wstecz działania powiadomień poza powinien zwrócić użytkownika z aplikacji i z powrotem do ekranu głównego. Do zarządzania wstecz stosu, Twoja aplikacja używa [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/) klasy w celu utworzenia `PendingIntent` z tyłu stosu.

Inną ważną kwestią w rzeczywistych warunkach jest źródłowego działania może być konieczne do wysyłania danych do działania powiadomień. Na przykład powiadomienie może wskazywać, że dostarczeniu wiadomości SMS, a działanie powiadomienia (wiadomość wyświetlanie ekranu), wymaga identyfikator komunikatu, aby wyświetlić komunikat dla użytkownika. Działanie, które tworzy `PendingIntent` służy [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/) metody w celu dodania do intencji danych (na przykład, ciąg), tak, aby te dane są przekazywane do działania powiadomień.

Poniższy przykład kodu ilustruje sposób używania `TaskStackBuilder` do zarządzania stosu wstecz, a także przykładowy sposób wysyłania ciąg pojedynczy komunikat dla działania powiadomień o nazwie `SecondActivity`:

```csharp
// Setup an intent for SecondActivity:
Intent secondIntent = new Intent (this, typeof(SecondActivity));

// Pass some information to SecondActivity:
secondIntent.PutExtra ("message", "Greetings from MainActivity!");

// Create a task stack builder to manage the back stack:
TaskStackBuilder stackBuilder = TaskStackBuilder.Create(this);

// Add all parents of SecondActivity to the stack:
stackBuilder.AddParentStack (Java.Lang.Class.FromType (typeof (SecondActivity)));

// Push the intent that starts SecondActivity onto the stack:
stackBuilder.AddNextIntent (secondIntent);

// Obtain the PendingIntent for launching the task constructed by
// stackbuilder. The pending intent can be used only once (one shot):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    stackBuilder.GetPendingIntent (pendingIntentId, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including
// the pending intent:
NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my second action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

W tym przykładzie kodu aplikacji składa się z dwóch działań: `MainActivity` (który zawiera kod powiadomienia powyżej), a `SecondActivity`, ekran, użytkownik zobaczy następujący po naciśnięciu powiadomienia. Po uruchomieniu tego kodu jest widoczne powiadomienie proste (podobny do poprzedniego przykładu). Naciskając powiadomienia powoduje otwarcie `SecondActivity` ekranu:

![Drugie działanie zrzut ekranu](local-notifications-images/11-second-activity.png)

Ciąg komunikatu (przekazany do intencji `PutExtra` metody) jest pobierana w `SecondActivity` za pośrednictwem ten wiersz kodu:

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

Zostanie wyświetlony ten komunikat pobrane, "Greetings z MainActivity!," `SecondActivity` ekranu, jak pokazano na powyższym zrzucie ekranu. Gdy użytkownik naciśnie **ponownie** przycisk podczas `SecondActivity`, nawigacji potencjalnych klientów z aplikacji i z powrotem do ekranu poprzedzających uruchomienia aplikacji.

Aby uzyskać więcej informacji na temat tworzenia oczekujące intencje, zobacz [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/).


<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>Poza podstawowe powiadomienie

Domyślnie powiadomienia prostego układu podstawowego formatu w systemie Android, ale ten podstawowy format można zwiększyć, wprowadzając dodatkowe `NotificationCompat.Builder` wywołania metody. W tej sekcji dowiesz się, jak dodać ikonę dużych zdjęć do usługi powiadomień, a zobaczysz przykłady sposobu tworzenia układu rozwiniętej powiadomienia.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>Format duże ikony

Powiadomień systemu android zazwyczaj wyświetlić ikonę aplikacji źródłowy (po lewej stronie powiadomienia). Jednak można wyświetlić powiadomienia, obrazu lub zdjęcie ( *duża ikona*) zamiast standardowego mała ikona. Na przykład aplikację do obsługi wiadomości można wyświetlić zdjęcie nadawcy, a nie na ikonie aplikacji.

Oto przykład podstawowe powiadomienia w systemie Android 5.0 &ndash; Wyświetla ikonę niewielką aplikację:

![Przykład powiadomienia normalnego](local-notifications-images/13-sample-notification.png)

A Oto zrzut ekranu przedstawiający powiadomienie po zmodyfikowaniu go, aby wyświetlić duża ikona &ndash; używa ikona utworzonego na podstawie obrazu małp kodu środowiska Xamarin:

![Przykład duża ikona powiadomień](local-notifications-images/14-large-icon-sample.png)

Należy zauważyć, że powiadomienia są prezentowane w formacie duże ikony, niewielką aplikację ikona jest wyświetlana jako wskaźnik w prawym dolnym rogu duża ikona.

Aby użyć obrazu jako duża ikona w powiadomieniu, należy wywołać konstruktora powiadomień [SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/) metody i przekazać mapy bitowej obrazu. W odróżnieniu od `SetSmallIcon`, `SetLargeIcon` akceptuje tylko mapy bitowej. Aby przekonwertować plik obrazu mapy bitowej, należy użyć [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/) klasy. Na przykład:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

Ten przykładowy kod zostanie otwarty plik obrazu w **Resources/drawable/monkey_icon.png**, a następnie konwertuje ją na mapę bitową i przekazuje wynikowy mapy bitowej do `NotificationCompat.Builder`. Zazwyczaj rozdzielczość obrazu źródłowego jest większy niż mała ikona &ndash; , ale nie jest znacznie większa. Obraz, który jest zbyt duży, może spowodować niepotrzebne operacje zmiany rozmiaru, które może opóźnić ogłaszania powiadomienia.


### <a name="big-text-style"></a>Styl tekstu big Data

*Tekst Big* styl jest szablon układu rozwinięty, używanej do wyświetlania długie wiadomości w powiadomieniach. Podobnie jak wszystkie rozwinięte układ powiadomienia początkowo wyświetlane jest powiadomienie tekst big Data w formacie zwarty:

![Przykład duży tekst powiadomienia](local-notifications-images/15-big-text-notification.png)

W tym formacie tylko fragment komunikat jest wyświetlany, został przerwany przez dwie kropki. Gdy użytkownik przeciągnie w dół w powiadomieniu, go rozszerzany, aby wyświetlić całą powiadomienie:

![Rozwinięty powiadomienie tekstowe big Data](local-notifications-images/16-big-text-expanded.png)

Ten format rozwiniętej układ zawiera także tekstu podsumowania w dolnej części powiadomienia. Maksymalna wysokość *tekst Big* powiadomień to 256 punktu dystrybucji.

Do utworzenia *tekst Big* powiadomień, możesz utworzyć wystąpienie `NotificationCompat.Builder` obiektu, tak jak poprzednio, a następnie utwórz wystąpienie i Dodaj [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/) obiekt `NotificationCompat.Builder` obiektu. Oto przykład:

```csharp
// Instantiate the Big Text style:
Notification.BigTextStyle textStyle = new Notification.BigTextStyle();

// Fill it with text:
string longTextMessage = "I went up one pair of stairs.";
longTextMessage += " / Just like me. ";
//...
textStyle.BigText (longTextMessage);

// Set the summary text:
textStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (textStyle);

// Create the notification and publish it ...
```

W tym przykładzie tekst komunikatu i tekst podsumowania są przechowywane w `BigTextStyle` obiektu (`textStyle`) zanim zostanie przekazany do `NotificationCompat.Builder.`


### <a name="image-style"></a>Styl obrazu

*Obraz* stylu (nazywane również *w szerszej perspektywie* styl) to format powiadomień rozwinięty, który służy do wyświetlania obrazu w treści powiadomienia. Na przykład, można użyć aplikacji zrzut ekranu lub aplikacji photo *obraz* powiadomień stylu, aby zapewnić użytkownika powiadomienie z informacją o ostatniej obrazu, który został przechwycony. Należy pamiętać, że maksymalną wysokość *obraz* powiadomień to 256 dp &ndash; systemu Android będzie Zmień rozmiar obrazu, który pasuje do to ograniczenie maksymalnej wysokości w granicach dostępnej pamięci.

Podobnie jak wszystkie rozwinięte powiadomienia układ *obraz* powiadomienia najpierw są wyświetlane w kompaktowego formatu, który wyświetla się fragment towarzyszący tekst komunikatu:

![Obraz Compact powiadomień nie wyświetla obrazu](local-notifications-images/17-image-compact.png)

Gdy użytkownik przeciągnie w dół *obraz* powiadomienia, jego rozszerzany, aby wyświetlić obraz. Na przykład w tym miejscu jest rozszerzona wersja poprzednie powiadomienie:

![Obraz prezentuje powiadomień rozwiniętej obrazu](local-notifications-images/18-image-expanded.png)

Zwróć uwagę, że powiadomienia są wyświetlane w krótkiej formie, wyświetla tekst powiadomienia (tekst, który jest przekazywany do konstruktora powiadomień `SetContentText` metody, jak pokazano wcześniej). Jednakże powiadomienie zostanie rozszerzona, aby wyświetlić obraz, wyświetla tekst podsumowania powyżej obrazu.

Do utworzenia *obraz* powiadomień, możesz utworzyć wystąpienie `NotificationCompat.Builder` obiektu tak jak poprzednio, a następnie utwórz i Wstaw [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/) do obiektu `NotificationCompat.Builder` obiektu. Na przykład:

```csharp
// Instantiate the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Convert the image to a bitmap before passing it into the style:
picStyle.BigPicture (BitmapFactory.DecodeResource (Resources, Resource.Drawable.x_bldg));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create the notification and publish it ...
```

Podobnie jak `SetLargeIcon` metody `NotificationCompat.Builder`, [BigPicture](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/) metody `BigPictureStyle` wymaga mapę bitową obrazu, który ma być wyświetlany w treści powiadomienia. W tym przykładzie [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32)) metody `BitmapFactory` odczytuje plik obrazu, który znajduje się w **Resources/drawable/x_bldg.png** i konwertuje je do mapy bitowej.

Można także wyświetlać obrazy, które nie są dostarczane jako zasób. Na przykład następujący przykładowy kod ładuje obraz z lokalnego karta SD i wyświetla go w *obraz* powiadomień:

```csharp
// Using the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Read an image from the SD card, subsample to half size:
BitmapFactory.Options options = new BitmapFactory.Options();
options.InSampleSize = 2;
string imagePath = "/sdcard/Pictures/my-tshirt.jpg";
picStyle.BigPicture (BitmapFactory.DecodeFile (imagePath, options));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("Check out my new T-Shirt!");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create notification and publish it ...
```

W tym przykładzie plik obrazu znajdującym się w **/sdcard/Pictures/my-tshirt.jpg** jest ładowany, rozmiar połowę oryginalnego rozmiaru i następnie konwertowany na mapę bitową do użycia w powiadomienia:

![Przykładowy obraz koszulki powiadomienia](local-notifications-images/19-tshirt-notification.png)

Jeśli rozmiar pliku obrazu nie jest znana z wyprzedzeniem, to dobry pomysł, aby opakować wywołanie [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/) w obsłudze wyjątków &ndash; `OutOfMemoryError` może zostać wygenerowany wyjątek, jeśli obraz jest za duży dla Android, aby zmienić rozmiar.

Więcej informacji na temat ładowania i dekodowaniu obrazów duże mapy bitowe, zobacz [obciążenia duże mapy bitowe efektywnie](https://github.com/xamarin/recipes/tree/master/Recipes/android/resources/general/load_large_bitmaps_efficiently).


### <a name="inbox-style"></a>Styl skrzynki odbiorczej

*Skrzynki odbiorczej* format to szablon układu rozwiniętej przeznaczone do wyświetlania osobnych wierszach tekstu (na przykład adres e-mail skrzynki odbiorczej podsumowania) w treści powiadomienia. *Skrzynki odbiorczej* w krótkiej formie najpierw zostanie wyświetlone powiadomienie formatu:

![Przykład skrzynki odbiorczej compact powiadomień](local-notifications-images/20-inbox-compact.png)

Gdy użytkownik przeciągnie w dół w powiadomieniu, jego rozszerzany, aby wyświetlić podsumowanie poczty e-mail, jak pokazano na poniższym zrzucie ekranu:

![Przykład skrzynki odbiorczej powiadomień rozszerzone](local-notifications-images/21-inbox-expanded.png)

Aby utworzyć *skrzynki odbiorczej* powiadomień, możesz utworzyć wystąpienie `NotificationCompat.Builder` obiektu, tak jak poprzednio i Dodaj [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/) do obiektu `NotificationCompat.Builder`. Oto przykład:

```csharp
// Instantiate the Inbox style:
Notification.InboxStyle inboxStyle = new Notification.InboxStyle();

// Set the title and text of the notification:
builder.SetContentTitle ("5 new messages");
builder.SetContentText ("chimchim@xamarin.com");

// Generate a message summary for the body of the notification:
inboxStyle.AddLine ("Cheeta: Bananas on sale");
inboxStyle.AddLine ("George: Curious about your blog post");
inboxStyle.AddLine ("Nikko: Need a ride to Evolve?");
inboxStyle.SetSummaryText ("+2 more");

// Plug this style into the builder:
builder.SetStyle (inboxStyle);
```

Aby dodać nowe wiersze tekstu na treść powiadomienia, należy wywołać [Addline](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/) metody `InboxStyle` obiektu (maksymalną wysokość *skrzynki odbiorczej* powiadomień to 256 dp). Należy zauważyć, że w przeciwieństwie do *tekst Big* stylu *skrzynki odbiorczej* styl obsługuje pojedyncze wiersze tekstu w treści powiadomienia.

Można również użyć *skrzynki odbiorczej* stylu na potrzeby wszystkich powiadomień, który wymaganych, aby wyświetlić pojedyncze wiersze tekstu w formacie rozwinięty. Na przykład *skrzynki odbiorczej* styl powiadomienie może służyć do łączenia wielu oczekujące powiadomienia do podsumowania powiadomień &ndash; można zaktualizować jeden *skrzynki odbiorczej* powiadomienia o nowych stylów Wiersze zawartości powiadomienia (zobacz [aktualizowanie powiadomienie](#updating-a-notification) powyżej), a nie Generuj ciągłego strumienia nowy, przede wszystkim podobny powiadomienia.


## <a name="configuring-metadata"></a>Konfigurowanie metadanych

`NotificationCompat.Builder` zawiera metody, które można wywołać, aby ustawić metadane dotyczące Twojej powiadomienia, taki jak priorytet, widoczność i kategorii. System android używa tych informacji &mdash; wraz z ustawień preferencji użytkownika &mdash; ustalenie, jak i kiedy należy wyświetlać powiadomienia.


### <a name="priority-settings"></a>Ustawienia priorytetu

Aplikacje działające w systemie Android 7.1 i niższy muszą ustawić priorytet bezpośrednio w samej powiadomienia. Po opublikowaniu powiadomienia, ustawienie priorytetu powiadomienia określa dwie sytuacje:

-   Gdzie powiadomienie jest wyświetlane w odniesieniu do innych powiadomień.
    Na przykład o wysokim priorytecie powiadomienia są prezentowane powyżej niższe powiadomienia o priorytecie w szufladzie powiadomienia bez względu na to w przypadku każdego powiadomienia została opublikowana.

-   Czy powiadomienie jest wyświetlana w formacie Heads-up powiadomień (Android 5.0 lub nowszy). Tylko *wysokiej* i *maksymalna* priorytet powiadomienia są wyświetlane jako hud powiadomienia.

Xamarin.Android definiuje następujące wyliczenia do ustawiania priorytetu powiadomień:

-   `NotificationPriority.Max` &ndash; Ostrzega o tym użytkownika, aby warunek pilne lub krytyczny (na przykład, wywołanie przychodzące, kierunki Włącz, wyłącz lub alert o awarii). W systemie Android 5.0 lub nowszym maksymalny priorytet powiadomienia są wyświetlane w formacie hud.

-   `NotificationPriority.High` &ndash; Informuje użytkownika o ważnych zdarzeniach, (na przykład ważnych wiadomości e-mail lub nadejście wiadomości rozmowy w czasie rzeczywistym). W systemie Android 5.0 lub nowszym powiadomienia o wysokim priorytecie są wyświetlane w formacie hud.

-   `NotificationPriority.Default` &ndash; Powiadamia użytkownika warunków, które muszą średni poziom ważności.

-   `NotificationPriority.Low` &ndash; Pilne informacji, że użytkownik musi być świadome (na przykład, przypomnienia dotyczące aktualizacji oprogramowania lub aktualizacje sieci społecznościowych).

-   `NotificationPriority.Min` &ndash; Aby uzyskać ogólne informacje, że użytkownik powiadomienia tylko wtedy, gdy wyświetlania powiadomień (na przykład, lokalizacji lub pogody informacji).

Aby ustawić priorytet powiadomienia, należy wywołać [SetPriority](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/) metody `NotificationCompat.Builder` obiektu, przekazując poziom priorytetu. Na przykład:

```csharp
builder.SetPriority (NotificationPriority.High);
```

W poniższym przykładzie, powiadomienia o wysokim priorytecie, "Ważną wiadomość!" pojawia się w górnej części menu powiadomień:

![Powiadomienie o wysokim priorytecie przykład](local-notifications-images/22-hi-priority-drawer.png)

Ponieważ to jest powiadomienie o wysokim priorytecie, jest ona również wyświetlana jako powiadomienie hud powyżej bieżącego ekranu aktywności użytkownika w systemie Android 5.0:

![Przykład hud powiadomień](local-notifications-images/23-heads-up-example.png)

W następnym przykładzie "Traktować w ciągu dnia" powiadomienia o niskim priorytecie zostanie wyświetlona w obszarze powiadomień do poziomu baterii o wyższym priorytecie:

![Powiadomienia o niskim priorytecie przykład](local-notifications-images/24-lo-priority-drawer.png)

Ponieważ powiadomienie "Myślenia w ciągu dnia" powiadomienia o niskim priorytecie, systemu Android nie będą wyświetlane go w formacie Heads-up.

> [!NOTE]
> W systemie Android 8.0 lub nowszym priorytet ustawień powiadomień kanału i użytkownik określi priorytet zawiadomienia.

### <a name="visibility-settings"></a>Ustawienia widoczności

Począwszy od systemu Android 5.0 *widoczność* ustawienie jest dostępne do kontrolowania ilości zawartości powiadomień, który pojawia się na bezpiecznej blokady ekranu.
Xamarin.Android definiuje następujące wyliczenia do ustawiania widoczność powiadomień:

-   `NotificationVisibility.Public` &ndash; Pełna zawartość powiadomienie jest wyświetlane na bezpiecznej blokady ekranu.

-   `NotificationVisibility.Private` &ndash; Tylko niezbędne informacje są wyświetlane na bezpiecznym ekranu blokady (na przykład ikony powiadomień i nazwę aplikacji, który opublikował go), ale pozostałe szczegóły powiadomienia są ukryte. Domyślnie wszystkie powiadomienia `NotificationVisibility.Private`.

-   `NotificationVisibility.Secret` &ndash; Nic nie jest wyświetlane na ekranie blokady bezpieczny, nawet ikonę powiadomienia. Zawartość powiadomień jest dostępna tylko wtedy, gdy ten użytkownik odblokuje urządzenia.

Aby ustawić widoczność powiadomienie wywołanie aplikacji `SetVisibility` metody `NotificationCompat.Builder` obiektu, przekazując ustawienie widoczności. Na przykład, to wywołanie `SetVisibility` sprawia, że powiadomienia `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

Gdy `Private` powiadomienie zostanie opublikowana, tylko nazwa i ikona aplikacji jest wyświetlany na ekranie bezpiecznej blokady. Zamiast komunikatu powiadomienia użytkownik zobaczy "Odblokuj urządzenie będzie to powiadomienie":

![Odblokowanie komunikatu powiadomienia urządzenia](local-notifications-images/25-lockscreen-private.png)

W tym przykładzie **NotificationsLab** nazywa się źródłowy aplikacji. Ta wersja redagowane powiadomienia zostanie wyświetlona tylko wtedy, gdy ekranu blokady jest bezpieczne (czyli zabezpieczone za pomocą numeru PIN, wzorca lub hasło) &ndash; Jeśli ekranu blokady nie jest bezpieczne, pełna zawartość powiadomienia jest dostępna na ekranie blokady.


### <a name="category-settings"></a>Ustawienia kategorii

Począwszy od systemu Android 5.0, wstępnie zdefiniowane kategorie są dostępne dla klasyfikacji i filtrowania powiadomień. Platforma Xamarin.Android zawiera następujące wyliczenia dla tych kategorii:

-   `Notification.CategoryCall` &ndash; Przychodzące połączenia telefonicznego.

-   `Notification.CategoryMessage` &ndash; Przychodzące wiadomości SMS.

-   `Notification.CategoryAlarm` &ndash; Alarm warunku lub czasomierza wygaśnięcia.

-   `Notification.CategoryEmail` &ndash; Przychodzące wiadomości e-mail.

-   `Notification.CategoryEvent` &ndash; Zdarzenie w kalendarzu.

-   `Notification.CategoryPromo` &ndash; Promocyjne wiadomości lub anonsu.

-   `Notification.CategoryProgress` &ndash; Postęp operacji w tle.

-   `Notification.CategorySocial` &ndash; Społecznościowych aktualizacja sieci.

-   `Notification.CategoryError` &ndash; Błąd operacji lub uwierzytelniania proces w tle.

-   `Notification.CategoryTransport` &ndash; Aktualizacja odtwarzania multimediów.

-   `Notification.CategorySystem` &ndash; Zarezerwowany do użycia przez system (stan systemu lub urządzenia).

-   `Notification.CategoryService` &ndash; Wskazuje, czy działa usługa tła.

-   `Notification.CategoryRecommendation` &ndash; Komunikat zalecenia związane z aktualnie uruchomionej aplikacji.

-   `Notification.CategoryStatus` &ndash; Informacje o urządzeniu.

Gdy powiadomienia są sortowane, powiadomienia o priorytecie mają pierwszeństwo przed jego ustawienie kategorii. Na przykład powiadomienia o wysokim priorytecie będą wyświetlane jako hud nawet wtedy, gdy należy ona do `Promo` kategorii. Aby ustawić kategorii powiadomienia, należy wywołać `SetCategory` metody `NotificationCompat.Builder` obiektu, przekazując ustawienie kategorii. Na przykład:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

*Nie przeszkadzać* funkcji (Nowa funkcja w systemie Android 5.0) filtruje powiadomienia na podstawie kategorii. Na przykład *nie przeszkadzać* ekranu w **ustawienia** zezwala użytkownikowi na wykluczenie powiadomienia dla połączeń telefonicznych i wiadomości:

![Nie przeszkadzać przełączników ekranu](local-notifications-images/26-do-not-disturb.png)

Jeśli użytkownik skonfiguruje *nie przeszkadzać* Aby zablokować wszystkie przerwań, z wyjątkiem połączeń telefonicznych (jak pokazano na powyższym zrzucie ekranu), systemów Android umożliwia powiadomień za pomocą ustawienia kategorii `Notification.CategoryCall` mają zostać wyświetlone, a urządzenie Trwa *nie przeszkadzać* trybu. Należy pamiętać, że `Notification.CategoryAlarm` powiadomienia nigdy nie są blokowane w *nie przeszkadzać* trybu.

[LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications) przykład pokazuje, jak używać `NotificationCompat.Builder` można uruchomić drugiego działania dla powiadomienia. Ten przykładowy kod zostało wyjaśnione w [przy użyciu lokalnego powiadomienia w Xamarin.Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) wskazówki.


### <a name="notification-styles"></a>Style powiadomień

Aby utworzyć *tekst Big*, *obrazu*, lub *skrzynki odbiorczej* stylu powiadomień za pomocą `NotificationCompat.Builder`, aplikacji należy użyć wersji zgodności tych stylów. Na przykład, aby użyć *tekst Big* stylu, Utwórz wystąpienie `NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

Podobnie, Twoja aplikacja może używać `NotificationCompat.InboxStyle` i `NotificationCompat.BigPictureStyle` dla *skrzynki odbiorczej* i *obraz* style, odpowiednio.


### <a name="notification-priority-and-category"></a>Powiadomienie, priorytetu i kategorii

`NotificationCompat.Builder` obsługuje `SetPriority` — metoda (dostępne począwszy od systemu Android 4.1). Jednak `SetCategory` metodą jest *nie* obsługiwane przez `NotificationCompat.Builder` ponieważ kategorie są częścią nowego systemu metadanych powiadomień, która została wprowadzona w systemie Android 5.0.

Do obsługi starszych wersji systemu Android, gdzie `SetCategory` jest niedostępne, kod może sprawdzać, poziom interfejsu API w czasie wykonywania, aby warunkowo wywołać `SetCategory` , gdy poziom interfejsu API jest równa lub większa niż 5.0 dla systemu Android (poziom interfejsu API 21):

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= BuildVersionCodes.Lollipop) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

W tym przykładzie aplikacja firmy **platformę docelową** jest ustawiona na Android 5.0 i **minimalna wersja systemu Android** ustawiono **Android 4.1 (16 poziom interfejsu API)**. Ponieważ `SetCategory` jest dostępna w poziom interfejsu API 21 i nowsze, wywoła ten przykładowy kod `SetCategory` tylko gdy jest dostępna &ndash; nie wywoła `SetCategory` gdy poziom interfejsu API jest mniej niż 21.


### <a name="lockscreen-visibility"></a>Widoczność ekranu blokady

Ponieważ system Android nie obsługuje powiadomień ekranu blokady przed Android 5.0 (poziom 21 interfejsu API), `NotificationCompat.Builder` nie obsługuje `SetVisibility` metody. Zgodnie z powyższymi wskazówkami dla `SetCategory`, kod może sprawdzać, poziom interfejsu API środowiska uruchomieniowego i wywołania `SetVisiblity` tylko gdy jest dostępna:

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetVisibility (Notification.Public);
}
```


## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono sposób tworzenia lokalnego powiadomienia w systemie Android. On opisany anatomia powiadomienia, jego wyjaśniono, jak używać `NotificationCompat.Builder` do utworzenia powiadomień, jak styl powiadomienia w duża ikona *tekst Big*, *obraz* i *skrzynki odbiorczej*  formaty, jak ustawić powiadomienie ustawienia metadane, takie jak priorytet, widoczność i kategorii i jak można uruchomić działania z powiadomienia. W tym artykule opisano, jak działają te ustawienia powiadomień o nowych hud, blokady ekranu, a *nie przeszkadzać* funkcje wprowadzone w systemie Android 5.0. Ponadto przedstawiono sposób użycia `NotificationCompat.Builder` utrzymać zgodność powiadomień z wcześniejszych wersji systemu android.

Aby uzyskać wskazówki dotyczące projektowania powiadomienia dla systemu Android, zobacz [powiadomienia](http://developer.android.com/guide/topics/ui/notifiers/notifications.html).


## <a name="related-links"></a>Linki pokrewne

- [NotificationsLab (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (przykład)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Powiadomienia lokalne w przewodniku dla systemu Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [Powiadomienia użytkownika](http://developer.android.com/training/notify-user/index.html)
- [Powiadomienia](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
