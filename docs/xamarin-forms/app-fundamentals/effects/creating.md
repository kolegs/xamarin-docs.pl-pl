---
title: Tworzenie efektu
description: Efekty uprościć Dostosowywanie formantu. W tym artykule przedstawiono sposób tworzenia wpływ, jaki kolor tła formantu wpis zostanie zmieniona, gdy formant uzyska fokus.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: 6138bd1f9211248b3a260795c2ef9d3db87580be
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="creating-an-effect"></a>Tworzenie efektu

_Efekty uprościć Dostosowywanie formantu. W tym artykule przedstawiono sposób tworzenia wpływ, jaki kolor tła formantu wpis zostanie zmieniona, gdy formant uzyska fokus._

Proces tworzenia efektu w każdym projekcie specyficzne dla platformy jest następujący:

1. Utwórz podklasę `PlatformEffect` klasy.
1. Zastąpienie `OnAttached` — metoda i zapisu logiki, aby dostosować formantu.
1. Zastąpienie `OnDetached` — metoda i zapisu logiki, aby wyczyścić dostosowanie formantu, jeśli jest to wymagane.
1. Dodaj [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) do klasy efekt atrybutu. Ten atrybut ustawia firmy szeroki przestrzeń nazw dla efekty uniemożliwia kolizji z innych skutków o takiej samej nazwie. Należy pamiętać, że ten atrybut można stosować tylko raz w projekcie.
1. Dodaj [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) do klasy efekt atrybutu. Ten atrybut rejestruje efekt Unikatowy identyfikator, który jest używany przez platformy Xamarin.Forms, wraz z nazwą grupy, aby zlokalizować efekt przed zastosowaniem do formantu. Atrybut przyjmuje dwa parametry — Nazwa typu wpływu i unikatowy ciąg, który będzie służyć do zlokalizowania efekt przed zastosowaniem do formantu.

Efekt następnie mogą być używane przez dołączenie go do właściwej opcji kontroli.

> [!NOTE]
> Jest to pozycja opcjonalna zapewnienie efektu w każdym projekcie platformy. Podjęto próbę użycia efektu, jeśli jedna nie jest zarejestrowana zwróci wartość inną niż null, która nie działa.

Aplikacja przykładowa prezentuje `FocusEffect` po jego zyskuje fokus który zmienia kolor tła formantu. Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](creating-images/focus-effect.png "Obowiązki projektu efekt fokus")

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Kontrolować na `HomePage` jest dostosowane przez `FocusEffect` klasy w każdym projekcie specyficzne dla platformy. Każdy `FocusEffect` pochodną klasy `PlatformEffect` klasy dla każdej platformy. Powoduje to `Entry` kontrolować renderowanego kolorem tła specyficzne dla platformy, która zostanie zmieniona, gdy formant uzyska fokus, jak pokazano na poniższych zrzutach ekranu:

![](creating-images/screenshots-1.png "Skupić się wpływu na każdej platformie")
![](creating-images/screenshots-2.png "skupić się wpływu na każdej z Platform")

## <a name="creating-the-effect-on-each-platform"></a>Tworzenie wpływu na każdej platformie

W poniższych sekcjach omówiono implementacja specyficzna dla platformy `FocusEffect` klasy.

## <a name="ios-project"></a>iOS projektu

Poniższy kod przedstawia przykład `FocusEffect` implementacji dla projektu iOS:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.iOS
{
    public class FocusEffect : PlatformEffect
    {
        UIColor backgroundColor;

        protected override void OnAttached ()
        {
            try {
                Control.BackgroundColor = backgroundColor = UIColor.FromRGB (204, 153, 255);
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);

            try {
                if (args.PropertyName == "IsFocused") {
                    if (Control.BackgroundColor == backgroundColor) {
                        Control.BackgroundColor = UIColor.White;
                    } else {
                        Control.BackgroundColor = backgroundColor;
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached` Zestawów — metoda `BackgroundColor` właściwości formantu światła purpurowy z `UIColor.FromRGB` metody i w polu przechowuje także tego koloru. Ta funkcja jest ujęte w `try` / `catch` blokowania w przypadku, gdy formant efekt jest dołączony do nie ma `BackgroundColor` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ czyszczenie nie jest niezbędne.

`OnElementPropertyChanged` Zastąpienie odpowiada na zmiany właściwości możliwej do wiązania w formancie platformy Xamarin.Forms. Gdy [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) zmiany właściwości `BackgroundColor` właściwości formantu zostanie zmieniona na kolor biały, jeśli formant ma fokus, w przeciwnym razie zostanie zmieniona na światła purpurowy. Ta funkcja jest ujęte w `try` / `catch` blokowania w przypadku, gdy formant efekt jest dołączony do nie ma `BackgroundColor` właściwości.

## <a name="android-project"></a>Projekt systemu android

Poniższy kod przedstawia przykład `FocusEffect` implementacji dla projektu Android:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached ()
        {
            try {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor (backgroundColor);

            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);
            try {
                if (args.PropertyName == "IsFocused") {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor) {
                        Control.SetBackgroundColor (Android.Graphics.Color.Black);
                    } else {
                        Control.SetBackgroundColor (backgroundColor);
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached` Wywołania metody `SetBackgroundColor` metodę, aby ustawić kolor tła formantu jasny green i przechowuje także kolor tego pola. Ta funkcja jest ujęte w `try` / `catch` blokowania w przypadku, gdy formant efekt jest dołączony do nie ma `SetBackgroundColor` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ czyszczenie nie jest niezbędne.

`OnElementPropertyChanged` Zastąpienie odpowiada na zmiany właściwości możliwej do wiązania w formancie platformy Xamarin.Forms. Gdy [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) zmiany właściwości kolor tła formantu zostanie zmieniona na kolor biały, jeśli formant ma fokus, w przeciwnym razie zostanie zmieniona na jasnozielony. Ta funkcja jest ujęte w `try` / `catch` blokowania w przypadku, gdy formant efekt jest dołączony do nie ma `BackgroundColor` właściwości.

## <a name="universal-windows-platform-projects"></a>Projekty platformy uniwersalnej systemu Windows

Poniższy kod przedstawia przykład `FocusEffect` implementacji dla projektów uniwersalnych platformy systemu Windows (UWP):

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.UWP
{
    public class FocusEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            try
            {
                (Control as Windows.UI.Xaml.Controls.Control).Background = new SolidColorBrush(Colors.Cyan);
                (Control as FormsTextBox).BackgroundFocusBrush = new SolidColorBrush(Colors.White);
            }
            catch (Exception ex)
            {
                Debug.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }
    }
}
```

`OnAttached` Ustawia metodę `Background` właściwości formantu błękitny i zestawy `BackgroundFocusBrush` właściwości na kolor biały. Ta funkcja jest ujęte w `try` / `catch` blokowania w przypadku, gdy te właściwości nie ma efekt jest dołączony do formantu. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ czyszczenie nie jest niezbędne.

## <a name="consuming-the-effect"></a>Korzystanie z efektu

Proces służący do konsumowania efekt platformy Xamarin.Forms przenośnej biblioteki klasy (PCL) lub projektu biblioteki udostępnione przebiega w następujący sposób:

1. Zadeklarować formantu, który będzie można dostosować skutków.
1. Dołącz efekt do formantu przez dodanie go do formantu [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekcji.

> [!NOTE]
> Wystąpienie efektu może zostać dołączona tyko do jednego formantu. W związku z tym wpływ muszą zostać rozwiązane dwa razy, aby go używać na dwóch formantów.

## <a name="consuming-the-effect-in-xaml"></a>Korzystanie z efektu w języku XAML

Przedstawia poniższy przykładowy kod XAML [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) formantu, do którego `FocusEffect` jest dołączony:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

`FocusEffect` Klasy w PCL obsługuje efekt użycia w języku XAML i przedstawiono w poniższym przykładzie:

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

`FocusEffect` Klasy podklasy [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) klasy, która reprezentuje efekt niezależne od platformy, który opakowuje wewnętrzny wpływ, jaki jest zazwyczaj specyficzne dla platformy. `FocusEffect` Klasa wywołuje konstruktor klasy podstawowej, przekazując parametr składające się z łączenia rozpoznawania nazwy grupy (określone za pomocą [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) atrybutu dla klasy efekt), i unikatowy identyfikator została określona za pomocą [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) atrybutu dla klasy efekt. W związku z tym, kiedy [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) został zainicjowany w czasie wykonywania, nowe wystąpienie klasy `MyCompany.FocusEffect` zostanie dodany do formantu [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekcji.

Efekty również można dołączyć do formantów za pomocą zachowanie lub przy użyciu dołączone właściwości. Aby uzyskać więcej informacji dotyczących dołączania efekt do formantu przy użyciu zachowania, zobacz [EffectBehavior wielokrotnego użytku](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Aby uzyskać więcej informacji dotyczących dołączania efekt do formantu przy użyciu dołączone właściwości, zobacz [przekazywanie parametrów w celu](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>Korzystanie z efektu w języku C&num;

Odpowiednik [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) w języku C# przedstawiono w poniższym przykładzie:

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect` Jest dołączony do `Entry` wystąpienia przez dodanie do formantu [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekcji, jak pokazano w poniższym przykładzie:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

[ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) Zwraca [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) dla podanej nazwy, która jest złączeniem rozpoznawania nazwy grupy (określone za pomocą [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) atrybutu dla klasy efekt) i unikatowy identyfikator, który został określony przy użyciu [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) atrybutu dla klasy efekt. Jeśli platforma nie zapewnia efekt, `Effect.Resolve` metoda zwróci niż`null` wartości.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób tworzenia efekt, który zmienia kolor tła [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kontroli, gdy formant uzyska fokus.


## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe programy renderujące](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Efekt](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [Efekt koloru tła (przykład)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [Efekt fokus (przykład)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
