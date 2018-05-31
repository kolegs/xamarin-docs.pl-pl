---
title: Platformy systemu Windows — szczegóły
description: Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty. W tym artykule pokazano, jak korzystać z systemu Windows platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2018
ms.openlocfilehash: d4ddb662bf167a0c80561cce097104a7f5fc8096
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/25/2018
ms.locfileid: "34546011"
---
# <a name="windows-platform-specifics"></a>Platformy systemu Windows — szczegóły

_Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty. W tym artykule pokazano, jak korzystać z systemu Windows platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms._

W systemie Windows platformy Uniwersalnej, platformy Xamarin.Forms zawiera następujące szczegóły platformy:

- Ustawianie opcji położenia paska narzędzi. Aby uzyskać więcej informacji, zobacz [zmiana rozmieszczenia narzędzi](#toolbar_placement).
- Zwijanie [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) paska nawigacyjnego. Aby uzyskać więcej informacji, zobacz [zwijanie paska nawigacyjnego MasterDetailPage](#collapsable_navigation_bar).
- Włączanie [ `WebView` ](xref:Xamarin.Forms.WebView) do wyświetlania alertów JavaScript w oknie dialogowym komunikatu platformy uniwersalnej systemu Windows. Aby uzyskać więcej informacji, zobacz [wyświetlanie alertów JavaScript](#webview-javascript-alert).

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

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób korzystać z systemu Windows platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms. Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty.

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie funkcji specyficznych dla platformy](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
