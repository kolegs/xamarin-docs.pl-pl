---
title: Klasy podstawowe renderowania i kontrolki natywne
description: "Formant co platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu. W tym artykule wymieniono renderowania i klasy macierzystego formantu, które implementują każdej platformy Xamarin.Forms strony, układ widoku i komórki."
ms.topic: article
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 95518d9b23db68cc972549c730deeea968512444
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="renderer-base-classes-and-native-controls"></a>Klasy podstawowe renderowania i kontrolki natywne

_Formant co platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu. W tym artykule wymieniono renderowania i klasy macierzystego formantu, które implementują każdej platformy Xamarin.Forms strony, układ widoku i komórki._

Z wyjątkiem produktów `MapRenderer` klasy renderowania specyficzne dla platformy można znaleźć w następujących przestrzeni nazw:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** — Xamarin.Forms.Platform.Android.AppCompat
- **Windows Phone 8** — Xamarin.Forms.Platform.WinPhone
- **WinRT** – Xamarin.Forms.Platform.WinRT
- **Platforma uniwersalna systemu Windows (UWP)** — Xamarin.Forms.Platform.UWP

`MapRenderer` Klasy można znaleźć w następujących przestrzeni nazw:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Windows Phone 8** – Xamarin.Forms.Maps.WP8
- **WinRT** – Xamarin.Forms.Maps.WinRT
- **Platforma uniwersalna systemu Windows (UWP)** — Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Strony

W poniższej tabeli wymieniono renderowania i macierzystego formantu klasy, które implementują każdej platformy Xamarin.Forms [strony](~/xamarin-forms/user-interface/controls/pages.md) typu:

<table>
 <thead>
   <tr>
     <td><strong>Strona</strong></td>
     <td><strong>Moduł renderowania</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Android (AppCompat)</strong></td>
     <td><strong>Windows Phone 8 < / strong</td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md">PageRenderer</a></td>
     <td>UIViewController</td>
     <td>Grupie widoków</td>
     <td></td>
     <td>Panel</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/">MasterDetailPage</a></td>
     <td><p>PhoneMasterDetailRenderer (iOS — telefon)</p><p>TabletMasterDetailPageRenderer (iOS — Tablet)</p><p>MasterDetailRenderer (Android)</p><p>MasterDetailPageRenderer (AppCompat systemu Android)</p><p>MasterDetailRenderer (Windows Phone)</p><p>MasterDetailPageRenderer (WinRT)</p></td>
     <td><p>UIViewController (telefon)</p><p>UISplitViewController (Tablet)</p></td>
     <td>DrawerLayout (v4)</td>
     <td>DrawerLayout (v4)</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement (formant niestandardowy)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/">NavigationPage</a></td>
     <td><p>NavigationRenderer (z systemem iOS i Android)</p><p>NavigationPageRenderer (AppCompat systemu Android)</p><p>NavigationPageRenderer (Windows Phone 8 i WinRT)</p></td>
     <td>UIToolbar</td>
     <td>Grupie widoków</td>
     <td>Grupie widoków</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement (formant niestandardowy)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/">TabbedPage</a></td>
     <td><p>TabbedRenderer (z systemem iOS i Android)</p><p>TabbedPageRenderer (AppCompat systemu Android)</p><p>TabbedPageRenderer (Windows Phone 8 i WinRT)</p></td>
     <td>UIView</td>
     <td>ViewPager</td>
     <td>ViewPager</td>
     <td>UIElement (Tabela przestawna)</td>
     <td>FrameworkElement (Tabela przestawna)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/">TemplatedPage</a></td>
     <td>PageRenderer</td>
     <td>UIViewController</td>
     <td>Grupie widoków</td>
     <td></td>
     <td>Panel</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/">CarouselPage<a/></td>
     <td>CarouselPageRenderer</td>
     <td>UIScrollView</td>
     <td>ViewPager</td>
     <td>ViewPager</td>
     <td>UIElement (Panorama)</td>
     <td>FrameworkElement (właściwości FlipView)</td>
   </tr>
 </tbody>
</table>

## <a name="layouts"></a>Układy

W poniższej tabeli wymieniono renderowania i macierzystego formantu klasy, które implementują każdej platformy Xamarin.Forms [układu](~/xamarin-forms/user-interface/controls/layouts.md) typu:

<table>
 <thead>
   <tr>
     <td><strong>Układ</strong></td>
     <td><strong>Moduł renderowania</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Widok</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Widok</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">Ramka</a></td>
     <td>FrameRenderer</td>
     <td>UIView</td>
     <td>Grupie widoków</td>
     <td>Obramowanie</td>
     <td>Obramowanie</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a></td>
     <td>ScrollViewRenderer</td>
     <td>UIScrollView</td>
     <td>ScrollView</td>
     <td>ScrollViewer</td>
     <td>ScrollViewer</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Widok</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Widok</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Siatka</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Widok</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Widok</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>Widok</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
 </tbody>
</table>

## <a name="views"></a>Widoki

W poniższej tabeli wymieniono renderowania i macierzystego formantu klasy, które implementują każdej platformy Xamarin.Forms [widoku](~/xamarin-forms/user-interface/controls/views.md) typu:

<table>
 <thead>
   <tr>
     <td><strong>Widoki</strong></td>
     <td><strong>Moduł renderowania</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Android (AppCompat)</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a></td>
     <td>ActivityIndicatorRenderer</td>
     <td>UIActivityIndicator</td>
     <td>ProgressBar</td>
     <td></td>
     <td>ProgressBar</td>
     <td>ProgressBar</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a></td>
     <td><p>BoxRenderer (z systemem iOS i Android)</p><p>BoxViewRenderer (Windows Phone i WinRT)</p></td>
     <td>UIView</td>
     <td>Grupie widoków</td>
     <td></td>
     <td>Prostokąt</td>
     <td>Prostokąt</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">Przycisk</a></td>
     <td>ButtonRenderer</td>
     <td>UIButton</td>
     <td>Przycisk</td>
     <td>AppCompatButton</td>
     <td>Przycisk</td>
     <td>Przycisk</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/">CarouselView</a></td>
     <td>CarouselViewRenderer</td>
     <td>UIScrollView</td>
     <td>RecyclerView</td>
     <td></td>
     <td>Właściwości FlipView</td>
     <td>Właściwości FlipView</td>
   </tr>   
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a></td>
     <td>DatePickerRenderer</td>
     <td>UITextField</td>
     <td>Typu części EditText</td>
     <td></td>
     <td>DatePicker</td>
     <td>DatePicker</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Edytor</a></td>
     <td>EditorRenderer</td>
     <td>UITextView</td>
     <td>Typu części EditText</td>
     <td></td>
     <td>TextBox</td>
     <td>TextBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/entry.md">EntryRenderer</a></td>
     <td>UITextField</td>
     <td>Typu części EditText</td>
     <td></td>
     <td>PhoneTextBox/PasswordBox</td>
     <td>TextBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Obraz</a></td>
     <td>ImageRenderer</td>
     <td>UIImageView</td>
     <td>ImageView</td>
     <td></td>
     <td>Obraz</td>
     <td>Obraz</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">Etykieta</a></td>
     <td>LabelRenderer</td>
     <td>UILabel</td>
     <td>Element TextView</td>
     <td></td>
     <td>TextBlock</td>
     <td>TextBlock</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/listview.md">ListViewRenderer</a></td>
     <td>UITableView</td>
     <td>ListView</td>
     <td></td>
     <td>LongListSelector</td>
     <td>ListView</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/">mapy</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md">MapRenderer</a></td>
     <td>MKMapView</td>
     <td>MapView</td>
     <td></td>
     <td>mapy</td>
     <td>MapControl</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">Selektor</a></td>
     <td>PickerRenderer</td>
     <td>UITextField</td>
     <td>Typu części EditText</td>
     <td>Typu części EditText</td>
     <td>FrameworkElement</td>
     <td>ComboBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a></td>
     <td>ProgressBarRenderer</td>
     <td>UIProgressView</td>
     <td>ProgressBar</td>
     <td></td>
     <td>ProgressBar</td>
     <td>ProgressBar</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a></td>
     <td>SearchBarRenderer</td>
     <td>UISearchBar</td>
     <td>SearchView</td>
     <td></td>
     <td>PhoneTextBox</td>
     <td><p>SearchBox (WinRT)</p><p>AutoSuggestBox (UWP)</p></td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Slider</a></td>
     <td>SliderRenderer</td>
     <td>UISlider</td>
     <td>SeekBar</td>
     <td></td>
     <td>Suwak</td>
     <td>Suwak</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Stepper</a></td>
     <td>StepperRenderer</td>
     <td>UIStepper</td>
     <td>LinearLayout</td>
     <td></td>
     <td>Obramowanie z Panel stosu i dwóch przycisków</td>
     <td><p>UserControl (WinRT)</p><p>Kontrola (UWP)</p></td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">Przełącznik</a></td>
     <td>SwitchRenderer</td>
     <td>UISwitch</td>
     <td>Przełącznik</td>
     <td>SwitchCompat</td>
     <td>Obramowanie z ToggleSwitchButton</td>
     <td>ToggleSwitch</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView</a></td>
     <td>TableViewRenderer</td>
     <td>UITableView</td>
     <td>ListView</td>
     <td></td>
     <td>Siatkę z blokiem tekstu i ListBox</td>
     <td>ListView</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a></td>
     <td>TimePickerRenderer</td>
     <td>UITextField</td>
     <td>Typu części EditText</td>
     <td></td>
     <td>TimePicker</td>
     <td>TimePicker</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">Widok sieci Web</a></td>
     <td>WebViewRenderer</td>
     <td>UIWebView</td>
     <td>Widok sieci Web</td>
     <td></td>
     <td>WebBrowser</td>
     <td>Widok sieci Web</td>
   </tr>
 </tbody>
</table>

## <a name="cells"></a>Komórki

W poniższej tabeli wymieniono renderowania i macierzystego formantu klasy, które implementują każdej platformy Xamarin.Forms [komórki](~/xamarin-forms/user-interface/controls/cells.md) typu:

<table>
 <thead>
   <tr>
     <td><strong>Komórki</strong></td>
     <td><strong>Moduł renderowania</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a></td>
   <td>EntryCellRenderer</td>
   <td>UITableViewCell z UITextField</td>
   <td>LinearLayout TextView i typu części EditText</td>
   <td>DataTemplate z siatką zawierający blok tekstu i PhoneTextBox</td>
   <td>DataTemplate z pola tekstowego</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a></td>
   <td>SwitchCellRenderer</td>
   <td>UITableViewCell z UISwitch</td>
   <td>Przełącznik</td>
   <td>DataTemplate z ToggleSwitch</td>
   <td>DataTemplate z siatką zawierający blok tekstu i ToggleSwitch</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a></td>
   <td>TextCellRenderer</td>
   <td>UITableViewCell</td>
   <td>LinearLayout z dwóch TextViews</td>
   <td>DataTemplate z przyciskiem zawierający Panel stosu z dwa obiekty TextBlock</td>
   <td>DataTemplate z Panel stosu zawierającego dwa obiekty TextBlock</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a></td>
   <td>ImageCellRenderer</td>
   <td>UITableViewCell z UIImage</td>
   <td>LinearLayout z dwóch TextViews i ImageView</td>
   <td>DataTemplate z przyciskiem zawierający siatkę z obrazu i Panel stosu zawierającego dwa obiekty TextBlock</td>
   <td>DataTemplate siatki, zawierający obraz i dwa obiekty TextBlock</td>  
   </td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/">ViewCell</a></td>
   <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md">ViewCellRenderer</a></td>
   <td>UITableViewCell</td>
   <td>Widok</td>
   <td>DataTemplate z ContentPresenter</td>
   <td>DataTemplate z ContentPresenter</td>  
   </td>
 </tr>
 </tbody>
</table>

## <a name="summary"></a>Podsumowanie

W tym artykule zgłosił renderowania i klasy macierzystego formantu, które implementują każdej platformy Xamarin.Forms strony, układ widoku i komórki. Formant co platformy Xamarin.Forms ma towarzyszący renderowania dla każdej platformy, która tworzy wystąpienie macierzystego formantu.



## <a name="related-links"></a>Linki pokrewne

- [Niestandardowe moduły renderowania (Xamarin University wideo)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
