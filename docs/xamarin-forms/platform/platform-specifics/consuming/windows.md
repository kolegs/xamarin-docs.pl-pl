---
title: Specyficznych dla platformy Windows
description: Zezwalaj na specyficznych dla platformy, umożliwiają korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe programy renderujące lub efekty. W tym artykule pokazano, jak używać Windows specyficznych dla platformy, które są wbudowane w platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 52895564ef327845940d687a58b007fb1502e62b
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935130"
---
# <a name="windows-platform-specifics"></a>Specyficznych dla platformy Windows

_Zezwalaj na specyficznych dla platformy, umożliwiają korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe programy renderujące lub efekty. W tym artykule pokazano, jak używać Windows specyficznych dla platformy, które są wbudowane w platformy Xamarin.Forms._

Na Universal Windows Platform (platformy UWP), zestawu narzędzi Xamarin.Forms zawiera następujące specyficznych dla platformy:

- Ustawianie opcji umieszczania paska narzędzi. Aby uzyskać więcej informacji, zobacz [zmiana położenia narzędzi](#toolbar_placement).
- Zwijanie [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) paska nawigacyjnego. Aby uzyskać więcej informacji, zobacz [zwijanie pasek nawigacyjny MasterDetailPage](#collapsable_navigation_bar).
- Włączanie [ `WebView` ](xref:Xamarin.Forms.WebView) do wyświetlania alertów JavaScript w oknie dialogowym komunikatu platformy uniwersalnej systemu Windows. Aby uzyskać więcej informacji, zobacz [wyświetlanie alertów JavaScript](#webview-javascript-alert).
- Włączanie [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) wchodzić w interakcje z aparatem sprawdzania pisowni. Aby uzyskać więcej informacji, zobacz [Włączanie sprawdzania pisowni SearchBar](#searchbar-spellcheck).
- Wykrywanie, kolejność odczytu z pliku tekstowego zawartości [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), i [ `Label` ](xref:Xamarin.Forms.Label) wystąpień. Aby uzyskać więcej informacji, zobacz [wykrywanie kolejność czytania od zawartości](#inputview-readingorder).
- Wyłączenie trybu kolorów starszej wersji w obsługiwanej [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Aby uzyskać więcej informacji, zobacz [wyłączenie starszej wersji trybu kolorów](#legacy-color-mode).
- Włączanie obsługi gestu wzorca tap w [ `ListView` ](xref:Xamarin.Forms.ListView). Aby uzyskać więcej informacji, zobacz [Włączanie naciśnij obsługi gestu w ListView](#listview-selectionmode).

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>Zmiana położenia paska narzędzi

Umożliwia zmienić położenie paska narzędzi na określonych platform [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)i jest używane w XAML, ustawiając [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/) dołączona właściwość na wartość [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) wyliczenia:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

`Page.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na Windows. [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw jest używana do ustawiania położenie paska narzędzi z [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) dostarczanie wyliczenia trzy wartości: [ `Default` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default), [ `Top` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top), i [ `Bottom` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom).

Wynik jest zastosowana do umieszczania określonego narzędzi [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) wystąpienie:

[![](windows-images/toolbar-placement.png "Pasek narzędzi umieszczania specyficzne dla platformy")](windows-images/toolbar-placement-large.png#lightbox "narzędzi umieszczania specyficzne dla platformy")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Zwijanie MasterDetailPage paska nawigacyjnego

Umożliwia Zwiń pasek nawigacyjny na określonych platform [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)i jest używane w XAML, ustawiając [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/) i [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)dołączonych właściwości:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`MasterDetailPage.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na Windows. [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) przestrzeni nazw, jest używany do określania stylu Zwiń z [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) wyliczenie, zapewniając dwóch wartości: [ `Full` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) i [ `Partial` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial). [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/) Metoda służy do określania szerokości częściowo zwiniętym paskiem nawigacji.

Wynik jest fakt, że określonym [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) jest stosowany do [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) wystąpienia o szerokości także określić:

[![](windows-images/collapsed-navigation-bar.png "Zwinięty pasek nawigacji specyficznymi dla platformy")](windows-images/collapsed-navigation-bar-large.png#lightbox "zwinięty pasek nawigacji specyficznymi dla platformy")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>Wyświetlanie alertów JavaScript

Umożliwia to specyficzne dla platformy [ `WebView` ](xref:Xamarin.Forms.WebView) do wyświetlania alertów JavaScript w oknie dialogowym komunikatu platformy uniwersalnej systemu Windows. Jest używany w XAML, ustawiając [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) dołączonych właściwości `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var webView = new Xamarin.Forms.WebView
{
  Source = new HtmlWebViewSource
  {
    Html = @"<html><body><button onclick=""window.alert('Hello World from JavaScript');"">Click Me</button></body></html>"
  }
};
webView.On<Windows>().SetIsJavaScriptAlertEnabled(true);
```

`WebView.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na platformie Universal Windows. [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw jest używana do kontroli, czy alerty JavaScript są włączone. Ponadto `WebView.SetIsJavaScriptAlertEnabled` metoda może służyć do Przełącz alerty JavaScript przez wywołanie metody [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) metodę, aby zwrócić na to, czy są włączone:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

Wynik jest, że JavaScript alerty mogą być wyświetlane w oknie dialogowym komunikatu platformy uniwersalnej systemu Windows:

![Ostrzeżenie WebView JavaScript specyficzne dla platformy](windows-images/webview-javascript-alert.png "alert WebView JavaScript specyficzne dla platformy")

<a name="searchbar-spellcheck" />

## <a name="enabling-searchbar-spell-check"></a>Włączanie sprawdzania pisowni SearchBar

Umożliwia to specyficzne dla platformy [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) wchodzić w interakcje z aparatem sprawdzania pisowni. Jest używany w XAML, ustawiając [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) dołączonych właściwości `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na platformie Universal Windows. [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw, włącza moduł sprawdzania pisowni i wyłącza. Ponadto `SearchBar.SetIsSpellCheckEnabled` metoda może służyć do przełączenia sprawdzania pisowni przez wywołanie metody [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) metodę, aby zwrócić na to, czy moduł sprawdzania pisowni jest włączone:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Wynik jest ten tekst wprowadzony w [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) może być sprawdzana pisownia, za pomocą niepoprawne błędy pisowni, które są określone dla użytkownika:

![SearchBar pisowni wyboru specyficzne dla platformy](windows-images/searchbar-spellcheck.png "SearchBar pisowni wyboru specyficzne dla platformy")

> [!NOTE]
> `SearchBar` Klasy w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw ma również [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) i [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) metod, które mogą służyć do włączania i wyłączania Moduł sprawdzania pisowni na `SearchBar`, odpowiednio.

<a name="inputview-readingorder" />

## <a name="detecting-reading-order-from-content"></a>Wykrywanie, kolejność czytania od zawartości

Określonych platform umożliwia kolejność odczytu (od lewej do prawej lub od prawej do lewej) tekstu dwukierunkowego w [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), i [ `Label` ](xref:Xamarin.Forms.Label) wystąpień, aby zostało wykryte, dynamicznie. Jest używany w XAML, ustawiając [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (dla `Entry` i `Editor` wystąpień) lub [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) dołączonych właściwości `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na platformie Universal Windows. [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw jest używana do kontroli, czy wykryto kolejność czytania od zawartości w [ `InputView` ](xref:Xamarin.Forms.InputView). Ponadto `InputView.SetDetectReadingOrderFromContent` metoda może służyć do wyświetlania i ukrywania wykryto kolejność czytania od zawartości przez wywołanie metody [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) metodę do zwracania bieżącej wartości:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

W wyniku [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), i [ `Label` ](xref:Xamarin.Forms.Label) wystąpień może mieć kolejność czytania ich zawartość dynamicznie wykryto:

[![InputView wykrywanie kolejność czytania od zawartości specyficznej dla platformy](windows-images/editor-readingorder.png "InputView wykrywanie kolejność czytania od zawartości specyficznej dla platformy")](windows-images/editor-readingorder-large.png#lightbox "InputView wykrywanie kolejność czytania od zawartość określonej platformy")

> [!NOTE]
> W przeciwieństwie do ustawienia [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwość, logiki dla widoków, które wykrywają kolejność czytania od ich zawartości tekstowej nie wpłynie na wyrównanie tekstu w widoku. Zamiast tego dostosowuje kolejność, w którym są ułożone bloki tekstu dwukierunkowego.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Wyłączenie trybu kolorów starszej wersji

Niektóre widoki Xamarin.Forms są wyposażone w trybie starszej wersji kolorów. W tym trybie gdy [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) widoku zostaje ustalona `false`, widoku spowoduje zastąpienie kolory ustawionych przez użytkownika za pomocą natywnego kolory domyślne w stanie wyłączenia. Dla zapewnienia zgodności, to tryb kolorów starszej wersji pozostaje domyślne zachowanie dla obsługiwane widoki.

Określonych platform wyłącza ten tryb kolorów starszej wersji, tak, aby kolorów, ustaw dla widoku przez użytkownika pozostają, nawet wtedy, gdy widok jest wyłączony. Jest używany w XAML, ustawiając [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) dołączonych właściwości `false`:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na Windows. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw jest używana do kontroli, czy tryb starszej wersji kolorów jest wyłączona. Ponadto [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) metoda może służyć do zwrócenia, czy tryb starszej wersji kolorów jest wyłączona.

Wynik jest, można wyłączyć tryb kolorów starszej wersji, co gwarantuje kolorów, ustaw dla widoku przez użytkownika nawet wtedy, gdy widok jest wyłączony:

![](windows-images/legacy-color-mode-disabled.png "Wyłączono tryb starszej wersji kolorów")

> [!NOTE]
> Podczas ustawiania [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) w widoku, tryb starszej wersji kolor całkowicie jest ignorowany. Aby uzyskać więcej informacji na temat stanów wizualnych zobacz [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="listview-selectionmode" />

## <a name="enabling-tap-gesture-support-in-a-listview"></a>Włączanie obsługi gestu wzorca Tap w ListView

Na platformie Universal Windows domyślnie Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) korzysta z natywnym `ItemClick` zdarzenie, aby odpowiedzieć na interakcję, a nie w natywnym `Tapped` zdarzeń. Dzięki temu funkcji ułatwień dostępu, aby Windows Narrator i klawiatury mogą wchodzić w interakcje z `ListView`. Jednak również renderuje wszystkie gesty wzorca tap wewnątrz `ListView` przestanie działać.

To określa specyficzne dla platformy czy elementy w [ `ListView` ](xref:Xamarin.Forms.ListView) mogą odpowiadać na naciśnij gestów i dlatego czy natywnych `ListView` generowane `ItemClick` lub `Tapped` zdarzeń. Jest używany w XAML, ustawiając [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) dołączona właściwość na wartość [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) wyliczenia:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być wykorzystywane za pomocą języka C# przy użyciu wygodnego interfejsu API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>` Metoda określa, że określonych platform będzie uruchamiane tylko na platformie Universal Windows. [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw jest używana do kontroli tego, czy elementy w [ `ListView` ](xref:Xamarin.Forms.ListView) mogą odpowiadać na naciśnij gesty, przy użyciu [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) wyliczenie, zapewniając dwóch wartości:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) — Wskazuje, że `ListView` nastąpi natywnych `ItemClick` zdarzenia w celu obsługi interakcji i dlatego udostępniają funkcje ułatwień dostępu. W związku z tym, Windows Narrator i klawiatury może interakcyjnie przeprowadzić `ListView`. Jednak elementy w `ListView` nie może odpowiadać na naciśnij gestów. Jest to domyślne zachowanie dla `ListView` wystąpień na platformie Universal Windows.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) — Wskazuje, że `ListView` nastąpi natywnych `Tapped` zdarzenia w celu obsługi interakcji. W związku z tym, elementy w `ListView` mogą odpowiadać na naciśnij gestów. Jednak istnieje żadne funkcje ułatwień dostępu i dlatego Windows Narrator i klawiatury nie mogą współdziałać z `ListView`.

> [!NOTE]
> `Accessible` i `Inaccessible` tryby wyboru wykluczają się wzajemnie, a, musisz wybrać dostępny [ `ListView` ](xref:Xamarin.Forms.ListView) lub `ListView` , mogą odpowiadać na naciśnij gestów.

Ponadto [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) metoda może służyć do zwrócenia bieżącego [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

Wynik jest fakt, że określonym [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) jest stosowany do [ `ListView` ](xref:Xamarin.Forms.ListView), które kontrolki czy elementy w `ListView` mogą odpowiadać na gesty, naciśnij i dlatego czy natywnych `ListView` generowane `ItemClick` lub `Tapped` zdarzeń.

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak używać Windows specyficznych dla platformy, które są wbudowane w platformy Xamarin.Forms. Zezwalaj na specyficznych dla platformy, umożliwiają korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe programy renderujące lub efekty.

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie funkcji specyficznych dla platformy](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
