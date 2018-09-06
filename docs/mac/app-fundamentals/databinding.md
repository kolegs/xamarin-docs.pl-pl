---
title: Powiązanie danych i kodowanie klucz wartość platformie Xamarin.Mac
description: W tym artykule opisano, przy użyciu kodowania i pary klucz wartość, monitorowanie, aby umożliwić wiązanie danych do elementów interfejsu użytkownika w program Xcode Interface Builder pary klucz wartość.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 0adb8cda71ca8803c535679da2aecf00f3fa46a5
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780630"
---
# <a name="data-binding-and-key-value-coding-in-xamarinmac"></a>Powiązanie danych i kodowanie klucz wartość platformie Xamarin.Mac

_W tym artykule opisano, przy użyciu kodowania i pary klucz wartość, monitorowanie, aby umożliwić wiązanie danych do elementów interfejsu użytkownika w program Xcode Interface Builder pary klucz wartość._

## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji platformy Xamarin.Mac, masz dostęp do tego samego kodowanie klucz wartość i technik wiązania danych, deweloper pracujący w *języka Objective-C* i *Xcode* jest. Ponieważ rozszerzenia Xamarin.Mac integruje się bezpośrednio za pomocą edytora Xcode, można użyć program Xcode _programu Interface Builder_ powiązać dane elementów interfejsu użytkownika zamiast pisania kodu.

Przy użyciu kodowania i powiązania danych technik w aplikacji platformy Xamarin.Mac pary klucz wartość, może znacznie skrócić ilość kodu, który trzeba napisać i obsługiwać do wypełniania i pracować z elementami interfejsu użytkownika. Masz także zaletą dalsze oddzielenie danych zapasowy (_modelu danych_) z usługi frontonu zakończenia interfejsu użytkownika (_Model-View-Controller_), prowadzącego do łatwiejsza Obsługa bardziej elastycznych aplikacji Projekt.

[![Przykładem uruchomionej aplikacji](databinding-images/intro01.png "przykładem uruchomionej aplikacji")](databinding-images/intro01-large.png#lightbox)

W tym artykule omówiono podstawowe informacje dotyczące pracy z kodowanie klucz wartość i powiązanie danych w aplikacji platformy Xamarin.Mac. Zdecydowanie zalecane jest, pracy za pośrednictwem [Witaj, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programu Xcode i programu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) i [gniazd i akcje](~/mac/get-started/hello-mac.md#outlets-and-actions) sekcje w postaci, w jakiej omawia kluczowe założenia i technik, które będzie używany w tym artykule.

Możesz chcieć spojrzeć na [udostępnianie klasy języka C# / metod języka Objective-C](~/mac/internals/how-it-works.md) części [elementach wewnętrznych rozszerzenia Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentów, jak również wyjaśniono `Register` i `Export` atrybutów Umożliwia podłączanie klas języka C# do języka Objective-C obiektów i interfejsu użytkownika elementów.

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>Co to jest kodowanie klucz wartość

Kodowanie klucz wartość (KVC) to mechanizm służący do uzyskiwania dostępu do właściwości obiektu pośrednio, za pomocą klucze (specjalnie sformatowanego ciągi), aby zidentyfikować właściwości zamiast uzyskanie do nich dostępu za pośrednictwem zmienne wystąpienia lub metody dostępu (`get/set`). Wdrażając pary klucz wartość kodowania zgodne metod dostępu do aplikacji platformy Xamarin.Mac, możesz uzyskać dostęp do innych funkcji z systemem macOS (wcześniej znane jako OS X), takich jak obserwowania pary klucz wartość (KVO), wiązanie danych, danych podstawowych, cocoa dla powiązania i scriptability.

Przy użyciu kodowania i powiązania danych technik w aplikacji platformy Xamarin.Mac pary klucz wartość, może znacznie skrócić ilość kodu, który trzeba napisać i obsługiwać do wypełniania i pracować z elementami interfejsu użytkownika. Masz także zaletą dalsze oddzielenie danych zapasowy (_modelu danych_) z usługi frontonu zakończenia interfejsu użytkownika (_Model-View-Controller_), prowadzącego do łatwiejsza Obsługa bardziej elastycznych aplikacji Projekt. 

Na przykład Przyjrzyjmy się poniższą definicję klasy obiektu zgodne KVC:

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

Po pierwsze, `[Register("PersonModel")]` atrybut rejestruje klasę i udostępniła je systemowi Objective-C. Następnie trzeba dziedziczyć z klasy `NSObject` (lub podklasa klasy, która dziedziczy `NSObject`), spowoduje to dodanie kilku podstawowa metoda, dzięki czemu do klasy jako KVC zgodne. Następnie `[Export("Name")]` atrybutu ujawnia `Name` właściwości i wartości klucza, który później będzie służyć do dostępu do właściwości przy użyciu techniki KVC i KVO definiuje. 

Na koniec się być obserwowane pary klucz-wartość zmienia się na wartość właściwości metody dostępu należy opakować zmiany jego wartości w `WillChangeValue` i `DidChangeValue` wywołania metody (Określanie tego samego klucza `Export` atrybutu).  Na przykład:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

Ten krok jest _bardzo_ ważne dla powiązania danych w środowisku Xcode użytkownika programu Interface Builder (jak zobaczymy w dalszej części tego artykułu).

Aby uzyskać więcej informacji, zobacz firmy Apple [pary klucz-wartość kodowania Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html).

### <a name="keys-and-key-paths"></a>Klucze i ścieżki klucza

A _klucza_ jest ciągiem, który identyfikuje określone właściwości obiektu. Zazwyczaj klucz odpowiada nazwę metody dostępu w kodowania obiektów zgodnego z pary klucz wartość. Klucze muszą używać kodowanie ASCII, zwykle zaczynać się od małej litery i nie może zawierać białych znaków. Dlatego podana w powyższym przykładzie, `Name` będzie wartość klucza `Name` właściwość `PersonModel` klasy. Klucz i nazwę właściwości, które udostępniają one nie muszą być takie same, jednak w większości przypadków.

A _ścieżka klucza_ jest ciągiem z dot rozdzielonych klucze używane do określania hierarchię właściwości obiektu, aby przejść. Właściwość pierwszy klucz w sekwencji jest określana względem odbiorcy, a każdy klucz kolejnych jest oceniane względem wartości właściwości poprzedniego. Tak samo jak umożliwia kropkowego przechodzenie przez obiekt i jego właściwości w klasie języka C#.

Na przykład, jeśli zostanie rozwinięte `PersonModel` klasy i dodać `Child` właściwości:

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        private PersonModel _child = new PersonModel();
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        [Export("Child")]
        public PersonModel Child {
            get { return _child; }
            set {
                WillChangeValue ("Child");
                _child = value;
                DidChangeValue ("Child");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

Ścieżka klucza do nazwy elementu podrzędnego będzie `self.Child.Name` lub po prostu `Child.Name` (oparte na jak używana była wartość klucza).

### <a name="getting-values-using-key-value-coding"></a>Pobieranie wartości przy użyciu kodowanie klucz wartość

`ValueForKey` Metoda zwraca wartość dla określonego klucza (jako `NSString`), względem wystąpienia klasy KVC odbiera żądanie. Na przykład jeśli `Person` jest wystąpieniem `PersonModel` klasy zdefiniowane powyżej:

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

To zwróci wartość `Name` właściwości dla danego wystąpienia `PersonModel`. 

### <a name="setting-values-using-key-value-coding"></a>Ustawienie wartości za pomocą kodowanie klucz wartość

Podobnie `SetValueForKey` ustaw wartość dla określonego klucza (jako `NSString`), względem wystąpienia klasy KVC odbiera żądanie. Dlatego ponownie przy użyciu wystąpienia `PersonModel` klasy, jak pokazano poniżej:

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

Czy zmienić wartość `Name` właściwość `Jane Doe`.

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>Monitorowanie zmian wartości

Za pomocą pary klucz wartość obserwowania (KVO), można dołączyć obserwatora do określonego klucza klasy zgodne KVC i powiadomić każdym razem, gdy wartość tego klucza jest modyfikowany (przy użyciu technik KVC lub bezpośredni dostęp do danej właściwości w kodzie języka C#). Na przykład:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

Teraz w dowolnym momencie `Name` właściwość `Person` wystąpienie `PersonModel` klasy został zmodyfikowany, nowa wartość jest zapisywany do konsoli. 

Aby uzyskać więcej informacji, zobacz firmy Apple [wprowadzenie do pary klucz-wartość obserwowania Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i).

## <a name="data-binding"></a>Powiązanie danych

Poniższe sekcje pokaże, jak umożliwia kodowanie klucz wartość i pary klucz wartość obserwowania zgodne klasy wiązanie danych do elementów interfejsu użytkownika w program Xcode Interface Builder zamiast odczytywanie i zapisywanie wartości przy użyciu kodu C#. W ten sposób można oddzielić swoje _modelu danych_ z widoków, które są używane do ich wyświetlania, wprowadzania bardziej elastyczne i łatwiejsze w obsłudze aplikacji platformy Xamarin.Mac. Możesz również znacznie zmniejszyć ilość kodu, który ma zostać zapisany.

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>Zdefiniowanie modelu danych

Zanim będzie można powiązać danych elementu interfejsu użytkownika w programu Interface Builder, konieczne jest posiadanie KVC/KVO zgodne klasy zdefiniowanej w aplikacji Xamarin.Mac do działania jako _modelu danych_ dla wiązania. Model danych zawiera wszystkie dane, która będzie wyświetlana w interfejsie użytkownika i odbiera wszelkie modyfikacje danych, który użytkownik dokona w interfejsie użytkownika podczas uruchamiania aplikacji.

Na przykład podczas pisania aplikacji, która zarządzane grupy pracowników, można użyć następującej klasy do definiowania modelu danych:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

Większość funkcji ta klasa zostały pokryte w [co to jest kodowanie klucz wartość](#What_is_Key-Value_Coding) powyższej sekcji. Jednakże, Przyjrzyjmy się kilku określonych elementów i niektóre dodatki, które zostały wprowadzone, aby umożliwić tej klasy, aby pełnić rolę Model danych do **kontrolerów macierzy** i **kontrolerów drzewa** (który będzie używany później do danych Powiąż **widoków drzewa**, **widoki konspektu** i **widoki kolekcji**).

Po pierwsze, ponieważ pracownik może być menedżera, używaliśmy `NSArray` (w szczególności `NSMutableArray` , dzięki czemu można zmodyfikować wartości) sposób umożliwić pracownikom, zarządzanych do dołączenia do nich:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

Należy pamiętać, w tym miejscu dwie rzeczy:

1. Użyliśmy `NSMutableArray` zamiast standardowego języka C# tablicy lub kolekcji, ponieważ jest to wymagane, aby powiązać dane do kontrolek AppKit takich jak **widoki tabel**, **widoki konspektu** i **kolekcji** .
2. Firma Microsoft udostępnianych przez rzutowanie go do tablicy pracowników `NSArray` dla wiązaniu danych do celów i zmienić jej C# sformatowane nazwy `People`, do jednego, który oczekuje, że powiązanie danych, `personModelArray` w formularzu **{class_name} tablicy** (Uwaga czy pierwszym znakiem przeprowadzono małe litery).

Następnie należy dodać niektóre specjalnie nazwę metody publiczne do obsługi **kontrolerów macierzy** i **kontrolerów drzewa**:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Umożliwiają one kontrolery do żądania i modyfikować dane, które są wyświetlane. Jak narażonych `NSArray` powyżej, muszą one być bardzo określonych konwencji nazewnictwa (który różni się od typowego języka C# konwencji nazewnictwa):

- `addObject:` -Dodaje obiekt do tablicy.
- `insertObject:in{class_name}ArrayAtIndex:` — W przypadku gdy `{class_name}` jest nazwą klasy. Ta metoda wstawia obiektu do tablicy pod danym indeksem.
- `removeObjectFrom{class_name}ArrayAtIndex:` — W przypadku gdy `{class_name}` jest nazwą klasy. Ta metoda usuwa obiekt w tablicy pod danym indeksem.
- `set{class_name}Array:` — W przypadku gdy `{class_name}` jest nazwą klasy. Ta metoda pozwala zastąpić istniejące przeniesienia na nową.

Wewnątrz tych metod, będziemy opakowane zmiany do tablicy w `WillChangeValue` i `DidChangeValue` wiadomości pod kątem zgodności KVO.

Na koniec, ponieważ `Icon` właściwość opiera się na wartość `isManager` zmiany właściwości `isManager` właściwość nie może zostać uwzględniona w `Icon` dla danych powiązane elementy interfejsu użytkownika (w ramach KVO):

```csharp
[Export("Icon")]
public NSImage Icon {
    get {
        if (isManager) {
            return NSImage.ImageNamed ("group.png");
        } else {
            return NSImage.ImageNamed ("user.png");
        }
    }
}
``` 

Aby rozwiązać problem, który, użyjemy następujący kod:

```csharp
[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");
    }
}
```

Należy pamiętać, że oprócz własnego klucza `isManager` dostępu również wysyła `WillChangeValue` i `DidChangeValue` komunikaty dla `Icon` kluczy, więc zostanie wyświetlony, jak również zmiana.

Będziemy używać `PersonModel` modelu danych w dalszej części tego artykułu.

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>Proste powiązanie danych

Za pomocą naszych zdefiniowany Model danych Przyjrzyjmy się prosty przykład powiązanie danych w program Xcode Interface Builder. Na przykład możemy dodać formularz do naszej aplikacji rozszerzenia Xamarin.Mac, który może służyć do edycji `PersonModel` zdefiniowanego powyżej. Dodamy kilka pól tekstowych i pola wyboru, aby wyświetlić i edytować właściwości nasz model.

Najpierw Dodajmy nową **kontrolera widoku** do naszych **Main.storyboard** plik programu Interface Builder i nadaj jej klasa `SimpleViewController`: 

[![Dodawanie nowego kontrolera widoku](databinding-images/simple01.png "dodawania nowego kontrolera widoku")](databinding-images/simple01-large.png#lightbox)

Następnie wróć do programu Visual Studio dla komputerów Mac, Edytuj **SimpleViewController.cs** zawierający (została automatycznie dodana do naszego projektu) i udostępnić wystąpienia `PersonModel` będziemy naszego formularza do wiązania danych. Dodaj następujący kod:

```csharp
private PersonModel _person = new PersonModel();
...

[Export("Person")]
public PersonModel Person {
    get {return _person; }
    set {
        WillChangeValue ("Person");
        _person = value;
        DidChangeValue ("Person");
    }
}
```

Następnie po załadowaniu widoku, Utwórz wystąpienie naszych `PersonModel` i wypełnić ją przy użyciu tego kodu:

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set a default person
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    Person = Craig;

}
```

Teraz musimy utworzyć naszego formularza, kliknij dwukrotnie **Main.storyboard** plik, aby go otworzyć do edycji w programu Interface Builder. Układ formularza, aby wyglądać mniej więcej następująco:

[![Edytowanie scenorysu w środowisku Xcode](databinding-images/simple02.png "edycji scenorysu w środowisku Xcode")](databinding-images/simple02-large.png#lightbox)

Do danych należy powiązać formularz, aby `PersonModel` , firma Microsoft jest udostępniane za pośrednictwem `Person` klucza, wykonaj następujące czynności:

1. Wybierz **nazwiska pracownika** pole tekstowe, przełącz się do **Inspektor powiązania**.
2. Sprawdź **powiązać** pole, a następnie wybierz **proste kontrolera widoku** z listy rozwijanej. Następnie wprowadź `self.Person.Name` dla **ścieżka klucza**: 

    [![Wprowadzanie ścieżki klucza](databinding-images/simple03.png "wprowadzanie ścieżki klucza")](databinding-images/simple03-large.png#lightbox)
3. Wybierz **Dziadka** pole tekstowe i sprawdź **powiązać** pole, a następnie wybierz **proste kontrolera widoku** z listy rozwijanej. Następnie wprowadź `self.Person.Occupation` dla **ścieżka klucza**:  

    [![Wprowadzanie ścieżki klucza](databinding-images/simple04.png "wprowadzanie ścieżki klucza")](databinding-images/simple04-large.png#lightbox)
4. Wybierz **pracowników to Menedżer** pole wyboru i sprawdź **powiązać** pole, a następnie wybierz **proste kontrolera widoku** z listy rozwijanej. Następnie wprowadź `self.Person.isManager` dla **ścieżka klucza**:  

    [![Wprowadzanie ścieżki klucza](databinding-images/simple05.png "wprowadzanie ścieżki klucza")](databinding-images/simple05-large.png#lightbox)
5. Wybierz **liczbę zarządzanych pracowników** pole tekstowe i sprawdź **powiązać** pole, a następnie wybierz **proste kontrolera widoku** z listy rozwijanej. Następnie wprowadź `self.Person.NumberOfEmployees` dla **ścieżka klucza**:  

    [![Wprowadzanie ścieżki klucza](databinding-images/simple06.png "wprowadzanie ścieżki klucza")](databinding-images/simple06-large.png#lightbox)
6. Pracownik nie jest menedżera, chcemy ukryć numer z pracownikom zarządzane etykiety i pola tekstowego.
7. Wybierz **liczbę zarządzanych pracowników** etykiety, rozwiń węzeł **ukryty** turndown i sprawdź, czy **powiązać** pole, a następnie wybierz **proste kontrolera widoku** z listy rozwijanej. Następnie wprowadź `self.Person.isManager` dla **ścieżka klucza**:  

    [![Wprowadzanie ścieżki klucza](databinding-images/simple07.png "wprowadzanie ścieżki klucza")](databinding-images/simple07-large.png#lightbox)
8. Wybierz `NSNegateBoolean` z **przekształcania wartości** listy rozwijanej:  

    ![Wybieranie Przekształcenie klucza NSNegateBoolean](databinding-images/simple08.png "wybierając Przekształcenie klucza NSNegateBoolean")
9. Oznacza to, powiązań danych czy etykieta zostaną ukryte, jeśli wartość `isManager` właściwość `false`.
10. Powtórz kroki 7 i 8 dla **liczbę zarządzanych pracowników** pola tekstowego.
11. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Po uruchomieniu aplikacji, wartości z `Person` właściwości zostaną wypełnione automatycznie naszego formularza:

[![Wyświetlanie formularza automatycznie wypełniony](databinding-images/simple09.png "przedstawiający automatycznie wypełniony formularz")](databinding-images/simple09-large.png#lightbox)

Wszelkie zmiany, które użytkownicy sprawia, że do formularza będzie można zapisać zwrotnie w `Person` właściwości w kontroler widoku. Na przykład, usunięcie zaznaczenia **pracowników to Menedżer** aktualizacje `Person` wystąpienia naszej `PersonModel` i **liczbę zarządzanych pracowników** etykiety i pole tekstowe są ukryte automatycznie (za pośrednictwem Powiązanie danych):

[![Ukrywanie liczba pracowników dla osoby niebędące menedżerami](databinding-images/simple10.png "ukrywanie liczba pracowników dla osoby niebędące menedżerami")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>Powiązanie danych w widoku tabeli

Teraz, gdy mamy już podstawowe informacje dotyczące powiązania danych na bok, Przyjrzyjmy się bardziej złożone zadania powiązania danych przy użyciu _kontroler macierzy_ i powiązywanie danych do widoku tabeli. Aby uzyskać więcej informacji na temat pracy z widoki tabel, zobacz nasze [widoki tabel](~/mac/user-interface/table-view.md) dokumentacji.

Najpierw Dodajmy nową **kontrolera widoku** do naszych **Main.storyboard** plik programu Interface Builder i nadaj jej klasa `TableViewController`:

[![Dodawanie nowego kontrolera widoku](databinding-images/table01.png "dodawania nowego kontrolera widoku")](databinding-images/table01-large.png#lightbox)

Następnie umożliwia edytowanie **TableViewController.cs** plik (została automatycznie dodana do naszego projektu) i udostępniają tablicy (`NSArray`) z `PersonModel` klas, które firma Microsoft ponosi naszego formularza do wiązania danych. Dodaj następujący kod:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Tak samo, jak zrobiliśmy na `PersonModel` klasy powyżej w [zdefiniowanie modelu danych](#Defining_your_Data_Model) sekcji, możemy zostały ujawnione cztery specjalnej metody publiczne, aby kontroler macierzy i Odczyt i zapis danych z naszych kolekcji `PersonModels`.

Następnie po załadowaniu widoku należy wypełnić naszych tablicy przy użyciu tego kodu:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Teraz musimy utworzyć naszych widoku tabeli, kliknij dwukrotnie **Main.storyboard** plik, aby go otworzyć do edycji w programu Interface Builder. Układ tabeli, aby wyglądać mniej więcej następująco:

[![Projektowanie układu nowy widok tabeli](databinding-images/table02.png "układania nowy widok tabeli")](databinding-images/table02-large.png#lightbox)

Musimy dodać **kontroler macierzy** zapewnienie powiązane dane do tabeli, wykonaj następujące czynności:

1. Przeciągnij **tablicy kontrolera** z **Inspektor biblioteki** na **interfejs edytora**:  

    ![Wybiera kontroler macierzy z biblioteki](databinding-images/table03.png "wybierając kontroler macierzy z biblioteki")
2. Wybierz **kontroler macierzy** w **hierarchii interfejsów** i przełącz się do **Inspektor atrybut**:  

    [![Wybieranie Inspektor atrybuty](databinding-images/table04.png "wybierając Inspektor atrybutów")](databinding-images/table04-large.png#lightbox)
3. Wprowadź `PersonModel` dla **Nazwa klasy**, kliknij przycisk **oraz** przycisku i Dodaj trzy klucze. Nazwij je `Name`, `Occupation` i `isManager`:  

    ![Dodawanie wymaganych kluczy ścieżek](databinding-images/table05.png "dodanie wymaganych ścieżki klucza")
4. Informuje kontroler macierzy co zarządza tablicę, i właściwości, które powinien ujawniać (przy użyciu kluczy).
5. Przełącz się do **Inspektor powiązania** i w obszarze **zawartości tablicy** wybierz **powiązać** i **kontrolera widoku tabeli**. Wprowadź **modelu ścieżki klucza** z `self.personModelArray`:  

    ![Wprowadzanie ścieżki klucza](databinding-images/table06.png "wprowadzanie ścieżki klucza")
6. To wiąże kontroler macierzy do tablicy `PersonModels` , możemy uwidocznić na kontrolera widoku.

Teraz należy powiązać kontroler macierzy naszych widoku tabeli, wykonaj następujące czynności:

1. Wybierz widok tabeli i **powiązanie Inspektor**:  

    [![Wybieranie Inspektor powiązania](databinding-images/table07.png "wybierając Inspektor powiązania")](databinding-images/table07-large.png#lightbox)
2. W obszarze **treści** turndown wybierz **powiązać** i **kontroler macierzy**. Wprowadź `arrangedObjects` dla **klucz kontrolera** pola:  

    ![Zdefiniowanie klucza kontrolera](databinding-images/table08.png "zdefiniowanie klucza kontrolera")
3. Wybierz **komórka widoku tabeli** w obszarze **pracowników** kolumny. W **Inspektor powiązania** w obszarze **wartość** turndown wybierz **powiązać** i **widoku komórki tabeli**. Wprowadź `objectValue.Name` dla **modelu ścieżki klucza**:  

    [![Ustawianie ścieżki klucza modelu](databinding-images/table09.png "ustawienia ścieżki klucza w modelu")](databinding-images/table09-large.png#lightbox)
4. `objectValue` jest bieżący `PersonModel` w tablicy, zarządzane przez kontroler macierzy.
5. Wybierz **komórka widoku tabeli** w obszarze **Dziadka** kolumny. W **Inspektor powiązania** w obszarze **wartość** turndown wybierz **powiązać** i **widoku komórki tabeli**. Wprowadź `objectValue.Occupation` dla **modelu ścieżki klucza**:  

    [![Ustawianie ścieżki klucza modelu](databinding-images/table10.png "ustawienia ścieżki klucza w modelu")](databinding-images/table10-large.png#lightbox)
6. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Jeśli możemy uruchomić aplikację, tabela zostanie wypełniony naszych tablicę `PersonModels`:

[![Uruchomiona jest aplikacja](databinding-images/table11.png "uruchomiona jest aplikacja")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>Powiązanie danych widoku konspektu

Powiązanie danych dla widoku konspektu jest bardzo podobny do wiązania widoku tabeli. Kluczowa różnica polega na, będziemy używać **kontrolera drzewa** zamiast **kontroler macierzy** zapewnienie powiązane dane do widoku konspektu. Aby uzyskać więcej informacji na temat pracy z widoki konspektu, zobacz nasze [widoki konspektu](~/mac/user-interface/outline-view.md) dokumentacji.

Najpierw Dodajmy nową **kontrolera widoku** do naszych **Main.storyboard** plik programu Interface Builder i nadaj jej klasa `OutlineViewController`: 

[![Dodawanie nowego kontrolera widoku](databinding-images/outline01.png "dodawania nowego kontrolera widoku")](databinding-images/outline01-large.png#lightbox)

Następnie umożliwia edytowanie **OutlineViewController.cs** plik (została automatycznie dodana do naszego projektu) i udostępniają tablicy (`NSArray`) z `PersonModel` klas, które firma Microsoft ponosi naszego formularza do wiązania danych. Dodaj następujący kod:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Tak samo, jak zrobiliśmy na `PersonModel` klasy powyżej w [zdefiniowanie modelu danych](#Defining_your_Data_Model) sekcji, możemy zostały ujawnione cztery specjalnej metody publiczne, aby kontroler drzewa i Odczyt i zapis danych z naszych kolekcji `PersonModels`.

Następnie po załadowaniu widoku należy wypełnić naszych tablicy przy użyciu tego kodu:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (Craig);

    var Larry = new PersonModel ("Larry O'Brien", "API Documentation Manager");
    Larry.AddPerson (new PersonModel ("Mike Norman", "API Documenter"));
    AddPerson (Larry);

}
```

Teraz należy utworzyć widok naszej konspektu, kliknij dwukrotnie **Main.storyboard** plik, aby go otworzyć do edycji w programu Interface Builder. Układ tabeli, aby wyglądać mniej więcej następująco:

[![Tworzenie widoku konspektu](databinding-images/outline02.png "tworzenia widoku konspektu")](databinding-images/outline02-large.png#lightbox)

Musimy dodać **kontrolera drzewa** zapewnienie powiązane dane do naszych konspektu, wykonaj następujące czynności:

1. Przeciągnij **drzewa kontrolera** z **Inspektor biblioteki** na **interfejs edytora**:  

    ![Wybiera kontroler drzewa z biblioteki](databinding-images/outline03.png "wybiera kontroler drzewa z biblioteki")
2. Wybierz **kontrolera drzewa** w **hierarchii interfejsów** i przełącz się do **Inspektor atrybut**:  

    [![Wybieranie Inspektor atrybut](databinding-images/outline04.png "wybierając Inspektor atrybutu")](databinding-images/outline04-large.png#lightbox)
3. Wprowadź `PersonModel` dla **Nazwa klasy**, kliknij przycisk **oraz** przycisku i Dodaj trzy klucze. Nazwij je `Name`, `Occupation` i `isManager`:  

    ![Dodawanie wymaganych kluczy ścieżek](databinding-images/outline05.png "dodanie wymaganych ścieżki klucza")
4. Informuje kontrolera drzewa co zarządza tablicę, i właściwości, które powinien ujawniać (przy użyciu kluczy).
5. W obszarze **kontrolera drzewa** sekcji, wprowadź `personModelArray` dla **dzieci**, wprowadź `NumberOfEmployees` w obszarze **liczba** i wprowadź `isEmployee` w obszarze  **Liścia**:  

    ![Ustawianie ścieżki klucza kontrolera drzewa](databinding-images/outline05.png "ustawienia ścieżki klucza kontrolera drzewa")
6. Kontroler drzewa informuje gdzie można znaleźć wszystkich podrzędnych węzłów, ile węzły podrzędne są i czy bieżący węzeł ma węzły podrzędne.
7. Przełącz się do **Inspektor powiązania** i w obszarze **zawartości tablicy** wybierz **powiązać** i **właściciel pliku**. Wprowadź **modelu ścieżki klucza** z `self.personModelArray`:  

    ![Edytowanie ścieżki klucza](databinding-images/outline06.png "edytowanie ścieżki klucza")
8. To wiąże kontroler drzewa na tablicę `PersonModels` , możemy uwidocznić na kontrolera widoku.

Teraz należy powiązać naszych widoku konspektu z kontroler drzewa, wykonaj następujące czynności:

1. Wybierz widok konspektu i **powiązanie Inspektor** wybierz:  

    [![Wybieranie Inspektor powiązania](databinding-images/outline07.png "wybierając Inspektor powiązania")](databinding-images/outline07-large.png#lightbox)
2. W obszarze **konspektu Wyświetl zawartość** turndown wybierz **powiązać** i **kontrolera drzewa**. Wprowadź `arrangedObjects` dla **klucz kontrolera** pola:  

    ![Ustawianie klawisza kontrolera](databinding-images/outline08.png "klucza kontrolera")
3. Wybierz **komórka widoku tabeli** w obszarze **pracowników** kolumny. W **Inspektor powiązania** w obszarze **wartość** turndown wybierz **powiązać** i **widoku komórki tabeli**. Wprowadź `objectValue.Name` dla **modelu ścieżki klucza**:  

    [![Wprowadzanie ścieżki klucza modelu](databinding-images/outline09.png "wprowadzanie ścieżki klucza w modelu")](databinding-images/outline09-large.png#lightbox)
4. `objectValue` jest bieżący `PersonModel` w tablicy jest zarządzana przez kontroler drzewa.
5. Wybierz **komórka widoku tabeli** w obszarze **Dziadka** kolumny. W **Inspektor powiązania** w obszarze **wartość** turndown wybierz **powiązać** i **widoku komórki tabeli**. Wprowadź `objectValue.Occupation` dla **modelu ścieżki klucza**:  

    [![Wprowadzanie ścieżki klucza modelu](databinding-images/outline10.png "wprowadzanie ścieżki klucza w modelu")](databinding-images/outline10-large.png#lightbox)
6. Zapisz zmiany i powrócić do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode.

Jeśli możemy uruchomić aplikację, konspektu zostanie wypełniony naszych tablicę `PersonModels`:

[![Uruchomiona jest aplikacja](databinding-images/outline11.png "uruchomiona jest aplikacja")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>Powiązanie danych widoku kolekcji

Powiązanie danych z widokiem kolekcji jest bardzo podobne powiązanie z widoku tabeli, ponieważ kontroler macierzy jest używany do dostarczania danych dla kolekcji. Ponieważ widok kolekcji nie ma format wyświetlania wstępnie zdefiniowane, więcej pracy jest wymagane, aby przekazać opinię interakcji użytkownika oraz śledzenie wybór użytkownika.

> [!IMPORTANT]
> Ze względu na problem w środowisku Xcode 7 i macOS 10.11 (i nowszych) nie ma być używany w plikach scenorysu (.storyboard) będą widoki kolekcji. W rezultacie należy nadal używać .pliki XIb do definiowania widoków kolekcji dla aplikacji platformy Xamarin.Mac. Zobacz nasze [widoki kolekcji](~/mac/user-interface/collection-view.md) dokumentacji, aby uzyskać więcej informacji.

<!--KKM 012/16/2015 - Once Apple fixes the issue with Xcode and Collection Views in Storyboards, we can uncomment this section.

First, let's add a new **View Controller** to our **Main.storyboard** file in Interface Builder and name its class `CollectionViewController`: 

![](databinding-images/collection01.png)

Next, let's edit the **CollectionViewController.cs** file (that was automatically added to our project) and expose an array (`NSArray`) of `PersonModel` classes that we will be data binding our form to. Add the following code:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Just like we did on the `PersonModel` class above in the [Defining your Data Model](#Defining_your_Data_Model) section, we've exposed four specially named public methods so that the Array Controller and read and write data from our collection of `PersonModels`.

Next when the View is loaded, we need to populate our array with this code:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Now we need to create our Collection View, double-click the **Main.storyboard** file to open it for editing in Interface Builder. Layout the Collection View to look something like the following:

![](databinding-images/collection02.png)

When you add a Collection View to a User Interface design, two extra elements are also added:

1.  **Collection View Item** -  That manages a single instance of an item in the collection.
2.  **View** - A custom view that provides the visual size and appearance of each item in the collection. This view is tied to and managed by the **Collection View Item**.

Select the view and make it look like the following using an Image View and two Text Fields:

![](databinding-images/collection03.png)

One thing to note here, a `NSBox` was added behind everything in the view with the following attributes:

![](databinding-images/collection04.png)

We'll be using this box to provide feedback to the user when an item is selected in the Collection View.

We need to add an **Array Controller** to provide bound data to our collection, do the following:

1. Drag an **Array Controller** from the **Library Inspector** onto the **Interface Editor**: <br/>![](databinding-images/table03.png)
2. Select **Array Controller** in the **Interface Hierarchy** and switch to the **Attribute Inspector**: <br/>![](databinding-images/collection05.png)
3. Enter `PersonModel` for the **Class Name**, click the **Plus** button and add four Keys. Name them `Icon`, `Name`, `Occupation` and `isManager`: <br/>![](databinding-images/table05.png)
4. This tells the Array Controller what it is managing an array of, and which properties it should expose (via Keys).
5. Switch to the **Bindings Inspector** and under **Content Array** select **Bind to** and **File's Owner**. Enter a **Model Key Path** of `self.personModelArray`: <br/>![](databinding-images/table06.png)
6. This ties the Array Controller to the array of `PersonModels` that we exposed on our View Controller.

Now we need to bind our Collection View to the Array Controller, do the following:

1. Select the Collection View and the **Binding Inspector**: <br/>![](databinding-images/collection06.png)
2. Under the **Contents** turndown, select **Bind to** and **Array Controller**. Enter `arrangedObjects` for the **Controller Key** field: <br/>![](databinding-images/collection07.png)
3. Under the **Selection Indexes** turndown, select **Bind to** and **Array Controller**. Enter `selectionIndexes` for the **Controller Key** field: <br/>![](databinding-images/collection08.png)
4. Select the **Image View**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Person** (the name of our Collection View Item). Enter `representedObject.Icon` for the **Model Key Path**: <br/>![](databinding-images/collection09.png)
5. `representedObject` is the current `PersonModel` in the array being managed by the Array Controller.
6. Select the first **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Name` for the **Model Key Path**: <br/>![](databinding-images/collection10.png)
7. Select the second **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Occupation` for the **Model Key Path**: <br/>![](databinding-images/collection11.png)
8. Select the `NSBox`. In the **Bindings Inspector** under the **Hidden** turndown, select **Bind to** and **Collection View Item**. Enter `selected` for the **Model Key Path** and `NSNegateBoolean` for the **Value Transformer**: <br/>![](databinding-images/collection12.png)
9. Save your changes and return to Visual Studio for Mac to sync with Xcode.

If we run the application, the table will be populated with our array of `PersonModels`:

![](databinding-images/collection13.png)

For more information on working with Collection Views, please see our [Collection Views](~/mac/user-interface/collection-view.md) documentation.-->

## <a name="debugging-native-crashes"></a>Debugowanie natywnej awarii

Pomyłki w wiązania danych może spowodować _natywnej awarii_ w kod niezarządzany i spowodować, że aplikacja platformy Xamarin.Mac zakończy się niepowodzeniem z `SIGABRT` błąd:

[![Przykład okno dialogowe natywnej awarii](databinding-images/debug01.png "przykład okno dialogowe natywnej awarii")](databinding-images/debug01-large.png#lightbox)

Podczas tworzenia powiązań danych zazwyczaj są cztery główne przyczyny natywnej awarii:

1. Model danych nie dziedziczy `NSObject` lub podklasa `NSObject`.
2. Twoja własność za pomocą języka Objective-C nie udostępnił `[Export("key-name")]` atrybutu.
3. Nie zawinięty zmiany wartości metody dostępu w `WillChangeValue` i `DidChangeValue` wywołania metody (Określanie tego samego klucza `Export` atrybutu).
4. Masz problem lub błędnie wpisane klucz w **powiązanie Inspektor** w programu Interface Builder.

### <a name="decoding-a-crash"></a>Dekodowanie awarii

Możemy spowodować natywnej awarii wiązania z danymi, dzięki czemu firma Microsoft pokazują, jak zlokalizować i rozwiązać ten problem. W programu Interface Builder Zmieńmy nasz powiązania pierwsza etykieta w przykładzie widok kolekcji z `Name` do `Title`:

[![Edytowanie klucz powiązania](databinding-images/debug02.png "edycji klucz powiązania")](databinding-images/debug02-large.png#lightbox)

Teraz zapisać zmiany, przełącz się do programu Visual Studio dla komputerów Mac do synchronizacji z narzędziem Xcode i uruchomić aplikację. Gdy zostanie wyświetlony widok kolekcji, aplikacja chwilowo ulegnie awarii z `SIGABRT` błąd (jak pokazano na **dane wyjściowe aplikacji** w programie Visual Studio dla komputerów Mac) od czasu `PersonModel` nie ujawnia właściwość przy użyciu klucza `Title`:

[![Błąd wiązania się z przykładem](databinding-images/debug03.png "błąd wiązania się z przykładem")](databinding-images/debug03-large.png#lightbox)

Firma Microsoft przewijania na samej górze błąd w **dane wyjściowe aplikacji** widać klawisz w celu rozwiązania problemu:

[![Znajdowanie problemu w dzienniku błędów](databinding-images/debug04.png "znajdowanie problemu w dzienniku błędów programu")](databinding-images/debug04-large.png#lightbox)

Ten wiersz informuje NAS, że klucz `Title` nie istnieje w obiekcie, który możemy dokonywane jest wiązanie. Jeśli zmienimy powiązanie z powrotem do `Name` w programu Interface Builder, Zapisz, synchronizacja, ponownie skompilować i uruchomić, aplikacja będzie działać zgodnie z oczekiwaniami, bez problemu.

## <a name="summary"></a>Podsumowanie

W tym artykule jest zajęta szczegółowe omówienie pracy z powiązania danych i kodowanie klucz wartość w aplikacji platformy Xamarin.Mac. Po pierwsze znaleziono w znajdowaniu klasy C# do języka Objective-C przy użyciu kodowanie klucz wartość (KVC) i klucz wartość obserwowania (KVO). Następnie pokazano, jak używać klasy zgodne KVO i powiązanie danych go do elementów interfejsu użytkownika w program Xcode Interface Builder. Na koniec wykazało złożone powiązanie, używając **kontrolerów macierzy** i **kontrolerów drzewa**.


## <a name="related-links"></a>Linki pokrewne

- [MacDatabinding scenorysu (przykład)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [XIb MacDatabinding (przykład)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Kontrolki standardowe](~/mac/user-interface/standard-controls.md)
- [Widoki tabel](~/mac/user-interface/table-view.md)
- [Widoki konspektu](~/mac/user-interface/outline-view.md)
- [Widoki kolekcji](~/mac/user-interface/collection-view.md)
- [Pary klucz wartość, kodowania, przewodnik programowania](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Wprowadzenie do obserwowania pary klucz wartość przewodnik programowania](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Wprowadzenie do powiązania cocoa dla tematów związanych z programowaniem](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Wprowadzenie do odwołania powiązania Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [System macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
