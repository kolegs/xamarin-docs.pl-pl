---
title: Jak wykonać dokładnej odinstalować xamarin dla Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 99fde9330498ee62d3cf6b5910c2cbfae39cfdeb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Jak wykonać dokładnej odinstalować xamarin dla Visual Studio?


1.  W Panelu sterowania systemu Windows Odinstaluj jedną z następujących czynności, które znajdują się:

    -   Xamarin
    -   Xamarin dla systemu Windows
    -   Xamarin.Android
    -   Xamarin.iOS
    -   Program Xamarin dla Visual Studio

2.  W Eksploratorze usunąć wszelkie pozostałe pliki z folderów rozszerzenia programu Visual Studio Xamarin (wszystkie wersje, w tym zarówno _Program Files_ i _Program Files (x86)_):

    _C:\\pliki programów\*\\programu Microsoft Visual Studio 1\*.0\\Common7\\IDE\\rozszerzenia\\Xamarin_

3.  Usuń Visual Studio MEF składnika katalog pamięci podręcznej również:

    _%LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*.0\\ComponentModelCache_

    W rzeczywistości ten krok samodzielnie często jest wystarczająca do rozpoznawania błędów, takich jak:

    -   "Pakietu"XamarinShellPackage"nie został załadowany poprawnie"

    -   "... Plik projektu nie może zostać otwarty. Brak Brak podtypu projektu"

    -   "Odwołanie do obiektu nie jest ustawione na wystąpienie obiektu.  at Xamarin.VisualStudio.IOS.XamarinIOSPackage.Initialize()"

    -   "SetSite nie powiodło się dla pakietu" (w programie Visual Studio _ActivityLog.xml_)

    -   "LegacySitePackage nie powiodło się dla pakietu" (w programie Visual Studio _ActivityLog.xml_)

    (Zobacz też [Wyczyść pamięć podręczną składników MEF](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) rozszerzenie programu Visual Studio.  Zobaczyć [usterki 40781, 19 komentarz](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19) bardziej kontekstu temat nadrzędny problemu w programie Visual Studio, która może spowodować błędy.)

4.  Sprawdź również w _VirtualStore_ katalogu, aby sprawdzić, w przypadku systemu Windows może mieć zapisany żadnego nakładki pliki _rozszerzenia\\Xamarin_ lub _ComponentModelCache_katalogów:

    _%LOCALAPPDATA%\\VirtualStore_

5.  Otwórz Edytor rejestru (`regedit`).

6.  Wyszukaj następujący klucz:

    _Klucz HKEY\_lokalnego\_maszyny\\oprogramowania\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7.  Znajdź i Usuń wpisy, które odpowiada tego wzorca:

    _C:\\pliki programów\*\\programu Microsoft Visual Studio 1\*.0\\Common7\\IDE\\rozszerzenia\\Xamarin_

8.  Wyszukaj ten klucz:

    _Klucz HKEY\_bieżącego\_użytkownika\\oprogramowania\\Microsoft\\VisualStudio\\1\*.0\\ExtensionManager\\PendingDeletions_

9.  Usuwanie wpisów, które wygląda jak mogą być one związane z Xamarin.  Na przykład co w tym, które umożliwiają spowodować problemy w starszej wersji programu Xamarin:

    _Mono.VisualStudio.Shell,1.0_

10. Otwórz administratora `cmd.exe` wiersz polecenia, a następnie uruchom `devenv /setup` i `devenv /updateconfiguration` polecenia dla każdej zainstalowanej wersji programu Visual Studio.  Na przykład dla programu Visual Studio 2015:

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. Ponowne uruchomienie komputera.

12. Ponownie zainstaluj bieżącą wersję stabilna Xamarin przy użyciu z [visualstudio.com](https://visualstudio.com/xamarin/).

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>Dodatkowe kroki rozwiązywania problemów dla "pakiet nie został poprawnie załadowany"

W przypadkach, gdy powyższe kroki nie rozpoznać błąd "pakiet nie został poprawnie załadowany" poniżej przedstawiono kilka więcej kroki w celu.

1.  Utwórz nowe konto użytkownika systemu Windows.

2.  Sprawdź, czy rozszerzenia programu Visual Studio Xamarin obciążenia bez błędów dla nowego użytkownika.

3.  Jeśli poprawnie załadować rozszerzeń problem prawdopodobnie jest spowodowany przez niektórych ustawień przechowywanych dla oryginalnego użytkownika:

    -   **W Eksploratorze** — _LOCALAPPDATA %\\Microsoft\\VisualStudio\\1\*.0_
    -   **W programie regedit** — _HKEY\_bieżącego\_użytkownika\\oprogramowania\\Microsoft\\VisualStudio\\1\*.0_
    -   **W programie regedit** — _HKEY\_bieżącego\_użytkownika\\oprogramowania\\Microsoft\\VisualStudio\\1\*.0\_konfiguracji_

4.  Jeśli te ustawienia przechowywane w rzeczywistości wydają się być problemu, możesz spróbować tworzenie ich kopii zapasowych, a następnie usuwając je.
