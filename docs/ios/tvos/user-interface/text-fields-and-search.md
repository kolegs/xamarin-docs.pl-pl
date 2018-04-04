---
title: Praca z tekstem i pola wyszukiwania
description: Ten artykuł obejmuje projektowanie i Praca z pola wyszukiwania i tekst wewnątrz aplikacji Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 220c6e3d1c6f358c67a2f596c977f4d2132298a8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-text-and-search-fields"></a>Praca z tekstem i pola wyszukiwania

_Ten artykuł obejmuje projektowanie i Praca z pola wyszukiwania i tekst wewnątrz aplikacji Xamarin.tvOS._



Gdy jest to wymagane, aplikację Xamarin.tvOS mogą żądać małych fragmentów tekstu od użytkownika (na przykład nazwy użytkownika i hasła) przy użyciu pola tekstowego i program Klawiatura ekranowa:

[![](text-fields-and-search-images/intro01.png "Przykładowe pole wyszukiwania")](text-fields-and-search-images/intro01.png#lightbox)

Opcjonalnie można podać — słowo kluczowe możliwości wyszukiwania zawartości aplikacji przy użyciu pola wyszukiwania:

[![](text-fields-and-search-images/intro02.png "Przykładowe wyniki wyszukiwania")](text-fields-and-search-images/intro02.png#lightbox)

Ten dokument będzie obejmować szczegóły dotyczące pracy z tekstem i pola wyszukiwania w aplikacji Xamarin.tvOS.

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>Tekst i pola wyszukiwania — informacje

Jak już wspomniano, jeśli jest to wymagane, może ona powodować Xamarin.tvOS Twojego jednego lub więcej pól tekstowych zbieranie niewielkich ilości tekstu od użytkownika za pomocą na ekranie (lub klawiatury bluetooth opcjonalna w zależności od wersji systemu tvOS użytkownik zainstalował). 

Ponadto aplikacji wyświetlane dużej ilości zawartości dla użytkownika (na przykład muzyka, filmy lub kolekcji obraz), możesz dołączyć pole wyszukiwania, który umożliwia użytkownikowi wprowadzenie małej ilości tekstu, aby filtrować listę dostępnych elementów.

<a name="Text-Fields" />

## <a name="text-fields"></a>Pola tekstowe

W systemu tvOS, pola tekstowego zostaje przedstawiony jako okno wysokości, zaokrąglona rogu wejścia, które zostaną wyświetlone po kliknięciu przez użytkownika na nim program Klawiatura ekranowa:

[![](text-fields-and-search-images/text01.png "Tekst pola w systemu tvOS")](text-fields-and-search-images/text01.png#lightbox)

Gdy użytkownik przesuwa [fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) do danego pola tekstowego będzie coraz większe i wyświetlić głębokie cienia. Należy pamiętać, to podczas projektowania interfejsu użytkownika, ponieważ pola tekstowe mogą nakładać się na inne elementy interfejsu użytkownika, gdy fokus.

Apple ma poniższe sugestie dotyczące pracy z polami tekstu:

- **Tekst wpisu efektów** — z powodu rodzaj program Klawiatura ekranowa, wprowadzając długich fragmentów tekstu lub wypełniania wielu pól tekstowych jest niewygodne użytkownika. Lepszym rozwiązaniem jest, aby ograniczyć ilość wpisywanie tekstu przy użyciu listach wyboru lub [przyciski](~/ios/tvos/user-interface/buttons.md).
- **Korzystanie ze wskazówek w celu komunikowania się** -pola tekstowego można wyświetlić symbolu zastępczego "wskazówki", gdy parametr jest pusty. Jeśli to możliwe, użyj wskazówek przeznaczenie pola tekstowego zamiast oddzielnych etykiety.
- **Wybierz odpowiedni typ klawiatury domyślne** -systemu tvOS udostępnia kilka, cel wbudowane klawiatury typów określonych dla pola tekstowego. Na przykład klawiatury adres E-mail może ułatwić wpisu, umożliwiając użytkownikowi dokonanie wyboru z listy ostatnio wprowadzone adresy.
- **Gdy jest to konieczne, użyj pola tekstowe Secure** — pole tekstowe Secure przedstawia znaków wprowadzana jako kropki (a nie rzeczywiste litery). Pole tekstowe Secure należy zawsze używać podczas zbierania informacji poufnych, takich jak hasła.

<a name="Keyboards" />

## <a name="keyboards"></a>Klawiatury

Gdy użytkownik kliknie w polu tekstowym w interfejsie użytkownika, liniowej na ekranie zostanie wyświetlony klawiatury. Użytkownik używa dotykać powierzchni [zdalnego Siri](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) wybierz poszczególne koperty z klawiatury i wprowadź wymagane informacje:

[![](text-fields-and-search-images/keyboard01.png "Używanie programu Siri zdalnego klawiatury")](text-fields-and-search-images/keyboard01.png#lightbox)

Jeśli istnieje więcej niż jedno pole tekstowe w bieżącym widoku **dalej** przycisk będzie automatycznie wyświetlany przenieść użytkownika do następnego pola tekstowego. A **gotowe** przycisk będzie wyświetlany dla ostatniego pola tekstowego będzie kończyć wpisywanie tekstu i powrócić do poprzedniego ekranu użytkownika. 

W dowolnym momencie, użytkownik może również nacisnąć **Menu** przycisk na komputerze zdalnym Siri, aby zakończyć wprowadzanie tekstu i ponownie powrócić do poprzedniego ekranu.

Apple ma poniższe sugestie dotyczące pracy z wyświetlanymi klawiatury:

- **Wybierz odpowiedni typ klawiatury domyślne** -systemu tvOS udostępnia kilka, cel wbudowane klawiatury typów określonych dla pola tekstowego. Na przykład klawiatury adres E-mail może ułatwić wpisu, umożliwiając użytkownikowi dokonanie wyboru z listy ostatnio wprowadzone adresy.
- **Gdy jest to konieczne, za pomocą klawiatury akcesoriów widoków** — oprócz standardowego informacje, które są zawsze wyświetlane, opcjonalne widoków akcesoriów (takich jak obrazy lub etykiety) można dodać do program Klawiatura ekranowa objaśnienie jego przeznaczenia wprowadzania tekstu lub pomóc użytkownikowi w wprowadzenie wymaganych informacji.

Aby uzyskać więcej informacji na temat pracy z Klawiatura ekranowa, zobacz firmy Apple [UIKeyboardType](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType), [Zarządzanie klawiatury](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1), [widoki niestandardowe dla danych wejściowych](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) i [ Przewodnik programowania w języku tekstu w systemach iOS](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) dokumentacji.

<a name="Search" />

## <a name="search"></a>Wyszukaj

Pole wyszukiwania stanowią specjalne ekranu, zapewniając pola tekstowego i Klawiatura ekranowa, który umożliwia użytkownikowi filtrować kolekcję elementów, które są wyświetlane poniżej klawiatury:

[![](text-fields-and-search-images/search01.png "Przykładowe wyniki wyszukiwania")](text-fields-and-search-images/search01.png#lightbox)

Jako użytkownik wprowadzi litery w polu wyszukiwania, poniższe wyniki zostanie automatycznie uwzględniona wyniki wyszukiwania. W dowolnym momencie użytkownik może przenieść fokus na wyniki i wybierz jeden z elementów przedstawiony.

Apple ma poniższe sugestie dotyczące pracy z polami wyszukiwania:

- **Podaj ostatnie wyszukiwania** — ponieważ wprowadzanie tekstu za pomocą zdalnego Siri może być niewygodny i użytkownicy często powtarzane żądania wyszukiwania, należy rozważyć dodanie sekcji najnowszych wyników wyszukiwania przed bieżącym wyników w obszarze klawiatury.
- **Jeśli to możliwe, należy ograniczyć liczbę wyników** — ponieważ dużych listy elementów może być trudne dla użytkownika przeanalizować i przejść, należy rozważyć ograniczenie liczby zwracanych wyników.
- **W razie potrzeby podaj wynik wyszukiwania filtry** — Jeśli zawartość dostarczone przez aplikację jest przydatna, należy rozważyć dodanie paski zakres umożliwia użytkownikom dokładniej przefiltrować wyniki wyszukiwania zwrócone.

Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania do klasy UISearchController](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html).

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>Praca z pola tekstowe

Najprostszym sposobem, aby pracować z pól tekstowych w aplikacji Xamarin.tvOS jest je dodać do projektu interfejsu użytkownika przy użyciu projektanta dla systemu iOS.

Wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. W **konsoli rozwiązania**, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji.
1. Przeciągnij co najmniej jeden **pól tekstowych** int powierzchnię na widok: 

    [![](text-fields-and-search-images/text02.png "Pole tekstowe")](text-fields-and-search-images/text02.png#lightbox)
1. Wybierz **pól tekstowych** i nadaj każdej unikatowego **nazwa** w **elementu Widget** karcie **konsoli właściwości**: 

    [![](text-fields-and-search-images/text03.png "Karta Widget właściwości konsoli")](text-fields-and-search-images/text03.png#lightbox)
1. W **pola tekstowego** sekcji, można zdefiniować elementów, takich jak **symbolu zastępczego** wskazówki i domyślne **wartość**: 

    [![](text-fields-and-search-images/text04.png "W sekcji pola tekstowego.")](text-fields-and-search-images/text04.png#lightbox)
1. Przewiń w dół do definiowania właściwości, takie jak **sprawdzanie pisowni**, **wielkość liter** i domyślnie **typu klawiatury**: 

    [![](text-fields-and-search-images/text05.png "Sprawdzanie pisowni, wielkość liter i domyślnego typu klawiatury")](text-fields-and-search-images/text05.png#lightbox) 
1. Zapisać zmiany w Twojej scenorysu.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji.
1. Przeciągnij co najmniej jeden **pól tekstowych** int powierzchnię na widok: 

    [![](text-fields-and-search-images/text02-vs.png "Pole tekstowe")](text-fields-and-search-images/text02-vs.png#lightbox)
1. Wybierz **pól tekstowych** i nadaj każdej unikatowego **nazwa** w **elementu Widget** karcie **Explorer właściwości**: 

    [![](text-fields-and-search-images/text03-vs.png "Na karcie widżetu")](text-fields-and-search-images/text03-vs.png#lightbox)
1. W **pola tekstowego** sekcji, można zdefiniować elementów, takich jak **symbolu zastępczego** wskazówki i domyślne **wartość**: 

    [![](text-fields-and-search-images/text04-vs.png "W sekcji pola tekstowego.")](text-fields-and-search-images/text04-vs.png#lightbox)
1. Przewiń w dół do definiowania właściwości, takie jak **sprawdzanie pisowni**, **wielkość liter** i domyślnie **typu klawiatury**: 

    [![](text-fields-and-search-images/text05-vs.png "Sprawdzanie pisowni, wielkość liter i domyślnego typu klawiatury")](text-fields-and-search-images/text05-vs.png#lightbox) 
1. Zapisać zmiany w Twojej scenorysu.
    
-----

W kodzie, można uzyskać lub ustaw wartość pola tekstowego przy użyciu jego `Text` właściwości:

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

Opcjonalnie można użyć `Started` i `Ended` zdarzenia pola tekstowego odpowiedzieć na wpisywanie tekstu początkową i końcową.

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>Praca z pola wyszukiwania

Najprostszym sposobem, aby pracować z pola wyszukiwania w aplikacji Xamarin.tvOS jest je dodać do projektu interfejsu użytkownika przy użyciu narzędzia Projektant interfejsu.

Wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. W **konsoli rozwiązania**, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji.
1. Przeciągnij nowego kontrolera widoku kolekcji do scenorysu do prezentowania wyniki wyszukiwania użytkownika: 

    [![](text-fields-and-search-images/search02.png "Kontroler widoku kolekcji")](text-fields-and-search-images/search02.png#lightbox)
1. W **elementu Widget** karcie **konsoli właściwości**, użyj `SearchResultsViewController` dla **klasy** i `SearchResults` dla **identyfikator scenorysu**: 

    [![](text-fields-and-search-images/search03.png "Na karcie widżetu")](text-fields-and-search-images/search03.png#lightbox)
1. Wybierz **prototypu komórki** na powierzchni projektu.
1. W **elementu Widget** karcie **Explorer właściwości**, użyj `SearchResultCell` dla **klasy** i `ImageCell` dla **identyfikator**: 

    [![](text-fields-and-search-images/search04.png "Na karcie widżetu")](text-fields-and-search-images/search04.png#lightbox)
1. Układ projektu z **prototypu komórki** i ujawnia każdy element przy użyciu unikatowego **nazwa** w **elementu Widget** karty **Explorer właściwości**: 

    [![](text-fields-and-search-images/search05.png "Układ projektu prototypu komórki")](text-fields-and-search-images/search05.png#lightbox)
1. Zapisać zmiany w Twojej scenorysu.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji.
1. Przeciągnij nowego kontrolera widoku kolekcji do scenorysu do prezentowania wyniki wyszukiwania użytkownika: 

    [![](text-fields-and-search-images/seach02-vs.png "Kontroler widoku kolekcji")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. W **elementu Widget** karcie **Explorer właściwości**, użyj `SearchResultsViewController` dla **klasy** i `SearchResults` dla **scenorysu identyfikator**: 

    [![](text-fields-and-search-images/search03-vs.png "Na karcie widżetu")](text-fields-and-search-images/search03-vs.png#lightbox)
1. Wybierz **prototypu komórki** na powierzchni projektu.
1. W **elementu Widget** karcie **Explorer właściwości**, użyj `SearchResultCell` dla **klasy** i `ImageCell` dla **identyfikator**: 

    [![](text-fields-and-search-images/search04-vs.png "Na karcie widżetu")](text-fields-and-search-images/search04-vs.png#lightbox)
1. Układ projektu z **prototypu komórki** i ujawnia każdy element przy użyciu unikatowego **nazwa** w **elementu Widget** karty **Explorer właściwości**: 

    [![](text-fields-and-search-images/search05-vs.png "Układ projektu prototypu komórki")](text-fields-and-search-images/search05-vs.png#lightbox)
1. Zapisać zmiany w Twojej scenorysu.
    
-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>Określanie modeli danych

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Następnie należy podać klasę do działania jako Model danych dla wyników, który użytkownik zostanie wyszukiwanie. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Dodaj** > **nowego pliku...**   >  **Ogólne** > **pustą klasę** i podaj **nazwa**: 

[![](text-fields-and-search-images/search06.png "Wybierz pustą klasę i podaj nazwę")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Następnie należy podać klasę do działania jako Model danych dla wyników, który użytkownik zostanie wyszukiwanie. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Dodaj** > **nowy element...**   >  **Apple** > **różne** > **klasy** i podaj **nazwa**: 

[![](text-fields-and-search-images/search06-vs.png "Wybierz klasę i podaj nazwę")](text-fields-and-search-images/search06-vs.png#lightbox)

-----

Na przykład aplikacja, która umożliwia użytkownikowi do wyszukiwania kolekcji obrazów, tytuł i słowo kluczowe może wyglądać następująco:

```csharp
using System;
using Foundation;

namespace tvText
{
    public class PictureInformation : NSObject
    {
        #region Computed Properties
        public string Title { get; set;}
        public string ImageName { get; set;}
        public string Keywords { get; set;}
        #endregion

        #region Constructors
        public PictureInformation (string title, string imageName, string keywords)
        {
            // Initialize
            this.Title = title;
            this.ImageName = imageName;
            this.Keywords = keywords;
        }
        #endregion
    }
}
```

<a name="The-Collection-View-Cell" />

### <a name="the-collection-view-cell"></a>Kolekcja komórek widoku

W modelu danych w miejscu, należy edytować **komórki prototypu** (`SearchResultViewCell.cs`) i zapewnić ich wyglądu znajdują się następujące:

```csharp
using Foundation;
using System;
using UIKit;

namespace tvText
{
    public partial class SearchResultViewCell : UICollectionViewCell
    {
        #region Private Variables
        private PictureInformation _pictureInfo = null;
        #endregion

        #region Computed Properties
        public PictureInformation PictureInfo {
            get { return _pictureInfo; }
            set {
                _pictureInfo = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            UpdateUI ();
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Anything to process?
            if (PictureInfo == null) return;

            try {
                Picture.Image = UIImage.FromBundle (PictureInfo.ImageName);
                Picture.AdjustsImageWhenAncestorFocused = true;
                Title.Text = PictureInfo.Title;
                TextColor = UIColor.LightGray;
            } catch {
                // Ignore errors if view isn't fully loaded
            }
        }
        #endregion
    }

}
```

`UpdateUI` Metoda będzie używany do wyświetlania poszczególnych pól **PictureInformation** elementów ( `PictureInfo` właściwości) w elementy interfejsu użytkownika o każdym właściwość jest aktualizowany. Na przykład obraz i tytuł skojarzone z obrazem.

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>Kontroler widoku kolekcji

Następnie edytuj kontroler widoku kolekcji wyniki wyszukiwania (`SearchResultsViewController.cs`) i przydzielić mu wyglądać następująco:

```csharp
using Foundation;
using System;
using UIKit;
using System.Collections.Generic;

namespace tvText
{
    public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
    {
        #region Constants
        public const string CellID = "ImageCell";
        #endregion

        #region Private Variables
        private string _searchFilter = "";
        #endregion

        #region Computed Properties
        public List<PictureInformation> AllPictures { get; set;}
        public List<PictureInformation> FoundPictures { get; set; }
        public string SearchFilter {
            get { return _searchFilter; }
            set {
                _searchFilter = value.ToLower();
                FindPictures ();
                CollectionView?.ReloadData ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultsViewController (IntPtr handle) : base (handle)
        {
            // Initialize
            this.AllPictures = new List<PictureInformation> ();
            this.FoundPictures = new List<PictureInformation> ();
            PopulatePictures ();
            FindPictures ();

        }
        #endregion

        #region Private Methods
        private void PopulatePictures ()
        {
            // Clear list
            AllPictures.Clear ();

            // Add images
            AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
            AllPictures.Add (new PictureInformation ("Cheese Plate", "CheesePlate", "cheese,plate,bread"));
            AllPictures.Add (new PictureInformation ("Coffee House", "CoffeeHouse", "coffee,people,menu,restaurant,cafe"));
            AllPictures.Add (new PictureInformation ("Computer and Expresso", "ComputerExpresso", "computer,coffee,expresso,phone,notebook"));
            AllPictures.Add (new PictureInformation ("Hamburger", "Hamburger", "meat,bread,cheese,tomato,pickle,lettus"));
            AllPictures.Add (new PictureInformation ("Lasagna Dinner", "Lasagna", "salad,bread,plate,lasagna,pasta"));
            AllPictures.Add (new PictureInformation ("Expresso Meeting", "PeopleExpresso", "people,bag,phone,expresso,coffee,table,tablet,notebook"));
            AllPictures.Add (new PictureInformation ("Soup and Sandwich", "SoupAndSandwich", "soup,sandwich,bread,meat,plate,tomato,lettus,egg"));
            AllPictures.Add (new PictureInformation ("Morning Coffee", "TabletCoffee", "tablet,person,man,coffee,magazine,table"));
            AllPictures.Add (new PictureInformation ("Evening Coffee", "TabletMagCoffee", "tablet,magazine,coffee,table"));
        }

        private void FindPictures ()
        {
            // Clear list
            FoundPictures.Clear ();

            // Scan each picture for a match
            foreach (PictureInformation picture in AllPictures) {
                if (SearchFilter == "") {
                    // If no search term, everything matches
                    FoundPictures.Add (picture);
                } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
                    // If the search term is in the title or keywords, we've found a match
                    FoundPictures.Add (picture);
                }
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            // Only one section in this collection
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            // Return the number of matching pictures
            return FoundPictures.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a new cell and return it
            var cell = collectionView.DequeueReusableCell (CellID, indexPath);
            return (UICollectionViewCell)cell;
        }

        public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
        {
            // Grab the cell
            var currentCell = cell as SearchResultViewCell;
            if (currentCell == null)
                throw new Exception ("Expected to display a `SearchResultViewCell`.");

            // Display the current picture info in the cell
            var item = FoundPictures [indexPath.Row];
            currentCell.PictureInfo = item;
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // If this Search Controller was presented as a modal view, close
            // it before continuing
            // DismissViewController (true, null);

            // Grab the picture being selected and report it
            var picture = FoundPictures [indexPath.Row];
            Console.WriteLine ("Selected: {0}", picture.Title);
        }

        public void UpdateSearchResultsForSearchController (UISearchController searchController)
        {
            // Save the search filter and update the Collection View
            SearchFilter = searchController.SearchBar.Text ?? string.Empty;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
            if (previousItem != null) {
                UIView.Animate (0.2, () => {
                    previousItem.TextColor = UIColor.LightGray;
                });
            }

            var nextItem = context.NextFocusedView as SearchResultViewCell;
            if (nextItem != null) {
                UIView.Animate (0.2, () => {
                    nextItem.TextColor = UIColor.Black;
                });
            }
        }
        #endregion
    }
}
```

Najpierw `IUISearchResultsUpdating` interfejsu jest dodawana do klasy do obsługi filtru wyszukiwania kontrolera aktualizowana przez użytkownika:

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

Stałej jest także zdefiniowana, aby określić identyfikator **komórki prototypu** (który jest zgodny z Identyfikatorem zdefiniowanego w Projektancie interfejsu powyżej) który zostanie później użyty, gdy kontroler kolekcji zażąda nowej komórki:

```csharp
public const string CellID = "ImageCell";
```

Magazynu jest tworzone dla pełną listę elementów przeszukiwany wyszukiwany termin filtr oraz lista elementów pasujących do tego terminu:

```csharp
private string _searchFilter = "";
...

public List<PictureInformation> AllPictures { get; set;}
public List<PictureInformation> FoundPictures { get; set; }
public string SearchFilter {
    get { return _searchFilter; }
    set {
        _searchFilter = value.ToLower();
        FindPictures ();
        CollectionView?.ReloadData ();
    }
}
```

Gdy `SearchFilter` jest zmieniony, listy zgodnych elementów jest aktualizowana, a widok kolekcji, zawartość zostanie ponownie załadowana. `FindPictures` Procedura jest odpowiedzialny za wyszukiwanie elementów, które pasują do nowego szukanego terminu:

```csharp
private void FindPictures ()
{
    // Clear list
    FoundPictures.Clear ();

    // Scan each picture for a match
    foreach (PictureInformation picture in AllPictures) {
        if (SearchFilter == "") {
            // If no search term, everything matches
            FoundPictures.Add (picture);
        } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
            // If the search term is in the title or keywords, we've found a match
            FoundPictures.Add (picture);
        }
    }
}
```

Wartość `SearchFilter` zostaną zaktualizowane (który spowoduje zaktualizowanie widok wyników kolekcji) gdy użytkownik zmieni filtru w kontrolerze wyszukiwania:

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

`PopulatePictures` Metoda wstępnie wypełnia kolekcję dostępne elementy:

```csharp
private void PopulatePictures ()
{
    // Clear list
    AllPictures.Clear ();

    // Add images
    AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
    ...
}
```

Ze względu na przykład wszystkie dane przykładowe jest tworzony w pamięci podczas ładowania kontroler widoku kolekcji. W rzeczywistym aplikacji te dane prawdopodobnie będzie można odczytać z bazy danych, usługi sieci web, a tylko w razie potrzeby, aby zapobiec przekroczenia telewizora Apple ograniczony pamięci.

`NumberOfSections` i `GetItemsCount` metody udostępniają Liczba pasujących elementów:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    // Only one section in this collection
    return 1;
}

public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    // Return the number of matching pictures
    return FoundPictures.Count;
}
```

`GetCell` Metoda zwraca nową **komórki prototypu** (na podstawie `CellID` zdefiniowanych powyżej w scenorysu) dla każdego elementu w widoku kolekcji:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

`WillDisplayCell` Metoda jest wywoływana przed komórki wyświetlanej, można skonfigurować:

```csharp
public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
{
    // Grab the cell
    var currentCell = cell as SearchResultViewCell;
    if (currentCell == null)
        throw new Exception ("Expected to display a `SearchResultViewCell`.");

    // Display the current picture info in the cell
    var item = FoundPictures [indexPath.Row];
    currentCell.PictureInfo = item;
}
```

`DidUpdateFocus` Metoda zapewnia wizualne dla użytkownika zgodnie z ich Wyróżnij elementy w widoku zbierania wyników:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
    if (previousItem != null) {
        UIView.Animate (0.2, () => {
            previousItem.TextColor = UIColor.LightGray;
        });
    }

    var nextItem = context.NextFocusedView as SearchResultViewCell;
    if (nextItem != null) {
        UIView.Animate (0.2, () => {
            nextItem.TextColor = UIColor.Black;
        });
    }
}
```

Na koniec `ItemSelected` metoda obsługuje użytkownika, wybierając element (kliknięcie na powierzchni Touch za pomocą zdalnego Siri) w widoku zbierania wyników:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // If this Search Controller was presented as a modal view, close
    // it before continuing
    // DismissViewController (true, null);

    // Grab the picture being selected and report it
    var picture = FoundPictures [indexPath.Row];
    Console.WriteLine ("Selected: {0}", picture.Title);
}
```

Jeśli pole wyszukiwania został przedstawiony jako widok modalnego okna dialogowego (za pośrednictwem góry widoku wywoływanie jej), użyj `DismissViewController` metodę, aby odrzucić widoku wyszukiwania, gdy użytkownik wybierze element. Na przykład pole wyszukiwania jest przedstawiany jako zawartości na karcie Widok kartę tak nie jest on ukryty w tutaj.

Aby uzyskać więcej informacji na temat widoków kolekcji, zobacz nasze [Praca z widokami Kolekcja](~/ios/tvos/user-interface/collection-views.md) dokumentacji.

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>Umożliwienie korzystania z pola wyszukiwania

Istnieją dwa główne sposoby który pole wyszukiwania (i jego skojarzony program Klawiatura ekranowa i wyniki wyszukiwania) może być prezentowana w systemu tvOS do użytkownika: 

- **W widoku okna dialogowego modalne** — pole wyszukiwania może być prezentowana przez bieżący widok i kontroler widoku jako widok pełnoekranowy modalnego okna dialogowego. Zazwyczaj jest to realizowane w odpowiedzi na kliknięcie przycisku lub innego elementu interfejsu użytkownika przez użytkownika. Okno dialogowe jest odrzucane po wybraniu elementu w wynikach wyszukiwania.
- **Wyświetlanie zawartości** — pole wyszukiwania jest bezpośrednie częścią danego widoku. Na przykład jako zawartość na karcie wyszukiwania kontrolera widoku kartę.

Na przykład można wyszukiwać listy obrazów z powyższych pole wyszukiwania jest widoczne jako widok zawartości na karcie wyszukiwania i kontroler widoku wyszukiwania kartę wygląda następująco:

```csharp
using System;
using UIKit;

namespace tvText
{
    public partial class SecondViewController : UIViewController
    {
        #region Constants
        public const string SearchResultsID = "SearchResults";
        #endregion

        #region Computed Properties
        public SearchResultsViewController ResultsController { get; set;}
        #endregion

        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        public void ShowSearchController ()
        {
            // Build an instance of the Search Results View Controller from the Storyboard
            ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
            if (ResultsController == null)
                throw new Exception ("Unable to instantiate a SearchResultsViewController.");

            // Create an initialize a new search controller
            var searchController = new UISearchController (ResultsController) {
                SearchResultsUpdater = ResultsController,
                HidesNavigationBarDuringPresentation = false
            };

            // Set any required search parameters
            searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

            // The Search Results View Controller can be presented as a modal view
            // PresentViewController (searchController, true, null);

            // Or in the case of this sample, the Search View Controller is being
            // presented as the contents of the Search Tab directly. Use either one
            // or the other method to display the Search Controller (not both).
            var container = new UISearchContainerViewController (searchController);
            var navController = new UINavigationController (container);
            AddChildViewController (navController);
            View.Add (navController.View);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // If the Search Controller is being displayed as the content
            // of the search tab, include it here.
            ShowSearchController ();
        }

        public override void ViewDidAppear (bool animated)
        {
            base.ViewDidAppear (animated);

            // If the Search Controller is being presented as a modal view,
            // call it here to display it over the contents of the Search
            // tab.
            // ShowSearchController ();
        }
        #endregion
    }
}
```

Najpierw stałą zdefiniowano odpowiadający **identyfikator scenorysu** który został przypisany do kontrolera widoku zbierania wyników wyszukiwania w Projektancie interfejsu:

```csharp
public const string SearchResultsID = "SearchResults";
```

Następnie `ShowSearchController` metoda tworzy nowy kontroler kolekcji widoku wyszukiwania i wyświetla go było wymagane:

```csharp
public void ShowSearchController ()
{
    // Build an instance of the Search Results View Controller from the Storyboard
    ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
    if (ResultsController == null)
        throw new Exception ("Unable to instantiate a SearchResultsViewController.");

    // Create an initialize a new search controller
    var searchController = new UISearchController (ResultsController) {
        SearchResultsUpdater = ResultsController,
        HidesNavigationBarDuringPresentation = false
    };

    // Set any required search parameters
    searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

    // The Search Results View Controller can be presented as a modal view
    // PresentViewController (searchController, true, null);

    // Or in the case of this sample, the Search View Controller is being
    // presented as the contents of the Search Tab directly. Use either one
    // or the other method to display the Search Controller (not both).
    var container = new UISearchContainerViewController (searchController);
    var navController = new UINavigationController (container);
    AddChildViewController (navController);
    View.Add (navController.View);
}
```

W powyższej metodzie raz `SearchResultsViewController` zostały utworzone z scenorysu, nowy `UISearchController` utworzone Wyświetla pole wyszukiwania i program Klawiatura ekranowa dla użytkownika. Zbieranie wyników wyszukiwania (zgodnie z definicją `SearchResultsViewController`) zostanie wyświetlony w obszarze tej klawiatury.

Następnie `SearchBar` jest skonfigurowana z informacjami, takich jak **symbolu zastępczego** wskazówki. Zapewnia informacje dla użytkownika o typie wyszukiwania jest przeprowadzana.

Następnie pole wyszukiwania jest widoczne dla użytkownika w jeden z dwóch sposobów:

- **W widoku okna dialogowego modalne** — `PresentViewController` metoda jest wywoływana do prezentowania wyszukiwania przez istniejący widok pełnego ekranu.
- **Wyświetlanie zawartości** — `UISearchContainerViewController` zawiera kontroler wyszukiwania. A `UINavigationController` jest utworzony kontenera wyszukiwania kontrolera nawigacji jest dodawana do kontrolera widoku `AddChildViewController (navController)`oraz widok przedstawione `View.Add (navController.View)`.

Na koniec i ponownie na podstawie typu prezentacji albo `ViewDidLoad` lub `ViewDidAppear` wywoła metodę `ShowSearchController` metody do prezentowania wyszukiwania dla użytkownika:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // If the Search Controller is being displayed as the content
    // of the search tab, include it here.
    ShowSearchController ();
}

public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    // If the Search Controller is being presented as a modal view,
    // call it here to display it over the contents of the Search
    // tab.
    // ShowSearchController ();
}
```

Aplikacja jest uruchamiana po karta wyszukiwanie wybrane przez użytkownika, niefiltrowane pełną listę elementów zobaczy dla użytkownika:

[![](text-fields-and-search-images/intro02.png "Wyniki wyszukiwania domyślne")](text-fields-and-search-images/intro02.png#lightbox)

Gdy użytkownik rozpoczyna wprowadź wyszukiwany termin, na liście wyników zostaną przefiltrowane przez czas i aktualizowane automatycznie:

[![](text-fields-and-search-images/intro03.png "Filtrowane wyniki wyszukiwania")](text-fields-and-search-images/intro03.png#lightbox)

W dowolnym momencie użytkownik może przełączyć fokus do elementu w wynikach wyszukiwania, a następnie kliknij przycisk powierzchni Touch zdalnego Siri, aby go wybrać.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego projektowanie i Praca z pola wyszukiwania i tekst wewnątrz aplikacji Xamarin.tvOS. Pokazano, jak utworzyć tekst i kolekcji wyszukiwania zawartości w Projektancie interfejsu, a potem na dwa różne sposoby, które pola wyszukiwania może być przedstawiony w systemu tvOS do użytkownika.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
