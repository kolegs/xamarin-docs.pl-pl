---
title: "Ustawienie właściwości ItemsSource selektora"
description: "Widok selektora jest formant wyboru z listy danych elementu tekstowego. W tym artykule opisano sposób wypełnienia selektora z danymi, ustawiając właściwość ItemsSource oraz sposobu reagowania na wybór elementu przez użytkownika."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 5e3d20ad213df9fd9331c71c84003c7738bd5a29
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="setting-a-pickers-itemssource-property"></a>Ustawienie właściwości ItemsSource selektora

_Widok selektora jest formant wyboru z listy danych elementu tekstowego. W tym artykule opisano sposób wypełnienia selektora z danymi, ustawiając właściwość ItemsSource oraz sposobu reagowania na wybór elementu przez użytkownika._

Platformy Xamarin.Forms 2.3.4 został rozszerzony [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) widoku przez dodanie możliwość wypełnić je danymi przez ustawienie jego [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) właściwości oraz do pobierania wybranego elementu z [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) właściwości. Ponadto można zmienić kolor tekstu dla wybranego elementu, ustawiając [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.TextColor/) właściwości [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/).

## <a name="populating-a-picker-with-data"></a>Wypełnianie selektora z danymi

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) można wypełniać za pomocą danych przez ustawienie jej [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) właściwości `IList` kolekcji. Każdego elementu w kolekcji musi być typu lub pochodną, wpisz `object`. Można dodać elementy w języku XAML, inicjowanie `ItemsSource` właściwości z tablicy elementów:

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
> Należy pamiętać, że `x:Array` wymaga elementu `Type` atrybut wskazujący typ elementów w tablicy.

Poniżej przedstawiono równoważne kodu C#:

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

## <a name="responding-to-item-selection"></a>Odpowiada na zaznaczanie elementów

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) obsługuje wybór jednego elementu na raz. Po wybraniu elementu [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) generowane zdarzenie [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) właściwości są aktualizowane na liczbę całkowitą reprezentującą indeks wybranego elementu z listy i [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) właściwości jest aktualizowana w celu `object` reprezentujący wybranego elementu. [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) Właściwość jest liczony od zera liczbę określającą elementu wybrany przez użytkownika. Jeśli nie wybrano elementów, co ma miejsce po [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) najpierw jest tworzony i inicjowany, `SelectedIndex` będzie mieć wartość -1.

> [!NOTE]
> Element zachowanie dotyczące wyboru w [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) można dostosować w systemie iOS z poszczególnych platform. Aby uzyskać więcej informacji, zobacz [kontrolowanie Wybór elementu selektora](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Poniższy przykład kodu pokazuje, jak pobrać [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) wartość właściwości z [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) w języku XAML:

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

Poniżej przedstawiono równoważne kodu C#:

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

Ponadto program obsługi zdarzeń może być wykonana przy [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) generowane zdarzenie:

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

Ta metoda uzyskuje [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) wartość właściwości i korzysta z wartości można pobrać wybrany element z [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) kolekcji. To jest funkcjonalnym odpowiednikiem pobierania wybrany element z [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) właściwości. Należy pamiętać, że każdy element `ItemsSource` typ kolekcji to `object`i dlatego musi być rzutowane na `string` do wyświetlenia.

> [!NOTE]
> A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) mogą być inicjowane można wyświetlić określonego elementu, ustawiając [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) lub [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) właściwości. Jednak należy ustawić te właściwości po inicjowanie [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) kolekcji.

## <a name="populating-a-picker-with-data-using-data-binding"></a>Wypełnianie selektora z danymi przy użyciu powiązanie danych

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) można również wypełniać za pomocą danych przy użyciu wiązania danych można powiązać jej [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) właściwości `IList` kolekcji. W języku XAML jest to osiągane z [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) — rozszerzenie znaczników:

```xaml
<Picker Title="Select a monkey" ItemsSource="{Binding Monkeys}" ItemDisplayBinding="{Binding Name}" />
```

Poniżej przedstawiono równoważne kodu C#:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

[ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) Wiąże danych właściwości `Monkeys` właściwości modelu widoku połączone, która zwraca `IList<Monkey>` kolekcji. Poniższy kod przedstawia przykład `Monkey` klasy, która zawiera cztery właściwości:

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

Podczas wiązania z listy obiektów, [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) należy dowiedzieć się, które właściwości do wyświetlenia z każdego obiektu. Jest to osiągane przez ustawienie [ `ItemDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemDisplayBinding/) właściwości wymaganej właściwości z każdego obiektu. W przykładach kodu `Picker` jest ustawiony na wyświetlanie każdego `Monkey.Name` wartości właściwości.

### <a name="responding-to-item-selection"></a>Odpowiada na zaznaczanie elementów

Powiązanie danych może służyć do ustawioną obiektu [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) wartość właściwości, gdy zmienia:

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

Poniżej przedstawiono równoważne kodu C#:

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

[ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) Wiąże danych właściwości `SelectedMonkey` właściwości modelu połączonych widoku, który jest typu `Monkey`. W związku z tym, gdy użytkownik wybierze element [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), `SelectedMonkey` właściwość zostanie ustawiona do wybranego `Monkey` obiektu. `SelectedMonkey` Dane obiektu są wyświetlane w interfejsie użytkownika przez [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) i [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) widoków:

![](populating-itemssource-images/monkeys.png "Wybór elementu selektora")

> [!NOTE]
> Należy pamiętać, że [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) i [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) właściwości zarówno obsługuje powiązań dwukierunkowych domyślnie.

## <a name="summary"></a>Podsumowanie

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Widok jest formant wyboru z listy danych elementu tekstowego. W tym artykule wyjaśniono sposób wypełnienia `Picker` z danymi przez ustawienie [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) właściwości i odpowiadanie na wybór elementu przez użytkownika. Ta metoda, którą wprowadzono w platformy Xamarin.Forms 2.3.4, jest zalecane podejście do interakcji z `Picker`.


## <a name="related-links"></a>Linki pokrewne

- [Wersja demonstracyjna selektora (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Małp aplikacji (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/MonkeyAppPicker/)
- [Selektor powiązania (przykład)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
- [Selektor](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
