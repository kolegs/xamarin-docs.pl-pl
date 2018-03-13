---
title: "Podsumowanie rozdziału 24. Nawigacja strony"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 3390a298cd8d9967f0aea2bd9fb5a90830714ba5
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-24-page-navigation"></a>Podsumowanie rozdziału 24. Nawigacja strony

Wiele aplikacji składają się z wielu stron, wśród których użytkownik nawiguje. Aplikacja zawsze ma *głównego* strony lub *macierzystego* stronę, i z tego miejsca użytkownik przechodzi do innych stron, które są obsługiwane w stosie nawigacji Wstecz. Opcje dodatkowe nawigacji są objęte [ **rozdział 25. Strona odmian**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>Modalne i niemodalne stron

`VisualElement` definiuje [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) właściwości typu [ `INavigation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INavigation/), która obejmuje następujących dwóch metod, aby przejść do nowej strony:

- [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/)
- [`PushModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/)

Obie metody `Page` wystąpienia jako argumentów i `Task` obiektu. Następujących dwóch metod Przejdź wstecz do poprzedniej strony:

- [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync()/)
- [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/)

Jeśli interfejs użytkownika ma własną **ponownie** przycisk (tak jak Android i Windows Phone), a następnie nie jest niezbędna dla aplikacji do wywołania tych metod.

Mimo że te metody są dostępne z dowolnej `VisualElement`, zwykle są one nazywane `Navigation` właściwości bieżącego `Page` wystąpienia.

Aplikacje zazwyczaj używać modalne stron, gdy użytkownik musi podać niektóre informacje na stronie przed zwróceniem do poprzedniej strony. Stron, które nie są modalne są nazywane niemodalne lub *hierarchiczna*. Nic w samej strony odróżnia go jako modalne i niemodalne; go podlega zamiast tego metodę używaną do przejdź do niego. Aby pracować na wszystkich platformach, modalne strony podać interfejs użytkownika do nawigowania wstecz do poprzedniej strony.

[ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) próbki pozwala Eksploruj różnica między stronami niemodalne i modalne. Dowolnej aplikacji, która używa Nawigacja strony musi przejść na stronę główną [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) konstruktora, zwykle w programie `App` klasy. Jeden podwyższenia jest już potrzeby ustawić `Padding` na stronie dla systemu iOS.

Możesz zauważyć, że niemodalne strony, strony w [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) właściwość jest wyświetlana. IOS, Android i platformy tablet i pulpitu systemu Windows należy podać element interfejsu użytkownika, aby wrócić do poprzedniej strony. Plan, Android i Windows phone urządzenia mają standard **ponownie** przycisk, aby wrócić do poprzedniej strony.

Modalne strony, strony `Title` nie jest wyświetlany żaden element interfejsu użytkownika są udostępniane, aby powrócić do poprzedniej strony i. Mimo że używasz standardowego Android i Windows phone **ponownie** przycisk, aby powrócić do poprzedniej strony, strony modalnej na innych platformach podać mechanizm aby wrócić do poprzedniej strony.

### <a name="animated-page-transitions"></a>Przejścia animowany stron

Alternatywne wersje różnych metod nawigacji są dostarczane z drugiego argumentu logiczna ustawionej dla `true` Jeśli przejście strony, aby uwzględnić animacji:

- [PushAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PushModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PopAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync/p/System.Boolean/)
- [PopModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync/p/System.Boolean/)

Jednak metody standardowe Nawigacja strony obejmują animacji domyślnie, dzięki czemu są one tylko przydatna do nawigowania do określonej strony podczas uruchamiania (zgodnie z opisem w końcowej w tym rozdziale) lub podczas dostarczania animacji wejścia (zgodnie z opisem w [ **Chapter22. Animacja**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>Zmiany wizualne i funkcjonalne

`NavigationPage` dwie właściwości, które można ustawić podczas wystąpienia klasy w Twojej `App` metody:

- [`BarBackgroundColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/)
- [`BarTextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarTextColor/)

`NavigationPage` zawiera również cztery dołączonych właściwości możliwej do wiązania, które wpływają na określonej strony, na którym są ustawione:

- [`SetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasBackButton/p/Xamarin.Forms.Page/System.Boolean/) I [`GetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasBackButton/p/Xamarin.Forms.Page/)
- [`SetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasNavigationBar/p/Xamarin.Forms.BindableObject/System.Boolean/) I [`GetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasNavigationBar/p/Xamarin.Forms.BindableObject/)
- [`SetBackButtonTitle`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetBackButtonTitle/p/Xamarin.Forms.BindableObject/System.String/) i [ `GetBackButtonTitle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetBackButtonTitle/p/Xamarin.Forms.BindableObject/) pracować nad tylko system iOS
- [`SetTitleIcon`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetTitleIcon/p/Xamarin.Forms.BindableObject/Xamarin.Forms.FileImageSource/) i [ `GetTitleIcon` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetTitleIcon/p/Xamarin.Forms.BindableObject/) pracy w systemach iOS i Android tylko

### <a name="exploring-the-mechanics"></a>Eksploracja sposobu

Metody nawigacji strony są wszystkie asynchroniczne i powinien być używany z `await`. Zakończenie nie oznacza, że Nawigacja strony zostało ukończone, ale tylko bezpieczeństwa zbadanie stos nawigacji strony.

Po jednej stronie przechodzi do innej, pierwsza strona ogólnie pobiera wywołanie jego [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) — metoda i drugiej stronie pobiera wywołanie jego [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) — metoda. Podobnie w przypadku zwraca jedną stronę do innego, pierwsza strona pobiera wywołanie jego `OnDisappearing` ogólnie pobiera wywołania metody, a druga strona jego `OnAppearing` metody. Kolejność tych wywołań (i zakończeniu metod asynchronicznych wywołująca nawigacji) to platforma zależnych. Używanie wyrazów "ogólnie" w dwóch poprzednich instrukcji wynika z Android nawigacji strony modalne, w którym nie występują te wywołania metody.

Ponadto wywołań `OnAppearing` i `OnDisappearing` metody nie musi oznaczać nawigacji strony.

`INavigation` Interfejs zawiera dwie właściwości kolekcji, które pozwalają sprawdzić stos nawigacji:

- [`NavigationStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) typu `IReadOnlyList<Page>` niemodalne stosu
- [`ModalStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) typu `IReadOnlyList<Page>` modalne stosu

Jest możliwie najbezpieczniejszy dostęp do tych stosów od `Navigation` właściwość `NavigationPage` (powinny być `App` klasy [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) właściwości). Jest tylko bezpiecznych do sprawdzenia tych stosów po zakończeniu metody asynchronicznej Nawigacja strony. [ `CurrentPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.CurrentPage/) Właściwość `NavigationPage` nie wskazuje na bieżącej stronie bieżącej strony modalne strony, ale oznacza zamiast tego ostatniej strony niemodalne.

[ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) próbki pozwala poznać Nawigacja strony i stosy i typów prawnych nawigacji strony:

- Niemodalne strony można przejść do innego niemodalne lub strona modalne
- Modalne strony można przechodzić tylko do innej strony modalne

### <a name="enforcing-modality"></a>Wymuszanie modalność

Aplikacja używa modalne strony, gdy jest to niezbędne uzyskać informacje od użytkownika. Użytkownik powinien być zabroniony z powrót do poprzedniej strony, dopóki nie podano informacji. W systemach iOS, jest łatwy w celu zapewnienia **ponownie** przycisk i włącz ją, tylko wtedy, gdy użytkownik zakończy się ze stroną. Ale urządzeń systemu Android i Windows phone, aplikacja powinny zastępować [ `OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed()/) — metoda i wrócić `true` Jeśli obsłużyła program **ponownie** przycisku, jak pokazano w [ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) próbki.

[ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) przykładzie pokazano, jak to działa w scenariuszu MVVM.

## <a name="navigation-variations"></a>Zmiany nawigacji

Jeśli konkretnej strony modalne może zostać przesłane do wiele razy, powinien on przechowywania informacji, dzięki czemu użytkownik może edytować informacje, a nie wpisywać go wszystkie w ponownie. Problem można rozwiązać, zachowując konkretnego wystąpienia modalne strony, ale zachowuje informacje w modelu widoku lepszym rozwiązaniem (szczególnie w systemie iOS).

### <a name="making-a-navigation-menu"></a>Tworzenie menu nawigacji

[ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) przykładzie pokazano, za pomocą `TableView` do listy elementów menu. Każdy element jest skojarzony z `Type` obiektu dla określonej strony. Po wybraniu tego elementu, program tworzy stronę i przechodzi do niego.

[![Potrójna zrzut ekranu przedstawiający Widok Galeria typu](images/ch24fg21-small.png "TableView listę elementów Menu")](images/ch24fg21-large.png#lightbox "TableView listę elementów Menu")

[ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) próbki różni się nieco w tym menu zawiera wystąpienia każdej strony zamiast typów. Pozwala to zachować informacje na każdej stronie, ale wszystkie strony musi być tworzone przy uruchamianiu programu.

### <a name="manipulating-the-navigation-stack"></a>Manipulowanie stos nawigacji

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) przedstawiono kilka funkcji zdefiniowanych przez `INavigation` umożliwiające manipulowania stos nawigacji w strukturze sposób:

- [`RemovePage`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage/p/Xamarin.Forms.Page/)
- [`InsertPageBefore`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore/p/Xamarin.Forms.Page/Xamarin.Forms.Page/)
- [`PopToRootAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync()/) i [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync/p/System.Boolean/) z animacją opcjonalne

### <a name="dynamic-page-generation"></a>Strony dynamiczne generowanie

[ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) przykładzie pokazano, utworzenie strony w oparciu o dane wejściowe użytkownika w czasie wykonywania.

## <a name="patterns-of-data-transfer"></a>Wzorce transferu danych

Konieczne jest często udostępniać dane między stronami & #x 2014; Transfer danych do strony nawigować, a dla strony powrócić do strony, która ją wywołała danych. Istnieje kilka technik dla tej czynności.

### <a name="constructor-arguments"></a>Argumenty konstruktora

Podczas nawigowania do nowej strony, jest możliwość utworzenia wystąpienia klasy strony z argumentu konstruktora, który umożliwia strony do zainicjowania. [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) przykładzie pokazano to. Istnieje również możliwość nawigować strony jego `BindingContext` ustawione przez strony, który nawiguje do niego.

### <a name="properties-and-method-calls"></a>Właściwości i wywołania metody

Pozostałe przykłady transferu danych Poznaj problemu przekazywania informacji między Stronami po jednej stronie przechodzi do innej strony i z powrotem. W tych dyskusjach *macierzystego* strony przechodzi do *informacji* strony i należy przenieść zainicjowane informacje *informacji* strony. *Informacji* strony uzyskuje dodatkowe informacje od użytkownika i przekazuje informacje do *macierzystego* strony.

*Macierzystego* strony mogą łatwo uzyskiwać dostęp do publicznej metody i właściwości w *informacji* strony jak metoda tworzy tej strony. *Informacji* strony można także uzyskać dostęp do publicznej metody i właściwości w *macierzystego* strony, ale wybierając odpowiedni moment, może to być istotne znaczenie. [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) próbki robi to jej `OnDisappearing` zastąpienia. Wadą jest to, że *informacji* musi wiedzieć, typ strony *macierzystego* strony.

### <a name="messagingcenter"></a>MessagingCenter

Platformy Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) klasy zawiera dwie strony do komunikowania się ze sobą w inny sposób. Komunikaty są identyfikowane za pomocą ciągu tekstowego i mogą towarzyszyć dowolnych obiektów.

Program, który chce odbierać komunikaty z określonego typu musi subskrybować je przy użyciu [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender,TArgs%7D/p/System.Object/System.String/System.Action%7BTSender,TArgs%7D/TSender/) i określ funkcję wywołania zwrotnego. Później można anulować, wywołując [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/). Funkcja wywołania zwrotnego odbiera wszystkie wiadomości wysłane z określonego typu o określonej nazwie wysyłanych za pośrednictwem [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) metody.

[ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) program ilustruje sposób przesyłania danych za pomocą Centrum wiadomości, ale wymaga to, aby ponownie *informacji* strony jest znany typ *macierzystego* strony.

### <a name="events"></a>Zdarzenia

Zdarzenie jest time-honored podejście do jedną klasę do wysyłania informacji do innej klasy bez uprzedniego uzyskania informacji o typie tej klasy. W [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) próbki *informacji* klasa definiuje zdarzenia, które są generowane, gdy dane są gotowe. Jednak nie ma żadnych wygodne miejsce do *macierzystego* stronę, aby odłączyć programu obsługi zdarzeń.

### <a name="the-app-class-intermediary"></a>Pośrednik klasy aplikacji

[ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) przykładowych pokazano, jak uzyskać dostęp do właściwości zdefiniowane w `App` klasy zarówno *macierzystego* strony i *informacji*strony. Jest to dobre rozwiązanie, ale w następnej sekcji opisano coś lepiej.

### <a name="switching-to-a-viewmodel"></a>Przełączanie do ViewModel

Przy użyciu ViewModel w celu umożliwia informacje *macierzystego* strony i *informacji* stronę, aby udostępnić wystąpienia klasy informacji. To jest przedstawiona w [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) próbki.

### <a name="saving-and-restoring-page-state"></a>Zapisywanie i przywracanie stanu strony

`App` Pośrednika klasa lub metoda ViewModel jest idealne aplikacji musisz zapisać informacje, jeśli program przechodzi w stan uśpienia podczas *informacji* strona jest aktywna. [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) przykładzie pokazano to.

## <a name="saving-and-restoring-the-navigation-stack"></a>Zapisywanie i przywracanie stos nawigacji

W przypadku ogólnych wielostronicowe program, który przechodzi w stan uśpienia powinien przejdź do tej samej stronie, po jego przywróceniu. Oznacza to, że taki program należy zapisać zawartość stos nawigacji. W tej sekcji przedstawiono sposób zautomatyzować ten proces w klasie przeznaczone do tego celu. Ta klasa wywołuje również poszczególnych stron, aby zezwolić im na zapisywanie i przywracanie stanu strony.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Biblioteka definiuje interfejs o nazwie [ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) czy klasy można zaimplementować w celu zapisywania i przywracania elementy `Properties`słownika.

[ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) Klasy w **Xamarin.FormsBook.Toolkit** pochodną biblioteki `Application`. Następnie może pochodzić z `App` klasę z `MultiPageRestorableApp` i wykonywać niektórych celów.

[ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) zademonstrowano użycie `MultiPageRestorableApp`.

### <a name="something-like-a-real-life-app"></a>Coś, takich jak aplikacji rzeczywistych

[ **NoteTaker** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) przykładowa również sprawia, że użycie `MultiPageRestorableApp` i pozwala na wprowadzanie i edycję uwag, które są zapisywane w `Properties` słownika.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdziału 24 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [Przykłady rozdziału 24](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [Nawigacja hierarchiczna](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Strony modalne](~/xamarin-forms/app-fundamentals/navigation/modal.md)
