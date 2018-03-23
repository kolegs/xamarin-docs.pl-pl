---
title: Szczegóły implementacji czasu monety
description: W tym przewodniku opisano szczegóły implementacji w grze monety czas, tym Praca z kafelków mapy, tworzenie jednostek animacji ikony i implementowanie kolizji wydajne.
ms.topic: article
ms.prod: xamarin
ms.assetid: 5D285684-0417-4E16-BD14-2D1F6DEFBB8B
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 80250ca9fae98fae653c9b2837b2b1a96fb02203
ms.sourcegitcommit: 7b76c3d761b3ffb49541e2e2bcf292de6587c4e7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="coin-time-implementation-details"></a>Szczegóły implementacji czasu monety

_W tym przewodniku opisano szczegóły implementacji w grze monety czas, tym Praca z kafelków mapy, tworzenie jednostek animacji ikony i implementowanie kolizji wydajne._

Czas monety jest pełna platformera gier dla systemu iOS i Android. Celem gry jest zbieranie wszystkich monet na poziomie i następnie osiągnąć drzwi zakończenia unikając wrogów i wąskich gardeł.

![](cointime-images/image1.png "Celem gry jest zbieranie wszystkich monet na poziomie i następnie osiągnąć drzwi zakończenia unikając wrogów i wąskich gardeł")

W tym przewodniku omówiono szczegóły implementacji w czasie monety, obejmujące następujące tematy:

- [Praca z plikami TMX](#Working_with_TMX_Files)
- [Poziom ładowania](#Level_Loading)
- [Dodawanie nowych jednostek](#Adding_New_Entities)
- [Animowany jednostek](#Animated_Entities)


# <a name="content-in-coin-time"></a>W czasie monety zawartości

Czas monety jest przykładowy projekt, reprezentujący może organizowania pełne projektu CocosSharp. Monety czasu struktury ma na celu uproszczenie Dodawanie i Konserwacja zawartości. Używa **.tmx** pliki tworzone przez [sąsiadująco](http://www.mapeditor.org) dla poziomów i pliki XML do definiowania animacji. Modyfikowanie lub Dodawanie nowej zawartości można osiągnąć przy minimalnym wysiłku. 

Podczas tego podejścia sprawia, że czas monety skuteczne projektu do nauki i eksperymenty, również odzwierciedla jak professional gry zostały wprowadzone. W tym przewodniku omówiono podejścia podjąć w celu uproszczenia Dodawanie i modyfikowanie zawartości.


# <a name="working-with-tmx-files"></a>Praca z plikami TMX

Poziomów czasu monety są zdefiniowane przy użyciu formatu pliku .tmx, który jest wynikiem [sąsiadująco](http://www.mapeditor.org) kafelków mapy edytora. Szczegółowe omówienie pracy z sąsiadująco, zobacz [przy użyciu rozmieszczany z przewodnik gwałtowny Cocos](~/graphics-games/cocossharp/tiled.md). 

Każdy poziom jest zdefiniowany w własnego zawarte w pliku .tmx **CoinTime/zasoby/zawartości/poziomy** folderu. Wszystkie poziomy czasu monety udostępniania jednego pliku tileset, który jest zdefiniowany w **mastersheet.tsx** pliku. Ten plik definiuje niestandardowe właściwości dla każdego kafelka, na przykład czy Kafelek zawiera stałe kolizji lub czy Kafelek powinna zostać zastąpiona wystąpienia jednostki. Plik mastersheet.tsx umożliwia właściwości, które mają zostać zdefiniowany tylko raz i używać na wszystkich poziomach. 


## <a name="editing-a-tile-map"></a>Edytowanie mapy kafelka

Aby edytować kafelków mapy, otwórz plik .tmx w sąsiadująco, klikając dwukrotnie plik .tmx lub otwierając go za pomocą menu Plik w sąsiadująco. Czas monety mapy poziomu kafelka zawiera trzy warstwy: 

- **Jednostki** — ta warstwa zawiera Kafelki, które zostaną zastąpione wystąpienia jednostek w czasie wykonywania. Przykładami odtwarzacza, monet wrogów i drzwi zakończenia z poziomu.
- **Terenu** — ta warstwa zawiera Kafelki, które zwykle mają kolizji stałe. Stałe kolizji umożliwia player przeprowadzenie na tych kafelkach bez objętych za pośrednictwem. Kafelki z stałe kolizji mogą również działać jako ściany i poziomów.
- **Tło** — warstwę tła zawiera Kafelki, używane jako statycznego tła. Ta warstwa nie jest przewijane po przemieszczeniu aparatu w poziomie, tworzenie wyglądu głębokość za pośrednictwem paralaksy.

Jak przeanalizujemy później, kod ładowania poziom oczekuje, że te trzy warstwy na wszystkich poziomach monety czasu.

### <a name="editing-terrain"></a>Edytowanie terenu
Kafelki można umieścić, klikając w **mastersheet** tileset, a następnie klikając pozycję na kafelku mapy. Na przykład do malowania nowe terenu na poziomie:

1. Wybierz warstwę terenu
1. Kliknij Kafelek do rysowania
1. Kliknij przycisk lub wypychania i przeciągnij mapy do malowania kafelka

    ![](cointime-images/image2.png "Kliknij Kafelek, aby narysować 1")

Od lewej górnej tileset zawiera wszystkie terenu w czasie monety. Obejmuje terenu, który jest pełny, **SolidCollision** właściwości, jak pokazano w właściwości kafelka po lewej stronie ekranu:

![](cointime-images/image3.png "Terenu, który jest pełny, zawiera właściwość SolidCollision opisane we właściwościach kafelka po lewej stronie ekranu")

### <a name="editing-entities"></a>Edytowanie obiektów
Jednostki można dodać lub usunąć z poziomu — podobnie jak terenu. **Mastersheet** tileset ma wszystkich jednostek umieszczone o połowie poziomie, dlatego mogą nie być widoczne bez przewijania w prawo:

![](cointime-images/image4.png "Mastersheet tileset ma wszystkich jednostek umieszczone o połowie poziomie, dlatego mogą nie być widoczne bez przewijania w prawo")

Nowe jednostki powinny być umieszczane na **jednostek** warstwy.

![](cointime-images/image5.png "Nowe jednostki powinny być umieszczane na warstwie jednostek")

Sprawdza kod CoinTime **EntityType** po załadowaniu do identyfikowania Kafelki, które powinny zostać zastąpione przez jednostki poziomu: 

![](cointime-images/image6.png "Kod CoinTime szuka EntityType po załadowaniu poziomu do identyfikowania Kafelki, które powinny zostać zastąpione przez jednostki")

Gdy plik ma zostały zmodyfikowane i zapisane, zmiany będą automatycznie widoczne czy projekt jest wbudowana i uruchom:

![](cointime-images/image7.png "Gdy plik ma zostały zmodyfikowane i zapisane, zmiany będą automatycznie widoczne czy projekt jest wbudowana i uruchom")


## <a name="adding-new-levels"></a>Dodawanie nowych poziomów

Proces dodawania poziomy na czas monety wymaga nie zmian w kodzie, a tylko kilka niewielkich zmian w projekcie. Aby dodać nowy poziom:

1. Otwórz folder poziomu znajdujący się w <**głównego CoinTime > \CoinTime\Assets\Content\levels**
1. Skopiuj i Wklej jeden z poziomów, takich jak **level0.tmx**
1. Zmień nazwę nowego pliku .tmx, takich jak nadal poziomu sekwencja z poziomami istniejących **level8.tmx**
1. W programie Visual Studio lub Visual Studio dla komputerów Mac należy dodać nowy plik .tmx do folderu Android poziomów. Sprawdź, czy plik używa **AndroidAsset** akcji kompilacji.

    ![](cointime-images/image8.png "Sprawdź, czy plik używa AndroidAsset Akcja kompilacji")

1. Dodaj nowy plik .tmx do folderu poziomy systemu iOS. Połącz plik z oryginalnej lokalizacji i sprawdź, czy używa **BundleResource** akcji kompilacji.

    ![](cointime-images/image9.png "Połącz plik z oryginalnej lokalizacji i sprawdź korzysta z BundleResource Akcja kompilacji")

Nowy poziom powinien pojawiać się na poziomie ekranu wybierz jako poziom 9 (poziom nazwy plików rozpoczynają się od 0, ale przyciski poziomu rozpocząć o numerze 1):

![](cointime-images/image10.png "Nowy poziom powinien pojawiać się na poziomie ekranu wybierz jako start nazwy pliku poziomu poziom 9 na 0, ale przyciski poziomu rozpocząć o numerze 1")


# <a name="level-loading"></a>Poziom ładowania

Jak pokazano wcześniej, nowe poziomy nie wymagają żadnych zmian w kodzie — gry automatycznie wykrywa poziomy, jeśli są one poprawnie o nazwie i dodane do **poziomy** folder z akcją kompilacji poprawne (**BundleResource**lub **AndroidAsset**).

Logika pozwalająca ustalić liczbę poziomów znajduje się w `LevelManager` klasy. Gdy wystąpienie klasy `LevelManager` jest tworzony (przy użyciu wzorca singleton) `DetermineAvailbleLevels` metoda jest wywoływana:


```csharp
private void DetermineAvailableLevels()
{
    // This game relies on levels being named "levelx.tmx" where x is an integer beginning with
    // 1. We have to rely on MonoGame's TitleContainer which doesn't give us a GetFiles method - we simply
    // have to check if a file exists, and if we get an exception on the call then we know the file doesn't
    // exist. 
    NumberOfLevels = 0;
    while (true)
    {
        bool fileExists = false;
        try
        {
            using(var stream = TitleContainer.OpenStream("Content/levels/level" + NumberOfLevels + ".tmx"))
            {
            }
            // if we got here then the file exists!
            fileExists = true;
        }
        catch
        {
            // do nothing, fileExists will remain false
        }
        if (!fileExists)
        {
            break;
        }
        else
        {
            NumberOfLevels++;
        }
    }
}
```

CocosSharp nie zawiera metody i platform do ustalania, czy pliki są obecne, a więc musimy polegać na `TitleContainer` klasę, aby podjąć próbę otwarcia strumienia. Jeśli kod otwarciem strumienia zgłasza wyjątek, a następnie plik istnieje i podziały pętli while. Po zakończeniu pętli `NumberOfLevels` właściwości raportów, ile poziomów prawidłowe są częścią projektu.

`LevelSelectScene` Klasy używa `LevelManager.NumberOfLevels` Aby ustalić, ile przyciski umożliwiające tworzenie w `CreateLevelButtons` metody:


```csharp
private void CreateLevelButtons()
{
    const int buttonsPerPage = 6;
    int levelIndex0Based = buttonsPerPage * pageNumber;
    int maxLevelExclusive = System.Math.Min (levelIndex0Based + 6, LevelManager.Self.NumberOfLevels);
    int buttonIndex = 0;
    float centerX = this.ContentSize.Center.X;
    const float topRowOffsetFromCenter = 16;
    float topRowY = this.ContentSize.Center.Y + topRowOffsetFromCenter;
    for (int i = levelIndex0Based; i < maxLevelExclusive; i++)
    {
        ...
    }
}
```

`NumberOflevels` Właściwość jest używana do określenia przyciski, które ma zostać utworzony. Ten kod uwzględnia stronę, która jest aktualnie wyświetlany i tworzy tylko maksymalnie sześć przycisków na stronie użytkownika. Po kliknięciu przycisku wystąpienia wywołania `HandleButtonClicked` metody:


```csharp
private void HandleButtonClicked(object sender, EventArgs args)
{
    // levelNumber is 1-based, so subtract 1:
    var levelIndex = (sender as Button).LevelNumber - 1;
    LevelManager.Self.CurrentLevel = levelIndex;
    CoinTime.GameAppDelegate.GoToGameScene ();
}
```

Ta metoda przypisuje `CurrentLevel` właściwość, która jest używana przez `GameScene` podczas ładowania poziomu. Po ustawieniu `CurrentLevel`, `GoToGameScene` jest wywoływane metody przełączania sceny z `LevelSelectScene` do `GameScene`.

`GameScene` Wywołania konstruktora `GoToLevel`, który wykonuje logiki ładowania poziomu:


```csharp
private void GoToLevel(int levelNumber)
{
    LoadLevel (levelNumber);
    CreateCollision();
    ProcessTileProperties ();
    touchScreen = new TouchScreenInput(gameplayLayer);
    secondsLeft = secondsPerLevel;
}
```

Następnie przeniesiemy przyjrzeć się metody wywołane `GoToLevel`.


## <a name="loadlevel"></a>LoadLevel

`LoadLevel` Metoda jest odpowiedzialna za ładowania pliku .tmx i dodać go do `GameScene`. Ta metoda nie powoduje żadnych interakcyjne obiekty, takie jak kolizji lub jednostek — po prostu tworzy wizualnych poziomu, nazywana także *środowiska*.


```csharp
private void LoadLevel(int levelNumber)
{
    currentLevel = new CCTileMap ("level" + levelNumber + ".tmx");
    currentLevel.Antialiased = false;
    backgroundLayer = currentLevel.LayerNamed ("Background");
    // CCTileMap is a CCLayer, so we'll just add it under all entities
    this.AddChild (currentLevel);
    // put the game layer after
    this.RemoveChild(gameplayLayer);
    this.AddChild(gameplayLayer);
    this.RemoveChild (hudLayer);
    this.AddChild (hudLayer);
}
```

`CCTileMap` Konstruktor pobiera nazwę pliku, w której jest tworzony przy użyciu numer poziomu przekazany do `LoadLevel`. Aby uzyskać więcej informacji na temat tworzenia i Praca z `CCTileMap` wystąpień, zobacz [przy użyciu rozmieszczany prowadnicy CocosSharp](~/graphics-games/cocossharp/tiled.md).

Obecnie CocosSharp nie zezwala na zmianę kolejności warstw bez usunięcia i ponownego dodania ich do ich nadrzędnej `CCScene` (czyli `GameScene` w takim przypadku), przez co kilka ostatnich wierszy metody są wymagane, aby zmienić kolejność warstw.


## <a name="createcollision"></a>CreateCollision

`CreateCollision` Konstrukcji — metoda `LevelCollision` wystąpienia, który służy do wykonywania *stałe kolizji* między player i środowiska.


```csharp
private void CreateCollision()
{
    levelCollision = new LevelCollision();
    levelCollision.PopulateFrom(currentLevel);
}
```

Bez tej konflikt odtwarzacz spowoduje przejście do poziomu i gry będą odtworzona. Stałe kolizji umożliwia player opisano na podstawie i uniemożliwia player Instruktaż ściany lub przejście się za pomocą poziomów.

Kolizji w czasie monety można dodać z żaden dodatkowy kod — tylko modyfikacje plików w układzie sąsiadującym. 


## <a name="processtileproperties"></a>ProcessTileProperties

Po załadowaniu poziomu i utworzeniu kolizji `ProcessTileProperties` jest wywoływana w celu wykonywania logiki oparte na kafelku właściwości. Godzina monety zawiera `PropertyLocation` struktury do definiowania właściwości oraz współrzędne fragmentu z następującymi właściwościami:


```csharp
public struct PropertyLocation
{
    public CCTileMapLayer Layer;
    public CCTileMapCoordinates TileCoordinates;
    public float WorldX;
    public float WorldY;
    public Dictionary<string, string> Properties;
}
```

Ta struktura jest używany do tworzenia utworzenia wystąpienia jednostki i usunąć niepotrzebne kafelków w `ProcessTileProperties` metody:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```

Każda właściwość kafelka, sprawdzania, czy klucz jest ocenia pętli foreach `EntityType` lub `RemoveMe`. `EntityType` Wskazuje, czy można utworzyć wystąpienia jednostki. `RemoveMe` Wskazuje kafelka powinny zostać całkowicie usunięte w czasie wykonywania.

Jeśli właściwość o `EntityType` klucz zostanie znaleziony, następnie `TryCreateEntity` jest nazywany podejmuje próbę, aby utworzyć obiekt przy użyciu właściwości dopasowywania `EntityType` klucza:


```csharp
private bool TryCreateEntity(string entityType, float worldX, float worldY)
{
    CCNode entityAsNode = null;
    switch (entityType)
    {
    case "Player":
        player = new Player ();
        entityAsNode = player;
        break;
    case "Coin":
        Coin coin = new Coin ();
        entityAsNode = coin;
        coins.Add (coin);
        break;
    case "Door":
        door = new Door ();
        entityAsNode = door;
        break;
    case "Spikes":
        var spikes = new Spikes ();
        this.damageDealers.Add (spikes);
        entityAsNode = spikes;
        break;
    case "Enemy":
        var enemy = new Enemy ();
        this.damageDealers.Add (enemy);
        this.enemies.Add (enemy);
        entityAsNode = enemy;
        break;
    }
    if(entityAsNode != null)
    {
        entityAsNode.PositionX = worldX;
        entityAsNode.PositionY = worldY;
        gameplayLayer.AddChild (entityAsNode);
    }
    return entityAsNode != null;
}
```


# <a name="adding-new-entities"></a>Dodawanie nowych jednostek

Czas monety korzysta ze wzorca jednostki dla swoich gier obiektów (opisano w temacie [jednostek CocosSharp przewodnik](~/graphics-games/cocossharp/entities.md)). Dziedzicz wszystkich jednostek `CCNode`, co oznacza, że mogą być dodawane jako elementy podrzędne `gameplayLayer`.

Poszczególnych typów jednostek, również jest przywoływany bezpośrednio za pomocą listy lub pojedynczego wystąpienia. Na przykład `Player` odwołuje się do niego `player` pola i wszystkie `Coin` odwołuje się wystąpienia `coins` listy. Utrzymywanie bezpośrednie odwołania do jednostki (a nie odwołuje się do nich za pośrednictwem `gameLayer.Children` listy) sprawia, że kod, który uzyskuje dostęp do tych jednostek czytelności i eliminuje rzutowanie typów potencjalnie kosztowne.

Istniejący kod udostępnia wiele typów jednostek jako przykłady sposobu tworzenia nowych jednostek. Poniższe kroki mogą służyć do utworzenia nowego obiektu:


## <a name="1---define-a-new-class-using-the-entity-pattern"></a>1 — Zdefiniuj nową klasę przy użyciu wzorca jednostki

Jedynym wymaganiem tworzenia jednostki polega na utworzeniu klasy, która dziedziczy `CCNode`. Większość mają niektóre visual, takich jak `CCSprite`, które powinny zostać dodane jako element podrzędny obiektu jego konstruktora.

Udostępnia CoinTime `AnimatedSpriteEntity` klasy, która ułatwia tworzenie jednostek animowany. Animacje zostanie omówiona bardziej szczegółowo w [sekcji animowany jednostek](#Animated_Entities).


## <a name="2--add-a-new-entry-to-the-trycreateentity-switch-statement"></a>2 — Dodaj nowy wpis do TryCreateEntity instrukcji switch

Wystąpienia jednostki nowego wystąpienia powinny być tworzone w `TryCreateEntity`. Jeśli jednostka wymaga logiki co ramki kolizji, AI lub odczytywania danych wejściowych, a następnie `GameScene` trzeba utrzymywać odwołania do obiektu. Jeśli potrzebne są wielu wystąpień (takich jak `Coin` lub `Enemy` wystąpień), następnie nowy `List` powinny zostać dodane do `GameScene` klasy.


## <a name="3--modify-tile-properties-for-the-new-entity"></a>3 — Modyfikuj właściwości kafelków dla nowego obiektu

Po kodzie obsługuje tworzenie nowych jednostek, Nowa jednostka musi zostać dodany do tileset. Tileset mogą być edytowane przez otwarcie dowolnym poziomie `.tmx` pliku. 

Tileset są przechowywane oddzielnie od .tmx w **mastersheet.tsx** pliku, więc należy zaimportować przed można edytować:

![](cointime-images/image11.png "Tileset są przechowywane oddzielnie od plików .tsx, należy zaimportować przed może być edytowany")

Po zaimportowaniu właściwości wybrane Kafelki są edytowalne i EntityType, do którego można dodać:

![](cointime-images/image12.png "Po zaimportowaniu właściwości wybrane Kafelki są edytowalne i EntityType, do którego można dodać")

Po utworzeniu właściwości, aby był zgodny z nowym można ustawić jej wartości `case` w `TryCreateEntity`:

![](cointime-images/image13.png "Po utworzeniu właściwości można ustawić jej wartości do dopasowania w przypadku nowych TryCreateEntity")

Po zmianie tileset musi być eksportowany — dzięki temu zmiany dostępne dla wszystkich innych poziomach:

![](cointime-images/image14.png "Po zmianie tileset musi być eksportowany")

Tileset czy zastąpić istniejące **mastersheet.tsx** tileset:

![](cointime-images/image15.png "HE tileset czy zastąpić istniejące tileset mastersheet.tsx")


# <a name="entity-tile-removal"></a>Usunięcie kafelka jednostki

Podczas ładowania kafelków mapy do gier, poszczególne Kafelki są statycznych obiektów. Ponieważ jednostek wymagane jest zachowanie niestandardowych, takich jak przepływ, kodu w czasie monety usuwa Kafelki podczas tworzenia jednostek.

`ProcessTileProperties` zawiera logikę do usunięcia Kafelki, co spowoduje utworzenie jednostek przy użyciu `RemoveTile` metody:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            ...
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        ...
    }
}
```

Automatyczne usunięcie fragmentów jest wystarczająca dla jednostek, które zajmują tylko jednego kafelka w tileset, takie jak monet i wrogów. Większe jednostki wymagają dodatkową logikę i właściwości.

Drzwi wymagane są dwa Kafelki jest narysowanie całkowicie:

![](cointime-images/image16.png "Drzwi wymaga dwóch Kafelki jest narysowanie całkowicie")

Na kafelku dołu w drzwi zawiera właściwości do utworzenia jednostki (**EntityType** ustawioną **drzwi**):

![](cointime-images/image17.png "Na kafelku dołu w drzwi zawiera właściwości do utworzenia jednostki EntityType ustawione do drzwi")

Ponieważ kafelka w dół w drzwi jest usuwane, gdy jest tworzone wystąpienie drzwi, dodatkową logikę jest potrzebne do usunięcia najwyższym kafelków w czasie wykonywania. Górny Kafelek ma **RemoveMe** ustawioną właściwość **true**:

![](cointime-images/image18.png "Górny Kafelek ma właściwość RemoveMe ustawioną wartość true")



Ta właściwość jest używana do usuwania kafelków w `ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        ...
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```


# <a name="entity-offsets"></a>Jednostka przesunięcia

Utworzone na podstawie Kafelki jednostki są pozycjonowane ustawiając Centrum jednostki przy użyciu Centrum fragmentu. Większe jednostek, takich jak `Door`, można umieścić poprawnie za pomocą dodatkowych właściwości i logika. 

Kafelek drzwi dołu, który definiuje `Door` Określa położenie jednostki **YOffset** o wartości 4. Bez tej właściwości `Door` wystąpienie znajduje się w Centrum kafelka:

![](cointime-images/image19.png "Bez tej właściwości wystąpienia drzwi znajduje się w Centrum kafelka")

 

To jest rozwiązany przez zastosowanie **YOffset** wartość w `ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            ...
        }
...
    }
}
```


# <a name="animated-entities"></a>Animowany jednostek

Godzina monety zawiera kilka animowany jednostek. `Player` i `Enemy` jednostek odtwarzanie animacji przeszukiwania i `Door` encji animację otwierania po zebraniu wszystkich monet.


## <a name="achx-files"></a>pliki .achx

Monety czasu animacji są definiowane w plikach .achx. Każda animacja zdefiniowano między `AnimationChain` tagów, jak pokazano w poniższej animacji zdefiniowane w **propanimations.achx**:


```xml
<AnimationChain>
  <Name>Spikes</Name>
  <ColorKey>0</ColorKey>
  <Frame>
    <FlipHorizontal>false</FlipHorizontal>
    <FlipVertical>false</FlipVertical>
    <TextureName>..\images\mastersheet.png</TextureName>
    <FrameLength>0.1</FrameLength>
    <LeftCoordinate>1152</LeftCoordinate>
    <RightCoordinate>1168</RightCoordinate>
    <TopCoordinate>128</TopCoordinate>
    <BottomCoordinate>144</BottomCoordinate>
    <RelativeX>0</RelativeX>
    <RelativeY>0</RelativeY>
  </Frame>
</AnimationChain> 
```

Ta animacja zawiera tylko jedną ramkę, co w jednostce kolekcji wyświetlania obrazu statycznego. Jednostki można użyć plików .achx, czy są one wyświetlane jednego lub wielu ramki animacji. Dodatkowe ramki można dodać do plików .achx bez konieczności wprowadzania jakichkolwiek zmian w kodzie. 

Ramki zdefiniuj obraz do wyświetlenia w `TextureName` parametr i współrzędne ekranu w `LeftCoordinate`, `RightCoordinate`, `TopCoordinate`, i `BottomCoordinate` tagów. Reprezentuje współrzędne piksela ramki animacji w obrazie, która jest używana — **mastersheet.png** w takim przypadku.

![](cointime-images/image20.png "Reprezentuje współrzędne ramki animacji obrazu w pikselach")

`FrameLength` Właściwość definiuje liczbę sekund, które powinny być wyświetlane ramki animacji. Wartość ta jest ignorowana w jednym ramki animacji.

Wszystkie inne właściwości AnimationChain w pliku .achx są ignorowane przez czas monety.


## <a name="animatedspriteentity"></a>AnimatedSpriteEntity

Logika animacji jest zawarta w `AnimatedSpriteEntity` klasy, która służy jako klasa podstawowa dla większości obiektów używanych w `GameScene`. Udostępnia ona następujące funkcje:

 - Załadowanie `.achx` plików
 - Pamięć podręczna animacji załadować animacji
 - Wystąpienie CCSprite do wyświetlania animacji
 - Logika do zmiany bieżącej ramki

Konstruktor nagłego zapewnia prosty przykład sposobu obciążenia i używać animacji:


```csharp
public Spikes ()
{
    LoadAnimations ("Content/animations/propanimations.achx");
    CurrentAnimation = animations [0];
}
```

**PropAnimations.achx** zawiera tylko jeden animacji, więc konstruktora uzyskuje dostęp do tego animacji przez indeks. Jeśli plik .achx zawiera wiele animacji, a następnie animacji można odwoływać się według nazwy, jak pokazano w `Enemy` konstruktora:


```csharp
walkLeftAnimation = animations.Find (item => item.Name == "WalkLeft");
walkRightAnimation = animations.Find (item => item.Name == "WalkRight");
```


# <a name="summary"></a>Podsumowanie

W tym przewodniku przedstawiono szczegóły implementacji monety czasu. Monety czas ukończenia gry jest tworzona, ale jest również projektu, który można łatwo modyfikować i rozwinięty. Czytniki zachęcamy poświęcić czas wprowadzania zmian do poziomu, dodawanie nowych poziomów i tworzenia nowych jednostek, aby jeszcze lepiej zrozumieć, jak czas monety jest zaimplementowana.

## <a name="related-links"></a>Linki pokrewne

- [Gry projekt (przykład)](https://developer.xamarin.com/samples/mobile/CoinTime/)
