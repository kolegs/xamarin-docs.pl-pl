---
title: "Instalowanie NUnit 2.6.4 — przy użyciu narzędzia NuGet"
description: "Jak obniżyć NUnit 3.0 NUnit 2.6.4 — w tym przewodniku dotyczą przy użyciu narzędzia NuGet."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7683F2B8-7FDF-48C4-8E7D-649D4D4E79F0
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: b7f42d6e36638bf5c7e98b9363295e37997ee067
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="installing-nunit-264-using-nuget"></a>Instalowanie NUnit 2.6.4 — przy użyciu narzędzia NuGet

_Jak obniżyć NUnit 3.0 NUnit 2.6.4 — w tym przewodniku dotyczą przy użyciu narzędzia NuGet._

Deweloperów, którzy pisania testów w programie Visual Studio dla komputerów Mac lub przy użyciu Xamarin.UITest powinien być używany [NUnit 2.6.4 —](http://nunit.org/index.php?p=docHome&r=2.6.4) nie jest zgodny z programu Visual Studio for Mac lub Xamarin.UITest jako NUnit 3.0 lub nowszej. Próba uruchomienia testów jednostkowych programu Visual Studio dla komputerów Mac lub Xamarin.UITests z NUnit 3.0 zakończy się niepowodzeniem.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W tym przewodniku będzie omawiać temat Instalowanie NUnit 2.6.4 — przy użyciu narzędzia NuGet dla programu Visual Studio dla komputerów Mac. Te kroki spowoduje również odinstalowanie NUnit 3.0, jeśli to konieczne.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W tym przewodniku będzie omawiać temat na starszą wersję 3.0 NUnit NUnit 2.6.4 — przy użyciu narzędzia NuGet w programie Visual Studio 2015.

-----

## <a name="requirements"></a>Wymagania

W tym przewodniku założono, że istnieje istniejącego rozwiązania z projektem aplikacji mobilnej i projektu testowego.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="installing-nunit-264-in-visual-studio-for-mac"></a>Instalowanie NUnit 2.6.4 — w programie Visual Studio dla komputerów Mac

W poniższych krokach opisano sposób instalowania NUnit 2.6.4 —.


1. **Otwórz Menedżera pakietów** -kliknij prawym przyciskiem myszy **pakiety** i wybierz **Dodawanie pakietów** z menu podręcznego:

    [![](installing-nunit-using-nuget-images/add-packages-xs.png "Kliknij prawym przyciskiem pakietów i dodawanie pakietów, wybierz z menu podręcznego")](installing-nunit-using-nuget-images/add-packages-xs.png#lightbox)
    
1. **Wyszukaj `NUnit version:2.6.4`**  -odinstalować NUnit 3.0 (w razie potrzeby), a następnie pobierze i zainstaluje NUnit 2.6.4 — Visual Studio for Mac. W **Dodawanie pakietów** okna dialogowego, wprowadź tekst `nunit version:2.6.4` w **wyszukiwania** pole znajduje się w prawym górnym rogu. Wybierz **NUnit** w wynikach wyszukiwania, kliknij **Dodaj pakiet** przycisk:

    [![](installing-nunit-using-nuget-images/nunit-search-xs.png "Wybierz NUnit w wynikach wyszukiwania i kliknij przycisk Dodaj pakiet")](installing-nunit-using-nuget-images/nunit-search-xs.png#lightbox)


Istnieje możliwość upewnić się, że zainstalowano NUnit 2.6.4 — sprawdzając numer wersji pakietu NUnit w konsoli do rozwiązania:

[![](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png "Sprawdź numer wersji pakietu NUnit w konsoli do rozwiązania")](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png#lightbox)

## <a name="summary"></a>Podsumowanie

W tym przewodniku omówiono sposób na starszą wersję 3.0 NUnit NUnit 2.6.4 — w programie Visual Studio dla komputerów Mac przy użyciu konsoli Menedżera pakietów.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installing-nunit-264-in-visual-studio"></a>Instalowanie NUnit 2.6.4 — w programie Visual Studio

W tej sekcji dotyczą przy użyciu _Konsola Menedżera pakietów NuGet_ w programie Visual Studio 2015 NUnit 3.0 odinstalować i zainstalować NUnit 2.6.4 —.


1. **Uruchom konsolę Menedżera pakietów NuGet** — wybierz tę opcję **Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów**:

    [![](installing-nunit-using-nuget-images/package-manager-console.png "Uruchom konsoli Menedżera pakietów NuGet - konsoli Menedżera pakietów Menedżera pakietów NuGet wybierz menu Narzędzia")](installing-nunit-using-nuget-images/package-manager-console.png#lightbox)
    
1. **Sprawdź wersję NUnit** — opcjonalnie może zweryfikować wersji NUnit, który jest zainstalowany, uruchamiając polecenie `Get-Package -Project <UITEST PROJECT>`:

    ```bash
    [PM] Get-Package -Project [TEST PROJECT NAME]
    
        Id                                  Versions                                 ProjectName
        --                                  --------                                 -----------
    NUnit                               {3.0.1}                                  [TEST PROJECT NAME]
    Xamarin.UITest                      {1.2.0}                                  [TEST PROJECT NAME]
    ```

Jeśli widzisz NUnit 3.0 lub nowszej, a następnie należy obniżyć poziom NUnit 2.6.4 —.

1. **Odinstaluj NUnit 3.0** -użyj `Uninstall-Package` polecenia do odinstalowania NUnit 3.0:

        <PM> Uninstall-Package NUnit -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.3.0.1' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Resolving actions to uninstall package 'NUnit.3.0.1'
        Resolved actions to uninstall package 'NUnit.3.0.1'
        Removed package 'NUnit.3.0.1' from 'packages.config'
        Successfully uninstalled 'NUnit.3.0.1' from <TEST PROJECT NAME>

1. **Zainstaluj NUnit 2.6.4 —** -Zainstaluj Nunit 2.6.4 — z `Install-Package` polecenie commandlet, jak pokazano w poniższy fragment kodu:

        <PM> Install-Package NUnit -Version 2.6.4 -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.2.6.4' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Attempting to resolve dependencies for package 'NUnit.2.6.4' with DependencyBehavior 'Lowest'
        Resolving actions to install package 'NUnit.2.6.4'
        Resolved actions to install package 'NUnit.2.6.4'
        Adding package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to 'packages.config'
        Successfully installed 'NUnit 2.6.4' to <TEST PROJECT NAME>
    
## <a name="summary"></a>Podsumowanie

W tym przewodniku omówiono sposób na starszą wersję 3.0 NUnit NUnit 2.6.4 — w programie Visual Studio 2015, przy użyciu konsoli Menedżera pakietów.

-----

## <a name="related-links"></a>Linki pokrewne

- [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4)
- [NUnit 2.6.4 NuGet Package](https://www.nuget.org/packages/NUnit/2.6.4)
