---
title: "Powiązane usługi platformie Xamarin.Android"
description: "Powiązane usługi są usługi dla systemu Android, które zapewniają interfejs klient serwer, który może interakcyjnie przeprowadzić klienta (na przykład działanie systemu Android). W tym przewodniku będzie omawiać najważniejsze składniki związane z tworzeniem powiązania usługi i jak z niego korzystać w aplikacji platformy Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: a299969e6251bcea59ea2ec52db90d59cf0461ad
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="bound-services-in-xamarinandroid"></a>Powiązane usługi platformie Xamarin.Android

_Powiązane usługi są usługi dla systemu Android, które zapewniają interfejs klient serwer, który może interakcyjnie przeprowadzić klienta (na przykład działanie systemu Android). W tym przewodniku będzie omawiać najważniejsze składniki związane z tworzeniem powiązania usługi i jak z niego korzystać w aplikacji platformy Xamarin.Android._

## <a name="bound-services-overview"></a>Powiązane usługi — omówienie

Usługi, które udostępniają interfejs klient serwer dla klientów do bezpośredniej interakcji z usługą są określane jako _powiązane usługi_.  Może to być wielu klientów podłączone do pojedynczego wystąpienia usługi, w tym samym czasie. Powiązania usługi i klienta są od siebie odizolowane. Zamiast tego systemu Android udostępnia szereg obiekty pośrednie zarządzających stan połączenia między nimi. Ten stan jest obsługiwana przez obiekt, który implementuje [ `Android.Content.IServiceConnection` ](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/) interfejsu.  Ten obiekt jest tworzony przez klienta i przekazany jako parametr [ `BindService` ](https://developer.xamarin.com/api/member/Android.Content.Context.BindService/) metody. `BindService` Jest dostępne na każdym [ `Android.Content.Context` ](https://developer.xamarin.com/api/type/Android.Content.Context) obiektu (na przykład działanie). Jest to żądanie do systemu operacyjnego Android do uruchamiania usługi i powiązania klienta. Istnieją trzy sposoby klient może zostać powiązany z usługi za pomocą `BindService` metody:

* **Obiekt wiążący usługi** &ndash; integratora usługi jest klasa implementująca [ `Android.OS.IBinder` ](https://developer.xamarin.com/api/type/Android.OS.IBinder) interfejsu. Większość aplikacji będzie implementuje ten interfejs bezpośrednio, zamiast tego zostanie rozszerzyć [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) klasy. Jest to najbardziej typowym podejściem, odpowiedni w przypadku gdy usługa i klient istnieje w tym samym procesie.
* **Przy użyciu programu Messenger** &ndash; ta technika jest odpowiedni w przypadku gdy usługa może istnieć w oddzielnych procesach. Zamiast tego żądania obsługi są skierowany między klientem a usługą za pomocą [ `Android.OS.Messenger` ](https://developer.xamarin.com/api/type/Android.OS.Messenger). [ `Android.OS.Handler` ](https://developer.xamarin.com/api/type/Android.OS.Handler) Jest tworzony w usługę, która będzie obsługiwać `Messenger` żądań. To zostanie omówiona w innym przewodniku.
* **Przy użyciu AIDL** &ndash; to uderzenia terrorystyczne w serca większość deweloperów systemu Android i nie będzie można uwzględnione w tym przewodniku.

Klient został powiązany z usługą, komunikacja między nimi po odbywa się za pośrednictwem `Android.OS.IBinder` obiektu.  Ten obiekt jest odpowiedzialny za interfejs, który umożliwi klientowi interakcji z usługą. Zawiera zestaw SDK systemu Android nie jest niezbędna dla każdej aplikacji platformy Xamarin.Android implementuje ten interfejs od początku, [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) klasy, która zajmuje się większość kodu wymagana z kierowanie obiektu między Klient i usługa.

Po zakończeniu klienta z usługą go musi usunąć powiązania z niego przez wywołanie metody `UnbindService` metody. Po ostatnim klienta ma niezwiązane z usługą, Android spowoduje zatrzymanie i zlikwidować powiązanej usługi.

Ten diagram przedstawia sposób działania, połączenia z usługą, integratora i usługi ze sobą powiązane:

![Diagram przedstawiający sposób składniki usługi odnoszą się do siebie](bound-services-images/bound-services-02.png "diagram przedstawiający sposób składniki usługi odnoszą się do siebie.")

W tym przewodniku będzie omawiać temat rozszerzyć `Service` klasy do zaimplementowania powiązanej usługi. Będzie również obejmowała implementacja `IServiceConnection` i rozszerzanie `Binder` umożliwia klientowi komunikowanie się z usługą. Przykładowa aplikacja dołączony tego przewodnika, które zawierają rozwiązania z pojedynczego projektu platformy Xamarin.Android o nazwie  **[BoundServiceDemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)**  . Jest to bardzo proste aplikacji, w której przedstawiono implementację usługi oraz powiązać działania. Powiązane usługi jest bardzo prosty interfejs API z tylko jedną metodę `GetFormattedTimestamp`, która zwraca ciąg, który informuje użytkownika, gdy usługa została uruchomiona i jak długo od on uruchomiony. Aplikacja umożliwia użytkownikowi ręcznie usuń powiązanie i ich powiązania z usługą.

[![Zrzut ekranu aplikacji uruchomionych na telefonie z systemem Android](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png)

## <a name="implementing-and-consuming-a-bound-service"></a>Wdrażanie i korzystanie z powiązania usługi

Istnieją trzy składniki, które muszą zostać zaimplementowane w kolejności dla aplikacji systemu Android do pracy z usługą powiązane:

1. **Rozszerzanie `Service` klasy i metody wywołania zwrotnego cyklu życia wdrożenia** &ndash; tej klasy będzie zawierać kod, który będzie wykonywania pracy, które będzie wymagane usługi. To zostanie omówiona bardziej szczegółowo poniżej.
2. **Utwórz klasę tego implementuje `IServiceConnection`**  &ndash; ten obiekt zawiera metody wywołania zwrotnego, które powiadamiają o klienta, gdy jest podłączone do (lub utracono połączenie) z usługi. Połączenie usługi zawiera również odwołanie do obiektu, który klient może używać do bezpośredniej interakcji z usługą. To odwołanie jest nazywany _integratora_.
3. **Utwórz klasę tego implementuje `IBinder`**  &ndash; A _integratora_ implementacja udostępnia interfejs API, który klient używa do komunikacji z usługą. Obiekt wiążący zapewniają albo odwołanie do powiązanej usługi, zezwalając metody do wywołania bezpośrednio lub integrator może zapewnić klienta interfejsu API, który hermetyzuje i ukrywa usługi powiązane z aplikacji. `IBinder` Podaj kod niezbędne dla zdalnych wywołań procedur. Nie jest konieczne (lub zalecaną) do zaimplementowania `IBinder` interfejsu bezpośrednio. `IBinder` Aplikacji zamiast tego należy rozszerzyć `Binder` zapewniające podstawowe funkcje wymagane przez większość `IBinder`.
4. **Uruchamianie i powiązanie z usługą** &ndash; połączenia z usługą, integratora i usługi od momentu utworzenia aplikacji systemu Android jest odpowiedzialna za uruchamianie usługi i powiązania jej.

Każdy z tych kroków zostanie dokładnie omówione w poniższych sekcjach szczegółowo.

### <a name="extend-the-service-class"></a>Rozszerzanie `Service` — klasa

Aby utworzyć usługę za pomocą platformy Xamarin.Android, jest niezbędne do podklasy `Service` i adorn klasy z [ `ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute). Ten atrybut jest używana przez narzędzia kompilacji platformy Xamarin.Android do poprawnie zarejestrować usługę w aplikacji **AndroidManifest.xml** pliku podobne jak w przypadku działania, powiązane usługa ma swoją cykl życia i wywołanie zwrotne metody związane z istotne zdarzenia w cyklu jego życia. Poniżej znajduje się przykład niektóre z najczęściej metod wywołania zwrotnego implementujących usługi:

* `OnCreate` &ndash; Ta metoda jest wywoływana przez system Android, jak jest uruchamianiu usługi. Służy do inicjowania zmiennych ani obiektów, które są wymagane przez usługę przez jego okres istnienia. Ta metoda jest opcjonalne.
* `OnBind` &ndash; Ta metoda musi być implementowana przez wszystkie usługi powiązane. Jest wywoływana, gdy pierwszy klient próbuje połączyć się z usługą. Zwraca wystąpienie `IBinder` , dzięki czemu klient może współpracować z usługą. Tak długo, jak usługa jest uruchomiona, `IBinder` obiektu będzie służyć do spełnienia żądania przyszłych klientów do powiązania z usługą.
* `OnUnbind` &ndash; Ta metoda jest wywoływana, gdy wszyscy klienci powiązania ma niezwiązane. Zwracając `true` z tej metody później wywoła usługę `OnRebind` z zamiarem przekazany do `OnUnbind` podczas wiązania nowych klientów do niego. Można to zrobić po usługi nadal uruchomiona po przeprowadzeniu niepowiązanych. Może się to zdarzyć, jeśli ostatnio niezwiązanego usługi zostały również uruchomiono usługi, i `StopService` lub `StopSelf` nie została wywołana. W takiej sytuacji `OnRebind` umożliwia opcje, które mają zostać pobrane. Zwraca domyślny `false` , które nie działają. Opcjonalny.
* `OnDestroy` &ndash; Ta metoda jest wywoływana, gdy Android jest niszczenie usługi. Niezbędne oczyszczania, takie jak zwolnienie zasobów, należy wykonać w ramach tej metody. Opcjonalny.

Zdarzenia cyklu życia klucza powiązanej usługi są wyświetlane na tym diagramie:

![Diagram przedstawiający kolejność, w którym są wywoływane metody cyklu życia](bound-services-images/bound-services-01.png "diagram przedstawiający kolejność, w którym są wywoływane metody cyklu życia.")

Poniższy fragment kodu z aplikacji pomocnika, dołączony w tym przewodniku pokazano, jak do zaimplementowania powiązanej usługi w Xamarin.Android:

```csharp
using Android.App;
using Android.Util;
using Android.Content;
using Android.OS;

namespace BoundServiceDemo
{
    [Service(Name="com.xamarin.ServicesDemo1")]
    public class TimestampService : Service, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampService).FullName;
        IGetTimestamp timestamper;

        public IBinder Binder { get; private set; }

        public override void OnCreate()
        {
            // This method is optional to implement
            base.OnCreate();
            Log.Debug(TAG, "OnCreate");
            timestamper = new UtcTimestamper();
        }

        public override IBinder OnBind(Intent intent)
        {
            // This method must always be implemented
            Log.Debug(TAG, "OnBind");
            this.Binder = new TimestampBinder(this);
            return this.Binder;
        }

        public override bool OnUnbind(Intent intent)
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnUnbind");
            return base.OnUnbind(intent);
        }

        public override void OnDestroy()
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnDestroy");
            Binder = null;
            timestamper = null;
            base.OnDestroy();
        }

        /// <summary>
        /// This method will return a formatted timestamp to the client.
        /// </summary>
        /// <returns>A string that details what time the service started and how long it has been running.</returns>
        public string GetFormattedTimestamp()
        {
            return timestamper?.GetFormattedTimestamp();
        }
    }
}
```

W przykładzie `OnCreate` metoda inicjuje obiekt przechowujący logikę pobierania i formatowanie sygnaturę czasową żądany przez klienta. Gdy pierwszy klient podejmie próbę utworzenia powiązania z usługą, Android wywoła `OnBind` metody. Ta usługa zostanie utworzenie wystąpienia `TimestampServiceBinder` obiektów, które umożliwiają klientom dostęp do tego wystąpienia uruchomionej usługi. `TimestampServiceBinder` Klasa została szczegółowo opisana w następnej sekcji.

### <a name="implementing-ibinder"></a>Implementowanie IBinder

Jak wspomniano, `IBinder` obiektu udostępnia kanał komunikacji między klientem a usługą. Aplikacje systemu android nie powinny implementować `IBinder` interfejsu [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder/) powinien zostać rozszerzony. `Binder` Klasa udostępnia znacznie niezbędne infrastruktury, która jest konieczne kierowanie obiekt integratora z usługi (która może działać w osobnych procesach) do klienta. W większości przypadków `Binder` podklasy jest tylko kilka wierszy kodu i opakowuje odwołanie do usługi. W tym przykładzie `TimestampBinder` ma właściwość, która przedstawia `TimestampService` do klienta:

```csharp
public class TimestampBinder : Binder
{
    public TimestampBinder(TimestampService service)
    {
        this.Service = service;
    }

    public TimestampService Service { get; private set; }
}
```

To `Binder` umożliwia wywołanie metody publiczne usługi; na przykład:

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

Raz `Binder` został rozszerzony, jest niezbędne do zaimplementowania `IServiceConnection` do łączenia wszystko.

### <a name="creating-the-service-connection"></a>Tworzenie połączenia z usługą

`IServiceConnection` Przedstawi | wprowadzenie | ujawnia | połączyć `Binder` obiektu do klienta. Oprócz wykonania `IServiceConnection` interfejsu klasy muszą być rozszerzane `Java.Lang.Object`. Połączenia z usługą określić inny sposób, w których klient może uzyskać dostęp `Binder` (i w związku z tym komunikować się z usługą powiązane).

Ten kod jest z towarzyszącego przykładowy projekt jest możliwy sposób implementowania `IServiceConnection`:

```csharp
using Android.Util;
using Android.OS;
using Android.Content;

namespace BoundServiceDemo
{
    public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampServiceConnection).FullName;

        MainActivity mainActivity;
        public TimestampServiceConnection(MainActivity activity)
        {
            IsConnected = false;
            Binder = null;
            mainActivity = activity;
        }

        public bool IsConnected { get; private set; }
        public TimestampBinder Binder { get; private set; }

        public void OnServiceConnected(ComponentName name, IBinder service)
        {
            Binder = service as TimestampBinder;
            IsConnected = this.Binder != null;

            string message = "onServiceConnected - ";
            Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

            if (IsConnected)
            {
                message = message + " bound to service " + name.ClassName;
                mainActivity.UpdateUiForBoundService();
            }
            else
            {
                message = message + " not bound to service " + name.ClassName;
                mainActivity.UpdateUiForUnboundService();
            }

            Log.Info(TAG, message);
            mainActivity.timestampMessageTextView.Text = message;

        }

        public void OnServiceDisconnected(ComponentName name)
        {
            Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
            IsConnected = false;
            Binder = null;
            mainActivity.UpdateUiForUnboundService();
        }

        public string GetFormattedTimestamp()
        {
            if (!IsConnected)
            {
                return null;
            }

            return Binder?.GetFormattedTimestamp();
        }
    }
}

```

W ramach procesu powiązania, wywoła Android `OnServiceConnected` metody, zapewniając `name` usługi, która jest związany i `binder` przechowuje odwołanie do samej usługi. W tym przykładzie połączenia z usługą ma dwie właściwości, który zawiera odwołanie do obiektu wiążącego i Flaga wartości logicznej dla, jeśli klient jest połączony z usługą, czy nie.

`OnServiceDisconnected` — Metoda jest wywoływana tylko wtedy, gdy połączenie między klientem a usługą nieoczekiwanie zostanie utracony lub uszkodzony. Ta metoda umożliwia klientowi możliwość odpowiadanie na czas przestoju usługi.  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>Uruchamianie i powiązanie z usługą z zamiarem jawne

Aby użyć powiązane usługi, klienta (na przykład działanie) musi utworzyć wystąpienia obiektu implementującego `Android.Content.IServiceConnection` i wywoływać `BindService` metody.` BindService` Zwraca `true` Jeśli usługa jest powiązany, `false` Jeśli nie jest. `BindService` Metoda przyjmuje trzy parametry:

* **`Intent`**  &ndash; Zamiar jawnie należy zidentyfikować usługę do nawiązania połączenia.
* **`IServiceConnection` Obiektu** &ndash; ten obiekt jest pośrednik udostępniający metody wywołania zwrotnego, aby powiadomić klienta podczas uruchamiania i zatrzymywania powiązanej usługi.
* **[`Android.Content.Bind`](https://developer.xamarin.com/api/type/Android.Content.Bind/) wyliczenia** &ndash; tego parametru jest zestaw flag są używane przez system podczas powiązać obiektu. Wartość najczęściej używane jest [ `Bind.AutoCreate` ](https://developer.xamarin.com/api/field/Android.Content.Bind.AutoCreate/), które zostaną automatycznie uruchomione usługi, jeśli nie jest już uruchomiony.

Poniższy fragment kodu przedstawiono przykładowy sposób uruchamiania powiązanej usługi w działaniu za pomocą jawnego celem:

```csharp
protected override void OnStart ()
{
    base.OnStart ();

    if (serviceConnection == null)
    {
        this.serviceConnection = new TimestampServiceConnection(this);
    }

    Intent serviceToStart = new Intent(this, typeof(TimestampService));
    BindService(serviceToStart, this.serviceConnection, Bind.AutoCreate);

}
```

> [!IMPORTANT]
> Uruchamianie z systemem Android 5.0 (poziom interfejsu API 21) jest tylko możliwe powiązanie z usługą z zamiarem jawnego.

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>Architektury uwagi dotyczące połączenia z usługą i obiekt wiążący.

Niektóre purists Obiektowo może odrzucić poprzedniego wdrożenia `TimestampBinder` klasy, ponieważ jest to naruszenie [prawa Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) , w jego najprostszej formie Stany "nie komunikował się obcymi; tylko zwróć się do Twoich znajomych". Tej konkretnej implementacji przedstawia konkretnych `TimestampService` klasy do wszystkich klientów.

Mówiąc ściślej, nie jest niezbędna dla klienta wiedzieć o `TimestampService` i udostępnianie konkretnych klas klientom może wykonywać aplikacji bardziej łamliwa i trudniej Obsługa okresu jego istnienia. Alternatywne rozwiązaniem jest użycie interfejsu, który opisuje `GetFormattedTimestamp()` — metoda i wywołania usługi za pośrednictwem serwera proxy `Binder` (lub możliwe klasy połączenia usługi):  

```csharp
public class TimestampBinder : Binder, IGetTimesamp
{
    TimestampService service;
    public TimestampBinder(TimestampService service)
    {
        this.service = service;
    }

    public string GetFormattedTimestamp()
    {
        return service?.GetFormattedTimestamp();
    }
}
```

Zezwalaj na tym przykładzie działanie, aby wywołać metod w samej usługi:

```csharp
// In this example the Activity is only talking to a friend, i.e. the IGetTimestamp interface provided by the Binder.
string currentTimestamp = serviceConnection.Binder.GetFormattedTimestamp()
```


## <a name="related-links"></a>Linki pokrewne

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.Content.Bind](https://developer.xamarin.com/api/type/Android.Content.Bind/)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [Android.Content.IServiceConnection](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/)
- [Android.OS.Binder](https://developer.xamarin.com/api/type/Android.OS.Binder/)
- [Android.OS.IBinder](https://developer.xamarin.com/api/type/Android.OS.IBinder)
- [BoundServiceDemo (przykład)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/BoundServiceDemo/)
