---
title: Dostosowywanie ViewCell
description: "ViewCell platformy Xamarin.Forms jest komórki, można dodać do elementu ListView lub TableView, zawierający widok zdefiniowany przez dewelopera. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla ViewCell, który znajduje się w formancie ListView platformy Xamarin.Forms. Zatrzymuje obliczeń układ platformy Xamarin.Forms jest wielokrotnie wywoływane podczas przewijania ListView."
ms.topic: article
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 2c65bce7ae468ef07c6d898e3f532aa95580f2ba
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="customizing-a-viewcell"></a>Dostosowywanie ViewCell

_ViewCell platformy Xamarin.Forms jest komórki, można dodać do elementu ListView lub TableView, zawierający widok zdefiniowany przez dewelopera. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla ViewCell, który znajduje się w formancie ListView platformy Xamarin.Forms. Zatrzymuje obliczeń układ platformy Xamarin.Forms jest wielokrotnie wywoływane podczas przewijania ListView._

Każdej komórki platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu. Gdy [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) jest renderowany przez aplikację platformy Xamarin.Forms w systemie iOS `ViewCellRenderer` tworzenia wystąpienia klasy, która z kolei tworzy natywny `UITableViewCell` formantu. Na platformie Android `ViewCellRenderer` natywny tworzy wystąpienie klasy `View` formantu. Windows Phone i Windows platformy Uniwersalnej `ViewCellRenderer` natywny tworzy wystąpienie klasy `DataTemplate`. Aby uzyskać więcej informacji na temat klasy macierzystego formantu, mapowane na formanty platformy Xamarin.Forms i renderowania, zobacz [renderowania klasy podstawowej i kontrolki natywne](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono związek między [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) i odpowiednie natywnego formantów, które implementuje ona:

![](viewcell-images/viewcell-classes.png "Relacja między kontroli ViewCell i wykonawczych kontrolki natywne")

Proces renderowania można podjąć zaletą do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) na każdej z platform. Proces ten wygląda następująco:

1. [Utwórz](#Creating_the_Custom_Cell) komórki niestandardowych platformy Xamarin.Forms.
1. [Korzystać z](#Consuming_the_Custom_Cell) niestandardowych komórki z platformy Xamarin.Forms.
1. [Utwórz](#Creating_the_Custom_Renderer_on_each_Platform) niestandardowego modułu renderowania dla komórki na każdej z platform.

Każdy element teraz omówione zostaną z kolei, aby zaimplementować `NativeCell` renderowania wykorzystującego układu specyficzne dla platformy, dla każdej komórki hostowaną wewnątrz platformy Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) formantu. Powoduje to zatrzymanie obliczenia układ platformy Xamarin.Forms z wielokrotnie wywoływane podczas `ListView` przewijania.

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>Tworzenie niestandardowych komórki

Kontrolki niestandardowe komórki mogą być tworzone przez podklasy [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) klasy, jak pokazano w poniższym przykładzie:

```csharp
public class NativeCell : ViewCell
{
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(NativeCell), "");

  public string Name {
    get { return (string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }

  public static readonly BindableProperty CategoryProperty =
    BindableProperty.Create ("Category", typeof(string), typeof(NativeCell), "");

  public string Category {
    get { return (string)GetValue (CategoryProperty); }
    set { SetValue (CategoryProperty, value); }
  }

  public static readonly BindableProperty ImageFilenameProperty =
    BindableProperty.Create ("ImageFilename", typeof(string), typeof(NativeCell), "");

  public string ImageFilename {
    get { return (string)GetValue (ImageFilenameProperty); }
    set { SetValue (ImageFilenameProperty, value); }
  }
}
```
`NativeCell` Klasy jest tworzony w projekcie (PCL) biblioteki klas przenośnych i definiuje interfejsu API dla niestandardowych komórki. Przedstawia komórki niestandardowych `Name`, `Category`, i `ImageFilename` właściwości, które mogą być wyświetlane przez powiązanie danych. Aby uzyskać więcej informacji na temat wiązania danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>Korzystanie z niestandardowych komórki

`NativeCell` Komórki niestandardowy może być przywoływany w Xaml w projekcie PCL deklarowanie przestrzeni nazw dla lokalizacji, a następnie użyć prefiksu przestrzeni nazw w elemencie niestandardowej komórki. Poniższy kod przedstawia przykład sposobu `NativeCell` komórki niestandardowe mogą być używane przez strony XAML:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Xamarin.Forms native cell" HorizontalTextAlignment="Center" />
            <ListView x:Name="listView" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <local:NativeCell Name="{Binding Name}" Category="{Binding Category}" ImageFilename="{Binding ImageFilename}" />
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`local` Prefiks przestrzeni nazw może mieć nazwę żadnych czynności. Jednak `clr-namespace` i `assembly` wartości muszą być zgodne szczegóły kontrolki niestandardowej. Po zadeklarowaniu obszaru nazw prefiks jest używany do odwołać się do niestandardowego komórki.

Poniższy kod przedstawia przykład sposobu `NativeCell` komórki niestandardowe mogą być używane przez stronę C#:

```csharp
public class NativeCellPageCS : ContentPage
{
    ListView listView;

    public NativeCellPageCS()
    {
        listView = new ListView(ListViewCachingStrategy.RecycleElement)
        {
            ItemsSource = DataSource.GetList(),
            ItemTemplate = new DataTemplate(() =>
            {
                var nativeCell = new NativeCell();
                nativeCell.SetBinding(NativeCell.NameProperty, "Name");
                nativeCell.SetBinding(NativeCell.CategoryProperty, "Category");
                nativeCell.SetBinding(NativeCell.ImageFilenameProperty, "ImageFilename");

                return nativeCell;
            })
        };

        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
                Padding = new Thickness(0, 20, 0, 0);
                break;
            case Device.Android:
            case Device.UWP:
                Padding = new Thickness(0);
                break;
        }

        Content = new StackLayout
        {
            Children = {
                new Label { Text = "Xamarin.Forms native cell", HorizontalTextAlignment = TextAlignment.Center },
                listView
            }
        };
        listView.ItemSelected += OnItemSelected;
    }
    ...
}
```

Platformy Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) formantu służy do wyświetlania listy danych, który znajduje się za pośrednictwem [ `ItemSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemsSource/) właściwości. [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) Buforowanie strategii próbuje zminimalizować `ListView` zużycie pamięci i wykonywania szybkości w komórkach listy odtwarzania. Aby uzyskać więcej informacji, zobacz [strategii buforowania](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

Każdy wiersz na liście zawiera trzy elementy danych — nazwę, kategorię i nazwę pliku obrazu. Układ każdy wiersz na liście jest definiowana za pomocą `DataTemplate` który odwołuje się do [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemTemplate/) właściwości możliwej do wiązania. `DataTemplate` Określa, że każdy wiersz danych na liście będzie `NativeCell` który wyświetla jego `Name`, `Category`, i `ImageFilename` właściwości za pośrednictwem powiązania danych. Aby uzyskać więcej informacji na temat `ListView` sterowania, zobacz [ListView](~/xamarin-forms/user-interface/listview/index.md).

Teraz można dodać niestandardowego modułu renderowania do każdego projektu aplikacji w celu dostosowania układu specyficzne dla platformy, dla każdej komórki.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Tworzenie niestandardowego modułu renderowania na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania wygląda następująco:

1. Utwórz podklasę `ViewCellRenderer` klasy, która renderuje niestandardowych komórki.
1. Zastąp metodę specyficzne dla platformy renderujący komórki niestandardowych i zapisu logiki, aby dostosować go.
1. Dodaj `ExportRenderer` atrybutu klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania komórki niestandardowych platformy Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania z platformy Xamarin.Forms.

> [!NOTE]
> **Uwaga**: w przypadku większości elementów platformy Xamarin.Forms jest opcjonalne zapewnić niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej formantu będzie używany. Jednak niestandardowe moduły renderowania są wymagane w każdym projekcie platformy podczas renderowania [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) elementu.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](viewcell-images/solution-structure.png "Obowiązki NativeCell niestandardowe renderowania projektu:")

`NativeCell` Niestandardowych komórki jest renderowany przez klasy renderowania specyficzne dla platformy, które wynikają z `ViewCellRenderer` klasy dla każdej platformy. Powoduje to w każdym `NativeCell` niestandardowych komórki renderowanego z układem specyficzne dla platformy, jak pokazano na poniższych zrzutach ekranu:

![](viewcell-images/screenshots.png "NativeCell na każdej platformie")

`ViewCellRenderer` Klasa opisuje metody specyficzne dla platformy renderowanie niestandardowych komórki. Jest to `GetCell` metody na platformie iOS `GetCellCore` metody na platformie Android i `GetTemplate` metody na platformie Windows Phone.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje mechanizm renderujący platformy Xamarin.Forms. Atrybut przyjmuje dwa parametry — Nazwa typu komórki platformy Xamarin.Forms renderowanego i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono implementacji każdej klasy niestandardowego modułu renderowania specyficzne dla platformy.

### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie modułu renderowania niestandardowe w systemie iOS

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy iOS:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeiOSCellRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        NativeiOSCell cell;

        public override UITableViewCell GetCell(Cell item, UITableViewCell reusableCell, UITableView tv)
        {
            var nativeCell = (NativeCell)item;

            cell = reusableCell as NativeiOSCell;
            if (cell == null)
                cell = new NativeiOSCell(item.GetType().FullName, nativeCell);
            else
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;
            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

`GetCell` Metoda jest wywoływana w celu zbudowania każdej komórki mają być wyświetlane. Każda komórka jest `NativeiOSCell` wystąpienia, która definiuje układ komórki, a jego dane. Działanie `GetCell` metody jest zależny od [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) strategii buforowania:

- Gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) buforowanie strategii jest [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/), `GetCell` będzie można wywołać metody dla każdej komórki. A `NativeiOSCell` będzie można utworzyć wystąpienia dla każdego `NativeCell` wystąpienia, które jest początkowo wyświetlane na ekranie. Jako użytkownik przewija `ListView`, `NativeiOSCell` wystąpień będą ponownie używane. Aby uzyskać więcej informacji na temat iOS komórki ponownego użycia, zobacz [komórki ponowne użycie](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

  > [!NOTE]
  > Ten kod niestandardowego modułu renderowania będzie wykonywać pewne komórki ponownego użycia nawet wtedy, gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ustawiono opcję zachowania komórek.

  Dane wyświetlane przez każdą `NativeiOSCell` wystąpienia, czy nowo utworzony lub używana ponownie, zostaną zaktualizowane przy użyciu danych z każdego `NativeCell` wystąpienia przez `UpdateCell` metody.

  > [!NOTE]
  > `OnNativeCellPropertyChanged` Metoda nigdy nie będzie wywoływany, gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) buforowanie strategii ustawiono zachować komórek.

- Gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) buforowanie strategii jest [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/), `GetCell` będzie można wywołać metody dla każdej komórki wyświetlany na ekranie. A `NativeiOSCell` będzie można utworzyć wystąpienia dla każdego `NativeCell` wystąpienia, które jest początkowo wyświetlane na ekranie. Dane wyświetlane przez każdą `NativeiOSCell` wystąpienie zostanie zaktualizowany przy użyciu danych z `NativeCell` wystąpienia przez `UpdateCell` metody. Jednak `GetCell` metody nie można wywołać, gdy użytkownik przewija za pośrednictwem `ListView`. Zamiast tego `NativeiOSCell` wystąpień będą ponownie używane. `PropertyChanged` zdarzenia, które będą wywoływane w `NativeCell` wystąpienie zmianie jego danych i `OnNativeCellPropertyChanged` obsługi zdarzeń spowoduje zaktualizowanie danych w każdym ponownym użyciem `NativeiOSCell` wystąpienia.

Poniższy kod przedstawia przykład `OnNativeCellPropertyChanged` metodę, która została wywołana podczas `PropertyChanged` zdarzenia:

```csharp
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingLabel.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingLabel.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.CellImageView.Image = cell.GetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Ponownie użyć tej metody aktualizacji danych wyświetlany przez `NativeiOSCell` wystąpień. Dla właściwości, które uległy zmianie dokonuje, jak metoda może być wywołana wiele razy.

`NativeiOSCell` Klasy definiuje układu dla każdej komórki i przedstawiono w poniższym przykładzie:

```csharp
internal class NativeiOSCell : UITableViewCell, INativeElementView
{
  public UILabel HeadingLabel { get; set; }
  public UILabel SubheadingLabel { get; set; }
  public UIImageView CellImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeiOSCell(string cellId, NativeCell cell) : base(UITableViewCellStyle.Default, cellId)
  {
    NativeCell = cell;

    SelectionStyle = UITableViewCellSelectionStyle.Gray;
    ContentView.BackgroundColor = UIColor.FromRGB(255, 255, 224);
    CellImageView = new UIImageView();

    HeadingLabel = new UILabel()
    {
      Font = UIFont.FromName("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB(127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    SubheadingLabel = new UILabel()
    {
      Font = UIFont.FromName("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB(38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add(HeadingLabel);
    ContentView.Add(SubheadingLabel);
    ContentView.Add(CellImageView);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingLabel.Text = cell.Name;
    SubheadingLabel.Text = cell.Category;
    CellImageView.Image = GetImage(cell.ImageFilename);
  }

  public UIImage GetImage(string filename)
  {
    return (!string.IsNullOrWhiteSpace(filename)) ? UIImage.FromFile("Images/" + filename + ".jpg") : null;
  }

  public override void LayoutSubviews()
  {
    base.LayoutSubviews();

    HeadingLabel.Frame = new CGRect(5, 4, ContentView.Bounds.Width - 63, 25);
    SubheadingLabel.Frame = new CGRect(100, 18, 100, 20);
    CellImageView.Frame = new CGRect(ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Ta klasa definiuje formanty używany do renderowania zawartości komórki przez użytkownika i ich układ. Implementuje klasy [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) interfejs, który jest wymagany, gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) używa [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) strategii buforowania. Ten interfejs określa, że klasa musi implementować [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) właściwość, która powinna zwrócić dane niestandardowe komórki odtwarzania komórek.

`NativeiOSCell` Konstruktor inicjuje wygląd `HeadingLabel`, `SubheadingLabel`, i `CellImageView` właściwości. Te właściwości są używane do wyświetlania danych przechowywanych w `NativeCell` wystąpienia, z `UpdateCell` metoda jest wywoływana, aby ustawić wartość każdej właściwości. Ponadto, jeśli [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) używa [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) buforowanie strategii, dane wyświetlane przez `HeadingLabel`, `SubheadingLabel`, i `CellImageView` właściwości mogą być zaktualizowany przez `OnNativeCellPropertyChanged` metoda niestandardowego modułu renderowania.

Układ komórki jest wykonywane przez `LayoutSubviews` zastąpić, który ustawia współrzędne `HeadingLabel`, `SubheadingLabel`, i `CellImageView` wewnątrz komórki przy realizacji.

### <a name="creating-the-custom-renderer-on-android"></a>Tworzenie niestandardowego modułu renderowania w systemie Android

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy systemu Android:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeAndroidCellRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        NativeAndroidCell cell;

        protected override Android.Views.View GetCellCore(Cell item, Android.Views.View convertView, ViewGroup parent, Context context)
        {
            var nativeCell = (NativeCell)item;
            Console.WriteLine("\t\t" + nativeCell.Name);

            cell = convertView as NativeAndroidCell;
            if (cell == null)
            {
                cell = new NativeAndroidCell(context, nativeCell);
            }
            else
            {
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;
            }

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;

            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

`GetCellCore` Metoda jest wywoływana w celu zbudowania każdej komórki mają być wyświetlane. Każda komórka jest `NativeAndroidCell` wystąpienia, która definiuje układ komórki, a jego dane. Działanie `GetCellCore` metody jest zależny od [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) strategii buforowania:

- Gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) buforowanie strategii jest [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/), `GetCellCore` będzie można wywołać metody dla każdej komórki. A `NativeAndroidCell` zostaną utworzone dla każdego `NativeCell` wystąpienia, które jest początkowo wyświetlane na ekranie. Jako użytkownik przewija `ListView`, `NativeAndroidCell` wystąpień będą ponownie używane. Aby uzyskać więcej informacji na temat Android komórki ponownego użycia, zobacz [wiersza widoku ponownego użycia](~/android/user-interface/layouts/list-view/populating.md).

  > [!NOTE]
  > Należy pamiętać, że ten kod niestandardowego modułu renderowania będzie wykonywać niektórych komórki ponownego użycia nawet wtedy, gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ustawiono opcję zachowania komórek.

  Dane wyświetlane przez każdą `NativeAndroidCell` wystąpienia, czy nowo utworzony lub używana ponownie, zostaną zaktualizowane przy użyciu danych z każdego `NativeCell` wystąpienia przez `UpdateCell` metody.

  > [!NOTE]
  > Należy pamiętać, że podczas `OnNativeCellPropertyChanged` metoda jest wywoływana, gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) jest ustawiona, aby zachować komórek, nie zostaną zaktualizowane `NativeAndroidCell` wartości właściwości.

- Gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) buforowanie strategii jest [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/), `GetCellCore` będzie można wywołać metody dla każdej komórki wyświetlany na ekranie. A `NativeAndroidCell` będzie można utworzyć wystąpienia dla każdego `NativeCell` wystąpienia, które jest początkowo wyświetlane na ekranie. Dane wyświetlane przez każdą `NativeAndroidCell` wystąpienie zostanie zaktualizowany przy użyciu danych z `NativeCell` wystąpienia przez `UpdateCell` metody. Jednak `GetCellCore` metody nie można wywołać, gdy użytkownik przewija za pośrednictwem `ListView`. Zamiast tego `NativeAndroidCell` wystąpień będą ponownie używane.  `PropertyChanged` zdarzenia, które będą wywoływane w `NativeCell` wystąpienie zmianie jego danych i `OnNativeCellPropertyChanged` obsługi zdarzeń spowoduje zaktualizowanie danych w każdym ponownym użyciem `NativeAndroidCell` wystąpienia.

Poniższy kod przedstawia przykład `OnNativeCellPropertyChanged` metodę, która została wywołana podczas `PropertyChanged` zdarzenia:

```csharp
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingTextView.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingTextView.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.SetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Ponownie użyć tej metody aktualizacji danych wyświetlany przez `NativeAndroidCell` wystąpień. Dla właściwości, które uległy zmianie dokonuje, jak metoda może być wywołana wiele razy.

`NativeAndroidCell` Klasy definiuje układu dla każdej komórki i przedstawiono w poniższym przykładzie:

```csharp
internal class NativeAndroidCell : LinearLayout, INativeElementView
{
  public TextView HeadingTextView { get; set; }
  public TextView SubheadingTextView { get; set; }
  public ImageView ImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeAndroidCell(Context context, NativeCell cell) : base(context)
  {
    NativeCell = cell;

    var view = (context as Activity).LayoutInflater.Inflate(Resource.Layout.NativeAndroidCell, null);
    HeadingTextView = view.FindViewById<TextView>(Resource.Id.HeadingText);
    SubheadingTextView = view.FindViewById<TextView>(Resource.Id.SubheadingText);
    ImageView = view.FindViewById<ImageView>(Resource.Id.Image);

    AddView(view);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingTextView.Text = cell.Name;
    SubheadingTextView.Text = cell.Category;

    // Dispose of the old image
    if (ImageView.Drawable != null)
    {
      using (var image = ImageView.Drawable as BitmapDrawable)
      {
        if (image != null)
        {
          if (image.Bitmap != null)
          {
            image.Bitmap.Dispose();
          }
        }
      }
    }

    SetImage(cell.ImageFilename);
  }

  public void SetImage(string filename)
  {
    if (!string.IsNullOrWhiteSpace(filename))
    {
      // Display new image
      Context.Resources.GetBitmapAsync(filename).ContinueWith((t) =>
      {
        var bitmap = t.Result;
        if (bitmap != null)
        {
          ImageView.SetImageBitmap(bitmap);
          bitmap.Dispose();
        }
      }, TaskScheduler.FromCurrentSynchronizationContext());
    }
    else
    {
      // Clear the image
      ImageView.SetImageBitmap(null);
    }
  }
}
```

Ta klasa definiuje formanty używany do renderowania zawartości komórki przez użytkownika i ich układ. Implementuje klasy [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) interfejs, który jest wymagany, gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) używa [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) strategii buforowania. Ten interfejs określa, że klasa musi implementować [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) właściwość, która powinna zwrócić dane niestandardowe komórki odtwarzania komórek.

`NativeAndroidCell` Nadyma Konstruktor `NativeAndroidCell` układ i inicjuje `HeadingTextView`, `SubheadingTextView`, i `ImageView` właściwości do formantów w nadmuchany układu. Te właściwości są używane do wyświetlania danych przechowywanych w `NativeCell` wystąpienia, z `UpdateCell` metoda jest wywoływana, aby ustawić wartość każdej właściwości. Ponadto, jeśli [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) używa [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) buforowanie strategii, dane wyświetlane przez `HeadingTextView`, `SubheadingTextView`, i `ImageView` właściwości mogą być zaktualizowany przez `OnNativeCellPropertyChanged` metoda niestandardowego modułu renderowania.

Poniższy przykład kodu pokazuje definicji układu `NativeAndroidCell.axml` pliku układu:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="8dp"
    android:background="@drawable/CustomSelector">
    <LinearLayout
        android:id="@+id/Text"
        android:orientation="vertical"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingLeft="10dip">
        <TextView
            android:id="@+id/HeadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/SubheadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="14dip"
            android:textColor="#FF267F00"
            android:paddingLeft="100dip" />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout>
```

Ten układ Określa, że dwa `TextView` formantów i `ImageView` formantu są używane do wyświetlenia zawartości komórki. Dwa `TextView` kontrolki są orientacji pionowej w `LinearLayout` sterowania wszystkie formanty, które są zawarte w `RelativeLayout`.

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Tworzenie niestandardowego modułu renderowania na Windows Phone i platformy uniwersalnej systemu Windows

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla Windows Phone i platformy uniwersalnej systemu Windows:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeWinPhoneCellRenderer))]
namespace CustomRenderer.WinPhone81
{
    public class NativeWinPhoneCellRenderer : ViewCellRenderer
    {
        public override Windows.UI.Xaml.DataTemplate GetTemplate(Cell cell)
        {
            return App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
        }
    }
}
```

`GetTemplate` Wywoływana jest metoda zwraca komórki do renderowania dla każdego wiersza danych na liście. Tworzy `DataTemplate` dla każdego `NativeCell` wystąpienia, który będzie wyświetlany na ekranie z `DataTemplate` Definiowanie wygląd i zawartość komórki.

`DataTemplate` Znajduje się w słowniku zasobów na poziomie aplikacji i przedstawiono w poniższym przykładzie:

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="LightYellow">
        <Grid.Resources>
            <local:ConcatImageExtensionConverter x:Name="ConcatImageExtensionConverter" />
        </Grid.Resources>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.40*" />
            <ColumnDefinition Width="0.40*"/>
            <ColumnDefinition Width="0.20*" />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.ColumnSpan="2" Foreground="#7F3300" FontStyle="Italic" FontSize="22" VerticalAlignment="Top" Text="{Binding Name}" />
        <TextBlock Grid.RowSpan="2" Grid.Column="1" Foreground="#267F00" FontWeight="Bold" FontSize="12" VerticalAlignment="Bottom" Text="{Binding Category}" />
        <Image Grid.RowSpan="2" Grid.Column="2" HorizontalAlignment="Left" VerticalAlignment="Center" Source="{Binding ImageFilename, Converter={StaticResource ConcatImageExtensionConverter}}" Width="50" Height="50" />
        <Line Grid.Row="1" Grid.ColumnSpan="3" X1="0" X2="1" Margin="30,20,0,0" StrokeThickness="1" Stroke="LightGray" Stretch="Fill" VerticalAlignment="Bottom" />
    </Grid>
</DataTemplate>
```

`DataTemplate` Określa formanty używane do wyświetlenia zawartości komórki, a ich układu i wyglądu. Dwa `TextBlock` formantów i `Image` formantu są używane do wyświetlenia zawartości komórki przez powiązanie danych. Ponadto wystąpienia `ConcatImageExtensionConverter` służy do łączenia `.jpg` rozszerzenie nazwy pliku każdego obrazu. Gwarantuje to, że `Image` formantu można załadować i renderować obraz, gdy jest `Source` właściwość jest ustawiona.

## <a name="summary"></a>Podsumowanie

Ten artykuł ma przedstawiono sposób tworzenia niestandardowego modułu renderowania dla [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) który znajduje się wewnątrz platformy Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) formantu. Powoduje to zatrzymanie obliczenia układ platformy Xamarin.Forms z wielokrotnie wywoływane podczas `ListView` przewijania.


## <a name="related-links"></a>Linki pokrewne

- [Wydajność ListView](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
