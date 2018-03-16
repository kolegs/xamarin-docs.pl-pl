---
title: Proces kompilacji
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2018
ms.openlocfilehash: 30cfb1c8bbd65ec8ef69d2d9bc22906a8726ae62
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="build-process"></a>Proces kompilacji


## <a name="overview"></a>Omówienie

Proces kompilacji Xamarin.Android jest odpowiedzialny za przyklejanie wszystko, co łącznie: [generowania `Resource.designer.cs` ](~/android/internals/api-design.md), pomocnicze `AndroidAsset`, `AndroidResource`i innych [akcji](#Build_Actions), Generowanie [Android wywoływane otoki](~/android/platform/java-integration/android-callable-wrappers.md)oraz generowanie `.apk` do wykonania na urządzeniach z systemem Android.


## <a name="application-packages"></a>Pakiety aplikacji

Szerokie warunków, istnieją dwa typy pakietów aplikacji systemu Android (`.apk` plików), które mogą generować Xamarin.Android system kompilacji:

-   **Wersja** kompilacje, które są całkowicie niezależne i nie wymagają dodatkowych pakietów do wykonania. Są to pakiety, które można dostarczyć do sklepu z aplikacjami.

-   **Debugowanie** kompilacje, które nie są.

Nie przypadkowo odpowiadają one MSBuild `Configuration` który tworzy pakiet.


### <a name="shared-runtime"></a>Udostępniony środowiska wykonawczego

*Udostępnionych środowiska uruchomieniowego* jest para dodatkowe pakiety systemu Android, które zawierają Biblioteka klasy podstawowej (`mscorlib.dll`, itd.) i biblioteki systemu Android powiązania (`Mono.Android.dll`itp.). Debugowanie kompilacji opierają się na udostępnionym środowiska uruchomieniowego zamiast tym Biblioteka klasy podstawowej i zestawy powiązania w pakiecie aplikacji systemu Android, dzięki czemu pakiet debugowania może być mniejszy.

Udostępniony środowiska uruchomieniowego, które mogą być wyłączone w kompilacjach debugowania przez ustawienie `$(AndroidUseSharedRuntime)` właściwości `False`.

<a name="Fast_Deployment" />

### <a name="fast-deployment"></a>Szybkie wdrażanie

*Szybkie wdrażanie* współdziała z udostępnionego środowiska uruchomieniowego dalsze zmniejszenie rozmiaru pakietu aplikacji systemu Android. Jest to realizowane przez nie tworzenie pakietów aplikacji zestawów w pakiecie. Zamiast tego są kopiowane na urządzeniu docelowym za pośrednictwem `adb push`. Ten proces szybciej cyklu kompilacji/wdrażanie/debug, ponieważ jeśli *tylko* zestawy są zmieniane, pakiet nie zostanie ponownie zainstalowana. Zamiast tego zaktualizowane zestawy są ponownie zsynchronizować na urządzeniu docelowym.

Szybkie wdrożenie jest znany niepowodzenie na urządzeniach, które blokują `adb` z synchronizacją katalogu `/data/data/@PACKAGE_NAME@/files/.__override__`.

Szybkie wdrożenie jest domyślnie włączona i mogą być wyłączone w wydajność debugowania kompilacji dzięki ustawienie `$(EmbedAssembliesIntoApk)` właściwości `True`.



## <a name="msbuild-projects"></a>Projektów MSBuild

Proces kompilacji Xamarin.Android jest oparta na MSBuild, który jest także format pliku projektu, które są używane przez program Visual Studio for Mac i Visual Studio.
Zwykle, użytkownicy nie muszą ręcznie edytować pliki programu MSBuild &ndash; IDE tworzy funkcjonalnej projektów i aktualizuje je ze zmianami wprowadzonymi i automatycznie wywołują obiekty docelowe kompilacji zgodnie z potrzebami.

Użytkownicy wersji Advanced możesz wykonać czynności nie są obsługiwane przez interfejsu graficznego programu IDE, więc proces kompilacji jest funkcję przez bezpośrednią edycję pliku projektu.
Ta strona dokumentów tylko funkcje specyficzne dla platformy Xamarin.Android i dostosowania &ndash; wiele więcej opcji są możliwe z normalnym MSBuild elementów, właściwości i elementów docelowych.

<a name="Build_Targets" />

## <a name="build-targets"></a>Tworzenie elementów docelowych

Następujące obiekty docelowe kompilacji są definiowane dla platformy Xamarin.Android projektów:

-   **Tworzenie** &ndash; tworzy pakiet.

-   **Wyczyść** &ndash; spowoduje usunięcie wszystkich plików wygenerowanych przez proces kompilacji.

-   **Zainstaluj** &ndash; instaluje pakiet na domyślne urządzenie lub urządzenia wirtualnego.

-   **Odinstaluj** &ndash; odinstalowuje pakiet z domyślne urządzenie lub urządzenia wirtualnego.

-   **SignAndroidPackage** &ndash; tworzy i rejestruje pakietu (`.apk`). Za pomocą `/p:Configuration=Release` do generowania niezależne pakiety "Wersja".

-   **UpdateAndroidResources** &ndash; aktualizacje `Resource.designer.cs` pliku. Ten element docelowy jest zazwyczaj wywoływana IDE po dodaniu nowych zasobów do projektu.


## <a name="build-properties"></a>Właściwości kompilacji

Właściwości programu MSBuild kontrolują zachowanie docelowych. Są one np. określone w pliku projektu **MyApp.csproj**, poziomu [MSBuild PropertyGroup — element](http://msdn.microsoft.com/en-us/library/t4w159bs.aspx).

-   **Konfiguracja** &ndash; Określa konfigurację kompilacji, aby użyć, np. "Debug" i "Release". Właściwość konfiguracji jest używana do określania wartości domyślnej dla innych właściwości, które określają zachowanie docelowego. Można tworzyć dodatkowe konfiguracje w ramach programu IDE.

    *Domyślnie*, `Debug` konfiguracji powoduje `Install` i `SignAndroidPackage` celów tworzenia mniejszych pakietu systemu Android, która wymaga obecności inne pliki i pakietów do działania.

    Wartość domyślna `Release` konfiguracji powoduje w `Install` i `SignAndroidPackage` celów tworzenia pakietu, który jest Android *autonomicznej*i może być używany bez instalowania innych pakietów lub plików.

-   **DebugSymbols** &ndash; wartość logiczna, która określa, czy jest pakietu systemu Android *możliwością debugowania*, w połączeniu z `$(DebugType)` właściwości. Możliwością debugowania pakiet zawiera symbole debugowania, ustawia `//application/@android:debuggable` atrybutu `true`i automatycznie dodaje `INTERNET` uprawnienia tak, aby debuger może dołączyć do procesu. Aplikacja jest możliwością debugowania Jeśli `DebugSymbols` jest `True` *i* `DebugType` jest pustym ciągiem lub `Full`.

-   **DebugType** &ndash; Określa [typu symboli debugowania](http://msdn.microsoft.com/en-us/library/s5c8athz.aspx) do generowania jako część kompilacji, która wpływa także, czy aplikacja jest możliwością debugowania. Możliwe wartości:

    - **Pełna**: Pełna symbole zostaną wygenerowane. Jeśli `DebugSymbols` właściwość MSBuild jest również `True`, a następnie pakiet aplikacji jest możliwością debugowania.

    - **PdbOnly**: symbole "PDB" zostaną wygenerowane. Pakiet aplikacji będzie *nie* debugować.

    Jeśli `DebugType` nie jest ustawiona lub jest pustym ciągiem, a następnie `DebugSymbols` właściwość określa, czy tą aplikacja jest możliwością debugowania.


### <a name="install-properties"></a>Zainstaluj właściwości

Właściwości instalacji sterowania zachowaniem `Install` i `Uninstall` elementów docelowych.

-   **AdbTarget** &ndash; określa może być zainstalowana tak, aby lub usunięte z urządzenia z systemem Android docelowego pakietu systemu Android. Wartość tej właściwości jest taka sama jak [ `adb` opcji urządzenie docelowe](http://developer.android.com/tools/help/adb.html#issuingcommands):

    ```bash
    # Install package onto emulator via -e
    # Use `/Library/Frameworks/Mono.framework/Commands/msbuild` on OS X
    MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
    ```


### <a name="packaging-properties"></a>Właściwości pakowania

Właściwości pakowania kontroli tworzenia pakietu systemu Android i są używane przez `Install` i `SignAndroidPackage` elementów docelowych.
[Właściwości podpisywania](#Signing_Properties) mają również zastosowanie podczas packaing wersji aplikacji.


-   **AndroidApplication** &ndash; wartość logiczną wskazującą, czy projekt jest dla aplikacji systemu Android (`True`) lub w projekcie biblioteki systemu Android (`False` lub nieobecny).

    Tylko jeden projekt z `<AndroidApplication>True</AndroidApplication>` może znajdować się w ramach pakietu systemu Android. (Niestety ta nie została jeszcze zweryfikowana, co może skutkować delikatny i Nieoczekiwana błędy dotyczące zasobów dla systemu Android.)

-   **AndroidBuildApplicationPackage** &ndash; wartość logiczna, która wskazuje, czy należy utworzyć i podpisać pakiecie (apk). Ustawienie tej wartości na `True` odpowiada za pomocą [SignAndroidPackage](#Build_Targets) kompilacji docelowej.

    Po Xamarin.Android 7.1 została dodana obsługa dla tej właściwości.

    Ta właściwość jest `False` domyślnie.

-   **AndroidEnableMultiDex** &ndash; właściwości typu boolean, która określa, czy obsługa wielu dex będzie używany w ostatecznym `.apk`.

    Obsługa tej właściwości został dodany w 5.1 platformy Xamarin.Android.

    Ta właściwość jest `False` domyślnie.

-   **AndroidEnableSGenConcurrent** &ndash; właściwości typu boolean określającą czy Mono w [równoczesnych GC modułu zbierającego](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen) będą używane.

    Obsługa tej właściwości został dodany w 7.2 platformy Xamarin.Android.

    Ta właściwość jest `False` domyślnie.

-   **AndroidFastDeploymentType** &ndash; A `:` (dwukropek)-rozdzielana lista wartości, aby kontrolować, jakie typy mogą być wdrażane [katalogu szybkiego wdrożenia](#Fast_Deployment) na urządzeniu docelowym po `$(EmbedAssembliesIntoApk)` Właściwość MSBuild jest `False`. Jeśli zasób jest szybkie wdrożony, to *nie* osadzone w wygenerowanym `.apk`, który przyspiesza czas wdrażania. (Więcej, który jest szybkie wdrożone, następnie rzadziej `.apk` musi zostać również przebudowany i proces instalacji może być szybsza.) Prawidłowe wartości to:

    - `Assemblies`: Wdrażanie zestawów aplikacji.

    - `Dexes`: Wdrażanie `.dex` plików, Android zasobów i zasoby systemu Android. **Ta wartość może *tylko* można używać na urządzeniach z systemem Android 4.4 lub nowszy (interfejs API-19).**

    Wartość domyślna to `Assemblies`.

    **Eksperymentalne**. Dodane w Xamarin.Android 6.1.

-   **AndroidApplicationJavaClass** &ndash; Pełna nazwa klasy Java do użycia zamiast `android.app.Application` po klasie dziedziczy [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/).

    Ta właściwość jest zwykle ustawiona *innych* właściwości, takie jak `$(AndroidEnableMultiDex)` właściwości programu MSBuild.

    Dodane w Xamarin.Android 6.1.

-   **AndroidHttpClientHandlerType** &ndash; umożliwia ustawienie wartości [ `XA_HTTP_CLIENT_HANDLER_TYPE` zmiennej środowiskowej](~/android/deploy-test/environment.md).
    Ta wartość nie spowoduje zastąpienia jawnie określona `XA_HTTP_CLIENT_HANDLER_TYPE` wartość. `XA_HTTP_CLIENT_HANDLER_TYPE` Określony w wartości zmiennej środowiskowej [ `@(AndroidEnvironment)` ](#AndroidEnvironment) pliku będzie mieć wyższy priorytet.

    Dodane w Xamarin.Android 6.1.

-   **AndroidTlsProvider** &ndash; wartość ciągu, która określa, który dostawca TLS powinny być używane w aplikacji. Możliwe wartości to: prawidłowe wartości to:

    - `btls`: Użyj [wytaczania SSL](https://boringssl.googlesource.com/boringssl) TLS komunikacji z [HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.aspx).
      Umożliwia to korzystanie z protokołu TLS 1.2.

    - `legacy`: Użyj historycznych zarządzaną implementację protokołu SSL do interakcji z sieci. To *nie* obsługi protokołu TLS 1.2.

    - `default`, lub nie ustawiono / pusty ciąg: Xamarin.Android 7.1, co jest równoważne `legacy`.

    Wartość domyślna to ciąg pusty.

    **Eksperymentalne**. Dodane w Xamarin.Android 7.1.

-   **AndroidLinkMode** &ndash; Określa typ [łączenie](~/android/deploy-test/linker.md) powinien być wykonywany na zestawy zawartych w pakiecie systemu Android. Używana tylko w projektach aplikacji systemu Android. Wartość domyślna to *SdkOnly*. Prawidłowe wartości to:

    - **Brak**: połączenie nie zostanie podjęta.

    - **SdkOnly**: łączenie będą wykonywane na biblioteki klasy podstawowej, nie zestawów użytkownika.

    - **Pełna**: łączenie odbędzie się w klasie podstawowej biblioteki i zestawów użytkownika. **Uwaga:** Using `AndroidLinkMode` wartość *pełne* często skutkuje przerwane aplikacje, szczególnie w przypadku, gdy jest używany odbicia. Unikaj, chyba że użytkownik *naprawdę* wiedzieć, wykonywanych czynności.

    ```xml
    <AndroidLinkMode>SdkOnly</AndroidLinkMode>
    ```

-   **AndroidLinkSkip** &ndash; określa Rozdzielana średnikami (`;`) lista nazw zestawów bez rozszerzeń plików zestawów, które nie powinny być połączone. Używany tylko w projektach aplikacji systemu Android.

    ```xml
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
    ```

-   **AndroidManagedSymbols** &ndash; właściwości typu boolean, czy formanty czy sekwencji punkty są generowane, dzięki czemu można wyodrębnić pliku wiersza numer informacje o nazwie i z `Release` stos danych śledzenia.

    Dodane w Xamarin.Android 6.1.

-   **AndroidManifest** &ndash; Określa nazwę pliku do użycia jako szablon dla aplikacji [ `AndroidManifest.xml` ](~/android/platform/android-manifest.md).
    Podczas kompilacji, zostaną scalone niezbędnych wartości do tworzenia rzeczywistych `AndroidManifest.xml`.
    `$(AndroidManifest)` Musi zawierać nazwę pakietu w `/manifest/@package` atrybutu.

-   **AndroidSdkBuildToolsVersion** &ndash; pakietu narzędzi kompilacji dla systemu Android SDK udostępnia **aapt** i **zipalign** narzędzi, m.in. Jednocześnie można zainstalować wiele różnych wersji pakietu narzędzia kompilacji. Pakiet narzędzi kompilacji wybranych do tworzenia pakietów jest realizowane przez sprawdzanie i przy użyciu wersji narzędzia kompilacji "preferowane", jeśli jest obecny; Jeśli wersja "preferowane" jest *nie* istnieje, a następnie jest używany pakiet narzędzi kompilacji zainstalowany highested z kontrolą wersji.

    `$(AndroidSdkBuildToolsVersion)` Właściwość MSBuild zawiera własną wersję narzędzia kompilacji. System kompilacji platformy Xamarin.Android dostarcza wartość domyślną w `Xamarin.Android.Common.targets`, i wartość domyślną mogą zostać zastąpione w pliku projektu youur wybrać wersję narzędzia kompilacji alternatywne, jeśli (na przykład) awarii najnowsze aapt się podczas poprzedniej wersji aapt jest znany do pracy.

-   **AndroidSupportedAbis** &ndash; właściwości ciągu zawierający średnikiem (`;`) — lista ograniczanych znakami ABIs, które powinny zostać włączone do `.apk`.

    Obsługiwane wartości to:

    -   `armeabi`
    -   `armeabi-v7a`
    -   `x86`
    -   `arm64-v8a`: Wymaga platformy Xamarin.Android 5.1 i nowszych.
    -   `x86_64`: Wymaga platformy Xamarin.Android 5.1 i nowszych.

-   **AndroidUseSharedRuntime** &ndash; określa właściwości typu boolean, która jest czy *udostępnionych pakietów środowiska uruchomieniowego* są wymagane do uruchomienia aplikacji na urządzeniu docelowym. Zależne pakiety udostępnionego środowiska uruchomieniowego umożliwia pakiet aplikacji, przyspieszanie proces tworzenia i wdrażania pakietu, co powoduje szybsze cykl przetwarzania kompilacji/wdrażanie/debug.

    Ta właściwość powinna być `True` w przypadku kompilacji debugowania i `False` dla projektów w wersji.

-   **AotAssemblies** &ndash; właściwości typu boolean, która określa, czy zestawy będzie Ahead elementu Time skompilować kodu natywnego i uwzględniane w `.apk`.

    Obsługa tej właściwości został dodany w 5.1 platformy Xamarin.Android.

    Ta właściwość jest `False` domyślnie.

-   **EmbedAssembliesIntoApk** &ndash; właściwości typu boolean określającą czy zespoły aplikacji powinny osadzone w pakiet aplikacji.

    Ta właściwość powinna być `True` dla wersji kompilacji i `False` w przypadku kompilacji debugowania. On *może* muszą być `True` w kompilacjach debugowania, jeśli szybkiego wdrożenia nie obsługuje urządzenia docelowego.

    Gdy ta właściwość jest `False`, a następnie `$(AndroidFastDeploymentType)` MSBuild właściwość również określa, jaki będzie embedd do `.apk`, która może wpłynąć na czas wdrażania i rebuidl.

-   **EnableLLVM** &ndash; właściwości typu boolean określającą czy LLVM będą używane podczas Ahead z czasu kompilacji assemblines kodu natywnego.

    Obsługa tej właściwości został dodany w 5.1 platformy Xamarin.Android.

    Ta właściwość jest `False` domyślnie.

    Ta właściwość jest ignorowana, chyba że `$(AotAssemblies)` właściwość MSBuild jest `True`.

-   **EnableProguard** &ndash; właściwości typu boolean określającą czy [proguard](http://developer.android.com/tools/help/proguard.html) jest uruchamiany jako część procesu tworzenia pakietów do łączenia kodu języka Java.

    Obsługa tej właściwości został dodany w 5.1 platformy Xamarin.Android.

    Ta właściwość jest `False` domyślnie.

    Gdy `True`, [ProguardConfiguration](#ProguardConfiguration) pliki będą używane do sterowania `proguard` wykonywania.

-   **JavaMaximumHeapSize** &ndash; określa wartość **java** 
     `-Xmx` wartość parametru do użycia podczas tworzenia `.dex` pliku w ramach procesu tworzenia pakietów. Jeśli nie zostanie określony, a następnie `-Xmx` do nie zostanie podana opcja **java**.

    Określenie tej właściwości jest konieczne, jeśli [ `_CompileDex` target zgłasza `java.lang.OutOfMemoryError` ](https://bugzilla.xamarin.com/show_bug.cgi?id=18327).

    ```xml
    <JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
    ```

-   **JavaOptions** &ndash; określa dodatkowe opcje wiersza polecenia do przekazania do **java** podczas kompilowania `.dex` pliku.

-   **MandroidI18n** &ndash; określa obsługę internacjonalizacji dołączona do aplikacji, takich jak sortowania i sortowanie tabel. Wartość jest oddzielonych przecinkami lub średnikami lista co najmniej jeden z następujących wartości bez uwzględniania wielkości liter:

    -   **Brak**: zawierać nie dodatkowych kodowania.

    -   **Wszystkie**: obejmują wszystkie dostępne kodowania.

    -   **CJK**: obejmują w języku chińskim, japońskim i koreańskim kodowania, takich jak *japoński (EUC)* \[kodera jp, CP51932\], *japoński (Shift JIS)* \[ shift ISO-2022-jp,\_jis, CP932\], *japoński (JIS)* \[CP50220\], *chiński uproszczony (GB2312)* \[gb2312, CP936\], *koreański (UHC)* \[łączy\_c\_CP949 5601-1987\], *koreański (EUC)* \[euc-kr, CP51949\], *chiński tradycyjny (Big5)* \[big5, CP950\], i *chiński uproszczony (GB18030)* \[ GB18030, CP54936\].

    -   **MidEast**: kodowania obejmują bliski-Wschodniej, takich jak *turecki (system Windows)* \[iso-8859-9, CP1254\], *hebrajski (system Windows)* \[ 1255 systemu Windows, CP1255\], *arabski (system Windows)* \[1256 systemu windows, CP1256\], *arabski (ISO)* \[iso-8859-6, CP28596\], *Hebrajski (ISO)* \[iso-8859-8, CP28598\], *alfabetu łacińskiego 5 (ISO)* \[iso-8859-9, CP28599\]i *Hebrajski (Iso alternatywna)* \[iso-8859-8, CP38598\].

    -   **Inne**: obejmują takie jak inne kodowanie *cyrylica (system Windows)* \[CP1251\], *Bałtyckiego (system Windows)* \[iso-8859-4 CP1257\], *Wietnamski (system Windows)* \[CP1258\], *cyrylica (KOI8-R)* \[koi8-r, CP1251\], *ukraiński (KOI8-U)*  \[koi8-u, CP1251\], *Bałtyckiego (ISO)* \[iso-8859-4 CP1257\], *cyrylica (ISO)* \[iso-8859-5, CP1251\], *ISCII Davenagari* \[x-iscii-de, CP57002\], *ISCII bengalski* \[x-iscii być, CP57003 \], *ISCII tamilski* \[x-iscii — ta, CP57004\], *ISCII Telugu* \[x-iscii-te CP57005\], *ISCII assamski* \[x-iscii — jako CP57006\], *ISCII orija* \[x iscii lub, CP57007\], *ISCII Kannada* \[x-iscii-ka CP57008\], *ISCII malajalam* \[x-iscii-ma CP57009\], *ISCII Gudżarati* \[x-iscii-gu CP57010\], *ISCII pendżabski* \[x-iscii-pa CP57011\], i *tajski (System Windows)*  \[CP874\].

    -   **Rzadkie**: obejmują rzadkich kodowania, takich jak *IBM EBCDIC (turecki)* \[CP1026\], *IBM EBCDIC (Otwórz systemów Latin 1)* \[CP1047\], *IBM EBCDIC (USA Kanada z EUR)* \[CP1140\], *IBM EBCDIC (Niemcy z EUR)* \[CP1141\], *IBM EBCDIC (Dania/Norwegia z EUR)* \[CP1142\], *IBM EBCDIC (Finlandia/Szwecja z EUR)* \[CP1143\], *IBM EBCDIC (Włochy z EUR)* \[CP1144\], *IBM EBCDIC (Ameryka Łacińska/Hiszpania z EUR)* \[CP1145\], *IBM EBCDIC (Zjednoczone Brytania z EUR)* \[CP1146\], *IBM EBCDIC (Francja z EUR)* \[CP1147\], *IBM EBCDIC (międzynarodowy z EUR)*  \[CP1148\], *IBM EBCDIC (islandzki z EUR)* \[CP1149\], *IBM EBCDIC (Niemcy)* \[ CP20273\], *IBM EBCDIC (Dania/Norwegia)* \[CP20277\], *IBM EBCDIC (Finlandia/Szwecja)* \[CP20278\], *IBM EBCDIC (Włochy)* \[CP20280\], *IBM EBCDIC (Ameryka Łacińska/Hiszpania)* \[CP20284\], *IBM EBCDIC (Zjednoczone Królestwo)* \[CP20285\], *IBM EBCDIC (japoński Katakana rozszerzony)* \[CP20290\], *IBM EBCDIC (Francja)* \[CP20297\], *IBM EBCDIC (arabski)* \[CP20420\], *IBM EBCDIC (wersja hebrajska)* \[CP20424\], *IBM EBCDIC (islandzki)* \[ CP20871\], *IBM EBCDIC (cyrylica — serbski, bułgarski)* \[CP21025\], *IBM EBCDIC (USA Kanada)* \[ CP37\], *IBM EBCDIC (międzynarodowy)* \[CP500\], *arabski (ASMO 708){10

    -   **Zachodnie**: obejmują zachodni kodowania, takich jak *Europa Zachodnia (Mac)* \[macintosh, CP10000\], *islandzkim (Mac)* \[x-mac islandzkim CP10079\], *centralnej Europejskiego (system Windows)* \[iso 8859-2, CP1250\], *Europa Zachodnia (system Windows)* \[iso 8859-1, CP1252\], *Grecki (system Windows)* \[iso-8859-7 CP1253\], *centralnej Europejskiego (ISO)* \[iso 8859-2, CP28592\], *Alfabetu łacińskiego 3 (ISO)* \[iso 8859-3 CP28593\], *Grecki (ISO)* \[iso-8859-7 CP28597\], *Łaciński 9 (ISO)*  \[iso-8859-15, CP28605\], *Stanów Zjednoczonych OEM* \[CP437\], *zachodni Europejskiego (DOS)* \[CP850\], *portugalski (DOS)* \[CP860\], *islandzkim (DOS)* \[CP861\],  *Francuski kanadyjskich (DOS)* \[CP863\], i *Nordycki (DOS)* \[CP865\].

    ```xml
    <MandroidI18n>West</MandroidI18n>
    ```

-   **MonoSymbolArchive** &ndash; właściwości typu boolean, który kontroluje, czy `.mSYM` artefakty są tworzone w celu późniejszego użycia z `mono-symbolicate`, aby wyodrębnić &ldquo;rzeczywistych&rdquo; nazwę pliku i wiersza informacji z o numerze Wersja śladów stosu.

    Jest to wartość PRAWDA domyślnie &ldquo;wersji&rdquo; aplikacje, które mają włączone symbole debugowania: `$(EmbedAssembliesIntoApk)` ma wartość PRAWDA, `$(DebugSymbols)` ma wartość PRAWDA, i `$(Optimize)` ma wartość True.

    Dodane w Xamarin.Android 7.1.

-   **AndroidVersionCodePattern** &ndash; właściwości ciągu, który umożliwia deweloperowi dostosować `versionCode` w manifeście.
    Zobacz [tworzenie kod wersji dla plik APK](~/android/deploy-test/building-apps/abi-specific-apks.md) informacji na temat wyboru `versionCode`.

    Przykłady, jeśli `abi` jest `armeabi` i `versionCode` w manifeście jest `123`, `{abi}{versionCode}` utworzy versionCode z `1123` podczas `$(AndroidCreatePackagePerAbi)` ma wartość PRAWDA, w przeciwnym razie zostanie uzyskiwania wartości 123.
    Jeśli `abi` jest `x86_64` i `versionCode` w manifeście jest `44`. Spowoduje to utworzenie `544` podczas `$(AndroidCreatePackagePerAbi)` ma wartość PRAWDA, w przeciwnym razie będzie mieć wartość z `44`.

    Jeśli przeprowadzamy Lewe dopełnienie ciąg formatu `{abi}{versionCode:0000}`, jego dałby w efekcie `50044` ponieważ są pozostawiane, dopełnienie `versionCode` z `0`. Możesz też użyć przecinka, takie jak uzupełnianie `{abi}{versionCode:D4}` która działa identycznie jak poprzedni.

    Tylko "0" i "Dx" padding format ciągi są obsługiwane, ponieważ wartość musi być liczbą całkowitą.

    Wstępnie zdefiniowane elementy klucza

    -   **ABI** &ndash; wstawia abi docelowych dla aplikacji
        -   1 &ndash; `armeabi`
        -   2 &ndash; `armeabi-v7a`
        -   3 &ndash; `x86`
        -   4 &ndash; `arm64-v8a`
        -   5 &ndash; `x86_64`

    -   **minSDK** &ndash; wstawia minimalna obsługiwana wartość zestawu Sdk z `AndroidManifest.xml` lub `11` Jeśli brak jest zdefiniowany.

    -   **versionCode** &ndash; używa wersji direrctly kodu z `Properties\AndroidManifest.xml`.

    Możesz zdefiniować niestandardowy elementów za pomocą `AndroidVersionCodeProperties` właściwość (dalej).

    Dodane w Xamarin.Android 7.2.

-   **AndroidVersionCodeProperties** &ndash; właściwości ciągu, który umożliwia deweloperowi definiowania niestandardowych elementów do użycia z `AndroidVersionCodePattern`. Są one w formie `key=value` pary. Wszystkie elementy w `value` powinny być wartościami całkowitymi. Na przykład: `screen=23;target=$(_SupportedApiLevel)`. Jak widać, możesz wprowadzić, użyj programu MSBuild istniejących lub niestandardowych właściwości w ciągu.

    Dodane w Xamarin.Android 7.2.


### <a name="binding-project-build-properties"></a>Właściwości kompilacji projektu powiązania

Następujące właściwości programu MSBuild są używane z [powiązania projektów](~/android/platform/binding-java-library/index.md):

-   **AndroidClassParser** &ndash; właściwości ciągu, który kontroluje sposób `.jar` pobieranych plików. Możliwe wartości:

    - **Klasa analizy**: używa `class-parse.exe` można przeanalizować kodu bajtowego Java bezpośrednio, bez pomocy maszyny wirtualnej Java. Ta wartość jest eksperymentalne.


    - **jar2xml**: Użyj `jar2xml.jar` używać odbicia Java w celu wyodrębnienia typów i członków z `.jar` pliku.

    Korzyści wynikające z `class-parse` za pośrednictwem `jar2xml` są:

    - `class-parse` jest w stanie można wyodrębnić nazwy parametrów z kodu bajtowego Java, która zawiera *debugowania* symbole (np. kodu bajtowego skompilowane z `javac -g`).

    - `class-parse` nie "Pomiń" klasy, które dziedziczą z lub zawierać elementów członkowskich typu nierozpoznane.

    **Eksperymentalne**. Added in Xamarin.Android 6.0.

    Wartość domyślna to `jar2xml`.

    Wartość domyślna ulegnie zmianie w przyszłych wydaniach.

-   **AndroidCodegenTarget** &ndash; właściwości ciągu, który kontroluje ABI element docelowy generowania kodu. Możliwe wartości:

    - **XamarinAndroid**: używa interfejsu API powiązania JNI, które są obecne w od Mono 1.0 z systemem Android. Zestawy powiązanie skompilowanej za pomocą platformy Xamarin.Android 5.0 lub nowszej można tylko na platformy Xamarin.Android 5.0 lub nowszej (dodatków interfejsu API/ABI), ale *źródła* jest zgodny z poprzednich wersji produktu.

    - **XAJavaInterop1**: Java.Interop używany dla wywołań JNI. Powiązania zestawów przy użyciu `XAJavaInterop1` można tylko tworzenie i wykonywanie z platformy Xamarin.Android 6.1 lub nowszej. 6.1 platformy Xamarin.Android i nowszym bind `Mono.Android.dll` o tej wartości.

      Korzyści wynikające z `XAJavaInterop1` obejmują:

      - Mniejsze zestawy.

      - `jmethodID` buforowanie `base` wywołań metody opisywanego tak długo, jak wszystkie inne powiązanie typy w hierarchii dziedziczenia są tworzone za `XAJavaInterop1` lub nowszym.

      - `jmethodID` buforowanie Java wywoływana otoka konstruktory podklasy zarządzanych.

    Wartość domyślna to `XamarinAndroid`.

    Wartość domyślna ulegnie zmianie w przyszłych wydaniach.



### <a name="resource-properties"></a>Właściwości zasobów

Właściwości zasobów formantu Generowanie `Resource.designer.cs` pliku, który zapewnia dostęp do zasobów systemu Android.

-   **AndroidResgenExtraArgs** &ndash; określa dodatkowe opcje wiersza polecenia do przekazania do **aapt** poleceń podczas przetwarzania zasoby systemu Android i zasobów.

-   **AndroidResgenFile** &ndash; Określa nazwę pliku zasobu do wygenerowania. Domyślny szablon ustawia to `Resource.designer.cs`.

-   **MonoAndroidResourcePrefix** &ndash; Określa *prefiks ścieżki* który zostanie usunięty z początku nazwy plików z akcją kompilacji `AndroidResource`. To można zmieniać, w którym znajdują się zasoby.

    Wartość domyślna to `Resources`. Aby zmienić `res` dla języka Java projektu struktury.

-   **AndroidExplicitCrunch** &ndash; Jeśli tworzysz aplikację z bardzo dużej liczby drawables lokalnego, tworzenie początkowej (lub odbudowywanie) może zająć minut. Aby przyspieszyć proces kompilacji, spróbuj włącznie tej właściwości, a ustawienie `True`. Gdy ta właściwość jest ustawiona, proces kompilacji crunches wstępnie pliki PNG.

    **Eksperymentalne**. Dodane w Xamarin.Android 7.0.


<a name="Signing_Properties" />

### <a name="signing-properties"></a>Podpisywanie właściwości

Podpisywanie właściwości kontrolują sposób pakiet aplikacji jest podpisany, dzięki czemu mogą być zainstalowane na urządzeniu z systemem Android. Umożliwia szybsze iteracji kompilacji zadania platformy Xamarin.Android nie podpisywania pakietów podczas procesu kompilacji, ponieważ podpisywanie jest bardzo wolno. Zamiast tego są podpisane (w razie potrzeby) przed rozpoczęciem instalacji lub podczas eksportu IDE lub *zainstalować* kompilacji docelowej. Wywoływanie *SignAndroidPackage* docelowej utworzy pakiet o `-Signed.apk` sufiks w katalogu wyjściowego.

Domyślnie element docelowy podpisywania generuje nowego klucza podpisywania debugowania, w razie potrzeby. Jeśli chcesz użyć określonego klucza, na przykład na serwerze kompilacji następujące właściwości programu MSBuild można:

-   **AndroidKeyStore** &ndash; wartość logiczną wskazującą, czy można używać niestandardowych informacji do podpisywania. Wartość domyślna to `False`, co oznacza, że domyślny klucz podpisywania debugowania będzie służyć do podpisywania pakietów.

-   **AndroidSigningKeyAlias** &ndash; określa aliasu dla klucza w magazynie kluczy. Jest to **keytool-alias** wartość używana podczas tworzenia magazynu kluczy.

-   **AndroidSigningKeyPass** &ndash; Określa hasło dostępu do klucza w pliku magazynu kluczy. Jest to wartość wprowadzona podczas `keytool` zapyta **wprowadź hasło klucza dla $(AndroidSigningKeyAlias)**.

-   **AndroidSigningKeyStore** &ndash; Określa nazwę pliku magazynu kluczy utworzone przez `keytool`. Odpowiada to wartość podana dla **keytool - keystore** opcji.

-   **AndroidSigningStorePass** &ndash; Określa hasło `$(AndroidSigningKeyStore)`. Jest to wartość podana dla `keytool` podczas tworzenia pliku magazynu kluczy i wystąpiły **wprowadź hasło magazynu kluczy:**.

Na przykład, należy wziąć pod uwagę następujące `keytool` wywołania:

```shell
$ keytool -genkey -v -keystore filename.keystore -alias keystore.alias -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password: keystore.filename password
Re-enter new password: keystore.filename password
...
Is CN=... correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: ...
Enter key password for keystore.alias
        (RETURN if same as keystore password): keystore.alias password
[Storing filename.keystore]
```

Aby użyć magazynu kluczy wygenerowany powyżej, użyj właściwości grupy:

```xml
<PropertyGroup>
    <AndroidKeyStore>True</AndroidKeyStore>
    <AndroidSigningKeyStore>filename.keystore</AndroidSigningKeyStore>
    <AndroidSigningStorePass>keystore.filename password</AndroidSigningStorePass>
    <AndroidSigningKeyAlias>keystore.alias</AndroidSigningKeyAlias>
    <AndroidSigningKeyPass>keystore.alias password</AndroidSigningKeyPass>
</PropertyGroup>
```

<a name="Build_Actions" />

## <a name="build-actions"></a>Tworzenie działania

*Tworzenie akcje* są [zastosowane do plików](http://msdn.microsoft.com/en-us/library/bb629388.aspx) w ramach projektu i kontroli sposobu przetwarzania pliku.

<a name="AndroidEnvironment" />

### <a name="androidenvironment"></a>AndroidEnvironment

Pliki z akcją kompilacji `AndroidEnvironment` są używane do [zainicjować zmienne środowiskowe i właściwości systemu podczas uruchamiania procesu](~/android/deploy-test/environment.md).
`AndroidEnvironment` Akcji kompilacji można stosować do wielu plików i zostanie obliczone w losowej kolejności (tak, aby nie Określ tę samą właściwość zmiennej lub systemu środowiska w wielu plikach).


### <a name="androidjavasource"></a>AndroidJavaSource

Pliki z akcją kompilacji `AndroidJavaSource` są kodu źródłowego języka Java, które mają być uwzględnieni w ostatnim pakietu systemu Android.


### <a name="androidjavalibrary"></a>AndroidJavaLibrary

Pliki z akcją kompilacji `AndroidJavaLibrary` są archiwa Java ( `.jar` pliki) które mają być uwzględnieni w ostatnim pakietu systemu Android.


### <a name="androidresource"></a>AndroidResource

Wszystkie pliki z *AndroidResource* akcji kompilacji są kompilowane do zasobów systemu Android podczas procesu kompilacji i udostępniane za pośrednictwem `$(AndroidResgenFile)`.

```xml
<ItemGroup>
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
```

Bardziej zaawansowanym użytkownikom może być może chcesz, aby różne zasoby używane w różnych konfiguracjach, ale o takiej samej ścieżce skuteczne. Można to osiągnąć, mających wiele katalogów zasobów mających pliki z tymi samymi względnymi ścieżkami w tych katalogach różnych i przy użyciu warunki MSBuild warunkowo dołączać pliki różne w różnych konfiguracjach. Na przykład:

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
<ItemGroup  Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug\values\strings.xml"/>
</ItemGroup>
<PropertyGroup>
  <MonoAndroidResourcePrefix>Resources;Resources-Debug<MonoAndroidResourcePrefix>
</PropertyGroup>
```

**LogicalName** &ndash; jawnie określa ścieżki zasobu. Umożliwia &ldquo;aliasów&rdquo; plików, dzięki czemu będą one dostępne jako wiele nazw zasobów distinct.

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources/values/strings.xml"/>
</ItemGroup>
<ItemGroup Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug/values/strings.xml">
    <LogicalName>values/strings.xml</LogicalName>
  </AndroidResource>
</ItemGroup>
```


### <a name="androidnativelibrary"></a>AndroidNativeLibrary

[Natywnych bibliotek](~/android/platform/native-libraries.md) są dodawane do kompilacji, ustawiając ich akcji kompilacji `AndroidNativeLibrary`.

Należy pamiętać, że od systemu Android obsługuje wiele interfejsów binarne aplikacji (ABIs), system kompilacji musi znać które ABI natywnej biblioteki zaprojektowano pod kątem. Istnieją dwa sposoby, które można to zrobić:

1.  Ścieżka "wykrywania".
2.  Przy użyciu `Abi` atrybutu element.

Z wykrywanie ścieżka, nazwa katalogu nadrzędnego natywnej biblioteki służy do określania ABI który cele biblioteki. W związku z tym jeśli dodasz `lib/armeabi/libfoo.so` do kompilacji, następnie ABI będzie można "ten sposób" jako `armeabi`.


### <a name="item-attribute-name"></a>Nazwa atrybutu elementu

**ABI** &ndash; określa ABI natywnej biblioteki.

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```

### <a name="content"></a>Zawartość

Normalnej `Content` Akcja kompilacji nie jest obsługiwana (jak nie okazało się, jak do jego obsługi bez kosztownych prawdopodobnie kroku pierwszego uruchomienia).

Począwszy od platformy Xamarin.Android 5.1, próba użycia thw `@(Content)` Akcja kompilacji spowoduje `XA0101` ostrzeżenie.

### <a name="linkdescription"></a>LinkDescription

Pliki z *LinkDescription* akcji kompilacji są używane do [kontrolowania zachowania konsolidatora](~/cross-platform/deploy-test/linker.md).


<a name="ProguardConfiguration" />

### <a name="proguardconfiguration"></a>ProguardConfiguration

Pliki z *ProguardConfiguration* akcji kompilacji zawiera opcje, które są używane do sterowania `proguard` zachowanie. Aby uzyskać więcej informacji dotyczących tej akcji kompilacji, zobacz [narzędzia ProGuard](~/android/deploy-test/release-prep/proguard.md).

Pliki te są ignorowane, chyba że `$(EnableProguard)` właściwość MSBuild jest `True`.



## <a name="target-definitions"></a>Definicje docelowego

Specyficzne dla platformy Xamarin.Android części procesu kompilacji są zdefiniowane w `$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets`, ale w normalnych specyficzny dla języka elementów docelowych, takich jak *Microsoft.CSharp.targets* są również wymagane do utworzenia zestawu.

Przed zaimportowaniem obiektów docelowych, język należy ustawić następujące właściwości kompilacji:

```xml
<PropertyGroup>
  <TargetFrameworkIdentifier>MonoDroid</TargetFrameworkIdentifier>
  <MonoDroidVersion>v1.0</MonoDroidVersion>
  <TargetFrameworkVersion>v2.2</TargetFrameworkVersion>
</PropertyGroup>
```

Wszystkie te elementy docelowe i właściwości mogą być uwzględnione w języku C#, importując *Xamarin.Android.CSharp.targets*:

```xml
<Import Project="$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets" />
```

Ten plik można łatwo zaadaptować do innych języków.
