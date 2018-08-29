---
title: Za pomocą narzędzia potoku MonoGame
description: Narzędzia potoku MonoGame służy do tworzenia i zarządzania projektami zawartości MonoGame. Pliki w projektach zawartości są przetworzony przez narzędzie potoku MonoGame i zwrócone jako .xnb plików do użytku w aplikacjach CocosSharp i MonoGame.
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 347cb7e9d417f97cb6e8d78e67b1c76a378cd188
ms.sourcegitcommit: 7ffbecf4a44c204a3fce2a7fb6a3f815ac6ffa21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "34783302"
---
# <a name="using-the-monogame-pipeline-tool"></a>Za pomocą narzędzia potoku MonoGame

_Narzędzia potoku MonoGame służy do tworzenia i zarządzania projektami zawartości MonoGame. Pliki w projektach zawartości są przetworzony przez narzędzie potoku MonoGame i zwrócone jako .xnb plików do użytku w aplikacjach CocosSharp i MonoGame._

Narzędzie potoku MonoGame zapewnia łatwy w użyciu środowisko do konwersji plików zawartości do **.xnb** plików do użytku w aplikacjach CocosSharp i MonoGame. Aby uzyskać informacje na temat potoków zawartości i dlaczego są przydatne do tworzenia gier, zobacz [to wprowadzenie potoków zawartości](~/graphics-games/cocossharp/content-pipeline/introduction.md)

W tym przewodniku opisano następujące czynności:

 - Instalowanie narzędzia potoku MonoGame
 - Tworzenie projektu CocosSharp
 - Tworzenie zawartości projektu
 - Przetwarzanie plików za pomocą narzędzia potoku MonoGame
 - Korzystanie z plików w czasie wykonywania

W tym instruktażu wykorzystano projektu CocosSharp w celu zademonstrowania sposobu **.xnb** plików może być załadowany i używane w aplikacji. Użytkownicy MonoGame również będzie można odwoływać się w tym przewodniku CocosSharp i MonoGame używać tego samego **.xnb** zawartości plików.

Gotowa aplikacja wyświetli pojedynczego sprite wyświetlanie tekstury z **.xnb** plików i wyświetlanie sprite czcionkę z pojedynczą etykietę **.xnb** pliku:

![](walkthrough-images/image1.png "Gotowa aplikacja wyświetli sprite jednego tekstury z pliku .xnb wyświetlania")


## <a name="monogame-pipeline-tool-discussion"></a>Omówienie narzędzia potoku MonoGame

Narzędzie potoku MonoGame jest dostępne w Windows, OS X i Linux. W tym przewodniku zostanie uruchomione narzędzie Windows, ale można było śledzić na komputerach Mac i Linux oraz. Aby uzyskać informacje na temat pobierania narzędzia w systemie Linux lub maksymalnej, zobacz [tę stronę](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/).

Narzędzia MonoGame w potoku jest możliwość tworzenia zawartości dla aplikacji systemu iOS nawet kiedy uruchamiać w Windows, więc deweloperzy korzystający z [Xamarin Mac Agent](~/ios/get-started/installation/windows/connecting-to-mac/index.md) będzie można kontynuować tworzenie na Windows.


## <a name="installing-the-monogame-pipeline-tool"></a>Instalowanie narzędzia potoku MonoGame

Firma Microsoft rozpocznie się po zainstalowaniu rozwiązania MonoGame, która zawiera potok zawartości MonoGame. Należy pamiętać, że potok zawartości MonoGame pobrania osobno dla komputerów Mac. Wszystkie instalatory MonoGame znajduje się na [strony plików do pobrania MonoGame](http://www.monogame.net/downloads/). Firma Microsoft będzie pobranie MonoGame dla programu Visual Studio, ale po zainstalowaniu Deweloper służy MonoGame w programie Visual Studio dla komputerów Mac za:

![](walkthrough-images/image2.png "Pobierz MonoGame dla programu Visual Studio, ale po zainstalowaniu MonoGame użycie dla deweloperów w programie Visual Studio dla komputerów Mac za")

Po pobraniu, utworzymy przez Instalatora i zaakceptuj opcje domyślne. Po zakończeniu pracy Instalatora Narzędzia potoku MonoGame jest zainstalowany i znajduje się w polu wyszukiwania menu Start:

![](walkthrough-images/image3.png "Po zakończeniu pracy Instalatora Narzędzia potoku MonoGame jest zainstalowany i znajduje się w polu wyszukiwania menu Start")

Uruchom narzędzie MonoGame potoku:

![](walkthrough-images/image4.png "Uruchom narzędzie do potoku MonoGame")

Po uruchomieniu narzędzia potoku MonoGame Rozpoczniemy pracę zapewnienie naszych projektów gry i zawartość.


## <a name="creating-an-empty-cocossharp-project"></a>Tworzenie pustego projektu w narzędziu CocosSharp

Następnym krokiem jest utworzenie projektu CocosSharp. Należy pamiętać, że możemy utworzyć projektu CocosSharp najpierw tak, aby nasz zawartości projektu można zapisać w strukturze folderów, utworzony przez projektu CocosSharp. Aby poznać strukturę projektu CocosSharp, Przyjrzyj się [gry BouncingGame](~/graphics-games/cocossharp/bouncing-game.md), która będzie używana w tym przewodniku. Jednak w przypadku istniejącego projektu CocosSharp, który chcesz dodać zawartość do możesz użyć tego projektu zamiast gry BouncingGame.

Po utworzeniu projektu uruchomimy go w celu zweryfikowania, że zbudował i wszystko, co mamy prawidłowo skonfigurowane:

![](walkthrough-images/image5.png "Po utworzeniu projektu, uruchom go w celu zweryfikowania, że kompilacje i że wszystko prawidłowo skonfigurowane")


## <a name="creating-a-content-project"></a>Tworzenie zawartości projektu

Teraz, gdy projekt gry, możemy utworzyć projektu MonoGame potoku. Aby to zrobić, wybierz narzędzia potoku MonoGame w **Plik > Nowy...**  i przejdź do folderu zawartości projektu. Dla systemów Android, jej folder znajduje się w **[projektu root]\BouncingGame.Android\Assets\Content\**. W przypadku systemu iOS jej folder znajduje się w **[projektu root]\BouncingGame.iOS\Content\**.

Zmiana **nazwy pliku** do **ContentProject** i kliknij przycisk **Zapisz** przycisku:

![](walkthrough-images/image8.png "Zmień nazwę pliku ContentProject, a następnie kliknij przycisk Zapisz")

Po utworzeniu projektu MonoGame narzędzia potoku spowoduje to wyświetlenie informacji o projekcie podczas głównego **ContentProject** jest zaznaczony element:

![](walkthrough-images/image9.png "Po utworzeniu projektu MonoGame narzędzia potoku spowoduje to wyświetlenie informacji o projekcie po wybraniu elementu ContentProject głównego")

Spójrzmy na niektóre z najważniejszych opcjach dla zawartości projektu.


### <a name="output-folder"></a>Folder wyjściowy

Gdzie jest to folder (względem zawartości projektu, sam) danych wyjściowych **.xnb** będą zapisywane pliki. Aby zachować ich prostotę, użyjemy tego samego folderu do przechowywania nasze dane wejściowe i wyjściowe pliki. Innymi słowy zmienimy **Folder wyjściowy** jako **.\**  :

![](walkthrough-images/image10.png "")


### <a name="platform"></a>Platforma

Definiuje platformę docelową dla zawartości. Należy zauważyć, że jest to **Windows** domyślnie, więc chcemy ją zmienić na naszej platformy docelowej, która jest **Android** (lub z systemem iOS, jeśli wraz z projektu systemu iOS).

![](walkthrough-images/image11.png "Należy zauważyć, że to Windows, domyślnie, dlatego ją zmienić na platformę docelową, która jest systemem Android lub iOS, jeśli wraz z projektu systemu iOS")


## <a name="processing-files-in-the-monogame-pipeline-tool"></a>Przetwarzanie plików za pomocą narzędzia potoku MonoGame

Następnie będziemy dodawać zawartość do naszych **ContentProject**. Dla tego projektu będziemy dodawać pliki do katalogu głównego projektu, ale większe projekty zazwyczaj zorganizuje ich zawartość w folderach.

Dwa pliki zostanie dodany do naszych projektu:

 - A **.png** pliku, który będzie używany do rysowania sprite. Ten plik można [pobrana w tym miejscu](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true).
 - A **.spritefont** pliku, który będzie używany do rysowania tekstu na ekranie. Narzędzia potoku zawartości obsługuje tworzenie nowych plików .spritefont, więc nie ma pliku do pobrania.


### <a name="adding-a-png-file"></a>Dodawanie pliku PNG

Aby dodać **.png** plik do projektu, firma Microsoft będzie najpierw skopiuj go do tego samego katalogu projektu potoku, który ma **.mgcb** rozszerzenia.

![](walkthrough-images/image12.png "Dodaj plik PNG do projektu")

Następnie dodamy go do projektu potoku. Aby to zrobić w narzędziu MonoGame potoku, wybierz **Edytuj > Dodaj element...** , wybierz opcję **ball.png** plik i kliknij przycisk **Otwórz**. Plik będzie teraz część zawartości projektu i po wybraniu spowoduje wyświetlenie jej właściwości:

![](walkthrough-images/image13.png "Plik będzie teraz część zawartości projektu i po wybraniu spowoduje wyświetlenie jej właściwości")

Wyślemy wiadomość, jest pozostawienie wszystkie wartości domyślne wartości, ponieważ żadne zmiany nie są wymagane do załadowania pliku .xnb w narzędziu CocosSharp. Możemy utworzyć plik, wybierając **kompilacji > Tworzenie** opcję menu, która uruchomi kompilację i wyświetlić dane wyjściowe dotyczące kompilacji. Można sprawdzić, czy kompilacja działa poprawnie, sprawdzając **zawartości** nowy folder **ball.xnb** pliku:

![](walkthrough-images/image14.png "Sprawdź, czy kompilacja działa poprawnie, sprawdzając zawartość folderu dla nowego pliku ball.xnb")


### <a name="adding-a-spritefont-file"></a>Dodawanie pliku .spritefont

Można utworzyć pliku .spritefont za pomocą narzędzia potoku MonoGame. CocosSharp wymaga czcionek w **czcionki** folder i szablony CocosSharp automatycznie automatycznie Utwórz folder czcionek. Ten folder możemy dodać do rozwiązania MonoGame narzędzia potoku, wybierając **Edytuj > Dodaj > istniejący Folder...** . Przejdź do **zawartości** i wybierz polecenie **czcionki** folder i kliknij przycisk **OK**:

![](walkthrough-images/browsetofonts.png "Przejdź do folderu zawartości i wybierz folder czcionek i kliknij przycisk OK")

Aby dodać nowy plik .sprintefont, kliknij prawym przyciskiem myszy na folder czcionek, a następnie wybierz **Dodaj > Nowy element...** , wybierz opcję **opis SpriteFont** opcji, wprowadź nazwę **arial 36**i kliknij przycisk **Ok**. CocosSharp wymaga bardzo szczegółowych nazewnictwa plików czcionki — muszą być w formacie [FontType]-[FontSize]. Jeśli czcionki nie jest zgodna z ten format nazewnictwa go nie zostanie załadowany w narzędziu CocosSharp w czasie wykonywania.

![](walkthrough-images/image15.png "Jeśli czcionki nie jest zgodna z ten format nazewnictwa go nie zostanie załadowany w narzędziu CocosSharp w czasie wykonywania")

Plik .spritefont jest faktycznie pliku XML, który można edytować w edytorze tekstu, w tym Visual Studio dla komputerów Mac. Najbardziej typowe zmienne edytowane w pliku .spritefont `FontName` i `Size` właściwości:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

Firma Microsoft będzie Otwórz plik w dowolnym edytorze tekstów. Jako naszych **arial 36.spritefont** sugeruje nazwa, pozostawimy `FontName` jako `Arial` , ale Zmień `Size` wartość `36`:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
## <a name="using-files-at-runtime"></a>Korzystanie z plików w czasie wykonywania

Pliki .xnb teraz są tworzone i gotowe do użycia w projekcie. Będziemy dodawać pliki do programu Visual Studio dla komputerów Mac, a następnie dodasz kod, aby nasz `GameScene.cs` plik do załadowania tych plików i ich wyświetlenie.


### <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Dodawanie plików .xnb do programu Visual Studio dla komputerów Mac

Najpierw dodamy pliki do naszego projektu. W programie Visual Studio dla komputerów Mac, będziesz rozwijania **BouncingGame.Android** projektu, rozwiń węzeł **zasoby** folderu, kliknij prawym przyciskiem myszy **zawartości** folder, a następnie wybierz **Dodaj > Dodaj pliki...** Najpierw należy wybrać **ball.xnb** firma Microsoft utworzone wcześniej i kliknij przycisk **Otwórz**. Następnie powtórz powyższe kroki, ale Dodaj **arial 36.xnb** pliku. Wybierzemy **przechowywać plik w jego bieżącym podkatalogu** opcję, jeśli program Visual Studio for Mac pyta, jak dodać plik. Po zakończeniu obu plików powinna być częścią naszego projektu:

![](walkthrough-images/image20.png "Po zakończeniu obu plików powinna być częścią projektu")


### <a name="adding-gamescenecs"></a>Dodawanie **GameScene.cs**

Utworzymy klasę o nazwie `GameScene,` zawierający obiekty naszych sprite i tekst. Aby to zrobić, kliknij prawym przyciskiem myszy **gry BouncingGame** (nie BouncingGame.Android) projektu, a następnie wybierz **Dodaj > Nowy plik...** . Wybierz **ogólne** kategorii, wybierz opcję **pustą klasę** opcji, a następnie wprowadź nazwę **GameScene**.

Po utworzeniu zmodyfikujemy `GameScene.cs` plik zawiera następujący kod:


```csharp
using System;
using CocosSharp;

namespace BouncingGame
{
    public class GameScene : CCScene
    {
        // All visual elements must be added to a CCLayer:
        CCLayer mainLayer;

        // The CCSprite is used to display the "ball" texture
        CCSprite sprite;
        // The CCLabelTtf is used to display the Arial36 sprite font
        CCLabelTtf label;

        public GameScene(CCWindow mainWindow) : base(mainWindow)
        {
            // Instantiate the CCLayer first:
            mainLayer = new CCLayer ();
            AddChild (mainLayer);

            // Now we can create the Sprite using the ball.xnb file:
            sprite = new CCSprite ("ball");
            sprite.PositionX = 200;
            sprite.PositionY = 200;
            mainLayer.AddChild (sprite);

            // The font name (arial) and size (36) need to match 
            // the .spritefont definition and file name.  
            label = new CCLabelTtf ("Using font 36", "arial", 36);
            label.PositionX = 200;
            label.PositionY = 300;
            mainLayer.AddChild (label);
        }
    }
} 
```

Firma Microsoft nie będzie w niniejszym dokumencie powyższy kod od pracy przy użyciu obiektów wizualnych CocosSharp, takich jak CCSprite i CCLabelTtf zostało omówione w [przewodnik gry BouncingGame](~/graphics-games/cocossharp/bouncing-game.md).

Należy również dodać kod, aby załadować naszym nowo utworzonym `GameScene`. W tym otworzymy `GameAppDelegate.cs` pliku (znajdujący się w **gry BouncingGame** PCL) i modyfikować `ApplicationDidFinishLaunching` metoda tak, aby wyglądał jak:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";

    // New code:
    GameScene gameScene = new GameScene (mainWindow);
    mainWindow.RunWithScene (gameScene);
} 
```

Podczas uruchamiania, nasze gra będzie wyglądać następująco:

![](walkthrough-images/image1.png "Podczas uruchamiania, gra będzie wyglądać")


## <a name="summary"></a>Podsumowanie

W tym przewodniku pokazano, jak za pomocą narzędzia potoku MonoGame tworzenie .xnb pliki z pliku wejściowego PNG, a także sposobu tworzenia nowego pliku .xnb z .sprintefont nowo utworzony plik. Omówiono również sposób CocosSharp projektów, aby używać plików .xnb struktury i ładowania tych plików w czasie wykonywania.

## <a name="related-links"></a>Linki pokrewne

- [Pobieranie rozwiązania MonoGame](http://www.monogame.net/downloads/)
- [Dokumentacja rozwiązania MonoGame potoku](http://www.monogame.net/documentation/?page=Pipeline)
- [Uruchamianie projektu gry BouncingGame dla systemu Android (przykład)](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [Ball.PNG grafiki (przykład)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
