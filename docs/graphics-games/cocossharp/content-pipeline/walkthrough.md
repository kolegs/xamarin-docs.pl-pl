---
title: "Za pomocą narzędzia MonoGame potoku"
description: "Narzędzie MonoGame potoku jest używane do tworzenia projektów i zarządzanie nimi MonoGame zawartości. Pliki w projektach zawartości są przetworzone przez narzędzie Monogame potoku i wyjściowych jako pliki .xnb do użycia w aplikacjach CocosSharp i MonoGame."
ms.topic: article
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: df3692777eaa0791385c9ef3d114fbc8a9ab752e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="using-the-monogame-pipeline-tool"></a>Za pomocą narzędzia MonoGame potoku

_Narzędzie MonoGame potoku jest używane do tworzenia projektów i zarządzanie nimi MonoGame zawartości. Pliki w projektach zawartości są przetworzone przez narzędzie Monogame potoku i wyjściowych jako pliki .xnb do użycia w aplikacjach CocosSharp i MonoGame._

Narzędzie potoku MonoGame zapewnia łatwy w użyciu środowisko Konwertowanie plików zawartości do **.xnb** plików do użycia w aplikacjach CocosSharp i MonoGame. Informacje dotyczące zawartości potoki i dlaczego jest przydatny do tworzenia gier, zobacz [to wprowadzenie na potoki zawartości](~/graphics-games/cocossharp/content-pipeline/introduction.md)

Ten przewodnik obejmuje następujące czynności:

 - Instalowanie narzędzia MonoGame potoku
 - Tworzenie projektu CocosSharp
 - Tworzenie projektu zawartości
 - Przetwarzanie plików w narzędziu MonoGame potoku
 - Przy użyciu plików w czasie wykonywania

W tym przewodniku zastosowano projektu CocosSharp aby zademonstrować sposób **.xnb** plików może być załadowany i używane w aplikacji. Użytkownicy MonoGame będzie także można odwoływać się w tym przewodniku CocosSharp i MonoGame używać tego samego **.xnb** zawartości plików.

Zakończono aplikacji spowoduje wyświetlenie pojedynczego sprite wyświetlanie tekstury z **.xnb** plików i pojedynczej etykiecie, wyświetlanie czcionkę sprite z **.xnb** pliku:

![](walkthrough-images/image1.png "Zakończono aplikacji spowoduje wyświetlenie pojedynczego sprite wyświetlanie tekstury z pliku .xnb")


# <a name="monogame-pipeline-platform-discussion"></a>Omówienie platformy MonoGame potoku

Narzędzie MonoGame potoku jest dostępne w systemie Windows, OS X i Linux. W tym przewodniku zostanie uruchomione narzędzie w systemie Windows, ale jego może występować Mac i Linux oraz. Aby uzyskać informacje na temat pobierania narzędzia na systemie Linux lub maksymalnej, zobacz [tej strony](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/).

Narzędzie MonoGame potoku jest w stanie tworzenie zawartości dla aplikacji systemu iOS nawet podczas uruchomienia w systemie Windows, deweloperzy tak przy użyciu [Xamarin Mac Agent](~/ios/get-started/installation/windows/connecting-to-mac/index.md) będzie można kontynuować tworzenie w systemie Windows.


# <a name="installing-the-monogame-pipeline-tool"></a>Instalowanie narzędzia MonoGame potoku

Firma Microsoft będzie Rozpocznij od zainstalowania MonoGame, w tym potoku zawartości MonoGame. Należy pamiętać, że potok zawartości MonoGame osobny plik do pobrania dla komputerów Mac. Wszystkich instalatorów MonoGame można znaleźć w [MonoGame pobiera strony](http://www.monogame.net/downloads/). Firma Microsoft będzie pobranie MonoGame dla programu Visual Studio, ale po zainstalowaniu dewelopera służy MonoGame w programie Visual Studio dla komputerów Mac za:

![](walkthrough-images/image2.png "Pobierz MonoGame dla programu Visual Studio, ale po zainstalowaniu użyj developer MonoGame w programie Visual Studio for Mac zbyt")

Po pobraniu, firma Microsoft będzie Uruchom za pomocą Instalatora i zaakceptuj ustawienia domyślne opcje. Po zakończeniu działania Instalatora, narzędzie MonoGame potoku jest zainstalowana i znajduje się w polu wyszukiwania menu Start:

![](walkthrough-images/image3.png "Po zakończeniu działania Instalatora, narzędzie MonoGame potoku jest zainstalowana i znajduje się w polu wyszukiwania menu Start")

Uruchom narzędzie MonoGame potoku:

![](walkthrough-images/image4.png "Uruchom narzędzie MonoGame potoku")

Po uruchomieniu narzędzia MonoGame potoku, możemy rozpocząć zagwarantowania naszych projektów gier i zawartości.


# <a name="creating-an-empty-cocossharp-project"></a>Pusty projekt CocosSharp tworzenia

Następnym krokiem jest utworzenie projektu CocosSharp. Należy pamiętać, że firma Microsoft CocosSharp najpierw Utwórz projekt tak, aby naszej zawartości projektu można zapisać w strukturze folderu utworzone przez projekt CocosSharp. Aby uzyskać informacje na temat sposobu tworzenia nowego projektu, zobacz [przewodnik BouncingGame](~/graphics-games/cocossharp/first-game/part1.md). Dla tego przewodnika firma Microsoft będzie tworzenia projektu o nazwie BouncingGame, ale wszelkie istniejący projekt CocosSharp będzie działać prawidłowo. Jeśli masz istniejący projekt CocosSharp, który chcesz dodać zawartość, możesz użyć tego projektu zamiast BouncingGame projektu.

Po utworzeniu projektu zostanie uruchomiony na Sprawdź, czy zbudował i wszystko, co mamy prawidłowo skonfigurowane:

![](walkthrough-images/image5.png "Po utworzeniu projektu, uruchom ją, aby zweryfikować, że kompilacje i że wszystkie poprawnie skonfigurowany")


# <a name="creating-a-content-project"></a>Tworzenie projektu zawartości

Teraz, gdy mamy gier projektu można utworzyć projektu MonoGame potoku. Aby to zrobić, wybierz narzędzie potoku MonoGame w **Plik > Nowy...**  i przejdź do folderu zawartości projektu. Dla systemu Android, jej folder znajduje się na **[projektu root]\BouncingGame.Android\Assets\Content\**. Dla systemu iOS, jej folder znajduje się na **[projektu root]\BouncingGame.iOS\Content\**.

Zmień **nazwę pliku** do **ContentProject** i kliknij przycisk **zapisać** przycisk:

![](walkthrough-images/image8.png "Zmień nazwę pliku ContentProject i kliknij przycisk Zapisz")

Po utworzeniu projektu narzędzia potoku MonoGame spowoduje wyświetlenie informacji o projekcie podczas głównego **ContentProject** zaznaczono element:

![](walkthrough-images/image9.png "Po utworzeniu projektu narzędzia potoku MonoGame spowoduje wyświetlenie informacji o projekcie po wybraniu elementu ContentProject głównego")

Oto niektóre z najważniejszych opcji dla zawartości projektu.


## <a name="output-folder"></a>Folder wyjściowy

Gdzie jest to folder (względem zawartości projektu, sam) dane wyjściowe **.xnb** pliki zostaną zapisane. Aby zachować proste rzeczy, użyjemy tego samego folderu do przechowywania naszych danych wejściowych i wyjściowych plików. Innymi słowy zmienimy **folderu wyjściowego** być **.\**  :

![](walkthrough-images/image10.png "")


## <a name="platform"></a>Platforma

Określa platformę docelową dla zawartości. Należy zauważyć, że jest to **Windows** domyślnie dlatego chcemy zmienić to do naszej platformy docelowej, który jest **Android** (lub z systemem iOS, jeśli po wraz z projektu iOS).

![](walkthrough-images/image11.png "Zwróć uwagę, że jest to Windows domyślnie, aby zmienić to platformy docelowej, która jest Android lub iOS, jeśli po wraz z projektu iOS")


# <a name="processing-files-in-the-monogame-pipelinetool"></a>Przetwarzanie plików w MonoGame PipelineTool

Następnie, które będą dodawane zawartości do naszej **ContentProject**. Dla tego projektu które będą dodawane pliki w katalogu głównym projektu, ale większych projektów zwykle zorganizuje swoją zawartość w folderach.

Dodamy dwa pliki do naszej projektu:

 - A **.png** pliku, który będzie używany do rysowania sprite. Ten plik można [pobrana w tym miejscu](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true).
 - A **.spritefont** pliku, który będzie używany do rysowania tekstu na ekranie. Narzędzie ContentPipeline obsługuje tworzenie nowych plików .spritefont, więc nie ma żadnych plików do pobrania.


## <a name="adding-a-png-file"></a>Dodawanie PNG

Aby dodać **.png** plik do projektu, firma Microsoft będzie najpierw skopiuj go do katalogu projektu potok, który ma **.mgcb** rozszerzenia.

![](walkthrough-images/image12.png "Dodaj plik PNG do projektu")

Następnie dodamy plik do projektu potoku. Aby to zrobić za pomocą narzędzia MonoGame potoku, wybierz **Edytuj > Dodaj element...** , wybierz pozycję **ball.png** plik i kliknij przycisk **Otwórz**. Plik będzie teraz częścią projektu zawartości i gdy zaznaczone, wyświetli jego właściwości:

![](walkthrough-images/image13.png "Plik będzie teraz częścią projektu zawartości i, gdy zaznaczone, wyświetli jego właściwości")

Wyślemy wiadomość jest pozostawienie wszystkie wartości konfiguracji domyślnej, jak zmiany nie są wymagane do załadowania pliku .xnb w CocosSharp. Możemy utworzyć plik, wybierając **kompilacji > kompilacji** opcję menu, która uruchomi kompilacji i wyświetlania danych wyjściowych o kompilacji. Można sprawdzić, kompilacja działał poprawnie sprawdzając **zawartości** nowy folder **ball.xnb** pliku:

![](walkthrough-images/image14.png "Sprawdź, czy kompilacja działał poprawnie sprawdzając zawartości folderu dla nowego pliku ball.xnb")


## <a name="adding-a-spritefont-file"></a>Dodawanie pliku .spritefont

Można utworzyć pliku .spritefont za pomocą narzędzia MonoGame potoku. CocosSharp wymaga czcionek w **czcionki** folderu i szablony CocosSharp automatycznie automatycznie Utwórz folder czcionki. Ten folder można dodać do narzędzia do potoku MonoGame wybierając **Edytuj > Dodaj > istniejącego folderu...** . Przejdź do **zawartości** i wybierz polecenie **czcionki** folder i kliknij przycisk **OK**:

![](walkthrough-images/browsetofonts.png "Przejdź do folderu zawartości i wybierz folder, czcionki i kliknij przycisk OK")

Aby dodać nowy plik .sprintefont, kliknij prawym przyciskiem folder czcionki i wybierz **Dodaj > Nowy element...** , wybierz pozycję **opis SpriteFont** , należy wprowadzić nazwę **arial 36**i kliknij przycisk **Ok**. CocosSharp wymaga dokładnie nazewnictwa plików czcionek — muszą być w formacie [FontType]-[FontSize]. Czcionka jest niezgodna z tym formatem nazewnictwa go nie zostanie załadowany przez CocosSharp w czasie wykonywania.

![](walkthrough-images/image15.png "Czcionka jest niezgodna z tym formatem nazewnictwa go nie zostanie załadowany przez CocosSharp w czasie wykonywania")

Plik .spritefont jest rzeczywiście pliku XML, który można edytować w edytorze tekstu, w tym programu Visual Studio dla komputerów Mac. Najbardziej typowe zmienne edytowane w pliku .spritefont `FontName` i `Size` właściwości:


```csharp
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

Firma Microsoft będzie Otwórz plik w edytorze tekstu. Jako naszych **arial 36.spritefont** sugeruje nazwa pozostanie `FontName` jako `Arial` , ale zmiana `Size` do wartości `36`:


```csharp
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
# <a name="using-files-at-runtime"></a>Przy użyciu plików w czasie wykonywania

Pliki .xnb są obecnie wbudowane i gotowe do użycia w naszym projektu. Firma Microsoft będzie się dodawanie plików do programu Visual Studio dla komputerów Mac, a następnie dodamy kod do naszych `GameScene.cs` pliku do ładowania tych plików i je wyświetlić.


## <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Dodawanie plików do programu Visual Studio .xnb dla komputerów Mac

Najpierw dodamy pliki do naszej projektu. W programie Visual Studio dla komputerów Mac, firma Microsoft będzie rozwiń **BouncingGame.Android** projektu, rozwiń **zasoby** folderu, kliknij prawym przyciskiem myszy **zawartości** folder, a następnie wybierz **Dodaj > Dodawanie plików...** Po pierwsze, wybierzemy **ball.xnb** możemy utworzony wcześniej i kliknij przycisk **Otwórz**. Następnie powtórz powyższe kroki, ale Dodaj **arial 36.xnb** pliku. Wybierzemy **pozostawić plik w jego bieżącym podkatalogu** opcji w przypadku, gdy program Visual Studio for Mac zapyta, jak dodać plik. Raz Zakończono oba pliki powinny być częścią naszego projektu:

![](walkthrough-images/image20.png "Raz Zakończono oba pliki powinny być częścią projektu")


## <a name="adding-gamescenecs"></a>Dodawanie GameScene.cs

Utworzymy klasy o nazwie `GameScene,` zawierający obiekty naszych sprite i tekst. Aby to zrobić, kliknij prawym przyciskiem myszy **BouncingGame** (nie BouncingGame.Android) projektu i wybierz **Dodaj > Nowy plik...** . Wybierz **ogólne** kategorii, wybierz opcję **pustą klasę** opcji, a następnie wprowadź nazwę **GameScene**.

Po utworzeniu, firma Microsoft będzie zmodyfikować `GameScene.cs` plik zawiera następujący kod:


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

Firma Microsoft nie będzie w niniejszym dokumencie powyższym kodzie ponieważ znajdują się w pracy z obiektami visual CocosSharp, takich jak CCSprite i CCLabelTtf [Przewodnik wprowadzający CocosSharp](~/graphics-games/cocossharp/first-game/index.md).

Musimy także Dodaj kod, aby załadować naszych nowo utworzonego `GameScene`. W tym otworzymy `GameAppDelegate.cs` pliku (który znajduje się w **BouncingGame** PCL) i zmodyfikuj `ApplicationDidFinishLaunching` metody tak wygląda następująco:


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

Podczas uruchamiania, naszych gry będą wyglądać jak:

![](walkthrough-images/image1.png "Podczas uruchamiania, będzie wyglądać gry")


# <a name="summary"></a>Podsumowanie

W tym poradniku pokazano sposób użycia narzędzia MonoGame potoku do tworzenia plików .xnb z pliku wejściowego PNG, a także sposób tworzenia nowego pliku .xnb z nowo utworzonych .sprintefont pliku. Opisano również sposób struktury projektów CocosSharp .xnb plików i obciążenia tych plików w czasie wykonywania.

## <a name="related-links"></a>Linki pokrewne

- [MonoGame pliki do pobrania](http://www.monogame.net/downloads/)
- [Dokumentacja MonoGame potoku](http://www.monogame.net/documentation/?page=Pipeline)
- [Uruchamianie BouncingGame projektu dla systemu Android (przykład)](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [Ball.PNG grafiki (przykład)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
