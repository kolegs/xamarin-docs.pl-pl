---
title: Komórki zestawu narzędzi Xamarin.Forms
description: Zestaw narzędzi Xamarin.Forms komórek można dodać ListViews i TableViews. W tym artykule wymieniono komórek uwzględnione w interfejsie Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 3b69aaf0a10468a5950e0ccf5a61ab6ecbbc110f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995923"
---
# <a name="xamarinforms-cells"></a>Komórki zestawu narzędzi Xamarin.Forms

_Zestaw narzędzi Xamarin.Forms komórek można dodać ListViews i TableViews._

A *komórki* jest elementem wyspecjalizowane używane na potrzeby elementów w tabeli, a w tym artykule opisano, jak każdy element na liście powinien być renderowany. [ `Cell` ](xref:Xamarin.Forms.Cell) Klasa pochodzi od [ `Element` ](xref:Xamarin.Forms.Element), z którego [ `VisualElement` ](xref:Xamarin.Forms.Element) również pochodzi. Komórka nie stanowi elementu wizualnego; Zamiast tego jest szablon służący do tworzenia elementu wizualnego.

`Cell` jest używane wyłącznie w przypadku [ `ListView` ](views.md#listView) i [ `TableView` ](views.md#tableView) kontrolki. Aby dowiedzieć się, jak używać i dostosowywanie komórek, zapoznaj się [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md) i [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) dokumentacji.

## <a name="cells"></a>Komórki

Zestaw narzędzi Xamarin.Forms obsługuje następujące typy komórki:

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| A [ `TextCell` ](xref:Xamarin.Forms.TextCell) zawiera jeden lub dwa ciągi tekstowe. Ustaw [ `Text` ](xref:Xamarin.Forms.TextCell.Text) właściwości i, opcjonalnie, [ `Detail` ](xref:Xamarin.Forms.TextCell.Detail) właściwości tych ciągów tekstowych.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.TextCell) / [przewodnik](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![Przykład TextCell](cells-images/TextCell.png "przykład TextCell")](cells-images/TextCell-Large.png#lightbox "przykład TextCell")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| [ `ImageCell` ](xref:Xamarin.Forms.ImageCell) Wyświetla te same informacje co [ `TextCell` ](#textCell) , ale zawiera mapę bitową z ustawionym [ `Source` ](xref:Xamarin.Forms.Image.Source) właściwości.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.ImageCell) / [przewodnik](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![Przykład ImageCell](cells-images/ImageCell.png "przykład ImageCell")](cells-images/ImageCell-Large.png#lightbox "przykład ImageCell")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| [ `SwitchCell` ](xref:Xamarin.Forms.SwitchCell) Zawiera tekstu, ustawianego przy użyciu [ `Text`"](xref:Xamarin.Forms.SwitchCell.Text) właściwości i i/wyłącznik początkowo ustawiona za pomocą typu Boolean [ `On` ](xref:Xamarin.Forms.SwitchCell.On) właściwości. Obsługa [ `OnChanged` ](xref:Xamarin.Forms.SwitchCell.OnChanged) zdarzeń, aby otrzymywać powiadomienia, gdy `On` zmiany właściwości.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.SwitchCell) / [przewodnik](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Przykład SwitchCell](cells-images/SwitchCell.png "przykład SwitchCell")](cells-images/SwitchCell-Large.png#lightbox "przykład SwitchCell")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| [ `EntryCell` ](xref:Xamarin.Forms.EntryCell) Definiuje [ `Label` ](xref:Xamarin.Forms.EntryCell.Label) właściwość, która identyfikuje komórkę i pojedynczy wiersz tekstu można edytować w [ `Text` ](xref:Xamarin.Forms.EntryCell.Text) właściwości. Obsługa [ `Completed` ](xref:Xamarin.Forms.EntryCell.Completed) zdarzeń, aby otrzymywać powiadomienia, gdy użytkownik zakończy wprowadzania tekstu.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.EntryCell) / [przewodnik](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![Przykład EntryCell](cells-images/EntryCell.png "przykład EntryCell")](cells-images/EntryCell-Large.png#lightbox "przykład EntryCell")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do zestawu narzędzi Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Przykładowe FormsGallery zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
