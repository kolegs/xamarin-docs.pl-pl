---
title: Animacja przy CCAction
description: Klasa CCAction ułatwia dodawanie animacji do gier CocosSharp. Te animacji może służyć do implementowania lub dodać Polski.
ms.topic: article
ms.prod: xamarin
ms.assetid: 74DBD02A-6F10-4104-A61B-08CB49B733FB
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 7e64789f4e86dbcd47fc760fd9d4d7fb61c76121
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2018
---
# <a name="animating-with-ccaction"></a>Animacja przy CCAction

_Klasa CCAction ułatwia dodawanie animacji do gier CocosSharp. Te animacji może służyć do implementowania lub dodać Polski._

`CCAction` jest klasą podstawową, która może służyć do animowania CocosSharp obiektów. Ten przewodnik obejmuje wbudowane `CCAction` implementacji dla typowych zadań, takich jak rozmieszczania, skalowanie i obrót. Prawdopodobnie również w sposób tworzenia niestandardowych implementacji przez dziedziczenie z `CCAction`.

W tym przewodniku używa projektu o nazwie **ActionProject** którego [można pobrać tutaj](https://developer.xamarin.com/samples/mobile/CCAction). W tym przewodniku używa `CCDrawNode` klasy, która została opisana w [geometrii rysunku z CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md) przewodnik.


## <a name="running-the-actionproject"></a>Uruchomiona ActionProject

**ActionProject** to rozwiązanie CocosSharp, które mogą być wbudowane w systemy iOS i Android. Służy ona zarówno jako przykładowy kod, jak używać `CCAction` klasy jak w czasie rzeczywistym pokaz wspólnych `CCAction` implementacji.

Podczas uruchamiania, ActionProject wyświetla trzy `CCLabel` wystąpień po lewej stronie ekranu i obiekt wizualny rysowane przez dwa `CCDrawNode` wystąpień do wyświetlania różnych działań:

![](ccaction-images/image1.png "ActionProject Wyświetla wystąpienia CCLabel po lewej stronie ekranu i obiekt wizualny narysowanymi przez dwa wystąpienia CCDrawNode do wyświetlania różnych działań")

Etykiety po lewej stronie wskazywać typu `CCAction` zostanie utworzony podczas wybierania na ekranie. Domyślnie **pozycji** wybrano wartość, co powoduje `CCMove` akcji trwa utworzą i zastosują się czerwone kółko:

![](ccaction-images/image2.gif "Wartość pozycji jest zaznaczone, co jest akcja CCMove utworzą i zastosują się czerwone kółko")

Klikając pozycję etykiety po lewej stronie zmiany typu `CCAction` odbywa się na okręgu. Na przykład kliknięcie **pozycji** etykiety może mieć różne wartości, które można zmienić:

![](ccaction-images/image3.gif "Kliknięcie pozycji etykiety może mieć różne wartości, które mogą być zmieniane")

## <a name="common-variable-changing-ccaction-classes"></a>Klasy wspólnych zmiana zmiennej CCAction

**ActionProject** są używane następujące `CCAction`-dziedziczenia klasy, które są częścią CocosSharp:

 - `CCMoveTo` — Zmienia `CCNode` wystąpienia `Position` właściwości
 - `CCScaleTo` — Zmienia `CCNode` wystąpienia `Scale` właściwości
 - `CCRotateTo` — Zmienia `CCNode` wystąpienia `Rotation` właściwości

Ten przewodnik odwołuje się do tych działań jako *zmiana zmiennej*, co oznacza, że wpływ bezpośrednio zmiennej `CCNode` dodawane do. Inne typy akcji są określane jako *napięcia* działania, które zostały omówione w dalszej części tego przewodnika.

**ActionProject** pokazuje, że tych działań ma na celu zmodyfikować zmienną w czasie. W szczególności te `CCActions` konstruktorów podjąć dwa argumenty: długość czasu i wartość do przypisania. Następujący fragment kodu przedstawia, jak są tworzone trzy typy działań:


```csharp
switch (VariableOptions [currentVariableIndex])
{
    case "Position":
        coreAction = new CCMoveTo(timeToTake, touch.Location);

        break;
    case "Scale":
        var distance = CCPoint.Distance (touch.Location, drawNodeRoot.Position);
        var desiredScale = distance / DefaultCircleRadius;
        coreAction = new CCScaleTo(timeToTake, desiredScale);

        break;
    case "Rotation":
        float differenceY = touch.Location.Y - drawNodeRoot.PositionY;
        float differenceX = touch.Location.X - drawNodeRoot.PositionX;

        float angleInDegrees = -1 * CCMathHelper.ToDegrees(
            (float)System.Math.Atan2(differenceY, differenceX));

        coreAction = new CCRotateTo (timeToTake, angleInDegrees);

        break; 
...
}
```

Po utworzeniu akcji dodaniu do CCNode w następujący sposób:


```csharp
nodeToAddTo.AddAction (coreAction); 
```

`AddAction` Uruchamia `CCAction` zachowanie instancji i automatycznie zostanie przeprowadzone jej logiki po klatce aż do zakończenia.

Każdy z typów wymienionych powyżej kończy się wyrazem *do* co oznacza, że `CCAction` zmodyfikuje `CCNode` tak, że wartość argumentu reprezentuje stan końcowy po zakończeniu działania. Na przykład tworzenie `CCMoveTo` pozycji X = 100 i Y = 200 powoduje `CCNode` wystąpienia `Position` ustawiany X = 100, Y = 200 na końcu określonej, niezależnie od tego czasu `CCNode` wystąpienia obiektu lokalizacji początkowej.

Każdy "Do" klasy "Przez" wersji, co spowoduje dodanie do bieżącej wartości wartość argumentu na ma również `CCNode`. Na przykład tworzenie `CCMoveBy` pozycji X = 100 i Y = 200 spowoduje `CCNode` wystąpienia jest przenoszony do prawej 100 jednostek i 200 jednostek z pozycji było w uruchomienia akcji.


## <a name="easing-actions"></a>Akcje sterowania tempem zmian

Domyślnie będzie wykonywać akcje zmiana zmiennej *interpolacji liniowej* — akcja zostanie przesunięty na żądaną wartość stałą szybkością. Jeśli interpolacji *pozycji* liniowo, przenoszenie obiektów zostanie natychmiast uruchomić i zatrzymać, przeniesienie na początku i na końcu akcji i szybko pozostaną stałej jako wykonuje akcję. 

Interpolacji liniowej nie jest mniej jarring i dodaje element Polski, więc CocosSharp oferuje różne napięcia akcje, które mogą być używane do modyfikowania zmiana zmiennej akcji.

W **ActionProject** próbki, firma Microsoft można przełączać się między te typy akcji sterowania tempem zmian, klikając etykietę drugiego (jakie nie **<None>**):

![](ccaction-images/image4.gif "Użytkownika można przełączać się między te typy akcji sterowania tempem zmian, klikając etykietę drugi")

Akcje sterowania tempem zmian są szczególnie wydajne, ponieważ nie są związane z dowolnej określonej akcji ustawienie zmiennej. Oznacza to, że ta sama akcja sterowania tempem zmian może służyć do przypisywania pozycji, obracanie, skala lub akcje niestandardowe (jak będą wyświetlane w dalszej części tego przewodnika).

Akcje dynamiki zawijać akcje ustawienie zmiennej (tak długo, jak ustawienie zmiennej akcji dziedziczy `CCFiniteTimeAction`) akceptując akcję ustawienie zmiennej jako argumentu w ich konstruktorów.

Na przykład, jeśli etykiety są ustawione na **pozycji**, **CCEaseElastic**, a następnie następujący kod zostanie wykonany po wykryciu touch (Zauważ, że kod została pominięta, aby wyróżnić odpowiednich wierszy):


```csharp
CCFiniteTimeAction coreAction = null; 
...
coreAction = new CCMoveTo(timeToTake, touch.Location); 
...
CCAction easing = null; 
...
easing = new CCEaseSineOut (coreAction); 
...
nodeToAddTo.AddAction (easing); 
```

Przedstawiony przez aplikację, dokładne złagodzenie tego samego można zastosować do innych działań ustawienie zmiennej takich jak `CCRotateTo`:

![](ccaction-images/image5.gif "Dokładne złagodzenie tego samego można zastosować do innych ustawienie zmiennej akcji, takich jak CCRotateTo")


## <a name="easing-in-out-and-inout"></a>Ułatwianie In, Out i InOut

Wszystkie akcje dynamiki ma `In`, `Out`, lub `InOut` dołączany do sterowania tempem zmian typu. Te warunki dotyczą po zastosowaniu napięcia: `In` oznacza łatwiejszym zostaną zastosowane na początku, `Out` oznacza, że przed upływem i `InOut` oznacza zarówno na początku i na końcu.

`In` Łatwiejszym akcja będzie miało wpływ na sposób zmiennej jest stosowana w całej interpolacji, (zarówno na początku i na końcu), ale zazwyczaj najbardziej właściwości sterowania tempem zmian akcji zostanie przeprowadzone na początku. Podobnie `Out` dynamiki akcje charakteryzują się ich zachowania w końcu interpolacji. Na przykład `CCEaseBounceOut` spowoduje obiektu odbijania na końcu akcji.


### <a name="out"></a>Out

`Out` ułatwianie zazwyczaj stosuje się najbardziej zauważalne zmiany na końcu interpolacji. Na przykład `CCEaseExponentialOut` spowolni szybkość zmian zmiana zmiennej zbliża się wartość docelowa:

![](ccaction-images/image6.gif "CCEaseExponentialOut spowolni szybkość zmian zmiana zmiennej zbliża się wartość docelowa")


### <a name="in"></a>W

`In` ułatwianie zazwyczaj stosuje się najbardziej zauważalne zmiany na początku interpolacji. Na przykład `CCEaseExponentialIn` przeniesie wolniej na początku akcji:

![](ccaction-images/image7.gif "CCEaseExponentialIn przeniesie wolniej na początku akcji")


### <a name="inout"></a>InOut

`InOut` zazwyczaj stosuje się najbardziej zauważalne zmiany zarówno na początku i na końcu. `InOut` ułatwianie jest zwykle symetrycznego. Na przykład `CCEaseExponentialInOut` przeniesie powoli na początku i na końcu akcji:

![](ccaction-images/image8.gif "CCEaseExponentialInOut przeniesie powoli na początku i na końcu akcji")


## <a name="implementing-a-custom-ccaction"></a>Implementowanie niestandardowego CCAction

Wszystkie klasy, które zostały omówione wykonanej do tej pory zostaną uwzględnione w CocosSharp zapewnienie często używane funkcje. Niestandardowe `CCAction` implementacje zapewniają większą elastyczność. Na przykład `CCAction` która kontroluje stosunek wypełnienia paska środowisko można tak, aby na pasku środowisko rozwoju sprawnie zawsze, gdy użytkownik uzyskuje środowisko.

`CCAction` implementacje zwykle wymagają dwie klasy:

 - `CCFiniteTimeAction` Implementacja — klasa akcji skończonego czasu jest odpowiedzialna za uruchamianie akcji. Jest klasa, która zostanie uruchomiony i albo dodać bezpośrednio do `CCNode` lub do sterowania tempem zmian akcji. Utwórz wystąpienie i musi zwracać `CCFiniteTimeActionState`, który przeprowadzi aktualizacji.
 - `CCFiniteTimeActionState` Implementacja — klasa stanu akcji skończonego czasu jest odpowiedzialny za aktualizację zmienne objętego akcji. Musi on implementować funkcję aktualizacji, która przypisuje wartość w celu zgodnie z wartości typu time. Ta klasa nie ma jawnego odwołania do poza `CCFiniteTimeAction` co powoduje. Po prostu działa "w tle".

**ActionProject** zawiera niestandardowy `CCFiniteTimeAction` implementacja wywołuje `LineWidthAction,` używany do szerokość linii żółty rysowane na górze czerwonym kółku. Należy zauważyć, że jego tylko zadania do uruchomienia, a następnie wróć `LineWidthState` wystąpienie:


```csharp
public class LineWidthAction : CCFiniteTimeAction
{
    float endWidth;

    public LineWidthAction (float duration, float width) : base(duration)
    {
        endWidth = width;
    }

    public override CCFiniteTimeAction Reverse ()
    {
        throw new NotImplementedException ();
    }

    protected override CCActionState StartAction (CCNode target)
    {
        return new LineWidthState (this, target, endWidth);
    }
}
```

Jak wspomniano powyżej, `LineWidthState` wykonuje pracę przypisywanie linii `Width` ile właściwość `time` przeszło:


```csharp
public class LineWidthState : CCFiniteTimeActionState
{
    float deltaWidth;
    float startWidth;

    LineNode castedTarget;

    public LineWidthState(LineWidthAction action, CCNode target, float endWidth) : base(action, target)
    {
        castedTarget = target as LineNode;

        if (castedTarget == null)
        {
            throw new InvalidOperationException ("The argument target must be a LineNode");
        }

        startWidth = castedTarget.Width;
        deltaWidth = endWidth - startWidth;
    }

    public override void Update (float time)
    {
        castedTarget.Width = startWidth + deltaWidth * time;
    }
} 
```

LineWidthAction można łączyć z dowolnych sterowania tempem zmian akcji, aby zmienić szerokość linii na różne sposoby, jak pokazano w poniższej animacji:

![](ccaction-images/image9.gif "LineWidthAction można łączyć z dowolnych sterowania tempem zmian akcji, aby zmienić szerokość linii na różne sposoby, jak pokazano w tym animacji")


### <a name="interpolation-and-the-update-method"></a>Interpolacji i metody aktualizacji

Tylko logiki, jako uzupełnienie przechowywanie wartości w klasach powyżej, znajduje się `LineWidthState.Update` metody. `startWidth` Zmienna przechowuje szerokość elementu docelowego `LineNode` na początku tej akcji, a `deltaWidth` zmienna przechowuje wartość ile zmieni się w trakcie działania.

Podstawiając `time` zmiennej o wartości 0, zobaczysz, że element docelowy `LineNode` będzie w położeniu początkowy:


```csharp
castedTarget.Width = startWidth + deltaWidth * 0; 
```

Podobnie, zobaczysz, że element docelowy `LineNode` będzie w jego lokalizacji docelowej, wstawiając zmienną czasu o wartości 1:


```csharp
castedTarget.Width = startWidth + deltaWidth * 1; 
```

`time` Zazwyczaj zostanie użyta wartość pomiędzy 0 a 1 -, ale nie zawsze - i `Update` implementacji nie należy zakładać tych granic. W przypadku niektórych metod sterowania tempem zmian (takich jak `CCEaseBackIn` i `CCEaseBackOut`) zapewnia wartość godziny poza zakresem od 0 do 1.


## <a name="conclusion"></a>Wniosek

Interpolacji i łatwiejszym jest krytyczną częścią tworzenia dopracowany gier, szczególnie w przypadku, gdy tworzenie interfejsów użytkownika. W tym przewodniku przedstawiono sposób użycia `CCActions` do interpolacji wartości domyślnych, takich jak pozycja i obrotu, a także wartości niestandardowych. `LineWidthState` i `LineWidthAction` klasy przedstawiają sposób wykonania akcji niestandardowej.

## <a name="related-links"></a>Linki pokrewne

- [CCAction](https://developer.xamarin.com/api/type/CocosSharp.CCAction)
- [CCMoveTo](https://developer.xamarin.com/api/type/CocosSharp.CCMoveTo)
- [CCScaleTo](https://developer.xamarin.com/api/type/CocosSharp.CCScaleTo)
- [CCRotateTo](https://developer.xamarin.com/api/type/CocosSharp.CCRotateTo)
- [CCDrawNode](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode)
- [Pełny przykład](https://developer.xamarin.com/samples/mobile/CCAction/)
