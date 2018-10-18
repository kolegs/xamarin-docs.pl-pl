---
title: Korzystanie z aparatu UrhoSharp do tworzenia gier 3D
description: Ten dokument zawiera omówienie platformy UrhoSharp, opisujący scen, składników, kształty, aparaty fotograficzne, akcji, danych wejściowych użytkownika, dźwięk i więcej.
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 7d07733ebf62e6e12ccee05f9b72eaf1a74afad2
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "34784042"
---
# <a name="using-urhosharp-to-build-a-3d-game"></a>Korzystanie z aparatu UrhoSharp do tworzenia gier 3D

Przed przystąpieniem do napisania swoją pierwszą grę, chcesz zapoznaj się z podstawy funkcjami: jak skonfigurować sceny, jak można załadować zasobów (zawiera kompozycji) i jak utworzyć proste interakcje gry.

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>Scen, węzły, składniki i aparaty fotograficzne

Model sceny można określić jako wykres oparty na komponentach sceny. Sceny składa się z hierarchii węzły sceny, zaczynając od węzła głównego, który reprezentuje również całego sceny. Każdy [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/) ma przekształcenia 3D (pozycja, obrotu i skalowania), nazwę, identyfikator oraz dowolną liczbę składników.  Składniki Ożyw węzła, mogą robić więjsze Dodaj wizualnej reprezentacji ([`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)), mogły one wysyłać dźwięk ([`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource)), zapewniają granic kolizji i tak dalej.

Możesz utworzyć swoje scen i węzły instalacji za pomocą [edytora Urho](#UrhoEditor), lub można wykonywać takie czynności, w kodzie języka C#.  W tym dokumencie przeanalizujemy różne rzeczy przy użyciu kodu, ponieważ one ilustrują elementy niezbędne do doprowadzenia elementów do wyświetlenia na ekranie

Oprócz konfigurowania sceny, musisz instalacji [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/), jest to, co określa, jakie Pobierz wyświetlane użytkownikowi.

### <a name="setting-up-your-scene"></a>Konfigurowanie sceny

Przeważnie tworzy się ten formularz, metoda rozpoczęcia:

```csharp
var scene = new Scene ();
// Create the Octree component to the scene. This is required before
// adding any drawable components, or else nothing will show up. The
// default octree volume will be from -1000, -1000, -1000) to
//(1000, 1000, 1000) in world coordinates; it is also legal to place
// objects outside the volume but their visibility can then not be
// checked in a hierarchically optimizing manner
scene.CreateComponent<Octree> ();
// Create a child scene node (at world origin) and a StaticModel
// component into it. Set the StaticModel to show a simple plane mesh
// with a "stone" material. Note that naming the scene nodes is
// optional. Scale the scene node larger (100 x 100 world units)
var planeNode = scene.CreateChild("Plane");
planeNode.Scale = new Vector3 (100, 1, 100);
var planeObject = planeNode.CreateComponent<StaticModel> ();
planeObject.Model = ResourceCache.GetModel ("Models/Plane.mdl");
planeObject.SetMaterial(ResourceCache.GetMaterial("Materials/StoneTiled.xml"));
```

### <a name="components"></a>Składniki

Renderowanie, obiekty 3D, odtwarzanie dźwięku, fizyki oraz aktualizacje logiki inicjowanych przez skrypty są włączone, tworząc różne składniki do węzłów, wywołując [ `CreateComponent<T>()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateComponent%3CT%3E/p/Urho.CreateMode/System.UInt32/).  Na przykład instalacji usługi węzła i światła składnika następująco:

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

Utworzyliśmy powyżej węzła o nazwie "`DirectionalLight`" i ustaw kierunek go, ale nic innego.  Teraz można przejść powyżej węzła do węzła świetlnych, dołączając [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) składnika, za pomocą `CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

Składniki rozwiązań utworzone w `Scene` może mieć specjalną rolę: do implementacji funkcji całej sceny. Powinny zostać utworzone przed wszystkimi innymi składnikami i obejmują następujące czynności:

* [`Octree`](https://developer.xamarin.com/api/type/Urho.Octree/): implementuje przestrzenne partycjonowania i przyspieszonej widoczność zapytania. Bez tego 3D obiektów nie może być renderowany.
* [`PhysicsWorld`](https://developer.xamarin.com/api/type/Urho.Physics.PhysicsWorld/): implementuje fizyki symulacji. Fizyka składniki, takie jak [ `RigidBody` ](https://developer.xamarin.com/api/type/Urho.Physics.RigidBody/) lub [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) może nie działać poprawnie bez niej.
* [`DebugRenderer`](https://developer.xamarin.com/api/type/Urho.DebugRenderer/): implementuje debugowania geometrii renderowania.

Zwykłe składniki, takie jak [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light), [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera) lub [`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)
Nie można utworzyć, bezpośrednio do [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene), ale raczej do węzłów podrzędnych.

Biblioteka jest dostarczany z różnych składników, które można dołączyć do węzłów wprowadzić ich w życie: elementy widoczne dla użytkownika (modeli), dźwięki, sztywne treści, kształty kolizji, aparaty fotograficzne, źródła światła, emitery cząstki i wiele innych.

### <a name="shapes"></a>Kształty

Dla wygody różne kształty są dostępne jako prosty węzłów w przestrzeni nazw Urho.Shapes.  Należą do pola, obszary, szyszki, liczba cylindrów i płaszczyzny.

### <a name="camera-and-viewport"></a>Kamera i okienka ekranu

Podobnie jak światło, kamery są składnikami, więc należy dołączyć ten składnik do węzła, odbywa się podobnie do następującego:

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

Dzięki temu utworzono aparatu i umieszczono aparatu świat 3D, następnym krokiem jest poinformowanie `Application` czy jest to aparatu, z którego chcesz użyć, odbywa się z następującym kodem:

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

A teraz powinno być możliwe zobaczyć wyniki tworzenia usługi.

### <a name="identification-and-scene-hierarchy"></a>Identyfikacja i sceny hierarchii

W przeciwieństwie do węzłów składniki nie mają nazwy. składniki wewnątrz tego samego węzła tylko są identyfikowane przez ich typ i indeksu na liście składników węzła, który jest wypełniony w kolejności tworzenia, na przykład, możesz pobrać [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light) składnik poza `lightNode` obiektu powyżej następująco:

```csharp
var myLight = lightNode.GetComponent<Light>();
```

Można również uzyskać listę wszystkich składników przez pobranie [ `Components` ](https://developer.xamarin.com/api/property/Urho.Node.Components/) właściwość, która zwraca `IList<Component>` , można użyć.

Podczas tworzenia, zarówno węzły, jak i składniki pobrać identyfikatorów sceny globalnej liczby całkowitej. Może być odpytywany z sceny za pomocą funkcji [ `GetNode(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetNode/p/System.UInt32/) i [ `GetComponent(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetComponent/p/System.UInt32/). Jest to znacznie szybciej niż na przykład podczas rekursywnego sceny na podstawie nazwy węzła zapytania.

Nie obowiązuje koncepcja wbudowanej jednostki lub obiektem gier jest on raczej do programisty należy określić hierarchię węzłów i w węzły, które można umieścić wszelka logika inicjowanych przez skrypty. Zazwyczaj bezpłatne przenoszenie obiektów w świat 3D zostałyby utworzone jako elementy podrzędne węzła głównego. Może zostać utworzone węzły z lub bez użycia nazwy [ `CreateChild()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateChild/p/System.String/Urho.CreateMode/System.UInt32/). Unikatowość nazwy węzłów nie jest wymuszana.

Zawsze, gdy niektóre hierarchiczne kompozycji, jest zalecane (i w rzeczywistości to konieczne, ponieważ składniki nie mają własne przekształcenia 3D) do utworzenia węzła podrzędnego.

Na przykład jeśli znak był zawierający obiekt w jego posiadaniu, obiekt powinien mieć własny węzła, który może być elementem nadrzędnym kości ręcznie znak (również [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/)).  Wyjątek stanowi fizyki [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape), który może być offsetted i obrócony oddzielnie w odniesieniu do węzła.

Należy pamiętać, że [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Node/)firmy właścicielami przekształcenie celowo jest ignorowana optymalizacji, podczas obliczania transformacji świata pochodzące z węzłów podrzędnych, więc zmiana go nie ma wpływu i należy pozostawić, ponieważ jest (położenie pochodzenia, nie obrotu bez skalowania.)

[`Scene`](https://developer.xamarin.com/api/type/Urho.Node/) węzły można swobodnie pokrewnym. W odróżnieniu od nich składniki zawsze należy do węzła, dołączony do i nie można przenosić między węzłami. Zarówno węzły, jak i składniki zapewniają [ `Remove()` ](https://developer.xamarin.com/api/member/Urho.Node.Remove()/) funkcję, aby osiągnąć ten cel bez konieczności przechodzenia przez nadrzędne. Po usunięciu węzła żadnych operacji na węźle lub składnika w danym są bezpieczne po wywołaniu tej funkcji.

Istnieje również możliwość utworzenia `Node` nienależącym do sceny. Jest to przydatne na przykład za pomocą aparatu przenoszenia w scenie, który może być załadowany lub zapisane, ponieważ aparat fotograficzny nie zostaną zapisane razem z rzeczywistych sceny a następnie nie zostaną usunięte po załadowaniu sceny. Należy jednak zauważyć, że tworzenie geometrii, fizyki lub skrypt składników niedołączone węzłem, a następnie przenoszenia go do sceny później spowoduje, że te składniki nie będą działać poprawnie.

### <a name="scene-updates"></a>Aktualizacje sceny

Sceny, w których aktualizacje są włączone (ustawienie domyślne) zostaną automatycznie zaktualizowane w każdej iteracji pętli głównej.  Aplikacja [ `SceneUpdate` ](https://developer.xamarin.com/api/event/Urho.Scene.SceneUpdate/) programu obsługi zdarzeń jest wywoływany w nim.

Węzły i składniki mogą być wykluczone z aktualizacji sceny, wyłączając je, zobacz [ `Enabled` ](https://developer.xamarin.com/api/member/Urho.Node.Enabled).  Zachowanie zależy od określonego składnika, ale np. wyłączenie składnika drawable ułatwia też niewidoczne, natomiast wyłączenie składnika źródła dźwięku wycisza go. Jeśli węzeł jest wyłączony, wszystkie jego elementy są traktowane jako wyłączone, niezależnie od tego stanu włączenia/wyłączenia.

## <a name="adding-behavior-to-your-components"></a>Dodawanie zachowania do składników

Najlepszym sposobem struktury Twoja gra jest zapewnienie własnego składnika, który hermetyzacji aktora lub elementu nad swoją grą.  To sprawia, że funkcja niezależne od zasoby używane do wyświetlania, jego zachowanie.

Najprostszym sposobem dodawania zachowanie do składnika jest za pomocą akcji, które znajdują się instrukcje, aby w kolejce i scalanie jej z programowania asynchronicznego w języku C#.  Dzięki temu zachowanie składnika się bardzo wysokim poziomie, a ułatwia zrozumienie, co się dzieje.

Alternatywnie można kontrolować, dokładnie co się stanie do składnika, aktualizując właściwości składnika na każdej ramce (omówionych w sekcji opartych na klatkach zachowanie).

### <a name="actions"></a>Akcje

Możesz dodać zachowania do węzłów bardzo łatwe przy użyciu akcji.  Działania może zmieniać różne właściwości węzła i ich wykonania w okresie czasu lub powtarzanie ich wiele razy z krzywej danego animacji.

Rozważmy na przykład "chmura" węzła w sceny, Oddalanie w następujący sposób:

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

Akcje są niezmienne obiektów, które umożliwia ponowne używanie akcji do obsługi różnych obiektów.

Typowe idiomu jest utworzyć akcję, która wykonuje operację wstecznego:

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

Poniższy przykład spowoduje zanikanie obiekt dla Ciebie w okresie trzy sekundy.  Można również uruchomić jedno działanie po drugim, na przykład, możesz najpierw przenieść chmury i je ukryć:

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

Obie akcje wykonane w tym samym czasie, należy można użyć działania równoległe i podaj wszystkie akcje, które chcesz przeprowadzić równolegle:

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

W powyższym przykładzie chmury przeniesie i zanikanie w tym samym czasie.

Można zauważyć, że te używają języka C# await, co pozwala na liniowo myśleć o zachowaniu, aby uzyskać.

### <a name="basic-actions"></a>Akcje podstawowe

Poniżej przedstawiono akcje obsługiwane w platformie UrhoSharp:

* Przenoszenie węzłów: [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo), [ `MoveBy` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveBy), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `BezierTo` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierTo), [ `BezierBy` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierBy) , [`JumpTo`](https://developer.xamarin.com/api/type/Urho.Actions.JumpTo), [`JumpBy`](https://developer.xamarin.com/api/type/Urho.Actions.JumpBy)
* Obracanie węzłów: [ `RotateTo` ](https://developer.xamarin.com/api/type/Urho.Actions.RotateTo), [`RotateBy`](https://developer.xamarin.com/api/type/Urho.Actions.RotateBy)
* Skalowanie węzłów: [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo), [`ScaleBy`](https://developer.xamarin.com/api/type/Urho.Actions.ScaleBy)
* Zanikanie węzłów: [ `FadeIn` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeIn), [ `FadeTo` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeTo), [ `FadeOut` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeOut), [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [`Blink`](https://developer.xamarin.com/api/type/Urho.Actions.Blink)
* Barwienia: [ `TintTo` ](https://developer.xamarin.com/api/type/Urho.Actions.TintTo), [`TintBy`](https://developer.xamarin.com/api/type/Urho.Actions.TintBy)
* W czasie: [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [ `Show` ](https://developer.xamarin.com/api/type/Urho.Actions.Show), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `RemoveSelf` ](https://developer.xamarin.com/api/type/Urho.Actions.RemoveSelf), [`ToggleVisibility`](https://developer.xamarin.com/api/type/Urho.Actions.ToggleVisibility)
* Pętle: [ `Repeat` ](https://developer.xamarin.com/api/type/Urho.Actions.Repeat), [ `RepeatForever` ](https://developer.xamarin.com/api/type/Urho.Actions.RepeatForever), [`ReverseTime`](https://developer.xamarin.com/api/type/Urho.Actions.ReverseTime)

Inne zaawansowane funkcje obejmują kombinacja [ `Spawn` ](https://developer.xamarin.com/api/type/Urho.Actions.Spawn) i [ `Sequence` ](https://developer.xamarin.com/api/type/Urho.Actions.Sequence) akcji.

### <a name="easing---controlling-the-speed-of-your-actions"></a>Ułatwianie - kontrolowanie szybkości swoje działania

Ułatwianie jest sposób, który określa sposób, w jaki będą rozwijane animacji, i może sprawić, że animacji o wiele bardziej przyjemny.  Domyślnie czynności użytkownika będą mieć liniowej zachowania, na przykład [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo) akcji miałby bardzo robotów przepływu.  Można opakować swoje działania do dynamiki akcję, aby zmienić to zachowanie, na przykład taki, który będzie wolno uruchamiania przepływu, Przyspiesz i powoli zakończą się ([`EasyInOut`](https://developer.xamarin.com/api/type/Urho.Actions.EasyInOut)).

Można to zrobić, zawijanie istniejącej akcji do sterowania tempem zmian akcji, na przykład:

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

Istnieje wiele trybów sterowania tempem zmian, w poniższej tabeli przedstawiono różne typy sterowania tempem zmian i ich działania na wartość obiektu, który jest kontrolowany przez okres czasu, od początku do końca:

![Ułatwianie tryby](using-images/easing.png "ten wykres przedstawia różne typy sterowania tempem zmian i ich działania na wartość obiektu kontrolować w okresie czasu")

### <a name="using-actions-and-async-code"></a>Za pomocą akcji i kod asynchroniczny

W swojej [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component/) podklasy, należy wprowadzić metody asynchronicznej, który przygotowuje swoje zachowanie składników i dyski funkcji dla niego.
A następnie powodowałoby wywołanie tej metody, przy użyciu języka C# `await` — słowo kluczowe z innej części programu, albo usługi `Application.Start` metody lub w odpowiedzi na użytkownika lub wątek punktu w aplikacji.

Na przykład:

```csharp
class Robot : Component {
    public bool IsAlive;
    async void Launch ()
    {
        // Dress up our robot
        var cache = Application.ResourceCache;
        var model = node.CreateComponent<StaticModel>();
        model.Model = cache.GetModel ("robot.mdl"));
        model.SetMaterial (cache.GetMaterial ("robot.xml"));
        Node.SetScale (1);

        // Bring the robot into our scene
        await Node.RunActionsAsync(
            new MoveBy(duration: 0.6f, position: new Vector3(0, -2, 0)));
        // Make him move around to avoid the user fire
        MoveRandomly(minX: 1, maxX: 2, minY: -3, maxY: 3, duration: 1.5f);
        // And simultaneously have him shoot at the user
        StartShooting();
    }

    protected async void MoveRandomly (float minX, float maxX,
                                       float minY, float maxY,
                       float duration)
    {
        while (IsAlive){
            var moveAction = new MoveBy(duration,
                new Vector3(RandomHelper.NextRandom(minX, maxX),
                            RandomHelper.NextRandom(minY, maxY), 0));
            await Node.RunActionsAsync(moveAction, moveAction.Reverse());
        }
    }
    protected async void StartShooting()
    {
        while (IsAlive && Node.Components.Count > 0){
            foreach (var weapon in Node.Components.OfType<Weapon>()){
                await weapon.FireAsync(false);
                if (!IsAlive)
                    return;
            }
            await Node.RunActionsAsync(new DelayTime(0.1f));
        }
    }
}
```

W `Launch` metoda ponad trzy akcje są uruchamiane: robota trafia do sceny, ta akcja powodują zmianę lokalizacji węzła w okresie 0,6 sekund.  Ponieważ jest to opcja async, będzie to miało miejsce jednocześnie jako następnej instrukcji, która jest wywołanie do `MoveRandomly`.  Ta metoda będzie zmienić położenie robota równolegle z losowo wybranej lokalizacji.  Jest to osiągane przez wykonywanie dwa złożone akcji, ruch do nowej lokalizacji, a po powrocie do oryginalnego Umieść i powtórz tę czynność, tak długo, jak robota pozostaje aktywne.  I można było ciekawiej, robota będzie przechowywać rozwiązywania problemów jednocześnie.  Rozwiązywania problemów uruchamia się co 0,1 sekundy.

### <a name="frame-based-behavior-programming"></a>Zachowanie opartych na klatkach programowania

Jeśli chcesz kontrolować zachowanie składnika na zasadzie klatka po klatce, zamiast przy użyciu akcji, co możesz zrobić jest zastąpienie [ `OnUpdate` ](https://developer.xamarin.com/api/member/Urho.Component.OnUpdate) metody usługi [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component) podklasę.  Ta metoda jest wywoływana jeden raz na klatkę i jest wywoływana tylko wtedy, gdy właściwość ReceiveSceneUpdates ustawiona na wartość true.

Poniżej pokazano sposób tworzenia `Rotator` składnika, który następnie jest dołączony do węzła, co powoduje, że węzeł, aby obrócić:

```csharp
class Rotator : Component {
    public Rotator()
    {
        ReceiveSceneUpdates = true;
    }
    public Vector3 RotationSpeed { get; set; }
    protected override void OnUpdate(float timeStep)
    {
        Node.Rotate(new Quaternion(
            RotationSpeed.X * timeStep,
            RotationSpeed.Y * timeStep,
            RotationSpeed.Z * timeStep),
            TransformSpace.Local);
    }
}
```

I to, jak ten składnik będzie dołączanie do węzła:

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>Łącząc style

Można użyć danego modelu async/akcji na podstawie ilości zachowanie, która doskonale nadaje się ognia i zapominać stylu programowania programowania, ale można również dostosować zachowanie danego składnika można również uruchomić kodu aktualizacji na każdej ramce.

Na przykład w ramach pokazu SamplyGame jest on używany w `Enemy` klasy koduje akcje używa podstawowe zachowanie, ale również zapewnia punkt składniki kierunku użytkownika przez ustawienie kierunku węzła o `Node.LookAt`:

```csharp
    protected override void OnUpdate(SceneUpdateEventArgs args)
    {
        Node.LookAt(
            new Vector3(0, -3, 0),
            new Vector3(0, 1, -1),
            TransformSpace.World);
        base.OnUpdate(args);
    }
```

## <a name="loading-and-saving-scenes"></a>Ładowanie i zapisywanie sceny

Sceny może być ładowane i zapisywane w formacie XML. Zapoznaj się ze funkcjami [ `LoadXml` ](https://developer.xamarin.com/api/member/Urho.Scene.LoadXml) i [ `SaveXML()` ](https://developer.xamarin.com/api/member/Urho.Scene.SaveXml). Po załadowaniu scenę, najpierw usunąć całą istniejącą zawartość w niej (węzły podrzędne i składniki). Węzły i składniki, które są oznaczone jako tymczasowy z `Temporary` właściwości nie zostaną zapisane. Serializator wszystkie wbudowane składniki i właściwości, ale nie jest to inteligentnych do obsługi właściwości niestandardowych i pól zdefiniowanych w swojej podklasy składnika. Jednak udostępnia dwie metody wirtualne w tym:

* [`OnSerialize`](https://developer.xamarin.com/api/member/Urho.Component.OnSerialize) gdzie można zarejestrować możesz niestandardowe stany dla serializacji

* [`OnDeserialized`](https://developer.xamarin.com/api/member/Urho.Component.OnDeserialize) gdzie można uzyskać z zapisanych stanów niestandardowych.

Zwykle niestandardowych składników będzie wyglądać następująco:

```csharp
class MyComponent : Component {
    // Constructor needed for deserialization
    public MyComponent(IntPtr handle) : base(handle) { }
    public MyComponent() { }
    // user defined properties (managed state):
    public Quaternion MyRotation { get; set; }
    public string MyName { get; set; }

    public override void OnSerialize(IComponentSerializer ser)
    {
        // register our properties with their names as keys using nameof()
        ser.Serialize(nameof(MyRotation), MyRotation);
        ser.Serialize(nameof(MyName), MyName);
    }

    public override void OnDeserialize(IComponentDeserializer des)
    {
        MyRotation = des.Deserialize<Quaternion>(nameof(MyRotation));
        MyName = des.Deserialize<string>(nameof(MyName));
    }
    // called when the component is attached to some node
    public override void OnAttachedToNode()
    {
        var node = this.Node;
    }
}
```

### <a name="object-prefabs"></a>Obiekt Prefabs

Po prostu ładowania lub zapisywania całego sceny nie jest wystarczająco elastyczny, dla gier gdzie nowych obiektów muszą być tworzone dynamicznie. Z drugiej strony tworzenie złożonych obiektów i ustawiania ich właściwości w kodzie będzie również niewygodna. Z tego powodu jest również możliwość zapisania obejmujące jego węzły podrzędne, składniki i atrybutów węzła sceny. Później wygodnie te może być załadowany jako grupa.  Zapisany obiekt jest często nazywany prefab. Istnieją trzy sposoby, w tym celu:

- W kodzie, przez wywołanie metody [ `Node.SaveXml` ](https://developer.xamarin.com/api/member/Urho.Node.SaveXml) w węźle
- W edytorze, wybierając węzeł w oknie hierarchii i wybierając pozycję "Zapisz węzła jako" z menu "File".
- Za pomocą polecenia "węzeł" w `AssetImporter`, która zapisze hierarchię węzła sceny i żadnych modeli zawarte w danych wejściowych zasobu (np.) Plik Collada)

Do utworzenia wystąpienia zapisane węzła do sceny, należy wywołać [ `InstantiateXml()` ](https://developer.xamarin.com/api/member/Urho.Scene.InstantiateXml). Węzeł zostanie utworzony jako element podrzędny sceny, ale mogą być swobodnie pokrewnym po tym. Położenie i obrót związanych z umieszczeniem w węźle muszą być określone. Poniższy kod ilustruje sposób tworzenia wystąpienia prefab `Ninja.xm` do sceny z wybranym miejscu i wymiany:

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>Zdarzenia

UrhoObjects wywołuje szereg zdarzeń, te są udostępniane jako zdarzenia języka C# różnymi klasami, które generują je.  Oprócz języka C# — model na podstawie zdarzeń, istnieje również możliwość użycia `SubscribeToXXX` metody, które umożliwia subskrybowanie i zachować token subskrypcji, które później służy do anulowania subskrypcji.  Różnica polega na poprzednie umożliwi wiele wywołań do subskrybowania, drugi tylko umożliwia jeden, ale umożliwia wrażeń lambda stylu podejście ma być używany, a jeszcze, umożliwiają łatwe usuwanie subskrypcji.  Są one wzajemnie się wykluczają.

Gdy zasubskrybujesz zdarzenia, należy podać metodę, która przyjmuje argument, z argumentami odpowiedniego zdarzenia.

Na przykład jest to, jak subskrybować zdarzenie naciśnięcia przycisku myszy:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += HandleMouseButtonDown;
}

void HandleMouseButtonDown(MouseButtonDownEventArgs args)
{
    Console.WriteLine ("button pressed");
}
```

Styl lambda:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

Czasami można zrezygnować z otrzymywania powiadomień dla zdarzeń, w przypadkach, Zapisz wartość zwracaną z wywołania `SubscribeTo` metodę i wywoływać metodę anulowania subskrypcji na nim:

```csharp
Subscription mouseSub;

public void override Start ()
{
    mouseSub = UI.SubscribeToMouseButtonDown (args => {
    Console.WriteLine ("button pressed");
      mouseSub.Unsubscribe ();
    };
}
```

Parametr odebranych przez program obsługi zdarzeń jest zdarzenie silnie typizowanej klasy argumenty, będą specyficzne dla każdego zdarzenia, który zawiera ładunek zdarzenia.

## <a name="responding-to-user-input"></a>Odpowiadanie na dane wejściowe użytkownika

Możesz zasubskrybować różnych zdarzeń, takich jak naciśnięć klawiszy w dół subskrybowanie zdarzenia i reagować na dane wejściowe są dostarczane:

```csharp
Start ()
{
    UI.KeyDown += HandleKeyDown;
}

void HandleKeyDown (KeyDownEventArgs arg)
{
     switch (arg.Key){
     case Key.Esc: Engine.Exit (); return;
}
```

Jednak w wielu scenariuszach inne programy obsługi aktualizacji sceny do zapoznania się na bieżący stan kluczy, gdy są aktualizowane i odpowiednio zaktualizować swój kod.  Na przykład następujące może służyć do zaktualizować lokalizację aparat oparty na klawiaturze, wprowadź:

```csharp
protected override void OnUpdate(float timeStep)
{
    Input input = Input;
    // Movement speed as world units per second
    const float moveSpeed = 4.0f;
    // Read WASD keys and move the camera scene node to the
    // corresponding direction if they are pressed
    if (input.GetKeyDown(Key.W))
        CameraNode.Translate(Vector3.UnitY * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.S))
        CameraNode.Translate(new Vector3(0.0f, -1.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.A))
        CameraNode.Translate(new Vector3(-1.0f, 0.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.D))
        CameraNode.Translate(Vector3.UnitX * moveSpeed * timeStep, TransformSpace.Local);
}
```

## <a name="resources-assets"></a>Zasoby (zasoby)

Zasoby obejmują większość elementów na platformie UrhoSharp, które są ładowane z pamięci masowej podczas inicjowania lub środowiska uruchomieniowego:

- [`Animation`](https://developer.xamarin.com/api/type/Urho.Animation/) -używana animacji szkieletowych
- [`Image`](https://developer.xamarin.com/api/type/Urho.Resources.Image) -reprezentuje obrazów przechowywanych w różnych formatach graficznych
- [`Model`](https://developer.xamarin.com/api/type/Urho.Model/) -Modele 3D
- [`Material`](https://developer.xamarin.com/api/type/Urho.Material) -materiałów używany do renderowania modeli.
- [`ParticleEffect`](https://developer.xamarin.com/api/type/Urho.ParticleEffect)- [w tym artykule opisano](http://urho3d.github.io/documentation/1.4/_particles.html) działania nadajnika cząstek, zobacz "[cząstki](#particles)" poniżej.
- [`Shader`](https://developer.xamarin.com/api/type/Urho.Shader) -niestandardowych programów do cieniowania
- [`Sound`](https://developer.xamarin.com/api/type/Urho.Audio.Sound) -dźwięków do odtwarzania, zobacz "[dźwięk](#sound)" poniżej.
- [`Technique`](https://developer.xamarin.com/api/type/Urho.Technique/) -technik renderowanie materiału
- [`Texture2D`](https://developer.xamarin.com/api/type/Urho.Urho2D.Texture2D/) -Tekstury 2W
- [`Texture3D`](https://developer.xamarin.com/api/type/Urho.Texture3D/) -Tekstury 3D
- [`TextureCube`](https://developer.xamarin.com/api/type/Urho.TextureCube/) -Tekstura modułu
- `XmlFile`

Są one zarządzane i ładowany przez [ `ResourceCache` ](https://developer.xamarin.com/api/type/Urho.Resources.ResourceCache/) podsystemu (dostępna jako [ `Application.ResourceCache` ](https://developer.xamarin.com/api/property/Urho.Application.ResourceCache/)).

Same zasoby są identyfikowane przez ich ścieżek pliku względem katalogów zarejestrowanego zasobu lub pliki pakietu. Domyślnie aparat rejestruje katalogi zasobów `Data` i `CoreData`, lub pakiety `Data.pak` i `CoreData.pak` jeśli takie istnieją.

Jeśli podczas ładowania zasobu nie powiedzie się, zostanie zarejestrowany błąd i zwracany jest odwołanie o wartości null.

Poniższy przykład przedstawia typowy sposób pobierania zasobu z pamięci podręcznej zasobów.  W tym przypadku tekstury dla elementu interfejsu użytkownika, ta metoda korzysta z `ResourceCache` właściwość `Application` klasy.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

Zasoby można również tworzone ręcznie i przechowywane w pamięci podręcznej zasobu tak, jakby były załadowane z dysku.

Budżetów pamięci mogą być ustawiane dla typu zasobu: Jeśli zasobów jest coraz więcej pamięci niż jest to dozwolone, najstarsze zasoby zostaną usunięte z pamięci podręcznej Jeśli nie jest używana już. Domyślnie budżetów pamięci są ustawione na nieograniczoną.

### <a name="bringing-3d-models-and-images"></a>Modele 3D i obrazów

Urho3D próbuje użyć istniejącego formatów plików, jeśli to możliwe i definiowanie niestandardowych formatów plików tylko wtedy, gdy jest to absolutnie konieczne, takie jak w przypadku modeli (*.mdl) i animacji (*.ani). W przypadku tych typów zasobów, Urho zapewnia konwertera - [AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) którego mogą używać wielu popularnych 3D formatach fbx dae, 3ds i obj, np.

Dostępna jest również przydatna dodatek dla pakietu Blender [ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) , można wyeksportować w formacie, który jest odpowiedni dla Urho3D zasobów pakietu Blender.

### <a name="background-loading-of-resources"></a>Ładowanie zasobów w tle

Zwykle podczas żądania zasobów przy użyciu jednej z `ResourceCache`firmy `Get` metody, są one załadowane w wątku głównym, co może potrwać kilka milisekund, wszystkie wymagane kroki (załaduj plik z dysku, analizowanie danych, a następnie Przekaż do procesora GPU w razie potrzeby ) i w związku z tym może spowodować spadnie szybkości klatek.

Jeśli znasz wcześniej zasobów konieczne, możesz poprosić o mogą być ładowane w wątku w tle przez wywołanie metody `BackgroundLoadResource()`. Można subskrybować zdarzenia zasób załadowany w tle, za pomocą `SubscribeToResourceBackgroundLoaded` metody. zostanie powiadomiony, jeśli ładowanie zostało faktycznie sukces lub niepowodzenie. W zależności od zasobów, tylko część procesu ładowania, mogą być przenoszone do wątku w tle, na przykład kroku kończenia przekazywania procesora GPU zawsze musi zostać przeprowadzona w wątku głównym. Należy pamiętać, że wywołanie zasobu ładowania metody do zasobu, który znajduje się w kolejce w tle podczas ładowania wątku głównego będzie zatrzymania do momentu jego ładowania.

Asynchroniczne sceny ładowania funkcji `LoadAsync()` i `LoadAsyncXML()` ma możliwość ładowania tła zasobów najpierw przed przejściem do załadowania zawartości sceny. Może również służyć tylko załadować zasobów bez konieczności modyfikacji sceny, określając `LoadMode.ResourcesOnly`. Dzięki temu przygotować plik sceny lub obiektu prefab dla szybkiego podczas tworzenia wystąpienia.

Na koniec maksymalny czas (w milisekundach) poświęcony każdej ramce Kończenie załadować zasoby mogą być konfigurowane przez ustawienie tła `FinishBackgroundResourcesMs` właściwość `ResourceCache`.

<a name="sound"/>

## <a name="sound"></a>Dźwięk

Dźwięk jest ważną częścią gry i framework na platformie UrhoSharp stanowi sposób odtwarzanie dźwięków w grze.  Odtwarzanie dźwięków przez dołączenie [`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource/)
składnik do [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node) i następnie odtwarzanie wskazanego pliku z Twoich zasobów.

Jest to, jak to zrobić:

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>Cząstki

Cząstki zapewniają prosty sposób dodawania pewne proste i niedrogie efekty do aplikacji.  Za pomocą narzędzi, takich jak mogą wykorzystywać cząstki są przechowywane w formacie PEX [ http://onebyonedesign.com/flash/particleeditor/ ](http://onebyonedesign.com/flash/particleeditor/).

Cząstki są składnikami, które mogą być dodawane do węzła.  Potrzebne do wywoływania podrzędnego `CreateComponent<ParticleEmitter2D>` metodę, aby utworzyć cząstka, a następnie skonfiguruj cząstka przez ustawienie właściwości efektu 2D efekt, który jest ładowany z pamięci podręcznej zasobów.

Na przykład można wywołać tej metody na składnik w taki sposób, aby wyświetlić niektóre cząstek, które są renderowane jako rozłożenie w przypadku trafienia:

```csharp
public async void Explode (Component target)
{
    // show a small explosion when the missile reaches an aircraft.
    var cache = Application.ResourceCache;
    var explosionNode = Scene.CreateChild();
    explosionNode.Position = target.Node.WorldPosition;
    explosionNode.SetScale(1f);
    var particle = explosionNode.CreateComponent<ParticleEmitter2D>();
    particle.Effect = cache.GetParticleEffect2D("explosion.pex");
    var scaleAction = new ScaleTo(0.5f, 0f);
    await explosionNode.RunActionsAsync(
        scaleAction, new DelayTime(0.5f));
    explosionNode.Remove();
}
```

Powyższy kod zostanie utworzony węzeł rozłożenie, który jest dołączony do bieżącego składnika, w tym węźle rozłożenia utworzymy nadajnika cząstki 2D i jest skonfigurowana przez ustawienie właściwości efektu.  Przeprowadzamy dwie akcje jedną, która skaluje się węzeł, który ma być mniejszy i taki, który pozostawi je pod uwagę 0,5 sekund.  Następnie usuń rozłożenie, co spowoduje również usunięcie efekt cząsteczkowy "na ekranie.

Cząstka powyżej renderuje następująco, korzystając z teksturę kuli:

![Cząstki z teksturę kuli](using-images/image-1.png "powyżej cząstki renderuje następująco, korzystając z teksturę kuli")

I to wygląda użycie bloki tekstury:

![Cząstki teksturą pole](using-images/image-2.png "to wygląda korzystania bloki tekstury")

## <a name="multithreading-support"></a>Obsługa wielowątkowości

Na platformie UrhoSharp to jedna biblioteka wątków.  Oznacza to, że nie należy próbować wywołać metod w platformie UrhoSharp z wątku w tle lub ryzyko uszkodzenia stan aplikacji, a prawdopodobieństwo awarii aplikacji.

Jeśli chcesz uruchomić jakiś kod w tle, a następnie zaktualizować składniki Urho na głównego interfejsu użytkownika, możesz użyć [`Application.InvokeOnMain(Action)`](https://developer.xamarin.com/api/member/Urho.Application.InvokeOnMain)
Metoda.  Ponadto można Użyj await C# i .NET zadań interfejsów API, aby upewnić się, że kod jest wykonywany w odpowiednich wątku.

## <a name="urhoeditor"></a>UrhoEditor

Dla danej platformy można pobrać edytora Urho [Urho witryny sieci Web](http://urho3d.github.io/), przejdź do plików do pobrania i pobrania najnowszej wersji.

## <a name="copyrights"></a>Prawa autorskie

Niniejsza dokumentacja zawiera oryginalną zawartość z Xamarin Inc, ale rysuje często z dokumentacji "open source" dla projektu Urho3D i zawiera zrzuty ekranu z projektu Cocos2D.
