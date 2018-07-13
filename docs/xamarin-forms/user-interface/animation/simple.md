---
title: Proste animacje w interfejsie Xamarin.Forms
description: Klasa ViewExtensions udostępnia metody rozszerzenia, które mogą służyć do konstruowania proste animacje. W tym artykule przedstawiono tworzenie i anulowanie animacji przy użyciu klasy ViewExtensions.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: 124fc311d5e2c8c89353ba813df60f0bf1d0b34a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997074"
---
# <a name="simple-animations-in-xamarinforms"></a>Proste animacje w interfejsie Xamarin.Forms

_Klasa ViewExtensions udostępnia metody rozszerzenia, które mogą służyć do konstruowania proste animacje. W tym artykule przedstawiono tworzenie i anulowanie animacji przy użyciu klasy ViewExtensions._


[ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Klasy udostępnia następujące metody rozszerzenia, które mogą służyć do tworzenia proste animacje:

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animuje [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) i [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) właściwości [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`ScaleTo`](xref:Xamarin.Forms.VisualElement.Scale) Animuje [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) właściwość [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) stosuje animowany przyrostowe zwiększenia lub zmniejszenia, aby [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) właściwość [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animuje [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) właściwość [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) stosuje animowany przyrostowe zwiększenia lub zmniejszenia, aby [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) właściwość [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animuje [ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX) właściwość [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animuje [ `RotationY` ](xref:Xamarin.Forms.VisualElement.RotationY) właściwość [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animuje [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) właściwość [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).

Domyślnie każda animacja potrwa 250 milisekund. Jednakże czas trwania każdej animacji można określić podczas tworzenia animacji.

[ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Zawiera również klasy [ `CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) metodę, która może służyć do anulować wszelkie animacji.

> [!NOTE]
> [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Klasa udostępnia [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)) — metoda rozszerzenia. Jednak ta metoda jest przeznaczona do użycia przez układy animować przejścia między Stanami układu, które zawierają rozmiar i położenie zmiany. W związku z tym, można stosować tylko przez [ `Layout` ](xref:Xamarin.Forms.Layout) podklasach.

Metody rozszerzające animacji w [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) klasy są asynchroniczne i zwracany `Task<bool>` obiektu. Wartość zwracana jest `false` wykona animacji, i `true` Jeśli animacja zostanie anulowana. W związku z tym, metody animacji, zazwyczaj należy używać z `await` operatora, który sprawia, że można łatwo określić, po zakończeniu animacji. Ponadto następnie możliwe staje się tworzenie sekwencyjnego animacji przy użyciu metod kolejnych animacji wykonywanie po poprzedniej metodzie. Aby uzyskać więcej informacji, zobacz [złożone animacje](#compound).

Jeśli istnieje wymóg, aby umożliwić animacji ukończone w tle, a następnie `await` operator można pominąć. Metody rozszerzenia animacji w tym scenariuszu szybko zwróci się po zainicjowaniu animacji, za pomocą animacji, pojawiają się w tle. Ta operacja może zostać pobrany z zalet podczas tworzenia złożonych animacji. Aby uzyskać więcej informacji, zobacz [złożonego animacji](#composite).

Aby uzyskać więcej informacji na temat `await` operatora, zobacz [Omówienie obsługi asynchronicznej](~/cross-platform/platform/async.md).

## <a name="single-animations"></a>Pojedynczy animacji

Każda metoda rozszerzenia w [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) implementuje operację jednej animacji, który stopniowo zmienia się właściwość z jedną wartość na inną wartość w okresie czasu. W tej sekcji przedstawiono każdej operacji animacji.

### <a name="rotation"></a>Obrót

Poniższy przykład kodu demonstruje sposób użycia [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metodę, aby animować [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) właściwość [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Ten kod animuje [ `Image` ](xref:Xamarin.Forms.Image) wystąpienia obracając do 360 stopni ponad 2 sekundy (2000 MS). [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda uzyskuje bieżącą [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) właściwości wartość rozpoczęcia animacji, a następnie obraca się od tej wartości na swój pierwszy argument (360). Po zakończeniu animacji zakończeniu obrazu [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) właściwość zostaje zresetowana do 0. Gwarantuje to, że `Rotation` właściwości nie obowiązywać 360, po zakończeniu animacji, które uniemożliwiłyby dodatkowe wymiany.

Poniższe zrzuty ekranu Pokaż obrót w toku dla każdej platformy:

![](simple-images/rotateto.png "Animacja rotacji")

### <a name="relative-rotation"></a>Względna obrotu

Poniższy przykład kodu demonstruje sposób użycia [ `RelRotateTo` ](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metodę, aby stopniowo zwiększać lub zmniejszać [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) właściwość [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RelRotateTo (360, 2000);
```

Ten kod animuje [ `Image` ](xref:Xamarin.Forms.Image) wystąpienia obracając 360 stopni od jego pozycja początkowa ponad 2 sekundy (2000 MS). [ `RelRotateTo` ](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda uzyskuje bieżącą [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) właściwości wartość rozpoczęcia animacji, a następnie obraca się od tej wartości z wartości, a także swój pierwszy argument (360). Dzięki temu każda animacja będzie zawsze rotację 360 stopni od pozycji początkowej. W związku z tym jeśli nowej animacji jest wywoływana, gdy trwa już animacji, będzie rozpocznie się od bieżącej pozycji i może zostać zakończona w położeniu, który nie jest przyrost 360 stopni.

Poniższych zrzutach ekranu przedstawiono względne obrotu w toku dla każdej platformy:

![](simple-images/relrotateto.png "Animacja rotacji względnej")

### <a name="scaling"></a>Skalowanie

Poniższy przykład kodu demonstruje sposób użycia [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) metodę, aby animować [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) właściwość [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.ScaleTo (2, 2000);
```

Ten kod animuje [ `Image` ](xref:Xamarin.Forms.Image) wystąpienia, skalując w górę dwukrotnie ponad 2 sekundy (2000 MS). [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) Metoda uzyskuje bieżącą [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) wartość właściwości (wartość domyślna-1) rozpoczęcia animacji, a następnie skalować od tej wartości na swój pierwszy argument [2]. Ma to wpływ rozszerzania rozmiaru obrazu dwukrotnie.

Poniższe zrzuty ekranu Pokaż skalowania w toku dla każdej platformy:

![](simple-images/scaleto.png "Skalowanie animacji")

### <a name="relative-scaling"></a>Względna skalowania

Poniższy przykład kodu demonstruje sposób użycia [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metodę, aby animować [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) właściwość [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RelScaleTo (2, 2000);
```

Ten kod animuje [ `Image` ](xref:Xamarin.Forms.Image) wystąpienia, skalując w górę dwukrotnie ponad 2 sekundy (2000 MS). [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda uzyskuje bieżącą [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) wartość właściwości rozpoczęcia animacji, a następnie skalować od tej wartości z wartości, a także swój pierwszy argument [2]. Daje to gwarancję, że każda animacja będzie zawsze skalowanie 2 od pozycji początkowej.

### <a name="scaling-and-rotation-with-anchors"></a>Skalowanie i obrót o zakotwiczenia

[ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) i [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) właściwości ustaw środek skalowanie lub obrót dla [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) i [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) właściwości. W związku z tym, ich wartości również wpływać na [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) i [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) metody.

Biorąc pod uwagę [ `Image` ](xref:Xamarin.Forms.Image) który został umieszczony w środku układu, poniższy przykład kodu demonstruje, obracanie obrazów wokół środka układ, ustawiając jego [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) Właściwość:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

Aby obrócić [ `Image` ](xref:Xamarin.Forms.Image) wystąpienia wokół środka układu, [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) i [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) właściwości musi być równa wartości, które są względem szerokości i wysokości `Image`. W tym przykładzie Centrum `Image` jest zdefiniowany w środku układu i dlatego domyślnie `AnchorX` wartość 0,5 nie wymagało zmiany. Jednak `AnchorY` właściwość zostanie przedefiniowana, wartość od górnej krawędzi `Image` do punktu centralnego układu. Gwarantuje to, że `Image` sprawia, że pełna obrót 360 stopni wokół punktu centralnego układu, jak pokazano na poniższych zrzutach ekranu:

![](simple-images/rotate-anchors.png "Animacja rotacji z zakotwiczenia")

### <a name="translation"></a>{1&gt;Translacja&lt;1}

Poniższy przykład kodu demonstruje sposób użycia [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) metodę, aby animować [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) i [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) właściwości [ `Image`](xref:Xamarin.Forms.Image):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Ten kod animuje [ `Image` ](xref:Xamarin.Forms.Image) wystąpienia zamieniane go poziomo i pionowo trwa ponad 1 sekundę (1000 milisekund). [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda jednocześnie tłumaczy piksele obrazu 100 po lewej stronie i 100 pikseli w górę. Jest to spowodowane pierwszy i drugi argument są zarówno wartości ujemne. Zapewnianie liczb dodatnich przetłumaczy obraz w prawo i w dół.

Poniższych zrzutach ekranu przedstawiono tłumaczenia w toku dla każdej platformy:

![](simple-images/translateto.png "Animacja tłumaczenia")

> [!NOTE]
> Jeśli element jest początkowo rozmieszczony poza ekranem, a następnie przetłumaczony na ekranie, po translacji układ danych wejściowych elementu pozostanie wyłączony ekranu, a użytkownik nie może korzystać z niego. Dlatego zalecane jest, że widok powinien być rozmieszczone w położeniu końcowym, a następnie wszystkie wymagane tłumaczenia wykonywane.

### <a name="fading"></a>Zanikanie

Poniższy przykład kodu demonstruje sposób użycia [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metodę, aby animować [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) właściwość [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Ten kod animuje [ `Image` ](xref:Xamarin.Forms.Image) wystąpienie przez Rozjaśnianie ponad 4 sekund (4000 w milisekundach). [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda uzyskuje bieżącą [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) wartość właściwości rozpoczęcia animacji, a następnie stopniowo przechodzi w tej wartości w jej pierwszy argument (1).

Poniższych zrzutach ekranu przedstawiono fade w toku dla każdej platformy:

![](simple-images/fadeto.png "Zanikanie animacji")

<a name="compound" />

## <a name="compound-animations"></a>Animacje złożona (c)

Złożone animacji jest kombinacją sekwencyjne animacji i mogą być tworzone za pomocą `await` operatora, jak pokazano w poniższym przykładzie kodu:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

W tym przykładzie [ `Image` ](xref:Xamarin.Forms.Image) jest tłumaczony ponad 6 sekund (6000 w milisekundach). Tłumaczenie `Image` za pomocą pięciu animacji `await` operator wskazujący sekwencyjnie wykonuje każdą animację. W związku z tym animacji kolejnych metod wykonania po poprzedniej metodzie zostało zakończone.

<a name="composite" />

## <a name="composite-animations"></a>Złożone animacji

Animacja złożonego składa się z animacji gdzie animacji co najmniej dwa uruchomione jednocześnie. Złożone animacji można utworzyć przez mieszanie animacje oczekiwanych i najczęściej inne niż oczekiwane, jak pokazano w poniższym przykładzie kodu:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

W tym przykładzie [ `Image` ](xref:Xamarin.Forms.Image) jest skalowany i jednocześnie obracany ponad 4 sekund (4000 w milisekundach). Skalowanie `Image` używa dwóch sekwencyjne animacji, które występują w tym samym czasie jako obrót. [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Metoda jest wykonywana bez `await` operatora i zwraca wynik natychmiast, przy czym pierwsze [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) następnie Rozpoczynanie animacji. `await` Operator pierwszego `ScaleTo` wywołania metody opóźnia drugi `ScaleTo` wywołanie metody do momentu pierwszego `ScaleTo` wywołania metody została ukończona. W tym momencie `RotateTo` animacji jest w połowie drogi ukończone i `Image` będzie obracany o 180 stopni. W ostatnim 2 sekundy (2000 MS) drugi `ScaleTo` animacji i `RotateTo` animacji, zarówno ukończenia.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Równoczesne działanie wielu metod asynchronicznych

`static` `Task.WhenAny` i `Task.WhenAll` metody umożliwiają jednoczesne uruchamianie wielu metod asynchronicznych i dlatego może służyć do tworzenia złożonych animacji. Obie metody zwracają `Task` obiektu i zaakceptuj kolekcję metod każdy zwracany `Task` obiektu. `Task.WhenAny` Metoda zostaje ukończony po dowolną metodę w jego kolekcja kończy wykonywanie, jak pokazano w poniższym przykładzie kodu:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

W tym przykładzie `Task.WhenAny` wywołania metody zawiera dwa zadania. Pierwsze zadanie obrót ponad 4 sekund (4000 w milisekundach), a drugie zadanie podrzędne, skaluje obraz ponad 2 sekundy (2000 MS). Po ukończeniu drugie zadanie `Task.WhenAny` ukończenia wywołania metody. Jednak mimo że [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) metody jest nadal uruchomione, drugi [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) metoda można rozpocząć.

`Task.WhenAll` Metoda kończy po zakończeniu wszystkich metod w swojej kolekcji, jak pokazano w poniższym przykładzie kodu:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

W tym przykładzie `Task.WhenAll` wywołania metody zawiera trzy zadania, z których każdy wykonuje ponad 10 minut. Każdy `Task` sprawia, że różną liczbę rotacji 360 stopni — 307 rotacji dla [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), 251 rotacji dla [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))i wymiany 199 dla [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)). Te wartości są liczb pierwszych, w związku z tym zagwarantowanie, że obroty nie są zsynchronizowane i dlatego nie spowoduje wzorców powtarzających się.

Poniższych zrzutach ekranu przedstawiono wiele wymiany w toku dla każdej platformy:

![](simple-images/multiple-rotations.png "Złożone animacji")

## <a name="canceling-animations"></a>Trwa anulowanie animacji

Aplikację można anulować animacji co najmniej jeden z wywołaniem `static` [ `ViewExtensions.CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) metody, jak pokazano w poniższym przykładzie kodu:

```csharp
ViewExtensions.CancelAnimations (image);
```

To natychmiastowe spowoduje anulowanie wszystkich animacji, które są aktualnie uruchomione na [ `Image` ](xref:Xamarin.Forms.Image) wystąpienia.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono tworzenie i anulowanie animacji przy użyciu [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) klasy. Ta klasa udostępnia metody rozszerzenia, które może służyć do konstruowania proste animacje, obracanie, skalowanie, tłumaczenie i zanikanie [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) wystąpień.


## <a name="related-links"></a>Linki pokrewne

- [Asynchroniczna pomoc techniczna — omówienie](~/cross-platform/platform/async.md)
- [Podstawowa Animacja (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
