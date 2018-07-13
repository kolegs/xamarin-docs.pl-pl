---
title: Dodawanie danych do kolekcji elementów selektora
description: Widok selektora jest kontrolka służąca do wybierania elementu tekstowego z listą danych. W tym artykule opisano sposób wypełnić selektora z danymi, dodając ją do kolekcji elementów oraz odpowiadanie na wybór elementu przez użytkownika.
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 8d911108d7d72586a37a3281803eab9c0841f16c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997113"
---
# <a name="adding-data-to-a-pickers-items-collection"></a>Dodawanie danych do kolekcji elementów selektora

_Widok selektora jest kontrolka służąca do wybierania elementu tekstowego z listą danych. W tym artykule opisano sposób wypełnić selektora z danymi, dodając ją do kolekcji elementów oraz odpowiadanie na wybór elementu przez użytkownika._

## <a name="populating-a-picker-with-data"></a>Wypełnianie selektora z danymi

Przed Xamarin.Forms 2.3.4, proces wypełnianie [ `Picker` ](xref:Xamarin.Forms.Picker) przy użyciu danych było dodać dane, które ma być wyświetlany tylko do odczytu [ `Items` ](xref:Xamarin.Forms.Picker.Items) kolekcji, która jest typu `IList<string>`. Każdy element w kolekcji musi być typu `string`. Można dodawać elementy w XAML przez inicjowanie `Items` właściwości z listą `x:String` elementy:

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

Równoważny kod C# jest pokazany poniżej:

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

Oprócz dodawania danych przy użyciu `Items.Add` metody, również można wstawić dane do kolekcji przy użyciu `Items.Insert` metody.

## <a name="responding-to-item-selection"></a>Odpowiadanie na wybór elementu

A [ `Picker` ](xref:Xamarin.Forms.Picker) obsługuje wybór jednego elementu w danym momencie. Gdy użytkownik wybierze element [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) generowane zdarzenia i [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) właściwość zostanie zaktualizowany i będzie liczba całkowita reprezentująca indeks zaznaczonego elementu na liście. `SelectedIndex` Właściwość jest liczony od zera liczbę określającą element wybrany przez użytkownika. Jeśli żaden element nie jest zaznaczone, co ma miejsce w przypadku `Picker` najpierw zostanie utworzony i zainicjowany, `SelectedIndex` będzie mieć wartość -1.

> [!NOTE]
> Element zachowanie podczas zaznaczania w [ `Picker` ](xref:Xamarin.Forms.Picker) można dostosować w systemie iOS przy użyciu określonych platform. Aby uzyskać więcej informacji, zobacz [kontrolowanie zaznaczenie elementu selektora](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Poniższy kod przedstawia przykład `OnPickerSelectedIndexChanged` metody obsługi zdarzeń, który jest wykonywany, gdy [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) generowane zdarzenie:

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

Ta metoda uzyskuje [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) wartości właściwości i używa wartości, aby pobrać zaznaczony element w [ `Items` ](xref:Xamarin.Forms.Picker.Items) kolekcji. Ponieważ każdy element `Items` kolekcja jest `string`, mogą być wyświetlane przez [ `Label` ](xref:Xamarin.Forms.Label) bez rzutowania.

> [!NOTE]
> A [ `Picker` ](xref:Xamarin.Forms.Picker) może być inicjowany do wyświetlania określonego elementu, ustawiając [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) właściwości. Jednak `SelectedIndex` musi być ustawiona właściwość po inicjowanie [ `Items` ](xref:Xamarin.Forms.Picker.Items) kolekcji.

## <a name="summary"></a>Podsumowanie

[ `Picker` ](xref:Xamarin.Forms.Picker) Widok jest kontrolka służąca do wybierania elementu tekstowego z listą danych. W tym artykule wyjaśniono, jak wypełnić `Picker` z danymi, dodając ją do [ `Items` ](xref:Xamarin.Forms.Picker.Items) zbierania i odpowiadania na wybór elementu przez użytkownika. Jest to proces przy użyciu `Picker` przed 2.3.4 platformy Xamarin.Forms.


## <a name="related-links"></a>Linki pokrewne

- [Próbnika demonstracyjna (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Selektor](xref:Xamarin.Forms.Picker)
