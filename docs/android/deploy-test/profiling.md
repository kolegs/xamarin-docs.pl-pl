---
title: Profilowanie aplikacji systemu Android
description: W tym przewodniku objaśniono sposób użycia profiler narzędzi do sprawdzania wydajności i użycie pamięci przez aplikację systemu Android.
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C823FEE-A6F6-4C31-9EB6-E51407A2FD8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/03/2018
ms.openlocfilehash: e62ac290423db1c18e7e50d55b2b3550f99d1533
ms.sourcegitcommit: a4c2a63ba76b839cda99e4474e7ab46fe307cd39
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34549278"
---
# <a name="profiling-android-apps"></a>Profilowanie aplikacji systemu Android

Przed wdrożeniem aplikacji do sklepu z aplikacjami, jest ważne zidentyfikować i rozwiązać wszelkie wąskich gardeł wydajności, problemów z wykorzystaniem nadmiernego pamięci lub nieefektywne użycie zasobów sieciowych. Dwa profilera narzędzia są dostępne do obsługi tego celu:

-  Xamarin Profiler 
-  Profiler systemu android w programie Android Studio

W tym przewodniku wprowadzono Profiler platformy Xamarin i zawiera szczegółowe informacje o wprowadzenie przy użyciu profilera z systemem Android.

 
## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin Profiler to aplikacja autonomiczna, która jest zintegrowany z programu Visual Studio i Visual Studio dla komputerów Mac do profilowania aplikacji platformy Xamarin w środowisku IDE. Aby uzyskać więcej informacji o używaniu profilera Xamarin, zobacz [profilera Xamarin](~/tools/profiler/index.md).

> [!NOTE]
> Musi być [Visual Studio Enterprise](https://www.visualstudio.com/vs/compare/) subskrybenta do odblokowania funkcji Profiler platformy Xamarin w albo program Visual Studio Enterprise w systemie Windows lub programu Visual Studio dla komputerów Mac.
 
## <a name="android-studio-profiler"></a>Android Studio profilera

Android Studio 3.0 i nowszych zawiera narzędzie Android profilera. Android profilera służy do pomiaru wydajności aplikacji platformy Xamarin Android skompilowanych za pomocą programu Visual Studio &ndash; bez konieczności licencji programu Visual Studio Enterprise. W przeciwieństwie do profilera Xamarin Android Profiler nie jest zintegrowany z programem Visual Studio i może służyć tylko do profilu pakietu aplikacji systemu Android (APK), który został utworzony wcześniej i importowane do programu Android.

### <a name="launching-a-xamarin-android-app-in-android-profiler"></a>Uruchamianie aplikacji platformy Xamarin Android, w systemie Android Profiler

Poniższych krokach opisano sposób uruchamiania aplikacji dla systemu Xamarin Android w narzędziu Android profilera Android Studio. Na przykład zrzutach ekranu poniżej, Xamarin Forms [XamagonXuzzle](https://developer.xamarin.com/samples/mobile/LivePlayer/XamagonXuzzleLP/) aplikacji jest wbudowana i profilowany przy użyciu profilera z systemem Android:

1.  Opcje kompilacji projektu systemu Android, wyłącz **Użyj udostępnionych w czasie wykonywania**. Dzięki temu, że pakiet aplikacji systemu Android (APK) jest zbudowany bez zależności na udostępnionych runtime Mono czasie opracowywania.

    ![Wyłączanie Użyj udostępnionych środowiska wykonawczego](profiling-images/vswin/01-turn-off-shared-runtime.png)

2.  Tworzenie aplikacji dla **debugowania** i wdrożyć ją na fizyczne urządzenia lub emulatora. Powoduje to, że zalogowany **debugowania** wersji APK, który ma zostać utworzony.
    Aby uzyskać **XamagonXuzzle** przykład nosi nazwę wynikowy APK **com.companyname.XamagonXuzzle Signed.apk**.

3.  Otwórz folder projektu i przejdź do **bin/Debug**. W tym folderze, zlokalizuj **Signed.apk** wersji aplikacji i skopiuj go do łatwo dostępne miejscu (na przykład komputer stacjonarny). Na poniższym zrzucie ekranu plik APK **com.companyname.XamagonXuzzle Signed.apk** jest znajduje się i skopiowane do komputera:

    [![Lokalizacja debugowania podpisany plik APK](profiling-images/vswin/02-locating-the-debug-apk-sml.png)](profiling-images/vswin/02-locating-the-debug-apk.png#lightbox)

4.  Uruchom program Android Studio i wybierz **profilu lub debugowania APK**:

    ![Uruchamianie profilera z ekranu startowego Android Studio](profiling-images/vswin/03-android-studio.png)

5.  W **wybierz plik APK** okno dialogowe, przejdź do APK, który został utworzony i skopiowany wcześniej. Wybierz plik APK, a następnie kliknij przycisk **OK**: 
    
    ![Wybierając plik APK w oknie dialogowym Wybierz plik APK](profiling-images/vswin/04-select-apk-dialog.png)

6.  Android Studio załaduje APK i dissassembles **classes.dex**:

    ![Konfigurowanie plik APK](profiling-images/vswin/05-setting-up-the-apk.png)

7.  Po załadowaniu plik APK Android Studio przedstawia następujący ekran projektu plik APK. Kliknij prawym przyciskiem myszy nazwę aplikacji na widok drzewa po lewej stronie i wybierz **Otwórz ustawienia modułu**:

    [![Lokalizacja elementu menu Otwórz ustawienia modułu](profiling-images/vswin/06-open-module-settings-sml.png)](profiling-images/vswin/06-open-module-settings.png#lightbox)

8.  Przejdź do **ustawienia projektu > modułów**, wybierz pozycję **-podpisany** węzła aplikacji, następnie kliknij przycisk  **&lt;SDK nie&gt;**:

    [![Nawigowanie do ustawień zestawu SDK](profiling-images/vswin/07-project-settings-modules-sml.png)](profiling-images/vswin/07-project-settings-modules.png#lightbox)

9.  W **SDK modułu** menu rozwijanego wybierz poziom zestawu SDK systemu Android, który został użyty do budowania aplikacji (w tym przykładzie użyto poziom interfejsu API 26 do tworzenia **XamagonXuzzle**):

    [![Ustawienie poziomu SDK projektu](profiling-images/vswin/08-project-sdk-level-sml.png)](profiling-images/vswin/08-project-sdk-level.png#lightbox)

    Kliknij przycisk **Zastosuj** i **OK** można zapisać tego ustawienia.

10. Uruchamianie programu profilującego z ikony paska narzędzi:

    [![Położenie ikony paska narzędzi profilera](profiling-images/vswin/09-launch-profiler-sml.png)](profiling-images/vswin/09-launch-profiler.png#lightbox)

11. Wybierz cel wdrożenia dla uruchomiona/profilowania aplikacji i kliknij pozycję **OK**. Cel wdrożenia może być urządzenie fizyczne lub uruchamianie w emulatorze urządzenia wirtualnego. Urządzenie węzła 5 X jest używane w tym przykładzie:

    ![Wybieranie cel wdrożenia](profiling-images/vswin/10-select-deployment-target.png)

12. Po uruchomieniu profilera potrwa kilka sekund dla niego do łączenia się urządzeń wdrożenia i procesu aplikacji. Gdy jest on instalowany plik APK, profilera Android będzie zgłaszać **żadnych podłączonych urządzeń** i **żadne procesy możliwością debugowania**.

    [![Profiler instaluje plik APK](profiling-images/vswin/11-no-connected-devices-sml.png)](profiling-images/vswin/11-no-connected-devices.png#lightbox)

13. Po kilku sekundach profilera dla systemu Android zostanie APK instalacji i uruchom plik APK raportowania nazwę urządzenia i nazwę PROFILOWANEGO procesu aplikacji (w tym przykładzie **LGE węzła 5 X** i  **com.companyname.XamagonXuzzle**odpowiednio):

    [![Okno profilera po rozruchu](profiling-images/vswin/12-profiler-starts-sml.png)](profiling-images/vswin/12-profiler-starts.png#lightbox)

14. Po zidentyfikowaniu urządzenia i możliwością debugowania procesu systemu Android profilera rozpoczyna profilowania aplikacji:

    [![Wyświetla profilera dla uruchomionej aplikacji](profiling-images/vswin/13-profiler-running-sml.png)](profiling-images/vswin/13-profiler-running.png#lightbox)

15. Jeśli wybierzesz przycisk **losowo** znajdującego się na **XamagonXuzzle** (co spowoduje, że jego przesunięcia i ustaw losowy Kafelki), zobaczysz użycie procesora CPU, zwiększenie interwale losowe aplikacji:

    [![Użycie procesora CPU jest wybrany przycisk losowo](profiling-images/vswin/14-tap-randomize-sml.png)](profiling-images/vswin/14-tap-randomize.png#lightbox)


### <a name="using-the-android-profiler"></a>Przy użyciu profilera z systemem Android

Szczegółowe informacje dotyczące korzystania z systemem Android Profiler jest objęta [dokumentacji Android Studio](https://developer.android.com/studio/profile/android-profiler.html).
Poniższe tematy będą przydatne dla deweloperów platformy Xamarin Android:

-   [Profiler Procesora](https://developer.android.com/studio/profile/cpu-profiler.html) &ndash; wyjaśniono, jak sprawdzić Procesora użycia i wątku działanie aplikacji w czasie rzeczywistym.

-   [Profiler pamięci](https://developer.android.com/studio/profile/memory-profiler.html) &ndash; w czasie rzeczywistym wykres użycia pamięci aplikacji i zawiera przycisk alokacji pamięci rekordów do analizy.

-   [Sieci profilera](https://developer.android.com/studio/profile/network-profiler.html) &ndash; Wyświetla działanie sieci w czasie rzeczywistym danych wysłanych i odebranych przez aplikację.
