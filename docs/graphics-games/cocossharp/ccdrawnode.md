---
title: Rysowanie geometrii z CCDrawNode
description: CCDrawNode udostępnia metody rysowania obiektów pierwotnych, takich jak linie, okręgi i trójkąty.
ms.prod: xamarin
ms.assetid: 46A3C3CE-74CC-4A3A-AB05-B694AE182ADB
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 83d973ed013d1448915ee553069c1366922d28b6
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="drawing-geometry-with-ccdrawnode"></a>Rysowanie geometrii z CCDrawNode

_`CCDrawNode` udostępnia metody rysowania obiektów pierwotnych, takich jak linie, okręgi i trójkąty._

`CCDrawNode` Klasa w CocosSharp udostępnia kilka metod uzyskania Rysowanie kształtów geometrycznych wspólnej. Dziedziczy on z `CCNode` klasy i zwykle jest dodawany do `CCLayer` wystąpień. W tym przewodniku przedstawiono sposób użycia `CCDrawNode` instancje, aby wykonać niestandardowe renderowanie. Zapewnia również kompleksowe funkcje dostępne rysowania z zrzuty ekranu i przykładów kodu.


## <a name="creating-a-ccdrawnode"></a>Tworzenie CCDrawNode

`CCDrawNode` Klasa może być używana do rysowania obiekty geometryczne, np. okręgi, prostokąty i wierszy. Na przykład następujący przykładowy kod przedstawia sposób tworzenia `CCDrawNode` wystąpienia, które rysuje koło w `CCLayer` implementującej klasy:


```csharp
public class GameLayer : CCLayer
{
    public GameLayer ()
    {
        var drawNode = new CCDrawNode ();
        this.AddChild (drawNode);
        // Origin is bottom-left of the screen. This moves
        // the drawNode 100 pixels to the right and 100 pixels up
        drawNode.PositionX = 100;
        drawNode.PositionY = 100;

        drawNode.DrawCircle (
            center: new CCPoint (0, 0),
            radius: 20,
            color: CCColor4B.White);

    }
} 
```

Ten kod tworzy następujące okręgu w czasie wykonywania:

![](ccdrawnode-images/image1.png "Ten kod tworzy ten koło w czasie wykonywania")


## <a name="draw-method-details"></a>Rysowanie szczegółów — metoda

Spójrzmy na kilka szczegóły związane z rysowaniem z `CCDrawNode`:


### <a name="draw-methods-positions-are-relative-to-the-ccdrawnode"></a>Rysuj metody pozycji są względem CCDrawNode

Wszystkie metody rysowania wymagają co najmniej jedną pozycję wartości do rysowania. Jest to wartość pozycji względem `CCDrawNode` wystąpienia. Oznacza to, że `CCDrawNode` sam ma pozycję i rysowanie wszystkie wywołania `CCDrawNode` również wykonać jedną lub więcej wartości pozycji. Aby lepiej zrozumieć, jak łączyć tych wartości, Oto kilka przykładów.

Najpierw przyjrzymy `DrawCircle` w powyższym przykładzie:


```csharp
...
drawNode.PositionX = 100;
drawNode.PositionY = 100;

drawNode.DrawCircle (center: new CCPoint (0, 0),
...
```

W takim przypadku `CCDrawNode` znajduje się w (100,100), a narysowanego okrąg — u (0,0) względem `CCDrawNode`, co trwa wyśrodkowany 100 pikseli w górę i w prawo w lewym dolnym rogu ekranu gry okręgu.

`CCDrawNode` Również może być umieszczony w źródle (lewej dolnej części ekranu), jednostki uzależnionej na okręgu dla przesunięcia:


```csharp
...
drawNode.PositionX = 0;
drawNode.PositionY = 0;

drawNode.DrawCircle (center: new CCPoint (50, 60),
...
```

Kod nad wyników w Centrum okrąg na 50 jednostek (`drawNode.PositionX` + `CCPoint.X`) na prawo od lewej części ekranu i 60 (`drawNode.PositionY` + `CCPoint.Y`) jednostki powyżej dolnej części ekranu.

Po wywołaniu metody rysowania, nie można zmodyfikować obiektu narysowanego, chyba że `CCDrawNode.Clear` metoda jest wywoływana, tak aby zmiany położenia musi odbywać się w `CCDrawNode` samej siebie.

Obiekty rysowane przez `CCNodes` również wpływ `CCNode` wystąpienia `Rotation` i `Scale` właściwości.


### <a name="draw-methods-do-not-need-to-be-called-every-frame"></a>Nie trzeba wywołać każdej ramki metod rysowania

Rysuj metody należy wywołać tylko raz, aby utworzyć trwały visual. W przykładzie przedstawionym powyżej wywołanie `DrawCircle` w Konstruktorze `GameLayer` — `DrawCircle` nie trzeba wywołać co ramki do ponownego rysowania okręgu podczas odświeżania ekranu.

To różni się od metod rysowania w MonoGame, które zwykle spowoduje, że coś do ekranu dla tylko jednej ramki, i który musi zostać wywołana co ramki.

Jeśli rysowania metody są wywoływane każdej ramki, obiekty ostatecznie będą gromadzone wewnątrz wywołania `CCDrawNode` wystąpienia, co powoduje spadek szybkość klatek jako rysowania więcej obiektów.


### <a name="each-ccdrawnode-supports-multiple-draw-calls"></a>Każdy CCDrawNode obsługuje wiele wywołań rysowania

`CCDrawNode` wystąpień może służyć do wielu kształtów. Pozwala to złożonych obiektów visual można zamontowane w jeden obiekt. Na przykład następujący kod może służyć do renderowania wielu okręgi z jednym `CCDrawNode`:


```csharp
for (int i = 0; i < 8; i++)
{
    drawNode.DrawCircle (
        center: new CCPoint (i*15, 0),
        radius: 20,
        color: CCColor4B.White);
} 
```

Powoduje to na poniższym rysunku:

![](ccdrawnode-images/image2.png "Powoduje to grafiki")


## <a name="draw-call-examples"></a>Przykłady wywołań rysowania

Dostępne są następujące wywołania rysowania `CCDrawNode`:

- [`DrawCatmullRom`](#drawcatmullrom)
- [`DrawCircle`](#drawcircle)
- [`DrawCubicBezier`](#drawcubicbezier)
- [`DrawEllipse`](#drawellipse)
- [`DrawLineList`](#drawlinelist)
- [`DrawPolygon`](#drawpolygon)
- [`DrawQuadBezier`](#drawquadbezier)
- [`DrawRect`](#drawrect)
- [`DrawSegment`](#drawsegment)
- [`DrawSolidArc`](#drawsolidarc)
- [`DrawSolidCircle`](#drawsolidcircle)
- [`DrawTriangleList`](#drawtrianglelist)


### <a name="drawcardinalspline"></a>DrawCardinalSpline

`DrawCardinalSpline` Tworzy linię krzywej przez zmienną liczbę punktów. 

`config` Parametr definiuje wskazującą przekaże krzywej składanej. W poniższym przykładzie pokazano krzywej składanej, który przechodzi przez cztery punkty.

`tension` Parametr określa sposób ostrych lub round punktów na krzywej składanej są wyświetlane. A `tension` wartości 0 spowoduje krzywej składanej i `tension` wartości 1 spowoduje krzywej składanej narysowanymi przez proste i twardych krawędzi.

Choć krzywe linie, krzywe CocosSharp rysuje krzywe z proste. `segments` Parametr określa, ile segmentów, aby narysować krzywej składanej. Większą liczbę powoduje sprawnie krzywej składanej kosztem wydajności. 

Przykład kodu:


```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (50, 70));
splinePoints.Add (new CCPoint (0, 140));
splinePoints.Add (new CCPoint (100, 210));

drawNode.DrawCardinalSpline (
    config: splinePoints,
    tension: 0,
    segments: 64,
    color:CCColor4B.Red); 
```

![](ccdrawnode-images/image3.png "Parametr segmentów Określa, ile segmentów, aby narysować krzywej składanej")


### <a name="drawcatmullrom"></a>DrawCatmullRom

`DrawCatmullRom` Tworzy linię krzywej przez zmienną liczbę punktów, podobnie jak `DrawCardinalLine`. Ta metoda nie ma parametru naprężenia.

Przykład kodu:

```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (80, 90));
splinePoints.Add (new CCPoint (100, 0));
splinePoints.Add (new CCPoint (0, 130)); 

drawNode.DrawCatmullRom (
    points: splinePoints,
    segments: 64); 
```

![](ccdrawnode-images/image4.png "DrawCatmullRom tworzy krzywej przez zmienną liczbę punktów, podobnie jak DrawCardinalLine")


### <a name="drawcircle"></a>DrawCircle

`DrawCircle` Tworzy obwodu koło z danym `radius`.

Przykład kodu:

```csharp
drawNode.DrawCircle (
    center:new CCPoint (0, 0),
    radius:20,
    color:CCColor4B.Yellow); 
```

![](ccdrawnode-images/image5.png "DrawCircle tworzy obwodowej koła danego serwera radius")


### <a name="drawcubicbezier"></a>DrawCubicBezier

`DrawCubicBezier` Rysuje linię krzywej między dwoma punktami przy użyciu punktów kontrolnych, aby ustawić ścieżkę między dwoma punktami.

Przykład kodu:

```csharp
drawNode.DrawCubicBezier (
    origin: new CCPoint (0, 0),
    control1: new CCPoint (50, 150),
    control2: new CCPoint (250, 150),
    destination: new CCPoint (170, 0),
    segments: 64,
    lineWidth: 1,
    color: CCColor4B.Green); 
```

 ![](ccdrawnode-images/image6.png "DrawCubicBezier rysuje linię krzywej między dwoma punktami")


### <a name="drawellipse"></a>DrawEllipse

`DrawEllipse` Tworzy konturu *elipsy*, która jest często określany jako oval (mimo że nie są one identyczne geometryczne). Można zdefiniować kształtu elipsy `CCRect` wystąpienia.

Przykład kodu:


```csharp
drawNode.DrawEllipse (
    rect: new CCRect (0, 0, 130, 90),
    lineWidth: 2,
    color: CCColor4B.Gray); 
```

![](ccdrawnode-images/image8.png "DrawEllipse tworzy konturu elipsy, która jest często określany jako elipsę")


### <a name="drawline"></a>DrawLine

`DrawLine` nawiązanie połączenia punktów linią danego szerokości. Ta metoda jest podobna do `DrawSegment`, z wyjątkiem tworzy płaskiej punkty końcowe przeciwieństwie round punktów końcowych.

Przykład kodu:


```csharp
drawNode.DrawLine (
    from: new CCPoint (0, 0),
    to: new CCPoint (150, 30),
    lineWidth: 5,
    color:CCColor4B.Orange); 
```

![](ccdrawnode-images/image9.png "DrawLine łączy do punktów linią danego szerokości")


### <a name="drawlinelist"></a>DrawLineList

`DrawLineList` Tworzy wiele wierszy, łącząc każda para o punktów `CCV3F_C4B` tablicy. `CCV3F_C4B` Struktury zawiera wartości dla pozycja i kolor. `verts` Parametru zawsze powinna zawierać parzystą liczbę punktów, ustalony każdego wiersza przez dwa punkty.

Przykład kodu:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First line:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    // second line, will blend from white to red:
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(120,120), CCColor4B.Red)
};

drawNode.DrawLineList (verts); 
```

 ![](ccdrawnode-images/image10.png "Parametr verts zawsze powinna zawierać parzystą liczbę punktów, ustalony każdego wiersza przez dwa punkty.")




### <a name="drawpolygon"></a>DrawPolygon

`DrawPolygon` Tworzy wielokąta wypełniane konspektu o zmiennej szerokości i koloru.

Przykład kodu:


```csharp
CCPoint[] verts = new CCPoint[] {
    new CCPoint(0,0),
    new CCPoint(0, 100),
    new CCPoint(50, 150),
    new CCPoint(100, 100),
    new CCPoint(100, 0)
};

drawNode.DrawPolygon (verts,
    count: verts.Length,
    fillColor: CCColor4B.White,
    borderWidth: 5,
    borderColor: CCColor4B.Red,
    closePolygon: true); 
```

![](ccdrawnode-images/image11.png "DrawPolygon tworzy wielokąta wypełniane konspektu o zmiennej szerokości i koloru")


### <a name="drawquadbezier"></a>DrawQuadBezier

`DrawQuadBezier` łączy dwa punkty z wierszem. Działa podobnie do `DrawCubicBezier` , ale tylko obsługującego punkt kontrolny pojedynczego.

Przykład kodu:


```csharp
drawNode.DrawQuadBezier (
    origin:new CCPoint (0, 0),
    control:new CCPoint (200, 0),
    destination:new CCPoint (0, 300),
    segments:64,
    lineWidth:1,
    color:CCColor4B.White);
```

![](ccdrawnode-images/image12.png "DrawQuadBezier łączy dwa punkty z wiersza")


### <a name="drawrect"></a>DrawRect

`DrawRect` Tworzy wypełniony prostokąt z konturem o zmiennej szerokości i koloru.

Przykład kodu: 


```csharp
var shape = new CCRect (
    0, 0, 100, 200);
drawNode.DrawRect(shape,
    fillColor:CCColor4B.Blue,
    borderWidth: 4,
    borderColor:CCColor4B.White); 
```

![](ccdrawnode-images/image13.png "DrawRect tworzy wypełniony prostokąt z konturem o zmiennej szerokości i koloru")


### <a name="drawsegment"></a>DrawSegment

`DrawSegment` łączy dwa punkty z wiersza o zmiennej szerokości i koloru. Jest on podobny do `DrawLine`, z wyjątkiem tworzy round punktów końcowych, a nie płaskiej punktów końcowych.

Przykład kodu:


```csharp
drawNode.DrawSegment (from: new CCPoint (0, 0),
    to: new CCPoint (100, 200),
    radius: 5,
    color:new CCColor4F(1,1,1,1)); 
```

![](ccdrawnode-images/image14.png "DrawSegment łączy dwa punkty z wiersza o zmiennej szerokości i koloru")


### <a name="drawsolidarc"></a>DrawSolidArc

`DrawSolidArc` Tworzy klinowania wypełniane danego koloru i usługi radius.

Przykład kodu:


```csharp
drawNode.DrawSolidArc(
    pos:new CCPoint(100, 100),
    radius:200,
    startAngle:0,
    sweepAngle:CCMathHelper.Pi/2, // this is in radians, clockwise
    color:CCColor4B.White); 
```

![](ccdrawnode-images/image15.png "DrawSolidArc tworzy klinowania wypełniane danego koloru i usługi radius")


### <a name="drawsolidcircle"></a>DrawSolidCircle

`DrawCircle` Tworzy koła wypełnionego promienia danego.

Przykład kodu:


```csharp
drawNode.DrawSolidCircle(
    pos: new CCPoint (100, 100),
    radius: 50,
    color: CCColor4B.Yellow); 
```

![](ccdrawnode-images/image16.png "DrawCircle tworzy koła wypełnionego promienia danego")


### <a name="drawtrianglelist"></a>DrawTriangleList

`DrawTriangleList` Tworzy listę trójkątów. Każdy trójkąt jest definiowana za pomocą trzech `CCV3F_C4B` wystąpień w tablicy. Liczba wierzchołków w tablicy przekazanych do `verts` parametr musi być wielokrotnością liczby 3. Należy pamiętać, że tylko informacje zawarte w `CCV3F_C4B` to pozycja verts i ich kolor — `DrawTriangleList` — metoda nie obsługuje rysunku trójkąty z tekstury.

Przykład kodu:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First triangle:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    // second triangle, each point has different colors:
    new CCV3F_C4B( new CCPoint(90,0), CCColor4B.Yellow),
    new CCV3F_C4B( new CCPoint(120,60), CCColor4B.Red),
    new CCV3F_C4B( new CCPoint(150,0), CCColor4B.Blue)
};

drawNode.DrawTriangleList (verts); 
```

![](ccdrawnode-images/image17.png "DrawTriangleList tworzy listę trójkąty")


## <a name="summary"></a>Podsumowanie

W tym przewodniku objaśniono sposób tworzenia `CCDrawNode` i wykonywać renderowania na podstawie typów pierwotnych. Stanowi przykład każdego wywołania rysowania.

## <a name="related-links"></a>Linki pokrewne

- [CCDrawNode interfejsu API](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
- [Pełny przykład](https://developer.xamarin.com/samples/mobile/CCDrawNode/)
