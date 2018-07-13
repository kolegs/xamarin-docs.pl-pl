---
title: Przekazywanie parametrów efekt jako dołączone właściwości
description: Dołączone właściwości może służyć do definiują parametry efekt odpowiadanie na zmiany właściwości środowiska uruchomieniowego. W tym artykule przedstawiono, za pomocą dołączonych właściwości w celu przekazania parametrów do efektu i zmieniając parametr w czasie wykonywania.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 9483e424a74a88ce3f0eb49624bb5315551f2062
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996454"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Przekazywanie parametrów efekt jako dołączone właściwości

_Dołączone właściwości może służyć do definiują parametry efekt odpowiadanie na zmiany właściwości środowiska uruchomieniowego. W tym artykule przedstawiono, za pomocą dołączonych właściwości w celu przekazania parametrów do efektu i zmieniając parametr w czasie wykonywania._

Proces tworzenia parametrów efekt, które odpowiadanie na zmiany właściwości środowiska uruchomieniowego jest następująca:

1. Utwórz `static` klasę, która zawiera dołączoną właściwość dla każdego parametru, który zostanie przekazany do efektu.
1. Dodaj dodatkowe dołączoną właściwość do klasy, która będzie służyć do kontrolowania, dodawania lub usuwania efekt do kontrolki, która klasa zostanie dołączona do. Upewnij się, że to dołączone właściwości rejestrów `propertyChanged` delegata, która zostanie wykonana po zmianie wartości właściwości.
1. Utwórz `static` metod pobierających i ustawiających dla każdego dołączona właściwość.
1. Implementowanie logiki `propertyChanged` delegata do dodawania i usuwania skutków.
1. Implementowanie klasy zagnieżdżone wewnątrz `static` klasę o nazwie po efekt, które podklasy `RoutingEffect` klasy. Dla konstruktora należy wywołać konstruktora klasy bazowej, przekazując składa się z nazwy grupy rozdzielczość i unikatowy identyfikator, który został określony dla każdej klasy efekt specyficzne dla platformy.

Parametry mogą być następnie przekazywany do efekt, dodając dołączone właściwości i wartości właściwości do właściwej opcji kontroli. Oprócz parametrów można zmienić w czasie wykonywania, określając nową wartość dołączona właściwość.

> [!NOTE]
> Dołączona właściwość to specjalny rodzaj właściwości możliwej do wiązania, zdefiniowane w jedną klasę, ale dołączone do innych obiektów i rozpoznawalny w XAML jako atrybuty, które zawierają klasy i nazwy właściwości oddzielony kropką. Aby uzyskać więcej informacji, zobacz [dołączonych właściwości](~/xamarin-forms/xaml/attached-properties.md).

Przykładowa aplikacja demonstruje `ShadowEffect` dodająca cienia do tekstu wyświetlanego przez [ `Label` ](xref:Xamarin.Forms.Label) kontroli. Ponadto można zmienić kolor cienia w czasie wykonywania. Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](attached-properties-images/shadow-effect.png "Obowiązki projektu efekt w tle")

A [ `Label` ](xref:Xamarin.Forms.Label) kontrolować na `HomePage` dostosowanego przez `LabelShadowEffect` w każdym projekcie specyficzne dla platformy. Parametry są przekazywane do każdej `LabelShadowEffect` za pośrednictwem dołączonych właściwości `ShadowEffect` klasy. Każdy `LabelShadowEffect` klasa pochodzi od `PlatformEffect` klasy dla każdej platformy. Skutkuje to cienia do tekstu wyświetlanego przez dodawany `Label` kontrolować, jak pokazano na poniższych zrzutach ekranu:

![](attached-properties-images/screenshots.png "Efektem cienia na każdej platformie")

## <a name="creating-effect-parameters"></a>Tworzenie efektu parametrów

Element `static` klasy należy utworzyć reprezentują parametry efekt, jak pokazano w poniższym przykładzie kodu:

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

`ShadowEffect` Zawiera pięć dołączonych właściwości z `static` metod pobierających i ustawiających dla każdego dołączona właściwość. Cztery te właściwości reprezentują parametry do przekazania do każdego specyficzne dla platformy `LabelShadowEffect`. `ShadowEffect` Klasa definiuje również `HasShadow` dołączona właściwość, która służy do sterowania dodawania lub usuwania efekt do formantu, `ShadowEffect` klasy jest dołączony do. To dołączonych właściwości rejestrów `OnHasShadowChanged` metody, która zostanie wykonana po zmianie wartości właściwości. Ta metoda dodaje lub usuwa efektu na podstawie wartości z `HasShadow` dołączona właściwość.

Zagnieżdżony `LabelShadowEffect` klas i podklas, które [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) klasy obsługuje efekt Dodawanie i usuwanie. `RoutingEffect` Klasa reprezentuje efekt niezależne od platformy, która otacza efektu wewnętrzny, który jest zazwyczaj specyficzne dla platformy. Upraszcza to proces usuwania efektu, ponieważ brak jest czasu kompilacji dostępu do informacji o typie dla efektu specyficzne dla platformy. `LabelShadowEffect` Konstruktor wywołuje konstruktora klasy bazowej, przekazując w parametrze składający się z składa się z nazwy grupy rozdzielczość i unikatowy identyfikator, który został określony dla każdej klasy efekt specyficzne dla platformy. Dzięki temu wpływ dodawania i usuwania w `OnHasShadowChanged` metody w następujący sposób:

- **Dodawanie efektu** — nowe wystąpienie klasy `LabelShadowEffect` jest dodawany do formantu [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekcji. Spowoduje to zastąpienie przy użyciu [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) metodę, aby dodać ten efekt.
- **Wpływ usunięcia** — pierwsze wystąpienie `LabelShadowEffect` w formancie [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekcji są pobierane i usunięte.

## <a name="consuming-the-effect"></a>Korzystanie z wpływ

Każdy specyficzne dla platformy `LabelShadowEffect` mogą być używane przez dodanie właściwości dołączone do [ `Label` ](xref:Xamarin.Forms.Label) kontrolować, jak pokazano w poniższym przykładzie kodu XAML:

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

Odpowiednik [ `Label` ](xref:Xamarin.Forms.Label) w języku C# pokazano w poniższym przykładzie kodu:

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

Ustawienie `ShadowEffect.HasShadow` dołączonych właściwości `true` wykonuje `ShadowEffect.OnHasShadowChanged` metodę, która dodaje lub usuwa `LabelShadowEffect` do [ `Label` ](xref:Xamarin.Forms.Label) kontroli. W obu przykładach kodu `ShadowEffect.Color` dołączonej właściwości zawiera wartości kolorów specyficzne dla platformy. Aby uzyskać więcej informacji, zobacz [klasę urządzeń pamięci](~/xamarin-forms/platform/device.md).

Ponadto [ `Button` ](xref:Xamarin.Forms.Button) umożliwia kolor cienia, który ma zostać zmieniony w czasie wykonywania. Gdy `Button` kliknięciu, poniższy kod zmienia kolor cienia, ustawiając `ShadowEffect.Color` dołączona właściwość:

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Korzystanie z efekt przy użyciu stylu

Efekty, które mogą być wykorzystane przez dodanie właściwości dołączone do formantu, również mogą być używane przez stylu. Ilustruje poniższy przykład kodu XAML *jawne* stylu dla efektu w tle, które mogą być stosowane do [ `Label` ](xref:Xamarin.Forms.Label) kontrolki:

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

[ `Style` ](xref:Xamarin.Forms.Style) Mogą być stosowane do [ `Label` ](xref:Xamarin.Forms.Label) , ustawiając jego [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości `Style` przy użyciu `StaticResource`— rozszerzenie znaczników, jak pokazano w poniższym przykładzie kodu:

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Aby uzyskać więcej informacji na temat style zobacz [style](~/xamarin-forms/user-interface/styles/index.md).

## <a name="creating-the-effect-on-each-platform"></a>Tworzenie efektu na każdej platformie

W poniższych sekcjach omówiono implementacji specyficznych dla platformy `LabelShadowEffect` klasy.

### <a name="ios-project"></a>Projekt systemu iOS

Poniższy kod przedstawia przykład `LabelShadowEffect` implementację dla projektu systemu iOS:

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

`OnAttached` Metoda wywołuje metody, które pobierają wartości dołączonej właściwości, za pomocą `ShadowEffect` pobierające i które `Control.Layer` właściwości do wartości właściwości, aby utworzyć w tle. Ta funkcja jest opakowana w `try` / `catch` blokowania w przypadku, gdy formant, który efekt jest dołączony do nie ma `Control.Layer` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ żadne oczyszczanie jest wymagany.

#### <a name="responding-to-property-changes"></a>Reagowanie na zmiany właściwości

Jeśli dowolny z `ShadowEffect` dołączone zmiana wartości właściwości w czasie wykonywania, efekt musi odpowiedzieć, wyświetlając zmiany. Zastąpione wersję `OnElementPropertyChanged` metody w klasie efekt specyficzne dla platformy, to miejsce do reagowania na zmiany właściwości możliwej do wiązania, jak pokazano w poniższym przykładzie kodu:

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

`OnElementPropertyChanged` Metody aktualizacji usługi radius, kolor lub przesunięcie cienia, pod warunkiem, że odpowiednie `ShadowEffect` dołączonej właściwości wartość została zmieniona. Sprawdź właściwości, które uległy zmianie powinien zawsze się, jak to zastąpienie może być wywoływana wiele razy.

### <a name="android-project"></a>Projekt dla systemu android

Poniższy kod przedstawia przykład `LabelShadowEffect` implementacji projektu dla systemu Android:

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

`OnAttached` Metoda wywołuje metody, które pobierają wartości dołączonej właściwości, za pomocą `ShadowEffect` metody pobierające i wywołuje metodę, która wywołuje [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) metodą tworzenia w tle przy użyciu wartości właściwości. Ta funkcja jest opakowana w `try` / `catch` blokowania w przypadku, gdy formant, który efekt jest dołączony do nie ma `Control.Layer` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ żadne oczyszczanie jest wymagany.

#### <a name="responding-to-property-changes"></a>Reagowanie na zmiany właściwości

Jeśli dowolny z `ShadowEffect` dołączone zmiana wartości właściwości w czasie wykonywania, efekt musi odpowiedzieć, wyświetlając zmiany. Zastąpione wersję `OnElementPropertyChanged` metody w klasie efekt specyficzne dla platformy, to miejsce do reagowania na zmiany właściwości możliwej do wiązania, jak pokazano w poniższym przykładzie kodu:

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

`OnElementPropertyChanged` Metody aktualizacji usługi radius, kolor lub przesunięcie cienia, pod warunkiem, że odpowiednie `ShadowEffect` dołączonej właściwości wartość została zmieniona. Sprawdź właściwości, które uległy zmianie powinien zawsze się, jak to zastąpienie może być wywoływana wiele razy.

### <a name="universal-windows-platform-project"></a>Projekt dla platformy Universal Windows

Poniższy kod przedstawia przykład `LabelShadowEffect` implementacji dla projektu platformy uniwersalnej Windows (UWP):

```csharp
[assembly: ResolutionGroupName ("MyCompany")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
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

Platforma uniwersalna Windows nie zapewnia efektu w tle i dlatego `LabelShadowEffect` implementacji na obu platformach symuluje jeden, dodając przesunięcie drugiego [ `Label` ](xref:Xamarin.Forms.Label) za podstawowy `Label`. `OnAttached` Metoda tworzy nowy `Label` i ustawia niektóre właściwości układu w `Label`. Następnie wywołuje metody, które pobierają wartości dołączonej właściwości, za pomocą `ShadowEffect` metody pobierające i tworzy w tle, ustawiając [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX)i [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) właściwości w celu kontrolowania, kolor i położenie `Label`. `shadowLabel` Zostanie wstawiony przesunięcie za podstawowy `Label`. Ta funkcja jest opakowana w `try` / `catch` blokowania w przypadku, gdy formant, który efekt jest dołączony do nie ma `Control.Layer` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ żadne oczyszczanie jest wymagany.

#### <a name="responding-to-property-changes"></a>Reagowanie na zmiany właściwości

Jeśli dowolny z `ShadowEffect` dołączone zmiana wartości właściwości w czasie wykonywania, efekt musi odpowiedzieć, wyświetlając zmiany. Zastąpione wersję `OnElementPropertyChanged` metody w klasie efekt specyficzne dla platformy, to miejsce do reagowania na zmiany właściwości możliwej do wiązania, jak pokazano w poniższym przykładzie kodu:

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

`OnElementPropertyChanged` Metoda aktualizuje kolor lub przesunięcie cienia, pod warunkiem, że odpowiednie `ShadowEffect` dołączonej właściwości wartość została zmieniona. Sprawdź właściwości, które uległy zmianie powinien zawsze się, jak to zastąpienie może być wywoływana wiele razy.

## <a name="summary"></a>Podsumowanie

W tym artykule wykazała, za pomocą dołączonych właściwości w celu przekazania parametrów do efektu i zmieniając parametr w czasie wykonywania. Dołączone właściwości może służyć do definiują parametry efekt odpowiadanie na zmiany właściwości środowiska uruchomieniowego.


## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe programy renderujące](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Efekt](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [Efektem cienia (przykład)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
