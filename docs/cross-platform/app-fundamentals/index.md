---
title: Udostępnianie kodu
description: Podstawowe koncepcje aplikacji
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 4cca202627d1b901e00532c92598ffddd48b4853
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="sharing-code"></a>Udostępnianie kodu

Ta sekcja zawiera przewodnik dla niektórych zadań rzeczy częściej lub pojęcia, które deweloperzy muszą znać podczas opracowywania aplikacji dla urządzeń przenośnych.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Omówienie udostępniania kodu](code-sharing.md)

Więcej informacji na temat kodu różne opcje dostępne dla projektów Xamarin, łącznie z przenośnej biblioteki klas (PCLs), udostępnionych projektów i standardowych bibliotek .NET udostępniania.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Biblioteki klas przenośnych](~/cross-platform/app-fundamentals/pcl.md)

Projektów biblioteki klas przenośnych umożliwiają tworzenie i dystrybucji zestawów zawierających udostępniony kod wymagany do uruchomienia na wielu platformach. Tworzenie przenośnej biblioteki klas (lub "PCL") najpierw wybrać platformy, na których docelowego, a następnie pisać kod dla podzbioru .NET Framework, która jest dostępna w profilu zdefiniowane dla tych platform. Ten dokument zawiera opis sposobu tworzenia i używania PCLs za pomocą platformy Xamarin.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Projekty udostępnione](~/cross-platform/app-fundamentals/shared-projects.md)

Udostępnionych projektów umożliwiają pisanie wspólnej kodu, który odwołuje się wiele projektów różnych aplikacji. Kod jest skompilowana jako część każdego odwołaniem do projektu i może zawierać dyrektywy kompilatora, aby obsługiwać funkcje specyficzne dla platformy w podstawowym udostępnionego kodu. W tym artykule omówiono sposób działania udostępnionych projektów i jak utworzyć i używać go z projektami Xamarin.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard to nowa opcja udostępniania kodu na platformach. Działa w sposób podobny do bibliotek klas przenośnych; Kod utworzono przy użyciu określonej wersji (obecnie 1.0 za pośrednictwem 1.6), a następnie mogą być wykorzystane przez inne projekty, które obsługuje tego poziomu lub nowszej. Projekty platformy .NET standard są obsługiwane w Xamarin Studio 6.2, Visual Studio dla systemu Windows i programu Visual Studio dla komputerów Mac.

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet projekty: Biblioteki dla wielu platform udostępnianie kodu](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

Pakiety NuGet, które mogą być automatycznie generowane przez PCL lub platformy .NET standard projektów; i udostępnionych projektów można spakować do pakietów NuGet "przynęta i przełącznik" przy użyciu osobnych typu projektu NuGet. W tej sekcji opisano sposób tworzenia pakietów NuGet dla każdego scenariusza udostępniania kodu.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Ręczne tworzenie pakietów NuGet dla platformy Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Wskazówki dotyczące tworzenia pakietów NuGet, które współpracują z platformą Xamarin.
