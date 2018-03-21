---
title: Przygotowywanie aplikacji dla wersji
ms.topic: article
ms.prod: xamarin
ms.assetid: 9C8145B3-FCF1-4649-8C6A-49672DDA4159
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/19/2018
ms.openlocfilehash: e26c855133d0b32676aa2d0a6084754a80055e30
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2018
---
# <a name="preparing-an-application-for-release"></a>Przygotowywanie aplikacji dla wersji


Po ukończeniu kodowany i przetestować aplikację, jest potrzebne do przygotowania pakietu do dystrybucji. Pierwszym zadaniem w celu przygotowania tego pakietu jest do skompilowania aplikacji dla wersji głównie pociąga za sobą Ustawianie atrybutów niektórych aplikacji.

Aby utworzyć aplikację dla wersji, wykonaj następujące kroki:

-   **[Określ ikona aplikacji](#Specify_the_Application_Icon)**  &ndash; aplikacji platformy Xamarin.Android każdy powinien mieć określony ikonę aplikacji. Chociaż technicznie niekonieczne, niektóre rynkach, takich jak Google Play wymagają go.

-   **[Wersja aplikacji](#Versioning)**  &ndash; ten krok obejmuje inicjowania lub aktualizowanie informacji o wersji. Jest to ważne dla aplikacji przyszłych aktualizacji i upewnij się, że użytkownicy są pamiętać o zainstalowanie wersji aplikacji.

-   **[Zmniejszanie plik APK](#shrink_apk)**  &ndash; rozmiaru końcowego APK można znacznie zmniejszyć za pomocą konsolidatora Xamarin.Android kodu zarządzanego i narzędzia ProGuard na kodu bajtowego Java.

-   **[Ochrona aplikacji](#protect_app)**  &ndash; uniemożliwić użytkownikom lub osoby atakujące debugowania, zmanipulowaniem lub odtwarzanie aplikacji przez wyłączenie debugowania, obfuscating kod zarządzany, dodając przed debugowania i przed wykrycie i przy użyciu natywnej kompilacji.

-   **[Ustaw właściwości pakowania](#Set_Packaging_Properties)**  &ndash; pakowania właściwości formantu tworzenia pakietu aplikacji systemu Android (APK). Ten krok optymalizuje plik APK chroni jej zasoby i modularizes opakowywanie zgodnie z potrzebami.

-   **[Kompiluj](#Compile)**  &ndash; ten krok kompiluje kod i zasoby, aby sprawdzić zbudował w trybie wersji.

-   **[Archiwum publikowania](#archive)**  &ndash; tego kroku kompilacji aplikacji i umieszcza je w archiwum do podpisywania i publikowania.

Każdy z tych kroków poniżej opisano bardziej szczegółowo.

<a name="Specify_the_Application_Icon" />

## <a name="specify-the-application-icon"></a>Określ ikona aplikacji

Zdecydowanie zaleca się, że każda aplikacja platformy Xamarin.Android Określanie ikony aplikacji. Rynków niektórych aplikacji nie zezwoli aplikacji systemu Android do opublikowania bez ani jednego. `Icon` Właściwość `Application` atrybut służy do określania ikona aplikacji dla projektu platformy Xamarin.Android.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W programie Visual Studio 2015 lub nowszego oraz określić ikona aplikacji za pośrednictwem **manifestu systemu Android** sekcji projektu **właściwości**, jak pokazano na poniższym zrzucie ekranu:

[![Ustaw ikona aplikacji](images/vs/01-application-icon-sml.png)](images/vs/01-application-icon.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W programie Visual Studio dla komputerów Mac, jest również możliwe określenie ikony aplikacji za pośrednictwem **aplikacji systemu Android** sekcji **opcje projektu**, jak pokazano na poniższym zrzucie ekranu:

[![Ustaw ikona aplikacji](images/xs/01-application-icon-sml.png)](images/xs/01-application-icon.png#lightbox)

-----

W tych przykładach `@drawable/icon` odwołuje się do pliku ikony, która znajduje się pod adresem **Resources/drawable/icon.png** (należy pamiętać, że **.png** rozszerzenia nie jest uwzględniony w nazwie zasobów). Ten atrybut również mogą być deklarowane w pliku **Properties\AssemblyInfo.cs**, jak pokazano w ta Wstawka kodu próbki:

```csharp
[assembly: Application(Icon = "@drawable/icon")]
```

<a name="Versioning" />

## <a name="version-the-application"></a>Wersja aplikacji

Przechowywanie wersji jest ważne dla aplikacji systemu Android konserwacji i dystrybucji. Bez zainicjowania przechowywanie wersji w miejscu trudno jest określić, czy jest lub sposób aktualizowania aplikacji. Aby ułatwić zarządzanie wersjami, Android rozpoznaje dwóch różnych typów informacji: 

-   **Numer wersji** &ndash; wartość całkowitą (używana wewnętrznie przez system Android oraz aplikacja) oznacza wersję aplikacji. Większość aplikacji uruchamiane z wartością ustawioną wartość 1, a następnie jest zwiększany dla każdej kompilacji. Ta wartość nie ma relacji ani koligacji z wersji atrybutem nazwy (patrz poniżej). Aplikacje i usługi publikowania powinien nie wyświetlaj tej wartości dla użytkowników. Ta wartość jest przechowywana w **AndroidManifest.xml** pliku jako `android:versionCode`. 

-   **Nazwa wersji** &ndash; ciąg, który jest używany tylko w przypadku przekazywania informacji do użytkownika o wersji aplikacji (zgodnie z zainstalowanym określonym urządzeniem). Nazwa wersji jest przeznaczony do wyświetlania dla użytkowników lub w witrynie Google Play. Ten ciąg nie jest używana wewnętrznie przez system Android. Nazwa wersji może być dowolną wartością ciągu, który pomoże zidentyfikować kompilacji, która jest zainstalowana na urządzeniu użytkownika. Ta wartość jest przechowywana w **AndroidManifest.xml** pliku jako `android:versionName`. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W programie Visual Studio, te wartości można ustawić w **manifestu systemu Android** sekcji projektu **właściwości**, jak pokazano na poniższym zrzucie ekranu:

[![Ustaw numer wersji](images/vs/02-versioning-sml.png)](images/vs/02-versioning.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Te wartości można ustawić za pomocą **kompilacji > aplikacji systemu Android** sekcji **opcje projektu** jak pokazano na poniższym zrzucie ekranu:

[![Ustaw numer wersji](images/xs/02-versioning-sml.png)](images/xs/02-versioning.png#lightbox)

-----

<a name="shrink_apk" />

## <a name="shrink-the-apk"></a>Zmniejszanie plik APK

Xamarin.Android APKs można nawiązać mniejszym przy użyciu kombinacji konsolidator Xamarin.Android, co spowoduje usunięcie niepotrzebnych *zarządzane* kodu i *narzędzia ProGuard* narzędzi z zestawu SDK systemu Android, co spowoduje usunięcie nieużywane *kodu bajtowego Java*. Proces kompilacji najpierw używa konsolidator Xamarin.Android w celu optymalizowania aplikacji na poziomie kodu zarządzanego (C#), a następnie go później używa narzędzia ProGuard (jeśli jest włączona) w celu zoptymalizowania APK na poziomie kodu bajtowego Java.


### <a name="configure-the-linker"></a>Skonfiguruj konsolidator

Tryb wersji wyłącza udostępnionego środowiska uruchomieniowego i włącza łączenie tak, aby aplikacja jest dostarczany tylko elementy platformy Xamarin.Android wymagane w czasie wykonywania. *Konsolidatora* platformie Xamarin.Android używa analizy statycznej w celu określenia, które zestawy, typy i elementy członkowskie typu są używane lub odwołuje się do aplikacji platformy Xamarin.Android. Konsolidator odrzuca wszystkie nieużywane zestawy, typy i elementy członkowskie, które nie są używane (lub odwołania). Może to spowodować znaczne obniżenie zużycia rozmiar pakietu. Rozważmy na przykład [HelloWorld](~/android/deploy-test/linker.md) próbki, które napotyka 83% zmniejszenie rozmiaru końcowego jego APK: 

-   Konfiguracja: Brak &ndash; rozmiar Xamarin.Android 4.2.5 = 17.4 MB.

-   Konfiguracja: Tylko zestawy SDK &ndash; rozmiar Xamarin.Android 4.2.5 = 3,0 MB.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ustawianie opcji konsolidatora za pośrednictwem **Android opcje** sekcji projektu **właściwości**:

[![Opcje konsolidatora](images/vs/03-linking-sml.png)](images/vs/03-linking.png#lightbox)

**Konsolidację** menu rozwijanego udostępnia następujące opcje umożliwiające sterowanie konsolidator:

-   **Brak** &ndash; to wyłączenie konsolidator; bez konsolidacji zostanie wykonane.

-   **Tylko zestawy SDK** &ndash; tylko połączy zestawy, które są [wymagane przez platformy Xamarin.Android](~/cross-platform/internals/available-assemblies.md). 
    Inne zestawy nie zostaną połączone.

-   **Zestaw SDK i zestawów użytkownika** &ndash; połączy wszystkie zestawy, które są wymagane przez aplikację, a nie tylko te, które są wymagane przez platformy Xamarin.Android.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Ustawianie opcji konsolidatora za pośrednictwem **konsolidatora** karcie **Android kompilacji** sekcji **opcje projektu**, jak pokazano na poniższym zrzucie ekranu:

[![Opcje konsolidatora](images/xs/03-linking-sml.png)](images/xs/03-linking.png#lightbox)

Dostępne są następujące opcje umożliwiające sterowanie konsolidator:

-   **Nie zawierają łącza** &ndash; to wyłączenie konsolidator; bez konsolidacji zostanie wykonane.

-   **Połącz tylko zestawy SDK** &ndash; tylko połączy zestawy, które są [wymagane przez platformy Xamarin.Android](~/cross-platform/internals/available-assemblies.md). Inne zestawy nie zostaną połączone.

-   **Połącz wszystkie zestawy** &ndash; połączy wszystkie zestawy, które są wymagane przez aplikację, a nie tylko te, które są wymagane przez platformy Xamarin.Android.

-----

Konsolidacja może spowodować niektórych niezamierzone skutki uboczne, należy pamiętać, że aplikację można ponownie przetestowane w trybie wersji na urządzeniu fizycznym.


### <a name="proguard"></a>ProGuard

*ProGuard* to narzędzie Android SDK prowadzący i zaciemnia kodu języka Java. Narzędzia ProGuard zwykle jest używana do tworzenia aplikacji mniejszych przez ograniczenie rozmiaru dużych dołączone bibliotek (takich jak usług Google Play) w Twojej APK. Narzędzia ProGuard Usuwa nieużywane kodu bajtowego Java, dzięki czemu wynikowy aplikacji mniejsze. Na przykład za pomocą narzędzia ProGuard na małych aplikacji platformy Xamarin.Android zwykle osiąga o 24% zmniejszenia rozmiaru &ndash; za pomocą narzędzia ProGuard na większych aplikacji za pomocą wielu zależności biblioteki zwykle osiąga jeszcze bardziej znaczące zmniejszenie rozmiaru. 

ProGuard jest nie alternatywę do konsolidatora platformy Xamarin.Android. Łącza konsolidatora Xamarin.Android *zarządzane* kodu podczas ProGuard łącza kodu bajtowego Java. Proces kompilacji najpierw wykorzystuje konsolidator Xamarin.Android do optymalizacji zarządzanego kodu (C#) w aplikacji, a następnie go później używa narzędzia ProGuard (jeśli jest włączona) w celu zoptymalizowania APK na poziomie kodu bajtowego Java. 

Gdy **włączyć ProGuard** zaznaczono Xamarin.Android uruchamia narzędzie ProGuard na wynikowej APK. Plik konfiguracji ProGuard jest generowanie i używanie narzędzia ProGuard podczas kompilacji. Xamarin.Android obsługuje również niestandardowe *ProguardConfiguration* akcji. Można dodać plik konfiguracji niestandardowej ProGuard do projektu, kliknij go prawym przyciskiem myszy i wybierz ją jako akcja kompilacji, jak pokazano w poniższym przykładzie: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Akcja proguard kompilacji](images/vs/05-proguard-build-action-sml.png)](images/vs/05-proguard-build-action.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Akcja proguard kompilacji](images/xs/05-proguard-build-action-sml.png)](images/xs/05-proguard-build-action.png#lightbox)

-----

Narzędzia ProGuard jest domyślnie wyłączona. **Włączyć ProGuard** opcja jest dostępna tylko wtedy, gdy projekt jest ustawiona na **wersji** tryb. Wszystkie akcje ProGuard kompilacji są ignorowane, chyba że **włączyć ProGuard** jest zaznaczony. Konfiguracja platformy Xamarin.Android ProGuard nie zasłaniają plik APK, a nie jest możliwe włączyć zaciemnienie, nawet w przypadku plików konfiguracji niestandardowej. Jeśli chcesz użyć zaciemnienie, zobacz [ochrona aplikacji z Dotfuscator](~/android/deploy-test/release-prep/index.md#dotfuscator). 

Aby uzyskać szczegółowe informacje dotyczące korzystania z narzędzia ProGuard, zobacz [narzędzia ProGuard](~/android/deploy-test/release-prep/proguard.md).

<a name="protect_app" />

## <a name="protect-the-application"></a>Ochrona aplikacji

<a name="Disable_Debugging" />

### <a name="disable-debugging"></a>Wyłącz debugowanie

Podczas tworzenia aplikacji systemu Android, debugowanie jest wykonywane przy użyciu *protokół debugowania języka Java* (JDWP). Jest to technologia umożliwiająca przełączanie narzędzi, takich jak **adb** do komunikowania się z maszyny wirtualnej Java na potrzeby debugowania. JDWP jest włączona domyślnie w przypadku kompilacji debugowania aplikacji platformy Xamarin.Android. Podczas JDWP odgrywa ważną rolę w czasie projektowania, go może stanowić zagrożenie bezpieczeństwa dla opublikowane aplikacje. 

> [!IMPORTANT]
> Zawsze należy wyłączyć stan debugowania w wydanej aplikacji, ponieważ jest możliwe (za pośrednictwem JDWP) do uzyskania pełnego dostępu do procesu Java i wykonania dowolnego kodu w kontekście aplikacji, jeśli ten stan debugowania nie jest wyłączone.

Manifestu systemu Android zawiera `android:debuggable` atrybut, który kontroluje, czy można debugować aplikacji. Jest on uznawany za dobrą praktyką jest ustawiony `android:debuggable` atrybutu `false`. Najprostszym sposobem, aby to zrobić to przez dodanie instrukcji warunkowej kompilacji w **AssemblyInfo.cs**: 

```csharp
#if DEBUG
[assembly: Application(Debuggable=true)]
#else
[assembly: Application(Debuggable=false)]
#endif
```

Należy pamiętać, że kompilacje debugowania automatycznie Ustaw niektóre uprawnienia, aby łatwiej debugowania (takich jak **Internet** i **ReadExternalStorage**). Uprawnienia, które jawnie skonfigurować można jednak użyć w kompilacjach wydania. Jeśli okaże się, że przełączanie do kompilacji wydania powoduje utratę uprawnienia, które były dostępne w kompilacji debugowania aplikacji, sprawdź, czy zostały jawnie włączona tego uprawnienia w **wymagane uprawnienia** listy zgodnie z opisem w [ Uprawnienia](~/android/app-fundamentals/permissions.md). 
 

<a name="dotfuscator" id="dotfuscator" />

### <a name="application-protection-with-dotfuscator"></a>Ochrona aplikacji z Dotfuscator

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Nawet w przypadku [debugowania wyłączone](#Disable_Debugging), jest nadal możliwe osobom atakującym ponownego tworzenia pakietów aplikacji, dodawanie lub usuwanie opcji konfiguracji lub uprawnieniami. Dzięki temu ich odtwarzanie, debugowania i odporne na próby z aplikacją.
[Dotfuscator Community Edition (CE)](https://www.preemptive.com/products/dotfuscator/overview) może służyć do zasłaniają kodu zarządzanego i Wstaw kod wykrywania stan zabezpieczeń środowiska uruchomieniowego do aplikacji platformy Xamarin.Android podczas kompilacji.

Dotfuscator CE jest dołączony do programu Visual Studio, jednak tylko Visual Studio 2015 Update 3 (i nowszych) ma poprawną wersję do pracy z platformy Xamarin.Android. Aby użyć Dotfuscator, kliknij **Narzędzia > cenią sobie wcześniejsze ochrony - Dotfuscator**.

Aby skonfigurować Dotfuscator CE, zobacz [przy użyciu Dotfuscator Community Edition za pomocą platformy Xamarin](https://www.preemptive.com/obfuscating-xamarin-with-dotfuscator).
Po skonfigurowaniu, Dotfuscator CE będą automatycznie chronić każdej kompilacji, która jest tworzona.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Nawet w przypadku [debugowania wyłączone](#Disable_Debugging), jest nadal możliwe osobom atakującym ponownego tworzenia pakietów aplikacji, dodawanie lub usuwanie opcji konfiguracji lub uprawnieniami. Dzięki temu ich odtwarzanie, debugowania i odporne na próby z aplikacją.
Chociaż nie obsługuje programu Visual Studio dla komputerów Mac, można użyć [Dotfuscator Community Edition (CE)](https://www.preemptive.com/products/dotfuscator/overview) z programem Visual Studio zasłaniają kodu zarządzanego i Wstaw kod wykrywania stan zabezpieczeń środowiska uruchomieniowego do aplikacji platformy Xamarin.Android w czasie kompilacji .

Aby skonfigurować Dotfuscator CE, zobacz [przy użyciu Dotfuscator Community Edition za pomocą platformy Xamarin](https://www.preemptive.com/obfuscating-xamarin-with-dotfuscator).
Po skonfigurowaniu, Dotfuscator CE będą automatycznie chronić każdej kompilacji, która jest tworzona.

-----

<a name="bundle" />

### <a name="bundle-assemblies-into-native-code"></a>Zestawy pakietu kodu natywnego

Gdy ta opcja jest włączona, zestawy są połączone w natywnej biblioteki udostępnionej. Tej opcji powoduje kod bezpieczne. chroni zarządzanych zestawów osadzić je w natywne pliki binarne.

Ta opcja wymaga licencji Enterprise i jest dostępna tylko podczas **Użyj szybkiego wdrożenia** jest wyłączona. **Zestawy kodu natywnego pakietu** jest domyślnie wyłączona.

Należy pamiętać, że **pakietu kodu natywnego** tych opcji *nie* oznacza, że zestawy są kompilowane kodu natywnego. Nie jest możliwe użycie [ **kompilacji drzewa obiektów aplikacji** ](#aot) skompilować zestawów do kodu macierzystego (obecnie tylko eksperymentalna funkcji, a nie w środowisku produkcyjnym za pomocą).

<a name="aot" />

### <a name="aot-compilation"></a>Kompilacja drzewa obiektów aplikacji

**Kompilacji drzewa obiektów aplikacji** opcji (na [właściwości pakowania](#Set_Packaging_Properties) strony) włącza kompilację zestawy Ahead of Time (drzewa obiektów aplikacji). Gdy ta opcja jest włączona, narzut na uruchamianie tylko w czasie (JIT) jest zminimalizowany przez wstępnej kompilacji zestawy przed uruchomieniem. Wynikowy kod macierzysty znajduje się w APK wraz z nie skompilowanego zestawów. Powoduje to w krótszym czasie uruchomienia aplikacji, ale kosztem nieco większe rozmiary APK.

**Kompilacji drzewa obiektów aplikacji** opcja wymaga licencji Enterprise lub nowszej. **Kompilacja drzewa obiektów aplikacji** jest dostępna, tylko gdy projekt jest skonfigurowany dla trybu wersji i jest domyślnie wyłączona. Aby uzyskać więcej informacji o kompilacji drzewa obiektów aplikacji, zobacz [drzewa obiektów aplikacji](http://www.mono-project.com/docs/advanced/aot/).


#### <a name="llvm-optimizing-compiler"></a>LLVM optymalizacji kompilatora

_LLVM optymalizacji kompilatora_ utworzyć mniejsze i szybciej skompilowanego kodu i przekonwertować skompilowany drzewa obiektów aplikacji zestawów do kodu macierzystego, ale w expense z mniejszą kompilacji razy. Kompilator LLVM jest domyślnie wyłączona. Aby użyć kompilatora LLVM **kompilacji drzewa obiektów aplikacji** najpierw należy włączyć opcję (na [właściwości pakowania](#Set_Packaging_Properties) strony).


> [!NOTE]
> **LLVM optymalizacji kompilatora** opcja wymaga licencji Business.  

<a name="Set_Packaging_Properties" />

## <a name="set-packaging-properties"></a>Ustaw właściwości pakowania

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Właściwości tworzenia pakietów można ustawiać **Android opcje** sekcji projektu **właściwości**, jak pokazano na poniższym zrzucie ekranu:

[![Właściwości pakowania](images/vs/04-packaging-sml.png)](images/vs/04-packaging.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Właściwości tworzenia pakietów można ustawiać **opcje projektu**, jak pokazano na poniższym zrzucie ekranu:

[![Właściwości pakowania](images/xs/04-packaging-sml.png)](images/xs/04-packaging.png#lightbox)

-----

Wiele z tych właściwości, takie jak **Użyj udostępnionych w czasie wykonywania**, i **Użyj szybkiego wdrożenia** są przeznaczone do trybu debugowania. Gdy aplikacja jest skonfigurowana pod kątem trybu wersji, istnieją inne ustawienia, które określają sposób aplikacji [zoptymalizowane pod kątem rozmiaru i wykonywania szybkość](#shrink_apk), [jak jest chronione przed naruszeniami](#protect_app)i w jaki sposób mogą być pakowane do obsługi różnych architektur i ograniczenia rozmiaru.


### <a name="specify-supported-architectures"></a>Określ obsługiwane architektury

Przygotowywanie aplikacji platformy Xamarin.Android dla wersji, należy określić architektury Procesora, które są obsługiwane. Pojedynczy APK może zawierać kod maszynowy do obsługi wielu różnych architektur. Zobacz [architektury Procesora](~/android/app-fundamentals/cpu-architectures.md) szczegółowe informacje dotyczące obsługi wielu architektury Procesora.


### <a name="generate-one-package-apk-per-selected-abi"></a>Generowanie jeden pakiet (. APK) na wybranych ABI

Gdy ta opcja jest włączona, co APK zostaną utworzone dla każdej z obsługiwanych ABI (wybranego na **zaawansowane** karcie, zgodnie z opisem w [architektury Procesora](~/android/app-fundamentals/cpu-architectures.md)) zamiast jednej, duża APK dla wszystkich obsługiwanych ABI firmy. Ta opcja jest dostępna tylko wtedy, gdy projekt jest skonfigurowany dla trybu wersji i jest domyślnie wyłączona.


### <a name="multi-dex"></a>Multi-Dex

Gdy **włączyć wielu Dex** jest włączona opcja, zestaw SDK systemu Android narzędzia są używane do obejścia 65 K limit — metoda **.dex** format pliku. Ograniczenia metody 65K opiera się na wiele metod Java który aplikacji _odwołania_ (włącznie z zawartymi żadnych bibliotek, które zależy od aplikacji) &ndash; nie jest oparty na liczbę metod, które są _zapisany Kod źródłowy_. Jeśli aplikacja tylko definiuje kilka metod ale używa wielu (lub dużej biblioteki), jest możliwe, że zostanie przekroczony limit 65K.

Istnieje możliwość, że aplikacja nie używa każdej metody w każdej biblioteki, do którego odwołuje się; w związku z tym jest to możliwe, że narzędzia, takiego jak narzędzia ProGuard (zobacz powyżej) można usunąć nieużywane metody z kodu. Najlepszym rozwiązaniem jest umożliwienie **włączyć wielu Dex** tylko jeśli jest to bezwzględnie konieczne i.e.the aplikacja nadal odwołuje się do więcej niż 65 metody K Java nawet po użyciu narzędzia ProGuard.

Aby uzyskać więcej informacji na temat wielu Dex, zobacz [Konfigurowanie aplikacji za pomocą za pośrednictwem 64 K metody](http://developer.android.com/tools/building/multidex.html).

<a name="Compile" />

## <a name="compile"></a>Kompilacji

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po ukończeniu wszystkich powyższych kroków, aplikacja jest gotowa do kompilacji. Wybierz **kompilacji > Kompiluj ponownie rozwiązanie** Aby sprawdzić, czy pomyślnie kompilacje w trybie wersji. Należy pamiętać, że ten krok nie jeszcze tworzy APK.

[Podpisywanie pakietu aplikacji](~/android/deploy-test/signing/index.md) omówiono pakowanie i podpisywanie bardziej szczegółowo.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po ukończeniu wszystkich powyższych kroków Kompilowanie aplikacji (wybierz **kompilacji > kompilacji wszystkich**) można zweryfikować, że pomyślnie kompilacje w trybie wersji. Należy pamiętać, że ten krok nie jeszcze tworzy APK.

-----


<a name="archive" />

## <a name="archive-for-publishing"></a>Archiwum do publikowania

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby rozpocząć proces publikowania, kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **archiwum...**  elementu menu kontekstowego:

[![Archiwum aplikacji](images/vs/07-archive-for-publishing-sml.png)](images/vs/07-archive-for-publishing.png#lightbox)

**Archiwum...**  uruchamia **Menedżera archiwum** i rozpocznie się proces archiwizacji pakietu aplikacji, jak pokazano w tym zrzut ekranu:

[![Menedżer archiwum](images/vs/08-archive-manager-sml.png)](images/vs/08-archive-manager.png#lightbox)

Innym sposobem tworzenia archiwum jest kliknij prawym przyciskiem myszy rozwiązanie w **Eksploratora rozwiązań** i wybierz **archiwum wszystkich...** , który tworzy rozwiązania i archiwa wszystkich projektów Xamarin, które mogą generować archiwum:

[![Wszystkie archiwum](images/vs/09-archive-all-sml.png)](images/vs/09-archive-all.png#lightbox)


Zarówno **archiwum** i **archiwum wszystkich** automatyczne uruchomienie **Menedżera archiwum**. Aby uruchomić **Menedżera archiwum** bezpośrednio, kliknij przycisk **Narzędzia > archiwum Manager...**  element menu:

[![Uruchom Menedżera archiwum](images/vs/10-launch-archive-manager-sml.png)](images/vs/10-launch-archive-manager.png#lightbox)

Archiwa rozwiązania w dowolnym momencie, klikając prawym przyciskiem myszy **rozwiązania** węzła i wybierając **archiwa widoku**:

[![Archiwa widoku](images/vs/11-view-archives-sml.png)](images/vs/11-view-archives.png#lightbox)

### <a name="the-archive-manager"></a>Menedżer archiwum

**Menedżera archiwum** składa się z **listy rozwiązań** okienku **listy archiwa**, a **Panel szczegółów**:

[![Okienka Menedżera archiwum](images/vs/12-archive-manager-detail-sml.png)](images/vs/12-archive-manager-detail.png#lightbox)

**Listy rozwiązań** Wyświetla wszystkie rozwiązania, o co najmniej jeden projekt zarchiwizowane. **Listy rozwiązań** zawiera następujące sekcje:

* **Bieżące rozwiązanie** &ndash; Wyświetla bieżące rozwiązanie. Należy pamiętać, że ten obszar może być pusta, jeśli bieżące rozwiązanie nie ma istniejącego archiwum.
* **Archiwa wszystkich** &ndash; Wyświetla wszystkie rozwiązania, które mają archiwum.
* **Wyszukiwanie** pola tekstowego (u góry) &ndash; rozwiązań na liście filtrów **archiwa wszystkich** listę według wprowadzony w polu tekstowym ciąg do wyszukania.

**Archiwa listy** Wyświetla listę wszystkich archiwa wybranego rozwiązania. **Listy archiwa** zawiera następujące sekcje:

* **Nazwa wybranego rozwiązania** &ndash; Wyświetla nazwę wybranego w rozwiązaniu **listy rozwiązań**. Wszystkie informacje wyświetlone w **listy archiwa** odwołuje się do tego wybranego rozwiązania.
* **Filtr platform** &ndash; to pole umożliwia filtrowanie archiwa przez typ platformy (na przykład iOS lub Android).
* **Archiwizowanie elementów** &ndash; listy archiwa wybranego rozwiązania. Każdy element na liście zawiera nazwę projektu, Data utworzenia i platformy. Dodatkowe informacje, takie jak postęp może również pokazać, gdy element jest w trakcie archiwizacji lub opublikowane.

**Panel szczegółów** wyświetlane dodatkowe informacje o każdym archiwum. Pozwala użytkownikowi na uruchamianie przepływu pracy dystrybucji lub Otwórz folder, w którym utworzono dystrybucji. **Kompilacji komentarze** sekcji sprawiają uwzględnić komentarze kompilacji w archiwum.

### <a name="distribution"></a>Dystrybucji

Gdy zarchiwizowaną wersję aplikacji jest gotowa do opublikowania, wybierz archiwum w **Menedżera archiwum** i kliknij przycisk **dystrybucji...**  przycisk:

[![Przycisk Dystrybuuj](images/vs/13-distribute-sml.png)](images/vs/13-distribute.png#lightbox)

**Kanału dystrybucji** okna dialogowego pokazuje informacje o aplikacji, ze wskazaniem dystrybucji postępu przepływu pracy i wybór kanałów dystrybucji. Przy pierwszym uruchomieniu dostępne są dwie opcje:

[![Wybierz kanał dystrybucji](images/vs/14-distribution-channel-sml.png)](images/vs/14-distribution-channel.png#lightbox)

Użytkownik może wybrać jedną z następujących kanałów dystrybucji:

* **Ad Hoc** &ndash; podpisem APK jest zapisywany na dysku, które mogą być ładowane bezpośrednio do urządzenia z systemem Android. Nadal [podpisywanie pakietu aplikacji](~/android/deploy-test/signing/index.md) Aby dowiedzieć się, jak utworzyć Android tożsamość podpisywania, Utwórz nowy certyfikat podpisywania dla aplikacji systemu Android i opublikować _ad hoc_ wersji aplikacji na dysku. Jest to dobry sposób tworzenia APK do testowania.

* **Google Play** &ndash; publikuje podpisem APK do witryny Google Play. Nadal [publikowania do witryny Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md) Aby dowiedzieć się, jak utworzyć i opublikować APK w sklepie Google Play.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby rozpocząć proces publikowania, wybierz **kompilacji > archiwum publikowania**:

[![Archiwum do publikowania](images/xs/07-archive-for-publishing-sml.png)](images/xs/07-archive-for-publishing.png#lightbox)

**Archiwum publikowania** kompilacji projektu i zawiera on również do pliku archiwum. **Archiwum wszystkich** menu wyboru archiwa wszystkich można utworzyć archiwum projektów w rozwiązaniu. Obie te opcje automatycznego otwierania **Menedżera archiwum** po zakończeniu operacji kompilacji i paczki:

[![Wyświetl archiwum](images/xs/08-archives-view-sml.png)](images/xs/08-archives-view.png#lightbox)

W tym przykładzie **Menedżera archiwum** zawiera tylko jedną zarchiwizowane aplikacji, **moja_aplikacja**. Należy zauważyć, że pole Komentarz umożliwia krótki komentarz do zapisania z archiwum. Aby opublikować zarchiwizowaną wersję aplikacji platformy Xamarin.Android, wybierz aplikację w **Menedżera archiwum** i kliknij przycisk **logowania i rozpowszechnij...**  jak pokazano powyżej. Powstałe w ten sposób **logowania i rozpowszechnij** okna dialogowego przedstawiono dwie opcje:

[![Zaloguj się i dystrybucji](images/xs/09-sign-and-distribute-sml.png)](images/xs/09-sign-and-distribute.png#lightbox)


W tym miejscu jest możliwe, wybierz kanał dystrybucji:

-   **Ad Hoc** &ndash; zapisuje podpisem APK na dysku, dzięki czemu mogą być ładowane bezpośrednio do urządzenia z systemem Android. Nadal [podpisywanie pakietu aplikacji](~/android/deploy-test/signing/index.md) Aby dowiedzieć się, jak utworzyć Android tożsamość podpisywania, Utwórz nowy certyfikat podpisywania dla aplikacji systemu Android i opublikować &ldquo;ad hoc&rdquo; wersji aplikacji na dysku. Jest to dobry sposób tworzenia APK do testowania.


-   **Google Play** &ndash; publikuje podpisem APK do witryny Google Play.
    Nadal [publikowania do witryny Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md) Aby dowiedzieć się, jak utworzyć i opublikować APK w sklepie Google Play.

-----


## <a name="related-links"></a>Linki pokrewne

- [Urządzenia z procesorami wielordzeniowymi i platformy Xamarin.Android](~/android/deploy-test/multicore-devices.md)
- [Architektury procesorów](~/android/app-fundamentals/cpu-architectures.md)
- [AOT](http://www.mono-project.com/docs/advanced/aot/)
- [Zmniejszanie kod i zasoby](http://developer.android.com/tools/help/proguard.html)
- [Konfigurowanie aplikacji za pomocą metod ponad 64 K](http://developer.android.com/tools/building/multidex.html)
