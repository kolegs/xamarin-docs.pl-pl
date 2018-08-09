---
title: Rysowanie prostego okręgu w SkiaSharp
description: W tym artykule objaśnia podstawy rysowania SkiaSharp, łącznie z kanwami i paint, w aplikacjach Xamarin.Forms i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: e06a1310fad01da11c8d8b115df504cc19426344
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615226"
---
# <a name="drawing-a-simple-circle-in-skiasharp"></a>Rysowanie prostego okręgu w SkiaSharp

_Poznaj podstawy rysowania SkiaSharp, łącznie z kanwami i malowania_

W tym artykule przedstawiono koncepcję rysowania grafiki w interfejsie Xamarin.Forms korzystanie z biblioteki SkiaSharp, takich jak tworzenie `SKCanvasView` obiektu do hostowania obsługę grafiki `PaintSurface` zdarzeń i przy użyciu `SKPaint` obiektu w celu określenia koloru i innych rysowania atrybuty.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program zawiera wszystkie przykładowy kod dla tej serii artykułów SkiaSharp. Pierwsza strona jest upoważniona **prostego okręgu** i wywołuje klasy strony [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs). Ten kod przedstawia sposób narysuj okręg w środku strony z protokołem radius, 100 pikseli. Zarys koła ma kolor czerwony, a wewnątrz okręgu ma kolor niebieski.

![](circle-images/circleexample.png "Jasnoniebieski okrąg wyróżnione kolorem czerwonym")

[ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) Pochodną klasy strony `ContentPage` i zawiera dwa `using` dyrektywy dla przestrzeni nazw SkiaSharp:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Tworzy następujący Konstruktor klasy [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/) obiektów, dołącza obsługę do [ `PaintSurface` ](https://developer.xamarin.com/api/event/SkiaSharp.Views.Forms.SKCanvasView.PaintSurface/) zdarzeń i zestawy `SKCanvasView` obiektu jako zawartość strony:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvasView` Zajmuje cały obszar zawartości strony. Można też połączyć `SKCanvasView` z innym zestawem narzędzi Xamarin.Forms `View` pochodne, jak zostaną wyświetlone w innych przykładach.

`PaintSurface` Procedura obsługi zdarzeń jest, którym jest wykonywana na rysunku. Ta metoda jest wywoływana wiele razy zazwyczaj, gdy działa program, aby je zachować wszystkie informacje niezbędne do odtworzenia grafiki wyświetlania:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[ `SKPaintSurfaceEventArgs` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs/) Obiekt, który towarzyszy zdarzenie ma dwie właściwości:

- [`Info`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info/) tego typu [`SKImageInfo`](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/)
- [`Surface`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface/) tego typu [`SKSurface`](https://developer.xamarin.com/api/type/SkiaSharp.SKSurface/)

`SKImageInfo` Struktura zawiera informacje dotyczące powierzchni do rysowania, co najważniejsze, szerokość i wysokość w pikselach. `SKSurface` Obiekt reprezentuje powierzchni do rysowania sam. W tym programie powierzchni do rysowania jest wyświetlacza wideo, ale w innych programach `SKSurface` obiektu może również reprezentować mapy bitowej, umożliwiający SkiaSharp Rysowanie w.

Do najważniejszych właściwości `SKSurface` jest [ `Canvas` ](https://developer.xamarin.com/api/property/SkiaSharp.SKSurface.Canvas/) typu [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/). Ta klasa jest kontekst, który będzie używać do wykonywania rzeczywiste rysunku do rysowania grafiki. `SKCanvas` Hermetyzuje stan grafiki, co obejmuje przekształcenia grafiki i wycinka.

Poniżej przedstawiono typowe początek `PaintSurface` program obsługi zdarzeń:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();
    ...
}

```

[ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) Metoda czyści kanwy kolorem przezroczystym. Przeciążenie pozwala określić kolor tła dla obszaru roboczego.

Celem jest, aby narysować Czerwony okrąg wypełniony kolorem niebieski. Ponieważ tego konkretnego obrazka zawiera dwa różne kolory, zadanie musi odbywać się w dwóch krokach. Pierwszym krokiem jest rysowanie konturu koła. Aby określić kolor i inne cechy linii, Utwórz i zainicjuj [ `SKPaint` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaint/) obiektu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 25
    };
    ...
}
```

[ `Style` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Style/) Właściwość wskazuje, że chcesz *obrysu* wiersza (w tym przypadku konspektu okręgu) zamiast *wypełnienia* wewnętrznych. Trzy elementy członkowskie [ `SKPaintStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaintStyle/) wyliczenia są następujące:

- [`Fill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Fill/)
- [`Stroke`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Stroke/)
- [`StrokeAndFill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.StrokeAndFill/)

Wartość domyślna to `Fill`. Trzecia opcja umożliwia obrysu wiersz i wypełnij wewnętrznych przy użyciu tego samego koloru.

Ustaw [ `Color` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Color/) właściwości na wartość typu [ `SKColor` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColor/). Jednym ze sposobów, aby uzyskać `SKColor` wartość jest konwersja zestawu narzędzi Xamarin.Forms `Color` wartość `SKColor` wartości przy użyciu metody rozszerzenia [ `ToSKColor` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.Extensions.ToSKColor/p/Xamarin.Forms.Color/). [ `Extensions` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.Extensions/) Klasy w `SkiaSharp.Views.Forms` przestrzeń nazw zawiera inne metody, które Konwertowanie zestawu narzędzi Xamarin.Forms oraz SkiaSharp wartości.

[ `StrokeWidth` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeWidth/) Właściwość wskazuje grubość linii. W tym miejscu jest ustawiona na 25 pikseli.

Możesz używać `SKPaint` obiektu rysowanie okręgów:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Współrzędne są określane względem lewego górnego rogu powierzchni ekranu. X koordynuje zwiększenia po prawej stronie i zwiększenia współrzędne Y zostanie wyłączona. W dyskusję na temat grafiki często notacji matematycznych (x, y) służy do oznaczania punktu. Punkt (0, 0) w lewym górnym rogu powierzchni ekranu a jest często określany mianem *pochodzenia*.

Pierwsze dwa argumenty `DrawCircle` wskazują współrzędne X i Y środka okręgu. Są one przypisane połowę szerokości i wysokości wyświetlanej powierzchni należy umieścić środek okręgu w środku wyświetlanej powierzchni. Trzeci argument określa promień koła, a ostatni argument jest `SKPaint` obiektu.

Aby wypełnić wewnętrznego okręgu, można zmienić dwóch właściwości `SKPaint` obiektu, a następnie wywołać `DrawCircle` ponownie. Ten kod zawiera również alternatywny sposób uzyskania `SKColor` wartości z jednej z wielu pól [ `SKColors` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColors/) strukturę:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
Tym razem `DrawCircle` wywołanie wypełnia okręgu za pomocą nowej właściwości `SKPaint` obiektu.

W tym miejscu jest uruchomiony w systemach iOS, Android i platformy uniwersalnej Windows program:

[![](circle-images/simplecircle-small.png "Potrójna zrzut ekranu przedstawiający stronę prostego okręgu")](circle-images/simplecircle-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę prostego okręgu")

Po uruchomieniu programu samodzielnie można włączyć, telefon lub symulator na bok, aby zobaczyć, jak element graficzny jest ponownie rysowany. Każdorazowo grafiki musi zostać odświeżone `PaintSurface` program obsługi zdarzeń jest ponownie wywoływana.

`SKPaint` Obiekt jest nieco więcej niż zbiór właściwości do rysowania grafiki. Te obiekty są bardzo uproszczone. Można użyć ponownie `SKPaint` obiektów, ponieważ nie tego programu, lub możesz utworzyć wiele `SKPaint` obiektów dla różnych kombinacji właściwości rysunku. Można utworzyć i zainicjować tych obiektów poza `PaintSurface` program obsługi zdarzeń i można je zapisać jako pola w klasie strony.

Mimo że szerokość konturu koła jest określony jako 25 pikseli &mdash; lub jednej czwartej promień koła &mdash; wydaje się być cieńsza i powody, dla którego: połowę szerokości linii jest zasłonięte jasnoniebieski okrąg. Argumenty `DrawCircle` metody definiowania abstrakcyjne współrzędne geometrycznych koła. Niebieski wewnętrznych jest o rozmiarze do tego wymiaru do najbliższej pikseli, ale konspektu 25 pikseli na poziomie pokrywającej geometrycznych okrąg &mdash; połowa na wewnętrznych i połowy na zewnątrz.

Następny przykład w [integracji z zestawem narzędzi Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) artykuł przedstawia to wizualnie.


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
