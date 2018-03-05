---
title: "Podstawy języka XAML platformy Xamarin.Forms"
description: "Wprowadzenie do znaczników i platform urządzeń przenośnych"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: a3f3dbbe0f12cfa7cc1fc6606ec8bd48a96e407c
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-xaml-basics"></a>Podstawy języka XAML platformy Xamarin.Forms

Język XAML — Extensible Application Markup Language — umożliwia deweloperom definiowanie interfejsów użytkownika w aplikacjach Xamarin.Forms przy użyciu znaczników zamiast kodu. XAML nigdy nie jest wymagana w programie platformy Xamarin.Forms, ale jest często bardziej zwięzły i wizualnie spójnego niż równoważne kod i potencjalnie biorąc. XAML jest szczególnie dobrze nadają się do użytku z popularnych architektura MVVM (Model-View-ViewModel): XAML definiuje widok, który jest powiązany kod ViewModel za pośrednictwem powiązania danych opartych na języku XAML.

## <a name="xaml-basics-contents"></a>Zawartość podstawy języka XAML

* [Omówienie](#Overview)
* [Part 1. Część 1. Wprowadzenie do języka XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [Part 2. Część 2. Podstawowa składnia języka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [Part 3. Część 3. Rozszerzenia struktury znaczników XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [Part 4. Część 4. Powiązania danych — podstawy](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [Part 5. Z danych powiązanie z modelem MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

Oprócz tych artykułach podstawy języka XAML, możesz pobrać rozdziałach książki [tworzenia aplikacji mobilnych za pomocą platformy Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "Okładce książki")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

Tematy XAML bardziej szczegółowo w wielu rozdziałach książki, w tym:

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
    <h4>Rozdział 8. Kod i języka XAML w zgodzie</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">Pobierz plik PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">Podsumowanie</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Rozdział 10. Rozszerzenia znaczników XAML</h4>
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

Tych działów może być [pobrać bezpłatnie](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

<a name="Overview" />

## <a name="overview"></a>Omówienie

XAML to język opartych na języku XML utworzone przez firmę Microsoft jako alternatywę do programowania kodu do tworzenia wystąpienia i Inicjowanie obiektów i organizowania tych obiektów w hierarchii nadrzędny podrzędny. XAML został dostosowany do kilku technologii w programie .NET framework, ale znaleziono jego największy narzędzie podczas definiowania układu interfejsy użytkownika w Windows Presentation Foundation (WPF), Silverlight, środowiska uruchomieniowego systemu Windows i uniwersalnych systemu Windows Platformy Uniwersalnej.

XAML jest również częścią platformy Xamarin.Forms, dla systemu iOS, Android i platformy uniwersalnej systemu Windows na podstawie natywnie interfejsu programowania i platform urządzeń przenośnych. W pliku XAML deweloperów platformy Xamarin.Forms można zdefiniować przy użyciu wszystkich platformy Xamarin.Forms widoków, układu i strony, jako klas również jako niestandardowych interfejsów użytkownika. Plik XAML można skompilować lub osadzone w pliku wykonywalnego. W obu przypadkach analizowania informacji XAML w czasie kompilacji, aby zlokalizować nazwanych obiektów i ponownie w czasie wykonywania, wystąpienia i Inicjowanie obiektów oraz do ustanawiania połączeń między te obiekty i programowania kodu.

XAML ma kilka zalet w porównaniu z równoważną kodu:

-  XAML jest często zwięzły i czytelne niż równoważne kodu.
-  W XML hierarchii nadrzędny podrzędny umożliwia XAML naśladował z większą wizualna hierarchii nadrzędny podrzędny obiektów interfejsu użytkownika.
-  XAML mogą być łatwo odręcznego przez programistów, ale również pozwalają biorąc i został wygenerowany przez narzędzia visual projektu.

Oczywiście dostępne są również wady, przede wszystkim dotyczące ograniczenia, które są wewnętrzne dla języków znaczników:

-  XAML nie może zawierać kod. Wszystkie programy obsługi zdarzeń musi być zdefiniowana w pliku kodu.
-  XAML nie może zawierać pętle powtarzających się przetwarzania. (Jednak niektóre obiekty visual platformy Xamarin.Forms — głównie [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) — można wygenerować wiele elementów podrzędnych obiektów w oparciu o jego `ItemsSource` kolekcji.)
-  XAML nie może zawierać przetwarzania warunkowego (jednak wiązania danych można odwoływać konwerter opartej na kodzie powiązania, który umożliwia efektywne niektórych przetwarzania warunkowego).
-  XAML, zwykle nie można utworzyć wystąpienia klasy, które nie określają konstruktora bez parametrów. (Istnieje jednak czasami nawigację to ograniczenie.)
-  Ogólnie rzecz biorąc XAML nie można wywołać metody. (Ponownie, to ograniczenie czasami można rozwiązać.)

Nie ma jeszcze wizualnego projektanta generowania XAML w aplikacji platformy Xamarin.Forms. Wszystkie XAML musi być odręcznego, ale [podglądu plików XAML](~/xamarin-forms/xaml/xaml-previewer.md). Programistów, którzy XAML może być często tworzenie i uruchamianie aplikacji, zwłaszcza po wszystkich elementów, które mogą być oczywiście nieprawidłowe. Nawet deweloperom doświadczonych w języku XAML wiedzieć, że jest nagrody eksperymenty.

XAML jest zasadniczo pliku XML, ale niektóre funkcje unikatowy składni ma XAML. Najważniejsze są:

- Właściwości elementów
- Dołączone właściwości
- Rozszerzenia znaczników

Te funkcje są *nie* rozszerzenia XML. XAML jest całkowicie prawne XML. Ale te funkcje składni języka XAML w unikatowy sposób przy użyciu XML. One opisano szczegółowo w artykułach poniżej, które zawierają zawiera wprowadzenie do wdrażania MVVM przy użyciu kodu XAML.

## <a name="requirements"></a>Wymagania

W tym artykule przyjęto założenie, pracy znajomość platformy Xamarin.Forms. Odczytywanie [wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) jest zdecydowanie zalecane.

W tym artykule przyjęto założenie pewną znajomość XML, w tym opis użyj deklaracji przestrzeni nazw XML i warunki *elementu*, *tag*, i *atrybutu*.

Jeśli znasz platformy Xamarin.Forms i XML, należy rozpocząć odczyt [część 1. Wprowadzenie do języka XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md).



## <a name="related-links"></a>Linki pokrewne

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Tworzenie książki Mobile Apps](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
