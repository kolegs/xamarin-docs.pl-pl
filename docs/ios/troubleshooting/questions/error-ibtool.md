---
title: 'Błąd narzędzia IBTool: Nie można ukończyć operacji.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 593706de23d370d6bff03c97bb50e064d77a52ef
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350735"
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>Błąd narzędzia IBTool: Nie można ukończyć operacji.

## <a name="fixed-in-xcode-611"></a>Rozwiązane w programie Xcode 6.1.1

Apple [stałej](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) to `ibtool` usterki w programie Xcode 6.1.1, więc uaktualnianie do programu Xcode 6.1.1 lub nowszej jest najprostszym poprawki.

* * *

## <a name="description-of-the-problem"></a>Opis problemu

`ibtool` Polecenia w narzędziu Xcode 6.0 miał usterkę w systemie OS X 10.10 Yosemite. Program Xcode korzysta z platformy Xamarin.iOS `ibtool` skompilować scenorysy i `XIB` plików.

Więcej informacji na temat błędów w odniesieniu do środowiska Xcode, można znaleźć na następujących wpis w witrynie Stack Overflow: [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>komunikat o błędzie

> Nie można otworzyć dokumentu "MainStoryboard.storyboard". Nie można ukończyć operacji. (błąd com.apple.InterfaceBuilder -1.)

## <a name="workarounds-for-xcode-60"></a>Rozwiązanie problemu spowodowanego (program Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Opcja 1: Zarządzanie wszystkimi `UIImageView.Image` właściwości w kodzie

Zamiast ustawienie `Image` właściwość `UIImageView` w serii ujęć lub `.xib` pliku, można ustawić właściwości w jednym widoku cyklu życia zastąpienie metody kontrolera widoku (na przykład w `ViewDidLoad()`). Zobacz też [Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md) porady dotyczące korzystania `UIImage.FromBundle()` a `UIImage.FromFile()`.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Opcja 2: Przenieś wszystkie zasoby obrazów do najwyższego poziomu `Resources` folderu

Po przeniesieniu obrazy do najwyższego poziomu `Resources` folderu, należy zaktualizować scenorysu i `.xib` plików w celu używania nowej ścieżki obrazu.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Opcja 3: Ustaw `LogicalName` dla dowolnego zasoby problematyczne obrazów, dzięki czemu są one kopiowane na najwyższym poziomie`.app` pakietu

Załóżmy na przykład, oryginalny `.csproj` plik zawiera następujący wpis:

`<BundleResource Include="Resources\Images\image.png" />`

Można zmienić ten element i dodać `LogicalName` tak, aby obraz, który zamiast tego zostanie skopiowany do najwyższego poziomu `.app `pakietu:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

W programie Visual Studio dla komputerów Mac `LogicalName` można również ustawić za pomocą `Resource ID` pola obrazu w obszarze **Widok > okienka > właściwości**. (Zobacz również: [ http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545 ](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

Po tej zmianie, musisz zaktualizować scenorysu i `.xib` plików w celu używania nowej ścieżki obrazu najwyższego poziomu. Visual Studio dla komputerów Mac automatycznie aktualizuje listę autocompletions dla `Image` właściwości w narzędziu iOS Designer. W programie Visual Studio należy ręcznie edytować ścieżkę. Narzędzia iOS Designer następnie wyświetli się to jako brakujący obraz, ale projekt będzie kompilacji i działać poprawnie.

### <a name="next-steps"></a>Następne kroki

Aby uzyskać dalszą pomoc, skontaktuj się z nami lub jeśli ten problem pozostaje nawet po zakończeniu wykorzystujących powyższe informacje, zobacz [jakie opcje pomocy technicznej są dostępne dla programu Xamarin?](~/cross-platform/troubleshooting/support-options.md) uzyskać informacji na temat opcji kontaktu, sugestie, a także jak Zgłoś usterkę nowy, jeśli to konieczne. 

