---
title: Gdzie można ustawić Moje lokalizacji zestawu SDK systemu Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 11/16/2017
ms.openlocfilehash: e7af6635aeec72a9a0e0c01eeb6eb0a77d2a1d7b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Gdzie można ustawić Moje lokalizacji zestawu SDK systemu Android

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W programie Visual Studio, przejdź do **Narzędzia > Opcje > Xamarin > Ustawienia systemu Android** do wyświetlania i Ustaw lokalizację zestawu SDK systemu Android:

[![Przykład karta lokalizacje w preferencjach](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

Domyślna lokalizacja dla każdej ścieżki jest następujący:

- Java Development Kit lokalizacji: 

    **C:\\Program Files\\Java\\jdk1.8.0_131**

- Lokalizacja zestawu SDK systemu android: 

    **C:\\Program Files (x86)\\Android\\zestawu sdk systemu android**

- Lokalizacja zestawu NDK systemu android: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

Należy pamiętać, że numer wersji zestawu NDK może się różnić. Na przykład zamiast z **r13b-android-zestawu ndk**, takich jak może być starsza wersja **r10e-android-zestawu ndk**.

Aby ustawić lokalizacji zestawu SDK systemu Android, wprowadź pełną ścieżkę do katalogu zestawu SDK systemu Android **lokalizacji zestawu SDK systemu Android** pole. Przejdź do lokalizacji zestawu SDK systemu Android w Eksploratorze plików, skopiować ścieżkę na pasku adresu i wklej tę ścieżkę do **lokalizacji zestawu SDK systemu Android** pole.
Na przykład, jeśli lokalizacja zestawu SDK systemu Android w **C:\\użytkowników\\username\\AppData\\lokalnego\\Android\\Sdk**, wyczyść starej ścieżki w  **Lokalizacja zestawu SDK systemu android** wkleić w tej ścieżce, a kliknij **OK**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W programie Visual Studio dla komputerów Mac, przejdź do **Preferencje > Projekty > Lokalizacje SDK > Android**. W **Android** kliknij przycisk **lokalizacje** kartę, aby wyświetlić i Ustaw lokalizację zestawu SDK:

[![Przykład karta lokalizacje w preferencjach](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

Domyślna lokalizacja dla każdej ścieżki jest następujący:

- Lokalizacja zestawu SDK systemu android: 

    **~/Library/Developer/Xamarin/android-SDK-MacOSX**

- Lokalizacja zestawu NDK systemu android: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Java SDK (JDK) lokalizacji: 

    **/usr**

Należy pamiętać, że numer wersji zestawu NDK może się różnić. Na przykład zamiast z **r14b-android-zestawu ndk**, takich jak może być starsza wersja **r10e-android-zestawu ndk**.

Aby ustawić lokalizacji zestawu SDK systemu Android, wprowadź pełną ścieżkę do katalogu zestawu SDK systemu Android **lokalizacji zestawu SDK systemu Android** pole. Możesz wybrać folder zestawu SDK systemu Android w wyszukiwanie, naciśnij klawisz **CTRL +&#8984;+ I** Aby wyświetlić informacje o folderze, kliknij i przeciągnij ścieżkę na prawo od **gdzie:**, skopiować, a następnie wklej go do **zestawu SDK systemu Android Lokalizacja** polu **lokalizacje** kartę. Na przykład, jeśli lokalizacja zestawu SDK systemu Android w **~/Library/Developer/Android/Sdk**, wyczyść starej ścieżki w **lokalizacji zestawu SDK systemu Android** wkleić w tej ścieżce, a kliknij **OK**.

-----
