---
title: Część 5 - praktyczne kodu strategii udostępniania
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: d5f639cffc8ff2d134731374bd72663fec81c6a0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="part-5---practical-code-sharing-strategies"></a>Część 5 - praktyczne kodu strategii udostępniania

Ta sekcja zawiera przykłady sposobu udostępniania kod dla typowych scenariuszy aplikacji.



## <a name="data-layer"></a>Warstwa danych

Warstwa danych składa się z aparatu magazynu i metody służące do odczytywania i zapisywania informacji. Wydajność, elastyczność i Międzyplatformowe zgodności SQLite aparatu bazy danych jest zalecane dla aplikacji dla wielu platform Xamarin.
Jest uruchamiany na różnych platformach w tym systemu Windows, Android, iOS i Mac


### <a name="sqlite"></a>SQLite

SQLite jest implementacją open source bazy danych. Źródło i dokumentację można znaleźć w folderze [SQLite.org](http://www.sqlite.org/). SQLite jest dostępna na każdej z platform przenośnych:

-  **iOS** — wbudowanej w system operacyjny.
- **Android** — wbudowanej w system operacyjny od systemu Android w wersji 2.2 (10 poziom interfejsu API).
- **Windows** — zobacz [SQLite dla platformy uniwersalnej systemu Windows rozszerzenia](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).


Nawet w przypadku aparatu bazy danych jest dostępne na wszystkich platformach metod natywnych dostęp do bazy danych różnią się. Zarówno systemów iOS i Android oferuje wbudowane interfejsów API, aby uzyskiwać dostęp do bazy danych SQLite, która może być używana z Xamarin.iOS lub Xamarin.Android, jednak przy użyciu metod natywnych SDK oferuje możliwość udostępniania kodu (inne niż prawdopodobnie zapytania SQL, przy założeniu, że są one przechowywane jako ciąg) . Aby uzyskać szczegółowe informacje o wyszukiwaniu funkcji natywnej bazy danych dla `CoreData` w systemu iOS lub Android w `SQLiteOpenHelper` klasy; ponieważ te opcje nie są międzyplatformowa one wykraczają poza zakres tego dokumentu.



### <a name="adonet"></a>ADO.NET

Obsługa zarówno Xamarin.iOS i Xamarin.Android `System.Data` i `Mono.Data.Sqlite` (zobacz Xamarin.iOS [dokumentacji](~/ios/data-cloud/system.data.md) Aby uzyskać więcej informacji).
Używanie tych przestrzeni nazw umożliwia pisanie kodu ADO.NET, który działa na obu platform. Edytuj odwołania projektu do uwzględnienia `System.Data.dll` i `Mono.Data.Sqlite.dll` i dodaj je przy użyciu instrukcji w kodzie:

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

Następujący przykładowy kod będzie działać:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "items.db3");
bool exists = File.Exists (dbPath);
if (!exists)
    SqliteConnection.CreateFile (dbPath);
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open ();
if (!exists) {
    // This is the first time the app has run and/or that we need the DB.
    // Copy a "template" DB from your assets, or programmatically create one like this:
    var commands = new[]{
        "CREATE TABLE [Items] (Key ntext, Value ntext);",
        "INSERT INTO [Items] ([Key], [Value]) VALUES ('sample', 'text')"
    };
    foreach (var command in commands) {
        using (var c = connection.CreateCommand ()) {
            c.CommandText = command;
            c.ExecuteNonQuery ();
        }
    }
}
// use `connection`... here, we'll just append the contents to a TextView
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT [Key], [Value] from [Items]";
    var r = contents.ExecuteReader ();
    while (r.Read ())
        Console.Write("\n\tKey={0}; Value={1}",
                r ["Key"].ToString (),
                r ["Value"].ToString ());
}
connection.Close ();
```

Rzeczywistych wdrożeń ADO.NET oczywiście będzie można podzielić na różnych metod i klasy (w tym przykładzie jest tylko w celach demonstracyjnych).



### <a name="sqlite-net--cross-platform-orm"></a>NET SQLite — ORM między platformami

ORM (lub obiektów relacyjnych mapowania) próbuje uprościć magazynu danych, zgodnie z modelem w klasach. Zamiast ręcznie zapisywanie zapytania SQL w tym tabel, Utwórz lub wybierz, INSERT i DELETE danych, które ręcznie jest wyodrębniana z pola i właściwości, ORM dodaje warstwy kodu, który nie. Za pomocą odbicia do sprawdzenia struktury klas, ORM może automatycznie tworzyć tabele i kolumny, która pasuje do klasy i generowanie zapytań do odczytu i zapisu danych. Dzięki temu kod aplikacji, po prostu wysyłanie i pobieranie wystąpień obiektów do ORM, która zajmuje się wszystkie operacje pod maską programu SQL.

SQLite NET działa jako prosty ORM, które pozwolą zapisywania i pobierania z klas w SQLite. Ukrywa on złożoność cross platform SQLite dostępu z kombinacją dyrektywy kompilatora i inne wskazówki.

Funkcje bazy danych SQLite sieci:

-  Tabele są definiowane przez dodanie atrybutów do modelu klasy.
-  Wystąpienie bazy danych jest reprezentowana przez podklasę `SQLiteConnection` , klasy głównym w bibliotece SQLite Net.
-  Mogą być wstawiane danych, którego dotyczy zapytanie i usuwane przy użyciu obiektów. Nie instrukcji SQL są wymagane (mimo że można tworzenia instrukcji SQL, jeśli jest to wymagane).
-  Podstawowe zapytań Linq można wykonać w kolekcjach zwracany przez polecenie NET SQLite.


Kod źródłowy i dokumentacja dla SQLite NET jest dostępna na [SQLite-Net w serwisie github](https://github.com/praeclarum/sqlite-net) i zostało zaimplementowane w obu wdrożeń. Prosty przykład kodu SQLite NET (z *Tasky Pro* analizę przypadku) są wyświetlane poniżej.

Najpierw `TodoItem` klasy używa atrybutów, aby zdefiniować pole jako klucz podstawowy bazy danych:

```csharp
public class TodoItem : IBusinessEntity
{
    public TodoItem () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

Dzięki temu `TodoItem` tabeli do utworzenia o następujący wiersz kodu (i żadnych instrukcji SQL) na `SQLiteConnection` wystąpienie:

```csharp
CreateTable<TodoItem> ();
```

Dane w tabeli również można modyfikować za pomocą innych metod na `SQLiteConnection` (ponownie, bez konieczności instrukcji SQL):

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

Zobacz kod źródłowy analizę przypadku pełną przykłady.



## <a name="file-access"></a>Dostęp do plików

Dostęp do plików jest określone jako część klucza dowolnej aplikacji. Typowe przykłady plików, które mogą być częścią include aplikacji:

-  Pliki bazy danych SQLite.
-  Dane generowane przez użytkownika (tekst, obrazy, dźwięk, wideo).
-  Pobrane dane do buforowania (obrazy, html lub plików PDF).




### <a name="systemio-direct-access"></a>Dostęp bezpośredni System.IO

Zarówno Xamarin.iOS, jak i platformy Xamarin.Android Zezwalaj dostępu do systemu plików przy użyciu klas w `System.IO` przestrzeni nazw.

Każdej z platform mają inne ograniczenia dostępu, które należy wziąć pod uwagę:

-  uruchamianie aplikacji systemu iOS w bibliotece systemu plików bardzo ograniczony dostęp. Dalsze Apple decyduje o tym, jak należy używać systemu plików, określając określonych lokalizacjach, które są przechowywane w kopii zapasowej (i innych osób, które nie są). Zapoznaj się [Praca w systemie plików w Xamarin.iOS](~/ios/app-fundamentals/file-system.md) przewodnika, aby uzyskać więcej informacji.
-  Android również ogranicza dostęp do niektórych katalogów związanych z aplikacją, ale obsługuje ona również nośniki zewnętrzne (np.) Karty SD) i uzyskiwania dostępu do udostępnionych danych.
-  Windows Phone 8 (platformy Silverlight) nie zezwalają na dostęp bezpośredni pliku — może jedynie manipulować plików przy użyciu `IsolatedStorage`.
-  Projekty WinRT Windows 8.1 i Windows 10 platformy uniwersalnej systemu Windows oferuje tylko operacje asynchroniczne plików za pomocą `Windows.Storage` interfejsów API, które różnią się od innych platform.

#### <a name="example-for-ios-and-android"></a>Przykład dla systemów iOS i Android

Poniżej przedstawiono przykład prosta, który zapisuje i odczytuje plik tekstowy.
Przy użyciu `Environment.GetFolderPath` zezwala na tego samego kodu do uruchomienia w systemach iOS i Android, zwracające wartość każdego prawidłowy katalog oparte na Konwencji ich systemu plików.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.ReadAllText (filePath));
```

Odwoływać się do platformy Xamarin.iOS [Praca w systemie plików](~/ios/app-fundamentals/file-system.md) dokumentu, aby uzyskać więcej informacji na temat funkcji specyficznych dla systemu iOS, system plików. Podczas pisania kodu dostępu do plików i platform, należy pamiętać, że niektóre systemy plików jest rozróżniana wielkość liter i mieć separatorów innego katalogu. Dobrą praktyką jest zawsze używać tej samej wielkości liter w nazwach plików i `Path.Combine()` metody podczas konstruowania ścieżki pliku lub katalogu.



### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows.Storage dla systemu Windows 8 i Windows 10

*Tworzenia aplikacji mobilnych za pomocą platformy Xamarin.Forms* [książki](https://developer.xamarin.com/r/xamarin-forms/book/)
[działu 20. Async i We/Wy plików](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) obejmuje [przykłady dla Windows 8.1 i Windows 10](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20).

Przy użyciu [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) można odczytywać i pliku plików na tych platformach, korzystając z obsługiwanych interfejsów API:

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

Zapoznaj się [rozdziału książki](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) więcej szczegółów.


<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Izolowany magazyn na Windows Phone 7 i 8 (platformy Silverlight)

Izolowany magazyn jest wspólnego interfejsu API dla zapisywanie i ładowanie plików we wszystkich systemu iOS, Android i starszych platformach Windows Phone.

Jest to domyślny mechanizm przy dostępie do plików w Windows Phone (platformy Silverlight), które zostało zaimplementowane w Xamarin.iOS i Xamarin.Android wspólnej kodu dostępu do pliku do zapisania. `System.IO.IsolatedStorage` Klasy można odwoływać się na wszystkich platformach trzy w [projektu udostępnionego](~/cross-platform/app-fundamentals/shared-projects.md).

Zapoznaj się [izolowanego magazynu — omówienie dla Windows Phone](http://msdn.microsoft.com/en-us/library/windowsphone/develop/ff402541(v=vs.105).aspx) Aby uzyskać więcej informacji.

Interfejsy API izolowanego magazynu nie są dostępne w [przenośnej biblioteki klas](~/cross-platform/app-fundamentals/pcl.md). Jest jednym zamiast PCL [PCLStorage NuGet](https://pclstorage.codeplex.com/)



### <a name="cross-platform-file-access-in-pcls"></a>Dostęp do plików i platform w PCLs

Jest również Nuget zgodne z PCL — [PCLStorage](https://www.nuget.org/packages/PCLStorage/) — tego urządzenia wieloplatformowych plik dostęp do platformy obsługiwane platformy Xamarin i najnowszych interfejsów API systemu Windows.


## <a name="network-operations"></a>Operacje sieciowe

Aplikacje mobilne najbardziej mają składnik sieci, na przykład:

-  Pobieranie obrazów, wideo i audio (np.) miniatury zdjęcia, Muzyka).
-  Pobieranie (np dokumentów. HTML, PDF).
-  Przekazywanie danych użytkownika (na przykład zdjęcia lub tekst).
-  Uzyskiwanie dostępu do usług sieci web lub strona 3 interfejsów API (takie jak SOAP, XML lub JSON).


Platforma .NET Framework zapewnia kilka różnych klas dostępu do zasobów sieciowych: `HttpClient`, `WebClient`, i `HttpWebRequest`.

### <a name="httpclient"></a>HttpClient

`HttpClient` Klasy w `System.Net.Http` przestrzeń nazw jest dostępna w Xamarin.iOS, Xamarin.Android i większości platform Windows. Brak [Nuget biblioteki klienta HTTP Microsoft](https://www.nuget.org/packages/Microsoft.Net.Http/) który może służyć do zapewnienia tego interfejsu API przenośnej biblioteki klas (i Windows Phone 8 Silverlight).

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>Klient sieci Web

`WebClient` Klasa udostępnia prosty interfejs API do pobierania zdalnych danych z serwerów zdalnych.

Operacje Windows Platofrm Universal *musi* jest asynchroniczne, mimo że Xamarin.iOS i Xamarin.Android obsługują operacje synchroniczne (które można wprowadzić na wątki w tle).

Kod dla prostego asychronous `WebClient` operacja jest:

```csharp
var webClient = new WebClient ();
webClient.DownloadStringCompleted += (sender, e) =>
{
    var resultString = e.Result;
    // do something with downloaded string, do UI interaction on main thread
};
webClient.Encoding = System.Text.Encoding.UTF8;
webClient.DownloadStringAsync (new Uri ("http://some-server.com/file.xml"));
```

 `WebClient` ma również `DownloadFileCompleted` i `DownloadFileAsync` do pobierania danych binarnych.

<a name="HttpWebRequest" />

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest` oferuje więcej możliwości dostosowania niż `WebClient` i w związku z tym wymaga więcej kodu do użycia.

Kod dla prostego synchroniczne `HttpWebRequest` operacja jest:

```csharp
var request = HttpWebRequest.Create(@"http://some-server.com/file.xml ");
request.ContentType = "text/xml";
request.Method = "GET";
using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
    if (response.StatusCode != HttpStatusCode.OK)
        Console.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
    using (StreamReader reader = new StreamReader(response.GetResponseStream()))
    {
        var content = reader.ReadToEnd();
        // do something with downloaded string, do UI interaction on main thread
    }
}
```

Brak przykładem w naszym [dokumentacji usług sieci Web](~/cross-platform/data-cloud/web-services/index.md).

 <a name="Reachability" />


### <a name="reachability"></a>Uzyskiwanie

Urządzenia przenośne działają na podstawie różnych warunków sieciowych z szybkiej sieci Wi-Fi lub połączeń 4G do obszarów Słaby odbiór i powolnego łącza danych KRAWĘDZI. W związku z tym jest dobrym rozwiązaniem, aby wykryć, czy sieć jest dostępna, a jeśli tak, jakiego typu sieci jest dostępne, przed próbą nawiązania połączenia z serwerami zdalnymi.

Dostępne są następujące akcje aplikacji mobilnej może potrwać w następujących sytuacjach:

-  Jeśli sieć jest niedostępna, należy poinformować użytkownika. Jeśli te zostały ręcznie wyłączone go (np.) Tryb samolotowy lub wyłączenie sieci Wi-Fi), a następnie ich rozwiązania problemu.
-  Jeśli połączenie jest sieci 3G, aplikacje mogą zachowywać się inaczej (na przykład firmy Apple nie zezwala na aplikacje większe niż 20Mb do pobrania ponad 3G). Aplikacje mogą wykorzystać te informacje, aby ostrzec użytkownika o nadmiernej pobierania razy podczas pobierania dużych plików.
-  Nawet jeśli sieć jest dostępna, jest dobrym rozwiązaniem do weryfikowania łączności z serwerem docelowym przed zainicjowaniem innych żądań. Spowoduje to wielokrotnie zapobiec operacje sieciowe aplikacji z przekroczeniem limitu czasu i również umożliwić bardziej szczegółowy komunikat o błędzie ma być wyświetlony dla użytkownika.


Brak [próbki Xamarin.iOS](https://github.com/xamarin/monotouch-samples/tree/master/ReachabilitySample) dostępne (opartej na firmy Apple [uzyskiwanie przykładowy kod](http://developer.apple.com/library/ios/#samplecode/Reachability/Introduction/Intro.html) ) pomaga wykrywać dostępność sieci.


## <a name="webservices"></a>Usługi sieci Web

Zobacz dokumentację dotyczącą na [pracy z usługami sieci Web](~/cross-platform/data-cloud/web-services/index.md), który obejmuje dostęp do REST, SOAP i WCF punktów końcowych przy użyciu platformy Xamarin.iOS. Jest możliwość żądania usługi sieci web dostępne jednostki i przeanalizować odpowiedzi, jednak są dostępne ustawić to znacznie prostsze, w tym Azure, RestSharp i ServiceStack biblioteki. Można uzyskać nawet podstawowe operacje usługi WCF w aplikacji platformy Xamarin.

### <a name="azure"></a>Azure

Microsoft Azure to platforma chmury, która zapewnia szerokiej gamy usług dla aplikacji mobilnych, łącznie z magazynu danych i synchronizacji oraz powiadomień wypychanych.

Odwiedź stronę [azure.microsoft.com](https://azure.microsoft.com/) próby bezpłatnie.

### <a name="restsharp"></a>RestSharp

RestSharp jest biblioteki .NET, który można umieścić w aplikacjach mobilnych, aby zapewnić klienta REST, które ułatwiają dostęp do usług sieci web. Pomaga, zapewniając prosty interfejs API do żądania danych i przeanalizować odpowiedzi REST. RestSharp mogą być przydatne

[RestSharp witryny sieci Web](http://restsharp.org/) zawiera [dokumentacji](https://github.com/restsharp/RestSharp/wiki) implementowania klienta REST przy użyciu RestSharp.
RestSharp przykłady Xamarin.iOS i Xamarin.Android [github](https://github.com/restsharp/RestSharp/).

Jest również fragment kodu platformy Xamarin.iOS w naszym [dokumentacji usług sieci Web](~/cross-platform/data-cloud/web-services/index.md).

 <a name="ServiceStack" />


### <a name="servicestack"></a>ServiceStack

W odróżnieniu od RestSharp ServiceStack to zarówno rozwiązanie po stronie serwera do obsługi usługi sieci web, a także biblioteki klienckiej, która może być wdrożonych w aplikacji dla urządzeń przenośnych dostępu do tych usług.

[ServiceStack witryny sieci Web](http://servicestack.net/) objaśniający przeznaczenie projektu i linki do dokumentów i przykładowy kod. Przykładami zakończenia wdrożenia po stronie serwera usługi sieci web, a także różne aplikacje po stronie klienta, które do niego dostęp.

Brak [przykład Xamarin.iOS](http://www.servicestack.net/monotouch/remote-info/) ServiceStack witryny sieci Web i fragment kodu w naszym [dokumentacji usług sieci Web](~/cross-platform/data-cloud/web-services/index.md).


### <a name="wcf"></a>WCF

Xamarin narzędzia ułatwiają korzystanie z niektórych usług Windows Communication Foundation (WCF). Ogólnie rzecz biorąc Xamarin obsługuje samej podzbiór po stronie klienta WCF, który jest dostarczany z środowisko uruchomieniowe Silverlight. Obejmuje to najczęściej używane kodowanie i protokół implementacje WCF: transportu kodowany tekst wiadomości SOAP za pośrednictwem protokołu HTTP przy użyciu protokołu `BasicHttpBinding`.

Ze względu na rozmiar i złożoność platformy WCF mogą być implementacji aktualnych i przyszłych usługi, które wykracza poza zasięgiem obsługiwane przez domenę podzestawu klienta w programie Xamarin. Ponadto obsługa usług WCF wymaga użycia narzędzia dostępne tylko w środowisku systemu Windows, aby wygenerować serwera proxy.

 <a name="Threading" />


## <a name="threading"></a>Wątkowość

Czas odpowiedzi aplikacji jest ważna w przypadku aplikacji dla urządzeń przenośnych — Użytkownicy oczekują, że aplikacje do ładowania i szybko wykonać. Ekran "zablokowane" zatrzymuje akceptowanie danych wejściowych użytkownika pojawi się wskazująca, że wystąpiła awaria aplikacji, dlatego ważne jest, aby nie zajmować wątku interfejsu użytkownika z blokowaniem wywołań długotrwałe, takich jak żądania sieci lub powolne operacje lokalne (na przykład rozpakować pliku). W szczególności procesu uruchamiania nie powinna zawierać długotrwałych zadań — wszystkich platform urządzeń przenośnych będą kill aplikacji trwa zbyt długo, aby załadować.

Oznacza to, że interfejs użytkownika powinien implementować wskaźnik postępu lub w przeciwnym razie użyteczny interfejsu użytkownika, który można szybko wyświetlić i zadań asynchronicznych wykonywać operacje w tle. Wykonywanie zadania w tle wymaga użycia wątków, co oznacza, że na potrzeby zadań tła sposób komunikacji wątku głównego, aby wskazać postęp lub po zakończeniu.

 <a name="Parallel_Task_Library" />


### <a name="parallel-task-library"></a>Biblioteka zadań równoległych

Utworzone za pomocą Biblioteka zadań równoległych zadań można uruchomić asynchronicznie i zwraca na ich wątek wywołujący, dzięki czemu przydatne służącą do wyzwalania długotrwałej operacji bez blokowania interfejsu użytkownika.

Operacja prostych zadań równoległych może wyglądać następująco:

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

Klucz jest `TaskScheduler.FromCurrentSynchronizationContext()` której będzie używał wartość parametru SynchronizationContext.Current wątku wywołanie metody (tutaj wątku głównego, który działa `MainThreadMethod`) jako sposób organizowania wywołań wstecz do tego wątku. Oznacza to, gdy metoda została wywołana w wątku interfejsu użytkownika, zostanie uruchomiony `ContinueWith` operacji wstecz w wątku interfejsu użytkownika.

Jeśli kod jest uruchamianie zadań z innych wątków, użyj następującego wzorca, aby utworzyć odwołanie do wątku interfejsu użytkownika i zadanie może nadal wywołania zwrotnego do niej:

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />


### <a name="invoking-on-the-ui-thread"></a>Wywoływanie w wątku interfejsu użytkownika

Składnia dla operacji kierowania do wątku interfejsu użytkownika dla kodu, które nie korzystają Biblioteka zadań równoległych dotyczy wszystkich platform:

-  **iOS** – `owner.BeginInvokeOnMainThread(new NSAction(action))`
-  **Android** — `owner.RunOnUiThread(action)`
-  **Platformy Xamarin.Forms** — `Device.BeginInvokeOnMainThread(action)`
-  **Windows** — `Deployment.Current.Dispatcher.BeginInvoke(action)`



IOS i Android składni wymaga klasy 'context' ma być dostępny, co oznacza, że kod trzeba przekazać ten obiekt do żadnych metod, które wymagają wywołania zwrotnego dla wątku interfejsu użytkownika.

Aby wykonywać wywołania wątku interfejsu użytkownika w kodzie udostępnionego, wykonaj [przykład IDispatchOnUIThread](http://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (dzięki uprzejmości [ @follesoe ](http://jonas.follesoe.no/)). Deklarowanie i program w celu `IDispatchOnUIThread` interfejsu w kodzie udostępnionego, a następnie wdrożyć klasy specyficzne dla platformy, jak pokazano poniżej:

```csharp
// program to the interface in shared code
public interface IDispatchOnUIThread {
    void Invoke (Action action);
}
// iOS
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly NSObject owner;
    public DispatchAdapter (NSObject owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.BeginInvokeOnMainThread(new NSAction(action));
    }
}
// Android
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly Activity owner;
    public DispatchAdapter (Activity owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.RunOnUiThread(action);
    }
}
// WP7
public class DispatchAdapter : IDispatchOnUIThread {
    public void Invoke (Action action) {
        Deployment.Current.Dispatcher.BeginInvoke(action);
    }
}
```

Należy używać deweloperów platformy Xamarin.Forms [ `Device.BeginInvokeOnMainThread` ](~/xamarin-forms/platform/device.md#Device_BeginInvokeOnMainThread) w typowy kod (udostępnionych projektów lub PCL).

 <a name="Platform_and_Device_Capabilities_and_Degradation" />


## <a name="platform-and-device-capabilities-and-degradation"></a>Platforma i możliwości urządzenia i degradacji

Dalsze szczegółowe przykłady zajmowanie się różne możliwości są podane w dokumentacji możliwości platformy. Dotyczy on wykrywanie różne możliwości i sposobu bezpiecznie zmniejszyć aplikacji, aby zapewnić poprawne działanie, nawet wtedy, gdy aplikacja nie może działać w pełni możliwości.
