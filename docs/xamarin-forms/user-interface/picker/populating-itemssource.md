---
title: Ustawianie właściwości ItemsSource selektora
description: Widok selektora jest kontrolka służąca do wybierania elementu tekstowego z listą danych. W tym artykule opisano sposób wypełnić selektora z danymi przez ustawienie właściwości ItemsSource oraz odpowiadanie na wybór elementu przez użytkownika.
ms.prod: xamarin
ms.assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 3f82e4b7d52988bfef9736ace8c476a9cd2da02b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994747"
---
# <a name="setting-a-pickers-itemssource-property"></a>Ustawianie właściwości ItemsSource selektora

_Widok selektora jest kontrolka służąca do wybierania elementu tekstowego z listą danych. W tym artykule opisano sposób wypełnić selektora z danymi przez ustawienie właściwości ItemsSource oraz odpowiadanie na wybór elementu przez użytkownika._

Zestaw narzędzi Xamarin.Forms 2.3.4 ma rozszerzone [ `Picker` ](xref:Xamarin.Forms.Picker) widoku przez dodanie możliwości do wypełniania danymi, ustawiając jego [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) właściwości i pobrać zaznaczony element z [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) właściwości. Ponadto można zmienić kolor tekstu zaznaczonego elementu, ustawiając [ `TextColor` ](xref:Xamarin.Forms.Picker.TextColor) właściwości [ `Color` ](xref:Xamarin.Forms.Color).

## <a name="populating-a-picker-with-data"></a>Wypełnianie selektora z danymi

A [ `Picker` ](xref:Xamarin.Forms.Picker) można wypełnić danymi, ustawiając jego [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) właściwość `IList` kolekcji. Każdy element w kolekcji musi być lub pochodzi od typu `object`. Można dodawać elementy w XAML przez inicjowanie `ItemsSource` właściwości z tablicy elementy:

```xaml
<Picker x:Name="picker" Title="Select a monkey">
  <Picker.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </Picker.ItemsSource>
</Picker>
```

> [!NOTE]
> Należy pamiętać, że `x:Array` element wymaga `Type` atrybut wskazujący typ elementów w tablicy.

Równoważny kod C# jest pokazany poniżej:

```csharp
var monkeyList = new List<string>();
monkeyList.Add("Baboon");
monkeyList.Add("Capuchin Monkey");
monkeyList.Add("Blue Monkey");
monkeyList.Add("Squirrel Monkey");
monkeyList.Add("Golden Lion Tamarin");
monkeyList.Add("Howler Monkey");
monkeyList.Add("Japanese Macaque");

var picker = new Picker { Title = "Select a monkey" };
picker.ItemsSource = monkeyList;
```

## <a name="responding-to-item-selection"></a>Odpowiadanie na wybór elementu

A [ `Picker` ](xref:Xamarin.Forms.Picker) obsługuje wybór jednego elementu w danym momencie. Gdy użytkownik wybierze element [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) generowane zdarzenie [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) właściwość zostanie zaktualizowany i będzie liczba całkowita reprezentująca indeks zaznaczonego elementu na liście i [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) właściwość zostanie zaktualizowany w celu `object` reprezentujący wybranego elementu. [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) Właściwość jest liczony od zera liczbę określającą elementu wybranego użytkownika. Jeśli żaden element nie jest zaznaczone, co ma miejsce w przypadku [ `Picker` ](xref:Xamarin.Forms.Picker) najpierw zostanie utworzony i zainicjowany, `SelectedIndex` będzie mieć wartość -1.

> [!NOTE]
> Element zachowanie podczas zaznaczania w [ `Picker` ](xref:Xamarin.Forms.Picker) można dostosować w systemie iOS przy użyciu określonych platform. Aby uzyskać więcej informacji, zobacz [kontrolowanie zaznaczenie elementu selektora](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Poniższy przykład kodu pokazuje, jak pobrać [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) wartość właściwości z [ `Picker` ](xref:Xamarin.Forms.Picker) w XAML:

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

Równoważny kod C# jest pokazany poniżej:

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

Ponadto program obsługi zdarzeń może być wykonana przy [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) generowane zdarzenie:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = (string)picker.ItemsSource[selectedIndex];
  }
}
```

Ta metoda uzyskuje [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) wartości właściwości i używa wartości, aby pobrać zaznaczony element w [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) kolekcji. To jest funkcjonalnym odpowiednikiem pobierania wybrany element z [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) właściwości. Należy pamiętać, że każdy element na `ItemsSource` typ kolekcji to `object`i dlatego musi być rzutowany `string` do wyświetlenia.

> [!NOTE]
> A [ `Picker` ](xref:Xamarin.Forms.Picker) może być inicjowany do wyświetlania określonego elementu, ustawiając [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) lub [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) właściwości. Jednak należy ustawić te właściwości po inicjowanie [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) kolekcji.

## <a name="populating-a-picker-with-data-using-data-binding"></a>Wypełnianie selektora z danymi przy użyciu powiązania danych

A [ `Picker` ](xref:Xamarin.Forms.Picker) można również wypełnić przy użyciu danych za pomocą powiązania danych można powiązać jej [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) właściwość `IList` kolekcji. W XAML jest to osiągane przy użyciu [ `Binding` ](xref:Xamarin.Forms.Xaml.BindingExtension) — rozszerzenie znaczników:

```xaml
<Picker Title="Select a monkey" ItemsSource="{Binding Monkeys}" ItemDisplayBinding="{Binding Name}" />
```

Równoważny kod C# jest pokazany poniżej:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

[ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) Wiąże danych właściwości `Monkeys` właściwości modelu połączone widoku, który zwraca `IList<Monkey>` kolekcji. Poniższy kod przedstawia przykład `Monkey` klasy, która zawiera cztery właściwości:

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

Podczas tworzenia wiązania do listy obiektów, [ `Picker` ](xref:Xamarin.Forms.Picker) należy dowiedzieć się, których właściwość, aby wyświetlić z każdego obiektu. Jest to osiągane przez ustawienie [ `ItemDisplayBinding` ](xref:Xamarin.Forms.Picker.ItemDisplayBinding) właściwości wymaganej właściwości z każdego obiektu. W przykładach kodu `Picker` jest ustawiony na wyświetlanie każdego `Monkey.Name` wartości właściwości.

### <a name="responding-to-item-selection"></a>Odpowiadanie na wybór elementu

Powiązanie danych może służyć do ustaw obiekt do [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) wartości właściwości po jej zmianie:

```xaml
<Picker Title="Select a monkey"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}"
        SelectedItem="{Binding SelectedMonkey}" />
<Label Text="{Binding SelectedMonkey.Name}" ... />
<Label Text="{Binding SelectedMonkey.Location}" ... />
<Image Source="{Binding SelectedMonkey.ImageUrl}" ... />
<Label Text="{Binding SelectedMonkey.Details}" ... />
```

Równoważny kod C# jest pokazany poniżej:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.SetBinding(Picker.SelectedItemProperty, "SelectedMonkey");
picker.ItemDisplayBinding = new Binding("Name");

var nameLabel = new Label { ... };
nameLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Name");

var locationLabel = new Label { ... };
locationLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Location");

var image = new Image { ... };
image.SetBinding(Image.SourceProperty, "SelectedMonkey.ImageUrl");

var detailsLabel = new Label();
detailsLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Details");
```

[ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) Wiąże danych właściwości `SelectedMonkey` właściwości modelu połączone widoku, który jest typu `Monkey`. W związku z tym, gdy użytkownik wybierze element [ `Picker` ](xref:Xamarin.Forms.Picker), `SelectedMonkey` właściwość zostanie ustawiona do wybranych `Monkey` obiektu. `SelectedMonkey` Dane obiektu są wyświetlane w interfejsie użytkownika przez [ `Label` ](xref:Xamarin.Forms.Label) i [ `Image` ](xref:Xamarin.Forms.Image) widoki:

![](populating-itemssource-images/monkeys.png "Wybór elementu selektora")

> [!NOTE]
> Należy pamiętać, że [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) i [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) obie właściwości domyślnie obsługują powiązania dwukierunkowego.

## <a name="summary"></a>Podsumowanie

[ `Picker` ](xref:Xamarin.Forms.Picker) Widok jest kontrolka służąca do wybierania elementu tekstowego z listą danych. W tym artykule wyjaśniono, jak wypełnić `Picker` z danymi, ustawiając [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) właściwość i odpowiadania na wybór elementu przez użytkownika. To podejście, który został wprowadzony w interfejsie Xamarin.Forms 2.3.4, jest zalecane podejście do interakcji z `Picker`.


## <a name="related-links"></a>Linki pokrewne

- [Próbnika demonstracyjna (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Małp aplikacji (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/MonkeyAppPicker/)
- [Selektor możliwej do wiązania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
- [Selektor](xref:Xamarin.Forms.Picker)
