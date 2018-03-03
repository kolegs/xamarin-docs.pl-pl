---
title: Projekty NuGet (Nugetizer 3000)
description: "Automatyczne tworzenie pakietów NuGet udostępnianie kodu na platformach przy użyciu 'Nugetizer 3000'!"
ms.topic: article
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: 66bf9c215e3d30687fa8037220b8b35409ca285d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="nuget-projects-nugetizer-3000"></a>Projekty NuGet (Nugetizer 3000)

_Automatyczne tworzenie pakietów NuGet udostępnianie kodu na platformach przy użyciu 'Nugetizer 3000'!_

Istnieje możliwość automatycznego tworzenia pakietów NuGet udostępnianie kodu na platformach przy użyciu _Nugetizer 3000_. Sprawia to, że możliwe jest utworzenie pakietów NuGet z istniejących projektów biblioteki lub tworząc nowe **projektu biblioteki Multiplatform**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
Nugetizer 3000 jest dołączony do programu Visual Studio for Mac 6.2.
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
<a name="to-use-the-nugetizer-3000-in-visual-studio-please-download-and-run-the-vsix-installerhttpbitlynugetizer-2017"></a>Aby użyć Nugetizer 3000 w programie Visual Studio, skontaktuj się z [Pobierz i uruchom Instalatora VSIX](http://bit.ly/nugetizer-2017).
-----



[ ![](images/mulitplatform-library-sml.png "Utwórz nowe okno Multiplatform biblioteki")](images/mulitplatform-library.png)

Po skonfigurowaniu pełną pakiet NuGet, który jest używany wewnętrznie udostępnianie kodu z innych aplikacji lub przekazany do danych wyjściowych co kompilacji projektu [NuGet.org](https://www.nuget.org).

Istnieją trzy scenariusze dla tej funkcji:

- [Istniejące projekty bibliotek](existing-library.md)

  Utwórz pakiet NuGet z istniejących projektów PCL (lub .NET Standard).

- [Tworzenie nowego projektu biblioteki dla wielu platform](single-codebase.md)

  Utwórz nową bibliotekę udostępnianie typowy kod za pośrednictwem pakietu NuGet, za pomocą PCL lub .NET Standard.

- [Tworzenie nowych projektów biblioteki specyficzne dla platformy](platform-specific.md)

  Utwórz nową bibliotekę i NuGet zawiera kod specyficzne dla platformy dla systemów iOS i Android, która używa projektu udostępnionego zawierają typowy kod i projekty specyficzne dla platformy do obsługi funkcji specyficznych dla systemu iOS lub Android.

Zapoznaj się [przewodnik metadanych](metadata.md) szczegółowe informacje o wymaganych i opcjonalnych metadanych, który musi zostać dodany do dowolnego pakietu NuGet.


## <a name="further-nuget-information"></a>Dalsze informacje NuGet

Przeczytaj więcej na temat [ręcznego tworzenia NuGets xamarin](~/cross-platform/app-fundamentals/nuget-manual.md) oraz sposób [obejmują pakietu NuGet w aplikacji](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Firmy Microsoft [dokumentacji NuGet](https://docs.microsoft.com/nuget/) zawiera bardziej szczegółowe informacje na **.nupkg** format i za pomocą pakietów NuGet w programie Visual Studio.

Omówienie projektu dla projektów pakietu NuGet () NuGetizer 3000) jest dostępny na [repozytorium NuGet GitHub](https://github.com/NuGet/Home/wiki/NuGetizer-3000).


## <a name="related-links"></a>Linki pokrewne

- [Przypadki użycia NuGetizer 3000](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Ręcznie utwórz pakiety NuGet xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)
- [Dokumentacja NuGet](https://docs.microsoft.com/nuget/)
- [Uwagi do wersji](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
