---
title: Wprowadzenie do MonoTouch.Dialog
description: MonoTouch.Dialog (patrz hasło MT. D) zestaw narzędzi jest niezbędne framework dla aplikacji szybkie programowanie interfejsu użytkownika w Xamarin.iOS. PATRZ HASŁO MT. D umożliwia szybkie i łatwe do definiowania złożonych aplikacji interfejsu użytkownika przy użyciu podejścia deklaratywne zamiast konieczność powtarzania kontrolerów nawigacji, tabelach itp. Ponadto patrz hasło MT. D ma elastyczny zestaw interfejsów API, które zapewniają deweloperom pełną kontrolę lub ręce poza podejście, a także dodatkowe funkcje, takie jak obraz tła ściągnięcia do odświeżania, ładowania, wyszukiwanie pomocy technicznej i dynamiczne generowanie interfejsu użytkownika za pomocą danych JSON. Ten przewodnik przedstawia różne sposoby pracy z patrz hasło MT. D, a następnie dives dogłębną analizę zaawansowanego wykorzystania.
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: be979b35ffdd597dae74f1f661a381ae44433b10
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-monotouchdialog"></a>Wprowadzenie do MonoTouch.Dialog

_MonoTouch.Dialog (patrz hasło MT. D) zestaw narzędzi jest niezbędne framework dla aplikacji szybkie programowanie interfejsu użytkownika w Xamarin.iOS. PATRZ HASŁO MT. D umożliwia szybkie i łatwe do definiowania złożonych aplikacji interfejsu użytkownika przy użyciu podejścia deklaratywne zamiast konieczność powtarzania kontrolerów nawigacji, tabelach itp. Ponadto patrz hasło MT. D ma elastyczny zestaw interfejsów API, które zapewniają deweloperom pełną kontrolę lub ręce poza podejście, a także dodatkowe funkcje, takie jak obraz tła ściągnięcia do odświeżania, ładowania, wyszukiwanie pomocy technicznej i dynamiczne generowanie interfejsu użytkownika za pomocą danych JSON. Ten przewodnik przedstawia różne sposoby pracy z patrz hasło MT. D, a następnie dives dogłębną analizę zaawansowanego wykorzystania._


MonoTouch.Dialog, określany jako patrz hasło MT. D skrócie, jest szybkie toolkit programowanie interfejsu użytkownika, który umożliwia deweloperom tworzenie ekranów w aplikacji i nawigacji przy użyciu informacji zamiast konieczność powtarzania tworzenia widoku kontrolerów, tabelach itp. Tak oferuje znaczne uproszczenie redukcji programowanie i kodu interfejsu użytkownika. Rozważmy na przykład poniższy zrzut ekranu:

 [![](images/image1.png "Rozważmy na przykład tego zrzutu ekranu")](images/image1.png#lightbox)

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

Podczas pracy z tabel w systemie iOS, często istnieje wiele możliwości występują powtarzające kodu.
Na przykład za każdym razem, gdy tabeli jest potrzebny, źródła danych jest potrzebne do wypełniania tej tabeli. W aplikacji, która ma dwa ekrany oparte na tabeli, które są połączone za pośrednictwem kontrolera nawigacji każdy ekran udostępnia wiele tego samego kodu.

PATRZ HASŁO MT. D upraszcza który hermetyzując cały ten kod w ogólnym interfejsem API potrzeby tworzenia tabeli. Udostępnia abstrakcję u góry tego interfejsu API, który umożliwia dla obiekt deklaratywne powiązanie składnię, która jeszcze łatwiejsze. W efekcie dostępnych dwóch interfejsów API w patrz hasło MT. D:

-   **Interfejs API niskiego poziomu elementów** — *elementów interfejsu API* opiera się na tworzenie hierarchiczna drzewa elementów reprezentujących ekranów i ich elementy. Elementy interfejsu API zapewnia deweloperom większą elastyczność i kontroli w tworzeniu UI. Ponadto interfejs API elementów ma zaawansowana obsługa deklaratywne definicji za pomocą formatu JSON, co umożliwia obsługę zarówno deklaracji niezwykle szybka, jak również dynamiczne generowanie interfejsu użytkownika z serwera. 
-   **Ogólny API odbicia** — znanej także jako *powiązanie**interfejsu API* , w które klasy są opatrzoną wskazówek interfejsu użytkownika, a następnie patrz hasło MT. D automatycznie tworzy ekrany oparte na obiekty i zapewnia powiązanie między co jest wyświetlane (i opcjonalnie edytować) na ekranie i odpowiadającego obiektu kopii. W powyższym przykładzie przedstawiono użycie interfejsu API odbicia. Ten interfejs API nie zapewnia precyzyjną kontrolę, który wykonuje elementów interfejsu API, ale zmniejsza złożoność jeszcze bardziej przez automatycznego tworzenia limit hierarchia elementów na podstawie atrybutów klasy. 


PATRZ HASŁO MT. D pochodzi spakowana z dużego zestawu wbudowane elementy interfejsu użytkownika dla tworzenia ekranu, ale również rozpoznaje potrzebę elementy niestandardowe i układów ekranu Zaawansowane. Tak rozszerzalności jest najwyższej jakości umieszczony wypalanej do interfejsu API. Deweloperzy można rozszerzyć istniejące elementy lub utworzyć nowe i bezproblemowo zintegrować.

Ponadto patrz hasło MT. D ma wiele typowych funkcji środowiska użytkownika dla systemu iOS wbudowane, takie jak "pociągnij odświeżyć" obsługuje asynchroniczne ładowania, obrazu i wyszukiwanie pomocy technicznej.

W tym artykule będzie Przyjrzyjmy się kompleksowe Praca z patrz hasło MT. D, w tym:

-   **PATRZ HASŁO MT. Składniki D** — koncentruje się na opis klas, które tworzą patrz hasło MT. D, aby umożliwić pobieranie szybko szybko. 
-   **Dokumentacja elementów** — kompleksowej listy wbudowanych elementów patrz hasło MT. D. 
-   **Zaawansowane użycia** — obejmuje zaawansowane funkcje, takie jak pociągnij, aby odświeżyć, wyszukiwania ładowania obrazu tła, za pomocą LINQ budować element hierarchii i tworzenie niestandardowych elementów, komórek, i kontrolery za pomocą patrz hasło MT. D. 

## <a name="understanding-the-pieces-of-mtd"></a>Opis części patrz hasło MT. D

Nawet wtedy, gdy za pomocą odbicia interfejsu API, patrz hasło MT. D tworzy hierarchia elementów pod maską, tak jakby jego zostały utworzone za pomocą interfejsu API elementy bezpośrednio. Ponadto obsługa JSON wspomniano w poprzedniej sekcji tworzy elementy również. Z tego powodu należy mieć podstawową wiedzę na temat składników części patrz hasło MT. D.

PATRZ HASŁO MT. D kompilacje ekrany przy użyciu następujących czterech części:

-  **DialogViewController**
-  **Elementów RootElement**
-  **Sekcja**
-  **Element**


### <a name="dialogviewcontroller"></a>DialogViewController

A *DialogViewController*, lub *DVC* skrócie, dziedziczy `UITableViewController` i w związku z tym reprezentuje ekranu z tabelą. DVCs może zostać umieszczony na kontrolerze nawigacji, podobnie jak regularne UITableViewController.

### <a name="rootelement"></a>Elementów RootElement

A *elementów RootElement* jest kontenerem najwyższego poziomu dla elementów, które wchodzą w DVC. Zawiera sekcje, które następnie mogą zawierać elementy. Nie są odtwarzane RootElements; Zamiast tego są one po prostu kontenerami co faktycznie pobiera renderowane. Elementów RootElement jest przypisany do DVC, a następnie DVC renderuje jego elementów podrzędnych.

### <a name="section"></a>Sekcja

Sekcja jest grupą komórek w tabeli. Zgodnie z sekcją normalne tabeli, można opcjonalnie stosować nagłówku i stopce strony, która może być tekstem lub nawet niestandardowe widoki, tak jak poniższy zrzut ekranu:

 [![](images/image2.png "Zgodnie z sekcją normalne tabeli, można opcjonalnie stosować nagłówku i stopce strony, która może być tekstem lub nawet niestandardowe widoki, tak jak tego zrzutu ekranu")](images/image2.png#lightbox)

### <a name="element"></a>Element

Element reprezentuje rzeczywisty komórka w tabeli. PATRZ HASŁO MT. D pochodzi spakowana wiele różnych elementów reprezentujących różne typy danych lub różnych danych wejściowych. Na przykład poniższe zrzuty ekranu przedstawiają kilka dostępnych elementów:

 [![](images/image3.png "Na przykład tego zrzuty ekranu przedstawiają kilka dostępnych elementów")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>Więcej na sekcje i RootElements

Teraz omówimy RootElements i sekcji bardziej szczegółowo.

### <a name="rootelements"></a>RootElements

Co najmniej jeden elementów RootElement jest wymagany do uruchamiania procesu MonoTouch.Dialog.

Jeśli elementów RootElement zostanie zainicjowany z wartością/sekcja ta wartość jest używana do lokalizowania element podrzędny elementu, które będzie dostarczać Podsumowanie konfiguracji, który jest renderowany po prawej stronie ekranu. Na przykład poniższy zrzut ekranu przedstawia tabeli po lewej stronie z komórki zawierającej tytuł ekranu szczegółów po prawej stronie "Deserowe", wraz z wartością pustynia wybrane.

 [![](images/image4.png "Ten zrzut ekranu przedstawia tabeli po lewej stronie z komórki zawierającej tytuł ekranu szczegółów po prawej stronie, deserowe, wraz z wartością wybranego pustynia") ](images/image4.png#lightbox) [ ![ ] (images/image5.png "to Poniższy zrzut ekranu przedstawia tabeli po lewej stronie z komórki zawierającej tytuł ekranu szczegółów po prawej stronie, deserowe, wraz z wartością wybranego pustynia")](images/image5.png#lightbox)

Elementy główne można również wewnątrz sekcji do wyzwolenia ładowania nowej strony konfiguracji zagnieżdżonych, zgodnie z powyższym. Gdy jest używany w tym trybie podpis podany jest używany podczas renderowania wewnątrz sekcji i jest również używany jako tytuł dla podstrony. Na przykład:

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

W powyższym przykładzie po naciśnięciu na "Deserowe", MonoTouch.Dialog zostanie utworzenie nowej strony i przejdź do go z katalogu głównego jest "Deserowe" oraz o grupie opcji z trzech wartości.

W tym przykładzie określonej grupy opcji wybierze "Ciasto czekoladowe" w sekcji "Deserowe", ponieważ firma Microsoft przekazanego do RadioGroup wartość "2". Oznacza to, wybierz element 3 na liście (zero indeks).

Podczas wywoływania metody Add lub przy użyciu składni inicjatora 4 C# dodaje sekcje.
Metody Insert są dostarczane do wstawienia sekcji z animacji.

Jeśli utworzysz elementów RootElement z wystąpieniem grupy (zamiast RadioGroup) wartość podsumowania elementów RootElement pojawi się w sekcji będzie liczba skumulowana BooleanElements i CheckboxElements, który ma tego samego klucza, jako wartość Group.Key.

### <a name="sections"></a>Sekcje

Sekcje są używane do grupy elementów na ekranie i są one tylko prawidłowymi bezpośrednimi elementami podrzędnymi elementów RootElement. Sekcje mogą zawierać żadnego z elementów standardowe, w tym nowe RootElements.

RootElements osadzony w sekcji są używane do przechodzenia do nowego głębszą.

Sekcje może mieć nagłówków i stopek jako ciągi, albo UIViews.
Zwykle tylko użyje ciągi, ale można utworzyć niestandardowego UI może użyć dowolnego UIView jako nagłówka lub stopki. Możesz użyć ciągu je tworzyć następująco:

```csharp
var section = new Section ("Header", "Footer")
```

Aby móc używać widoków, po prostu Przekaż widoków do konstruktora:

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>Uzyskiwanie powiadomień

#### <a name="handling-nsaction"></a>Obsługa NSAction

PATRZ HASŁO MT. Powierzchnie D `NSAction` jako pełnomocnik obsługi wywołań zwrotnych.
Załóżmy na przykład, że chcesz obsługi zdarzenia touch dla komórki tabeli utworzone przez patrz hasło MT. D. Podczas tworzenia elementu przy użyciu patrz hasło MT. D, po prostu dostawy funkcji wywołania zwrotnego, jak pokazano poniżej:

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>Podczas pobierania wartości elementu

Połączone z `Element.Value` właściwości, wywołania zwrotnego można pobrać wartość ustawiona w innych elementów. Rozważmy na przykład następujący kod:

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

Ten kod tworzy interfejsu użytkownika, jak pokazano poniżej. Aby uzyskać pełny przewodnik, w tym przykładzie, zobacz [elementów interfejsu API wskazówki](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) samouczka.

 [![](images/image6.png "W połączeniu z właściwością Element.Value, wywołania zwrotnego można pobrać wartość ustawiona w innych elementów")](images/image6.png#lightbox)

Gdy użytkownik naciśnie dolnej komórki tabeli, kod w funkcji anonimowej wykonuje, zapisu wartości z `element` wystąpienie do **danych wyjściowych aplikacji** konsoli w programie Visual Studio dla komputerów Mac.

## <a name="built-in-elements"></a>Wbudowanych elementów

PATRZ HASŁO MT. D zawiera szereg wbudowanych elementów komórki znane jako elementy.
Te elementy są używane do wyświetlania szereg różnych typów w komórkach tabeli, takich jak ciągów, elementów przestawnych, dat i nawet obrazów, nazwę kilku. Każdy element odpowiada on za wyświetlanie odpowiednio typu danych. Na przykład boolean elementu wyświetli przełącznik, aby przełączyć jego wartość. Podobnie float element wyświetli suwaka można zmienić wartości typu float.

Istnieją bardziej złożone elementy do obsługi bardziej rozbudowane typy danych, takich jak obrazy i html. Na przykład element html, który zostanie otwarty UIWebView do załadowania strony sieci web, w przypadku wybrania, wyświetla podpisu w komórce tabeli.

### <a name="working-with-element-values"></a>Praca z wartości elementów

Elementy, które są używane do przechwytywania danych wejściowych użytkownika ujawniać publicznego `Value` właściwość, która zawiera bieżącą wartość elementu w dowolnym momencie. Jest automatycznie aktualizowany, ponieważ użytkownik korzysta z aplikacji.

Ta wartość jest stosowana dla wszystkich elementów, które są częścią MonoTouch.Dialog, ale nie jest wymagana dla elementów utworzonych przez użytkownika.

### <a name="string-element"></a>String Element

A `StringElement` zawiera podpis po lewej stronie komórki tabeli i wartości ciągu, po prawej stronie komórki.

 [![](images/image7.png "StringElement zawiera podpis po lewej stronie komórki tabeli i wartości ciągu, po prawej stronie komórki")](images/image7.png#lightbox)

Aby użyć `StringElement` jako przycisk, podaj delegata.

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [![](images/image8.png "Aby użyć StringElement jako przycisk, podaj delegata")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>Element styl ciągu

A `StyledStringElement` umożliwia ciągi przedstawiane albo style komórki tabeli wbudowanych lub niestandardowych formatowania.

 [![](images/image9.png "StyledStringElement umożliwia ciągi przedstawiane albo style komórki tabeli wbudowanych lub niestandardowych formatowania")](images/image9.png#lightbox)

`StyledStringElement` Pochodną klasy `StringElement`, ale umożliwia deweloperom dostosowywanie niewielki podzbiór właściwości, takie jak czcionkę, kolor tekstu, kolor tła komórki, tryb podziału wiersza, liczba wierszy do wyświetlenia, i czy element akcesoriów powinien być wyświetlany.

### <a name="multiline-element"></a>Wielowierszowy Element

 [![](images/image10.png "Wielowierszowy Element")](images/image10.png#lightbox)

### <a name="entry-element"></a>Entry Element

`EntryElement`, Jak nazwa oznacza, jest używany do pobierania danych wejściowych użytkownika. Obsługuje ona regularne ciągów lub haseł, w którym znaki są ukryte.

 [![](images/image11.png "EntryElement jest używany do pobierania danych wejściowych użytkownika")](images/image11.png#lightbox)

Jest on inicjowany z trzech wartości:

-  Podpis dla wpisu, który ma być wyświetlany dla użytkownika.
-  Tekst zastępczy (jest to tekst wyszarzone w poziomie, który udostępnia wskazówkę dla użytkownika). 
-  Wartość tekstu.


Symbol zastępczy i wartość może mieć wartości null. Podpis jest jednak wymagana.

W dowolnym momencie uzyskiwania dostępu do właściwości jego wartość można pobrać wartość `EntryElement`.

Ponadto `KeyboardType` właściwość można ustawić w czasie tworzenia styl typu klawiatury żądany dla wprowadzania danych. To może służyć do konfigurowania klawiatury za pomocą wartości `UIKeyboardType` wymienione poniżej:

-  numeryczne
-  Telefon
-  adres URL
-  Adres e-mail


### <a name="boolean-element"></a>Boolean — Element

 [![](images/image12.png "Boolean — Element")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>Element pola wyboru

 [![](images/image13.png "Element pola wyboru")](images/image13.png#lightbox)

### <a name="radio-element"></a>Element opcji

A `RadioElement` wymaga `RadioGroup` określonych w `RootElement`.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [![](images/image14.png "RadioElement wymaga RadioGroup określonych w elementów RootElement")](images/image14.png#lightbox)

 `RootElements` są również używane do koordynowania radiowych elementów. `RadioElement` Członkowie mogą znajdować się na wiele sekcji (na przykład do zaimplementowania polecenie podobne do następującego selektora sygnału pierścień i oddzielne pierścień niestandardowych dźwięków z dzwonków systemu). Widok podsumowania wyświetli obecnie wybranego elementu opcji. Aby użyć tej funkcji, należy utworzyć `RootElement` z konstruktorem grupy, jak to:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

Nazwa grupy w `RadioGroup` służy do wyświetlenia, wybranej wartości na stronie zawierającej (jeśli istnieje) i wartość, która jest w tym przypadku zero, jest indeks pierwszego wybranego elementu.

### <a name="badge-element"></a>Element wskaźnika

 [![](images/image15.png "Element wskaźnika")](images/image15.png#lightbox)

### <a name="float-element"></a>Float — Element

 [![](images/image16.png "Float — Element")](images/image16.png#lightbox)

### <a name="activity-element"></a>Element działania

 [![](images/image17.png "Element działania")](images/image17.png#lightbox)

### <a name="date-element"></a>Data — Element

 ![](images/image18.png "Data — Element")

Po wybraniu komórki odpowiadające DateElement wyboru daty są prezentowane w sposób przedstawiony poniżej:

 [![](images/image19.png "Po wybraniu komórki odpowiadające DateElement wyboru daty są prezentowane, jak pokazano")](images/image19.png#lightbox)

### <a name="time-element"></a>Time Element

 [![](images/image20.png "Time Element")](images/image20.png#lightbox)

Po wybraniu komórki odpowiadające TimeElement selektora czasu są prezentowane w sposób przedstawiony poniżej:

 [![](images/image21.png "Zaznaczenie komórki odpowiadające TimeElement selektora czasu są prezentowane, jak pokazano")](images/image21.png#lightbox)

### <a name="datetime-element"></a>DateTime Element

 [![](images/image22.png "DateTime Element")](images/image22.png#lightbox)

Po wybraniu komórki odpowiadające DateTimeElement selektora daty i godziny są prezentowane w sposób przedstawiony poniżej:

 [![](images/image23.png "Po wybraniu komórki odpowiadające DateTimeElement selektora daty i godziny są prezentowane, jak pokazano")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML Element

 [![](images/image24.png "HTML Element")](images/image24.png#lightbox)

`HTMLElement` Wyświetla wartość jego `Caption` właściwość w komórce tabeli. Wybrana, gdzie `Url` przypisana do elementu jest ładowany w `UIWebView` kontroli, jak pokazano poniżej:

 [![](images/image25.png "Gdzie zaznaczone, adres Url przypisane do elementu jest ładowany w formancie UIWebView, jak pokazano poniżej")](images/image25.png#lightbox)

### <a name="message-element"></a>Element wiadomości

 [![](images/image26.png "Element wiadomości")](images/image26.png#lightbox)

### <a name="load-more-element"></a>Załaduj Element więcej

Użyj tego elementu, aby umożliwić użytkownikom załadować więcej elementów na liście. Można dostosować normalnej i podpisy ładowania, a także kolor czcionki i tekst.
`UIActivity` Wskaźnik uruchamia animacji i podpis ładowania jest wyświetlane, gdy użytkownik naciska komórki, a następnie `NSAction` przekazany do konstruktora jest wykonywana. Po kodzie w `NSAction` zostało zakończone, `UIActivity` wskaźnik zatrzymuje animacji i normalnym podpis zostanie wyświetlony ponownie.

### <a name="uiview-element"></a>UIView Element

Ponadto niestandardowe `UIView` można wyświetlić przy użyciu `UIViewElement`.

### <a name="owner-drawn-element"></a>Element rysowanych przez właściciela

Ten element musi podklasą klasy, ponieważ jest klasą abstrakcyjną. Należy zastąpić `Height(RectangleF bounds)` metody, w którym powinien zwrócić wysokość elementu, a także `Draw(RectangleF bounds, CGContext context, UIView view)` , w którym należy wykonać na rysunku dostosowanego w danym zakresie przy użyciu parametrów kontekstu i widoku. Ten element jest lifting ciężki z podklasy `UIView`i umieszczenie w komórce ma zostać zwrócona, pozostawiając tylko konieczności wykonania dwóch zastąpień proste. Zostanie wyświetlony lepsze przykładowe zastosowanie w przykładowej aplikacji w `DemoOwnerDrawnElement.cs` pliku.

W tym miejscu jest bardzo prosty przykład Implementacja klasy:

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

`JsonElement` Jest podklasą `RootElement` który rozszerza `RootElement` aby można było załadować zawartość zagnieżdżonych elementów podrzędnych z lokalnego lub zdalnego adresu url.

`JsonElement` Jest `RootElement` który można wdrożyć w dwóch formach. Tworzy jedną wersję `RootElement` który załaduje zawartości na żądanie. Są one tworzone przy użyciu `JsonElement` konstruktorów przyjmujących dodatkowy argument na końcu adresu url, aby załadować zawartość z:

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

Inne formy tworzy dane z pliku lokalnego lub istniejącej `System.Json.JsonObject` , który ma już przeanalizować:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

Aby uzyskać więcej informacji o używaniu JSON z patrz hasło MT. D, zobacz [wskazówki elementu JSON](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) samouczka.

## <a name="other-features"></a>Inne funkcje

### <a name="pull-to-refresh-support"></a>Obsługa pociągnij, aby odświeżyć

 *Ściąganie-do-* *Odśwież* efektem pierwotnie znajduje się w *Tweetie2* aplikacji, które stały się efekt popularnych wielu aplikacjom.

Aby dodać automatyczna obsługa pociągnij, aby odświeżyć z okien dialogowych, wystarczy wykonać dwie czynności: Połącz program obsługi zdarzeń, aby otrzymać powiadomienie, gdy użytkownik tworzy dane i powiadom `DialogViewController` po załadowaniu danych Aby powrócić do stanu domyślnego.

Podłączanie powiadomienie jest proste; Wystarczy podłączyć do `RefreshRequested` zdarzenia w `DialogViewController`, podobnie do następującej:

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

Następnie na metodę `OnUserRequestedRefresh`, czy kolejka ładowania niektórych danych, niektóre dane żądania z sieci lub pokrętła wątku do obliczenia danych. Po załadowaniu danych należy powiadomić `DialogViewController` nowe dane są w, którą można przywrócić widok do stanu domyślnego, możesz to zrobić przez wywołanie `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>Obsługa wyszukiwania

Aby umożliwić wyszukiwanie, należy ustawić `EnableSearch` właściwości na Twojej `DialogViewController`. Można również ustawić `SearchPlaceholder` właściwości do użycia jako tekstu znaku wodnego na pasku wyszukiwania.

Wyszukiwanie zmieni zawartość widoku jako typy użytkownika. Wyszukuje widocznych pól i pokazuje te dla użytkownika. `DialogViewController` Udostępnia trzy metody programowo zainicjować, przerwanie wyzwalacza lub nowej operacji filtru na wyniki. Te metody są wymienione poniżej:

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


System jest rozszerzalny, więc jeśli chcesz zmienić to zachowanie.

### <a name="background-image-loading"></a>Ładowanie obrazu tła

Zawiera MonoTouch.Dialog [TweetStation](https://github.com/migueldeicaza/TweetStation) moduł ładujący obrazy aplikacji. Ten moduł ładujący obrazy mogą służyć do ładowania obrazów w tle obsługuje buforowanie i może powiadomić kodu, gdy obraz został załadowany.

Zostanie również ogranicza liczbę wychodzących połączeń sieciowych.

Moduł ładujący obrazy jest zaimplementowana w `ImageLoader` klasy, wszystko co należy zrobić to wywołanie `DefaultRequestImage` metody, konieczne będzie podanie identyfikatora Uri obrazu, aby załadować, a także wystąpienia `IImageUpdated` interfejs, który będzie wywoływane, gdy obraz ha s został załadowany.

Na przykład poniższy kod ładuje obraz z adresu Url do `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

Klasa ImageLoader udostępnia metody Purge, którą można wywołać, jeśli chcesz zwolnić wszystkie obrazy, które są obecnie w pamięci podręcznej w pamięci. Bieżący kod używa pamięci podręcznej obrazów 50. Jeśli chcesz użyć rozmiaru pamięci podręcznej inny (na przykład, jeśli są oczekiwane obrazy byłby za duży, że obrazy 50 byłby zbyt dużo), można po prostu utworzyć wystąpień ImageLoader i przekazać liczbę obrazów, które mają być zachowane w pamięci podręcznej.

## <a name="using-linq-to-create-element-hierarchy"></a>Tworzenie elementu hierarchii za pomocą LINQ

Za pomocą inteligentne użycie LINQ i C# w Inicjalizacja składni LINQ może służyć do tworzenia hierarchia elementów. Na przykład poniższy kod tworzy ekranu z niektórych tablic ciągów i wyboru za pomocą funkcji anonimowej, która została przekazana do każdej komórki uchwytów `StringElement`:

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

To prosty sposób można łączyć z magazynu danych XML lub dane z bazy danych do utworzenia złożonych aplikacji niemal całkowicie z danych.

## <a name="extending-mtd"></a>Rozszerzanie patrz hasło MT. D

### <a name="creating-custom-elements"></a>Tworzenie niestandardowych elementów

Można utworzyć własny element przez dziedziczenie z albo istniejącego elementu lub tworzenia klasy pochodnej z klasy głównym elementu.

Można utworzyć własny elementu, można zastąpić następujące metody:

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

Jeśli Twoje element może mieć rozmiar zmiennej, musisz wdrożyć `IElementSizing` interfejsu, który zawiera jedną metodę:

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

Jeśli planujesz wdrożenie programu `GetCell` metody przez wywołanie metody `base.GetCell(tv)` i dostosowywanie zwrócony komórki, musisz również zastąpić `CellKey` właściwości, aby przywrócić klucz, który jest unikatowa dla danego elementu, takich jak to:

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

Działa to dla większości elementów, ale nie dla `StringElement` i `StyledStringElement` jako te, użyj własny zestaw kluczy dla różnych scenariuszy renderowania. Trzeba replikowanie kod w tych klas.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

Zarówno odbicie, jak i interfejs API elementy używać tego samego `DialogViewController`. Czasami można dostosować wygląd widoku lub warto korzystać z niektórych funkcji z `UITableViewController` która wykracza poza podstawowe tworzenie UI.

`DialogViewController` Jedynie podklasą klasy `UITableViewController` i można go dostosować w taki sam sposób, który można dostosować, czy `UITableViewController`.

Na przykład, jeśli chcesz zmienić styl listy musi mieć przypisany `Grouped` lub `Plain`, można ustawić tej wartości, zmieniając właściwość podczas tworzenia kontrolera, takie jak to:

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

Aby uzyskać więcej zaawansowane dostosowania `DialogViewController`, takie jak ustawienie tła, jak podklasy go i zastąpienie właściwego metod, jak pokazano w poniższym przykładzie:

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

Następujące metody wirtualne w jest inny punkt dostosowywania `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

Ta metoda powinna zwrócić podklasą `DialogViewController.Source` przypadków, gdy Twoje komórki są równomiernie o rozmiarze lub podklasa klasy `DialogViewController.SizingSource` przypadku nierówna z komórek.

To zastąpienie umożliwia przechwytywanie dowolnego z `UITableViewSource` metody. Na przykład [TweetStation](https://github.com/migueldeicaza/TweetStation) używa go do śledzenia, gdy użytkownik ma przewijane w górnej i odpowiednio zaktualizować liczby nieprzeczytana tweetów.

## <a name="validation"></a>Walidacja

Elementy nie zapewniają weryfikacji się jako modeli, które dobrze nadają się do strony sieci web i aplikacje komputerowe nie mapują bezpośrednio do modelu interakcji iPhone.

Jeśli chcesz przeprowadzić sprawdzanie poprawności danych, należy to zrobić, gdy użytkownik wyzwala akcję z wprowadzone dane. Na przykład <span class="ui">gotowe</span> lub <span class="ui">dalej</span> przycisk na górnym pasku narzędzi lub niektóre `StringElement` używany jako przycisk, aby przejść do kolejnego etapu.

Jest to gdzie może wykonywać podstawowe sprawdzania poprawności danych wejściowych i prawdopodobnie więcej skomplikowane sprawdzania poprawności, takich jak sprawdzanie ważności kombinacja użytkownika i hasła, z serwerem.

Jak powiadomić użytkowników błędu jest określone dla aplikacji. Można wyskakujące `UIAlertView` lub Pokaż wskazówkę.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano wiele informacji o MonoTouch.Dialog. Go omówiono podstawowe informacje na temat sposobu, w jaki patrz hasło MT. D działa i jest objęty różne składniki wchodzące w skład patrz hasło MT. D. Wykazało również szerokiej gamy elementów i dostosowania tabeli obsługiwane przez patrz hasło MT. D i opisano sposób patrz hasło MT. D można rozszerzyć z niestandardowych elementów. Ponadto wyjaśniono jej obsługi JSON w patrz hasło MT. D, który umożliwia dynamiczne tworzenie elementów z formatu JSON.


## <a name="related-links"></a>Linki pokrewne

- [Tworzy Miguel de Icaza Screencast - ekran logowania dla systemu iOS z MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Screencast — łatwe tworzenie iOS interfejsy użytkownika z MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Przewodnik: tworzenie aplikacji przy użyciu interfejsu API elementów](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Przewodnik: tworzenie aplikacji przy użyciu interfejsu API odbicia](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Przewodnik: tworzenie interfejsu użytkownika przy użyciu elementu JSON](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch.Dialog JSON Markup](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Okno dialogowe MonoTouch w witrynie Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Odwołania do klasy UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Odwołania do klasy UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
