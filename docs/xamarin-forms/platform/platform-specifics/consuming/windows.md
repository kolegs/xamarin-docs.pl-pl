---
title: Platformy systemu Windows — szczegóły
description: Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty. W tym artykule pokazano, jak korzystać z systemu Windows platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 7299de658a3491928e9bbeaa4dd192a8e95c435e
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732804"
---
# <a name="windows-platform-specifics"></a>Platformy systemu Windows — szczegóły

_Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty. W tym artykule pokazano, jak korzystać z systemu Windows platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms._

W systemie Windows platformy Uniwersalnej, platformy Xamarin.Forms zawiera następujące szczegóły platformy:

- Ustawianie opcji położenia paska narzędzi. Aby uzyskać więcej informacji, zobacz [zmiana rozmieszczenia narzędzi](#toolbar_placement).
- Zwijanie [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) paska nawigacyjnego. Aby uzyskać więcej informacji, zobacz [zwijanie paska nawigacyjnego MasterDetailPage](#collapsable_navigation_bar).
- Włączanie [ `WebView` ](xref:Xamarin.Forms.WebView) do wyświetlania alertów JavaScript w oknie dialogowym komunikatu platformy uniwersalnej systemu Windows. Aby uzyskać więcej informacji, zobacz [wyświetlanie alertów JavaScript](#webview-javascript-alert).
- Włączanie [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) wchodzić w interakcje z aparatu sprawdzania pisowni. Aby uzyskać więcej informacji, zobacz [włączenie sprawdzania pisowni SearchBar](#searchbar-spellcheck).
- Wykrywanie kolejność odczytywania z pliku tekstowego zawartość [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), i [ `Label` ](xref:Xamarin.Forms.Label) wystąpień. Aby uzyskać więcej informacji, zobacz [wykrywanie kolejność czytania od zawartości](#inputview-readingorder).
- Wyłączenie trybu starszych kolor na obsługiwanej [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Aby uzyskać więcej informacji, zobacz [wyłączenie trybu koloru starszych](#legacy-color-mode).
- Włączanie obsługi gestów naciśnij w [ `ListView` ](xref:Xamarin.Forms.ListView). Aby uzyskać więcej informacji, zobacz [włączenie wybierz gestu pomocy technicznej w elemencie ListView](#listview-selectionmode).

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>Zmienianie rozmieszczenia paska narzędzi

Poszczególnych platform służy do zmiany rozmieszczenia paska narzędzi na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)i jest używane w języku XAML, ustawiając [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/) dołączona właściwość na wartość [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) wyliczenie:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

`Page.On<Windows>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie Windows. [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) przestrzeni nazw jest używana do ustawiania położenie paska narzędzi z [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) dostarczanie — wyliczenie trzy wartości: [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default/), [ `Top` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top/), i [ `Bottom` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom/).

Wynik jest zastosowana do umieszczania określonego narzędzi [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) wystąpienie:

[![](windows-images/toolbar-placement.png "Pasek narzędzi umieszczania specyficzne dla platformy")](windows-images/toolbar-placement-large.png#lightbox "narzędzi umieszczania specyficzne dla platformy")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Zwijanie MasterDetailPage paska nawigacyjnego

Umożliwia Zwiń na pasku nawigacyjnym poszczególnych platform [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)i jest używane w języku XAML, ustawiając [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/) i [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)dołączone do właściwości:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`MasterDetailPage.On<Windows>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie Windows. [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) — Metoda w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) przestrzeni nazw jest używana do określania stylu Zwiń z [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) udostępnia dwa — wyliczenie wartości: [ `Full` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full/) i [ `Partial` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial/). [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/) Metoda służy do określania szerokości paska nawigacji częściowo zwinięte.

Wynik jest to, że określonej [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) jest stosowany do [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) wystąpienia o również określonej szerokości:

[![](windows-images/collapsed-navigation-bar.png "Zwinięte specyficzne dla platformy pasek nawigacyjny")](windows-images/collapsed-navigation-bar-large.png#lightbox "zwinięte specyficzne dla platformy paska nawigacyjnego")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>Wyświetlanie alertów JavaScript

Umożliwia to specyficzne dla platformy [ `WebView` ](xref:Xamarin.Forms.WebView) do wyświetlania alertów JavaScript w oknie dialogowym komunikatu platformy uniwersalnej systemu Windows. Jest używany w języku XAML, ustawiając [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) dołączona właściwość do `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

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

`WebView.On<Windows>` Metoda określa, że poszczególnych platform będą uruchamiane tylko na platformy uniwersalnej systemu Windows. [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw, służy do sterowania czy JavaScript alerty są włączone. Ponadto `WebView.SetIsJavaScriptAlertEnabled` metody można użyć do wywoływania włączyć lub wyłączyć alerty JavaScript [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) metody do zwrócenia, czy są włączone:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

Wynik jest, że JavaScript alerty mogą być wyświetlane w oknie dialogowym komunikatu platformy uniwersalnej systemu Windows:

![Ostrzeżenie WebView JavaScript specyficzne dla platformy](windows-images/webview-javascript-alert.png "alert WebView JavaScript specyficzne dla platformy")

<a name="searchbar-spellcheck" />

## <a name="enabling-searchbar-spell-check"></a>Włączenie sprawdzania pisowni SearchBar

Umożliwia to specyficzne dla platformy [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) wchodzić w interakcje z aparatu sprawdzania pisowni. Jest używany w języku XAML, ustawiając [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) dołączona właściwość do `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>` Metoda określa, że poszczególnych platform będą uruchamiane tylko na platformy uniwersalnej systemu Windows. [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw, włącza moduł sprawdzania pisowni i wyłącza. Ponadto `SearchBar.SetIsSpellCheckEnabled` metody można użyć do przełączania sprawdzania pisowni, wywołując [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) metody do zwrócenia, czy włączone jest sprawdzanie pisowni:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Wynik jest ten tekst wprowadzony w [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) może być sprawdzana pisownia, z niepoprawne pisowni są określone dla użytkownika:

![SearchBar pisowni wyboru specyficzne dla platformy](windows-images/searchbar-spellcheck.png "SearchBar pisowni wyboru specyficzne dla platformy")

> [!NOTE]
> `SearchBar` Klasy w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw ma również [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) i [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) metod, które mogą służyć do włączania i wyłączania Moduł sprawdzania pisowni na `SearchBar`odpowiednio.

<a name="inputview-readingorder" />

## <a name="detecting-reading-order-from-content"></a>Wykrywanie kolejność czytania od zawartości

Kolejność odczytu (od lewej do prawej lub lewej do prawej) tekstu dwukierunkowego w umożliwia poszczególnych platform [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), i [ `Label` ](xref:Xamarin.Forms.Label) wystąpień, aby można było wykryć dynamicznie. Jest używany w języku XAML, ustawiając [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (dla `Entry` i `Editor` wystąpień) lub [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) dołączona właściwość do `boolean` wartość:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>` Metoda określa, że poszczególnych platform będą uruchamiane tylko na platformy uniwersalnej systemu Windows. [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw, służy do sterowania czy wykryto kolejność czytania od zawartości w [ `InputView` ](xref:Xamarin.Forms.InputView). Ponadto `InputView.SetDetectReadingOrderFromContent` metoda służy do przełączania, czy wykryto kolejność czytania od zawartości przez wywołanie metody [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) metody do zwracania bieżącej wartości:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

W wyniku [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), i [ `Label` ](xref:Xamarin.Forms.Label) wystąpienia mogą mieć kolejność odczytu ich zawartość wykryto dynamicznie:

[![InputView wykrywanie kolejność czytania od zawartości specyficzne dla platformy](windows-images/editor-readingorder.png "InputView wykrywanie kolejność czytania od zawartości specyficzne dla platformy")](windows-images/editor-readingorder-large.png#lightbox "InputView wykrywanie kolejność czytania od zawartość specyficzne dla platformy")

> [!NOTE]
> W przeciwieństwie do ustawienia [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) właściwości, logikę widoki, które wykrywa kolejność czytania od ich zawartości tekstowej nie wpłynie na wyrównanie tekstu w widoku. Zamiast tego można dostosować kolejność, w którym określono układ bloki tekstu dwukierunkowego.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Wyłączenie starsza wersja trybu koloru

Niektóre widoki platformy Xamarin.Forms funkcji trybu koloru starszej wersji. W tym trybie podczas [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) właściwości widoku ustawiono `false`, widok spowoduje zastąpienie kolory ustawionych przez użytkownika z domyślne kolory natywnego stanie wyłączenia. Dla zapewnienia zgodności, w tym trybie Kolor starszej wersji pozostaje domyślne zachowanie dla obsługiwane widoki.

Poszczególnych platform wyłącza ten tryb kolor starszej wersji, aby kolory, ustaw dla widoku przez użytkownika pozostają nawet wtedy, gdy widok jest wyłączony. Jest używany w języku XAML, ustawiając [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) dołączona właściwość do `false`:

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

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Windows>` — Metoda określa, czy ten specyficzne dla platformy działa tylko w systemie Windows. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) Metody w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw, służy do sterowania czy tryb starszej wersji kolorów jest wyłączona. Ponadto [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) metody można użyć do zwrócenia, czy tryb starszej wersji kolorów jest wyłączone.

Wynik jest czy kolor starszej wersji trybu można ją wyłączyć, dzięki czemu kolorów, ustaw dla widoku przez użytkownika pozostają nawet wtedy, gdy widok jest wyłączony:

![](windows-images/legacy-color-mode-disabled.png "Wyłączono tryb starszej wersji kolorów")

> [!NOTE]
> Podczas ustawiania [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) w widoku, tryb starszej wersji kolor całkowicie jest ignorowany. Aby uzyskać więcej informacji na temat stany wizualne, zobacz [platformy Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="listview-selectionmode" />

## <a name="enabling-tap-gesture-support-in-a-listview"></a>Włączanie obsługi gestów naciśnij w elemencie ListView

Na platformy uniwersalnej systemu Windows, domyślnie platformy Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) używa natywnego `ItemClick` zdarzeń odpowiedzieć interakcji, zamiast natywnego `Tapped` zdarzeń. Zapewnia funkcje ułatwień dostępu, aby interaktywnie Narrator systemu Windows i klawiatury `ListView`. Jednak również renderuje wszystkie gesty naciśnij wewnątrz `ListView` przestanie działać.

To określa specyficzne dla platformy czy elementy w [ `ListView` ](xref:Xamarin.Forms.ListView) mogą odpowiadać na wybierz gesty i dlatego czy natywnego `ListView` uruchamiany `ItemClick` lub `Tapped` zdarzeń. Jest używany w języku XAML, ustawiając [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) dołączona właściwość na wartość [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) wyliczenie:

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

Alternatywnie mogą być używane w języku C# przy użyciu interfejsu API fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>` Metoda określa, że poszczególnych platform będą uruchamiane tylko na platformy uniwersalnej systemu Windows. [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) — Metoda w [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) przestrzeni nazw jest używana do kontroli określa, czy elementy w [ `ListView` ](xref:Xamarin.Forms.ListView) mogą odpowiadać na dotknięcie gesty, [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) wyliczenie udostępnia dwa możliwe wartości:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) — Wskazuje, że `ListView` uruchomią natywnego `ItemClick` zdarzeń do obsłużenia interakcji i dlatego zapewniać funkcje ułatwień dostępu. W związku z tym Narrator systemu Windows i klawiatury może interakcyjnie `ListView`. Jednak elementy w `ListView` nie może odpowiadać na wybierz gestów. Jest to domyślne zachowanie dla `ListView` wystąpień na platformy uniwersalnej systemu Windows.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) — Wskazuje, że `ListView` uruchomią natywnego `Tapped` zdarzeń do obsługi interakcji. W związku z tym pozycje `ListView` mogą odpowiadać na wybierz gestów. Jednak nie ma żadnych funkcji ułatwień dostępu i dlatego Narrator systemu Windows i klawiatury nie mogą oddziaływać na `ListView`.

> [!NOTE]
> `Accessible` i `Inaccessible` tryby wyboru wykluczają się wzajemnie i należy wybrać dostępny [ `ListView` ](xref:Xamarin.Forms.ListView) lub `ListView` który mogą odpowiadać na wybierz gestów.

Ponadto [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) metody można użyć do zwracania bieżącej [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

Wynik jest to, że określonej [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) jest stosowany do [ `ListView` ](xref:Xamarin.Forms.ListView), które kontrolki czy elementy w `ListView` mogą odpowiadać na wybierz gesty i dlatego czy natywnego `ListView` uruchamiany `ItemClick` lub `Tapped` zdarzeń.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób korzystać z systemu Windows platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms. Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty.

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie funkcji specyficznych dla platformy](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
