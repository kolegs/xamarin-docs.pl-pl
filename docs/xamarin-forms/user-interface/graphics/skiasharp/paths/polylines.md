---
title: Linie łamane i równania parametryczne
description: W tym artykule wyjaśniono, jak do SkiaSharp użycia do renderowania wiersza można zdefiniować przy użyciu równania parametryczne i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 9118ca8e23e4c4a9023a1add89e26c4484979c8f
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615798"
---
# <a name="polylines-and-parametric-equations"></a>Linie łamane i równania parametryczne

_Umożliwia renderowanie każdego wiersza, które można zdefiniować przy użyciu równania parametryczne SkiaSharp_

W dalszej części tego przewodnika, zobaczysz różnych metod, `SKPath` definiuje do renderowania niektórych rodzajów krzywych. Jednak czasami jest konieczne zwrócenie typu krzywej, które nie są bezpośrednio obsługiwane przez `SKPath`. W takim przypadku można użyć łamanej (zbieranie połączone linie) do rysowania wszelkie krzywa ze sobą matematycznie można zdefiniować. Jeśli wprowadzisz wiersze wystarczająco mała, i liczne wystarczająco dużo, wynik będzie wyglądał jak krzywej. Ta spirali jest faktycznie 3600 nieco wiersze:

![](polylines-images/spiralexample.png "Spirali")

Zazwyczaj najlepiej jest określenie krzywej pod względem parę równania parametryczne. Oto równania X i Y współrzędne, które są zależne od trzeciego zmiennej, czasami nazywane `t` po raz. Na przykład, następujące równania parametryczne zdefiniować koła o promieniu 1, a ich tematyka w momencie (0, 0) dla *t* z zakresu od 0 do 1:

 x = cos(2πt) y = sin(2πt)

 Chcąc promień większy niż 1, możesz po prostu mnożenia wartości funkcji sinus i cosinus z tej usługi radius i jeśli musisz przenieść Centrum do innej lokalizacji, należy dodać te wartości:

 x = xCenter + radius·cos(2πt) y = yCenter + radius·sin(2πt)

Dla elipsę z równoległych osie na poziome i pionowe obejmuje dwa promienie:

x = xCenter + xRadius·cos(2πt) y = yCenter + yRadius·sin(2πt)

Następnie można umieścić równoważny kod SkiaSharp w pętli, wykonujące różne punkty i dodaje te ścieżki. Poniższy kod SkiaSharp tworzy `SKPath` obiekt elipsy, który wypełnia wyświetlanej powierzchni. Pętla cykli bezpośrednio do 360 stopni. Centrum jest połowę szerokości i wysokości wyświetlanej powierzchni i dlatego są dwóch promienie:

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

Skutkuje to elipsę definicją 360 nieco wierszy. Gdy jest on renderowany wygląda na to bezproblemowe.

Oczywiście nie trzeba tworzyć elipsę za pomocą łamaną, ponieważ `SKPath` obejmuje `AddOval` metodę, która zrobi to za Ciebie. Można jednak narysować obiekt wizualny, który nie został dostarczony przez `SKPath`.

**Spirali Archimedean** strona zawiera kod, że podobne do kodu elipsy, ale z istotną różnicą. Go w pętli wokół 360 stopni okręgu 10 razy stale Dostosowywanie promień:

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

Należy zauważyć, że `SKPath` jest tworzony w `using` bloku. To `SKPath` zużywa więcej pamięci niż `SKPath` obiektów w poprzednim programy, które sugeruje, które `using` blok jest bardziej odpowiednie do usuwania niezarządzanych zasobów.


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
