---
title: Funkcje marshmallow
description: Ten artykuł pomoże Ci rozpocząć korzystanie z przy użyciu platformy Xamarin.Android, aby programować aplikacje dla systemu Android Marshmallow 6.0.
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 4f0bcd25662d3def719a89ccf833e845eb1728f2
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242241"
---
# <a name="marshmallow-features"></a>Funkcje marshmallow

_Ten artykuł pomoże Ci rozpocząć korzystanie z przy użyciu platformy Xamarin.Android, aby programować aplikacje dla systemu Android Marshmallow 6.0._

Ten artykuł zawiera omówienie nowych funkcji w systemie Android Marshmallow 6.0, omówiono sposoby przygotowania Xamarin.Android do tworzenia aplikacji dla systemu Android Marshmallow i zawiera linki do przykładowych aplikacji, które ilustrują sposób korzystania z tych nowych dla systemu Android Marshmallow funkcje w aplikacji platformy Xamarin.Android. 


## <a name="overview"></a>Omówienie

[System android Marshmallow 6.0](http://developer.android.com/about/versions/marshmallow/index.html), jest dalej Android głównej wersji po systemie Android Lollipop.
Platforma Xamarin.Android obsługuje system Android Marshmallow i obejmuje:

-   **Interfejsu API 23/Android 6.0 powiązania** &ndash; system Android 6.0 dodaje wiele nowych interfejsów API dla nowych funkcji opisanych poniżej; te interfejsy API są dostępne dla aplikacji platformy Xamarin.Android, gdy docelowy poziom interfejsu API 23. Aby uzyskać więcej informacji na temat interfejsy API systemu Android w wersji 6.0, zobacz [interfejsy API systemu Android 6.0](http://developer.android.com/preview/api-overview.html). 

[![Tablety i telefony z systemem Marshmallow obrazy Hero](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Mimo że wydanie Marshmallow koncentruje się głównie na "Polski i jakości", umożliwia także wiele nowych funkcji przydatnych deweloperom platformy Xamarin.Android. Te funkcje obejmują: 

-   **Uprawnieniach w środowisku uruchomieniowym** &ndash; to rozszerzenie umożliwia użytkownikom zatwierdzić uprawnienia zabezpieczeń w przypadku, w czasie wykonywania. 

-   **Ulepszenia uwierzytelniania** &ndash; począwszy od systemu Android Marshmallow, aplikacje mogą teraz używać odcisków palców czujników do uwierzytelniania użytkowników i nową *potwierdzenie poświadczeń* funkcji minimalizuje potrzebę wprowadzania hasła. 

-   **Łączenie aplikacji** &ndash; ta funkcja pozwala wyeliminować konieczność **selektor aplikacji** wyskakujące okienko, automatyczne kojarzenie aplikacji z domeny sieci web. 

-   **Bezpośrednie udziału** &ndash; można zdefiniować *bezpośrednie obiekty docelowe udziału* ułatwiających udostępnianie szybki i intuicyjne dla użytkowników; ta funkcja pozwala użytkowników na potrzeby udostępniania zawartości w innych aplikacjach. 

-   **Interakcje głosowe** &ndash; ten nowy interfejs API umożliwia tworzenie funkcji głosowych konwersacji w swojej aplikacji. 

-   **Tryb wyświetlania usługi 4 K** &ndash; w systemie Android Marshmallow, aplikacja może żądać 4 K rozdzielczość ekranu na sprzęcie, który ją obsługuje. 

-   **Nowe funkcje Audio** &ndash; począwszy od Marshmallow systemu Android teraz obsługuje protokół MIDI. Zapewnia także nowe klasy do tworzenia cyfrowego przechwytywania audio i odtwarzania obiektów i oferuje nowy interfejs API punkty zaczepienia kojarzenia urządzenia audio i danych wejściowych. 

-   **Nowe funkcje wideo** &ndash; Marshmallow zawiera nową klasę, która ułatwia aplikacjom renderowania strumienie audio i wideo w synchronizacji; ta klasa dostarcza również wsparcie dla szybkości odtwarzania dynamicznych. 

-   **W programie android for Work** &ndash; Marshmallow zawiera rozszerzone formanty dla urządzeń należących do firmy, pojedynczego użytkownika. Go obsługuje instalacji dyskretnej i Dezinstalacja aplikacji przez właściciela urządzenia, akceptacji automatyczne aktualizacje systemu, zarządzania certyfikatami ulepszone, śledzenie użycia danych, zarządzanie uprawnieniami i powiadomienia o stanie pracy. 

-   **Materiału projektowania Support Library** &ndash; nowy *projektowania Support Library* umożliwia projektowanie składników i wzorców, które ułatwia tworzenie Material Design wyglądu i działania w swojej aplikacji. 

Ponadto wiele aktualizacji programu podstawowej biblioteki systemu Android zostały wydane z systemem Android M, a te aktualizacje zapewniają nowe funkcje dla systemu Android M i wcześniejszych wersjach systemu Android.

Ponadto wiele aktualizacji programu podstawowej biblioteki systemu Android zostały wydane z systemem Android Marshmallow, a te aktualizacje zapewniają nowe funkcje dla systemu Android Marshmallow i wcześniejszych wersjach systemu Android. W tym artykule wyjaśniono, jak zacząć tworzyć aplikacje z systemem Android Marshmallow i zapewnia, że omówienie nowych funkcji wyróżnia w systemie Android w wersji 6.0. 

## <a name="requirements"></a>Wymagania

Następujące wymagane jest wprowadzenie nowych funkcji systemu Android Marshmallow w aplikacjach opartych na środowisku Xamarin: 

-   **Platforma Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 lub nowszy musi być zainstalowane i skonfigurowane za pomocą programu Visual Studio lub Xamarin Studio.

-   **Program Visual Studio for Mac** lub **programu Visual Studio** &ndash; Jeśli używasz programu Visual Studio dla komputerów Mac, wersja 5.9.7.22 lub nowszy jest wymagany. Jeśli używasz programu Visual Studio w wersji 3.11.1537 lub nowszym z narzędzia środowiska Xamarin dla programu Visual Studio jest wymagany. 

-   **Zestaw SDK systemu android** &ndash; system Android 6.0 zestawu SDK (interfejsu API 23) lub nowszym należy zainstalować za pośrednictwem Menedżera zestawów Android SDK.

-   **Zestaw Java Developer Kit** &ndash; platforma Xamarin.Android wymaga [zestaw JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) lub nowszy, jeśli tworzysz, poziom interfejsu API, 24 lub nowszej (zestaw JDK 1.8 obsługuje również poziomy interfejsu API starszych niż 24, w tym Marshmallow). 64-bitowej wersji zestawu JDK 1.8 jest wymagany, jeśli używasz kontrolek niestandardowych lub podglądzie formularzy.

Będzie można kontynuować używanie [zestaw JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) przypadku opracowywania specjalnie dla poziom 23 interfejsu API lub starszym. 


## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć korzystanie z systemem Android Marshmallow z rozszerzeniem Xamarin.Android, należy pobrać i zainstalować najnowsze narzędzia i pakiety zestawu SDK, zanim będzie można utworzyć projekt dla systemu Android Marshmallow: 

1.  Zainstaluj najnowsze aktualizacje platformy Xamarin z **stabilne** kanału. 

2.  Zainstaluj pakiety zestawu SDK systemu Android Marshmallow 6.0 i narzędzi.

3.  Utwórz nowy projekt platformy Xamarin.Android przeznaczonych dla systemu Android Marshmallow 6.0 (poziom interfejsu API 23). 

4.  Skonfiguruj emulatora lub urządzenia dla systemu Android Marshmallow.

Każdy z tych kroków zostało wyjaśnione w poniższych sekcjach:


### <a name="install-xamarin-updates"></a>Zainstaluj aktualizacje platformy Xamarin

Aby zaktualizować Xamarin, aby obejmowała pomocy technicznej dla systemu Android Marshmallow 6.0, zmienić kanał aktualizacji do **stabilne** i zainstaluj wszystkie aktualizacje. Aby uzyskać więcej informacji o instalowaniu aktualizacji z kanału aktualizacji, zobacz [zmienić kanału aktualizacji](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel). 


### <a name="install-the-android-60-sdk"></a>Instalowanie zestawu SDK systemu Android 6.0

Do utworzenia projektu platformy Xamarin.Android dla systemu Android Marshmallow, należy najpierw użyć Menedżera zestawów Android SDK do zainstalowania zestawu SDK systemu Android 6.0:

-   Uruchom Menedżer zestawów SDK systemu Android (w programie Visual Studio dla komputerów Mac, należy użyć **Narzędzia > Menedżer zestawów SDK**; w programie Visual Studio, użyj **Narzędzia > Android > Menedżer zestawów SDK**) i zainstalować najnowszą Android SDK Tools:

    [![Wybieranie narzędzi zestawu SDK systemu Android w menedżera zestawów Android SDK](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

-   Ponadto, zainstaluj najnowszą wersję **system Android 6.0** pakiety zestawu SDK:

    [![Wybranie pakietów zestawu SDK systemu Android 6.0 w menedżera zestawów Android SDK](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Należy zainstalować poprawkę Android SDK Tools 24.3.4 lub nowszej.
Aby uzyskać więcej informacji na temat Instalowanie zestawu SDK systemu Android 6.0 za pomocą Menedżera zestawów Android SDK, zobacz [menedżera zestawów SDK](http://developer.android.com/tools/help/sdk-manager.html).



### <a name="start-a-xamarinandroid-project"></a>Uruchom projekt platformy Xamarin.Android

Utwórz nowy projekt platformy Xamarin.Android. Jeśli jesteś nowym użytkownikiem opracowywania aplikacji systemu Android za pomocą platformy Xamarin, zobacz [Witaj, Android](~/android/get-started/hello-android/index.md) Aby dowiedzieć się więcej o tworzeniu projektów dla systemu Android. 

Kiedy tworzysz projekt systemu Android, należy skonfigurować ustawienia wersji docelowej systemu Android MarshMallow 6.0. Aby skierować projekcie dla Marshmallow, należy skonfigurować projekt dla **poziom interfejsu API 23 (Xamarin.Android w wersji 6.0 pomocy technicznej)**. Aby uzyskać więcej informacji o konfigurowaniu poziomów poziom interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android w interpretacji](~/android/app-fundamentals/android-api-levels.md).



### <a name="configure-an-emulator-or-device"></a>Konfigurowanie emulatora lub urządzenia

Jeśli używasz emulatora, uruchom Menedżera AVD systemu Android i Utwórz nowe urządzenie, używając następujących ustawień:

-   Urządzenie: Nexus 5, 6 lub 9.
-   Cel: System Android 6.0 — poziom 23 interfejsu API
-   ABI: x86

Na przykład tego urządzenia wirtualnego jest skonfigurowany do emulowania 5 węzła:

[![Konfigurowanie urządzeń AVD, za pomocą urządzeń Nexus 5, dla systemu Android 6.0 docelowego i Intel Atom (x86)](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Jeśli używasz urządzenia fizycznego, takich jak Nexus 5 6- lub 9, można zainstalować obrazu podglądu z systemem Android Marshmallow. Aby uzyskać więcej informacji na temat aktualizowania urządzenia do systemu Android Marshmallow, zobacz [obrazów systemów sprzętowych](http://developer.android.com/preview/download.html#images).



## <a name="new-features"></a>Nowe funkcje

Wiele zmian wprowadzonych w systemie Android Marshmallow koncentrujące się na poprawa komfortu pracy użytkowników systemu Android, zwiększenie wydajności i naprawia błędy. Jednak Marshmallow również wprowadzić pewne zmiany rozbudowane podstawy platformy systemu Android. Poniższe sekcje wyróżnienie te rozszerzenia funkcjonalności i zapewnia łącza, aby rozpocząć korzystanie z nowych funkcji systemu Android Marshmallow w swojej aplikacji. 



### <a name="runtime-permissions"></a>Uprawnienia środowiska uruchomieniowego

System uprawnień systemu Android został znacznie zoptymalizowane pod kątem i uproszczone, ponieważ w systemie Android Lollipop. W systemie Android Marshmallow użytkowników przyznać uprawnienia na podstawie przez przypadek, w czasie wykonywania, a nie na czas instalacji. Aby zapewnić obsługę tej funkcji w systemie Android Marshmallow lub nowszy, projektujesz aplikację, aby monitować użytkownika o uprawnienia w czasie wykonywania (w ramach których są wymagane uprawnienia). Ta zmiana ułatwia użytkownikom rozpocząć korzystanie z aplikacji natychmiast, ponieważ jest on usprawnia proces instalowania i uaktualniania aplikacji. 

Zobacz [żąda uprawnień środowiska uruchomieniowego w systemie Android Marshmallow](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) szczegółowe (wraz z przykładami kodu) o implementowaniu uprawnieniach w środowisku uruchomieniowym w aplikacji platformy Xamarin.Android.
Xamarin udostępnia również przykładową aplikację, która ilustruje, jak uprawnieniach w środowisku uruchomieniowym działają w systemie Android Marshmallow (i nowszych): [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions).

Ta przykładowa aplikacja pokazuje następujące czynności:

-   Jak sprawdzić i zażądać uprawnień w czasie wykonywania.
-   Jak zadeklarować uprawnienia dla urządzeń z systemem Android M.

Aby użyć tej aplikacji przykładowej:

1.  Naciśnij pozycję **aparatu** lub **kontakty** przycisków, aby wyświetlić uprawnienia żądania okna dialogowego.
2.  Przyznaj uprawnienia do przeglądania fragmenty aparat fotograficzny lub kontaktów.

Aby uzyskać więcej informacji o nowych funkcjach uprawnień środowiska uruchomieniowego w systemie Android Marshmallow, zobacz [Praca z uprawnieniami systemu](https://developer.android.com/preview/features/runtime-permissions.html).



### <a name="authentication-enhancements"></a>Ulepszenia uwierzytelniania

System android Marshmallow obejmuje dwa ulepszenia uwierzytelniania, które pomagają wyeliminować konieczność stosowania haseł:

-   **Odcisku palca uwierzytelniania** &ndash; wykorzystuje skanowania linii papilarnych w celu uwierzytelniania użytkowników.

-   **Upewnij się, poświadczenia** &ndash; uwierzytelnia użytkowników oparte na jak długo urządzenie zostało odblokowane.

Łącza i przykładowe aplikacje opisane w dalszej części mogą pomóc stają się znanej z nowymi funkcjami.


#### <a name="fingerprint-authentication"></a>Uwierzytelnianie odciskiem palca

Na urządzeniach, które obsługują odcisku palca skanowanie sprzętu, można użyć nowej `FingerPrintManager` klasy w celu uwierzytelnienia użytkownika.
Aby uzyskać więcej informacji na temat funkcji uwierzytelniania odciskiem palca w systemie Android Marshmallow, zobacz [uwierzytelnianie odciskiem palca](https://developer.android.com/preview/api-overview.html#fingerprint-authentication).

Środowisko Xamarin zapewnia przykładową aplikację, która ilustruje sposób użycia zarejestrowanych odcisków palców do uwierzytelnienia użytkownika w Twojej aplikacji: [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog).

Aby użyć tej aplikacji przykładowej:

1.  Touch **zakupu** przycisk, aby otworzyć okno dialogowe uwierzytelniania odciskiem palca.
2.  Skanowanie w swojej zarejestrowanej odcisku palca do uwierzytelniania.

Należy pamiętać, że ta Przykładowa aplikacja wymaga urządzenia przy użyciu czytnika linii papilarnych.
Ta aplikacja nie przechowuje palca (lub hasło).



#### <a name="voice-interactions"></a>Interakcje głosowe

Nowa funkcja interakcje głosowe, wprowadzona w systemie Android Marshmallow umożliwia użytkownikom aplikacji za pomocą głosu Potwierdź akcje, a następnie wybierz z listy opcji. Aby uzyskać więcej informacji na temat interakcje głosowe, zobacz [omówienie interfejsu API interakcji głosowych](https://developers.google.com/voice-actions/interaction/). 

Zobacz [Dodaj konwersację do aplikacji dla systemu Android przy użyciu interakcje głosowe](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) szczegółowe (wraz z przykładami kodu) o implementowaniu interakcje głosowe w aplikacji platformy Xamarin.Android.
Przykładowa aplikacja jest dostępna, ilustruje sposób użycia interfejsu API interakcji głosowych w aplikacji platformy Xamarin.Android: [interakcje głosowe](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).



#### <a name="confirm-credential"></a>Upewnij się, poświadczeń

Za pomocą nowego *potwierdzenie poświadczeń* funkcji z systemem Android Marshmallow można zwolnić użytkowników z konieczności należy pamiętać, a następnie wprowadź hasła dla aplikacji przy uwierzytelnianiu ich oparte na jak długo urządzenie zostało odblokowane.
Aby to zrobić, należy użyć nowego `SetUserAuthenticationValidityDurationSeconds` metody `KeyGenerator`. Użyj `KeyGuardManager`firmy `CreateConfirmDeviceCredentialIntent` metoda konieczność ponownego uwierzytelnienia użytkownika z poziomu aplikacji. Aby uzyskać więcej informacji na temat tej nowej funkcji w systemie Android Marshmallow, zobacz [potwierdzenie poświadczeń](https://developer.android.com/preview/api-overview.html#confirm-credential).

Środowisko Xamarin zapewnia przykładowej aplikacji, który ilustruje sposób używania poświadczeń urządzenia (takich jak numer PIN, wzorca lub hasło) w swojej aplikacji: [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

Aby użyć tej aplikacji przykładowej:

1.  Konfigurowanie bezpiecznej blokady ekranu na urządzeniu (**bezpiecznego > Zabezpieczenia > Screenlock**).
2.  Naciśnij pozycję **zakupu** przycisk i upewnij się, poświadczenia bezpiecznej blokady ekranu.



### <a name="chrome-custom-tabs"></a>Niestandardowe karty dla programu Chrome

Deweloperzy aplikacji twarzy wyboru, gdy użytkownik wybiera adres URL: aplikacja można przeglądarkę lub przy użyciu przeglądarki w aplikacji na podstawie `WebView`. Obie opcje stanowi wyzwanie &ndash; uruchamianie w przeglądarce jest przełącznik mocno kontekstu, który nie jest możliwe do dostosowania, podczas `WebView`s nie mają stanu za pośrednictwem przeglądarki. Ponadto użycie `WebView`s można dodać obsługi dodatkowe obciążenie. 

*Chrome kart niestandardowych* umożliwia łatwe i sprawny wyświetlania witryn sieci Web, korzystając z możliwości Chrome bez użytkowników Pozostaw aplikację. Ta funkcja daje aplikacji większą kontrolę nad środowiskiem sieci web przez użytkownika; to, że przejścia między natywne oraz internetowe zawartości, aby usprawnić bez konieczności uciekania się do `WebView`. Aplikacja może również wpływać na, jak dla programu Chrome wygląda i działa poprzez dostosowanie następujące: 

-   Kolor paska narzędzi

-   Włączanie i wyłączanie animacji

-   Niestandardowe akcje w menu narzędzi i przepełnienia dla programu Chrome

-   Start wstępne Chrome i zawartość wstępnego pobierania (w przypadku szybsze ładowanie)

Aby móc korzystać z tej funkcji w aplikacji platformy Xamarin.Android, Pobierz i zainstaluj [Android Support Library kart niestandardowych](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/).
Aby uzyskać więcej informacji na temat tej funkcji, zobacz [Chrome niestandardowe karty](https://developer.chrome.com/multidevice/android/customtabs).



### <a name="material-design-support-library"></a>Biblioteka obsługi projektowania materiałów

Android Lollipop wprowadzone [Material Design](http://www.google.com/design/spec/material-design/introduction.html) jako nowy język projektu, można odświeżyć Praca w systemie Android (zobacz [motyw materiału](~/android/user-interface/material-theme.md) informacji o używaniu projektowania materiałów w aplikacji platformy Xamarin.Android). Z systemem Android Marshmallow wprowadzone Google *biblioteki obsługi systemu Android projektowania* ułatwiają aplikacji deweloperach, którzy zastosują materiał projektowanie wyglądu i działania. Ta biblioteka zawiera następujące składniki:

-   **CoordinatorLayout** &ndash; nowy `CoordinatorLayout` widget jest podobne do, ale bardziej zaawansowane niż `FrameLayout`. Możesz użyć `CoordinatorLayout` jako kontener dla widoków podrzędnej lub układ najwyższego poziomu, a zapewnia `layout_anchor` atrybut, który może służyć do widoków zakotwiczenia względem innych widoków.

-   **Zwijanie pasków narzędzi** &ndash; nowy `CollapsingToolbarLayout` jest zwijanie pasek aplikacji, który jest otoką `Toolbar`. (Należy pamiętać, że *paska aplikacji* to, co zostało wcześniej nazywane *pasek akcji*.)

-   **Zmiennoprzecinkowe przycisk akcji** &ndash; okrągły przycisk, który oznacza głównej akcji w interfejsie użytkownika aplikacji.

-   **Zmiennoprzecinkowe etykiety edycji tekstu** &ndash; używa nowego `TextInputLayout` widżet (który opakowuje `EditText`) do pokazuj przestawne etykietę w wskazówką jest ukryty, gdy użytkownik danych wejściowych tekstu.

-   **Widok okienka nawigacji** &ndash; nowy `NavigationView` widżet ułatwia korzystanie z menu nawigacji w sposób ułatwiający umożliwiające użytkownikom przechodzenie.

-   **Snackbar** &ndash; nowy `SnackBar` widżet to (podobnie do powiadomienia wyskakującego) mechanizm uproszczone opinii, który wyświetla krótką wiadomość w dolnej części ekranu, znajdujących się nad wszystkimi innymi elementami na ekranie.

-   **Karty materiału** &ndash; nowy `TabLayout` element widget udostępnia układzie poziomym dla wyświetlanie kart jako sposób na zaimplementowanie Nawigacja najwyższego poziomu w swojej aplikacji.

Aby móc korzystać z [projektowania Support Library](http://developer.android.com/tools/support-library/features.html#design) w aplikacji platformy Xamarin.Android, Pobierz i zainstaluj platformę Xamarin [Projekt Biblioteka pomocy technicznej platformy Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) pakietu NuGet.

Zobacz [piękne Material Design przy użyciu biblioteki systemu Android projektowania pomocy technicznej](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) szczegółowe (wraz z przykładami kodu) o używaniu biblioteki obsługę projektowania materiałów w aplikacji platformy Xamarin.Android.
Środowisko Xamarin zapewnia przykładową aplikację, która zawiera demonstrację Nowa biblioteka systemu Android projektowania na platformy Xamarin.Android &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare).
W tym przykładzie przedstawiono następujące funkcje biblioteki projektu:


-   Zwijanie paska narzędzi
-   Przestawnym przyciskiem akcji
-   Zakotwiczanie widoku
-   NavigationView
-   Snackbar

Aby uzyskać więcej informacji na temat biblioteki projektu, zobacz [biblioteki obsługi systemu Android projektowania](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) w blog deweloperów systemu Android.


### <a name="additional-library-updates"></a>Dodatkowa biblioteka aktualizacji

Oprócz systemu Android Marshmallow Google ogłosiła powiązane aktualizacje do kilku podstawowe biblioteki systemu Android. Środowisko Xamarin zapewnia obsługę platformy Xamarin.Android dla tych aktualizacji za pośrednictwem kilku pakietów NuGet w wersji zapoznawczej: 

-   [Usługi Google Play](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; najnowszą wersję usług Google Play obejmuje nowy *zaprasza aplikacji* funkcji, która umożliwia użytkownikom udostępnianie swoich aplikacji ze znajomymi. Aby uzyskać więcej informacji na temat tej funkcji, zobacz [zasięg rozwiń aplikacji za pomocą zaproszenia aplikacji firmy Google](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/). 

-   [Biblioteki obsługi systemu android](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; rozszerzeń Nuget te oferują funkcje, które są dostępne tylko dla interfejsów API biblioteki przy jednoczesnym zapewnieniu zgodne z poprzednimi wersjami, wersje interfejsów API platformy systemu Android. 

-   [Biblioteki systemu android noszenia](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; to NuGet zawiera powiązania usług Google Play. Najnowszą wersję biblioteki noszenia udostępnia nowe funkcje (w tym zapewnienia łatwiejszej nawigacji dotyczących niestandardowych aplikacji) do platformy systemu Android Wear. 


## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzone dla systemu Android Marshmallow i wyjaśniono, jak zainstalować i skonfigurować na Marshmallow najnowsze narzędzia i pakiety do tworzenia aplikacji platformy Xamarin.Android. Do tworzenia aplikacji platformy Xamarin.Android są również podane Przegląd najbardziej atrakcyjne funkcje dla systemu Android Marshmallow.


## <a name="related-links"></a>Linki pokrewne

- [System android Marshmallow 6.0](http://developer.android.com/about/versions/marshmallow/index.html)
- [Pobierz zestaw SDK systemu Android](https://developer.android.com/sdk/index.html#Other)
- [Omówienie funkcji](https://developer.android.com/preview/api-overview.html)
- [Uwagi do wersji](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (przykład)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (przykład)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (przykład)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
