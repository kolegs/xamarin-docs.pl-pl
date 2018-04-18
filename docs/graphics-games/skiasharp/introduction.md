---
title: Wprowadzenie do SkiaSharp
description: Zapewnia to krótkie wprowadzenie do pojęć dotyczących SkiaSharp
ms.prod: xamarin
ms.assetid: 19506F08-2603-465E-A806-6BD01638DE90
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: b16792a506b131be07c52275e3f40cbb8d5fca94
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="an-introduction-to-skiasharp"></a>Wprowadzenie do SkiaSharp

_Zapewnia to krótkie wprowadzenie do pojęć dotyczących SkiaSharp_

SkiaSharp zapewnia bogaty i wydajne grafiki 2D interfejs API, który może zostać użyty do renderowania do buforów 2D.  Można je implementować niestandardowe elementy interfejsu użytkownika i 2D grafiki, które mogą być włączone w aplikacji.  Powiązanie .NET jest SkiaSharp [Skia](https://skia.org) biblioteki i ma dziedziczyć funkcje i możliwości tej biblioteki.

Biblioteka jest obecnie dostępna jako między platformami [pakietu NuGet](https://www.nuget.org/packages/SkiaSharp), można dodać do projektu przez dodanie odwołania do NuGet.

Rysowanie, utworzy kodu `SkCanvas` która opisuje powierzchni, gdzie operacje rysowania będą miały miejsce.

## <a name="obtaining-an-skcanvas"></a>Uzyskiwanie SKCanvas

```csharp
using (var surface = SKSurface.Create (width: 640, height: 480, SKImageInfo.PlatformColorType, SKAlphaType.Premul)) {
    SKCanvas myCanvas = surface.Canvas;

    // Your drawing code goes here.
}
```

## <a name="drawing-on-skcanvas"></a>Rysowanie w SKCanvas

`SKCanvas` Używa modelu rysowania podobny do innych rysunku duchu modeli należy się zapoznać się z, używa kolorów z kanałem przezroczystość opcjonalny i może wykonywać Rysowanie linii, łuki, tekst i obrazy.

Poniżej przedstawiono kilka z wielu różnych rzeczy, które można wykonać za pomocą SkiaSharp.  W przykładzie poniżej zmienna `canvas` jest typu SKCanvas.

### <a name="drawing-xamagon"></a>Rysowanie Xamagon

W tym przykładzie rysuje logo firmy Xamarin Xamagon:

```csharp
// clear the canvas / fill with white
canvas.Clear (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x2c, 0x3e, 0x50);
  paint.StrokeCap = SKStrokeCap.Round;

  // create the Xamagon path
  using (var path = new SKPath ()) {
    path.MoveTo (71.4311121f, 56f);
    path.CubicTo (68.6763107f, 56.0058575f, 65.9796704f, 57.5737917f, 64.5928855f, 59.965729f);
    path.LineTo (43.0238921f, 97.5342563f);
    path.CubicTo (41.6587026f, 99.9325978f, 41.6587026f, 103.067402f, 43.0238921f, 105.465744f);
    path.LineTo (64.5928855f, 143.034271f);
    path.CubicTo (65.9798162f, 145.426228f, 68.6763107f, 146.994582f, 71.4311121f, 147f);
    path.LineTo (114.568946f, 147f);
    path.CubicTo (117.323748f, 146.994143f, 120.020241f, 145.426228f, 121.407172f, 143.034271f);
    path.LineTo (142.976161f, 105.465744f);
    path.CubicTo (144.34135f, 103.067402f, 144.341209f, 99.9325978f, 142.976161f, 97.5342563f);
    path.LineTo (121.407172f, 59.965729f);
    path.CubicTo (120.020241f, 57.5737917f, 117.323748f, 56.0054182f, 114.568946f, 56f);
    path.LineTo (71.4311121f, 56f);
    path.Close ();

    // draw the Xamagon path
    canvas.DrawPath (path, paint);
  }
}
```

### <a name="drawing-text"></a>Rysowanie tekstu

```csharp
// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.TextSize = 64.0f;
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x42, 0x81, 0xA4);
  paint.IsStroke = false;

  // draw the text
  canvas.DrawText ("Skia", 0.0f, 64.0f, paint);
}
```

### <a name="drawing-bitmaps"></a>Rysowanie mapy bitowe

```csharp
Stream fileStream = File.OpenRead ("MyImage.png");

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  canvas.DrawBitmap(bitmap, SKRect.Create(Width, Height), paint);
}
```

### <a name="drawing-with-image-filters"></a>Rysowanie za pomocą filtry obrazów

```csharp
Stream fileStream = File.OpenRead ("MyImage.png"); // open a stream to an image file

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  // create the image filter
  using (var filter = SKImageFilter.CreateBlur(5, 5)) {
    paint.ImageFilter = filter;

    // draw the bitmap through the filter
    canvas.DrawBitmap(bitmap, SKRect.Create(width, height), paint);
  }
}
```

## <a name="more-information"></a>Więcej informacji

Więcej informacji o używaniu SkiaSharp można znaleźć w [dokumentacji interfejsu API w trybie online](https://developer.xamarin.com/api/namespace/SkiaSharp/)


## <a name="related-links"></a>Linki pokrewne

- [IOS SkiaSharp skoroszytu](https://developer.xamarin.com/workbooks/graphics/skiasharp/logo/skialogo-ios.workbook)
