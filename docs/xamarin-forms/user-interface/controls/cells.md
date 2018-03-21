---
title: Xamarin.Forms Cells
description: "Widokach listy i TableViews można dodać komórek platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 0f8886546004702adbdbca7d991c67d5700e453e
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2018
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms Cells

_Widokach listy i TableViews można dodać komórek platformy Xamarin.Forms._

A *komórki* jest elementem specjalne używany do elementów w tabeli i w tym artykule opisano, jak każdy element na liście powinien być renderowany. [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) Pochodną klasy [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), z którego [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) również pochodzi. Komórki nie stanowi elementu wizualnego; Zamiast tego jest szablonem służącym do tworzenia elementu wizualnego. 

`Cell` jest używane wyłącznie w przypadku [ `ListView` ](views.md#listView) i [ `TableView` ](views.md#tableView) kontrolki. Aby dowiedzieć się, jak i dostosowywanie komórek, zajrzyj do [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md) i [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) dokumentacji.

## <a name="cells"></a>Komórki

Platformy Xamarin.Forms obsługuje następujące typy komórki:

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| A [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) zawiera jeden lub dwa ciągi tekstowe. Ustaw [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) właściwości i, opcjonalnie, [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) właściwości te ciągi.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) / [przewodnik](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![Przykład TextCell](cells-images/TextCell.png "przykład TextCell")](cells-images/TextCell-Large.png#lightbox "TextCell przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) Wyświetla te same informacje co [ `TextCell` ](#textCell) , ale zawiera mapę bitową ustawioną z [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) właściwości.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) / [przewodnik](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![Przykład ImageCell](cells-images/ImageCell.png "przykład ImageCell")](cells-images/ImageCell-Large.png#lightbox "przykład ImageCell")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| [ `SwitchCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) Zawiera tekstem z [ `Text`"](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCellText/) właściwości i i włączania/wyłączania początkowo ustawiona z wartością logiczną [ `On` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCell.On/) właściwości. Obsługa [ `OnChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SwitchCell.OnChanged/) zdarzenie zgłaszane po `On` zmiany właściwości.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) / [przewodnik](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Przykład SwitchCell](cells-images/SwitchCell.png "przykład SwitchCell")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| [ `EntryCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) Definiuje [ `Label` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Label/) właściwość, która identyfikuje komórkę i edycji tekstu w jednym wierszu [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Text/) właściwości. Obsługa [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.EntryCell.Completed/) zdarzeń, aby otrzymywać powiadomienia, gdy użytkownik zakończy wpisywanie tekstu.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) / [przewodnik](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell Example](cells-images/EntryCell.png "EntryCell Example")](cells-images/EntryCell-Large.png#lightbox "EntryCell Example")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Przykładowe FormsGallery platformy Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
