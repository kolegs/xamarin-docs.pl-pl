---
title: "Rysowanie okręgu prosty"
description: Poznaj podstawy rysunku SkiaSharp, w tym kanw i malowania
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 10f741e853603ef22cd45004a6c726ae579f3675
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="drawing-a-simple-circle"></a>Rysowanie okręgu prosty

_Poznaj podstawy rysunku SkiaSharp, w tym kanw i malowania_

W tym artykule przedstawiono koncepcję rysowanie grafiki w platformy Xamarin.Forms przy użyciu SkiaSharp, w tym tworzenie `SKCanvasView` obiektu do hostowania obsługę grafiki `PaintSurface` zdarzeń i przy użyciu `SKPaint` obiektu w celu określenia, kolor i inne rysunki atrybuty.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) program zawiera wszystkie przykładowy kod dla tej serii SkiaSharp artykułów. Pierwsza strona jest uprawniony **prosty koło** i wywołuje klasy strony [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs). Ten kod przedstawia sposób narysować okrąg na środku strony z protokołem radius 100 pikseli. Konspekt okręgu jest czerwony, a wewnątrz okręgu jest niebieski.

![](circle-images/circleexample.png "Niebieskie koło opisane na czerwono")

[ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) Pochodną klasy strony `ContentPage` i zawiera dwa `using` dyrektywy dla przestrzeni nazw SkiaSharp:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Tworzy następujący Konstruktor klasy [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/) obiektów, dołącza program obsługi [ `PaintSurface` ](https://developer.xamarin.com/api/event/SkiaSharp.Views.Forms.SKCanvasView.PaintSurface/) zdarzenia i ustawia `SKCanvasView` obiektu jako zawartości strony:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvasView` Zajmuje cały obszar zawartości strony. Alternatywnie można łączyć `SKCanvasView` z innych platformy Xamarin.Forms `View` pochodne, jak wyświetlone w innych przykładach.

`PaintSurface` Procedura obsługi zdarzeń jest, którym jest wykonywana na rysunku. Ta metoda jest zwykle wywołana wiele razy podczas działania programu, aby go powinien zachować wszystkie informacje niezbędne w celu ponownego utworzenia grafiki wyświetlania:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[ `SKPaintSurfaceEventArgs` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs/) Obiekt, który towarzyszy zdarzenie ma dwie właściwości:

- [`Info`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info/) typu [`SKImageInfo`](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/)
- [`Surface`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface/) typu [`SKSurface`](https://developer.xamarin.com/api/type/SkiaSharp.SKSurface/)

`SKImageInfo` Struktura zawiera najważniejsze informacje o powierzchni rysowania, szerokość i wysokość w pikselach. `SKSurface` Obiekt reprezentuje powierzchni do rysowania, samej siebie. W tym programie powierzchni do rysowania jest urządzenia wyświetlającego, ale w innych programów `SKSurface` obiektu może reprezentować używanej SkiaSharp rysowanie na mapę bitową.

Najważniejsze właściwości `SKSurface` jest [ `Canvas` ](https://developer.xamarin.com/api/property/SkiaSharp.SKSurface.Canvas/) typu [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/). Ta klasa jest grafiki rysowania kontekstu, używanej do wykonywania rzeczywiste rysunku. `SKCanvas` Hermetyzuje stan grafiki, w tym transformacje grafiki i wycinka.

Poniżej przedstawiono typowe początek `PaintSurface` obsługi zdarzeń:

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

[ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) Metody czyści obszaru roboczego z kolor przezroczysty. Przeciążenia pozwala określić kolor tła dla obszaru roboczego.

Celem jest do rysowania czerwone kółko wypełnione niebieski. Ponieważ tego konkretnego obrazka zawiera dwa różne kolory, zadanie musi odbywać się w dwóch krokach. Pierwszym krokiem jest rysowanie konturu okręgu. Aby określić kolor i inne cechy linii, Utwórz i zainicjuj [ `SKPaint` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaint/) obiektu:

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

[ `Style` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Style/) Właściwość wskazuje, że chcesz *stroke* wiersza (w tym przypadku konturu okręgu) zamiast *wypełnienia* wewnętrznych. Trzech elementów członkowskich [ `SKPaintStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaintStyle/) wyliczenia są następujące:

- [`Fill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Fill/)
- [`Stroke`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Stroke/)
- [`StrokeAndFill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.StrokeAndFill/)

Wartość domyślna to `Fill`. Trzecia opcja umożliwia obrysu wiersza i wypełnienie wewnątrz tego samego koloru.

Ustaw [ `Color` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Color/) właściwości na wartość typu [ `SKColor` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColor/). Aby uzyskać `SKColor` wartość jest za pomocą konwersji platformy Xamarin.Forms `Color` do wartości `SKColor` wartości przy użyciu metody rozszerzenia [ `ToSKColor` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.Extensions.ToSKColor/p/Xamarin.Forms.Color/). [ `Extensions` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.Extensions/) Klasy w `SkiaSharp.Views.Forms` przestrzeń nazw obejmuje innych metod, które konwertowanie platformy Xamarin.Forms oraz SkiaSharp wartości.

[ `StrokeWidth` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeWidth/) Właściwość wskazuje grubość linii. W tym miejscu jest ustawiona na 25 pikseli.

Możesz użyć tego `SKPaint` obiektu do rysowania okręgu:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Współrzędne są określane względem lewego górnego rogu powierzchni ekranu. Współrzędne X, Zwiększ po prawej stronie i zwiększyć współrzędne Y przechodzi w dół. W Omówienie grafiki często matematyczne notacji (x, y) służy do określenia punktu. Punkt (0, 0) jest lewego górnego rogu powierzchni wyświetlania i jest często nazywana *pochodzenia*.

Pierwsze dwa argumenty `DrawCircle` wskazują współrzędne X i Y środka okręgu. Te są przypisywane połowa szerokości i wysokości powierzchni wyświetlania mają zostać umieszczone w Centrum powierzchni wyświetlania środka okręgu. Trzeci argument określa promień koła i jest ostatni argument `SKPaint` obiektu.

Aby wypełnić wewnątrz okręgu, można zmienić w dwóch właściwości `SKPaint` obiekt i wywołanie `DrawCircle` ponownie. Ten kod zawiera również alternatywna metoda uzyskania `SKColor` wartość z jednego z wielu pól z [ `SKColors` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColors/) struktury:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
Teraz, `DrawCircle` wywołania wypełnia okręgu przy użyciu nowych właściwości `SKPaint` obiektu.

Oto programu uruchomionego w systemach iOS, Android i platformy uniwersalnej systemu Windows:

[![](circle-images/simplecircle-small.png "Potrójna zrzut ekranu przedstawiający stronę koło prosty")](circle-images/simplecircle-large.png#lightbox "Potrójna zrzut ekranu strony prosty okręgu")

Podczas uruchamiania programu samodzielnie, możesz włączyć telefonu lub symulator bok, aby zobaczyć, jak jest odświeżana grafiki. Zawsze musi być narysowany ponownie grafiki `PaintSurface` zostanie ponownie wywołany program obsługi zdarzeń.

`SKPaint` Obiekt jest więcej niż zbiór właściwości rysowania grafiki. Te obiekty są bardzo uproszczonego. Można użyć ponownie `SKPaint` obiektów, ponieważ nie tego programu, lub można utworzyć wiele `SKPaint` obiektów dla różnych kombinacji Rysowanie właściwości. Możesz utworzyć i zainicjować tych obiektów poza `PaintSurface` obsługi zdarzeń, a można ją zapisać jako pola w klasie strony.

Mimo że szerokość konturu koło jest określony jako 25 pikseli &mdash; lub co kwartał promień okręgu &mdash; wydaje się być grubsza i powody, dla którego: pełnej szerokości linii jest zasłonięty przez niebieski. Argumenty `DrawCircle` abstrakcyjny geometrycznych współrzędne koło zdefiniuj — metoda. Niebieski wewnętrznych jest dopasowywany do tego wymiaru do najbliższej pikseli, ale konspektu 25 pikseli na poziomie pokrywającej geometrycznych okręgu &mdash; połowy na wewnątrz, a druga połowa na zewnątrz.

Następna próbka w [integracji z platformy Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) artykule przedstawiono to wizualne.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
