---
title: Instalacja i wymagania
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: a587935e35882ed1dc68817fbbe1ae3e91200f29
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="installation-and-requirements"></a>Instalacja i wymagania

<script> var inspectorOnLoad = (funkcja) {var primaryTextBase = "Skoroszyty Xamarin & inspektora dla"; var secondaryTextBase = "lub Pobierz dla"; var inspectorDownloadUrlMac = "https://dl.xamarin.com/interactive/XamarinInteractive.pkg"; var inspectorDownloadUrlWin = "https://dl.xamarin.com/interactive/XamarinInteractive.msi";

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = aPrimary; var aWin = aSecondary; var macTextBase = primaryTextBase; var winTextBase = secondaryTextBase;

  Jeśli (/win/i.test(navigator.platform.toLowerCase())) {aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase;}

  aMac.href = inspectorDownloadUrlMac; aMac.text = macTextBase + " Mac"; aWin.href = inspectorDownloadUrlWin; aWin.text = winTextBase + " Windows"; };

document.addEventListener("DOMContentLoaded", inspectorOnLoad);
</script>

## <a name="download-and-installation"></a>Pobierania i instalacji

<ol>
  <li>Pobierz i zainstaluj <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">skoroszytów Xamarin & Inspektor dla komputerów Mac</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">lub pobrania dla systemu Windows</a>).
  </li>
  <li><a href="~/tools/inspector/inspect.md"> Sprawdź własną aplikację!</a>
    </li>
</ol>

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

<table>
<thead>
  <tr>
    <th>Platforma aplikacji</th>
    <th>Obsługa środowiska IDE</th>
    <th>Uwagi</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Mac (Unified)</td>
    <td>Obsługiwane tylko dla komputerów Mac</td>
    <td/>
  </tr>
  <tr>
    <td>iOS (Unified)</td>
    <td>Obsługiwane w XS i Visual Studio</td>
    <td>Procedury kontroli aplikacji systemu iOS z systemu Windows wymaga tej samej wersji Inspektora można także zainstalować na hoście kompilacji Mac.</td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Obsługiwane w XS i Visual Studio</td>
    <td>
      <ul>
        <li>Muszą wskazywać Android > = 4.0.3</li>
        <li>Musi mieć fastdev włączone</li>
        <li>Należy użyć Google, Visual Studio lub Xamarin Android emulatorów. Android emulatorów 7 kontroli w tej chwili jest niedozwolona.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Obsługiwane tylko w programie Visual Studio w systemie Windows</td>
    <td/>
  </tr>
</tbody>
</table>

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Raportowanie błędów

Powinny być zgłaszane usterki bezpośrednio za pomocą programu Visual Studio:

- **Pomoc → Wyślij opinię → Raportowanie problemu**

Podaj następujące informacje:

### <a name="platform-version-information"></a>Informacje o wersji platformy

Te informacje są istotne.

Visual Studio For Mac

- **Informacje programu Visual Studio → Pokaż szczegóły → kopii programu Visual Studio →**
- Wklej do usterek — raport

Xamarin Studio

- **Program Xamarin Studio → o program Xamarin Studio → Pokaż szczegóły → kopiowanie informacji**
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

- **Plik dziennika ujawniania → pomocy**

Visual Studio For Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Zawartość programu Visual Studio `Output` okienko może również służyć jako źródło informacji.

### <a name="project-settings"></a>Ustawienia projektu

Jeśli możesz dołączyć `.csproj` dla projektu chcesz sprawdzić, czy można bardzo przydatne. To jest łatwiejsze niż pytaniem o poszczególnych ustawieniach.

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

Jeśli masz program Visual Studio 2017 Otwórz "Instalator programu Visual Studio" i Znajdź "Pojedynczych składników" dla "Xamarin skoroszytów". Jeśli jest ono zaznaczone, usuń jej zaznaczenie, a następnie kliknij przycisk "Modyfikuj", aby odinstalować.

#### <a name="system-uninstall"></a>System Uninstall

Jeśli zainstalowano skoroszyty & Inspektor samodzielnie przy użyciu pobranego Instalatora, będzie musiała zostać usunięta przez **aplikacje i funkcje** strona Ustawienia systemu Windows 10 lub za pośrednictwem **Dodaj lub usuń programy**w Panelu sterowania w starszych wersjach systemu Windows.

> **Uruchom ustawienia → → systemu → aplikacje i funkcje**

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

