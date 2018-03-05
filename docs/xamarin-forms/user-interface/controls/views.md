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
ms.openlocfilehash: df8c8463b2556035c5369c70cb10dbc3dc6b6743
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-views"></a>Widoki platformy Xamarin.Forms

_Widoki platformy Xamarin.Forms są blokami konstrukcyjnymi interfejsów i platform przenośnych użytkownika._

<style>.tableimg {max-width: Brak! ważne;}</style>

## <a name="views"></a>Widoki

Wyraz korzysta z platformy Xamarin.Forms *widoku* do odwoływania się do visual obiekty, takie jak przyciski etykiety i pola wejścia tekstu — które mogą być bardziej powszechnie znane jako formanty elementy widget.

Zazwyczaj są to następujące elementy interfejsu użytkownika są podklasami [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).

<br clear="right" />

Obsługuje platformy Xamarin.Forms:

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Typ</strong>
    </th>
    <th>
      <strong>Opis elementu</strong>
    </th>
    <th style="min-width:400px">
      <strong>Zrzut ekranu</strong>
    </th>

  </thead>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a>
    </td>
    <td valign="top">
Formant visual służy do wskazania, że coś jest wykonywana. Ten formant umożliwia wizualny sygnał do użytkownika, który jest coś przeprowadzana, bez informacji o postępie.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ActivityIndicatorDemoPage.cs"><img src="views-images/ActivityIndicator.png" title="Przykład ActivityIndicator" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widok</a> używany do rysowania wypełnionego prostokąta kolorowe. BoxView jest przydatne podstawiony dla obrazów lub elementy niestandardowe podczas prototypowania początkowej. BoxView ma domyślne żądanie rozmiar 40 x 40. Jeśli potrzebujesz zmienić rozmiar, Przypisz <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/">VisualElement.WidthRequest</a> i <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/">VisualElement.HeightRequest</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/BoxViewDemoPage.cs"><img src="views-images/BoxView.png" title="Przykład BoxView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">Przycisk</a>
    </td>
    <td align="center" valign="top">
Przycisk <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> który reaguje na zdarzenia touch.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ButtonDemoPage.cs"><img src="views-images/Button.png" title="Przykład przycisku" class="tableimg">
    </a></td>
  </tr>
  <tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">Selektora daty</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> , która umożliwia pobieranie daty. Wizualną reprezentację selektora daty jest bardzo podobny do jednej z <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">wpis</a>, z wyjątkiem, że formant specjalne do pobrania Data pojawia się klawiatury </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/DatePickerDemoPage.cs"><img src="views-images/DatePicker.png" title="Przykład selektora daty" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Edytor</a>
    </td>
    <td valign="top">
Formant, który można edytować wiele wierszy tekstu. Wpisy w jednym wierszu, zobacz <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">wpis</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EditorDemoPage.cs"><img src="views-images/Editor.png" title="Przykład edytora" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a>
    </td>
    <td valign="top">
Formant, który można edytować pojedynczy wiersz tekstu. Wpis jest wpisem pojedynczy wiersz tekstu. Najlepiej służy do zbierania małych fragmentów odrębny informacje, takie jak nazwy użytkowników i hasła.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="views-images/Entry.png" title="Przykładowy wpis" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Obraz</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> zawierającego obraz.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Obraz interfejsu API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/images.md">Praca z obrazami</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageDemoPage.cs">Wersja demonstracyjna dla źródła</a>
    </td>
    <td>
    <img src="views-images/Image.png" title="Przykład obrazu" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">Label</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> wyświetlającym tekst w formacie tylko odczytu. Etykieta jest używana do wyświetlania elementów jednego wiersza tekstu, a także bloków wielu wierszy tekstu.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/LabelDemoPage.cs"><img src="views-images/Label.png" title="Przykład etykiety" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">Element ListView</a>
    </td>
    <td valign="top">
<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/">PrzedmiotWyświetl</a> który będzie wyświetlany jako pionowy listy zbierania danych.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">Element ListView interfejsu API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/listview/index.md">Dokumentacja elementu ListView</a>
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ListViewDemoPage.cs"><img src="views-images/ListView.png" title="ListView — przykład" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/">OpenGLView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> wyświetlający OpenGL zawartości.
    <ul>
      <li>Działa tylko dla systemów iOS i Android projektów (bez obsługi Windows Phone).
      <li>Wymaga odwołania do <b>OpenTK 1.0</b> zestawu w projektów dla systemu Android i iOS.</li>
      <li>Najlepiej nadaje się do użycia w udostępnionych projektów; Jeśli używana w PCL DependencyService również będzie wymagane.</li>
    </ul>
    </td>
    <td>
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/"><img src="views-images/OpenGL.png" title="Przykład OpenGlView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">Selektor</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> formantu do pobrania elementu na liście. Wizualną reprezentację selektora jest podobny do <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">wpis</a>, ale formant wyboru zamiast klawiatury.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/PickerDemoPage.cs"><img src="views-images/Picker.png" title="Przykład selektora" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> formantu postępu wskazujące.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ProgressBarDemoPage.cs"><img src="views-images/ProgressBar.png" title="Klasa przykład ProgressBar ="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> formant, który zawiera pole wyszukiwania.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SearchBarDemoPage.cs"><img src="views-images/SearchBar.png" title="Przykład SearchBar" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Suwak</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> formant, który danych wejściowych liniowej wartość.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SliderDemoPage.cs"><img src="views-images/Slider.png" title="Przykład suwaka" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Stepper</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> formant, który danych wejściowych wartość discrete ograniczone do zakresu.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StepperDemoPage.cs"><img src="views-images/Stepper.png" title="Przykład stepper" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">Przełącznik</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> formant, który udostępnia przełączony wartość.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchDemoPage.cs"><img src="views-images/Switch.png" title="Przykład przełącznika" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> przechowuje wiersze <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">komórki</a>s.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView interfejsu API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/tableview.md">Dokumentacja TableView</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TableViewFormDemoPage.cs">Wersja demonstracyjna dla źródła</a>
    </td>
    <td>
    <img src="views-images/TableViewNewest.png" title="Przykład TableView" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> formant, który zawiera czas pobrania. Wizualną reprezentację TimePicker jest bardzo podobny do jednej z <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">wpis</a>, ale pojawia się specjalne kontrolkę do pobrania przez czas klawiatury.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TimePickerDemoPage.cs"><img src="views-images/TimePicker.png" title="Przykład TimePicker" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">Widok sieci Web</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">widoku</a> przedstawiający zawartość HTML.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">Widok sieci Web interfejsu API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/webview.md">Dokumentacja widoku sieci Web</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/WebViewDemoPage.cs">Wersja demonstracyjna dla źródła</a>
    </td>
    <td>
    <img src="views-images/WebView.png" title="Przykładowy widok sieci Web" class="tableimg">
    </td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Galeria platformy Xamarin.Forms (przykład)](https://developer.xamarin.com/samples/FormsGallery/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Dokumentacja interfejsu API platformy Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
