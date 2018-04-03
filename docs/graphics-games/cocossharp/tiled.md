---
title: Przy użyciu rozmieszczany z CocosSharp
description: Rozmieszczany jest wydajne, elastyczne i mapowania dojrzałe aplikacji do tworzenia Kafelek prostopadły i izometryczny gier. CocosSharp zapewnia wbudowanej integracji sąsiadująco w natywny plik formatu.
ms.topic: article
ms.prod: xamarin
ms.assetid: 804C042C-F62A-4E6C-B10F-06528637F0E2
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 68afa9d175140fd5104e83282a2f72c47625d882
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2018
---
# <a name="using-tiled-with-cocossharp"></a>Przy użyciu rozmieszczany z CocosSharp

_Rozmieszczany jest wydajne, elastyczne i mapowania dojrzałe aplikacji do tworzenia Kafelek prostopadły i izometryczny gier. CocosSharp zapewnia wbudowanej integracji sąsiadująco w natywny plik formatu._

*Sąsiadująco* aplikacji jest standardem tworzenia *kafelków mapy* przeznaczonego do tworzenia gier. Ten przewodnik przeprowadzi jak przejąć istniejący plik .tmx (plik utworzony przez sąsiadująco) i użyć jej w grze CocosSharp. W szczególności ten przewodnik obejmuje:

 - Celem kafelków mapy
 - Praca z plikami .tmx
 - Zagadnienia dotyczące renderowania pikseli grafiki
 - Przy użyciu właściwości kafelka w czasie wykonywania

Gdy Zakończono firma Microsoft będzie mieć pokaz następujące:

![](tiled-images/image1.png "Aplikacja demonstracyjna utworzone, wykonując kroki opisane w tym przewodniku")


## <a name="the-purpose-of-tile-maps"></a>Celem kafelków mapy

Kafelek map istniejących w opracowywaniu gier dla dekad, ale są nadal powszechnie używane w gier 2W w celu ich wydajności i esthetics. Kafelków mapy są niemożliwe do osiągnięcia na bardzo wysokim poziomie wydajności za pośrednictwem ich używania zestawów kafelka — używane przez kafelków mapy obrazu źródłowego. Zestaw kafelków jest kolekcją obrazów połączone w jeden plik. Mimo że zestawy kafelka odwołują się do obrazy używane w kafelków mapy, pliki, które zawierają wiele obrazów mniejszych są również nazywane arkusze sprite lub sprite mapy w opracowywaniu gier. Firma Microsoft wizualizacji, używania zestawów kafelka, dodając siatka do zestawu kafelka, który będzie używany w naszej Pokaz:

![](tiled-images/image2.png "Wizualizowanego widoku używania zestawów kafelka przez dodanie do zestawu kafelka, który będzie używany w pokaz siatki")

Kafelków mapy rozmieszczać Kafelki poszczególnych z zestawów kafelka. Należy pamiętać, każda mapa kafelka nie musi przechowywać kopię kafelka ustawić — zamiast wielu map kafelka może się odwoływać ten sam zestaw kafelka. Oznacza to, jako uzupełnienie zestaw kafelków mapy kafelka wymaga bardzo mało pamięci. Dzięki temu tworzenie dużej liczby kafelków mapy, nawet wtedy, gdy są one używane do tworzenia obszaru dużych gry, takich jak [przewijanie platformera](http://en.wikipedia.org/wiki/Platform_game) środowiska. Poniżej przedstawiono możliwe uzgodnienia przy użyciu tego samego zestawu kafelka:

![](tiled-images/image3.png "Ten obraz zawiera uzgodnienia możliwe przy użyciu tego samego zestawu kafelka")

![](tiled-images/image4.png "Ten obraz zawiera uzgodnienia możliwe przy użyciu tego samego zestawu kafelka")


## <a name="working-with-tmx-files"></a>Praca z plikami .tmx

Format pliku .tmx jest plik XML utworzony przez aplikację sąsiadująco, który może być [pobrać bezpłatnie w witrynie sieci Web sąsiadująco](http://www.mapeditor.org/). Format pliku .tmx przechowuje informacje dla kafelków mapy. Zwykle gry będzie mieć jeden plik .tmx dla każdego poziomu lub osobnych obszaru.

Ten przewodnik koncentruje się na temat korzystania z istniejących plików .tmx w CocosSharp; jednak dodatkowe samouczki można znaleźć online, w tym [to wprowadzenie do edytora mapy sąsiadująco](http://gamedevelopment.tutsplus.com/tutorials/introduction-to-tiled-map-editor--gamedev-2838).

Musisz Rozpakuj [pliku zip zawartości](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true) będzie używane w naszych gier. Najpierw należy pamiętać, jest, że kafelków mapy używany zarówno w pliku .tmx w (dungeon.tmx) oraz jedną lub więcej pliki obrazów, które definiują kafelka zestawu danych (dungeon_1.png). Gier musi zawierać jednocześnie te pliki, aby załadować .tmx w czasie wykonywania, a więc Dodaj zarówno do projektu **zawartości** folderu (który jest zawarty w **zasoby** folderu w projektach systemu Android). W szczególności należy dodać pliki w folderze o nazwie **tilemaps** wewnątrz **zawartości** folderu:

![](tiled-images/image5.png "Dodaj pliki w folderze o nazwie tilemaps w folderze zawartości")

**Dungeon.tmx** można teraz załadować pliku w czasie wykonywania w `CCTileMap` obiektu. Następnie zmodyfikuj `GameLayer` (lub równoważne głównego obiektu kontenera) zawierać `CCTileMap` wystąpienia i dodać ją jako element podrzędny:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");
        this.AddChild (tileMap);
    }
} 
```

Jeśli przeprowadzana grę, którą zostanie wyświetlone mapy kafelka są wyświetlane w lewym dolnym rogu ekranu:

![](tiled-images/image6.png "Jeśli uruchomienia gry mapy kafelka są wyświetlane w lewym dolnym rogu ekranu")


## <a name="considerations-for-rendering-pixel-art"></a>Zagadnienia dotyczące renderowania pikseli grafiki

Pikseli grafiki, w kontekście rozwoju gier wideo odwołuje się do 2D visual grafikę, który jest zwykle tworzony przez strony, a często jest w niskiej rozdzielczości. Pikseli grafiki może być twierdzi czasu znacznym do tworzenia, więc pikseli grafiki kafelka zestawy często zawierają Kafelki w niskiej rozdzielczości, takich jak 16 lub 32 pikseli szerokości i wysokości. Jeśli nie są skalowane w czasie wykonywania, grafikę pikseli często jest zbyt mały dla większość nowoczesnych telefony i tablety.

Firma Microsoft może dostosować wyświetlane wymiary w pliku GameAppDelegate.cs naszych grę, gdzie dodamy wywołanie `CCScene.SetDefaultDesignResolution`:


```csharp
 public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");

    CCSize windowSize = mainWindow.WindowSizeInPixels;

    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to be (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDefaultDesignResolution (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    
    // Determine whether to use the high or low def versions of our images
    // Make sure the default texel to content size ratio is set correctly
    // Of course you're free to have a finer set of image resolutions e.g (ld, hd, super-hd)
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }

    // New code:
    CCScene.SetDefaultDesignResolution (380, 240, CCSceneResolutionPolicy.ShowAll);

    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);

    mainWindow.RunWithScene (scene);
} 
```

Aby uzyskać więcej informacji na temat `CCSceneResolutionPolicy`, zobacz w naszym przewodniku [Obsługa rozwiązania w CocosSharp](~/graphics-games/cocossharp/resolutions.md).

Teraz możemy uruchomienia gry, widzimy gier podejmowanie pełny ekran naszych urządzenia:

![](tiled-images/image7.png "Gry podejmowanie pełny ekran urządzenia")

Wreszcie chcemy wyłączyć antialiasingu na naszych kafelków mapy. `Antialiased` Właściwość jest stosowana efekt rozmywania podczas renderowania obiektów, które są powiększona. Antialiasingu może przyczynić się do zmniejszenia wygląd podzielony na piksele obiektów graficznych, ale może również wprowadzać własną artefakty renderowania. W szczególności antialiasingu rozmywa zawartości każdego kafelka. Jednak krawędzi każdego kafelka nie rozmywania, co czyni poszczególnych Kafelki wyróżniające zamiast mieszania się przy użyciu sąsiadujących Kafelki. Firma Microsoft należy również zauważyć, że pikseli grafikę gier często zachować ich wygląd podzielony na piksele, aby zachować *światła* uznać.

Ustaw `Antialiased` do `false` po konstruowania `tileMap`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();

    tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

    // new code:
    tileMap.Antialiased = false;

    this.AddChild (tileMap);
} 
```

Teraz naszych kafelków mapy nie będą wyświetlane rozmyte:

![](tiled-images/image8.png "Teraz kafelków mapy nie będą wyświetlane rozmyte")


## <a name="using-tile-properties-at-runtime"></a>Przy użyciu właściwości kafelka w czasie wykonywania

Do tej pory mamy `CCTileMap` ładowania pliku .tmx i wyświetlanie, ale firma Microsoft nie ma możliwości wchodzić w interakcje z nią. W szczególności niektórych Kafelki (np. nasze Skarb piersi) musi być niestandardowej logiki. Firma Microsoft czynności w sposób wykrywania kafelka niestandardowe właściwości i różnych sposobach reagowania na te właściwości zidentyfikowane raz w czasie wykonywania.

Zanim firma Microsoft pisania kodu, musimy dodać właściwości do naszej kafelków mapy za pośrednictwem sąsiadująco. Aby to zrobić, otwórz plik dungeon.tmx w programie sąsiadująco. Pamiętaj otworzyć plik, który jest używany w projekcie gier.

Po otwarciu wybierz piersi Skarb na kafelku ustawić, aby wyświetlić jego właściwości:

![](tiled-images/image9.png "Po otwarciu wybierz piersi Skarb na kafelku ustawić, aby wyświetlić jego właściwości")

Jeśli nie są wyświetlane właściwości piersi Skarb, kliknij prawym przyciskiem myszy na piersi Skarb i wybierz **kafelka właściwości**:

![](tiled-images/image10.png "Jeśli nie są wyświetlane właściwości piersi Skarb, kliknij prawym przyciskiem myszy na piersi Skarb i wybierz polecenie Właściwości kafelka")

Ułożenie sąsiadujące właściwości są wdrażane z nazwą i wartością. Aby dodać właściwość, kliknij przycisk **+** przycisku, wprowadź nazwę **IsTreasure**, kliknij przycisk **OK**, wprowadź wartość **true**: 

![](tiled-images/image11.png "Aby dodać właściwość, kliknij przycisk, wprowadź nazwę IsTreasure, kliknij przycisk OK, a następnie wprowadź wartość true")

Należy pamiętać zapisać plik .tmx po zmodyfikowaniu właściwości.

Na koniec dodamy kod, aby wyszukać naszych nowo dodanych właściwości. Pętla zostanie kroki poszczególnych `CCTileMapLayer` (naszym mapy ma warstwy 2), następnie za pomocą wiersza i kolumny, aby wyszukać wszystkie Kafelki, które mają `IsTreasure` właściwości:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

        // new code:
        tileMap.Antialiased = false;

        this.AddChild (tileMap);

        HandleCustomTileProperties (tileMap);
    }

    void HandleCustomTileProperties(CCTileMap tileMap)
    {
        // Width and Height are equal so we can use either
        int tileDimension = (int)tileMap.TileTexelSize.Width;

        // Find out how many rows and columns are in our tile map
        int numberOfColumns = (int)tileMap.MapDimensions.Size.Width;
        int numberOfRows = (int)tileMap.MapDimensions.Size.Height;

        // Tile maps can have multiple layers, so let's loop through all of them:
        foreach (CCTileMapLayer layer in tileMap.TileLayersContainer.Children)
        {
            // Loop through the columns and rows to find all tiles
            for (int column = 0; column < numberOfColumns; column++)
            {
                // We're going to add tileDimension / 2 to get the position
                // of the center of the tile - this will help us in 
                // positioning entities, and will eliminate the possibility
                // of floating point error when calculating the nearest tile:
                int worldX = tileDimension * column + tileDimension / 2;
                for (int row = 0; row < numberOfRows; row++)
                {
                    // See above on why we add tileDimension / 2
                    int worldY = tileDimension * row + tileDimension / 2;

                    HandleCustomTilePropertyAt (worldX, worldY, layer);
                }
            }
        }
    }

    void HandleCustomTilePropertyAt(int worldX, int worldY, CCTileMapLayer layer)
    {
        CCTileMapCoordinates tileAtXy = layer.ClosestTileCoordAtNodePosition (new CCPoint (worldX, worldY));

        CCTileGidAndFlags info = layer.TileGIDAndFlags (tileAtXy.Column, tileAtXy.Row);

        if (info != null)
        {
            Dictionary<string, string> properties = null;

            try
            {
                properties = tileMap.TilePropertiesForGID (info.Gid);
            }
            catch
            {
                // CocosSharp 
            }

            if (properties != null && properties.ContainsKey ("IsTreasure") && properties["IsTreasure"] == "true" )
            {
                layer.RemoveTile (tileAtXy);

                // todo: Create a treasure chest entity
            }
        }
    }
} 
```

Większość kodu jest oczywiste, ale powinien omówimy Obsługa Skarb kafelków. W takim przypadku zostaną usunięte wszystkie Kafelki, które są identyfikowane jako skrzyń skarb. Jest to spowodowane skrzyń Skarb prawdopodobnie będzie konieczne niestandardowego kodu w czasie wykonywania do kolizji efekt i udzielenia odtwarzacz zawartości Skarb po otwarciu. Ponadto Skarb może być konieczne reagowania na jest otwarty (zmiana jego wygląd) i może mieć logiki dla tylko kiedy wyświetlaniu wszystkich na ekranie wrogów zostały bezcelowe.

Innymi słowy, piersi Skarb będą korzystać z jest jednostką, a nie jako proste kafelka w `CCTileMap`. Aby uzyskać więcej informacji na jednostkach gier, zobacz [przewodnik jednostek w CocosSharp](~/graphics-games/cocossharp/entities.md).


## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano sposób załadować .tmx tworzonych przez sąsiadująco w aplikacji CocosSharp. Widoczny jest sposób modyfikowania rozpoznawania aplikacji do uwzględnienia niższej rozdzielczości pikseli grafiki i jak znaleźć Kafelki według ich właściwości, do wykonania niestandardowej logiki, takich jak tworzenie wystąpienia jednostki.

## <a name="related-links"></a>Linki pokrewne

- [Ułożenie sąsiadujące witryny sieci Web](http://www.mapeditor.org/)
- [Zip zawartości](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true)
