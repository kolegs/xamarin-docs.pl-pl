---
title: ARKit przy użyciu platformy UrhoSharp w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano sposób konfigurowania aplikacji ARKit w rozszerzeniu Xamarin.iOS, a następnie analizuje sposób renderowania ramek, jak dostosować aparatu, jak wykrywać płaszczyzn, jak pracować z udziału oświetlenia i nie tylko. Omówiono w nim również UrhoSharp i pisanie kodu dla HoloLens.
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/01/2017
ms.openlocfilehash: 728082eb27684c2176feb2038b7948986ce6a694
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351694"
---
# <a name="using-arkit-with-urhosharp-in-xamarinios"></a>ARKit przy użyciu platformy UrhoSharp w rozszerzeniu Xamarin.iOS

Wraz z wprowadzeniem [ARKit](https://developer.apple.com/arkit/), Apple wprowadził prosty deweloperom tworzyć aplikacje rzeczywistości rozszerzonej. ARKit mogą śledzić dokładnego położenia urządzenia i wykrywanie różnych powierzchniach na świecie, a następnie jest dla deweloperów, aby dopasować dane pochodzące z ARKit w kodzie.

[Na platformie UrhoSharp](~/graphics-games/urhosharp/index.md) zapewnia kompleksowe i łatwy w użyciu API 3D, który służy do tworzenia aplikacji 3D.   Oba te mogą być mieszane razem ARKit zapewnienie fizycznych informacji o świecie i Urho do renderowania wyniki.

Tej stronie wyjaśniamy sposób łączenia tych dwóch podejść ze sobą, aby utworzyć aplikacje doskonałe rzeczywistości rozszerzonej.


## <a name="the-basics"></a>Podstawowe informacje

Co chcemy zrobić jest obecny zawartości 3D na świecie, widziany przez telefon iPhone.   Chodzi o to, aby dopasować zawartość pochodzące z aparatu na telefonie z zawartością 3D i przemieszcza się użytkownik telefonu pomieszczeniu, aby upewnić się, że obiekt 3D zachowywać się one częścią tego miejsca — jest to realizowane przez Zakotwiczanie obiekty w tym świecie.

![Animowany cyfrze ARKit](urhosharp-images/image1.gif)


Firma Microsoft będzie używana biblioteka Urho ładować naszych zasobów 3D i umieścić je na świecie, a następnie użyjemy ARKit uzyskanie strumienia wideo pochodzące z aparatu, a także lokalizacji, telefonu, na całym świecie.   Gdy użytkownik przesuwa się za pomocą swojego telefonu, użyjemy zmiany w tym miejscu można zaktualizować współrzędnych Urho aparat są wyświetlane.

W ten sposób, gdy obiekt jest umieszczony w przestrzeni 3D i przenosi użytkownika, lokalizację obiektu 3D odzwierciedla wdrożona i lokalizację, w którym została umieszczona.

## <a name="setting-up-your-application"></a>Konfigurowanie aplikacji

### <a name="ios-application-launch"></a>Uruchamianie aplikacji systemu iOS

Aplikacja dla systemu iOS musi utworzyć i uruchomić zawartości 3D, możesz to zrobić, tworząc, implementowanie podklasą [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) i podanie kodu instalacji przez zastąpienie `Start` metody.  Jest to, gdzie jest wypełniana sceny z danymi, zdarzenie obsługi są instalacji i tak dalej.

Firma Microsoft wprowadza `Urho.ArkitApp` klasę tej podklasy `Urho.Application` i na jego `Start` metoda wykonuje ciężkie obciążenia.   Wszystko, czego potrzebujesz, aby zrobić do Twojego istniejącego Urho aplikacja jest Zmień klasę bazową typu `Urho.ArkitApp` i masz aplikację, która jest uruchamiana Twoja urho sceny w świecie.

### <a name="the-arkitapp-class"></a>Klasa ArkitApp

Ta klasa udostępnia zestaw wygodne ustawienia domyślne, zarówno sceny z niektórych kluczowych obiektów, a także przetwarzania zdarzeń ARKit, jaką są dostarczane przez system operacyjny.

Instalator ma miejsce w `Start` metodę wirtualną.   Gdy na Twojej podklasy zastąpienie tej metody, musisz upewnij się, że Twój rodzic łańcucha przy użyciu `base.Start()` na własnej implementacji.

`Start` Metody ustawia sceny, okienka ekranu, kamery i światła kierunkowego i udostępnia je jako właściwości publiczne:

- [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) do przechowywania obiektów,
- kierunkowego [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) cieni i którego lokalizacja jest dostępna za pośrednictwem `LightNode` właściwości
- [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) których elementy są aktualizowane w ramach ARKit dostarcza aktualizacji aplikacji i
- [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) wyświetlania wyników.


### <a name="your-code"></a>Twój kod

Następnie należy do podklasy `ArkitApp` klasy, a także Przesłoń `Start` metody.   Pierwszą rzeczą, które należy wykonać metodę, to do łańcucha `ArkitApp.Start` przez wywołanie metody `base.Start()`.  Po tym można użyć właściwości konfiguracji przez ArkitApp dodawania obiektów do sceny, dostosowywanie światła, cieniami lub zdarzenia, które mają być obsługiwane.

Przykładowe ARKit/UrhoSharp ładuje animowany znakiem tekstury i odtwarzania animacji, następującą implementacją:

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

I to naprawdę wszystkie opcje, które należy wykonać w tym momencie mieć zawartości 3D wyświetlony w rzeczywistości rozszerzonej.

Urho używa niestandardowych formatów modele 3D i animacji, więc należy wyeksportować zasobów do tego formatu.   Możesz użyć narzędzi, takich jak [dodatku pakietu Blender Urho3D](https://github.com/reattiva/Urho3D-Blender) i [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) , można przekonwertować te zasoby z najpopularniejszych formatów, takich jak DBX, DAE, OBJ, Blend, 3-w-Max do formatu wymaganego przez Urho.

Aby dowiedzieć się więcej na temat tworzenia aplikacji 3D przy użyciu Urho, odwiedź stronę [wprowadzenie do platformy UrhoSharp](~/graphics-games/urhosharp/introduction.md) przewodnik.

## <a name="arkitapp-in-depth"></a>ArkitApp szczegółowo

> [!NOTE]
> Ta sekcja jest przeznaczona dla deweloperów, które chcesz dostosować domyślne środowisko na platformie UrhoSharp i ARKit lub chcesz uzyskać lepszy wgląd w działania integracji.   Nie jest niezbędne do odczytu w tej sekcji.

Interfejs API ARKit jest dość prosty, należy utworzyć i skonfigurować [ARSession](https://developer.apple.com/documentation/arkit/arsession) obiekt, który, a następnie zacznij dostarczać [ARFrame](https://developer.apple.com/documentation/arkit/arframe) obiektów.   Zawierają one obraz przechwytywany przez aparat, a także szacowane położenie świata rzeczywistego urządzenia.

Firma Microsoft będzie można redagowania obrazy dostarczane przez aparat z nami za pomocą naszej zawartości 3D i dostosować kamery w platformie UrhoSharp, aby dopasować szanse w lokalizacji urządzenia i pozycji.

Na poniższym diagramie przedstawiono, co ma miejsce `ArkitApp` klasy:

[![Diagram klas i ekranów w ArkitApp](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>Renderowanie ramki

Koncepcja jest prosta, połącz wideo pochodzące z aparatu fotograficznego z naszych grafiką 3D, aby wygenerować połączonego obrazu.     Firma Microsoft będą się pojawiać szereg te obrazy przechwycone w sekwencji, a firma Microsoft będzie mieszać tych danych wejściowych z sceny Urho.

Jest najprostszym sposobem, aby to zrobić, można wstawić [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) w głównym [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/).  Jest to zestaw poleceń, które są wykonywane w celu rysowania jednej ramki.  To polecenie spowoduje wypełnienie okienka ekranu z wszelkich tekstur, które możemy przekazać do niego.    Możemy to skonfigurować dla pierwszej ramki, który jest procesem, a rzeczywistą definicją odbywa się w tym **ARRenderPath.xml** pliku, który jest ładowany w tym momencie.

Jednak firma Microsoft występują dwa problemy, aby dopasować tych dwóch rozwiązań razem:


1. W systemach iOS, tekstury procesora GPU musi mieć rozdzielczość, która jest potęgą liczby dwa, ale ramek, które można je uzyskać z aparatu fotograficznego ma rozwiązania, które są wartością potęgi liczby dwa, na przykład: 1280 x 720.
2. Ramki są kodowane w [YUV](https://en.wikipedia.org/wiki/YUV) formatu, reprezentowany przez dwa obrazy - luma i nasycenie.

Ramki YUV są dostępne w dwóch różnych rozdzielczościach.  Obraz 1280 x 720, reprezentujący jasności (zasadniczo obraz Skala szarości) i znacznie mniejszych 640 x 360, składnika chrominance:

![Obraz ukazujące łączenie Y i składniki UV](urhosharp-images/image3.png)


Aby narysować pełnego obrazu kolorowe przy użyciu OpenGL ES mamy zapisu małych modułem cieniującym, który przyjmuje jasności (składnik Y) i chrominance (UV płaszczyzny) z gniazda tekstury.  W platformie UrhoSharp w nazwach — "sDiffMap" i "sNormalMap" i przekonwertować je na RGB format:

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

Aby renderować teksturę, która nie ma potęgą liczby dwa rozwiązania mamy do definiowania Texture2D z następującymi parametrami:

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

Ten sposób możemy renderowanie przechwyconych obrazów jako tło i renderować żadnych sceny nad nim Krewny, który scary mutacji.

### <a name="adjusting-the-camera"></a>Dostosowywanie aparatu

`ARFrame` Obiekty zawierają także położenie szacowany urządzenia.  Teraz trzeba przenieść gry aparatu ARFrame — przed ARKit nie było hologramy wielkiego śledzenie orientacji urządzenia (wdrożenie, skoku i odchylenia) i renderowania przypiętych hologram na podstawie wideo —, ale należy przenieść urządzenie nieco — firma Microsoft będzie odstępstw.

Tak się stanie, ponieważ wbudowane czujniki, takie jak Żyroskop nie będą mogli śledzić przemieszczania, mogą oni przyspieszanie tylko.  Analizy ARKit każdej funkcji ramki i wyodrębnia punktów do śledzenia i dlatego jest w stanie największymi ścisłego Przekształć macierzy zawierającego przepływu i wymiany danych.

Na przykład jest to, jak można uzyskać bieżącego położenia:

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Używamy `-row.Z` ponieważ ARKit używa prawostronnego układu współrzędnych.


### <a name="plane-detection"></a>Wykrywanie płaszczyzna

ARKit jest w stanie wykryć poziome płaszczyzny i możesz wchodzić w interakcje ze światem rzeczywistym dzięki tej możliwości, na przykład możemy umieścić mutacji na rzeczywistych tabeli lub piętra. Najprostszym sposobem wykonania tego zadania jest hitTest — metoda (szczegóły technologii raycasting). Konwertuje współrzędne ekranu (0,5; o rozmiarze 0,5 to Centrum) na współrzędnych świata rzeczywistego (0; 0. 0 oznacza lokalizację pierwszej ramki).

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

Teraz możemy umieścić mutacji na płaszczyźnie poziomej zależności od tego, gdzie na ekranie urządzenia, które firma Microsoft naciśnij:

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

![Rysunek animowaną zmianę płaszczyzn jako przenosi widoku](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>Realistyczne oświetlenia

W zależności od rzeczywistych warunków oświetlenia sceny wirtualnych powinna być jaśniejsze i ciemniejszą, aby lepiej dopasować je do jego otoczenia. ARFrame zawiera właściwość LightEstimate możemy użyć, aby dopasować światła otoczenia Urho odbywa się podobnie do następującego:


    var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
    var zone = Scene.GetComponent<Zone>();
    zone.AmbientColor = Color.White * ambientIntensity;


### <a name="beyond-ios---hololens"></a>Poza dla systemu iOS — HoloLens

Na platformie UrhoSharp [działa we wszystkich najważniejszych systemach operacyjnych](~/graphics-games/urhosharp/platform/index.md), dzięki czemu można ponownie użyć istniejącego kodu gdzie indziej.

HoloLens jest jednym z najbardziej atrakcyjnych platform, których ono działa.   Oznacza to, że można łatwo przełączać się między dla systemów iOS i HoloLens tworzyć wspaniałe aplikacje rzeczywistości rozszerzonej, korzystanie z aparatu UrhoSharp.

Można znaleźć źródła MutantDemo w [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo).


## <a name="related-links"></a>Linki pokrewne

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (przy użyciu platformy UrhoSharp) (przykład)](https://github.com/EgorBo/ARKitXamarinDemo)
