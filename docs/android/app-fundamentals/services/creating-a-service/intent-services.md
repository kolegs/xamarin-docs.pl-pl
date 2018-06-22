---
title: Konwersji usługi na platformie Xamarin.Android
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 80849213649707615f8bd8e941e1a51c6b54e76e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763345"
---
# <a name="intent-services-in-xamarinandroid"></a>Konwersji usługi na platformie Xamarin.Android

## <a name="intent-services-overview"></a>Omówienie usług konwersji

Jednocześnie uruchomiona i powiązane usługi, uruchom w głównym wątku, co oznacza, aby zachować wydajność smooth, wymagane przez usługę do wykonywania pracy asynchronicznie. Jednym z najprostszych sposobów rozwiązania tego problemu jest z _roboczych kolejki procesora wzorzec_, gdzie zadań do wykonania jest umieszczony w kolejce, które są obsługiwane przez pojedynczy wątek. 

[ `IntentService` ](https://developer.xamarin.com/api/type/Android.App.IntentService/) Jest podklasą `Service` klasy, która zapewnia Android konkretnej implementacji tego wzorca. Będą zarządzać Kolejkowanie pracy, uruchamianie wątku roboczego do kolejki, usługi i żądania ściągania wyłączone kolejki do uruchamiania w wątku roboczego. `IntentService` Ciche zatrzyma się i Usuń wątku roboczego po nie więcej pracy w kolejce.
 
Praca jest przesyłany do kolejki, tworząc `Intent` , a następnie przekazywanie, który `Intent` do `StartService` metody.

Nie można zatrzymać ani przerwań `OnHandleIntent` metody `IntentService` podczas pracy. Z tego powodu `IntentService` powinny być przechowywane bezstanowych &ndash; nie polegać na aktywne połączenie lub komunikat od pozostałej części aplikacji. `IntentService` Mają na celu statelessly przetwarzania żądań pracy.

Istnieją dwa wymagania dotyczące podklasy `IntentService`:

1. Nowy typ (utworzony przez podklasy `IntentService`) tylko te zastąpienia `OnHandleIntent` metody.
2. Konstruktor dla nowego typu wymaga ciąg, który jest używany do nazywania wątku roboczego, który będzie obsługiwać żądań. Nazwa tego wątku roboczego jest głównie używana podczas debugowania aplikacji.

Poniższy kod przedstawia `IntentService` implementację przesłoniętych `OnHandleIntent` metody:

```csharp
[Service]
public class DemoIntentService: IntentService
{
    public DemoIntentService () : base("DemoIntentService")
    {
    }
    
    protected override void OnHandleIntent (Android.Content.Intent intent)
    {
        Console.WriteLine ("perform some long running work");
        ...
        Console.WriteLine ("work complete");
    }
}
```

Pracy są wysyłane do `IntentService` przez utworzenie wystąpienia `Intent` i wywołując [ `StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/) metody z tym profilem jako parametr. Celem zostaną przekazane do usługi jako parametru w `OnHandleIntent` metody. Przykładem wysłanie żądania pracy celem jest następujący fragment kodu: 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

`IntentService` Można wyodrębnić wartości z celem, jak pokazano w poniższym przykładzie:  

```csharp
protected override void OnHandleIntent (Android.Content.Intent intent)
{
    string fileToDownload = intent.GetStringExtra("file_to_download");
    
    Log.Debug("DemoIntentService", $"File to download: {fileToDownload}.");
}
```


## <a name="related-links"></a>Linki pokrewne

- [IntentService](https://developer.xamarin.com/api/type/Android.App.IntentService/)
- [StartService](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)
