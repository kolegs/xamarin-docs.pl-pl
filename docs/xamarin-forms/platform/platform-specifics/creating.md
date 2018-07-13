---
title: Tworzenie specyficznych dla platformy
description: W tym artykule pokazano, jak udostępnić efekt przy użyciu określonych platform.
ms.prod: xamarin
ms.assetid: 0D0E6274-6EF2-4D40-BB77-3D8E53BCD24B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/23/2016
ms.openlocfilehash: 1d9f07a089eabedf07bef49c9815fe7e93128f09
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997259"
---
# <a name="creating-platform-specifics"></a>Tworzenie specyficznych dla platformy

_Dostawcy mogą tworzyć własne specyficznych dla platformy z efektami. Efekt zapewnia określonych funkcji, który następnie jest uwidaczniany za pomocą określonych platform. Wynik jest efektu, które mogą być łatwo używane przy użyciu XAML, a także za pośrednictwem fluent kodu interfejsu API. W tym artykule pokazano, jak udostępnić efekt przy użyciu określonych platform._

## <a name="overview"></a>Omówienie

Proces tworzenia określonych platform jest następująca:

1. Implementowanie określonych funkcji jako efekt. Aby uzyskać więcej informacji, zobacz [Tworzenie efektu](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Utwórz klasę specyficzne dla platformy, która udostępni efekt. Aby uzyskać więcej informacji, zobacz [tworzenia klasy specyficzne dla platformy](#creating).
1. Klasy specyficzne dla platformy należy zaimplementować dołączona właściwość, aby zezwolić na określonych platform do użycia przy użyciu XAML. Aby uzyskać więcej informacji, zobacz [Dodawanie dołączonych właściwości](#attached_property).
1. Klasy specyficzne dla platformy implementować metody rozszerzenia umożliwiające określonych platform zużywanych przez fluent kod interfejsu API. Aby uzyskać więcej informacji, zobacz [Dodawanie metody rozszerzenia](#extension_methods).
1. Zmodyfikuj implementacji efekt, tak, aby efekt jest stosowane tylko w przypadku określonych platform zostało wywołane na jednej platformie jako efekt. Aby uzyskać więcej informacji, zobacz [tworzenia efekt](#creating_the_effect).

Wynik udostępnianie efektu jako specyficzne dla platformy jest, że efekt mogą być łatwo używane za pośrednictwem XAML oraz fluent kodu interfejsu API.

> [!NOTE]
> Przewiduje się, że dostawców będzie używać tej techniki do tworzenia własnych specyficznych dla platformy, na łatwość użycia przez użytkowników. Gdy użytkownicy mogą zdecydować się na tworzenie własnych specyficznych dla platformy, należy zauważyć, wymaga więcej kodu niż w przypadku tworzenia i używania efektu.

Przykładowa aplikacja demonstruje `Shadow` określonych platform, które dodadzą cienia do tekstu wyświetlanego przez [ `Label` ](xref:Xamarin.Forms.Label) sterowania:

![](creating-images/screenshots.png "Specyficzne dla platformy w tle")

Implementuje aplikacji przykładowej `Shadow` specyficzne dla platformy na każdej platformie, w celu ułatwienia zrozumienia. Jednak oprócz każda implementacja efekt specyficzne dla platformy implementacji klasy w tle jest niemal identyczne dla każdej platformy. W związku z tym ten przewodnik skupia się na implementacji klasy w tle i skojarzone efekt dla pojedynczej platformy.

Aby uzyskać więcej informacji na temat skutków, zobacz [Dostosowywanie kontrolki z efektami](~/xamarin-forms/app-fundamentals/effects/index.md).

<a name="creating" />

## <a name="creating-a-platform-specific-class"></a>Tworzenie klasy specyficzne dla platformy

Specyficzne dla platformy jest tworzona jako `public static` klasy:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

W poniższych sekcjach omówiono wykonania `Shadow` specyficzne dla platformy i skojarzone efekt.

<a name="attached_property" />

### <a name="adding-an-attached-property"></a>Dodawanie dołączoną właściwość

Dołączona właściwość musi zostać dodany do `Shadow` specyficzne dla platformy zezwolenie na użycie przy użyciu XAML:

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

`IsShadowed` Dołączonej właściwości służy do dodawania `MyCompany.LabelShadowEffect` efektu i usuń go z kontrolki, `Shadow` klasy jest dołączony do. To dołączonych właściwości rejestrów `OnIsShadowedPropertyChanged` metody, która zostanie wykonana po zmianie wartości właściwości. Z kolei wywołuje tę metodę `AttachEffect` lub `DetachEffect` metodę, aby dodać lub usunąć efekt na podstawie wartości z `IsShadowed` dołączona właściwość. Efekt jest dodane lub usunięte z kontrolki, modyfikując kontrolki [ `Effects` ](xref:Xamarin.Forms.Element.Effects) kolekcji.

> [!NOTE]
> Należy pamiętać, że efekt został rozwiązany, określając wartość będącej rozpoznawania nazwy grupy i unikatowy identyfikator, który jest określony w implementacji efekt. Aby uzyskać więcej informacji, zobacz [Tworzenie efektu](~/xamarin-forms/app-fundamentals/effects/creating.md).

Aby uzyskać więcej informacji na temat właściwości dołączone zobacz [dołączonych właściwości](~/xamarin-forms/xaml/attached-properties.md).

<a name="extension_methods" />

### <a name="adding-extension-methods"></a>Dodawanie metody rozszerzenia

Metody rozszerzenia muszą zostać dodane do `Shadow` specyficzne dla platformy zezwolenie na użycie za pomocą fluent kodu interfejsu API:

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

`IsShadowed` i `SetIsShadowed` metody rozszerzenia wywołanie get i ustaw metody dostępu dla `IsShadowed` dołączona właściwość, odpowiednio. Każda metoda rozszerzenia operuje na `IPlatformElementConfiguration<iOS, FormsElement>` typu, który określa, czy określonych platform może być wywołana na [ `Label` ](xref:Xamarin.Forms.Label) wystąpień z systemem iOS.

<a name="creating_the_effect" />

### <a name="creating-the-effect"></a>Tworzenie efektu

`Shadow` Dodaje specyficzne dla platformy `MyCompany.LabelShadowEffect` do [ `Label` ](xref:Xamarin.Forms.Label)i usuwa go. Poniższy kod przedstawia przykład `LabelShadowEffect` implementację dla projektu systemu iOS:

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

`UpdateShadow` Metody ustawia `Control.Layer` właściwości w celu utworzenia w tle, pod warunkiem, że `IsShadowed` dołączonej właściwości jest równa `true`oraz pod warunkiem że `Shadow` specyficzne dla platformy zostało wywołane na jednej platformie, Efekt został zaimplementowany dla. To sprawdzenie odbywa się za pomocą `OnThisPlatform` metody.

Jeśli `Shadow.IsShadowed` dołączyć zmiany wartości właściwości w czasie wykonywania, efekt musi odpowiadać, usuwając w tle. W związku z zastąpione wersją `OnElementPropertyChanged` metoda służy do odpowiedzi na zmianę właściwości możliwej do wiązania, wywołując `UpdateShadow` metody.

Aby uzyskać więcej informacji na temat tworzenia efektu zobacz [Tworzenie efektu](~/xamarin-forms/app-fundamentals/effects/creating.md) i [przekazywanie efekt parametry jako dołączone właściwości](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

## <a name="consuming-a-platform-specific"></a>Korzystanie z określonych Platform

`Shadow` Specyficzne dla platformy jest używany w XAML, ustawiając `Shadow.IsShadowed` dołączonych właściwości `boolean` wartość:

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

Aby uzyskać więcej informacji na temat używania specyficznych dla platformy, zobacz [wykorzystywanie specyficznych dla platformy](~/xamarin-forms/platform/platform-specifics/consuming/index.md).

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak udostępnić efekt przy użyciu określonych platform. Wynik jest efektu, które mogą być łatwo używane przy użyciu XAML, a także za pośrednictwem fluent kodu interfejsu API.


## <a name="related-links"></a>Linki pokrewne

- [ShadowPlatformSpecific (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [Dostosowywanie formantów z efektami](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Dołączone właściwości](~/xamarin-forms/xaml/attached-properties.md)
