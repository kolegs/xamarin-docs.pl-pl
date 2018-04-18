---
title: Obsługa języka Objective C
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 515185ca7be8b6e24c92c9f44eb6dadbaf6d9219
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="objective-c-support"></a>Obsługa języka Objective C

## <a name="specific-features"></a>Wyszczególnienie funkcji

Generowanie języka Objective-C ma kilka specjalne funkcje, które są warto zauważyć.

### <a name="automatic-reference-counting"></a>Automatyczne liczenie odwołań

Korzystanie z automatycznego odwołanie zliczania (ARC) jest **wymagane** do wywołania wygenerowanego powiązania. Przy użyciu biblioteki embeddinator na podstawie projektu musi być kompilowana przy użyciu `-fobjc-arc`.

### <a name="nsstring-support"></a>Obsługa NSString

Interfejsy API, który ujawnia `System.String` typy są konwertowane na `NSString`. Ułatwia to zarządzanie pamięcią niż podczas pracy nad `char*`.

### <a name="protocols-support"></a>Obsługa protokołów

Zarządzane interfejsy są konwertowane na protokoły Objective-C, gdzie są wszystkie elementy członkowskie `@required`.

### <a name="nsobject-protocol-support"></a>Obsługa protokołu NSObject

Domyślnie domyślnego tworzenia skrótów i równości .NET i środowiska uruchomieniowego języka Objective-C są rozpatrywane wymienne, jak mają podobne semantyki.

Gdy zastępuje typu zarządzanego `Equals(Object)` lub `GetHashCode`, zwykle oznacza to, że zachowanie domyślne (.NET) nie jest wystarczające; oznacza to, że domyślne zachowanie języka Objective C jest prawdopodobnie nie są wystarczające albo.

W takich przypadkach zastępuje generatora [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) — metoda i [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) właściwość zdefiniowaną w [ `NSObject` protokołu](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). Dzięki temu niestandardowych zarządzaną implementację do użycia z kodu języka Objective-C przezroczysty.

### <a name="exceptions-support"></a>Obsługa wyjątków

Przekazywanie `--nativeexception` jako argument `objcgen` przekonwertuje zarządzanych wyjątków do języka Objective-C wyjątki, które mogą być przechwycony i przetwarzane. 

### <a name="comparison"></a>Porównanie

Typy, które implementują zarządzane `IComparable` (lub jego wersja ogólnego `IComparable<T>`) powodują metody przyjazną Objective-C, które zwracają `NSComparisonResult` i zaakceptuj `nil` argumentu. Dzięki temu można bardziej przyjazny dla deweloperów języka Objective-C wygenerowanego interfejsu API. Na przykład:

```objc
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

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

Wiele kategorii Objective-C są generowane, gdy jeden typ zarządzany rozszerza kilka typów.

### <a name="subscripting"></a>Tworzenie indeksów dolnych

Właściwości indeksowane zarządzane są konwertowane na tworzenie indeksów dolnych obiektu. Na przykład:

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

Czy tworzyć podobne do języka Objective-C:

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

które mogą służyć za pomocą składni subscripting Objective-C:

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

W zależności od typu indeksator zostanie wygenerowany indeksowanych lub kluczem tworzenie indeksów dolnych odpowiednim.

To [artykułu](http://nshipster.com/object-subscripting/) jest doskonałym wprowadzenie do tworzenie indeksów dolnych.

## <a name="main-differences-with-net"></a>Główne różnice z platformą .NET

### <a name="constructors-vs-initializers"></a>Inicjatory vs konstruktorów

W języku Objective-C, można wywołać żadnego inicjatora prototypów klas nadrzędnych w łańcuch dziedziczenia, o ile nie jest oznaczony jako niedostępny (`NS_UNAVAILABLE`).

W języku C# Musisz jawnie zadeklarować członka Konstruktor w klasie, co oznacza, że nie są dziedziczone przez konstruktorów.

Do udostępnienia prawo reprezentację API języka C# do języka Objective-C, `NS_UNAVAILABLE` jest dodawany do dowolnego inicjatora, którego nie ma klasy podrzędnej z klasy nadrzędnej.

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

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

W tym miejscu `initWithId:` został oznaczony jako niedostępny.

### <a name="operator"></a>Operator

Objective-C nie obsługuje operatora przeładowanie jak C#, więc operatory są konwertowane na selektory klasy:

```csharp
public static AllOperators operator + (AllOperators c1, AllOperators c2)
{
    return new AllOperators (c1.Value + c2.Value);
}
```

na

```objc
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

Jednak w przypadku niektórych języków .NET nie obsługują przeładowanie, operatora tak często, aby obejmować ["przyjaznym"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) o nazwie metody oprócz przeciążenia operatora.

Jeśli zarówno wersji operatora, jak i wersji "przyjaznym" zostaną znalezione, zostanie wygenerowany tylko przyjazna dla użytkownika wersja, będzie generować w do tej samej nazwie Objective-C.

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

```objc
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Operator równości

W operatorze ogólne `==` w języku C# jest traktowane jako ogólną operator wspomnianego powyżej.

Jednak jeśli operator równa się "przyjaznym" zostanie znaleziony, zarówno operator `==` i operatora `!=` zostaną pominięte podczas generowania.

### <a name="datetime-vs-nsdate"></a>Data i godzina vs NSDate

Z [ `NSDate` ](https://developer.apple.com/reference/foundation/nsdate?language=objc) dokumentacji:

> `NSDate` obiekty hermetyzować pojedynczy punkt w czasie, niezależnie od wszelkich określonym systemie calendrical lub strefy czasowej. Data obiekty są niezmienne, reprezentujący interwał niezmiennej względem Data bezwzględna odwołania (00: 00:00 UTC w dniu 1 stycznia 2001).

Ze względu na `NSDate` Data, wszystkie konwersje między on odwołania i `DateTime` musi zostać wykonane w formacie UTC.

#### <a name="datetime-to-nsdate"></a>Data i godzina do NSDate

Podczas konwertowania z `DateTime` do `NSDate`, `Kind` właściwość `DateTime` jest brana pod uwagę:

|rodzaj|Wyniki|
|---|---|
|`Utc`|Konwersja odbywa się przy użyciu dostarczonego `DateTime` obiektu, ponieważ jest.|
|`Local`|Wyniku wywołania metody `ToUniversalTime()` w określonych `DateTime` obiekt jest używany na potrzeby konwersji.|
|`Unspecified`|Podana `DateTime` obiektu zakłada, że UTC, tak samo podczas `Kind` jest `Utc`.|

Konwersja używa następującej formuły:

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

Gdzie: 

- `NSDateReferenceDateTicks` jest obliczany na podstawie `NSDate` Odwołanie daty UTC 00:00:00 w dniu 1 stycznia 2001: 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) został zdefiniowany w [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Aby utworzyć `NSDate` obiektu `TimeInterval` jest używany z `NSDate` [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) selektora.

#### <a name="nsdate-to-datetime"></a>NSDate DateTime

Konwersja z `NSDate` do `DateTime` używa następującej formuły:

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

Gdzie: 

- `NSDateReferenceDateTicks` jest obliczany na podstawie `NSDate` Odwołanie daty UTC 00:00:00 w dniu 1 stycznia 2001: 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) został zdefiniowany w [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Po obliczanie `DateTimeTicks`, `DateTime` [Konstruktor](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_) zostanie wywołany, ustawienie jej `kind` do `DateTimeKind.Utc`.

> [!NOTE]
> `NSDate` może być `nil`, ale `DateTime` jest strukturą w .NET, który zgodnie z definicją nie może być `null`. Jeśli zostanie nadana `nil` `NSDate`, będzie można przetłumaczyć domyślne `DateTime` wartość, która mapuje `DateTime.MinValue`.

`NSDate` obsługuje maksymalną wyższą i mniejszą wartość minimalna niż `DateTime`. Podczas konwertowania z `NSDate` do `DateTime`, te wartości wyżej i niżej nie zostaną zmienione na `DateTime` [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) lub [MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue)odpowiednio.
