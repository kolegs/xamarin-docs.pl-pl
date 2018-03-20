---
title: "Narzędzia usług"
ms.topic: article
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: d1267bc4a530deb6dfb6eb2e30bee2facabd8fed
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2018
---
# <a name="foreground-services"></a>Narzędzia usług

Usługa pierwszego planu to specjalny typ powiązania usługi lub uruchomiono usługi. Od czasu do czasu usługi będzie wykonywać zadania, które użytkownicy muszą znać aktywnie, te usługi są określane jako _pierwszego planu usług_. Przykładem usługi pierwszego planu to aplikacja, która zapewnia użytkownikowi wskazówki podczas kierowania lub przejście. Nawet jeśli aplikacja jest w tle, jest nadal ważny, czy usługa ma wystarczające zasoby do poprawnego działania i czy użytkownik ma szybki i wygodny sposób dostęp do aplikacji. Dla aplikacji systemu Android, to oznacza, że usługa pierwszego planu powinien zostać wyświetlony wyższy priorytet niż "regularnej" i podaj usługi pierwszego planu `Notification` wyświetlające Android tak długo, jak usługa jest uruchomiona.
 
Aby uruchomić usługę pierwszego planu, aplikacja musi być wysłany celem informujący o Android, aby uruchomić usługę. Następnie usługa musi zarejestrować się jako pierwszego planu usługi za pomocą systemu Android. Aplikacje, które są uruchomione na 8.0 dla systemu Android (lub nowszej) należy użyć `Context.StartForegroundService` metodę, aby uruchomić usługę, podczas powinien używać aplikacji, które działają na urządzeniach przy użyciu starszej wersji systemu android `Context.StartService`

Ta metoda rozszerzenia C# jest przykładem uruchomić usługę pierwszego planu. W systemie Android 8.0 i nowsze użyje `StartForegroundService` metody, w przeciwnym razie starszej `StartService` zostanie użyta metoda.  

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

## <a name="registering-as-a-foreground-service"></a>Rejestrowanie jako usługa pierwszego planu

Po uruchomieniu usługi pierwszego planu, jego musi zarejestrować się z systemem Android za pomocą [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/). Jeśli usługa jest uruchomiona z `Service.StartForegroundService` metody, ale nie rejestruje, Android będą Zatrzymaj usługę i Flaga aplikacji jako przestać odpowiadać.

`StartForeground` przyjmuje dwa parametry, które są wymagane:
 
* Wartość całkowitą, która jest unikatowa w aplikacji do identyfikowania usługi.
* A `Notification` obiekt, który Android będą wyświetlane na pasku stanu dla tak długo, jak usługa jest uruchomiona.

Android wyświetli powiadomienie na pasku stanu dla tak długo, jak usługa jest uruchomiona. Powiadomienia, co najmniej zapewni wizualnie użytkownikowi, że usługa jest uruchomiona. W idealnym przypadku powiadomienia powinien zapewnić użytkownikowi skrót do aplikacji lub prawdopodobnie niektóre przycisków akcji, aby kontrolować aplikacji. Na przykład jest odtwarzaczem muzyki &ndash; powiadomień, która jest wyświetlana, może być przycisków Wstrzymaj/Odtwarzaj muzyki, przewiń do poprzedniego lub przejdź do następnego utworu. 

Następujący fragment kodu jest przykład rejestrowania usługi jako usługa pierwszego planu:   

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

Poprzednie powiadomień wyświetli powiadomienie paska stanu, która jest podobny do następującego:

![Obraz przedstawiający powiadomień na pasku stanu](foreground-services-images/foreground-services-01.png "obraz przedstawiający powiadomień na pasku stanu")

Ten zrzut ekranu przedstawia rozwinięty powiadomień na pasku powiadomień o dwie akcje, które umożliwiają użytkownikowi na kontrolowanie usługi:

![Obraz przedstawiający rozwinięty powiadomień](foreground-services-images/foreground-services-02.png "obraz przedstawiający rozwinięty powiadomień.")

Więcej informacji na temat powiadomień jest dostępna w [lokalnego powiadomienia](~/android/app-fundamentals/notifications/local-notifications.md) sekcji [powiadomień systemu Android](~/android/app-fundamentals/notifications/index.md) przewodnik.

## <a name="unregistering-as-a-foreground-service"></a>Wyrejestrowywanie jako usługa pierwszego planu

Usługi można cofnąć listy się jako usługa pierwszego planu, wywołując metodę `StopForeground`. `StopForeground` Usługa nie zostanie zatrzymana, ale usunie ikony powiadomień i sygnały systemu Android, które ta usługa może zostać wyłączony, jeśli to konieczne.

Wyświetlane powiadomienia paska stanu może zostać także usunięty przez przekazanie `true` do metody: 

```csharp
StopForeground(true);
```

Jeśli usługa jest zatrzymany w wyniku wywołania `StopSelf` lub `StopService`, powiadomień paska stanu zostaną usunięte.

## <a name="related-links"></a>Linki pokrewne

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Powiadomienia lokalne](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
