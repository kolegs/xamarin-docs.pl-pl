---
title: Xamarin.Android Environment
ms.topic: article
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: ee612d4a8982a6ae505b4d329b9abbc84624a1e0
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="xamarinandroid-environment"></a>Xamarin.Android Environment

## <a name="execution-environment"></a>Środowiska wykonawczego

*Środowiska wykonawczego* zestaw zmiennych środowiskowych i właściwości systemu Android, wpływające na wykonywanie programów. Można ustawić właściwości systemu android za pomocą `adb shell setprop` polecenia, aż do zmiennych środowiskowych można ustawić przez ustawienie `debug.mono.env` właściwości systemu:

```shell
## Enable GREF logging
adb shell setprop debug.mono.log gref

## Set the MONO_LOG_LEVEL and MONO_LOG_MASK environment variables
## so that additional Mono messages will be written to `adb logcat`.
adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
```

Android system właściwości są ustawiane dla wszystkich procesów na urządzeniu docelowym.

Począwszy od platformy Xamarin.Android 4.6, zarówno właściwości systemu i zmiennych środowiskowych może być ustawiona lub zastąpiona na podstawie dla aplikacji, dodając *pliku środowiska* do projektu. Środowisko plik jest plikiem zwykły tekst sformatowany Unix z [ **Akcja kompilacji** z `AndroidEnvironment` ](~/android/deploy-test/building-apps/build-process.md).
Plik środowisko zawiera wiersze z formatem *klucz = wartość*.
Komentarze są wiersze, które zaczynają `#`. Puste wiersze są ignorowane.

Jeśli *klucza* rozpoczyna się wielką literę, następnie *klucza* jest traktowany jako zmienną środowiskową i **setenv**(3) jest używana do ustawiania zmiennej środowiskowej do określonego *wartość* podczas uruchamiania procesu.

Jeśli *klucza* rozpoczyna się od małej litery, następnie *klucza* jest traktowany jako właściwość systemu Android i *wartość* jest *wartość domyślna*: Właściwości systemu android, które kontrolują sposób wykonywania platformy Xamarin.Android są najpierw wyszukiwane z serwera właściwości systemu Android, a jeśli wartość nie znajduje się wartość określona w pliku środowiska jest używany. To jest umożliwienie `adb shell setprop` ma być używany do zastępowania wartości, które pochodzą z pliku środowiska w celach diagnostycznych.

## <a name="xamarinandroid-environment-variables"></a>Zmienne środowiskowe platformy Xamarin.Android

Obsługuje platformy Xamarin.Android `XA_HTTP_CLIENT_HANDLER_TYPE` zmiennej, która może być ustawiona albo za pośrednictwem `adb shell setprop debug.mono.env` lub za pośrednictwem `$(AndroidEnvironment)` akcji kompilacji.


### `XA_HTTP_CLIENT_HANDLER_TYPE`

Typu kwalifikowanego zestawu, który musi dziedziczyć z [HttpMessageHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpmessagehandler(v=vs.118).aspx) i jest tworzona z [ `HttpClient()` domyślnego konstruktora](https://msdn.microsoft.com/en-us/library/hh138077(v=vs.118).aspx).

W Xamarin.Android 6.1 tej zmiennej środowiskowej nie jest ustawiona domyślnie i [HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler(v=vs.118).aspx) będą używane.

Alternatywnie wartość `Xamarin.Android.Net.AndroidClientHandler` można określić, aby użyć [ `java.net.URLConnection` ](https://developer.xamarin.com/api/type/Java.Net.URLConnection/) dostępu do sieci, które *może* zezwala na korzystanie z protokołu TLS 1.2, podczas Android obsługuje tę funkcję.

Dodane w Xamarin.Android 6.1.

## <a name="xamarinandroid-system-properties"></a>Xamarin.Android System Properties

Xamarin.Android obsługuje następujące właściwości systemu, które można ustawić albo za pośrednictwem `adb shell setprop` lub za pośrednictwem `$(AndroidEnvironment)` akcji kompilacji.

* `debug.mono.debug`
* `debug.mono.env`
* `debug.mono.gc`
* `debug.mono.log`
* `debug.mono.max_grefc`
* `debug.mono.profile`
* `debug.mono.runtime_args`
* `debug.mono.trace`
* `debug.mono.wref`
* `XA_HTTP_CLIENT_HANDLER_TYPE`

### `debug.mono.debug`

Wartość `debug.mono.debug` systemu właściwość jest liczbą całkowitą. Jeśli `1`, następnie zachowuje "" proces została uruchomiona z `mono --debug`.
To zwykle zawiera informacje plików i wierszy w śladów stosu itp., bez konieczności uruchomienia aplikacji z debugera.

### `debug.mono.env`

Zawiera `|`-przecinkami lista zmiennych środowiskowych.

### `debug.mono.gc`

Wartość `debug.mono.debug` systemu właściwość jest liczbą całkowitą.
Jeśli `1`, a następnie powinny być rejestrowane informacje GC.

Jest to równoważne o `debug.mono.log` właściwość systemu zawiera `gc`.

### `debug.mono.log`

Formanty dodatkowych informacji Xamarin.Android będzie rejestrować do `adb logcat`.
Jest to rozdzielane przecinkami ciąg (`,`), zawierający jedną z następujących wartości:

* `all`: Wydrukować *wszystkie* wiadomości. Jest to rzadko dobrym rozwiązaniem, ponieważ zawiera on `lref` wiadomości.
* `assembly`: Wydrukować `.apk` i zestawu analizowania wiadomości.
* `gc`: Wydrukować komunikatów GC.
* `gref`: Wydrukować odwołania globalnej JNI wiadomości.
* `lref`: Wydrukować JNI lokalnego odwołania wiadomości.  
    *Uwaga*: spowoduje to *naprawdę* spamu `adb logcat`.  
    W wersji 5.1 Xamarin.Android, spowoduje to również utworzenie `.__override__/lrefs.txt` pliku, który można uzyskać *Gigantyczny*.  
    Należy unikać.
* `timing`: Wydrukować niektóre informacje o metodzie chronometrażu. Spowoduje to również utworzenie pliki `.__override__/methods.txt` i `.__override__/counters.txt`.


### `debug.mono.max_grefc`

Wartość `debug.mono.max_grefc` systemu właściwość jest liczbą całkowitą.
Wartość tego *zastępuje* domyślnie wykryto maksymalna liczba GREF dla urządzenia.

*Uwaga:* tylko jest to użyteczne z `adb shell setprop
debug.mono.max_grefc` jako wartość, nie będą dostępne w czasie z **environment.txt** pliku.

### `debug.mono.profile`

`debug.mono.profile` Właściwość systemu umożliwia profilera.
Jest odpowiednikiem i korzysta z tej samej wartości jak `mono --profile` opcji. (Zobacz [ **mono**(1)](http://docs.go-mono.com/?link=man%3amono(1)) man strony, aby uzyskać więcej informacji.)

### `debug.mono.runtime_args`

`debug.mono.runtime_args` Właściwość systemu zawiera dodatkowe opcje, które ma być analizowana przez **mono**.

### `debug.mono.trace`

`debug.mono.trace` Właściwość systemu umożliwia śledzenie.
Jest odpowiednikiem i korzysta z tej samej wartości jak `mono --trace` opcji. (Zobacz [ **mono**(1)](http://docs.go-mono.com/?link=man%3amono(1)) man strony, aby uzyskać więcej informacji.)

Ogólnie rzecz biorąc *nie używaj*. Użyj śledzenia będzie spamu `adb logcat` danych wyjściowych severaly spowolnić działanie programu i Zmień zachowanie programu (maksymalnie i, w tym dodawanie dodatkowych błędów).

*Czasami*, jednak pozwala zbadaniu dodatkowe wykonywane...

### `debug.mono.wref`

`debug.mono.wref` Właściwości systemu umożliwia zastępowanie domyślnie wykryto mechanizm JNI słabe odwołanie. Istnieją dwie obsługiwane wartości:

* `jni`: Użyj JNI słabe odwołania, tworzony przez `JNIEnv::NewWeakGlobalRef()` i niszczone przez `JNIEnv::DeleteWeakGlobalREf()`.
* `java`: Globalne JNI Użyj odwołuje się do których odniesienia `java.lang.WeakReference` wystąpień.

`java` jest używana, domyślnie maksymalnie do 7 interfejsu API i dla interfejsu API 19 (zestaw Kat) ozdobione włączone. (Interfejs API-8 dodany `jni` odwołania i GRAFIK *spowodowało przerwanie* `jni` odwołania.)

Ta właściwość system jest przydatne w przypadku testowania i niektórych form postępowania.
*Ogólnie rzecz biorąc*, nie powinna być zmieniana.

### <a name="xahttpclienthandlertype"></a>XA\_HTTP\_KLIENTA\_OBSŁUGI\_TYPU

Wprowadzona w Xamarin.Android 6.1, tej zmiennej środowiskowej deklaruje domyślnie `HttpMessageHandler` implementację, która będzie używana przez `HttpClient`. Domyślnie ta zmienna nie jest ustawiona, i użyje Xamarin.Android `HttpClientHandler`.

```shell
XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
```

> [!NOTE]
> Podstawowym urządzeniu z systemem Android muszą obsługiwać protokół TLS 1.2.
Obsługa systemu android 5.0 i nowszych protokołu TLS 1.2


## <a name="example"></a>Przykład

```shell
## Comments are lines which start with '#'
## Blank lines are ignored.


## Enable GREF messages to `adb logcat`
debug.mono.log=gref

## Clear out a Mono environment variable to decrease logging
MONO_LOG_LEVEL=
```



## <a name="related-links"></a>Linki pokrewne

- [Transport Layer Security](~/cross-platform/app-fundamentals/transport-layer-security.md)
