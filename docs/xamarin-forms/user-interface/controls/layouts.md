---
title: "Układy platformy Xamarin.Forms"
description: "Układy platformy Xamarin.Forms są używane do tworzenia kontrolek interfejsu użytkownika do struktury logicznej."
ms.topic: article
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: ecea0f55360fcde7a50c52bb33c45a2c5fff5eeb
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-layouts"></a>Układy platformy Xamarin.Forms

_Układy platformy Xamarin.Forms są używane do tworzenia kontrolek interfejsu użytkownika do struktury logicznej._

<style>.tableimg {max-width: Brak! ważne;}</style>

## <a name="layouts"></a>Układy

[`Layout`](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) Klasy w platformy Xamarin.Forms jest podtypem specjalne widoku, który działa jako kontener dla innych układów lub widoków. Zawiera on zwykle logiki, aby ustawić położenie i rozmiar elementów podrzędnych w aplikacjach platformy Xamarin.Forms.

 [ ![](layouts-images/layouts-sml.png "Typy układ platformy Xamarin.Forms")](layouts-images/layouts.png "typy układ platformy Xamarin.Forms")

<br clear="all" />

Obsługuje platformy Xamarin.Forms:

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Typ</strong>
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
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a>
    </td>
    <td valign="top">
Menedżer układu dla widoków szablonem. Używane w ramach <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/">ControlTemplate</a> do oznaczania, gdy zostanie wyświetlona zawartość będą widoczne.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/Templates/ControlTemplates/SimpleTheme/SimpleTheme/App.xaml"><img src="layouts-images/ContentPresenter.png" title="Przykład ContentPresenter" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a>
    </td>
    <td valign="top">
Element z pojedynczą zawartość. ContentView ma bardzo rzadko używane własnych. Jej celem jest służenie jako klasę podstawową dla widoków złożone zdefiniowane przez użytkownika.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentViewDemoPage.cs"><img src="layouts-images/ContentView.png" title="Przykład ContentView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">Ramki</a>
    </td>
    <td valign="top">
Element zawierający pojedynczy element potomny, z opcjami ramek. Ramki mają domyślnie <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/">Xamarin.Forms.Layout.Padding</a> 20.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/FrameDemoPage.cs"><img src="layouts-images/Frame.png" title="Przykład ramki" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>
    </td>
    <td valign="top">
Wymaga funkcją przewijania, jeśli jest zawartości elementu.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ScrollViewDemoPage.cs"><img src="layouts-images/ScrollView.png" title="Przykład ScrollView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a>
    </td>
    <td valign="top">
Element, który wyświetla zawartość z szablon formantu i klasa podstawowa dla <a href=""/api/type/Xamarin.Forms.ContentView/">ContentView</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="layouts-images/TemplatedView.png" title="Przykład TemplatedView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a>
    </td>
    <td valign="top">
Określa położenie elementów podrzędnych na żądany położenia bezwzględne. Użytkownik, któremu przypisano zakotwiczenia i granice definiuje położenie i rozmiar formantu.
    </td>
    <td valign="top">
      <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/AbsoluteLayoutDemoPage.cs"><img src="layouts-images/AbsoluteLayout.png" title="Przykład AbsoluteLayout" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Siatki</a>
    </td>
    <td valign="top">
Układ zawierający widoki ułożone w wiersze i kolumny.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/GridDemoPage.cs"><img src="layouts-images/Grid.png" title="Przykład siatki" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/%601">układu</a> używającą <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/">ograniczenia</a>s do układu jego elementów podrzędnych.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/RelativeLayoutDemoPage.cs"><img src="layouts-images/RelativeLayout.png" title="Przykład RelativeLayout" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/">układu</a> który określa położenie elementów podrzędnych w jednym wierszu, który może być orientacji w pionie lub poziomie. Ten układ ustawi podrzędne granice automatycznie podczas cyklu układu. Użytkownik, któremu przypisano granice zostaną zastąpione i w związku z tym nie należy ustawiać na element podrzędny przez użytkownika.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StackLayoutDemoPage.cs"><img src="layouts-images/StackLayout.png" title="Przykład StackLayout" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Galeria platformy Xamarin.Forms (przykład)](https://developer.xamarin.com/samples/FormsGallery/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms)
