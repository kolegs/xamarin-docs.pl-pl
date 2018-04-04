---
title: Czy jest możliwe utworzenie archiwum .xcarchive z programu Visual Studio
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 23af3bf277ab68ffe93df2e1ee8ee64afc01828d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Czy jest możliwe utworzenie archiwum .xcarchive z programu Visual Studio

## <a name="for-xamarin-4"></a>Xamarin 4

Począwszy od platformy Xamarin 4.x, jest teraz możliwe tworzenie `.xcarchive` z systemu Windows przez ustawienie `ArchiveOnBuild` właściwości `true`. Na przykład za pomocą `MSBuild` w wierszu polecenia:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

`.xcarchive` Zostaną umieszczone w `$HOME/Library/Developer/Xcode/Archives` katalogu na hoście kompilacji Mac zarówno Xcode i Xamarin Studio Szukaj w celu wyświetlania wcześniejsza kompilacja archiwa.

Zobacz to [post fora Xamarin](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) dla niektórych krótki dodatkowe uwagi o `ArchiveOnBuild` właściwości. Można znaleźć w dokumentacji [wiersza polecenia platformy Xamarin.iOS kompilacje w systemie Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) dodatkowe szczegółowe informacje na temat `ServerAddress` i `ServerUser` właściwości.

* * *

## <a name="for-xamarin-3-and-earlier"></a>Dla platformy Xamarin 3 i starszych wersji

Rozszerzenie programu Visual Studio 3.x Xamarin nie udostępniają mechanizm do tworzenia `.xcarchive` archiwa. Inaczej mówiąc, używany do tworzenia logiki `.xcarchive` archiwa w programie Xamarin Studio dla komputerów Mac [opisany tutaj](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), więc prawdopodobnie można tworzyć własne `.xcarchive` ręcznie, jeśli użytkownik zamierza.

Warto zauważyć, że nie jest wymagane, ale `.xcarchive` do przesłania do sklepu z aplikacjami. Tak długo, jak jest podpisany z profilem aplikacji sklepu dystrybucji (nie Ad Hoc profil dystrybucji) można przesłać plik IPA.

W rzeczywistości użytkownik może nawet po prostu zip `.app` pakietu (do którego jest podpisany za pomocą profilu przechowywania dystrybucji aplikacji) i Prześlij formularz, który `.zip` pliku do sklepu z aplikacjami.

W obu przypadkach można użyć aplikacji moduł ładujący aplikacji do przesyłania aplikacji (zamiast Xcode).

