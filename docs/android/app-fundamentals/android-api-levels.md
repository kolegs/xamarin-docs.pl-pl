---
title: Opis poziomów interfejsu API systemu Android
description: Xamarin.Android ma kilka ustawienia poziom interfejsu API systemu Android, które określają zgodność aplikacji z wieloma wersjami systemu android. W tym przewodniku wyjaśniono znaczenie tych ustawień, jak skonfigurować je i wpływ, jaki mają w aplikacji w czasie wykonywania.
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: b942bb1be3441b1fb1a8bd65016914b3ecddbb26
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732323"
---
# <a name="understanding-android-api-levels"></a>Opis poziomów interfejsu API systemu Android

_Xamarin.Android ma kilka ustawienia poziom interfejsu API systemu Android, które określają zgodność aplikacji z wieloma wersjami systemu android. W tym przewodniku wyjaśniono znaczenie tych ustawień, jak skonfigurować je i wpływ, jaki mają w aplikacji w czasie wykonywania._


## <a name="quick-start"></a>Szybki Start

Xamarin.Android udostępnia trzy ustawienia poziomu projektu interfejsu API systemu Android:

-   [Platformy docelowej](#framework) &ndash; Określa, które framework do użycia z tworzeniem aplikacji. Ten poziom interfejsu API jest używany w *skompilować* czasu przez platformy Xamarin.Android.

-   [Minimalna wersja systemu Android](#minimum) &ndash; określa najstarsza wersja systemu Android, które mają aplikacji do obsługi. Ten poziom interfejsu API jest używany w *Uruchom* czasu przez system Android.

-   [Docelowa wersja systemu Android](#target) &ndash; określa systemu android, która aplikacja jest przeznaczona do uruchamiania na. Ten poziom interfejsu API jest używany w *Uruchom* czasu przez system Android.

Zanim będzie można skonfigurować poziom interfejsu API dla projektu, należy zainstalować składniki platformy SDK na danym poziomie interfejsu API. Aby uzyskać więcej informacji o pobieraniu i instalowaniu składników zestawu SDK systemu Android, zobacz [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md).

> [!NOTE]
> Począwszy od sierpnia 2018 konsoli Google Play będzie wymagać czy nowych aplikacji docelowy poziom interfejsu API 26 (8.0 dla systemu Android) lub nowszej.
Istniejące aplikacje będą musieli docelowy poziom interfejsu API 26 lub nowszej, począwszy od listopada 2018. Aby uzyskać więcej informacji, zobacz [poprawy zabezpieczeń aplikacji i wydajności w witrynie Google Play lat do](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Zwykle wszystkie trzy poziomy interfejsu API platformy Xamarin.Android są ustawiane na tę samą wartość. Na **aplikacji** ustaw **skompilować przy użyciu wersji dla systemu Android (platforma docelowa)** do najnowsza stabilna wersja interfejsu API (lub co najmniej do wersji dla systemu Android, która zawiera wszystkie funkcje niezbędne).
Na poniższym zrzucie ekranu, platforma docelowa ma ustawioną wartość **7.1 systemu Android (interfejs API poziom 25 - nugacie)**:

[![TARGET Framework w wersji, wartością domyślną będzie kompilacji przy użyciu wersji dla systemu Android](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

Na **manifestu systemu Android** Ustaw Minimum Android wersji **Użyj skompilować przy użyciu zestawu SDK w wersji** i ustaw wersję docelowej dla systemu Android do taką samą wartość jak wersja platformy docelowej (w następujących Zrzut ekranu, docelowej platformy systemu Android ma ustawioną wartość **Android 7.1 (nugacie)**):

[![Wartość minimalna i Android docelowej wersji wersja platformy docelowej](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

Jeśli chcesz zachować zgodność z poprzednimi wersjami z wcześniejszą wersją systemu android, ustaw **Minimum Android wersja docelowego** do najstarsza wersja systemu android, które mają aplikacji do obsługi. (Należy pamiętać, że 14 poziom interfejsu API minimalny poziom interfejsu API wymagane do [usług Google Play i pomocy technicznej Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).) Poniższa przykładowa konfiguracja obsługuje systemu Android w wersjach od 14 poziom interfejsu API za pośrednictwem poziom interfejsu API 25:

[![Kompilowanie przy użyciu interfejsu API poziomu 25 nugacie, wersja minimalna Android ustawiony poziom interfejsu API 14](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Zwykle wszystkie trzy poziomy interfejsu API platformy Xamarin.Android są ustawiane na tę samą wartość. Ustaw **platformy docelowej** do najnowsza stabilna wersja interfejsu API (lub co najmniej do wersji dla systemu Android, która zawiera wszystkie funkcje niezbędne). Aby ustawić **platformy docelowej**, przejdź do **kompilacji > Ogólne** w **opcje projektu**. Na poniższym zrzucie ekranu, platforma docelowa ma ustawioną wartość **użyj najnowszej zainstalowanej platformy (8.0)**:

[![Platforma docelowa przyjęty Użyj zainstalowana najnowsza wersja platformy](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

Ustawienia wersji minimalnej i docelowych z systemem Android można znaleźć w **kompilacji > aplikacji systemu Android** w **opcje projektu**. Ustaw wersję Minimum Android do **automatyczny — Użyj docelowej framework w wersji** i ustaw wersję docelowej dla systemu Android do taką samą wartość jak wersja platformy docelowej. Na poniższym zrzucie ekranu docelowej platformy systemu Android ma ustawioną wartość **8.0 dla systemu Android (interfejs API na poziomie 26)** odpowiadające powyższe ustawienia platformy docelowej:

[![Ustawianie framework i docelowe poziomy w opcje projektu](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

Jeśli chcesz zachować zgodność z poprzednimi wersjami z wcześniejszą wersją systemu android, zmień **Minimum Android wersji** do najstarsza wersja systemu android, które mają aplikacji do obsługi. Należy pamiętać, że 14 poziom interfejsu API minimalny poziom interfejsu API wymagane do [usług Google Play i pomocy technicznej Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).
Na przykład następująca konfiguracja obsługuje wersje systemu Android jak 14 poziom interfejsu API:

[![Minimalna i wersji docelowej ustawiono automatyczne - Użyj wersja docelowego frameworka](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----


Jeśli aplikacja obsługuje wiele wersji dla systemu Android, kod musi zawierać Sprawdzanie czasu wykonania, aby upewnić się, że aplikacja działa z Minimum Android ustawienie wersji (zobacz [Sprawdzanie czasu wykonania dla systemu Android wersji](#runtimechecks) poniżej szczegółowe informacje). Korzystanie z lub tworzenia biblioteki, zobacz [poziomy interfejsu API i biblioteki](#libraries) poniżej najlepsze rozwiązania w konfigurowaniu interfejsu API poziomu ustawień bibliotek.



## <a name="android-versions-and-api-levels"></a>Wersje systemu android i poziomy interfejsu API

Platformy systemu Android rozwoju i wydawane są nowe wersje systemu Android, każdej wersji systemu Android jest przypisany identyfikator unikatowy liczbą całkowitą o nazwie *poziom interfejsu API*. W związku z tym każda wersja systemu Android odpowiada jednej poziom interfejsu API systemu Android. Ponieważ użytkownicy będą instalować aplikacje w starszych wersjach również jako ostatni systemu android, praktyczne aplikacje dla systemu Android muszą być zaprojektowane do pracy z różnych poziomach interfejsu API systemu Android.


### <a name="android-versions"></a>Wersje systemu android

Każde wydanie systemu Android przechodzi przez wiele nazw:

-   Wersja systemu Android, takie jak **7.1 systemu Android**
-   A kodem nazwy, takie jak _nugacie_
-   Poziom odpowiedniego interfejsu API, takich jak **poziom interfejsu API 25**

Android Nazwa kodu może odpowiadać wielu wersji i poziomy interfejsu API (jak pokazano na poniższej liście), ale każda wersja systemu Android odpowiada dokładnie jeden poziom interfejsu API.

Ponadto definiuje Xamarin.Android *kody wersji kompilacji* mapowania znane obecnie poziomy interfejsu API systemu Android. Poniższa lista może pomóc tłumaczenia poziom interfejsu API, wersja systemu Android o nazwie kodowej i kod wersji kompilacji platformy Xamarin.Android.

-   **27 interfejsu API (8.1 dla systemu Android)** &ndash; _Oreo_, zwolniony grudnia 2017 r. Kod wersji kompilacji `Android.OS.BuildVersionCodes.OMr1`

-   **Interfejs API 26 (8.0 dla systemu Android)** &ndash; _Oreo_, zwolniony 2017 sierpnia. Kod wersji kompilacji `Android.OS.BuildVersionCodes.O`

-   **25 interfejsu API (Android 7.1)** &ndash; _nugacie_, zwolniony grudnia 2016. Kod wersji kompilacji `Android.OS.BuildVersionCodes.NMr1`

-   **Interfejs API 24 (z systemem Android 7.0)** &ndash; _nugacie_, zwolniony sierpnia 2016 r. Kod wersji kompilacji `Android.OS.BuildVersionCodes.N`

-   **Interfejs API 23 (Android 6.0)** &ndash; _Marshmallow_, zwolniony sierpnia 2015. Kod wersji kompilacji `Android.OS.BuildVersionCodes.M`

-   **22 interfejsu API (z systemem Android w wersji 5.1)** &ndash; _interfejs typu lizak_, zwolniony marca 2015 roku. Kod wersji kompilacji `Android.OS.BuildVersionCodes.LollipopMr1`

-   **Interfejs API 21 (Android 5.0)** &ndash; _interfejs typu lizak_, wydanej w listopadzie 2014 r. Kod wersji kompilacji `Android.OS.BuildVersionCodes.Lollipop`

-   **20 interfejsu API (Android 4.4W)** &ndash; _czujki Kitkat_, zwolniony czerwca 2014 r. Kod wersji kompilacji `Android.OS.BuildVersionCodes.KitKatWatch`

-   **Interfejs API 19 (z systemem Android 4.4)** &ndash; _Kitkat_, zwolniony października 2013. Kod wersji kompilacji `Android.OS.BuildVersionCodes.KitKat`

-   **Interfejs API 18 (z systemem Android 4.3)** &ndash; _ziarna galaretki_, zwolniony lipca 2013. Kod wersji kompilacji `Android.OS.BuildVersionCodes.JellyBeanMr2`

-   **17 interfejsu API (Android 4.2-4.2.2)** &ndash; _ziarna galaretki_, zwolniony listopad 2012. Kod wersji kompilacji `Android.OS.BuildVersionCodes.JellyBeanMr1`

-   **Interfejs API 16 (z systemem Android 4.1-4.1.1)** &ndash; _ziarna galaretki_, zwolniony czerwca 2012. Kod wersji kompilacji `Android.OS.BuildVersionCodes.JellyBean`

-   **Interfejs API 15 (Android 4.0.3-4.0.4)** &ndash; _Sandwich lodów_, zwolniony grudnia 2011. Kod wersji kompilacji `Android.OS.BuildVersionCodes.IceCreamSandwichMr1`

-   **14 interfejsu API (Android 4.0-4.0.2)** &ndash; _Sandwich lodów_, zwolniony października 2011. Kod wersji kompilacji `Android.OS.BuildVersionCodes.IceCreamSandwich`

-   **Interfejs API 13 (Android 3.2)** &ndash; _pustakowym_, zwolniony czerwca 2011. Kod wersji kompilacji `Android.OS.BuildVersionCodes.HoneyCombMr2`

-   **12 interfejsu API (Android 3.1.x)** &ndash; _pustakowym_, zwolniony maja 2011. Kod wersji kompilacji `Android.OS.BuildVersionCodes.HoneyCombMr1`

-   **Interfejs API 11 (Android 3.0.x)** &ndash; _pustakowym_, zwolniony lutego 2011. Kod wersji kompilacji `Android.OS.BuildVersionCodes.HoneyComb`

-   **Interfejs API 10 (Android 2.3.3-2.3.4)** &ndash; _pierniki_, zwolniony lutego 2011. Kod wersji kompilacji `Android.OS.BuildVersionCodes.GingerBreadMr1`

-   **Interfejs API 9 (Android 2.3 2.3.2)** &ndash; _pierniki_, wydanej w listopadzie 2010. Kod wersji kompilacji `Android.OS.BuildVersionCodes.GingerBread`

-   **8 interfejsu API (Android 2.2.x)** &ndash; _Froyo_, wydanej w czerwcu 2010. Kod wersji kompilacji `Android.OS.BuildVersionCodes.Froyo`

-   **Interfejs API 7 (Android 2.1.x)** &ndash; _Eclair_, zwolniony stycznia 2010. Kod wersji kompilacji `Android.OS.BuildVersionCodes.EclairMr1`

-   **Interfejs API 6 (z systemem Android 2.0.1)** &ndash; _Eclair_, zwolniony grudnia 2009. Kod wersji kompilacji `Android.OS.BuildVersionCodes.Eclair01`

-   **Interfejs API 5 (Android 2.0)** &ndash; _Eclair_, wydanej w listopadzie 2009. Kod wersji kompilacji `Android.OS.BuildVersionCodes.Eclair`

-   **Interfejs API 4 (dla systemu Android w wersji 1.6)** &ndash; _pierścień_, zwolniony września 2009. Kod wersji kompilacji `Android.OS.BuildVersionCodes.Donut`

-   **Interfejs API 3 (z systemem Android w wersji 1.5)** &ndash; _Cupcake_, zwolniony maja 2009. Kod wersji kompilacji `Android.OS.BuildVersionCodes.Cupcake`

-   **Interfejs API 2 (1.1 dla systemu Android)** &ndash; _Base_, zwolniony lutego 2009. Kod wersji kompilacji `Android.OS.BuildVersionCodes.Base11`

-   **Interfejs API 1 (Android 1.0)** &ndash; _Base_, zwolniony października 2008. Kod wersji kompilacji `Android.OS.BuildVersionCodes.Base`


Ponieważ ta lista wskazuje, Android wydawane są nowe wersje często &ndash; czasami kilku wersji w roku. W związku z tym całość urządzeń z systemem Android, które mogą być uruchamiane aplikacji obejmuje różne starszych i nowszych wersjach systemu Android. Jak można zagwarantować, że aplikacja zostanie uruchomiona spójnie i niezawodne na tyle różne wersje systemu android? Poziomy interfejsu API systemu android firmy ułatwia zarządzanie ten problem.


### <a name="android-api-levels"></a>Poziomy interfejsu API systemu android

Każde urządzenie z systemem Android jest uruchamiane w dokładnie *jeden* poziom interfejsu API &ndash; ten poziom interfejsu API jest musi być unikatowy dla poszczególnych wersji platformy Android. Poziom interfejsu API dokładnie identyfikuje wersję zestawu interfejsów API, które wywołują można aplikacji; identyfikuje kombinacja elementów manifestu, uprawnień, itp. kod dla deweloperów. System android firmy poziomy interfejsu API pomaga ustalić, czy aplikacja jest zgodna z obrazem systemu Android, przed zainstalowaniem aplikacji na urządzeniach Android.

Podczas tworzenia aplikacji zawiera następujące informacje poziom interfejsu API:

-   *Docelowej* poziom interfejsu API systemu android, korzysta z wbudowanej aplikacji do uruchamiania na.

-   *Minimalna* poziom interfejsu API systemu Android, urządzenia z systemem Android muszą mieć do uruchomienia aplikacji. 

Te ustawienia są używane do zapewnienia, że funkcje niezbędne do poprawnego działania aplikacji jest dostępny na urządzeniu z systemem Android w czasie instalacji. Jeśli nie, aplikacja jest blokowany działające na tym urządzeniu. Na przykład jeśli poziom interfejsu API urządzenia z systemem Android jest niższy niż minimalny poziom interfejsu API, określony dla aplikacji, urządzenia Android uniemożliwi użytkownika instalowania aplikacji.


## <a name="project-api-level-settings"></a>Ustawienia projektu poziom interfejsu API

W poniższych sekcjach opisano, jak przygotować poziomy interfejsu API ma być docelowa, a następnie szczegółowe wyjaśnienia dotyczące sposobu konfigurowania środowiska deweloperskiego przy użyciu Menedżera SDK *platformy docelowej*, *minimalna Wersja systemu android*, i *wersji docelowej Android* ustawienia platformy Xamarin.Android.


### <a name="android-sdk-platforms"></a>Zestaw android SDK platformy

Przed wybraniem poziomu docelowego lub co najmniej interfejsu API w Xamarin.Android, należy zainstalować wersję platformy zestawu SDK systemu Android, odpowiadającego na tym poziomie interfejsu API. Zakres dostępnych opcji dla platformy docelowej, Minimum Android wersji i wersji docelowej Android jest ograniczona do wersji zakresu z zestawu SDK systemu Android, które zainstalowano. Aby sprawdzić, czy są zainstalowane wymagane wersje zestawu SDK systemu Android i można go dodać wszystkie nowe poziomy interfejsu API wymagane dla aplikacji, można użyć SDK Manager. Jeśli nie masz doświadczenia w obsłudze sposobu instalowania poziomy interfejsu API, zobacz [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md).

<a name="framework" />

### <a name="target-framework"></a>Platforma docelowa

*Platformy docelowej* (znanej także jako `compileSdkVersion`) jest skompilowany dla aplikacji w czasie kompilacji wersji określonej platformy systemu Android (poziom interfejsu API). To ustawienie określa, jakie interfejsów API aplikacji *oczekuje* do użycia, gdy zostanie uruchomiony, ale nie ma wpływu na którym interfejsy API są faktycznie dostępne dla aplikacji po jej zainstalowaniu. W związku z tym ustawieniem platforma docelowa nie Zmiana zachowania w czasie wykonywania.

Platforma docelowa identyfikuje których aplikacja jest połączony z wersji biblioteki &ndash; określa interfejsów API, które można użyć w aplikacji. Na przykład, jeśli chcesz użyć [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) metodę, która została wprowadzona w Android 5.0 interfejs typu lizak, należy ustawić platformę docelową **21 poziom interfejsu API (interfejs typu lizak)** lub nowszym. Jeśli ustawisz platformy docelowej projektu do interfejsu API poziomu, takich jak **19 poziom interfejsu API (KitKat)** i spróbuj `SetCategory` metody w kodzie, wystąpi błąd kompilacji.

Firma Microsoft zaleca, aby zawsze kompilacja z *najnowsze* dostępna wersja platformy docelowej. W ten sposób zapewnia przydatne komunikaty ostrzegawcze dla przestarzałe interfejsy API, który może być wywoływany przez kod. Jest używana najnowsza wersja platformy docelowej jest szczególnie ważne, gdy używasz najnowszej wersji biblioteki obsługi &ndash; każdej biblioteki oczekuje aplikacji musi być skompilowany w minimalny poziom interfejsu API tej biblioteki pomocy technicznej lub większy. 


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dostępu do platformy docelowej ustawienia w programie Visual Studio, otwórz właściwości projektu w **Eksploratora rozwiązań** i wybierz **aplikacji** strony:

[![Strona aplikacji właściwości projektu](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

Określ platformę docelową, wybierając poziom interfejsu API w menu rozwijane w obszarze **skompilować przy użyciu wersji Android** zgodnie z powyższym.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby uzyskać dostęp do ustawienia platformy docelowej w programie Visual Studio for Mac, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **opcje**; ten otwiera **opcje projektu** okna dialogowego. W tym oknie dialogowym, przejdź do **kompilacji > Ogólne** w sposób pokazany poniżej:

[![Tworzenie ogólnych sekcji Opcje projektu strony](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

Określ platformę docelową, wybierając poziom interfejsu API w menu rozwijanym z prawej strony **platformy docelowej** zgodnie z powyższym.

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>Minimalna wersja systemu Android

*Wersji Minimum Android* (znanej także jako `minSdkVersion`) jest najstarszą wersję systemu operacyjnego Android (tj., najniższy poziom interfejsu API), które można zainstalować i uruchomić aplikację. Domyślnie aplikacja może być tylko zainstalowany na urządzeniach, które pasują do ustawienia platformy docelowej lub wyższy. Jeśli jest Minimum Android ustawienie wersji *niższe* niż ustawienie platformy docelowej aplikacji można również uruchomić we wcześniejszych wersjach systemu android. Na przykład, jeśli ustawiono platformę docelową **Android 7.1 (nugacie)** i ustaw wersję Minimum Android do **Android 4.0.3 (lodów Sandwich)**, można zainstalować aplikację na dowolnej platformie z poziomu interfejsu API 15 poziom interfejsu API 25 włącznie.

Chociaż aplikacji może pomyślnie kompilacji i instaluje ten zakres platform, nie gwarantuje pomyślnie będzie *Uruchom* we wszystkich tych platform. Na przykład, jeśli aplikacja jest zainstalowana na **Android 5.0 (interfejs typu lizak)** i kod wywołuje interfejs API, który jest dostępny tylko w **Android 7.1 (nugacie)** i nowszych, aplikacji spowoduje błąd środowiska uruchomieniowego i może ulec awarii. W związku z tym kodzie musi zapewnić &ndash; w czasie wykonywania &ndash; wywołuje tylko tych interfejsów API, które są obsługiwane przez działającej na urządzeniu z systemem Android. Innymi słowy kodu musi zawierać sprawdza jawne środowiska uruchomieniowego, upewnij się, że Twoja aplikacja korzysta z interfejsów API nowszej tylko na urządzeniach, które są wystarczająco aktualna, do ich obsługi.
[Sprawdzanie czasu wykonania dla systemu Android wersji](#runtimechecks)w dalszej części tego przewodnika wyjaśniono, jak dodawać kontrole środowiska uruchomieniowego w kodzie.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby uzyskać dostęp do Minimum Android ustawienie wersji programu Visual Studio, otwórz właściwości projektu w **Eksploratora rozwiązań** i wybierz **manifestu systemu Android** strony. W menu rozwijane w obszarze **Minimum Android wersji** można wybrać wersję Minimum Android aplikacji:

[![Minimalna systemu Android do opcji docelowej skompilować przy użyciu zestawu SDK w wersji](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

W przypadku wybrania **Użyj skompilować przy użyciu zestawu SDK w wersji**, wersja minimalna Android będzie taka sama jak ustawienie platformy docelowej.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby uzyskać dostęp do ustawienia platformy docelowej w programie Visual Studio for Mac, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **opcje**; ten otwiera **opcje projektu** okna dialogowego. Przejdź do **kompilacji > aplikacji systemu Android**.
Za pomocą listy rozwijanej z prawej strony **Minimum Android wersji**, można ustawić Minimum Android wersji aplikacji:

[![Minimalna wersja systemu Android ustawiono automatyczne - Użyj docelowej framework w wersji](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

W przypadku wybrania **automatyczne &ndash; Użyj wersja docelowego frameworka**, wersja minimalna Android będzie taka sama jak ustawienie platformy docelowej.

-----


<a name="target" />

### <a name="target-android-version"></a>Docelowa wersja systemu Android

*Docelową wersję systemu Android* (znanej także jako `targetSdkVersion`) jest interfejs API, poziom urządzenia z systemem Android, gdy aplikacja oczekuje do uruchomienia. Android używa tego ustawienia, aby określić, czy włączyć wszystkie zachowania zgodności &ndash; gwarantuje to, że aplikacji w dalszym ciągu działać zgodnie z oczekiwaniami. Android używa docelowej Android ustawienie wersji aplikacji do ustalenia, jakie zmiany zachowania może odnosić się do aplikacji bez przerywania go (jest to jak Android zapewnia zgodność z nowszymi wersjami).

Platforma docelowa i docelowy z systemem Android wersji, a jednocześnie ma bardzo podobne nazwy nie są to samo. Ustawienie platformy docelowej komunikuje się docelowego interfejsu API informacji na temat poziomu Xamarin.Android do użycia w lokalizacji *czas kompilacji*, podczas gdy wersji docelowej Android komunikuje się docelowego interfejsu API informacji na temat poziomu systemu Android do użycia w lokalizacji  *czas wykonywania* (Jeśli aplikacja jest zainstalowana i uruchomiona na urządzenie).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby uzyskać dostęp do tego ustawienia w programie Visual Studio, otwórz właściwości projektu w **Eksploratora rozwiązań** i wybierz **manifestu systemu Android** strony. W menu rozwijane w obszarze **wersji docelowej Android** wersji docelowej Android można wybrać dla aplikacji:

[![Docelowa wersja systemu Android ustawioną skompilować przy użyciu zestawu SDK w wersji](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

Firma Microsoft zaleca jawnie ustawiona wersja docelowego dla systemu Android do najnowszej wersji systemu android, którego używasz do testowania aplikacji. W idealnym przypadku powinien być ustawiony na najnowszej wersji zestawu SDK systemu Android &ndash; dzięki temu można korzystać z nowych interfejsów API przed pracy nad zmiany zachowania. Dla większości deweloperów firma Microsoft *nie* zaleca się ustawienie wersji docelowej Android **Użyj skompilować przy użyciu zestawu SDK w wersji**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby uzyskać dostęp do ustawienia platformy docelowej w programie Visual Studio for Mac, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **opcje**; ten otwiera **opcje projektu** okna dialogowego. Przejdź do **kompilacji > aplikacji systemu Android**.
Za pomocą listy rozwijanej z prawej strony **wersji docelowej Android**, można ustawić Android docelowej wersji aplikacji:

[![Docelowa wersja systemu Android ustawiono automatyczne - Użyj docelowej framework w wersji](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

Firma Microsoft zaleca jawnie ustawiona wersja docelowego dla systemu Android do najnowszej wersji systemu android, którego używasz do testowania aplikacji. W idealnym przypadku powinien być ustawiony na najnowszej dostępnej wersji zestawu SDK systemu Android &ndash; dzięki temu można korzystać z nowych interfejsów API przed pracy nad zmiany zachowania. Dla większości deweloperów, nie zaleca się ustawienie wersji docelowej Android **automatyczny — Użyj docelowej framework w wersji**.

-----

Ogólnie rzecz biorąc docelowa wersja systemu Android powinny być ograniczone przez minimalna wersja systemu Android i platformy docelowej. To znaczy:

**Minimalna wersja systemu Android < = docelową wersję systemu Android < = platformy docelowej**

Aby uzyskać więcej informacji na temat poziomów zestawu SDK, zobacz deweloperów systemu Android [sdk używa](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) dokumentacji.


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Środowisko uruchomieniowe kontroli wersji dla systemu Android

Każda nowa wersja systemu android jest zwolnione, framework interfejsu API jest zmodyfikowany, aby zapewnić nowych lub zastąpienie funkcji. Z pewnymi wyjątkami funkcji API z wcześniejszych wersji systemu Android jest przenoszone do nowszych wersji systemu Android bez modyfikacji. W związku z tym jeśli aplikacja jest uruchamiana na określonym poziomie interfejsu API systemu Android, zazwyczaj będzie można uruchamiać na nowsze poziom interfejsu API systemu Android bez modyfikacji. Ale co zrobić, jeśli chcesz także uruchamianie aplikacji we wcześniejszych wersjach systemu android?

W przypadku wybrania Minimum Android wersji *niższe* niż ustawienia platformy docelowej niektórych interfejsów API nie mogą być dostępne dla aplikacji w czasie wykonywania. Jednak aplikacji można nadal uruchamiać na urządzeniu z systemem wcześniej, ale z ograniczoną funkcjonalnością. Dla każdego interfejsu API, który nie jest dostępny na platformach systemu Android odpowiadający Minimum Android ustawienie wersji, kodu musi jawnie Sprawdź wartość `Android.OS.Build.VERSION.SdkInt` właściwości, aby określić poziom interfejsu API platformy aplikacja jest uruchomiona na. Jeśli poziom interfejsu API jest *niższe* niż wersja Minimum Android, która obsługuje interfejs API, który chcesz wybrać, a następnie kodu musi znaleźć sposób, aby działać poprawnie bez wprowadzania to wywołanie interfejsu API.

Na przykład, załóżmy, że firma Microsoft ma być używany [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) metody kategoryzację powiadomienie, gdy uruchomiony na **Android 5.0 interfejs typu lizak** (i nowszych), ale chcemy nadal aplikacji do Uruchamianie we wcześniejszych wersjach systemu android, takich jak **Android 4.1 galaretki ziarna** (gdzie `SetCategory` nie jest dostępna). Odwołanie do tabeli wersji dla systemu Android na początku przewodnika, widzimy, który kod wersji kompilacji **Android 5.0 interfejs typu lizak** jest `Android.OS.BuildVersionCodes.Lollipop`. Do obsługi starszych wersji systemu Android, gdzie `SetCategory` jest niedostępne, naszego kodu, można wykrywać poziom interfejsu API w czasie wykonywania i warunkowo wywołania `SetCategory` tylko gdy, poziom interfejsu API jest większa niż lub równa interfejs typu lizak kod wersji kompilacji:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

W tym przykładzie platformy docelowej naszej aplikacji ma ustawioną wartość **Android 5.0 (21 poziom interfejsu API)** i ustawienie jej wersji Minimum Android **Android 4.1 (16 poziom interfejsu API)**. Ponieważ `SetCategory` jest dostępna na poziomie interfejsu API `Android.OS.BuildVersionCodes.Lollipop` i później, ten przykładowy kod wywoła `SetCategory` tylko gdy jest rzeczywiście dostępne &ndash; będzie *nie* podjęto próbę wywołania `SetCategory` podczas interfejsu API poziom jest 16, 17, 18, 19 lub 20. Funkcjonalność jest ograniczona w tych starszych wersjach systemu Android, tylko w jakim powiadomienia nie są posortowane prawidłowo (ponieważ nie są one podzielone według typu), ale powiadomienia są nadal publikowane użytkownika. Nasze aplikacja nadal działa, ale jego działanie nieco będzie mniejsza.

Ogólnie rzecz biorąc sprawdzenie wersji kompilacji pomaga w kodzie zdecydować, w czasie wykonywania między robi nowy sposób i stary sposób. Na przykład:

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

Nie istnieje żadna reguła szybki i prosty, który objaśnia, jak ograniczyć lub zmodyfikować funkcje aplikacji uruchomionej w starszych wersjach systemu Android, których brakuje jednego lub więcej interfejsów API. W niektórych przypadkach (takim jak `SetCategory` w powyższym przykładzie), wystarczy po prostu pominąć wywołania interfejsu API, gdy nie jest dostępna. Jednak w innych przypadkach może być konieczne wdrożenie funkcji alternatywny program `Android.OS.Build.VERSION.SdkInt` wykrycia być mniejsza niż interfejsu API w poziomie, czy Twoja aplikacja powinna stanowić jego optymalne środowisko.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>Poziomy interfejsu API i biblioteki

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Podczas tworzenia projektu platformy Xamarin.Android biblioteki (np. biblioteki klas lub biblioteka powiązań), można skonfigurować tylko ustawienia platformy docelowej &ndash; Minimum Android wersji i docelowy Android ustawienia wersji nie są dostępne. To, ponieważ nie istnieje żadne **manifestu systemu Android** strony:

[![Jest dostępna tylko kompilowania przy użyciu opcji wersji dla systemu Android](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Podczas tworzenia projektu biblioteki platformy Xamarin.Android jest nie **aplikacji systemu Android** strony, którym można skonfigurować Minimum Android wersja oraz wersja docelowego dla systemu Android &ndash; Minimum Android wersji i obiekt docelowy Ustawienia dla systemu android w wersji nie są dostępne.
To, ponieważ nie istnieje żadne **kompilacji > aplikacji systemu Android** strony):

[![Strona ogólne bez opcji wersja minimalna i docelowa kompilacji](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

Wersja Minimum Android i ustawienia wersji docelowej Android są niedostępne, ponieważ wynikowa biblioteka nie jest to aplikacja autonomiczna &ndash; biblioteki można uruchomić w dowolnej wersji systemu Android, w zależności od aplikacji, która jest dostarczana z. Można określić, jak biblioteki ma być *skompilowany*, ale nie można przewidzieć platformy poziom interfejsu API biblioteki będzie uruchamiany na. Pamiętając o tym następujące najlepsze rozwiązania należy przestrzegać podczas używania lub tworzenia biblioteki:

-   **Podczas używania biblioteki systemu Android** &ndash; Jeśli zużywają biblioteki systemu Android w aplikacji, należy ustawić platformę docelową aplikacji ustawienie do interfejsu API poziomu *co najmniej tak dużych, jak* element docelowy Ustawienie Framework biblioteki.

-   **Podczas tworzenia biblioteki systemu Android** &ndash; w przypadku tworzenia biblioteki systemu Android do użytku przez inne aplikacje, należy ustawić minimalny poziom interfejsu API go wymaga, aby skompilować jej ustawienie platformy docelowej.

Zaleca się następujące najlepsze rozwiązania, aby zapobiec sytuacji, w którym biblioteki próby wywołania interfejsu API, który nie jest dostępny w czasie wykonywania (co może spowodować awarię aplikacji). Jeśli jesteś deweloperem biblioteki powinien Dokładamy wszelkich starań ograniczyć użycie wywołań interfejsu API dla małych i ustalonym podzbioru łącznie powierzchni interfejsu API. Pomaga zapewnić, że biblioteki może być używany przez szersze wersje zakresu systemu Android.


## <a name="summary"></a>Podsumowanie

W tym przewodniku wyjaśniono, jak poziomy interfejsu API systemu Android są używane do zarządzania zgodności aplikacji w różnych wersjach systemu android. Pod warunkiem uzyskać szczegółowe instrukcje dotyczące konfigurowania platformy Xamarin.Android *platformy docelowej*, *Minimum Android w wersji*, i *wersji docelowej Android* ustawień projektu. Zapewniała instrukcje dotyczące używania Android SDK Manager można zainstalować pakietów SDK dołączany przykłady tego, jak napisać kod radzenia sobie z różnymi poziomami interfejsu API w czasie wykonywania i wyjaśniono sposób zarządzania poziomy interfejsu API, podczas tworzenia lub korzystanie z biblioteki systemu Android. On również podane kompleksowe odpowiadającą poziomy interfejsu API platformy Xamarin.Android kompilacji wersji kody, nazwy wersji dla systemu Android (na przykład Kitkat) i numery wersji dla systemu Android (takich jak Android 4.4).


## <a name="related-links"></a>Linki pokrewne

- [Instalacja zestawu SDK systemu Android](~/android/get-started/installation/android-sdk.md)
- [Zmienia narzędzi interfejsu wiersza polecenia zestawu SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Pobieranie Twojej compileSdkVersion minSdkVersion i targetSdkVersion](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [Jaki jest poziom interfejsu API?](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [Codenames, znaczników i numery kompilacji](https://source.android.com/source/build-numbers)
