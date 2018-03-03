---
title: "Wydajność i efekty wizualne z CCRenderTexture"
description: "CCRenderTexture umożliwia deweloperom poprawić wydajność ich gry CocosSharp zmniejszając wywołań rysowania i umożliwia tworzenie efektów wizualnych. W tym przewodniku towarzyszy próbki CCRenderTexture zapewnienie praktyczne przykład efektywnie korzystać z tej klasy."
ms.topic: article
ms.prod: xamarin
ms.assetid: F02147C2-754B-4FB4-8BE0-8261F1C5F574
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 8283c299d0e6529ef4cf8c285ec47b4d42fc682a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="performance-and-visual-effects-with-ccrendertexture"></a>Wydajność i efekty wizualne z CCRenderTexture

_CCRenderTexture umożliwia deweloperom poprawić wydajność ich gry CocosSharp zmniejszając wywołań rysowania i umożliwia tworzenie efektów wizualnych. W tym przewodniku towarzyszy próbki CCRenderTexture zapewnienie praktyczne przykład efektywnie korzystać z tej klasy._

`CCRenderTexture` Klasa udostępnia funkcje do renderowania wiele obiektów CocosSharp do pojedynczego tekstury. Raz utworzone, `CCRenderTexture` wystąpień może służyć do renderowania grafiki wydajne i wdrożenie efektów wizualnych. `CCRenderTexture` zezwala na wiele obiektów, które mają być odwzorowane tekstury jeden raz. Następnie można ten tekstury ponownie co ramki, zmniejszając łączna liczba wywołań rysowania.

Jak używać sprawdza, czy ten przewodnik `CCRenderTexture` obiekt, aby poprawić wydajność karty renderowania w grę do zebrania karty (CCG). Także przedstawiono sposób `CCRenderTexture` może służyć do przezroczystego całej jednostki. Ten przewodnik odwołuje się do `CCRenderTexture` [przykładowy projekt](https://developer.xamarin.com/samples/mobile/CCRenderTexture/).

![](ccrendertexture-images/image1.png "Ten przewodnik odwołuje się do CCRenderTexture przykładowy projekt")


# <a name="card--a-typical-entity"></a>Karta — typowe jednostki

Przed spojrzenie na sposób używania `CCRenderTexture` obiektu, firma Microsoft będzie najpierw zapoznać nad z `Card` jednostki, która zostanie użyta w tym projekcie Aby zapoznać się z `CCRenderTexture` klasy. `Card` Klasy jest typowy jednostki, zgodnie ze wzorcem jednostki opisane w temacie [przewodnik jednostki](~/graphics-games/cocossharp/entities.md). Klasa karty ma wszystkie jego składniki visual (wystąpienia `CCSprite` i `CCLabel`) wymienionych jako pola:


```csharp
class Card : CCNode
{
    bool usesRenderTexture;
    List<CCNode> visualComponents = new List<CCNode>();
    CCSprite background;
    CCSprite colorIcon;
    CCSprite monsterSprite;
    CCLabel monsterNameDisplay;
    CCLabel hpDisplay;
    CCLabel descriptionDisplay;
```

Karta wystąpienia ma być renderowany przy użyciu `CCRenderTexture`, lub za pomocą rysowania każdego składnika visual pojedynczo. Mimo że każdego składnika jest obiektem niezależne `CCNode` elementy nadrzędne system używany w jednostki sprawia, że `Card` zachowują się tak samo — co najmniej w większości przypadków. Na przykład jeśli `Card` położenia, rozmiaru lub obracać jednostki, a następnie wpływ na wszystkie zawarte obiekty visual dokonanie karty mogą być pojedynczego obiektu. Aby wyświetlić karty zachowują się tak samo, możemy zmodyfikować `GameLayer.AddedToScene` metodę, aby ustawić `useRenderTextures` zmienną `false`:

    
```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    ...
}
```

`GameLayer` Kodu nie przenosi każdego elementu wizualnego niezależnie, jeszcze każdego elementu wizualnego w `Card` jednostki jest ustawiony prawidłowo:

![](ccrendertexture-images/image1.png "Kod GameLayer przenosić niezależnie każdego elementu wizualnego, ale każdy element wizualny, w ramach jednostki karty jest ustawiony prawidłowo")

Próbki jest kodowane do udostępnienia dwa problemy, które mogą wystąpić podczas każdego składnika visual renderowany:

- Wydajności mogą występować z powodu wielu wywołań rysowania
- Niektóre efekty wizualne, takie jak przezroczystości, nie można zaimplementować dokładnie, jak przeanalizujemy później


## <a name="card-draw-calls"></a>Karta wywołań rysowania

Naszego kodu jest uproszczenie co można znaleźć w pełnym *do zebrania gra* (CCG) takie jak "Zbieranie Magic:" lub "Hearthstone". Nasze gry tylko jednocześnie zawiera trzy karty i ma niewielkiej liczby możliwych jednostki (blue zielony i kolor pomarańczowy). Z kolei pełne gry może mieć ponad dwudziestu kart na ekranie w danym momencie, i odtwarzacze może mieć setki kart, które można wybierać podczas tworzenia ich talii. Mimo że nasze gry nie obecnie doświadczają problemy z wydajnością, może być pełny gry z implementacją podobne.

CocosSharp zawiera niektóre analizę wydajności renderowania w przypadku wystawianego wywołań rysowania wykonywane na ramki. Nasze `GameLayer.AddedToScene` zestawy metody `GameView.Stats.Enabled` do `true`, co pokazano w lewym dolnym rogu ekranu, informacje o wydajności:

![](ccrendertexture-images/image2.png "Metoda GameLayer.AddedToScene ustawia GameView.Stats.Enabled na wartość true, co pokazano w lewym dolnym rogu ekranu, informacje o wydajności")

Zwróć uwagę, pomimo o trzy karty na ekranie, mając dziewiętnastu wywołań rysowania (każdej karty wyników w sześć wywołań, pisania tekstu wyświetlanie kont informacji o wydajności dla jednej). Wywołań rysowania mieć znaczący wpływ na wydajność grę, więc CocosSharp udostępnia kilka sposobów, aby zmniejszyć ich. Techniki co opisano w [przewodnik CCSpriteSheet](~/graphics-games/cocossharp/ccspritesheet.md). Jest użycie innej techniki `CCRenderTexture` do zmniejszenia każdej jednostki w dół, aby jedno wywołanie, jak zostaną omówione w tym przewodniku.


## <a name="card-transparency"></a>Karta przezroczystości

Nasze `Card` zawiera jednostki `Opacity` przezroczystość kontroli, jak pokazano w poniższy fragment kodu dla właściwości:


```csharp
public override byte Opacity
{
    get
    {
        return base.Opacity;
    }
    set
    {
        base.Opacity = value;
        if (usesRenderTexture)
        {
            this.renderTexture.Sprite.Opacity = value;
        }
        else
        {
            foreach (var component in visualComponents)
            {
                component.Opacity = value;
            }
        }
    }
}
```

Zwróć uwagę, że obsługuje metody ustawiającej przy użyciu renderowania tekstury lub indywidualnie renderowania poszczególnych składników. Aby wyświetlić jego wpływ, należy zmienić `opacity` do wartości `127` (około połowa nieprzezroczystość) w `GameLayer.AddedToScene` co spowoduje każdy składnik o `Opacity` wartość `127`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    const byte opacity = 127;
    ...
}
```

Gry będą teraz renderowania kart z niektórych przezroczystości, powoduje wyświetlenie ciemniejsze od tła jest czarny:

![](ccrendertexture-images/image3.png "Gry będą teraz renderowania kart z niektórych przezroczystości, powoduje wyświetlenie ciemniejsze od tła jest czarny")

Na pierwszy rzut oka może wyglądać tak, jakby ich karty poprawnie wprowadzono przezroczysty. Jednak zrzut ekranu przedstawia liczbę problemów visual.

Ponieważ tło naszej jest czarny, oczekujemy będzie, że każdej części naszych karty staje się ciemniejszego z powodu przezroczystość. Oznacza to staje się bardziej przejrzyste karty, ciemniejszego staje się. W nieprzezroczystość 0 `Card` będzie całkowicie przezroczysty (całkowicie czarny). Jednak niektórych części naszych karty nie stanie ciemniejszego po nieprzezroczystość została zmieniona na `127`. Nawet gorsza niektórych części naszych karty faktycznie stał się jaśniejszy podczas stał się bardziej przejrzysty. Przyjrzyjmy się części naszych kart, które zostały czarny *przed* były przezroczyste — w szczególności tekst szczegółów i czarny konturów wokół monster grafiki. Możemy umieścić te obok siebie, po widzimy jego wpływ na stosowanie przezroczystości:

![](ccrendertexture-images/image4.png "Jeśli umieszczone obok siebie, można wyświetlić jego wpływ na stosowanie przezroczystości")

Jak wspomniano powyżej, wszystkie części karty stawali ciemniejszego się coraz bardziej przejrzysty, ale w wielu obszarach, to nie jest to:

- Konspekt robota staje się jaśniejszy (przechodzi od czerni na szary)
- Tekst opisu staje się jaśniejszy (przechodzi od czerni na szary)
- Zielony część robota staje się mniej przepełniony, ale nie stanie się ciemniejszego

Aby ułatwić wizualizacji, dlaczego dzieje się tak, musimy należy pamiętać, że każdego składnika visual jest rysowane niezależnie, każdy częściowo przezroczysty. Pierwszy składnik visual rysowane jest tła karty. Kolejne elementy przezroczyste będzie rysowany u góry karty i będzie mieć wpływ tła karty. Jeśli firma Microsoft Usuń fragment tekstu z naszych karty i Przenieś w dół grafiki robota widać, jak robota ma wpływ na tle karty. Zwróć uwagę, pomarańczowy wiersza na górnej liście będą widoczne na robota i że obszaru robota, która nakłada się na niebieskie linie w Centrum karty jest wstawiany ciemniejszego:

![](ccrendertexture-images/image5.png "Zwróć uwagę, pomarańczowy wiersza na górnej liście będą widoczne na robota i że obszaru robota, która nakłada się na niebieskie linie w Centrum karty jest wstawiany ciemniejszego")

Przy użyciu `CCRenderTexture` pozwala przezroczystego całego karty bez wpływu na renderowanie pojedynczych składników w ramach karty, jak przedstawiono w dalszej części tego przewodnika.


# <a name="using-ccrendertexture"></a>Przy użyciu CCRenderTexture

Teraz, gdy określiliśmy problemy związane z indywidualnie renderowania każdego składnika, firma Microsoft będzie włączyć renderowania `CCRenderTexture` i porównaj zachowanie.

Aby włączyć renderowania `CCRenderTexture`, zmień `userRenderTextures` zmienną `true` w `GameLayer.AddedToScene`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = true;
```


## <a name="card-draw-calls"></a>Karta wywołań rysowania

Jeśli teraz możemy uruchomić grę, zajmiemy się tym wywołań rysowania zmniejszony z nineteen cztery (każda karta zmniejszony z sześciu do jednego):

![](ccrendertexture-images/image6.png "Jeśli teraz uruchomienia gry wywołań rysowania zmniejszony z nineteen cztery każdej karty zmniejszony z sześciu do jednego")

Jak wcześniej wspomniano tego rodzaju redukcji może mieć znaczący wpływ na gry z kolejnych jednostek visual na ekranie.


## <a name="card-transparency"></a>Karta przezroczystości

Raz `useRenderTextures` ma ustawioną wartość `true`, przezroczysty kart spowoduje, że inaczej:

![](ccrendertexture-images/image7.png "Po ustawieniu na wartość true, przezroczysty kart useRenderTextures spowoduje, że inaczej")

Umożliwia porównanie karty przezroczysty robot, przy użyciu tekstury renderowania (po lewej) a bez (po prawej):

![](ccrendertexture-images/image8.png "Porównanie karty przezroczysty robota za pomocą vs renderowania tekstury (po lewej) bez (po prawej)")

Najbardziej oczywisty różnice znajdują się w tekst szczegóły (czarna zamiast światła szary) i sprite robota (zmniejszonym i ciemny zamiast światła).


# <a name="ccrendertexture-details"></a>Szczegóły CCRenderTexture

Teraz, gdy firma Microsoft w tym samouczku korzyści wynikające ze stosowania `CCRenderTexture`, Spójrzmy na sposobie ich użycia w `Card` jednostki.

`CCRenderTexture` Jest kanwy, która może być elementem docelowym renderowania. Ma dwa główne różnice w porównaniu do gier ekranu:

1. `CCRenderTexture` Będzie nadal występować między nimi ramki. Oznacza to, że `CCRenderTexture` musi być renderowana tylko w przypadku wystąpienia zmian. W tym przypadku `Card` jednostki nigdy nie zmieni się, więc zostanie wyświetlony tylko jeden raz. Jeśli istnieją `Card` składników zmienione, a następnie kartę musi nastąpić jego automatyczne odświeżenie do jego `CCRenderTexture`. Na przykład jeśli wartość HP (punkty kondycji) zmieniać w przypadku ataku, następnie karty potrzebny do samej siebie, aby odzwierciedlić nową wartość HP renderowania.
1. `CCRenderTexture` Wymiary nie są związane z ekranu. A `CCRenderTexture` może być większy lub mniejszy niż rozdzielczość urządzenia. `Card` Kod tworzy `CCRenderTexture` przy użyciu rozmiaru jego ikonki tła. Karta zawiera odwołanie do `CCRenderTexture` o nazwie `renderTexture`:


```csharp
CCRenderTexture renderTexture;
```

`renderTexture` Wystąpienie pozostaje `null` do momentu `UseRenderTexture` przypisano właściwości na wartość true, które wywołuje `SwitchToRenderTexture`:


```csharp
private void SwitchToRenderTexture()
{
    // The card needs to be moved to the origin (0,0) so it's rendered on the render target. 
    // After it's rendered to the CCRenderTexture, it will be moved back to its old position
    var oldPosition = this.Position;
    // Make sure visuals are part of the card so they get rendered
    bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
    if (!areVisualComponentsAlreadyAdded)
    {
        // Temporarily add them so we can render the object:
        foreach (var component in visualComponents)
        {
            this.AddChild(component);
        }
    }
    // Create the render texture if it hasn't yet been made:
    if (renderTexture == null)
    {
        // Even though the game is zoomed in to create a pixellated look, we are using
        // high-resolution textures. Therefore, we want to have our canvas be 2x as big as 
        // the background so fonts don't appear pixellated
        var pixelResolution = background.ContentSize * 2;
        var unitResolution = background.ContentSize;
        renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
        //renderTexture.Sprite.Scale = .5f;
    }
    // We don't want the render target to be a child of this when we update it:
    if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
    {
        this.Children.Remove(renderTexture.Sprite);
    }
    // Move this instance back to the origin so it is rendered inside the render texture:
    this.Position = CCPoint.Zero;
    // Clears the CCRenderTexture
    renderTexture.BeginWithClear(CCColor4B.Transparent);
    // Visit renders this object and all of its children
    this.Visit();
    // Ends the rendering, which means the CCRenderTexture's Sprite can be used
    renderTexture.End();
    // We no longer want the individual components to be drawn, so remove them:
    foreach (var component in visualComponents)
    {
        this.RemoveChild(component);
    }
    // Move this back to its original position:
    this.Position = oldPosition;
    // add the render texture sprite to this:
    renderTexture.Sprite.AnchorPoint = CCPoint.Zero;
    this.AddChild(renderTexture.Sprite);
}
```

`SwitchToRenderTexture` Można wywołać metody zawsze, gdy musi zostać odświeżona tekstury. Może ona zostać wywołana, czy karta używa już jego `CCRenderTexture` lub jest przełączanie do `CCRenderTexture` po raz pierwszy.

Poniższe sekcje Eksploruj `SwitchToRenderTexture` metody. 


## <a name="ccrendertexture-size"></a>Rozmiar CCRenderTexture

Konstruktor CCRenderTexture wymaga dwóch zestawów wymiarów. Pierwszy steruje rozmiarem pamięci `CCRenderTexture` podczas wprowadzania, a drugi określa pikseli szerokości i wysokości jego zawartość. `Card` Tworzy wystąpienie jednostki jego `CCRenderTexture` przy użyciu tła [contentsize będzie mieć](https://developer.xamarin.com/api/property/CocosSharp.CCSprite.ContentSize/). Nasze gra `DesignResolution` z 512 przez 384, jak pokazano w `ViewController.LoadGame` w systemie iOS i `MainActivity.LoadGame` w systemie Android:


```csharp
int width = 512;
int height = 384;
// Set world dimensions
gameView.DesignResolution = new CCSizeI(width, height);
```

`CCRenderTexture` Konstruktor jest wywoływany z `background.ContentSize` jako pierwszy parametr wskazujący, że `CCRenderTexture` powinien być tak duże w tle `CCSprite`. Ponieważ tła karty `CCSprite` wysokości 200 pikseli, karta zajmie około połowy wysokość ekranu.

Drugi parametr przekazany do `CCRenderTexture` Konstruktor Określa rozdzielczość pikseli `CCRenderTexture`. Zgodnie z opisem w [przewodnik rozpoznawania CocosSharp](~/graphics-games/cocossharp/resolutions.md), szerokość i wysokość obszaru wyświetlanego w jednostkach gier często nie jest taka sama jak pikseli rozdzielczość ekranu. Podobnie CCRenderTexture może używać rozwiązania większy niż jego rozmiar, elementy wizualne wyświetlane dokładnym na urządzeniach o wysokiej rozdzielczości.

Rozdzielczość piksela jest dwukrotnie rozmiar CCRenderTexture, aby zapobiec tekst z wyglądającej podzielony na piksele:


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize * 2;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

Do porównania, firma Microsoft może zmienić wartość pixelResolution odpowiadające `background.ContentSize` (bez podwójny) i porównanie wyniku: 


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

![](ccrendertexture-images/image9.png "Aby porównać, można zmienić wartość pixelResolution, aby dopasować tło. ContentSize bez podwajając i porównać wynik")


## <a name="rendering-to-a-ccrendertexture"></a>Renderowania CCRenderTexture

Zazwyczaj obiekty widoczne w CocosSharp nie są jawnie renderowane. Zamiast tego obiekty widoczne są dodawane do `CCLayer` który jest częścią `CCScene`. Automatycznie renderuje CocosSharp `CCScene` i jego visual hierarchii w każdej ramce bez żadnego kodu renderowania wywoływane. 

Z kolei `CCRenderTexture` musi być jawnie rysowane. Renderowanie tego mogą być dzielone na trzy kroki:

1. `CCRenderTexture.BeginWithClear` Po wywołaniu wskazujący, że wszystkie kolejne renderowania będzie obowiązywać wywołujący `CCRenderTexture`.
1. Obiektów dziedziczących `CCNode` (takich jak `Card` jednostek) są renderowane do `CCRenderTexture` przez wywołanie metody `Visit`.
1. `CCRenderTexture.End` Po wywołaniu renderowania tego wskazująca `CCRenderTexture` została ukończona.

Dowolną liczbę obiektów mogą być odwzorowywany na `CCRenderTexture` między jego `Begin` i `End` wywołania. Przed renderowaniem, wszystkie niezbędne obiekty widoczne są dodawane jako elementy podrzędne:


```csharp
bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
if (!areVisualComponentsAlreadyAdded)
{
    // Temporarily add them so we can render the object:
    foreach (var component in visualComponents)
    {
        this.AddChild(component);
    }
}
```

`renderTexture` Nie powinno należeć karty podczas renderowania, więc zostanie ono usunięte:


```csharp
// We don't want the render texture to be a child of the card 
// when we call Visit
if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
{
    this.RemoveChild(renderTexture.Sprite);
}
```

Teraz `Card` wystąpienia mogą się do renderowania `CCRenderTexture` wystąpienie:


```csharp
// Clears the CCRenderTexture
renderTexture.BeginWithClear(CCColor4B.Transparent);
// Visit renders this object and all of its children
this.Visit();
// Ends the rendering, which means the CCRenderTexture's Sprite can be used
renderTexture.End();
```

Po zakończeniu renderowania poszczególnych składników są usuwane i `CCRenderTexture` dodaje się ponownie.


```csharp
// We no longer want the individual components to be drawn, so remove them:
foreach (var component in visualComponents)
{
    this.RemoveChild(component);
}
// add the render target sprite to this:
this.AddChild(renderTexture.Sprite);
```

# <a name="summary"></a>Podsumowanie

Omówione w tym przewodniku `CCRenderTexture` przy użyciu `Card` jednostki, która mogła zostać użyta w grze kolekcjonowanych karty. Pokazano, jak używać `CCRenderTexture` klasę, aby zwiększyć szybkość klatek i prawidłowo zaimplementować przezroczystość całej jednostki.

Chociaż ten przewodnik używany `CCRenderTexture` zawartych w jednostki, ta klasa mogą być używane do renderowania wiele jednostek lub nawet całych `CCLayer` wystąpień dla całej ekranu efekty i wydajności.

## <a name="related-links"></a>Linki pokrewne

- [Dokumentacja interfejsu API CCRenderTexture](https://developer.xamarin.com/api/type/CocosSharp.CCRenderTexture/)
- [Pełna projekt (przykład)](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)
