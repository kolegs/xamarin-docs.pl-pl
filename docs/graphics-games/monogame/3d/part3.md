---
title: Współrzędne MonoGame 3D
description: Zrozumienie 3D współrzędnych jest ważnym krokiem podczas tworzenia gier 3D. MonoGame udostępnia szereg klas rozmieszczania, zmiana orientacji i skalowanie obiektów w przestrzeni 3D.
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 2f14d21302ed4295d16baa28723df6ef79863686
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921641"
---
# <a name="3d-coordinates-in-monogame"></a>Współrzędne MonoGame 3D

_Zrozumienie 3D współrzędnych jest ważnym krokiem podczas tworzenia gier 3D. MonoGame udostępnia szereg klas rozmieszczania, zmiana orientacji i skalowanie obiektów w przestrzeni 3D._

Opracowanie grach 3W wymaga zrozumienia sposobu modyfikowania obiektów 3W układu współrzędnych. W tym przewodniku opisano sposób modyfikowania visual obiekty (w szczególności Model). Firma Microsoft będzie kompilacji na koncepcji kontrolowanie model, aby utworzyć klasę aparatu 3D.

Koncepcji przedstawionych pochodzić z algebraiczną skali liniowej, ale przeniesiemy praktyczne rozwiązania, dzięki czemu dowolny użytkownik bez tła silne matematyczne mogą stosować te pojęcia związane z własnych gier.

Firma Microsoft będzie obejmujące następujące tematy:

- Tworzenie projektu
- Tworzenie jednostki Robot
- Przenoszenie jednostki Robot
- Mnożenie macierzy
- Tworzenie jednostki aparatu
- Przenoszenie aparatu przy użyciu danych wejściowych

Po skończeniu mamy będzie projekt z robotem przenoszenie koło i aparatu, które mogą być kontrolowane na podstawie wprowadzania dotykowego:

![](part3-images/image1.gif "Po skończeniu aplikacji obejmuje projekt robotem przenoszenie koło i aparatu, które mogą być kontrolowane na podstawie wprowadzania dotykowego")


## <a name="creating-a-project"></a>Tworzenie projektu

Ten przewodnik koncentruje się na przenoszenie obiektów w przestrzeni 3D. Rozpocznie się z projektem dla modeli renderowania i tablice wierzchołków [której można znaleźć tutaj](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/). Po pobraniu, Rozpakuj i otworzyć projekt, aby upewnić się na uruchomieniu i firma Microsoft powinien zostać wyświetlony następujący:

![](part3-images/image2.png "Po pobraniu, Rozpakowywanie i otwieranie projektu, aby upewnić się na uruchomieniu i można wyświetlić tego widoku")


## <a name="creating-a-robot-entity"></a>Tworzenie jednostki Robot

Zanim zaczniemy przenoszenie naszych robota wokół, utworzymy `Robot` klasa może zawierać logiki rysowania i przepływu. Deweloperzy gier odwoływać się do tego hermetyzacji logikę i dane jako *jednostki*.

Dodaj nowy plik pustą klasę **MonoGame3D** przenośnej biblioteki klas (nie ModelAndVerts.Android specyficzne dla platformy). Nadaj mu nazwę **robotem** i kliknij przycisk **nowy**:

![](part3-images/image3.png "Nadaj mu nazwę Robot, a następnie kliknij przycisk Nowy")

Modyfikowanie `Robot` klas w następujący sposób:

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Content;

namespace MonoGame3D
{
    public class Robot
    {
        Model model;

        public void Initialize(ContentManager contentManager)
        {
            model = contentManager.Load<Model> ("robot");

        }

        // For now we'll take these values in, eventually we'll
        // take a Camera object
        public void Draw(Vector3 cameraPosition, float aspectRatio)
        {
            foreach (var mesh in model.Meshes)
            {
                foreach (BasicEffect effect in mesh.Effects)
                {
                    effect.EnableDefaultLighting ();
                    effect.PreferPerPixelLighting = true;

                    effect.World = Matrix.Identity; 
                    var cameraLookAtVector = Vector3.Zero;
                    var cameraUpVector = Vector3.UnitZ;

                    effect.View = Matrix.CreateLookAt (
                        cameraPosition, cameraLookAtVector, cameraUpVector);

                    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                    float nearClipPlane = 1;
                    float farClipPlane = 200;

                    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);


                }

                // Now that we've assigned our properties on the effects we can
                // draw the entire mesh
                mesh.Draw ();
            }
        }
    }
}
```

`Robot` Kod jest zasadniczo taki sam kod w `Game1` do rysowania `Model`. Przegląd na `Model` ładowania i rysowanie, zobacz [ten przewodnik na temat pracy z modelami](~/graphics-games/monogame/3d/part1.md). Można teraz usunąć wszystkie `Model` ładowanie i renderowania kodu z `Game1`i zastąp go z `Robot` wystąpienie:

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        Vector3 cameraPosition = new Vector3(15, 10, 10);

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

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

            robot = new Robot ();
            robot.Initialize (Content);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            robot.Draw (cameraPosition, aspectRatio);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
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
    }
}
```

Jeśli jest przeprowadzana kod teraz będzie mamy sceny z tylko jedną robot, który jest rysowany głównie w obszarze floor:

![](part3-images/image4.png "Jeśli kod jest teraz uruchomione, aplikacja wyświetli sceny tylko jeden robot, który jest rysowany głównie w obszarze piętro")

## <a name="moving-the-robot"></a>Przenoszenie robota

Teraz, gdy mamy `Robot` klasy, można dodać do robota logiki przenoszenia. W takim przypadku po prostu wybierzemy robota przenieść koło uzależnionych od czasu gry. Jest to nieco niepraktyczne implementacji dla rzeczywistym gry, ponieważ znak zwykle mogą odpowiadać na dane wejściowe lub sztucznego analizy, ale zapewnia środowisko nam poznać 3D pozycjonowanie i obrotu.

Tylko informacje będą potrzebne z poza `Robot` klasy jest bieżący czas gier. Dodamy `Update` metodę, która spowoduje przejście `GameTime` parametru. To `GameTime` parametr będzie służyć do zwiększane kątowi zmiennej, która zostanie użyta do określenia pozycji końcowej dla robota.

Najpierw dodamy pola kąt `Robot` klasy w obszarze `model` pola:

```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ...
```

 Teraz możemy zwiększenie tej wartości w `Update` funkcji:

```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

Należy upewnić się, że `Update` metoda jest wywoływana z `Game1.Update`:

```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
}
```

Oczywiście w tym momencie nie działają w polu Kąt — należy napisać kod, aby go użyć. Firma Microsoft będzie zmodyfikować `Draw` — metoda tak, aby firma Microsoft może obliczyć świecie `Matrix` w metodzie dedykowany: 

```csharp
public void Draw(Vector3 cameraPosition, float aspectRatio)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            // We’ll be doing our calculations here...
            effect.World = GetWorldMatrix();

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }

        mesh.Draw ();
    }
}
```

Następnie będzie wdrożymy `GetWorldMatrix` metoda `Robot` klasy:

```csharp
Matrix GetWorldMatrix()
{
    const float circleRadius = 8;
    const float heightOffGround = 3;

    // this matrix moves the model "out" from the origin
    Matrix translationMatrix = Matrix.CreateTranslation (
        circleRadius, 0, heightOffGround);

    // this matrix rotates everything around the origin
    Matrix rotationMatrix = Matrix.CreateRotationZ (angle);

    // We combine the two to have the model move in a circle:
    Matrix combined = translationMatrix * rotationMatrix;

    return combined;
}
```

Wynik uruchomienia tego kodu powoduje robota przenoszenie w koło:

![](part3-images/image5.gif "Uruchamianie wyniki tego kodu w robota przenoszenie w okręgu")

## <a name="matrix-multiplication"></a>Mnożenie macierzy

Powyższy kod obraca robota, tworząc `Matrix` w `GetWorldMatrix` metody. `Matrix` Struktury zawiera 16 wartości typu float, które mogą być używane do tłumaczenia (zestaw pozycji), obracanie i skalowanie (Ustaw rozmiar). Gdy firma Microsoft przypisać `effect.World` właściwości, firma Microsoft informuje bazowy renderowania pozycji, rozmiaru i orientacji niezależnie od systemu możemy się zdarzyć, aby pobierać ( `Model` lub geometrii wierzchołków). 

Na szczęście `Matrix` struktury obejmuje kilka metod, które ułatwiają tworzenie typowych macierzy. Jest używany w powyższym kodzie pierwszy `Matrix.CreateTranslation`. Termin matematyczne *tłumaczenia* odwołuje się do operacji, która powoduje w punkcie (lub w tym przypadku w modelu) przenoszenie z jednej lokalizacji do innej bez jakiejkolwiek modyfikacji (na przykład obracanie lub zmiany rozmiaru). Funkcja przyjmuje wartość X, Y i Z tłumaczenia. Czy możemy wyświetlić naszych sceny od góry do dołu, naszych `CreateTranslation` — metoda (w izolacji) wykonuje następujące czynności:

![](part3-images/image6.png "Ta akcja wykonuje metodę CreateTranslation w izolacji")

Drugi macierzy utworzymy jest macierzy obrotu przy użyciu `CreateRotationZ` macierzy. Jest to jeden z trzech metod, które mogą być używane do tworzenia obrotu:

- `CreateRotationX`
- `CreateRoationY`
- `CreateRotationZ`

Każda metoda tworzy macierzy obrotu przez obrócenie o danym osi. W tym przypadku firma Microsoft obrót wokół osi Z, które punktów "". Następujące może pomóc wizualizować sposób oparte na osi obrotu działania:

![](part3-images/image7.png "Może to ułatwić wizualizować sposób oparte na osi obrotu działania")

Korzystamy również `CreateRotationZ` metody z polem kąta, który zwiększa się wraz z upływem czasu ze względu na naszych `Update` wywołania metody. W wyniku `CreateRotationZ` metoda powoduje, że nasze robot obracać wokół źródła w miarę upływu czasu.

Końcowe wiersz kodu łączy macierzy dwa na jeden:

```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

Jest to określane jako mnożenie macierzy, które działa nieco inne niż w regularnych mnożenia. *Przemienne właściwości mnożenia* stany kolejność numerów w operacji mnożenia nie zmienia wynik. Oznacza to 3 * 4 jest odpowiednikiem 4 * 3. Mnożenie macierzy różni się, że nie jest przemienne. Oznacza to, że wiersz powyżej może zostać odczytany jako "Zastosuj translationMatrix można przenieść modelu, a następnie Obróć wszystkie elementy, stosując rotationMatrix". Firma Microsoft można wizualizować sposób, że wiersz powyżej ma wpływ na położenie i obrotu w następujący sposób:

![](part3-images/image8.png "Wizualizacja pf sposób pozycji i obrotu wpływa na wiersz powyżej")

Aby lepiej zrozumieć, jak kolejnością mnożenie macierzy może wpłynąć na wyniki, należy wziąć pod uwagę poniższe polecenie, gdzie jest odwrócony mnożenie macierzy:

```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

Powyższy kod będzie najpierw Obróć model w miejscu, a następnie przekształca ją:

![](part3-images/image9.png "Powyższy kod będzie najpierw Obróć model w miejscu, a następnie przekształca ją")

Możemy uruchomić kod z odwróconą mnożenia, firma Microsoft zauważyć, że ponieważ obrót stosuje się najpierw, ma wpływ tylko orientację modelu oraz pozycji modelu pozostanie taka sama. Innymi słowy model obraca się w miejscu:

![](part3-images/image10.gif "Model obraca się w miejscu")

## <a name="creating-the-camera-entity"></a>Tworzenie jednostki aparatu

`Camera` Jednostki będzie zawierać wszystkie niezbędne do wykonania na podstawie danych wejściowych przepływu i zapewnienia właściwości przypisywania właściwości na logiki `BasicEffect` klasy.

Firma Microsoft będzie najpierw implementuje statycznej aparatu (nie na podstawie danych wejściowych przepływu) i zintegrować ją z naszych istniejący projekt. Dodaj nową klasę do **MonoGame3D** przenośnej biblioteki klas (takie same projektu z `Robot.cs`) i nadaj mu nazwę **aparatu**. Zastąp zawartość pliku następującym kodem:

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Camera
    {
        // We need this to calculate the aspectRatio
        // in the ProjectionMatrix property.
        GraphicsDevice graphicsDevice;

        Vector3 position = new Vector3(15, 10, 10);

        public Matrix ViewMatrix
        {
            get
            {
                var lookAtVector = Vector3.Zero;
                var upVector = Vector3.UnitZ;

                return Matrix.CreateLookAt (
                    position, lookAtVector, upVector);
            }
        }

        public Matrix ProjectionMatrix
        {
            get
            {
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                float nearClipPlane = 1;
                float farClipPlane = 200;
                float aspectRatio = graphicsDevice.Viewport.Width / (float)graphicsDevice.Viewport.Height;

                return Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
            }
        }

        public Camera(GraphicsDevice graphicsDevice)
        {
            this.graphicsDevice = graphicsDevice;
        }

        public void Update(GameTime gameTime)
        {
            // We'll be doing some input-based movement here
        }
    }
}
```

Powyższy kod jest bardzo podobny do kodu z `Game1` i `Robot` który przypisać macierze `BasicEffect`. 

Teraz zostanie zintegrowana nowe `Camera` klasy do naszej istniejące projekty. Najpierw, firma Microsoft będzie zmodyfikować `Robot` klasy podjęcie `Camera` wystąpienia w jego `Draw` metodę, która wyeliminuje wiele zduplikowanych kodu. Zastąp `Robot.Draw` metody z następujących czynności:

```csharp
public void Draw(Camera camera)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = GetWorldMatrix();
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;
        }

        mesh.Draw ();
    }
}
```

Następnie należy zmodyfikować `Game1.cs` pliku:

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        // New camera code
        Camera camera;

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

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

            robot = new Robot ();
            robot.Initialize (Content);

            // New camera code
            camera = new Camera (graphics.GraphicsDevice);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            robot.Update (gameTime);
            // New camera code
            camera.Update (gameTime);
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            // New camera code
            robot.Draw (camera);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            // New camera code
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;

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
    }
}
```

Zmiany `Game1` z poprzedniej wersji (które są oznaczone symbolem `// New camera code` ) są:

- `Camera` pole w `Game1`
- `Camera` Podczas tworzenia wystąpienia w `Game1.Initialize`
- `Camera.Update` Wywołanie w `Game1.Update`
- `Robot.Draw` obecnie trwa `Camera` parametru
- `Game1.Draw` używa teraz `Camera.ViewMatrix` i `Camera.ProjectionMatrix`

## <a name="moving-the-camera-with-input"></a>Przenoszenie aparatu przy użyciu danych wejściowych

Do tej pory dodaliśmy `Camera` jednostki, ale nie dokonano żadnych zmian, które służy do zmiany zachowania w czasie wykonywania. Zachowanie, dzięki czemu użytkownik zostanie dodany do:

- Touch lewej części ekranu, aby włączyć aparatu w lewo
- Touch z prawej strony ekranu, aby włączyć aparatu w prawo
- Touch Centrum ekranu, aby przejść do przodu aparatu

### <a name="making-lookat-relative"></a>Czeki przekształcenie względne

Najpierw będziemy informować `Camera` klasy, aby uwzględnić `angle` pola, które będą używane do zestawu kierunek `Camera` jest skierowany. Obecnie naszych `Camera` Określa kierunek jest on skierowany za pośrednictwem lokalnej `lookAtVector`, który jest przypisany do `Vector3.Zero`. Innymi słowy naszych `Camera` zawsze szuka w źródle. Jeśli aparat jest przenoszony, kąt, który jest skierowany aparatu również zostanie zmieniona:

![](part3-images/image11.gif "Jeśli aparat jest przenoszony, następnie kąta, który aparat jest ukierunkowane spowoduje również zmianę")

Chcemy `Camera` do zwrócone niezależnie od jego położenie — tym samym kierunku co najmniej do czasu firma Microsoft implementują logikę obracanie `Camera` przy użyciu danych wejściowych. Pierwsza zmiana należy dostosować `lookAtVector` zmienną zależeć od naszych bieżącej lokalizacji, a nie na położenie bezwzględne wyglądającej:

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    // Let's start at X = 0 so we're looking at things head-on
    Vector3 position = new Vector3(0, 20, 10);

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

Powoduje to `Camera` wyświetlanie bezpośrednio na świecie. Zwróć uwagę, że początkowe `position` wartości został zmodyfikowany w celu `(0, 20, 10)` więc `Camera` jest wyśrodkowane na osi X. Uruchamianie gier Wyświetla:

![](part3-images/image12.png "Uruchamianie gry zawiera tego widoku")

### <a name="creating-an-angle-variable"></a>Tworzenie kąta zmiennej

`lookAtVector` Zmiennej określa kąt naszych aparatu jest wyświetlana. Obecnie jest stałą wyświetlić dół ujemna osi Y i nieco przechylono w dół (z `-.5f` wartość Z). Utworzymy `angle` zmiennej, która będzie używana w celu dostosowania `lookAtVector` właściwości. 

We wcześniejszych sekcjach tego przewodnika firma Microsoft wykazały, że macierzy może służyć do obracania, w jaki sposób obiekty są rysowane. Możemy również użyć obracania kierunków, takich jak macierze `lookAtVector` przy użyciu `Vector3.Transform` metody. 

Dodaj `angle` pól i zmodyfikuj `ViewMatrix` właściwości w następujący sposób:

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    Vector3 position = new Vector3(0, 20, 10);

    float angle;

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            // We'll create a rotation matrix using our angle
            var rotationMatrix = Matrix.CreateRotationZ (angle);
            // Then we'll modify the vector using this matrix:
            lookAtVector = Vector3.Transform (lookAtVector, rotationMatrix);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

### <a name="reading-input"></a>Odczytywanie danych wejściowych

Nasze `Camera` jednostki można teraz pełni kontrolowane przez jego położenie i zmienne kąt — potrzebujemy je zmienić zgodnie z danych wejściowych.

Po pierwsze, możemy uzyskać `TouchPanel` stanu, aby dowiedzieć się, gdy użytkownik jest dotknięcie ekranu. Aby uzyskać więcej informacji na temat używania `TouchPanel` , zobacz [dokumentacja interfejsu API TouchPanel](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel).

Jeśli użytkownik zachodzi na po lewej stronie trzeciej, firma Microsoft będzie dostosować `angle` wartość Tak `Camera` obraca się po lewej, i jeśli użytkownik zachodzi na prawo trzeciej, firma Microsoft będzie obracania inny sposób. Jeśli użytkownik jest dotknięcie w trzecim polu w środkowej części ekranu, a następnie możemy przeniesienie `Camera` do przodu.

Najpierw dodaj using instrukcji, aby zakwalifikować `TouchPanel` i `TouchCollection` klas w `Camera.cs`:

```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

Następnie należy zmodyfikować `Update` metody odczytu panelu touch i odpowiednio `angle` i `position` zmienne odpowiednio:

```csharp
public void Update(GameTime gameTime)
{
    TouchCollection touchCollection = TouchPanel.GetState();

    bool isTouchingScreen = touchCollection.Count > 0;
    if (isTouchingScreen)
    {
        var xPosition = touchCollection [0].Position.X;

        float xRatio = xPosition / (float)graphicsDevice.Viewport.Width;

        if (xRatio < 1 / 3.0f)
        {
            angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
        else if (xRatio < 2 / 3.0f)
        {
            var forwardVector = new Vector3 (0, -1, 0);

            var rotationMatrix = Matrix.CreateRotationZ (angle);
            forwardVector = Vector3.Transform (forwardVector, rotationMatrix);

            const float unitsPerSecond = 3;

            this.position += forwardVector * unitsPerSecond *
                (float)gameTime.ElapsedGameTime.TotalSeconds ;
        }
        else
        {
            angle -= (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
    }
}
```

Teraz `Camera` będzie odpowiadać wprowadzania dotykowego:

![](part3-images/image1.gif "Teraz aparatu będzie odpowiadać wprowadzania dotykowego")

Metoda aktualizacji rozpoczyna się przez wywołanie metody `TouchPanel.GetState`, która zwraca zbiór poprawek. Mimo że `TouchPanel.GetState` może zwracać wiele punktów touch, firma Microsoft będzie tylko martwić pierwszego dla uproszczenia.

Jeśli użytkownik jest dotknięcie ekranu, kod sprawdza sprawdzać, czy pierwszy touch po lewej stronie, środka lub prawej trzeci ekranu. Lewy i prawy trzecie Obróć aparatu, zwiększ lub Zmniejsz `angle` zmiennej zgodnie z `TotalSeconds` (tak, aby gry działa tak samo, niezależnie od tego, szybkość klatek).

Jeśli użytkownik dotknięcie innej Centrum ekranu, następnie aparatu zostanie przesunięty do przodu. W tym celu najpierw uzyskiwania wektorów do przodu, który jest wstępnie zdefiniowany jako skierowana ujemna osi Y, a następnie obracać o macierzy utworzone za pomocą `Matrix.CreateRotationZ` i `angle` wartość. Na koniec `forwardVector` jest stosowany do `position` przy użyciu `unitsPerSecond` współczynnik.

## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano sposób przenoszenia i Obróć `Models` w 3D miejsce przy użyciu `Matrices` i `BasicEffect.World` właściwości. Ten formularz przepływ stanowi podstawę do przenoszenia obiektów w grach 3W. Ten przewodnik obejmuje również implementowania `Camera` jednostki do wyświetlania na świecie z dowolnej pozycji i kąta.

## <a name="related-links"></a>Linki pokrewne

- [MonoGame interfejsu API łącza](http://www.monogame.net/documentation/?page=api)
- [Zakończono projekt (przykład)](https://developer.xamarin.com/samples/monodroid/MonoGame3DCamera/)
