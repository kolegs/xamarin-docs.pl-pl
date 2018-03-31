---
title: Implementowanie podskakujące gry
description: W tym przewodniku pokazano, jak dodać logikę gier i zawartości do pustego szablonu do tworzenia gier pełne o nazwie BouncingGame.
ms.topic: article
ms.prod: xamarin
ms.assetid: AC9FD56F-6E4A-40DA-8168-45A761D869FD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 584ae03a3a773ae7e16fa7a24b9dbfa6c5056342
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/30/2018
---
# <a name="implementing-the-bouncing-game"></a>Implementowanie podskakujące gry

_W tym przewodniku pokazano, jak dodać logikę gier i zawartości do pustego szablonu do tworzenia gier pełne o nazwie BouncingGame._

Poprzedniej części tego przewodnika pokazano, jak utworzyć rozwiązanie CocosSharp z projektów dla systemu iOS i Android development. W tym przewodniku będzie zaimplementowanie pełne gier, obejmujące następujące:

 - Rozpakować naszej zawartości
 - Wspólne elementy wizualne CocosSharp
 - Dodawanie elementów wizualnych na `GameLayer`
 - Implementowanie logiki co ramki

Nasze Zakończono gry będzie wyglądać następująco:

![](part2-images/image1.png "Zakończono gry będzie wyglądać następująco")


# <a name="unzipping-our-game-content"></a>Rozpakować naszej zawartości

Deweloperzy gier często używany jest termin *zawartości* do odwoływania się do plików z systemem innym niż kodu, które są zwykle tworzone przez projektantów audio, gier projektantów lub artystów visual. Popularne typy zawartości obejmują pliki służące do wyświetlania elementów wizualnych, Odtwórz dźwięk i kontrolowania zachowania sztucznego analizy (AI). Z perspektywy zespołu deweloperów gier zawartość jest zazwyczaj tworzony przez zwykłym. Nasze gry obejmuje dwa typy zawartości:

 - PNG, pliki do definiowania wygląd naszego Piłka i paletkę.
 - Pojedynczy plik XNB do definiowania czcionkę używaną przez naszych wyświetlania wynik (szczegółowo podczas dodamy naszych wyświetlania wyniku poniżej)

Ta zawartość używane w tym miejscu można znaleźć w [zawartości zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true). Potrzebujemy tych plików pobrane i rozpakowane do lokalizacji, który firma Microsoft będzie miał dostęp w dalszej części tego przewodnika.


# <a name="common-cocossharp-visual-elements"></a>Wspólne elementy CocosSharp Visual

CocosSharp zawiera szereg klas, używany do wyświetlania elementów wizualnych. Niektóre elementy są bezpośrednio widoczne, a inne są używane w organizacji. W naszym gry będą używane następujące:

 - `CCNode` — Klasa podstawowa dla wszystkich obiektów visual CocosSharp. `CCNode` Klasa zawiera `AddChild` funkcja, która może służyć do tworzenia hierarchii nadrzędny/podrzędny, nazywana także *drzewa wizualnego* lub *wykres sceny*. Wszystkie klasy wymienione poniżej dziedziczą `CCNode`.
 - `CCScene` — Główny drzewa wizualnego wszystkich CocosSharp gier. Wszystkie elementy wizualne musi być częścią drzewa wizualnego z `CCScene` w katalogu głównym, lub nie będą one widoczne.
 - `CCLayer` — Obiekty kontenera wizualnego, takich jak `CCSprite`. Jak wskazuje nazwę, `CCLayer` klasa jest używana do kontrolowania sposobu visual warstwy elementów na siebie.
 - `CCSprite` — Wyświetla obraz lub część obrazu. `CCSprite` wystąpienia może być umieszczony, zmiany rozmiaru i podaj liczbę efektów wizualnych.
 - `CCLabel` — Wyświetla ciąg na ekranie. Czcionka używana przez `CCLabel` jest zdefiniowana w pliku XNB. Omówiono XNBs bardziej szczegółowo poniżej.

Aby zrozumieć, jak są używane różne typy teraz przyjrzymy się jeden `CCSprite`. Elementy wizualne musi zostać dodany do `CCLayer`, i musi mieć drzewa wizualnego `CCScene` w jego katalogu głównego. W związku z tym relacji nadrzędny/podrzędny z jednym `CCSprite` będzie `CCScene`  >  `CCLayer`  >  `CCSprite`.


# <a name="adding-visual-elements-to-gamelayer"></a>Dodawanie elementów wizualnych do GameLayer


## <a name="adding-the-paddlesprite"></a>Dodawanie paddleSprite

Początkowo zaplanujemy podstawy gry czarne, a także dodać jeden `CCSprite` renderowania na ekranie. Modyfikowanie `GameLayer` klasa do dodania `CCSprite` w następujący sposób:


```csharp
using System;
using System.Collections.Generic;
using CocosSharp;
namespace BouncingGame
{
    public class GameLayer : CCLayer
    {
        CCSprite paddleSprite;
        public GameLayer () : base(CCColor4B.Black)
        {
            // "paddle" refers to the paddle.png image
            paddleSprite = new CCSprite ("paddle");
            paddleSprite.PositionX = 100;
            paddleSprite.PositionY = 100;
            AddChild (paddleSprite);
        }
        protected override void AddedToScene ()
        {
            base.AddedToScene ();
            // Use the bounds to layout the positioning of our drawable assets
            CCRect bounds = VisibleBoundsWorldspace;
            // Register for touch events
            var touchListener = new CCEventListenerTouchAllAtOnce ();
            touchListener.OnTouchesEnded = OnTouchesEnded;
            AddEventListener (touchListener, this);
        }
        void OnTouchesEnded (List<CCTouch> touches, CCEvent touchEvent)
        {
            if (touches.Count > 0)
            {
                // Perform touch handling here
            }
        }
    }
}
```

Powyższy kod tworzy pojedynczy `CCSprite` i dodaje go jako element podrzędny `GameLayer`. `CCSprite` Konstruktor pozwala zdefiniować plik obrazu, który będzie używany jako ciąg. Naszego kodu informuje CocosSharp, aby wyszukać plik o nazwie `paddle` (rozszerzenie zostanie pominięty, które omówiono w dalszej części tego przewodnika). CocosSharp będzie szukał plików o nazwach `paddle` w folderze głównym zawartości (czyli **zawartości**) oraz foldery dodane do `gameView.ContentManager.SearchPaths` (opisanych w poprzedniej sekcji).

Dodamy plików bezpośrednio do katalogu głównego **zawartości** folderu dla systemu iOS i Android. Aby to zrobić, kliknij prawym przyciskiem myszy lub sterowania, kliknij pozycję **zawartości** folderu projektu iOS i wybierz **Dodaj** > **Dodawanie plików...** Przejdź do których zawartość możemy unzipped wcześniej i wybierz **paddle.png**. Jeśli pojawi się pytanie o tym, jak dodać plik do folderu, należy wybrać możemy **kopiowania** opcji:

![](part2-images/image2.png "Jeśli pojawi się pytanie o tym, jak dodać plik do folderu, wybierz opcję Kopiuj")

Następnie dodamy plik do projektu systemu Android. Kliknij prawym przyciskiem myszy lub kliknięcia formantu folderu zawartości (w **zasoby** folderu na projekty systemu Android) i wybierz kliknij **Dodaj** > **Dodawanie plików...** . Teraz, przejdź do projektu iOS **zawartości** folderu. Gdy pojawi się pytanie o sposobie dodawania pliku, wybierz **dodać łącze** opcji:

![](part2-images/addalink.png "Gdy pojawi się pytanie o tym, jak dodać plik, wybierz opcję Dodaj link — opcja")

Omówiono Dlaczego miała plików mają zostać dodane do obu tych projektów poniżej. Każdy projekt **zawartości** zawierają teraz folderu **paddle.png** pliku:

![](part2-images/image3.png "Folder zawartości każdego projektu zawierają teraz plik paddle.png")

Jeśli firma Microsoft uruchomienia gry zajmiemy się naszego `CCSprite` rysowania:

![](part2-images/image4.png "Jeśli uruchomienia gry CCSprite będzie rysowany")


## <a name="file-details"></a>Szczegóły pliku

Do tej pory dodaliśmy pojedynczy plik do projektu, a proces był dość proste. Po prostu dodane **paddle.png** pliku na naszych **zawartości** folderów i odwołuje się do kodu. Załóżmy Poświęć chwilę, aby przyjrzeć się pewne kwestie podczas pracy z plikami w CocosSharp.

### <a name="capitalization"></a>Wielkość liter

Powiadomienie, że nazwa pliku i ciąg użyliśmy w kodzie dostępu do tego pliku są małymi literami. Jest to spowodowane niektóre platformy (na przykład Windows simulator pulpitu i iOS) są bez uwzględniania wielkości liter, podczas gdy inne platformy (na przykład urządzenie Android i iOS) jest uwzględniana wielkość liter. Używamy wszystkie małe pliki w pozostałej części tego samouczka, dzięki czemu naszych pliki i kod one między platformami jak to możliwe.

### <a name="extensions"></a>Rozszerzenia

Podczas odwoływania się do pliku paletkę Konstruktor do tworzenia ikonki nie zawiera rozszerzenia "PNG". Rozszerzenia plików zazwyczaj zostanie pominięty podczas pracy nad projektami CocosSharp jako rozszerzenia plików dla tego samego typu zasobów może różnić się między platformami. Na przykład pliki audio mogą być .aiff, MP3 lub .wma formatów plików, w zależności od platformy. Pozostawienie wyłączyć rozszerzenie umożliwia sam kod do działania na wszystkich platformach niezależnie od tego rozszerzenia pliku.

### <a name="content-in-platform-specific-projects"></a>Zawartości w projektach specyficzne dla platformy

W przeciwieństwie do większości plików naszego kodu, które mogą znajdować się w PCL, zawartości plików musi zostać dodany do każdego projektu specyficzne dla platformy. CocosSharp wymaga to dwóch powodów:

1. Dotyczy wszystkich platform różnych **akcji**. Zawartość dodana do projektów dla systemu iOS, należy użyć **BundleResource** akcji kompilacji. Zawartość dodana do projektów dla systemu Android, należy użyć **AndroidAsset** akcji kompilacji.
2. Niektóre platformy wymagają formatów plików specyficzne dla platformy. Na przykład pliki .xnb czcionki różnią się między systemów iOS i Android, jak zajmiemy się w dalszej części tego przewodnika.

Jeśli format pliku nie różni się od platformy (na przykład .png), każdej platformy można użyć tego samego pliku, w którym może być połączony pliku z tej samej lokalizacji co najmniej jedną platformę.

# <a name="orientation"></a>Orientacja

Podobnie jak każda inna aplikacja CocosSharp aplikacje można uruchomić w orientacji pionowej lub poziomej. Firma Microsoft będzie dopasować naszym gry do uruchamiania w trybie tylko do orientacji pionowej. Po pierwsze zmienimy kodu rozwiązania w naszym gry do obsługi współczynnik proporcji orientacji pionowej. Aby to zrobić, należy zmodyfikować `width` i `height` wartości w `LoadGame` metody w `ViewController` klasy w systemie iOS i `MainActivity` klasy w systemie Android:


```csharp
void LoadGame (object sender, EventArgs e)
{
    CCGameView gameView = sender as CCGameView;

    if (gameView != null)
    {
        var contentSearchPaths = new List<string> () { "Fonts", "Sounds" };
        CCSizeI viewSize = gameView.ViewSize;

        int width = 768;
        int height = 1027;
    ...
```

Musisz zmodyfikować każdy projekt specyficzne dla platformy, do uruchamiania w trybie portret dalej.


## <a name="ios-orientation"></a>iOS orientacji

Aby zmodyfikować orientacji projekt iOS, wybierz opcję **Info.plist** w pliku **BouncingGame.iOS** projektu, a następnie zmień **iPhone/iPod informacji o wdrożeniu** i **iPad informacji o wdrożeniu** jedynie do orientacji pionowej:

![](part2-images/image5.png "Zmień iPhone/iPod informacji o wdrożeniu i iPad informacji o wdrożeniu jedynie do orientacji pionowej")


## <a name="android-orientation"></a>Orientacja systemu android

Aby zmodyfikować orientacji projekt systemu Android, otwórz plik MainActivity.cs w projekcie BouncingGame.Android. Definicja atrybutu działania należy zmodyfikować, aby określa orientacji pionowej w następujący sposób:


```csharp
[Activity (
    Label = "BouncingGame.Android",
    AlwaysRetainTaskState = true,
    Icon = "@drawable/icon",
    Theme = "@android:style/Theme.NoTitleBar",
    ScreenOrientation = ScreenOrientation.Portrait,
    LaunchMode = LaunchMode.SingleInstance,
    MainLauncher = true,
    ConfigurationChanges = ConfigChanges.Orientation | ConfigChanges.ScreenSize | ConfigChanges.Keyboard | ConfigChanges.KeyboardHidden)
]
public class MainActivity : Activity
{ 
...
```


# <a name="default-coordinate-system"></a>Domyślny układ współrzędnych

Naszego kodu, która tworzy wystąpienie `CCSprite`, ustawia `PositionX` i `PositionY` wartości do 100. Domyślnie, oznacza to, że `CCSprite` center będzie znajdować się na 100 pikseli górę i w prawo od lewej dolnej części ekranu. System współrzędnych może być modyfikowany przez dołączenie `CCCamera` do `CCLayer`. Firma Microsoft nie będzie działać z `CCCamera` w tym projekcie, ale więcej informacji na temat `CCCamera` znajdują się w [dokumentacja interfejsu API CocosSharp](https://developer.xamarin.com/api/type/CocosSharp.CCLayer/).

100 pikseli wymienione powyżej pikseli "gier" zamiast pikseli na sprzęcie. Oznacza to, że uruchamianie tej samej gry na urządzeniu z innego rozwiązania (takich jak iPad i iPhone) spowoduje obiektów jest ustawiony i mały rozmiar względem ekranu fizycznego. W szczególności widocznym obszarze naszych gry będą zawsze miały wartość 1024 pikseli wysokości i szerokości 768 pikseli, jak jest to rozwiązanie możemy wybranymi wcześniej w `LoadGame` metody.


# <a name="adding-the-ball-ccsprite"></a>Dodawanie CCSprite piłka

Więc już znasz podstawowe informacje dotyczące pracy z `CCSprite`, dodamy naszych sekundę `CCSprite` — piłka. Firma Microsoft będzie następujące kroki, które są bardzo podobne do jak utworzyliśmy paletki `CCSprite`. 

Najpierw dodamy **ball.png** obrazu z folderu rozpakowane do naszej projektu iOS **zawartości** folderu. Zaznacz, aby **kopiowania** pliku do naszej **zawartości** katalogu. Wykonaj te same czynności co powyżej, aby dodać łącze do **ball.png** plik w projekcie systemu Android.

Następnie utworzyć `CCSprite` tego piłka, dodając element członkowski o nazwie `ballSprite` do naszej `GameScene` klasy, a także kod wystąpienia `ballSprite`. Po zakończeniu naszych `GameLayer` klasy będzie wyglądać następująco:


```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
    }
```


# <a name="adding-the-score-cclabel"></a>Dodawanie CCLabel wynik

Jest ostatnim elementem visual dodamy do naszej gry `CCLabel` do wyświetlenia liczba użytkownik został pomyślnie zwrócenia piłkę. `CCLabel` Używa czcionki określonym w Konstruktorze do ciągów wyświetlania na ekranie.

Dodaj następujący kod, aby utworzyć `CCLabel` wystąpienia w `GameLayer`. Raz Zakończono naszego kodu powinna wyglądać następująco:


```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    CCLabel scoreLabel;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "Arial", 20, CCLabelFormat.SystemFont);
        scoreLabel.PositionX = 50;
        scoreLabel.PositionY = 1000;
        scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
        AddChild (scoreLabel);
    }
    ...
```

Należy zauważyć, że używa scoreLabel `AnchorPoint` z `CCPoint.AnchorUpperLeft`, co oznacza, że `PositionX` i `PositionY` wartości definiują położenia lewego górnego. Pozwala to uzyskać pozycji `scoreLabel` względem górnym rogu ekranu, bez konieczności określania wymiary etykiety.


# <a name="implementing-every-frame-logic"></a>Implementowanie logiki co ramki

Do tej pory naszych gry przedstawia statycznych sceny. Firma Microsoft będzie Dodawanie logiki kontroli przemieszczania obiektów w naszym sceny przez dodanie kodu, która aktualizuje pozycji naszych obiektów w wysokiej częstotliwości. W takim przypadku naszego kodu zostanie uruchomiony sześćdziesiąt razy na sekundę - również nazywany sixty *ramki* na sekundę (chyba, że sprzęt nie obsługuje aktualizacji to często). W szczególności które będą dodawane logiki, aby piłka podlegają i odbijanie przed paletki, aby przenieść paletkę zgodnie z danych wejściowych oraz do aktualizowania wyniku za każdym razem, gdy piłkę uderzenia paletki.

`Schedule` Metodę, która jest dostarczana przez `CCNode` klasy, umożliwia nam dodanie logiki co ramki do naszej gry. Dodamy kod po `// New code` do konstruktora GameLayer:


```csharp
public GameLayer () : base (CCColor4B.Black)
{
    // "paddle" refers to the paddle.png image
    paddleSprite = new CCSprite ("paddle");
    paddleSprite.PositionX = 100;
    paddleSprite.PositionY = 100;
    AddChild (paddleSprite);
    ballSprite = new CCSprite ("ball");
    ballSprite.PositionX = 320;
    ballSprite.PositionY = 600;
    AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "arial", 22, CCLabelFormat.SpriteFont);
    scoreLabel.PositionX = 50;
    scoreLabel.PositionY = 1000;
    scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
    AddChild (scoreLabel);
    // New code:
    Schedule (RunGameLogic);
}
```

Teraz utworzyć `RunGameLogic` metody w naszym `GameLayer` klasy, która będzie zawierać wszystkie logiki co ramki:


```csharp
void RunGameLogic(float frameTimeInSeconds)
{
}
```

Parametr typu float Określa, jak długo trwa naszych ramki, w sekundach. Będziemy używać tej wartości podczas implementowania piłka przepływu.


## <a name="making-the-ball-fall"></a>Tworzenie spadek piłka

Firma Microsoft może wprowadzać piłka dzielą zaimplementowanie albo ręcznie swój własny kod grawitacji lub za pomocą wbudowanej funkcji Box2D w CocosSharp. Aparat symulacji Box2D fizyczny jest dostępna do gier CocosSharp. Jest bardzo wydajny i efektywny, ale wymaga pisanie kodu konfiguracji. Ponieważ naszych symulacji fizycznych jest dość proste, zostanie on ręcznie zaimplementować.

Aby zaimplementować grawitacji potrzebujemy sklepie bieżącym X i Y szybkie naszych piłka. Dodamy dwa elementy członkowskie do naszej `GameLayer` klasy:


```csharp
float ballXVelocity;
float ballYVelocity;
// How much to modify the ball's y velocity per second:
const float gravity = 140;
```

Następnie można wdrożyć logikę objęte `RunGameLogic`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
}
```

## <a name="moving-the-paddle-with-touch-input"></a>Przenoszenie paletkę z wprowadzaniem dotykowym

Teraz, objętych piłkę dodamy poziomy ruchu do naszej paletkę przy użyciu `CCEventListenerTouchAllAtOnce` obiektu. Ten obiekt zawiera wiele zdarzeń związanych z danych wejściowych. W tym przypadku chcemy otrzymasz powiadomienie, jeśli wszystkie punkty touch Przenieś tak, aby firma Microsoft może dostosować położenie paletki. `GameLayer` Już tworzy `CCEventListenerTouchAllAtOnce`, więc należy przypisać `OnTouchesMoved` delegowanie. Aby to zrobić, zmodyfikuj metodę AddedToScene w następujący sposób:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();
    // Use the bounds to layout the positioning of our drawable assets
    CCRect bounds = VisibleBoundsWorldspace;
    // Register for touch events
    var touchListener = new CCEventListenerTouchAllAtOnce ();
    touchListener.OnTouchesEnded = OnTouchesEnded;
    // new code:
    touchListener.OnTouchesMoved = HandleTouchesMoved;
    AddEventListener (touchListener, this);
}
```

Następnie będzie wdrożymy `HandleTouchesMoved`:


```csharp
void HandleTouchesMoved (System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    // we only care about the first touch:
    var locationOnScreen = touches [0].Location;
    paddleSprite.PositionX = locationOnScreen.X;
}
```


## <a name="implementing-ball-collision"></a>Implementowanie piłka kolizji

Jeśli teraz możemy zauważyć, że piłkę znajduje się za pośrednictwem paletki przeprowadzana gry. Firma Microsoft będzie zaimplementować *kolizji* (logika reagowanie na nakładających się obiektów gier) w naszym kodzie co ramki. Ponieważ zmiany obiekty przenoszenia umieszcza każdej ramki, sprawdzanie kolizji jest zwykle również wykonywane co ramki. Które będą również dodawane prędkość na osi X piłkę trafienia paletka, aby dodać niektóre żądania do gry, ale oznacza to, że należy zachować piłkę przenoszenia poza krawędzi ekranu. Firma Microsoft będzie to zrobić w `RunGameLogic` kod po zastosowaniu prędkość do `ballSprite` po `// New Code`:


```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
    // New Code:
    // Check if the two CCSprites overlap...
    bool doesBallOverlapPaddle = ballSprite.BoundingBoxTransformedToParent.IntersectsRect(
        paddleSprite.BoundingBoxTransformedToParent);
    // ... and if the ball is moving downward.
    bool isMovingDownward = ballYVelocity < 0;
    if (doesBallOverlapPaddle && isMovingDownward)
    {
        // First let's invert the velocity:
        ballYVelocity *= -1;
        // Then let's assign a random value to the ball's x velocity:
        const float minXVelocity = -300;
        const float maxXVelocity = 300;
        ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    }
    // First let’s get the ball position:   
    float ballRight = ballSprite.BoundingBoxTransformedToParent.MaxX;
    float ballLeft = ballSprite.BoundingBoxTransformedToParent.MinX;
    // Then let’s get the screen edges
    float screenRight = VisibleBoundsWorldspace.MaxX;
    float screenLeft = VisibleBoundsWorldspace.MinX;
    // Check if the ball is either too far to the right or left:    
    bool shouldReflectXVelocity = 
        (ballRight > screenRight && ballXVelocity > 0) ||
        (ballLeft < screenLeft && ballXVelocity < 0);
    if (shouldReflectXVelocity)
    {
        ballXVelocity *= -1;
    }
}
```


## <a name="adding-scoring"></a>Dodawanie oceniania

Teraz, naszych gry jest rozgrywane, ostatnim krokiem jest dodanie logiki oceniania. Najpierw dodamy członka wynik do klasy GameLayer o nazwie `score`:


```csharp
int score;
```

Ta zmienna zostanie użyta do śledzenia wyniku i wyświetl ją za pomocą naszych `scoreLabel`. Aby zrobić to dodaj następujący kod wewnątrz instrukcji if w `RunGameLogic` po Piłka i paletkę nakłada się na:


```csharp
...
if (doesBallOverlapPaddle && isMovingDownward)
{
    // First let's invert the velocity:
    ballYVelocity *= -1;
    // Then let's assign a random to the ball's x velocity:
    const float minXVelocity = -300;
    const float maxXVelocity = 300;
    ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    // New code:
    score++;
    scoreLabel.Text = "Score: " + score;
}
...
```

Teraz możemy uruchomienia gry i zobacz, czy naszych gry wyświetla wynik, który zwiększa się, jak piłkę odrzuceń wylogowuje paletki:

![](part2-images/image1.png "Uruchom gry i zobacz, czy wyświetla wynik, który zwiększa się, jak piłkę odrzuceń wylogowuje paletki")


# <a name="summary"></a>Podsumowanie

W tym przewodniku przedstawione tworzenia gier i platform grafiki, fizycznych i przy użyciu CocosSharp danych wejściowych. Jest pierwszym etapem wprowadzenie do tworzenia gier CocosSharp. Analizujemy niektóre z najczęściej klas w CocosSharp, jak utworzyć drzewa wizualnego, więc obiektów renderowane poprawnie i implementowania co ramka gry logiki.

W tym przewodniku opisano tylko niewielką częścią oferuje aparat gier CocosSharp. Aby uzyskać informacje i wskazówki dotyczące innych kwestii CocosSharp, zobacz [reszty nasze wskazówki CocosSharp](~/graphics-games/cocossharp/index.md).

## <a name="related-links"></a>Linki pokrewne

- [Ukończono gry (przykład)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [Zawartości (przykład)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
