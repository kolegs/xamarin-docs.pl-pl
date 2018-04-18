---
title: Wprowadzenie do UrhoSharp
description: Zapewnia to krótkie wprowadzenie do pojęć dotyczących UrhoSharp
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 6c46d7648d1f1bb8863abe092bae5c44850d3cf1
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="an-introduction-to-urhosharp"></a>Wprowadzenie do UrhoSharp

_Zapewnia to krótkie wprowadzenie do pojęć dotyczących UrhoSharp_

![UrhoSharp logo](introduction-images/urhosharp-icon.png)

UrhoSharp to zaawansowany aparat 3D gry dla deweloperów platformy Xamarin i .NET.  Podobnie w duchu SceneKit i SpriteKit firmy Apple, a obejmują fizycznych, nawigacji, sieci i wiele więcej przy zachowaniu cross platform.

Jest powiązanie .NET [Urho3D](http://urho3d.github.io/) aparat i umożliwia deweloperom napisać kod obsługujący różne platformy, który może współpracować z systemami Android, iOS, Windows i Mac o tej samej ścieżki bazowej kodu i może renderować systemów zarówno OpenGL i Direct3D.

UrhoSharp jest aparatem gier wiele funkcji poza pola:

- Zaawansowane renderowania grafiki 3D
- [Fizyka symulacji](https://developer.xamarin.com/api/namespace/Urho.Physics/) (przy użyciu biblioteki punktorów)
- [Obsługa sceny](https://developer.xamarin.com/api/type/Urho.Scene/)
- Obsługa await Async
- [Akcje przyjazną interfejsu API](https://developer.xamarin.com/api/namespace/Urho.Actions/)
- [2D integrację sceny 3W](https://developer.xamarin.com/api/namespace/Urho.Urho2D/)
- [Renderowanie czcionki z FreeType](https://developer.xamarin.com/api/type/Urho.Gui.FontFaceFreeType/)
- [Na kliencie i serwerze sieć możliwości](https://developer.xamarin.com/api/namespace/Urho.Network/)
- [Importowanie wielu zasobów](https://developer.xamarin.com/api/namespace/Urho.Resources/) (z otwarte zasoby biblioteki)
- [Siatka nawigacji i pathfinding](https://developer.xamarin.com/api/namespace/Urho.Navigation/) (przy użyciu zmienione/przekierowania)
- [Generowanie wypukłych powłoki wykrywania kolizji](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) (przy użyciu StanHull)
- [Odtwarzanie](https://developer.xamarin.com/api/namespace/Urho.Audio/) (z **libvorbis**)

## <a name="getting-started"></a>Wprowadzenie

UrhoSharp wygodnie jest rozpowszechniany jako [pakietu NuGet](https://www.nuget.org/) i jego mogą być dodawane do projektów C# lub języka F # kierowanych systemu Windows, Mac, Android lub iOS.  NuGet zawiera zarówno biblioteki wymagane do uruchamiania programu, jak również zasoby podstawowe (CoreData) używanego przez aparat.

### <a name="urho-as-a-portable-class-library"></a>Urho jako biblioteki klas przenośnych

Pakiet Urho mogą być używane z projektu specyficzne dla platformy lub z projektu biblioteki klas przenośnych, co umożliwia ponowne użycie kodu na wszystkich platformach.  Oznacza, że wszystko, co należy wykonać na każdej platformie zapisu z platformy konkretny punkt wejścia, a następnie transfer kontroli do udostępnionego kodu gier.

### <a name="samples"></a>Przykłady

Smak funkcji systemu Urho można uzyskać przez otwarcie w programie Visual Studio for Mac lub Visual Studio przykładowe rozwiązanie z:

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

Domyślne rozwiązanie zawiera projekty dla systemu Android, iOS, Windows i Mac.  Firma Microsoft struktury rozwiązania mamy uruchamianie określonych platform niewielki rozmiar, a przykładowy kod i gier kod znajduje się w bibliotece klas przenośnych pokazujący, jak zmaksymalizować ponowne użycie kodu na wszystkich platformach.

Zapoznaj się [Urho i platforma Your](~/graphics-games/urhosharp/platform/index.md) strony, aby uzyskać więcej informacji na temat tworzenia własnych rozwiązań.

Ponieważ wszystkie próbki korzystają ze wspólnego zestawu elementów interfejsu użytkownika, przykłady ma pobieranej podstawowa konfiguracja, w tym pliku:

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

Udostępnia klasę podstawową próbki obsługuje niektóre podstawowe naciśnięć klawiszy i touch aparatu zdarzeń, konfiguracje, zawiera elementy interfejsu użytkownika podstawowego, a to pozwala każda próbka skupić się na określonych funkcji, która jest są pokazywane.

Poniższy przykład pokazuje, co aparat jest w stanie zrobić:

- [Samply gier](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) proste Sklonowanie ShootySkies.

Podczas gdy inne przykłady Pokaż poszczególnych właściwości każdej próbki.

## <a name="basic-structure"></a>Podstawowa struktura

Podklasy powinny gry [ `Application` ](https://developer.xamarin.com/api/type/Urho.Application/) klasa, jest to, gdzie należy skonfigurować gry (na [ `Setup` ](https://developer.xamarin.com/api/member/Urho.Application.Setup/) — metoda) i uruchomienia gry (w [ `Start` ](https://developer.xamarin.com/api/member/Urho.Application.Start) metody).  Następnie można utworzyć interfejsu użytkownika głównego.  Zamierzamy przeprowadzenie małej przykładowej, pokazujący interfejsów API, aby Instalator scenę 3D, niektóre elementy interfejsu użytkownika i dołączanie do niego proste zachowanie.

```csharp
class MySample : Application {
    protected override void Start ()
    {
        CreateScene ();
        Input.KeyDown += (args) => {
            if (args.Key == Key.Esc) Exit ();
        };
    }

    async void CreateScene()
    {
        // UI text
        var helloText = new Text()
        {
            Value = "Hello World from MySample",
            HorizontalAlignment = HorizontalAlignment.Center,
            VerticalAlignment = VerticalAlignment.Center
        };
        helloText.SetColor(new Color(0f, 1f, 1f));
        helloText.SetFont(
            font: ResourceCache.GetFont("Fonts/Font.ttf"),
            size: 30);
        UI.Root.AddChild(helloText);

        // Create a top-level scene, must add the Octree
        // to visualize any 3D content.
        var scene = new Scene();
        scene.CreateComponent<Octree>();
        // Box
        Node boxNode = scene.CreateChild();
        boxNode.Position = new Vector3(0, 0, 5);
        boxNode.Rotation = new Quaternion(60, 0, 30);
        boxNode.SetScale(0f);
        StaticModel modelObject = boxNode.CreateComponent<StaticModel>();
        modelObject.Model = ResourceCache.GetModel("Models/Box.mdl");
        // Light
        Node lightNode = scene.CreateChild(name: "light");
        lightNode.SetDirection(new Vector3(0.6f, -1.0f, 0.8f));
        lightNode.CreateComponent<Light>();
        // Camera
        Node cameraNode = scene.CreateChild(name: "camera");
        Camera camera = cameraNode.CreateComponent<Camera>();
        // Viewport
        Renderer.SetViewport(0, new Viewport(scene, camera, null));
        // Perform some actions
        await boxNode.RunActionsAsync(
            new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
        await boxNode.RunActionsAsync(
            new RepeatForever(new RotateBy(duration: 1,
                deltaAngleX: 90, deltaAngleY: 0, deltaAngleZ: 0)));
     }
}
```

Po uruchomieniu tej aplikacji, możesz szybko zauważyć, że środowisko wykonawcze jest strona skarżąca o zasobów nie są dostępne.  Co należy zrobić to tworzenie hierarchii w projekcie, który rozpoczyna się od nazwy katalogu specjalnego "Dane", a w tym celu będzie umieścić zasoby, które odwołują się w programie.  Następnie należy skonfigurować we właściwościach elementu dla każdego zasobu "Kopiowania do katalogu wyjściowego" do "Kopiuj Jeśli nowsza", który zagwarantować, że dane jest.

Daj nam opisano, co dzieje się w tym miejscu.

Do uruchamiania aplikacji, należy wywołać funkcję inicjowania aparat następuje Tworzenie nowego wystąpienia klasy aplikacji, takich jak to:

```csharp
new MySample().Run();
```

Środowisko uruchomieniowe wywoła `Setup` i `Start` metody dla Ciebie.  Jeśli można zastąpić `Setup` można skonfigurować parametrów aparat (nie pokazuj tego przykładu).

Konieczne jest przesłonięcie `Start` ponieważ spowoduje to uruchomienie gry.  W tej metodzie zostanie ładowanie zasobów, połączenia z procedury obsługi zdarzeń, skonfigurować sceny i wszystkie akcje, które chcesz uruchomić.  W naszym przykładzie zarówno utworzymy niewielki interfejsu użytkownika do przedstawienia inżynierom użytkownika, jak również konfigurowanie scenę 3D.

Następujący fragment kodu używa framework interfejsu użytkownika do tworzenia tekstowy element i dodaj go do aplikacji:

```csharp
// UI text
var helloText = new Text()
{
    Value = "Hello World from UrhoSharp",
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center
};
helloText.SetColor(new Color(0f, 1f, 1f));
helloText.SetFont(
    font: ResourceCache.GetFont("Fonts/Font.ttf"),
    size: 30);
UI.Root.AddChild(helloText);
```

W ramach interfejsu użytkownika ma na celu dostarczenie interfejsu użytkownika w grze bardzo proste i działa przez dodanie nowych węzłów do [ `UI.Root` ](https://developer.xamarin.com/api/property/Urho.Gui.UI.Root/) węzła.

Druga część nasze przykładowe konfiguracje głównego sceny.  Ten proces obejmuje kilka kroków tworzenia scenę 3D, tworzenie 3D pole na ekranie, dodawanie światło, aparatu i okienko ekranu.  Zostały one opisane bardziej szczegółowo w sekcji [sceny, węzłów, składników i aparaty fotograficzne](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras).

Trzeci część naszej próbki wyzwala kilka akcji.  Akcje są przepisami opisano szczególnych skutków, które utworzonej one mogą być wykonywane przez węzeł na żądanie przez wywołanie metody [ `RunActionAsync` ](https://developer.xamarin.com/api/member/Urho.Node.RunActionsAsync) metoda `Node`.

Pierwszą akcją Skaluje pole z efektem podskakujące i drugi obraca pole nieskończona:

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

Powyższe pokazuje, jak jest pierwszym akcję, która utworzymy [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo/) akcji, to jedynie przepisu, która wskazuje, że skalowania na sekundę na jedną wartość właściwości scale węzła.  Ta akcja z kolei jest otaczający akcję dynamiki [ `EaseBounceOut` ](https://developer.xamarin.com/api/type/Urho.Actions.EaseBounceInOut/) akcji.  Akcje dynamiki zakłóca liniowej wykonywanie akcji i efekt, w tym przypadku zapewnia efekt odbijania poza.
Dlatego naszych przepisu może być zapisany jako:

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

Po utworzeniu przepisu, możemy wykonać przepisu:

```csharp
await boxNode.RunActionsAsync (recipe)
```

Await wskazuje, że chcesz wznowić pracę po tym wierszu, po ukończeniu akcji.  Po ukończeniu akcji wyzwalana drugi animacji.

[Przy użyciu UrhoSharp](~/graphics-games/urhosharp/using.md) dokumentu Eksploruje bardziej szczegółowo pojęcia dotyczące Urho i struktury kodu do tworzenia gier.

## <a name="copyrights"></a>Prawa autorskie

W tej dokumentacji zawiera oryginalną zawartość z Xamarin Inc, ale często rysuje dokumentacji typu open source dla projektu Urho3D i zawiera zrzuty ekranu z projektu Cocos2D.

### <a name="related-links"></a>Linki pokrewne

- [Planety ziemi skoroszytu](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [Eksploracja współrzędne skoroszytu](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
