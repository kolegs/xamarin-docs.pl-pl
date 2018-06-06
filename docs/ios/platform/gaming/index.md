---
title: interfejsy API gier w Xamarin.iOS systemu iOS
description: W tym artykule omówiono nowe ulepszenia gier dostarczane przez system iOS 9, który może służyć do poprawy Twoja gra Xamarin.iOS grafiki i funkcji audio.
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 263c325816867e9eee32c92edf97f703b39bda7c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786863"
---
# <a name="ios-gaming-apis-in-xamarinios"></a>interfejsy API gier w Xamarin.iOS systemu iOS

_W tym artykule omówiono nowe ulepszenia gier dostarczane przez system iOS 9, który może służyć do poprawy Twoja gra Xamarin.iOS grafiki i funkcji audio._

Apple oferuje ulepszenia wielu technologii do gier interfejsów API w systemie iOS 9 ułatwiające wdrażanie grafikę gier i audio w aplikacji platformy Xamarin.iOS.
Dotyczy to zarówno łatwość programowania za pośrednictwem struktury wysokiego poziomu i wykorzystanie mocy procesora GPU urządzenie iOS ulepszone szybkości i możliwości graficzne.

[![](images/flocking01.png "Przykład systemem flocking aplikacji")](images/flocking01.png#lightbox)

W tym GameplayKit, ReplayKit, Model we/wy, MetalKit i programów do cieniowania wydajności systemu operacyjnego oraz nowego, ulepszone funkcje systemu operacyjnego, SceneKit i SpriteKit.

W tym artykule przedstawiono wszystkie sposobów poprawy gry Xamarin.iOS nowych ulepszeń gier dla systemu iOS 9 w:

## <a name="introducing-gameplaykit"></a>Wprowadzenie do GameplayKit

Nowa struktura GameplayKit firmy Apple zawiera zestaw technologii, który można łatwo tworzyć gry dla urządzeń z systemem iOS przez skrócenie czasu powtarzających się typowe kodu wymaganych do wdrożenia. GameplayKit zawiera narzędzi do tworzenia gier mechanika, który można łatwo połączyć z aparatem grafiki (na przykład SceneKit lub SpriteKit) do szybkiego dostarczania gry ukończone.

GameplayKit obejmuje kilka typowych, gry odtwarzać algorytmów, takich jak:

- Zachowanie na podstawie, symulacji agenta, który służy do definiowania przepływu i cele, które automatycznie przeprowadzi AI.
- Minmax sztucznego analizy na podstawie Włącz gry.
- System reguł opartych na danych gier logiki z rozmytego uzasadnienie, aby zapewnić zachowanie wschodzący.

Ponadto GameplayKit przyjmuje podejścia bloku konstrukcyjnego do tworzenia gier za pomocą modularnej architekturze, który udostępnia następujące funkcje:

- Automatu stanów obsługi złożonych, procedurach kodu na podstawie systemów w gry.
- Narzędzia umożliwiające wybierane gry i nieprzewidywalność bez powodowania debugowania problemów.
- Architektura wielokrotnego użytku, składnikowa jednostki na podstawie.

Aby dowiedzieć się więcej na temat GameplayKit, zobacz firmy Apple [przewodnik programowania w języku Gameplaykit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) i [odwołania Framework GameplayKit](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199).

## <a name="gameplaykit-examples"></a>Przykłady GameplayKit

Spójrzmy szybkie na wdrażanie w aplikacji platformy Xamarin.iOS przy użyciu zestawu gry mechanika niektórych prostych gry.

### <a name="pathfinding"></a>Pathfinding

Pathfinding jest możliwość wyszukiwania jego nawigację tablicy gier dla elementu AI gry.
Na przykład 2D wroga sposób Labirynt lub znak 3D za pośrednictwem terenu world technologia pierwszej osoby.

Należy wziąć pod uwagę następujące mapy:

[![](images/gkpathfindpath.png "Przykładowa mapa pathfinding")](images/gkpathfindpath.png#lightbox)

Przy użyciu pathfinding tego kodu C# można znaleźć sposób za pośrednictwem mapy:

```csharp
var a = GKGraphNode2D.FromPoint (new Vector2 (0, 5));
var b = GKGraphNode2D.FromPoint (new Vector2 (3, 0));
var c = GKGraphNode2D.FromPoint (new Vector2 (2, 6));
var d = GKGraphNode2D.FromPoint (new Vector2 (4, 6));
var e = GKGraphNode2D.FromPoint (new Vector2 (6, 5));
var f = GKGraphNode2D.FromPoint (new Vector2 (6, 0));

a.AddConnections (new [] { b, c }, false);
b.AddConnections (new [] { e, f }, false);
c.AddConnections (new [] { d }, false);
d.AddConnections (new [] { e, f }, false);

var graph = GKGraph.FromNodes(new [] { a, b, c, d, e, f });

var a2e = graph.FindPath (a, e); // [ a, c, d, e ]
var a2f = graph.FindPath (a, f); // [ a, b, f ]

Console.WriteLine(String.Join ("->", (object[]) a2e));
Console.WriteLine(String.Join ("->", (object[]) a2f));
```

### <a name="classical-expert-system"></a>System ekspert klasycznego

Poniższy fragment kodu C# pokazano, jak można użyć GameplayKit wdrożenia klasycznego systemu ekspert:

```csharp
string output = "";
bool reset = false;
int input = 15;

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    /*
    If reset is true, clear the output and set reset to false
    */
    var clearRule = GKRule.FromPredicate ((rules) => reset, rules => {
        output = "";
        reset = false;
    });
    clearRule.Salience = 1;

    var fizzRule = GKRule.FromPredicate (mod (3), rules => {
        output += "fizz";
    });
    fizzRule.Salience = 2;

    var buzzRule = GKRule.FromPredicate (mod (5), rules => {
        output += "buzz";
    });
    buzzRule.Salience = 2;

    /*
    This *always* evaluates to true, but is higher Salience, so evaluates after lower-salience items
    (which is counter-intuitive). Print the output, and reset (thus triggering "ResetRule" next time)
    */
    var outputRule = GKRule.FromPredicate (rules => true, rules => {
        System.Console.WriteLine(output == "" ? input.ToString() : output);
        reset = true;
    });
    outputRule.Salience = 3;

    var rs = new GKRuleSystem ();
    rs.AddRules (new [] {
        clearRule,
        fizzRule,
        buzzRule,
        outputRule
    });

    for (input = 1; input < 16; input++) {
        rs.Evaluate ();
        rs.Reset ();
    }
}

protected Func<GKRuleSystem, bool> mod(int m)
{
    Func<GKRuleSystem,bool> partiallyApplied = (rs) => input % m == 0;
    return partiallyApplied;
}
```

Na podstawie danego zestawu reguł (`GKRule`) i znanych zestaw wejść, expert system (`GKRuleSystem`) spowoduje utworzenie wartości prognozowanych danych wyjściowych (`fizzbuzz` w naszym przykładzie powyżej).

### <a name="flocking"></a>Flocking

Flocking umożliwia grupy AI kontrolowane gier jednostek będzie działać jako stada, w którym grupa odpowiada przepływu i akcje jednostki potencjalnych klientów, takich jak stadzie ptaki w locie lub szkoły ryb swimming.

Poniższy fragment kodu C# implementuje flocking zachowanie przy użyciu GameplayKit i SpriteKit wyświetlania grafiki:

```csharp
using System;
using SpriteKit;
using CoreGraphics;
using UIKit;
using GameplayKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
using OpenTK;

namespace FieldBehaviorExplorer
{
    public static class FlockRandom
    {
        private static GKARC4RandomSource rand = new GKARC4RandomSource ();

        static FlockRandom ()
        {
            rand.DropValues (769);
        }

        public static float NextUniform ()
        {
            return rand.GetNextUniform ();
        }
    }

    public class FlockingScene : SKScene
    {
        List<Boid> boids = new List<Boid> ();
        GKComponentSystem componentSystem;
        GKAgent2D trackingAgent; //Tracks finger on screen
        double lastUpdateTime = Double.NaN;
        //Hold on to behavior so it doesn't get GC'ed
        static GKBehavior flockingBehavior;
        static GKGoal seekGoal;


        public FlockingScene (CGSize size) : base (size)
        {
            AddRandomBoids (20);

            var scale = 0.4f;
            //Flocking system
            componentSystem = new GKComponentSystem (typeof(GKAgent2D));
            var behavior = DefineFlockingBehavior (boids.Select (boid => boid.Agent).ToArray<GKAgent2D>(), scale);
            boids.ForEach (boid => {
                boid.Agent.Behavior = behavior;
                componentSystem.AddComponent(boid.Agent);
            });

            trackingAgent = new GKAgent2D ();
            trackingAgent.Position = new Vector2 ((float) size.Width / 2.0f, (float) size.Height / 2.0f);
            seekGoal = GKGoal.GetGoalToSeekAgent (trackingAgent);
        }

        public override void TouchesBegan (NSSet touches, UIEvent evt)
        {
            boids.ForEach(boid => boid.Agent.Behavior.SetWeight(1.0f, seekGoal));
        }

        public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            boids.ForEach (boid => boid.Agent.Behavior.SetWeight (0.0f, seekGoal));
        }

        public override void TouchesMoved (NSSet touches, UIEvent evt)
        {
            var touch = (UITouch) touches.First();
            var loc = touch.LocationInNode (this);
            trackingAgent.Position = new Vector2((float) loc.X, (float) loc.Y);
        }


        private void AddRandomBoids (int count)
        {
            var scale = 0.4f;
            for (var i = 0; i < count; i++) {
                var b = new Boid (UIColor.Red, this.Size, scale);
                boids.Add (b);
                this.AddChild (b);
            }
        }

        internal static GKBehavior DefineFlockingBehavior(GKAgent2D[] boidBrains, float scale)
        {
            if (flockingBehavior == null) {
                var flockingGoals = new GKGoal[3];
                flockingGoals [0] = GKGoal.GetGoalToSeparate (boidBrains, 100.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [1] = GKGoal.GetGoalToAlign (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [2] = GKGoal.GetGoalToCohere (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);

                flockingBehavior = new GKBehavior ();
                flockingBehavior.SetWeight (25.0f, flockingGoals [0]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [1]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [2]);
            }
            return flockingBehavior;
        }

        public override void Update (double currentTime)
        {
            base.Update (currentTime);
            if (Double.IsNaN(lastUpdateTime)) {
                lastUpdateTime = currentTime;
            }
            var delta = currentTime - lastUpdateTime;
            componentSystem.Update (delta);
        }
    }

    public class Boid : SKNode, IGKAgentDelegate
    {
        public GKAgent2D Agent { get { return brains; } }
        public SKShapeNode Sprite { get { return sprite; } }

        class BoidSprite : SKShapeNode
        {
            public BoidSprite (UIColor color, float scale)
            {
                var rot = CGAffineTransform.MakeRotation((float) (Math.PI / 2.0f));
                var path = new CGPath ();
                path.MoveToPoint (rot, new CGPoint (10.0, 0.0));
                path.AddLineToPoint (rot, new CGPoint (0.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 20.0));
                path.AddLineToPoint (rot, new CGPoint (20.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 0.0));
                path.CloseSubpath ();

                this.SetScale (scale);
                this.Path = path;
                this.FillColor = color;
                this.StrokeColor = UIColor.White;

            }
        }

        private GKAgent2D brains;
        private BoidSprite sprite;
        private static int boidId = 0;

        public Boid (UIColor color, CGSize size, float scale)
        {
            brains = BoidBrains (size, scale);
            sprite = new BoidSprite (color, scale);
            sprite.Position = new CGPoint(brains.Position.X, brains.Position.Y);
            sprite.ZRotation = brains.Rotation;
            sprite.Name = boidId++.ToString ();

            brains.Delegate = this;

            this.AddChild (sprite);
        }

        private GKAgent2D BoidBrains(CGSize size, float scale)
        {
            var brains = new GKAgent2D ();
            var x = (float) (FlockRandom.NextUniform () * size.Width);
            var y = (float) (FlockRandom.NextUniform () * size.Height);
            brains.Position = new Vector2 (x, y);

            brains.Rotation = (float)(FlockRandom.NextUniform () * Math.PI * 2.0);
            brains.Radius = 30.0f * scale;
            brains.MaxSpeed = 0.5f;
            return brains;
        }

        [Export ("agentDidUpdate:")]
        public void AgentDidUpdate (GameplayKit.GKAgent agent)
        {
        }

        [Export ("agentWillUpdate:")]
        public void AgentWillUpdate (GameplayKit.GKAgent agent)
        {
            var brainsIn = (GKAgent2D) agent;
            sprite.Position = new CGPoint(brainsIn.Position.X, brainsIn.Position.Y);
            sprite.ZRotation = brainsIn.Rotation;
            Console.WriteLine ($"{sprite.Name} -> [{sprite.Position}], {sprite.ZRotation}");
        }
    }
}
```

Następnie implementować ten sceny kontrolera widoku:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.
        this.View = new SKView {
        ShowsFPS = true,
        ShowsNodeCount = true,
        ShowsDrawCount = true
    };
}

public override void ViewWillLayoutSubviews ()
{
    base.ViewWillLayoutSubviews ();

    var v = (SKView)View;
    if (v.Scene == null) {
        var scene = new FlockingScene (View.Bounds.Size);
        scene.ScaleMode = SKSceneScaleMode.AspectFill;
        v.PresentScene (scene);
    }
}
```

Uruchamiania, nieco animowany _"Boids"_ będzie stadzie wokół naszych podsłuchu palca:

[![](images/flocking01.png "Nieco animowany Boids będzie stadzie wokół podsłuchu palca")](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>Inne przykłady firmy Apple

Oprócz próbek przedstawionych powyżej Apple udostępnia następujące przykładowych aplikacji, które mogą być przekodowane C# i platformy Xamarin.iOS:

- [FourInARow: Przy użyciu Strategist GameplayKit Minmax przeciwnikowi AI](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog: W systemie agentów w GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots: Tworzenie gry Międzyplatformowego SpriteKit i GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>Metal

W systemie iOS 9 Apple wprowadziła kilka zmian i dodatków systemu operacyjnego dostęp niskiego obciążenia procesora GPU. Przy użyciu systemu operacyjnego można zmaksymalizować grafiki i możliwości przetwarzania danych aplikacji systemu iOS.

Struktura metali obejmuje następujące nowe funkcje:

- Nowe prywatne i głębokość wzornika tekstury dla systemu OS X.
- Ulepszone tle jakość z przodu zaczepów i oddzielne głębokość i wartościami wzornika Wstecz.
- Ulepszenia metali cieniowania języka i bibliotekę standardową systemu operacyjnego.
- Obliczeniowa programów do cieniowania obsługi większej liczby formatów pikseli.

### <a name="the-metalkit-framework"></a>W ramach MetalKit

W ramach MetalKit zawiera zestaw klas narzędzia i funkcje, które zmniejszyć nakład pracy wymagany do użycia w aplikacji systemu iOS systemu operacyjnego. MetalKit zapewnia obsługę w trzech obszarach klucza:

1. Ładowanie z różnych źródeł, w tym typowych formatach PNG, JPEG, KTX i PVR asynchroniczne tekstury.
2. Łatwy dostęp do modelu we/wy na podstawie zasoby do obsługi kompletnego stanu określonego modelu. Te funkcje zostały stopniu zoptymalizowany przesyłanie danych wydajne siatki modelu we/wy i buforów kompletnego stanu.
3. Wstępnie zdefiniowanych widoków kompletnego stanu i zarządzanie widoku, który znacznie skrócić kod wymagany do wyświetlania grafiki obrzutki w aplikacji systemu iOS.

Aby dowiedzieć się więcej na temat MetalKit, zobacz firmy Apple [odwołania Framework MetalKit](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356), [przewodnik programowania w języku systemu operacyjnego](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221), [odwołanie do platformy systemu operacyjnego](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) i [systemu operacyjnego Język-Podręcznik cieniowania](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

### <a name="metal-performance-shaders-framework"></a>Framework programów do cieniowania kompletnego stanu wydajności

W ramach programu do cieniowania wydajności systemu operacyjnego zawiera zestaw stopniu zoptymalizowany grafiki i obliczeniowe na podstawie programów do cieniowania do użytku w sieci systemu operacyjnego na podstawie aplikacji systemu iOS. Każdy program do cieniowania w wydajności systemu operacyjnego programu do cieniowania framework ma został zoptymalizowany pod kątem zapewnienie wysokiej wydajności na systemu operacyjnego obsługiwane iOS GPU.

Za pomocą klasy cieniowania wydajności systemu operacyjnego, można osiągnąć najwyższym możliwym poziomie wydajności w każdej określonej procesora GPU w systemie iOS bez konieczności docelowego, a obsługa baz kodu poszczególnych. Kompletnego stanu wydajności programów do cieniowania może służyć z dowolnego zasobu kompletnego stanu, takie jak tekstury i bufory.

Framework cieniowania wydajności systemu operacyjnego zawiera zbiór typowych programów do cieniowania, takich jak:

- **Rozmycia Gaussa** (`MPSImageGaussianBlur`)
- **Wykrywanie krawędzi Sobel** (`MPSImageSobel`)
- **Obraz Histogram** (`MPSImageHistogram`)

Aby uzyskać więcej informacji, zobacz firmy Apple [systemu operacyjnego cieniowania języka przewodnik](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

## <a name="introducing-model-io"></a>Wprowadzenie do modelu we/wy

Framework Model we/wy firmy Apple zapewnia dokładnego zrozumienia 3D zasobów (takich jak modele i ich powiązanych zasobów). We/Wy modelu zawiera swoich gier dla systemu iOS na podstawie fizycznej materiałów modeli i oświetlenia, który może być używany z GameplayKit, systemu operacyjnego i SceneKit.

Z modelu we/wy może obsługiwać następujące rodzaje zadań:

- Importuj oświetlenia materiałów, siatka danych, ustawień aparatu i inne informacje oparte na scenie z różnych popularnych i formaty aparat gier.
- Przetwarzanie lub generowania informacji sceny, takich jak tworzenie proceduralnie teksturą solnych niebo lub wypieków (kwesta) oświetlenia do siatki.
- Współdziała z MetalKit, SceneKit i GLKit wydajnie załadować zasoby gier do procesora GPU buforów do renderowania.
- Eksportuj informacje oparte na scenie do różnych popularnych i formaty aparat gier.

Aby dowiedzieć się więcej na temat modelu we/wy, zobacz firmy Apple [odwołanie modelu we/wy](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)

## <a name="introducing-replaykit"></a>Wprowadzenie do ReplayKit

Nowej struktury ReplayKit firmy Apple umożliwia łatwe dodawanie nagrania gry do gry z systemem iOS i Zezwalaj użytkownikowi na szybkie i łatwe edytowanie i udostępnić ten film z poziomu aplikacji.

Aby uzyskać więcej informacji, zobacz firmy Apple [przechodzi społecznościowych wideo ReplayKit i Game Center](https://developer.apple.com/videos/wwdc/2015/?id=605) i ich [DemoBots: Tworzenie gry Cross Platform z SpriteKit i GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) przykładowej aplikacji.

## <a name="scenekit"></a>SceneKit

Zestaw sceny jest wykres scenę 3D interfejsu API, które upraszcza pracy z grafiki 3D. Została wprowadzona w systemie OS X 10.8 i teraz nadszedł iOS 8. Z zestawem sceny tworzenie wizualizacji 3W bez ramek i zwykłych grach 3W nie wymaga doświadczenia w OpenGL. Opierając się na wspólne pojęcia wykres sceny, zestaw sceny abstracts optymalizacji złożoności OpenGL i interfejsy OpenGL ES, dzięki czemu bardzo łatwo dodać 3D zawartości do aplikacji. Jednak w przypadku eksperta OpenGL sceny zestawu ma doskonałą pomoc techniczną dla poleceń bezpośrednio z również OpenGL. Ponadto zawiera wiele funkcji, które stanowią uzupełnienie grafiki 3D, takie jak fizycznych, a bardzo dobrze integruje się z kilku innych platform firmy Apple, takich jak zestaw Sprite, obrazu Core i Core animacji.

Aby uzyskać więcej informacji, zobacz nasze [SceneKit](~/ios/platform/gaming/scenekit.md) dokumentacji.

### <a name="scenekit-changes"></a>SceneKit zmiany

Apple dodano następujące nowe funkcje do SceneKit systemu iOS 9:

- Xcode udostępnia teraz Edytor sceny, która pozwala na szybkie tworzenie gier i aplikacji 3D interaktywne, edytując sceny bezpośrednio w programie Xcode.
- `SCNView` i `SCNSceneRenderer` klasy mogą być używane, aby umożliwić renderowanie kompletnego stanu (na urządzeniach iOS obsługiwane).
- `SCNAudioPlayer` i `SCNNode` klasy mogą być używane do dodawania przestrzennych efekty audio, które automatycznie śledzą pozycji player w aplikacji systemu iOS.

Aby uzyskać więcej informacji, zobacz nasze [dokumentacji SceneKit](~/ios/platform/introduction-to-ios8.md#scenekit) i firmy Apple [odwołania Framework SceneKit](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) i [Fox: Tworzenie gry SceneKit z edytora sceny Xcode](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154)przykładowy projekt.

## <a name="spritekit"></a>SpriteKit

Zestaw Sprite, gier 2W platformę od firmy Apple, posiada niektóre ciekawe nowe funkcje w systemie iOS 8 i OS X Yosemite. Obejmuje to integrację z zestawu sceny, obsługuje programów do cieniowania oświetlenia, cieni, ograniczenia, generowania mapy normalnej i ulepszenia fizycznych. W szczególności nowe funkcje fizycznych ułatwiają bardzo realistyczne efektów gier.

Aby uzyskać więcej informacji, zobacz nasze [SpriteKit](~/ios/platform/gaming/spritekit.md) dokumentacji.

### <a name="spritekit-changes"></a>SpriteKit zmiany

Apple dodano następujące nowe funkcje do SpriteKit systemu iOS 9:

- Przestrzenne efekt audio, który automatycznie śledzić położenie odtwarzacza z `SKAudioNode` klasy.
- Xcode obejmuje teraz sceny edytora i Edytor akcji ułatwiający 2D tworzenie gier i aplikacji.
- Łatwe przewijanie gier techniczną, podając nowe węzły aparatu (`SKCameraNode`) obiektów.
- Na urządzeniach z systemem iOS, które obsługuje systemu operacyjnego SpriteKit będą automatycznie używać go do renderowania, nawet jeśli były już przy użyciu niestandardowych OpenGL ES programów do cieniowania.

Aby uzyskać więcej informacji, zobacz nasze [dokumentacji SpriteKit](~/ios/platform/introduction-to-ios8.md#spritekit) firmy Apple [odwołania Framework SpriteKit](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) i ich [DemoBots: Tworzenie gry dla wielu Platform z SpriteKit i GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) przykładowej aplikacji.

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego nowe funkcje gier tego systemu iOS 9 udostępnia dla aplikacji platformy Xamarin.iOS.
Spowodował GameplayKit i Model we/wy; poważniejsze ulepszenia do systemu operacyjnego; i nowe funkcje SceneKit i SpriteKit.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
