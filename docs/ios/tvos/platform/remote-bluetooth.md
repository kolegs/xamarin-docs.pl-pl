---
title: "Używanie programu Siri zdalnego i kontrolerów Bluetooth"
description: "W tym artykule omówiono pomocniczych w aplikacjach Xamarin.tvOS nowe kontrolery gier Siri Remote i Bluetooth."
ms.topic: article
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: ca3dd71c3da316e467d8c388efbbded3d9778bf0
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="siri-remote-and-bluetooth-controllers"></a>Używanie programu Siri zdalnego i kontrolerów Bluetooth

_W tym artykule omówiono pomocniczych w aplikacjach Xamarin.tvOS nowe kontrolery gier Siri Remote i Bluetooth._


Użytkownicy aplikacji Xamarin.tvOS nie będzie interakcja jego interfejsu bezpośrednio z systemem iOS gdzie one wybierz obrazów na ekranie urządzenia, ale pośrednio z między przy użyciu miejsca [zdalnego Siri](#The-Siri-Remote).

Jeśli aplikacja jest gier, można opcjonalnie utworzyć wsparcie dla strony 3, wprowadzone dla systemu iOS (MIF) [kontrolery gier Bluetooth](#Bluetooth-Game-Controllers) również Twojej aplikacji.

[![](remote-bluetooth-images/intro01.png "Bluetooth zdalnego i kontroler gier")](remote-bluetooth-images/intro01.png#lightbox)

W tym artykule opisano [zdalnego Siri](#The-Siri-Remote), [dotykać powierzchni gestów](#Touch-Surface-Gestures) i [przycisków zdalnego Siri](#Siri-Remote-Buttons) i pokazuje, jak pracować z nimi za pośrednictwem [gestów i Scenorys](#Gestures-and-Storyboards), [gestów i kod](#Gestures-and-Code) i [obsługi zdarzeń niskiego poziomu](#Low-Level-Event-Handling). Ponadto omówiono [Praca z kontrolery gier](#Working-with-Game-Controllers) w aplikacji Xamarin.tvOS.

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Używanie programu Siri zdalnego

Sposób głównego, który użytkownicy będą interakcji z Apple TV, a aplikacja Xamarin.tvOS jest dołączony zdalnego Siri. Apple zaprojektowane zdalnego do zestawiania odległość między użytkownikiem z kanapie i interfejs użytkownika telewizora firmy Apple, wyświetlane przez miejsca na ekranie TV.

Twoje żądanie jako Deweloper aplikacji systemu tvOS jest tworzenie interfejsu użytkownika szybkie, łatwe w użyciu i wizualny, który korzysta z powierzchni touch zdalnego Siri, przyspieszeniomierza Żyroskop i przycisków.

[![](remote-bluetooth-images/remote01.png "Używanie programu Siri zdalnego")](remote-bluetooth-images/remote01.png#lightbox)

Używanie programu Siri zdalnego ma następujące funkcje i oczekiwanego użycia w aplikacji systemu tvOS:

|Funkcja|Użycie aplikacji ogólne|Użycie aplikacji gry|
|---|---|---|
|**Dotykać powierzchni**<br />Przejdź do Przejdź, naciśnij klawisz, aby wybrać i przytrzymaj ją menu kontekstowe.|**Naciśnij/przejdź**<br />Interfejs użytkownika nawigacji między elementami focusable.<br /><br />**Kliknij przycisk**<br />Aktywuje wybranego elementu (fokusu).|**Naciśnij/przejdź**<br />Zależy od gier projektu i mogą być używane jako D-Pad, naciskając pozycję na krawędzi.<br /><br />**Kliknij przycisk**<br />Wykonanie czynności przycisk podstawowy.|
|**Menu**<br />Naciśnij, aby powrócić do poprzedniego ekranu lub menu.|Zwraca do poprzedniego ekranu i kończy działanie do firmy Apple TV Home ekranu z ekranu głównego aplikacji.|Wstrzymywanie i wznawianie gry, zwraca do poprzedniego ekranu i wyjścia do firmy Apple TV Home ekranu z ekranu głównego aplikacji.|
|**Siri/Search**<br />W krajach z Siri naciśnij i przytrzymaj ją głosu formantu, we wszystkich innych krajach, wyświetla ekran wyszukiwania.|n/d|n/d|
|**Odtwórz/Wstrzymaj**<br />Odtwarzanie i wstrzymać media lub dodatkowej funkcję w aplikacjach.|Rozpoczyna odtwarzanie multimediów i wstrzymanie/wznowienie odtwarzania.|Wykonuje funkcję przycisku dodatkowej lub pomija wprowadzenie wideo (jeśli istnieje).|
|**Strona główna**<br />Naciśnij, aby powrócić do ekranu główną, kliknij dwukrotnie, aby wyświetlić uruchomionych aplikacji, naciśnij i przytrzymaj ją w stan uśpienia urządzenia.|n/d|n/d|
|**Wolumin**<br />Formanty dołączony woluminu urządzenia audio i wideo.|n/d|n/d|

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>Powierzchni gestów dotykowych

Zdalne Siri dotykać powierzchni jest w stanie wykryć różnych palca jednym gestów można uwzględniać w aplikacji Xamarin.tvOS:

|Przejdź|Kliknij|Wybierz opcję|
|---|---|---|
|![](remote-bluetooth-images/Gesture01.png)|![](remote-bluetooth-images/Gesture02.png)|![](remote-bluetooth-images/Gesture03.png)|
|Przenosi zaznaczenie (fokus) między elementy interfejsu użytkownika na ekranie (górę, dół po lewej stronie kliknij prawym przyciskiem myszy). Szybko przesuwając może służyć do przewijania dużych list zawartości szybko przy użyciu bezwładności.|Aktywuje wybranego elementu (fokusu) lub zachowuje się jak przycisk podstawowy w grę. Kliknięcie i przytrzymanie aktywować menu kontekstowe lub dodatkowej funkcji.|Naciskając lekkim dotykać powierzchni na krawędziach działa jak kierunkową przycisków w D-konsola, przeniesienie fokusu w górę, dół, lewo lub w prawo w zależności od obszaru dotknięciu. W zależności od aplikacji można wyświetlić ukryte kontrolki.|

Apple zawiera poniższe sugestie dotyczące pracy za pomocą gestów dotykać powierzchni:

* **Rozróżnianie między kliknięciami a podsłuchu** — jest zamierzone akcji przez użytkownika i jest odpowiednia dla zaznaczenia, aktywacji i przycisk podstawowy gry. Naciśnięcie przycisku jest aspekty i powinny być używane rzadko, ponieważ często wstrzymał zdalnego Siri na ich ręcznie i mogą przypadkowo aktywować zdarzeń naciśnij łatwo użytkownika.
* **Ponownie zdefiniować standardowe gestów nie** — użytkownik ma oczekiwanie czy gestów określonych będzie wykonywać określone czynności, nie należy zmienić znaczenie lub funkcji tych gestów, które znajdują się w aplikacji. Jedynym wyjątkiem jest aplikacji gry podczas gry aktywne.
* **Definiowanie nowych gestów oszczędnie** -ponownie, użytkownik ma oczekuje, że określonych gestów będzie wykonywać określone czynności. Należy unikać Definiowanie gestów niestandardowych do wykonywania standardowych czynności. I ponownie, gry są zwykle wyjątku, której gestów niestandardowych można dodać play przyjemne, bez ramek do gry.
* **W razie potrzeby, odpowiadanie na konsoli D naciska** — lekkim naciskając rogu krawędzie powierzchni Touch będzie reagować, takich jak konsola D na przenoszenie gier kontroler lub kierunek, w dół, lewo lub w prawo. W razie potrzeby należy odpowiedzieć tych gestów w aplikacji lub w grę.

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Używanie programu Siri zdalnego przycisków

Oprócz gestów na powierzchni Touch aplikacja może odpowiadać na użytkownika, klikając dotykać powierzchni lub naciśnięcie przycisku odtwarzania i wstrzymania. Jeśli uzyskują dostęp do zdalnego Siri przy użyciu platformy kontrolera gier, można wykryć naciśnięcie przycisku Menu. 

Ponadto naciśnięcie przycisku menu mogą zostać wykryte przy użyciu aparat rozpoznawania gestów ze standardowym `UIKit` elementów. Jeśli przechwycić naciśnięcie przycisku Menu, będzie odpowiedzialny za zamknięcie bieżącego widoku oraz widoku kontrolera i wróć do poprzedniego.

> [!IMPORTANT]
> Należy **zawsze** przydzielenie funkcji przycisk Odtwórz/Wstrzymaj na komputerze zdalnym. Posiadające przycisk współzależności funkcjonalnych można zapewnić aplikacji Szukaj przerwane dla użytkownika końcowego. Jeśli nie jest prawidłową funkcją dla tego przycisku, przypisz taką samą funkcję jak przycisk podstawowy (dotykać powierzchni kliknij).




<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>Gesty i planów

Najprostszym sposobem pracy za pomocą zdalnego Siri w aplikacji Xamarin.tvOS jest dodać aparaty rozpoznawania gestów widoków w Projektancie interfejsu.

Aby dodać aparat rozpoznawania gestów, wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` plik i otworzyć do edycji w Projektancie interfejsu.
2. Przeciągnij **wybierz aparat rozpoznawania gestów** z **biblioteki** i upuść go w widoku: 

    [![](remote-bluetooth-images/storyboard01.png "Aparat rozpoznawania gestów Tap")](remote-bluetooth-images/storyboard01.png#lightbox)
3. Sprawdź **wybierz** w **przycisk** sekcji **inspektora atrybutu**: 

    [![](remote-bluetooth-images/storyboard02.png "Sprawdź wybierz")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **Wybierz** oznacza gestu będzie odpowiadać na kliknięcie użytkownika **dotykać powierzchni** na komputerze zdalnym Siri. Istnieje również możliwość reagowania na **Menu**, **Odtwórz/Wstrzymaj**, **się**, **dół**, **lewej** i **Prawej** przycisków.
5. Następnie okablować się **akcji** z **wybierz aparat rozpoznawania gestów** i nadaj mu `TouchSurfaceClicked`: 

    [![](remote-bluetooth-images/storyboard03.png "Aparat rozpoznawania gestów naciśnij akcji")](remote-bluetooth-images/storyboard03.png#lightbox)
6. Zapisz zmiany i wróć do programu Visual Studio dla komputerów Mac.

Edytuj kontroler widoku (przykład `FirstViewController.cs`) i Dodaj następujący kod, aby obsłużyć gestu inicjowane:

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class FirstViewController : UIViewController
    {
        ...

        #region Custom Actions
        partial void TouchSurfaceClicked (Foundation.NSObject sender) {
            // Handle click here
            ...
        }
        #endregion
    }
}
```

Aby uzyskać więcej informacji na temat pracy z Scenorys, zobacz nasze [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md). W szczególności [Tworzenie interfejsu użytkownika](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface) i [pisanie kodu z gniazda i działaniami](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) sekcje.

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>Gestów i kod

Opcjonalnie możesz utworzyć gestów bezpośrednio w kodzie języka C# i dodaj je do widoków w interfejsie użytkownika. Na przykład aby dodać serii aparaty rozpoznawania gestów Przejdź, Edytuj kontroler widoku i Dodaj następujący kod:

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class SecondViewController : UIViewController
    {
        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();    

            // Wire-up gestures
            var upGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Up";
                ButtonLabel.Text = "Swiped Up";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Up
            };
            this.View.AddGestureRecognizer (upGesture);

            var downGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Down";
                ButtonLabel.Text = "Swiped Down";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Down
            };
            this.View.AddGestureRecognizer (downGesture);

            var leftGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Left";
                ButtonLabel.Text = "Swiped Left";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Left
            };
            this.View.AddGestureRecognizer (leftGesture);

            var rightGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Right";
                ButtonLabel.Text = "Swiped Right";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Right
            };
            this.View.AddGestureRecognizer (rightGesture);
        }
        #endregion
    }
}
```

<a name="Low-Level-Event-Handling" />

## <a name="low-level-event-handling"></a>Obsługa zdarzeń niskiego poziomu

W przypadku tworzenia niestandardowych typu, na podstawie `UIKit` w aplikacji Xamarin.tvOS (na przykład `UIView`), również mieć możliwość zapewnienia niskiego poziomu obsługi naciśnięcie przycisku za pośrednictwem `UIPress` zdarzenia. 

A `UIPress` zdarzenie jest do systemu tvOS `UITouch` zdarzenie jest w systemach iOS, z wyjątkiem `UIPress` zwraca informacje o przycisku naciśnie na komputerze zdalnym Siri lub inne dołączone urządzenia Bluetooth (na przykład kontroler gry). `UIPress` zdarzenia opisano naciśnięcie przycisku i jego stan (Began, anulowane, zmienione lub upłynął). 

Analogowy przycisków w urządzeniami, takimi jak kontrolery gier Bluetooth `UIPress` również zwraca ilość życie stosowana na przycisku. `Type` Właściwość `UIPress` zdarzeń definiuje, które fizyczne przycisk ma zmienione stanu, a pozostałe właściwości opisano zmiany, który wystąpił.

Poniższy kod przedstawia przykład obsługa niskiego poziomu `UIPress` zdarzeń dla `UIView`:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvRemote
{
    public partial class EventView : UIView
    {
        #region Computed Properties
        public override bool CanBecomeFocused {
            get {
                return true;
            }
        }
        #endregion

        #region 
        public EventView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void PressesBegan (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesBegan (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Red;
                }
            }
        }

        public override void PressesCancelled (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesCancelled (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }

        public override void PressesChanged (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesChanged (presses, evt);
        }

        public override void PressesEnded (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesEnded (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }
        #endregion
    }
}
```

Jak `UITouch` zdarzenia, jeśli musisz wdrożyć dowolną z `UIPress` zastąpienia zdarzeń, należy zaimplementować wszystkie cztery.

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Kontrolery gier Bluetooth

Oprócz standardowych zdalnego Siri dostarczaną z programem Apple TV, strona 3, wprowadzone dla systemu iOS można łączyć się z Apple TV i użyć do kontrolowania aplikacji Xamarin.tvOS kontrolery gier Bluetooth (MIF).

[![](remote-bluetooth-images/game01.png "Kontrolery gier Bluetooth")](remote-bluetooth-images/game01.png#lightbox)

Kontrolery gier może służyć do zwiększenia gry i zapewnienia w pewnym sensie immersyjną w grę. One mogą służyć do kontrolowania standardowy interfejs Apple TV, więc użycie nie ma w celu przełączania się między zdalnego i kontroler.

> [!IMPORTANT]
> Kontrolery gier Bluetooth są opcjonalne zakupu, które może uniemożliwić użytkownikom końcowym, aplikacja nie zmusza użytkownika do kupić. Jeśli aplikacja obsługuje kontrolery gier również musi obsługiwać zdalne Siri tak, aby gry jest użyteczny przez wszystkich użytkowników Apple TV.

Kontroler gry ma następujące funkcje i oczekiwanego użycia w aplikacji systemu tvOS:

|Funkcja|Użycie aplikacji ogólne|Użycie aplikacji gry|
|---|---|---|
|**D-Pad**|Nawiguje elementy interfejsu użytkownika (zmiany fokusu).|Zależy od gry.|
|**A**|Aktywuje wybranego elementu (fokusu).|Przycisk podstawowy działa i potwierdza Akcje okna dialogowego.|
|**B**|Zwraca do poprzedniego ekranu lub kończy pracę na ekranie głównym, jeśli na ekranie głównym aplikacji.|Wykonuje funkcję przycisku dodatkowej lub zwraca do poprzedniego ekranu.|
|**X**|Rozpoczyna odtwarzanie multimediów lub Wstrzymaj/wznawia odtwarzanie.|Zależy od gry.|
|**Y**|n/d|Zależy od gry.|
|**Menu**|Zwraca do poprzedniego ekranu lub kończy pracę na ekranie głównym, jeśli na ekranie głównym aplikacji.|Wstrzymanie/wznowienie gry, zwraca do poprzedniego ekranu lub wyjść na ekranie głównym Jeśli na ekranie głównym aplikacji.|
|**Ramię lewego przycisku**|Nawiguje po lewej.|Zależy od gry.|
|**Po lewej stronie wyzwalacza**|Nawiguje po lewej.|Zależy od gry.|
|**Ramię prawy przycisk**|Nawiguje po prawej.|Zależy od gry.|
|**Prawy wyzwalacza**|Przechodzi do prawej|Zależy od gry.|
|**Po lewej stronie Thumbstick**|Nawiguje elementy interfejsu użytkownika (zmiany fokusu).|Zależy od gry.|
|**Prawy Thumbstick**|n/d|Zależy od gry.|

Apple zawiera poniższe sugestie dotyczące pracy z kontrolery gier:

- **Potwierdź połączenia kontrolera gry** -aplikację systemu tvOS można uruchomić i zatrzymać w dowolnym momencie przez użytkownika końcowego. Należy zawsze sprawdzić obecność kontrolera gry na uruchomienie aplikacji lub wznowione godzin i podejmij akcję, zgodnie z potrzebami.
- **Upewnij się, Twoja aplikacja działa na używanie programu Siri zdalnego i kontrolery gier** — nie wymagać od użytkowników w celu przełączania się między zdalnego Siri a kontrolerem gry, aby móc korzystać z aplikacji. Testowanie aplikacji często z oboma typami kontrolerów zagwarantowaniu, że wszystko, co ułatwia przechodzenie i działa zgodnie z oczekiwaniami.
- **Podaj sposób ponownie** -naciśnięcie przycisku Menu powinien zawsze powrócić do poprzedniego ekranu. Jeśli użytkownik znajduje się na ekranie głównym aplikacji, przycisk Menu powinien zwrócić je do ekranu Apple TV Home. Podczas gry przycisk Menu powinien wyświetlić alert, która umożliwia użytkownikowi możliwość wstrzymywania/wznawiania gry lub wrócić do menu głównego.

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>Praca z kontrolery gier

Jak już wspomniano, oprócz standardowej Siri zdalnie, który jest dostarczany z Apple TV, użytkownik może opcjonalnie dołączyć 3rd innej firmy, kontrolery gier Bluetooth wprowadzone dla systemu iOS (MIF) i używać go do kontroli aplikacji Xamarin.tvOS.

W razie potrzeby wprowadzania niskiego poziomu kontrolera aplikacji można używa firmy Apple [Framework kontrolera gry](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) mającego następujących modyfikacji systemu tvOS:

- Profil kontrolera gry Micro (`GCMicroGamepad`) został dodany do zdalnego Siri.
- Nowe `GCEventViewController` klasy może służyć do kierowania zdarzenia kontroler gier za pośrednictwem aplikacji. Zobacz [określania wprowadzania kontrolera gry](#Determining-Game-Controller-Input) sekcji poniżej, aby uzyskać więcej informacji.

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>Wymagania dotyczące obsługi kontrolera gier

Apple ma kilka wymagań, które muszą zostać spełnione w przypadku aplikacji Xamarin.tvOS obsługuje kontrolery gier:

- **Musi obsługiwać zdalne Siri** -zawsze musi obsługiwać zdalne Siri. Twoja gra nie mogą wymagać 3 firmy będzie można odtwarzać kontroler.
- **Rozszerzone układ formantu musi obsługiwać** -systemu tvOS wszystkie kontrolery gier są nie-rozciągliwe, dopasowane, rozszerzony kontrolerów.
- **Gry musi być Playable z kontrolerami autonomicznego** — Jeśli aplikacja obsługuje rozszerzony gry kontrolera, musi ona będzie można odtwarzać wyłącznie z danego kontrolera gier.
- **Przycisk Odtwórz/Wstrzymaj musi obsługiwać** -podczas gry, gdy użytkownik naciśnie przycisk Odtwórz/Wstrzymaj należy wyświetlić alert, zapewniając możliwość wstrzymywania/wznawiania gry użytkownika lub wrócić do menu głównego.

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>Włączanie obsługi kontrolera gier

Aby włączyć obsługę kontrolera gry w aplikacji Xamarin.tvOS, kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji:

[![](remote-bluetooth-images/game02.png "Edytor Info.plist")](remote-bluetooth-images/game02.png#lightbox)

W obszarze **kontrolera gry** sekcji, zaznacz przez **włączyć kontrolery gier**, następnie sprawdź wszystkich typów kontrolera gier, które będą obsługiwane przez aplikację.

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>Przy użyciu zdalnego Siri jako kontrolera gier

Zdalne Siri, które są dostarczane z Apple TV może służyć jako kontroler gry ograniczone. Podobnie jak inne kontrolery gier, jest wyświetlane w ramach kontrolera gry jako `GCController` obiekt i obsługuje `GCMotion` i `GCMicroGamepad` profilów.

Kiedy jako kontrolera gry, zdalnego Siri ma następującą charakterystykę:

- Dotykać powierzchni może służyć jako D-konsoli, która zapewnia analogowy danych wejściowych.
- Zdalne mogą być używane w orientacji pionowej lub poziomej i decyduje o tym aplikacji, jeśli obiekt profil powinien automatycznie Przerzuć danych wejściowych.
- Kliknięcie przycisku dotykać powierzchni działa jak naciśnięcie przycisku **A** na kontrolerze gier.
- Przycisk Odtwórz/Wstrzymaj zachowuje się jak przycisk **X** na kontrolerze gier.
- Przycisk Menu powinien wyświetlić alert, która umożliwia użytkownikowi możliwość wstrzymywania/wznawiania gry lub wrócić do menu głównego.

<a name="Summary" />

### <a name="determining-game-controller-input"></a>Określanie wprowadzania kontrolera gier

W przeciwieństwie do iOS, gdy kontroler gry zdarzeń może zostać odebrany równolegle ze zdarzeniami Touch, systemu tvOS przetwarza wszystkie zdarzenia niskiego poziomu do dostarczania wysokiego poziomu `UIKit` zdarzenia. W związku z tym, jeśli potrzebujesz dostępu na niskim poziomie zdarzenia kontrolera gry, musisz wyłączyć `UIKit`przez zachowanie domyślne.

Na systemu tvOS, gdy użytkownik chce przetworzyć danych wejściowych kontrolera gier, bezpośrednio należy użyć `GCEventViewController` (lub jego odpowiednia podklasa) do wyświetlenia gry elementu interfejsu użytkownika. Zawsze, gdy `GCEventViewController` jest *pierwszy obiekt odpowiadający w trybie*, wprowadzania gry kontrolera zostanie przechwycony i dostarczane do aplikacji za pośrednictwem Framework kontrolera gry.

Można użyć `UserInteractionEnabled` właściwość `GCEventViewController` klasy, aby przełączyć sposób przetwarzania i obsługi zdarzeń.

Informacji o implementacji obsługi kontrolera gier, zobacz firmy Apple [Praca z kontrolery gier](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html) sekcji [Podręcznik programowania aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html) i [kontrolera gry Przewodnik programowania w języku](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html).

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego nowego zdalnego Siri dostarczaną z programem Apple TV, gestów dotykać powierzchni i zdalnego Siri przycisków. Następnie objętych usługą pracy z gestów Scenorys, gestów i kodu oraz zdarzenia niskiego poziomu. Ponadto jeśli omówione Praca z kontrolery gier.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
