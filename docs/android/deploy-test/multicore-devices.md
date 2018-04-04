---
title: Urządzenia z procesorami wielordzeniowymi & platformy Xamarin.Android
description: Android można uruchamiać na wielu architektur innym komputerze. W tym dokumencie omówiono różne architektury Procesora, które mogą być zastosowane do aplikacji platformy Xamarin.Android. Ten dokument również objaśnia sposób Android aplikacje pakiecie w celu obsługi innej architektury Procesora. Aplikacja interfejsu binarne (ABI) zostaną wprowadzone, a będzie wskazówki dotyczące ABIs, których można użyć w aplikacji platformy Xamarin.Android.
ms.prod: xamarin
ms.assetid: D812883C-A14A-E74B-0F72-E50071E96328
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0288ba6aa8a3c9eb89208161f60ba831723444c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="multi-core-devices--xamarinandroid"></a>Urządzenia z procesorami wielordzeniowymi & platformy Xamarin.Android

_Android można uruchamiać na wielu architektur innym komputerze. W tym dokumencie omówiono różne architektury Procesora, które mogą być zastosowane do aplikacji platformy Xamarin.Android. Ten dokument również objaśnia sposób Android aplikacje pakiecie w celu obsługi innej architektury Procesora. Aplikacja interfejsu binarne (ABI) zostaną wprowadzone, a będzie wskazówki dotyczące ABIs, których można użyć w aplikacji platformy Xamarin.Android._


## <a name="overview"></a>Omówienie

Android umożliwia tworzenie "fat plików binarnych," pojedynczy `.apk` pliku, który zawiera kod maszynowy, która będzie obsługiwać wiele różnych architektury Procesora. Jest to osiągane przez kojarzenie każdego z nich kod maszynowy z *binarny interfejsu aplikacji*. ABI służy do kontrolowania, które kod maszynowy będzie uruchamiany na urządzenia sprzętowego. Na przykład w przypadku aplikacji systemu Android do uruchamiania na x86 urządzenia jest konieczne jest stosowanie x86 ABI obsługuje podczas kompilowania aplikacji.

W szczególności każdej aplikacji systemu Android będzie obsługiwać co najmniej jeden *interfejsu binarnych osadzonego aplikacji* (EABI). EABI są specyficzne dla osadzonych programów Konwencji. Typowy EABI opisano czynności takich jak:

-   Zestaw instrukcji procesora CPU.

-   Kolejności bajtów pamięci przechowuje i ładuje w czasie wykonywania.

-   Format binarny plików obiektu i bibliotek programu, jak również jakiego typu zawartości jest dozwolona lub obsługiwane przez te pliki i bibliotek.

-   Konwencje różnych, używany do przekazywania danych między kodem aplikacji i systemu (na przykład: jak rejestrów i/lub stosu są używane podczas funkcje są nazywane ograniczeń wyrównania itp.)

-   Ograniczenia wielkości i typów wyliczenia, struktury, pól i tablic.

-   Lista symboli funkcji dostępnych w kodzie maszyny w czasie wykonywania, zazwyczaj z bardzo specyficznego zestawu wybranych bibliotek.



### <a name="armeabi-and-thread-safety"></a>armeabi i bezpieczeństwa wątków

Interfejs binarne zostaną dokładniej omówione szczegółowo poniżej, ale należy pamiętać, że `armeabi` jest używany przez platformy Xamarin.Android środowiska uruchomieniowego *bezpieczeństwa wątków*. Jeśli aplikacja, która ma `armeabi` pomocy technicznej jest wdrażana na `armeabi-v7a` urządzenia, nastąpi wiele wyjątków dziwne i unexplainable.

Z powodu błędu w systemie Android 4.0.0, 4.0.1 4.0.2 i 4.0.3 zostanie pobrana natywnych bibliotek z `armeabi` katalog, nawet jeśli nie wystąpi `armeabi-v7a` katalog istnieje i urządzenia jest `armeabi-v7a` urządzenia.

> [!NOTE]
> Xamarin.Android sprawdzi, czy `.so` są dodawane do APK we właściwej kolejności. Ten błąd nie powinien być problemem w przypadku użytkowników platformy Xamarin.Android.


### <a name="abi-descriptions"></a>Opisy ABI

Każdy ABI obsługiwane przez system Android jest identyfikowany przez unikatową nazwę.



#### <a name="armeabi"></a>armeabi

Jest to nazwa EABI opartego na architekturze ARM procesorów CPU, które obsługuje co najmniej ARMv5TE zestaw instrukcji. Android następuje ABI GNU/Linux ARM little endian. Ta ABI nie obsługuje sprzętowa obliczeń zmiennoprzecinkowych. Wszystkie operacje FP są wykonywane przez funkcje pomocnicze oprogramowania, które pochodzą z kompilatora `libgcc.a` biblioteki statycznej. Urządzenia SMP nie są obsługiwane przez `armeabi`.

**Uwaga**: dla platformy Xamarin.Android `armeabi` kod nie jest bezpieczne dla wątków i nie powinien być używany w Wieloprocesorowych `armeabi-v7a`urządzeń (opisanych poniżej). Przy użyciu `aremabi` kod na jednym rdzeniu `armeabi-v7a` urządzenia jest bezpieczne.



#### <a name="armeabi-v7a"></a>armeabi-v7a

Jest to inny zestaw instrukcji Procesora opartego na architekturze ARM rozszerza `armeabi` EABI opisane powyżej. `armeabi-v7a` EABI ma obsługę operacji zmiennoprzecinkowych sprzętu i wielu urządzeń procesora CPU (SMP). Aplikacja, która używa `armeabi-v7a` EABI można spodziewać się znacznej wydajności za pośrednictwem aplikacji, która używa `armeabi`.

**Uwaga:** `armeabi-v7a` kod maszynowy nie będzie uruchamiany na urządzeniach ARMv5.



#### <a name="arm64-v8a"></a>arm64-v8a

Jest to zestaw instrukcji 64-bitowym, który jest oparty na architekturze ARMv8 Procesora. Taka architektura jest używany w *9 węzła*.
Xamarin.Android 5.1 zapewnia obsługę eksperymentalną dla tej architektury (Aby uzyskać więcej informacji, zobacz [eksperymentalną](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Experimental_Features)).



#### <a name="x86"></a>x86

Jest nazwa ABI dla procesorów CPU, które obsługuje zestaw często nazwanego instrukcji *x86* lub *IA-32*. Ten ABI odnosi się do instrukcji dotyczących zestaw instrukcji Pentium Pro, między innymi zestawy instrukcji MMX, SSE, SSE2 i SSE3. Nie obejmuje ona wszelkie inne opcjonalne IA-32 instrukcji set rozszerzenia takich jak:

-  Instrukcja MOVBE.
-  Dodatkowe rozszerzenia SSE3 (instrukcji SSSE3).
-  wszelkie wariant SSE4.


**Uwaga:** Google TV, chociaż jest uruchamiany na x86, nie jest obsługiwana przez firmy Android zestawu NDK lub



#### <a name="x8664"></a>x86_64

Jest nazwa ABI dla procesorów CPU, które obsługują x86 64-bitowy zestaw instrukcji (zwaną także *x64* lub *AMD64*). Xamarin.Android 5.1 zapewnia obsługę eksperymentalną dla tej architektury (Aby uzyskać więcej informacji, zobacz [eksperymentalną](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Experimental_Features)).


#### <a name="mips"></a>MIPS

Jest to nazwa ABI, na podstawie MIPS procesorów CPU, które obsługuje co najmniej `MIPS32r1` zestaw instrukcji. Żadna MIPS 16 ani `micromips` są obsługiwane przez system Android.

**Uwaga:** MIPS urządzenia nie są obecnie obsługiwane przez Xamarin.Android, ale będzie w przyszłej wersji.



#### <a name="apk-file-format"></a>Format pliku APK

Pakiet aplikacji systemu Android to format pliku, który przechowuje wszystkie kodu, zasobów, zasobów i certyfikaty wymagane dla aplikacji systemu Android. Jest `.zip` pliku, ale `.apk` rozszerzenia nazwy pliku. Po rozwinięciu zawartość `.apk` utworzone przez Xamarin.Android widać na poniższym zrzucie ekranu:

[![Zawartość pliku apk](multicore-devices-images/00.png)](multicore-devices-images/00.png#lightbox)

Szybkie opis zawartości `.apk` pliku:

-   **AndroidManifest.xml** &ndash; to `AndroidManifest.xml` pliku w formacie binarnym XML.

-   **Classes.dex** &ndash; zawiera kod aplikacji skompilowany w `dex` formacie, który jest używany przez środowisko wykonawcze systemu Android maszyny Wirtualnej.

-   **Resources.arsc** &ndash; ten plik zawiera wszystkie zasoby wstępnie skompilowana dla aplikacji.

-   **lib** &ndash; ten katalog zawiera skompilowanego kodu dla każdego interfejsu ABI. Będzie zawierać jeden folder dla każdej ABI, który został opisany w poprzedniej sekcji. Zrzut ekranu powyżej `.apk` zagrożona ma natywnej biblioteki dla obu `armeabi-v7a` i `x86` .

-   **META-INF** &ndash; ten katalog (jeśli istnieje) jest używany do przechowywania informacji podpisywania, pakietu i dane konfiguracji rozszerzenia.

-   **res** &ndash; ten katalog zawiera zasoby, które nie zostały skompilowane w `resources.arsc` .

> [!NOTE]
> Plik `libmonodroid.so` jest natywnej biblioteki wymagane przez wszystkie aplikacje platformy Xamarin.Android.



#### <a name="android-device-abi-support"></a>Obsługa interfejsu ABI urządzenia z systemem android

Każde urządzenie z systemem Android obsługuje wykonywanie kodu macierzystego w maksymalnie dwóch ABIs:

-   **ABI "primary"** &ndash; odpowiada kod maszynowy używane w obrazie systemu.

-   **"Secondary" ABI** &ndash; jest opcjonalny ABI, który jest również obsługiwana przez obrazu systemu.


Na przykład typowa urządzenia ARMv5TE będzie miał tylko podstawowy ABI z `armeabi`, gdy urządzenie ARMv7 określić podstawowy ABI z `armeabi-v7a` i dodatkowej ABI z `armeabi`. Typowy x86 urządzenia tylko określić podstawowy ABI z `x86`.


### <a name="android-native-library-installation"></a>Biblioteka systemu android Native instalacji

Podczas instalacji pakietu, natywnych bibliotek w obrębie `.apk` są wyodrębniane do katalogu macierzystego biblioteki aplikacji, zwykle `/data/data/<package-name>/lib`, a następnie są określane jako `$APP/lib`.

Zachowanie podczas instalacji natywnej biblioteki dla systemu android znacznie różni się między wersjami systemu Android.



#### <a name="installing-native-libraries-pre-android-40"></a>Instalowanie natywnych bibliotek: Wstępne Android 4.0

Android starszą niż 4.0 Sandwich lodów tylko wyodrębni natywnych bibliotek pochodzących z *pojedynczy ABI* w `.apk`. Aplikacje dla systemu android z tego zbioru próbują używać najpierw wyodrębnić wszystkich natywnych bibliotek dla podstawowego ABI, a jeśli nie istnieją żadne takich bibliotek, Android następnie Wyodrębnij wszystkie biblioteki macierzystego dla dodatkowej ABI. Nie odbywa się "scalanie".

Rozważmy na przykład sytuacji, gdy aplikacja jest zainstalowana na `armeabi-v7a` urządzenia. `.apk,` Który obsługuje zarówno `armeabi` i `armeabi-v7a`, ma następujące ABI `lib` katalogów i plików:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

Po zakończeniu instalacji będzie zawierać katalog macierzysty biblioteki:

```shell
$APP/lib/libtwo.so # from the armeabi-v7a directory in the apk
```

Innymi słowy, nie `libone.so` jest zainstalowany. Spowoduje to problemów, jako `libone.so` nie ma dla aplikacji do ładowania w czasie wykonywania. To zachowanie podczas nieoczekiwany, zostało zarejestrowane jako usterki i ponownie sklasyfikowane jako "[działa zgodnie z założeniami](http://code.google.com/p/android/issues/detail?id=9089)."

W związku z tym, gdy przeznaczonych dla wersji systemu Android starszą niż 4.0, konieczne jest zapewnienie *wszystkie* natywnych bibliotek *każdego* ABI, która aplikacja będzie obsługiwać, oznacza to, że `.apk` powinien zawierać :

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libone.so
lib/armeabi-v7a/libtwo.so
```


#### <a name="installing-native-libraries-android-40-ndash-android-403"></a>Instalowanie natywnych bibliotek: Systemu Android 4.0 &ndash; systemu Android to 4.0.3

Android 4.0 Sandwich lodów zmienia logiki wyodrębniania. On będzie wyliczyć wszystkich natywnych bibliotek, czy już został wyodrębniony plik nazwę bazową, a jeśli oba poniższe warunki są spełnione, następnie biblioteki zostaną wyodrębnione:

-   Już nie zostały wyodrębnione.

-   ABI natywnej biblioteki zgodny element docelowy ABI podstawowy lub pomocniczy.


Spełniające te warunki umożliwia zachowanie "scalanie"; oznacza to gdy będziemy mieć `.apk` z następującą zawartość:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

Następnie po zakończeniu instalacji, katalog macierzysty biblioteki będzie zawierać:

```shell
$APP/lib/libone.so
$APP/lib/libtwo.so
```

Niestety, to zachowanie jest kolejności zależnych, zgodnie z opisem w następującym dokumencie - [problem 24321: węzła Galaxy 4.0.2 korzysta z kodu natywnego armeabi zarówno armeabi i armeabi v7a jest uwzględniona w apk](http://code.google.com/p/android/issues/detail?id=25321).

Natywnych bibliotek są przetwarzane "w kolejności" (jak te wyświetlane przez, na przykład plików) i *najpierw odpowiada* jest wyodrębniana. Ponieważ `.apk` zawiera `armeabi` i `armeabi-v7a` wersji `libtwo.so`i `armeabi` znajduje się najpierw, jest `armeabi` wersji, która jest wyodrębniany *nie* `armeabi-v7a` Wersja:

```shell
$APP/lib/libone.so # armeabi
$APP/lib/libtwo.so # armeabi, NOT armeabi-v7a!
```

Ponadto, nawet jeśli obie `armeabi` i `armeabi-v7a` podano ABIs (zgodnie z poniższym opisem w sekcji *deklarowanie obsługiwane ABIs*), platformy Xamarin.Android utworzy następujący element w.
`csproj`:

```xml
<AndroidSupportedAbis>armeabi,armeabi-v7a</AndroidSupportedAbis>
```

W rezultacie `armeabi` `libmonodroid.so` znajdują się najpierw w `.apk`i `armeabi` `libmonodroid.so` będzie konto, z którego jest wyodrębniany, nawet jeśli `armeabi-v7a` `libmonodroid.so` istnieje i jest zoptymalizowany do obiekt docelowy. Może to również spowodować zasłoniętej błędów czasu wykonywania, jako `armeabi` nie jest SMP bezpieczne.


##### <a name="installing-native-libraries-android-404-and-later"></a>Instalowanie natywnych bibliotek: Android 4.0.4 i nowsze

Android 4.0.4 zmienia logiki wyodrębniania: go spowoduje wyliczenie wszystkich natywnych bibliotek, nazwę bazową pliku do odczytu, a następnie wyodrębnić głównej wersji ABI (jeśli istnieje) lub dodatkowej ABI (jeśli istnieje). Umożliwia to zachowanie "scalanie"; oznacza to gdy będziemy mieć `.apk` z następującą zawartość:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

Następnie po zakończeniu instalacji, katalog macierzysty biblioteki będzie zawierać:

```shell
$APP/lib/libone.so # from armeabi
$APP/lib/libtwo.so # from armeabi-v7a
```


### <a name="xamarinandroid-and-abis"></a>Platformy Xamarin.Android i ABIs

Xamarin.Android obsługuje architektury następujące:

-  `armeabi`
-  `armeabi-v7a`
-  `x86`

Xamarin.Android zapewnia obsługę eksperymentalną architektury następujące:

-  `arm64-v8a`
-  `x86_64`


Należy zauważyć, że 64-bitowego środowiska uruchomieniowe *nie* wymagane do uruchamiania aplikacji na urządzeniach 64-bitowych. Aby uzyskać więcej informacji na temat eksperymentalną w wersji 5.1 platformy Xamarin.Android, zobacz [eksperymentalną](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Experimental_Features).

Xamarin.Android nie ma obecnie obsługę `mips`.



### <a name="declaring-supported-abis"></a>Deklarowanie obsługiwane przez ABI

Domyślnie zostaną domyślnie Xamarin.Android `armeabi-v7a` dla **wersji** kompilacje i `armeabi-v7a` i `x86` dla **debugowania** kompilacji. Obsługa różnych ABIs można ustawić za pomocą opcji projektu w projekcie platformy Xamarin.Android. W programie Visual Studio, można ją skonfigurować **Android opcje** strony projektu **właściwości**w obszarze **zaawansowane** karcie, jak pokazano na poniższym zrzucie ekranu:

![Właściwości zaawansowane opcje systemu android](multicore-devices-images/vs-abi-selections.png)


W programie Visual Studio dla komputerów Mac obsługiwane architektury może zostać wybrany na **Android kompilacji** strony **opcje projektu**w obszarze **zaawansowane** karcie, jak pokazano w następującym Zrzut ekranu:

[![ABIs obsługiwane kompilacji systemu android](multicore-devices-images/xs-abi-selections-sml.png)](multicore-devices-images/xs-abi-selections.png#lightbox)

Istnieje kilka sytuacji, gdy może być konieczne w celu zadeklarowania dodatkowe wsparcie ABI, takie jak czas:

-   Wdrażanie aplikacji do `x86` urządzenia.

-   Wdrażanie aplikacji do `armeabi-v7a` urządzeniu w celu zapewnienia bezpieczeństwa wątków.



## <a name="summary"></a>Podsumowanie

Ten dokument omówiono różne architektury Procesora, których aplikacja systemu Android mogą być uruchamiane na. Spowodował interfejsu binarne aplikacji i jak jest używana przez system Android do obsługi różnych architektury Procesora.
Następnie przechodził w celu omówienia sposobu określania ABI pomocy technicznej w aplikacji platformy Xamarin.Android i wyróżnione problemów, które powstają w przypadku korzystania z aplikacji platformy Xamarin.Android na `armeabi-v7a` urządzenia, które są przeznaczone tylko dla `armeabi`.


## <a name="related-links"></a>Linki pokrewne

- [Architektura MIPS](http://www.mips.com/products/product-materials/processor/mips-architecture)
- [ABI z architekturą ARM (PDF)](http://infocenter.arm.com/help/topic/com.arm.doc.ihi0036b/IHI0036B_bsabi.pdf)
- [Zestawu NDK systemu android](http://developer.android.com/tools/sdk/ndk/index.html)
- [Wystawiać 9089:Nexus jeden — nie można załadować żadnych natywnych bibliotek z armeabi Jeśli istnieje co najmniej jedną bibliotekę armeabi v7a](http://code.google.com/p/android/issues/detail?id=9089)
- [Problem 24321: Węzła Galaxy 4.0.2 używa kodu natywnego armeabi zarówno armeabi i armeabi v7a jest uwzględniona w apk](http://code.google.com/p/android/issues/detail?id=25321)
