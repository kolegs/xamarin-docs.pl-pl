---
title: Widoki platformy Xamarin.Forms
description: "Widoki platformy Xamarin.Forms są blokami konstrukcyjnymi interfejsów i platform przenośnych użytkownika."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: ef4de2d544f3bcfb661b29dd90de738ae0442373
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2018
---
# <a name="xamarinforms-views"></a>Widoki platformy Xamarin.Forms

_Widoki platformy Xamarin.Forms są blokami konstrukcyjnymi interfejsów i platform przenośnych użytkownika._

Widoki są obiekty interfejsu użytkownika, takie jak etykiety, przycisków i suwaki, które są powszechnie znane jako *formanty* lub *elementy widget* w innych środowiskach programowania graficznego. Widoki obsługiwane przez wszystkie pochodzi od platformy Xamarin.Forms [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) klasy. Mogą one podzielone na kilka kategorii:

## <a name="views-for-presentation"></a>Widoki prezentacji

### <a name="label"></a>Etykieta

|     |     |
| --- | --- |
| [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Wyświetla ciągi tekstowe w jeden wiersz lub bloków wiele wierszy tekstu, za pomocą stałych czy zmiennych formatowania. Ustaw [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) ciąg stałej formatowanie lub ustaw dla właściwości [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) właściwości [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/) obiektu dla zmiennej formatowanie.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) / [przewodnik](~/xamarin-forms/user-interface/text/label.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![Przykład etykiety](views-images/Label.png "etykietę przykład")](views-images/Label-Large.png#lightbox "etykietę przykład")<br /> [Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Obraz

|     |     |
| --- | --- |
| [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Wyświetla mapy bitowej. Mapy bitowe, można pobrać za pośrednictwem sieci Web, osadzony jako zasoby w typowych projektu lub projektów platformy lub utworzone za pomocą .NET `Stream` obiektu.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) / [przewodnik](~/xamarin-forms/user-interface/images.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![Przykład obrazu](views-images/Image.png "obrazu przykład")](views-images/Image-Large.png#lightbox "przykład obrazu")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) Wyświetla wypełnionego prostokąta kolorowane w [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) właściwości. `BoxView` ma domyślne żądanie rozmiar 40 x 40. O innych rozmiarach przypisać [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) i [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) właściwości.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) / [przewodnik](~/xamarin-forms/user-interface/boxview.md) / [próbkowania 1](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView), [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration), [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife), [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), i [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![Przykład BoxView](views-images/BoxView.png "przykład BoxView")](views-images/BoxView-Large.png#lightbox "BoxView przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>Widok sieci Web

|     |     |
| --- | --- |
| [`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) Wyświetla strony sieci Web lub HTML zawartości, na podstawie [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) właściwość jest ustawiona na [ `UriWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UrlWebViewSource/) lub [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/) obiektu.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) / [przewodnik](~/xamarin-forms/user-interface/webview.md) / [próbkowania 1](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) i [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![Przykład WebView](views-images/WebView.png "przykład WebView")](views-images/WebView-Large.png#lightbox "przykład widoku sieci Web")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/) Wyświetla grafiki OpenGL projektów dla systemu Android i iOS. Nie jest obsługiwane dla platformy uniwersalnej systemu Windows. IOS i Android projektów wymagają odwołania do **OpenTK 1.0** zestawu lub **OpenTK** zestaw w wersji 1.0.0.0. `OpenGLView` ułatwia stosowanie w projekcie udostępnionym; Jeśli używane w bibliotece PCL lub .NET Standard, Usługa zależności również będzie wymagane (jak pokazano w przykładowym kodzie).<br /><br />Jest to funkcji tylko grafiki, który jest wbudowanych w platformy Xamarin.Forms, ale aplikacji platformy Xamarin.Forms można również renderowania grafiki przy użyciu [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md), [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), lub [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/)<br /><br /> | [![Przykład OpenGLView](views-images/OpenGLView.png "przykład OpenGLView")](views-images/OpenGLView-Large.png#lightbox "OpenGLView przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) z [związane z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>mapy

|     |     |
| --- | --- |
| [`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) Wyświetla mapy. **Xamarin.Forms.Maps** musi być zainstalowany pakiet Nuget. Android i platformy uniwersalnej systemu Windows wymagają klucza autoryzacji mapy.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) / [przewodnik](~/xamarin-forms/user-interface/map.md) / [próbki](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![Mapowanie przykład](views-images/Map.png "mapy przykład")](views-images/Map-Large.png#lightbox "mapy przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Widoki, które inicjują poleceń

### <a name="button"></a>Przycisk

|     |     |
| --- | --- |
| [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) jest prostokątny obiekt, który wyświetla tekst, i który uruchamiany [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) zdarzenie, gdy jest został naciśnięty.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) | [![Przykład przycisku](views-images/Button.png "przycisk przykład")](views-images/Button-Large.png#lightbox "przycisk przykład")<br /> [Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) z [związane z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Wyświetla obszar dla użytkownika do typu ciąg tekstowy, a przycisk (lub klawisz), który zasygnalizuje aplikacji, aby rozpocząć wyszukiwanie. [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.Text/) Właściwości zapewnia dostęp do tekstu, a [ `SearchButtonPressed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/) zdarzenie wskazuje, czy przycisk jest naciśnięty.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) | [![Przykład SearchBar](views-images/SearchBar.png "przykład SearchBar")](views-images/SearchBar-Large.png#lightbox "SearchBar przykład")<br /> [Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) z [związane z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Widoki do ustawiania wartości 

### <a name="slider"></a>Suwak

|     |     |
| --- | --- |
| [`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) Umożliwia użytkownikowi wybranie `double` wartość z zakresu ciągłego określony za pomocą [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) i [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) właściwości.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) | [![Przykład suwaka](views-images/Slider.png "przykład suwaka")](views-images/Slider-Large.png#lightbox "przykład suwaka")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Stepper

|     |     |
| --- | --- |
| [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) Umożliwia użytkownikowi wybranie `double` wartość z zakresu wartości przyrostowe określony za pomocą [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Minimum/), [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Maximum/), i [ `Increment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) właściwości.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) | [![Przykład stepper](views-images/Stepper.png "przykład Stepper")](views-images/Stepper-Large.png#lightbox "przykład Stepper")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Przełącznik 

|     |     |
| --- | --- |
| [`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) ma postać przełącznik wł. / wył. umożliwia użytkownikowi wybranie wartość logiczną. [ `IsToggled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) Właściwości jest w stanie przełącznika i [ `Toggled` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) zdarzenie jest wywoływane po zmianie stanu.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) | [![Przełącz przykład](views-images/Switch.png "przełącznika przykład")](views-images/Stepper-Large.png#lightbox "przełącznika przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) Zezwala użytkownikowi na wybranie daty z wyboru daty platformy. Ustaw zakres dat dopuszczalny z [ `MinimumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) i [ `MaximumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) właściwości. [ `Date` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) Właściwość jest wybrana data i [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) zdarzenie jest wywoływane po zmianie tej właściwości.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) / [przewodnik](~/xamarin-forms/user-interface/datepicker.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![Przykład selektora daty](views-images/DatePicker.png "przykład selektora daty")](views-images/DatePicker-Large.png#lightbox "przykład selektora daty")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) Umożliwia użytkownikowi wybranie raz z selektora czasu platformy. [ `Time` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) Właściwość jest wybrana wartość czasu. Aplikację można monitorować zmian w `Time` właściwości, instalując program obsługi [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) zdarzeń.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) | [![Przykład TimePicker](views-images/TimePicker.png "przykład TimePicker")](views-images/TimePicker-Large.png#lightbox "TimePicker przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Widoki do edycji tekstu

Te dwie klasy pochodzi od [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) klasy, która definiuje [ `Keyboard` ](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) właściwości.

<a name="entry" />

### <a name="entry"></a>Wpis

|     |     |
| --- | --- |
| [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Zezwala użytkownikowi na wprowadzanie i edytowanie pojedynczy wiersz tekstu. Tekst jest dostępna jako [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) właściwości oraz [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) i [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) zdarzenia są wywoływane, gdy tekst zostanie zmieniony lub użytkownik sygnalizuje ukończenia, naciskając klawisz enter.<br /><br />Użyj [ `Editor` ](#editor) dla wprowadzanie i edycję wiele wierszy tekstu.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) / [przewodnik](~/xamarin-forms/user-interface/text/entry.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Przykładowy wpis](views-images/Entry.png "przykładowy wpis")](views-images/Entry-Large.png#lightbox "przykładowy wpis")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>Edytor

|     |     |
| --- | --- |
| [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) Zezwala użytkownikowi na wprowadzanie i edytowanie wielu wierszy tekstu. Tekst jest dostępna jako [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Editor.Text/) właściwości oraz [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) i [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) zdarzenia są wywoływane, gdy tekst zostanie zmieniony lub użytkownik sygnalizuje ukończenia.<br /><br />Użyj [ `Entry` ](#entry) widok wprowadzanie i edycję pojedynczy wiersz tekstu.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) / [przewodnik](~/xamarin-forms/user-interface/text/editor.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Przykładowy wpis](views-images/Editor.png "przykład edytor")](views-images/Editor-Large.png#lightbox "przykład edytora")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Widoki, aby wskazać działania

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) używa animacji, aby pokazać, czy aplikacja jest zaangażowane w działaniu, długotrwałą bez wskazania postępu. [ `IsRunning` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) Właściwość kontroluje animacji.<br /><br />Jeśli postęp działania jest znana, użyj [ `ProgressBar` ](#progressbar) zamiast tego.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) | [![Przykład ActivityIndicator](views-images/ActivityIndicator.png "przykład ActivityIndicator")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) używa animacji, aby pokazać, że aplikacji jest wykonywane za pośrednictwem długich działania. Ustaw [ `Progress` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ProgressBar.Progress/) właściwości do wartości od 0 do 1 do informowania o postępie.<br /><br />Jeśli postęp działania nie jest znana, użyj [ `ActivityIndicator` ](#activityindicator) zamiast tego.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) | [![Przykład ProgressBar](views-images/ProgressBar.png "przykład ProgressBar")](views-images/ProgressBar-Large.png#lightbox "przykład ProgressBar")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) z [związane z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Widoki, które są wyświetlane kolekcje

### <a name="picker"></a>Selektor

|     |     |
| --- | --- |
| [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Wyświetla wybrany element z listy ciągów tekstowych i umożliwia wybranie tego elementu w widoku jest wybrany. Ustaw [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) właściwość do listy ciągów, lub [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) właściwości do kolekcji obiektów. [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) Zdarzenie jest wywoływane, gdy element jest zaznaczony.<br /><br />`Picker` Wyświetla listę elementów tylko wtedy, gdy jest zaznaczone. Użyj [ `ListView` ](#listView) lub [ `TableView` ](#tableView) przewijanej listy, który pozostaje na stronie.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) / [przewodnik](~/xamarin-forms/user-interface/picker/index.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![Przykład selektora](views-images/Picker.png "przykład selektora")](views-images/Picker-Large.png#lightbox "przykład selektora")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) z [związane z kodem](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) pochodną [ `ItemsView[Cell]` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) i zawiera listę elementów wybieranych danych. Ustaw [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) kolekcję obiektów i set dla właściwości [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) właściwości [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Obiekt opisujący sposób elementy będą Aby sformatować dysk twardy. [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) Zdarzenie sygnalizuje, że zaznaczenie zostały wprowadzone, który jest dostępny jako [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) właściwości.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) / [przewodnik](~/xamarin-forms/user-interface/listview/index.md) / [próbki](https://developer.xamarin.com/samples/WorkingWithListview) | [![Przykład ListView](views-images/ListView.png "przykład ListView")](views-images/ListView-Large.png#lightbox "przykład ListView")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) Wyświetla listę wierszy typu [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) subheaders i opcjonalne nagłówki. Ustaw [ `Root` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) dla właściwości typu obiektu [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/)i Dodaj [ `TableSection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) obiektów w tym `TableRoot`. Każdy `TableSection` jest kolekcją `Cell` obiektów.<br /><br />[Dokumentacja interfejsu API](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) / [przewodnik](~/xamarin-forms/user-interface/tableview.md) / [próbki](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![Przykład TableView](views-images/TableView.png "przykład TableView")](views-images/TableView-Large.png#lightbox "TableView przykład")<br />[Kod C# dla tej strony](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [strony XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Przykładowe FormsGallery platformy Xamarin.Forms](https://developer.xamarin.com/samples/FormsGallery/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
