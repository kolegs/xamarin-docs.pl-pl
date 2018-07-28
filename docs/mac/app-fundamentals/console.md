---
title: Za pomocą powiązań platformy Xamarin.Mac dla aplikacji konsoli
description: Tworzenie aplikacji konsolowej bezobsługowego dostęp do natywnych interfejsów API systemu macOS za pomocą platformy Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 9E52B4CC-F530-4B3E-984A-7F5719A6D528
ms.technology: xamarin-mac
author: migueldeicaza
ms.author: miguel
ms.date: 07/27/2018
ms.openlocfilehash: 135ef06cd044235023c3acc970c8e7a33f144b47
ms.sourcegitcommit: d0da5ce4158239abd2b36f67550e9b475055766a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320814"
---
# <a name="xamarinmac-bindings-in-console-apps"></a>Powiązania platformy Xamarin.Mac w aplikacji konsoli

Istnieją pewne scenariusze, w której chcesz korzystać z niektórych natywnych interfejsów API firmy Apple w języku C# do tworzenia aplikacji bezobsługowego &ndash; taki, który nie ma interfejsu użytkownika &ndash; przy użyciu języka C#.

Szablony projektu dla aplikacji dla komputerów Mac obejmują wywołania `NSApplication.Init()` następuje po wywołaniu `NSApplication.Main(args)`, zwykle wygląda następująco:

```csharp
static class MainClass {
    static void Main (string [] args)
    {
        NSApplication.Init ();
        NSApplication.Main (args);
    }
}
```

Wywołanie `Init` przygotowuje środowiska uruchomieniowego platformy Xamarin.Mac wywołanie `Main(args)` uruchamia cocoa dla aplikacji głównej pętli, przygotowując aplikację do odbierania zdarzeń klawiatury oraz myszy i pokazuj okna głównego aplikacji.   Wywołanie `Main` również będzie podejmować próby lokalizowanie zasobów Cocoa, należy przygotować okna toplevel i oczekuje, że program ma być częścią pakietu aplikacji (programy rozpowszechniane w katalogu z `.app` rozszerzenie i bardzo określonego układu).

Bezobsługowe aplikacji nie ma potrzeby użytkownika interfejsu, a nie powinny być uruchamiane w ramach pakietu aplikacji.

## <a name="creating-the-console-app"></a>Tworzenie aplikacji konsoli

Dlatego zaleca się rozpoczynać regularnego typu Projekt konsoli .NET.

Należy wykonać kilka czynności:

- Utwórz pusty projekt.
- Odwołanie do biblioteki Xamarin.Mac.dll.
- Zapewnij niezarządzanej zależności projektu.

Te kroki są wyjaśnione bardziej szczegółowo poniżej:

### <a name="create-an-empty-console-project"></a>Utwórz pusty projekt konsoli

Utwórz nowy projekt konsoli .NET, upewnij się, że jest .NET i nie .NET Core, jako Xamarin.Mac.dll nie działa w ramach środowiska uruchomieniowego .NET Core, działa ze środowiskiem uruchomieniowym Mono.

### <a name="reference-the-xamarinmac-library"></a>Odwołanie do biblioteki platformy Xamarin.Mac

Aby skompilować kod, można odwoływać się do `Xamarin.Mac.dll` zestawu z tego katalogu: `/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full`

Aby to zrobić, przejdź do odwołań projektu, wybierz opcję **zestawu .NET** kartę, a następnie kliknij przycisk **Przeglądaj** przycisk, aby zlokalizować plik w systemie plików.  Przejdź do wymienionej powyżej, a następnie wybierz **Xamarin.Mac.dll** z tego katalogu.

Umożliwi to dostęp do interfejsów API Cocoa w czasie kompilacji.   W tym momencie można dodawać `using AppKit` na początku pliku i wywołania `NSApplication.Init()` metody.   Istnieje tylko jeden krok, aby można było uruchomić aplikację.

### <a name="bring-the-unmanaged-support-library-into-your-project"></a>Przenieś biblioteki niezarządzanej pomocy technicznej do projektu

Aplikacja jest uruchamiana, musisz najpierw przenieść `Xamarin.Mac` Biblioteka pomocy technicznej do projektu.   Aby to zrobić, Dodaj nowy plik do projektu (Opcje projektu wybierz **Dodaj**, a następnie **Dodaj istniejący plik**) i przejdź do tego katalogu:

`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib`

W tym miejscu, wybierz plik **libxammac.dylib**.   Będą oferowane wybór kopiowania, łączenie lub przenoszenia.   Czy mogę osobiście, takie jak łączenie, ale kopiowanie działa również.    Musisz wybrać plik i w okienku właściwości (wybierz **Widok > okienka > właściwości** Jeśli konsola właściwość nie jest widoczna), przejdź do **kompilacji** sekcji i ustaw **skopiuj dane wyjściowe Katalog** ustawienie **Kopiuj Jeśli nowszy**.

Teraz możesz uruchomić aplikację platformy Xamarin.Mac.

Wynik w katalogu bin będzie wyglądać następująco:

```
Xamarin.Mac.dll
Xamarin.Mac.pdb
consoleapp.exe
consoleapp.pdb
libxammac.dylib
```

Aby uruchomić tę aplikację, potrzebujesz tych plików w tym samym katalogu.

## <a name="building-a-standalone-application-for-distribution"></a>Tworzenie aplikacji autonomicznej do dystrybucji

Warto rozpowszechniać pojedynczy plik wykonywalny dla użytkowników.  Aby to zrobić, można użyć `mkbundle` narzędzie, aby przekształcić różnych plików w niezależna pliku wykonywalnego.

Najpierw upewnij się, że aplikacja kompiluje i uruchamia.   Gdy jesteś zadowolony z wyników, możesz uruchomić następujące polecenie z wiersza polecenia:

```
$ mkbundle --simple -o /tmp/consoleapp consoleapp.exe --library libxammac.dylib --config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/config --machine-config /Library/Frameworks/Mono.framework/Versions/Current//etc/mono/4.5/machine.config
[Output from the bundling tool]
$ _
```

W wywołaniu wiersza polecenia z powyższych opcji `-o` jest używana do określenia wygenerowanych danych wyjściowych, w tym przypadku możemy przekazywane `/tmp/consoleapp`.   Ten element jest teraz aplikacją, którą można dystrybuować i nie ma zależności zewnętrznych dla platformy Mono lub platformy Xamarin.Mac, jest w pełni niezależnym pliku wykonywalnego.

Wiersza polecenia ręcznie określić **machine.config** plik do użycia, a plik konfiguracji mapowania biblioteki całego systemu.   Nie są niezbędne dla wszystkich aplikacji, ale jest wygodne powiązać je, ponieważ są one używane, gdy używasz więcej możliwości platformy .NET

## <a name="project-less-builds"></a>Bez projektu kompilacji

Nie potrzebujesz pełnej projekt do tworzenia aplikacji platformy Xamarin.Mac niezależna, również umożliwia proste Unix pliki reguł programu make swoją pracę.   Poniższy przykład pokazuje, jak skonfigurować pliku reguł programu make dla aplikacji proste wiersza polecenia:

```
XAMMAC_PATH=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full/
DYLD=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib
MONODIR=/Library/Frameworks/Mono.framework/Versions/Current/etc/mono

all: consoleapp.exe

consoelapp.exe: consoleapp.cs Makefile
    mcs -g -r:$(XAMMAC_PATH)/Xamarin.Mac.dll consoleapp.cs
    
run: consoleapp.exe
    MONO_PATH=$(XAMMAC_PATH) DYLD_LIBRARY_PATH=$(DYLD) mono --debug consoleapp.exe $(COMMAND)


bundle: consoleapp.exe
    mkbundle --simple consoleapp.exe -o ncsharp -L $(XAMMAC_PATH) --library $(DYLD)/libxammac.dylib --config $(MONODIR)/config --machine-config $(MONODIR)/4.5/machine.config
```

Powyższe `Makefile` zawiera trzy elementy docelowe:

- `make` zostanie skompilowany program
- `make run` utworzysz i uruchomić program w bieżącym katalogu
- `make bundle` Spowoduje to utworzenie niezależna pliku wykonywalnego
