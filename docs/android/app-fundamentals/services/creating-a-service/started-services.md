---
title: Rozpoczęto usług za pomocą platformy Xamarin.Android
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: c0aeeaad3798dd840e69c6da6d7857298f4da3c1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767469"
---
# <a name="started-services-with-xamarinandroid"></a>Rozpoczęto usług za pomocą platformy Xamarin.Android

## <a name="started-services-overview"></a>Omówienie uruchomionych usług

Uruchomionych usług na ogół wykonać jednostki pracy nie oferuje żadnych bezpośrednich opinii lub nie zwróciło wyników do klienta. Przykład jednostki pracy to usługa, która przekazuje plik do serwera. Klient będzie żądania do usługi w celu przekazania pliku z urządzenia do witryny sieci Web. Usługa ciche przekazać plik (nawet, jeśli aplikacja nie ma żadnych działań na pierwszym planie), a jego zakończenia, po zakończeniu przekazywania. Należy koniecznie należy pamiętać, że uruchomiono usługi zostanie uruchomiony w wątku interfejsu użytkownika aplikacji. Oznacza to, że w przypadku usługi do wykonywania pracy, który będzie blokować wątku interfejsu użytkownika, musi utworzyć i usuwanie wątków w razie potrzeby.

W przeciwieństwie do powiązanej usługi nie ma żadnych kanału komunikacyjnego między usługą wprowadzenie "czysty" i jego klientów. Oznacza to, że uruchomiono usługi wdroży niektóre metody cyklu życia innego niż powiązanej usługi. Poniższa lista przedstawia typowe metody cyklu życia w uruchomiono usługi:

* `OnCreate` &ndash; Wywoływane jeden raz po pierwszym uruchomieniu usługi. Jest to, gdy kod inicjujący powinny zostać wdrożone.
* `OnBind` &ndash; Ta metoda musi być implementowana przez wszystkie klasy usługi, jednak uruchomiono usługi nie ma zazwyczaj klient powiązane z nim. W związku z tym zwraca właśnie uruchomiono usługi `null`. Z kolei ma usługi hybrydowych (która jest kombinacją powiązania usługi i uruchomiono usługę) do wdrożenia i zwracać `Binder` dla klienta.
* `OnStartCommand` &ndash; Wywoływane dla każdego żądania uruchomić usługę, albo w odpowiedzi na wywołanie `StartService` lub ponownego uruchomienia systemu. Jest to, gdzie usługa można rozpocząć wszystkie długotrwałe zadania. Metoda zwraca `StartCommandResult` wartość wskazującą sposób lub jeśli system powinna obsługiwać ponownego uruchamiania usługi po zamknięciu z powodu braku pamięci. To wywołanie odbywa się w głównym wątku. Ta metoda została opisana szczegółowo poniżej.
* `OnDestroy` &ndash; Ta metoda jest wywoływana, gdy usługa jest niszczone. Służy do wykonywania żadnych final Oczyszczanie wymagane.

Jest ważne metody uruchomiono usługi `OnStartCommand` metody. Zostanie ono wywołane zawsze usługa odbierze żądanie wykonania dodatkowych czynności. Poniższy fragment kodu przedstawiono przykładowy `OnStartCommand`: 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

Pierwszym parametrem jest `Intent` obiekt zawierający metadane dotyczące pracy do wykonania. Drugi parametr zawiera `StartCommandFlags` wartość, która zawiera informacje o wywołaniu metody. Ten parametr ma jedną z dwóch wartości:

* `StartCommandFlag.Redelivery` &ndash; Oznacza to, że `Intent` jest ponowna dostarczania poprzedniej `Intent`. Ta wartość zostanie podana podczas zwrócił usługi `StartCommandResult.RedeliverIntent` , ale został zatrzymany przed jego może zostać prawidłowo zamknięty.
* `StartCommandFlag.Retry` &dash; Ta wartość jest Odebrano poprzedniej `OnStartCommand` wywołanie nie powiodło się i Android próbuje uruchomić usługę ponownie przy użyciu tego samego zamiaru jak poprzednia próba nie powiodło się.
 
Na koniec trzeci parametr jest wartość całkowitą, który jest unikatowy dla aplikacji, która identyfikuje żądania. Istnieje możliwość, że wiele wywołań może wywołać ten sam obiekt usługi. Ta wartość jest używana do skojarzenia żądanie zatrzymania usługi za pomocą danego żądania, aby uruchomić usługę. Go zostanie omówiona bardziej szczegółowo w sekcji [zatrzymanie usługi](#Stopping_the_Service). 

Wartość `StartCommandResult` są zwracane przez usługę jako sugestię w systemie Android co zrobić, jeśli usługa jest przerwana z powodu ograniczenia zasobów. Istnieją trzy możliwe wartości `StartCommandResult`:

* **[StartCommandResult.NotSticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.NotSticky/)**  &ndash; ta wartość informuje system Android, która nie jest konieczne ponowne uruchomienie usługi, który ma zostały zakończone. Na przykład należy wziąć pod uwagę to usługa, która generuje miniatury dla galerii w aplikacji. Jeśli usługa jest przerwana, nie jest niezwykle ważny od razu ponownie utworzyć miniatury &ndash; miniatury można utworzyć ponownie przy następnym uruchomieniu aplikacji.
* **[StartCommandResult.Sticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.Sticky/)**  &ndash; informuje systemu Android, aby ponownie uruchomić usługę, ale nie w celu dostarczenia ostatniego celem, który uruchomił usługę. Jeśli nie ma żadnych oczekujące intencje do obsługi, a następnie `null` zostanie podana dla parametru metody konwersji. Na przykład może być to aplikacja odtwarzacza muzyki; usługa uruchomi gotowe do odtwarzania muzyki, ale zostanie odtworzona ostatniego utworu. 
* **[StartCommandResult.RedeliverIntent](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.RedeliverIntent/)**  &ndash; ta wartość zostanie stwierdzić, Android, uruchom ponownie usługę i ponownie dostarczania ostatniego `Intent`. Na przykład jest to usługa, która pobiera plik danych dla aplikacji. Jeśli usługa jest przerwana, pliku danych nadal muszą zostać pobrane. Zwracając `StartCommandResult.RedeliverIntent`, gdy Android uruchamia ponownie usługę, będzie on również zawierał zamiar (zawierający adres URL pliku do pobrania) do usługi. Umożliwi to pobieranie ponownie uruchomić lub wznowić (w zależności od implementacji dokładnego kodu).

Brak czwarta wartość `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`. Ta wartość jest zwracana przez `OnStartCommand` i opisano, jak Android będzie usługa ma zostały zakończone. Ta wartość nie jest zwykle używane do uruchamiania usługi.

Zdarzenia cyklu życia klucza uruchomiono usługi są wyświetlane na tym diagramie: 

![Diagram przedstawiający kolejność, w którym są wywoływane metody cyklu życia](started-services-images/started-service-01.png "diagram przedstawiający kolejność, w którym są wywoływane metody cyklu życia.")


<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>Zatrzymywanie usługi

Uruchomiono usługi będzie działać w nieskończoność; Android zachowa Usługa uruchomiona tak długo, jak dostępne są wystarczające zasoby systemu. Klienta należy zatrzymać usługę albo usługa może zatrzymać się po zakończeniu pracy. Istnieją dwa sposoby, aby zatrzymać usługę: 
 
* **[Android.Content.Context.StopService()](https://developer.xamarin.com/api/member/Android.Content.Context.StopService/p/Android.Content.Intent/)**  &ndash; może żądać klient (na przykład działanie), usługa zatrzymać wywołanie `StopService` metody: 

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

* **[Android.App.Service.StopSelf()](https://developer.xamarin.com/api/member/Android.App.Service.StopSelf()/)**  &ndash; usługa może zostać wyłączony sam wywołując `StopSelf`:

    ```csharp
    StopSelf();
    ```
    
### <a name="using-startid-to-stop-a-service"></a>Aby zatrzymać usługę przy użyciu startId

Wiele wywołań może wysłać żądanie uruchomienia usługi. W przypadku rozpoczęcia oczekujące żądanie usługi można użyć `startId` jest ono przekazywane do `OnStartCommand` aby zapobiec przedwcześnie zatrzymanie usługi. `startId` Odpowiada najnowsze wywołanie `StartService`i jest zwiększany za każdym razem, jest ona wywoływana. W związku z tym Jeśli kolejne żądanie `StartService` nie ma jeszcze spowodowały wywołanie `OnStartCommand`, można wywołać usługę `StopSelfResult`, przekazanie jej najnowszej wartości `startId` otrzymała (zamiast kontaktować się po prostu `StopSelf`). Jeśli wywołanie `StartService` jeszcze nie doprowadziło do odpowiedniego wywołania `OnStartCommand`, systemu lokacji nie zatrzyma usługi, ponieważ `startId` używane w `StopSelf` wywołania nie odpowiadają najnowszej `StartService` wywołania.


## <a name="related-links"></a>Linki pokrewne

- [StartedServicesDemo (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/StartedServicesDemo/)
- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service)
- [Android.App.StartCommandFlags](https://developer.xamarin.com/api/type/Android.App.StartCommandFlags)
- [Android.App.StartCommandResult](https://developer.xamarin.com/api/type/Android.App.StartCommandResult)
- [Android.Content.BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Android.Content.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent)
- [Android.OS.Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Android.Widget.Toast](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
- [Ikony paska stanu](http://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
