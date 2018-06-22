---
title: Dodawanie danych do kolekcji elementów selektora
description: Widok selektora jest formant wyboru z listy danych elementu tekstowego. W tym artykule opisano sposób wypełnienia selektora z danymi przez dodanie go do kolekcji elementów oraz jak reagować na wybór elementu przez użytkownika.
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 63a72861895f79d2d0154297f88610ddb8bb8beb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30792657"
---
# <a name="adding-data-to-a-pickers-items-collection"></a>Dodawanie danych do kolekcji elementów selektora

_Widok selektora jest formant wyboru z listy danych elementu tekstowego. W tym artykule opisano sposób wypełnienia selektora z danymi przez dodanie go do kolekcji elementów oraz jak reagować na wybór elementu przez użytkownika._

## <a name="populating-a-picker-with-data"></a>Wypełnianie selektora z danymi

Przed platformy Xamarin.Forms 2.3.4, procesu wypełniania [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) z danymi było dodać dane mają być wyświetlane tylko do odczytu [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekcji, która jest typu `IList<string>`. Każdy element w kolekcji musi być typu `string`. Można dodać elementy w języku XAML, inicjowanie `Items` właściwości z listą `x:String` elementów:

```xaml
<Picker Title="Select a monkey">
  <Picker.Items>
    <x:String>Baboon</x:String>
    <x:String>Capuchin Monkey</x:String>
    <x:String>Blue Monkey</x:String>
    <x:String>Squirrel Monkey</x:String>
    <x:String>Golden Lion Tamarin</x:String>
    <x:String>Howler Monkey</x:String>
    <x:String>Japanese Macaque</x:String>
  </Picker.Items>
</Picker>
```

Poniżej przedstawiono równoważne kodu C#:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.Items.Add("Baboon");
picker.Items.Add("Capuchin Monkey");
picker.Items.Add("Blue Monkey");
picker.Items.Add("Squirrel Monkey");
picker.Items.Add("Golden Lion Tamarin");
picker.Items.Add("Howler Monkey");
picker.Items.Add("Japanese Macaque");
```

Oprócz dodania danych przy użyciu `Items.Add` metody danych również można wstawiać do kolekcji przy użyciu `Items.Insert` metody.

## <a name="responding-to-item-selection"></a>Odpowiada na zaznaczanie elementów

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) obsługuje wybór jednego elementu na raz. Po wybraniu elementu [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) generowane zdarzenie i [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) właściwości są aktualizowane na liczbę całkowitą reprezentującą indeks wybranego elementu na liście. `SelectedIndex` Właściwość jest liczony od zera liczbę określającą, którą użytkownik wybrał element. Jeśli nie wybrano elementów, co ma miejsce po `Picker` najpierw jest tworzony i inicjowany, `SelectedIndex` będzie mieć wartość -1.

> [!NOTE]
> Element zachowanie dotyczące wyboru w [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) można dostosować w systemie iOS z poszczególnych platform. Aby uzyskać więcej informacji, zobacz [kontrolowanie Wybór elementu selektora](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Poniższy kod przedstawia przykład `OnPickerSelectedIndexChanged` metoda obsługi zdarzeń, które jest wykonywane, kiedy [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) generowane zdarzenie:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = picker.Items[selectedIndex];
  }
}
```

Ta metoda uzyskuje [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) wartość właściwości i korzysta z wartości można pobrać wybrany element z [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekcji. Ponieważ każdy element `Items` kolekcja jest `string`, może być wyświetlany przez [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) bez konieczności rzutowanie.

> [!NOTE]
> A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) mogą być inicjowane można wyświetlić określonego elementu, ustawiając [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) właściwości. Jednak `SelectedIndex` po inicjowania musi być ustawiona właściwość [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekcji.

## <a name="summary"></a>Podsumowanie

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Widok jest formant wyboru z listy danych elementu tekstowego. W tym artykule wyjaśniono sposób wypełnienia `Picker` z danymi, dodając ją do [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekcji i jak reagować na wybór elementu przez użytkownika. To proces użycia `Picker` przed 2.3.4 platformy Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [Wersja demonstracyjna selektora (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Selektor](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
