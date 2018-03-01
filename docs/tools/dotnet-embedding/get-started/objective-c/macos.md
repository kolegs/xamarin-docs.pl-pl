---
title: Wprowadzenie do korzystania z macOS
ms.topic: article
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 5e2f14e7b29f85e838563914089743f56239d7bb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-macos"></a>Wprowadzenie do korzystania z macOS


## <a name="what-you-will-need"></a>Dane będą potrzebne

* Postępuj zgodnie z instrukcjami wyświetlanymi w naszym [wprowadzenie do języka Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) przewodnik.

* Zestaw .NET do użycia z **Embeddinator 4000**.

* MacOS Cocoa aplikacji

Kontynuuj po wykonaniu instrukcji w naszym [wprowadzenie do języka Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) przewodnik. Jeśli masz już zestaw .NET możesz przejść bezpośrednio do **4000 Embeddinator przy użyciu** sekcji.

## <a name="creating-a-net-assembly"></a>Tworzenie zestawu .NET

Tworzenie zestawu .NET, należy otworzyć [programu Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) i utworzyć nową **projektu biblioteki .NET** za pomocą ćwiczeń *pliku > nowe rozwiązanie > Inne > .NET > biblioteki*. Kliknij przycisk Dalej i nadaj *pogody* jako *Nazwa projektu*i kliknij przycisk *Utwórz*.

Wykonaj następujące kroki:

1. Usuń **MyClass.cs** pliku i **właściwości** folderu.

2. Kliknij prawym przyciskiem myszy *pogody projektu > Dodaj > Nowy plik.*

3. Wybierz *pustą klasę* i użyj **XAMWeatherFetcher** jako nazwę, a następnie kliknij przycisk Nowy.

4. Zastąp zawartość *XAMWeatherFetcher.cs* następującym kodem:

```csharp
using System;
using System.Json;
using System.Net;

public class XAMWeatherFetcher {

    static string urlTemplate = @"https://query.yahooapis.com/v1/public/yql?q=select%20item.condition%20from%20weather.forecast%20where%20woeid%20in%20(select%20woeid%20from%20geo.places(1)%20where%20text%3D%22{0}%2C%20{1}%22)&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys";
    public string City { get; private set; }
    public string State { get; private set; }

    public XAMWeatherFetcher (string city, string state)
    {
        City = city;
        State = state;
    }

    public XAMWeatherResult GetWeather ()
    {
        try {
            using (var wc = new WebClient ()) {
                var url = string.Format (urlTemplate, City, State);
                var str = wc.DownloadString (url);
                var json = JsonValue.Parse (str)["query"]["results"]["channel"]["item"]["condition"];
                var result = new XAMWeatherResult (json["temp"], json["text"]);
                return result;
            }
        }
        catch (Exception ex) {
            // Log some of the exception messages
            Console.WriteLine (ex.Message);
            Console.WriteLine (ex.InnerException?.Message);
            Console.WriteLine (ex.InnerException?.InnerException?.Message);

            return null;
        }

    }
}

public class XAMWeatherResult {
    public string Temp { get; private set; }
    public string Text { get; private set; }

    public XAMWeatherResult (string temp, string text)
    {
        Temp = temp;
        Text = text;
    }
}
```

Można zauważyć, że `Using System.Json;` zwraca błąd; Aby rozwiązać, to należy wykonać następujące czynności:

1. Kliknij dwukrotnie **odwołania** folderu.

2. Polecenie **pakiety** kartę.

3. Sprawdź **System.Json**.

4. Click **Ok**.

Po powyższym odbywa się wszystkie konieczne jest kompilacji naszego zestawu .NET, klikając *menu Kompiluj > kompilacji wszystkich* lub ⌘ + b. Tworzenie pomyślnie, powinien pojawić się komunikat w pasku stanu top.

Teraz kliknij prawym przyciskiem myszy *pogody* węzeł projektu i wybierz *ujawnić w wyszukiwarce*. Finder przejdź do *bin/Debug* folderu; wewnątrz go znajdziesz **Weather.dll.**

## <a name="using-embeddinator-4000"></a>Przy użyciu Embeddinator 4000

Jeśli zainstalowano Embeddinator-4000 za pomocą naszych Instalatora pkg i uruchomiona, a nową sesję terminala po zakończeniu instalacji, powinno być możliwe do użycia **objcgen** polecenia (w przeciwnym razie można użyć ścieżki bezwzględnej: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`); **objcgen** to narzędzie, które należy wygenerować natywnej biblioteki z zestawu .NET.

Otwórz terminal `cd` do folderu zawierającego Weather.dll i wykonaj **objcgen** z argumentami pokazano poniżej:

```shell
cd /Users/Alex/Projects/Weather/Weather/bin/Debug

objcgen --debug --outdir=output -c Weather.dll
```

Wszystko, co użytkownik musi zostaną umieszczone w **dane wyjściowe** dalej do katalogu *Weather.dll*. Jeśli masz własny zestaw .NET, Zastąp *Weather.dll* go i wykonaj kroki takie same powyżej.

## <a name="using-the-generated-output-in-an-xcode-project"></a>Przy użyciu wygenerowanych danych wyjściowych w projekcie Xcode

Otwórz program Xcode i Utwórz nowy **macOS aplikacji Cocoa** i nadaj mu nazwę **MyWeather**. Kliknij prawym przyciskiem myszy *węzła projektu MyWeather*, wybierz pozycję *Dodaj pliki do "MyWeather"*, przejdź do **dane wyjściowe** Katalog utworzony przez *Embeddinator 4000* i dodaj następujące pliki:

* bindings.h
* embeddinator.h
* glib.h
* mono support.h
* mono_embeddinator.h
* objc support.h
* libWeather.dylib
* Weather.dll

Upewnij się, że **skopiować elementy w razie potrzeby** zaznaczono w panelu Opcje okna dialogowego pliku.

Teraz musimy upewnić się, że **libWeather.dylib** i **Weather.dll** pobranie do pakietu aplikacji:

* Polecenie *węzła projektu MyWeather*.
* Wybierz *fazy kompilacji* kartę.
* Dodaj nową *kopiowania plików*.
* Na *docelowego* wybierz **struktury** i Dodaj **libWeather.dylib**.
* Dodaj nową *kopiowania plików*.
* Na *docelowego* wybierz **pliki wykonywalne**, Dodaj **Weather.dll** i upewnij się, że *kodu logowania kopiowania* zaznaczono.

Teraz otworzyć **ViewController.m** i zastąp jego zawartość z:

```objective-c
#import "ViewController.h"
#import "bindings.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    XAMWeatherFetcher * fetcher = [[XAMWeatherFetcher alloc] initWithCity:@"Boston" state:@"MA"];
    XAMWeatherResult * weather = [fetcher getWeather];

    NSString * result;
    if (weather)
        result = [NSString stringWithFormat:@"%@ °F - %@", weather.temp, weather.text];
    else
        result = @"An error occured";

    NSTextField * textField = [NSTextField labelWithString:result];
    [self.view addSubview:textField];
}

@end
```

Na koniec Uruchom projekt Xcode i zostaną wyświetlone informacje podobne:

![Uruchamianie przykładowych MyWeather](macos-images/weather-from-csharp-macos.png)

Przykład bardziej szczegółowy i lepiej wyglądającej jest dostępny [tutaj](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
