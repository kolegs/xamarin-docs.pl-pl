---
title: Instalowanie osadzanie .NET
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
ms.technology: xamarin-cross-platform
author: chamons
ms.author: chhamo
ms.date: 4/18/2018
ms.openlocfilehash: d4ee3ef610611dd489d90a364d15f727ea49a79e
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="installing-net-embedding"></a>Instalowanie osadzanie .NET

## <a name="installing-net-embedding-from-nuget"></a>Instalowanie .NET osadzanie z NuGet

Wybierz **Dodaj > Dodawanie pakietów NuGet...**  i zainstaluj **Embeddinator 4000** z Menedżera pakietów NuGet:

![Menedżer pakietów NuGet](images/visualstudionuget.png)

Spowoduje to zainstalowanie **Embeddinator 4000.exe** i **objcgen** do **pakietów/Embeddinator-4000/narzędzia** katalogu.

Ogólnie rzecz biorąc należy wybrać pobierania najnowszej wersji Embeddinator-4000. Obsługa języka Objective-C wymaga 0,4 lub nowszym.

## <a name="running-manually"></a>Ręczne uruchomienie

Teraz, gdy NuGet jest zainstalowany, możesz uruchomić narzędzia ręcznie.

- Otwórz Terminal (macOS) lub wiersza polecenia (system Windows)
- Zmień katalog w katalogu głównym rozwiązania
- Narzędzia jest instalowany w:
    - **. / pakietów/Embeddinator-4000. [Wersja] / tools/objcgen** (Objective-C)
    - **. / pakietów/Embeddinator-4000. [VERSION]/tools/Embeddinator-4000.exe** (Java/C) 
- Na macOS **objcgen** można uruchomić bezpośrednio. 
- W systemie Windows **Embeddinator 4000.exe** można uruchomić bezpośrednio.
- Na macOS **Embeddinator 4000.exe** musi zostać uruchomione z **mono**: 
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

Każde wywołanie polecenia należy liczba ma parametrów wymienionych w dokumentacji specyficzne dla platformy.

## <a name="automatic-binding-generation"></a>Powiązanie automatyczne generowanie

Istnieją dwa podejścia do automatycznego uruchamiania osadzanie .NET część procesu kompilacji.

- Niestandardowe docelowych elementów MSBuild
- Kroki mające miejsce po kompilacji

Gdy w tym dokumencie opisano zarówno, przy użyciu niestandardowych docelowy programu MSBuild jest preferowana. Kroki tworzenia POST nie uruchomi niekoniecznie podczas kompilowania z wiersza polecenia.

### <a name="custom-msbuild-targets"></a>Niestandardowe docelowych elementów MSBuild

Aby dostosować kompilacji z celami programu MSbuild, należy najpierw utworzyć **Embeddinator 4000.targets** pliku obok Twojej csproj podobny do następującego:

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

W tym miejscu tekst polecenia powinno być wypełnione przy użyciu jednej z wywołań osadzanie .NET wymienione w dokumentacji specyficzne dla platformy.

Aby użyć tego obiektu docelowego:

- Zamknij projekt w programie Visual Studio 2017 lub Visual Studio dla komputerów Mac
- Otwórz w edytorze tekstów csproj biblioteki
- Dodaj następujący wiersz na dole powyżej ostatecznych `</Project>` wiersza:

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- Ponownie otwórz projekt

### <a name="post-build-steps"></a>Kroki mające miejsce po kompilacji

Kroki, aby dodać krok postkompilacyjnego do uruchomienia .NET osadzanie różnić w zależności od środowiska IDE:

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

W programie Visual Studio dla komputerów Mac, przejdź do **opcje projektu > kompilacji > polecenia niestandardowych** i Dodaj **po kompilacji** kroku.

Konfigurowanie polecenia wymienione w dokumentacji specyficzne dla platformy.

> [!NOTE]
> Upewnij się, że numer wersji z NuGet została zainstalowana

Jeśli zamierzasz realizacji bieżących prac nad projektu C#, może również Dodawanie polecenia niestandardowego do czyszczenia **dane wyjściowe** katalogu przed uruchomieniem osadzanie .NET:

```shell
rm -Rf '${SolutionDir}/output/'
```

![Akcji niestandardowej kompilacji](images/visualstudiocustombuild.png)

> [!NOTE]
> Powiązanie wygenerowanego zostaną umieszczone w katalogu wskazywanego przez `--outdir` lub `--out` parametru. Nazwę wygenerowanego może się różnić, jak niektóre platformy podlegają ograniczeniom na nazwy pakietu.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Zasadniczo skonfigurujemy ten sam efekt, ale menu w programie Visual Studio 2017 są nieco inne. Polecenia powłoki również są nieco inne.

Przejdź do **opcje projektu > zdarzeń kompilacji** , a następnie wprowadź polecenie wymienione w dokumentacji specyficzne dla platformy w **wiersz polecenia zdarzenia po kompilacji** pole. Na przykład:

![Osadzanie w systemie Windows .NET](images/visualstudiowindows.png)
