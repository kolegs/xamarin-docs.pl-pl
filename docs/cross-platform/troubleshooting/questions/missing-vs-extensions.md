---
title: Brakuje rozszerzeń programu Visual Studio po instalacji
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: asb3993
ms.author: amburns
ms.date: 03/20/2017
ms.openlocfilehash: e47cfc4de77a6310a81867eefb07c3c1e5cc7060
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="missing-visual-studio-extensions-after-installation"></a>Brakuje rozszerzeń programu Visual Studio po instalacji

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>Komunikat o błędzie: Ten projekt jest niezgodny z bieżącej wersji programu Visual Studio

Upewnij się, że zainstalowano zgodną wersję programu Visual Studio:

-   Visual Studio 2017 (Community, Professional lub Enterprise)
-   Program Visual Studio 2015 (Community, Professional lub Enterprise)

Zobacz też [wymagania dotyczące systemu Windows](~/cross-platform/get-started/requirements.md#windows).

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>Możliwe poprawka 1: Zmień instalacji, aby upewnić się, że są zainstalowane rozszerzenia programu Visual Studio

W niektórych sytuacjach Xamarin Instalator może automatycznie usunąć zaznaczenie opcji instalacji dla rozszerzeń programu Visual Studio. Jeśli jest to przyczyną problemu, należy zainstalować brakuje rozszerzeń programu Visual Studio za pomocą Instalatora **zmiany** polecenia. Na przykład, aby zainstalować rozszerzenia dla programu Visual Studio 2013:

1. Otwórz program Windows **programy i funkcje** w Panelu sterowania.

2. Kliknij prawym przyciskiem myszy **Xamarin** wpis, a następnie wybierz **zmiany**.

3. Kliknij przycisk **dalej**, następnie **zmiany**.

4. Upewnij się, że **Xamarin dla Visual Studio 2013** opcja jest ustawiona do zainstalowania:

    ![](missing-vs-extensions-images/installer.png "Włącz program Xamarin dla opcji instalacji programu Visual Studio 2013")

5. Postępuj zgodnie z instrukcjami w dalszej części kreatora Instalatora.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>Możliwe poprawka 2: Visual Studio, aby ponownie skonfigurować rozszerzenia poproś

1. Sprawdź, czy rozszerzenia Xamarin zostały skopiowane do folderu rozszerzeń programu Visual Studio:

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    Jeśli rozszerzenia są poprawnie zainstalowane (dla wersji 3.1.228), będą 60 elementy w folderze:


    ![](missing-vs-extensions-images/folder.png "Lista zawartości folderu "Xamarin\3.1.228.0" w Eksploratorze")

2. Po potwierdzeniu, że ten folder poprawne, Przekaż programowi Visual Studio próby konfigurowania rozszerzenia ponownie:

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>Poprawka możliwe 3: Spróbuj świeże konieczności ponownej instalacji programu Xamarin

1.  W Panelu sterowania systemu Windows Odinstaluj jedną z następujących czynności, które znajdują się:

    *   Xamarin

    *   Xamarin dla systemu Windows

    *   Xamarin.Android

    *   Xamarin.iOS

    *   Program Xamarin dla Visual Studio

2.  W Eksploratorze usunąć wszelkie pozostałe pliki z folderów rozszerzenia programu Visual Studio Xamarin (wszystkie wersje, w tym zarówno **Program Files** i **Program Files (x86)**):

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3.  Sprawdź również w katalogu "VirtualStore", aby sprawdzić, czy może być kopie "nakładki" żadnym z katalogów rozszerzenia:

    `%LOCALAPPDATA%\VirtualStore`

4.  Otwórz Edytor rejestru (regedit).

5.  Wyszukaj ten klucz:

    _HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6.  Znajdź i Usuń wpisy, które odpowiada tego wzorca:

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7.  Wyszukaj ten klucz:

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8.  Usuwanie wpisów, które wygląda jak mogą być one związane z Xamarin. Na przykład co w tym, które umożliwiają spowodować problemy w starszej wersji programu Xamarin:

    _Mono.VisualStudio.Shell,1.0_

9.  Ponowne uruchomienie komputera.

10.  Ponownie zainstaluj bieżącą wersję stabilna Xamarin z [visualstudio.com](https://visualstudio.com/xamarin).

## <a name="possible-fix-4-repair-visual-studio-installation"></a>Możliwe poprawka 4: Napraw instalację Visual Studio

1.  Otwórz program Windows **programy i funkcje** w Panelu sterowania.

2.  Kliknij prawym przyciskiem myszy odpowiedni wpis programu Microsoft Visual Studio i wybierz **zmiany**

3.  Kliknij przycisk **naprawy** przycisk w Visual Studio zostanie otwarte okno dialogowe.
