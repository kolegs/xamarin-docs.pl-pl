---
title: Jak obniżyć pakietu NuGet?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 2eec947ab748eccd45971054f2f5b232cf788cfa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Jak obniżyć pakietu NuGet?

Visual Studio for Mac & Visual Studio ma funkcje wyboru starsze wersje pakietów i instalowania ich automatycznie; Podobnie jak aktualizowanie pakietów działa. Te kroki są opisane poniżej.

## <a name="visual-studio"></a>Visual Studio
1. Przejdź do **Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów**
2. Ustaw projekt w obszarze **domyślny projekt**
3. Należy użyć następującej składni:

    > Install-Package [PackageName]-wersji [tab wersji menu]

Użytkownik może również kopiowania/wklejania pełne polecenie z pakietu NuGet strony. Przykład platformy Xamarin.Forms: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. W projekcie, kliknij prawym przyciskiem myszy folder pakietów i zaznacz **Dodawanie pakietów**
2. W searchbar następującej składni służy do wyszukiwania wymagane pakiety:

    `[PackageName] version:*`

### <a name="examples"></a>Przykłady 
- Wyświetla listę wszystkich pakietów platformy Xamarin.Forms: 

    `Xamarin.Forms version:`
- Wyświetla listę wszystkich pakietów 1.4.x platformy Xamarin.Forms: 

    `Xamarin.Forms version:1.4`

*Uwaga: Jeśli dodasz odstęp między `version:` & numer wersji, wyszukiwanie będzie działać tak, jakby została określona żadna wersja.*

