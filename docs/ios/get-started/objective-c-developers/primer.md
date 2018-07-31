---
title: Elementarz języka C# dla deweloperów języka Objective-C
description: W tym dokumencie opisano C# dla deweloperów języka Objective-C. On porównano i zestawiono ze sobą dwa języki, sprawdzając selektory protokołów i interfejsów, kategorie i metody rozszerzenia, struktur i zestawów i nazwanych parametrów i nie tylko.
ms.prod: xamarin
ms.assetid: 00285CBD-AE5E-4126-8F22-6B231B9467EA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 7fbc37a561b0a1c0b0d5a16fea2892e7faf1a86b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351512"
---
# <a name="c-primer-for-objective-c-developers"></a>Elementarz języka C# dla deweloperów języka Objective-C

_Xamarin.iOS umożliwia niezależne od platformy kod napisany w języku C#, aby być współdzielone na różnych platformach. Jednak istniejące aplikacje dla systemu iOS mogą chcieć wykorzystać kod języka Objective-C, który został już utworzony. Ten artykuł służy jako krótka podstawowe informacje dla deweloperów języka Objective-C, które chcesz przenieść do platformy Xamarin i języka C#._

dla systemów iOS i OS X aplikacje opracowane w języku Objective-C, mogą korzystać z platformy Xamarin przy użyciu języka C# w miejscach, w którym kodu specyficznego dla platformy nie jest wymagane, umożliwiając takiego kodu, ma być używany na urządzeniach innych niż firmy Apple. Elementy, takie jak usługi sieci web, JSON i XML algorytmów analizy i niestandardowych mogą posłużyć w sposób dla wielu platform.

Aby móc korzystać z platformy Xamarin przy zachowaniu istniejących zasobów języka Objective-C, pierwsza mogą łączyć się z C# w technologii z platformy Xamarin, znane jako powiązania, które powierzchni kod języka Objective-C do zarządzanego, world języka C#. Ponadto jeśli to konieczne, kod można przenieść wiersz po wierszu do języka C# oraz. Niezależnie od tego podejścia jednak być powiązanie, lub przenoszenia należy pewną wiedzę na temat języka Objective-C i C# jest skutecznie korzystać z istniejącego kodu języka Objective-C za pomocą rozszerzenia Xamarin.iOS.

## <a name="objective-c-interop"></a>Współdziałanie języka Objective-C

Nie istnieje obecnie obsługiwane mechanizm do tworzenia biblioteki w języku C# za pomocą rozszerzenia Xamarin.iOS, które mogą być wywoływane z Objective-C. Głównym powodem tego jest środowiska uruchomieniowego Mono jest również wymagane oprócz powiązania. Nadal można jednak utworzyć większość logiki w języku Objective-C, w tym interfejsów użytkownika. Aby to zrobić, zawijania kodu języka Objective-C w bibliotece, a następnie utworzyć powiązania do niego. Xamarin.iOS jest wymagane do uruchamiania aplikacji (co oznacza, musi utworzyć `Main` punktu wejścia). Po tym wszelka logika innych może być w języku Objective-C, uwidaczniany w języku C#, za pośrednictwem powiązania (lub za pośrednictwem metody P/Invoke). W ten sposób można zapewnić logikę specyficzną dla platformy w języku Objective-C i tworzenie części niezależny od platformy w języku C#.

Ten artykuł prezentuje pewne podobieństwa klucza, a także zestawiono ze sobą kilka różnic w obu językach, która będzie służyć jako podstawowe informacje przy przechodzeniu do języka C# z rozszerzeniem Xamarin.iOS, czy powiązanie z istniejącego kodu języka Objective-C lub przenoszone do języka C#.

Aby uzyskać szczegółowe informacje na temat tworzenia powiązań, zobacz dokumenty w [powiązań języka Objective-C](~/ios/platform/binding-objective-c/index.md).

## <a name="language-comparison"></a>Porównanie języka

Objective-C i C# są bardzo różnych języków, składniowo i z punktu widzenia środowiska uruchomieniowego. Objective-C jest języka dynamicznego i wykorzystuje schemat przekazywania komunikatów, natomiast C# statycznie został wpisany. Syntax-Wise Objective-C przypomina Smalltalk, natomiast C# pochodzi wiele jego składni podstawowych w języku Java, mimo że dojrzewania się zawarte dużo większe możliwości niż języka Java w ostatnich latach.

Inaczej mówiąc, że nie ma kilka funkcji języka Objective-C i C#, które są podobne w funkcji. Podczas tworzenia wiązania do kodu języka Objective-C za pomocą języka C# lub portowaniu Objective-C, aby C#, informacje o tych podobieństwa jest przydatna.

### <a name="protocols-vs-interfaces"></a>Protokoły programu vs. Interfejsy

Zarówno języka Objective-C i C# to języki pojedyncze dziedziczenie. Jednakże oba języki posiadają Obsługa implementowania wielu interfejsów w danej klasy. W języku Objective-C interfejsy logiczne, te są nazywane *protokołów* natomiast w języku C# są nazywane *interfejsów*. Implementation-Wise główną różnicą między interfejs C# i protokołu języka Objective-C jest jego może mieć opcjonalny metody. Aby uzyskać więcej informacji, zobacz [protokołów, Delegaty i zdarzenia](~/ios/app-fundamentals/delegates-protocols-and-events.md) artykułu.

### <a name="categories-vs-extension-methods"></a>Kategorie programu vs. Metody rozszerzenia

Języka Objective-C umożliwia metod, które mają zostać dodane do klasy, dla której nie masz do wykonania kodu przy użyciu *kategorie*. W języku C# podobne koncepcja jest dostępna za pośrednictwem co to jest znany jako *metody rozszerzenia*.

Metody rozszerzenia umożliwiające dodawanie metod statycznych dla danej klasy, której metody statyczne w języku C# są analogiczne do metody klasy w Objective-C. Na przykład, poniższy kod dodaje metodę o nazwie `ScrollToBottom` do `UITextView` klasy, która z kolei jest zarządzany klasę, która jest powiązana z języka Objective-C `UITextView` klasy z UIKit:

```csharp
public static class UITextViewExtensions
{
    public static void ScrollToBottom (this UITextView textView)
    {
        // code to scroll textView
    }
}
```

Następnie, gdy wystąpienie klasy `UITextView` jest tworzony w kodzie, Metoda ta będzie dostępna na liście autouzupełniania, jak pokazano poniżej:

 ![](primer-images/01-extensionmethodintellisense.png "Metody, które są dostępne w funkcji autouzupełniania")

Gdy metoda rozszerzająca zostanie wywołana wystąpienie jest przekazywany do argumentu, takich jak `textView` w tym przykładzie.

### <a name="frameworks-vs-assemblies"></a>Struktury programu vs. Zestawy

Pakiety języka Objective-C powiązanymi klasami w katalogach specjalne, znane jako struktury. W języku C# i .NET, zestawy służą do zapewniania wielokrotnego użytku bity wstępnie skompilowany kod. W środowiskach poza dla systemu iOS zestawy zawierają kod języka pośredniego (IL), który jest dokładnie na czas (JIT) są kompilowane w czasie wykonywania. Jednak firmy Apple nie zezwala na JIT w aplikacjach systemu iOS. W związku z tym kod C#, przeznaczonych dla systemu iOS za pomocą platformy Xamarin jest wcześniej skompilowany (AOT), tworzenie jednego pliku wykonywalnego systemu Unix wraz z plikami metadanych, które znajdują się w pakiecie aplikacji.

### <a name="selectors-vs-named-parameters"></a>Selektory programu vs. Nazwane parametry

Metody języka Objective-C natury obejmują nazwy parametrów w selektorach z natury. Na przykład selektora takich jak `AddCrayon:WithColor:` wyraźnie co każdy parametr oznacza, że gdy jest używana w kodzie. C# obsługuje opcjonalnie również argumenty nazwane.

Na przykład będzie podobny kod w języku C# przy użyciu argumenty nazwane:

```csharp
AddCrayon (crayon: myCrayon, color: UIColor.Blue);
```

Mimo że C# w wersji 4.0 języka, w praktyce, który nie jest bardzo często używane dodano tej obsługi. Jeśli chcesz była niejawna w kodzie, pomocy technicznej dla niej jest jednak istnieje.

### <a name="headers-and-namespaces"></a>Nagłówki i przestrzenie nazw

Jest nadzbiorem języka C, Objective-C używa nagłówki dla publicznego deklaracje, które są niezależne od pliku implementacji. C# nie są używane pliki nagłówkowe. W przeciwieństwie do języka Objective-C kod w języku C# znajduje się w przestrzeni nazw. Jeśli chcesz dołączyć kod jest dostępny w niektórych przestrzeni nazw, należy dodać przy użyciu dyrektywy na początku pliku implementacji lub kwalifikuje typ z pełną przestrzeni nazw.

Na przykład, poniższy kod zawiera `UIKit` przestrzeni nazw, udostępniając każda klasa w tej przestrzeni nazw do implementacji:

```csharp
using UIKit
namespace MyAppNamespace
{
    // implementation of classes
}
```

Ponadto słowo kluczowe przestrzeni nazw w powyższym kodzie ustawia przestrzeń nazw używaną w poszukiwaniu pliku implementacji, sam. Wiele plików implementacji udostępnianie tej samej przestrzeni nazw, czy nie trzeba wpisywać przestrzeni nazw w za pomocą dyrektywy, ponieważ jest domyślna.

### <a name="properties"></a>Właściwości

Zarówno języka Objective-C i C# mają koncepcji właściwości w celu zapewnienia wysokiego poziomu abstrakcji wokół metody dostępu. W języku Objective-C @property dyrektywy kompilatora służy do efektywnego generowania metody dostępu. Z kolei języka C# obsługuje właściwości w ramach sam język. Właściwość języka C# można zaimplementować przy użyciu dłuższego styl, który uzyskuje dostęp do pola pomocniczego lub przy użyciu krótszy, składnia właściwości automatycznych, jak pokazano w poniższych przykładach:

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

*Statyczne* — słowo kluczowe ma bardzo różne znaczenie między Objective-C i C#. W języku Objective-C statyczne funkcje są używane do ograniczenia zakresu funkcji w bieżącym pliku. W języku C#, jednak zakresu jest obsługiwane za pomocą *publicznych*, *prywatnej* i *wewnętrzny* słów kluczowych.

Podczas static — słowo kluczowe jest zastosowany do zmiennej w języku Objective-C, zmienna zawiera wartość rozmieszczonych w wywołania funkcji.

C# zawiera również static — słowo kluczowe. Po zastosowaniu do metody, jego skutecznie działa tak samo, `+` modyfikator jest Objective-c. To znaczy tworzy metody klasy. Podobnie gdy jest stosowany do innych konstrukcji, takich jak pola, właściwości i zdarzenia, zapewnia tych części typu, które zostały zgłoszone w ciągu, a nie z dowolnego wystąpienia tego typu. Można również ustawić klasy statycznej, w którym wszystkie metody, które są zdefiniowane w klasie musi być statyczna także.

### <a name="nsarray-vs-list-initialization"></a>NSArray programu vs. Inicjalizacja listy.

Języka Objective-C obejmuje teraz literału składni do użycia z usługą `NSArray`, co ułatwia inicjowanie. C# jednak ma bardziej rozbudowane typie o nazwie `List` czyli *ogólny*, co oznacza typ listy przechowuje mogą być zapewniane przez kod, który tworzy listę (na przykład szablonów języka C++). Ponadto listy obsługują składni inicjowania automatycznego, jak pokazano poniżej:

```csharp
MyClass object1 = new MyClass ();
MyClass object2 = new MyClass ();
List<MyClass> myList = new List<MyClass>{ object1, object2 };
```

### <a name="blocks-vs-lambda-expressions"></a>Program vs blokuje. Wyrażenia lambda

Używa języka Objective-C *bloki* Aby utworzyć zamknięć, umożliwiającego utworzenie wbudowanych funkcji, który może zgłaszać Użyj stanu, gdy jest zamknięta. C# ma podobne koncepcji przy użyciu wyrażenia lambda. W języku C# lambda wyrażenia są tworzone za pomocą `=>` operatora, jak pokazano poniżej:

```csharp
(args) => {
    //  implementation code
};
```

Aby uzyskać więcej informacji na temat wyrażeń lambda, zobacz firmy Microsoft [przewodnik programowania w języku C#](http://msdn.microsoft.com/library/vstudio/bb397687.aspx).

## <a name="summary"></a>Podsumowanie

W tym artykule różne funkcje języka były przedstawiane jako przeciwieństwo między Objective-C i C#. W niektórych przypadkach go wywoływane funkcje analogiczne między obu języków, takich jak bloki wyrażenia lambda i kategorie w celu metody rozszerzenia. Ponadto porównanie miejsca, w których języki rozróżnić, takie podobnie jak w przypadku przestrzeni nazw w C# i static — słowo kluczowe znaczenie.
