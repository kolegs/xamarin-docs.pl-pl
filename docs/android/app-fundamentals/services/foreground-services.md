---
title: "Narzędzia usług"
ms.topic: article
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 857c7fe47ad5fa0f50fce95672bc8ffbf6771dc9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="foreground-services"></a>Narzędzia usług

Niektóre usługi są wykonywane niektórych zadań, które użytkownicy są aktywnie wiedzieć, te usługi są określane jako _pierwszego planu usług_. Przykładem usługi pierwszego planu to aplikacja, która zapewnia użytkownikowi wskazówki podczas kierowania lub przejście. Nawet jeśli aplikacja jest w tle, jest nadal ważny, czy usługa ma wystarczające zasoby do poprawnego działania i czy użytkownik ma szybki i wygodny sposób dostęp do aplikacji. Dla aplikacji systemu Android, to oznacza, że usługa pierwszego planu powinien zostać wyświetlony wyższy priorytet niż "regularnej" i podaj usługi pierwszego planu `Notification` wyświetlające Android tak długo, jak usługa jest uruchomiona.
 
Usługa pierwszego planu utworzeniu i uruchomieniu podobnie jak inne usługi. Podczas uruchamiania usługi zarejestruje się w Android jako usługa pierwszego planu.
 
W tym przewodniku będzie omawiać dodatkowych czynności, które należy podjąć zarejestrować usługi pierwszego planu i zatrzymać usługę po jej zakończeniu.

## <a name="registering-as-a-foreground-service"></a>Rejestrowanie jako usługa pierwszego planu

Usługa pierwszego planu to specjalny typ powiązania usługi lub uruchomiono usługi. Usługi, po rozpoczęciu, wywołuje [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/) metody do zarejestrowania się w usłudze Android jako usługa pierwszego planu.   

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

Jeśli usługa jest zatrzymany w wyniku wywołania `StopSelf` lub `StopService`, a następnie powiadomienie paska stanu zostaną również usunięte.


## <a name="related-links"></a>Linki pokrewne

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Lokalnego powiadomienia](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
