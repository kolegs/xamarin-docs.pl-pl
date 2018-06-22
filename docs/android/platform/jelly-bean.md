---
title: Funkcje ziarna galaretki
description: 'Ten dokument zapewni wysokiego poziomu omówienie nowych funkcji dla deweloperów, które zostały wprowadzone w systemie Android 4.1. Te funkcje obejmują: powiadomienia, aktualizacji systemu Android światła udostępnianie duże pliki, aktualizacje do odnajdywania sieci multimedia, peer-to-peer, animacji, nowe uprawnienia rozszerzone.'
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 1d8068ccfc8d0f159a88704370261ec5f20d8b7c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771554"
---
# <a name="jelly-bean-features"></a>Funkcje ziarna galaretki

_Ten dokument zapewni wysokiego poziomu omówienie nowych funkcji dla deweloperów, które zostały wprowadzone w systemie Android 4.1. Te funkcje obejmują: powiadomienia, aktualizacji systemu Android światła udostępnianie duże pliki, aktualizacje do odnajdywania sieci multimedia, peer-to-peer, animacji, nowe uprawnienia rozszerzone._



## <a name="overview"></a>Omówienie

Android 4.1 (16 poziom interfejsu API), nazywane również "galaretki ziarna", została wersji 9 lipca 2012. W tym artykule zapewni wysokiego poziomu wprowadzenie do pewnych nowych funkcji w systemie Android 4.1 dla deweloperów korzystających z platformy Xamarin.Android. Niektóre z tych nowych funkcji wprowadzonych są ulepszenia animacje uruchomienie działania, nowe dźwięki dla aparatu i lepszą obsługę aplikacji stos nawigacji. Teraz istnieje możliwość wycinanie i wklejanie w lokalizacji docelowych.

Większa stabilność aplikacji systemu Android umożliwia izolowanie zależność od niestabilny dostawców zawartości. Usługi także mogą być izolowane, aby były dostępne tylko dla działania, które je uruchomić.

Dodano obsługę dla odnajdywania usług sieci za pomocą funkcji Bonjour, architektury UPnP lub multiemisji DNS na podstawie usług. Teraz istnieje możliwość powiadomień bardziej zaawansowane funkcje, które sformatowane tekstu, przycisków akcji i duże obrazy.

Na koniec kilka nowe uprawnienia zostały dodane w systemie Android 4.1.



## <a name="requirements"></a>Wymagania

Do tworzenia aplikacji platformy Xamarin.Android przy użyciu ziarna galaretki wymaga platformy Xamarin.Android 4.2.6 lub nowszej i Android 4.1 (16 poziom interfejsu API) można zainstalować za pomocą Android SDK Manager, jak pokazano na poniższym zrzucie ekranu:

[![Wybieranie Android 4.1 w Menedżerze zestawu SDK systemu Android](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)



## <a name="whats-new"></a>Nowości



### <a name="animations"></a>Animacji

Działania mogą zostać uruchomione przy użyciu animacje powiększenia lub animacji niestandardowej `ActivityOptions` klasy. Dostępne są następujące nowe metody do obsługi tych animacji:

-   `MakeScaleUpAnimation` — To utworzy animacji, która może obsłużyć okno działanie od pozycji początkowej i rozmiar na ekranie.
-   `MakeThumbnailScaleUpAnimation` — To utworzy animacji, która może obsłużyć z obraz miniatury od określonej pozycji na ekranie.
-   `MakeCustomAnimation` Animacja — tworzy z zasobów w aplikacji. Istnieje jeden animację po otwarciu działania i innego momentu zatrzymania działania.


Nowy `TimeAnimator` klasa udostępnia interfejsem `TimeAnimator.ITimeListener` który powiadomić aplikację, każdej zmianie ramki animacji. Na przykład, należy wziąć pod uwagę następujące wykonania `TimeAnimator.ITimeListener`:

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

I teraz można użyć klasy wystąpienia `TimeAnimator` jest tworzony, i ustawiono odbiornika:

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

Jako `TimeAnimator` jest uruchomione wystąpienie, jego wywoła `ITimeAnimator.ITimeListener`, który następnie będzie rejestrować jak długo zresztą została uruchomiona i jak długo go jako zostały od czasu ostatniego metoda została wywołana.



### <a name="application-stack-navigation"></a>Aplikacja stos nawigacji

Android 4.1 poprawia w nawigacji stosu aplikacji, która została wprowadzona w systemie Android 3.0. Określając `ParentName` właściwość `ActivityAttribute`, Android można otworzyć prawidłowego działania nadrzędnego, gdy użytkownik naciśnie [przycisk](http://developer.android.com/design/patterns/navigation.html#up-vs-back) na pasku akcji - Android zostanie wystąpienia działania określony przez `ParentName`właściwości. Dzięki temu aplikacje mogą zachować hierarchii działań, które należy danego zadania.

Dla większości aplikacji ustawienie `ParentName` w działaniu jest wystarczających informacji dla systemu Android zapewnić prawidłowe działanie do nawigowania stosu aplikacji; Android zostanie syntetyzowania niezbędne stosie przechodzenia wstecz, tworząc szereg opcji dla każdego działania nadrzędnego. Jednak ponieważ jest to stosu sztuczne aplikacji, każde działanie syntetycznych nie będzie miał zapisany stan, które byłyby fizycznych działania. Zapewnienie zapisany stan działania nadrzędnego syntetycznych, działania mogą zastąpić `OnPrepareNavigationUpTaskStack` metody. Ta metoda `TaskStackBuilder` wystąpienie, które będą miały kolekcję założeń obiektów, że Android będzie używać do tworzenia stosie przechodzenia wstecz. Działania mogą być modyfikowane tych opcji, aby przy tworzeniu syntetycznych działania zostanie wyświetlony informacje prawidłowego stanu.

Aby uzyskać bardziej złożonymi scenariuszami istnieją nowych metod w klasie działania, które mogą być używane do obsługi zachowanie się nawigacji i utworzyć stosie przechodzenia wstecz:

-   `OnNavigateUp` -Przez zastąpienie tej metody jest możliwość wykonywania akcji niestandardowej podczas <span class="ui">się</span> przycisk jest naciśnięty.
-   `NavigateUpTo` — Wywołaniem tej metody spowoduje, że aplikację można przejść z bieżącego działania określony przez dany celem.
-   `ParentActivityIntent` — Służy do uzyskania zamiar, która spowoduje uruchomienie działania nadrzędnego bieżącego działania.
-   `ShouldUpRecreateTask` — Ta metoda służy do zapytania, jeśli syntetycznych stosie przechodzenia wstecz musi zostać utworzony, można przejść do działania nadrzędnego. Zwraca `true` Jeśli syntetycznych stos musi zostać utworzony. 
-   `FinishAffinity` — Wywołaniem tej metody zostaną ukończone bieżące działanie i wszystkie działania poniżej w bieżącym zadaniu mających koligację z tego samego zadania.
-   `OnCreateNavigateUpTaskStack` — Ta metoda zostanie przesłonięta, gdy jest konieczne mają pełną kontrolę nad tworzenia syntetycznych stosu.




### <a name="camera"></a>Aparat fotograficzny

Dostępny jest nowy interfejs, `Camera.IAutoFocusMoveCallback`, które mogą służyć do wykrycia, gdy fokus automatycznie rozpoczął się lub przenoszenie. Przykład nowy interfejs są widoczne w poniższy fragment kodu:

```csharp
public class AutoFocusCallbackActivity : Activity, Camera.IAutoFocusCallback
{
    public void OnAutoFocus(bool success, Camera camera)
    {
        // camera is an instance of the camera service object.

        if (success)
        {
            // Auto focus was successful - do something here.
        }
        else
        {
            // Auto focus didn't happen for some reason - react to that here.
        }
    }
}
```

Nowa klasa `MediaActionSound` zawiera zestaw interfejsów API do produkcji dźwięków dla różnych działań nośnika. Kilka czynności, które mogą wystąpić z aparatu, są one zdefiniowane przez wyliczenia `Android.Media.MediaActionSoundType`:

-   `MediaActionSoundType.FocusComplete` — Ten dźwięk jest odtwarzany po ukończeniu koncentrujących się.
-   `MediaActionSoundType.ShutterClick` — Ten dźwięk odtwarzania po podjęciu zdjęcia obrazu.
-   `MediaActionSoundType.StartVideoRecording` — Ten dźwięk jest używana wskazują początek nagrywanie wideo.
-   `MediaActionSoundType.StopVideoRecording` — Ten dźwięk odtwarzania do wskazywania końca nagrywanie wideo.


Przykład sposobu użycia `MediaActionSound` klasy są widoczne w poniższy fragment kodu:

```csharp
var mediaActionPlayer = new MediaActionSound();

// Preload the sound for a shutter click.
mediaActionPlayer.Load(MediaActionSoundType.ShutterClick);
var button = FindViewById<Button>(Resource.Id.MyButton);

// Play the sound on a button click.
button.Click += (sender, args) => mediaActionPlayer.Play(MediaActionSoundType.ShutterClick);

// This releases the preloaded resources. Don’t make any calls on
// mediaActionPlayer after this.
mediaActionPlayer.Release();
```



### <a name="connectivity"></a>Łączność



#### <a name="android-beam"></a>Android Beam

Android światła to technologia NFC na podstawie, która umożliwia dwa urządzenia z systemem Android do komunikowania się ze sobą. Android 4.1 oferuje lepszą obsługę transferu dużych plików. Korzystając z nowej metody `NfcAdapter.SetBeamPushUris()` Android będzie przełączać się między mechanizmów transportu alternatywne (takie jak połączenia Bluetooth) do uzyskania szybkości szybkiego transferu.



#### <a name="network-services-discovery"></a>Odnajdowanie usługi

Android 4.1 zawiera nowego interfejsu API dla potrzeb odnajdywania usług DNS na podstawie multiemisji.
Dzięki temu aplikacja do wykrywania i łączyć się za pośrednictwem sieci Wi-Fi z innymi urządzeniami, takich jak drukarki, aparaty fotograficzne i nośnika. Te nowe API znajdują się w `Android.Net.Nsd` pakietu.

Aby utworzyć usługę, która może być używane przez inne usługi `NsdServiceInfo` klasa jest używana do utworzenia obiektu, który będzie definiował właściwości usługi. Ten obiekt jest następnie dostarczany w celu `NsdManager.RegisterService()` wraz z implementacją `NsdManager.ResolveListener`. Implementacje `NsdManager.ResolveListener` są używane do powiadamiania o pomyślnej rejestracji i wyrejestrowania usługi.

Do odnajdywania usług sieci i stosowania `Nsd.DiscoveryListener` przekazany do `NsdManager.discoverServices()`.



#### <a name="network-usage"></a>Użycie sieci

Nowa metoda `ConnectivityManager.IsActiveNetworkMetered` umożliwia urządzenia sprawdzić, czy jest dołączone do sieci taryfowej. Ta metoda może służyć pomagające w zarządzaniu wykorzystanie danych przez dokładnie informowania użytkowników czy może być kosztowne opłat za operacje na danych.



#### <a name="wifi-direct-service-discovery"></a>Odnajdowanie usługi sieci Wi-Fi Direct

`WifiP2pManager` Klasy została wprowadzona w systemie Android 4.0 do obsługi *zeroconf*. Zeroconf (zero sieci konfiguracji) to zestaw metod umożliwiający urządzeniom (komputery, drukarki, telefony) do łączenia się z sieciami automatycznie, z interwencji Operatorzy sieci człowieka lub specjalnej konfiguracji serwerów.

W ziarna galaretki `WifiP2pManager` można wykrywanie pobliskich urządzeń przy użyciu *Bonjour* lub *Upnp*. Bonjour to implementacja firmy Apple zeroconf. UPnP to zestaw protokołów sieciowych, które obsługuje również zeroconf. Następujące metody dodane do `WiFiP2pManager` do obsługi sieci Wi-Fi usługi odnajdywania:

-   `AddLocalService()` — Ta metoda jest używana ogłaszamy aplikacji jako usługi za pośrednictwem sieci Wi-Fi odnajdywane przez elementy równorzędne.
-   `AddServiceRequest(` ) — Ta metoda jest wysłanie żądania odnajdywania usługi do struktury. Służy do inicjowania odnajdywania usług sieci Wi-Fi.
-   `SetDnsSdResponseListeners()` — Ta metoda służy do rejestrowania wywołań zwrotnych do wywołania po otrzymaniu odpowiedzi na żądania odnajdywania z Bonjour.
-   `SetUpnpServiceResponseListener()` — Ta metoda służy do rejestrowania wywołań zwrotnych do wywołania po otrzymaniu odpowiedzi na żądania odnajdywania Upnp.




### <a name="content-providers"></a>Dostawców zawartości

`ContentResolver` Klasy otrzymała nową metodę `AcquireUnstableContentProvider`. Ta metoda umożliwia aplikacji w celu uzyskania "niestabilny" dostawcy zawartości. Zwykle gdy aplikacja uzyskuje dostawcy zawartości, tego dostawcy zawartości ulegnie awarii, więc zostanie aplikacji. Aplikacja z wywołanie tej metody nie ulegnie awarii, jeśli wystąpiła awaria dostawcy zawartości. Zamiast tego `Android.OS.DeadObjectionException` zostanie zgłoszony z wywołań w dostawcy zawartości do informowania aplikacja dostawcy zawartości przeszedł do optymalizacji. "Niestabilny" dostawcy zawartości jest przydatne, gdy interakcji z dostawców zawartości z innych aplikacji — jest mniej prawdopodobne, że buggy kod z innej aplikacji będzie miało wpływ na inną aplikację.



### <a name="copy-and-paste-with-intents"></a>Kopiowanie i wklejanie w lokalizacji docelowych

`Intent` Klasa teraz może mieć `ClipData` obiekt skojarzony z nim za pośrednictwem `Intent.ClipData` właściwości. Ta metoda umożliwia dodatkowe dane ze Schowka są przesyłane przy użyciu zamiaru. Wystąpienie `ClipData` może zawierać jeden lub więcej `ClipData.Item`. `ClipData.Item`dla elementów z następujących typów:

-   **Tekst** — to jest dowolny ciąg tekstu, albo HTML lub obejmuje dowolny ciąg, którego format jest obsługiwany przez wbudowany styl systemu Android.
-  **Celem** — wszystkie `Intent` obiektu.
-   **Identyfikator URI** — może to być dowolny identyfikator URI, takich jak HTTP zakładki lub identyfikator URI do dostawcy zawartości.




### <a name="isolated-services"></a>Izolowane usługi

Izolowane usługi to usługa, która zostanie uruchomiona w ramach własnej specjalny proces i nie ma uprawnień własnych. Tylko komunikacja z usługą jest podczas uruchamiania usługi i powiązanie go za pomocą interfejsu API usługi. Można zadeklarować jako izolowane usługi przez ustawienie właściwości `IsolatedProcess="true"` w `ServiceAttribute` który adorns klasy usługi.


### <a name="media"></a>Nośnik

Nowe `Android.Media.MediaCodec` klasa udostępnia interfejs API do koderów-dekoderów media niskiego poziomu. Aplikacje można badać systemu, aby dowiedzieć się, jakie koderów-dekoderów, niski poziom dostępnych na urządzeniu.

Nowe `Android.Media.Audiofx.AudioEffect` podklasy zostały dodane do obsługi audio dodatkowe przetwarzanie wstępne na przechwyconych audio:

-   `Android.Media.Audiofx.AcousticEchoCanceler` — Ta klasa jest używana do przetwarzania wstępnego audio usunąć sygnał z strona zdalna z przechwyconych sygnału dźwiękowego. Na przykład usunięcie echo z aplikacją komunikacji głosu.
-   `Android.Media.Audiofx.AutomaticGainControl` — Ta klasa służy do normalizacji sygnał przechwycone przez zwiększania lub obniżenia sygnału wejściowego, dzięki czemu sygnałów jest stałą.
-   `Android.Media.Audiofx.NoiseSuppressor` — Ta klasa spowoduje usunięcie szumu tła przechwyconych sygnału.


Nie wszystkie urządzenia będą obsługują tych skutków. Metoda `AudioEffect.IsAvailable` powinna być wywoływana przez aplikację, aby zobaczyć, czy efekt audio zagrożona jest obsługiwany na urządzenie, na którym uruchomiono aplikację.

`MediaPlayer` Klasa obsługuje teraz gapless odtwarzania z `SetNextMediaPlayer()` metody. Ta nowa metoda określa dalej MediaPlayer można uruchomić podczas odtwarzania zakończeniem bieżącego odtwarzacza multimedialnego.

Następujące nowe klasy zapewniają stosowanie standardowych mechanizmów i interfejsu użytkownika do wybierania, gdzie można odtwarzać nośnika:

-   `MediaRouter` — Ta klasa umożliwia aplikacjom kontrolę routingu kanałów nośnika z urządzeniem osobom używającym zewnętrznych lub innych urządzeń.
-   `MediaRouterActionProvider` i `MediaRouteButton` — te klasy zapewnić spójny interfejs użytkownika wyboru i odtwarzania multimediów.




### <a name="notifications"></a>Powiadomienia

Android 4.1 umożliwia aplikacjom większą elastyczność i formantu o wyświetlania powiadomień. Aplikacje umożliwia teraz wyświetlanie większy lepsze powiadomień do użytkowników. Nowa metoda `NotificationBuilder.SetStyle()` umożliwia dla jednego z trzech nowych nowy styl można ustawić dla powiadomień:

-   `Notification.BigPictureStyle` — To jest klasa pomocy, która spowoduje wygenerowanie powiadomień, które mają obrazu w nich. Na poniższej ilustracji przedstawiono przykład powiadomienia o duży obraz:


 [![Zrzut ekranu BigPictureStyle powiadomienia](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

-   `Notification.BigTextStyle` — To jest klasa pomocy, która spowoduje wygenerowanie powiadomień, które mają wiele wierszy tekstu, na przykład wiadomości e-mail. Przykładem tego nowego stylu powiadomienia są widoczne na poniższym zrzucie ekranu:


 [![Zrzut ekranu BigTextStyle powiadomienia](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

-   `Notification.InboxStyle` — To jest klasa pomocy, która spowoduje wygenerowanie powiadomienia, które zawierają listę ciągów, takich jak wstawki z wiadomości e-mail, jak pokazano w tym zrzut ekranu:


 [![Zrzut ekranu Notification.InboxStyle powiadomienia](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

Istnieje możliwość dodanie maksymalnie dwóch przycisków akcji w dolnej części komunikatu powiadomienia powiadomienie jest przy użyciu stylu normalnym lub większy.
Na przykład widać na poniższym zrzucie ekranu, gdy przycisków akcji są widoczne w dolnej części powiadomienia:

 [![Zrzut ekranu przycisków akcji wyświetlone poniżej komunikatu powiadomienia](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

`Notification` Klasy otrzymał nowy stałe, które umożliwiają deweloperom Określ jeden z pięciu poziomów priorytet dla powiadomienia. Te opcje można ustawić na powiadomienia za pomocą `Priority` właściwości.



### <a name="permissions"></a>Uprawnienia

Dodano następujące nowe uprawnienia:

-   `READ_EXTERNAL_STORAGE` -Aplikacja wymaga tylko uprawnień odczytu do magazynu zewnętrznego. Obecnie wszystkie aplikacje mają dostęp do odczytu domyślnie, ale w przyszłych wydaniach systemu Android wymaga się, że aplikacje jawne żądanie dostępu do odczytu.
-   `READ_USER_DICTIONARY` — Umożliwia dostęp do odczytu do słownika słowa użytkownika.
-   `READ_CALL_LOG` — Umożliwia aplikacji uzyskać informacje dotyczące połączeń przychodzących i wychodzących przez odczytanie dziennika wywołań.
-   `WRITE_CALL_LOG` — Umożliwia aplikacji do zapisania do dziennika wywołań w telefonie.
-   `WRITE_USER_DICTIONARY` — Umożliwia aplikacji do zapisania do słownika słowa użytkownika.


Ważne zmiany, należy pamiętać, `READ_EXTERNAL_STORAGE` — obecnie to uprawnienie jest automatycznie przyznawane przez system Android. Przyszłych wersji systemu android wymaga aplikacji, aby zażądać tego uprawnienia przed uprawnienie.



## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono niektóre z nowych interfejsów API, które są dostępne w systemie Android 4.1 (16 poziom interfejsu API). Wyróżnione niektóre zmiany na animacji i uruchomienie działania animacji i wprowadzono nowe interfejsu API dla odnajdowania sieci innych urządzeń za pomocą protokołów, takich jak Bonjour lub UPnP. Inne zmiany do interfejsu API zostały wyróżnione również, takie jak możliwość wycinanie i wklejanie danych przy użyciu lokalizacji docelowych, możliwość używania usługi izolowanej lub "niestabilny" dostawców zawartości.

W tym artykule następnie poszło wprowadzenie do powiadomień o aktualizacji i opisano niektóre nowe uprawnienia, które zostały wprowadzone z systemem Android 4.1


## <a name="related-links"></a>Linki pokrewne

- [Przykład animacji czasu (przykład)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TimeAnimatorExample/)
- [Android 4.1 interfejsów API](http://developer.android.com/about/versions/android-4.1.html)
- [Zadania i Wstecz stosów](http://developer.android.com/guide/components/tasks-and-back-stack.html)
- [Nawigacja z użyciem Wstecz i w górę](http://developer.android.com/design/patterns/navigation.html)
