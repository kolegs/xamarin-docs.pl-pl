---
title: Dostosowywanie ListView
description: ListView platformy Xamarin.Forms jest widoku, który będzie wyświetlany jako pionowy listy zbierania danych. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania hermetyzujący kontrolki listy specyficzne dla platformy i układy natywnego komórki, dzięki czemu większa kontrola nad macierzysty listy kontrolowania wydajności.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0d1afc2c14b19bbd03244affed494405776a3c99
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="customizing-a-listview"></a>Dostosowywanie ListView

_ListView platformy Xamarin.Forms jest widoku, który będzie wyświetlany jako pionowy listy zbierania danych. W tym artykule przedstawiono sposób tworzenia niestandardowego modułu renderowania hermetyzujący kontrolki listy specyficzne dla platformy i układy natywnego komórki, dzięki czemu większa kontrola nad macierzysty listy kontrolowania wydajności._

Każdy widok platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu. Gdy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) jest renderowany przez aplikację platformy Xamarin.Forms w systemie iOS `ListViewRenderer` tworzenia wystąpienia klasy, która z kolei tworzy natywny `UITableView` formantu. Na platformie Android `ListViewRenderer` natywny tworzy wystąpienie klasy `ListView` formantu. W systemie Windows platformy Uniwersalnej, `ListViewRenderer` natywny tworzy wystąpienie klasy `ListView` formantu. Aby uzyskać więcej informacji na temat klasy macierzystego formantu, mapowane na formanty platformy Xamarin.Forms i renderowania, zobacz [renderowania klasy podstawowej i kontrolki natywne](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Na poniższym diagramie przedstawiono związek między [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kontroli i odpowiednie natywnego formantów, które implementuje ona:

![](listview-images/listview-classes.png "Relacja między formantu ListView i implementujący kontrolki natywne")

Proces renderowania można podjąć zaletą do zaimplementowania dostosowań specyficzne dla platformy przez utworzenie niestandardowego modułu renderowania dla [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) na każdej z platform. Proces ten wygląda następująco:

1. [Utwórz](#Creating_the_Custom_ListView_Control) kontrolkę niestandardową platformy Xamarin.Forms.
1. [Korzystać z](#Consuming_the_Custom_Control) kontrolka niestandardowa z platformy Xamarin.Forms.
1. [Utwórz](#Creating_the_Custom_Renderer_on_each_Platform) niestandardowego modułu renderowania dla formantu w każdej z platform.

Każdy element teraz omówione zostaną z kolei, aby zaimplementować `NativeListView` renderowania wykorzystującego kontrolki listy specyficzne dla platformy i układy natywnego komórki. Ten scenariusz przydaje się podczas przenoszenia istniejących aplikacji natywnej zawierający listę i kod komórki, które mogą być ponownie używane. Ponadto umożliwia szczegółowe dostosowanie listy funkcje kontroli, które mogą wpłynąć na wydajność, takie jak wirtualizacja danych.

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>Tworzenie niestandardowych ListView — formant

Niestandardowy [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) formant może zostać utworzony przez podklasy `ListView` klasy, jak pokazano w poniższym przykładzie:

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

`NativeListView` Jest tworzony w .NET Standard projektu biblioteki i definiuje interfejsu API dla kontrolki niestandardowej. Ten formant przedstawia `Items` właściwość, która służy do wypełniania `ListView` przy użyciu danych i które mogą być danymi powiązanymi do wyświetlenia celów. Również przedstawia `ItemSelected` zdarzenie, które będą wywoływane zawsze, gdy element jest zaznaczony w formancie listy natywnego specyficzne dla platformy. Aby uzyskać więcej informacji na temat wiązania danych, zobacz [podstawy powiązania danych](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Korzystanie z formantu niestandardowego

`NativeListView` Formant niestandardowy może być przywoływany w języku Xaml w .NET Standard projektu biblioteki deklarowanie przestrzeni nazw dla lokalizacji, a następnie użyć prefiksu przestrzeni nazw w formancie. Poniższy kod przedstawia przykład sposobu `NativeListView` kontrolki niestandardowej, może być zużyte przez strony XAML:

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

`local` Prefiks przestrzeni nazw może mieć nazwę żadnych czynności. Jednak `clr-namespace` i `assembly` wartości muszą być zgodne szczegóły kontrolki niestandardowej. Po zadeklarowaniu obszaru nazw prefiks jest używany do odwołania kontrolki niestandardowej.

Poniższy kod przedstawia przykład sposobu `NativeListView` kontrolki niestandardowej mogą być używane przez stronę C#:

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

`NativeListView` Kontrolki niestandardowej używa niestandardowe moduły renderowania specyficzne dla platformy, aby wyświetlić listę danych, które są wprowadzane przy użyciu `Items` właściwości. Każdy wiersz na liście zawiera trzy elementy danych — nazwę, kategorię i nazwę pliku obrazu. Układ każdy wiersz na liście jest zdefiniowana przez niestandardowego modułu renderowania specyficzne dla platformy.

> [!NOTE]
> Ponieważ `NativeListView` kontrolki niestandardowej będzie renderowany przy użyciu formantów listy specyficzne dla platformy, które obejmują przewijanie możliwości, formantu niestandardowego należy nie jest hostowana w formantach przewijanego układu takich jak [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/).

Teraz można dodać niestandardowego modułu renderowania do każdego projektu aplikacji do tworzenia kontrolki listy specyficzne dla platformy i układy natywnego komórki.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Tworzenie niestandardowego modułu renderowania na każdej platformie

Proces tworzenia klasy niestandardowego modułu renderowania wygląda następująco:

1. Utwórz podklasę `ListViewRenderer` klasy, która renderuje kontrolkę niestandardową.
1. Zastąpienie `OnElementChanged` metodę, która renderuje niestandardowej logiki kontroli i zapisu, aby dostosować go. Ta metoda jest wywoływana, gdy odpowiednie platformy Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) jest tworzony.
1. Dodaj `ExportRenderer` atrybutu klasy niestandardowego modułu renderowania, aby określić, że będą używane do renderowania Kontrolki niestandardowe platformy Xamarin.Forms. Ten atrybut służy do rejestrowania niestandardowego modułu renderowania z platformy Xamarin.Forms.

> [!NOTE]
> Jest to pozycja opcjonalna zapewnienie niestandardowego modułu renderowania w każdym projekcie platformy. Jeśli nie jest zarejestrowany niestandardowego modułu renderowania, domyślne renderowanie dla klasy podstawowej komórki będzie używany.

Na poniższym diagramie przedstawiono obowiązki każdego projektu w przykładowej aplikacji, oraz relacje między nimi:

![](listview-images/solution-structure.png "Obowiązki NativeListView niestandardowe renderowania projektu:")

`NativeListView` Kontrolki niestandardowej jest renderowany przez klasy renderowania specyficzne dla platformy, które wynikają z `ListViewRenderer` klasy dla każdej platformy. Powoduje to w każdym `NativeListView` kontrolki niestandardowej renderowanego z kontrolki listy specyficzne dla platformy i układy natywnego komórki, jak pokazano na poniższych zrzutach ekranu:

![](listview-images/screenshots.png "NativeListView na każdej platformie")

`ListViewRenderer` Klasy ujawnia `OnElementChanged` metodę, która jest wywoływana po utworzeniu platformy Xamarin.Forms formantu niestandardowego do renderowania kontrolki na odpowiednich macierzystego. Ta metoda przyjmuje `ElementChangedEventArgs` parametr, który zawiera `OldElement` i `NewElement` właściwości. Te właściwości reprezentuje elementu platformy Xamarin.Forms który renderującego *został* dołączyć i elementu platformy Xamarin.Forms który renderującego *jest* dołączony do, odpowiednio. W przykładowej aplikacji `OldElement` właściwość będzie `null` i `NewElement` właściwości będzie zawierać odwołanie do `NativeListView` wystąpienia.

Zastąpiona wersja `OnElementChanged` metody w każdej klasie renderowania specyficzne dla platformy jest miejscem do wykonania dostosowanie macierzystego formantu. Typu odwołanie do macierzystego formantu używanego na platformie jest możliwy za pośrednictwem `Control` właściwości. Ponadto można uzyskać odwołanie do formantu platformy Xamarin.Forms, który jest renderowany przy użyciu `Element` właściwości.

Należy zachować ostrożność podczas subskrybowania obsługi zdarzeń w `OnElementChanged` metody, jak pokazano w poniższym przykładzie:

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

Macierzystego formantu tylko należy skonfigurować i subskrypcję procedury obsługi zdarzeń, gdy niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms. Podobnie programy obsługi zdarzeń, które zostały subskrybuje, należy anulować w tylko wtedy, gdy element renderującego jest dołączony do zmiany. Przyjmowanie takie podejście pomoże utworzyć niestandardowego modułu renderowania nie boryka się z przecieki pamięci.

Zastąpiona wersja `OnElementPropertyChanged` metody w każdej klasie renderowania specyficzne dla platformy jest używana do reagowania na zmiany właściwości możliwej do wiązania na platformy Xamarin.Forms kontrolki niestandardowej. Sprawdź właściwości, które uległy zmianie zawsze należy, jak to zastąpienie może zostać wywołana wiele razy.

Każda klasa niestandardowego modułu renderowania zostanie nadany `ExportRenderer` atrybut, który rejestruje mechanizm renderujący platformy Xamarin.Forms. Atrybut przyjmuje dwa parametry — Nazwa typu formantu niestandardowego platformy Xamarin.Forms renderowanego i nazwę typu niestandardowego modułu renderowania. `assembly` Prefiks atrybutu określa, że ten atrybut ma zastosowanie do całego zestawu.

W poniższych sekcjach omówiono implementacji każdej klasy niestandardowego modułu renderowania specyficzne dla platformy.

### <a name="creating-the-custom-renderer-on-ios"></a>Tworzenie modułu renderowania niestandardowe w systemie iOS

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

`UITableView` Sterowanie jest konfigurowane przez utworzenie wystąpienia `NativeiOSListViewSource` klasy, pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms. Ta klasa dostarcza dane do `UITableView` kontroli przez zastąpienie `RowsInSection` i `GetCell` metody `UITableViewSource` klasy, a przez udostępnianie `Items` właściwość, która zawiera listę danych do wyświetlenia. Udostępnia klasy `RowSelected` zastąpienie metody, która wywołuje `ItemSelected` zdarzenia dostarczone przez `NativeListView` kontrolki niestandardowej. Aby uzyskać więcej informacji o metodzie przesłania, zobacz [UITableViewSource podklasy](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). `GetCell` Metoda zwraca `UITableCellView` jest wypełniana dane dla każdego wiersza na liście i przedstawiono w poniższym przykładzie:

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

Ta metoda tworzy `NativeiOSListViewCell` wystąpienia dla każdego wiersza danych, który będzie wyświetlany na ekranie. `NativeiOSCell` Wystąpienia definiuje układ każdej komórki, a dane w komórce. Gdy komórki zniknie z ekranu z powodu przewijania, komórki będą dostępne do ponownego użycia. Dzięki temu można uniknąć traci pamięci przez zapewnienie, że istnieją tylko `NativeiOSCell` wystąpień dla danych będzie wyświetlany na ekranie, a nie wszystkie dane na liście. Aby uzyskać więcej informacji na temat ponownemu komórki, zobacz [komórki ponowne użycie](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). `GetCell` Metoda odczytuje również `ImageFilename` właściwości każdego wiersza danych, pod warunkiem, że istnieje i obraz odczytuje i zapisuje go jako `UIImage` wystąpienia przed zaktualizowaniem `NativeiOSListViewCell` wystąpienie z danymi (nazwa, kategoria i obrazu) wiersz.

`NativeiOSListViewCell` Klasy definiuje układu dla każdej komórki i przedstawiono w poniższym przykładzie:

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

Ta klasa definiuje formanty używany do renderowania zawartości komórki przez użytkownika i ich układ. `NativeiOSListViewCell` Konstruktora tworzy wystąpienia `UILabel` i `UIImageView` formanty oraz inicjuje ich wyglądu. Formanty te służą do wyświetlania każdego wiersza danych, z `UpdateCell` metodę ustawić te dane w `UILabel` i `UIImageView` wystąpień. Lokalizacja tych wystąpień jest ustawiana przez przesłoniętych `LayoutSubviews` metody, określając ich współrzędnych wewnątrz komórki przy realizacji.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Odpowiada na żądania zmiany właściwości formantu niestandardowego

Jeśli `NativeListView.Items` właściwości zmian z powodu dodawane do elementów lub usunięty z listy, niestandardowego modułu renderowania musi odpowiedzieć, wyświetlając zmiany. Można to osiągnąć przez zastąpienie `OnElementPropertyChanged` metodę, która jest wyświetlana w poniższym przykładzie:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

Ta metoda tworzy nowe wystąpienie klasy `NativeiOSListViewSource` klasy, która dostarcza dane do `UITableView` kontrolować, pod warunkiem, że można powiązać `NativeListView.Items` właściwość zostanie zmieniona.

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

Natywnego `ListView` kontroli jest skonfigurowana pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms. Ta konfiguracja obejmuje utworzenie wystąpienia `NativeAndroidListViewAdapter` klasy, która dostarcza dane do natywnego `ListView` kontroli i rejestrowania programu obsługi zdarzeń do przetwarzania `ItemClick` zdarzeń. Z kolei ten program obsługi wywoła `ItemSelected` zdarzenia dostarczone przez `NativeListView` kontrolki niestandardowej. `ItemClick` Zdarzeń jest Anulowano subskrypcję, jeśli element platformy Xamarin.Forms renderującego jest dołączony do zmiany.

`NativeAndroidListViewAdapter` Pochodną `BaseAdapter` klasy i ujawnia `Items` właściwość, która zawiera dane mają być wyświetlane na liście, a także zastępowanie `Count`, `GetView`, `GetItemId`, i `this[int]` metody. Aby uzyskać więcej informacji na temat te zastąpienia metody, zobacz [implementacja ListAdapter](~/android/user-interface/layouts/list-view/populating.md). `GetView` Metoda zwraca widok dla każdego wiersza, zawierają dane i przedstawiono w poniższym przykładzie:

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

`GetView` Wywoływana jest metoda zwraca komórki ma być renderowany jako `View`, dla każdego wiersza danych na liście. Tworzy `View` wystąpienia dla każdego wiersza danych, który będzie wyświetlany na ekranie z wygląd `View` wystąpienia są zdefiniowane w pliku układu. Gdy komórki zniknie z ekranu z powodu przewijania, komórki będą dostępne do ponownego użycia. Dzięki temu można uniknąć traci pamięci przez zapewnienie, że istnieją tylko `View` wystąpień dla danych będzie wyświetlany na ekranie, a nie wszystkie dane na liście. Aby uzyskać więcej informacji na temat ponownemu widoku, zobacz [wiersza widoku ponownego użycia](~/android/user-interface/layouts/list-view/populating.md).

`GetView` Metoda również wypełnia `View` wystąpienia o danych, w tym odczytywania danych obrazów z nazwy pliku określonej w `ImageFilename` właściwości.

Układ dispayed każdej komórki przez natywnego `ListView` jest zdefiniowany w `NativeAndroidListViewCell.axml` pliku układu, który jest zwiększony przez `LayoutInflater.Inflate` metody. W poniższym przykładzie przedstawiono definicję układu:

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

Ten układ Określa, że dwa `TextView` formantów i `ImageView` formantu są używane do wyświetlenia zawartości komórki. Dwa `TextView` kontrolki są orientacji pionowej w `LinearLayout` sterowania wszystkie formanty, które są zawarte w `RelativeLayout`.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Odpowiada na żądania zmiany właściwości formantu niestandardowego

Jeśli `NativeListView.Items` właściwości zmian z powodu dodawane do elementów lub usunięty z listy, niestandardowego modułu renderowania musi odpowiedzieć, wyświetlając zmiany. Można to osiągnąć przez zastąpienie `OnElementPropertyChanged` metodę, która jest wyświetlana w poniższym przykładzie:

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

Ta metoda tworzy nowe wystąpienie klasy `NativeAndroidListViewAdapter` klasy, która dostarcza dane do natywnego `ListView` kontrolować, pod warunkiem, że można powiązać `NativeListView.Items` właściwość zostanie zmieniona.

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

Natywnego `ListView` kontroli jest skonfigurowana pod warunkiem, że niestandardowego modułu renderowania jest dołączony do nowego elementu platformy Xamarin.Forms. Ta konfiguracja obejmuje ustawienia jak natywnego `ListView` formant będzie odpowiadać elementów, podczas wypełniania danych wyświetlany przez formant, definiowanie wygląd i zawartość każdej komórki i rejestrowania programu obsługi zdarzeń do przetwarzania `SelectionChanged` zdarzeń. Z kolei ten program obsługi wywoła `ItemSelected` zdarzenia dostarczone przez `NativeListView` kontrolki niestandardowej. `SelectionChanged` Zdarzeń jest Anulowano subskrypcję, jeśli element platformy Xamarin.Forms renderującego jest dołączony do zmiany.

Wygląd i zawartość poszczególnych native `ListView` komórki są definiowane przez `DataTemplate` o nazwie `ListViewItemTemplate`. To `DataTemplate` znajduje się w słowniku zasobów na poziomie aplikacji i przedstawiono w poniższym przykładzie:

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

`DataTemplate` Określa formanty używane do wyświetlenia zawartości komórki, a ich układu i wyglądu. Dwa `TextBlock` formantów i `Image` formantu są używane do wyświetlenia zawartości komórki przez powiązanie danych. Ponadto wystąpienia `ConcatImageExtensionConverter` służy do łączenia `.jpg` rozszerzenie nazwy pliku każdego obrazu. Gwarantuje to, że `Image` formantu można załadować i renderować obraz, gdy jest `Source` właściwość jest ustawiona.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Odpowiada na żądania zmiany właściwości formantu niestandardowego

Jeśli `NativeListView.Items` właściwości zmian z powodu dodawane do elementów lub usunięty z listy, niestandardowego modułu renderowania musi odpowiedzieć, wyświetlając zmiany. Można to osiągnąć przez zastąpienie `OnElementPropertyChanged` metodę, która jest wyświetlana w poniższym przykładzie:

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

Metoda ponownie wypełnia natywnego `ListView` formantu z danymi zmienione, pod warunkiem, że można powiązać `NativeListView.Items` właściwość zostanie zmieniona.

## <a name="summary"></a>Podsumowanie

W tym artykule ma przedstawiono sposób tworzenia niestandardowego modułu renderowania hermetyzujący kontrolki listy specyficzne dla platformy i układy natywnego komórki, dzięki czemu większa kontrola nad macierzysty listy kontrolowania wydajności.


## <a name="related-links"></a>Linki pokrewne

- [CustomRendererListView (przykład)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
