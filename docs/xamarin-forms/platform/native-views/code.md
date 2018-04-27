---
title: Natywny widoków w języku C#
description: Natywny widoków z systemami iOS, Android i platformy uniwersalnej systemu Windows można odwoływać się bezpośrednio ze stron platformy Xamarin.Forms utworzone przy użyciu języka C#. W tym artykule przedstawiono sposób dodawania widoków natywnego układ platformy Xamarin.Forms utworzone przy użyciu języka C# oraz do zastąpienia układ widoki niestandardowe, aby poprawić ich pomiaru użycia interfejsu API.
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 46284fd1b0863f904e9f24f125aef75fe3eb8caa
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="native-views-in-c"></a>Natywny widoków w języku C#

_Natywny widoków z systemami iOS, Android i platformy uniwersalnej systemu Windows można odwoływać się bezpośrednio ze stron platformy Xamarin.Forms utworzone przy użyciu języka C#. W tym artykule przedstawiono sposób dodawania widoków natywnego układ platformy Xamarin.Forms utworzone przy użyciu języka C# oraz do zastąpienia układ widoki niestandardowe, aby poprawić ich pomiaru użycia interfejsu API._

## <a name="overview"></a>Omówienie

Wszystkie platformy Xamarin.Forms formant, który umożliwia `Content` ustawiania, lub `Children` kolekcji, można dodawać widoki specyficzne dla platformy. Na przykład iOS `UILabel` można bezpośrednio dodać do [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) właściwości, lub do [ `StackLayout.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) kolekcji. Jednak należy pamiętać, że ta funkcja wymaga użycia `#if` definiuje w rozwiązaniach platformy Xamarin.Forms projektu udostępnionego i nie jest dostępna z rozwiązań platformy Xamarin.Forms przenośnej biblioteki klasy (PCL).

Poniższe zrzuty ekranu pokazują widoki specyficzne dla platformy, został on dodany do platformy Xamarin.Forms [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/):

[![](code-images/screenshots-sml.png "StackLayout zawierający widoki specyficzne dla platformy")](code-images/screenshots.png#lightbox "StackLayout zawierający widoki specyficzne dla platformy")

Możliwość dodawania widoki specyficzne dla platformy układ platformy Xamarin.Forms jest włączona za pomocą dwóch metod rozszerzenia na każdej platformie:

- `Add` — dodaje specyficzne dla platformy w celu [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) kolekcji układu.
- `ToView` — Trwa widoku specyficzne dla platformy i koduje go jako platformy Xamarin.Forms [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) mogą być ustawiani jako `Content` właściwości formantu.

W projekcie udostępnionym platformy Xamarin.Forms przy użyciu tych metod wymaga importowania odpowiednich przestrzeń nazw platformy Xamarin.Forms specyficzne dla platformy:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Platforma uniwersalna systemu Windows (UWP)** — Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>Dodawanie widoków specyficzne dla platformy na każdej platformie

Poniższe sekcje przedstawiają sposób dodawania widoki specyficzne dla platformy układ platformy Xamarin.Forms na każdej z platform.

### <a name="ios"></a>iOS

Poniższy przykładowy kod przedstawia sposób dodawania `UILabel` do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) i [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

W przykładzie założono, że `stackLayout` i `contentView` wystąpień wcześniej zostały utworzone w języku XAML i C#.

### <a name="android"></a>Android

Poniższy przykładowy kod przedstawia sposób dodawania `TextView` do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) i [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

W przykładzie założono, że `stackLayout` i `contentView` wystąpień wcześniej zostały utworzone w języku XAML i C#.

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Poniższy przykładowy kod przedstawia sposób dodawania `TextBlock` do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) i [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

W przykładzie założono, że `stackLayout` i `contentView` wystąpień wcześniej zostały utworzone w języku XAML i C#.

## <a name="overriding-platform-measurements-for-custom-views"></a>Zastępowanie pomiarów platformy dla widoków niestandardowych

Niestandardowe widoki na każdej platformie często tylko prawidłowo zaimplementować miary dla scenariusza układ, dla którego zostały zaprojektowane. Na przykład widok niestandardowy może zostały zaprojektowane tylko zajmować połowy dostępnej szerokość urządzenia. Jednak po jest udostępniany innym użytkownikom, widok niestandardowy może wymagać zajmować pełne dostępne szerokość urządzenia. W związku z tym może być konieczne do przesłonięcia implementację miary niestandardowe widoki podczas ponownego użycia w układzie platformy Xamarin.Forms. Z tego powodu `Add` i `ToView` metody rozszerzenia udostępniają zastąpienia umożliwiających delegatów miary można określić, które można zastąpić widok niestandardowy układ, gdy jest ona dodawana do układu platformy Xamarin.Forms.

Poniższe sekcje pokazują, jak zastąpić układ widoki niestandardowe, aby poprawić ich pomiaru użycia interfejsu API.

### <a name="ios"></a>iOS

Poniższy kod przedstawia przykład `CustomControl` klasy, która dziedziczy `UILabel`:

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

Wystąpienie tego widoku jest dodawany do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), jak pokazano w poniższym przykładzie:

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

Jednak ponieważ `CustomControl.SizeThatFits` zastąpienie zawsze zwraca wysokość 150, zostanie wyświetlony widok z puste miejsce powyżej i poniżej, jak pokazano na poniższym zrzucie ekranu:

![](code-images/ios-bad-measurement.png "iOS CustomControl ze złą implementacją SizeThatFits")

Rozwiązanie tego problemu jest zapewnienie `GetDesiredSizeDelegate` implementacji, jak pokazano w poniższym przykładzie:

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

Ta metoda używa szerokość podał `CustomControl.SizeThatFits` metody, ale zastępuje wysokość 150 wysokości 70. Gdy `CustomControl` wystąpienia jest dodawany do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), `FixSize` metoda może być określona jako `GetDesiredSizeDelegate` ustalenie zły miary udostępnione przez `CustomControl` klasy:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Powoduje to widok niestandardowy są wyświetlane poprawnie, bez puste miejsce powyżej i poniżej, jak pokazano na poniższym zrzucie ekranu:

![](code-images/ios-good-measurement.png "iOS CustomControl za GetDesiredSize zastąpienia")

### <a name="android"></a>Android

Poniższy kod przedstawia przykład `CustomControl` klasy, która dziedziczy `TextView`:

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

Wystąpienie tego widoku jest dodawany do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), jak pokazano w poniższym przykładzie:

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

Jednak ponieważ `CustomControl.OnMeasure` zastąpienie zawsze zwraca informacje o połowę żądanej szerokości, zostanie wyświetlony widok zajmują tylko pełnej szerokości dostępne urządzenia, jak pokazano na poniższym zrzucie ekranu:

![](code-images/android-bad-measurement.png "Android CustomControl z implementacją zły OnMeasure")

Rozwiązanie tego problemu jest zapewnienie `GetDesiredSizeDelegate` implementacji, jak pokazano w poniższym przykładzie:

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

Ta metoda używa szerokość podał `CustomControl.OnMeasure` metody, ale mnożona przez dwa. Gdy `CustomControl` wystąpienia jest dodawany do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), `FixSize` metoda może być określona jako `GetDesiredSizeDelegate` ustalenie zły miary udostępnione przez `CustomControl` klasy:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Powoduje to jest widok niestandardowy wyświetlane prawidłowo, zajmujące szerokość urządzenia, jak pokazano na poniższym zrzucie ekranu:

![](code-images/android-good-measurement.png "Android CustomControl z obiektem delegowanym GetDesiredSize niestandardowych")

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Poniższy kod przedstawia przykład `CustomControl` klasy, która dziedziczy `Panel`:

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

Wystąpienie tego widoku jest dodawany do [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), jak pokazano w poniższym przykładzie:

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

Jednak ponieważ `CustomControl.ArrangeOverride` zastąpienie zawsze zwraca informacje o połowę żądanej szerokości, widoku zostaną przycięte do pełnej szerokości dostępne urządzenia, jak pokazano na poniższym zrzucie ekranu:

![](code-images/winrt-bad-measurement.png "Formant niestandardowy platformy uniwersalnej systemu Windows z implementacją zły ArrangeOverride")

Rozwiązanie tego problemu jest zapewnienie `ArrangeOverrideDelegate` implementacji, podczas dodawania widoku umożliwia [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), jak pokazano w poniższym przykładzie:

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

Ta metoda używa szerokość podał `CustomControl.ArrangeOverride` metody, ale mnożona przez dwa. Powoduje to jest widok niestandardowy wyświetlane prawidłowo, zajmujące szerokość urządzenia, jak pokazano na poniższym zrzucie ekranu:

![](code-images/winrt-good-measurement.png "Formant niestandardowy platformy uniwersalnej systemu Windows z obiektem delegowanym ArrangeOverride")

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak dodać widoków natywnego układ platformy Xamarin.Forms utworzone przy użyciu języka C# i jak układ widoki niestandardowe, aby poprawić ich pomiaru użycia interfejsu API.


## <a name="related-links"></a>Linki pokrewne

- [NativeEmbedding (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)
- [Formularze natywne](~/xamarin-forms/platform/native-forms.md)
