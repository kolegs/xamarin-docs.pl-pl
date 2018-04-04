---
title: Tworzenie usługi
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/01/2018
ms.openlocfilehash: d1e0fdb1c4b159b6db283d7b9b3be673b73a0ee0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-service"></a>Tworzenie usługi

Usługi platformy Xamarin.Android muszą przestrzegać dwie reguły nienaruszalności usług dla systemu Android:

* Muszą one być rozszerzane [ `Android.App.Service` ](https://developer.xamarin.com/api/type/Android.App.Service/).
* Musi być dekorowane za [ `Android.App.ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/).

Innym wymogiem Android usług jest musi być zarejestrowana w **AndroidManifest.xml** i nadać unikatową nazwę. Xamarin.Android automatycznie zarejestrować usługi w manifeście w czasie kompilacji z niezbędne atrybutu XML.

Następujący fragment kodu jest najprostsza przykładem tworzenie platformie Xamarin.Android, który spełnia wymagania dwie usługi:  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

W czasie kompilacji platformy Xamarin.Android zarejestruje usługi przez wstrzykiwanie następujących elementów XML w **AndroidManifest.xml** (powiadomień platformy Xamarin.Android wygenerowania losową nazwę usługi):

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

Można udostępniać usługi inne aplikacje systemu Android przez _eksportowanie_ go. Jest to osiągane przez ustawienie `Exported` właściwość `ServiceAttribute`. Podczas eksportowania usługa `ServiceAttribute.Name` właściwości powinien również być ustawiona na Podaj opisową nazwę publiczną dla usługi. Ta Wstawka kodu pokazano, jak wyeksportować i nazwa usługi:

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

**AndroidManifest.xml** element dla tej usługi następnie będzie wyglądać mniej więcej tak:

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

Usługi mają technicznego z metody wywołania zwrotnego, które są wywoływane przy tworzeniu usługi. Dokładnie które metody są wywoływane zależy od typu usługi. Uruchomiono usługi musi implementować cyklu życia różnych metod niż powiązane usługi, gdy usługa hybrydowego musi implementować metody wywołania zwrotnego dla uruchomiono usługi i powiązane usługi. Te metody są wszystkie elementy członkowskie `Service` klasy; sposób uruchamiania usługi określają, jakie metody cyklu życia zostanie wywołany. Te metody cyklu życia zostanie omówiona bardziej szczegółowo później.

Domyślnie usługa zostanie uruchomiona w tym samym procesie jako aplikacji systemu Android. Istnieje możliwość uruchomić usługę w swoim własnym procesie, ustawiając `ServiceAttribute.IsolatedProcess` właściwości na wartość true:

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

Następnym krokiem jest zbadanie jak uruchomić usługę, a następnie przenieść zbadanie implementowania trzy różne typy usług.

> [!NOTE]
> Usługa jest uruchomiona w wątku interfejsu użytkownika, aby w przypadku każdej pracy należy wykonać, który blokuje interfejsu użytkownika, usługa musi używać wątków do wykonywania pracy.

## <a name="starting-a-service"></a>Uruchomienie usługi

Najbardziej podstawową metodą, aby uruchomić usługę w systemie Android jest wysłanie `Intent` zawierający metadane do identyfikowania usługi, która powinna być uruchamiana. Istnieją dwa różne style lokalizacji docelowych, które mogą służyć do uruchamiania usługi:

-   **Jawne zamiar** &ndash; _jawne zamiar_ określi dokładnie usługi, które mają służyć do wykonania danej akcji. Jawne celem można traktować jako literą, która ma określonego adresu; Android będzie przekierowywać celem użycia jej do usługi, które jawnie określono. Ta Wstawka kodu jest jednym z przykładów przy użyciu jawnych celem można uruchomić usługi o nazwie `DownloadService`:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

-   **Niejawne zamiar** &ndash; tego typu założeń słabo identyfikuje akcji która powinna być wykonywana, ale dokładne usługi do ukończenia tego działania jest nieznany. Niejawne celem można traktować jako literą, która jest skierowana "Do Whom It może dotyczą...".
    Android zbada zawartość zamiar i determin przypadku istniejącą usługę, której celem jest zgodna.

    _Konwersji filtru_ służą do niejawnego pozostała z zarejestrowanej usługi. Filtr konwersji jest element XML, który jest dodawany do **AndroidManifest.xml** zawierającą wymagane metadane do wyszukiwania usługi z zamiarem niejawne.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Jeśli dla systemu Android ma więcej niż jedno dopasowanie możliwa do niejawnego celem, jego monitowanie użytkownika o wybierz składnik do obsługi akcji:

![Zrzut ekranu przedstawiający okno dialogowe Uściślanie](images/creating-a-service-01.png "zrzut ekranu przedstawiający okno dialogowe Uściślanie")

> [!IMPORTANT]
> Począwszy od systemu Android 5.0 (poziom Region 21) niejawne celem nie może służyć do uruchamiania tej usługi.

Jeśli to możliwe, aplikacje powinny używać jawne intencje uruchamiania tej usługi. Niejawne celem nie pytaj o określonej usługi uruchomić &ndash; jest żądania niektóre usługi zainstalowany na urządzeniu do obsługi żądania. Niejednoznaczne żądanie może spowodować niewłaściwej usługi obsługi żądania lub innej aplikacji niepotrzebnie uruchamiania (co zwiększa wykorzystanie zasobów na urządzeniu).

Jak wysyłane jest celem zależy od typu usługi i zostanie omówiona bardziej szczegółowo w dalszej części przewodniki określonych dla każdego typu usługi.


### <a name="creating-an-intent-filter-for-implicit-intents"></a>Tworzenie konwersji filtru dla niejawnej konwersji kolorów

Aby skojarzyć usługi z zamiarem niejawne, aplikację systemu Android podać niektóre metadane do identyfikowania możliwości usługi. Meta dane te są udostępniane przez _konwersji filtry_. Filtry konwersji zawiera pewne informacje, takie jak akcję lub typ danych, który musi znajdować się w opcje, aby uruchomić usługę. Platformie Xamarin.Android, konwersji filtr jest zarejestrowany w **AndroidManifest.xml** przez dekoracji usługi za pomocą [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Na przykład następujący kod dodaje filtr konwersji z akcją skojarzone `com.xamarin.DemoService`:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

Powoduje to wpis dołączaniu **AndroidManifest.xml** pliku &ndash; wpis z aplikacji w sposób analogiczny do poniższego przykładu:

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Podstawy usługi platformy Xamarin.Android przeszkadza Przeanalizujmy różnych podtypów usługi bardziej szczegółowo.


## <a name="related-links"></a>Linki pokrewne

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)
- [Android.App.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
