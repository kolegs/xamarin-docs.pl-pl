---
title: Osadzanie .NET najlepsze rozwiązania dla języka Objective C
description: W tym dokumencie opisano różne najlepsze rozwiązania dotyczące przy użyciu osadzanie .NET z Objective-C. Omówiono jej udostępnianie podzbiór kodu zarządzanego, udostępnianie chunkier interfejsu API, nazw i inne.
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: b4b0df6f1c7c1d5931c0c18a1508747a7c570bea
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793499"
---
# <a name="net-embedding-best-practices-for-objective-c"></a>Osadzanie .NET najlepsze rozwiązania dla języka Objective C

Jest to projekt i może nie być synchronizacji przy użyciu funkcji obecnie obsługiwany przez narzędzie. Mamy nadzieję, że ten dokument zostanie rozwijać oddzielnie i ostatecznie odpowiada końcowego narzędzia, tj. będzie Sugerujemy długoterminowej najlepszych metod — nie natychmiastowego rozwiązania.

Duża część tego dokumentu dotyczy również innych języków. Jednak wszystkie podane przykłady znajdują się w języku C# i Objective-C.

## <a name="exposing-a-subset-of-the-managed-code"></a>Udostępnianie podzbiór kodu zarządzanego

Wygenerowany natywnej biblioteki/framework zawiera kod języka Objective-C do wywołania wszystkich zarządzanych interfejsów API, która jest widoczna. Więcej możesz powierzchni interfejsu API (upublicznij) następnie większych natywnego _sklejki_ staną się biblioteki.

Może być warto utworzyć inną, mniejszym zestawu do udostępnienia tylko wymaganych interfejsów API do natywnej deweloperów. Tego fasad będzie można również większą kontrolę nad widoczność nazewnictwa błąd podczas sprawdzania... wygenerowanego kodu.

## <a name="exposing-a-chunkier-api"></a>Udostępnianie chunkier interfejsu API

Brak cen zapłacenia przejścia z natywnego do zarządzanego (i wstecz). Tak, warto udostępnić _ociężale zamiast chatty_ interfejsów API deweloperom macierzystego, np.

**Chatty**

```csharp
public class Person {
  public string FirstName { get; set; }
  public string LastName { get; set; }
}
```

```objc
// this requires 3 calls / transitions to initialize the instance
Person *p = [[Person alloc] init];
p.firstName = @"Sebastien";
p.lastName = @"Pouliot";
```

**Chunky**

```csharp
public class Person {
  public Person (string firstName, string lastName) {}
}
```

```objc
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

Ponieważ liczby przejść jest mniejszy wydajność będzie lepiej. Mniejsza ilość kodu ma zostać wygenerowane, wymagany jest również tak spowoduje także mniejszych natywnej biblioteki.

## <a name="naming"></a>Nadawanie nazw

Nazw elementów jest jeden z dwóch najtrudniejsze problemów w nauce komputera innych trwa pamięci podręcznej błędy unieważnianie i wyłączone na 1. Miejmy nadzieję, że osadzanie .NET można włączyć osłony można z wyjątkiem nazewnictwa.

### <a name="types"></a>Types

Objective-C nie obsługuje przestrzeni nazw. Ogólnie rzecz biorąc, jego typy są poprzedzane prefiksem 2 (dla Apple) lub 3 (dla strony 3) znaków prefiksu, tak samo, jak `UIView` UIKit w widoku, który określa platformę.

Dla typów .NET pomijanie przestrzeń nazw nie jest możliwe mogą stać się zduplikowany lub mylące, nazwy. Dzięki temu istniejących typów .NET bardzo długi, np.

```csharp
namespace Xamarin.Xml.Configuration {
  public class Reader {}
}
```

Czy można używać jak:

```objc
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

Jednak ponownie może ujawnić typu jako:

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

Co więcej przyjazny dla języka Objective-C do użycia, np.:

```objc
id reader = [[XAMXmlConfigReader alloc] init];
```

### <a name="methods"></a>Metody

Nawet dobre nazw .NET może nie być idealna dla interfejsu API języka Objective-C.

Konwencje nazewnictwa w języku Objective C są inne niż .NET (camelcase zamiast przypadku pascal pełniejsze).
Przeczytaj [kodowania wytyczne dotyczące Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF).

Z programisty Objective-C punktu widzenia metodę o `Get` prefiks oznacza nie ma wystąpienia, tj. [Pobierz regułę](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1).

Ta reguła nazewnictwa jest niezgodna w świecie .NET GC; Metoda .NET z `Create` prefiks będą zachowywać się tak samo w .NET. Jednak dla deweloperów języka Objective-C, zwykle oznacza to właścicielem zwrócone wystąpienie, tj. [utworzyć regułę](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029).

## <a name="exceptions"></a>Wyjątki

Jest bardzo często w .NET używać wyjątków często może raportować błędy. Są to jednak powolne i jeszcze nie są identyczne w Objective-C. W miarę możliwości należy je ukryć od dewelopera Objective-C.

Na przykład, .NET `Try` wzorzec będzie znacznie łatwiejsze do korzystania z kodu języka Objective-C:

```csharp
public int Parse (string number)
{
  return Int32.Parse (number);
}
```

a

```csharp
public bool TryParse (string number, out int value)
{
  return Int32.TryParse (number, out value);
}
```

### <a name="exceptions-inside-init"></a>Wyjątki wewnątrz `init*`

W środowisku .NET konstruktora musi albo powiedzie się i zwracać (_miejmy nadzieję, że_) prawidłowe wystąpienie lub Zgłoś wyjątek.

Z kolei umożliwia Objective-C `init*` do zwrócenia `nil` gdy nie można utworzyć wystąpienia. Jest to wzorzec wspólne, ale nie ogólne, używany w wielu platform firmy Apple. W innych przypadkach `assert` może się tak zdarzyć (i kill bieżący proces).

Generator wykonaj takie same `return nil` wzorca dla wygenerowany `init*` metody. Zarządzany wyjątek, a następnie są drukowane (przy użyciu `NSLog`) i `nil` zostanie zwrócony do obiektu wywołującego.

## <a name="operators"></a>Operatory

Objective-C nie zezwala na operatory przeciążony jak C#, dlatego te są konwertowane na selektory klasy.

["Przyjaznym"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) nazwane metody są generowane, zamiast przeciążenia operatora podczas znaleziono i może utworzyć łatwiej korzystać z interfejsu API.

Klasy, które zastępują operatory `==` woluminu `!=` powinny zastępować oraz standardowe metody Equals (obiekt).
