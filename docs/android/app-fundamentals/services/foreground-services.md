---
title: Usługi pierwszego planu
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: 47e1eda2f701b654f81f664050847677fba8bcc5
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986037"
---
# <a name="foreground-services"></a>Usługi pierwszego planu

Usługi pierwszego planu jest specjalnym typem powiązanej usługi lub uruchomiono usługę. Od czasu do czasu usługi będzie wykonywać zadania, które użytkownicy muszą znać aktywnie, te usługi są określane jako _usługi pierwszego planu_. Przykładem usługi pierwszego planu, to aplikacja, która dostarcza użytkownika z kierunkami podczas prowadzenia lub zalet. Nawet jeśli aplikacja znajduje się w tle, jest nadal ważny, że usługa ma wystarczające zasoby do prawidłowego działania i czy użytkownik ma szybki i wygodny sposób uzyskiwać dostęp do aplikacji. Dla aplikacji systemu Android, to oznacza, że usługi pierwszego planu, powinien zostać wyświetlony wyższy priorytet niż "regularne" service i usługi pierwszego planu, należy podać `Notification` systemu Android będzie wyświetlana tak długo, jak usługa jest uruchomiona.
 
Aby uruchomić usługę pierwszego planu, aplikacja musi wysyłać cel, który zawiera informacje dla systemu Android, aby uruchomić usługę. Następnie usługa musi zarejestrować się jako usługa pierwszego planu z systemem Android. Należy używać w aplikacjach z systemem Android 8.0 (lub nowszy) `Context.StartForegroundService` metodę, aby uruchomić usługę, w przypadku, gdy należy używać w aplikacjach, które działają na urządzeniach ze starszą wersją systemu android `Context.StartService`

Ta metoda rozszerzenia języka C# jest przykładem sposobu uruchamiania usługi pierwszego planu. W systemie Android 8.0 i nowsze użyje `StartForegroundService` metody, w przeciwnym razie starszej wersji `StartService` zostanie użyta metoda.  

```csharp
public static void StartForegroundServiceComapt<T>(this Context context, Bundle args = null) where T : Service
{
    var intent = new Intent(context, typeof(T));
    if (args != null) 
    {
        intent.PutExtras(args);
    }

    if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.O)
    {
        context.StartForegroundService(intent);
    }
    else
    {
        context.StartService(intent);
    }
}
```

## <a name="registering-as-a-foreground-service"></a>Rejestrowanie w trybie usługi pierwszego planu

Po rozpoczęciu usługi pierwszego planu, jego musi zarejestrować się z systemem Android za pomocą wywołania [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/). Jeśli usługa jest uruchomiona przy użyciu `Service.StartForegroundService` metody, ale nie rejestruje, systemu Android będą Zatrzymaj usługę i Flaga aplikacji jako przestanie odpowiadać.

`StartForeground` przyjmuje dwa parametry, które są wymagane:
 
* Wartość całkowitą, która jest unikatowa w obrębie aplikacji do identyfikowania usługi.
* A `Notification` obiekt, który Android będą wyświetlane na pasku stanu, tak długo, jak usługa jest uruchomiona.

Android będą wyświetlane powiadomienia na pasku stanu, tak długo, jak usługa jest uruchomiona. Powiadomienie, co najmniej zapewni wizualna podpowiedź dla użytkownika, że usługa jest uruchomiona. W idealnym przypadku powiadomienie powinien udostępniać użytkownikom skrót do aplikacji lub prawdopodobnie niektóre przyciski akcji do sterowania aplikacji. Na przykład jest odtwarzacz muzyczny &ndash; powiadomień, która jest wyświetlana może być przyciski grać pause/muzyki, przewiń do poprzedniego lub od razu przejść do następnego utworu. 

Ten fragment kodu jest przykładem rejestrowaniu usługi jako usługi pierwszego planu:   

```csharp
// This is any integer value unique to the application.
public const int SERVICE_RUNNING_NOTIFICATION_ID = 10000;

public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
{
    // Code not directly related to publishing the notification has been omitted for clarity.
    // Normally, this method would hold the code to be run when the service is started.
    
    var notification = new Notification.Builder(this)
        .SetContentTitle(Resources.GetString(Resource.String.app_name))
        .SetContentText(Resources.GetString(Resource.String.notification_text))
        .SetSmallIcon(Resource.Drawable.ic_stat_name)
        .SetContentIntent(BuildIntentToShowMainActivity())
        .SetOngoing(true)
        .AddAction(BuildRestartTimerAction())
        .AddAction(BuildStopServiceAction())
        .Build();

    // Enlist this instance of the service as a foreground service
    StartForeground(SERVICE_RUNNING_NOTIFICATION_ID, notification);
}
```

Poprzednie powiadomienie zostanie wyświetlone powiadomienie paska stanu, który jest podobny do następującego:

![Obraz przedstawiający powiadomienie na pasku stanu](foreground-services-images/foreground-services-01.png "obraz przedstawiający powiadomienie na pasku stanu")

Ten zrzut ekranu przedstawia rozwinięty powiadomienie na pasku powiadomień za pomocą dwie akcje, które umożliwiają użytkownikom, aby kontrolować usługę:

![Obraz przedstawiający powiadomienie rozszerzonej](foreground-services-images/foreground-services-02.png "obraz przedstawiający powiadomienie rozszerzonej.")

Więcej informacji na temat powiadomień jest dostępna w [powiadomień lokalnych](~/android/app-fundamentals/notifications/local-notifications.md) części [powiadomień systemu Android](~/android/app-fundamentals/notifications/index.md) przewodnik.

## <a name="unregistering-as-a-foreground-service"></a>Wyrejestrowywanie jako usługi pierwszego planu

Usługi można cofnąć listy sama jako usługi pierwszego planu przez wywołanie metody `StopForeground`. `StopForeground` Usługa nie zostanie zatrzymana, ale jej spowoduje usunięcie ikony powiadomień i sygnały systemu Android, które ta usługa może zostać wyłączony, jeśli to konieczne.

Powiadomienie paska stanu, które jest wyświetlana, może zostać także usunięty przez przekazanie `true` metody: 

```csharp
StopForeground(true);
```

Jeśli usługa jest zatrzymywana z wywołaniem `StopSelf` lub `StopService`, powiadomień paska stanu zostaną usunięte.

## <a name="related-links"></a>Linki pokrewne

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForeground](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Powiadomienia lokalne](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
