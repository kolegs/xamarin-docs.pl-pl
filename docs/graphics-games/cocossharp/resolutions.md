---
title: Obsługa wielu rozwiązań w CocosSharp
description: W tym przewodniku pokazano, jak pracować z CocosSharp tworzenia gier, w których wyświetlać się poprawnie na urządzeniach różnej rozdzielczości.
ms.prod: xamarin
ms.assetid: 859ABF98-2646-431A-A4A8-3E7E48DA5A43
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 4077af2351b8ab3ef718a71cc672add54b6ef05a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="handling-multiple-resolutions-in-cocossharp"></a>Obsługa wielu rozwiązań w CocosSharp

_W tym przewodniku pokazano, jak pracować z CocosSharp tworzenia gier, w których wyświetlać się poprawnie na urządzeniach różnej rozdzielczości._

CocosSharp udostępnia metody dla standaryzacji wymiary obiektu w grze niezależnie od fizycznej liczba pikseli na ekranie urządzenia. Tradycyjnie gry opracowanych dla komputerów i konsole do gier przeznaczona jednego rozwiązania. Nowoczesne gry — i szczególnie gry dla urządzeń przenośnych — muszą zostać skompilowane aby zmieścił się w szerokiej gamy urządzeń. W tym przewodniku pokazano, jak normalizacji CocosSharp gry niezależnie od właściwości rozwiązania wyświetlania fizycznej.

Domyślne zachowanie rozpoznawania CocosSharp jest pasuje współrzędnych w grze w pikselach fizycznych. W poniższej tabeli przedstawiono, jak różnych urządzeń będzie renderować sprite środowiska tła o szerokości i wysokości 368 x 240. Pierwszy wiersz jest technicznie nie rzeczywistego urządzenia, ale zamiast oczekiwanego renderowania ikonki, niezależnie od rozdzielczość urządzenia:


| **Urządzenia** | **Rozdzielczość ekranu** | **Zrzut ekranu** |
|--- | --- |--- |
|Żądane|368 x 240 (z czarnym paski współczynnik proporcji)| ![368 x 240 (z czarnym paski współczynnik proporcji)](resolutions-images/image1.png) |
|iPhone 4s|960x640| ![iPhone 4s 960 x 640](resolutions-images/image2.png) |
|iPhone 6 Plus|1920x1080| ![iPhone 6 Plus 1920 x 1080 pikseli](resolutions-images/image3.png) |

W tym dokumencie opisano sposób użycia CocosSharp, aby rozwiązać ten problem, przedstawione w powyższej tabeli. Oznacza to, że firma Microsoft będzie opisano, jak wprowadzić dowolne urządzenie renderowania, jak pokazano w pierwszym wierszu — niezależnie od rozdzielczości ekranu.


## <a name="working-with-setdesignresolutionsize"></a>Praca z SetDesignResolutionSize

`CCScene` Klasa zwykle jest używana jako kontener główny dla wszystkich obiektów visual, ale również zapewnia metody statycznej o nazwie `SetDesignResolutionSize` służący do określania domyślny rozmiar wszystkie sceny. Innymi słowy `SetDesignResolutionSize` metody umożliwia deweloperom tworzenie gier, które będą wyświetlane poprawnie w różnych rozdzielczości sprzętu. Szablony projektów CocosSharp używać tej metody można ustawić domyślny rozmiar projektu do 1024 x 768, jak to pokazano w poniższym kodzie:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");
    CCSize windowSize = mainWindow.WindowSizeInPixels;
    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```

Możesz zmienić `desiredWidth` i `desiredHeight` zmienne powyżej, aby zmodyfikować widok, aby odpowiadała rozdzielczości odpowiednią grę. Na przykład powyższy kod mogły zostać zmodyfikowane w następujący sposób do wyświetlenia w tle 368 x 240 jako pełny ekran:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");
    CCSize windowSize = mainWindow.WindowSizeInPixels;
    float desiredWidth = 368.0f;
    float desiredHeight = 240.0f;
    
    // This will set the world bounds to(0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```


## <a name="ccsceneresolutionpolicy"></a>CCSceneResolutionPolicy

`SetDesignResolutionSize` Pozwala określić sposób dostosowania okna do gier do żądanej rozdzielczości. Poniższe sekcje pokazują sposób wyświetlania obrazu 500 x 500 z różnymi `CCSceneResolutonPolicy` wartości przekazanych do `SetDesignResolutionSize` metody. Następujące wartości są dostarczane przez `CCSceneResolutionPolicy` wyliczenia:

 - `ShowAll` — Wyświetla całą rozpoznawania żądanej zachowaniu współczynnika proporcji.
 - `ExactFit` — Wyświetla całą rozpoznawania żądanej, ale nie zachowuje współczynnik proporcji.
 - `FixedWidth` — Wyświetla całą szerokość żądanego zachowaniu współczynnika proporcji.
 - `FixedHeight` — Wyświetla całą wysokość żądanego zachowaniu współczynnika proporcji.
 - `NoBorder` — Wyświetla całą wysokość albo całą szerokość zachowaniu współczynnika proporcji. Jeśli współczynnik proporcji urządzenia jest większa niż współczynnik proporcji pożądaną rozdzielczość, całą szerokość jest wyświetlany. Jeśli współczynnik proporcji urządzenia jest mniejsza niż współczynnik proporcji pożądaną rozdzielczość, wysokość całego jest wyświetlany.
 - `Custom` — Zależy od wyświetlanie `Viewport` właściwości bieżącego `CCScene`.

Wszystkie zrzuty ekranu są tworzone w iPhone 4s rozdzielczości (960 x 640) w orientacji poziomej i skorzystaj z tego obrazu:

![](resolutions-images/image4.png "Wszystkie zrzuty ekranu są produkowane iPhone 4s rozdzielczością 960 x 640 w orientacji poziomej i skorzystaj z tego obrazu")


### <a name="ccsceneresolutionpolicyshowall"></a>CCSceneResolutionPolicy.ShowAll

`ShowAll` Określa całego rozwiązania gry będą widoczne na ekranie, ale może być wyświetlany *letterboxing* (czarne paski), aby dostosować ją do różnych współczynników proporcji. Ta zasada używa się zazwyczaj gwarantuje całego widoku gier zostanie wyświetlone na ekranie bez zakłóceń.

Przykład kodu:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Letterboxing jest widoczny w lewo i prawo obrazu dla fizycznego współczynnik proporcji jest szerszy niż żądany rozwiązania:

![](resolutions-images/image5.png "Letterboxing jest widoczny w lewo i prawo obrazu dla fizycznego współczynnik proporcji jest szerszy niż żądany rozwiązania")


### <a name="ccsceneresolutionpolicyexactfit"></a>CCSceneResolutionPolicy.ExactFit

`ExactFit` Określa, że cały rozpoznawania gry będą widoczne na ekranie z nie letterboxing. Może być zniekształcony widocznego obszaru (współczynnik proporcji może się nie powieść) zgodnie ze sprzętu współczynnik proporcji.

Przykład kodu:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ExactFit);
```

Brak letterboxing nie jest widoczna, ale ponieważ rozdzielczość urządzenia to prostokątne jest zniekształcony gier widoku:

![](resolutions-images/image6.png "Brak letterboxing nie jest widoczna, ale ponieważ rozdzielczość urządzenia to prostokątne gier widoku jest zniekształcony")


### <a name="ccsceneresolutionpolicyfixedwidth"></a>CCSceneResolutionPolicy.FixedWidth

`FixedWidth` Określa, czy szerokość widok będzie zgodna z wartością szerokości, przekazany do `SetDesignResolutionSize`, ale widoczne wysokość podlega współczynnik proporcji urządzenia fizycznego. Wartość wysokości przekazany do `SetDesignResolutionSize` jest ignorowany, ponieważ zostanie obliczona w czasie wykonywania na podstawie współczynnika proporcji urządzenia fizycznego. Oznacza to, że obliczeniowej wysokość może być mniejszy od żądaną wysokość (co powoduje części widoku gier trwa ukrytej) lub obliczeniowych wysokość może być większa niż żądaną wysokość (co powoduje więcej gier widoku wyświetlane). Ponieważ może to spowodować więcej gry będzie wyświetlany, następnie może okazać się tak, jakby wystąpił letterboxing; jednak dodatkowe miejsce nie musi będzie czarny, jeśli zostanie wyświetlone dowolnego obiektu visual. 

Przykład kodu:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedWidth);
```

IPhone 4s ma współczynnik proporcji: 2, 3, więc obliczeniowej wysokość jest około 333 jednostki:

![](resolutions-images/image7.png "IPhone 4s ma współczynnik proporcji: 2, 3, więc obliczeniowej wysokość wynosi około 333 jednostki")


### <a name="ccsceneresolutionpolicyfixedheight"></a>CCSceneResolutionPolicy.FixedHeight

Koncepcyjnie `FixedHeight` działa podobnie do `FixedWidth` — gry będzie przestrzegać wartość wysokości przekazany do `SetDesignResolutionSize,` , ale będzie obliczać szerokość w czasie wykonywania na podstawie rozdzielczości fizycznej. Jak wspomniano powyżej, oznacza to, że wyświetlane szerokość być mniejsza lub większa od żądaną szerokość, co w części gier są wyłączone ekranu lub więcej gry są wyświetlane odpowiednio.

Przykład kodu:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

Od chwili utworzenia Poniższy zrzut ekranu z aplikacji działających w trybie krajobraz, szerokość obliczeniowego jest większy niż 500 — w szczególności 750. Te zasady przechowuje wartość X 0 wyrównane do lewej, więc dodatkowe rozwiązanie będzie widoczny po prawej stronie ekranu.

![](resolutions-images/image8.png "Te zasady przechowuje wartość X 0 wyrównane do lewej, więc dodatkowe rozwiązanie będzie widoczny po prawej stronie ekranu")


### <a name="ccsceneresolutionpolicynoborder"></a>CCSceneResolutionPolicy.NoBorder

`NoBorder` próby wyświetlenia aplikacji z nie letterboxing, zachowując jego oryginalny współczynnik proporcji (zakłócenie). Jeśli współczynnik proporcji rozpoznawania żądanej zgodne urządzenia fizycznego współczynnik proporcji, wycinka nie zostanie przeprowadzona. Jeśli proporcje nie są zgodne, następnie wycinka zostanie przeprowadzona.

Przykład kodu:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

Poniższy zrzut ekranu przedstawia górnej i dolnej części wyświetlania przycinana, gdy są wyświetlane wszystkie 500 pikseli szerokości ekranu:

![](resolutions-images/image9.png "Ten zrzut ekranu przedstawia górnej i dolnej części wyświetlania przycinana, gdy są wyświetlane wszystkie 500 pikseli szerokości ekranu")


### <a name="ccsceneresolutionpolicycustom"></a>CCSceneResolutionPolicy.Custom

`Custom` Włącza każdego `CCScene` do określenia własnego niestandardowego okienka ekranu względem rozpoznawania określone w `SetDesignResolutionSize`.

Zdefiniuj sceny ich `Viewport` właściwości tworząc `CCViewport`, który z kolei składa się z `CCRect`. Aby uzyskać więcej informacji na temat `CCViewport` i `CCRect` zobacz [dokumentacji interfejsu API CocosSharp](https://developer.xamarin.com/api/namespace/CocosSharp/). W przypadku tworzenia `CCViewport` `CCRect` konstruktora parametry określają (w kolejności):

 - x — wielkość na poziomie przesunięcie okienka ekranu, gdzie każda jednostka reprezentuje jeden szerokość cały ekran. Na przykład za pomocą wartości .5f wyników na scenie spowoduje przesunięcie w prawo o połowę szerokości ekranu.
 - y — kwota przesunięcie w pionie okienka ekranu, gdzie każda jednostka reprezentuje jeden wysokość cały ekran. Na przykład za pomocą wartości .5f wyników na scenie przesunięty w o połowę wysokość ekranu.
 - szerokość — poziome fragment, który będzie zajmować sceny. Na przykład za pomocą wartość 1 / 3.0f powoduje sceny poziomie zajmujące jedna trzecia ekranu.
 - wysokość — fragment pionowy, który będzie zajmować sceny. Na przykład za pomocą wartość 1 / 3.0f powoduje sceny pionowo zajmujące jedna trzecia ekranu.

Przykład kodu:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.Custom);
...
float horizontalDisplayPortion = 2 / 3.0f;
float verticalDisplayPortion = 1 / 2.0f;
float displayOffsetInScreenWidths = 0;
float displayOffsetInScreenHeights = .5f;
var rectangle = new CCRect (
    displayOffsetInScreenWidths, 
    displayOffsetInScreenHeights, 
    horizontalDisplayPortion, 
    verticalDisplayPortion);
scene.Viewport = new CCViewport (rectangle);
```

Powyższy kod wyniki będą następujące:

![](resolutions-images/image10.png "Powyższy kod wynikiem tego zrzutu ekranu")


## <a name="defaulttexeltocontentsizeratio"></a>DefaultTexelToContentSizeRatio

`DefaultTexelToContentSizeRatio` Upraszcza przy użyciu tekstury wyższej rozdzielczości na urządzeniach za pomocą ekrany o wyższej rozdzielczości. W szczególności ta właściwość umożliwia gry na potrzeby wyższej rozdzielczości zasobów bez konieczności zmiany rozmiaru lub położenia elementów wizualnych. 

Na przykład gry może zawierać zasób grafikę gier znak, który jest wysokości 200 pikseli i gier ekranu (zgodnie z określonym `SetDesignResolutionSize`) może być wysokości 400 pikseli. W takim przypadku znak zajmie połowę ekranu. Jednak jeśli zasób 400 pikseli wysoka użyto znaku dla urządzeń z wyższej rozdzielczości, znak zajmują cały wysokość ekranu. Jeśli chcesz zachować znak ten sam rozmiar, aby móc korzystać z wyższej rozdzielczości urządzenia, niektóre dodatkowe kod jest muszą być sprite znak w wysokości połowy ekranu.

Prosty przykład przedstawionych powyżej reprezentuje to powszechny problem w opracowywaniu gier — Dodawanie zasobów o większym rozmiarze bez konieczności developer do wykonania, zmiana rozmiaru na każdy element wizualny, zgodnie z rozdzielczość urządzenia. `DefaultTexelToContentSizeRatio` Właściwość globalna służy do zmiany rozmiaru elementów wizualnych. Ta wartość zmienia rozmiar wszystkich elementów wizualnych określonego typu przypisanej wartości.

Ta właściwość znajduje się w następujących klas: 

 - `CCApplication`
 - `CCSprite`
 - `CCLabelTtf`
 - `CCLabelBMFont`
 - `CCTMXLayer`

CocosSharp programu Visual Studio for Mac szablony ustawienie `CCSprite.DefaultTexelToContentSizeRatio` zgodnie z szerokość urządzenia, na przykład jak mogą być używane. Poniższy kod znajduje się w `CCApplicationDelegate.ApplicationDidFinishLaunching`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
           
    ...
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }
    ...           
}
```


### <a name="defaulttexeltocontentsizeratio-example"></a>Przykład DefaultTexelToContentSizeRatio

Aby zobaczyć, jak `DefaultTexelToContentSizeRatio` ma wpływ na rozmiar visual elementów, należy wziąć pod uwagę kod przedstawiony powyżej:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Czego skutkiem będzie na poniższej ilustracji, używanie tekstury 500 x 500:

![](resolutions-images/image5.png "Co spowoduje ten obraz przy użyciu tekstury 500 x 500")

Tabletów iPad siatkówki działa z rozdzielczością fizycznych 2048 x 1536. Oznacza to, gier, jak wyświetlone powyżej czy stretch 500 pikseli w pikselach fizycznych 1536. Gry można korzystać z tego dodatkowe rozwiązania przez utworzenie co to są zwykle nazywane *hd* zasoby — zasoby, które są przeznaczone do uruchamiania w ekrany o wyższej rozdzielczości. Na przykład gry można użyć wersji hd to tekstury o rozmiarze na 1000 x 1000. Jednak za pomocą większego obrazu spowoduje powstanie `CCSprite` o szerokości i wysokości 1000 jednostek. Ponieważ naszych gry zawsze wyświetlane 500 jednostek (ze względu na `SetDesignResolutioSize` wywołać), naszych sprite 1000 x 1000 jest za duży (wyświetla tylko lewej dolnej części z nich), a następnie:

![](resolutions-images/image11.png "Ponieważ gry zawsze wyświetlane 500 jednostek ze względu na wywołania SetDesignResolutioSize, sprite 1000 x 1000 będzie zbyt duży, wyświetla tylko lewej dolnej części z nich")

Firma Microsoft może ustawić `CCSprite.DefaultTexelToContentSizeRatio` dla tekstury 1000 x 1000 jest dwukrotnie jako szerokość lub wysokość jako oryginalnego tekstury 500 x 500:


```csharp
CCSprite.DefaultTexelToContentSizeRatio = 2;
```

Obecnie Jeśli przeprowadzana gry tekstury 1000 x 1000 pełni widoczne będą:

![](resolutions-images/image12.png "Teraz możemy uruchomienia gry tekstury 1000 x 1000 będą widoczne w pełni")


### <a name="defaulttexeltocontentsizeratio-details"></a>DefaultTexelToContentSizeRatio details

`DefaultTexelToContentSizeRatio` Jest właściwość `static,` co oznacza, że wszystkie ikony w aplikacji będą mieć takiej samej wartości. Typowa metoda gry z zasobów wprowadzone dla różnych rozdzielczości jest zawierają pełny zestaw zasobów dla każdej kategorii rozwiązania. Domyślnie CocosSharp programu Visual Studio dla szablonów Mac podaj **ld** i **hd** folderów dla zasobów, które mogłyby być przydatne w przypadku gry obsługi dwóch zestawów tekstury. Przykładowy folder zawartości z zawartością może wyglądać tak:

![](resolutions-images/image13.png "Przykładowy folder zawartości z zawartością może wyglądać jak")

Powiadomienie, że szablon domyślny zawiera również kod, aby dodać warunkowo w aplikacji `ContentSearchPaths`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }
    ...           
}
```

Oznacza to, że aplikacja będzie wyszukiwania plików w ścieżkach w zależności od tego, czy działa w trybie regularne rozwiązania lub wysokiej rozdzielczości. Na przykład jeśli **hd** i **ld** folderów zawierać plik o nazwie **background.png** , a następnie następujący kod będzie uruchomić i wybrać poprawny plik rozwiązania:


```csharp
backgroundSprite  = new CCSprite ("background");
```


## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób tworzenia gier, które są wyświetlane nieprawidłowo, niezależnie od rozdzielczości. Przedstawiono przykłady użycia w różnych `CCSceneResolutionPolicy` wartości do zmiany rozmiaru gry zgodnie z rozdzielczość urządzenia. Zapewnia także przykładowy sposób `DefaultTexelToContentSizeRatio` może służyć do uwzględnienia wielu zestawów zawartości bez konieczności elementy wizualne, można zmienić indywidualnie.

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie CocosSharp](~/graphics-games/cocossharp/index.md)
- [Dokumentacja interfejsu API CocosSharp](https://developer.xamarin.com/api/namespace/CocosSharp/)
