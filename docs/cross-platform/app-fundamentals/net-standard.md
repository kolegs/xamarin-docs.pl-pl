---
title: Użyj standardowych bibliotek .NET umożliwiające udostępnianie kodu
description: W tym dokumencie opisano, jak używać standardowych bibliotek platformy .NET do udostępniania kodu. Omówiono w nim tworzenia biblioteki .NET Standard, edytowania jej ustawienia i korzystania z niego w aplikacji.
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 65ba1915a2a968a14f0ce21bcada76e1b83531b0
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270635"
---
# <a name="net-standard-library-code-sharing"></a>Udostępniania kodu Biblioteka .NET standard

Biblioteki .NET standard mieć jednolitego interfejsu API dla wszystkich platform .NET, w tym Xamarin i .NET Core. Utwórz pojedynczy Biblioteka .NET Standard i używać jej z dowolnego środowiska uruchomieniowego, które obsługuje platforma .NET Standard. Zapoznaj się [ten wykres](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support) informacje na temat obsługiwanych platform.

Gdy .NET Standard wersji 1.0 za pośrednictwem 1.6 zapewnia przyrostowo większych podzbiorem .NET Framework, .NET Standard 2.0 zapewnia najlepsze poziom pomocy technicznej dla aplikacji środowiska Xamarin i przenoszenie istniejących bibliotek klas przenośnych.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

Ta sekcja zawiera szczegółowe instrukcje dotyczące tworzenia i używania biblioteki standardowej platformy .NET przy użyciu programu Visual Studio dla komputerów Mac.

### <a name="creating-a-net-standard-library"></a>Tworzenie biblioteki .NET Standard

Dodawanie biblioteki .NET Standard do rozwiązania jest już bardzo proste.

1. W **Dodaj nowy projekt** okno dialogowe, wybierz opcję **platformy .NET Core** kategorii, a następnie wybierz **Biblioteka .NET Standard**:

    ![Tworzenie biblioteki .NET Standard](net-standard-images/vsm01-m157.png "tworzenia nowych .NET Standard biblioteki")

2. Na następnym ekranie Wybierz platformę docelową - **.NET Standard 2.0** jest zalecane:

    [![Wybierz pozycję .NET Standard 2.0](net-standard-images/vsm01a-m157-sml.png)](net-standard-images/vsm01a-m157.png#lightbox)

3. Na ostatnim ekranie, wpisz nazwę projektu, a następnie kliknij przycisk **Utwórz**.

4. Projekt Biblioteka .NET Standard pojawi się, jak pokazano w Eksploratorze rozwiązań. Węzeł zależności będą wskazywać, że korzysta z biblioteki [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

    ![Węzeł zależności w rozwiązaniu wskazuje .NET Standard](net-standard-images/vsm02-m157.png)

#### <a name="editing-net-standard-library-settings"></a>Edytowanie ustawień Biblioteka .NET Standard

Ustawienia Biblioteka .NET Standard można wyświetlać i zmienić, klikając prawym przyciskiem myszy na projekt i wybierając `Options` pokazany na tym zrzucie ekranu:

![Edytuj .NET Standard platforma docelowa, opcje projektu](net-standard-images/vsm03-m157.png "Edytuj wersję programu .NET Framework docelowej standardowe opcje projektu")

Wewnątrz zmienisz wersję `netstandard` , zmieniając `Target Framework` wartość listy rozwijanej.

**Dodatkowo:** można edytować `.csproj` bezpośrednio, aby zmienić tę wartość.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-2017-windows"></a>Program Visual Studio 2017 (Windows)

Ta sekcja zawiera szczegółowe instrukcje dotyczące tworzenia i używania biblioteki standardowej platformy .NET przy użyciu programu Visual Studio.

### <a name="creating-a-net-standard-library"></a>Tworzenie biblioteki .NET Standard

Dodawanie biblioteki .NET Standard do rozwiązania jest już bardzo proste.

1. W **nowy projekt** okno dialogowe, wybierz opcję **.NET Standard** kategorii, a następnie wybierz **biblioteki klas (.NET Standard)**.

    ![Tworząc nową bibliotekę klas standardowy .NET](net-standard-images/vs01-w157.png "Tworzenie biblioteki klas .NET Standard nowe")

2. Projekt Biblioteka .NET Standard pojawi się, jak pokazano w Eksploratorze rozwiązań. Węzeł zależności będą wskazywać, że korzysta z biblioteki [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

    ![NETStandard.Library w folderze projektu](net-standard-images/vs02-w157.png ".NET Standard projektu w rozwiązaniu")

### <a name="editing-net-standard-library-settings"></a>Edytowanie ustawień biblioteki .NET Standard

Ustawienia Biblioteka .NET Standard można wyświetlać i zmienić, klikając prawym przyciskiem myszy na projekt i wybierając **właściwości** pokazany na tym zrzucie ekranu:

![Edytuj platform docelowych standardowy .NET we właściwościach projektu](net-standard-images/vs03-w157.png "odwołać ten sam sposób jak inne projekty biblioteki .NET Standard")

**Dodatkowo:** można edytować `.csproj` bezpośrednio do edycji `TargetFramework` elementu i zmianę, która wersja jest docelowe (np.) `<TargetFramework>netstandard2.0</TargetFramework>`).

### <a name="using-a-net-standard-library-project"></a>Za pomocą projektu Biblioteka .NET Standard

Po utworzeniu biblioteka .NET Standard, można dodać odwołanie do niej z poziomu dowolnej aplikacji zgodnych lub biblioteki projektu w taki sam sposób możesz normalnie należy dodać odwołania. W programie Visual Studio, kliknij prawym przyciskiem myszy węzeł odwołania i wybierz polecenie **Dodaj odwołanie...**  następnie przełącz się do **projektów > rozwiązania** karty, jak pokazano:

![Odwołanie do biblioteki .NET Standard](net-standard-images/vs04.png "w programie Visual Studio, kliknij prawym przyciskiem myszy węzeł odwołania i wybierz polecenie Dodaj odwołanie..., a następnie przejdź do karty projektów rozwiązania, jak pokazano")

-----

## <a name="related-links"></a>Linki pokrewne

* [.NET standard](https://docs.microsoft.com/dotnet/standard/net-standard) — szczegółowe informacje i porównanie PCL.
