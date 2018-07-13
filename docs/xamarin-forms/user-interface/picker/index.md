---
title: Selektor zestawu narzędzi Xamarin.Forms
description: Selektor zestawu narzędzi Xamarin.Forms Wyświetla krótką listę elementów, z których użytkownik może wybrać element. W tym artykule opisano sposób użycia klasy selektora, aby wybrać element tekstu z wykaz danych.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: c852cd29197b000ed1ff53853d64cfa25fb699e7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996600"
---
# <a name="xamarinforms-picker"></a>Selektor zestawu narzędzi Xamarin.Forms

_Widok selektora jest kontrolka służąca do wybierania elementu tekstowego z listą danych._

Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker) Wyświetla krótką listę elementów, z których użytkownik może wybrać element. `Picker` Definiuje właściwości osiem:

- [`Title`](xref:Xamarin.Forms.Picker.Title) typu `string`, które domyślnie używa `null`.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) typu `IList`, źródło listę elementów do wyświetlenia, która domyślnie `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) typu `int`, indeks zaznaczonego elementu, którego wartość domyślna to -1.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) typu `object`, wybranego elementu, którego wartość domyślna to `null`.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) typu [ `Color` ](xref:Xamarin.Forms.Color), kolor używany do wyświetlania tekstu, która domyślnie [ `Color.Default` ](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) typu [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), które domyślnie używa [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) typu `string`, które domyślnie używa `null`.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) typu `double`, którego wartość domyślna to od – 1,0.

Wszystkie właściwości osiem są wspierane przez [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) obiektów, które oznacza, że mogą one być różne i właściwości mogą być celami powiązania danych. [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) i [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) właściwości mają domyślny tryb powiązania z [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), co oznacza, że mogą być celami powiązania danych w aplikacji, która używa [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architektury. Aby uzyskać informacji na temat właściwości czcionki ustawień, zobacz [czcionki](~/xamarin-forms/user-interface/text/fonts.md).

A [ `Picker` ](xref:Xamarin.Forms.Picker) nie wyświetla żadnych danych, gdy najpierw jest wyświetlany. Zamiast tego wartość jego [ `Title` ](xref:Xamarin.Forms.Picker.Title) właściwości jest wyświetlana jako symbol zastępczy w systemach iOS i Android platform:

[![](images/picker-initial.png "Wstępne wyświetlanie selektora")](images/picker-initial-large.png#lightbox "początkowej wyświetlania selektora")

Gdy [ `Picker` ](xref:Xamarin.Forms.Picker) uzyskuje fokus, jego danych jest wyświetlana, a użytkownik może wybrać element:

[![](images/picker-selection.png "Selektor zaznaczenie elementu")](images/picker-selection-large.png#lightbox "selektora zaznaczenie elementu")

[ `Picker` ](xref:Xamarin.Forms.Picker) Generowane [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) zdarzenie, gdy użytkownik wybierze element. Następujące zaznaczenia, jest wyświetlany wybrany element `Picker`:

![](images/picker-after-selection.png "Selektor po zaznaczeniu")

Istnieją dwie techniki zapełnianie [ `Picker` ](xref:Xamarin.Forms.Picker) z danymi:

- Ustawienie [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) właściwości do danych, które mają być wyświetlane. Jest to zalecana technika, którą wprowadzono w interfejsie Xamarin.Forms 2.3.4. Aby uzyskać więcej informacji, zobacz [Ustawianie właściwości ItemsSource selektora](populating-itemssource.md).
- Dodawanie danych ma być wyświetlany [ `Items` ](xref:Xamarin.Forms.Picker.Items) kolekcji. Ta technika była oryginalnego procesu wypełniania [ `Picker` ](xref:Xamarin.Forms.Picker) z danymi. Aby uzyskać więcej informacji, zobacz [Dodawanie danych do kolekcji elementów selektora](populating-items.md).

## <a name="related-links"></a>Linki pokrewne

- [Selektor](xref:Xamarin.Forms.Picker)
