---
title: Linię i parametryczne równania
description: W tym artykule wyjaśniono, jak do SkiaSharp używany do renderowania wiersza można zdefiniować z parametrycznych równania i pokazuje to z przykładowym kodzie.
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 9539a21b7dbc91da63795639610886233ed705be
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245312"
---
# <a name="polylines-and-parametric-equations"></a>Linię i parametryczne równania

_SkiaSharp używany do renderowania żadnych wiersz, który można zdefiniować z parametrycznych równania_

W późniejszym części tego przewodnika, zobaczysz różnych metod który `SKPath` definiuje do renderowania niektórych typów krzywych. Jednak czasami jest to niezbędne do rysowania typu krzywej, która nie jest bezpośrednio obsługiwana przez `SKPath`. W takim przypadku łamanej (kolekcja połączone linie) służy do rysowania żadnych krzywa ze sobą matematycznie można zdefiniować. Jeśli wprowadzone wiersze duże i wiele wystarczająco wynik będzie wyglądać krzywej. To spirali jest rzeczywiście 3600 małego wiersze:

![](polylines-images/spiralexample.png "Spirali")

Zazwyczaj najlepiej zdefiniować krzywej pod względem parę równania parametrycznych. Są równania X i Y współrzędne, które są zależne od trzeciej zmiennej, nazywane również `t` raz. Na przykład zdefiniować następujące równania parametrycznych koło z protokołem radius 1 wyśrodkowany w punkcie (0, 0) dla *t* od 0 do 1:

 x = cos(2πt) y = sin(2πt)

 Jeśli chcesz promień większy niż 1, możesz po prostu mnożenia wartości sinus i cosinus z tej usługi radius i jeśli musisz przenieść Centrum do innej lokalizacji, Dodaj te wartości:

 x = xCenter + radius·cos(2πt) y = yCenter + radius·sin(2πt)

Dla elipsę z równoległe osie na poziome i pionowe obejmuje dwa promień:

x = xCenter + xRadius·cos(2πt) y = yCenter + yRadius·sin(2πt)

Następnie możesz umieścić równoważny kod SkiaSharp w pętli, który oblicza różnych punktach i dodaje je na ścieżki. Poniższy kod SkiaSharp tworzy `SKPath` obiektu dla elipsę wypełnia powierzchni ekranu. Pętla cykli bezpośrednio do 360 stopni. Centrum jest połowa szerokości i wysokości wyświetlania powierzchni, a więc są dwa promień:

```csharp
SKPath path = new SKPath();

for (float angle = 0; angle < 360; angle += 1)
{
    double radians = Math.PI * angle / 180;
    float x = info.Width / 2 + (info.Width / 2) * (float)Math.Cos(radians);
    float y = info.Height / 2 + (info.Height / 2) * (float)Math.Sin(radians);

    if (angle == 0)
    {
        path.MoveTo(x, y);
    }
    else
    {
        path.LineTo(x, y);
    }
}
path.Close();
```

Powoduje to elipsy zdefiniowane przez 360 małego wierszy. Gdy jest on renderowany wyświetlany smooth.

Oczywiście nie trzeba utworzyć elipsy przy użyciu łamaną, ponieważ `SKPath` obejmuje `AddOval` metodę, która jest dla Ciebie. Jednak należy narysować obiekt wizualny, który nie został dostarczony przez `SKPath`.

**Spirali Archimedean** strona zawiera kod, że podobne do kodu elipsy, ale z istotną różnicą. Go pętli wokół 360 stopni okręgu 10 razy, stale dostosowywania radius:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(center.X, center.Y);

    using (SKPath path = new SKPath())
    {
        for (float angle = 0; angle < 3600; angle += 1)
        {
            float scaledRadius = radius * angle / 3600;
            double radians = Math.PI * angle / 180;
            float x = center.X + scaledRadius * (float)Math.Cos(radians);
            float y = center.Y + scaledRadius * (float)Math.Sin(radians);
            SKPoint point = new SKPoint(x, y);

            if (angle == 0)
            {
                path.MoveTo(point);
            }
            else
            {
                path.LineTo(point);
            }
        }

        SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 5
        };

        canvas.DrawPath(path, paint);
    }
}
```

Wynik jest również nazywany *arytmetyczne spirali* ponieważ przesunięcie od każdej pętli jest stałe:

[![](polylines-images/archimedeanspiral-small.png "Potrójna zrzut ekranu przedstawiający stronę spirali Archimedean")](polylines-images/archimedeanspiral-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę spirali Archimedean")

Zwróć uwagę, że `SKPath` jest tworzony w `using` bloku. To `SKPath` wykorzystuje więcej pamięci niż `SKPath` obiektów w poprzednim programy, które sugeruje, które `using` blok jest bardziej odpowiednie do usunięcia żadnych niezarządzanych zasobów.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
