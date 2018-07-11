---
title: Formanty standardowe (wersja zapoznawcza) XAML
description: W tym artykule przeanalizowano kontrolek standardowych XAML dostępnych w interfejsie Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 1b01d0773f0c2150db575875b770957eb6452f41
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38843946"
---
# <a name="xaml-standard-preview-controls"></a>Formanty standardowe (wersja zapoznawcza) XAML

![Wersja zapoznawcza](~/media/shared/preview.png)

Ta strona zawiera listę kontrolek XAML Standard, dostępna w wersji zapoznawczej, wraz z ich równoważne kontrolki zestawu narzędzi Xamarin.Forms.

Jest również lista elementów sterujących, które mają nowe nazwy właściwości i wyliczenia w XAML Standard.

## <a name="controls"></a>Formanty

|Xamarin.Forms|Standardowa XAML|
|--- |--- |
|Klatka|Obramowanie|
|Selektor|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|Etykieta|TextBlock|
|Wpis|TextBox|
|Przełącznik|ToggleSwitch|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>Właściwości i wyliczenia

|Kontrolki zestawu narzędzi Xamarin.Forms przy użyciu zaktualizowanych właściwości|Właściwości zestawu narzędzi Xamarin.Forms lub typu wyliczeniowego|Odpowiednik XAML Standard|
|--- |--- |--- |
|Przycisk, zapis, etykiety, DatePicker, edytor, SearchBar, TimePicker|TextColor|Pierwszy plan|
|VisualElement|BackgroundColor|Tło *|
|Selektor, przycisk|BorderColor, OutlineColor|BorderBrush|
|Przycisk|BorderWidth|BorderThickness|
|ProgressBar|Postęp|Wartość|
|Przycisk, wpis, etykiety, edytor, SearchBar, zakres, czcionka|FontAttributesBold, kursywy, Brak|FontStyleItalic, normalny|
|Przycisk, wpis, etykiety, edytor, SearchBar, zakres, czcionka|FontAttributes|FontWeights * Bold normalny|
|InputView|KeyboardDefault, adres Url, numer telefonu, tekst, rozmowy, Wyślij wiadomość E-mail|InputScopeNameValue * domyślne, adres Url, liczby, TelephoneNumber, tekst, rozmowy, EmailNameOrAddress|
|StackPanel|StackOrientation|Orientacja *|

> [!IMPORTANT]
> Elementy oznaczone znakiem * są niekompletne w bieżącej wersji zapoznawczej

## <a name="related-links"></a>Linki pokrewne

- [NuGet w wersji zapoznawczej](https://aka.ms/xf-xamlstandard-nuget)
