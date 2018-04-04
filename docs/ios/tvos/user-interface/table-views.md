---
title: Praca z widokami tabeli
description: Ten artykuł obejmuje projektowanie i Praca z tabel, widoków i kontrolerów widok tabeli wewnątrz aplikacji Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8864e85e4d657fc242f6c06b21c815f62055c9f4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-table-views"></a>Praca z widokami tabeli

_Ten artykuł obejmuje projektowanie i Praca z tabel, widoków i kontrolerów widok tabeli wewnątrz aplikacji Xamarin.tvOS._

W systemu tvOS widok tabeli jest przedstawiany jako pojedynczej kolumny przewijanie wierszy, na które opcjonalnie można podzielić na grupy lub sekcje. Widoki tabeli powinien być używany, gdy konieczne jest wyświetlenie dużej ilości danych wydajnie użytkownikowi w Wyczyść, aby zrozumieć sposób.

Widoki tabel zwykle są wyświetlane na jednej stronie [podzielony widok](~/ios/tvos/user-interface/split-views.md) jako nawigacji ze szczegółami wybranego elementu wyświetlany w przeciwną stronę:

[![](table-views-images/intro01.png "Przykładowy widok tabeli")](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>Widoki tabeli — informacje

A `UITableView` pojedynczej kolumny przewijanego wierszy będzie wyświetlany jako listę hierarchiczną informacje, które mogą być opcjonalnie zorganizowane w grupy lub sekcje: 

[![](table-views-images/table01.png "Wybrany element")](table-views-images/table01.png#lightbox)

Apple ma poniższe sugestie dotyczące pracy z tabelami:

- **Należy pamiętać o szerokości** -spróbować znaleźć poprawne saldo w szerokości tabeli. Jeśli tabela jest zbyt długa, może być trudne do skanowania z odległości i może trwać od dostępny obszar zawartości. Jeśli tabela jest zbyt ograniczone, może to spowodować informacji do skrócenia lub zawijania ponownie to może być trudne dla użytkowników do odczytu w pomieszczeniu.
- **Pokaż zawartość tabeli szybko** — dużych list danych opóźnionego ładowania zawartości i rozpoczęcia wyświetlania informacji jak tabela jest widoczne dla użytkownika. Tabela przyjmuje za długa, aby załadować, może spowodować utratę zainteresowanie aplikację lub pomyśleć, który jest zablokowany się przez użytkownika.
- **Powiadamia użytkownika o długich zawartości ładunków** — Jeśli czas ładowania długa tabela jest niezbędne, przedstawia [pasek postępu lub wskaźnika działania](~/ios/tvos/user-interface/progress-indicators.md) nie zablokowane, aby wiedzieli aplikacji.

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>Typy komórki w widoku tabeli

A `UITableViewCell` jest używana do reprezentowania poszczególnych wierszy danych w widoku tabeli. Apple zdefiniowano kilka domyślnych typów komórki tabeli:

- **Domyślna** — ten typ przedstawia informacje o liczbie opcję Obraz po lewej stronie komórki i tytuł wyrównany po prawej stronie. 
- **Podtytuł** — ten typ przedstawia informacje o liczbie tytuł wyrównane do lewej w pierwszym wierszu i mniejszej wyrównane do lewej podtytuł w następnym wierszu.
- **Wartość 1** — ten typ przedstawia tytuł wyrównany z pomocniczą jaśniejsze kolorowe, wyrównany do prawej w tym samym wierszu.
- **Wartość 2** — ten typ przedstawia tytuł wyrównany z pomocniczą jaśniejsze kolorowe, wyrównane do lewej w tym samym wierszu.

Wszystkie typy komórki widok tabeli domyślne obsługują także graficznego elementów, takich jak ujawnianie wskaźników lub znaczniki wyboru. 

Ponadto można zdefiniować **niestandardowy** komórki widok tabeli i obecnie _komórki prototypu_, albo tworzonych w Projektancie interfejsu, lub za pomocą kodu.

Apple ma poniższe sugestie dotyczące pracy z komórek w widoku tabeli:

- **Unikaj wycinka tekst** -zachować poszczególne wiersze tekstu krótki, aby nie przechodzili obcięte. Skrócona słów ani fraz są trudne dla użytkownika, można przeanalizować od w pomieszczeniu.
- **Należy wziąć pod uwagę stanu wiersza Focused** — ponieważ wiersz staje się większy z więcej zaokrąglone narożniki aktywnego, należy przeprowadzić testy w komórce wygląd wszystkie stany. Obrazy lub tekst może zostać przycięty lub Szukaj niepoprawne w stanie Focused.
- **Użyj oszczędnie edytowalnych tabel** -więcej czasu na systemu tvOS niż iOS jest przeniesienie lub usunięcie wierszy tabeli. Należy uważnie zdecydować, jeśli ta funkcja spowoduje dodanie lub niekorzystnie wpłynąć na aplikację systemu tvOS.
- **Utwórz niestandardowe komórki typy gdzie odpowiednie** — gdy typy wbudowane komórkę widoku tabeli są doskonałe rozwiązanie dla wielu sytuacjach należy rozważyć utworzenie typy komórki niestandardowe niestandardowych informacji, aby zapewnić lepszą kontrolę i lepiej prezentować te informacje do użytkownik.

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>Praca z widokami tabeli

Najprostszym sposobem Praca z widokami tabeli w aplikacji Xamarin.tvOS jest tworzenie i modyfikowanie ich wyglądu w Projektancie interfejsu.

Aby rozpocząć pracę, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. W programie Visual Studio dla komputerów Mac, Utwórz nowy projekt aplikacji systemu tvOS, a następnie wybierz **systemu tvOS** > **aplikacji** > **jednej aplikacji widoku** i kliknij przycisk  **Następny** przycisk: 

    [![](table-views-images/table02.png "Wybierz jednego widoku aplikacji")](table-views-images/table02.png#lightbox)
1. Wprowadź **nazwa** dla aplikacji i kliknij przycisk **dalej**: 

    [![](table-views-images/table03.png "Wprowadź nazwę aplikacji")](table-views-images/table03.png#lightbox)
1. Dostosuj albo **Nazwa projektu** i **Nazwa rozwiązania** lub zaakceptuj wartości domyślne i kliknij przycisk **Utwórz** przycisk, aby utworzyć nowe rozwiązanie: 

    [![](table-views-images/table04.png "Nazwa projektu i nazwa rozwiązania")](table-views-images/table04.png#lightbox)
1. W **konsoli rozwiązania**, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć go w Projektancie systemu iOS: 

    [![](table-views-images/table05.png "Plik Main.storyboard")](table-views-images/table05.png#lightbox)
1. Wybierz i Usuń **domyślnego kontrolera widoku**: 

    [![](table-views-images/table06.png "Wybierz i Usuń kontrolera widok domyślny")](table-views-images/table06.png#lightbox)
1. Wybierz **kontrolera widoku podziału** z **przybornika** i przeciągnij go na powierzchnię projektu.
1. Domyślnie, otrzymasz [podzielony widok](~/ios/tvos/user-interface/split-views.md) z **kontrolera widoku nawigacji** i **kontrolera widoku tabeli** po lewej stronie i **kontrolera widoku** po prawej stronie. Jest to sugerowane użycie widoku tabeli w systemu tvOS firmy Apple: 

    [![](table-views-images/table08.png "Dodaj widok podziału")](table-views-images/table08.png#lightbox)
1. Konieczne będzie wybierz każdej części widoku tabeli i przypisz go niestandardowego **Nazwa klasy** w **elementu Widget** karcie **Explorer właściwości** , aby było dostępne później w języku C# Kod. Na przykład **kontrolera widoku tabeli**: 

    [![](table-views-images/table09.png "Przypisz nazwę klasy")](table-views-images/table09.png#lightbox)
1. Upewnij się, utworzenie niestandardowej klasy dla **kontrolera widoku tabeli**, **widoku tabeli** oraz wszelkie **komórek prototypu**. Visual Studio for Mac doda tworzonych klas niestandardowych w drzewie projektu: 

    [![](table-views-images/table10.png "Klas niestandardowych w drzewie projektu")](table-views-images/table10.png#lightbox)
1. Następnie wybierz widok tabeli na powierzchni projektu i dostosować jego właściwości, zgodnie z potrzebami. Takie jak liczba **komórek prototypu** i **styl** (zwykły lub grupowanego): 

    [![](table-views-images/table11.png "Na karcie widżetu")](table-views-images/table11.png#lightbox)
1. Dla każdego **komórki prototypu**, zaznacz go i przypisywać unikatowy **identyfikator** w **elementu Widget** karcie **Explorer właściwości**. Ten krok jest _bardzo ważne_ ponieważ ten identyfikator będzie on potrzebny później podczas wypełniania tabeli. Na przykład `AttrCell`: 

    [![](table-views-images/table12.png "Na karcie widżetu")](table-views-images/table12.png#lightbox)
1. Można również wybrać do prezentowania komórki wśród [domyślne typy komórki widok tabeli](#Table-View-Cell-Types) za pośrednictwem **styl** listy rozwijanej lub ustaw ją na **niestandardowe** i użyj powierzchnię projektu do układu komórki przeciągając w innych widżetów interfejsu użytkownika za pomocą **przybornika**: 

    [![](table-views-images/table13.png "Układ komórki")](table-views-images/table13.png#lightbox)
1. Przypisz unikatowy **nazwa** do każdego elementu interfejsu użytkownika w projekcie komórki prototypu w **elementu Widget** karty **Explorer właściwości** umożliwi dostęp je później w kodzie języka C#: 

    [![](table-views-images/table14.png "Przypisz nazwę")](table-views-images/table14.png#lightbox)
1. Powtórz powyższy krok dla wszystkich komórek prototypu w widoku tabeli.
1. Następnie przypisać klas niestandardowych do pozostałej części projektu interfejsu użytkownika, układ widoku szczegółów i przypisz unikatowy **nazwy** do każdego elementu interfejsu użytkownika na podstawie szczegółów wyświetlić tak, aby można je dostępu również w języku C#. Na przykład: 

    [![](table-views-images/table15.png "Układ interfejsu użytkownika")](table-views-images/table15.png#lightbox)
1. Zapisz zmiany do scenorysu.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. W programie Visual Studio Utwórz nowy projekt aplikacji systemu tvOS, a następnie wybierz **systemu tvOS** > **jednej aplikacji widoku** , a następnie wprowadź nazwę aplikacji. Kliknij przycisk **OK** przycisk, aby utworzyć nowe rozwiązanie: 

    [![](table-views-images/table02-vs.png "Wybierz jednego widoku aplikacji")](table-views-images/table02-vs.png#lightbox)
1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć go w Projektancie systemu iOS: 

    [![](table-views-images/table05-vs.png "Plik Main.storyboard")](table-views-images/table05-vs.png#lightbox)
1. Wybierz i Usuń **domyślnego kontrolera widoku**: 

    [![](table-views-images/table06-vs.png "Wybierz i Usuń kontrolera widok domyślny")](table-views-images/table06-vs.png#lightbox)
1. Wybierz **kontrolera widoku podziału** z **przybornika** i przeciągnij go na powierzchnię projektu: 

    [![](table-views-images/table07-vs.png "Kontroler widoku podziału")](table-views-images/table07-vs.png#lightbox)
1. Domyślnie, otrzymasz [podzielony widok](~/ios/tvos/user-interface/split-views.md) z **kontrolera widoku nawigacji** i **kontrolera widoku tabeli** po lewej stronie i **kontrolera widoku** po prawej stronie. Jest to sugerowane użycie widoku tabeli w systemu tvOS firmy Apple: 

    [![](table-views-images/table08-vs.png "Układ interfejsu użytkownika")](table-views-images/table08-vs.png#lightbox)
1. Konieczne będzie wybierz każdej części widoku tabeli i przypisz go niestandardowego **Nazwa klasy** w **elementu Widget** karcie **Explorer właściwości** , aby było dostępne później w języku C# Kod. Na przykład **kontrolera widoku tabeli**: 

    [![](table-views-images/table09-vs.png "Na karcie widżetu")](table-views-images/table09-vs.png#lightbox)
1. Upewnij się, utworzenie niestandardowej klasy dla **kontrolera widoku tabeli**, **widoku tabeli** oraz wszelkie **komórek prototypu**. Visual Studio for Mac doda tworzonych klas niestandardowych w drzewie projektu: 

    [![](table-views-images/table10-vs.png "Klas niestandardowych w drzewie projektu")](table-views-images/table10-vs.png#lightbox)
1. Następnie wybierz widok tabeli na powierzchni projektu i dostosować jego właściwości, zgodnie z potrzebami. Takie jak liczba **komórek prototypu** i **styl** (zwykły lub grupowanego): 

    [![](table-views-images/table11-vs.png "Na karcie widżetu")](table-views-images/table11-vs.png#lightbox)
1. Dla każdego **komórki prototypu**, zaznacz go i przypisywać unikatowy **identyfikator** w **elementu Widget** karcie **Explorer właściwości**. Ten krok jest _bardzo ważne_ ponieważ ten identyfikator będzie on potrzebny później podczas wypełniania tabeli. Na przykład `AttrCell`: 

    [![](table-views-images/table12-vs.png "Przypisz identyfikator")](table-views-images/table12-vs.png#lightbox)
1. Można również wybrać do prezentowania komórki wśród [domyślne typy komórki widok tabeli](#Table-View-Cell-Types) za pośrednictwem **styl** listy rozwijanej lub ustaw ją na **niestandardowe** i użyj powierzchnię projektu do układu komórki przeciągając w innych widżetów interfejsu użytkownika za pomocą **przybornika**: 

    [![](table-views-images/table13-vs.png "Styl menu rozwijanego")](table-views-images/table13-vs.png#lightbox)
1. Przypisz unikatowy **nazwa** do każdego elementu interfejsu użytkownika w projekcie komórki prototypu w **elementu Widget** karty **Explorer właściwości** umożliwi dostęp je później w kodzie języka C#: 

    [![](table-views-images/table14-vs.png "Na karcie widżetu")](table-views-images/table14-vs.png#lightbox)
1. Powtórz powyższy krok dla wszystkich komórek prototypu w widoku tabeli.
1. Następnie przypisać klas niestandardowych do pozostałej części projektu interfejsu użytkownika, układ widoku szczegółów i przypisz unikatowy **nazwy** do każdego elementu interfejsu użytkownika na podstawie szczegółów wyświetlić tak, aby można je dostępu również w języku C#. Na przykład: 

    [![](table-views-images/table15.png "Układ interfejsu użytkownika")](table-views-images/table15.png#lightbox)
1. Zapisz zmiany do scenorysu.
    
-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>Projektowanie modelu danych

Sprawiają, że praca z tabeli zostaną wyświetlone łatwiej informacjami oraz do jej obsługi ułatwiają przedstawiania szczegółowych informacji (jak użytkownik wybiera lub wyróżnia wierszy w widoku tabeli), tworzenie niestandardowej klasy lub klas do działania jako Model danych, aby uzyskać informacje przedstawione .

Przykładowy aplikacji rezerwacji podróży, który zawiera listę **miasta**, każdy, który zawiera unikatową listę **atrakcje** wybranego użytkownika. Użytkownik będzie mógł oznaczyć przyciągania jako *ulubionych*, wybierz pozycję Pobierz *instrukcjami* do przyciągania i *książki lot* do danego miasta.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Do tworzenia modelu danych dla **przyciągania**, kliknij prawym przyciskiem myszy nazwę projektu w **konsoli rozwiązania** i wybierz **Dodaj** > **nowego pliku...** . Wprowadź `AttractionInformation` dla **nazwa** i kliknij przycisk **nowy** przycisk: 

[![](table-views-images/data01.png "Wprowadź nazwę AttractionInformation")](table-views-images/data01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby utworzyć Model danych dla **przyciągania**, kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierz **Dodaj** > **nowy element ...** . Wybierz **klasy** , a następnie wprowadź `AttractionInformation` dla **nazwa** i kliknij przycisk **Dodaj** przycisk: 

[![](table-views-images/data01-vs.png "Wybierz klasę i wprowadź nazwę AttractionInformation")](table-views-images/data01-vs.png#lightbox)

-----

Edytuj `AttractionInformation.cs` pliku i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;

namespace tvTable
{
    public class AttractionInformation : NSObject
    {
        #region Computed Properties
        public CityInformation City { get; set;}
        public string Name { get; set;}
        public string Description { get; set;}
        public string ImageName { get; set;}
        public bool IsFavorite { get; set;}
        public bool AddDirections { get; set;}
        #endregion

        #region Constructors
        public AttractionInformation (string name, string description, string imageName)
        {
            // Initialize
            this.Name = name;
            this.Description = description;
            this.ImageName = imageName;
        }
        #endregion
    }
}
```

Ta klasa udostępnia właściwości do przechowywania informacji o danym **przyciągania**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Następnie kliknij prawym przyciskiem myszy nazwę projektu w **konsoli rozwiązania** ponownie i wybierz **Dodaj** > **nowego pliku...** . Wprowadź `CityInformation` dla **nazwa** i kliknij przycisk **nowy** przycisk: 

[![](table-views-images/data02.png "Wprowadź nazwę CityInformation")](table-views-images/data02.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Następnie kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** ponownie i wybierz **Dodaj** > **nowy element...** . Wprowadź `CityInformation` dla **nazwa** i kliknij przycisk **Dodaj** przycisk: 

[![](table-views-images/data02-vs.png "Wprowadź nazwę CityInformation")](table-views-images/data02-vs.png#lightbox)

-----

Edytuj `CityInformation.cs` pliku i zapewnić ich wyglądać następująco:

```csharp
using System;
using System.Collections.Generic;
using Foundation;

namespace tvTable
{
    public class CityInformation : NSObject
    {
        #region Computed Properties
        public string Name { get; set; }
        public List<AttractionInformation> Attractions { get; set;}
        public bool FlightBooked { get; set;}
        #endregion

        #region Constructors
        public CityInformation (string name)
        {
            // Initialize
            this.Name = name;
            this.Attractions = new List<AttractionInformation> ();
        }
        #endregion

        #region Public Methods
        public void AddAttraction (AttractionInformation attraction)
        {
            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }

        public void AddAttraction (string name, string description, string imageName)
        {
            // Create attraction
            var attraction = new AttractionInformation (name, description, imageName);

            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }
        #endregion
    }
}
```

Ta klasa zawiera wszystkie informacje o miejsce docelowe **miasta**, Kolekcja **atrakcje** dla tego miasta i udostępnia dwie metody pomocnika (`AddAttraction`) aby ułatwić dodawanie atrakcje do miasta.

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>Źródło danych w widoku tabeli

Źródło danych wymaga każdego widoku tabeli (`UITableViewDataSource`) do udostępniania danych dla tabeli i generowanie niezbędne wiersze jako wymagane przez widok tabeli.

Na przykład podane powyżej, kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** > **nowego pliku...**  i nadaj mu `AttractionTableDatasource` i kliknij przycisk **nowy** przycisk, aby utworzyć. Następnie Edytuj `AttractionTableDatasource.cs` pliku i zapewnić ich wyglądać następująco:

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDatasource : UITableViewDataSource
    {
        #region Constants
        const string CellID = "AttrCell";
        #endregion

        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        public List<CityInformation> Cities { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDatasource (AttractionTableViewController controller)
        {
            // Initialize
            this.Controller = controller;
            this.Cities = new List<CityInformation> ();
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities ()
        {
            // Clear existing
            Cities.Clear ();

            // Define cities and attractions
            var Paris = new CityInformation ("Paris");
            Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
            Paris.AddAttraction ("Musée du Louvre", "is one of the world's largest museums and a historic monument in Paris, France.", "Louvre");
            Paris.AddAttraction ("Moulin Rouge", "French for 'Red Mill', is a cabaret in Paris, France.", "MoulinRouge");
            Paris.AddAttraction ("La Seine", "Is a 777-kilometre long river and an important commercial waterway within the Paris Basin.", "RiverSeine");
            Cities.Add (Paris);

            var SanFran = new CityInformation ("San Francisco");
            SanFran.AddAttraction ("Alcatraz Island", "Is located in the San Francisco Bay, 1.25 miles (2.01 km) offshore from San Francisco.", "Alcatraz");
            SanFran.AddAttraction ("Golden Gate Bridge", "Is a suspension bridge spanning the Golden Gate strait between San Francisco Bay and the Pacific Ocean", "GoldenGateBridge");
            SanFran.AddAttraction ("San Francisco", "Is the cultural, commercial, and financial center of Northern California.", "SanFrancisco");
            SanFran.AddAttraction ("Telegraph Hill", "Is primarily a residential area, much quieter than adjoining North Beach.", "TelegraphHill");
            Cities.Add (SanFran);

            var Houston = new CityInformation ("Houston");
            Houston.AddAttraction ("City Hall", "It was constructed in 1938-1939, and is located in Downtown Houston.", "CityHall");
            Houston.AddAttraction ("Houston", "Is the most populous city in Texas and the fourth most populous city in the US.", "Houston");
            Houston.AddAttraction ("Texas Longhorn", "Is a breed of cattle known for its characteristic horns, which can extend to over 6 ft.", "LonghornCattle");
            Houston.AddAttraction ("Saturn V Rocket", "was an American human-rated expendable rocket used by NASA between 1966 and 1973.", "Rocket");
            Cities.Add (Houston);
        }
        #endregion

        #region Override Methods
        public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Get cell
            var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

            // Populate cell
            cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

            // Return new cell
            return cell;
        }

        public override nint NumberOfSections (UITableView tableView)
        {
            // Return number of cities
            return Cities.Count;
        }

        public override nint RowsInSection (UITableView tableView, nint section)
        {
            // Return the number of attractions in the given city
            return Cities [(int)section].Attractions.Count;
        }

        public override string TitleForHeader (UITableView tableView, nint section)
        {
            // Get the name of the current city
            return Cities [(int)section].Name;
        }
        #endregion
    }
}
```

Spójrzmy na kilka sekcje klasy szczegółowo.

Najpierw możemy zdefiniowane stałą, aby pomieścić Unikatowy identyfikator komórki prototypu (jest to ten sam identyfikator przypisane w Projektancie interfejsu powyżej), dodaniu skrót powrót do tabeli kontroler widoku i Magazyn utworzony dla danych:

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

Następnie możemy zapisać kontrolera widoku tabeli, a następnie kompilacji i wypełnić naszych źródła danych (przy użyciu modeli danych zdefiniowanych powyżej) podczas tworzenia klasy:

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

Ze względu na przykład `PopulateCities` — metoda po prostu tworzy obiekty modelu danych w pamięci, jednak można je łatwo odczytać z bazy danych lub usługi sieci web w rzeczywistej aplikacji:

```csharp
public void PopulateCities ()
{
    // Clear existing
    Cities.Clear ();

    // Define cities and attractions
    var Paris = new CityInformation ("Paris");
    Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
    ...
}
```

`NumberOfSections` Metoda zwraca liczbę sekcji w tabeli:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

Aby uzyskać **zwykły** stylem widoków tabel, zawsze zwraca 1.

`RowsInSection` Metoda zwraca liczbę wierszy w bieżącej sekcji:

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

Ponownie, aby uzyskać **zwykły** widoków tabel zwraca całkowitą liczbę elementów w źródle danych.

`TitleForHeader` Metoda zwraca tytuł dla danego sekcji:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

Dla **zwykły** widoku tabeli wpisz, pozostaw to pole puste tytuł (`""`).

Na koniec żądanie widoku tabeli, tworzenia i wypełniania, przy użyciu prototypu komórki `GetCell` metody: 

```csharp
public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Get cell
    var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

    // Populate cell
    cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

    // Return new cell
    return cell;
}
```

Aby uzyskać więcej informacji na temat pracy z `UITableViewDatasource`, można znaleźć pod adresem firmy Apple [UITableViewDatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) dokumentacji.

<a name="The-Table-View-Delegate" />

## <a name="the-table-view-delegate"></a>Delegat widoku tabeli

Każdy widok tabeli wymaga delegata (`UITableViewDelegate`) odpowiedzieć interakcji z użytkownikiem lub inne zdarzenia systemowe w tabeli.

Na przykład podane powyżej, kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań**, wybierz pozycję **Dodaj** > **nowego pliku...**  i nadaj mu `AttractionTableDelegate` i kliknij przycisk **nowy** przycisk, aby utworzyć. Następnie Edytuj `AttractionTableDelegate.cs` pliku i zapewnić ich wyglądać następująco:

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDelegate : UITableViewDelegate
    {
        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDelegate (AttractionTableViewController controller)
        {
            // Initializw
            this.Controller = controller;
        }
        #endregion

        #region Override Methods
        public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
            attraction.IsFavorite = (!attraction.IsFavorite);

            // Update UI
            Controller.TableView.ReloadData ();
        }

        public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Inform caller of highlight change
            RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
            return true;
        }
        #endregion

        #region Events
        public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
        public event AttractionHighlightedDelegate AttractionHighlighted;

        internal void RaiseAttractionHighlighted (AttractionInformation attraction)
        {
            // Inform caller
            if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
        }
        #endregion
    }
}
```

Spójrzmy na kilku sekcji tej klasy w szczegółach.

Najpierw utworzymy skrót do kontrolera widoku tabeli po utworzeniu klasy:

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

Następnie, gdy zostanie wybrany wiersz (użytkownik klika dotykać powierzchni zdalnego Apple) chcemy oznaczyć **przyciągania** reprezentowany przez wybranego wiersza jako ulubione:

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

Następnie, gdy użytkownik wyróżnia wiersza (przekazując go za pomocą firmy Apple zdalnego dotykać powierzchni) chcemy przedstawia szczegóły **przyciągania** reprezentowanego przez ten wiersz w sekcji szczegółów kontrolera widoku podziału:

```csharp
public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Inform caller of highlight change
    RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
    return true;
}
...

public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
public event AttractionHighlightedDelegate AttractionHighlighted;

internal void RaiseAttractionHighlighted (AttractionInformation attraction)
{
    // Inform caller
    if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
}
```

`CanFocusRow` Metoda jest wywoływana dla każdego wiersza, który ma uzyskać fokusu w widoku tabeli. Zwraca `true` w przypadku wiersza może uzyskać fokusu, przeciwnym razie zwraca `false`. W tym przykładzie został utworzony niestandardowy `AttractionHighlighted` zdarzeń, który zostanie wygenerowany w każdym wierszu, jak staje się aktywny.

Aby uzyskać więcej informacji na temat pracy z `UITableViewDelegate`, można znaleźć pod adresem firmy Apple [UITableViewDelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) dokumentacji.

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>Komórka widoku tabeli

Dla każdej komórki prototypu, który został dodany do tabeli widoku w Projektancie interfejsu, również utworzyć niestandardowego wystąpienia komórkę widoku tabeli (`UITableViewCell`) pozwala wypełnić nowej komórki (wiersz) zgodnie z jego tworzenia.

Przykładową aplikację, kliknij dwukrotnie `AttractionTableCell.cs` plik, aby otworzyć do edycji i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableCell : UITableViewCell
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public AttractionTableCell (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Trap all errors
            try {
                Title.Text = Attraction.Name;
                Favorite.Hidden = (!Attraction.IsFavorite);
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion
    }
}
```

Ta klasa udostępnia magazynu dla obiekt modelu danych przyciągania (`AttractionInformation` zdefiniowany powyżej) w danym wierszu wyświetlane:

```csharp
private AttractionInformation _attraction = null;
...

public AttractionInformation Attraction {
    get { return _attraction; }
    set {
        _attraction = value;
        UpdateUI ();
    }
}
```

`UpdateUI` Metoda wypełnia widżetów interfejsu użytkownika (które zostały dodane do komórki prototypu w Projektancie interfejsu) zgodnie z wymaganiami:

```csharp
private void UpdateUI ()
{
    // Trap all errors
    try {
        Title.Text = Attraction.Name;
        Favorite.Hidden = (!Attraction.IsFavorite);
    } catch {
        // Since the UI might not be fully loaded, ignore
        // all errors at this point
    }
}
```

Aby uzyskać więcej informacji na temat pracy z `UITableViewCell`, można znaleźć pod adresem firmy Apple [UITableViewCell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) dokumentacji.

<a name="The-Table-View-Controller" />

## <a name="the-table-view-controller"></a>Kontroler widoku tabeli

Kontroler widoku tabeli (`UITableViewController`) zarządza tabeli widoku, który został dodany do scenorysu za pomocą projektanta interfejsu.

Przykładową aplikację, kliknij dwukrotnie `AttractionTableViewController.cs` plik, aby otworzyć do edycji i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableViewController : UITableViewController
    {
        #region Computed Properties
        public AttractionTableDatasource Datasource {
            get { return TableView.DataSource as AttractionTableDatasource; }
        }

        public AttractionTableDelegate TableDelegate {
            get { return TableView.Delegate as AttractionTableDelegate; }
        }
        #endregion

        #region Constructors
        public AttractionTableViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Setup table
            TableView.DataSource = new AttractionTableDatasource (this);
            TableView.Delegate = new AttractionTableDelegate (this);
            TableView.ReloadData ();
        }
        #endregion
    }
}
```

Spójrzmy bliższe spojrzenie na tej klasy. Po pierwsze, zostały utworzone skróty, aby ułatwić dostęp do widoku tabeli `DataSource` i `TableDelegate`. Użyjemy tych później do komunikacji między widoku tabeli po lewej stronie widoku podziału i widoku szczegółów w prawo.

Na koniec, gdy tabela widok jest ładowany do pamięci, utworzymy wystąpienia `AttractionTableDatasource` i `AttractionTableDelegate` (zarówno utworzony w poprzednim przykładzie) i dołącz je do widoku tabeli.

Aby uzyskać więcej informacji na temat pracy z `UITableViewController`, można znaleźć pod adresem firmy Apple [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) dokumentacji.

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>Razem wszystkie pobieranie modułu

Jak już wspomniano w chwili rozpoczęcia tego dokumentu, widoki tabel zwykle są wyświetlane w jednej stronie [podzielony widok](~/ios/tvos/user-interface/split-views.md) jako nawigacji ze szczegółami wybranego elementu wyświetlany w przeciwną stronę. Na przykład: 

[![](table-views-images/intro01.png "Przykładowa aplikacja Uruchom")](table-views-images/intro01.png#lightbox)

Ponieważ jest to standardowy wzorzec systemu tvOS, Przyjrzyjmy się ostatnie kroki wszystko ze sobą i mają po lewej i prawej strony podzielony widok oddziaływać na siebie.

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>Widok szczegółowy

Na przykład aplikacji podróży przedstawionych powyżej, niestandardowe klasy (`AttractionViewController`) jest zdefiniowany dla standardowych kontroler widoku przedstawionych w prawej części podzielony widok jako widok szczegółów:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionViewController : UIViewController
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }

        public MasterSplitView SplitView { get; set;}
        #endregion

        #region Constructors
        public AttractionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void UpdateUI ()
        {
            // Trap all errors
            try {
                City.Text = Attraction.City.Name;
                Title.Text = Attraction.Name;
                SubTitle.Text = Attraction.Description;

                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
                IsFavorite.Hidden = (!Attraction.IsFavorite);
                IsDirections.Hidden = (!Attraction.AddDirections);
                BackgroundImage.Image = UIImage.FromBundle (Attraction.ImageName);
                AttractionImage.Image = BackgroundImage.Image;
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Ensure the UI Updates
            UpdateUI ();
        }
        #endregion

        #region Actions
        partial void BookFlight (NSObject sender)
        {
            // Ask user to book flight
            AlertViewController.PresentOKCancelAlert ("Book Flight",
                                                      string.Format ("Would you like to book a flight to {0}?", Attraction.City.Name),
                                                      this,
                                                      (ok) => {
                Attraction.City.FlightBooked = ok;
                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
            });
        }

        partial void GetDirections (NSObject sender)
        {
            // Ask user to add directions
            AlertViewController.PresentOKCancelAlert ("Add Directions",
                                                     string.Format ("Would you like to add directions to {0} to you itinerary?", Attraction.Name),
                                                     this,
                                                     (ok) => {
                                                         Attraction.AddDirections = ok;
                                                         IsDirections.Hidden = (!Attraction.AddDirections);
                                                     });
        }

        partial void MarkFavorite (NSObject sender)
        {
            // Flip favorite state
            Attraction.IsFavorite = (!Attraction.IsFavorite);
            IsFavorite.Hidden = (!Attraction.IsFavorite);

            // Reload table
            SplitView.Master.TableController.TableView.ReloadData ();
        }
        #endregion
    }
}
```

W tym miejscu możemy podano **przyciągania** (`AttractionInformation`) są wyświetlane jako właściwość i utworzyć `UpdateUI` metodę, która wypełnia elementy widget interfejsu użytkownika dodano do widoku w Projektancie interfejsu.

Również zdefiniowaniu skrót do kontrolera widoku podziału (`SplitView`) firma Microsoft będzie służyć do komunikacji zmiany widoku tabeli (`AcctractionTableView`).

Na koniec akcje niestandardowe (zdarzeń) zostały dodane do trzech `UIButton` wystąpienia utworzone w Projektancie interfejsu, który umożliwia użytkownikowi oznaczyć przyciągania jako _ulubionych_, Pobierz _instrukcjami_ do przyciągania i _książki lot_ do danego miasta.

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>Kontroler widoku nawigacji

Ponieważ kontroler widoku tabeli jest zagnieżdżony w kontrolerze widoku nawigacji po lewej stronie widoku podziału, kontroler widoku nawigacji przypisano niestandardowej klasy (`MasterNavigationController`) w Projektancie interfejsu i zdefiniowane w następujący sposób:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterNavigationController : UINavigationController
    {
        #region Computed Properties
        public MasterSplitView SplitView { get; set;}
        public AttractionTableViewController TableController {
            get { return TopViewController as AttractionTableViewController; }
        }
        #endregion

        #region Constructors
        public MasterNavigationController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Ta klasa definiuje ponownie, wystarczy kilka skróty ułatwiające komunikują się za pośrednictwem partnerami kontroler widoku podziału:

* `SplitView` — Jest to łącze do kontrolera widoku podziału (`MainSpiltViewController`) należącą do kontrolera widoku nawigacji.
* `TableController` -Pobiera kontroler widoku tabeli (`AttractionTableViewController`) występuje w postaci widoku Top w kontroler widoku nawigacji.

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>Kontroler widoku podziału

Ponieważ kontroler widoku podziału jest podstawą naszej aplikacji, utworzyliśmy niestandardowej klasy (`MasterSplitViewController`) dla niego w Projektancie interfejsu i zdefiniowane go w następujący sposób:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterSplitView : UISplitViewController
    {
        #region Computed Properties
        public AttractionViewController Details {
            get { return ViewControllers [1] as AttractionViewController; }
        }

        public MasterNavigationController Master {
            get { return ViewControllers [0] as MasterNavigationController; }
        }
        #endregion

        #region Constructors
        public MasterSplitView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            Master.SplitView = this;
            Details.SplitView = this;

            // Wire-up events
            Master.TableController.TableDelegate.AttractionHighlighted += (attraction) => {
                // Display new attraction
                Details.Attraction = attraction;
            };
        }
        #endregion
    }
}
```

Najpierw utworzymy skróty do **szczegóły** obok siebie widoku podziału (`AttractionViewController`) i **wzorca** po stronie (`MasterNavigationController`). Ponownie to ułatwia komunikacji między dwiema stronami.

Następnie podczas podzielony widok jest ładowany do pamięci, możemy dołączanie kontrolera widoku podziału na obu stronach podzielony widok i reagowanie na użytkownika wyróżnianie przyciągania w widoku tabeli (`AttractionHighlighted`) za pomocą wyświetlania nowych przyciągania w **szczegóły**  obok siebie widoku podziału.

Zobacz [tvTables](https://developer.xamarin.com/samples/monotouch/tvos/tvTable/) Przykładowa aplikacja dla pełnej implementacji widoków tabel wewnątrz podzielony widok.

## <a name="table-views-in-detail"></a>Widoki tabel szczegółowo

Ponieważ systemu tvOS opiera się z systemem iOS, tabeli widokach i kontrolerach widok tabeli zostały zaprojektowane i zachowują się w podobny sposób. Aby uzyskać szczegółowe informacje na temat pracy z widoku tabeli w aplikacji platformy Xamarin, zobacz nasze iOS [Praca z tabelami i komórkami](~/ios/user-interface/controls/tables/index.md) dokumentacji.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego projektowanie i Praca z widokami tabeli wewnątrz aplikacji Xamarin.tvOS. I został przedstawiony przykład pracy z widoku tabeli wewnątrz widoku podziału typowe zastosowania widoku tabeli w aplikacji systemu tvOS.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
