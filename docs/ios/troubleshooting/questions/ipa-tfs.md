---
title: Jak skopiować pliki wyjściowe IPA do folderu przechowywania TFS
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 2139be01b95a0a4287bba43b8a2ebad537ac7a4f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>Jak skopiować pliki wyjściowe IPA do folderu przechowywania TFS

Otwórz `.csproj` plików projektu aplikacji systemu iOS w edytorze tekstu, a następnie dodaj następujące wiersze na końcu (bezpośrednio przed tagiem zamykającym `</Project>` tagu):

```xml
<PropertyGroup>
    <CreateIpaDependsOn>
        $(CreateIpaDependsOn);
        CopyIpa
    </CreateIpaDependsOn>
</PropertyGroup>

<Target Name="CopyIpa"
    Condition="'$(OutputType)' == 'Exe'
        And '$(ComputedPlatform)' == 'iPhone'
        And '$(BuildIpa)' == 'true'
        And '$(TF_BUILD)' == 'true'">
    <Copy
        SourceFiles="$(OutputPath)$(IpaPackageName)"
        DestinationFolder="$(TF_BUILD_BINARIESDIRECTORY)"
    />
</Target>
```

## <a name="notes"></a>Uwagi

-   Jest to ta sama metoda ogólna w [można zmienić ścieżki wyjściowej pliku IPA?](~/ios/troubleshooting/questions/ipa-output-path.md). Dwie ważne kwestie mają `$(TF_BUILD_BINARIESDIRECTORY)` jako folder docelowy, a więc Dodaj dodatkowy warunek `CopyIpa` działa tylko dla kompilacji TFS.

-   Opis `TF_BUILD_BINARIESDIRECTORY` zobacz [ https://msdn.microsoft.com/en-us/library/hh850448.aspx ](https://msdn.microsoft.com/en-us/library/hh850448.aspx).

## <a name="additional-references"></a>Dodatkowe informacje

- [Dokumentacja na temat instalowania programu TFS do użycia z programem Xamarin](https://docs.microsoft.com/vsts/tfvc/overview)
- [TFS Build Task: Xamarin.Android](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/xamarin-android)
- [TFS Build Task: Xamarin.iOS](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Następne kroki
W tym dokumencie omówiono bieżące zachowanie począwszy od platformy Xamarin 3.11.666 dla programu Visual Studio i platformy Xamarin.iOS 8.10.3 na Mac kompilacji hosta. Aby uzyskać dodatkową pomoc, skontaktuj się z nami, lub jeśli ten problem pozostanie nawet po użyciu powyższe informacje, zobacz [jakie opcje pomocy technicznej są dostępne dla platformy Xamarin?](~/cross-platform/troubleshooting/support-options.md) informacji o opcjach kontaktu, masz sugestie, oraz jak Plik nowe usterki, w razie potrzeby. 



