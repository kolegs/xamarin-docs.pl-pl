---
title: Komunikat zaawansowanych aplikacji rozszerzenia
description: "W tym artykule przedstawiono zaawansowane techniki pracy z rozszerzeniami aplikacji wiadomości w rozwiązaniu Xamarin.iOS, która integruje się z aplikacji Messages i wyświetla użytkownikowi nowych funkcji."
ms.topic: article
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7e6621dc580e478873ce2db7139b04284bee355c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="advanced-message-app-extensions"></a>Komunikat zaawansowanych aplikacji rozszerzenia

_W tym artykule przedstawiono zaawansowane techniki pracy z rozszerzeniami aplikacji wiadomości w rozwiązaniu Xamarin.iOS, która integruje się z aplikacji Messages i wyświetla użytkownikowi nowych funkcji._


Nowe w systemach iOS 10, rozszerzenie aplikacji komunikat integruje się z **wiadomości** aplikacji i przedstawia nowych funkcji dla użytkownika. Rozszerzenie można wysyłać tekstu, nalepek, plików multimedialnych i interaktywne wiadomości.

## <a name="about-message-app-extensions"></a>Temat wiadomości rozszerzeń aplikacji

Jak już wspomniano, rozszerzenie aplikacji komunikat integruje się z **wiadomości** aplikacji i przedstawia nowych funkcji dla użytkownika. Rozszerzenie można wysyłać tekstu, nalepek, plików multimedialnych i interaktywne wiadomości. Dostępne są dwa typy rozszerzenia aplikacji komunikat:

- **Pakiety naklejce** — zawiera kolekcję nalepek, które użytkownik może dodać do komunikatu. Naklejce pakiety mogą być tworzone bez pisania żadnego kodu.
- **iMessage aplikacji** — może ona powodować niestandardowy interfejs użytkownika w aplikacji Messages na wybranie nalepek, wprowadzania tekstu, w tym plików multimedialnych (z konwersje typów opcjonalne) i tworzeniem, edytowaniem i wysyłanie komunikatów interakcji.

Rozszerzenia komunikatów aplikacji zawierają trzy typy zawartości:

- **Interakcyjne wiadomości** -są typ zawartości niestandardowy komunikat, który generuje aplikacji, po naciśnięciu w komunikacie, aplikacja zostanie uruchomiona na pierwszym planie.
- **Nalepek** — obrazy generowane przez aplikację, która może być uwzględniony w wiadomości między użytkownikami. Zobacz nasze [konstruktora lodów](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) Przykładowa aplikacja dla przykładem implementacji naklejce pakietu aplikacji.
- **Inne obsługiwane zawartości** -aplikacji można udostępnić zawartość, takie jak zdjęcia, wideo, tekst lub łącza typu, który ma zawsze obsługiwane przez aplikację wiadomości.

Nowość w systemach iOS 10, komunikat aplikacji zawiera teraz własnego Sklepu z aplikacjami dedykowanym, wbudowane. Wszystkie aplikacje, które obejmują rozszerzenia komunikatów aplikacji są wyświetlane i wspierane w tym magazynie. Szufladzie aplikacji komunikaty będą wyświetlane wszystkie aplikacje, które zostały pobrane ze sklepu z aplikacjami wiadomości co zapewnia szybki dostęp do użytkowników.

Ponadto nowy w systemie iOS 10, Apple został dodany wbudowanego autorstwa aplikacji, dzięki czemu można łatwo wykryć aplikacji. Na przykład jeśli jeden użytkownik wysyła zawartość do innego z aplikacji, która 2 użytkownik nie ma zainstalowany (na przykład naklejce na przykład), nazwę aplikacji wysyłania jest wyświetlony w obszarze zawartości w historii wiadomości. Jeśli użytkownik naciska aplikacji nazw, firma Microsoft można otworzyć magazynu aplikacji oraz aplikacji wybrane w magazynie.

Rozszerzenia komunikatów aplikacji są podobne do istniejących aplikacji systemu iOS, że dewelopera jest znana tworzenie i dostęp do wszystkich platform standardowe i funkcje aplikacji iOS standardowe. Na przykład:

- Mają dostęp do zakupu w aplikacji.
- Mają dostęp do Apple Pay.
- Mają dostęp do urządzeń sprzętowych, takich jak aparatu.

Rozszerzenia komunikatów aplikacji są obsługiwane tylko w systemie iOS 10, jednak zawartość, która wysyłać te rozszerzenia są widoczne na urządzeniach watchOS i macOS. Nowy _strony ostatnich_ dodane do watchOS 3, wyświetlać ostatnie naklejek zostały wysłane w telefonie, łącznie z tymi z rozszerzeń aplikacji wiadomości i Zezwalaj użytkownikowi na wysyłanie tych nalepek z czujki.

## <a name="about-interactive-messages"></a>Temat wiadomości interakcyjne

Interakcyjne komunikaty stanowią bąbelków komunikat niestandardowy i są dostarczane przez rozszerzenie aplikacji wiadomości. Zezwalaj użytkownikowi na tworzenie interaktywnych komunikat zawartości, ich Wstaw go w polu danych wejściowych komunikat i wysłać go.

[ ![](advanced-message-app-extensions-images/interactive01.png "Tworzenie zawartości wiadomości interakcyjne")](advanced-message-app-extensions-images/interactive01.png)

Odbieranie użytkownika można odpowiedzi na wiadomość interaktywne, naciskając jego bąbelków komunikat w historii wiadomości, aby załadować rozszerzenia aplikacji komunikat, który go utworzył. Rozszerzenie zostanie uruchomiona pełnego ekranu i umożliwia użytkownikowi utworzenie odpowiedzi i wysłać go użytkownikowi pochodzące.

[ ![](advanced-message-app-extensions-images/interactive02.png "Rozszerzenie uruchomić pełnego ekranu")](advanced-message-app-extensions-images/interactive02.png)


Poniższe tematy zostanie omówiona szczegółowo poniżej:

- Przegląd interfejsu API wiadomości
- Cykl życia rozszerzenia
- W wiadomości
- Wysyłanie wiadomości

## <a name="messages-api-overview"></a>Przegląd interfejsu API wiadomości

Po wywołaniu przez użytkownika, rozszerzenie aplikacji komunikat zostanie wyświetlony w dolnej części historii wiadomości w trybie compact widoku:

[ ![](advanced-message-app-extensions-images/interactive03.png "Przegląd interfejsu API wiadomości")](advanced-message-app-extensions-images/interactive03.png)

1. `MSMessageAppViewController` Obiektu w rozszerzeniu aplikacji wiadomości jest klasy głównym, która jest wywoływane, gdy zostanie wyświetlony widok rozszerzenia dla użytkownika.
2. Konwersacji jest wyświetlana jako `MSConversation` wystąpienie obiektu.
3. `MSMessage` Klasa reprezentuje danego bąbelków komunikat w konwersacji.
4. `MSSession` Określa, jak jest wysyłany komunikat.
5. `MSMessageTemplateLayout` Określa sposób wyświetlania komunikatu

## <a name="the-extension-lifecycle"></a>Cykl życia rozszerzenia

Spójrz na proces aktywację rozszerzenie aplikacji komunikat:

[ ![](advanced-message-app-extensions-images/interactive04.png "Proces aktywację rozszerzenie aplikacji wiadomości")](advanced-message-app-extensions-images/interactive04.png)

1. Gdy rozszerzenie jest uruchamiana (na przykład z szuflady aplikacji), aplikacji komunikat spowoduje uruchomienie procesu.
2. `DidBecomeActive` Metoda jest wywoływana i przekazany `MSConversation` reprezentujący komunikat rozszerzenia aplikacji działającej w konwersacji.
3. Ponieważ rozszerzenia jest oparta na off z `UIViewController` zarówno `ViewWillAppear` i `ViewDidAppear` są nazywane.

Następnie spójrz na proces staje się dezaktywowane rozszerzenie aplikacji komunikat:

[ ![](advanced-message-app-extensions-images/interactive05.png "Proces rozszerzenia aplikacji wiadomości, staje się dezaktywowany")](advanced-message-app-extensions-images/interactive05.png)

1. Jeśli rozszerzenie aplikacji wiadomości jest dezaktywowany, `ViewWillDisappear` najpierw wywołana metoda.
2. Następnie przy użyciu `ViewDidDisappear` metoda zostanie wywołana.
3. `WillResignActive` Metoda jest wywoływana i przekazany `MSConversation` reprezentujący komunikat rozszerzenia aplikacji działającej w konwersacji. W tym momencie połączenia między aplikacją wiadomości i rozszerzenia zostaną zwolnione.
4. W pewnym momencie nowsze proces jest zakończony przez aplikację wiadomości.

Ponieważ rozszerzenia jest procesem krótko, zostanie zakończony agresywnie przez system w celu zaoszczędzenia energii baterii i przetwarzania. Deweloper powinien pamiętać to podczas projektowania i implementacji rozszerzenia aplikacji wiadomości.

## <a name="composing-a-message"></a>W wiadomości

Po komunikat rozszerzenia aplikacji jest uruchomiony w ramach procesu i jest wyświetlany interfejs użytkownika, poniższy kod może służyć do tworzenia nowej wiadomości:

```csharp
MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
{
    var components = new NSUrlComponents {
        QueryItems = iceCream.QueryItems
    };

    var layout = new MSMessageTemplateLayout {
        Image = iceCream.RenderSticker (true),
        Caption = caption
    };

    var message = new MSMessage (session ?? new MSSession()) {
        Url = components.Url,
        Layout = layout
    };

    return message;
}
```

Ten kod tworzy nową `MSMessage` i ustawia kilka właściwości (takie jak `Url`). Gdy wiadomość można tworzyć tylko w systemie iOS, mogą być wysyłane zarówno dla systemu iOS, jak i system macOS mają być wyświetlane.

Gdy użytkownik kliknie bąbelków komunikat w konwersacji na macOS, Mac spróbuje otworzyć adresu określonego w adresie URL w przeglądarce sieci web. W związku z tym dewelopera witryny sieci Web powinno być możliwe, że niektóre reprezentację komunikatu w przeglądarce sieci web na macOS na podstawie maszyn.

`AccessibilityLabel` Właściwość jest używana przez czytników ekranu do odczytu zapisu konwersacji dla użytkownika. `Layout` Właściwość określa, jak zostanie wyświetlony komunikat, obecnie tylko `MSMessageTemplateLayout` jest obsługiwana i wygląda podobnie do następującej:

[ ![](advanced-message-app-extensions-images/interactive06.png "Szablon MSMessageTemplateLayout")](advanced-message-app-extensions-images/interactive06.png)

`Image` Właściwość `MSMessageTemplateLayout` udostępnia zawartość dla głównej części MessageBubble na ekranie. `MediaFileUrl` Właściwość również zapewnia zawartości dla treści wiadomości bąbelków, ale zezwala na zawartość, która nie jest obsługiwana przez `UIImage` (na przykład plik wideo, który będzie Pętla w tle). Jeśli oba `Image` i `MediaFileUrl` podano właściwości `Image` właściwość będzie mieć wyższy priorytet. `MediaFileUrl` Obsługuje PNG, JPEG, GIF i wideo (w formacie, który może zostać odtworzone przez platformę Media Player) formatów multimediów.

Rozmiar nośnika zalecane jest 300 x 300 pikseli przy rozdzielczości 3 x. Akceptowane są również zasoby nieco większych i mniejszych i Apple sugeruje testowanie za pomocą kilku różnych rozmiarach, aby uzyskać najlepsze wyniki. Komunikat aplikacji będzie próbki w dół i skalować tego nośnika zgodnie z wymaganiami.

Gdy zasoby są wysyłane do odbiornika, wszelkie nośniki dołączony zostanie automatycznie przekodowane przez aplikację wiadomości w celu zoptymalizowania je z transfer za pośrednictwem sieci. W związku z tym Apple odradza tym tekst w nośnika, który jest dołączony do wiadomości, ponieważ nośnik jest skalowany w dół i skompresowane transmisji, w związku z tym potencjalnie renderowanie tekstu nieczytelne.

`ImageTitle` i `ImageSubtitle` właściwości Podaj opis dla nośnika, które są prezentowane w bąbelków wiadomości. Te właściwości zostanie wysłany jako tekst na urządzenie odbierające których one będą crisply renderowane w dolnym rogu po lewej stronie obrazu.

`Caption`, `SubCaption`, `TrailingCaption` i `TrailingSubcaption` właściwości dodatkowo opisujący obraz i są wyświetlane w sekcji poniżej obrazu. Wszystkie te właściwości do ustawienia `null` utworzy bąbelkowy wiadomość bez obszaru podpis:

[ ![](advanced-message-app-extensions-images/interactive07.png "Bąbelkowy wiadomość bez obszaru podpisu")](advanced-message-app-extensions-images/interactive07.png)

Ostatnim etapem należy pamiętać, jest aplikacji Messages będzie rysowanie ikony wiadomości rozszerzenia aplikacji w górnym lewym dolnym rogu bąbelków wiadomości.

## <a name="sending-a-message"></a>Wysyłanie wiadomości

Raz `MSMessage` został następujący kod może być używany do wysyłania składa:

```csharp
public void SendMessage (MSMessage message)
{
    // Insert the message into the conversation
    ActiveConversation.InsertMessage (message, (error) => {
        // Did the message send successfully?
        if (error == null) {
            // Handle successful send
        } else {
            // Report Error
            Console.WriteLine ("Error: {0}", error);
        }
    };

}
```

`ActiveConversation` Właściwość `MSMessagesAppViewController` będą przechowywane bieżącej konwersacji uruchomione w wiadomości rozszerzenia aplikacji.

Wywołanie `InsertMessage` z `MSConversation` obejmują komunikat w konwersacji i wszelkie błędy, które mogą wystąpić. Jeśli pomyślnie dołączono wiadomości, bąbelków komunikat zostanie wyświetlony w polu danych wejściowych.

Ponadto rozszerzenia można wysyłać różne typy danych do konwersacji, takich jak:

- **Text** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **Załączniki** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **Nalepek**  -  `ActiveConversation.InsertSticker (sticker, (obj) => {...});` gdzie `sticker` jest `MSSticker`.

Po nowej zawartości znajduje się w polu dane wejściowe, użytkownik będzie mógł wysłać wiadomość, wybierając niebieski **wysyłania** przycisk (tak samo, jak dowolny typowy komunikat). Nie istnieje sposób rozszerzenia aplikacji komunikat do automatycznego wysyłania zawartości, proces ten jest całkowicie pod kontrolą użytkownika.

## <a name="handling-the-compact-and-expanded-modes"></a>Obsługa tryby Compact i rozszerzonej

Rozszerzenie aplikacji komunikat może być wyświetlany w jednym z dwóch trybów inny widok:

[ ![](advanced-message-app-extensions-images/interactive08.png "Rozszerzenie aplikacji komunikat wyświetlany w dwóch trybach inny widok: CD & rozwinięty")](advanced-message-app-extensions-images/interactive08.png)

- **Compact** — jest to domyślny tryb gdzie rozszerzenia aplikacji komunikat zajmuje 25% widoku wiadomości. Tryb kompaktowy aplikacji nie ma dostępu do klawiatury, przewijanie w poziomie lub aparaty rozpoznawania gestów Przejdź. Aplikacja ma dostęp do pola danych wejściowych i wywołań `InsertMessage` natychmiast będzie widoczny dla użytkownika istnieje.
- **Rozszerzona** — rozszerzenie aplikacji komunikat wypełnienia całego widoku komunikatów. Nie ma dostępu do pola danych wejściowych, ale ma dostępu do klawiatury, przewijanie w poziomie i aparatów rozpoznawania gestów Przejdź.

Należy natychmiast odbierać wszelkie zmiany w widoku tryb rozszerzenia aplikacji wiadomości i mogą być przełączane tych trybów programowo lub ręcznie przez użytkownika w dowolnym momencie.

Spójrz na poniższym przykładzie obsługa przełączanie między dwa tryby inny widok. Dwa różne kontrolerów widoku będzie wymagane dla każdego stanu. `StickerBrowserViewController` Dojść **Compact** widoku i `AddStickerViewController` obsłuży **rozwinięty** widoku:

```csharp
using System;

using Messages;
using Foundation;
using UIKit;

namespace MessagesExtension {
    public partial class MessagesViewController : MSMessagesAppViewController, IIceCreamsViewControllerDelegate, IBuildIceCreamViewControllerDelegate {
        public MessagesViewController (IntPtr handle) : base (handle)
        {
        }

        public override void WillBecomeActive (MSConversation conversation)
        {
            base.WillBecomeActive (conversation);

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, PresentationStyle);
        }

        public override void WillTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected an active converstation");

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, presentationStyle);
        }

        void PresentViewController (MSConversation conversation, MSMessagesAppPresentationStyle presentationStyle)
        {
            // Determine the controller to present.
            UIViewController controller;

            if (presentationStyle == MSMessagesAppPresentationStyle.Compact) {
                // Show a list of previously created ice creams.
                controller = InstantiateIceCreamsController ();
            } else {
                var iceCream = new IceCream (conversation.SelectedMessage);
                controller = iceCream.IsComplete ? InstantiateCompletedIceCreamController (iceCream) : InstantiateBuildIceCreamController (iceCream);
            }

            foreach (var child in ChildViewControllers) {
                child.WillMoveToParentViewController (null);
                child.View.RemoveFromSuperview ();
                child.RemoveFromParentViewController ();
            }

            AddChildViewController (controller);
            controller.View.Frame = View.Bounds;
            controller.View.TranslatesAutoresizingMaskIntoConstraints = false;
            View.AddSubview (controller.View);

            controller.View.LeftAnchor.ConstraintEqualTo (View.LeftAnchor).Active = true;
            controller.View.RightAnchor.ConstraintEqualTo (View.RightAnchor).Active = true;
            controller.View.TopAnchor.ConstraintEqualTo (View.TopAnchor).Active = true;
            controller.View.BottomAnchor.ConstraintEqualTo (View.BottomAnchor).Active = true;

            controller.DidMoveToParentViewController (this);
        }

        UIViewController InstantiateIceCreamsController ()
        {
            // Instantiate a `IceCreamsViewController` and present it.
            var controller = Storyboard.InstantiateViewController (IceCreamsViewController.StoryboardIdentifier) as IceCreamsViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an IceCreamsViewController from the storyboard");

            controller.Builder = this;
            return controller;
        }

        UIViewController InstantiateBuildIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (BuildIceCreamViewController.StoryboardIdentifier) as BuildIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an BuildIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            controller.Builder = this;

            return controller;
        }

        public UIViewController InstantiateCompletedIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (CompletedIceCreamViewController.StoryboardIdentifier) as CompletedIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an CompletedIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            return controller;
        }

        public void DidSelectAdd (IceCreamsViewController controller)
        {
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void Build (BuildIceCreamViewController controller, IceCreamPart iceCreamPart)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected a conversation");

            var iceCream = controller.IceCream;
            if (iceCream == null)
                throw new Exception ("Expected the controller to be displaying an ice cream");

            string messageCaption = string.Empty;
            var b = iceCreamPart as Base;
            var s = iceCreamPart as Scoops;
            var t = iceCreamPart as Topping;

            if (b != null) {
                iceCream.Base = b;
                messageCaption = "Let's build an ice cream";
            } else if (s != null) {
                iceCream.Scoops = s;
                messageCaption = "I added some scoops";
            } else if (t != null) {
                iceCream.Topping = t;
                messageCaption = "Our finished ice cream";
            } else {
                throw new Exception ("Unexpected type of ice cream part selected.");
            }

            // Create a new message with the same session as any currently selected message.
            var message = ComposeMessage (iceCream, messageCaption, conversation.SelectedMessage?.Session);

            // Add the message to the conversation.
            conversation.InsertMessage (message, null);

            // If the ice cream is complete, save it in the history.
            if (iceCream.IsComplete) {
                var history = IceCreamHistory.Load ();
                history.Append (iceCream);
                history.Save ();
            }

            Dismiss ();
        }

        MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
        {
            var components = new NSUrlComponents {
                QueryItems = iceCream.QueryItems
            };

            var layout = new MSMessageTemplateLayout {
                Image = iceCream.RenderSticker (true),
                Caption = caption
            };

            var message = new MSMessage (session ?? new MSSession()) {
                Url = components.Url,
                Layout = layout
            };

            return message;
        }
    }
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

Opcjonalnie można mają korzystać z aplikacji `WillTransition` metodę, aby obsłużyć zmiana trybu widoku, zanim jest proponowane użytkownikowi (co jest wykonywane w powyższym przykładzie konstruktora Icecream). Aby uzyskać więcej informacji, zobacz nasze [dalsze dostosowanie naklejce](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) dokumentacji.

## <a name="replying-to-a-message"></a>Odpowiadaniu na wiadomość

Istnieją dwa przypadków, które musi obsłużyć przy odpowiadaniu na wiadomość rozszerzenia aplikacji komunikat:

[ ![](advanced-message-app-extensions-images/interactive09.png "Rozszerzenie aplikacji wiadomości w trybach nieaktywny i aktywne")](advanced-message-app-extensions-images/interactive09.png)

- **Rozszerzenie jest nieaktywna** — istnieje bąbelków komunikat rozszerzenia aplikacji wiadomości w wykazie komunikat, który użytkownik może nacisnąć do aktywowania rozszerzeń i kontynuować interakcyjne konwersacji.
- **Rozszerzenie jest aktywny** — użytkownik może nacisnąć bąbelków komunikat rozszerzenia aplikacji wiadomości w wykazie komunikatów do trybu widoku rozwinięty i kontynuować proces interaktywny, z którym ją przerwał.

### <a name="the-extension-is-inactive"></a>Rozszerzenie jest nieaktywny.

Gdy bąbelków wiadomości jest dotknięciu przez użytkownika w wykazie wiadomości i rozszerzenie aplikacji wiadomości jest nieaktywny, odbędzie się następujący proces:

[ ![](advanced-message-app-extensions-images/interactive10.png "Obsługa nieaktywne bąbelków wiadomości")](advanced-message-app-extensions-images/interactive10.png)

1. Użytkownik naciska bąbelków komunikat rozszerzenia.
2. Po uruchomieniu rozszerzenie, komunikat aplikacji spowoduje uruchomienie procesu.
3. `DidBecomeActive` Metoda jest wywoływana i przekazany `MSConversation` reprezentujący komunikat rozszerzenia aplikacji działającej w konwersacji.
4. Ponieważ rozszerzenia jest oparta na off z `UIViewController` zarówno `ViewWillAppear` i `ViewDidAppear` są nazywane.

Po ukończeniu procesu rozszerzenia aplikacji komunikat zostanie wyświetlone w trybie widoku rozwinięty.

### <a name="the-extension-is-active"></a>Rozszerzenie jest aktywny

Gdy bąbelków wiadomości jest dotknięciu przez użytkownika w wykazie wiadomości i rozszerzenie aplikacji wiadomości jest aktywny, odbędzie się następujący proces:

[ ![](advanced-message-app-extensions-images/interactive11.png "Obsługa active bąbelków wiadomości")](advanced-message-app-extensions-images/interactive11.png)

1. Użytkownik naciska bąbelków komunikat rozszerzenia.
2. Ponieważ rozszerzenia aplikacji wiadomości jest już aktywny, `WillTransition` metoda `MSMessagesAppViewController` jest wywoływana w celu obsługi kompaktowania przełączania do trybu wyświetlania rozwinięty.
3. `DidSelectMessage` Metody `MSMessagesAppViewController` jest nazywane i przekazany `MSMessage` i `MSConversation` należącą do bąbelków wiadomości.
4. `DidTransition` Metoda `MSMessagesAppViewController` jest wywoływana w celu obsługi kompaktowania przełączania do trybu wyświetlania rozwinięty.

Ponownie po ukończeniu procesu rozszerzenia aplikacji komunikat zostanie wyświetlone w trybie widoku rozwinięty.

### <a name="accessing-the-selected-message"></a>Uzyskiwanie dostępu do wybranego komunikatu

W obu przypadkach po naciśnięciu bąbelkowy komunikat należących do rozszerzenia aplikacji wiadomości, należy uzyskać dostęp do `MSMessage` który został dotknięciu przy użyciu `SelectedMessage` właściwość `MSConversation`.

Na przykład:

```csharp
using System;
using Foundation;
using Messages;
using UIKit;


namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        ...

        #region Override Methods
        public override void DidSelectMessage (MSMessage message, MSConversation conversation)
        {
            base.DidSelectMessage (message, conversation);

            // Get selected message
            var selected = conversation.SelectedMessage;

            // Present the user interface to continue editing
            // the conversation
            ...
        }
        #endregion
    }
}
```

Wybranego komunikatu mają być wyświetlane w interfejsie użytkownika rozszerzenia aplikacji komunikat, a użytkownik powinien mieć możliwość tworzenia odpowiedzi.

## <a name="removing-partially-completed-messages"></a>Usuwanie częściowo ukończone wiadomości

W trakcie wysyłania wykonania różnych kroków interakcyjne konwersację między dwiema użytkownika w konwersacji, częściowo ukończone dymki wiadomości, można zacząć zajmowały wykaz komunikat:

[ ![](advanced-message-app-extensions-images/interactive12.png "Częściowo ukończone dymki komunikat może przeładowania wykaz wiadomości")](advanced-message-app-extensions-images/interactive12.png)

Zamiast tego rozszerzenia aplikacji komunikat powinien zwijane poprzedniej dymki wiadomości zwięzły komentarz w wykazie komunikat:

[ ![](advanced-message-app-extensions-images/interactive13.png "Zwijanie poprzedniej dymki wiadomości w wykazie wiadomości")](advanced-message-app-extensions-images/interactive13.png)

Jest to obsługiwane przy użyciu `MSSession` Aby zwinąć wszystkich istniejących kroków. Dlatego `DidSelectMessage` metody `MSMessagesAppViewController` klasy mogą być modyfikowane w wyglądać następująco:

```csharp
public override void DidSelectMessage (MSMessage message, MSConversation conversation)
{
    base.DidSelectMessage (message, conversation);

    MSSession session;
    var summary = "";

    // Get selected message
    var selected = conversation.SelectedMessage;

    // Does the selected message already have a session?
    if (selected.Session == null) {
        // No, create a new session
        session = new MSSession ();
        summary = "Let's build an ice cream";
    } else {
        // Yes, use the existing session
        session = selected.Session;
        summary = "I added some scoops";
    }

    // Create an instance of the message being composed
    var existingMessage = new MSMessage (session);
    message.SummaryText = summary;
    ...

    // Present the user interface to continue editing
    // the conversation
    ...
}
```

Jeśli już wybranego komunikatu Kończenie `MSSession`, jest używany else nowy `MSSession` jest tworzony. `SummaryText` Właściwość `MSMessage` służy do dodawania podpis do zwinięte poprzednie kroki. Jeśli `SummaryText` właściwość jest ustawiona na `null`, poprzednie kroki w konwersacji zostaną całkowicie usunięte z wykaz konwersacji.

## <a name="advanced-message-api-features"></a>Zaawansowane funkcje interfejsu API wiadomości

Z podstawowymi funkcjami nowy interfejs API komunikat omówiona szczegółowo powyżej następnie sprawdź, czy niektórych bardziej zaawansowanych funkcji, które Apple ma wbudowane w platformę.

Po pierwsze, istnieje kilka innych metod zastąpienia w `MSMessagesAppViewController` klasy, która zapewnić lepszy dostęp do konwersacji:

- `DidStartSendingMessage` — Jest to po naciśnięciu przycisku Wyślij. Nie oznacza komunikat był on dostarczania do adresata, tylko uruchomienia procesu wysyłania.
- `DidCancelSendingMessage` -Dzieje się tak po naciśnięciu *X* przycisk w prawym górnym rogu bąbelków komunikat w wykazie konwersacji.
- `DidReceiveMessage` — Ta metoda jest wywoływana, gdy rozszerzenie aplikacji wiadomości jest aktywny nową wiadomość została odebrana jeden z uczestników konwersacji.

### <a name="group-conversations"></a>Grupa konwersacji

Rozszerzenie aplikacji wiadomości mogą być używane, a użytkownicy są zaangażowane w konwersacji grup (z osobami 3 lub więcej) i ten będzie musiał należy wziąć pod uwagę podczas projektowania i implementacji rozszerzenia aplikacji wiadomości.

Spójrz na poniższe interakcji grupy konwersacji z trzech użytkowników:

[ ![](advanced-message-app-extensions-images/interactive14.png "Interakcja grupy konwersacji z trzech użytkowników")](advanced-message-app-extensions-images/interactive14.png)

1. Użytkownika 1 wysyła grupę komunikat interakcyjne pytania użytkownika 2 i 3 użytkownika, aby wybrać nadmiarów burgera.
2. Użytkownik 2 wybiera pomidorów.
3. Użytkownik 3 wybierze czekoladę.
4. Opcje zarówno użytkownika 2 i 3 użytkownika został odebrany z 1 użytkownika w niemal tym samym czasie. W związku z tym wybór użytkownika 2 jest zwinięte do podsumowania linii i jest niedostępny. Ten przypadek może mieć również został odwrócony, z wyborem 2 użytkownika jest pokazywany i 3 użytkownika obiektu jest zwinięty.

W obu przypadkach to zachowanie jest niepożądane, ponieważ 1 użytkownik powinien mieć możliwość dostępu zarówno użytkownika 2 i 3 użytkownika. Aby obsłużyć taką sytuację, Apple jest sugerujące, że rozszerzenie aplikacji wiadomości przechowuje stan komunikatu w chmurze oraz właściwość adres URL `MSMessage` (które są wysyłane między użytkownikami) można uzyskać dostępu do tego stanu.

Gdy użytkownik wysyła wiadomość, tokenu sesji jest generowany i przypisany do chmury przy bieżącym stanie wiadomości. Gdy użytkownik naciska na bąbelkowy komunikat w wykazie konwersacji, tokenu sesji służy do pobrania stanu bieżąca sesja z chmury.

### <a name="sender-identifiers"></a>Identyfikatorów nadawcy

Omówienia, uzyskiwanie dostępu do identyfikator nadawcy wiadomości, wykonaj przykład grupy konwersacji podane powyżej:

[ ![](advanced-message-app-extensions-images/interactive15.png "Grupa konwersacji wysyłania identyfikatorów")](advanced-message-app-extensions-images/interactive15.png)

1. Ponownie, 1 użytkownik wysyła grupę interakcyjne komunikat zapytaniem użytkownika 2 i 3 użytkownika, aby wybrać nadmiarów burgera.
2. Użytkownik 3 wybiera czekoladę.
3. Wybór użytkownika 3 dociera do użytkownika 1 i 2 użytkownik nie ma jeszcze odpowiedzi.
4. Ponieważ Apple poświęca wiele uwagi ochronie prywatności użytkownika, rozszerzenie aplikacji wiadomości tylko zna Unikatowy identyfikator (jako `NSUUID`) przypisany uczestnik w konwersacji. Na urządzeniu lokalnym tylko identyfikator bieżącego użytkownika jest znane.
5. `MSMessage` Ma `SenderIdentifier` właściwość, która pasuje do jednego użytkownika na liście uczestnika znane rozszerzenia.
6. Każde urządzenie użytkownika ma własną kopię listy uczestnika, gdzie ponownie, tylko identyfikator użytkownika lokalnego jest znany.
7. Po wysłaniu komunikatu jego `SenderIdentifier` właściwości jest również znany jako użytkownika lokalnego.

Identyfikatorów nadawcy mogą być używane w następujący sposób:

- Patrząc na liście uczestnika rozszerzenia można uzyskać liczbę użytkowników w konwersacji.
- Kiedy rozszerzenie odbiera komunikat od użytkownika, jego można zachować informacje o identyfikator nadawcy. Jeśli odbierze kolejną wiadomość o tym samym identyfikatorze nadawcy rozszerzenia wie, że pochodzi on z tego samego użytkownika.
- Mogą one używane do identyfikowania użytkownika określonego w konwersacji.

Identyfikator nadawcy mogą być używane w polach tekstowych z `MSMessageTemplateLayout` , używając znak dolara prefiksu (`$`). Na przykład:

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

W przypadku aplikacji Messages Wyświetla bąbelkowy wiadomości z tego typu formatowania, zastąpi `$uuid...` nazwą kontaktu uczestnika konwersacji, która wysłała wiadomość.

Identyfikator nadawcy jest unikatowe na każdym urządzeniu, więc patrzeć na diagramie powyżej ponownie Uwaga urządzenie użytkownika 1 i 3 użytkownika ma inny unikatowy identyfikator nadawcy dla każdego uczestnika w konwersacji.

Zakres identyfikatorów nadawcy to Instalowanie rozszerzenia aplikacji wiadomości. Tak, jeśli użytkownik powoduje odinstalowanie i ponowne zainstalowanie komunikat rozszerzenia aplikacji nowych identyfikatorów nadawcy zostanie wygenerowany dla nowej instalacji.

Aby uzyskać dostęp do identyfikatorów nadawcy, rozszerzenie można użyć poniższego kodu:

```csharp
public override void DidStartSendingMessage (MSMessage message, MSConversation conversation)
{
    base.DidStartSendingMessage (message, conversation);

    // Get the sender's participant ID
    var senderID = message.SenderParticipantIdentifier;

    // Get the local participant ID
    var localID = conversation.LocalParticipantIdentifier;

    // Get the remote participant IDs
    var remoteIDs = conversation.RemoteParticipantIdentifiers;
}
```

## <a name="supported-platforms"></a>Obsługiwane platformy

Interakcyjne komunikaty generowane przez rozszerzenie aplikacji wiadomości są dostarczane na następujących platformach firmy Apple:

- watchOS 3
- macOS Sierra
- iOS 10

Trzy platform iOS 10 umożliwi użytkownika, aby wygenerować komunikat interaktywnego. Na macOS Sierra, gdy użytkownik kliknie na interakcyjne bąbelków wiadomości, adres URL dołączony do `MSMessage` będą otwierane w programie Safari i powinna być wyświetlana reprezentację komunikatu.

WatchOS aplikacji Messages umożliwia programowi handoff interakcyjne komunikat na urządzeniu z systemem iOS dołączone gdzie użytkownika można utworzyć odpowiedź.

Nowy interfejs API wiadomości obsługuje rezerwowe, jeśli interakcyjne komunikat jest odbierany na starszych platformach firmy Apple:

- watchOS 2 +
- OS X 10.11 +
- iOS 9 +

Będą one dostarczane w formacie rezerwowy jako dwa osobne komunikaty:

- Obraz będzie dostarczone przez szablon układu.
- Drugi będzie adres URL, co zostało opisane w `MSMessage`.

## <a name="summary"></a>Podsumowanie

Ten artykuł został przedstawiony zaawansowane techniki pracy z rozszerzeniami aplikacji wiadomości w rozwiązaniu Xamarin.iOS, która integruje się z **wiadomości** aplikacji i obecny nowych funkcji dla użytkownika.


## <a name="related-links"></a>Linki pokrewne

- [Konstruktor lodów (przykład)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [Odwołanie wiadomości](https://developer.apple.com/reference/messages)
- [Przewodnik programowania w języku rozszerzenia aplikacji](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
