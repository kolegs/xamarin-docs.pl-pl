---
title: Rysowania 2D za pomocą SkiaSharp
description: Ten dokument zawiera omówienie 2D dla wielu platform, rysowanie przy użyciu SkiaSharp. Łączy różne przewodniki, które opisują SkiaSharp i jej różnych interfejsów API.
ms.prod: xamarin
ms.techonology: xamarin-skiasharp
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
ms.openlocfilehash: 7207f33e56f566a5528d93f9957e2ff780a22a65
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615525"
---
# <a name="2d-drawing-with-skiasharp"></a>Rysowania 2D za pomocą SkiaSharp

Skiasharp — udostępnia zaawansowany interfejs API języka C# do wykonywania grafika 2D. Jest obsługiwana przez [biblioteki Skia firmy Google](http://skia.org), tej samej bibliotece, zapewniająca Google Chrome, Firefox i systemu Android stosów grafiki.

[![](images/ide-sml.png "Skiasharp — udostępnia zaawansowany interfejs API języka C# do wykonywania grafika 2D")](images/ide.png#lightbox)

Skiasharp — jest przenośnej biblioteki i jest dostarczany w wygodny sposób jako [pakiet NuGet dla wielu platform](https://www.nuget.org/packages/SkiaSharp)i obsługuje następujące platformy gotowych: macOS, platformy Xamarin.Android, Xamarin.iOS i pulpitu Windows.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[Wprowadzenie do SkiaSharp](~/graphics-games/skiasharp/introduction.md)

Omówienie podstawowe pojęcia dotyczące SkiaSharp i przykładowy kod do renderowania grafiki, tekst, mapy bitowe oraz filtry obrazów.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[Skiasharp — samouczki dotyczące zestawu narzędzi Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Dowiedz się, jak pracować z wielu platform grafiki, renderowanie w interfejsie Xamarin.Forms:

- [Podstawy rysowania](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Rysowanie prostego okręgu](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Integracja z zestawem narzędzi Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Piksele i jednostki niezależne od urządzenia](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Podstawowa Animacja](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [Integrowanie tekstu i grafiki](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Mapa bitowa — podstawy](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Linie i ścieżki](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Linie i zakończenia pociągnięć](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Ścieżka — podstawy](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [Typy wypełnień ścieżek](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Linie łamane i równania parametryczne](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Kropki i kreski](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Malowanie palcami](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Przekształcenia](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [Przekład — przekształcenie](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [Skalowanie — przekształcenie](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [Obrót — przekształcenie](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [Pochylenie — przekształcenie](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Przekształcenia macierzowe](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Manipulacje za pomocą dotyku](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Inne niż affine — przekształcenia](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [Obrót 3D](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Krzywe i ścieżki](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Trzy sposoby narysowania łuku](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Trzy typy krzywych Béziera](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [Dane ścieżki SVG](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Obcinanie przy użyciu ścieżek i regionów](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Efekty ścieżek](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Ścieżki i tekst](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Wyliczanie i informacje o ścieżce](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)
- [Mapy bitowe](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/index.md)
  * [Wyświetlanie map bitowych](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/displaying.md)
  * [Tworzenie map bitowych i rysowanie na nich](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/drawing.md)
  * [Przycinanie map bitowych](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/cropping.md)
  * [Segmentowe wyświetlanie map bitowych](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/segmented.md)
  * [Zapisywanie map bitowych w plikach](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/saving.md)
  * [Uzyskiwanie dostępu do bitów pikseli mapy bitowej](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/pixel-bits.md)
  * [Animowanie map bitowych](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/animating.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[Uwagi dotyczące określonej platformy](~/graphics-games/skiasharp/platform.md)

Na tej stronie opisano instrukcje dotyczące SkiaSharp instalacji na różnych platformach, w tym z systemem iOS, Android, macOS i Windows.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[Dokumentacja interfejsu API](https://developer.xamarin.com/api/namespace/SkiaSharp/)

Możesz przeglądać [dokumentacji interfejsu API](https://developer.xamarin.com/api/namespace/SkiaSharp/) dla SkiaSharp w naszej witrynie sieci web.

## <a name="work-in-progress"></a>W toku

Skiasharp — jest w toku udostępniamy z naszą społecznością. Gdy firma Microsoft powiązania ważne elementy interfejsu API Skia dużo pracy pozostają do wykonania. Używamy udostępniane przez Skia stabilne interfejsu API języka C, a nasz plan jest, aby kontynuować, współtworzenia naszej pracy, do powiązania C Skia, aby zapewnić pełne pokrycie do interfejsów API.

Aby pomóc nam przewodnik naszych wysiłkach powiązania, pozostaw komentarze lub sugestie jako problemy w repozytorium GitHub [ http://github.com/mono/SkiaSharp ](http://github.com/mono/SkiaSharp).
