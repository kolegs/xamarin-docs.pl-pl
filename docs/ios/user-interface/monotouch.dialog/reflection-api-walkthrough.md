---
title: Tworzenie aplikacji platformy Xamarin.iOS przy użyciu interfejsu API odbicia
description: W tym dokumencie opisano elementu MonoTouch.Dialog opartych na atrybutach interfejsu API odbicia, co powoduje utworzenie interfejsu użytkownika w oparciu o atrybuty klasy.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: a1b77f46410ef20892485a866221bab2c872e54c
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038475"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>Tworzenie aplikacji platformy Xamarin.iOS przy użyciu interfejsu API odbicia

Patrz hasło MT. Interfejsu API odbicia D umożliwia klasy powinny być dekorowane za pomocą atrybutów tego patrz hasło MT. D używa do tworzenia ekranów automatycznie. Interfejs API odbicia zapewnia powiązanie między tymi klasami i wyświetlanych na ekranie. Mimo że ten interfejs API nie zapewnia precyzyjną kontrolę, który wykonuje elementów interfejsu API, zmniejsza złożoność, automatycznie tworząc poziomie hierarchii elementu odpowiednio do dekoracji klasy.

## <a name="setting-up-mtd"></a>Konfigurowanie patrz hasło MT. D

PATRZ HASŁO MT. D jest rozpowszechniana z platformy Xamarin.iOS. Aby go użyć, kliknij prawym przyciskiem myszy **odwołania** węzła Xamarin.iOS projektu w programie Visual Studio 2017 lub Visual Studio dla komputerów Mac i Dodaj odwołanie do **1 elementu MonoTouch.Dialog** zestawu. Następnie należy dodać `using MonoTouch.Dialog` instrukcji w źródle kodu zgodnie z potrzebami.

## <a name="getting-started-with-the-reflection-api"></a>Wprowadzenie do interfejsu API odbicia

Korzystanie z interfejsu API odbicia jest tak proste, jak:

1.  Tworzenie klasy ozdobione patrz hasło MT. Atrybuty D.
1.  Tworzenie `BindingContext` wystąpienia, przekazywanie jej przez wystąpienie klasy powyżej. 
1.  Tworzenie `DialogViewController` , podając mu `BindingContext’s` `RootElement` . 


Spójrzmy na przykład ilustruje sposób korzystania z interfejsu API odbicia. W tym przykładzie utworzymy ekranu wprowadzania danych proste jak pokazano poniżej:

 [![](reflection-api-walkthrough-images/01-expense-entry.png "W tym przykładzie utworzymy ekranu wprowadzania danych proste jak pokazano poniżej")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

## <a name="creating-a-class-with-mtd-attributes"></a>Tworzenie klasy za pomocą patrz hasło MT. Atrybuty D

Pierwszą rzeczą, należy użyć interfejsu API odbicia jest klasą atrybuty. Te atrybuty, które będą używane przez patrz hasło MT. D wewnętrznie, aby utworzyć obiekty z interfejsu API elementów. Na przykład należy wziąć pod uwagę poniższą definicję klasy:

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

`SectionAttribute` Spowoduje sekcje `UITableView` tworzona z argumentem ciągu używanych do wypełniania nagłówek sekcji. Po zadeklarowaniu sekcji każde pole, który po nim następuje będą uwzględniane w tej sekcji, dopóki nie jest zadeklarowany w innej sekcji.
Typ elementu interfejsu użytkownika tworzone dla pola zależy od typu pola i patrz hasło MT. Atrybut D urządzanie go.

Na przykład `Name` pole jest `string` i zostanie nadany `EntryAttribute`. Skutkuje to dodawane do tabeli za pomocą pole wprowadzania tekstu i podpis określony wiersz. Podobnie `IsApproved` pole jest `bool` z `CheckboxAttribute`, dając w wierszu tabeli za pomocą pola wyboru w prawym rogu komórki tabeli. PATRZ HASŁO MT. D używa nazwy pola Automatyczne dodawanie spację, w tym przypadku utworzyć podpisu, ponieważ nie jest określony w atrybucie.

## <a name="adding-the-bindingcontext"></a>Dodawanie BindingContext.

Aby użyć `Expense` klasy, należy utworzyć `BindingContext`. Element `BindingContext` jest klasą, który powiąże dane z atrybutami klasy, aby utworzyć hierarchię elementów. Aby go utworzyć, możemy po prostu tworzenia jego instancji i przekazać do konstruktora wystąpienia klasy opartego na atrybutach.

Na przykład, aby dodać interfejs użytkownika, który możemy zadeklarowane za pomocą atrybutu w `Expense` klasy, Dołącz następujący kod w `FinishedLaunching` metody `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Następnie musimy to zrobić, aby utworzyć interfejs użytkownika wystarczy dodać `BindingContext` do `DialogViewController` i ustaw go jako `RootViewController` okna, jak pokazano poniżej:

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

Uruchomiona jest aplikacja teraz wyników na ekranie powyżej są wyświetlane.

### <a name="adding-a-uinavigationcontroller"></a>Dodawanie UINavigationController

Należy jednak zauważyć, że tytuł "Utwórz zadanie" który możemy przekazany do `BindingContext` nie jest wyświetlana. Jest to spowodowane `DialogViewController` jest nie jest częścią `UINavigatonController`. Zmieńmy kod, aby dodać `UINavigationController` jako okno `RootViewController,` i Dodaj `DialogViewController` jako katalog główny `UINavigationController` jak pokazano poniżej:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Teraz gdy możemy uruchomić aplikację, tytuł jest wyświetlany w `UINavigationController’s` paska nawigacji jako zrzucie ekranu poniżej przedstawia:

 [![](reflection-api-walkthrough-images/02-create-task.png "Uruchomienie aplikacji, tytuł pojawi się na pasku nawigacyjnym UINavigationControllers")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

Jeśli dołączysz `UINavigationController`, firma Microsoft może teraz korzystać z innych funkcji MT. D, dla którego Nawigacja jest to konieczne. Na przykład można dodać do wyliczenia `Expense` klasy, aby określić przynależność do kategorii wydatków i patrz hasło MT. D automatycznie utworzy ekranu wyboru. Aby zademonstrować, zmodyfikuj `Expense` klasy, aby uwzględnić `ExpenseCategory` pola w następujący sposób:

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

Uruchomiona jest aplikacja teraz powoduje zwrócenie nowego wiersza w tabeli dla kategorii, jak pokazano:

 [![](reflection-api-walkthrough-images/03-set-details.png "Uruchomiona jest aplikacja teraz skutkuje nowy wiersz w tabeli dla kategorii, jak pokazano")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

Zaznaczając wiersz wyników w aplikacji przejdź do nowego ekranu z wierszami odpowiadający wyliczenia, jak pokazano poniżej:

 [![](reflection-api-walkthrough-images/04-set-category.png "Zaznaczając wiersz wyników w aplikacji przejdź do nowego ekranu z wierszami odpowiadający wyliczenia")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Podsumowanie

W tym artykule przedstawione wskazówki dotyczące interfejsu API odbicia. Pokazaliśmy, jak dodać atrybuty do klasy, aby kontrolować, co jest wyświetlane. Omówiono również sposób użycia `BindingContext` Aby powiązać dane z klasy do hierarchii element, który zostanie utworzony, a także sposób wykorzystania patrz hasło MT. D z `UINavigationController`.


## <a name="related-links"></a>Linki pokrewne

- [MTDReflectionWalkthrough (przykład)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [Zrzut ekranu — Miguel de Icaza tworzy ekran logowania dla systemu iOS przy użyciu elementu MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Zrzut ekranu — łatwe tworzenie dla systemu iOS interfejsy użytkownika przy użyciu elementu MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Wprowadzenie do okna dialogowego MonoTouch](~/ios/user-interface/monotouch.dialog/index.md)
- [Elementy interfejsu API wskazówki](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Przewodnik elementu JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Okno dialogowe MonoTouch w witrynie Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation aplikacji](https://github.com/migueldeicaza/TweetStation)
- [Informacje o klasach UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Informacje o klasach UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
