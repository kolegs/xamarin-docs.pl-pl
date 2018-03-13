---
title: "Przy użyciu ARKit z UrhoSharp"
ms.topic: article
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/01/2016
ms.openlocfilehash: 94963e92f8372316a982bbe38f1fb653d38b2a3b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="using-arkit-with-urhosharp"></a>Przy użyciu ARKit z UrhoSharp

Wraz z wprowadzeniem [ARKit](https://developer.apple.com/arkit/), Apple wprowadził oferuje deweloperom tworzenie aplikacji w rzeczywistości zwiększonej. ARKit można śledzić dokładne położenie urządzenie wykrywanie różnych powierzchni na świecie i jest następnie deweloperowi blend danych wystawała ARKit w kodzie.

[UrhoSharp](~/graphics-games/urhosharp/index.md) zapewnia kompleksowe i łatwy w użyciu API 3D, używanej do tworzenia aplikacji 3D.   Oba te mogą być połączeniu, ARKit zapewnienie fizycznych informacji o świecie i Urho do renderowania wyniki.

Ta strona wyjaśniono, jak połączyć te dwie względem ze sobą w celu tworzenia niezwykłych rzeczywistości zwiększonej aplikacji.


## <a name="the-basics"></a>Podstawy

Chcemy się czy jest obecny zawartość 3D na świecie, widziany przez telefonów iPhone.   Będzie blend zawartość pochodzące z aparatu fotograficznego telefonu z zawartością 3D, a jako użytkownik telefonu przenosi wokół pomieszczeniu, upewnij się, że obiekt 3D zachowywać się jak są one częścią tego miejsca — jest to realizowane przez Zakotwiczanie obiektów w tym world.

![Animowany cyfrze ARKit](urhosharp-images/image1.gif)


Użyjemy biblioteki Urho do ładowania naszych 3D zasobów i umieścić je na świecie, i użyjemy ARKit można uzyskać strumienia wideo pochodzących z aparatu, a także lokalizacji telefonu na całym świecie.   Gdy użytkownik przechodzi z jego telefon, użyjemy zmiany w lokalizacji można zaktualizować współrzędnych, który aparat Urho są wyświetlane.

W ten sposób gdy obiekt jest umieszczony w przestrzeni 3D, a użytkownik przesuwa, lokalizacji obiektu 3D odzwierciedla miejsce i lokalizacji, w którym została umieszczona.

## <a name="setting-up-your-application"></a>Konfigurowanie aplikacji

### <a name="ios-application-launch"></a>Uruchamianie aplikacji systemu iOS

Aplikacji systemu iOS musi utworzyć i uruchomić zawartości 3D, możesz to zrobić, tworząc podklasą implementacja [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) i podaj swój kod instalacji przez zastąpienie `Start` metody.  Jest to, gdzie sceny pobiera zawierają dane, zdarzenie obsługi są Instalatora i tak dalej.

Zostały wprowadzone `Urho.ArkitApp` klasy tego podklasy `Urho.Application` i na jego `Start` metoda wykonuje lifting ciężki.   Wszystkie informacje wymagane do Twojej Urho istniejących aplikacji jest zmiana klasy podstawowej typu `Urho.ArkitApp` i masz aplikację, która zostanie uruchomiony z urho sceny na świecie.

### <a name="the-arkitapp-class"></a>Klasa ArkitApp

Ta klasa udostępnia zestaw wygodne ustawienia domyślne, zarówno sceny, z niektórych obiektów klucza, a także przetwarzania zdarzeń ARKit, jaką są dostarczane przez system operacyjny.

Instalator ma miejsce w `Start` metoda wirtualna.   W przypadku przesłonić tę metodę w podklasa użytkownika, musisz upewnij się, że łańcucha do nadrzędnego za pomocą `base.Start()` na własną implementację.

`Start` — Metoda konfiguruje sceny, okienka ekranu, aparatu i światło kierunkowe i wyświetla je jako właściwości publiczne:

- [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) do przechowywania obiektów,
- kierunkowe [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) cieni i jest dostępny za pośrednictwem którego lokalizacja `LightNode` właściwości
- [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) których elementy są aktualizowane w ramach ARKit dostarcza aktualizacji do aplikacji i
- [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) wyświetlania wyników.


### <a name="your-code"></a>Kod

Następnie należy do podklasy `ArkitApp` klasy i zastąpić `Start` — metoda.   Najpierw, które należy wykonać metodę, to do łańcucha `ArkitApp.Start` przez wywołanie metody `base.Start()`.  Po tym można użyć dowolnej właściwości konfiguracji ArkitApp Aby dodać obiekty do sceny, dostosować świateł, cieni lub zdarzeń, które mają być obsługiwane.

Przykładowe ARKit/UrhoSharp ładuje animowany znak z tekstury i odtwarzania animacji, o następującej implementacji:

    ```csharp
    public class MutantDemo : ArkitApp
    {
        [Preserve]
        public MutantDemo(ApplicationOptions opts) : base(opts) { }

        Node mutantNode;

        protected override void Start()
        {
            base.Start ();

            // Mutant
            mutantNode = Scene.CreateChild();
            mutantNode.Rotation = new Quaternion(x: 0, y:15, z:0);
            mutantNode.Position = new Vector3(0, -1f, 2f); /*two meters away*/
            mutantNode.SetScale(0.5f);

            var mutant = mutantNode.CreateComponent<AnimatedModel>();
            mutant.Model = ResourceCache.GetModel("Models/Mutant.mdl");
            mutant.Material = ResourceCache.GetMaterial("Materials/mutant_M.xml");

            var animation = mutantNode.CreateComponent<AnimationController>();
            animation.Play("Animations/Mutant_HipHop1.ani", 0, true, 0.2f);
        }
    }
    ```

Oraz że to naprawdę wszystkie opcje, które musisz wykonać na tym etapie, mieć 3D zawartość wyświetlana w rzeczywistości zwiększonej.

Urho używa niestandardowych formatów modeli 3D i animacji, więc należy wyeksportować zasobów w tym formacie.   Można użyć narzędzia, takie jak [dodatku mieszarce Urho3D](https://github.com/reattiva/Urho3D-Blender) i [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) który można przekonwertować te zasoby z popularnych formatach, takich jak DBX, DAE, OBJ, Blend, 3 Max do formatu wymaganego przez Urho.

Aby dowiedzieć się więcej na temat tworzenia aplikacji 3D przy użyciu Urho, odwiedź stronę [wprowadzenie do UrhoSharp](~/graphics-games/urhosharp/introduction.md) przewodnik.

## <a name="arkitapp-in-depth"></a>ArkitApp szczegółowo

> [!NOTE]
> Ta sekcja jest przeznaczona dla deweloperów, które chcesz dostosować domyślny czynności UrhoSharp i ARKit lub chcesz uzyskać lepszy wgląd w sposób działania integracji.   Nie jest konieczne do odczytu w tej sekcji.

Interfejs API ARKit jest dość proste, tworzenie i konfigurowanie [ARSession](https://developer.apple.com/documentation/arkit/arsession) obiekt, który, a następnie uruchomić dostarczania [ARFrame](https://developer.apple.com/documentation/arkit/arframe) obiektów.   Zawierają one obrazu przechwycone przez aparat, jak również szacowany pozycji rzeczywistych urządzenia.

Firma Microsoft będzie można tworzenia obrazów są dostarczane przez aparat do nas z naszej zawartości 3D i Dostosuj kamery w UrhoSharp odpowiadające ryzyko w lokalizacji urządzenia i pozycji.

Na poniższym diagramie przedstawiono, co ma miejsce `ArkitApp` klasy:

[![Diagram klas i ekrany ArkitApp](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>Renderowanie ramki

Koncepcja jest prosta, łączącą wystawała z kamerą naszych grafiki 3D można utworzyć połączonego obrazu wideo.     Firma Microsoft będzie występować szereg tych przechwyconych obrazów w sekwencji, a firma Microsoft będzie mieszać tych danych wejściowych z Urho sceny.

Najprostszym sposobem, aby wykonać to zadanie jest do wstawienia [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) w głównym [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/).  Jest to zestaw poleceń, które są wykonywane do rysowania jedną ramkę.  To polecenie spowoduje wypełnienie okienka ekranu z dowolnego tekstury, który jest przekazywana do niego.    Możemy to skonfigurować na pierwszej ramki, który to proces, a rzeczywista definicji odbywa się w tym **ARRenderPath.xml** pliku, który jest ładowany w tym momencie.

Jednak firma Microsoft występują dwa problemy, aby dopasować tych dwóch względem siebie:


1. W systemach iOS, tekstury procesora GPU musi mieć rozdzielczość, która jest potęgą liczby dwa, ale ramek, które uzyskujemy z aparatu fotograficznego nie ma rozwiązania, które są potęgami liczby dwa, na przykład: 1280 x 720.
2. Ramki są kodowane w [YUV](https://en.wikipedia.org/wiki/YUV) format reprezentowany przez dwa obrazy - luma i nasycenie.

Ramki YUV są dostępne w dwóch różnych rozdzielczości.  Obraz 1280 x 720 reprezentujący jasności (zasadniczo obrazu w odcieniach szarości) i znacznie mniejszy 640 x 360, składnika chrominance:

![Prezentacja łączenie Y i składniki UV obrazu](urhosharp-images/image3.png)


Rysowanie pełnego obrazu kolorowe przy użyciu OpenGL ES mamy zapisu małych programu do cieniowania, pobierającej jasności (składnik Y) i chrominance (UV płaszczyzny) z gniazda tekstury.  W UrhoSharp mają nazwy - "sDiffMap" i "sNormalMap" i przekonwertować je na RGB format:

```csharp
mat4 ycbcrToRGBTransform = mat4(
    vec4(+1.0000, +1.0000, +1.0000, +0.0000),
    vec4(+0.0000, -0.3441, +1.7720, +0.0000),
    vec4(+1.4020, -0.7141, +0.0000, +0.0000),
    vec4(-0.7010, +0.5291, -0.8860, +1.0000));

vec4 ycbcr = vec4(texture2D(sDiffMap, vTexCoord).r,
                    texture2D(sNormalMap, vTexCoord).ra, 1.0);
gl_FragColor = ycbcrToRGBTransform * ycbcr;
```

Do renderowania tekstury, który nie ma potęgą liczby dwa rozwiązania musimy zdefiniować Texture2D z następującymi parametrami:

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

W związku z tym możemy renderować obrazy przechwycone jako tło i renderowania tak jak przerażenie mutacji żadnych sceny powyżej.

### <a name="adjusting-the-camera"></a>Dopasowywanie aparatu

`ARFrame` Obiekty zawierają również pozycji szacowany urządzenia.  Teraz trzeba przenieść gier aparatu ARFrame — przed ARKit nie było hologramy transakcji big orientacji urządzenia Śledź (zbiorczego, wysokości i odchylenia) i renderowania przypiętych hologram na górze wideo -, ale jeśli urządzenie jest przeniesienie nieco - firma Microsoft będzie rozchodzenia.

Tak się stanie, ponieważ wbudowanych czujników, takich jak Żyroskop nie są w stanie śledzenia przemieszczania, mogą one tylko przyspieszenie.  Analizy ARKit każdej funkcji ramki i wyodrębnia punktów do śledzenia i w związku z tym jest w stanie Przekaż nam ścisłego przekształcenia macierzy zawierającego dane dotyczące przepływu i obrotu.

Na przykład jest to, jak można uzyskać bieżącego położenia:

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Używamy `-row.Z` ponieważ ARKit używany praworęczny układ współrzędnych.


### <a name="plane-detection"></a>Wykrywanie płaszczyzny

ARKit jest w stanie wykryć poziome płaszczyzny i ta możliwość pozwala na interakcję z rzeczywistych, na przykład, możemy umieścić mutacji rzeczywistych tabeli lub piętra. Jest najprostszym sposobem, aby to zrobić przy użyciu metody HitTest (raycasting). Konwertuje współrzędne ekranu (0,5; 0,5 jest Centrum) do współrzędnych świata rzeczywistego (0; 0; 0 jest lokalizacja pierwszej ramki).

```chsarp
protected Vector3? HitTest(float screenX = 0.5f, float screenY = 0.5f)
{
    var result = ARSession.CurrentFrame.HitTest(new CGPoint(screenX, screenY),
        ARHitTestResultType.ExistingPlaneUsingExtent)?.FirstOrDefault();
    if (result != null)
    {
        var row = result.WorldTransform.Row3;
        return new Vector3(row.X, row.Y, -row.Z);
    }
    return null;
}
```

Teraz możemy umieścić mutacji na płaszczyźnie poziomej w zależności od tego, gdzie na ekranie urządzenia, które możemy wybierz:

```chsarp
void OnTouchEnd(TouchEndEventArgs e)
{
    float x = e.X / (float)Graphics.Width;
    float y = e.Y / (float)Graphics.Height;
    var pos = HitTest(x, y);
    if (pos != null)
    mutantNode.Position = pos.Value;
}
```

![Rysunek animowany zmiana płaszczyzn jako przenosi widoku](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>Realistyczne oświetlenia

W zależności od rzeczywistych warunkach oświetlenia sceny wirtualnych powinna być jaśniejsze i ciemniejsze, aby lepiej dopasować jej otoczeniu. ARFrame zawiera właściwość LightEstimate, które firma Microsoft umożliwia dostosowanie światła otoczenia Urho odbywa się podobnie do następującej:


    var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
    var zone = Scene.GetComponent<Zone>();
    zone.AmbientColor = Color.White * ambientIntensity;


### <a name="beyond-ios---hololens"></a>Poza dla systemu iOS — HoloLens

UrhoSharp [działa we wszystkich najważniejszych systemach operacyjnych](~/graphics-games/urhosharp/platform/index.md), więc można wykorzystać istniejący kod w innym miejscu.

HoloLens jest jednym z najbardziej atrakcyjnych platformy, z którymi jest uruchamiany na.   Oznacza to, że można łatwo przełączać się między systemów iOS i HoloLens do tworzyć atrakcyjne aplikacje rozszerzony rzeczywistości przy użyciu UrhoSharp.

Można znaleźć źródła MutantDemo w [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo).


## <a name="related-links"></a>Linki pokrewne

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (z UrhoSharp) (przykład)](https://github.com/EgorBo/ARKitXamarinDemo)
