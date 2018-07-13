---
title: Widoki natywne w języku C#
description: Widoki natywne z systemami iOS, Android i platformy uniwersalnej systemu Windows można odwoływać się bezpośrednio ze stron Xamarin.Forms utworzone przy użyciu języka C#. W tym artykule przedstawiono sposób dodawania widoki natywne do układu Xamarin.Forms utworzone przy użyciu języka C# oraz jak przesłonić układ widoki niestandardowe, aby poprawić ich pomiaru użycia interfejsu API.
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: ad633f49c1c448529fa4c2b50483ec233c1ee841
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996197"
---
# <a name="native-views-in-c"></a>Widoki natywne w języku C#

_Widoki natywne z systemami iOS, Android i platformy uniwersalnej systemu Windows można odwoływać się bezpośrednio ze stron Xamarin.Forms utworzone przy użyciu języka C#. W tym artykule przedstawiono sposób dodawania widoki natywne do układu Xamarin.Forms utworzone przy użyciu języka C# oraz jak przesłonić układ widoki niestandardowe, aby poprawić ich pomiaru użycia interfejsu API._

## <a name="overview"></a>Omówienie

Formant dowolnego zestawu narzędzi Xamarin.Forms, który umożliwia `Content` ustawiania, lub `Children` kolekcji można dodawać widoki specyficzne dla platformy. Na przykład dla systemu iOS `UILabel` można bezpośrednio dodać do [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) właściwości lub [ `StackLayout.Children` ](xref:Xamarin.Forms.Layout`1.Children) kolekcji. Należy jednak zauważyć, że ta funkcja wymaga użycia `#if` definiuje w rozwiązaniach projektu udostępnionego zestawu narzędzi Xamarin.Forms, a nie jest dostępna przy użyciu zestawu narzędzi Xamarin.Forms .NET Standard biblioteki.

Poniższe zrzuty ekranu pokazują specyficzne dla platformy widoków, został on dodany do zestawu narzędzi Xamarin.Forms [ `StackLayout` ](xref:Xamarin.Forms.StackLayout):

[![](code-images/screenshots-sml.png "StackLayout zawierają widoki specyficzne dla platformy")](code-images/screenshots.png#lightbox "StackLayout zawierają widoki specyficzne dla platformy")

Możliwość dodawania widoki specyficzne dla platformy do układu Xamarin.Forms jest włączone za pomocą dwóch metod rozszerzenia na każdej z platform:

- `Add` — Dodaje widok specyficzny dla platformy, który chcesz [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) kolekcja układzie.
- `ToView` — Trwa widoku specyficzne dla platformy i otacza go jako rozwiązanie Xamarin.Forms [ `View` ](xref:Xamarin.Forms.View) może być ustawiona jako `Content` właściwości formantu.

W projekcie udostępnionym Xamarin.Forms przy użyciu tych metod wymaga importowania odpowiedniego obszaru nazw zestawu narzędzi Xamarin.Forms specyficzne dla platformy:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Platforma Universal Windows (UWP)** — Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>Dodawanie widoków specyficzne dla platformy, na każdej platformie

W poniższych sekcjach przedstawiono sposób dodawania widoki specyficzne dla platformy do układu Xamarin.Forms na każdej platformie.

### <a name="ios"></a>iOS

Poniższy przykład kodu demonstruje sposób dodawania `UILabel` do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) i [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
var uiLabel = new UILabel {
  MinimumFontSize = 14f,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = originalText,
};
stackLayout.Children.Add (uiLabel);
contentView.Content = uiLabel.ToView();
```

W przykładzie założono, że `stackLayout` i `contentView` wystąpienia zostały utworzone wcześniej w XAML lub C#.

### <a name="android"></a>Android

Poniższy przykład kodu demonstruje sposób dodawania `TextView` do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) i [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

W przykładzie założono, że `stackLayout` i `contentView` wystąpienia zostały utworzone wcześniej w XAML lub C#.

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Poniższy przykład kodu demonstruje sposób dodawania `TextBlock` do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) i [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
var textBlock = new TextBlock
{
    Text = originalText,
    FontSize = 14,
    FontFamily = new FontFamily("HelveticaNeue"),
    TextWrapping = TextWrapping.Wrap
};
stackLayout.Children.Add(textBlock);
contentView.Content = textBlock.ToView();
```

W przykładzie założono, że `stackLayout` i `contentView` wystąpienia zostały utworzone wcześniej w XAML lub C#.

## <a name="overriding-platform-measurements-for-custom-views"></a>Zastępowanie pomiarów platformy dla widoków niestandardowych

Widoki niestandardowe na każdej platformie często tylko prawidłowo zaimplementować miary dla scenariusza układ, dla którego zostały zaprojektowane. Na przykład niestandardowy widok mogły być zaprojektowane tylko zajmować połowę szerokości dostępne urządzenia. Jednak po są udostępniane innym użytkownikom, widok niestandardowy może być wymagane zajmować pełne dostępne szerokość urządzenia. W związku z tym może być konieczne do przesłonięcia implementację pomiaru widoków niestandardowych podczas ponownego użycia w układzie zestawu narzędzi Xamarin.Forms. Z tego powodu `Add` i `ToView` metody rozszerzenia udostępniają zastąpienia, umożliwiające delegatów pomiaru należy określić, które można przesłonić układ widoku niestandardowego, gdy jest ona dodawana do układu zestawu narzędzi Xamarin.Forms.

Poniższe sekcje pokazują, jak zastąpić układ widoki niestandardowe, aby poprawić ich pomiaru użycia interfejsu API.

### <a name="ios"></a>iOS

Poniższy kod przedstawia przykład `CustomControl` klasy, która dziedziczy po elemencie `UILabel`:

```csharp
public class CustomControl : UILabel
{
  public override string Text {
    get { return base.Text; }
    set { base.Text = value.ToUpper (); }
  }

  public override CGSize SizeThatFits (CGSize size)
  {
    return new CGSize (size.Width, 150);
  }
}
```

Wystąpienie tego widoku jest dodawany do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), jak pokazano w poniższym przykładzie kodu:

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

Jednak ponieważ `CustomControl.SizeThatFits` zastąpienie zawsze zwraca wartość wysokości 150, zostanie wyświetlony widok przy użyciu puste miejsce powyżej i poniżej, jak pokazano na poniższym zrzucie ekranu:

![](code-images/ios-bad-measurement.png "Formant niestandardowy, za pomocą złą implementacją SizeThatFits systemu iOS")

Rozwiązanie tego problemu jest zapewnienie `GetDesiredSizeDelegate` wdrażania, jak pokazano w poniższym przykładzie kodu:

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, double width, double height)
{
  var uiView = renderer.Control;

  if (uiView == null) {
    return null;
  }

  var constraint = new CGSize (width, height);

  // Let the CustomControl determine its size (which will be wrong)
  var badRect = uiView.SizeThatFits (constraint);

  // Use the width and substitute the height
  return new SizeRequest (new Size (badRect.Width, 70));
}
```

Ta metoda używa szerokość związaną z `CustomControl.SizeThatFits` metody, ale zastępuje wysokości 150 wysokości 70. Gdy `CustomControl` wystąpienia jest dodawany do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), `FixSize` metoda może być określona jako `GetDesiredSizeDelegate` naprawić zły miary, dostarczone przez `CustomControl` klasy:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Skutkuje to widoku niestandardowego są wyświetlane poprawnie, bez puste miejsce powyżej i poniżej, jak pokazano na poniższym zrzucie ekranu:

![](code-images/ios-good-measurement.png "Formant niestandardowy, za pomocą zastąpienia GetDesiredSize systemu iOS")

### <a name="android"></a>Android

Poniższy kod przedstawia przykład `CustomControl` klasy, która dziedziczy po elemencie `TextView`:

```csharp
public class CustomControl : TextView
{
  public CustomControl (Context context) : base (context)
  {
  }

  protected override void OnMeasure (int widthMeasureSpec, int heightMeasureSpec)
  {
    int width = MeasureSpec.GetSize (widthMeasureSpec);

    // Force the width to half of what's been requested.
    // This is deliberately wrong to demonstrate providing an override to fix it with.
    int widthSpec = MeasureSpec.MakeMeasureSpec (width / 2, MeasureSpec.GetMode (widthMeasureSpec));

    base.OnMeasure (widthSpec, heightMeasureSpec);
  }
}
```

Wystąpienie tego widoku jest dodawany do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), jak pokazano w poniższym przykładzie kodu:

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

Jednak ponieważ `CustomControl.OnMeasure` zastąpienie zawsze zwraca informacje o połowę szerokości żądanego, zostanie wyświetlony widok zajmuje tylko połowę szerokości dostępne urządzenia, jak pokazano na poniższym zrzucie ekranu:

![](code-images/android-bad-measurement.png "Android formant niestandardowy z implementacją zły OnMeasure")

Rozwiązanie tego problemu jest zapewnienie `GetDesiredSizeDelegate` wdrażania, jak pokazano w poniższym przykładzie kodu:

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, int widthConstraint, int heightConstraint)
{
  var nativeView = renderer.Control;

  if ((widthConstraint == 0 && heightConstraint == 0) || nativeView == null) {
    return null;
  }

  int width = Android.Views.View.MeasureSpec.GetSize (widthConstraint);
  int widthSpec = Android.Views.View.MeasureSpec.MakeMeasureSpec (
    width * 2, Android.Views.View.MeasureSpec.GetMode (widthConstraint));
  nativeView.Measure (widthSpec, heightConstraint);
  return new SizeRequest (new Size (nativeView.MeasuredWidth, nativeView.MeasuredHeight));
}
```

Ta metoda używa szerokość związaną z `CustomControl.OnMeasure` metody, ale mnożona przez dwa. Gdy `CustomControl` wystąpienia jest dodawany do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), `FixSize` metoda może być określona jako `GetDesiredSizeDelegate` naprawić zły miary, dostarczone przez `CustomControl` klasy:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Skutkuje to widok niestandardowy, są wyświetlane prawidłowo, zajmuje szerokość urządzenia, jak pokazano na poniższym zrzucie ekranu:

![](code-images/android-good-measurement.png "Android formant niestandardowy z delegatem GetDesiredSize niestandardowe")

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Poniższy kod przedstawia przykład `CustomControl` klasy, która dziedziczy po elemencie `Panel`:

```csharp
public class CustomControl : Panel
{
  public static readonly DependencyProperty TextProperty =
    DependencyProperty.Register(
      "Text", typeof(string), typeof(CustomControl), new PropertyMetadata(default(string), OnTextPropertyChanged));

  public string Text
  {
    get { return (string)GetValue(TextProperty); }
    set { SetValue(TextProperty, value.ToUpper()); }
  }

  readonly TextBlock textBlock;

  public CustomControl()
  {
    textBlock = new TextBlock
    {
      MinHeight = 0,
      MaxHeight = double.PositiveInfinity,
      MinWidth = 0,
      MaxWidth = double.PositiveInfinity,
      FontSize = 14,
      TextWrapping = TextWrapping.Wrap,
      VerticalAlignment = VerticalAlignment.Center
    };

    Children.Add(textBlock);
  }

  static void OnTextPropertyChanged(DependencyObject dependencyObject, DependencyPropertyChangedEventArgs args)
  {
    ((CustomControl)dependencyObject).textBlock.Text = (string)args.NewValue;
  }

  protected override Size ArrangeOverride(Size finalSize)
  {
      // This is deliberately wrong to demonstrate providing an override to fix it with.
      textBlock.Arrange(new Rect(0, 0, finalSize.Width/2, finalSize.Height));
      return finalSize;
  }

  protected override Size MeasureOverride(Size availableSize)
  {
      textBlock.Measure(availableSize);
      return new Size(textBlock.DesiredSize.Width, textBlock.DesiredSize.Height);
  }
}
```

Wystąpienie tego widoku jest dodawany do [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), jak pokazano w poniższym przykładzie kodu:

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

Jednak ponieważ `CustomControl.ArrangeOverride` zastąpienie zawsze zwraca informacje o połowę szerokości żądanego, widoku zostaną przycięte do połowę szerokości dostępne urządzenia, jak pokazano na poniższym zrzucie ekranu:

![](code-images/winrt-bad-measurement.png "Formant niestandardowy platformy uniwersalnej systemu Windows, z implementacją zły ArrangeOverride")

Rozwiązanie tego problemu jest zapewnienie `ArrangeOverrideDelegate` implementacji, podczas dodawania widoku umożliwia [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), jak pokazano w poniższym przykładzie kodu:

```csharp
stackLayout.Children.Add(fixedControl, arrangeOverrideDelegate: (renderer, finalSize) =>
{
    if (finalSize.Width <= 0 || double.IsInfinity(finalSize.Width))
    {
        return null;
    }
    var frameworkElement = renderer.Control;
    frameworkElement.Arrange(new Rect(0, 0, finalSize.Width * 2, finalSize.Height));
    return finalSize;
});
```

Ta metoda używa szerokość związaną z `CustomControl.ArrangeOverride` metody, ale mnożona przez dwa. Skutkuje to widok niestandardowy, są wyświetlane prawidłowo, zajmuje szerokość urządzenia, jak pokazano na poniższym zrzucie ekranu:

![](code-images/winrt-good-measurement.png "Formant niestandardowy platformy uniwersalnej systemu Windows, za pomocą delegowanego ArrangeOverride")

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak dodać widoki natywne do układu Xamarin.Forms utworzone przy użyciu języka C# oraz jak przesłonić układ widoki niestandardowe, aby poprawić ich pomiaru użycia interfejsu API.


## <a name="related-links"></a>Linki pokrewne

- [NativeEmbedding (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)
- [Formularze natywne](~/xamarin-forms/platform/native-forms.md)
