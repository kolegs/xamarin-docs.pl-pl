---
title: Podsumowanie rozdziałów 25. Różne typy stron
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziałów 25. Różne typy stron'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 3496504ed6326992aabfceb1d4d3476223c9e4e0
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156551"
---
# <a name="summary-of-chapter-25-page-varieties"></a>Podsumowanie rozdziałów 25. Różne typy stron

Do tej pory w tym samouczku dwóch klas, które wynikają z `Page`: `ContentPage` i `NavigationPage`. W tym rozdziale przedstawiono dwie pozostałe:

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) zarządza dwoma stronami, główny i szczegółów
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) zarządza wiele stron podrzędnych, dostępne za pośrednictwem karty

Te typy Strony zapewniają bardziej zaawansowane opcje nawigacji niż `NavagationPage` omówione w [rozdziału 24. Strona nawigacji](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md).

## <a name="master-and-detail"></a>Wzorzec i szczegółów

[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) Definiuje dwie właściwości typu `Page`: [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) i [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail). Ogólnie każdej z tych właściwości można ustawić `ContentPage`. `MasterDetailPage` Wyświetla i zmienia między te dwie strony.

Istnieją dwa podstawowe sposoby przełączać się między te dwie strony:

- *Podziel* gdzie głównej maszynie wirtualnej i szczegóły są obok siebie
- *popover* gdzie strony szczegółów obejmuje lub częściowo obejmuje master strony

Istnieje kilka odmian *popover* podejście (*slajdów*, *nakładają się*, i *wymiany*), ale są one zazwyczaj platformy zależne. Możesz ustawić [ `MasterDetailBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) właściwość `MasterDetailPage` do elementu członkowskiego [ `MasterBehavior` ](xref:Xamarin.Forms.MasterBehavior) wyliczenia:

- [`Default`](xref:Xamarin.Forms.MasterBehavior.Default)
- [`Split`](xref:Xamarin.Forms.MasterBehavior.Split)
- [`SplitOnLandscape`](xref:Xamarin.Forms.MasterBehavior.SplitOnLandscape)
- [`SplitOnPortrait`](xref:Xamarin.Forms.MasterBehavior.SplitOnPortrait)
- [`Popover`](xref:Xamarin.Forms.MasterBehavior.Popover)

Jednakże ta właściwość nie ma wpływu na telefonach. Telefony zawsze ma zachowanie popover. Tylko tabletów i pulpitu systemu windows może mieć zachowanie podziału.

### <a name="exploring-the-behaviors"></a>Eksplorowanie zachowania

[ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) próbki pozwala na eksperymentowanie z domyślnym zachowaniem na różnych urządzeniach. Program zawiera dwa oddzielne `ContentPage` pochodnych dla głównej maszynie wirtualnej i szczegóły (przy użyciu `Title` ustawioną zarówno właściwość) oraz inną klasę, która pochodzi od klasy `MasterDetailPage` łączy je. Strona szczegółów jest ujęty w `NavigationPage` ponieważ program platformy uniwersalnej systemu Windows nie działa bez niego.

Platformy Windows 8.1 i Windows Phone 8.1 wymagają, że mapy bitowej być równa `Icon` właściwości strony wzorcowej.

### <a name="back-to-school"></a>Powrót do szkoły

[ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) przykładowe zajmuje nieco innego podejścia do konstruowania program, który będzie wyświetlać uczniów z [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) biblioteki.

`Master` i `Detail` właściwości są definiowane przy użyciu drzewa wizualnego w [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) pliku, który pochodzi od klasy `MasterDetailPage`. To rozwiązanie umożliwia powiązań danych, należy ustawić między stron wzorcowych i szczegółów.

Plik XAML także ustawia [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) właściwość `MasterDetailPage` do `True`. Powoduje to, że strony wzorcowej, który będzie wyświetlany podczas uruchamiania; Domyślnie zostanie wyświetlona strona szczegółów. [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) plików zestawów `IsPresented` do `false` po wybraniu elementu z `ListView` na stronie głównej. Następnie zostanie wyświetlona strona szczegółów:

[![Potrójna zrzut ekranu szczegółów i szkoły](images/ch25fg09-small.png "strony szczegółów MasterDetailPage")](images/ch25fg09-large.png#lightbox "strony szczegółów MasterDetailPage")

### <a name="your-own-user-interface"></a>Interfejs użytkownika

Mimo że Xamarin.Forms udostępnia interfejs użytkownika do przełączania się między widokami głównego i szczegółów, możesz podać własne. Aby to zrobić:

- Ustaw [ `IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabled) właściwości `false` można wyłączyć, szybko przesuwając
- Zastąp [ `ShouldShowToolbarButton` ](xref:Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton) metody i zwrócenie `false` Aby ukryć przyciski paska narzędzi na Windows 8.1 i Windows Phone 8.1.

Następnie należy podać sposób przełączać się między strony wzorcowe i szczegółowe, takie jak dowodzą [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) próbki.

[ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) przykładzie pokazano użycie innego podejścia `TapGestureRecognizer` na stronach wzorcowych i szczegółowych.

## <a name="tabbedpage"></a>TabbedPage

[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) To zbiór stron, które można przełączać się między za pomocą karty. Pochodzi od klasy `MultiPage<Page>` i umożliwia zdefiniowanie, nie publiczny właściwości lub metody własne. [`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1), jednak zdefiniować właściwości:

- [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) właściwości typu `IList<T>`

Wypełnij ten `Children` kolekcji z obiektami strony.

Innym podejściem pozwala na zdefiniowanie `TabbedPage` podobnie jak elementy podrzędne `ListView` za pomocą te dwie właściwości, które automatycznie wygenerować strony z kartami:

- [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) tego typu `IEnumerable`
- [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) tego typu `DataTemplate`

Jednak to podejście nie działa dobrze w systemie iOS Jeśli kolekcja zawiera więcej niż kilka elementów.

`MultiPage<T>` definiuje dwa więcej właściwości, które pozwalają Ci śledzić strony, na które są aktualnie wyświetlane:

- [`CurrentPage`](xref:Xamarin.Forms.MultiPage`1.CurrentPage) typu `T`odwołujący się do strony
- [`SelectedItem`](xref:Xamarin.Forms.MultiPage`1.SelectedItem) typu `Object`odwołujący się do obiektu w `ItemsSource` kolekcji

`MultiPage<T>` definiuje również dwa zdarzenia:

- [`PagesChanged`](xref:Xamarin.Forms.MultiPage`1.PagesChanged) gdy `ItemsSource` zmiany kolekcji
- [`CurrentPageChanged`](xref:Xamarin.Forms.MultiPage`1.CurrentPageChanged) Podczas zmiany wyświetlane strony

### <a name="discrete-tab-pages"></a>Strony kart dyskretnych

[ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) próbka składa się z trzech z kartami stron, które wyświetlają kolory na trzy różne sposoby. Każda karta jest `ContentPage` utworów zależnych, a następnie `TabbedPage` utworów zależnych [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) łączy trzy strony.

Dla każdej strony, która pojawia się w `TabbedPage`, `Title` właściwość jest wymagana, aby określić tekst, na karcie i Store firmy Apple wymaga także używana ikona więc `Icon` właściwość jest ustawiona dla systemu iOS:

[![Potrójna zrzut ekranu przedstawiający dyskretnych kolory na kartach](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

[ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) przykład ma stronę główną, która zawiera listę wszystkich uczniów. Wybrany student powoduje to przejście do `TabbedPage` utworów zależnych, [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), która zawiera trzy `ContentPage` obiekty w jego drzewa wizualnego, z których jedna umożliwia, wprowadzając kilka uwag dla tej uczniów lub studentów.

### <a name="using-an-itemtemplate"></a>Za pomocą właściwości ItemTemplate

[ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) przykładowy używa [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki. [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) plików zestawów `DataTemplate` właściwość `TabbedPage` drzewa wizualnego, rozpoczynając `ContentPage` zawierający powiązania właściwości `NamedColor` (w tym powiązanie z `Title` właściwości).

To problemy w systemie iOS. Tylko kilka elementów, które mogą być wyświetlane i jest dobrym sposobem ciągowych ikon.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 25 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [Przykłady 25 rozdział](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [Strona wzorzec / szczegół](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [Strona z kartami](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
