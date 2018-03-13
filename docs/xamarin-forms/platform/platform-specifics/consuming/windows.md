---
title: "Platformy systemu Windows — szczegóły"
description: "Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty. W tym artykule pokazano, jak korzystać z systemu Windows platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 01b32d1ca5356ed69480dff0d31c210a8c460417
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="windows-platform-specifics"></a>Platformy systemu Windows — szczegóły

_Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty. W tym artykule pokazano, jak korzystać z systemu Windows platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms._

W systemie Windows platformy Uniwersalnej, platformy Xamarin.Forms zawiera następujące szczegóły platformy:

- Opcje położenie paska narzędzi. Aby uzyskać więcej informacji, zobacz [zmiana rozmieszczenia narzędzi](#toolbar_placement).
- Częściowo zwijane [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) paska nawigacyjnego. Aby uzyskać więcej informacji, zobacz [zwijanie paska nawigacyjnego MasterDetailPage](#collapsable_navigation_bar).

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

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono sposób korzystać z systemu Windows platformy — szczegółowe informacje na temat wbudowanych w platformy Xamarin.Forms. Szczegóły platformy pozwalają na korzystanie z funkcji, które są dostępne tylko na danej platformie, bez stosowania niestandardowe moduły renderowania lub efekty.


## <a name="related-links"></a>Linki pokrewne

- [Tworzenie funkcji specyficznych dla platformy](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (przykład)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
