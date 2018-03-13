---
title: "Podsumowanie działu 20. We/Wy Async i plików"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 86ae56fc2baac3eab0fbf375c5f67f7b2327721a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>Podsumowanie działu 20. We/Wy Async i plików

 Graficzny interfejs użytkownika musi odpowiedzieć na dane wejściowe użytkownika zdarzenia sekwencyjnie. Oznacza to, że przetwarzania danych wejściowych użytkownika zdarzenia musi występować w jednym wątku, często nazywane *wątku głównego* lub *wątku interfejsu użytkownika*.

Użytkownicy oczekują graficzne interfejsy użytkownika do reagować. Oznacza to, że program musi szybko przetworzyć zdarzenia dane wejściowe użytkownika. Jeśli nie jest to możliwe, następnie przetwarzania musi być były odpowiedzialne dodatkowej wątków wykonywania.

Kilka przykładowych programów w tej sekcji zostały użyte [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) klasy. W tej klasie [ `BeginGetReponse` ](https://developer.xamarin.com/api/member/System.Net.WebRequest.BeginGetResponse/p/System.AsyncCallback/System.Object/) metody uruchamiania wątku roboczego, który wywołuje funkcję wywołania zwrotnego, po jego zakończeniu. Jednak ta funkcja wywołania zwrotnego działa w wątku roboczego, więc program należy wywołać [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) metody uzyskać dostępu do interfejsu użytkownika.

Nowoczesną podejście do asynchronicznego przetwarzania jest dostępny w .NET i C#. Obejmuje to [ `Task` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task/) i [ `Task<TResult>` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task%3CTResult%3E/) klasy i innych typów w [ `System.Threading` ](https://developer.xamarin.com/api/namespace/System.Threading/) i [ `System.Threading.Tasks` ](https://developer.xamarin.com/api/namespace/System.Threading.Tasks/) przestrzeni nazw, a także 5.0 C# `async` i `await` słów kluczowych. To, co w tym rozdziale skupiono się na.

## <a name="from-callbacks-to-await"></a>Z wywołań zwrotnych await

`Page` Samej klasy zawiera trzy metody asynchroniczne, aby wyświetlić pola alertu:

- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) Zwraca `Task` obiektu
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/System.String/) Zwraca `Task<bool>` obiektu
- [`DisplayActionSheet`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) Zwraca `Task<string>` obiektu

`Task` Obiektów wskazują, że te metody wdrożenia opartego na zadaniach wzorca asynchronicznego, znany jako NACIŚNIJ. Te `Task` obiekty są szybko zwrócona przez metodę. `Task<T>` Zwracać wartości stanowią "promise" na wartość typu `TResult` będą dostępne po zakończeniu zadania. `Task` Zwracana wartość wskazuje akcja asynchroniczna tego zostanie zwrócony pełną ale bez wartości.

W takich przypadkach `Task` została ukończona, gdy użytkownik odrzuci pole alertu.  

### <a name="an-alert-with-callbacks"></a>Alert o wywołań zwrotnych

[ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) przykładzie pokazano sposób obsługi `Task<bool>` zwracać obiekty i `Device.BeginInvokeOnMainThread` połączenia za pomocą metody wywołania zwrotnego.

### <a name="an-alert-with-lambdas"></a>Alert o wyrażeń lambda

[ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) przykładzie pokazano, jak do obsługi funkcji anonimowych lambda `Task` i `Device.BeginInvokeOnMainThread` wywołania.  

### <a name="an-alert-with-await"></a>Alert o await

Obejmuje więcej prosta metoda `async` i `await` wprowadzono w języku C# 5 słów kluczowych. [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) przykładzie pokazano ich używania.

### <a name="an-alert-with-nothing"></a>Alert o nic

Jeśli metoda asynchroniczna zwraca `Task` zamiast `Task<TResult>`, a następnie program nie musi używać dowolną z tych metod, jeśli nie musi wiedzieć, po zakończeniu wykonywania zadania asynchronicznego. [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) przykładzie pokazano to.

### <a name="saving-program-settings-asynchronously"></a>Zapisywanie ustawień programu asynchronicznie

[ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) przykładzie przedstawiono użycie [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) metody `Application` można zapisać ustawień programu, ponieważ mogą one zmianie bez Zastępowanie `OnSleep` metody.

### <a name="a-platform-independent-timer"></a>Czasomierz niezależne od platformy

Istnieje możliwość użycia [ `Task.Delay` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Delay/p/System.Int32/) do utworzenia czasomierza niezależne od platformy. [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) przykładzie pokazano to.

## <a name="file-inputoutput"></a>Plik danych wejściowych/wyjściowych

Tradycyjnie .NET [ `System.IO` ](https://developer.xamarin.com/api/namespace/System.IO/) przestrzeni nazw został źródła obsługi We/Wy pliku. Mimo że niektóre metody w tej przestrzeni nazw obsługują operacje asynchroniczne, większość nie. Przestrzeń nazw obsługuje również kilka wywołań prosta metoda wykonujące funkcje We/Wy pliku zaawansowane.

### <a name="good-news-and-bad-news"></a>Dobre wieści i złych wiadomości

Wszystkie platformy obsługiwane przez Magazyn lokalny aplikacji platformy Xamarin.Forms Obsługa & #x 2014; pamięć masowa jest prywatny do aplikacji.

Biblioteki Xamarin.iOS i platformy Xamarin.Android zawierają wersję platformy .NET, który Xamarin ma wyraźnie zoptymalizowanych pod kątem te dwie platformy. Obejmują one klas z `System.IO` można wykonać operacji We/Wy pliku z magazynu lokalnego aplikacji w tych dwóch platform.

Jednak w przypadku tych `System.IO` klas w PCL platformy Xamarin.Forms, użytkownik nie będzie je znaleźć. Problem jest ten plik Microsoft utworzona całkowicie od nowa we/wy dla interfejsu API środowiska wykonawczego systemu Windows. Nie należy używać programów przeznaczonych dla platformy uniwersalnej systemu Windows, Windows 8.1 i Windows Phone 8.1 `System.IO` dla we/wy pliku.

Oznacza to, że należy używać [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) (najpierw omówione w [ **rozdział 9. Wywołania API specyficzne dla platformy** ](chapter09.md) do zaimplementowania We/Wy plików.

### <a name="a-first-shot-at-cross-platform-file-io"></a>Pierwszy zrzut wieloplatformowych plik we/wy

[ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) definiuje próbki [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) interfejs We/Wy plików i implementacje tego interfejsu na wszystkich platformach. Jednak implementacji środowiska wykonawczego systemu Windows nie działają z metod opisanych w tym interfejsie, ponieważ asynchroniczne We/Wy pliku środowiska wykonawczego systemu Windows metody.

### <a name="accommodating-windows-runtime-file-io"></a>Obsługa środowiska uruchomieniowego systemu Windows plik we/wy

Programy do uruchamiania środowiska uruchomieniowego systemu Windows używają klas w [ `Windows.Storage` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.aspx) i [ `Windows.Storage.Streams` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.aspx) przestrzeni nazw dla we/wy magazynu lokalnego aplikacji w tym pliku. Ponieważ Microsoft ustalić, który żadnych operacji wymagających więcej niż 50 milisekund powinien być asynchroniczne w celu unikania blokowania wątku interfejsu użytkownika, te metody we/wy pliku najczęściej są asynchroniczne.

Prezentacja to nowe podejście kod będzie w bibliotece, dzięki czemu mogą być używane przez inne aplikacje.

## <a name="platform-specific-libraries"></a>Biblioteki specyficzne dla platformy

Jest korzystne do przechowywania do ponownego użycia kodu w bibliotekach. Jest to zdecydowanie trudniejsze w przypadku różnych fragmentów kodu do ponownego użycia w całkowicie różnych systemach operacyjnych.

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) rozwiązanie przedstawia jeden z nich. To rozwiązanie zawiera siedmiu różnych projektów:

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), normalne PCL platformy Xamarin.Forms
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), biblioteki klas z systemem iOS
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), biblioteki klas dla systemu Android
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), biblioteki klas uniwersalnych systemu Windows
- [**Xamarin.FormsBook.Platform.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Windows), PCL dla Windows 8.1.
- [**Xamarin.FormsBook.Platform.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinPhone), PCL dla Windows Phone 8.1
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), projektu udostępnionego dla kodu, który jest wspólny dla wszystkich platform Windows

Wszystkie projekty poszczególnych platform (z wyjątkiem produktów **Xamarin.FormsBook.Platform.WinRT**) odwołują się do **Xamarin.FormsBook.Platform**. Trzy projekty systemu Windows zawierają odwołanie do **Xamarin.FormsBook.Platform.WinRT**.

Wszystkie projekty zawierają statycznego `Toolkit.Init` metody, aby upewnić się, że biblioteka jest załadowany, jeśli nie jest bezpośrednio wywoływany przez projekt w rozwiązaniu aplikacji platformy Xamarin.Forms.

**Xamarin.FormsBook.Platform** projekt zawiera nowe [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) interfejsu. Wszystkie metody mają teraz nazwy w `Async` sufiksy i przywracać `Task` obiektów.

**Xamarin.FormsBook.Platform.WinRT** projekt zawiera [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) klasy dla środowiska uruchomieniowego systemu Windows.

**Xamarin.FormsBook.Platform.iOS** projekt zawiera [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) klasy dla systemu iOS. Te metody teraz musi być asynchroniczne. Użyj niektórych metod asynchronicznych wersji metody zdefiniowane w `StreamWriter` i `StreamReader`: [ `WriteAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamWriter.WriteAsync/p/System.String/) i [ `ReadToEndAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamReader.ReadToEndAsync()/). Konwertuj innym wynik w celu `Task` przy użyciu [ `FromResult` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.FromResult%7BTResult%7D/p/TResult/) metody.

**Xamarin.FormsBook.Platform.Android** projekt zawiera podobne [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) klasy dla systemu Android.

**Xamarin.FormsBook.Platform** projektu zawiera także [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) klasy, która ułatwia użycie `DependencyService` obiektu.

Aby korzystać z tych bibliotek, rozwiązanie aplikacji musi zawierać wszystkie projekty w **Xamarin.FormsBook.Platform** rozwiązania, a każdy z projektów aplikacji musi mieć odwołanie do odpowiedniego biblioteki w  **Xamarin.FormsBook.Platform**.

[ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) rozwiązanie przedstawia sposób użycia **Xamarin.FormsBook.Platform** biblioteki. Każdy z projektów ma wywołania `Toolkit.Init`. Aplikacja korzysta z pliku asynchroniczne We/Wy funkcji.

### <a name="keeping-it-in-the-background"></a>Utrzymywanie go w tle

Metody w bibliotekach, które wykonywania wywołań do wielu metod asynchronicznych & #x 2014; takie jak `WriteFileAsync` i `ReadFileASync` metody w środowisku wykonawczym systemu Windows [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) klasy & #x 2014; może się nieco bardziej wydajne przy użyciu [ `ConfigureAwait` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task%3CTResult%3E.ConfigureAwait/p/System.Boolean/) metody Unikaj przełączanie z powrotem do wątku interfejsu użytkownika.

### <a name="dont-block-the-ui-thread"></a>Nie należy blokować wątku interfejsu użytkownika!

Czasami jest kuszące, aby uniknąć użycia `ContinueWith` lub `await` za pomocą [ `Result` ](https://developer.xamarin.com/api/property/System.Threading.Tasks.Task%3CTResult%3E.Result/) właściwości metody. To należy unikać można zablokować wątku interfejsu użytkownika, lub nawet zawieszenie aplikacji.

## <a name="your-own-awaitable-methods"></a>Metody oczekujący

Uruchomienie kodu asynchronicznie przez przekazanie jej do jednego z [ `Task.Run` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Run/p/System.Action/) metody. Możesz wywołać `Task.Run` wewnątrz metody asynchronicznej obsługujący niektóre obciążenie.

Różnych `Task.Run` wzorce omówiono poniżej.

### <a name="the-basic-mandelbrot-set"></a>Podstawowy zestaw Mandelbrot

Rysowanie Mandelbrot w czasie rzeczywistym [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka ma [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) struktury podobny do przedstawionego w `System.Numerics` przestrzeń nazw.

[ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) próbki `CalculateMandeblotAsync` w jego pliku związanym z kodem, który oblicza podstawowego zestawu Mandelbrot czarno i używa metody [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)go umieścić na mapę bitową.

### <a name="marking-progress"></a>Oznaczenie postępu

Do raportowania postępu z metody asynchronicznej można utworzyć wystąpienia [ `Progress<T>` ](https://developer.xamarin.com/api/type/System.Progress%3CT%3E/) klasy i zdefiniuj metodę asynchroniczną ma argumentu typu [ `IProgress<T>` ](https://developer.xamarin.com/api/type/System.IProgress%3CT%3E/). To jest przedstawiona w [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) próbki.

### <a name="cancelling-the-job"></a>Anulowanie zadania

Można również napisać metody asynchronicznej być możliwe. Rozpoczynać się od klasy o nazwie [ `CancellationTokenSource` ](https://developer.xamarin.com/api/type/System.Threading.CancellationTokenSource/). [ `Token` ](https://developer.xamarin.com/api/property/System.Threading.CancellationTokenSource.Token/) Właściwość jest wartością typu [ `CancellationToken` ](https://developer.xamarin.com/api/type/System.Threading.CancellationToken/). To jest przekazany do funkcji asynchronicznej. Wywołuje program [ `Cancel` ](https://developer.xamarin.com/api/member/System.Threading.CancellationTokenSource.Cancel()/) metody `CancellationTokenSource` (zazwyczaj w odpowiedzi na akcję przez użytkownika) aby anulować funkcji asynchronicznej.

Metody asynchronicznej mogą okresowo sprawdzać [ `IsCancellationRequested` ](https://developer.xamarin.com/api/property/System.Threading.CancellationToken.IsCancellationRequested/) właściwości `CancellationToken` i zamknąć, jeśli właściwość jest `true`, lub po prostu Wywołaj [ `ThrowIfCancellationRequested` ](https://developer.xamarin.com/api/member/System.Threading.CancellationToken.ThrowIfCancellationRequested()/) metody w którym to przypadku metoda kończy [ `OperationCancelledException` ](https://developer.xamarin.com/api/type/System.OperationCanceledException/).

[ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) przykładzie pokazano na użycie funkcji możliwe.

### <a name="an-mvvm-mandelbrot"></a>Mandelbrot MVVM

[ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) próbki ma szerszej interfejsu użytkownika, a przede wszystkim opiera się na [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) i [ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)klasy:

[![Potrójna zrzut ekranu przedstawiający Mandelbrot X F](images/ch20fg13-small.png "MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "MVVM Mandelbrot")

## <a name="back-to-the-web"></a>Powrót do sieci web

[ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) Klasa używana w niektórych próbek protokół stosowane asynchroniczne o nazwie Model programowania asynchronicznego lub APM. Taka klasa można przekonwertować na nowoczesnych protokołu NACIŚNIJ przy użyciu jednej z `FromAsync` metod w [ `TaskFactory` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskFactory%3CTResult%3E/) klasy. [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) przykładzie pokazano to.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 20 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [Przykłady działu 20](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [Praca z plikami](~/xamarin-forms/app-fundamentals/files.md)
