---
title: "Przekazywanie parametrów efekt jako dołączone właściwości"
description: "Dołączone właściwości umożliwia zdefiniowanie parametrów efekt, które odpowiadają na zmiany właściwości środowiska uruchomieniowego. W tym artykule przedstawiono przy użyciu dołączonych właściwości do przekazania parametrów do efektu i zmianę parametru w czasie wykonywania."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 585d0422b4dc2b35fc8ba50ed82d2d34e53a784e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Przekazywanie parametrów efekt jako dołączone właściwości

_Dołączone właściwości umożliwia zdefiniowanie parametrów efekt, które odpowiadają na zmiany właściwości środowiska uruchomieniowego. W tym artykule przedstawiono przy użyciu dołączonych właściwości do przekazania parametrów do efektu i zmianę parametru w czasie wykonywania._

Proces tworzenia skutku parametrów, które odpowiadają na zmiany właściwości środowiska uruchomieniowego jest następujący:

1. Utwórz `static` klasę, która zawiera dołączona właściwość dla każdego parametru do przekazania do efekt.
1. Dodaj dodatkowe właściwości dołączonej do klasy, która będzie służyć do kontrolowania dodawania lub usuwania efekt kontrolkę, która klasa zostanie dołączona do. Upewnij się, że to dołączona właściwość rejestrów `propertyChanged` delegata, która zostanie wykonana po zmianie wartości właściwości.
1. Utwórz `static` pobierające i ustawiające dla każdego dołączona właściwość.
1. Wdrożyć logikę w `propertyChanged` delegata do dodawania i usuwania skutków.
1. Implementuje wewnątrz klasy zagnieżdżonej `static` klasy, nosi efekt, który podklasy `RoutingEffect` klasy. Dla konstruktora wywołanie konstruktora klasy podstawowej, przekazując łączenia rozpoznawania nazwy grupy i unikatowy identyfikator, który został określony w każdej klasie efekt specyficzne dla platformy.

Następnie parametry mogą być przekazywane w celu przez dodanie dołączone właściwości i wartości właściwości do właściwej opcji kontroli. Ponadto parametrów można zmienić w czasie wykonywania przez podanie nowej wartości właściwości dołączonych.

> [!NOTE]
> Dołączona właściwość jest specjalnym rodzajem właściwości możliwej do wiązania, zdefiniowana w jedną klasę, ale dołączone do innych obiektów i rozpoznawalną w języku XAML jako atrybuty, które zawiera klasy i nazwę właściwości oddzielone kropką. Aby uzyskać więcej informacji, zobacz [dołączonych właściwości](~/xamarin-forms/xaml/attached-properties.md).

Aplikacja przykładowa prezentuje `ShadowEffect` dodaje cienia do tekstu wyświetlanego przez [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) formantu. Ponadto można zmienić kolor cienia w czasie wykonywania. Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](attached-properties-images/shadow-effect.png "Obowiązki tle wpływ projektu:")

A [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) kontrolować na `HomePage` jest dostosowane przez `LabelShadowEffect` w każdym projekcie specyficzne dla platformy. Parametry są przekazywane do każdej `LabelShadowEffect` za pośrednictwem dołączone właściwości w `ShadowEffect` klasy. Każdy `LabelShadowEffect` pochodną klasy `PlatformEffect` klasy dla każdej platformy. Powoduje to cienia dodawany do tekstu wyświetlanego przez `Label` kontroli, jak pokazano na poniższych zrzutach ekranu:

![](attached-properties-images/screenshots.png "Efektem cienia na każdej platformie")

## <a name="creating-effect-parameters"></a>Tworzenie skutku parametrów

A `static` klasy należy utworzyć do przedstawiania parametrów efekt, jak pokazano w poniższym przykładzie:

```csharp
public static class ShadowEffect
{
  public static readonly BindableProperty HasShadowProperty =
    BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false, propertyChanged: OnHasShadowChanged);
  public static readonly BindableProperty ColorProperty =
    BindableProperty.CreateAttached ("Color", typeof(Color), typeof(ShadowEffect), Color.Default);
  public static readonly BindableProperty RadiusProperty =
    BindableProperty.CreateAttached ("Radius", typeof(double), typeof(ShadowEffect), 1.0);
  public static readonly BindableProperty DistanceXProperty =
    BindableProperty.CreateAttached ("DistanceX", typeof(double), typeof(ShadowEffect), 0.0);
  public static readonly BindableProperty DistanceYProperty =
    BindableProperty.CreateAttached ("DistanceY", typeof(double), typeof(ShadowEffect), 0.0);

  public static bool GetHasShadow (BindableObject view)
  {
    return (bool)view.GetValue (HasShadowProperty);
  }

  public static void SetHasShadow (BindableObject view, bool value)
  {
    view.SetValue (HasShadowProperty, value);
  }
  ...

  static void OnHasShadowChanged (BindableObject bindable, object oldValue, object newValue)
  {
    var view = bindable as View;
    if (view == null) {
      return;
    }

    bool hasShadow = (bool)newValue;
    if (hasShadow) {
      view.Effects.Add (new LabelShadowEffect ());
    } else {
      var toRemove = view.Effects.FirstOrDefault (e => e is LabelShadowEffect);
      if (toRemove != null) {
        view.Effects.Remove (toRemove);
      }
    }
  }

  class LabelShadowEffect : RoutingEffect
  {
    public LabelShadowEffect () : base ("MyCompany.LabelShadowEffect")
    {
    }
  }
}
```

`ShadowEffect` Zawiera pięć dołączonych właściwości z `static` pobierające i ustawiające dla każdego dołączona właściwość. Cztery z tymi właściwościami reprezentują parametry do przekazania do każdego specyficzne dla platformy `LabelShadowEffect`. `ShadowEffect` Klasy definiuje również `HasShadow` dołączona właściwość, która jest używana do sterowania Dodawanie lub usuwanie efektu do formantu który `ShadowEffect` klasy jest dołączony do. To dołączona właściwość rejestrów `OnHasShadowChanged` — metoda, która zostanie wykonana po zmianie wartości właściwości. Ta metoda dodaje lub usuwa wpływu na podstawie wartości z `HasShadow` dołączona właściwość.

Zagnieżdżone `LabelShadowEffect` klasy, które podklasy [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) klasy obsługuje efekt Dodawanie i usuwanie. `RoutingEffect` Klasa reprezentuje efekt niezależne od platformy, który opakowuje wewnętrzny wpływ, jaki jest zazwyczaj specyficzne dla platformy. Upraszcza proces usuwania efektu, ponieważ nie istnieje żadne kompilacji dostęp do informacji o typie dla efektu specyficzne dla platformy. `LabelShadowEffect` Konstruktor wywołuje konstruktor klasy podstawowej, przekazując parametr składające się z łączenia rozpoznawania nazwy grupy i unikatowy identyfikator, który został określony w każdej klasie efekt specyficzne dla platformy. Dzięki temu efekt Dodawanie i usuwanie w `OnHasShadowChanged` metody, w następujący sposób:

- **Dodanie efektu** — nowe wystąpienie klasy `LabelShadowEffect` zostanie dodany do formantu [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekcji. Spowoduje to zastąpienie przy użyciu [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) metodę, aby dodać ten efekt.
- **Wpływ usunięcia** — pierwsze wystąpienie `LabelShadowEffect` w formancie [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekcji jest pobierany i usunąć.

## <a name="consuming-the-effect"></a>Korzystanie z efektu

Każdy specyficzne dla platformy `LabelShadowEffect` mogą być używane przez dodanie dołączonych właściwości do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) kontroli, jak pokazano w poniższym przykładzie kodu XAML:

```xaml
<Label Text="Label Shadow Effect" ...
       local:ShadowEffect.HasShadow="true" local:ShadowEffect.Radius="5"
       local:ShadowEffect.DistanceX="5" local:ShadowEffect.DistanceY="5">
  <local:ShadowEffect.Color>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="Black" />
        <On Platform="Android" Value="White" />
        <On Platform="UWP" Value="Red" />
    </OnPlatform>
  </local:ShadowEffect.Color>
</Label>
```

Odpowiednik [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) w języku C# przedstawiono w poniższym przykładzie:

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};

Color color = Color.Default;
switch (Device.RuntimePlatform)
{
    case Device.iOS:
        color = Color.Black;
        break;
    case Device.Android:
        color = Color.White;
        break;
    case Device.UWP:
        color = Color.Red;
        break;
}

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

Ustawienie `ShadowEffect.HasShadow` dołączona właściwość do `true` wykonuje `ShadowEffect.OnHasShadowChanged` metodę, która dodaje lub usuwa `LabelShadowEffect` do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) formantu. W obu przykłady kodu `ShadowEffect.Color` dołączona właściwość udostępnia wartości kolorów specyficzne dla platformy. Aby uzyskać więcej informacji, zobacz [klasę urządzeń](~/xamarin-forms/platform/device.md).

Ponadto [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) umożliwia kolor cienia zostanie zmieniony w czasie wykonywania. Gdy `Button` zostanie kliknięty poniższy kod zmienia kolor cienia przez ustawienie `ShadowEffect.Color` dołączona właściwość:

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Wykorzystywanie obowiązywać przy użyciu stylu

Efekty, które mogą być używane przez dodanie dołączonych właściwości do formantu również mogą być używane przez styl. Przedstawia poniższy przykładowy kod XAML *jawne* styl efektem cienia, który można zastosować do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) kontrolki:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="True" />
    <Setter Property="local:ShadowEffect.Radius" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceX" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceY" Value="5" />
  </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Można zastosować do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) przez ustawienie jej [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości `Style` wystąpienia przy użyciu `StaticResource`— rozszerzenie znaczników, jak pokazano w poniższym przykładzie:

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Aby uzyskać więcej informacji na temat stylów, zobacz [style](~/xamarin-forms/user-interface/styles/index.md).

## <a name="creating-the-effect-on-each-platform"></a>Tworzenie wpływu na każdej platformie

W poniższych sekcjach omówiono implementacja specyficzna dla platformy `LabelShadowEffect` klasy.

### <a name="ios-project"></a>iOS projektu

Poniższy kod przedstawia przykład `LabelShadowEffect` implementacji dla projektu iOS:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                Control.Layer.ShadowOpacity = 1.0f;
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateRadius ()
        {
            Control.Layer.CornerRadius = (nfloat)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            Control.Layer.ShadowColor = ShadowEffect.GetColor (Element).ToCGColor ();
        }

        void UpdateOffset ()
        {
            Control.Layer.ShadowOffset = new CGSize (
                (double)ShadowEffect.GetDistanceX (Element),
                (double)ShadowEffect.GetDistanceY (Element));
        }
    }
```

`OnAttached` Metoda wywołuje metody, które służą do pobierania wartości właściwości dołączonych przy użyciu `ShadowEffect` pobierające i który ustawiony `Control.Layer` właściwości do wartości właściwości, aby utworzyć cień. Ta funkcja jest ujęte w `try` / `catch` blokowania w przypadku, gdy formant, który efekt jest dołączony do nie ma `Control.Layer` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ czyszczenie nie jest niezbędne.

#### <a name="responding-to-property-changes"></a>Odpowiada na żądania zmiany właściwości

Jeśli dowolny z `ShadowEffect` dołączony zmiany wartości właściwości w czasie wykonywania, potrzeb efekt odpowiedzieć, wyświetlając zmiany. Zastąpiona wersja `OnElementPropertyChanged` metody w klasie specyficzne dla platformy efekt jest używana do reagowania na zmiany właściwości możliwej do wiązania, jak pokazano w poniższym przykładzie:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

`OnElementPropertyChanged` Metody aktualizacji usługi radius, kolor lub przesunięcie cienia, pod warunkiem, że odpowiednie `ShadowEffect` dołączona właściwość wartość została zmieniona. Sprawdź właściwości, które uległy zmianie zawsze należy, jak to zastąpienie może zostać wywołana wiele razy.

### <a name="android-project"></a>Projekt systemu android

Poniższy kod przedstawia przykład `LabelShadowEffect` implementacji dla projektu Android:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        Android.Widget.TextView control;
        Android.Graphics.Color color;
        float radius, distanceX, distanceY;

        protected override void OnAttached ()
        {
            try {
                control = Control as Android.Widget.TextView;
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                UpdateControl ();
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateControl ()
        {
            if (control != null) {
                control.SetShadowLayer (radius, distanceX, distanceY, color);
            }
        }

        void UpdateRadius ()
        {
            radius = (float)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            color = ShadowEffect.GetColor (Element).ToAndroid ();
        }

        void UpdateOffset ()
        {
            distanceX = (float)ShadowEffect.GetDistanceX (Element);
            distanceY = (float)ShadowEffect.GetDistanceY (Element);
        }
    }
```

`OnAttached` Metoda wywołuje metody, które służą do pobierania wartości właściwości dołączonych przy użyciu `ShadowEffect` metody pobierające i wywołuje metodę, która wywołuje [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) metodę w celu utworzenia cienia przy użyciu wartości właściwości. Ta funkcja jest ujęte w `try` / `catch` blokowania w przypadku, gdy formant, który efekt jest dołączony do nie ma `Control.Layer` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ czyszczenie nie jest niezbędne.

#### <a name="responding-to-property-changes"></a>Odpowiada na żądania zmiany właściwości

Jeśli dowolny z `ShadowEffect` dołączony zmiany wartości właściwości w czasie wykonywania, potrzeb efekt odpowiedzieć, wyświetlając zmiany. Zastąpiona wersja `OnElementPropertyChanged` metody w klasie specyficzne dla platformy efekt jest używana do reagowania na zmiany właściwości możliwej do wiązania, jak pokazano w poniższym przykładzie:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
      UpdateControl ();
    }
  }
  ...
}
```

`OnElementPropertyChanged` Metody aktualizacji usługi radius, kolor lub przesunięcie cienia, pod warunkiem, że odpowiednie `ShadowEffect` dołączona właściwość wartość została zmieniona. Sprawdź właściwości, które uległy zmianie zawsze należy, jak to zastąpienie może zostać wywołana wiele razy.

### <a name="windows-phone--universal-windows-platform-projects"></a>Windows Phone & projekty platformy uniwersalnej systemu Windows

Poniższy kod przedstawia przykład `LabelShadowEffect` implementacji dla projektów Windows Phone i Windows platformy Uniwersalnej:

```csharp
[assembly: ResolutionGroupName ("MyCompany")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.WinPhone81
{
    public class LabelShadowEffect : PlatformEffect
    {
        Label shadowLabel;
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;

                    shadowLabel = new Label ();
                    shadowLabel.Text = textBlock.Text;
                    shadowLabel.FontAttributes = FontAttributes.Bold;
                    shadowLabel.HorizontalOptions = LayoutOptions.Center;
                    shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;

                    UpdateColor ();
                    UpdateOffset ();

                    ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                    shadowAdded = true;
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateColor ()
        {
            shadowLabel.TextColor = ShadowEffect.GetColor (Element);
        }

        void UpdateOffset ()
        {
            shadowLabel.TranslationX = ShadowEffect.GetDistanceX (Element);
            shadowLabel.TranslationY = ShadowEffect.GetDistanceY (Element);
        }
    }
}
```

Środowisko wykonawcze systemu Windows i platformy uniwersalnej systemu Windows nie oferują efekt cieniowania i dlatego element `LabelShadowEffect` implementacji na obu platform symuluje jedną przez dodanie przesunięcia drugi [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) za podstawowy `Label`. `OnAttached` Metoda tworzy nowy `Label` i ustawia niektóre właściwości układu `Label`. Następnie wywołuje metody, które służą do pobierania wartości właściwości dołączonych przy użyciu `ShadowEffect` metody pobierające i tworzy cień przez ustawienie [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)i [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) właściwości, aby kontrolować kolorów i lokalizację `Label`. `shadowLabel` Zostanie wstawiony przesunięcie za podstawowy `Label`. Ta funkcja jest ujęte w `try` / `catch` blokowania w przypadku, gdy formant, który efekt jest dołączony do nie ma `Control.Layer` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ czyszczenie nie jest niezbędne.

#### <a name="responding-to-property-changes"></a>Odpowiada na żądania zmiany właściwości

Jeśli dowolny z `ShadowEffect` dołączony zmiany wartości właściwości w czasie wykonywania, potrzeb efekt odpowiedzieć, wyświetlając zmiany. Zastąpiona wersja `OnElementPropertyChanged` metody w klasie specyficzne dla platformy efekt jest używana do reagowania na zmiany właściwości możliwej do wiązania, jak pokazano w poniższym przykładzie:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
                      args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

`OnElementPropertyChanged` Metody aktualizacji kolor lub przesunięcie cienia, pod warunkiem, że odpowiednie `ShadowEffect` dołączona właściwość wartość została zmieniona. Sprawdź właściwości, które uległy zmianie zawsze należy, jak to zastąpienie może zostać wywołana wiele razy.

## <a name="summary"></a>Podsumowanie

W tym artykule wykazała, za pomocą dołączonych właściwości do przekazania parametrów do efektu i zmianę parametru w czasie wykonywania. Dołączone właściwości umożliwia zdefiniowanie parametrów efekt, które odpowiadają na zmiany właściwości środowiska uruchomieniowego.


## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe programy renderujące](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effect](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [Efektem cienia (przykład)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
