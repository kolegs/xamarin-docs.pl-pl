---
title: Skiasharp — linie i ścieżki
description: W tym artykule wyjaśniono, jak używać SkiaSharp Rysowanie linii i ścieżki grafiki w aplikacjach Xamarin.Forms i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 9febfabb7b44b1ec09abda4b352691b37565cb48
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615138"
---
# <a name="skiasharp-lines-and-paths"></a>Skiasharp — linie i ścieżki

_Rysowanie linii i grafiki ścieżek za pomocą SkiaSharp_

[Poprzedniej sekcji](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) udowodnić, że SkiaSharp `SKCanvas` klasy zawiera kilka metod Rysowanie prostokątów, wielokropek i prostokąty zaokrąglony. Tej sekcji i w kolejnych sekcjach obejmują różnymi klasami, które są połączone z tworzeniem i renderowanie *ścieżki grafiki*.

Ścieżki grafiki jest najbardziej ogólnych podejście do rysowania linii i krzywych w SkiaSharp. W tej sekcji opisano przy użyciu `SKPath` obiektu, aby rysować linie proste i należy używać kolekcji bezpiecznych niewielki rozmiar linii prostych (o nazwie *linii łamanej*) do rysowania, które można zdefiniować matematycznych. Dalszej części tego tematu będzie w tym artykule omówiono różne rodzaje krzywych obsługiwane przez `SKPath`.

Wszystkich przykładowych programów w tej sekcji są wyświetlane pod nagłówkiem **linie i ścieżki** na stronie głównej [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program, a następnie w [ **Ścieżki** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths) folderu rozwiązania.

## <a name="lines-and-stroke-capslinesmd"></a>[Linie i zakończenia pociągnięć](lines.md)

Dowiedz się, jak używać SkiaSharp Rysowanie linii za pomocą różnych pociągnięć.

## <a name="path-basicspathsmd"></a>[Ścieżka — podstawy](paths.md)

Zapoznaj się z obiektu SkiaSharp SKPath łączenia linii i krzywych.

## <a name="the-path-fill-typesfill-typesmd"></a>[Typy wypełnień ścieżek](fill-types.md)

Poznaj różne efekty możliwego typy wypełnień ścieżek SkiaSharp.

## <a name="polylines-and-parametric-equationspolylinesmd"></a>[Linie łamane i równania parametryczne](polylines.md)

Skiasharp — Umożliwia renderowanie każdego wiersza, które można zdefiniować przy użyciu równania parametryczne.

## <a name="dots-and-dashesdotsmd"></a>[Kropki i kreski](dots.md)

Wzorca niewymagającego rysunku kropkowane i kreskowane wierszy w SkiaSharp.

## <a name="finger-paintingfinger-paintmd"></a>[Malowanie palcami](finger-paint.md)

Użyj palców do rysowania na kanwie.


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
