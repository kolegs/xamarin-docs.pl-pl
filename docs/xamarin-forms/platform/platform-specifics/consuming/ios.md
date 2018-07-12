---
title: specyficznych dla platformy systemu iOS
description: Zezwalaj na specyficznych dla platformy, umożliwiają korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe programy renderujące lub efekty. W tym artykule pokazano, jak korzystać z systemem iOS specyficznych dla platformy, które są wbudowane w platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2018
ms.openlocfilehash: 29cb00c100918bf03efe3f078c366750080c0627
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986151"
---
# <a name="ios-platform-specifics"></a>specyficznych dla platformy systemu iOS

_Zezwalaj na specyficznych dla platformy, umożliwiają korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe programy renderujące lub efekty. W tym artykule pokazano, jak korzystać z systemem iOS specyficznych dla platformy, które są wbudowane w platformy Xamarin.Forms._

W systemie iOS Xamarin.Forms zawiera następujące specyficznych dla platformy:

- Rozmycie pomocy technicznej dla każdego [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/). Aby uzyskać więcej informacji, zobacz [stosowanie Rozmycie](#blur).
- Kontrolowanie, czy tytuł strony jest wyświetlany jako dużych tytuł paska nawigacji strony. Aby uzyskać więcej informacji, zobacz [wyświetlania dużych tytułów](#large_title).
- Zapewnienie tej strony zawartości znajduje się na obszar ekranu, który jest bezpieczny dla wszystkich urządzeń z systemem iOS. Aby uzyskać więcej informacji, zobacz [Włączanie układ prowadnic obszaru bezpiecznego](#safe_area_layout).
- Pasek nawigacyjny przezroczyste. Aby uzyskać więcej informacji, zobacz [wprowadzania półprzezroczyste paska nawigacji](#translucent_navigation_bar).
- Kontrolowanie czy tekst paska stanu kolorów na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) jest dopasowany do jasność części paska nawigacyjnego. Aby uzyskać więcej informacji, zobacz [Dostosowywanie tryb kolor tekstu pasek stanu](#status_bar_color_mode).
- Zapewnienie, że klatkach, w których tekst jest dopasowywana do [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) przez dostosowanie rozmiaru czcionki. Aby uzyskać więcej informacji, zobacz [Dopasowywanie rozmiaru czcionki wpisu](#adjust_font_size).
- Kontrolowanie, gdy wystąpi zaznaczenie elementu w [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Aby uzyskać więcej informacji, zobacz [kontrolowanie zaznaczenie elementu selektora](#picker_update_mode).
- Ustawienie widoczności paska stanu w [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Aby uzyskać więcej informacji, zobacz [ustawienie widoczności paska stanu na stronie](#set_status_bar_visibility).
- Kontrolowanie czy [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) obsługi gestów dotykowych i przekazuje je do jego zawartości. Aby uzyskać więcej informacji, zobacz [opóźniania zawartości poprawek w ScrollView](#delay_content_touches).
- Ustawienie Styl separatora [ `ListView` ](xref:Xamarin.Forms.ListView). Aby uzyskać więcej informacji, zobacz [ustawienie Styl separatora ListView](#listview-separatorstyle).
- Wyłączenie trybu kolorów starszej wersji w obsługiwanej [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Aby uzyskać więcej informacji, zobacz [wyłączenie starszej wersji trybu kolorów](#legacy-color-mode).
- Włączanie cień na [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Aby uzyskać więcej informacji, zobacz [Włączanie cień](#drop-shadow).
- Włączanie [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) w widoku przewijania do przechwytywania i udostępniania gestu pan przewijania widoku. Aby uzyskać więcej informacji, zobacz [Włączanie jednoczesnych rozpoznawania gestów Pan](#simultaneous-pan-gesture).

<a name="blur" />

## <a name="applying-blur"></a>Stosowanie Rozmycie

To specyficzne dla platformy jest używana do rozmycia zawartości w warstwie pod spodem i używane w XAML, ustawiając [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/) dołączona właściwość na wartość [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) wyliczenia:

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

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

`BoxView.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) przestrzeni nazw jest używana do stosowania efektu rozmycia z [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) wyliczenie, zapewniając cztery wartości: [ `None` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None), [ `ExtraLight` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight), [ `Light` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light), i [ `Dark` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark).

Wynik jest fakt, że określonym [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) jest stosowany do [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) wystąpienia, które rozmywa [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) warstwie znajdujące się poniżej:

![](ios-images/blur-effect.png "Rozmycie efekt specyficzne dla platformy")

> [!NOTE]
> Podczas dodawania efektu Rozmycie [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), zdarzenia dotykowe nadal będą odbierane przez `VisualElement`.

<a name="large_title" />

## <a name="displaying-large-titles"></a>Wyświetlanie dużych tytułów

Określonych platform jest używany do wyświetlania tytułu strony dużych tytułu na pasku nawigacyjnym urządzenia korzystające z systemem iOS 11 lub nowszej. Duże tytuł jest wyrównywany do lewej i używa czcionki większych i przechodzi do standardowa tytuł, gdy użytkownik rozpoczyna przewijanie zawartości, tak aby efektywne wykorzystanie powierzchnię ekranu. Jednak w orientacji poziomej tytuł powróci do środka na pasku nawigacyjnym, aby zoptymalizować układ zawartości. Jest używany w XAML, ustawiając `NavigationPage.PrefersLargeTitles` dołączonych właściwości `boolean` wartość:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Można również mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. `NavigationPage.SetPrefersLargeTitle` Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw, określa, czy włączono dużych tytułów.

Pod warunkiem, że dużych tytułów są włączone na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), wszystkich stron w stosie nawigacji będą wyświetlane dużych tytułów. To zachowanie może zostać zastąpiona przez strony, ustawiając `Page.LargeTitleDisplay` dołączona właściwość na wartość `LargeTitleDisplayMode` wyliczenia:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Alternatywnie można zastąpić zachowanie strony za pomocą języka C# przy użyciu interfejsu API fluent:

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

`Page.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. `Page.SetLargeTitleDisplay` Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw, steruje zachowaniem dużej tytuł na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), za pomocą `LargeTitleDisplayMode` wyliczenie, zapewniając trzy możliwe wartości:

- `Always` – force pasku nawigacyjnym, a czcionki rozmiar, aby użyć formatu dużych.
- `Automatic` — Użyj tego samego stylu (duże lub małe) jako poprzedniego elementu w stosie nawigacji.
- `Never` — wymusić użycie części paska nawigacyjnego regularnych, małe formatu.

Ponadto `SetLargeTitleDisplay` metoda może służyć do przełączenia wartości wyliczenia, wywołując `LargeTitleDisplay` metody, która zwraca bieżącą `LargeTitleDisplayMode`:

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

Wynik jest fakt, że określonym `LargeTitleDisplayMode` jest stosowany do [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), która steruje zachowaniem dużej tytułu:

![](ios-images/large-title.png "Rozmycie efekt specyficzne dla platformy")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>Włączanie układ prowadnic obszaru bezpiecznego

Określonych platform jest używany do zapewnienia, że zawartość strony znajduje się na obszar ekranu, który jest bezpieczny dla wszystkich urządzeń, które korzystają z systemem iOS 11 lub nowszym. W szczególności pozwoli to upewnić się, że tę zawartość nie jest przycinana urządzenia zaokrąglone rogi, wskaźnik macierzystego lub obudowie czujnika na telefonie iPhone X. Jest używany w XAML, ustawiając `Page.UseSafeArea` dołączonych właściwości `boolean` wartość:

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

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. `Page.SetUseSafeArea` Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw, określa, czy układ prowadnic obszaru bezpiecznego jest włączony.

Wynik jest, że zawartość strony może być umieszczony w obszar ekranu, który jest bezpieczny dla wszystkich telefonów iPhone:

[![](ios-images/safe-area-layout.png "Układ prowadnic obszaru bezpiecznego")](ios-images/safe-area-layout-large.png#lightbox "układ prowadnic obszaru bezpiecznego")

> [!NOTE]
> Bezpieczny obszar zdefiniowany przez firmę Apple służy do ustawiania w interfejsie Xamarin.Forms [ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) właściwości i spowoduje zastąpienie poprzedniej wartości tej właściwości, które zostały ustawione.

Obszaru bezpiecznego można dostosowywać przez pobieranie jej [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) wartością `Page.SafeAreaInsets` metody z [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw. Następnie może być modyfikowany jako wymagane i ponownie przypisany do elementu `Padding` właściwość w Konstruktorze strony lub [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) zastąpienia:

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

## <a name="making-the-navigation-bar-translucent"></a>Tworzenie na pasku nawigacyjnym półprzezroczyste

Określonych platform umożliwia zmienianie przezroczystości na pasku nawigacyjnym, a następnie używane w XAML, ustawiając [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/) dołączonych właściwości `boolean` wartość:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw jest wykorzystywany do wykonania na pasku nawigacyjnym przezroczyste. Ponadto [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/) klasy w `Xamarin.Forms.PlatformConfiguration.iOSSpecific` przestrzeni nazw ma również [ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) metodę, która przywraca pasek nawigacyjny do stanu domyślnego i [ `SetIsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/) metody, która może służyć do przełączenia przezroczystości paska nawigacji, wywołując [ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) metody:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Wynik jest przezroczystość na pasku nawigacyjnym można zmienić:

![](ios-images/translucent-navigation-bar.png "Specyficzne dla platformy półprzezroczyste pasek nawigacyjny")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>Dostosowywanie paska tryb kolorów tekstu stanu

To określa specyficzne dla platformy czy tekst paska stanu kolorów na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) jest dopasowany do jasność części paska nawigacyjnego. Jest używany w XAML, ustawiając [ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/) dołączona właściwość na wartość [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) wyliczenia:

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

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

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

`NavigationPage.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw, formanty czy tekst paska stanu kolorów na [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) jest dopasowany do jasność części paska nawigacyjnego za pomocą [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) wyliczenie, zapewniając dwóch wartości:

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust) — Wskazuje stan paska koloru tekstu nie powinny być dostosowane.
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity) — Wskazuje, że pasek koloru tekstu stanu powinien być zgodny jasność części paska nawigacyjnego.

Ponadto [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/) metoda może służyć do pobierania wartości bieżącej [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) wyliczenia, która jest stosowana do [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage).

Wynik jest pasek koloru tekstu na stanu [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) mogą być dostosowane do dopasowania Jasność części paska nawigacyjnego. W tym przykładzie paska koloru zmiany w tekście stanu jako użytkownik przełącza między [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) i [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) stron [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

![](ios-images/status-bar-text-color-mode.png "Pasek stanu tekst koloru tryb specyficzne dla platformy")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>Dostosowywanie rozmiaru czcionki wpisu

Umożliwia skalowanie rozmiaru czcionki określonych platform [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) aby upewnić się, że podano: tekst mieści się w kontrolce. Jest używany w XAML, ustawiając [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/) dołączonych właściwości `boolean` wartość:

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

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

`Entry.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) umożliwia skalowanie rozmiaru czcionki podano: tekst, aby upewnić się, że mieści się w przestrzeni nazw, [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). Ponadto [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/) klasy w `Xamarin.Forms.PlatformConfiguration.iOSSpecific` przestrzeni nazw ma również [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) metodę, która wyłącza tego specyficznego dla platformy, a [ `SetAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/) metody, która może służyć do przełączenia rozmiar czcionki, skalowanie, wywołując [ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) metody:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Wynik jest rozmiar czcionki [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) jest skalowana, aby upewnić się, że podano: tekst mieści się w kontrolce:

![](ios-images/entry-font-size.png "Dostosuj wpis czcionka rozmiaru specyficzne dla platformy")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>Kontrolowanie selektora Wybór elementu

Określonych platform kontrolki, gdy wystąpi zaznaczenie elementu w [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), umożliwiając użytkownikowi na określenie, że zaznaczenie elementu odbywa się podczas przeglądania elementów w formancie lub tylko wtedy, gdy **gotowe** naciśnięciu przycisku. Jest używany w XAML, ustawiając `Picker.UpdateMode` dołączona właściwość na wartość `UpdateMode` wyliczenia:

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

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. `Picker.SetUpdateMode` Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw, jest używane do kontrolowania, gdy wystąpi Wybór elementu z `UpdateMode` wyliczenie, zapewniając dwóch wartości:

- `Immediately` — Wybór elementu wypada użytkownik będzie przeglądać elementy w [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Jest to domyślne zachowanie w interfejsie Xamarin.Forms.
- `WhenFinished` — Wybór elementu występuje tylko wtedy, gdy użytkownik nacisnął **gotowe** znajdujący się w [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/).

Ponadto `SetUpdateMode` metoda może służyć do przełączenia wartości wyliczenia, wywołując `UpdateMode` metody, która zwraca bieżącą `UpdateMode`:

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

Wynik jest fakt, że określonym `UpdateMode` jest stosowany do [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), która kontroluje, gdy wystąpi Wybór elementu:

[![](ios-images/picker-updatemode.png "Selektor UpdateMode specyficzne dla platformy")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>Ustawienia paska stanu widoczności na stronie

Określonych platform jest używane do definiowania widoczności paska stanu w [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), a także możliwość sterowania przechodzi na pasku stanu i pozostawia `Page`. Jest używany w XAML, ustawiając `Page.PrefersStatusBarHidden` dołączona właściwość na wartość `StatusBarHiddenMode` wyliczenia i opcjonalnie `Page.PreferredStatusBarUpdateAnimation` dołączona właściwość na wartość `UIStatusBarAnimation` wyliczenia:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. `Page.SetPrefersStatusBarHidden` Metody w `Xamarin.Forms.PlatformConfiguration.iOSSpecific` przestrzeni nazw jest używana do ustawiania widoczność paska stanu w [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) , określając jedną z `StatusBarHiddenMode` wartości wyliczenia: `Default`, `True` , lub `False`. `StatusBarHiddenMode.True` i `StatusBarHiddenMode.False` wartości ustawione widoczności paska stanu, niezależnie od tego, orientacja urządzenia i `StatusBarHiddenMode.Default` wartość ukrywa pasek stanu w środowisku w pionie compact.

Wynik jest widoczność paska stanu [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) można ustawić:

![](ios-images/hide-status-bar.png "Pasek stanu widoczności specyficzne dla platformy")

> [!NOTE]
> Na [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), określony `StatusBarHiddenMode` wartość wyliczenia jest również aktualizowane na pasku stanu na wszystkich stronach podrzędnych. Na wszystkich innych [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-pochodne typy określonego `StatusBarHiddenMode` wartość wyliczenia szablon zaktualizuje jedynie na pasku stanu na bieżącej stronie.

`Page.SetPreferredStatusBarUpdateAnimation` Metoda jest używana do ustawiania jak pasek stanu wprowadza lub go opuszcza [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) , określając jedną z `UIStatusBarAnimation` wartości wyliczenia: `None`, `Fade`, lub `Slide`. Jeśli `Fade` lub `Slide` określono wartość wyliczenia na sekundę 0,25 wykonywane animacji, ponieważ wprowadza na pasku stanu lub go opuszcza `Page`.

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>Opóźnione ma zawartość w ScrollView

Niejawne czasomierza jest wyzwalany, gdy gestów dotykowych rozpoczyna się w [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) w systemie iOS i `ScrollView` decyduje, na podstawie użytkownika akcji w zasięgu czasomierza, czy należy obsługi gestu, czy przekazać go do jego zawartości. Domyślnie, ustawień systemu iOS `ScrollView` zawartości ma opóźnienia, ale może to powodować problemy w pewnych okolicznościach, za pomocą `ScrollView` zawartości nie winning gestu, chociaż powinno. W związku z tym, to określa specyficzne dla platformy czy `ScrollView` obsługi gestów dotykowych i przekazuje je do jego zawartości. Jest używany w XAML, ustawiając `ScrollView.ShouldDelayContentTouches` dołączonych właściwości `boolean` wartość:

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

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. `ScrollView.SetShouldDelayContentTouches` Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) przestrzeni nazw jest używana do kontroli tego, czy [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) obsługi gestów dotykowych i przekazuje je do jego zawartości. Ponadto `SetShouldDelayContentTouches` metoda może służyć do Przełącz opóźniania ma zawartość, wywołując `ShouldDelayContentTouches` metodę, aby zwrócić na to, czy zawartość ma są opóźnione:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

W wyniku [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) można wyłączyć, opóźnienia, więc odbierania zawartości akcenty, w tym scenariuszu [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) odbiera gestu zamiast [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) strony [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "Opóźnienie ScrollView zawartości dotyka specyficzne dla platformy")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

<a name="listview-separatorstyle" />

## <a name="setting-the-separator-style-on-a-listview"></a>Ustawienie Styl separatora w ListView

Określonych platform kontroluje, czy separator między komórek w [ `ListView` ](xref:Xamarin.Forms.ListView) używa całą szerokość `ListView`. Jest używany w XAML, ustawiając [ `ListView.SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) dołączona właściwość na wartość [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) wyliczenia:

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

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

`ListView.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. [ `ListView.SetSeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SetSeparatorStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) przestrzeni nazw jest używana do kontroli, czy separator między komórek w [ `ListView` ](xref:Xamarin.Forms.ListView) używa pełnych szerokość `ListView`, za pomocą [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) wyliczenie, zapewniając dwóch wartości:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) — Wskazuje, domyślne zachowanie separatora dla systemu iOS. Jest to domyślne zachowanie w interfejsie Xamarin.Forms.
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) — Wskazuje, że separatory będą pobierane z jednej krawędzi `ListView` do drugiego.

Wynik jest fakt, że określonym [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) wartość jest stosowana do [ `ListView` ](xref:Xamarin.Forms.ListView), która kontroluje szerokość separatora między komórkami:

![](ios-images/listview-separatorstyle.png "ListView SeparatorStyle specyficzne dla platformy")

> [!NOTE]
> Gdy ustawiono Styl separatora `FullWidth`, nie można zmienić do `Default` w czasie wykonywania.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Wyłączenie trybu kolorów starszej wersji

Niektóre widoki Xamarin.Forms są wyposażone w trybie starszej wersji kolorów. W tym trybie gdy [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) widoku zostaje ustalona `false`, widoku spowoduje zastąpienie kolory ustawionych przez użytkownika za pomocą natywnego kolory domyślne w stanie wyłączenia. Dla zapewnienia zgodności, to tryb kolorów starszej wersji pozostaje domyślne zachowanie dla obsługiwane widoki.

Określonych platform wyłącza ten tryb kolorów starszej wersji, tak, aby kolorów, ustaw dla widoku przez użytkownika pozostają, nawet wtedy, gdy widok jest wyłączony. Jest używany w XAML, ustawiając [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsLegacyColorModeEnabledProperty) dołączonych właściwości `false`:

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

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

_legacyColorModeDisabledButton.On<iOS>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) przestrzeni nazw jest używana do kontroli, czy tryb starszej wersji kolorów jest wyłączona. Ponadto [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})) metoda może służyć do zwrócenia, czy tryb starszej wersji kolorów jest wyłączona.

Wynik jest, można wyłączyć tryb kolorów starszej wersji, co gwarantuje kolorów, ustaw dla widoku przez użytkownika nawet wtedy, gdy widok jest wyłączony:

![](ios-images/legacy-color-mode-disabled.png "Wyłączono tryb starszej wersji kolorów")

> [!NOTE]
> Podczas ustawiania [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) w widoku, tryb starszej wersji kolor całkowicie jest ignorowany. Aby uzyskać więcej informacji na temat stanów wizualnych zobacz [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="drop-shadow" />

## <a name="enabling-a-drop-shadow"></a>Włączanie cień

Określonych platform służy do włączenia cień [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Jest używany w XAML, ustawiając [ `VisualElement.IsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsShadowEnabledProperty) dołączonych właściwości `true`, wraz z liczbą dodatkowych opcjonalne dołączonych właściwości, które kontrolują cienia:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <BoxView ...
                 ios:VisualElement.IsShadowEnabled="true"
                 ios:VisualElement.ShadowColor="Purple"
                 ios:VisualElement.ShadowOpacity="0.7"
                 ios:VisualElement.ShadowRadius="12">
            <ios:VisualElement.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </ios:VisualElement.ShadowOffset>
         </BoxView>
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var boxView = new BoxView { Color = Color.Aqua, WidthRequest = 100, HeightRequest = 100 };
boxView.On<iOS>()
       .SetIsShadowEnabled(true)
       .SetShadowColor(Color.Purple)
       .SetShadowOffset(new Size(10,10))
       .SetShadowOpacity(0.7)
       .SetShadowRadius(12);
```

`VisualElement.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. [ `VisualElement.SetIsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsShadowEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) przestrzeni nazw jest używana do kontroli, czy włączona jest funkcja Cień `VisualElement`. Ponadto następujące metody może być użyta do kontrolowania cienia:

- [`SetShadowColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.Color)) — Ustawia kolor cienia. Domyślny kolor jest [ `Color.Default` ](xref:Xamarin.Forms.Color.Default*).
- [`SetShadowOffset`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},Xamarin.Forms.Size)) — Ustawia przesunięcie cienia. Przesunięcie zmiany kierunku cienia jest rzutowanie i jest określony jako [ `Size` ](xref:Xamarin.Forms.Size) wartości. `Size` Struktury wartości są wyrażone w jednostkach niezależnych od urządzenia, pierwsza wartość jest odległość od lewej (wartości ujemnej) lub w prawo (dodatnia wartość), a druga wartość jest odległości powyżej (wartości ujemnej) lub poniżej (dodatnia wartość) . Wartość domyślna tej właściwości to (0.0, 0.0), co powoduje, że w polu w tle jest rzutowany na każdej stronie `VisualElement`.
- [`SetShadowOpacity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowOpacity(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Double)) — Ustawia Nieprzezroczystość cienia, o wartości znajdujące się w zakresie od 0,0 (przezroczyste) 1.0 (nieprzezroczysty). Wartość nieprzezroczystości domyślna to 0,5.
- [`SetShadowRadius`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetShadowRadius(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Double)) — Ustawia promień rozmycia, używany do renderowania cienia. Wartość domyślna usługi radius jest 10.0.

> [!NOTE]
> Stan cień mogą być przeszukiwane przez wywołanie metody [ `GetIsShadowEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsShadowEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), [ `GetShadowColor` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), [ `GetShadowOffset` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), [ `GetShadowOpacity` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowOpacity(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})), i [ `GetShadowRadius` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetShadowRadius(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})) metody.

Wynik nie będzie można włączyć na cień [ `VisualElement` ](xref:Xamarin.Forms.VisualElement):

![](ios-images/drop-shadow.png "Cień włączone")

<a name="simultaneous-pan-gesture" />

## <a name="enabling-simultaneous-pan-gesture-recognition"></a>Włączanie rozpoznawanie gestu równoczesne Pan

Gdy [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) jest dołączony do widoku w widoku przewijania, wszystkie pan gestów są przechwytywane przez `PanGestureRecognizer` i nie są przekazywane do przewijania widoku. W związku z tym nie jest już przewinie przewijania widoku.

Umożliwia to specyficzne dla platformy `PanGestureRecognizer` w widoku przewijania do przechwytywania i udostępniania gestu pan przewijania widoku. Jest używany w XAML, ustawiając [ `Application.PanGestureRecognizerShouldRecognizeSimultaneously` ](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.platformconfiguration.iosspecific.application.pangesturerecognizershouldrecognizesimultaneouslyproperty?view=xamarin-forms) dołączonych właściwości `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

`Application.On<iOS>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie iOS. [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.iosspecific.application.setpangesturerecognizershouldrecognizesimultaneously?view=xamarin-forms) Metody w [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) przestrzeni nazw jest używana do kontroli, czy aparat rozpoznawania gestów pan w widoku przewijania będzie przechwytywać gestu pan lub przechwytywania i udostępniania panoramowanie gest przewijania widoku. Ponadto [ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.iosspecific.application.getpangesturerecognizershouldrecognizesimultaneously?view=xamarin-forms) metoda może służyć do zwrócenia, czy widok przewijania, który zawiera, są udostępniane w gestu pan [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer).

W związku z tym specyficzne dla platformy włączone, gdy [ `ListView` ](xref:Xamarin.Forms.ListView) zawiera [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer), zarówno `ListView` i `PanGestureRecognizer` otrzyma gestu Kadrowanie i go przetworzyć. Jednak w przypadku tego specyficznego dla platformy wyłączone, gdy `ListView` zawiera `PanGestureRecognizer`, `PanGestureRecognizer` będzie przechwytywać gestu pan i przetworzyć te dane i `ListView` nie będziesz otrzymywać gestu przesuwanie.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób korzystania z systemem iOS specyficznych dla platformy, które są wbudowane w platformy Xamarin.Forms. Zezwalaj na specyficznych dla platformy, umożliwiają korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe programy renderujące lub efekty.

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie funkcji specyficznych dla platformy](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
