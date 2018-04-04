---
title: Async — omówienie
description: 'Najnowsza wersja języka C# — w wersji 5 — wprowadzone dwóch nowych słów kluczowych express operacji asynchronicznych: async i await. Słowa kluczowe umożliwiają pisania prostych kodu, który korzysta z biblioteki równoległych zadań do wykonania długotrwałe operacje (np. dostępu do sieci) w innym wątku i łatwo uzyskiwać dostęp do wyników po zakończeniu. Najnowsze wersje platformy Xamarin.iOS i platformy Xamarin.Android obsługuje async i await — ten dokument zawiera wyjaśnienia dotyczące i przykład za pomocą nowej składni za pomocą platformy Xamarin.'
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 0ecad6259cb0d472ac39afb0a6be980d4582812c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="async-support-overview"></a>Omówienie obsługi Async

_Najnowsza wersja języka C# — w wersji 5 — wprowadzone dwóch nowych słów kluczowych express operacji asynchronicznych: async i await. Słowa kluczowe umożliwiają pisania prostych kodu, który korzysta z biblioteki równoległych zadań do wykonania długotrwałe operacje (np. dostępu do sieci) w innym wątku i łatwo uzyskiwać dostęp do wyników po zakończeniu. Najnowsze wersje platformy Xamarin.iOS i platformy Xamarin.Android obsługuje async i await — ten dokument zawiera wyjaśnienia dotyczące i przykład za pomocą nowej składni za pomocą platformy Xamarin._

Obsługa asynchronicznego dla platformy Xamarin jest oparty na podstawie Mono 3.0 i uaktualnia profilu interfejsu API jest przyjazna wersji Silverlight, aby być w wersji platformy .NET 4.5 przyjaznych dla urządzeń przenośnych.

## <a name="overview"></a>Omówienie

Ten dokument wprowadza nowe async i await słowa kluczowe, a następnie przeszukiwań za pośrednictwem przykłady proste wdrożenie metod asynchronicznych w Xamarin.iOS i platformy Xamarin.Android.

Bardziej szczegółowe omówienie nowych funkcji asynchronicznych języka C# 5 (w tym wiele przykłady i scenariusze użycia różnych) można znaleźć w dokumentacji MSDN [programowanie asynchroniczne z Async i Await](http://msdn.microsoft.com/en-us/library/vstudio/hh191443.aspx).

Przykładowa aplikacja wysyła żądanie proste asynchroniczne sieci web (bez blokowania wątku głównego), a następnie aktualizuje interfejsu użytkownika przy użyciu pobranego html i liczba znaków.

 [![](async-images/AsyncAwait_427x368.png "Przykładowa aplikacja wysyła żądanie simple web asynchroniczne bez blokowania wątku głównego, a następnie aktualizuje interfejsu użytkownika przy użyciu pobranego html i liczba znaków")](async-images/AsyncAwait.png#lightbox)

Obsługa asynchronicznego dla platformy Xamarin jest oparty na podstawie Mono 3.0 i uaktualnia profilu interfejsu API jest przyjazna wersji Silverlight, aby być w wersji platformy .NET 4.5 przyjaznych dla urządzeń przenośnych.

## <a name="requirements"></a>Wymagania

Funkcje języka C# 5 wymagają Mono 3.0 jest dołączony do platformy Xamarin.iOS 6.4 i 4.8 platformy Xamarin.Android. Pojawi się monit do uaktualnienia programu Mono, Xamarin.iOS, Xamarin.Android i Xamarin.Mac mógł korzystać z niego.

## <a name="using-async-amp-await"></a>Użycie async &amp; await

 `async` i `await` są nowe C# języka funkcje, które działają w połączeniu z Biblioteka zadań równoległych ułatwia pisanie kodu wątków do wykonywania długotrwałych zadań bez blokowania wątku głównego aplikacji.

## <a name="async"></a>async

### <a name="declaration"></a>Deklaracja

`async` — Słowo kluczowe jest umieszczona w deklaracji metody (lub na lambda ani metoda anonimowa) wskazująca, że zawiera on kod, który może zostać wykonany asynchronicznie, tj. nie blokuj wątków obiektu wywołującego.

Metody oznaczone `async` powinien zawierać co najmniej jeden await wyrażenia lub instrukcji. Jeśli nie `await`s znajdują się w metodzie, a następnie będzie wykonywana synchronicznie (takie same jak w przypadku nie `async` modyfikator). Spowoduje to również w ostrzeżenie kompilatora (ale nie jest błąd).

### <a name="return-types"></a>Typy zwracane

Metoda async powinna zwrócić `Task`, `Task<TResult>` lub `void`.

Określ `Task` typ zwracany, jeśli metoda nie zwraca wszelkie inne wartości.

Określ `Task<TResult>` Jeśli metoda musi zwracać wartość, której `TResult` jest zwracany typ (takich jak `int`, na przykład).

`void` Zwracany typ jest używany głównie do obsługi zdarzeń, które tego wymagają. Kod, który wywołuje metod asynchronicznych zwracające typ void nie `await` na wynik.

### <a name="parameters"></a>Parametry

Metody asynchroniczne nie mogą deklarować `ref` lub `out` parametrów.

## <a name="await"></a>await

Operatora await można zastosować do zadania wewnątrz metody oznaczone jako asynchroniczny. Powoduje metodę, aby zatrzymać wykonanie w tym momencie i poczekaj na zakończenie zadania.

Przy użyciu await nie blokuje wątków obiektu wywołującego — zamiast zwróceniem sterowania do obiektu wywołującego. Oznacza to, że wątek wywołujący nie jest zablokowany, aby na przykład użytkownik wątku interfejsu może nie być blokowane podczas oczekiwanie na zadanie.

Po zakończeniu zadania, metoda wznawia wykonywanie w tym samym punkcie w kodzie. Dotyczy to również, wracając do zakresu try blok try-catch-finally (jeśli istnieje). await nie można użyć w instrukcji catch lub bloku finally.

Przeczytaj więcej na temat [await w witrynie MSDN](http://msdn.microsoft.com/en-us/library/vstudio/hh156528.aspx).

## <a name="exception-handling"></a>Obsługa wyjątków

Wyjątki, które występuje wewnątrz metody asynchronicznej są przechowywane w zadaniu i generowany, gdy zadanie jest `await`ed. Wyjątki te można przechwycony i obsługiwane wewnątrz bloku try-catch.

## <a name="cancellation"></a>Anulowanie

Metody asynchroniczne, które trwać bardzo długo powinien obsługiwać anulowania. Zazwyczaj anulowania jest wywoływana w następujący sposób:

- A `CancellationTokenSource` tworzony jest obiekt.
- `CancellationTokenSource.Token` Wystąpienia są przekazywane do możliwe metody asynchronicznej.
- Zażądano anulowania przez wywołanie metody `CancellationTokenSource.Cancel` metody.

Następnie zadanie anulowany automatycznie i potwierdza anulowania.

Aby uzyskać więcej informacji na temat anulowania, zobacz [jak anulować zadanie asynchroniczne](http://msdn.microsoft.com/en-us/library/vstudio/jj155761.aspx) w witrynie MSDN.

## <a name="example"></a>Przykład

Pobierz [przykład rozwiązania Xamarin](https://developer.xamarin.com/samples/mobile/AsyncAwait/) (dla systemu iOS i Android). Aby wyświetlić przykład pracy `async` i `await` w aplikacjach mobilnych. Przykładowy kod jest szczegółowo opisane w tej sekcji.

### <a name="writing-an-async-method"></a>Pisanie metody asynchronicznej

Następującą metodę pokazano, jak kod `async` metody z `await`ed zadań:

```csharp
public async Task<int> DownloadHomepage()
{
    var httpClient = new HttpClient(); // Xamarin supports HttpClient!

    Task<string> contentsTask = httpClient.GetStringAsync("http://xamarin.com"); // async method!

    // await! control returns to the caller and the task continues to run on another thread
    string contents = await contentsTask;

    ResultEditText.Text += "DownloadHomepage method continues after async call. . . . .\n";

    // After contentTask completes, you can calculate the length of the string.
    int exampleInt = contents.Length;

    ResultEditText.Text += "Downloaded the html and found out the length.\n\n\n";

    ResultEditText.Text += contents; // just dump the entire HTML

    return exampleInt; // Task<TResult> returns an object of type TResult, in this case int
}
```

Należy uwzględnić następujące punkty:

-  Deklaracja metody zawiera `async` — słowo kluczowe.
-  Typ zwracany jest `Task<int>` wywołanie kodu dostępu do `int` wartość, która jest obliczana w ramach tej metody.
-  Instrukcja return jest `return exampleInt;` która jest obiektem całkowitą — fakt, że metoda zwraca `Task<int>` jest częścią ulepszenia języka.


### <a name="calling-an-async-method-1"></a>Wywoływanie metody asynchronicznej 1

Kliknij ten przycisk, zdarzenie obsługi można znaleźć w Android przykładowej aplikacji do wywoływania metody opisanych wyżej:

```csharp
GetButton.Click += async (sender, e) => {

    Task<int> sizeTask = DownloadHomepage();

    ResultTextView.Text = "loading...";
    ResultEditText.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await sizeTask;

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultTextView.Text = "Length: " + intResult ;
    // "returns" void, since it's an event handler
};
```

Uwagi:

-  Anonimowe pełnomocnika prefiks async — słowo kluczowe.
-  Metoda asynchroniczna DownloadHomepage zwraca klasę Task, <int> jest przechowywana w zmiennej sizeTask.
-  Kod oczekiwanie na sizeTask zmiennej.  *To* jest lokalizację, która metoda jest wstrzymana i zwróceniem sterowania do kodu wywołującego, zakończenie zadania asynchronicznego w jego własnym wątku.
-  Wykonanie jest *nie* wstrzymać, gdy zadanie zostanie utworzone w pierwszym wierszu metody, niezależnie od zadania tworzona istnieje. Słowo kluczowe await oznacza, że lokalizacja, w której wykonywanie jest wstrzymane.
-  Po zakończeniu zadania asynchronicznego intResult jest ustawiona, i kontynuuje wykonywanie w oryginalnym wątku, w wierszu await.


### <a name="calling-an-async-method-2"></a>Wywoływanie metody asynchronicznej 2

W przykładowej aplikacji systemu iOS jest napisany nieco inaczej aby zademonstrować o innym podejściu. Zamiast niż używanie delegata anonimowy w tym przykładzie deklaruje `async` obsługi zdarzeń, który jest przypisany, takich jak program obsługi zdarzeń regularne:

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

Metoda obsługi zdarzeń jest następnie zdefiniowane, jak pokazano poniżej:

```csharp
async void HandleTouchUpInside (object sender, EventArgs e)
{
    ResultLabel.Text = "loading...";
    ResultTextView.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await DownloadHomepage();

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultLabel.Text = "Length: " + intResult ;
}
```

Kilka ważnych kwestii:

-  Metoda jest oznaczona jako `async` , ale zwraca `void` . To jest zazwyczaj wykonywane tylko dla programów obsługi zdarzeń (w przeciwnym razie zwróci `Task` lub `Task<TResult>` ).
-  Kod `await` s na `DownloadHomepage` metody bezpośrednio na przypisanie do zmiennej ( `intResult` ) inaczej niż w poprzednim przykładzie, gdzie użyliśmy pośredni `Task<int>` zmienną, aby odwołać się do zadania.  *To* to lokalizacja, w którym zwróceniem sterowania obiektowi wywołującemu, dopiero po ukończeniu metody asynchronicznej w innym wątku.
-  Gdy metoda asynchroniczna kończy pracę i zwraca, wykonanie wznawia działanie w `await` oznacza to całkowitą wynik został zwrócony, a następnie renderowane w widget interfejsu użytkownika.


## <a name="summary"></a>Podsumowanie

Za pomocą async i await jest znacznie ułatwione kod wymagany do zduplikować długotrwałe operacje na wątki w tle bez blokowania głównego wątku. One również ułatwiają dostęp wyniki, gdy zadanie zostało ukończone.

Tego dokumentu została podana omówienie nowych słów kluczowych języka i przykłady dla platformy Xamarin.Android i Xamarin.iOS.



## <a name="related-links"></a>Linki pokrewne

- [AsyncAwait (przykład)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [Wywołania zwrotne jako naszych generacje przejdź do — instrukcja](http://tirania.org/blog/archive/2013/Aug-15.html)
- [Dane (iOS) (przykład)](https://developer.xamarin.com/samples/monotouch/Data/)
- [HttpClient (iOS) (przykład)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
- [MapKitSearch (iOS) (przykład)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [Webinar: C# asynchronicznych w systemach iOS i Android (klip wideo)](http://xamarin.wistia.com/medias/k27mc627xz)
- [Programowanie asynchroniczne z Async i Await (MSDN)](http://msdn.microsoft.com/en-us/library/vstudio/hh191443.aspx)
- [Szczegółowe Dostrajanie aplikacji Async (MSDN)](http://msdn.microsoft.com/en-us/library/vstudio/jj155761.aspx)
- [Operator await i interfejsu użytkownika i zakleszczenie! Niestety Moje! (MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2011/01/13/10115163.aspx)
- [Przetwarzanie zadań w chwili zakończenia (MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2012/08/02/processing-tasks-as-they-complete.aspx)
- [Wzorzec asynchroniczny oparty na zadaniach (TAP)](http://msdn.microsoft.com/en-us/library/hh873175.aspx)
- [Asynchrony w języku C# 5 (blog Lippert marek) — o wprowadzeniu słowa kluczowe](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
