---
title: Wprowadzenie do scenorysów w rozszerzeniu Xamarin.iOS
description: Ten dokument zawiera wprowadzenie do scenorysów w rozszerzeniu Xamarin.iOS. W tym artykule opisano, jak scenorys jest używany do definiowania interfejsu użytkownika, przejść oraz jak edytować pliki scenorysu za pomocą narzędzia iOS Designer.
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: bd8fee1b8f1941203bb0e6f00e261cbfbbccc9a7
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242345"
---
# <a name="introduction-to-storyboards-in-xamarinios"></a>Wprowadzenie do scenorysów w rozszerzeniu Xamarin.iOS

W tym przewodniku Wyjaśnijmy, jakie Scenorys jest i sprawdzać niektóre z kluczowych składników — takich jak przejścia. Zapoznamy się jak Scenorysy można tworzyć i używać, i jakie korzyści, które mają dla dewelopera.

Przed format pliku scenorysu została wprowadzona przez firmę Apple jako wizualnej reprezentacji interfejsu użytkownika aplikacji systemu iOS, deweloperzy utworzone pliki XIB dla każdego kontrolera widoku i programowana ręcznie nawigację między każdym widoku.  Za pomocą scenorysu umożliwia deweloperowi, zdefiniuj nawigację między je na powierzchni projektowej i kontrolerów widoku i oferuje edycję w trybie WYSIWYG interfejsu użytkownika aplikacji.

Scenorysu można tworzyć, otworzyć i edytować za pomocą platformy Xamarin iOS Designer. Ten przewodnik będzie również wskazówki sposób tworzenia własnych scenorysów podczas korzystania z języka C# program nawigacji za pomocą projektanta.


## <a name="requirements"></a>Wymagania

Scenorysy może służyć w narzędziu iOS Designer w programie Visual Studio dla komputerów Mac lub za pomocą programu Visual Studio 2015 i 2017 z zainstalowanych obciążeń platformy Xamarin.

## <a name="what-is-a-storyboard"></a>Co to jest scenorysu?

Scenorys jest wizualna reprezentacja wszystkich ekranów w aplikacji. Zawiera ona sekwencję sceny z każdego reprezentujący sceny *kontrolera widoku* i jego *widoków*. Widoki te mogą zawierać obiekty i [formantów](~/ios/user-interface/controls/index.md) która pozwala użytkownikowi interakcję z aplikacją. Ta kolekcja widoków i kontrolek (lub *widoków podrzędnych*) jest znany jako *zawartości Wyświetl hierarchię*. Są połączone w tle przez segue obiektów, które reprezentują przejścia między kontrolerami widoku. Zazwyczaj jest to osiągane przez utworzenie segue między obiektu na widok początkowy a widokiem nawiązującego połączenie. Relacje na powierzchni projektowej zostały przedstawione na poniższej ilustracji:

 [![](images/storyboardsview.png "Relacje na powierzchni projektowej zostały przedstawione na tej ilustracji")](images/storyboardsview.png#lightbox)

Jak widać, scenorysu będzie układ każdej usługi sceny z zawartością już renderowana i przedstawia połączenia między nimi.  Warto zauważyć, w tym momencie, gdy mówimy o sceny na telefonie iPhone, jest bezpiecznie przyjąć założenie, że jeden *sceny* w serii ujęć jest równa jeden *ekranu* zawartości na urządzeniu. Jednak z tabletu iPad, które można mieć wiele scen, które są wyświetlane tylko raz — na przykład za pomocą kontrolera widoku Popover.

Ma wiele zalet Tworzenie interfejsu użytkownika aplikacji, zwłaszcza w przypadku korzystania z platformy Xamarin przy użyciu scenorysów. Po pierwsze, jest wizualna reprezentacja interfejsu użytkownika, jak wszystkie obiekty — w tym [niestandardowe formanty](~/ios/user-interface/designer/ios-designable-controls-overview.md) — zostaną zrenderowane w czasie projektowania. Oznacza to, że przed przystąpieniem do tworzenia lub wdrażania aplikacji można wizualizować jego wygląd i przepływ. Na przykład potrwać powyższym obrazie. Możemy określić krótkie omówienie projektowania powierzchni sceny ile są, układ każdego widoku i jak wszystkie elementy powiązane. Jest to, co sprawia, że scenorysów polegających.

Zdarzenia są łatwiejsze w zarządzaniu za pomocą scenorysów, szczególnie przy użyciu narzędzia iOS Designer. Większości kontrolek interfejsu użytkownika będzie zawierać listę prawdopodobne zdarzenia w okienku właściwości. Program obsługi zdarzeń można dodanych w tym miejscu i ukończone w metody częściowej klasy kontrolerów widoku...

Zawartość scenorys jest przechowywany jako plik XML. W czasie, kompilacji wszelkie `.storyboard` pliki są kompilowane do plików binarnych, znane jako nibs. W czasie wykonywania te nibs są inicjowane i uruchomiony w celu tworzenia nowych widoków.

## <a name="segues"></a>Przejść

A *Segue*, lub *Segue obiektu*, jest używany w trakcie programowania dla systemu iOS do reprezentowania przejścia między sceny. Aby utworzyć segue, przytrzymaj wciśnięty **Ctrl** klucz i kliknij i przeciągnij od sceny jednego do drugiego. Przeciągania myszy naszych, pojawi się niebieskie łącznika wskazująca, gdzie będzie prowadzić segue, jak pokazano na poniższej ilustracji:

 [![](images/createsegue.png "Pojawi się niebieskie łącznika, wskazująca, gdzie będzie prowadzić segue, jak pokazano na tej ilustracji")](images/createsegue.png#lightbox)

Na górę myszy zostanie wyświetlone menu, umożliwiając nam wybrać akcję dla naszych segue. Może wyglądać podobnie do poniższej obrazów: 

**Klasy wstępnego dla systemu iOS 8 i rozmiar**:

[![](images/segue1.png "Lista rozwijana Segue akcji bez klasy rozmiaru")](images/segue1.png#lightbox)

**Podczas korzystania z klas rozmiaru i przejść adaptacyjne**:

[![](images/16new.png "Lista rozwijana Segue akcji z klas rozmiaru")](images/16new.png#lightbox)

> [!IMPORTANT]
> Jeśli używasz programu VMWare na maszynę wirtualną Windows, przytrzymując klawisz Ctrl jest zamapowany jako _kliknij prawym przyciskiem myszy_ przycisku myszy domyślnie. Aby utworzyć Segue, Edytuj swoje preferencje klawiatury, za pośrednictwem **preferencje** > **klawiatura i mysz** > **skróty myszy** i ponowne mapowanie usługi **Przycisk pomocniczy** tak jak przedstawiono poniżej:
> 
> [![](images/image22.png "Ustawienia preferencji klawiatury i myszy")](images/image22.png#lightbox)
> 
> Teraz można dodać segue między kontrolerami widoku normalnie.

Istnieją różne rodzaje każdego zabezpieczeniu kontrolę nad jak nowy kontroler widoku są prezentowane użytkownikowi i sposób jej interakcji z innymi kontrolerami widoku w scenorysu, przejścia. Poniżej opisano te. Jest również możliwe do podklasy obiektu segue do implementuje niestandardowe przejścia:

-  **Pokaż / wypychania** — powiadomienie wypychane segue dodaje kontroler widoku stos nawigacji. Założono, że kontroler widoku pochodzące wypychania jest częścią tego samego kontrolera nawigacji jako kontroler widoku, który jest dodawany do stosu. To działa tak samo jak `pushViewController` i są zazwyczaj stosowane, gdy ma niektórych zależności między danymi na ekranach. Za pomocą wypychania segue umożliwia luksusowe o pasek nawigacji przycisku Wstecz i tytuł dodany do każdego widoku w stosie, umożliwiając Przechodzenie do szczegółów nawigację Wyświetl hierarchię.
-  **Modalne** — segue modalny Utwórz relację między kontrolerów widoku dwóch w projekcie z opcją animowany przejścia są wyświetlane. Kontroler widoku podrzędne zostaną całkowicie zasłaniać kontrolera widoku elementu nadrzędnego, gdy przeznaczone do wyświetlenia. W przeciwieństwie do wypychania segue, która dodaje przycisk Wstecz dla nas; Po użyciu modalne segue `DismissViewController` należy użyć, aby można było powrócić do poprzedniego kontrolera widoku.
-  **Niestandardowe** — wszystkie niestandardowe segue mogą być tworzone jako podklasą ` UIStoryboardSegue`.
-  **Operacja unwind** — unwind segue może służyć do poruszanie się wstecz wypychania lub modalne segue — na przykład przez odrzucanie kontrolera widoku trybie modalnym przedstawione. Oprócz tego można odpoczynek, dzięki nie tylko jeden, ale szereg wypychania i modalne przejść i przejdź wstecz, że wiele kroków w hierarchii nawigacji za pomocą jednej operacji unwind akcji. Aby zrozumieć sposób użycia rozwinięcie segue w systemie iOS, przeczytaj [tworzenia Unwind przejść](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/unwind_segue) przepisu.
-  **Sourceless** — sourceless segue wskazuje sceny zawierającej kontrolera widok początkowy, jak i w związku z tym którego widoku użytkownik zostanie wyświetlony natychmiast. Jest reprezentowana przez segue pokazano poniżej:  

    [![](images/sourcelesssegue.png "Sourceless segue")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>Funkcje adaptacyjnego sterowania Segue typów

 System iOS 8 wprowadzono [klasy rozmiaru](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) umożliwia plik scenorysu systemu iOS współdziałały ze wszystkimi rozmiarami dostępne ekranu, dzięki czemu deweloperzy mogą tworzyć jednego interfejsu użytkownika dla wszystkich urządzeń z systemem iOS. Domyślnie wszystkie nowe aplikacje Xamarin.iOS użyje klas rozmiaru. Aby korzystać z klas rozmiaru z starsze projektu, zobacz [wprowadzenie do scenorysów Unified](~/ios/user-interface/storyboards/unified-storyboards.md) przewodnik. 
 
Aplikacje korzystające z klas rozmiaru będzie także korzystać z nowego [ *adaptacyjne przejść*](~/ios/user-interface/storyboards/unified-storyboards.md). Korzystając z klas rozmiaru, należy pamiętać, że firma Microsoft nie są bezpośrednio określać informuje używamy iPhone lub iPad. Innymi słowy tworzymy jeden interfejs użytkownika, który zawsze będzie wyglądać takie same, niezależnie od tego, ile nieruchomości, musi on pracować. Funkcje adaptacyjnego sterowania pracy przejścia przy ocenie środowiska i najlepszy sposób określania się do prezentowania zawartości. Funkcje adaptacyjnego sterowania przejść zostały wymienione poniżej: 

[![](images/adaptivesegue.png "Lista rozwijana adaptacyjne przejść")](images/adaptivesegue.png#lightbox)

|Segue|Opis|
|--- |--- |
|Pokaż|Jest to bardzo podobne do segue powiadomienie wypychane, ale uwzględnia zawartości ekranu.|
|Pokaż szczegóły|Jeśli aplikacja jest wyświetlany widok główny i szczegóły (na przykład w kontroler widoku podzielonego na urządzeniu iPad), zawartości spowoduje zastąpienie widok szczegółowy. Jeśli aplikacja jest wyświetlany tylko główny lub szczegółów, zawartości spowoduje zastąpienie Szczyt stosu kontrolera widoku.|
|Prezentacja|Jest podobny do modalne segue i pozwala na wybór style prezentacji i przejścia.|
|Prezentacja popover|Przedstawia zawartość jako popover|

### <a name="transferring-data-with-segues"></a>Transferowanie danych za pomocą przejść

Zalety segue nie kończy się przejścia. One może również do zarządzania przesyłaniem danych między kontrolerami widoku. Jest to osiągane przez zastąpienie `PrepareForSegue` metody na kontrolerze widok początkowy i obsługi danych, określić główną przyczynę. Po wyzwoleniu segue — na przykład o naciśnięcie przycisku — aplikacja będzie wywołać tej metody, zapewniając możliwość przygotuj nowy kontroler widoku *przed* wszelkie nawigacji. Poniższego kodu, z [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) przykładowy, potwierdza to: 


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

W tym przykładzie `PrepareForSegue` metoda zostanie wywołana, gdy segue zostanie wywołany przez użytkownika. Musimy najpierw utworzyć wystąpienie "odbieranie" kontroler widoku i Ustaw jako docelowy segue kontroler widoku. Jest to realizowane przez wiersz poniższym kodem:

```csharp
var callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

Metoda ma teraz możliwość ustawienia właściwości `DestinationViewController`. W tym przykładzie iż podejmujemy dzięki temu, przekazując listę o nazwie `PhoneNumbers` do `CallHistoryController` i przypisywanie jej do obiektu o takiej samej nazwie:

```csharp
if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
```

Po zakończeniu przejścia, użytkownik będzie widział `CallHistoryController` z listą wypełnione.

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>Dodawanie scenorysu do projektu nie Scenorys

Czasami może być konieczne dodanie scenorysu do pliku wcześniej niż scenorysu. Jeden raz w ten sposób w programie Visual Studio dla komputerów Mac można uproszczenie przez następujące kroki:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Utwórz nowy plik scenorysu, przechodząc do **Plik > Nowy plik > iOS > scenorysu**, jak pokazano poniżej: 
    
    [![](images/new-storyboard-xs.png "Okno dialogowe Nowy plik")](images/new-storyboard-xs.png#lightbox)

2. Dodaj nazwę scenorysu do **interfejsu Main** części **Info.plist**, jak pokazano poniżej:
    
    [![](images/infoplist.png "W edytorze pliku Info.plist")](images/infoplist.png#lightbox)
    
    Jest to wielokrotność wystąpienia początkowej kontrolera widoku w `FinishedLaunching` metodę delegata aplikacji. Z ustawioną tą opcją aplikacja tworzy okno (patrz poniżej), ładuje głównego storyboard i przypisuje wystąpienie kontroler widoku scenorysu — początkowa (znajdujący się obok sourceless Segue) jako `RootViewController` właściwości okna, a następnie sprawia, że Okno widoczne na ekranie.

3. W `AppDelegate`, zastąpić to ustawienie domyślne `Window` metody, przy użyciu następującego kodu, aby zaimplementować właściwości okna:
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Utwórz nowy plik scenorysu, klikając prawym przyciskiem myszy projekt, aby **Dodaj > Nowy plik > iOS > pusty Scenorys**, jak pokazano poniżej: 
    
    [![](images/new-storyboard-vs.png "Okno dialogowe nowego elementu")](images/new-storyboard-vs.png#lightbox)

2. Dodaj nazwę scenorysu do **interfejsu Main** części systemu iOS aplikacji, jak pokazano poniżej:
    
    [![](images/ios-app.png "W edytorze pliku Info.plist")](images/ios-app.png#lightbox)
    
    Jest to wielokrotność wystąpienia początkowej kontrolera widoku w `FinishedLaunching` metodę delegata aplikacji. Z ustawioną tą opcją aplikacja tworzy okno (patrz poniżej), ładuje głównego storyboard i przypisuje wystąpienie kontroler widoku scenorysu — początkowa (znajdujący się obok sourceless Segue) jako `RootViewController` właściwości okna, a następnie sprawia, że Okno widoczne na ekranie.

3. W `AppDelegate`, zastąpić to ustawienie domyślne `Window` metody, przy użyciu następującego kodu, aby zaimplementować właściwości okna:

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>Scenorys utworzony za pomocą narzędzia iOS Designer

Scenorys mogą być tworzone za pomocą projektanta platformy Xamarin dla systemów iOS, które zostało zintegrowane bezproblemowo z programem Visual Studio dla komputerów Mac i Visual Studio.

Aby rozpocząć pracę, przy użyciu narzędzia iOS Designer do tworzenia scenorysów, postępuj zgodnie z [Witaj, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) przewodnik. W ramach tego przewodnika przedstawimy nawigacji między kontrolerami widoku przy użyciu przejścia i sposób obsługi zdarzenia w Twoich kontrolek.

## <a name="instantiate-storyboards-manually"></a>Ręcznie Utwórz wystąpienie scenorysów

Scenorysy całkowicie Zastąp indywidualne plików XIB w projekcie, jednak kontrolerów widoku poszczególnych w scenorysu można nadal tworzone przy użyciu `Storyboard.InstantiateViewController`.

Czasami aplikacje mają specjalne wymagania, które nie mogą być obsługiwane za pomocą przejścia wbudowanych scenorysu, dostarczone przez projektanta. Na przykład jeśli możemy utworzyć aplikację, która uruchamia różnych ekranów przy użyciu tego samego przycisku, w zależności od bieżącego stanu aplikacji, może chcemy ręcznie utworzyć wystąpienie kontrolerów widoku, a program przejścia, określić główną przyczynę.

Poniższy zrzut ekranu pokazuje, że dwa kontrolery widoku na powierzchni projektowej, nasze bez segue między nimi. Następna sekcja przeprowadzi jak tego przejścia należy skonfigurować w kodzie.

 [![](images/viewcontrollerspink.png "Ten zrzut ekranu pokazuje, że dwa kontrolery widoku na powierzchni projektowej bez segue między nimi")](images/viewcontrollerspink.png#lightbox)

1. Dodaj _pusty Scenorys dla telefonu iPhone_ do istniejącego projektu projektu:
    
    [![](images/add-storyboard1.png "Dodawanie scenorysu")](images/add-storyboard1.png#lightbox)

2. Kliknij dwukrotnie nowo utworzony scenorysu, aby otworzyć go i Dodaj nowy **kontrolera nawigacji** do powierzchni projektowej. Ponieważ kontrolera nawigacji bez interfejsu użytkownika, domyślnie będą pochodzić z kontrolera widoku głównego, jak przedstawiono poniżej:

    [![](images/uinavigationcontroller.png "Kontrolery widoku przejść")](images/uinavigationcontroller.png#lightbox)

3. Wybierz _kontrolera widoku_ , klikając przycisk na pasku czarny u dołu. W oknie Projektant **Konsola właściwość**w obszarze **tożsamości** można określić klasę niestandardową, a także unikatowy identyfikator dla kontrolera widoku. Ustaw **Nazwa klasy** i **identyfikator scenorysu** do `MainViewController`.

    [![](images/identitypanelnew.png "Określ klasę niestandardową")](images/identitypanelnew.png#lightbox)

4. Później, konieczne będzie wystąpienia naszej kontrolerów widoku scenorysu i użyje identyfikator scenorysu, aby odwoływać się do nich w naszym kodzie. Ustawienie Identyfikatora przywracania, aby jest zgodny z Identyfikatorem scenorysu zapewnia, że kontroler widoku pobiera poprawnie utworzenia, jeśli stan musi zostać przywrócone.

5. Obecnie mamy jeden kontroler widoku. Przeciągnij inny kontroler widoku na powierzchni projektowej. W **Konsola właściwość**, w obszarze tożsamość, ustaw klasy i identyfikator scenorysu `PinkViewController`, jak pokazano poniżej:

    [![](images/pinkvcnew.png "Blok właściwości")](images/pinkvcnew.png#lightbox)
    
    Klasy niestandardowe dla kontrolerów widoku spowoduje to utworzenie środowiska IDE. Mogą być wyświetlane w **konsoli rozwiązania**, jak pokazano na poniższym zrzucie ekranu:
    
    [![](images/solution-pad.png "Konsola rozwiązania")](images/solution-pad.png#lightbox)

6. W `PinkViewController`, wybierz widok, klikając w kierunku środka ramki kontrolera. W okienku właściwości w widoku zmienić **tła** do amarantowy:
    
    [![](images/pinkcontroller.png "Ustaw kolor tła")](images/pinkcontroller.png#lightbox)

7. Na koniec, przeciągnij przycisk od **przybornika** na `MainViewController`. W okienku właściwości, nadaj jej nazwę `PinkButton` i GoToPink tytuł, jak przedstawiono poniżej:

    [![](images/pinkbutton.png "Ustaw nazwę przycisku")](images/pinkbutton.png#lightbox)

Scenorys zostało ukończone, ale jeśli teraz wdrożymy projekt, wystąpi wspomniany pusty ekran. To, ponieważ nadal należy przekazać do IDE, aby użyć naszego scenorysu i skonfigurować kontroler widoku głównego, która będzie służyć jako pierwszy widok. Zazwyczaj można to zrobić za pomocą opcji projektu, jak pokazano powyżej. Jednak w tym przykładzie zostanie osiągnąć ten sam wynik w kodzie, dodając następujące polecenie, aby **elemencie AppDelegate**:

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

To duża ilość kodu, ale tylko kilka wierszy jest nieznane. Po pierwsze, zarejestrujemy nasz scenorysu z **elemencie AppDelegate** , przekazując nazwę scenorysu **MainStoryboard**. Następnie o aplikacji do tworzenia wystąpienia kontrolera widok początkowy, od scenorysu, wywołując `InstantiateInitialViewController` na naszych scenorysu i możemy ustawić ten kontroler widoku jako kontroler widoku głównego naszej aplikacji. Ta metoda określa pierwszy ekran widoczny dla użytkownika, i tworzy nowe wystąpienie klasy kontrolera widoku.

Zwróć uwagę, w okienku rozwiązania utworzonego w środowisku IDE `MainViewcontroller.cs` klasy i jego `corresponding designer.cs` po dodaliśmy nazwę klasy do konsoli do właściwości w kroku 4. Widać, ta klasa utworzone specjalnych konstruktora, który zawiera klasę bazową:

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


Podczas tworzenia scenorysu, za pomocą projektanta, IDE automatycznie doda [[zarejestrować]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) atrybutu w górnej części `designer.cs` klasy, a następnie przekaż identyfikator ciągu, która jest taka sama jak identyfikator scenorysu, określony w poprzedniego kroku. To łączenie języka C# z odpowiednimi sceny w scenorysu.

W pewnym momencie możesz chcieć Dodaj istniejącą klasę, która była **nie** utworzone w projektancie. W tym przypadku będzie zarejestrować tej klasy jako normalny:

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

Aby uzyskać więcej informacji na temat rejestrowania, klasy i metody, zobacz [Typ rejestratora](http://docs.xamarin.com/guides/ios/advanced_topics/registrar/) dokumentacji.

Ostatnim krokiem w tej klasie jest Podłączanie przycisku i przejście do kontrolera widoku różowy. Firma Microsoft będzie wystąpienia `PinkViewController` z scenorysu; firma Microsoft będzie Zaprogramuj powiadomienie wypychane segue z `PushViewController`, co ilustruje poniższy kod przykładowy:

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

Uruchomiona jest aplikacja generuje aplikacji ekranu 2:

![](images/finishedstoryboard.png "Przykładowa aplikacja ekrany uruchamiania")

## <a name="conditional-segues"></a>Warunkowe przejść

Często przechodzenia z jednego kontrolera widoku do następnego są zależne od określonego warunku. Na przykład, jeśli wysyłamy ekran logowania proste czy ma być uruchamiany tylko przenieść na następnym ekranie *Jeśli* zostały potwierdzone nazwy użytkownika i hasła.

W następnym przykładzie dodamy pole hasła do powyższego przykładu. Użytkownik będzie tylko można uzyskać dostęp do *PinkViewController* jeśli one Wprowadź poprawne hasło, w przeciwnym razie błąd zostanie wyświetlony.

Przed rozpoczęciem wykonaj kroki 1 – 8 powyżej. W tych krokach możemy utworzyć naszych scenorysu, rozpocząć tworzenie naszego interfejsu użytkownika i przekaż delegata naszej aplikacji kontroler widoku się RootViewController.

1. Teraz sklonujemy zbudowania naszego interfejsu użytkownika i dodać dodatkowe widoki na liście do `MainViewController` się to wyglądać tak jak na poniższym zrzucie ekranu:

    - UITextField
        - Nazwa: PasswordTextField
        - Symbol zastępczy: "Wprowadź klucz tajny"
    - UILabel
        - Tekst: "Błąd: niewłaściwa hasła. Nie przekazuje! "
        - Kolor: czerwony
        - Wyrównanie: Centrum
        - Wiersze: 2
        - "Hidden" pole wyboru zaznaczone 
        
    [![](images/passwordvc.png "Centrum wierszy")](images/passwordvc.png#lightbox)
    
2. Utwórz Segue między przycisk Przejdź do różowy i kontroler widoku, przeciągając Ctrl z *PinkButton* do *PinkViewController*i wybierając polecenie **wypychania** na myszy w górę . 

3. Kliknij pozycję Segue i nadaj mu *identyfikator* `SegueToPink`:

    [![](images/namesegue.png "Kliknij Segue i nadaj mu SegueToPink identyfikator")](images/namesegue.png#lightbox)  
    

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

W tym kodzie możemy być dopasowane segueIdentifier do naszych `SegueToPink` segue, dzięki czemu możemy następnie sprawdzić warunku; prawidłowe hasło w tym przypadku. Jeśli nasze warunek zwraca `true`, Segue wykona i będzie przedstawiać `PinkViewController`. Jeśli `false`, nie będą widzieć nowy kontroler widoku.

Możemy zastosować takie podejście do dowolnego Segue na tym kontrolerze widoku, sprawdzając segueIdentifier argument do metody ShouldPerformSegue. W tym przypadku mamy jeden identyfikator Segue — `SegueToPink`.

Zapoznaj się z rozwiązaniem Storyboards.Conditional w [przykładowe scenorysów ręczne](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/) na przykład roboczy.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Przy użyciu odwołań scenorysu

Odwołanie do scenorysu pozwala pobrać projekt scenorysu dużymi, złożonymi i podzielenie go na mniejsze Scenorysy, które uzyskać odwołanie z oryginalnym, ten sposób usuwania usuwając złożoność i dzięki czemu wynikowy poszczególnych Scenorysy łatwiejsze do projektu i Obsługa.

Ponadto może zapewnić odwołanie scenorysu _zakotwiczenia_ do innego sceny w obrębie tego samego scenorysu lub scenę określonych na inny.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Odwołanie do zewnętrznej scenorysu

Aby dodać odwołanie do zewnętrznej scenorysu, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Dodaj** > **nowy plik...**   >  **iOS** > **scenorysu**. Wprowadź **nazwa** nowy Scenorys, a następnie kliknij przycisk **New** przycisku:
    
    [![](images/ref01.png "Okno dialogowe Nowy plik")](images/ref01.png#lightbox)
    
2. Projektowanie układu sceny nowy Scenorys, jak zwykle będzie i Zapisz zmiany: 
    
    [![](images/ref02.png "Układ nową scenę")](images/ref02.png#lightbox)
    
3. Otwórz Scenorys, który ma być dodawane odwołanie do w narzędziu iOS Designer.

4. Przeciągnij **scenorysu odwołanie** z **przybornika** na powierzchnię projektu: 
    
    [![](images/ref03.png "Odwołanie do scenorysu")](images/ref03.png#lightbox)
    
5. W **widżet** karcie **Eksplorator właściwości**, wybierz nazwę **scenorysu** utworzoną wcześniej: 

    [![](images/ref04.png "Na karcie widżetu")](images/ref04.png#lightbox)
    
6. Kombinacji Control + kliknięcie na element Widget interfejsu użytkownika (na przykład przycisk) na scenę istniejących i tworzenie nowych Segue do **odwołania scenorysu** właśnie utworzony: 

    [![](images/ref05.png "Tworzenie segue")](images/ref05.png#lightbox) 
    
7. Wybierz z menu podręcznego **Pokaż** do ukończenia Segue: 

    [![](images/ref06.png "Wybierając opcję Pokaż do ukończenia Segue")](images/ref06.png#lightbox) 
    
8. Zapisz zmiany do scenorysu.

Pojawi się po uruchomieniu aplikacji, a użytkownik kliknie element interfejsu użytkownika tworzone Segue od początkowego kontrolera widoku z zewnętrznego scenorysu, określone w odwołaniu do scenorysu.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Odwoływanie się do określonych sceny w zewnętrznych scenorysu

Aby dodać odwołanie do określonego sceny zewnętrznych scenorysu (a nie początkowego kontrolera widoku) wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie zewnętrznych scenorysu, aby go otworzyć do edycji.

2. Dodaj nową scenę i zaprojektować jej układ, tak jak zwykle: 

    [![](images/ref07.png "Nowy układ sceny")](images/ref07.png#lightbox)
    
3. W **widżet** karcie **Eksplorator właściwości**, wprowadź **identyfikator scenorysu** do sceny nowy kontroler widoku: 

    [![](images/ref08.png "Wprowadź identyfikator scenorysu dla nowego kontrolera widoku sceny")](images/ref08.png#lightbox)
    
3. Otwórz Scenorys, który ma być dodawane odwołanie do w narzędziu iOS Designer.

4. Przeciągnij **scenorysu odwołanie** z **przybornika** na powierzchnię projektu: 

    [![](images/ref03.png "Odwołanie do scenorysu")](images/ref03.png#lightbox)
    
5. W **widżet** karcie **Eksplorator właściwości**, wybierz nazwę **scenorysu** i **Identyfikator odwołania** (identyfikator: scenorysu) z Scen, które zostały utworzone powyżej: 

    [![](images/ref09.png "Na karcie widżetu ")](images/ref09.png#lightbox)
    
6. Kombinacji Control + kliknięcie na element Widget interfejsu użytkownika (na przykład przycisk) na scenę istniejących i tworzenie nowych Segue do **odwołania scenorysu** właśnie utworzony: 

    [![](images/ref10.png "Tworzenie segue")](images/ref10.png#lightbox) 
    
7. Wybierz z menu podręcznego **Pokaż** do ukończenia Segue: 

    [![](images/ref06.png "Wybierając opcję Pokaż do ukończenia Segue")](images/ref06.png#lightbox) 
    
8. Zapisz zmiany do scenorysu.

Gdy aplikacja jest uruchamiania i użytkownik kliknie element interfejsu użytkownika, utworzony Segue z sceny z danego **identyfikator scenorysu** z zewnętrznego scenorysu, określone w odwołaniu do scenorysu będzie wyświetlany.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Odwoływanie się do określonych sceny, w tym samym scenorysu

Aby dodać odwołanie do określonego sceny tego samego scenorysu, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie scenorysu, aby go otworzyć do edycji.

2. Dodaj nową scenę i zaprojektować jej układ, tak jak zwykle: 

    [![](images/ref11.png "Nowy układ sceny")](images/ref11.png#lightbox)

3. W **widżet** karcie **Eksplorator właściwości**, wprowadź **identyfikator scenorysu** do sceny nowy kontroler widoku: 

    [![](images/ref12.png "Na karcie widżetu")](images/ref12.png#lightbox)
    
3. Przeciągnij **scenorysu odwołanie** z **przybornika** na powierzchnię projektu: 

    [![](images/ref03.png "Odwołanie do scenorysu")](images/ref03.png#lightbox)
    
5. W **widżet** karcie **Eksplorator właściwości**, wybierz opcję **Identyfikator odwołania** (identyfikator: scenorysu) scen, które zostały utworzone powyżej: 

    [![](images/ref13.png "Na karcie widżetu")](images/ref13.png#lightbox)
    
6. Kombinacji Control + kliknięcie na element Widget interfejsu użytkownika (na przykład przycisk) na scenę istniejących i tworzenie nowych Segue do **odwołania scenorysu** właśnie utworzony: 

    [![](images/ref14.png "Tworzenie segue")](images/ref14.png#lightbox) 
    
7. Wybierz z menu podręcznego **Pokaż** do ukończenia Segue: 

    [![](images/ref06.png "Wybierając opcję Pokaż do ukończenia Segue")](images/ref06.png#lightbox) 
    
8. Zapisz zmiany do scenorysu.

Gdy aplikacja jest uruchamiania i użytkownik kliknie element interfejsu użytkownika, utworzony Segue z sceny z danego **identyfikator scenorysu** w tym samym scenorysu, określone w odwołaniu do scenorysu będzie wyświetlany.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono koncepcję Scenorysy i jak mogą być przydatne podczas opracowywania aplikacji dla systemu iOS. Omówiono w nim scen, kontrolerów widoku, widoki i hierarchie z widoku i jak sceny są połączone razem z różnych rodzajów przejścia.  On również odkrywa kontrolerów widoku wystąpienia ręcznie scenorysu i tworzenia warunkowego przejścia.



## <a name="related-links"></a>Linki pokrewne

- [Ręczne scenorysu (przykład)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [Wprowadzenie do narzędzia iOS Designer](~/ios/user-interface/designer/introduction.md)
- [Konwertowanie do scenorysów](http://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [Informacje o klasach UIStoryboard](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
