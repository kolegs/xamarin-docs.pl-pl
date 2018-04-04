---
title: Witaj, iOS Multiscreen
description: W tym przewodniku dwuczęściową możemy rozwiń aplikację Phoneword utworzone w Hello, iOS Podręcznik obsługi drugi ekranu. Wzdłuż sposób firma Microsoft będzie wprowadzenie wzorca projektowego Model-View-Controller, zaimplementować w naszym pierwszym nawigacji dla systemu iOS oraz opracowanie głębsze zrozumienie struktury aplikacji dla systemu iOS i funkcji.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: c866e5f4-8154-4342-876e-efa0693d66f5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: 6f3c02bf3e5def0ad4acdb82e4c8a2606159846a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="hello-ios-multiscreen-deep-dive"></a>Witaj, iOS Wieloekranowy nowości

W przewodniku Szybki Start firma Microsoft wbudowany i uruchomiono naszych pierwszej aplikacji platformy Xamarin.iOS wielu ekranu. Teraz nadszedł czas na opracowanie lepiej zrozumieć architektura i nawigacji dla systemu iOS.

W tym przewodniku wprowadzeniu *modelu widoku, kontroler (MVC)* wzorca i jego rolą architektura systemu iOS i nawigacji.
Następnie możemy Poznaj kontrolera nawigacji i Dowiedz się użyć go do zapewnienia środowiska znany w systemie iOS.

<a name="Model_View_Controller" />

## <a name="model-view-controller-mvc"></a>Model View Controller (MVC)

W [Hello, iOS](~/ios/get-started/hello-ios/index.md) samouczka dowiedzieliśmy się, że aplikacje systemu iOS mają tylko jeden *okna* będące kontrolerów widoku odpowiedzialnym za ładowania ich *zawartości widoku hierarchie* do Okno. W drugim wskazówki Phoneword możemy dodać drugi ekranu do naszej aplikacji i przekazywane niektóre dane — Lista numerów telefonów — między obu ekranach, jak pokazano na poniższym diagramie:

 [![](hello-ios-multiscreen-deepdive-images/08.png "Ten diagram przedstawia przekazywanie danych pomiędzy dwoma ekranów")](hello-ios-multiscreen-deepdive-images/08.png#lightbox)

W naszym przykładzie dane zostały zebrane na pierwszym ekranie przekazane z pierwszego kontrolera widoku do drugiego i wyświetlane w drugiej ekranu. Rozdzielenie ekranów, kontrolerów widoku i danych następuje *modelu widoku, kontroler (MVC)* wzorca. W kolejnych sekcjach kilka omówimy korzyści wzorzec, jego składników i jak możemy używany w naszej aplikacji Phoneword.

### <a name="benefits-of-the-mvc-pattern"></a>Zalety wzorzec MVC

Model-View-Controller jest *wzorca projektowego* — wielokrotnego użytku architektury rozwiązania typowych przypadkach problem lub używania w kodzie. MVC to Architektura aplikacji za pomocą *graficznego interfejsu użytkownika (GUI)*. Przypisuje obiektów w aplikacji, z jedną z trzech ról - *modelu* (logika danych lub aplikacji), *widoku* (interfejsu użytkownika) i *kontrolera* (kod za). Na poniższym diagramie przedstawiono relacje między trzy wzorzec MVC i użytkownika:

 [![](hello-ios-multiscreen-deepdive-images/00.png "Ten diagram przedstawia relacje między trzy wzorzec MVC i użytkownika")](hello-ios-multiscreen-deepdive-images/00.png#lightbox)

Wzorzec MVC jest przydatne, ponieważ zawiera separacji logicznej między poszczególnymi częściami aplikacji graficznego interfejsu użytkownika i ułatwia nam do ponownego użycia w kodzie i widokach. Umożliwia przejście w i spójrz na każdej z trzech ról bardziej szczegółowo.

> [!NOTE]
> Wzorzec MVC jest luźno odpowiednikiem struktury stron ASP.NET lub aplikacji WPF. W tym przykładzie widok jest składnik, który są odpowiedzialne za opisu interfejsu użytkownika i odpowiada strony ASPX (HTML) w programie ASP.NET lub XAML w aplikacji WPF. Kontroler jest składnik, który jest odpowiedzialny za zarządzanie widoku, który odpowiada kodem w programie ASP.NET lub WPF.


### <a name="model"></a>Model

Obiekt modelu jest zazwyczaj specyficzne dla aplikacji reprezentację danych, które ma być wyświetlane lub wprowadzany do widoku. Model jest często słabo zdefiniowano — na przykład w naszym **Phoneword_iOS** modelu jest aplikacja, listę numerów telefonów (reprezentowane jako lista ciągów). Firma Microsoft zostały kompilowania aplikacji i platform, firma Microsoft można wybrać opcji udostępniania **PhonewordTranslator** kod między systemów iOS i Android aplikacje. Można naszym zdaniem udostępnionego kodu jako również modelu.

MVC jest całkowicie niezależny od *trwałości danych* i *dostępu* modelu. Innymi słowy, MVC nowe zależy co naszych danych prawdopodobnie lub jego przechowywania, jak danych jest tylko *reprezentowane*. Na przykład firma może wybrać do przechowywania danych w bazie danych SQL lub zachować go w mechanizmu magazynu w chmurze lub po prostu użyć `List<string>`. Do celów MVC reprezentację danych, sama znajduje się we wzorcu.

W niektórych przypadkach może być pusty Model część platformy MVC. Na przykład wybieramy opcję Dodaj niektórych statycznych stron do naszej aplikacji wyjaśniający, jak działa translator telefonu, dlaczego budujemy oraz sposobu skontaktuj się z nami usterki raportu. Ekrany tych aplikacji będą nadal utworzone przy użyciu widoków i kontrolerów, ale nie będzie zawierało wszystkie dane modelu.

> [!NOTE]
> W niektórych materiały części modelu wzorzec MVC mogą odwoływać się do całej aplikacji wewnętrznej bazy danych, nie tylko dane wyświetlane w interfejsie użytkownika. W tym przewodniku używamy nowoczesnych interpretacja modelu, ale rozróżnienie nie jest szczególnie istotna.


### <a name="view"></a>Widok

Widok jest składnik, który jest odpowiedzialny za renderowania interfejsu użytkownika. W niemal wszystkich platform korzystających ze wzorca MVC, w interfejsie użytkownika składa się z hierarchii widoków. Firma Microsoft można traktować jako widok hierarchii z jednym widokiem — znane jako widok główny - u góry hierarchii i dowolną liczbę widoków podrzędnych widok MVC (nazywane lub widoków podrzędnych) poniżej. W systemie iOS składnik widoku MVC odpowiada hierarchii widok zawartości ekranu.

### <a name="controller"></a>Kontrolera

Obiekt kontrolera jest składnikiem ze sobą wszystkie elementy wiążący, który jest reprezentowana w systemie iOS przez `UIViewController`. Naszym zdaniem kontrolera jako zapasowy kod ekranu lub zestaw widoków. Kontroler jest odpowiedzialny za nasłuchiwanie żądań od użytkownika i zwracanie hierarchii odpowiedni widok. Nasłuchuje żądań z widoku (kliknięć przycisków, wprowadzania tekstu, itp.), a wykonuje odpowiednie przetwarzania modyfikacji widoku i ponownie załadowanie widoku. Kontroler jest również odpowiedzialne za tworzenie lub pobierania modelu z dowolnego zapasowy magazyn danych istnieje w aplikacji i wypełnianie widoku jego dane.

Kontrolery można również zarządzać kontrolerów. Na przykład jeden kontroler może załadować innego kontrolera, jeśli należy wyświetlić inny ekran lub stosie, kontrolerów, aby monitorować ich kolejność oraz przejść między nimi zarządzać. W następnej sekcji zajmiemy się tym przykładem kontrolera, który zarządza innymi kontrolerami jako wprowadzeniu specjalny typ iOS o nazwie kontrolera widoku *kontrolera nawigacji*.

## <a name="navigation-controller"></a>Kontroler nawigacji

W aplikacji Phoneword użyliśmy *kontrolera nawigacji* pomagające w zarządzaniu nawigacji między kilka ekranów. Kontroler nawigacji jest specjalistycznej `UIViewController` reprezentowany przez `UINavigationController` klasy. Zamiast zarządzania pojedynczą hierarchię widok zawartości, kontrolera nawigacji zarządza inne kontrolery widoku, a także własną specjalne hierarchii widok zawartości w formie narzędzi nawigacji, który zawiera tytuł, przycisku Wstecz i inne funkcje opcjonalne.

Kontroler nawigacji jest typowe w przypadku aplikacji systemu iOS oraz udostępnia nawigowanie dla aplikacji systemu iOS zszywania, takich jak **ustawienia** aplikacji, jak pokazano na poniższym zrzucie ekranu:

 [![](hello-ios-multiscreen-deepdive-images/01.png "Kontroler nawigacji udostępnia nawigowanie w aplikacjach systemu iOS, takie jak aplikacja ustawienia pokazane")](hello-ios-multiscreen-deepdive-images/01.png#lightbox)

Nawigacji kontroler trzy podstawowe funkcje:

-  **Udostępnia punkty zaczepienia dla nawigacji do przodu** — kontroler nawigacji używa metaphor hierarchiczna nawigacji, gdzie zawartości widoku hierarchie są *wypchniętych* na *stos nawigacji* . Stos nawigacji można traktować jako stos karty do gry, w których tylko górna karta większości jest widoczny, jak pokazano na poniższym diagramie:  

    [![](hello-ios-multiscreen-deepdive-images/02.png "Ten diagram przedstawia nawigacji jako stos kart")](hello-ios-multiscreen-deepdive-images/02.png#lightbox)


-  **Opcjonalnie zawiera przycisk Wstecz** — gdy firma Microsoft push nowy element na stosie nawigacji na pasku tytułu może automatycznie wyświetlane *przycisku Wstecz* umożliwiająca użytkownikowi Przejdź wstecz. Naciśnięcie przycisku Wstecz *POP* bieżący kontroler widoku stos nawigacji i obciążeń poprzedniej hierarchii widok zawartości do okna:  

    [![](hello-ios-multiscreen-deepdive-images/03.png "Ten diagram przedstawia wyświetlanie karty ze stosu")](hello-ios-multiscreen-deepdive-images/03.png#lightbox)


-  **Pasek tytułu zawiera** — górnej części **kontrolera nawigacji** jest nazywany *paska tytułu* . Odpowiada za wyświetlanie tytułu kontrolera widoku, jak pokazano na poniższym diagramie:  

    [![](hello-ios-multiscreen-deepdive-images/04.png "Na pasku tytułu jest odpowiedzialny za wyświetlanie tytułu kontrolera widoku")](hello-ios-multiscreen-deepdive-images/04.png#lightbox)




### <a name="root-view-controller"></a>Kontroler widoku głównego

A **kontrolera nawigacji** nie Zarządzanie hierarchii zawartości widoku, dzięki go nie ma nic do wyświetlenia samodzielnie.
Zamiast tego **kontrolera nawigacji** wraz z *kontrolera widoku głównego*:

 [![](hello-ios-multiscreen-deepdive-images/05.png "Kontroler nawigacji łączyć się z kontrolerem widoku głównego")](hello-ios-multiscreen-deepdive-images/05.png#lightbox)

Kontroler widoku głównego reprezentuje pierwszy kontroler widoku **kontrolera nawigacji** stosu, a kontroler widoku głównego zawartości widoku hierarchii jest pierwszy hierarchii widok zawartości do załadowania do okna. Jeśli chcemy, aby umieścić naszych całej aplikacji na stosie kontrolera nawigacji, możesz teraz przystąpić Sourceless Segue do **kontrolera nawigacji** i ustaw kontroler widoku naszym pierwszym ekranie jako kontroler widoku głównego, jak robiliśmy Phoneword aplikacji:

 [![](hello-ios-multiscreen-deepdive-images/06.png "Sourceless Segue ustawia ekrany pierwszy kontroler widoku jako kontroler widoku głównego")](hello-ios-multiscreen-deepdive-images/06.png#lightbox)

### <a name="additional-navigation-options"></a>Opcje dodatkowe nawigacji

**Kontrolera nawigacji** jest często stosowana metoda obsługi nawigacji w systemie iOS, ale nie jest jedyną opcją. A [kontrolera pasek kartę](~/ios/user-interface/controls/creating-tabbed-applications.md) można podzielić aplikacji w różnych obszarów funkcjonalnych; [kontrolera widoku podziału](https://developer.xamarin.com/recipes/ios/content_controls/split_view/use_split_view_to_show_two_controllers) można tworzyć widoki wzorzec/szczegół; i [kontrolera nawigacji wysuwane](http://components.xamarin.com/view/flyoutnavigation) tworzy nawigacji, które użytkownik może palcem ze strony. Wszystkie te można łączyć z **kontrolera nawigacji** dla to intuicyjne rozwiązanie w prezentowanie zawartości.

## <a name="handling-transitions"></a>Obsługa przejścia

W przewodniku Phoneword możemy obsługi przejścia między dwa kontrolery widok na dwa sposoby — najpierw z Segue scenorysu, a następnie programowo. Przyjrzyjmy się obie te opcje bardziej szczegółowo.

### <a name="prepareforsegue"></a>PrepareForSegue

Gdy dodamy Segue z **Pokaż** akcji do scenorysu, możemy poinstruować iOS wypychanej drugiego kontrolera widoku na stosie kontrolera nawigacji:

 [![](hello-ios-multiscreen-deepdive-images/09.png "Ustawienie typu segue z listy rozwijanej")](hello-ios-multiscreen-deepdive-images/09.png#lightbox)

Dodawanie Segue do scenorysu jest wystarczająca liczba na tworzenie prostych przejścia między ekranami. Jeśli chcemy przekazać dane między kontrolerami widoku, konieczne jest zastąpienie `PrepareForSegue` metody i obsługi danych, nad:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);
    ...
}
```

wywołania z systemem iOS `PrepareForSegue` przed przejścia występuje i przekazuje Segue, utworzonej w scenorysu do metody.
W tym momencie mamy ręcznie ustawić Segue docelowego kontrolera widoku. Poniższy kod pobiera dojścia do docelowego kontrolera widoku i rzutuje odpowiednią klasą - CallHistoryController, w tym przypadku:

```csharp
CallHistoryController callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Na koniec listy numerów telefonów (Model) jest przekazywana z `ViewController` do `CallHistoryController` przez ustawienie `PhoneHistory` właściwość `CallHistoryController` do listy numerów telefonów wybieranego:

```csharp
callHistoryContoller.PhoneNumbers = PhoneNumbers;
```

Kompletny kod dla przekazywania danych za pomocą Segue wygląda następująco:

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

### <a name="navigation-without-segues"></a>Bez Segues nawigacji

Przejście z pierwszego kontrolera widoku na sekundę w kodzie jest te same czynności co, Segue, ale kilka kroków trzeba przeprowadzić ręcznie.
Najpierw używamy `this.NavigationController` Aby odwołać się do kontrolera nawigacji którego stosu Pracujemy obecnie włączone. Następnie możemy użyć kontrolera nawigacji `PushViewController` metody ręcznego przekazywania dalej kontroler widoku na stosie, przekazując kontroler widoku i opcję animacji przejścia (możemy Ustaw tę wartość na `true`).

Poniższy kod obsługi przejścia na ekranie Phoneword do ekranu Historia połączeń:

```csharp
this.NavigationController.PushViewController (callHistory, true);
```

Zanim firma Microsoft może przejść do następnego widoku kontrolera, musimy wystąpienia go ręcznie z scenorysu, wywołując `this.Storyboard.InstantiateViewController` i przekazując identyfikator scenorysu `CallHistoryController`:

```csharp
CallHistoryController callHistory =
this.Storyboard.InstantiateViewController
("CallHistoryController") as CallHistoryController;
```

Na koniec listy numerów telefonów (Model) jest przekazywana z `ViewController` do `CallHistoryController` przez ustawienie `PhoneHistory` właściwość `CallHistoryController` do listy numerów telefonów wybieranego, tak samo, jak robiliśmy możemy podczas przejścia z Segue:

```csharp
callHistory.PhoneNumbers = PhoneNumbers;
```

Kompletny kod dla przejścia programowe jest następujący:

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

Aplikacja Phoneword wprowadzono kilka koncepcji, które nie są uwzględnione w tym przewodniku. Pojęcia te obejmują:

-  **Automatyczne tworzenie widoku kontrolerów** — kiedy możemy wprowadź nazwę klasy kontrolera widoku w **konsoli właściwości** , Projektant iOS sprawdza Jeśli tej klasy istnieje, a następnie generuje kontroler widoku kopii klasy firmie Microsoft. Aby uzyskać więcej informacji na ten temat oraz inne funkcje projektanta iOS dotyczą [wprowadzenie do projektanta dla systemu iOS](~/ios/user-interface/designer/introduction.md) przewodnik.
-  **Tabela kontrolera widoku** — `CallHistoryController` kontroler widoku tabeli. Kontroler widoku tabeli zawiera widok tabeli, najczęściej układ i dane wyświetlić narzędzie w systemie iOS. Tabele wykraczają poza zakres tego podręcznika. Aby uzyskać więcej informacji na kontrolerach widok tabeli, zapoznaj się [Praca z tabelami i komórkami](~/ios/user-interface/controls/tables/index.md) przewodnik.
-   **Identyfikator scenorysu** — ustawienie Identyfikatora scenorysu tworzy klasę kontrolera widoku w języku Objective-C, zawierający kodem kontrolera widoku w scenorysu. Możemy użyć Identyfikatora scenorysu można znaleźć klasy Objective-C i Utwórz wystąpienie kontrolera widoku w scenorysu. Aby uzyskać więcej informacji na identyfikatorów scenorysu, zapoznaj się [wprowadzenie do scenorysu](~/ios/user-interface/storyboards/index.md) przewodnik.


## <a name="summary"></a>Podsumowanie

Gratulacje, zakończyła się swoją pierwszą aplikację iOS wielu ekranu!

W tym przewodniku możemy wprowadzono wzorzec MVC i umożliwia tworzenie wielu ekranowanej aplikacji. Możemy również zbadane kontrolerów nawigacji i ich role w Włączanie nawigacji dla systemu iOS. Masz teraz solidną podstawę, należy rozpocząć tworzenie aplikacji platformy Xamarin.iOS.

Następnie nauczysz się, jak tworzenie wieloplatformowych aplikacji za pomocą platformy Xamarin z [wprowadzenie do programowania aplikacji mobilnych](~/cross-platform/get-started/introduction-to-mobile-development.md) i [tworzenie aplikacji wieloplatformowych](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) przewodników.


## <a name="related-links"></a>Linki pokrewne

- [Witaj, iOS (przykład)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [System iOS w portalu inicjowania obsługi](https://developer.apple.com/ios/manage/overview/index.action)
