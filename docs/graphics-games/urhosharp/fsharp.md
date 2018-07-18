---
title: 'Programowanie na platformie UrhoSharp z F #'
description: 'W tym dokumencie opisano sposób tworzenia aplikacja proste hello world na platformie UrhoSharp przy użyciu języka F # w programie Visual Studio dla komputerów Mac.'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: a4e1a31a2591c799a153e1333e4a4a4a0719a107
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111202"
---
# <a name="programming-urhosharp-with-f"></a>Programowanie na platformie UrhoSharp z F #

Może być zaprogramowany platformie UrhoSharp w języku F # przy użyciu tego samego pojęcia używane przez programistów języka C# i bibliotek. [Przy użyciu platformy UrhoSharp](~/graphics-games/urhosharp/using.md) artykuł zawiera omówienie aparatu UrhoSharp i powinny być odczytywane przed w tym artykule.

Podobnie jak wiele bibliotek, które są tworzone w świecie C++ wiele UrhoSharp zwracają wartości logicznych lub liczby całkowite oznaczający powodzenie lub niepowodzenie. Należy używać `|> ignore` ignoruje te wartości.

[Przykładowy program](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) jest "Hello World" na platformie UrhoSharp z języka F #.

## <a name="creating-an-empty-project"></a>Tworzenie pustego projektu

Brak szablonów F # dla platformy UrhoSharp jeszcze dostępne utworzyć projekt na platformie UrhoSharp możesz albo rozpoczyna się od [przykładowe](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) lub wykonaj następujące kroki:

1. W programie Visual Studio dla komputerów Mac, Utwórz nową **rozwiązania**. Wybierz **dla systemu iOS > aplikacji > Aplikacja pojedynczego widoku** i wybierz **F #** jako implementacji języka. 
1. Usuń **Main.storyboard** pliku. Otwórz **Info.plist** pliku i **iPhone / iPod informacje o wdrożeniu** okienku Usuń `Main` ciągu w **głównego interfejsu** listy rozwijanej.
1. Usuń **ViewController.fs** pliku.

## <a name="building-hello-world-in-urho"></a>Tworzenie Hello World w Urho

Teraz można przystąpić do rozpoczęcia Definiowanie klas swoją grę. Co najmniej należy zdefiniować podklasą `Urho.Application` i zastąpić jej `Start` metody. Aby utworzyć ten plik, kliknij prawym przyciskiem myszy na projekt języka F #, wybierz polecenie **Dodaj nowy plik...**  i dodać pustą klasę F # do projektu. Nowy plik zostanie dodany na końcu listy plików w projekcie, ale należy go przeciągnąć, wygląda tak, jakby *przed* jest używany w **AppDelegate.fs**.

1. Dodaj odwołanie do pakietu Urho NuGet.
1. Skopiuj katalogów (duża liczba godzin) z istniejącego projektu Urho **CoreData /** i **danych /** w swoim projekcie **zasobów /** katalogu. W projekcie języka F #, kliknij prawym przyciskiem myszy **zasobów** folder i użyj **Dodaj / Dodaj istniejący Folder** można dodać wszystkie te pliki do projektu.

Do struktury projektu powinna wyglądać tak jak:

![](fsharp-images/solutionpane.png "Struktura projektu powinna wyglądać tak jak")

Zdefiniuj nowo utworzone klasy jako podtypem `Urho.Application` i zastąpić jej `Start` metody:

```fsharp
namespace HelloWorldUrho1

open Urho
open Urho.Gui
open Urho.iOS

type HelloWorld(o : ApplicationOptions) =
    inherit Urho.Application(o) 

override this.Start() = 
        let cache = this.ResourceCache
        let helloText = new Text()

        helloText.Value <- "Hello World from Urho3D, Mono, and F#"
        helloText.HorizontalAlignment <- HorizontalAlignment.Center
        helloText.VerticalAlignment <- VerticalAlignment.Center

        helloText.SetColor (new Color(0.f, 1.f, 0.f))
        let f = cache.GetFont("Fonts/Anonymous Pro.ttf")

        helloText.SetFont(f, 30) |> ignore
                  
        this.UI.Root.AddChild(helloText)
            
```

Kod jest bardzo proste. Używa ona `Urho.Gui.Text` klasy do wyświetlenia ciągu wyrównane z określonego rozmiaru czcionek i kolorów. 

Przed uruchomieniem tego kodu, jednak na platformie UrhoSharp musi zostać zainicjowany. 

Otwórz plik AppDelegate.fs i zmodyfikować `FinishedLaunching` metody w następujący sposób:

```fsharp
namespace HelloWorldUrho1

open System
open UIKit
open Foundation
open Urho
open Urho.iOS

[<Register ("AppDelegate")>]
type AppDelegate () =
    inherit UIApplicationDelegate ()

    override this.FinishedLaunching (app, options) =
        let o = ApplicationOptions.Default
     
        let g = new HelloWorld(o)
        g.Run() |> ignore
       
        true
```

`ApplicationOptions.Default` Zapewnia domyślne opcje dla aplikacji w trybie poziomym. Przekaż `ApplicationOptions` do domyślnego konstruktora dla Twojego `Application` podklasy (należy pamiętać, że po zdefiniowaniu `HelloWorld` klasy wiersz `inherit Application(o)` wywołuje konstruktora klasy podstawowej). 

`Run` Metody usługi `Application` inicjuje program. Jest on zdefiniowany jako zwracanie `int`, które mogą być przesyłane potokiem do `ignore`. 

Program wynikowy powinien wyglądać:

![](fsharp-images/helloworldfsharp.png "Program wynikowy powinno wyglądać podobnie do")








## <a name="related-links"></a>Linki pokrewne

- [Przeglądaj w usłudze GitHub (przykład)](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
