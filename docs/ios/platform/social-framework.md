---
title: "Framework społecznościowych"
description: "Społecznościowych Framework zapewnia ujednolicony interfejs API do interakcji z sieciami społecznościowymi, łącznie z serwisem Twitter i Facebook, a także SinaWeibo dla użytkowników w Chinach."
ms.topic: article
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 7a190014abd3386a3a675d50ce6a89101d0588a7
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="social-framework"></a>Framework społecznościowych

_Społecznościowych Framework zapewnia ujednolicony interfejs API do interakcji z sieciami społecznościowymi, łącznie z serwisem Twitter i Facebook, a także SinaWeibo dla użytkowników w Chinach._


Przy użyciu platformy społecznościowych umożliwia aplikacjom komunikowanie się z sieciami społecznościowymi z jednego interfejsu API bez konieczności zarządzania uwierzytelniania. Obejmuje on system dostępne kontrolera widoku w celu tworzenia wpisów, a także abstrakcji, która umożliwia korzystanie z interfejsu API w każdej sieci społecznościowych za pośrednictwem protokołu HTTP.

> [!IMPORTANT]
> Dla interfejsu API i platform nawiązać połączenia z różnymi sieciami społecznościowymi, zobacz [Xamarin.Social](http://components.xamarin.com/view/xamarin.social/) składnika w magazynie składników Xamarin.

## <a name="connecting-to-twitter"></a>Nawiązywanie połączenia z serwisem Twitter

### <a name="twitter-account-settings"></a>Ustawienia konta w usłudze Twitter

Aby połączyć się przy użyciu platformy społecznościowych usługi Twitter, konto musi zostać skonfigurowana w ustawieniach urządzenia, jak pokazano poniżej:

 [![](social-framework-images/twitter01.png "Ustawienia konta w usłudze Twitter")](social-framework-images/twitter01.png#lightbox)

Gdy konto zostało wprowadzone i zweryfikować Twitter, aplikacji na urządzenia, które korzysta z klas Framework społecznościowych usługi Twitter dostępu do będzie używała tego konta.

### <a name="sending-tweets"></a>Wysyłanie Tweetów

Struktura społecznościowych obejmuje kontrolerze o nazwie `SLComposeViewController` przedstawiający systemu określonego widoku do edycji i wysyłanie tweet. Poniższy zrzut ekranu przedstawia przykład tego widoku:

 [![](social-framework-images/twitter02.png "Ten zrzut ekranu przedstawia przykład SLComposeViewController")](social-framework-images/twitter02.png#lightbox)

Aby użyć `SLComposeViewController` z serwisem Twitter, należy utworzyć wystąpienie kontrolera przez wywołanie metody `FromService` metody z `SLServiceType.Twitter` w sposób przedstawiony poniżej:

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

Po `SLComposeViewController` wystąpienie jest zwracane, może służyć do prezentowania interfejsu użytkownika do wysłania do usługi Twitter. Najpierw musisz jest jednak w takim przypadku sprawdza dostępność sieci społecznościowych usługi Twitter, wywołując `IsAvailable`:

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController` nigdy nie przesyła tweet bezpośrednio, bez interakcji z użytkownikiem. Jednak mogą być inicjowane z następujących metod:

-   `SetInitialText` — Dodaje początkowy tekst do wyświetlenia w tweet. 
-  `AddUrl` — Dodaje adres Url do tweeta.
-  `AddImage` — Dodaje obrazu do tweeta.


Po zainicjowany, wywoływania `PresentVIewController` Wyświetla widok utworzony przez `SLComposeViewController`. Użytkownika można opcjonalnie edycji i wysłać tweet lub Anuluj wysłaniem. W obu przypadkach kontroler powinien być ukryty w `CompletionHandler`, gdy wynik można również sprawdzić, aby sprawdzić, czy tweet został wysłany lub anulować, jak pokazano poniżej:

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>Przykład tweet

Poniższy kod przedstawia przy użyciu `SLComposeViewController` do prezentowania widok używane do wysyłania tweet:

```csharp
using System;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _twitterComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
        #endregion

        #region Computed Properties
        public bool isTwitterAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Twitter); }
        }

        public SLComposeViewController TwitterComposer {
            get { return _twitterComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            SendTweet.Enabled = isTwitterAvailable;
        }
        #endregion

        #region Actions
        partial void SendTweet_TouchUpInside (UIButton sender)
        {
            // Set initial message
            TwitterComposer.SetInitialText ("Hello Twitter!");
            TwitterComposer.AddImage (UIImage.FromFile ("Icon.png"));
            TwitterComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (TwitterComposer, true, null);
        }
        #endregion
    }
}
```

### <a name="calling-twitter-api"></a>Wywołanie interfejsu API usługi Twitter

Społecznościowych Framework obejmuje również obsługę wysyłania żądań HTTP do sieci społecznościowych. Hermetyzuje żądania w `SLRequest` klasy, która jest używana do docelowego interfejsu API określonej sieci społecznościowych.

Na przykład następujący kod wysyła żądanie do usługi Twitter można pobrać publicznego osi czasu (rozwijając na kodu podanego powyżej):

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _twitterAccount;
#endregion

#region Computed Properties
public ACAccount TwitterAccount {
    get { return _twitterAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    SendTweet.Enabled = isTwitterAvailable;
    RequestTwitterTimeline.Enabled = false;

    // Initialize Twitter Account access 
    var accountStore = new ACAccountStore ();
    var accountType = accountStore.FindAccountType (ACAccountType.Twitter);

    // Request access to Twitter account
    accountStore.RequestAccess (accountType, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestTwitterTimeline.Enabled = true;
            });
        }
    });
}
#endregion

#region Actions
partial void RequestTwitterTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
    var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = TwitterAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Przyjrzyjmy się ten kod szczegółowo. Po pierwsze uzyskuje dostęp do konta magazynu i pobiera typ konta w usłudze Twitter:

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

Następnie go pyta użytkownika, jeśli aplikacja może mieć dostęp do konta w usłudze Twitter i udzielenie dostępu do konta zostanie załadowana do pamięci i Interfejsie użytkownika aktualizacji:

```csharp
// Request access to Twitter account
accountStore.RequestAccess (accountType, (granted, error) => {
    // Allowed by user?
    if (granted) {
        // Get account
        _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
        InvokeOnMainThread (() => {
            // Update UI
            RequestTwitterTimeline.Enabled = true;
        });
    }
});
```

Gdy użytkownik żąda danych osi czasu (naciskając przycisk w interfejsie użytkownika), aplikacji najpierw tworzy żądanie dostępu do danych z serwisem Twitter:

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```
W tym przykładzie jest ograniczenie zwrócone wyniki dziesięć ostatnich wpisów umieszczając `?count=10` w adresie URL. Na koniec dołącza żądanie do konta w usłudze Twitter, (który załadowano powyżej) i wykonuje wywołanie Twitter do pobierania danych:

```csharp
// Request data
request.Account = TwitterAccount;
request.PerformRequest ((data, response, error) => {
    // Was there an error?
    if (error == null) {
        // Was the request successful?
        if (response.StatusCode == 200) {
            // Yes, display it
            InvokeOnMainThread (() => {
                Results.Text = data.ToString ();
            });
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", response.StatusCode);
            });
        }
    } else {
        // No, display error
        InvokeOnMainThread (() => {
            Results.Text = string.Format ("Error: {0}", error);
        });
    }
});
```

Jeśli dane zostały pomyślnie załadowane, zostanie wyświetlony nieprzetworzone dane JSON (jak przykład danych wyjściowych poniżej):

[![](social-framework-images/twitter03.png "Przykład wyświetlania nieprzetworzone dane JSON")](social-framework-images/twitter03.png#lightbox)

W rzeczywistym aplikacji wyniki JSON następnie może być analizowana jako normalne i wyświetlane wyniki. Zobacz [usług sieci Web wprowadzenie](~/cross-platform/data-cloud/web-services/index.md) informacji na temat sposobu przeanalizować składni JSON.

## <a name="connecting-to-facebook"></a>Nawiązywanie połączenia z serwisem Facebook

### <a name="facebook-account-settings"></a>Ustawienia konta w usłudze Facebook

Nawiązywanie połączenia z usługi Facebook społecznościowych Framework jest niemal identyczna z proces wykorzystywany do usługi Twitter przedstawionych powyżej. Konto użytkownika usługi Facebook muszą być skonfigurowane w ustawieniach urządzenia, jak pokazano poniżej:

[![](social-framework-images/facebook01.png "Ustawienia konta w usłudze Facebook")](social-framework-images/facebook01.png#lightbox)

Po skonfigurowaniu aplikacji na urządzenia, których używa społecznościowych Framework użyje tego konta nawiązywania połączenia z serwisem Facebook.

### <a name="posting-to-facebook"></a>Zamieszczając w serwisie Facebook

Społecznościowych Framework jest ujednolicony interfejs API, umożliwiającą dostęp do wielu sieci społecznościowych, kod pozostaje niemal identyczne, niezależnie od tego, sieci społecznościowych używane.

Na przykład `SLComposeViewController` można dokładnie tak jak w przykładzie Twitter, wcześniej, wyświetlane jest tylko różnych przełączanie do ustawień specyficznych dla usługi Facebook i opcje. Na przykład:

```csharp
using System;
using Foundation;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _facebookComposer = SLComposeViewController.FromService (SLServiceType.Facebook);
        #endregion

        #region Computed Properties
        public bool isFacebookAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Facebook); }
        }

        public SLComposeViewController FacebookComposer {
            get { return _facebookComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            PostToFacebook.Enabled = isFacebookAvailable;
        }
        #endregion

        #region Actions
        partial void PostToFacebook_TouchUpInside (UIButton sender)
        {
            // Set initial message
            FacebookComposer.SetInitialText ("Hello Facebook!");
            FacebookComposer.AddImage (UIImage.FromFile ("Icon.png"));
            FacebookComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (FacebookComposer, true, null);
        }
        #endregion
    }
}
```

W przypadku użycia z usługą Facebook, `SLComposeViewController` Wyświetla widok, która wygląda podobnie jak w przykładzie Twitter przedstawiający **Facebook** jako tytuł w tym przypadku:

[![](social-framework-images/facebook02.png "Wyświetlanie SLComposeViewController")](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Wywołanie interfejsu API programu Graph usługi Facebook

Podobnie jak Twitter przykład: w ramach społecznościowych w `SLRequest` obiekt może być używany z graph API usługi Facebook na. Na przykład następujący kod zwraca informacje z interfejs API graph o koncie Xamarin (rozwijając na kodu podanego powyżej):

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _facebookAccount;
#endregion

#region Computed Properties
public ACAccount FacebookAccount {
    get { return _facebookAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    PostToFacebook.Enabled = isFacebookAvailable;
    RequestFacebookTimeline.Enabled = false;

    // Initialize Facebook Account access 
    var accountStore = new ACAccountStore ();
    var options = new AccountStoreOptions ();
    var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
    accountType = accountStore.FindAccountType (ACAccountType.Facebook);

    // Request access to Facebook account
    accountStore.RequestAccess (accountType, options, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _facebookAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestFacebookTimeline.Enabled = true;
            });
        }
    });

}
#endregion

#region Actions
partial void RequestFacebookTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl ("https://graph.facebook.com/283148898401104");
    var request = SLRequest.Create (SLServiceKind.Facebook, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = FacebookAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Tylko rzeczywistych różnica między ten kod i wersja Twitter przedstawionych powyżej, jest wymagane w serwisie Facebook uzyskanie Developer/określonych identyfikator aplikacji (które można wygenerować z portalu dla deweloperów w serwisie Facebook), który musi być ustawiona jako opcję podczas zgłaszania żądania:

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

Nie można ustawić tę opcję (lub przy użyciu nieprawidłowy klucz) spowoduje błąd lub żadne dane są zwracane.

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak użyć społecznościowych Framework do interakcji z serwisów Facebook i Twitter. Potem where do konfiguracji kont dla każdej sieci społecznościowych w ustawieniach urządzenia. Również omówiony sposób użycia `SLComposeViewController` do prezentowania ujednoliconego podglądu dla publikowanie do sieci społecznościowych. Ponadto zbadać `SLRequest` klasy, która jest używana do wywołania interfejsu API w każdej sieci społecznościowych.


## <a name="related-links"></a>Linki pokrewne

- [SocialFrameworkDemo (przykład)](https://developer.xamarin.com/samples/SocialFrameworkDemo/)
- [Wprowadzenie do usług sieci Web](~/cross-platform/data-cloud/web-services/index.md)
