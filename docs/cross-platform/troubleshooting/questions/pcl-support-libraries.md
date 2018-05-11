---
title: Jak wyświetlić, jakie biblioteki są obsługiwane w PCL?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: asb3993
ms.author: amburns
ms.openlocfilehash: 87f65ba2cff2d5990c32aa142f97766a76d6ba05
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Jak wyświetlić, jakie biblioteki są obsługiwane w PCL?

- Można znaleźć omówienie różne funkcje obsługiwane przez różnych platform docelowych PCL w obszarze *obsługiwane funkcje* część tej strony: [http://msdn.microsoft.com/library/gg597391.aspx](https://msdn.microsoft.com/library/gg597391.aspx)

- Inną opcją jest użycie [.NET przenośność analizator](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) do oceny, czy profil PCL można przekonwertować istniejącej biblioteki.

- Trzecia możliwość jest aby przeglądać zawartość rzeczywiste profil, który może używać. Na przykład przy użyciu profil 78, można przejść w tym miejscu: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` i wyświetlić wszystkie zestawy w niej.

Niezależnie od metody wybrano, proszę należy pamiętać, że niektóre funkcje ma zostać pobrane za pomocą NuGet i biblioteki BCL firmy Microsoft.
