---
title: Jak obniżyć wersję pakietu NuGet?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 72fdf7246b148fa95ea312284957072ecda47121
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351219"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Jak obniżyć wersję pakietu NuGet?

Visual Studio dla komputerów Mac i Visual Studio mają funkcje wyboru starsze wersje pakietów i instalując je automatycznie. Podobnie jak aktualizowanie pakietów działa. Te kroki są opisane poniżej.

## <a name="visual-studio"></a>Visual Studio
1. Przejdź do **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**
2. Ustaw projekt w obszarze **domyślny projekt**
3. Użyj następującej składni:

    > Install-Package [Nazwa_pakietu]-[tab menu Wersja] wersja

Możesz również skopiować lub wkleić pełne polecenie z pakietu NuGet strony. Przykład dla platformy Xamarin.Forms: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. W projekcie, kliknij prawym przyciskiem myszy packages folder instrukcji & select **Dodawanie pakietów**
2. W searchbar można użyć następującej składni wyszukiwania wymaganych pakietów:

    `[PackageName] version:*`

### <a name="examples"></a>Przykłady 
- Wyświetla listę wszystkich pakietów platformy Xamarin.Forms: 

    `Xamarin.Forms version:`
- Wyświetla listę wszystkich pakietów 1.4.x Xamarin.Forms: 

    `Xamarin.Forms version:1.4`

*Uwaga: Jeśli dodasz odstęp między `version:` i numer wersji wyszukiwania będą zachowywać się tak, jakby nie określono wersji.*

