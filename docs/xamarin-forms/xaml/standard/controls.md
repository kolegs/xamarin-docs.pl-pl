---
title: Formanty standardowe (wersja zapoznawcza) XAML
description: Ten artykuł opisuje formantów standardowych XAML dostępnych w platformy Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 1b01d0773f0c2150db575875b770957eb6452f41
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245575"
---
# <a name="xaml-standard-preview-controls"></a>Formanty standardowe (wersja zapoznawcza) XAML

![Wersja zapoznawcza](~/media/shared/preview.png)

Ta strona zawiera listę XAML standardowych kontrolek dostępnych w wersji zapoznawczej wraz z ich równoważne kontroli platformy Xamarin.Forms.

Istnieje również listę formantów, które mają nowych nazw właściwości i wyliczenia w XAML Standard.

## <a name="controls"></a>Formanty

|Xamarin.Forms|Standard języka XAML|
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

|Formanty platformy Xamarin.Forms z właściwościami zaktualizowane|Właściwości platformy Xamarin.Forms lub wyliczenia|Odpowiednik standardowe XAML|
|--- |--- |--- |
|Przycisk, zapis, etykiety, selektora daty, edytor, SearchBar, TimePicker|TextColor|Pierwszy plan|
|VisualElement|BackgroundColor|Tło *|
|Selektor, przycisk|BorderColor, OutlineColor|BorderBrush|
|Przycisk|BorderWidth|BorderThickness|
|ProgressBar|Postęp|Wartość|
|Przycisk, wpis, etykiety, edytor, SearchBar, zakres, czcionka|FontAttributesBold, kursywa, Brak|FontStyleItalic, normalny|
|Przycisk, wpis, etykiety, edytor, SearchBar, zakres, czcionka|FontAttributes|FontWeights * Bold normalny|
|InputView|KeyboardDefault, adres Url, numer telefonu, tekst, rozmowy, poczty E-mail|InputScopeNameValue * domyślny adres Url, numer, TelephoneNumber, tekst, rozmowy, EmailNameOrAddress|
|StackPanel|StackOrientation|Orientacja *|

> [!IMPORTANT]
> Elementy oznaczone znakiem * są niekompletne w bieżącej wersji zapoznawczej

## <a name="related-links"></a>Linki pokrewne

- [NuGet w wersji zapoznawczej](https://aka.ms/xf-xamlstandard-nuget)
