---
title: Funkcje określić wersję nougat
description: Jak rozpocząć pracę, korzystając z platformy Xamarin.Android do tworzenia aplikacji dla systemu Android określić wersję Nougat.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 2e15de944f05fede802dbf52987d80a46fb890ef
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242156"
---
# <a name="nougat-features"></a>Funkcje określić wersję nougat

_Jak rozpocząć pracę, korzystając z platformy Xamarin.Android do tworzenia aplikacji dla systemu Android określić wersję Nougat._

Ten artykuł zawiera zarys funkcji wprowadzonych w systemie Android określić wersję Nougat, wyjaśnia, jak przygotować Xamarin.Android do tworzenia aplikacji dla systemu Android określić wersję Nougat oraz podano linki do przykładowych aplikacji, które ilustrują sposób korzystania z funkcji systemu Android określić wersję Nougat Aplikacji platformy Xamarin.Android.


## <a name="overview"></a>Omówienie

[Android określić wersję Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) jest kolejna firmy Google do systemu Android Marshmallow 6.0. Platforma Xamarin.Android dodano obsługę **Android 7.x powiązania** w środowisku Xamarin Android 7.0 i nowszych wersjach. Android określić wersję Nougat dodaje wiele nowych interfejsów API funkcji określić wersję Nougat opisanych poniżej; te interfejsy API są dostępne dla aplikacji platformy Xamarin.Android, korzystając z platformy Xamarin.Android 7.0.

[![Obrazy Hero tabletów z systemem Android i telefony z systemem Android określić wersję Nougat](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Aby uzyskać więcej informacji na temat Android 7.x interfejsów API, zobacz [Android 7.1 dla deweloperów](http://developer.android.com/preview/api-overview.html).
Aby uzyskać listę znanych problemów 7.0 platformy Xamarin.Android, zobacz [informacje o wersji](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/).

Android określić wersję Nougat zawiera wiele nowych funkcji przydatnych deweloperom platformy Xamarin.Android. Te funkcje obejmują:

-   **Obsługa wielu okien** &ndash; to rozszerzenie umożliwia użytkownikom otwieranie dwie aplikacje na ekranie jednocześnie.

-   **Ulepszenia powiadomień** &ndash; system przeprojektowana powiadomienia w systemie Android określić wersję Nougat obejmuje *bezpośrednie odpowiedzi* funkcja, która umożliwia użytkownikom szybkie odpowiadanie na wiadomości tekstowych bezpośrednio z poziomu powiadomienia INTERFEJS UŻYTKOWNIKA. Także jeśli aplikacja tworzy powiadomienia dotyczące odebranych komunikatów, nowa *powiązane powiadomienia* funkcji można powiązać powiadomienia razem jako pojedynczej grupy po odebraniu więcej niż jeden komunikat.

-   **Oszczędzanie danych** &ndash; ta funkcja jest nowa usługa systemu, która pozwala zmniejszyć użycie danych komórkowych przez aplikacje; zapewnia kontrolę nad jak korzystać z danych komórkowych przez aplikacje użytkownikom.

Ponadto dla systemu Android określić wersję Nougat oferuje wiele innych rozszerzeń dla deweloperów aplikacji takich jak nowa funkcja konfiguracji zabezpieczeń sieci, Doze podczas podróży, klucza zaświadczania, nowe szybkie ustawienia interfejsów API, obsługi wielu ustawień regionalnych, interfejsów API ICU4J WebView rozszerzenia dostęp do funkcji języka Java 8, dostęp do katalogu o określonym zakresie, wskaźnik niestandardowego interfejsu API, pomoc techniczna platformy VR, pliki wirtualne i optymalizacje przetwarzania w tle.

W tym artykule wyjaśniono, jak zacząć tworzyć aplikacje przy użyciu systemu Android określić wersję Nougat, aby wypróbować nowe funkcje i planowanie migracji lub funkcji pracy pod kątem nowa platforma Android określić wersję Nougat.


## <a name="requirements"></a>Wymagania

Następujące wymagane jest wprowadzenie nowych funkcji systemu Android określić wersję Nougat w aplikacjach opartych na środowisku Xamarin:

-   **Visual Studio lub Visual Studio dla komputerów Mac** &ndash; Jeśli używasz programu Visual Studio w wersji 4.2.0.628 lub nowszego, programu Visual Studio Tools for Xamarin jest wymagana. Jeśli używasz programu Visual Studio dla komputerów Mac, wersja 6.1.0 lub nowszy programu Visual Studio dla komputerów Mac jest wymagane.

-   **Platforma Xamarin.Android** &ndash; Xamarin.Android 7.0 lub nowszy musi być zainstalowane i skonfigurowane za pomocą programu Visual Studio lub Visual Studio dla komputerów Mac.

-   **Zestaw SDK systemu android** — Android 7.0 zestawu SDK (interfejs API 24) lub nowszym należy zainstalować za pośrednictwem Menedżera zestawów Android SDK.

-   **Zestaw Java Developer Kit** &ndash; narzędzia deweloperskie dla platformy Xamarin Android 7.0 wymagają [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) lub nowszy, jeśli tworzysz, poziom interfejsu API, 24 lub nowszej (JDK 8 obsługuje również poziomy interfejsu API starszych niż 24). 64-bitowej wersji zestawu JDK 8 jest wymagany, jeśli używasz kontrolek niestandardowych lub podglądzie formularzy.

> [!IMPORTANT]
> Platforma Xamarin.Android nie obsługuje zestaw JDK 9.

Należy pamiętać, że aplikacje musi zostać zrekompilowany, za pomocą platformy Xamarin C6SR4 lub nowszym niezawodną pracę z systemem Android określić wersję Nougat. Ponieważ Android określić wersję Nougat można połączyć tylko z [dostarczone do zestawu NDK bibliotek natywnych](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), istniejących aplikacji przy użyciu bibliotek, takich jak **Mono.Data.Sqlite.dll** może ulec awarii podczas uruchamiania w Menedżerze Nuget dla systemu Android, jeśli nie są prawidłowo odbudowane.



## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć pracę, przy użyciu systemu Android określić wersję Nougat z rozszerzeniem Xamarin.Android, należy pobrać i zainstalować najnowsze narzędzia i pakiety zestawu SDK, zanim będzie można utworzyć projekt dla systemu Android określić wersję Nougat:

1.  Zainstaluj najnowsze aktualizacje produktu Xamarin.Android z Xamarin.

2.  Zainstaluj **Android 7.0 (interfejs API 24)** pakietów i narzędzia lub nowszej.

3.  Utwórz nowy projekt platformy Xamarin.Android przeznaczonych dla systemu Android określić wersję Nougat.

4.  Skonfiguruj emulatora lub urządzenia, aby określić wersję Nougat w systemie Android.

Każdy z tych kroków zostało wyjaśnione w poniższych sekcjach:


### <a name="install-xamarin-updates"></a>Zainstaluj aktualizacje platformy Xamarin

Aby dodać obsługę programu Xamarin dla systemu Android określić wersję Nougat, zmień kanał aktualizacji w programie Visual Studio lub Visual Studio dla komputerów Mac na kanał stabilne i Zastosuj najnowsze aktualizacje. Jeśli potrzebujesz także funkcje, które są obecnie dostępne tylko w kanał alfa lub Beta, możesz przełączyć się do kanału alfa lub wersji Beta (kanałów alfa i beta można uzyskać również zapewnić obsługę dla systemu Android 7.x). Aby uzyskać informacje o zmianie kanału aktualizacji (wersje), zobacz [zmianę kanału aktualizacji](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel).



### <a name="install-the-android-sdk"></a>Instalowanie zestawu SDK systemu Android

Aby utworzyć projekt za pomocą platformy Xamarin Android 7.0, należy najpierw użyć Menedżera zestawów Android SDK do zainstalowania **zestawu SDK platformy systemu Android N (interfejs API 24)** lub nowszej. Należy również zainstalować najnowszy **Android SDK Tools**:

1.  Uruchom Menedżer zestawów SDK systemu Android (w programie Visual Studio dla komputerów Mac, należy użyć **Narzędzia > Otwórz Menedżer zestawów SDK&hellip;**; w programie Visual Studio, użyj **Narzędzia > Android > Menedżer zestawów SDK**).

2.  Zainstaluj **Android 7.0 (interfejs API 24)** lub nowszej:

    [![Wybranie pakietów dla systemu Android 7.0 w menedżera zestawów Android SDK](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  Zainstaluj najnowsze narzędzia zestawu SDK systemu Android:

    [![Wybieranie najnowszych narzędzi zestawu SDK systemu Android w menedżera zestawów Android SDK](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Należy zainstalować poprawkę Android SDK Tools 25.2.2 lub nowszym, systemem Android narzędzia zestawu SDK platformy 24.0.3 lub nowszy i program Android SDK narzędzia do kompilacji 24.0.2 lub nowszej.

4.  Upewnij się, że **lokalizację zestawu Java Development Kit** jest skonfigurowany dla zestawu JDK 1.8:

    [![Konfigurowanie ścieżki JDK 8 w ramach opcji narzędzi](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Aby wyświetlić to ustawienie w programie Visual Studio, kliknij **Narzędzia > Opcje > Xamarin > Ustawienia systemu Android**. W programie Visual Studio dla komputerów Mac, kliknij przycisk **Preferencje > Projekty > Lokalizacje zestawów SDK > Android**.



### <a name="start-a-xamarinandroid-project"></a>Uruchom projekt platformy Xamarin.Android

Utwórz nowy projekt platformy Xamarin.Android. Jeśli jesteś nowym użytkownikiem opracowywania aplikacji systemu Android za pomocą platformy Xamarin, zobacz [Witaj, Android](~/android/get-started/hello-android/index.md) Aby dowiedzieć się więcej o tworzeniu projekty platformy Xamarin.Android.

Kiedy tworzysz projekt systemu Android, należy skonfigurować ustawienia wersji docelowej systemu Android 7.0 lub nowszej. Na przykład, aby skierować projekcie dla systemu Android 7.0, należy skonfigurować docelowy poziom interfejsu API systemu Android projektu do **Android 7.0 (interfejs API 24 - określić wersję Nougat)**. Zaleca się ustawienie poziomie struktury docelowej do interfejsu API, 24 lub nowszej. Aby uzyskać więcej informacji o konfigurowaniu poziomów poziom interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android w interpretacji](~/android/app-fundamentals/android-api-levels.md).


> [!NOTE]
> Aktualnie należy ustawić **wersja systemu Android, które są co najmniej** do **Android 7.0 (interfejs API 24 - określić wersję Nougat)** do wdrożenia aplikacji dla systemu Android określić wersję Nougat urządzeń lub emulatorów.



### <a name="configure-an-emulator-or-device"></a>Konfigurowanie emulatora lub urządzenia

Jeśli używasz emulatora, uruchom Menedżera AVD systemu Android i Utwórz nowe urządzenie, używając następujących ustawień:

-   Urządzenie: Nexus 5 X Nexus 6, Nexus 6P, gracz Nexus Nexus 9 lub pikselach C.
-   Cel: Android 7.0 — poziom interfejsu API 24
-   ABI: x86 lub x86\_64

Na przykład tego urządzenia wirtualnego jest skonfigurowany do emulowania Nexus 6:

[![Konfigurowanie urządzeń AVD, za pomocą urządzeń Nexus 6 docelowych z systemem Android 7.0 i Intel Atom x86 procesora CPU/ABI](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Jeśli używasz urządzenia fizycznego, takich jak Nexus 5 X, 6 lub 9 można zaktualizować urządzenie za pośrednictwem automatyczne za pośrednictwem aktualizacji air (Stachnio) lub pobranie obrazu systemu i flash urządzenia bezpośrednio. Aby uzyskać więcej informacji na temat ręcznego aktualizowania urządzenia do systemu Android określić wersję Nougat zobacz [Stachnio obrazów dotyczy urządzeń Nexus](https://developers.google.com/android/nexus/ota).

Należy pamiętać, że Nexus 5 urządzenia nie są obsługiwane przez Android określić wersję Nougat.



## <a name="new-features"></a>Nowe funkcje

Android określić wersję Nougat wprowadzono szereg nowych funkcji i możliwości, takich jak wielu okien obsługi i ulepszenia powiadomień, wygaszacz danych. Poniższe sekcje pokazują następujące funkcje i zapewniają łącza, aby rozpocząć pracę, ich użycie w swojej aplikacji.



### <a name="multi-window-mode"></a>Tryb wielu okien

Tryb wielu okien sprawiają, że użytkownicy będą mogli otwierać dwie aplikacje jednocześnie z obsługą wielozadaniowość pełne. Te aplikacje mogą być uruchamiane side-by-side (w poziomie) lub jeden powyżej inne (w pionie) w trybie podzielonym ekranem.
Użytkowników można przeciągać pasek podziału między aplikacjami, aby zmieniać ich rozmiar i ich można wyciąć i wkleić zawartości między aplikacjami. Dwie aplikacje umieszczeniem w trybie wielu okien wybrane działanie w dalszym ciągu uruchomiona podczas niezaznaczone działanie jest wstrzymane, ale nadal widoczne. Tryb wielu okien nie powoduje modyfikacji cyklu życia działanie systemu Android.

[![Przykładowe aplikacje uruchomione w trybie wielu okien w pionowa i pozioma](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Można skonfigurować sposób działania aplikacji platformy Xamarin.Android obsługują tryb wielu okien. Na przykład można skonfigurować atrybuty, które ustawić minimalny rozmiar i domyślną szerokość i wysokość aplikacji w trybie wielu okien. Można użyć nowej `Activity.IsInMultiWindowMode` właściwości w celu określenia, czy działania w trybie wielu okien. Na przykład:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) Przykładowa aplikacja zawiera kod języka C#, który pokazuje, jak korzystać z zalet okna wielu interfejsów użytkownika z aplikacją.

Aby uzyskać więcej informacji na temat tryb wielu okien, zobacz [wielu okien obsługi](https://developer.android.com/guide/topics/ui/multi-window.html).



### <a name="enhanced-notifications"></a>Ulepszone powiadomienia

Android określić wersję Nougat wprowadza system przeprojektowana powiadomień. Zawiera funkcje nową funkcję bezpośrednie odpowiedzi, która umożliwia użytkownikom szybkie odpowiedzi na powiadomienia dla przychodzących wiadomości tekstowych bezpośrednio w powiadomieniu interfejsu użytkownika. Począwszy od systemu Android 7.0, powiadomienie o wiadomości można stworzyć pakiet jako pojedynczej grupy po odebraniu więcej niż jeden komunikat. Ponadto deweloperzy będą mogli dostosować powiadomienie widoków, wykorzystaj dekoracje system w powiadomieniach i korzystać z zalet nowych szablonów powiadomień podczas generowania powiadomień.


#### <a name="direct-reply"></a>Bezpośrednie odpowiedzi

Gdy użytkownik otrzyma powiadomienie dla wiadomości przychodzących, Android określić wersję Nougat umożliwia odpowiedzieć na wiadomość w treści powiadomienia (zamiast Otwórz aplikację do obsługi wiadomości do wysłania odpowiedzi).
Tej wbudowanej funkcji odpowiedzi umożliwia użytkownikom szybkie odpowiadanie na wiadomości SMS lub wiadomości tekstowej bezpośrednio z poziomu interfejsu powiadomień:

[![Zrzut ekranu przedstawiający powiadomienie z polem bezpośrednie odpowiedzi wbudowane](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Aby obsługiwać tę funkcję w swojej aplikacji, należy dodać *wbudowanych akcji odpowiedzi* do swojej aplikacji za pośrednictwem [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) obiekt, tak aby użytkownicy potrzebują pomocy eksperta za pośrednictwem tekst bezpośrednio z poziomu powiadomień interfejsu użytkownika.
Na przykład, poniższy kod kompilacje `RemoteInput` do odbierania wprowadzania tekstu, kompilacje oczekiwanie zamierzone akcji odpowiedzi i tworzy akcję zdalną włączono danych wejściowych:

```csharp
// Build a RemoteInput for receiving text input:
var remoteInput = new Android.Support.V4.App.RemoteInput.Builder (EXTRA_REMOTE_REPLY)
    .SetLabel (GetString (Resource.String.reply))
    .Build ();

// Build a Pending Intent for the reply action to trigger:
PendingIntent replyIntent = PendingIntent.GetBroadcast (ApplicationContext,
                                conversation.ConversationId,
                                GetMessageReplyIntent (conversation.ConversationId),
                                PendingIntentFlags.UpdateCurrent);

// Build an Android 7.0 compatible Remote Input enabled action:
NotificationCompat.Action actionReplyByRemoteInput = new NotificationCompat.Action.Builder (
    Resource.Drawable.notification_icon,
    GetString (Resource.String.reply),
    replyIntent).AddRemoteInput (remoteInput).Build ();
```

Ta akcja jest dodawany do powiadomień:

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

[Usługa wiadomości](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) Przykładowa aplikacja zawiera kod języka C#, który demonstruje sposób rozszerzyć powiadomień za pomocą `RemoteInput` obiektu. Aby uzyskać więcej informacji o dodawaniu odpowiedzi wbudowane akcje do aplikacji dla systemu Android 7.0 lub nowszego, zobacz Android [odpowiadania na powiadomienia](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) tematu.


#### <a name="bundled-notifications"></a>Powiązane powiadomienia

Android określić wersję Nougat można zgrupować komunikaty powiadomień (na przykład w temacie wiadomości) i grupy, a nie poszczególnych oddzielną wiadomość.
To *powiązane powiadomienia* funkcja umożliwia użytkownikom odrzucać lub archiwum grupy powiadomień w jednej akcji. Użytkownik może Przesuń w dół rozwiń pakietu powiadomienia, aby wyświetlić szczegóły każdego powiadomienia:

[![Zrzut ekranu przykładu powiązane powiadomień](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Aby obsługiwać powiązane powiadomienia, Twoja aplikacja może używać [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) metodę do zbioru podobnych powiadomienia. Aby uzyskać więcej informacji na temat grup powiązane powiadomienia w systemie Android N, zobacz Android [tworzenie pakietów powiadomienia](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) tematu.


#### <a name="custom-views"></a>Widoki niestandardowe

Android określić wersję Nougat pozwala tworzyć widoki niestandardowe powiadomienie z systemu powiadomień, akcje i rozwijania układów. Aby uzyskać więcej informacji na temat widoki niestandardowe powiadomienie w systemie Android określić wersję Nougat Zobacz Android [ulepszenia powiadomień](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) tematu.



### <a name="data-saver"></a>Oszczędzanie danych

Począwszy od systemu Android określić wersję Nougat, użytkownicy mogą włączyć nowy *wygaszacz danych* ustawienie, że bloki w tle użycia danych. To ustawienie również sygnały aplikację do używania mniejszej ilości danych na pierwszym planie, tam gdzie to możliwe. [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) została rozszerzona w systemie Android określić wersję Nougat tak, aby aplikację można sprawdzić, czy użytkownik ma włączone wygaszacz danych tak, aby aplikację można postarać się ograniczenie jego użycia danych w przypadku, gdy wygaszacz danych jest włączone.

Aby uzyskać więcej informacji na temat nowej funkcji wygaszacz danych w systemie Android określić wersję Nougat Zobacz Android [optymalizacji użycia danych sieci](https://developer.android.com/training/basics/network-ops/data-saver.html) tematu.



### <a name="app-shortcuts"></a>Skróty aplikacji

Android 7.1 wprowadzone *skróty aplikacji* funkcja, która umożliwia użytkownikom szybko wspólnych lub zalecanych zadania uruchamiania aplikacji.
Aby uaktywnić menu skrótów, użytkownik long naciśnięcia ikonę aplikacji na kilka sekund &ndash; za pomocą szybkiego wibracje w wyświetlonym menu.
Zwalnianie naciśnięciem powoduje, że pozostają w menu:

[![Przykładowy ekran aplikacji menu skrótów dla aplikacji obsługi wiadomości](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Ta funkcja jest dostępna tylko poziom interfejsu API 25 lub większą.
Aby uzyskać więcej informacji na temat nowych funkcji skróty aplikacji w systemie Android 7.1 Zobacz Android [skróty aplikacji](https://developer.android.com/guide/topics/ui/shortcuts.html) tematu.


### <a name="sample-code"></a>Przykładowy kod

Przykłady rozszerzenia Xamarin.Android kilka są dostępne dla pokazują, jak korzystać z funkcji systemu Android określić wersję Nougat:

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) zademonstrowano użycie wielu okien interfejsów API dostępnych w systemie Android określić wersję Nougat. Możesz przełączać przykładową aplikację w trybie wielu systemu windows, aby zobaczyć, jak wpływa na zachowanie i cyklem życia aplikacji.

-   [Usługa komunikatów](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) jest prostą usługę, która wysyła powiadomienia za pomocą `NotificationCompatManager`. Rozszerzał powiadomienie z `RemoteInput` obiektu na urządzeniach Android określić wersję Nougat będą odpowiadać za pośrednictwem tekst bezpośrednio z poziomu powiadomienia, bez konieczności otwierania aplikacji.

-   [Aktywne powiadomienia](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) pokazuje sposób użycia `NotificationManager` interfejsu API, aby poinformować Cię, jak wiele powiadomień aplikacji są obecnie wyświetlane.

-   [Zakres dostępu do katalogów](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) pokazuje sposób użycia interfejs API dostępu do katalogu o określonym zakresie, aby łatwo uzyskiwać dostęp do określonych katalogów. Służy jako alternatywę do konieczności wcześniejszego definiowania `READ_EXTERNAL_STORAGE` lub `WRITE_EXTERNAL_STORAGE` uprawnień w manifeście.

-   [Bezpośrednie rozruchu](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) pokazano, jak przechowywać dane w magazynie urządzenie zaszyfrowane, który zawsze jest dostępny, gdy urządzenie rozruchu zarówno przed i po wprowadzeniu credentials(PIN/Pattern/Password) dowolnego użytkownika.


## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzone dla systemu Android określić wersję Nougat i wyjaśniono, jak zainstalować i skonfigurować najnowsze narzędzia i pakiety do tworzenia aplikacji platformy Xamarin.Android w Menedżerze Nuget dla systemu Android. Również podane z omówieniem kluczowych funkcji dostępnych w systemie Android określić wersję Nougat, wraz z łączami do przykładowym kodzie źródłowym, aby ułatwić wprowadzenie do tworzenia aplikacji dla systemu Android określić wersję Nougat.


## <a name="related-links"></a>Linki pokrewne

- [Android 7.1 dla deweloperów](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Informacje o wersji platformy Xamarin Android 7.0](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
