---
title: "Porady dotyczące rozwiązywania problemów"
ms.topic: article
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 015fff63c612c3acf29681b90c1e945c5e460034
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="troubleshooting-tips"></a>Porady dotyczące rozwiązywania problemów


## <a name="getting-diagnostic-information"></a>Otrzymanie informacji diagnostycznych

Xamarin.Android ma kilka miejsca do przeszukania podczas śledzenia różnych usterki w dół.
Należą do nich następujące elementy:

1.  Dane wyjściowe diagnostyki programu MSBuild.
2.  Dzienniki wdrożenia urządzenia.
3.  Dane wyjściowe dzienników debugowania dla systemu android.


<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>Dane wyjściowe diagnostyki programu MSBuild

MSBuild diagnostycznych może zawierać dodatkowe informacje dotyczące tworzenia pakietu i może zawierać niektóre informacje na temat wdrażania pakietu.

Aby włączyć dane wyjściowe diagnostyki programu MSBuild w programie Visual Studio:

1.  Kliknij przycisk **Narzędzia > Opcje...**
2.  W widoku drzewa po lewej stronie wybierz **projekty i rozwiązania > Tworzenie i uruchamianie**
3.  W okienku po prawej stronie Ustaw listy rozwijanej poziom szczegółowości danych wyjściowych kompilacji MSBuild diagnostyki
4.  Kliknij przycisk **OK**
5.  Czyszczenie i skompiluj ponownie pakiet.
6.  Dane wyjściowe diagnostyki jest widoczna w panelu wyjścia.


Aby włączyć dane wyjściowe diagnostyki programu MSBuild w programie Visual Studio dla systemu Mac/OS X:

1.  Kliknij przycisk **programu Visual Studio for Mac > Preferencje...**
2.  W widoku drzewa po lewej stronie wybierz **projekty > kompilacji**
3.  W okienku po prawej stronie Ustaw poziom szczegółowości dziennika listy rozwijanej diagnostyki
4.  Kliknij przycisk **OK**
5.  Uruchom ponownie program Visual Studio dla komputerów Mac
6.  Czyszczenie i skompiluj ponownie pakiet.
7.  Wyjście diagnostyczne jest widoczny w konsoli błędy (**widoku > konsole > błędy** ), klikając przycisk tworzenia danych wyjściowych.




## <a name="device-deployment-logs"></a>Dzienniki wdrożenia urządzenia

Aby włączyć rejestrowanie wdrożenia urządzenia w programie Visual Studio:

1.  **Narzędzia > Opcje...**>
2.  W widoku drzewa po lewej stronie wybierz **Xamarin > Ustawienia systemu Android**
3.  W okienku po prawej stronie umożliwiają [X] **rejestrowania debugowania rozszerzenia (zapisuje monodroid.log pulpitu)** pole wyboru.
4.  Komunikaty dziennika są zapisywane do pliku monodroid.log na pulpicie.


Visual Studio for Mac zawsze zapisuje dzienniki wdrożenia urządzenia. Znajdowanie ich jest nieco trudniejsze; *AndroidUtils* plik dziennika jest tworzony dla każdego dnia + czas wdrożenia występuje na przykład: **AndroidTools-2012-10-24_12-35-45.log**.

-  W systemie Windows, pliki dziennika są zapisywane w `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`.
-  Na OS X, pliki dziennika są zapisywane w `$HOME/Library/Logs/XamarinStudio-{VERSION}`.




## <a name="android-debug-log-output"></a>Dane wyjściowe dzienników debugowania dla systemu android

System android będzie zapisywać wiele komunikatów do [dzienników debugowania systemu Android](~/android/deploy-test/debugging/android-debug-log.md).
Xamarin.Android używa właściwości systemu Android do sterowania Generowanie przejrzeć dodatkowe komunikaty w dzienniku systemu Android debugowania. Można ustawić właściwości systemu android za pośrednictwem *setprop* w ciągu [mostka debugowania systemu Android (adb)](http://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

Właściwości systemu są odczytywane podczas uruchamiania procesu, a w związku z tym musi być skonfigurowany, aby aplikacja jest uruchamiana lub aplikacji, należy ponownie uruchomić po zmianie właściwości systemu.



### <a name="xamarinandroid-system-properties"></a>Xamarin.Android System Properties

Xamarin.Android obsługuje następujące właściwości systemu:

-   *Debug.mono.Debug*: Jeśli niepustym ciągiem, co jest równoważne `*mono-debug*`.

-   *Debug.mono.ENV*: oddzielone kreski pionowej ("*|*") listę zmiennych środowiskowych, aby wyeksportować podczas uruchamiania aplikacji *przed* mono został zainicjowany. Dzięki temu ustawienia zmiennych środowiskowych rejestrowania mono tego formantu.

    - *Uwaga*: ponieważ jest wartość "*|*"-rozdzielone, wartość musi zawierać dodatkowy poziom zamykający, jako \` *powłoki adb* \` polecenia spowoduje usunięcie zestaw znaków cudzysłowu.

    - *Uwaga*: wartości właściwości systemu Android nie mogą przekraczać 92 znaków.

    - Przykład:

            adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"

-   *Debug.mono.log*: rozdzielanych przecinkami ("*,*") lista składników, które mają być drukowane przejrzeć dodatkowe komunikaty w dzienniku systemu Android debugowania. Domyślnie ma wartość nothing. Następujące składniki:

    -   *wszystkie*: Drukuj komunikaty
    -   *wykaz globalny*: GC drukowania komunikatów.
    -   *gref*: Drukuj komunikaty alokacji i dezalokacji odwołania (słabe, globalne).
    -   *lref*: Drukuj komunikaty alokacji i dezalokacji lokalnego odwołania.

    *Uwaga*: są to *bardzo* pełne. Nie należy włączać, chyba że naprawdę konieczne.

-   *Debug.mono.trace*: umożliwia ustawienie [mono — śledzenia](http://docs.go-mono.com/?link=man%3amono(1)) `=PROPERTY_VALUE` ustawienie.



## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android nie można rozpoznać System.ValueTuple

Ten błąd występuje z powodu niezgodności z programem Visual Studio.

- **Visual Studio 2017 Update 1** (15,1 lub starsza wersja) jest zgodna tylko z **System.ValueTuple NuGet 4.3.0** (lub starszy).

- **Visual Studio 2017 Update 2** (wersja 15.2 lub nowsza) jest zgodna tylko z **System.ValueTuple NuGet 4.3.1** (lub nowsza).

Wybierz prawidłowe System.ValueTuple NuGet odpowiadający z instalacją programu Visual Studio 2017 r.


## <a name="gc-messages"></a>Komunikaty GC

Komunikaty składników GC można wyświetlić przez ustawienie właściwości systemu debug.mono.log na wartość, która zawiera gc.

GC komunikaty są generowane, gdy GC wykonuje i udostępnia informacje, o ile pracy GC czy:

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

Informacje dodatkowe GC, takie jak informacji mogą być generowane przez ustawienie `MONO_LOG_LEVEL` zmienną środowiskową `debug`:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

Spowoduje to (wiele) dodatkowe Mono wiadomości, łącznie z tych trzech konsekwencji:

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

W `GC_BRIDGE` wiadomości, `num-objects` jest liczba obiektów Mostek rozważa tego przebiegu i `num_hash_entries` jest liczba obiektów przetworzonych w ciągu to wywołanie kodu mostek.

W `GC_MINOR` i `GC_MAJOR` wiadomości, `total` jest ilość czasu, podczas gdy jest wstrzymana świecie (wątków nie są wykonywane), podczas `bridge` ilość czasu potrzebnego do mostka przetwarzania kodu (która maszyny Wirtualnej Java). Świat jest *nie* wstrzymana podczas przetwarzania mostek.

 *Ogólnie rzecz biorąc*, im większa wartość `num_hash_entries`, więcej czasu `bridge` potrwa kolekcje i większy `total` będzie czas zbierania.



## <a name="global-reference-messages"></a>Odwołanie do globalnych wiadomości

Aby włączyć loggig odwołanie do globalnych (GREF) rejestrowania, *debug.mono.log* musi zawierać właściwość systemu *gref*, np.:

```shell
adb shell setprop debug.mono.log gref
```

Platformy Xamarin.Android używa Android globalne odwołań do zapewnienia mapowania między wystąpieniami Java i skojarzonych wystąpień zarządzanych, jak podczas wywoływania metody Java, które wystąpienie Java musi zostać zapewniony języka Java.

Niestety Android emulatorów Zezwalaj 2000 globalne odwołania do istnieje naraz. Sprzęt ma wiele wyższy limit 52000 odwołań do globalnych. Dolna granica może być powodować problemy podczas korzystania z aplikacji w emulatorze, wiedząc, więc *gdzie* dostarczonej wystąpienia z może być bardzo przydatne.

 *Uwaga*: liczba odwołań globalny jest wewnętrzną platformy Xamarin.Android i nie ma i nie może obejmują globalne odwołań przez innych natywnych bibliotek załadowany do procesu. Użyj liczby odwołania globalnej jako szacowania.

```shell
I/monodroid-gref(12405): +g+ grefc 108 gwrefc 0 obj-handle 0x40517468/L -> new-handle 0x40517468/L from    at Java.Lang.Object.RegisterInstance(IJavaObject instance, IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object.SetHandle(IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object..ctor(IntPtr handle, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler, Boolean removable)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler)
I/monodroid-gref(12405):    at Android.App.Activity.RunOnUiThread(System.Action action)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.UseLotsOfMemory(Android.Widget.TextView textview)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.<OnCreate>m__3(System.Object o)
I/monodroid-gref(12405): handle 0x40517468; key_handle 0x40517468: Java Type: `mono/java/lang/RunnableImplementor`; MCW type: `Java.Lang.Thread+RunnableImplementor`
I/monodroid-gref(12405): Disposing handle 0x40517468
I/monodroid-gref(12405): -g- grefc 107 gwrefc 0 handle 0x40517468/L from    at Java.Lang.Object.Dispose(System.Object instance, IntPtr handle, IntPtr key_handle, JObjectRefType handle_type)
I/monodroid-gref(12405):    at Java.Lang.Object.Dispose()
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor.Run()
I/monodroid-gref(12405):    at Java.Lang.IRunnableInvoker.n_Run(IntPtr jnienv, IntPtr native__this)
I/monodroid-gref(12405):    at System.Object.c200fe6f-ac33-441b-a3a0-47659e3f6750(IntPtr , IntPtr )
I/monodroid-gref(27679): +w+ grefc 1916 gwrefc 296 obj-handle 0x406b2b98/G -> new-handle 0xde68f4bf/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1915 gwrefc 294 handle 0xde691aaf/W from take_global_ref_jni
```

Istnieją cztery komunikaty konsekwencji:

-  Tworzenie odwołania globalnej: są to wiersze rozpoczynające się *+ g +* i udostępni śledzenia stosu dla tworzenia ścieżki kodu.
-  Zniszczenie odwołania globalnej: są to wiersze rozpoczynające się *- g-* i może udostępniać śladu stosu dla ścieżki kodu usuwania odwołania do globalnego. Jeśli wykaz Globalny jest usuwanie gref, zapewnia się śladów stosu.
-  Niska globalne odwołania tworzenia: są to wiersze rozpoczynające się *+ w +* .
-  Niska globalne odwołania zniszczenia: są to wiersze rozpoczynające się *-w —* .


W wszystkie komunikaty *grefc* wartość jest liczba odwołań globalne, które platformy Xamarin.Android został utworzony, gdy *grefwc* wartość jest liczbą słabe odwołania globalne, które utworzył platformy Xamarin.Android. *Obsługi* lub *dojście obj* wartość jest wartością dojście JNI i znaków po "  */* " jest typ wartości dojścia: */L* lokalnego odwołania */G* globalne odwołań i */W* słabego odwołania globalnego.

Jako część procesu GC, odwołania do globalnego (+ g +) są konwertowane na słabe odwołania globalnego (przyczyną + w + i - g-), jest kopać GC Java serwerowe i sprawdzana słabe odwołanie globalne aby zobaczyć, czy został zebrany. Jeśli jest nadal aktywna, nowe gref jest tworzony wokół słabe odwołanie (+ g +, -w —), w przeciwnym razie zostanie zniszczony słabe odwołanie (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Wystąpienie Java jest tworzony i opakowane przez MCW

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>Wykaz Globalny jest wykonywane...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>Obiekt jest nadal aktywna jako uchwyt! = null
## <a name="wref-turned-back-into-a-gref"></a>Wref z powrotem do gref

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>Obiekt jest martwy jako uchwyt == wartości null
## <a name="wref-is-freed-no-new-gref-created"></a>Wref jest gref zwolnionych, żadna nowa utworzony

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

Istnieje jeden Marszczenie "interesujące" w tym miejscu: dla elementów docelowych z systemem Android starszą niż 4.0, wartość gref jest równa adresu obiektu Java Android runtime pamięci. (Oznacza to, wykaz Globalny jest — przenoszenie, zakładając, moduł zbierający, a jest jego przekazywanie bezpośredniego odniesienia do tych obiektów.) W związku z tym po + g +, + w +, - g-, + g +, - w sekwencji, gref wynikowy będzie mieć taką samą wartość jak oryginalna wartość gref. Dzięki temu można bardzo prosta grepping za pomocą dzienników.

Android 4.0 jednak ma przenoszenie moduł zbierający i już nie przekazuje bezpośredniego odniesienia do środowiska wykonawczego systemu Android obiektów maszyny Wirtualnej. W rezultacie po + g +, + w +, - g-, + g +, - w sekwencji, wartość gref *będzie różna*. Jeśli obiekt przeżyje wielu wykazów globalnych, przechodzą przez kilka wartości gref, dzięki czemu trudniejsze do określenia, gdzie wystąpienia faktycznie został przydzielony.

### <a name="querying-programmatically"></a>Wykonywanie zapytania programowo

Można zbadać zarówno GREF i WREF liczby badając `JniRuntime` obiektu.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` -Globalne liczba odwołań

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` -Słabe liczba odwołań



## <a name="offline-activation"></a>Aktywacji w trybie offline

Jeśli nie można uaktywnić Xamarin.Android w systemie Windows lub nie można zainstalować pełną wersję platformy Xamarin.Android w systemie Mac OS X, zapoznaj się z artykułem [aktywacji w trybie Offline](~/android/get-started/installation/index.md) strony.



## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>Nie można uaktualnić Indie/firmy z konta wersji próbnej

Jeśli ostatnio zakupu platformy Xamarin.Android i wcześniej uruchomione z wersji próbnej platformy Xamarin.Android, może być konieczne wykonaj następujące kroki, aby uzyskać tę zmianę licencji pobrana przez Visual Studio dla komputerów Mac lub Visual Studio.

-  Zamknij program Visual Studio for Mac/Visual Studio
-  Usuń wszystkie pliki z ~/Library/MonoAndroid dla komputerów Mac lub %PROGRAMDATA%\Mono Android\License\ w systemie Windows
-  Otwórz ponownie program Visual Studio for Mac/Visual Studio i utworzyć projekt platformy Xamarin.Android


To powinno programy do pracy. Jeśli nadal występują problemy, możesz spróbować [aktywacji w trybie Offline](~/android/get-started/installation/index.md) do ukończenia aktywacji stacji roboczej.



## <a name="receiving-activation-incomplete-error-message"></a>Odbieranie "niekompletne komunikat aktywacji

Ten problem może wystąpić w przypadku używania platformy Xamarin.Android dla programu Visual Studio. Aby rozwiązać ten problem, należy wysłać dzienniki z następującej lokalizacji w celu  *contact@xamarin.com* .

-  Lokalizacja dziennika: **LocalAppData %\\Xamarin\\dzienniki**




## <a name="receiving-error-retrieving-update-information-error-message"></a>Odbieranie komunikatu o błędzie "Błąd podczas pobierania informacji o aktualizacji"

Od czasu do czasu aktualizacji zakończy się niepowodzeniem z tego następujący błąd, który często wystąpi podczas sprawdzania dostępności aktualizacji:

Większość czasu, ten błąd można rozwiązać logując poza konta Xamarin i następnie ponownie rejestrowania.

W tym celu Znajdź platformy wyboru poniżej i wykonaj kroki:

**Dla komputerów Mac:**
1. Otwieranie programu Visual Studio dla komputerów Mac
2. Wybierz program Visual Studio for Mac > konto...
3. Kliknij przycisk wylogowania
4. Kliknij przycisk Zaloguj
5. Wprowadź swoje poświadczenia
6. Sprawdź dostępność aktualizacji

**Na komputerze za pomocą Visual Studo:**
1. Otwórz program Visual Studio
2. Wybierz Narzędzia > Konto Xamarin
3. Kliknij przycisk wylogowania
4. Kliknij przycisk Zaloguj
5. Wprowadź swoje poświadczenia
6. Sprawdź dostępność aktualizacji

Jeśli ten komunikat o błędzie będzie nadal występować, skontaktuj e-mail  **contact@xamarin.com** .




## <a name="android-debug-logs"></a>Dzienniki systemu android

[Android dzienniki](~/android/deploy-test/debugging/android-debug-log.md) może zapewnić dodatkowy kontekst dotyczące błędów czasu wykonywania jest wyświetlane.



## <a name="floating-point-performance-is-terrible"></a>Zmiennoprzecinkowe wydajności jest olbrzymich!

Alternatywnie "Moja aplikacja działa 10 x szybsze z kompilacji debugowania niż z kompilacji wydania!"

Xamarin.Android obsługuje wiele urządzeniu ABIs: *armeabi*, *armeabi v7a*, i *x86*. ABIs urządzenia mogą być określone w **właściwości projektu > karta aplikacji > obsługiwane architektury**.

Kompilacje debugowania Użyj pakietu systemu Android, zawiera wszystkie ABIs i w związku z tym użyje najszybszym ABI dla urządzenia.

Kompilacje wydania zostaną uwzględnione jedynie ABIs wybranego na karcie właściwości projektu. Można wybrać więcej niż jeden.

*armeabi* jest domyślnym ABI i ma najszerszych obsługi urządzenia. *Jednak*, armeabi nie obsługuje Wieloprocesorowych urządzeń i sprzętu zmiennoprzecinkowego, amont inne czynności. W rezultacie aplikacji przy użyciu wersji środowiska uruchomieniowego armeabi powiązane pojedynczego rdzenia i będzie używana implementacja zmiennoprzecinkowych. Oba te można współtworzyć znacznie niższej wydajności dla aplikacji.

Jeśli aplikacja wymaga zadowalający wydajności zmiennoprzecinkowe (np. gry), należy włączyć *armeabi v7a* ABI. Może zajść potrzeba obsługują tylko *armeabi v7a* środowiska uruchomieniowego, ale oznacza to, że starszych urządzeń, które obsługują tylko *armeabi* będzie mógł uruchomić aplikację.



## <a name="could-not-locate-android-sdk"></a>Nie można zlokalizować zestawu SDK systemu Android

Brak dostępnych 2 pliki do pobrania z Google dla systemu Android SDK dla systemu Windows.
Jeśli Instalator .exe, zapisze klucze rejestru, które informują Xamarin.Android, w którym został zainstalowany. Wybierz plik zip, Rozpakuj samodzielnie platformy Xamarin.Android nie wiedzieć, gdzie ma zostać wyszukane w zestawie SDK. Można określić Xamarin.Android przypadku zestawu SDK programu Visual Studio, przechodząc do **Narzędzia > Opcje > Xamarin > Ustawienia systemu Android**:

[![Lokalizacja zestawu SDK systemu android w ustawieniach platformy Xamarin Android](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)



## <a name="ide-does-not-display-target-device"></a>IDE nie są wyświetlane urządzenia docelowego

Czasami podejmie próbę wdrożyć aplikację na urządzeniu, ale urządzenie, które chcesz wdrożyć, który nie jest wyświetlany w oknie dialogowym Wybierz urządzenie. Może to być spowodowane mostka debugowania Android decyduje o tym przejść na urlopie.

Aby zdiagnozować problem, Znajdź [adb program](~/android/deploy-test/debugging/android-debug-log.md), następnie uruchom:

```shell
adb devices
```

Jeśli urządzenie nie jest obecny, następnie należy ponownie uruchomić serwer mostka debugowania systemu Android, tak, aby można było znaleźć urządzenia:

```shell
adb kill-server
adb start-server
```

Oprogramowanie do synchronizacji HTC może uniemożliwić **adb start-server** działać prawidłowo. Jeśli **adb start-server** polecenia nie wydrukować port, który jest uruchamiana na, zamknij oprogramowania HTC synchronizacji i spróbuj ponownie uruchomić serwer adb.


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>Nie można uruchomić pliku wykonywalnego określonego zadania "keytool"

Oznacza to, czy ŚCIEŻKA nie zawiera katalog, w którym znajduje się zestaw SDK Java katalogu bin. Sprawdź, czy wykonaniu tych kroków z [instalacji](~/android/get-started/installation/index.md) przewodnik.


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe lub aresgen.exe zostało zakończone przez kod 1

Aby debugować ten problem, przejdź do programu Visual Studio i zmień poziom szczegółowości MSBuild, aby to zrobić, wybierz: **Narzędzia > Opcje > Projekt** i **rozwiązań > kompilacji** i **Uruchom > Szczegółowości danych wyjściowych kompilacji projektu MSBuild** i ustaw tę wartość na **normalny**.

Odbuduj i sprawdź okienko dane wyjściowe programu Visual Studio, które musi zawierać pełną błędu.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>Na urządzeniu, aby wdrożyć pakiet nie ma wystarczającej ilości miejsca do magazynowania

Dzieje się tak, gdy nie uruchamiaj emulatora z poziomu programu Visual Studio. Podczas uruchamiania emulatora poza Visual Studio, musisz przekazać `-partition-size 512` opcje, np.

```shell
emulator -partition-size 512 -avd MonoDroid
```

Upewnij się, tj. Użyj nazwy poprawne symulator, [nazwa użyta podczas konfigurowania symulator](~/android/get-started/installation/windows.md#device).


## <a name="installfailedinvalidapk-when-installing-a-package"></a>Zainstaluj\_\_nieprawidłowy\_APK podczas instalowania pakietu

Nazwy pakietu systemu android *musi* zawierać kropki ("*.*"). Edytuj nazwę pakietu, tak, aby zawierał okres.

-   W programie Visual Studio:
    -   Kliknij prawym przyciskiem myszy Projekt > Właściwości
    -   Kliknij kartę manifestu systemu Android po lewej stronie.
    -   Zaktualizuj pole Nazwa pakietu.
        -   Jeśli zostanie wyświetlony komunikat &ldquo;AndroidManifest.xml nie znaleziono. Kliknij, aby dodać jeden. &rdquo;, kliknij łącze, a następnie zaktualizuj pole Nazwa pakietu.
-   W programie Visual Studio dla komputerów Mac:
    -   Kliknij prawym przyciskiem myszy Projekt > Opcje.
    -   Przejdź do kompilacji / sekcji aplikacji systemu Android.
    -   Zmiany w polu nazwy pakietu zawiera '.'.




## <a name="installfailedmissingsharedlibrary-when-installing-a-package"></a>Zainstaluj\_\_Brak\_SHARED\_biblioteki podczas instalowania pakietu

"Biblioteki udostępnionej" w tym kontekście jest *nie* natywnej biblioteki udostępnionej (*libfoo.so*) pliku; zamiast tego jest bibliotekę, którą należy zainstalować osobno na urządzeniu docelowym, takich jak Google Maps.

Określa pakietu systemu Android, które biblioteki udostępnione są wymagane w przypadku `<uses-library/>` elementu. Jeśli *wymagane* biblioteka nie jest obecny na urządzeniu docelowym (np. `//uses-library/@android:required` jest *true*, co jest ustawieniem domyślnym), instalacja pakietu zakończy się niepowodzeniem z *zainstalować\_ Nie powiodło się\_Brak\_SHARED\_biblioteki*.

Aby ustalić, które biblioteki udostępnione są wymagane, Wyświetl *wygenerowany*
**AndroidManifest.xml** pliku (np. **obj\\debugowania\\systemu android \\AndroidManifest.xml**) i poszukaj `<uses-library/>` elementów. `<uses-library/>` elementy mogą zostać ręcznie dodane do projektu **właściwości\\AndroidManifest.xml** pliku i za pośrednictwem [atrybutu niestandardowego UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/).

Na przykład dodać odwołanie do zestawu *Mono.Android.GoogleMaps.dll* niejawnie doda `<uses-library/>` dla biblioteki udostępnionej map programu Google.



## <a name="installfailedupdateincompatible-when-installing-a-package"></a>Zainstaluj\_\_aktualizacji\_niezgodne podczas instalowania pakietu

Pakiety systemu android ma trzy wymagania:

-   Mogą zawierać "." (zobacz poprzedni wpis)
-   Muszą mieć nazwę pakietu unikatowy ciąg (stąd Konwencji domeny najwyższego poziomu wstecznego w nazwach aplikacji systemu Android, np. com.android.chrome dla programu Chrome)
-   Podczas uaktualniania pakietów, pakiet musi mieć tego samego klucza podpisywania.

W związku z tym Wyobraź sobie następujący scenariusz:

1.  Tworzenie i wdrażanie aplikacji jako aplikacji debugowania
2.  Np. zmienić klucza podpisywania, można użyć jako aplikację wersji (lub ponieważ Ci się nie podoba podane domyślne debugowania klucza podpisywania)
3.  Zainstaluj aplikację bez wcześniejszego usunięcia, np. debugowanie > Uruchom bez debugowania w programie Visual Studio


W takim przypadku instalacja pakietu zakończy się niepowodzeniem w ramach instalacji\_\_aktualizacji\_błąd niezgodne, ponieważ nazwa pakietu nie zmieniły podczas podpisywania klucz został. [Dzienników debugowania systemu Android](~/android/deploy-test/debugging/android-debug-log.md) zawiera również komunikatu podobnego do:

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

Aby naprawić ten błąd, całkowicie Usuń aplikację z urządzenia przed ponowną instalacją.


## <a name="installfaileduidchanged-when-installing-a-package"></a>Zainstaluj\_\_UID\_zmienione podczas instalowania pakietu

Po zainstalowaniu pakietu systemu Android przypisano *identyfikator użytkownika* (UID).
*Czasami*, obecnie nieznanych powodów, instalując za pośrednictwem już zainstalowaną aplikację, instalacja zakończy się niepowodzeniem z `INSTALL_FAILED_UID_CHANGED`:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

Aby obejść ten problem, *pełni odinstalować* pakietu systemu Android, instalowanie aplikacji z graficznego interfejsu użytkownika systemu Android docelowej lub używając `adb`:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**NIE UŻYWAJ** `adb uninstall -k`, ponieważ spowoduje to *zachować* danych aplikacji, w związku z tym zachowaniu powodujące konflikt UID na urządzeniu docelowym.



## <a name="release-apps-fail-to-launch-on-device"></a>Wersja aplikacji nie można uruchomić na urządzeniu

Dane wyjściowe dzienników debugowania systemu Android zostanie zawiera komunikatu podobnego do:

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

Jeśli tak, istnieją dwie możliwe przyczyny to:

1.  Apk nie zapewnia interfejsu ABI obsługującego urządzenia docelowego.
    Na przykład apk zawiera tylko pliki binarne armeabi v7a i armeabi obsługuje tylko urządzenia docelowego.

2.  [Android usterki](http://code.google.com/p/android/issues/detail?id=21670). Jeśli jest to możliwe, odinstalowanie aplikacji, między palcami i zainstaluj ponownie aplikację.

Aby rozwiązać problem (1), Edytuj opcje/właściwości projektu i [dodać obsługę ABI wymagane do listy obsługiwanych ABIs](~/android/app-fundamentals/cpu-architectures.md). Aby ustalić, które ABI należy dodać, uruchom następujące polecenie adb na urządzeniu docelowym:

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

Dane wyjściowe będą zawierać serwera podstawowego (i opcjonalnie pomocnicze) ABIs.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>Nie ustawiono właściwości OutPath dla projektu &ldquo;MyApp.csproj&rdquo;

Oznacza to zwykle, HP komputera i zmiennej środowiskowej &ldquo;platformy&rdquo; została ustawiona jako element MCD lub HPD. To powoduje konflikt z właściwością platformy MSBuild, która zwykle jest ustawiona na &ldquo;Any CPU&rdquo; lub &ldquo;x86&rdquo;. Będzie konieczne usunięcie tej zmiennej środowiskowej z komputera, zanim program MSBuild mógł działać:

-   Panel sterowania > System > Zaawansowane > Zmienne środowiskowe

Uruchom ponownie program Visual Studio lub Visual Studio dla komputerów Mac i spróbuj przebudować. Elementy teraz powinny działać zgodnie z oczekiwaniami.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException: nie można rzutować mono.android.runtime.JavaObject...

Xamarin.Android 4.x prawidłowo nie kierować zagnieżdżone typy ogólne poprawnie. Na przykład, należy wziąć pod uwagę następujące C\# w kodzie za pomocą [SimpleExpandableListAdapter](https://developer.xamarin.com/api/type/Android.Widget.SimpleExpandableListAdapter/):


```csharp
// BAD CODE; DO NOT USE
var groupData = new List<IDictionary<string, object>> () {
        new Dictionary<string, object> {
                { "NAME", "Group 1" },
                { "IS_EVEN", "This group is odd" },
        },
};
var childData = new List<IList<IDictionary<string, object>>> () {
        new List<IDictionary<string, object>> {
                new Dictionary<string, object> {
                        { "NAME", "Child 1" },
                        { "IS_EVEN", "This group is odd" },
                },
        },
};
mAdapter = new SimpleExpandableListAdapter (
        this,
        groupData,
        Android.Resource.Layout.SimpleExpandableListItem1,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
        childData,
        Android.Resource.Layout.SimpleExpandableListItem2,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
);
```


Problem polega na tym, że Xamarin.Android niepoprawnie marshals zagnieżdżone typy ogólne. `List<IDictionary<string, object>>` Jest są przekazywane do [java.lang.ArrrayList](https://developer.xamarin.com/api/type/Java.Util.ArrayList/), ale `ArrayList` jest zawierającego `mono.android.runtime.JavaObject` wystąpień (które odwołanie `Dictionary<string, object>` wystąpień) zamiast obiekt implementujący [java.util.Map](https://developer.xamarin.com/api/type/Java.Util.IMap/), co następujący wyjątek:

```shell
E/AndroidRuntime( 2991): FATAL EXCEPTION: main
E/AndroidRuntime( 2991): java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to java.util.Map
E/AndroidRuntime( 2991):        at android.widget.SimpleExpandableListAdapter.getGroupView(SimpleExpandableListAdapter.java:278)
E/AndroidRuntime( 2991):        at android.widget.ExpandableListConnector.getView(ExpandableListConnector.java:446)
E/AndroidRuntime( 2991):        at android.widget.AbsListView.obtainView(AbsListView.java:2271)
E/AndroidRuntime( 2991):        at android.widget.ListView.makeAndAddView(ListView.java:1769)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillDown(ListView.java:672)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillFromTop(ListView.java:733)
E/AndroidRuntime( 2991):        at android.widget.ListView.layoutChildren(ListView.java:1622)
```

Obejście tego problemu jest użycie udostępnionych [typy kolekcji Java](~/android/internals/api-design.md) zamiast `System.Collections.Generic` typy &ldquo;wewnętrzny&rdquo; typów. To spowoduje odpowiednie typy Java podczas organizowania wystąpienia. (Poniższy kod jest bardziej skomplikowane niż jest to konieczne w celu zmniejszenia gref okresy istnienia. Można uprościć do zmiany oryginalnego kodu za pomocą `s/List/JavaList/g` i `s/Dictionary/JavaDictionary/g` Jeśli okresy istnienia gref nie martw się.)

```csharp
// insert good code here
using (var groupData = new JavaList<IDictionary<string, object>> ()) {
    using (var groupEntry = new JavaDictionary<string, object> ()) {
        groupEntry.Add ("NAME", "Group 1");
        groupEntry.Add ("IS_EVEN", "This group is odd");
        groupData.Add (groupEntry);
    }
    using (var childData = new JavaList<IList<IDictionary<string, object>>> ()) {
        using (var childEntry = new JavaList<IDictionary<string, object>> ())
        using (var childEntryDict = new JavaDictionary<string, object> ()) {
            childEntryDict.Add ("NAME", "Child 1");
            childEntryDict.Add ("IS_EVEN", "This child is odd.");
            childEntry.Add (childEntryDict);
            childData.Add (childEntry);
        }
        mAdapter = new SimpleExpandableListAdapter (
            this,
            groupData,
            Android.Resource.Layout.SimpleExpandableListItem1,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
            childData,
            Android.Resource.Layout.SimpleExpandableListItem2,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
        );
    }
}
```

[Ten problem zostanie rozwiązany w przyszłej wersji](https://bugzilla.xamarin.com/show_bug.cgi?id=5401).


## <a name="unexpected-nullreferenceexceptions"></a>Nieoczekiwany NullReferenceExceptions

Czasami [dzienników debugowania systemu Android](~/android/deploy-test/debugging/android-debug-log.md) zaznaczyć NullReferenceExceptions który &ldquo;nie może mieć miejsce,&rdquo; lub pochodzić z Mono dla systemu Android środowiska uruchomieniowego kodu tuż przed śmierci aplikacji:

```shell
E/mono(15202): Unhandled Exception: System.NullReferenceException: Object reference not set to an instance of an object
E/mono(15202):   at Java.Lang.Object.GetObject (IntPtr handle, System.Type type, Boolean owned)
E/mono(15202):   at Java.Lang.Object._GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Java.Lang.Object.GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Android.Views.View+IOnTouchListenerAdapter.n_OnTouch_Landroid_view_View_Landroid_view_MotionEvent_(IntPtr jnienv, IntPtr native__this, IntPtr native_v, IntPtr native_e)
E/mono(15202):   at (wrapper dynamic-method) object:b039cbb0-15e9-4f47-87ce-442060701362 (intptr,intptr,intptr,intptr)
```

lub

```shell
E/mono    ( 4176): Unhandled Exception:
E/mono    ( 4176): System.NullReferenceException: Object reference not set to an instance of an object
E/mono    ( 4176): at Android.Runtime.JNIEnv.NewString (string)
E/mono    ( 4176): at Android.Util.Log.Info (string,string)
```

Może się to zdarzyć, gdy Android środowiska uruchomieniowego decyduje się na przerwanie procesu, który może mieć dowolną liczbę powodów, takich jak naciśnięcie limit GREF element docelowy lub robi &ldquo;niewłaściwy&rdquo; z JNI.

Aby zobaczyć, jeśli jest to możliwe, sprawdź Android dziennik debugowania komunikatu podobny do procesu:

```shell
E/dalvikvm(  123): VM aborting
```


## <a name="abort-due-to-global-reference-exhaustion"></a>Przerwano działanie z powodu wyczerpania globalnej odwołania

Warstwa JNI wykonawczym systemu Android obsługuje tylko ograniczoną liczbę odwołań do obiektów JNI ma obowiązywać w każdej w czasie. W przypadku przekroczenia tego limitu, Podziel rzeczy.

GREF (*odwołania globalnej*) limit to 2000 odwołań w emulatorze i ~ 52000 odwołuje się na sprzęcie.

Wiadomo, że zaczynasz tworzenie zbyt wiele GREFs po wyświetleniu komunikaty, takie jak to w dzienniku systemu Android debugowania:

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

Po osiągnięciu limitu GREF drukowania wiadomości, takie jak następujące:

```shell
D/dalvikvm(  602): GREF has increased to 2001
W/dalvikvm(  602): Last 10 entries in JNI global reference table:
W/dalvikvm(  602):  1991: 0x4057eff8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1992: 0x4057f010 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1993: 0x40698e70 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1994: 0x40698e88 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1995: 0x40698ea0 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1996: 0x406981f0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1997: 0x40698208 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1998: 0x40698220 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1999: 0x406956a8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  2000: 0x406956c0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602): JNI global reference table summary (2001 entries):
W/dalvikvm(  602):    51 of Ljava/lang/Class; 164B (41 unique)
W/dalvikvm(  602):    46 of Ljava/lang/Class; 188B (17 unique)
W/dalvikvm(  602):     6 of Ljava/lang/Class; 212B (6 unique)
W/dalvikvm(  602):    11 of Ljava/lang/Class; 236B (7 unique)
W/dalvikvm(  602):     3 of Ljava/lang/Class; 260B (3 unique)
W/dalvikvm(  602):     4 of Ljava/lang/Class; 284B (2 unique)
W/dalvikvm(  602):     8 of Ljava/lang/Class; 308B (6 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 316B
W/dalvikvm(  602):     4 of Ljava/lang/Class; 332B (3 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 356B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 380B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 452B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 476B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 500B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 548B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 572B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 596B (2 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 692B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 956B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1004B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1148B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 1172B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1316B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3452B
W/dalvikvm(  602):     1 of Ljava/lang/String; 28B
W/dalvikvm(  602):     2 of Ldalvik/system/VMRuntime; 12B (1 unique)
W/dalvikvm(  602):    10 of Ljava/lang/ref/WeakReference; 28B (10 unique)
W/dalvikvm(  602):     1 of Ldalvik/system/PathClassLoader; 44B
W/dalvikvm(  602):  1553 of Landroid/graphics/Point; 20B (1553 unique)
W/dalvikvm(  602):   261 of Landroid/graphics/Point; 28B (261 unique)
W/dalvikvm(  602):     1 of Landroid/view/MotionEvent; 100B
W/dalvikvm(  602):     1 of Landroid/app/ActivityThread$ApplicationThread; 28B
W/dalvikvm(  602):     1 of Landroid/content/ContentProvider$Transport; 28B
W/dalvikvm(  602):     1 of Landroid/view/Surface$CompatibleCanvas; 44B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$ControlledInputConnectionWrapper; 36B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$1; 12B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$W; 28B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/view/accessibility/AccessibilityManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout$LayoutParams; 44B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout; 332B
W/dalvikvm(  602):     2 of Lorg/apache/harmony/xnet/provider/jsse/TrustManagerImpl; 28B (1 unique)
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$MyWindow; 36B
W/dalvikvm(  602):     1 of Ltouchtest/RenderThread; 92B
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$3; 12B
W/dalvikvm(  602):     1 of Ltouchtest/DrawingView; 412B
W/dalvikvm(  602):     1 of Ltouchtest/Activity1; 180B
W/dalvikvm(  602): Memory held directly by tracked refs is 75624 bytes
E/dalvikvm(  602): Excessive JNI global references (2001)
E/dalvikvm(  602): VM aborting
```


W powyższym przykładzie (okazjonalnie, które pochodzą z [usterek 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)) problemu jest to, że zbyt wiele Android.Graphics.Point tworzone są wystąpienia; zobacz [komentarz \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2) listę poprawek dla tego konkretnego błędu.

Zazwyczaj jest to przydatne rozwiązanie można znaleźć typu ma zbyt wiele wystąpień przydzielone &ndash; Android.Graphics.Point w powyższym zrzutu &ndash; następnie znajdź, w których są generowane w kodzie źródłowym i usuwania ich odpowiednio (dzięki czemu ich Okres istnienia Java-Object jest obcinana). Nie jest to zawsze właściwe (\#685215 jest wielowątkowe, więc trivial rozwiązanie pozwala uniknąć wywołanie metody Dispose), ale to przede wszystkim należy wziąć pod uwagę.

Można włączyć [GREF rejestrowanie](~/android/troubleshooting/index.md) utworzenia GREFs i ile istnieją.


## <a name="abort-due-to-jni-type-mismatch"></a>Przerwano działanie z powodu niezgodności typów JNI

Jeśli musisz ręcznie listy JNI kodu, może się zdarzyć, że typy nie są zgodne poprawnie, np. Próba wywołania `java.lang.Runnable.run` na typ, który nie implementuje `java.lang.Runnable`. W takim przypadku będzie komunikat podobny do poniższego w dzienniku systemu Android debugowania:

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>Obsługa dynamicznego kodu

### <a name="dynamic-code-does-not-compile"></a>Kod dynamicznych nie kompiluje się

Aby użyć C\# dynamicznych aplikacji lub w bibliotece, należy dodać System.Core.dll, Microsoft.CSharp.dll i Mono.CSharp.dll do projektu.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>W kompilacji wydania missingmethodexception — występuje dynamiczne kodu w czasie wykonywania.

-   Istnieje prawdopodobieństwo, że projekt aplikacji nie ma odwołania do System.Core.dll, Microsoft.CSharp.dll lub Mono.CSharp.dll. Upewnij się, że te zestawy są przywoływane.

    -   Należy pamiętać dynamiczny kod zawsze kosztów. Jeśli potrzebne są wydajne kodu, należy wziąć pod uwagę nie przy użyciu kodu dynamicznych.

-   W pierwszej wersji zapoznawczej te zestawy zostały wyłączone, chyba że typy w każdym zestawie jawnie są używane przez kod aplikacji. Zobacz następujące obejścia: [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>Projekty z drzewa obiektów aplikacji + LLVM awarii w oparciu x86 urządzeń

W przypadku wdrażania aplikacji skompilowanej za pomocą [drzewa obiektów aplikacji + LLVM](~/android/deploy-test/release-prep/index.md) na urządzeniach z systemem x86, zobaczysz komunikat o błędzie podobny do następującego:

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

Jest to znany problem, zgodnie z informacjami w [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111). Obejście jest wyłączenie LLVM.
