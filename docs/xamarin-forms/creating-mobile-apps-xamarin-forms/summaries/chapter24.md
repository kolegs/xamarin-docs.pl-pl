---
title: Podsumowanie rozdziałów 24. Nawigacja strony
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziałów 24. Nawigacja strony'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 94434eb1fbce07f06b21d4e0daf37e4f7f47670b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995657"
---
# <a name="summary-of-chapter-24-page-navigation"></a>Podsumowanie rozdziałów 24. Nawigacja strony

Wiele aplikacji składają się z wielu stron, wśród których użytkownik przechodzi. Aplikacja zawsze ma *głównego* strony lub *macierzystego* strony, a w tym miejscu użytkownik przechodzi do innych stron, które są przechowywane w stosie podczas przechodzenia wstecz. Opcje dodatkowe nawigacji zostały omówione w [ **rozdział 25. Stronie odmian**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Modalne i niemodalne stron

`VisualElement` definiuje [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) właściwości typu [ `INavigation` ](xref:Xamarin.Forms.INavigation), która obejmuje następujących dwóch metod, aby przejść do nowej strony:

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

Obie metody `Page` wystąpienie jako argument i zwrócenia `Task` obiektu. Następujących dwóch metod przejdź z powrotem do poprzedniej strony:

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

Jeśli interfejs użytkownika ma swój własny **ponownie** przycisku (tak jak dla systemów Android i Windows Phone), a następnie nie jest konieczne dla aplikacji w celu wywołania tych metod.

Mimo że te metody są dostępne z dowolnego `VisualElement`, zazwyczaj są one nazywane `Navigation` właściwości bieżącego `Page` wystąpienia.

Aplikacje zazwyczaj używają strony modalne po użytkownik musi podać niektóre informacje na stronie przed powrotem do poprzedniej strony. Stron, które nie są modalne są czasami nazywane niemodalne lub *hierarchiczne*. Żadne postanowienie sama strona odróżnia go jako modalne lub niemodalne; jego podlega zamiast tego metodę używaną do przejście do niej. Aby pracować na wszystkich platformach, modalne strony należy podać interfejs użytkownika nawigacji wrócić do poprzedniej strony.

[ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) przykładowe umożliwia Eksplorowanie różnica między stronami niemodalne i modal. Dowolne aplikacje korzystające z nawigacji na stronie musi upłynąć jego strony głównej, aby [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) konstruktora, zwykle w pliku programu `App` klasy. Jeden podwyższenia jest już należy ustawić `Padding` na stronie dla systemu iOS.

Użytkownik będzie stwierdza, że dla niemodalne strony, strony firmy [ `Title` ](xref:Xamarin.Forms.Page.Title) właściwość jest wyświetlana. Systemu iOS, Android i platformy tablet i pulpitu Windows należy podać element interfejsu użytkownika, aby wrócić do poprzedniej strony. Kurs, Android i Windows phone urządzenia mają standardowego **ponownie** przycisk, aby przejść z powrotem.

Dla modalnego strony, strony `Title` nie jest wyświetlany, i uwzględniane w stosownym żaden element interfejsu użytkownika, aby wrócić do poprzedniej strony. Chociaż można używać standardowych telefonu dla systemów Android i Windows **ponownie** przycisk, aby wrócić do poprzedniej strony, strony modalne na innych platformach, należy podać swój własny mechanizm, aby wrócić.

### <a name="animated-page-transitions"></a>Przejścia animowany stron

Alternatywne wersje różnych metod nawigacji są dostarczane z drugi argument logiczny, który ustawiony na `true` chcącym przejście strony, aby uwzględnić animacji:

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

Jednak metody standardowa Nawigacja strony obejmują animacji domyślnie, dzięki czemu tylko są one przydatne do nawigowania na danej stronie, podczas uruchamiania (zgodnie z opisem na końcu tego rozdziału) lub podczas tworzenia animacji wejścia (zgodnie z opisem w [ **Chapter22. Animacja**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Zmiany wizualne i funkcjonalne

`NavigationPage` zawiera dwie właściwości, które można ustawić podczas tworzenia wystąpienia klasy w swojej `App` metody:

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

`NavigationPage` zawiera także cztery dołączone właściwości możliwej do wiązania, które wpływają na danej stronie, na którym są ustawione:

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) i [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) i [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) i [ `GetBackButtonTitle` ](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) działają tylko w systemie iOS
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) i [ `GetTitleIcon` ](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) działają w systemach iOS i Android tylko

### <a name="exploring-the-mechanics"></a>Poznawanie sposobu

Metody nawigacji strony są wszystkie asynchroniczne i powinien być używany z `await`. Zakończenie nie oznacza, że Nawigacja strony została ukończona, ale tylko bezpieczeństwa zbadać stos nawigacji na stronie.

Po jednej stronie przechodzi do innego, pierwsza strona ogólnie pobiera wywołanie w celu jego [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) metoda i drugiej stronie pobiera wywołanie w celu jego [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) metody. Podobnie, gdy zwraca jedną stronę do innego, pierwsza strona pobiera wywołanie w celu jego `OnDisappearing` metoda i drugiej stronie ogólnie pobiera wywołanie w celu jego `OnAppearing` metody. Kolejność tych wywołań (i uzupełniania metod asynchronicznych, który wywołuje nawigacji) to platforma zależnych. Użycie słowa "ogólnie" w dwóch poprzednich instrukcji jest spowodowane Android nawigowania po stronach modalne, w którym nie występują te wywołania metody.

Ponadto wywołania `OnAppearing` i `OnDisappearing` metody nie musi oznaczać nawigacji na stronie.

`INavigation` Interfejs zawiera dwie właściwości kolekcji, które pozwalają sprawdzić stos nawigacji:

- [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) typu `IReadOnlyList<Page>` niemodalne stosu
- [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) typu `IReadOnlyList<Page>` modalne stosu

Dostęp do tych stosów od najbezpieczniej jest `Navigation` właściwość `NavigationPage` (powinien być `App` klasy [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) właściwości). Tylko jest bezpieczne zbadać te stosy po zakończeniu metody asynchronicznej Nawigacja strony. [ `CurrentPage` ](xref:Xamarin.Forms.NavigationPage.CurrentPage) Właściwość `NavigationPage` nie wskazuje na bieżącą stronę bieżącej strony modalne strony, ale wskazuje zamiast ostatniej strony niemodalne.

[ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) próbki pozwala eksplorować nawigacji na stronie i stosów i typów prawnych dokonywania nawigacji strony:

- Niemodalne strony można przejść do innej strony niemodalne lub strony modalne
- Strony modalne poruszanie się tylko do innej strony modalne

### <a name="enforcing-modality"></a>Wymuszanie modalności

Aplikacja używa modalnego strony, gdy jest to konieczne uzyskać pewne informacje od użytkownika. Użytkownik powinien być zabroniony zwracanie do poprzedniej strony, dopóki nie podano tych informacji. W systemach iOS, to proste zapewnienie **ponownie** przycisk i włącz ją, tylko wtedy, gdy użytkownik zakończył się ze stroną. Jednak systemem Android lub Windows phone, należy zastąpić aplikacji [ `OnBackButtonPressed` ](xref:Xamarin.Forms.Page.OnBackButtonPressed) metody i zwrócenie `true` Jeśli program został obsłużony **ponownie** przycisk sam, jak pokazano w [ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) próbki.

[ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) w przykładzie pokazano, jak to działa w scenariuszu MVVM.

## <a name="navigation-variations"></a>Zmiany nawigacji

Jeśli określonej strony modalne można nawigować w taki sposób, aby wiele razy, należy go przechowywać informacje, dzięki czemu użytkownik może edytować informacje, a nie wpisywać go w ponownie. Problem można rozwiązać, zachowując konkretnego wystąpienia modalne strony, ale lepszym rozwiązaniem (szczególnie w systemie iOS) zachowuje informacje zawarte w modelu widoku.

### <a name="making-a-navigation-menu"></a>Tworzenie menu nawigacji

[ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) przykładzie pokazano użycie `TableView` do listy elementów menu. Każdy element jest skojarzony z `Type` obiektu dla określonej strony. Po wybraniu tego elementu program tworzy stronę i przechodzi do niego.

[![Potrójna zrzut ekranu przedstawiający widok galerii typu](images/ch24fg21-small.png "elementów Menu ofercie TableView")](images/ch24fg21-large.png#lightbox "elementów Menu ofercie TableView")

[ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) próbki jest nieco inny, w tym menu zawiera wystąpienia każdej strony zamiast typów. Pomaga to zachować informacje z każdej strony, ale wszystkie strony musi być tworzone w momencie uruchamiania programu.

### <a name="manipulating-the-navigation-stack"></a>Manipulowanie stos nawigacji

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) pokazuje kilka funkcji zdefiniowanych przez `INavigation` umożliwiające manipulowanie stos nawigacji w sposób ze strukturą:

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) i [ `PopToRootAsync` ](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean)) za pomocą opcjonalnych animacji

### <a name="dynamic-page-generation"></a>Generowanie strony dynamicznej

[ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) przykład pokazuje, utworzenie strony w oparciu o dane wejściowe użytkownika w czasie wykonywania.

## <a name="patterns-of-data-transfer"></a>Wzorce transferu danych

Jest często udostępniać dane między stronami &mdash; do transferu danych, aby nawigować strony, a dla strony zwrócić dane do strony, które je wywołało. Istnieje kilka technik do takiego postępowania.

### <a name="constructor-arguments"></a>Argumenty konstruktora

Podczas przechodzenia do nowej strony, istnieje możliwość tworzenia wystąpienia klasy strony, przy użyciu argumentu konstruktora, który pozwala na stronę, aby zainicjować sam. [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) przykład pokazuje to. Istnieje również możliwość nawigować strony jego `BindingContext` ustawione przez strony, która przechodzi do niego.

### <a name="properties-and-method-calls"></a>Wywołań metod i właściwości

Pozostałe przykłady transferu danych zapoznaj się z problemem przekazywania informacji między stronami, gdy po jednej stronie przechodzi do innej strony i z powrotem. W tych dyskusji *macierzystego* strony przechodzi do *informacje* strony, a trzeba przekazać zainicjowanej informacje *informacje* strony. *Informacje* strony uzyskuje dodatkowe informacje od użytkownika i przekazuje informacje do *macierzystego* strony.

*Macierzystego* strony mogą łatwo uzyskiwać dostęp do metod i właściwości w *informacje* stronie tak szybko, jak metoda tworzy tę stronę. *Informacje* strony mogą też uzyskiwać dostęp metody publiczne i właściwości w *macierzystego* strony, ale wybierając odpowiedni moment, dla tego może być trudne. [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) przykład robi to jej `OnDisappearing` zastąpienia. Wadą jest to, że *informacje* strony musi znać typ *macierzystego* strony.

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) klasa udostępnia dwie strony do komunikowania się ze sobą w inny sposób. Komunikaty są identyfikowane przez ciąg tekstowy i mogą być dołączone w dowolnych obiektów.

Program, który chce odbierać komunikaty z określonego typu musi subskrybować je przy użyciu [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) i określić funkcję wywołania zwrotnego. Później można anulować, wywołując [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*). Funkcja wywołania zwrotnego odbiera wszystkie komunikaty wysyłane z określonego typu o określonej nazwie, które są wysyłane za pośrednictwem [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) metody.

[ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) program pokazuje, jak na przesyłanie danych za pomocą Centrum obsługi komunikatów, ale wymaga to, aby ponownie *informacje* strony jest znany typ *macierzystego* strony.

### <a name="events"></a>Zdarzenia

Zdarzenie jest to time-honored podejście do jednej klasy do wysyłania informacji do innej klasy, nie wiedząc o tym typie tej klasy. W [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) przykładowe *informacje* klasa definiuje zdarzenia, które są generowane, gdy informacje są gotowe. Jednak nie ma żadnych wygodnym miejscem *macierzystego* strony, aby odłączyć program obsługi zdarzeń.

### <a name="the-app-class-intermediary"></a>Pośrednika klasa aplikacji

[ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) przykład pokazuje, jak uzyskać dostęp do właściwości zdefiniowane w `App` klasy przez *macierzystego* strony i *informacje*strony. Jest to dobre rozwiązanie, ale w następnej sekcji opisano coś lepiej.

### <a name="switching-to-a-viewmodel"></a>Przełączanie do ViewModel

Za pomocą ViewModel w celu umożliwia informacje *macierzystego* strony i *informacje* strony, aby udostępnić wystąpienia klasy informacji. Jest to zaprezentowane w [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) próbki.

### <a name="saving-and-restoring-page-state"></a>Zapisywanie i przywracanie stanu strony

`App` Pośrednika klasy lub metody ViewModel jest idealnym rozwiązaniem w przypadku aplikacji należy zapisać informacje, jeśli program przechodzi w stan uśpienia podczas *informacje* strona jest aktywna. [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) przykład pokazuje to.

## <a name="saving-and-restoring-the-navigation-stack"></a>Zapisywanie i przywracanie stos nawigacji

W przypadku ogólnych wielostronicowe program, który przechodzi w stan uśpienia powinien przejdź do tej samej stronie, po przywróceniu. Oznacza to, że taki program, należy zapisać zawartość stos nawigacji. W tej sekcji pokazano, jak zautomatyzować ten proces w klasie przeznaczone do tego celu. Ta klasa wywołuje również poszczególnych stron, aby umożliwić im do zapisywania i przywracania ich stanu strony.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Biblioteka definiuje interfejs o nazwie [ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) czy klasy można zaimplementować w celu zapisywania i przywracania elementy w `Properties`słownika.

[ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) Klasy w **Xamarin.FormsBook.Toolkit** pochodzi od klasy biblioteki `Application`. Może pochodzić z `App` klasy z `MultiPageRestorableApp` i wykonywać niektóre celów.

[ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) demonstruje użycie `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Coś w rodzaju aplikacji rzeczywistych

[ **NoteTaker** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) próbki, również sprawia, że użycie `MultiPageRestorableApp` i umożliwia wprowadzanie i edytowanie uwagi, które są zapisywane w `Properties` słownika.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdziału 24 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Przykłady rozdziału 24](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Nawigacja hierarchiczna](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Strony modalne](~/xamarin-forms/app-fundamentals/navigation/modal.md)
