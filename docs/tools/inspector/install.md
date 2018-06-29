---
title: Inspektor instalacji i wymagania
description: Ten dokument zawiera opis sposobu instalowania inspektora Xamarin i omówiono obsługiwany system operacyjny, IDEs i platform aplikacji.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: 690329aa1577c66b3aa2794342a8e367477d3a74
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066926"
---
# <a name="inspector-installation-and-requirements"></a>Inspektor instalacji i wymagania

## <a name="download-and-installation"></a>Pobierania i instalacji

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Pobierz i zainstaluj [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/) i wybierz **Mobile development z platformą .NET** obciążenia.
1. [Zaloguj się](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio) umożliwiające subskrypcji Enterprise.
1. [Sprawdź](~/tools/inspector/inspect.md) własną aplikację!

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Pobierz i zainstaluj [programu Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).
1. [Zaloguj się](https://docs.microsoft.com/visualstudio/mac/activation) umożliwiające subskrypcji Enterprise.
1. [Sprawdź](~/tools/inspector/inspect.md) własną aplikację!

-----

## <a name="requirements"></a>Wymagania

### <a name="supported-operating-systems"></a>Obsługiwane systemy operacyjne

- **Mac** -OS X 10.11 lub większa
- **Windows** — Windows 7 lub nowszy (z programu Internet Explorer 11 lub nowszej i .NET 4.6.1 lub nowszej)

### <a name="supported-ides"></a>Obsługiwane IDEs

- Visual Studio for Mac
- Visual Studio 2017 z **Mobile development z platformą .NET** obciążenia

Kontroli aplikacji na żywo jest dostępna dla klientów korporacyjnych.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Platformy obsługiwanej aplikacji

|Platforma aplikacji|Obsługa środowiska IDE|Uwagi|
|--- |--- |--- |
|Mac|Obsługiwane tylko w programie Visual Studio dla komputerów Mac|
|iOS|Obsługiwane w programie Visual Studio 2017 i Visual Studio dla komputerów Mac| |
|Android|Obsługiwane w programie Visual Studio 2017 i Visual Studio dla komputerów Mac|Muszą wskazywać Android > = 4.0.3, z **fastdev** włączone.<br />Należy użyć Google, Visual Studio lub Xamarin Android emulatorów. Android emulatorów 7 kontroli w tej chwili jest niedozwolona.|
|WPF|Obsługiwane tylko w programie Visual Studio 2017 r.|

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
