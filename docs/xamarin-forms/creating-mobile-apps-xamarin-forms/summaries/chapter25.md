---
title: "Podsumowanie rozdział 25. Odmian strony"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: bbe960357d9180df90a4423d6acfdf3f869d1b77
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-25-page-varieties"></a>Podsumowanie rozdział 25. Odmian strony

Do tej pory przedstawiono dwie klasy, które pochodzą z `Page`: `ContentPage` i `NavigationPage`. W tym rozdziale przedstawiono dwa inne:

- [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) zarządza dwoma stronami, wzorca i szczegółów
- [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) zarządza wiele stron podrzędnych dostępne za pośrednictwem karty

Te typy Strony zapewniają bardziej zaawansowane opcje nawigacji niż `NavagationPage` omówione w [rozdziału 24. Strona nawigacji](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md).

## <a name="master-and-detail"></a>Wzorzec i szczegóły

[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) Definiuje dwie właściwości typu `Page`: [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) i [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/). Ogólnie rzecz biorąc każdej z tych właściwości można ustawić `ContentPage`. `MasterDetailPage` Wyświetla i zmienia między tymi dwoma stronami.

Istnieją dwa podstawowe sposoby przełączania się między tymi dwoma stronami:

- *Podziel* gdzie wzorca i szczegóły są obok siebie
- *popover* gdzie strony szczegółów obejmuje lub częściowo obejmuje wzorca strony

Kilka różnych wersji *popover* podejście (*slajdów*, *nakłada się na*, i *wymiany*), ale zazwyczaj są to platformy zależne. Można ustawić [ `MasterDetailBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) właściwość `MasterDetailPage` do elementu członkowskiego [ `MasterBehavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterBehavior/) wyliczenie:

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Default/)
- [`Split`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Split/)
- [`SplitOnLandscape`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnLandscape/)
- [`SplitOnPortrait`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnPortrait/)
- [`Popover`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Popover/)

Jednak ta właściwość nie ma wpływu na telefonach. Zachowanie popover zawsze występuje w telefonach. Zachowanie podziału mogą dotyczyć wyłącznie tabletów i pulpitu systemu windows.

### <a name="exploring-the-behaviors"></a>Eksploracja zachowania

[ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) próbki pozwala na wypróbowanie domyślne zachowanie na różnych urządzeniach. Program zawiera dwa oddzielne `ContentPage` pochodnych dla wzorca i szczegóły (z `Title` właściwość zarówno), a inną klasę, która jest pochodną `MasterDetailPage` łączy je. Strona szczegółów jest ujęta w `NavigationPage` ponieważ program platformy uniwersalnej systemu Windows nie działa bez niego.

Platformy Windows 8.1 i Windows Phone 8.1 wymagają, że mapy bitowej mieć ustawioną `Icon` właściwości strony wzorcowej.

### <a name="back-to-school"></a>Powrót do służbowego

[ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) próbki ma nieco inny podejście do konstruowania były wyświetlane studentów z [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) biblioteki.

`Master` i `Detail` właściwości są zdefiniowane z drzewa wizualnego [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) pliku, który jest pochodną `MasterDetailPage`. To rozmieszczenie umożliwia powiązania danych, należy ustawić wartość między stron wzorcowych i szczegółów.

Plik XAML także ustawia [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) właściwość `MasterDetailPage` do `True`. Powoduje to, że strony wzorcowej, który będzie wyświetlany podczas uruchamiania; Domyślnie zostanie wyświetlona strona szczegółów. [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) plików zestawów `IsPresented` do `false` po wybraniu elementu z `ListView` na stronie głównej. Następnie zostanie wyświetlona strona szczegółów:

[![Potrójna zrzut ekranu szczegółów i szkoły](images/ch25fg09-small.png "strony szczegółów z MasterDetailPage")](images/ch25fg09-large.png "strony szczegółów z MasterDetailPage")

### <a name="your-own-user-interface"></a>Interfejs użytkownika

Mimo że platformy Xamarin.Forms udostępnia interfejs użytkownika do przełączania się między widokami wzorca i szczegółów, możesz podać własne. Aby to zrobić:

- Ustaw [ `IsGestureEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsGestureEnabled/) właściwości `false` można wyłączyć, szybko przesuwając
- Zastąpienie [ `ShouldShowToolbarButton` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton()/) — metoda i przywracać `false` ukrycia przycisków paska narzędzi na Windows 8.1 i Windows Phone 8.1.

Następnie należy podać sposób przełączania się między stron wzorcowych i szczegółów, takich jak dowodzą [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) próbki.

[ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) przykładzie pokazano, przy użyciu innej metody `TapGestureRecognizer` na stron wzorcowych i szczegółów.

## <a name="tabbedpage"></a>TabbedPage

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) To zbiór stron, które można przełączać się między przy użyciu karty. Dziedziczy `MultiPage<Page>` i definiuje nie właściwości publiczne lub jego własnej metody. [`MultiPage<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/), jednak zdefiniować właściwości:

- [`Children`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) właściwości typu `IList<T>`

Wypełnij to `Children` kolekcji z obiektami strony.

Innym rozwiązaniem można zdefiniować `TabbedPage` dzieci podobnie jak `ListView` za pomocą te dwie właściwości, które automatycznie generować stron z kartami:

- [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) typu `IEnumerable`
- [`ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) typu `DataTemplate`

Jednak ta metoda nie działa dobrze w systemie iOS Jeśli kolekcja zawiera więcej niż kilka elementów.

`MultiPage<T>` definiuje dwie więcej właściwości, które umożliwiają użytkownikowi śledzenie strony, która jest aktualnie wyświetlany:

- [`CurrentPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.CurrentPage/) typu `T`odwołujący się do strony
- [`SelectedItem`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.SelectedItem/) typu `Object`odwołujący się do obiektu w `ItemsSource` kolekcji

`MultiPage<T>` definiuje również dwa zdarzenia:

- [`PagesChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.PagesChanged/) gdy `ItemsSource` zmiany kolekcji
- [`CurrentPageChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.CurrentPageChanged/) Podczas zmiany przeglądanej strony

### <a name="discrete-tab-pages"></a>Osobne karty

[ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) próbka składa się z trzech karty, zawierające kolory na trzy różne sposoby. Każda karta jest `ContentPage` pochodnej, a następnie `TabbedPage` pochodną [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) łączy trzy strony.

Dla każdej strony, która jest wyświetlana w `TabbedPage`, `Title` jest wymagana właściwość, aby określić na karcie tekst i sklepu Apple wymaga również użycia ikony więc `Icon` właściwość jest ustawiona dla systemu iOS:

[![Potrójna zrzut ekranu przedstawiający odrębne kolory na kartach](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png "TabbedPage")

[ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) strony głównej, który zawiera listę wszystkich studentów ma próbki. Wybrany studenta powoduje to przejście do `TabbedPage` pochodnej, [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), który zawiera trzy `ContentPage` obiekty w jego drzewie wizualnym, z których jedna umożliwia wprowadzanie niektórych notatek dla tego uczniów.

### <a name="using-an-itemtemplate"></a>Przy użyciu ItemTemplate.

[ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) przykładowe używa [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) klasy w [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteki. [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) plików zestawów `DataTemplate` właściwość `TabbedPage` do drzewa wizualnego, począwszy od `ContentPage` zawierający powiązania z właściwości `NamedColor` (w tym powiązanie z `Title` właściwości).

To powodować problemy w systemie iOS. Mogą być wyświetlane tylko kilka elementów, a nie istnieje sposób dobrej nadanie im ikony.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst 25 rozdziałów (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [Przykłady rozdział 25](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [Strona główny szczegółowy](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [Strona z kartami](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
