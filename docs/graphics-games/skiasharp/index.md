---
title: Rysunek 2D z SkiaSharp
description: Ten dokument zawiera omówienie 2W i platform Rysowanie za pomocą SkiaSharp. Łączy różnych prowadnice, które opisują SkiaSharp i jego różnych interfejsach API.
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: 962fe657f25976f9b5069f2d434e92f816d249ca
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783295"
---
# <a name="2d-drawing-with-skiasharp"></a>Rysunek 2D z SkiaSharp

SkiaSharp udostępnia zaawansowane API języka C# do wykonywania grafiki 2D. Jest obsługiwany przez [biblioteki Skia firmy Google](http://skia.org), tę samą bibliotekę obsługującego Google Chrome, Firefox i Android w stosach grafiki.

[![](images/ide-sml.png "SkiaSharp udostępnia zaawansowane API języka C# do wykonywania 2D grafiki")](images/ide.png#lightbox)

SkiaSharp jest przenośnej biblioteki i wygodnie dostarczany w formie [pakietu NuGet i platform](https://www.nuget.org/packages/SkiaSharp)i obsługuje następujące platformy fabrycznej: macOS, Xamarin.Android i Xamarin.iOS, pulpitu systemu Windows.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[Wprowadzenie do SkiaSharp](~/graphics-games/skiasharp/introduction.md)

Omówienie podstawowe koncepcje SkiaSharp i próbki kodu do renderowania grafiki, tekst, mapy bitowe i używać filtrów obrazu.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[SkiaSharp samouczki dotyczące platformy Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Dowiedz się, jak pracować z cross platform grafiki, renderowanie w platformy Xamarin.Forms:

- [Podstawy rysowania](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Rysowanie Proste kółko](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Integracja z zestawem narzędzi Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Pikseli i jednostki niezależnych od urządzenia](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Podstawowe animacji](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [Integrowanie tekstu i grafiki](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Podstawy mapy bitowej](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Linie i ścieżek](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Linie i obrysu CAP](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Podstawowe informacje o ścieżce](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [Typy wypełnienia ścieżki](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Linię i parametryczne równania](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Kropki i łączniki](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Malowanie palcami](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Przekształca](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [Przekształcanie Przetłumacz](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [Przekształcanie skali](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [Przekształcenie obracania](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [Przekształcanie pochylenia](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Przekształcenia macierzowe](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Manipulacje Touch](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Inne niż affine — przekształcenia](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [Obrotu 3W](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Krzywe i ścieżek](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Trzy sposoby narysowania łuku](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Trzy typy krzywych Béziera](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [Dane ścieżki SVG](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Obcinanie przy użyciu ścieżek i regionów](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Efekty ścieżek](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Ścieżki i tekst](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Wyliczanie i informacje o ścieżce](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[Uwagi dotyczące określonej platformy](~/graphics-games/skiasharp/platform.md)

Na tej stronie opisano instrukcje dotyczące SkiaSharp instalacji na różnych platformach z systemami iOS, Android, macOS i systemu Windows.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[Dokumentacja interfejsu API](https://developer.xamarin.com/api/namespace/SkiaSharp/)

Możesz przeglądać [dokumentacji interfejsu API](https://developer.xamarin.com/api/namespace/SkiaSharp/) dla SkiaSharp w naszej witrynie sieci web.

## <a name="work-in-progress"></a>Pracy w toku

SkiaSharp jest pracy w toku możemy korzystają z naszej społeczności. Gdy firma Microsoft powiązania ważne części Skia API dużo pracy pozostaje do zrobienia. Używamy stabilna C API udostępniane przez Skia i naszego planu jest kontynuowanie Współtworzenie naszej współpracują w celu powiązania C Skia, aby zapewnić pełne pokrycie interfejsów API.

Aby pomóc nam przewodnik dążenie powiązanie, pozostaw komentarze i propozycje jako problemów w repozytorium GitHub [ http://github.com/mono/SkiaSharp ](http://github.com/mono/SkiaSharp).
