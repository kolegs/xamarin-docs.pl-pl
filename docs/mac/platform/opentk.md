---
title: Wprowadzenie do biblioteki OpenTK platformie Xamarin.Mac
description: Ten artykuł zawiera wprowadzenie do pracy z biblioteki OpenTK w aplikacji platformy Xamarin.Mac. Obejmuje on tworzenia i utrzymywania okna gry, renderowanie prostego obiektu oraz do wyświetlania obiektu użytkownika.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 02cd32ac3016a3556cac8611ff839336f3089235
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780594"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Wprowadzenie do biblioteki OpenTK platformie Xamarin.Mac

Biblioteki OpenTK (Otwórz zestawu narzędzi) jest zaawansowanych, niskiego poziomu bibliotekę języka C#, która ułatwia pracę z OpenGL i OpenCL OpenAL. Biblioteki OpenTK może służyć do gier, naukowych lub inne projekty, które wymagają Grafika 3D, audio lub obliczeniową funkcje. Ten artykuł zawiera krótkie wprowadzenie do korzystania z biblioteki OpenTK w aplikacji platformy Xamarin.Mac.

[![](opentk-images/intro01.png "Uruchamianie przykładowej aplikacji")](opentk-images/intro01.png#lightbox)

W tym artykule omówiono podstawowe informacje dotyczące biblioteki OpenTK w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` poleceń Umożliwia o komunikacji sieciowej w górę klas języka C# do języka Objective-C obiektów i elementów interfejsu użytkownika.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>Informacje biblioteki OpenTK

Jak wspomniano powyżej, OpenTK (Otwórz zestawu narzędzi) jest zaawansowanych, niskiego poziomu bibliotekę języka C#, która ułatwia pracę z OpenGL i OpenCL OpenAL. W aplikacji platformy Xamarin.Mac przy użyciu biblioteki OpenTK oferuje następujące funkcje:

- **Błyskawiczne** -OpenTK zawiera typy danych i wbudowana dokumentacja, aby poprawić pracy kodowania i wyłapywanie błędów, łatwiej i szybciej.
- **Łatwa integracja** -OpenTK zaprojektowano tak, aby łatwo zintegrować ją z aplikacji .NET.
- **Licencja zezwalająca** -OpenTK jest rozpowszechniany na licencji MIT/X11 i jest całkowicie bezpłatna.
- **Rozbudowane, bezpiecznych powiązań** -OpenTK obsługuje najnowsze wersje ze specyfikacji OpenGL, ze specyfikacji OpenGL | ES, OpenAL i OpenCL przy użyciu ładowania rozszerzenia automatyczne sprawdzenie i wbudowanej dokumentacji błędów.
- **Elastyczne opcje graficznego interfejsu użytkownika** -OpenTK zapewnia natywnych, okna gry o wysokiej wydajności przeznaczony specjalnie dla gier i aplikacji platformy Xamarin.Mac.
- **W pełni zarządzana, zgodne ze specyfikacją CLS kodu** -OpenTK obsługuje 32-bitowych i 64-bitowej wersji systemu macOS za pomocą żadnych niezarządzanych bibliotek.
- **3D Toolkit matematyczne** dostarcza OpenTK `Vector`, `Matrix`, `Quaternion` i `Bezier` struktury za pomocą jego 3D Toolkit matematyczne.

Biblioteki OpenTK może służyć do gier, naukowych lub inne projekty, które wymagają Grafika 3D, audio lub obliczeniową funkcje.

Aby uzyskać więcej informacji, zobacz [Toolkit Otwórz](http://www.opentk.com) witryny sieci Web.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>Przewodnik Szybki Start OpenTK

Jako krótkie wprowadzenie do korzystania z biblioteki OpenTK w aplikacji rozszerzenia Xamarin.Mac zamierzamy utworzyć prostą aplikację, która spowoduje otwarcie widoku gry, powoduje wyświetlenie prosty trójkąt, w tym widoku i attachs widoku grę do okna głównego aplikacji Mac, aby wyświetlić trójkąt do użytkownika.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>Uruchamianie nowego projektu

Uruchom program Visual Studio dla komputerów Mac i Utwórz nowe rozwiązanie platformy Xamarin.Mac. Wybierz **Mac** > **aplikacji** > **ogólne** > **aplikacja cocoa dla**:

[![](opentk-images/sample01.png "Dodawanie nowej aplikacji Cocoa")](opentk-images/sample01.png#lightbox)

Wprowadź `MacOpenTK` dla **nazwy projektu**:

[![](opentk-images/sample02.png "Ustawianie nazwy projektu")](opentk-images/sample02.png#lightbox)

Kliknij przycisk **Utwórz** przycisk, aby utworzyć nowy projekt.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>W tym OpenTK

Zanim użyjesz TK otwartych w aplikacji platformy Xamarin.Mac, należy dołączyć odwołanie do biblioteki OpenTK zestawu. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **odwołania** i wybierz polecenie **Edytuj odwołania...** .

Należy zaznaczyć przez `OpenTK` i kliknij przycisk **OK** przycisku:

[![](opentk-images/sample03.png "Edytowanie odwołań w projekcie")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>Przy użyciu biblioteki OpenTK

Przy użyciu nowo utworzonego projektu, kliknij dwukrotnie `MainWindow.cs` w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji. Wprowadź `MainWindow` wygląd klasy, jak pokazano poniżej:

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

Zajmijmy się ten kod poniżej.

<a name="Required_APIs" />

### <a name="required-apis"></a>Wymagane interfejsy API

Kilka odwołań są wymagane do użycia biblioteki OpenTK w klasie platformy Xamarin.Mac. Na początku definicji wprowadzono następujące `using` instrukcji:

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
To minimalny zestaw będzie wymagana dla każdej klasy przy użyciu biblioteki OpenTK.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>Dodawanie widoku gier

Następnie należy utworzyć widok gra zawierają wszystkie nasze interakcje z biblioteki OpenTK i wyświetlić wyniki. Użyliśmy następującego kodu:

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

W tym miejscu wprowadziliśmy widoku gry taki sam rozmiar jak okno Mac nasze główne i zastąpione zawartości widoku okna przy użyciu nowego `MonoMacGameView`. Ponieważ możemy zastąpić istniejącą zawartość okna, widok naszej udostępniła zostanie automatycznie dopasowane, gdy zmieniany jest rozmiar Windows Main.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>Odpowiadanie na zdarzenia

Istnieje kilka domyślne zdarzenia, które każdy widok Gra powinna odpowiadać na. W tej sekcji opisano główne zdarzeń wymagane.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Załadowane zdarzenie

`Load` Zdarzeń jest miejscem, które można załadować zasobów z dysku, takich jak obrazy, tekstury lub utworów muzycznych. W naszym prostym, aplikacja testowa nie użyto `Load` zdarzenia, ale wprowadzono odwołania:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>Zdarzenie zmiany rozmiaru

`Resize` Zdarzeń powinna być wywoływana za każdym razem, gdy zmiany rozmiaru widoku gry. Dla nasza Przykładowa aplikacja wprowadzamy okienka ekranu GL taki sam rozmiar jak widok nasze gry, (który ma być automatycznie poddany zmianie rozmiaru przez okno główne Mac) z następującym kodem:

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>Zdarzenie UpdateFrame

`UpdateFrame` Zdarzeń służy do obsługi danych wejściowych użytkownika, położenie obiektu, wykonywania fizyki lub obliczeń sztucznej Inteligencji. W naszym prostym, aplikacja testowa nie użyto `UpdateFrame` zdarzenia, ale wprowadzono odwołania: 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> Nie ma implementacji platformy Xamarin.Mac OpenTK `Input API`, więc musisz użyć firmy Apple, pod warunkiem obsługuje interfejsy API, aby dodać klawiatury i myszy. Opcjonalnie można utworzyć niestandardowego wystąpienia `MonoMacGameView` i zastąpić `KeyDown` i `KeyUp` metody.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>Zdarzenie RenderFrame

`RenderFrame` Zdarzenie zawiera kod, który jest używany do renderowania (rysowania) grafiki. Przykład aplikacji firma Microsoft są wypełnianie widoku gry prosty trójkąt: 

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

Zazwyczaj kod renderowania pozwoli jest wywołaniem `GL.Clear` usunąć wszelkie elementy istniejących przed rysowane nowych elementów.

> [!IMPORTANT]
> Dla wersji platformy Xamarin.Mac OpenTK **nie** wywołania `SwapBuffers` metody usługi `MonoMacGameView` wystąpienia na końcu kod renderowania. To spowoduje, że widok gry, aby strobe szybko zamiast widoku renderowanego.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>Uruchamianie widoku gier

W przypadku wszystkich wymaganych zdarzeń należy zdefiniować i widoku gry dołączone do głównego okna Mac naszej aplikacji, firma Microsoft są odczytywane do widoku gry uruchamiania i wyświetlania naszych grafiki. Użyj następującego kodu:

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

Przekazanie żądaną klatek, które chcemy widoku grę do aktualizowania w, w tym przykładzie Wybraliśmy `60` klatek na sekundę (tym samym częstotliwość odświeżania jako normalny TV).

Umożliwia uruchamianie aplikacji w języku i wyświetlić dane wyjściowe:

[![](opentk-images/intro01.png "Przykładowe dane wyjściowe aplikacji")](opentk-images/intro01.png#lightbox)

Jeśli firma Microsoft naszych okno Widok gra będzie również znajdują się i trójkąt będzie ze zmienionym rozmiarem i zaktualizowano także w czasie rzeczywistym.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>Gdzie dalej?

Podstawowe informacje dotyczące pracy z biblioteki OpenTk w aplikacji platformy Xamarin.mac gotowe Oto kilka sugestii w zakresie elementów, które należy wypróbować dalej:

- Spróbuj zmienić kolor trójkąta i kolor tła widoku gry w `Load` i `RenderFrame` zdarzenia.
- Wprowadź trójkąt zmienić kolor po użytkownik klawisza w `UpdateFrame` i `RenderFrame` zdarzenia lub wprowadzić własny, niestandardowy `MonoMacGameView` klasy, a także Przesłoń `KeyUp` i `KeyDown` metody.
- Wprowadzić trójkąt przy użyciu pamiętać kluczy w po ekranie poruszają `UpdateFrame` zdarzeń. Wskazówka: Użyj `Matrix4.CreateTranslation` metodę w celu utworzenia macierzy tłumaczenia i wywołanie `GL.LoadMatrix` metodę, aby załadować go w `RenderFrame` zdarzeń.
- Użyj `for` pętli do renderowania kilka trójkąty w `RenderFrame` zdarzeń.
- Obracanie kamery, aby nadać inny widok trójkąta w przestrzeni 3D. Wskazówka: Użyj `Matrix4.CreateTranslation` metodę w celu utworzenia macierzy tłumaczenia i wywołanie `GL.LoadMatrix` metodę, aby go załadować. Można również użyć `Vector2`, `Vector3`, `Vector4` i `Matrix4` klasy dla aparatu manipulacji.

Aby uzyskać więcej przykładów, zobacz [OpenTK Samples GitHub](https://github.com/opentk/opentk/tree/master/Source/Examples) repozytorium. Zawiera on oficjalną listę przy użyciu biblioteki OpenTK przykłady. Należy dostosować te przykłady korzystania z wersji platformy Xamarin.Mac OpenTK.

Bardziej złożonym przykładem Xamarin.Mac OpenTK implementacji, zobacz nasze [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/) próbki.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta krótkie omówienie pracy z biblioteki OpenTK w aplikacji platformy Xamarin.Mac. Firma Microsoft pokazano, jak można utworzyć okna gry, jak dołączyć okna gry do okna Mac oraz sposób renderowania prosty kształt w oknie gry.

## <a name="related-links"></a>Linki pokrewne

- [MacOpenTK (przykład)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (przykład)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Praca z Windows](~/mac/user-interface/window.md)
- [Otwórz zestaw narzędzi](http://www.opentk.com)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
