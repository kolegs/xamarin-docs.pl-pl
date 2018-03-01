---
title: "Czy można zmienić ścieżki wyjściowej pliku IPA?"
ms.topic: article
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 2cb5ef615bfd965ce3fbd4efbab7669fe12679a4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>Czy można zmienić ścieżki wyjściowej pliku IPA?

## <a name="for-cycle-7-and-higher"></a>Cykl 7 lub nowszej
Tak, można użyć niestandardowych docelowych elementów MSBuild można to osiągnąć. Opcja najprostszym prawdopodobnie ma na celu kopiowanie `.ipa` plików po został skompilowany.

Te kroki będzie działać dla żadnego projektu iOS korzystającym z aparatu MSBuild kompilacji na Mac lub Windows. (Uwaga: wszystkie projekty Unified API korzystać z aparatu kompilacji MSBuild.)

1. Otwórz `.csproj` plików projektu aplikacji systemu iOS w edytorze tekstu, a następnie dodaj następujące wiersze na końcu (bezpośrednio przed tagiem zamykającym `</Project>` tagu):
    
    ```
    <PropertyGroup>
           <CreateIpaDependsOn>
           $(CreateIpaDependsOn);
            CopyIpa
           </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(IpaPackagePath)"
            DestinationFolder="$(OutputPath)"
        />
    </Target>
    ```

2. Ustaw DestinationFolder do żądanego folderu wyjściowego. W zwykły sposób można użyć właściwości programu MSBuild (na przykład $(OutputPath)) w ramach tego argumentu, w razie potrzeby.

## <a name="notes"></a>Uwagi
- `CreateIpaDependsOn` Właściwość jest zdefiniowana w `Xamarin.iOS.Common.targets` pliku część platformy Xamarin.iOS. Działa zgodnie z opisem w *właściwości "DependsOn" zastąpiona* na [https://msdn.microsoft.com/en-us/library/ms366724.aspx](https://msdn.microsoft.com/en-us/library/ms366724.aspx).

- Można użyć **Przenieś** zadań zamiast **kopiowania** zadań, jeśli Twoja preferowana. Jeśli zostanie wybrana opcja i tworzenia w systemie Windows, należy użyć zadania w pełni kwalifikowaną nazwę `<Microsoft.Build.Tasks.Move>` Aby uniknąć niejednoznaczności z XamarinVS zadania kompilacji.

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>W przypadku wersji przed Xamarin Studio 6.0.0.5174 | Xamarin dla Visual Studio 4.1.0.530

Tak, można użyć niestandardowych docelowych elementów MSBuild można to osiągnąć. Opcja najprostszym prawdopodobnie ma na celu kopiowanie `.ipa` plików po został skompilowany.

Te kroki będzie działać dla żadnego projektu iOS korzystającym z aparatu MSBuild kompilacji na Mac lub Windows. (Uwaga: wszystkie projekty Unified API korzystać z aparatu kompilacji MSBuild.)

1. Otwórz `.csproj` plików projektu aplikacji systemu iOS w edytorze tekstu, a następnie dodaj następujące wiersze na końcu (bezpośrednio przed tagiem zamykającym `</Project>` tag).

    ```csharp
    <PropertyGroup>
        <CreateIpaDependsOn>
            $(CreateIpaDependsOn);
            CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(OutputPath)$(IpaPackageName)"
            DestinationFolder="/Users/macuser/Desktop/"
        />
    </Target>
    ```

2. Ustaw `DestinationFolder` do żądanego folderu wyjściowego. W zwykły sposób można użyć właściwości programu MSBuild (takie jak `$(OutputPath)`) w ramach tego argumentu, w razie potrzeby.

## <a name="notes"></a>Uwagi
- `CreateIpaDependsOn` Właściwość jest zdefiniowana w `Xamarin.iOS.Common.targets` pliku część platformy Xamarin.iOS. Działa zgodnie z opisem w *właściwości "DependsOn" zastąpiona* na [https://msdn.microsoft.com/en-us/library/ms366724.aspx](https://msdn.microsoft.com/en-us/library/ms366724.aspx).

- Można użyć **Przenieś** zadań zamiast **kopiowania** zadań, jeśli Twoja preferowana. Jeśli zostanie wybrana opcja i tworzenia w systemie Windows, należy użyć zadania w pełni kwalifikowaną nazwę `<Microsoft.Build.Tasks.Move>` Aby uniknąć niejednoznaczności z XamarinVS zadania kompilacji.
