---
title: Animacji niestandardowej
description: "Klasa animacji jest blokiem konstrukcyjnym wszystkie animacje platformy Xamarin.Forms, za pomocą metod rozszerzenia klasy ViewExtensions utworzenie co najmniej jeden obiekt animacji. W tym artykule pokazano, jak klasa animacji służy do tworzenia i anulować animacji, synchronizowanie wielu animacji i utworzyć niestandardowe animacji, które animowania właściwości, które nie są animowane przez istniejące metody animacji."
ms.topic: article
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 42ef3e6c82763831b5114f3de7603bba8f59eac6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="custom-animations"></a>Animacji niestandardowej

_Klasa animacji jest blokiem konstrukcyjnym wszystkie animacje platformy Xamarin.Forms, za pomocą metod rozszerzenia klasy ViewExtensions utworzenie co najmniej jeden obiekt animacji. W tym artykule pokazano, jak klasa animacji służy do tworzenia i anulować animacji, synchronizowanie wielu animacji i utworzyć niestandardowe animacji, które animowania właściwości, które nie są animowane przez istniejące metody animacji._


Liczba parametrów należy określić podczas tworzenia `Animation` obiektu, w tym wartości początkowa i końcowa animowany, właściwości i wywołanie zwrotne, które zmienia się wartość właściwości. `Animation` Obiektu można również Obsługa kolekcji animacji podrzędne, które można uruchomić i zsynchronizowane. Aby uzyskać więcej informacji, zobacz [animacje podrzędnych](#child).

Uruchomiona Animacja utworzone za pomocą [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) klasy, która może lub nie może zawierać podrzędnych animacji, uzyskuje się poprzez wywołanie [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) metody. Ta metoda określa czas trwania animacji i spośród innych elementów wywołanie zwrotne, które określa, czy animacji.

## <a name="creating-an-animation"></a>Tworzenie animacji

Podczas tworzenia [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) obiekt zazwyczaj co najmniej trzy parametry są wymagane, jak pokazano w poniższym przykładzie:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Ten kod definiuje animację [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) właściwość [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wystąpienie z wartością 1 wartość 2. Animowany wartość, która jest uzyskiwana w wyniku platformy Xamarin.Forms, jest przekazywany do wywołania zwrotnego określony jako pierwszy argument służy do zmiany wartości `Scale` właściwości.

Animacja została uruchomiona z wywołaniem do [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) metody, jak pokazano w poniższym przykładzie:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Należy pamiętać, że [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) metoda nie zwraca `Task` obiektu. Zamiast tego powiadomienia są realizowane za pośrednictwem metody wywołania zwrotnego.

Następujące argumenty zostały określone w `Commit` metody:

- Pierwszy argument (*właściciela*) identyfikuje właściciela animacji. Może to być element wizualny zastosowania animacji lub inny element wizualny, takich jak strony.
- Drugi argument (*nazwa*) identyfikuje animacji z nazwą. Nazwa w połączeniu z właściciela do unikatowego identyfikowania animacji. Ten unikatowy identyfikator następnie może służyć do określenia, czy animacja jest uruchomiona ([`AnimationIsRunning`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AnimationIsRunning/p/Xamarin.Forms.IAnimatable/System.String/)), lub Anuluj ją ([`AbortAnimation`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/)).
- Trzeci argument (*szybkość*) wskazuje wyrażony w milisekundach czas między każde wywołanie metody wywołania zwrotnego zdefiniowanej w [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) — Konstruktor
- Czwarty argument (*długość*) wskazuje czas trwania animacji, w milisekundach.
- Piąty argument (*napięcia*) definiuje funkcji sterowania tempem zmian do użycia w animacji. Alternatywnie można określić jako argument do funkcji sterowania tempem zmian [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) konstruktora. Aby uzyskać więcej informacji na temat łatwiejszym funkcji, zobacz [łatwiejszym funkcji](~/xamarin-forms/user-interface/animation/easing.md).
- Argument szóstego (*Zakończono*) jest wywołaniem zwrotnym, które będą wykonywane po zakończeniu animacji. To wywołanie zwrotne przyjmuje dwa argumenty z pierwszym argumentem wskazujący końcowa wartość i drugi argument jest `bool` który ustawiono `true` Jeśli animacja została anulowana. Alternatywnie *Zakończono* wywołania zwrotnego można określić jako argument [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) konstruktora. Jednak w przypadku pojedynczego animacji Jeśli *Zakończono* wywołania zwrotne zostały określone zarówno `Animation` Konstruktor i `Commit` metody, tylko wywołania zwrotnego, określona w `Commit` będzie można wykonać metody.
- Argument siódmego (*Powtórz*) jest wywołaniem zwrotnym, które umożliwia animacji należy powtórzyć. Jest ona wywoływana po zakończeniu animacji i zwracanie `true` wskazuje, czy powinny być powtarzane animacji.

Ogólny efekt jest utworzenie animacji, które zwiększa [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) właściwość [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) od 1 do 2, ponad 2 sekundy (2000 MS), za pomocą [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) wyjścia funkcji sterowania tempem. Zawsze zakończeniu animacji, jego `Scale` właściwości jest zmieniany na 1 i powtarza animacji.

> [!NOTE]
> **Uwaga**: równoczesnych animacji, działające niezależnie od siebie nawzajem można skonstruować przez utworzenie `Animation` obiekt każdej animacji, a następnie wywołując `Commit` metody w każdej animacji.

<a name="child" />

### <a name="child-animations"></a>Animacje podrzędne

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Klasy obsługuje również animacje podrzędne, które obejmuje utworzenie `Animation` obiektu, do których innych `Animation` obiekty zostaną dodane. Dzięki temu szereg animacji można uruchomić i zsynchronizowane. W poniższym przykładzie kodu pokazano tworzenie i uruchamianie animacji podrzędnych:

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

Alternatywnie przykładowy kod może być zapisany więcej zwięzłym jako wykazały w poniższym przykładzie kodu:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

W obu przykłady kodu, nadrzędny [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) utworzony obiekt, do którego dodatkowe `Animation` obiekty zostaną następnie dodane. Pierwsze dwa argumenty [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) metody Określ, kiedy rozpoczęcia i zakończenia animacji podrzędnych. Wartości argumentu musi należeć do zakresu od 0 do 1 i stanowią względną termin animacji nadrzędnego animacji określony podrzędny będzie aktywny. W związku z tym, w tym przykładzie `scaleUpAnimation` będzie aktywny dla pierwszej połowy animacji, `scaleDownAnimation` będzie aktywny do drugiej połowie animacji i `rotateAnimation` będzie aktywny cały czas trwania.

Ogólny efekt jest animacji ponad 4 sekundy (4000 w milisekundach). `scaleUpAnimation` Animuje [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) właściwości z zakresu od 1 do 2, ponad 2 sekundy. `scaleDownAnimation` Następnie animuje `Scale` właściwości od 2 do 1, ponad 2 sekundy. Gdy występują zarówno animacje skali, `rotateAnimation` animuje [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) właściwości z zakresu od 0 do 360, ponad 4 sekundy. Należy pamiętać, że skalowania animacje również używać funkcji sterowania tempem zmian. [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) Wyjścia funkcji sterowania tempem powoduje, że [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) początkowo zmniejszyć przed pobraniem większy i [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) wyjścia funkcji sterowania tempem powoduje, że `Image` być mniejszy od rozmiaru rzeczywistego pod koniec pełną animacji.

Istnieje wiele różnic między [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) obiekt, który używa animacji podrzędne a, który nie:

- Korzystając z animacji podrzędnych *Zakończono* wywołania zwrotnego dla animacji podrzędnych wskazuje po zakończeniu podrzędne i *Zakończono* wywołanie zwrotne przekazane do `Commit` metoda wskazuje, kiedy Ukończono całej animacji.
- Korzystając z animacji podrzędnych, zwracając `true` z *Powtórz* wywołania zwrotnego na `Commit` — metoda nie powoduje powtarzanie animacji, ale animacji będzie kontynuował pracę bez nowe wartości.
- Podczas włączania funkcji sterowania tempem zmian w `Commit` — metoda i funkcji sterowania tempem zmian zwraca wartość większą niż 1, zostanie zakończona Animacja. Jeśli funkcji sterowania tempem zmian zwróci wartość mniejszą niż 0, wartość jest zablokowane za pomocą 0. Aby używać funkcji sterowania tempem zmian, która zwraca wartość mniejsza niż 0 lub większą niż 1, musi on określony w jednej animacji podrzędnych, a nie w `Commit` metody.

[ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Zawiera również klasy [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/) metod, które mogą służyć do dodania animacji podrzędnych do elementu nadrzędnego `Animation` obiektu. Jednak ich *rozpocząć* i *Zakończ* wartości argumentów nie są ograniczone do 0, 1, ale tylko część animacji podrzędnych, która odnosi się do zakresu od 0 do 1 będzie aktywny. Na przykład jeśli `WithConcurrent` wywołanie metody definiuje animacji podrzędny, którego celem jest [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) właściwości z zakresu od 1 do 6, ale z *rozpocząć* i *Zakończ* wartości -2 i 3, *rozpocząć* wartość -2 odpowiada `Scale` wartość 1 i *Zakończ* odpowiada wartości 3 `Scale` wartość 6. Ponieważ wartości spoza zakresu od 0 do 1 odtwarzania bez części w animacji `Scale` właściwość zostanie tylko animowany od 3 do 6.

## <a name="canceling-an-animation"></a>Anulowanie animacji

Aplikację można anulować animacji wywołaniem [ `AbortAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/) — metoda rozszerzenia, jak pokazano w poniższym przykładzie:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Należy pamiętać, że animacji jest unikatowo identyfikowana przez kombinację animacji właściciela i nazwy animacji. W związku z tym właściciela i nazwy określone podczas uruchamiania animacji należy określić anulować animacji. W związku z tym przykładowy kod natychmiast anulować animacji, o nazwie `SimpleAnimation` który jest właścicielem strony.

## <a name="creating-a-custom-animation"></a>Tworzenie animacji niestandardowej

Przykłady podane tutaj wykonanej do tej pory wykazały animacji, które są równie może zostać osiągnięty przy metod w [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) klasy. Jednak zaletą [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) jest klasa, ma dostęp do metody wywołania zwrotnego, który zostanie wykonany po zmianie wartości animowany. Dzięki temu wywołania zwrotnego do zaimplementowania wszystkie żądane animacji. Na przykład w poniższym przykładzie kodu animuje [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) właściwości strony, ustawiając wartość [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) wartościami utworzonymi przez [ `Color.FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/)metody hue wartości od 0 do 1:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Wynikowa animacja zawiera wygląd przesuwania tła strony za pomocą kolorów siłowe.

Więcej przykładów dotyczących tworzenia złożonych animacji, w tym animację krzywej Beziera zobacz [22 rozdział](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) z [tworzenia aplikacji mobilnych za pomocą platformy Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="creating-a-custom-animation-extension-method"></a>Tworzenie animacji niestandardowej metody rozszerzenia

Metody rozszerzenia w [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) klasy animować właściwości od bieżącej wartości do określonej wartości. Dzięki temu trudne do tworzenia, na przykład `ColorTo` animacji metodę, która służy do animowania koloru z jedną wartość na inny, ponieważ:

- Jedynym [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) właściwości zdefiniowane przez [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) jest klasa [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), który nie jest zawsze żądaną `Color` właściwości Aby animować.
- Często bieżącą wartość [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) właściwość jest [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), który nie jest prawdziwe kolorów i nie można użyć w obliczeniach interpolacji.

Rozwiązaniem tego problemu jest ma `ColorTo` metoda docelowa określonego [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) właściwości. Zamiast tego mogą być zapisywane z metody wywołania zwrotnego, która przekazuje interpolowane `Color` wartość do wywołującego. Ponadto otrzymuje rozpoczęcia i zakończenia metody `Color` argumentów.

`ColorTo` Metody można zaimplementować jako metodę rozszerzenia, która używa [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) metody w [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) klasę, aby zapewnić jego funkcjonalność. Jest to spowodowane `Animate` metoda może służyć do właściwości obiektu docelowego, które nie są typu `double`, jak pokazano w poniższym przykładzie:

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

[ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) Metoda wymaga *przekształcenie* argumentu, który jest metoda wywołania zwrotnego. Dane wejściowe tego wywołania zwrotnego jest zawsze `double` od 0 do 1. W związku z tym `ColorTo` metoda definiuje własną transformacji `Func` która akceptuje `double` od 0 do 1 i który zwraca [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) wartość odpowiadająca tej wartości. `Color` Wartość jest obliczana na podstawie interpolacji [ `R` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/), [ `G` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/), [ `B` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/), i [ `A` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/) wartości dwa podane `Color` argumentów. `Color` Wartości są następnie przekazywane do metody wywołania zwrotnego dla aplikacji do określonej właściwości.

Takie podejście umożliwia `ColorTo` metody do animowania żadnego [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) właściwości, jak pokazano w poniższym przykładzie:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

W tym przykładzie kodu `ColorTo` animuje metody [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) i [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) właściwości [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), `BackgroundColor`właściwości strony oraz [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) właściwość [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/).

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób użycia [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) klasa do tworzenia i anulować animacji, synchronizowanie wielu animacji i utworzyć niestandardowe animacji, które animowania właściwości, które nie są animowane przez animację istniejących metody. `Animation` Klasa jest blokiem konstrukcyjnym wszystkie animacje platformy Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [Animacje niestandardowe (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [Animacja](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)
- [AnimationExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)
