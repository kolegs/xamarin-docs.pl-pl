---
title: Proste animacji
description: "Klasa ViewExtensions udostępnia metody rozszerzenia, które mogą służyć do tworzenia prostych animacji. W tym artykule przedstawiono tworzenie i anulowania za pomocą klasy ViewExtensions animacji."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: fb7ca216978e4c890349a44b07d5a383e9ca2384
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="simple-animations"></a>Proste animacji

_Klasa ViewExtensions udostępnia metody rozszerzenia, które mogą służyć do tworzenia prostych animacji. W tym artykule przedstawiono tworzenie i anulowania za pomocą klasy ViewExtensions animacji._


[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasy udostępnia następujące metody rozszerzenia, które mogą służyć do tworzenia prostych animacji:

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animuje [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) i [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) właściwości [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`ScaleTo`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Animuje [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) właściwość [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) stosuje animowany przyrostowe zwiększenia lub zmniejszenia się [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) właściwość [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animuje [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) właściwość [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RelRotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) stosuje animowany przyrostowe zwiększenia lub zmniejszenia się [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) właściwość [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateXTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animuje [ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) właściwość [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateYTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animuje [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) właściwość [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`FadeTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animuje [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) właściwość [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).

Domyślnie każda animacja potrwa 250 milisekund. Jednak okres każdej animacji można określić podczas tworzenia animacji.

[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Zawiera również klasy [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) metodę, która może służyć do anulować wszelkie animacji.

> [!NOTE]
> [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasa udostępnia [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/) — metoda rozszerzenia. Jednak ta metoda jest przeznaczona do użycia przez układów do animowania przejścia między Stanami układu, które zawierają rozmiar i pozycja zmiany. W związku z tym można stosować tylko przez [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) podklasy.

Metody rozszerzenia animacji w [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) klasy są asynchroniczne i zwracany `Task<bool>` obiektu. Wartość zwracana jest `false` po zakończeniu animacji, i `true` Jeśli animacja zostanie anulowana. W związku z tym metody animacji zwykle powinien być używany z `await` operatora, dzięki czemu można łatwo określić, po zakończeniu animacji. Ponadto następnie staje się on można tworzyć kolejnych animacji metody kolejnych animacji, wykonywanie po wykonaniu poprzednich — metoda. Aby uzyskać więcej informacji, zobacz [złożone animacje](#compound).

Jeśli jest wymagany, aby umożliwić animacji pełną w tle, a następnie `await` operator może zostać pominięty. W tym scenariuszu metody rozszerzenia animacji szybko zwróci się po zainicjowaniu animacji, o animacji wykonywane w tle. Ta operacja może zostać pobrany z zalet podczas tworzenia złożonych animacji. Aby uzyskać więcej informacji, zobacz [złożone animacje](#composite).

Aby uzyskać więcej informacji na temat `await` operatora, zobacz [Przegląd pomocy technicznej Async](~/cross-platform/platform/async.md).

## <a name="single-animations"></a>Pojedynczy animacji

Każda metoda rozszerzenia w [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) implementuje operację animacji w jednej, która stopniowo zmieni się właściwość z jedną wartość na inną wartość w danym okresie czasu. Ta sekcja opisuje każdej operacji animacji.

### <a name="rotation"></a>Obracanie

Poniższy przykład kodu pokazuje, za pomocą [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metody w celu animowania [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) właściwość [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Ten kod animuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wystąpienia przez obrócenie do 360 stopni ponad 2 sekundy (2000 MS). [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metoda uzyskuje bieżącego [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) właściwości wartość rozpoczęcia animacji, a następnie obraca się z daną wartością do jego pierwszego argumentu (360). Po zakończeniu animacji Ukończ, obrazu [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) właściwość zostanie zresetowana do 0. Gwarantuje to, że `Rotation` po zawiera animacji, które mogłyby uniemożliwiać obrotów dodatkowe właściwości nie pozostają w 360.

Poniższe zrzuty ekranu pokazują obrót w toku na każdej platformie:

![](simple-images/rotateto.png "Obracanie animacji")

### <a name="relative-rotation"></a>Względne obrotu

Poniższy przykład kodu pokazuje, za pomocą [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metody w celu stopniowego zwiększenia lub zmniejszenia [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) właściwość [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelRotateTo (360, 2000);
```

Ten kod animuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wystąpienia przez obrócenie 360 stopni od punktu początkowego ponad 2 sekundy (2000 MS). [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metoda uzyskuje bieżącego [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) właściwości wartość rozpoczęcia animacji, a następnie obraca się z daną wartość oraz jego pierwszego argumentu (360). Dzięki temu, że każda animacja będzie zawsze rotacji 360 stopni od pozycji początkowej. Dlatego jeśli nowa animacja jest wywoływane, gdy animacji jest już w toku, go rozpocznie się od bieżącej pozycji i może zostać zakończona w pozycji, która nie jest przyrostu 360 stopni.

Poniższe zrzuty ekranu pokazują względną obrót w toku na każdej platformie:

![](simple-images/relrotateto.png "Względne obrotu animacji")

### <a name="scaling"></a>Skalowanie

Poniższy przykład kodu pokazuje, za pomocą [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) metody w celu animowania [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) właściwość [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.ScaleTo (2, 2000);
```

Ten kod animuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wystąpienia przez skalowanie dwukrotnie ponad 2 sekundy (2000 MS). [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Metoda uzyskuje bieżącego [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) właściwości wartość (domyślnej wartości 1) rozpoczęcia animacji, a następnie skali od tej wartości na jej pierwszy argument [2]. To powoduje zwiększenie rozmiaru obrazu dwukrotnie.

Poniższe zrzuty ekranu pokazują, skalowania w toku na każdej platformie:

![](simple-images/scaleto.png "Skalowanie animacji")

### <a name="relative-scaling"></a>Względna skalowania

Poniższy przykład kodu pokazuje, za pomocą [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metody w celu animowania [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) właściwość [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelScaleTo (2, 2000);
```

Ten kod animuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wystąpienia przez skalowanie dwukrotnie ponad 2 sekundy (2000 MS). [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metoda uzyskuje bieżącego [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) wartości właściwości dla animacji i następnie skali z daną wartość oraz jego pierwszy argument [2]. Dzięki temu, że każda animacja będzie zawsze skalowanie 2 od pozycji początkowej.

### <a name="scaling-and-rotation-with-anchors"></a>Skalowanie i obrót o zakotwiczenia

[ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) i [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) właściwości ustawione Centrum skalowanie i obrót dla [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) i [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) właściwości. W związku z tym ich wartości wpłynąć na [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) i [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) metody.

Podane [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) który została umieszczona w Centrum układu, w poniższym przykładzie kodu pokazano, obracanie obrazów wokół środka układ przez ustawienie jej [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) Właściwości:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

Obracanie [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wystąpienia wokół środka układu [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) i [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) właściwości musi być ustawiona na wartości, które są względem szerokości i wysokości `Image`. W tym przykładzie środek `Image` jest zdefiniowany w Centrum układ i dlatego wartość domyślna `AnchorX` wartość 0,5 nie wymagają zmiany. Jednak `AnchorY` właściwość zostało ponownie zdefiniowane jako wartość z zakresu od początku `Image` do punktu centralnego układu. Gwarantuje to, że `Image` sprawia, że pełna rotacją 360 stopni wokół punktu centralnego układu, jak pokazano na poniższych zrzutach ekranu:

![](simple-images/rotate-anchors.png "Animacja Obrót o zakotwiczenia")

### <a name="translation"></a>Translacja

Poniższy przykład kodu pokazuje, za pomocą [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) metody w celu animowania [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) i [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) właściwości [ `Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Ten kod animuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wystąpienia przez tłumaczenie go w poziomie i w pionie ponad 1 sekundę (1000 milisekund). [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metody jednocześnie tłumaczy piksele obrazu 100 w lewo i 100 pikseli w górę. Jest tak, ponieważ pierwszy i drugi argument są obie wartości ujemne. Zapewnianie dodatnie przekłada obraz w prawo i w dół.

Poniższe zrzuty ekranu pokazują tłumaczenia w toku na każdej platformie:

![](simple-images/translateto.png "Animacja tłumaczenia")

> [!NOTE]
> Jeśli element jest początkowo zwolniono ekranu, a następnie przełożyć na ekranie, po translacji układ wejściowy elementu pozostanie wyłączony ekranu, a użytkownik nie może korzystać z niego. W związku z tym zalecane jest, że widoku należy określić w położeniu końcowego i następnie wszystkie wymagane tłumaczeń wykonywane.

### <a name="fading"></a>Zanikania

Poniższy przykład kodu pokazuje, za pomocą [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metody w celu animowania [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) właściwość [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Ten kod animuje [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wystąpienia przez zanikania go w ponad 4 sekundy (4000 w milisekundach). [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metoda uzyskuje bieżącego [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) wartości właściwości dla uruchomienia animacji, a następnie stopniowo przechodzi w od tej wartości w jego pierwszego argumentu (1).

Poniższe zrzuty ekranu pokazują zanikania w toku na każdej platformie:

![](simple-images/fadeto.png "Zanikania animacji")

<a name="compound" />

## <a name="compound-animations"></a>Złożone animacji

Złożone animacji jest kombinacją sekwencyjnych animacji i mogą być tworzone za pomocą `await` operatora, jak pokazano w poniższym przykładzie:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

W tym przykładzie [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) jest translacja ponad 6 sekund (6000 w milisekundach). W tłumaczeniu wartości `Image` używa animacji pięć z `await` operator wskazujący sekwencyjnie wykonuje każdy animacji. W związku z tym metody kolejnych animacji wykonać po ukończeniu poprzedniego — metoda.

<a name="composite" />

## <a name="composite-animations"></a>Złożone animacji

Złożone animacji jest kombinacją animacje gdzie dwóch lub więcej animacji działać jednocześnie. Złożone animacji można utworzyć przez mieszanie animacji oczekiwano i nie jest oczekiwane, jak pokazano w poniższym przykładzie kodu:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

W tym przykładzie [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) jest skalowany i jednocześnie obracany ponad 4 sekundy (4000 w milisekundach). Skalowanie `Image` używa dwóch sekwencyjnych animacji, które występują w tym samym czasie jako obrót. [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Metoda wykonuje bez `await` operatora i natychmiast zwraca przy pierwszym [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) animacji, następnie rozpoczyna. `await` Operator pierwszego `ScaleTo` wywołanie metody opóźnienia drugi `ScaleTo` wywołanie metody do momentu pierwszego `ScaleTo` wywołanie metody została ukończona. W tym momencie `RotateTo` animacji jest połowy sposób ukończone i `Image` będzie obracany o 180 stopni. Podczas końcowego 2 sekundy (2000 MS) drugi `ScaleTo` animacji i `RotateTo` animacji zarówno ukończenia.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Równoczesne działanie wielu metod asynchronicznych

`static` `Task.WhenAny` i `Task.WhenAll` metody są używane w celu jednoczesnego uruchamiania wielu metod asynchronicznych i w związku z tym może służyć do tworzenia złożonych animacji. Obie metody zwracają `Task` obiektu i zaakceptować kolekcję metod aby każdy zwracany `Task` obiektu. `Task.WhenAny` Metody działanie jest kończone po dowolnej metody w jego kolekcja zakończeniem wykonywania, jak pokazano w poniższym przykładzie:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

W tym przykładzie `Task.WhenAny` wywołanie metody zawiera dwa zadania. Pierwszym zadaniem obrót ponad 4 sekundy (4000 w milisekundach), a drugie zadanie skaluje obraz ponad 2 sekundy (2000 MS). Po ukończeniu zadania drugi `Task.WhenAny` ukończenia wywołania metody. Jednak mimo że [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) metody jest nadal uruchomiony, drugi [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) metody można rozpocząć.

`Task.WhenAll` Ukończeniu metody po zakończeniu wszystkich metod w swojej kolekcji, jak pokazano w poniższym przykładzie:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

W tym przykładzie `Task.WhenAll` wywołanie metody zawiera trzy zadania, które wykonuje ponad 10 minut. Każdy `Task` sprawia, że różne liczby obrotów 360 stopni — 307 obrotów dla [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), 251 obrotów dla [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)i 199 obrotów dla [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/). Te wartości są liczb pierwszych, dlatego zapewnienie, że obrotów nie są zsynchronizowane i dlatego nie spowoduje powtarzających się wzorce.

Poniższe zrzuty ekranu pokazują wielu obrotów w toku na każdej platformie:

![](simple-images/multiple-rotations.png "Złożone animacji")

## <a name="canceling-animations"></a>Anulowanie animacji

Aplikację można anulować animacje co najmniej jeden z wywołaniem do `static` [ `ViewExtensions.CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) metody, jak pokazano w poniższym przykładzie:

```csharp
ViewExtensions.CancelAnimations (image);
```

To natychmiast spowoduje anulowanie wszystkich animacji, które są aktualnie uruchomione na [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wystąpienia.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono tworzenie i anulowanie animacji przy użyciu [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) klasy. Ta klasa udostępnia metody rozszerzenia, które mogą służyć do tworzenia prostych animacji, które obracanie, skalowanie tłumaczenie i zanikania [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) wystąpień.


## <a name="related-links"></a>Linki pokrewne

- [Asynchroniczna pomoc techniczna — omówienie](~/cross-platform/platform/async.md)
- [Podstawowe animacji (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
