---
title: "Wskazówki — przy użyciu Usługa transferu w tle i NSURLSession"
description: "W tym przewodniku używamy Usługa transferu w tle i NSURLSession interfejsu API można rozpocząć wyłączyć pobieranie duży obraz, który można pobrać, gdy aplikacja jest w tle."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 4ab11239caf5986bba52f080945d90a91ea9453e
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="walkthrough---using-background-transfer-service-and-nsurlsession"></a>Wskazówki — przy użyciu Usługa transferu w tle i NSURLSession

_W tym przewodniku używamy Usługa transferu w tle i NSURLSession interfejsu API można rozpocząć wyłączyć pobieranie duży obraz, który można pobrać, gdy aplikacja jest w tle._

Transferu w tle jest inicjowane przez skonfigurowanie tło `NSURLSession` i enqueuing przekazywanie lub pobieranie zadań. Jeśli zadania ukończone podczas backgrounded, wstrzymane lub przerwane aplikacji, iOS powiadomi aplikacji przez wywołanie metody obsługi uzupełniania w aplikacji *AppDelegate*. Poniższy diagram ilustruje to w akcji:

 [![](background-transfer-walkthrough-images/transfer.png "Transferu w tle jest inicjowane przez skonfigurowanie tła NSURLSession i enqueuing przekazywanie lub pobieranie zadań")](background-transfer-walkthrough-images/transfer.png#lightbox)

Zobaczmy, to wygląda podobnie jak w kodzie.

## <a name="configuring-a-background-session"></a>Konfigurowanie sesji tła

Aby sesji tła, Utwórz nową `NSUrlSession` i konfigurowanie ich za pomocą `NSUrlSessionConfiguration` obiektu.

Określa obiekt konfiguracji, co można zrobić w sesji i rodzaje zadań można je uruchomić.
Sesje skonfigurowany za pomocą `CreateBackgroundSessionConfiguration` metody w oddzielnych procesach, a następnie wykonuje DACL transferów (Wi-Fi), aby zachować dane i baterii.
W poniższym przykładzie kodu pokazano prawidłową konfigurację tła transfer sesji przy użyciu `CreateBackgroundSessionConfiguration` — metoda i unikatowym ciągiem identyfikatora:

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, (NSUrlSessionDelegate) new MySessionDelegate, new NSOperationQueue());

}
```

Oprócz obiekt konfiguracji sesji wymaga również delegata sesji i kolejki.
Kolejki określa kolejność wykonać zadania. Delegat sesji chaperones proces przesyłania i uwierzytelniania dojść, buforowanie i inne problemy związane z sesji.

## <a name="working-with-tasks-and-delegates"></a>Praca z zadaniami i Delegaty

Teraz, gdy skonfigurowano możemy sesji tła, teraz rozpocząć poza zadań w celu obsługi przeniesienia. Firma Microsoft może przechowywać informacje o tych zadań za pomocą `NSUrlSessionDelegate` wystąpienia o nazwie delegata sesji. Delegat sesji jest zakończone lub wstrzymania aplikacji w tle do obsługi uwierzytelniania, błędy lub zakończeniu transferu wznowienie pracy.

`NSUrlSessionDelegate` Udostępnia następujące metody podstawowej, aby sprawdzić stan transferu:

-  *DidFinishEventsForBackgroundSession* — ta metoda jest wywoływana po zakończeniu wszystkich zadań i zakończeniu transferu.
-  *DidReceiveChallenge* — wywołane do żądania poświadczeń, gdy jest wymagana autoryzacja.
-  *DidBecomeInvalidWithError* — Jeśli o nazwie `NSURLSession` staje się unieważnione.


Sesje tła wymagają wyspecjalizowanego delegatów w zależności od typów zadań, które są uruchomione. Sesje w tle są ograniczone do dwa rodzaje zadań:

-  *Przekaż zadania* -zadania typu `NSUrlSessionUploadTask` użyj `NSUrlSessionTaskDelegate` , który dziedziczy z `NSUrlSessionDelegate` . Ten delegat zapewnia dodatkowe metody do śledzenia Przekaż postępu, Przekierowanie HTTP dojścia i inne.
-  *Pobierz zadania* -zadania typu `NSUrlSessionDownloadTask` użyj `NSUrlSessionDownloadDelegate` , który dziedziczy z `NSUrlSessionTaskDelegate` . Ten delegat zawiera Przekaż wszystkie metody dla zadań, a także metody specyficzne dla pobierania można śledzić postęp pobierania i określania, kiedy wznowione lub wykonać zadania pobierania.


Poniższy kod definiuje klasę task, która może służyć do pobierania obrazu z adresu URL. Możemy rozpocząć wyłączyć zadania, wywołując `CreateDownloadTask` w naszym sesji tła i przekazywanie żądanie adresu URL:

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

Następnie utworzymy delegata pobierania nowej sesji, aby śledzić wszystkie zadania pobierania w tej sesji:

```csharp
public class MySessionDelegate : NSUrlSessionDownloadDelegate
{
  public override void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
  {
    Console.WriteLine (string.Format ("DownloadTask: {0}  progress: {1}", downloadTask, progress));
    InvokeOnMainThread( () => {
      // update UI with progress bar, if desired
    });
  }
  ...
}
```

Jeśli chcemy sprawdzić postęp zadania ładowania, możemy zastąpić `DidWriteData` metodę, aby śledzić postęp, a nawet aktualizacji interfejsu użytkownika. Aktualizacje interfejsu użytkownika będzie wyświetlany natychmiast, jeśli aplikacja działa na pierwszym planie lub będzie oczekiwania dla użytkownika, przy następnym otwarciu aplikacji.

API delegata sesji udostępnia szerokie zestawu narzędzi do interakcji z zadania. Aby uzyskać pełną listę sesji delegować metody, zapoznaj się `NSUrlSessionDelegate` dokumentacji interfejsu API.

> [!IMPORTANT]
> Sesje tła są uruchamiane w wątku w tle, co wezwań do zaktualizowania interfejsu użytkownika musi jawnie uruchamiane w wątku interfejsu użytkownika, wywołując `InvokeOnMainThread` w celu uniknięcia przerywanie aplikacji systemu iOS. 


## <a name="handling-transfer-completion"></a>Obsługa uzupełniania transferu

Ostatnim krokiem jest umożliwienie aplikacji wiedzieć, kiedy zakończone wszystkie zadania związane z sesją i obsługiwać nową zawartość.

W *AppDelegate*, subskrybować `HandleEventsForBackgroundUrl` zdarzeń. Po przejściu tło w aplikacji i sesji transferu jest uruchomiony, ta metoda jest wywoływana i system przekazuje nam obsługi zakończenia:

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

Aby dowiedzieć się, gdy jest wykonywane naszej aplikacji systemu iOS używamy programu obsługi zakończenia przetwarzania.

Odwołaj, że sesji można zduplikować kilka zadań w celu przetworzenia transferu. Po ukończeniu zadania ostatniego wstrzymania lub zakończone aplikacji jest ponownie uruchomiona w tle. Następnie aplikacja ponownie łączy się z `NSURLSession` przy użyciu identyfikator unikatowy sesji i wywołań `DidFinishEventsForBackgroundSession` w delegacie sesji. Ta metoda jest możliwość obsługi nowej zawartości, w tym aktualizacja interfejsu użytkownika w celu odzwierciedlenia wyniki transferu aplikacji:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

Gdy skończymy obsługi nowej zawartości nazywamy obsługi uzupełniania aby wiedzieć, że jest bezpieczne migawki aplikacji i wrócić do poprzedniej strony w stan uśpienia system:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  var appDelegate = UIApplication.SharedApplication.Delegate as AppDelegate;

  // Handle new information, update UI, etc.

  // call completion handler when you're done
  if (appDelegate.backgroundSessionCompletionHandler != null) {
    NSAction handler = appDelegate.backgroundSessionCompletionHandler;
    appDelegate.backgroundSessionCompletionHandler = null;
    handler.Invoke ();
  }
}
```

W tym przewodniku możemy opisano podstawowe kroki, aby zaimplementować Usługa transferu w tle w systemie iOS 7.



## <a name="related-links"></a>Linki pokrewne

- [Proste transferu w tle (przykład)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
