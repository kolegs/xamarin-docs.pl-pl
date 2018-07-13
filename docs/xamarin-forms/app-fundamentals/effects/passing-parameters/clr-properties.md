---
title: Przekazywanie parametrów efekt jako wspólne właściwości środowisko uruchomieniowe języka
description: Wspólne właściwości środowiska uruchomieniowego języka wspólnego (CLR) może służyć do definiowania parametrów efekt, które nie odpowiadają na zmiany właściwości środowiska uruchomieniowego. W tym artykule przedstawiono, przy użyciu właściwości aparatu CLR w celu przekazania parametrów do efektu.
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 1bb357b256a7cc6d52d1d92613f38cbf48400c4c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995771"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>Przekazywanie parametrów efekt jako wspólne właściwości środowisko uruchomieniowe języka

_Wspólne właściwości środowiska uruchomieniowego języka wspólnego (CLR) może służyć do definiowania parametrów efekt, które nie odpowiadają na zmiany właściwości środowiska uruchomieniowego. W tym artykule przedstawiono, przy użyciu właściwości aparatu CLR w celu przekazania parametrów do efektu._

Proces tworzenia parametrów efekt, które nie odpowiadają na zmiany właściwości środowiska uruchomieniowego jest następująca:

1. Tworzenie `public` klasę tej podklasy [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) klasy. `RoutingEffect` Klasa reprezentuje efekt niezależne od platformy, która otacza efektu wewnętrzny, który jest zazwyczaj specyficzne dla platformy.
1. Utwórz konstruktora, który wywołuje konstruktora klasy bazowej, przekazując składa się z nazwy grupy rozdzielczość i unikatowy identyfikator, który został określony dla każdej klasy efekt specyficzne dla platformy.
1. Dodaj właściwości do klasy dla każdego parametru, który zostanie przekazany do efektu.

Parametry mogą być następnie przekazywany do efekt, określając wartości dla każdej właściwości podczas tworzenia wystąpienia efekt.

Przykładowa aplikacja demonstruje `ShadowEffect` dodająca cienia do tekstu wyświetlanego przez [ `Label` ](xref:Xamarin.Forms.Label) kontroli. Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](clr-properties-images/shadow-effect.png "Obowiązki projektu efekt w tle")

A [ `Label` ](xref:Xamarin.Forms.Label) kontrolować na `HomePage` dostosowanego przez `LabelShadowEffect` w każdym projekcie specyficzne dla platformy. Parametry są przekazywane do każdej `LabelShadowEffect` za pośrednictwem właściwości `ShadowEffect` klasy. Każdy `LabelShadowEffect` klasa pochodzi od `PlatformEffect` klasy dla każdej platformy. Skutkuje to cienia do tekstu wyświetlanego przez dodawany `Label` kontrolować, jak pokazano na poniższych zrzutach ekranu:

![](clr-properties-images/screenshots.png "Efektem cienia na każdej platformie")

## <a name="creating-effect-parameters"></a>Tworzenie efektu parametrów

A `public` klasę tej podklasy [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) klasy należy utworzyć reprezentują parametry efekt, jak pokazano w poniższym przykładzie kodu:

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

`ShadowEffect` Zawiera cztery właściwości, które reprezentują parametry do przekazania do każdego specyficzne dla platformy `LabelShadowEffect`. Konstruktor klasy wywołuje konstruktora klasy bazowej, przekazując w parametrze składający się z składa się z nazwy grupy rozdzielczość i unikatowy identyfikator, który został określony dla każdej klasy efekt specyficzne dla platformy. W związku z tym, nowe wystąpienie klasy `MyCompany.LabelShadowEffect` zostaną dodane do kontroli [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekcji podczas `ShadowEffect` konkretyzacji.

## <a name="consuming-the-effect"></a>Korzystanie z wpływ

Ilustruje poniższy przykład kodu XAML [ `Label` ](xref:Xamarin.Forms.Label) kontroli, do którego `ShadowEffect` jest dołączony:

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

label.Effects.Add (new ShadowEffect {
  Radius = 5,
  Color = color,
  DistanceX = 5,
  DistanceY = 5
});
```

W obu przykładach kodu wystąpienie `ShadowEffect` klasy jest utworzone za pomocą wartości są określone dla każdej właściwości przed dodaniem do formantu [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekcji. Należy pamiętać, że `ShadowEffect.Color` właściwość używa wartości kolorów specyficzne dla platformy. Aby uzyskać więcej informacji, zobacz [klasę urządzeń pamięci](~/xamarin-forms/platform/device.md).

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

`OnAttached` Metoda pobiera `ShadowEffect` wystąpienie i konfiguruje `Control.Layer` właściwości do wartości określonej właściwości, aby utworzyć w tle. Ta funkcja jest opakowana w `try` / `catch` blokowania w przypadku, gdy formant, który efekt jest dołączony do nie ma `Control.Layer` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ żadne oczyszczanie jest wymagany.

### <a name="android-project"></a>Projekt dla systemu android

Poniższy kod przedstawia przykład `LabelShadowEffect` implementacji projektu dla systemu Android:

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

`OnAttached` Metoda pobiera `ShadowEffect` wystąpienie i wywołania [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) metodą tworzenia w tle przy użyciu wartości określonej właściwości. Ta funkcja jest opakowana w `try` / `catch` blokowania w przypadku, gdy formant, który efekt jest dołączony do nie ma `Control.Layer` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ żadne oczyszczanie jest wymagany.

### <a name="universal-windows-platform-project"></a>Projekt dla platformy Universal Windows

Poniższy kod przedstawia przykład `LabelShadowEffect` implementacji dla projektu platformy uniwersalnej Windows (UWP):

```csharp
[assembly: ResolutionGroupName ("Xamarin")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
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

Platforma uniwersalna Windows nie zapewnia efektu w tle i dlatego `LabelShadowEffect` implementacji na obu platformach symuluje jeden, dodając przesunięcie drugiego [ `Label` ](xref:Xamarin.Forms.Label) za podstawowy `Label`. `OnAttached` Metoda pobiera `ShadowEffect` wystąpienia, tworzy nowy `Label`i ustawia niektóre właściwości układu w `Label`. Następnie tworzy w tle, ustawiając [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), i [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) właściwości w celu kontrolowania, kolor i położenie `Label`. `shadowLabel` Zostanie wstawiony przesunięcie za podstawowy `Label`. Ta funkcja jest opakowana w `try` / `catch` blokowania w przypadku, gdy formant, który efekt jest dołączony do nie ma `Control.Layer` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ żadne oczyszczanie jest wymagany.

## <a name="summary"></a>Podsumowanie

W tym artykule wykazała, przy użyciu właściwości aparatu CLR w celu przekazania parametrów do efektu. Właściwości aparatu CLR może służyć do definiowania parametrów efekt, które nie odpowiadają na zmiany właściwości środowiska uruchomieniowego.


## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe programy renderujące](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Efekt](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [Efektem cienia (przykład)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
