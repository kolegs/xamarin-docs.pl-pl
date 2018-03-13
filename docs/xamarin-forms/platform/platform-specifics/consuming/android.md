---
title: "Platformy systemu android — szczegóły"
description: "Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty. W tym artykule pokazano, jak korzystać z systemem Android platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
ms.openlocfilehash: 739ee4ebeb3176d23ab1eb911baaab31a26252c4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="android-platform-specifics"></a>Platformy systemu android — szczegóły

_Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty. W tym artykule pokazano, jak korzystać z systemem Android platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms._

W systemie Android platformy Xamarin.Forms zawiera następujące szczegóły platformy:

- Ustawianie trybu pracy klawiatura programowa. Aby uzyskać więcej informacji, zobacz [ustawienie nietrwałego trybu Input klawiatury](#soft_input_mode).
- Włączanie szybkie przewijanie w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) uzyskać więcej informacji, zobacz [umożliwiające szybkie przewijanie w elemencie ListView](#fastscroll).
- Włączanie, szybko przesuwając między stronami w [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Aby uzyskać więcej informacji, zobacz [włączenie szybko przesuwając między stronami w TabbedPage](#enable_swipe_paging).
- Kontrolowanie porządek elementów wizualnych, aby określić kolejność rysowania. Aby uzyskać więcej informacji, zobacz [kontrolowanie podniesienia uprawnień elementy wizualne](#elevation).
- Wyłączanie [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) i [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) strony zdarzenia cyklu życia w wstrzymania i wznowienia odpowiednio dla aplikacji używających AppCompat. Aby uzyskać więcej informacji, zobacz [wyłączenie Disappearing i wyświetlaniu zdarzenia cyklu życia strony](#disable_lifecycle_events).

<a name="soft_input_mode" />

## <a name="setting-the-soft-keyboard-input-mode"></a>Ustawienie trybu wprowadzania klawiatura programowa

Służy do ustawiania trybu działania do obszaru wprowadzania klawiatura programowa poszczególnych platform i jest używany w języku XAML, ustawiając [ `Application.WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty/) dołączona właściwość na wartość [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) Wyliczenie:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

`Application.On<Android>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie Android. [ `Application.UseWindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) przestrzeni nazw jest używana do ustawiania trybu działania obszaru wprowadzania klawiatura programowa z [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) wyliczenie, zapewniając dwóch wartości: [ `Pan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan/) i [ `Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/). `Pan` Wartość używa [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/) opcji dostosowania, która nie zmienia rozmiaru okna, po aktywowaniu kontrolki wprowadzania. Zamiast tego zawartość okna przesuwanych tak, aby bieżący fokus nie zostanie przesłonięty przez klawiatura programowa. `Resize` Wartość używa [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/) opcji dostosowania, która zmienia rozmiar okna w przypadku wprowadzania formant ma fokus, aby zwolnić miejsce dla klawiatura programowa.

Wynik jest, że klawiatura programowa wprowadź obszar, który można ustawić trybu pracy po aktywowaniu kontrolki wprowadzania:

[![](android-images/pan-resize.png "Klawiatura programowa działających w trybie specyficzne dla platformy")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="fastscroll" />

## <a name="enabling-fast-scrolling-in-a-listview"></a>Włączanie szybkie przewijanie w elemencie ListView

Poszczególnych platform umożliwia szybkie przewijanie danych w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Jest używany w języku XAML, ustawiając `ListView.IsFastScrollEnabled` dołączona właściwość do `boolean` wartość:

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

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie Android. `ListView.SetIsFastScrollEnabled` Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) przestrzeni nazw, umożliwia szybkie przewijanie danych w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Ponadto `SetIsFastScrollEnabled` metody można użyć do przełączania się między szybkie przewijanie przez wywołanie metody `IsFastScrollEnabled` metody do zwrócenia, czy włączono szybkie przewijanie:

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

Wynik jest, że szybkie przewijanie danych w [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) można włączyć, który zmienia rozmiar przycisku przewijania suwaka:

[![](android-images/fastscroll.png "Element ListView FastScroll specyficzne dla platformy")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="enable_swipe_paging" />

## <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>Włączanie, szybko przesuwając między stronami w TabbedPage

Poszczególnych platform pozwala szybko przesuwając z gestu poziomych linii papilarnych między stronami w [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Jest używany w języku XAML, ustawiając [ `TabbedPage.IsSwipePagingEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty/) dołączona właściwość do `boolean` wartość:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie Android. [ `TabbedPage.SetIsSwipePagingEnabled` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled/p/Xamarin.Forms.BindableObject/System.Boolean/) Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) przestrzeni nazw jest używana do włączenia, szybko przesuwając między stronami w [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Ponadto `TabbedPage` klasy w `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` przestrzeni nazw ma również [ `EnableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) metodę, która umożliwia tym specyficzne dla platformy, a [ `DisableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) metodę, która wyłącza Ta specyficzne dla platformy. [ `TabbedPage.OffscreenPageLimit` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty/) Dołączona właściwość, a [ `SetOffscreenPageLimit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit/p/Xamarin.Forms.BindableObject/System.Int32/) metody umożliwiają określenie liczby stron, które powinny być przechowywane w stanie bezczynności, po obu stronach bieżącej strony.

Wynik jest tym stronicowania przejdź na stronach wyświetlane przez [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) jest włączona:

![](android-images/tabbedpage-swipe.png)

<a name="elevation" />

## <a name="controlling-the-elevation-of-visual-elements"></a>Kontrolowanie podniesienie elementy wizualne

Ta specyficzne dla platformy jest używany do kontrolowania podniesienia uprawnień lub porządek, elementów wizualnych na aplikacje interfejsu API 21 obiektu docelowego lub większą. Podniesienie poziomu elementu wizualnego określa jego kolejność rysowania visual elementami o wyższych wartościach Z occluding elementy wizualne o niższych wartościach Z. Jest używany w języku XAML, ustawiając `Elevation.Elevation` dołączona właściwość do `boolean` wartość:

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
            <Button Text="Button Above BoxView - Click Me" android:Elevation.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

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

`Button.On<Android>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie Android. `Elevation.SetElevation` Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) przestrzeni nazw jest używana do ustawiania podniesienie element wizualny na dopuszczający wartości null `float`. Ponadto `Elevation.GetElevation` metody można użyć do pobierania wartości elementu wizualnego podniesienia uprawnień.

Wynik jest, że podwyższenie poziomu elementy wizualne mogą być kontrolowane tak, aby elementy wizualne o wyższych wartościach Z occlude elementy wizualne o niższych wartościach Z. W związku z tym, w tym przykładzie drugi [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) jest renderowany powyżej [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) , ponieważ ma ona wartość większą podniesienia uprawnień:

![](android-images/elevation.png)

<a name="disable_lifecycle_events" />

## <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Wyłączanie zdarzenia cyklu życia strony znikający i umieszczone

Ta specyficzne dla platformy jest używana do wyłączenia [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) i [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) zdarzenia strony w aplikacji wstrzymywanie i wznawianie w odpowiednio dla aplikacji używających AppCompat. Ponadto obejmuje możliwość kontrolowania, czy klawiatura programowa jest wyświetlany po wznowieniu, jeśli został wyświetlony na wstrzymanie, pod warunkiem, że ustawiono tryb działania programu Klawiatura programowa [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

> [!NOTE]
> Należy pamiętać, że te zdarzenia są włączone domyślnie Aby zachować istniejące zachowanie dla aplikacji, które opierają się na zdarzenia. Wyłączenie tych zdarzeń powoduje, że cykl zdarzeń AppCompat odpowiada wersji pre-AppCompat cykl zdarzeń.

Poszczególnych platform mogą być używane w języku XAML, ustawiając [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty/), [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty/), i [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty/) dołączonych właściwości do `boolean` wartości:

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

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

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

`Application.Current.On<Android>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie Android. [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Metody w [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) służy obszar nazw, aby włączyć lub wyłączyć uruchamiania [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) zdarzeń strony, gdy aplikacji wprowadza tła. [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Metody umożliwia włączanie lub wyłączanie uruchamiania [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) zdarzeń strony, gdy aplikacja zostanie wznowione po tła. [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Używana jest metoda kontroli czy klawiatura programowa jest wyświetlany po wznowieniu, jeśli został wyświetlony na wstrzymanie, pod warunkiem że tryb działania programu Klawiatura programowa ma ustawioną [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

W wyniku [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) i [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) zdarzenia strony nie będą uruchamiane w wstrzymania aplikacji i wznowić odpowiednio i że jeśli klawiatura programowa został wyświetlony podczas aplikacji została wstrzymana, będzie również wyświetlana po wznowieniu działania aplikacji:

[![](android-images/keyboard-on-resume.png "Cykl życia zdarzenia specyficzne dla platformy")](android-images/keyboard-on-resume-large.png#lightbox "cyklu życia zdarzenia specyficzne dla platformy")

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób wykorzystywania Android platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms. Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty.


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie funkcji specyficznych dla platformy](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
