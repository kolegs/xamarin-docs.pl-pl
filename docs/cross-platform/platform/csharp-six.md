---
title: C# 6 nowych funkcji — omówienie
description: Najnowsza wersja języka C# — w wersji 6 — w dalszym ciągu rozwijany języka mniej standardowej, zwiększenia przejrzystości i spójności więcej. Składnia inicjalizacji czyszczący, możliwość używania await w blokach catch/finally i warunkowe null? operator są szczególnie użyteczne.
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.technology: xamarin-cross-platform
ms.custom: xamu-video
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 2a189a19280576876e5d5a6a4fa34d2d00cab330
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="c-6-new-features-overview"></a>C# 6 nowych funkcji — omówienie

_Najnowsza wersja języka C# — w wersji 6 — w dalszym ciągu rozwijany języka mniej standardowej, zwiększenia przejrzystości i spójności więcej. Składnia inicjalizacji czyszczący, możliwość używania await w blokach catch/finally i warunkowe null? operator są szczególnie użyteczne._

Ten dokument wprowadza nowe funkcje języka C# 6. Jest ona w pełni obsługiwane przez kompilator mono i deweloperów można uruchomić na wszystkich platformach docelowych Xamarin przy użyciu nowych funkcji.

Ten artykuł zawiera krótkie fragmenty kodu języka C# 6 ilustrujące podstawowe wykorzystanie.
Przykładowa aplikacja jest działającą na wszystkich platformach docelowych Xamarin i wykonuje różnych funkcji programu wiersza polecenia.


> [!VIDEO https://youtube.com/embed/7UdV7zGPfMU]

**Nowości w języku C# 6, przez [Xamarin University](https://university.xamarin.com/)**


## <a name="development-environment"></a>Środowisko deweloperskie

### <a name="mac"></a>Mac

* **Visual Studio for Mac** ma obsługę języka C# 6: możesz skompilować i kompilowania aplikacji platformy Xamarin.IOS przy użyciu funkcji języka C# 6.
  Przeczytaj więcej na temat [programu Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/).

### <a name="windows"></a>Windows

* **Visual Studio 2015 i 2017** i nowszym mają pełną obsługę języka C# 6. Wcześniejszych wersji programu Visual Studio nie obsługuje języka C# 6.

* **Program Xamarin Studio dla systemu Windows** nie obsługuje funkcji języka C# 6 w edytorze.



## <a name="compiler"></a>Kompilatora

Kompilator Mono C# 6 jest uwzględniona w Mono 4.0 lub nowszy, czyli [bezpłatnie pobrać](http://www.mono-project.com/download/).
Visual Studio for Mac automatycznie aktualizuje Mono instalacji w tym systemie.

Użytkownicy systemu Windows muszą mieć [programu Visual Studio 2015 lub 2017 ^](https://www.visualstudio.com/) zainstalowane do kompilowania kodu C# 6 (nawet jeśli wybierzesz Xamarin Studio dla systemu Windows jako środowiskiem IDE).

^ lub *[Microsoft kompilacji narzędzia 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48159)* polecenia wiersz na przykład na kompilacji lub serwery kompilacji.

## <a name="using-c-6"></a>Przy użyciu języka C# 6

Kompilator języka C# 6 jest używany we wszystkich najnowszych wersjach programu Visual Studio dla komputerów Mac.
Te przy użyciu kompilatory w wierszu polecenia należy upewnić się, że `mcs --version` zwraca 4.0 lub nowszy.
Visual Studio dla użytkowników komputerów Mac można Sprawdź, czy mają one Mono 4 (lub nowsza) zainstalowany, odwołując się do **dotyczące programu Visual Studio for Mac > programu Visual Studio for Mac > Pokaż szczegóły**.

## <a name="less-boilerplate"></a>Mniej standardowego
### <a name="using-static"></a>przy użyciu statycznego
Wyliczenia i niektórych klas takich jak `System.Math`, są głównie posiadaczy wartości statyczne i funkcji. W języku C# 6, możesz zaimportować wszystkie statyczne elementy członkowskie tego typu za pomocą jednej `using static` instrukcji. Porównanie typowych funkcji trygonometryczne w języku C# 5 i 6 C#:

```csharp
// Classic C#
class MyClass
{
    public static Tuple<double,double> SolarAngleOld(double latitude, double declination, double hourAngle)
    {
        var tmp = Math.Sin (latitude) * Math.Sin (declination) + Math.Cos (latitude) * Math.Cos (declination) * Math.Cos (hourAngle);
        return Tuple.Create (Math.Asin (tmp), Math.Acos (tmp));
    }
}

// C# 6
using static System.Math;

class MyClass
{
    public static Tuple<double, double> SolarAngleNew(double latitude, double declination, double hourAngle)
    {
        var tmp = Asin (latitude) * Sin (declination) + Cos (latitude) * Cos (declination) * Cos (hourAngle);
        return Tuple.Create (Asin (tmp), Acos (tmp));
    }
}
```

`using static` nie powoduje publicznego `const` pól, takich jak `Math.PI` i `Math.E`, jest dostępny bezpośrednio:

```csharp
for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... 
//PI is const, not static, so requires Math.PI
```

### <a name="using-static-with-extension-methods"></a>przy użyciu statycznych z metody rozszerzenia

`using static` Zakładzie nieco działa inaczej za pomocą metod rozszerzenia. Mimo że metody rozszerzenia są napisane przy użyciu `static`, ich nie ma sensu bez wystąpienia, na którym ma działać. W takim przypadku `using static` jest używany z typem, który definiuje metody rozszerzenia, metody rozszerzenia udostępniane na ich typ docelowy (metoda `this` typu). Na przykład `using static System.Linq.Enumerable` może służyć do rozszerzenia interfejsu API z `IEnumerable<T>` obiektów bez wyświetlania we wszystkich typów LINQ:

```csharp
using static System.Linq.Enumerable;
using static System.String;

class Program
{
    static void Main()
    {
        var values = new int[] { 1, 2, 3, 4 };
        var evenValues = values.Where (i => i % 2 == 0);
        System.Console.WriteLine (Join(",", evenValues));
    }
}
```

W poprzednim przykładzie pokazano różnice w zachowaniu: metody rozszerzenia `Enumerable.Where` jest skojarzony z tablicą, podczas metody statycznej `String.Join` można wywołać bez odwołania do `String` typu.

### <a name="nameof-expressions"></a>nameof wyrażenia
Czasami, chcesz odwołać się do nazwy został przyznany zmiennej lub pola. W języku C# 6 `nameof(someVariableOrFieldOrType)` zwróci ciąg `"someVariableOrFieldOrType"`. Na przykład gdy trwa Zgłaszanie wyjątku `ArgumentException` jest bardzo prawdopodobne nadać nazwy, która argumentów jest nieprawidłowa:

```csharp
throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))
```

Zaletą dyrektora `nameof` wyrażenia jest są sprawdzane typu i są zgodne z zasilane z narzędzia do refaktoryzacji. Typ sprawdzania `nameof` wyrażenia jest powitalnej szczególnie w sytuacjach, w którym `string` służy do dynamicznie Skojarz typy. Na przykład w systemie iOS `string` służy do określania typ używany do prototypu `UITableViewCell` obiekty w `UITableView`. `nameof` mogą zapewnić, że tego skojarzenia nie kończy się niepowodzeniem ze względu na błąd pisowni lub refaktoryzacji Niechlujna treść:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
    cell.TextLabel.Text = objects [indexPath.Row].ToString ();
    return cell;
}
```

Mimo że można przekazać do nazwy kwalifikowanej `nameof`, ostatniego elementu (po ostatniej `.`) jest zwracany. Na przykład można dodać powiązanie danych w platformy Xamarin.Forms:

```csharp
var myReactiveInstance = new ReactiveType ();
var myLabelOld.BindingContext = myReactiveInstance;
var myLabelNew.BindingContext = myReactiveInstance;
var myLabelOld.SetBinding (Label.TextProperty, "StringField");
var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));
```

Dwa wywołań `SetBinding` przekazywane identycznymi wartościami: `nameof(ReactiveType.StringField)` jest `"StringField"`, a nie `"ReactiveType.StringField"` zgodnie z oczekiwaniami może początkowo.

## <a name="null-conditional-operator"></a>Operatora warunkowego wartości null
Typy dopuszczające wartości zerowe i operatora łączenia wartości null wprowadzone wcześniej aktualizacje C# `??` Aby zmniejszyć ilość schematyczny kod podczas przetwarzania wartości null. C# 6 nadal ten motyw z operatorem"warunkowe null" `?.`. Użycie obiektu po prawej stronie wyrażenia, operatora warunkowego wartości null zwraca wartość elementu członkowskiego, jeśli obiekt nie jest `null` i `null` inaczej:

```csharp
var ss = new string[] { "Foo", null };
var length0 = ss [0]?.Length; // 3
var length1 = ss [1]?.Length; // null
var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]
```

(Zarówno `length0` i `length1` są wywnioskować typu `int?`)

W ostatnim wierszu w poprzednim przykładzie pokazano `?` operatora warunkowego wartości null w połączeniu z `??` operatora łączenia wartości null. Zwraca nowy operator warunkowe null C# 6 `null` 2 elementu w tablicy, w którym operatora łączenia wartości null jest uruchamiane i dostarcza 0, aby `lengths` tablicy (czy jest albo nie jest, oczywiście konkretnego problemu).

Operator warunkowe null powinien znacznie skrócić umożliwiającego niezbędnych do sprawdzenia wartości null w wielu, wiele aplikacji.

Istnieją pewne ograniczenia dotyczące operatora warunkowego wartości null z powodu niejednoznaczności. Nie można bezpośrednio po `?` z listą argumentów ujętego w nawiasy, jako użytkownik może nadzieję, że z delegata:

```csharp
SomeDelegate?("Some Argument") // Not allowed
```

Jednak `Invoke` może służyć do rozdzielania `?` z listy argumentów i nadal jest oznaczony jako poprawy za pośrednictwem `null`— sprawdzanie bloku standardowa:

```csharp
public event EventHandler HandoffOccurred;
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    HandoffOccurred?.Invoke (this, userActivity.UserInfo);
    return true;
}
```

## <a name="string-interpolation"></a>Ciąg interpolacji
`String.Format` Funkcja tradycyjnie użył indeksów jako symbole zastępcze w ciągu formatu, np. `String.Format("Expected: {0} Received: {1}.", expected, received`). Oczywiście dodanie nowej wartości zawsze został użyty irytujące małego zadanie inwentaryzacji się argumenty, zmiany numeracji symbole zastępcze i wstawianie nowych argumentów w sekwencji prawej na liście argumentów.

Nowa funkcja interpolacji ciągu języka C# 6 w znacznie poprawia `String.Format`. Teraz, bezpośrednio nazwy zmiennych w ciągu prefiksem `$`. Przykład:

```csharp
$"Expected: {expected} Received: {received}."
```

Oczywiście zmienne zaznaczono i zmiennej błędne lub bez dostępności spowoduje błąd kompilatora.

Symbole zastępcze muszą być zmienne proste, mogą być dowolne wyrażenie. W ramach tych symboli zastępczych można użyć znaków cudzysłowu *bez* anulowanie tych ofert. Należy zwrócić uwagę na wystąpienia `"s"` poniżej:

```csharp
var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"
```

Ciąg interpolacji obsługuje wyrównanie i formatowania składnię `String.Format`. Podobnie jak uprzednio zapisano `{index, alignment:format}`, w języku C# 6 pisania `{placeholder, alignment:format}`:

```csharp
using static System.Linq.Enumerable;
using System;

class Program
{
    static void Main ()
    {
        var values = new int[] { 1, 2, 3, 4, 12, 123456 };
        foreach (var s in values.Select (i => $"The value is { i,10:N2}.")) {
            Console.WriteLine (s);
        }
    Console.WriteLine ($"Minimum is { values.Min(i => i):N2}.");
    }
}
```
wynikiem:

    The value is       1.00.
    The value is       2.00.
    The value is       3.00.
    The value is       4.00.
    The value is      12.00.
    The value is 123,456.00.
    Minimum is 1.00.

Ciąg interpolacji jest składni sugar dla `String.Format`: nie można używać z `@""` Literały ciągu i nie jest zgodny z `const`, nawet jeśli są używane nie symbole zastępcze:

```csharp
const string s = $"Foo"; //Error : const requires value
```

W typowych — przypadek użycia tworzenia argumenty funkcji z interpolacji ciągu należy nadal należy zachować ostrożność anulowanie, kodowanie i problemy kultury. Zapytania SQL i adres URL są oczywiście krytyczne oczyszczenia. Jak `String.Format`, string używa interpolacji `CultureInfo.CurrentCulture`. Przy użyciu `CultureInfo.InvariantCulture` jest nieco długi:

```csharp
Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"
```

## <a name="initialization"></a>Inicjalizacja

C# 6 zapewnia kilka różnych sposobów zwięzła, aby określić właściwości, pól i elementów członkowskich.

### <a name="auto-property-initialization"></a>Inicjowanie Auto właściwością

Teraz można zainicjować właściwości auto w taki sam sposób zwięzły jako pola. Niezmienne auto właściwości mogą być zapisywane z tylko metody pobierającej:

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
```

W Konstruktorze można ustawić wartość tylko do metody pobierającej auto właściwością:

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
    public string Description { get; }

    public ToDo (string description)
    {
        this.Description = description; //Can assign (only in constructor!)
    }
```

Ten inicjowania właściwości auto jest funkcją ogólne oszczędności miejsca i zwiększa deweloperom korzystającym chcące wyróżnić immutability w nich obiekty.

### <a name="index-initializers"></a>Inicjatory indeksu

C# 6 wprowadza inicjatory indeksu, które umożliwiają skonfigurowanie klucz i wartość w typach, które mają indeksatora. Zazwyczaj jest to dla `Dictionary`— styl struktur danych:

```csharp
partial void ActivateHandoffClicked (WatchKit.WKInterfaceButton sender)
{
    var userInfo = new NSMutableDictionary {
        ["Created"] = NSDate.Now,
        ["Due"] = NSDate.Now.AddSeconds(60 * 60 * 24),
        ["Task"] = Description
    };
    UpdateUserActivity ("com.xamarin.ToDo.edit", userInfo, null);
    statusLabel.SetText ("Check phone");
}
```

### <a name="expression-bodied-function-members"></a>Elementy członkowskie zabudowanych wyrażenie — funkcja

Funkcje lambda ma wiele zalet, z których jeden jest po prostu oszczędzanie miejsca. Podobnie elementy członkowskie klasy zabudowanych wyrażenie Zezwalaj małe funkcje wyrażane nieco krótkiej formie niż we wcześniejszych wersjach języka C# 6.

Elementy członkowskie zabudowanych wyrażenia funkcji należy użyć zamiast składni tradycyjnych bloku składni strzałkę lambda:

```csharp
public override string ToString () => $"{FirstName} {LastName}";
```

Powiadomienia, czy składnia strzałkę lambda nie używa jawnego `return`. Dla funkcji zwracające `void`, wyrażenie musi być również instrukcja:

```csharp
public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");
```

Zabudowanych wyrażenie elementy członkowskie są nadal dotyczyć reguła który `async` jest obsługiwana w przypadku metod, ale nie właściwości:

```csharp
//A method, so async is valid
public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
//The following property will not compile
public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;
```

## <a name="exceptions"></a>Wyjątki

Nie istnieje żadne dwa sposoby o: obsługa wyjątków jest trudna do pobrania prawo. Nowe funkcje w języku C# 6 należy obsługi wyjątków, bardziej elastyczne i spójne.

### <a name="exception-filters"></a>Filtry wyjątków

Zgodnie z definicją w nietypowych występują wyjątki, a może być bardzo trudne Przyczyna i kodu o *wszystkie* sposoby może wystąpić wyjątek określonego typu. C# 6 wprowadzono możliwość zabezpieczenia programu obsługi, który wykonanie z filtrem oceniane w czasie wykonywania. Jest to realizowane przez dodanie `when (bool)` wzorzec po normalnej `catch(ExceptionType)` deklaracji. Poniższa filtr odróżnia błąd podczas analizy odnoszących się do `date` parametru w przeciwieństwie do innych błędy analizy.

```csharp
public void ExceptionFilters(string aFloat, string date, string anInt)
{
    try
    {
        var f = Double.Parse(aFloat);
        var d = DateTime.Parse(date);
        var n = Int32.Parse(anInt);
    } catch (FormatException e) when (e.Message.IndexOf("DateTime") > -1) {
        Console.WriteLine ($"Problem parsing \"{nameof(date)}\" argument");
    } catch (FormatException x) {
        Console.WriteLine ("Problem parsing some other argument");
    }
}
```

### <a name="await-in-catchfinally"></a>await w catch... finally...

`async` Możliwości wprowadzono w języku C# 5 zostały zmieniacz gry dla języka. W języku C# 5 `await` nie jest dozwolone w `catch` i `finally` blokuje irytację podane wartości `async/await` możliwości. C# 6 usuwa to ograniczenie, umożliwiając asynchroniczne wyniki, aby być spójnie oczekiwane przez program pokazane na poniższy fragment kodu:

```csharp
async void SomeMethod()
{
    try {
        //...etc...
    } catch (Exception x) {
        var diagnosticData = await GenerateDiagnosticsAsync (x);
        Logger.log (diagnosticData);
    } finally {
        await someObject.FinalizeAsync ();
    }
}
```

## <a name="summary"></a>Podsumowanie

W języku C# w dalszym ciągu rozwijany wprowadzania deweloperzy efektywniej również promowanie dobrych praktyk i obsługi narzędzi. Tego dokumentu została podana omówienie nowych funkcji języka C# 6 i krótko wykazuje ich używania.

## <a name="related-links"></a>Linki pokrewne

- [Nowe funkcje języka C# 6](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)
- [Przy użyciu języka C# 6 skoroszytu](https://developer.xamarin.com/workbooks/csharp/csharp6/csharp6.workbook)
