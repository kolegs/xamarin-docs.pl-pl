---
title: Przy użyciu UrhoSharp
description: Omówienie aparatu UrhoSharp
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 7d54203fe391af6acde70f4c2a073b7f71332c91
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="using-urhosharp"></a>Przy użyciu UrhoSharp

_Omówienie aparatu UrhoSharp_

Przed przystąpieniem do napisania pierwszy gry chcesz pobrać zapoznaniu podstawy: Konfigurowanie sceny, jak załadować zasoby (zawiera kompozycji) oraz sposobu tworzenia prostych interakcje gry.

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>Sceny, węzłów, składników i aparaty fotograficzne

Model sceny można określić jako wykres na podstawie składnika sceny. Sceny składa się z hierarchii sceny węzłów, począwszy od węzła głównego, reprezentujący całej sceny. Każdy [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/) ma transformacja 3D (pozycja, obracanie i skalowania), nazwę, identyfikator oraz dowolną liczbę składników.  Składniki przenieść węzła do życia, mogą one ułatwić dodawanie wizualną reprezentację ([`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)), mogły one wysyłać dźwięk ([`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource)), zapewniają granic kolizji i tak dalej.

Można utworzyć sceny, a Instalator węzłów za pomocą [Edytor Urho](#UrhoEditor), lub umożliwia wprowadzanie zmian w kodzie C#.  W tym dokumencie przeanalizujemy różne rzeczy przy użyciu kodu, ponieważ oni zilustrowania elementy niezbędne do rozpoczęcia wykonywania zadań wyświetlone na ekranie

Oprócz konfigurowania sceny, należy w Instalatorze [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/), jest to, co określa, jakie uzyskać pokazywana użytkownikowi.

### <a name="setting-up-your-scene"></a>Konfigurowanie sceny

Przeważnie tworzy się tego formularza metody uruchamiania:

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

Renderowanie obiektów 3W, odtwarzanie dźwięku, fizycznych i korzystające ze skryptów logiki aktualizacji są włączone przez utworzenie różnych składników do węzłów, wywołując [ `CreateComponent<T>()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateComponent%3CT%3E/p/Urho.CreateMode/System.UInt32/).  Na przykład instalacji węzła, a światła składnika następująco:

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

Utworzono powyżej węzła o nazwie "`DirectionalLight`" i skonfigurować go, ale nic kierunku.  Teraz, firma Microsoft można przekształcić w węźle powyżej węzła wysyłającą światło, dołączając [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) składnika, z `CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

Składniki utworzone w `Scene` może mieć specjalne uprawnienia: do implementowania całej sceny. Powinny zostać utworzone przed wszystkimi innymi składnikami i obejmują następujące czynności:

* [`Octree`](https://developer.xamarin.com/api/type/Urho.Octree/): implementuje przestrzennych partycjonowania i przyspieszony widoczność zapytania. Bez tego 3D obiektów może nie być renderowane.
* [`PhysicsWorld`](https://developer.xamarin.com/api/type/Urho.Physics.PhysicsWorld/): implementuje symulacji fizycznych. Fizyka składniki, takie jak [ `RigidBody` ](https://developer.xamarin.com/api/type/Urho.Physics.RigidBody/) lub [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) może nie działać poprawnie bez niej.
* [`DebugRenderer`](https://developer.xamarin.com/api/type/Urho.DebugRenderer/): implementuje debugowania geometrii renderowania.

Zwykłe składników, takich jak [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light), [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera) lub [ `StaticModel` ](https://developer.xamarin.com/api/type/Urho.StaticModel) nie należy tworzyć bezpośrednio do [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene), ale raczej do węzłów podrzędnych.

Biblioteka jest dostarczany z różnych składników, które można dołączyć do węzły w celu zapewnienia ich życia: elementy widoczny dla użytkownika (modeli), dźwięki, sztywne treści, kolizji kształtów, aparaty fotograficzne, źródła światła, emitery cząstki i wiele innych.

### <a name="shapes"></a>Kształty

Dla wygody różnych kształtów są dostępne jako proste węzłów w przestrzeni nazw Urho.Shapes.  Obejmują one pola, obszary, szyszki, cylindrów i płaszczyzny.

### <a name="camera-and-viewport"></a>Aparat fotograficzny i okienka ekranu

Podobnie jak światło, kamer są składnikami, więc należy dołączyć składnik do węzła, odbywa się podobnie do następującej:

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

O tym, utworzono aparatu i umieszczono aparatu na świecie 3D, następnym krokiem jest poinformowanie `Application` czy jest to kamera, która ma być używany, odbywa się z następującym kodem:

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

I teraz powinno być możliwe wyświetlić wyniki tworzenia użytkownika.

### <a name="identification-and-scene-hierarchy"></a>Identyfikowanie i sceny hierarchii

W przeciwieństwie do węzłów składniki nie mają nazw; składniki w tym samym węźle tylko są identyfikowane według typu i indeks w węzła składnika listę, która jest wypełniony w kolejności tworzenia, na przykład można pobrać [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light) składnika z `lightNode` obiektu powyżej podobnie do następującej:

```csharp
var myLight = lightNode.GetComponent<Light>();
```

Lista wszystkich składników można także uzyskać, pobierając [ `Components` ](https://developer.xamarin.com/api/property/Urho.Node.Components/) właściwość, która zwraca `IList<Component>` pomocne.

Podczas tworzenia, zarówno węzły i składniki pobrać identyfikatorów sceny globalnej liczby całkowitej. Może być badana z sceny za pomocą funkcji [ `GetNode(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetNode/p/System.UInt32/) i [ `GetComponent(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetComponent/p/System.UInt32/). To jest znacznie szybsza niż na przykład wykonywania cyklicznych zapytań węzła sceny na podstawie nazwy.

Brak nie wbudowanego pojęcia jednostki lub obiekt gier; zamiast jest maksymalnie programisty podjęcie hierarchii węzeł i w węzły, które można umieścić wszelka logika inicjowanych przez skrypty. Zazwyczaj wolne przenoszenie obiektów w świecie 3D zostałyby utworzone jako elementy podrzędne węzła głównego. Węzły można utworzyć lub bez nazwy przy użyciu [ `CreateChild()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateChild/p/System.String/Urho.CreateMode/System.UInt32/). Unikatowość nazw węzeł nie jest wymuszana.

Zawsze, gdy ma niektórych hierarchiczna kompozycji, jest zalecane (i w rzeczywistości to konieczne, ponieważ składniki nie mają własnych transformacje 3D) można utworzyć węzła podrzędnego.

Na przykład jeśli znak był zawierający obiekt w jego ręcznie, obiekt powinien mieć własne węzła, który może być elementem nadrzędnym w kości ręcznie znaku (również [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/)).  Wyjątek stanowi fizycznych [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape), które mogą być offsetted i obrócone indywidualnie w odniesieniu do tego węzła.

Należy pamiętać, że [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Node/)na własnych transformacji jest celowo zostało zignorowane, ponieważ optymalizacji podczas obliczania transformacji świata pochodnych węzłów podrzędnych, tak zmieniając go nie obowiązuje i powinna pozostać niezmieniona (pozycja pochodzenia, bez obrotu bez skalowania.)

[`Scene`](https://developer.xamarin.com/api/type/Urho.Node/) węzły można swobodnie pokrewnym. Z kolei składniki zawsze należy do węzła powiązany i nie można przenosić między węzłami. Podaj zarówno węzły i składniki [ `Remove()` ](https://developer.xamarin.com/api/member/Urho.Node.Remove()/) funkcji, aby osiągnąć ten cel bez konieczności przechodzenia przez element nadrzędny. Gdy węzeł zostanie usunięty, nie ma operacji na węzeł lub składnik programu są bezpieczne po wywołaniu tej funkcji.

Istnieje również możliwość utworzenia `Node` nienależącym do sceny. Jest to przydatne na przykład za pomocą aparatu przenoszenie sceny, który może być załadowany lub zapisane, ponieważ aparat nie zostaną zapisane razem z rzeczywistego sceny, a następnie nie zostaną usunięte po załadowaniu sceny. Jednak należy pamiętać, że tworzenie składników geometry, fizycznych lub skrypt do węzła odłączyć, a następnie jego przeniesieniem do sceny później spowoduje, że te składniki, może nie działać poprawnie.

### <a name="scene-updates"></a>Aktualizacje sceny

Sceny, w której aktualizacje są włączone (ustawienie domyślne) zostanie automatycznie zaktualizowana w każdej iteracji pętli głównej.  Aplikacji [ `SceneUpdate` ](https://developer.xamarin.com/api/event/Urho.Scene.SceneUpdate/) program obsługi zdarzeń jest wywoływana na nim.

Węzły i składniki mogą być wykluczone z aktualizacji sceny wyłączając je, zobacz [ `Enabled` ](https://developer.xamarin.com/api/member/Urho.Node.Enabled).  Zachowanie zależy od określonego składnika, ale np. wyłączenie składnika obiektów drawable ułatwia też niewidoczne, gdy wyłączenie składnika źródła dźwięku wycisza go. Jeśli węzeł jest wyłączony, wszystkie jego składniki są traktowane jako wyłączone niezależnie od stanu włączenia i wyłączenia.

## <a name="adding-behavior-to-your-components"></a>Dodawanie zachowania do składników programu

Najlepszym sposobem struktury gry jest zapewnienie własne składnik, który hermetyzować aktora lub element na grę.  Dzięki temu funkcja samodzielnego zawartych z zasobów używany do wyświetlania, do jego działania.

Jest to najprostszy sposób dodawania zachowanie składnika akcje, które są instrukcje może kolejka i połączenie wyniku z C# — programowanie asynchroniczne.  Umożliwia zachowanie dla składnika się bardzo wysokim poziomie i uproszczono zorientować się.

Alternatywnie można kontrolować, dokładnie co się stanie do składnika, aktualizując właściwości składnika w każdej ramce (opisanych w sekcji zachowanie na podstawie ramki).

### <a name="actions"></a>Akcje

Zachowanie można dodać do węzłów bardzo łatwo przy użyciu akcji.  Działania może zmieniać różne właściwości węzła i ich wykonania w okresie czasu lub powtarzanie ich wiele razy z krzywej danego animacji.

Rozważmy na przykład węzeł "chmura" na sceny, możesz go zanikania następująco:

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

Akcje są niezmienne obiektów, które umożliwia użycie akcji dla wspierania różnych obiektów.

Typowe idiom jest tworzy akcję, która wykonuje operację wstecznego:

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

Poniższy przykład czy stopniowe obiektu dla Ciebie w okresie trzy sekundy.  Można również uruchomić jedną akcję po kolei, na przykład można najpierw przenieść chmury i je ukryć:

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

Ma zarówno akcje została wykonana, w tym samym czasie, można użyć działania równoległe i podaj wszystkie żądane akcje wykonywane równolegle:

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

W powyższym przykładzie chmury przeniesie i zanikania w tym samym czasie.

Można zauważyć, że te używają C# await, co pozwala na wziąć pod uwagę liniowo działanie, aby uzyskać informacje.

### <a name="basic-actions"></a>Podstawowe akcje

Są obsługiwane w UrhoSharp akcje:

* Przenoszenie węzłów: [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo), [ `MoveBy` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveBy), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `BezierTo` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierTo), [ `BezierBy` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierBy) , [`JumpTo`](https://developer.xamarin.com/api/type/Urho.Actions.JumpTo), [`JumpBy`](https://developer.xamarin.com/api/type/Urho.Actions.JumpBy)
* Obracanie węzłów: [ `RotateTo` ](https://developer.xamarin.com/api/type/Urho.Actions.RotateTo), [`RotateBy`](https://developer.xamarin.com/api/type/Urho.Actions.RotateBy)
* Skalowanie węzłów: [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo), [`ScaleBy`](https://developer.xamarin.com/api/type/Urho.Actions.ScaleBy)
* Zanikania węzłów: [ `FadeIn` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeIn), [ `FadeTo` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeTo), [ `FadeOut` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeOut), [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [`Blink`](https://developer.xamarin.com/api/type/Urho.Actions.Blink)
* Cieniowanie: [ `TintTo` ](https://developer.xamarin.com/api/type/Urho.Actions.TintTo), [`TintBy`](https://developer.xamarin.com/api/type/Urho.Actions.TintBy)
* Czasie: [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [ `Show` ](https://developer.xamarin.com/api/type/Urho.Actions.Show), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `RemoveSelf` ](https://developer.xamarin.com/api/type/Urho.Actions.RemoveSelf), [`ToggleVisibility`](https://developer.xamarin.com/api/type/Urho.Actions.ToggleVisibility)
* Pętle: [ `Repeat` ](https://developer.xamarin.com/api/type/Urho.Actions.Repeat), [ `RepeatForever` ](https://developer.xamarin.com/api/type/Urho.Actions.RepeatForever), [`ReverseTime`](https://developer.xamarin.com/api/type/Urho.Actions.ReverseTime)

Inne zaawansowane funkcje programu to kombinacja [ `Spawn` ](https://developer.xamarin.com/api/type/Urho.Actions.Spawn) i [ `Sequence` ](https://developer.xamarin.com/api/type/Urho.Actions.Sequence) akcje.

### <a name="easing---controlling-the-speed-of-your-actions"></a>Ułatwianie - kontrolowanie prędkości czynności użytkownika

Ułatwianie jest sposób, który kieruje sposób ujawniać będzie animacji, czy go animacji znacznie więcej przyjemne.  Domyślnie czynności użytkownika będzie mieć zachowanie liniowego, na przykład [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo) akcji musi bardzo zautomatyzowanej przepływu.  Podczas opakowywania akcje użytkownika na akcję dynamiki Aby zmienić to zachowanie, na przykład czy wolno start przemieszczania, przyspieszanie i powoli zakończą się ([`EasyInOut`](https://developer.xamarin.com/api/type/Urho.Actions.EasyInOut)).

Aby to zrobić, zawijania istniejącą akcję do sterowania tempem zmian akcji, na przykład:

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

Istnieje wiele metod sterowania tempem zmian, w poniższej tabeli przedstawiono różne typy sterowania tempem zmian i ich zachowanie na wartość obiektu, który kontrolować przez okres czasu, od początku do końca:

![Ułatwianie tryby](using-images/easing.png "ten wykres pokazuje różnych typów sterowania tempem zmian i ich zachowanie na wartość obiektu kontrolować w okresie czasu")

### <a name="using-actions-and-async-code"></a>Przy użyciu akcji i kod Async

W Twojej [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component/) podklasy, należy wprowadzić to metoda asynchroniczna, który przygotowuje Twoje zachowanie składnika i dyski funkcji dla niego.
Powodowałoby wywołanie tej metody, przy użyciu języka C#, a następnie `await` — słowo kluczowe z części innego programu, albo z `Application.Start` metody lub w odpowiedzi na użytkownika lub wątek punktu w aplikacji.

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

W `Launch` metody powyżej trzy czynności są uruchamiane: robota wejścia sceny, ta akcja spowoduje zmianę lokalizacji węzła w okresie 0,6 sekund.  Ponieważ jest to opcja asynchronicznego, nastąpi to jednocześnie jako następną instrukcję, która jest wywołanie do `MoveRandomly`.  Ta metoda zmieni położenie robota równolegle losowo wybranej lokalizacji.  Jest to osiągane przez wykonanie dwie akcje złożone, ruch do nowej lokalizacji, a po powrocie do oryginalnej pozycji i powtórz tę czynność, dopóki robota pozostaje aktywne.  I Postaramy bardziej interesujące, robota będzie przechowywać premia jednocześnie.  Wydawania uruchamia się co 0,1 sekund.

### <a name="frame-based-behavior-programming"></a>Zachowanie na podstawie ramki programowania

Jeśli chcesz kontrolować zachowanie składnika na podstawie przez klatka zamiast akcje, co możesz zrobić jest zastąpienie [ `OnUpdate` ](https://developer.xamarin.com/api/member/Urho.Component.OnUpdate) metody z [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component) podklasy.  Ta metoda jest wywoływana raz na klatkę i jest wywoływane tylko wtedy, gdy właściwość ReceiveSceneUpdates zostanie ustawiona wartość true.

Poniżej pokazano, jak utworzyć `Rotator` składnika, następnie dołączony do węzła, co powoduje, że węzeł obracanie:

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

I jest to, jak będzie dołączać ten składnik do węzła:

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>Łączenie style

Można użyć danego modelu async/akcji na podstawie znacznie zachowania, które jest doskonałym rozwiązaniem fire i zapomnij styl programowania w języku programowania, ale można również precyzyjnego określania zachowania danego składnika można także uruchomić kod niektórych aktualizacji w każdej ramce.

Na przykład w pokaz SamplyGame jest on używany w `Enemy` klasy koduje akcje używa podstawowe zachowanie, ale również zapewnia punkt składniki kierunku użytkownika przez ustawienie kierunku węzła o `Node.LookAt`:

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

Sceny może być załadowany i zapisane w formacie XML. Zobacz funkcje [ `LoadXml` ](https://developer.xamarin.com/api/member/Urho.Scene.LoadXml) i [ `SaveXML()` ](https://developer.xamarin.com/api/member/Urho.Scene.SaveXml). Po załadowaniu sceny najpierw usunąć wszystkie istniejące zawartości (węzły podrzędne i składniki). Węzły i składników, które są oznaczone jako tymczasowy z `Temporary` właściwości nie zostaną zapisane. Serializator wszystkich wbudowanych składników i właściwości, ale nie inteligentnych do obsługi niestandardowej właściwości i pola zdefiniowane w Twojej podklasy składnika. Natomiast udostępnia dwie metody wirtualne w tym:

* [`OnSerialize`](https://developer.xamarin.com/api/member/Urho.Component.OnSerialize) gdzie można zarejestrować przypadku stanów niestandardowych dla serializacji

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

Tylko podczas ładowania lub zapisywania całego sceny nie jest wystarczająco elastyczny, gier której nowych obiektów muszą być tworzone dynamicznie. Z drugiej strony tworzenia złożonych obiektów i ustawiania ich właściwości w kodzie będzie również nużące. Z tego powodu jest również można zapisać węzła sceny, który będzie zawierać jej podrzędnych węzłów, składników i atrybutów. Później łatwo te mogą być ładowane jako grupa.  Obiekt zapisany jest często określany jako prefab. Istnieją trzy sposoby, w tym:

- W kodzie, wywołując [ `Node.SaveXml` ](https://developer.xamarin.com/api/member/Urho.Node.SaveXml) w węźle
- W edytorze, wybierając węzeł w oknie hierarchii i wybierając pozycję "zapisać węzeł jako" z menu "File".
- Za pomocą polecenia "węzła" w `AssetImporter`, która zapisze hierarchii węzła sceny i żadnych modeli zawartych w wejściowych zasobów (np.) Plik Collada)

Można utworzyć wystąpienia węzła zapisane do sceny, wywołaj [ `InstantiateXml()` ](https://developer.xamarin.com/api/member/Urho.Scene.InstantiateXml). Węzeł zostanie utworzona jako element podrzędny sceny, ale może być za darmo pokrewnym po tym. Położenie i obrót do umieszczenia w węźle muszą być określone. Poniższy kod ilustruje sposób tworzenia wystąpienia prefab `Ninja.xm` do sceny z odpowiednią pozycję i obrotu:

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>Zdarzenia

UrhoObjects podnieść liczbę zdarzeń, te są udostępniane jako C# zdarzenia różnych klas, które generują je.  Oprócz języka C# — zdarzenia na podstawie modelu, istnieje również możliwość użycia `SubscribeToXXX` metod, które umożliwia subskrybowanie i Zachowaj token subskrypcji, który później można anulować subskrypcję.  Różnica polega na pierwszej umożliwi wiele wywołań do subskrypcji, drugi tylko jedną umożliwia, ale umożliwia wrażeń stylu lambda podejścia do użycia, a jeszcze, umożliwiają łatwe usuwania subskrypcji.  Są one wykluczają się wzajemnie.

Po zasubskrybowaniu zdarzenia, musisz podać metodę, która przyjmuje argumentu z argumentami odpowiednie zdarzenie.

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

Czasami można zrezygnować z otrzymywania powiadomień dla zdarzeń, w przypadkach, Zapisz wartość zwrotna z wywołania `SubscribeTo` metody i wywoływać dla niego metodę anulowania subskrypcji:

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

Parametr odebranych przez program obsługi zdarzeń jest klasą argumenty zdarzeń jednoznacznie będą specyficzne dla każdego zdarzenia, który zawiera ładunek zdarzenia.

## <a name="responding-to-user-input"></a>Odpowiada na dane wejściowe użytkownika

Możesz uzyskać subskrypcję do różnych zdarzeń, takich jak naciśnięcia klawiszy dół przez subskrybowanie zdarzeń i reagowanie na dane wejściowe są dostarczane:

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

Jednak w wielu scenariuszach ma Twoje programy obsługi aktualizacji sceny sprawdzania z bieżącym stanem klucze przy są aktualizowane i odpowiednio zaktualizować kodu.  Na przykład następujące może posłużyć do zaktualizowania lokalizacji aparatu oparte na przy użyciu klawiatury:

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

Zasoby obejmują w UrhoSharp większości zadań, które są ładowane z pamięci masowej podczas inicjowania środowiska uruchomieniowego:

- [`Animation`](https://developer.xamarin.com/api/type/Urho.Animation/) — używany szkieletowych animacji
- [`Image`](https://developer.xamarin.com/api/type/Urho.Resources.Image) -reprezentuje obrazy przechowywane w różnych formatach graficznych
- [`Model`](https://developer.xamarin.com/api/type/Urho.Model/) -Modeli 3D
- [`Material`](https://developer.xamarin.com/api/type/Urho.Material) -materiałów używany do renderowania modeli.
- [`ParticleEffect`](https://developer.xamarin.com/api/type/Urho.ParticleEffect)- [w tym artykule opisano](http://urho3d.github.io/documentation/1.4/_particles.html) nadajnika cząstki działa, zobacz "[cząstki](#particles)" poniżej.
- [`Shader`](https://developer.xamarin.com/api/type/Urho.Shader) -niestandardowych programów do cieniowania
- [`Sound`](https://developer.xamarin.com/api/type/Urho.Audio.Sound) -dźwięki do odtwarzania, zobacz "[dźwięk](#sound)" poniżej.
- [`Technique`](https://developer.xamarin.com/api/type/Urho.Technique/) -techniki materiału renderowania
- [`Texture2D`](https://developer.xamarin.com/api/type/Urho.Urho2D.Texture2D/) -Tekstury 2W
- [`Texture3D`](https://developer.xamarin.com/api/type/Urho.Texture3D/) -Tekstury 3D
- [`TextureCube`](https://developer.xamarin.com/api/type/Urho.TextureCube/) — Tekstura moduł
- `XmlFile`

Zarządzane i ładowane przez [ `ResourceCache` ](https://developer.xamarin.com/api/type/Urho.Resources.ResourceCache/) podsystemu (dostępna jako [ `Application.ResourceCache` ](https://developer.xamarin.com/api/property/Urho.Application.ResourceCache/)).

Samych zasobach są identyfikowane przez ich ścieżki pliku względem katalogi zarejestrowanych zasobów lub pliki pakietu. Domyślnie aparat rejestruje katalogi zasobów `Data` i `CoreData`, lub pakiety `Data.pak` i `CoreData.pak` jeżeli istnieją.

Czy ładowanie zasobu nie powiedzie się, błąd zostanie zarejestrowany i zwracane jest odwołanie o wartości null.

W poniższym przykładzie przedstawiono typowy sposób pobieranie zasobu z pamięci podręcznej zasobów.  W takim przypadku teksturę dla elementu interfejsu użytkownika używa `ResourceCache` właściwość z `Application` klasy.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

Zasoby można również utworzone ręcznie i przechowywane w pamięci podręcznej zasobu tak, jakby były załadowane z dysku.

Budżetów pamięci można ustawić na typ zasobu: Jeśli zasoby zużywać więcej pamięci niż jest to dozwolone, najstarsze zasoby zostaną usunięte z pamięci podręcznej w przeciwnym razie używanymi już. Domyślnie budżetów pamięci są ustawione na nieograniczony.

### <a name="bringing-3d-models-and-images"></a>Przywracanie modele 3D i obrazów

Urho3D próbuje użyć istniejącego formatów plików w miarę możliwości i definiowanie niestandardowych formatów plików tylko wtedy, gdy jest to bezwzględnie konieczne, takie jak w przypadku modeli (*.mdl) i animacji (*.ani). W przypadku tych typów zasobów Urho zapewnia konwertera - [AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) zużywające wielu popularnych formatach 3D fbx dae, 3ds i obj, np.

Dostępna jest również przydatne dodatku dla mieszarce [ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) który można wyeksportować w formacie, który jest odpowiedni dla Urho3D zasobów mieszarce.

### <a name="background-loading-of-resources"></a>Tło ładowanie zasobów

Zwykle podczas żądania zasobów przy użyciu jednej z `ResourceCache`w `Get` metody są załadowane bezpośrednio w głównym wątku, co może potrwać kilka milisekund wszystkie kroki wymagane (załaduj plik z dysku, analizy danych, przekazywanie do procesora GPU, jeśli to konieczne ) i w związku z tym może spowodować porzucania szybkość klatek.

Jeśli znasz z wyprzedzeniem zasobów konieczne, możesz poprosić o mogą być ładowane w wątku w tle przez wywołanie metody `BackgroundLoadResource()`. Będzie możliwe subskrybowanie zdarzeń załadować tła zasobów przy użyciu `SubscribeToResourceBackgroundLoaded` metody. zostanie powiadomiony, czy ładowanie faktycznie się sukcesem lub niepowodzeniem. W zależności od zasobów, tylko część procesu ładowania mogą być przenoszone do wątku w tle, na przykład kroku kończenia przekazywania procesora GPU zawsze musi mieć miejsce w głównym wątku. Należy pamiętać, że wywołanie zasobu ładowania metody do zasobu, który jest w kolejce w tle podczas ładowania wątku głównego spowoduje zatrzymania aż do zakończenia jego ładowania.

Asynchroniczne sceny funkcji ładowania `LoadAsync()` i `LoadAsyncXML()` ma możliwość obciążenia tła zasobów najpierw przed przystąpieniem do załadowania zawartości sceny. Może również służyć tylko załadować zasobów bez modyfikowania sceny, określając `LoadMode.ResourcesOnly`. Dzięki temu przygotowania sceny lub obiektu pliku prefab potrzeby szybkiego utworzenia wystąpienia obiektu.

Na koniec maksymalny czas (w milisekundach) działania każdej ramce na zakończenie załadować zasobów można skonfigurować ustawienie tła `FinishBackgroundResourcesMs` właściwości na `ResourceCache`.

<a name="sound"/>

## <a name="sound"></a>Dźwięk

Dźwięk jest ważnym elementem gry i UrhoSharp framework zapewnia możliwość odtwarzanie dźwięku w grę.  Odtwarzanie dźwięków dołączając [ `SoundSource` ](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource/) składnika do [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node) , a następnie odtworzyć wskazanego pliku z Twoich zasobów.

Jest to, jak jest wykonywane:

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>Cząstki

Cząstki zapewniają prosty sposób dodawania niektóre proste i niedrogie efekty w aplikacji.  Za pomocą takich narzędzi jak może wykorzystać cząstki przechowywane w formacie PEX [ http://onebyonedesign.com/flash/particleeditor/ ](http://onebyonedesign.com/flash/particleeditor/).

Cząstki są składniki, które mogą zostać dodane do węzła.  Należy wywołać węzła `CreateComponent<ParticleEmitter2D>` metodę, aby utworzyć cząstka, a następnie skonfiguruj cząstka przez ustawienie właściwości efekt 2D efekt, który jest ładowany z pamięci podręcznej zasobów.

Na przykład można wywołać tej metody na składnika można wyświetlić niektórych cząstek, które mają być renderowane jako rozłożenie podczas jego trafienia:

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

Powyższy kod utworzy rozłożenia węzeł, który jest dołączony do bieżącego składnika, w tym węźle rozłożenia możemy utworzyć nadajnika 2D cząstki i skonfigurować go przez ustawienie właściwości efekt.  Przeprowadzana dwie akcje, jedną, która może obsłużyć węzeł może być mniejszy, a drugie pozostawia jej w tym rozmiarze 0,5 sekund.  Następnie usuń masowego, co spowoduje również usunięcie cząstki wpływu na ekranie.

Cząstka powyżej renderuje takie, używając tekstury kuli:

![Cząstki teksturą kuli](using-images/image-1.png "powyżej cząstki renderuje takie, używając tekstury kuli")

I to wygląda użycie bloki tekstury:

![Cząstki teksturą pole](using-images/image-2.png "i są to wygląda używania bloki tekstury")

## <a name="multithreading-support"></a>Obsługa wielowątkowości

UrhoSharp to jedna biblioteka wątków.  Oznacza to, że nie należy próbować wywołać metod w UrhoSharp z wątku w tle lub ryzyko uszkodzenia stanu aplikacji i może ulec awarii aplikacji.

Jeśli chcesz uruchomić kodu w tle, a następnie zaktualizuj Urho składników na głównym interfejsu użytkownika, możesz użyć [ `Application.InvokeOnMain(Action)` ](https://developer.xamarin.com/api/member/Urho.Application.InvokeOnMain) metody.  Ponadto można Użyj C# await i .NET zadanie interfejsów API, aby upewnić się, że kod jest wykonywany w odpowiednich wątku.

## <a name="urhoeditor"></a>UrhoEditor

Możesz pobrać edytor Urho dla danej platformy z [Urho witryny sieci Web](http://urho3d.github.io/), przejdź do pobrania i pobrania najnowszej wersji.

## <a name="copyrights"></a>Prawa autorskie

W tej dokumentacji zawiera oryginalną zawartość z Xamarin Inc, ale często rysuje dokumentacji typu open source dla projektu Urho3D i zawiera zrzuty ekranu z projektu Cocos2D.

## <a name="related-links"></a>Linki pokrewne

- [Planety ziemi skoroszytu](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [Eksploracja współrzędne skoroszytu](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
