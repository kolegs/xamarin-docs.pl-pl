---
title: Wprowadzenie do OpenTK
description: Ten artykuł zawiera wprowadzenie do pracy z OpenTK w aplikacji Xamarin.Mac. Obejmuje ona tworzenie i utrzymywanie okna gry, renderowania prostego obiektu i wyświetlanie obiektu dla użytkownika.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d5f19dac8dc362e1ac4a36cbe5cf5db3e31ae363
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-opentk"></a>Wprowadzenie do OpenTK

OpenTK (Otwórz zestaw narzędzi) jest zaawansowanych, niskiego poziomu C# biblioteki, która ułatwia pracę z OpenGL i OpenCL OpenAL. OpenTK może służyć do gier, aplikacji naukowych lub inne projekty, które wymagają grafiki 3D, audio lub obliczeniowych funkcji. Ten artykuł zawiera krótkie wprowadzenie do używania OpenTK w aplikacji Xamarin.Mac.

[![](opentk-images/intro01.png "Uruchom przykładową aplikację")](opentk-images/intro01.png#lightbox)

W tym artykule firma Microsoft będzie obejmować podstawy OpenTK w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` poleceń używany do przesyłania w górę klas C# do obiektów języka Objective C i elementy interfejsu użytkownika.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>O OpenTK

Jak już wspomniano, OpenTK (Otwórz zestaw narzędzi) jest zaawansowanych, niskiego poziomu C# biblioteki, która ułatwia pracę z OpenGL i OpenCL OpenAL. Używanie OpenTK w aplikacji Xamarin.Mac zapewnia następujące funkcje:

- **Szybkie opracowywanie** -OpenTK zawiera typy danych i dokumentacji kodu, do poprawy kodowania przepływu pracy i catch błędy łatwiej a wcześniej.
- **Łatwa integracja** -OpenTK zaprojektowano tak, aby łatwo zintegrować ją z aplikacji .NET.
- **Licencja zezwalająca** -OpenTK jest dystrybuowane na podstawie licencji MIT/X11 i jest całkowicie bezpłatna.
- **RTF, bezpieczne powiązania** — OpenTK obsługuje najnowsze wersje OpenGL, OpenGL | ES, OpenAL i OpenCL z obciążeniem automatycznego rozszerzenia dokumentacji sprawdzanie i wbudowanego błędów.
- **Elastyczne opcje GUI** -OpenTK zapewnia natywnego, wysokiej wydajności gry okna zaprojektowany specjalnie z myślą gier i Xamarin.Mac.
- **Do pełnego zarządzania, zgodne ze specyfikacją CLS kodu** -OpenTK obsługuje 32-bitowe i 64-bitowe wersje macOS z niezarządzanych bibliotek.
- **3D Toolkit matematyczne** dostarcza OpenTK `Vector`, `Matrix`, `Quaternion` i `Bezier` struktur za pomocą jego 3D Toolkit matematycznych.

OpenTK może służyć do gier, aplikacji naukowych lub inne projekty, które wymagają grafiki 3D, audio lub obliczeniowych funkcji.

Aby uzyskać więcej informacji, zobacz [Toolkit Otwórz](http://www.opentk.com) witryny sieci Web.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>OpenTK Szybki Start

Jako szybkie wprowadzenie do używania OpenTK w aplikacji Xamarin.Mac zamierzamy utworzyć prostą aplikację, która otwiera widok grę, renderuje prosty trójkąt w tym widoku i attachs widoku gry aplikacji Mac główne okno, aby wyświetlić trójkąt do użytkownika.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>Uruchamianie nowego projektu

Uruchom program Visual Studio dla komputerów Mac i utworzyć nowe rozwiązanie Xamarin.Mac. Wybierz **Mac** > **aplikacji** > **ogólne** > **Cocoa aplikacji**:

[![](opentk-images/sample01.png "Dodawanie nowej aplikacji Cocoa")](opentk-images/sample01.png#lightbox)

Wprowadź `MacOpenTK` dla **Nazwa projektu**:

[![](opentk-images/sample02.png "Ustawienie nazwy projektu")](opentk-images/sample02.png#lightbox)

Kliknij przycisk **Utwórz** przycisk, aby utworzyć nowy projekt.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>W tym OpenTK

Zanim użyjesz TK otwartych w aplikacji Xamarin.Mac należy dołączyć odwołanie do zestawu OpenTK. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **odwołania** i wybierz polecenie **odwołuje się do edycji...** .

Zaznacz przez `OpenTK` i kliknij przycisk **OK** przycisk:

[![](opentk-images/sample03.png "Edytowanie odwołania do projektu")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>Przy użyciu OpenTK

Z nowo utworzonego projektu, kliknij dwukrotnie `MainWindow.cs` w pliku **Eksploratora rozwiązań** go otworzyć do edycji. Wprowadź `MainWindow` wygląd klasy podobne do poniższych:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacOpenTK
{
    public partial class MainWindow : NSWindow
    {
        #region Computed Properties
        public MonoMacGameView Game { get; set; }
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create new Game View and replace the window content with it
            Game = new MonoMacGameView(ContentView.Frame);
            ContentView = Game;

            // Wire-up any required Game events
            Game.Load += (sender, e) =>
            {
                // TODO: Initialize settings, load textures and sounds here
            };

            Game.Resize += (sender, e) =>
            {
                // Adjust the GL view to be the same size as the window
                GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
            };

            Game.UpdateFrame += (sender, e) =>
            {
                // TODO: Add any game logic or physics
            };

            Game.RenderFrame += (sender, e) =>
            {
                // Setup buffer
                GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
                GL.MatrixMode(MatrixMode.Projection);

                // Draw a simple triangle
                GL.LoadIdentity();
                GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
                GL.Begin(BeginMode.Triangles);
                GL.Color3(Color.MidnightBlue);
                GL.Vertex2(-1.0f, 1.0f);
                GL.Color3(Color.SpringGreen);
                GL.Vertex2(0.0f, -1.0f);
                GL.Color3(Color.Ivory);
                GL.Vertex2(1.0f, 1.0f);
                GL.End();

            };

            // Run the game at 60 updates per second
            Game.Run(60.0);
        }
        #endregion
    }
}
```

Przejdź przez ten kod szczegółowo poniżej.

<a name="Required_APIs" />

### <a name="required-apis"></a>Wymaganych interfejsów API

Kilka odwołań są wymagane do użycia w klasie Xamarin.Mac OpenTK. Na początku definicji firma Microsoft uwzględniła następujące `using` instrukcji:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using CoreGraphics;
```
To minimalny zestaw będzie wymagane dla klasy przy użyciu OpenTK.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>Dodawanie widoku gier

Następnie należy utworzyć widok gry zawiera wszystkich naszych interakcji z OpenTK i wyświetlić wyniki. Użyliśmy następującego kodu:

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

W tym miejscu wprowadziliśmy widoku gry taki sam rozmiar jak nasze główne okno Mac i zastąpione nowy widok zawartości okna `MonoMacGameView`. Ponieważ możemy zastąpić istniejącą zawartość okna, naszych nadać widok będzie zmieniany automatycznie, gdy zmieni się rozmiar okna głównego.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>Odpowiadanie na zdarzenia

Istnieje kilka domyślne zdarzenia, które każdego widoku gry powinny odpowiadać. W tej sekcji opisano zdarzenia głównego wymagane.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Zdarzenie obciążenia

`Load` Zdarzeń jest używana do załadowania zasobów z dysku, takich jak obrazy, tekstury lub utworów muzycznych. W naszym prostej aplikacji testów nie użyto `Load` zdarzeń, ale zostały uwzględnione dla odwołania:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>Zdarzenie zmiany rozmiaru

`Resize` Zdarzeń powinna być wywoływana za każdym razem, gdy zmieni się rozmiar widoku gry. Dla aplikacji przykładowej, firma Microsoft wprowadza okienka ekranu GL taki sam rozmiar jak naszych widoku grę, (co jest automatycznej zmiany rozmiaru okna głównego Mac) z następującym kodem:

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>Zdarzenie UpdateFrame

`UpdateFrame` Zdarzeń służy do obsługi danych wejściowych użytkownika, obiekt pozycji, uruchamiania fizycznych lub AI obliczeń. W naszym prostej aplikacji testów nie użyto `UpdateFrame` zdarzeń, ale zostały uwzględnione dla odwołania: 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> Nie ma implementacji Xamarin.Mac OpenTK `Input API`, więc będą musieli używać firmy Apple, pod warunkiem obsługuje interfejsy API, aby dodać klawiatury i myszy. Opcjonalnie można utworzyć niestandardowego wystąpienia `MonoMacGameView` i zastąpić `KeyDown` i `KeyUp` metody.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>Zdarzenie RenderFrame

`RenderFrame` Zdarzenia zawiera kod, który jest używany do renderowania (rysowania) grafiki. Na przykład aplikacji firma Microsoft są wypełnianie widoku gry prosty trójkąt: 

```csharp
Game.RenderFrame += (sender, e) =>
{
    // Setup buffer
    GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
    GL.MatrixMode(MatrixMode.Projection);

    // Draw a simple triangle
    GL.LoadIdentity();
    GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
    GL.Begin(BeginMode.Triangles);
    GL.Color3(Color.MidnightBlue);
    GL.Vertex2(-1.0f, 1.0f);
    GL.Color3(Color.SpringGreen);
    GL.Vertex2(0.0f, -1.0f);
    GL.Color3(Color.Ivory);
    GL.Vertex2(1.0f, 1.0f);
    GL.End();

};
```

Zwykle renderowania kodu zostanie jest wywołaniem `GL.Clear` usunąć wszelkie istniejące elementy przed narysowaniem nowe elementy.

> [!IMPORTANT]
> Dla wersji Xamarin.Mac OpenTK **nie** wywołać `SwapBuffers` metody z `MonoMacGameView` wystąpienia na końcu renderowania kodu. To spowoduje, że widok gry do światła szybko zamiast wyświetlania widoku renderowanym.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>Uruchomiona gier widoku

Za pomocą wszystkich wymaganych zdarzeń definiować i widoku gry dołączona do głównego okna Mac naszej aplikacji, są odczytano uruchomienia gry widoku i wyświetlania grafiki naszych. Użyj następującego kodu:

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

Jest przekazywana w szybkość klatek żądaną które chcemy widoku grę, aby zaktualizować na, w naszym przykładzie Wybraliśmy `60` klatek na sekundę (tym samym częstotliwość odświeżania jako normalne TV).

Umożliwia uruchamianie aplikacji i wyświetlić dane wyjściowe:

[![](opentk-images/intro01.png "Przykładowe dane wyjściowe aplikacji")](opentk-images/intro01.png#lightbox)

Jeśli firma Microsoft naszych okno widoku gry będzie również znajdują się i trójkąt zostanie zmiany rozmiaru i także zaktualizować w czasie rzeczywistym.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>Gdzie dalej?

Podstawowe informacje dotyczące pracy z OpenTk w aplikacji Xamarin.mac gotowe Oto kilka sugestii, jakie można wypróbować dalej:

- Spróbuj zmienić kolor trójkąta i kolor tła w widoku gry `Load` i `RenderFrame` zdarzenia.
- Wprowadź trójkąt zmienić kolor, kiedy użytkownik klawisz w `UpdateFrame` i `RenderFrame` zdarzenia lub utworzyć własne `MonoMacGameView` klasy i Zastąp `KeyUp` i `KeyDown` metody.
- Wprowadź trójkąt porusza się na ekranie przy użyciu pamiętać kluczy w `UpdateFrame` zdarzeń. Wskazówka: Użyj `Matrix4.CreateTranslation` metodę w celu utworzenia macierzy tłumaczenia i wywołanie `GL.LoadMatrix` metodę, aby załadować go w `RenderFrame` zdarzeń.
- Użyj `for` pętli do renderowania kilka trójkąty w `RenderFrame` zdarzeń.
- Obróć aparatu, aby przekazać inny widok trójkąt w przestrzeni 3D. Wskazówka: Użyj `Matrix4.CreateTranslation` metodę w celu utworzenia macierzy tłumaczenia i wywołanie `GL.LoadMatrix` metody go załadować. Można również użyć `Vector2`, `Vector3`, `Vector4` i `Matrix4` klasy dla manipulacje aparatu.

Aby uzyskać więcej przykładów, zobacz [GitHub przykłady OpenTK](https://github.com/opentk/opentk/tree/master/Source/Examples) repozytorium. Zawiera listę oficjalnego przykłady użycia OpenTK. Należy dostosować te przykłady korzystania z wersji Xamarin.Mac OpenTK.

Xamarin.Mac bardziej złożonych przykładem implementacji OpenTK, zobacz nasze [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/) próbki.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało krótki przegląd pracy z OpenTK w aplikacji Xamarin.Mac. Widzieliśmy sposób tworzenia okna gier, jak dołączyć okna gry do okna Mac oraz sposób renderowania prostego kształtu w oknie gry.

## <a name="related-links"></a>Linki pokrewne

- [MacOpenTK (przykład)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (przykład)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Praca z systemu Windows](~/mac/user-interface/window.md)
- [Otwórz zestaw narzędzi](http://www.opentk.com)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do systemu Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
