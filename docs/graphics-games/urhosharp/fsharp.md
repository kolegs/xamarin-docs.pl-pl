---
title: 'UrhoSharp programowania w języku F #'
description: 'Jak utworzyć prostą aplikację UrhoSharp przy użyciu języka F # w programie Visual Studio dla komputerów Mac'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: d2b21204d1d328831419308827e1a2de2b6aef1c
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="programming-urhosharp-with-f"></a>UrhoSharp programowania w języku F #

_Jak utworzyć prostą aplikację UrhoSharp przy użyciu języka F # w programie Visual Studio dla komputerów Mac_

UrhoSharp mogą być programowane w języku F # za pomocą tej samej biblioteki i pojęcia używane przez programistów C#. [Przy użyciu UrhoSharp](~/graphics-games/urhosharp/using.md) artykuł zawiera omówienie aparatu UrhoSharp i należy przeczytać przed w tym artykule.

Jak wiele bibliotek pochodzących na świecie C++ wiele UrhoSharp zwracają wartości logiczne i wskazujący powodzenie lub niepowodzenie liczb całkowitych. Należy używać `|> ignore` ignoruje te wartości.

[Przykładowy program](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) jest UrhoSharp z F # "Hello World".

## <a name="creating-an-empty-project"></a>Pusty projekt do tworzenia

Nie szablonów F # dla UrhoSharp jeszcze dostępne, tak utworzyć projekt UrhoSharp możesz albo rozpoczyna się od [próbki](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) lub wykonaj następujące kroki:

1. W programie Visual Studio dla komputerów Mac, Utwórz nową **rozwiązania**. Wybierz **systemu iOS > aplikacji > jednej aplikacji widoku** i wybierz **F #** jako język implementacji. 
1. Usuń **Main.storyboard** pliku. Otwórz **Info.plist** pliku i w **iPhone / iPod informacji o wdrożeniu** okienku Usuń `Main` ciąg w **interfejsu Main** listy rozwijanej.
1. Usuń **ViewController.fs** pliku.

## <a name="building-hello-world-in-urho"></a>Kompilowanie Hello World w Urho

Teraz można przystąpić do rozpoczęcia Definiowanie klas Twoja gra. Co najmniej, należy zdefiniować podklasą `Urho.Application` i zastąp jego `Start` metody. Aby utworzyć ten plik, kliknij prawym przyciskiem myszy projekt F #, wybierz **Dodaj nowy plik...**  i dodaj pustą klasę F # do projektu. Nowy plik zostanie dodany na końcu listy plików w projekcie, ale przeciągnij go, aby był wyświetlany *przed* jest używany w **AppDelegate.fs**.

1. Dodaj odwołanie do pakietu Urho NuGet.
1. Z istniejącego projektu Urho kopiowania (duże) katalogów **CoreData /** i **danych /** w swoim projekcie **zasobów /** katalogu. W F # projektu, kliknij prawym przyciskiem myszy **zasobów** folder i użyj **Dodawanie lub Dodaj istniejący Folder** jest dodanie wszystkich tych plików do projektu.

Struktury projektu powinna wyglądać tak jak:

![](fsharp-images/solutionpane.png "Struktury projektu powinna wyglądać tak jak")

Zdefiniować klasy nowo utworzony jako podtypem `Urho.Application` i zastąp jego `Start` metody:

```csharp
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

Kod jest bardzo proste. Używa `Urho.Gui.Text` klasy do wyświetlenia ciągu wyrównane z określonego rozmiaru czcionek i kolorów. 

Zanim będzie można uruchomić ten kod, jednak UrhoSharp musi zostać zainicjowany. 

Otwórz plik AppDelegate.fs i zmodyfikować `FinishedLaunching` metody w następujący sposób:

```csharp
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

`ApplicationOptions.Default` Zawiera domyślne opcje aplikacji tryb poziomo. Przekaż te `ApplicationOptions` do domyślnego konstruktora dla Twojego `Application` podklasy (należy pamiętać, że po zdefiniowaniu `HelloWorld` klasy wiersza `inherit Application(o)` wywołuje konstruktor klasy podstawowej). 

`Run` Metody z `Application` inicjuje program. Jest on zdefiniowany jako zwracanie `int`, które mogą być przetwarzane potokowo do `ignore`. 

Program wynikowy powinien wyglądać:

![](fsharp-images/helloworldfsharp.png "Program wynikowy powinien wyglądać")








## <a name="related-links"></a>Linki pokrewne

- [Przeglądaj w serwisie GitHub (przykład)](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
