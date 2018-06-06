---
title: Elementarz C# dla programistów języka Objective C
description: W tym dokumencie opisano C# dla programistów języka Objective-C. Porównuje, a zachowanie różni się od dwóch języków, sprawdzając selektorów protokołów i interfejsów, kategorii i metody rozszerzenia, struktur i zestawy, a parametry nazwane i inne.
ms.prod: xamarin
ms.assetid: 00285CBD-AE5E-4126-8F22-6B231B9467EA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 514841bb18ebed72c07377ff95127dff247f0d71
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786216"
---
# <a name="c-primer-for-objective-c-developers"></a>Elementarz C# dla programistów języka Objective C

_Xamarin.iOS umożliwia niezależne od platformy kod napisany w języku C# do udostępnienia platformach. Istniejące aplikacje dla systemu iOS może być jednak wykorzystać kod języka Objective-C, który został już utworzony. W tym artykule służy jako krótki Elementarz dla deweloperów języka Objective-C chce przenieść do platformy Xamarin i języka C#._

iOS i OS X aplikacji utworzonych w języku Objective C mogą korzystać z platformy Xamarin przy użyciu języka C# w miejscach, gdy kod specyficzne dla platformy nie jest wymagane, dzięki czemu takie kod ma być używany na urządzeniach z systemem innym niż Apple. Czynności, takie jak usługi sieci web, JSON i XML algorytmy analizy i niestandardowych można w sposób między platformami.

Aby móc korzystać z platformy Xamarin przy zachowaniu istniejących zasobów języka Objective-C, pierwsza może być narażony na C# technologię z Xamarin znany jako powiązania, które powierzchni Objective C dla kodu zarządzanego, World C#. Ponadto w razie potrzeby kod można przenieść wiersz po wierszu dla C# oraz. Niezależnie od tego podejścia, czy można go powiązanie lub przenoszenie, pewna znajomość języka Objective C i C# jest efektywnie wykorzystać istniejący kod języka Objective-C z platformy Xamarin.iOS.

## <a name="objective-c-interop"></a>Współdziałanie języka Objective C

Obecnie nie istnieje obsługiwana mechanizm do tworzenia biblioteki w języku C# przy użyciu platformy Xamarin.iOS, który można wywołać z Objective-C. Główną przyczyną tego jest Mono środowiska uruchomieniowego jest również wymagany dodatkowo do powiązania. Nadal można jednak utworzyć większość logiki w języku Objective-C, łącznie z interfejsów użytkownika. Aby to zrobić, zawijać kod języka Objective-C w bibliotece, a następnie utwórz powiązanie do niego. Xamarin.iOS jest potrzebne do ładowania początkowego aplikacji (co oznacza, musi utworzyć `Main` punktu wejścia). Po wykonaniu tej innych logiki można w języku Objective-C, ujawniony dla C# za pomocą powiązania (lub przy użyciu elementu P/Invoke). W ten sposób można zachować konkretnej logiki platformy w języku Objective C i opracowanie części o niesprecyzowanym platformy w języku C#.

W tym artykule prezentuje niektóre podobieństwa, a także różnic między kilka różnic w obu mają służyć jako Elementarz podczas przenoszenia na C# z Xamarin.iOS, czy powiązanie z istniejącego kodu języka Objective-C lub przenoszenia go do języka C#.

Szczegółowe informacje na temat tworzenia powiązań można znaleźć inne dokumenty w [powiązanie Objective-C](~/ios/platform/binding-objective-c/index.md).

## <a name="language-comparison"></a>Porównania języka

Objective-C i C# są bardzo różnych języków, składnię i z punktu widzenia środowiska wykonawczego. Języka Objective C jest języka dynamicznego i wykorzystuje schemat przekazywanie komunikatów, podczas gdy C# statycznie został wpisany. Syntax-Wise Objective-C przypomina Smalltalk, podczas gdy C# pochodzi znacznie składnia podstawowych za pomocą języka Java, mimo że dojrzewania on uwzględniony w ostatnich latach wiele innych funkcji poza Java.

Inaczej mówiąc, że nie ma kilka funkcji języka Objective-C i C#, które są podobne w funkcji. Podczas tworzenia powiązania kod języka Objective-C w języku C# lub przenoszenie Objective-C, aby C#, opis tych podobieństwa jest użyteczne.

### <a name="protocols-vs-interfaces"></a>Vs protokołów. Interfejsy

Zarówno Objective-C i C# to pojedyncze dziedziczenie języki. Jednak zarówno języków obsługują wykonania wielu interfejsów w danej klasy. W języku Objective C te interfejsy logiczne są nazywane *protokołów* należy w języku C# są nazywane *interfejsów*. Implementation-Wise podstawowa różnica między interfejs języka C# i protokół języka Objective C jest jego może opcjonalnie metody. Aby uzyskać więcej informacji, zobacz [protokołów, delegaci i zdarzenia](~/ios/app-fundamentals/delegates-protocols-and-events.md) artykułu.

### <a name="categories-vs-extension-methods"></a>Vs kategorii. Metody rozszerzenia

Objective-C umożliwia metody ma zostać dodana do klasy, dla którego nie może mieć implementacji kodu za pomocą *kategorii*. W języku C# jest dostępna za pośrednictwem określane jako podobne koncepcji *metody rozszerzenia*.

Metody rozszerzenia umożliwiają dodawanie metody statyczne do klasy, w którym są podobne do metod klasy w Objective-C. metod statycznych w języku C# Na przykład następujący kod dodaje metodę o nazwie `ScrollToBottom` do `UITextView` klasy, która z kolei jest zarządzanej klasy, który jest powiązany z języka Objective C `UITextView` klasy z UIKit:

```csharp
public static class UITextViewExtensions
{
    public static void ScrollToBottom (this UITextView textView)
    {
        // code to scroll textView
    }
}
```

Następnie, gdy wystąpienie klasy `UITextView` jest tworzony w kodzie metody będą dostępne na liście autouzupełniania, jak pokazano poniżej:

 ![](primer-images/01-extensionmethodintellisense.png "Metoda dostępna w automatycznego uzupełniania")

Po wywołaniu metody rozszerzenia wystąpienie jest przekazany do argumentu, takich jak `textView` w tym przykładzie.

### <a name="frameworks-vs-assemblies"></a>Vs struktury. Zestawy

Pakiety języka Objective-C powiązanymi klasami w katalogach specjalne znany jako struktury. W języku C# i .NET jednak zestawów są używane do zapewnienia wielokrotnego użytku fragmenty kodu prekompilowany. W środowiskach poza iOS zestawy zawiera kod języka pośrednim (IL), który właśnie trwa time (JIT) kompilowane w czasie wykonywania. Jednak firmy Apple nie zezwala JIT w aplikacjach systemu iOS. W związku z tym przeznaczonych dla systemu iOS za pomocą platformy Xamarin kodu C# jest wcześniejsze skompilowany drzewa obiektów (aplikacji), tworzenie pojedynczy plik wykonywalny systemu Unix oraz plików metadanych, które znajdują się w pakiecie aplikacji.

### <a name="selectors-vs-named-parameters"></a>Vs selektorów. Parametry nazwane

Metody języka Objective-C są z założenia nazwy parametru selektorów z natury. Na przykład selektora takich jak `AddCrayon:WithColor:` ułatwia wyczyść każdego parametru znaczenia w przypadku używania w kodzie. C# opcjonalnie obsługuje również argumenty nazwane.

Na przykład będzie podobny kod w języku C# przy użyciu nazwane argumenty:

```csharp
AddCrayon (crayon: myCrayon, color: UIColor.Blue);
```

Mimo że C# dodać tę obsługę w języku, w praktyce, który nie jest bardzo często używane w wersji 4.0. Jeśli jednak chcesz mieć jawnego w kodzie, obsługę jest istnieje.

### <a name="headers-and-namespaces"></a>Nagłówki i przestrzenie nazw

Stanowi nadzbiór C, Objective-C używa nagłówki dla deklaracji publicznych, które są oddzielne od pliku implementacji. C# nie korzystają z plików nagłówka. W przeciwieństwie do języka Objective-C kod w języku C# znajduje się w przestrzeni nazw. Jeśli chcesz dołączyć kod dostępna w niektórych nazw, można dodać przy użyciu dyrektywy na początku pliku implementacji, lub kwalifikuje typ z pełną przestrzeni nazw.

Na przykład poniższy kod zawiera `UIKit` przestrzeni nazw, udostępniając każdej klasy w tej przestrzeni nazw do implementacji:

```csharp
using UIKit
namespace MyAppNamespace
{
    // implementation of classes
}
```

Ponadto słowo kluczowe przestrzeni nazw w powyższym kodzie ustawia przestrzeni nazw używany dla w samym pliku implementacji. Jeśli wiele plików implementacji współużytkować tego samego obszaru nazw, nie jest konieczne uwzględnienie przestrzeni nazw w przy użyciu dyrektywy również, w jakiej jest domyślna.

### <a name="properties"></a>Właściwości

Zarówno Objective-C i C# ma pojęcie właściwości w celu zapewnienia wysokiego poziomu abstrakcji wokół metody dostępu. W języku Objective C @property dyrektywy kompilatora służy do generowania skutecznie metody dostępu. Z kolei C# obsługuje właściwości w języku sam. Właściwość C# można zaimplementować przy użyciu dłużej styl, który uzyskuje dostęp do pole zapasowe lub przy użyciu krótszej, składni właściwości automatyczne, jak pokazano w poniższych przykładach:

```csharp
// automatic property syntax
public string Name { get; set; }

// property implemented with a backing field
string address;
public string Address {
    get {
        // could add additional code here
        return address;
    }
    set {
        address = value;
    }
}
```

### <a name="static-keyword"></a>Static — słowo kluczowe

*Statycznych* — słowo kluczowe ma znaczenie bardzo różnią się od języka Objective C i C#. W języku Objective C statyczne funkcje są używane do ograniczenia zakresu funkcji do bieżącego pliku. W języku C#, jednak zakres jest zachowywana przez *publicznego*, *prywatnej* i *wewnętrzny* słów kluczowych.

Kiedy static — słowo kluczowe jest stosowany do zmiennej w języku Objective-C, zmienna utrzymuje wartość między wywołania funkcji.

C# ma również static — słowo kluczowe. Po zastosowaniu do metody, efektywnie tak samo który `+` modyfikator jest Objective-c. To znaczy tworzy metody klasy. Podobnie gdy jest stosowany do innych elementów składowych, takich jak pola, właściwości i zdarzeń, dobrym tych części typu, które zostały zgłoszone w ciągu, a nie z dowolnego wystąpienia tego typu. Należy również wybrać klasy statycznej, w którym wszystkie metody zdefiniowany w klasie, muszą być statyczne oraz.

### <a name="nsarray-vs-list-initialization"></a>NSArray vs. Inicjalizacja listy.

Objective-C zawiera teraz składni literału do użycia z `NSArray`, co ułatwia zainicjować. C# jednak ma rozbudowane typ o nazwie `List` czyli *ogólnego*, co oznacza typ listy przechowuje może być udostępniane przez kod, który tworzy listę (na przykład szablonów języka C++). Ponadto listy obsługują automatycznie Inicjalizacja składni, jak pokazano poniżej:

```csharp
MyClass object1 = new MyClass ();
MyClass object2 = new MyClass ();
List<MyClass> myList = new List<MyClass>{ object1, object2 };
```

### <a name="blocks-vs-lambda-expressions"></a>Bloki vs. Wyrażenia lambda

Używa języka Objective-C *bloków* utworzyć zamknięcia, gdzie można utworzyć wbudowanej funkcji, który może zgłaszać przy użyciu stanu, gdy jest zamknięta. C# ma podobne koncepcji przy użyciu wyrażenia lambda. W języku C# lambda wyrażenia są tworzone za pomocą `=>` operatora, jak pokazano poniżej:

```csharp
(args) => {
    //  implementation code
};
```

Aby uzyskać więcej informacji dotyczących wyrażeń lambda, zobacz firmy Microsoft [C# przewodnik programowania w języku](http://msdn.microsoft.com/library/vstudio/bb397687.aspx).

## <a name="summary"></a>Podsumowanie

W tym artykule różne funkcje języka zostały odróżniające między Objective-C i C#. W niektórych przypadkach mu się podobne funkcje, które istnieją między obu języków, takich jak bloków do wyrażeń lambda i kategorie, aby metody rozszerzenia. Ponadto go odróżniające miejsca, gdzie językach odchodzenia, takie jak w przypadku przestrzeni nazw w języku C# oraz na znaczenie właściwości static — słowo kluczowe.
