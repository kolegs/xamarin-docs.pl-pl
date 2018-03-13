---
title: GDB
ms.topic: article
ms.prod: xamarin
ms.assetid: CD0BE462-FA38-4881-B481-82AD05B3B8FE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 55d72a49f90095a33577279d018e1696dda8fc42
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="gdb"></a>GDB

## <a name="overview"></a>Omówienie

Xamarin.Android 4.10 wprowadzono częściowe obsługę przy użyciu `gdb` przy użyciu `_Gdb` docelowy programu MSBuild. 

> [!NOTE]
> `gdb` Obsługa wymaga zainstalowania zestawu Android NDK.

Istnieją trzy sposoby używania `gdb`:

1.  [Zwiększono wydajność debugowania kompilacji z szybkiego wdrożenia włączone](#Debug_Builds_with_Fast_Deployment) .
1.  [Zwiększono wydajność debugowania kompilacji z szybkiego wdrożenia wyłączone](#Debug_Builds_without_Fast_Deployment) .
1.  [Kompilacje wydania](#Release_Builds) .


W przypadku wystąpienia problemów, zobacz [Rozwiązywanie problemów](#Troubleshooting) sekcji.

<a name="Debug_Builds_with_Fast_Deployment" />

### <a name="debug-builds-with-fast-deployment"></a>Debugowanie kompilacji z szybkiego wdrażania

Tworzenie i wdrażanie debugowania kompilowania z szybkiego wdrożenia włączone, `gdb` mogą być dołączane za pomocą `_Gdb` docelowy programu MSBuild.

Najpierw należy zainstalować aplikację. Można to zrobić za pomocą środowiska IDE lub z wiersza polecenia:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:Install *.csproj
```

Po drugie, uruchom `_Gdb` docelowej. Po zakończeniu wykonywania `gdb` wydruku wiersza polecenia:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
...
```

`_Gdb` Docelowy uruchomi uruchamiania dowolne działanie jest deklarowane w ramach Twojej `AndroidManifest.xml` pliku. Aby jawnie określić działanie (Activity) do uruchomienia, użyj `RunActivity` właściwości programu MSBuild. Uruchamianie usług i innych konstrukcji systemu Android nie jest obsługiwana w tej chwili.

`_Gdb` Utworzy docelowej `gdb-symbols` katalogu i skopiuj zawartość urządzenie docelowe `/system/lib` i `$APPDIR/lib` katalogów.


> [!NOTE]
> Zawartość `gdb-symbols` katalogu są powiązane z Android wdrożone do obiektu docelowego i nie będzie automatycznie zastąpione powinien zmienisz element docelowy. (Należy wziąć pod uwagę to usterkę.) Jeśli zmienisz urządzeń docelowych z systemem Android należy ręcznie usunąć tego katalogu.

Na koniec, skopiuj wygenerowany `gdb` poleceń i wykonaj go w powłoki:

```bash
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) bt
#0  0x40082e84 in nanosleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#1  0x4008ffe6 in sleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#2  0x74e46240 in ?? ()
#3  0x74e46240 in ?? ()
(gdb) c
```

<a name="Debug_Builds_without_Fast_Deployment" />

## <a name="debug-builds-without-fast-deployment"></a>Debugowanie kompilacji bez szybkiego wdrażania

Debugowanie kompilacji *z* szybkie wdrożenie działa przez kopiowanie zestawu Android NDK `gdbserver` programu do wdrożenia Fast `.__override__` katalogu. Po wyłączeniu szybkiego wdrożenia tego katalogu może nie istnieć.

Istnieją dwa obejścia:

-   Ustaw `debug.mono.log` właściwości systemu, aby `.__override__` jest tworzony katalog.
-   Obejmują `gdbserver` w ramach Twojej `.apk`.

### <a name="setting-the-debugmonolog-system-property"></a>Ustawienie `debug.mono.log` właściwości systemu

Aby ustawić `debug.mono.log` właściwości systemu, należy zastosować `adb` polecenia:

```bash
$ adb shell setprop debug.mono.log gc
```

Po ustawieniu właściwości systemu wykonać `_Gdb` wydruku i docelowe `gdb` polecenie jako z Debug kompiluje z konfiguracją szybkiego wdrażania:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```


### <a name="including-gdbserver-in-your-app"></a>W tym `gdbserver` w aplikacji

Aby uwzględnić `gdbserver` aplikacji:

1. Znajdź `gdbserver` w ramach Twojego zestawu NDK systemu Android (powinna być w **$ANDROID\_zestawu NDK\_ścieżki/wbudowane/android — arm/gdbserver/gdbserver**) i skopiuj go do katalogu projektu.

2. Zmień nazwę `gdbserver` do **libs/armeabi-v7a/libgdbserver.so**.

3. Dodaj **libs/armeabi-v7a/libgdbserver.so** do projektu z **Akcja kompilacji** z `AndroidNativeLibrary`.

4. Ponowne skompilowanie i ponowne zainstalowanie aplikacji.

Po ponownej instalacji aplikacji należy wykonać `_Gdb` wydruku i docelowe `gdb` polecenie jako z Debug kompiluje z konfiguracją szybkiego wdrażania:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```

<a name="Release_Builds" />

## <a name="release-builds"></a>Kompilacje wydania

`gdb` Obsługa wymaga trzy czynności:

1.  `INTERNET` Uprawnienia.
2.  Włączono debugowanie aplikacji.
3.  Dostępny `gdbserver`.

`INTERNET` Uprawnienie jest domyślnie włączone w aplikacjach debugowania. Jeśli jeszcze nie jest obecny w aplikacji można dodać go przez edytowanie **Properties/AndroidManifest.xml** lub edytując [właściwości projektu](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest/).

Debugowanie aplikacji można ją włączyć przez dowolnego z tych ustawień [ApplicationAttribute.Debugging](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Debuggable/) właściwości atrybutu niestandardowego `true`, lub edytując **Properties/AndroidManifest.xml** i ustawienia `//application/@android:debuggable` atrybutu `true`:

```xml
<application android:label="Example.Name.Here" android:debuggable="true">
```

Dostępne `gdbserver` mogą być udostępniane przez następujące [Debug kompiluje bez szybkiego wdrożenia](#Debug_Builds_without_Fast_Deployment) sekcji.

Jeden Marszczenie: `_Gdb` docelowy programu MSBuild będzie kill wcześniej uruchomione wystąpienia aplikacji. To nie będzie działać dla elementów docelowych wstępnie Android 4.0.

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="monopmip-doesnt-work"></a>`mono_pmip` nie działa

`mono_pmip` — Funkcja (przydatne w przypadku [uzyskiwania ramek stosu zarządzanych](http://www.mono-project.com/Debugging#Debugging_with_GDB)) są eksportowane z `libmonosgen-2.0.so`, który `_Gdb` docelowych nie będzie ściągać w dół. (Ten problem zostanie rozwiązany w przyszłej wersji.)

Aby włączyć wywoływanie funkcji znajduje się w `libmonosgen-2.0.so`, skopiuj go z tego urządzenia do `gdb-symbols` katalogu:

```bash
$ adb pull /data/data/Mono.Android.DebugRuntime/lib/libmonosgen-2.0.so Project/gdb-symbols
```

Następnie uruchom ponownie sesję debugowania.

### <a name="bus-error-10-when-running-the-gdb-command"></a>Błąd magistrali: 10 podczas uruchamiania `gdb` polecenia

Gdy `gdb` błędy się przy użyciu polecenia `"Bus error: 10"`, uruchom ponownie urządzenie z systemem Android.

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
Bus error: 10
$
```

### <a name="no-stack-trace-after-attach"></a>Dołącz śladów stosu po

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
(gdb) bt
No stack.
```

Zazwyczaj jest to znak który zawartość `gdb-symbols` katalogu nie są zsynchronizowane z docelowym z systemem Android. (Czy Zmień urządzenie docelowe z systemem Android?)

Usuń `gdb-symbols` katalogu i spróbuj ponownie.
