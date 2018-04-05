---
title: Linie SkiaSharp i ścieżek
description: Rysowanie linii i grafiki ścieżek za pomocą SkiaSharp
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 2f9941305f165ec04e5fc80e3c41e3150a21a9b7
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/05/2018
---
# <a name="skiasharp-lines-and-paths"></a>Linie SkiaSharp i ścieżek

_Rysowanie linii i grafiki ścieżek za pomocą SkiaSharp_

[Poprzedniej sekcji](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) wykazać, że SkiaSharp `SKCanvas` klasy są dostępne różne metody Rysowanie prostokątów, elipsy i prostokąty zaokrąglone. Tej sekcji i w kolejnych sekcjach opisano różne klasy związane z tworzeniem i renderowania *ścieżki grafiki*.

Ścieżki grafiki jest najbardziej ogólnych podejście do rysowania linii i krzywych w SkiaSharp. W tej sekcji omówiono przy użyciu `SKPath` do rysowania linii prostej i należy używać kolekcji niewielki rozmiar linii prostych obiektów (nazywane *linię łamaną*) do rysowania, które można zdefiniować ze sobą matematycznie. Dalszej części artykułu zostanie w tym artykule omówiono różne rodzaje krzywych obsługiwane przez `SKPath`.

Wszystkie przykładowe programy w tej sekcji są wyświetlane pod nagłówkiem **wierszy i ścieżki** na stronie głównej [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program oraz w [ **Ścieżki** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths) folderu rozwiązania.

## <a name="lines-and-stroke-capslinesmd"></a>[Linie i zakończenia pociągnięć](lines.md)

Dowiedz się, jak używać SkiaSharp do rysowania linii z informacji o różnych możliwościach.

## <a name="path-basicspathsmd"></a>[Ścieżka — podstawy](paths.md)

Poznaj obiektu SkiaSharp SKPath do łączenia linii i krzywych.

## <a name="the-path-fill-typesfill-typesmd"></a>[Typy wypełnień ścieżek](fill-types.md)

Poznaj efekty różnych możliwe typy wypełnienia ścieżki SkiaSharp.

## <a name="polylines-and-parametric-equationspolylinesmd"></a>[Linie łamane i równania parametryczne](polylines.md)

SkiaSharp używany do renderowania żadnych wiersz, który można zdefiniować z równania parametrycznych.

## <a name="dots-and-dashesdotsmd"></a>[Kropki i kreski](dots.md)

Zawiłościami Rysowanie linii kropkowanej i kreskowane w SkiaSharp.

## <a name="finger-paintingfinger-paintmd"></a>[Malowanie palcami](finger-paint.md)

Użyj palcami do rysowania na kanwie.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
