---
title: Projekty NuGet (Nugetizer 3000)
description: Ten dokument zawiera opis sposobu narzędzie Nugetizer 3000 służy do automatycznego tworzenia pakietów NuGet, aby udostępnić kodu na platformach.
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: f79ed775173e05dd850fbc74c53127b63c0d5f34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780773"
---
# <a name="nuget-projects-nugetizer-3000"></a>Projekty NuGet (Nugetizer 3000)

_Automatyczne tworzenie pakietów NuGet udostępnianie kodu na platformach przy użyciu 'Nugetizer 3000'!_

Istnieje możliwość automatycznego tworzenia pakietów NuGet udostępnianie kodu na platformach przy użyciu _Nugetizer 3000_. Sprawia to, że możliwe jest utworzenie pakietów NuGet z istniejących projektów biblioteki lub tworząc nowe **projektu biblioteki Multiplatform**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Nugetizer 3000 jest dołączony do programu Visual Studio for Mac 6.2.

[![](images/mulitplatform-library-sml.png "Utwórz nowe okno Multiplatform biblioteki")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby użyć Nugetizer 3000 w programie Visual Studio, skontaktuj się z [Pobierz i uruchom Instalatora VSIX](http://bit.ly/nugetizer-2017).

-----

## <a name="building-nuget-packages"></a>Pakiety NuGet Building

Po skonfigurowaniu pełną pakiet NuGet, który jest używany wewnętrznie udostępnianie kodu z innych aplikacji lub przekazany do danych wyjściowych co kompilacji projektu [NuGet.org](https://www.nuget.org).

Istnieją trzy scenariusze dla tej funkcji:

- [Istniejące projekty bibliotek](existing-library.md)

  Utwórz pakiet NuGet z istniejących projektów PCL (lub .NET Standard).

- [Tworzenie nowego projektu biblioteki dla wielu platform](single-codebase.md)

  Utwórz nową bibliotekę udostępnianie typowy kod za pośrednictwem pakietu NuGet, za pomocą PCL lub .NET Standard.

- [Tworzenie nowych projektów biblioteki specyficzne dla platformy](platform-specific.md)

  Utwórz nową bibliotekę i NuGet zawiera kod specyficzne dla platformy dla systemów iOS i Android, która używa projektu udostępnionego zawierają typowy kod i projekty specyficzne dla platformy do obsługi funkcji specyficznych dla systemu iOS lub Android.

Zapoznaj się [przewodnik metadanych](metadata.md) szczegółowe informacje o wymaganych i opcjonalnych metadanych, który musi zostać dodany do dowolnego pakietu NuGet.


## <a name="further-nuget-information"></a>Więcej informacji o NuGet

Przeczytaj więcej na temat [ręcznego tworzenia NuGets xamarin](~/cross-platform/app-fundamentals/nuget-manual.md) oraz sposób [obejmują pakietu NuGet w aplikacji](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Firmy Microsoft [dokumentacji NuGet](https://docs.microsoft.com/nuget/) zawiera bardziej szczegółowe informacje na **.nupkg** format i za pomocą pakietów NuGet w programie Visual Studio.

Omówienie projektu dla projektów pakietu NuGet () NuGetizer 3000) jest dostępny na [repozytorium NuGet GitHub](https://github.com/NuGet/Home/wiki/NuGetizer-3000).


## <a name="related-links"></a>Linki pokrewne

- [Przypadki użycia NuGetizer 3000](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Ręcznie utwórz pakiety NuGet xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)
- [Dokumentacja NuGet](https://docs.microsoft.com/nuget/)
- [Uwagi do wersji](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
