---
title: Osadzanie kodu .NET
description: 'Osadzanie kodu .NET umożliwia istniejącego kodu .NET (C#, F # i inne) do użycia przez kod napisany w innych językach programowania.'
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 16f59498a49d10a43e04989136d8835bf78bd89d
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38830403"
---
# <a name="net-embedding"></a>Osadzanie kodu .NET

![Wersja zapoznawcza](~/media/shared/preview.png)

Osadzanie kodu .NET umożliwia istniejącego kodu .NET (C#, F # i inne) do użycia w innych językach programowania i w różnych środowiskach różnych.

Oznacza to, że jeśli masz bibliotekę .NET, którą chcesz korzystać z istniejących aplikacji systemu iOS, możesz to zrobić.   Lub jeśli chcesz połączyć je z natywną bibliotekę języka C++, również to zrobić.   Lub wykorzystali kodu platformy .NET w języku Java.

Osadzanie kodu .NET jest oparta na [Embeddinator 4000](https://github.com/mono/Embeddinator-4000) projekt typu open source.

## <a name="environments-and-languages"></a>Środowisk i języków

To narzędzie jest zarówno pamiętać o środowisku, który będzie używany, a także język, który zajmie on.   Na przykład platformę iOS nie zezwala na just-in-time (JIT) kompilacja, dzięki czemu osadzanie kodu .NET statycznie skompilowany kod .NET do kodu macierzystego, który może służyć w systemie iOS.  Innych środowisk zezwalają na kompilację JIT, a w tych środowiskach, możemy wybrać opcję kompilacji JIT.

Obsługuje ona różne konsumentów języka, więc go wydobywa informacje dotyczące kodu platformy .NET jako idiomatyczną kod w języku docelowym.   Listę obsługiwanych języków jest obecnie:

- [**Języka Objective-C** ](objective-c/index.md) — mapowanie .NET idiomatyczną interfejsów API języka Objective-C
- [**Java** ](android/index.md) — mapowanie .NET idiomatyczną interfejsów API języka Java
- [**C** ](get-started/c.md) — mapowanie .NET do zorientowane obiektowo takich jak interfejsy API języka C

Więcej języków zostanie dodana później.

## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć pracę, sprawdź jedną z naszych przewodników dla każdego z aktualnie obsługiwane języki:

- [**Języka Objective-C** ](get-started/objective-c/index.md) — obejmuje macOS i iOS
- [**Java** ](get-started/java/index.md) — obejmuje macOS i Android
- [**C** ](get-started/c.md) — obejmuje język C w klasycznej platformy

## <a name="related-links"></a>Linki pokrewne

- [Embeddinator-4000 w witrynie GitHub](https://github.com/mono/Embeddinator-4000)
