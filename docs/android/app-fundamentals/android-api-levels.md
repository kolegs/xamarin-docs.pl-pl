---
title: Opis poziomów interfejsu API systemu Android
description: Platforma Xamarin.Android ma kilka ustawienia poziomu interfejsu API systemu Android, które określają zgodność swojej aplikacji z wieloma wersjami systemu android. W tym przewodniku wyjaśniono znaczenie tych ustawień, sposobów ich konfigurowania i wpływ, jaki mają w swojej aplikacji w czasie wykonywania.
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/02/2018
ms.openlocfilehash: 3b060567b47395bc213627c9378de4fca9db41bb
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403341"
---
# <a name="understanding-android-api-levels"></a>Opis poziomów interfejsu API systemu Android

_Platforma Xamarin.Android ma kilka ustawienia poziomu interfejsu API systemu Android, które określają zgodność swojej aplikacji z wieloma wersjami systemu android. W tym przewodniku wyjaśniono znaczenie tych ustawień, sposobów ich konfigurowania i wpływ, jaki mają w swojej aplikacji w czasie wykonywania._


## <a name="quick-start"></a>Przewodnik Szybki Start

Rozszerzenie Xamarin.Android uwidacznia trzech ustawień poziomu projektu interfejsu API systemu Android:

-   [Docelowy Framework](#framework) &ndash; Określa, które środowisko do użycia podczas tworzenia aplikacji. Ten poziom interfejsu API są używane w *skompilować* czas przez rozszerzenie Xamarin.Android.

-   [Minimalna wersja systemu Android](#minimum) &ndash; określa najstarszą wersję systemu Android mają aplikacji do obsługi. Ten poziom interfejsu API są używane w *Uruchom* czasu przez system Android.

-   [Docelowa wersja systemu Android](#target) &ndash; Określa wersję systemu Android, które Twoja aplikacja jest przeznaczony do uruchamiania w. Ten poziom interfejsu API są używane w *Uruchom* czasu przez system Android.

Aby można było skonfigurować poziom interfejsu API dla projektu, należy zainstalować składniki platformy zestawu SDK dla poziomu tego interfejsu API. Aby uzyskać więcej informacji na temat pobierania i instalowania składników zestawu Android SDK, zobacz [Instalacja zestawu Android SDK](~/android/get-started/installation/android-sdk.md).

> [!NOTE]
> Począwszy od sierpnia 2018 konsolę Google Play wymaga czy nowe aplikacje docelowy poziom interfejsu API 26 (Android 8.0) lub nowszej.
Istniejące aplikacje będą musieli docelowy poziom interfejsu API 26 lub nowszej, począwszy od listopada 2018 r. Aby uzyskać więcej informacji, zobacz [poprawy zabezpieczeń aplikacji i wydajności w witrynie Google Play przez wiele lat pochodzić](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Normalnie wszystkie trzy poziomy interfejsu API platformy Xamarin.Android są ustawiane na tę samą wartość. Na **aplikacji** ustaw **kompilowanie przy użyciu wersji dla systemu Android (platforma docelowa)** do najnowszej stabilnej wersji interfejsu API (lub co najmniej do wersji dla systemu Android, która zawiera wszystkie funkcje potrzebne).
W poniższym zrzucie ekranu, platforma docelowa jest ustawiona na **Android 7.1 (interfejs API poziom 25 - określić wersję Nougat)**:

[![Domyślnie wersja Framework kompilacji przy użyciu systemu Android w wersji docelowej](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

Na **manifestu systemu Android** Ustaw wersja systemu Android, które są co najmniej **Użyj skompilować przy użyciu zestawu SDK w wersji** i równa docelowej wersji systemu Android z taką samą wartość jak wersja platformy docelowej (w następującym Zrzut ekranu, docelowej platformy systemu Android jest ustawiona na **Android 7.1 (określić wersję Nougat)**):

[![Wersje minimalna i docelowego dla systemu Android ustawiona wersja platformy docelowej](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

Jeśli chcesz zachować zgodność z poprzednimi wersjami z wcześniejszą wersją systemu android, należy ustawić **systemu Android, które są co najmniej w wersji docelowej** do najstarszą wersję systemu android mają aplikacji do obsługi. (Należy pamiętać, że poziom interfejsem API w wersji 14 minimalny poziom interfejsu API wymagane do [usług Google Play i pomocy technicznej usługi Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).) Poniższa przykładowa konfiguracja obsługuje dla systemu Android w wersjach od 14 poziom interfejsu API do poziom interfejsu API 25:

[![Kompilowanie przy użyciu poziom interfejsu API 25 określić wersję Nougat, wersja systemu Android, które są co najmniej równa poziom interfejsu API 14](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Normalnie wszystkie trzy poziomy interfejsu API platformy Xamarin.Android są ustawiane na tę samą wartość. Ustaw **platformę docelową** do najnowszej stabilnej wersji interfejsu API (lub co najmniej do wersji dla systemu Android, która zawiera wszystkie funkcje potrzebne). Aby ustawić **platformę docelową**, przejdź do **kompilacji > Ogólne** w **opcje projektu**. W poniższym zrzucie ekranu, platforma docelowa jest ustawiona na **korzystanie z najnowszej zainstalowanej platformy (8.0)**:

[![Platforma docelowa, użyj najnowszej zainstalowanej platformy przyjęto wartość domyślną](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

Ustawienia wersję minimalną i docelowego dla systemu Android można znaleźć w obszarze **kompilacji > Aplikacja dla systemu Android** w **opcje projektu**. Wersja systemu Android, które są co najmniej równa **automatyczny — Użyj docelowej wersji struktury** i równa docelowej wersji systemu Android z taką samą wartość jak wersja platformy docelowej. Na poniższym zrzucie ekranu jest równa docelowej platformy systemu Android **8.0 dla systemu Android (poziom interfejsu API 26)** do dopasowania powyższe ustawienie platforma docelowa:

[![Ustawienie framework i docelowe poziomy w opcjach projektu](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

Jeśli chcesz zachować zgodność z poprzednimi wersjami z wcześniejszą wersją systemu android, zmień **wersja systemu Android, które są co najmniej** do najstarszą wersję systemu android mają aplikacji do obsługi. Należy pamiętać, że poziom interfejsem API w wersji 14 minimalny poziom interfejsu API wymagane do [usług Google Play i pomocy technicznej usługi Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).
Na przykład następująca konfiguracja obsługuje wersje systemu Android tak szybko, jak 14 poziom interfejsu API:

[![Minimalna i wersji docelowej, automatyczny — Użyj wersji platformy docelowej](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----


Jeśli aplikacja obsługuje wielu wersji dla systemu Android, kod musi zawierać testy środowiska uruchomieniowego, aby upewnić się, że Twoja aplikacja współpracuje ze Minimum Android ustawienie wersji (zobacz [środowisko uruchomieniowe sprawdza, czy wersje systemu Android](#runtimechecks) poniżej szczegółowe informacje). Jeśli są wykorzystywanie lub tworzenia biblioteki, zobacz [poziomy interfejsu API i bibliotek](#libraries) poniżej najlepsze rozwiązania dotyczące konfigurowania interfejsu API poziomu ustawień bibliotek.



## <a name="android-versions-and-api-levels"></a>Wersje systemu android i poziomy interfejsu API

Ewoluuje wraz z platformy Android i wydawane są nowe wersje systemu Android, każda wersja systemu Android jest przypisany identyfikator unikatowy liczbą całkowitą o nazwie *poziom interfejsu API*. W związku z tym każda wersja systemu Android odnosi się do pojedynczego poziom interfejsu API systemu Android. Ponieważ użytkownicy będą instalować aplikacje na starszych również jak najnowsze wersje systemu android, praktyczne aplikacje dla systemu Android muszą być zaprojektowane do pracy z wielu poziomów interfejsu API systemu Android.


### <a name="android-versions"></a>Wersje systemu android

Wszystkie wersje systemu Android przechodzi przez wiele nazw:

-   Wersja systemu Android, takich jak **Android 7.1**
-   Element code nazwy, takie jak _określić wersję Nougat_
-   Poziom odpowiedniego interfejsu API, takich jak **poziom interfejsu API 25**

Android Nazwa kodu może odpowiadać wielu wersji i poziomy interfejsu API (jak pokazano na poniższej liście), ale każda wersja systemu Android odpowiada dokładnie jeden poziom interfejsu API.

Ponadto definiuje Xamarin.Android *kompilacji kody wersji* mapowania obecnie znane poziomy interfejsu API systemu Android. Poniższa lista może pomóc w tłumaczenie poziom interfejsu API, wersja systemu Android, Nazwa kodu i kod wersji kompilacji platformy Xamarin.Android.

-   **27 interfejsu API (8.1 dla systemu Android)** &ndash; _Oreo_, wydanej w grudniu 2017 r. Twórz kod wersji `Android.OS.BuildVersionCodes.OMr1`

-   **Interfejs API 26 (Android 8.0)** &ndash; _Oreo_, wydanej w sierpniu 2017 r. Twórz kod wersji `Android.OS.BuildVersionCodes.O`

-   **Interfejsu API 25 (Android 7.1)** &ndash; _określić wersję Nougat_, wydanej w grudniu 2016 roku. Twórz kod wersji `Android.OS.BuildVersionCodes.NMr1`

-   **Interfejs API 24 (dla systemu Android 7.0)** &ndash; _określić wersję Nougat_, wydanej w sierpniu 2016. Twórz kod wersji `Android.OS.BuildVersionCodes.N`

-   **Interfejsu API 23 (Android 6.0)** &ndash; _Marshmallow_, wydanej w sierpniu 2015. Twórz kod wersji `Android.OS.BuildVersionCodes.M`

-   **22 interfejsu API (Android 5.1)** &ndash; _interfejsu typu lizak_, wydanej w marcu 2015. Twórz kod wersji `Android.OS.BuildVersionCodes.LollipopMr1`

-   **21 interfejsu API (Android 5.0)** &ndash; _interfejsu typu lizak_, wydana w listopadzie 2014 roku. Twórz kod wersji `Android.OS.BuildVersionCodes.Lollipop`

-   **Interfejsu API 20 (Android 4.4W)** &ndash; _Obejrzyj Kitkat_, wydanej w czerwcu 2014. Twórz kod wersji `Android.OS.BuildVersionCodes.KitKatWatch`

-   **Interfejs API 19 (Android 4.4)** &ndash; _Kitkat_, wydana w październiku 2013. Twórz kod wersji `Android.OS.BuildVersionCodes.KitKat`

-   **18 interfejsu API (system Android 4.3)** &ndash; _Jelly Bean_, wydanej lipca 2013. Twórz kod wersji `Android.OS.BuildVersionCodes.JellyBeanMr2`

-   **17 interfejsu API (Android 4.2-4.2.2)** &ndash; _Jelly Bean_, wydanej listopada 2012 r. Twórz kod wersji `Android.OS.BuildVersionCodes.JellyBeanMr1`

-   **Interfejs API 16 (Android 4.1 — 4.1.1)** &ndash; _Jelly Bean_, wydanej czerwca 2012. Twórz kod wersji `Android.OS.BuildVersionCodes.JellyBean`

-   **Interfejs API 15 (Android 4.0.3-4.0.4)** &ndash; _Sandwich Ice Cream_, wydanej grudnia 2011. Twórz kod wersji `Android.OS.BuildVersionCodes.IceCreamSandwichMr1`

-   **14 interfejsu API (4.0.2 dla systemu Android 4.0)** &ndash; _Sandwich Ice Cream_, wydanej październik 2011 r. Twórz kod wersji `Android.OS.BuildVersionCodes.IceCreamSandwich`

-   **Interfejs API 13 (Android 3.2)** &ndash; _pustakowym_, wydanej w czerwcu 2011. Twórz kod wersji `Android.OS.BuildVersionCodes.HoneyCombMr2`

-   **12 API (Android 3.1.x)** &ndash; _pustakowym_, wydanej maja 2011 r. Twórz kod wersji `Android.OS.BuildVersionCodes.HoneyCombMr1`

-   **Interfejs API 11 (Android 3.0.x)** &ndash; _pustakowym_, wydanej w lutym 2011. Twórz kod wersji `Android.OS.BuildVersionCodes.HoneyComb`

-   **Interfejs API 10 (Android 2.3.3-2.3.4)** &ndash; _Gingerbread_, wydanej w lutym 2011. Twórz kod wersji `Android.OS.BuildVersionCodes.GingerBreadMr1`

-   **Interfejs API 9 (Android 2.3 2.3.2)** &ndash; _Gingerbread_, wydana w listopadzie 2010. Twórz kod wersji `Android.OS.BuildVersionCodes.GingerBread`

-   **Interfejs API 8 (dla systemu Android 2.2.x)** &ndash; _Froyo_, wydanej czerwca 2010 r. Twórz kod wersji `Android.OS.BuildVersionCodes.Froyo`

-   **Interfejs API 7 (Android 2.1.x)** &ndash; _Eclair_, wydanej stycznia 2010. Twórz kod wersji `Android.OS.BuildVersionCodes.EclairMr1`

-   **Interfejs API 6 (dla systemu Android 2.0.1)** &ndash; _Eclair_, wydanej w grudniu 2009 r. Twórz kod wersji `Android.OS.BuildVersionCodes.Eclair01`

-   **Interfejs API, 5 (dla systemu Android w wersji 2.0)** &ndash; _Eclair_, wydana w listopadzie 2009 r. Twórz kod wersji `Android.OS.BuildVersionCodes.Eclair`

-   **Interfejs API 4 (dla systemu Android w wersji 1.6)** &ndash; _pierścieniowy_, wydanej września 2009. Twórz kod wersji `Android.OS.BuildVersionCodes.Donut`

-   **3 interfejsu API (dla systemu Android w wersji 1.5)** &ndash; _Cupcake_, wydanej maj 2009 r. Twórz kod wersji `Android.OS.BuildVersionCodes.Cupcake`

-   **Interfejs API 2 (dla systemu Android 1.1)** &ndash; _Base_, wydanej w lutym 2009 r. Twórz kod wersji `Android.OS.BuildVersionCodes.Base11`

-   **Interfejs API 1 (dla systemu Android w wersji 1.0)** &ndash; _Base_, wydana w październiku 2008. Twórz kod wersji `Android.OS.BuildVersionCodes.Base`


Jak wskazuje tej listy, dla systemu Android wydawane są nowe wersje często &ndash; czasami kilku wersji rocznie. W rezultacie nieograniczonej liczby urządzeń z systemem Android, które mogą być uruchamiane aplikacji obejmuje z szerokiej gamy starszych i nowszych wersji systemu Android. Jak można zagwarantować, że aplikacja zostanie uruchomiona spójne i niezawodne tak wiele różnych wersji systemu android? Poziomy interfejsu API systemu android może pomóc Ci w zarządzaniu ten problem.


### <a name="android-api-levels"></a>Poziomy interfejsu API systemu android

Każde urządzenie Android jest uruchamiane w dokładnie *jeden* poziom interfejsu API &ndash; ten poziom interfejsu API jest musi być unikatowa dla wersji platformy systemu Android. Poziom interfejsu API dokładnie identyfikuje wersję zestawu interfejsów API, które aplikacja może wywoływać; identyfikuje kombinacji elementy manifestu, uprawnienia, itp. kod względem jako deweloper. System poziomy interfejsu API systemu android pomaga ustalić, czy aplikacja jest zgodna z obrazem systemu Android przed zainstalowaniem aplikacji na urządzeniu z systemem Android.

Podczas kompilowania aplikacji zawiera następujące informacje poziom interfejsu API:

-   *Docelowej* poziom interfejsu API systemu android, przeznaczonego do uruchamiania w aplikacji.

-   *Minimalne* poziom interfejsu API systemu Android, który musi mieć urządzenia z systemem Android, aby uruchomić aplikację. 

Te ustawienia są używane do zapewnienia, że funkcje niezbędne do poprawnego działania aplikacji dostępne na urządzeniu z systemem Android w czasie instalacji. W przeciwnym razie aplikacja jest zablokowany na tym urządzeniu. Na przykład jeśli poziom interfejsu API urządzenia z systemem Android jest niższy niż minimalny poziom interfejsu API, określające dla aplikacji, urządzenie z systemem Android uniemożliwi użytkownika instalacji aplikacji.


## <a name="project-api-level-settings"></a>Ustawienia projektu na poziomie interfejsu API

W poniższych sekcjach opisano, jak przygotować swoje Środowisko deweloperskie poziomy interfejsu API, aby skierować je, a następnie szczegółowe objaśnienia dotyczące sposobu konfigurowania za pomocą Menedżera zestawów SDK *platformę docelową*, *Minimum Wersja systemu android*, i *docelowej wersji systemu Android* ustawienia platformy Xamarin.Android.


### <a name="android-sdk-platforms"></a>Platformy zestawu SDK systemu android

Zanim będzie można wybrać docelowego lub interfejsu API z co najmniej poziomu platformie Xamarin.Android, należy zainstalować wersję platformy zestawu SDK systemu Android, który odpowiada na tym poziomie interfejsu API. Zakres dostępnych opcji platformę docelową, Minimum systemu Android w wersji i docelowej wersji systemu Android jest ograniczona do wersji zakresu dla systemu Android SDK, które zostały zainstalowane. Aby sprawdzić, czy są zainstalowane wymagane wersje zestawu SDK systemu Android i służy do dodawania żadnych nowych poziomów interfejsu API potrzebne dla aplikacji, można użyć Menedżera zestawów SDK. Jeśli nie znasz sposób instalowania poziomy interfejsu API, zobacz [Instalacja zestawu Android SDK](~/android/get-started/installation/android-sdk.md).

<a name="framework" />

### <a name="target-framework"></a>Platforma docelowa

*Platformę docelową* (znany także jako `compileSdkVersion`) jest wersja określonej platformy systemu Android (poziom interfejsu API), który Twoja aplikacja jest kompilowana w czasie kompilacji. To ustawienie określa, jakie interfejsów API aplikacji *oczekuje* do użycia podczas działa, ale nie ma wpływu, na którym interfejsy API są rzeczywiście dostępne dla aplikacji po jej zainstalowaniu. W rezultacie zmiana z ustawieniem platforma docelowa nie powoduje zmiany zachowania w czasie wykonywania.

Platforma docelowa określa wersje biblioteki, które Twoja aplikacja jest połączona względem &ndash; określa interfejsy API, których można użyć w aplikacji. Na przykład, jeśli chcesz użyć [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) metody, która została wprowadzona w systemie Android 5.0 Lollipop, należy ustawić platformę docelową **poziom interfejsu API 21 (Lollipop)** lub nowszej. Jeśli ustawisz platforma docelowa projektu do interfejsu API poziomu, takie jak **poziom interfejsu API 19 (KitKat)** , a następnie spróbuj wywołać `SetCategory` metody w kodzie, wystąpi błąd kompilacji.

Firma Microsoft zaleca, aby zawsze kompilujesz z *najnowsze* dostępnej wersji platformy docelowej. Ten sposób zapewnia pomocne komunikaty ostrzegawcze przestarzałe interfejsy API, który może być wywoływany przez kod. Za pomocą najnowszej wersji platformy docelowej jest szczególnie ważne, korzystając z najnowszych wersji biblioteki obsługi &ndash; każdej biblioteki oczekuje, że aplikacja musi być skompilowany w tej pomocy technicznej biblioteki minimalny poziom interfejsu API lub większy. 


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby uzyskać dostęp z ustawieniem platforma docelowa, w programie Visual Studio, otwórz właściwości projektu w **Eksploratora rozwiązań** i wybierz **aplikacji** strony:

[![Strony właściwości projektu aplikacji](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

Ustawić platformę docelową, wybierając poziom interfejsu API w menu rozwijane w obszarze **kompilowanie przy użyciu systemu Android w wersji** jak pokazano powyżej.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Dostępu do platformy docelowej ustawienia w programie Visual Studio dla komputerów Mac, kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz **opcje**; ta otwiera **opcje projektu** okna dialogowego. W tym oknie dialogowym Przejdź do **kompilacji > Ogólne** jak pokazano poniżej:

[![Tworzenie sekcji Ogólne na stronie Opcje projektu](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

Ustawić platformę docelową, wybierając poziom interfejsu API w menu listy rozwijanej z prawej strony **platformę docelową** jak pokazano powyżej.

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>Minimalna wersja systemu Android

*Wersja systemu Android, które są co najmniej* (znany także jako `minSdkVersion`) jest najstarszą wersję systemu operacyjnego Android (czyli najniższy poziom interfejsu API), które można zainstalować i uruchomić aplikację. Domyślnie aplikacja może składać się zainstalowanych na urządzeniach zgodnych z ustawieniem platforma docelowa lub nowszych. Jeśli to ustawienie wersji minimalnej dla systemu Android ma *niższe* niż ustawienie platforma docelowa, aplikację można również uruchomić na wcześniejszych wersjach systemu Android. Na przykład, jeśli platforma docelowa jest ustawiona na **Android 7.1 (określić wersję Nougat)** i wersja systemu Android, które są co najmniej równa **systemu Android to 4.0.3 (Ice Cream Sandwich)**, aplikację można zainstalować na dowolnej platformie, z poziomu interfejsu API 15 poziom interfejsu API 25 (włącznie).

Mimo że aplikacja może pomyślnie skompilować i zainstalować w tym zakresie platform, to nie gwarantuje, czy zostaną pomyślnie *Uruchom* we wszystkich tych platform. Na przykład, jeśli Twoja aplikacja jest zainstalowana na **Android 5.0 (Lollipop)** i kod wywołuje interfejs API, który jest dostępny tylko w **Android 7.1 (określić wersję Nougat)** i nowszych, aplikacji spowoduje błąd środowiska uruchomieniowego i być może ulec awarii. W związku z tym, kod musi zapewnić &ndash; w czasie wykonywania &ndash; wywołuje tylko tych interfejsów API, które są obsługiwane przez działającej na urządzeniu z systemem Android. Innymi słowy kod musi zawierać testy jawne środowiska uruchomieniowego, aby upewnić się, że aplikacja używa interfejsów API nowszych tylko na urządzeniach, które są wystarczająco aktualne, do ich obsługi.
[Środowisko uruchomieniowe sprawdza, czy wersje systemu Android](#runtimechecks), później w tym przewodniku wyjaśniono, jak dodać te testy środowiska uruchomieniowego w kodzie.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby uzyskać dostęp do Minimum Android ustawienie wersji w programie Visual Studio, otwórz właściwości projektu w **Eksploratora rozwiązań** i wybierz **manifestu systemu Android** strony. W menu rozwijane w obszarze **wersja systemu Android, które są co najmniej** wersja systemu Android, które są co najmniej można wybrać dla swojej aplikacji:

[![Minimalna systemu Android, aby opcja docelowej równa kompilowanie przy użyciu wersji zestawu SDK](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

Jeśli wybierzesz **Użyj skompilować przy użyciu zestawu SDK w wersji**, będzie taka sama jak ustawienie platforma docelowa wersja systemu Android, które są co najmniej.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Można uzyskać dostęp do systemu Android, które są co najmniej wersji w programie Visual Studio dla komputerów Mac, kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz **opcje**; ta otwiera **opcje projektu** okna dialogowego. Przejdź do **kompilacji > Aplikacja dla systemu Android**.
Za pomocą listy rozwijanej z prawej strony **wersja systemu Android, które są co najmniej**, wersja systemu operacyjnego Minimum Android można ustawić dla aplikacji:

[![Minimalna wersja systemu Android automatyczny — Użyj docelowej wersji struktury](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

Jeśli wybierzesz **automatyczne &ndash; Użyj docelowej wersji struktury**, będzie taka sama jak ustawienie platforma docelowa wersja systemu Android, które są co najmniej.

-----


<a name="target" />

### <a name="target-android-version"></a>Docelowa wersja systemu Android

*Docelowa wersja systemu Android* (znany także jako `targetSdkVersion`) jest interfejs API, urządzenie z systemem Android na poziomie, gdy aplikacja oczekuje, że do uruchomienia. System android używa tego ustawienia w celu określenia, czy włączyć zachowania zgodności &ndash; daje to gwarancję, że aplikacja nadal działać w oczekiwany sposób. System android używa Target Android ustawienie wersji aplikacji ustalić zmiany zachowania, które można stosować do aplikacji bez podzielenie go (jest to jak Android zapewnia zgodność z nowszymi wersjami).

Platforma docelowa i docelowej wersji systemu Android, a jednocześnie ma bardzo podobnych nazwach, nie są tak samo. Ustawienie platforma docelowa komunikuje się docelowego interfejsu API informacji na temat poziomu Xamarin.Android do użycia w *czas kompilacji*, podczas gdy docelowej wersji systemu Android komunikuje się docelowego interfejsu API informacji na temat poziomu systemu Android do użycia w  *czas wykonywania* (Jeśli aplikacja jest zainstalowana i uruchomiona na urządzeniu).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby uzyskać dostęp do tego ustawienia w programie Visual Studio, otwórz właściwości projektu w **Eksploratora rozwiązań** i wybierz **manifestu systemu Android** strony. W menu rozwijane w obszarze **docelowej wersji systemu Android** docelowej wersji systemu Android można wybrać dla swojej aplikacji:

[![Docelowa wersja systemu Android równa kompilowanie przy użyciu wersji zestawu SDK](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

Firma Microsoft zaleca jawnie ustawić docelowej wersji systemu Android do najnowszej wersji systemu android, których używasz do testowania aplikacji. W idealnym przypadku powinien być ustawiony na najnowszej wersji zestawu SDK systemu Android &ndash; pozwala na używanie nowych interfejsów API przed pracy nad zmiany zachowania. Dla większości deweloperów firma Microsoft *nie* zaleca się ustawienie docelowej wersji systemu Android **Użyj skompilować przy użyciu zestawu SDK w wersji**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby uzyskać dostęp do tego ustawienia w programie Visual Studio dla komputerów Mac, kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz **opcje**; ta otwiera **opcje projektu** okna dialogowego. Przejdź do **kompilacji > Aplikacja dla systemu Android**. Za pomocą listy rozwijanej z prawej strony **docelowej wersji systemu Android**, można ustawić docelowej wersji systemu Android dla aplikacji:

[![Docelowa wersja systemu Android jest ustawiona na automatyczny — Użyj docelowej wersji struktury](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

Firma Microsoft zaleca jawnie ustawić docelowej wersji systemu Android do najnowszej wersji systemu android, których używasz do testowania aplikacji. W idealnym przypadku powinien być ustawiony na najnowszej dostępnej wersji zestawu SDK systemu Android &ndash; pozwala na używanie nowych interfejsów API przed pracy nad zmiany zachowania. Dla większości deweloperów, nie zaleca się ustawienie docelowej wersji systemu Android **automatyczny — Użyj docelowej wersji struktury**.

-----

Ogólnie rzecz biorąc docelowa wersja systemu Android powinny być ograniczone przez minimalna wersja systemu operacyjnego Android i platformy docelowej. To znaczy:

**Minimalna wersja systemu Android < = docelowa wersja systemu Android < = platformy docelowej**

Aby uzyskać więcej informacji na temat poziomów zestawu SDK, zobacz dewelopera systemu Android [zestaw sdk używa](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) dokumentacji.


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Środowisko uruchomieniowe sprawdza, czy wersje systemu Android

Zgodnie z każdą nową wersją systemu Android jest zwolniony, framework interfejsu API jest aktualizowana w celu Podaj nowy lub zastąpienie funkcji. Z pewnymi wyjątkami funkcje interfejsu API z wcześniejszych wersji systemu Android jest przenoszone do nowszej wersji systemu Android bez modyfikacji. W rezultacie jeśli aplikacja działa na określonym poziomie interfejsu API systemu Android, zazwyczaj będzie można uruchamiać na nowsze poziom interfejsu API systemu Android bez modyfikacji. Ale co zrobić, jeśli użytkownik chce również uruchomić aplikację we wcześniejszych wersjach systemu Android?

Jeśli zostanie wybrana wersja systemu Android, które są co najmniej, który jest *niższe* niż z ustawieniem platforma docelowa niektóre interfejsy API mogą nie być dostępne do aplikacji w czasie wykonywania. Jednak aplikacja nadal można uruchomić na urządzeniu z systemem wcześniejszej, ale z ograniczoną funkcjonalnością. Dla każdego interfejsu API, który nie jest dostępny na platformach Android odpowiadający ustawienia systemu Android, które są co najmniej wersji, kod musi jawnie sprawdziła wartość `Android.OS.Build.VERSION.SdkInt` własność, aby określić poziom interfejsu API platformy, aplikacja jest uruchomiona na. Jeśli poziom interfejsu API jest *niższe* niż minimalna wersja systemu operacyjnego Android, która obsługuje interfejs API, który chcesz wybrać, a następnie kod musi znaleźć sposób działała poprawnie, bez konieczności szukania to wywołanie interfejsu API.

Na przykład, załóżmy, że chcemy użyć [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) metodę, aby kategoryzować powiadomienia podczas uruchamiania na **Android 5.0 Lollipop** (i nowszych), ale nadal chcemy, aby nasza aplikacja do Uruchamianie we wcześniejszych wersjach systemu Android, takich jak **Android 4.1 X201ejelly Bean** (gdzie `SetCategory` nie jest dostępna). Odwołujące się do tabeli systemu Android w wersji na początku tego przewodnika, widzimy, że kod wersji kompilacji **Android 5.0 Lollipop** jest `Android.OS.BuildVersionCodes.Lollipop`. Do obsługi starszych wersji systemu Android gdzie `SetCategory` jest niedostępne, naszego kodu można wykrywać poziom interfejsu API w czasie wykonywania i warunkowe wywołanie `SetCategory` tylko gdy, poziom interfejsu API jest większa lub równa kod wersji kompilacji interfejsu typu lizak:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

W tym przykładzie jest równa wartości docelowej naszej aplikacji **Android 5.0 (poziom interfejsu API 21)** i ustawiono jej wersja systemu Android, które są co najmniej **Android 4.1 (16 poziom interfejsu API)**. Ponieważ `SetCategory` jest dostępna na poziomie interfejsu API `Android.OS.BuildVersionCodes.Lollipop` i nowszy, ten przykładowy kod wywoła `SetCategory` tylko gdy jest rzeczywiście dostępne &ndash; będzie *nie* podjęto próbę wywołania `SetCategory` podczas interfejsu API poziom jest 16, 17, 18, 19 i 20. Funkcje zmniejszono na tych starszych wersji systemu Android tylko, w jakim powiadomienia nie są sortowane poprawnie (ponieważ nie są one pogrupowane według typu), ale powiadomienia są nadal publikowane, aby ostrzec użytkownika. Nasza aplikacja nadal działa, ale jego działanie nieco będzie mniejsza.

Ogólnie rzecz biorąc Kontrola wersji kompilacji pomaga kodu w czasie wykonywania między robi, nowy sposób i stary sposób zdecyduj. Na przykład:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    // Do things the Lollipop way
}
else
{
    // Do things the pre-Lollipop way
}
```

Nie istnieje żadna reguła szybki i prosty, który wyjaśnia sposób zmniejszenia lub zmodyfikować funkcje aplikacji uruchomionej na starsze wersje systemu Android, których brakuje co najmniej jednego interfejsu API. W niektórych przypadkach (np. w `SetCategory` w powyższym przykładzie), wystarczy po prostu pominąć wywołania interfejsu API, gdy nie jest dostępna. Jednak w innych przypadkach może być konieczne implementacji funkcji alternatywne, kiedy `Android.OS.Build.VERSION.SdkInt` wykryciu jest mniejsza niż interfejs API poziomu, że Twoja aplikacja wymaga prezentować swoje doświadczenie optymalne.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>Poziomy interfejsu API i bibliotek

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Podczas tworzenia projektu platformy Xamarin.Android biblioteki (np. biblioteki klas lub biblioteka powiązań) można skonfigurować z ustawieniem platforma docelowa &ndash; wersja systemu operacyjnego Minimum Android i ustawienia systemu Android, które są docelowej wersji nie są dostępne. To, ponieważ nie istnieje żadne **manifestu systemu Android** strony:

[![Tylko do kompilacji przy użyciu opcji systemu Android w wersji jest dostępna](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Podczas tworzenia projektu biblioteki platformy Xamarin.Android jest nie **aplikacji dla systemu Android** strony, w którym można skonfigurować Minimum Android wersji i docelowej wersji systemu Android &ndash; co najmniej systemu Android w wersji i docelowej Ustawienia systemu android w wersji nie są dostępne.
To, ponieważ nie istnieje żadne **kompilacji > aplikacji dla systemu Android** strony):

[![Strona ogólne bez opcji wersji minimalnej i docelowej kompilacji](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

Wersja systemu Android, które są co najmniej i ustawień wersji docelowej systemu Android nie są dostępne, ponieważ wynikowy biblioteki nie jest aplikacją autonomiczną &ndash; biblioteki, można uruchomić w dowolnej wersji systemu Android, w zależności od aplikacji, która jest dostarczana z. Można określić, jak biblioteki ma być *skompilowany*, ale nie można przewidzieć, jaką platformę poziom interfejsu API biblioteki będą korzystać. Mając to na uwadze następujące najlepsze rozwiązania należy przestrzegać podczas używania lub tworzenie bibliotek:

-   **Podczas korzystania z biblioteki systemu Android** &ndash; Jeśli biblioteka systemu Android korzystających w aplikacji, należy ustawić platformę docelową aplikacji ustawienie do interfejsu API poziomu oznacza to *przynajmniej tak duży jak* element docelowy Ustawienia Framework biblioteki.

-   **Podczas tworzenia biblioteki systemu Android** &ndash; Jeśli tworzysz biblioteki systemu Android do użytku przez inne aplikacje, pamiętaj ustawienie minimalny poziom interfejsu API, ile potrzebuje, aby skompilować jego platformę docelową.

Następujące najlepsze rozwiązania są zalecane, aby zapobiec sytuacji, w którym bibliotekę podejmuje próbę wywołania interfejsu API, która nie jest dostępna w czasie wykonywania (co może spowodować, że aplikacja awarii). Jeśli jesteś deweloperem biblioteki, powinien Dokładamy wszelkich starań ograniczyć użycie wywołań interfejsu API dla małych i sprawdzone podzbioru całkowity obszar powierzchni interfejsu API. Pomaga upewnić się, że biblioteki może być używany w szerszym wersjach zakresu z systemem Android.


## <a name="summary"></a>Podsumowanie

W przewodniku wyjaśniono, jak poziomy interfejsu API systemu Android są używane do zarządzania zgodność aplikacji w różnych wersjach systemu Android. Pod warunkiem szczegółowe instrukcje dotyczące konfigurowania Xamarin.Android *platformę docelową*, *wersja systemu Android, które są co najmniej*, i *docelowej wersji systemu Android* ustawienia projektu. Ona udostępniana instrukcje dotyczące przy użyciu Menedżera zestawów Android SDK, aby zainstalować pakiety zestawu SDK uwzględnione przykłady sposobu pisania kodu, aby poradzić sobie z różnymi poziomami interfejsu API w czasie wykonywania i wyjaśniono, jak zarządzać poziomy interfejsu API podczas tworzenia lub używania biblioteki systemu Android. Ona również udostępniana kompleksowej listy, które odnoszą się poziomy interfejsu API do numery wersji systemu Android (na przykład dla systemu Android 4.4), nazwy systemu Android w wersji (na przykład Kitkat) i kody wersji kompilacji platformy Xamarin.Android.


## <a name="related-links"></a>Linki pokrewne

- [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md)
- [Zmienia narzędzi interfejsu wiersza polecenia zestawu SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Pobieranie Twojej compileSdkVersion minSdkVersion i targetSdkVersion](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [Co to jest poziom interfejsu API?](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [Codenames, znaczników oraz numery kompilacji](https://source.android.com/source/build-numbers)
