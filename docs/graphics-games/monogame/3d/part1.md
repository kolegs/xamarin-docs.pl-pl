---
title: "Używanie klasy modelu"
description: "Klasa modelu jest znacznie ułatwione renderowania złożonych obiektów 3W w porównaniu do tradycyjnego metody renderowania grafiki 3D. Obiekty modelu są tworzone na podstawie plików zawartości, co zapewnia łatwą integrację zawartości z żadnego kodu niestandardowego."
ms.topic: article
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: dd0f97f450d131bcbddff43ffc778b91bff6b12e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="using-the-model-class"></a>Używanie klasy modelu

_Klasa modelu jest znacznie ułatwione renderowania złożonych obiektów 3W w porównaniu do tradycyjnego metody renderowania grafiki 3D. Obiekty modelu są tworzone na podstawie plików zawartości, co zapewnia łatwą integrację zawartości z żadnego kodu niestandardowego._

Interfejs API MonoGame zawiera `Model` klasy, która może służyć do przechowywania danych załadowane z zawartości pliku i wykonywania renderowania. Pliki modelu może być bardzo proste (na przykład stałe trójkąt kolorowe) lub może obejmować informacje dotyczące renderowania złożonych, w tym teksturowania i oświetlenia.

W tym przewodniku zastosowano [modelu 3D robotem](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) i obejmuje następujące czynności:

 - Uruchamianie nowego projektu gier
 - Tworzenie XNBs dla modelu i jego tekstury
 - W tym XNBs w projekcie gier
 - Rysowanie modelu 3D
 - Rysowanie wielu modeli

Po zakończeniu naszych projektu będzie wyglądać następująco:

![](part1-images/image1.png "Po zakończeniu projektu pojawia się jako to")


# <a name="creating-an-empty-game-project"></a>Tworzenie pusty projekt gier

Potrzebujemy skonfigurować projekt gier najpierw o nazwie MonoGame3D. Aby uzyskać informacje dotyczące tworzenia nowego projektu MonoGame, zobacz [ten przewodnik dotyczący tworzenia projektu Monogame Cross Platform](~/graphics-games/monogame/introduction/part1.md).

Przed przejściem możemy zweryfikować, że projekt zostanie otwarty i wdraża poprawnie. Raz wdrożonym mamy powinna zostać wyświetlona pusta niebieski ekran:

![](part1-images/image2.png "Raz wdrożonej powinna zostać wyświetlona projektanta ekranu blue pustym")


# <a name="including-the-xnbs-in-the-game-project"></a>W tym XNBs w projekcie gier

Format pliku .xnb jest standardowym rozszerzeniem wbudowanej zawartości (zawartości, który został utworzony przez [MonoGame potoku narzędzia](http://www.monogame.net/documentation/?page=Pipeline)). Cała zawartość wbudowanych ma (czyli pliku fbx w przypadku naszego modelu) pliku źródłowego i docelowego pliku (.xnb). Format fbx jest wspólny format modelu 3D, można utworzyć przez aplikacje takie jak [Maya](http://www.autodesk.com/products/maya/overview) i [mieszarce](http://www.blender.org/). 

`Model` Klasy można skonstruować przez ładowanie pliku .xnb z dysku, który zawiera dane geometrii 3D.   Ten plik .xnb jest tworzony za pomocą zawartości projektu. Szablony Monogame automatycznie obejmują zawartości projektu (z rozszerzeniem .mgcp) w naszym folder zawartości. Szczegółowe omówienie narzędzia MonoGame potoku, zobacz [potok zawartość przewodnika](~/graphics-games/cocossharp/content-pipeline/index.md).

Tego przewodnika pominiemy za pośrednictwem za pomocą potoku MonoGame narzędzia i będzie używana. XNB pliki zawarte w tym miejscu. Należy pamiętać, że. Pliki XNB różnią się dla każdej platformy, dlatego należy użyć poprawny zestaw plików XNB niezależnie od platformy pracujesz.

Firma Microsoft będzie Rozpakuj [pliku Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) , dzięki czemu możemy użyć plików .xnb zawartych w naszym gry. Jeśli pracy nad projektem systemu Android, kliknij prawym przyciskiem myszy **zasoby** folderu w **WalkingGame.Android** projektu. Pracy projekt dla systemu iOS, kliknij prawym przyciskiem myszy **WalkingGame.iOS** projektu. Wybierz **Dodaj -> Dodaj pliki...**  i wybierz oba pliki .xnb w folderze platformy pracujesz.

Dwa pliki powinny być teraz częścią naszego projektu:

![](part1-images/xnbsinxs.png "Dwa pliki powinna być teraz częścią projektu")

Visual Studio for Mac nie może automatycznie ustawiona akcja kompilacji dla nowo dodanych XNBs. Dla systemu iOS, kliknij prawym przyciskiem myszy każdy z plików, a następnie wybierz **BundleResource -> Akcja kompilacji**. Dla systemu Android, kliknij prawym przyciskiem myszy każdy z plików, a następnie wybierz **AndroidAsset -> Akcja kompilacji**.

# <a name="rendering-a-3d-model"></a>Renderowanie modelu 3D

Ostatnim krokiem należy na ekranie Zobacz modelu jest, aby dodać ładowania i rysowanie kodu. W szczególności firma Microsoft będzie można wykonać następujące czynności:

 - Definiowanie `Model` wystąpienia w naszym `Game1` — klasa
 - Ładowanie `Model` wystąpienia w `Game1.LoadContent`
 - Rysowanie `Model` wystąpienia w `Game1.Draw`

Zastąp `Game1.cs` pliku kodu (który znajduje się w **WalkingGame** PCL) z następujących czynności:


```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;

    // This is the model instance that we'll load
    // our XNB into:
    Model model;

    public Game1()
    {
        graphics = new GraphicsDeviceManager(this);
        graphics.IsFullScreen = true;
                    
        Content.RootDirectory = "Content";
    }
    protected override void LoadContent()
    {
        // Notice that loading a model is very similar
        // to loading any other XNB (like a Texture2D).
        // The only difference is the generic type.
        model = Content.Load<Model> ("robot");
    }

    protected override void Update(GameTime gameTime)
    {
        base.Update(gameTime);
    }

    protected override void Draw(GameTime gameTime)
    {
        GraphicsDevice.Clear(Color.CornflowerBlue);

        // A model is composed of "Meshes" which are
        // parts of the model which can be positioned
        // independently, which can use different textures,
        // and which can have different rendering states
        // such as lighting applied.
        foreach (var mesh in model.Meshes)
        {
            // "Effect" refers to a shader. Each mesh may
            // have multiple shaders applied to it for more
            // advanced visuals. 
            foreach (BasicEffect effect in mesh.Effects)
            {
                // We could set up custom lights, but this
                // is the quickest way to get somethign on screen:
                effect.EnableDefaultLighting ();
                // This makes lighting look more realistic on
                // round surfaces, but at a slight performance cost:
                effect.PreferPerPixelLighting = true;

                // The world matrix can be used to position, rotate
                // or resize (scale) the model. Identity means that
                // the model is unrotated, drawn at the origin, and
                // its size is unchanged from the loaded content file.
                effect.World = Matrix.Identity;

                // Move the camera 8 units away from the origin:
                var cameraPosition = new Vector3 (0, 8, 0);
                // Tell the camera to look at the origin:
                var cameraLookAtVector = Vector3.Zero;
                // Tell the camera that positive Z is up
                var cameraUpVector = Vector3.UnitZ;

                effect.View = Matrix.CreateLookAt (
                    cameraPosition, cameraLookAtVector, cameraUpVector);

                // We want the aspect ratio of our display to match
                // the entire screen's aspect ratio:
                float aspectRatio = 
                    graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
                // Field of view measures how wide of a view our camera has.
                // Increasing this value means it has a wider view, making everything
                // on screen smaller. This is conceptually the same as "zooming out".
                // It also 
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                // Anything closer than this will not be drawn (will be clipped)
                float nearClipPlane = 1;
                // Anything further than this will not be drawn (will be clipped)
                float farClipPlane = 200;

                effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            }

            // Now that we've assigned our properties on the effects we can
            // draw the entire mesh
            mesh.Draw ();
        }
        base.Draw(gameTime);
    }
}
                                                                                                                 
```

Jeśli firma Microsoft uruchomić ten kod zajmiemy się tym modelu na ekranie:

![](part1-images/image8.png "Jeśli ten kod jest uruchamiana, model zostanie wyświetlony na ekranie")

Oto niektóre z ważniejsze części kodu powyżej.


## <a name="model-class"></a>Klasa modelu

`Model` Klasa jest klasą core do wykonywania renderowania 3W z zawartości plików (takich jak pliki fbx). Zawiera wszystkie informacje wymagane do renderowania, w tym geometrii 3D odwołania tekstury i `BasicEffect` wystąpień kontrolujące wartości rozmieszczania, oświetlenia i aparatu.

`Model` Sama klasa nie ma bezpośrednio zmienne pozycjonowanie od wystąpienia pojedynczego modelu mogą być renderowane w wielu lokalizacjach, jak pokażemy w dalszej części tego przewodnika.

Każdy `Model` składa się z co najmniej jeden `ModelMesh` wystąpienia, które są dostępne za pośrednictwem `Meshes` właściwości. Mimo że firma Microsoft może wziąć pod uwagę `Model` jako pojedynczy gry obiektu (na przykład robotem lub samochód), każdy `ModelMesh` mogą pochodzić z różnych `BasicEffect` wartości. Na przykład części poszczególnych sieci może stanowić etapy robotem lub koła na samochodu i firma Microsoft może przypisać `BasicEffect` Przenieś wartości pokrętła koła lub etapy. 


## <a name="basiceffect-class"></a>Klasa BasicEffect

`BasicEffect` Klasa udostępnia właściwości umożliwiające sterowanie opcje renderowania. Modyfikacja pierwszy wykonujemy do `BasicEffect` jest wywołanie `EnableDefaultLighting` metody. Jak wskazuje nazwę, dzięki temu oświetlenia domyślny, który jest bardzo przydatna do sprawdzenia, czy `Model` pojawia się w grze zgodnie z oczekiwaniami. Jeśli firma Microsoft komentarz `EnableDefaultLighting` wywołań, a następnie zajmiemy się tym model renderowany tylko jego tekstury, ale nie blask cieniowania lub odblasków:


```csharp
//effect.EnableDefaultLighting (); 
```

![](part1-images/image9.png "Model renderowany tylko jego tekstury, ale nie blask cieniowania lub odblasków")

`World` Właściwości można ustawić pozycji, obracanie i skali modelu. Kod nad używa `Matrix.Identity` wartości, co oznacza, że `Model` spowoduje, że w grze dokładnie tak jak określono w pliku fbx. Firma Microsoft będzie obejmujące macierzy i 3D współrzędne bardziej szczegółowo w [część 3](~/graphics-games/monogame/3d/part3.md), ale jako przykład możemy zmienić jego położenie `Model` zmieniając `World` właściwości w następujący sposób:


```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

Ten kod wyniki w obiekcie przenoszony w jednostkach 3:

![](part1-images/image10.png "Ten kod wyniki w obiekcie przenoszony w jednostkach 3")

Końcowe dwie właściwości, przypisane na `BasicEffect` są `View` i `Projection`. Firma Microsoft będzie obejmujące 3D kamer w [część 3](~/graphics-games/monogame/3d/part3.md), ale na przykład firma Microsoft modyfikować, zmieniając lokalnej pozycja kamery `cameraPosition` zmienną:


```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

Możemy stwierdzić, aparatu przeniósł dalsze Wstecz, co powoduje `Model` znajdujących się na mniejsze z powodu perspektywy:

![](part1-images/image11.png "Aparatu przeniósł dalsze, co w modelu, przy mniejszym z powodu perspektywy")


# <a name="rendering-multiple-models"></a>Renderowanie wielu modeli

Jak wspomniano powyżej, pojedynczy `Model` mogą być wystawiane wiele razy. Aby to ułatwić będziemy przenoszenie `Model` rysowania kod na własną metodę, która przyjmuje żądaną `Model` pozycji jako parametr. Po zakończeniu naszych `Draw` i `DrawModel` będzie wyglądać metod:


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);
    DrawModel (new Vector3 (-4, 0, 0));
    DrawModel (new Vector3 ( 0, 0, 0));
    DrawModel (new Vector3 ( 4, 0, 0));
    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));
    base.Draw(gameTime);
}
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            effect.World = Matrix.CreateTranslation (modelPosition);
            var cameraPosition = new Vector3 (0, 10, 0);
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
        }
        // Now that we've assigned our properties on the effects we can
        // draw the entire mesh
        mesh.Draw ();
    }
}
```

Powoduje to robot modelu rysowania sześć razy:

![](part1-images/image1.png "Powoduje to robot modelu rysowania sześć razy")


# <a name="summary"></a>Podsumowanie

W tym przewodniku wprowadzone przez MonoGame `Model` klasy. Obejmuje konwertowania pliku fbx na .xnb, które w Włącz można załadować do `Model` klasy. Zawiera także sposób modyfikacje `BasicEffect` może wpłynąć na wystąpienie `Model` rysowania.

## <a name="related-links"></a>Linki pokrewne

- [Odwołanie modelu MonoGame](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [Ukończono projekt (przykład)](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)
