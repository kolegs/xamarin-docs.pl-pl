---
title: Przykład integracji
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 8c46fa05a8ffcedfa7fa9d1344946fac518b5036
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="sample-integrations"></a>Przykład integracji

Zobacz [Sink kuchni] [ KitchenSink] przykładowa przykład pracy integracji. Po prostu zbudować `KitchenSink.sln` w Visual Studio for Mac lub Visual Studio, a następnie otwórz `KitchenSink.workbook`.

[![Zrzut ekranu integracji zbiornika kuchni](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

Przykładowe Sink kuchni przedstawiono oba zestawy pojęcia:

* Elementy reprezentacja pokazują, jak używać `RepresentationManager` w celu zwiększenia renderowania za pomocą wbudowanych oświadczenia.
* `Person` Obiekt i jego skojarzony renderowania JavaScript pokazanie sposobu używania `ISerializableObject` bez przechodzenia przez dostawcę reprezentacji.

Zobacz też [SkiaSharp] [ skiasharp] przykład rzeczywistych integracji, który korzysta z istniejącej [reprezentacje](~/tools/workbooks/sdk/representations.md) dostarczonych przez Xamarin skoroszyty do renderowania jej typów.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks