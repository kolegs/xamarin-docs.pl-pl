---
title: Przekształcenia macierzowe w SkiaSharp
description: W tym artykule bardziej omawia przekształcenia biblioteki SkiaSharp z macierzy transformacji wszechstronna i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: 8d5f1a08f7e1bff5ca2f9b696463bc03340476af
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615434"
---
# <a name="matrix-transforms-in-skiasharp"></a>Przekształcenia macierzowe w SkiaSharp

_Szczegółowe informacje przekształcenia biblioteki SkiaSharp z macierzy transformacji wszechstronna_

Wszystkie przekształcenia, które są stosowane do `SKCanvas` obiektu i dalszych są skonsolidowane w pojedyncze wystąpienie [ `SKMatrix` ](https://developer.xamarin.com/api/type/SkiaSharp.SKMatrix/) struktury. Jest to standardowy macierzy transformacji 3 x 3 podobne do tych we wszystkich systemach nowoczesnych grafika 2D.

Jak wiesz, można użyć przekształceń w SkiaSharp bez wiedzy przekształcenia macierzy, ale macierzy transformacji jest ważne z punktu widzenia teoretycznych i jest niezwykle istotne w przypadku używania przekształceń do modyfikowania ścieżek lub do obsługi wprowadzania dotykowego złożonych, oba które zostały przedstawione w tym artykule, a następnie.

![](matrix-images/matrixtransformexample.png "Mapy bitowej poddane affine — przekształcenia")

Stosowane do bieżącego macierzy transformacji `SKCanvas` jest dostępna w dowolnym momencie, uzyskując dostęp do tylko do odczytu [ `TotalMatrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.TotalMatrix/) właściwości. Możesz ustawić używającego macierzy transformacji [ `SetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.SetMatrix/p/SkiaSharp.SKMatrix/) metody, a można przywrócić tego macierzy transformacji do wartości domyślnych, wywołując [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix/).

Tylko inne `SKCanvas` elementu członkowskiego, który bezpośrednio współpracuje z obszaru roboczego macierzy transformacji jest [ `Concat` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Concat/p/SkiaSharp.SKMatrix@/) który łączy dwie macierzy mnożąc je ze sobą.

Domyślne macierzy transformacji macierz tożsamości i składa się z 1 w ukośne komórek i inne wszędzie, gdzie 0:

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

Możesz utworzyć macierz tożsamości przy użyciu statycznych [ `SKMatrix.MakeIdentity` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeIdentity()/) metody:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

`SKMatrix` Działa Konstruktor domyślny *nie* zwracają macierz tożsamości. Zwraca macierzy przy użyciu wszystkich komórek ustawioną wartość zero. Nie używaj `SKMatrix` konstruktora, chyba że zamierzasz ręcznie ustawić tych komórek.

Skiasharp — renderuje obiekt graficzny, każdy punkt (x, y) jest skutecznie konwertowany macierzy 1, 3, 1 w trzeciej kolumnie:

<pre>
| x  y  1 |
</pre>

Ta macierz 1, 3 reprezentuje punkt trójwymiarowej z Współrzędna ustawiona na 1. Dlaczego dwuwymiarowej macierzy transformacji wymaga Praca w trzech wymiarach jest przyczyn matematyczne (omówione w dalszej części). Można potraktować ten macierzy 1, 3 reprezentując punktu 3W układu współrzędnych, ale zawsze na płaszczyźnie 2D gdzie Z jest równa 1.

Ta macierz 1, 3 jest mnożony przez macierzy transformacji, a wynik jest punkt renderowane na kanwie:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

Przekonwertowana punkty za pomocą mnożenie macierzy standardowe, są następujące:

x' = x

y' = y

z' = 1

To jest domyślną transformację.

Gdy `Translate` metoda jest wywoływana w `SKCanvas` obiektu, `tx` i `ty` argumenty `Translate` metoda stają się pierwsze dwie komórki w trzecim wierszu macierzy transformacji:

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

Mnożenie jest teraz w następujący sposób:

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

Poniżej przedstawiono formuły przekształcenia:

x' = x + tx

y "=, y + ty

Czynniki skalowania ma wartość domyślną 1. Podczas wywoływania `Scale` metody na nowym `SKCanvas` obiektu zawiera macierzy transformacji wynikowe `sx` i `sy` argumentów w komórkach ukośne:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

Formuły przekształcenia są następujące:

x' = sx · x

y "= XI sy y

Po wywołaniu macierzy transformacji `Skew` zawiera dwa argumenty w komórkach macierzy sąsiadujące skalowania czynniki:

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

Formuły przekształcenia są:

x "= x + xSkew XI y

y "= ySkew XI x + y

Wywołania `RotateDegrees` lub `RotateRadians` kąta α macierzy transformacji jest następująca:

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

Poniżej przedstawiono formuły przekształcenia:

x "= cos(α) XI x - sin(α) XI y

y "= sin(α) XI x - cos(α) XI y

Gdy α wynosi 0 stopni, jest macierzą. Α po 180 stopni macierzy transformacji jest następująca:

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

180 stopni jest odpowiednikiem Przerzucanie obiektu w poziomie i w pionie, które również odbywa się przez ustawienie skale-1.

Tego rodzaju transformacje są klasyfikowane jako *przekształceniem afinicznym* przekształcenia. Affine — przekształcenia nigdy nie obejmują trzecia kolumna macierz, która pozostaje na wartości domyślne, 0, 0 lub 1. Artykuł [inne niż affine — przekształcenia](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md) w tym artykule omówiono przekształcenia nieafiniczne.

## <a name="matrix-multiplication"></a>Mnożenie macierzy

Jedną z zalet big Data przy użyciu macierzy transformacji jest można uzyskać przez mnożenie macierzy, który jest często określana w dokumentacji SkiaSharp jako złożone przekształcenia *łączenia*. Wiele metod związanych z transformacji w `SKCanvas` odnoszą się do "łączenia wstępnego" lub "pre-concat." Odnosi się to polecenie mnożenie, co jest ważne, ponieważ mnożenie macierzy nie jest przemiennego.

Na przykład w dokumentacji dotyczącej [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) metody mówi jej "Pre-concats bieżącego macierzy oraz w określonym tłumaczenie" podczas dokumentacji dla [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) Metoda mówi jej "Pre-concats bieżącego macierzy przy użyciu określonej skali."

Oznacza to, że przekształcania określonej przez wywołanie metody jest mnożnik (lewostronny operand), a bieżący macierzy transformacji jest którą mnożona jest mnożna (prawostronny operand).

Załóżmy, że `Translate` nosi nazwę następuje `Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

`Scale` Transformacji jest mnożony przez `Translate` przekształcenia macierzy złożone przekształcenia:

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` może zostać wywołana przed `Translate` następująco:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

W takim przypadku kolejność mnożenia została wycofana i skalowania czynniki skutecznie są stosowane do tłumaczenia czynników:

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

Oto `Scale` metody za pomocą punktu obrotu:

```csharp
canvas.Scale(sx, sy, px, py);
```

Jest to równoważne następujące wywołania translate i skalowania:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Trzy macierzy transformacji są mnożone w odwrotnej kolejności z wygląd metody w kodzie:

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

### <a name="the-skmatrix-structure"></a>Struktura SKMatrix

`SKMatrix` Struktury definiuje dziewięć właściwości odczytu/zapisu o typie `float` odpowiadający dziewięć komórkach macierzy transformacji:

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` definiuje również właściwość o nazwie [ `Values` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix.Values/) typu `float[]`. Ta właściwość może służyć do ustawiania lub uzyskać dziewięć wartości w jednym działaniu w kolejności `ScaleX`, `SkewX`, `TransX`, `SkewY`, `ScaleY`, `TransY`, `Persp0`, `Persp1`, i `Persp2`.

`Persp0`, `Persp1`, I `Persp2` komórek są omówione w artykule [inne niż affine — przekształcenia](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md). Jeśli te komórki mają wartości domyślne, 0, 0 lub 1, transformacja jest mnożony przez współrzędnych punktu następująco:

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

x "= ScaleX XI x + SkewX XI y + TransX

y "= SkewX XI x + ScaleY XI y + TransY

z' = 1

Jest to pełną dwuwymiarową affine — przekształcenia. Affine — przekształcenia zachowuje równoległych, co oznacza, że prostokąt nigdy nie jest przekształcana na coś innego niż równoległobok.

`SKMatrix` Struktury definiuje kilka metod statycznych, aby utworzyć `SKMatrix` wartości. Te wszystkie zwracany `SKMatrix` wartości:

- [`MakeTranslation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeTranslation/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/System.Single/System.Single/) za pomocą punktu obrotu
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/) kąt w radianach
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/System.Single/System.Single/) kąt w radianach z punktem obrotu
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/)
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/System.Single/System.Single/) za pomocą punktu obrotu
- [`MakeSkew`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeSkew/p/System.Single/System.Single/)

`SKMatrix` definiuje również kilka metod statycznych, które złączyć dwa macierzy, co oznacza, że należy pomnożyć je. Metody te są nazywane `Concat`, `PostConcat`, i `PreConcat`, istnieją dwie wersje każdego z nich. Te metody mają bez wartości zwracanej; Zamiast tego mogą odwoływać się do istniejącego `SKMatrix` wartości za pomocą `ref` argumentów. W poniższym przykładzie `A`, `B`, i `R` ("wynik") są wszystkie `SKMatrix` wartości.

Dwa `Concat` metody są wywoływane w następujący sposób:

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

Te należy wykonać następujące mnożenia:

R = B × A

Inne metody ma tylko dwa parametry. Pierwszy parametr jest zmodyfikowany, a na powrót z wywołania metody, zawiera iloczyn dwóch macierzy. Dwa `PostConcat` metody są wywoływane w następujący sposób:

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

Te wywołania wykonać następującą operację:

A = A × B

Dwa `PreConcat` metody są podobne:

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

Te wywołania wykonać następującą operację:

A = B × A

Wersje tych wywołań metody wywołuje z wszystkimi `ref` argumenty są nieco bardziej efektywne, wywoływanie podstawowej implementacji, ale może być mylące dla kogoś czytanie kodu i przy założeniu, że wszystko przy `ref` argument jest zmodyfikowane przez metodę. Ponadto, często jest wygodne przekazać argument, który znajduje się wynik jednego z `Make` metody, na przykład:

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

Spowoduje to utworzenie następujących macierzy:

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

To jest skalowanie — przekształcenie pomnożona przez przekład — przekształcenie. W tym konkretnym przypadku `SKMatrix` struktura zapewnia skrótu za pomocą metody o nazwie [ `SetScaleTranslate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.SetScaleTranslate/p/System.Single/System.Single/System.Single/System.Single/):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

Jest to jedna z kilka razy, gdy jest bezpieczny w użyciu `SKMatrix` konstruktora. `SetScaleTranslate` Metoda ustawia wszystkich dziewięciu komórek macierzy. Jest również bezpiecznie korzystać `SKMatrix` konstruktora o statyczną `Rotate` i `RotateDegrees` metody:

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

Wykonaj te metody *nie* łączenie obrót — przekształcenie do istniejących transformacji. Metody ustawić komórkach macierzy. Są one funkcjonalnie identyczny `MakeRotation` i `MakeRotationDegrees` metody z tą różnicą, że nie utworzenia wystąpienia `SKMatrix` wartość.

Załóżmy, że masz `SKPath` obiekt, który chcesz wyświetlić, ale wolisz ją mieć nieco innej orientacji lub punkt innego Centrum. Współrzędne tej ścieżki można zmodyfikować przez wywołanie metody [ `Transform` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Transform/p/SkiaSharp.SKMatrix/) metody `SKPath` z `SKMatrix` argumentu. **Przekształcania ścieżki** strony pokazuje, jak to zrobić. [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) Klasy odwołania `HendecagramPath` obiektu w polu, ale używa jej konstruktora, aby zastosować przekształcenie do tej ścieżki:

```csharp
public class PathTransformPage : ContentPage
{
    SKPath transformedPath = HendecagramArrayPage.HendecagramPath;

    public PathTransformPage()
    {
        Title = "Path Transform";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        SKMatrix matrix = SKMatrix.MakeScale(3, 3);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(360f / 22));
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(300, 300));

        transformedPath.Transform(matrix);
    }
    ...
}
```

`HendecagramPath` Obiekt ma Centrum pod adresem (0, 0), i jedenaście punktów gwiazdy na zewnątrz rozszerza z tego Centrum 100 jednostek we wszystkich kierunkach. Oznacza to, że ścieżka ma współrzędne pozytywne i negatywne. **Przekształcania ścieżki** strony preferuje do pracy z gwiazdką trzykrotnie tak dużej i wszystkie współrzędne dodatnią. Ponadto go nie ma jeden punkt gwiazdkę, aby wskazać w górę. Chce zamiast jednego z punktów gwiazdy punktu w dół. (Ponieważ gwiazdka ma jedenaście punktów, go nie może mieć zarówno.) Ta migracja wymaga obracanie gwiazdka przez 360 stopni podzielona przez 22.

Tworzy konstruktora `SKMatrix` obiekt z trzech oddzielnych przekształcenia przy użyciu `PostConcat` metody z następującym wzorcem, w których wystąpienia elementu A, B i C `SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

To seria kolejnych mnożenia, więc wynik jest następujący:

A × B × C

Kolejne mnożenia pomocy zrozumieć działanie poszczególnych transformacji. Skalowanie — przekształcenie zwiększa rozmiar współrzędne ścieżki o 3, więc współrzędne do zakresu od –300 do 300. Obrót — przekształcenie obraca star wokół pochodzenia. Przekład — przekształcenie przenosi go następnie 300 pikseli prawo i w dół, więc wszystkie współrzędne stają się dodatnią.

Istnieją inne sekwencje, które tworzą tej samej macierzy. Oto inny:

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

To najpierw obraca ścieżce wokół środka i tłumaczy to po 100 pikseli po prawej stronie i w dół, więc wszystkie współrzędne są podawane dodatnią. Gwiazdka jest następnie zwiększyć rozmiar względem jego nowych lewego górnego rogu, który jest punktem (0, 0).

`PaintSurface` Obsługi można po prostu renderować tej ścieżki:

```csharp
public class PathTransformPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Magenta;
            paint.StrokeWidth = 5;

            canvas.DrawPath(transformedPath, paint);
        }
    }
}

```

Wydaje się w lewym górnym rogu kanwy:

[![](matrix-images/pathtransform-small.png "Potrójna zrzut ekranu przedstawiający stronę przekształcenia ścieżki")](matrix-images/pathtransform-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę przekształcenia ścieżki")

Konstruktor ten program mają zastosowanie macierzy do ścieżki przy użyciu następującego wywołania:

```csharp
transformedPath.Transform(matrix);
```

Ścieżka jest *nie* zachowania tej macierzy jako właściwość. Zamiast tego dotyczy ona przekształcenia wszystkie współrzędne ścieżki. Jeśli `Transform` nazywa się ponownie, transformacji jest ponownie stosowana, a jedynym sposobem, możesz przejść z powrotem, jest przez zastosowanie innej macierzy, która cofa transformacji. Na szczęście `SKMatrix` definiuje strukturę [ `TryInverse` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.TryInvert/p/SkiaSharp.SKMatrix/) metodę, która uzyskuje macierzy, który odwraca danej macierzy:

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

Metoda jest wywoływana `TryInverse` , ponieważ nie wszystkie macierzy są invertible, ale nie mogą być używane na potrzeby przekształcania grafiki — invertible macierzy.

Można również zastosować przekształcenie do macierzy `SKPoint` wartość, tablica punktów `SKRect`, lub nawet tylko jeden numer Twojego programu. `SKMatrix` Struktura obsługuje te operacje z kolekcją metod, które zaczynają się od słowa `Map`, takie jak te:

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

Jeśli używasz tej ostatniej metody, należy pamiętać, że `SKRect` struktury nie jest zdolny do reprezentowania obrócony prostokąta. Metoda tylko ma sens dla `SKMatrix` wartość reprezentująca tłumaczenia i skalowania.

### <a name="interactive-experimentation"></a>Interakcyjne eksperymentowanie

Jest jednym ze sposobów, aby można było uzyskać pewne pojęcie affine — przekształcenia interaktywnie przenoszenie trzy narożników mapy bitowej na ekranie i sprawdzając, jakie przekształcenie wyników. Jest to ideą **Pokaż przekształceniem Afinicznym macierzy** strony. Ta strona wymaga dwóch klas, które są również używane w innych pokazów:

[ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchPoint.cs) Klasa Wyświetla półprzezroczyste koło, które mogą być przeciągnięte na ekranie. `TouchPoint` wymaga, aby `SKCanvasView` lub element, który jest elementem nadrzędnym `SKCanvasView` mają [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchEffect.cs) dołączone. Ustaw `Capture` właściwość `true`. W `TouchAction` program obsługi zdarzeń musi wywołać `ProcessTouchEvent` method in Class metoda `TouchPoint` dla każdego `TouchPoint` wystąpienia. Metoda ta zwraca `true` Jeśli zdarzenia dotykowe spowodowało punktu touch przenoszenia. Ponadto `PaintSurface` obsługi musi wywołać `Paint` metodę w każdym `TouchPoint` wystąpienia, przekazując do niej `SKCanvas` obiektu.

`TouchPoint` przedstawia typowy sposób, że wizualizację SkiaSharp można hermetyzować w osobnej klasy. Klasę można zdefiniować właściwości do określania cech częścią wizualizacji i metodę o nazwie `Paint` z `SKCanvas` argument można renderować ją.

`Center` Właściwość `TouchPoint` wskazuje lokalizację obiektu. Tę właściwość można ustawić zainicjować lokalizacji. zmiany właściwości, gdy użytkownik przeciągnie wokół kanwy.

**Pokaż przekształceniem Afinicznym stronie macierzy** wymaga również [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/MatrixDisplay.cs) klasy. Ta klasa przedstawia komórek `SKMatrix` obiektu. Posiada dwie metody publiczne: `Measure` uzyskać wymiary renderowanej macierzy i `Paint` aby go wyświetlić. Zawiera klasy `MatrixPaint` właściwości typu `SKPaint` mogą być zastępowane dla inny rozmiar czcionki lub kolor.

[ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) plik `SKCanvasView` i dołącza `TouchEffect`. [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) pliku związanego z kodem tworzy trzy `TouchPoint` obiektów, a następnie ustawia je do pozycji odpowiadający trzy narożników mapę bitową, która ładuje z osadzonego Zasób:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    SKMatrix matrix;
    SKBitmap bitmap;
    SKSize bitmapSize;

    TouchPoint[] touchPoints = new TouchPoint[3];

    MatrixDisplay matrixDisplay = new MatrixDisplay();

    public ShowAffineMatrixPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }

        touchPoints[0] = new TouchPoint(100, 100);                  // upper-left corner
        touchPoints[1] = new TouchPoint(bitmap.Width + 100, 100);   // upper-right corner
        touchPoints[2] = new TouchPoint(100, bitmap.Height + 100);  // lower-left corner

        bitmapSize = new SKSize(bitmap.Width, bitmap.Height);
        matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                           touchPoints[1].Center,
                                           touchPoints[2].Center);
    }
    ...
}
```

Affine — macierz jest jednoznacznie zdefiniowany przez trzy punkty. Trzy `TouchPoint` obiektów odpowiada lewym, prawym górnym rogu i lewym dolnym rogu mapy bitowej. Ponieważ macierz przekształceniem afinicznym służy tylko do przekształcania prostokąt w równoległobok, punkt czwarty jest implikowany przez pozostałych trzech. Konstruktor stwierdza, z wywołaniem `ComputeMatrix`, — oblicza komórek `SKMatrix` obiekt z tych trzech punktów.

`TouchAction` Obsługi zdarzeń wywołuje `ProcessTouchEvent` metoda każdego `TouchPoint`. `scale` Konwertuje wartości z zestawu narzędzi Xamarin.Forms współrzędne pikseli:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = canvasView.CanvasSize.Width / (float)canvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                               touchPoints[1].Center,
                                               touchPoints[2].Center);
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Jeśli istnieją `TouchPoint` została przeniesiona, a następnie wywołuje metodę `ComputeMatrix` ponownie i unieważnia powierzchni.

`ComputeMatrix` Metoda określa macierzy też dorozumianych przez te trzy punkty. Macierz o nazwie `A` przekształcenia prostokąt kwadratowy jeden piksel w równoległobok oparte na trzy punkty podczas przekształcania skalowania o nazwie `S` skaluje mapy bitowej do jednego piksela prostokąt kwadrat. Złożone macierz jest `S` × `A`:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL)
    {
        // Scale transform
        SKMatrix S = SKMatrix.MakeScale(1 / size.Width, 1 / size.Height);

        // Affine transform
        SKMatrix A = new SKMatrix
        {
            ScaleX = ptUR.X - ptUL.X,
            SkewY = ptUR.Y - ptUL.Y,
            SkewX = ptLL.X - ptUL.X,
            ScaleY = ptLL.Y - ptUL.Y,
            TransX = ptUL.X,
            TransY = ptUL.Y,
            Persp2 = 1
        };

        SKMatrix result = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

Na koniec `PaintSurface` metoda powoduje wyświetlenie mapy bitowej, oparte na tej macierzy, Wyświetla macierz w dolnej części ekranu i renderuje punkty dotykowe w rogach trzy mapy bitowej:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap using the matrix
        canvas.Save();
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(matrix);

        matrixDisplay.Paint(canvas, matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));

        // Display the touchpoints
        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }
  }
```

Na poniższym ekranie pokazano dla systemu iOS zawiera mapę bitową po stronie najpierw jest ładowany, gdy dwa inne ekrany wyświetlane po niektórych manipulowania:

[![](matrix-images/showaffinematrix-small.png "Potrójna zrzut ekranu przedstawiający stronę pokazać przekształceniem Afinicznym macierzy")](matrix-images/showaffinematrix-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę pokazać affine — macierz")

Chociaż wydaje się tak, jakby punkty dotykowe przeciągnij narożniki mapy bitowej, który jest tylko wrażenie. Macierz obliczonym na podstawie punkty dotykowe przekształca mapę bitową tak, aby narożników pokrywają się z punktami touch.

Jest bardziej naturalne użytkownikom przenoszenie, zmienianie rozmiaru i Obróć mapy bitowe nie, przeciągając rogi, ale przy użyciu jednej lub dwóch palców bezpośrednio w obiekcie, aby przeciągnąć, ściśnięcie i Obróć. To zagadnienie opisano w następnym artykule [Touch manipulowania](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md).

### <a name="the-reason-for-the-3-by-3-matrix"></a>Przyczyna macierzy 3 x 3

Można się spodziewać, że system grafiki dwuwymiarowej wymaga tylko macierzy transformacji 2-przez-2:

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

Działa to w przypadku skalowania, obrotu i nawet pochylanie, ale nie jest zdolny najprostsza przekształceń, czyli tłumaczenia.

Problem polega na to, że macierz 2 na 2 reprezentuje *liniowej* przekształcania w dwóch wymiarach. Liniowy przekształcenie zachowuje niektóre podstawowe operacje arytmetyczne, ale jest jeden z skutków, że liniowej przekształcenie nigdy nie zmienia punktu (0, 0). Liniowy przekształcenie sprawia, że tłumaczenia jest niemożliwe.

W trzech wymiarach macierzy transformacji liniowej wygląda następująco:

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

Komórki oznaczone `SkewXY` oznacza, że wartość pochyla współrzędną X na podstawie wartości Y; komórki `SkewXZ` oznacza, że wartość pochyla współrzędną X na podstawie wartości Z; wartości pochylanie podobnie dla siebie `Skew` komórek.

Umożliwia ograniczenie tej macierzy transformacji 3D na płaszczyźnie dwuwymiarowej przez ustawienie `SkewZX` i `SkewZY` 0, a `ScaleZ` 1:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

Gdy grafiki dwuwymiarowej są rysowane w całości na płaszczyźnie w przestrzeni 3D, gdzie Z jest równa 1, mnożenie przekształcenie wygląda następująco:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Wszystko, co jest realizowany zgodnie z warstwą dwuwymiarową gdzie Z jest równa 1, ale `SkewXZ` i `SkewYZ` skutecznie powoduje utworzenie dwuwymiarową translację czynników.

Jest to, jak trójwymiarowej przekształcenie liniowy służy jako dwuwymiarową transformację nieliniowych. (Analogicznie przekształcenia w Grafika 3D są oparte na macierzy 4 na 4.)

`SKMatrix` Strukturze SkiaSharp definiuje właściwości trzeciego wiersza:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

Niezerowa Koniunkcja wartości `Persp0` i `Persp1` spowodować przekształcenia łączące obiektów poza warstwą dwuwymiarowy, gdzie Z jest równa 1. Co się stanie, gdy te obiekty są przeniesiony z powrotem do tej płaszczyzny został omówiony w artykule na [inne niż affine — przekształcenia](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md).


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
