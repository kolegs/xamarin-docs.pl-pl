---
title: .NET standard
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: f448a3ee9c018aa475775a5ac2c614f3e7ddc324
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="net-standard"></a>.NET standard

## <a name="using-net-standard-library-projects-to-share-code"></a>Udostępnianie kodu przy użyciu standardowych projektów bibliotek .NET

Standardowa biblioteka .NET jest formalną specyfikację interfejsów API architektury .NET, które mają być dostępne na wszystkich programów .NET. Motywacją za biblioteki standardowej jest ustanowienie większej jednolitości w ekosystemie .NET.
[ECMA-335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) ustanowić jednolitość zachowania środowiska uruchomieniowego .NET, w dalszym ciągu, ale nie specyfikacji podobne dla .NET Base klasy biblioteki (BCL) w przypadku implementacji biblioteki .NET nie istnieje.

Można traktować go jako uproszczony, generacji [przenośnej biblioteki klas](https://msdn.microsoft.com/library/gg597391.aspx).
Jest jedna biblioteka z uniform interfejsu API dla wszystkich platform .NET, w tym oprogramowanie .NET Core. Po prostu utworzyć jednej biblioteki standardowej .NET i użyć go z dowolnym środowisko uruchomieniowe, które obsługuje standardowa platforma programu .NET.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="xamarin-studio"></a>Xamarin Studio

Projekty bibliotek .NET standard można utworzyć w programie Xamarin Studio 6.2, tworząc pierwszy projektu biblioteki przenośnej:

[![](net-standard-images/xs01-sml.png "Tworzenie nowego projektu biblioteki przenośnej")](net-standard-images/xs01.png#lightbox)

Po utworzeniu projektu, kliknij prawym przyciskiem myszy i Otwórz **opcje projektu** okna.
W **ogólne** sekcji projektu można przekonwertować na .NET Standard i skonfigurowane do korzystania z określonej wersji w **platformy** listy rozwijanej:

[![](net-standard-images/xs02-sml.png "Konwertuj na .NET Standard ogólnie opcje")](net-standard-images/xs02.png#lightbox)

Następnie możesz [Utwórz pakiet NuGet](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/existing-library.md) do udziału biblioteki z innymi deweloperami.

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac Walkthrough

W tej sekcji przedstawiono sposób tworzenia i używania biblioteki standardowej .NET przy użyciu programu Visual Studio dla komputerów Mac. Zapoznaj się z rozdziałem standardowe przykład biblioteki .NET dla pełnej implementacji.

### <a name="creating-a-net-standard-library"></a>Tworzenie biblioteki standardowej .NET

Dodawanie biblioteki standardowej .NET do rozwiązania jest dość proste do przodu.

1. W oknie dialogowym Dodawanie nowego projektu, zaznacz `.NET Core` kategorii, a następnie wybierz `Class Library(.NET Core)`.

  **Uwaga:** ten szablon zostanie zmieniona na `.NET Standard` w przyszłych wersjach programu Visual Studio dla komputerów Mac.

  ![Utwórz bibliotekę klas .NET Core](net-standard-images/vsm01.png)

2. Projektu biblioteki standardowej .NET będą wyświetlane, jak pokazano w Eksploratorze rozwiązań. Węzeł zależności wskaże, że biblioteka używa [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![Węzeł zależności w rozwiązaniu wskazuje .NET Standard](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>Edytowanie ustawień biblioteki standardowej platformy .NET

Ustawienia biblioteki standardowej .NET można wyświetlać i zmieniony przez kliknięcie prawym przyciskiem myszy projekt i wybierając `Options` opisane w tym zrzut ekranu:

![Edytuj .NET Standard platformy docelowej w opcje projektu](net-standard-images/vsm03.png)

Wewnątrz można zmienić swoją wersję `netstandard` zmieniając `Target Framework` wartość z listy rozwijanej.

**Ponadto:** można edytować `.csproj` bezpośrednio, aby zmienić tę wartość.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-windows-walkthrough"></a>Wskazówki programu Visual Studio (z systemem Windows)

W tej sekcji przedstawiono sposób tworzenia i używania biblioteki standardowej .NET przy użyciu programu Visual Studio. Zapoznaj się z rozdziałem standardowe przykład biblioteki .NET dla pełnej implementacji.

### <a name="creating-a-net-standard-library"></a>Tworzenie biblioteki standardowej .NET

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Dodawanie biblioteki standardowej .NET do rozwiązania jest dość proste do przodu.

1. W oknie dialogowym Dodawanie nowego projektu, zaznacz `.NET Standard` kategorii, a następnie wybierz `Class Library(.NET Standard)`.

  ![](net-standard-images/vs01.png "Utwórz nową bibliotekę klas .NET Standard")

2. Projektu biblioteki standardowej .NET będą wyświetlane, jak pokazano w Eksploratorze rozwiązań. Węzeł zależności wskaże, że biblioteka używa [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![](net-standard-images/vs02.png ".NET standard projektu w rozwiązaniu")

#### <a name="editing-net-standard-library-settings"></a>Edytowanie ustawień biblioteki standardowej platformy .NET

Ustawienia biblioteki standardowej .NET można wyświetlać i zmieniony przez kliknięcie prawym przyciskiem myszy projekt i wybierając `Properties` opisane w tym zrzut ekranu:

![](net-standard-images/vs03.png "Odwołuje się do platformy .NET Standard biblioteki taki sam sposób jak inne projekty")

Wewnątrz można zmienić swoją wersję `netstandard` zmieniając `Target Framework` wartość z listy rozwijanej.

**Ponadto:** można edytować `.csproj` bezpośrednio, aby zmienić tę wartość.

#### <a name="using-net-standard-library"></a>Przy użyciu biblioteki standardowej .NET

Po utworzeniu biblioteki standardowej .NET można dodać odwołania do niego z dowolnego zgodnego projektu aplikacji lub biblioteki w taki sam sposób, zwykle Dodaj odwołania. W programie Visual Studio, kliknij prawym przyciskiem myszy w węźle odwołania, a następnie wybierz pozycję `Add Reference...` następnie przełącz się do `Solution : Projects` karcie pokazany:

![](net-standard-images/vs04.png "W programie Visual Studio kliknij prawym przyciskiem myszy w węźle odwołania i wybierz polecenie Dodaj odwołanie..., a następnie przejść do karty rozwiązania, projekty, jak pokazano")

-----


## <a name="related-links"></a>Linki pokrewne

- [Uwagi do wersji](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#.NET_Standard_Support)
