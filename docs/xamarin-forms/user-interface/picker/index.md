---
title: Selektor
description: Widok selektora jest formant wyboru z listy danych elementu tekstowego.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 7f0050351ca28d7f8afeb82a85e82e51d399824b
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847501"
---
# <a name="picker"></a>Selektor

_Widok selektora jest formant wyboru z listy danych elementu tekstowego._

Platformy Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker) Wyświetla krótką listę elementów, z których użytkownik może wybrać element. `Picker` Definiuje właściwości osiem:

- [`Title`](xref:Xamarin.Forms.Picker.Title) typu `string`, jakie nie `null`.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) typu `IList`, źródło listę elementów do wyświetlenia, która domyślnie określa `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) typu `int`, indeks wybranego elementu, która domyślnie określa wartość -1.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) typu `object`, wybranego elementu, który domyślnie `null`.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) typu [ `Color` ](xref:Xamarin.Forms.Color), kolor używany do wyświetlania tekstu, która domyślnie określa [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) typu [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), jakie nie [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) typu `string`, jakie nie `null`.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) typu `double`, jakie nie -1,0.

Wszystkie właściwości osiem bazują na [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) obiektów, które oznacza, że mogą one być stylem, a właściwości mogą być elementami docelowymi powiązania danych. [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) i [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) właściwości mają domyślny tryb powiązania z [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), co oznacza, że mogą być elementami docelowymi powiązania danych w aplikacji, która używa [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architektury. Informacji o właściwościach czcionki ustawienia, zobacz [czcionki](~/xamarin-forms/user-interface/text/fonts.md).

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) nie wyświetla żadnych danych, gdy jest najpierw wyświetlany. Zamiast tego wartość jego [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) właściwości jest wyświetlany jako element zastępczy w systemach iOS i Android platformy:

[![](images/picker-initial.png "Początkowa wyświetlania selektora")](images/picker-initial-large.png#lightbox "początkowa wyświetlania selektora")

Gdy [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) zyski fokus, jego danych jest wyświetlany, a użytkownik może wybrać element:

[![](images/picker-selection.png "Selektor zaznaczenie elementu")](images/picker-selection-large.png#lightbox "selektora zaznaczenie elementu")

[ `Picker` ](xref:Xamarin.Forms.Picker) Uruchamiany [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) zdarzenie, gdy użytkownik wybiera element. Następujące zaznaczenia, wybrany element jest wyświetlany przez `Picker`:

![](images/picker-after-selection.png "Selektor po zaznaczeniu")

Istnieją dwie metody w celu wypełnienia [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) z danymi:

- Ustawienie [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) właściwości do danych, które mają być wyświetlane. Jest to zalecane technika, którą wprowadzono w 2.3.4 platformy Xamarin.Forms. Aby uzyskać więcej informacji, zobacz [ustawienie właściwości ItemsSource selektora](populating-itemssource.md).
- Dodawanie danych do wyświetlenia na [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekcji. Ta metoda została oryginalnego procesu wypełniania [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) z danymi. Aby uzyskać więcej informacji, zobacz [Dodawanie danych do kolekcji elementów selektora](populating-items.md).

## <a name="related-links"></a>Linki pokrewne

- [Selektor](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
