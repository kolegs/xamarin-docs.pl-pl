---
title: Odbiornik emisji w platformy Xamarin.Android
description: "W tej sekcji omówiono sposób użycia odbiornik emisji."
ms.topic: article
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 7ea280e11e4051b51da8c20eb9ac5a4e298547f3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="broadcast-receiver-in-xamarinandroid"></a>Odbiornik emisji w platformy Xamarin.Android

_W tej sekcji omówiono sposób użycia odbiornik emisji._


## <a name="broadcast-receiver-overview"></a>Omówienie odbiornika emisji

A _emisji odbiornika_ jest składnikiem systemu Android umożliwiający aplikacji odpowiada na komunikaty (Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)) rozgłaszanych przez system operacyjny Android lub przez aplikację. Wykonaj emisje _publikowania / subskrypcji_ modelu &ndash; zdarzenia powoduje emisji zostanie opublikowany i odbierane przez te składniki, które są zainteresowane w zdarzeniu. 

Android identyfikuje dwie kategorie emisji:

* **Normalne emisji** &ndash; normalne emisji będą kierowane do wszystkich zarejestrowanych odbiorców emisji w nieokreślonej kolejności. Każdy odbiorca otrzyma zamiar niezdefiniowaną kolejność. 
* **Uporządkowane emisji** &ndash; uporządkowanej emisji jest dostarczany pojedynczo do zarejestrowanych odbiorców. Po odebraniu celem emisji odbiornika można zmodyfikować celem lub jego zakończenie emitowania.

Odbiornik emisji jest podklasą `BroadcastReceiver` przesłonięcie klasy i [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/) metody. Android zostanie wykonany `OnReceive` w głównym wątku, więc tej metody należy tak zaprojektować wykonać szybko. Należy zwrócić uwagę, gdy dochodzi do uruchamiania wątków w `OnReceive` ponieważ Android może zakończyć proces po zakończeniu metody. Jeśli odbiornik emisji należy wykonać długotrwała pracy, zaleca się zaplanowanie _zadania_ przy użyciu `JobScheduler` lub _dyspozytora zadania Firebase_. Planowanie pracy z zadaniem zostaną dokładniej omówione w przewodniku oddzielne.

_Konwersji filtru_ służy do rejestrowania emisji odbiornika Android prawidłowo rozesłać komunikaty. Filtr konwersji można określić w czasie wykonywania (jest to czasami określane jako _kontekst zarejestrowany odbiornik_ lub jako _dynamicznej rejestracji_) lub może być statycznie zdefiniowany w ( manifestusystemuAndroid_zarejestrowany manifestu odbiorcy_). Xamarin.Android zapewnia C# atrybutu, `IntentFilterAttribute`, który zarejestruje statycznie filtr konwersji (to zostanie dokładnie omówione w dalszej części tego przewodnika). 

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

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>Statycznie rejestrowanie emisji odbiornika konwersji filtru

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

### <a name="context-registering-a-broadcast-receiver"></a>Kontekst zarejestrowanie odbiornika emisji 

Rejestracja kontekstu odbiornika odbywa się za pomocą `RegisterReceiver` — metoda i emisji odbiornika musi być wyrejestrowany z wywołaniem do `UnregisterReceiver` metody. Zapobiegaj ulatniający zasobów, jest wyrejestrować odbiornik, gdy nie jest już nieaktualny do kontekstu. Na przykład usługa może emisji celem informują o tym działaniu dostępnych aktualizacji, który będzie wyświetlany użytkownikowi. Po uruchomieniu działania może zarejestrować się w tych opcji. Podczas działania jest przenoszony do tła i nie jest już widoczny dla użytkownika, jego należy wyrejestrować odbiornika ponieważ interfejsu użytkownika do wyświetlania widoku aktualizacji nie jest już widoczna. Poniższy fragment kodu przedstawiono przykładowy sposób rejestrowanie i wyrejestrowywanie odbiornik emisji w ramach działania:

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

Emisja jest publikowany hermetyzując _akcji_ celem i wysyła je z jedną z dwóch interfejsów API: 

1. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; Opcje, które są publikowane z `LocalBroadcastManager` tylko zostanie odebrana przez aplikacji, nie będą kierowane do innych aplikacji. Powinno to być preferowane, przechowując intencje w bieżącej aplikacji zapewnia dodatkowy poziom zabezpieczeń i wszystko jest w trakcie więc nie istnieje żadne obciążenia związanego z wywołań między procesami. Następujący fragment kodu przedstawia sposób działania może wysłać konwersji przy użyciu `LocalBroadcastManager`:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
   ```

2. **[`Context.SendBroadcast`](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/) metody** &ndash; istnieje kilka implementacje tej metody.
   Tych metod będzie emisji zamiarem całego systemu. To zapewnia dużą elastyczność, ale oznacza, że inne aplikacje mogą zarejestrować do odbierania aplikacji. Może to stanowić zagrożenie bezpieczeństwa. Aplikacje może być konieczne dodanie zabezpieczeń, aby zapewnić nieautoryzowanego dostępu do celem wdrożenia. Przykład sposobu wysyłania przy użyciu jednej z celem jest ten fragment kodu `SendBroadcast` metod:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   SendBroadcast(intent);
   ```
        
> [!NOTE]
> Jest dostępna za pośrednictwem LocalBroadcastManager [v4 Biblioteka obsługi Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) pakietu NuGet. 

Ta Wstawka kodu jest inny przykład wysyła przy użyciu emisji `Intent.SetAction` metodę identyfikowania akcji:

```csharp 
Intent intent = new Intent();
intent.SetAction("com.xamarin.example.TEST");
intent.PutExtra("key", "value");
SendBroadcast(intent);
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
