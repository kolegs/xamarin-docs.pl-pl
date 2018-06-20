---
title: Skoroszyty instalacji i wymagania
description: Ten dokument zawiera opis sposobu Pobierz i zainstaluj skoroszyty Xamarin, dyskutować obsługiwanych platform i wymaganiami systemowymi.
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: a35038a948a89889bbf067a453b7465c1a6a7b49
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268995"
---
# <a name="workbooks-installation-and-requirements"></a>Skoroszyty instalacji i wymagania

<a name="install" />

## <a name="download-and-install"></a>Pobierz i zainstaluj

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Sprawdź [wymagania](#requirements) poniżej.
2. Pobierz i zainstaluj [skoroszytów Xamarin dla systemu Windows](https://dl.xamarin.com/interactive/XamarinInteractive.msi).
3. Uruchom [odtwarzanie](~/tools/workbooks/workbook.md) skoroszytów lub wypróbowywać [próbek](https://developer.xamarin.com/workbooks)

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Sprawdź [wymagania](#Requirements) poniżej.
2. Pobierz i zainstaluj [skoroszytów Xamarin dla komputerów Mac](https://dl.xamarin.com/interactive/XamarinInteractive.pkg).
3. Uruchom [odtwarzanie](~/tools/workbooks/workbook.md) skoroszytów lub wypróbowywać [próbek](https://developer.xamarin.com/workbooks)

-----

## <a name="requirements"></a>Wymagania

#### <a name="supported-operating-systems"></a>Obsługiwane systemy operacyjne

- **Mac** -OS X 10.11 lub większa
- **Windows** — Windows 7 lub nowszy (z programu Internet Explorer 11 lub nowszej i .NET 4.6.1 lub nowszej)

#### <a name="supported-app-platforms"></a>Platformy obsługiwanej aplikacji

|Platforma aplikacji|Obsługa systemu operacyjnego|Uwagi|
|--- |--- |--- |
|Mac|Obsługiwane tylko dla komputerów Mac|
|iOS|Obsługiwane w Mac i systemu Windows|Xamarin.iOS 11.0 i Xcode 9.0 lub nowszej musi być zainstalowany na komputerach Mac. Uruchamianie iOS skoroszytów w systemie Windows wymaga hosta kompilacji Mac z systemem wszystkie powyższe i [zdalny symulatora systemu iOS](~/tools/ios-simulator.md) zainstalowany w systemie Windows.|
|Android|Obsługiwane w Mac i systemu Windows|Należy użyć emulatora programu Visual Studio lub Xamarin Android Google, z urządzenia wirtualnego > = 5.0|
|WPF|Obsługiwane tylko w systemie Windows|
|W konsoli (.NET Framework)|Obsługiwane w Mac i systemu Windows|
|W konsoli (.NET Core)|Obsługiwane w Mac i systemu Windows|


## <a name="reporting-bugs"></a>Raportowanie błędów

Sprawdź [zgłaszania problemów w serwisie GitHub][bugs], znajdują się wszystkie następujące informacje:

### <a name="log-files"></a>Pliki dziennika

Zawsze Dołącz pliki dziennika klienta skoroszyty:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- System Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

1.4.x funkcji również możliwość wybrania pliku dziennika w wyszukiwania (macOS) lub w Eksploratorze (system Windows) bezpośrednio z poziomu menu głównego:

- **Pomoc > ujawnić pliku dziennika**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>Ścieżki dziennika dla skoroszytów 1.3 i starszych wersji:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- System Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>Informacje o wersji platformy

Jest bardzo przydatne poznać szczegóły systemu operacyjnego i zainstalowanych produktów Xamarin.

Z poziomu menu głównego w skoroszytach:

* **Pomoc > skopiować informacje o wersji**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>Instrukcje dla skoroszytów 1.3 i starszych wersji:

Visual Studio For Mac

- **Visual Studio > o Visual Studio > Pokaż szczegóły > Kopiowanie informacji**
- Wklej do usterek — raport

Visual Studio

- **Pomoc > o Visual Studio > Kopiowanie informacji**
- Daj nam znać, wersji systemu operacyjnego i czy jest uruchomiona 32-bitowy lub 64-bitowego systemu Windows.

### <a name="samples"></a>Przykłady

Jeśli możesz dołączyć lub połączyć **.workbooks** plik problemów z, może pomóc w rozwiązaniu z usterki szybciej.

### <a name="devices"></a>Urządzenia

Jeśli występują problemy z połączeniem z systemem iOS lub Android skoroszyt, a już zaznaczono [stronę dotyczącą rozwiązywania problemów](~/tools/workbooks/troubleshooting/index.md), musisz wiedzieć:

- Nazwa urządzenia, które próbujesz nawiązać połączenie
- Wersja systemu operacyjnego urządzenia
- System android: Sprawdź, czy używasz x86 emulatora
- System android: Platformy emulatora używasz? Emulator systemu Google?
  Visual Studio Emulator systemu Android? Odtwarzacz dla systemu Xamarin Android?
- System iOS w systemie Windows: jakiej wersji zdalnego Xamarin iOS Simulator zainstalowano (Sprawdź **Dodaj lub usuń programy** w **Panelu sterowania**)?
- System iOS w systemie Windows: również Podaj informacje o wersji platformy dla komputerów Mac hosta kompilacji
- Czy urządzenie ma łączność sieciową (wyboru za pomocą przeglądarki sieci web)?

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Odinstaluj

### <a name="windows"></a>Windows

W zależności od tego, jak nabył skoroszytów należy wykonać dwie procedury dezinstalacji. Sprawdź, czy oba te można całkowicie odinstalować oprogramowania.

#### <a name="visual-studio-installer"></a>Instalator programu Visual Studio

Jeśli masz program Visual Studio 2017 Otwórz **Instalator programu Visual Studio**i Znajdź **pojedynczych składników** dla **skoroszytów Xamarin**. Jeśli jest ono zaznaczone, usuń jej zaznaczenie, a następnie kliknij przycisk **Modyfikuj** do odinstalowania.

#### <a name="system-uninstall"></a>System Uninstall

Jeśli zainstalowano skoroszyty samodzielnie przy użyciu pobranego Instalatora, będzie musiała zostać usunięta przez **aplikacje i funkcje** strona Ustawienia systemu Windows 10 lub za pośrednictwem **Dodaj lub usuń programy** w formancie Panel w starszych wersjach systemu Windows.

> **Start > Ustawienia > System > aplikacje i funkcje**

![](install-images/windows-remove.png "Skoroszyty Xamarin wymienionych w &quot;aplikacji &amp; funkcji&quot;")

**Należy nadal wykonać procedury Instalator programu Visual Studio się upewnić, że nie Pobierz ponownie zainstalowane skoroszytów bez wiedzy użytkownika.**

<a name="uninstall-macos" />

### <a name="macos"></a>macOS

Począwszy od [1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/), skoroszyty Xamarin mogła zostać usunięta z terminalu, uruchamiając:

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

Identyfikator pakietu dla **Workbooks.app aplikacji/Xamarin** zmieniła się z `com.xamarin.Inspector` do `com.xamarin.Workbooks` w wersji 1.4, skoroszytów i inspektora są teraz pełni podziału.

Ze względu na błąd w starszych programów instalacyjnych nie jest możliwe obniżyć 1.4 lub nowszej wersji za pomocą 1.3.2 lub starsze pliki instalacyjne.

Zmiany na starszą od 1,4 lub nowszego 1.3.2 lub starsze:

1. [Ręcznie Odinstaluj skoroszyty & Inspektor](#macOS)
2. Uruchom 1.3.2 starszej `.pkg` Instalatora
