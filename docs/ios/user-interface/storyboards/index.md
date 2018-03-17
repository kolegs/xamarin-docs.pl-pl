---
title: Wprowadzenie do scenorysu
description: "Scenorysu jest wizualną reprezentację wygląd i przepływu aplikacji. Xamarin wprowadziła projektanta, aby umożliwić aplikacjom Xamarin.iOS zalet scenorys, dzięki czemu można zaprojektować wizualnie ekranu aplikacji i dostępu do widoków, kontrolery i segues w języku C# Aby uzyskać większą kontrolę."
ms.topic: article
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: a0b0ca9857e706a9a84f1c661f7f6ff294e112c1
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="introduction-to-storyboards"></a>Wprowadzenie do scenorysu

_Scenorysu jest wizualną reprezentację wygląd i przepływu aplikacji. Xamarin wprowadziła projektanta, aby umożliwić aplikacjom Xamarin.iOS zalet scenorys, dzięki czemu można zaprojektować wizualnie ekranu aplikacji i dostępu do widoków, kontrolery i segues w języku C# Aby uzyskać większą kontrolę._

W tym przewodniku Wyjaśnijmy, jakie scenorysu jest i sprawdź, czy niektóre najważniejsze składniki — takie jak Segues. Wyjaśniono, jak Scenorys można tworzyć i używane, i jakie zalety mają dla deweloperów.

Przed format pliku scenorysu została wprowadzona przez firmę Apple jako wizualną reprezentację interfejsu użytkownika aplikacji systemu iOS, deweloperzy utworzone pliki XIB dla każdego kontrolera widoku i ręcznie zaprogramowany nawigacji między każdym widoku.  Za pomocą scenorysu deweloperom definiować zarówno kontrolery widoku i nawigacji między nimi na powierzchni projektowej i udostępnia edytowanie w trybie WYSIWYG interfejsu użytkownika aplikacji.

Scenorysu można tworzyć, otworzyć i edytować za pomocą platformy Xamarin iOS projektanta. W tym przewodniku będzie również wskazówki jak utworzyć scenorys z podczas przy użyciu języka C# program nawigacji za pomocą projektanta.


## <a name="requirements"></a>Wymagania

Scenorys można z projektanta w programie Visual Studio for Mac z systemem iOS lub z programu Visual Studio 2015 i 2017 z obciążeniami Xamarin zainstalowane.

## <a name="what-is-a-storyboard"></a>Co to jest scenorysu?

Scenorysu jest wizualną reprezentację wszystkich ekranów w aplikacji. Zawiera sekwencję sceny z każdym reprezentujący sceny *kontrolera widoku* i jego *widoków*. Widoki te mogą zawierać obiekty i [formanty](~/ios/user-interface/controls/index.md) która umożliwi użytkownikowi interakcji z aplikacją. Ta kolekcja widoków i kontrolek (lub *widoków podrzędnych*) jest nazywany *hierarchii widok zawartości*. Sceny są połączone przez segue obiektów, które reprezentują przejścia między kontrolerami widoku. Zazwyczaj jest to osiągane przez utworzenie segue między obiektu na widok początkowy, a następnie Wyświetl nawiązującego połączenie. Na poniższym obrazie przedstawiono relacje na powierzchni projektu:

 [![](images/storyboardsview.png "Relacje na powierzchni projektu zostały przedstawione w tym obrazie")](images/storyboardsview.png#lightbox)

Co zostało pokazane, scenorysu będzie Ułóż każdego użytkownika sceny z zawartością już renderowana i przedstawia połączenia między nimi.  Warto zauważyć w tym momencie, że przy omawianiu sceny na telefonie iPhone jest bezpieczne założono, że jeden *sceny* dla scenorysu jest równa jeden *ekranu* zawartości na urządzeniu. Jednak za pomocą iPad, które można mieć wiele scen występować jednocześnie — na przykład przy użyciu kontrolera widoku Popover.

Istnieje wiele zalet z użyciem scenorys do tworzenia interfejsu użytkownika aplikacji, szczególnie w przypadku, gdy za pomocą platformy Xamarin. Po pierwsze, jest wizualną reprezentację interfejsu użytkownika, jako wszystkich obiektów — w tym [Kontrolki niestandardowe](~/ios/user-interface/designer/ios-designable-controls-overview.md) — są renderowane w czasie projektowania. Oznacza to, że przed tworzenia lub wdrażania aplikacji można zwizualizować wygląd i przepływu. Na przykład mieć powyższy obraz. Firma Microsoft może odróżnić od krótki przegląd projektu powierzchni sceny ile są, układ każdego widoku i jak wszystko, co jest powiązane. Jest to, co sprawia, że Scenorys tak zaawansowanego.

Zdarzenia są łatwiejsze w zarządzaniu z Scenorys, zwłaszcza w przypadku korzystania z systemem iOS projektanta. Większość formantów interfejsu użytkownika będzie mieć listę możliwych zdarzeń w konsoli do właściwości. Program obsługi zdarzeń można tu dodać i zostało ukończone w częściowej metody w klasie widoku kontrolerów..

Zawartość scenorysu jest przechowywana jako plik XML. Czas, kompilacji AT żadnych `.storyboard` pliki są kompilowane do plików binarnych, znany jako nibs. W czasie wykonywania tych nibs są zainicjowany i wystąpienia do tworzenia nowych widoków.

## <a name="segues"></a>Segues

A *Segue*, lub *Segue obiektu*, jest używana w opracowywania aplikacji systemu iOS do reprezentowania przejścia między sceny. Aby utworzyć segue, przytrzymaj **Ctrl** klucza i kliknij i przeciągnij od jednej sceny do innego. Przeciągania naszych mysz, pojawi się niebieski łącznika, wskazujące, gdzie segue doprowadzi, jak pokazano na poniższej ilustracji:

 [![](images/createsegue.png "Pojawi się niebieski łącznika, wskazujące, gdzie segue doprowadzi, jak pokazano w tym obrazie")](images/createsegue.png#lightbox)

Na myszy w górę zostanie wyświetlone menu, umożliwiając nam wybierz akcję dla naszych segue. Może wyglądać podobnie do poniższej obrazów: 

**Wstępne iOS 8 i rozmiarze klasy**:

[![](images/segue1.png "Lista rozwijana Segue akcji bez klasy wielkości")](images/segue1.png#lightbox)

**Podczas używania klas wielkości i adaptacyjną Segues**:

[![](images/16new.png "Lista rozwijana Segue akcji z klasami rozmiar")](images/16new.png#lightbox)

> [!IMPORTANT]
> **Uwaga:** Jeśli korzystasz z programu VMWare do maszyny wirtualnej systemu Windows, przytrzymując klawisz Ctrl jest mapowany jako _kliknij prawym przyciskiem myszy_ przycisk myszy domyślnie. Aby utworzyć Segue, Edytuj preferencje klawiatury za pośrednictwem **preferencje** > **klawiatura i mysz** > **skróty myszy** i ponownie zamapować użytkownika **Przycisk dodatkowej** jak przedstawiono poniżej:
> 
> [![](images/image22.png "Klawiatura i mysz ustawień preferencji")](images/image22.png#lightbox)
> 
> Teraz można dodać segue między kontrolerami widoku normalnie.

Istnieją różne typy każdego dające kontrolę nad sposób prezentowania nowego kontrolera widoku użytkownika i jak współdziała z innymi kontrolerami widoku w scenorysu, przejścia. Poniżej opisano te. Istnieje również możliwość do podklasy obiektu segue do zaimplementowania niestandardowego przejścia:

-  **Pokaż / Push** — segue wypychanej dodaje kontroler widoku stos nawigacji. Zakłada się, że kontroler widoku pochodzących wypychania jest częścią tego samego kontrolera nawigacji jako kontroler widoku, który jest dodawany do stosu. To jest odpowiednikiem `pushViewController` i jest zazwyczaj stosowane, gdy ma niektórych zależności między danymi na ekranach. Przy użyciu powiadomienia wypychanego segue daje możliwość zaprojektowania kątem zabezpieczeń o pasek nawigacyjny z przycisku Wstecz i tytuł dodane do każdego widoku w stosie, umożliwiając Przechodzenie do nawigacji w widoku hierarchii.
-  **Modalne** — modalne segue Utwórz relację między żadnych kontrolerów dwóch widoku w projekcie z opcją animowany przejścia jest pokazywany. Kontroler widoku podrzędne zostaną całkowicie przesłaniać kontroler widoku nadrzędnego, gdy przeznaczone do wyświetlenia. W przeciwieństwie do wypychania segue, która dodaje przycisku Wstecz w firmie Microsoft; Po użyciu modalne segue `DismissViewController` musi być używany w celu powrotu do poprzedniego kontrolera widoku.
-  **Niestandardowe** — wszystkie niestandardowe segue mogą być tworzone jako podklasa ` UIStoryboardSegue`.
-  **Unwind** — moduł unwind segue może służyć do przechodzenia wstecz za pomocą wypychania lub modalne segue — na przykład, odrzucając kontrolera widoku w trybie modalnym przedstawiony. Oprócz tego można unwind za pomocą nie tylko jednego, ale szereg wypychania i modalne segues i wrócić do poprzedniej strony się, że wiele czynności ułożonych w hierarchii nawigacji za pomocą jednej operacji unwind akcji. Aby zrozumieć, jak używać unwind segue w systemie iOS, przeczytaj [tworzenie Unwind Segues](https://developer.xamarin.com/recipes/ios/general/storyboard/unwind_segue/) przepisu.
-  **Sourceless** — sourceless segue wskazuje sceny zawierającej kontroler widoku początkowej i w związku z tym którego widoku użytkownik zostanie wyświetlony pierwszy. Jest on reprezentowany przez segue pokazano poniżej:  

    [![](images/sourcelesssegue.png "Sourceless segue")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>Adaptacyjną Segue typów

 System iOS 8 wprowadzono [klasy wielkości](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) umożliwia plikiem scenorysu z systemem iOS do pracy z rozmiaru obrazu, umożliwiają deweloperom tworzenie jednego interfejsu użytkownika dla wszystkich urządzeń z systemem iOS. Domyślnie wszystkie nowej aplikacji platformy Xamarin.iOS użyje rozmiaru klasy. Aby używać klas rozmiaru z starsze projektu, należy zapoznać się [wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) przewodnik. 
 
Aplikacje korzystające z klasy wielkości będą też używać nowego [ *adaptacyjną Segues*](~/ios/user-interface/storyboards/unified-storyboards.md). Podczas korzystania z klasy wielkości, pamiętaj, że firma Microsoft nie są bezpośrednio określać wether używamy iPhone lub iPad. Innymi słowy tworzymy jednego interfejsu użytkownika, który zawsze będzie wyglądać takie same, niezależnie od tego, ile nieruchomości musi współpracować. Adaptacyjną pracy Segues oceny środowiska i określania najlepszy sposób prezentowanie zawartości. Adaptacyjną Segues są pokazane poniżej: 

[![](images/adaptivesegue.png "Lista rozwijana adaptacyjną Segues")](images/adaptivesegue.png#lightbox)

|Segue|Opis|
|--- |--- |
|Pokaż|To jest bardzo podobny do wypychania segue, lecz uwzględnia ona zawartości ekranu.|
|Pokaż szczegóły|Jeśli aplikacja wyświetla widok główny i szczegóły (na przykład w kontrolerze widoku podziału na urządzeniu iPad), zawartości spowoduje zastąpienie widok szczegółów. Jeśli aplikacja wyświetla tylko głównego lub szczegółów, zawartości spowoduje zastąpienie ze szczytu stosu kontrolera widoku.|
|Prezentacji|To jest podobny do segue modalne i pozwala na wybór stylów prezentacji i przejścia.|
|Popover prezentacji|Stwarza zawartości jako popover|

### <a name="transferring-data-with-segues"></a>Transfer danych z Segues

Korzyści wynikające z segue nie kończyć przejścia. Mogą one również używane do zarządzania przesyłaniem danych między kontrolerami widoku. Jest to osiągane przez zastąpienie `PrepareForSegue` metody na kontrolerze widok początkowy i obsługi danych, nad. Po wyzwoleniu segue — na przykład o naciśnięcie przycisku — aplikacja zostanie wywołać tę metodę, zapewniając możliwość przygotowanie nowego kontrolera widoku *przed* żadnych nawigacji. Kod poniżej, z [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) przykładowe, pokazano to: 


```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, 
NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryContoller = segue.DestinationViewController 
                                  as CallHistoryController;

    if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
}
```

W tym przykładzie `PrepareForSegue` metoda zostanie wywołana po wyzwoleniu segue przez użytkownika. Musimy najpierw utworzyć wystąpienia "do odbierania" kontroler widoku i Ustaw jako miejsce docelowe segue kontrolera widoku. Można to zrobić za pomocą wiersza kodu poniżej:

```csharp
var callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Metoda ma teraz możliwość ustawienia właściwości na `DestinationViewController`. W tym przykładzie przekierowaliśmy dzięki temu, przekazując listę o nazwie `PhoneNumbers` do `CallHistoryController` i przypisywania go do obiektu o takiej samej nazwie:

```csharp
if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
```

Po zakończeniu przejścia, użytkownik będzie widział `CallHistoryController` z listą wypełnione.

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>Dodawanie do projektu z systemem innym niż scenorysu scenorysu

Czasami może być konieczne dodanie scenorysu w pliku wcześniej z systemem innym niż scenorysu. Po tej czynności w programie Visual Studio for Mac może usprawnić przez następujące kroki:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Utwórz nowy plik scenorysu, przechodząc do **Plik > Nowy plik > iOS > scenorysu**, jak pokazano poniżej: 
    
    [![](images/new-storyboard-xs.png "Okno dialogowe nowego pliku")](images/new-storyboard-xs.png#lightbox)

2. Dodaj nazwę scenorysu do **interfejsu Main** sekcji **Info.plist**, jak pokazano poniżej:
    
    [![](images/infoplist.png "Edytor Info.plist")](images/infoplist.png#lightbox)
    
    To jest odpowiednikiem uruchamianiu początkowej kontrolera widoku w `FinishedLaunching` metodę delegata aplikacji. Z tą opcją aplikacja tworzy okno (patrz poniżej), ładuje głównego storyboard i przypisuje wystąpienie kontrolera widoku początkowej scenorysu (znajdujący się obok sourceless Segue) jako `RootViewController` właściwość okna, a następnie sprawia, że okna widocznego na ekranie.

3. W `AppDelegate`, zastąpić domyślną `Window` metody z następujący kod do implementacji okna właściwości:
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Utwórz nowy plik scenorysu, klikając prawym przyciskiem myszy projekt w celu **Dodaj > Nowy plik > iOS > pusty scenorysu**, jak pokazano poniżej: 
    
    [![](images/new-storyboard-vs.png "Okno dialogowe nowego elementu")](images/new-storyboard-vs.png#lightbox)

2. Dodaj nazwę scenorysu do **interfejsu Main** sekcji iOS aplikacji, jak pokazano poniżej:
    
    [![](images/ios-app.png "Edytor Info.plist")](images/ios-app.png#lightbox)
    
    To jest odpowiednikiem uruchamianiu początkowej kontrolera widoku w `FinishedLaunching` metodę delegata aplikacji. Z tą opcją aplikacja tworzy okno (patrz poniżej), ładuje głównego storyboard i przypisuje wystąpienie kontrolera widoku początkowej scenorysu (znajdujący się obok sourceless Segue) jako `RootViewController` właściwość okna, a następnie sprawia, że okna widocznego na ekranie.

3. W `AppDelegate`, zastąpić domyślną `Window` metody z następujący kod do implementacji okna właściwości:

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>Tworzenie scenorysu z projektanta dla systemu iOS

Scenorysu mogą być tworzone przy użyciu narzędzia Projektant Xamarin dla systemu iOS, które zostało zintegrowane bezproblemowo z programem Visual Studio for Mac i Visual Studio.

Aby rozpocząć tworzenie planów przy użyciu projektanta dla systemu iOS, wykonaj [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) przewodnik. W tym przewodniku może zapoznać nawigacji między kontrolerami widoku przy użyciu Segues i sposobu obsługi zdarzenia w Twoich kontrolek.

## <a name="instantiate-storyboards-manually"></a>Ręcznie Utwórz wystąpienie Scenorys

Scenorys całkowicie zastąpić poszczególnych plików XIB w projekcie, jednak nadal można wdrożyć widoku poszczególnych kontrolerów w scenorysu przy użyciu `Storyboard.InstantiateViewController`.

Czasami aplikacje mają szczególne wymagania, które nie mogą być obsługiwane z przejścia do scenorysu wbudowanych udostępniane przez projektanta. Na przykład jeśli możemy utworzyć aplikację, która uruchamia różnych ekranach z tego samego przycisku w zależności od bieżącego stanu aplikacji może chcemy ręcznie utworzyć wystąpienia widoku kontrolerów i nad program przejścia.

Na poniższym zrzucie ekranu przedstawiono dwa kontrolery widok na naszych powierzchni projektowej bez segue między nimi. Następna sekcja przeprowadzi jak tego przejścia można skonfigurować w kodzie.

 [![](images/viewcontrollerspink.png "Ten zrzut ekranu pokazuje, że dwa kontrolery widok na powierzchni projektu bez segue między nimi")](images/viewcontrollerspink.png#lightbox)

1. Dodaj _pusta iPhone scenorysu_ do istniejącego projektu projektu:
    
    [![](images/add-storyboard1.png "Dodawanie scenorysu")](images/add-storyboard1.png#lightbox)

2. Kliknij dwukrotnie nowo utworzony scenorysu, aby go otworzyć i Dodaj nową **kontrolera nawigacji** na powierzchnię projektu. Jak kontroler nawigacji jest interfejs użytkownika bez, domyślnie zostanie ona za pomocą kontrolera widoku głównego, jak przedstawiono poniżej:

    [![](images/uinavigationcontroller.png "Widok Segues kontrolerów z")](images/uinavigationcontroller.png#lightbox)

3. Wybierz _kontrolera widoku_ , klikając na pasku czarny u dołu. W Projektancie **konsoli właściwości**w obszarze **tożsamości** można określić klasy niestandardowej, a także unikatowy identyfikator dla kontrolera widoku. Ustaw **Nazwa klasy** i **identyfikator scenorysu** do `MainViewController`.

    [![](images/identitypanelnew.png "Określ klasę niestandardową")](images/identitypanelnew.png#lightbox)

4. Później, musimy wystąpienia naszych kontrolerów widoku scenorysu i użyje identyfikator scenorysu do odwołania się je w naszego kodu. Ustawienie tego Identyfikatora przywracania jest zgodny z Identyfikatorem scenorysu zapewnia, że kontroler widoku pobiera poprawnie odtworzyć w przypadku należy przywrócić stan.

5. Obecnie tylko mamy jeden kontroler widoku. Przeciągnij innego kontrolera widoku na powierzchnię projektu. W **konsoli właściwości**, w ramach tożsamości, ustaw klasy i Identyfikatora scenorysu `PinkViewController`, jak pokazano poniżej:

    [![](images/pinkvcnew.png "Konsola właściwości")](images/pinkvcnew.png#lightbox)
    
    IDE utworzy tych klas niestandardowych dla kontrolerów widoku. Mogą być wyświetlane w **konsoli rozwiązania**, jak pokazano na poniższym zrzucie ekranu:
    
    [![](images/solution-pad.png "Konsola rozwiązania")](images/solution-pad.png#lightbox)

6. W `PinkViewController`, wybierz widok, klikając kierunku Centrum ramki kontrolera. W konsoli właściwości widoku zmienić **tła** amarantowym:
    
    [![](images/pinkcontroller.png "Ustawianie koloru tła")](images/pinkcontroller.png#lightbox)

7. Na koniec, przeciągnij element button z **przybornika** na `MainViewController`. W konsoli właściwości nadaj mu nazwę `PinkButton` i GoToPink tytuł, jak przedstawiono poniżej:

    [![](images/pinkbutton.png "Nazwa przycisku zestawu")](images/pinkbutton.png#lightbox)

Scenorysu zostało ukończone, ale jeśli mamy teraz wdrożyć projekt, uzyskujemy będzie pusty ekran. Wynika to z nadal trzeba sprawdzić IDE, aby użyć naszych scenorysu i konfigurowania kontrolera widoku głównego celu służyć jako pierwszy widok. Zwykle można to zrobić za pomocą opcji projektu, jak pokazano powyżej. Jednak w tym przykładzie zostaną osiągnięte takiego samego wyniku w kodzie, dodając następujące polecenie, aby **AppDelegate**:

```csharp
public partial class AppDelegate : UIApplicationDelegate
    {
        UIWindow window;
        public static UIStoryboard Storyboard = UIStoryboard.FromName ("MainStoryboard", null);
        public static UIViewController initialViewController;

        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            window = new UIWindow (UIScreen.MainScreen.Bounds);

            initialViewController = Storyboard.InstantiateInitialViewController () as UIViewController;

            window.RootViewController = initialViewController;
            window.MakeKeyAndVisible ();
            return true;
        }

    }
```

Dużo kodu, ale tylko kilka wierszy jest nieznane. Najpierw, możemy zarejestrować naszych scenorysu z **AppDelegate** przez przekazywanie nazwę scenorysu, **MainStoryboard**. Następnie możemy powiedzieć wystąpienia kontrolera widoku początkowego z scenorysu, wywołując aplikacji `InstantiateInitialViewController` naszych scenorysu i ustawiliśmy na ten kontroler widoku jako kontroler widoku głównego naszej aplikacji. Ta metoda określa pierwszym ekranie widoczny dla użytkownika, i tworzy nowe wystąpienie tego kontrolera widoku.

Powiadomienie w okienku rozwiązania, który utworzył IDE `MainViewcontroller.cs` klasy i jego `corresponding designer.cs` po dodaliśmy nazwę klasy do konsoli do właściwości w kroku 4. Możemy stwierdzić, ta klasa utworzone specjalne konstruktora, który zawiera klasę podstawową:

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


Podczas tworzenia przy użyciu narzędzia Projektant scenorysu, automatycznie doda IDE [[zarejestrować]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) atrybutu w górnej części `designer.cs` klasy i podaj identyfikator ciągu, który jest taki sam jak identyfikator scenorysu określony w poprzedniego kroku. Będzie to łącze C# do odpowiednich scenę w scenorysu.

W pewnym momencie można dodać istniejącej klasy, która była **nie** utworzone w projektancie. W takim przypadku czy zarejestrować tę klasę jako normalne:

```csharp
[Register ("MainViewController")]
public partial class MainViewController : UIViewController
{
public MainViewController (IntPtr handle) : base (handle) 
{
}

...
}
```

Aby uzyskać więcej informacji o rejestrowaniu klasy i metody, zapoznaj się [rejestratora typu](http://docs.xamarin.com/guides/ios/advanced_topics/registrar/) dokumentacji.

Ostatnim krokiem tej klasy jest okablować przycisku i przejścia do widoku różowym kontrolera. Firma Microsoft będzie wystąpienia `PinkViewController` z scenorysu; następnie, firma Microsoft będzie program wypychanej segue z `PushViewController`, jak pokazano przykład kodu przedstawiony poniżej:

```csharp
public partial class MainViewController : UIViewController
{
    UIViewController pinkViewController;

    public MainViewController (IntPtr handle) : base (handle)
    {

    }

    public override void AwakeFromNib ()
    {
    // Called when loaded from xib or storyboard.

    this.Initialize ();
    }

    public void Initialize(){

    //Instantiating View Controller with Storyboard ID 'PinkViewController'
    pinkViewController = Storyboard.InstantiateViewController ("PinkViewController") as PinkViewController;
    }

    public override void ViewDidLoad ()
    {
    base.ViewDidLoad ();

    //When we push the button, we will push the pinkViewController onto our current Navigation Stack
    PinkButton.TouchUpInside += (o, e) =&gt; {
        this.NavigationController.PushViewController (pinkViewController, true);
    };
    }

}
```

Uruchamianie aplikacji daje aplikacji ekranu 2:

![](images/finishedstoryboard.png "Przykładowa aplikacja Uruchom ekranów")

## <a name="conditional-segues"></a>Segues warunkowe

Często przechodzenia z jednego kontrolera widoku do następnego jest zależne od określonego warunku. Na przykład, jeśli zostały czynione ekran logowania proste czy tylko chcemy Przenieś do następnego ekranu *Jeśli* nazwę użytkownika i hasło zostały potwierdzone.

W następnym przykładzie dodamy pole hasła do powyższego przykładu. Użytkownik będzie tylko dostęp do *PinkViewController* jeśli one Wprowadź poprawne hasło, w przeciwnym razie błąd zostanie wyświetlony.

Zanim zaczniemy, wykonaj kroki 1 – 8 powyżej. W tych krokach możemy utworzyć naszych scenorysu, zacząć tworzyć naszych interfejsu użytkownika i poinformuj naszych delegata aplikacji kontroler widoku jako jest RootViewController.

1. Teraz umożliwia tworzenie naszego interfejsu użytkownika i Dodaj dodatkowe widoki wymienione `MainViewController` aby było to wyglądać tak jak na poniższym zrzucie ekranu:

    - UITextField
        - Nazwa: PasswordTextField
        - Symbol zastępczy: "podać poufne hasło"
    - UILabel
        - Tekst: "Błąd: niewłaściwa hasła. Nie przekazuje! "
        - Kolor: czerwony
        - Wyrównanie: Center
        - Wiersze: 2
        - Zaznaczono element checkbox "Hidden" 
        
    [![](images/passwordvc.png "Centrum wierszy")](images/passwordvc.png#lightbox)
    
2. Utwórz Segue między przycisk Przejdź do różowym i kontroler widoku przez przeciągnięcie Ctrl z *PinkButton* do *PinkViewController*i wybierając **Push** na myszy w górę . 

3. Polecenie Segue i nadaj mu *identyfikator* `SegueToPink`:

    [![](images/namesegue.png "Kliknij Segue i nadaj mu identyfikator SegueToPink")](images/namesegue.png#lightbox)  
    

4. Na koniec Dodaj następującą metodę ShouldPerformSegue do `MainViewController` klasy:

    ```csharp
    public override bool ShouldPerformSegue (string segueIdentifier, NSObject sender)
    {
        
        if(segueIdentifier == "SegueToPink"){
            if (PasswordTextField.Text == "password") {
                PasswordTextField.ResignFirstResponder ();
                return true;
            }
            else{
                ErrorLabel.Hidden = false;
                return false;
            }
        }
        return base.ShouldPerformSegue (segueIdentifier, sender);
    }
    ```

W tym kodzie potwierdziliśmy ma segueIdentifier do naszej `SegueToPink` segue, więc można następnie sprawdzać, czy warunek; prawidłowe hasło w tym przypadku. Jeśli zwróci naszych warunku `true`, Segue wykona i wyświetli `PinkViewController`. Jeśli `false`, nie będą widzieć nowego kontrolera widoku.

Firma Microsoft można stosować do dowolnego Segue na tym kontrolerze widoku takie podejście, sprawdzając segueIdentifier argument do metody ShouldPerformSegue. W takim przypadku będziemy mieć tylko jeden identyfikator Segue — `SegueToPink`.

Odwołuje się do rozwiązania Storyboards.Conditional w [Scenorys ręcznego próbki](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/) na przykład pracy.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Za pomocą odwołań do scenorysu

Odwołanie do scenorysu pozwala pobrać projekt scenorysu dużych, złożonych i podziel go na mniejsze Scenorys, których uzyskać odwołania z oryginalnego, w związku z tym usuwanie złożoność oraz wprowadzania poszczególnych powstałe w ten sposób usuwania Scenorys łatwiejsze do projektowania i Obsługa.

Ponadto można podać odwołanie scenorysu _zakotwiczenia_ do innego sceny w ramach tej samej Storyboard lub określonych sceny na inny.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Odwołanie do zewnętrznej scenorysu

Aby dodać odwołanie do zewnętrznej scenorysu, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Dodaj** > **nowego pliku...**   >  **iOS** > **scenorysu**. Wprowadź **nazwa** nowego scenorysu i kliknij **nowy** przycisk:
    
    [![](images/ref01.png "Okno dialogowe nowego pliku")](images/ref01.png#lightbox)
    
2. Projektowanie układu sceny nowego scenorysu zazwyczaj będzie i Zapisz zmiany: 
    
    [![](images/ref02.png "Układ nowe sceny")](images/ref02.png#lightbox)
    
3. Otwórz scenorysu, który ma być Dodawanie odwołania do w systemie iOS projektanta.

4. Przeciągnij **scenorysu odwołanie** z **przybornika** na powierzchnię projektu: 
    
    [![](images/ref03.png "Odwołanie do scenorysu")](images/ref03.png#lightbox)
    
5. W **elementu Widget** karcie **Explorer właściwości**, wybierz nazwę **scenorysu** utworzoną wcześniej: 

    [![](images/ref04.png "Na karcie widżetu")](images/ref04.png#lightbox)
    
6. Formantu, kliknij pozycję element Widget interfejsu użytkownika (na przykład przycisk) na istniejących sceny i Utwórz nowe Segue do **odwołania scenorysu** nowo utworzony: 

    [![](images/ref05.png "Tworzenie segue")](images/ref05.png#lightbox) 
    
7. Wybierz z menu podręcznego **Pokaż** przeprowadzenie Segue: 

    [![](images/ref06.png "Wybieranie Pokaż przeprowadzenie Segue")](images/ref06.png#lightbox) 
    
8. Zapisz zmiany do scenorysu.

Gdy aplikacja jest uruchamiana, a użytkownik kliknie elementu interfejsu użytkownika utworzone Segue z początkowej kontrolera widoku z zewnętrznego scenorysu, określonych w odwołaniu scenorysu będą wyświetlane.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Odwołanie do określonego sceny w zewnętrznych scenorysu

Aby dodać odwołanie do określonego sceny scenorysu zewnętrznych (i nie początkowej View Controller) wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie zewnętrznych scenorysu, aby go otworzyć do edycji.

2. Dodaj nowe sceny i projektowanie układu w zwykły sposób: 

    [![](images/ref07.png "Nowy układ sceny")](images/ref07.png#lightbox)
    
3. W **elementu Widget** karcie **Explorer właściwości**, wprowadź **identyfikator scenorysu** sceny nowego kontrolera widoku: 

    [![](images/ref08.png "Podaj nazwę scenorysu na nowy kontroler widoku sceny")](images/ref08.png#lightbox)
    
3. Otwórz scenorysu, który ma być Dodawanie odwołania do w systemie iOS projektanta.

4. Przeciągnij **scenorysu odwołanie** z **przybornika** na powierzchnię projektu: 

    [![](images/ref03.png "Odwołanie do scenorysu")](images/ref03.png#lightbox)
    
5. W **elementu Widget** karcie **Explorer właściwości**, wybierz nazwę **scenorysu** i **Identyfikatora odwołania** (identyfikator scenorysu) z Sceny utworzoną wcześniej: 

    [![](images/ref09.png "Na karcie widżetu ")](images/ref09.png#lightbox)
    
6. Formantu, kliknij pozycję element Widget interfejsu użytkownika (na przykład przycisk) na istniejących sceny i Utwórz nowe Segue do **odwołania scenorysu** nowo utworzony: 

    [![](images/ref10.png "Tworzenie segue")](images/ref10.png#lightbox) 
    
7. Wybierz z menu podręcznego **Pokaż** przeprowadzenie Segue: 

    [![](images/ref06.png "Wybieranie Pokaż przeprowadzenie Segue")](images/ref06.png#lightbox) 
    
8. Zapisz zmiany do scenorysu.

Gdy aplikacja jest uruchamiania i użytkownik kliknie elementu interfejsu użytkownika, utworzony Segue przy użyciu sceny z danego **identyfikator scenorysu** z zewnętrznego scenorysu, określonych w odwołaniu scenorysu będzie wyświetlany.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Odwołanie do sceny określonych w tej samej scenorysu

Aby dodać odwołanie do określonego sceny tego samego scenorysu, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie scenorysu, aby go otworzyć do edycji.

2. Dodaj nowe sceny i projektowanie układu w zwykły sposób: 

    [![](images/ref11.png "Nowy układ sceny")](images/ref11.png#lightbox)

3. W **elementu Widget** karcie **Explorer właściwości**, wprowadź **identyfikator scenorysu** sceny nowego kontrolera widoku: 

    [![](images/ref12.png "Na karcie widżetu")](images/ref12.png#lightbox)
    
3. Przeciągnij **scenorysu odwołanie** z **przybornika** na powierzchnię projektu: 

    [![](images/ref03.png "Odwołanie do scenorysu")](images/ref03.png#lightbox)
    
5. W **elementu Widget** karcie **Explorer właściwości**, wybierz pozycję **Identyfikatora odwołania** (identyfikator scenorysu) sceny utworzoną wcześniej: 

    [![](images/ref13.png "Na karcie widżetu")](images/ref13.png#lightbox)
    
6. Formantu, kliknij pozycję element Widget interfejsu użytkownika (na przykład przycisk) na istniejących sceny i Utwórz nowe Segue do **odwołania scenorysu** nowo utworzony: 

    [![](images/ref14.png "Tworzenie segue")](images/ref14.png#lightbox) 
    
7. Wybierz z menu podręcznego **Pokaż** przeprowadzenie Segue: 

    [![](images/ref06.png "Wybieranie Pokaż przeprowadzenie Segue")](images/ref06.png#lightbox) 
    
8. Zapisz zmiany do scenorysu.

Gdy aplikacja jest uruchamiania i użytkownik kliknie elementu interfejsu użytkownika, utworzony Segue przy użyciu sceny z danym **identyfikator scenorysu** w tej samej scenorysu, określonych w odwołaniu scenorysu będzie wyświetlany.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono koncepcję Scenorys i jak mogą być przydatne podczas opracowywania aplikacji systemu iOS. Zawarto informacje sceny, widok kontrolerów, widoków i Widok hierarchii i jak sceny są połączone razem z różnych rodzajów Segues.  On również Eksploruje tworzenia wystąpienia widoku kontrolerów ręcznie z scenorysu i tworzenie Segues warunkowego.



## <a name="related-links"></a>Linki pokrewne

- [Ręczne scenorysu (przykład)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [Wprowadzenie do projektanta systemu iOS](~/ios/user-interface/designer/introduction.md)
- [Konwertowanie na Scenorys](http://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [Odwołania do klasy UIStoryboard](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
