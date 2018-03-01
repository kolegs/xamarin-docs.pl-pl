---
title: .NET 2.0 Pomoc techniczna standard w platformy Xamarin.Forms
description: "W tym artykule opisano sposób konwertowania aplikację platformy Xamarin.Forms używać programu .NET 2.0 standardowa."
ms.topic: article
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 7923ccdf85ffcbbc239df9e9df751f561615baa1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="net-standard-20-support-in-xamarinforms"></a>.NET 2.0 Pomoc techniczna standard w platformy Xamarin.Forms

_W tym artykule opisano sposób konwertowania aplikację platformy Xamarin.Forms używać programu .NET 2.0 standardowa._

.NET standard jest specyfikacją interfejsów API platformy .NET, które mają być dostępne na wszystkich implementacji .NET. Go ułatwia udostępnianie kodu w aplikacje komputerowe, aplikacje mobilne i gry i usługami w chmurze, przełączając identyczne interfejsów API na różnych platformach. Informacje o platformach obsługiwanych przez .NET Standard, zobacz [Obsługa implementacji .NET](/dotnet/standard/net-standard#net-implementation-support/).

Biblioteki .NET standard to zastąpienia dla przenośnych klasy bibliotek PCL (). Jednak biblioteka, przeznaczonego dla platformy .NET Standard jest nadal PCL i jest nazywany PCL opartych na .NET Standard. Niektóre PCL profile są mapowane do wersji platformy .NET Standard i profilów, które ma mapowania, będzie mógł odwoływać się siebie dwóch biblioteki typów. Aby uzyskać więcej informacji, zobacz [zgodności PCL](/dotnet/standard/net-standard#pcl-compatibility).

Platformy Xamarin.Forms 2.4 umożliwia aplikacji platformy Xamarin.Forms docelowej .NET 2.0 standardowe przez zamianę PCL biblioteki .NET 2.0 standardowa. Można to osiągnąć w następujący sposób:

- Upewnij się, [.NET Core 2.0](https://www.microsoft.com/net/download/core) jest zainstalowany.
- Zaktualizuj platformy Xamarin.Forms rozwiązania do używania platformy Xamarin.Forms 2.4 lub nowszego.
- Dodaj bibliotekę .NET Standard do rozwiązania, przeznaczonego dla platformy .NET Standard 2.0.
- Usuń klasę, która jest dodawane do biblioteki .NET Standard.
- Dodaj pakiet NuGet 2.4 (lub nowszego) platformy Xamarin.Forms do biblioteki .NET Standard.
- Projekty platformy Dodaj odwołanie do biblioteki standardowej .NET i Usuń odwołanie do projektu PCL, który zawiera logikę interfejsu użytkownika platformy Xamarin.Forms.
- Skopiuj pliki z projektu PCL do biblioteki .NET Standard.
- Usuń projekt PCL, który zawiera logikę interfejsu użytkownika platformy Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Opcje udostępniania kodu](~/cross-platform/app-fundamentals/code-sharing.md)
