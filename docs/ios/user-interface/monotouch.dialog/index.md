---
title: Wprowadzenie do elementu MonoTouch.Dialog dla platformy Xamarin.iOS
description: W tym dokumencie opisano elementu MonoTouch.Dialog (góra MT.) D), umożliwiająca szybkie, deklaratywne projektowania interfejsu użytkownika z rozszerzeniem Xamarin.iOS. Omówiono w nim sposób korzystania z interfejsów API elementu MonoTouch.Dialog Tworzenie interfejsu w kodzie lub w formacie JSON i używanie funkcji, takich jak ściągnięcia do odświeżenia, wyszukiwania, ładowanie obrazu tła i.
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bee4b460552c7273021b16955b52ba3d95d3e07c
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038407"
---
# <a name="introduction-to-monotouchdialog-for-xamarinios"></a>Wprowadzenie do elementu MonoTouch.Dialog dla platformy Xamarin.iOS

Elementu MonoTouch.Dialog, określane jako patrz hasło MT. D w skrócie, to szybkie interfejsu użytkownika narzędzi, który umożliwia deweloperom tworzenie ekranów w aplikacji i nawigowanie przy użyciu informacji zamiast monotonnych czynności tworzenia kontrolerów widoku, tabelach itp. W efekcie zapewnia znaczne uproszczenie redukcji rozwoju i kodu interfejsu użytkownika. Na przykład rozważmy poniższy zrzut ekranu:

 [![](images/image1.png "Rozważmy na przykład na tym zrzucie ekranu")](images/image1.png#lightbox)

Następujący kod został użyty do definiowania tego cały ekran:

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}
        
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;
    [Section("Expense Details")]
  
    [Caption("Description")]
    [Entry]
    public string Details;
        
    [Checkbox]
    public bool IsApproved = true;
    [Caption("Category")]
    public Category ExpenseCategory;
}
```

Podczas pracy z tabelami w systemie iOS, często istnieje wiele monotonnych kodu.
Na przykład za każdym razem, gdy potrzebna jest tabela, źródła danych jest potrzebna do wypełniania tej tabeli. W aplikacji, która ma dwa ekrany na podstawie tabeli, które są połączone za pośrednictwem kontrolera nawigacji każdy ekran udostępnia wiele ten sam kod.

PATRZ HASŁO MT. D upraszcza, hermetyzując cały ten kod do ogólnego interfejsu API do tworzenia tabeli. Zapewnia warstwą abstrakcji nałożoną na tego interfejsu API, który umożliwia deklaratywne obiektu powiązania składnia, która jeszcze łatwiejsze. Jako takie istnieją dwa interfejsy API dostępne w patrz hasło MT. D:

-   **Interfejs API niskiego poziomu elementów** — *interfejsu API elementów* opiera się na tworzenie hierarchii drzewa elementów reprezentujących ekranów oraz ich składników. Elementy interfejsu API zapewnia deweloperom większą elastyczność i kontrolę w tworzeniu interfejsów użytkownika. Ponadto interfejsu API elementów ma zaawansowaną obsługę definicji deklaratywne, przy użyciu wartości JSON, która umożliwia zarówno bardzo szybka deklaracji, jak i dynamiczne generowanie interfejsu użytkownika z serwera. 
-   **Interfejsu API odbicia wysokiego poziomu** — znanej także jako *powiązanie**API* , w klas, które posiadają adnotację, za pomocą wskazówki dotyczące interfejsu użytkownika, a następnie patrz hasło MT. D automatycznie tworzy ekrany na podstawie obiektów i zapewnia powiązanie między co jest wyświetlane (i opcjonalnie edytować) na ekranie i podstawowych obiektów, tworzenia kopii.   W powyższym przykładzie pokazano użycie interfejsu API odbicia. Ten interfejs API nie zapewnia precyzyjną kontrolę, który wykonuje elementów interfejsu API, ale zmniejsza złożoność jeszcze bardziej, automatycznie tworząc poziomie hierarchii elementów na podstawie atrybutów klasy. 


PATRZ HASŁO MT. D pochodzi upakowaną z dużym zestawem wbudowane elementy interfejsu użytkownika dla tworzenia ekranu, ale również rozpoznaje potrzebę dostosowane elementy i układów ekranu Zaawansowane. W efekcie możliwość rozbudowy jest teraz najwyższej jakości polecanych ramach interfejsu API. Deweloperzy mogą rozszerzać istniejące elementy lub tworzenie nowych i zapewnić bezproblemową integrację.

Ponadto patrz hasło MT. D ma szereg typowych funkcji środowiska użytkownika dla systemu iOS wbudowane, takie jak "pull odświeżania" obsługuje asynchroniczne ładowania, obrazu i wyszukiwania pomocy technicznej.

W tym artykule zajmie kompleksowych informacji o pracy z patrz hasło MT. D, w tym:

-   **PATRZ HASŁO MT. Składniki D** — to koncentruje się na opis klas, które tworzą patrz hasło MT. D, aby umożliwić biegłości szybko. 
-   **Dokumentacja elementów** — uzyskać pełną listę wbudowanych elementów MT. D. 
-   **Zaawansowane zastosowania** — w tym artykule opisano funkcje zaawansowane, takie jak ściągnięcia do odświeżenia, wyszukiwania, ładowania obrazu tła, za pomocą LINQ do skompilowania elementu hierarchie i tworzenie elementów niestandardowych, komórki, a kontrolery za pomocą patrz hasło MT. D. 

## <a name="setting-up-mtd"></a>Konfigurowanie patrz hasło MT. D

PATRZ HASŁO MT. D jest rozpowszechniana z platformy Xamarin.iOS. Aby go użyć, kliknij prawym przyciskiem myszy **odwołania** węzła Xamarin.iOS projektu w programie Visual Studio 2017 lub Visual Studio dla komputerów Mac i Dodaj odwołanie do **1 elementu MonoTouch.Dialog** zestawu. Następnie należy dodać `using MonoTouch.Dialog` instrukcji w źródle kodu zgodnie z potrzebami.

## <a name="understanding-the-pieces-of-mtd"></a>Opis elementów MT. D

Nawet wtedy, gdy za pomocą odbicia interfejsu API, patrz hasło MT. D tworzy hierarchia elementów kulisy, tak jak w przypadku bezpośrednio, jakby zostały utworzone za pomocą interfejsu API elementów. Ponadto obsługą notacji JSON, wymienione w poprzedniej sekcji tworzy elementy również. Z tego powodu warto mieć podstawową wiedzę na temat elementów składowych MT. D.

PATRZ HASŁO MT. D kompilacje ekranów za pomocą następujących czterech części:

-  **DialogViewController**
-  **Obiektów RootElement**
-  **Sekcja**
-  **Element**


### <a name="dialogviewcontroller"></a>DialogViewController

A *DialogViewController*, lub *DVC* w skrócie, dziedziczy `UITableViewController` i reprezentuje zatem ekranu za pomocą tabeli. DVCs wypychania do kontrolera nawigacji, podobnie jak UITableViewController regularne.

### <a name="rootelement"></a>Obiektów RootElement

A *obiektów RootElement* jest kontenerem najwyższego poziomu dla elementów, które bardziej szczegółowo DVC. Zawiera sekcje, które następnie mogą zawierać elementy. Nie są renderowane RootElements; Zamiast tego są one po prostu kontenerami dla co faktycznie pobiera renderowane. Obiektów RootElement jest przypisany do DVC, a następnie DVC powoduje wyświetlenie jego elementów podrzędnych.

### <a name="section"></a>Sekcja

Sekcja jest grupą komórek w tabeli. Zgodnie z sekcją normalne tabeli mogą opcjonalnie mieć nagłówku i stopce, które mogą być tekstem lub nawet niestandardowe widoki, tak jak w poniższym zrzucie ekranu:

 [![](images/image2.png "Zgodnie z sekcją normalne tabeli mogą opcjonalnie mieć nagłówku i stopce, które mogą być tekstem lub nawet niestandardowe widoki, tak jak w tym zrzucie ekranu")](images/image2.png#lightbox)

### <a name="element"></a>Element

Element reprezentuje rzeczywisty komórka w tabeli. PATRZ HASŁO MT. D jest dostarczany upakowaną z szerokiej gamy elementów, które reprezentują różne typy danych lub inne dane wejściowe. Na przykład następujące zrzuty ekranu przedstawiają kilka dostępne elementy:

 [![](images/image3.png "Na przykład ten zrzuty ekranu pokazują niektóre z dostępnych elementów")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>W sekcji coraz RootElements

Teraz omówimy RootElements i sekcje bardziej szczegółowo.

### <a name="rootelements"></a>RootElements

Co najmniej jeden obiektów RootElement jest wymagana do uruchamiania procesu elementu MonoTouch.Dialog.

Jeśli obiektów RootElement jest inicjowany z wartością section/element ta wartość jest używana do lokalizowania elementem podrzędnym elementu, który zapewni Podsumowanie konfiguracji, który jest renderowany po prawej stronie ekranu. Na przykład poniższy zrzut ekranu przedstawia tabelę po lewej stronie z komórki zawierającej tytuł ekran szczegółów po prawej stronie, "Dessert", wraz z wartości wybranych pustynia.

 [![](images/image4.png "Ten zrzut ekranu przedstawia tabelę po lewej stronie z komórki zawierającej tytuł ekran szczegółów po prawej stronie, deserów, wraz z wartości wybranych pustynia") ](images/image4.png#lightbox) [ ![ ] (images/image5.png "to Poniższy zrzut ekranu przedstawia tabelę po lewej stronie z komórki zawierającej tytuł ekran szczegółów po prawej stronie, deserów, wraz z wartości wybranych pustynia")](images/image5.png#lightbox)

Elementy główne można również wewnątrz sekcji do wyzwolenia ładowania nowej stronie konfiguracji zagnieżdżonych, jak pokazano powyżej. Gdy jest używana w tym trybie podpis pod warunkiem jest używana podczas renderowania wewnątrz sekcji, a także jest używane jako tytuł dla podstrony. Na przykład:

```csharp
var root = new RootElement ("Meals") {
    new Section ("Dinner"){
            new RootElement ("Dessert", new RadioGroup ("dessert", 2)) {
                new Section () {
                    new RadioElement ("Ice Cream", "dessert"),
                    new RadioElement ("Milkshake", "dessert"),
                    new RadioElement ("Chocolate Cake", "dessert")
                }
            }
        }
    }
```

W powyższym przykładzie kiedy użytkownik naciska na "Dessert" elementu MonoTouch.Dialog będzie Utwórz nową stronę i przejdź do jej z elementem głównym jest "Dessert", a po grupie opcji z trzech wartości.

W tym przykładzie określonej grupy opcji wybierze "Ciasto czekoladowe" w sekcji "Dessert", ponieważ firma Microsoft przekazana wartość "2" do radiogroup widoku. Oznacza to, wybierz 3rd elementu na liście (0 index).

Wywoływanie metody Add lub przy użyciu składni inicjatora w języku C# 4 dodaje sekcje.
Metody Insert są dostarczane do wstawienia sekcje z animacji.

W przypadku utworzenia obiektów RootElement wystąpienie grupy, (a nie radiogroup widoku) wartość obiektów RootElement pojawi się w sekcji podsumowania będzie łącznej liczby wszystkich BooleanElements i CheckboxElements, który ma ten sam klucz jako wartość Group.Key.

### <a name="sections"></a>Sekcje

Sekcje są używane w celu zgrupowania elementów na ekranie, i są jedynymi prawidłowymi bezpośrednie elementy podrzędne obiektów RootElement. Sekcje mogą zawierać dowolne standardowe elementy, w tym nowe RootElements.

RootElements osadzone w sekcji są używane do przejścia na nowy poziom głębiej.

Sekcje mogą mieć nagłówki i stopki jako ciągi lub jako UIViews.
Zwykle po prostu użyje ciągi, ale do tworzenia niestandardowych interfejsów użytkownika, można użyć dowolnego UIView jako nagłówka lub stopki. Możesz użyć ciągu do ich utworzenia w następujący sposób:

```csharp
var section = new Section ("Header", "Footer")
```

Aby korzystać z widoków, po prostu Przekaż widoków do konstruktora:

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>Otrzymywanie powiadomień

#### <a name="handling-nsaction"></a>Obsługa NSAction

PATRZ HASŁO MT. Powierzchnie D `NSAction` jako pełnomocnik obsługi wywołań zwrotnych.
Na przykład załóżmy, że chcesz obsługiwać to zdarzenie touch dla utworzonych przez patrz hasło MT. komórki tabeli D. Podczas tworzenia elementu przy użyciu patrz hasło MT. D, po prostu dostarczają funkcji wywołania zwrotnego, jak pokazano poniżej:

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>Pobieranie wartości elementu

W połączeniu z `Element.Value` właściwość wywołania zwrotnego można pobrać wartość ustawiona w innych elementów. Na przykład rozważmy następujący kod:

```csharp
var element = new EntryElement (task.Name, "Enter task description",
        task.Description);
                
var taskElement = new RootElement (task.Name){
        new Section () { element },
        new Section () { 
                new DateElement ("Due Date", task.DueDate)
        },
        new Section ("Demo Retrieving Element Value") {
                new StringElement ("Output Task Description", 
                        delegate { Console.WriteLine (element.Value); })
        }
};
```

Ten kod tworzy interfejs użytkownika, jak pokazano poniżej. Aby uzyskać szczegółowy przewodnik, w tym przykładzie, zobacz [elementów interfejsu API wskazówki](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) samouczka.

 [![](images/image6.png "W połączeniu z właściwością Element.Value wywołania zwrotnego można pobrać wartość ustawiona w innych elementów")](images/image6.png#lightbox)

Gdy użytkownik naciśnie dolnej komórki tabeli, kod w funkcji anonimowych wykonuje, zapisu wartości z `element` wystąpienia do **dane wyjściowe aplikacji** konsoli w programie Visual Studio dla komputerów Mac.

## <a name="built-in-elements"></a>Wbudowanych elementów

PATRZ HASŁO MT. D jest powiązana z określonej liczby elementów komórki wbudowana Tabela znane jako elementy.
Te elementy są używane do wyświetlania różnymi typami w komórkach tabeli, takich jak ciągi, wartości zmiennoprzecinkowe, dat i nawet, obrazów, aby nazwać kilku. Każdy element zajmuje się odpowiednio wyświetlania typu danych. Na przykład elementu boolean wyświetli przełącznika, aby przełączyć jego wartość. Podobnie float element wyświetli suwak, aby zmienić wartość zmiennoprzecinkową.

Istnieją jeszcze bardziej złożonych elementów, które umożliwiają bardziej rozbudowane typy danych, takich jak obrazy i html. Na przykład element html, który otworzy UIWebView ładowania strony sieci web po wybraniu Wyświetla podpis w komórce tabeli.

### <a name="working-with-element-values"></a>Praca z wartościami — Element

Elementy, które są używane do przechwytywania danych wejściowych użytkownika udostępnienia publicznego `Value` właściwość, która zawiera bieżącą wartość elementu w dowolnym momencie. Jest automatycznie aktualizowana, ponieważ użytkownik używa aplikacji.

Ta wartość jest stosowana dla wszystkich elementów, które są częścią elementu MonoTouch.Dialog, ale nie jest wymagane elementy utworzone przez użytkownika.

### <a name="string-element"></a>Element ciągu

Element `StringElement` zawiera podpis po lewej stronie w komórce tabeli i wartość ciągu w komórce po prawej stronie.

 [![](images/image7.png "StringElement zawiera podpis po lewej stronie w komórce tabeli i wartość ciągu w komórce po prawej stronie")](images/image7.png#lightbox)

Aby użyć `StringElement` jako przycisk, podasz delegata.

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [![](images/image8.png "Aby użyć StringElement jako przycisk, podasz delegata")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>Element ze stylem ciągu

A `StyledStringElement` umożliwia ciągów, które mają zostać wyświetlone style komórki albo wbudowana tabela lub za pomocą formatowania niestandardowego.

 [![](images/image9.png "StyledStringElement umożliwia ciągów, które mają zostać wyświetlone style komórki albo wbudowana tabela lub niestandardowe formatowanie")](images/image9.png#lightbox)

`StyledStringElement` Klasa pochodzi od `StringElement`, ale umożliwia deweloperom dostosowywanie niewielki podzbiór właściwości, takie jak czcionkę, kolor tekstu, kolor komórki w tle, tryb podziału wiersza, liczba wierszy do wyświetlenia, i czy powinien zostać wyświetlony element akcesoriów.

### <a name="multiline-element"></a>Wielowierszowy Element

 [![](images/image10.png "Wielowierszowy Element")](images/image10.png#lightbox)

### <a name="entry-element"></a>Entry Element

`EntryElement`, Jak nazwa wskazuje, że jest używana do pobierania danych wejściowych użytkownika. Obsługuje ona zwykłe ciągi lub haseł, w którym znaki są ukryte.

 [![](images/image11.png "EntryElement jest używana do pobierania danych wejściowych użytkownika")](images/image11.png#lightbox)

Jest on inicjowany z trzech wartości:

-  Podpis dla wpisu, która będzie widoczna dla użytkownika.
-  Tekst symbolu zastępczego (jest to tekst wyszarzony, który stanowi wskazówkę dla użytkownika). 
-  Wartość tekstu.


Symbol zastępczy i wartość może mieć wartości null. Jednak tytuł jest wymagany.

W dowolnym momencie uzyskiwania dostępu do jego wartości właściwości można pobrać wartość `EntryElement`.

Ponadto `KeyboardType` właściwość można ustawić w czasie tworzenia na styl czcionki klawiatury, które są konieczne do wprowadzania danych. To może służyć do konfigurowania klawiatury, za pomocą wartości `UIKeyboardType` wymienione poniżej:

-  Numeryczne
-  Telefon
-  adres URL
-  Adres e-mail


### <a name="boolean-element"></a>Wartość logiczna elementu

 [![](images/image12.png "Wartość logiczna elementu")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>Element pola wyboru

 [![](images/image13.png "Element pola wyboru")](images/image13.png#lightbox)

### <a name="radio-element"></a>Element radiowych

A `RadioElement` wymaga `RadioGroup` należy określić w `RootElement`.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [![](images/image14.png "RadioElement wymaga radiogroup widoku, należy określić w obiektów RootElement")](images/image14.png#lightbox)

 `RootElements` są również używane do zapewnienia koordynacji elementy radio. `RadioElement` Członkowie mogą znajdować się na wiele sekcji (na przykład zaimplementować podobne do selektor sygnału pierścienia i barwy oddzielne pierścień niestandardowe z dzwonków systemu). Widok podsumowania pokaże element radiowego, który jest aktualnie wybrany. Aby użyć tej opcji, należy utworzyć `RootElement` za pomocą konstruktora grupy w następujący sposób:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

Nazwa grupy w `RadioGroup` służy do pokazywania, wybranej wartości na stronie zawierającej (jeśli istnieje) i wartość, która w tym przypadku wynosi zero, to indeks pierwszego wybranego elementu.

### <a name="badge-element"></a>Element karty identyfikacyjnej

 [![](images/image15.png "Element karty identyfikacyjnej")](images/image15.png#lightbox)

### <a name="float-element"></a>Float — Element

 [![](images/image16.png "Float — Element")](images/image16.png#lightbox)

### <a name="activity-element"></a>Działanie elementu

 [![](images/image17.png "Działanie elementu")](images/image17.png#lightbox)

### <a name="date-element"></a>Data — Element

 ![](images/image18.png "Data — Element")

Po wybraniu komórki odpowiadające DateElement zostanie wyświetlony selektor daty, jak pokazano poniżej:

 [![](images/image19.png "Po wybraniu komórki odpowiadające DateElement selektor daty, zostanie wyświetlony pokazany")](images/image19.png#lightbox)

### <a name="time-element"></a>Time Element

 [![](images/image20.png "Time Element")](images/image20.png#lightbox)

Po wybraniu komórki odpowiadające TimeElement zostanie wyświetlony selektor czasu, jak pokazano poniżej:

 [![](images/image21.png "Po wybraniu komórki odpowiadające TimeElement selektor czasu, zostanie wyświetlony pokazany")](images/image21.png#lightbox)

### <a name="datetime-element"></a>DateTime Element

 [![](images/image22.png "DateTime Element")](images/image22.png#lightbox)

Po wybraniu komórki odpowiadające DateTimeElement zostanie wyświetlony selektor daty/godziny, jak pokazano poniżej:

 [![](images/image23.png "Po wybraniu komórki odpowiadające DateTimeElement selektora daty/godziny, zostanie wyświetlony pokazany")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML Element

 [![](images/image24.png "HTML Element")](images/image24.png#lightbox)

`HTMLElement` Wyświetlana jest wartość jego `Caption` właściwość w komórce tabeli. Zaznaczone, gdzie `Url` przypisać do elementu jest ładowany w `UIWebView` kontrolować, jak pokazano poniżej:

 [![](images/image25.png "Gdzie zaznaczone, adres Url, przypisany do elementu jest załadowanego w kontrolce UIWebView, jak pokazano poniżej")](images/image25.png#lightbox)

### <a name="message-element"></a>Element komunikatu

 [![](images/image26.png "Element komunikatu")](images/image26.png#lightbox)

### <a name="load-more-element"></a>Załaduj więcej — Element

Użyj tego elementu, aby umożliwić użytkownikom załadować więcej elementów na liście. Można dostosować normalnych i podpisy ładowanie, a także kolor czcionki i tekstu.
`UIActivity` Wskaźnik rozpoczyna, animowanie, a podpis ładowania jest wyświetlane, gdy użytkownik naciśnie komórki, a następnie `NSAction` przekazany do konstruktora jest wykonywany. Po kodzie w `NSAction` został zakończony, `UIActivity` wskaźnik zatrzymuje, animowanie i normalnym podpis jest wyświetlany ponownie.

### <a name="uiview-element"></a>UIView Element

Ponadto wszelkie niestandardowe `UIView` mogą być wyświetlane przy użyciu `UIViewElement`.

### <a name="owner-drawn-element"></a>Element rysowanych przez właściciela

Ten element musi podklasą klasy, ponieważ jest klasą abstrakcyjną. Należy zastąpić `Height(RectangleF bounds)` metody, w którym powinien zwrócić wysokość elementu, a także `Draw(RectangleF bounds, CGContext context, UIView view)` , w którym należy wykonać na rysunku dostosowanego w danym zakresie przy użyciu parametrów kontekstu i widoku. Ten element jest ciężkie obciążenia podklasy `UIView`i umieszczenie w komórce, która ma zostać zwrócone, pozostawiając tylko konieczności wykonania dwóch prostych zastąpień. Możesz zobaczyć lepsze przykładową implementację w przykładowej aplikacji w `DemoOwnerDrawnElement.cs` pliku.

Poniżej przedstawiono bardzo prosty przykład implementacji klasy:

```csharp
public class SampleOwnerDrawnElement : OwnerDrawnElement
 {
    public SampleOwnerDrawnElement (string text) : base(UITableViewCellStyle.Default, "sampleOwnerDrawnElement")
    {
        this.Text = text;
    }

    public string Text
    {
        get;set;    
    }

    public override void Draw (RectangleF bounds, CGContext context, UIView view)
    {
        UIColor.White.SetFill();
        context.FillRect(bounds);

        UIColor.Black.SetColor();   
        view.DrawString(this.Text, new RectangleF(10, 15, bounds.Width - 20, bounds.Height - 30), UIFont.BoldSystemFontOfSize(14.0f), UILineBreakMode.TailTruncation);
    }

    public override float Height (RectangleF bounds)
    {
        return 44.0f;
    }
 }
```

### <a name="json-element"></a>JSON Element

`JsonElement` Jest podklasą `RootElement` rozszerzający `RootElement` aby można było załadować zawartość zagnieżdżonych elementów podrzędnych z lokalnego lub zdalnego adresu url.

`JsonElement` Jest `RootElement` mogą być utworzone w dwóch formach. Tworzy jedną wersję `RootElement` który załaduje zawartość na żądanie. Są one tworzone przy użyciu `JsonElement` konstruktorów, które przyjmują dodatkowy argument na końcu adresu url do załadowania zawartości z:

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

Druga forma tworzy dane z pliku lokalnego lub istniejącej `System.Json.JsonObject` który ma już analizowany:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

Aby uzyskać więcej informacji na temat korzystania z formatu JSON przy użyciu patrz hasło MT. D, zobacz [wskazówki elementu JSON](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) samouczka.

## <a name="other-features"></a>Inne funkcje

### <a name="pull-to-refresh-support"></a>Obsługę ściągnięcia do odświeżania

 *Ściągnij-do-* *Odśwież* efekt wizualny pierwotnie znajduje się w *Tweetie2* aplikacji, który stał się popularny efekt przez wiele aplikacji.

Aby dodać automatyczna obsługa ściągnięcia do odświeżania z okien dialogowych, wystarczy wykonać dwie czynności: podpinanie program obsługi zdarzeń, aby otrzymywać powiadomienia, gdy użytkownik pobiera dane i powiadom `DialogViewController` po załadowaniu danych Aby powrócić do stanu domyślnego.

Podłączanie powiadomienie jest prosta; wystarczy nawiązać połączenie, aby `RefreshRequested` zdarzenie na `DialogViewController`, podobnie do następującego:

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

Następnie na metodę `OnUserRequestedRefresh`, czy kolejki ładowania niektórych danych, niektóre dane żądania z sieci lub uruchom wątku w celu obliczenia danych. Po załadowaniu danych możesz powiadomić `DialogViewController` nowe dane, a aby przywrócić widok stanu domyślnego, możesz to zrobić, wywołując `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>Wyszukaj pomoc techniczną

Aby obsługiwać, wyszukiwanie, należy ustawić `EnableSearch` właściwości usługi `DialogViewController`. Można również ustawić `SearchPlaceholder` właściwość do użycia jako tekstu znaku wodnego, na pasku wyszukiwania.

Wyszukiwanie zmieni się zawartość widoku jako typy użytkownika. Wyszukuje widoczne pola i pokazuje te dla użytkownika. `DialogViewController` Udostępnia trzy metody, aby programowo zainicjować, kończenie lub Wyzwól nową operację filtru na wyniki. Te metody są wymienione poniżej:

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


System jest rozszerzalny, więc jeśli chcesz, aby zmienić to zachowanie.

### <a name="background-image-loading"></a>Ładowanie obrazów tła

Dołącza elementu MonoTouch.Dialog [TweetStation](https://github.com/migueldeicaza/TweetStation) moduł ładujący obrazy aplikacji. Ten moduł ładujący obrazy może służyć do ładowania obrazów w tle obsługuje buforowanie i może powiadomić Twój kod, gdy obraz został załadowany.

Będzie on także ograniczać liczbę wychodzących połączeń sieciowych.

Moduł ładujący obrazy jest zaimplementowana w `ImageLoader` klasy, wszystko, czego potrzebujesz, aby zrobić to wywołanie `DefaultRequestImage` metody, konieczne będzie podanie identyfikatora Uri obraz, który ma zostać załadowany, a także wystąpienia `IImageUpdated` interfejsu, który będzie wywoływany, gdy obraz zaświadczanie o kondycji s zostały załadowane.

Na przykład poniższy kod ładuje obraz z adresu Url do `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

Klasa ImageLoader udostępnia metody Purge, który można wywołać, jeśli chcesz zwolnić wszystkie obrazy, które są aktualnie w pamięci podręcznej w pamięci. Bieżący kod ma pamięci podręcznej do 50 obrazów. Jeśli chcesz użyć rozmiaru pamięci podręcznej różnych (na przykład, jeśli są oczekiwane obrazów zbyt duży, że obrazy 50 byłoby zbyt dużo), można po prostu utworzyć wystąpienia ImageLoader i przekazać liczbę obrazów, które chcesz przechowywać w pamięci podręcznej.

## <a name="using-linq-to-create-element-hierarchy"></a>Tworzenie hierarchii elementów za pomocą LINQ

Za pomocą Sprytne użycia LINQ i C# w składni inicjowania LINQ może służyć do tworzenia hierarchii elementów. Na przykład, poniższy kod tworzy się ekran z niektórych tablic ciągów i obsługuje komórki wybór za pomocą funkcji anonimowy, który jest przekazywany do każdej z nich `StringElement`:

```csharp
var rootElement = new RootElement ("LINQ root element") {
from x in new string [] { "one", "two", "three" }
select new Section (x) {
from y in "Hello:World".Split (':')
select (Element) new StringElement (y,
delegate { Debug.WriteLine("cell tapped"); })
}
};
```

Można to łatwo połączyć z magazynem danych XML lub dane z bazy danych do utworzenia złożonych aplikacji niemal wyłącznie z danych.

## <a name="extending-mtd"></a>Rozszerzanie patrz hasło MT. D

### <a name="creating-custom-elements"></a>Tworzenie niestandardowych elementów

Możesz utworzyć własne elementu przez dziedziczenie z poziomu istniejącego elementu lub pochodząca z klasy głównego elementu.

Aby utworzyć własny Element, należy zastąpić następujące metody:

```csharp
// To release any heavy resources that you might have
    void Dispose (bool disposing);

    // To retrieve the UITableViewCell for your element
    // you would need to prepare the cell to be reused, in the
    // same way that UITableView expects reusable cells to work
    UITableViewCell GetCell (UITableView tv)

    // To retrieve a "summary" that can be used with
    // a root element to render a summary one level up.  
    string Summary ()
    // To detect when the user has tapped on the cell
    void Selected (DialogViewController dvc, UITableView tableView, NSIndexPath path)
    // If you support search, to probe if the cell matches the user input
    bool Matches (string text)
```

Jeśli Twoje element może mieć zmiennym rozmiarze, musisz zaimplementować `IElementSizing` interfejs, który zawiera jedną metodę:

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

Jeśli planowane jest wdrażanie usługi `GetCell` metoda przez wywołanie metody `base.GetCell(tv)` i dostosowywania zwracany komórki, trzeba także Przesłoń `CellKey` właściwości do zwrócenia klucza, która jest unikatowa dla danego elementu, podobnie do następującego:

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

Ta funkcja działa dla większości elementów, ale nie dla `StringElement` i `StyledStringElement` zgodnie z tymi użyć własnych zestawu kluczy dla różnych scenariuszy renderowania. Trzeba replikować kod w tych klas.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

Odbicie i interfejsu API elementów należy używać tego samego `DialogViewController`. Czasami można dostosować wygląd widoku lub możesz chcieć użyć niektórych funkcji `UITableViewController` który wykracza poza podstawowe Tworzenie interfejsów użytkownika.

`DialogViewController` Jest jedynie podklasą `UITableViewController` i można go dostosować w taki sam sposób, który będzie można dostosować `UITableViewController`.

Jeśli chcesz zmienić styl listy, aby być na przykład `Grouped` lub `Plain`, tę wartość można ustawić, zmieniając właściwość podczas tworzenia kontrolera, następująco:

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

Dla bardziej zaawansowanych dostosowań `DialogViewController`, takie jak Ustawianie tła, jak w przypadku podklas go i zastąpienie właściwego metody, jak pokazano w poniższym przykładzie:

```csharp
class SpiffyDialogViewController : DialogViewController {
    UIImage image;

    public SpiffyDialogViewController (RootElement root, bool pushing, UIImage image) 
        : base (root, pushing) 
    {
        this.image = image;
    }

    public override LoadView ()
    {
        base.LoadView ();
        var color = UIColor.FromPatternImage(image);
        TableView.BackgroundColor = UIColor.Clear;
        ParentViewController.View.BackgroundColor = color;
    }
}
```

Inny punkt dostosowywania są następujące metody wirtualne w `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

Ta metoda powinna zwracać podklasą `DialogViewController.Source` przypadki, w którym Twoja komórki mają równy rozmiar lub podklasa `DialogViewController.SizingSource` Jeśli Twoje komórki są nierówne.

To zastąpienie umożliwia przechwytywanie dowolnego z `UITableViewSource` metody. Na przykład [TweetStation](https://github.com/migueldeicaza/TweetStation) używa tych informacji do śledzenia, gdy użytkownik jest przewijane do góry, a następnie odpowiednio zaktualizować liczba tweetów nieprzeczytane.

## <a name="validation"></a>Walidacja

Elementy nie są oferowane weryfikacji siebie jako modeli, które są odpowiednie dla stron sieci web i aplikacje komputerowe nie są mapowane bezpośrednio do modelu interakcji dla telefonu iPhone.

Jeśli chcesz wykonać sprawdzanie poprawności danych, należy to zrobić, gdy użytkownik wyzwala akcję z danymi wprowadzonymi. Na przykład <span class="ui">gotowe</span> lub <span class="ui">dalej</span> przycisk na górnym pasku narzędzi lub niektóre `StringElement` używany jako przycisk, aby przejść do kolejnego etapu.

Jest to, gdzie możesz wykonać podstawowe sprawdzanie poprawności danych wejściowych i może być skomplikowane więcej sprawdzania poprawności, takich jak sprawdzanie poprawności użytkownika i hasło połączenia z serwerem.

Sposób powiadamiania użytkownika błąd jest specyficzny dla danej aplikacji. Może się pojawiać `UIAlertView` lub wyświetlić wskazówkę.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano wiele informacji na temat elementu MonoTouch.Dialog. Jego omówione podstawy patrz hasło MT. D działa i są objęte różne składniki wchodzące w skład patrz hasło MT. D. Pokazano też szeroką gamę elementów i dostosowania tabeli obsługiwane przez patrz hasło MT. D i omówiono sposób patrz hasło MT. D można rozszerzyć za pomocą niestandardowych elementów. Ponadto wyjaśniono jej obsługą notacji JSON w patrz hasło MT. D, który umożliwia dynamiczne tworzenie elementów z formatu JSON.


## <a name="related-links"></a>Linki pokrewne

- [Zrzut ekranu — Miguel de Icaza tworzy ekran logowania dla systemu iOS przy użyciu elementu MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Zrzut ekranu — łatwe tworzenie dla systemu iOS interfejsy użytkownika przy użyciu elementu MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Przewodnik: tworzenie aplikacji przy użyciu interfejsu API elementów](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Przewodnik: tworzenie aplikacji przy użyciu interfejsu API odbicia](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Przewodnik: tworzenie interfejsu użytkownika przy użyciu elementu JSON](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch.Dialog JSON Markup](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Okno dialogowe MonoTouch w witrynie Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Informacje o klasach UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Informacje o klasach UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
