---
title: Podsumowanie rozdziałów 20. Async i plik we/wy
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie działu 20. Async i plik we/wy'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: charlespetzold
ms.author: chape
ms.date: 07/18/2018
ms.openlocfilehash: d606432174807498fd458470647109de4fa0b6b4
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156733"
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>Podsumowanie rozdziałów 20. Async i plik we/wy

> [!NOTE] 
> Uwagi na tej stronie wskazać obszary, w którym Xamarin.Forms podzielił z materiału znajdujące się w książce.

 Graficzny interfejs użytkownika musi odpowiadać na zdarzenia w danych wejściowych użytkownika po kolei. Oznacza to, że całego procesu przetwarzania zdarzeń, dane wejściowe użytkownika musi wystąpić w pojedynczych wątkach, często nazywane *wątku głównego* lub *wątku interfejsu użytkownika*.

Użytkownicy oczekują graficznego interfejsu użytkownika do szybkość reakcji. Oznacza to, że program musi szybko przetwarzania zdarzeń danych wejściowych użytkownika. Jeśli nie jest to możliwe, następnie przetwarzania musi być były odpowiedzialne pomocnicze wątki wykonywania.

Użyto kilka przykładowych programów w tej książce [ `WebRequest` ](xref:System.Net.WebRequest) klasy. W tej klasie [ `BeginGetReponse` ](xref:System.Net.WebRequest.BeginGetResponse(System.AsyncCallback,System.Object)) metoda uruchamia wątku roboczego, która wywołuje funkcję wywołania zwrotnego po jego zakończeniu. Jednak tej funkcji wywołania zwrotnego działa w wątku roboczego, dzięki czemu program musi wywoływać [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) metoda uzyskać dostępu do interfejsu użytkownika.

> [!NOTE]
> Należy użyć zestawu narzędzi Xamarin.Forms programy [ `HttpClient` ](xref:System.Net.Http.HttpClient) zamiast [ `WebRequest` ](xref:System.Net.WebRequest) do uzyskiwania dostępu do plików za pośrednictwem Internetu. `HttpClient` obsługuje operacje asynchroniczne.

Bardziej nowoczesnego podejścia do przetwarzania asynchronicznego jest dostępna w .NET i języka C#. Obejmuje to [ `Task` ](xref:System.Threading.Tasks.Task) i [ `Task<TResult>` ](xref:System.Threading.Tasks.Task`1) klasy i inne typy w [ `System.Threading` ](xref:System.Threading) i [ `System.Threading.Tasks` ](xref:System.Threading.Tasks) przestrzeni nazw, a także 5.0 C# `async` i `await` słów kluczowych. To, co w tym rozdziale koncentruje się na.

## <a name="from-callbacks-to-await"></a>Z wywołań zwrotnych oczekują na

`Page` Samej klasy zawiera trzy metod asynchronicznych, aby wyświetlić pola alertu:

- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) Zwraca `Task` obiektu
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) Zwraca `Task<bool>` obiektu
- [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) Zwraca `Task<string>` obiektu

`Task` Obiektów wskazują, że te metody wdrożenia opartego na zadaniach wzorca asynchronicznego, znane jako wzorca TAP. Te `Task` obiekty są szybko zwracane przez metodę. `Task<T>` Zwracają wartości stanowią element "promise" wartości typu `TResult` będą dostępne po zakończeniu zadania. `Task` Zwracana wartość wskazuje akcję asynchroniczną tego zostanie zwrócony pełną ale bez wartości.

W takich przypadkach `Task` została ukończona, gdy użytkownik odrzuci polu alertu.  

### <a name="an-alert-with-callbacks"></a>Alert o wywołania zwrotne

[ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) przykład pokazuje, jak obsługiwać `Task<bool>` zwracać obiekty i `Device.BeginInvokeOnMainThread` wywołań przy użyciu metod wywołania zwrotnego.

### <a name="an-alert-with-lambdas"></a>Alert z wyrażenia lambda

[ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) przykładowych pokazano, jak do obsługi funkcji lambda anonimowe `Task` i `Device.BeginInvokeOnMainThread` wywołania.  

### <a name="an-alert-with-await"></a>Alert o await

Prostsze strategia polega na `async` i `await` słowa kluczowe w języku C# 5. [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) w przykładzie pokazano ich użycie.

### <a name="an-alert-with-nothing"></a>Alert o nic

Jeśli metoda asynchroniczna zwraca `Task` zamiast `Task<TResult>`, a następnie program nie musi używać dowolną z poniższych metod, jeśli nie musi wiedzieć, po zakończeniu zadania asynchronicznego. [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) przykład pokazuje to.

### <a name="saving-program-settings-asynchronously"></a>Zapisywanie ustawień programu asynchronicznie

[ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) przykład demonstruje użycie [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) metody `Application` można zapisać ustawień programu w miarę jak ulegają zmianom, bez Zastępowanie `OnSleep` metody.

### <a name="a-platform-independent-timer"></a>Czasomierz niezależne od platformy

Istnieje możliwość użycia [ `Task.Delay` ](xref:System.Threading.Tasks.Task.Delay(System.Int32)) do utworzenia czasomierza niezależne od platformy. [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) przykład pokazuje to.

## <a name="file-inputoutput"></a>Plik wejścia/wyjścia

Tradycyjnie .NET [ `System.IO` ](xref:System.IO) przestrzeń nazw została źródła obsługi We/Wy pliku. Mimo że niektóre metody w tej przestrzeni nazw obsługują operacji asynchronicznych, nie są najczęściej. Przestrzeń nazw obsługuje również kilka wywołań prostej metody, które wykonują funkcje We/Wy pliku zaawansowane.

### <a name="good-news-and-bad-news"></a>Dobra wiadomość i złe wiadomości

Wszystkie platformy obsługiwane przez lokalny magazyn aplikacji obsługę zestawu narzędzi Xamarin.Forms &mdash; magazynu, który jest prywatny do aplikacji.

Biblioteki interfejsu Xamarin.iOS i Xamarin.Android obejmują wersję platformy .NET, który Xamarin ma wyraźnie zoptymalizowanych pod kątem tych dwóch platform. Należą do klasy z `System.IO` służące do wykonywania operacji We/Wy pliku za pomocą magazynu lokalnego dla aplikacji w tych dwóch platform.

Jednak w przypadku wyszukiwania dla tych `System.IO` klas w aplikacji PCL Xamarin.Forms, użytkownik nie będzie je znaleźć. Problem polega na ten plik Microsoft całkowicie odnowionych operacji We/Wy dla interfejsu API środowiska wykonawczego Windows. Nie należy używać programy przeznaczone dla Windows 8.1, Windows Phone 8.1 i platformy uniwersalnej Windows `System.IO` dla we/wy pliku.

Oznacza to, że będziesz musiał użyć [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) (najpierw omówione w [ **rozdział 9. Wywołania interfejsu API specyficzne dla platformy** ](chapter09.md) do zaimplementowania we/wy.

> [!NOTE]
> Przenośne bibliotekami klasy zostało zastąpione przy użyciu biblioteki .NET Standard 2.0 i .NET Standard 2.0 obsługuje [ `System.IO` ](xref:System.IO) typów dla wszystkich platform zestawu narzędzi Xamarin.Forms. Nie jest już używać `DependencyService` do wykonywania większości zadań we/wy pliku. Zobacz [Obsługa plików w interfejsie Xamarin.Forms](~/xamarin-forms/app-fundamentals/files.md) nowocześniejszych podejścia do We/Wy pliku.

### <a name="a-first-shot-at-cross-platform-file-io"></a>Pierwszy zrzut na We/Wy plików dla wielu platform

[ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) Przykładowa aplikacja definiuje [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) interfejs dla We/Wy i implementacje tego interfejsu na wszystkich platformach. Jednak implementacji środowiska uruchomieniowego Windows nie działają z metod opisanych w tym interfejsie, ponieważ metody operacji We/Wy plików środowiska uruchomieniowego Windows są asynchroniczne.

### <a name="accommodating-windows-runtime-file-io"></a>Obsługa środowiska uruchomieniowego Windows we/wy

Programy działających w ramach środowiska uruchomieniowego Windows użyć klas w [ `Windows.Storage` ](/uwp/api/Windows.Storage) i [ `Windows.Storage.Streams` ](/uwp/api/Windows.Storage.Streams) przestrzeni nazw dla pliku operacji We/Wy, łącznie z lokalnego magazynu aplikacji. Ponieważ Microsoft ustali, że wszelkie operacje, wymaga więcej niż 50 MS powinien być asynchroniczne w celu unikania blokowania wątku interfejsu użytkownika, te metody operacji We/Wy pliku przede wszystkim są asynchroniczne.

Kod, demonstrując to nowe podejście będzie w bibliotece, dzięki czemu mogą być używane przez inne aplikacje.

## <a name="platform-specific-libraries"></a>Biblioteki charakterystyczne dla platformy

Jest korzystne do przechowywania kodu wielokrotnego użytku w bibliotekach. Jest to oczywiście trudniejsze, w przypadku różnych rodzajów kodu wielokrotnego użytku dla całkowicie różnych systemów operacyjnych.

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) rozwiązanie przedstawia jedno z podejść. To rozwiązanie zawiera siedem różnych projektów:

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), normalne PCL zestawu narzędzi Xamarin.Forms
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), biblioteka klas systemu iOS
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), biblioteka klas systemu Android
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), biblioteki klas Windows Universal
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), Współdzielony projekt dla kodu, który jest wspólne dla wszystkich platform Windows

Wszystkie projekty poszczególnych platform (z wyjątkiem produktów **Xamarin.FormsBook.Platform.WinRT**) odwołują się do **Xamarin.FormsBook.Platform**. Trzy projekty Windows odwoływałby się do **Xamarin.FormsBook.Platform.WinRT**.

Wszystkie projekty zawierają statycznego `Toolkit.Init` metodę, aby upewnić się, że biblioteki jest ładowany, jeśli nie jest bezpośrednio wywoływany przez projektu w rozwiązaniu aplikacji platformy Xamarin.Forms.

**Xamarin.FormsBook.Platform** projektu zawiera nowe [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) interfejsu. Wszystkie metody mają teraz nazwy w `Async` sufiksy i zwrócenie `Task` obiektów.

**Xamarin.FormsBook.Platform.WinRT** projekt zawiera [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) klasy dla środowiska wykonawczego Windows.

**Xamarin.FormsBook.Platform.iOS** projekt zawiera [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) klasy dla systemu iOS. Te metody teraz musi być asynchroniczne. Niektóre metody używaj wersji asynchronicznej metody zdefiniowane w `StreamWriter` i `StreamReader`: [ `WriteAsync` ](xref:System.IO.StreamWriter.WriteAsync(System.String)) i [ `ReadToEndAsync` ](xref:System.IO.StreamReader.ReadToEndAsync). Inne przekonwertować wynik w celu `Task` przy użyciu [ `FromResult` ](xref:System.Threading.Tasks.Task.FromResult*) metody.

**Xamarin.FormsBook.Platform.Android** projekt zawiera podobne [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) klasy dla systemu Android.

**Xamarin.FormsBook.Platform** zawiera także projekt [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) klasę, która ułatwia korzystanie z `DependencyService` obiektu.

Aby korzystać z tych bibliotek, rozwiązanie aplikacji musi zawierać wszystkie projekty w **Xamarin.FormsBook.Platform** rozwiązania, a każdy z projektów aplikacji muszą mieć odniesienie do odpowiedniej biblioteki w  **Xamarin.FormsBook.Platform**.

[ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) rozwiązanie pokazuje, jak używać **Xamarin.FormsBook.Platform** bibliotek. Każdy z projektów ma wywołanie `Toolkit.Init`. Aplikacja korzysta z asynchronicznego We/Wy funkcji.

### <a name="keeping-it-in-the-background"></a>Utrzymywanie jej w tle

Metody w bibliotekach, które są wybierane wiele metod asynchronicznych &mdash; takich jak `WriteFileAsync` i `ReadFileASync` metod środowiska wykonawczego Windows [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) klasy &mdash; może się nieco bardziej efektywne, za pomocą [ `ConfigureAwait` ](xref:System.Threading.Tasks.Task`1.ConfigureAwait(System.Boolean)) metody w celu uniknięcia przełączanie z powrotem do wątku interfejsu użytkownika.

### <a name="dont-block-the-ui-thread"></a>Nie blokuj wątek interfejsu użytkownika!

Czasami jest kuszące, aby uniknąć użycia `ContinueWith` lub `await` przy użyciu [ `Result` ](xref:System.Threading.Tasks.Task`1.Result) właściwość metody. Należy tego unikać mogą zablokować wątek interfejsu użytkownika, lub nawet zawieszanie aplikacji.

## <a name="your-own-awaitable-methods"></a>Oczekujący metody

Uruchomienie kodu asynchronicznie przez przekazanie jej do jednego z [ `Task.Run` ](xref:System.Threading.Tasks.Task.Run(System.Action)) metody. Możesz wywołać `Task.Run` w metodzie asynchronicznej, która obsługuje niektóre obciążenie.

Różne `Task.Run` wzorców omówiono poniżej.

### <a name="the-basic-mandelbrot-set"></a>Podstawowy zestaw Mandelbrot

Aby narysować Mandelbrot w czasie rzeczywistym [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka ma [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) struktury podobne do pokazanego na `System.Numerics` przestrzeń nazw.

[ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) przykład ma `CalculateMandeblotAsync` metody w jego pliku związanym z kodem, który oblicza podstawowego czarno zestawu Mandelbrot i używa [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)go umieścić na mapę bitową.

### <a name="marking-progress"></a>Oznaczanie postępu

Aby raport postęp na podstawie metody asynchronicznej można utworzyć wystąpienie [ `Progress<T>` ](xref:System.Progress`1) klasy i zdefiniować swoje metodzie asynchronicznej, aby mieć argumentu typu [ `IProgress<T>` ](xref:System.IProgress`1). Jest to zaprezentowane w [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) próbki.

### <a name="cancelling-the-job"></a>Anulowanie zadania

Można także napisać metodę asynchroniczną za możliwe. Rozpocznij przy użyciu klasy o nazwie [ `CancellationTokenSource` ](xref:System.Threading.CancellationTokenSource). [ `Token` ](xref:System.Threading.CancellationTokenSource.Token) Właściwość jest wartością typu [ `CancellationToken` ](xref:System.Threading.CancellationToken). To jest przekazywane do funkcji asynchronicznej. Wywołuje program [ `Cancel` ](xref:System.Threading.CancellationTokenSource.Cancel) metody `CancellationTokenSource` (zwykle w odpowiedzi na akcję przez użytkownika) do anulowania funkcji asynchronicznej.

Metoda asynchroniczna może okresowo sprawdzać [ `IsCancellationRequested` ](xref:System.Threading.CancellationToken.IsCancellationRequested) właściwość `CancellationToken` i zamknąć okno, jeśli właściwość jest `true`, lub po prostu Wywołaj [ `ThrowIfCancellationRequested` ](xref:System.Threading.CancellationToken.ThrowIfCancellationRequested) metody w tym przypadku metoda kończy się [ `OperationCancelledException` ](xref:System.OperationCanceledException).

[ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) w przykładzie pokazano użycie funkcji można anulować.

### <a name="an-mvvm-mandelbrot"></a>Mandelbrot MVVM

[ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) przykładowe ma bardziej rozległe interfejsu użytkownika, a przede wszystkim opiera się na [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) i [ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)klasy:

[![Potrójna zrzut ekranu przedstawiający Mandelbrot X F](images/ch20fg13-small.png "MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "MVVM Mandelbrot")

## <a name="back-to-the-web"></a>Powrót do sieci web

[ `WebRequest` ](xref:System.Net.WebRequest) Klasa używana w kilka przykładów używa protokołu stara asynchronicznego modelu programowania asynchronicznego lub APM. Taka klasa można przekonwertować na nowoczesnych protokołu wzorca TAP przy użyciu jednej z `FromAsync` metody [ `TaskFactory` ](xref:System.Threading.Tasks.TaskFactory`1) klasy. [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) przykład pokazuje to.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 20 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [Przykłady działu 20](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [Praca z plikami](~/xamarin-forms/app-fundamentals/files.md)
