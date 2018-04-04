---
title: CallKit
description: W tym artykule opisano nowy interfejs API CallKit tego Apple wydane w ramach iOS 10 i jak ją wdrożyć w aplikacji platformy Xamarin.iOS VOIP.
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 67c761aa6656b571f16632dd1a076ff11737a424
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="callkit"></a>CallKit

_W tym artykule opisano nowy interfejs API CallKit tego Apple wydane w ramach iOS 10 i jak ją wdrożyć w aplikacji platformy Xamarin.iOS VOIP._


Nowy interfejs API CallKit w systemie iOS 10 umożliwia aplikacjom VOIP integrują się z iPhone interfejsu użytkownika i znanych interfejs i środowisko użytkownika końcowego. W tym interfejsie API użytkownicy mogą wyświetlać i interakcyjnie VOIP wywołania z ekranem blokady urządzenia z systemem iOS i zarządzać kontaktami przy użyciu aplikacji Phone **ulubione** i **ostatnich** widoków.

## <a name="about-callkit"></a>O CallKit

Zgodnie z firmy Apple CallKit jest nowe środowisko będzie podniesienie poziomu 3 aplikacji Voice Over IP (VOIP) firm do obsługi w systemie iOS 10 firmy 1. Interfejs API CallKit umożliwia aplikacjom VOIP integrują się z iPhone interfejsu użytkownika i znanych interfejs i środowisko użytkownika końcowego. Podobnie jak wbudowana aplikacja telefonu użytkownika można wyświetlić i interakcyjnie VOIP wywołania z ekranem blokady urządzenia z systemem iOS i zarządzać kontaktami przy użyciu aplikacji Phone **ulubione** i **ostatnich** widoków.

Ponadto interfejs API CallKit zapewnia możliwość tworzenia Extensions aplikacji, które można skojarzyć numeru telefonu z nazwą (identyfikator wywołującego) lub stwierdzić, że systemu, jeśli liczba może być zablokowany (wywołanie blokowania).

### <a name="the-existing-voip-app-experience"></a>Istniejące środowisko VOIP w aplikacji

Przed omówieniem nowy interfejs API CallKit i jego możliwości, podejmij przyjrzeć się bieżące środowisko użytkownika z 3rd strony VOIP aplikacji w systemie iOS 9 (lub starszej) przy użyciu fikcyjne aplikacji VOIP o nazwie MonkeyCall. MonkeyCall jest prosta aplikacja, która umożliwia wysyłanie i odbieranie połączeń VOIP przy użyciu istniejących interfejsów API dla systemu iOS.

Jeśli użytkownik otrzymuje połączenia przychodzącego na MonkeyCall i ich iPhone jest zablokowany, powiadomienia na ekranie blokady Odebrano jest obecnie można odróżnić od typu powiadomienia (podobne do tych z wiadomości lub poczty aplikacji, na przykład).

Jeśli użytkownik chce odpowiedzi na wywołanie, zostałyby slajd MonkeyCall powiadomienie, aby otworzyć aplikację, a następnie wprowadź ich dostępu (lub funkcję Touch ID użytkownika) do odblokowania telefonu, zanim można zaakceptować wywołanie i rozpocząć konwersację.

Działanie jest równie skomplikowane, jeśli telefon jest odblokowany. Ponownie połączenia przychodzącego MonkeyCall jest wyświetlana jako Baner powiadomień w wersji standard, który slajdów w górnej części ekranu. Ponieważ powiadomienie jest tymczasowy, mogą być łatwo pominięte przez użytkownika zmusi go do albo otwórz Centrum powiadomień i Znajdź określone powiadomienia do odpowiedzi, a następnie wywołać znaleźć i ręcznie uruchom aplikację MonkeyCall.

### <a name="the-callkit-voip-app-experience"></a>Środowisko aplikacji VOIP CallKit

Implementując nowych interfejsów API CallKit w aplikacji MonkeyCall, środowisko użytkownika z przychodzących połączeń VOIP może znacznie ulepszona w systemie iOS 10. Zająć przykład użytkownika odbieranie VOIP wywołania, gdy telefon jest zablokowana z powyżej. Zaimplementowanie CallKit wywołania będą wyświetlane na ekranie blokady iPhone, tak samo, jak gdyby wywołania została odebrana wbudowana aplikacja telefonu z pełnego ekranu, natywnego interfejsu użytkownika i standardowych funkcji przejdź do odpowiedzi.

Ponownie Jeśli telefonów iPhone został odblokowany w momencie odebrania połączenia MonkeyCall VOIP, tej samej pełnego ekranu, natywnego interfejsu użytkownika i standardowe przejdź do odpowiedzi i naciśnij do odrzucenia funkcje wbudowane zobaczy aplikację telefonów i MonkeyCall ma możliwość odtwarzanie dzwonek niestandardowych .

CallKit zapewnia dodatkowe funkcje do MonkeyCall, dzięki czemu jego VOIP wywołuje wchodzić w interakcje z innych typów połączeń, do jego wyświetlenia na wbudowanych w ostatnich i listy ulubionych, do korzystania z wbudowanych funkcji nie przeszkadzać i bloku, należy uruchomić MonkeyCall wywołania z Siri i oferuje możliwość przez użytkowników można przypisać MonkeyCall wywołania do osób w aplikacji kontaktów.

Poniższe sekcje omówiono architekturę CallKit, przychodzących i wychodzących wywoływać przepływy i interfejsu API CallKit szczegółowo.


## <a name="the-callkit-architecture"></a>Architektura CallKit

W systemie iOS 10 Apple przyjęła CallKit we wszystkich usług System tak, aby wywołania CarPlay, na przykład wiadomo, że interfejs użytkownika systemu za pośrednictwem CallKit. W podanym przykładzie poniżej, ponieważ MonkeyCall przyjmuje CallKit on jest nieznany w systemie w taki sam sposób jak te wbudowane usług systemowych i pobiera wszystkie te same funkcje:

[![](callkit-images/callkit01.png "Stos CallKit usługi")](callkit-images/callkit01.png#lightbox)

Przyjrzyjmy się bliżej aplikacji MonkeyCall z powyższym diagramie. Zawiera wszystkie jego kod do komunikowania się z sieci i zawiera interfejsy użytkownika aplikacji. Łączy w CallKit do komunikacji z systemem:

[![](callkit-images/callkit02.png "Architektura aplikacji MonkeyCall")](callkit-images/callkit02.png#lightbox)

Istnieją dwa główne interfejsy CallKit, który korzysta z aplikacji:

- `CXProvider` — Umożliwia aplikacji MonkeyCall informuje system żadnych powiadomień poza pasmem, które mogą wystąpić.
- `CXCallController` — Umożliwia aplikacji MonkeyCall informuje system akcje użytkownika lokalnego.

### <a name="the-cxprovider"></a>CXProvider

Jak wspomniano powyżej, `CXProvider` umożliwia aplikacji poinformować system żadnych powiadomień poza pasmem, które mogą wystąpić. Są to powiadomienie, że nie występuje z powodu działań użytkownika lokalnego, ale występuje z powodu zdarzenia zewnętrzne, takie jak połączenia przychodzące.

Aplikacja powinna używać `CXProvider` dla następujących:

- Raport połączenia przychodzącego do systemu.
- Raport, który wychodzące połączenie ma podłączone do systemu.
- Raport użytkownika zdalnego kończy wywołanie systemu.

Gdy aplikacja potrzebuje do komunikowania się do systemu, używa `CXCallUpdate` klasy, a gdy System musi komunikować się z aplikacją, używa `CXAction` klasy:

[![](callkit-images/callkit03.png "Podczas komunikacji z systemu za pośrednictwem CXProvider")](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>CXCallController

`CXCallController` Dzięki czemu aplikacja może poinformować system akcje użytkownika lokalnego, takie jak uruchamianie wywołanie VOIP użytkownika. Zaimplementowanie `CXCallController` aplikacja pobiera na wzajemne oddziaływanie z innych typów wywołań w systemie. Na przykład, jeśli istnieje już połączenie active telefonii w toku `CXCallController` można zezwolić aplikacji VOIP umieścić tego wywołania wstrzymane i uruchomić lub odpowiedzi na wywołanie VOIP.

Aplikacja powinna używać `CXCallController` dla następujących:

- Raport, gdy użytkownik rozpocznie wywołania wychodzącego do systemu.
- Raport, gdy użytkownik odbierze połączenie do systemu.
- Raport, gdy użytkownik zakończy połączenie do systemu.

Gdy aplikacja chce komunikacji akcje użytkownika lokalnego do systemu, używa `CXTransaction` klasy:

[![](callkit-images/callkit04.png "Raportowanie w systemie przy użyciu CXCallController")](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>Implementowanie CallKit

Poniższe sekcje pokazują, jak zaimplementować CallKit w aplikacji platformy Xamarin.iOS VOIP. Ze względu na przykład tego dokumentu będą używać kodu z fikcyjne aplikacji MonkeyCall VOIP. Kod przedstawiony tutaj reprezentuje kilka klas pomocniczych, CallKit zostanie określone fragmenty zostały szczegółowo opisane w poniższych sekcjach.

### <a name="the-activecall-class"></a>Klasa ActiveCall

`ActiveCall` Klasa jest używana przez aplikację MonkeyCall do przechowywania wszystkich informacji o wywołaniu VOIP, który jest obecnie aktywny w następujący sposób:

```csharp
using System;
using CoreFoundation;
using Foundation;

namespace MonkeyCall
{
    public class ActiveCall
    {
        #region Private Variables
        private bool isConnecting;
        private bool isConnected;
        private bool isOnhold;
        #endregion

        #region Computed Properties
        public NSUuid UUID { get; set; }
        public bool isOutgoing { get; set; }
        public string Handle { get; set; }
        public DateTime StartedConnectingOn { get; set;}
        public DateTime ConnectedOn { get; set;}
        public DateTime EndedOn { get; set; }

        public bool IsConnecting {
            get { return isConnecting; }
            set {
                isConnecting = value;
                if (isConnecting) StartedConnectingOn = DateTime.Now;
                RaiseStartingConnectionChanged ();
            }
        }

        public bool IsConnected {
            get { return isConnected; }
            set {
                isConnected = value;
                if (isConnected) {
                    ConnectedOn = DateTime.Now;
                } else {
                    EndedOn = DateTime.Now;
                }
                RaiseConnectedChanged ();
            }
        }

        public bool IsOnHold {
            get { return isOnhold; }
            set {
                isOnhold = value;
            }
        }
        #endregion

        #region Constructors
        public ActiveCall ()
        {
        }

        public ActiveCall (NSUuid uuid, string handle, bool outgoing)
        {
            // Initialize
            this.UUID = uuid;
            this.Handle = handle;
            this.isOutgoing = outgoing;
        }
        #endregion

        #region Public Methods
        public void StartCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call starting successfully
            completionHandler (true);

            // Simulate making a starting and completing a connection
            DispatchQueue.MainQueue.DispatchAfter (new DispatchTime(DispatchTime.Now, 3000), () => {
                // Note that the call is starting
                IsConnecting = true;

                // Simulate pause before connecting
                DispatchQueue.MainQueue.DispatchAfter (new DispatchTime (DispatchTime.Now, 1500), () => {
                    // Note that the call has connected
                    IsConnecting = false;
                    IsConnected = true;
                });
            });
        }

        public void AnswerCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call being answered
            IsConnected = true;
            completionHandler (true);
        }

        public void EndCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call ending
            IsConnected = false;
            completionHandler (true);
        }
        #endregion

        #region Events
        public delegate void ActiveCallbackDelegate (bool successful);
        public delegate void ActiveCallStateChangedDelegate (ActiveCall call);

        public event ActiveCallStateChangedDelegate StartingConnectionChanged;
        internal void RaiseStartingConnectionChanged ()
        {
            if (this.StartingConnectionChanged != null) this.StartingConnectionChanged (this);
        }

        public event ActiveCallStateChangedDelegate ConnectedChanged;
        internal void RaiseConnectedChanged ()
        {
            if (this.ConnectedChanged != null) this.ConnectedChanged (this);
        }
        #endregion
    }
}
```

`ActiveCall` zawiera kilka właściwości, które definiują stan połączenia i dwa zdarzenia, które można wywoływane, gdy zmieni się stan wywołania. Ponieważ jest tylko przykładem, istnieją trzy metody używane do symulowany początkową, odpowiedzi i końcową wywołania.

### <a name="the-startcallrequest-class"></a>Klasa StartCallRequest

`StartCallRequest` Klasa statyczna zawiera kilka metody pomocnicze, które będą używane podczas pracy wychodzącego wywołania:

```csharp
using System;
using Foundation;
using Intents;

namespace MonkeyCall
{
    public static class StartCallRequest
    {
        public static string URLScheme {
            get { return "monkeycall"; }
        }

        public static string ActivityType {
            get { return INIntentIdentifier.StartAudioCall.GetConstant ().ToString (); }
        }

        public static string CallHandleFromURL (NSUrl url)
        {
            // Is this a MonkeyCall handle?
            if (url.Scheme == URLScheme) {
                // Yes, return host
                return url.Host;
            } else {
                // Not handled
                return null;
            }
        }

        public static string CallHandleFromActivity (NSUserActivity activity)
        {
            // Is this a start call activity?
            if (activity.ActivityType == ActivityType) {
                // Yes, trap any errors
                try {
                    // Get first contact
                    var interaction = activity.GetInteraction ();
                    var startAudioCallIntent = interaction.Intent as INStartAudioCallIntent;
                    var contact = startAudioCallIntent.Contacts [0];

                    // Get the person handle
                    return contact.PersonHandle.Value;
                } catch {
                    // Error, report null
                    return null;
                }
            } else {
                // Not handled
                return null;
            }
        }
    }
}
```

`CallHandleFromURL` i `CallHandleFromActivity` klasy są używane w AppDelegate można pobrać uchwytu kontaktu, osoby, wywoływana w wywołaniu wychodzących. Aby uzyskać więcej informacji, zobacz [obsługi połączeń wychodzących](#Handling-Outgoing-Calls) poniższej sekcji.

### <a name="the-activecallmanager-class"></a>Klasa ActiveCallManager

`ActiveCallManager` Klasa obsługuje wszystkie otwarte połączenia w aplikacji MonkeyCall.

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using CallKit;

namespace MonkeyCall
{
    public class ActiveCallManager
    {
        #region Private Variables
        private CXCallController CallController = new CXCallController ();
        #endregion

        #region Computed Properties
        public List<ActiveCall> Calls { get; set; }
        #endregion

        #region Constructors
        public ActiveCallManager ()
        {
            // Initialize
            this.Calls = new List<ActiveCall> ();
        }
        #endregion

        #region Private Methods
        private void SendTransactionRequest (CXTransaction transaction)
        {
            // Send request to call controller
            CallController.RequestTransaction (transaction, (error) => {
                // Was there an error?
                if (error == null) {
                    // No, report success
                    Console.WriteLine ("Transaction request sent successfully.");
                } else {
                    // Yes, report error
                    Console.WriteLine ("Error requesting transaction: {0}", error);
                }
            });
        }
        #endregion

        #region Public Methods
        public ActiveCall FindCall (NSUuid uuid)
        {
            // Scan for requested call
            foreach (ActiveCall call in Calls) {
                if (call.UUID == uuid) return call;
            }

            // Not found
            return null;
        }

        public void StartCall (string contact)
        {
            // Build call action
            var handle = new CXHandle (CXHandleType.Generic, contact);
            var startCallAction = new CXStartCallAction (new NSUuid (), handle);

            // Create transaction
            var transaction = new CXTransaction (startCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void EndCall (ActiveCall call)
        {
            // Build action
            var endCallAction = new CXEndCallAction (call.UUID);

            // Create transaction
            var transaction = new CXTransaction (endCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void PlaceCallOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, true);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void RemoveCallFromOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, false);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }
        #endregion
    }
}
```

Ponownie, ponieważ jest to tylko symulację `ActiveCallManager` tylko zapewnia zbiór `ActiveCall` obiekty i zawiera procedury do znajdowania danego wywołanie przez jego `UUID` właściwości. Zawiera również metody służące do uruchamiania, kończyć i Zmień stan wstrzymania wywołania wychodzącego. Aby uzyskać więcej informacji, zobacz [obsługi połączeń wychodzących](#Handling-Outgoing-Calls) poniższej sekcji.

### <a name="the-providerdelegate-class"></a>Klasa ProviderDelegate

Jak wspomniano powyżej, `CXProvider` zapewnia dwukierunkowej komunikacji między aplikacji i systemu powiadomień poza pasmem. Deweloper musi podać niestandardowy `CXProviderDelegate` i dołączenie go do `CXProvider` dla aplikacji do obsługi zdarzeń CallKit poza pasmem. MonkeyCall są używane następujące `CXProviderDelegate`:

```csharp
using System;
using Foundation;
using CallKit;
using UIKit;

namespace MonkeyCall
{
    public class ProviderDelegate : CXProviderDelegate
    {
        #region Computed Properties
        public ActiveCallManager CallManager { get; set;}
        public CXProviderConfiguration Configuration { get; set; }
        public CXProvider Provider { get; set; }
        #endregion

        #region Constructors
        public ProviderDelegate (ActiveCallManager callManager)
        {
            // Save connection to call manager
            CallManager = callManager;

            // Define handle types
            var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };

            // Get Image Mask
            var maskImage = UIImage.FromFile ("telephone_receiver.png");

            // Setup the initial configurations
            Configuration = new CXProviderConfiguration ("MonkeyCall") {
                MaximumCallsPerCallGroup = 1,
                SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
                IconMaskImageData = maskImage.AsPNG(),
                RingtoneSound = "musicloop01.wav"
            };

            // Create a new provider
            Provider = new CXProvider (Configuration);

            // Attach this delegate
            Provider.SetDelegate (this, null);

        }
        #endregion

        #region Override Methods
        public override void DidReset (CXProvider provider)
        {
            // Remove all calls
            CallManager.Calls.Clear ();
        }

        public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
        {
            // Create new call record
            var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

            // Monitor state changes
            activeCall.StartingConnectionChanged += (call) => {
                if (call.isConnecting) {
                    // Inform system that the call is starting
                    Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
                }
            };

            activeCall.ConnectedChanged += (call) => {
                if (call.isConnected) {
                    // Inform system that the call has connected
                    provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
                }
            };

            // Start call
            activeCall.StartCall ((successful) => {
                // Was the call able to be started?
                if (successful) {
                    // Yes, inform the system
                    action.Fulfill ();

                    // Add call to manager
                    CallManager.Calls.Add (activeCall);
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.AnswerCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.EndCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Remove call from manager's queue
                    CallManager.Calls.Remove (call);

                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformSetHeldCallAction (CXProvider provider, CXSetHeldCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Update hold status
            call.isOnHold = action.OnHold;

            // Inform system of success
            action.Fulfill ();
        }

        public override void TimedOutPerformingAction (CXProvider provider, CXAction action)
        {
            // Inform user that the action has timed out
        }

        public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // Start the calls audio session here
        }

        public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // End the calls audio session and restart any non-call
            // related audio
        }
        #endregion

        #region Public Methods
        public void ReportIncomingCall (NSUuid uuid, string handle)
        {
            // Create update to describe the incoming call and caller
            var update = new CXCallUpdate ();
            update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

            // Report incoming call to system
            Provider.ReportNewIncomingCall (uuid, update, (error) => {
                // Was the call accepted
                if (error == null) {
                    // Yes, report to call manager
                    CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
                } else {
                    // Report error to user here
                    Console.WriteLine ("Error: {0}", error);
                }
            });
        }
        #endregion
    }
}
```

Gdy tworzone jest wystąpienie tego obiektu delegowanego, jest przekazywany `ActiveCallManager` zostanie użyty do obsługi żadnego działania wywołania. Następnie definiuje typ dojścia (`CXHandleType`) który `CXProvider` będzie odpowiadać:

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

I pobiera maski, które zostaną zastosowane do ikony aplikacji, gdy połączenie jest w toku:

```csharp
// Get Image Mask
var maskImage = UIImage.FromFile ("telephone_receiver.png");
```

Te wartości get połączone w `CXProviderConfiguration` który będzie używany do konfigurowania `CXProvider`:

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconMaskImageData = maskImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

Delegat następnie tworzy nowy `CXProvider` w tych konfiguracjach i dołącza się do niej:

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

Używając CallKit, aplikacja będzie już tworzenia i obsługi własnej sesji audio, zamiast tego należy skonfigurować i używać audio sesji, który utworzy i obsługi dla tego systemu. 

Jeśli była to rzeczywista aplikacji, `DidActivateAudioSession` metoda będzie służyć rozpoczynać wywołanie wstępnie skonfigurowane `AVAudioSession` zapewnianej przez System:

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

Może również użyć `DidDeactivateAudioSession` audio sesji podana metoda finalize i wersji jego połączenia z systemem:

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

Pozostała część kod zostanie omówiona szczegółowo w kolejnych sekcjach.

### <a name="the-appdelegate-class"></a>Klasa AppDelegate

MonkeyCall używa AppDelegate do przechowywania wystąpień `ActiveCallManager` i `CXProviderDelegate` które będą używane w całej aplikacji:

```csharp
using Foundation;
using UIKit;
using Intents;
using System;

namespace MonkeyCall
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        #region Constructors
        public override UIWindow Window { get; set; }
        public ActiveCallManager CallManager { get; set; }
        public ProviderDelegate CallProviderDelegate { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Initialize the call handlers
            CallManager = new ActiveCallManager ();
            CallProviderDelegate = new ProviderDelegate (CallManager);

            return true;
        }

        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            // Get handle from url
            var handle = StartCallRequest.CallHandleFromURL (url);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from URL: {0}", url); 
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
        {
            var handle = StartCallRequest.CallHandleFromActivity (userActivity);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        ...
        #endregion
    }
}
```

`OpenUrl` i `ContinueUserActivity` zastąpienie metody są używane, gdy aplikacja przetwarza wywołanie wychodzące. Aby uzyskać więcej informacji, zobacz [obsługi połączeń wychodzących](#Handling-Outgoing-Calls) poniższej sekcji.

## <a name="handling-incoming-calls"></a>Obsługiwanie połączeń przychodzących

Istnieje kilka stanów i procesy, które przychodzących połączeń VOIP przejść przez kolejne podczas Typowy przepływ pracy przychodzące wywołania takich jak:

- Informowanie użytkowników (i System) czy połączenie istnieje.
- Odbieranie powiadomienia, gdy użytkownik chce odebranie połączenia i inicjowania połączenie z innym użytkownikiem.
- Gdy użytkownik chce zakończenia bieżącego wywołania informuje System i komunikacji sieciowej.

Poniższe sekcje będzie szczegółowe Spójrz na aplikację użycia CallKit do obsługi przychodzącego przepływu pracy wywołanie, ponownie przy użyciu aplikacji MonkeyCall VOIP jako przykład.

### <a name="informing-user-of-incoming-call"></a>Informowanie użytkowników połączenia przychodzącego

Użytkownik zdalny został uruchomiony konwersacji VOIP użytkownika lokalnego, są następujące operacje:

[![](callkit-images/callkit05.png "Użytkownik zdalny rozpoczęto konwersacji VOIP")](callkit-images/callkit05.png#lightbox)

1. Aplikacja pobiera powiadomienie z jego sieć komunikacji przychodzących połączeń VOIP istnieje.
2. Aplikacja używa `CXProvider` do wysyłania `CXCallUpdate` do informowania o wywołaniu systemu.
3. System publikuje wywołanie interfejsu użytkownika systemu, usług systemowych i wszystkie inne aplikacje VOIP przy użyciu CallKit.

Na przykład w `CXProviderDelegate`:

```csharp
public void ReportIncomingCall (NSUuid uuid, string handle)
{
    // Create update to describe the incoming call and caller
    var update = new CXCallUpdate ();
    update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

    // Report incoming call to system
    Provider.ReportNewIncomingCall (uuid, update, (error) => {
        // Was the call accepted
        if (error == null) {
            // Yes, report to call manager
            CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
        } else {
            // Report error to user here
            Console.WriteLine ("Error: {0}", error);
        }
    });
}
```

Ten kod tworzy nową `CXCallUpdate` wystąpienia i dołącza dojścia do niego identyfikujący wywołującego. Następnie używa `ReportNewIncomingCall` metody `CXProvider` klasę, aby poinformować system wywołania. Jeśli ten zakończy się powodzeniem, wywołanie zostanie dodany do kolekcji aplikacji active wywołań, jeśli nie, błąd muszą być zgłaszane do użytkownika.

### <a name="user-answering-incoming-call"></a>Odpowiadanie na przychodzące wywołanie użytkownika

Jeśli użytkownik chce odebranie połączenia przychodzącego VOIP, są następujące operacje:

[![](callkit-images/callkit06.png "Użytkownik odpowiada na przychodzące wywołanie VOIP")](callkit-images/callkit06.png#lightbox)

1. Interfejsu użytkownika systemu informuje System, że użytkownik chce odpowiedzi na wywołanie VOIP.
2. Wysyła System `CXAnswerCallAction` w aplikacji `CXProvider` informowania o zamiar odpowiedzi.
3. Sieci komunikacyjnej aplikacji informuje o tym, że użytkownik jest udzielenie odpowiedzi na wywołanie i wywołanie VOIP będzie kontynuowana w zwykły sposób.

Na przykład w `CXProviderDelegate`:

```csharp
public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.AnswerCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Ten kod najpierw szuka danego wywołania swoją listę aktywnych połączeń. Jeśli nie można odnaleźć wywołania, system zostaje powiadomiony i metoda. Jeśli okaże się `AnswerCall` metody `ActiveCall` klasy jest wywoływana w celu uruchomienia wywołania i systemu znajdują się informacje, jeśli jego powodzenia lub niepowodzenia.

### <a name="user-ending-incoming-call"></a>Użytkownik końcowy połączenia przychodzącego

Jeśli użytkownik chce przerwanie połączenia od w Interfejsie użytkownika aplikacji, są następujące operacje:

[![](callkit-images/callkit07.png "Użytkownik kończy wywołania od w Interfejsie użytkownika aplikacji")](callkit-images/callkit07.png#lightbox)

1. Tworzy aplikację `CXEndCallAction` który pobiera połączone w `CXTransaction` wysyłana do uzna, że kończy wywołanie do systemu.
2. System sprawdza, czy próba wywołania zakończenia i wysyła `CXEndCallAction` powrót do aplikacji za pomocą `CXProvider`.
3. Aplikacja informuje następnie sieci komunikacyjnej kończy wywołanie.

Na przykład w `CXProviderDelegate`:

```csharp
public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.EndCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Remove call from manager's queue
            CallManager.Calls.Remove (call);

            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Ten kod najpierw szuka danego wywołania swoją listę aktywnych połączeń. Jeśli nie można odnaleźć wywołania, system zostaje powiadomiony i metoda. Jeśli okaże się `EndCall` metody `ActiveCall` klasy jest wywoływana, aby zakończyć połączenie i systemu znajdują się informacje, jeśli jego powodzenia lub niepowodzenia. W przypadku powodzenia, wywołanie zostanie usunięty z kolekcji aktywne wywołania.

## <a name="managing-multiple-calls"></a>Zarządzanie wielu wywołań

Większość aplikacji VOIP może obsługiwać wiele połączeń jednocześnie. Na przykład, jeśli jest aktualnie aktywnego połączenia VOIP i powiadomienie pobiera aplikacji nowego połączenia przychodzącego użytkownik istnieje można wstrzymać lub rozłączenie przy pierwszym wywołaniu odpowiedzi na drugi.

W powyższym określają sytuacji System będzie wysyłać `CXTransaction` aplikacja, która będzie zawierać listę wielu działań (takie jak `CXEndCallAction` i `CXAnswerCallAction`). Wszystkie te działania należy oddzielnie, spełnione tak, aby System można odpowiednio zaktualizować interfejsu użytkownika.

## <a name="handling-outgoing-calls"></a>Obsługa wywołania wychodzące

Jeśli użytkownik naciska wpisu z listy ostatnich (w aplikacji telefonu), na przykład, to po wywołaniu należące do aplikacji, go będą wysyłane _Start próba wywołania_ przez system:

[![](callkit-images/callkit08.png "Próba wywołania rozpoczęcia odbierania")](callkit-images/callkit08.png#lightbox)

1. Aplikacja utworzy _Start wywołanie_ oparte na Start wywołać zamiar otrzymanej z systemu. 
2. Aplikacja będzie używać `CXCallController` żądania Akcja uruchamiania wywołania z systemu.
3. Jeśli System akceptuje akcji, zostanie zwrócony do aplikacji za pośrednictwem `XCProvider` delegowanie.
4. Aplikacja rozpoczyna wywołanie wychodzące z sieci komunikacyjnej.

Aby uzyskać więcej informacji o lokalizacji docelowych, zobacz nasze [intencje i rozszerzeń interfejsu użytkownika intencje](~/ios/platform/sirikit/understanding-sirikit.md) dokumentacji. 

### <a name="the-outgoing-call-lifecycle"></a>Wychodzące cyklu życia wywołania

Podczas pracy z CallKit i wywołania wychodzącego, należy poinformować System następujące zdarzenia cyklu życia aplikacji:

1. **Uruchamianie** -informuje system, który ma rozpocząć wywołania wychodzącego.
2. **Rozpoczęto** -informuje system, który wywołania wychodzącego została uruchomiona.
3. **Łączenie** -informuje system, który nawiązuje połączenie wywołania wychodzącego.
4. **Połączone** -informuje wychodzącej został połączony wywołania i że obie strony mogą teraz komunikować.

Na przykład następujący kod rozpocznie wywołania wychodzącego:

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void StartCall (string contact)
{
    // Build call action
    var handle = new CXHandle (CXHandleType.Generic, contact);
    var startCallAction = new CXStartCallAction (new NSUuid (), handle);

    // Create transaction
    var transaction = new CXTransaction (startCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Tworzy `CXHandle` i używa go do skonfigurowania `CXStartCallAction` które jest połączone w `CXTransaction` to znaczy wysyłane do systemu za pomocą `RequestTransaction` metody `CXCallController` klasy. Wywołując `RequestTransaction` metody, System można umieścić żadnych istniejących połączeń wstrzymane, niezależnie od tego źródła (FaceTime, VOIP, telefon itp.), przed uruchomieniem nowego wywołania.

Żądania uruchomienia wywołania wychodzącego VOIP mogą pochodzić z różnych źródeł, takich jak Siri, wpis na karcie kontaktu (w aplikacji kontaktów) lub z listy ostatnich (w aplikacji Phone). W takich przypadkach aplikacji zostanie wysłana Start wywołać zamiar wewnątrz `NSUserActivity` i trzeba je obsłużyć AppDelegate:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var handle = StartCallRequest.CallHandleFromActivity (userActivity);

    // Found?
    if (handle == null) {
        // No, report to system
        Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
        return false;
    } else {
        // Yes, start call and inform system
        CallManager.StartCall (handle);
        return true;
    }
}
```

W tym miejscu `CallHandleFromActivity` metody klasy Pomocnika `StartCallRequest` jest używana, można uzyskać dojścia do osoby wywoływana (zobacz [klasy StartCallRequest](#The-StartCallRequest-Class) powyżej). 

`PerformStartCallAction` Metody [klasy ProviderDelegate](#The-ProviderDelegate-Class) służy do na koniec uruchom to rzeczywiste wywołanie wychodzących i informuje System jego życia:

```csharp
public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
{
    // Create new call record
    var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

    // Monitor state changes
    activeCall.StartingConnectionChanged += (call) => {
        if (call.IsConnecting) {
            // Inform system that the call is starting
            Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
        }
    };

    activeCall.ConnectedChanged += (call) => {
        if (call.IsConnected) {
            // Inform system that the call has connected
            Provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
        }
    };

    // Start call
    activeCall.StartCall ((successful) => {
        // Was the call able to be started?
        if (successful) {
            // Yes, inform the system
            action.Fulfill ();

            // Add call to manager
            CallManager.Calls.Add (activeCall);
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Tworzy wystąpienie `ActiveCall` klasy (do przechowywania informacji o wywołaniu w toku) i wypełnia osoby wywoływane. `StartingConnectionChanged` i `ConnectedChanged` zdarzenia są używane do monitorowania i raportowania wychodzących cyklu życia wywołania. Wywołanie jest uruchomiona i System poinformowany spełnienia akcji.

### <a name="ending-an-outgoing-call"></a>Kończenie wywołania wychodzącego

Gdy użytkownik zakończył się z połączeń wychodzących i chce Zakończ ją, może służyć następujący kod:

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void EndCall (ActiveCall call)
{
    // Build action
    var endCallAction = new CXEndCallAction (call.UUID);

    // Create transaction
    var transaction = new CXTransaction (endCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Jeśli tworzy `CXEndCallAction` z identyfikatorem UUID wywołania do końca, zawiera on w `CXTransaction` to znaczy wysyłane do systemu za pomocą `RequestTransaction` metody `CXCallController` klasy. 

## <a name="additional-callkit-details"></a>CallKit dodatkowe szczegóły

W tej sekcji opisano niektóre dodatkowe szczegóły, które dewelopera należy wziąć pod uwagę podczas pracy z CallKit, takich jak:

- Konfiguracja dostawcy
- Błędy akcji
- Ograniczenia systemu
- VOIP Audio

### <a name="provider-configuration"></a>Konfiguracja dostawcy

Konfiguracja dostawcy, dzięki czemu aplikacja systemu iOS 10 VOIP dostosowywaniu środowiska użytkownika (wewnątrz natywnego interfejsu użytkownika w wywołaniu) podczas pracy z CallKit.

Aplikacja może wykonać następujące dostosowania:

- Zlokalizowana nazwa wyświetlana.
- Włącz obsługę połączenia wideo.
- Dostosowywanie przycisków, wywołań w interfejsie użytkownika z uwzględnieniem ikonę maskowanego obrazu. Interakcji użytkowników z przycisków niestandardowych są wysyłane bezpośrednio do aplikacji do przetworzenia. 

### <a name="action-errors"></a>Błędy akcji

aplikacji dla systemu iOS 10 VOIP CallKit muszą obsługiwać akcji nie może bezpiecznie i użytkownika o stan akcji przez cały czas. 

Poniższy przykład wziąć pod uwagę:

1. Aplikacja otrzymał Akcja uruchamiania wywołań i rozpoczął proces inicjowania nowe wywołanie VOIP z jego siecią komunikacji.
2. Ze względu na ograniczone lub nie możliwości komunikacji w sieci to połączenie nie powiedzie się.
3. Aplikacja *musi* wysyłania **się nie powieść** Akcja uruchamiania wywołać wiadomości (`Action.Fail()`) do informowania o awarii systemu.
4. Dzięki temu System informują użytkownika o stanie wywołania. Na przykład, aby wyświetlić błąd wywołania interfejsu użytkownika.

Ponadto należy odpowiedzieć na aplikację systemu iOS 10 VOIP _błędy przekroczenia limitu czasu_ który może wystąpić, gdy nie można przetworzyć akcji oczekiwanego w określonym czasie. Każdy typ działania podał CallKit ma maksymalną wartość limitu czasu skojarzony z nim. Te wartości limitu czasu upewnij się, że wszelkie działania CallKit żądanej przez użytkownika jest obsługiwane w sposób elastyczny, w związku z tym zachowaniu systemu operacyjnego płynne i szybkie również.

Istnieje kilka metod w delegacie dostawcy (`CXProviderDelegate`) który powinna zostać zastąpiona w celu obsługiwać także tej sytuacji limitu czasu.

### <a name="system-restrictions"></a>Ograniczenia systemu

Na podstawie bieżącego stanu urządzenia z systemem iOS, aplikację systemu iOS 10 VOIP, niektóre ograniczenia systemu może wtedy zostać wymuszone.

Na przykład przychodzących połączeń VOIP można ograniczyć przez System, jeśli:

1. Osoba kontaktująca znajduje się na liście wywołującego zablokowane przez użytkownika.
2. Urządzenie systemu iOS użytkownika jest w trybie przeszkadzać nie są.

Jeśli wywołanie VOIP jest ograniczony przy użyciu jednej z tych sytuacji, należy użyć poniższego kodu do jej obsługi:

```csharp
public class ProviderDelegate : CXProviderDelegate
{
...

    public void ReportIncomingCall (NSUuid uuid, string handle)
    {
        // Create update to describe the incoming call and caller
        var update = new CXCallUpdate ();
        update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);
    
        // Report incoming call to system
        Provider.ReportNewIncomingCall (uuid, update, (error) => {
            // Was the call accepted
            if (error == null) {
                // Yes, report to call manager
                CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
            } else {
                // Report error to user here
                if (error.Code == (int)CXErrorCodeIncomingCallError.CallUuidAlreadyExists) {
                    // Handle duplicate call ID
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByBlockList) {
                    // Handle call from blocked user
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByDoNotDisturb) {
                    // Handle call while in do-not-disturb mode
                } else {
                    // Handle unknown error
                }
            }
        });
    }

}
```

### <a name="voip-audio"></a>VOIP Audio

CallKit zapewnia kilka korzyści dla obsługi audio zasoby wymagające aplikację systemu iOS 10 VOIP głosu VOIP na żywo. Jedną z największych korzyści jest aplikacji audio sesji zostanie podniesiony priorytetów uruchomionej w systemie iOS 10. Jest tym samym poziomie priorytetu jako wbudowanej Phone i aplikacji FaceTime i ten poziom priorytetu rozszerzone będzie uniemożliwiać innych uruchomionych aplikacji zakłócania pracy aplikacji VOIP audio sesji.

Ponadto CallKit ma dostęp do innych audio wskazówek routingu, które mogą zwiększyć wydajność oraz inteligentnie kierować VOIP audio do konkretnych urządzeń wyjściowych podczas połączenia na żywo na podstawie preferencji użytkownika i Stany urządzeń. Na przykład na podstawie podłączone urządzenia, takie jak słuchawki bluetooth, połączenia na żywo CarPlay lub ustawienia ułatwień dostępu.

Podczas cyklu typowe VOIP wywołanie przy użyciu CallKit, należy skonfigurować strumień Audio, który dostarczy go CallKit aplikacji. Spójrz na poniższym przykładzie:

[![](callkit-images/callkit09.png "Sekwencja uruchamiania wywołanie akcji")](callkit-images/callkit09.png#lightbox)

1. Akcja uruchamiania wywołać jest odbierany przez aplikację do odpowiedzi połączenia przychodzącego.
2. Zanim ta akcja jest spełnione przez aplikację, zapewnia powoduje wymaga konfiguracji, który jest jego `AVAudioSession`.
3. Aplikacja informuje System spełnienia akcji.
4. Przed nawiązaniem połączenia, CallKit zapewnia wysoki priorytet `AVAudioSession` dopasowania konfiguracji żądanej aplikacji. Aplikacja zostanie powiadomiony poprzez `DidActivateAudioSession` metoda jego `CXProviderDelegate`.

## <a name="working-with-call-directory-extensions"></a>Praca z wywołania rozszerzenia katalogów

Podczas pracy z CallKit, _wywołania rozszerzenia katalogów_ umożliwiają dodawanie zablokowanych numerów i zidentyfikować numery, które są specyficzne dla danej aplikacji VOIP do kontaktów w skontaktuj się z aplikacji na urządzeniu z systemem iOS.

### <a name="implementing-a-call-directory-extension"></a>Wdrażanie rozszerzenia katalogu wywołania

Aby zaimplementować rozszerzenie katalogu wywołań w aplikacji platformy Xamarin.iOS, wykonaj następujące czynności:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otwórz rozwiązanie aplikacji w programie Visual Studio dla komputerów Mac.
2. Kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksploratora rozwiązań** i wybierz **Dodaj** > **Dodawanie nowego projektu**.
3. Wybierz **iOS** > **rozszerzenia** > **wywołania rozszerzenia katalogów** i kliknij przycisk **dalej** przycisk: 

    [![](callkit-images/calldir01.png "Tworzenie nowego rozszerzenia katalogu wywołania")](callkit-images/calldir01.png#lightbox)
4. Wprowadź **nazwa** rozszerzenia i kliknij **dalej** przycisk: 

    [![](callkit-images/calldir02.png "Wprowadź nazwę rozszerzenia")](callkit-images/calldir02.png#lightbox)
5. Dostosuj **Nazwa projektu** i/lub **Nazwa rozwiązania** wymagane, a następnie kliknij przycisk **Utwórz** przycisk: 

    [![](callkit-images/calldir03.png "Tworzenie projektu")](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Otwórz rozwiązanie aplikacji w programie Visual Studio.
2. Kliknij prawym przyciskiem myszy nazwę rozwiązania w **Eksploratora rozwiązań** i wybierz **Dodaj** > **Dodawanie nowego projektu**.
3. Wybierz **iOS** > **rozszerzenia** > **wywołania rozszerzenia katalogów** i kliknij przycisk **dalej** przycisk: 

    [![](callkit-images/calldir01w.png "Tworzenie nowego rozszerzenia katalogu wywołania")](callkit-images/calldir01.png#lightbox)
4. Wprowadź **nazwa** rozszerzenia i kliknij **OK** przycisku

-----


Spowoduje to dodanie `CallDirectoryHandler.cs` klasy do projektu, który wygląda podobnie do następującej:

```csharp
using System;

using Foundation;
using CallKit;

namespace MonkeyCallDirExtension
{
    [Register ("CallDirectoryHandler")]
    public class CallDirectoryHandler : CXCallDirectoryProvider, ICXCallDirectoryExtensionContextDelegate
    {
        #region Constructors
        protected CallDirectoryHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void BeginRequest (CXCallDirectoryExtensionContext context)
        {
            context.Delegate = this;

            if (!AddBlockingPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add blocking phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 1, null);
                context.CancelRequest (error);
                return;
            }

            if (!AddIdentificationPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add identification phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 2, null);
                context.CancelRequest (error);
                return;
            }

            context.CompleteRequest (null);
        }
        #endregion

        #region Private Methods
        private bool AddBlockingPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to block from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 14085555555, 18005555555 };

            foreach (var phoneNumber in phoneNumbers)
                context.AddBlockingEntry (phoneNumber);

            return true;
        }

        private bool AddIdentificationPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to identify and their identification labels from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 18775555555, 18885555555 };
            string [] labels = { "Telemarketer", "Local business" };

            for (var i = 0; i < phoneNumbers.Length; i++) {
                long phoneNumber = phoneNumbers [i];
                string label = labels [i];
                context.AddIdentificationEntry (phoneNumber, label);
            }

            return true;
        }
        #endregion

        #region Public Methods
        public void RequestFailed (CXCallDirectoryExtensionContext extensionContext, NSError error)
        {
            // An error occurred while adding blocking or identification entries, check the NSError for details.
            // For Call Directory error codes, see the CXErrorCodeCallDirectoryManagerError enum.
            //
            // This may be used to store the error details in a location accessible by the extension's containing app, so that the
            // app may be notified about errors which occurred while loading data even if the request to load data was initiated by
            // the user in Settings instead of via the app itself.
        }
        #endregion
    }
}
```

`BeginRequest` Metody obsługi katalogu wywołań będzie musiał zostać zmodyfikowane w celu zapewnienie wymaganej funkcjonalności. W przypadku powyższych próbki próbuje ustawić listę numerów zablokowana i jest dostępny w aplikacji VOIP kontaktów w bazie danych. Jakiegokolwiek powodu albo żądanie kończy się niepowodzeniem, należy utworzyć `NSError` opisujący błąd i przekazywanie ich `CancelRequest` metody `CXCallDirectoryExtensionContext` klasy.

Ustawić używanie zablokowanych numery `AddBlockingEntry` metody `CXCallDirectoryExtensionContext` klasy. Numery przekazane do metody _musi_ w kolejności rosnącej wartość liczbowa. Dla uzyskania optymalnej wydajności i użycia pamięci w przypadku się, że wiele numerów telefonów, należy wziąć pod uwagę tylko podzestaw liczb znajdujących się w danym momencie oraz za pomocą pule autorelease zwolnić obiekty przydzielone podczas każdej z partii liczb, które są ładowane.

Poinformowanie aplikacji kontaktów kontaktu liczb znane aplikacji VOIP, użyj `AddIdentificationEntry` metody `CXCallDirectoryExtensionContext` klasy i Podaj etykietę identyfikującą i numeru. Ponownie, numery, które zostały przekazane do metody _musi_ w kolejności rosnącej wartość liczbowa. Dla uzyskania optymalnej wydajności i użycia pamięci w przypadku się, że wiele numerów telefonów, należy wziąć pod uwagę tylko podzestaw liczb znajdujących się w danym momencie oraz za pomocą pule autorelease zwolnić obiekty przydzielone podczas każdej z partii liczb, które są ładowane.


## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego nowy interfejs API CallKit tego Apple wydane w ramach iOS 10 i jak ją wdrożyć w aplikacji platformy Xamarin.iOS VOIP. Pokazuje sposób CallKit umożliwia aplikacji zintegrować System z systemem iOS, jak udostępnia parzystość funkcji przy użyciu wbudowanych aplikacji (np. telefon) i jak zwiększa widoczność aplikacji w całym systemu iOS w lokalizacjach, takie jak Blokada i ekrany Narzędzia główne, za pośrednictwem Siri interakcji i za pośrednictwem aplikacje kontaktów.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
