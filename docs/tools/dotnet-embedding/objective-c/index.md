---
title: "Obsługa języka Objective C"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: f6d19f0f6573b17dfb3feb6bf131686413d4e68f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="objective-c-support"></a>Obsługa języka Objective C

## <a name="specific-features"></a>Wyszczególnienie funkcji

Generowanie ObjC ma kilka, specjalne funkcje, które są warto zauważyć.

### <a name="automatic-reference-counting"></a>Automatyczne liczenie odwołań

Korzystanie z automatycznego odwołanie zliczania (ARC) jest **wymagane** do wywołania wygenerowanego powiązania. Przy użyciu biblioteki embeddinator na podstawie projektu musi być kompilowana przy użyciu `-fobjc-arc`.

### <a name="nsstring-support"></a>Obsługa NSString

Interfejsy API, który ujawnia `System.String` typy są konwertowane na `NSString`. Ułatwia to zarządzanie pamięcią niż zajmowanie `char*`.

### <a name="protocols-support"></a>Obsługa protokołów

Zarządzane interfejsy są konwertowane na protokołów ObjC, gdzie są wszystkie elementy członkowskie `@required`.

### <a name="nsobject-protocol-support"></a>Obsługa protokołu NSObject

Domyślnie przyjęto założenie, domyślne wyznaczania wartości skrótu i równości .net i środowiska uruchomieniowego ObjC są poprawnie i wymienne, ponieważ mają bardzo podobne semantyki.

Gdy zastępuje typu zarządzanego `Equals(Object)` lub `GetHashCode` oznacza to zwykle najlepszy nie jest zachowanie defaut (.NET). Przyjęto założenie, że domyślne zachowanie języka Objective-C albo nie jest.

W takim przypadku przesłania generatora [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) — metoda i [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) właściwość zdefiniowaną w [ `NSObject` protokołu](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). Dzięki temu niestandardowych zarządzaną implementację do użycia w ObjC kod przezroczysty.

### <a name="comparison"></a>Porównanie

Typy, które implementują zarządzane `IComparable` lub jest ogólny wersja `IComparable<T>` utworzy ObjC przyjazną metody, które zwraca `NSComparisonResult` i zaakceptuj `nil` argumentu. Dzięki temu API wygenerowany bardziej przyjazny dla deweloperów ObjC, np.

```csharp
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Kategorie

Zarządzane rozszerzenia, które metody są konwertowane na kategorie. Na przykład następujące metody rozszerzenia na `Collection`:

```csharp
    public static class SomeExtensions {

        public static int CountNonNull (this Collection collection) { ... }

        public static int CountNull (this Collection collection) { ... }
    }
```

utworzyć kategorii Objective-C, podobny do tego:

```csharp
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;
@end
```

Gdy zarządzany jeden typ rozszerza kilka typów generowane są liczne kategorie Objective-C, a następnie.

### <a name="subscripting"></a>Tworzenie indeksów dolnych

Właściwości indeksowane zarządzane są konwertowane na tworzenie indeksów dolnych obiektu. Na przykład:

```csharp
    public bool this[int index] {
        get { return c[index]; }
        set { c[index] = value; }
    }
```

Czy tworzyć podobne do języka Objective-C:

```csharp
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

które mogą służyć za pomocą składni subscripting Objective-C:

```csharp
    if ([intCollection [0] isEqual:@42])
        intCollection[0] = @13;
```

W zależności od typu indeksator zostanie wygenerowany indeksowanych lub kluczem tworzenie indeksów dolnych odpowiednim.

To [artykułu](http://nshipster.com/object-subscripting/) jest doskonałym wprowadzenie do tworzenie indeksów dolnych.

## <a name="main-differences-with-net"></a>Główne różnice z platformą .NET

### <a name="constructors-vs-initializers"></a>Konstruktory vs inicjatory

W języku Objective-C można wywołać żadnego inicjatora prototypów klas nadrzędnych w łańcuch dziedziczenia, chyba że jest on oznaczony jako niedostępny (NS_UNAVAILABLE).

W języku C# Musisz jawnie zadeklarować członka Konstruktor w klasie, oznacza to, że nie są dziedziczone przez konstruktorów.

Do udostępnienia prawo reprezentację API języka C# do języka Objective-C, dodamy `NS_UNAVAILABLE` do dowolnego inicjatora, którego nie ma klasy podrzędnej z klasy nadrzędnej.

C# INTERFEJSU API:

```csharp
public class Unique {
    public Unique () : this (1)
    {
    }

    public Unique (int id)
    {
    }
}

public class SuperUnique : Unique {
    public SuperUnique () : base (911)
    {
    }
}
```

Objective-C udostępniane interfejsu API:

```objectivec
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

W tym miejscu zobaczysz, że `initWithId:` został oznaczony jako niedostępny.

### <a name="operator"></a>Operator

ObjC nie obsługuje operatora przeładowanie jak C#, więc operatory są konwertowane na selektory klasy:

```csharp
    public static AllOperators operator + (AllOperators c1, AllOperators c2)
    {
        return new AllOperators (c1.Value + c2.Value);
    }
```

na

```csharp
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

Jednak w przypadku niektórych języków .NET nie obsługują przeładowanie, operatora tak często, aby obejmować ["przyjaznym"](https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx) o nazwie metody oprócz przeciążenia operatora.

Jeśli zarówno wersji operatora, jak i wersji "przyjaznym" zostaną znalezione, zostanie wygenerowany tylko przyjazna dla użytkownika wersja, będzie generować w do tej samej nazwie objective-c.

```csharp
    public static AllOperatorsWithFriendly operator + (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
    {
        return new AllOperatorsWithFriendly (c1.Value + c2.Value);
    }

    public static AllOperatorsWithFriendly Add (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
    {
        return new AllOperatorsWithFriendly (c1.Value + c2.Value);
    }
```

Staje się:

```csharp
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Operator równości

W operatorze ogólne == w C# jest obsługiwany jako ogólne operatora jakie zostały zanotowane powyżej.

Jednak jeśli operator równa się "przyjaznym" zostanie znaleziony, zarówno operator == i operator! = zostanie pominięte podczas generowania.

### <a name="datetime-vs-nsdate"></a>Data i godzina vs NSDate

Z [w NSDate](https://developer.apple.com/reference/foundation/nsdate?language=objc) dokumentacji:

> Obiekty NSDate hermetyzować pojedynczy punkt w czasie, niezależnie od wszelkich określonym systemie calendrical lub strefy czasowej. Data obiekty są niezmienne, reprezentujący interwał niezmiennej względem Data bezwzględna odwołania (00: 00:00 UTC w dniu 1 stycznia 2001).

Ze względu na `NSDate` Data, wszystkie konwersje między on odwołania i `DateTime` musi zostać wykonane w formacie UTC.

#### <a name="datetime-to-nsdate"></a>Data i godzina do NSDate

Podczas konwertowania z `DateTime` do `NSDate` element DateTime `Kind` właściwości jest brana pod uwagę.

<table>
<tr><th> rodzaj         </th><th> Wyniki                                                                                            </th></tr>
<!--tr><td> ------------ </td><td> -------------------------------------------------------------------------------------------------- </td></tr-->
<tr><td> Czas UTC          </td><td> Konwersja odbywa się przy użyciu podany obiekt DateTime, jak to.                                  </td></tr>
<tr><td> Lokalny        </td><td> Wyniku wywołania metody `ToUniversalTime ()` w DateTime podany obiekt jest używany na potrzeby konwersji. </td></tr>
<tr><td> Nieokreślony  </td><td> Podany obiekt DateTime zakłada, że UTC, tak samo jak rodzaj == Utc.                </td></tr>
</table>

Konwersja odbywa się przy użyciu następującej formuły:

> [!NOTE]
> **Parametru TimeInterval** = DateTimeObjectTicks - NSDateReferenceDateTicks [dt] / [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx)

Gdy będziemy mieć parametru TimeInterval używamy jego NSDate [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) selektor, aby go utworzyć.

#### <a name="nsdate-to-datetime"></a>NSDate DateTime

Przejście od NSDate DateTime przyjęto założenie, że firma Microsoft uzyskują NSDate wystąpienia, który jest Data odwołania jest **UTC 00:00:00 w dniu 1 stycznia 2001** i za pomocą następującej formuły:

> [!NOTE]
> **DateTimeTicks** = NSDateTimeIntervalSinceReferenceDate * [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx) + NSDateReferenceDateTicks [dt]

Po możemy obliczana **DateTimeTicks** używamy następujące DateTime [Konstruktor](https://msdn.microsoft.com/en-us/library/w0d47c9c(v=vs.110).aspx) ustawienie jej `kind` do `DateTimeKind.Utc`.

Istnieją pewne kwestie, które należy zwrócić uwagę, może być NSDate `nil` , ale wartości daty i godziny jest strukturą w .NET i definicji nie może być `null`. Jeśli zostanie nadana `nil` NSDate firma Microsoft będzie przekształca ją domyślną wartość daty/godziny, który mapuje na `DateTime.MinValue`.

Wartość MinValue i MaxValue różnią się również, NSDate może obsługiwać większą i dolnych granic niż data i godzina na tak zawsze, gdy należy nadać wartość większą lub niższy firma Microsoft będzie wartości daty i godziny w [MaxValue](https://msdn.microsoft.com/en-us/library/system.datetime.maxvalue(v=vs.110).aspx) lub [MinValue](https://msdn.microsoft.com/en-us/library/system.datetime.minvalue(v=vs.110).aspx) odpowiednio.

**DT**: Data odwołania NSDate **UTC 00:00:00 w dniu 1 stycznia 2001** => `new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;`
