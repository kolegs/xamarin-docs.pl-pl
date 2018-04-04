---
title: Przy użyciu CocosSharp w platformy Xamarin.Forms
description: CocosSharp można dodać do aplikacji dla zaawansowanych wizualizacji dokładne kształtu, obrazu i renderowanie tekstu
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/03/2016
ms.openlocfilehash: 5fcc3405780e0c5e8a0e8d32caf35abf59808c8e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="using-cocossharp-in-xamarinforms"></a>Przy użyciu CocosSharp w platformy Xamarin.Forms

_CocosSharp można dodać do aplikacji dla zaawansowanych wizualizacji dokładne kształtu, obrazu i renderowanie tekstu_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**Rozwija 2016: Cocos # w platformy Xamarin.Forms**

## <a name="overview"></a>Omówienie

CocosSharp jest elastyczne, zaawansowanych technologii wyświetlania grafiki, odczytywanie wprowadzania dotykowego odtwarzanie zawartości audio i zarządzanie nimi. W tym przewodniku objaśniono sposób dodawania CocosSharp do aplikacji platformy Xamarin.Forms. Obejmuje on następujące czynności:

* [Co to jest CocosSharp?](#what)
* [Dodawanie pakietów CocosSharp Nuget](#nuget)
* [Wskazówki: Dodawanie CocosSharp do aplikacji platformy Xamarin.Forms](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>Co to jest CocosSharp?

[CocosSharp](~/graphics-games/cocossharp/index.md) jest aparatem gier typu open source, który jest dostępny na platformie Xamarin.
CocosSharp jest wydajność środowiska uruchomieniowego bibliotekę, która obejmuje następujące funkcje:

* Przy użyciu renderowanie obrazu [CCSprite — klasa](https://developer.xamarin.com/api/type/CocosSharp.CCSprite/)
* Za pomocą renderowania kształtu [CCDrawNode — klasa](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
* Przy użyciu logiki w każdej ramce [CCNode.Schedule — metoda](https://developer.xamarin.com/api/member/CocosSharp.CCNode.Schedule/p/System.Action%7BSystem.Single%7D/)
* Za pomocą zarządzania zawartością (ładowanie i zwalnianie zasobów, takich jak pliki PNG) [CCTextureCache — klasa](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
* Animacje przy użyciu [CCAction — klasa](https://developer.xamarin.com/api/type/CocosSharp.CCAction/)

CocosSharp w głównym celem jest uproszczenie procesu tworzenia gier 2W i platform; jednak również może być bardzo dodawania do aplikacji platformy Xamarin formularza. Ponieważ gry zwykle wymagają wydajne renderowanie i precyzyjna kontrola nad elementy wizualne, CocosSharp można dodać zaawansowane wizualizacje i efekty do aplikacji z systemem innym niż gry.

Platformy Xamarin.Forms oparto na natywny, specyficzne dla platformy systemów interfejsu użytkownika. Na przykład [ `Button`s](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) wyglądać inaczej w systemach iOS i Android, a nawet może różnić się od wersji systemu operacyjnego. Z kolei CocosSharp nie używa żadnych obiektów visual specyficzne dla platformy, wszystkie obiekty widoczne są wyświetlane takie same na wszystkich platformach. Oczywiście rozdzielczość i współczynnik proporcji różnią się między urządzeniami i może wpłynąć na sposób CocosSharp renderuje jego elementów wizualnych. Te informacje zostaną dokładniej omówione w dalszej części tego przewodnika.

Bardziej szczegółowe informacje można znaleźć w [sekcji CocosSharp](~/graphics-games/cocossharp/index.md).

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>Dodawanie pakietów CocosSharp Nuget

Przed użyciem CocosSharp, deweloperzy musi utworzyć kilka ma zostać dodany do projektu platformy Xamarin.Forms.
W tym przewodniku założono projekt platformy Xamarin.Forms z systemem iOS, Android i PCL projektu.
Cały kod zostanie zapisany w projekcie PCL; jednak należy dodać biblioteki dla systemu iOS i Android projektów.

Pakiet CocosSharp Nuget zawiera wszystkie obiekty, potrzebne do utworzenia obiektów CocosSharp.
Pakiet nuget CocosSharp.Forms zawiera `CocosSharpView` klasy, która jest używana do hosta CocosSharp w platformy Xamarin.Forms.
Dodaj **CocosSharp.Forms** NuGet i **CocosSharp** zostaną automatycznie dodane również.
Aby to zrobić, kliknij prawym przyciskiem myszy PCL <span class="UIItem">pakiety</span> i wybierz polecenie <span class="UIItem">Dodawanie pakietów... </span>. Wprowadź wyszukiwany termin <span class="UIItem">CocosSharp.Forms</span>, wybierz pozycję <span class="UIItem">CocosSharp dla platformy Xamarin.Forms</span>, następnie kliknij przycisk <span class="UIItem">Dodaj pakiet</span>.

![](cocossharp-images/image1.png "Dodaj pakiety okna dialogowego")

Zarówno **CocosSharp** i **CocosSharp.Forms** pakiety NuGet zostaną dodane do projektu:

![](cocossharp-images/image2.png "Folder pakietów")

Powtórz powyższe kroki dla projektów specyficzne dla platformy (na przykład iOS i Android).

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>Wskazówki: Dodawanie CocosSharp do aplikacji platformy Xamarin.Forms

Wykonaj następujące kroki, aby dodać prosty widok CocosSharp do aplikacji platformy Xamarin.Forms:

1. [Tworzenie Xamarin Forms strony](#1)
1. [Dodawanie CocosSharpView](#2)
1. [Tworzenie GameScene](#3)
1. [Dodawanie okręgu](#4)
1. [Interakcja z CocosSharp](#5)

Gdy widok CocosSharp zostały pomyślnie dodane do aplikacji platformy Xamarin.Forms, odwiedź stronę [dokumentacji CocosSharp](~/graphics-games/cocossharp/index.md) Aby dowiedzieć się więcej o tworzeniu zawartości w CocosSharp.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Tworzenie Xamarin Forms strony

CocosSharp mogą być hostowane w dowolnym kontenerze platformy Xamarin.Forms. Dla tej strony w przykładzie użyto stronę o nazwie `HomePage`. `HomePage` jest podzielony na dwie części przez `Grid` pokazanie, jak platformy Xamarin.Forms i CocosSharp mogą być renderowane jednocześnie na tej samej stronie.

Najpierw należy skonfigurować strony tak, aby zawierał `Grid` i dwa `Button` wystąpień:


```csharp
public class HomePage : ContentPage
{
public HomePage ()
    {
        // This is the top-level grid, which will split our page in half
        var grid = new Grid ();
        this.Content = grid;
        grid.RowDefinitions = new RowDefinitionCollection {
            // Each half will be the same size:
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
        };
        CreateTopHalf (grid);
        CreateBottomHalf (grid);
    }
    void CreateTopHalf(Grid grid)
    {
        // We'll be adding our CocosSharpView here:
    }
    void CreateBottomHalf(Grid grid)
    {
        // We'll use a StackLayout to organize our buttons
        var stackLayout = new StackLayout();
        // The first button will move the circle to the left when it is clicked:
        var moveLeftButton = new Button {
            Text = "Move Circle Left"
        };
        stackLayout.Children.Add (moveLeftButton);

        // The second button will move the circle to the right when clicked:
        var moveCircleRight = new Button {
            Text = "Move Circle Right"
        };
        stackLayout.Children.Add (moveCircleRight);
        // The stack layout will be in the bottom half (row 1):

        grid.Children.Add (stackLayout, 0, 1);
    }
}
```

W systemach iOS `HomePage` pojawia się, jak pokazano na poniższej ilustracji:

![](cocossharp-images/image3.png "Zrzut ekranu strony głównej")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. Dodawanie CocosSharpView

`CocosSharpView` Klasa jest używana do osadzania CocosSharp w aplikacji platformy Xamarin.Forms. Ponieważ `CocosSharpView` dziedziczy [Xamarin.Forms.View](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) klasy, zapewnia znanego interfejsu do układu i może służyć w kontenery układów takich jak [Xamarin.Forms.Grid](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). Dodaj nową `CocosSharpView` do projektu, wykonując `CreateTopHalf` metody:


```csharp
void CreateTopHalf(Grid grid)
{
    // This hosts our game view.
    var gameView = new CocosSharpView () {
        // Notice it has the same properties as other XamarinForms Views
        HorizontalOptions = LayoutOptions.FillAndExpand,
        VerticalOptions = LayoutOptions.FillAndExpand,
        // This gets called after CocosSharp starts up:
        ViewCreated = HandleViewCreated
    };
    // We'll add it to the top half (row 0)
    grid.Children.Add (gameView, 0, 0);
}
```

Inicjowanie CocosSharp nie jest bezpośrednim, więc zarejestrować zdarzenia program `CocosSharpView` zakończył jego utworzenia. Tym `HandleViewCreated` metody:


```csharp
void HandleViewCreated (object sender, EventArgs e)
{
    var gameView = sender as CCGameView;
    if (gameView != null)
    {
        // This sets the game "world" resolution to 100x100:
        gameView.DesignResolution = new CCSizeI (100, 100);
        // GameScene is the root of the CocosSharp rendering hierarchy:
        gameScene = new GameScene (gameView);
        // Starts CocosSharp:
        gameView.RunWithScene (gameScene);
    }
}
```

`HandleViewCreated` Metoda ma dwie ważne informacje, które firma Microsoft będzie spojrzenie na. Pierwsza to `GameScene` klasy, która zostanie utworzona w następnej sekcji. Należy pamiętać, że aplikacja nie zostanie skompilowany do `GameScene` jest tworzony i `gameScene` odwołania do wystąpienia został rozwiązany.

Drugi ważne są `DesignResolution` właściwość, która definiuje gry widocznym obszarze CocosSharp obiektów. `DesignResolution` Właściwości będzie przeglądał po utworzeniu `GameScene`.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. Tworzenie GameScene

`GameScene` Klasa dziedziczy w CocosSharp `CCScene`. `GameScene` jest pierwszego punktu, w którym firma Microsoft dotyczy wyłącznie CocosSharp. Kod zawarty w `GameScene` będzie działać we wszystkich aplikacjach CocosSharp, czy lub nie są przechowywane w projekcie platformy Xamarin.Forms.

`CCScene` Klasa jest elementem głównym visual renderowania CocosSharp wszystkich. Dowolny obiekt widoczne CocosSharp musi być zawarty w `CCScene`. W szczególności obiektów visual musi zostać dodany do `CCLayer` wystąpienia, a także `CCLayer` wystąpień musi zostać dodany do `CCScene`.

Wykres może pomóc wizualizacji typowe hierarchii CocosSharp:

![](cocossharp-images/image4.png "Typowy CocosSharp hierarchii")

Tylko jeden `CCScene` mogą być jednocześnie aktywne. Większość gier używanych jest wiele `CCLayer` wystąpień do sortowania zawartość, ale naszej aplikacji używa tylko jeden. Podobnie większość gier używać wielu obiektów visual, ale firma Microsoft będzie mieć tylko jeden w naszej aplikacji. Bardziej szczegółowe omówienie CocosSharp visual hierarchii znajdują się w [wskazówki BouncingGame](~/graphics-games/cocossharp/bouncing-game.md).

Początkowo `GameScene` klasy będzie niemal pusty — tylko utworzymy go do zaspokojenia odwołania w `HomePage`. Dodaj nową klasę do Twojej PCL o nazwie `GameScene`. Powinien on dziedziczyć `CCScene` klas w następujący sposób:


```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

Teraz, gdy `GameScene` jest zdefiniowany, firma Microsoft może wrócić do `HomePage` i dodawanie pola:


```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

Firma Microsoft teraz skompilować naszych projektu i uruchom go, aby wyświetlić CocosSharp systemem. Firma Microsoft nie dodano żadnych czynności, aby naszych `GameScene,` więc naszą stronę w górnej połowie czarne — domyślny kolor sceny CocosSharp:

![](cocossharp-images/image5.png "Blank GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. Dodawanie okręgu

Aplikacja ma aktualnie uruchomione wystąpienie aparatu CocosSharp, wyświetlanie pustą `CCScene`. Następnie dodamy obiekt wizualny: okręgu. `CCDrawNode` Klasa może być używana do rysowania różnych kształty geometryczne, zgodnie z opisem w [geometrii rysunku z przewodnika CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md).

Dodaj okręgu do naszej `GameScene` klasy i wystąpienia w konstruktorze, jak pokazano w poniższym kodzie:


```csharp
public class GameScene : CCScene
{
    CCDrawNode circle;
    public GameScene (CCGameView gameView) : base(gameView)
    {
        var layer = new CCLayer ();
        this.AddLayer (layer);
        circle = new CCDrawNode ();
        layer.AddChild (circle);
        circle.DrawCircle (
            // The center to use when drawing the circle,
            // relative to the CCDrawNode:
            new CCPoint (0, 0),
            radius:15,
            color:CCColor4B.White);
        circle.PositionX = 20;
        circle.PositionY = 50;
    }
}
```

Uruchamianie aplikacji teraz przedstawia koło po lewej stronie obszaru wyświetlania CocosSharp:

![](cocossharp-images/image6.png "Okrąg GameScene")


#### <a name="understanding-designresolution"></a>Opis DesignResolution

Teraz, gdy zostanie wyświetlony visual obiekt CocosSharp, możemy zbadać `DesignResolution` właściwości.

`DesignResolution` Reprezentuje szerokość i wysokość obszaru CocosSharp umieszczania i zmiany rozmiaru obiektów. Rzeczywiste rozwiązania obszaru jest mierzony w *pikseli* podczas `DesignResolution` jest mierzony w świecie *jednostki*. Na poniższym diagramie przedstawiono rozwiązanie różnych części widoku wyświetlane na telefonie iPhone 5 przy rozdzielczości ekranu w pikselach 640 x 1136:

![](cocossharp-images/image7.png "iPhone 5s projektu rozwiązania")

Na powyższym diagramie Wyświetla wymiary na zewnątrz ekranu w czarny tekst. Jednostki są wyświetlane wewnątrz diagramu w białego tekstu. Poniżej przedstawiono niektóre ważne informacje wyświetlane powyżej:

* Wyświetlanie CocosSharp pochodzi w lewym dolnym rogu. Przesunięcie w prawo zwiększa wartości X i przesunięcie w górę zwiększa wartości Y. Zwróć uwagę, że wartości Y jest odwrócony w porównaniu do niektórych innych silniki układu 2W, gdzie (0,0) jest górnego lewego do obszaru roboczego.
* Domyślne zachowanie CocosSharp jest obsługa współczynnik proporcji swoją opinię. Ponieważ pierwszy wiersz w siatce jest większa niż wysokość, CocosSharp nie wypełnia całą szerokość jego komórki pokazany kropkowanej biały prostokąt. To zachowanie można zmienić, zgodnie z opisem w [Podręcznik obsługi wielu rozwiązań w CocosSharp](~/graphics-games/cocossharp/resolutions.md).
* W tym przykładzie obszar wyświetlania 100 jednostek szerokość lub wysokość niezależnie od rozmiaru lub współczynnika proporcji jego urządzenia mają być przechowywane CocosSharp. Oznacza to, że kodu można założyć, że wyświetlane reprezentuje X = 100 powiązany z prawej CocosSharp, układ zachowanie spójności na wszystkich urządzeniach.


#### <a name="ccdrawnode-details"></a>Szczegóły CCDrawNode

Nasze używa prostej aplikacji `CCDrawNode` klasy, aby narysować okrąg. Ta klasa może być bardzo przydatne w przypadku aplikacji biznesowych, ponieważ zapewnia renderowania wektorowa geometrii — funkcja brakuje platformy Xamarin.Forms. Oprócz okręgi `CCDrawNode` klasa może być używana do Rysowanie prostokątów, krzywe, wierszy i niestandardowych wielokątów. `CCDrawNode` jest także łatwy w użyciu, ponieważ nie wymaga korzystania z plików obrazów (na przykład .png). Szczegółowe omówienie CCDrawNode znajdują się w [geometrii rysunku z przewodnika CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md).

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. Interakcja z CocosSharp

CocosSharp elementy wizualne (takie jak `CCDrawNode`) dziedziczyć `CCNode` klasy. `CCNode` zapewnia dwie właściwości, które mogą być używane do położenia obiektu względem jego elementu nadrzędnego: `PositionX` i `PositionY`. Naszego kodu obecnie używa tych dwóch właściwości do pozycji środka okręgu, jak pokazano w poniższym przykładzie:


```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

Należy pamiętać, że CocosSharp obiekty są pozycjonowane według wartości jawnie określonych pozycji, w przeciwieństwie do większości widoków platformy Xamarin.Forms, które są automatycznie ustawione zachowanie ich formantów układu nadrzędny jest.

Dodamy kod, aby zezwolić użytkownikowi na kliknij jedną z dwóch przycisków, aby przenieść okręgu w lewo lub w prawo 10 jednostek (nie pikseli, ponieważ okręgu rysuje przestrzeni jednostki world CocosSharp). Najpierw utworzymy dwóch metod publicznych w `GameScene` klasy:


```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

Następnie dodamy obsługi do dwóch przycisków w `HomePage` na odpowiadanie na kliknięcia. Po zakończeniu naszych `CreateBottomHalf` metoda zawiera następujący kod:


```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

Koło CocosSharp porusza się teraz w odpowiedzi na kliknięcia. Również wyraźnie widać, granice obszaru CocosSharp przez przeniesienie okręgu stopniu do lewej lub do prawej:

![](cocossharp-images/image8.png "GameScene z przenoszeniem okręgu")

## <a name="summary"></a>Podsumowanie

W tym przewodniku przedstawiono sposób dodawania CocosSharp do istniejącej platformy Xamarin.Forms projekt, jak utworzyć interakcji między platformy Xamarin.Forms i CocosSharp i w tym artykule omówiono zagadnienia dotyczące różnych podczas tworzenia układów w CocosSharp.

Aparat gier CocosSharp oferuje wiele funkcji i głębokość, w tym przewodniku tylko scratches czynności CocosSharp powierzchni. Deweloperzy zainteresowana dodatkowe materiały związane z CocosSharp znajduje się wiele artykułów w [sekcji CocosSharp](~/graphics-games/cocossharp/index.md).



## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API CocosSharp](https://developer.xamarin.com/api/root/CocosSharp/)
- [CocosSharpForms (przykład)](https://developer.xamarin.com/samples/xamarin-forms/CocosSharpForms/)
