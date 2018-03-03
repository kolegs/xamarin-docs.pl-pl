---
title: ProGuard
description: "Narzędzia ProGuard jest shrinker pliku Klasa Java, optymalizator, faktem i weryfikatora wstępne. Go wykrywa i usunięcie nieużywanych kodu, analizuje i optymalizuje kod bajtowy, a następnie zaciemnia klas i członków klasy. W tym przewodniku objaśniono sposób działania narzędzia ProGuard, jak włączyć go w projekcie i sposobie konfigurowania go. Umożliwia także kilka przykładów ProGuard konfiguracji."
ms.topic: article
ms.prod: xamarin
ms.assetid: 29C0E850-3A49-4618-9078-D59BE0284D5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 50666708bde2f2e7a61c30c6c9b383541e7ae9d5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="proguard"></a>ProGuard

_Narzędzia ProGuard jest shrinker pliku Klasa Java, optymalizator, faktem i weryfikatora wstępne. Go wykrywa i usunięcie nieużywanych kodu, analizuje i optymalizuje kod bajtowy, a następnie zaciemnia klas i członków klasy. W tym przewodniku objaśniono sposób działania narzędzia ProGuard, jak włączyć go w projekcie i sposobie konfigurowania go. Umożliwia także kilka przykładów ProGuard konfiguracji._

<a name="overview" />

## <a name="overview"></a>Omówienie

Narzędzia ProGuard wykrywa i usuwa nieużywane klas, pola, metody i atrybuty spakowanych aplikacji. Można nawet wykonaj te same przywoływanego bibliotek (to może pomóc w uniknięciu granicy odniesienia 64 k). Narzędzia ProGuard z zestawu SDK systemu Android zostanie również optymalizacji kodu bajtowego, Usuń nieużywane kodu instrukcje i zasłaniają pozostałych klas, pól i metod o krótszej nazwy. Odczyty proGuard **wejściowych słoików** , a następnie zmniejsza, optymalizuje zaciemnia i wstępnie sprawdza; zapisuje wyniki w co najmniej jeden **output słoików**. 

Procesy proGuard danych wejściowych w APK, wykonując następujące czynności: 

1.  **Zmniejszanie krok** &ndash; ProGuard rekursywnie Określa, które klas i członków klasy są używane. Wszystkie klasy i elementów członkowskich klasy nie zostaną odrzucone. 

2.  **Krok optymalizacji** &ndash; narzędzia ProGuard dodatkowo optymalizuje kod. 
    Wśród innych optymalizacji, klasy i metody, które nie są prywatne, statyczne lub końcowego można wprowadzić punkty wejścia można usunąć nieużywane parametry, a niektóre metody mogą być wbudowane. 

3.  **Krok zaciemnienie** &ndash; narzędzia ProGuard zmienia nazwę klasy i elementów członkowskich klasy, które nie są punkty wejścia. Zachowywanie punkty wejścia gwarantuje, że nadal będą one dostępne przez ich oryginalnych nazw. 

4.  **Krok preverification** &ndash; sprawdza bytecodes Java wcześniejsze środowisko uruchomieniowe i oznacza plików klas dla maszyny Wirtualnej Java. To jest jedynym krokiem, który nie musi wiedzieć, punkty wejścia. 

Każdy z tych kroków jest *opcjonalne*. Jak opisano w następnej sekcji, Xamarin.Android ProGuard korzysta z podzbioru następujące kroki. 


<a name="xa_proguard" />

## <a name="proguard-in-xamarinandroid"></a>ProGuard platformie Xamarin.Android

Konfiguracja platformy Xamarin.Android ProGuard nie zasłaniają plik APK. W rzeczywistości nie jest możliwe do włączenia zaciemnienie za pomocą narzędzia ProGuard (nawet przy użyciu plików konfiguracji niestandardowej). W związku z tym dla platformy Xamarin.Android narzędzia ProGuard przeprowadza tylko **zmniejszanie** i **optymalizacji** kroki: 

[ ![Zmniejszanie i optymalizacji](proguard-images/01-xa-chain-sml.png)](proguard-images/01-xa-chain.png)

Jeden element ważne należy wiedzieć z wyprzedzeniem przed przy użyciu narzędzia ProGuard jest, jak działa w ramach `Xamarin.Android` proces kompilacji. Ten proces wykorzystuje dwa oddzielne kroki: 

1.  Xamarin Android Linker

2.  ProGuard

Każda z tych czynności opisano dalej.


<a name="linker" />

### <a name="linker-step"></a>Krok konsolidatora

Konsolidator Xamarin.Android wdraża analizy statycznej aplikacji w celu określenia następujących czynności: 

-   Zestawy, które są rzeczywiście używane.

-   Typy, które są rzeczywiście używane.

-   Składniki, które są rzeczywiście używane. 

Konsolidator zawsze będzie uruchamiane przed wykonaniem kroku ProGuard. W związku z tym konsolidator może paska zestawu/typu/elementów członkowskich, które mogą wymagać narzędzia ProGuard do uruchamiania na. (Aby uzyskać więcej informacji na temat łączenia platformie Xamarin.Android, zobacz [konsolidacji w systemie Android](~/android/deploy-test/linker.md).)


<a name="proguard_step" />

### <a name="proguard-step"></a>Krok proGuard

Po kroku konsolidatora zakończy się pomyślnie, narzędzia ProGuard jest uruchamiane na usuwanie nieużywanych Java kodu bajtowego. Jest to krok, który jest zoptymalizowany plik APK. 


<a name="using" />

## <a name="using-proguard"></a>Przy użyciu ProGuard

Aby użyć narzędzia ProGuard w projekcie aplikacji, należy najpierw włączyć narzędzia ProGuard. Następnie albo umożliwić Xamarin.Android kompilacji procesu Użyj domyślnego pliku konfiguracji ProGuard lub można utworzyć własny plik konfiguracji niestandardowej dla narzędzia ProGuard do użycia. 


<a name="enabling" />

### <a name="enabling-proguard"></a>Włączanie ProGuard

Aby włączyć narzędzia ProGuard w projekcie aplikacji, wykonaj następujące kroki:

1.  Upewnij się, że projekt jest ustawiona na **wersji** konfiguracji (to jest ważne, ponieważ konsolidator muszą działać w kolejności dla narzędzia ProGuard uruchamianie): 

    [ ![Wybierz konfigurację wydania](proguard-images/02-set-release-sml.png)](proguard-images/02-set-release.png)
   
2.  Włącz narzędzia ProGuard sprawdzając **włączyć ProGuard** opcję w obszarze **pakowania** karty **właściwości > Opcje Android**: 

    [ ![Włącz Proguard opcji](proguard-images/03-enable-proguard-sml.png)](proguard-images/03-enable-proguard.png)

Dla większości aplikacji platformy Xamarin.Android, domyślny plik konfiguracji ProGuard dostarczonych przez Xamarin.Android będą wystarczające, aby usunąć wszystkie (i tylko) nieużywane kodu. Aby wyświetlić ProGuard konfiguracji domyślnej, otwórz plik **obj\\wersji\\proguard\\proguard_xamarin.cfg**. W następnej sekcji opisano sposób tworzenia dostosowanego ProGuard pliku konfiguracji. 


<a name="customizing" />

### <a name="customizing-proguard"></a>Dostosowywanie ProGuard

Opcjonalnie można dodać niestandardowego pliku konfiguracji ProGuard większą kontrolę nad narzędzia ProGuard. Na przykład można jawnie Poinformuj narzędzia ProGuard klas, które do zachowania. W tym celu należy utworzyć nową **.cfg** plików i stosuje się `ProGuardConfiguration` Akcja kompilacji **właściwości** okienku **Eksploratora rozwiązań**: 

[ ![Wybranej akcji kompilacji ProguardConfiguration](proguard-images/04-build-action-sml.png)](proguard-images/04-build-action.png)

Należy pamiętać, że ten plik konfiguracji nie zastępuje platformy Xamarin.Android **proguard_xamarin.cfg** pliku, ponieważ są używane przez narzędzia ProGuard. 

Poniższy przykład przedstawia plik typowej konfiguracji ProGuard:
    

    # This is Xamarin-specific (and enhanced) configuration.

    -dontobfuscate

    -keep class mono.MonoRuntimeProvider { *; <init>(...); }
    -keep class mono.MonoPackageManager { *; <init>(...); }
    -keep class mono.MonoPackageManager_Resources { *; <init>(...); }
    -keep class mono.android.** { *; <init>(...); }
    -keep class mono.java.** { *; <init>(...); }
    -keep class mono.javax.** { *; <init>(...); }
    -keep class opentk.platform.android.AndroidGameView { *; <init>(...); }
    -keep class opentk.GameViewBase { *; <init>(...); }
    -keep class opentk_1_0.platform.android.AndroidGameView { *; <init>(...); }
    -keep class opentk_1_0.GameViewBase { *; <init>(...); }

    -keep class android.runtime.** { <init>(***); }
    -keep class assembly_mono_android.android.runtime.** { <init>(***); }
    # hash for android.runtime and assembly_mono_android.android.runtime.
    -keep class md52ce486a14f4bcd95899665e9d932190b.** { *; <init>(...); }
    -keepclassmembers class md52ce486a14f4bcd95899665e9d932190b.** { *; <init>(...); }

    # Android's template misses fluent setters...
    -keepclassmembers class * extends android.view.View {
       *** set*(***);
    }

    # also misses those inflated custom layout stuff from xml...
    -keepclassmembers class * extends android.view.View {
       <init>(android.content.Context,android.util.AttributeSet);
       <init>(android.content.Context,android.util.AttributeSet,int);
    }
    

Może to być przypadku narzędzia ProGuard nie można poprawnie przeanalizować aplikacji; go może potencjalnie usunąć kod, który aplikacja faktycznie. Jeśli dzieje się tak, możesz dodać `-keep` wiersza do pliku konfiguracji niestandardowej ProGuard: 

    -keep public class MyClass

W tym przykładzie `MyClass` ma ustawioną wartość rzeczywistą nazwą klasy, która ma narzędzia ProGuard do pominięcia.

Można również zarejestrować nazwy z `[Register]` adnotacje i użycie tych nazw do dostosowania reguł ProGuard. Można zarejestrować nazwy kart, widoki, BroadcastReceivers, usług, ContentProviders, działań i fragmenty. Aby uzyskać więcej informacji o korzystaniu z `[Register]` atrybutu niestandardowego, zobacz [Praca z JNI](~/android/platform/java-integration/working-with-jni.md).


<a name="options" />

### <a name="proguard-options"></a>Opcje proGuard

Narzędzia ProGuard oferuje wiele opcji, które można skonfigurować, aby zapewnić bardziej precyzyjną kontrolę nad jego działania. [ProGuard ręcznego](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/introduction.html) zapewnia pełną dokumentację w dokumentacji użycie narzędzia ProGuard. 

Xamarin.Android obsługuje ProGuard następujące opcje: 


-    [Opcje we/wy](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#iooptions)

-    [Zachowaj opcje](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoptions)

-    [Zmniejszanie opcje](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#shrinkingoptions)

-    [Opcje ogólne](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#generaloptions)

-    [Klasa ścieżki](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#classpath)

-    [Nazwy plików](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filename)

-    [Filtry plików](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filefilters)

-    [Filtry](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filters)

-    [Omówienie `Keep` opcje](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoverview)

-    [Zachowaj opcję modyfikatorów](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoptionmodifiers)

-    [Specyfikacje — klasa](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#classspecification)

Poniższe opcje są *ignorowane* przez Xamarin.Android:

-    [Opcje optymalizacji](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#optimizationoptions)

-    [Opcje zaciemnienie](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#obfuscationoptions) 

-    [Opcje preverification](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#preverificationoptions)


<a name="nougat" />

## <a name="proguard-and-android-nougat"></a>Nugacie proGuard i Android

Jeśli chcesz użyć narzędzia ProGuard z systemem Android 7.0 lub nowszej, możesz pobrać nowszą wersję narzędzia ProGuard ponieważ zestawu SDK systemu Android nie jest składnikiem nowej wersji, która jest zgodna z JDK 1.8.

Możesz użyć tej funkcji [pakietu NuGet](https://www.nuget.org/packages/name.atsushieno.proguard.facebook/5.3.0) Aby zainstalować nowszą wersję `proguard.jar`. Aby uzyskać więcej informacji o aktualizowaniu domyślny zestaw SDK systemu Android `proguard.jar`, zobacz ten [przepełnienie stosu](http://stackoverflow.com/questions/39514518/xamarin-android-proguard-unsupported-class-version-number-52-0/39514706#39514706) dyskusji.

Wszystkie wersje narzędzia ProGuard w można znaleźć [strony SourceForge](https://sourceforge.net/projects/proguard/files/). 


<a name="examples" />

## <a name="example-proguard-configurations"></a>Przykładowe konfiguracje ProGuard

Poniżej przedstawiono dwa pliki konfiguracji ProGuard przykład. Podaj należy pamiętać, że w tych przypadkach Xamarin.Android proces kompilacji **wejściowych**, **dane wyjściowe**, i **biblioteki** słoików. W związku z tym można skupić się na inne opcje, takie jak `-keep`. 

### <a name="a-simple-android-activity"></a>Proste działanie systemu Android

Poniżej przedstawiono przykładową konfigurację proste działanie systemu Android:

    -injars  bin/classes
    -outjars bin/classes-processed.jar
    -libraryjars /usr/local/java/android-sdk/platforms/android-9/android.jar

    -dontpreverify
    -repackageclasses ''
    -allowaccessmodification
    -optimizations !code/simplification/arithmetic

    -keep public class mypackage.MyActivity

### <a name="a-complete-android-application"></a>Kompletna aplikacja systemu Android

Poniżej przedstawiono przykładową konfigurację dla pełnej aplikacji systemu Android:

    -injars  bin/classes
    -injars  libs
    -outjars bin/classes-processed.jar
    -libraryjars /usr/local/java/android-sdk/platforms/android-9/android.jar

    -dontpreverify
    -repackageclasses ''
    -allowaccessmodification
    -optimizations !code/simplification/arithmetic
    -keepattributes *Annotation*

    -keep public class * extends android.app.Activity
    -keep public class * extends android.app.Application
    -keep public class * extends android.app.Service
    -keep public class * extends android.content.BroadcastReceiver
    -keep public class * extends android.content.ContentProvider

    -keep public class * extends android.view.View {
    public <init>(android.content.Context);
    public <init>(android.content.Context, android.util.AttributeSet);
    public <init>(android.content.Context, android.util.AttributeSet, int);
    public void set*(...);
    }

    -keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet);
    }

    -keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet, int);
    }

    -keepclassmembers class * implements android.os.Parcelable {
    static android.os.Parcelable$Creator CREATOR;
    }

    -keepclassmembers class **.R$* {
    public static <fields>;
    }

<a name="build" />

## <a name="proguard-and-the-xamarinandroid-build-process"></a>ProGuard i proces kompilacji platformy Xamarin.Android

W poniższych sekcjach opisano sposób uruchamiania narzędzia ProGuard podczas Xamarin.Android **wersji** kompilacji.


### <a name="what-command-is-proguard-running"></a>Jakie polecenia narzędzia ProGuard jest uruchomiona?

Narzędzia ProGuard jest po prostu `.jar` dostarczane z zestawu SDK systemu Android. W związku z tym jest wywoływana w poleceniu: 

```shell
java -jar proguard.jar options ...
```

### <a name="the-proguard-task"></a>ProGuard zadań

ProGuard zadań znajduje się wewnątrz **Xamarin.Android.Build.Tasks.dll** zestawu. Jest ona częścią `_CompileToDalvikWithDx` docelowej, która jest częścią programu `_CompileDex` docelowej. 

Poniżej przedstawiono przykład parametrów domyślnych, które są generowane po Utwórz nowy projekt za pomocą **Plik > Nowy projekt**: 

    ProGuardJarPath = C:\Android\android-sdk\tools\proguard\lib\proguard.jar
    AndroidSdkDirectory = C:\Android\android-sdk\
    JavaToolPath = C:\Program Files (x86)\Java\jdk1.8.0_92\\bin
    ProGuardToolPath = C:\Android\android-sdk\tools\proguard\
    JavaPlatformJarPath = C:\Android\android-sdk\platforms\android-25\android.jar
    ClassesOutputDirectory = obj\Release\android\bin\classes
    AcwMapFile = obj\Release\acw-map.txt
    ProGuardCommonXamarinConfiguration = obj\Release\proguard\proguard_xamarin.cfg
    ProGuardGeneratedReferenceConfiguration = obj\Release\proguard\proguard_project_references.cfg
    ProGuardGeneratedApplicationConfiguration = obj\Release\proguard\proguard_project_primary.cfg
    ProGuardConfigurationFiles

      {sdk.dir}tools\proguard\proguard-android.txt;
      {intermediate.common.xamarin};
      {intermediate.references};
      {intermediate.application};
      ;
     
    JavaLibrariesToEmbed = C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\MonoAndroid\v7.0\mono.android.jar
    ProGuardJarInput = obj\Release\proguard\__proguard_input__.jar
    ProGuardJarOutput = obj\Release\proguard\__proguard_output__.jar
    DumpOutput = obj\Release\proguard\dump.txt
    PrintSeedsOutput = obj\Release\proguard\seeds.txt
    PrintUsageOutput = obj\Release\proguard\usage.txt
    PrintMappingOutput = obj\Release\proguard\mapping.txt

Kolejnym przykładzie przedstawiono typowe polecenie ProGuard, który jest uruchamiany z IDE:

```cmd
C:\Program Files (x86)\Java\jdk1.8.0_92\\bin\java.exe -jar C:\Android\android-sdk\tools\proguard\lib\proguard.jar -include obj\Release\proguard\proguard_xamarin.cfg -include obj\Release\proguard\proguard_project_references.cfg -include obj\Release\proguard\proguard_project_primary.cfg "-injars 'obj\Release\proguard\__proguard_input__.jar';'C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\MonoAndroid\v7.0\mono.android.jar'" "-libraryjars 'C:\Android\android-sdk\platforms\android-25\android.jar'" -outjars "obj\Release\proguard\__proguard_output__.jar" -optimizations !code/allocation/variable
```


<a name="troubleshoot" />

## <a name="troubleshooting"></a>Rozwiązywanie problemów

<a name="files" />

### <a name="file-issues"></a>Zagadnienia dotyczące plików

Może zostać wyświetlony następujący komunikat o błędzie, gdy narzędzia ProGuard odczytuje plikiem konfiguracji: 

    Unknown option '-keep' in line 1 of file 'proguard.cfg'

Ten problem zwykle spowodowany w systemie Windows `.cfg` plik ma nieprawidłowe kodowanie. Nie można obsłużyć narzędzia ProGuard _znacznika kolejności bajtów_ (BOM) które mogą być obecne w plikach tekstowych. Jeśli występuje BOM narzędzia ProGuard zostanie zakończona z powodu błędu powyżej. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby zapobiec temu problemowi, należy edytować plik konfiguracji niestandardowej w edytorze tekstu, co umożliwi pliku do zapisania bez BOM. Aby rozwiązać ten problem, upewnij się, że w edytorze tekstów ma kodowanie ustawioną `UTF-8`. Na przykład, Edytor tekstu [Notatnik ++](https://notepad-plus-plus.org/) można zapisywać pliki bez BOM wybierając **kodowanie &gt; kodowanie UTF-8 bez BOM** podczas zapisywania pliku. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby zapobiec temu problemowi, należy zapisać plik konfiguracji niestandardowej z edytora tekstu, który pozwala na pominięcie BOM. 

-----


<a name="other" />

### <a name="other-issues"></a>Inne problemy

Narzędzia ProGuard [Rozwiązywanie problemów](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/troubleshooting.html) strony omówiono typowe problemy mogą wystąpić (i rozwiązań) przy użyciu narzędzia ProGuard.

<a name="summary" />

## <a name="summary"></a>Podsumowanie

W tym przewodniku wyjaśniono, jak narzędzia ProGuard działa w Xamarin.Android, jak włączyć go w projekcie aplikacji i sposobie konfigurowania go. Zapewniała przykładowe konfiguracje ProGuard i go opisane rozwiązania typowych problemów. Aby uzyskać więcej informacji na temat narzędzia ProGuard i Android, zobacz [zmniejszyć kodu i zasobów](http://developer.android.com/tools/help/proguard.html). 


## <a name="related-links"></a>Linki pokrewne

- [Przygotowywanie aplikacji dla wersji](~/android/deploy-test/release-prep/index.md)
