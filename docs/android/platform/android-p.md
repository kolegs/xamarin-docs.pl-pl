---
title: P dla systemu android (wersja zapoznawcza)
description: Jak rozpocząć pracę, korzystając z platformy Xamarin.Android do programowania aplikacji dla najnowszej wersji systemu Android.
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/27/2018
ms.openlocfilehash: a3eef6f2537a4b09f603787d7cdbf70a173fca80
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351865"
---
# <a name="android-p-preview"></a>P dla systemu android (wersja zapoznawcza)

_Jak rozpocząć pracę, korzystając z platformy Xamarin.Android do programowania aplikacji dla najnowszej wersji systemu Android._

![](~/media/shared/preview.png)

[Android P Developer Preview](https://developer.android.com/preview/) jest teraz dostępna od firmy Google. Nowe funkcje i interfejsy API są udostępniane w tej wersji, a wiele z nich są niezbędne móc korzystać z nowych możliwości sprzętu w najnowszych urządzeniach z systemem Android.

![Android obraz hero P](android-p-images/01-android-p-logo.png)

W tym artykule jest struktury, aby pomóc Ci rozpocząć pracę w opracowywaniu aplikacji platformy Xamarin.Android dla systemu Android w wersji zapoznawczej P. Pokazuje, jak zainstalować niezbędne aktualizacje i skonfigurować zestaw SDK przygotowanie emulatora lub urządzenia do testowania. Również zawiera omówienie nowych funkcji w systemie Android P i zapewnia przykładowym kodzie źródłowym, który ilustruje sposób korzystać z niektórych kluczowych funkcji P dla systemu Android.


## <a name="requirements"></a>Wymagania

Korzystanie z funkcji P dla systemu Android w aplikacjach opartych na środowisku Xamarin niezbędne jest, następujące elementy:

-   **Program Visual Studio** &ndash; korzystania z Windows należy zachować 15,8 wersji zapoznawczej 5 lub nowszego, programu Visual Studio jest wymagana wersja.  Jeśli używasz komputera Mac bieżącej wersji Beta programu Visual Studio dla komputerów Mac lub nowszy jest wymagany.

-   **Platforma Xamarin.Android** &ndash; Xamarin.Android 9.0.0.17 lub nowszy musi być zainstalowany w programie Visual Studio.

-   **Zestaw Java Developer Kit** &ndash; narzędzia deweloperskie dla platformy Xamarin dla systemu Android 9.0 wymagają [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (lub możesz spróbować dystrybucja firmy Microsoft w wersji zapoznawczej [OpenJDK](~/android/get-started/installation/openjdk.md)).

-   **Zestaw SDK systemu android** &ndash; API zestawu SDK systemu Android 28 lub nowszym należy zainstalować za pośrednictwem Menedżera zestawów Android SDK.

## <a name="getting-started"></a>Wprowadzenie

Rozpocząć pracę z usługą P dla systemu Android z rozszerzeniem Xamarin.Android, możesz pobrać i zainstalować najnowsze narzędzia i pakiety zestawu SDK, aby można było utworzyć swój pierwszy projekt P dla systemu Android:

1. Aktualizacja do wersji Visual Studio 15.8 w wersji zapoznawczej 5 lub nowszej. Jeśli używasz programu Visual Studio dla komputerów Mac, przełącz się do programu Visual Studio dla komputerów Mac [Beta](https://docs.microsoft.com/visualstudio/mac/update) kanału.

2. Zainstaluj **P dla systemu Android (28 interfejsu API)** lub nowsze pakiety i narzędzi za pośrednictwem Menedżera zestawów SDK.

3. Utwórz nowy projekt platformy Xamarin.Android przeznaczonych dla systemu Android P (28 interfejsu API).

4. Skonfiguruj emulatora lub urządzenia do testowania aplikacji systemu Android P.

Każdy z tych kroków zostało wyjaśnione w poniższych sekcjach:


### <a name="update-visual-studio"></a>Aktualizowanie programu Visual Studio

Aby dodać P dla systemu Android obsługuje do programu Visual Studio, zaktualizuj do wersji programu Visual Studio 2017 Preview należy zachować 15,8 5 lub nowszym jak wyjaśniono w [aktualizacji Visual Studio 2017 do najnowszej wersji](https://docs.microsoft.com/visualstudio/install/update-visual-studio).
Aby dodać P dla systemu Android obsługuje do programu Visual Studio dla komputerów Mac, zaktualizuj do najnowszej wersji Beta programu Visual Studio 2017 dla komputerów Mac zgodnie z objaśnieniem w [aktualizowania programu Visual Studio dla komputerów Mac](https://docs.microsoft.com/visualstudio/mac/update).


### <a name="install-the-android-sdk"></a>Instalowanie zestawu SDK systemu Android

Aby utworzyć projekt platformy Xamarin.Android 9.0, należy najpierw użyć Menedżera zestawów Android SDK do zainstalowania platformy zestawu SDK dla **P dla systemu Android (poziom interfejsu API 28)** lub nowszej.

1. Uruchom Menedżer zestawów SDK. W programie Visual Studio, kliknij przycisk **Narzędzia > Android > Menedżer zestawów SDK**. W programie Visual Studio dla komputerów Mac, kliknij przycisk **Narzędzia > Menedżer zestawów SDK**.

2. W prawym dolnym rogu, kliknij ikonę koła zębatego i wybierz **repozytorium > Google (nieobsługiwany)**:

    [![Ustawienie repozytorium do usługi Google](android-p-images/vs/set-repo-sml.png)](android-p-images/vs/set-repo.png#lightbox)

3. Zainstaluj **P dla systemu Android** pakietów, które są wyświetlane jako **Android SDK platformy 28** w **platform** kartę (Aby uzyskać więcej informacji na temat używania Menedżera zestawów SDK, zobacz (system Android Setup)[~/android/get-started/installation/android-sdk.md]) zestawu SDK: 

    [![Instalowanie pakietów P dla systemu Android](android-p-images/vs/sdk-manager-sml.png)](android-p-images/vs/sdk-manager.png#lightbox)

4. Jeśli używasz emulatora, tworzenie urządzenia wirtualnego, który obsługuje **28 poziom interfejsu API**. Aby uzyskać więcej informacji na temat tworzenia urządzeń wirtualnych, zobacz [Zarządzanie urządzeń wirtualnych przy użyciu Menedżera urządzeń Android](~/android/get-started/installation/android-emulator/device-manager.md).



### <a name="start-a-xamarinandroid-project"></a>Uruchom projekt platformy Xamarin.Android

Utwórz nowy projekt platformy Xamarin.Android. Jeśli jesteś nowym użytkownikiem opracowywania aplikacji systemu Android za pomocą platformy Xamarin, zobacz [Witaj, Android](~/android/get-started/hello-android/index.md) Aby dowiedzieć się więcej o tworzeniu projekty platformy Xamarin.Android.

Kiedy tworzysz projekt systemu Android, należy skonfigurować ustawienia wersję docelową P dla systemu Android lub nowszej. Na przykład, aby skierować projekcie dla systemu Android P, należy skonfigurować docelowy poziom interfejsu API systemu Android projektu do **P dla systemu Android (28 interfejsu API)**. Zalecane jest również ustawienie poziomie struktury docelowej do 28 interfejsu API lub nowszej. Aby uzyskać więcej informacji o konfigurowaniu poziomów poziom interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android w interpretacji](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-a-device-or-emulator"></a>Konfigurowanie urządzenia lub emulatora

Używasz urządzenie fizyczne, takie jak piksel, można zaktualizować urządzenie do systemu Android P (wersja zapoznawcza), postępując zgodnie z instrukcjami wyświetlanymi w [urządzeń z systemem Android w wersji Beta P](https://developer.android.com/preview/devices/).

Jeśli używasz emulatora utworzyć urządzenie wirtualne dla poziomu interfejsu API przy użyciu obrazu x86 28. Aby dowiedzieć się, jak za pomocą Menedżera urządzeń systemu Android do tworzenia i Zarządzaj urządzeniami wirtualnymi, zobacz [Zarządzanie urządzeń wirtualnych przy użyciu Menedżera urządzeń Android](~/android/get-started/installation/android-emulator/device-manager.md).
Aby dowiedzieć się, jak za pomocą emulatora systemu Android do testowania i debugowania, zobacz [debugowania za pomocą emulatora systemu Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md).



## <a name="new-features"></a>Nowe funkcje

P dla systemu android wprowadzono wiele nowych funkcji. Niektóre z tych nowych funkcji mają korzystać z nowych możliwości sprzętu, oferowanych przez najnowszych urządzeniach z systemem Android, natomiast inne są zaprojektowane w celu dodatkowego zwiększenia środowisko użytkownika systemu Android:

-   **Wyświetlanie pomocy technicznej wycinania** &ndash; udostępnia interfejsy API, aby znaleźć lokalizację i kształt _wycinania_ w górnej części ekranu na nowszych urządzeniach z systemem Android.

-   **Ulepszenia powiadomień** &ndash; powiadomienia teraz wyświetlić obrazy i nowe `Person` klasa jest używana do uproszczenia uczestników konwersacji.

-   **Pozycjonowanie wewnętrznych** &ndash; platformy obsługiwane przez protokół Round round-trip-Time sieci Wi-Fi, który umożliwia aplikacji za pomocą sieci Wi-Fi urządzeń do nawigacji w ustawieniach wewnętrznych.

-   **Obsługa wielu aparatów** &ndash; oferuje możliwości do dostępu do strumieni jednocześnie z wielu fizycznych kamer (takich jak aparaty fotograficzne dwa porty frontonu i z powrotem podwójne).


W poniższych sekcjach przedstawiające te funkcje i zapewnić, że kod krótki przykłady ułatwiające rozpoczęcie pracy, ich użycie w swojej aplikacji.

### <a name="display-cutout-support"></a>Obsługa wycinania wyświetlania

Masz wiele nowszych urządzeniach z systemem Android z ekranami krawędzi do krawędzi *wyświetlić wycinania* (lub "więcej") w górnej części ekranu kamery i głośników.
Poniższy zrzut ekranu przedstawiono przykład emulatora wycinania:

[![Symulowanie wycinania emulatora systemu android](android-p-images/02-example-cutout-sml.png)](android-p-images/02-example-cutout.png#lightbox)

Aby zarządzać, sposobu wyświetlania jej zawartość okna aplikacji na urządzeniach przy użyciu wycinania wyświetlania, P dla systemu Android został dodany nowy [LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode) atrybut układu okna. Ten atrybut można wybrać jedną z następujących wartości:

-   [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) &ndash; okna nigdy nie mogą nakładać się z obszarem wycinania.

-   [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) &ndash; okna można rozszerzyć do obszaru wycinania, ale tylko w krótkich krawędzi ekranu. 

-   [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) &ndash; okna może rozszerzyć do obszaru wycinania, jeśli wycięcie znajduje się w pasku systemu.

Na przykład, aby zapobiec nakładać się na obszarze wycinania okna aplikacji, ustaw tryb wycinania układ na *nigdy nie*: 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

W poniższych przykładach udostępniono przykłady tych trybów wycinania. Pierwszy zrzut ekranu po lewej stronie jest aplikacji w trybie pełnoekranowym innych. Zrzut ekranu Centrum, aplikacja przechodzi trybie pełnoekranowym z `LayoutInDisplayCutoutMode` równa `LayoutInDisplayCutoutModeShortEdges`. Zwróć uwagę, że białe tło aplikacji rozszerza się w obszarze wycinania:

[![Przykład wyświetlanie trybów wycinania w emulatorze](android-p-images/03-cutout-modes-sml.png)](android-p-images/03-cutout-modes.png#lightbox)

W ostatnim zrzut ekranu (powyżej po prawej stronie), `LayoutInDisplayCutoutMode` ustawiono `LayoutInDisplayCutoutModeShortNever` zanim przejdzie do pełnego ekranu.
Należy zauważyć, że białe tło w aplikacji nie może rozszerzyć do obszaru wycinania wyświetlania.

Jeśli potrzebujesz bardziej szczegółowe informacje o obszarze wycinania na urządzeniu, można użyć nowej [DisplayCutout](https://developer.android.com/reference/android/view/DisplayCutout.html) klasy. `DisplayCutout` reprezentuje obszar wyświetlania, którego nie można użyć do wyświetlania zawartości. Można pobrać lokalizacji i kształt wycięcie, dzięki czemu aplikacja nie jest podejmowana próba wyświetlania zawartości w tym obszarze współzależności funkcjonalnych, można użyć tych informacji.

Aby uzyskać więcej informacji na temat nowych funkcji wycinania w P dla systemu Android, zobacz [Wyświetlanie pomocy technicznej wycinania](https://developer.android.com/preview/features#cutout).



### <a name="notifications-enhancements"></a>Ulepszenia powiadomień

P dla systemu android wprowadzono następujące ulepszenia w celu usprawnienie obsługi komunikatów:

-   Kanały powiadomień (wprowadzona w [systemu Android Oreo](~/android/platform/oreo.md)) obsługuje teraz blokuje grup kanału.

-   Systemu powiadomień ma trzy nowe kategorie przeszkadzać nie czy (priorytety, alarmy, dźwięki systemu oraz źródeł multimediów). Ponadto istnieją siedem nowych przeszkadzać nie są tryby, które mogą służyć do pomijania visual przerw w działaniu (na przykład wskaźniki, powiadomienie światła, wygląd paska stanu i uruchamianie działań pełnego ekranu).

-   Nowy [osoby](https://developer.android.com/reference/android/app/Person.html) klasa została dodana do reprezentowania nadawcy wiadomości. Użyj tej klasy pomaga w optymalizacji renderowania Każde powiadomienie, określając osoby zaangażowane w konwersacji (w tym identyfikatory URI oraz awatary).

-   Powiadomienia można teraz wyświetlać obrazy. 

Poniższy przykład ilustruje sposób korzystania z nowych interfejsów API do generowania powiadomień, który zawiera obraz. W poniższych zrzutach ekranu powiadomienie tekstowe zostanie opublikowana, a następuje powiadomienie z osadzonego obrazu. Gdy powiadomień zostaną rozwinięte (widziany po prawej stronie), wyświetlany jest tekst pierwszego powiadomienia, a obraz osadzony w powiększenia drugie powiadomienie:

[![Przykład powiadomienia za pomocą obrazu](android-p-images/04-example-notifications-sml.png)](android-p-images/04-example-notifications.png#lightbox)

W poniższym przykładzie pokazano jak dołączyć obraz w wiadomość z powiadomieniem P dla systemu Android i pokazuje sposób użycia nowej `Person` klasy:

1. Utwórz `Person` obiekt, który reprezentuje nadawcy. Na przykład nazwy i ikony nadawcy są objęte `fromPerson`:

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. Tworzenie `Notification.MessagingStyle.Message` zawiera obraz, aby wysłać, przekazywanie obrazu do nowego [Notification.MessagingStyle.Message.SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri) metody.
   Na przykład:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. Dodany komunikat `Notification.MessagingStyle` obiektu. Na przykład:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. Podłącz ten styl do konstruktora powiadomień. Na przykład:

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. Opublikuj powiadomienia. Na przykład:

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

Aby uzyskać więcej informacji o tworzeniu powiadomień, zobacz [powiadomień lokalnych](~/android/app-fundamentals/notifications/local-notifications.md).


### <a name="indoor-positioning"></a>Pozycjonowanie wewnętrznych

P dla systemu android zapewnia obsługę IEEE 802.11mc (znany także jako _sieci Wi-Fi Round-round-trip-Time_ lub _RTT sieci Wi-Fi_), co umożliwia aplikacji wykrywanie odległość do jednego lub wskazuje szerszy dostęp do sieci Wi-Fi. Korzystając z tych informacji jest możliwe dla swojej aplikacji móc korzystać z *wewnętrznych pozycjonowanie* z dokładnością do jednej lub dwóch liczników. Na urządzeniach z systemem Android, które zapewniają pomoc techniczna dotycząca sprzętu dla IEEE 801.11mc aplikacji mogą oferować funkcje nawigacji, takie jak oparte na lokalizacji kontrolę nad inteligentnych urządzeń lub Włącz, wyłącz instrukcje za pośrednictwem sklepu:

[![Przykład wewnętrznych nawigacji za pomocą RTT sieci Wi-Fi](android-p-images/05-wifi-rtt-sml.png)](android-p-images/05-wifi-rtt.png#lightbox)

Nowy [WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager) klasy i kilka klas pomocniczych udostępnia środki do pomiarów odległości do sieci Wi-Fi urządzenia. Aby uzyskać więcej informacji na temat wewnętrznych interfejsów API pozycjonowania, wprowadzona w systemie Android P, zobacz [Android.Net.Wifi.Rtt](https://developer.android.com/reference/android/net/wifi/rtt/package-summary).


### <a name="multi-camera-support"></a>Obsługa wielu aparatów

Wiele nowych urządzeń dla systemu Android ma dwa porty frontonu i/lub podwójne do tyłu aparaty fotograficzne, które są przydatne w przypadku takie funkcje jak stereo wizji, rozszerzone efektów wizualnych i powiększenia ulepszone możliwości. P dla systemu android wprowadzono nowy [wielu aparatu](https://developer.android.com/preview/features#camera) interfejsu API, który pozwala na używanie przez aplikację do używania *logiczne aparatu* (lub *logiczne aparatu wielu*) który bazuje na dwóch lub więcej aparaty fizycznych.
Aby ustalić, czy urządzenie obsługuje aparatu logiczne multi, można przyjrzeć się możliwości każdej kamery na urządzeniu, aby zobaczyć, czy obsługuje [RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA).

P dla systemu android zawiera także nowy [SessionConfiguration](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html) klasę, która może służyć do zmniejszenia opóźnień podczas przechwytywania wstępnego i wyeliminować potrzebę rozpoczęcie strumienia kamery i.

Aby uzyskać więcej informacji na temat wielu aparatu obsługi P dla systemu Android, zobacz [aparatu wielu aktualizacji pomocy technicznej i aparat](https://developer.android.com/preview/features#camera).


### <a name="other-features"></a>Inne funkcje

Ponadto P dla systemu Android obsługuje kilka nowych funkcji:

-   Nowy [AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html) klasy, która może służyć do rysowania i wyświetlanie obrazów animowany.

-   Nowy [ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html) klasy, która zastępuje `BitmapFactory`. `ImageDecoder` może służyć do zdekodowania `AnimatedImageDrawable`.

-   Obsługa obrazów wideo HDR (zakres dynamiczny wysokiej) i HEIF (wysoka wydajność Image File Format).

-   [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) zostało ulepszone, aby bardziej inteligentnie obsługę zadań związanych z siecią. Nowy [GetNetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29) metody [JobParameters](https://developer.android.com/reference/android/app/job/JobParameters) Klasa zwraca optymalne sieciowe do wykonywania żądań sieci dla danego zadania.

Aby uzyskać więcej informacji na temat najnowszych funkcji P dla systemu Android, zobacz [P dla systemu Android, funkcje i interfejsy API](https://developer.android.com/preview/features).


## <a name="behavior-changes"></a>Zmiany zachowania

Gdy docelowa wersja systemu Android jest ustawiony na poziom interfejsu API 28, istnieje kilka zmian platformy, które mogą wpłynąć na zachowanie aplikacji, nawet wtedy, gdy nie są Implementowanie nowe funkcje opisane powyżej. Poniżej przedstawiono krótkie podsumowanie tych zmian:

-  Aplikacje teraz musi żądać uprawnienia pierwszego planu, przed rozpoczęciem korzystania z usługi pierwszego planu.

-  Jeśli aplikacja ma więcej niż jeden proces, nie można udostępnić w niej pojedynczej [WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) katalog danych między procesami.

-  Bezpośredni dostęp do katalogu danych innej aplikacji przy użyciu ścieżki nie jest już dozwolone.

Aby uzyskać więcej informacji na temat zmiany zachowania dla aplikacji przeznaczonych dla systemu Android P, zobacz [zmiany zachowania](https://developer.android.com/preview/behavior-changes.html#p-apps).


## <a name="sample-code"></a>Przykładowy kod

[AndroidPMiniDemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo) jest przykładową aplikację platformy Xamarin.Android P dla systemu Android, które pokazuje, jak ustawić wycinania tryby wyświetlania, jak korzystać z nowych `Person` klasy oraz jak wysyłać wiadomość z powiadomieniem, który zawiera obraz.


## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzone dla systemu Android P (wersja zapoznawcza) i wyjaśniono, jak zainstalować i skonfigurować najnowsze narzędzia i pakiety do tworzenia aplikacji platformy Xamarin.Android przy użyciu P dla systemu Android w wersji zapoznawczej. Ona udostępniana z omówieniem kluczowych funkcji dostępnych w P dla systemu Android z przykładowym kodzie źródłowym dla kilku z tych funkcji. Zawierała linki do dokumentacji interfejsu API i tematy dewelopera systemu Android, aby ułatwić wprowadzenie do tworzenia aplikacji dla systemu Android P. Wyróżnione najważniejsze zmiany zachowania P dla systemu Android, które może mieć wpływ na istniejące aplikacje.


## <a name="related-links"></a>Linki pokrewne

- [P dla systemu android Developer Preview](https://developer.android.com/preview/)
