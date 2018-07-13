---
title: Korzystanie z biblioteki CocosSharp w interfejsie Xamarin.Forms
description: CocosSharp można dodać dokładne kształtu, obrazu i renderowanie tekstu do aplikacji dla zaawansowanych wizualizacji
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/03/2016
ms.openlocfilehash: c823eb27552f0a42ad428ed6f36790e925079295
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998810"
---
# <a name="using-cocossharp-in-xamarinforms"></a>Korzystanie z biblioteki CocosSharp w interfejsie Xamarin.Forms

_CocosSharp można dodać dokładne kształtu, obrazu i renderowanie tekstu do aplikacji dla zaawansowanych wizualizacji_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**Rozwój 2016: Cocos # w interfejsie Xamarin.Forms**

## <a name="overview"></a>Omówienie

CocosSharp to technologia elastycznym i zaawansowanym wyświetlanie grafiki, odczytywanie wprowadzania dotykowego, odtwarzanie zawartości audio i zarządzanie nimi. W tym przewodniku objaśniono sposób dodawania CocosSharp do aplikacji platformy Xamarin.Forms. Obejmuje następujące czynności:

* [Co to jest CocosSharp?](#what)
* [Trwa dodawanie pakietów Nuget w narzędziu CocosSharp](#nuget)
* [Wskazówki: Dodawanie CocosSharp do aplikacji platformy Xamarin.Forms](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>Co to jest CocosSharp?

[CocosSharp](~/graphics-games/cocossharp/index.md) to aparat gier "open source", który jest dostępny na platformie Xamarin.
CocosSharp jest efektywne środowiska uruchomieniowego biblioteki, która obejmuje następujące funkcje:

* Renderowanie obrazów przy użyciu [CCSprite, klasa](https://developer.xamarin.com/api/type/CocosSharp.CCSprite/)
* Kształt renderowania za pomocą [klasy pomocą narzędzia CCDrawNode](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
* Przy użyciu logiki co ramki [CCNode.Schedule — metoda](https://developer.xamarin.com/api/member/CocosSharp.CCNode.Schedule/p/System.Action%7BSystem.Single%7D/)
* Za pomocą zarządzania zawartością (Trwa ładowanie i zwalnianie zasobów, takich jak pliki PNG) [CCTextureCache, klasa](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
* Animacji przy użyciu [klasy pomocą narzędzia CCAction](https://developer.xamarin.com/api/type/CocosSharp.CCAction/)

W narzędziu CocosSharp głównej koncentruje się na uproszczenie tworzenia międzyplatformowych gier 2D; jednak może być również doskonałe dodawania do aplikacji Xamarin formularza. Ponieważ gry zwykle wymagają wydajne renderowanie i precyzyjną kontrolę nad tym wizualizacje, CocosSharp może służyć do dodawania zaawansowane wizualizacje i wpływ na aplikacje inne niż gry.

Zestaw narzędzi Xamarin.Forms oparto się na macierzystym, specyficzne dla platformy systemów interfejsu użytkownika. Na przykład [ `Button`s](xref:Xamarin.Forms.Button) wyglądać inaczej w systemach iOS i Android, a nawet może różnić się od wersji systemu operacyjnego. Z drugiej strony CocosSharp nie używa żadnych obiektów wizualnych specyficzne dla platformy, dzięki czemu wszystkie obiekty wizualne są wyświetlane takie same na wszystkich platformach. Oczywiście rozdzielczości i współczynnik proporcji różni się między urządzeniami i może wpłynąć na sposób CocosSharp powoduje wyświetlenie jego wizualizacje. Te informacje zostaną dokładniej omówione w dalszej części tego przewodnika.

Bardziej szczegółowe informacje można znaleźć w [sekcji CocosSharp](~/graphics-games/cocossharp/index.md).

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>Trwa dodawanie pakietów Nuget w narzędziu CocosSharp

Przed rozpoczęciem korzystania z narzędzia CocosSharp deweloperzy muszą wprowadzić kilka dodatki do swojego projektu Xamarin.Forms.
W tym przewodniku założono projektu Xamarin.Forms przy użyciu systemu iOS, Android i .NET Standard projektu biblioteki.
Obejmie cały kod w projekcie biblioteki .NET Standard; jednak należy dodać biblioteki do projektów dla systemu Android i iOS.

Pakiet CocosSharp Nuget zawiera wszystkie obiekty niezbędne do tworzenia obiektów w narzędziu CocosSharp.
Obejmuje pakiet nuget CocosSharp.Forms `CocosSharpView` klasę, która zostanie użyta do hosta CocosSharp w interfejsie Xamarin.Forms.
Dodaj **CocosSharp.Forms** NuGet i **CocosSharp** zostaną automatycznie dodane również.
Aby to zrobić, kliknij prawym przyciskiem myszy <span class="UIItem">pakietów</span> folderu w projekcie biblioteki .NET Standard i wybierz pozycję <span class="UIItem">Dodawanie pakietów... </span>. Wprowadź wyszukiwany termin <span class="UIItem">CocosSharp.Forms</span>, wybierz opcję <span class="UIItem">CocosSharp dla platformy Xamarin.Forms</span>, następnie kliknij przycisk <span class="UIItem">Dodaj pakiet</span>.

![](cocossharp-images/image1.png "Dodaj okno dialogowe pakiety")

Zarówno **CocosSharp** i **CocosSharp.Forms** pakiety NuGet zostaną dodane do projektu:

![](cocossharp-images/image2.png "Packages Folder")

Powtórz powyższe kroki dla projektów specyficznych dla platformy (na przykład iOS i Android).

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>Wskazówki: Dodawanie CocosSharp do aplikacji platformy Xamarin.Forms

Wykonaj następujące kroki, aby dodać widok prosty CocosSharp do aplikacji platformy Xamarin.Forms:

1. [Tworzenie Xamarin Forms strony](#1)
1. [Dodawanie CocosSharpView](#2)
1. [Tworzenie GameScene](#3)
1. [Dodawanie okręgu](#4)
1. [Wchodzenie w interakcje za pomocą narzędzia CocosSharp](#5)

Po widoku CocosSharp zostało pomyślnie dodane do aplikacji platformy Xamarin.Forms, odwiedź witrynę [dokumentacji CocosSharp](~/graphics-games/cocossharp/index.md) Aby dowiedzieć się więcej na temat tworzenia zawartości za pomocą narzędzia CocosSharp.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Tworzenie Xamarin Forms strony

CocosSharp mogą być hostowane w dowolnym kontenerze zestawu narzędzi Xamarin.Forms. W tym przykładzie na tej stronie użyto stronę o nazwie `HomePage`. `HomePage` jest podzielony na dwie części przez `Grid` pokazanie, jak platformy Xamarin.Forms i CocosSharp możliwą do wyrenderowania równocześnie w tej samej stronie.

Najpierw skonfiguruj stronę, aby zawierała `Grid` oraz dwóch `Button` wystąpień:


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

`CocosSharpView` Klasa jest używana do osadzania CocosSharp w aplikacji platformy Xamarin.Forms. Ponieważ `CocosSharpView` dziedziczy [Xamarin.Forms.View](xref:Xamarin.Forms.View) klasy, zapewnia znany interfejs dla układu, przy czym może służyć w kontenery układów takich jak [Xamarin.Forms.Grid](xref:Xamarin.Forms.Grid). Dodaj nową `CocosSharpView` do projektu, wykonując `CreateTopHalf` metody:


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

Inicjowanie CocosSharp nie jest procesem natychmiastowym, więc zarejestrować zdarzenia, kiedy `CocosSharpView` zakończeniu jego tworzenia. To zrobić w `HandleViewCreated` metody:


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

`HandleViewCreated` Metoda ma dwie ważne informacje, które firma Microsoft będzie spojrzenie na. Pierwsza to `GameScene` klasy, która zostanie utworzona w następnej sekcji. Ważne jest, aby należy pamiętać, że aplikacja nie zostanie skompilowany do momentu `GameScene` jest tworzony i `gameScene` odwołania do wystąpienia nie zostanie rozwiązany.

Drugi ważne szczegóły są `DesignResolution` właściwość, która definiuje obszar CocosSharp obiekty widoczne gry. `DesignResolution` Właściwość będzie kształtować po utworzeniu `GameScene`.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. Tworzenie GameScene

`GameScene` Klasa dziedziczy w narzędziu CocosSharp `CCScene`. `GameScene` to pierwszy punkt, gdzie Radzimy wyłącznie za pomocą narzędzia CocosSharp. Kod zawarty w `GameScene` będzie działać, we wszystkich aplikacjach CocosSharp, czy są przechowywane w ramach projektu Xamarin.Forms, czy nie.

`CCScene` Klasa jest głównym visual renderowania CocosSharp wszystkich. Dowolnego widocznego obiektu CocosSharp musi być zawarta w `CCScene`. W szczególności obiektów wizualnych muszą zostać dodane do `CCLayer` wystąpień, a te `CCLayer` wystąpienia muszą zostać dodane do `CCScene`.

Poniższy wykres mogą pomóc w wizualizacji typowe hierarchii w narzędziu CocosSharp:

![](cocossharp-images/image4.png "Typowe hierarchii w narzędziu CocosSharp")

Tylko jeden `CCScene` może być aktywny w tym samym czasie. Większość gier używanych jest wiele `CCLayer` wystąpień Sortowanie zawartości, ale nasza aplikacja używa tylko jeden. Podobnie większości gier wykorzystuje wiele obiektów wizualnych, ale firma Microsoft będzie zawierać tylko jeden w naszej aplikacji. Bardziej szczegółowe omówienie CocosSharp visual hierarchii znajdują się w [wskazówki gry BouncingGame](~/graphics-games/cocossharp/bouncing-game.md).

Początkowo `GameScene` klasa będzie prawie pusty — po prostu utworzymy je spełnić odwołania w `HomePage`. Dodaj nową klasę do projektu biblioteki .NET Standard, o nazwie `GameScene`. Powinien on dziedziczyć `CCScene` klasy w następujący sposób:


```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

Teraz, gdy `GameScene` jest zdefiniowany, firma Microsoft może zwrócić do `HomePage` i dodać pole:


```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

Firma Microsoft jest teraz skompilować naszych projektów i uruchom go, aby wyświetlić uruchomione w narzędziu CocosSharp. Firma Microsoft nie dodano żadnych elementów do naszych `GameScene,` więc czarnego — domyślny kolor sceny CocosSharp w górnej połowie strony:

![](cocossharp-images/image5.png "Blank GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. Dodawanie okręgu

Aplikacja ma aktualnie uruchomione wystąpienie aparatu CocosSharp, wyświetlanie pustego `CCScene`. Następnie dodamy obiekt wizualny: okrąg. `CCDrawNode` Klasa może być używana do rysowania różnych kształtów geometrycznych, zgodnie z opisem w [geometrii rysowania za pomocą narzędzia CCDrawNode przewodnik](~/graphics-games/cocossharp/ccdrawnode.md).

Dodaj koło, aby nasz `GameScene` klasy i wystąpienia w konstruktorze, jak pokazano w poniższym kodzie:


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

Twoja aplikacja działa teraz pokazuje okrąg po lewej stronie obszaru wyświetlania CocosSharp:

![](cocossharp-images/image6.png "Okrąg w GameScene")


#### <a name="understanding-designresolution"></a>Opis DesignResolution

Teraz wyświetlany obiekt CocosSharp visual zbadanie `DesignResolution` właściwości.

`DesignResolution` Reprezentuje szerokość i wysokość obszaru CocosSharp umieszczania i ustalania rozmiaru obiektów. Rzeczywista rozdzielczość obszaru jest mierzony w *pikseli* podczas `DesignResolution` jest mierzony w świecie *jednostek*. Na poniższym diagramie przedstawiono rozwiązanie różne części widoku wyświetlane na telefonie iPhone 5 przy użyciu rozdzielczości ekranu w pikselach 640 x 1136:

![](cocossharp-images/image7.png "dla telefonu iPhone 5 s rozwiązania projektu")

Na powyższym diagramie Wyświetla rozmiar w pikselach na zewnątrz ekranu w czarny tekst. Jednostki są wyświetlane wewnątrz diagramu w białego tekstu. Oto niektóre ważne informacje wyświetlone powyżej:

* Wyświetlanie CocosSharp pochodzi w lewym dolnym rogu. Przesunięcie w prawo powoduje zwiększenie wartości X i przeniesienie zwiększa wartość Y. Należy zauważyć, że wartości Y jest odwrócony w porównaniu do niektórych innych silniki układu 2D, gdzie (0,0) jest od lewej górnej części kanwy.
* Domyślne zachowanie CocosSharp jest zachować proporcje jej widok. Ponieważ pierwszy wiersz w siatce jest większa niż wysokość, CocosSharp nie wypełnił całe jego komórki, jak to przedstawiono w zapisie kropkowo-biały prostokąt. To zachowanie można zmienić zgodnie z opisem w [Przewodnik obsługi wielu rozdzielczości w narzędziu CocosSharp](~/graphics-games/cocossharp/resolutions.md).
* W tym przykładzie CocosSharp spowoduje zachowanie obszaru wyświetlania, 100 jednostek szerokość lub wysokość niezależnie od rozmiaru lub współczynnika proporcji jego urządzenia. Oznacza to, że kodu, można założyć, że wyświetlić reprezentuje X = 100 skrajna prawa powiązane CocosSharp, układ zachowanie zgodne na wszystkich urządzeniach.


#### <a name="ccdrawnode-details"></a>Szczegóły pomocą narzędzia CCDrawNode

Nasz prosty aplikacja używa `CCDrawNode` klasy, aby narysować okrąg. Ta klasa może być bardzo przydatne w przypadku aplikacji biznesowych, ponieważ zapewnia renderowania opartego na wektorach geometrii — funkcja brakuje zestawu narzędzi Xamarin.Forms. Oprócz okręgów `CCDrawNode` klasy może służyć do Rysowanie prostokątów, krzywe, linie i niestandardowe wielokątów. `CCDrawNode` jest również łatwe w użyciu, ponieważ nie wymaga korzystania z plików obrazu (na przykład w formacie PNG). Szczegółowe omówienie pomocą narzędzia CCDrawNode znajdują się w [geometrii rysowania za pomocą narzędzia CCDrawNode przewodnik](~/graphics-games/cocossharp/ccdrawnode.md).

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. Wchodzenie w interakcje za pomocą narzędzia CocosSharp

Elementy wizualne umieść w narzędziu CocosSharp (takie jak `CCDrawNode`) dziedziczyć `CCNode` klasy. `CCNode` udostępnia dwie właściwości, które mogą być używane do położenia obiektu względem jego elementu nadrzędnego: `PositionX` i `PositionY`. Nasz kod aktualnie używa tych dwóch właściwości do pozycji środek okręgu, jak pokazano w poniższym przykładzie:


```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

Należy zauważyć, że CocosSharp obiektów są umieszczone przez wartości jawnie określonych pozycji, w przeciwieństwie do większości widoków platformy Xamarin.Forms, które są automatycznie ustawione w zachowania ich nadrzędnego układu kontrolek.

Dodamy kod, aby umożliwić użytkownikowi kliknij jedną z dwóch przyciski Przenieś w lewo lub w prawo koło przez 10 jednostek (nie w pikselach, ponieważ koła rysuje w przestrzeni jednostki CocosSharp świata). Najpierw utworzymy dwóch metod publicznych w `GameScene` klasy:


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

Następnie dodamy programy obsługi dla dwóch przycisków w `HomePage` na odpowiadanie na kliknięcia. Po zakończeniu naszych `CreateBottomHalf` metoda zawiera następujący kod:


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

Okrąg CocosSharp porusza się teraz w odpowiedzi na kliknięcia. Widać również wyraźne granice kanwy CocosSharp, przenosząc koła daleko po lewej stronie lub w prawo:

![](cocossharp-images/image8.png "GameScene przy przenoszeniu okręgu")

## <a name="summary"></a>Podsumowanie

W przewodniku przedstawiono sposób dodawania CocosSharp do istniejącego zestawu narzędzi Xamarin.Forms projektu, jak utworzyć interakcji między platformy Xamarin.Forms i CocosSharp i w tym artykule omówiono różne zagadnienia podczas tworzenia układów w narzędziu CocosSharp.

Aparat do tworzenia gier CocosSharp oferuje wiele funkcji i głębi, ten przewodnik tylko uproszczone co robić CocosSharp. Deweloperów zainteresowanych dodatkowe materiały związane z CocosSharp można znaleźć wiele artykułów w [sekcji CocosSharp](~/graphics-games/cocossharp/index.md).



## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API w narzędziu CocosSharp](https://developer.xamarin.com/api/root/CocosSharp/)
- [CocosSharpForms (przykład)](https://developer.xamarin.com/samples/xamarin-forms/CocosSharpForms/)
