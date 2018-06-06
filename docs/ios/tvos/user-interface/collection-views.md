---
title: Praca z systemu tvOS widoki kolekcji w programie Xamarin
description: Ten dokument zawiera opis sposobu pracy z widokiem kolekcji w systemu tvOS aplikacji skompilowanej za pomocą platformy Xamarin. Obejmuje ona układów widoku kolekcji, tworzenie komórek i dodatkowych widoków, w odpowiedzi na zdarzenia użytkownika i inne.
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 9b411ac6bb8d1492511f9e5d2234731ae64c3a82
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789281"
---
# <a name="working-with-tvos-collection-views-in-xamarin"></a>Praca z systemu tvOS widoki kolekcji w programie Xamarin

Kolekcja widoków umożliwiają grupą zawartości mają być wyświetlane przy użyciu dowolnego układów. Przy użyciu wbudowanych funkcji, pozwalają one na łatwe tworzenie układów siatki lub liniowy obsługuje również układy niestandardowe.

[![](collection-views-images/collection01.png "Przykładowy widok kolekcji")](collection-views-images/collection01.png#lightbox)

Widok kolekcji przechowuje kolekcję elementów, używając delegata i źródła danych do interakcji z użytkownikiem i zawartość elementu kolekcji. Ponieważ widok kolekcji jest oparty na podsystemu układu, która jest niezależna od samego widoku, podając inny układ można łatwo zmienić prezentacji widok kolekcji danych na bieżąco.

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>Widoki kolekcji — informacje

Jak wspomniano powyżej, widok kolekcji (`UICollectionView`) zarządza uporządkowaną kolekcję elementów i przedstawia te elementy dostosowywalne układów. Widoki kolekcji działają w sposób podobny do widoku tabeli (`UITableView`), z wyjątkiem służy układów do elementów obecnych w więcej niż jednej kolumny.

Korzystając z widokiem kolekcji w systemu tvOS, aplikacja jest odpowiedzialna za przekazywanie danych powiązanych z kolekcją przy użyciu źródła danych (`UICollectionViewDataSource`). Dane widoku kolekcji można opcjonalnie podzielone i przedstawione na różnych grup (sekcje).

Widok kolekcji przedstawia informacje o poszczególnych elementów na ekranie, przy użyciu komórki (`UICollectionViewCell`) zapewnia prezentacji danego elementu informacji z kolekcji (na przykład obrazu i jego tytuł).

Opcjonalnie można dodać dodatkowe widoki prezentacji widok kolekcji do działania jako nagłówki i stopki sekcje i komórek. Widok kolekcji układ jest odpowiedzialny za definiowanie rozmieszczenie tych widoków wraz z pojedynczych komórek.

Widok kolekcji mogą odpowiadać na interakcji z użytkownikiem przy użyciu delegata (`UICollectionViewDelegate`). Ten delegat jest również odpowiedzialna za określenie, czy dana komórka można uzyskać fokusu, jeśli wyróżnioną pozycją komórki czy został wybrany. W niektórych przypadkach delegat Określa rozmiar poszczególnych komórek.

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>Kolekcja układów widoku

Kluczową funkcją widok kolekcji jest jego oddzielenie dane, które jest umożliwienie korzystania i jego układu. Układ widoku kolekcji (`UICollectionViewLayout`) jest odpowiedzialny za zapewnienie organizacji i lokalizację komórki (i wszystkie dodatkowe widoki) z prezentacji na ekranie Widok kolekcji.

Pojedynczych komórek są tworzone przez widok kolekcji z dołączonego źródła danych są rozmieszczone i wyświetlane przez układ danego widoku kolekcji.

Układ widoku kolekcji znajduje się zwykle po utworzeniu widok kolekcji. Jednak układu widoku kolekcji można zmienić w dowolnym momencie i na ekranie prezentację danych Widok kolekcji będą automatycznie aktualizowane przy użyciu nowy układ podane.

Układ widoku kolekcji zapewnia kilka metod, które mogą służyć do animowania przejścia między dwa różne układy (domyślnie odbywa się bez animacji). Ponadto układów widoku kolekcji może współpracować z aparatów rozpoznawania gestów do animowania dalszej interakcji z użytkownikiem, który powoduje zmianę w układzie.

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>Tworzenie komórek i dodatkowych widoków

Widok kolekcji źródła danych nie jest tylko odpowiedzialna za przekazywanie danych kopii kolekcji elementów, ale także komórki, które są używane do wyświetlania zawartości.

Ponieważ widoki kolekcji są przeznaczone do obsługi dużych kolekcji elementów, pojedynczych komórek można usuniętej i ponownie, aby zapobiec przekroczenia ograniczenia ilości pamięci. Istnieją dwie metody dequeueing widoków:

- `DequeueReusableCell` — Umożliwia utworzenie lub zwraca komórki danego typu (jak określono w scenorysu aplikacji).
- `DequeueReusableSupplementaryView` — Umożliwia utworzenie lub zwraca dodatkowy widok danego typu (jak określono w scenorysu aplikacji).

Przed wywołaniem dowolnej z tych metod, należy zarejestrować klasy, scenorysu lub `.xib` plik używany do tworzenia widoku komórki z widokiem kolekcji. Na przykład:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

Gdzie `typeof(CityCollectionViewCell)` zawiera klasę obsługującą widok i `CityViewDatasource.CardCellId` zawiera identyfikator używany, gdy komórka (lub Widok) jest usunięty z kolejki.

Po komórki jest usunięty z kolejki, jest skonfigurowana przy użyciu danych dla elementu, który jest ona reprezentujący i powrócić do widoku kolekcji do wyświetlenia.

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>O kontrolerach widok kolekcji

Kontroler widoku kolekcji (`UICollectionViewController`) jest specjalne kontrolera widoku (`UIViewController`) zapewnia następujące działania:

- Jest odpowiedzialny za ładowania widoku kolekcji z jego scenorysu lub `.xib` plików i tworzenia wystąpienia widoku. Jeśli utworzony w kodzie, automatycznie tworzy nowy, nieskonfigurowanego widok kolekcji.
- Po załadowaniu widoku kolekcji kontrolera próbuje załadować jego źródła danych i delegata z scenorysu lub `.xib` pliku. Jeśli nie jest dostępna żadna, ustawia się jako źródło obu.
- Sprawdza, czy dane zostanie załadowana przed widok kolekcji znajduje się na pierwszym wyświetlane i ponowne załadowanie i wyczyść wybierz na każdym ekranie kolejnych.

Ponadto kontroler widoku kolekcji zapewnia nadpisywalnych metod, które mogą służyć do zarządzania cyklem życia widok kolekcji, takie jak `AwakeFromNib` i `ViewWillDisplay`.

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>Kolekcja widoków i planów

Najprostszym sposobem widok kolekcji w Twojej aplikacji Xamarin.tvOS jest Dodaj je do jego scenorysu. Jak przedstawiono prosty przykład zamierzamy utworzyć przykładowej aplikacji, który przedstawia obraz, tytuł i wybierz przycisk. Jeśli użytkownik kliknij przycisk Wybierz, Kolekcja zostanie wyświetlony widok umożliwiająca użytkownikowi na wybranie nowego obrazu. Po wybraniu obrazu, widok kolekcji jest zamknięty i pojawi się nowy obraz i tytuł.

Teraz należy wykonać następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

    
1. Rozpoczęcie nowej **systemu tvOS pojedynczego widoku aplikacji** w programie Visual Studio dla komputerów Mac.
1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go w systemie iOS projektanta.
1. Dodaj widok obrazu, etykiety i przycisku do istniejącego widoku i skonfigurować je do następującego: 

    [![](collection-views-images/collection02.png "Przykładowy układ")](collection-views-images/collection02.png#lightbox)
1. Przypisz **nazwa** do widoku obrazu i etykiet w **kartę Widget** z **Explorer właściwości**. Na przykład: 

    [![](collection-views-images/collection03.png "Nazwa ustawienia")](collection-views-images/collection03.png#lightbox)
1. Następnie można przeciągnięcie kontrolera widoku kolekcji do scenorysu: 

    [![](collection-views-images/collection04.png "Kontroler widoku kolekcji")](collection-views-images/collection04.png#lightbox)
1. Kontroli przeciągnij od przycisku do kolekcji kontrolera widoku i wybierz **Push** z menu podręcznego: 

    [![](collection-views-images/collection05.png "Wybierz z menu podręcznego wypychania")](collection-views-images/collection05.png#lightbox)
1. Gdy aplikacja jest uruchamiana, spowoduje to widok kolekcji można Pokaż, gdy użytkownik kliknie przycisk.
1. Wybierz widok kolekcji, a następnie wprowadź następujące wartości w **kartę Układ** z **Explorer właściwości**: 

    [![](collection-views-images/collection06.png "W Eksploratorze właściwości")](collection-views-images/collection06.png#lightbox)
1. To steruje rozmiarem pamięci pojedynczych komórek i obramowania między komórkami i zewnętrznej krawędzi widok kolekcji.
1. Wybierz kontroler widoku kolekcji i ustawić jej klasa `CityCollectionViewController` w **kartę Widget**: 

    [![](collection-views-images/collection07.png "Ustaw klasy CityCollectionViewController")](collection-views-images/collection07.png#lightbox)
1. Wybierz widok kolekcji i ustawić jej klasa `CityCollectionView` w **kartę Widget**: 

    [![](collection-views-images/collection08.png "Ustaw klasy CityCollectionView")](collection-views-images/collection08.png#lightbox)
1. Zaznacz komórkę widoku kolekcji i ustawić jej klasa `CityCollectionViewCell` w **kartę Widget**: 

    [![](collection-views-images/collection09.png "Ustaw klasy CityCollectionViewCell")](collection-views-images/collection09.png#lightbox)
1. W **kartę Widget** upewnij się, że **układu** jest `Flow` i **kierunek przewijania** jest `Vertical` widoku kolekcji: 

    [![](collection-views-images/collection10.png "Na karcie widżetu")](collection-views-images/collection10.png#lightbox)
1. Zaznacz komórkę widoku kolekcji i ustawić jej **tożsamości** do `CityCell` w **kartę Widget**: 

    [![](collection-views-images/collection11.png "Ustaw tożsamość CityCell")](collection-views-images/collection11.png#lightbox)
1. Zapisz zmiany.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. Rozpoczęcie nowej **systemu tvOS pojedynczego widoku aplikacji** w programie Visual Studio.
1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go w systemie iOS projektanta.
1. Dodaj widok obrazu, etykiety i przycisku do istniejącego widoku i skonfigurować je do następującego: 

    [![](collection-views-images/collection02vs.png "Skonfiguruj układ")](collection-views-images/collection02vs.png#lightbox)
1. Przypisz **nazwa** do widoku obrazu i etykiet w **kartę Widget** z **Explorer właściwości**. Na przykład: 

    [![](collection-views-images/collection03vs.png "W Eksploratorze właściwości")](collection-views-images/collection03vs.png#lightbox)
1. Następnie można przeciągnięcie kontrolera widoku kolekcji do scenorysu: 

    [![](collection-views-images/collection04vs.png "Kontroler widoku kolekcji")](collection-views-images/collection04vs.png#lightbox)
1. Kontroli przeciągnij od przycisku do kolekcji kontrolera widoku i wybierz **Push** z menu podręcznego: 

    [![](collection-views-images/collection05vs.png "Wybierz z menu podręcznego wypychania")](collection-views-images/collection05vs.png#lightbox)
1. Gdy aplikacja jest uruchamiana, spowoduje to widok kolekcji można Pokaż, gdy użytkownik kliknie przycisk.
1. Wybierz widok kolekcji i w **kartę Układ** z **Explorer właściwości** wprowadź **szerokość** jako _361_ i  **Wysokość** jako _256_ 
1. To steruje rozmiarem pamięci pojedynczych komórek i obramowania między komórkami i zewnętrznej krawędzi widok kolekcji.
1. Wybierz kontroler widoku kolekcji i ustawić jej klasa `CityCollectionViewController` w **kartę Widget**: 

    [![](collection-views-images/collection07vs.png "Ustaw klasy CityCollectionViewController")](collection-views-images/collection07vs.png#lightbox)
1. Wybierz widok kolekcji i ustawić jej klasa `CityCollectionView` w **kartę Widget**: 

    [![](collection-views-images/collection08vs.png "Ustaw klasy CityCollectionView")](collection-views-images/collection08vs.png#lightbox)
1. Zaznacz komórkę widoku kolekcji i ustawić jej klasa `CityCollectionViewCell` w **kartę Widget**: 

    [![](collection-views-images/collection09vs.png "Ustaw klasy CityCollectionViewCell")](collection-views-images/collection09vs.png#lightbox)
1. W **kartę Widget** upewnij się, że **układu** jest `Flow` i **kierunek przewijania** jest `Vertical` widoku kolekcji: 

    [![](collection-views-images/collection10vs.png "Karta tnie widżetu")](collection-views-images/collection10vs.png#lightbox)
1. Zaznacz komórkę widoku kolekcji i ustawić jej **tożsamości** do `CityCell` w **kartę Widget**: 

    [![](collection-views-images/collection11vs.png "Ustaw tożsamość CityCell")](collection-views-images/collection11vs.png#lightbox)
1. Zapisz zmiany.
    

-----

W przypadku wybrania `Custom` widoku kolekcji **układu**, firma Microsoft może określono niestandardowego układu. Apple udostępnia wbudowaną `UICollectionViewFlowLayout` i `UICollectionViewDelegateFlowLayout` można łatwo obecne w układzie siatki na podstawie danych (są one używane przez `flow` stylu układu). 

Aby uzyskać więcej informacji na temat pracy z Scenorys, zobacz nasze [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md).

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>Dostarcza dane dla widoku kolekcji

Teraz, gdy mamy naszych widok kolekcji (i kolekcji View Controller) dodane do naszych scenorysu, należy podać dane dla kolekcji. 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>Model danych

Po pierwsze zamierzamy utworzyć model dla naszych danych, który zawiera nazwę pliku obrazu do wyświetlania, tytuł i flagi stosowanie Miasto należy wybrać.

Utwórz `CityInfo` klasy i zapewnić ich wyglądać następująco:

```csharp
using System;

namespace tvCollection
{
    public class CityInfo
    {
        #region Computed Properties
        public string ImageFilename { get; set; }
        public string Title { get; set; }
        public bool CanSelect{ get; set; }
        #endregion

        #region Constructors
        public CityInfo (string filename, string title, bool canSelect)
        {
            // Initialize
            this.ImageFilename = filename;
            this.Title = title;
            this.CanSelect = canSelect;
        }
        #endregion
    }
}
```

### <a name="the-collection-view-cell"></a>Kolekcja komórek widoku

Teraz musimy zdefiniować sposób dane będą przedstawiane dla każdej komórki. Edytuj `CityCollectionViewCell.cs` pliku (tworzone automatycznie z pliku scenorysu) i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;
using UIKit;
using CoreGraphics;

namespace tvCollection
{
    public partial class CityCollectionViewCell : UICollectionViewCell
    {
        #region Private Variables
        private CityInfo _city;
        #endregion

        #region Computed Properties
        public UIImageView CityView { get ; set; }
        public UILabel CityTitle { get; set; }

        public CityInfo City {
            get { return _city; }
            set {
                _city = value;
                CityView.Image = UIImage.FromFile (City.ImageFilename);
                CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
                CityTitle.Text = City.Title;
            }
        }
        #endregion

        #region Constructors
        public CityCollectionViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            CityView = new UIImageView(new CGRect(22, 19, 320, 171));
            CityView.AdjustsImageWhenAncestorFocused = true;
            AddSubview (CityView);

            CityTitle = new UILabel (new CGRect (22, 209, 320, 21)) {
                TextAlignment = UITextAlignment.Center,
                TextColor = UIColor.White,
                Alpha = 0.0f
            };
            AddSubview (CityTitle);
        }
        #endregion
    

    }
}
```

Dla aplikacji systemu tvOS możemy będą wyświetlane obrazu i opcjonalnie tytuł. Jeśli nie można wybrać danego Miasto, możemy są wygaszania widoku obrazu, używając następującego kodu:

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

Gdy komórkę zawierającą obraz zostanie przywrócony fokusu przez użytkownika, chcemy użyć wbudowanych efekt paralaksy w nim być ustawienie następujących właściwości:

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

Aby uzyskać więcej informacji dotyczących nawigacji i fokus, zobacz nasze [Praca z nawigacji i skoncentrować się](~/ios/tvos/app-fundamentals/navigation-focus.md) i [Siri zdalnego oraz kontrolerów Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) dokumentacji.


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>Dostawca danych widoku kolekcji

Nasz model danych utworzone i naszych układu komórki zdefiniowane Utwórzmy źródła danych dla naszych widok kolekcji. Źródło danych będzie odpowiedzialna za nie tylko przekazywanie danych zapasowy, ale także dequeueing komórek do wyświetlenia pojedynczych komórek na ekranie.

Utwórz `CityViewDatasource` klasy i zapewnić ich wyglądać następująco:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using ObjCRuntime;

namespace tvCollection
{
    public class CityViewDatasource : UICollectionViewDataSource
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Static Constants
        public static NSString CardCellId = new NSString ("CityCell");
        #endregion

        #region Computed Properties
        public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
        public CityCollectionView ViewController { get; set; }
        #endregion

        #region Constructors
        public CityViewDatasource (CityCollectionView controller)
        {
            // Initialize
            this.ViewController = controller;
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities() {

            // Clear existing cities
            Cities.Clear();

            // Add new cities
            Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
            Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
            Cities.Add(new CityInfo("City03.jpg", "Skyline at Night", true));
            Cities.Add(new CityInfo("City04.jpg", "Golden Gate Bridge", true));
            Cities.Add(new CityInfo("City05.jpg", "Roads by Night", true));
            Cities.Add(new CityInfo("City06.jpg", "Church Domes", true));
            Cities.Add(new CityInfo("City07.jpg", "Mountain Lights", true));
            Cities.Add(new CityInfo("City08.jpg", "City Scene", false));
            Cities.Add(new CityInfo("City09.jpg", "House in Winter", true));
            Cities.Add(new CityInfo("City10.jpg", "By the Lake", true));
            Cities.Add(new CityInfo("City11.jpg", "At the Dome", true));
            Cities.Add(new CityInfo("City12.jpg", "Cityscape", true));
            Cities.Add(new CityInfo("City13.jpg", "Model City", true));
            Cities.Add(new CityInfo("City14.jpg", "Taxi, Taxi!", true));
            Cities.Add(new CityInfo("City15.jpg", "On the Sidewalk", true));
            Cities.Add(new CityInfo("City16.jpg", "Midnight Walk", true));
            Cities.Add(new CityInfo("City17.jpg", "Lunchtime Cafe", true));
            Cities.Add(new CityInfo("City18.jpg", "Coffee Shop", true));
            Cities.Add(new CityInfo("City19.jpg", "Rustic Tavern", true));
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            return Cities.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
            var city = Cities [indexPath.Row];

            // Initialize city
            cityCell.City = city;

            return cityCell;
        }
        #endregion
    }
}
```

Let Przyjrzyj się tej klasy szczegółowo. Po pierwsze, możemy dziedziczyć `UICollectionViewDataSource` i podaj skrót do komórek o identyfikatorze (przypisana możemy w Projektancie systemu iOS):

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

Następnie możemy magazynowanie danych kolekcji i podaj klasę do wypełniania danych:

```csharp
public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
...

public void PopulateCities() {

    // Clear existing cities
    Cities.Clear();

    // Add new cities
    Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
    Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
    ...
}
```

Następnie możemy zastąpić `NumberOfSections` — metoda i przywracać ma liczbę sekcji (grupy elementów), które wyświetlać zapoznaj się z kolekcją. W takim przypadku jest tylko jeden:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

Następnie zostanie zwrócona liczba elementów w naszej kolekcji przy użyciu następującego kodu:

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

Na koniec mamy usuwania z kolejki wielokrotnego użytku komórki widok kolekcji żądania z następującym kodem:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
    var city = Cities [indexPath.Row];

    // Initialize city
    cityCell.City = city;

    return cityCell;
}
```

Po uzyskujemy komórce widoku kolekcji naszych `CityCollectionViewCell` typu, możemy umieścić w nim danego elementu.

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>Odpowiadanie na zdarzenia użytkownika

Ponieważ chcemy, aby użytkownik mógł wybierz element z naszych kolekcji, należy podać delegata widok kolekcji do obsługi interakcji. I musimy zapewniają sposób umożliwi naszym wywoływania widoku wiedzieć, jaki element użytkownika została wybrana.

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>Delegat aplikacji

Potrzebujemy sposób powiązać aktualnie zaznaczonego elementu w widoku kolekcji do wywoływania widoku. Będziemy używać właściwości niestandardowej na naszych `AppDelegate`. Edytuj `AppDelegate.cs` pliku i Dodaj następujący kod:

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

Definiuje właściwości i ustawia Miasto domyślny, w którym będą początkowo wyświetlane. Później firma Microsoft będzie używać tej właściwości, aby wyświetlić wybór użytkownika i Zezwalaj na wybieranie zostanie zmieniony.

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>Delegat widok kolekcji

Następnie dodaj nową `CityViewDelegate` klas do projektu i przydzielić mu wyglądać następująco:


```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace tvCollection
{
    public class CityViewDelegate : UICollectionViewDelegateFlowLayout
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public CityViewDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
        {
            return new CGSize (361, 256);
        }

        public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
        {
            if (indexPath == null) {
                return false;
            } else {
                var controller = collectionView as CityCollectionView;
                return controller.Source.Cities[indexPath.Row].CanSelect;
            }
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var controller = collectionView as CityCollectionView;
            App.SelectedCity = controller.Source.Cities [indexPath.Row];

            // Close Collection
            controller.ParentController.DismissViewController(true,null);
        }
        #endregion
    }
}
```

Spójrzmy bliższe spojrzenie na tej klasy. Po pierwsze, możemy dziedziczyć `UICollectionViewDelegateFlowLayout`. Przyczyny, które firma Microsoft pochodne względem tej klasy, a nie `UICollectionViewDelegate` jest używania wbudowanych `UICollectionViewFlowLayout` do prezentowania naszych elementów i nie jest typem niestandardowego układu.

Następnie zostanie zwrócona rozmiar dla poszczególnych elementów przy użyciu następującego kodu:

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

Następnie możemy zdecydować, jeśli dana komórka może uzyskać fokusu, używając następującego kodu: 

```csharp
public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
{
    if (indexPath == null) {
        return false;
    } else {
        var controller = collectionView as CityCollectionView;
        return controller.Source.Cities[indexPath.Row].CanSelect;
    }
}
```

Sprawdzamy, czy dany element danych zapasowy ma jego `CanSelect` ustawić flagi `true` i zwraca tę wartość. Aby uzyskać więcej informacji dotyczących nawigacji i fokus, zobacz nasze [Praca z nawigacji i skoncentrować się](~/ios/tvos/app-fundamentals/navigation-focus.md) i [Siri zdalnego oraz kontrolerów Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) dokumentacji.

Na koniec odpowiemy użytkownikowi wybranie elementu z następującym kodem:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

W tym miejscu możemy ustawić `SelectedCity` właściwość naszych `AppDelegate` do elementu, czy wybrany przez użytkownika i możemy zamknąć kontroler widoku kolekcji, powrót do widoku, który wywołuje nam. Firma Microsoft nie zostały zdefiniowane `ParentController` właściwości jeszcze naszych widok kolekcji, robimy to obok.

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>Konfigurowanie widoku kolekcji

Teraz należy edytować naszych widok kolekcji i przypisz naszych źródłem danych a obiektem delegowanym. Edytuj `CityCollectionView.cs` pliku (utworzonego dla nas automatycznie z naszych scenorysu) i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionView : UICollectionView
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public CityViewDatasource Source {
            get { return DataSource as CityViewDatasource;}
        }

        public CityCollectionViewController ParentController { get; set;}
        #endregion

        #region Constructors
        public CityCollectionView (IntPtr handle) : base (handle)
        {
            // Initialize
            RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
            DataSource = new CityViewDatasource (this);
            Delegate = new CityViewDelegate ();
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections ()
        {
            return 1;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
            if (previousItem != null) {
                Animate (0.2, () => {
                    previousItem.CityTitle.Alpha = 0.0f;
                });
            }

            var nextItem = context.NextFocusedView as CityCollectionViewCell;
            if (nextItem != null) {
                Animate (0.2, () => {
                    nextItem.CityTitle.Alpha = 1.0f;
                });
            }
        }
        #endregion
    }
}
```

Najpierw udostępniamy skrót dostępu do naszej `AppDelegate`: 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

Następnie firma Microsoft udostępnia skrót do źródła danych Widok kolekcji oraz właściwości dostępu kontrolera widoku kolekcji (używany przez naszych delegata powyżej zamknąć kolekcji po dokonaniu wyboru):

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

Następnie możemy użyć poniższego kodu do zainicjowania widok kolekcji i przypisać naszej klasy komórki, źródła danych i delegata:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

Wreszcie chcemy mieć tytuł w obszarze obraz, który jest widoczny tylko wtedy, gdy użytkownik ma on wyróżniony (fokusu). Przejdziemy następującym kodem:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
    if (previousItem != null) {
        Animate (0.2, () => {
            previousItem.CityTitle.Alpha = 0.0f;
        });
    }

    var nextItem = context.NextFocusedView as CityCollectionViewCell;
    if (nextItem != null) {
        Animate (0.2, () => {
            nextItem.CityTitle.Alpha = 1.0f;
        });
    }
}
```

Możemy ustawić czystość poprzedni element utraci fokus na zero (0) i czystość następnej uzyskanie fokus na 100%. Te przejścia uzyskać również animowane.


## <a name="configuring-the-collection-view-controller"></a>Konfigurowanie kontrolera widoku kolekcji

Teraz potrzebujemy konfiguracji końcowej na naszych widok kolekcji i umożliwić kontrolerowi można ustawić właściwości, które zdefiniowanego tak widok kolekcji można zamknąć po dokonaniu wyboru.

Edytuj `CityCollectionViewController.cs` pliku (utworzony automatycznie na podstawie naszych scenorysu) i zapewnić ich wyglądać następująco:

```csharp
// This file has been autogenerated from a class added in the UI designer.

using System;

using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionViewController : UICollectionViewController
    {
        #region Computed Properties
        public CityCollectionView Collection {
            get { return CollectionView as CityCollectionView; }
        }
        #endregion

        #region Constructors
        public CityCollectionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Save link to controller
            Collection.ParentController = this;
        }
        #endregion
    }
}

```

## <a name="putting-it-all-together"></a>Umieścić go wszystkie wspólne 

Teraz, gdy mamy wszystkie części opracować wypełnić i sterowanie naszych widok kolekcji musimy upewnić w ostatnim zmiany do naszej Widok główny wszystko ze sobą.

Edytuj `ViewController.cs` pliku (utworzony automatycznie na podstawie naszych scenorysu) i zapewnić ich wyglądać następująco:

```csharp
using System;
using Foundation;
using UIKit;
using tvCollection;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update image with the currently selected one
            CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
            BackgroundView.Image = CityView.Image;
            CityTitle.Text = App.SelectedCity.Title;
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

Poniższy kod wyświetla początkowo wybrany element z `SelectedCity` właściwość `AppDelegate` i zostanie ona ponownie, gdy użytkownik wprowadził zaznaczenia z widoku kolekcji:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update image with the currently selected one
    CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
    BackgroundView.Image = CityView.Image;
    CityTitle.Text = App.SelectedCity.Title;
}
```

<a name="Testing-the-app" />

## <a name="testing-the-app"></a>Testowanie aplikacji

Z wszystko w miejscu Jeśli skompilować i uruchomić aplikację, widok główny zostanie wyświetlony z Miasto domyślne:

[![](collection-views-images/run01.png "Ekran główny")](collection-views-images/run01.png#lightbox)

Jeśli użytkownik kliknij **wybierz widok** pojawi się przycisk, widok kolekcji:

[![](collection-views-images/run02.png "Widok kolekcji")](collection-views-images/run02.png#lightbox)

Wszelkie Miasto, w którym ma jego `CanSelect` ustawioną właściwość `false` pojawi się nieaktywne i użytkownik nie będzie można ustawić fokusu do niego. Gdy użytkownik wyróżnia elementu (stał się fokusu) jest wyświetlany tytuł i używają efekt paralaksy w celu subtlety pochylenie obrazu w 3D.

Gdy użytkownik kliknie wybierz obraz, widok kolekcji zostanie zamknięte, a widok główny zostanie wyświetlony ponownie z nowego obrazu:

[![](collection-views-images/run03.png "Nowy obraz ekranu głównego")](collection-views-images/run03.png#lightbox)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>Tworzenie układu niestandardowego i ponowne porządkowanie elementów

Jednym z kluczowych funkcji usługi przy użyciu widoku kolekcji jest możliwość tworzenia niestandardowych układów. Ponieważ systemu tvOS dziedziczy z systemem iOS, proces tworzenia niestandardowego układu jest taka sama. Zobacz nasze [wprowadzenie do kolekcji widoków](~/ios/user-interface/controls/uicollectionview.md) dokumentacji, aby uzyskać więcej informacji.

Ostatnio dodane do kolekcji widoków dla systemu iOS 9 była możliwość łatwego Zezwalaj na zmiany kolejności elementów w kolekcji. Ponownie, ponieważ systemu tvOS 9 jest podzbiorem systemu iOS 9, odbywa się nimi tak samo. Zobacz nasze [przeglądanie zmian w kolekcji](~/ios/user-interface/controls/uicollectionview.md) dokumentu, aby uzyskać więcej informacji.


<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego projektowanie i Praca z widokami kolekcja wewnątrz aplikacji Xamarin.tvOS. Najpierw należy go omówione wszystkie elementy, które tworzą widok kolekcji. Następnie go pokazano, jak projektować i implementować widokiem kolekcji przy użyciu scenorysu. Na koniec podano linki do informacji na temat tworzenia niestandardowych układów i zmianę kolejności elementów.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
