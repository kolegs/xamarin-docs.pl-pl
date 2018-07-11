---
title: Tworzenie motywu niestandardowego zestawu narzędzi Xamarin.Forms
description: W tym artykule opisano sposób tworzenia motywu niestandardowego zestawu narzędzi Xamarin.Forms do odwoływania się do aplikacji.
ms.prod: xamarin
ms.assetid: 4FE08ADC-093F-47FA-B33C-20CF08B5D7E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 018193cf0b198fd87f0f09cbfeba52e9d2a0f68b
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38838173"
---
# <a name="creating-a-custom-xamarinforms-theme"></a>Tworzenie motywu niestandardowego zestawu narzędzi Xamarin.Forms

![](~/media/shared/preview.png "Ten interfejs API jest obecnie w wersji zapoznawczej")

Oprócz dodawania motyw z pakietu Nuget (takie jak [światła](~/xamarin-forms/user-interface/themes/light.md) i [ciemny](~/xamarin-forms/user-interface/themes/dark.md) motywy), można utworzyć własny zasób motywów słownika, które mogą być przywoływane w swojej aplikacji.

## <a name="example"></a>Przykład

Trzy `BoxView`s wyświetlane na [strony motywy](~/xamarin-forms/user-interface/themes/index.md) mają różne zgodnie z trzech klas zdefiniowanych w dwóch dostępnych do pobrania kompozycji.

Aby dowiedzieć się, jak te składniki współpracują, następujące znaczniki tworzy równoważne style, które można dodać bezpośrednio do swojej **App.xaml**.

Uwaga `Class` atrybutu dla `Style` (w przeciwieństwie do [ `x:Key` ](~/xamarin-forms/user-interface/styles/inheritance.md) atrybut dostępne we wcześniejszych wersjach interfejsu Xamarin.Forms).

```xml
<ResourceDictionary>
  <!-- DEFINE ANY CONSTANTS -->
  <Color x:Key="SeparatorLineColor">#CCCCCC</Color>
  <Color x:Key="iOSDefaultTintColor">#007aff</Color>
  <Color x:Key="AndroidDefaultAccentColorColor">#1FAECE</Color>
  <OnPlatform x:TypeArguments="Color" x:Key="AccentColor">
    <On Platform="iOS" Value="{StaticResource iOSDefaultTintColor}" />
    <On Platform="Android" Value="{StaticResource AndroidDefaultAccentColorColor}" />
  </OnPlatform>
  <!--  BOXVIEW CLASSES -->
  <Style TargetType="BoxView" Class="HorizontalRule">
    <Setter Property="BackgroundColor" Value="{ StaticResource SeparatorLineColor }" />
    <Setter Property="HeightRequest" Value="1" />
  </Style>

  <Style TargetType="BoxView" Class="Circle">
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />
    <Setter Property="WidthRequest" Value="34"/>
    <Setter Property="HeightRequest" Value="34"/>
    <Setter Property="HorizontalOptions" Value="Start" />

    <Setter Property="local:ThemeEffects.Circle" Value="True" />
  </Style>

  <Style TargetType="BoxView" Class="Rounded">
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />

    <Setter Property="local:ThemeEffects.CornerRadius" Value="4" />
  </Style>
</ResourceDictionary>
```

Należy zauważyć, że `Rounded` klasy odwołuje się do niestandardowego efektu `CornerRadius`.
Kod ten efekt są podane poniżej - odwołać się do niego prawidłowo niestandardowego `xmlns` muszą zostać dodane do **App.xaml**firmy element główny:

```csharp
xmlns:local="clr-namespace:ThemesDemo;assembly=ThemesDemo"
```

### <a name="c-code-in-the-net-standard-library-project-or-shared-project"></a>Kod C# w projekcie biblioteki .NET Standard lub projektu udostępnionego

Kodu na potrzeby tworzenia tworzy narożnik round `BoxView` używa [efekty](~/xamarin-forms/app-fundamentals/effects/index.md).
Promienia narożnika jest stosowany przy użyciu `BindableProperty` i jest implementowana przez zastosowanie [efekt](~/xamarin-forms/app-fundamentals/effects/index.md). Efekt wymaga kodu specyficznego dla platformy w [iOS](#ios) i [Android](#android) projektów (pokazana poniżej).

```csharp
namespace ThemesDemo
{
  public static class ThemeEffects
  {
  public static readonly BindableProperty CornerRadiusProperty =
    BindableProperty.CreateAttached("CornerRadius", typeof(double), typeof(ThemeEffects), 0.0, propertyChanged: OnChanged<CornerRadiusEffect, double>);
    private static void OnChanged<TEffect, TProp>(BindableObject bindable, object oldValue, object newValue)
              where TEffect : Effect, new()
    {
        var view = bindable as View;
        if (view == null)
        {
            return;
        }

        if (EqualityComparer<TProp>.Equals(newValue, default(TProp)))
        {
            var toRemove = view.Effects.FirstOrDefault(e => e is TEffect);
            if (toRemove != null)
            {
                view.Effects.Remove(toRemove);
            }
        }
        else
        {
            view.Effects.Add(new TEffect());
        }

    }
    public static void SetCornerRadius(BindableObject view, double radius)
    {
        view.SetValue(CornerRadiusProperty, radius);
    }

    public static double GetCornerRadius(BindableObject view)
    {
        return (double)view.GetValue(CornerRadiusProperty);
    }

    private class CornerRadiusEffect : RoutingEffect
    {
        public CornerRadiusEffect()
            : base("Xamarin.CornerRadiusEffect")
        {
        }
    }
  }
}
```

<a name="ios" />

### <a name="c-code-in-the-ios-project"></a>Kod C# w projekcie dla systemu iOS

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;
using CoreGraphics;
using Foundation;
using XFThemes;

namespace ThemesDemo.iOS
{
    public class CornerRadiusEffect : PlatformEffect
    {
        private nfloat _originalRadius;

        protected override void OnAttached()
        {
            if (Container != null)
            {
                _originalRadius = Container.Layer.CornerRadius;
                Container.ClipsToBounds = true;

                UpdateCorner();
            }
        }

        protected override void OnDetached()
        {
            if (Container != null)
            {
                Container.Layer.CornerRadius = _originalRadius;
                Container.ClipsToBounds = false;
            }
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == ThemeEffects.CornerRadiusProperty.PropertyName)
            {
                UpdateCorner();
            }
        }

        private void UpdateCorner()
        {
            Container.Layer.CornerRadius = (nfloat)ThemeEffects.GetCornerRadius(Element);
        }
    }
}
```

<a name="android" />

### <a name="c-code-in-the-android-project"></a>Kod C# w projekcie dla systemu Android

```csharp
using System;
using Xamarin.Forms.Platform;
using Xamarin.Forms.Platform.Android;
using Android.Views;
using Android.Graphics;

namespace ThemesDemo.Droid
{
    public class CornerRadiusEffect : BaseEffect
    {
        private ViewOutlineProvider _originalProvider;

        protected override bool CanBeApplied()
        {
            return Container != null && (int)Android.OS.Build.VERSION.SdkInt >= 21;
        }

        protected override void OnAttachedInternal()
        {
            _originalProvider = Container.OutlineProvider;
            Container.OutlineProvider = new CornerRadiusOutlineProvider(Element);
            Container.ClipToOutline = true;
        }

        protected override void OnDetachedInternal()
        {
            Container.OutlineProvider = _originalProvider;
            Container.ClipToOutline = false;
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (!Attached)
            {
                return;
            }

            if (args.PropertyName == ThemeEffects.CornerRadiusProperty.PropertyName)
            {
                Container.Invalidate();
            }
        }

        private class CornerRadiusOutlineProvider : ViewOutlineProvider
        {
            private Xamarin.Forms.Element _element;

            public CornerRadiusOutlineProvider(Xamarin.Forms.Element element)
            {
                _element = element;
            }

            public override void GetOutline(Android.Views.View view, Outline outline)
            {
                var pixles =
                    (float)ThemeEffects.GetCornerRadius(_element) *
                    view.Resources.DisplayMetrics.Density;

                outline.SetRoundRect(new Rect(0, 0, view.Width, view.Height), (int)pixles);
            }
        }
    }
}
```


## <a name="summary"></a>Podsumowanie

Niestandardowy motyw mogą być tworzone przez definiowanie style dla każdego formantu, który wymaga niestandardowy wygląd. Wiele stylów dla formantu wyróżnia się przez różne `Class` atrybuty w słowniku zasobów, a następnie stosowane przez ustawienie `StyleClass` atrybutów w formancie.

Styl może również korzystać z [efekty](~/xamarin-forms/app-fundamentals/effects/index.md) dalsze dostosowywanie wyglądu formantu.

[Style niejawne](~/xamarin-forms/user-interface/styles/implicit.md) (bez `x:Key` lub `Style` atrybutu) mają zastosowanie do wszystkich formantów, które odpowiadają `TargetType`.
