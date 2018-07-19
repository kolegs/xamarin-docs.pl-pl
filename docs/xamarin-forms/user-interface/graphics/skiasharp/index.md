---
title: Korzystanie z biblioteki SkiaSharp w interfejsie Xamarin.Forms
description: Skiasharp — to system grafika 2D dla platformy .NET i języka C# obsługiwane przez aparat grafiki Skia typu open source, który jest szeroko stosowany w produktach firmy Google. Ten przewodnik wyjaśnia, jak SkiaSharp na użytek grafika 2D w aplikacjach Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: bdc0603c6ae406adac42533e251106ccccf2cb7c
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130949"
---
# <a name="using-skiasharp-in-xamarinforms"></a>Korzystanie z biblioteki SkiaSharp w interfejsie Xamarin.Forms

_Skiasharp — na użytek grafika 2D w aplikacjach Xamarin.Forms_

Skiasharp — to system grafika 2D dla platformy .NET i języka C# obsługiwane przez aparat grafiki Skia typu open source, który jest szeroko stosowany w produktach firmy Google. Skiasharp — można użyć w swoich aplikacjach Xamarin.Forms do rysowania grafiki wektorowej 2D, mapy bitowe i tekst. Zobacz [rysowania 2D](~/graphics-games/skiasharp/index.md) przewodnika, aby uzyskać więcej ogólnych informacji na temat biblioteki SkiaSharp i kilka innych samouczków.

Tym przewodniku założono, że czytelnik zna programowania zestawu narzędzi Xamarin.Forms.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Seminarium internetowe: SkiaSharp dla platformy Xamarin.Forms**

## <a name="skiasharp-preliminaries"></a>Skiasharp — wymagania wstępne

Skiasharp — dla platformy Xamarin.Forms jest dostarczana jako pakiet NuGet. Po utworzeniu rozwiązania platformy Xamarin.Forms w programie Visual Studio lub Visual Studio dla komputerów Mac, aby wyszukać, można użyć Menedżera pakietów NuGet **SkiaSharp.Views.Forms** pakietu i dodaj go do rozwiązania. Jeśli zaznaczysz **odwołania** sekcji każdego projektu po dodaniu SkiaSharp, możesz zobaczyć, że różne **SkiaSharp** biblioteki zostały dodane do wszystkich projektów w rozwiązaniu.

Jeśli aplikacja Xamarin.Forms jest przeznaczona dla systemu iOS, należy użyć strony właściwości projektu, aby zmienić minimalny cel wdrożenia do wersji iOS 8.0.

W dowolnej C# strona używająca SkiaSharp warto uwzględnić `using` dyrektywy dla [ `SkiaSharp` ](https://developer.xamarin.com/api/namespace/SkiaSharp/) przestrzeń nazw, która obejmuje wszystkie SkiaSharp klas, struktur i wyliczeń, których można używać grafiki programowanie. Należy również `using` dyrektywy dla [ `SkiaSharp.Views.Forms` ](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) przestrzeni nazw dla klas, które są specyficzne dla platformy Xamarin.Forms. Jest to znacznie mniejszą przestrzeń nazw, przy użyciu klasy najważniejsza jest [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/). Ta klasa jest pochodną Xamarin.Forms `View` klasy i obsługuje dane wyjściowe SkiaSharp grafiki.

> [!IMPORTANT]
> `SkiaSharp.Views.Forms` Przestrzeń nazw zawiera także `SKGLView` klasę pochodzącą od `View` , ale korzysta ze specyfikacji OpenGL dla renderowania grafiki. W celu uproszczenia, ten przewodnik ogranicza się do `SKCanvasView`, ale przy użyciu `SKGLView` zamiast tego jest bardzo podobne.

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[Podstawy rysowania za pomocą biblioteki SkiaSharp](basics/index.md)

Najprostsza grafiki danych liczbowych można narysować SkiaSharp należą okręgów, elips i prostokąty. W trakcie wyświetlania tych wyników, dowiesz się o współrzędne SkiaSharp, rozmiarów i kolorów.

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[SkiaSharp — linie i ścieżki](paths/index.md)

Ścieżki grafiki jest szereg podłączonych bezpośrednio linii i krzywych. Ścieżki mogą być malowania, wypełnione, lub obu. Ten temat obejmuje wiele aspektów Rysowanie linii, na tym kończy się obrysu i sprzężeń i linia przerywana i linii kropkowanej, ale zatrzymuje ograniczony krzywej geometrii.

## <a name="skiasharp-transformstransformsindexmd"></a>[Przekształcenia biblioteki SkiaSharp](transforms/index.md)

Przekształcenia dopuszcza się użycia obiektów grafiki równomiernie translacji, skalować, obracać, lub nierówne. W tym artykule przedstawiono również wykorzystania standardowa macierzy transformacji 3 x 3 tworzenia przekształcenia nieafiniczne i stosowanie przekształceń do ścieżki.

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[SkiaSharp — krzywe i ścieżki](curves/index.md)

Eksploracja ścieżek będzie kontynuowane przy użyciu Dodawanie krzywych do obiektów ścieżki i wykorzystanie innych funkcji zaawansowanych ścieżki. Zobaczysz, jak określić pełną ścieżkę, w ciągu tekstowym zwięzły, jak używać efekty ścieżek i jak zagłębiania się w ścieżce elementy wewnętrzne.

## <a name="skiasharp-bitmapsbitmapsindexmd"></a>[Mapy bitowe SkiaSharp](bitmaps/index.md)

Mapy bitowe mają rozmiar tablice prostokątne odpowiadający pikseli urządzenia wyświetlającego bitów. W tej serii artykułów pokazano, jak załadować, Zapisz, wyświetlanie, tworzenie, rysować na, animować i dostępu do bity mapy bitowe SkiaSharp.

## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Skiasharp — przy użyciu zestawu narzędzi Xamarin.Forms seminarium internetowym (wideo)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
