---
title: Projekty bibliotek dla wielu platform NuGet (zwane również Nugetizer 3000)
description: W tym dokumencie opisano sposób użycia narzędzia Nugetizer 3000, aby automatycznie utworzyć pakiety NuGet umożliwiające udostępnianie kodu między platformami.
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 1d48bc28aa4477361ca8057fda91ee3258f36a73
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270431"
---
# <a name="nuget-multiplatform-library-projects-nugetizer-3000"></a>Projekty programu NuGet biblioteki dla wielu platform (Nugetizer 3000)

_Automatyczne tworzenie pakietów NuGet do udostępniania kodu na wielu platformach za pomocą "3000 Nugetizer"!_

Istnieje możliwość automatycznie utworzyć pakiety NuGet umożliwiające udostępnianie kodu między platformami przy użyciu _Nugetizer 3000_. To sprawia, że możliwe jest utworzenie pakietów NuGet z istniejących projektów biblioteki lub tworząc nowe **projekt biblioteki dla wielu platform**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Nugetizer 3000 jest dołączana do programu Visual Studio dla komputerów Mac &ndash; poszukaj **biblioteki > biblioteki Mulitplatform** projektu wpisz **Plik > Nowy** okna:

[![](images/mulitplatform-library-sml.png "Tworzenie nowego okna biblioteki dla wielu platform")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Aby użyć Nugetizer 3000 w programie Visual Studio, [Pobierz i uruchom Instalator VSIX](http://bit.ly/nugetizer-2017).

-----

## <a name="building-nuget-packages"></a>Tworzenie pakietów NuGet

Po skonfigurowaniu każdej kompilacji projektu danych wyjściowych to kompletny pakiet NuGet, które mogą być używane do udostępniania kodu wewnętrznie z innymi aplikacjami lub przekazany do [NuGet.org](https://www.nuget.org).

Istnieją trzy scenariusze dotyczące używania tej funkcji:

- [Istniejące projekty bibliotek](existing-library.md)

  Utwórz pakiet NuGet z istniejących projektów PCL (lub .NET Standard).

- [Tworzenie nowego projektu Biblioteka wieloplatformowa](single-codebase.md)

  Utwórz nową bibliotekę, aby udostępnić wspólny kod za pomocą narzędzia NuGet, za pomocą aplikacji PCL lub .NET Standard.

- [Tworzenie nowych projektów biblioteki specyficzne dla platformy](platform-specific.md)

  Utwórz nową bibliotekę i NuGet, który zawiera kod specyficzny dla platformy dla systemów iOS i Android oraz korzysta z udostępnionego projektu zawierają typowy kod i projekty specyficzne dla platformy do obsługi funkcji specyficznych dla systemu iOS lub Android.

Zapoznaj się [Przewodnik po metadanych](metadata.md) Aby uzyskać szczegółowe informacje na temat metadanych wymaganych i opcjonalnych, które muszą zostać dodane do dowolnego pakietu NuGet.

## <a name="further-nuget-information"></a>Dalsze informacje NuGet

Przeczytaj więcej na temat [ręcznego tworzenia rozszerzeń Nuget dla programu Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md) oraz sposób [obejmują pakiet NuGet w aplikacji](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Firmy Microsoft [dokumentacja programu NuGet](https://docs.microsoft.com/nuget/) zawiera bardziej szczegółowe informacje na **.nupkg** format i korzystania z pakietów NuGet w programie Visual Studio.

Omówienie projektu dla projektów pakietu NuGet (zwane) NuGetizer 3000) jest dostępny na [repozytorium NuGet GitHub](https://github.com/NuGet/Home/wiki/NuGetizer-3000).

## <a name="related-links"></a>Linki pokrewne

- [Przypadki użycia NuGetizer 3000](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Ręczne tworzenie pakietów NuGet dla platformy Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)
- [Dokumentacja programu NuGet](https://docs.microsoft.com/nuget/)
- [Uwagi do wersji](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
