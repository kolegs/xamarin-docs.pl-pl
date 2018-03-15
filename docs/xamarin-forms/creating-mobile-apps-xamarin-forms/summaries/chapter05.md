---
title: "Podsumowanie rozdziału 5. Zajmujących się rozmiary"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0c61727e90a03d618a7423e5b865a7fcc9e0b399
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>Podsumowanie rozdziału 5. Zajmujących się rozmiary

Do tej pory napotkano kilka rozmiary platformy Xamarin.Forms:

- Wysokość paska stanu systemu iOS jest 20
- `BoxView` Ma domyślnej szerokości i wysokości 40
- Wartość domyślna `Padding` w `Frame` wynosi 20
- Wartość domyślna `Spacing` na `StackLayout` to 6
- `Device.GetNamedSize` Metoda zwraca rozmiar czcionki numeryczne

Rozmiary nie są pikseli. Są to jednostki niezależnych od urządzenia, niezależnie rozpoznał każdej platformy.

## <a name="pixels-points-dps-dips-and-dius"></a>Pikseli, punkty punktu dystrybucji, DIP i DIUs

Na początku historii Mac firmy Apple i Microsoft Windows programistów działał jednostki pikseli. Jednak pojawienie się Wyświetla wyższej rozdzielczości wymagane zwirtualizowanych i bardziej abstrakcyjny podejście na współrzędne ekranu. W świecie Mac programistów działał w jednostkach *punktów*, tradycyjnie 1/72 cala, podczas deweloperów systemu Windows używane *jednostki niezależnych od urządzenia* (DIUs) oparte na 1/96 cala.

Urządzenia przenośne, jednak zwykle są przechowywane znacznie bliżej kroju i mieć o rozdzielczości wyższej niż pulpitu ekrany, które oznacza może następować większej gęstości pikseli.

Programiści przeznaczonych dla urządzeń firmy Apple iPhone i iPad kontynuować pracę w jednostkach *punktów*, ale istnieją 160 punkty na cal. W zależności od urządzenia może być 1, 2 lub 3 pikseli do punktu.

Przypomina systemu android. Programiści pracy w jednostkach *pikselach niezależnych od gęstości* (punktu dystrybucji), i relacji między punktu dystrybucji i pikseli jest oparta na 160 punktu dystrybucji do cala.

Środowisko wykonawcze systemu Windows ma również jest ustalana skalowania czynników, które oznacza coś bliski 160 jednostek niezależnych od urządzenia na cal.

Podsumowując programista platformy Xamarin.Forms przeznaczonych dla telefonów i tabletów, można założyć, że wszystkie jednostki miary są oparte na następujących kryteriów:

- 160 jednostek na cal, odpowiednikiem
- 64 jednostki do centymetr

Tylko do odczytu [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) i [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) właściwości zdefiniowane przez `VisualElement` domyślny "mock" wartości &ndash;1. Tylko wtedy, gdy element został o rozmiarze i umieszczone w układzie te właściwości odpowiada rzeczywisty rozmiar elementu w jednostkach niezależnych od urządzenia. Ten rozmiar zawiera dowolną `Padding` ustawionej dla elementu, ale nie `Margin`.

Element wizualny generowane [ `SizeChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/) zdarzeń po jego `Width` lub `Height` została zmieniona. [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) w przykładzie użyto to zdarzenie, aby wyświetlić rozmiar ekranu programu.

## <a name="metrical-sizes"></a>Rozmiary metrical

[ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) używa [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) i [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) do wyświetlenia `BoxView` jeden cal wysoka, a drugi centymetr szerokości.

## <a name="estimated-font-sizes"></a>Rozmiary czcionek szacowany

[ **Rozmiary** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) przykładowych pokazano, jak użyć reguły 160 jednostki do cala do określenia rozmiary czcionek w jednostkach punktów. Visual spójność platformy przy użyciu tej metody jest lepszym rozwiązaniem niż `Device.GetNamedSize`.

## <a name="fitting-text-to-available-size"></a>Dopasowanie tekstu do dostępny rozmiar

Istnieje możliwość dopasowania bloku tekstu w prostokącie określonego obliczając `FontSize` z `Label` posługując się następującymi kryteriami:

- Odstępy między wierszami wynosi 120% rozmiaru czcionki (130% na platformach systemu Windows).
- Szerokość znaku średni to 50% rozmiaru czcionki.

[ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) przykładzie pokazano tej metody. Ten program został napisany przed [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) właściwość była dostępna, więc używa [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) z [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) ustawienie, aby symulować margines.

[![Potrójna zrzut ekranu przedstawiający rozmiar czcionki szacowany](images/ch05fg07-small.png "tekst Dopasuj do rozmiaru dostępne")](images/ch05fg07-large.png#lightbox "tekst Dopasuj do rozmiaru dostępne")

## <a name="a-fit-to-size-clock"></a>Zegar Dopasuj do rozmiaru

[ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) przykładzie pokazano, za pomocą [ `Device.StartTimer` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.StartTimer/p/System.TimeSpan/System.Func%7BSystem.Boolean%7D/) uruchomić czasomierza okresowo powiadamia aplikację, gdy nadejdzie czas, aby zaktualizować zegara. Rozmiar czcionki jest równa co szóstego szerokości strony umożliwiający wyświetlanie możliwie jak największy.

## <a name="accessibility-issues"></a>Ułatwienia dostępu

**EstimatedFontSize** program i **FitToSizeClock** program obie zawierają niewielkie luka: Jeśli użytkownik zmieni telefonu ustawienia ułatwień dostępu systemu Android lub Windows 10 Mobile, program nie jest już można oszacować wielkość tekst jest renderowany na podstawie rozmiaru czcionki. [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) w przykładzie pokazano, ten problem.

## <a name="empirically-fitting-text"></a>Empirycznie dopasowywania tekstu

Innym sposobem tekstu do prostokąta jest empirycznie Obliczanie rozmiaru tekstu renderowanych i dostosowanie go w górę lub w dół. Program w wywołaniach książki [ `GetSizeRequest` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/) na element wizualny, aby uzyskać wymagany rozmiar elementu. Metoda jest przestarzała, czy zamiast tego należy wywołać programy [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/).

Dla `Label`, pierwszy argument powinien być szerokość kontenera (aby umożliwić zawijania), a drugi argument powinien być ustawiony na `Double.PositiveInfinity` aby nieograniczonego wysokość. [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) przykładzie pokazano tej metody.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 5 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [Przykłady rozdział 5](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [Przykłady rozdział 5 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
