---
title: Klasy podstawowe renderowania i kontrolki natywne
description: Formant co platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu. W tym artykule wymieniono renderowania i klasy macierzystego formantu, które implementują każdej platformy Xamarin.Forms strony, układ widoku i komórki.
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 9402bd53ab3bfb0b11182eb700aa560e8f962de3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="renderer-base-classes-and-native-controls"></a>Klasy podstawowe renderowania i kontrolki natywne

_Formant co platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu. W tym artykule wymieniono renderowania i klasy macierzystego formantu, które implementują każdej platformy Xamarin.Forms strony, układ widoku i komórki._

Z wyjątkiem produktów `MapRenderer` klasy renderowania specyficzne dla platformy można znaleźć w następujących przestrzeni nazw:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** — Xamarin.Forms.Platform.Android.AppCompat
- **Platforma uniwersalna systemu Windows (UWP)** — Xamarin.Forms.Platform.UWP

`MapRenderer` Klasy można znaleźć w następujących przestrzeni nazw:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Platforma uniwersalna systemu Windows (UWP)** — Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Strony

W poniższej tabeli wymieniono renderowania i macierzystego formantu klasy, które implementują każdej platformy Xamarin.Forms [strony](~/xamarin-forms/user-interface/controls/pages.md) typu:

|Strona|Moduł renderowania|iOS|Android|Android (AppCompat)|Platforma UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|Grupie widoków||FrameworkElement|
|[`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)|PhoneMasterDetailRenderer (iOS — telefonu), TabletMasterDetailPageRenderer (iOS — Tablet), MasterDetailRenderer (Android), MasterDetailPageRenderer (AppCompat systemu Android), MasterDetailPageRenderer (UWP)|UIViewController (telefon) UISplitViewController (Tablet)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (formant niestandardowy)|
|[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)|NavigationRenderer (z systemem iOS i Android), NavigationPageRenderer (AppCompat systemu Android), NavigationPageRenderer (UWP)|UIToolbar|Grupie widoków|Grupie widoków|FrameworkElement (formant niestandardowy)|
|[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)|TabbedRenderer (z systemem iOS i Android), TabbedPageRenderer (AppCompat systemu Android), TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (Tabela przestawna)|
|[`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)|PageRenderer|UIViewController|Grupie widoków||FrameworkElement|
|[`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (właściwości FlipView)|

## <a name="layouts"></a>Układy

W poniższej tabeli wymieniono renderowania i macierzystego formantu klasy, które implementują każdej platformy Xamarin.Forms [układu](~/xamarin-forms/user-interface/controls/layouts.md) typu:

|Układ|Moduł renderowania|iOS|Android|Platforma UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)|ViewRenderer|UIView|Widok|FrameworkElement|
|[`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)|ViewRenderer|UIView|Widok|FrameworkElement|
|[`Frame`](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/)|FrameRenderer|UIView|Grupie widoków|Obramowanie|
|[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)|ViewRenderer|UIView|Widok|FrameworkElement|
|[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)|ViewRenderer|UIView|Widok|FrameworkElement|
|[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)|ViewRenderer|UIView|Widok|FrameworkElement|
|[`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)|ViewRenderer|UIView|Widok|FrameworkElement|
|[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)|ViewRenderer|UIView|Widok|FrameworkElement|

## <a name="views"></a>Widoki

W poniższej tabeli wymieniono renderowania i macierzystego formantu klasy, które implementują każdej platformy Xamarin.Forms [widoku](~/xamarin-forms/user-interface/controls/views.md) typu:

|Widoki|Moduł renderowania|iOS|Android|Android (AppCompat)|Platforma UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)|BoxRenderer (z systemem iOS i Android), BoxViewRenderer (Windows Phone i WinRT)|UIView|Grupie widoków||Prostokąt|
|[`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)|ButtonRenderer|UIButton|Przycisk|AppCompatButton|Przycisk|
|[`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/)|CarouselViewRenderer|UIScrollView|RecyclerView||Właściwości FlipView|
|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)|DatePickerRenderer|UITextField|Typu części EditText||DatePicker|
|[`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)|EditorRenderer|UITextView|Typu części EditText||TextBox|
|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|Typu części EditText||TextBox|
|[`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)|ImageRenderer|UIImageView|ImageView||Obraz|
|[`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)|LabelRenderer|UILabel|Element TextView||TextBlock|
|[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)|PickerRenderer|UITextField|Typu części EditText|Typu części EditText|ComboBox|
|[`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)|SliderRenderer|UISlider|SeekBar||Suwak|
|[`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|StepperRenderer|UIStepper|LinearLayout||Formant|
|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|SwitchRenderer|UISwitch|Przełącznik|SwitchCompat|ToggleSwitch|
|[`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|TimePickerRenderer|UITextField|Typu części EditText||TimePicker|
|[`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)|WebViewRenderer|UIWebView|Widok sieci Web||Widok sieci Web|

## <a name="cells"></a>Komórki

W poniższej tabeli wymieniono renderowania i macierzystego formantu klasy, które implementują każdej platformy Xamarin.Forms [komórki](~/xamarin-forms/user-interface/controls/cells.md) typu:

|Komórki|Moduł renderowania|iOS|Android|Platforma UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/)|EntryCellRenderer|UITableViewCell z UITextField|LinearLayout TextView i typu części EditText|DataTemplate z pola tekstowego|
|[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/)|SwitchCellRenderer|UITableViewCell z UISwitch|Przełącznik|DataTemplate z siatką zawierający blok tekstu i ToggleSwitch|
|[`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/)|TextCellRenderer|UITableViewCell|LinearLayout z dwóch TextViews|DataTemplate z Panel stosu zawierającego dwa obiekty TextBlock|
|[`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/)|ImageCellRenderer|UITableViewCell z UIImage|LinearLayout z dwóch TextViews i ImageView|DataTemplate siatki, zawierający obraz i dwa obiekty TextBlock|
|[`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|Widok|DataTemplate z ContentPresenter|

## <a name="summary"></a>Podsumowanie

W tym artykule zgłosił renderowania i klasy macierzystego formantu, które implementują każdej platformy Xamarin.Forms strony, układ widoku i komórki. Formant co platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu.

## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe moduły renderowania (Xamarin University wideo)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
