---
title: Proces kompilacji
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2018
ms.openlocfilehash: bf8dfb43115806f28935c6dec0ebd2d6d7bd2cdc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998263"
---
# <a name="build-process"></a>Proces kompilacji


## <a name="overview"></a>Omówienie

Proces kompilacji platformy Xamarin.Android jest odpowiedzialny za łączenia ze wszystkiego razem: [generowania `Resource.designer.cs` ](~/android/internals/api-design.md), pomocnicze `AndroidAsset`, `AndroidResource`i inne [Akcja kompilacji](#Build_Actions), Generowanie [Android wywoływanych otok](~/android/platform/java-integration/android-callable-wrappers.md)oraz generowanie `.apk` do wykonania na urządzeniach z systemem Android.


## <a name="application-packages"></a>Pakiety aplikacji

W warunkach szerokie, istnieją dwa typy pakietów aplikacji systemu Android (`.apk` pliki) który może generować systemu kompilacji platformy Xamarin.Android: 

-   **Wersja** kompilacje, które są całkowicie niezależne i nie wymagają dodatkowych pakietów do wykonania. Są to pakiety, które zostanie dostarczone do sklepu z aplikacjami. 

-   **Debugowanie** kompilacje, które nie są. 

Nie przypadkowo odpowiadały MSBuild `Configuration` wytwarzająca pakietu.

### <a name="shared-runtime"></a>Udostępnione środowisko uruchomieniowe

*Udostępnionego środowiska uruchomieniowego* jest parą dodatkowych pakietów dla systemu Android, które zapewniają Biblioteka klasy podstawowej (`mscorlib.dll`, itp.) i Biblioteka powiązań systemu Android (`Mono.Android.dll`itp.). Debugowanie kompilacji korzystają z udostępnionego środowiska uruchomieniowego audytów Biblioteka klasy podstawowej i zestawy powiązań w pakiecie aplikacji dla systemu Android, dzięki czemu debugowanie pakietu może być mniejszy.

Udostępnione środowisko uruchomieniowe, które mogą być wyłączone w kompilacjach debugowania, ustawiając `$(AndroidUseSharedRuntime)` właściwość `False`. 

<a name="Fast_Deployment" />

### <a name="fast-deployment"></a>Szybkie wdrażanie

*Szybkie wdrażanie* współdziała z udostępnionego środowiska uruchomieniowego, aby dodatkowo zmniejszyć rozmiar pakietu aplikacji dla systemu Android. Odbywa się przez nie kompilowanie zestawów aplikacji w pakiecie. Zamiast tego są one kopiowane na miejsce docelowe za pomocą `adb push`. Ten proces przyspiesza cykl build/wdrażanie/debug, ponieważ jeśli *tylko* zestawy są zmieniane, pakiet nie zostanie ponownie zainstalowany. Zamiast tego należy tylko zaktualizowane zestawy są ponownie zsynchronizowane na urządzeniu docelowym. 

Szybkie wdrażanie wiadomo, że się nie powieść na urządzeniach, które blokują `adb` po nieudanym synchronizowaniu do katalogu `/data/data/@PACKAGE_NAME@/files/.__override__`. 

Szybkie wdrażanie jest domyślnie włączona i mogą być wyłączone w zwiększono wydajność debugowania kompilacji przez ustawienie `$(EmbedAssembliesIntoApk)` właściwość `True`.


## <a name="msbuild-projects"></a>Projekty programu MSBuild

Proces kompilacji platformy Xamarin.Android jest oparty na MSBuild, która jest również formatu pliku projektu, które są używane przez program Visual Studio dla komputerów Mac i Visual Studio.
Zazwyczaj użytkownicy nie muszą ręcznie edytować pliki MSBuild &ndash; IDE tworzy projektów w pełni funkcjonalne i aktualizuje je ze zmianami wprowadzonymi i automatycznie wywoływać obiekty docelowe kompilacji, zgodnie z potrzebami. 

Zaawansowani użytkownicy mogą chcieć wykonać czynności, które nie są obsługiwane przez interfejsu graficznego środowiska IDE, dzięki czemu proces kompilacji nie można dostosować, edytując plik projektu bezpośrednio. Ta strona dokumentów tylko funkcje specyficzne dla platformy Xamarin.Android i dostosowania &ndash; możliwe normalne MSBuild elementy, właściwości i elementy docelowe są wiele innych rzeczy. 

<a name="Build_Targets" />

## <a name="build-targets"></a>Obiekty docelowe kompilacji

Następujące obiekty docelowe kompilacji są zdefiniowane dla projektów platformy Xamarin.Android:

-   **Tworzenie** &ndash; kompilacji pakietu.

-   **Wyczyść** &ndash; usuwa wszystkie pliki generowane przez proces kompilacji.

-   **Zainstaluj** &ndash; pakiet zostanie zainstalowany na urządzeniu domyślnym lub urządzenia wirtualnego.

-   **Odinstaluj** &ndash; odinstalowanie pakietu z domyślne urządzenie lub urządzenia wirtualnego.

-   **SignAndroidPackage** &ndash; tworzy i podpisuje pakiet (`.apk`). Za pomocą `/p:Configuration=Release` do generowania niezależna pakietów "Wersja".

-   **UpdateAndroidResources** &ndash; aktualizacje `Resource.designer.cs` pliku. Ten element docelowy zwykle jest wywoływana przez środowisko IDE, gdy nowe zasoby są dodawane do projektu.


## <a name="build-properties"></a>Właściwości kompilacji

Właściwości programu MSBuild kontrolują zachowanie elementów docelowych. Są wymienione w pliku projektu, np. **MyApp.csproj**, w ramach [MSBuild PropertyGroup — element](https://docs.microsoft.com/visualstudio/msbuild/propertygroup-element-msbuild).

-   **Konfiguracja** &ndash; Określa konfigurację kompilacji, aby użyć, na przykład "Debug" lub "Release". Właściwość konfiguracji jest używana do określenia wartości domyślne dla innych właściwości, które określają zachowanie docelowego. Dodatkowe konfiguracje mogą być utworzone w ramach zintegrowanego środowiska Projektowego.

    *Domyślnie*, `Debug` konfiguracji powoduje `Install` i `SignAndroidPackage` tworzenie mniejszych pakietu systemu Android, która wymaga obecności innych plików i pakietów do działania elementów docelowych.

    Wartość domyślna `Release` konfiguracji powoduje `Install` i `SignAndroidPackage` cele Tworzenie pakietu systemu Android, która jest *autonomicznej*i mogą być używane bez konieczności instalowania innych pakietów lub plików.

-   **DebugSymbols** &ndash; wartość logiczna, która określa, czy pakietu systemu Android jest *debugowania*, w połączeniu z `$(DebugType)` właściwości. Pakiet debugowania zawierają symbole debugowania, zestawy `//application/@android:debuggable` atrybutu `true`i automatycznie dodaje `INTERNET` uprawnienia tak, aby dołączyć debuger do procesu. Aplikacja jest debugowania Jeśli `DebugSymbols` jest `True` *i* `DebugType` jest pustym ciągiem lub `Full`.

-   **DebugType** &ndash; Określa [typu symbole debugowania](https://docs.microsoft.com/visualstudio/msbuild/csc-task) do generowania jako część kompilacji, który ma również wpływ na czy aplikacja jest debugowania. Możliwe wartości:

    - **Pełna**: pełne symbole są generowane. Jeśli `DebugSymbols` właściwość MSBuild jest również `True`, pakiet aplikacji jest debugowania.

    - **PdbOnly**: "PDB" symbole są generowane. Pakiet aplikacji zostanie *nie* były debugowalne.

    Jeśli `DebugType` nie jest ustawiona lub jest pustym ciągiem, a następnie `DebugSymbols` właściwość określa, czy aplikacja jest debugowania.


### <a name="install-properties"></a>Zainstaluj właściwości

Właściwości instalacji sterowania zachowaniem `Install` i `Uninstall` elementów docelowych.

-   **AdbTarget** &ndash; określa urządzenie docelowe dla systemu Android pakietu systemu Android może być zainstalowane do lub usunięte z. Wartość tej właściwości jest taka sama jak [ `adb` opcji urządzenie docelowe](http://developer.android.com/tools/help/adb.html#issuingcommands):

    ```bash
    # Install package onto emulator via -e
    # Use `/Library/Frameworks/Mono.framework/Commands/msbuild` on OS X
    MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
    ```


### <a name="packaging-properties"></a>Właściwości pakowania

Właściwości pakowania kontrolować tworzenie pakietu systemu Android oraz są używane przez `Install` i `SignAndroidPackage` elementów docelowych.
[Właściwości podpisywania](#Signing_Properties) są również istotne podczas pakowania wersji aplikacji.


-   **AndroidApkSigningAlgorithm** &ndash; wartość ciągu, który określa algorytm podpisywania za pomocą `jarsigner -sigalg`.

    Wartość domyślna to `md5withRSA`.

    Dodane w Xamarin.Android 8.2.

-   **AndroidApplication** &ndash; wartość logiczną, wskazującą, czy projekt jest dla aplikacji systemu Android (`True`) lub projekt biblioteki systemu Android (`False` lub brak).

    Tylko jeden projekt za pomocą `<AndroidApplication>True</AndroidApplication>` mogą być obecne w ramach pakietu systemu Android. (Niestety ta nie została jeszcze zweryfikowana, co może spowodować błędy subtelnym, Nieoczekiwana dotyczące zasobów dla systemu Android.)

-   **AndroidBuildApplicationPackage** &ndash; wartość logiczna, która wskazuje, czy należy utworzyć i podpisać pakiet (apk). Ustawienie tej wartości na `True` jest równoważne użyciu [SignAndroidPackage](#Build_Targets) kompilacji docelowej.

    Obsługa dla tej właściwości została dodana po 7.1 platformy Xamarin.Android.

    Ta właściwość jest `False` domyślnie.

-   **AndroidEnableMultiDex** &ndash; właściwość typu boolean, która określa, czy obsługa funkcji Multi-dex będą używane w końcowym `.apk`.

    W 5.1 platformy Xamarin.Android dodano obsługę dla tej właściwości.

    Ta właściwość jest `False` domyślnie.

-   **AndroidEnableSGenConcurrent** &ndash; właściwość typu boolean określającą czy Mono firmy [współbieżnego modułu odzyskiwania pamięci](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen) będą używane.

    Obsługa tej właściwości został dodany w Xamarin.Android 7.2.

    Ta właściwość jest `False` domyślnie.

-   **AndroidErrorOnCustomJavaObject** &ndash; właściwość typu boolean, która określa, czy typy mogą implementować `Android.Runtime.IJavaObject` 
     *bez* również dziedziczenie z `Java.Lang.Object` lub `Java.Lang.Throwable`:

    ```csharp
    class BadType : IJavaObject {
        public IntPtr Handle {
            get {return IntPtr.Zero;}
        }

        public void Dispose()
        {
        }
    }
    ```

    W przypadku wartości True takich typów spowoduje wygenerowanie błędu XA4212, w przeciwnym razie będą generowane ostrzeżenie XA4212.

    W 8.1 platformy Xamarin.Android dodano obsługę dla tej właściwości.

    Ta właściwość jest `True` domyślnie.

-   **AndroidFastDeploymentType** &ndash; A `:` (dwukropek) — można wdrożyć lista rozdzielonych wartości, aby kontrolować, jakie typy [Fast Deployment directory](#Fast_Deployment) na urządzeniu docelowym po `$(EmbedAssembliesIntoApk)` Właściwości MSBuild `False`. Jeśli zasób jest szybko wdrożyć, jest *nie* osadzona w wygenerowanym `.apk`, który można skrócić czas wdrażania. (Więcej, będącego szybko wdrożone, następnie rzadziej `.apk` musi zostać ponownie skompilowany, a proces instalacji może być szybszy.) Prawidłowe wartości to:

    - `Assemblies`: Wdrażanie zestawów aplikacji.

    - `Dexes`: Wdrażanie `.dex` plików, zasoby systemu Android i zasoby dla systemu Android. **Ta wartość może *tylko* można używać na urządzeniach z systemem Android 4.4 lub nowszy (interfejs API — 19).**

    Wartość domyślna to `Assemblies`.

    **Eksperymentalne**. Dodane w Xamarin.Android 6.1.

-   **AndroidApplicationJavaClass** &ndash; Pełna nazwa klasy Java do użycia zamiast `android.app.Application` gdy klasa dziedziczy [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/).

    Ta właściwość jest zwykle ustawiona *innych* właściwości, takie jak `$(AndroidEnableMultiDex)` właściwości programu MSBuild.

    Dodane w Xamarin.Android 6.1.

-   **AndroidHttpClientHandlerType** &ndash; są kontrolowane ustawienia domyślne `System.Net.Http.HttpMessageHandler` implementacji, które będzie używane przez `System.Net.Http.HttpClient` domyślnego konstruktora. Wartość jest nazwa typu kwalifikowanego zestawu `HttpMessageHandler` podklasy odpowiedni do użytku z [ `System.Type.GetType(string)` ](/dotnet/api/system.type.gettype?view=netcore-2.0#System_Type_GetType_System_String_).

    Wartość domyślna to `System.Net.Http.HttpClientHandler, System.Net.Http`.

    To może zostać zastąpiona w celu zamiast tego zawierają `Xamarin.Android.Net.AndroidClientHandler`, która wykorzystuje interfejsy API języka Java dla systemu Android do wykonania żądania sieciowe. Dzięki temu, uzyskiwanie dostępu do protokołu TLS 1.2 w adresach URL podstawowej wersji systemu Android obsługuje protokół TLS 1.2.  
    Tylko Android 5.0 lub nowszy niezawodnie zapewniają obsługę protokołu TLS 1.2, za pomocą języka Java.

    *Uwaga*: Obsługa protokołu TLS 1.2 Jeśli jest wymagana dla systemu Android wersjach wcześniejszych niż 5.0, *lub* Jeśli obsługa protokołu TLS 1.2 jest wymagany w przypadku `System.Net.WebClient` i powiązanych interfejsów API, następnie `$(AndroidTlsProvider)` powinny być używane.

    *Uwaga*: Obsługa ta właściwość działa przez ustawienie [ `XA_HTTP_CLIENT_HANDLER_TYPE` zmiennej środowiskowej](~/android/deploy-test/environment.md).
    A `$XA_HTTP_CLIENT_HANDLER_TYPE` wartość znaleziona w pliku z akcją kompilacji `@(AndroidEnvironment)` mają wyższy priorytet.

    Dodane w Xamarin.Android 6.1.

-   **AndroidTlsProvider** &ndash; wartość ciągu, który określa, który dostawca protokołu TLS powinny być używane w aplikacji. Możliwe wartości to:

    - `btls`: Służy [wytaczania SSL](https://boringssl.googlesource.com/boringssl) za komunikację TLS z [HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.aspx).
      Umożliwia to korzystanie z protokołu TLS 1.2 we wszystkich wersjach systemu Android.

    - `legacy`: Służy historycznych zarządzaną implementację protokołu SSL do interakcji z sieci. To *nie* obsługi protokołu TLS 1.2.

    - `default`: Zezwalaj na *Mono* wybrać domyślnego dostawcę protokołu TLS.
      Jest to równoważne `legacy`, nawet w 7.3 platformy Xamarin.Android.  
      *Uwaga*: Ta wartość jest mało prawdopodobne, które będą wyświetlane na `.csproj` wartości jako IDE "Domyślna" wartość skutkuje *usuwania* z `$(AndroidTlsProvider)` właściwości.

    - Unset / pusty ciąg: 7.1 platformy Xamarin.Android jest to równoważne `legacy`.  
      W 7.3 platformy Xamarin.Android, jest to równoważne `btls`.

    Wartość domyślna to ciąg pusty.

    Dodane w Xamarin.Android 7.1.

-   **AndroidLinkMode** &ndash; Określa, jakiego typu [łączenie](~/android/deploy-test/linker.md) powinno być przeprowadzane w zestawie zawartym w ramach pakietu systemu Android. Używana tylko w projektach aplikacji systemu Android. Wartość domyślna to *SdkOnly*. Prawidłowe wartości to:

    - **Brak**: połączenie nie zostanie podjęta.

    - **SdkOnly**: łączenie będą wykonywane na bibliotek klas bazowych, nie zestawów użytkownika.

    - **Pełna**: łączenie zostanie przeprowadzone bibliotek klas bazowych i zestawów użytkownika. **Uwaga:** Using `AndroidLinkMode` wartość *pełne* często skutkuje uszkodzone aplikacje, szczególnie w przypadku, gdy odbicie jest używany. Należy unikać, chyba że użytkownik *naprawdę* wiedzieć, co robisz.

    ```xml
    <AndroidLinkMode>SdkOnly</AndroidLinkMode>
    ```

-   **AndroidLinkSkip** &ndash; Określa rozdzielaną średnikami (`;`) listę nazw zestawów, bez rozszerzeń plików, zestawów, które nie powinny być połączone. Używany tylko w projektach aplikacji systemu Android.

    ```xml
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
    ```

-   **AndroidManagedSymbols** &ndash; właściwość typu boolean formantów punktami czy sekwencji są generowane, dzięki czemu można wyodrębnić pliku nazwa i wiersza informacji o numerze z `Release` stos danych śledzenia.

    Dodane w Xamarin.Android 6.1.

-   **AndroidManifest** &ndash; Określa nazwę pliku do użycia jako szablon dla aplikacji [ `AndroidManifest.xml` ](~/android/platform/android-manifest.md).
    Podczas kompilacji, niezbędnych wartości zostanie scalona w celu utworzenia rzeczywistych `AndroidManifest.xml`.
    `$(AndroidManifest)` Musi zawierać nazwę pakietu `/manifest/@package` atrybutu.

-   **AndroidSdkBuildToolsVersion** &ndash; pakiet narzędzi kompilacji zestawu SDK systemu Android zawiera **aapt** i **zipalign** narzędzi, między innymi. Jednocześnie można zainstalować wiele różnych wersji pakietu narzędzi do kompilacji. Pakiet narzędzi kompilacji dla pakietu jest realizowane przez sprawdzanie i przy użyciu wersji narzędzia kompilacji "preferowane", jeśli jest obecny; Jeśli jest w wersji "preferowane" *nie* obecne, a następnie jest używany najwyższy pakiet numerów wersji zainstalowanego narzędzia kompilacji.

    `$(AndroidSdkBuildToolsVersion)` Właściwość MSBuild zawiera wersję preferowanego narzędzia kompilacji. System kompilacji platformy Xamarin.Android zawiera wartość domyślna `Xamarin.Android.Common.targets`, a wartość domyślna może zostać zastąpione w pliku projektu, aby wybierz wersję alternatywne narzędzia kompilacji, jeśli (na przykład) najnowsze aapt uległa awarii się podczas poprzedniej wersji aapt jest znany do pracy.

-   **AndroidSupportedAbis** &ndash; właściwość ciągu, który zawiera średnikiem (`;`) — lista interfejsów ABI, które powinny zostać włączone do `.apk`.

    Obsługiwane wartości to:

    -   `armeabi`
    -   `armeabi-v7a`
    -   `x86`
    -   `arm64-v8a`: Wymaga platformy Xamarin.Android, 5.1 i nowszych.
    -   `x86_64`: Wymaga platformy Xamarin.Android, 5.1 i nowszych.

-   **AndroidUseSharedRuntime** &ndash; właściwość logiczna, która jest Określa, czy *udostępnione pakiety środowiska uruchomieniowego* są wymagane do uruchomienia aplikacji na urządzeniu docelowym. Opierając się na pakiety udostępnionego środowiska uruchomieniowego umożliwia pakietu aplikacji ma być mniejsze, przyspieszanie procesu tworzenia i wdrażania pakietów, skutkuje szybszy cyklu przetwarzania kompilacji/wdrażanie/debug.

    Ta właściwość powinna być `True` w przypadku kompilacji debugowania i `False` dla wersji projektów.

-   **AotAssemblies** &ndash; właściwość typu boolean, która określa, czy zestawy będą Ahead-of-Time kompilowane do kodu macierzystego i uwzględniane w `.apk`.

    W 5.1 platformy Xamarin.Android dodano obsługę dla tej właściwości.

    Ta właściwość jest `False` domyślnie.

-   **EmbedAssembliesIntoApk** &ndash; właściwość typu boolean, która określa, czy zestawy aplikacji powinien być osadzony w pakiecie aplikacji.

    Ta właściwość powinna być `True` dla kompilacji oficjalnych i `False` w przypadku kompilacji debugowania. Jego *może* muszą być `True` w kompilacjach debugowania, jeśli Fast Deployment nie obsługuje urządzenia docelowego.

    Gdy ta właściwość jest `False`, a następnie `$(AndroidFastDeploymentType)` właściwość MSBuild Określa, jakie zostaną osadzone w `.apk`, które mogą mieć wpływ na wdrożenie i Odbuduj razy.

-   **EnableLLVM** &ndash; właściwość typu boolean, która określa, czy maszyny wirtualnej niskiego poziomu będzie używana podczas Ahead of Time kompilowanie zestawów do kodu natywnego.

    W 5.1 platformy Xamarin.Android dodano obsługę dla tej właściwości.

    Ta właściwość jest `False` domyślnie.

    Ta właściwość jest ignorowana, chyba że `$(AotAssemblies)` właściwość MSBuild jest `True`.

-   **EnableProguard** &ndash; właściwość typu boolean określającą czy [proguard](http://developer.android.com/tools/help/proguard.html) jest uruchamiany jako część procesu tworzenia pakietów do łączenia kodu języka Java.

    W 5.1 platformy Xamarin.Android dodano obsługę dla tej właściwości.

    Ta właściwość jest `False` domyślnie.

    Gdy `True`, [ProguardConfiguration](#ProguardConfiguration) pliki będą używane do sterowania `proguard` wykonywania.

-   **JavaMaximumHeapSize** &ndash; określa wartość **java** 
     `-Xmx` wartość parametru do użycia podczas tworzenia `.dex` pliku w ramach procesu tworzenia pakietów. Jeśli nie zostanie określony, a następnie `-Xmx` nie podano opcję **java**.

    Określenie tej właściwości jest konieczne, jeśli [ `_CompileDex` docelowe zgłasza `java.lang.OutOfMemoryError` ](https://bugzilla.xamarin.com/show_bug.cgi?id=18327).

    ```xml
    <JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
    ```

-   **JavaOptions** &ndash; określa dodatkowe opcje wiersza polecenia do przekazania do **java** podczas kompilowania `.dex` pliku.

-   **MandroidI18n** &ndash; określa Obsługa internacjonalizacji dołączona do aplikacji, takich jak sortowania i sortowania tabel. Wartość jest listę rozdzielonych przecinkami lub średnikami, co najmniej jednego z następujących wartości bez uwzględniania wielkości liter:

    -   **Brak**: obejmują nie dodatkowe kodowania.

    -   **Wszystkie**: obejmują wszystkie dostępne kodowania.

    -   **CJK**: obejmują chińskim, japońskim i koreańskim kodowania, takie jak *japoński (EUC)* \[kodera jp, CP51932\], *japoński (Shift-JIS)* \[ shift ISO-2022-jp\_jis, CP932\], *japoński (JIS)* \[CP50220\], *chiński uproszczony (GB2312)* \[gb2312, CP936\], *koreański (UHC)* \[osy\_c\_CP949 5601-1987\], *koreański (EUC)* \[euc-kr, CP51949\], *chiński tradycyjny (Big5)* \[big5, CP950\], i *chiński uproszczony (GB18030)* \[ GB18030, CP54936\].

    -   **MidEast**: kodowania obejmują bliski-Wschodniej, takich jak *turecki (Windows)* \[iso-8859-9, CP1254\], *hebrajski (Windows)* \[ System Windows-1255, CP1255\], *arabski (Windows)* \[1256 system windows, CP1256\], *arabski (ISO)* \[iso-8859-6, CP28596\], *Hebrajski (ISO)* \[iso-8859-8 CP28598\], *Latin-5 (ISO)* \[iso-8859-9, CP28599\]i *Hebrajski (Iso alternatywy)* \[iso-8859-8 CP38598\].

    -   **Inne**: obejmują innego kodowania, takie jak *cyrylica (Windows)* \[CP1251\], *Bałtycki (Windows)* \[iso-8859-4 CP1257\], *Wietnamski (Windows)* \[CP1258\], *Cyrilice (KOI8-R)* \[koi8-r, CP1251\], *ukraiński (KOI8-U)*  \[koi8-u, CP1251\], *Bałtycki (ISO)* \[iso-8859-4 CP1257\], *Cyrilice (ISO)* \[iso-8859-5, CP1251\], *ISCII Davenagari* \[x-iscii-de CP57002\], *ISCII bengalski* \[x-iscii-be, CP57003 \], *ISCII tamilski* \[x-iscii — ta, CP57004\], *ISCII Telugu* \[x-iscii — te CP57005\], *ISCII assamski* \[x-iscii — jako CP57006\], *ISCII orija* \[x-iscii — lub, CP57007\], *ISCII Kannada* \[x-iscii-ka CP57008\], *ISCII malajalam* \[x-iscii-ma CP57009\], *ISCII Gudżarati* \[x-iscii-gu CP57010\], *ISCII pendżabski* \[x-iscii-pa CP57011\], i *tajski (Windows)*  \[CP874\].

    -   **Rzadkie**: obejmują rzadkich kodowania, takich jak *IBM EBCDIC (turecki)* \[CP1026\], *IBM EBCDIC (Otwórz systemów Latin 1)* \[CP1047\], *IBM EBCDIC (USA Kanada z EUR)* \[CP1140\], *IBM EBCDIC (Niemcy z EUR)* \[CP1141\], *IBM EBCDIC (Dania/Norwegia z EUR)* \[CP1142\], *IBM EBCDIC (Finlandia/Szwecja z EUR)* \[CP1143\], *IBM EBCDIC (Włochy z EUR)* \[CP1144\], *IBM EBCDIC (Ameryka Łacińska/Hiszpania z EUR)* \[CP1145\], *IBM EBCDIC (Zjednoczone Królestwo z EUR)* \[CP1146\], *IBM EBCDIC (Francja z EUR)* \[CP1147\], *IBM EBCDIC (międzynarodowy z EUR)*  \[CP1148\], *IBM EBCDIC (islandzki z EUR)* \[CP1149\], *IBM EBCDIC (Niemcy)* \[ CP20273\], *IBM EBCDIC (Dania/Norwegia)* \[CP20277\], *IBM EBCDIC (Finlandia/Szwecja)* \[CP20278\], *IBM EBCDIC (Włochy)* \[CP20280\], *IBM EBCDIC (Ameryka Łacińska/Hiszpania)* \[CP20284\], *IBM EBCDIC (Zjednoczone Królestwo)* \[CP20285\], *IBM EBCDIC (japoński Katakana rozszerzony)* \[CP20290\], *IBM EBCDIC (Francja)* \[CP20297\], *IBM EBCDIC (arabski)* \[CP20420\], *IBM EBCDIC (wersja hebrajska)* \[CP20424\], *IBM EBCDIC (islandzki)* \[ CP20871\], *IBM EBCDIC (cyrylica — serbski, bułgarski)* \[CP21025\], *IBM EBCDIC (USA Kanada)* \[ CP37\], *IBM EBCDIC (międzynarodowy)* \[CP500\], *arabski (ASMO 708)* \[CP708\], *Central European (DOS)* \[CP852\]*, Cyrillic (DOS)* \[CP855\], *Turkish (DOS)* \[CP857\], *Western European (DOS with Euro)* \[CP858\], *Hebrew (DOS)* \[CP862\], *Arabic (DOS)* \[CP864\], *Russian (DOS)* \[CP866\], *Greek (DOS)* \[CP869\], *IBM EBCDIC (Latin 2)* \[CP870\], and *IBM EBCDIC (Greek)* \[CP875\].

    -   **Zachód**: obejmują zachodni kodowania, takie jak *Zachodnioeuropejski (Mac)* \[macintosh, CP10000\], *Islandzki (Mac)* \[x-mac islandzki CP10079\], *centralnej Środkowoeuropejski (Windows)* \[iso-8859-2, CP1250\], *Zachodnioeuropejski (Windows)* \[iso-8859-1 CP1252\], *Grecki (Windows)* \[iso-8859-7, CP1253\], *centralnej Środkowoeuropejski (ISO)* \[iso-8859-2, CP28592\], *3 alfabetu łacińskiego (ISO)* \[iso-8859-3, CP28593\], *Grecki (ISO)* \[iso-8859-7, CP28597\], *Łaciński 9 (ISO)*  \[iso-8859-15, CP28605\], *OEM-USA* \[CP437\], *zachodni Środkowoeuropejski (DOS)* \[CP850\], *portugalski (DOS)* \[CP860\], *Islandzki (DOS)* \[CP861\],  *Francuski kanadyjski (DOS)* \[CP863\], i *Nordycki (DOS)* \[CP865\].


    ```xml
    <MandroidI18n>West</MandroidI18n>
    ```

-   **MonoSymbolArchive** &ndash; właściwość typu boolean, który kontroluje, czy `.mSYM` artefakty są tworzone do późniejszego użycia z `mono-symbolicate`, w celu wyodrębnienia &ldquo;rzeczywistych&rdquo; nazwę pliku i wierszu numer informacji z Ślady stosu wydania.

    Wartość True domyślnie &ldquo;wersji&rdquo; aplikacje, które mają włączone symboli debugowania: `$(EmbedAssembliesIntoApk)` ma wartość PRAWDA, `$(DebugSymbols)` ma wartość PRAWDA, i `$(Optimize)` ma wartość True.

    Dodane w Xamarin.Android 7.1.

-   **AndroidVersionCodePattern** &ndash; właściwość ciągu, który umożliwia deweloperom dostosowywanie `versionCode` w manifeście.
    Zobacz [tworzenie kodu w wersji dla zestawu APK](~/android/deploy-test/building-apps/abi-specific-apks.md) uzyskać informacji na temat ustawiania `versionCode`.
    
    Kilka przykładów, jeśli `abi` jest `armeabi` i `versionCode` w manifeście jest `123`, `{abi}{versionCode}` dadzą znaków versionCode elementu `1123` podczas `$(AndroidCreatePackagePerAbi)` ma wartość True, w przeciwnym razie będzie mieć wartość równą 123.
    Jeśli `abi` jest `x86_64` i `versionCode` w manifeście jest `44`. Dzięki temu można osiągnąć `544` podczas `$(AndroidCreatePackagePerAbi)` ma wartość True, w przeciwnym razie zostanie uzyskiwania wartości `44`.

    Jeśli Lewe dopełnienie ciąg formatu zawiera `{abi}{versionCode:0000}`, jego dałby w efekcie `50044` ponieważ są pozostawiane, wypełnienie `versionCode` z `0`. Alternatywnie można użyć przecinka dopełnienie, takie jak `{abi}{versionCode:D4}` której działa tak samo, jak w poprzednim przykładzie.

    Tylko "0" i "Dx' dopełnienie format ciągów są obsługiwane, ponieważ wartość musi być liczbą całkowitą.
    
    Wstępnie zdefiniowane kluczowych elementach

    -   **ABI** &ndash; wstawia docelowych abi dla aplikacji  
        -   1 &ndash; `armeabi`
        -   2 &ndash; `armeabi-v7a`
        -   3 &ndash; `x86`
        -   4 &ndash; `arm64-v8a`
        -   5 &ndash; `x86_64`

    -   **minSDK** &ndash; wstawia minimalna obsługiwana wartość zestawu Sdk z `AndroidManifest.xml` lub `11` Jeśli brak jest zdefiniowany.  

    -   **znaków versionCode** &ndash; używa wersji kodu bezpośrednio z `Properties\AndroidManifest.xml`. 

    Można zdefiniować niestandardowe elementy przy użyciu `$(AndroidVersionCodeProperties)` właściwości (zdefiniowany dalej).

    Domyślnie wartość zostanie ustawiona na `{abi}{versionCode:D6}`. Jeśli Deweloper chce, aby zachować stare zachowanie można zastąpić domyślną, ustawiając `$(AndroidUseLegacyVersionCode)` właściwości `true`

    Dodane w Xamarin.Android 7.2.

-   **AndroidVersionCodeProperties** &ndash; właściwość ciągu, który umożliwia deweloperom Definiowanie niestandardowych elementów za pomocą `AndroidVersionCodePattern`. Są one w formie `key=value` pary. Wszystkie elementy w `value` powinny być liczbami całkowitymi. Na przykład: `screen=23;target=$(_SupportedApiLevel)`. Jak widać, możesz skorzystać z istniejących lub niestandardowy program MSBuild właściwości w ciągu.

    Dodane w Xamarin.Android 7.2.

-   **AndroidUseLegacyVersionCode** &ndash; właściwość typu boolean będzie umożliwia deweloperowi przywrócić obliczeń znaków versionCode do jego pre stare zachowanie Xamarin.Android 8.2. To można stosować tylko dla deweloperów z istniejącymi aplikacjami w Google Play Store. Zdecydowanie zalecane jest nowym `$(AndroidVersionCodePattern)` właściwość jest używana.

    Dodane w Xamarin.Android 8.2.

-  **AndroidUseManagedDesignTimeResourceGenerator** &ndash; właściwość typu boolean, która spowoduje przełączenie w czasie projektowania opiera się na korzystanie z analizatora zasobów zarządzanych zamiast `aapt`.

    Dodane w Xamarin.Android 8.1.

-  **AndroidUseApkSigner** &ndash; właściwości wartości logicznej, która umożliwia deweloperom stosowanie do `apksigner` narzędzia zamiast `jarsigner`.

    Dodane w Xamarin.Android 8.2.

-  **AndroidApkSignerAdditionalArguments** &ndash; właściwość ciągu, który umożliwia deweloperom podać dodatkowe argumenty `apksigner` narzędzia.

    Dodane w Xamarin.Android 8.2.

### <a name="binding-project-build-properties"></a>Wiązanie właściwości kompilacji projektu

Następujące właściwości programu MSBuild są używane wraz z [projekty powiązań](~/android/platform/binding-java-library/index.md):

-   **AndroidClassParser** &ndash; właściwość ciągu, który kontroluje sposób `.jar` pliki są parsowane. Możliwe wartości:

    - **Klasa analizy**: używa `class-parse.exe` bezpośrednio analizować kodu bajtowego Java bez pomocy maszyny JVM. Ta wartość jest eksperymentalne. 


    - **jar2xml**: Użyj `jar2xml.jar` używać odbicia Java do wyodrębnienia typów i elementów członkowskich z `.jar` pliku.

    Korzyści wynikające z `class-parse` za pośrednictwem `jar2xml` są:

    - `class-parse` jest możliwość wyodrębniania nazw parametrów z kodu bajtowego języka Java, która zawiera *debugowania* symbole (np. kodu bajtowego skompilowanego z `javac -g`).

    - `class-parse` nie "pomija" klas, które dziedziczą z lub zawierają ich składowe typów nierozpoznawalne.

    **Eksperymentalne**. Xamarin.Android dodano w wersji 6.0.

    Wartość domyślna to `jar2xml`.

    Wartość domyślna zmienia się w przyszłej wersji.

-   **AndroidCodegenTarget** &ndash; właściwość ciągu, który kontroluje cel generowania kodu interfejsu ABI. Możliwe wartości:

    - **XamarinAndroid**: używa interfejsu API powiązania interfejsem JNI, które są obecne w od platformy Mono dla systemu Android w wersji 1.0. Zestawy powiązań utworzone za pomocą platformy Xamarin.Android w wersji 5.0 lub nowszym można tylko uruchamiać na platformy Xamarin.Android 5.0 lub nowszy (dodatki interfejsu API/ABI), ale *źródła* jest zgodny z wcześniejszych wersji produktu.

    - **XAJavaInterop1**: Użyj Java.Interop dla wywołania interfejsem JNI. Powiązania zestawów przy użyciu `XAJavaInterop1` można tylko tworzenie i wykonywanie, za pomocą platformy Xamarin.Android 6.1 lub nowszej. 6.1 platformy Xamarin.Android i nowszych powiązania `Mono.Android.dll` o tej wartości.

      Korzyści wynikające z `XAJavaInterop1` obejmują:

      - Mniejsze zestawy.

      - `jmethodID` buforowanie `base` wywołań metody opisywanego tak długo, jak wszystkie inne powiązanie typów w hierarchii dziedziczenia są tworzone za pomocą `XAJavaInterop1` lub nowszej.

      - `jmethodID` buforowanie konstruktory podklasy zarządzanych otoka wywoływana z języka Java.

    Wartość domyślna to `XamarinAndroid`.

    Wartość domyślna zmienia się w przyszłej wersji.


### <a name="resource-properties"></a>Właściwości zasobu

Właściwości zasobu kontrolować Generowanie `Resource.designer.cs` pliku, który zapewnia dostęp do zasobów systemu Android. 

-   **AndroidResgenExtraArgs** &ndash; określa dodatkowe opcje wiersza polecenia do przekazania do **aapt** polecenia podczas przetwarzania elementów zawartości systemu Android i zasobów.

-   **AndroidResgenFile** &ndash; Określa nazwę pliku zasobu do wygenerowania. Domyślny szablon ustawia to `Resource.designer.cs`.

-   **MonoAndroidResourcePrefix** &ndash; Określa *prefiks ścieżki* , zostanie usunięta z początku nazwy plików z akcją kompilacji `AndroidResource`. Ma to na zmianę, gdzie znajdują się zasoby.

    Wartość domyślna to `Resources`. Aby zmienić `res` struktura projektu dla języka Java.

-   **AndroidExplicitCrunch** &ndash; Jeśli tworzysz aplikację z bardzo dużej liczby lokalnych drawables początkowej kompilacji (lub ponowną kompilację) może zająć minut. Aby przyspieszyć proces kompilacji, spróbuj łącznie z tej właściwości, a ustawienie `True`. Gdy ta właściwość jest ustawiona, proces kompilacji crunches wstępnie pliki PNG.

    **Eksperymentalne**. Xamarin.Android dodano w wersji 7.0.


<a name="Signing_Properties" />

### <a name="signing-properties"></a>Podpisywanie właściwości

Podpisywanie właściwości kontrolują, jak pakiet aplikacji jest podpisany, dzięki czemu mogą być zainstalowane na urządzeniu z systemem Android. Aby umożliwić szybsze iteracji kompilacji, zadania platformy Xamarin.Android nie podpiszesz pakietów podczas procesu kompilacji, ponieważ podpisywanie jest dość wolnego. Zamiast tego są podpisane (Jeśli wymagane) przed rozpoczęciem instalacji lub w trakcie eksportowania, IDE lub *zainstalować* kompilacji docelowej. Wywoływanie *SignAndroidPackage* docelowej powoduje wygenerowanie pakietu o `-Signed.apk` sufiks w katalogu wyjściowym.

Domyślnie podpisywania docelowej generuje nowy klucz podpisywania debugowania, jeśli to konieczne. Jeśli chcesz użyć określonego klucza, na przykład na serwerze kompilacji, następujące właściwości programu MSBuild można:

-   **AndroidKeyStore** &ndash; wartość logiczna, która wskazuje, czy można używać niestandardowych informacji dotyczących podpisywania. Wartość domyślna to `False`, co oznacza, że domyślny klucz podpisywania debugowania będzie służyć do podpisywania pakietów.

-   **AndroidSigningKeyAlias** &ndash; określa alias klucza w magazynie kluczy. Jest to **keytool-alias** wartość używaną podczas tworzenia magazynu kluczy. 

-   **AndroidSigningKeyPass** &ndash; Określa hasło klucza w pliku magazynu kluczy. Jest to wartość wprowadzona, kiedy `keytool` prosi **wprowadź hasło klucza dla $(AndroidSigningKeyAlias)**.

-   **AndroidSigningKeyStore** &ndash; Określa nazwę pliku magazynu kluczy utworzonych przez `keytool`. Odpowiada to wartości dostarczone **keytool - magazynu kluczy** opcji.

-   **AndroidSigningStorePass** &ndash; Określa hasło do `$(AndroidSigningKeyStore)`. Jest to wartość udostępniane `keytool` podczas tworzenia pliku magazynu kluczy i zadawane **wprowadź hasło magazynu keystore:**. 

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

Aby korzystać z magazynu kluczy, wygenerowany powyżej, należy użyć grupy właściwości:

```xml
<PropertyGroup>
    <AndroidKeyStore>True</AndroidKeyStore>
    <AndroidSigningKeyStore>filename.keystore</AndroidSigningKeyStore>
    <AndroidSigningStorePass>keystore.filename password</AndroidSigningStorePass>
    <AndroidSigningKeyAlias>keystore.alias</AndroidSigningKeyAlias>
    <AndroidSigningKeyPass>keystore.alias password</AndroidSigningKeyPass>
</PropertyGroup>
```

-   **AndroidDebugKeyAlgorithm** &ndash; Określa domyślny algorytm na potrzeby `debug.keystore`. Jego wartość domyślna to `RSA`.

-   **AndroidDebugKeyValidity** &ndash; określa ważność domyślny do użycia dla `debug.keystore`. Jego wartość domyślna to `10950` lub `30 * 365` lub `30 years`.

<a name="Build_Actions" />

## <a name="build-actions"></a>Akcja kompilacji

*Akcja kompilacji* są [stosowane do plików](https://docs.microsoft.com/visualstudio/msbuild/common-msbuild-project-items) w ramach projektu i kontroli, jak przebiega przetwarzanie pliku. 

<a name="AndroidEnvironment" />

### <a name="androidenvironment"></a>AndroidEnvironment

Pliki z akcją kompilacji `AndroidEnvironment` są używane do [zainicjować zmienne środowiskowe i właściwości systemu podczas uruchamiania procesu](~/android/deploy-test/environment.md).
`AndroidEnvironment` Akcji kompilacji można stosować do wielu plików, a zostanie obliczone w losowej kolejności (tak, aby określać tej samej zmiennej lub system właściwości środowiska w wielu plikach).


### <a name="androidjavasource"></a>AndroidJavaSource

Pliki z akcją kompilacji `AndroidJavaSource` są kod źródłowy Java, które zostaną uwzględnione w pakiecie końcowym dla systemu Android.


### <a name="androidjavalibrary"></a>AndroidJavaLibrary

Pliki z akcją kompilacji `AndroidJavaLibrary` są archiwa Java ( `.jar` pliki) które zostaną uwzględnione w ostatnim pakietu systemu Android.


### <a name="androidresource"></a>AndroidResource

Wszystkie pliki z *AndroidResource* akcji kompilacji są kompilowane do zasobów systemu Android w procesie kompilacji i udostępniane za pośrednictwem `$(AndroidResgenFile)`.

```xml
<ItemGroup>
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
```

Bardziej zaawansowanych użytkowników, może być może chcesz, aby różne zasoby używane w różnych konfiguracjach, ale pod tą samą ścieżką skuteczne. Można to osiągnąć przez posiadanie wielu katalogów zasobów mających pliki z tymi samymi względnymi ścieżkami w ramach tych różnych katalogach i stosując warunki MSBuild warunkowo obejmujący różnych plików w różnych konfiguracjach. Na przykład:

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

**LogicalName** &ndash; jawnie określa ścieżki zasobu. Umożliwia &ldquo;aliasów&rdquo; plików dzięki czemu będą one dostępne jako wiele nazw zasobów distinct.

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

[Natywnych bibliotek](~/android/platform/native-libraries.md) są dodawane do kompilacji przez ustawienie ich akcję kompilacji na `AndroidNativeLibrary`.

Należy pamiętać, że od systemu Android obsługuje wiele interfejsów binarnym aplikacji (interfejsów ABI), system kompilacji musi znać które ABI bibliotekę natywną została stworzona z myślą. Istnieją dwa sposoby, które można to zrobić:

1.  Ścieżka "wykrywania".
2.  Za pomocą `Abi` atrybut elementu.

Przy użyciu ścieżki wykrywanie, nazwę katalogu nadrzędnego bibliotekę natywną służy do określania interfejsu ABI, elementy docelowe biblioteki. W związku z tym jeśli dodasz `lib/armeabi/libfoo.so` do kompilacji, następnie ABI będzie mieć "ten" jako `armeabi`. 


#### <a name="item-attribute-name"></a>Nazwa atrybutu elementu

**ABI** &ndash; określa ABI natywnej biblioteki.

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```


### <a name="androidaarlibrary"></a>AndroidAarLibrary

Akcja kompilacji `AndroidAarLibrary` powinna służyć do bezpośredniego odwołania .aar plików. Ta akcja kompilacji będzie najczęściej używane przez składniki platformy Xamarin. To znaczy uwzględniał odwołania do plików .aar, które są wymagane do sklepu Google Play i innych usług, w pracy.

Z tą kompilacją akcji będą traktowane w podobny sposób zbyt zasoby osadzone znaleźć plików w projektach biblioteki. .aar będą wyodrębniane do katalogu, pośrednich. Wszystkie zasoby, a następnie zostaną uwzględnione pliki zasobów i JAR w grupach odpowiedni element.  

### <a name="content"></a>Zawartość

Normalną `Content` akcji kompilacji nie jest obsługiwana (jak firma Microsoft nie wiesz, jak obsługiwać go bez kosztownych prawdopodobnie kroku pierwszego uruchomienia).

Począwszy od platformy Xamarin.Android, 5.1, próba użycia `@(Content)` akcji kompilacji będzie skutkować `XA0101` ostrzeżenie.

### <a name="linkdescription"></a>LinkDescription

Pliki z *LinkDescription* akcji kompilacji są używane do [kontrolować zachowanie konsolidatora](~/cross-platform/deploy-test/linker.md).


<a name="ProguardConfiguration" />

### <a name="proguardconfiguration"></a>ProguardConfiguration

Pliki z *ProguardConfiguration* akcji kompilacji zawiera opcje, które są używane do sterowania `proguard` zachowanie. Aby uzyskać więcej informacji dotyczących tej akcji kompilacji, zobacz [narzędzia ProGuard](~/android/deploy-test/release-prep/proguard.md).

Te pliki są ignorowane, chyba że `$(EnableProguard)` właściwość MSBuild jest `True`.


## <a name="target-definitions"></a>Definicje docelowej

Specyficzne dla platformy Xamarin.Android części procesu kompilacji są zdefiniowane w `$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets`, ale normalne cele specyficzny dla języka, takich jak *Microsoft.CSharp.targets* również są wymagane do utworzenia zestawu.

Następujące właściwości kompilacji musi być ustawione przed zaimportowaniem wszystkie elementy docelowe języka:

```xml
<PropertyGroup>
  <TargetFrameworkIdentifier>MonoDroid</TargetFrameworkIdentifier>
  <MonoDroidVersion>v1.0</MonoDroidVersion>
  <TargetFrameworkVersion>v2.2</TargetFrameworkVersion>
</PropertyGroup>
```

Wszystkie te cele i właściwości można uwzględnić dla języka C#, importując *Xamarin.Android.CSharp.targets*: 

```xml
<Import Project="$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets" />
```

Ten plik można łatwo dostosować w przypadku pozostałych języków.
