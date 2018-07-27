---
title: Wprowadzenie do bibliotek klas przenośnych (PCL)
description: Ten artykuł wprowadza projektów biblioteki klas przenośnych (PCL) i zawiera opis sposobu tworzenia i używania projektów PCL w programie Visual Studio dla komputerów Mac i Visual Studio.
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 83b1da5cd10a46b8480b0755eeb16bf7434a5906
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270092"
---
# <a name="portable-class-libraries-pcl"></a>Biblioteki klas przenośnych (PCL)

> [!WARNING]
> Biblioteki klas przenośnych (PCLs) są uznawane za przestarzałe w najnowszych wersjach programu Visual Studio.
> Chociaż można nadal otwierać, edytować i skompilować PCLs, dla nowych projektów zaleca się używać [biblioteki .NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

Kluczowym elementem tworzenia aplikacji dla wielu platform jest możliwość udostępniania kodu w różnych projektach specyficzne dla platformy. Jednak jest to skomplikowane przez fakt, że różnych platformach często używają innego zestawu podrzędnych Biblioteka klasy podstawowej platformy .NET (BCL) i w związku z tym są w rzeczywistości skompilowano do innego profilu platformy .NET Core biblioteki. Oznacza to, że każdej z platform można używać tylko bibliotek klas, które są przeznaczone do tego samego profilu, dzięki czemu będą wyświetlane będą musieli osobnej klasy biblioteki projektów dla każdej platformy.

Istnieją trzy główne metody udostępniania kodu, które rozwiązują ten problem: **projektów .NET Standard**, **projektów udostępnionych zasobów**, i **projektów biblioteki klas przenośnych (PCL)**.

- **Projekty .NET standard** jest preferowanym podejściem do udostępniania kodu platformy .NET, przeczytaj więcej na temat [projektów .NET Standard i Xamarin](~/cross-platform/app-fundamentals/net-standard.md).
- **Udostępnione projekty zasobów** korzysta z pojedynczego zestawu plików i oferuje szybki i prosty sposób, w którym do udostępniania kodu, w ramach rozwiązania i ogólnie stosuje dyrektywy kompilacji warunkowej do określania ścieżek kodu dla różnych platform, który będzie używany (Aby uzyskać więcej informacji, zobacz [artykułu udostępnionych projektów](~/cross-platform/app-fundamentals/shared-projects.md)).
- **PCL** projekty ukierunkowane na konkretnych profilów, które obsługują znany zbiór BCL klasy/funkcji. Jednak dół stronie, aby PCL jest często potrzebują dodatkowych architektury starań, aby oddzielić profilu określonego kodu do własnej biblioteki.

Tej stronie wyjaśniamy, jak utworzyć **PCL** projektu, który jest przeznaczony dla określonego profilu, które następnie mogą być przywoływane przez wiele projektów specyficznych dla platformy.

## <a name="what-is-a-portable-class-library"></a>Co to jest Portable Class Library?

Po utworzeniu projektu aplikacji lub projektu biblioteki wynikowy DLL jest ograniczony do pracy z określonej platformy, który jest tworzony dla. Uniemożliwia to Zapisywanie zestawu dla aplikacji Windows, a następnie ponowne użycie na Xamarin.iOS i Xamarin.Android.

Podczas tworzenia biblioteki klas przenośnych, jednak można kombinacji platform, które chcesz, aby kod. Opcje zgodności, wprowadzone podczas tworzenia biblioteki klas przenośnych są tłumaczone na identyfikator "Profil", który zawiera opis platformy, które obsługuje biblioteki.

W poniższej tabeli przedstawiono niektóre funkcje, które zależą od platformy .NET. Aby napisać zestaw PCL, który gwarantuje działanie w określonych urządzeń/platform po prostu wybierz których obsługa jest wymagana podczas tworzenia projektu.

|Funkcja|.NET Framework|Aplikacje platformy uniwersalnej systemu Windows|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|Core|T|T|T|T|T|
|LINQ|T|T|T|T|T|
|IQueryable|T|T|T|7.5 +|T|
|Serializacja|T|T|T|T|T|
|Adnotacje danych|4.0.3 +|T|T||T|

Kolumna Xamarin odzwierciedla fakt, że Xamarin.iOS i Xamarin.Android obsługuje wszystkie profile, które są dostarczane z programem Visual Studio, a tylko jest ograniczona dostępność funkcji w bibliotekach, wszystkie utworzone przez innych platform, które chce obsługiwać.

Dane te obejmują profile, które są kombinacje:

- .NET 4 lub .NET 4.5
- Silverlight 5
- Windows Phone 8
- Aplikacje platformy uniwersalnej systemu Windows

Możesz dowiedzieć się więcej o możliwościach różnych profilów na [witrynę internetową firmy Microsoft](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx) zobaczyć inny Członek społeczności [profil PCL podsumowania](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) w tym obsługiwane informacje o framework i inne informacje.

**Korzyści**

1. Udostępnianie kodu scentralizowane — zapisu i przetestować kod w jednym projekcie, które mogą być używane przez inne biblioteki lub aplikacje.
2. Operacji refaktoryzacji, będzie miało wpływ na cały kod załadowanego w rozwiązaniu (Portable Class Library i projektów specyficznych dla platformy).
3. Można je było łatwo przywoływać projektu PCL przez inne projekty w rozwiązaniu lub zestawu wyjściowego można udostępniać innym osobom do odwołania w swoich rozwiązaniach.

**Wady**

1. Ponieważ ten sam Portable Class Library jest współużytkowana przez wiele aplikacji, nie może być przywoływany biblioteki charakterystyczne dla platformy (np. Community.CsharpSqlite.WP7).
2. Podzbiór biblioteki klas przenośnych nie może zawierać klasy, które w przeciwnym razie będą dostępne zarówno w przypadku MonoTouch, jak i narzędzie Mono dla systemu Android (na przykład DllImport lub System.IO.File).

> [!NOTE]
> Biblioteki klas przenośnych zostały zaniechane w najnowszej wersji programu Visual Studio i [standardowych bibliotek .NET](net-standard.md) zamiast tego zaleca się.

W pewnym stopniu może zostać ominięte zarówno wady kodu rzeczywistą implementację w projektach platformy względem interfejsu lub klasy bazowej, która jest zdefiniowana w Portable Class Library za pomocą wzorca dostawcy lub iniekcji zależności.

Ten diagram przedstawia architekturę aplikacji dla wielu platform przy użyciu biblioteki klas przenośnych do udostępniania kodu, ale również za pomocą iniekcji zależności do przekazania funkcji zależnych od platformy:

[![](pcl-images/image1.png "Ten diagram przedstawia architekturę aplikacji dla wielu platform przy użyciu biblioteki klas przenośnych do udostępniania kodu, ale również za pomocą iniekcji zależności do przekazania funkcji zależnych od platformy")](pcl-images/image1.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Program Visual Studio for Mac wskazówki

Ta sekcja zawiera szczegółowe instrukcje dotyczące tworzenia i używania biblioteki klas przenośnych przy użyciu programu Visual Studio dla komputerów Mac. Zobacz przykład PCL sekcji pełną implementację.

### <a name="creating-a-pcl"></a>Tworzenie aplikacji PCL

Dodawanie biblioteki klas przenośnych do rozwiązania jest bardzo podobne do dodawania regularne projektu biblioteki.

1. W **nowy projekt** okna dialogowego wybierz **dla wielu platform > biblioteki > Portable Library** opcji:

    ![Utwórz nowy projekt aplikacji PCL](pcl-images/image2.png)

1. Gdy aplikacji PCL zostanie utworzony w programie Visual Studio dla komputerów Mac jest automatycznie konfigurowany za pomocą profilu, który działa w przypadku Xamarin.iOS i Xamarin.Android. Projektu PCL pojawi się, jak pokazano w tym zrzucie ekranu:

    ![Projektu PCL w konsoli rozwiązania](pcl-images/image3.png)

PCL jest teraz gotowy do kodu, które mają zostać dodane. Mogą się też odwoływać przez inne projekty (projekty aplikacji, projekty biblioteki i nawet w innych projektów PCL).

### <a name="editing-pcl-settings"></a>Edytowanie ustawień aplikacji PCL

Aby wyświetlić i zmienić ustawienia aplikacji PCL dla tego projektu, kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Opcje > kompilacji > Ogólne** aby zobaczyć ekran, pokazano poniżej:

[![Opcje projektu PCL można ustawić profilu](pcl-images/image4-sml.png)](pcl-images/image4.png#lightbox)

Kliknij przycisk **zmian...**  zmienić profil docelowy dla tej biblioteki klas przenośnych.

Jeśli profil ulegnie zmianie po kod został już dodany do PCL, istnieje możliwość, biblioteki już nie zostanie skompilowany, jeśli kod odwołuje się do funkcji, które nie są częścią profilu nowo wybrany.

## <a name="working-with-a-pcl"></a>Praca z aplikacji PCL

Jeśli kod jest zapisana w bibliotece PCL, Visual Studio dla komputerów Mac edytora rozpozna ograniczenia wybranego profilu i odpowiednio dostosować opcje automatycznego uzupełniania. Na przykład ten zrzut ekranu przedstawia opcje autouzupełniania System.IO, przy użyciu domyślnego profilu (Profile136) używane w programie Visual Studio dla komputerów Mac — Zwróć uwagę, pasek przewijania, co oznacza połowa dostępnych klas są wyświetlane (w rzeczywistości istnieją tylko 14 klasy dostępne).

[![Lista IntelliSense 14 klas w klasie System.IO aplikacji PCL](pcl-images/image6.png)](pcl-images/image6.png#lightbox)

Porównaj z System.IO automatycznego uzupełniania w projekcie Xamarin.iOS lub Xamarin.Android — to Brak 40 klasy dostępne w tym powszechnie używane klasy, takie jak `File` i `Directory` które nie znajdują się w żadnym profilu PCL.

[![Lista IntelliSense 40 klas w przestrzeni nazw .NET Framework System.IO](pcl-images/image7.png)](pcl-images/image7.png#lightbox)

Ten sposób odzwierciedlono bazowego kompromis programu przy użyciu języka PCL — możliwość bezproblemowo udostępniaj kod na wielu platformach oznacza, że niektóre interfejsy API nie są dostępne dla Ciebie, ponieważ nie mają implementacji porównywalne na wszystkich platformach możliwe.

### <a name="using-pcl"></a>Przy użyciu języka PCL

Po utworzeniu projektu PCL, można dodać odwołanie do niej z poziomu dowolnej aplikacji zgodnych lub biblioteki projektu w taki sam sposób możesz normalnie należy dodać odwołania. W programie Visual Studio dla komputerów Mac, kliknij prawym przyciskiem myszy węzeł odwołania i wybierz polecenie **Edytuj odwołania...**  następnie przełącz się do **projektów** karty, jak pokazano:

[![Dodaj odwołanie do aplikacji PCL za pomocą opcji Edytuj odwołania](pcl-images/image8.png)](pcl-images/image8.png#lightbox)

Poniższy zrzut ekranu przedstawia Konsola rozwiązań dla aplikacji przykładowej TaskyPortable przedstawiający biblioteki PCL u dołu i odwołania do tej biblioteki PCL w projekcie platformy Xamarin.iOS.

[![TaskyPortable przykładowego rozwiązania przedstawiający PCL projektu](pcl-images/image9.png)](pcl-images/image9.png#lightbox)

Dane wyjściowe z aplikacji PCL (tj. zestaw wynikowy DLL) także może być dodany jako odwołanie do większości projektów. Dzięki temu PCL idealny sposób na potrzeby wysłania składników dla wielu platform i bibliotek.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Przewodnik usługi Visual Studio

Ta sekcja zawiera szczegółowe instrukcje dotyczące tworzenia i używania biblioteki klas przenośnych za pomocą programu Visual Studio. Zobacz przykład PCL sekcji pełną implementację.

### <a name="creating-a-pcl"></a>Tworzenie aplikacji PCL

Dodawanie aplikacji PCL do rozwiązania w programie Visual Studio różni się nieco na dodanie regularne projektu:

1. W **Dodaj nowy projekt** ekranu, wybierz opcję **biblioteki klas (starsza wersja przenośny)** opcji. Należy pamiętać, że opis po prawej stronie informacją o tym, że ten typ projektu jest przestarzała.

    [![Okno nowego projektu do tworzenia biblioteki klas przenośnych](pcl-images/image10-sml.png "Portable Class Library")](pcl-images/image10.png#lightbox)

2. Program Visual Studio wyświetli monit o natychmiast z następujące okno dialogowe, dzięki czemu można skonfigurować profil.
 Znaczników platform, których potrzebujesz do obsługi i naciśnij przycisk OK.

    [![Wybierz docelowe platformy dla biblioteki](pcl-images/image11-sml.png "znaczników platform potrzebne do obsługi i naciśnij przycisk OK")](pcl-images/image11.png#lightbox)

3. Projektu PCL pojawi, jak pokazano w Eksploratorze rozwiązań &ndash; tekst **(przenośny)** pojawia się obok nazwy projektu, aby wskazać, że jest on aplikacji PCL:

    ![NET Framework zdefiniowane przez profil PCL](pcl-images/image12.png "NET Framework zdefiniowane przez profil PCL")

PCL jest teraz gotowy do kodu, które mają zostać dodane. Mogą się też odwoływać przez inne projekty (projekty aplikacji, projekty biblioteki i nawet w innych projektów PCL).

### <a name="editing-pcl-settings"></a>Edytowanie ustawień aplikacji PCL

Można wyświetlać i zmienić, klikając prawym przyciskiem myszy na projekt i wybierając pozycję Ustawienia aplikacji PCL **właściwości > biblioteki** , jak pokazano na tym zrzucie ekranu:

[![Edytuj elementy docelowe platformy](pcl-images/image13-sml.png)](pcl-images/image13.png#lightbox)

Jeśli profil ulegnie zmianie po kod został już dodany do PCL, istnieje możliwość, biblioteki już nie zostanie skompilowany, jeśli kod odwołuje się do funkcji, które nie są częścią profilu nowo wybrany.

> [!TIP]
> Dostępna jest również komunikat, który wniosku **. NETStandard jest zalecaną metodą udostępniania kodu**. Jest to wskazanie, że podczas PCLs są nadal obsługiwane, zaleca się uaktualnienie do usługi .NET w warstwie standardowa.

### <a name="working-with-a-pcl"></a>Praca z aplikacji PCL

Jeśli kod jest zapisana w bibliotece PCL, Visual Studio rozpozna ograniczenia wybranego profilu i odpowiednio dostosować opcje Intellisense. Na przykład ten zrzut ekranu przedstawia opcje autouzupełniania System.IO, przy użyciu domyślnego profilu (Profile136) — Zwróć uwagę, pasek przewijania, co oznacza połowa dostępnych klas są wyświetlane (w rzeczywistości dostępnych tylko klasy 14).

[![Zmniejszenie liczby operacji We/Wy klasy dostępne w aplikacji PCL](pcl-images/image14.png)](pcl-images/image14.png#lightbox)

Porównaj z System.IO automatycznego uzupełniania w projekcie regularne — to Brak 40 klasy dostępne w tym powszechnie używane klasy, takie jak `File` i `Directory` które nie znajdują się w żadnym profilu PCL.

[![Wiele innych klas we/wy dostępnych w programie .NET Framework](pcl-images/image15.png)](pcl-images/image15.png#lightbox)

Ten sposób odzwierciedlono bazowego kompromis programu przy użyciu języka PCL — możliwość bezproblemowo udostępniaj kod na wielu platformach oznacza, że niektóre interfejsy API nie są dostępne dla Ciebie, ponieważ nie mają implementacji porównywalne na wszystkich platformach możliwe.

> [!TIP]
> .NET standard 2.0 reprezentuje znacznie większe obszarze powierzchni interfejsu API niż PCLs, włącznie z przestrzenią nazw System.IO. Dla nowych projektów .NET Standard zamiast zaleca PCL.

### <a name="using-pcl"></a>Przy użyciu języka PCL

Po utworzeniu projektu PCL, można dodać odwołanie do niej z poziomu dowolnej aplikacji zgodnych lub biblioteki projektu w taki sam sposób możesz normalnie należy dodać odwołania. W programie Visual Studio, kliknij prawym przyciskiem myszy węzeł odwołania i wybierz polecenie `Add Reference...` następnie przełącz się do **rozwiązania > Projekty** karty, jak pokazano:

[![Dodaj odwołanie do aplikacji PCL za pośrednictwem karty dodawanie projektów z odwołaniem](pcl-images/image16.png)](pcl-images/image16.png#lightbox)

Poniższy zrzut ekranu przedstawia w okienku rozwiązania TaskyPortable przykładową aplikację, przedstawiający biblioteki PCL u dołu i odwołania do tej biblioteki PCL w projekcie platformy Xamarin.iOS.

[![TaskyPortable przykładowe rozwiązanie przedstawiający biblioteki PCL](pcl-images/image17.png)](pcl-images/image17.png#lightbox)

Dane wyjściowe z aplikacji PCL (tj. zestaw wynikowy DLL) także może być dodany jako odwołanie do większości projektów.
Dzięki temu PCL idealny sposób na potrzeby wysłania składników dla wielu platform i bibliotek.

-----

## <a name="pcl-example"></a>Przykład PCL

[TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/) Przykładowa aplikacja demonstruje, jak używać biblioteki klas przenośnych za pomocą platformy Xamarin.
Poniżej przedstawiono niektóre zrzuty ekranu przedstawiające wynikowy aplikacje uruchomione w systemach iOS i Android:

[![](pcl-images/image18.png "Poniżej przedstawiono niektóre zrzuty ekranu, wynikowy aplikacji uruchomionej w systemie iOS, Android i Windows Phone")](pcl-images/image18.png#lightbox)

Współdzieli wiele klas danych i logiki, które są wyłącznie przenośny kod, a ilustruje też sposób wprowadzić wymagania specyficzne dla platformy, przy użyciu iniekcji zależności dla implementacji bazy danych SQLite.

Poniżej przedstawiono strukturę rozwiązania (w programie Visual Studio dla komputerów Mac i Visual Studio odpowiednio):

[![](pcl-images/image19.png "Struktura rozwiązania jest wyświetlany w tym miejscu w programie Visual Studio dla komputerów Mac i Visual Studio odpowiednio")](pcl-images/image19.png#lightbox)

Ponieważ kod SQLite NET ma elementy specyficzne dla platformy (w celu pracy z implementacjami SQLite na każdy inny system operacyjny) pokaz celów go ma zostały zaprojektowane od nowa do abstrakcyjnego klasy, które mogą być skompilowane w ramach Portable Class Library, i Rzeczywisty kod zaimplementowany jako podklasy w projektów dla systemu Android i iOS.

### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

Portable Class Library ma ograniczone funkcje platformy .NET, które może obsługiwać. Ponieważ jest ona kompilowana do uruchamiania na wielu platformach, nie może wprowadzać użytkowania `[DllImport]` funkcje używane w sieci, bazy danych SQLite. Zamiast tego SQLite NET jest implementowany jako klasa abstrakcyjna, a następnie przywoływany przez pozostałą część udostępnionego kodu. Wyodrębnij abstrakcyjny interfejs API jest pokazany poniżej:

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

W pozostałej części udostępnionego kodu używa klasy abstrakcyjnej "przechowywać" i "pobierania" obiektów z bazy danych. W dowolnej aplikacji, która korzysta z tej klasy abstrakcyjnej muszą możemy przekazać pełną implementację, która udostępnia funkcje bazy danych.

### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid i TaskyiOS

Projektów aplikacji dla systemu Android i iOS zawierają interfejs użytkownika i innego kodu specyficznego dla platformy, używany do udostępnionego kodu w aplikacji PCL w górę o komunikacji sieciowej.

Te projekty zawierają również implementację abstrakcyjnej bazy danych interfejsu API, który działa na tej platformie. W systemach iOS i Android Sqlite aparatu bazy danych jest wbudowana w system operacyjny dzięki implementacji za pomocą `[DllImport]` jak pokazano w celu zapewnienia konkretną implementację łączność z bazą danych. Fragment kodu implementację specyficzną dla platformy jest następujący:

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```

W przykładowym kodzie widać pełną implementację.

## <a name="summary"></a>Podsumowanie

Ten artykuł ma krótko omówiono korzyści i pułapek przenośnej biblioteki klas, pokazano sposób tworzenia i wykorzystywania PCLs z poziomu programu Visual Studio dla komputerów Mac i Visual Studio; i wreszcie wprowadzono pełną przykładową aplikację — TaskyPortable — pokazuje aplikacji PCL w działaniu.

## <a name="related-links"></a>Linki pokrewne

- [TaskyPortable (przykład)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Tworzenie aplikacji międzyplatformowych](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Przenośne języka Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [Projekty udostępnione](~/cross-platform/app-fundamentals/shared-projects.md)
- [Opcje udostępniania kodu](~/cross-platform/app-fundamentals/code-sharing.md)
- [Programowanie dla wielu Platform za pomocą programu .NET Framework (Microsoft)](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)
