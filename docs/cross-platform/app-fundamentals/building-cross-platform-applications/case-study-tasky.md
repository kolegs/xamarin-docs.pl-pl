---
title: 'Analiza przypadku: Tasky'
description: "Tym dokumencie opisano, jak zastosowano zasady tworzenie wieloplatformowych aplikacji w przenośnych Tasky przykładowej aplikacji. Krawędzi na projekt aplikacji mobilnej, zapisywanie typowy kod do ponownego użycia i wdrażanie projektach specyficzne dla platformy, które są przeznaczone dla systemu iOS, Android i Windows Phone platformy."
ms.topic: article
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 5bd19e04934f6b86143c93c759c0c2ac76956a7e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="case-study-tasky"></a>Analiza przypadku: Tasky

_Tym dokumencie opisano, jak zastosowano zasady tworzenie wieloplatformowych aplikacji w przenośnych Tasky przykładowej aplikacji. Krawędzi na projekt aplikacji mobilnej, zapisywanie typowy kod do ponownego użycia i wdrażanie projektach specyficzne dla platformy, które są przeznaczone dla systemu iOS, Android i Windows Phone platformy._

*Tasky* *przenośne* jest aplikacją listy zadań do wykonania prostego. W tym dokumencie omówiono sposób został zaprojektowany i zbudowany, następujących wskazówek z [tworzenie aplikacji wieloplatformowych](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) dokumentu. Dyskusja obejmuje następujące zagadnienia:


### <a name="design"></a>Projekt

W tej sekcji opisano ogólne podejście do rozpoczynania nowy projekt aplikacji mobilnej i platform, takich jak wymagania dotyczące generowania, tworzenie makiety ekranu i identyfikowania najważniejsze funkcje kodu.

 <a name="Common_Code" />


### <a name="common-code"></a>Typowy kod

Wyjaśniono, jak i platform jest tworzony warstwy bazy danych i biznesowych. A `TaskItemManager` klasy są zapisywane w zapewniają prosty "interfejs API", który jest dostępny przez warstwę interfejsu użytkownika. Szczegóły implementacji typowy kod są hermetyzowane przez `TaskItemManager` i `TaskItem` klasy, które zwraca. Typowy kod z uzyskuje dostęp do każdej z platform **przenośne Library(PCL) — klasa**

 <a name="Platform-Specific_Applications" />


### <a name="platform-specific-applications"></a>Specyficzne dla platformy aplikacji

Tasky działa w systemie iOS, Android i Windows Phone. Każda aplikacja specyficzne dla platformy implementuje natywnego interfejsu użytkownika można wykonać następujące czynności:

1.  Wyświetlona zostanie lista zadań
2.  Tworzenie, edytowanie, Zapisz i usuwanie zadań.

 <a name="Design_Process" />


# <a name="design-process"></a>Proces projektowania

Zaleca się tworzenie z mapy drogowej co chcesz osiągnąć przed rozpoczęciem kodowania. Dotyczy to zwłaszcza aplikacji dla wielu platform, gdy tworzysz funkcje, które mają być widoczne na wiele sposobów. Począwszy od Wyczyść informacje o tym, co tworzysz pozwala zaoszczędzić czas i wysiłek później w cyklu programowania.

 <a name="Requirements" />


## <a name="requirements"></a>Wymagania

Pierwszy krok w projektowaniu aplikacji jest do identyfikowania żądanych funkcji. Mogą to być ogólnych celów lub szczegółowo opisane przypadki użycia. Tasky ma proste wymagania funkcjonalności:

 -  Wyświetlanie listy zadań
 -  Dodawanie, edytowanie i usuwanie zadań
 -  Ustaw stan zadania na "gotowe"


Należy rozważyć korzystanie z funkcji specyficznych dla platformy.  Tasky skorzystać z geofencing z systemem iOS lub Windows Phone na żywo Kafelki? Nawet jeśli nie używasz funkcji specyficznych dla platformy w pierwszej wersji, należy zaplanować dalej upewnij się, że Twoja firma i warstwach danych mogą je obsługiwać.

 <a name="User_Interface_Design" />


## <a name="user-interface-design"></a>Projekt interfejsu użytkownika

Uruchom z ogólnymi procedurami projektowania, który może być zaimplementowany w platformach docelowych. Zwrócić uwagę, aby Uwaga przeznaczonych dla określonej platformy interfejsu użytkownika ograniczenia. Na przykład `TabBarController` w systemie iOS można wyświetlić więcej niż pięciu przycisków, natomiast jego odpowiednik Windows Phone można wyświetlać maksymalnie cztery.
Rysuj przepływu ekranu za pomocą narzędzia wyboru (papieru działa).

 [ ![](case-study-tasky-images/taskydesign.png "Rysuj przepływu ekranu przy użyciu narzędzia prac papieru wyboru")](case-study-tasky-images/taskydesign.png)

 <a name="Data_Model" />


## <a name="data-model"></a>Model danych

Wiedząc, jakie dane muszą być przechowywane pomogą określić mechanizm trwałości. Zobacz [dostęp do danych i Platform](~/cross-platform/app-fundamentals/index.md) uzyskać informacji na temat mechanizmów dostępny magazyn i pomoc przy wyborze między nimi. Dla tego projektu będziemy używać SQLite.NET.

Tasky musi przechowywać trzech właściwości dla każdego TaskItem:

 -  **Nazwa** — ciąg
 -  **Informacje o** — ciąg
 -  **Gotowe** — wartość logiczna


 <a name="Core_Functionality" />


## <a name="core-functionality"></a>Podstawowe funkcje

Należy wziąć pod uwagę interfejsu API, który trzeba korzystać w celu spełnienia wymagań interfejsu użytkownika. Lista zadań do wykonania wymagane są następujące funkcje:

 -   **Wyświetl listę wszystkich zadań** — Aby wyświetlić listę wszystkich dostępnych zadań ekranu głównego
 -  **Pobierz zadanie** — po dotknięciu jest wiersz zadania
 -  **Zapisz jedno zadanie** — podczas edycji zadania
 -  **Usuń jedno zadanie** — gdy zadanie zostanie usunięta.
 -  **Utwórz zadanie puste** — po utworzeniu nowego zadania


Umożliwia ponowne użycie kodu tego interfejsu API powinny być raz implementowane w *przenośnej biblioteki klas*.

 <a name="Implementation" />


## <a name="implementation"></a>Implementacja

Po został uzgodniony projekt aplikacji, należy rozważyć sposób mogą zostać zaimplementowane jako aplikację i platform. Będzie to Architektura aplikacji. Postępując zgodnie ze wskazówkami w [tworzenie aplikacji wieloplatformowych](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) dokument, kod aplikacji powinien być przerwane w dół do następujących elementów:

 -   **Typowy kod** — wspólnej projekt, który zawiera recyklingowi kod do przechowywania danych zadań; ujawnia klasę modelu i interfejs API do zarządzania zapisywanie i ładowanie danych.
 -   **Kod specyficzne dla platformy** — projekty specyficzne dla platformy, które implementuje natywnego interfejsu użytkownika dla każdego systemu operacyjnego przy użyciu typowy kod jako "wewnętrzna".


 [ ![](case-study-tasky-images/taskypro-architecture.png "Projekty specyficzne dla platformy wdrożenie natywnego interfejsu użytkownika dla każdego systemu operacyjnego przy użyciu typowy kod jako wewnętrzna")](case-study-tasky-images/taskypro-architecture.png)

W poniższych sekcjach opisano te dwie części.

 <a name="Common_(PCL)_Code" />


# <a name="common-pcl-code"></a>Typowy kod (PCL)

Przenośna tasky używa strategii przenośnej biblioteki klas dla udostępniania typowy kod. Zobacz [opcje udostępniania kodu](~/cross-platform/app-fundamentals/code-sharing.md) dokumentu opis opcji udostępniania kodu.

Typowy kod, w tym Warstwa dostępu do danych, baza danych kodu i kontrakty, znajduje się w projekcie biblioteki.


Poniżej przedstawiono kompletnego projektu PCL. Cały kod w przenośna biblioteka jest zgodny z każdej platformy docelowej. Po wdrożeniu każdej aplikacji natywnej będzie odwoływać się do tej biblioteki.

![](case-study-tasky-images/portable-project.png "Po wdrożeniu każdej aplikacji natywnej będzie odwoływać się do tej biblioteki")

Na poniższym diagramie klasy zawiera klasy pogrupowane według warstwy. `SQLiteConnection` Klasa jest schematyczny kod z pakietu Sqlite NET. Pozostała klas jest niestandardowy kod Tasky. `TaskItemManager` i `TaskItem` klasy reprezentują interfejsu API, który jest uwidaczniany w aplikacji specyficzne dla platformy.

 [ ![](case-study-tasky-images/classdiagram-core.png "Klasy TaskItemManager i TaskItem reprezentuje interfejs API, który jest uwidaczniany w aplikacji specyficzne dla platformy")](case-study-tasky-images/classdiagram-core.png)

Używanie przestrzeni nazw do oddzielania warstwy ułatwia zarządzanie odwołania między każdej warstwy. Projekty specyficzne dla platformy wystarcza tylko obejmują `using` instrukcji dla warstwy biznesowej. Warstwa dostępu do danych i warstwą danych powinien hermetyzowany przez interfejs API, który jest udostępniany przez `TaskItemManager` w warstwie biznesowej.

 <a name="References" />


## <a name="references"></a>Odwołania

Biblioteki klas przenośnych konieczne może być używany na wielu platformach, każde z nich różne poziomy wsparcia dla platformy i framework funkcji. Z tego powodu istnieją ograniczenia, na których można użyć pakietów i bibliotek platformy. Na przykład Xamarin.iOS nie obsługuje języka c# `dynamic` — słowo kluczowe, więc biblioteki klas przenośnych nie można użyć dowolnego pakietu, który jest zależna od dynamiczny kod, nawet jeśli taki kod będzie działać w systemie Android. Visual Studio for Mac uniemożliwi Dodawanie niezgodnych pakietów i odwołań, ale należy pamiętać ograniczenia w celu uniknięcia niespodzianki później.

Uwaga: Zobaczysz odwoływania projektów bibliotek platformy, które nie były używane. Te odwołania są dołączane jako część szablonów projektu Xamarin. Gdy aplikacje są kompilowane, proces łączenia spowoduje usunięcie kodu bez odwołań, dlatego nawet jeśli `System.Xml` było odwołać się do jego nie będą uwzględniane w końcowym aplikacji ponieważ firma Microsoft nie używają żadnych funkcji Xml.

 <a name="Data_Layer_(DL)" />


## <a name="data-layer-dl"></a>Warstwa danych (DL)

Warstwa danych zawiera kod, który obsługuje magazynu fizycznego danych — do bazy danych, plików prostych lub inny mechanizm. Warstwa danych Tasky składa się z dwóch części: Biblioteka SQLite NET i kod niestandardowy dodane do połączenie go w górę.

Tasky polega na pakiet nuget Sqlite net (opublikowanych przez Kreuger Piotr) do osadzenia SQLite NET kod, który udostępnia interfejs bazy danych relacyjnych obiektu mapowania ORM (). `TaskItemDatabase` Klasa dziedziczy `SQLiteConnection` i dodaje wymagane tworzenia, odczytu, aktualizacji, usuwania (CRUD) metody do odczytu i zapisu danych SQLite. To proste umożliwiającego stosowania metody rodzajowe CRUD, które mogą być ponownie używane w innych projektach.

`TaskItemDatabase` Jest klasą pojedynczą, zapewniając, że dostęp wszystkich występuje względem tego samego wystąpienia. Blokada jest używany do chronienia współbieżny dostęp wiele wątków.

 <a name="SQLite_on_WIndows_Phone" />


### <a name="sqlite-on-windows-phone"></a>SQLite na Windows Phone

IOS i Android zarówno dostarczanych z SQLite jako część systemu operacyjnego Windows Phone nie zawiera aparat bazy danych zgodnej. Udostępnianie kodu na wszystkich platformach trzy wersję systemu Windows phone native SQLite jest wymagana. Zobacz [Praca z lokalnej bazy danych](~/xamarin-forms/app-fundamentals/databases.md) Aby uzyskać więcej informacji o konfigurowaniu projektu Windows Phone dla bazy danych Sqlite.

 <a name="Using_an_Interface_to_Generalize_Data_Access" />


### <a name="using-an-interface-to-generalize-data-access"></a>Przy użyciu interfejsu do uogólnienia dostępu do danych

Warstwa danych przejmuje zależność `BL.Contracts.IBusinessIdentity` , dzięki czemu można implementować metody dostępu abstrakcyjny danych, które wymagają klucza podstawowego. Dowolnej klasy warstwie Business, która implementuje interfejs może następnie utrwalone w warstwie danych.

Interfejs określa tylko właściwość typu integer do działania jako klucz podstawowy:

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

Klasa podstawowa implementuje interfejs i dodaje atrybuty SQLite NET oznaczyć je jako klucz podstawowy zwiększanie automatycznie. Każda klasa w warstwie Business, który implementuje ta klasa podstawowa można następnie utrwalone w warstwie danych:

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

To jest przykład metody rodzajowe w warstwie danych korzystające z interfejsu `GetItem<T>` metody:

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />


### <a name="locking-to-prevent-concurrent-access"></a>Blokowanie zapobiegające współbieżny dostęp

A [blokady](http://msdn.microsoft.com/en-us/library/c5kehkcz(v=vs.100).aspx) zostanie wdrożony w `TaskItemDatabase` klasę, aby zapobiec równoczesny dostęp do bazy danych. Dzięki jest serializowany współbieżny dostęp z różnych wątkach (w przeciwnym razie składnik interfejsu użytkownika może podejmować wielokrotne próby odczytu bazy danych w tym samym czasie wątku w tle jest aktualizacją). Przykład implementowania blokady jest następujący:

```csharp
static object locker = new object ();
public IEnumerable<T> GetItems<T> () where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return (from i in Table<T> () select i).ToList ();
    }
}
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

Większość kodu Warstwa danych mogą być ponownie używane w innych projektach. Kod tylko specyficzne dla aplikacji w warstwie `CreateTable<TaskItem>` wywołanie w `TaskItemDatabase` konstruktora.

 <a name="Data_Access_Layer_(DAL)" />


## <a name="data-access-layer-dal"></a>Warstwa dostępu do danych (DAL)

`TaskItemRepository` Klasa hermetyzuje mechanizmu magazynowania danych z jednoznacznie interfejs API umożliwiający `TaskItem` obiektów należy utworzyć, usunąć, pobrać i zaktualizować.

 <a name="Using_Conditional_Compilation" />


### <a name="using-conditional-compilation"></a>Przy użyciu kompilacja warunkowa

Klasy używane kompilacja warunkowa w celu ustawienia lokalizacji pliku — jest to przykład wykonawczych rozbieżności platformy. Właściwość, która zwraca ścieżkę kompiluje się do innego kodu na każdej z platform. Kod i dyrektywy kompilatora specyficzne dla platformy są wyświetlane tutaj:

```csharp
public static string DatabaseFilePath {
    get {
        var sqliteFilename = "TaskDB.db3";
#if SILVERLIGHT
        // Windows Phone expects a local path, not absolute
        var path = sqliteFilename;
#else
#if __ANDROID__
        // Just use whatever directory SpecialFolder.Personal returns
        string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
        // we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder
#endif
        var path = Path.Combine (libraryPath, sqliteFilename);
                #endif
                return path;
    }
}
```

W zależności od platformy, dane wyjściowe będą "<app
path>/Library/TaskDB.db3" dla systemu iOS, "<app
path>/Documents/TaskDB.db3" dla systemu Android lub po prostu "TaskDB.db3" dla Windows Phone.

## <a name="business-layer-bl"></a>Warstwa biznesowa (czarnej listy)

Warstwa biznesowa implementuje klasy modelu i fasad nimi zarządzać.
W Tasky Model jest `TaskItem` klasy i `TaskItemManager` implementuje wzorzec fasad, aby dostarczać interfejs API zarządzania `TaskItems`.

 <a name="Façade" />


### <a name="faade"></a>Fasad

 `TaskItemManager` opakowuje `DAL.TaskItemRepository` Get, Zapisz i Usuń metody, które będzie odwoływać się do aplikacji i warstwy interfejsu użytkownika.

Reguły biznesowe i logiki zostanie umieszczony w tym miejscu w razie potrzeby — na przykład reguł sprawdzania poprawności, które muszą zostać spełnione przed zapisaniem obiektu.

 <a name="API_for_Platform-Specific_Code" />


## <a name="api-for-platform-specific-code"></a>Interfejs API specyficzne dla platformy kodu

Po zapisaniu typowy kod interfejs użytkownika muszą zostać skompilowane do zbierania i wyświetlania danych udostępnianych przez go. `TaskItemManager` Klasa implementuje wzorzec fasad zapewnienie prosty interfejs API dla kodu aplikacji do uzyskania dostępu.

Kod napisany w każdym projekcie specyficzne dla platformy będzie zazwyczaj ściśle powiązane do natywnej SDK tego urządzenia i uzyskiwać dostęp tylko do typowy kod przy użyciu interfejsu API zdefiniowany przez `TaskItemManager`. Dotyczy to również metody firm klasy i jej ujawnia, takich jak `TaskItem`.

Obrazy nie są udostępniane na różnych platformach, ale dodany niezależnie w każdym projekcie. Jest to ważne, ponieważ każdej platformy obsługi obrazów w różny sposób, przy użyciu innych nazw plików, katalogów i rozwiązania.

W pozostałych sekcjach omówiono szczegóły implementacji specyficzne dla platformy Tasky interfejsu użytkownika.

 <a name="iOS_App" />


# <a name="ios-app"></a>Aplikacja systemu iOS

Istnieje tylko kilka klas wymagane do wykonania dla systemu iOS Tasky aplikacji przy użyciu wspólnej projektu PCL do przechowywania i pobierania danych. Poniżej przedstawiono projektu platformy Xamarin.iOS pełną iOS:

 ![](case-study-tasky-images/taskyios-solution.png "Projekt dla systemu iOS jest wyświetlany w tym miejscu")

Klasy są wyświetlane na tym diagramie, podzielone na warstwy.

 [ ![](case-study-tasky-images/classdiagram-android.png "Klasy są wyświetlane na tym diagramie, podzielone na warstwy")](case-study-tasky-images/classdiagram-android.png)

 <a name="References" />


## <a name="references"></a>Odwołania

Aplikacja systemu iOS odwołuje się specyficzne dla platformy biblioteki zestawu SDK — np. Xamarin.iOS i MonoTouch.Dialog-1.

Należy także odwoływać `TaskyPortableLibrary` PCL projektu.
Lista odwołań jest następujący:

 ![](case-study-tasky-images/taskyios-references.png "Lista odwołań jest wyświetlany w tym miejscu")

W tym projekcie przy użyciu tych odwołań są stosowane warstwy aplikacji i warstwy interfejsu użytkownika.

 <a name="Application_Layer_(AL)" />


## <a name="application-layer-al"></a>Warstwa aplikacji (AL)

Warstwa aplikacji zawiera klasy specyficzne dla platformy wymaganej do powiązania obiektów udostępnianych przez PCL do interfejsu użytkownika. Aplikacja specyficzne dla systemu iOS ma dwie klasy, aby wyświetlić zadania:

 -   **EditingSource** — ta klasa jest używana do powiązania listy zadań do interfejsu użytkownika. Ponieważ `MonoTouch.Dialog` użyto listy zadań musimy zaimplementować tego pomocnika, aby włączyć funkcję przejdź do usunięcia w `UITableView` . Przejdź do usunięcia jest typowe na systemy iOS, ale nie Android lub Windows Phone, więc określonego projektu iOS jest tylko jeden, który implementuje on.
 -   **TaskDialog** — ta klasa jest używana do powiązania pojedyncze zadanie do interfejsu użytkownika. Używa `MonoTouch.Dialog` interfejsu API odbicia opakowywać "" `TaskItem` obiekt z klasy, która zawiera prawidłowe atrybuty, które umożliwia wprowadzania ekranie, aby poprawnie sformatowane.


`TaskDialog` Klasy używa `MonoTouch.Dialog` atrybuty tworzenia ekranu na podstawie klasy właściwości. Klasa wygląda następująco:

```csharp
public class TaskDialog {
    public TaskDialog (TaskItem task)
    {
        Name = task.Name;
        Notes = task.Notes;
        Done = task.Done;
    }
    [Entry("task name")]
    public string Name { get; set; }
    [Entry("other task info")]
    public string Notes { get; set; }
    [Entry("Done")]
    public bool Done { get; set; }
    [Section ("")]
    [OnTap ("SaveTask")]    // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Save;
    [Section ("")]
    [OnTap ("DeleteTask")]  // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Delete;
}
```

Powiadomienia `OnTap` atrybuty wymagają nazwy metody — te metody musi istnieć w klasie, gdzie `MonoTouch.Dialog.BindingContext` jest tworzony (w tym przypadku `HomeScreen` klasy omówiona w następnej sekcji).

 <a name="User_Interface_Layer_(UI)" />


## <a name="user-interface-layer-ui"></a>Warstwę interfejsu użytkownika (UI)

Warstwę interfejsu użytkownika składa się z następujących klas:

1.   **AppDelegate** — zawiera wywołania interfejsu API wygląd stylu czcionki i kolory używane w aplikacji. Tasky jest prosta aplikacja, nie ma żadnych innych zadań inicjowania w `FinishedLaunching` .
2.   **Ekrany** — podklasy `UIViewController` definiującą każdego ekranu i jego zachowanie. Ekrany powiązać razem z warstwy aplikacji klasy interfejsu użytkownika i wspólnego interfejsu API ( `TaskItemManager` ). W tym przykładzie ekrany są tworzone w kodzie, ale można zostały zaprojektowane przy użyciu konstruktora interfejsu w środowisku Xcode lub projektanta scenorysu.
3.   **Obrazy** — elementy wizualne są ważnym elementem każdej aplikacji. Tasky ma obrazów powitalny ekranu i ikony, które musi być podane w zwykłych oraz siatkówki rozwiązania dla systemu iOS.


 <a name="Home_Screen" />


### <a name="home-screen"></a>Ekranu głównego

Ekran Home jest `MonoTouch.Dialog` ekranu, który wyświetla listę zadań z bazy danych SQLite. Dziedziczy on z `DialogViewController` i implementuje kod, aby ustawić `Root` zawiera zbiór `TaskItem` obiektów do wyświetlenia.

 [ ![](case-study-tasky-images/ios-taskylist.png "Dziedziczy DialogViewController i implementuje kod, aby ustawić głównego zawiera kolekcję obiektów TaskItem do wyświetlenia")](case-study-tasky-images/ios-taskylist.png)

Są dwie metody main związane z wyświetlaniem i interakcji z listy zadań:

1.   **PopulateTable** — używa warstwie Business `TaskManager.GetTasks` metoda pobierania kolekcję `TaskItem` obiektów do wyświetlenia.
2.   **Wybrane** — po dotknięciu jest wiersz, wyświetla zadania w nowych ekranu.


 <a name="Task_Details_Screen" />


### <a name="task-details-screen"></a>Ekran szczegółów zadania

Szczegóły zadania jest wejściowych ekranu, który umożliwia zadań można edytować ani usuwać.

Używa tasky `MonoTouch.Dialog`tego interfejsu API odbicia, aby wyświetlić ekran, dlatego jest nie `UIViewController` implementacji. Zamiast tego `HomeScreen` klasy tworzy i wyświetla `DialogViewController` przy użyciu `TaskDialog` klasy z warstwy aplikacji.

Ten zrzut ekranu przedstawia pusty ekranu, który demonstruje `Entry` atrybutu Ustawianie tekstu znaku wodnego w **nazwa** i **uwagi** pola:

 [ ![](case-study-tasky-images/ios-taskydetail.png "Ten zrzut ekranu przedstawia pusty ekranu demonstrujący atrybut wpisu Ustawianie tekstu znaku wodnego w polach nazwy i uwagi")](case-study-tasky-images/ios-taskydetail.png)

Funkcje **szczegóły zadania** ekranie (np. Zapisywanie lub usuwanie zadania) musi zostać wdrożona w `HomeScreen` klasy, ponieważ jest to, gdy `MonoTouch.Dialog.BindingContext` jest tworzony. Następujące `HomeScreen` metody obsługują ekranu szczegóły zadania:

1.   **ShowTaskDetails** — tworzy `MonoTouch.Dialog.BindingContext` do renderowania na ekranie. Tworzy wejściowych ekranu przy użyciu odbicia można pobrać nazw właściwości i typów z `TaskDialog` klasy. Dodatkowe informacje, takie jak tekstu znaku wodnego pól wejściowych jest realizowana za pomocą atrybutów we właściwościach.
2.   **SaveTask** — odwołuje się do tej metody `TaskDialog` klasy za pomocą `OnTap` atrybutu. Jest wywoływana po **zapisać** naciśnięciu i używa `MonoTouch.Dialog.BindingContext` można pobrać danych wprowadzonych przez użytkownika przed zapisaniem zmian przy użyciu `TaskItemManager` .
3.   **DeleteTask** — odwołuje się do tej metody `TaskDialog` klasy za pomocą `OnTap` atrybutu. Używa `TaskItemManager` umożliwia usunięcie danych przy użyciu klucza podstawowego (właściwość ID).


 <a name="Android_App" />


# <a name="android-app"></a>Aplikacji systemu android

Kompletnego projektu platformy Xamarin.Android jest przedstawiony poniżej:

 ![](case-study-tasky-images/taskyandroid-solution.png "W tym miejscu przedstawiono projekt systemu android")

Diagram klas z klasami pogrupowane według warstwy:

 [ ![](case-study-tasky-images/classdiagram-android.png "Diagram klas z klasami pogrupowane według warstwy")](case-study-tasky-images/classdiagram-android.png)

 <a name="References" />


## <a name="references"></a>Odwołania

Projekt aplikacji systemu Android musi odwoływać się do zestawu platformy Xamarin.Android specyficzne dla platformy do klasy dostępu z zestawu SDK systemu Android.

Musi również odwoływać się do projektu PCL (np. TaskyPortableLibrary) dostęp do wspólnych kodu warstwy danych i biznesowych.

 ![](case-study-tasky-images/taskyandroid-references.png "TaskyPortableLibrary dostęp do wspólnych kodu warstwy danych i business")

 <a name="Application_Layer_(AL)" />


## <a name="application-layer-al"></a>Warstwa aplikacji (AL)

Podobnie jak wersja systemu iOS, które analizujemy wcześniej, warstwy aplikacji w wersji dla systemu Android zawiera specyficzne dla platformy klasy wymaganej do powiązania obiektów udostępnianych przez podstawowe do interfejsu użytkownika.

 **TaskListAdapter** — Aby wyświetlić listę<T> obiektów musimy zaimplementować adapter w celu wyświetlenia niestandardowych obiektów w `ListView`. Formanty karty układu, która jest używana dla każdego elementu na liście — w takim przypadku w kodzie za pomocą wbudowanego układu systemu Android `SimpleListItemChecked`.

 <a name="User_Interface_(UI)" />


## <a name="user-interface-ui"></a>Interfejs użytkownika (UI)

Warstwę interfejsu użytkownika dla aplikacji systemu Android to kombinacja kodu i znaczników XML.

 -   **Zasoby/układ** — układów ekranu głównego i wiersza komórki zaimplementowane jako AXML plików projektu. AXML mogą być napisane ręcznie lub określić wychodzący wizualnie przy użyciu narzędzia Projektant interfejsu użytkownika platformy Xamarin dla systemu Android.
 -   **Zasoby/Drawable** — obrazów (ikony) i przycisk niestandardowe.
 -   **Ekrany** — podklasy działania, definiujące każdego ekranu i jego zachowanie. Wiąże ze sobą interfejsu użytkownika z klasami warstwy aplikacji i wspólnego interfejsu API (`TaskItemManager`).


 <a name="Home_Screen" />


### <a name="home-screen"></a>Ekranu głównego

Na ekranie Home składa się z podklasę działania `HomeScreen` i `HomeScreen.axml` pliku, który definiuje układu (pozycja na liście przycisk i zadań). Ekran wygląda następująco:

 [ ![](case-study-tasky-images/android-taskylist.png "Ekran wygląda następująco")](case-study-tasky-images/android-taskylist.png)

Programy obsługi kliknięcie przycisku i klikając elementy na liście, a także podczas wypełniania listy definiuje kod macierzysty ekranu `OnResume` — metoda (tak że odzwierciedla zmiany dokonane w ekran szczegółów zadania). Dane są ładowane przy użyciu warstwie Business `TaskItemManager` i `TaskListAdapter` z warstwy aplikacji.

 <a name="Task_Details_Screen" />


### <a name="task-details-screen"></a>Ekran szczegółów zadania

Ekran szczegółów zadań również składa się z `Activity` podklasy i pliku AXML układu. Układ określa lokalizację kontrolki wejściowe i Klasa C# definiuje zachowania przy ładowaniu i zapisywaniu `TaskItem` obiektów.

 [ ![](case-study-tasky-images/android-taskydetail.png "Klasy definiuje zachowania przy ładowaniu i zapisywaniu TaskItem obiektów")](case-study-tasky-images/android-taskydetail.png)

Wszystkie odwołania do biblioteki PCL się za pośrednictwem `TaskItemManager` klasy.

 <a name="Windows_Phone_App" />


# <a name="windows-phone-app"></a>Aplikacji Windows Phone
Kompletnego projektu Windows Phone:

 ![](case-study-tasky-images/taskywp7-solution.png "Windows Phone App kompletnego projektu systemu Windows Phone")

Poniższy diagram przedstawia informacje o klasach podzielone na warstwy:

 [ ![](case-study-tasky-images/classdiagram-wp7.png "Ten diagram przedstawia informacje o klasach podzielone na warstwy")](case-study-tasky-images/classdiagram-wp7.png)

 <a name="References" />


## <a name="references"></a>Odwołania

Projekt specyficzne dla platformy musi odwoływać się wymaganych bibliotek specyficzne dla platformy (takich jak `Microsoft.Phone` i `System.Windows`) można utworzyć prawidłową aplikację Windows Phone.

Musi również odwoływać się do projektu PCL (np. `TaskyPortableLibrary`) mogą korzystać z `TaskItem` klasy i bazy danych.

 ![](case-study-tasky-images/taskywp7-references.png "TaskyPortableLibrary korzystanie z klasy TaskItem i bazy danych")

 <a name="Application_Layer_(AL)" />


## <a name="application-layer-al"></a>Warstwa aplikacji (AL)

Ponownie tak jak w przypadku wersji systemu Android i iOS warstwy aplikacji składa się z elementów Niewizualne pomagających wiązanie danych do interfejsu użytkownika.

 <a name="ViewModels" />


### <a name="viewmodels"></a>ViewModels

ViewModels zawijać danych z PCL ( `TaskItemManager`) i wyświetla go w sposób, który może być zużyte przez powiązanie danych Silverlight/XAML. Jest to przykład zachowanie specyficzne dla platformy (zgodnie z opisem w dokumencie i Platform aplikacji).

 <a name="User_Interface_(UI)" />


## <a name="user-interface-ui"></a>Interfejs użytkownika (UI)

XAML ma unikatowy możliwości wiązania danych, które mogą być deklarowane w znaczniku i zmniejszyć liczbę kod wymagany do wyświetlania obiektów:

1.   **Strony** — plików XAML i ich plik codebehind zdefiniować interfejs użytkownika i odwoływać ViewModels i projektu PCL, aby wyświetlić i zbierania danych.
2.   **Obrazy** — obrazy ekranu, tła i ikona powitalny odgrywają kluczową rolę interfejsu użytkownika.


 <a name="MainPage" />


### <a name="mainpage"></a>MainPage

Używa klasy MainPage `TaskListViewModel` do wyświetlania danych przy użyciu funkcji wiązania danych w języku XAML. Strony `DataContext` ma ustawioną wartość modelu widoku, który jest wypełniana asynchronicznie. `{Binding}` Składni w pliku XAML określa sposób wyświetlania danych.

 <a name="TaskDetailsPage" />


### <a name="taskdetailspage"></a>TaskDetailsPage

Każde zadanie jest wyświetlany przez powiązanie `TaskViewModel` w języku XAML, zdefiniowane w TaskDetailsPage.xaml. Dane zadania są pobierane za pośrednictwem `TaskItemManager` w warstwie biznesowej.

 <a name="Results" />


# <a name="results"></a>Wyniki

Wynikowa aplikacji wyglądać następująco na każdej platformie:

 <a name="iOS" />


### <a name="ios"></a>iOS

Aplikacja używa projektu interfejsu użytkownika systemu iOS — standard, takich jak przycisk "Dodaj" jest ustawiony na pasku nawigacyjnym i za pomocą wbudowanych **plus (+)** ikony. Również używa domyślnej `UINavigationController` przycisk "Wstecz", zachowania i obsługuje "Przejdź do Usuń" w tabeli.

 [ ![](case-study-tasky-images/ios-taskylist.png "Również stosowanie domyślnego zachowania przycisku Wstecz UINavigationController i obsługuje przejdź do usuwania w tabeli") ](case-study-tasky-images/ios-taskylist.png) [ ![ ] (case-study-tasky-images/ios-taskydetail.png "używa również domyślny UINavigationController kopii zachowanie przycisku i obsługuje przejdź do usuwania w tabeli")](case-study-tasky-images/ios-taskydetail.png)

 <a name="Android" />


### <a name="android"></a>Android

Android aplikacja korzysta z wbudowanych formantów w tym wbudowane układu dla wierszy, które wymagają "znaczników" wyświetlane. Zachowanie wstecz sprzętu/system jest obsługiwana oprócz przycisku Wstecz na ekranie.

 [ ![](case-study-tasky-images/android-taskylist.png "Zachowanie wstecz sprzętu/system jest obsługiwane oprócz przycisku Wstecz na ekranie") ](case-study-tasky-images/android-taskylist.png) [ ![ ] (case-study-tasky-images/android-taskydetail.png "zachowanie wstecz sprzętu/system jest obsługiwana w uzupełnieniu do na ekranie przycisk Wstecz")](case-study-tasky-images/android-taskydetail.png)

 <a name="Windows_Phone" />


### <a name="windows-phone"></a>Windows Phone

Aplikacji Windows Phone używa standardowego układu wypełnianie na pasku aplikacji w dolnej części ekranu zamiast paska nawigacji, u góry.

 [ ![](case-study-tasky-images/wp-taskylist.png "Aplikacja Windows Phone używa standardowego układu wypełnianie na pasku aplikacji w dolnej części ekranu zamiast paska nawigacji, w górnej") ](case-study-tasky-images/wp-taskylist.png) [ ![ ] (case-study-tasky-images/wp-taskydetail.png "aplikacji Windows Phone korzysta ze standardu Układ podczas wypełniania na pasku aplikacji w dolnej części ekranu zamiast paska nawigacji, u góry")](case-study-tasky-images/wp-taskydetail.png)

 <a name="Summary" />


# <a name="summary"></a>Podsumowanie

Niniejszy dokument jest udostępniany szczegółowe wyjaśnienie, jak zasady projektowania aplikacji warstwowych zostały zastosowane do prostej aplikacji w celu ułatwienia ponownego użycia kodu w trzech platform urządzeń przenośnych: systemy iOS, Android i Windows Phone.

Ma ona opisano proces projektowania warstwy aplikacji i opisem jaki kod &amp; funkcji zostało zaimplementowane w każdej warstwie.

Kod można pobrać z [github](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable).

## <a name="related-links"></a>Linki pokrewne

- [Tworzenie wieloplatformowych aplikacji (głównego dokumentu)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky przenośne przykładowej aplikacji (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
