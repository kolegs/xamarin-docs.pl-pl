---
title: Natywne kontrolki i klasy bazowe programu renderującego
description: Każdy formant zestawu narzędzi Xamarin.Forms ma towarzyszący modułu renderowania dla każdej platformy, która tworzy wystąpienie kontrolki natywne. W tym artykule wymieniono renderowania i klasy natywne kontrolki, które implementują strony, układ, widok i komórki każdego zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 01610581a0fd72401cb6daa4d5c8c2cb99fe6a2f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995111"
---
# <a name="renderer-base-classes-and-native-controls"></a>Natywne kontrolki i klasy bazowe programu renderującego

_Każdy formant zestawu narzędzi Xamarin.Forms ma towarzyszący modułu renderowania dla każdej platformy, która tworzy wystąpienie kontrolki natywne. W tym artykule wymieniono renderowania i klasy natywne kontrolki, które implementują strony, układ, widok i komórki każdego zestawu narzędzi Xamarin.Forms._

Z wyjątkiem produktów `MapRenderer` klasy, moduły renderowania specyficzne dla platformy można znaleźć w następujących przestrzeni nazw:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** — Xamarin.Forms.Platform.Android.AppCompat
- **Platforma Universal Windows (UWP)** — Xamarin.Forms.Platform.UWP

`MapRenderer` Klasy można znaleźć w następujących przestrzeni nazw:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Platforma Universal Windows (UWP)** — Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Strony

W poniższej tabeli wymieniono renderowania i klasy natywne kontrolki, które implementują każdego zestawu narzędzi Xamarin.Forms [strony](~/xamarin-forms/user-interface/controls/pages.md) typu:

|Strona|Moduł renderowania|iOS|Android|Android (AppCompat)|Platforma UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|Grupie widoków||FrameworkElement|
|[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)|PhoneMasterDetailRenderer (system iOS — telefon), TabletMasterDetailPageRenderer (system iOS — tablecie), MasterDetailRenderer (Android), MasterDetailPageRenderer (AppCompat dla systemu Android), MasterDetailPageRenderer systemu Windows (UWP)|UIViewController (telefon), UISplitViewController (Tablet)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (kontrolki niestandardowej)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|NavigationRenderer (iOS i Android), NavigationPageRenderer (AppCompat dla systemu Android), NavigationPageRenderer systemu Windows (UWP)|UIToolbar|Grupie widoków|Grupie widoków|FrameworkElement (kontrolki niestandardowej)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|TabbedRenderer (iOS i Android), TabbedPageRenderer (AppCompat dla systemu Android), TabbedPageRenderer systemu Windows (UWP)|UIView|Obiekt ViewPager|Obiekt ViewPager|FrameworkElement (Tabela przestawna)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|UIViewController|Grupie widoków||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView|Obiekt ViewPager|Obiekt ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>Układy

W poniższej tabeli wymieniono renderowania i klasy natywne kontrolki, które implementują każdego zestawu narzędzi Xamarin.Forms [układ](~/xamarin-forms/user-interface/controls/layouts.md) typu:

|Układ|Moduł renderowania|iOS|Android|Platforma UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)|ViewRenderer|UIView|Widok|FrameworkElement|
|[`ContentView`](xref:Xamarin.Forms.ContentView)|ViewRenderer|UIView|Widok|FrameworkElement|
|[`Frame`](xref:Xamarin.Forms.Frame)|FrameRenderer|UIView|Grupie widoków|Obramowanie|
|[`ScrollView`](xref:Xamarin.Forms.ScrollView)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)|ViewRenderer|UIView|Widok|FrameworkElement|
|[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)|ViewRenderer|UIView|Widok|FrameworkElement|
|[`Grid`](xref:Xamarin.Forms.Grid)|ViewRenderer|UIView|Widok|FrameworkElement|
|[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)|ViewRenderer|UIView|Widok|FrameworkElement|
|[`StackLayout`](xref:Xamarin.Forms.StackLayout)|ViewRenderer|UIView|Widok|FrameworkElement|

## <a name="views"></a>Widoki

W poniższej tabeli wymieniono renderowania i klasy natywne kontrolki, które implementują każdego zestawu narzędzi Xamarin.Forms [widoku](~/xamarin-forms/user-interface/controls/views.md) typu:

|Widoki|Moduł renderowania|iOS|Android|Android (AppCompat)|Platforma UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer (iOS i Android), BoxViewRenderer systemu Windows (UWP)|UIView|Grupie widoków||Prostokąt|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|Obiektu klasy UIButton|Przycisk|AppCompatButton|Przycisk|
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|UITextField|Typu części EditText||DatePicker|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|Typu części EditText||TextBox|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|Typu części EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||Obraz|
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|Element TextView||TextBlock|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|UITextField|Typu części EditText|Typu części EditText|ComboBox|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|SeekBar||Suwak|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||Formant|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|Przełącznik|SwitchCompat|ToggleSwitch|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|UITextField|Typu części EditText||TimePicker|
|[`WebView`](xref:Xamarin.Forms.WebView)|WebViewRenderer|UIWebView|Widok sieci Web||Widok sieci Web|

## <a name="cells"></a>Komórki

W poniższej tabeli wymieniono renderowania i klasy natywne kontrolki, które implementują każdego zestawu narzędzi Xamarin.Forms [komórki](~/xamarin-forms/user-interface/controls/cells.md) typu:

|Komórki|Moduł renderowania|iOS|Android|Platforma UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|UITableViewCell z UITextField|LinearLayout TextView i typu części EditText|DataTemplate za pomocą TextBox|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|UITableViewCell z UISwitch|Przełącznik|DataTemplate z siatką zawierającą TextBlock i ToggleSwitch|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|LinearLayout za pomocą dwóch TextViews|DataTemplate z StackPanel zawierającą dwa obiekty TextBlock|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|UITableViewCell z UIImage|LinearLayout za pomocą dwóch TextViews i ImageView|DataTemplate z siatką zawierającą obraz i dwie obiekty TextBlock|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|Widok|DataTemplate z ContentPresenter.|

## <a name="summary"></a>Podsumowanie

W tym artykule ma na liście programu renderującego i klasy natywne kontrolki, które implementują strony, układ, widok i komórki każdego zestawu narzędzi Xamarin.Forms. Każdy formant zestawu narzędzi Xamarin.Forms ma towarzyszący modułu renderowania dla każdej platformy, która tworzy wystąpienie kontrolki natywne.

## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe programy renderujące (Xamarin University wideo)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
