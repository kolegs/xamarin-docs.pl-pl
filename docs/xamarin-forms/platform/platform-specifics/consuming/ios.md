---
title: Szczegóły Platform systemu iOS
description: Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty. W tym artykule pokazano, jak korzystać z systemem iOS platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: dbcf97248c1d4537319691f6e18e0d41c931f423
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848268"
---
# <a name="ios-platform-specifics"></a>Szczegóły Platform systemu iOS

_Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty. W tym artykule pokazano, jak korzystać z systemem iOS platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms._

W systemach iOS platformy Xamarin.Forms zawiera następujące szczegóły platformy:

- Rozmywa obsługę dowolnego [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/). Aby uzyskać więcej informacji, zobacz [stosowania rozmycia](#blur).
- Kontrolowanie, czy tytuł strony jest wyświetlana jako tytuł dużych na pasku nawigacji strony. Aby uzyskać więcej informacji, zobacz [wyświetlanie tytułów dużych](#large_title).
- Zapewnienie, że strony zawartości znajduje się w obszarze ekranu, który jest bezpieczne dla wszystkich urządzeń z systemem iOS. Aby uzyskać więcej informacji, zobacz [Włączanie bezpiecznego przewodnik układ obszaru](#safe_area_layout).
- Pasek nawigacyjny przezroczysty. Aby uzyskać więcej informacji, zobacz [wprowadzania półprzezroczyste paska nawigacji](#translucent_navigation_bar).
- Kontrolowanie czy tekst na pasku stanu kolor na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) jest dopasowany do jasność paska nawigacyjnego. Aby uzyskać więcej informacji, zobacz [dostosowywania tryb kolor tekstu paska stanu](#status_bar_color_mode).
- Zapewnienie, że wprowadzona: tekst jest dopasowywana do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) dostosowując rozmiar czcionki. Aby uzyskać więcej informacji, zobacz [dopasować rozmiar czcionki wpis](#adjust_font_size).
- Kontrolowanie, gdy wystąpi Zaznaczanie elementów w [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Aby uzyskać więcej informacji, zobacz [kontrolowanie Wybór elementu selektora](#picker_update_mode).
- Ustawienie widoczności paska stanu [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Aby uzyskać więcej informacji, zobacz [ustawienie widoczności paska stanu na stronie](#set_status_bar_visibility).
- Kontrolowanie czy [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) obsługi gestów dotykowych i przekazuje je do jego zawartości. Aby uzyskać więcej informacji, zobacz [opóźnienia zawartości poprawki w ScrollView](#delay_content_touches).
- Ustawienie stylu separatora [ `ListView` ](xref:Xamarin.Forms.ListView). Aby uzyskać więcej informacji, zobacz [ustawienie stylu separatora w elemencie ListView](#listview-separatorstyle).
- Wyłączenie trybu starszych kolor na obsługiwanej [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Aby uzyskać więcej informacji, zobacz [wyłączenie trybu koloru starszych](#legacy-color-mode).

<a name="blur" />

## <a name="applying-blur"></a>Stosowanie rozmycia

Poszczególnych platform służy do rozmycia zawartości warstwie poniżej i jest używany w języku XAML, ustawiając [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/) dołączona właściwość na wartość [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) wyliczenie:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
  ...
  <AbsoluteLayout HorizontalOptions="Center">
      <Image Source="monkeyface.png" />
      <BoxView x:Name="boxView" ios:VisualElement.BlurEffect="ExtraLight" HeightRequest="300" WidthRequest="300" />
  </AbsoluteLayout>
  ...
</ContentPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

`BoxView.On<iOS>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie iOS. [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw jest używana do stosowania efektu rozmycia z [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) udostępnia cztery — wyliczenie wartości: [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None/), [ `ExtraLight` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight/), [ `Light` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light/), i [ `Dark` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark/).

Wynik jest to, że określonej [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) jest stosowany do [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) wystąpienia, który rozmywa [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) warstwie znajdujące się poniżej:

![](ios-images/blur-effect.png "Rozmywa efekt specyficzne dla platformy")

<a name="large_title" />

## <a name="displaying-large-titles"></a>Wyświetlanie dużych tytułów

Ta specyficzne dla platformy jest używana do wyświetlania tytułu strony dużych tytułu na pasku nawigacyjnym dla urządzeń, które korzystają z systemem iOS 11 lub nowszej. Duże tytuł jest wyrównywany do lewej używa większej czcionki i przechodzi w standardowe tytuł, gdy użytkownik rozpoczyna przewijanie zawartości, dzięki czemu nieruchomości ekranu jest efektywne wykorzystanie. Jednak w orientacji poziomej tytuł powróci do środka na pasku nawigacji w celu zoptymalizowania układ zawartości. Jest używany w języku XAML, ustawiając `NavigationPage.PrefersLargeTitles` dołączona właściwość do `boolean` wartość:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Możesz też mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie iOS. `NavigationPage.SetPrefersLargeTitle` Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw, określa, czy włączono dużych tytułów.

Pod warunkiem, że duża tytuły są włączone na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), wszystkich stron w stosie nawigacji będą wyświetlane dużych tytułów. To zachowanie może zostać zastąpione przez ustawienie na stronach `Page.LargeTitleDisplay` dołączona właściwość na wartość `LargeTitleDisplayMode` wyliczenie:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Alternatywnie można zastąpić zachowanie strony w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

`Page.On<iOS>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie iOS. `Page.SetLargeTitleDisplay` — Metoda w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw, steruje zachowaniem dużych tytuł na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), z `LargeTitleDisplayMode` udostępnia trzy możliwe — wyliczenie wartości:

- `Always` — wymusić na pasku nawigacyjnym i czcionki rozmiar czcionki do użycia formatu.
- `Automatic` — Użyj tego samego stylu (duże lub małe) jako poprzedni element w stosie nawigacji.
- `Never` — wymusić użycie paska nawigacji zwykłym, mała format.

Ponadto `SetLargeTitleDisplay` metody można użyć do przełączania wartości wyliczenia, wywołując `LargeTitleDisplay` metodę, która zwraca bieżącą `LargeTitleDisplayMode`:

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

Wynik jest to, że określonej `LargeTitleDisplayMode` jest stosowany do [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), która kontroluje zachowanie dużych tytuł:

![](ios-images/large-title.png "Rozmywa efekt specyficzne dla platformy")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>Włączanie bezpieczny obszar prowadnicy układu

Poszczególnych platform służy do zapewnienia, że zawartość strony znajduje się w obszarze ekranu, który jest bezpieczne dla wszystkich urządzeń, które korzystają z systemem iOS 11 i większa. W szczególności ułatwią upewnij się, że zawartość nie jest przycinana urządzenia zaokrąglone narożniki, macierzystego wskaźnika lub obudowy czujnik na telefonie iPhone X. Jest używany w języku XAML, ustawiając `Page.UseSafeArea` dołączona właściwość do `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie iOS. `Page.SetUseSafeArea` Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw, określa, czy włączono bezpieczny obszar przewodnik układu.

Wynik jest, że zawartość strony może być umieszczony na powierzchni ekranu, który jest bezpieczne dla wszystkich telefonów iPhone:

[![](ios-images/safe-area-layout.png "Przewodnik układu bezpieczny obszar")](ios-images/safe-area-layout-large.png#lightbox "bezpieczne obszaru układu przewodnik")

> [!NOTE]
> Bezpieczne obszar zdefiniowany przez firmę Apple służy do ustawiania w platformy Xamarin.Forms [ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) właściwości oraz spowoduje zastąpienie poprzedniej wartości tej właściwości, które zostały ustawione.

Bezpieczny obszar może zostać dostosowane przez pobieranie jej [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) wartości z `Page.SafeAreaInsets` metody z [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw. Następnie można zmodyfikować jako wymagane i ponownie przypisany do `Padding` właściwości w Konstruktorze strony lub [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) zastąpienia:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

<a name="translucent_navigation_bar" />

## <a name="making-the-navigation-bar-translucent"></a>Tworzenie półprzezroczyste paska nawigacyjnego

Poszczególnych platform służy do zmiany przezroczystość na pasku nawigacyjnym i jest używany w języku XAML, ustawiając [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/) dołączona właściwość do `boolean` wartość:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie iOS. [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw jest używane do obliczania półprzezroczyste na pasku nawigacyjnym. Ponadto [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/) klasy w `Xamarin.Forms.PlatformConfiguration.iOSSpecific` przestrzeni nazw ma również [ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) metodę, która przywraca stan domyślnego, na pasku nawigacyjnym i [ `SetIsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/) metodę, która służy do przełączania przezroczystość paska nawigacji przez wywołanie metody [ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) metody:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Wynik jest, można zmienić przezroczystość paska nawigacyjnego:

![](ios-images/translucent-navigation-bar.png "Specyficzne dla platformy półprzezroczyste paska nawigacyjnego")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>Dostosowywanie paska tryb kolor tekstu stanu

To określa specyficzne dla platformy czy tekst na pasku stanu kolor na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) jest dopasowany do jasność paska nawigacyjnego. Jest używany w języku XAML, ustawiając [ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/) dołączona właściwość na wartość [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) wyliczenie:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    x:Class="PlatformSpecifics.iOSStatusBarTextColorModePage">
    <MasterDetailPage.Master>
        <ContentPage Title="Master Page Title" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage BarBackgroundColor="Blue" BarTextColor="White"
                        ios:NavigationPage.StatusBarTextColorMode="MatchNavigationBarTextLuminosity">
            <x:Arguments>
                <ContentPage>
                    <Label Text="Slide the master page to see the status bar text color mode change." />
                </ContentPage>
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>

```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

IsPresentedChanged += (sender, e) =>
{
    var mdp = sender as MasterDetailPage;
    if (mdp.IsPresented)
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.DoNotAdjust);
    else
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.MatchNavigationBarTextLuminosity);
};
```

`NavigationPage.On<iOS>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie iOS. [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw kontrolki czy tekst na pasku stanu kolor na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) jest dopasowany do jasność pasek nawigacyjny z [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) wyliczenie udostępnia dwa możliwe wartości:

- [`DoNotAdjust`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust/) — Wskazuje stan paska koloru tekstu nie powinny być dostosowane.
- [`MatchNavigationBarTextLuminosity`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity/) — Wskazuje, że pasek koloru tekstu stanu powinna być zgodna jasność paska nawigacyjnego.

Ponadto [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/) metody można użyć do pobrania bieżącą wartość [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) wyliczenia, która jest stosowana do [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/).

Wynik jest stan paska koloru tekstu na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) można dostosować do dopasowania Jasność paska nawigacyjnego. W tym przykładzie stan paska tekst zostanie zmieniony kolor jako użytkownik przełącza między [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) i [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) stronach [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

![](ios-images/status-bar-text-color-mode.png "Pasek stanu tekst kolor tryb specyficzne dla platformy")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>Dostosowywanie rozmiaru czcionki wpis

Ta specyficzne dla platformy jest używana do skalowania rozmiar czcionki [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) aby upewnić się, że podano: tekst mieści się w formancie. Jest używany w języku XAML, ustawiając [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/) dołączona właściwość do `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

`Entry.On<iOS>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie iOS. [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw jest używana do skalowania rozmiar czcionki tekstu podano: Upewnij się, że mieści się w [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). Ponadto [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/) klasy w `Xamarin.Forms.PlatformConfiguration.iOSSpecific` przestrzeni nazw ma również [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) metodę, która wyłącza to specyficzne dla platformy, a [ `SetAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/) metodę, która służy do przełączania rozmiar czcionki skalowania przez wywołanie metody [ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) metody:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Wynik jest rozmiar czcionki [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) jest skalowana, aby upewnić się, że podano: tekst mieści się w formancie:

![](ios-images/entry-font-size.png "Dostosuj wpis czcionki rozmiar specyficzne dla platformy")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>Kontrolowanie selektora Zaznaczanie elementów

Steruje tym specyficzne dla platformy po wystąpieniu Zaznaczanie elementów w [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), co pozwala użytkownikowi na określenie, czy wybór elementu występuje podczas przeglądania elementów w formancie lub tylko raz **gotowe** przycisk jest naciśnięty. Jest używany w języku XAML, ustawiając `Picker.UpdateMode` dołączona właściwość na wartość `UpdateMode` wyliczenie:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie iOS. `Picker.SetUpdateMode` Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw jest używana do sterowania, gdy wystąpi Wybór elementu z `UpdateMode` wyliczenie udostępnia dwa możliwe wartości:

- `Immediately` — Wybór elementu występuje jako użytkownik będzie przeglądać elementy [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Jest to domyślne zachowanie w platformy Xamarin.Forms.
- `WhenFinished` — Wybór elementu występuje tylko wtedy, gdy użytkownik nacisnął **gotowe** przycisk [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/).

Ponadto `SetUpdateMode` metody można użyć do przełączania wartości wyliczenia, wywołując `UpdateMode` metodę, która zwraca bieżącą `UpdateMode`:

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

Wynik jest to, że określonej `UpdateMode` jest stosowany do [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), która kontroluje, gdy wystąpi Wybór elementu:

[![](ios-images/picker-updatemode.png "Selektor UpdateMode specyficzne dla platformy")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>Pasek stanu ustawienie widoczności na stronie

Poszczególnych platform jest używana do ustawiania widoczność paska stanu w [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), a także możliwość sterowania wprowadza i pozostawia na pasku stanu `Page`. Jest używany w języku XAML, ustawiając `Page.PrefersStatusBarHidden` dołączona właściwość na wartość `StatusBarHiddenMode` wyliczenia i opcjonalnie `Page.PreferredStatusBarUpdateAnimation` dołączona właściwość na wartość `UIStatusBarAnimation` wyliczenie:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie iOS. `Page.SetPrefersStatusBarHidden` Metody w `Xamarin.Forms.PlatformConfiguration.iOSSpecific` przestrzeni nazw jest używana do ustawiania widoczność paska stanu w [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) przez określenie jednego z `StatusBarHiddenMode` wartości wyliczenia: `Default`, `True` , lub `False`. `StatusBarHiddenMode.True` i `StatusBarHiddenMode.False` wartości ustawiać widoczność paska stanu, niezależnie od orientacji urządzenia i `StatusBarHiddenMode.Default` wartość ukrywa pasek stanu w pionie compact środowiska.

Wynik jest widoczność paska stanu [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) można ustawić:

![](ios-images/hide-status-bar.png "Pasek stanu widoczność specyficzne dla platformy")

> [!NOTE]
> Na [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), określony `StatusBarHiddenMode` wartość wyliczenia powoduje aktualizację na pasku stanu na wszystkich stronach podrzędnych. Na wszystkich innych [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-typów określonego pochodnych `StatusBarHiddenMode` wartość wyliczenia zaktualizuje tylko na pasku stanu na bieżącej stronie.

`Page.SetPreferredStatusBarUpdateAnimation` Metoda jest używana do ustawiania jak pasek stanu wprowadza lub pozostawia [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) przez określenie jednego z `UIStatusBarAnimation` wartości wyliczenia: `None`, `Fade`, lub `Slide`. Jeśli `Fade` lub `Slide` wartość wyliczenia jest określona, drugi 0,25 animacji wykonuje jako pasek stanu wprowadza lub pozostawia `Page`.

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>Opóźnione zawartości poprawki w ScrollView

Niejawne czasomierz jest wyzwalane, gdy gestów dotykowych rozpoczyna się w [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) w systemie iOS i `ScrollView` decyduje, oparte na akcję użytkownika w ramach zakresu czasomierza powinna obsługiwać gestu, lub przekaż go do jego zawartości. Domyślnie iOS `ScrollView` poprawki zawartości opóźnienia, ale może to powodować problemy w niektórych sytuacjach z `ScrollView` nie zastosowanie gestu powinna zawartości. W związku z tym to określa specyficzne dla platformy czy `ScrollView` obsługi gestów dotykowych i przekazuje je do jego zawartości. Jest używany w języku XAML, ustawiając `ScrollView.ShouldDelayContentTouches` dołączona właściwość do `boolean` wartość:

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie iOS. `ScrollView.SetShouldDelayContentTouches` Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw, służy do sterowania czy [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) obsługi gestów dotykowych i przekazuje je do jego zawartości. Ponadto `SetShouldDelayContentTouches` metody można użyć do przełączania się między opóźnienia poprawki zawartości przez wywołanie metody `ShouldDelayContentTouches` metody do zwrócenia opóźnienia zawartości poprawki:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

W wyniku [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) można wyłączyć opóźnienia odbieranie zawartości akcenty, tak że w tym scenariuszu [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) odbiera gestu zamiast [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) strony [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "Opóźnienie ScrollView zawartość stykała specyficzne dla platformy")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

<a name="listview-separatorstyle" />

## <a name="setting-the-separator-style-on-a-listview"></a>Ustawienie stylu separatora w elemencie ListView

Poszczególnych platform kontroluje, czy separator pomiędzy komórek w [ `ListView` ](xref:Xamarin.Forms.ListView) korzysta z pełnej szerokości wprowadzonej `ListView`. Jest używany w języku XAML, ustawiając [ `ListView.SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) dołączona właściwość na wartość [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) wyliczenie:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

`ListView.On<iOS>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie iOS. [ `ListView.SetSeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SetSeparatorStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) przestrzeni nazw, służy do sterowania czy separatora między komórek w [ `ListView` ](xref:Xamarin.Forms.ListView) używa pełnych szerokość `ListView`, z [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) wyliczenie udostępnia dwa możliwe wartości:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) — Wskazuje domyślne zachowanie separatora dla systemu iOS. Jest to domyślne zachowanie w platformy Xamarin.Forms.
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) — Wskazuje, że separatorów będą pobierane z jednej krawędzi `ListView` do drugiego.

Wynik jest to, że określonej [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) wartość jest stosowana do [ `ListView` ](xref:Xamarin.Forms.ListView), która kontroluje szerokość separatora między komórkami:

![](ios-images/listview-separatorstyle.png "Element ListView SeparatorStyle specyficzne dla platformy")

> [!NOTE]
> Po stylu separatora została ustawiona jako `FullWidth`, nie można zmienić do `Default` w czasie wykonywania.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Wyłączenie starsza wersja trybu koloru

Niektóre widoki platformy Xamarin.Forms funkcji trybu koloru starszej wersji. W tym trybie podczas [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) właściwości widoku ustawiono `false`, widok spowoduje zastąpienie kolory ustawionych przez użytkownika z domyślne kolory natywnego stanie wyłączenia. Dla zapewnienia zgodności, w tym trybie Kolor starszej wersji pozostaje domyślne zachowanie dla obsługiwane widoki.

Poszczególnych platform wyłącza ten tryb kolor starszej wersji, aby kolory, ustaw dla widoku przez użytkownika pozostają nawet wtedy, gdy widok jest wyłączony. Jest używany w języku XAML, ustawiając [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsLegacyColorModeEnabledProperty) dołączona właściwość do `false`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                ios:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

_legacyColorModeDisabledButton.On<iOS>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<iOS>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie iOS. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) przestrzeni nazw, służy do sterowania czy tryb starszej wersji kolorów jest wyłączona. Ponadto [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})) metody można użyć do zwrócenia, czy tryb starszej wersji kolorów jest wyłączone.

Wynik jest czy kolor starszej wersji trybu można ją wyłączyć, dzięki czemu kolorów, ustaw dla widoku przez użytkownika pozostają nawet wtedy, gdy widok jest wyłączony:

![](ios-images/legacy-color-mode-disabled.png "Wyłączono tryb starszej wersji kolorów")

> [!NOTE]
> Podczas ustawiania [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) w widoku, tryb starszej wersji kolor całkowicie jest ignorowany. Aby uzyskać więcej informacji na temat stany wizualne, zobacz [platformy Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób korzystania z systemem iOS platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms. Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty.


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie funkcji specyficznych dla platformy](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
