---
title: Pikseli i jednostki niezależnych od urządzenia
description: Eksploruj różnice między współrzędne SkiaSharp i współrzędne platformy Xamarin.Forms
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 95f782fd4670782217d8ce4bc055341747a71170
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="pixels-and-device-independent-units"></a>Pikseli i jednostki niezależnych od urządzenia

_Eksploruj różnice między współrzędne SkiaSharp i współrzędne platformy Xamarin.Forms_

Ten artykuł opisuje różnice w układzie współrzędnych używane w SkiaSharp i platformy Xamarin.Forms. Możesz uzyskać informacje na temat konwersji między tymi dwoma systemami współrzędnych i przyciągać grafiki, który wypełnił określonego obszaru:

![](pixels-images/screenfillexample.png "Oval, które wypełnia ekranu")

Jeśli możesz już został Programowanie w platformy Xamarin.Forms przez pewien czas, może być działanie dla współrzędnych platformy Xamarin.Forms i rozmiary. Okręgi rysowana w dwóch poprzednich artykułach może wydawać się nieco mały.

Te okręgi *są* małe w porównaniu z rozmiary platformy Xamarin.Forms. Domyślnie SkiaSharp pobiera w jednostkach pikseli a platformy Xamarin.Forms podstawowych współrzędnych i rozmiary w jednostce niezależnych od urządzenia, zgodnie z podstawowej platformy. (Więcej informacji na temat platformy Xamarin.Forms współrzędnych znajdują się w [rozdział 5. Zajmujących się rozmiary](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) książki *tworzenia aplikacji mobilnych za pomocą platformy Xamarin.Forms*.)

Strona w [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program prawo **rozmiar powierzchni** używa SkiaSharp tekst wyjściowy rozmiar powierzchni ekranu z trzech różnych źródeł:

- Normalne platformy Xamarin.Forms [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) i [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) właściwości `SKCanvasView` obiektu.
- [ `CanvasSize` ](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKCanvasView.CanvasSize/) Właściwość `SKCanvasView` obiektu.
- [ `Size` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Size/) Właściwość `SKImageInfo` wartość, która jest zgodna z `Width` i `Height` właściwości używane w dwóch poprzednich stron.

[ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) Klasa przedstawia sposób wyświetlania tych wartości. Zapisuje konstruktora `SKCanvasView` obiektu jako pole, aby był on dostępny w `PaintSurface` obsługi zdarzeń:

```csharp
SKCanvasView canvasView;

public SurfaceSizePage()
{
    Title = "Surface Size";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvas` obejmuje sześć różnych `DrawText` metody, ale [ `DrawText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawText/p/System.String/System.Single/System.Single/SkiaSharp.SKPaint/) jest najprostszą metodę:

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Określ ciąg tekstowy, współrzędne X i Y którym ma rozpocząć, tekst i `SKPaint` obiektu. Określa współrzędną X gdzie znajduje się lewej tekstu, ale czujki limit: Współrzędna Y określa położenie *linii bazowej* tekstu. Jeśli kiedykolwiek napisanych ręcznie na linie papieru, linia bazowa jest wierszu na którym hostowana znaków i poniżej malejąca które wydłużeń dolnych (takich jak litery g, p, q i y).

`SKPaint` Obiektu można określić kolor tekstu, rodziny czcionek i rozmiar tekstu. Domyślnie [ `TextSize` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.TextSize/) właściwość ma wartość 12, co powoduje bardzo małego tekstu na urządzeniach o wysokiej rozdzielczości, takich jak telefony. W opcjami najprostszym aplikacji, należy również niektóre informacje na rozmiar tekstu, który jest aktualnie używany. `SKPaint` Klasa definiuje [ `FontMetrics` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontMetrics/) właściwości i kilka [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) metody, ale mniej ozdobne potrzeby [ `FontSpacing` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontSpacing/) Właściwość określa odstępy kolejnych wierszy tekstu zalecaną wartość.

Następujące `PaintSurface` tworzy program obsługi `SKPaint` obiekt do `TextSize` 40 pikseli, co stanowi żądaną wysokość pionowego tekstu od góry wydłużenia dolnego do dołu wydłużeń dolnych. `FontSpacing` Wartość `SKPaint` zwraca obiekt jest nieco większy niż który o 47 pikseli.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 40
    };

    float fontSpacing = paint.FontSpacing;
    float x = 20;               // left margin
    float y = fontSpacing;      // first baseline
    float indent = 100;

    canvas.DrawText("SKCanvasView Height and Width:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(String.Format("{0:F2} x {1:F2}",
                                  canvasView.Width, canvasView.Height),
                    x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKCanvasView CanvasSize:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(canvasView.CanvasSize.ToString(), x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKImageInfo Size:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(info.Size.ToString(), x + indent, y, paint);
}
```

Metoda rozpoczyna się pierwszego wiersza tekstu z współrzędną X 20 (dla małego marginesu w lewym) i współrzędną Y `fontSpacing`, czyli więcej niż jest to konieczne wyświetlić pełną wysokość pierwszy wiersz tekstu w górnej części powierzchni ekranu. Po każdym wywołaniu `DrawText`, współrzędną Y jest podwyższana o jeden lub dwa `fontSpacing`.

Oto programu uruchomionego na wszystkich platformach trzy:

[![](pixels-images/surfacesize-small.png "Potrójna zrzut ekranu przedstawiający stronę rozmiar powierzchni")](pixels-images/surfacesize-large.png#lightbox "Potrójna zrzut ekranu przedstawiający powierzchni rozmiaru strony")

Jak widać, `CanvasSize` właściwość `SKCanvasView` i `Size` właściwość `SKImageInfo` wartości są spójne w raportowaniu wymiarów w pikselach. `Height` i `Width` właściwości `SKCanvasView` właściwości platformy Xamarin.Forms i zgłoś rozmiar widoku w jednostkach niezależnych od urządzenia, zdefiniowane przez platformę.

2 pikseli na jednostkę niezależnych od urządzenia ma symulatora systemu iOS 7 po lewej stronie, a Android 5 węzła w Centrum ma 3 pikseli na jednostkę. Dlatego Proste kółko przedstawiona wcześniej ma różne rozmiary na różnych platformach.

Jeśli chcesz użyć do pracy w całości jednostki niezależnych od urządzenia, możesz to zrobić przez ustawienie `IgnorePixelScaling` właściwość `SKCanvasView` do `true`. Jednak nie może Cię zainteresować wyniki. SkiaSharp Renderowanie grafiki na powierzchni mniejsze urządzenia, o rozmiarze pikseli równe rozmiarowi widoku jednostki niezależnych od urządzenia. (Na przykład SkiaSharp użyje powierzchni wyświetlania 360 x 512 pikseli na 5 węzła.) Go następnie skalowanie obrazu w rozmiarze, co w jaggies zauważalne mapy bitowej.

Aby zachować tę samą rozdzielczość obrazu, lepszym rozwiązaniem jest napisanie prostych funkcji do konwersji między tymi dwoma systemami współrzędną.

Oprócz `DrawCircle` metody `SKCanvas` również definiuje dwie `DrawOval` metody elipsy. Elipsy jest zdefiniowana przez dwa promień zamiast pojedynczego serwera radius. Są one nazywane *głównych radius* i *pomocnicza radius*. `DrawOval` Metody Rysuje elipsę z dwóch promień zbliżony do osi X i Y. Tego ograniczenia można rozwiązać transformacji lub użyj ścieżki grafiki (mają być objęte później), ale [to `DrawOval` — metoda](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) nazwy argumentu dwóch promień `rx` i `ry` wskazuje, że są zbliżony do Osie X i Y:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Czy jest możliwe do rysowania elipsy, które wypełnia obszar wyświetlania? **Wypełnienia elipsy** strony demonstruje sposób. `PaintSurface` Obsługi zdarzeń w [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) klasy odejmuje pełnej szerokości obrysu z `xRadius` i `yRadius` wartości, aby pomieścić cały elipsy i jego konspekt w powierzchnię wyświetlania:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float strokeWidth = 50;
    float xRadius = (info.Width - strokeWidth) / 2;
    float yRadius = (info.Height - strokeWidth) / 2;

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = strokeWidth
    };
    canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
}
```

W tym miejscu jest uruchomiona na trzy platformach:

[![](pixels-images/ellipsefill-small.png "Potrójna zrzut ekranu przedstawiający stronę rozmiar powierzchni")](pixels-images/ellipsefill-large.png#lightbox "Potrójna zrzut ekranu przedstawiający powierzchni rozmiaru strony")

[Innych `DrawOval` metody](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/) ma [ `SGRect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRect/) argumentu, który jest zdefiniowany w postaci współrzędne X i Y lewego górnego rogu i prawym dolnym rogu prostokąta. Oval wypełnia tego prostokąta, które sugeruje, że może być możliwe do użycia w **wypełnienia elipsy** strony w następujący sposób:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Jednak który obcina krawędzi konturu Wielokropek na czterech stron. Należy skorygować wszystkie `SKRect` na podstawie argumentów konstruktora `strokeWidth` aby działało prawa:

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
