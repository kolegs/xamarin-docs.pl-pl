---
title: Lokalnego powiadomienia
description: "W tej sekcji przedstawiono sposób wykonania lokalnego powiadomienia o platformie Xamarin.Android. Opisano różne elementy interfejsu użytkownika powiadomień systemu Android i w tym artykule omówiono interfejsu API użytkownika związane z tworzenie i wyświetlanie powiadomienie."
ms.topic: article
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: f13515326bd75f2b2c15e2b6059e6f829814ea5c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="local-notifications"></a>Lokalnego powiadomienia

_W tej sekcji przedstawiono sposób wykonania lokalnego powiadomienia o platformie Xamarin.Android. Opisano różne elementy interfejsu użytkownika powiadomień systemu Android i w tym artykule omówiono interfejsu API użytkownika związane z tworzenie i wyświetlanie powiadomienie._

## <a name="local-notifications-overview"></a>Omówienie lokalnego powiadomienia

W tym temacie wyjaśniono, jak wdrożyć lokalnego powiadomienia w aplikacji platformy Xamarin.Android. Go w tym artykule omówiono różne części powiadomień systemu Android, wyjaśniono style różnych powiadomień, które są dostępne dla deweloperów aplikacji i wprowadza ona niektórych interfejsów API, które są używane do tworzenia i publikowania powiadomienia.

Android udostępnia dwa obszary pod kontrolą systemu wyświetlania ikony powiadomień oraz powiadomień informacji do użytkownika. Powiadomienie o pierwszej publikacji, jego ikona jest wyświetlana w *obszaru powiadomień*, jak pokazano na poniższym zrzucie ekranu:

![Przykład obszaru powiadomień na urządzeniu](local-notifications-images/01-notification-shade.png)

Aby uzyskać szczegółowe informacje o powiadomienia, użytkownik może otwierać menu powiadomień (który rozszerza każdego ikonę powiadomienia, aby wyświetlić zawartość powiadomień) i wykonywać żadnych akcji skojarzonych z powiadomień. Następujący ekran zrzut pokazuje *menu powiadomień* odpowiadający obszaru powiadomień wyświetlane powyżej:

[![Przykład menu powiadomień wyświetlania trzy powiadomień](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Powiadomień systemu android użyć dwóch typów układów:

-   ***Układu podstawowego*** &ndash; formacie kompaktowym, stały prezentacji.

-   ***Układ rozwinięte*** &ndash; format prezentacji można rozszerzyć na większy rozmiar, aby wyświetlić więcej informacji.

Każdy z tych typów układu (i sposób ich tworzenia) znajduje się w poniższych sekcjach.


### <a name="base-layout"></a>Układu podstawowego

Wszystkie powiadomienia Android są wbudowane w formacie układu podstawowego, której co najmniej obejmuje następujące elementy:

1.  A *ikonę powiadomienia*, jeśli aplikacja obsługuje różne rodzaje powiadomienia reprezentuje źródłowy aplikacji lub typ powiadomienia.

2.  Powiadomienia *tytuł*, lub nazwą nadawcy, jeśli powiadomienie jest osobistą wiadomość.

3.  Komunikat powiadomienia.

4.  A *sygnatury czasowej*.

Te elementy są wyświetlane w sposób przedstawiony na poniższym diagramie:

[![Lokalizacja elementów powiadomień](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

Układy podstawowej są ograniczone do 64 pikselach niezależnych od gęstości (dp) w wysokości. Android domyślnie tworzy ten styl powiadomień w wersji basic.

Opcjonalnie powiadomienia można wyświetlić duże ikony, która reprezentuje nadawcy zdjęcie lub aplikacji. W przypadku dużych ikon powiadomienia w systemie Android 5.0 i nowszych wersjach jako wskaźnika jest wyświetlana ikona małych powiadomień za pośrednictwem dużych ikon:

![Zdjęcie prostego powiadomienia](local-notifications-images/04-simple-notification-photo.png)

Począwszy od systemu Android 5.0 powiadomień może również zostać wyświetlony na ekranu blokady:

[![Przykład powiadomień ekranu blokady](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

Użytkownik może dwukrotnego powiadomienia ekranu blokady, aby odblokować urządzenie i przejść do aplikacji, która pochodzi z tego powiadomienia, lub przejdź na odrzucenie powiadomienia. Aplikacje można ustawić poziom widoczności powiadomienie, aby kontrolować, co przedstawiono na ekranu blokady, a użytkownicy mogą wybrać, czy zezwalać poufnej zawartości ma być wyświetlany w powiadomień ekranu blokady.

Android 5.0 wprowadzono format prezentacji powiadomienia o wysokim priorytecie, nazywany *projekcyjny wskazuje pozycję*. Powiadomienia projekcyjny wskazuje pozycję slajd w dół od górnej krawędzi ekranu przez kilka sekund i następnie retreat tworzenie kopii zapasowych w obszarze powiadomień:

[![Przykład heads-up powiadomień](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

Powiadomienia projekcyjny wskazuje pozycję umożliwiają interfejsu użytkownika mają zostać umieszczone ważne informacje przed użytkownika bez wpływu na stan aktualnie uruchomione działanie systemu.

Android obsługuje powiadomienia metadanych, dzięki czemu powiadomienia można sortować i wyświetlać inteligentnie. Metadane powiadomienia kontroluje również, jak powiadomienia są prezentowane w ekranu blokady i format projekcyjny wskazuje pozycję. Aplikacje można ustawić następujące typy metadanych powiadomień:

-   **Priorytet** &ndash; priorytet Określa, jak i kiedy są prezentowane powiadomienia. Na przykład w systemie Android 5.0, powiadomienia o wysokim priorytecie są wyświetlane jako projekcyjny wskazuje pozycję powiadomienia.

-   **Widoczność** &ndash; Określa, ile zawartość powiadomień jest wyświetlany, kiedy powiadomienie zostanie wyświetlone na ekranu blokady.

-   **Kategoria** &ndash; informuje system sposób obsługi powiadomień w różnych okolicznościach, np. gdy urządzenie jest w *nie przeszkadzać* tryb.

**Uwaga:** **widoczność** i **kategorii** zostały wprowadzone w systemie Android 5.0 i są niedostępne w starszych wersjach systemu android. Począwszy od systemu Android 8.0 [kanały powiadomień](#notif-chan) służą do określania, jak są prezentowane powiadomień dla użytkownika.


### <a name="expanded-layouts"></a>Układy rozszerzonej

Począwszy od systemu Android 4.1, można skonfigurować powiadomienia za pomocą stylów układu rozwinięte, które umożliwiają użytkownikom rozwiń wysokość powiadomienie, aby wyświetlić więcej zawartości. Na przykład poniższy przykład przedstawia powiadomienie układu rozwinięty w trybie Umowie:

![Umowie powiadomień](local-notifications-images/07-contracted-notification.png)

Po rozwinięciu tego powiadomienia ujawnia cały komunikat:

![Rozwinięte powiadomień](local-notifications-images/08-expanded-notification.png)

Android obsługuje trzy stylów rozwinięte układu dla pojedynczego zdarzenia powiadomień:

-   ***Tekst big*** &ndash; w trybie umowie Wyświetla fragment pierwszego wiersza komunikatu następuje dwie kropki. W tryb rozszerzony Wyświetla cały komunikat (jak w powyższym przykładzie).

-   ***Skrzynka odbiorcza*** &ndash; w trybie umowie Wyświetla liczbę nowych komunikatów. W tryb rozszerzony Wyświetla pierwszy wiadomości e-mail lub listę komunikatów w skrzynce odbiorczej.

-   ***Obraz*** &ndash; w trybie umowie wyświetla tylko tekst komunikatu. W tryb rozszerzony Wyświetla tekst i obraz.

[Poza powiadomień w wersji Basic](#beyond-the-basic-notification) (dalszej części tego artykułu) wyjaśnia sposób tworzenia *tekst Big*, *skrzynki odbiorczej*, i *obrazu* powiadomienia.


## <a name="notification-creation"></a>Tworzenie powiadomień

Aby utworzyć powiadomienie w systemie Android, należy użyć [Notification.Builder](https://developer.xamarin.com/api/type/Android.App.Notification+Builder/) klasy. `Notification.Builder` został wprowadzony w Android 3.0, aby uprościć tworzenie obiektów powiadomień. Aby utworzyć powiadomienia, które są zgodne ze starszymi wersjami systemu android, można użyć [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) klasy zamiast `Notification.Builder` (zobacz [zgodności](#compatibility) dalszej części tego tematu więcej informacji o używaniu `NotificationCompat.Builder`).
`Notification.Builder` udostępnia metody do ustawiania różnych opcji w powiadomienia, takich jak:

-   Zawartość, w tym tytuł, tekst komunikatu i ikony powiadomień.

-   Styl powiadomień, takie jak *tekst Big*, *skrzynki odbiorczej*, lub *obrazu* stylu.

-   Priorytet powiadomienia: minimum, niski, domyślnie wysoki lub maksymalną.

-   Widoczność powiadomienia na ekranu blokady: public, private lub klucz tajny.

-   Metadane kategorii, które pomaga Android klasyfikowania i filtrować powiadomienia.

-   Opcjonalne zamiar, który wskazuje działanie, aby uruchomić powiadomienie jest wybrany.

Po skonfigurowaniu tych opcji w Konstruktorze generowania obiektu powiadomienia, który zawiera ustawienia. Do publikowania powiadomienia, przekaż ten obiekt powiadomień do *Menedżera powiadomień*. Udostępnia android [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/) klasy, która jest odpowiedzialna za publikowania powiadomienia i wyświetlanie ich do użytkownika. Odwołanie do tej klasy można uzyskać z dowolnego kontekstu, takie jak działania lub usługi.


### <a name="how-to-generate-a-notification"></a>Sposób generowania powiadomienia

Aby wygenerować powiadomienie w systemie Android, wykonaj następujące kroki:

1.  Utwórz wystąpienie `Notification.Builder` obiektu.

2.  Wywoływanie różnych metod na `Notification.Builder` obiekt, aby ustawić opcje powiadamiania.

3.  Wywołanie [kompilacji](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/) metody `Notification.Builder` obiektu do utworzenia wystąpienia obiektu powiadamiania.

4.  Wywołanie [powiadamiania](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification)) metody Menedżera powiadomień do publikowania powiadomienia.

Należy podać co najmniej następujące informacje dla każdego powiadomienia:

-   Mała ikona (24 x 24 dp rozmiar)

-   Krótka nazwa

-   Tekst powiadomienia.

Poniższy przykładowy kod przedstawia sposób użycia `Notification.Builder` do generowania powiadomień w wersji basic. Zwróć uwagę, że `Notification.Builder` metody obsługują [tworzeniach łańcucha metody](http://en.wikipedia.org/wiki/Method_chaining); oznacza to, każda metoda zwraca obiekt konstruktora dzięki wynik ostatniego wywołania metody można użyć do następnego wywołania metody invoke:

```csharp
// Instantiate the builder and set notification elements:
Notification.Builder builder = new Notification.Builder (this)
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

W tym przykładzie nową `Notification.Builder` obiektu o nazwie `builder` jest uruchomiony, tytuł i treść powiadomienia są ustawione oraz ikony powiadomień są ładowane z **Resources/drawable/ic_notification.png**. Wywołanie konstruktora powiadomień `Build` metoda tworzy obiekt powiadomienia przy użyciu tych ustawień. Następnym krokiem jest wywołanie `Notify` metody Menedżera powiadomień. Aby zlokalizować Menedżera powiadomień, należy wywołać `GetSystemService`, jak pokazano powyżej.

`Notify` Metoda przyjmuje dwa parametry: identyfikator powiadomień i obiekt powiadomień. Identyfikator powiadomień jest unikatowa liczba całkowita, która identyfikuje powiadomienia do aplikacji. W tym przykładzie identyfikator powiadomień jest ustawiony na zero (0); Jednak w aplikacji produkcyjnej, należy podać unikatowy identyfikator każdego powiadomienia. Ponowne użycie poprzednich wartości identyfikatora w wywołaniu `Notify` powoduje, że ostatniego powiadomienia zostaną zastąpione.

Po uruchomieniu tego kodu na urządzeniu z systemem Android 5.0 generuje powiadomienie, która wygląda jak w następującym przykładzie:

![Wynik powiadomienia przykładowy kod](local-notifications-images/09-hello-world.png)

Ikona powiadomienia jest wyświetlany na stronie po lewej stronie powiadomienia &ndash; ten obraz kółku &ldquo;i&rdquo; ma kanału alfa Android można narysować szare tło cykliczne za nią. Może też podawać ikona bez kanał alfa. Aby wyświetlić obraz fotograficzne jako ikona, zobacz [formatu ikona](#large-icon-format) dalszej części tego tematu.

Sygnatura czasowa jest ustawiany automatycznie, ale można zastąpić to ustawienie, wywołując [SetWhen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/) metody konstruktora powiadomień. Na przykład w poniższym przykładzie kodu ustawia sygnaturę czasową bieżący czas:

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>Włączenie dźwięku i wibrację

Jeśli chcesz, aby powiadomienia także odtwarzanie dźwięku, można wywołać konstruktora powiadomień [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/) — metoda i przekaż `NotificationDefaults.Sound` flagi:

```csharp
// Instantiate the notification builder and enable sound:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

To wywołanie `SetDefaults` spowoduje, że urządzenie odtwarzanie dźwięku po opublikowaniu powiadomienia. Jeśli chcesz, aby urządzenia Włącz wibrację zamiast odtwarzanie dźwięku, można przekazać `NotificationDefaults.Vibrate` do `SetDefaults.` Jeśli chcesz, aby urządzenia odtwarzanie dźwięku i Włącz wibrację urządzenia, można przekazać obu flag w celu `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

Po włączeniu dźwięku bez określania odtwarzanie dźwięku Android używa domyślny dźwięk powiadomienia systemu. Można jednak zmienić dźwięk, który będzie odtwarzana przez wywołanie konstruktora powiadomień [SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/) metody. Na przykład odtwarzać alarm dźwiękowy z powiadomienia (zamiast domyślny dźwięk powiadomienie), można znaleźć identyfikatora URI alarmu dźwięk z [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/) i przekaż go do `SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

Alternatywnie można użyć dzwonek domyślny system dźwięku powiadomienia:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

Po utworzeniu obiektu powiadomień, jest możliwość ustawienia właściwości powiadomienia w obiekcie powiadomienia (zamiast je skonfigurować wcześniej za pomocą `Notification.Builder` metody). Na przykład zamiast wywoływać metodę `SetDefaults` metodę umożliwiającą włączenie wibrację na powiadomienie, możesz bezpośrednio modyfikować Flaga bitowa powiadomienia [domyślne](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/) właściwości:

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

W tym przykładzie powoduje, że urządzenia Włącz wibrację po opublikowaniu powiadomienia.

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>Powiadomienie o aktualizacji

Jeśli chcesz zaktualizować zawartość powiadomienie po opublikowaniu, można wykorzystać istniejące `Notification.Builder` obiekt, aby utworzyć nowy obiekt powiadomień i publikowanie tego powiadomienia o identyfikatorze ostatniego powiadomienia. Na przykład:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

W tym przykładzie istniejące `Notification.Builder` obiekt jest używany do utworzenia nowego obiektu powiadomienia o różnych tytuł i wiadomości.
Nowy obiekt powiadomień jest publikowana przy użyciu identyfikatora poprzedniej powiadomień i spowoduje to zaktualizowanie zawartość wcześniej publikowane powiadomień:

![Zaktualizowano powiadomień](local-notifications-images/12-updated-notification.png)

Treść poprzedniej powiadomień zostanie ponownie użyty &ndash; tylko tytuł i tekst powiadomienie ulega zmianie, gdy zostanie wyświetlony w szufladzie powiadomień. Tekst tytułu zmiany z "Przykładowe powiadomienie" na "Powiadomienie o aktualizacji" i tekst komunikatu zmiany z "Witaj świecie! Jest to Mój pierwszego powiadomienia!" Aby "Zmienione na tę wiadomość."

Powiadomienie o pozostaje widoczna, dopóki jedna z trzech zdarzeń sytuacji:

-   Użytkownik odrzuci powiadomienie (lub naciska *Wyczyść wszystko*).

-   Aplikacja nawiązuje połączenie `NotificationManager.Cancel`, przekazując identyfikator unikatowy powiadomień, która została przypisana w momencie opublikowania powiadomienia.

-   Wywołania aplikacji `NotificationManager.CancelAll`.

Aby uzyskać więcej informacji na temat aktualizowania powiadomień systemu Android, zobacz [zmodyfikować powiadomienie](http://developer.android.com/training/notify-user/managing.html#Updating).


### <a name="starting-an-activity-from-a-notification"></a>Uruchamianie działania dla powiadomienia

W systemie Android jest typowe dla powiadomienie ma być skojarzone z *akcji* &ndash; działania, który jest uruchamiany po naciśnięciu powiadomienia. To działanie może znajdować się w innej aplikacji lub nawet inne zadanie. Aby dodać akcję do powiadomienia, należy utworzyć [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/) obiektu i skojarzyć `PendingIntent` powiadomienia. A `PendingIntent` to specjalny typ założeń, umożliwiająca odbiorcy aplikacji do uruchomienia wstępnie zdefiniowanego fragment kodu z uprawnieniami aplikację wysyłającą. Po naciśnięciu powiadomienia Android uruchamiania działania określony przez `PendingIntent`.

Poniższy fragment kodu ilustruje sposób tworzenia powiadomienie dotyczące `PendingIntent` który uruchomi działania źródłowego aplikacji `MainActivity`:

```csharp
// Set up an intent so that tapping the notifications returns to this app:
Intent intent = new Intent (this, typeof(MainActivity));

// Create a PendingIntent; we're only using one PendingIntent (ID = 0):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    PendingIntent.GetActivity (this, pendingIntentId, intent, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including pending intent:
Notification.Builder builder = new Notification.Builder(this)
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

Ten kod jest bardzo podobny do kodu powiadomienia w poprzedniej sekcji, z wyjątkiem `PendingIntent` jest dodawane do obiektu powiadomień. W tym przykładzie `PendingIntent` jest skojarzony z działania źródłowego aplikacji, zanim zostanie przekazany do konstruktora powiadomień [SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/) metody. `PendingIntentFlags.OneShot` Flagi są przekazywane do `PendingIntent.GetActivity` metody, aby `PendingIntent` jest używana tylko raz. Po uruchomieniu tego kodu, zostanie wyświetlone następujące powiadomienie:

![Pierwszy działań powiadomień](local-notifications-images/10-first-action-notification.png)

Naciśnięcie tego powiadomienia przejście użytkownika do źródłowego działania.

W aplikacji produkcyjnej, aplikacja musi obsługiwać *stosie przechodzenia wstecz* gdy użytkownik naciśnie **ponownie** przycisk w ramach działania powiadomień (Jeśli nie masz doświadczenia z systemem Android zadania i stosie przechodzenia wstecz, zobacz [ Zadania i stosie przechodzenia wstecz](http://developer.android.com/guide/components/tasks-and-back-stack.html)).
W większości przypadków nawigowania wstecz działania powiadomień poza powinien zwrócić użytkownika poza aplikacją i z powrotem do głównej ekranu. Aby zarządzać stosie przechodzenia wstecz, używany przez aplikację [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/) klasy w celu utworzenia `PendingIntent` o stosie przechodzenia wstecz.

Kolejnym zagadnieniem rzeczywistych jest źródłowego działania może być konieczne wysyłać dane do działania powiadomień. Na przykład powiadomienia może wskazywać, że odebrano wiadomość SMS, i działania powiadomień (komunikat wyświetlania ekranu), wymaga Identyfikatora komunikatu do wyświetlania komunikatu dla użytkownika. Działanie, które tworzy `PendingIntent` można użyć [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/) — metoda (na przykład ciąg) do wskazywania zamiar tak, aby te dane są przekazywane do działania powiadomień.

Poniższy przykładowy kod przedstawia sposób użycia `TaskStackBuilder` do zarządzania stosie przechodzenia wstecz, a także przykładowy sposób wysyłania ciąg jednego komunikatu do działania powiadomień o nazwie `SecondActivity`:

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
Notification.Builder builder = new Notification.Builder (this)
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

W tym przykładzie kodu aplikacja składa się z dwóch działań: `MainActivity` (który zawiera kod powiadomienia powyżej), a `SecondActivity`, użytkownik zobaczy następujący po naciśnięcie powiadomienia ekranu. Po uruchomieniu tego kodu przedstawiono prosty powiadomienie (podobnie jak w poprzednim przykładzie). Naciskając pozycję powiadomienie ma użytkownikowi `SecondActivity` ekranu:

![Drugi działania zrzut ekranu](local-notifications-images/11-second-activity.png)

Ciąg komunikatu (przekazany zamiar `PutExtra` metoda) jest pobierana w `SecondActivity` za pośrednictwem tego wiersza kodu:

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

Ten komunikat pobrane "Pozdrowienia z MainActivity!," jest wyświetlany w `SecondActivity` ekranu, jak pokazano na powyższym zrzucie ekranu. Gdy użytkownik naciśnie **ponownie** przycisk podczas `SecondActivity`, nawigacji prowadzi poza aplikacją i z powrotem do ekranu poprzedzającym uruchamiania aplikacji.

Aby uzyskać więcej informacji o tworzeniu oczekujące intencje, zobacz [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/).


<a name="notif-chan" />

## <a name="notification-channels"></a>Kanały powiadomień

Począwszy od systemu Android 8.0 (Oreo), można użyć *kanały powiadomień* funkcji, aby utworzyć kanał użytkownika można dostosowywać dla poszczególnych typów powiadomień, który chcesz wyświetlić. Kanały powiadomień umożliwiają Ci powiadomienia grupy tak, aby wszystkie powiadomienia publikowanego zawiera kanału takie samo zachowanie. Na przykład może być kanał powiadomień, który jest przeznaczony dla powiadomień, które wymagają natychmiastowej uwagi i oddzielny kanał "zapewniająca", używany do komunikaty informacyjne.

**YouTube** aplikacji, który jest instalowany z systemem Android Oreo zawiera dwie kategorie powiadomień: **Pobierz powiadomienia** i **ogólne powiadomienia**:

[![Ekrany powiadomień dla YouTube w Oreo systemu Android](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

Każdy z tych kategorii odpowiada kanału powiadomień. Implementuje aplikacji YouTube **Pobierz powiadomienia** kanału i **ogólne powiadomienia** kanału. Użytkownik może nacisnąć **Pobierz powiadomienia**, które powoduje wyświetlenie ekranu ustawienia dla aplikacji Pobierz kanału powiadomień:

[![Pobierz powiadomienia ekranu aplikacji YouTube](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

Na tym ekranie użytkownik może zmodyfikować zachowanie **Pobierz** kanału powiadomień, wykonując następujące czynności:

-   Ustaw poziom znaczenie **pilne**, **wysokiej**, **średni**, lub **małej**, co pozwala skonfigurować poziom przerwania dźwięku i visual.

-   Kropka powiadomień należy włączyć lub wyłączyć.

-   Jasny migający należy włączyć lub wyłączyć.

-   Pokaż lub Ukryj powiadomienia na ekranie blokady.

-   Zastąpienie **nie przeszkadzać** ustawienie.

**Ogólne powiadomienia** kanału ma podobne ustawienia:

[![Ogólne powiadomienia ekranu aplikacji YouTube](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

Powiadomienie ma Pełna kontrola nad sposób kanałów powiadomień interakcji z użytkownikiem &ndash; użytkownik może zmodyfikować ustawienia dla dowolnego kanału powiadomień na urządzeniu, jak pokazano na zrzutach ekranu powyżej. Można jednak skonfigurować wartości domyślne, (zgodnie z poniższym opisem można będzie). Jak tych przykładach, nowa funkcja kanały powiadomień umożliwia można umożliwić użytkownikom precyzyjną kontrolę nad różnych rodzajów powiadomienia.

Należy dodać obsługę kanały powiadomień do aplikacji? Jeśli aplikacja jest przeznaczona dla 8.0 dla systemu Android, aplikację *musi* zaimplementować kanały powiadomień.
Aplikacji przeznaczony dla Oreo, który próbuje wysłać lokalne powiadomienie do użytkownika bez korzystania z kanału powiadomień zakończy się niepowodzeniem wyświetlić powiadomienie na urządzeniach Oreo. Nie w przypadku skierowania 8.0 dla systemu Android, aplikacji będą nadal działać na 8.0 dla systemu Android, ale z tej samej zachowanie powiadomienia jako będzie działać, podczas uruchamiania systemu Android 7.1 lub wcześniej.


### <a name="creating-a-notification-channel"></a>Tworzenie kanału powiadomień

Aby utworzyć kanał powiadomień, wykonaj następujące czynności:

1. Utworzyć [NotificationChannel](https://developer.android.com/reference/android/app/NotificationChannel.html) obiektu z następujących czynności:

    - Ciąg Identyfikatora, który jest unikatowy w ramach pakietu. W poniższym przykładzie ciąg `com.xamarin.myapp.urgent` jest używany.

    - Widoczny dla użytkownika nazwę kanału (mniej niż 40 znaków). W poniższym przykładzie nazwa **pilne** jest używany.

    - Znaczenie kanału, który kontroluje sposób interruptive powiadomienia są wysyłane do `NotificationChannel`. Może mieć znaczenie `Default`, `High`, `Low`, `Max`, `Min`, `None`, lub `Unspecified`.

    Przekaż te wartości do [Konstruktor](https://developer.android.com/reference/android/app/NotificationChannel.html#NotificationChannel%28java.lang.String,%20java.lang.CharSequence,%20int%29) (w tym przykładzie `Resource.String.noti_chan_urgent` ustawiono **pilne**):

    ```csharp
    public const string URGENT_CHANNEL = "com.xamarin.myapp.urgent";
    . . .
    string chanName = GetString (Resource.String.noti_chan_urgent);
    var importance = NotificationImportance.High;
    NotificationChannel chan =
       new NotificationChannel (URGENT_CHANNEL, chanName, importance);
    ```

2.  Skonfiguruj `NotificationChannel` obiektu ustawień wstępnych.
    Na przykład następujący kod konfiguruje `NotificationChannel` obiektów, dzięki czemu przesłane do tego kanału powiadomień zostanie Włącz wibrację urządzenia i domyślnie wyświetlane na ekranie blokady:

    ```csharp
    chan.EnableVibration (true);
    chan.LockscreenVisibility = NotificationVisibility.Public;
    ```

3.  Wyślij powiadomienie obiekt kanał do Menedżera powiadomień:

    ```csharp
    NotificationManager notificationManager =
        (NotificationManager) GetSystemService (NotificationService);
    notificationManager.CreateNotificationChannel (chan);
    ```


### <a name="posting-to-a-notifications-channel"></a>Zamieszczając kanał powiadomień

Można wysłać powiadomienia kanał powiadomień, wykonaj następujące czynności:

1.  Skonfigurować za pomocą powiadomień `Notification.Builder`, przekazując identyfikator kanału do `SetChannelId` metody. Na przykład:

    ```csharp
    Notification.Builder builder = new Notification.Builder (this)
        .SetContentTitle ("Attention!")
        .SetContentText ("This is an urgent notification message!")
        .SetChannelId (URGENT_CHANNEL);
    ```

2.  Utworzyć i wydać powiadomień przy użyciu Menedżera powiadomień [powiadamiania](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/p/System.Int32/Android.App.Notification/) metody:

    ```csharp
    const int notificationId = 0;
    notificationManager.Notify (notificationId, builder.Build());
    ```

Możesz powtarzać powyższe kroki, aby utworzyć inny kanał powiadomień dla komunikaty informacyjne. Ten drugi kanał można domyślnie wyłączyć wibrację, ustawioną domyślną widoczność ekranu blokady `Private`i Ustaw ważność powiadomień `Default`.

Aby uzyskać pełny przykład kodu kanałów powiadomień systemu Android Oreo w akcji, zobacz [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) przykładowa; tej aplikacji przykładowej zarządza dwa kanały i ustawia opcje dodatkowe powiadomień.



<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>Poza powiadomień w wersji Basic

Powiadomienia domyślny format no-frills układu podstawowego w systemie Android, ale ten podstawowy format można zwiększyć, wprowadzając dodatkowe `Notification.Builder` wywołania metody. W tej sekcji dowiesz się, jak dodać ikony dużych zdjęcie do powiadomienia, a zobaczysz przykłady sposobu tworzenia układu rozwinięte powiadomienia.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>Format dużych ikon

Powiadomień systemu android zazwyczaj wyświetlana ikona źródłowy aplikacji (w lewej powiadomienia). Jednak powiadomienia można wyświetlać obrazów lub fotografii ( *dużych ikon*) zamiast standardowego małych ikon. Na przykład aplikacji obsługi wiadomości, można wyświetlić zdjęcie nadawcy, a nie jako ikonę aplikacji.

Poniżej przedstawiono przykład podstawowego powiadomień systemu Android 5.0 &ndash; Wyświetla ikonę małych aplikacji:

![Przykład normalne powiadomień](local-notifications-images/13-sample-notification.png)

A Oto zrzut ekranu powiadomienie po zmodyfikowaniu go, aby wyświetlić dużych ikon &ndash; używa ikony utworzony na podstawie obrazu małp kodu Xamarin:

![Przykład dużych ikon powiadomień](local-notifications-images/14-large-icon-sample.png)

Zwróć uwagę, że gdy powiadomienie będzie wyświetlana w formacie dużych ikon, ikona małych aplikacji jest wyświetlany jako wskaźnika w prawym dolnym rogu dużych ikon.

Aby użyć obrazu jako dużych ikon powiadomienia, należy wywołać konstruktora powiadomień [SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/) — metoda i przekazać obrazu mapy bitowej. W odróżnieniu od `SetSmallIcon`, `SetLargeIcon` akceptuje tylko mapy bitowej. Aby skonwertować plik obrazu mapy bitowej, należy użyć [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/) klasy. Na przykład:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

Ten przykładowy kod otwiera plik obrazu na **Resources/drawable/monkey_icon.png**, konwertuje ją na mapę bitową i przekazuje wynikowy mapy bitowej do `Notification.Builder`. Zazwyczaj rozdzielczość obrazu źródłowego jest większy niż małych ikon &ndash; , ale nie jest znacznie większa. Obraz, który jest zbyt duży, może spowodować niepotrzebnych operacji zmiany rozmiaru, które można opóźnić publikowanie powiadomienia.
Aby uzyskać więcej informacji na temat rozmiarów ikony powiadomień w systemie Android, zobacz [ikony powiadomień](http://developer.android.com/design/style/iconography.html#notification).


### <a name="big-text-style"></a>Styl tekstu duży

*Tekst Big* styl jest szablon układu rozwinięte, używanej do wyświetlania długich komunikatów powiadomień. Podobnie jak wszystkie rozwinięte układu powiadomienia początkowo wyświetlane jest powiadomienie duży tekst w formacie kompaktowym prezentacji:

![Przykładowy tekst Big powiadomień](local-notifications-images/15-big-text-notification.png)

W tym formacie tylko fragment komunikat jest wyświetlany, został przerwany przez dwie kropki. Gdy użytkownik przeciąga powiadomienie, rozszerza on ujawnić całego powiadomienie:

![Rozwinięte tekst Big powiadomień](local-notifications-images/16-big-text-expanded.png)

Ten format rozwinięte układu także tekstu podsumowania w dolnej części powiadomienia. Maksymalna wysokość *tekst Big* powiadomień to 256 punktu dystrybucji.

Aby utworzyć *tekst Big* powiadomień, można utworzyć wystąpienia `Notification.Builder` obiektu, jak wcześniej, a następnie utworzyć wystąpienia i dodać [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/) do obiektu `Notification.Builder` obiektu. Na przykład:

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

W tym przykładzie tekst komunikatu i tekst podsumowania są przechowywane w `BigTextStyle` obiektu (`textStyle`) zanim zostanie przekazany do `Notification.Builder.`


### <a name="image-style"></a>Styl obrazu

*Obrazu* styl (nazywane również *szerszej* styl) to format rozwinięte powiadomień, który służy do wyświetlania obrazu w treści powiadomienie. Na przykład można użyć aplikacji zrzut ekranu lub aplikacji zdjęcie *obrazu* przechwycenia obrazu stylu, aby przyznać użytkownikowi powiadomienie ostatniego powiadomienia. Należy pamiętać, że maksymalna wysokość *obrazu* powiadomień to 256 dp &ndash; Android zmieni się rozmiar obrazu do dopasowania do to ograniczenie maksymalnej wysokości w granicach dostępnej pamięci.

Podobnie jak wszystkie rozwinięte powiadomienia układu *obrazu* powiadomienia najpierw są wyświetlane w formacie kompaktowym, wyświetlająca fragment towarzyszący tekst komunikatu:

![Obraz Compact powiadomienia nie wyświetla obrazu](local-notifications-images/17-image-compact.png)

Gdy użytkownik przeciąga w dół *obrazu* powiadomienia rozszerza aby pokazać obrazu. Na przykład w tym miejscu jest rozszerzona wersja poprzedniego powiadomień:

![Obraz prezentuje powiadomień rozwinięte obrazu](local-notifications-images/18-image-expanded.png)

Powiadomienie, że jeśli powiadomienie jest wyświetlane w formacie kompaktowym, wyświetla tekst powiadomienia (tekst, który zostanie przekazany do konstruktora powiadomień `SetContentText` metody, jak pokazano wcześniej). Jednak jeśli powiadomienie jest rozwinięty, aby wyświetlić obraz, wyświetla tekst podsumowania powyżej obrazu.

Aby utworzyć *obrazu* powiadomień, można utworzyć wystąpienia `Notification.Builder` obiekt jak wcześniej, a następnie utwórz i Wstaw [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/) obiekt do `Notification.Builder` obiektu. Na przykład:

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

Podobnie jak `SetLargeIcon` metody `Notification.Builder`, [BigPicture](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/) metody `BigPictureStyle` wymaga mapę bitową obrazu, który ma być wyświetlany w treści powiadomienia. W tym przykładzie [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32)) metody `BitmapFactory` odczytów w lokalizacji pliku obrazu **Resources/drawable/x_bldg.png** i konwertuje ją do mapy bitowej.

Można również wyświetlać obrazów, które nie są dostarczane jako zasób. Na przykład następujący przykładowy kod ładuje obraz z lokalnym karty SD i wyświetla je w *obrazu* powiadomień:

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

W tym przykładzie plik obrazu znajdujący się w **/sdcard/Pictures/my-tshirt.jpg** jest załadowany, zmiany rozmiaru połowę oryginalnego rozmiaru i następnie konwertowana do mapy bitowej do użycia w powiadomieniu:

![Przykład obrazu obrazów na koszulki powiadomienia](local-notifications-images/19-tshirt-notification.png)

Jeśli nie znasz z wyprzedzeniem rozmiar pliku obrazu, jest dobrym pomysłem powodującą otoczenie wywołania [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/) w obsłudze wyjątków &ndash; `OutOfMemoryError` wyjątek może zostać zgłoszony, jeśli obraz jest za duża dla Android zmiany rozmiaru.

Aby uzyskać więcej informacji o ładowania i dekodowaniu obrazów dużej mapy bitowej, zobacz [obciążenia dużych map bitowych wydajnie](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently).


### <a name="inbox-style"></a>Styl skrzynki odbiorczej

*Skrzynki odbiorczej* format jest szablon układu rozwinięte przeznaczony do wyświetlania oddzielne wiersze tekstu (na przykład skrzynki odbiorczej poczty e-mail podsumowanie) w treści powiadomienia. *Skrzynki odbiorczej* w formacie kompaktowym najpierw zostanie wyświetlone powiadomienie format:

![Przykład skrzynki odbiorczej compact powiadomień](local-notifications-images/20-inbox-compact.png)

Gdy użytkownik przeciąga powiadomienie, rozszerza on wyświetlić podsumowanie poczty e-mail, jak pokazano na poniższym zrzucie ekranu:

![Przykład skrzynki odbiorczej powiadomień rozwinięty](local-notifications-images/21-inbox-expanded.png)

Aby utworzyć *skrzynki odbiorczej* powiadomień, można utworzyć wystąpienia `Notification.Builder` obiektów, jak wcześniej i Dodaj [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/) do obiektu `Notification.Builder`. Na przykład:

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

Aby dodać nowe wiersze tekstu treści powiadomienia, należy wywołać [Addline](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/) metody `InboxStyle` obiektu (maksymalną wysokość *skrzynki odbiorczej* powiadomień to 256 dp). Należy zauważyć, że w przeciwieństwie do *tekst Big* styl *skrzynki odbiorczej* styl obsługuje pojedyncze wiersze tekstu w treści powiadomień.

Można również użyć *skrzynki odbiorczej* stylu dla dowolnego powiadomienie, które mają być wyświetlone poszczególnych wierszy tekstu w formacie rozwinięte. Na przykład *skrzynki odbiorczej* styl powiadomienie może służyć do łączenia wielu oczekujące powiadomienia do podsumowania powiadomień &ndash; możesz zaktualizować pojedynczy *skrzynki odbiorczej* styl powiadomienia za pomocą instrukcji new Wiersze zawartości powiadomienia (zobacz [aktualizowanie powiadomienie](#updating-a-notification) powyżej), a nie niż generowanie stały strumień nowy, przede wszystkim podobny powiadomień. Aby uzyskać więcej informacji na temat tej metody, zobacz [Podsumuj powiadomienia](http://developer.android.com/design/patterns/notifications.html#summarize_your_notifications).


## <a name="configuring-metadata"></a>Konfigurowanie metadanych

`Notification.Builder` zawiera metody, które można wywołać, aby ustawić metadane dotyczące powiadomienia, takich jak priorytet, widoczność i kategorii. Te informacje są używane dla systemu android &mdash; wraz z ustawień preferencji użytkownika &mdash; ustalenie, jak i kiedy należy wyświetlać powiadomienia.


### <a name="priority-settings"></a>Ustawienia priorytetu

Po opublikowaniu powiadomienia, ustawienia priorytetu powiadomienia określa dwóch wyników:

-   Gdy powiadomienie jest wyświetlane w odniesieniu do innych powiadomień.
    Na przykład powiadomienia o wysokim priorytecie są przedstawione powyżej niższy priorytet powiadomienia w szufladzie powiadomienia bez względu na to podczas każdego powiadomienia został opublikowany.

-   Określa, czy powiadomienie jest wyświetlane w formacie Heads-up powiadomienia (Android 5.0 i nowszych). Tylko *wysokiej* i *maksymalna* priorytet powiadomienia są wyświetlane jako projekcyjny wskazuje pozycję powiadomienia.

Xamarin.Android definiuje następujące wyliczenia do ustawiania powiadomień o priorytecie:

-   `NotificationPriority.Max` &ndash; Powiadamia użytkownika o warunku pilnych lub krytyczne (na przykład połączenia przychodzącego, Włącz przez Włącz kierunki lub alert o awarii). W systemie Android 5.0 i urządzenia z nowszymi wersjami maksymalny priorytet powiadomienia są wyświetlane w formacie projekcyjny wskazuje pozycję.

-   `NotificationPriority.High` &ndash; Informuje użytkownika ważnych zdarzeń (np. ważne wiadomości e-mail lub nadejściu wiadomości rozmów w czasie rzeczywistym). W systemie Android 5.0 i urządzenia z nowszymi wersjami powiadomienia o wysokim priorytecie są wyświetlane w formacie projekcyjny wskazuje pozycję.

-   `NotificationPriority.Default` &ndash; Powiadamia użytkownika warunków, które muszą średni poziom ważności.

-   `NotificationPriority.Low` &ndash; Pilnych informacji, użytkownik musi być powiadamiane o (na przykład przypomnień aktualizacji oprogramowania lub aktualizacje sieci społecznościowych).

-   `NotificationPriority.Min` &ndash; Aby uzyskać ogólne informacje, że użytkownik powiadomienia tylko wtedy, gdy wyświetlanie powiadomień (na przykład, lokalizacji lub pogody informacje).

Aby ustawić priorytet powiadomienia, należy wywołać [SetPriority](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/) metody `Notification.Builder` obiektu, przekazując poziom priorytetu. Na przykład:

```csharp
builder.SetPriority (NotificationPriority.High);
```

W poniższym przykładzie, powiadomienia o wysokim priorytecie, "Ważną wiadomość!" pojawia się w górnej części menu powiadomień:

![Przykład powiadomienia o wysokim priorytecie](local-notifications-images/22-hi-priority-drawer.png)

Ponieważ jest to powiadomienie o wysokim priorytecie, jest on również wyświetlany jako powiadomienie projekcyjny wskazuje pozycję powyżej bieżącego ekranu działań użytkownika w systemie Android 5.0:

![Przykład projekcyjny wskazuje pozycję powiadomień](local-notifications-images/23-heads-up-example.png)

W następnym przykładzie powiadomień "Traktować dzień" niskiego priorytetu jest wyświetlana w obszarze powiadomienia poziomie baterii o wyższym priorytecie:

![Przykład powiadomienia o niskim priorytecie](local-notifications-images/24-lo-priority-drawer.png)

Ponieważ powiadomienie "Myśl dzień" jest powiadomienie o niskim priorytecie, nie Android zostanie wyświetlona w formacie Heads-up.


### <a name="visibility-settings"></a>Ustawienia widoczności

Począwszy od systemu Android 5.0 *widoczność* ustawienie jest dostępne do kontrolowania ilości zawartości powiadomień pojawia się w bezpiecznym ekranu blokady.
Xamarin.Android definiuje następujące wyliczenia do ustawiania widoczność powiadomień:

-   `NotificationVisibility.Public` &ndash; Pełna zawartość powiadomienia są wyświetlane na bezpiecznym ekranu blokady.

-   `NotificationVisibility.Private` &ndash; Tylko ważne informacje są wyświetlane na bezpiecznego ekranu blokady (na przykład ikony powiadomień i nazwę aplikacji, która ją opublikować), ale pozostałe szczegóły powiadomienia są ukryte. Domyślnie wszystkie powiadomienia `NotificationVisibility.Private`.

-   `NotificationVisibility.Secret` &ndash; Nie będą wyświetlane żadne na bezpiecznego ekranu blokady nawet ikonę powiadomienia. Zawartość powiadomień jest dostępna tylko wtedy, gdy użytkownik odblokowuje urządzenie.

Aby ustawić widoczność powiadomienia, aplikacje wywołania `SetVisibility` metody `Notification.Builder` przekazania w ustawieniu widoczności obiektu. Na przykład to wywołanie `SetVisibility` sprawia, że powiadomienia `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

Gdy `Private` jest przesyłana z powiadomień, tylko nazwy i ikony aplikacji jest wyświetlany na bezpiecznym ekranu blokady. Zamiast komunikatu powiadomienia użytkownik zobaczy "Odblokuj urządzenie wyświetlić tego powiadomienia":

![Odblokuj urządzenie komunikatu powiadomienia](local-notifications-images/25-lockscreen-private.png)

W tym przykładzie **NotificationsLab** to nazwa źródłowego aplikacji. Ta wersja zredagowanym powiadomienia jest wyświetlana tylko wtedy, gdy jest bezpieczne ekranu blokady (tj., zabezpieczone za pomocą numeru PIN, wzorzec lub hasło) &ndash; czy ekranu blokady nie jest bezpieczne, pełnej zawartości powiadomienie jest dostępna w ekranu blokady.


### <a name="category-settings"></a>Kategoria Ustawienia

Począwszy od systemu Android 5.0, wstępnie zdefiniowanych kategorii są dostępne dla klasyfikacji i filtrowanie powiadomienia. Xamarin.Android zapewnia następujące wyliczenia dla tych kategorii:

-   `Notification.CategoryCall` &ndash; Przychodzących połączeń telefonicznych.

-   `Notification.CategoryMessage` &ndash; Komunikat przychodzący tekstu.

-   `Notification.CategoryAlarm` &ndash; Alarm warunku lub czasomierza wygaśnięcia.

-   `Notification.CategoryEmail` &ndash; Przychodzących wiadomości e-mail.

-   `Notification.CategoryEvent` &ndash; Zdarzenia kalendarza.

-   `Notification.CategoryPromo` &ndash; Promocyjne wiadomości lub anonsu.

-   `Notification.CategoryProgress` &ndash; Postęp operacji w tle.

-   `Notification.CategorySocial` &ndash; Aktualizacja sieci społecznościowych.

-   `Notification.CategoryError` &ndash; Niepowodzenie operacji lub uwierzytelniania proces w tle.

-   `Notification.CategoryTransport` &ndash; Aktualizacja odtwarzania multimediów.

-   `Notification.CategorySystem` &ndash; Zarezerwowane do użycia przez system (stan systemu lub urządzenia).

-   `Notification.CategoryService` &ndash; Wskazuje, czy działa usługa tła.

-   `Notification.CategoryRecommendation` &ndash; Komunikat zalecenia dotyczące aktualnie uruchomionej aplikacji.

-   `Notification.CategoryStatus` &ndash; Informacje o urządzeniu.

Gdy sortowane są powiadomienia, powiadomienia priorytet mają pierwszeństwo przed jej ustawienie kategorii. Na przykład powiadomienia o wysokim priorytecie będą wyświetlane jako projekcyjny wskazuje pozycję nawet wtedy, gdy należy `Promo` kategorii. Aby ustawić kategorii powiadomienia, należy wywołać `SetCategory` metody `Notification.Builder` obiektu, przekazując ustawienie kategorii. Na przykład:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

*Nie przeszkadzać* funkcji (Nowa funkcja w systemie Android 5.0) filtruje powiadomienia na podstawie kategorii. Na przykład *nie przeszkadzać* ekranu w **ustawienia** zezwala użytkownikowi na powiadomienia zwolnienia połączenia telefoniczne i wiadomości:

![Nie zakłócają przełączniki ekranu](local-notifications-images/26-do-not-disturb.png)

Jeśli użytkownik skonfiguruje *nie przeszkadzać* Aby zablokować wszystkie przerwania z wyjątkiem połączeń telefonicznych (zgodnie z opisami na zrzucie ekranu powyżej), Android umożliwia powiadomienia z ustawieniem kategorii `Notification.CategoryCall` będą widoczne podczas urządzenia Trwa *nie przeszkadzać* tryb. Należy pamiętać, że `Notification.CategoryAlarm` powiadomienia nigdy nie są blokowane w *nie przeszkadzać* tryb.


<a name="compatibility" />

## <a name="compatibility"></a>Zgodność

Jeśli tworzysz aplikację który również uruchomić we wcześniejszych wersjach systemu android (początku interfejsu API poziom 4), czy będziesz używać [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) klasy zamiast `Notification.Builder`. Po utworzeniu powiadomienia z `NotificationCompat.Builder`, Android zapewnia czy powiadomień w wersji basic zawartości jest prawidłowo wyświetlany na starszych urządzeń. Jednakże ponieważ niektóre funkcje zaawansowane powiadomienia nie są dostępne w starszych wersjach systemu android, kodu jawnie musi obsługiwać problemy ze zgodnością style rozwinięte powiadomień, kategorii i poziomu widoczność jak wyjaśniono poniżej.

Aby użyć `NotificationCompat.Builder` w aplikacji, należy uwzględnić [biblioteki obsługi systemu Android w wersji 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet w projekcie.

Poniższy przykładowy kod przedstawia sposób utworzyć przy użyciu powiadomień w wersji basic `NotificationCompat.Builder`:

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();
```

Jak pokazano w poniższym przykładzie, wywołania metody dla opcji powiadomień niezbędne są identyczne z `Notification.Builder`. Jednak kod musi obsługiwać pobieranej zgodności problemy dotyczące powiadomień bardziej złożonych (opisany w następnej sekcji).

[LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications) przykładzie pokazano sposób użycia `NotificationCompat.Builder` można uruchomić drugi działania dla powiadomienia. Ten przykładowy kod znajduje się w [przy użyciu lokalnego powiadomienia o platformie Xamarin.Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) wskazówki.


### <a name="notification-styles"></a>Style powiadomień

Aby utworzyć *tekst Big*, *obrazu*, lub *skrzynki odbiorczej* styl powiadomienia z `NotificationCompat.Builder`, aplikacji należy używać wersji zgodności tych stylów. Na przykład, aby użyć *tekst Big* stylów, Utwórz wystąpienie `NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

Analogicznie, można użyć aplikacji `NotificationCompat.InboxStyle` i `NotificationCompat.BigPictureStyle` dla *skrzynki odbiorczej* i *obrazu* style, odpowiednio.


### <a name="notification-priority-and-category"></a>Powiadomienie priorytetu i kategorii

`NotificationCompat.Builder` obsługuje `SetPriority` — metoda (dostępne począwszy od systemu Android 4.1). Jednak `SetCategory` jest metoda *nie* obsługiwane przez `NotificationCompat.Builder` ponieważ kategorie są częścią nowego systemu powiadomień metadanych został wprowadzony w systemie Android 5.0.

Do obsługi starszych wersji systemu Android, gdzie `SetCategory` jest niedostępny, kodu można sprawdzić poziom interfejsu API w czasie wykonywania, aby wywołać warunkowo `SetCategory` , gdy poziom interfejsu API jest równa lub większa niż Android 5.0 (poziom interfejsu API 21):

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

W tym przykładzie aplikacja firmy **platformy docelowej** ma ustawioną wartość Android 5.0 i **minimalna wersja systemu Android** ustawiono **Android 4.1 (16 poziom interfejsu API)**. Ponieważ `SetCategory` jest dostępna na poziomie interfejsu API 21 i nowsze, wywoła ten przykładowy kod `SetCategory` tylko gdy jest dostępna &ndash; nie wywoła `SetCategory` gdy poziom interfejsu API jest mniejsza niż
21.


### <a name="lockscreen-visibility"></a>Widoczność ekranu blokady

Ponieważ Android nie obsługuje powiadomienia ekranu blokady przed Android 5.0 (poziom interfejsu API 21), `NotificationCompat.Builder` nie obsługuje `SetVisibility` metody. Jak opisano powyżej dla `SetCategory`, kodu można sprawdzić poziom interfejsu API środowiska uruchomieniowego i wywołanie `SetVisiblity` tylko gdy jest dostępne:

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetVisibility (Notification.Public);
}
```


## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak utworzyć lokalnego powiadomienia w systemie Android. On opisany anatomia powiadomienie, jego wyjaśniono, jak używać `Notification.Builder` do utworzenia powiadomień, jak styl powiadomienia w dużych ikon *tekst Big*, *obrazu* i *skrzynki odbiorczej*  formatów, sposobu ustawiania powiadomień ustawienia metadanych, takich jak priorytet, widoczność i kategorii oraz sposobu uruchamiania działania dla powiadomienia. W tym artykule opisano, jak działają te ustawienia powiadomień o projekcyjny wskazuje pozycję Nowy — ekranu blokady, i *nie przeszkadzać* funkcje wprowadzone w systemie Android 5.0. Ponadto przedstawiono sposób używania `NotificationCompat.Builder` do obsługi powiadomień zgodności z wcześniejszymi wersjami systemu android.

Aby uzyskać wskazówki dotyczące projektowania powiadomienia dla systemu Android, zobacz [powiadomienia](http://developer.android.com/preview/notifications.html).


## <a name="related-links"></a>Linki pokrewne

- [NotificationsLab (przykład)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (przykład)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Lokalnego powiadomienia w przewodniku dla systemu Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [Powiadomienia](http://developer.android.com/design/patterns/notifications.html)
- [Powiadomienia użytkownika](http://developer.android.com/training/notify-user/index.html)
- [powiadomienia](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
