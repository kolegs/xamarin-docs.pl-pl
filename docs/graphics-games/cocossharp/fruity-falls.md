---
title: Szczegóły gier owocowy wypada
description: W tym przewodniku przegląda gry wypada Fruity obejmujące wspólnej CocosSharp i tworzenia gier pojęcia, takie jak fizycznych, zarządzania zawartością stanu gry i gier projektu.
ms.prod: xamarin
ms.assetid: A5664930-F9F0-4A08-965D-26EF266FED24
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 19b57d41fdf8aa4f8a749cf4979755161a0e7e51
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921563"
---
# <a name="fruity-falls-game-details"></a>Szczegóły gier owocowy wypada

_W tym przewodniku przegląda gry wypada Fruity obejmujące wspólnej CocosSharp i tworzenia gier pojęcia, takie jak fizycznych, zarządzania zawartością stanu gry i gier projektu._

Wypada owocowy jest prostą, opartą na fizycznych gry, w którym odtwarzacz sortuje owoców czerwony i żółty do kolorowe zasobników uzyskanie punktów. Celem gry jest uzyskanie dowolną liczbę punktów możliwe bez umożliwienie spadek owoców do niewłaściwej pojemnika zakończenia gry.

![](fruity-falls-images/image1.png "Celem gry jest uzyskanie dowolną liczbę punktów możliwe bez umożliwienie spadek owoców do niewłaściwej pojemnika zakończenia gry")

Wypada owocowy rozszerza pojęciami opisanymi w [przewodnik BouncingGame](~/graphics-games/cocossharp/bouncing-game.md) przez dodanie poniższego:

 - Zawartości w formie PNG
 - Zaawansowane fizycznych
 - Stanu gry (przejścia między sceny)
 - Możliwość dostrojenia współczynników gier za pomocą zmiennych zawarte w jednej klasy
 - Wbudowana obsługa debugowania visual
 - Kod organizacji przy użyciu jednostek gier
 - Projekt gier koncentruje się na wartość fun i powtórzeń

Gdy [przewodnik BouncingGame](~/graphics-games/cocossharp/bouncing-game.md) wypada Fruity koncentruje się na wprowadzenie podstawowe koncepcje CocosSharp, pokazuje, jak wszystkie ze sobą w celu gotowego produktu gier. Ponieważ ten przewodnik odwołuje się BouncingGame, czytników najpierw należy zapoznać się z [przewodnik BouncingGame](~/graphics-games/cocossharp/bouncing-game.md) przed odczytaniem w tym przewodniku.

Ten przewodnik zawiera implementację projektu Fruity powróci do udostępniającym szczegółowe informacje ułatwiające własne gry. Obejmuje on następujące tematy:


- [Klasa GameController](#gamecontroller-class)
- [Gry jednostek](#game-entities)
- [Owoców grafiki](#fruit-graphics)
- [Fizycznych](#physics)
- [Gry zawartości](#game-content)
- [GameCoefficients](#gamecoefficients)


## <a name="gamecontroller-class"></a>Klasa GameController

Projekt owocowy PCL wypada zawiera `GameController` klasy, która odpowiada za tworzenie wystąpień gry i przechodzenia między sceny. Ta klasa jest używana przez iOS i Android projekty w celu wyeliminowania zduplikowanych kodu.

Kod zawarty w `Initialize` metoda jest podobna do kodu w`Initialize` w niezmienionym szablonu CocosSharp, ale zawiera wiele zmian.

Domyślnie `GameView.ContentManager.SearchPaths` są zależne od rozdzielczości urządzenia. Jak wyjaśniono poniżej bardziej szczegółowo, wypada Fruity używa tej samej zawartości, niezależnie od rozdzielczości, więc `Initialize` dodaje metody `Images` ścieżka (z bez podfolderów) do `SearchPaths`:


```csharp
contentSearchPaths.Add ("Images");
```

Nowe szablony CocosSharp zawiera dziedziczenia z klasy `CCLayer`, co oznacza, że gier elementów wizualnych i logiki powinni zostać dodani do tej klasy. Zamiast tego należy Fruity używa wielu `CCLayer` kolejność rysowania wystąpień do formantu. Te `CCLayer` wystąpienia są zawarte w `GameView` klasy, która dziedziczy `CCScene`. Wypada owocowy zawiera także wiele scen, przy czym pierwszy `TitleScene`. W związku z tym `Initialize` metoda tworzy `TitleScene` wystąpienia, który jest przekazywany do `RunWithScene`:


```csharp
var scene = new TitleScene (GameView);
GameView.Director.RunWithScene (scene);
```

Zawartość wypada Fruity został utworzony jako niskiej pikseli grafiki estetycznych powodów. `GameView.DesignResolution` Ustawiono tak, aby gry jest tylko 384 jednostki szerokości i wysoka 512:


```csharp
// We use a lower-resolution display to get a pixellated appearance
int width = 384;
int height = 512;
GameView.DesignResolution = new CCSizeI (width, height); 
```

Na koniec `GameController` klasa dostarcza metody statycznej, który może być wywoływany przez żadną `CCGameScene` do przejścia na inny `CCScene`. Ta metoda służy do przenoszenia między `TitleScene` i `GameScene`.


## <a name="game-entities"></a>Gry jednostek

Wypada owocowy korzysta z wzorca jednostki dla większości obiektów gier. Szczegółowy opis tego wzorca znajdują się w [przewodnik jednostek w CocosSharp](~/graphics-games/cocossharp/entities.md).

Implementowanie jednostki obiektów gier znajduje się w folderze jednostek w **FruityFalls.Common** projektu:

![](fruity-falls-images/image2.png "Folder jednostki w projekcie FruityFalls.Common")

Jednostki są obiekty, które dziedziczą z `CCNode`i może mieć elementów wizualnych, kolizji i zachowanie co ramki.

`Fruit` Obiekt reprezentuje typowy jednostki CocosSharp: zawiera obiekt wizualny, kolizji oraz logiki w każdej ramce. Jego konstruktor jest odpowiedzialny za inicjowanie owocu:


```csharp
public Fruit ()
{
    CreateFruitGraphic ();
    if (GameCoefficients.ShowCollisionAreas)
    {
        CreateDebugGraphic ();
    }
    CreateCollision ();
    CreateExtraPointsLabel ();
    Acceleration.Y = GameCoefficients.FruitGravity;
}
```


### <a name="fruit-graphics"></a>Owoców grafiki

`CreateFruitGraphic` Metoda tworzy `CCSprite` wystąpienia i dodaje go do `Fruit`. `IsAntialiased` Właściwość jest ustawiona na FAŁSZ umożliwiają gry wygląd podzielony na piksele. Ta wartość jest ustawiana na wartość false na wszystkich `CCSprite` i `CCLabel` wystąpień w trakcie gry:


```csharp
private void CreateFruitGraphic()
{
    graphic = new CCSprite ("cherry.png");
    graphic.IsAntialiased = false;
    this.AddChild (graphic);
}
```

Po każdej zmianie dotyka odtwarzacz `Fruit` wystąpienia z `Paddle`, wartość punktu tego owoców zwiększa się o jeden. Zapewnia to dodatkowe żądania do doświadczonych graczy na łatwiejszą obsługę owoców dla dodatkowe punkty. Wartość punktu owocu jest wyświetlany za pomocą `extraPointsLabel` wystąpienia.

`CreateExtraPointsLabel` Metoda tworzy `CCLabel` wystąpienia i dodaje go do `Fruit`:


```csharp
private void CreateExtraPointsLabel()
{
    extraPointsLabel = new CCLabel("", "Arial", 12, CCLabelFormat.SystemFont);
    extraPointsLabel.IsAntialiased = false;
    extraPointsLabel.Color = CCColor3B.Black;
    this.AddChild(extraPointsLabel);
}
```

Wypada owocowy obejmuje dwa różne rodzaje owoców — wiśni i cytryn. `CreateFruitGraphic` Przypisuje visual domyślny, ale elementy wizualne owoców można zmienić za pomocą `FruitColor` właściwość, która z kolei wywołuje `UpdateGraphics`:


```csharp
private void UpdateGraphics()
{
if (GameCoefficients.ShowCollisionAreas)
    {
        debugGrahic.Clear ();
        const float borderWidth = 4;
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius,
            CCColor4B.Black);
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius - borderWidth,
            fruitColor.ToCCColor ());
    }
    if (this.FruitColor == FruitColor.Yellow)
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("lemon.png");
        extraPointsLabel.Color = CCColor3B.Black;
        extraPointsLabel.PositionY = 0;
    }
    else
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("cherry.png");
        extraPointsLabel.Color = CCColor3B.White;
        extraPointsLabel.PositionY = -8;
    }
}
```

Pierwsza instrukcja if w `UpdateGraphics` aktualizacji grafiki debugowania, które są używane do wizualizacji kolizji obszarów. Te elementy wizualne są wyłączone, zanim ostateczną wersją gry staje się, ale mogą być przechowywane podczas tworzenia do debugowania fizycznych. Druga część zmiany `graphic.Texture` właściwości przez wywołanie metody `CCTextureCache.SharedTextureCache.AddImage`. Ta metoda zapewnia dostęp do tekstury według nazwy pliku. Więcej informacji na temat tej metody można znaleźć w [buforowanie tekstury przewodnik](~/graphics-games/cocossharp/texture-cache.md).

`extraPointsLabel` Kolor jest dostosowana do zachować kontrastu z obrazem owoców i jego `PositionY` wartość jest dostosowana do Centrum `CCLabel` owocu `CCSprite`:

![](fruity-falls-images/image3.png "Kolor extraPointsLabel jest dostosowana do zachowania kontrastu z obrazem owoców i jego wartość PositionY jest dostosowana do środka CCLabel na owoców CCSprite")

![](fruity-falls-images/image4.png "Kolor extraPointsLabel jest dostosowana do zachowania kontrastu z obrazem owoców i jego wartość PositionY jest dostosowana do środka CCLabel na owoców CCSprite")


### <a name="collision"></a>Kolizji

Wypada owocowy zaimplementuje rozwiązanie niestandardowe kolizji w folderze geometrii przy użyciu obiektów:

![](fruity-falls-images/image5.png "Wypada owocowy zaimplementuje rozwiązanie niestandardowe kolizji w folderze geometrii przy użyciu obiektów")

Kolizji w wypada Fruity jest zaimplementowany przy użyciu `Circle` lub `Polygon` obiektu, zwykle za pomocą jednego z tych dwóch typów dodawany jako element podrzędny jednostki. Na przykład `Fruit` obiekt ma `Circle` o nazwie `Collision` która inicjuje go w jego `CreateCollision` metody:


```csharp
private void CreateCollision()
{
    Collision = new Circle ();
    Collision.Radius = GameCoefficients.FruitRadius;
    this.AddChild (Collision);
}
```

Należy pamiętać, że `Circle` klasa dziedziczy `CCNode` klasy, więc można go dodać jako element podrzędny do jednostek naszych gry:


```csharp
public class Circle : CCNode
{
    ...
}
```

`Polygon` Tworzenie jest podobny do `Circle` tworzenie, jak pokazano w `Paddle` klasy:


```csharp
private void CreateCollision()
{
    Polygon = Polygon.CreateRectangle(width, height);
    this.AddChild (Polygon);
}
```

Logika kolizji jest objęte [dalszej części tego przewodnika](#collision).


## <a name="physics"></a>Fizycznych

Fizycznych w wypada Fruity można podzielić na dwie kategorie: przemieszczanie i kolizji. 


### <a name="movement-using-velocity-and-acceleration"></a>Przenoszenie przy użyciu szybkość pracy i przyspieszenia

Używa owocowy wypada `Velocity` i `Acceleration` wartości kontroli przemieszczania jej podmioty, podobnie jak [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md). Jednostek implementować ich logiki przenoszenia metodę o nazwie `Activity`, która jest wywoływana raz na klatkę. Na przykład można zobaczyć implementacji przepływu w `Fruit` klasy `Activity` metody:

```csharp
public void Activity(float frameTimeInSeconds)
{
    timeUntilExtraPointsCanBeAdded -= frameTimeInSeconds;
    
    // linear approximation:
    this.Velocity += Acceleration * frameTimeInSeconds;

    // This is a linear approximation to drag. It's used to
    // keep the object from falling too fast, and eventually
    // to slow its horizontal movement. This makes the game easier
    this.Velocity -= Velocity * GameCoefficients.FruitDrag * frameTimeInSeconds;
    
    this.Position += Velocity * frameTimeInSeconds;
}
```
`Fruit` Obiektu jest unikatowa w jej implementacja przeciągania — porusza się wartość, która zmniejsza szybkość pracy w odniesieniu do sposobu szybkiego owocu. Ta implementacja przeciągania udostępnia *terminali prędkość*, co jest maksymalną szybkość przypadające wystąpienia owocu. Przeciągnij spowalnia poziome przepływu owoców, co sprawia, że gry nieco łatwiejsze do odtwarzania.

`Paddle` Obiekt ma również zastosowanie `Velocity` w jego `Activity` metody, ale jego `Velocity` jest obliczana przy użyciu `desiredLocation` wartość:


```csharp
public void Activity(float frameTimeInSeconds)
{
    // This code will cause the cursor to lag behind the touch point
    // Increasing this value reduces how far the paddle lags
    // behind the player’s finger. 
    const float velocityCoefficient = 4;
    // Get the velocity from current location and touch location
    Velocity = (desiredLocation - this.Position) * velocityCoefficient;
    this.Position += Velocity * frameTimeInSeconds;
    ...
}
```

Zwykle, gier, które używają `Velocity` do sterowania położeniem ich obiektów nie jest bezpośrednio ustawić pozycji jednostki, co najmniej nie po zainicjowaniu. Zamiast bezpośrednio ustawienie `Paddle` pozycji `Paddle.HandleInput` przypisuje metody `desiredLocation` wartość:

```csharp
public void HandleInput(CCPoint touchPoint)
{
    desiredLocation = touchPoint;
}
```

### <a name="collision"></a>Kolizji

Owocowy wypada implementuje częściowo realistyczne zderzenia owoców i innych obiektów collidable, takich jak `Paddle` i `GameScene.Splitter`. Aby debugować kolizji wypada Fruity kolizji obszarów były widoczne, zmieniając `GameCoefficients.ShowDebugInfo` w `GameCoefficients.cs` pliku:


```csharp
public static class GameCoefficients
{
    ... 
    public const bool ShowCollisionAreas = true;
    ...
}
```

Ustawienie tej wartości na `true` Umożliwia renderowanie kolizji obszarów:  

![](fruity-falls-images/image6.png "Ustawienie tej wartości na wartość true umożliwia renderowanie obszarów kolizji")

Zaczyna logiki kolizji w `GameScene.Activity` metody:


```csharp
private void Activity(float frameTimeInSeconds)
{
    if (hasGameEnded == false)
    {
        paddle.Activity(frameTimeInSeconds);
        foreach (var fruit in fruitList)
        {
            fruit.Activity(frameTimeInSeconds);
        }
        spawner.Activity(frameTimeInSeconds);
        DebugActivity();
        PerformCollision();
    }
}
```

`PerformCollision` obsługuje wszystkie kolizji `Fruit` wystąpienia z innymi obiektami: 


```csharp
private void PerformCollision()
{
    // reverse for loop since fruit may be destroyed:
    for(int i = fruitList.Count - 1; i > -1; i--)
    {
        var fruit = fruitList[i];
        FruitVsPaddle(fruit);
        FruitPolygonCollision(fruit, splitter.Polygon, CCPoint.Zero);
        FruitVsBorders(fruit);
        FruitVsBins(fruit);
    }
}
```

#### <a name="fruitvsborders"></a>FruitVsBorders

`FruitVsBorders` kolizji przeprowadza własnej logiki kolizji zamiast polegania na logice zawarte w innej klasy. Ta różnica nie istnieje, ponieważ zderzenia owoców i obramowania ekranu nie jest dokładnie pełny — istnieje możliwość owoców, które mają być mieści się krawędzi ekranu, dokładne paletkę ruchem. Owoców będzie Odbijanie poza ekran po trafieniu z paletki, ale jeśli odtwarzacz powoli wypchnięcia owoców przejdzie poza krawędź włączenia i wyłączenia ekranu:


```csharp
private void FruitVsBorders(Fruit fruit)
{
    if(fruit.PositionX - fruit.Radius < 0 && fruit.Velocity.X < 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionX + fruit.Radius > gameplayLayer.ContentSize.Width && fruit.Velocity.X > 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionY + fruit.Radius > gameplayLayer.ContentSize.Height && fruit.Velocity.Y > 0)
    {
        fruit.Velocity.Y *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
}
```

#### <a name="fruitvsbins"></a>FruitVsBins

`FruitVsBins` Metoda jest odpowiedzialna za sprawdzania, czy wszystkie owoców spadła do jednej z dwóch bins. Jeśli tak, odtwarzacza otrzymał punktów (jeśli owoców/bin kolory dopasowania) lub gra kończy się (jeśli kolory nie są zgodne):


```csharp
private void FruitVsBins(Fruit fruit)
{
    foreach(var bin in fruitBins)
    {
        if(bin.Polygon.CollideAgainst(fruit.Collision))
        {
            if(bin.FruitColor == fruit.FruitColor)
            {
                // award points:
                score += 1 + fruit.ExtraPointValue;
                scoreText.Score = score;
                Destroy(fruit);
            }
            else
            {
                EndGame();
            }
            break;
        }
    }
}
```

#### <a name="fruitvspaddle-and-fruitpolygoncollision"></a>FruitVsPaddle i FruitPolygonCollision

Owoców a paletkę i owoców a podziału (obszar rozdzielających dwa bins) kolizji za pośrednictwem `FruitPolygonCollision` metody. Ta metoda ma trzy części:

1. Sprawdź, czy obiekty dojść do kolizji
1. Przenieść obiekty (tylko owoce wypada Fruity), tak aby nie nakładają się
1. Dostosuj prędkość obiektów (tylko owoce wypada Fruity), aby symulować podskokiem, poniższy kod przedstawia punktów powyżej:


```csharp
private static bool FruitPolygonCollision(Fruit fruit, Polygon polygon, CCPoint polygonVelocity)
{
    // Test whether the fruit collides
    bool didCollide = polygon.CollideAgainst(fruit.Collision);
    if (didCollide)
    {
        var circle = fruit.Collision;
        // Get the separation vector to reposition the fruit so it doesn't overlap the polygon
        var separation = CollisionResponse.GetSeparationVector(circle, polygon);
        fruit.Position += separation;
        // Adjust the fruit's Velocity to make it bounce:
        var normal = separation;
        normal.Normalize();
        fruit.Velocity = CollisionResponse.ApplyBounce(
            fruit.Velocity, 
            polygonVelocity, 
            normal, 
            GameCoefficients.FruitCollisionElasticity);
    }
    return didCollide;
}
```

Wypada owocowy kolizji odpowiedź jest jednostronne — są ustawiane tylko owoców szybkość pracy i pozycji. Warto zauważyć, że inne gry może mieć kolizji, co wpływa na oba obiekty, których to dotyczy, takie jak znak wypychanie boulder lub dwóch samochodów awarii do siebie.

Obliczenia za logiki kolizji zawarte w `Polygon` i `CollisionResponse` klasy wykracza poza zakres tego podręcznika, ale jako napisany, te metody mogą być używane dla wielu typów gier. Wielokąta i `CollisionResponse` klasy nawet obsługuje nieregularnych i wypukłych wielokąty, dlatego ten kod może być używany dla wielu typów gier.

 


## <a name="game-content"></a>Gry zawartości

Grafikę w wypada Fruity natychmiast odróżnia gry od BouncingGame. Podczas gry projektów są podobne, odtwarzaczach będzie wyświetlany natychmiast różnica w wygląd dwóch gier. Często gracze zdecyduj, czy próby gry przez jego elementy wizualne. W związku z tym jest zasadnicze znaczenie, że deweloperzy inwestycji zasobów podczas podejmowania atrakcyjność gier.

Grafiki w wypada Fruity utworzono za pomocą następujących celów:

 - Spójne motywu
 - Nacisk położono na pojedynczy znak — małp Xamarin
 - Jasny kolorów, aby utworzyć mniejsze jest przyjemne wystąpić
 - Natomiast między tła i pierwszego planu, co upraszcza śledzić wizualnie obiektów gry
 - Możliwość tworzenia prostych efekty wizualne bez animacji ciężki zasobów


### <a name="content-location"></a>Lokalizacja zawartości

Wypada owocowy obejmuje całą jego zawartość w folderze obrazów w projekt systemu Android:

![](fruity-falls-images/image7.png "Wypada owocowy obejmuje całą jego zawartość folderu obrazów w projekcie systemu Android")

Tych samych plików połączonych w projekcie systemu iOS, aby uniknąć duplikowania i tak zmiany do plików mieć wpływ na obu projektów:

![](fruity-falls-images/image8.png "Tych samych plików połączonych w projekcie systemu iOS, aby uniknąć duplikowania, a oba projekty wpływ tak zmiany do plików")

Warto zauważyć, że zawartość nie jest zawarty w **Ld** lub **Hd** foldery, które są częścią szablonu CocosSharp domyślne. **Ld** i **Hd** foldery mają służyć do gier, które zawierają dwa zestawy zawartości — jeden dla urządzeń niższej rozdzielczości, takich jak telefony i jeden dla urządzeń wysokiej rozdzielczości, takich jak tablety. Grafik wypada Fruity celowo tworzona jest podzielony na piksele estetycznych, więc nie trzeba wprowadzić zawartość dla różnych rozmiarów ekranu. W związku z tym **Ld** i **Hd** foldery zostały całkowicie usunięte z projektu.


### <a name="gamescene-layering"></a>Tworzenie warstw GameScene

Jak wspomniano wcześniej w tym przewodniku `GameScene` jest odpowiedzialny za wszystkie wystąpienia obiektu gier, pozycjonowanie i logiki obiektów (takich jak kolizji). Wszystkie obiekty są dodawane do jednej z czterech `CCLayer` wystąpień:


```csharp
CCLayer backgroundLayer;
CCLayer gameplayLayer;
CCLayer foregroundLayer;
CCLayer hudLayer;
```

Te cztery warstwy są tworzone w `CreateLayers` metody. Należy pamiętać, że są one dodawane do `GameScene` w kolejności wstecz do przodu:


```csharp
private void CreateLayers()
{
    backgroundLayer = new CCLayer();
    this.AddLayer(backgroundLayer);
    gameplayLayer = new CCLayer();
    this.AddLayer(gameplayLayer);
    foregroundLayer = new CCLayer();
    this.AddLayer(foregroundLayer);
    hudLayer = new CCLayer();
    this.AddLayer(hudLayer);
}
```

Po utworzeniu warstwy, wszystkie obiekty visual są dodawane do warstwy przy użyciu `AddChild` metody, jak pokazano w `CreateBackground` metody:


```csharp
private void CreateBackground()
{
    var background = new CCSprite("background.png");
    background.AnchorPoint = new CCPoint(0, 0);
    background.IsAntialiased = false;
    backgroundLayer.AddChild(background);
}
```


### <a name="vine-entity"></a>Winorośli jednostki

`Vine` Jednostki jednoznacznie jest używany dla zawartości — nie ma to wpływu na gry. Składa się z 20 `CCSprite` wystąpienia liczbą wybranych przez prób i błędów, aby upewnić się, winorośli zawsze osiągnie górnej części ekranu:


```csharp
public Vine ()
{
    const int numberOfVinesToAdd = 20;
    for (int i = 0; i < numberOfVinesToAdd; i++)
    {
        var graphic = new CCSprite ("vine.png");
        graphic.AnchorPoint = new CCPoint(.5f, 0);
        graphic.PositionY = i * graphic.ContentSize.Height;
        this.AddChild (graphic);
    }
}
```

Pierwszy `CCSprite` znajduje się więc pozycji winorośli odpowiada jego dolnej krawędzi. Jest to osiągane przez ustawienie AnchorPoint `new CCPoint(.5f, 0)`. `AnchorPoint` Używa właściwości *znormalizowanych współrzędnych* którym z zakresu od 0 do 1 niezależnie od rozmiaru `CCSprite`:

![](fruity-falls-images/image9.png "Właściwość AnchorPoint używa znormalizowane współrzędne którym z zakresu od 0 do 1 niezależnie od rozmiaru CCSprite")

Ikony winorośli kolejne będą umieszczane przy użyciu `graphic.ContentSize.Height` wartość, która zwraca wysokość obrazu w pikselach. Na poniższym diagramie przedstawiono, jak grafiki winorośli Kafelki w górę:

![](fruity-falls-images/image10.png "Ten diagram przedstawia, jak grafiki winorośli Kafelki w górę")

Ponieważ pochodzenia `Vine` jednostka jest w dolnej części winorośli, a następnie jego położenie jest proste, jak to pokazano w `Paddle.CreateVines` metody:


```csharp
private void CreateVines()
{
    // Increasing this value moves the vines closer to the 
    // center of the paddle.
    const int vineDistanceFromEdge = 4;
    leftVine = new Vine ();
    var leftEdge = -width / 2.0f;
    leftVine.PositionX = leftEdge + vineDistanceFromEdge;
    this.AddChild (leftVine);
    rightVine = new Vine ();
    var rightEdge = width / 2.0f;
    rightVine.PositionX = rightEdge - vineDistanceFromEdge;
    this.AddChild (rightVine);
}
```

Winorośli wystąpień w `Paddle` jednostka ma również interesujące zachowanie obrotu. Ponieważ `Paddle` jednostki jako całość obraca się zgodnie z danych wejściowych player, (co w tym przewodniku opisano szczegółowo poniżej), `Vine` wystąpień również ma wpływ ta obrotu. Jednak obracanie `Vine` wystąpienia wraz z programem `Paddle` tworzy visual niepożądane, jak pokazano w poniższej animacji:

![](fruity-falls-images/image11.gif "Jednak obracanie wystąpień winorośli wraz z paletki tworzy visual niepożądane, jak pokazano w poniższej animacji")

Do przeciwdziałania `Paddle` obracanie `leftVine` i `rightVine` wystąpienia są obracane paletki, jak pokazano w kierunku odwrotnym `Paddle.Activity` metody:


```csharp
public void Activity(float frameTimeInSeconds)
{
    ...
    // This value adds a slight amount of rotation
    // to the vine. Having a small amount of tilt looks nice.
    float vineAngle = this.Velocity.X / 100.0f;
    leftVine.Rotation = -this.Rotation + vineAngle;
    rightVine.Rotation = -this.Rotation + vineAngle;
}
```

Zwróć uwagę, że niewielkie obrotu został dodany do winorośli za pośrednictwem `vineAngle` współczynnik. Aby dostosować, ile Obróć winorośli można zmienić tę wartość.


## <a name="gamecoefficients"></a>GameCoefficients

Co dobrej gry jest produktu iteracji, więc wypada Fruity zawiera klasy o nazwie `GameCoefficients` które kontroluje sposób gry jest odtwarzany. Ta klasa zawiera obszerne zmiennych, które są używane w trakcie gry do sterowania fizycznych, układ, dochodzi do uruchamiania i oceniania.

Na przykład fizycznych owoców są kontrolowane przez następujące zmienne:


```csharp
public static class GameCoefficients
{
    ...
    
    // The strength of the gravity. Making this a 
    // smaller (bigger negative) number will make the
    // fruit fall faster. A larger (smaller negative) number
    // will make the fruit more floaty.
    public const float FruitGravity = -60;
    // The impact of "air drag" on the fruit, which gives
    // the fruit terminal velocity (max falling speed) and slows
    // the fruit's horizontal movement (makes the game easier)
    public const float FruitDrag = .1f;
    // Controls fruit collision bouncyness. A value of 1
    // means no momentum is lost. A value of 0 means all
    // momentum is lost. Values greater than 1 create a spring-like effect
    public const float FruitCollisionElasticity = .5f;
    
    ...
}
```

Jak oznacza nazwy tych zmiennych może służyć do dostosować szybkość owoców powróci, jak poziomy przepływu spowalnia przez czas i bounciness paletkę.

Gry współczynników przechowywane w kodu lub pliki danych (na przykład XML) mogą być szczególnie cenne dla zespołów z gier projektantów, którzy nie są również programistów.

`GameCoefficients` Klasa zawiera także wartości włączenie informacji o debugowaniu, takiej jak duplikowanie informacji lub kolizji obszarów:


```csharp
public static class GameCoefficients
{
    ...
    // This controls whether debug information is displayed on screen.
    public const bool ShowDebugInfo = false;
    public const bool ShowCollisionAreas = false;
    ...
}
```


## <a name="conclusion"></a>Wniosek

W tym przewodniku przedstawione gry wypada Fruity. On objęty koncepcji, w tym zawartości, fizycznych i zarządzanie stanem gier.

## <a name="related-links"></a>Linki pokrewne

- [Dokumentacja interfejsu API CocosSharp](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Ukończono projekt (przykład)](https://developer.xamarin.com/samples/mobile/FruityFalls/)
