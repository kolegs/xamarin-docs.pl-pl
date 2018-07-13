---
title: Dostosowywanie ListView
description: ListView Xamarin.Forms jest widok, który wyświetla zbiór danych jako pionowy listy. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania hermetyzujący kontrolki listy specyficzne dla platformy i układy natywnych komórki, dzięki czemu większa kontrola nad natywnych listy kontrolowania wydajności.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: b3b73d542faebdb8ab85c989d7812368f4f3ffac
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997494"
---
# <a name="customizing-a-listview"></a>Dostosowywanie ListView

_ListView Xamarin.Forms jest widok, który wyświetla zbiór danych jako pionowy listy. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania hermetyzujący kontrolki listy specyficzne dla platformy i układy natywnych komórki, dzięki czemu większa kontrola nad natywnych listy kontrolowania wydajności._

Każdego widoku interfejsu Xamarin.Forms ma towarzyszący modułu renderowania dla każdej platformy, która tworzy wystąpienie kontrolki natywne. Gdy [ `ListView` ](xref:Xamarin.Forms.ListView) jest renderowany przez aplikację platformy Xamarin.Forms w systemie iOS `ListViewRenderer` tworzenia wystąpienia klasy, która z kolei tworzy macierzystej `UITableView` kontroli. Na platformie Android `ListViewRenderer` klasy tworzy macierzystej `ListView` kontroli. Na Universal Windows Platform (platformy UWP), `ListViewRenderer` klasy tworzy macierzystej `ListView` kontroli. Aby uzyskać więcej informacji na temat renderowania i klasy natywnych kontrolek, mapowane kontrolek zestawu narzędzi Xamarin.Forms, zobacz [natywne kontrolki i klasy podstawowej renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono relację między [ `ListView` ](xref:Xamarin.Forms.ListView) kontroli i odpowiednie kontrolki natywne, które ją implementują:

![](listview-images/listview-classes.png "Relacja między formantem ListView i implementowanie natywne kontrolki")

Proces renderowania może podjąć zalet do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `ListView` ](xref:Xamarin.Forms.ListView) na każdej platformie. Proces ten jest w następujący sposób:

1. [Utwórz](#Creating_the_Custom_ListView_Control) formantu niestandardowego zestawu narzędzi Xamarin.Forms.
1. [Używanie](#Consuming_the_Custom_Control) kontrolki niestandardowej z zestawu narzędzi Xamarin.Forms.
1. [Utwórz](#Creating_the_Custom_Renderer_on_each_Platform) niestandardowego modułu renderowania kontrolki na każdej platformie.

Każdy element zostaną teraz dokładniej omówione w implementacji `NativeListView` modułu renderowania, wykorzystującego kontrolki listy specyficzne dla platformy i układów natywnych komórki. Ten scenariusz przydaje się podczas przenoszenia istniejących aplikacji natywnej, zawierający listę i komórki kodu, które mogą być ponownie używane. Ponadto umożliwia szczegółowe dostosowania różnych funkcji kontroli listy, które mogą wpłynąć na wydajność, takie jak wirtualizacja danych.

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>Tworzenie formantu niestandardowego ListView

Niestandardowy [ `ListView` ](xref:Xamarin.Forms.ListView) kontrolki mogą być tworzone przez podklasy `ListView` klasy, jak pokazano w poniższym przykładzie kodu:

```csharp
public class NativeListView : ListView
{
  public static readonly BindableProperty ItemsProperty =
    BindableProperty.Create ("Items", typeof(IEnumerable<DataSource>), typeof(NativeListView), new List<DataSource> ());

  public IEnumerable<DataSource> Items {
    get { return (IEnumerable<DataSource>)GetValue (ItemsProperty); }
    set { SetValue (ItemsProperty, value); }
  }

  public event EventHandler<SelectedItemChangedEventArgs> ItemSelected;

  public void NotifyItemSelected (object item)
  {
    if (ItemSelected != null) {
      ItemSelected (this, new SelectedItemChangedEventArgs (item));
    }
  }
}
```

`NativeListView` Jest tworzony w projekcie biblioteki .NET Standard i definiuje interfejs API dla formantu niestandardowego. Udostępnia tę kontrolkę `Items` właściwość, która jest używana do zapełniania `ListView` z danymi i które mogą być danymi powiązanymi dla wyświetlania celów. Ponadto udostępnia ona `ItemSelected` zdarzenia, które będzie uruchamiane zawsze, gdy element jest wybrany w kontrolce listy macierzystego specyficznego dla platformy. Aby uzyskać więcej informacji na temat tworzenia powiązań danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Korzystanie z kontrolki niestandardowej

`NativeListView` Formantu niestandardowego może odwoływać się w języku Xaml w projekcie biblioteki .NET Standard deklarowanie przestrzeni nazw dla lokalizacji i przy użyciu prefiksu przestrzeni nazw w formancie. Poniższy kod przedstawia przykładowy sposób, w jaki `NativeListView` kontrolki niestandardowej mogą być wykorzystane przez strony XAML:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*" />
            </Grid.RowDefinitions>
          <Label Text="{x:Static local:App.Description}" HorizontalTextAlignment="Center" />
            <local:NativeListView Grid.Row="1" x:Name="nativeListView" ItemSelected="OnItemSelected" VerticalOptions="FillAndExpand" />
          </Grid>
      </ContentPage.Content>
</ContentPage>
```

`local` Prefiks przestrzeni nazw może mieć nazwę niczego. Jednak `clr-namespace` i `assembly` wartości muszą być zgodne szczegóły kontrolki niestandardowej. Po zadeklarowaniu przestrzeń nazw prefiks jest używany do odwołania kontrolki niestandardowej.

Poniższy kod przedstawia przykładowy sposób, w jaki `NativeListView` kontrolki niestandardowej mogą być wykorzystane przez strony C#:

```csharp
public class MainPageCS : ContentPage
{
    NativeListView nativeListView;

    public MainPageCS()
    {
        nativeListView = new NativeListView
        {
            Items = DataSource.GetList(),
            VerticalOptions = LayoutOptions.FillAndExpand
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

        Content = new Grid
        {
            RowDefinitions = {
                new RowDefinition { Height = GridLength.Auto },
                new RowDefinition { Height = new GridLength (1, GridUnitType.Star) }
            },
            Children = {
                new Label { Text = App.Description, HorizontalTextAlignment = TextAlignment.Center },
                nativeListView
            }
        };
        nativeListView.ItemSelected += OnItemSelected;
    }
    ...
}
```

`NativeListView` Kontrolki niestandardowej użyto niestandardowe programy renderujące specyficzne dla platformy, aby wyświetlić listę danych, które jest wypełniana przy użyciu `Items` właściwości. Każdy wiersz na liście zawiera trzy elementy danych — nazwy, kategorii i nazwa pliku obrazu. Układ każdy wiersz na liście jest definiowany przez niestandardowego modułu renderowania specyficzne dla platformy.

> [!NOTE]
> Ponieważ `NativeListView` kontrolki niestandardowej będzie renderowany przy użyciu kontrolki listy specyficzne dla platformy, obejmujących przewijanie możliwości, formant niestandardowy powinna nie być hostowana w kontrolkach przewijany układ takich jak [ `ScrollView` ](xref:Xamarin.Forms.ScrollView).

Teraz można dodać niestandardowego modułu renderowania do każdego projektu aplikacji do tworzenia kontrolek listy specyficzne dla platformy i układy natywnych komórki.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Tworzenie niestandardowego modułu renderowania na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania jest następująca:

1. Utwórz podklasę `ListViewRenderer` klasę, która renderuje kontrolki niestandardowej.
1. Zastąp `OnElementChanged` metodę, która renderuje niestandardowe logiki kontrola i zapisz go dostosować. Ta metoda jest wywoływana, gdy odpowiedni zestaw narzędzi Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) zostanie utworzony.
1. Dodaj `ExportRenderer` atrybutów do klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania formantu niestandardowego zestawu narzędzi Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania przy użyciu zestawu narzędzi Xamarin.Forms.

> [!NOTE]
> Jest to opcjonalne zapewnić niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej w komórce będą używane.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](listview-images/solution-structure.png "NativeListView niestandardowego modułu renderowania projektu obowiązki")

`NativeListView` Formantu niestandardowego jest renderowany przez klasy renderowania specyficzne dla platformy, które wynikają z `ListViewRenderer` klasy dla każdej platformy. Skutkuje to każda `NativeListView` kontrolek niestandardowych, które są renderowane za pomocą kontrolki listy specyficzne dla platformy i układy natywnych komórki, jak pokazano na poniższych zrzutach ekranu:

![](listview-images/screenshots.png "NativeListView na każdej platformie")

`ListViewRenderer` Klasy ujawnia `OnElementChanged` metody, która jest wywoływana podczas tworzenia formantu niestandardowego zestawu narzędzi Xamarin.Forms do renderowania odpowiedniej kontrolki natywne. Ta metoda przyjmuje `ElementChangedEventArgs` parametr, który zawiera `OldElement` i `NewElement` właściwości. Te właściwości reprezentują element zestawu narzędzi Xamarin.Forms, modułu renderowania *został* dołączyć i element zestawu narzędzi Xamarin.Forms, modułu renderowania *jest* dołączone do, odpowiednio. W przykładowej aplikacji `OldElement` właściwość będzie miała `null` i `NewElement` właściwość będzie zawierać odwołanie do `NativeListView` wystąpienia.

Zastąpione wersję `OnElementChanged` metody, w każdej klasie renderowania specyficzne dla platformy jest w tym miejscu Przeprowadź Dostosowywanie kontrolki natywne. Wpisane odwołania do kontrolki natywne używane na platformie jest możliwy za pośrednictwem `Control` właściwości. Ponadto można uzyskać odwołanie do formantu Xamarin.Forms, który jest renderowany przy użyciu `Element` właściwości.

Należy zachować ostrożność podczas subskrybowania obsługi zdarzeń w `OnElementChanged` metody, jak pokazano w poniższym przykładzie kodu:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

Natywne kontrolki tylko należy skonfigurować i subskrypcję procedury obsługi zdarzeń, gdy niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms. Podobnie programy obsługi zdarzeń, które zostały zasubskrybowane przez powinna być usuwania subskrypcji tylko wtedy, gdy element renderer jest dołączony do zmiany. Przyjęcie tego podejścia pomoże utworzyć niestandardowego modułu renderowania, który nie odczuwają przecieków pamięci.

Zastąpione wersję `OnElementPropertyChanged` metody, w każdej klasie renderowania specyficzne dla platformy jest miejscem, aby odpowiadanie na zmiany właściwości możliwej do wiązania kontrolki niestandardowego zestawu narzędzi Xamarin.Forms. Sprawdź właściwości, które uległy zmianie powinien zawsze się, jak to zastąpienie może być wywoływana wiele razy.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje modułu renderowania za pomocą zestawu narzędzi Xamarin.Forms. Ten atrybut przyjmuje dwa parametry — Nazwa typu formantu niestandardowego zestawu narzędzi Xamarin.Forms renderowanego i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono wykonania każdej klasy specyficzne dla platformy niestandardowego modułu renderowania.

### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie niestandardowego modułu renderowania w systemie iOS

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy iOS:

```csharp
[assembly: ExportRenderer (typeof(NativeListView), typeof(NativeiOSListViewRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSListViewRenderer : ListViewRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null) {
                // Unsubscribe
            }

            if (e.NewElement != null) {
                Control.Source = new NativeiOSListViewSource (e.NewElement as NativeListView);
            }
        }
    }
}
```

`UITableView` Kontroli jest konfigurowana przez utworzenie wystąpienia `NativeiOSListViewSource` klasy, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms. Ta klasa dostarcza dane do `UITableView` kontroli przez zastąpienie `RowsInSection` i `GetCell` metody z `UITableViewSource` klasy, a przez udostępnianie `Items` właściwość, która zawiera lista danych, które mają być wyświetlane. Ta klasa oferuje również `RowSelected` zastąpienie metody, która wywołuje `ItemSelected` zdarzenie dostarczone przez `NativeListView` kontrolki niestandardowej. Aby uzyskać więcej informacji na temat metody zastąpienia, zobacz [UITableViewSource podklasy](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). `GetCell` Metoda zwraca `UITableCellView` jest wypełniana danymi dla każdego wiersza na liście i przedstawiono w poniższym przykładzie kodu:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
  // request a recycled cell to save memory
  NativeiOSListViewCell cell = tableView.DequeueReusableCell (cellIdentifier) as NativeiOSListViewCell;

  // if there are no cells to reuse, create a new one
  if (cell == null) {
    cell = new NativeiOSListViewCell (cellIdentifier);
  }

  if (String.IsNullOrWhiteSpace (tableItems [indexPath.Row].ImageFilename)) {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , null);
  } else {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , UIImage.FromFile ("Images/" + tableItems [indexPath.Row].ImageFilename + ".jpg"));
  }

  return cell;
}
```

Ta metoda tworzy `NativeiOSListViewCell` wystąpienia dla każdego wiersza danych, która będzie wyświetlana na ekranie. `NativeiOSCell` Wystąpienia definiuje układ każdej komórki, a dane w komórce. Gdy komórki zniknie z ekranu z powodu przewijania, komórki zostanie udostępniona do ponownego wykorzystania. Pozwala to uniknąć marnowania pamięci, zapewniając, że istnieją tylko `NativeiOSCell` wystąpień dla danych wyświetlanych na ekranie, a nie wszystkich danych znajdujących się na liście. Aby uzyskać więcej informacji na temat ponownego użycia komórki zobacz [ponownie użyć komórki](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). `GetCell` Odczytuje również metoda `ImageFilename` właściwości każdego wiersza danych, pod warunkiem, że istnieje i obraz, który odczytuje i zapisuje go jako `UIImage` wystąpienia przed zaktualizowaniem `NativeiOSListViewCell` wystąpienie z danymi (nazwa, kategoria i obrazu) wiersz.

`NativeiOSListViewCell` Klasa definiuje układ każdej komórki i przedstawiono w poniższym przykładzie kodu:

```csharp
public class NativeiOSListViewCell : UITableViewCell
{
  UILabel headingLabel, subheadingLabel;
  UIImageView imageView;

  public NativeiOSListViewCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
  {
    SelectionStyle = UITableViewCellSelectionStyle.Gray;

    ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);

    imageView = new UIImageView ();

    headingLabel = new UILabel () {
      Font = UIFont.FromName ("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB (127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    subheadingLabel = new UILabel () {
      Font = UIFont.FromName ("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB (38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add (headingLabel);
    ContentView.Add (subheadingLabel);
    ContentView.Add (imageView);
  }

  public void UpdateCell (string caption, string subtitle, UIImage image)
  {
    headingLabel.Text = caption;
    subheadingLabel.Text = subtitle;
    imageView.Image = image;
  }

  public override void LayoutSubviews ()
  {
    base.LayoutSubviews ();

    headingLabel.Frame = new CoreGraphics.CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
    subheadingLabel.Frame = new CoreGraphics.CGRect (100, 18, 100, 20);
    imageView.Frame = new CoreGraphics.CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Ta klasa definiuje kontrolki, używany do renderowania zawartości komórki i ich rozmieszczenie. `NativeiOSListViewCell` Konstruktor tworzy wystąpienia `UILabel` i `UIImageView` kontroluje i inicjuje ich występowania. Te kontrolki są używane do wyświetlania danych każdy wiersz z `UpdateCell` metody używanej do zestawu danych na `UILabel` i `UIImageView` wystąpień. Lokalizacja tych wystąpień jest ustawiana przez zastąpione `LayoutSubviews` metoda przez podanie współrzędnych w komórce.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reagowanie na zmiany właściwości w formancie niestandardowym

Jeśli `NativeListView.Items` właściwość zmieni się, ze względu na elementy są dodawane do lub usunięte z listy, niestandardowego modułu renderowania, które musi odpowiedzieć, wyświetlając zmiany. Można to osiągnąć przez zastąpienie `OnElementPropertyChanged` metody, która jest wyświetlana w poniższym przykładzie kodu:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

Ta metoda tworzy nowe wystąpienie klasy `NativeiOSListViewSource` klasę, która udostępnia dane z `UITableView` sterowania, pod warunkiem, że może być powiązana `NativeListView.Items` właściwości została zmieniona.

### <a name="creating-the-custom-renderer-on-android"></a>Tworzenie niestandardowego modułu renderowania w systemie Android

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy systemu Android:

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeAndroidListViewRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidListViewRenderer : ListViewRenderer
    {
        Context _context;

        public NativeAndroidListViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // unsubscribe
                Control.ItemClick -= OnItemClick;
            }

            if (e.NewElement != null)
            {
                // subscribe
                Control.Adapter = new NativeAndroidListViewAdapter(_context as Android.App.Activity, e.NewElement as NativeListView);
                Control.ItemClick += OnItemClick;
            }
        }
        ...

        void OnItemClick(object sender, Android.Widget.AdapterView.ItemClickEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(((NativeListView)Element).Items.ToList()[e.Position - 1]);
        }
    }
}
```

Natywnych `ListView` kontroli jest skonfigurowany, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms. Ta konfiguracja obejmuje utworzenie wystąpienia `NativeAndroidListViewAdapter` klasy, która udostępnia dane do natywnych `ListView` kontrolowanie i rejestrowanie programu obsługi zdarzeń do przetworzenia `ItemClick` zdarzeń. Z kolei ten program obsługi będzie wywoływać `ItemSelected` zdarzenie dostarczone przez `NativeListView` kontrolki niestandardowej. `ItemClick` Zdarzeń jest subskrypcja została anulowana, jeśli element zestawu narzędzi Xamarin.Forms modułu renderowania jest dołączony do zmiany.

`NativeAndroidListViewAdapter` Pochodzi od klasy `BaseAdapter` klasy i ujawnia `Items` właściwość, która zawiera lista danych, które mają być wyświetlane, a także zastępowanie `Count`, `GetView`, `GetItemId`, i `this[int]` metody. Aby uzyskać więcej informacji na temat tych zastąpienia metody, zobacz [Implementowanie ListAdapter](~/android/user-interface/layouts/list-view/populating.md). `GetView` Metoda zwraca widok dla każdego wiersza, wypełniony danymi i przedstawiono w poniższym przykładzie kodu:

```csharp
public override View GetView (int position, View convertView, ViewGroup parent)
{
  var item = tableItems [position];

  var view = convertView;
  if (view == null) {
    // no view to re-use, create new
    view = context.LayoutInflater.Inflate (Resource.Layout.NativeAndroidListViewCell, null);
  }
  view.FindViewById<TextView> (Resource.Id.Text1).Text = item.Name;
  view.FindViewById<TextView> (Resource.Id.Text2).Text = item.Category;

  // grab the old image and dispose of it
  if (view.FindViewById<ImageView> (Resource.Id.Image).Drawable != null) {
    using (var image = view.FindViewById<ImageView> (Resource.Id.Image).Drawable as BitmapDrawable) {
      if (image != null) {
        if (image.Bitmap != null) {
          //image.Bitmap.Recycle ();
          image.Bitmap.Dispose ();
        }
      }
    }
  }

  // If a new image is required, display it
  if (!String.IsNullOrWhiteSpace (item.ImageFilename)) {
    context.Resources.GetBitmapAsync (item.ImageFilename).ContinueWith ((t) => {
      var bitmap = t.Result;
      if (bitmap != null) {
        view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (bitmap);
        bitmap.Dispose ();
      }
    }, TaskScheduler.FromCurrentSynchronizationContext ());
  } else {
    // clear the image
    view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (null);
  }

  return view;
}
```

`GetView` Metoda jest wywoływana w celu zwrócenia komórki mógł być renderowany jako `View`, dla każdego wiersza danych na liście. Tworzy `View` wystąpienia dla każdego wiersza danych, która będzie wyświetlana na ekranie za pomocą wygląd `View` wystąpienia, które są zdefiniowane w pliku układu. Gdy komórki zniknie z ekranu z powodu przewijania, komórki zostanie udostępniona do ponownego wykorzystania. Pozwala to uniknąć marnowania pamięci, zapewniając, że istnieją tylko `View` wystąpień dla danych wyświetlanych na ekranie, a nie wszystkich danych znajdujących się na liście. Aby uzyskać więcej informacji na temat ponownego użycia widoku zobacz [wiersz widoku ponownego użycia](~/android/user-interface/layouts/list-view/populating.md).

`GetView` Metoda również wypełnia `View` wystąpienie z danych, w tym odczytywanie danych obrazu z nazwy pliku określonej w `ImageFilename` właściwości.

Układ każdej dispayed komórki w natywnym `ListView` jest zdefiniowany w `NativeAndroidListViewCell.axml` plik układu, który jest zwiększony przez `LayoutInflater.Inflate` metody. Poniższy przykładowy kod przedstawia definicję układu:

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
            android:id="@+id/Text1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/Text2"
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

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reagowanie na zmiany właściwości w formancie niestandardowym

Jeśli `NativeListView.Items` właściwość zmieni się, ze względu na elementy są dodawane do lub usunięte z listy, niestandardowego modułu renderowania, które musi odpowiedzieć, wyświetlając zmiany. Można to osiągnąć przez zastąpienie `OnElementPropertyChanged` metody, która jest wyświetlana w poniższym przykładzie kodu:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

Ta metoda tworzy nowe wystąpienie klasy `NativeAndroidListViewAdapter` klasy, która udostępnia dane do natywnych `ListView` sterowania, pod warunkiem, że może być powiązana `NativeListView.Items` właściwości została zmieniona.

### <a name="creating-the-custom-renderer-on-uwp"></a>Tworzenie niestandardowego modułu renderowania na platformy uniwersalnej systemu Windows

Poniższy przykład kodu pokazuje niestandardowego modułu renderowania dla platformy uniwersalnej systemu Windows:

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeUWPListViewRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPListViewRenderer : ListViewRenderer
    {
        ListView listView;

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            listView = Control as ListView;

            if (e.OldElement != null)
            {
                // Unsubscribe
                listView.SelectionChanged -= OnSelectedItemChanged;
            }

            if (e.NewElement != null)
            {
                listView.SelectionMode = ListViewSelectionMode.Single;
                listView.IsItemClickEnabled = false;
                listView.ItemsSource = ((NativeListView)e.NewElement).Items;             
                listView.ItemTemplate = App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
                // Subscribe
                listView.SelectionChanged += OnSelectedItemChanged;
            }  
        }

        void OnSelectedItemChanged(object sender, SelectionChangedEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(listView.SelectedItem);
        }
    }
}
```

Natywnych `ListView` kontroli jest skonfigurowany, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu zestawu narzędzi Xamarin.Forms. Ta konfiguracja obejmuje ustawienia jak natywnych `ListView` kontrolki będą odpowiadać na elementy są zaznaczone, wypełnianie danych wyświetlany przez kontrolkę, definiując wygląd i zawartość każdej komórki i rejestrowania programu obsługi zdarzeń do przetworzenia `SelectionChanged` zdarzeń. Z kolei ten program obsługi będzie wywoływać `ItemSelected` zdarzenie dostarczone przez `NativeListView` kontrolki niestandardowej. `SelectionChanged` Zdarzeń jest subskrypcja została anulowana, jeśli element zestawu narzędzi Xamarin.Forms modułu renderowania jest dołączony do zmiany.

Wygląd i zawartość każdego native `ListView` komórki są definiowane przez `DataTemplate` o nazwie `ListViewItemTemplate`. To `DataTemplate` znajduje się w słowniku zasobów na poziomie aplikacji i przedstawiono w poniższym przykładzie kodu:

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="#DAFF7F">
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

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Reagowanie na zmiany właściwości w formancie niestandardowym

Jeśli `NativeListView.Items` właściwość zmieni się, ze względu na elementy są dodawane do lub usunięte z listy, niestandardowego modułu renderowania, które musi odpowiedzieć, wyświetlając zmiany. Można to osiągnąć przez zastąpienie `OnElementPropertyChanged` metody, która jest wyświetlana w poniższym przykładzie kodu:

```csharp
protected override void OnElementPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
    base.OnElementPropertyChanged(sender, e);

    if (e.PropertyName == NativeListView.ItemsProperty.PropertyName)
    {
        listView.ItemsSource = ((NativeListView)Element).Items;
    }
}
```

Metoda wypełni ponownie natywnych `ListView` kontrolki z danymi zmienione, pod warunkiem, że może być powiązana `NativeListView.Items` właściwości została zmieniona.

## <a name="summary"></a>Podsumowanie

W tym artykule ma pokazano sposób tworzenia niestandardowego modułu renderowania hermetyzujący kontrolki listy specyficzne dla platformy i układy natywnych komórki, dzięki czemu większa kontrola nad natywnych listy kontrolowania wydajności.


## <a name="related-links"></a>Linki pokrewne

- [CustomRendererListView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
