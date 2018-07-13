---
title: Piksele i jednostki niezależne od urządzenia
description: W tym artykule przedstawiono różnice między współrzędne SkiaSharp oraz współrzędne platformy Xamarin.Forms i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: b0655934b0389b06dca5ef6bab3badc0023400b4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997388"
---
# <a name="pixels-and-device-independent-units"></a>Piksele i jednostki niezależne od urządzenia

_Zapoznaj się z różnicami między współrzędne SkiaSharp oraz współrzędne zestawu narzędzi Xamarin.Forms_

W tym artykule przeanalizowano różnice w układzie współrzędnych używane w SkiaSharp i zestawu narzędzi Xamarin.Forms. Możesz uzyskać informacje na temat konwersji między dwoma układami współrzędnych i narysuj także grafiki, która wypełnienia określonym obszarze:

![](pixels-images/screenfillexample.png "Elipsa, który wypełnia ekranu")

Jeśli możesz już lat. zaczynał w interfejsie Xamarin.Forms od pewnego czasu, może być działanie współrzędne platformy Xamarin.Forms i rozmiarów. Okręgów rysowane w poprzednich dwóch artykułach mogą wydawać się nieco małych do Ciebie.

Tych kręgów *są* niewielki w porównaniu rozmiary zestawu narzędzi Xamarin.Forms. Domyślnie SkiaSharp pobiera w jednostkach pikseli a Xamarin.Forms określa współrzędne i rozmiary na jednostki niezależne od urządzenia, nawiązane z odpowiedniej platformy. (Więcej informacji na temat zestawu narzędzi Xamarin.Forms współrzędnych znajdują się w [rozdział 5. Obsługa rozmiarów](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) książki *tworzenia aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms*.)

Na stronie [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program uprawnionych **rozmiar powierzchni** używa SkiaSharp tekst wyjściowy rozmiar powierzchni ekranu z trzech różnych źródeł:

- Normalne Xamarin.Forms [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) i [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) właściwości `SKCanvasView` obiektu.
- [ `CanvasSize` ](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKCanvasView.CanvasSize/) Właściwość `SKCanvasView` obiektu.
- [ `Size` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Size/) Właściwość `SKImageInfo` wartość, która jest zgodna z `Width` i `Height` właściwości używane w poprzednich dwóch stron.

[ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) Klasy pokazuje, jak wyświetlać te wartości. Zapisuje Konstruktor `SKCanvasView` obiektu jako pole, aby był on dostępny w `PaintSurface` programu obsługi zdarzeń:

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

`SKCanvas` obejmuje sześć różnych `DrawText` metody, ale [ `DrawText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawText/p/System.String/System.Single/System.Single/SkiaSharp.SKPaint/) jest najprostsza metoda:

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Określ ciąg tekstowy, współrzędne X i Y gdzie tekst jest, aby rozpocząć i `SKPaint` obiektu. Współrzędna X określa, gdzie jest umieszczony po lewej stronie tekstu, ale Obejrzyj out: Współrzędna Y określa położenie *linii bazowej* tekstu. Jeśli kiedykolwiek napisanych ręcznie na papierze linie, linia bazowa jest linii, na które sit znaków i poniżej jest elementem podrzędnym elementu które wydłużeń (takich jak te na litery k, p, pytania i y).

`SKPaint` Obiektu można określić kolor tekstu, rodzinę czcionek i rozmiar tekstu. Domyślnie [ `TextSize` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.TextSize/) właściwość ma wartość 12, co skutkuje bardzo małego tekstu na urządzeniach o wysokiej rozdzielczości, takich jak telefony. W tylko najprostszej aplikacji, należy także pewne informacje o rozmiar tekstu, który jest aktualnie używany. `SKPaint` Klasa definiuje [ `FontMetrics` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontMetrics/) właściwości i kilka [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) metody, ale mniejszej ozdobnego potrzeby [ `FontSpacing` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontSpacing/) Właściwość zawiera zalecaną wartością dla kolejnych wierszy tekstu odstępy.

Następujące `PaintSurface` tworzy program obsługi `SKPaint` dla obiektu `TextSize` 40 pikseli, czyli żądaną wysokość pionie tekstu w górnej części wydłużenia górne do dołu wydłużeń. `FontSpacing` Wartość, która `SKPaint` zwraca jest nieco większy niż która o 47 pikseli.

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

Metoda rozpoczyna się pierwszego wiersza tekstu współrzędną X 20 (w przypadku nieco marginesie po lewej stronie), a Współrzędna Y `fontSpacing`, która jest nieco więcej, niż jest to konieczne wyświetlić pełną wysokość pierwszy wiersz tekstu w górnej części powierzchni ekranu. Po każdym wywołaniu `DrawText`, Współrzędna Y jest podwyższana o jeden lub dwa `fontSpacing`.

W tym miejscu jest uruchomiony na wszystkich trzech platformach program:

[![](pixels-images/surfacesize-small.png "Potrójna zrzut ekranu przedstawiający stronę rozmiar powierzchni")](pixels-images/surfacesize-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę rozmiar powierzchni")

Jak widać, `CanvasSize` właściwość `SKCanvasView` i `Size` właściwość `SKImageInfo` wartości są spójne w raportowaniu wymiarów w pikselach. `Height` i `Width` właściwości `SKCanvasView` właściwości platformy Xamarin.Forms i zgłosić rozmiar widoku w jednostkach niezależnych od urządzenia, zdefiniowane przez platformę.

Symulator systemu iOS 7 po lewej stronie zawiera 2 piksele na jednostkę niezależnych od urządzenia, a Android 5 Nexus w Centrum zawiera 3 pikseli na jednostkę. Dlatego Proste kółko przedstawionej wcześniej ma różne rozmiary na różnych platformach.

Jeśli wolisz pracować w całości w jednostkach niezależnych od urządzenia, możesz to zrobić, ustawiając `IgnorePixelScaling` właściwość `SKCanvasView` do `true`. Jednak nie może Cię zainteresować wyniki. Skiasharp — Renderowanie grafiki na mniejsze powierzchni urządzeniu o rozmiarze pikseli równy rozmiarowi widoku w jednostkach niezależnych od urządzenia. (Na przykład SkiaSharp użyje wyświetlanej powierzchni 360 x 512 pikseli na Nexus 5.) Go następnie powiększenie obrazu w rozmiarze, co w jaggies zauważalne mapy bitowej.

Aby zachować tę samą rozdzielczość obrazu, lepszym rozwiązaniem jest napisanie proste funkcje do konwersji między dwoma układami współrzędnych.

Oprócz `DrawCircle` metody `SKCanvas` definiuje również dwie `DrawOval` metody, które narysować elipsę. Elipsy jest definiowany przez dwa promieni, a nie pojedynczego serwera radius. Są to znane jako *głównych radius* i *pomocnicza radius*. `DrawOval` Metoda Rysuje elipsę o dwóch promienie równoległej na osi X i Y. Tego ograniczenia można rozwiązać za pomocą przekształcenia lub użyj ścieżki grafiki (w celu podawany później), ale [to `DrawOval` metoda](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) nazwy argumentu promienie dwóch `rx` i `ry` do wskazania, że są one zbliżony do Osie X i Y:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Czy jest możliwe narysować elipsę, który wypełnia powierzchni ekranu? **Wypełnienia elipsy** strony pokazuje sposób. `PaintSurface` Programu obsługi zdarzeń w [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) klasy odejmuje połowę szerokości obrysu z `xRadius` i `yRadius` wartości, aby dopasować całego elipsy i jego przedstawiają, w ramach powierzchni ekranu:

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

W tym miejscu jest uruchomiona na trzech platformach:

[![](pixels-images/ellipsefill-small.png "Potrójna zrzut ekranu przedstawiający stronę rozmiar powierzchni")](pixels-images/ellipsefill-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę rozmiar powierzchni")

[Innych `DrawOval` metoda](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/) ma [ `SGRect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRect/) argumentu, który jest zdefiniowany w zakresie współrzędne X i Y lewego górnego rogu i prawym dolnym rogu prostokąta. Elipsa wypełnia tego prostokąta, co sugeruje, że może być możliwe, stosowanie ich w **wypełnienia elipsy** strony w następujący sposób:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Jednakże, obcina krawędzi konspektu elipsy na czterech bokach. Wymagane jest dostosowanie wszystkich `SKRect` na podstawie argumentów konstruktora `strokeWidth` aby umożliwić odpowiednie:

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
