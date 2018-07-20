---
title: Podsumowanie rozdział 5. Obsługa rozmiarów
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdział 5. Obsługa rozmiarów'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: c82e222fd47f3a3f13043c076c488b4769659352
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156499"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>Podsumowanie rozdział 5. Obsługa rozmiarów

> [!NOTE] 
> Uwagi na tej stronie wskazać obszary, w którym Xamarin.Forms podzielił z materiału znajdujące się w książce.

Do tej pory napotkano kilku rozmiarów w interfejsie Xamarin.Forms:

- Wysokość paska stanu systemu iOS to 20
- `BoxView` Domyślnej szerokości i wysokości 40
- Wartość domyślna `Padding` w `Frame` wynosi 20
- Wartość domyślna `Spacing` na `StackLayout` wynosi 6
- `Device.GetNamedSize` Metoda zwraca rozmiar czcionki liczbowe

Te rozmiary nie są pikseli. Są to jednostki miary niezależne od urządzenia niezależnie rozpoznał każdej z platform.

## <a name="pixels-points-dps-dips-and-dius"></a>Piksele, punkty, punktu dystrybucji, spadku i DIUs

Na wczesnym etapie historiach komputerami Mac firmy Apple i Microsoft Windows programistów pracował w jednostkach pikseli. Jednak pojawienie się Wyświetla wyższej rozdzielczości wymagane zwirtualizowanych i bardziej abstrakcyjne podejście na współrzędne ekranu. W świecie Mac programistów pracy w jednostkach *punktów*, tradycyjnie 1/72 cala, podczas deweloperów Windows używane *jednostki miary niezależne od urządzenia* (DIUs) oparte na 1/96 cala.

Urządzenia przenośne, jednak ogólnie posiadane są znacznie bliższe udostępnienia do powierzchni i mieć większej rozdzielczości niż pulpitu ekrany, które oznacza, że może być tolerowana większa gęstość pikseli.

Programiści przeznaczone dla urządzeń iPhone i iPad firmy Apple w dalszym ciągu działać w jednostkach *punktów*, ale istnieją 160 te punkty na cal. W zależności od urządzenia może być 1, 2 lub 3 pikseli w punkcie.

Android jest podobny. Programiści pracować w jednostkach *pikselach niezależnych od gęstości* (dps) i relacji między dps i pikseli opiera się na 160 dps na cal.

Windows Phone i urządzeń przenośnych również ustalonymi skalowania czynników, które oznaczają coś blisko 160 jednostki niezależne od urządzenia w celu CAL.

> [!NOTE]
> Zestaw narzędzi Xamarin.Forms nie obsługuje już dowolnego telefonu z systemem Windows lub urządzenia przenośnego.

Podsumowując programisty Xamarin.Forms, przeznaczone dla telefonów i tabletów założyć, że wszystkie jednostki miary są oparte na następujących kryteriów:

- 160 jednostki w celu CAL, odpowiednikiem
- 64 jednostki do centymetr

Tylko do odczytu [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) i [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) właściwości zdefiniowane przez `VisualElement` domyślny "Testowanie" wartości &ndash;1. Tylko wtedy, gdy element został wielkości i umieszczone w układzie te właściwości odzwierciedlają rzeczywisty rozmiar elementu w jednostkach niezależnych od urządzenia. Ten rozmiar zawiera dowolną `Padding` skonfigurować w elemencie, ale nie `Margin`.

Uruchamia element wizualny [ `SizeChanged` ](xref:Xamarin.Forms.VisualElement.SizeChanged) zdarzenia podczas jego `Width` lub `Height` został zmieniony. [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) przykładowych użyto to zdarzenie, aby wyświetlić rozmiar ekranu tego programu.

## <a name="metrical-sizes"></a>Rozmiary metrical

[ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) używa [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) i [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) do wyświetlenia `BoxView` 1 cal taluj i jeden centymetr szerokości.

## <a name="estimated-font-sizes"></a>Rozmiary czcionek szacowany

[ **Rozmiary** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) przykład pokazuje, jak używać reguły 160 jednostek do cala Aby określić rozmiary czcionek w jednostkach punktów. Wizualne spójności między platformami przy użyciu tej metody jest lepsze niż `Device.GetNamedSize`.

## <a name="fitting-text-to-available-size"></a>Dopasowanie tekstu do dostępny rozmiar

Istnieje możliwość dopasowania blok tekstu w prostokącie określonego przez obliczenie `FontSize` z `Label` przy użyciu następujących kryteriów:

- Interlinia jest 120% rozmiar czcionki (130% na platformach Windows).
- Szerokość znaków średni wynosi 50% rozmiaru czcionki.

[ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) w przykładzie pokazano tej techniki. Ten program został napisany przed [ `Margin` ](xref:Xamarin.Forms.View.Margin) właściwość była dostępna, więc używa [ `ContentView` ](xref:Xamarin.Forms.ContentView) z [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) ustawienie, aby symulować margines.

[![Potrójna zrzut ekranu przedstawiający rozmiar czcionki szacowany](images/ch05fg07-small.png "tekstu, Dopasuj do rozmiaru dostępne")](images/ch05fg07-large.png#lightbox "tekstu, Dopasuj do rozmiaru dostępne")

## <a name="a-fit-to-size-clock"></a>Zegar Dopasuj do rozmiaru

[ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) przykładzie pokazano użycie [ `Device.StartTimer` ](xref:Xamarin.Forms.Device.StartTimer(System.TimeSpan,System.Func{System.Boolean})) uruchomić czasomierza, powiadamiający okresowo aplikacji, gdy nadejdzie czas na aktualizację zegara. Rozmiar czcionki jest równa jednej szóstej szerokość strony umożliwiają wyświetlanie możliwie jak największy.

## <a name="accessibility-issues"></a>Ułatwienia dostępu

**EstimatedFontSize** program i **FitToSizeClock** program oba zawierają subtelne wady: Jeśli użytkownik zmieni się na telefonie ustawienia ułatwień dostępu systemu Android lub Windows 10 Mobile, program nie jest już można oszacować rozmiar, jaki tekst jest renderowany na podstawie rozmiaru czcionki. [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) przykład demonstruje ten problem.

## <a name="empirically-fitting-text"></a>Empirically dopasowywania tekstu

Innym sposobem, aby dopasować tekst do prostokąta jest empirically Obliczanie rozmiaru tekstu renderowanego i dostosować go w górę lub w dół. Program w wywołaniach książki [ `GetSizeRequest` ](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double)) na element wizualny, aby uzyskać żądany rozmiar elementu. Czy metoda jest przestarzała i zamiast tego wywołać programy [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)).

Aby uzyskać `Label`, pierwszy argument powinien być szerokość kontenera (aby dać zawijania), podczas gdy drugi argument powinien mieć wartość do `Double.PositiveInfinity` aby wysokość nieograniczone. [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) w przykładzie pokazano tej techniki.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 5 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [Przykłady rozdział 5](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [Przykłady rozdział 5 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
