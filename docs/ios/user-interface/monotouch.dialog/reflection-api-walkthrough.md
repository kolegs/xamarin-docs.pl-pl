---
title: 'Wskazówki: Tworzenie aplikacji przy użyciu interfejsu API odbicia'
description: Oprócz elementów interfejsu API, MonoTouch.Dialog (patrz hasło MT. D) także interfejsu API odbicia atrybutu. Tworzenie ekranów z patrz hasło MT. sprawia, że interfejs API odbicia D, wystarczy dekoracji klas atrybutów. Ten artykuł zawiera przechodzenia przez pokazujący sposób tworzenia aplikacji przy użyciu interfejsu API odbicia.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e56eaeccb2e09d9f1ad84245bf41e2a4bf1b56f1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough-creating-an-application-using-the-reflection-api"></a>Wskazówki: Tworzenie aplikacji przy użyciu interfejsu API odbicia

_Oprócz elementów interfejsu API, MonoTouch.Dialog (patrz hasło MT. D) także interfejsu API odbicia atrybutu. Tworzenie ekranów z patrz hasło MT. sprawia, że interfejs API odbicia D, wystarczy dekoracji klas atrybutów. Ten artykuł zawiera przechodzenia przez pokazujący sposób tworzenia aplikacji przy użyciu interfejsu API odbicia._


Patrz hasło MT. Interfejs API odbicia D umożliwia jako ozdobione atrybutów tego patrz hasło MT. D używa automatyczne tworzenie ekranów. Odbicie interfejs API udostępnia powiązania między tymi klasami i wyświetlanych na ekranie. Mimo że ten interfejs API nie zapewnia precyzyjną kontrolę, który wykonuje elementów interfejsu API, zmniejsza złożoność automatyczne tworzenie limit hierarchia elementów oparte na decoration klasy.

 <a name="Getting_Started_with_the_Reflection_API" />


## <a name="getting-started-with-the-reflection-api"></a>Wprowadzenie do korzystania z interfejsu API odbicia

Przy użyciu interfejsu API odbicia jest niezwykle prosta:

1.  Tworzenie klasy ozdobione patrz hasło MT. Atrybuty D.
1.  Tworzenie `BindingContext` wystąpienia, przekazanie jej przez wystąpienie klasy powyżej. 
1.  Tworzenie `DialogViewController` , przekazanie jej `BindingContext’s` `RootElement` . 


Oto przykład ilustrujący sposób użycia interfejsu API odbicia. W tym przykładzie oprzemy będzie ekranu wprowadzania danych proste jak pokazano poniżej:

 [![](reflection-api-walkthrough-images/01-expense-entry.png "W tym przykładzie będzie budujemy ekranu wprowadzania danych proste w sposób pokazany poniżej")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

 <a name="Creating_a_Class_with_MT.D_Attributes" />


## <a name="creating-a-class-with-mtd-attributes"></a>Tworzenie klasy za pomocą patrz hasło MT. Atrybuty D

Najpierw musimy za pomocą interfejsu API odbicia jest klasą atrybuty. Te atrybuty będą używane przez patrz hasło MT. D wewnętrznie w celu utworzenia obiektów z elementów interfejsu API. Rozważmy na przykład następującą definicję klasy:

```csharp
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
}
```

`SectionAttribute` Spowoduje sekcje `UITableView` tworzona z argumentem ciągu służące do wypełniania nagłówek sekcji. Po zadeklarowaniu sekcji każde pole, które następuje będą uwzględniane w tej sekcji, dopóki zadeklarowano innej sekcji.
Typ elementu interfejsu użytkownika utworzone dla pola zależy od typu pola i patrz hasło MT. Atrybut D dekoracji go.

Na przykład `Name` pole jest `string` i ma ona `EntryAttribute`. Powoduje to dodawane do tabeli z pole wprowadzania tekstu i podpis określonego wiersza. Podobnie `IsApproved` pole jest `bool` z `CheckboxAttribute`, co z wyboru po prawej stronie komórki tabeli wiersza tabeli. PATRZ HASŁO MT. D używa nazwy pola, automatyczne dodawanie z parzystością, można utworzyć podpisu w tym przypadku, ponieważ nie jest określona w atrybucie.

 <a name="Adding_the_BindingContext" />


## <a name="adding-the-bindingcontext"></a>Dodawanie BindingContext.

Aby użyć `Expense` klasy, należy utworzyć `BindingContext`. A `BindingContext` jest klasa, która będzie powiązać dane z klasy oparte na atrybutach można utworzyć hierarchii elementów. Aby go utworzyć, możemy po prostu utworzyć i Przekaż do konstruktora wystąpienia klasy oparte na atrybutach.

Na przykład, aby dodać interfejs użytkownika, który został zadeklarowany za pomocą atrybutu w `Expense` klasy, Dołącz następujący kod w `FinishedLaunching` metody `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Wszystkie musimy wykonać, aby utworzyć interfejsu użytkownika jest dodać `BindingContext` do `DialogViewController` i ustawić go jako `RootViewController` okna, jak pokazano poniżej:

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
   
        window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        var expense = new Expense ();
        var bctx = new BindingContext (null, expense, "Create a task");
        var dvc = new DialogViewController (bctx.Root);
            
        window.RootViewController = dvc;
        window.MakeKeyAndVisible ();
            
        return true;
}
```

Uruchamianie aplikacji teraz powoduje ekranu pokazano powyżej będzie wyświetlany.

 <a name="Adding_a_UINavigationController" />


### <a name="adding-a-uinavigationcontroller"></a>Dodawanie UINavigationController

Zwróć jednak uwagę, że tytuł "Utwórz zadanie" który mamy przekazany do `BindingContext` nie jest wyświetlany. Jest to spowodowane `DialogViewController` jest nie jest częścią `UINavigatonController`. Zmieńmy kod, aby dodać `UINavigationController` jako okna `RootViewController,` i Dodaj `DialogViewController` jako katalog główny `UINavigationController` w sposób przedstawiony poniżej:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Teraz uruchamianych aplikacji, tytuł jest wyświetlany w `UINavigationController’s` pasek nawigacyjny jako poniżej przedstawiono zrzut ekranu:

 [![](reflection-api-walkthrough-images/02-create-task.png "Uruchomienie aplikacji, tytuł pojawi się teraz na pasku nawigacyjnym UINavigationControllers")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

W tym `UINavigationController`, firma Microsoft można teraz korzystać z innych funkcji patrz hasło MT. D, dla których konieczne jest nawigacji. Na przykład można dodać wyliczenia do `Expense` klasy do definiowania kategorii wydatków i patrz hasło MT. D automatycznie utworzy ekranu wyboru. Aby zademonstrować, zmodyfikuj `Expense` klasy, aby uwzględnić `ExpenseCategory` pola w następujący sposób:

```csharp
public enum Category
{
        Travel,
        Lodging,
        Books
}
        
public class Expense
{
        …

        [Caption("Category")]
        public Category ExpenseCategory;
}
```

Uruchamianie aplikacji teraz wyniki nowy wiersz w tabeli dla kategorii, jak pokazano:

 [![](reflection-api-walkthrough-images/03-set-details.png "Uruchamianie aplikacji teraz powoduje nowy wiersz w tabeli dla kategorii pokazany")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

Zaznaczenie wiersza powoduje aplikacji przechodzenia do nowego ekranu z wierszy odpowiadających wyliczenia, jak pokazano poniżej:

 [![](reflection-api-walkthrough-images/04-set-category.png "Zaznaczenie wiersza powoduje aplikacji przejście do nowego ekranu z wierszy odpowiadających wyliczenia")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Podsumowanie

W tym artykule przedstawione wskazówki interfejsu API odbicia. Firma Microsoft pokazano, jak dodać atrybuty do klasy, aby kontrolować, co jest wyświetlane. Omówiono sposób użycia `BindingContext` Aby powiązać dane z klasy hierarchii element, który został utworzony, a także sposobu użycia patrz hasło MT. D z `UINavigationController`.


## <a name="related-links"></a>Linki pokrewne

- [MTDReflectionWalkthrough (przykład)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [Tworzy Miguel de Icaza Screencast - ekran logowania dla systemu iOS z MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Screencast — łatwe tworzenie iOS interfejsy użytkownika z MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Wprowadzenie do okna dialogowego MonoTouch](~/ios/user-interface/monotouch.dialog/index.md)
- [Elementy interfejsu API wskazówki](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Wskazówki elementu JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Okno dialogowe MonoTouch w witrynie Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation aplikacji](https://github.com/migueldeicaza/TweetStation)
- [Odwołania do klasy UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Odwołania do klasy UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
