---
title: Interfejs typu lizak funkcji
description: "Ten artykuł zawiera omówienie nowych funkcji wprowadzonych w systemie Android 5.0 (interfejs typu lizak) wysokiego poziomu. Te funkcje obejmują nowe styl interfejsu użytkownika o nazwie motywu materiałów, a także nowe funkcje pomocnicze, takie jak animacji, shadows widoku i obiektów drawable cieniowanie. Android 5.0 zawiera także rozszerzone powiadomienia, dwa nowe elementy widget interfejsu użytkownika, nowego harmonogramu zadań i kilka nowych interfejsów API, zwiększające magazynu, sieci, łączności i możliwości multimedialne."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 24d85d7be580f8db8621d91ebbb27c0b7881b4eb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="lollipop-features"></a>Interfejs typu lizak funkcji

_Ten artykuł zawiera omówienie nowych funkcji wprowadzonych w systemie Android 5.0 (interfejs typu lizak) wysokiego poziomu. Te funkcje obejmują nowe styl interfejsu użytkownika o nazwie motywu materiałów, a także nowe funkcje pomocnicze, takie jak animacji, shadows widoku i obiektów drawable cieniowanie. Android 5.0 zawiera także rozszerzone powiadomienia, dwa nowe elementy widget interfejsu użytkownika, nowego harmonogramu zadań i kilka nowych interfejsów API, zwiększające magazynu, sieci, łączności i możliwości multimedialne._

## <a name="lollipop-overview"></a>Interfejs typu lizak — omówienie

Android 5.0 (interfejs typu lizak) wprowadzono nowy język projektu *projektowania materiałów*, i z nią dokumentację rzutowanie nowych funkcji do tworzenia aplikacji, łatwiejsze i bardziej intuicyjne do użycia. Materiał projekt Android 5.0 nie tylko zapewnia telefony internetową; zapewnia także nowy zestaw reguł projektowania tablety z systemem Android, komputerów stacjonarnych, obserwowanie i telewizorach inteligentne. Te reguły projektowania wyróżnić prostoty i minimalism podczas wprowadzania umożliwia znanych atrybutów dotykiem (na przykład realistyczne podpowiedzi powierzchni i krawędzi) umożliwia szybkie i intuicyjnie zrozumieć interfejsu.

*Motyw materiału* jest więc odzwierciedleniem tych zasad projektu interfejsu użytkownika w systemie Android. W tym artykule rozpoczyna się od obejmujące funkcje pomocnicze materiałów motywu:

-   **Animacje** &ndash; *Touch opinii* animacji, *przejścia działania* animacji, *wyświetlić przejście stanu* animacji i *ujawnić efekt*.

-   **Wyświetl cieni i podniesienia uprawnień** &ndash; widoków obecnie ma `elevation` właściwości; widoków z wyższej `elevation` wartości rzutowania większych shadows w tle.

-   **Kolor funkcji** &ndash; *odcieni obiektów Drawable* umożliwia ponowne używanie zasobów obrazu, zmieniając ich kolor i *wyodrębniania poświęcone kolor* pomaga dynamicznie motyw aplikacji oparte na kolorów obrazu.

Wiele motywu materiału, który już wbudowanych funkcji interfejsu użytkownika dla systemu Android 5.0 występują, podczas gdy inne należy jawnie dodać do aplikacji. Na przykład niektóre widoki standardowe (takie jak przyciski) już obejmuje animacje opinii touch, podczas gdy aplikacje należy włączyć większości shadows widoku.

Oprócz spowodowaną przez materiał motywu usprawnień interfejsu użytkownika systemu Android 5.0 zawiera kilka innych nowych funkcji, które zostały omówione w tym artykule:

-   **Ulepszone powiadomienia** &ndash; powiadomienia w systemie Android 5.0 znacznie zyskało wygląda obsługę powiadomień ekranu blokady i nowy *projekcyjny wskazuje pozycję* format prezentacji powiadomienia.

-   **Nowy interfejs użytkownika elementy widget** &ndash; nowy `RecyclerView` elementu widget ułatwia tworzenie aplikacji w celu przedstawienia dużych zestawów danych i złożonych informacji i nowych `CardView` elementu widget zawiera format Uproszczona prezentacja jak karty do wyświetlania tekstu i obrazy.

-   **Nowe interfejsy API** &ndash; Android 5.0 dodaje nowe interfejsy API do obsługi wielu sieci, ulepszona łączności Bluetooth, łatwiejsze zarządzanie magazynami i bardziej elastyczną kontrolę nad odtwarzacze multimedialne i aparatu fotograficznego urządzenia. Nowe zadanie funkcji planowania jest dostępna do uruchamiania zadań asynchronicznie w określonych godzinach. Funkcja ta umożliwia wydłużyć czas pracy baterii, na przykład Planowanie zadań miejsce, gdy urządzenie jest podłączone i ładowania.


## <a name="requirements"></a>Wymagania

Poniżej jest wymagany do korzystania z nowych funkcji systemu Android 5.0 w aplikacjach opartych na platformie Xamarin:

-   **Xamarin.Android** &ndash; Xamarin.Android 4.20 lub nowszej, musi być zainstalowana i skonfigurowana przy użyciu programu Visual Studio lub Visual Studio dla komputerów Mac. 

-   **Zestaw SDK systemu android** &ndash; Android 5.0 (interfejs API 21) lub nowszym należy zainstalować za pomocą Menedżera zestawu SDK systemu Android.

-   **Java Developer Kit** &ndash; wymaga platformy Xamarin.Android [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) lub nowszym, jeśli tworzysz poziom interfejsu API 24 lub większym (JDK 1.8 obsługuje również poziomy interfejsu API starszych niż 24, w tym interfejs typu lizak). 64-bitowej wersji JDK 1.8 jest wymagany, jeśli używasz Kontrolki niestandardowe lub podglądzie formularzy.

Można nadal używać [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Jeśli tworzenie specjalnie dla interfejsu API na poziomie 23 lub starszym.


<a name="settingup" />

## <a name="setting-up-an-android-50-project"></a>Konfigurowanie projektu programu Android 5.0

Aby utworzyć projekt Android 5.0, należy zainstalować najnowsze narzędzia i zestaw SDK pakietów. Aby skonfigurować projekt platformy Xamarin.Android którego element docelowy z systemem Android 5.0, wykonaj następujące kroki:

1. Zainstaluj narzędzia platformy Xamarin.Android i aktywować licencję Xamarin. Zobacz [instalacja i Konfiguracja](~/android/get-started/installation/index.md) Aby uzyskać więcej informacji o instalowaniu platformy Xamarin.Android.

2. Jeśli używasz programu Visual Studio dla komputerów Mac, należy zainstalować najnowsze aktualizacje systemu Android 5.0.

3. Uruchom Menedżera zestawu SDK systemu Android (w programie Visual Studio dla komputerów Mac, należy użyć **narzędzia &gt; Open Android SDK Manager&hellip;**) i zainstalować narzędzia zestawu SDK systemu Android 23.0.5 lub nowszym:

    [![Wybieranie narzędzia zestawu SDK systemu Android w programie Android SDK Manager](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png)

   Ponadto zainstalować najnowszych pakietów zestawu SDK systemu Android 5.0 (interfejs API 21 lub nowszej):

    [![Instalowanie zestawu SDK systemu Android 5.0 pakietów w Menedżerze zestawu SDK systemu Android](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png)

   Aby uzyskać więcej informacji o używaniu Android SDK Manager, zobacz [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html).

4. Utwórz nowy projekt platformy Xamarin.Android. Jeśli jesteś nowym użytkownikiem programowanie dla systemu Android za pomocą platformy Xamarin, zobacz [Hello, Android](~/android/get-started/hello-android/index.md) Aby dowiedzieć się więcej o tworzeniu projektów dla systemu Android. Podczas tworzenia projektu systemu Android, należy skonfigurować ustawienia wersji dla systemu Android 5.0.
   W programie Visual Studio dla komputerów Mac, przejdź do **opcje projektu &gt; kompilacji &gt; ogólne** i ustaw **platformy docelowej** do **Android 5.0 (interfejs typu lizak)** lub później:

    ![Ustawienie Framwework docelowego dla systemu Android 5.0 interfejs typu lizak](lollipop-images/target-framework.png)

   W obszarze **opcje projektu &gt; kompilacji &gt; aplikacji systemu Android**, wartość minimalna i docelowa wersja systemu Android, aby **automatyczny — Użyj docelowej framework w wersji**:

    ![Ustawienie wersji minimalnej i docelowych z systemem Android na automatyczny](lollipop-images/minimum-android-version.png)

5. Skonfiguruj emulatora albo urządzenia z systemem Android, aby przetestować aplikację. Jeśli używasz emulatora, zobacz [Konfiguracja emulatora systemu Android](~/android/get-started/installation/android-emulator/index.md) Aby dowiedzieć się, jak skonfigurować emulatorze systemu Android do użytku z Xamarin Studio lub Visual Studio. Jeśli używasz urządzenia z systemem Android, zobacz [ustawienie się zestaw SDK w wersji zapoznawczej](https://developer.android.com/preview/setup-sdk.html) Aby dowiedzieć się, jak zaktualizować urządzenie z systemem Android 5.0. Aby skonfigurować urządzenia z systemem Android służący do uruchamiania i debugowanie aplikacji platformy Xamarin.Android, zobacz [ustawić się urządzenia na potrzeby programowania](~/android/get-started/installation/set-up-device-for-development.md).

Uwaga: Aby zaktualizować istniejący projekt Android, który został przeznaczonych dla systemu Android Podgląd L, należy zaktualizować **platformy docelowej** i **wersji Android** wartości opisane powyżej.


<a name="changes" />

## <a name="important-changes"></a>Ważne zmiany

Wcześniej opublikowane aplikacje dla systemu Android może mieć wpływ zmian w systemie Android 5.0. W szczególności Android 5.0 używa nowe środowisko uruchomieniowe i format powiadomienia znacznie zmienione.

<a name="runtime" />

### <a name="android-runtime"></a>Środowisko uruchomieniowe systemu android

Android 5.0 używa nowego środowiska uruchomieniowego systemu Android (GRAFIKI) jako domyślnego środowiska uruchomieniowego zamiast platformy Dalvik. GRAFIKA implementuje kilka najważniejszych nowych funkcji:

-   **Kompilacja z wyprzedzeniem elementu time (drzewa obiektów aplikacji)** &ndash; drzewa obiektów aplikacji może poprawić wydajność aplikacji przez skompilowanie kodu aplikacji przed pierwszym uruchomieniem aplikacji. Po zainstalowaniu aplikacji GRAFIKĘ generuje skompilowanej aplikacji sieci pliku wykonywalnego dla urządzenia.

-   **Ulepszone wyrzucanie elementów bezużytecznych (GC)** &ndash; ulepszenia GC kompozycji można również poprawić wydajność aplikacji. Wyrzucanie elementów bezużytecznych teraz korzysta z jednego Wstrzymaj GC zamiast dwóch i ukończyć jednoczesnych operacji GC w sposób bardziej aktualny.

-   **Ulepszone debugowanie aplikacji** &ndash; GRAFIKĘ zapewnia bardziej szczegółowo diagnostycznych, aby ułatwić analizowanie wyjątków i raporty o awarii.

Istniejące aplikacje powinny działać bez zmian w obszarze KOMPOZYCJI &ndash; z wyjątkiem aplikacji, które wykorzystać unikatowe dla wcześniejsze środowisko uruchomieniowe platformy Dalvik techniki, które mogą nie działać prawidłowo w obszarze KOMPOZYCJI. Aby uzyskać więcej informacji na temat tych zmian, zobacz [weryfikowanie zachowanie aplikacji na Android środowiska uruchomieniowego (GRAFIKA)](http://developer.android.com/guide/practices/verifying-apps-art.html).

<a name="notifchanges" />

### <a name="notification-changes"></a>Powiadomienie ulega zmianie

Powiadomienia zmieniły się znacznie w systemie Android 5.0:

-   **Dźwięki i wibrację są obsługiwane inaczej** &ndash; dźwięki powiadomień i drgań są teraz obsługiwane przez `Notification.Builder` zamiast `Ringtone`, `MediaPlayer`, i `Vibrator`.

-   **Nowy schemat kolorów** &ndash; zgodnie z motywu materiałów powiadomienia są renderowane tekstem ciemny za pośrednictwem białe lub bardzo małe tła. Ponadto kanałów alfa w ikony powiadomień może być modyfikowany przez system Android do zapewnienia koordynacji z schematy kolorów systemu. 

-   **Powiadomienia ekranu blokady** &ndash; powiadomienia mogą być teraz wyświetlane na ekranu blokady urządzenia.

-   **Projekcyjny wskazuje pozycję** &ndash; powiadomienia o wysokim priorytecie zostaną wyświetlone w małych przestawne okno (projekcyjny wskazuje pozycję powiadomienia) po odblokowaniu urządzenia i ekranu jest włączona.

W większości przypadków eksportowanie istniejące funkcje powiadomień aplikacji dla systemu Android w wersji 5.0 wymaga wykonania następujących czynności:

1.  Konwertuj kod, aby używał `Notification.Builder` (lub `NotificationsCompat.Builder`) do tworzenia powiadomienia. 

2.  Sprawdź, czy można wyświetlić w schemat kolorów motywu materiałów istniejących zasobów powiadomień.

3.  Zdecyduj, jakie widoczność powinny mieć powiadomienia, gdy mają być przedstawiane na ekranu blokady. Jeśli powiadomienie nie jest publiczny, jaka zawartość powinny być widoczne na ekranu blokady?

4.  Ustaw kategorię powiadomienia, są poprawnie obsługiwane w nowych Android 5.0 *nie przeszkadzać* tryb.

Jeśli powiadomienia istnieje transportu formantów, media wyświetlania stanu odtwarzania, użyj `RemoteControlClient`, lub zadzwoń `ActivityManager.GetRecentTasks`, zobacz [ważne zmiany zachowania](http://developer.android.com/preview/api-overview.html#Behaviors) Aby uzyskać więcej informacji o aktualizowaniu powiadomienia dla systemu Android 5.0.

Informacje o tworzeniu powiadomienia w systemie Android, zobacz [lokalnego powiadomienia](~/android/app-fundamentals/notifications/local-notifications.md). [Zgodności](~/android/app-fundamentals/notifications/local-notifications.md#compatibility) części w tym artykule opisano sposób tworzenia powiadomień, które są zgodne z pobieranej z wcześniejszymi wersjami systemu android.

<a name="materialtheme" />

## <a name="material-theme"></a>Materiał motywu

Nowy motyw materiałów Android 5.0 wprowadzono zmian polegających na usuwaniu do wyglądu i działania interfejsu użytkownika dla systemu Android. Elementy wizualne teraz używać dotykiem powierzchnie przybrać bold grafiki, typografii i kolory jasny drukowania na podstawie projektu. Przykłady motywu materiałów są przedstawione na poniższych zrzutach ekranu:

[![Zrzuty ekranu ekran głównej motywu materiałów, ekranu aplikacji oraz ustawienia ekranu](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png)

Android 5.0 złoży Ci wyświetlane po lewej stronie ekranu głównego. Zrzut ekranu Centrum jest pierwszym ekranie listy aplikacji i zrzut ekranu po prawej stronie jest **ustawienia** ekranu. Firmy Google [projektowania materiałów](https://material.io/guidelines/material-design/introduction.html) specyfikacji wyjaśniono podstawowej reguły projektowania za nowe pojęcie materiałów motywu.

Kompozycja materiału zawiera trzy odmian wbudowanych, których można użyć w aplikacji: `Theme.Material` ciemny motyw (ustawienie domyślne), `Theme.Material.Light` motywu i `Theme.Material.Light.DarkActionBar` motywu: 

[![Zrzuty ekranu ciemny, jasnym i DarkActionBar motywów](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png)

Aby uzyskać więcej informacji o korzystaniu z funkcji motywu materiału w aplikacji platformy Xamarin.Android zobacz [motyw materiału](~/android/user-interface/material-theme.md).

<a name="animations" />

## <a name="animations"></a>Animacji

Android 5.0 zawiera animacji opinii touch, działania animacji przejścia i animacji przejścia stanu widoku, aby bardziej intuicyjne używać interfejsów aplikacji. Ponadto można użyć aplikacji systemu Android 5.0 *ujawnić efekt* animacji, aby ukryć lub ujawnić widoków. Można użyć *krzywych ruchu* ustawienia konfiguracji, jak szybko lub wolno animacje są renderowane.

<a name="touchanim" />

### <a name="touch-feedback-animations"></a>Touch animacje opinii

Animacje opinii Touch udzielać użytkownikom wizualne widok został dotknięciu. Na przykład przycisków jest teraz wyświetlany wpływ mają dotknąć &ndash; Animacja opinie touch domyślne w systemie Android 5.0 jest. Animacja ripple jest implementowany przez nowe `RippleDrawable` klasy. Wpływ można skonfigurować na końcu granice widoku lub wykracza poza granice tego widoku. Na przykład następująca sekwencja zrzuty ekranu przedstawiono wpływ w przycisku podczas touch animacji:

![Ramki przez zrzuty ekranu ramki animacji ripple na przycisku](lollipop-images/touch-animation.png)

Touch początkowej skontaktuj się z przyciskiem występuje w pierwszy obraz po lewej stronie, gdy pozostałe sekwencji (od lewej do prawej) przedstawiono, jak wpływ rozprzestrzenia krawędzią przycisku. Po zakończeniu animacji ripple widoku zwraca do oryginalnego wyglądu. Animacja ripple domyślna odbywa się w ułamek sekund, ale można dostosowywać długość animacji dla długości dłuższy lub krótszy czas.

Aby uzyskać więcej informacji na temat touch animacje opinii w systemie Android 5.0, zobacz [dostosować opinii Touch](http://developer.android.com/training/material/animations.html#Touch).

<a name="activityanim" />

### <a name="activity-transition-animations"></a>Działania animacji przejścia

Działania animacji przejścia zwracanie użytkowników w pewnym sensie visual ciągłości jedno działanie przechodzi do innej. Aplikacje można określić trzy rodzaje animacji przejścia:

-   **Wprowadź przejścia** &ndash; dla działania wejścia w sceny.

-   **Zakończ przejście** &ndash; dla podczas działania zamknięciu scenę.

-   **Udostępniony element przejścia** &ndash; dla zmiany widoku, który jest wspólny dla dwóch działań jako pierwszy działania przejścia do następnego.

Na przykład następująca sekwencja zrzuty ekranu przedstawiono przejścia udostępnionego elementu:

[![Ramki przez zrzuty ekranu ramki animacji przejścia udostępnionego elementu](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png)

Udostępniony element (zdjęcie gąsienice) jest jednym z kilku widoków w pierwsze działanie; powiększa go jako widok tylko w drugiej działania jako pierwszy przejścia działania do drugiego.

#### <a name="enter-transition-animation-types"></a>Wprowadź typów animacji przejścia

Dla przejścia enter Android 5.0 udostępnia trzy typy animacji:

-   **Rozwiń animacji** &ndash; Powiększenie widoku z Centrum sceny.

-   **Slajd animacji** &ndash; widoku są przenoszone z jednego z krawędziami sceny.

-   **Stopniowe animacji** &ndash; stopniowo wgląd do sceny.

#### <a name="exit-transition-animation-types"></a>Zakończ przejście animacji typów

Do zakończenia przejścia Android 5.0 udostępnia trzy typy animacji:

-   **Rozwiń animacji** &ndash; zmniejsza widok, aby Centrum sceny.

-   **Slajd animacji** &ndash; przenosi widoku do jednego z krawędziami sceny.

-   **Stopniowe animacji** &ndash; stopniowo widoku poza sceny.

#### <a name="shared-element-transition-animation-types"></a>Udostępnione typy animacji przejścia elementu

Udostępniony element przejścia obsługuje wiele typów animacji, takich jak:

-   Zmiana układu lub klip granice widoku.

-   Zmiana skalowanie i obrót widoku.

-   Zmiana rozmiaru i skali typu widoku.

Aby uzyskać więcej informacji na temat działania animacji przejścia w systemie Android 5.0, zobacz [dostosować przejścia działania](http://developer.android.com/training/material/animations.html#Transitions).

<a name="viewstate" />

### <a name="view-state-transition-animations"></a>Animacji przejścia w stan widoku

Android 5.0 umożliwia animacje do uruchomienia po zmianie stanu widoku. Przejścia stanów widoku można animować przy użyciu jednej z następujących metod:

-   Utwórz drawables, który animowanie zmian stanu skojarzonego z konkretnym widoku. Nowe `AnimatedStateListDrawable` klasa umożliwia tworzenie drawables wyświetlające animacje między zmian stanu widoku.

-   Definiowanie funkcji animacji, który jest uruchamiany po zmianie stanu widoku. Nowe `StateListAnimator` klasy pozwala zdefiniować zresztą, który jest uruchamiany po zmianie stanu widoku.

Aby uzyskać więcej informacji o animacji przejścia stanu widoku w systemie Android 5.0, zobacz [animowanie zmian stanu widoku](http://developer.android.com/training/material/animations.html#ViewState).

<a name="reveal" />

### <a name="reveal-effect"></a>Odsłoń efektu

*Ujawnić efekt* jest wycinka koła tego radius zmiany, aby pokazać lub ukryć widoku. W tym celu można kontrolować przez ustawienie początkowych i końcowych promień okręgu wycinka. Następująca sekwencja zrzuty ekranu przedstawiono animację efekt ujawniania z Centrum ekranu:

[![Ramki przez zrzuty ekranu ramki animacji ujawniania](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png)

Sekwencja dalej przedstawiono animacji efekt ujawniania, która ma miejsce, w lewym dolnym rogu ekranu:

[![Ramki przez zrzuty ekranu ramki animacji wycinka](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png)

Ujawnić, że można wycofać animacji; oznacza to, że wycinka koła można zmniejszyć, aby ukryć widok zamiast Powiększ, aby wyświetlić widok.

Aby uzyskać więcej informacji na efekt ujawniania Android 5.0, zobacz [efekt ujawnić](http://developer.android.com/training/material/animations.html#Reveal).

<a name="curvedmotion" />

### <a name="curved-motion"></a>Zakrzywioną ruchu

Oprócz tych funkcji animacji Android 5.0 także nowych interfejsów API, które umożliwiają określenie czasu i ruchu krzywych animacji. Android 5.0 używa tych krzywych do interpolacji przepływu danych czasowych i przestrzennych podczas animacji. Trzech krzywych są zdefiniowane w systemie Android 5.0:

-   **Szybkie\_limit\_liniowej\_w** &ndash; przyspieszają szybko i w dalszym ciągu przyspieszanie aż do zakończenia animacji.

-   **Szybkie\_limit\_powolne\_w** &ndash; Accelerates szybkie i powoli zwalnia pod koniec animacji.

-   **Liniowy\_limit\_powolne\_w** &ndash; Begins prędkość szczytowe i powoli zwalnia na końcu animacji.

Można użyć nowej `PathInterpolator` klasę, aby określić, jak odbywa się interpolacji ruchu. `PathInterpolator` jest interpolatora, który przechodzi przez ścieżki animacji zgodnie z punktów kontrolnych określonych i krzywych ruchu. Aby uzyskać więcej informacji na temat określania ustawień zakrzywioną ruchu w systemie Android 5.0, zobacz [ruchu zakrzywioną użyj](http://developer.android.com/training/material/animations.html#CurvedMotion).

<a name="viewshadows" />

## <a name="view-shadows--elevation"></a>Shadows widoku & podniesienia uprawnień

W systemie Android 5.0, można określić *podniesienia uprawnień* widoku ustawiając nową `Z` właściwości. Większy `Z` wartość powoduje widoku można rzutować większych cienia w tle, tworzenie widoku prawdopodobnie float wyższej powyżej tła. Początkowej podniesienie widoku można ustawić, konfigurując jego `elevation` atrybutu w układzie.

Poniższy przykład przedstawia shadows rzutowanie pustą `TextView` kontrolować, jeśli jego atrybut podniesienia uprawnień ma wartość 2dp, 4dp i 6dp, odpowiednio:

[![Zrzuty ekranu progessively większych cieni widoku](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png)

Widok ustawień w tle może być statyczne (jak pokazano powyżej) lub użyciem w animacji można utworzyć widok wydają się tymczasowo wzrostu powyżej tło widoku. Można użyć `ViewPropertyAnimator` klasy do animowania podniesienie widoku. Podniesienie uprawnień w widoku jest sumą jego układ `elevation` ustawienie plus `translationZ` właściwość, którą można ustawić za pomocą `ViewPropertyAnimator` wywołania metody.

Aby uzyskać więcej informacji o shadows widoku w systemie Android 5.0, zobacz [wycinka widoków i definiowanie Shadows](http://developer.android.com/training/material/shadows-clipping.html).

<a name="colorfeatures" />

## <a name="color-features"></a>Funkcje kolorów

Zarządzanie kolorami w aplikacjach systemu android 5.0 zawiera dwie nowe funkcje:

-   *Cieniowanie obiektów drawable* pozwala zmienić kolory obrazów, zmieniając atrybut układu.

-   *Kolor poświęcone wyodrębniania* umożliwia dynamiczne Dostosowywanie motywu kolorów aplikacji do zapewnienia koordynacji z palety kolorów wyświetlany obraz.

<a name="tinting" />

### <a name="drawable-tinting"></a>Cieniowanie obiektów drawable

Android 5.0 układów rozpoznaje nowy `tint` atrybut, który można użyć, aby ustawić kolor drawables bez konieczności tworzenia wielu wersji tych zasobów, aby wyświetlić różne kolory. Aby użyć tej funkcji, należy zdefiniować mapy bitowej maski alfa i użyj `tint` atrybut do definiowania kolor elementu zawartości. Dzięki temu można utworzyć zasoby raz i kolor je w układzie odpowiadające motywu.

W poniższym przykładzie pojedynczy zasób obrazu &ndash; białe logo z przeźroczystym tłem &ndash; służy do tworzenia odcień zmian:

![Logo Xamarin biały z przezroczystego tła](lollipop-images/xamarin-logo-white.png)

To logo jest wyświetlany powyżej niebieskie tło cykliczne, jak pokazano w poniższych przykładach. Obraz po lewej stronie jest jak logo pojawia się bez `tint` ustawienie. W Centrum obrazu, logo `tint` atrybucie ustawiono ciemny szary. W obrazie po prawej stronie `tint` ma ustawioną wartość światła szary:

![Przykłady powyżej logo z różnych tint — ustawienia](lollipop-images/drawable-tinting.png)

Aby uzyskać więcej informacji na temat obiektów drawable cieniowanie w systemie Android 5.0, zobacz [odcieni obiektów Drawable](http://developer.android.com/training/material/drawables.html#DrawableTint).

<a name="colorextract" />

### <a name="prominent-color-extraction"></a>Kolor poświęcone wyodrębniania

Nowe Android 5.0 `Palette` klasa pozwala wyodrębnić kolory z obrazu, dzięki czemu można dynamicznie stosować do paletę kolorów niestandardowych. `Palette` Klasy wyodrębnia sześć kolorów z obrazu i etykiety kolory zgodnie z ich względnych poziomów nasycenie kolorów i jasność:

-   Aktywny

-   Aktywny ciemny

-   Jasny aktywny

-   Wyciszony

-   Stonowane ciemny

-   Jasny stonowane

Na przykład poniższe zrzuty ekranu, zdjęcie wyświetlanie aplikacji wyodrębnia poświęcone kolory z obrazu na ekran i używa kolorów te dostosowania schemat kolorów aplikacji do dopasowania obrazu:

[![Zrzuty ekranu ekstrakcje kolor zielony, różowym i niebieski motyw](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png)

W powyższym zrzuty ekranu, na pasku akcji ma ustawioną wartość wyodrębnionego "żywe jasny" kolor i tło ma ustawioną wartość wyodrębnionego "żywe ciemny" kolorów. W każdym powyższego przykładu wiersz kwadratów kolorów jest uwzględniona w celu zilustrowania kolorów palety, które zostały wyodrębnione z obrazu.

Aby uzyskać więcej informacji o wyodrębniania kolorów w systemie Android 5.0, zobacz [wyodrębniania poświęcone kolory z obrazu](http://developer.android.com/training/material/drawables.html#ColorExtract).

<a name="newuiwidgets" />

## <a name="new-ui-widgets"></a>Nowe elementy widget interfejsu użytkownika

Android 5.0 wprowadzono dwie nowe elementy widget interfejsu użytkownika:

-   `RecyclerView` &ndash; Wyświetlanie grup zostanie wyświetlona lista elementów przewijanego.

-   `CardView` &ndash; Podstawowy układ z zaokrąglonymi narożnikami.

Oba elementy widget obsługują rozszerzania funkcji motywu materiałów; na przykład `RecyclerView` używa animacji do dodawania i usuwania widoków i `CardView` używa wyświetlić shadows dokonanie każdej karty prawdopodobnie przestawiany powyżej w tle. Na poniższych zrzutach ekranu przedstawiono przykładowe te nowe elementy widget:

[![Zrzuty ekranu aplikacji skompilowanej za pomocą RecyclerView](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png)

Zrzut ekranu po lewej stronie jest przykładem `RecyclerView` używany w aplikacji poczty e-mail i tworzenia zrzutów ekranu na prawo jest przykładem `CardView` w aplikacji rezerwacji podróży.

<a name="recyclerview" />

### <a name="recyclerview"></a>RecyclerView

`RecyclerView` przypomina `ListView,` , ale lepiej jest odpowiednie dla dużych zestawów widoków lub list z elementami, które zmienia się dynamicznie. Podobnie jak `ListView,` Określ adapter w celu dostępu podstawowy zestaw danych. Jednak w przeciwieństwie do `ListView,` używasz *menedżera układu* położenie elementów w obrębie `RecyclerView`. Menedżer układu również zajmuje się widok recyklingu; zarządza ponowne użycie elementu widoków, które nie są już widoczne dla użytkowników.

Jeśli używasz `RecyclerView` widget, należy określić `LayoutManager` i karty. Jak pokazano na tym rysunku `LayoutManager` jest pośrednik między karty i `RecyclerView`:

![Diagram RecyclerView wraz z pomocniczymi karty, LayoutManager i zestawu danych](lollipop-images/recyclerview-diagram.png)

Poniższe zrzuty ekranu przedstawiają `RecyclerView` zawiera elementy 100 (każdego elementu składa się z `ImageView` i `TextView`):

[![Zrzuty ekranu aplikacji RecyclerView przewijanie obrazów](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png)

`RecyclerView` obsługuje ten dużych zestawów danych z łatwością &ndash; przewijania z początku listy, aby zakończyć listy w tym przykładzie aplikacja zajmuje tylko kilka sekund. `RecyclerView` obsługuje także animacje; w rzeczywistości animacji Dodawanie i usuwanie elementów są domyślnie włączone. Gdy element zostanie dodany do `RecyclerView`, zniknie w, jak pokazano w tej sekwencji zrzuty ekranu:

[![Ramki przez ramki zrzut ekranu przedstawiający Znikająca elementu fotografii, w](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png)

Aby uzyskać więcej informacji na temat `RecyclerView`, zobacz [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).

<a name="cardview" />

### <a name="cardview"></a>CardView

`CardView` jest to prosty widok symuluje przestawne karty z zaokrąglonymi narożnikami. Ponieważ `CardView` ma wbudowane widoku cieni, zapewnia prosty sposób można dodać do aplikacji visual głębokość. Poniższe zrzuty ekranu pokazują trzy tekstowych przykładowe `CardView`:

[![Przykład zrzuty ekranu aplikacji korzystających z elementami na podstawie CardView RecyclerView](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png)

Każda z kart w powyższym przykładzie zawiera `TextView`; kolor tła ustawione za pośrednictwem `cardBackgroundColor` atrybutu.

Aby uzyskać więcej informacji na temat `CardView`, zobacz [CardView](~/android/user-interface/controls/card-view.md).

<a name="enhanced" />

## <a name="enhanced-notifications"></a>Ulepszone powiadomienia

System powiadomień w systemie Android 5.0 została zaktualizowana znacznie z nowego formatu visual i nowe funkcje. Powiadomienia mieć wygląda w systemie Android 5.0. Na przykład powiadomienia w systemie Android 5.0 teraz używać ciemny tekst w tle światła:

![Przykład powiadomień Android 5.0, które nie zostały rozwinięte](lollipop-images/expanded-notification-contracted.png)

Po wyświetleniu powiadomienia (jak pokazano w powyższym przykładzie) dużych ikon Android 5.0 przedstawia małych ikon jako wskaźnika za pośrednictwem dużych ikon. 

W systemie Android 5.0 powiadomień może również zostać wyświetlony na ekranu blokady urządzenia.
Na przykład w tym miejscu jest zrzucie ekranu blokady z jednego powiadomienia:

[![Zrzut ekranu przedstawiający powiadomienia na ekranie blokady](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png)

Użytkownicy mogą dwukrotnego powiadomienie na ekranu blokady do odblokowania urządzenia i przejść do aplikacji, którą zainicjowano powiadomienie, lub przejdź na odrzucenie powiadomienia. Powiadomienia ma nowego *widoczność* ustawienie określa, ile zawartości mogą być wyświetlane na ekranu blokady. Użytkownicy mogą wybrać, czy zezwalać poufnej zawartości ma być wyświetlany w powiadomień ekranu blokady.

Android 5.0 wprowadzono nowy format prezentacji powiadomienia o wysokim priorytecie o nazwie *projekcyjny wskazuje pozycję*. Powiadomienia projekcyjny wskazuje pozycję slajd w dół od górnej krawędzi ekranu przez kilka sekund i następnie retreat do cień powiadomienia w górnej części ekranu. Powiadomienia projekcyjny wskazuje pozycję umożliwiają systemu interfejsu użytkownika, aby umieścić ważne informacje przed użytkownika bez zakłócania działania aktualnie uruchomione. Poniższy przykład przedstawia prostego powiadomienia projekcyjny wskazuje pozycję wyświetlany u góry aplikacji:

[![Przykładowe powiadomienie heads-up](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png)

Powiadomienia projekcyjny wskazuje pozycję zwykle są używane następujące zdarzenia:

-   Nowa wiadomość dalej

-   Przychodzących połączeń telefonicznych

-   Wskazuje na niskim poziomie naładowania baterii

-   Alarm

Android 5.0 wyświetli powiadomienie w formacie projekcyjny wskazuje pozycję tylko wtedy, gdy ma ona ustawienie priorytet Wysoki lub max.

W systemie Android 5.0 musisz podać metadanych powiadomień do pomocy systemu Android, sortowania i bardziej inteligentnie wyświetlać powiadomienia. Android 5.0 organizuje powiadomienia w zależności od priorytetu, widoczność i kategorii.
Kategorie powiadomienia są używane do filtrowania powiadomienia, które mogą być przedstawiane, gdy urządzenie jest w *nie przeszkadzać* tryb.

Aby uzyskać szczegółowe informacje na temat tworzenia i uruchamiania powiadomienia z najnowszych funkcji systemu Android 5.0, zobacz [lokalnego powiadomienia](~/android/app-fundamentals/notifications/local-notifications.md).

<a name="newapis" />

## <a name="new-apis"></a>Nowe interfejsy API

Oprócz nowych funkcji wyglądu i działania opisane powyżej Android 5.0 dodaje nowe interfejsy API umożliwiające rozszerzenie możliwości istniejących multimedia, magazynowania i funkcji bezprzewodowej/połączeniem. Ponadto Android 5.0 obejmuje nowych interfejsów API, które zapewniają obsługę nowych funkcji harmonogram zadania.

### <a name="camera"></a>Aparat fotograficzny

Android 5.0 udostępnia kilka nowych interfejsów API funkcji rozszerzoną aparatu. Nowe `Android.Hardware.Camera2` przestrzeń nazw obejmuje funkcjonalność do uzyskiwania dostępu do poszczególnych aparatu fotograficznego urządzenia podłączone do urządzenia z systemem Android. Ponadto `Android.Hardware.Camera2` modele poszczególnych urządzeń kamery jako potoku: go akceptuje żądania przechwycenia przechwytuje obraz i danych wyjściowych wynik. Takie podejście umożliwia aplikacji do kolejki wiele żądań przechwytywania do aparatu fotograficznego urządzenia.

Następujące interfejsy API umożliwiają te nowe funkcje:

-   `CameraManager.GetCameraIdList` &ndash; Ułatwia do uzyskania programowego dostępu do urządzeń kamery; Możesz użyć `CameraManager.OpenCamera` nawiązać połączenia z określonego aparatu fotograficznego urządzenia.

-   `CameraCaptureSession` &ndash; Przechwytuje lub strumienie obrazów z aparatu fotograficznego urządzenia. Można zaimplementować `CameraCaptureSession.CaptureListener` interfejs do obsługi nowych zdarzeń przechwytywania obrazu.

-   `CaptureRequest` &ndash; Definiuje parametry przechwytywania.

-   `CaptureResult` &ndash; Wyświetla wyniki operacji przechwytywania obrazu.

Aby uzyskać więcej informacji o nowych interfejsów API w systemie Android 5.0 aparatu zobacz [nośnika](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="audio-playback"></a>Odtwarzanie dźwięku

Android 5.0 aktualizacje `AudioTrack` klasy dla lepsze odtwarzanie dźwięku:

-   `ENCODING_PCM_FLOAT` &ndash; Konfiguruje `AudioTrack` zaakceptować danych audio w formacie liczb zmiennoprzecinkowych lepsze zakres dynamiczny, wysokość większa i wyższej jakości (dzięki użyciu większą dokładność). Ponadto pozwala uniknąć audio wycinka format liczb zmiennoprzecinkowych.

-   `ByteBuffer` &ndash; Teraz możesz podać danych audio `AudioTrack` jako tablicę bajtów.

-   `WRITE_NON_BLOCKING` &ndash; Tej opcji upraszcza buforowania i wielowątkowości w przypadku niektórych aplikacji.

Aby uzyskać więcej informacji na temat `AudioTrack` ulepszenia w systemie Android 5.0, zobacz [nośnika](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="media-playback-control"></a>Formant odtwarzania multimediów

Android 5.0 wprowadzono nowe `Android.Media.MediaController` klasy, która zastępuje `RemoteControlClient`. `Android.Media.MediaController` zapewnia kontrolę transportu uproszczonych interfejsów API i udostępnia bezpieczne wątkowo kontroli odtwarzania poza kontekstem interfejsu użytkownika. Następujących nowych interfejsów API obsługi sterowania transportu:

-   `Android.Media.Session.MediaSession` &ndash; Sesji sterowania nośnika, która obsługuje wiele kontrolerów. Należy wywołać `MediaSession.GetSessionToken` żądania tokenu, który używa aplikacji do interakcji z sesją.

-   `MediaController.TransportControls` &ndash; Uchwyty transportu poleceń, takich jak **odtwarzanie**, **zatrzymać**, i **Pomiń**.

Ponadto można użyć nowej `Android.App.Notification.MediaStyle` klasę, aby skojarzyć sesji nośnika z zawartością sformatowanego powiadomień (np. wyodrębnianie i wyświetlanie albumów).

Aby uzyskać więcej informacji na temat nowych funkcji kontroli odtwarzania multimediów w systemie Android 5.0, zobacz [nośnika](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="storage"></a>Magazyn

Android 5.0 aktualizacji Framework dostępu do magazynu w celu ułatwienia aplikacji do pracy z katalogów i dokumentów:

-   Aby wybrać poddrzewa katalogu, tworzenia i wysyłania `Android.Intent.Action.OPEN_DOCUMENT_TREE` celem. Celem tego powoduje, że system wyświetlić wszystkie wystąpienia dostawcy, które obsługuje wyboru poddrzewo; następnie przegląda i wybiera katalogu użytkownika.

-   Aby utworzyć i zarządzać nimi nowych dokumentów lub katalogów w dowolnym obszarze poddrzewo, należy użyć nowego `CreateDocument`, `RenameDocument`, i `DeleteDocument` metody `DocumentsContract`.

-   Aby uzyskać ścieżek do katalogów nośników na wszystkich urządzeniach magazynu udostępnionego, należy wywołać nowe `Android.Content.Context.GetExternalMediaDirs` metody.

Aby uzyskać więcej informacji na temat nowego magazynu, interfejsy API w systemie Android 5.0, zobacz [magazynu](http://developer.android.com/preview/api-overview.html#Storage).

### <a name="wireless--connectivity"></a>Połączenia bezprzewodowe i łączności

Android 5.0 dodaje następujące rozszerzenia interfejsu API dla sieci bezprzewodowej i łączności:

-   Nowy *wielu sieci* interfejsów API, które umożliwiają aplikacjom Znajdź i zaznacz pozycję sieci z określonymi możliwościami przed wykonaniem połączenia.

-   Funkcji emisji Bluetooth umożliwia urządzeniu z systemem Android 5.0 na działanie jako urządzenie peryferyjne niskiej energii Bluetooth.

-   Za pomocą funkcji komunikacji zbliżeniowej NFC ulepszeń, które ułatwiają udostępnianie danych z innych urządzeń.

Aby uzyskać więcej informacji o nowych sieci bezprzewodowej i łączność interfejsów API w systemie Android 5.0, zobacz [sieci bezprzewodowej i łączności](http://developer.android.com/preview/api-overview.html#Wireless).

### <a name="job-scheduling"></a>Planowanie zadań

Android 5.0 wprowadzono nowy `JobScheduler` interfejsu API, które mogą ułatwić użytkownikom zminimalizować zużycie baterii planowania niektórych zadań, aby uruchomić tylko wtedy, gdy urządzenie jest podłączone i ładowania. Ta funkcja harmonogram zadania mogą służyć do planowania zadania do uruchomienia, gdy są bardziej odpowiednie do zadania, takie jak pobieranie dużych plików, gdy urządzenie jest połączone za pośrednictwem sieci Wi-Fi, zamiast sieci taryfowej warunki.

Aby uzyskać więcej informacji na temat planowania interfejsów API w systemie Android 5.0 nowego zadania, zobacz [Planowanie zadań](http://developer.android.com/preview/api-overview.html#JobScheduler).

## <a name="summary"></a>Podsumowanie

W tym artykule pod warunkiem omówienie ważne nowe funkcje w systemie Android 5.0 dla deweloperów aplikacji platformy Xamarin.Android:

-   Materiał motywu

-   Animacji

-   Shadows widoku i podniesienia uprawnień

-   Kolor funkcje, takie jak cieniowanie obiektów drawable i poświęcone kolor wyodrębniania

-   Nowy `RecyclerView` i `CardView` widżetów

-   Ulepszenia powiadomień

-   Nowe interfejsy API aparatu, odtwarzania audio, kontrola nośnika, magazynu, sieci bezprzewodowej/łączności i planowanie zadań

Jeśli jesteś nowym użytkownikiem programowanie dla systemu Xamarin Android, przeczytaj [instalacja i Konfiguracja](~/android/get-started/installation/index.md) ułatwiające rozpoczęcie pracy z platformy Xamarin.Android.
[Witaj, Android](~/android/get-started/hello-android/index.md) jest doskonałym wprowadzenie do uczenia tworzenie projektów dla systemu Android.



## <a name="related-links"></a>Linki pokrewne

- [Android L Developer Preview](http://developer.android.com/preview/index.html)
- [Pobierz zestaw SDK systemu Android](https://developer.android.com/sdk/index.html#Other)
- [Materiał projektu](http://developer.android.com/preview/material/index.html)
- [Zasady materiału projektowania](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
