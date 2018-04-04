---
title: Tworzenie APKs specyficzne dla interfejsu ABI
description: Ten dokument zostanie omówiono sposób tworzenia APK, który będzie współpracować z jednym ABI, przy użyciu platformy Xamarin.Android.
ms.prod: xamarin
ms.assetid: D21B195B-4530-4EB2-8704-5C4349A2CDD8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 89a78c8dd1243dcfea9d14bd9758c5403d1d21ef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="building-abi-specific-apks"></a>Tworzenie APKs specyficzne dla interfejsu ABI

_Ten dokument zostanie omówiono sposób tworzenia APK, który będzie współpracować z jednym ABI, przy użyciu platformy Xamarin.Android._



## <a name="overview"></a>Omówienie

W niektórych sytuacjach może być korzystne dla aplikacji, aby mieć wiele APKs — każdego APK jest podpisany za pomocą tego samego magazynu kluczy i udziałów tej samej nazwy pakietu, ale jest on skompilowany dla określonego urządzenia lub konfiguracji systemu Android. Nie jest to zalecane podejście — jest znacznie prostsza do mają jeden APK, który może obsługiwać wiele urządzeń i konfiguracji. Brak niektórych sytuacjach tworzenia wielu APKs mogą być przydatne, takich jak:

-  **Zmniejsz rozmiar plik APK** — limit rozmiaru 100 MB na plikach APK nakłada Google Play. Tworzenie urządzenia określonego APK można zmniejszyć rozmiar plik APK potrzebnych do dostarczania podzbiór zasobów i zasobów dla aplikacji.

-  **Obsługuje różne architektury Procesora** — Jeśli aplikacja udostępnił biblioteki dla określonego Procesora, można rozpowszechniać tylko biblioteki udostępnionej dla tego Procesora.


Wiele APKs skomplikować dystrybucji — problem, którego dotyczy Google Play. Google Play zapewni, że poprawne APK jest dostarczany do urządzenia, na podstawie kodu wersji aplikacji i innych metadanych z **AndroidManifest.XML**. Aby uzyskać szczegółowe informacje i ograniczenia dotyczące jak Google Play obsługuje wiele APKs dla aplikacji, zapoznaj się [dokumentacji firmy Google na obsługę wielu APK](http://developer.android.com/google/play/publishing/multiple-apks.html).

W tym przewodniku dotyczą skryptu tworzenie wielu APKs dla aplikacji platformy Xamarin.Android, każdy APK przeznaczonych dla określonego interfejsu ABI. Obejmuje on następujące tematy:

1.  Utworzyć unikatową *kod wersji* dla plik APK.
1.  Utwórz tymczasową wersję **AndroidManifest.XML** który będzie używany dla tego APK.
1.  Tworzenie aplikacji przy użyciu **AndroidManifest.XML** z poprzedniego kroku.
1.  Przygotuj plik APK w wersji podpisywania i zip wyrównywania go.


Na końcu tego przewodnika jest wskazówki, które będą pokazują, jak te czynności przy użyciu skryptów do [nachylenia](http://martinfowler.com/articles/rake.html).



### <a name="creating-the-version-code-for-the-apk"></a>Tworzenie kod wersji dla plik APK

Google zaleca konkretnego algorytmu kod wersji, który korzysta z kodu w wersji 7 cyfr (można znaleźć w sekcji *przy użyciu schematu kodu wersji* w [dokument pomocy technicznej w wielu APK](http://developer.android.com/google/play/publishing/multiple-apks.html)).
Rozszerzanie schematu kod tej wersji do ośmiu cyfr, jest możliwy do uwzględnienia niektórych informacji ABI na kod wersji, który sprawdzi, czy Google Play roześle poprawne APK na urządzeniu. Poniżej opisano to osiem formatu kodu wersji cyfrę (indeksowane od lewej do prawej):

-   **Indeks 0** (kolor czerwony na diagramie poniżej) &ndash; całkowitą dla ABI:
    -   1 &ndash; `armeabi`
    -   2 &ndash; `armeabi-v7a`
    -   6 &ndash; `x86`

-   **Indeks 1 i 2** (pomarańczowy na diagramie poniżej) &ndash; minimalny poziom interfejsu API, obsługiwane przez aplikację.

-   **Indeks 3 — 4** (niebieski na diagramie poniżej) &ndash; rozmiaru ekranu obsługiwane:
    -   1 &ndash; małe
    -   2 &ndash; normalne
    -   3 &ndash; duże
    -   4 &ndash; xlarge

-   **Indeks 5-7** (kolor zielony na diagramie poniżej) &ndash; unikatowy numer wersji kodu. 
    Zostało to określone przez dewelopera. Należy go zwiększyć do każdej publicznej wersji aplikacji.

Na poniższym diagramie przedstawiono pozycja każdego kodu opisane w powyższej listy:

[![Diagram w wersji 8 cyfrowy kod format, kodowane przez kolor](abi-specific-apks-images/image00.png)](abi-specific-apks-images/image00.png#lightbox)


Google Play zapewni, że poprawne APK jest dostarczany do urządzenia, na podstawie `versionCode` i APK konfiguracji. APK najwyższy kod wersji będą dostarczane do urządzenia. Na przykład aplikacja może mieć trzy APKs o następujących kodach wersji:

-  11413456 jest ABI `armeabi` ; poziom interfejsu API wskazuje 14, mały dla dużych ekranów; 456 numer wersji.
-  21423456 jest ABI `armeabi-v7a` ; poziom interfejsu API wskazuje 14, normal &amp; dużych ekranów; o numerze wersji 456.
-  61423456 jest ABI `x86` ; poziom interfejsu API wskazuje 14, normal &amp; dużych ekranów; o numerze wersji 456.

Aby kontynuować w tym przykładzie, Wyobraź sobie usterki został rozwiązany, który został specyficzne dla `armeabi-v7a`. Wersja aplikacji zwiększa 457 i nowych APK został skompilowany za `android:versionCode` ustawioną 21423457. VersionCodes dla `armeabi` i `x86` wersji pozostanie taka sama.

Teraz załóżmy, że x86 wersji odbiera niektórych aktualizacji lub poprawek, które odnoszą się do nowszej interfejsu API (poziom interfejsu API 19), co ta wersja 500 aplikacji. Nowe `versionCode` zmieniłby 61923500 podczas armeabi/armeabi-v7a pozostaną bez zmian. W tym momencie kody wersji mogą być następujące:

-  11413456 jest ABI `armeabi` ; poziom interfejsu API wskazuje 14, małych dużych ekranów; o nazwie wersji 456.
-  21423457 jest ABI `armeabi-v7a` ; poziom interfejsu API wskazuje 14, normal &amp; dużych ekranów; o nazwie wersji 457.
-  61923500 jest ABI `x86` ; poziom dotyczących interfejsu API 19, normal &amp; dużych ekranów; o nazwie wersji 500.


Obsługa te kody wersji ręcznie może być znaczne obciążenie dewelopera. Proces obliczania poprawny `android:versionCode` , a następnie tworzenie plik APK powinno zostać zautomatyzowane.
Przykładem to zrobić zostanie omówiona w przewodniku na końcu tego dokumentu.


### <a name="create-a-temporary-androidmanifestxml"></a>Tworzenie tymczasowego pliku AndroidManifest.XML

Chociaż nie niezbędne, tworzenie tymczasowych **AndroidManifest.XML** dla każdego interfejsu ABI mogą pomagać w zapobieganiu problemów, które mogą wystąpić przeciek informacji z APK jednego do drugiego. Na przykład jest niezwykle istotne który `android:versionCode` atrybut jest unikatowy dla każdego APK.

Jak to zrobić zależy od skryptów systemu zaangażowany, ale zwykle obejmuje tworzenie kopii manifestu systemu Android używane podczas tworzenia, modyfikowania go, a następnie za pomocą zmodyfikuj manifest podczas procesu kompilacji.



### <a name="compiling-the-apk"></a>Kompilowanie plik APK

Tworzenie APK na ABI najlepiej odbywa się za pomocą `xbuild` lub `msbuild` jak pokazano w następującym wierszu polecenia przykładowe:

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:Package /p:AndroidSupportedAbis=<TARGET_ABI> /p:IntermediateOutputPath=obj.<TARGET_ABI>/ /p:AndroidManifest=<PATH_TO_ANDROIDMANIFEST.XML> /p:OutputPath=bin.<TARGET_ABI> /p:Configuration=Release <CSPROJ FILE>
```

Poniżej opisano każdy parametr wiersza polecenia:

-   `/t:Package` &ndash; Tworzy Android APK, który jest podpisany przy użyciu magazynu kluczy debugowania

-   `/p:AndroidSupportedAbis=<TARGET_ABI>` &ndash; Ta ABI do obiektu docelowego. Musi wynosić `armeabi`, `armeabi-v7a`, lub `x86`

-   `/p:IntermediateOutputPath=obj.<TARGET_ABI>/` &ndash; To jest katalog, w którym będą przechowywane plików pośrednich, które są tworzone w ramach kompilacji. Jeśli to konieczne, Xamarin.Android utworzy katalog o nazwie po ABI, takich jak `obj.armeabi-v7a`. Zalecane jest używany inny folder dla każdej ABI jako pozwoli to uniknąć problemów, które należy wyników z plikami "przeciek" z jednej kompilacji do innego. Zwróć uwagę, że wartość ta kończy się znakiem separatora katalogu ( `/` w przypadku OS X).

-   `/p:AndroidManifest` &ndash; Ta właściwość określa ścieżkę do **AndroidManifest.XML** pliku, który będzie używany podczas kompilacji.

-   `/p:OutputPath=bin.<TARGET_ABI>` &ndash; To jest katalog, który będzie zawierać końcowego APK. Xamarin.Android utworzy katalog o nazwie po ABI, na przykład `bin.armeabi-v7a`.

-   `/p:Configuration=Release` &ndash; Wykonaj kompilacji wydania programu plik APK. Zwiększono wydajność debugowania kompilacji może nie być przekazane do witryny Google Play.

-   `<CS_PROJ FILE>` &ndash; To jest ścieżka do `.csproj` pliku projektu platformy Xamarin.Android.



### <a name="sign-and-zipalign-the-apk"></a>Zaloguj się i Zipalign plik APK

Należy podpisać plik APK przed mogą być dystrybuowane za pośrednictwem witryny Google Play. Można to wykonać przy użyciu `jarsigner` aplikacji, która jest częścią zestawu Java Developer. Następujące polecenie demonstrats wiersza, jak używać `jarsigner` w wierszu polecenia:

```shell
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore <PATH/TO/KEYSTORE> -storepass <PASSWORD> -signedjar <PATH/FOR/SIGNED_JAR> <PATH/FOR/JAR/TO/SIGN> <NAME_OF_KEY_IN_KEYSTORE>
```

Wszystkie aplikacje platformy Xamarin.Android musi być zip wyrównany można było uruchomić na urządzeniu. Jest to format wiersz polecenia, który ma być używany:

```shell
zipalign -f -v 4 <SIGNED_APK_TO_ZIPALIGN> <PATH/TO/ZIP_ALIGNED.APK>
```


## <a name="automating-apk-creation-with-rake"></a>Automatyzacja tworzenia APK z nachylenia

Przykładowy projekt [OneABIPerAPK](https://github.com/xamarin/monodroid-samples/tree/master/OneABIPerAPK) to proste projekt systemu Android, który przedstawiono sposób obliczania ABI odpowiedniego numeru wersji i kompilacji trzech oddzielnych APK firmy dla każdego z następujących ABI:

-  armeabi
-  armeabi-v7a
-  x86


[Rakefile](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb) w przykładowym projekcie wykonuje poszczególne kroki, które zostały opisane w poprzednich sekcjach:

1. [Utwórz android: versionCode](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L30) dla plik APK.

1. [Zapis systemu android: versionCode](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L55) do niestandardowego **AndroidManifest.XML** dla tego APK.

1. [Kompiluj kompilacji wydania](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L63) projektu platformy Xamarin.Android, który pojedynczo będzie współpracować z interfejsem ABI i przy użyciu **AndroidManifest.XML** utworzony w poprzednim kroku.

1. [Podpisać plik APK ](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L66) z magazynu kluczy produkcji.

1. [Zipalign](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L67) APK.


Aby utworzyć wszystkie APKs dla aplikacji, uruchom `build` nachylenia zadań w wierszu polecenia:

```shell
$ rake build
==> Building an APK for ABI armeabi with ./Properties/AndroidManifest.xml.armeabi, android:versionCode = 10814120.
==> Building an APK for ABI x86 with ./Properties/AndroidManifest.xml.x86, android:versionCode = 60814120.
==> Building an APK for ABI armeabi-v7a with ./Properties/AndroidManifest.xml.armeabi-v7a, android:versionCode = 20814120.
```

Po zakończeniu zadania nachylenia będzie trzy `bin` foldery z plikiem `xamarin.helloworld.apk`. Następny zrzut ekranu przedstawia każdego z tych folderów z ich zawartość:

[![Lokalizacje folderów specyficzne dla platformy zawierających xamarin.helloworld.apk](abi-specific-apks-images/image01.png)](abi-specific-apks-images/image01.png#lightbox)


> [!NOTE]
> Proces kompilacji opisane w tym przewodniku mogą być wykonywane w jednym z wielu systemów różnych kompilacji. Mimo że nie mamy przykład wcześniej zapisany, należy również możliwe za pomocą [Powershell](http://technet.microsoft.com/en-ca/scriptcenter/powershell.aspx) / [psake](https://github.com/psake/psake) lub [sfałszować](http://fsharp.github.io/FAKE/).


## <a name="summary"></a>Podsumowanie

W tym przewodniku podać kilka sugestii z tworzenie Android APK przeznaczonych Określ ABI. Również omówiona jeden schemat możliwych do utworzenia `android:versionCodes` który identyfikuje plik APK przeznaczony dla architektury Procesora. Wskazówki zawarte przykładowy projekt, który ma inicjowanych przez skrypty przy użyciu nachylenia kompilacji.



## <a name="related-links"></a>Linki pokrewne

- [OneABIPerAPK (przykład)](https://developer.xamarin.com/samples/OneABIPerAPK/)
- [Publikowanie aplikacji](~/android/deploy-test/publishing/index.md)
- [Obsługa wielu APK do witryny Google Play](http://developer.android.com/google/play/publishing/multiple-apks.html)
