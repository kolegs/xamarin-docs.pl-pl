---
title: Wprowadzenie do korzystania z systemu Android
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: ed3d6ae3f8537bfae456c6919743e8c9fbfb2009
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-android"></a>Wprowadzenie do korzystania z systemu Android


Oprócz wymagań dotyczących z naszych [wprowadzenie do języka Java](~/tools/dotnet-embedding/get-started/java/index.md) przewodniku, należy również:

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) lub nowszy
* [Android Studio 3.x](https://developer.android.com/studio/index.html) z językiem Java 1.8

Jako Przegląd zostaną wykonane następujące czynności:

* Tworzenie projektu biblioteki systemu Android C#
* Zainstaluj .NET osadzanie za pośrednictwem pakietu NuGet
* Uruchom Embeddinator w zestawie biblioteki systemu Android
* Użyj wygenerowanego pliku AAR w projekcie języka Java w programie Android Studio

## <a name="create-an-android-library-project"></a>Tworzenie projektu biblioteki systemu Android

Otwórz program Visual Studio dla systemu Windows lub Mac, Utwórz nowy projekt dla systemu Android biblioteki klas, nadaj jej nazwę `hello-from-csharp`i zapisać go do `~/Projects/hello-from-csharp` lub `%USERPROFILE%\Projects\hello-from-csharp`.

Dodaj nowe działanie systemu Android o nazwie `HelloActivity.cs`, a następnie Android układu w `Resource/layout/hello.axml`.

Dodaj nową `TextView` układ i zmiana tekstu jest przyjemne inny.

Źródła układu powinien wyglądać następująco:
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text="Hello from C#!"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center" />
</LinearLayout>
```

Upewnij się, wywoływania w Twoich działaniach `SetContentView` z nowego układu:
```csharp
[Activity(Label = "HelloActivity"),
    Register("hello_from_csharp.HelloActivity")]
public class HelloActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SetContentView(Resource.Layout.hello);
    }
}
```
*Uwaga: nie zapomnij `[Register]` atrybutów, zobacz szczegóły w obszarze ograniczenia*

Skompiluj projekt, wynikowy zestaw zostanie zapisany w `bin/Debug/hello-from-csharp.dll`.

## <a name="installing-net-embedding-from-nuget"></a>Instalowanie .NET osadzanie z NuGet

Wybierz **Dodaj > Dodawanie pakietów NuGet...**  i zainstaluj `Embeddinator-4000` z Menedżera pakietów NuGet: ![Menedżera pakietów NuGet](android-images/visualstudionuget.png)

Spowoduje to zainstalowanie `Embeddinator-4000.exe` do `packages/Embeddinator-4000/tools` katalogu.

## <a name="run-embeddinator-4000"></a>Run Embeddinator-4000

Dodamy krok mające miejsce po kompilacji, aby uruchomić Embeddinator i utworzyć natywny plik AAR dla zestawu projektu biblioteki systemu Android.

W programie Visual Studio dla komputerów Mac, przejdź do _opcje projektu > kompilacji > polecenia niestandardowych_ i Dodaj _po kompilacji_ kroku.

Ustaw następujące commnd:
```
mono '${SolutionDir}/packages/Embeddinator-4000.0.2.0.80/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

> [!NOTE]
> Uwaga: Upewnij się, że numer wersji z NuGet została zainstalowana

Jeśli zamierzasz realizacji bieżących prac nad projektu C#, może również Dodawanie polecenia niestandardowego do czyszczenia `output` katalogu przed uruchomieniem Embeddinator:
```
rm -Rf '${SolutionDir}/output/'
```

![Akcji niestandardowej kompilacji](android-images/visualstudiocustombuild.png)

Plik AAR systemu Android, zostaną umieszczone w `~/Projects/hello-from-csharp/output/hello_from_csharp.aar`. _Uwaga: łączniki są zastępowane, ponieważ Java nie jest ona obsługiwana w polu nazwy pakietu._

### <a name="running-net-embedding-on-windows"></a>Uruchomiona .NET osadzanie w systemie Windows

Firma Microsoft będzie zasadniczo Instalatora samo, ale menu w programie Visual Studio są nieco inny w systemie Windows. Polecenia powłoki również są nieco inne.

Przejdź do **opcje projektu > zdarzeń kompilacji** , a następnie wprowadź poniższy **wiersz polecenia zdarzenia po kompilacji** pola:
```
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

Takie jak poniższy zrzut ekranu:

![Embeddinator w systemie Windows](android-images/visualstudiowindows.png)

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Użyj wygenerowanych danych wyjściowych w projekcie programu Android Studio

1. Otwórz program Android Studio i utworzyć nowy projekt z **pusty działania**.
2. Kliknij prawym przyciskiem myszy użytkownika `app` modułu i wybierz polecenie **nowy > Moduł**.
3. Wybierz **importu. JAR /. Pakiet AAR**.
4. Użyj przeglądarki katalogu, aby zlokalizować `~/Projects/hello-from-csharp/output/hello_from_csharp.aar` i kliknij przycisk **Zakończ**.

![Importuj AAR do programu Android Studio](android-images/androidstudioimport.png)

Spowoduje to skopiowanie pliku AAR do nowego modułu o nazwie `hello_from_csharp`.

![Android Studio Project](android-images/androidstudioproject.png)

Aby użyć nowego modułu z Twojej `app`, kliknij prawym przyciskiem myszy i wybierz opcję **Otwórz ustawienia modułu**. Na **zależności** , Dodaj nowy **zależności modułu** i wybierz polecenie `:hello_from_csharp`.

![Android Studio zależności](android-images/androidstudiodependencies.png)

W Twoich działaniach, Dodaj nową `onResume` — metoda i użyj następujący kod, aby uruchomić nasze działania C#:

```java
import hello_from_csharp.*;

public class MainActivity extends AppCompatActivity {
    //... Other stuff here ...
    @Override
    protected void onResume() {
        super.onResume();

        Intent intent = new Intent(this, HelloActivity.class);
        startActivity(intent);
    }
}
```

### <a name="assembly-compression-important"></a>Kompresja zestawów *ważne*

Jeden dalszych zmian jest wymagany do osadzania .NET w projekcie Android Studio.

Otwórz aplikacji `build.gradle` i dodaj następujące zmiany:
```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```
Obecnie Xamarin.Android ładuje zestawów platformy .NET bezpośrednio z APK, ale wymaga to zestawy ma nie być skompresowany.

Jeśli nie masz tej instalacji, aplikacja awarii po uruchomieniu i przypominać drukowanie do konsoli:

```csharp
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>Uruchamianie aplikacji

Podczas uruchamiania aplikacji:

![Witaj z systemem w emulatorze próbki C#](android-images/hello-from-csharp-android.png)

Należy zwrócić uwagę na to, co się stało tutaj:

* Mamy klasy C# `HelloActivity`, że Java podklasy
* Mamy plików zasobów dla systemu Android
* My używamy tych za pomocą języka Java w programie Android Studio

Tak aby ten przykład działał, są wszystkie poniższe ustawienia w ostatnim APK:

* Xamarin.Android jest skonfigurowana na uruchamianie aplikacji
* Uwzględnione w zestawów platformy .NET `assets/assemblies`
* `AndroidManifest.xml` modyfikacje dla Twojego C# działania itd.
* Android zasobów i zasoby z bibliotekami .NET
* [Wywoływane otoki android](https://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/android_callable_wrappers/) jakichkolwiek `Java.Lang.Object` podklasy

Jeśli szukasz dodatkowe wskazówki wyewidencjonować ten film osadzanie Charlesa Petzold [pokaz FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/) w programie Android Studio project tutaj:

[![Embeddinator-4000 dla systemu Android](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Przy użyciu języka Java 1.8

Począwszy od to pisanie, najlepszym rozwiązaniem jest za pomocą systemu Android Studio 3.0 ([Pobierz tutaj](https://developer.android.com/studio/index.html)).

Aby włączyć obsługę języka Java 1.8 w module aplikacji `build.gradle` pliku:
```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```
Można również wykonać spojrzeć na projekt testowy naszych Android Studio [tutaj](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) więcej szczegółów.

Są chce używać Android Studio 2.3.x stabilny, musisz włączyć przestarzałe łańcuch narzędzi gniazda:
```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Bieżące ograniczenia w systemie Android

Teraz, jeśli można podklasy `Java.Lang.Object`, Xamarin.Android wygeneruje stub Java (Android wywoływana otoka) zamiast Embeddinator.

Dlatego należy wykonać te same zasady eksportowanie C# języka Java jako platformy Xamarin.Android.

Tak na przykład w języku C#:
```csharp
    [Register("mono.embeddinator.android.ViewSubclass")]
    public class ViewSubclass : TextView
    {
        public ViewSubclass(Context context) : base(context) { }

        [Export("apply")]
        public void Apply(string text)
        {
            Text = text;
        }
    }
```

* `[Register]` jest wymagany do mapowania odpowiednią nazwę pakietu języka Java
* `[Export]` jest wymagany do uwidaczniania Java — metoda

Możemy użyć `ViewSubclass` w języku Java w następujący sposób:
```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

Przeczytaj więcej na temat [Java integracji z platformy Xamarin.Android](~/android/platform/java-integration/index.md).

## <a name="multiple-assemblies"></a>Wiele zestawów

Osadzanie w jednym zestawie jest bezpośrednie; jednak jest znacznie bardziej prawdopodobne, konieczne będzie więcej niż jednego języka C# zestawu. Wiele razy będzie mieć zależności na pakiety NuGet, takie jak obsługa systemu Android biblioteki lub usług Google Play który skomplikować dalszych czynności.

Powoduje to dilemma, ponieważ Embeddinator musi dołączać wiele typów plików do końcowego AAR, takich jak:
* Zasoby dla systemu android
* Zasoby dla systemu android
* Android natywnych bibliotek
* Źródłowy java systemu android

Prawdopodobnie nie będzie dołączać te pliki z biblioteki obsługi systemu Android lub usług Google Play do Twojej AAR, ale raczej użyje oficjalną wersją z Google w programie Android Studio.

Poniżej przedstawiono zalecane podejście:
* Przekaż Embeddinator zestawu, który własnej (ma źródło) i ma zostać wywołana za pomocą języka Java
* Przekaż Embeddinator zestawu, który należy Android zasoby natywnych bibliotek i zasobów z
* Dodanie obsługi języka Java zależności, takich jak Android biblioteki lub usług Google Play w programie Android Studio

To polecenie może być:
```
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```
Powinno wykluczać niczego z pakietu NuGet, chyba że dowiedzieć się nim Android zasobów, zasobów, itp., który będzie potrzebny w projekcie Android Studio. Można również pominąć zależności, które nie należy wywoływać z języka Java i konsolidator _powinien_ zawierać części biblioteki, które są potrzebne.

Można dodać zależności Java potrzebne w programie Android Studio Twojej `build.gradle` pliku może wyglądać tak:
```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>Dalsze informacje

* [Wywołania zwrotne w systemie Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Wstępne badanie systemu Android](~/tools/dotnet-embedding/android/index.md)
* [Ograniczenia Embeddinator](~/tools/dotnet-embedding/limitations.md)
* [Współtworzenie projekt open source](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Kody błędów wraz z opisami](~/tools/dotnet-embedding/errors.md)


## <a name="related-links"></a>Linki pokrewne

- [Przykładowe pogody (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
