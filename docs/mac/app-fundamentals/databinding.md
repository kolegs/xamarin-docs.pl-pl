---
title: Powiązanie danych i kluczy i wartości kodowania w Xamarin.Mac
description: W tym artykule omówiono przy użyciu kluczy i wartości kodowania i klucz wartość obserwowania, aby umożliwić wiązanie danych do elementów interfejsu użytkownika w Konstruktorze interfejsu w środowisku Xcode.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 88567e47f488a94fcf7334584a678c9689b83306
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792141"
---
# <a name="data-binding-and-key-value-coding-in-xamarinmac"></a>Powiązanie danych i kluczy i wartości kodowania w Xamarin.Mac

_W tym artykule omówiono przy użyciu kluczy i wartości kodowania i klucz wartość obserwowania, aby umożliwić wiązanie danych do elementów interfejsu użytkownika w Konstruktorze interfejsu w środowisku Xcode._

## <a name="overview"></a>Omówienie

Podczas pracy z C# i .NET w aplikacji Xamarin.Mac, masz dostęp do tej samej wartości klucza kodowania i technik wiązania danych który deweloper pracujący *Objective-C* i *Xcode* jest. Ponieważ Xamarin.Mac integruje się bezpośrednio z Xcode, można użyć w środowisku Xcode _konstruktora interfejsu_ powiązania danych z elementów interfejsu użytkownika zamiast pisania kodu.

Przy użyciu kodowania i powiązanie technik w aplikacji Xamarin.Mac danych klucz wartość, można znacznie zmniejszyć ilość kodu, które trzeba zapisać i zachować do wypełnienia i pracować z nimi elementy interfejsu użytkownika. Masz również zaletą dalsze oddzielenie danych zapasowy (_modelu danych_) od z przodu kończyć interfejsu użytkownika (_Model-View-Controller_), prowadzących do łatwiejsze w obsłudze, bardziej elastyczne aplikacji Projekt.

[![Przykład uruchomionej aplikacji](databinding-images/intro01.png "przykładem uruchomionej aplikacji")](databinding-images/intro01-large.png#lightbox)

W tym artykule omówione zostaną następujące czynności podstawowe informacje dotyczące pracy z kluczy i wartości kodowania i powiązanie danych w aplikacji Xamarin.Mac. Zdecydowanie zaleca się pracę za pośrednictwem [Hello, Mac](~/mac/get-started/hello-mac.md) artykuł najpierw, w szczególności [wprowadzenie do programów Xcode i kompilatora interfejsu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) i [gniazda i akcje](~/mac/get-started/hello-mac.md#Outlets_and_Actions) sekcje, w jakiej omawia kluczowe założenia i techniki, które będzie używana w tym artykule.

Może zajść potrzeba Przyjrzyjmy się [udostępnianie klasy języka C# / metody Objective-C](~/mac/internals/how-it-works.md) sekcji [wewnętrzne Xamarin.Mac](~/mac/internals/how-it-works.md) dokumentu również wyjaśniono `Register` i `Export` atrybutów umożliwiają połączenie klas C# do obiektów języka Objective C i interfejsu użytkownika elementy.

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>Co to jest klucz wartość kodowania

Klucz wartość kodowania (KVC) to mechanizm służący do uzyskiwania dostępu do właściwości obiektu pośrednio, za pomocą kluczy (specjalnie sformatowana ciągi) do identyfikowania właściwości zamiast dostępu do nich za pośrednictwem zmienne wystąpienia lub metody dostępu (`get/set`). Implementując kluczy i wartości kodowania zgodne metod dostępu do aplikacji Xamarin.Mac, uzyskasz dostęp do innych funkcji macOS (wcześniej znane jako OS X), takich jak obserwowania klucz wartość (KVO), wiązanie danych podstawowych danych, Cocoa powiązania i scriptability.

Przy użyciu kodowania i powiązanie technik w aplikacji Xamarin.Mac danych klucz wartość, można znacznie zmniejszyć ilość kodu, które trzeba zapisać i zachować do wypełnienia i pracować z nimi elementy interfejsu użytkownika. Masz również zaletą dalsze oddzielenie danych zapasowy (_modelu danych_) od z przodu kończyć interfejsu użytkownika (_Model-View-Controller_), prowadzących do łatwiejsze w obsłudze, bardziej elastyczne aplikacji Projekt. 

Na przykład Przyjrzyjmy się następujące definicji klasy obiektu zgodne KVC:

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

Najpierw `[Register("PersonModel")]` atrybut rejestruje klasę i naraża go na Objective-C. Następnie klasy musi dziedziczyć `NSObject` (lub podklasa klasy, która dziedziczy `NSObject`), spowoduje to dodanie kilka podstawowa metoda umożliwiająca do klasy jako KVC zgodne. Następnie `[Export("Name")]` atrybutu ujawnia `Name` właściwości i definiuje wartości klucza, który później będzie używane do dostępu do właściwości, za pomocą techniki KVC i KVO. 

Finally, aby mogły być obserwowane klucz-wartość zmienia się na wartość właściwości akcesor musi zawijać zmiany jego wartości w `WillChangeValue` i `DidChangeValue` wywołania metody (Określanie tego samego klucza `Export` atrybutu).  Na przykład:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

Ten krok jest _bardzo_ ważne dla powiązania danych w środowisku Xcode do konstruktora interfejsu (jak zostanie wyświetlone w dalszej części tego artykułu).

Aby uzyskać więcej informacji, zobacz firmy Apple [kluczy i wartości kodowania przewodnik programowania w języku](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html).

### <a name="keys-and-key-paths"></a>Klucze i ścieżki klucza

A _klucza_ jest ciągiem, który identyfikuje określonych właściwości obiektu. Zazwyczaj klucz odpowiada nazwa metody dostępu kodowania obiektów zgodnego z wartości klucza. Klucze muszą używać kodowanie ASCII, zwykle zaczynać się małą literą i nie może zawierać spacji. Dlatego podane w przykładzie powyżej `Name` będzie wartość klucza `Name` właściwość `PersonModel` klasy. Klucz i nazwa właściwości, które udostępniają nie muszą być takie same, jednak w większości przypadków będą.

A _ścieżka klucza_ jest ciągiem kropka oddzielone kluczy używanych do określania hierarchii właściwości obiektu do przechodzenia. Właściwości pierwszego klucza w sekwencji jest określana względem odbiornika, a każdy klucz kolejnych jest obliczane względem wartości właściwości poprzedniej. W ten sam sposób umożliwia kropkowego przechodzenie przez obiekt i jego właściwości w klasie C#.

Na przykład, jeśli zostanie rozwinięty `PersonModel` klasy i dodać `Child` właściwości:

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

Ścieżka klucza do tego elementu podrzędnego name `self.Child.Name` lub po prostu `Child.Name` (oparte na jak wartości klucza było ono używane).

### <a name="getting-values-using-key-value-coding"></a>Pobieranie wartości przy użyciu kodowania klucz wartość

`ValueForKey` Metoda zwraca wartość dla określonego klucza (jako `NSString`), względnym w stosunku do wystąpienia klasy KVC żądania odbierania. Na przykład jeśli `Person` jest wystąpieniem `PersonModel` klasy zdefiniowanych powyżej:

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

To spowoduje zwrócenie wartości `Name` właściwości danego wystąpienia `PersonModel`. 

### <a name="setting-values-using-key-value-coding"></a>Ustawienie wartości przy użyciu kodowania klucz wartość

Podobnie `SetValueForKey` wartości dla określonego klucza (jako `NSString`), względnym w stosunku do wystąpienia klasy KVC żądania odbierania. Aby ponownie za pomocą wystąpienia `PersonModel` klasy, jak pokazano poniżej:

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

Czy zmienić wartość `Name` właściwości `Jane Doe`.

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>Obserwowania zmiany wartości

Przy użyciu kluczy i wartości obserwowania (KVO), można dołączyć obserwatora do określonego klucza klasy zgodne KVC i powiadomić zawsze, gdy wartość dla tego klucza jest modyfikowana (przy użyciu technik KVC lub bezpośredni dostęp do danej właściwości w kodzie języka C#). Na przykład:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

Teraz, w dowolnym momencie `Name` właściwość `Person` wystąpienie `PersonModel` klasy jest modyfikowany, nowa wartość jest zapisywany do konsoli. 

Aby uzyskać więcej informacji, zobacz firmy Apple [wprowadzenie do klucz-wartość obserwowania przewodnik programowania w języku](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i).

## <a name="data-binding"></a>Powiązanie danych

Poniższe sekcje pokazują, jak można użyć kluczy i wartości kodowania i klucz wartość obserwowania klasy zgodne wiązanie danych do elementów interfejsu użytkownika w Konstruktorze interfejsu w programie Xcode, zamiast odczytywanie i zapisywanie wartości przy użyciu kodu C#. W ten sposób można oddzielić Twojej _modelu danych_ z widoków, które są używane, aby je wyświetlić, co aplikacja Xamarin.Mac bardziej elastyczne i łatwiejsze w obsłudze. Znacznie zmniejszyć ilość kodu, który ma zostać zapisany.

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>Definiowanie modelu danych

Zanim będzie można powiązać danych elementu interfejsu użytkownika w Konstruktorze interfejsu, musi mieć klasę zgodne KVC/KVO zdefiniowane w aplikacji do działania jako Xamarin.Mac _modelu danych_ dla wiązania. Model danych zawiera wszystkie dane, która będzie wyświetlana w interfejsie użytkownika i odbiera wszelkie modyfikacje dane użytkownika w interfejsie użytkownika podczas uruchamiania aplikacji.

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

Większość funkcji tej klasy zostały omówione w [co to jest klucz wartość kodowania](#What_is_Key-Value_Coding) powyższej sekcji. Jednak Przyjrzyjmy się kilka określonych elementów i występują pewne różnice, które zostały wprowadzone, aby zezwolić na działanie jako Model danych dla tej klasy **kontrolerów macierzy** i **kontrolerów drzewa** (która będzie używana później do danych Powiąż **widok drzewa**, **widoków konspektu** i **widoki kolekcji**).

Najpierw, ponieważ pracownik może być Menedżer, możemy używano `NSArray` (w szczególności `NSMutableArray` wartości mogą być modyfikowane) sposób umożliwić pracownikom, zarządzanych jest dołączony do nich:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

Dwie rzeczy do uwzględnienia w tym miejscu:

1. Użyliśmy `NSMutableArray` zamiast standardowego tablicy C# lub kolekcji, ponieważ takie jak jest to wymagane do powiązania danych z formantami AppKit **widoków tabel**, **widoków konspektu** i **kolekcje** .
2. Tablica pracowników możemy udostępnianych przez rzutowanie na `NSArray` dla nazwy, sformatowanych danych powiązania celów i zmienić jej C# `People`, na taki, który oczekuje wiązania z danymi, `personModelArray` w formularzu **{class_name} tablicy** (Uwaga czy pierwszy znak przeprowadzono małe litery).

Następnie należy dodać niektóre specjalnie nazwy publicznej metody do obsługi **kontrolerów macierzy** i **kontrolerów drzewa**:

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

Umożliwiają one kontrolerów do żądania i modyfikować dane, które są one wyświetlane. Jak narażonych `NSArray` powyżej, mają one bardzo określonych konwencji nazewnictwa (który różni się od typowego C# konwencje nazewnictwa):

- `addObject:` -Dodaje obiekt do tablicy.
- `insertObject:in{class_name}ArrayAtIndex:` -Gdy `{class_name}` jest nazwą klasy. Ta metoda wstawia obiektu do tablicy w danym indeksie.
- `removeObjectFrom{class_name}ArrayAtIndex:` -Gdy `{class_name}` jest nazwą klasy. Ta metoda usuwa obiektu w tablicy w danym indeksie.
- `set{class_name}Array:` -Gdy `{class_name}` jest nazwą klasy. Ta metoda pozwala zastąpić istniejące przenoszące nową.

Wewnątrz tych metod, możemy opakowana zmiany do tablicy w `WillChangeValue` i `DidChangeValue` wiadomości KVO zgodności.

Na koniec od `Icon` zależy od wartości właściwości `isManager` zmiany właściwości, aby `isManager` właściwości mogą nie zostać odzwierciedlone w `Icon` dla danych powiązane elementy interfejsu użytkownika (podczas KVO):

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

Aby poprawić, który, możemy użyć poniższego kodu:

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

Należy pamiętać, że oprócz własnego klucza `isManager` również wysyła akcesor `WillChangeValue` i `DidChangeValue` komunikaty dla `Icon` klucza, zostanie wyświetlony również zmianę.

Będziemy używać `PersonModel` modelu danych w dalszej części tego artykułu.

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>Proste powiązanie danych

Z naszych Model danych zdefiniowany Przyjrzyjmy się prosty przykład powiązanie danych w Konstruktorze interfejsu w środowisku Xcode. Na przykład Dodajmy formularza do naszej aplikacji Xamarin.Mac, który może służyć do edycji `PersonModel` zdefiniowanego powyżej. Dodamy kilka pól tekstowych i pola wyboru, aby wyświetlić i edytować właściwości modelu.

Najpierw możemy dodać nowy **kontrolera widoku** do naszej **Main.storyboard** pliku w Konstruktorze interfejsu i nazwę swojej klasy `SimpleViewController`: 

[![Dodawanie nowego kontrolera widoku](databinding-images/simple01.png "dodawania nowego kontrolera widoku")](databinding-images/simple01-large.png#lightbox)

Następnie wróć do programu Visual Studio dla komputerów Mac, Edytuj **SimpleViewController.cs** plików (do którego została automatycznie dodana do naszej projektu) i ujawnia wystąpienia `PersonModel` będziemy naszego formularza do powiązania danych. Dodaj następujący kod:

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

Następnie po załadowaniu widoku umożliwia utworzenie wystąpienia naszych `PersonModel` i wypełnić ją ten kod:

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

Teraz należy utworzyć naszego formularza, kliknij dwukrotnie **Main.storyboard** plik, aby otworzyć do edycji w Konstruktorze interfejsu. Układ formularza, aby wyglądać jak poniżej:

[![Edytowanie scenorysu w środowisku Xcode](databinding-images/simple02.png "edycji scenorysu w środowisku Xcode")](databinding-images/simple02-large.png#lightbox)

Dane powiązać formularza, aby `PersonModel` który mamy udostępniane za pośrednictwem funkcji `Person` klucza, wykonaj następujące czynności:

1. Wybierz **nazwa pracownika** pola tekstowego i przełącznik **inspektora powiązania**.
2. Sprawdź **powiązać** i wybierz **proste kontrolera widoku** z listy rozwijanej. Następnie wprowadź `self.Person.Name` dla **ścieżka klucza**: 

    [![Wprowadzanie ścieżki klucza](databinding-images/simple03.png "wprowadzanie ścieżki klucza")](databinding-images/simple03-large.png#lightbox)
3. Wybierz **zawód** pola tekstowego i wyboru **powiązać** i wybierz **proste kontrolera widoku** z listy rozwijanej. Następnie wprowadź `self.Person.Occupation` dla **ścieżka klucza**:  

    [![Wprowadzanie ścieżki klucza](databinding-images/simple04.png "wprowadzanie ścieżki klucza")](databinding-images/simple04-large.png#lightbox)
4. Wybierz **pracownik jest kierownikiem** wyboru i sprawdź **powiązać** i wybierz **proste kontrolera widoku** z listy rozwijanej. Następnie wprowadź `self.Person.isManager` dla **ścieżka klucza**:  

    [![Wprowadzanie ścieżki klucza](databinding-images/simple05.png "wprowadzanie ścieżki klucza")](databinding-images/simple05-large.png#lightbox)
5. Wybierz **numer zarządzane pracowników** pola tekstowego i wyboru **powiązać** i wybierz **proste kontrolera widoku** z listy rozwijanej. Następnie wprowadź `self.Person.NumberOfEmployees` dla **ścieżka klucza**:  

    [![Wprowadzanie ścieżki klucza](databinding-images/simple06.png "wprowadzanie ścieżki klucza")](databinding-images/simple06-large.png#lightbox)
6. Jeśli pracownik nie jest Menedżer, chcemy ukryć numer z pracowników zarządzane etykiety i pola tekstowego.
7. Wybierz **numer zarządzane pracowników** etykiety, rozwiń węzeł **Hidden** turndown i zaznacz pole wyboru **powiązać** i wybierz **proste kontrolera widoku** z listy rozwijanej. Następnie wprowadź `self.Person.isManager` dla **ścieżka klucza**:  

    [![Wprowadzanie ścieżki klucza](databinding-images/simple07.png "wprowadzanie ścieżki klucza")](databinding-images/simple07-large.png#lightbox)
8. Wybierz `NSNegateBoolean` z **transformatora wartość** listy rozwijanej:  

    ![Wybieranie Przekształcenie klucza NSNegateBoolean](databinding-images/simple08.png "wybranie Przekształcenie klucza NSNegateBoolean")
9. Informuje wiązania danych czy etykieta zostanie ukryte, jeśli wartość `isManager` jest właściwość `false`.
10. Powtórz kroki 7 i 8 dla **numer zarządzane pracowników** pola tekstowego.
11. Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Po uruchomieniu aplikacji wartości z `Person` właściwości zostaną wypełnione automatycznie naszego formularza:

[![Wyświetlanie formularza wypełniana automatycznie](databinding-images/simple09.png "przedstawiający formularza wypełniana automatycznie")](databinding-images/simple09-large.png#lightbox)

Wszelkie zmiany, które użytkownicy, wysyła do formularza będą zapisywane ponownie `Person` właściwości w widoku kontrolera. Na przykład unselecting **pracownik jest kierownikiem** aktualizacje `Person` wystąpienie naszych `PersonModel` i **numer zarządzane pracowników** etykiety i pola tekstowego są ukryte automatycznie (za pośrednictwem Powiązanie danych):

[![Ukrywanie liczba pracowników z systemem innym niż menedżerów](databinding-images/simple10.png "ukrywanie liczba pracowników z systemem innym niż menedżerów")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>Powiązanie danych w widoku tabeli

Teraz, gdy mamy podstawy sposób powiązania danych, Przyjrzyjmy się bardziej złożonych zadań powiązania danych przy użyciu _kontroler macierzy_ i powiązanie danych w tabeli. Aby uzyskać więcej informacji na temat pracy z widokami tabeli, zobacz nasze [widoków tabel](~/mac/user-interface/table-view.md) dokumentacji.

Najpierw możemy dodać nowy **kontrolera widoku** do naszej **Main.storyboard** pliku w Konstruktorze interfejsu i nazwę swojej klasy `TableViewController`:

[![Dodawanie nowego kontrolera widoku](databinding-images/table01.png "dodawania nowego kontrolera widoku")](databinding-images/table01-large.png#lightbox)

Następnie umożliwia edytowanie **TableViewController.cs** plik (została automatycznie dodana do naszej projektu) i Ujawnij tablicy (`NSArray`) z `PersonModel` klasy będziemy naszego formularza do powiązania danych. Dodaj następujący kod:

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

Tak samo, jak robiliśmy na `PersonModel` klasy powyżej w [Definiowanie modelu danych](#Defining_your_Data_Model) sekcji możemy zostały uwidocznione cztery specjalnej metody publiczne, aby kontroler macierzy i odczytu i zapisu danych z naszych kolekcji `PersonModels`.

Następnie po załadowaniu widoku, musimy wypełnić naszych tablicy o tym kodzie:

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

Teraz należy utworzyć naszych widoku tabeli, kliknij dwukrotnie **Main.storyboard** plik, aby otworzyć do edycji w Konstruktorze interfejsu. Układ Tabela, która ma wyglądać jak poniżej:

[![Projektowanie układu nowy widok tabeli](databinding-images/table02.png "układania nowy widok tabeli")](databinding-images/table02-large.png#lightbox)

Musimy dodać **kontroler macierzy** zapewnienie powiązana z danymi do naszej tabeli, wykonaj następujące czynności:

1. Przeciągnij **tablicy kontrolera** z **inspektora biblioteki** na **interfejs edytora**:  

    ![Wybiera kontroler macierzy z biblioteki](databinding-images/table03.png "wybiera kontroler macierzy z biblioteki")
2. Wybierz **kontroler macierzy** w **hierarchii interfejsów** i przejdź do **inspektora atrybutu**:  

    [![Wybieranie inspektora atrybuty](databinding-images/table04.png "wybranie inspektora atrybutów")](databinding-images/table04-large.png#lightbox)
3. Wprowadź `PersonModel` dla **Nazwa klasy**, kliknij przycisk **Plus** przycisk, a następnie dodaj trzy klucze. Nazwa je `Name`, `Occupation` i `isManager`:  

    ![Dodawanie wymaganych ścieżki klucza](databinding-images/table05.png "Dodawanie wymagane ścieżki klucza")
4. Informuje kontroler macierzy co zarządza tablicą, i właściwości, które go powinny ujawniać (za pośrednictwem kluczy).
5. Przełącz się do **inspektora powiązania** i w obszarze **zawartości tablicy** wybierz **powiązać** i **kontrolera widoku tabeli**. Wprowadź **modelu ścieżki klucza** z `self.personModelArray`:  

    ![Wprowadzanie ścieżki klucza](databinding-images/table06.png "wprowadzanie ścieżki klucza")
6. To wiąże kontroler macierzy do tablicy z `PersonModels` który mamy narażone na kontrolera widoku.

Teraz należy powiązać naszych widoku tabeli kontroler macierzy, wykonaj następujące czynności:

1. Wybierz widok tabeli i **powiązanie inspektora**:  

    [![Wybieranie inspektora powiązania](databinding-images/table07.png "wybranie inspektora powiązania")](databinding-images/table07-large.png#lightbox)
2. W obszarze **spisu treści** turndown, wybierz opcję **powiązać** i **kontroler macierzy**. Wprowadź `arrangedObjects` dla **klucza kontrolera** pola:  

    ![Definiowanie klucza kontrolera](databinding-images/table08.png "Definiowanie klucza kontrolera")
3. Wybierz **komórkę widoku tabeli** w obszarze **pracownika** kolumny. W **inspektora powiązania** w obszarze **wartość** turndown, wybierz opcję **powiązać** i **widoku komórki tabeli**. Wprowadź `objectValue.Name` dla **modelu ścieżki klucza**:  

    [![Ustawianie ścieżki klucza modelu](databinding-images/table09.png "Ustawianie ścieżki klucza w modelu")](databinding-images/table09-large.png#lightbox)
4. `objectValue` jest bieżącą `PersonModel` w tablicy zarządzany przez kontroler macierzy.
5. Wybierz **komórkę widoku tabeli** w obszarze **zawód** kolumny. W **inspektora powiązania** w obszarze **wartość** turndown, wybierz opcję **powiązać** i **widoku komórki tabeli**. Wprowadź `objectValue.Occupation` dla **modelu ścieżki klucza**:  

    [![Ustawianie ścieżki klucza modelu](databinding-images/table10.png "Ustawianie ścieżki klucza w modelu")](databinding-images/table10-large.png#lightbox)
6. Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Czy możemy uruchomić aplikację, tabeli zostaną wypełnione z naszych tablicę `PersonModels`:

[![Uruchamianie aplikacji](databinding-images/table11.png "uruchamiania aplikacji")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>Powiązanie danych widoku konspektu

Powiązanie danych z widoku konspektu jest bardzo podobny do wiązania widoku tabeli. Najważniejsza różnica polega na to, że będziemy używać **kontrolera drzewa** zamiast **kontroler macierzy** zapewnienie powiązana z danymi widoku konspektu. Aby uzyskać więcej informacji na temat pracy z widokami konspektu, zobacz nasze [widoków konspektu](~/mac/user-interface/outline-view.md) dokumentacji.

Najpierw możemy dodać nowy **kontrolera widoku** do naszej **Main.storyboard** pliku w Konstruktorze interfejsu i nazwę swojej klasy `OutlineViewController`: 

[![Dodawanie nowego kontrolera widoku](databinding-images/outline01.png "dodawania nowego kontrolera widoku")](databinding-images/outline01-large.png#lightbox)

Następnie umożliwia edytowanie **OutlineViewController.cs** plik (została automatycznie dodana do naszej projektu) i Ujawnij tablicy (`NSArray`) z `PersonModel` klasy będziemy naszego formularza do powiązania danych. Dodaj następujący kod:

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

Tak samo, jak robiliśmy na `PersonModel` klasy powyżej w [Definiowanie modelu danych](#Defining_your_Data_Model) sekcji możemy zostały uwidocznione cztery specjalnej metody publiczne, aby kontroler drzewa i odczytu i zapisu danych z naszych kolekcji `PersonModels`.

Następnie po załadowaniu widoku, musimy wypełnić naszych tablicy o tym kodzie:

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

Teraz należy utworzyć naszych widoku konspektu, kliknij dwukrotnie **Main.storyboard** plik, aby otworzyć do edycji w Konstruktorze interfejsu. Układ Tabela, która ma wyglądać jak poniżej:

[![Tworzenie widoku konspektu](databinding-images/outline02.png "tworzenia widoku konspektu")](databinding-images/outline02-large.png#lightbox)

Musimy dodać **kontrolera drzewa** zapewnienie powiązana z danymi do naszej konspektu, wykonaj następujące czynności:

1. Przeciągnij **drzewa kontrolera** z **inspektora biblioteki** na **interfejs edytora**:  

    ![Wybiera kontroler drzewa z biblioteki](databinding-images/outline03.png "wybiera kontroler drzewa z biblioteki")
2. Wybierz **kontrolera drzewa** w **hierarchii interfejsów** i przejdź do **inspektora atrybutu**:  

    [![Wybranie atrybutu inspektora](databinding-images/outline04.png "wybranie inspektora atrybutu")](databinding-images/outline04-large.png#lightbox)
3. Wprowadź `PersonModel` dla **Nazwa klasy**, kliknij przycisk **Plus** przycisk, a następnie dodaj trzy klucze. Nazwa je `Name`, `Occupation` i `isManager`:  

    ![Dodawanie wymaganych ścieżki klucza](databinding-images/outline05.png "Dodawanie wymagane ścieżki klucza")
4. Informuje kontrolera drzewa co zarządza tablicą, i właściwości, które go powinny ujawniać (za pośrednictwem kluczy).
5. W obszarze **kontrolera drzewa** wprowadź `personModelArray` dla **dzieci**, wprowadź `NumberOfEmployees` w obszarze **liczba** , a następnie wprowadź `isEmployee` w obszarze  **Liścia**:  

    ![Ustawianie ścieżki klucza kontrolera drzewa](databinding-images/outline05.png "Ustawianie ścieżki klucza kontrolera drzewa")
6. Ta wartość informuje kontrolera drzewa gdzie można znaleźć wszystkich podrzędnych węzłów, ile węzły podrzędne są i jeśli bieżący węzeł ma podrzędnych węzłów.
7. Przełącz się do **inspektora powiązania** i w obszarze **zawartości tablicy** wybierz **powiązać** i **właścicielem pliku**. Wprowadź **modelu ścieżki klucza** z `self.personModelArray`:  

    ![Edytowanie ścieżki klucza](databinding-images/outline06.png "edycji ścieżki klucza")
8. To wiąże kontrolera drzewa do tablicy z `PersonModels` który mamy narażone na kontrolera widoku.

Teraz należy powiązać naszych widoku konspektu kontrolera drzewa, wykonaj następujące czynności:

1. Wybierz widok konspektu i **powiązanie inspektora** wybierz:  

    [![Wybieranie inspektora powiązania](databinding-images/outline07.png "wybranie inspektora powiązania")](databinding-images/outline07-large.png#lightbox)
2. W obszarze **konspektu Wyświetl zawartość** turndown, wybierz opcję **powiązać** i **kontrolera drzewa**. Wprowadź `arrangedObjects` dla **klucza kontrolera** pola:  

    ![Ustawianie klawisza kontrolera](databinding-images/outline08.png "ustawiania klucza kontrolera")
3. Wybierz **komórkę widoku tabeli** w obszarze **pracownika** kolumny. W **inspektora powiązania** w obszarze **wartość** turndown, wybierz opcję **powiązać** i **widoku komórki tabeli**. Wprowadź `objectValue.Name` dla **modelu ścieżki klucza**:  

    [![Wprowadzanie ścieżki klucza modelu](databinding-images/outline09.png "wprowadzanie ścieżki klucza w modelu")](databinding-images/outline09-large.png#lightbox)
4. `objectValue` jest bieżącą `PersonModel` w tablicy zarządzany przez kontroler drzewa.
5. Wybierz **komórkę widoku tabeli** w obszarze **zawód** kolumny. W **inspektora powiązania** w obszarze **wartość** turndown, wybierz opcję **powiązać** i **widoku komórki tabeli**. Wprowadź `objectValue.Occupation` dla **modelu ścieżki klucza**:  

    [![Wprowadzanie ścieżki klucza modelu](databinding-images/outline10.png "wprowadzanie ścieżki klucza w modelu")](databinding-images/outline10-large.png#lightbox)
6. Zapisz zmiany i wróć do programu Visual Studio for Mac synchronizację w programie Xcode.

Czy możemy uruchomić aplikację, konturu zostaną wypełnione z naszych tablicę `PersonModels`:

[![Uruchamianie aplikacji](databinding-images/outline11.png "uruchamiania aplikacji")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>Powiązanie danych widoku kolekcji

Powiązanie danych z widokiem kolekcji jest bardzo podobne powiązania z widokiem tabeli jako kontroler macierzy służy do zapewnienia danych dla kolekcji. Ponieważ widok kolekcji nie ma formatu wyświetlania predefiniowanych, więcej pracy jest wymagana, aby przekazać opinię interakcji użytkownika oraz śledzenie wybór użytkownika.

> [!IMPORTANT]
> Ze względu na problem w programie Xcode 7 i macOS 10.11 (lub nowszej) nie może być używany w plikach scenorysu (.storyboard) są widoki kolekcji. W związku z tym należy nadal używać do definiowania widoków kolekcji dla aplikacji Xamarin.Mac .xib plików. Zobacz nasze [widoki kolekcji](~/mac/user-interface/collection-view.md) dokumentacji, aby uzyskać więcej informacji.

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

## <a name="debugging-native-crashes"></a>Debugowanie natywnych awarii (Crash)

Popełnienia błędu w wiązania danych może spowodować _natywnego awarii_ w niezarządzanych kodu i spowodować, że aplikacja Xamarin.Mac zakończy się niepowodzeniem z `SIGABRT` błąd:

[![Przykład okno dialogowe natywnego awarii](databinding-images/debug01.png "przykład okno dialogowe natywnego awarii")](databinding-images/debug01-large.png#lightbox)

Podczas tworzenia powiązań danych zwykle są cztery główne przyczyny awarii native:

1. Modelu danych nie dziedziczy `NSObject` lub podklasa klasy of `NSObject`.
2. Nie uwidacznia równą przy użyciu języka Objective-C `[Export("key-name")]` atrybutu.
3. Nie zawinięty zmiany wartości dostępu w `WillChangeValue` i `DidChangeValue` wywołania metody (Określanie tego samego klucza `Export` atrybutu).
4. Ma nieprawidłową lub błędnie klucz w **powiązanie inspektora** w Konstruktorze interfejsu.

### <a name="decoding-a-crash"></a>Dekodowanie awarii

Załóżmy powoduje natywnego awarii w naszym powiązania danych, dlatego zostanie przedstawiony sposób Znajdź i napraw. W Konstruktorze interfejsu Zmieńmy naszych powiązanie pierwsza etykieta w przykładzie widok kolekcji z `Name` do `Title`:

[![Edytowanie powiązania klucza](databinding-images/debug02.png "edycji klucza powiązania")](databinding-images/debug02-large.png#lightbox)

Teraz zapisać zmiany, przełączyć się do programu Visual Studio for Mac w celu synchronizacji z Xcode i uruchomić aplikację. Gdy zostanie wyświetlony widok kolekcji, aplikacja na chwilę ulegnie awarii z `SIGABRT` błędu (jak pokazano w **danych wyjściowych aplikacji** w programie Visual Studio dla komputerów Mac) od czasu `PersonModel` nie ujawnia właściwości z kluczem `Title`:

[![Przykład błąd wiązania](databinding-images/debug03.png "przykład błąd wiązania")](databinding-images/debug03-large.png#lightbox)

Jeśli firma Microsoft przewiń do góry bardzo błędu w **danych wyjściowych aplikacji** widzimy kluczem do rozwiązywania problemu:

[![Znajdowanie problem w dzienniku błędów](databinding-images/debug04.png "znajdowanie problem w dzienniku błędów")](databinding-images/debug04-large.png#lightbox)

Ten wiersz informuje NAS, że klucz `Title` nie istnieje dla obiektu, który mamy dokonywane jest wiązanie. Jeżeli zmienimy powiązania z powrotem do `Name` interfejsu konstruktora, Zapisz, synchronizacja, ponownie skompilować i uruchomić, aplikacja będzie działać zgodnie z oczekiwaniami bez problemu.

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się praca z powiązania danych i kluczy i wartości kodowania w aplikacji Xamarin.Mac. Najpierw należy go przeglądał udostępnianie klasy C# do języka Objective-C za pomocą kluczy i wartości kodowania (KVC) i klucz wartość obserwowania (KVO). Następnie go pokazano, jak użyć klasy zgodne KVO i danych powiązać go z elementów interfejsu użytkownika w Konstruktorze interfejsu w środowisku Xcode. Na koniec wykazało za pomocą powiązania danych złożonych **kontrolerów macierzy** i **kontrolerów drzewa**.


## <a name="related-links"></a>Linki pokrewne

- [MacDatabinding scenorysu (przykład)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [XIBs MacDatabinding (przykład)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Formanty standardowe](~/mac/user-interface/standard-controls.md)
- [Widoki tabel](~/mac/user-interface/table-view.md)
- [Widoki konspektu](~/mac/user-interface/outline-view.md)
- [Kolekcja widoków](~/mac/user-interface/collection-view.md)
- [Kodowanie przewodnik programowania w języku klucz wartość](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Wprowadzenie do przewodnik programowania w języku obserwowania klucz wartość](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Wprowadzenie do programowania tematy powiązania Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Wprowadzenie do Cocoa powiązania odwołania](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [System macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
