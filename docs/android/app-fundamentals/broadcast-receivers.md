---
title: Emisji odbiorców w platformy Xamarin.Android
description: W tej sekcji omówiono sposób użycia odbiornik emisji.
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/20/2018
ms.openlocfilehash: 9c17641312384634983c2cbb34fa923a9416c9f7
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/20/2018
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Emisji odbiorców w platformy Xamarin.Android

_W tej sekcji omówiono sposób użycia odbiornik emisji._

## <a name="broadcast-receiver-overview"></a>Omówienie odbiornika emisji

A _emisji odbiornika_ jest składnikiem systemu Android umożliwiający aplikacji odpowiada na komunikaty (Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)) rozgłaszanych przez system operacyjny Android lub przez aplikację. Wykonaj emisje _publikowania / subskrypcji_ modelu &ndash; zdarzenia powoduje emisji zostanie opublikowany i odbierane przez te składniki, które są zainteresowane w zdarzeniu. 

Android identyfikuje dwa typy programów:

* **Jawne emisji** &ndash; tych typów emisji docelowych określonych aplikacji. Najczęściej używane jawne emisji jest uruchomienia działania. Przykład jawne emisji, gdy aplikacja potrzebuje do wybierania numeru telefonu; wyśle jego celem przeznaczonego dla aplikacji Phone w systemach Android i przebiegu wzdłuż numer telefonu ma być wybrany. Android skieruje następnie celem aplikacji telefonicznej.
* **Niejawne broadcase** &ndash; emisje te są wysyłane do wszystkich aplikacji na urządzeniu. Na przykład niejawne emisji `ACTION_POWER_CONNECTED` celem. Celem tego jest publikowany w każdym razem, gdy Android wykryje, że jest ładowana baterii na urządzeniu. Android będzie przekierowywać to zamierzone do wszystkich aplikacji, które zostały zarejestrowane dla tego zdarzenia.

Odbiornik emisji jest podklasą `BroadcastReceiver` przesłonięcie typu i [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/) metody. Android zostanie wykonany `OnReceive` w głównym wątku, więc tej metody należy tak zaprojektować wykonać szybko. Należy zwrócić uwagę, gdy dochodzi do uruchamiania wątków w `OnReceive` ponieważ Android może zakończyć proces po zakończeniu metody. Jeśli odbiornik emisji należy wykonać długotrwała pracy, zaleca się zaplanowanie _zadania_ przy użyciu `JobScheduler` lub _dyspozytora zadania Firebase_. Planowanie pracy z zadaniem zostaną dokładniej omówione w przewodniku oddzielne.

_Konwersji filtru_ służy do rejestrowania emisji odbiornika Android prawidłowo rozesłać komunikaty. Filtr konwersji można określić w czasie wykonywania (jest to czasami określane jako _kontekst zarejestrowany odbiornik_ lub jako _dynamicznej rejestracji_) lub może być statycznie zdefiniowany w ( manifestusystemuAndroid_zarejestrowany manifestu odbiorcy_). Xamarin.Android zapewnia C# atrybutu, `IntentFilterAttribute`, który zarejestruje statycznie filtr konwersji (to zostanie dokładnie omówione w dalszej części tego przewodnika). Począwszy od systemu Android 8.0, nie jest możliwe w dla aplikacji, aby zarejestrować statycznie niejawne emisji.

Główną różnicą między odbiornika zarejestrowany manifestu i kontekst zarejestrowany odbiornik jest, że kontekst zarejestrowany odbiornik tylko odpowie na emisje aplikacja jest uruchomiona, gdy odbiornik zarejestrowany manifest mogą odpowiadać na emituje, nawet jeśli nie jest uruchomiona aplikacja.  

Istnieją dwa zestawy interfejsów API do zarządzania emisji odbiornik i wysyłanie emisji:

1. **`Context`** &ndash; `Android.Content.Context` Klasa może być używana do rejestrowania emisji odbiornika, który będzie odpowiadał na zdarzenia systemowe. `Context` Jest również używana do publikowania emisji całego systemu.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; Jest to interfejs API, który jest dostępny za pośrednictwem [pakietu NuGet w wersji 4 Biblioteka obsługi Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Ta klasa jest używana do emisji i emisji odbiorniki samodzielnie w kontekście aplikacji, która używa ich. Ta klasa może być pomaga zapobiegać odpowiada na emisje tylko do aplikacji i wysyłanie komunikatów do prywatnego odbiorniki innych aplikacji.

Emisji odbiorcy mogą nie wyświetlać okien dialogowych i jest zalecane, aby uruchomić działanie w emisji odbiornika. Jeśli odbiornik emisji należy powiadomić użytkownika, należy ją opublikować powiadomienie.

Nie jest możliwe powiązanie lub uruchomić usługę z wewnątrz emisji odbiornika. 

W tym przewodniku opisano sposobu tworzenia odbiornika emisji i go zarejestrować, dzięki czemu może odbierać emisji.

## <a name="creating-a-broadcast-receiver"></a>Tworzenie odbiorcy emisji

Aby utworzyć odbiornik emisji w Xamarin.Android, aplikacja powinna podklasy `BroadcastReceiver` klasy, za pomocą adorn `BroadcastReceiverAttribute`i Zastąp `OnReceive` — metoda:

```csharp
[BroadcastReceiver(Enabled = true, Exported = false)]
public class SampleReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here.

        String value = intent.GetStringExtra("key");
    }
}
```

Kiedy Xamarin.Android kompiluje klasy, jego również zaktualizuje AndroidManifest niezbędnych danych meta zarejestrować odbiornika. Statycznie zarejestrowany odbiornika emisji `Enabled` poprawnie musi mieć ustawioną `true`, w przeciwnym razie Android nie można utworzyć wystąpienia odbiornika.
 
`Exported` Właściwość określa, czy odbiornik emisji może odbierać komunikaty z poza aplikacją. Jeśli właściwość nie jest jawnie ustawiona, wartość domyślna właściwości zależy od systemu Android na podstawie przypadku żadnych zamiar filtrów skojarzonych z emisji odbiornika. Jeśli istnieje co najmniej jeden filtr zamiar emisji odbiornika, Android zakłada, że `Exported` jest właściwość `true`. Jeśli nie zamiar-filtrów skojarzonych z odbiornikiem emisji, a następnie Android przyjmie założenie, że wartość jest `false`. 

`OnReceive` Metody odbiera odwołanie do `Intent` który został wysłany do emisji odbiornika. To sprawia, że jest możliwe nadawcy opcje do przekazania wartości do emisji odbiornika.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>Statycznie zarejestrowanie odbiornika emisji opcje filtru

Gdy `BroadcastReceiver` zostanie nadany [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/), Xamarin.Android doda niezbędnych `<intent-filter>` elementu Android manifestu w czasie kompilacji. Poniższy fragment jest przykładem emisji odbiornika, który będzie uruchamiany, gdy urządzenie zostało zakończone, rozruch (jeśli jest to odpowiednie dla systemu Android uprawnienia zostały przyznane przez użytkownika):

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { Android.Content.Intent.ActionBootCompleted })]
public class MyBootReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Work that should be done when the device boots.     
    }
}
```

Istnieje również możliwość utworzenia konwersji filtr, który będzie odpowiadał na profile niestandardowe. Rozważmy następujący przykład: 

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { "com.xamarin.example.TEST" })]
public class MySampleBroadcastReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here
    }
}
```

Aplikacje, które odnoszą się do 8.0 dla systemu Android (interfejs API na poziomie 26) lub nowszego mogą nie statycznie zarejestrować niejawne emisji. Aplikacje mogą nadal statycznie zarejestrować jawne emisji. Brak krótką listę niejawne programów, które są wykluczone z tego ograniczenia. Wyjątki te są opisane w [niejawne wyjątki emisji](https://developer.android.com/guide/components/broadcast-exceptions.html) przewodnik w dokumentacji systemu Android. Aplikacje, które są zainteresowane w niejawnych emisji należy wykonać, więc dynamicznie za pomocą `RegisterReceiver` metody. To jest opisane w dalszej części.

### <a name="context-registering-a-broadcast-receiver"></a>Kontekst zarejestrowanie odbiornika emisji

Kontekst Rejestracja (zwaną także dynamicznej rejestracji) odbiornik odbywa się za pomocą `RegisterReceiver` — metoda i emisji odbiornika musi być wyrejestrowany z wywołaniem do `UnregisterReceiver` metody. Aby zapobiec ulatniający zasobów, ważne jest wyrejestrować odbiornik, gdy nie jest już nieaktualny kontekstu (działania lub usługi). Na przykład usługa może emisji celem informują o tym działaniu dostępnych aktualizacji, który będzie wyświetlany użytkownikowi. Po uruchomieniu działania może zarejestrować się w tych opcji. Podczas działania jest przenoszony do tła i nie jest już widoczny dla użytkownika, jego należy wyrejestrować odbiornika ponieważ interfejsu użytkownika do wyświetlania widoku aktualizacji nie jest już widoczna. Poniższy fragment kodu przedstawiono przykładowy sposób rejestrowanie i wyrejestrowywanie odbiornik emisji w ramach działania:

```csharp
[Activity(Label = "MainActivity", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity: Activity 
{
    MySampleBroadcastReceiver receiver;

    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        receiver = new MySampleBroadcastReceiver()

        // Code omitted for clarity
    }

    protected override OnResume() 
    {
        base.OnResume();
        RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
        // Code omitted for clarity
    }

    protected override OnPause() 
    {
        UnregisterReceiver(receiver);
        // Code omitted for clarity
        base.OnPause();
    }
}
```

W poprzednim przykładzie, gdy działanie na pierwszym planie, zarejestruje emisji odbiornika, który będzie nasłuchiwać celem niestandardowych za pomocą `OnResume` metody cyklu życia. Podczas przenoszenia działania w tle, `OnPause()` metody wyrejestruje odbiornika.

## <a name="publishing-a-broadcast"></a>Publikowanie emisji

Wszystkie aplikacje zainstalowane na urządzeniu tworzenia obiektu konwersji i wysyła je z mogą być publikowane emisji `SendBroadcast` lub `SendOrderedBroadcast` metody.  

1. **Metody Context.SendBroadcast** &ndash; istnieje kilka implementacje tej metody.
   Tych metod będzie emisji zamiarem całego systemu. Odbiorniki emisji thatwill odbierać zamiar w nieokreślonej kolejności. To zapewnia dużą elastyczność, ale oznacza, że inne aplikacje mogą zarejestrować i odbierać celem. Może to stanowić zagrożenie bezpieczeństwa. Aplikacje muszą implementować dodanie zabezpieczeń, aby uniemożliwić nieautoryzowany dostęp. Możliwe rozwiązanie jest użycie `LocalBroadcastManager` która wyśle tylko komunikaty w prywatnej przestrzeni aplikacji. Przykład sposobu wysyłania przy użyciu jednej z celem jest ten fragment kodu `SendBroadcast` metod:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   SendBroadcast(intent);
   ```

    Ta Wstawka kodu jest inny przykład wysyła przy użyciu emisji `Intent.SetAction` metodę identyfikowania akcji:

    ```csharp 
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context.SendOrderedBroadcast** &ndash; jest metoda jest bardzo podobny do `Context.SendBroadcast`, z tą różnicą jest, że celem będzie jeden opublikowanych w czasie do odbiorców w kolejności, że zarejestrowano recievers.

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

[V4 Biblioteka obsługi Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) udostępnia klasę pomocy o nazwie [ `LocalBroadcastManager` ](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html). `LocalBroadcastManager` Jest przeznaczony dla aplikacji, których nie chcesz wysyłać ani odbierać emisje z innych aplikacji na urządzeniu. `LocalBroadcastManager` Będzie publikować tylko komunikaty w kontekście aplikacji i tylko te odbiorniki emisji, które są zarejestrowane w usłudze `LocalBroadcastManager`. Następujący fragment kodu jest przykład rejestrowania emisji odbiornika z `LocalBroadcastManager`:

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

Inne aplikacje na urządzeniu nie można odebrać wiadomości, które są publikowane z `LocalBroadcastManager`. Następujący fragment kodu przedstawia sposób wysłania konwersji przy użyciu `LocalBroadcastManager`:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
intent.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>Linki pokrewne

- [BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [Celem](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Lokalnego powiadomienia w systemie Android](~/android/app-fundamentals/notifications/local-notifications.md)
- [V4 biblioteki obsługi systemu android](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
