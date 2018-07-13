---
title: Podstawy XAML zestawu narzędzi Xamarin.Forms
description: Ten przewodnik wyjaśnia, jak rozpocząć pracę z XAML dla wielu platform dla urządzeń przenośnych. XAML umożliwia deweloperom definiowanie interfejsów użytkownika w aplikacjach Xamarin.Forms przy użyciu znaczników zamiast kodu.
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 901174eb9510eaab670564655f9f6b4bff940bd7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995443"
---
# <a name="xamarinforms-xaml-basics"></a>Podstawy XAML zestawu narzędzi Xamarin.Forms

Język XAML — Extensible Application Markup Language — umożliwia deweloperom definiowanie interfejsów użytkownika w aplikacjach Xamarin.Forms przy użyciu znaczników zamiast kodu. XAML nigdy nie jest wymagana w programie Xamarin.Forms, ale jest często bardziej zwięzły i bardziej spójną niż równoważny kod i potencjalnie biorąc. XAML szczególnie dobrze nadaje się do użytku z popularnej architektury MVVM (Model-View-ViewModel) w aplikacji: XAML definiuje widok, który jest połączony z ViewModel kodu za pomocą powiązania danych opartych na XAML.

## <a name="xaml-basics-contents"></a>XAML — podstawy zawartość

* [Omówienie](#Overview)
* [Part 1. Część 1. Wprowadzenie do języka XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [Part 2. Część 2. Podstawowa składnia języka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [Part 3. Część 3. Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [Part 4. Część 4. Powiązania danych — podstawy](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [Part 5. Dane powiązanie z modelem MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

Oprócz tych artykułów XAML — podstawy, możesz pobrać rozdziały książki [tworzenia aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "Okładki książki")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

Tematy XAML bardziej szczegółowo w sekcji wiele rozdziały książki, w tym:

<table style="border:0px; box-shadow:0 0px 0px" cellpadding="0" cellspacing="2" border="0" width="85%">
<tr style="background:#ecf0f1">
  <td style="border:0px;">
    <h4>Rozdział 7. XAML vs. Kod</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf">Pobierz plik PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md">Podsumowanie</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Rozdział 8. Kod i XAML w zgodzie</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">Pobierz plik PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">Podsumowanie</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Rozdział 10. Rozszerzenia znaczników w XAML</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf">Pobierz plik PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md">Podsumowanie</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Rozdział 18. MVVM</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf">Pobierz plik PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md">Podsumowanie</a></td></tr>
</table>

Może być tych działów [pobrać bezpłatnie](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

<a name="Overview" />

## <a name="overview"></a>Omówienie

XAML to oparty na standardzie XML język stworzone przez firmę Microsoft jako alternatywę dla kodu do tworzenia wystąpienia i Inicjowanie obiektów i organizowania tych obiektów w hierarchii nadrzędny podrzędny. XAML został dostosowany do kilku technologii w ramach programu .NET framework, ale znaleziono jego najlepsze narzędzie podczas definiowania układu interfejsów użytkownika w Windows Presentation Foundation (WPF), Silverlight, środowisko wykonawcze Windows i Windows Universal Platforma (systemu Windows UWP).

XAML jest również częścią zestawu narzędzi Xamarin.Forms dla systemów iOS, Android i platformy uniwersalnej systemu Windows na podstawie natywnie interfejs programowania dla wielu platform urządzeń przenośnych. W pliku XAML zestawu narzędzi Xamarin.Forms dla deweloperów można zdefiniować interfejsów użytkownika przy użyciu wszystkich Xamarin.Forms widoków układów i stron, jak również jako niestandardowych klas. Plik XAML może skompilowany lub osadzone w pliku wykonywalnym. W obu przypadkach informacje XAML jest analizowany w czasie kompilacji, aby zlokalizować nazwane obiekty i ponownie w czasie wykonywania, aby utworzyć wystąpienia i zainicjalizować obiektów i w celu ustalenia łącza między tymi obiektami i kodu.

XAML ma kilka zalet w stosunku równoważny kod:

-  XAML jest często bardziej zwięzły i czytelne niż równoważny kod.
-  Hierarchii nadrzędny podrzędny, nieprzerwaną pracę w pliku XML umożliwia XAML do naśladowania Komitet itcs visual hierarchii nadrzędny podrzędny, obiektów interfejsu użytkownika.
-  XAML mogą być łatwo odręcznej przez programistów, ale także pozwala biorąc i generowane przez narzędzi do projektowania wizualnego.

Oczywiście dostępne są również wady, przede wszystkim dotyczące ograniczeń, które są nierozerwalnie związane języków znaczników:

-  XAML nie może zawierać kod. Wszystkie procedury obsługi zdarzeń musi być zdefiniowany w pliku kodu.
-  XAML nie może zawierać pętli do przetworzenia powtarzających się. (Jednak kilka obiektów wizualnych zestawu narzędzi Xamarin.Forms — zwłaszcza [ `ListView` ](xref:Xamarin.Forms.ListView) — można generować wiele obiektów podrzędnych, na podstawie obiektów w jego `ItemsSource` kolekcji.)
-  XAML nie może zawierać przetwarzanie warunkowe (jednak powiązanie danych może odwoływać się konwerter powiązania oparte na kodzie, który umożliwia efektywne jakieś operacje przetwarzania warunkowego.)
-  XAML, zwykle nie można utworzyć wystąpienia klasy, które nie definiują konstruktora bez parametrów. (Istnieje jednak czasami sposób obejścia tego ograniczenia.)
-  Ogólnie XAML nie można wywołać metody. (Ponownie, to ograniczenie czasami można rozwiązać.)

Nie ma jeszcze wizualnego projektanta do generowania XAML w aplikacjach Xamarin.Forms. Wszystkie XAML musi być w sejfie, ale ma [Podgląd XAML programu](~/xamarin-forms/xaml/xaml-previewer.md). Programistów, którzy XAML może być często tworzenie i uruchamianie aplikacji, szczególnie po wszystkich elementów, które mogą być niepoprawne oczywiście. Nawet deweloperzy doświadczenia w XAML wiedzieć, że jest nagradzanie eksperymentowanie w usłudze.

XAML jest zasadniczo XML, ale niektóre funkcje unikatowe składnia XAML. Najważniejsze są:

- Właściwości elementów
- Dołączone właściwości
- Rozszerzenia znaczników

Te funkcje są *nie* rozszerzenia XML. XAML jest całkowicie prawne XML. Ale te funkcje składnia XAML XML w unikatowy sposób. One opisano szczegółowo w poniższych artykułach, które zawierają wprowadzenie do implementowania MVVM przy użyciu XAML.

## <a name="requirements"></a>Wymagania

W tym artykule założono znajomość pracy z zestawem narzędzi Xamarin.Forms. Odczytywanie [wprowadzenie do zestawu narzędzi Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) zdecydowanie zaleca się.

W tym artykule założono również pewną znajomość XML, w tym informacje o użycie deklaracje przestrzeni nazw XML wraz ze wszystkimi postanowieniami *elementu*, *tag*, i *atrybutu*.

Gdy jesteś zaznajomiony z zestawu narzędzi Xamarin.Forms i XML, należy rozpocząć odczyt [część 1. Rozpoczęcie pracy z XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md).



## <a name="related-links"></a>Linki pokrewne

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Wprowadzenie do zestawu narzędzi Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Tworzenie aplikacji mobilnych książki](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
