---
title: "Przy użyciu SkiaSharp w platformy Xamarin.Forms"
description: "Użyj SkiaSharp 2D grafiki w aplikacji platformy Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: eaebd8cbae996e9a5792d0a4898fafb72bdded47
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="using-skiasharp-in-xamarinforms"></a>Przy użyciu SkiaSharp w platformy Xamarin.Forms

_Użyj SkiaSharp 2D grafiki w aplikacji platformy Xamarin.Forms_

SkiaSharp to system grafiki 2D .NET i C# obsługiwane przez aparat grafiki Skia open source, która jest często używanych w produktach firmy Google. SkiaSharp w aplikacji platformy Xamarin.Forms służy do rysowania grafiki wektorowej 2D, mapy bitowe i tekst. Zobacz [2D rysunku](~/graphics-games/skiasharp/index.md) przewodnik więcej ogólnych informacji na temat biblioteki SkiaSharp i kilka innych samouczków.

W tym przewodniku założono, że czytelnik zna programowania platformy Xamarin.Forms.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Webinar: SkiaSharp dla platformy Xamarin.Forms**

## <a name="skiasharp-preliminaries"></a>Czynności wstępne SkiaSharp

SkiaSharp dla platformy Xamarin.Forms jest dostarczana jako pakietu NuGet. Po utworzeniu rozwiązania platformy Xamarin.Forms w programie Visual Studio lub Visual Studio dla komputerów Mac, aby wyszukać można użyć Menedżera pakietów NuGet **SkiaSharp.Views.Forms** pakiet, a następnie dodaj go do rozwiązania. Jeśli wybierzesz **odwołania** sekcji każdego projektu po dodaniu SkiaSharp można stwierdzić, że różne **SkiaSharp** biblioteki zostały dodane do wszystkich projektów w rozwiązaniu.

Jeśli w aplikacji platformy Xamarin.Forms przeznaczony dla systemu iOS, należy użyć strony właściwości projektu, aby zmienić cel wdrożenia minimalna do wersji iOS 8.0.

W dowolnym C# strona używa SkiaSharp należy uwzględnić `using` dyrektywy dla [ `SkiaSharp` ](https://developer.xamarin.com/api/namespace/SkiaSharp/) przestrzeni nazw, która obejmuje wszystkie SkiaSharp klas, struktur i wyliczenia, które będą używane grafiki programowania. Należy również `using` dyrektywy dla [ `SkiaSharp.Views.Forms` ](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) przestrzeń nazw dla klasy specyficzne dla platformy Xamarin.Forms. Jest znacznie mniejszy przestrzeni nazw, klasy najważniejszych jest [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/). Ta klasa pochodzi od platformy Xamarin.Forms `View` klasy i obsługuje grafiki SkiaSharp dane wyjściowe.

> [!IMPORTANT]
> `SkiaSharp.Views.Forms` Przestrzeń nazw zawiera także `SKGLView` klasą pochodzącą z `View` , ale używa OpenGL do renderowania grafiki. W celu uproszczenia, w tym przewodniku ogranicza się do `SKCanvasView`, ale przy użyciu `SKGLView` zamiast tego jest bardzo podobne.

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[Podstawy rysowania za pomocą biblioteki SkiaSharp](basics/index.md)

Najprostsza dane grafiki rysowane z SkiaSharp należą okręgi, elipsy i prostokąty. W trakcie wyświetlania tych wartości, dowiesz się o współrzędnych SkiaSharp, rozmiarów i kolory.

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[SkiaSharp — linie i ścieżki](paths/index.md)

Ścieżki grafiki jest szereg połączonych prostej linii i krzywych. Ścieżki mogą być malowania, wypełniony, lub obie. Ten temat obejmuje wiele aspektów Rysowanie linii, na tym kończy się obrysu i sprzężenia i oznaczone kreskowaną i przerywana wierszy, ale zatrzymuje mają krzywej geometrię jest zbyt mała.

## <a name="skiasharp-transformstransformsindexmd"></a>[Przekształcenia biblioteki SkiaSharp](transforms/index.md)

Transformacje pozwalają obiektów grafiki jednolicie przetłumaczony, skalowania, obracać lub niesymetryczna. W tym artykule przedstawiono również używania standardowych macierzy transformacji 3 x 3 tworzenia-affine — przekształcenia i stosowanie przekształceń do ścieżki.

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[SkiaSharp — krzywe i ścieżki](curves/index.md)

Eksploracja ścieżek kontynuuje Dodawanie krzywych do obiektów ścieżki i wykorzystaniu innych funkcji zaawansowanych ścieżki. Zobaczysz, jak można określić pełną ścieżkę w ciągu tekstowym zwięzły, jak używać efekty ścieżki i dotyczące dalszego do ścieżki wewnętrzne.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [SkiaSharp z platformy Xamarin.Forms seminarium internetowego (klip wideo)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
