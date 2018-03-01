---
title: Formanty standardowe (wersja zapoznawcza) XAML
description: "Jak rozpocząć eksplorowanie Podgląd standardowe XAML w platformy Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 3f30a77975a9f42380ecf7efd73426763ec83ef0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-standard-preview-controls"></a>Formanty standardowe (wersja zapoznawcza) XAML

![Wersja zapoznawcza](~/media/shared/preview.png)

Ta strona zawiera listę XAML standardowych kontrolek dostępnych w wersji zapoznawczej wraz z ich równoważne kontroli platformy Xamarin.Forms.

Istnieje również listę formantów, które mają nowych nazw właściwości i wyliczenia w XAML Standard.

## <a name="controls"></a>Formanty

<table style="width:300px">
  <tr><th>Xamarin.Forms</th><th>Standard języka XAML</th></tr>
  <tr><td>Klatka</td><td>Obramowanie</td></tr>
  <tr><td>Selektor</td><td>ComboBox</td></tr>
  <tr><td>ActivityIndicator</td><td>ProgressRing</td></tr>
  <tr><td>StackLayout</td><td>StackPanel</td></tr>
  <tr><td>Etykieta</td><td>TextBlock</td></tr>
  <tr><td>Wpis</td><td>TextBox</td></tr>
  <tr><td>Przełącznik</td><td>ToggleSwitch</td></tr>
  <tr><td>ContentView</td><td>UserControl</td></tr>
</table>

## <a name="properties-and-enumerations"></a>Właściwości i wyliczenia

<table>
  <tr><th>Xamarin.Forms<br/>Formanty z właściwościami zaktualizowane</th><th>Xamarin.Forms<br/>Właściwość lub wyliczenia</th><th>Standard języka XAML<br/>Odpowiednik</th></tr>
  <tr><td>Przycisk, zapis, etykiety, selektora daty, edytor, SearchBar, TimePicker</td><td>TextColor</td><td>Pierwszy plan</td></tr>
  <tr><td>VisualElement</td><td>BackgroundColor</td><td><i>Tło *</i></td></tr>
  <tr><td>Selektor, przycisk</td><td>BorderColor, OutlineColor</td><td>BorderBrush</td></tr>
  <tr><td>Przycisk</td><td>BorderWidth</td><td>BorderThickness</td></tr>
  <tr><td>ProgressBar</td><td>Postęp</td><td>Wartość</td></tr>
  <tr><td>Przycisk, wpis, etykiety, edytor, SearchBar, zakres, czcionka</td><td>FontAttributes<br/>Pogrubienie, kursywa, Brak</td><td>FontStyle<br/>Kursywa normalny</td></tr>
  <tr><td>Przycisk, wpis, etykiety, edytor, SearchBar, zakres, czcionka</td><td>FontAttributes</td><td><i>FontWeights *</i><br/>Bold, Normal</td></tr>
  <tr><td>InputView</td><td>Klawiatury<br/>Domyślnie adres Url, numer, telefonów, tekst, rozmowy, poczty E-mail</td><td><i>InputScopeNameValue *</i><br/>Domyślnie adres Url, numer, TelephoneNumber, tekst, rozmowy, EmailNameOrAddress</td></tr>
  <tr><td>StackPanel</td><td>StackOrientation</td><td><i>Orientacja *</i></td></tr>
</table>

> [!IMPORTANT]
> Elementy oznaczone znakiem * są niekompletne w bieżącej wersji zapoznawczej


## <a name="related-links"></a>Linki pokrewne

- [NuGet w wersji zapoznawczej](https://aka.ms/xf-xamlstandard-nuget)
