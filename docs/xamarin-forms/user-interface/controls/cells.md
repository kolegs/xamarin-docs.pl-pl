---
title: Xamarin.Forms Cells
description: "Widokach listy i TableViews można dodać komórek platformy Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 509ecc509754bba544115c140e619f634bd64eae
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms Cells

_Widokach listy i TableViews można dodać komórek platformy Xamarin.Forms._

<style></style>

## <a name="cells"></a>Komórki

Komórki jest elementem specjalne używany do elementów w tabeli i opisano, w jaki sposób ma być rysowany każdy element na liście. Komórka jest pochodną [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), z którego VisualElement również pochodzi. Jednak komórki nie jest element wizualny, po prostu opisuje szablonem służącym do tworzenia elementu wizualnego. [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) udostępnia klasę podstawową i możliwości we wszystkich komórkach platformy Xamarin.Forms. Komórki są przeznaczone do dodania do elementów [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) lub [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) kontrolki.

Aby dowiedzieć się, jak i dostosowywanie komórek, zajrzyj do [ListView](~/xamarin-forms/user-interface/listview/index.md) i [TableView](~/xamarin-forms/user-interface/tableview.md) dokumentacji.

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong></strong>
    </th>
    <th>
      <strong>Opis elementu</strong>
    </th>
    <th style="min-width:400px">
      <strong>Zrzut ekranu</strong>
    </th>
  </thead>
  <tbody>
    <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> z etykiety i pola wejścia pojedynczy wiersz tekstu.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="cells-images/EntryCell.png" title="Przykład EntryCell" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> z etykiety i włączony/wyłączony.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchCellDemoPage.cs"><img src="cells-images/SwitchCell.png" title="Przykład SwitchCell" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> tekstem podstawowego i pomocniczego.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TextCellDemoPage.cs"><img src="cells-images/TextCell.png" title="Przykład TextCell" class="tableimg">
    </a></td>
  </tr>
      <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">tekstu komórki</a> zawierającego obraz.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageCellDemoPage.cs"><img src="cells-images/ImageCell.png" title="Przykład ImageCell" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Galeria platformy Xamarin.Forms (przykład)](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
