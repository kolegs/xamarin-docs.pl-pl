---
title: 'Błąd IBTool: Nie można ukończyć operacji.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 4647227ad208bfa968f8282a966220a09ab7f4a6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="ibtool-error-the-operation-couldnt-be-completed"></a>Błąd IBTool: Nie można ukończyć operacji.

## <a name="fixed-in-xcode-611"></a>Usunięto w programie Xcode 6.1.1

Apple [stałej](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) to `ibtool` usterki w programie Xcode 6.1.1, więc uaktualnianie xcode 6.1.1 lub nowszej jest najprostszym.

* * *

## <a name="description-of-the-problem"></a>Opis problemu

`ibtool` Polecenie w programie Xcode 6.0 miał usterki na OS X 10.10 Yosemite. W środowisku Xcode korzysta z platformy Xamarin.iOS `ibtool` skompilować scenorys i `XIB` plików.

Więcej informacji na temat błędów w odniesieniu do Xcode, można znaleźć na następujących przepełnienie stosu post: [http://stackoverflow.com/questions/25754763/cant-open-storyboard](http://stackoverflow.com/questions/25754763/cant-open-storyboard)

### <a name="error-message"></a>Komunikat o błędzie

> Nie można otworzyć dokumentu "MainStoryboard.storyboard". Nie można ukończyć operacji. (błąd com.apple.InterfaceBuilder -1.)

## <a name="workarounds-for-xcode-60"></a>Rozwiązanie problemu spowodowanego (Xcode 6.0)

### <a name="option-1-manage-all-uiimageviewimage-properties-in-code"></a>Opcja 1: Zarządzanie wszystkimi `UIImageView.Image` właściwości w kodzie

Zamiast ustawienie `Image` właściwość `UIImageView` w scenorysu lub `.xib` plików, można ustawić właściwości w jednym widoku metody zastąpienie cyklu życia w kontroler widoku (na przykład w `ViewDidLoad()`). Zobacz też [Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md) porady dotyczące korzystania `UIImage.FromBundle()` a `UIImage.FromFile()`.

### <a name="option-2-move-all-of-the-image-resources-to-the-top-level-resources-folder"></a>Opcja 2: Przenieś wszystkie zasoby obrazów do najwyższego poziomu `Resources` folderu

Po przeniesieniu obrazów na najwyższym poziomie `Resources` folderu, musisz zaktualizować scenorysu i `.xib` plików do użycia nowej ścieżki obrazu.

### <a name="option-3-set-the-logicalname-for-any-problematic-image-assets-so-they-are-copied-to-the-top-level-of-theapp-bundle"></a>Opcja 3: Ustaw `LogicalName` żadnych zasobów obrazu problemy, są one kopiowane na najwyższym poziomie w`.app` pakietu

Na przykład oryginalny `.csproj` plik zawiera następujący wpis:

`<BundleResource Include="Resources\Images\image.png" />`

Możesz zmienić ten element i Dodaj `LogicalName` tak, aby obraz zamiast tego zostanie skopiowany na najwyższym poziomie `.app `pakietu:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

W programie Visual Studio for Mac `LogicalName` można również ustawić za pomocą `Resource ID` dla obrazu w pole **Widok > konsole > właściwości**. (Zobacz też: [ http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545 ](http://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

Po tej zmianie, musisz zaktualizować scenorysu i `.xib` plików do użycia nowej ścieżki obrazu najwyższego poziomu. Automatycznie aktualizuje listę autocompletions dla programu Visual Studio for Mac `Image` właściwości w systemie iOS projektanta. W programie Visual Studio musisz ręcznie edytować ścieżkę. Projektant z systemem iOS będzie wyświetlana to jako obraz Brak, ale projekt skompiluje oraz działała poprawnie.

### <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkową pomoc, skontaktuj się z nami, lub jeśli ten problem pozostanie nawet po użyciu powyższe informacje, zobacz [jakie opcje pomocy technicznej są dostępne dla platformy Xamarin?](~/cross-platform/troubleshooting/support-options.md) informacji o opcjach kontaktu, masz sugestie, oraz jak Plik nowe usterki, w razie potrzeby. 

