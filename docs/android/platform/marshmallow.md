---
title: Funkcje marshmallow
description: "Ten artykuł ułatwia rozpoczęcie pracy przy użyciu w celu opracowywania aplikacji dla systemu Android Marshmallow 6.0 przy użyciu platformy Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: b28ca68701394a8b7b0b543a5ae646910e7c8361
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="marshmallow-features"></a>Funkcje marshmallow

_Ten artykuł ułatwia rozpoczęcie pracy przy użyciu w celu opracowywania aplikacji dla systemu Android Marshmallow 6.0 przy użyciu platformy Xamarin.Android._

Ten artykuł zawiera omówienie nowych funkcji systemu Android Marshmallow 6.0, omówiono sposoby przygotowania do tworzenia aplikacji systemu Android Marshmallow platformy Xamarin.Android i zawiera łącza do aplikacji przykładowej, które pokazano, jak korzystać z nowego systemu Android Marshmallow funkcje w aplikacji platformy Xamarin.Android. 

<a name="overview" />

## <a name="overview"></a>Omówienie

[System android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html), jest dalej Android głównych wersji po Android interfejs typu lizak.
Xamarin.Android obsługuje system Android Marshmallow i obejmuje:

-   **Interfejs API 23/Android 6.0 powiązania** &ndash; Android 6.0 dodaje wiele nowych interfejsów API dla nowych funkcji opisanych poniżej; te interfejsy API są dostępne dla aplikacji platformy Xamarin.Android, jeśli zostanie rozpoczęta 23 poziom interfejsu API. Aby uzyskać więcej informacji na temat Android 6.0 interfejsów API, zobacz [Android 6.0 interfejsów API](http://developer.android.com/preview/api-overview.html). 

[![Obrazy bohater tablety i telefony z systemem Marshmallow](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png)

Mimo że wersji Marshmallow koncentruje się głównie na "Polski i jakości", umożliwia także wiele nowych funkcji środowiska zainteresowań dla deweloperów platformy Xamarin.Android. Te funkcje obejmują: 

-   **Środowisko uruchomieniowe uprawnienia** &ndash; to rozszerzenie pozwala użytkownikom na zatwierdzenie uprawnień zabezpieczeń w przypadku w czasie wykonywania. 

-   **Ulepszenia uwierzytelniania** &ndash; począwszy od systemu Android Marshmallow, aplikacje można teraz używać czujników odcisk palca do uwierzytelniania użytkowników i nowy *potwierdzenie poświadczeń* funkcji minimalizuje konieczność wprowadzania hasła. 

-   **Łączenie aplikacji** &ndash; ta funkcja pozwala wyeliminować konieczność **selektora aplikacji** wyskakującego okienka przez automatyczne kojarzenie aplikacji z domen sieci web. 

-   **Bezpośrednie udziału** &ndash; można zdefiniować *bezpośrednie elementy docelowe udziału* ułatwiających udostępnianie szybki i intuicyjne dla użytkowników; ta funkcja umożliwia uers na współużytkowanie zawartości z innych aplikacji. 

-   **Głosowych interakcje** &ndash; ten nowy interfejs API umożliwia tworzenie funkcje konwersacji głosowego w swojej aplikacji. 

-   **Tryb wyświetlania K 4** &ndash; w systemie Android Marshmallow, aplikacja może zażądać 4 K rozdzielczości ekranu na sprzęcie, który go obsługuje. 

-   **Nowe funkcje Audio** &ndash; począwszy od Marshmallow systemu Android teraz obsługuje protokół MIDI. Zapewnia także nowe klasy do tworzenia cyfrowych rejestracji dźwięku i odtwarzania obiektów i oferuje nowy interfejs API punkty zaczepienia kojarzenia urządzenia audio i wejściowych. 

-   **Nowe funkcje wideo** &ndash; Marshmallow zapewnia nowa klasa, która pomaga aplikacji renderowania strumieni audio i wideo w synchronizacji; Klasa ta oferuje również obsługę szybkość odtwarzania dynamicznych. 

-   **Android for Work** &ndash; Marshmallow zawiera formanty rozszerzonego dla urządzeń należących do firmy, jednego użytkownika. Obsługuje instalacji dyskretnej, a Dezinstalacja aplikacji przez właściciela urządzenia, auto akceptację aktualizacji systemu, zarządzania certyfikatami ulepszone śledzenia użycia danych, uprawnienia zarządzania i pracy powiadomienia o stanie. 

-   **Biblioteka obsługi projektu materiału** &ndash; nowy *Biblioteka obsługi projektu* zawiera składniki projektu i wzorce, które ułatwia tworzenie projektu materiałów wygląd i działanie w aplikacji. 

Ponadto wiele aktualizacji biblioteki systemu Android core zostały wydane z systemem Android M, a te aktualizacje zapewniają nowe funkcje dla systemu Android M i wcześniejszych wersjach systemu android.

Ponadto wiele aktualizacji biblioteki systemu Android core zostały wydane z systemem Android Marshmallow, a te aktualizacje zapewniają nowe funkcje dla systemu Android Marshmallow i wcześniejszych wersjach systemu android. W tym artykule wyjaśniono, jak rozpocząć tworzenie aplikacji za pomocą systemu Android Marshmallow i zapewnia wyróżnia omówienie nowych funkcji w systemie Android w wersji 6.0. 


<a name="requirements" />

## <a name="requirements"></a>Wymagania

Poniżej jest wymagany do korzystania z nowych funkcji systemu Android Marshmallow w aplikacjach opartych na platformie Xamarin: 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 lub później musi być zainstalowana i skonfigurowana z programu Visual Studio lub Xamarin Studio.

-   **Visual Studio for Mac** lub **programu Visual Studio** &ndash; przy użyciu programu Visual Studio dla komputerów Mac, wersja 5.9.7.22 lub nowszy jest wymagany. Jeśli używasz programu Visual Studio w wersji 3.11.1537 lub nowszej narzędzi platformy Xamarin dla Visual Studio jest wymagany. 

-   **Zestaw SDK systemu android** &ndash; Android SDK 6.0 (23 interfejsu API) lub nowszym należy zainstalować za pomocą Menedżera zestawu SDK systemu Android.

-   **Java Developer Kit** &ndash; wymaga platformy Xamarin.Android [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) lub nowszym, jeśli tworzysz poziom interfejsu API 24 lub większym (JDK 1.8 obsługuje również poziomy interfejsu API starszych niż 24, w tym Marshmallow). 64-bitowej wersji JDK 1.8 jest wymagany, jeśli używasz Kontrolki niestandardowe lub podglądzie formularzy.

Można nadal używać [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Jeśli tworzenie specjalnie dla interfejsu API na poziomie 23 lub starszym. 

<a name="gettingstarted" />

## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć korzystanie z platformy Xamarin.Android Android Marshmallow, musisz pobrać i zainstalować najnowsze narzędzia i pakietów SDK przed utworzeniem projektu systemu Android Marshmallow: 

1.  Zainstaluj najnowsze aktualizacje Xamarin z **stabilna** kanału. 

2.  Zainstaluj zestaw SDK systemu Android Marshmallow 6.0 pakietów i narzędzia.

3.  Utwórz nowy projekt platformy Xamarin.Android którego element docelowy dla systemu Android Marshmallow 6.0 (23 poziom interfejsu API). 

4.  Skonfiguruj emulator lub urządzenie dla systemu Android Marshmallow.

Każdy z tych kroków znajduje się w następujących sekcjach:

<a name="updates" />

### <a name="install-xamarin-updates"></a>Zainstaluj aktualizacje Xamarin

Aby zaktualizować program Xamarin, co zawierają one obsługę systemu Android Marshmallow 6.0, zmień kanału aktualizacji **stabilna** i zainstaluj wszystkie aktualizacje. Aby uzyskać więcej informacji o instalowaniu aktualizacji z kanału aktualizacji, zobacz [zmienić kanału aktualizacji](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/). 

<a name="sdkpreview" />

### <a name="install-the-android-60-sdk"></a>Zainstaluj zestaw SDK systemu Android 6.0

Aby utworzyć projekt platformy Xamarin.Android for Android Marshmallow, musi najpierw użyć Android SDK Manager można zainstalować zestawu SDK systemu Android 6.0:

-   Uruchom Menedżera zestawu SDK systemu Android (w programie Visual Studio dla komputerów Mac, należy użyć **Narzędzia > SDK Manager**; w programie Visual Studio, użyj **Narzędzia > Android > Android SDK Manager**) i zainstaluj najnowsze narzędzia zestawu SDK dla systemu Android:

    [![Wybieranie narzędzia zestawu SDK systemu Android w programie Android SDK Manager](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png)

-   Ponadto zainstalowanie najnowszych **Android 6.0** pakietów SDK:

    [![Wybieranie zestawu SDK systemu Android 6.0 pakietów w Menedżerze zestawu SDK systemu Android](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png)

Należy zainstalować poprawkę narzędzia zestawu SDK systemu Android 24.3.4 lub nowszym.
Aby uzyskać więcej informacji o używaniu Android SDK Manager można zainstalować zestawu SDK systemu Android 6.0, zobacz [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html).


<a name="xaproject" />

### <a name="start-a-xamarinandroid-project"></a>Uruchom projekt platformy Xamarin.Android

Utwórz nowy projekt platformy Xamarin.Android. Jeśli jesteś nowym użytkownikiem programowanie dla systemu Android za pomocą platformy Xamarin, zobacz [Hello, Android](~/android/get-started/hello-android/index.md) Aby dowiedzieć się więcej o tworzeniu projektów dla systemu Android. 

Podczas tworzenia projektu systemu Android, należy skonfigurować ustawienia wersji do docelowego dla systemu Android marshmallow w wersji 6.0. Aby skierować je do projektu dla systemu Marshmallow, należy skonfigurować projekt dla **interfejsu API na poziomie 23 (Xamarin.Android 6.0 pomocy technicznej)**. Aby uzyskać więcej informacji o konfigurowaniu poziomów poziom interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md).


<a name="emudev" />

### <a name="configure-an-emulator-or-device"></a>Skonfiguruj Emulator lub urządzenie

Jeśli używasz emulatora, uruchom Menedżera AVD Android i utworzyć nowe urządzenie przy użyciu następujących ustawień:

-   Urządzenia: Węzła 5, 6 lub 9.
-   Cel: Android 6.0 — interfejs API na poziomie 23
-   ABI: x86

Na przykład to urządzenie wirtualne jest skonfigurowany do emulowania 5 węzła:

[![Konfigurowanie AVD przy użyciu węzła 5 urządzeń, systemem Android 6.0 docelowych i Intel Atom (x86)](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png)

Jeśli używasz urządzenia fizycznego, takie jak 5 węzła 6 lub 9, można zainstalować obraz podglądu dla systemu Android Marshmallow. Aby uzyskać więcej informacji o aktualizowaniu urządzenia do systemu Android Marshmallow, zobacz [obrazów systemu sprzętu](http://developer.android.com/preview/download.html#images).


<a name="newfeatures" />

## <a name="new-features"></a>Nowe funkcje

Wiele zmiany wprowadzone w systemie Android Marshmallow są koncentruje się na poprawę środowiska użytkownika dla systemu Android, zwiększenie wydajności i naprawiania błędów. Jednak Marshmallow również wprowadzić niektóre rozległych zmian podstaw dotyczących platformy systemu Android. W poniższych sekcjach zaznacz te ulepszenia i zawierają łącza do ułatwiające rozpoczęcie pracy przy użyciu nowych funkcji systemu Android Marshmallow w aplikacji. 


<a name="permissions" />

### <a name="runtime-permissions"></a>Uprawnienia w czasie wykonywania

System Android uprawnienia zostały znacznie zoptymalizowanych pod kątem i uproszczony od systemu Android interfejs typu lizak. W systemie Android Marshmallow użytkowników przydziel uprawnienia na podstawie przez przypadek, w czasie wykonywania, a nie na zainstalowanie. Do obsługi tej funkcji w systemie Android Marshmallow i nowsze, projektowania aplikacji w celu monitowania użytkownika o uprawnienia w czasie wykonywania (w przypadku, gdy są potrzebne uprawnienia). Ta zmiana ułatwia użytkownikom rozpocząć korzystanie z aplikacji natychmiast, ponieważ jest on usprawnia proces instalowania i uaktualniania aplikacji. 

Zobacz [żąda uprawnienia środowiska uruchomieniowego w systemie Android Marshmallow](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) więcej szczegółów (wraz z przykładami kodu) o implementacji uprawnienia środowiska uruchomieniowego w aplikacji platformy Xamarin.Android.
Xamarin udostępnia również przykładową aplikację, która ilustruje działanie uprawnienia środowiska uruchomieniowego w systemie Android Marshmallow (i nowszych): [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions).

Ta przykładowa aplikacja przedstawiono poniżej:

-   Jak sprawdzić i zażądać uprawnień w czasie wykonywania.
-   Jak zadeklarować uprawnienia dla urządzeń z systemem Android M.

Aby użyć tej aplikacji przykładowej:

1.  Wybierz **aparatu** lub **kontaktów** przyciski, aby wyświetlić uprawnienia żądania okna dialogowego.
2.  Przyznaj uprawnienia do wyświetlania fragmenty aparat fotograficzny lub kontaktów.

Aby uzyskać więcej informacji o nowych funkcjach uprawnienia środowiska uruchomieniowego w systemie Android Marshmallow, zobacz [Praca z uprawnieniami systemu](https://developer.android.com/preview/features/runtime-permissions.html).


<a name="authentication" />

### <a name="authentication-enhancements"></a>Ulepszenia uwierzytelniania

Android Marshmallow zawiera dwa rozszerzenia uwierzytelniania, które wyeliminować potrzebę hasła:

-   **Odcisków palców uwierzytelniania** &ndash; używa skanowania odcisk palca do uwierzytelniania użytkowników.

-   **Potwierdzenie poświadczeń** &ndash; uwierzytelnia użytkowników oparte na jak długo urządzenie zostało odblokowane.

Łącza i przykładowe aplikacje opisane w dalszej części może ułatwić stają się znanym te nowe funkcje.


<a name="fingerprint" />

#### <a name="fingerprint-authentication"></a>Odcisk palca uwierzytelniania

Na urządzeniach, które obsługują skanowanie sprzętu linii papilarnych, można użyć nowej `FingerPrintManager` klasy w celu uwierzytelnienia użytkownika.
Aby uzyskać więcej informacji o funkcji uwierzytelniania odcisku palca w systemie Android Marshmallow, zobacz [odcisku palca uwierzytelniania](https://developer.android.com/preview/api-overview.html#fingerprint-authentication).

Xamarin udostępnia przykładowej aplikacji, która ilustruje sposób używania palców w zarejestrowany do uwierzytelnienia użytkownika w aplikacji: [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog).

Aby użyć tej aplikacji przykładowej:

1.  Touch **zakupu** przycisk, aby otworzyć okno dialogowe odcisku palca uwierzytelniania.
2.  Skanowanie w zarejestrowany odcisku palca uwierzytelniania.

Należy pamiętać, że to przykładowa aplikacja wymaga podłączenia urządzenia z czytnikiem linii papilarnych.
Ta aplikacja nie przechowuje odcisku palca (lub hasło).


<a name="voice" />

#### <a name="voice-interactions"></a>Interakcje głosu

Nowa funkcja interakcje głosu wprowadzona w systemie Android Marshmallow umożliwia użytkownikom aplikacji użyj głosu, aby potwierdzić akcje, a następnie wybierz z listy opcji. Aby uzyskać więcej informacji na temat interakcji głosu, zobacz [omówienie API interakcji głosu](https://developers.google.com/voice-actions/interaction/). 

Zobacz [dodać konwersacji do aplikacji systemu Android z interakcje głosu](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) więcej szczegółów (wraz z przykładami kodu) o implementacji głosu interakcji w aplikacji platformy Xamarin.Android.
Przykładowa aplikacja jest dostępna który ilustruje sposób użycia interfejsu API interakcji głosowe w aplikacji platformy Xamarin.Android: [interakcje głosu](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).


<a name="confirmcred" />

#### <a name="confirm-credential"></a>Potwierdzenie poświadczeń

Za pomocą nowej *potwierdzenie poświadczeń* funkcji systemu Android Marshmallow można zwolnić użytkowników mających do zapamiętania i wprowadź hasła dla aplikacji uwierzytelniający je w oparciu o czas swojego urządzenia został odblokowany.
Aby to zrobić, należy użyć nowego `SetUserAuthenticationValidityDurationSeconds` metody `KeyGenerator`. Użyj `KeyGuardManager`w `CreateConfirmDeviceCredentialIntent` metody ponownego uwierzytelnienia użytkownika w aplikacji. Aby uzyskać więcej informacji o tej nowej funkcji systemu Android Marshmallow, zobacz [potwierdzenie poświadczeń](https://developer.android.com/preview/api-overview.html#confirm-credential).

Xamarin udostępnia przykładowej aplikacji, która ilustruje sposób używania poświadczeń urządzenia (takich jak numer PIN, wzorzec lub hasło) w aplikacji: [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

Aby użyć tej aplikacji przykładowej:

1.  Konfiguracja bezpiecznego blokady ekranu na urządzeniu (**bezpiecznego > Zabezpieczenia > Screenlock**).
2.  Wybierz **zakupu** przycisk i Potwierdź poświadczenia bezpiecznej blokady ekranu.


<a name="chrometabs" />

### <a name="chrome-custom-tabs"></a>Niestandardowe karty Chrome

Deweloperzy aplikacji stają przed wybór, gdy użytkownik naciska adresu URL: aplikacji można uruchomić przeglądarki sieci lub przy użyciu przeglądarki w aplikacji na podstawie `WebView`. Obie te opcje stanowi wyzwanie &ndash; uruchamianie przeglądarki jest mocno kontekstu przełącznikiem, który nie jest modyfikowalny, podczas `WebView`s nie mają stanu za pośrednictwem przeglądarki. Ponadto użycie `WebView`s można dodać obsługi dodatkowe obciążenie. 

*Chromowana kart niestandardowych* umożliwia łatwe i które wyświetlić witryn sieci Web dzięki możliwościom Chrome bez użytkowników pozostaw aplikacji. Ta funkcja umożliwia to aplikacji większą kontrolę nad środowiskiem sieci web użytkownika; to, że przejścia między macierzystego i zawartość, aby usprawnić sieci web bez konieczności odwołać się do `WebView`. Aplikacji może wpłynąć na sposób Chrome wyszukuje i tak dostosowując następujące: 

-   Kolor paska narzędzi

-   Włączanie i wyłączanie animacji

-   Akcje niestandardowe w menu paska narzędzi i przepełnienie Chrome

-   Przed uruchomieniem Chrome i zawartości wstępnie pobrać (na szybsze ładowania)

Aby móc korzystać z tej funkcji w aplikacji platformy Xamarin.Android, Pobierz i zainstaluj [biblioteki karty niestandardowe Obsługa systemu Android](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/).
Aby uzyskać więcej informacji na temat tej funkcji, zobacz [kart niestandardowych Chrome](https://developer.chrome.com/multidevice/android/customtabs).


<a name="designlib" />

### <a name="material-design-support-library"></a>Biblioteka obsługi materiałów projektu

Interfejs typu lizak android wprowadzone [projektowania materiałów](http://www.google.com/design/spec/material-design/introduction.html) jako nowy język projektu Aby odświeżyć Praca w systemie Android (zobacz [motyw materiałów](~/android/user-interface/material-theme.md) informacji o używaniu materiału projektu w aplikacji platformy Xamarin.Android). Z systemem Android Marshmallow wprowadzone Google *biblioteki obsługi systemu Android projekt* ułatwiają aplikacji deweloperom przyjmuje materiałów projektowania wyglądu i działania. Ta biblioteka zawiera następujące składniki:

-   **CoordinatorLayout** &ndash; nowy `CoordinatorLayout` elementu widget jest podobny do jednak większe znaczenie niż `FrameLayout`. Można użyć `CoordinatorLayout` jako kontener dla widoków podrzędnych lub układu najwyższego poziomu który zapewnia `layout_anchor` atrybutu, który może służyć do widoków zakotwiczenia względem innych widoków.

-   **Zwijanie paski narzędzi** &ndash; nowy `CollapsingToolbarLayout` jest zwijanie paska aplikacji, która jest otoki dla `Toolbar`. (Należy pamiętać, że *paska aplikacji* jest, co zostało wcześniej nazywane *pasku akcji*.)

-   **Liczby zmiennoprzecinkowe przycisku akcji** &ndash; round przycisku, który określa akcji głównej w interfejsie użytkownika aplikacji.

-   **Przestawne edycji tekstu etykiety** &ndash; używa nowego `TextInputLayout` elementu widget (który opakowuje `EditText`) aby wyświetlić przestawne etykietę, gdy wskazówkę jest ukryty, gdy tekst danych wejściowych użytkownika.

-   **Widok nawigacji** &ndash; nowe `NavigationView` elementu widget ułatwiają korzystanie z szuflady nawigacji w sposób ułatwiający nawigowania.

-   **Snackbar** &ndash; nowe `SnackBar` widget jest (podobnie jak wyskakujące) mechanizm lekkie opinii, który wyświetla krótki komunikat w dolnej części ekranu, znajdujących się nad wszystkimi innymi elementami na ekranie.

-   **Karty materiału** &ndash; nowe `TabLayout` elementu widget zapewnia układzie poziomym wyświetlanie kart jako sposób implementowania nawigacji najwyższego poziomu w aplikacji.

Aby móc korzystać z [Biblioteka obsługi projektu](http://developer.android.com/tools/support-library/features.html#design) w aplikacji platformy Xamarin.Android, Pobierz i zainstaluj program Xamarin [projektu biblioteki obsługi Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) pakietu NuGet.

Zobacz [doskonałych projektowania materiału z biblioteki projektu Obsługa systemu Android](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) więcej szczegółów (wraz z przykładami kodu) dotyczące korzystania z biblioteki obsługi projektowania materiałów w aplikacji platformy Xamarin.Android.
Xamarin udostępnia przykładowej aplikacji, która demos nowej biblioteki projektu systemu Android na Xamarin.Android &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare).
W tym przykładzie przedstawiono następujące funkcje biblioteki projektu:


-   Zwijanie paska narzędzi
-   Liczby zmiennoprzecinkowe przycisku akcji
-   Zakotwiczanie widoku
-   NavigationView
-   Snackbar

Aby uzyskać więcej informacji na temat biblioteki projektu, zobacz [biblioteki obsługi systemu Android projekt](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) w blog deweloperów systemu Android.


<a name="libraries" />

### <a name="additional-library-updates"></a>Biblioteka dodatkowe aktualizacje

Oprócz systemu Android Marshmallow Google ogłosiła powiązane aktualizacje do kilku podstawowe biblioteki systemu Android. Program Xamarin obsługuje platformy Xamarin.Android te aktualizacje za pośrednictwem kilku pakietów NuGet w wersji zapoznawczej: 

-   [Usług Google Play](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; najnowszą wersję usług Google Play zawiera nowe *zaprasza aplikacji* funkcji, która umożliwia użytkownikom udostępnianie aplikacji swoich znajomych. Aby uzyskać więcej informacji na temat tej funkcji, zobacz [Reach rozwiń Twojej aplikacji z zaprasza aplikacji firmy Google](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/). 

-   [Biblioteki obsługi systemu android](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; NuGets te udostępniają funkcje, które są dostępne tylko dla biblioteki interfejsów API, zapewniając zapewniającej wersje interfejsów API platformy systemu Android. 

-   [Biblioteka systemu android Wearable](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; NuGet ta obejmuje powiązań usług Google Play. Najnowszą wersję biblioteki wearable wprowadzono nowe funkcje (w tym ułatwiające nawigację dla aplikacji niestandardowych) do platformy systemu Android zużycia. 


## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono systemu Android Marshmallow i wyjaśniono, jak zainstalować i skonfigurować na Marshmallow najnowsze narzędzia i pakietów do tworzenia aplikacji platformy Xamarin.Android. Dostępne również omówienie najbardziej atrakcyjne funkcje systemu Android Marshmallow do tworzenia aplikacji platformy Xamarin.Android.


## <a name="related-links"></a>Linki pokrewne

- [System android Marshmallow 6.0](http://developer.android.com/about/versions/marshmallow/index.html)
- [Pobierz zestaw SDK systemu Android](https://developer.android.com/sdk/index.html#Other)
- [Omówienie funkcji](https://developer.android.com/preview/api-overview.html)
- [Uwagi do wersji](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (przykład)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (przykład)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (przykład)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
