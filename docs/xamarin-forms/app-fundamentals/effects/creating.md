---
title: Tworzenie efektu
description: Efekty uprościć Dostosowywanie formantu. W tym artykule pokazano, jak utworzyć efekt, który zmienia kolor tła kontrolki wpis, gdy formant uzyskuje fokus.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: b29d83999724a35293882f7b9efc0158171c4fd2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998163"
---
# <a name="creating-an-effect"></a>Tworzenie efektu

_Efekty uprościć Dostosowywanie formantu. W tym artykule pokazano, jak utworzyć efekt, który zmienia kolor tła kontrolki wpis, gdy formant uzyskuje fokus._

Proces tworzenia efektu w każdym projekcie specyficzne dla platformy jest następująca:

1. Utwórz podklasę `PlatformEffect` klasy.
1. Zastąp `OnAttached` metody i zapisu logiki, aby dostosować formant.
1. Zastąp `OnDetached` — metoda i zapisu logiki, aby wyczyścić niestandardowe Dostosowywanie formantu, jeśli jest to wymagane.
1. Dodaj [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) atrybutów do klasy efekt. Ten atrybut ustawia firmy szeroki obszar nazw dla skutki, co uniemożliwia kolizji z innych skutków o takiej samej nazwie. Należy pamiętać, że ten atrybut może być stosowany tylko raz na projekt.
1. Dodaj [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) atrybutów do klasy efekt. Ten atrybut rejestruje efekt Unikatowy identyfikator, który jest używany przez zestawu narzędzi Xamarin.Forms wraz z nazwą grupy, aby zlokalizować efekt sprzed zastosowania go do formantu. Ten atrybut przyjmuje dwa parametry — Nazwa typu, wpływ i unikatowego ciągu, która będzie służyć do zlokalizowania efekt sprzed zastosowania go do formantu.

Efekt może być bezpiecznie spożywane, dołączając ją do właściwej opcji kontroli.

> [!NOTE]
> Jest to opcjonalne zapewnić efektu w każdym projekcie platformy. Podjęto próbę użycia efektu, gdy jedna nie jest zarejestrowana zwróci wartość inną niż null, która nie wykonuje żadnych działań.

Przykładowa aplikacja demonstruje `FocusEffect` kolor tła kontrolki, które zmieniają się po jego uzyska fokus. Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](creating-images/focus-effect.png "Obowiązki projektu efekt koncentracji uwagi")

[ `Entry` ](xref:Xamarin.Forms.Entry) Kontrolować na `HomePage` dostosowanego przez `FocusEffect` klasy w każdym projekcie specyficzne dla platformy. Każdy `FocusEffect` klasa pochodzi od `PlatformEffect` klasy dla każdej platformy. Skutkuje to `Entry` sterowania są renderowane przy użyciu koloru tła specyficzne dla platformy, która zmienia się, gdy formant uzyskuje fokus, jak pokazano na poniższych zrzutach ekranu:

![](creating-images/screenshots-1.png "Skupić się wpływu na każdej platformie")
![](creating-images/screenshots-2.png "skupić się wpływu na każdej platformie")

## <a name="creating-the-effect-on-each-platform"></a>Tworzenie efektu na każdej platformie

W poniższych sekcjach omówiono implementacji specyficznych dla platformy `FocusEffect` klasy.

## <a name="ios-project"></a>Projekt systemu iOS

Poniższy kod przedstawia przykład `FocusEffect` implementację dla projektu systemu iOS:

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

`OnAttached` Metody ustawia `BackgroundColor` właściwości formantu, aby purpurowy światła z `UIColor.FromRGB` metody, a także przechowuje ten kolor w polu. Ta funkcja jest opakowana w `try` / `catch` blokowania w przypadku, gdy nie ma kontroli efekt jest dołączony do `BackgroundColor` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ żadne oczyszczanie jest wymagany.

`OnElementPropertyChanged` Zastąpienie reaguje na zmiany właściwości możliwej do wiązania dla kontrolki zestawu narzędzi Xamarin.Forms. Gdy [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) zmiany właściwości `BackgroundColor` zmienić właściwości kontrolki na biały, jeśli kontrolka ma fokus, w przeciwnym razie zostanie on zmieniony purpurowy światła. Ta funkcja jest opakowana w `try` / `catch` blokowania w przypadku, gdy nie ma kontroli efekt jest dołączony do `BackgroundColor` właściwości.

## <a name="android-project"></a>Projekt dla systemu android

Poniższy kod przedstawia przykład `FocusEffect` implementacji projektu dla systemu Android:

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

`OnAttached` Wywołania metody `SetBackgroundColor` metodę, aby ustawić kolor tła kontrolki jasny kolor zielony i przechowuje ten kolor w polu. Ta funkcja jest opakowana w `try` / `catch` blokowania w przypadku, gdy nie ma kontroli efekt jest dołączony do `SetBackgroundColor` właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ żadne oczyszczanie jest wymagany.

`OnElementPropertyChanged` Zastąpienie reaguje na zmiany właściwości możliwej do wiązania dla kontrolki zestawu narzędzi Xamarin.Forms. Gdy [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) zmiany właściwości zmienić kolor tła kontrolki na biały, jeśli kontrolka ma fokus, w przeciwnym razie zostanie on zmieniony na zielony światła. Ta funkcja jest opakowana w `try` / `catch` blokowania w przypadku, gdy nie ma kontroli efekt jest dołączony do `BackgroundColor` właściwości.

## <a name="universal-windows-platform-projects"></a>Universal Windows Platform projektów

Poniższy kod przedstawia przykład `FocusEffect` wdrożenia dla projektów uniwersalnych platformy Windows (UWP):

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

`OnAttached` Metody ustawia `Background` właściwości formantu błękitny i zestawy `BackgroundFocusBrush` właściwości na biały. Ta funkcja jest opakowana w `try` / `catch` blokowania w przypadku, gdy formant efekt jest dołączony do brakuje tych właściwości. Implementacja nie są dostarczane przez `OnDetached` metody ponieważ żadne oczyszczanie jest wymagany.

## <a name="consuming-the-effect"></a>Korzystanie z wpływ

Proces, co umożliwia korzystanie z efektu z biblioteki zestawu narzędzi Xamarin.Forms .NET Standard lub Projekt Biblioteka udostępniona jest następująca:

1. Zadeklaruj formant, który będzie można dostosować przez efekt.
1. Dołącz efekt do formantu, dodając ją do formantu [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekcji.

> [!NOTE]
> Wystąpienie efektu może być dołączony tylko do jednego formantu. W związku z tym efekt muszą zostać rozwiązane, dwa razy, aby używać jej na dwóch kontrolek.

## <a name="consuming-the-effect-in-xaml"></a>Korzystanie z efekt w XAML

Ilustruje poniższy przykład kodu XAML [ `Entry` ](xref:Xamarin.Forms.Entry) kontroli, do którego `FocusEffect` jest dołączony:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

`FocusEffect` Klasy w bibliotece programu .NET Standard obsługuje użycie efektu w XAML i przedstawiono w poniższym przykładzie kodu:

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

`FocusEffect` Podklasy klasy [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) klasy, która reprezentuje efekt niezależne od platformy, która otacza efektu wewnętrzny, który jest zazwyczaj specyficzne dla platformy. `FocusEffect` Klasa wywołuje konstruktora klasy bazowej, przekazując w parametrze składający się z łączenia rozpoznawania nazwy grupy (określony za pomocą [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) atrybut w klasie efekt), a to unikatowy identyfikator określono za pomocą [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) atrybut w klasie efekt. W związku z tym, kiedy [ `Entry` ](xref:Xamarin.Forms.Entry) jest inicjowana w czasie wykonywania, nowe wystąpienie klasy `MyCompany.FocusEffect` jest dodawany do formantu [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekcji.

Efekty również można dołączyć do kontrolki za pomocą zachowania lub za pomocą dołączonych właściwości. Aby uzyskać więcej informacji na temat dołączania efektu do formantu za pomocą zachowania, zobacz [EffectBehavior wielokrotnego użytku, do](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Aby uzyskać więcej informacji na temat dołączania efektu do formantu za pomocą właściwości dołączone zobacz [przekazywania parametrów do efektu](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>Korzystanie z efektu w języku C&num;

Odpowiednik [ `Entry` ](xref:Xamarin.Forms.Entry) w języku C# pokazano w poniższym przykładzie kodu:

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect` Jest dołączony do `Entry` wystąpienie przez dodanie do formantu [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekcji, jak pokazano w poniższym przykładzie kodu:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

[ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) Zwraca [ `Effect` ](xref:Xamarin.Forms.Effect) dla podanej nazwy, która jest łączenie rozpoznawania nazwy grupy (określone za pomocą [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) atrybut w klasie efekt) i unikatowy identyfikator, który został określony przy użyciu [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) atrybut w klasie efekt. Jeśli platforma nie zapewnia efekt, `Effect.Resolve` metoda zwróci innej niż`null` wartości.

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak utworzyć efekt, który zmienia kolor tła [ `Entry` ](xref:Xamarin.Forms.Entry) kontroli, gdy formant uzyskuje fokus.


## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe programy renderujące](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Efekt](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [Wpływ kolorów tła (przykład)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [Efekt fokus (przykład)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
