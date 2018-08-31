---
title: 'Przewodnik: Używanie powiadomień lokalnych na platformie Xamarin.Android'
description: W tym instruktażu pokazano, jak używać lokalnego powiadomienia w aplikacji platformy Xamarin.Android. Pokazuje podstawowe informacje dotyczące tworzenia i publikowania lokalnego powiadomienia. Gdy użytkownik kliknie powiadomienie w obszarze powiadomień, uruchomieniu drugiego działania.
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/16/2018
ms.openlocfilehash: a2ca3755e3201263584447ba47ec36d2096386da
ms.sourcegitcommit: f9fb316344fc4dce2fdce0dde3c5f4c2e4a42d08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/30/2018
ms.locfileid: "30766585"
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>Przewodnik: Używanie powiadomień lokalnych na platformie Xamarin.Android

_W tym instruktażu pokazano, jak używać lokalnego powiadomienia w aplikacji platformy Xamarin.Android. Pokazuje podstawowe informacje dotyczące tworzenia i publikowania lokalnego powiadomienia. Gdy użytkownik kliknie powiadomienie w obszarze powiadomień, uruchomieniu drugiego działania._


## <a name="overview"></a>Omówienie

W tym przewodniku utworzymy aplikację dla systemu Android, która generuje powiadomienie, gdy użytkownik kliknie przycisk w działaniu. Gdy użytkownik kliknie powiadomienie, uruchamia drugiego działania, który wyświetla ile razy użytkownik miał kliknął element button w pierwsze działanie.

Poniższych zrzutach ekranu przedstawiono kilka przykładów w tej aplikacji:

[![Przykładowe zrzuty ekranu z powiadomieniem](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)

> [!NOTE]
> Ten przewodnik koncentruje się na [interfejsów API NotificationCompat](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) z [biblioteki obsługi systemu Android](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Te interfejsy API zapewni maksymalnie wstecznej zgodności dla systemu Android 4.0 (poziom interfejsu API 14).

## <a name="creating-the-project"></a>Tworzenie projektu

Aby rozpocząć, Utwórz nowy projekt dla systemu Android przy użyciu **aplikacji dla systemu Android** szablonu. Nadajmy ten projekt **LocalNotifications**. (Jeśli użytkownik nie jest zaznajomiony z tworzeniem projekty platformy Xamarin.Android, zobacz [Witaj, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)

Edytuj plik zasobów **values/Strings.xml** tak, że zawiera dwa zasoby dodatkowe parametry, które będą używane, gdy nadejdzie czas, aby utworzyć kanał powiadomień:


```xml
<?xml version="1.0" encoding="utf-8"?>

<resources>
  <string name="Hello">Hello World, Click Me!</string>
  <string name="ApplicationName">Notifications</string>

  <string name="channel_name">Local Notifications</string>
  <string name="channel_description">The count from MainActivity.</string>
</resources>
```

### <a name="add-the-androidsupportv4-nuget-package"></a>Dodaj pakiet Android.Support.V4 NuGet

W tym przewodniku używamy `NotificationCompat.Builder` do tworzenia naszej lokalnego powiadomienia. Jak wyjaśniono w [powiadomień lokalnych](~/android/app-fundamentals/notifications/local-notifications.md), firma Microsoft musi zawierać [biblioteki obsługi systemu Android w wersji 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet w projekcie, aby użyć `NotificationCompat.Builder`.

Następnie możemy edytować **MainActivity.cs** i dodaj następującą `using` instrukcji tak, aby typy w `Android.Support.V4.App` są dostępne dla naszego kodu:

```csharp
using Android.Support.V4.App;
```

Ponadto firma Microsoft musi ułatwiają Wyczyść, aby kompilator, że użyto `Android.Support.V4.App` wersję `TaskStackBuilder` zamiast `Android.App` wersji. Dodaj następujący kod `using` instrukcję, aby rozwiązać wszelkie niejednoznaczności:

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```

### <a name="create-the-notification-channel"></a>Tworzenia kanału powiadomień

Następnie dodaj metodę, która `MainActivity` spowoduje to utworzenie kanału powiadomień (jeśli jest to konieczne):

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

    var name = Resources.GetString(Resource.String.channel_name);
    var description = GetString(Resource.String.channel_description);
    var channel = new NotificationChannel(CHANNEL_ID, name, NotificationImportance.Default)
                  {
                      Description = description
                  };

    var notificationManager = (NotificationManager) GetSystemService(NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Aktualizacja `OnCreate` metodę do wywołania tej nowej metody:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();
}
```


### <a name="define-the-notification-id"></a>Zdefiniuj identyfikator powiadomień

Musimy Unikatowy identyfikator dla naszych powiadomień i kanał powiadomień. Umożliwia edytowanie **MainActivity.cs** i dodaj następującą zmienną statycznej wystąpienia do `MainActivity` klasy:

```csharp
static readonly int NOTIFICATION_ID = 1000;
static readonly string CHANNEL_ID = "location_notification";
internal static readonly string COUNT_KEY = "count";
```

### <a name="add-code-to-generate-the-notification"></a>Dodaj kod, aby wygenerować powiadomienie

Następnie należy utworzyć nowy program obsługi zdarzeń dla przycisku `Click` zdarzeń. Dodaj następującą metodę do `MainActivity`:

```csharp
void ButtonOnClick(object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    var valuesForActivity = new Bundle();
    valuesForActivity.PutInt(COUNT_KEY, count);

    // When the user clicks the notification, SecondActivity will start up.
    var resultIntent = new Intent(this, typeof(SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras(valuesForActivity);

    // Construct a back stack for cross-task navigation:
    var stackBuilder = TaskStackBuilder.Create(this);
    stackBuilder.AddParentStack(Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent(resultIntent);

    // Create the PendingIntent with the back stack:
    var resultPendingIntent = stackBuilder.GetPendingIntent(0, (int) PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    var builder = new NotificationCompat.Builder(this, CHANNEL_ID)
                  .SetAutoCancel(true) // Dismiss the notification from the notification area when the user clicks on it
                  .SetContentIntent(resultPendingIntent) // Start up this activity when the user clicks the intent.
                  .SetContentTitle("Button Clicked") // Set the title
                  .SetNumber(count) // Display the count in the Content Info
                  .SetSmallIcon(Resource.Drawable.ic_stat_button_click) // This is the icon to display
                  .SetContentText($"The button has been clicked {count} times."); // the message to display.

    // Finally, publish the notification:
    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(NOTIFICATION_ID, builder.Build());

    // Increment the button press count:
    count++;
}
```

`OnCreate` Metoda MainActivity musi wykonać wywołanie do tworzenia kanału powiadomień i przypisywania `ButtonOnClick` metody `Click` zdarzenia przycisku (Zastąp wystąpienia delegata obsługi zdarzeń, dostarczone przez szablon):

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();

    // Display the "Hello World, Click Me!" button and register its event handler:
    var button = FindViewById<Button>(Resource.Id.MyButton);
    button.Click += ButtonOnClick;
}
```


### <a name="create-a-second-activity"></a>Tworzenie drugiego działania

Teraz musimy utworzyć inne czynności, które Android będą wyświetlane, gdy użytkownik kliknie naszych powiadomień. Dodaj kolejne działanie systemu Android do projektu o nazwie **SecondActivity**. Otwórz **SecondActivity.cs** i zastąp jego zawartość przy użyciu tego kodu:

```csharp
using System;
using Android.App;
using Android.OS;
using Android.Widget;

namespace LocalNotifications
{
    [Activity(Label = "Second Activity")]
    public class SecondActivity : Activity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            // Get the count value passed to us from MainActivity:
            var count = Intent.Extras.GetInt(MainActivity.COUNT_KEY, -1);

            // No count was passed? Then just return.
            if (count <= 0)
            {
                return;
            }

            // Display the count sent from the first activity:
            SetContentView(Resource.Layout.Second);
            var txtView = FindViewById<TextView>(Resource.Id.textView1);
            txtView.Text = $"You clicked the button {count} times in the previous activity.";
        }
    }
}
```

Układ zasobów należy także utworzyć **SecondActivity**. Dodaj nową **układ systemu Android** plik do projektu o nazwie **Second.axml**. Edytuj **Second.axml** i wklej następujący kod układu:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text=""
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:id="@+id/textView1" />
</LinearLayout>
```


### <a name="add-a-notification-icon"></a>Dodaj ikonę powiadomień

Na koniec należy dodać małą ikonę, która będzie wyświetlana w obszarze powiadomień, po uruchomieniu powiadomienia. Możesz skopiować [ta ikona](local-notifications-walkthrough-images/ic-stat-button-click.png) do projektu lub Utwórz niestandardową ikoną. Nazwa pliku ikony **ic\_stat\_przycisk\_click.png** i skopiuj go do **zasobów/drawable** folderu. Pamiętaj, aby używać **Dodaj > istniejący element...**  aby uwzględnić ten plik ikony w projekcie.


### <a name="run-the-application"></a>Uruchamianie aplikacji

Skompiluj i uruchom aplikację. Powinno zostać wyświetlone pierwszego działania, podobnie jak poniższy zrzut ekranu:

[![Pierwsze działanie zrzut ekranu](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

Jak przycisk, można zauważyć, że małą ikonę powiadomienia zostanie wyświetlona w obszarze powiadomień:

[![Pojawi się ikona powiadomień](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

Jeśli Przesuń w dół, a następnie udostępnić menu powiadomień, powinny zostać wyświetlone powiadomienie:

[![Komunikat z powiadomieniem](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

Kliknij powiadomienie, jego powinien zniknąć, gdy naszych innych działań powinien zostać uruchomiony &ndash; wyszukiwanie nieco podobnie jak na poniższym zrzucie ekranu:

[![Drugie działanie zrzut ekranu](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

Gratulacje! W tym momencie ukończono przewodnik dla systemu Android lokalnego powiadomienia, i masz próbkę pracy, który może odwoływać się do. Jest dużo więcej na temat powiadomień niż firma Microsoft wykazały, w tym miejscu, więc jeśli chcesz uzyskać więcej informacji, zapoznaj się z [dokumentacji firmy Google w powiadomieniach](http://developer.android.com/guide/topics/ui/notifiers/notifications.html).


## <a name="summary"></a>Podsumowanie

W tym przewodniku używane `NotificationCompat.Builder` do tworzenia i wyświetlać powiadomienia. A potem podstawowy przykład uruchomić drugiego działania jako sposób reagowanie na interakcję z użytkownikiem z powiadomieniem, a przedstawiona transfer danych z pierwszego działania do drugiego działania.


## <a name="related-links"></a>Linki pokrewne

- [LocalNotifications (przykład)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Kanały powiadomień systemu android Oreo](https://blog.xamarin.com/android-oreo-notification-channels/)
- [Powiadomienia](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
