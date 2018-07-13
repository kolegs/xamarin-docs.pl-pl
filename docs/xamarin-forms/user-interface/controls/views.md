---
title: Widoki zestawu narzędzi Xamarin.Forms
description: Zestaw narzędzi Xamarin.Forms widoki są blokami konstrukcyjnymi interfejsów użytkownika mobilnego dla wielu platform. W tym artykule wymieniono widoki, które znajdują się w interfejsie Xamarin.Forms.
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 52d8d5f6eb38e5cb501d6284d08f7317981e0dcf
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998976"
---
# <a name="xamarinforms-views"></a>Widoki zestawu narzędzi Xamarin.Forms

_Zestaw narzędzi Xamarin.Forms widoki są blokami konstrukcyjnymi interfejsów użytkownika mobilnego dla wielu platform._

Widoki są obiekty interfejsu użytkownika, takie jak etykiety, przyciski i suwaki, które są powszechnie znane jako *kontrolki* lub *elementy widget* w innych środowiskach programowania graficznego. Widoki obsługiwane przez wszystkie pochodzą z zestawu narzędzi Xamarin.Forms [ `View` ](xref:Xamarin.Forms.View) klasy. Mogą one podzielone na kilka kategorii:

## <a name="views-for-presentation"></a>Widoki, aby obejrzeć prezentację

### <a name="label"></a>Etykieta

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label) Wyświetla ciągi tekstowe w jeden wiersz lub wielowierszowe bloki tekstu, albo za pomocą formatowania stałą lub zmienną. Ustaw [ `Text` ](xref:Xamarin.Forms.Label.Text) właściwości ciągu dla stałej formatowania lub zestaw [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) właściwości [ `FormattedString` ](xref:Xamarin.Forms.FormattedString) obiektu w zmiennej formatowanie.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.Label) / [przewodnik](~/xamarin-forms/user-interface/text/label.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![Przykład etykiety](views-images/Label.png "etykiety przykład")](views-images/Label-Large.png#lightbox "przykład etykiety")<br /> [Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Obraz

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image) wyświetlać mapę bitową. Mapy bitowe, które można pobrać za pośrednictwem sieci Web, osadzane jako zasoby w typowych projekt lub projekty platformy lub utworzone przy użyciu platformy .NET `Stream` obiektu.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.Image) / [przewodnik](~/xamarin-forms/user-interface/images.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![Obraz przykładu](views-images/Image.png "obrazu przykład")](views-images/Image-Large.png#lightbox "obraz przykładu")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) Wyświetla wypełniony prostokąt w kolorze kolorowane w [ `Color` ](xref:Xamarin.Forms.BoxView.Color) właściwości. `BoxView` zawiera domyślne żądanie rozmiar, 40 x 40. Inne rozmiary, Przypisz [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) i [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) właściwości.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.BoxView) / [przewodnik](~/xamarin-forms/user-interface/boxview.md) / [przykładowe 1](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView), [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration), [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife), [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), i [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![Przykład BoxView](views-images/BoxView.png "przykład BoxView")](views-images/BoxView-Large.png#lightbox "przykład BoxView")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>Widok sieci Web

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) Wyświetla stron sieci Web lub zawartość HTML na ich podstawie [ `Source` ](xref:Xamarin.Forms.WebView.Source) właściwość jest ustawiona na [ `UriWebViewSource` ](xref:Xamarin.Forms.UrlWebViewSource) lub [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource) obiektu.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.WebView) / [przewodnik](~/xamarin-forms/user-interface/webview.md) / [przykładowe 1](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) i [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![Przykład WebView](views-images/WebView.png "przykład WebView")](views-images/WebView-Large.png#lightbox "przykład WebView")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView) Wyświetla grafiki OpenGL dla urządzeń z systemem iOS i Android projektów. Nie jest obsługiwane dla platformy uniwersalnej Windows. Projektów dla systemu Android i iOS wymagają odwołania do **OpenTK 1.0** zestawu lub **OpenTK** zestawu w wersji 1.0.0.0. `OpenGLView` łatwiej jest używać projektu udostępnionego; Jeśli używany w biblioteki .NET Standard, Usługa zależności również będzie wymagany (jak pokazano w przykładowym kodzie).<br /><br />Jest to funkcji tylko grafiki, która jest wbudowana w Xamarin.Forms, ale aplikacji platformy Xamarin.Forms umożliwia również Renderowanie grafiki przy użyciu [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md), [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), lub [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![Przykład OpenGLView](views-images/OpenGLView.png "przykład OpenGLView")](views-images/OpenGLView-Large.png#lightbox "przykład OpenGLView")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) z [związanym z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>Mapy

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map) Wyświetla mapę. **Projekt xamarin.Forms.Maps dla** musi być zainstalowany pakiet Nuget. Dla systemu android i platformy uniwersalnej Windows wymagają klucza autoryzacji mapy.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.Maps.Map) / [przewodnik](~/xamarin-forms/user-interface/map.md) / [próbki](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![Mapowanie przykład](views-images/Map.png "mapy przykład")](views-images/Map-Large.png#lightbox "mapy przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Widoki, które inicjują poleceń

### <a name="button"></a>Przycisk

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button) jest prostokątnym zestawem obiekt, który wyświetla tekst, i który będzie uruchamiany [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) zdarzenie, kiedy wciśnięto zostały.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.Button) / [przewodnik](~/xamarin-forms/user-interface/button.md) / [próbki](https://developer.xamarin.com/samples/UserInterface/ButtonDemos/) | [![Przykład przycisku](views-images/Button.png "przycisk przykład")](views-images/Button-Large.png#lightbox "przycisk przykład")<br /> [Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) z [związanym z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar) Wyświetla obszar dla użytkownika do typu ciąg tekstowy i przycisk (lub klawisz), który sygnały aplikację, aby wykonać wyszukiwanie. [ `Text` ](xref:Xamarin.Forms.SearchBar.Text) Właściwości zapewnia dostęp do tekstu, a [ `SearchButtonPressed` ](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) zdarzenie oznacza, że przycisk został naciśnięty.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.SearchBar) | [![Przykład SearchBar](views-images/SearchBar.png "przykład SearchBar")](views-images/SearchBar-Large.png#lightbox "przykład SearchBar")<br /> [Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) z [związanym z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Widoki do ustawiania wartości

### <a name="slider"></a>Suwak

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider) Zezwala użytkownikowi na wybranie `double` wartość z zakresu ciągłym, określić za pomocą [ `Minimum` ](xref:Xamarin.Forms.Slider.Minimum) i [ `Maximum` ](xref:Xamarin.Forms.Slider.Maximum) właściwości.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.Slider) / [przewodnik](~/xamarin-forms/user-interface/slider.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) | [![Przykład suwaka](views-images/Slider.png "przykład suwaka")](views-images/Slider-Large.png#lightbox "przykład suwaka")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Stepper

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper) Zezwala użytkownikowi na wybranie `double` wartość z zakresu wartości przyrostowe określony za pomocą [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum), [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum), i [ `Increment` ](xref:Xamarin.Forms.Stepper.Increment) właściwości.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.Stepper) | [![Przykład stepper](views-images/Stepper.png "przykład Stepper")](views-images/Stepper-Large.png#lightbox "przykład Stepper")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Przełącznik

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch) ma postać przełącznika wł. / wył. Aby zezwolić użytkownikowi na wybranie typu wartość logiczna. [ `IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled) Właściwość jest stan przełącznika, a [ `Toggled` ](xref:Xamarin.Forms.Switch.Toggled) zdarzenie jest wywoływane po zmianie stanu.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.Switch) | [![Przełącz przykład](views-images/Switch.png "Przełącz przykład")](views-images/Switch-Large.png#lightbox "Przełącz przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker) Umożliwia użytkownikowi wybranie daty przy użyciu platformy selektora daty. Ustaw zakres dopuszczalnych dat za pomocą [ `MinimumDate` ](xref:Xamarin.Forms.DatePicker.MinimumDate) i [ `MaximumDate` ](xref:Xamarin.Forms.DatePicker.MaximumDate) właściwości. [ `Date` ](xref:Xamarin.Forms.DatePicker.Date) Właściwość jest wybranej daty i [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) zdarzenie jest wywoływane po zmianie właściwości.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.DatePicker) / [przewodnik](~/xamarin-forms/user-interface/datepicker.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![Przykład selektora](views-images/DatePicker.png "przykład DatePicker")](views-images/DatePicker-Large.png#lightbox "DatePicker — przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker) Zezwala użytkownikowi na wybranie czasu za pomocą selektora czasu platformy. [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) Właściwość jest wybrana wartość czasu. Aplikację można monitorować zmiany w `Time` właściwości, instalując program obsługi [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) zdarzeń.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.TimePicker) | [![Przykład TimePicker](views-images/TimePicker.png "przykład TimePicker")](views-images/TimePicker-Large.png#lightbox "przykład TimePicker")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Widoki do edycji tekstu

Te dwie klasy pochodzić od [ `InputView` ](xref:Xamarin.Forms.InputView) klasy, która definiuje [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) właściwości.

<a name="entry" />

### <a name="entry"></a>Wpis

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry) Umożliwia użytkownikowi wprowadzanie i edytowanie pojedynczy wiersz tekstu. Tekst jest dostępna jako [ `Text` ](xref:Xamarin.Forms.Entry.Text) właściwości i [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) i [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) zdarzenia są uruchamiane podczas zmiany tekstu lub użytkownika sygnalizuje zakończenie, naciskając klawisz enter.<br /><br />Użyj [ `Editor` ](#editor) dla wprowadzanie i edycję wiele wierszy tekstu.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.Entry) / [przewodnik](~/xamarin-forms/user-interface/text/entry.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Przykładowy wpis](views-images/Entry.png "przykładowy wpis")](views-images/Entry-Large.png#lightbox "przykładowy wpis")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>Edytor

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor) Umożliwia użytkownikowi wprowadzanie i edytowanie wiele wierszy tekstu. Tekst jest dostępna jako [ `Text` ](xref:Xamarin.Forms.Editor.Text) właściwości i [ `TextChanged` ](xref:Xamarin.Forms.Editor.TextChanged) i [ `Completed` ](xref:Xamarin.Forms.Editor.Completed) zdarzenia są uruchamiane podczas zmiany tekstu lub użytkownika sygnalizuje zakończenie.<br /><br />Użyj [ `Entry` ](#entry) widok wprowadzanie i edycję pojedynczy wiersz tekstu.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.Editor) / [przewodnik](~/xamarin-forms/user-interface/text/editor.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Przykładowy wpis](views-images/Editor.png "przykład edytora")](views-images/Editor-Large.png#lightbox "przykład edytora")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Widoki, aby wskazać działania

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) używa animacji, aby pokazać, że aplikacja jest zaangażowana w długich działania bez żadnego wskazania dotyczącego postępu. [ `IsRunning` ](xref:Xamarin.Forms.ActivityIndicator.IsRunning) Właściwość kontroluje animacji.<br /><br />Jeśli postęp działania jest znana, użyj [ `ProgressBar` ](#progressbar) zamiast tego.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.ActivityIndicator) | [![Przykład ActivityIndicator](views-images/ActivityIndicator.png "przykład ActivityIndicator")](views-images/ActivityIndicator-Large.png#lightbox "przykład ActivityIndicator")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) używa animacji, aby pokazać, czy aplikacja postępuje za pośrednictwem długich działania. Ustaw [ `Progress` ](xref:Xamarin.Forms.ProgressBar.Progress) właściwości do wartości z zakresu od 0 do 1 do informowania o postępie.<br /><br />Jeśli postęp działania nie jest znana, użyj [ `ActivityIndicator` ](#activityindicator) zamiast tego.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.ProgressBar) | [![Przykład elementu ProgressBar](views-images/ProgressBar.png "przykład ProgressBar")](views-images/ProgressBar-Large.png#lightbox "przykład ProgressBar")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) z [związanym z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Widoki, które są wyświetlane kolekcje

### <a name="picker"></a>Selektor

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker) Wyświetla wybrany element z listy ciągów tekstowych i umożliwia wybranie ten element jest wybrany widok. Ustaw [ `Items` ](xref:Xamarin.Forms.Picker.Items) właściwości listy ciągów, lub [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) właściwości kolekcji obiektów. [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Zdarzenie jest wywoływane, gdy element jest wybrany.<br /><br />`Picker` Wyświetla listę elementów, tylko wtedy, gdy jest wybrana. Użyj [ `ListView` ](#listView) lub [ `TableView` ](#tableView) dla przewijaną listę, która pozostaje na stronie.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.Picker) / [przewodnik](~/xamarin-forms/user-interface/picker/index.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![Przykład selektora](views-images/Picker.png "przykład selektora")](views-images/Picker-Large.png#lightbox "przykład selektora")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) z [związanym z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView) pochodzi od klasy [ `ItemsView[Cell]` ](xref:Xamarin.Forms.ItemsView`1) i zawiera listę elementów danych można wybierać. Ustaw [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) właściwości w kolekcji obiektów, a następnie ustaw [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) właściwości [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Obiekt opisujący, jak elementy są Aby sformatować dysk twardy. [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Zdarzenie sygnalizuje, że zaznaczono tekstu, który jest dostępny jako [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) właściwości.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.ListView) / [przewodnik](~/xamarin-forms/user-interface/listview/index.md) / [próbki](https://developer.xamarin.com/samples/WorkingWithListview) | [![Przykład ListView](views-images/ListView.png "przykład ListView")](views-images/ListView-Large.png#lightbox "przykład ListView")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView) Wyświetla listę wierszy typu [ `Cell` ](xref:Xamarin.Forms.Cell) opcjonalne nagłówki i podnagłówki. Ustaw [ `Root` ](xref:Xamarin.Forms.TableView.Root) właściwość do obiektu typu [ `TableRoot` ](xref:Xamarin.Forms.TableRoot)i Dodaj [ `TableSection` ](xref:Xamarin.Forms.TableSection) obiektów, `TableRoot`. Każdy `TableSection` to zbiór `Cell` obiektów.<br /><br />[Dokumentacja interfejsu API](xref:Xamarin.Forms.TableView) / [przewodnik](~/xamarin-forms/user-interface/tableview.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![Przykład TableView](views-images/TableView.png "przykład TableView")](views-images/TableView-Large.png#lightbox "przykład TableView")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do zestawu narzędzi Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Przykładowe FormsGallery zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/FormsGallery/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
