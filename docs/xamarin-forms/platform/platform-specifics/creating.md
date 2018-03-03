---
title: "Tworzenie charakterystykę platformy"
description: "Dostawcy mogą tworzyć własne charakterystyki platformy o skutki. Efekt zapewnia określonych funkcji, który następnie jest ujawniany przez poszczególnych platform. Wynik jest wpływ, jaki mogą być łatwo używane za pośrednictwem XAML i za pośrednictwem interfejsu API fluent kodu. W tym artykule przedstawiono sposób ujawniać efekt za pomocą poszczególnych platform."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0D0E6274-6EF2-4D40-BB77-3D8E53BCD24B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/23/2016
ms.openlocfilehash: 7cdc67f8ea1038226bb6ef8c8add8c03e9635e6a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="creating-platform-specifics"></a>Tworzenie charakterystykę platformy

_Dostawcy mogą tworzyć własne charakterystyki platformy o skutki. Efekt zapewnia określonych funkcji, który następnie jest ujawniany przez poszczególnych platform. Wynik jest wpływ, jaki mogą być łatwo używane za pośrednictwem XAML i za pośrednictwem interfejsu API fluent kodu. W tym artykule przedstawiono sposób ujawniać efekt za pomocą poszczególnych platform._

## <a name="overview"></a>Omówienie

Proces tworzenia poszczególnych platform wygląda następująco:

1. Implementuje określonych funkcji jako efekt. Aby uzyskać więcej informacji, zobacz [Tworzenie efektu](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Tworzenie klasy specyficzne dla platformy, która powoduje to udostępnienie efekt. Aby uzyskać więcej informacji, zobacz [Tworzenie klasy specyficzne dla platformy](#creating).
1. Klasy specyficzne dla platformy implementuje dołączona właściwość umożliwia poszczególnych platform do użycia przy użyciu języka XAML. Aby uzyskać więcej informacji, zobacz [Dodawanie właściwości dołączonych](#attached_property).
1. Klasy specyficzne dla platformy implementuje metody rozszerzenia umożliwiające poszczególnych platform zużywanych za pośrednictwem interfejsu API fluent kodu. Aby uzyskać więcej informacji, zobacz [dodanie metody rozszerzenia](#extension_methods).
1. Zmodyfikuj implementacji efekt, tak, aby efekt jest stosowana tylko w przypadku poszczególnych platform została wywołana na tę samą platformę jako efekt. Aby uzyskać więcej informacji, zobacz [tworzenie efekt](#creating_the_effect).

Ujawnienia efekt co poszczególnych platform powstaje czy efekt mogą być łatwo używane za pośrednictwem XAML oraz fluent kodu interfejsu API.

> [!NOTE]
> Przewiduje się, że dostawców będzie używać tej metody do tworzenia własnych platformy charakterystyka, łatwość użycia przez użytkowników. Gdy użytkownicy mogą wybrać utworzyć własne charakterystyki platformy, należy zauważyć, wymaga większej ilości kodu niż tworzenia i używania efektu.

Aplikacja przykładowa prezentuje `Shadow` specyficzne dla platformy umożliwiający Dodawanie cienia do tekstu wyświetlanego przez [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) sterowania:

![](creating-images/screenshots.png "Specyficzne dla platformy w tle")

Implementuje aplikacji przykładowej `Shadow` specyficzne dla platformy na każdej platformie, w celu ułatwienia zrozumienia. Jako uzupełnienie każda implementacja efekt specyficzne dla platformy Implementacja klasy cień jest jednak przeważającej mierze identyczny dla każdej platformy. W związku z tym w tym przewodniku skupia się na implementacji klasy w tle i skojarzone wpływu na pojedynczą platformę.

Aby uzyskać więcej informacji na temat efektów, zobacz [Dostosowywanie formantów z efektami](~/xamarin-forms/app-fundamentals/effects/index.md).

<a name="creating" />

## <a name="creating-a-platform-specific-class"></a>Tworzenie klasy specyficzne dla platformy

Specyficzne dla platformy zostanie utworzona jako `public static` klasy:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

W poniższych sekcjach omówiono wykonania `Shadow` efektu specyficzne dla platformy i skojarzone.

<a name="attached_property" />

### <a name="adding-an-attached-property"></a>Dodawanie dołączona właściwość

Dołączona właściwość musi zostać dodany do `Shadow` specyficzne dla platformy zezwolenie na użycie przez XAML:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        const string EffectName = "MyCompany.LabelShadowEffect";

        public static readonly BindableProperty IsShadowedProperty =
            BindableProperty.CreateAttached("IsShadowed",
                                            typeof(bool),
                                            typeof(Shadow),
                                            false,
                                            propertyChanged: OnIsShadowedPropertyChanged);

        public static bool GetIsShadowed(BindableObject element)
        {
            return (bool)element.GetValue(IsShadowedProperty);
        }

        public static void SetIsShadowed(BindableObject element, bool value)
        {
            element.SetValue(IsShadowedProperty, value);
        }

        ...

        static void OnIsShadowedPropertyChanged(BindableObject element, object oldValue, object newValue)
        {
            if ((bool)newValue)
            {
                AttachEffect(element as FormsElement);
            }
            else
            {
                DetachEffect(element as FormsElement);
            }
        }

        static void AttachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || controller.EffectIsAttached(EffectName))
            {
                return;
            }
            element.Effects.Add(Effect.Resolve(EffectName));
        }

        static void DetachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || !controller.EffectIsAttached(EffectName))
            {
                return;
            }

            var toRemove = element.Effects.FirstOrDefault(e => e.ResolveId == Effect.Resolve(EffectName).ResolveId);
            if (toRemove != null)
            {
                element.Effects.Remove(toRemove);
            }
        }
    }
}
```

`IsShadowed` Dołączona właściwość służy do dodawania `MyCompany.LabelShadowEffect` efektu i usunąć go z formantu który `Shadow` klasy jest dołączony do. To dołączona właściwość rejestrów `OnIsShadowedPropertyChanged` — metoda, która zostanie wykonana po zmianie wartości właściwości. Z kolei wywołuje tę metodę `AttachEffect` lub `DetachEffect` metody, aby dodać lub usunąć wpływu na podstawie wartości z `IsShadowed` dołączona właściwość. Efekt jest dodane lub usunięte z formantu, modyfikując formantu [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) kolekcji.

> [!NOTE]
> Należy pamiętać, że efekt został rozwiązany przez określenie wartość, która jest złączeniem rozpoznawania nazwy grupy i unikatowy identyfikator, który jest określona w implementacji efekt. Aby uzyskać więcej informacji, zobacz [Tworzenie efektu](~/xamarin-forms/app-fundamentals/effects/creating.md).

Aby uzyskać więcej informacji na temat dołączone właściwości, zobacz [dołączonych właściwości](~/xamarin-forms/xaml/attached-properties.md).

<a name="extension_methods" />

### <a name="adding-extension-methods"></a>Dodawanie metody rozszerzenia

Metody rozszerzenia muszą zostać dodane do `Shadow` specyficzne dla platformy zezwolenie na użycie przez kod fluent API:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        ...
        public static bool IsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config)
        {
            return GetIsShadowed(config.Element);
        }

        public static IPlatformElementConfiguration<iOS, FormsElement> SetIsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config, bool value)
        {
            SetIsShadowed(config.Element, value);
            return config;
        }
        ...
    }
}
```

`IsShadowed` i `SetIsShadowed` metody rozszerzenia wywołanie get i ustaw metody dostępu dla `IsShadowed` dołączona właściwość, odpowiednio. Każda metoda rozszerzenia działa na `IPlatformElementConfiguration<iOS, FormsElement>` typu, który określa, że poszczególnych platform może być wywoływana na [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) wystąpień z systemem iOS.

<a name="creating_the_effect" />

### <a name="creating-the-effect"></a>Tworzenie efektu

`Shadow` Dodaje specyficzne dla platformy `MyCompany.LabelShadowEffect` do [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)i usuwa go. Poniższy kod przedstawia przykład `LabelShadowEffect` implementacji dla projektu iOS:

```csharp
[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace ShadowPlatformSpecific.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            UpdateShadow();
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == Shadow.IsShadowedProperty.PropertyName)
            {
                UpdateShadow();
            }
        }

        void UpdateShadow()
        {
            try
            {
                if (((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.CornerRadius = 5;
                    Control.Layer.ShadowColor = UIColor.Black.CGColor;
                    Control.Layer.ShadowOffset = new CGSize(5, 5);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
                else if (!((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.ShadowOpacity = 0;
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`UpdateShadow` Ustawia metodę `Control.Layer` właściwości w celu utworzenia w tle, pod warunkiem, że `IsShadowed` dołączona właściwość jest ustawiona na `true`i pod warunkiem, że `Shadow` specyficzne dla platformy została wywołana na tę samą platformę który Efekt został zaimplementowany dla. To sprawdzanie jest wykonywane z `OnThisPlatform` metody.

Jeśli `Shadow.IsShadowed` dołączony zmiany wartości właściwości w czasie wykonywania, potrzeb efekt odpowiada przez usunięcie cienia. W związku z tym zastąpiona wersja `OnElementPropertyChanged` umożliwia Odpowiedz na zmianę właściwości możliwej do wiązania, wywołując metodę `UpdateShadow` metody.

Aby uzyskać więcej informacji o tworzeniu efektu, zobacz [Tworzenie efektu](~/xamarin-forms/app-fundamentals/effects/creating.md) i [przekazywanie parametrów efekt jako dołączonych właściwości](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

## <a name="consuming-a-platform-specific"></a>Korzystanie z poszczególnych Platform

`Shadow` Specyficzne dla platformy jest używany w języku XAML, ustawiając `Shadow.IsShadowed` dołączona właściwość do `boolean` wartość:

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

Aby uzyskać więcej informacji o korzystanie z platformy szczegóły, zobacz [korzystanie z platformy — szczegóły](~/xamarin-forms/platform/platform-specifics/consuming/index.md).

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób ujawniać efekt za pomocą poszczególnych platform. Wynik jest wpływ, jaki mogą być łatwo używane za pośrednictwem XAML i za pośrednictwem interfejsu API fluent kodu.


## <a name="related-links"></a>Linki pokrewne

- [ShadowPlatformSpecific (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [Dostosowywanie formantów z efektami](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Dołączone właściwości](~/xamarin-forms/xaml/attached-properties.md)
