---
title: Czy jest możliwe tworzenie archiwum .xcarchive w programie Visual Studio
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 1c20534e1d4476ce7ff85d9ffd5ae8742c445983
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350213"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Czy jest możliwe tworzenie archiwum .xcarchive w programie Visual Studio

## <a name="for-xamarin-4"></a>Dla programu Xamarin 4

Począwszy od platformy Xamarin 4.x, jest teraz możliwość utworzenia `.xcarchive` z Windows, ustawiając `ArchiveOnBuild` właściwość `true`. Na przykład za pomocą `MSBuild` w wierszu polecenia:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

`.xcarchive` Zostaną umieszczone w `$HOME/Library/Developer/Xcode/Archives` katalogu hostem kompilacji komputera Mac, że środowisko Xcode i Xamarin Studio wyszukiwania do archiwów wyświetlana wcześniejsza kompilacja.

Zobacz ten [wpis fora platformy Xamarin](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) dla niektórych krótki dodatkowe uwagi dotyczące `ArchiveOnBuild` właściwości. Zobacz dokumentację na temat [kompilacje z wiersza polecenia platformy Xamarin.iOS na Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) dodatkowe informacje szczegółowe o `ServerAddress` i `ServerUser` właściwości.

* * *

## <a name="for-xamarin-3-and-earlier"></a>Dla platformy Xamarin, 3 i starszych

Rozszerzenie programu Visual Studio 3.x Xamarin nie zapewnia mechanizm do tworzenia `.xcarchive` archiwa. Inaczej mówiąc, przez logikę używaną do tworzenia `.xcarchive` archiwów w programie Xamarin Studio dla komputerów Mac [opisany tutaj](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), więc prawdopodobnie można tworzyć własne `.xcarchive` ręcznie, jeśli użytkownik zamierza.

Warto zauważyć, że nie jest wymagane, ale `.xcarchive` do przesłania do Store aplikacji. Możesz przesłać plik IPA, tak długo, jak jest podpisany za pomocą profilu dystrybucji Store App (nie Ad Hoc profil dystrybucji).

W rzeczywistości, nawet po prostu można spakowanie `.app` pakietu (który jest podpisany przy użyciu profilu dystrybucji App Store) i przesłać, które `.zip` pliku do sklepu z aplikacjami.

W obu przypadkach można użyć aplikacji program Application Loader, można przesłać aplikacji (zamiast Xcode).

