---
title: Niestandardowe animacje w interfejsie Xamarin.Forms
description: W tym artykule pokazano, jak klasa animacji Xamarin.FOrms służy do tworzenia i anulować animacji, synchronizowanie wielu animacji i utworzyć niestandardowe animacje, które animować właściwości, które nie są animowane za pomocą istniejących metod animacji.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 519368031384e72a2d2e0a7c99053be44ea4cffc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995224"
---
# <a name="custom-animations-in-xamarinforms"></a>Niestandardowe animacje w interfejsie Xamarin.Forms

_Klasa animacji jest blokiem konstrukcyjnym wszystkie animacje Xamarin.Forms, za pomocą metod rozszerzenia w klasie ViewExtensions, tworząc jeden lub więcej obiektów w animacji. W tym artykule pokazano, jak klasa animacji służy do tworzenia i anulować animacji, synchronizowanie wielu animacji i utworzyć niestandardowe animacje, które animować właściwości, które nie są animowane za pomocą istniejących metod animacji._


Podczas tworzenia, należy określić liczbę parametrów `Animation` obiektu, z uwzględnieniem wartości początkowa i końcowa właściwości animowany i wywołanie zwrotne, które zmienia wartość właściwości. `Animation` Obiektu można również Obsługa kolekcji animacji podrzędne, które można uruchomić i zsynchronizowane. Aby uzyskać więcej informacji, zobacz [animacji podrzędnych](#child).

Uruchamianie animacji, który został utworzony za pomocą [ `Animation` ](xref:Xamarin.Forms.Animation) klasy, która może lub nie może zawierać podrzędnych animacji, odbywa się przez wywołanie metody [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) metody. Ta metoda określa czas trwania animacji, jak i między innymi elementami, wywołanie zwrotne, które określa, czy animacji.

## <a name="creating-an-animation"></a>Tworzenie animacji

Podczas tworzenia [ `Animation` ](xref:Xamarin.Forms.Animation) obiektu zazwyczaj co najmniej trzy parametry są wymagane, jak pokazano w poniższym przykładzie kodu:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Ten kod definiuje animację [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) właściwość [ `Image` ](xref:Xamarin.Forms.Image) wystąpienie z wartością 1 wartość 2. Animowany wartość, która jest uzyskiwana w wyniku Xamarin.Forms, jest przekazywany do wywołania zwrotnego, określony jako pierwszy argument, gdzie jest używany, aby zmienić wartość `Scale` właściwości.

Animacja została uruchomiona z wywołaniem [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) metody, jak pokazano w poniższym przykładzie kodu:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Należy pamiętać, że [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) metoda nie zwraca `Task` obiektu. Zamiast tego powiadomienia są dostarczane za pośrednictwem metod wywołania zwrotnego.

Następujące argumenty są określone w `Commit` metody:

- Pierwszy argument (*właściciela*) identyfikuje właściciela animacji. Może to być element wizualny zastosowania animacji lub inny element wizualny, takich jak strony.
- Drugi argument (*nazwa*) identyfikuje animacji o nazwie. Nazwa w połączeniu z właścicielem aby jednoznacznie zidentyfikować animacji. Ten unikatowy identyfikator, następnie może służyć do określenia, czy animacja jest uruchomiona ([`AnimationIsRunning`](xref:Xamarin.Forms.AnimationExtensions.AnimationIsRunning(Xamarin.Forms.IAnimatable,System.String))), lub Anuluj ją ([`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String))).
- Trzeci argument (*współczynnik*) wskazuje liczbę milisekund między każdym wywołaniem metody wywołania zwrotnego, zdefiniowanej w [ `Animation` ](xref:Xamarin.Forms.Animation) konstruktora
- Czwarty argument (*długość*) wskazuje czas trwania animacji, w milisekundach.
- Piąty argument (*ułatwianie*) definiuje funkcję sterowania tempem zmian, które ma być używany w animacji. Alternatywnie można określić jako argument do funkcji sterowania tempem zmian [ `Animation` ](xref:Xamarin.Forms.Animation) konstruktora. Aby uzyskać więcej informacji na temat funkcje easingu zobacz [funkcji Easingu](~/xamarin-forms/user-interface/animation/easing.md).
- Szósty argument (*Zakończono*) jest wywołaniem zwrotnym, które zostaną wykonane po zakończeniu animacji. To wywołanie zwrotne przyjmuje dwa argumenty, z pierwszym argumentem wskazujący końcowa wartość, a drugi argument jest `bool` który jest skonfigurowany do `true` Jeśli animacji zostało anulowane. Alternatywnie *Zakończono* wywołanie zwrotne, które można określić jako argument do [ `Animation` ](xref:Xamarin.Forms.Animation) konstruktora. Jednak w przypadku pojedynczego animacji Jeśli *Zakończono* wywołania zwrotne są określone w obu `Animation` Konstruktor i `Commit` metody tylko wywołania zwrotnego, określone w `Commit` będzie można wykonać metody.
- Siódmego argumentu (*Powtórz*) jest wywołaniem zwrotnym, które umożliwia animacji do powtarzania. Jest wywoływana po zakończeniu animacji i zwracanie `true` wskazuje, że należy powtórzyć animację.

Ogólny efekt jest utworzyć animację, która zwiększa [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) właściwość [ `Image` ](xref:Xamarin.Forms.Image) z zakresu od 1 do 2, ponad 2 sekundy (2000 MS), za pomocą [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) funkcja sterowania tempem zmian. Każdym zakończeniu animacji, jego `Scale` właściwość jest resetowana do 1 i powtarza animacji.

> [!NOTE]
> Można skonstruować współbieżnych animacji, działających niezależnie od siebie nawzajem, tworząc `Animation` obiektu każdej animacji, a następnie wywołując `Commit` metody na każdą animację.

<a name="child" />

### <a name="child-animations"></a>Animacje podrzędne

[ `Animation` ](xref:Xamarin.Forms.Animation) Klasy obsługuje również animacje podrzędne, które obejmuje utworzenie `Animation` obiektu, do których innych `Animation` obiekty zostaną dodane. Dzięki temu szereg animacji musi być uruchamiany i zsynchronizowane. Poniższy przykład kodu demonstruje, tworzenie i uruchamianie animacji podrzędne:

```csharp
var parentAnimation = new Animation ();
var scaleUpAnimation = new Animation (v => image.Scale = v, 1, 2, Easing.SpringIn);
var rotateAnimation = new Animation (v => image.Rotation = v, 0, 360);
var scaleDownAnimation = new Animation (v => image.Scale = v, 2, 1, Easing.SpringOut);

parentAnimation.Add (0, 0.5, scaleUpAnimation);
parentAnimation.Add (0, 1, rotateAnimation);
parentAnimation.Add (0.5, 1, scaleDownAnimation);

parentAnimation.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

Alternatywnie przykład kodu mogą być zapisywane bardziej zwięzłym jako wykazały w poniższym przykładzie kodu:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

W obu przykładach kodu nadrzędnego [ `Animation` ](xref:Xamarin.Forms.Animation) obiekt zostanie utworzony, w którym dodatkowe `Animation` obiekty zostaną następnie dodane. Pierwsze dwa argumenty [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) metoda Określ, kiedy rozpoczęcia i zakończenia animacji podrzędnych. Wartości argumentu musi należeć do zakresu od 0 do 1 i reprezentuje względną okres, w animacji nadrzędnego animacji określony podrzędny zostanie uaktywniona. W związku z tym, w tym przykładzie `scaleUpAnimation` zostanie uaktywniona w pierwszej połowie animacji, `scaleDownAnimation` zostanie uaktywniona w drugiej połowie animacji i `rotateAnimation` będzie aktywna przez cały czas.

Ogólny efekt jest animacji ponad 4 sekund (4000 w milisekundach). `scaleUpAnimation` Animuje [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) właściwości z zakresu od 1 do 2, ponad 2 sekundy. `scaleDownAnimation` Następnie `Scale` właściwości od 2 do 1, ponad 2 sekundy. Gdy występują zarówno animacjami skalowania `rotateAnimation` animuje [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) właściwości z zakresu od 0 do 360, ponad 4 sekund. Należy pamiętać, że animacjami skalowania również używają funkcji sterowania tempem zmian. [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) Funkcja sterowania tempem zmian powoduje, że [ `Image` ](xref:Xamarin.Forms.Image) początkowo zmniejszyć przed pobraniem większych i [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) funkcja sterowania tempem zmian powoduje, że `Image` przestanie mniejszy niż rozmiaru rzeczywistego pod koniec pełną animacji.

Istnieje kilka różnic między [ `Animation` ](xref:Xamarin.Forms.Animation) obiektu, który używa animacji podrzędnego a, który nie:

- Korzystając z animacjami podrzędnych *Zakończono* wywołania zwrotnego dla animacji podrzędnych wskazuje po zakończeniu element podrzędny i *Zakończono* wywołanie zwrotne przekazane do `Commit` metoda wskazują, kiedy Ukończono całej animacji.
- Korzystając z podrzędnych animacji, zwracając `true` z *Powtórz* wywołanie zwrotne bezużytecznego `Commit` metoda nie powoduje powtarzanie animacji, ale animacji, będą nadal działać bez nowe wartości.
- Podczas włączania funkcji sterowania tempem zmian w `Commit` metoda i funkcji sterowania tempem zmian zwraca wartość większą niż 1, zostanie zakończona Animacja. Jeśli funkcja sterowania tempem zmian zwróci wartość mniejszą niż 0, jest powiązany wartość 0. Do użycia funkcji sterowania tempem zmian, która zwraca wartość mniejsza od 0 lub większa niż 1 musi określony w jednej animacji podrzędnych, a nie w `Commit` metody.

[ `Animation` ](xref:Xamarin.Forms.Animation) Zawiera również klasy [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)) metod, które mogą służyć do dodawania animacji podrzędnych do elementu nadrzędnego `Animation` obiektu. Jednak ich *rozpocząć* i *Zakończ* wartości argumentu nie są ograniczone do 0, 1, ale tylko część animacji podrzędny, która odnosi się do zakresu od 0 do 1 będą aktywne. Na przykład jeśli `WithConcurrent` wywołania metody definiuje animacji podrzędnego, który jest przeznaczony dla [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) właściwości z zakresu od 1 do 6, ale z *rozpocząć* i *Zakończ* wartości -2 i 3, *rozpocząć* wartości od -2 odpowiada `Scale` wartość 1, a *Zakończ* wartość 3 odnosi się do `Scale` wartość 6. Ponieważ wartości spoza zakresu od 0 do 1 odtwarzania bez części w animacji, `Scale` właściwość będzie tylko animowana od 3 do 6.

## <a name="canceling-an-animation"></a>Trwa anulowanie animacji

Aplikację można anulować animację po wywołaniu [ `AbortAnimation` ](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String)) metodę rozszerzenia, jak pokazano w poniższym przykładzie kodu:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Należy pamiętać, że animacji unikatowo zidentyfikować za pomocą kombinacji właściciela animacji i nazwa animacji. W związku z tym właściciela i nazwa określona podczas uruchamiania animacji należy określić anulować animacji. W związku z tym, w przykładzie kodu natychmiast anulować animację o nazwie `SimpleAnimation` który jest własnością strony.

## <a name="creating-a-custom-animation"></a>Tworzenie animacji niestandardowej

Przykłady zamieszczone w tym miejscu do tej pory wykazały animacji, które są równie można osiągnąć za pomocą metod w [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) klasy. Jednak zaletą [ `Animation` ](xref:Xamarin.Forms.Animation) klasa jest, że ma dostęp do metody wywołania zwrotnego, która jest wykonywana po zmianie wartości animowany. Dzięki temu wywołania zwrotnego zaimplementować wszystkie żądane animacji. Na przykład, poniższy kod animuje [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) właściwości strony, ustawiając dla niej [ `Color` ](xref:Xamarin.Forms.Color) wartościami utworzonymi przez [ `Color.FromHsla` ](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))metody, przy użyciu wartości odcień z zakresu od 0 do 1:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Wynikowa animacja zapewnia przesuwania tła strony za pomocą kolorów tęczowego wygląd.

Aby uzyskać więcej przykładów dotyczących tworzenia złożonych animacje, łącznie z animację krzywej Beziera zobacz [22 rozdział](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) z [tworzenia aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="creating-a-custom-animation-extension-method"></a>Tworzenie animacji niestandardowej metody rozszerzenia

Metody rozszerzające w [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) klasy animować właściwości bieżącej wartości określonej wartości. To sprawia, że trudno utworzyć, na przykład `ColorTo` metoda animacji, którego można animować kolor z jedną wartość na inny, ponieważ:

- Tylko [ `Color` ](xref:Xamarin.Forms.Color) zdefiniowaną przez właściwość [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) klasa jest [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor), który nie jest zawsze żądaną `Color` właściwości Aby animować.
- Często bieżącą wartość [ `Color` ](xref:Xamarin.Forms.Color) właściwość [ `Color.Default` ](xref:Xamarin.Forms.Color.Default), który nie jest prawdziwe kolorów i nie można użyć w obliczeniach interpolacji.

Rozwiązanie tego problemu jest nie `ColorTo` metoda docelowy określonego [ `Color` ](xref:Xamarin.Forms.Color) właściwości. Zamiast tego mogą być zapisywane z metodą wywołania zwrotnego, która przekazuje interpolowane `Color` wartość obiektu wywołującego. Ponadto metoda będzie zająć rozpoczęcia i zakończenia `Color` argumentów.

`ColorTo` Można zaimplementować metodę jako metodę rozszerzenia, która używa [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) method in Class metoda [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions) klasy w celu zapewnienia jego funkcji. Jest to spowodowane `Animate` metoda może służyć do właściwości obiektu docelowego, które nie są typu `double`, jak pokazano w poniższym przykładzie kodu:

```csharp
public static class ViewExtensions
{
  public static Task<bool> ColorTo(this VisualElement self, Color fromColor, Color toColor, Action<Color> callback, uint length = 250, Easing easing = null)
  {
    Func<double, Color> transform = (t) =>
      Color.FromRgba(fromColor.R + t * (toColor.R - fromColor.R),
                     fromColor.G + t * (toColor.G - fromColor.G),
                     fromColor.B + t * (toColor.B - fromColor.B),
                     fromColor.A + t * (toColor.A - fromColor.A));
    return ColorAnimation(self, "ColorTo", transform, callback, length, easing);
  }

  public static void CancelAnimation(this VisualElement self)
  {
    self.AbortAnimation("ColorTo");
  }

  static Task<bool> ColorAnimation(VisualElement element, string name, Func<double, Color> transform, Action<Color> callback, uint length, Easing easing)
  {
    easing = easing ?? Easing.Linear;
    var taskCompletionSource = new TaskCompletionSource<bool>();

    element.Animate<Color>(name, transform, callback, 16, length, easing, (v, c) => taskCompletionSource.SetResult(c));
    return taskCompletionSource.Task;
  }
}
```

[ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) Metoda wymaga *Przekształcanie* argumentu, który jest metodą wywołania zwrotnego. Dane wejściowe to wywołanie zwrotne jest zawsze `double` od 0 do 1. W związku z tym `ColorTo` metoda definiuje swój własny przekształcenie `Func` akceptujący `double` od 0 do 1 oraz że zwraca [ `Color` ](xref:Xamarin.Forms.Color) wartość odpowiadającą tej wartości. `Color` Wartość jest obliczana na podstawie interpolacji [ `R` ](xref:Xamarin.Forms.Color.R), [ `G` ](xref:Xamarin.Forms.Color.G), [ `B` ](xref:Xamarin.Forms.Color.B), i [ `A` ](xref:Xamarin.Forms.Color.A) wartości dwóch dostarczonych `Color` argumentów. `Color` Wartość jest następnie przekazywany do metody wywołania zwrotnego dla aplikacji do określonej właściwości.

Takie podejście umożliwia `ColorTo` metodę, aby animować dowolne [ `Color` ](xref:Xamarin.Forms.Color) właściwości, jak pokazano w poniższym przykładzie kodu:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

W tym przykładzie kodu `ColorTo` animuje metoda [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) i [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) właściwości [ `Label` ](xref:Xamarin.Forms.Label), `BackgroundColor`właściwości strony, a [ `Color` ](xref:Xamarin.Forms.BoxView.Color) właściwość [ `BoxView` ](xref:Xamarin.Forms.BoxView).

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia [ `Animation` ](xref:Xamarin.Forms.Animation) klasa do tworzenia i anulować animacji, synchronizowanie wielu animacji i utworzyć niestandardowe animacje, które animować właściwości, które nie są animowane przez istniejące animacji metody. `Animation` Klasa jest elementem składowym wszystkie animacje zestawu narzędzi Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe animacje (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [Animacja](xref:Xamarin.Forms.Animation)
- [AnimationExtensions](xref:Xamarin.Forms.AnimationExtensions)
