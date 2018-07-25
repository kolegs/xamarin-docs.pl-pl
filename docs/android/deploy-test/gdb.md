---
title: GDB
ms.prod: xamarin
ms.assetid: CD0BE462-FA38-4881-B481-82AD05B3B8FE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 886cc1de87bd8225bd0389d2e7b84b546ffb39d7
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241500"
---
# <a name="gdb"></a>GDB

## <a name="overview"></a>Omówienie

Rozszerzenie Xamarin.Android 4.10 wprowadzone częściowa obsługa przy użyciu `gdb` przy użyciu `_Gdb` programu MSBuild. 

> [!NOTE]
> `gdb` Pomoc techniczna wymaga zainstalowania zestawu Android NDK.

Istnieją trzy sposoby na wykorzystanie `gdb`:

1.  [Zwiększono wydajność debugowania kompilacji za pomocą Fast Deployment włączone](#Debug_Builds_with_Fast_Deployment) .
1.  [Zwiększono wydajność debugowania kompilacji dzięki Fast Deployment wyłączone](#Debug_Builds_without_Fast_Deployment) .
1.  [Kompilacje wydania](#Release_Builds) .


Gdy coś pójdzie źle, zobacz [Rozwiązywanie problemów](#Troubleshooting) sekcji.

<a name="Debug_Builds_with_Fast_Deployment" />

### <a name="debug-builds-with-fast-deployment"></a>Debugowanie kompilacji przy użyciu szybkiego wdrażania

Podczas tworzenia i wdrażania debugowania kompilacji dzięki Fast Deployment włączone, `gdb` mogą być dołączane za pomocą `_Gdb` programu MSBuild.

Najpierw zainstaluj aplikację. Można to zrobić za pomocą środowiska IDE lub za pomocą wiersza polecenia:

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

`_Gdb` Docelowy uruchomi uruchamiania dowolnego działania zadeklarowane w obrębie swojej `AndroidManifest.xml` pliku. Aby jawnie określić które działania, aby uruchamiać, użyj `RunActivity` właściwości programu MSBuild. Uruchamianie usług i innych konstrukcji systemu Android nie jest obsługiwana w tej chwili.

`_Gdb` Utworzy docelowej `gdb-symbols` katalogu i skopiuj zawartość elementu docelowego `/system/lib` i `$APPDIR/lib` istnieje katalogów.


> [!NOTE]
> Zawartość `gdb-symbols` katalogu są powiązane z obiektem docelowym dla systemu Android została wdrożona na i nie będzie automatycznie zastąpione powinien zmienisz element docelowy. (Należy wziąć pod uwagę to usterka.) Jeśli zmienisz urządzenia docelowe dla systemu Android, należy ręcznie usunąć tego katalogu.

Na koniec skopiuj wygenerowany `gdb` polecenia, a następnie uruchomić go w powłoce:

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

## <a name="debug-builds-without-fast-deployment"></a>Debugowanie kompilacji bez szybkie wdrażanie

Debugowanie kompilacji *z* Fast Deployment działają przez kopiowanie zestawu Android NDK `gdbserver` programu do Fast Deployment `.__override__` katalogu. Gdy Fast Deployment jest wyłączona, ten katalog może nie istnieć.

Istnieją dwa obejścia problemu:

-   Ustaw `debug.mono.log` właściwości systemu, aby `.__override__` katalog jest tworzony.
-   Obejmują `gdbserver` w ramach Twojej `.apk`.

### <a name="setting-the-debugmonolog-system-property"></a>Ustawienie `debug.mono.log` właściwości systemu

Aby ustawić `debug.mono.log` właściwości systemu, należy zastosować `adb` polecenia:

```bash
$ adb shell setprop debug.mono.log gc
```

Po ustawieniu właściwości systemu wykonaj `_Gdb` obiektu docelowego i drukować `gdb` polecenia, jak debugowanie kompilacji za pomocą konfiguracji Fast Deployment:

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


### <a name="including-gdbserver-in-your-app"></a>W tym `gdbserver` w swojej aplikacji

Aby uwzględnić `gdbserver` w aplikacji:

1. Znajdź `gdbserver` w ramach Twojego zestawu Android NDK (powinien znajdować się w **$ANDROID\_zestawu NDK\_ŚCIEŻKA/wstępnie/android-arm/serwera gdbserver/serwera gdbserver**) i skopiować je do katalogu projektu.

2. Zmień nazwę `gdbserver` do **libs/armeabi-v7a/libgdbserver.so**.

3. Dodaj **libs/armeabi-v7a/libgdbserver.so** do projektu przy użyciu **Akcja kompilacji** z `AndroidNativeLibrary`.

4. Ponownie skompiluj i zainstaluj ponownie aplikację.

Po ponownej instalacji aplikacji wykonywanie `_Gdb` obiektu docelowego i drukować `gdb` polecenia, jak debugowanie kompilacji za pomocą konfiguracji Fast Deployment:

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

`gdb` Dział pomocy technicznej wymaga trzy rzeczy:

1.  `INTERNET` Uprawnień.
2.  Włączyć debugowanie aplikacji.
3.  Dostępne `gdbserver`.

`INTERNET` Uprawnienie jest domyślnie włączone w debugowaniu aplikacji. Jeśli jeszcze nie jest obecny w aplikacji można dodać go albo poprzez edycję **Properties/AndroidManifest.xml** lub też edytując [właściwości projektu](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest).

Debugowanie aplikacji, można włączyć albo ustawiając [ApplicationAttribute.Debugging](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Debuggable/) właściwość atrybutu niestandardowego `true`, lub też edytując **Properties/AndroidManifest.xml** i ustawienia `//application/@android:debuggable` atrybutu `true`:

```xml
<application android:label="Example.Name.Here" android:debuggable="true">
```

Dostępne `gdbserver` mogą być udostępniane przez następujące [Debug kompiluje bez Fast Deployment](#Debug_Builds_without_Fast_Deployment) sekcji.

Jeden Marszczenie: `_Gdb` programu MSBuild będą kill wcześniej uruchomione wystąpienia aplikacji. To nie będzie działać dla elementów docelowych wstępnie systemu Android w wersji 4.0.

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="monopmip-doesnt-work"></a>`mono_pmip` nie działa

`mono_pmip` — Funkcja (przydatne w przypadku [uzyskanie ramek stosu zarządzanych](http://www.mono-project.com/docs/debug+profile/debug/#debugging-with-gdb)) są eksportowane z `libmonosgen-2.0.so`, który `_Gdb` docelowego nie będzie ściągać. (Ten problem zostanie rozwiązany w przyszłej wersji.)

Aby włączyć funkcje wywoływania znajdujące się w `libmonosgen-2.0.so`, skopiuj go z tego urządzenia do `gdb-symbols` katalogu:

```bash
$ adb pull /data/data/Mono.Android.DebugRuntime/lib/libmonosgen-2.0.so Project/gdb-symbols
```

Następnie uruchom ponownie sesję debugowania.

### <a name="bus-error-10-when-running-the-gdb-command"></a>Service Bus błąd: 10 podczas uruchamiania `gdb` polecenia

Gdy `gdb` polecenia występuje błąd z `"Bus error: 10"`, uruchom ponownie urządzenie z systemem Android.

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
Bus error: 10
$
```

### <a name="no-stack-trace-after-attach"></a>Brak śladu stosu po dołączania

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
(gdb) bt
No stack.
```

Zazwyczaj jest to znak, zawartość `gdb-symbols` katalogu nie są zsynchronizowane z docelowym systemem Android. (Czy Zmień urządzenie docelowe dla systemu Android?)

Usuń `gdb-symbols` katalog i spróbuj ponownie.
