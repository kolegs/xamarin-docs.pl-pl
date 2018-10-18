---
title: Wprowadzenie do platformy UrhoSharp
description: W tym dokumencie opisano podstawowe struktury aplikacji na platformie UrhoSharp i łącza do różnych przewodniki i przykładowe aplikacje demonstrujące sposób użycia platformy UrhoSharp.
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: a3e14ebca961e828fc578035adaca5ba2a809438
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "34783558"
---
# <a name="an-introduction-to-urhosharp"></a>Wprowadzenie do platformy UrhoSharp

![Logo platformy UrhoSharp](introduction-images/urhosharp-icon.png)

Na platformie UrhoSharp to zaawansowany aparat gier 3D dla deweloperów platformy Xamarin i .NET.  Jest podobna do struktury SceneKit firmy Apple i struktury SpriteKit duchu i obejmują fizyki, nawigacji, sieci i wiele więcej podczas gdy nadal trwa wiele platform.

To powiązanie .NET [Urho3D](http://urho3d.github.io/) aparatu i umożliwia programistom pisanie kodu dla wielu platform, który można wskazać systemów Android, iOS, Windows i Mac o takiej samej bazy kodu i renderować systemów zarówno ze specyfikacji OpenGL, jak i Direct3D.

Na platformie UrhoSharp jest aparat do tworzenia gier za pomocą wiele gotowych funkcji:

- Zaawansowane renderowania grafiki 3D
- [Fizyka symulacji](https://developer.xamarin.com/api/namespace/Urho.Physics/) (przy użyciu biblioteki punktorów)
- [Obsługa sceny](https://developer.xamarin.com/api/type/Urho.Scene/)
- Obsługa await/Async
- [Przyjazne operacje interfejsu API](https://developer.xamarin.com/api/namespace/Urho.Actions/)
- [2D integracji scen 3D](https://developer.xamarin.com/api/namespace/Urho.Urho2D/)
- [Renderowanie czcionek, za pomocą FreeType](https://developer.xamarin.com/api/type/Urho.Gui.FontFaceFreeType/)
- [Klient i serwer możliwości sieciowe](https://developer.xamarin.com/api/namespace/Urho.Network/)
- [Importowanie wielu zasobów](https://developer.xamarin.com/api/namespace/Urho.Resources/) (przy użyciu otwartych zasoby biblioteki)
- [Siatka nawigacji i pathfinding](https://developer.xamarin.com/api/namespace/Urho.Navigation/) (przy użyciu zmienione/przekierowania)
- [Generowanie wypukłe kadłuba wykrywanie kolizji](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) (przy użyciu StanHull)
- [Odtwarzanie](https://developer.xamarin.com/api/namespace/Urho.Audio/) (przy użyciu **libvorbis**)

## <a name="getting-started"></a>Wprowadzenie

Na platformie UrhoSharp wygodnie jest rozpowszechniany jako [pakietu NuGet](https://www.nuget.org/) i jego mogą być dodawane do projektów C# lub F # przeznaczonych Windows, Mac, Android lub iOS.  NuGet jest powiązana z zarówno identyfikacji bibliotek wymaganych do uruchomienia programu, a także zasoby podstawowe (CoreData) używanego przez aparat.

### <a name="urho-as-a-portable-class-library"></a>Urho jako biblioteki klas przenośnych

Pakiet Urho mogą być używane z projektu specyficznego dla platformy, albo z projektu Portable Class Library, umożliwiając ponowne używanie kodu na wszystkich platformach.  Oznacza to, że wszystko, co trzeba zrobić na każdej platformie jest zapisu punktem wejścia określonym platformy, a następnie przekazać sterowanie do udostępnionego kodu gier.

### <a name="samples"></a>Przykłady

Smak dla funkcji Urho można uzyskać przez otwarcie w programie Visual Studio for Mac lub Visual Studio przykładowe rozwiązanie z:

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

Domyślnego rozwiązania zawiera projekty dla systemów Android, iOS, Windows i Mac.  Firma Microsoft ma strukturę tego rozwiązania, aby mamy uruchamianie programu określonych platform niewielki rozmiar, a wszystkie przykładowy kod i kod gry znajduje się w bibliotece klas przenośnych pokazujący, jak i zmaksymalizować ponowne wykorzystanie kodu na wszystkich platformach.

Zapoznaj się z [Urho i usługi Platform](~/graphics-games/urhosharp/platform/index.md) strony, aby uzyskać więcej informacji na temat tworzenia własnych rozwiązań.

Ponieważ wszystkie przykłady korzystają ze wspólnego zestawu, elementów interfejsu użytkownika, przykłady zostały wyodrębnione podstawowa konfiguracja, w tym pliku:

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

Udostępnia klasę bazową przykładowy, który obsługuje niektóre podstawowe naciśnięcia klawiszy i zdarzenia, konfiguracje dotykowe aparatu, zawiera elementy interfejsu użytkownika podstawowego, a to pozwala każdy przykład skoncentrować się na określonych funkcji, która jest reprezentowanie.

Poniższy przykład pokazuje, w jaki aparat jest w stanie to:

- [Samply gier](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) proste klonu ShootySkies.

Podczas gdy inne przykłady pokazują poszczególne właściwości każdej próbki.

## <a name="basic-structure"></a>Podstawowa struktura

Twoja Gra powinna podklasy [`Application`](https://developer.xamarin.com/api/type/Urho.Application/)
Klasa, jest to, gdzie możesz skonfigurować swoją grę (na [ `Setup` ](https://developer.xamarin.com/api/member/Urho.Application.Setup/) metoda) i Rozpocznij tworzenie gry w (w [ `Start` ](https://developer.xamarin.com/api/member/Urho.Application.Start) metody).  Następnie możesz utworzyć interfejs użytkownika głównego.  Zamierzamy przeprowadzenie niewielką próbkę, pokazujący interfejsów API można skonfigurować w scenie 3D, niektóre elementy interfejsu użytkownika i dołączanie proste zachowanie do niego.

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

Po uruchomieniu tej aplikacji, szybko wykryjesz, środowisko uruchomieniowe jest skarżących dotyczące zasobów nie istnieją.  Co należy zrobić to tworzenie hierarchii w projekcie, który rozpoczyna się od nazwy katalogu specjalnego "Dane", a w tym celu możesz umieścić zasoby, które odwołują się w programie.  Następnie należy ustawić we właściwościach elementu dla każdego zasobu "Kopiuj do katalogu wyjściowego" do "Kopiuj Jeśli nowszy", który zapewni Twoje dane są dostępne.

Daj nam wyjaśnić, co się dzieje w tym miejscu.

Aby uruchomić aplikację, należy wywołać funkcji inicjowania aparatu, a następnie utworzyć nowe wystąpienie klasy aplikacji, takich jak to:

```csharp
new MySample().Run();
```

Środowisko uruchomieniowe wywoła `Setup` i `Start` metody dla Ciebie.  Jeśli zastąpisz `Setup` można skonfigurować parametry silnika (nie show w tym przykładzie).

Konieczne jest przesłonięcie `Start` ponieważ spowoduje to uruchomienie swoją grę.  W przypadku tej metody będzie załadować zasoby, Połącz programy obsługi zdarzeń, konfigurowanie sceny i wszystkie akcje, które chcesz uruchomić.  W naszym przykładzie oba utworzymy trochę interfejsu użytkownika do przedstawienia użytkownikowi, a także konfigurowanie sceny 3D.

Poniższy fragment kodu używa struktury interfejsu użytkownika, aby utworzyć element tekstowy i dodać go do aplikacji:

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

Struktury interfejsu użytkownika ma na celu dostarczenie bardzo prosty w grze interfejs użytkownika i działa przez dodanie nowych węzłów do [ `UI.Root` ](https://developer.xamarin.com/api/property/Urho.Gui.UI.Root/) węzła.

Druga część nasze przykładowe konfiguracje głównego sceny.  Ten proces obejmuje kilka kroków, tworzenie sceny 3D, tworzenie 3D pola na ekranie, dodawanie światło, aparatu i okienko ekranu.  Te zostały opisane bardziej szczegółowo w sekcji [sceny, węzły, składniki i aparaty fotograficzne](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras).

Trzecia część naszego przykładu wyzwala kilka akcji.  Akcje są przepisy, które opisania szczególnych skutków, a po ich utworzeniu mogą być wykonywane przez węzeł na żądanie, wywołując [ `RunActionAsync` ](https://developer.xamarin.com/api/member/Urho.Node.RunActionsAsync) metody `Node`.

Pierwszą akcją skaluje efekt odbijania w tym polu, a drugi zawsze obraca się pole:

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

Powyższe pokazuje, jak pierwszej akcji, który utworzymy [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo/) akcji, jest to jedynie przepisu, który wskazuje, że skalowania na sekundę na jedną wartość właściwości skalowania węzła.  Ta akcja z kolei jest otaczający akcję przyspieszania [ `EaseBounceOut` ](https://developer.xamarin.com/api/type/Urho.Actions.EaseBounceInOut/) akcji.  Działania sterowania tempem zmian zniekształcają liniowej wykonywania akcji Zastosuj efekt, w tym przypadku daje efekt odbijania w poziomie.
Dzięki naszym przepisu może być zapisany jako:

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

Po utworzeniu przepisu wykonywania przepisu:

```csharp
await boxNode.RunActionsAsync (recipe)
```

Await wskazuje, że chcesz wznowić wykonywanie po tym wierszu, po ukończeniu akcji.  Po ukończeniu akcji wyzwalamy drugiej animacji.

[Przy użyciu platformy UrhoSharp](~/graphics-games/urhosharp/using.md) dokumentu Eksploruje omówiona bardziej szczegółowo pojęcia Urho i jak tworzyć strukturę kodu do tworzenia gier.

## <a name="copyrights"></a>Prawa autorskie

Niniejsza dokumentacja zawiera oryginalną zawartość z Xamarin Inc, ale rysuje często z dokumentacji "open source" dla projektu Urho3D i zawiera zrzuty ekranu z projektu Cocos2D.

