---
title: Funkcje nugacie
description: "Jak rozpocząć pracę, aby opracowywać aplikacje dla systemu Android nugacie przy użyciu platformy Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: c666b7d5b680eab3c990950569868eacdb6f30af
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="nougat-features"></a>Funkcje nugacie

_Jak rozpocząć pracę, aby opracowywać aplikacje dla systemu Android nugacie przy użyciu platformy Xamarin.Android._

Ten artykuł zawiera konspekt funkcje wprowadzone w Android nugacie omówiono sposoby przygotowania do tworzenia aplikacji systemu Android nugacie platformy Xamarin.Android i łącza do przykładowych aplikacji, które ilustrują sposób korzystania z funkcji systemu Android nugacie w Aplikacji platformy Xamarin.Android.


## <a name="overview"></a>Omówienie

[Android nugacie](https://developer.android.com/about/versions/nougat/android-7.0.html) jest monitowania firmy Google do systemu Android marshmallow w wersji 6.0. Xamarin.Android zapewnia obsługę **Android 7.x powiązania** w Xamarin Android 7.0 i nowszych. Android nugacie dodaje wiele nowych interfejsów API funkcji nugacie opisanych poniżej; te interfejsy API są dostępne dla aplikacji platformy Xamarin.Android, gdy używasz 7.0 platformy Xamarin.Android.

[![Obrazy bohater tablety z systemem Android i telefony z systemem Android nugacie](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Aby uzyskać więcej informacji na temat Android 7.x interfejsów API, zobacz [Android 7.1 dla deweloperów](http://developer.android.com/preview/api-overview.html).
Aby uzyskać listę znanych problemów Xamarin.Android 7.0, zobacz [informacje o wersji](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/).

Android nugacie zawiera wiele nowych funkcji środowiska zainteresowań dla deweloperów platformy Xamarin.Android. Te funkcje obejmują:

-   **Obsługa wielu okna** &ndash; to rozszerzenie pozwala użytkownikom na raz otwórz dwóch aplikacji na ekranie.

-   **Ulepszenia powiadomień** &ndash; obejmuje system zmienioną powiadomienia w systemie Android nugacie *bezpośrednie odpowiedzi* funkcji, które umożliwiają użytkownikom szybkie odpowiadanie na wiadomości tekstowych bezpośrednio z powiadomienia INTERFEJS UŻYTKOWNIKA. Także jeśli aplikacja tworzy powiadomienia dla odebranych komunikatów, nowa *powiązane powiadomienia* funkcji można powiązać powiadomienia razem jako pojedynczej grupy po odebraniu więcej niż jeden komunikat.

-   **Oszczędzanie danych** &ndash; ta funkcja jest nowa usługa systemu, która umożliwia ograniczenie użycia danych komórkowych przez aplikacje; zapewnia użytkownikom kontrolę nad jak korzystać z danych komórkowych przez aplikacje.

Ponadto Android nugacie oferuje inne rozszerzenia przydatne dla deweloperów aplikacji, takich jak nowa funkcja konfiguracji zabezpieczeń sieci, Doze w podróży, klucza zaświadczania, nowe szybkie ustawienia interfejsów API, obsługa wielu ustawień regionalnych, ICU4J interfejsów API, widoku sieci Web rozszerzenia dostęp do funkcji języka Java 8, dostęp do katalogu w zakresie API niestandardowego wskaźnika, obsługa VR platform, pliki wirtualne i optymalizacje przetwarzania w tle.

W tym artykule wyjaśniono, jak rozpocząć tworzenie aplikacji za pomocą systemu Android nugacie wypróbowanie nowych funkcji i planowanie migracji lub funkcja pracy pod kątem nowych platformy systemu Android nugacie.


## <a name="requirements"></a>Wymagania

Poniżej jest wymagany do korzystania z nowych funkcji systemu Android nugacie w aplikacjach opartych na platformie Xamarin:

-   **Visual Studio lub Visual Studio for Mac** &ndash; Jeśli używasz programu Visual Studio w wersji 4.2.0.628 lub nowszej programu Xamarin dla Visual Studio jest wymagany. Jeśli używasz programu Visual Studio dla komputerów Mac, wersji 6.1.0 lub nowszej programu Visual Studio dla komputerów Mac jest wymagana.

-   **Xamarin.Android** &ndash; Xamarin.Android w wersji 7.0 lub nowszej, musi być zainstalowana i skonfigurowana przy użyciu programu Visual Studio lub Visual Studio dla komputerów Mac.

-   **Zestaw SDK systemu android** — 7.0 zestawu SDK systemu Android (interfejs API 24) lub nowszym należy zainstalować za pomocą Menedżera zestawu SDK systemu Android.

-   **Java Developer Kit** &ndash; tworzenie aplikacji platformy Xamarin Android 7.0 wymaga [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) lub nowszym, jeśli tworzysz poziom interfejsu API 24 lub większym (JDK 8 obsługuje również poziomy interfejsu API starszych niż 24). 64-bitowej wersji JDK 8 jest wymagany, jeśli używasz Kontrolki niestandardowe lub podglądzie formularzy.

> [!IMPORTANT]
> Xamarin.Android nie obsługuje JDK 9.

Należy pamiętać, że aplikacje musi zostać również przebudowany z Xamarin C6SR4 lub nowszym niezawodny sposób pracy z nugacie systemu Android. Ponieważ nugacie Android można połączyć tylko z [dostarczane do zestawu NDK natywnych bibliotek](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), istniejących aplikacji przy użyciu biblioteki, takich jak **Mono.Data.Sqlite.dll** może ulec awarii podczas uruchamiania na nugacie systemu Android, jeśli nie są poprawnie odbudowane.



## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć korzystanie z platformy Xamarin.Android nugacie systemu Android, musisz pobrać i zainstalować najnowsze narzędzia i pakietów SDK przed utworzeniem projektu systemu Android nugacie:

1.  Zainstaluj najnowsze aktualizacje Xamarin.Android z Xamarin.

2.  Zainstaluj **Android 7.0 (interfejs API 24)** pakietów i narzędzi lub nowszym.

3.  Utwórz nowy projekt platformy Xamarin.Android którego element docelowy nugacie systemu Android.

4.  Skonfiguruj emulator lub urządzenie dla systemu Android nugacie.

Każdy z tych kroków znajduje się w następujących sekcjach:


### <a name="install-xamarin-updates"></a>Zainstaluj aktualizacje Xamarin

Aby dodać obsługę Xamarin Android nugacie, zmienić kanału aktualizacji w programie Visual Studio lub Visual Studio dla komputerów Mac do kanału stabilna i Zastosuj najnowsze aktualizacje. Należy również funkcje, które są obecnie dostępne tylko w kanału alfa lub Beta, możesz przełączyć kanału alfa lub Beta (kanałów alfa i Beta również zapewnić obsługę dla systemu Android 7.x). Aby uzyskać informacje o tym, jak zmienić kanału aktualizacji (wydań), zobacz [Zmiana kanału aktualizacji](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/).



### <a name="install-the-android-sdk"></a>Zainstaluj zestaw SDK systemu Android

Aby utworzyć projekt z Xamarin Android 7.0, Android SDK Manager musi najpierw użyć do zainstalowania **N Android SDK platformy (interfejs API 24)** lub nowszym. Należy również zainstalować najnowszą **narzędzia zestawu SDK systemu Android**:

1.  Uruchom Menedżera zestawu SDK systemu Android (w programie Visual Studio dla komputerów Mac, należy użyć **Narzędzia > Open Android SDK Manager&hellip;**; w programie Visual Studio, użyj **Narzędzia > Android > Android SDK Manager**).

2.  Zainstaluj **Android 7.0 (interfejs API 24)** lub nowszy:

    [![Wybranie pakietów Android 7.0 w Menedżerze zestawu SDK systemu Android](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  Zainstaluj najnowsze narzędzia zestawu SDK systemu Android:

    [![Wybieranie najnowsze narzędzia zestawu SDK systemu Android w programie Android SDK Manager](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Należy zainstalować poprawkę narzędzia zestawu SDK systemu Android 25.2.2 lub nowszym, systemem Android SDK platformy narzędzia 24.0.3 lub nowszy oraz Android kompilacji zestawu SDK 24.0.2 lub nowszym.

4.  Sprawdź, czy **Java Development Kit lokalizacji** jest skonfigurowany dla JDK 1.8:

    [![Konfigurowanie ścieżki JDK 8 w obszarze opcji narzędzi](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Aby wyświetlić to ustawienie w programie Visual Studio, kliknij przycisk **Narzędzia > Opcje > Xamarin > Ustawienia systemu Android**. W programie Visual Studio for Mac, kliknij przycisk **Preferencje > Projekty > Lokalizacje SDK > Android**.



### <a name="start-a-xamarinandroid-project"></a>Uruchom projekt platformy Xamarin.Android

Utwórz nowy projekt platformy Xamarin.Android. Jeśli jesteś nowym użytkownikiem programowanie dla systemu Android za pomocą platformy Xamarin, zobacz [Hello, Android](~/android/get-started/hello-android/index.md) Aby dowiedzieć się więcej o tworzeniu projektów platformy Xamarin.Android.

Podczas tworzenia projektu systemu Android, należy skonfigurować ustawienia wersji do docelowego dla systemu Android w wersji 7.0 lub nowszej. Na przykład, aby skierować je do projektu dla systemu Android 7.0, należy skonfigurować docelowy poziom interfejsu API systemu Android projektu do **Android 7.0 (interfejs API 24 - nugacie)**. Zaleca się, że ustawiony poziom docelowy framework do interfejsu API, 24 lub nowszej. Aby uzyskać więcej informacji o konfigurowaniu poziomów poziom interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md).


> [!NOTE]
> Obecnie należy ustawić **wersji Minimum Android** do **Android 7.0 (interfejs API 24 - nugacie)** do wdrożenia aplikacji na urządzeniach z systemem Android nugacie lub emulatory.



### <a name="configure-an-emulator-or-device"></a>Skonfiguruj Emulator lub urządzenie

Jeśli używasz emulatora, uruchom Menedżera AVD Android i utworzyć nowe urządzenie przy użyciu następujących ustawień:

-   Urządzenia: Węzła 5 X węzła 6, węzła 6P, Player węzła węzła 9 lub pikseli C.
-   Cel: Android 7.0 — poziom interfejsu API 24
-   ABI: x86 lub x86\_64

Na przykład to urządzenie wirtualne jest skonfigurowany do emulowania 6 węzła:

[![Konfigurowanie AVD przy użyciu urządzenia 6 węzła, docelowy z systemem Android 7.0 i Intel Atom x86 procesora CPU/ABI](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Jeśli używasz urządzenia fizycznego, takie jak węzła 5 X, 6 lub 9, można zaktualizować urządzenie do automatycznego za pośrednictwem aktualizacji lotniczego (Stachnio) lub pobranie obrazu systemu i flash urządzenia bezpośrednio. Aby uzyskać więcej informacji na temat ręcznie zaktualizować urządzenie do nugacie systemu Android, zobacz [Stachnio obrazów dla urządzeń z węzła](https://developers.google.com/android/nexus/ota).

Uwaga 5 węzła urządzenia nie są obsługiwane przez nugacie systemu Android.



## <a name="new-features"></a>Nowe funkcje

Android nugacie wprowadzono wiele nowych funkcji i możliwości, takie jak obsługa wielu okna, rozszerzenia powiadomienia i wygaszacz danych. W poniższych sekcjach zaznacz te funkcje i znajdują się linki ułatwiające rozpoczęcie pracy z ich w aplikacji.



### <a name="multi-window-mode"></a>Tryb wielu okna

Tryb wielu okna sprawiają, że użytkownicy będą mogli otwierać dwie aplikacje jednocześnie z obsługą wielozadaniowości pełna. Te aplikacje można uruchomić side-by-side (pozioma) lub jeden powyżej inne (w pionie) w trybie pełnoekranowym podziału.
Użytkownicy będą mogli przeciągać linię podziału między aplikacjami, aby zmienić ich rozmiar, a ich wycinanie i wklejanie zawartości między aplikacjami. Gdy dwie aplikacje są prezentowane w trybie wielu okna, wybrane działanie jest nadal uruchomiona podczas działania niezaznaczone jest wstrzymana, ale nadal widoczny. Tryb wielu okna nie modyfikuje cyklu życia działanie systemu Android.

[![Przykład aplikacji działających w trybie wielu okna zarówno w orientacji pionowej, jak i w orientacji poziomej](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Można skonfigurować sposób działania aplikacji platformy Xamarin.Android obsługuje tryb wielu okna. Na przykład można skonfigurować atrybuty, które ustawić minimalny rozmiar i domyślna wysokość i szerokość aplikacji w trybie wielu okna. Można użyć nowej `Activity.IsInMultiWindowMode` właściwość, aby określić, czy działanie jest w trybie wielu okna. Na przykład:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) Przykładowa aplikacja zawiera kod C#, który demonstruje sposób wykorzystać wiele okien interfejsów użytkownika za pomocą aplikacji.

Aby uzyskać więcej informacji o trybie okna usługi, zobacz [wielu okien obsługi](https://developer.android.com/guide/topics/ui/multi-window.html).



### <a name="enhanced-notifications"></a>Ulepszone powiadomienia

Android nugacie wprowadza system zmienioną powiadomień. Zawiera funkcje nowa funkcja bezpośredniego odpowiedzi, która umożliwia użytkownikom szybkie odpowiedzi na powiadomienia przychodzących wiadomości tekstowych bezpośrednio w powiadomieniu interfejsu użytkownika. Począwszy od systemu Android 7.0, powiadomienie o wiadomości mogą stworzyć pakiet jako pojedynczej grupy po odebraniu więcej niż jeden komunikat. Ponadto deweloperzy można dostosować powiadomień widoków, korzystać z systemu dekoracje w powiadomieniach i korzystać z nowych szablonów powiadomień podczas generowania powiadomień.


#### <a name="direct-reply"></a>Bezpośrednie odpowiedzi

Gdy użytkownik otrzyma powiadomienie o wiadomości przychodzących, Android nugacie umożliwia odpowiedzi na wiadomość w ramach powiadomienia (zamiast otwarcie aplikacji obsługi komunikatów do wysłania odpowiedzi).
Ta funkcja odpowiedzi wbudowanego umożliwia użytkownikom szybkie odpowiadanie na wiadomość SMS lub tekst bezpośrednio z poziomu interfejsu powiadomień:

[![Zrzut ekranu przedstawiający powiadomienie z polem bezpośrednie odpowiedzi wbudowany](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Do obsługi tej funkcji w aplikacji, należy dodać *akcji odpowiedzi wbudowanego* do aplikacji za pośrednictwem [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) obiektów, dzięki czemu użytkownicy mogą wysyłać odpowiedzi przy użyciu tekstu bezpośrednio z powiadomień interfejsu użytkownika.
Na przykład poniższy kod kompilacji `RemoteInput` do odbierania wprowadzania tekstu, tworzy oczekujące celem akcji odpowiedzi, a następnie tworzy akcję zdalną włączone wejściowych:

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

[Usługa wiadomości](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) Przykładowa aplikacja zawiera kod C#, który demonstruje sposób rozszerzyć powiadomienia z `RemoteInput` obiektu. Aby uzyskać więcej informacji o dodawaniu odpowiedzi wbudowanej akcji do aplikacji dla systemu Android 7.0 lub nowszej, zobacz Android [podczas odpowiadania na powiadomienia](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) tematu.


#### <a name="bundled-notifications"></a>Powiązane powiadomienia

Android nugacie można zgrupować komunikatów powiadomień (na przykład w temacie wiadomości) i grupy, a nie oddzielnych wiadomości.
To *powiązane powiadomienia* funkcja umożliwia użytkownikom odrzucić lub archiwum grupy powiadomień w jednej akcji. Użytkownika można Przesuń w dół, aby rozwinąć pakietu powiadomienia, aby wyświetlić szczegóły każdego powiadomienia:

[![Zrzut ekranu przykład powiązane powiadomienia](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Aby obsługiwać powiązane powiadomienia, może używać aplikacja [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) metodę, aby powiązać podobne powiadomienia. Aby uzyskać więcej informacji o grupach powiązane powiadomienia w systemie Android N, zobacz Android [tworzenie pakietów powiadomienia](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) tematu.


#### <a name="custom-views"></a>Widoki niestandardowe

Android nugacie pozwala tworzyć widoki niestandardowe powiadomienie z systemu powiadomień, akcje i rozwijania układów. Aby uzyskać więcej informacji na temat widoków niestandardowych powiadomień w systemie Android nugacie Zobacz Android [ulepszenia powiadomień](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) tematu.



### <a name="data-saver"></a>Oszczędzanie danych

Począwszy od systemu Android nugacie, użytkownicy mogą włączyć nowe *danych wygaszacz* ustawienie, że bloki w tle danych użycia. To ustawienie sygnalizuje również Twojej aplikacji przy użyciu mniejszej ilości danych na pierwszym planie, tam gdzie to możliwe. [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) została rozszerzona w systemie Android nugacie tak, aby aplikację można sprawdzić, czy użytkownik ma włączony wygaszacz danych tak, aby wprowadzić starań, aby ograniczyć jej użycie danych po włączeniu wygaszacza danych aplikacji.

Aby uzyskać więcej informacji o nowej funkcji wygaszacz danych w systemie Android nugacie, zobacz Android [optymalizacji użycia danych sieci](https://developer.android.com/training/basics/network-ops/data-saver.html) tematu.



### <a name="app-shortcuts"></a>Skróty do aplikacji

Android 7.1 wprowadzone *skróty aplikacji* funkcja, która umożliwia użytkownikom szybko start wspólnej lub zalecanych zadań z aplikacją.
Aby aktywować menu skrótów, long naciśnięcie przez użytkownika ikony aplikacji na kilka sekund &ndash; z szybkiego wibrację w wyświetlonym menu.
Zwalnianie naciśnij powoduje, że menu będzie:

[![Przykładowy ekran menu skrótów aplikacji dla aplikacji obsługi wiadomości](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Ta funkcja jest dostępna tylko poziom interfejsu API 25 lub większą.
Aby uzyskać więcej informacji o nowej funkcji skróty do aplikacji w systemie Android 7.1, zobacz Android [skróty aplikacji](https://developer.android.com/guide/topics/ui/shortcuts.html) tematu.


### <a name="sample-code"></a>Przykładowy kod

Kilka przykładów Xamarin.Android są dostępne dla pokazano, jak korzystać z funkcji systemu Android nugacie:

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) zademonstrowano użycie dostępne w systemie Android nugacie API wielu okna. Przykładową aplikację można przełączyć w trybie usługi systemu windows, aby zobaczyć, jak wpływa na cyklem życia aplikacji i zachowania.

-   [Usługa komunikatów](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) używa prostego usługa, która wysyła powiadomienia `NotificationCompatManager`. Rozszerza też powiadomienie z `RemoteInput` obiektu na urządzeniach Android nugacie odpowiedzi przy użyciu tekstu bezpośrednio z powiadomienia bez konieczności otwierania aplikacji.

-   [Powiadomienia Active](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) przedstawiono sposób użycia `NotificationManager` interfejsu API, aby sprawdzić, jak wiele powiadomień aplikacji są obecnie wyświetlane.

-   [Zakres dostępu do katalogów](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) pokazano, jak na potrzeby dostępu do katalogu zakresami interfejsu API łatwo uzyskiwać dostęp do określonych katalogów. Służy zamiast konieczności Zdefiniuj `READ_EXTERNAL_STORAGE` lub `WRITE_EXTERNAL_STORAGE` uprawnienia w manifeście.

-   [Bezpośrednie rozruchu](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) przedstawiono sposób przechowywania danych w magazynie urządzenie zaszyfrowane, który zawsze jest dostępny, gdy urządzenie rozruchu zarówno przed i po wprowadzeniu credentials(PIN/Pattern/Password) dowolnego użytkownika.


## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono Android nugacie i wyjaśniono, jak zainstalować i skonfigurować najnowsze narzędzia i pakietów do tworzenia aplikacji platformy Xamarin.Android na nugacie systemu Android. Dostępne również omówienie najważniejsze funkcje dostępne w Android nugacie wraz z łączami do kodu źródłowego przykładu, aby pomóc Ci rozpocząć tworzenie aplikacji dla systemu Android nugacie.


## <a name="related-links"></a>Linki pokrewne

- [Android 7.1 dla deweloperów](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Informacje o wersji dla systemu Xamarin Android 7.0](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
