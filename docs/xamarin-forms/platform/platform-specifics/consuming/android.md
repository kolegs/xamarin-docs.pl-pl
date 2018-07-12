---
title: Android specyficznych dla platformy
description: Zezwalaj na specyficznych dla platformy, umożliwiają korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe programy renderujące lub efekty. W tym artykule pokazano, jak korzystać z systemem Android specyficznych dla platformy, które są wbudowane w platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2018
ms.openlocfilehash: 4a97bec37c99209fa6de26a08f8bde44753d0f2d
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986203"
---
# <a name="android-platform-specifics"></a>Android specyficznych dla platformy

_Zezwalaj na specyficznych dla platformy, umożliwiają korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe programy renderujące lub efekty. W tym artykule pokazano, jak korzystać z systemem Android specyficznych dla platformy, które są wbudowane w platformy Xamarin.Forms._

W systemie Android Xamarin.Forms zawiera następujące specyficznych dla platformy:

- Ustawianie trybu operacyjne klawiatura programowa. Aby uzyskać więcej informacji, zobacz [Ustawianie nietrwałego trybu wejście klawiatury](#soft_input_mode).
- Włączenie szybkie przewijanie [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Aby uzyskać więcej informacji, zobacz [umożliwiające szybkie przewijanie w ListView](#fastscroll).
- Włączanie, szybko przesuwając między stronami w [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Aby uzyskać więcej informacji, zobacz [Włączanie szybko przesuwając między stronami w TabbedPage](#enable_swipe_paging).
- Kontrolowanie porządek elementów wizualnych, aby określić kolejność rysowania. Aby uzyskać więcej informacji, zobacz [kontrolowanie podniesienia uprawnień elementów wizualnych](#elevation).
- Wyłączanie [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) i [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) strony zdarzenia cyklu życia na wstrzymanie i wznowienie odpowiednio dla aplikacji, które używają AppCompat. Aby uzyskać więcej informacji, zobacz [wyłączanie Disappearing i pojawiają się zdarzenia cyklu życia strony](#disable_lifecycle_events).
- Kontrolowanie czy [ `WebView` ](xref:Xamarin.Forms.WebView) można wyświetlić zawartości mieszanej. Aby uzyskać więcej informacji, zobacz [Włączanie mieszanej zawartości w widoku sieci Web](#webview-mixed-content).
- Ustawianie metodę wprowadzania opcji edytora dla klawiatura programowa dla [ `Entry` ](xref:Xamarin.Forms.Entry). Aby uzyskać więcej informacji, zobacz [Opcje ustawienia wpisu Input Method Editor](#entry-imeoptions).
- Wyłączenie trybu kolorów starszej wersji w obsługiwanej [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Aby uzyskać więcej informacji, zobacz [wyłączenie starszej wersji trybu kolorów](#legacy-color-mode).
- Za pomocą wypełnienie domyślne i wartości w tle przycisków dla systemu Android. Aby uzyskać więcej informacji, zobacz [przy użyciu przycisków dla systemu Android](#button-padding-shadow).
- Ustawianie narzędzi położenie i kolor na [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Aby uzyskać więcej informacji, zobacz [ustawienie TabbedPage narzędzi położenie i kolor](#tabbedpage-toolbar).

<a name="soft_input_mode" />

## <a name="setting-the-soft-keyboard-input-mode"></a>Ustawienie trybu wprowadzania klawiatura programowa

Określonych platform służy do ustawiania trybu pracy dla obszaru danych wejściowych klawiatura programowa i jest używany w XAML, ustawiając [ `Application.WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty/) dołączona właściwość na wartość [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) Wyliczanie:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

`Application.On<Android>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie Android. [ `Application.UseWindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) przestrzeni nazw jest używana do ustawiania trybu pracy obszar wejścia klawiatura programowa z [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) wyliczenie, zapewniając dwóch wartości: [ `Pan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan/) i [ `Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/). `Pan` Wartość używa [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/) opcji dostosowania, która nie zmienia rozmiaru okna, po aktywowaniu kontrolkę wejściową. Zamiast tego są panned zawartość okna, tak, aby bieżący fokus nie zostanie przesłonięty przez klawiatura programowa. `Resize` Wartość używa [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/) opcji dostosowywania, która zmienia rozmiar okna, gdy kontrolkę wejściową ma fokus, aby zwolnić miejsce dla klawiatura programowa.

Wynik jest, że klawiatura programowa wprowadź obszar, w którym można ustawić trybu pracy po aktywowaniu kontrolkę wejściową:

[![](android-images/pan-resize.png "Klawiatura programowa działających w trybie specyficzne dla platformy")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="fastscroll" />

## <a name="enabling-fast-scrolling-in-a-listview"></a>Włączenie szybkie przewijanie w ListView

Określonych platform umożliwia szybkie przewijanie za dane w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Jest używany w XAML, ustawiając `ListView.IsFastScrollEnabled` dołączonych właściwości `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie Android. `ListView.SetIsFastScrollEnabled` Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) przestrzeni nazw, umożliwia szybkie przewijanie za dane w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Ponadto `SetIsFastScrollEnabled` metoda może służyć do przełączenia, szybkie przewijanie, wywołując `IsFastScrollEnabled` metodę, aby zwrócić na to, czy włączono szybkie przewijanie:

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

Wynik jest, że szybkie przewijanie dane w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) można włączyć, który zmienia rozmiar przycisku przewijania suwaka:

[![](android-images/fastscroll.png "ListView FastScroll specyficzne dla platformy")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="enable_swipe_paging" />

## <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>Włączanie, szybko przesuwając między stronami w TabbedPage

To specyficzne dla platformy jest używana do włączenia, szybko przesuwając za pomocą gestu poziomy finger między stronami w [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Jest używany w XAML, ustawiając [ `TabbedPage.IsSwipePagingEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty/) dołączonych właściwości `boolean` wartość:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie Android. [ `TabbedPage.SetIsSwipePagingEnabled` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled/p/Xamarin.Forms.BindableObject/System.Boolean/) Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) przestrzeni nazw jest używana do włączenia, szybko przesuwając między stronami w [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Ponadto `TabbedPage` klasy w `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` przestrzeni nazw ma również [ `EnableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) metodę, która umożliwia tym specyficzne dla platformy, a [ `DisableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) metodę, która wyłącza to specyficzne dla platformy. [ `TabbedPage.OffscreenPageLimit` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty/) Dołączona właściwość, a [ `SetOffscreenPageLimit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit/p/Xamarin.Forms.BindableObject/System.Int32/) metody są używane do ustawiania liczbę stron, które ma być przechowywana w stanie bezczynności, po obu stronach bieżącej strony.

Wynik jest tego przesunięcia stronicować stron wyświetlanych przez [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) jest włączona:

![](android-images/tabbedpage-swipe.png)

<a name="elevation" />

## <a name="controlling-the-elevation-of-visual-elements"></a>Kontrolowanie podniesienia uprawnień elementów wizualnych

Określonych platform jest używane do kontrolowania podniesienia uprawnień, czyli porządku, elementów wizualnych w aplikacjach przeznaczonych 21 interfejsu API lub nowszej. Podniesienie poziomu elementu wizualnego określa jego kolejności rysowania za pomocą elementów wizualnych o wyższych wartościach Z occluding elementy wizualne o niższych wartościach Z. Jest używany w XAML, ustawiając `VisualElement.Elevation` dołączonych właściwości `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

`Button.On<Android>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie Android. `VisualElement.SetElevation` Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) przestrzeni nazw jest używana do ustawiania podniesienie poziomu elementu wizualnego do dopuszczający wartości null `float`. Ponadto `VisualElement.GetElevation` metoda może służyć do pobierania wartości podniesienie poziomu elementu wizualnego.

Powoduje to, że podwyższenie poziomu elementy wizualne mogą być kontrolowane tak, aby elementy wizualne o wyższych wartościach Z occlude elementy wizualne o niższych wartościach Z. W związku z tym, w tym przykładzie druga [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) jest renderowany powyżej [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) ma on wyższą wartość podniesienia uprawnień:

![](android-images/elevation.png)

<a name="disable_lifecycle_events" />

## <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Wyłączanie znikający i umieszczone zdarzenia cyklu życia strony

To specyficzne dla platformy jest używana do wyłączenia [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) i [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) zdarzenia strony aplikacji wstrzymać i wznowić odpowiednio dla aplikacji, które używają AppCompat. Ponadto obejmuje możliwość kontrolowania, czy klawiatura programowa jest wyświetlany po wznowieniu, jeśli został wyświetlony na temat wstrzymywania, pod warunkiem, że ustawiono tryb eksploatacji klawiatura programowa [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

> [!NOTE]
> Należy pamiętać, że te zdarzenia są włączone domyślnie, aby zachować istniejące zachowanie dla aplikacji, które zależą od zdarzenia. Wyłączenie tych zdarzeń sprawia, że cykl zdarzeń AppCompat dopasowania pre AppCompat cykl zdarzeń.

Określonych platform mogą być używane w XAML, ustawiając [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty/), [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty/), i [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty/) dołączonych właściwości do `boolean` wartości:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

`Application.Current.On<Android>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie Android. [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) przestrzeni nazw jest używana do włączania lub wyłączania uruchomieniu którego [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) zdarzeń strony, gdy aplikacja wprowadza tła. [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Metoda jest używana do włączania lub wyłączania uruchomieniu którego [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) zdarzeń strony, po wznowieniu działania aplikacji w tle. [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Metoda jest używana kontrolka czy klawiatura programowa jest wyświetlany po wznowieniu, jeśli został wyświetlony na temat wstrzymywania, pod warunkiem że tryb eksploatacji klawiatura programowa jest ustawiona na [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

W wyniku [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) i [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) zdarzenia strony nie będzie uruchamiany na temat wstrzymywania aplikacji i wznowić odpowiednio i że jeśli klawiatura programowa był wyświetlany, gdy aplikacja został wstrzymany, będzie również wyświetlana po wznowieniu działania aplikacji:

[![](android-images/keyboard-on-resume.png "Cykl życia zdarzenia specyficzne dla platformy")](android-images/keyboard-on-resume-large.png#lightbox "cyklu życia zdarzenia specyficzne dla platformy")

<a name="webview-mixed-content" />

## <a name="enabling-mixed-content-in-a-webview"></a>Włączanie zawartości mieszanej w widoku sieci Web

To określa specyficzne dla platformy czy [ `WebView` ](xref:Xamarin.Forms.WebView) można wyświetlana zawartość mieszaną w aplikacjach przeznaczonych interfejsu API 21 lub nowszej. Zawartość mieszana jest zawartość, która początkowo jest ładowany za pośrednictwem połączenia HTTPS, ale która ładuje zasoby (takie jak obrazy, audio, wideo, arkusze stylów, skrypty) za pośrednictwem połączenia HTTP. Jest używany w XAML, ustawiając [ `WebView.MixedContentMode` ](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) dołączona właściwość na wartość [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) wyliczenia:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie Android. [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) przestrzeni nazw jest używana do kontroli, czy zawartość mieszaną, mogą być wyświetlane, za pomocą [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) wyliczenie, zapewniając trzema możliwymi wartościami:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) — Wskazuje, że [ `WebView` ](xref:Xamarin.Forms.WebView) umożliwi pochodzi HTTPS do załadowania zawartości ze źródła HTTP.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) — Wskazuje, że [ `WebView` ](xref:Xamarin.Forms.WebView) nie pozwoli na pochodzenie HTTPS do załadowania zawartości ze źródła HTTP.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) — Wskazuje, że [ `WebView` ](xref:Xamarin.Forms.WebView) podejmie próbę był zgodny z podejściem najnowsze przeglądarkę internetową na urządzeniu. Część zawartości HTTP mogły być ładować pochodzi HTTPS i inne typy zawartości, będą blokowane. Typy zawartości, które są zablokowane lub dozwolone mogą ulec zmianie w każdej wersji systemu operacyjnego.

Wynik jest fakt, że określonym [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) wartość jest stosowana do [ `WebView` ](xref:Xamarin.Forms.WebView), która kontroluje, czy można wyświetlić zawartość mieszana:

[![WebView mieszane obsługi zawartości specyficznej dla platformy](android-images/webview-mixedcontent.png "WebView mieszane specyficzne dla platformy obsługi zawartości")](android-images/webview-mixedcontent-large.png#lightbox "WebView mieszane obsługi zawartości specyficznej dla platformy")

<a name="entry-imeoptions" />

## <a name="setting-entry-input-method-editor-options"></a>Opcje edytora IME ustawienie wpisu

Określonych platform Ustawia metodę wprowadzania opcji edytora IME klawiatura programowa dla [ `Entry` ](xref:Xamarin.Forms.Entry). Obejmuje to ustawienie przycisk akcji użytkownika w dolnym rogu klawiatura programowa i interakcje z `Entry`. Jest używany w XAML, ustawiając [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) dołączona właściwość na wartość [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) wyliczenia:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie Android. [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) przestrzeni nazw jest używana do ustawiania opcji akcji metodę wprowadzania klawiatura programowa dla [ `Entry` ](xref:Xamarin.Forms.Entry), za pomocą [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) wyliczenie, podając następujące wartości:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) — Wskazuje, że klucz nie określonej akcji jest wymagana i podstawowej kontroli generuje automatycznie, jeżeli jest to możliwe. Będzie to `Next` lub `Done`.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) — Wskazuje, że brak klucza akcji zostanie udostępniona.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) — Wskazuje, że klucz akcji wykona operację "Przejdź", biorąc użytkownika do obiektu docelowego tekstu one wpisane.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) — Wskazuje, że klucz akcji wykonuje operację "Szukaj", wpisał biorąc użytkownikowi wyniki wyszukiwania dla tekstu.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) — Wskazuje, że klucz akcji będzie wykonywać operacji "Wyślij", dostarczanie tekst do jego element docelowy.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) — Wskazuje, że klucz akcji wykona operację "dalej", biorąc użytkownika do następnego pola, które będzie akceptować tekstu.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) — Wskazuje, że klucz akcji wykona operację "gotowego", zamyka klawiatura programowa.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) — Wskazuje, że klucz akcji wykona operację "poprzednie" biorąc użytkownika do poprzedniego pola, które będzie akceptować tekstu.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) — maskę, aby wybrać opcje akcji.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) — Wskazuje, że sprawdzanie pisowni będzie ucz się od użytkownika ani Sugeruj korekty oparte na wpisana przez użytkownika została wcześniej.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) — Wskazuje, czy interfejsu użytkownika nie powinien przeprowadzić pełny ekran.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) — Wskazuje, że interfejsu użytkownika będzie widoczna dla wyodrębnionych tekstu.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) — Wskazuje, że interfejsu użytkownika będą wyświetlane dla akcji niestandardowej.

Wynik jest fakt, że określonym [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) wartość jest stosowana do klawiatura programowa dla [ `Entry` ](xref:Xamarin.Forms.Entry), który Ustawia metodę wprowadzania opcji edytora:

[![Wejścia danych wejściowych metody edytora specyficznych dla platformy](android-images/entry-imeoptions.png "wprowadzania danych wejściowych metody edytora specyficznych dla platformy")](android-images/entry-imeoptions-large.png#lightbox "wprowadzania danych wejściowych metody edytora specyficznych dla platformy")

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Wyłączenie trybu kolorów starszej wersji

Niektóre widoki Xamarin.Forms są wyposażone w trybie starszej wersji kolorów. W tym trybie gdy [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) widoku zostaje ustalona `false`, widoku spowoduje zastąpienie kolory ustawionych przez użytkownika za pomocą natywnego kolory domyślne w stanie wyłączenia. Dla zapewnienia zgodności, to tryb kolorów starszej wersji pozostaje domyślne zachowanie dla obsługiwane widoki.

Określonych platform wyłącza ten tryb kolorów starszej wersji, tak, aby kolorów, ustaw dla widoku przez użytkownika pozostają, nawet wtedy, gdy widok jest wyłączony. Jest używany w XAML, ustawiając [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) dołączonych właściwości `false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Android>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie Android. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) przestrzeni nazw jest używana do kontroli, czy tryb starszej wersji kolorów jest wyłączona. Ponadto [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) metoda może służyć do zwrócenia, czy tryb starszej wersji kolorów jest wyłączona.

Wynik jest, można wyłączyć tryb kolorów starszej wersji, co gwarantuje kolorów, ustaw dla widoku przez użytkownika nawet wtedy, gdy widok jest wyłączony:

![](android-images/legacy-color-mode-disabled.png "Wyłączono tryb starszej wersji kolorów")

> [!NOTE]
> Podczas ustawiania [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) w widoku, tryb starszej wersji kolor całkowicie jest ignorowany. Aby uzyskać więcej informacji na temat stanów wizualnych zobacz [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="button-padding-shadow" />

## <a name="using-android-buttons"></a>Za pomocą przycisków dla systemu Android

Określonych platform kontroluje, czy przyciski Xamarin.Forms używają wypełnienie domyślne i wartości w tle przycisków dla systemu Android. Jest używany w XAML, ustawiając [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) i [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) dołączonych właściwości `boolean` wartości:

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

`Button.On<Android>` Metoda określa, że określonych platform będzie uruchamiane tylko w systemie Android. [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) i[ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) metod w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) przestrzeni nazw są używane do kontroli, czy przyciski Xamarin.Forms, użyj wartości domyślnej Wypełnienie i wartości w tle przycisków dla systemu Android. Ponadto [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) i [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) metody może służyć do zwrócenia, czy przycisk używa domyślnego dopełnienie wartość i wartość domyślną w tle odpowiednio.

Wynik jest przyciski Xamarin.Forms można wypełnienie domyślne i wartości w tle przycisków dla systemu Android:

![](android-images/button-padding-and-shadow.png "Wyłączono tryb starszej wersji kolorów")

Należy pamiętać, że na zrzucie ekranu powyżej każdego [ `Button` ](xref:Xamarin.Forms.Button) ma identyczne definicji, chyba że po prawej stronie `Button` używa wypełnienie domyślne i wartości w tle przycisków dla systemu Android.

<a name="tabbedpage-toolbar" />

## <a name="setting-tabbedpage-toolbar-placement-and-color"></a>Ustawianie narzędzi TabbedPage położenie i kolor

Te specyficznych dla platformy są używane do ustawiania położenia i koloru paska narzędzi na [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Są one używane w XAML, ustawiając [ `TabbedPage.ToolbarPlacement` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.toolbarplacementproperty?view=xamarin-forms) dołączona właściwość na wartość [ `ToolbarPlacement` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.toolbarplacement?view=xamarin-forms) wyliczenia, a [ `TabbedPage.BarItemColor` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.baritemcolorproperty?view=xamarin-forms) i [ `TabbedPage.BarSelectedItemColor` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.barselecteditemcolorproperty?view=xamarin-forms) dołączonych właściwości [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

`TabbedPage.On<Android>` Metoda określa, że te specyficznych dla platformy będzie uruchamiane tylko w systemie Android. [ `TabbedPage.SetToolbarPlacement` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.settoolbarplacement?view=xamarin-forms) Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) przestrzeni nazw jest używana do ustawiania położenia paska narzędzi na [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), za pomocą [ `ToolbarPlacement` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.toolbarplacement?view=xamarin-forms) wyliczenie, podając następujące wartości:

- [`Default`](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.toolbarplacement#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_ToolbarPlacement_Default) — Wskazuje, że pasek narzędzi jest umieszczany w lokalizacji domyślnej na stronie. Jest to górnej części strony na telefonach i dolnej części strony na inne idiomy urządzenia.
- [`Top`](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.toolbarplacement#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_ToolbarPlacement_Top) — Wskazuje, że pasek narzędzi znajduje się w górnej części strony.
- [`Bottom`](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.toolbarplacement#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_ToolbarPlacement_Bottom) — Wskazuje, że pasek narzędzi znajduje się w dolnej części strony.

Ponadto [ `TabbedPage.SetBarItemColor` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.setbaritemcolor?view=xamarin-forms#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_TabbedPage_SetBarItemColor_Xamarin_Forms_IPlatformElementConfiguration_Xamarin_Forms_PlatformConfiguration_Android_Xamarin_Forms_TabbedPage__Xamarin_Forms_Color_) i [ `TabbedPage.SetBarSelectedItemColor` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.setbarselecteditemcolor?view=xamarin-forms#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_TabbedPage_SetBarSelectedItemColor_Xamarin_Forms_IPlatformElementConfiguration_Xamarin_Forms_PlatformConfiguration_Android_Xamarin_Forms_TabbedPage__Xamarin_Forms_Color_) metody są używane odpowiednio ustawić kolor elementów paska narzędzi i elementów wybranych narzędzi.

> [!NOTE]
> [ `GetToolbarPlacement` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.gettoolbarplacement?view=xamarin-forms#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_TabbedPage_GetToolbarPlacement_Xamarin_Forms_IPlatformElementConfiguration_Xamarin_Forms_PlatformConfiguration_Android_Xamarin_Forms_TabbedPage__), [ `GetBarItemColor` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.getbaritemcolor?view=xamarin-forms#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_TabbedPage_GetBarItemColor_Xamarin_Forms_IPlatformElementConfiguration_Xamarin_Forms_PlatformConfiguration_Android_Xamarin_Forms_TabbedPage__), I [ `GetBarSelectedItemColor` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.platformconfiguration.androidspecific.tabbedpage.getbarselecteditemcolor?view=xamarin-forms#Xamarin_Forms_PlatformConfiguration_AndroidSpecific_TabbedPage_GetBarSelectedItemColor_Xamarin_Forms_IPlatformElementConfiguration_Xamarin_Forms_PlatformConfiguration_Android_Xamarin_Forms_TabbedPage__) metody może służyć do pobierania, położenie i kolor [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) paska narzędzi.

Wynik jest, że położenie paska narzędzi, kolor elementów paska narzędzi i kolor elementu wybranych narzędzi można ustawić na [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/):

![](android-images/tabbedpage-toolbar-placement.png)

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak używać systemu Android specyficznych dla platformy, które są wbudowane w platformy Xamarin.Forms. Zezwalaj na specyficznych dla platformy, umożliwiają korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe programy renderujące lub efekty.

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie funkcji specyficznych dla platformy](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
