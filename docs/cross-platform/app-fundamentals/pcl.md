---
title: "Wprowadzenie do bibliotek klas przenośnych"
description: "W tym artykule przedstawiono projekty przenośnych klasy biblioteki PCL () oraz przeprowadzi Cię przez tworzenia i używania PCL projekty w programie Visual Studio for Mac i Visual Studio."
ms.topic: article
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e3701960f246a8f627d991edf244656b5fd8958e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-portable-class-libraries"></a>Wprowadzenie do bibliotek klas przenośnych

_W tym artykule przedstawiono projekty przenośnych klasy biblioteki PCL () oraz przeprowadzi Cię przez tworzenia i używania PCL projekty w programie Visual Studio for Mac i Visual Studio._


Kluczowym elementem tworzenia aplikacji dla wielu platform jest możliwość udostępnianie kodu w różnych projektów specyficzne dla platformy. Jest to jednak złożona przez fakt, że różne platformy często Użyj innego zbioru podrzędne biblioteki klasy podstawowej platformy .NET (BCL) i w związku z tym faktycznie są przeznaczone do innego profilu .NET Core biblioteki. Oznacza to, że każdej platformy można używać tylko biblioteki klas, które są przeznaczone do tego samego profilu, więc pojawią się one wymagać projektów biblioteki klas osobne dla każdej platformy.

Istnieją trzy główne metody udostępniania kodu rozwiązania tego problemu: **.NET Standard projekty**, **projekty przenośnych klasy biblioteki PCL ()**, i **zasobów udostępnionych projektów**.

- **Projekty platformy .NET standard** [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md).
-  **PCL** projektów docelowych określonych profilów, które obsługuje zestaw znanych BCL klasy/funkcji. Jednak dół strony na PCL jest często potrzebują bardzo architektury starań, aby podzielić kod określonego profilu na ich własnych bibliotek. Aby uzyskać szczegółowe omówienie na te dwie metody, zobacz [przewodniku opcji udostępniania kodu](~/cross-platform/app-fundamentals/code-sharing.md) .
-  **Udostępnionych projektów zasobów** używać jednego zestawu plików i oferty szybki i prosty sposób umożliwiający udostępnianie kodu w ramach rozwiązania i zazwyczaj stosuje kompilacja warunkowa dyrektywy do określenia ścieżki kodu dla różnych platform, które będą korzystać (Aby uzyskać więcej informacji, zobacz [artykułu udostępnionych projektów](~/cross-platform/app-fundamentals/shared-projects.md) i [Konfigurowanie przewodnik rozwiązania i Platform Xamarin](~/cross-platform/app-fundamentals/code-sharing.md) ).


Ta strona wyjaśnia sposób tworzenia **PCL** projektu przeznaczonego dla określonego profilu można odwoływać się do wielu projektów specyficzne dla platformy.

## <a name="requirements"></a>Wymagania

Projektów bibliotek przenośnych są automatycznie włączone w programie Visual Studio dla komputerów Mac na macOS i są wbudowane w Visual Studio 2013 lub nowszego.


## <a name="what-is-a-portable-class-library"></a>Co to jest przenośnej biblioteki klas?

Po utworzeniu projektu aplikacji lub projektu biblioteki wynikowa Biblioteka DLL jest ograniczone do pracy na danej platformie, jest tworzona dla. Uniemożliwia to Zapisywanie zestawu aplikacji systemu Windows, a następnie ponowne wykorzystanie go na Xamarin.iOS i platformy Xamarin.Android.

Podczas tworzenia biblioteki klas przenośnych, jednak można wybrać kombinację platformy, które mają kodu do uruchomienia na. Opcji zgodności wprowadzone podczas tworzenia biblioteki klas przenośnych są przekształcane na identyfikator "Profilu", który opisuje platformy, które obsługuje biblioteki.

W poniższej tabeli przedstawiono niektóre funkcje, które zależą od platformy .NET. Można zapisać zestawu PCL, który działanie na urządzeniach określonych/platform możesz po prostu wybierz których obsługa jest wymagana podczas tworzenia projektu.

<table border="1" cellpadding="1" cellspacing="1">
  <tbody>
    <tr>
      <td>
Funkcja </td>
      <td>
.NET Framework </td>
      <td>
Aplikacji platformy uniwersalnej systemu Windows </td>
      <td>
Silverlight </td>
      <td>
Windows Phone </td>
      <td>
Xamarin </td>
    </tr>
    <tr>
      <td>
Core </td>
      <td>
T </td>
      <td>
T </td>
      <td>
T </td>
      <td>
T </td>
      <td>
T </td>
    </tr>
    <tr>
      <td>
LINQ </td>
      <td>
T </td>
      <td>
T </td>
      <td>
T </td>
      <td>
T </td>
      <td>
T </td>
    </tr>
    <tr>
      <td>
IQueryable </td>
       <td>
T </td>
      <td>
T </td>
      <td>
T </td>
      <td>
7.5 + </td>
      <td>
T </td>
    </tr>
    <tr>
      <td>
Serializacja </td>
      <td>
T </td>
      <td>
T </td>
      <td>
T </td>
      <td>
T </td>
      <td>
T </td>
    </tr>
    <tr>
      <td>
Adnotacji danych </td>
      <td>
4.0.3 + </td>
      <td>
T </td>
      <td>
T </td>
      <td>
      </td>
      <td>
T </td>
    </tr>
  </tbody>
</table>

Fakt, że Xamarin.iOS i Xamarin.Android obsługuje wszystkie profile wysłane z programu Visual Studio 2013 lub nowszym i dostępność funkcji w żadnych bibliotek, które możesz utworzyć tylko będzie ograniczony przez platformy, które mają zostać odzwierciedla kolumny Xamarin Obsługa.

Obejmuje to profilów, które są kombinacje:

-  .NET 4 lub .NET 4.5
-  Silverlight 5
-  Windows Phone 8
-  Aplikacji platformy uniwersalnej systemu Windows

Więcej o możliwościach różne profile na [witryny sieci Web firmy Microsoft](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx) i zobacz innego członka społeczności [podsumowanie profilu PCL](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) w tym obsługiwane framework informacji oraz inne wybrane uwagi.



Tworzenie PCL udostępnianie kodu ma wiele zalet i wad i alternatywne łączenie plików:


**Korzyści**

1. Udostępnianie kodu scentralizowane — pisania i testowania kodu w jednym projekcie, które mogą być używane przez inne biblioteki lub aplikacje.
1. Refaktoryzacja operacji będzie miało wpływ na cały kod załadowanego w rozwiązaniu (przenośnej biblioteki klas i projekty specyficzne dla platformy).
1. Projekt PCL można łatwo odwoływać się do innych projektów w rozwiązaniu lub zestawu wyjściowego można udostępniać innym użytkownikom odwołuje się w ich rozwiązania.


**Wady**

1. Ponieważ tego samego przenośnej biblioteki klas są współużytkowane przez wiele aplikacji, nie może być przywoływany bibliotek specyficzne dla platformy (np. Community.CsharpSqlite.WP7).
1. Podzbiór przenośnej biblioteki klas nie może zawierać klasy, które w przeciwnym razie będą dostępne zarówno MonoTouch, jak i Mono dla systemu Android (na przykład DllImport lub System.IO.File).



W pewnym stopniu można obejść wady obu rzeczywiste implementacją w projektach platformy względem interfejsu lub klasy podstawowej zdefiniowanej w przenośnej biblioteki klas w kodzie za pomocą wzorca dostawcy lub iniekcji zależności.



Ten diagram przedstawia architekturę i platform aplikacji korzystanie z przenośnej biblioteki klas udostępnianie kodu, lecz również przy użyciu iniekcji zależności do przekazania w funkcje zależne od platformy:



[![](pcl-images/image1.png "Ten diagram przedstawia architekturę i platform aplikacji korzystanie z przenośnej biblioteki klas udostępnianie kodu, lecz również przy użyciu iniekcji zależności do przekazania w funkcje zależne od platformy")](pcl-images/image1.png)



# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)



## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac Walkthrough


W tej sekcji przedstawiono sposób tworzenia i używania biblioteki klas przenośnych przy użyciu programu Visual Studio dla komputerów Mac. Zobacz na przykład PCL sekcji zakończenia wdrożenia.



### <a name="creating-a-pcl"></a>Tworzenie PCL


Dodawanie biblioteki klas przenośnych do rozwiązania jest bardzo podobne do dodawania regularne projektu biblioteki.


1. W oknie dialogowym Nowy projekt wybierz `Multiplatform > Library > Portable Library` opcji


    ![](pcl-images/image2.png "Multiplatform > Biblioteka > przenośnej biblioteki")


1. PCL utworzenia w programie Visual Studio for Mac jest automatycznie konfigurowany za pomocą profilu dla platformy Xamarin.iOS i platformy Xamarin.Android. Projekt PCL pojawi się jak pokazano w tym zrzut ekranu:

    ![](pcl-images/image3.png "Projekt PCL pojawi się jak pokazano w tym zrzut ekranu")

PCL jest teraz gotowy do kodu do dodania. Mogą się też odwoływać linie innych projektów (projekty aplikacji, projekty bibliotek i nawet innych projektów PCL).



### <a name="editing-pcl-settings"></a>Edytowanie ustawień PCL


Aby wyświetlić i zmienić ustawienia PCL dla tego projektu, kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Opcje > kompilacji > Ogólne** wyświetlić ekranu pokazano poniżej:



[![](pcl-images/image4.png "Aby wyświetlić i zmienić ustawienia PCL dla tego projektu, kliknij prawym przyciskiem myszy projekt i wybierz polecenie Opcje kompilacji ogólne, aby zobaczyć ekran pokazane")](pcl-images/image4.png)



Tych ustawień na tym ekranie można określić platformy, na których jest zgodny z tym konkretnym PCL. Zmiana któregoś z tych opcji zmienia profil używany przez ten PCL, który z kolei kontroluje, jakie funkcje mogą być używane w kodzie przenośnej.



Zmiana któregoś z `Target Framework` opcje automatycznie aktualizuje `Current Profile`; ekranu będą również wyświetlane ostrzeżenie, jeśli zostanie wybrana opcja niezgodne.



[![](pcl-images/image5.png "Zmiana opcji platformy docelowej automatycznie aktualizuje bieżący profil ekranu będą również wyświetlane ostrzeżenie, jeśli zostanie wybrana opcja niezgodne")](pcl-images/image5.png)



Zmiana profilu po kodu został już dodany do PCL prawdopodobnie biblioteki już zostanie skompilowany, jeśli kod odwołuje się do funkcji, które nie są częścią profilu nowo zaznaczone.


## <a name="working-with-a-pcl"></a>Praca z PCL


Jeśli kod jest zapisana w bibliotece PCL, programu Visual Studio for Mac Edytor rozpozna ograniczenia wybranego profilu i odpowiednio zmienić opcje automatycznego zakończenia. Na przykład ten zrzut ekranu przedstawia opcje automatycznego zakończenia System.IO, przy użyciu domyślnego profilu (Profile136) używane w programie Visual Studio dla komputerów Mac — zauważyć pasek przewijania, wskazujący, są wyświetlane w połowie z dostępnych klas (faktycznie istnieją tylko 14 klasy dostępne).



[![](pcl-images/image6.png "We/Wy przy użyciu domyślnego profilu Profile136 używane w programie Visual Studio dla adnotacji Mac pasek przewijania, wskazujący w połowie z dostępnych klas w rzeczywistości są wyświetlane są tylko klasy 14 dostępne")](pcl-images/image6.png)



Porównaj z System.IO funkcja automatycznego uzupełniania w projekcie platformy Xamarin.iOS lub Xamarin.Android — czy 40 klasy dostępne w tym często używane klasy takie jak `File` i `Directory` które nie są w żadnym profilu PCL.



[![](pcl-images/image7.png "Brak klasy 40 dostępne w tym powszechnie używane klas takich jak plików i katalogów, które nie są w żadnym profilu PCL")](pcl-images/image7.png)



Odzwierciedla podstawowej zależność użycia PCL — umożliwiają łatwe udostępnianie kod na wielu platformach oznacza, że niektórych interfejsów API nie są dostępne, ponieważ nie mają implementacje porównywalne na wszystkich platformach możliwe.



### <a name="using-pcl"></a>Przy użyciu PCL


Po utworzeniu projektu PCL, można dodać odwołania do niego z dowolnego zgodnego projektu aplikacji lub biblioteki w taki sam sposób, zwykle Dodaj odwołania. W programie Visual Studio for Mac, kliknij prawym przyciskiem myszy w węźle odwołania, a następnie wybierz pozycję `Edit References…` następnie przejść do karty projektów, jak pokazano:



[![](pcl-images/image8.png "W programie Visual Studio for Mac kliknij prawym przyciskiem myszy węzeł odniesienia i wybierz pozycję Edytuj odwołania, a następnie przejść do karty projektów, jak pokazano")](pcl-images/image8.png)



Poniższy zrzut ekranu przedstawia konsoli rozwiązanie dla aplikacji przykładowej TaskyPortable przedstawiający biblioteki PCL na dole i odwołania do tej biblioteki PCL w projekcie platformy Xamarin.iOS.



[![](pcl-images/image9.png "Konsola rozwiązania dla TaskyPortable przykładowej aplikacji")](pcl-images/image9.png)



Dane wyjściowe z PCL (tj. wynikowego zestawu biblioteki DLL) można również dodać jako odwołanie do większości projektów. Dzięki temu PCL idealny na potrzeby wysłania wieloplatformowych składniki i biblioteki.




# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)




## <a name="visual-studio-walkthrough"></a>Visual Studio Walkthrough


W tej sekcji przedstawiono sposób tworzenia i używania biblioteki klas przenośnych przy użyciu programu Visual Studio. Zobacz na przykład PCL sekcji zakończenia wdrożenia.



### <a name="creating-a-pcl"></a>Tworzenie PCL


Dodawanie PCL do rozwiązania w programie Visual Studio różni się nieznacznie się dodanie regularne projektu.



1. Na ekranie Dodawanie nowego projektu, zaznacz `Portable Class Library` opcji


    ![](pcl-images/image10.png "Biblioteka klas przenośnych")


1. Visual Studio natychmiast wyświetli monit o z następujących okna dialogowego, dzięki czemu można skonfigurować profil.
 Znaczników platformy potrzebnych do obsługi i naciśnij przycisk OK.


    ![](pcl-images/image11.png "Znaczników platformy potrzebnych do obsługi i naciśnij przycisk OK")


1. Projekt PCL będą wyświetlane, jak pokazano w Eksploratorze rozwiązań. Węzła odwołań wskaże, że biblioteka korzysta z podzbioru programu .NET Framework (zdefiniowane przez profil PCL).

    ![](pcl-images/image12.png "NET Framework zdefiniowane przez profil PCL")

PCL jest teraz gotowy do kodu do dodania. Mogą się też odwoływać linie innych projektów (projekty aplikacji, projekty bibliotek i nawet innych projektów PCL).



### <a name="editing-pcl-settings"></a>Edytowanie ustawień PCL


Ustawienia PCL można wyświetlać i zmieniony przez kliknięcie prawym przyciskiem myszy projekt i wybierając polecenie **właściwości > biblioteki** , jak pokazano w tym zrzut ekranu:



[![](pcl-images/image13.png "Ustawienia PCL można wyświetlać i zmienić prawym przyciskiem myszy projekt i wybierając pozycję Właściwości biblioteki, jak pokazano w tym zrzut ekranu")](pcl-images/image13.png)



Zmiana profilu po kodu został już dodany do PCL prawdopodobnie biblioteki już zostanie skompilowany, jeśli kod odwołuje się do funkcji, które nie są częścią profilu nowo zaznaczone.



### <a name="working-with-a-pcl"></a>Praca z PCL


Jeśli kod jest zapisana w bibliotece PCL, Visual Studio rozpozna ograniczenia wybranego profilu i odpowiednio zmienić opcje Intellisense. Na przykład ten zrzut ekranu przedstawia opcje automatycznego zakończenia System.IO, przy użyciu domyślnego profilu (Profile136) — zauważyć pasek przewijania, wskazujący, są wyświetlane w połowie z dostępnych klas (w rzeczywistości dostępnych tylko klasy 14).



[![](pcl-images/image14.png "We/Wy przy użyciu domyślnego profilu Profile136")](pcl-images/image14.png)



Porównaj z System.IO funkcja automatycznego uzupełniania w projekcie regularnych — czy 40 klasy dostępne w tym często używane klasy takie jak `File` i `Directory` które nie są w żadnym profilu PCL.



[![](pcl-images/image15.png "Funkcja automatycznego uzupełniania w regularnych projektu")](pcl-images/image15.png)



Odzwierciedla podstawowej zależność użycia PCL — umożliwiają łatwe udostępnianie kod na wielu platformach oznacza, że niektórych interfejsów API nie są dostępne, ponieważ nie mają implementacje porównywalne na wszystkich platformach możliwe.



### <a name="using-pcl"></a>Przy użyciu PCL


Po utworzeniu projektu PCL, można dodać odwołania do niego z dowolnego zgodnego projektu aplikacji lub biblioteki w taki sam sposób, zwykle Dodaj odwołania. W programie Visual Studio, kliknij prawym przyciskiem myszy w węźle odwołania, a następnie wybierz pozycję `Add Reference...` następnie przełącz się do **rozwiązania: projekty** karcie pokazany:



[![](pcl-images/image16.png "Karta projekty, jak pokazano")](pcl-images/image16.png)



Poniższy zrzut ekranu przedstawia okienku rozwiązanie dla aplikacji przykładowej TaskyPortable przedstawiający biblioteki PCL na dole i odwołania do tej biblioteki PCL w projekcie platformy Xamarin.iOS.



[![](pcl-images/image17.png "W okienku rozwiązania TaskyPortable przykładowej aplikacji")](pcl-images/image17.png)



Dane wyjściowe z PCL (tj. wynikowego zestawu biblioteki DLL) można również dodać jako odwołanie do większości projektów.
Dzięki temu PCL idealny na potrzeby wysłania wieloplatformowych składniki i biblioteki.




-----



## <a name="pcl-example"></a>Przykład PCL


[TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/) przedstawiono przykładową aplikację przenośną bibliotekę klas możliwości korzystania z platformą Xamarin.
Poniżej przedstawiono niektóre zrzuty ekranu wynikowy aplikacje uruchomione w systemach iOS, Android i Windows Phone:



[![](pcl-images/image18.png "Poniżej przedstawiono niektóre zrzuty ekranu wynikowy aplikacje uruchomione w systemach iOS, Android i Windows Phone")](pcl-images/image18.png)



Współużytkuje liczbę danych i logiki klas, które są całkowicie przenośnego kodu i również przedstawiono sposób dodawania wymagań specyficznych dla platformy przy użyciu iniekcji zależności dla implementacji bazy danych SQLite.




Poniżej przedstawiono struktury rozwiązania (w programie Visual Studio for Mac i Visual Studio odpowiednio):



[![](pcl-images/image19.png "Struktura rozwiązania jest tutaj wyświetlane w Visual Studio for Mac i Visual Studio odpowiednio")](pcl-images/image19.png)



Ponieważ kod SQLite NET ma elementy specyficzne dla platformy (do pracy z implementacji SQLite na każdy inny system operacyjny) dla pokaz celów ma został zrefaktoryzowany do abstrakcyjnego klasy, która może być łączone w przenośnej biblioteki klas, i Rzeczywisty kod zaimplementowane jako podklasy w projektów dla systemu Android i iOS.



### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

Funkcje platformy .NET, które może obsługiwać przenośnej biblioteki klas jest ograniczony. Ponieważ jest on skompilowany do uruchamiania na wielu platformach, nie można wprowadzić użycie `[DllImport]` funkcje używane w sieci, SQLite. Zamiast tego SQLite NET jest zaimplementowany jako klasa abstrakcyjna, a następnie odwołania do przez pozostałą udostępnionego kodu. Poniżej przedstawiono wyodrębniania abstrakcyjny interfejsu API:


```csharp
public abstract class SQLiteConnection : IDisposable {

    public string DatabasePath { get; private set; }
    public bool TimeExecution { get; set; }
    public bool Trace { get; set; }
    public SQLiteConnection(string databasePath) {
         DatabasePath = databasePath;
    }
    public abstract int CreateTable<T>();
    public abstract SQLiteCommand CreateCommand(string cmdText, params object[] ps);
    public abstract int Execute(string query, params object[] args);
    public abstract List<T> Query<T>(string query, params object[] args) where T : new();
    public abstract TableQuery<T> Table<T>() where T : new();
    public abstract T Get<T>(object pk) where T : new();
    public bool IsInTransaction { get; protected set; }
    public abstract void BeginTransaction();
    public abstract void Rollback();
    public abstract void Commit();
    public abstract void RunInTransaction(Action action);
    public abstract int Insert(object obj);
    public abstract int Update(object obj);
    public abstract int Delete<T>(T obj);

    public void Dispose()
    {
        Close();
    }
    public abstract void Close();

}
```


W pozostałej części udostępniony kod używa klasy abstrakcyjnej "store" i "pobieranie" obiektów z bazy danych. W dowolnej aplikacji, która korzysta z tej klasy abstrakcyjnej możemy przekazać w zakończenia wdrożenia, który udostępnia funkcjonalność rzeczywistej bazy danych.



### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid i TaskyiOS


Projektów aplikacji systemu Android i iOS zawierają interfejs użytkownika i innego kodu specyficzne dla platformy, używany do udostępnionego kodu w PCL przewodowy w górę.



Te projekty również zawierać implementację abstrakcyjnej bazy danych interfejsu API, który działa na tej platformie. W systemach iOS i Android Sqlite aparatu bazy danych jest wbudowana w system operacyjny, więc można używać implementacji `[DllImport]` pokazany Podaj konkretną implementację łączność z bazą danych. Fragment kodu implementacja specyficzna dla platformy jest następujący:


```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```


W przykładowym kodzie zostanie wyświetlona pełnego wdrożenia.

### <a name="taskywinphone"></a>TaskyWinPhone


Aplikacji Windows Phone ma jego interfejsie użytkownika skompilowanych przy użyciu języka XAML i zawiera inny kod specyficzne dla platformy, aby połączyć obiekty udostępnione przy użyciu interfejsu użytkownika.



W przeciwieństwie do implementacji używany dla systemów iOS i Android, należy utworzyć i użyć wystąpienia aplikacji Windows Phone `Community.Sqlite.dll` jako część jej abstrakcyjny bazy danych interfejsu API. Zamiast przy użyciu `DllImport`, metody, takie jak otwieranie są wykonywane względem zestawu Community.Sqlite, który odwołuje się do `TaskWinPhone` projektu. W tym miejscu przedstawiono fragment do porównania z systemów iOS i Android w wersji powyżej


```csharp
public static Result Open(string filename, out Sqlite3.sqlite3 db)
{
    db = new Sqlite3.sqlite3();
    return (Result)Sqlite3.sqlite3_open(filename, ref db);
}

public static Result Close(Sqlite3.sqlite3 db)
{
    return (Result)Sqlite3.sqlite3_close(db);
}
```


## <a name="summary"></a>Podsumowanie


W tym artykule ma krótko omówione korzyści i problemów przenośnej biblioteki klas, przedstawiono sposób tworzenia i zużywać PCLs od w programie Visual Studio for Mac i Visual Studio; i na koniec wprowadzono pełną przykładową aplikację — TaskyPortable — zawiera PCL w akcji.


## <a name="related-links"></a>Linki pokrewne

- [TaskyPortable (przykład)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Tworzenie aplikacji międzyplatformowych](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Przenośne Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [Projekty udostępnione](~/cross-platform/app-fundamentals/shared-projects.md)
- [Opcje udostępniania kodu](~/cross-platform/app-fundamentals/code-sharing.md)
- [Programowanie wieloplatformowych aplikacji za pomocą programu .NET Framework (Microsoft)](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
