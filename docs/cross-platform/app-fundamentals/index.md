---
title: Udostępnianie kodu na wielu platformach
description: Ten dokument prowadzi różne przewodniki z instrukcjami, które opisują technik do udostępniania kodu, w tym bibliotek klas przenośnych, projekty udostępnione, .NET Standard i NuGet.
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 3a2c3f98e3ba83db0794a68ff1d62a9845a111c0
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270192"
---
# <a name="sharing-code-on-multiple-platforms"></a>Udostępnianie kodu na wielu platformach

Artykuły te wyjaśniają zasady różnymi opcjami dostępnymi współdzielenie kodu między platformami, w tym Windows, Android, iOS i innych.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Omówienie udostępniania kodu](code-sharing.md)

Więcej informacji na temat różnych udostępniania opcje dostępne dla projektów platformy Xamarin, w tym standardowych bibliotek .NET i udostępnionych projektów kodu. Obsługiwane są również przenośnej biblioteki klas, jednak są uznawane za przestarzałe na rzecz .NET Standard.

## <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard jest preferowaną opcją do udostępniania kodu między platformami. Kod jest kompilowany dla określonej wersji (w wersji 2.0 zapewnia najlepsze zgodnością z interfejsem API przy użyciu istniejącego kodu .NET Framework) i następnie mogą być wykorzystane przez inne projekty, które obsługują tego poziomu lub nowszej. Projekty .NET standard są obsługiwane zarówno w programie Visual Studio 2017 i Visual Studio dla komputerów Mac.

## <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Projekty udostępnione](~/cross-platform/app-fundamentals/shared-projects.md)

Projekty udostępnione umożliwiają napisanie wspólnej kod, który odwołuje się do niej kilka projektów innej aplikacji. Kod jest kompilowany jako część każdego projektu z odwołaniem i może zawierać dyrektywy kompilatora ułatwiające włączać funkcje specyficzne dla platformy w podstawowym współużytkowanym kodem. W tym artykule omówiono, jak działają projekty udostępnione i jak tworzyć i używać ich z projektami Xamarin.

## <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Biblioteki klas przenośnych](~/cross-platform/app-fundamentals/pcl.md)

Przenośne biblioteki klas projektów pozwalają na tworzenie i dystrybuowanie zestawy zawierające kod udostępniony do uruchamiania na wielu platformach. Do tworzenia biblioteki klas przenośnych (lub "PCL") należy najpierw wybrać które platformy docelowej, a następnie napisać kod w odniesieniu do podzbioru .NET Framework, która jest dostępna w profilu zdefiniowane dla tych platform. PCLs są uznawane za przestarzały w najnowszej wersji programu Visual Studio; Deweloperzy są zachęcani do użycia zamiast kodu .NET Standard 2.0.

## <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[Projekty NuGet: bibliotek dla wielu platform, na potrzeby udostępniania kodu](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

Pakiety NuGet, które mogą być automatycznie generowane z projektów PCL lub .NET standard; i projekty udostępnione można spakować w pakiety NuGet "przynęta i przełącznik" przy użyciu oddzielnych NuGet typu projektu. W tej sekcji opisano sposób tworzenia pakietów NuGet dla każdego scenariusza udostępniania kodu.

## <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Ręczne tworzenie pakietów NuGet dla platformy Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Wskazówki dotyczące tworzenia pakietów NuGet, które działają na platformie Xamarin.
