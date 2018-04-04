---
title: Selektor
description: Widok selektora jest formant wyboru z listy danych elementu tekstowego.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 9889502b635997dbb5e2b79a7654bf1ff0c99861
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="picker"></a>Selektor

_Widok selektora jest formant wyboru z listy danych elementu tekstowego._

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Wyświetla krótką listę elementów, z których użytkownik może wybrać. Jednak [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) nie wyświetla żadnych danych, gdy jest najpierw wyświetlany. Zamiast tego wartość jego [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) właściwości jest wyświetlany jako element zastępczy w systemach iOS i Android platformy:

[![](images/picker-initial.png "Początkowa wyświetlania selektora")](images/picker-initial-large.png#lightbox "początkowa wyświetlania selektora")

Gdy [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) zyski fokus, jego danych jest wyświetlany, a użytkownik może wybrać element:

[![](images/picker-selection.png "Selektor zaznaczenie elementu")](images/picker-selection-large.png#lightbox "selektora zaznaczenie elementu")

Następujące zaznaczenia, wybrany element jest wyświetlany przez [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/):

![](images/picker-after-selection.png "Selektor po zaznaczeniu")

Istnieją dwie metody w celu wypełnienia [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) z danymi:

- Ustawienie [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) właściwości do danych, które mają być wyświetlane. Jest to zalecane technika, którą wprowadzono w 2.3.4 platformy Xamarin.Forms. Aby uzyskać więcej informacji, zobacz [ustawienie właściwości ItemsSource selektora](populating-itemssource.md).
- Dodawanie danych do wyświetlenia na [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekcji. Ta metoda została oryginalnego procesu wypełniania [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) z danymi. Aby uzyskać więcej informacji, zobacz [Dodawanie danych do kolekcji elementów selektora](populating-items.md).


## <a name="related-links"></a>Linki pokrewne

- [Selektor](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
