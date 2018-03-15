---
title: Inspektor instalacji i wymagania
description: "Jak pobrać, zainstalować i używać Inspektora Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: a2e6f254c77ac099b5700543db5763b8bbb44fef
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="inspector-installation-and-requirements"></a>Inspektor instalacji i wymagania

## <a name="download-and-installation"></a>Pobierania i instalacji


# <a name="windowstabvswin"></a>[Windows](#tab/vswin)

1. Pobierz i zainstaluj [skoroszytów Xamarin & inspektora dla systemu Windows](https://dl.xamarin.com/interactive/XamarinInteractive.msi).
2. [Sprawdź własną aplikację!](~/tools/inspector/inspect.md)

# <a name="macostabvsmac"></a>[macOS](#tab/vsmac)

1. Pobierz i zainstaluj [skoroszytów Xamarin & Inspektor dla komputerów Mac](https://dl.xamarin.com/interactive/XamarinInteractive.pkg).
2. [Sprawdź własną aplikację!](~/tools/inspector/inspect.md)

-----

## <a name="requirements"></a>Wymagania

### <a name="supported-operating-systems"></a>Obsługiwane systemy operacyjne

- **Mac** -OS X 10.11 lub większa
- **Windows** — Windows 7 lub nowszy (z programu Internet Explorer 11 lub nowszej i .NET 4.6.1 lub nowszej)

### <a name="supported-ides"></a>Obsługiwane IDEs

- Program Xamarin Studio 6.2 lub większa
- Visual Studio dla komputerów Mac Preview 4 lub nowszej
- Visual Studio 2015 z platformą Xamarin 4.3.x lub nowszej
- Visual Studio 2017 z obciążenia Xamarin

Kontroli aplikacji na żywo jest dostępna dla klientów korporacyjnych.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Platformy obsługiwanej aplikacji

|Platforma aplikacji|Obsługa środowiska IDE|Uwagi|
|--- |--- |--- |
|Mac (Unified)|Obsługiwane tylko dla komputerów Mac|
|iOS (Unified)|Obsługiwane w XS i Visual Studio|Procedury kontroli aplikacji systemu iOS z systemu Windows wymaga tej samej wersji Inspektora można także zainstalować na hoście kompilacji Mac.|
|Android|Obsługiwane w XS i Visual Studio|Muszą wskazywać Android > = 4.0.3, z **fastdev** włączone.<br />Należy użyć Google, Visual Studio lub Xamarin Android emulatorów. Android emulatorów 7 kontroli w tej chwili jest niedozwolona.|
|WPF|Obsługiwane tylko w programie Visual Studio w systemie Windows|


<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Raportowanie błędów

Powinny być zgłaszane usterki bezpośrednio za pomocą programu Visual Studio:

- **Pomoc > Prześlij opinię > Zgłoś Problem**

Podaj następujące informacje:

### <a name="platform-version-information"></a>Informacje o wersji platformy

Te informacje są istotne.

Visual Studio For Mac

- **Visual Studio > o Visual Studio > Pokaż szczegóły > Kopiowanie informacji**
- Wklej do usterek — raport

Xamarin Studio

- **Program Xamarin Studio > o Xamarin Studio > Pokaż szczegóły > Kopiowanie informacji**
- Wklej do usterek — raport

Visual Studio

- **Pomoc > o Visual Studio > Kopiowanie informacji**
- Daj nam znać, wersji systemu operacyjnego i czy jest uruchomiona 32-bitowy lub 64-bitowego systemu Windows.

### <a name="log-files"></a>Pliki dziennika

Zawsze należy dołączyć zarówno IDE i Inspector plików dziennika klienta.

Inspektor klienta

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- System Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4.x funkcji również możliwość wybrania pliku dziennika w wyszukiwania (macOS) lub w Eksploratorze (system Windows) bezpośrednio z poziomu menu głównego:

- **Pomoc > ujawnić pliku dziennika**

Visual Studio For Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Zawartość programu Visual Studio **dane wyjściowe** okienko może również służyć jako źródło informacji.

### <a name="project-settings"></a>Ustawienia projektu

Jeśli możesz dołączyć **.csproj** dla projektu chcesz sprawdzić, czy można bardzo przydatne. To jest łatwiejsze niż pytaniem o poszczególnych ustawieniach.

Również Potwierdź, że jesteś w konfiguracji debugowania.

### <a name="selected-devices"></a>Wybrane urządzenia

Dla systemów Android i iOS jest istotne, że było wiadomo, jakiego urządzenia są debugowania na, jeśli chcesz sprawdzić. Musimy wiedzieć:

- Nazwa urządzenia, jak pokazano w środowiskiem IDE
- Wersja systemu operacyjnego urządzenia
- System android: Sprawdź, czy używasz x86 emulatora
- System android: Platformy emulatora używasz? Emulator systemu Google? Visual Studio Emulator systemu Android? Odtwarzacz dla systemu Xamarin Android?
- Prawidłowo debugowanej aplikacji są wyświetlane i funkcji w urządzeniu?
- Czy urządzenie ma łączność sieciową (wyboru za pomocą przeglądarki sieci web)?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Odinstaluj

### <a name="windows"></a>Windows

W zależności od tego, jak nabył skoroszyty & Inspektor należy wykonać dwie procedury dezinstalacji. Sprawdź, czy oba te można całkowicie odinstalować oprogramowania.

#### <a name="visual-studio-installer"></a>Visual Studio Installer

Jeśli masz program Visual Studio 2017 Otwórz **Instalator programu Visual Studio**i Znajdź **pojedynczych składników** dla **skoroszytów Xamarin**. Jeśli jest ono zaznaczone, usuń jej zaznaczenie, a następnie kliknij przycisk "Modyfikuj", aby odinstalować.

#### <a name="system-uninstall"></a>System Uninstall

Jeśli zainstalowano skoroszyty & Inspektor samodzielnie przy użyciu pobranego Instalatora, będzie musiała zostać usunięta przez **aplikacje i funkcje** strona Ustawienia systemu Windows 10 lub za pośrednictwem **Dodaj lub usuń programy**w Panelu sterowania w starszych wersjach systemu Windows.

> **Start > Ustawienia > System > aplikacje i funkcje**

![](install-images/windows-remove.png "Skoroszyty Xamarin i Inspektora wymienionych w temacie "Aplikacje i funkcje"")

**Należy nadal wykonać procedury Instalator programu Visual Studio upewnić się, że skoroszyty & inspektora nie Pobierz ponownie zainstalowany bez wiedzy użytkownika.**

### <a name="macos"></a>macOS

Począwszy od [1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/), skoroszyty Xamarin & Inspektor mogła zostać usunięta z terminalu, uruchamiając:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Dezinstalator szczegółowe pliki i katalogi spowoduje usunięcie i Pytaj o potwierdzenie przed kontynuowaniem.

Przekaż `-help` argument `uninstall` skryptu dla bardziej zaawansowanych scenariuszy.

Dla starszych wersji należy ręcznie usunąć następujące czynności:

1. Usuń aplikację skoroszytów w `"/Applications/Xamarin Workbooks.app"`
2. Usuń aplikację inspektora w `"Applications/Xamarin Inspector.app"`
2. Usuń dodatki: `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` i `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"`
3. Usuń inspektora i tutaj plików pomocniczych: `/Library/Frameworks/Xamarin.Interactive.framework` i `/Library/Frameworks/Xamarin.Inspector.framework`

