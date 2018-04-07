---
title: Jak ręcznie zsynchronizować Xamarin licencji?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D0BD93E9-3A1F-4E5B-8EE8-36ADC33DCFE4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: b06a1a7d525c91d7c3973b2b02d3d2835ce482f9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-manually-resynchronize-xamarin-licenses"></a>Jak ręcznie zsynchronizować Xamarin licencji?

> [!IMPORTANT]
> W tym przewodniku nie ma zastosowania do większości użytkowników MSDN, ponieważ nie są wymagane do własnych lub zaloguj się do kont Xamarin, chyba że za pomocą [przechowywania składników Xamarin](https://components.xamarin.com/) lub [programu Visual Studio for Mac (Mac)](~/cross-platform/get-started/requirements.md).




## <a name="overview"></a>Omówienie

Zwykle informacje o licencji zostaną zsynchronizowane z serwerem licencji Xamarin automatycznie po uruchomieniu IDE. Na przykład po ponownym uruchomieniu IDE pojawi się zwykle automatycznie uaktualnienia licencji, zmiany na starszą wersję i odnawiania. Wyświetlany w IDE informacji o licencji powinien być zgodny informacje wyświetlone na Twojej [strony konta](https://store.xamarin.com/account/my/subscription/computers). Jeśli podane informacje są nadal niezgodne, należy najpierw dokładnie że informacje o koncie magazynu poprawne, a następnie ręcznie zsynchronizować licencji wylogowaniu, usuwanie plików licencji na dysku, a następnie zalogować ponownie.

### <a name="note-for-ios-developers-on-windows"></a>Uwagi dla deweloperów systemu iOS w systemie Windows

Deweloperzy systemu iOS w systemie Windows może być również konieczne wykonaj te kroki dla programu Visual Studio dla komputerów Mac; nawet jeśli nie tworzenia aplikacji bezpośrednio na sparowanego hosta Mac kompilacji.

## <a name="quick-manual-refresh-steps"></a>Odświeżanie ręczne Szybkie kroki

Ten proces szybkiego pomija krok usunięcia plików licencji na dysku, która może być wystarczające w niektórych przypadkach. 

1.  Wyloguj się z konta za pomocą środowiska IDE programu Xamarin:
    -   Visual Studio for Mac
        -   Prawym górnym rogu ekranu powitalnego
        -   **Visual Studio for Mac > konto** (Mac)
        -   **Narzędzia > konto** (system Windows)
    -   Visual Studio
        -   **Narzędzia > Konto Xamarin**
2.  Zaloguj się do konta platformy Xamarin w IDE.

## <a name="extended-manual-license-refresh-steps"></a>Rozszerzony licencji ręczne odświeżanie kroki

1.  Wyloguj się z kontem Xamarin za pomocą środowiska IDE. 
2.  Zamyka IDE.
3.  Sprawdź, czy poprawne informacje o koncie magazynu. W szczególności Sprawdzaj duplikaty nazw komputerów w [strona komputery](https://store.xamarin.com/account/my/subscription/computers).

4.  Jeśli widzisz parę duplikaty nazw komputerów, użyj **Dezaktywuj** elementu menu rozwijanego, aby usunąć _zarówno_ członków pary:
    
    ![Dezaktywuj -> licencji na https://store.xamarin.com/account/my/subscription/computers ] (resync-licenses-images/deactivate.png "użyć elementu menu rozwijanego Dezaktywuj usunąć oba elementy pary.")

5.  Usuń wszystkie pozostałe kopie plików licencji nadal znajdują się na dysku.
    -   Windows

        Usuń wszystkie z następujących folderów:
        -   `%PROGRAMDATA%\Mono for Android`
        -   `%PROGRAMDATA%\MonoTouch`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\Mono for Android`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\MonoTouch`

        W domyślnej instalacji systemu Windows:
        -   `%PROGRAMDATA%` zostanie rozwinięty w celu `C:\ProgramData`
        -   `%LOCALAPPDATA%` zostanie rozwinięty w celu `C:\Users\%USERNAME%\AppData\Local`

        `VirtualStore` Kopię licencji są tworzone automatycznie przez system Windows w niektórych scenariuszach. Jeśli istnieją kopie VirtualStore, będzie można ich odczytać _zamiast_ licencji w `%PROGRAMDATA%`.

    -   Mac

        Usuń wszystkie z następujących folderów:

        -   `~/Library/MonoAndroid`
        -   `~/Library/MonoTouch`
        -   `~/Library/Xamarin.Mac`

        Można uzyskać dostępu do tych ścieżek w **wyszukiwania** wybierając **Przejdź > Przejdź do folderu** menu i wklejając je w oknie dialogowym. Wyszukiwanie automatycznie zastępuje ~ znak tyldy o folderze użytkownika.

6.  Otwórz program Visual Studio dla komputerów Mac lub Visual Studio i dziennika z powrotem do swojego konta Xamarin.

## <a name="supplementary-information-individual-license-file-locations"></a>Informacje dodatkowe: lokalizacje plików poszczególnych licencji

### <a name="windows"></a>Windows

W systemie Windows plików poszczególnych licencji są przechowywane w następujących lokalizacjach:

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.licx`

Licencje *wersji próbnej* subskrypcji są inne nazwy:

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.trial.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.trial.licx`

### <a name="mac"></a>Mac

W systemie Mac plików poszczególnych licencji są przechowywane w następujących lokalizacjach:

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.v2`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License`

Licencje *wersji próbnej* subskrypcji są inne nazwy:

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License.trial`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.trial`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License.trial`

## <a name="additional-troubleshooting-steps"></a>Dodatkowe kroki rozwiązywania problemów

-   Sprawdź, czy istnieje znaków innych niż ASCII w swoją nazwę użytkownika, nazwę komputera lub w pełni rozwinięte ścieżki plików licencji.

-   [Skontaktuj się z obsługą Xamarin Developer](http://xamarin.com/support)
