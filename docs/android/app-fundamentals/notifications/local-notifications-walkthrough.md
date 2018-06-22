---
title: Wskazówki — przy użyciu lokalnego powiadomienia o platformie Xamarin.Android
description: W tym przewodniku przedstawiono sposób użycia lokalnego powiadomienia w aplikacji platformy Xamarin.Android. Jednak przedstawia podstawy tworzenia i publikowania lokalnego powiadomienia. Gdy użytkownik kliknie powiadomienie w obszarze powiadomień, uruchamiany drugi działania.
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/30/2018
ms.openlocfilehash: a2ca3755e3201263584447ba47ec36d2096386da
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766585"
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>Wskazówki — przy użyciu lokalnego powiadomienia o platformie Xamarin.Android

_W tym przewodniku przedstawiono sposób użycia lokalnego powiadomienia w aplikacji platformy Xamarin.Android. Jednak przedstawia podstawy tworzenia i publikowania lokalnego powiadomienia. Gdy użytkownik kliknie powiadomienie w obszarze powiadomień, uruchamiany drugi działania._


## <a name="overview"></a>Omówienie

W tym przewodniku utworzymy aplikację systemu Android, która zgłasza powiadomienie, gdy użytkownik kliknie przycisk w działaniu. Gdy użytkownik kliknie powiadomienie, uruchamia drugi działanie, które wskazuje liczbę razy użytkownik miał kliknął element button w pierwsze działanie.

Poniższe zrzuty ekranu przedstawiają przykłady tej aplikacji:

[![Zrzuty ekranu przedstawiające Przykładowe powiadomienie](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)



## <a name="walkthrough"></a>Wskazówki

Aby rozpocząć, Utwórz nowy projekt systemu Android przy użyciu **aplikacji systemu Android** szablonu. Umożliwia wywołanie tego projektu **LocalNotifications**. (Jeśli nie masz doświadczenia w tworzeniu projektów platformy Xamarin.Android, zobacz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)


### <a name="add-the-androidsupportv4app-component"></a>Dodaj składnik Android.Support.V4.App

W tym przewodniku używamy `NotificationCompat.Builder` do tworzenia lokalnego powiadomienia o naszych. Zgodnie z objaśnieniem w [lokalnego powiadomienia](~/android/app-fundamentals/notifications/local-notifications.md), firma Microsoft musi zawierać [biblioteki obsługi systemu Android w wersji 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet w naszym projektu do użycia `NotificationCompat.Builder`.

Następnie umożliwia edytowanie **MainActivity.cs** i dodaj następującą `using` instrukcji, aby typy w `Android.Support.V4.App` są dostępne dla naszego kodu:

```csharp
using Android.Support.V4.App;
```

Ponadto możemy ustawić go Wyczyść, aby kompilator, który jest używany `Android.Support.V4.App` wersji `TaskStackBuilder` zamiast `Android.App` wersji. Dodaj następujące `using` instrukcji, aby usunąć wszelkie niejednoznaczność:

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```


### <a name="define-the-notification-id"></a>Zdefiniuj identyfikator powiadomień

Unikatowy identyfikator są wymagane dla naszych powiadomienia. Umożliwia edytowanie **MainActivity.cs** i dodaj następujące zmiennej wystąpień statycznych `MainActivity` klasy:

```csharp
private static readonly int ButtonClickNotificationId = 1000;
```


### <a name="add-code-to-generate-the-notification"></a>Dodaj kod, aby wygenerować powiadomienie

Następnie należy utworzyć nowy program obsługi zdarzeń dla przycisku `Click` zdarzeń. Dodaj następującą metodę do `MainActivity`:

```csharp
private void ButtonOnClick (object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    Bundle valuesForActivity = new Bundle();
    valuesForActivity.PutInt ("count", count);

    // When the user clicks the notification, SecondActivity will start up.
    Intent resultIntent = new Intent(this, typeof (SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras (valuesForActivity);

    // Construct a back stack for cross-task navigation:
    TaskStackBuilder stackBuilder = TaskStackBuilder.Create (this);
    stackBuilder.AddParentStack (Java.Lang.Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent (resultIntent);

    // Create the PendingIntent with the back stack:            
    PendingIntent resultPendingIntent =
        stackBuilder.GetPendingIntent (0, (int)PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
        .SetAutoCancel (true)                    // Dismiss from the notif. area when clicked
        .SetContentIntent (resultPendingIntent)  // Start 2nd activity when the intent is clicked.
        .SetContentTitle ("Button Clicked")      // Set its title
        .SetNumber (count)                       // Display the count in the Content Info
        .SetSmallIcon(Resource.Drawable.ic_stat_button_click)  // Display this icon
        .SetContentText (String.Format(
            "The button has been clicked {0} times.", count)); // The message to display.

    // Finally, publish the notification:
    NotificationManager notificationManager =
        (NotificationManager)GetSystemService(Context.NotificationService);
    notificationManager.Notify(ButtonClickNotificationId, builder.Build());

    // Increment the button press count:
    count++;
}
```

W `OnCreate` metody, Przypisz to `ButtonOnClick` metodę `Click` zdarzeń przycisku (Zastąp obiektu delegowanego obsługi zdarzeń dostarczone przez szablon):

```csharp
button.Click += ButtonOnClick;
```


### <a name="create-a-second-activity"></a>Utworzyć drugi działanie

Teraz należy utworzyć innego działania Android wyświetlane, gdy użytkownik kliknie naszych powiadomień. Dodaj kolejne działanie systemu Android do projektu o nazwie **SecondActivity**. Otwórz **SecondActivity.cs** i zastąp jego zawartość tego kodu:

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
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Get the count value passed to us from MainActivity:
            int count = Intent.Extras.GetInt ("count", -1);

            // No count was passed? Then just return.
            if (count <= 0)
                return;

            // Display the count sent from the first activity:
            SetContentView (Resource.Layout.Second);
            TextView textView = FindViewById<TextView>(Resource.Id.textView);
            textView.Text = String.Format (
                "You clicked the button {0} times in the previous activity.", count);
        }
    }
}
```

Układ zasobów należy także utworzyć **SecondActivity**. Dodaj nową **Android układu** plik do projektu o nazwie **Second.axml**. Edytuj **Second.axml** i wklej poniższy kod układu:

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
        android:id="@+id/textView" />
</LinearLayout>
```


### <a name="add-a-notification-icon"></a>Dodawanie ikony powiadomień

Na koniec Dodajmy mała ikona, która będzie wyświetlana w obszarze powiadomień, po uruchomieniu naszych powiadomień. Możesz skopiować [ta ikona](local-notifications-walkthrough-images/ic-stat-button-click.png) do projektu lub Utwórz własną ikon niestandardowych. Firma Microsoft będzie Nazwij plik ikony **MF\_stat\_przycisk\_click.png** i skopiuj go do **obiektów drawable/zasoby** folderu. Pamiętaj, aby użyć **Dodaj > istniejący element...**  Aby dołączyć ten plik ikony do projektu.


### <a name="run-the-application"></a>Uruchamianie aplikacji

Umożliwia tworzenie i uruchamianie aplikacji. Powinna pojawić gdzie wykonywanie pierwszego działania, podobnie jak poniższy zrzut ekranu:

[![Pierwsze działanie zrzut ekranu](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

Po kliknięciu przycisku można zauważyć, mała ikona powiadomienia są wyświetlane w obszarze powiadomień:

[![Pojawi się ikona powiadomienia](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

Jeśli szybko przesuń w dół i ujawnia menu powiadomień, powinno zostać wyświetlone powiadomienie:

[![Komunikat z powiadomieniem](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

Gdy kliknij powiadomienie, powinien on zniknąć i powinna być uruchamiana w naszych innych działań &ndash; wyszukiwania przypominać poniższy zrzut ekranu:

[![Drugi działania zrzut ekranu](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

Gratulacje! W tym momencie zostały ukończone wskazówki Android lokalnego powiadomienia i masz próbki pracy, którego może dotyczyć. Jest dużo więcej na temat powiadomień niż firma Microsoft wykazały, więc jeśli chcesz uzyskać więcej informacji, zapoznaj się [dokumentacji firmy Google na powiadomienia](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) i Android [powiadomienia](http://developer.android.com/design/patterns/notifications.html) przewodnik dotyczący projektowania.



## <a name="summary"></a>Podsumowanie

W tym przewodniku użyliśmy `NotificationCompat.Builder` tworzenie i wyświetlanie powiadomień. Widzieliśmy podstawowy przykład uruchomić drugi działania jako sposób odpowiadanie na interakcję użytkownika z powiadomienia, a firma Microsoft przedstawiona transferu danych z pierwszego działania do drugiego działania.


## <a name="related-links"></a>Linki pokrewne

- [LocalNotifications (przykład)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Wzorce projektowe Przewodnik na powiadomienia](http://developer.android.com/design/patterns/notifications.html)
- [powiadomienia](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
