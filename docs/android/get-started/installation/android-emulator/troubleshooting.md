---
title: Rozwiązywanie problemów z emulatorem problemy z instalacją
description: W tym artykule wyjaśniono, jak zdiagnozować i rozwiązać problemy, które mogą wystąpić, gdy za pomocą Menedżera urządzeń systemu Android.
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: 4cbd7d7626d114856d6c68fe7bc9feb7c2a5abc2
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734057"
---
# <a name="troubleshooting-emulator-setup-problems"></a>Rozwiązywanie problemów z emulatorem problemy z instalacją

_W tym artykule wyjaśniono, jak zdiagnozować i rozwiązać problemy, które mogą wystąpić podczas konfigurowania emulatora systemu Android przy użyciu Menedżera urządzeń systemu Android. Aby uzyskać informacje o rozwiązywaniu problemów podczas uruchamiania emulatora systemu Android, zobacz [Google Android Emulator Rozwiązywanie problemów z](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md)._

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="android-sdk-in-non-standard-location"></a>Zestaw SDK systemu android w lokalizacji niestandardowej

Zazwyczaj zestaw SDK systemu Android jest instalowany w następującej lokalizacji:

**C:\\Program Files (x86)\\Android\\zestawu sdk systemu android**

Jeśli zestaw SDK nie jest zainstalowany w tej lokalizacji, błąd ten może wystąpić podczas uruchamiania Menedżera urządzeń systemu Android:

![Błąd wystąpienia zestawu SDK systemu android](troubleshooting-images/win/01-sdk-error.png)

Aby obejść ten problem, wykonaj następujące czynności:

1. Na pulpicie systemu Windows, przejdź do **C:\\użytkowników\\*username*\\AppData\\mobilny\\XamarinDeviceManager**:

    ![Lokalizacja pliku dziennika Menedżera urządzeń systemu android](troubleshooting-images/win/02-log-files.png)

2. Kliknij dwukrotnie, aby otworzyć jeden z plików dziennika i Znajdź **ścieżkę pliku konfiguracyjnego**. Na przykład:

    [![Ścieżka pliku konfiguracji w pliku dziennika](troubleshooting-images/win/03-config-file-path-sml.png)](troubleshooting-images/win/03-config-file-path.png#lightbox)

3. Przejdź do tej lokalizacji i kliknij dwukrotnie **user.config** go otworzyć. 

4. W **user.config**, zlokalizuj **&lt;ustawienia użytkownika&gt;** element i Dodaj **AndroidSdkPath** do niej atrybut. Ten atrybut ustawiony na ścieżkę zainstalowanym zestawu SDK systemu Android na komputerze, a następnie zapisz plik. Na przykład **&lt;ustawienia użytkownika&gt;** będzie wyglądać podobnie do następującej, jeśli zainstalowano zestaw SDK systemu Android w **C:\\programy\\Android\\zestawuSDK**:
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

Po wprowadzeniu tej zmiany do **user.config**, można uruchomić Menedżera urządzeń systemu Android.

## <a name="snapshot-disables-wifi-on-android-oreo"></a>Migawki wyłącza sieci Wi-Fi na Oreo systemu Android

Jeśli masz AVD skonfigurowano dla systemu Android Oreo symulowane dostępu do sieci Wi-Fi, ponowne uruchomienie AVD po migawki może spowodować dostępu do sieci Wi-Fi ma zostać wyłączona.

Aby obejść ten problem

1. Wybierz AVD w Menedżerze urządzeń systemu Android.

2. W menu opcji dodatkowych kliknij **ujawnić w Eksploratorze**.

3. Przejdź do **migawki > default_boot**.

4. Usuń **snapshot.pb** pliku:

    ![Lokalizacja pliku snapshot.pb](troubleshooting-images/win/05-delete-snapshot.png)

5. Uruchom ponownie AVD. 

Po wprowadzeniu tych zmian, AVD zostanie uruchomiony ponownie w stanie, który umożliwia sieci Wi-Fi do pracy ponownie.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="snapshot-disables-wifi-on-android-oreo"></a>Migawki wyłącza sieci Wi-Fi na Oreo systemu Android

Jeśli masz AVD skonfigurowano dla systemu Android Oreo symulowane dostępu do sieci Wi-Fi, ponowne uruchomienie AVD po migawki może spowodować dostępu do sieci Wi-Fi ma zostać wyłączona.

Aby obejść ten problem

1. Wybierz AVD w Menedżerze urządzeń systemu Android.

2. W menu opcji dodatkowych kliknij **ujawnić w wyszukiwarce**.

3. Przejdź do **migawki > default_boot**.

4. Usuń **snapshot.pb** pliku:

    [![Lokalizacja pliku snapshot.pb](troubleshooting-images/mac/02-delete-snapshot-sml.png)](troubleshooting-images/mac/02-delete-snapshot.png#lightbox)

5. Uruchom ponownie AVD. 

Po wprowadzeniu tych zmian, AVD zostanie uruchomiony ponownie w stanie, który umożliwia sieci Wi-Fi do pracy ponownie.


-----

## <a name="generating-a-bug-report"></a>Generowanie raport o usterce

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Jeśli napotkasz problem przy użyciu systemu Android Menedżera urządzeń którego nie można rozwiązać przy użyciu powyższego wskazówki dotyczące rozwiązywania problemów, klikając prawym przyciskiem myszy pasek tytułu i wybierając pliku raport o usterce **wygenerować raport o usterce**:

[![Lokalizacja elementu menu dla składanie raport o usterce](troubleshooting-images/win/04-bug-report-sml.png)](troubleshooting-images/win/04-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Jeśli napotkasz problem przy użyciu systemu Android Menedżera urządzeń którego nie można rozwiązać przy użyciu powyższego wskazówki dotyczące rozwiązywania problemów, klikając pliku raport o usterce **Pomoc > Generowanie raportu o usterce**:

[![Lokalizacja elementu menu dla składanie raport o usterce](troubleshooting-images/mac/01-bug-report-sml.png)](troubleshooting-images/mac/01-bug-report.png#lightbox)

-----
