---
title: Osadzanie kodu .NET
description: 'Osadzanie .NET umożliwia istniejącego kodu platformy .NET (C#, F # i inne) do użycia z innych języków programowania'
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: f6edf25faa00bc7c90a52b76a6e90168ccd85b32
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="net-embedding"></a>Osadzanie kodu .NET

![Wersja zapoznawcza](~/media/shared/preview.png)

Osadzanie .NET umożliwia istniejącego kodu platformy .NET (C#, F # i inne) do użycia z innych języków programowania i w różnych środowiskach różnych.

Oznacza to, że jeśli biblioteki .NET, która ma być używany z istniejącej aplikacji systemu iOS, możesz to zrobić.   Lub jeśli chcesz połączyć ją z natywnej biblioteki języka C++, możesz też to zrobić.   Zawartość lub korzystać z kodu platformy .NET w języku Java.

Osadzanie .NET jest oparta na [Embeddinator 4000](https://github.com/mono/Embeddinator-4000) projekt open source.

## <a name="environments-and-languages"></a>Środowisk i języki

Narzędzie to jest zarówno pamiętać o środowisku, który będzie używany, a także język, w którym będą korzystać z jej.   Na przykład w przypadku platformy iOS dopuszczają just in time (JIT) kompilacji, .NET osadzanie statycznie zostanie skompilowany kod .NET kodu natywnego, które mogą być używane w systemie iOS.  Innych środowisk Zezwalaj kompilacji JIT, a w tych enviroments możemy zdecydować się na kompilacji JIT.

Obsługuje ona różnych języka konsumentów, więc go powierzchnie kodu platformy .NET idiomatyczne kodu w języku docelowym.   Listę obsługiwanych języków jest obecnie:

- [**Objective-C** ](objective-c/index.md) — mapowania .NET do idiomatyczne interfejsów API języka Objective C
- [**Java** ](android/index.md) — mapowania .NET do idiomatyczne interfejsów API języka Java
- [**C** ](get-started/c.md) — mapowanie .NET do zorientowane obiektowo takich jak C interfejsów API

Więcej języków zostanie nastąpić później.

## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć, sprawdź jedną z naszych wskazówek dla każdego z aktualnie obsługiwanych języków:

- [**Objective-C** ](get-started/objective-c/index.md) — obejmuje system macOS i iOS
- [**Java** ](get-started/java/index.md) — obejmuje system macOS i Android
- [**C** ](get-started/c.md) — obejmuje języka C na platformach pulpitu

## <a name="related-links"></a>Linki pokrewne

- [Embeddinator-4000 w witrynie GitHub](https://github.com/mono/Embeddinator-4000)
