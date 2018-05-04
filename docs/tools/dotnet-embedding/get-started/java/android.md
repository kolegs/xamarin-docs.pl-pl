---
title: Wprowadzenie do korzystania z systemu Android
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: 1d50d7dbc53271a15e06a7a18d2fa95f91b76ea4
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-android"></a>Wprowadzenie do korzystania z systemu Android

Oprócz wymagań dotyczących z [wprowadzenie do języka Java](~/tools/dotnet-embedding/get-started/java/index.md) przewodniku, należy również:

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) lub nowszy
* [Android Studio 3.x](https://developer.android.com/studio/index.html) z językiem Java 1.8

Jako Przegląd zostaną wykonane następujące czynności:

* Tworzenie projektu biblioteki systemu Android C#
* Zainstaluj .NET osadzanie za pośrednictwem pakietu NuGet
* Uruchom .NET osadzenia w zestawie biblioteki systemu Android
* Użyj wygenerowanego pliku AAR w projekcie języka Java w programie Android Studio

## <a name="create-an-android-library-project"></a>Tworzenie projektu biblioteki systemu Android

Otwórz program Visual Studio dla systemu Windows lub Mac, Utwórz nowy projekt dla systemu Android biblioteki klas, nadaj jej nazwę **hello z csharp**i zapisać go do **~/Projects/hello-from-csharp** lub **%USERPROFILE%\ Projects\hello z csharp**.

Dodaj nowe działanie systemu Android o nazwie **HelloActivity.cs**, a następnie Android układu w **Resource/layout/hello.axml**.

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

> [!NOTE]
> Nie zapomnij `[Register]` atrybutu. Aby uzyskać więcej informacji, zobacz [ograniczenia](#current-limitations-on-android).

Skompiluj projekt. Wynikowy zestaw zostanie zapisany w `bin/Debug/hello-from-csharp.dll`.

## <a name="installing-net-embedding-from-nuget"></a>Instalowanie .NET osadzanie z NuGet

Postępuj zgodnie z następującymi [instrukcje](~/tools/dotnet-embedding/get-started/install/install.md) do instalowania i konfigurowania osadzanie .NET projektu.

Wywołanie polecenia, które należy skonfigurować będzie wyglądać następująco:

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

```shell
mono '${SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

#### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Użyj wygenerowanych danych wyjściowych w projekcie programu Android Studio

1. Otwórz program Android Studio i utworzyć nowy projekt z **pusty działania**.
2. Kliknij prawym przyciskiem myszy użytkownika **aplikacji** modułu i wybierz polecenie **nowy > Moduł**.
3. Wybierz **importu. JAR /. Pakiet AAR**.
4. Użyj przeglądarki katalogu, aby zlokalizować **~/Projects/hello-from-csharp/output/hello_from_csharp.aar** i kliknij przycisk **Zakończ**.

![Importuj AAR do programu Android Studio](android-images/androidstudioimport.png)

Spowoduje to skopiowanie pliku AAR do nowego modułu o nazwie **hello_from_csharp**.

![Android Studio Project](android-images/androidstudioproject.png)

Aby użyć nowego modułu z Twojej **aplikacji**, kliknij prawym przyciskiem myszy i wybierz opcję **Otwórz ustawienia modułu**. Na **zależności** , Dodaj nowy **zależności modułu** i wybierz polecenie **: hello_from_csharp**.

![Android Studio zależności](android-images/androidstudiodependencies.png)

W Twoich działaniach, Dodaj nową `onResume` — metoda i użyj następujący kod, aby uruchomić działanie C#:

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

### <a name="assembly-compression-important"></a>Kompresja zestawów (*ważne*)

Jeden dalszych zmian jest wymagany do osadzania .NET w projekcie Android Studio.

Otwórz aplikacji **build.gradle** i dodaj następujące zmiany:

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

```shell
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>Uruchamianie aplikacji

Podczas uruchamiania aplikacji:

![Witaj z systemem w emulatorze próbki C#](android-images/hello-from-csharp-android.png)

Należy zwrócić uwagę na to, co się stało tutaj:

* Mamy klasy C# `HelloActivity`, że Java podklasy
* Mamy plików zasobów dla systemu Android
* My używamy tych za pomocą języka Java w programie Android Studio

Aby ten przykład działał wszystkie poniższe są zainstalowane w ostatnim APK:

* Xamarin.Android jest skonfigurowana na uruchamianie aplikacji
* Zestawy .NET objęte **zasoby/zespołów**
* **AndroidManifest.xml** modyfikacje dla Twojego C# działania itd.
* Zasoby dla systemu android i zasoby z bibliotekami .NET
* [Wywoływane otoki android](~/android/platform/java-integration/android-callable-wrappers.md) jakichkolwiek `Java.Lang.Object` podklasy

Jeśli szukasz dodatkowe wskazówki wyewidencjonować poniższego klipu wideo, który demonstruje osadzanie Charlesa Petzold [pokaz FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/) w projekcie programu Android Studio:

[![Embeddinator-4000 dla systemu Android](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Przy użyciu języka Java 1.8

Począwszy od to pisanie, najlepszym rozwiązaniem jest za pomocą systemu Android Studio 3.0 ([Pobierz tutaj](https://developer.android.com/studio/index.html)).

Aby włączyć obsługę języka Java 1.8 w module aplikacji **build.gradle** pliku:

```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

Można również wykonać przyjrzeć się [projekt programu Android Studio testu](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) więcej szczegółów. 

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

Teraz, jeśli użytkownik podklasy `Java.Lang.Object`, Xamarin.Android wygeneruje stub Java (Android wywoływana otoka) zamiast osadzania ich .NET. W związku z tym należy wykonać te same zasady eksportowanie C# języka Java jako platformy Xamarin.Android. Na przykład:

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

Powoduje to dilemma, ponieważ osadzanie .NET musi zawierać wiele typów plików do końcowego AAR, takich jak:

* Zasoby dla systemu android
* Zasoby dla systemu android
* Android natywnych bibliotek
* Źródłowy java systemu android

Prawdopodobnie nie będzie dołączać te pliki z biblioteki obsługi systemu Android lub usług Google Play do Twojej AAR, ale raczej użyje oficjalną wersją z Google w programie Android Studio.

Poniżej przedstawiono zalecane podejście:

* Przekaż osadzanie .NET zestawu, który własnej (ma źródło) i ma zostać wywołana za pomocą języka Java
* Przekaż osadzanie .NET zestawu, który należy Android zasoby natywnych bibliotek i zasobów z
* Dodanie obsługi języka Java zależności, takich jak Android biblioteki lub usług Google Play w programie Android Studio

To polecenie może być:

```shell
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```

Powinno wykluczać niczego z pakietu NuGet, chyba że dowiedzieć się nim Android zasobów, zasobów, itp., który będzie potrzebny w projekcie Android Studio. Można również pominąć zależności, które nie należy wywoływać z języka Java i konsolidator _powinien_ zawierać części biblioteki, które są potrzebne.

Można dodać zależności Java potrzebne w programie Android Studio Twojej **build.gradle** pliku może wyglądać tak:

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
* [Ograniczenia osadzania .NET](~/tools/dotnet-embedding/limitations.md)
* [Współtworzenie projekt open source](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Kody błędów wraz z opisami](~/tools/dotnet-embedding/errors.md)

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe pogody (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
