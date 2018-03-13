---
title: "Podstawowe rozszerzenia aplikacji wiadomości"
description: "W tym artykule przedstawiono sposób obejmują rozszerzenia aplikacji wiadomości w rozwiązaniu Xamarin.iOS, która integruje się z aplikacji Messages i wyświetla użytkownikowi nowych funkcji."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d9b6b5a778e0e4d5092d1036109f82896acf639b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="message-app-extension-basics"></a>Podstawowe rozszerzenia aplikacji wiadomości

_W tym artykule przedstawiono sposób obejmują rozszerzenia aplikacji wiadomości w rozwiązaniu Xamarin.iOS, która integruje się z aplikacji Messages i wyświetla użytkownikowi nowych funkcji._

Nowe w systemach iOS 10, rozszerzenie aplikacji komunikat integruje się z **wiadomości** aplikacji i przedstawia nowych funkcji dla użytkownika. Rozszerzenie można wysyłać tekstu, nalepek, plików multimedialnych i interaktywne wiadomości.

## <a name="about-message-app-extensions"></a>Temat wiadomości rozszerzeń aplikacji

Jak już wspomniano, rozszerzenie aplikacji komunikat integruje się z **wiadomości** aplikacji i przedstawia nowych funkcji dla użytkownika. Rozszerzenie można wysyłać tekstu, nalepek, plików multimedialnych i interaktywne wiadomości. Dostępne są dwa typy rozszerzenia aplikacji komunikat:

- **Pakiety naklejce** — zawiera kolekcję nalepek, które użytkownik może dodać do komunikatu. Naklejce pakiety mogą być tworzone bez pisania żadnego kodu.
- **iMessage aplikacji** — może ona powodować niestandardowy interfejs użytkownika w aplikacji Messages na wybranie nalepek, wprowadzania tekstu, w tym plików multimedialnych (z konwersje typów opcjonalne) i tworzeniem, edytowaniem i wysyłanie komunikatów interakcji.

Rozszerzenia komunikatów aplikacji zawierają trzy typy zawartości:

- **Interakcyjne wiadomości** -są typ zawartości niestandardowy komunikat, który generuje aplikacji, po naciśnięciu w komunikacie, aplikacja zostanie uruchomiona na pierwszym planie.
- **Nalepek** — obrazy generowane przez aplikację, która może być uwzględniony w wiadomości między użytkownikami.
- **Inne obsługiwane zawartości** — aplikacji można udostępnić zawartość, takie jak zdjęcia, wideo, tekst lub typy łącza do innej zawartości, które są zawsze obsługiwane przez aplikację wiadomości.

Nowość w systemach iOS 10, komunikat aplikacji zawiera teraz własnego Sklepu z aplikacjami dedykowanym, wbudowane. Wszystkie aplikacje, które obejmują rozszerzenia komunikatów aplikacji są wyświetlane i wspierane w tym magazynie. Szufladzie aplikacji komunikaty będą wyświetlane wszystkie aplikacje, które zostały pobrane ze sklepu z aplikacjami wiadomości co zapewnia szybki dostęp do użytkowników.

Ponadto nowy w systemie iOS 10, Apple został dodany wbudowanego autorstwa aplikacji, dzięki czemu można łatwo wykryć aplikacji. Na przykład jeśli jeden użytkownik wysyła zawartość do innego z aplikacji, która 2 użytkownik nie ma zainstalowany (na przykład naklejce na przykład), nazwę aplikacji wysyłania jest wyświetlony w obszarze zawartości w historii wiadomości. Jeśli użytkownik naciska aplikacji nazw, firma Microsoft można otworzyć magazynu aplikacji oraz aplikacji wybrane w magazynie.

Rozszerzenia komunikatów aplikacji są podobne do istniejących aplikacji systemu iOS, że dewelopera jest zapoznać się z tworzeniem i dostęp do wszystkich platform standardowe i funkcje aplikacji iOS standardowe. Na przykład:

- Mają dostęp do zakupu w aplikacji.
- Mają dostęp do Apple Pay.
- Mają dostęp do urządzeń sprzętowych, takich jak aparatu.

Rozszerzenia komunikatów aplikacji są obsługiwane tylko w systemie iOS 10, jednak zawartość, która wysyłać te rozszerzenia są widoczne na urządzeniach watchOS i macOS. Nowy _strony ostatnich_ dodane do watchOS 3, wyświetlać ostatnie naklejek zostały wysłane w telefonie, łącznie z tymi z rozszerzeń aplikacji wiadomości i Zezwalaj użytkownikowi na wysyłanie tych nalepek z czujki.

## <a name="about-the-messages-framework"></a>Temat wiadomości Framework

Nowość w systemach iOS 10, framework wiadomości udostępnia interfejs między rozszerzenie aplikacji wiadomości i aplikacji komunikat na urządzeniu z systemem iOS przez użytkownika. Gdy użytkownik uruchomi aplikację z wewnątrz aplikacji wiadomości, platforma umożliwia aplikacji, które mają zostać wykryte i dostarcza dane i kontekstu wymagane do określania układu jego interfejsie użytkownika.

Po uruchomieniu aplikacji, użytkownik wchodzi w interakcję z go w celu utworzenia nowej zawartości udostępnianej za pośrednictwem wiadomości. Aplikacja używa następnie framework wiadomości do transferu zawartości nowo utworzonej aplikacji komunikaty do przetwarzania.

Struktura wiadomości i rozszerzenia komunikatów aplikacji tworzonych z myślą iOS już istniejących technologii rozszerzeń aplikacji. Aby uzyskać więcej informacji dotyczących rozszerzeń aplikacji, zobacz firmy Apple [przewodnik programowania w języku rozszerzenie aplikacji](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

W przeciwieństwie do innych punktów rozszerzenia zapewnianej przez firmy Apple w całym systemie dewelopera trzeba podawać aplikacji hosta dla ich rozszerzeń aplikacji wiadomości, ponieważ komunikat aplikacja działa jako kontener. Jednak deweloper może z rozszerzeniem komunikat aplikacji wewnątrz aplikacji iOS nowego lub istniejącego i wysyłania go wraz z pakietu.

Jeśli komunikatu rozszerzeń aplikacji jest uwzględniony w pakiecie aplikacji systemu iOS, pojawi się ikona aplikacji zarówno w ekranu urządzenia, jak i w szufladzie aplikacji wiadomości z poziomu aplikacji wiadomości. Jeśli nie jest uwzględniony w pakiecie aplikacji, rozszerzenie aplikacji wiadomości tylko będzie wyświetlane w szufladzie komunikatów aplikacji.

Nawet w przypadku komunikatu rozszerzeń aplikacji nie jest uwzględniony w pakiecie aplikacji hosta, deweloper będzie musiał podać ikony aplikacji w pakiecie rozszerzenia aplikacji wiadomości jest ikonę, która będzie wyświetlana w innych częściach systemu, takie jak wiadomości aplikacji szuflady lub ustawienia , rozszerzenia.

## <a name="about-stickers"></a>O nalepek

Apple zaprojektowane nalepek nowy sposób iMessage użytkowników do komunikacji przez nalepek do wysłania wbudowanego jako treść komunikatu lub ich może zostać dołączony do poprzedniego dymki komunikat w konwersacji.

Co to są nalepek?

- Są one obrazów, które udostępnia rozszerzenie aplikacji wiadomości.
- Mogą to być animowane lub statycznej obrazów.
- Udostępniają one nowy sposób, aby udostępnić zawartość obrazu z wewnątrz aplikacji.

Istnieją dwa sposoby tworzenia nalepek:

1. Naklejce pakiet komunikatu aplikacji rozszerzeń mogą być tworzone z wewnątrz Xcode bez uwzględniania żadnego kodu. Jest wymagany jest zasoby nalepek i ikony aplikacji.
2. Tworząc standardowym rozszerzeniem wiadomości w aplikacji, która zapewnia nalepek z kodu za pomocą framework wiadomości.

### <a name="creating-sticker-packs"></a>Tworzenie pakietów naklejce

Pakiety naklejce są tworzone na podstawie szablonu specjalne wewnątrz Xcode i po prostu zapewniają zestaw statyczny obraz zasoby, które mogą być używane jako nalepek. Jak już wspomniano, nie wymaga żadnego kodu, po prostu dewelopera przeciąga pliki obrazów do folderu pakietu naklejce wewnątrz nalepek katalogu zasobów.

W przypadku obrazu do uwzględnienia w pakiecie naklejce spełniają następujące wymagania:

- Obrazy muszą być w formacie PNG, APNG, GIF lub JPEG. Apple sugeruje przy użyciu tylko format PNG i APNG podczas dostarczania naklejce zasoby.
- Animowany nalepek obsługują tylko format APNG i GIF.
- Obrazy naklejce powinien zapewnić przezroczyste tło, ponieważ mogą być nawiązywane za pośrednictwem dymki komunikat w konwersacji przez użytkownika.
- Pliki obrazów muszą mieć mniej niż 500kb.
- Obrazy nie może być mniejsza niż 100 x 100 punktów lub większa tego punktów 206 x 206.

> [!IMPORTANT]
> **Uwaga:** zawsze należy podawać obrazy naklejce na `@3x` rozwiązania w zakresie 300 x 300 do 618 x 618 pikseli. System automatycznie wygeneruje `@2x` i `@1x` wersji w czasie wykonywania, zgodnie z wymaganiami.

Apple sugeruje testowania zasoby obrazu naklejce przed różnych różnych kolorowe tło (na przykład biały, czarny, czerwony, żółty i wielu kolorze) i powyżej zdjęć do zapewnienia wyglądają najlepiej w wszystkich możliwych sytuacji.

Pakiety naklejce zapewniają nalepek w jednym z trzech dostępnych rozmiarów:

- **Mała** - 100 x 100 punktów.
- **Średnia liczba godzin** — punkty 136 x 136. Jest to domyślny rozmiar.
- **Duże** — punkty 206 x 206.

Użyj Inspektora atrybutów w środowisku Xcode ustawiania rozmiaru dla całego pakietu naklejce i zapewniają tylko zasoby obrazów, zgodne żądany rozmiar, aby uzyskać najlepsze wyniki w przeglądarce naklejce wewnątrz aplikacji wiadomości.

Aby uzyskać więcej informacji, zobacz nasze [konstruktora lodów](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) aplikacji i firmy Apple [odwołania wiadomości](https://developer.apple.com/reference/messages).

## <a name="creating-a-custom-sticker-experience"></a>Tworzenie środowiska naklejce niestandardowych

Jeśli aplikacja wymaga więcej kontroli i elastyczność niż są udostępniane przez pakiet naklejce, jej rozszerzenie aplikacji wiadomości i podaj nalepek za pośrednictwem wiadomości framework środowisko naklejce niestandardowe.

Jakie są zalety utworzyć środowisko naklejce niestandardowe?

1. Zezwala aplikacji na dostosowanie sposobu wyświetlania nalepek do użytkowników aplikacji. Na przykład do prezentowania nalepek w formacie innym niż standardowe siatki układu lub na innym kolor tła.
2. Umożliwia nalepek można dynamicznie utworzyć z kodu zamiast dołączaniu jako statyczny obraz zasoby.
3. Umożliwia naklejce obrazu zasoby mają być dynamicznie pobrane z serwera sieci web developer's bez konieczności wydania nowej wersji do sklepu z aplikacjami.
4. Dla dostępu aparatu fotograficznego urządzenia umożliwia utworzenie nalepek na bieżąco.
5. Zezwala na zakupy w aplikacji, więc użytkownika można kupić więcej nalepek z wewnątrz aplikacji.

Aby utworzyć środowisko naklejce niestandardowe, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom program Visual Studio dla komputerów Mac.
2. Otwórz rozwiązanie, aby dodać rozszerzenie aplikacji wiadomości do. 
3. Wybierz **iOS** > **rozszerzenia** > **iMessage rozszerzenia** i kliknij przycisk **dalej** przycisk: 

    [![](intro-to-message-app-extensions-images/message01.png "Wybierz iMessage rozszerzenia")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. Wprowadź **Nazwa rozszerzenia** i kliknij przycisk **dalej** przycisk: 

    [![](intro-to-message-app-extensions-images/message02.png "Wprowadź nazwę rozszerzenia")](intro-to-message-app-extensions-images/message02.png#lightbox)
5. Kliknij przycisk **Utwórz** przycisku do tworzenia rozszerzenia: 

    [![](intro-to-message-app-extensions-images/message03.png "Kliknij przycisk Utwórz")](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Uruchom program Visual Studio.
2. Otwórz rozwiązanie, aby dodać rozszerzenie aplikacji wiadomości do. 
3. Wybierz **iOS** > **rozszerzenia** > **iMessage rozszerzenia** i kliknij przycisk **dalej** przycisk: 

    [![](intro-to-message-app-extensions-images/message01w.png "Wybierz iMessage rozszerzenia")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. Wprowadź **Nazwa rozszerzenia** i kliknij przycisk **OK** przycisku

-----

Domyślnie `MessagesViewController.cs` plik zostanie dodany do rozwiązania. To jest główny punkt wejścia do rozszerzenia i dziedziczy go `MSMessageAppViewController` klasy.

Framework wiadomości udostępnia klasy do prezentowania nalepek dostępnych dla użytkownika:

- `MSStickerBrowserViewController` -Steruje nalepek będą wyświetlane w widoku. Również odpowiada `IMSStickerBrowserViewDataSource` interfejsu zwrócą liczba naklejce i naklejce przeglądarki danego indeksu.
- `MSStickerBrowserView` -To jest dostępne nalepek będą wyświetlane w widoku.
- `MSStickerSize` -Postanowi rozmiary pojedynczych komórek w siatce nalepek przedstawionych w widoku przeglądarki.

### <a name="creating-a-custom-sticker-browser"></a>Tworzenie przeglądarki naklejce niestandardowych

Dewelopera można dostosować środowisko naklejce dla użytkownika, podając przeglądarki naklejce niestandardowe (`MSMessageAppBrowserViewController`) w rozszerzeniu aplikacji wiadomości. Przeglądarka naklejce niestandardowe zmienia sposób przedstawiania nalepek do użytkownika podczas wybierania naklejce do uwzględnienia w strumieniu wiadomości.

Wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. W **konsoli rozwiązania**, kliknij prawym przyciskiem myszy na rozszerzenie nazwy projektu i wybierz **Dodaj** > **nowego pliku...**   >  **systemu iOS | Apple Watch** > **Kontroler interfejsu**.
2. Wprowadź `StickerBrowserViewController` dla **nazwa** i kliknij przycisk **nowy** przycisk: 

    [![](intro-to-message-app-extensions-images/browser01.png "Wprowadź nazwę StickerBrowserViewController")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. Otwórz `StickerBrowserViewController.cs` plik do edycji.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na rozszerzenie nazwy projektu i wybierz **Dodaj** > **nowego pliku...**   >  **systemu iOS | Apple Watch** > **Kontroler interfejsu**.
2. Wprowadź `StickerBrowserViewController` dla **nazwa** i kliknij przycisk **nowy** przycisk: 

    [![](intro-to-message-app-extensions-images/browser01w.png "Wprowadź nazwę StickerBrowserViewController")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. Otwórz `StickerBrowserViewController.cs` plik do edycji.

-----

Wprowadź `StickerBrowserViewController.cs` wyglądać następująco:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MonkeyStickers
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MSStickerSize stickerSize) : base (stickerSize)
        {
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            ...
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();
        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            return Stickers[(int)index];
        }
        #endregion
    }
}
```

Spójrz na powyższy kod szczegółowo. Tworzy magazynu dla naklejek udostępnia rozszerzenie:

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

I zastępuje dwie metody `MSStickerBrowserViewController` klasy dostarczający dane dla przeglądarki z tego magazynu danych:

```csharp
public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
{
    return Stickers.Count;
}

public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
{
    return Stickers[(int)index];
}
```

`CreateSticker` Metoda pobiera ścieżkę zasób obrazu z pakietu rozszerzenia i używa go do utworzenia nowego wystąpienia `MSSticker` z tego zasobu, który dodaje go do kolekcji:

```csharp
private void CreateSticker (string assetName, string localizedDescription)
{

    // Get path to asset
    var path = NSBundle.MainBundle.PathForResource (assetName, "png");
    if (path == null) {
        Console.WriteLine ("Couldn't create sticker {0}.", assetName);
        return;
    }

    // Build new sticker
    var stickerURL = new NSUrl (path);
    NSError error = null;
    var sticker = new MSSticker (stickerURL, localizedDescription, out error);
    if (error == null) {
        // Add to collection
        Stickers.Add (sticker);
    } else {
        // Report error
        Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
    }
}
```

`LoadSticker` Metoda jest wywoływana z `ViewDidLoad` Utwórz naklejce na podstawie zasobów nazwanych obrazów (zawartych w pakiecie aplikacji) i dodaj go do kolekcji nalepek.

Aby zaimplementować przeglądarki naklejce niestandardowe, należy edytować `MessagesViewController.cs` pliku i zapewnić ich wyglądać następująco:

```csharp
using System;
using UIKit;
using Messages;

namespace MonkeyStickers
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public StickerBrowserViewController BrowserViewController { get; set;}
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();


            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);
        }
        #endregion
    }
}
```

Biorąc Obejrzyj ten kod szczegółowo, tworzy magazynu niestandardowego przeglądarki:

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

I w `ViewDidLoad` metody go tworzy i konfiguruje nowe przeglądarki:

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

Następnie dodaje przeglądarki do widoku, aby go wyświetlić:

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>Dalsze dostosowanie naklejce

Możliwe jest dalsze dostosowanie naklejce przez uwzględnienie dwóch klas w wiadomości rozszerzenia aplikacji:

- `MSStickerView`
- `MSSticker`

Przy użyciu metod powyżej, rozszerzenia może obsługiwać naklejce wybranej nie bazuje na standardowe metody naklejce przeglądarki. Ponadto wyświetlana naklejce mogą być przełączane z dwóch trybów inny widok:

- **Compact** — jest to domyślny tryb gdy widok naklejce zajmuje 25% widoku wiadomości.
- **Rozszerzona** — widok naklejce wypełnienia całego widoku komunikatów.

Ten widok naklejce mogą być przełączane tych trybów programowo lub ręcznie przez użytkownika.

Spójrz na poniższym przykładzie obsługa przełączanie między dwa tryby inny widok. Dwa różne kontrolerów widoku będzie wymagane dla każdego stanu. `StickerBrowserViewController` Dojść **Compact** widoku i wygląda podobnie do następującego:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MessageExtension
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set; }
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MessagesViewController messagesAppViewController, MSStickerSize stickerSize) : base (stickerSize)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("add", "Add New Sticker");
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();

        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            // Wanting to add a new sticker?
            if (index == 0) {
                // Yes, ask controller to present add sticker interface
                MessagesAppViewController.AddNewSticker ();
                return null;
            } else {
                // No, return existing sticker
                return Stickers [(int)index];
            }
        }
        #endregion
    }
}
```

`AddStickerViewController` Obsłuży **rozwinięty** naklejce widoku i wyglądać podobnie do następującej:

```csharp
using System;
using Foundation;
using UIKit;
using Messages;

namespace MessageExtension
{
    public class AddStickerViewController : UIViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set;}
        public MSSticker NewSticker { get; set;}
        #endregion

        #region Constructors
        public AddStickerViewController (MessagesViewController messagesAppViewController)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Override Method
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Build interface to create new sticker
            var cancelButton = new UIButton (UIButtonType.RoundedRect);
            cancelButton.TouchDown += (sender, e) => {
                // Cancel add new sticker
                MessagesAppViewController.CancelAddNewSticker ();
            };
            View.AddSubview (cancelButton);

            var doneButton = new UIButton (UIButtonType.RoundedRect);
            doneButton.TouchDown += (sender, e) => {
                // Add new sticker to collection
                MessagesAppViewController.AddStickerToCollection (NewSticker);
            };
            View.AddSubview (doneButton);
            
            ...
        }
        #endregion
    }
}
```

`MessageViewController` Implementuje te kontrolery widoku do żądanego stanu:

```csharp
using System;
using UIKit;
using Messages;

namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public bool IsAddingSticker { get; set;}
        public StickerBrowserViewController BrowserViewController { get; set; }
        public AddStickerViewController AddStickerController { get; set;}
        #endregion

        #region Constructors
        protected MessagesViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Public Methods
        public void PresentStickerBrowser ()
        {
            // Is the Add sticker view being displayed?
            if (IsAddingSticker) {
                // Yes, remove it from view
                AddStickerController.RemoveFromParentViewController ();
                AddStickerController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);

            // Save mode
            IsAddingSticker = false;
        }

        public void PresentAddSticker ()
        {
            // Is the sticker browser being displayed?
            if (!IsAddingSticker) {
                // Yes, remove it from view
                BrowserViewController.RemoveFromParentViewController ();
                BrowserViewController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (AddStickerController);
            AddStickerController.DidMoveToParentViewController (this);
            View.AddSubview (AddStickerController.View);

            // Save mode
            IsAddingSticker = true;
        }

        public void AddNewSticker ()
        {
            // Switch to expanded view mode
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void CancelAddNewSticker ()
        {
            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }

        public void AddStickerToCollection (MSSticker sticker)
        {
            // Add sticker to collection
            BrowserViewController.Stickers.Add (sticker);

            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (this, MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Create new Add controller and configure it as well
            AddStickerController = new AddStickerViewController (this);
            AddStickerController.View.Frame = View.Frame;

            // Initially present the sticker browser
            PresentStickerBrowser ();
        }

        public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            base.DidTransition (presentationStyle);

            // Take action based on style
            switch (presentationStyle) {
            case MSMessagesAppPresentationStyle.Compact:
                PresentStickerBrowser ();
                break;
            case MSMessagesAppPresentationStyle.Expanded:
                PresentAddSticker ();
                break;
            }
        }
        #endregion
    }
}
```

Gdy użytkownik zażąda, aby dodać nowy naklejce do ich dostępnych kolekcji, nowy `AddStickerViewController` staje się widoczny kontrolerem a widokiem naklejce wprowadza **rozwinięty** widoku:

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

Gdy użytkownik wybierze naklejce można dodać, jest ona dodawana do ich dostępnych kolekcji i **Compact** zażądano widoku:

```csharp
public void AddStickerToCollection (MSSticker sticker)
{
    // Add sticker to collection
    BrowserViewController.Stickers.Add (sticker);

    // Switch to compact view mode
    Request (MSMessagesAppPresentationStyle.Compact);
}
```

`DidTransition` Metoda zostanie przesłonięta do obsługi przełączanie tych dwóch trybów:

```csharp
public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
{
    base.DidTransition (presentationStyle);

    // Take action based on style
    switch (presentationStyle) {
    case MSMessagesAppPresentationStyle.Compact:
        PresentStickerBrowser ();
        break;
    case MSMessagesAppPresentationStyle.Expanded:
        PresentAddSticker ();
        break;
    }
}
``` 

## <a name="summary"></a>Podsumowanie

Ma omówione w tym artykule rozszerzenie aplikacji wiadomości w rozwiązaniu Xamarin.iOS, która integruje się z **wiadomości** aplikacji i obecny nowych funkcji dla użytkownika. Objęte go przy użyciu rozszerzenia do wysyłania tekstu, nalepek, plików multimedialnych i interaktywne wiadomości.



## <a name="related-links"></a>Linki pokrewne

- [Konstruktor lodów (przykład)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [Odwołanie wiadomości](https://developer.apple.com/reference/messages)
- [Przewodnik programowania w języku rozszerzenia aplikacji](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
