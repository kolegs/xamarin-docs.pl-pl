---
title: Brak rozszerzeń programu Visual Studio po instalacji
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: asb3993
ms.author: amburns
ms.date: 03/20/2017
ms.openlocfilehash: 7b1f96807d77d9db0a892c5e78124eb3a9890edc
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780615"
---
# <a name="missing-visual-studio-extensions-after-installation"></a>Brak rozszerzeń programu Visual Studio po instalacji

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>Komunikat o błędzie: Ten projekt jest niezgodny z bieżącą wersją programu Visual Studio

Upewnij się, że zainstalowano zgodną wersję programu Visual Studio:

-   Program Visual Studio 2017 (Community, Professional lub Enterprise)
-   Program Visual Studio 2015 (Community, Professional lub Enterprise)

Zobacz też [wymagania Windows](~/cross-platform/get-started/requirements.md#windows-requirements).

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>Możliwe poprawka 1: Zmień instalację, aby upewnić się, że są zainstalowane rozszerzenia programu Visual Studio

W niektórych sytuacjach Instalatora platformy Xamarin może automatycznie zaznaczenie opcji instalacji dla rozszerzeń programu Visual Studio. Jeżeli jest to przyczyną problemu, należy zainstalować brakujące rozszerzeń programu Visual Studio za pomocą Instalatora programu **zmiany** polecenia. Na przykład, aby zainstalować rozszerzenia dla programu Visual Studio 2013:

1. Otwórz Windows **programy i funkcje** w Panelu sterowania.

2. Kliknij prawym przyciskiem myszy **Xamarin** wpis, a następnie wybierz **zmiany**.

3. Kliknij przycisk **dalej**, następnie **zmiany**.

4. Upewnij się, że **Xamarin dla programu Visual Studio 2013** opcja jest ustawiona do zainstalowania:

    ![](missing-vs-extensions-images/installer.png "Włącz program Xamarin dla opcji instalacji programu Visual Studio 2013")

5. Przejdź przez pozostałą część Kreatora instalacji.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>Możliwe poprawka 2: Pytaj ponownie skonfigurować rozszerzenia programu Visual Studio

1. Sprawdź, jeśli rozszerzenia środowiska Xamarin zostały skopiowane do folderu rozszerzeń programu Visual Studio:

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    Jeśli rozszerzenia są poprawnie zainstalowane (dla wersji 3.1.228), nie będą 60 elementów w folderze:


    ![](missing-vs-extensions-images/folder.png "Listę zawartości folderu \"Xamarin\3.1.228.0\" w Eksploratorze")

2. Po potwierdzeniu, że ten folder wydaje się prawidłowe, Przekaż programowi Visual Studio do wypróbowania konfigurowania rozszerzenia ponownie:

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>Poprawka możliwości 3: Spróbuj świeże konieczności ponownej instalacji Xamarin

1.  W Panelu sterowania Windows Odinstaluj jedną z następujących czynności, które znajdują się:

    *   Xamarin

    *   Platforma Xamarin dla Windows

    *   Xamarin.Android

    *   Xamarin.iOS

    *   Program Xamarin dla Visual Studio

2.  W Eksploratorze, należy usunąć wszelkie pozostałe pliki z folderów rozszerzenie Xamarin Visual Studio (wszystkie wersje, w tym **Program Files** i **Program Files (x86)**):

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3.  Również zapoznać się w katalogu "VirtualStore", aby zobaczyć, jeśli wszystkie kopie "nakładki" żadnym z katalogów rozszerzenia może być:

    `%LOCALAPPDATA%\VirtualStore`

4.  Otwórz Edytor rejestru (regedit).

5.  Wyszukaj ten klucz:

    _HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6.  Znajdowanie i usuwanie wpisów, które pasuje do tego wzorca:

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7.  Wyszukaj ten klucz:

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8.  Usuń wszystkie wpisy, które wyglądają tak, może być powiązany środowiska xamarin. Na przykład, w tym z nich, używany powodować problemy w starszych wersjach programu Xamarin:

    _Mono.VisualStudio.Shell,1.0_

9.  Ponowne uruchomienie komputera.

10.  Ponownie zainstaluj bieżącą wersję stabilną Xamarin z [visualstudio.com](https://visualstudio.com/xamarin).

## <a name="possible-fix-4-repair-visual-studio-installation"></a>Możliwe poprawka 4: Napraw instalację Visual Studio

1.  Otwórz Windows **programy i funkcje** w Panelu sterowania.

2.  Kliknij prawym przyciskiem myszy odpowiedni wpis programu Microsoft Visual Studio, a następnie wybierz **zmiany**

3.  Kliknij przycisk **naprawy** przycisku w programie Visual Studio zostanie otwarte okno dialogowe.
