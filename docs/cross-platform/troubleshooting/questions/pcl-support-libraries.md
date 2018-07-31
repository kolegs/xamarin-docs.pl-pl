---
title: Jak mogę wyświetlić biblioteki obsługiwane w aplikacji PCL?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: asb3993
ms.author: amburns
ms.date: 07/27/2018
ms.openlocfilehash: 7e1017baf7daed68b5e55319a9ce13a4a2df5f2e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351473"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Jak mogę wyświetlić biblioteki obsługiwane w aplikacji PCL?

- Można znaleźć przegląd różnych funkcjach obsługiwanych przez różne platformy docelowe PCL w obszarze *obsługiwane funkcje* części tej strony: [https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

- Innym rozwiązaniem jest użycie [narzędzia .NET Portability Analyzer](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) do oceny, czy profil PCL można przekonwertować istniejącej biblioteki.

- Trzecia możliwość polega na do przeglądania zawartości rzeczywiste profil, który może używać. Na przykład przy użyciu profilu 78, można przejść w tym miejscu: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` i wyświetlać wszystkie zestawy znajdujące się w nim.

Niezależnie od metody wybranej,. należy pamiętać, że niektóre funkcje mają zostać pobrane za pośrednictwem NuGet i biblioteki Microsoft BCL.
