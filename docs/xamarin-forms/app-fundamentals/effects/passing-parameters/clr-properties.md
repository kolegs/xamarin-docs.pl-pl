---
title: "Przekazywanie parametrów efekt jako właściwości środowiska uruchomieniowego języka wspólnego"
description: "Wspólne właściwości środowiska uruchomieniowego języka wspólnego (CLR) umożliwia zdefiniowanie parametrów efekt, które nie odpowiadają na zmiany właściwości środowiska uruchomieniowego. W tym artykule przedstawiono przy użyciu właściwości CLR do przekazania parametrów do efektu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: afe30ae87aa2e465013eb7fef3089cf701d98da6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>Przekazywanie parametrów efekt jako właściwości środowiska uruchomieniowego języka wspólnego

_Wspólne właściwości środowiska uruchomieniowego języka wspólnego (CLR) umożliwia zdefiniowanie parametrów efekt, które nie odpowiadają na zmiany właściwości środowiska uruchomieniowego. W tym artykule przedstawiono przy użyciu właściwości CLR do przekazania parametrów do efektu._

Proces tworzenia efekt parametrów, które nie odpowiadają na zmiany właściwości środowiska uruchomieniowego jest następujący:

1. Utwórz `public` klasy tego podklasy [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) klasy. `RoutingEffect` Klasa reprezentuje efekt niezależne od platformy, który opakowuje wewnętrzny wpływ, jaki jest zazwyczaj specyficzne dla platformy.
1. Utwórz konstruktora, który wywołuje konstruktor klasy podstawowej, przekazując łączenia rozpoznawania nazwy grupy i unikatowy identyfikator, który został określony w każdej klasie efekt specyficzne dla platformy.
1. Dodaj właściwości do klasy dla każdego parametru do przekazania do efekt.

Następnie parametry mogą być przekazywane w celu podczas tworzenia wystąpienia efekt, określając wartości dla każdej właściwości.

Aplikacja przykładowa prezentuje `ShadowEffect` dodaje cienia do tekstu wyświetlanego przez [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) formantu. Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](clr-properties-images/shadow-effect.png "Obowiązki tle wpływ projektu:")

A [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) kontrolować na `HomePage` jest dostosowane przez `LabelShadowEffect` w każdym projekcie specyficzne dla platformy. Parametry są przekazywane do każdej `LabelShadowEffect` za pośrednictwem właściwości `ShadowEffect` klasy. Każdy `LabelShadowEffect` pochodną klasy `PlatformEffect` klasy dla każdej platformy. Powoduje to cienia dodawany do tekstu wyświetlanego przez `Label` kontroli, jak pokazano na poniższych zrzutach ekranu:

![](clr-properties-images/screenshots.png "Efektem cienia na każdej platformie")

## <a name="creating-effect-parameters"></a>Tworzenie skutku parametrów

A `public` klasy tego podklasy [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) klasy należy utworzyć do przedstawiania parametrów efekt, jak pokazano w poniższym przykładzie:

```csharp
public class ShadowEffect : RoutingEffect
{
  public float Radius { get; set; }

  public Color Color { get; set; }

  public float DistanceX { get; set; }

  public float DistanceY { get; set; }

  public ShadowEffect () : base ("MyCompany.LabelShadowEffect")
  {         
  }
}
```

`ShadowEffect` Zawiera cztery właściwości, które reprezentują parametry do przekazania do każdego specyficzne dla platformy `LabelShadowEffect`. Konstruktor klasy wywołuje konstruktor klasy podstawowej, przekazując parametr składające się z łączenia rozpoznawania nazwy grupy i unikatowy identyfikator, który został określony w każdej klasie efekt specyficzne dla platformy. W związku z tym nowe wystąpienie klasy `MyCompany.LabelShadowEffect` zostanie dodany do formantu [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekcji po `ShadowEffect` zostanie uruchomiony.

## <a name="consuming-the-effect"></a>Korzystanie z efektu

Przedstawia poniższy przykładowy kod XAML [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) formantu, do którego `ShadowEffect` jest dołączony:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Effects>
    <local:ShadowEffect Radius="5" DistanceX="5" DistanceY="5">
      <local:ShadowEffect.Color>
        <OnPlatform x:TypeArguments="Color">
            <On Platform="iOS" Value="Black" />
            <On Platform="Android" Value="White" />
            <On Platform="UWP" Value="Red" />
        </OnPlatform>
      </local:ShadowEffect.Color>
    </local:ShadowEffect>
  </Label.Effects>
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

label.Effects.Add (new ShadowEffect {
  Radius = 5,
  Color = color,
  DistanceX = 5,
  DistanceY = 5
});
```

W obu przykłady kodu, wystąpienie `ShadowEffect` utworzyć wystąpienia klasy z wartościami określany dla każdej właściwości, przed dodaniem go do formantu [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekcji. Należy pamiętać, że `ShadowEffect.Color` właściwość używa wartości kolorów specyficzne dla platformy. Aby uzyskać więcej informacji, zobacz [klasę urządzeń](~/xamarin-forms/platform/device.md).

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
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    Control.Layer.CornerRadius = effect.Radius;
                    Control.Layer.ShadowColor = effect.Color.ToCGColor ();
                    Control.Layer.ShadowOffset = new CGSize (effect.DistanceX, effect.DistanceY);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

`OnAttached` Metoda pobiera `ShadowEffect` wystąpienie i konfiguruje `Control.Layer` właściwości do wartości określonej właściwości, aby utworzyć cień. Ta funkcja jest ujęte w `try` / `catch` blokowania w przypadku, gdy formant, który efekt jest dołączony do nie ma `Control.Layer` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ czyszczenie nie jest niezbędne.

### <a name="android-project"></a>Projekt systemu android

Poniższy kod przedstawia przykład `LabelShadowEffect` implementacji dla projektu Android:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var control = Control as Android.Widget.TextView;
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    float radius = effect.Radius;
                    float distanceX = effect.DistanceX;
                    float distanceY = effect.DistanceY;
                    Android.Graphics.Color color = effect.Color.ToAndroid ();
                    control.SetShadowLayer (radius, distanceX, distanceY, color);
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

`OnAttached` Metoda pobiera `ShadowEffect` wystąpienia i wywołania [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) metodę w celu utworzenia cienia przy użyciu wartości określonej właściwości. Ta funkcja jest ujęte w `try` / `catch` blokowania w przypadku, gdy formant, który efekt jest dołączony do nie ma `Control.Layer` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ czyszczenie nie jest niezbędne.

### <a name="windows-phone--universal-windows-platform-projects"></a>Windows Phone & projekty platformy uniwersalnej systemu Windows

Poniższy kod przedstawia przykład `LabelShadowEffect` implementacji dla projektów Windows Phone i Windows platformy Uniwersalnej:

```csharp
[assembly: ResolutionGroupName ("Xamarin")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.WinPhone81
{
    public class LabelShadowEffect : PlatformEffect
    {
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                    if (effect != null) {
                        var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;
                        var shadowLabel = new Label ();
                        shadowLabel.Text = textBlock.Text;
                        shadowLabel.FontAttributes = FontAttributes.Bold;
                        shadowLabel.HorizontalOptions = LayoutOptions.Center;
                        shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;
                        shadowLabel.TextColor = effect.Color;
                        shadowLabel.TranslationX = effect.DistanceX;
                        shadowLabel.TranslationY = effect.DistanceY;

                        ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                        shadowAdded = true;
                    }
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

Środowiska uruchomieniowego systemu Windows i platformy uniwersalnej systemu Windows nie oferują efekt cieniowania i dlatego element `LabelShadowEffect` implementacji na obu platform symuluje jedną przez dodanie przesunięcia drugi [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) za podstawowy `Label`. `OnAttached` Metoda pobiera `ShadowEffect` wystąpienia, tworzy nowy `Label`i ustawia niektóre właściwości układu `Label`. Następnie tworzy cień przez ustawienie [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), i [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) właściwości, aby kontrolować koloru i lokalizacji `Label`. `shadowLabel` Zostanie wstawiony przesunięcie za podstawowy `Label`. Ta funkcja jest ujęte w `try` / `catch` blokowania w przypadku, gdy formant, który efekt jest dołączony do nie ma `Control.Layer` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ czyszczenie nie jest niezbędne.

## <a name="summary"></a>Podsumowanie

W tym artykule wykazała, przy użyciu właściwości CLR do przekazania parametrów do efektu. CLR właściwości mogą służyć do definiowania efekt parametrów, które nie odpowiadają na zmiany właściwości środowiska uruchomieniowego.


## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe moduły renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effect](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [Efektem cienia (przykład)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
