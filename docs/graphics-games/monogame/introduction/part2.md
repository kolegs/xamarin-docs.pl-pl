---
title: Część 2 — wdrażanie WalkingGame
description: W tym przewodniku pokazano, jak dodać logikę gier i zawartości na pusty projekt do tworzenia pokaz animowany sprite przenoszenie z MonoGame wprowadzania dotykowego.
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: bc4ab2e77bfce9c9ba6043533bcfda5a359d322e
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="part-2--implementing-the-walkinggame"></a>Część 2 — wdrażanie WalkingGame

_W tym przewodniku pokazano, jak dodać logikę gier i zawartości na pusty projekt do tworzenia pokaz animowany sprite przenoszenie z MonoGame wprowadzania dotykowego._

Poprzedniej części w tym przewodniku pokazano, jak tworzenie pustych projektów MonoGame. Zostanie oprzemy się na te części poprzedniej poprzez prosty pokaz gier. Ten artykuł zawiera następujące sekcje:

- Rozpakować naszej zawartości
- Przegląd klasy MonoGame
- Renderowanie naszym pierwszym Sprite
- Tworzenie CharacterEntity
- Dodawanie CharacterEntity do gry
- Tworzenie klasy animacji
- Dodawanie animacji pierwszy do CharacterEntity
- Dodawanie przepływu na znak
- Dopasowywanie przepływu i animacji


## <a name="unzipping-our-game-content"></a>Rozpakować naszej zawartości

Zanim zaczniemy pisania kodu, należy do rozpakowania naszych gry *zawartości*. Deweloperzy gier często używany jest termin *zawartości* do odwoływania się do plików z systemem innym niż kodu, które są zwykle tworzone przez projektantów audio, gier projektantów lub artystów visual. Popularne typy zawartości obejmują pliki służące do wyświetlania elementów wizualnych, Odtwórz dźwięk i kontrolowania zachowania sztucznego analizy (AI). Z tworzenia gier zespołu perspektywy zawartości jest zazwyczaj tworzony przez zwykłym.

Zawartość użyta w tym miejscu można znaleźć [w serwisie github](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true). Potrzebujemy tych plików pobrane do lokalizacji, który firma Microsoft będzie miał dostęp w dalszej części tego przewodnika.

## <a name="monogame-class-overview"></a>Przegląd klasy MonoGame

Po pierwsze przeanalizujemy klasy MonoGame używane w przypadku renderowania podstawowe:

- `SpriteBatch` — używany do rysowania grafiki 2D do ekranu. *Ikony* są 2D elementów wizualnych, które są używane do wyświetlania obrazów na ekranie. `SpriteBatch` Obiektu może pobierać jednego sprite w czasie między jego `Begin` i `End` metod lub wielu ikony można grupować, lub *wsadów*.
- `Texture2D` — reprezentuje obiekt obrazu w czasie wykonywania. `Texture2D` wystąpienia często są tworzone na podstawie formatów plików takich jak PNG lub bmp, mimo że ich można również tworzyć dynamicznie w czasie wykonywania. `Texture2D` wystąpienia są używane podczas renderowania z `SpriteBatch` wystąpień.
- `Vector2` — reprezentuje pozycji 2D układu współrzędnych, która jest często używany do rozmieszczania obiektów visual. Zawiera również MonoGame `Vector3` i `Vector4` , ale tylko używamy `Vector2` w tym przewodniku.
- `Rectangle` — obszar pełnego z pozycji, szerokość i wysokość. Będziemy używać to do definiowania część naszego `Texture2D` do renderowania, gdy firma Microsoft rozpoczęcie pracy z animacji.

Możemy również zauważyć, że MonoGame nie jest obsługiwany przez firmę Microsoft — niezależnie od jego przestrzeni nazw. `Microsoft.Xna` Przestrzeni nazw jest używana w MonoGame ułatwiające migrację istniejących projektów XNA do MonoGame.

## <a name="rendering-our-first-sprite"></a>Renderowanie naszym pierwszym Sprite

Następnie firma Microsoft narysuje pojedynczego sprite do ekranu pokazują, jak wykonać 2D renderowania w MonoGame.

### <a name="creating-a-texture2d"></a>Tworzenie Texture2D

Należy utworzyć `Texture2D` wystąpienie do użycia podczas renderowania naszych sprite. Cała zawartość gier ostatecznie znajduje się w folderze o nazwie **zawartości,** znajduje się w projekcie specyficzne dla platformy. MonoGame udostępnionych projektów nie może zawierać zawartości, ponieważ zawartość należy użyć akcji kompilacji specyficzne dla platformy. Deweloperzy CocosSharp znajdziesz folder zawartości znane pojęcia — znajdują się w tym samym miejscu w projektach zarówno CocosSharp i MonoGame. Folder zawartości znajdują się w projekcie systemu iOS i znajduje się w folderze zasobów w projekcie systemu Android.

Aby dodać zawartość naszych grę, kliknij prawym przyciskiem myszy **zawartości** i wybierz polecenie **Dodaj > Dodaj pliki...** Przejdź do lokalizacji, w którym został wyodrębniony plik content.zip i wybierz **charactersheet.png** pliku. Jeśli pojawi się pytanie o tym, jak dodać plik do folderu, należy wybrać możemy **kopiowania** opcji:

![](part2-images/image1.png "Jeśli pojawi się pytanie o tym, jak dodać plik do folderu, wybierz opcję Kopiuj")

Folder zawartości zawiera teraz plik charactersheet.png:

![](part2-images/image2.png "Folder zawartości zawiera teraz plik charactersheet.png")

Następnie dodamy kod, aby załadować plik charactersheet.png i utworzyć `Texture2D`. Aby zrobić to open `Game1.cs` pliku i dodaj następujące pole do klasy Game1.cs:

```csharp
Texture2D characterSheetTexture;
```

Następnie utworzymy `characterSheetTexture` w `LoadContent` metody. Przed dokonaniem jakichkolwiek modyfikacji `LoadContent` metody wygląda następująco:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

Należy pamiętać, domyślny projekt już tworzy `spriteBatch` wystąpienia firmie Microsoft. Będziemy używać to później, ale firma Microsoft nie będzie dodanie jakiegokolwiek dodatkowego kodu, aby przygotować `spriteBatch` do użycia. Z drugiej strony naszych `spriteSheetTexture` wymaga inicjowania, dzięki czemu można będzie możemy zainicjować go po `spriteBatch` utworzeniu:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
    {
        characterSheetTexture = Texture2D.FromStream (this.GraphicsDevice, stream);

    }
}
```

Teraz, gdy mamy `SpriteBatch` wystąpienia i `Texture2D` dodamy naszego kodu renderowania do wystąpienia `Game1.Draw` metody:

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    spriteBatch.Begin ();

    Vector2 topLeftOfSprite = new Vector2 (50, 50);
    Color tintColor = Color.White;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);

    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Teraz uruchomione gry zawiera pojedynczy sprite wyświetlanie tekstury utworzone na podstawie charactersheet.png:

![](part2-images/image3.png "Teraz uruchomione gry zawiera pojedynczy sprite wyświetlanie tekstury utworzone na podstawie charactersheet.png")

## <a name="creating-the-characterentity"></a>Tworzenie CharacterEntity

Do tej pory dodaliśmy kodu do renderowania pojedynczego sprite do ekranu; Jednak jeśli możemy opracowanie pełnej gry bez tworzenia innych klas, firma Microsoft może działać na problemy z kodem organizacji.

### <a name="what-is-an-entity"></a>Co to jest jednostką?

Typowe wzorzec służący do organizowania gier kodu jest Utwórz nową klasę dla każdej gry *jednostki* typu. Jednostki w opracowywaniu gier jest obiekt, który może zawierać następujące czynniki (nie wszystkie są wymagane):

- Element wizualny, takie jak sprite, tekst lub modelu 3D
- Fizycznych lub co zachowanie ramki, takich jak jednostki patrolling Ustaw ścieżkę lub znak player odpowiada jako danych wejściowych
- Utworzona i zniszczona dynamicznie, takie jak wyświetlaniu zasilania i są zbierane przez odtwarzacz

Jednostki systemy organizacji może być skomplikowane i wiele gier aparaty oferować klasy pomagające w zarządzaniu jednostek. Firma Microsoft będzie można wdrożenie bardzo proste jednostki systemu, więc warto zauważyć, czy pełna gry zwykle wymagają więcej organizacji w części dewelopera.


### <a name="defining-the-characterentity"></a>Definiowanie CharacterEntity

Nasze jednostki, która Zadzwonimy `CharacterEntity`, ma następującą charakterystykę:

- Możliwość ładowania własną `Texture2D`
- Możliwość renderowania, w tym zawierających wywołania metody aktualizowania jednoosiowych animacji
- `X` i właściwości Y, aby kontrolować znaku na pozycji.
- Możliwość zaktualizowanie — w szczególności się, aby odczytać wartości z dotykowego ekranu i odpowiednio dostosować położenie.

Aby dodać `CharacterEntity` naszych grę, kliknij prawym przyciskiem myszy lub sterowania, kliknij pozycję **WalkingGame** projekt i wybierz **Dodaj > Nowy plik...** . Wybierz **pustą klasę** opcji i nazwę nowego pliku **CharacterEntity**, następnie kliknij przycisk **nowy**.

Najpierw dodamy przez `CharacterEntity` załadować `Texture2D` jak również, aby zwrócić się. Zostaną zmodyfikowane, nowo dodany `CharacterEntity.cs` plików w następujący sposób:

```csharp
using System;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Input.Touch;


namespace WalkingGame
{
    public class CharacterEntity
    {
        static Texture2D characterSheetTexture;

        public float X
        {
            get;
            set;
        }

        public float Y
        {
            get;
            set;
        }

        public CharacterEntity (GraphicsDevice graphicsDevice)
        {
            if (characterSheetTexture == null)
            {
                using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
                {
                    characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
            Color tintColor = Color.White;
            spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);
        }
    }
}
```

Powyższy kod dodaje odpowiedzialność rozmieszczania, renderowania i ładowanie zawartości do `CharacterEntity`. Załóżmy Poświęć chwilę, aby zwrócić uwagę pewne kwestie w powyższym kodzie.

### <a name="why-are-x-and-y-floats"></a>Dlaczego są X i Y elementów przestawnych?

Deweloperzy, którzy dopiero zaczynasz korzystać z gier programowanie może zastanawiasz się, dlaczego `float` typ jest używany w przeciwieństwie do `int` lub `double`. Przyczyną jest najbardziej typowe dla pozycjonowanie w kodzie renderowania niskiego poziomu 32-bitową wartość. Typ double zajmuje 64-bitowy precyzja, rzadko potrzebnego do rozmieszczania obiektów. Gdy różnica 32-bitowe mogą wydawać się nieważny, wiele nowoczesnych gier obejmują grafiki, które wymagają przetwarzania dziesiątki tysięcy wartości pozycji każdej ramce (30 do 60 razy na sekundę). Wycinanie ilość pamięci, które należy przenieść przy użyciu grafiki potoku o połowę może mieć znaczący wpływ na wydajność gier.

`int` — Typ danych może być odpowiednie jednostki miary rozmieszczania, ale umieszczać ograniczenia w drodze są pozycjonowane jednostek. Na przykład przy użyciu wartości będące liczbami całkowitymi ułatwia trudniejsze do zaimplementowania smooth przepływu gry obsługujące powiększyć lub kamer 3D (które są dozwolone w `SpriteBatch`).

Ponadto należy sprawdzić, czy logiki, który przenosi naszych znak po ekranie będzie to zrobić przy użyciu wartości czasu gry. Wynik mnożenia szybkość pracy, przez ile czasu upłynęło w danym przedziale rzadko spowoduje liczbą całkowitą, należy użyć `float` dla `X` i `Y`.

### <a name="why-is-charactersheettexture-static"></a>Dlaczego jest characterSheetTexture statycznych?

`characterSheetTexture` `Texture2D` Wystąpienie jest reprezentację pliku charactersheet.png środowiska wykonawczego. Innymi słowy, wartości kolorów dla każdego piksela zawiera jako wyodrębnione ze źródła **charactersheet.png** pliku. Gdyby utworzyć dwa naszych gry `CharacterEntity` wystąpienia, a następnie każdej z nich potrzebny dostęp do informacji z charactersheet.png. W takiej sytuacji nie chcemy się pociągnąć za sobą dwa razy podczas ładowania charactersheet.png koszt wydajności ani czy chcemy pamięci zduplikowane tekstury przechowywane w pamięci ram. Przy użyciu `static` pole pozwala udostępniać je `Texture2D` we wszystkich `CharacterEntity` wystąpień.

Przy użyciu `static` członków reprezentacji środowiska uruchomieniowego zawartości jest rozwiązaniem simplistic i nie usuwa problemy występujące w grach większy, takich jak zwalniania zawartości podczas przechodzenia z jednego poziomu do innego. Bardziej złożone rozwiązania, które wykraczają poza zakres tego przewodnika, mogą za pomocą potoku zawartości w MonoGame lub tworzenie system zarządzania zawartością niestandardowych.

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>Dlaczego jest SpriteBatch przekazany jako Argument Instead of utworzone przez podmiot?

`Draw` Metody zgodnie z implementacją powyżej przyjmuje `SpriteBatch` argumentu, ale z kolei `characterSheetTexture` jest utworzone przez `CharacterEntity`.

Przyczyną jest ponieważ renderowania najbardziej efektywne, jest możliwe, gdy taka sama `SpriteBatch` wystąpienia jest używany w przypadku wszystkich `Draw` wywołań i kiedy wszystkie `Draw` trwa wywołań między jednego zestawu `Begin` i `End` wywołania. Oczywiście naszych gry będą zawierać tylko wystąpienia pojedynczej jednostki, ale gry bardziej skomplikowane będą korzystać z wzorca, który zezwala na wiele jednostek używać tego samego `SpriteBatch` wystąpienia.


## <a name="adding-characterentity-to-the-game"></a>Dodawanie CharacterEntity do gry

Teraz, po dodaniu naszych `CharacterEntity` z kodem sam renderowania, możemy Zastąp kod w `Game1.cs` Aby użyć wystąpienia tej nowej jednostki. W tym firma Microsoft będzie Zastąp `Texture2D` pole z `CharacterEntity` w `Game1`:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;
    SpriteBatch spriteBatch;
    // New code:
    CharacterEntity character;

    public Game1()
    {
    ...
```

Następnie dodamy wystąpienia tej jednostki w `Game1.Initialize`:

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

Należy również usunąć `Texture2D` tworzenie za pomocą `LoadContent` , ponieważ jest teraz obsługiwany wewnątrz naszego `CharacterEntity`. `Game1.LoadContent` powinna wyglądać następująco:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

Warto zauważyć, że pomimo swojej nazwy `LoadContent` metoda nie jest jedynym miejscem, w których zawartość może zostać załadowany — wiele implementacji gry zawartości ładowania ich metoda inicjowania lub nawet swój kod aktualizacji jako zawartości, mogą być niepotrzebne do momentu osiągnięcia odtwarzacza Niektóre punkt gry.

Na koniec firma Microsoft można zmodyfikować metody rysowania w następujący sposób:


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    // We'll start all of our drawing here:
    spriteBatch.Begin ();

    // Now we can do any entity rendering:
    character.Draw(spriteBatch);
    // End renders all sprites to the screen:
    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Firma Microsoft uruchomienia gry, możemy teraz będą widzieć znak. Ponieważ X i Y domyślnie ustawione na 0, przed lewym górnym rogu ekranu znajduje się znak:

![](part2-images/image4.png "Ponieważ X i Y domyślnie ustawione na 0, następnie znak znajduje się przed lewym górnym rogu ekranu")

## <a name="creating-the-animation-class"></a>Tworzenie klasy animacji

Obecnie naszych `CharacterEntity` Wyświetla pełny **charactersheet.png** pliku. To rozmieszczenie wiele obrazów w jednym pliku jest określany jako *arkusza sprite*. Zazwyczaj sprite spowoduje, że tylko część arkusza sprite. Firma Microsoft zmodyfikuje `CharacterEntity` do renderowania stanowi część **charactersheet.png**, i ta część zmieniają się wraz z upływem czasu, aby wyświetlić jednoosiowych animacji.

Utworzymy `Animation` klasy do kontrolowania logiki i stanie animacji CharacterEntity. Klasa animacji będzie ogólne klasy, która może służyć do dowolnej jednostki nie tylko `CharacterEntity` animacji. Ultimate `Animation` zapewni klasy `Rectangle` którego `CharacterEntity` będzie używana podczas rysowania samej siebie. Również utworzymy `AnimationFrame` klasy, która będzie używana do definiowania animacji.


### <a name="defining-animationframe"></a>Definiowanie AnimationFrame

`AnimationFrame` nie będzie zawierać wszelka logika związane z animacji. Będziemy używać go tylko do przechowywania danych. Aby dodać `AnimationFrame` klasy, kliknij prawym przyciskiem myszy lub sterowania, kliknij pozycję **WalkingGame** udostępnionych projektu i wybierz **Dodaj > Nowy plik...** Wprowadź nazwę **AnimationFrame** i kliknij przycisk **nowy** przycisku. Firma Microsoft będzie zmodyfikować `AnimationFrame.cs` pliku tak, aby zawierał następujący kod:


```csharp
using System;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class AnimationFrame
    {
        public Rectangle SourceRectangle { get; set; }
        public TimeSpan Duration { get; set; }
    }
}
```

`AnimationFrame` Klasa zawiera dwa rodzaje informacji:

- `SourceRectangle` — Definiuje obszar `Texture2D` będzie on wyświetlany zgodnie `AnimationFrame`. Ta wartość jest podawana w pikselach, z górnym lewym trwa (0, 0).
- `Duration` — Definiuje czas `AnimationFrame` jest wyświetlany, gdy są używane w `Animation`.

### <a name="defining-animation"></a>Definiowanie animacji

`Animation` Klasy będzie zawierać `List<AnimationFrame>` oraz logiki do przełącznika, ramkę, do której jest aktualnie wyświetlany zgodnie z czas, jaki upłynął.

Aby dodać `Animation` klasy, kliknij prawym przyciskiem myszy lub sterowania, kliknij pozycję **WalkingGame** udostępnionych projektu i wybierz **Dodaj > Nowy plik...** . Wprowadź nazwę **animacji** i kliknij przycisk **nowy** przycisku. Firma Microsoft będzie zmodyfikować `Animation.cs` pliku tak, aby zawierał następujący kod:


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class Animation
    {
        List<AnimationFrame> frames = new List<AnimationFrame>();
        TimeSpan timeIntoAnimation;

        TimeSpan Duration
        {
            get
            {
                double totalSeconds = 0;
                foreach (var frame in frames)
                {
                    totalSeconds += frame.Duration.TotalSeconds;
                }

                return TimeSpan.FromSeconds (totalSeconds);
            }
        }

        public void AddFrame(Rectangle rectangle, TimeSpan duration)
        {
            AnimationFrame newFrame = new AnimationFrame()
            {
                SourceRectangle = rectangle,
                Duration = duration
            };

            frames.Add(newFrame);
        }

        public void Update(GameTime gameTime)
        {
            double secondsIntoAnimation = 
                timeIntoAnimation.TotalSeconds + gameTime.ElapsedGameTime.TotalSeconds;


            double remainder = secondsIntoAnimation % Duration.TotalSeconds;

            timeIntoAnimation = TimeSpan.FromSeconds (remainder);
        }
    }
}
```

Oto niektóre szczegóły z `Animation` klasy.

### <a name="frames-list"></a>ramki listy

`frames` Element członkowski jest co przechowuje dane do naszej animacji. Doda kod, który tworzy animacji `AnimationFrame` wystąpień do `frames` listy za pośrednictwem `AddFrame` metody. Może oferować bardziej szczegółowy implementacji `public` metody lub właściwości do modyfikowania `frames`, ale firma Microsoft będzie ograniczenie funkcjonalności do dodawania ramki w ramach tego przewodnika.

### <a name="duration"></a>Czas trwania

Czas trwania zwraca całkowity czas trwania `Animation,` jest uzyskiwany przez dodanie czasu trwania wszystkich zamkniętego `AnimationFrame` wystąpień. Ta wartość może być buforowane, jeśli `AnimationFrame` zostały niezmienne obiektu, ale ponieważ wprowadziliśmy AnimationFrame jako klasy, które można zmienić po dodaniu do animacji należy obliczyć tej wartości, przy każdym uzyskaniu dostępu do właściwości.

### <a name="update"></a>Aktualizacja

`Update` Można wywołać metody każdej ramki (oznacza to, co czasu gry całego jest aktualizowana). Jej celem jest zwiększenie `timeIntoAnimation` elementu członkowskiego, który służy do zwracania bieżącej ramki. Logikę `Update` uniemożliwia `timeIntoAnimation` kiedykolwiek jest większa niż długość całej animacji.

Ponieważ będziemy używać `Animation` klasy do wyświetlenia jednoosiowych animacji, a następnie chcemy mieć animacji, to po jego osiągnął koniec. Firma Microsoft można to zrobić przez sprawdzenie, czy `timeIntoAnimation` jest większy niż `Duration` wartości. Jeśli tak spowoduje cykl do początku i zachować resztę w pętli animacji.

### <a name="obtaining-the-current-frames-rectangle"></a>Uzyskiwanie prostokąt bieżącej ramki

Celem `Animation` klasy ostatecznie jest zapewnienie `Rectangle` której można użyć podczas rysowania sprite. Obecnie `Animation` klasy zawiera logikę zmiany `timeIntoAnimation` elementu członkowskiego, które będą używane do uzyskania którego `Rectangle` powinien być wyświetlany. Firma Microsoft będzie to zrobić, tworząc `CurrentRectangle` właściwości w `Animation` klasy. Skopiuj tę właściwość w `Animation` klasy:

```csharp
public Rectangle CurrentRectangle
{
    get
    {
        AnimationFrame currentFrame = null;

        // See if we can find the frame
        TimeSpan accumulatedTime;
        foreach(var frame in frames)
        {
            if (accumulatedTime + frame.Duration >= timeIntoAnimation)
            {
                currentFrame = frame;
                break;
            }
            else
            {
                accumulatedTime += frame.Duration;
            }
        }

        // If no frame was found, then try the last frame, 
        // just in case timeIntoAnimation somehow exceeds Duration
        if (currentFrame == null)
        {
            currentFrame = frames.LastOrDefault ();
        }

        // If we found a frame, return its rectangle, otherwise
        // return an empty rectangle (one with no width or height)
        if (currentFrame != null)
        {
            return currentFrame.SourceRectangle;
        }
        else
        {
            return Rectangle.Empty;
        }
    }
}
```

## <a name="adding-the-first-animation-to-characterentity"></a>Dodawanie animacji pierwszy do CharacterEntity

`CharacterEntity` Będzie zawierać animacji przejście i stałego, a także odwołanie do bieżącego `Animation` będzie wyświetlany.

Najpierw dodamy nasz pierwszy `Animation,` której użyjemy przetestować funkcje, jak podano w do tej pory. Do klasy CharacterEntity Dodajmy następujące elementy:

```csharp
Animation walkDown;
Animation currentAnimation;
```

Następnie zdefiniujmy `walkDown` animacji. To zmodyfikować `CharacterEntity` konstruktora w następujący sposób:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Jak wspomniano wcześniej, należy wywołać `Animation.Update` na podstawie czasu animacji do odtwarzania. Należy również przypisać `currentAnimation`. Aby firma Microsoft będzie teraz przypisać `currentAnimation` do `walkDown`, zastępując firma Microsoft będzie można tego kodu później podczas wprowadzania logiki przenoszenia. Dodamy `Update` metodę `CharacterEntity` w następujący sposób:


```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Teraz, gdy mamy `currentAnimation` jest przypisany i zaktualizować, możemy go używać do wykonywania naszych rysunku. Firma Microsoft będzie zmodyfikować `CharacterEntity.Draw` w następujący sposób:

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

Ostatni krok, aby pierwsze `CharacterEntity` animacji jest wywołanie nowo dodanego `Update` metody z `Game1`. Modyfikowanie `Game1.Update` w następujący sposób:

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

Teraz `CharacterEntity` zostanie odtworzona jego `walkDown` animacji:

![](part2-images/image5.gif "Teraz CharacterEntity odtwarzania animacji walkDown")

## <a name="adding-movement-to-the-character"></a>Dodawanie przepływu na znak

Następnie które będą dodawane przepływu naszych znak za pomocą formantów touch. Gdy użytkownik krawędzi ekranu, znak zostanie przesunięty w kierunku punktu, w którym jest dotknięciu ekranu. Jeśli żadne poprawki nie wykryto, a następnie znak będzie znajdować się w miejscu.

### <a name="defining-getdesiredvelocityfrominput"></a>Definiowanie GetDesiredVelocityFromInput

Będziemy używać w MonoGame `TouchPanel` klasy, która zawiera informacje o bieżącym stanie ekran dotykowy. Dodajmy metodę, która będzie sprawdzać `TouchPanel` i zwracać prędkość żądaną naszych znaków:


```csharp
Vector2 GetDesiredVelocityFromInput()
{
    Vector2 desiredVelocity = new Vector2 ();

    TouchCollection touchCollection = TouchPanel.GetState();

    if (touchCollection.Count > 0)
    {
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;

        if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)
        {
            desiredVelocity.Normalize();
            const float desiredSpeed = 200;
            desiredVelocity *= desiredSpeed;
        }
    }

    return desiredVelocity;
}
```

`TouchPanel.GetState` Metoda zwraca `TouchCollection` zawierający informacje o którym użytkownik jest dotknięcie ekranu. Jeśli użytkownik nie zachodzi ekranu, a następnie `TouchCollection` będzie pusty, w tym przypadku nie należy przenieść znak.

Jeśli użytkownik jest dotknięcie ekranu, możemy przeniesie znak kierunku pierwszy touch, innymi słowy, `TouchLocation` pod indeksem 0. Początkowo zaplanujemy żądaną prędkość równą różnicy między lokalizację znaku i pierwszy touch lokalizacji:

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

Poniżej znajduje się nieco matematyczne, który będzie zachowanie znak przenoszenie z szybkością. Aby ułatwić wyjaśnić, dlaczego jest to ważne, zastanówmy sytuacji, gdy użytkownik jest dotknięcie pikseli 500 ekran, po którym znajduje się znak. Pierwszy wiersz where `desiredVelocity.X` jest zestaw przypisywanej wartość 500. Jednakże, jeśli użytkownik był dotknięcie ekranu w odległości jednostki maksymalnie 100 znaków, a następnie `desiredVelocity.X `ustawiał do 100. Wynik będzie, że szybkość przepływu znak odpowiada jak daleko się, że punkt touch znajduje się od znaku. Ponieważ chcemy znak zawsze będą przenoszone z taką samą szybkością należy zmodyfikować desiredVelocity.

`if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)` Instrukcji sprawdza, czy prędkość jest nie-zero — innymi słowy, sprawdza się upewnić, że użytkownik nie zachodzi na tym samym miejscu jako bieżącego położenia znaku. Jeśli nie, trzeba ustawić szybkość znak ma stałą niezależnie od tego, jak daleko touch jest. Firma Microsoft to osiągnąć, normalizacji wektora prędkości, co powoduje jego trwa długość 1. Wektora prędkości 1 oznacza, że znak zostanie przesunięty w 1 piksela na sekundę. Firma Microsoft będzie to przyspieszyć iloczyn wartości żądaną szybkość 200.


### <a name="applying-velocity-to-position"></a>Stosowanie do położenia szybkość pracy

Prędkość zwrócony z `GetDesiredVelocityFromInput` musi zostać zastosowana na znak `X` i `Y` wartości były uwzględniane w czasie wykonywania. Firma Microsoft będzie zmodyfikować `Update` metody w następujący sposób:

```csharp
public void Update(GameTime gameTime)
{

    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;

    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Co wdrożyliśmy w tym miejscu jest nazywany *oparte na czasie* przepływu (w przeciwieństwie do *na podstawie ramki* przepływu). Wartość prędkość mnoży ruchu na podstawie czasu (w tym przypadku wartości przechowywane w `velocity` zmiennej) przez czas, jaki upłynął od ostatniej aktualizacji, który jest przechowywany w `gameTime.ElapsedGameTime.TotalSeconds`. Jeśli gry jest uruchamiane w mniejszą liczbę klatek na sekundę, czas, który upłynął między ramki rośnie — w rezultacie jest, że obiektów przy użyciu przepływu oparte na czasie będzie zawsze przenosić z taką samą szybkością, niezależnie od tego, szybkość klatek.

Jeśli teraz przeprowadzana naszych grę, zajmiemy się tym, czy znak jest na drodze do lokalizacji touch:

![](part2-images/image6.gif "Znak jest przenoszona do lokalizacji touch")

## <a name="matching-movement-and-animation"></a>Dopasowywanie przepływu i animacji

Po mamy naszych znak, przenoszenie i odtwarzanie animacji pojedynczego firma Microsoft może zdefiniować pozostałą część naszego animacji, a następnie użyj ich, aby odzwierciedlić przepływu znak. Gdy Zakończono będzie mamy osiem animacje łącznie:

- Animacji przejście w górę, dół, lewo i w prawo
- Animacji dla stałego nadal i skierowany w górę, dół, lewo i w prawo

### <a name="defining-the-rest-of-the-animations"></a>Definiowanie reszty animacji

Najpierw dodamy `Animation` wystąpień do `CharacterEntity` klasy dla wszystkich naszych animacji, w tym samym miejscu, w którym dodano `walkDown`. Po tym, jak `CharacterEntity` będzie mieć następujące `Animation` członków:

```csharp
Animation walkDown;
Animation walkUp;
Animation walkLeft;
Animation walkRight;

Animation standDown;
Animation standUp;
Animation standLeft;
Animation standRight;

Animation currentAnimation;
```

Teraz zdefiniujemy animacji `CharacterEntity` konstruktora w następujący sposób:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkUp = new Animation ();
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (160, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (176, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkLeft = new Animation ();
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (64, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (80, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkRight = new Animation ();
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (112, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (128, 0, 16, 16), TimeSpan.FromSeconds (.25));

    // Standing animations only have a single frame of animation:
    standDown = new Animation ();
    standDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standUp = new Animation ();
    standUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standLeft = new Animation ();
    standLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standRight = new Animation ();
    standRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Należy zauważyć, że powyższe kodu została dodana do `CharacterEntity` konstruktora, aby zachować tego przewodnika krótszy. Gry będą zazwyczaj rozdzielić definicji animacji znak do ich własnych klas lub załadować te informacje z formatu danych, takich jak XML lub JSON.

Następnie firma Microsoft będzie Dostosuj logikę do użycia animacje zgodnie z kierunkiem znak jest przenoszona lub zgodnie z ostatnich animacji, jeśli znak właśnie została zatrzymana. Aby to zrobić, firma Microsoft będzie zmodyfikować `Update` metody:


```csharp
public void Update(GameTime gameTime)
{
    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;


    if (velocity != Vector2.Zero)
    {
        bool movingHorizontally = Math.Abs (velocity.X) > Math.Abs (velocity.Y);
        if (movingHorizontally)
        {
            if (velocity.X > 0)
            {
                currentAnimation = walkRight;
            }
            else
            {
                currentAnimation = walkLeft;
            }
        }
        else
        {
            if (velocity.Y > 0)
            {
                currentAnimation = walkDown;
            }
            else
            {
                currentAnimation = walkUp;
            }
        }
    }
    else
    {
        // If the character was walking, we can set the standing animation
        // according to the walking animation that is playing:
        if (currentAnimation == walkRight)
        {
            currentAnimation = standRight;
        }
        else if (currentAnimation == walkLeft)
        {
            currentAnimation = standLeft;
        }
        else if (currentAnimation == walkUp)
        {
            currentAnimation = standUp;
        }
        else if (currentAnimation == walkDown)
        {
            currentAnimation = standDown;
        }
        else if (currentAnimation == null)
        {
            currentAnimation = standDown;
        }

        // if none of the above code hit then the character
        // is already standing, so no need to change the animation.
    }

    currentAnimation.Update (gameTime);
}
```

Kod, aby przełączyć animacji jest podzielony na dwa bloki. Pierwszy sprawdza, czy prędkość nie jest równa `Vector2.Zero` — informuje NAS czy porusza się znak. Jeśli znak jest przenoszenie, a następnie można przyjrzymy się `velocity.X` i `velocity.Y` wartości, aby ustalić którym jednoosiowych rozpoczęcia odtwarzania animacji.

Jeśli znak nie jest w ruchu, a następnie chcemy ustawić znak `currentAnimation` do animacji stałego —, ale tylko przejdziemy go Jeśli `currentAnimation` jest przejście animacji lub animacji nie została ustawiona. Jeśli `currentAnimation` nie jest jedną z czterech jednoosiowych animacji, znak już jest stały, więc nie należy zmienić `currentAnimation`.

Wynik tego kodu jest znak zostanie prawidłowo wtedy, gdy przejście czy stają przed ostatnim kierunku, w którym został przejście, gdy przestanie on:

![](part2-images/image7.gif "Wynik tego kodu jest znak zostanie prawidłowo wtedy, gdy przejście czy stają przed ostatnim kierunku, w którym został przejście, gdy przestanie on")

## <a name="summary"></a>Podsumowanie

W tym poradniku pokazano sposób pracy z MonoGame, aby utworzyć i platform gier z ikony, przenoszenie obiektów wejściowych wykrywania i animacji. Obejmuje tworzenie klasy animacji ogólnego przeznaczenia. On również pokazano, jak Utwórz jednostkę znak służący do organizowania logika kodu.

## <a name="related-links"></a>Linki pokrewne

- [Zasób obrazu CharacterSheet (przykład)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [Przejście pełną gry (przykład)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
