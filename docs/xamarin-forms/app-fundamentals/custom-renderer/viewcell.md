---
title: Dostosowywanie obiektu ViewCell
description: Xamarin.Forms ViewCell jest komórki, które mogą być dodawane do ListView lub TableView, która zawiera widok zdefiniowany dla deweloperów. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla ViewCell, który znajduje się w formancie ListView zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: b1ebe2694ad5fa996b8b679cfb31a203588de05c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38999002"
---
# <a name="customizing-a-viewcell"></a>Dostosowywanie obiektu ViewCell

_Xamarin.Forms ViewCell jest komórki, które mogą być dodawane do ListView lub TableView, która zawiera widok zdefiniowany dla deweloperów. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania dla ViewCell, który znajduje się w formancie ListView zestawu narzędzi Xamarin.Forms. Spowoduje to zatrzymanie obliczeń układ Xamarin.Forms miałyby wielokrotnie wywoływane podczas przewijania ListView._

Każdej komórki zestawu narzędzi Xamarin.Forms ma towarzyszący modułu renderowania dla każdej platformy, która tworzy wystąpienie kontrolki natywne. Gdy [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) jest renderowany przez aplikację platformy Xamarin.Forms w systemie iOS `ViewCellRenderer` tworzenia wystąpienia klasy, która z kolei tworzy macierzystej `UITableViewCell` kontroli. Na platformie Android `ViewCellRenderer` klasy tworzy macierzystej `View` kontroli. Na Universal Windows Platform (platformy UWP), `ViewCellRenderer` klasy tworzy macierzystej `DataTemplate`. Aby uzyskać więcej informacji na temat renderowania i klasy natywnych kontrolek, mapowane kontrolek zestawu narzędzi Xamarin.Forms, zobacz [natywne kontrolki i klasy podstawowej renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono relację między [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) odpowiedniego natywne kontrolki, które ją implementują i:

![](viewcell-images/viewcell-classes.png "Relacja między formantem ViewCell i implementowanie natywne kontrolki")

Proces renderowania może podjąć zalet do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) na każdej platformie. Proces ten jest w następujący sposób:

1. [Utwórz](#Creating_the_Custom_Cell) komórki niestandardowego zestawu narzędzi Xamarin.Forms.
1. [Używanie](#Consuming_the_Custom_Cell) niestandardowe komórki z zestawu narzędzi Xamarin.Forms.
1. [Utwórz](#Creating_the_Custom_Renderer_on_each_Platform) niestandardowego modułu renderowania dla komórki na każdej platformie.

Każdy element zostaną teraz dokładniej omówione w implementacji `NativeCell` modułu renderowania, wykorzystującego układ specyficzne dla platformy, dla każdej komórki hostowaną wewnątrz zestawu narzędzi Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) kontroli. Spowoduje to zatrzymanie obliczenia układ interfejsu Xamarin.Forms z wielokrotnie wywoływana podczas `ListView` przewijania.

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>Tworzenie niestandardowych komórki

Kontrolkę komórki niestandardowe mogą być tworzone przez podklasy [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) klasy, jak pokazano w poniższym przykładzie kodu:

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
`NativeCell` Klasa jest tworzony w projekcie biblioteki .NET Standard i definiuje interfejs API dla niestandardowych komórki. Przedstawia komórkę niestandardowe `Name`, `Category`, i `ImageFilename` właściwości, które mogą być wyświetlane za pomocą powiązania danych. Aby uzyskać więcej informacji na temat tworzenia powiązań danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>Korzystanie z niestandardowych komórki

`NativeCell` Komórki niestandardowego może być przywoływany w Xaml w projekcie biblioteki .NET Standard deklarowanie przestrzeni nazw dla lokalizacji i używając prefiksu przestrzeni nazw w elemencie niestandardowej komórki. Poniższy kod przedstawia przykładowy sposób, w jaki `NativeCell` komórki niestandardowe mogą być wykorzystane przez strony XAML:

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

`local` Prefiks przestrzeni nazw może mieć nazwę niczego. Jednak `clr-namespace` i `assembly` wartości muszą być zgodne szczegóły kontrolki niestandardowej. Po zadeklarowaniu przestrzeń nazw prefiks jest używany do niestandardowych komórki odwołania.

Poniższy kod przedstawia przykładowy sposób, w jaki `NativeCell` komórki niestandardowe mogą być wykorzystane przez strony C#:

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

Rozwiązanie Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) formantu służy do wyświetlania listy danych, który jest wypełniana przy użyciu [ `ItemSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) właściwości. [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) Strategii buforowania próbuje zminimalizować `ListView` zużycia pamięci i wykonania przyspieszyć przez odtwarzanie komórek listy. Aby uzyskać więcej informacji, zobacz [strategii buforowania](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

Każdy wiersz na liście zawiera trzy elementy danych — nazwy, kategorii i nazwa pliku obrazu. Układ każdy wiersz na liście jest definiowany przez `DataTemplate` które jest przywoływane za pośrednictwem [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) właściwości możliwej do wiązania. `DataTemplate` Definiuje się, że każdy wiersz danych na liście będzie `NativeCell` wyświetlającą jego `Name`, `Category`, i `ImageFilename` właściwości za pomocą powiązania danych. Aby uzyskać więcej informacji na temat `ListView` sterowania, zobacz [ListView](~/xamarin-forms/user-interface/listview/index.md).

Teraz można dodać niestandardowego modułu renderowania do każdego projektu aplikacji, aby dostosować układ specyficzne dla platformy, dla każdej komórki.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Tworzenie niestandardowego modułu renderowania na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania jest następująca:

1. Utwórz podklasę `ViewCellRenderer` klasę, która renderuje niestandardowe komórkę.
1. Zastąpienie metody specyficzne dla platformy, która renderuje niestandardowe komórkę i zapisanie logiki zgodnie z własnymi.
1. Dodaj `ExportRenderer` atrybutów do klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania komórki niestandardowego zestawu narzędzi Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania przy użyciu zestawu narzędzi Xamarin.Forms.

> [!NOTE]
> W przypadku większości elementów zestawu narzędzi Xamarin.Forms jest opcjonalny w celu zapewnienia niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej kontrolki będą używane. Jednakże, niestandardowe programy renderujące są wymagane w każdym projekcie platformy podczas renderowania [ViewCell](xref:Xamarin.Forms.ViewCell) elementu.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](viewcell-images/solution-structure.png "NativeCell niestandardowego modułu renderowania projektu obowiązki")

`NativeCell` Niestandardowe komórki jest renderowany przez klasy renderowania specyficzne dla platformy, które wynikają z `ViewCellRenderer` klasy dla każdej platformy. Skutkuje to każda `NativeCell` niestandardowe komórki są renderowane przy użyciu układu specyficzne dla platformy, jak pokazano na poniższych zrzutach ekranu:

![](viewcell-images/screenshots.png "NativeCell na każdej platformie")

`ViewCellRenderer` Klasa udostępnia metody specyficzne dla platformy do renderowania niestandardowe komórki. Jest to `GetCell` metody na platformy iOS `GetCellCore` metoda platformy Android i `GetTemplate` metody dla platformy uniwersalnej systemu Windows.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje modułu renderowania za pomocą zestawu narzędzi Xamarin.Forms. Ten atrybut przyjmuje dwa parametry — Nazwa typu komórki zestawu narzędzi Xamarin.Forms renderowanego i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono wykonania każdej klasy specyficzne dla platformy niestandardowego modułu renderowania.

### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie niestandardowego modułu renderowania w systemie iOS

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

`GetCell` Metoda jest wywoływana w celu zbudowania każdej komórki, które mają być wyświetlane. Każda komórka jest `NativeiOSCell` wystąpienia, która definiuje układ komórki i jego danych. Działanie `GetCell` metoda zależy od [ `ListView` ](xref:Xamarin.Forms.ListView) strategii buforowania:

- Gdy [ `ListView` ](xref:Xamarin.Forms.ListView) buforowanie strategii jest [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement), `GetCell` metoda będzie wywoływana dla każdej komórki. A `NativeiOSCell` zostanie utworzone wystąpienie dla każdego `NativeCell` wystąpienia, które początkowo jest wyświetlany na ekranie. Jak użytkownik przewija `ListView`, `NativeiOSCell` wystąpieniach będą używane ponownie. Aby uzyskać więcej informacji na temat iOS komórki ponownego użycia. zobacz [ponownie użyć komórki](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

  > [!NOTE]
  > Ten kod niestandardowego modułu renderowania będzie wykonywać niektóre komórki ponownego użycia nawet wtedy, gdy [ `ListView` ](xref:Xamarin.Forms.ListView) zachowuje komórek.

  Dane wyświetlane przez każdą `NativeiOSCell` wystąpienia, czy nowo utworzone lub ponownego użycia, zostaną zaktualizowane przy użyciu danych z każdej `NativeCell` wystąpienia przez `UpdateCell` metody.

  > [!NOTE]
  > `OnNativeCellPropertyChanged` Metoda nigdy nie będzie wywoływany, gdy [ `ListView` ](xref:Xamarin.Forms.ListView) strategii buforowania jest ustawiony do przechowania komórek.

- Gdy [ `ListView` ](xref:Xamarin.Forms.ListView) buforowanie strategii jest [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), `GetCell` będzie można wywołać metody dla każdej komórki, które początkowo jest wyświetlany na ekranie. A `NativeiOSCell` zostanie utworzone wystąpienie dla każdego `NativeCell` wystąpienia, które początkowo jest wyświetlany na ekranie. Dane wyświetlane przez każdą `NativeiOSCell` wystąpienia zostaną zaktualizowane przy użyciu danych z `NativeCell` wystąpienia przez `UpdateCell` metody. Jednak `GetCell` nie można wywołać metody jako użytkownik przewija `ListView`. Zamiast tego `NativeiOSCell` wystąpieniach będą używane ponownie. `PropertyChanged` zdarzenia zostaną podniesione o `NativeCell` wystąpienie, gdy zmienia swoje dane, a `OnNativeCellPropertyChanged` programu obsługi zdarzeń spowoduje zaktualizowanie danych w każdym ponownym użyciem `NativeiOSCell` wystąpienia.

Poniższy kod przedstawia przykład `OnNativeCellPropertyChanged` metodę, która zostało wywołane, gdy `PropertyChanged` zdarzenie jest zgłaszane:

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

Te metody aktualizacje danych będą wyświetlane przez ponownie używane `NativeiOSCell` wystąpień. Dla właściwości, które uległy zmianie dokonuje, ponieważ metoda może być wywoływana wiele razy.

`NativeiOSCell` Klasa definiuje układ każdej komórki i przedstawiono w poniższym przykładzie kodu:

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

Ta klasa definiuje kontrolki, używany do renderowania zawartości komórki i ich rozmieszczenie. Klasa implementuje [ `INativeElementView` ](xref:Xamarin.Forms.INativeElementView) interfejs, który jest wymagany, gdy [ `ListView` ](xref:Xamarin.Forms.ListView) używa [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) strategii buforowania. Ten interfejs określa, że klasa musi implementować [ `Element` ](xref:Xamarin.Forms.INativeElementView.Element) właściwość, która powinna zwrócić do niestandardowych danych dla odtwarzania komórek.

`NativeiOSCell` Konstruktor inicjuje wygląd `HeadingLabel`, `SubheadingLabel`, i `CellImageView` właściwości. Te właściwości są używane do wyświetlania danych przechowywanych w `NativeCell` wystąpienia, z `UpdateCell` wywołania metody można ustawić wartość każdej właściwości. Ponadto, jeśli [ `ListView` ](xref:Xamarin.Forms.ListView) używa [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) buforowania strategii, dane wyświetlane przez `HeadingLabel`, `SubheadingLabel`, i `CellImageView` właściwości mogą być zaktualizowane przez `OnNativeCellPropertyChanged` metody w niestandardowego modułu renderowania.

Układ komórek odbywa się przez `LayoutSubviews` zastąpić, który ustawia współrzędne `HeadingLabel`, `SubheadingLabel`, i `CellImageView` w komórce.

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

`GetCellCore` Metoda jest wywoływana w celu zbudowania każdej komórki, które mają być wyświetlane. Każda komórka jest `NativeAndroidCell` wystąpienia, która definiuje układ komórki i jego danych. Działanie `GetCellCore` metoda zależy od [ `ListView` ](xref:Xamarin.Forms.ListView) strategii buforowania:

- Gdy [ `ListView` ](xref:Xamarin.Forms.ListView) buforowanie strategii jest [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement), `GetCellCore` metoda będzie wywoływana dla każdej komórki. A `NativeAndroidCell` zostaną utworzone dla każdego `NativeCell` wystąpienia, które początkowo jest wyświetlany na ekranie. Jak użytkownik przewija `ListView`, `NativeAndroidCell` wystąpieniach będą używane ponownie. Aby uzyskać więcej informacji na temat Android komórki ponownego użycia, zobacz [wiersz widoku ponownego użycia](~/android/user-interface/layouts/list-view/populating.md).

  > [!NOTE]
  > Należy pamiętać, że ten kod niestandardowego modułu renderowania będzie wykonywać niektóre komórki ponownego użycia nawet wtedy, gdy [ `ListView` ](xref:Xamarin.Forms.ListView) zachowuje komórek.

  Dane wyświetlane przez każdą `NativeAndroidCell` wystąpienia, czy nowo utworzone lub ponownego użycia, zostaną zaktualizowane przy użyciu danych z każdej `NativeCell` wystąpienia przez `UpdateCell` metody.

  > [!NOTE]
  > Należy pamiętać, że podczas `OnNativeCellPropertyChanged` metoda będzie wywołana, gdy [ `ListView` ](xref:Xamarin.Forms.ListView) jest ustawiony na zachowania komórek, nie zostaną zaktualizowane `NativeAndroidCell` wartości właściwości.

- Gdy [ `ListView` ](xref:Xamarin.Forms.ListView) buforowanie strategii jest [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), `GetCellCore` będzie można wywołać metody dla każdej komórki, które początkowo jest wyświetlany na ekranie. A `NativeAndroidCell` zostanie utworzone wystąpienie dla każdego `NativeCell` wystąpienia, które początkowo jest wyświetlany na ekranie. Dane wyświetlane przez każdą `NativeAndroidCell` wystąpienia zostaną zaktualizowane przy użyciu danych z `NativeCell` wystąpienia przez `UpdateCell` metody. Jednak `GetCellCore` nie można wywołać metody jako użytkownik przewija `ListView`. Zamiast tego `NativeAndroidCell` wystąpieniach będą używane ponownie.  `PropertyChanged` zdarzenia zostaną podniesione o `NativeCell` wystąpienie, gdy zmienia swoje dane, a `OnNativeCellPropertyChanged` programu obsługi zdarzeń spowoduje zaktualizowanie danych w każdym ponownym użyciem `NativeAndroidCell` wystąpienia.

Poniższy kod przedstawia przykład `OnNativeCellPropertyChanged` metodę, która zostało wywołane, gdy `PropertyChanged` zdarzenie jest zgłaszane:

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

Te metody aktualizacje danych będą wyświetlane przez ponownie używane `NativeAndroidCell` wystąpień. Dla właściwości, które uległy zmianie dokonuje, ponieważ metoda może być wywoływana wiele razy.

`NativeAndroidCell` Klasa definiuje układ każdej komórki i przedstawiono w poniższym przykładzie kodu:

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

Ta klasa definiuje kontrolki, używany do renderowania zawartości komórki i ich rozmieszczenie. Klasa implementuje [ `INativeElementView` ](xref:Xamarin.Forms.INativeElementView) interfejs, który jest wymagany, gdy [ `ListView` ](xref:Xamarin.Forms.ListView) używa [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) strategii buforowania. Ten interfejs określa, że klasa musi implementować [ `Element` ](xref:Xamarin.Forms.INativeElementView.Element) właściwość, która powinna zwrócić do niestandardowych danych dla odtwarzania komórek.

`NativeAndroidCell` Zwiększa Konstruktor `NativeAndroidCell` układ i inicjuje `HeadingTextView`, `SubheadingTextView`, i `ImageView` właściwości do kontrolki z nadmuchany układu. Te właściwości są używane do wyświetlania danych przechowywanych w `NativeCell` wystąpienia, z `UpdateCell` wywołania metody można ustawić wartość każdej właściwości. Ponadto, jeśli [ `ListView` ](xref:Xamarin.Forms.ListView) używa [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) buforowania strategii, dane wyświetlane przez `HeadingTextView`, `SubheadingTextView`, i `ImageView` właściwości mogą być zaktualizowane przez `OnNativeCellPropertyChanged` metody w niestandardowego modułu renderowania.

Poniższy przykład kodu pokazuje definicji układu `NativeAndroidCell.axml` plik układu:

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

Ten układ Określa, że dwa `TextView` kontrolek i `ImageView` kontrolki są używane do wyświetlenia zawartości komórki. Dwa `TextView` formanty są w orientacji pionowej w ramach `LinearLayout` kontroli przy użyciu wszystkich formantów, które są zawarte w `RelativeLayout`.

### <a name="creating-the-custom-renderer-on-uwp"></a>Tworzenie niestandardowego modułu renderowania na platformy uniwersalnej systemu Windows

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy uniwersalnej systemu Windows:

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeUWPCellRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPCellRenderer : ViewCellRenderer
    {
        public override Windows.UI.Xaml.DataTemplate GetTemplate(Cell cell)
        {
            return App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
        }
    }
}
```

`GetTemplate` Metoda jest wywoływana w celu zwrócenia komórki do renderowania dla każdego wiersza danych na liście. Tworzy `DataTemplate` dla każdego `NativeCell` wystąpienia, która będzie wyświetlana na ekranie za pomocą `DataTemplate` Definiowanie wygląd i zawartość komórki.

`DataTemplate` Znajduje się w słowniku zasobów na poziomie aplikacji i przedstawiono w poniższym przykładzie kodu:

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

`DataTemplate` Określa kontrolek używanych w celu wyświetlenia zawartości komórki, a ich układ i wygląd. Dwa `TextBlock` kontrolek i `Image` kontrolki są używane do wyświetlenia zawartości komórki za pomocą powiązania danych. Ponadto, wystąpienie `ConcatImageExtensionConverter` służy do łączenia `.jpg` rozszerzenie nazwy pliku każdego obrazu. Gwarantuje to, że `Image` kontroli można załadować i renderować obraz, gdy jest ona `Source` właściwość jest ustawiona.

## <a name="summary"></a>Podsumowanie

Ten artykuł ma pokazano sposób tworzenia niestandardowego modułu renderowania dla [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) hostowaną wewnątrz zestawu narzędzi Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) kontroli. Spowoduje to zatrzymanie obliczenia układ interfejsu Xamarin.Forms z wielokrotnie wywoływana podczas `ListView` przewijania.


## <a name="related-links"></a>Linki pokrewne

- [Wydajność ListView](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
