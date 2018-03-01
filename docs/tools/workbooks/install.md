---
title: Instalacja i wymagania
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: dffea94b309b3c945a37a116668486f404f544b3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="installation-and-requirements"></a>Instalacja i wymagania

<script> var inspectorOnLoad = (funkcja) {var primaryTextBase = "Skoroszyty Xamarin dla"; var secondaryTextBase = "lub Pobierz dla"; var inspectorDownloadUrlMac = "https://dl.xamarin.com/interactive/XamarinInteractive.pkg"; var inspectorDownloadUrlWin = " https://DL.xamarin.com/Interactive/XamarinInteractive.msi";

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = aPrimary; var aWin = aSecondary; var macTextBase = primaryTextBase; var winTextBase = secondaryTextBase;

  Jeśli (/win/i.test(navigator.platform.toLowerCase())) {aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase;}

  aMac.href = inspectorDownloadUrlMac; aMac.text = macTextBase + " Mac"; aWin.href = inspectorDownloadUrlWin; aWin.text = winTextBase + " Windows"; };

document.addEventListener("DOMContentLoaded", inspectorOnLoad);
</script>

<a name="install" />

## <a name="download-and-install"></a>Pobierz i zainstaluj

<ol>
  <li>Sprawdź <a href="#Requirements"> wymagania</a> poniżej.</li>
  <li>Pobierz i zainstaluj <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">skoroszytów Xamarin dla komputerów Mac</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">lub pobrania dla systemu Windows</a>).
  </li>
  <li>Uruchom <a href="~/tools/workbooks/workbook.md"> odtwarzanie</a> skoroszytów lub wypróbowywać <a href="https://developer.xamarin.com/workbooks/">przykłady</a>.
    </li>
</ol>

## <a name="requirements"></a>Wymagania

#### <a name="supported-operating-systems"></a>Obsługiwane systemy operacyjne

- **Mac** -OS X 10.11 lub większa
- **Windows** — Windows 7 lub nowszy (z programu Internet Explorer 11 lub nowszej i .NET 4.6.1 lub nowszej)

#### <a name="supported-app-platforms"></a>Platformy obsługiwanej aplikacji

<table>
<thead>
  <tr>
    <th>Platforma aplikacji</th>
    <th>Obsługa systemu operacyjnego</th>
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
    <td>Obsługiwane w Mac i systemu Windows</td>
    <td>
      <ul>
        <li>Xamarin.iOS 11.0 i Xcode 9.0 lub nowszej musi być zainstalowany na komputerach Mac.</li>
        <li>Uruchamianie iOS skoroszytów w systemie Windows wymaga hosta kompilacji Mac z systemem wszystkie powyższe i <a href="~/tools/ios-simulator.md">zdalny symulatora systemu iOS</a> zainstalowany w systemie Windows.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Obsługiwane w Mac i systemu Windows</td>
    <td>Należy użyć emulatora programu Visual Studio lub Xamarin Android Google, z urządzenia wirtualnego > = 5.0</td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Obsługiwane tylko w systemie Windows</td>
    <td/>
  </tr>
  <tr>
    <td>W konsoli (.NET Framework)</td>
    <td>Obsługiwane w Mac i systemu Windows</td>
    <td/>
  </tr>
  <tr>
    <td>W konsoli (.NET Core)</td>
    <td>Obsługiwane w Mac i systemu Windows</td>
    <td/>
  </tr>
</tbody>
</table>

## <a name="reporting-bugs"></a>Raportowanie błędów

Sprawdź [zgłaszania problemów w serwisie GitHub][bugs], znajdują się wszystkie następujące informacje:

### <a name="log-files"></a>Pliki dziennika

Zawsze Dołącz pliki dziennika klienta skoroszyty:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- System Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

1.4.x funkcji również możliwość wybrania pliku dziennika w wyszukiwania (macOS) lub w Eksploratorze (system Windows) bezpośrednio z poziomu menu głównego:

- **Plik dziennika ujawniania → pomocy**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>Ścieżki dziennika dla skoroszytów 1.3 i starszych wersji:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- System Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>Informacje o wersji platformy

Jest bardzo przydatne poznać szczegóły systemu operacyjnego i zainstalowanych produktów Xamarin.

Z poziomu menu głównego w skoroszytach:

* **Pomoc informacje o wersji kopiowania →**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>Instrukcje dla skoroszytów 1.3 i starszych wersji:

Visual Studio For Mac

- **Informacje programu Visual Studio → Pokaż szczegóły → kopii programu Visual Studio →**
- Wklej do usterek — raport

Visual Studio

- **Pomoc → o informacje o kopii → programu Visual Studio**
- Daj nam znać, wersji systemu operacyjnego i czy jest uruchomiona 32-bitowy lub 64-bitowego systemu Windows.

### <a name="samples"></a>Przykłady

Jeśli możesz dołączyć lub połączyć `.workbooks` plik problemów z, może pomóc w rozwiązaniu z usterki szybciej.

### <a name="devices"></a>Urządzenia

Jeśli występują problemy z połączeniem z systemem iOS lub Android skoroszyt, a już zaznaczono [stronę dotyczącą rozwiązywania problemów](~/tools/workbooks/troubleshooting/index.md), musisz wiedzieć:

- Nazwa urządzenia, które próbujesz nawiązać połączenie
- Wersja systemu operacyjnego urządzenia
- System android: Sprawdź, czy używasz x86 emulatora
- System android: Platformy emulatora używasz? Emulator systemu Google?
  Visual Studio Emulator systemu Android? Odtwarzacz dla systemu Xamarin Android?
- System iOS w systemie Windows: jakiej wersji zdalnego Xamarin iOS Simulator zainstalowano (Sprawdź `Add/Remove Programs` w `Control Panel`)?
- System iOS w systemie Windows: również Podaj informacje o wersji platformy dla komputerów Mac hosta kompilacji
- Czy urządzenie ma łączność sieciową (wyboru za pomocą przeglądarki sieci web)?

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Odinstaluj

### <a name="windows"></a>Windows

W zależności od tego, jak nabył skoroszyty & Inspektor należy wykonać dwie procedury dezinstalacji. Sprawdź, czy oba te można całkowicie odinstalować oprogramowania.

#### <a name="visual-studio-installer"></a>Visual Studio Installer

Jeśli masz program Visual Studio 2017 Otwórz **Instalator programu Visual Studio**i Znajdź **pojedynczych składników** dla **skoroszytów Xamarin**. Jeśli jest ono zaznaczone, usuń jej zaznaczenie, a następnie kliknij przycisk **Modyfikuj** do odinstalowania.

#### <a name="system-uninstall"></a>System Uninstall

Jeśli zainstalowano skoroszyty & Inspektor samodzielnie przy użyciu pobranego Instalatora, będzie musiała zostać usunięta przez **aplikacje i funkcje** strona Ustawienia systemu Windows 10 lub za pośrednictwem **Dodaj lub usuń programy**w Panelu sterowania w starszych wersjach systemu Windows.

> **Uruchom ustawienia → → systemu → aplikacje i funkcje**

![](install-images/windows-remove.png "Skoroszyty Xamarin i Inspektora wymienionych w &quot;aplikacji &amp; funkcji&quot;")

**Należy nadal wykonać procedury Instalator programu Visual Studio upewnić się, że skoroszyty & inspektora nie Pobierz ponownie zainstalowany bez wiedzy użytkownika.**

<a name="uninstall-macos" />

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

## <a name="downgrading"></a>Zmiana wersji na starszą

Identyfikator pakietu dla `/Applications/Xamarin Workbooks.app` zmieniła się z `com.xamarin.Inspector` do `com.xamarin.Workbooks` w wersji 1.4 ułatwiające przyszłych podziału instalatorów skoroszyty Xamarin & Inspektor.

Ze względu na błąd w starszych programów instalacyjnych nie jest możliwe obniżyć 1.4 lub nowszej wersji za pomocą 1.3.2 lub starsze pliki instalacyjne.

Zmiany na starszą od 1,4 lub nowszego 1.3.2 lub starsze:

1. [Ręcznie Odinstaluj skoroszyty & Inspektor](#macOS)
2. Uruchom 1.3.2 starszej `.pkg` Instalatora