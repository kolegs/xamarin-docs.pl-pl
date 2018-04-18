---
title: Rysowanie grafiki 3D z wierzchołków w MonoGame
description: Używanie tablic wierzchołków do definiowania sposobu obiektu 3D jest renderowany na podstawie na punkt obsługuje MonoGame. Użytkownicy mogą korzystać z tablic wierzchołków do tworzenia dynamicznych geometrii, implementowanie skutków specjalne i zwiększyć wydajność ich renderowania za pośrednictwem usuwanie powierzchni.
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 4736bedd413663af098bbad522cc56f432e36ea0
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>Rysowanie grafiki 3D z wierzchołków w MonoGame

_Używanie tablic wierzchołków do definiowania sposobu obiektu 3D jest renderowany na podstawie na punkt obsługuje MonoGame. Użytkownicy mogą korzystać z tablic wierzchołków do tworzenia dynamicznych geometrii, implementowanie skutków specjalne i zwiększyć wydajność ich renderowania za pośrednictwem usuwanie powierzchni._

Użytkownicy, którzy mają zapoznaj się z artykułem [Przewodnik na renderowanie modele](~/graphics-games/monogame/3d/part1.md) będzie należy zapoznać się z renderowania 3W modelu w MonoGame. `Model` Klasa jest efektywny sposób renderowania grafiki 3D, podczas pracy z danymi zdefiniowanymi w pliku (na przykład fbx), a podczas pracy nad danymi statycznymi. Niektóre gry wymagają geometrii 3D zdefiniowany lub manipulować dynamicznie w czasie wykonywania. W takich przypadkach firma Microsoft Użycie tablic *wierzchołków* do definiowania i renderowania geometrii. Wierzchołek jest ogólne określenie punktu w przestrzeni 3D, która jest częścią listy uporządkowanej używane do definiowania geometrii. Zwykle wierzchołków są uporządkowane w taki sposób, aby zdefiniować szereg trójkąty.

Aby zwizualizować jak wierzchołków są używane do tworzenia obiektów 3W, zastanówmy kuli następujące:

![](part2-images/image1.png "Aby zwizualizować jak wierzchołków są używane do tworzenia obiektów 3W, należy wziąć pod uwagę tym zakresie")

Jak pokazano powyżej, kuli wyraźnie składa się z wielu trójkąty. Firma Microsoft można wyświetlić szkielet kuli, aby zobaczyć, jak wierzchołki łączyć się trójkąty formularza:

![](part2-images/image2.png "Wyświetl szkielet kuli, aby zobaczyć, jak wierzchołki łączyć się trójkąty formularza")

W tym przewodniku omówiono następujące tematy:

- Tworzenie projektu
- Tworzenie wierzchołki
- Dodawanie kodu rysowania
- Renderowanie teksturą
- Modyfikowanie współrzędnych tekstury
- Renderowanie wierzchołków przy użyciu modeli

Zakończono projektu będzie zawierać szachownicą floor, który będzie rysowany przy użyciu tablicy wierzchołka:

![](part2-images/image3.png "Zakończono projektu będzie zawierać szachownicą floor, który będzie rysowany przy użyciu tablicy wierzchołków")

## <a name="creating-a-project"></a>Tworzenie projektu

Firma Microsoft będzie najpierw Pobierz projektu, który będzie służyć jako naszych punkt początkowy. Użyjemy projekt z modelem [której można znaleźć tutaj](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/).

Po pobrane i rozpakowane, Otwórz i uruchom projekt. Oczekuje się zobaczyć sześciu modeli robot jest rysowany na ekranie:

![](part2-images/image4.png "Sześć modeli robot jest rysowany na ekranie")

Na koniec tego projektu firma Microsoft będzie można połączenie renderowania własne niestandardowe wierzchołka z robota `Model`, dlatego firma Microsoft nie mają Usuń kod renderowania robota. Zamiast tego, firma Microsoft będzie tylko czyszczenie `Game1.Draw` wywołanie usunięcia na rysunku 6 robotów teraz. Aby to zrobić, otwórz **Game1.cs** plików i Znajdź `Draw` metody. Zmodyfikuj go tak, aby zawierał następujący kod:

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

Spowoduje to naszych gry wyświetlania ekranu blue pustym:

![](part2-images/image5.png "Spowoduje to w grę wyświetlania ekranu blue pustym")

## <a name="creating-the-vertices"></a>Tworzenie wierzchołki

Utworzymy tablicę wierzchołków do definiowania naszych geometrii. W tym przewodniku możemy utworzyć 3D płaszczyzny (kwadratowe w przestrzeni 3D, nie samolotowy). Mimo że nasze płaszczyzny ma cztery strony i cztery rogi, będzie składa się z dwóch trójkąty, z których każdy wymaga trzech wierzchołków. W związku z tym firma Microsoft będzie można definiowania łącznie z sześciu punktów.

Do tej pory firma Microsoft już został omówieniu wierzchołków w sposób ogólny, ale MonoGame zawiera niektóre standardowe struktury, którego można użyć dla wierzchołków:

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

Nazwa każdego typu wskazuje składników, które zawiera. Na przykład `VertexPositionColor` zawiera wartości dla pozycja i kolor. Przyjrzyjmy się poszczególne składniki:

- Obejmują wszystkie typy wierzchołków pozycji — `Position` składnika. `Position` Wartości definiują położenie wierzchołka w przestrzeni 3D (X, Y i Z).
- Kolor — wierzchołków można opcjonalnie określić `Color` wartość do wykonania niestandardowej cieniowanie.
- Normalny — normalne zdefiniuj jaki sposób jest ukierunkowane powierzchni obiektu. Normalne są niezbędne, jeśli renderowania obiektu oświetlenie od kierunku, który powierzchni jest ukierunkowane wpływa na ilość światła odbiera. Normalne zwykle są określone jako *wektor jednostki* — wektor 3W, która ma długość 1.
- Tekstura — tekstury odnosi się do współrzędnych tekstury — to znaczy, która część tekstury powinien pojawić się na wierzchołku danego. Tekstury, wartości są niezbędne, jeśli renderowania 3W obiektu teksturą. Współrzędne tekstury są znormalizowane współrzędne, co oznacza, że wartości zostaną objęte od 0 do 1. Firma Microsoft będzie obejmować współrzędnych tekstury bardziej szczegółowo w dalszej części tego przewodnika.

Nasze płaszczyzny będzie służyć jako piętra i chcemy zastosować teksturę podczas wykonywania naszych renderowania, użyjemy `VertexPositionTexture` typu do definiowania naszych wierzchołków.

Najpierw dodamy członka do naszej klasy Game1:

```csharp
VertexPositionTexture[] floorVerts; 
```

Następnie określ naszych wierzchołków w `Game1.Initialize`. Należy zauważyć, że nie zawiera podanego szablonu odwołuje się do wcześniej w tym artykule `Game1.Initialize` metody, więc musimy dodać metodę cały `Game1`:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];
    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);
    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // We’ll be assigning texture values later
    base.Initialize ();
}
```

Aby zwizualizować wygląd naszego wierzchołków, należy wziąć pod uwagę poniższym diagramie:

![](part2-images/image6.png "Aby zwizualizować wygląd wierzchołki, należy wziąć pod uwagę na tym wykresie")

Musimy polegać na naszych diagram do wizualizacji wierzchołków do momentu zakończenia wdrażania naszego kodu renderowania.

## <a name="adding-drawing-code"></a>Dodawanie kodu rysującego

Teraz, gdy mamy stanowisk dla naszych geometrii zdefiniowane możemy zapisać naszego kodu renderowania.

Najpierw musimy zdefiniować `BasicEffect` wystąpienia, w której będą przechowywane parametry w celu renderowania, takie jak pozycja i oświetlenia. Aby to zrobić, należy dodać `BasicEffect` członka `Game1` klasy poniżej where `floorVerts` zdefiniowano pola:


```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

Następnie należy zmodyfikować `Initialize` metodę, aby określić wpływ:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // new code:
    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Teraz możemy dodać kod do wykonania na rysunku:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);

    float aspectRatio = 
        graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
    float nearClipPlane = 1;
    float farClipPlane = 200;

    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);


    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
            // We’ll be rendering two trinalges
            PrimitiveType.TriangleList,
            // The array of verts that we want to render
            floorVerts,
            // The offset, which is 0 since we want to start 
            // at the beginning of the floorVerts array
            0,
            // The number of triangles to draw
            2);
    }
}
```

Musisz wywołać `DrawGround` w naszym `Game1.Draw`:

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

W aplikacji zostaną wyświetlone następujące po wykonaniu:

![](part2-images/image7.png "Aplikacja wyświetli, to podczas wykonywania")

Oto niektóre szczegółowe informacje w powyższym kodzie.

### <a name="view-and-projection-properties"></a>Wyświetl i właściwości projekcji

`View` i `Projection` właściwości formantu, jak możemy wyświetlić sceny. Firma Microsoft będzie można modyfikowania ten kod później podczas możemy ponownie dodać kod renderowania modelu. W szczególności `View` Określa lokalizację i orientację aparatu, i `Projection` formanty *polu widoku* (które mogą służyć do powiększania aparatu).

### <a name="techniques-and-passes"></a>Techniki i przebiegów

Raz możemy przypisane właściwości na naszych efekt, który może wykonać rzeczywiste renderowania. 

Firma Microsoft nie będzie można zmienić `CurrentTechnique` właściwość w tym wskazówki, ale bardziej zaawansowane gry może mieć pojedynczy efekt, które mogą wykonywać rysowanie na różne sposoby (np. sposób stosowania wartości koloru). Każdy z tych trybów renderowanie może być reprezentowana jako technika, który można przypisać przed renderowaniem. Ponadto każdy technika może wymagać wielu przekazuje do renderowania poprawnie. Jeśli renderowania złożonych elementów wizualnych, takich jak świecącego powierzchni lub futerkowych efekty może być konieczne wiele przebiegów.

Jest to ważne jest, aby należy pamiętać, że `foreach` pętli umożliwia tego samego kodu C# do renderowania działa niezależnie od złożoności podstawowych `BasicEffect`.

### <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives` to, gdzie mają być renderowane wierzchołków. Pierwszy parametr określa metodę, jak ma zorganizowane naszych wierzchołków. Firma Microsoft ich struktury, aby każdy trójkąt jest definiowana za pomocą trzech wierzchołków uporządkowanych, więc używamy `PrimitiveType.TriangleList` wartość.

Drugi parametr jest tablicą wierzchołków, które zdefiniowanego wcześniej.

Trzeci parametr określa indeks pierwszego do rysowania. Ponieważ chcemy naszych tablicy całego wierzchołków do renderowania firma Microsoft będzie przekazać wartość 0.

Ponadto firma Microsoft Określ ile trójkąty do renderowania. Nasze wierzchołków tablica zawiera dwa trójkąty, więc przekazać wartość 2.

## <a name="rendering-with-a-texture"></a>Renderowanie teksturą

W tym momencie nasze aplikacji renderuje płaszczyźnie białe (w perspektywie). Teraz dodamy tekstury do naszej projektu do użycia podczas renderowania naszych płaszczyzny. 

Aby zapewnić proste dodamy .png bezpośrednio do naszej projektu, a nie za pomocą narzędzia MonoGame potoku. Aby to zrobić, należy pobrać [ten plik PNG](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true) do komputera. Po pobraniu, kliknij prawym przyciskiem myszy **zawartości** folderu w konsoli rozwiązania i wybierz **Dodaj > Dodaj pliki...**  . Jeśli działa w systemie Android, następnie ten będzie znajdować się w **zasoby** folderu w projekcie specyficzne dla systemu Android. Jeśli w systemie iOS, to ten folder będą w folderze głównym projektu iOS. Przejdź do lokalizacji, gdzie **checkerboard.png** jest zapisywana i wybrać ten plik. Wybierz, aby skopiować plik do katalogu.

Następnie dodamy kod w celu utworzenia naszych `Texture2D` wystąpienia. Najpierw dodaj `Texture2D` jako element członkowski `Game1` w obszarze `BasicEffect` wystąpienie:

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

Modyfikowanie `Game1.LoadContent` w następujący sposób:


```csharp
protected override void LoadContent()
{
    // Notice that loading a model is very similar
    // to loading any other XNB (like a Texture2D).
    // The only difference is the generic type.
    model = Content.Load<Model> ("robot");

    // We aren't using the content pipeline, so we need
    // to access the stream directly:
    using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
    {
        checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
    }
}
```

Następnie należy zmodyfikować `DrawGround` metody. Modyfikacja tylko konieczne jest przypisanie `effect.TextureEnabled` do `true` i ustawić `effect.Texture` do `checkerboardTexture`:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);

    float aspectRatio = 
        graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
    float nearClipPlane = 1;
    float farClipPlane = 200;

    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

    // new code:
    effect.TextureEnabled = true;
    effect.Texture = checkerboardTexture;

    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
                    PrimitiveType.TriangleList,
            floorVerts,
            0,
            2);
    }
}
```

Na koniec należy zmodyfikować `Game1.Initialize` metody można również przypisać tekstury koordynuje na naszych wierzchołków:


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    // New code:
    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, 1);
    floorVerts [2].TextureCoordinate = new Vector2 (1, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (1, 1);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
} 
```

Jeśli firma Microsoft wykonywania kodu widzimy że nasze płaszczyzny są obecnie wyświetlane wzorzec szachownicy:

![](part2-images/image8.png "Płaszczyzny są obecnie wyświetlane wzorzec szachownicy")

## <a name="modifying-texture-coordinates"></a>Modyfikowanie tekstury koordynuje

Używa MonoGame znormalizowanych współrzędnych tekstury, będące współrzędne od 0 do 1, a nie od 0 do tekstury szerokość lub wysokość. Poniższy diagram może pomóc wizualizacji znormalizowane współrzędne:

![](part2-images/image9.png "Ten diagram może pomóc wizualizacji znormalizowane współrzędnych")

Współrzędne tekstury znormalizowane Zezwalaj tekstury zmiana rozmiaru bez konieczności ponownego pisania kodu lub Utwórz ponownie modeli (takich jak pliki fbx). Jest to możliwe, ponieważ znormalizowane współrzędne reprezentuje stosunek zamiast pikseli. Na przykład (1,1) będzie reprezentować zawsze prawego dolnego rogu niezależnie od rozmiaru tekstury.

Można zmienić przypisania współrzędnych tekstury być pojedynczą zmienną liczbę powtórzeń:


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    int repetitions = 20;

    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
    floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Powoduje to tekstury powtarzające się 20 razy:

![](part2-images/image10.png "Powoduje to tekstury powtarzające się 20 razy")


## <a name="rendering-vertices-with-models"></a>Renderowanie wierzchołków przy użyciu modeli

Teraz, prawidłowo renderowania naszych płaszczyzny możemy ponownie dodać modele, aby wyświetlić wszystkie elementy ze sobą. Po pierwsze, ponownie dodamy kod modelu do naszych `Game1.Draw` — metoda (za pomocą pozycji modyfikacji):

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    DrawGround ();

    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));

    DrawModel (new Vector3 (-4, 4, 3));
    DrawModel (new Vector3 ( 0, 4, 3));
    DrawModel (new Vector3 ( 4, 4, 3));

    base.Draw(gameTime);
} 
```

Zostanie również utworzony `Vector3` w `Game1` do reprezentowania położenia naszych kamery. Dodamy pole w obszarze naszych `checkerboardTexture` deklaracji:

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10); 
```

Następnie usuń lokalnej `cameraPosition` zmienną z `DrawModel` metody:

```csharp
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = Matrix.CreateTranslation (modelPosition);

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector); 
            ...
```

Podobnie Usuń lokalnej `cameraPosition` zmienną z `DrawGround` metody:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);
    ... 
```

Teraz możemy uruchomić kod możemy widzą zarówno modeli i podstaw w tym samym czasie:

![](part2-images/image11.png "Zarówno modeli i podstaw są wyświetlane w tym samym czasie")

Firma Microsoft zmodyfikowania pozycji kamery (takich jak przez odpowiednie zwiększenie jego wartości X, które w tym przypadku przesuwa aparatu w lewo) zobaczysz, że wartość ma wpływ zarówno ziemi, jak i modelu:

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

Ten kod wyniki będą następujące:

![](part2-images/image3.png "Ten kod wyników w tym widoku")

## <a name="summary"></a>Podsumowanie

W tym przewodniku pokazano, jak używać tablicy wierzchołków do wykonywania niestandardowego renderowania. W takim przypadku utworzyliśmy szachownicą floor łącząc naszych renderowania opartego na wierzchołku teksturą i `BasicEffect`, ale kod przedstawionych w tym miejscu służy jako podstawa dowolnego renderowania 3W. Również wykazało się, czy renderowanie wierzchołków na podstawie można można łączyć z modeli w tej samej sceny.

## <a name="related-links"></a>Linki pokrewne

- [Plik szachownicy (przykład)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [Ukończono projekt (przykład)](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)
