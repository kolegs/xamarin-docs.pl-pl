---
title: Powiadomienia użytkowników zaawansowanych w Xamarin.iOS
description: Ten artykuł przedstawia głębiej w ramach powiadomienia użytkownika, wprowadzone w systemie iOS 10. Zawarto informacje powiadomienia użytkownika, interfejs użytkownika powiadomienie, załączniki nośnika i niestandardowych interfejsów użytkownika.
ms.prod: xamarin
ms.assetid: 4E0C60AE-6F54-4098-8FA0-AADF9AC86805
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: 09a73ebc3dab90e6342a45c0f1fb5a40184d18a6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788533"
---
# <a name="advanced-user-notifications-in-xamarinios"></a>Powiadomienia użytkowników zaawansowanych w Xamarin.iOS

Jesteś nowym użytkownikiem iOS 10, powiadomienie użytkownika pozwala dostarczania i obsługi powiadomień lokalnych i zdalnych. Przy użyciu tego framework, aplikacji lub rozszerzenia aplikacji można zaplanować dostarczania lokalnego powiadomienia, określając zestaw warunków, takich jak lokalizacji lub godzinę.

## <a name="about-user-notifications"></a>Powiadomienia użytkowników — informacje

Nowa struktura powiadomienie użytkownika umożliwia dostarczania i obsługę powiadomień lokalnych i zdalnych. Przy użyciu tego framework, aplikacji lub rozszerzenia aplikacji można zaplanować dostarczania lokalnego powiadomienia, określając zestaw warunków, takich jak lokalizacji lub godzinę.

Ponadto, aplikacji lub rozszerzenia można odbierać (i potencjalnie modyfikować) powiadomień lokalnych i zdalnych jako zostaną dostarczone do urządzeń z systemem iOS przez użytkownika.

Nowa struktura interfejsu użytkownika powiadomienie użytkownika umożliwia aplikacji lub rozszerzenia aplikacji do dostosowywania wyglądu powiadomień lokalnych i zdalnych, kiedy mają być przedstawiane dla użytkownika.

Ta struktura zapewnia następujące sposoby, że aplikację można dostarczać powiadomienia dla użytkownika:

- **Alerty Visual** — gdy przedstawia powiadomienia w dół od górnej krawędzi ekranu w formie transparentu.
- **Dźwięk i drgań** — może być skojarzony z powiadomienie.
- **Badging ikona aplikacji** — w przypadku, gdy ikona aplikacji jest wyświetlana wskaźnika wskazująca, że nowa zawartość jest dostępna. Takie jak liczba nieprzeczytanych wiadomości.

Ponadto w zależności od bieżącego kontekstu użytkownika, istnieją różne sposoby, które zostanie wyświetlone powiadomienie:

- Jeśli urządzenie zostanie odblokowane, powiadomienia będą wycofać z górnej części ekranu jako transparent.
- Jeśli urządzenie jest zablokowane, powiadomienia będą wyświetlane na ekranie blokady użytkownika.
- Jeśli użytkownik ma pominięte powiadomienie, mogą otworzyć Centrum powiadomień i wyświetlić wszystkie dostępne, powiadomienia oczekiwania.

Aplikacji platformy Xamarin.iOS ma dwa typy powiadomień użytkownika, który jest w stanie wysyłać:

- **Lokalnego powiadomienia** — te są wysyłane przez aplikacje są instalowane lokalnie na urządzeniach użytkowników.
- **Powiadomienia zdalnego** — są wysyłane z serwera zdalnego, a następnie użytkownik widzi lub wyzwala aktualizację tła zawartości aplikacji.

Aby uzyskać więcej informacji, zobacz nasze [rozszerzone powiadomienia użytkownika](~/ios/platform/user-notifications/enhanced-user-notifications.md) dokumentacji.

## <a name="the-new-user-notification-interface"></a>Nowy interfejs powiadomienie użytkownika

Powiadomienia użytkowników w systemie iOS 10 są prezentowane nowy projekt interfejsu użytkownika, który zawiera więcej zawartości, takich jak tytuł, Subtitle i opcjonalne załączniki nośnika, który może być prezentowana na ekranie blokady jako transparent u góry urządzenia lub w Centrum powiadomień.

Niezależnie od tego, gdzie w systemie iOS 10 zostanie wyświetlone powiadomienie użytkownika są prezentowane z tego samego wyglądu i działania i te same funkcje i funkcje.

W systemie iOS 8 Firma Apple wprowadziła można wykonać powiadomienia, gdy deweloper może dołączyć akcje niestandardowe powiadomienie i Zezwalaj użytkownikowi do wykonania akcji na powiadomienie bez konieczności uruchamiania aplikacji. W systemie iOS 9 Apple rozszerzone powiadomienia można wykonać przy użyciu szybką odpowiedź, który umożliwia użytkownikowi Odpowiedz na powiadomienie dotyczące wprowadzania tekstu.

Ponieważ powiadomienia użytkownika są bardziej integralną częścią środowisko użytkownika w systemie iOS 10, Apple ma rozwinąć, można wykonać powiadomienia do obsługi technologii 3D Touch, gdy użytkownik naciśnie na powiadomienie i niestandardowego interfejsu użytkownika jest wyświetlany obraz do zapewnienia sformatowanego interakcji powiadomienia.

Po wyświetleniu niestandardowego interfejsu użytkownika powiadomienie użytkownika, jeśli użytkownik wchodzi w interakcję z wszystkie akcje dołączony do powiadomienia, niestandardowego interfejsu użytkownika można natychmiast zaktualizować wyrazić swoją opinię określające, co zostało zmienione.

Nowość w systemach iOS 10, interfejs API interfejsu użytkownika powiadomienie użytkownika umożliwia aplikacji platformy Xamarin.iOS można łatwo korzystać z nowych funkcji interfejsu użytkownika powiadomienia użytkownika.

## <a name="adding-media-attachments"></a>Dodawanie załączników nośnika

Jeden z elementów częściej, które pobrania udostępniane między użytkownikami jest zdjęć, więc iOS 10 dodano możliwość dołączenia elementu nośnika (na przykład zdjęcie) bezpośrednio do powiadomienia, gdy będzie przedstawioną i łatwo dostępne dla użytkownika wraz z resztą num powiadomienia NT.

Jednak ze względu na rozmiary związanych z wysyłaniem nawet mały obraz, dołączeniu go do zdalnego ładunek powiadomienia staje się niepraktyczne. Aby obsłużyć taką sytuację, deweloper służy nowe rozszerzenie usługi w systemie iOS 10 pobranie obrazu z innego źródła (na przykład CloudKit magazyn danych) i dołączenie go do zawartości powiadomienia, zanim zostanie wyświetlony dla użytkownika.

Dla zdalnego powiadomienie ma być zmodyfikowana przez rozszerzenie usługi jego ładunku muszą być oznaczone jako modyfikowalne. Na przykład:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

Spójrz na poniższe przebieg procesu:

[![](advanced-user-notifications-images/extension02.png "Dodawanie załączników nośnika procesu")](advanced-user-notifications-images/extension02.png#lightbox)

Po zdalnego powiadomień jest dostarczane do urządzenia (za pośrednictwem usługi APN), rozszerzenie usługi może pobierać wymagane obrazu za pomocą jakichkolwiek metod żądanego (takie jak `NSURLSession`) i po otrzymaniu obrazu można modyfikować zawartość powiadomień i wyświetlania go użytkownikowi.

Oto przykład tego procesu może być obsługi w kodzie:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyNotification
{
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Constructors
        public NotificationService ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            // Get file URL
            var attachementPath = request.Content.UserInfo.ObjectForKey (new NSString ("my-attachment"));
            var url = new NSUrl (attachementPath.ToString ());

            // Download the file
            var localURL = new NSUrl ("PathToLocalCopy");

            // Create attachment
            var attachmentID = "image";
            var options = new UNNotificationAttachmentOptions ();
            NSError err;
            var attachment = UNNotificationAttachment.FromIdentifier (attachmentID, localURL, options , out err);

            // Modify contents
            var content = request.Content.MutableCopy() as UNMutableNotificationContent;
            content.Attachments = new UNNotificationAttachment [] { attachment };

            // Display notification
            contentHandler (content);
        }

        public override void TimeWillExpire ()
        {
            // Handle service timing out
        }
        #endregion
    }
}
```

Po otrzymaniu powiadomienia z APNs adresu niestandardowego obrazu jest odczytywana zawartość i plik jest pobierany z serwera. Następnie `UNNotificationAttachement` jest tworzony unikatowy identyfikator i lokalnej lokalizacji obrazu (jako `NSUrl`). Utworzono modyfikowalną kopię zawartości powiadomienia i załączniki nośnika są dodawane. Na koniec zostanie wyświetlone powiadomienie do użytkownika, wywołując `contentHandler`.

Po dodaniu powiadomienie załącznika systemu przejmuje przepływu i zarządzania pliku.

Oprócz zdalnego powiadomień przedstawionych powyżej, załączniki nośnika również są obsługiwane z poziomu lokalnego powiadomienia, gdy `UNNotificationAttachement` jest tworzony i dołączona do powiadomień wraz z jego zawartością.

Powiadomienie w systemie iOS 10 obsługuje załączników nośnika obrazów (statyczne i pliki GIF), audio i wideo i system zostanie automatycznie wyświetlona poprawne niestandardowego interfejsu użytkownika dla każdego z tych typów załączników przedstawionej powiadomienie do użytkownika.

> [!NOTE]
> Należy zachować ostrożność optymalizacji rozmiaru nośnika i czas wymagany do pobrania nośnika z serwera zdalnego (lub utworzyć nośnik lokalnego powiadomienia o) jako system nakłada strict limity zarówno podczas uruchamiania rozszerzenia usługi aplikacji. Rozważmy na przykład wysyłanie skalowany w dół wersję obrazu lub niewielki rozmiar klipu wideo przedstawiane w powiadomienia.

## <a name="creating-custom-user-interfaces"></a>Tworzenie niestandardowych interfejsów użytkownika

Aby utworzyć niestandardowego interfejsu użytkownika dla jego powiadomienia użytkownika, deweloper musi dodać powiadomień zawartości rozszerzenie (nowe w systemach iOS 10) do rozwiązanie aplikacji.

Rozszerzenie zawartość powiadomień umożliwia deweloperowi dodać własne widoki do interfejsu użytkownika powiadomienie i wyodrębnić zawartość, które mają. Jednak są interaktywne widżetów interfejsu użytkownika (na przykład przycisków i pola tekstowe) **nie** obsługiwane przez interfejs użytkownika powiadomienie i nigdy nie powinien zostać dodany do projektu niestandardowego interfejsu użytkownika.

Do obsługi interakcji z użytkownikiem z powiadomienie użytkownika, akcje niestandardowe powinny być utworzone, zarejestrowany w systemie i dołączyć do powiadomienia przed zaplanowaną w systemie. Do obsługi przetwarzania tych działań zostanie wywołana rozszerzenia zawartości powiadomienia. Zobacz [Praca z działań powiadomień](~/ios/platform/user-notifications/enhanced-user-notifications.md) sekcji [rozszerzone powiadomienia użytkownika](~/ios/platform/user-notifications/enhanced-user-notifications.md) dokumentu, aby uzyskać więcej informacji dotyczących działań niestandardowych.

Gdy jest wyświetlone powiadomienie użytkownika o niestandardowego interfejsu użytkownika, będzie mieć następujące elementy:

[![](advanced-user-notifications-images/customui01.png "Powiadomienie użytkownika o elementy niestandardowego interfejsu użytkownika")](advanced-user-notifications-images/customui01.png#lightbox)

Jeśli użytkownik wchodzi w interakcję z akcje niestandardowe (przedstawione poniżej powiadomienie), interfejs użytkownika może zostać zaktualizowana, aby przekazać opinię użytkownika jako co się stało po wywołaniu one danej akcji.

### <a name="adding-a-notification-content-extension"></a>Dodawanie powiadomień rozszerzeniu zawartości

Aby zaimplementować interfejs użytkownika powiadomienie użytkownika niestandardowego w aplikacji platformy Xamarin.iOS, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otwórz rozwiązanie aplikacji w programie Visual Studio dla komputerów Mac.
2. Kliknij prawym przyciskiem myszy nazwę rozwiązania w **konsoli rozwiązania** i wybierz **Dodaj** > **Dodawanie nowego projektu**.
3. Wybierz **iOS** > **rozszerzenia** > **rozszerzenia zawartości powiadomienia** i kliknij przycisk **dalej** przycisk: 

    [![](advanced-user-notifications-images/notify01.png "Wybierz rozszerzenia zawartości powiadomień")](advanced-user-notifications-images/notify01.png#lightbox)
4. Wprowadź **nazwa** rozszerzenia i kliknij **dalej** przycisk: 

    [![](advanced-user-notifications-images/notify02.png "Wprowadź nazwę rozszerzenia")](advanced-user-notifications-images/notify02.png#lightbox)
5. Dostosuj **Nazwa projektu** i/lub **Nazwa rozwiązania** wymagane, a następnie kliknij przycisk **Utwórz** przycisk: 

    [![](advanced-user-notifications-images/notify03.png "Zmień nazwę projektu i/lub nazwa rozwiązania")](advanced-user-notifications-images/notify03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Otwórz rozwiązanie aplikacji w programie Visual Studio dla komputerów Mac.
2. Kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksploratora rozwiązań** i wybierz **Dodaj > Nowy projekt...** .
3. Wybierz **Visual C# > rozszerzenia systemu iOS > rozszerzenia zawartości powiadomienia**:

    [![](advanced-user-notifications-images/notify01.w157-sml.png "Wybierz rozszerzenia zawartości powiadomień")](advanced-user-notifications-images/notify01.w157.png#lightbox)
4. Wprowadź **nazwa** rozszerzenia i kliknij **OK** przycisku.

-----

Po rozszerzeniu zawartości powiadomień zostanie dodany do rozwiązania, zostaną utworzone trzy pliki w projekcie rozszerzenia:

1. `NotificationViewController.cs` — Jest to widok główny kontroler rozszerzenia zawartości powiadomienia.
2. `MainInterface.storyboard` — W przypadku gdy dewelopera wychodzi poza widoczne interfejsu użytkownika dla rozszerzenia zawartości powiadomienia w systemie iOS projektanta.
3. `Info.plist` -Steruje konfiguracji rozszerzenia zawartości powiadomienia.

Wartość domyślna `NotificationViewController.cs` pliku wygląda podobnie do następującej:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

        }
        #endregion
    }
}
```

`DidReceiveNotification` Metoda jest wywoływana po rozwinięciu powiadomienia przez użytkownika, dzięki czemu powiadomienia rozszerzenia zawartości można wypełnić niestandardowego interfejsu użytkownika z zawartością `UNNotification`. Na przykład powyżej, etykiety został dodany do widoku ujawniony dla kodu o nazwie `label` i służy do wyświetlania treść powiadomienia.

### <a name="setting-the-notification-content-extensions-categories"></a>Ustawienie kategorii rozszerzenia zawartości powiadomień

System musi o sposobach znajdowania na specyficznych kategorii, który odpowiada na podstawie rozszerzenia zawartości powiadomienie aplikacji. Wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Kliknij dwukrotnie rozszerzenia `Info.plist` w pliku **konsoli rozwiązania** go otworzyć do edycji.
2. Przełącz się do **źródła** widoku.
3. Rozwiń węzeł `NSExtension` klucza.
4. Dodaj `UNNotificationExtensionCategory` klucza jako typ **ciąg** z wartością rozszerzenia należy do kategorii (w tym przykładzie "zaproszenia zdarzeń): 

    [![](advanced-user-notifications-images/customui02.png "Dodaj klucz UNNotificationExtensionCategory")](advanced-user-notifications-images/customui02.png#lightbox)
5. Zapisz zmiany.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij dwukrotnie rozszerzenia `Info.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
3. Rozwiń węzeł `NSExtension` klucza.
4. Dodaj `UNNotificationExtensionCategory` klucza jako typ **ciąg** z wartością rozszerzenia należy do kategorii (w tym przykładzie "zaproszenia zdarzeń): 

    [![](advanced-user-notifications-images/customui02w.png "Dodaj klucz UNNotificationExtensionCategory")](advanced-user-notifications-images/customui02w.png#lightbox)
5. Zapisz zmiany.

-----

Kategorie rozszerzenia zawartości powiadomienia (`UNNotificationExtensionCategory`) Użyj tej samej wartości kategorii, które są używane do rejestrowania działań powiadomień. W sytuacji, w której aplikacja będzie używać tego samego interfejsu użytkownika dla wielu kategorii, Przełącz `UNNotificationExtensionCategory` typowi **tablicy** i udostępniają wszystkie kategorie wymagane. Na przykład:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](advanced-user-notifications-images/customui03.png "Kategorie zawartości rozszerzenia powiadomień")](advanced-user-notifications-images/customui03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui03w.png "Kategorie zawartości rozszerzenia powiadomień")](advanced-user-notifications-images/customui03w.png#lightbox)

-----

### <a name="hiding-the-default-notification-content"></a>Ukrywanie domyślnej zawartości powiadomień

W sytuacji, gdy niestandardowego UI powiadomień będą wyświetlane tę samą zawartość jako domyślny powiadomienia (tytuł, Subtitle i treści automatycznie wyświetlane w dolnej części interfejsu użytkownika powiadomienie), to domyślne informacje mogą być ukryte przez dodanie `UNNotificationExtensionDefaultContentHidden`klucza `NSExtensionAttributes` klucza jako typ **logiczna** o wartości `YES` do rozszerzenia `Info.plist` pliku:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](advanced-user-notifications-images/customui04.png "Znajdowanie informacji domyślnych")](advanced-user-notifications-images/customui04.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui04w.png "Znajdowanie informacji domyślnych")](advanced-user-notifications-images/customui04w.png#lightbox)

-----

### <a name="designing-the-custom-ui"></a>Projektowanie niestandardowego interfejsu użytkownika

Projektowanie rozszerzenia zawartości powiadomienia niestandardowego interfejsu użytkownika, kliknij dwukrotnie `MainInterface.storyboard` plik, aby otworzyć do edycji w systemie iOS projektanta, przeciągnij elementy, które należy utworzyć żądanego interfejsu (takich jak `UILabels` i `UIImageViews`).

> [!NOTE]
> Interfejs użytkownika powiadomienie jest _nie_ obsługuje interaktywnych kontrolek, takie jak przyciski lub pól tekstowych w rozszerzeniu zawartości powiadomienia. Gdy będzie możliwe ich dodanie do scenorysu, użytkownik nie będzie obsługiwać interakcję z nimi. Aby dodać interakcji z użytkownikiem do niestandardowego interfejsu użytkownika powiadomienia, należy użyć akcji niestandardowych.

Po Interfejsie użytkownika została zaprojektowana i niezbędnych opcji ujawniony dla kodu C#, otwórz `NotificationViewController.cs` do edycji i zmodyfikować `DidReceiveNotification` metody do wypełnienia interfejsu użytkownika, gdy użytkownik rozwija powiadomienia. Na przykład:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

        }
        #endregion
    }
}
```

### <a name="setting-the-content-area-size"></a>Ustawianie rozmiaru obszaru zawartości

Aby dostosować rozmiar obszaru zawartości wyświetlany użytkownikowi, poniższy kod jest ustawienie `PreferredContentSize` właściwości w `ViewDidLoad` metody do żądanego rozmiaru. Ten rozmiar można również należy dostosować przez zastosowanie ograniczeń do widoku w Projektancie z systemem iOS, jest pozostawiany bez do deweloperów wybierz metodę, która najlepiej odpowiada je.

Ponieważ powiadomień systemu jest już uruchomiony przed zgłoszeniem zawartości rozszerzenie zostanie wywołany, obszar zawartości rozpocznie pełnej o rozmiarze i można animować w dół żądany rozmiar, gdy użytkownik widzi.

Aby wyeliminować ten efekt, należy edytować `Info.plist` pliku dla rozszerzenia i zestaw `UNNotificationExtensionInitialContentSizeRatio` klucza z `NSExtensionAttributes` typ **numer** o żądanej stosunek wartości. Na przykład:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](advanced-user-notifications-images/customui05.png "Klucz UNNotificationExtensionInitialContentSizeRatio")](advanced-user-notifications-images/customui05.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui05w.png "Klucz UNNotificationExtensionInitialContentSizeRatio")](advanced-user-notifications-images/customui05w.png#lightbox)

-----

### <a name="using-media-attachments-in-custom-ui"></a>Przy użyciu nośnika załączników w niestandardowego interfejsu użytkownika

Ponieważ załączniki nośnika (taki jak pokazano w [dodawanie załączników nośnika](#Adding-Media-Attachments) powyższej sekcji) są częścią ładunku powiadomienia, mogą być dostępne i wyświetlane w rozszerzeniu zawartości powiadomień, tak samo, jak będą one w domyślnym Powiadomienie interfejsu użytkownika.

Na przykład, jeśli włączone niestandardowego interfejsu użytkownika powyżej `UIImageView` który został ujawniony dla kodu C#, następujący kod może być używany do wypełnić ją z nośnika załącznika:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;

namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }
        #endregion
    }
}
```

Ponieważ załącznika nośnika jest zarządzany przez system, znajduje się poza aplikacji piaskownicy. Rozszerzenie musi zawiadomić system potrzebuje dostępu do pliku, wywołując `StartAccessingSecurityScopedResource` metody. Po zakończeniu rozszerzenia przy użyciu pliku musi wywołać `StopAccessingSecurityScopedResource` zwolnienia połączenia.

### <a name="adding-custom-actions-to-a-custom-ui"></a>Dodawanie działań niestandardowych do niestandardowego interfejsu użytkownika

Jak już wspomniano, interfejsu użytkownika powiadomienia nie obsługuje interakcji z użytkownikiem, więc formanty, takie jak przyciski lub pola tekstowe nie są obsługiwane w projekcie interfejsu użytkownika.

Zamiast tego mechanizmu standardowe akcje niestandardowe używany do zwiększenia interakcji do niestandardowego interfejsu użytkownika powiadomienie. Zobacz [Praca z działań powiadomień](~/ios/platform/user-notifications/enhanced-user-notifications.md) sekcji [rozszerzone powiadomienia użytkownika](~/ios/platform/user-notifications/enhanced-user-notifications.md) dokumentu, aby uzyskać więcej informacji dotyczących działań niestandardowych.

Oprócz akcje niestandardowe rozszerzenie zawartości powiadomienia mogą odpowiadać na również następujące akcje wbudowane:

- **Akcja domyślna** — jest to, gdy użytkownik naciska powiadomienie, aby otworzyć aplikację i wyświetlić szczegóły danego powiadomienia.
- **Odrzuć akcji** — ta akcja jest wysyłane do aplikacji, gdy użytkownik odrzuci powiadomienie o danym.

Rozszerzenia zawartości powiadomienia mają również możliwość aktualizowania ich interfejsu użytkownika, gdy użytkownik wywoła jedną z akcji niestandardowych, takich jak wyświetlanie daty jako zaakceptowane, gdy użytkownik naciska **Akceptuj** przycisku akcji niestandardowej. Ponadto rozszerzenia zawartości powiadomień może poinformować system opóźnienia zwolnienia powiadomień interfejsu użytkownika, więc użytkownik może zobaczyć efekt ich działania, przed zamknięciem powiadomienia.

W tym celu implementowania druga wersja `DidReceiveNotification` metodę, która obejmuje obsługi uzupełniania. Na przykład:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;
using CoreGraphics;

namespace myApp {
    public class NotificationViewController : UIViewController, UNNotificationContentExtension {

        public override void ViewDidLoad() {
            base.ViewDidLoad();

            // Adjust the size of the content area
            var size = View.Bounds.Size
            PreferredContentSize = new CGSize(size.Width, size.Width/2);
        }

        public void DidReceiveNotification(UNNotification notification) {

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = Content.UserInfo["location"] as string;
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments[0];
                if (attachment.Url.StartAccessingSecurityScopedResource()) {
                    EventImage.Image = UIImage.FromFile(attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch(response.ActionIdentifier){
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
    }
}
```

Dodając `Server.PostEventResponse` program obsługi `DidReceiveNotification` — metoda rozszerzenia zawartości powiadomienia, rozszerzenie *musi* obsłużyć wszystkie akcje niestandardowe. Rozszerzenia mogą być przekazywane dalej akcje niestandardowe do aplikacji zawierającego zmieniając `UNNotificationContentExtensionResponseOption`. Na przykład:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>Praca z akcji wejściowego tekstu w niestandardowego interfejsu użytkownika

W zależności od projektu aplikacji i powiadomienie może być razy, które wymagają od użytkownika wprowadzenia tekstu do powiadomienia (na przykład odpowiadaniu na wiadomość). Rozszerzenie zawartości powiadomienie ma dostęp do akcji wejścia tekst, tak samo, jak jest powiadomień w wersji standard.

Na przykład:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Computed Properties
        // Allow to take input
        public override bool CanBecomeFirstResponder {
            get { return true; }
        }

        // Return the custom created text input view with the
        // required buttons and return here
        public override UIView InputAccessoryView {
            get { return InputView; }
        }
        #endregion

        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Private Methods
        private UNNotificationCategory MakeExtensionCategory ()
        {

            // Create Accept Action
            ...

            // Create decline Action
            ...

            // Create Text Input Action
            var commentID = "comment";
            var commentTitle = "Comment";
            var textInputButtonTitle = "Send";
            var textInputPlaceholder = "Enter comment here...";
            var commentAction = UNTextInputNotificationAction.FromIdentifier (commentID, commentTitle, UNNotificationActionOptions.None, textInputButtonTitle, textInputPlaceholder);

            // Create category
            var categoryID = "event-invite";
            var actions = new UNNotificationAction [] { acceptAction, declineAction, commentAction };
            var intentIDs = new string [] { };
            var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);

            // Return new category
            return category;

        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Is text input?
            if (response is UNTextInputNotificationResponse) {
                var textResponse = response as UNTextInputNotificationResponse;
                Server.Send (textResponse.UserText, () => {
                    // Close Notification
                    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
                });
            }

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch (response.ActionIdentifier) {
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
        #endregion
    }
}
```

Ten kod tworzy nową akcję wejściowego tekstu i dodaje go do kategorii rozszerzenia (w `MakeExtensionCategory`) metody. W `DidReceive` zastąpić metodę, obsługuje on użytkownika wprowadzanie tekstu z następującym kodem:

```csharp
// Is text input?
if (response is UNTextInputNotificationResponse) {
    var textResponse = response as UNTextInputNotificationResponse;
    Server.Send (textResponse.UserText, () => {
        // Close Notification
        completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
    });
}
```

Jeśli projekt wymaga dodawanie przycisków niestandardowych do pola tekstowego danych wejściowych, Dodaj następujący kod, aby je uwzględnić:

```csharp
// Allow to take input
public override bool CanBecomeFirstResponder {
    get {return true;}
}

// Return the custom created text input view with the
// required buttons and return here
public override UIView InputAccessoryView {
    get {return InputView;}
}
```

Po wyzwoleniu komentarz akcji przez użytkownika zarówno kontroler widoku, jak i pole wprowadzania tekstu niestandardowego konieczne można aktywować:

```csharp
// Update UI when the user interacts with the
// Notification
Server.PostEventResponse += (response) {
    // Take action based on the response
    switch(response.ActionIdentifier){
    ...
    case "comment":
        BecomeFirstResponder();
        TextField.BecomeFirstResponder();
        break;
    }

    // Close Notification
    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);

};
```

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule trwało zaawansowane przyjrzeć się przy użyciu nowej struktury powiadomienie użytkownika w aplikacji platformy Xamarin.iOS. Omówione, dodawanie załączników nośnika do lokalnego i zdalnego powiadomień i objęte, tworzenie niestandardowego UI powiadomienia za pomocą nowego interfejsu użytkownika powiadomienie użytkownika.


## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Odwołanie UserNotifications Framework](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Podręcznik programowania powiadomień lokalnych i zdalnych](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
