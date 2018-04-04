---
title: Xamarin dla deweloperów języka Java
description: Jeśli jesteś deweloperem Java, to również w sposób korzystania z umiejętności, a istniejący kod na platformie Xamarin podczas korzystać kod ponownie wykorzystać zalety języka C#. Znajdziesz się, że składnia języka C# jest bardzo podobny do składni języka Java i że oba języków udostępnia bardzo podobne funkcje. Ponadto dowiesz się funkcje unikatowe dla C#, która będzie ułatwiać programowanie pracę.
ms.prod: xamarin
ms.assetid: A3B6C041-4052-4E7D-999C-C4FA10BE3D67
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 29fc698e6ed1cfe02ce329813342916d5e7a1651
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="xamarin-for-java-developers"></a>Xamarin dla deweloperów języka Java

_Jeśli jesteś deweloperem Java, to również w sposób korzystania z umiejętności, a istniejący kod na platformie Xamarin podczas korzystać kod ponownie wykorzystać zalety języka C#. Znajdziesz się, że składnia języka C# jest bardzo podobny do składni języka Java i że oba języków udostępnia bardzo podobne funkcje. Ponadto dowiesz się funkcje unikatowe dla C#, która będzie ułatwiać programowanie pracę._


## <a name="overview"></a>Omówienie

Ten artykuł zawiera wprowadzenie do języka C# programowania dla deweloperów języka Java, koncentrujących się głównie na funkcji języka C#, które można napotkać podczas tworzenia aplikacji platformy Xamarin.Android. Ponadto w tym artykule opisano, jak te funkcje różnią się od ich odpowiedniki Java i wprowadza ona ważnych funkcji C# (dotyczy Xamarin.Android), które nie są dostępne w języku Java. Łącza do materiałów referencyjnych dodatkowe są uwzględnione, dzięki czemu można użyć w tym artykule jako "Przechodzenie" punkt do dalszego poznawania C# i platformy .NET.

Jeśli znasz języka Java, będzie uważasz, że natychmiast w domu przy użyciu składni języka C#. Składnia języka C# jest bardzo podobny do składni języka Java &ndash; C# to język "nawias klamrowy" jak Java, C i C++. Na wiele sposobów składnia języka C# odczytuje jak nadzbiorem składni języka Java, ale kilka słów kluczowych zmieniona i dodany.

Wiele kluczowych właściwości Java można znaleźć w języku C#:

-   Programowanie zorientowane obiektowo na podstawie klasy

-   Silne wpisywanie

-   Obsługa interfejsów

-   Typy ogólne

-   Wyrzucanie elementów bezużytecznych

-   Kompilacja środowiska uruchomieniowego

Zarówno Java i C# są kompilowane na język pośredni, który jest uruchamiany w środowisku zarządzanym wykonywania. Zarówno C# i Java statycznie wpisany, a oba języków Traktuj ciągi jako niezmienialny typy.
Zarówno języków użyć hierarchii root pojedynczej klasy. Podobnie jak języka Java, C# obsługuje tylko pojedyncze dziedziczenie i nie zezwala na metody globalne.
W obu językach obiekty są tworzone przy użyciu sterty `new` — słowo kluczowe i obiekty są — w ramach odzyskiwania pamięci gdy są one już używane. Zarówno języków udostępnia posiadanie wyjątek obsługi z `try` / `catch` semantyki. Zarówno obsługi wątku zarządzania i synchronizacji.

Istnieje jednak wiele różnic między Java i C#. Na przykład:

-   Java nie obsługuje niejawnie wpisane zmienne lokalne (C# obsługuje `var` — słowo kluczowe).

-   W języku Java należy przekazać parametry tylko przez wartość, w języku C# można przekazać przez odwołanie, a także przez wartość. (C# zawiera `ref` i `out` słowa kluczowe parametrów może być przekazywany odwołanie; nie ma odpowiednika się one w języku Java).

-   Java nie obsługuje dyrektywy preprocesora, takich jak `#define`.

-   Java nie obsługuje typów całkowitych bez znaku, gdy C zawiera typy liczbę całkowitą bez znaku, takich jak `ulong`, `uint`, `ushort` i `byte`.

-   Java nie obsługuje operatora przeładowanie; w języku C# można przeciążać operatory i konwersje.

-   W języku Java `switch` instrukcji kodu można przejść do następnej sekcji przełącznika, ale w języku C# koniec każdego `switch` sekcji musi wygasać przełącznika (koniec każdej sekcji należy zamknąć z `break` instrukcji).

-   W języku Java, określ wyjątki wyrzucane przez metodę o `throws` — słowo kluczowe, ale C# jest nie zawierają checked wyjątki &ndash; `throws` — słowo kluczowe nie jest obsługiwane w języku C#.

-   C# obsługuje język Language-Integrated zapytania (LINQ), który pozwala używać słów zastrzeżonych `from`, `select`, i `where` można zapisać zapytania względem kolekcji w sposób podobny do kwerend bazy danych.


Oczywiście istnieje wiele więcej różnic między C# i Java nie może być omówione w tym artykule. Ponadto zarówno Java i C# nadal podlegać ewolucji (na przykład obsługuje Java 8, które nie są jeszcze w systemie Android łańcuch narzędzi, C# — styl wyrażenia lambda), te różnice zmieni się wraz z upływem czasu. Tylko najważniejsze różnice występujących obecnie deweloperom języka Java nowych Xamarin.Android zostały opisane w tym miejscu.

-   [Zamierza C# Programowanie w języku Java](#fundamentals) zawiera wprowadzenie do podstawowych różnic między C# i Java.

-   [Funkcje programowanie zorientowane obiektowo](#oopfeatures) opisano najważniejsze różnice w funkcjach zorientowane obiektowo między dwóch języków.

-   [Różnice — słowo kluczowe](#keywords) zawiera tabelę odpowiedniki przydatne — słowo kluczowe języka C# — tylko słowa kluczowe i łącza do definicji — słowo kluczowe języka C#.

C# oferuje wiele funkcji klucza platformy Xamarin.Android, który obecnie nie są łatwo dostępne dla deweloperów języka Java w systemie Android. Te funkcje mogą ułatwić do pisania kodu, lepiej w krótszym czasie:

-   [Właściwości](#properties) &ndash; C# dla właściwości systemu, umożliwia dostęp do zmiennych Członkowskich bezpośrednio i bezpiecznie bez konieczności pisania metody ustawiającej i metody pobierającej.

-   [Lambda Expressionss](#lambdas) &ndash; w języku C# za pomocą metod anonimowych (nazywane również *wyrażeń lambda*) Express z funkcji więcej krótkiej formie i bardziej efektywnie. Można uniknąć konieczności pisania jednorazowe jednorazowej Użyj obiektów obciążenie i stanu lokalnego można przekazać do metody bez dodawania parametrów.

-   [Obsługa zdarzeń](#events) &ndash; C# obsługuje poziom języka *zdarzeniami programowania*, gdzie można zarejestrować obiektu Aby otrzymywać powiadomienia, gdy wystąpi zdarzenie zainteresowań. `event` — Słowo kluczowe definiuje mechanizm emisji multiemisji klasy wydawcy można użyć do powiadamiania subskrybentów zdarzeń.

-   [Programowanie asynchroniczne](#async) &ndash; funkcje programowania asynchroniczne języka C# (`async`/`await`) zachowanie reakcji aplikacji.
    Obsługa języka poziomu tej funkcji powoduje, że programowania asynchronicznego łatwa do wdrożenia i mniej podatne na błędy.


Na koniec Xamarin umożliwia [korzystać z istniejących zasobów Java](#interop) za pomocą technologii, znany jako *powiązania*. W języku C# można wywołać z istniejących Java kodu, struktury i biblioteki dokonując Użyj generatory automatyczne powiązanie dla platformy Xamarin. Aby to zrobić, należy po prostu utworzyć bibliotekę statyczną w języku Java i pozostawić ją C# za pomocą powiązania.

<a name="fundamentals" />

## <a name="going-from-java-to-c-development"></a>Będzie w języku Java Development C#

W poniższych sekcjach podano podstawowe "wprowadzenie" różnice w języku C# i Java; dalszej części artykułu opisano zorientowane obiektowo różnice w tych językach.



### <a name="libraries-vs-assemblies"></a>Vs bibliotek. Zestawy

Java zwykle pakietów powiązanymi klasami w **JAR** plików. W języku C# i środowiska .NET, jednak wielokrotnego użytku fragmenty kodu prekompilowany są pakowane w *zestawy*, które zwykle są dostarczane w *.dll* plików. Zestaw jest jednostką wdrożenia w języku C# / kodu platformy .NET, a każdy zestaw jest zwykle skojarzone z projektu C#. Zestawy zawiera kod pośrednim (IL), który jest just-in-time kompilowane w czasie wykonywania.

Aby uzyskać więcej informacji na temat zestawów, zobacz witrynę MSDN [zestawy i Global Assembly Cache](https://msdn.microsoft.com/en-us/library/ms173099.aspx) tematu.


### <a name="packages-vs-namespaces"></a>Pakiety programu vs. Namespaces

C# używa `namespace` — słowo kluczowe do grupy ze sobą powiązane typy; efekt jest podobny jak w języku Java `package` — słowo kluczowe. Zazwyczaj aplikacji platformy Xamarin.Android będą znajdować się w przestrzeni nazw utworzone dla danej aplikacji. Na przykład deklaruje następującego kodu C# `WeatherApp` przestrzeń nazw otoki raportowania pogody aplikacji:

```csharp
namespace WeatherApp
{
    ...
```


### <a name="importing-types"></a>Trwa importowanie typów

Podczas wprowadzania z typów zdefiniowanych w przestrzeni nazw zewnętrznych, zaimportować te typy z `using` instrukcji (który jest bardzo podobny do Java `import` instrukcji). W języku Java może zaimportować jednego typu z instrukcją podobne do poniższych:

```java
import javax.swing.JButton
```

Może zaimportować cały pakiet języka Java z instrukcją następująco:

```java
import javax.swing.*
```

C# `using` instrukcji działa bardzo podobnie, ale umożliwia importowanie całego pakietu bez określania symbol wieloznaczny. Na przykład często można szereg `using` instrukcje na początku Xamarin.Android te pliki źródłowe, jak pokazano w poniższym przykładzie:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Net;
using System.IO;
using System.Json;
using System.Threading.Tasks;
```

Te instrukcje funkcji z importowania `System`, `Android.App`, `Android.Content`, obszarów nazw itp.



### <a name="generics"></a>Typy ogólne

Obsługa zarówno Java i C# *ogólne*, które są symbole zastępcze, które umożliwiają podłączenie różnych typów w czasie kompilacji. Jednak ogólne działać inaczej w języku C#. W języku Java [wpisz usunięcia](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html) udostępnia informacje o typie kompilacji w czasie, ale nie w czasie wykonywania. Z kolei .NET środowisko uruchomieniowe języka wspólnego (CLR) obsługuje jawnych typów ogólnych, co oznacza, że C# ma dostęp do informacji o typie w czasie wykonywania. W codziennych rozwoju platformy Xamarin.Android, importantance takiego rozróżnienia nie jest często oczywista, ale jeśli używasz [odbicia](https://msdn.microsoft.com/en-us/library/ms173183.aspx), będzie zależeć od tej funkcji w celu dostępu do informacji o typie w czasie wykonywania.

Platformie Xamarin.Android, często można Metoda ogólna `FindViewById` można odwołać się do sterowania układem. Ta metoda przyjmuje parametr typu ogólnego, który określa typ formantu do wyszukiwania. Na przykład:

```csharp
TextView label = FindViewById<TextView> (Resource.Id.Label);
```

W tym przykładzie kodu `FindViewById` pobiera odwołanie do `TextView` formant, który jest zdefiniowany w układzie jako **etykiety**, zwraca go jako `TextView` typu.

Aby uzyskać informacje ogólne, zobacz witrynę MSDN [ogólne](https://msdn.microsoft.com/en-us/library/512aeb7t.aspx) tematu.
Należy pamiętać, że istnieją pewne ograniczenia dotyczące platformy Xamarin.Android obsługę języka C# klas ogólnych; Aby uzyskać więcej informacji, zobacz [ograniczenia](~/android/internals/limitations.md).


<a name="oopfeatures" />

## <a name="object-oriented-programming-features"></a>Funkcje programowanie zorientowane obiektowo

Zarówno Java i C# Użyj bardzo podobne zorientowane obiektowo programowania idioms:

-   Wszystkie klasy ostatecznie pochodzą z obiektu z jednym elementem głównym &ndash; wszystkie obiekty Java pochodzi od `java.lang.Object`, a wszystkie obiekty C# pochodzi od `System.Object`.

-   Wystąpienia klas są typy referencyjne.

-   Gdy uzyskujesz dostęp do właściwości i metody wystąpienia, użyj "<code>.</code>" operator.

-   Wszystkie wystąpienia klasy są tworzone na stosie za pośrednictwem `new` operatora.

-   Ponieważ obu językach używają wyrzucanie elementów bezużytecznych, jest sposób jawnego zwolnienia nieużywanych obiektów (tj. nie ma `delete` — słowo kluczowe, ponieważ wystąpiły jest w języku C++).

-   Możesz rozszerzyć klasy poprzez dziedziczenie i obu językach Zezwalaj tylko pojedyncza klasa podstawowa dla typu.

-   Można zdefiniować interfejsów i może dziedziczyć po klasie (np. wdrożenia) wiele definicji interfejsu.

Istnieją jednak również kilka istotnych różnic:

-   Java ma dwie funkcje zaawansowane, które C# nie obsługuje: anonimowe klasy i wewnętrzny klasy. (C# jednak zezwalają zagnieżdżenia definicje klas &ndash; C# w zagnieżdżonych klasach są podobne do języka Java w statycznej klasy zagnieżdżone.)

-   C# obsługuje typy struktur w stylu języka C (`struct`) a nie w języku Java.

-   W języku C#, można zaimplementować definicję klasy w plikach źródłowych oddzielne przy użyciu `partial` — słowo kluczowe.

-   C# interfejsy nie mogą deklarować pól.

-   C# używa składni destruktora stylu C++ Express finalizatory. Składnia różni się od jego Java `finalize` metody, ale semantyka są prawie takie same. (Należy pamiętać, że w języku C# destruktory automatycznie wywołania destruktora klasy podstawowej &ndash; w przeciwieństwie do języka Java w przypadku, gdy jawnym wywołaniem `super.finalize` jest używany.)



### <a name="class-inheritance"></a>Dziedziczenie oparte na klasach

Aby rozszerzyć klasę w języku Java, należy użyć `extends` — słowo kluczowe. Aby rozszerzyć klasę w języku C#, należy użyć dwukropka (`:`) wskazująca pochodnym. Na przykład w aplikacji platformy Xamarin.Android często można pochodnymi klasy, które przypominają następującego fragmentu kodu:

```csharp
public class MainActivity : Activity
{
    ...
```

W tym przykładzie `MainActivity` dziedziczy `Activity` klasy.

Aby zadeklarować obsługę interfejsu w języku Java, należy użyć `implements` — słowo kluczowe. Jednak w języku C#, wystarczy dodać nazwy interfejsu do listy klas odziedziczone, jak pokazano w tym fragmencie kodu:

```csharp
public class SensorsActivity : Activity, ISensorEventListener
{
    ...
```

W tym przykładzie `SensorsActivity` dziedziczy `Activity` i implementuje funkcje zadeklarowany w `ISensorEventListener` interfejsu. Uwaga na liście interfejsów musi występować po klasie podstawowej (czy wystąpi błąd podczas kompilacji). Konwencja nazwy interfejsu C# jest poprzedzony przez wielkimi literami "I"; Dzięki temu można określić, które klasy są interfejsy bez konieczności `implements` — słowo kluczowe.

Jeśli chcesz uniemożliwić dalsze jest podklasą klasy w języku C# klasy, należy poprzedzić nazwę klasy `sealed` &ndash; w języku Java, poprzedź nazwę klasy z `final`.

Aby uzyskać więcej informacji na temat języka C# definicje klas, zobacz MSDN [klas i struktur](https://msdn.microsoft.com/en-us/library/x9afc042.aspx) i [dziedziczenia](https://msdn.microsoft.com/en-us/library/x9afc042.aspx) tematów.


<a name="properties" />

### <a name="properties"></a>Właściwości

W języku Java metody ustawiającej (ustawiające) i metody inspektora (pobierające) są często używane do kontrolowania sposobu zmiany zostały wprowadzone do elementów członkowskich klasy podczas ukrywanie i chronić tych członków z kodu zewnętrznego. Na przykład Android `TextView` klasa udostępnia `getText` i `setText` metody. C# zapewnia mechanizm podobne, lecz więcej bezpośredniego znany jako *właściwości*.
Użytkownicy klasy C# można uzyskiwać dostęp do właściwości w taki sam sposób czy uzyskania dostępu do pola, ale każdy dostępu faktycznie wynikiem wywołania metody, która jest niewidoczny dla obiekt wywołujący. Ta metoda "w tle" można zaimplementować efekty uboczne, takie jak ustawienie inne wartości, wykonywania konwersji lub zmienianie stanu obiektu.

Właściwości są często używane do uzyskiwania dostępu i modyfikowanie elementów członkowskich obiektu (interfejsu użytkownika) interfejsu użytkownika. Na przykład:

```csharp
int width = rulerView.MeasuredWidth;
int height = rulerView.MeasuredHeight;
...
rulerView.DrawingCacheEnabled = true;
```

W tym przykładzie wysokość i szerokość są odczytywane z `rulerView` obiektu uzyskując dostęp do jego `MeasuredWidth` i `MeasuredHeight` właściwości. Jeśli te właściwości są do odczytu, wartości z wartości pola skojarzony (ale ukryte) pobrane w tle i zwracany do obiektu wywołującego. `rulerView` Obiekt może przechowywać wartości szerokości i wysokości w jednej jednostki miary (na przykład pikseli) i przekonwertować te wartości na bieżąco na inną jednostkę miary (na przykład milimetry) po `MeasuredWidth` i `MeasuredHeight` uzyskać dostępu do właściwości.

`rulerView` Obiekt ma również właściwość o nazwie `DrawingCacheEnabled` &ndash; przykładowy kod ustawia tę właściwość na `true` włączenie rysowania pamięci podręcznej w `rulerView`. W tle skojarzone ukryte pole jest aktualizowany nową wartość i prawdopodobnie innych aspektów `rulerView` stanu są modyfikowane. Na przykład, jeśli `DrawingCacheEnabled` ustawiono `false`, `rulerView` może także usunięcie buforować informacje rysowania już zebranych elementów w obiekcie.

Dostęp do właściwości mogą być odczytu/zapisu, tylko do odczytu lub w trybie tylko do zapisu. Ponadto można użyć modyfikatory dostępu różnych odczytu i zapisu. Na przykład można zdefiniować właściwości, która ma publiczny dostęp do odczytu, ale prywatny dostęp do zapisu.

Aby uzyskać więcej informacji na temat właściwości języka C#, zobacz witrynę MSDN [właściwości](https://msdn.microsoft.com/en-us/library/x9fsa0sw.aspx) tematu.



### <a name="calling-base-class-methods"></a>Wywołanie metod klasy podstawowej

Aby wywołać konstruktora klasy podstawowej w języku C#, należy użyć dwukropka (`:`) następuje `base` — słowo kluczowe i listy inicjalizatora; to `base` wywołania Konstruktora znajduje się natychmiast po liście parametrów pochodnych konstruktora. Konstruktor klasy podstawowej jest wywoływana przy wejściu do konstruktora pochodne; Kompilator wstawia wywołanie konstruktora podstawowego na początku treści metody. Poniższy fragment kodu przedstawia konstruktora podstawowego wywoływana z konstruktora pochodnych w aplikacji platformy Xamarin.Android:

```csharp
public class PictureLayout : ViewGroup
{
    ...
    public class PictureLayout (Context context)
           : base (context)
    {
        ...
    }
    ...
}

```

W tym przykładzie `PictureLayout` jest pochodną klasy `ViewGroup` klasy. `PictureLayout` w poniższym przykładzie Konstruktor akceptuje `context` argumentu i przekazuje je do `ViewGroup` Konstruktor za pośrednictwem `base(context)` wywołania.

Aby wywołać metodę klasy podstawowej w języku C#, użyj `base` — słowo kluczowe. Na przykład aplikacji platformy Xamarin.Android często wykonywać wywołania do metody podstawowej, jak pokazano poniżej:

```csharp
public class MainActivity : Activity
{
    ...
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
```

W takim przypadku `OnCreate` metody zdefiniowanej w klasie pochodnej (`MainActivity`) wywołań `OnCreate` metody klasy podstawowej (`Activity`).



### <a name="access-modifiers"></a>Modyfikatory dostępu

Java i C# obsługuje zarówno `public`, `private`, i `protected` modyfikatorów dostępu. C# obsługuje jednak dwóch modyfikatory dostępu dodatkowe:

-   **`internal`** &ndash; Element członkowski klasy jest dostępny tylko w bieżącym zestawie.

-   **`protected internal`** &ndash; Element członkowski klasy jest dostępny w ramach zestawu definiującego: Definiowanie klasy, a (klas pochodnych wewnątrz lub na zewnątrz zestaw ma dostęp) klas pochodnych.

Aby uzyskać więcej informacji na temat modyfikatory dostępu C#, zobacz witrynę MSDN [modyfikatory dostępu](https://msdn.microsoft.com/en-us/library/ms173121.aspx) tematu.



### <a name="virtual-and-override-methods"></a>Wirtualny i zastąpić metody

Obsługa zarówno Java i C# *polimorfizm*, możliwość traktowania obiekty powiązane w taki sam sposób. W obu językach odwołania klasy podstawowej można użyć do odwoływania się do obiektów klas pochodnych i metod klasy pochodnej można zastąpić metody z jej klas podstawowych. Zarówno języki są pojęcie *wirtualnego* metody, metody w klasie podstawowej, która ma zostać zastąpione przez metodę w klasie pochodnej.
Program Java, C# obsługuje `abstract` klasy i metody.

Istnieją pewne różnice między Java i C# w sposób deklarować w metodach wirtualnych i zastąpić je:

-   W języku C# metody są Niewirtualny domyślnie. Klas nadrzędnych musi jawnie etykietę będące do zastąpienia przy użyciu metody `virtual` — słowo kluczowe. Z kolei metody wirtualne domyślnie są wszystkie metody w języku Java.

-   Aby zapobiec zastąpieniu w języku C# metody, po prostu pozostanie `virtual` — słowo kluczowe. Z kolei używa języka Java `final` — słowo kluczowe, aby oznaczyć metodę o "override nie jest dozwolone."

-   C# pochodnych należy użyć klasy `override` — słowo kluczowe, aby jawnie wskazać, czy metoda wirtualna klasa podstawowa jest zastępowana.

Aby uzyskać więcej informacji na temat C# w obsługę polimorfizm, zobacz witrynę MSDN [polimorfizm](https://msdn.microsoft.com/en-us/library/ms173152.aspx) tematu.


<a name="lambdas" />

## <a name="lambda-expressions"></a>Wyrażenia lambda

C# umożliwia tworzenie *zamknięcia*: wbudowany, anonimowe metody, które mogą uzyskiwać dostęp do stanu metody, w którym są ujęte.
Za pomocą wyrażenia lambda, może zapisać mniej wierszy kodu można zaimplementować te same funkcje, które być może zostało zaimplementowane w języku Java o wiele więcej wierszy kodu.

Wyrażenia lambda umożliwiają można pominąć dodatkowe procedury związane z tworzeniem jednorazowe jednorazowej Użyj klasy lub anonimowe, tak jak w języku Java &ndash; zamiast tego po prostu zapisu logiki biznesowej w tekście kodu metody. Ponadto ponieważ wyrażeń lambda mają dostęp do zmiennych w otaczających metody, nie masz umożliwia utworzenie listy długich parametr do przekazania do kodu metody stanu.

W języku C#, są tworzone wyrażenia lambda za pomocą `=>` operatora, jak pokazano poniżej:

```csharp
(arg1, arg2, ...) => {
    // implementation code
};
```

Platformie Xamarin.Android wyrażenia lambda są często używane w celu zdefiniowania procedury obsługi zdarzeń. Na przykład:

```csharp
button.Click += (sender, args) => {
    clickCount += 1;    // access variable in surrounding code
    button.Text = string.Format ("Clicked {0} times.", clickCount);
};
```

W tym przykładzie kodu wyrażenia lambda (kod w nawiasach klamrowych) zwiększa liczbę kliknij i aktualizacje `button` tekst do wyświetlenia liczba kliknij. To wyrażenie lambda jest zarejestrowana w usłudze `button` obiektu jako obsługi zdarzeń kliknięcia, aby być wywoływana, gdy przycisk jest dotknięciu. (Procedury obsługi zdarzeń są objaśnione szczegółowo poniżej). W tym prostym przykładzie `sender` i `args` parametry nie są używane przez kod wyrażenia lambda, ale są one wymagane w wyrażeniu lambda w celu spełnienia wymagań podpisu metody rejestracji zdarzeń. Pod maską, kompilator języka C# tłumaczy wyrażenia lambda do metody anonimowej, która jest wywoływana przy każdym kliknij przycisk zdarzeń została wykonana.

Aby uzyskać więcej informacji na temat wyrażeń C# i lambda, zobacz witrynę MSDN [wyrażenia Lambda](https://msdn.microsoft.com/en-us/library/bb397687.aspx) tematu.


<a name="events" />

## <a name="event-handling"></a>Obsługa zdarzeń

*Zdarzeń* sposób dla obiekt powiadomiono zarejestrowanych subskrybentów, gdy coś interesującego się do tego obiektu. W odróżnieniu od w języku Java, w przypadku, gdy subskrybent zazwyczaj wykonuje `Listener` interfejs, który zawiera metodę wywołania zwrotnego, C# zapewnia obsługę języka poziom obsługi za pomocą zdarzeń *delegatów*. A *delegować* przypomina wskaźnik funkcji bezpieczny zorientowane obiektowo &ndash; hermetyzuje odwołanie do obiektu i token metody. Jeśli obiekt klient chce subskrybować zdarzenia, tworzy delegata i przekazuje obiekt delegowany do zgłaszających obiektu.
Po wystąpieniu zdarzenia, obiekt zgłaszających wywołuje metodę reprezentowaną przez obiektu delegowanego powiadamiania obiektu subskrybującego klienta zdarzenia. W języku C# programy obsługi zdarzeń są zasadniczo nic więcej niż metody, które są wywoływane przy użyciu delegatów.

Aby uzyskać więcej informacji na temat delegatów, zobacz witrynę MSDN [delegatów](https://msdn.microsoft.com/en-us/library/ms173171.aspx) tematu.

W języku C#, zdarzenia są *multiemisji*; to znaczy więcej niż jeden odbiornik powiadomienia mogą być wysyłane po wystąpieniu zdarzenia. Ta różnica zaobserwowania, kiedy należy wziąć pod uwagę różnice składniowe między rejestracja zdarzeń Java i C#. W języku Java można wywołać `SetXXXListener` zarejestrować powiadomienia o zdarzeniach; w języku C# możesz użyć `+=` operatora, aby zarejestrować powiadomień o zdarzeniach, dodając"" pełnomocnika do listy odbiorników zdarzeń.
W języku Java, należy wywołać `SetXXXListener` wyrejestrować, podczas gdy w języku C# możesz użyć `-=` do "odjęcia" pełnomocnika z listy odbiorników.

Platformie Xamarin.Android zdarzenia są często używane do powiadamiania obiektów, gdy użytkownik wykona coś do formantu interfejsu użytkownika. Zwykle ma elementów członkowskich, które są zdefiniowane przy użyciu formantu interfejsu użytkownika `event` — słowo kluczowe; możesz dołączyć jego delegatów do tych elementów członkowskich, aby subskrybować zdarzenia z tego formantu interfejsu użytkownika.

Aby subskrybować zdarzenia:

1.  Utwórz obiekt delegowany, który odwołuje się do metody, która ma być wywoływana, gdy wystąpi zdarzenie.

2.  Użyj `+=` operatora, aby dołączyć do zdarzenia są subskrybowanie pełnomocnika.

W poniższym przykładzie zdefiniowano delegata (z użyciem jawnego `delegate` — słowo kluczowe) do subskrybowania kliknięcia przycisku.
Ten program obsługi kliknięcia przycisków rozpoczyna się nowego działania:

```csharp
startActivityButton.Click += delegate {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

Jednak również służy wyrażenia lambda do rejestrowania zdarzeń, pomijanie `delegate` — słowo kluczowe całkowicie. Na przykład:

```csharp
startActivityButton.Click += (sender, e) => {
    Intent intent = new Intent (this, typeof (MyActivity));
    StartActivity (intent);
};

```

W tym przykładzie `startActivityButton` obiekt ma zdarzenie oczekuje za pomocą podpisu metody delegata: akceptuje argumenty nadawcy i zdarzeń i zwraca typ void. Jednak ponieważ nie chcemy przejść do problemów, aby oznaczyć obiektu delegowanego lub jego metody, możemy zadeklarować podpis metody z `(sender, e)` i użyć wyrażenia lambda do wdrożenia programu obsługi zdarzeń.
Należy pamiętać, że mamy zadeklarować tej listy parametrów, mimo że firma Microsoft nie są używane `sender` i `e` parametrów.

Należy pamiętać, że możesz anulować subskrypcję delegata (za pośrednictwem `-=` — operator), ale wyrażenia lambda nie można anulować &ndash; próby może powodować przecieki pamięci. Formularz lambda rejestracji zdarzeń tylko wtedy, gdy Twoje obsługi nie będą subskrypcję zdarzenia.

Zwykle wyrażenia lambda są używane do Zadeklaruj procedury obsługi zdarzeń w kodzie platformy Xamarin.Android. Dzięki temu skrócona do deklarowania procedury obsługi zdarzeń może wydawać się one niezrozumiałe, w pierwszej, ale zapisuje ogromnych ilości godzinie, gdy jest zapisywanie i odczytywanie kodu. Zwiększenie znajomości, staje się przyzwyczajony do rozpoznawania tego wzorca (które często występuje w kodzie Xamarin.Android) i poświęcić więcej czasu planowania logiki biznesowej aplikacji i mniej czasu Praca za pośrednictwem syntaktycznych koszty.


<a name="async" />

## <a name="asynchronous-programming"></a>Programowanie asynchroniczne

*Programowanie asynchroniczne* pozwala zwiększyć ogólną szybkość reakcji aplikacji. Funkcje asynchroniczne programowania umożliwiają pozostałej części kodu aplikacji do kontynuowania pracy, gdy część aplikacji jest zablokowana z powodu długotrwałej operacji.
Uzyskiwanie dostępu do sieci web, przetwarzania obrazów i odczytu/zapisu, czy pliki są operacje, które mogą spowodować wyświetlenie zawiesza, jeśli nie są one zapisywane asynchronicznie całą aplikację.

C# obsługuje poziom języka programowania asynchronicznego za pośrednictwem `async` i `await` słów kluczowych. Te funkcje językowe stał się bardzo łatwe do pisania kodu, który wykonuje długotrwałe zadania bez blokowania wątku głównego aplikacji. Krótko mówiąc, użyj `async` — słowo kluczowe dla metody, aby wskazać, że kod w metodzie jest uruchamiane asynchronicznie i blokuje wątków obiektu wywołującego. Możesz użyć `await` — słowo kluczowe po wywołaniu metody, które są oznaczone ikoną z `async`. Kompilator interpretuje `await` jako punkt przypadku Twojego wykonanie metody zostanie przeniesiony na wątku w tle (zadanie jest zwracany do obiektu wywołującego). Po ukończeniu tego zadania wykonywania kodu wznawia w wątku obiektu wywołującego w `await` punktu w kodzie, zwracania wyników z `async` wywołania. Według Konwencji mają metod, które są uruchamiane asynchronicznie `Async` sufiks nazwy.

W aplikacji platformy Xamarin.Android `async` i `await` są zazwyczaj używane w celu zwolnienia wątku interfejsu użytkownika, dzięki czemu mogą odpowiadać na dane wejściowe użytkownika (takich jak naciśnięcie z **anulować** przycisk) podczas długotrwałej operacji odbywa się w zadanie w tle.

W poniższym przykładzie przycisk kliknij powoduje, że program obsługi zdarzeń operację asynchroniczną można pobrać obrazu z sieci web:


```csharp
downloadButton.Click += downloadAsync;
...
async void downloadAsync(object sender, System.EventArgs e)
{
    webClient = new WebClient ();
    var url = new Uri ("http://photojournal.jpl.nasa.gov/jpeg/PIA15416.jpg");
    byte[] bytes = null;

    bytes = await webClient.DownloadDataTaskAsync(url);

    // display the downloaded image ...
```

W tym przykładzie, gdy użytkownik kliknie `downloadButton` kontroli, `downloadAsync` tworzy program obsługi zdarzeń `WebClient` obiektu i `Uri` obiektu można pobrać obrazu z określonego adresu URL. Następnie wywołuje `WebClient` obiektu `DownloadDataTaskAsync` metodę z tego adresu URL do pobrania obrazu.

Zwróć uwagę, że deklaracja metody `downloadAsync` jest poprzedzone `async` słowo kluczowe, aby wskazać, że będą uruchamiane asynchronicznie i zwracać zadanie. Należy również zauważyć, że wywołanie `DownloadDataTaskAsync` jest poprzedzony `await` — słowo kluczowe. Aplikacja przenosi wykonywanie programu obsługi zdarzeń (począwszy od punktu gdzie `await` pojawi się) do wątku w tle do `DownloadDataTaskAsync` kończy i zwraca.
W tym samym czasie nadal wątku interfejsu użytkownika aplikacji odpowiada na dane wejściowe użytkownika i wyzwalać obsługi zdarzeń dla innych formantów. Gdy `DownloadDataTaskAsync` zakończeniu (może to zająć kilka sekund), gdzie wznawia wykonywanie `bytes` zmienna jest ustawiona na wynik wywołania `DownloadDataTaskAsync`, i pozostałej części kod obsługi zdarzenia Wyświetla pobrany obraz na wywołującego (UI) Wątek.

Aby obejrzeć wprowadzenie do `async` / `await` w języku C#, zobacz witrynę MSDN [programowanie asynchroniczne z Async i Await](https://msdn.microsoft.com/en-us/library/hh191443.aspx) tematu.
Aby uzyskać więcej informacji o obsłudze Xamarin funkcje programowania asynchroniczne, zobacz [omówienie obsługuje asynchroniczne](~/cross-platform/platform/async.md).


<a name="keywords" />

## <a name="keyword-differences"></a>Różnice — słowo kluczowe

Wiele słów kluczowych języka używany w języku Java są również używane w języku C#. Istnieje również wiele słów kluczowych języka Java, które mają równoważne, ale inaczej o nazwie będącej jej odpowiednikiem w języku C#, wymienione w poniższej tabeli:

|Java|C#|Opis|
|---|---|---|
|`boolean`|[bool](https://msdn.microsoft.com/en-us/library/c8f5xwh7.aspx)|Używane do deklarowania wartości logiczne true i false.|
|`extends`|`:`|Poprzedza klasy i interfejsy dziedziczyć.|
|`implements`|`:`|Poprzedza klasy i interfejsy dziedziczyć.|
|`import`|[using](https://msdn.microsoft.com/en-us/library/zhdeatwt.aspx)|Importuje typy z przestrzeni nazw, również używane w celu utworzenia aliasu przestrzeni nazw.|
|`final`|[sealed](https://msdn.microsoft.com/en-us/library/88c54tsw.aspx)|Zapobiega pochodnym klasy; Zapobiega metody i właściwości zostały przesłonięte w klasach pochodnych.|
|`instanceof`|[is](https://msdn.microsoft.com/en-us/library/scekt9xw.aspx)|Określa, czy obiekt jest zgodny z danym typem.|
|`native`|[extern](https://msdn.microsoft.com/en-us/library/e59b22c5.aspx)|Deklaruje metodę, która jest zaimplementowana zewnętrznie.|
|`package`|[namespace](https://msdn.microsoft.com/en-us/library/z2kcy19k.aspx)|Deklaruje zakres dla zestawu powiązanych obiektów.|
|`T...`|[params T](https://msdn.microsoft.com/en-us/library/w5zay9db.aspx)|Określa parametr metody, która przyjmuje zmienną liczbę argumentów.|
|`super`|[base](https://msdn.microsoft.com/en-us/library/hfw7t1ce.aspx)|Umożliwia dostęp do elementów członkowskich z klasy nadrzędnej, w klasie pochodnej.|
|`synchronized`|[lock](https://msdn.microsoft.com/en-us/library/c5kehkcz.aspx)|Opakowuje krytyczne sekcji kodu z nabycia blokady i wersji.|


Ponadto istnieje wiele słów kluczowych, które są unikatowe dla C# i nie mają odpowiedników w języku Java. Kod platformy Xamarin.Android często korzysta z następujących słów kluczowych C# (Ta tabela jest przydatne do odwoływania się do podczas czytania za pośrednictwem platformy Xamarin.Android [przykładowy kod](https://developer.xamarin.com/samples/android/all/)):

|C#|Opis|
|---|---|
|[as](https://msdn.microsoft.com/en-us/library/cscsdfbt.aspx)|Wykonuje konwersje między niezgodne odwołania typy lub wartości null.|
|[async](https://msdn.microsoft.com/en-us/library/hh156513.aspx)|Określa, że metoda lub wyrażenie lambda jest asynchroniczne.|
|[await](https://msdn.microsoft.com/en-us/library/hh156528.aspx)|Wstrzymuje wykonywanie metody do momentu ukończenia zadania.|
|[byte](https://msdn.microsoft.com/en-us/library/5bdb6693.aspx)|Typ bez znaku 8-bitową liczbą całkowitą.|
|[delegate](https://msdn.microsoft.com/en-us/library/900fyy8e.aspx)|Używane w celu hermetyzacji metody lub metody anonimowej.|
|[enum](https://msdn.microsoft.com/en-us/library/sbbt4032.aspx)|Deklaruje wyliczenie zestaw stałe nazwane.|
|[event](https://msdn.microsoft.com/en-us/library/8627sbea.aspx)|Deklaruje zdarzenia w klasie wydawcy.|
|[Stałe](https://msdn.microsoft.com/en-us/library/f58wzh21.aspx)|Zapobiega przeniesieniu zmiennej.|
|`get`|Definiuje metodę dostępu, który pobiera wartość właściwości.|
|[in](https://msdn.microsoft.com/en-us/library/dd469484.aspx)|Umożliwia parametrów przyjąć typu mniej pochodnego w ogólny interfejs.|
|[object](https://msdn.microsoft.com/en-us/library/9kkx3h3c.aspx)|Alias dla typu obiektu w programie .NET framework.|
|[out](https://msdn.microsoft.com/en-us/library/t3c3bfhx.aspx)|Modyfikator parametrów lub deklaracji parametru typu ogólnego.|
|[override](https://msdn.microsoft.com/en-us/library/ebca9ah3.aspx)|Rozszerza lub modyfikuje implementacji dziedziczonego elementu członkowskiego.|
|[częściowe](https://msdn.microsoft.com/en-us/library/6b0scde8.aspx)|Deklaruje definicja, która ma zostać podzielony na wiele plików lub dzieli definicję metody z jej wdrożenia.|
|[readonly](https://msdn.microsoft.com/en-us/library/acdd6hb7.aspx)|Deklaruje, że element członkowski klasy można przypisać tylko w czasie zgłoszenia lub konstruktora klasy.|
|[ref](https://msdn.microsoft.com/en-us/library/14akc2c7.aspx)|Powoduje, że argument przekazywany przez odwołanie, a nie przez wartość.|
|[set](https://msdn.microsoft.com/en-us/library/ms228368.aspx)|Definiuje metodę dostępu, która ustawia wartości właściwości.|
|[string](https://msdn.microsoft.com/en-us/library/362314fe.aspx)|Alias typu ciąg w programie .NET framework.|
|[struct](https://msdn.microsoft.com/en-us/library/ah19swz4.aspx)|Typ wartości, która hermetyzuje grupę powiązanych zmiennych.|
|[typeof](https://msdn.microsoft.com/en-us/library/58918ffs.aspx)|Pobiera typ obiektu.|
|[var](https://msdn.microsoft.com/en-us/library/bb383973.aspx)|Deklaruje niejawnie wpisanych zmiennej lokalnej.|
|[value](https://msdn.microsoft.com/en-us/library/a1khb4f8.aspx)|Odwołuje się do wartości, które chce kod klienta można przypisać do właściwości.|
|[virtual](https://msdn.microsoft.com/en-us/library/9fkccyh4.aspx)|Zapewnia metody do zastąpienia w klasie pochodnej.|


<a name="interop" />

## <a name="interoperating-with-existing-java-code"></a>Współdziałanie z istniejącego kodu języka Java

Jeśli masz istniejące funkcje Java, która nie ma zostać przekonwertowany na język C#, mogą ponownie wykorzystać istniejące bibliotek języka Java w aplikacji platformy Xamarin.Android przy użyciu dwóch metod:

-  **Utwórz bibliotekę powiązania Java** &ndash; przy użyciu tej metody, za pomocą platformy Xamarin narzędzi wygenerować otoki C# wokół typów języka Java. Te otoki są nazywane *powiązania*. W związku z tym można użyć aplikacji platformy Xamarin.Android z *JAR* pliku przez wywołanie te otoki.

-  **Interfejs natywny Java** &ndash; *natywnego interfejsu Java* (JNI) to platforma, która umożliwia aplikacji C# połączeń telefonicznych lub być wywoływany przez kod języka Java.

Aby uzyskać więcej informacji o tych metod, zobacz [Omówienie integracji Java](~/android/platform/java-integration/index.md).



## <a name="for-further-reading"></a>Dalsze informacje

MSDN [C# przewodnik programowania w języku](https://msdn.microsoft.com/en-us/library/67ef8sbd.aspx) to dobry sposób, aby rozpocząć się dowiedzieć, język programowania C# i może używać [odwołanie w C#](https://msdn.microsoft.com/en-us/library/618ayhy6.aspx) do wyszukiwania określonego funkcji języka C#.

W ten sam sposób, że Java wiedzy jest co najmniej tyle o znajomość języka Java biblioteki klas jako znajomość języka Java wiedzy praktycznej języka C# wymaga masz pewną znajomość programu .NET framework. Firmy Microsoft [przenoszenie C# i .NET Framework dla deweloperów języka Java](https://www.microsoft.com/en-us/download/details.aspx?id=6073) uczenia pakietów jest to dobry sposób, aby dowiedzieć się więcej o programie .NET framework z punktu widzenia Java (podczas uzyskiwania głębsze zrozumienie języka C#).

Gdy wszystko będzie gotowe do pracy nad pierwszy projekt platformy Xamarin.Android w języku C# naszych [Hello, Android](~/android/get-started/hello-android/index.md) serii ułatwiają tworzenie pierwszej aplikacji platformy Xamarin.Android i stopień zaawansowania zrozumienie podstaw systemu android Tworzenie aplikacji za pomocą platformy Xamarin.



## <a name="summary"></a>Podsumowanie

W tym artykule podać wprowadzenie do środowiska programowania Xamarin.Android C# z punktu widzenia Java developer. Wskazała podobieństwa między C# i Java podczas wyjaśniający praktyczne różnice między nimi. Go wprowadzono zestawy i przestrzenie nazw, wyjaśniono sposób importowania typów zewnętrznych i podać omówienie różnice w modyfikatory dostępu, ogólne, pochodnym klasy, wywoływanie metod klasy podstawowej, zastępowanie metody i obsługi zdarzeń. Spowodował funkcji C#, które nie są dostępne w języku Java, takie jak właściwości `async` / `await` programowanie asynchroniczne, wyrażenia lambda, obiektów delegowanych C# i C# Obsługa zdarzeń systemu. Go uwzględnione tabel ważne C# słów kluczowych, wyjaśniono sposób współdziałać z istniejących bibliotek języka Java i podano linki do powiązanej dokumentacji dla dalszego badania.


## <a name="related-links"></a>Linki pokrewne

- [Omówienie integracji Java](~/android/platform/java-integration/index.md)
- [Przewodnik programowania w języku C#](https://msdn.microsoft.com/en-us/library/67ef8sbd.aspx)
- [Odwołanie w C#](https://msdn.microsoft.com/en-us/library/618ayhy6.aspx)
- [Przenoszenie do języka C# i .NET Framework dla deweloperów języka Java](https://www.microsoft.com/en-us/download/details.aspx?id=6073)
