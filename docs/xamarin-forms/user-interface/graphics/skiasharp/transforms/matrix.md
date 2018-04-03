---
title: Przekształcenia macierzowe
description: Przejść głębiej do przekształcenia SkiaSharp z macierzy transformacji elastyczne
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: d40f898f07077ec2e2466cd5de3cd582636cfb15
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2018
---
# <a name="matrix-transforms"></a>Przekształcenia macierzowe

_Przejść głębiej do przekształcenia SkiaSharp z macierzy transformacji elastyczne_

Wszystkie transformacji zastosowanych do `SKCanvas` obiektu są konsolidowane w jednym wystąpieniu [ `SKMatrix` ](https://developer.xamarin.com/api/type/SkiaSharp.SKMatrix/) struktury. Jest to podobne do tych we wszystkich systemach nowoczesnych grafiki 2D standardowe macierzy transformacji 3 x 3.

Jak przedstawiono, można użyć transformacji w SkiaSharp bez wiedzy przekształcenia macierzy, ale macierzy transformacji jest ważna z punktu widzenia teoretycznego i jest niezwykle istotne w przypadku używania przekształceń do modyfikowania ścieżki lub obsługi złożonych dotykowego, oba przedstawiono w którym w tym artykule i dalej.

![](matrix-images/matrixtransformexample.png "Mapy bitowej poddane affine — przekształcenia")

Stosowane do bieżącego macierzy transformacji `SKCanvas` jest dostępna w dowolnym momencie po zalogowaniu się tylko do odczytu do [ `TotalMatrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.TotalMatrix/) właściwości. Można ustawić używającego macierzy transformacji [ `SetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.SetMatrix/p/SkiaSharp.SKMatrix/) metody, a można przywrócić tej macierzy transformacji do wartości domyślnych, wywołując [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix/).

Tylko innych `SKCanvas` element członkowski, który współpracuje bezpośrednio z obszaru roboczego macierzy transformacji jest [ `Concat` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Concat/p/SkiaSharp.SKMatrix@/) które łączy macierzy dwa mnożąc je razem.

Domyślne macierzy transformacji macierzą i składa się z 1 w komórkach ukośnych i wszędzie else 0:

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

Możesz utworzyć macierzą przy użyciu statycznych [ `SKMatrix.MakeIdentity` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeIdentity()/) metody:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

`SKMatrix` Domyślny konstruktor jest *nie* zwracać macierzą. Zwraca macierzy z wszystkie komórki ustawione na zero. Nie używaj `SKMatrix` Konstruktor Jeśli nie chcesz ręcznie ustawić tych komórek.

Gdy SkiaSharp renderuje obiektu graficznego, każdy punkt (x, y) skutecznie jest konwertowana na macierzy 1 na 3 z 1 trzeciej kolumny:

<pre>
| x  y  1 |
</pre>

Ta macierz 1 na 3 reprezentuje punkt trójwymiarowy z Współrzędna ustawioną wartość 1. Dlaczego dwuwymiarowa macierzy transformacji wymaga Praca w trzech wymiarach jest przyczyn matematyczne (omówione później). Można potraktować ten macierzy 1 na 3 jako reprezentujący punkt 3D układu współrzędnych, ale zawsze na płaszczyźnie 2D gdzie Z jest równa 1.

Ta macierz 1 na 3 jest mnożony przez macierzy transformacji, a wynik jest punktem renderowane w obszarze roboczym:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

Punkty przekonwertowany przy użyciu mnożenie macierzy standardowe, są następujące:

x' = x

y' = y

z' = 1

To jest domyślną transformację.

Gdy `Translate` wywoływana jest metoda `SKCanvas` obiektu, `tx` i `ty` argumenty `Translate` metody stają się dwóch pierwszych komórek w trzecim wierszu macierzy transformacji:

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

Iloczyn jest teraz w następujący sposób:

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

Poniżej przedstawiono formuły przekształcenia:

x' = x + tx

y "= y + ty

Czynniki skalowania ma wartość domyślną 1. Podczas wywoływania `Scale` metody na nowym `SKCanvas` obiektu zawiera macierzy transformacji wynikowe `sx` i `sy` argumenty w komórkach ukośnych:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

Przekształcanie formuły są następujące:

x' = sx · x

y "= sy · y

Po wywołaniu macierzy transformacji `Skew` zawiera dwa argumenty w komórkach macierzy sąsiadujące skalowania czynniki:

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

Przekształcanie formuły są:

x "= x + xSkew · y

y "= ySkew · x + y

Wywołania `RotateDegrees` lub `RotateRadians` kąta α macierzy transformacji jest następująca:

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

Poniżej przedstawiono formuły przekształcenia:

x "= cos(α) · x - sin(α) · y

y "= sin(α) · x - cos(α) · y

Gdy α jest równy 0 stopni, jest macierzą. Α po 180 stopni macierzy transformacji jest następujący:

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

180 stopni jest odpowiednikiem Przerzucanie obiektu w poziomie i w pionie, które również odbywa się przez ustawienie skali czynników -1.

Tego rodzaju przekształceń są sklasyfikowane jako *podobne* przekształca. Affine — przekształcenia nigdy nie obejmują trzeciej kolumny macierzy, który pozostaje na wartości domyślne, 0, 0 lub 1. Artykuł [inne niż affine — przekształcenia](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md) omówiono-affine — przekształcenia.

## <a name="matrix-multiplication"></a>Mnożenie macierzy

Jedną z zalet big przy użyciu macierzy transformacji jest można uzyskać przez mnożenie macierzy, która jest często określany w dokumentacji SkiaSharp jako złożonych przekształceń *łączenia*. Wiele metod Przekształć powiązane `SKCanvas` odwołują się do "łączenia wstępnego" lub "concat sprzed." Odnosi się kolejnością mnożenie, co jest ważne, ponieważ nie jest przemienne mnożenie macierzy.

Na przykład w dokumentacji [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) metody mówi jego "Pre-concats bieżącego macierzy z określonym tłumaczenia" podczas dokumentację dla [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) Metoda mówi jego "Pre-concats bieżącego macierzy o określonej skali."

Oznacza to, że przekształcenia, określonego przez wywołanie metody jest mnożnik (lewego operandu) i bieżącego macierzy transformacji jest multiplicand (prawostronny operand).

Załóżmy, że `Translate` jest wywoływana po nich `Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

`Scale` Transformacji jest mnożona przez `Translate` przekształcenie macierzy złożone przekształcenia:

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` może zostać wywołana przed `Translate` podobnie do następującej:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

W takim przypadku kolejnością mnożenie została wycofana i skalowania czynniki efektywnie są stosowane do tłumaczenia czynniki:

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

Oto `Scale` metody za pomocą punktu obrotu:

```csharp
canvas.Scale(sx, sy, px, py);
```

Jest to równoważne następujące wywołania Przetłumacz i skali:

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

`SKMatrix` Struktury definiuje dziewięć właściwości odczytu/zapisu typu `float` odpowiadający dziewięć komórkach macierzy transformacji:

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` definiuje również właściwość o nazwie [ `Values` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix.Values/) typu `float[]`. Tej właściwości można ustawić lub uzyskać dziewięć wartości w jednej zrzut w kolejności `ScaleX`, `SkewX`, `TransX`, `SkewY`, `ScaleY`, `TransY`, `Persp0`, `Persp1`, i `Persp2`.

`Persp0`, `Persp1`, I `Persp2` komórki zostały omówione w artykule [inne niż affine — przekształcenia](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md). Jeśli te komórki mają wartości domyślne, 0, 0 lub 1, transformacja jest mnożona przez współrzędnych punktu następująco:

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

x "= ScaleX · x + SkewX · y + TransX

y "= SkewX · x + ScaleY · y + TransY

z' = 1

Jest to pełny dwuwymiarowa affine — przekształcenia. Affine — przekształcenia zachowuje równoległych, co oznacza, że prostokąt nigdy nie jest on przekształcany w innym niż równoległobok.

`SKMatrix` Struktury definiuje kilka metod statycznych, aby utworzyć `SKMatrix` wartości. Te wszystkie zwracany `SKMatrix` wartości:

- [`MakeTranslation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeTranslation/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/System.Single/System.Single/) z punktu przestawnego
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/) kąt w radianach
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/System.Single/System.Single/) kąt w radianach do punktu przestawnego
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/)
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/System.Single/System.Single/) z punktu przestawnego
- [`MakeSkew`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeSkew/p/System.Single/System.Single/)

`SKMatrix` definiuje również kilka metod statycznych, które łączenie macierzy dwa, co oznacza, że należy pomnożyć je. Te metody są nazywane `Concat`, `PostConcat`, i `PreConcat`, i istnieją dwie wersje każdego z nich. Te metody mają nie zwracanych wartości; Zamiast tego odwołujących się do istniejących `SKMatrix` wartości za pośrednictwem `ref` argumentów. W poniższym przykładzie `A`, `B`, i `R` ("wynik") są wszystkie `SKMatrix` wartości.

Dwa `Concat` metody są wywoływane w następujący sposób:

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

Te należy wykonać następujące mnożenia:

R = B × A

Inne metody ma tylko dwa parametry. Pierwszy parametr jest zmodyfikowany, a na powrót z wywołania metody, zawiera produktu macierzy dwa. Dwa `PostConcat` metody są wywoływane w następujący sposób:

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

Wersje tych wywołań metody ze wszystkimi `ref` argumenty są nieco bardziej wydajne wywołać podstawowej implementacji, ale może być mylące dla kogoś odczytywanie kodu i przy założeniu, że wszystko przy `ref` argument jest zmodyfikowane przez metodę. Ponadto często jest wygodne do przekazania argument, który znajduje się wynik jednego z `Make` metod, na przykład:

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

Spowoduje to utworzenie poniższej tabeli:

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

Jest to pomnożona przez przekształcenie Przetłumacz transformacji skali. W tym przypadku `SKMatrix` struktury udostępnia metodę o nazwie skrót [ `SetScaleTranslate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.SetScaleTranslate/p/System.Single/System.Single/System.Single/System.Single/):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

Jest to jedna z kilka razy, gdy jest bezpiecznie korzystać `SKMatrix` konstruktora. `SetScaleTranslate` Metoda ustawia wszystkie komórki dziewięć macierzy. Jest również bezpiecznie korzystać `SKMatrix` konstruktora z statycznych `Rotate` i `RotateDegrees` metod:

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

Wykonaj te metody *nie* przekształcenie obracania do przekształcania istniejącego połączenia. Metody ustawić wszystkich komórkach macierzy. Są one identyczne funkcjonalnie do `MakeRotation` i `MakeRotationDegrees` metody z tą różnicą, że nie utworzyć wystąpienia `SKMatrix` wartość.

Załóżmy, że masz `SKPath` obiekt, który chcesz wyświetlić, ale chcesz użyć, że ma orientację nieco inny lub innego środka. Współrzędne tę ścieżkę można zmodyfikować przez wywołanie metody [ `Transform` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Transform/p/SkiaSharp.SKMatrix/) metody `SKPath` z `SKMatrix` argumentu. **Przekształcenie ścieżki** strony pokazano, jak to zrobić. [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) Odwołania do klasy `HendecagramPath` obiektu w polu, ale używa jego konstruktora do stosowania przekształcenia do tej ścieżki:

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

`HendecagramPath` Obiekt ma Centrum na (0, 0) i 11 punktów gwiazdy rozszerzyć na zewnątrz z tym Centrum przez 100 jednostek we wszystkich kierunkach. Oznacza to, że ścieżka ma współrzędne zarówno dodatnie i ujemne. **Przekształcenie ścieżki** strony preferuje do pracy z gwiazdką trzy razy na duży i wszystkie współrzędne dodatnią. Ponadto go nie ma jeden punkt gwiazdy do punktu w górę. Chce ona zamiast jednego z punktów gwiazdy punktu w dół. (Ponieważ gwiazdy ma 11 punktów, nie może mieć zarówno.) Wymaga to obracanie gwiazdy przez 360 stopni rozdzielonych 22.

Tworzy konstruktora `SKMatrix` obiektu z trzech oddzielnych transformacji przy użyciu `PostConcat` metody za pomocą następującego wzorca, w którym są wystąpieniami klasy A, B i C `SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

Jest to szeregu kolejnych mnożenia, więc wynik jest następujący:

A × B × C

Kolejne mnożenia pomocy w zrozumienie, czego każdego przekształcenia. Przekształcanie skali spowoduje zwiększenie rozmiaru współrzędne ścieżki o 3, więc współrzędne należeć do zakresu od –300 do 300. Przekształcenie obracania obraca gwiazdy wokół swoją witryną źródłową. Przekształcanie Przetłumacz przewiduje ją następnie przez 300 pikseli prawo i w dół, więc wszystkie współrzędne, stają się dodatnią.

Brak innych sekwencji, które powodują powstanie tej samej macierzy. Oto co:

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

To najpierw obraca ścieżki wokół środka i tłumaczy go po 100 pikseli w prawo i w dół, więc wszystkie współrzędne są dodatnie. Gwiazdy następnie zwiększają rozmiar względem jego nowego lewego górnego rogu, który jest punktem (0, 0).

`PaintSurface` Obsługi można po prostu renderować tę ścieżkę:

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

Wygląda na to, w lewym górnym rogu obszaru roboczego:

[![](matrix-images/pathtransform-small.png "Potrójna zrzut ekranu strony Przekształcenie ścieżki")](matrix-images/pathtransform-large.png#lightbox "Potrójna zrzut ekranu strony Przekształcenie ścieżki")

Konstruktor ten program dotyczy macierzy ścieżkę o następujące wywołanie:

```csharp
transformedPath.Transform(matrix);
```

Ścieżka jest *nie* zachować tej macierzy jako właściwość. Zamiast tego stosuje przekształcenia do wszystkich współrzędne ścieżki. Jeśli `Transform` nazywa się ponownie, ponownie stosowana jest transformacja i jest jedynym sposobem możesz wrócić przez zastosowanie innej macierzy, która spowoduje cofnięcie transformacji. Na szczęście `SKMatrix` definiuje strukturę [ `TryInverse` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.TryInvert/p/SkiaSharp.SKMatrix/) metody uzyskujący macierzy odwraca danej macierzy:

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

Metoda jest wywoływana `TryInverse` , ponieważ nie wszystkie macierze się odwracalne, ale nie można odwrócić macierzy nie mogą być używane na potrzeby przekształcania grafiki.

Można także zastosować dla macierzy transformacji do `SKPoint` wartość, tablica punktów, `SKRect`, lub nawet jednego numeru w programie. `SKMatrix` Struktura obsługuje te operacje z kolekcją metod, które zaczynają się od słowa `Map`, takich jak te:

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

Jeśli używasz tej ostatniej metody, należy pamiętać, że `SKRect` struktury nie jest zdolny do reprezentowania prostokątne obracany. Metoda ma sens tylko `SKMatrix` wartość reprezentującą tłumaczenia i skalowania.

### <a name="interactive-experimentation"></a>Eksperymenty interakcyjne

Jest jednym ze sposobów uzyskać pewne pojęcie affine — przekształcenia interaktywnie przenoszenie trzy narożników mapy bitowej po ekranie i sprawdzając, jakie przekształcenia wyniki. Jest to ideą **Pokaż podobne macierzy** strony. Ta strona wymaga dwóch klas, które są również używane w innych pokazów:

[ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchPoint.cs) Klasa Wyświetla półprzezroczyste okrąg, który może być przeciągnięte po ekranie. `TouchPoint` wymaga, aby `SKCanvasView` lub element, który jest elementem nadrzędnym `SKCanvasView` ma [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchEffect.cs) dołączony. Ustaw `Capture` właściwości `true`. W `TouchAction` program obsługi zdarzeń, należy wywołać `ProcessTouchEvent` metody w `TouchPoint` dla każdego `TouchPoint` wystąpienia. Metoda zwraca `true` Jeśli zdarzenie touch spowodowała punktu touch przenoszenia. Ponadto `PaintSurface` obsługi musi wywołać `Paint` metody w każdym `TouchPoint` wystąpienia, przekazywanie do niego `SKCanvas` obiektu.

`TouchPoint` przedstawia typowy sposób, że element wizualny SkiaSharp można hermetyzowany w osobnej klasy. Klasę można zdefiniować właściwości służący do określania właściwości elementu wizualnego i metody o nazwie `Paint` z `SKCanvas` argument może renderować ją.

`Center` Właściwość `TouchPoint` wskazuje lokalizację, do obiektu. Tej właściwości można ustawić zainicjować lokalizacji. zmiany właściwości, gdy użytkownik przeciąga wokół obszaru roboczego.

**Pokaż stronę macierzy podobne** wymaga również [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/MatrixDisplay.cs) klasy. Ta klasa przedstawia komórki `SKMatrix` obiektu. Składa się z dwóch metod publicznych: `Measure` uzyskanie wymiary renderowanych macierzy, i `Paint` aby go wyświetlić. Zawiera klasy `MatrixPaint` właściwości typu `SKPaint` mogą być zastępowane dla inny rozmiar lub kolor.

[ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) tworzy plik `SKCanvasView` i dołącza `TouchEffect`. [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) plik CodeBehind tworzy trzy `TouchPoint` obiekty i ustawia je do pozycji odpowiadający trzy narożników ładowaną z osadzonych mapy bitowej Zasób:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
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

Affine — macierz jest unikatowo zdefiniowane przez trzy punkty. Trzy `TouchPoint` obiektów odpowiada lewym górnym, prawym górnym i lewym dolnym rogu mapy bitowej. Ponieważ podobne macierzy służy tylko do przekształcania prostokąt w równoległobok, punkt czwarty technicznego przez inne trzech. Konstruktor stwierdza, w wyniku wywołania `ComputeMatrix`, który oblicza komórki `SKMatrix` obiektu z tych trzech punktów.

`TouchAction` Wywołań obsługi `ProcessTouchEvent` metody każdego `TouchPoint`. `scale` Wartość konwertuje współrzędne platformy Xamarin.Forms następującą liczbę pikseli:

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

`ComputeMatrix` Metoda określa macierzy implikowana przez te trzy punkty. Wywołuje macierzy `A` transformacje jeden piksel kwadratowe prostokąta do równoległobok oparte na trzy punkty podczas skalowania przekształcenia o nazwie `S` skaluje mapy bitowej do jeden piksel kwadratowe prostokąta. Jest złożone `S` x `A`:

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

        SKMatrix result;
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

Na koniec `PaintSurface` — metoda renderuje mapy bitowej oparte na tej macierzy, wyświetla macierzy w dolnej części ekranu i renderuje punkty dotykowe w trzech narożnikach mapy bitowej:

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

Na poniższym zrzucie ekranu z systemem iOS przedstawia mapy bitowej, gdy strona jest ładowana jako pierwsza, podczas dwóch innych ekranów pokazać po niektórych manipulowania:

[![](matrix-images/showaffinematrix-small.png "Potrójna zrzut ekranu przedstawiający stronę Pokaż podobne macierzy")](matrix-images/showaffinematrix-large.png#lightbox "Potrójna zrzut ekranu przedstawiający stronę Pokaż podobne macierzy")

Chociaż wydaje się tak, jakby punktów touch przeciągnij narożników mapy bitowej, która jest tylko wrażenie. Macierz obliczana na podstawie punktów touch przekształca mapę bitową tak, aby narożnikach pokrywa się z punktami touch.

Jest więcej fizycznych, aby umożliwić użytkownikom przenoszenie, zmienianie rozmiaru i Obróć mapy bitowe nie przeciągając rogi, ale przy użyciu jednym lub dwoma palcami bezpośrednio na obiekcie, aby przeciągnąć, ściśnięcie i Obróć. Ten temat znajdują się w artykule [Touch manipulowania](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md).

### <a name="the-reason-for-the-3-by-3-matrix"></a>Przyczyna macierzy 3 x 3

Można się spodziewać, że system dwuwymiarowa grafiki wymaga tylko macierzy transformacji 2-za-2:

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

Działa to w przypadku skalowania, obracanie i pochylanie nawet, ale nie jest zdolny najprostsza transformacje, który jest tłumaczenie.

Problem jest, że reprezentuje macierzy 2-za-2 *liniowej* przekształcania w dwóch wymiarów. Przekształcenie liniowej zachowuje niektóre podstawowe operacje arytmetyczne, a skutków jest czy liniowej transformacji nigdy nie zmienia punkt (0, 0). Przekształcenie liniowej uniemożliwia tłumaczenia.

W trzech wymiarów macierzy transformacji liniowej wygląda następująco:

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

Komórki etykietą `SkewXY` oznacza, że wartość pochyla współrzędną X na podstawie wartości Y; komórki `SkewXZ` oznacza, że wartość pochyla współrzędną X na podstawie wartości Z; wartości podobnie pochylanie dla siebie `Skew` komórek.

Umożliwia ograniczenie tej macierzy transformacji 3D do dwuwymiarowego rzutu przez ustawienie `SkewZX` i `SkewZY` 0 i `ScaleZ` 1:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

Jeśli dwuwymiarowa grafiki są rysowane na płaszczyźnie przestrzeni 3D, gdzie Z jest równa 1, mnożenia przekształcenia wygląda następująco:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Wszystko pozostaje na płaszczyźnie dwuwymiarowa gdzie Z jest równa 1, ale `SkewXZ` i `SkewYZ` komórek skutecznie stają się dwuwymiarową translację czynników.

Jest to, jak trójwymiarową transformację liniowej służy jako dwuwymiarową transformację liniowej. (Analogicznie transformacje w grafiki 3D są oparte na macierz 4 przez 4.)

`SKMatrix` Struktury SkiaSharp definiuje właściwości dla tego trzeciego wiersza:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

Wartości inne niż zero `Persp0` i `Persp1` spowodować przekształcenia mające przesuwanie poza dwuwymiarowego rzutu, w którym Z jest równa 1. Co się stanie, jeśli te obiekty są przeniesiony z powrotem do tej płaszczyzny zostało opisane w artykule na [inne niż affine — przekształcenia](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md).


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
