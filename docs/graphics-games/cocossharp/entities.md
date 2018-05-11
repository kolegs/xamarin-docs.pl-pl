---
title: Jednostki w CocosSharp
description: Wzorzec jednostki to wydajny sposób organizowania gier kodu. Poprawia czytelność, sprawia, że kod jest łatwiejsze w obsłudze i korzysta z funkcji wbudowanych nadrzędny/podrzędny.
ms.prod: xamarin
ms.assetid: 1D3261CE-AC96-4296-8A53-A76A42B927A8
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 58a8d4e6fcb8a2165fafad74a5c59481d1550351
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="entities-in-cocossharp"></a>Jednostki w CocosSharp

_Wzorzec jednostki to wydajny sposób organizowania gier kodu. Poprawia czytelność, sprawia, że kod jest łatwiejsze w obsłudze i korzysta z funkcji wbudowanych nadrzędny/podrzędny._

Wzorzec jednostki można zwiększyć wysiłków dewelopera z CocosSharp za pośrednictwem organizacji lepszą kodu. W tym przewodniku będzie jest praktyczne przykład pokazujący sposób tworzenia dwie jednostki — jednostka wysyłki i jednostki punktor. Te jednostki zostaną własnym zawarte obiekty, co oznacza, że raz wystąpienia automatycznie będzie renderowany i wykona logiki przenoszenia odpowiednio do ich typu. 

Ten przewodnik obejmuje następujące tematy:

 - Wprowadzenie do gier jednostek
 - Ogólne a jednostki określonych typów
 - Ustawienia projektu
 - Tworzenie klas jednostki
 - Dodawanie wystąpień jednostek `GameLayer`
 - Reagowanie na logikę jednostki `GameLayer`

Zakończono gry będzie wyglądać następująco:

![](entities-images/image1.png "Zakończono gry będzie wyglądać następująco")


## <a name="introduction-to-game-entities"></a>Wprowadzenie do gier jednostek

Gry jednostki są klasy, które definiują obiekty wymagające logiki renderowania, kolizję, fizycznych lub sztucznego analizy. Na szczęście jednostek w grę kodu odpowiada często koncepcyjnej obiektów w grę. Jeśli to PRAWDA, identyfikowanie jednostek w grę można łatwiej wykonywać. 

Na przykład miejsca motywem [zdobycia 'em zapasowej gry](http://en.wikipedia.org/wiki/Shoot_%27em_up) może obejmować następujące jednostki:

 - `PlayerShip`
 - `EnemyShip`
 - `PlayerBullet`
 - `EnemyBullet`
 - `CollectableItem`
 - `HUD`
 - `Environment`

Te jednostki są własnych klas w grze i każde wystąpienie wymagają instalacji żadnych poza wystąpienia.


## <a name="general-vs-specific-entity-types"></a>Ogólne a jednostki określonych typów

Jedno z pierwszym pytań przez deweloperów gier przy użyciu systemu jednostki, które muszą ponieść jest znacznie do uogólnienia ich jednostek. Specyficzny implementacje zdefiniowane klasy dla każdego typu jednostki, nawet jeżeli różnią się one kilka właściwości. Więcej ogólnych systemów łączenie grup jednostek w jedną klasę i zezwolić wystąpień do dostosowania.

Załóżmy na przykład firma Microsoft może gry miejsca, który definiuje następujące klasy:

 - `PlayerDogfighter`
 - `PlayerBomber`
 - `EnemyMissileShip`
 - `EnemyLaserShip`

Podejście uogólniony więcej byłoby Utwórz jedną klasę do odtwarzacza jest dostarczany i jednej klasy dla przeciwnika jest dostarczany, które może zostać skonfigurowany do obsługi statku różnych typów. Dostosowania mogą obejmować który obraz można załadować typu jednostek punktor można utworzyć podczas robienia zdjęć współczynników przepływu i logikę AI wroga jest dostarczany. W takim przypadku na liście obiektów można zmniejszyć do:

 - `PlayerShip`
 - `EnemyShip`

Oczywiście tych typów jednostek można dodatkowo uogólniony zezwalając dostosowywania poszczególnych wystąpień przepływu sterowania. Player statku wystąpień będzie odczytywać dane wejściowe podczas wystąpień przeciwnika statku może wykonywać logiki AI. Oznacza to, że może być uogólniony jednostek nawet dalsze do jednej klasy:

 - `Ship`

Generalizacji można nadal jeszcze bardziej — gry może używać pojedyncza klasa podstawowa dla wszystkich obiektów. Ta jednej klasy, która może zostać wywołana `GameEntity`, będzie użyta dla każdego wystąpienia jednostki w grze całego klasa, łącznie z jednostkami, które nie są dostarczany takie jak punktory i elementy kolekcjonowanych (ulepszeń).

Ogólne większość systemów jest to często określane jako *składnik systemu*. W takich systemów gier jednostek może mieć pojedynczych składników, takich jak fizycznych, AI, lub renderowania składniki dodawane w celu dostosowania zachowania i wyglądu. Czysty systemów opartych na procesorze składnika ultimate i elastyczność i uniknąć problemy spowodowane użyciem dziedziczenia, takich jak łańcuchów złożonych dziedziczenia. Podobnie jak w przypadku innych generalizacji skutecznych składników może być trudne do skonfigurowania i może zmniejszyć wyrazistość kodu.

Poziom generalizacji używane zależy od wielu zagadnienia, w tym:

 - Rozmiar gier — można utworzyć określonej klasy, podczas gry większych może być trudne do zarządzania z dużą liczbą klas akceptowalny gry mniejsze.
 - Programowanie — opartych na danych gry, które są oparte na danych (obrazów, modeli 3D i plików danych, takich jak JSON i XML) mogą korzystać z o ogólnych typów jednostek i konfigurowanie szczegółowe informacje na temat na podstawie danych. Jest to szczególnie importu dla gry, w których jest oczekiwany, aby dodać nową zawartość, podczas tworzenia lub gry został zwolniony.
 - Wzorce gier aparat — niektóre gier aparaty zdecydowanie zaleca użycie składników a inne umożliwiają deweloperom decyzję dotyczącą sposobu organizowania jednostek. CocosSharp nie wymaga użycia składnika systemu, więc deweloperzy mogą implementować dowolnego typu jednostki. 

Dla uproszczenia będziemy używać określonego rozwiązania opartego na klasach z pojedynczej jednostki wysyłki i punktor w tym samouczku.


## <a name="project-setup"></a>Ustawienia projektu

Zanim zaczniemy implementacja naszej jednostek, należy utworzyć projekt. Szablony projektów CocosSharp będzie używany w celu uproszczenia procesu tworzenia projektu. [Sprawdź ten wpis](http://forums.xamarin.com/discussion/26822/cocossharp-project-templates-for-xamarin-studio) informacje dotyczące tworzenia projektu CocosSharp z programu Visual Studio for Mac szablonów. W dalszej części tego podręcznika użyje nazwy projektu **EntityProject**.

Po utworzeniu projektu naszych zaplanujemy rozdzielczość naszych gry do uruchomienia na 480 x 320. Aby to zrobić, należy wywołać `CCScene.SetDefaultDesignResolution` w `GameAppDelegate.ApplicationDidFinishLaunching` metody w następujący sposób:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...

    // New code for resolution setting:
    CCScene.SetDefaultDesignResolution(480, 320, CCSceneResolutionPolicy.ShowAll);
    
    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);
    mainWindow.RunWithScene (scene);
} 
```

Aby uzyskać więcej informacji o obsłudze CocosSharp rozwiązania, zobacz nasze [Przewodnik po obsługi wielu rozwiązań w CocosSharp](~/graphics-games/cocossharp/resolutions.md).


## <a name="adding-content-to-the-project"></a>Dodawanie zawartości do projektu

Po utworzeniu projektu naszych dodamy plików znajdujących się w [ten plik zip zawartości](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true). Aby to zrobić, Pobierz plik zip i Rozpakuj go. Dodaj **ship.png** i **bullet.png** do **zawartości** folderu. **Zawartości** folder będzie znajdować się wewnątrz **zasoby** folder w systemie Android i będzie w katalogu głównym projektu w systemie iOS. Po dodaniu powinien przedstawiono oba pliki w **zawartości** folderu:

![](entities-images/image2.png "Po dodaniu oba pliki powinny się znajdować w folderze zawartości")


## <a name="creating-the-ship-entity"></a>Tworzenie jednostki wysyłki

`Ship` Klasa będzie naszych gry pierwszy jednostki. Aby dodać `Ship` klasy, najpierw utwórz folder o nazwie **jednostek** na poziomie głównym projektu. Dodaj nową klasę w **jednostek** folder o nazwie `Ship`:

![](entities-images/image3.png "Dodaj nową klasę w folderze jednostek wysyłki")

Pierwsza zmiana wybierzemy do naszej `Ship` klasy jest umożliwienie on dziedziczyć `CCNode` klasy. `CCNode` Służy jako klasa podstawowa dla klasy CocosSharp wspólnych, takich jak `CCSprite` i `CCLayer`i udostępnia następujące funkcje:

 - `Position` Właściwość przenoszenia statku po ekranie.
 - `Children` Dodawanie właściwości `CCSprite.`
 - `Parent` Właściwość, która może służyć do podłączenia `Ship` wystąpienia inny `CCNodes`. Firma Microsoft nie będzie używać tej funkcji w tym samouczku; gry większych często zalet dołączanie jednostek do innych `CCNodes`. 
 - `AddEventListener` Metoda odpowiada jako danych wejściowych do przenoszenia statku.
 - `Schedule` Metoda premii punktory.

Również dodamy `CCSprite` , aby nasze statku są widoczne na ekranie:


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Ship : CCNode
    {
        CCSprite sprite;

        public Ship () : base()
        {
            sprite = new CCSprite ("ship.png");
            // Center the Sprite in this entity to simplify
            // centering the Ship when it is instantiated
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);
        }
    }
}
```

Następnie dodamy dostawy do naszej `GameLayer` wyświetlić je w naszym gry:


```csharp
public class GameLayer : CCLayer
{
    Ship ship;

    public GameLayer ()
    {
        ship = new Ship ();
        ship.PositionX = 240;
        ship.PositionY = 50;
        this.AddChild (ship);
    } 
    ...
```

Jeśli przeprowadzana naszych gry teraz zostanie wyświetlone naszych jednostki statku:

![](entities-images/image4.png "Podczas uruchamiania gry, zostaną wyświetlone jednostki wysyłki")


### <a name="why-inherit-from-ccnode-instead-of-ccsprite"></a>Dlaczego dziedziczyć CCNode zamiast CCSprite?

W tym momencie nasze `Ship` klasy jest proste otoki dla `CCSprite` wystąpienia. Ponieważ `CCSprite` również dziedziczy `CCNode`, firma Microsoft może mieć odziedziczone bezpośrednio `CCSprite`, która będzie ograniczona kod w `Ship.cs`. Ponadto dziedziczenie bezpośrednio z `CCSprite` zmniejsza liczbę obiektów w pamięci i może poprawić wydajność dzięki mniejszych drzewo zależności.

Pomimo tych korzyści, możemy odziedziczone `CCNode` można ukryć część `CCSprite` właściwości z każdego wystąpienia. Na przykład `Texture` nie można zmodyfikować właściwości poza `Ship` klasy, a dziedziczących `CCNode` pozwala ukryć tej właściwości. Publiczne elementy członkowskie naszych jednostek stają się szczególnie ważne, jak zwiększa się gier oraz jak deweloperzy dodatkowe są dodawane do zespołu.


## <a name="adding-input-to-the-ship"></a>Dodawanie danych wejściowych do wydania

Teraz, naszych statku jest widoczny na ekranie dodamy danych wejściowych. Nasze podejście będą wyglądać podobnie do podejście przyjęte [przewodnik BouncingGame](~/graphics-games/cocossharp/bouncing-game.md), z wyjątkiem tego, że firma Microsoft będzie wprowadzenie kodu dla ruchu w `Ship` klasy, a nie w zawierający `CCLayer` lub `CCScene`.

Dodaj kod, aby `Ship` do obsługi przeniesienie jej do wszędzie tam, gdzie użytkownik zachodzi ekranu:


```csharp
public class Ship : CCNode
{
    CCSprite sprite;

    CCEventListenerTouchAllAtOnce touchListener;

    public Ship () : base()
    {
        sprite = new CCSprite ("ship.png");
        // Center the Sprite in this entity to simplify
        // centering the Ship on screen
        sprite.AnchorPoint = CCPoint.AnchorMiddle;
        this.AddChild(sprite);

        touchListener = new CCEventListenerTouchAllAtOnce();
        touchListener.OnTouchesMoved = HandleInput;
        AddEventListener(touchListener, this);

    }

    private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
    {
        if(touches.Count > 0)
        {
            CCTouch firstTouch = touches[0];

            this.PositionX = firstTouch.Location.X;
            this.PositionY = firstTouch.Location.Y;
        } 
    }
} 
```

Wiele zdobycia 'em się wdrożenie gry maksymalna szybkość pracy mimicking tradycyjnych przepływu oparte na kontrolerze. Inaczej mówiąc, po prostu będzie wprowadzania natychmiastowego ruchu do zachowania naszego kodu krótszy.


## <a name="creating-the-bullet-entity"></a>Tworzenie jednostki punktor

Drugi jednostki w naszym proste gry jest jednostką wyświetlania punktory. Podobnie jak `Ship` jednostki, `Bullet` jednostka będzie zawierać `CCSprite` tak, aby na ekranie. Logikę przepływu różni się, że nie jest zależny od danych wejściowych użytkownika dla przepływu; zamiast `Bullet` wystąpień zostanie przesunięty w prostym przy użyciu właściwości szybkość pracy.

Najpierw dodamy plik klasy do naszej **jednostek** folder i nadaj mu **punktor**:

![](entities-images/image5.png "Dodaj nowy plik klasy do folderu jednostek i wywołać go punktor")

Po dodaniu, firma Microsoft będzie zmodyfikować `Bullet.cs` kodu w następujący sposób:


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Bullet : CCNode
    {
        CCSprite sprite;

        public float VelocityX
        {
            get;
            set;
        }

        public float VelocityY
        {
            get;
            set;
        }

        public Bullet () : base()
        {
            sprite = new CCSprite ("bullet.png");
            // Making the Sprite be centered makes
            // positioning easier.
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);

            this.Schedule (ApplyVelocity);
        }

        void ApplyVelocity(float time)
        {
            PositionX += VelocityX * time;
            PositionY += VelocityY * time;
        }
    }
} 
```

Jako uzupełnienie Zmienianie pliku używany do `CCSprite` do `bullet.png`, kod w `ApplyVelocity` zawiera logikę przepływu, która opiera się na dwóch współczynników: `VelocityX` i `VelocityY`.

`Schedule` Metoda umożliwia dodawanie delegatów wywoływanie co ramki. W takim przypadku Trwa dodawanie `ApplyVelocity` metody, aby nasze punktor przenosi zgodnie z jego wartości szybkość pracy. `Schedule` Ma metodę `Action<float>`, gdzie parametr float określa ilość czasu (w sekundach) od czasu ostatniego ramki, który używamy w celu wdrożenia na podstawie czasu przepływu. Od czasu wartość jest mierzony w sekundach, a następnie naszych wartości prędkość reprezentują ruch w *pikseli na sekundę*.


## <a name="adding-bullets-to-gamelayer"></a>Dodawanie do GameLayer punktorów

Zanim dodamy żadnego `Bullet` wystąpień do naszej gry firma Microsoft będzie wprowadzać kontener, w szczególności `List<Bullet>`. Modyfikowanie `GameLayer` , uwzględniając listę punktorów:


```csharp
    public class GameLayer : CCLayer
    {
        Ship ship;
        List<Bullet> bullets;

        public GameLayer ()
        {
            ship = new Ship ();
            ship.PositionX = 240;
            ship.PositionY = 50;
            this.AddChild (ship);

            bullets = new List<Bullet> ();
        }
        ... 
```

Następnie musisz wypełnić `Bullet` listy. Kiedy tworzyć logikę `Bullet` powinny być zawarte w `Ship` jednostki, ale `GameLayer` jest odpowiedzialny za zapisywanie listy punktory. Używamy wzorzec fabryki umożliwia `Ship` jednostki do utworzenia `Bullet` wystąpień. Ta fabryka uwidoczni zdarzenie który `GameLayer` może obsłużyć. 

W tym celu najpierw dodamy folder do naszej projektu o nazwie **fabryki**, a następnie dodaj nową klasę o nazwie `BulletFactory`:

![](entities-images/image6.png "Dodaj folder do projektu o nazwie fabryki, a następnie dodaj nową klasę o nazwie BulletFactory")

Następnie będzie wdrożymy `BulletFactory` klasa pojedyncza:


```csharp
using System;

namespace EntityProject
{
    public class BulletFactory
    {
        static Lazy<BulletFactory> self = 
            new Lazy<BulletFactory>(()=>new BulletFactory());

        // simple singleton implementation
        public static BulletFactory Self
        {
            get
            {
                return self.Value;
            }
        }

        public event Action<Bullet> BulletCreated;

        private BulletFactory()
        {

        }

        public Bullet CreateNew()
        {
            Bullet newBullet = new Bullet ();

            if (BulletCreated != null)
            {
                BulletCreated (newBullet);
            }

            return newBullet;
        }
    }
} 
```

`Ship` Jednostki obsługi tworzenia `Bullet` wystąpień — w szczególności będą obsługiwać częstotliwość `Bullet` można tworzyć wystąpień, (tj., jak często punktor jest uruchamiany), ich pozycji, a ich prędkość.

Modyfikowanie `Ship` jednostki konstruktora, aby dodać nowy `Schedule` wywołań, a następnie zaimplementować tę metodę w następujący sposób:


```csharp
...
public Ship () : base()
{
    sprite = new CCSprite ("ship.png");
    // Center the Sprite in this entity to simplify
    // centering the Ship on screen
    sprite.AnchorPoint = CCPoint.AnchorMiddle;
    this.AddChild(sprite);

    touchListener = new CCEventListenerTouchAllAtOnce();
    touchListener.OnTouchesMoved = HandleInput;
    AddEventListener(touchListener, this);

    Schedule (FireBullet, interval: 0.5f);

}

void FireBullet(float unusedValue)
{
    Bullet newBullet = BulletFactory.Self.CreateNew ();
    newBullet.Position = this.Position;
    newBullet.VelocityY = 100;
} 
...
```

Ostatnim krokiem jest obsługa tworzenia nowych `Bullet` wystąpień w `GameLayer` kodu. Dodawanie obsługi zdarzeń do `BulletCreated` zdarzeń, który dodaje nowo utworzony `Bullet` do odpowiednich wykazów:


```csharp
...
public GameLayer ()
{
    ship = new Ship ();
    ship.PositionX = 240;
    ship.PositionY = 50;
    this.AddChild (ship);

    bullets = new List<Bullet> ();
    BulletFactory.Self.BulletCreated += HandleBulletCreated;
}

void HandleBulletCreated(Bullet newBullet)
{
    AddChild (newBullet);
    bullets.Add (newBullet);
}
... 
```

Teraz możemy uruchomienia gry i zobacz `Ship` premia `Bullet` wystąpień:

![](entities-images/image1.png "Uruchom gry i statku będzie premia punktor wystąpień")


## <a name="why-gamelayer-has-ship-and-bullets-members"></a>Dlaczego GameLayer ma członków wysyłki i punktorów

Naszych `GameLayer` klasa definiuje dwa pola do przechowywania odwołań do naszej wystąpień jednostek (`ship` i `bullets`), ale żadne z nich. Ponadto jednostki są zobowiązani do ich własnych zachowanie, takie jak przepływu i premia. Dlaczego możemy dodać `ship` i `bullets` pól do `GameLayer`?

Jest przyczyna dodaliśmy tych elementów członkowskich, ponieważ implementacja pełnej gier wymaga logikę w `GameLayer` interakcji między różnymi jednostkami. Na przykład gry może dalej rozwijany do uwzględnienia wrogów, które mogą zostać zniszczone przez odtwarzacz. Te wrogów byłby zawarty w `List` w `GameLayer`i logiki, aby sprawdzić czy `Bullet` wystąpień kolidują z wrogów zostałoby przeprowadzone w `GameLayer` również. Innymi słowy `GameLayer` główny *właściciela* jednostki wszystkich wystąpień, a jest odpowiedzialny za interakcje między wystąpieniami jednostki.


## <a name="bullet-destruction-considerations"></a>Zagadnienia dotyczące zniszczenie punktor

Nasze gry obecnie nie ma kodu niszczenia `Bullet` wystąpień. Każdy `Bullet` wystąpienie ma logiki przenoszenia na ekranie, ale nie dodano żadnego kodu do zniszczenia żadnego ekranem `Bullet` wystąpień.

Ponadto zniszczenie `Bullet` wystąpień nie może należeć w `GameLayer`. Na przykład zamiast niszczony podczas ukrytej, `Bullet` jednostki może mieć logiki można zniszczyć się po pewnym czasie. W takim przypadku `Bullet` musi mieć możliwość komunikacji, że należy zniszczyć do `GameLayer`, podobne `Ship` przekazywane jednostki `GameLayer` który nowy `Bullet` został utworzony za pomocą `BulletFactory`.

Najprostszym rozwiązaniem jest rozwiń odpowiedzialność fabryki klasy do obsługi zniszczenia. Następnie fabryka powiadomienia mogą być wysyłane wystąpienia jednostki niszczone, które są obsługiwane przez inne obiekty, takie jak `GameLayer` usuwanie wystąpienia jednostki z jego list. 

## <a name="summary"></a>Podsumowanie

W tym przewodniku przedstawiono sposób tworzenia jednostek CocosSharp przez dziedziczenie z `CCNode` klasy. Te jednostki są obiektami niezależne, obsługa tworzenia własnych elementów wizualnych i niestandardowej logiki. Ten przewodnik określa kod, który należy wewnątrz jednostki (przepływu i tworzenia innych jednostek) z kodu, który należy do kontenera jednostek głównego (kolizji i logika interakcji innych jednostek).

## <a name="related-links"></a>Linki pokrewne

- [Dokumentacja interfejsu API CocosSharp](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Zip zawartości](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)
