---
title: Witaj, iOS Multiscreen — szczegółowe informacje
description: W tym dokumencie przyjmuje lepiej poznać rozwiniętej aplikacji Phoneword, kolejne, biorąc pod uwagę model-view-controller, nawigacji dla systemu iOS i innych pojęć programowania dla systemu iOS.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: c866e5f4-8154-4342-876e-efa0693d66f5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: eaf77dd68895a3fbf677e1d0aa68125d81d709c1
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111228"
---
# <a name="hello-ios-multiscreen--deep-dive"></a>Witaj, iOS Multiscreen — szczegółowe informacje

W przewodniku Szybki Start będziemy skompilowane i uruchomiono naszej pierwszej aplikacji platformy Xamarin.iOS wieloma ekranami. Teraz nadszedł czas na programowanie lepiej zrozumieć architektura i nawigacji dla systemu iOS.

W tym przewodniku wprowadzeniu *, widoku kontrolera MVC (Model)* wzorzec i jego roli w architekturze systemu iOS i nawigacji.
Następnie możemy Poznaj kontrolera nawigacji i Dowiedz się, jak korzystać z niego do zapewnienia środowisko nawigacji znanych w systemie iOS.

<a name="Model_View_Controller" />

## <a name="model-view-controller-mvc"></a>Model-View-Controller (MVC)

W [Witaj, iOS](~/ios/get-started/hello-ios/index.md) samouczku dowiedzieliśmy się, czy aplikacje systemu iOS mają tylko jeden *okna* będące kontrolerów widoku odpowiedzialnym za ładowania ich *zawartości widoku hierarchii* do Okno. W drugiej wskazówki Phoneword firma Microsoft dodany drugi ekranu do naszej aplikacji i przekazywane dane — Lista numerów telefonów — między dwa ekrany, jak pokazano na poniższym diagramie:

 [![](hello-ios-multiscreen-deepdive-images/08.png "Na tym diagramie przedstawiono przekazywanie danych pomiędzy dwoma ekranami")](hello-ios-multiscreen-deepdive-images/08.png#lightbox)

W tym przykładzie dane zostały zebrane na pierwszym ekranie przekazywane z pierwszego kontrolera widoku do drugiego i wyświetlane przez drugi ekran. Ta separacja ekranów, kontrolerów widoku i danych następuje *, widoku kontrolera MVC (Model)* wzorca. W kolejnych sekcjach omówimy zalety wzorzec, jej składniki oraz jak możemy ją wykorzystać w naszej aplikacji Phoneword.

### <a name="benefits-of-the-mvc-pattern"></a>Zalety wzorzec MVC

Model-View-Controller jest *wzorzec projektowy* — do wielokrotnego użytku architektury rozwiązania często zdarza problem lub używania w kodzie. MVC to Architektura aplikacji za pomocą *graficznego interfejsu użytkownika (GUI)*. Przypisuje obiektów w aplikacji, jedną z trzech ról - *modelu* (logika danych lub aplikacji), *widoku* (interfejs użytkownika) i *kontrolera* (kod: za zaporą). Na poniższym diagramie przedstawiono relacje między trzy wzorzec MVC i użytkownika:

 [![](hello-ios-multiscreen-deepdive-images/00.png "Na tym diagramie przedstawiono relacje między trzy wzorzec MVC i użytkownika")](hello-ios-multiscreen-deepdive-images/00.png#lightbox)

Wzorzec MVC jest przydatne, ponieważ logiczne rozdzielenie między poszczególnymi częściami aplikacji graficznego interfejsu użytkownika i ułatwia nam ponowne używanie kodów i widoków. Teraz od razu i przyjrzyj się każdej z trzech ról bardziej szczegółowo.

> [!NOTE]
> Wzorzec MVC jest luźno odpowiednikiem struktury stron ASP.NET lub aplikacji WPF. W tych przykładach widok jest składnikiem, który są odpowiedzialne za opisu interfejsu użytkownika i odnosi się do strony ASPX (HTML) w programie ASP.NET lub XAML w aplikacji WPF. Kontroler jest składnikiem, który jest odpowiedzialny za zarządzanie widoku, który odpowiada kodem w programie ASP.NET lub WPF.

### <a name="model"></a>Model

Obiekt modelu jest zazwyczaj specyficzne dla aplikacji reprezentację danych, które ma być wyświetlane lub wprowadzany do widoku. Model jest często słabo zdefiniowana — na przykład w naszym **Phoneword_iOS** aplikacji, listę numerów telefonów (reprezentowana jako listę ciągów) to Model. Jeśli firma Microsoft kompilowania aplikacji dla wielu platform, firma Microsoft można wybrać opcji udostępniania **PhonewordTranslator** kodu między naszych aplikacji dla systemu Android i iOS. Można uważamy udostępnionego kodu jako modelu, jak również.

MVC jest całkowicie niezależny od *funkcji trwałości danych* i *dostępu* modelu. Innymi słowy, MVC zależy co naszych danych wygląda i jak są przechowywane, jak dane jest tylko *reprezentowane*. Na przykład firma może wybrać do przechowywania danych w bazie danych SQL lub utrwalić ją w niektórych mechanizm magazynu w chmurze lub po prostu użyć `List<string>`. Ze względów MVC reprezentacji danych, sama znajduje się we wzorcu.

W niektórych przypadkach części modelu MVC może być pusta. Na przykład możemy wybrać dodać niektórych stron statycznych do naszej aplikacji, wyjaśniające, jak działa telefonu w usłudze translator, dlaczego dostosowaliśmy ją i jak skontaktuj się z nami Zgłoś usterki. Te ekrany aplikacji będzie nadal można utworzyć przy użyciu widoków i kontrolerów, ale nie mają oni żadnych rzeczywistych danych modelu.

> [!NOTE]
> W niektórych literaturze modelu część wzorca MVC mogą odwoływać się do całej aplikacji wewnętrznej bazy danych, nie tylko dane wyświetlane w interfejsie użytkownika. W tym przewodniku używamy nowoczesnych interpretacji modelu, ale rozróżnienie nie jest szczególnie ważne.

### <a name="view"></a>Widok

Widok jest składnikiem, który jest odpowiedzialny za renderowania interfejsu użytkownika. W niemal wszystkich platform, używające wzorca MVC interfejs użytkownika składa się z hierarchii widoków. Firma Microsoft można traktować jako Wyświetl hierarchię z jednym widokiem — nazywane widokiem głównym - u góry hierarchii i dowolną liczbę podrzędnych widoków widoku MVC (nazywane lub widoków podrzędnych) poniżej. W systemie iOS ekranie zawartości widoku hierarchii odnosi się do widoku składnika MVC.

### <a name="controller"></a>Kontroler

Obiekt kontrolera jest składnikiem, przewody wszystko ze sobą, która jest reprezentowana w systemie iOS przez `UIViewController`. Naszym zdaniem kontrolera jako zapasowy kod ekranu lub zestaw widoków. Kontroler jest odpowiedzialny za nasłuchiwanie żądań od użytkownika i zwracanie hierarchii odpowiedniego widoku. Jej nasłuchuje żądań z widoku (kliknięć przycisków, wprowadzanie tekstu itp.) i wykonuje odpowiednie przetwarzania modyfikacji widoku i ponowne załadowanie widoku. Kontroler jest również odpowiedzialny za tworzenie lub pobierania modelu z dowolnych zapasowy magazyn danych istnieje w aplikacji i wypełnianie widoku ze swoimi danymi.

Kontrolery można również zarządzać innymi kontrolerami. Na przykład jeden kontroler może ładować innego kontrolera, wymaga wyświetlić inny ekran lub stos kontrolerów, aby monitorować ich kolejność oraz przejść między nimi zarządzać. W następnej sekcji zobaczymy przykładem na kontrolerze, który zarządza innymi kontrolerami, jak możemy wprowadzić specjalny typ o nazwie kontrolera widoku dla systemu iOS *kontrolera nawigacji*.

## <a name="navigation-controller"></a>Kontrolera nawigacji

W aplikacji Phoneword użyliśmy kontrolera nawigacji pomagające w zarządzaniu nawigację między wiele ekranów. Kontrolera nawigacji to wyspecjalizowana `UIViewController` reprezentowany przez `UINavigationController` klasy. Zamiast zarządzać pojedynczą hierarchię widok zawartości, kontrolera nawigacji zarządza innymi kontrolerami widoku, a także swoje własne szczególne zawartości Wyświetl hierarchię w formie narzędzi nawigacji, która obejmuje tytuł, przycisk Wstecz i inne funkcje opcjonalne.

Kontrolera nawigacji jest często używany w aplikacjach systemu iOS i zapewnia nawigację dla aplikacji systemu iOS zszywania, takich jak **ustawienia** aplikacji, jak pokazano na poniższym zrzucie ekranu:

 [![](hello-ios-multiscreen-deepdive-images/01.png "Kontrolera nawigacji umożliwia nawigację w aplikacjach systemu iOS, takie jak aplikacja ustawienia pokazane tutaj")](hello-ios-multiscreen-deepdive-images/01.png#lightbox)

Kontrolera nawigacji służy trzy podstawowe funkcje:

-  **Udostępnia punkty zaczepienia dla nawigacji do przodu** — kontrolera nawigacji używa metaphor Nawigacja hierarchiczna, w których zawartości widoku hierarchii *wypchnięcie* na *stos nawigacji* . Stos nawigacji można traktować jako stos kart gry, w których najważniejsze większość kart jest widoczne, jak pokazano na poniższym diagramie:  

    [![](hello-ios-multiscreen-deepdive-images/02.png "Na tym diagramie przedstawiono nawigacji, jakby były stosem kart")](hello-ios-multiscreen-deepdive-images/02.png#lightbox)


-  **Opcjonalnie zawiera przycisk Wstecz** — w przypadku przesuwamy nowy element na stosie nawigacji na pasku tytułu może automatycznie wyświetlać *przycisku Wstecz* umożliwiająca użytkownikowi Przejdź wstecz. Naciśnięcie przycisku Wstecz *POP* bieżącego kontrolera widoku w stosie nawigacji i ładowania poprzedniej hierarchii widok zawartości do okna:  

    [![](hello-ios-multiscreen-deepdive-images/03.png "Na tym diagramie przedstawiono usuwanie karty ze stosu")](hello-ios-multiscreen-deepdive-images/03.png#lightbox)


-  **Pasek tytułu zawiera** — górna część kontrolera nawigacji jest nazywany *paska tytułu* . Odpowiada za wyświetlanie tytułu kontrolera widoku, jak pokazano na poniższym diagramie:  

    [![](hello-ios-multiscreen-deepdive-images/04.png "Na pasku tytułu jest odpowiedzialny za wyświetlanie tytułu kontrolera widoku")](hello-ios-multiscreen-deepdive-images/04.png#lightbox)

### <a name="root-view-controller"></a>Kontroler widoku głównego

Kontrolera nawigacji nie zarządza hierarchii zawartości widoku, więc go nie ma nic do wyświetlania na swój własny.
Zamiast tego kontrolera nawigacji jest powiązany z *kontrolera widoku głównego*:

 [![](hello-ios-multiscreen-deepdive-images/05.png "Z kontrolera nawigacji jest powiązany z kontrolera widoku głównego")](hello-ios-multiscreen-deepdive-images/05.png#lightbox)

Kontroler widoku głównego reprezentuje pierwszy kontroler widoku w stosie kontrolera nawigacji i kontroler widoku głównego zawartości widoku hierarchii jest pierwsza hierarchia widoku zawartości do załadowania do okna. Jeśli chcemy umieścić naszej całej aplikacji na stosie kontrolera nawigacji, możemy przenieść Sourceless Segue kontrolera nawigacji i ustawić kontroler widoku nasz pierwszy ekran jako kontroler widoku głównego, tak jak Robiliśmy to aplikacja Phoneword:

 [![](hello-ios-multiscreen-deepdive-images/06.png "Sourceless Segue ustawia ekrany pierwszy kontroler widoku jako kontroler widoku głównego")](hello-ios-multiscreen-deepdive-images/06.png#lightbox)

### <a name="additional-navigation-options"></a>Opcje dodatkowe nawigacji

Kontrolera nawigacji jest często stosowaną metodą obsługi nawigacji w systemie iOS, ale nie jest jedyną opcją. Na przykład [kontrolera paska kart](~/ios/user-interface/controls/creating-tabbed-applications.md) można podzielić aplikacji do różnych obszarów funkcjonalnych i [kontrolera widoku podzielonego](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/split_view/use_split_view_to_show_two_controllers) może służyć do tworzenia widoków wzorzec/szczegół. Łączenie kontrolerów nawigacji przy użyciu tych paradygmatów nawigacji pozwala na wiele sposobów elastyczne umożliwiające zaprezentowanie i przejdź do zawartości w systemie iOS.

## <a name="handling-transitions"></a>Obsługa przejścia

W instruktażu Phoneword możemy obsługiwane przejścia między dwoma kontrolerów widoku na dwa różne sposoby — najpierw za pomocą scenorysu Segue a następnie programowo. Przyjrzyjmy się obu tych opcji bardziej szczegółowo.

### <a name="prepareforsegue"></a>PrepareForSegue

Jeśli dodamy Segue z **Pokaż** akcji do scenorysu, firma Microsoft wydać polecenie systemu iOS, aby wypchnąć drugiego kontrolera widoku na stosie kontrolera nawigacji:

 [![](hello-ios-multiscreen-deepdive-images/09.png "Ustawianie typu segue z listy rozwijanej")](hello-ios-multiscreen-deepdive-images/09.png#lightbox)

Dodawanie Segue do scenorysu jest wystarczająco dużo, aby utworzyć proste przejście między ekranami. Jeśli chcemy przekazać dane między kontrolerami widoku, mamy do zastąpienia `PrepareForSegue` metody i obsługi danych, określić główną przyczynę:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);
    ...
}
```

wywołania z systemem iOS `PrepareForSegue` przed przejścia występuje i przekazuje Segue, które zostały utworzone w scenorysu do metody.
W tym momencie mamy do ręcznego ustawiania Segue docelowy kontroler widoku. Poniższy kod pobiera uchwyt do kontrolera widoku docelowego i rzutuje odpowiednią klasą - CallHistoryController, w tym przypadku:

```csharp
CallHistoryController callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Na koniec możemy przekazać listę numerów telefonów (Model) z `ViewController` do `CallHistoryController` , ustawiając `PhoneHistory` właściwość `CallHistoryController` do listy numerów telefonów wybieranego:

```csharp
callHistoryContoller.PhoneNumbers = PhoneNumbers;
```

Kompletny kod do przekazywania danych za pomocą Segue jest następująca:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

    if (callHistoryContoller != null) {
         callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
 }
```

### <a name="navigation-without-segues"></a>Nawigacja bez przejść.

Przechodzenie z pierwszego kontrolera widoku do drugiego w kodzie jest tym samym procesie co przy użyciu Segue, ale kilka kroków trzeba ręcznie wykonać.
Po pierwsze, używamy `this.NavigationController` można odwołać się do kontrolera nawigacji którego stosu, firma Microsoft jest obecnie włączona. Następnie użyjemy kontrolera nawigacji `PushViewController` metodę, aby ręcznie wypychania dalej kontrolera widoku na stosie, przekazując kontrolera widoku i wybranie opcji animacji przejścia (Firma Microsoft Ustaw tę pozycję na `true`).

Poniższy kod obsługuje przejście z ekranu Phoneword do ekranu Historia połączeń:

```csharp
this.NavigationController.PushViewController (callHistory, true);
```

Zanim firma Microsoft może przejść do następnego kontrolera widoku, musimy tworzenia jego instancji ręcznie z scenorysu, wywołując `this.Storyboard.InstantiateViewController` i przekazywania w identyfikatorze scenorysu `CallHistoryController`:

```csharp
CallHistoryController callHistory =
this.Storyboard.InstantiateViewController
("CallHistoryController") as CallHistoryController;
```

Na koniec możemy przekazać listę numerów telefonów (Model) z `ViewController` do `CallHistoryController` , ustawiając `PhoneHistory` właściwość `CallHistoryController` do listy numerów telefonów wybieranego, tak samo, jak zrobiliśmy podczas obsługi możemy przejścia z Segue:

```csharp
callHistory.PhoneNumbers = PhoneNumbers;
```

Kompletny kod dla przejścia programowe jest następująca:

```csharp
CallHistoryButton.TouchUpInside += (object sender, EventArgs e) => {
    // Launches a new instance of CallHistoryController
    CallHistoryController callHistory = this.Storyboard.InstantiateViewController ("CallHistoryController") as CallHistoryController;
    if (callHistory != null) {
     callHistory.PhoneNumbers = PhoneNumbers;
     this.NavigationController.PushViewController (callHistory, true);
    }
};
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Dodatkowe założenia Phoneword

Aplikacja Phoneword wprowadzono kilka koncepcji, które nie są uwzględnione w tym przewodniku. Te pojęcia obejmują:

-  **Automatyczne tworzenie kontrolerów widoku** — w przypadku możemy wprowadzić nazwę klasy dla kontrolera widoku w **konsoli właściwości** , narzędzia iOS designer umożliwia sprawdzenie, jeśli ta klasa istnieje, a następnie generuje kontrolera widoku kopii klasy dla nas. Aby uzyskać więcej informacji na ten temat i inne funkcje projektanta systemu iOS, zobacz [wprowadzenie do narzędzia iOS Designer](~/ios/user-interface/designer/introduction.md) przewodnik.
-  **Kontroler widoku tabeli** — `CallHistoryController` jest kontroler widoku tabeli. Kontroler widoku tabeli zawiera widok tabeli, najbardziej typowe układ i dane są wyświetlane narzędzi w systemie iOS. Tabele wykraczają poza zakres tego podręcznika. Aby uzyskać więcej informacji na temat kontrolerów widoku tabeli, można znaleźć [Praca z tabelami i komórek](~/ios/user-interface/controls/tables/index.md) przewodnik.
-   **Tworzenie scenorysów identyfikator** — Ustawianie Identyfikatora scenorysu tworzy klasę kontrolera widoku w języku Objective-C, zawierający kodem kontrolera widoku w scenorysu. Używamy Identyfikatora scenorysu można znaleźć klasy języka Objective-C i utworzyć wystąpienie kontrolera widoku w scenorysu. Aby uzyskać więcej informacji na temat identyfikatorów scenorysu, można znaleźć [wprowadzenie do scenorysów](~/ios/user-interface/storyboards/index.md) przewodnik.

## <a name="summary"></a>Podsumowanie

Gratulacje, ukończono swoją pierwszą aplikację dla systemu iOS z wieloma ekranami!

W tym przewodniku możemy wprowadzono wzorca MVC i umożliwia tworzenie wielu ekranowaną aplikacji. Rozważyliśmy również kontrolerów nawigacji i ich roli w zapewniająca nawigacji dla systemu iOS. Masz teraz solidną podstawę, należy rozpocząć tworzenie aplikacji platformy Xamarin.iOS.

Następnie nauczysz się, jak tworzyć aplikacje dla wielu platform za pomocą platformy Xamarin przy użyciu [wprowadzenie do programowania aplikacji mobilnych](~/cross-platform/get-started/introduction-to-mobile-development.md) i [tworzenie aplikacji dla wielu Platform](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) przewodników.

## <a name="related-links"></a>Linki pokrewne

- [Witaj, iOS (przykład)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [Portal Aprowizowania dla systemu iOS](https://developer.apple.com/ios/manage/overview/index.action)
