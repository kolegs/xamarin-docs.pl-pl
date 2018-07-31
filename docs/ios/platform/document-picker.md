---
title: Selektor dokumentów w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano selektora dokumentów dla systemu iOS i w tym artykule omówiono sposób używania go w rozszerzeniu Xamarin.iOS. Zajmuje się w usłudze iCloud, dokumentów, wspólny kod instalacji i rozszerzenia usługodawcy dokumentu.
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: ca0c7a6e655fdc44aa673a59be71bc83044d3085
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353337"
---
# <a name="document-picker-in-xamarinios"></a>Selektor dokumentów w rozszerzeniu Xamarin.iOS

Selektor dokumentów umożliwia dokumentów można udostępniać między aplikacjami. Te dokumenty mogą być przechowywane w usłudze iCloud lub w katalogu innej aplikacji. Dokumenty są udostępniane za pośrednictwem zestawu [dokumentu dostawcy rozszerzeń](~/ios/platform/extensions.md) użytkownik ma zainstalowaną na swoim urządzeniu. 

Ze względu na trudności utrzymania dokumentów synchronizowane aplikacje i w chmurze ich używaniem wiążą się pewne złożoności niezbędne.

## <a name="requirements"></a>Wymagania

Aby wykonać kroki przedstawione w tym artykule niezbędne jest, następujące elementy:

-  **Xcode 7 i iOS 8 lub nowszego** — Xcode 7 i iOS 8 lub nowszych interfejsów API firmy Apple muszą być zainstalowane i skonfigurowane na komputerze dewelopera.
-  **Visual Studio lub Visual Studio dla komputerów Mac** — należy zainstalować najnowszą wersję programu Visual Studio dla komputerów Macintosh.
-  **iOS Device** — urządzenia z systemem iOS z systemem iOS 8 lub nowszy.

## <a name="changes-to-icloud"></a>Zmiany w usłudze iCloud

Aby wdrożyć nowe funkcje selektora dokumentów, firmy Apple w usłudze iCloud usługi wprowadzono następujące zmiany:

-  Demon w usłudze iCloud ma została napisana przy użyciu CloudKit.
-  Istniejące usługi iCloud, które funkcje zostały zmienić nazwy dysku w usłudze iCloud.
-  Obsługa systemu operacyjnego Windows firmy Microsoft, dodano do usługi iCloud.
-  W programie Finder systemu operacyjnego Mac dodano folder w usłudze iCloud.
-  urządzenia z systemem iOS można uzyskać dostęp do zawartości folderu w usłudze iCloud systemu Mac OS.

> [!IMPORTANT]
> Apple [udostępnia narzędzia](https://developer.apple.com/support/allowing-users-to-manage-data/) pomagające deweloperom poprawnie obsługiwać Unii Europejskiej ogólnego danych (GDPR Protection Regulation).

## <a name="what-is-a-document"></a>Co to jest dokument?

Przy odwoływaniu się do dokumentu w usłudze iCloud, jest jednostką pojedynczy, autonomicznej i powinien postrzegane jako takie przez użytkownika. Użytkownik może chcieć modyfikowanie dokumentu lub udostępnić go innym użytkownikom (przy użyciu poczty e-mail, na przykład).

Istnieje kilka typów plików, że użytkownik natychmiast rozpozna jako dokumentów, takich jak strony, prezentacji lub liczby plików. Jednak w usłudze iCloud nie jest ograniczona do tę koncepcję. Na przykład stan gry (na przykład dopasowanie gra) można traktowane jako dokument i przechowywane w usłudze iCloud. Ten plik może być przekazywane między urządzeniami użytkownika i zezwolić im na przejmą gra którym ją przerwał na innym urządzeniu.

## <a name="dealing-with-documents"></a>Rozwiązywania problemów związanych z dokumentami

Przed zagłębieniem się w kod wymagany do selektora dokumentów za pomocą platformy Xamarin, w tym artykule zamierza obejmują najlepsze rozwiązania dotyczące pracy z dokumentami w usłudze iCloud i kilka zmian wprowadzonych do istniejących interfejsów API wymagane do obsługi selektora dokumentów.

### <a name="using-file-coordination"></a>Przy użyciu pliku koordynacji

Ponieważ plik można modyfikować z różnych miejsc, koordynacja musi służyć do zapobiegania utracie danych.

 [![](document-picker-images/image1.png "Przy użyciu pliku koordynacji")](document-picker-images/image1.png#lightbox)

Spójrzmy na powyższej ilustracji:

1.  Urządzenia z systemem iOS przy użyciu pliku koordynacji tworzy nowy dokument i zapisuje go do folderu w usłudze iCloud.
2.  w usłudze iCloud zapisuje zmodyfikowanych plików do chmury na potrzeby dystrybucji dla każdego urządzenia.
3.  Dołączone Mac widzi zmodyfikowany plik w folderze w usłudze iCloud i używa pliku koordynacji, aby skopiować zmiany do pliku.
4.  Urządzenia nie przy użyciu pliku koordynacji wprowadza zmianę do pliku i zapisuje go do folderu w usłudze iCloud. Te zmiany są natychmiast replikowane do innych urządzeń.

Przyjęto założenie, oryginalnym urządzenia z systemem iOS lub Mac został edycję pliku, teraz utracone i zastąpione wersję pliku z urządzenia nieskoordynowane swoje zmiany. Aby zapobiec utracie danych, koordynacja pliku to podczas pracy z dokumentami oparte na chmurze.

### <a name="using-uidocument"></a>Za pomocą UIDocument

 `UIDocument` upraszcza czynności (lub `NSDocument` w systemie macOS), wykonując wszystkie skomplikowanymi dla deweloperów. Zapewnia wbudowane koordynacji plików za pomocą kolejek tła, aby zapobiec blokuje interfejsu użytkownika aplikacji.

 `UIDocument` udostępnia wiele wymaga wysokiego poziomu interfejsów API, które ułatwiają pracach deweloperskich tej aplikacji platformy Xamarin na cel dewelopera.

Poniższy kod tworzy podklasą `UIDocument` do zaimplementowania ogólny dokument tekstowy, który może służyć do przechowywania i pobierania tekstu z usługi iCloud:

```csharp
using System;
using Foundation;
using UIKit;

namespace DocPicker
{
    public class GenericTextDocument : UIDocument
    {
        #region Private Variable Storage
        private NSString _dataModel;
        #endregion

        #region Computed Properties
        public string Contents {
            get { return _dataModel.ToString (); }
            set { _dataModel = new NSString(value); }
        }
        #endregion

        #region Constructors
        public GenericTextDocument (NSUrl url) : base (url)
        {
            // Set the default document text
            this.Contents = "";
        }

        public GenericTextDocument (NSUrl url, string contents) : base (url)
        {
            // Set the default document text
            this.Contents = contents;
        }
        #endregion

        #region Override Methods
        public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Were any contents passed to the document?
            if (contents != null) {
                _dataModel = NSString.FromData( (NSData)contents, NSStringEncoding.UTF8 );
            }

            // Inform caller that the document has been modified
            RaiseDocumentModified (this);

            // Return success
            return true;
        }

        public override NSObject ContentsForType (string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Convert the contents to a NSData object and return it
            NSData docData = _dataModel.Encode(NSStringEncoding.UTF8);
            return docData;
        }
        #endregion

        #region Events
        public delegate void DocumentModifiedDelegate(GenericTextDocument document);
        public event DocumentModifiedDelegate DocumentModified;

        internal void RaiseDocumentModified(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentModified != null) {
                this.DocumentModified (document);
            }
        }
        #endregion
    }
}
```

`GenericTextDocument` Klasy przedstawiony powyżej będą używane w tym artykule podczas pracy z selektora dokumentów i zewnętrzne dokumenty w aplikacji platformy Xamarin.iOS 8.

## <a name="asynchronous-file-coordination"></a>Koordynacja asynchroniczne pliku

System iOS 8 oferuje następujące nowe funkcje asynchroniczne koordynacji pliku przy użyciu nowych interfejsów API koordynacji pliku. Przed system iOS 8 wszystkich istniejących plików koordynacji interfejsów API zostały całkowicie synchroniczne. Oznacza to, że deweloper był odpowiedzialny za wdrażanie własnych tła kolejkowania wiadomości, aby uniemożliwić koordynacji pliku blokuje interfejsu użytkownika aplikacji.

Nowy `NSFileAccessIntent` klasy zawiera adres URL wskazuje plik i kilka opcji, aby kontrolować typ koordynacji wymagane. Poniższy przykład demonstruje przenoszenie plików z jednej lokalizacji do innej przy użyciu opcji:

```csharp
// Get source options
var srcURL = NSUrl.FromFilename ("FromFile.txt");
var srcIntent = NSFileAccessIntent.CreateReadingIntent (srcURL, NSFileCoordinatorReadingOptions.ForUploading);

// Get destination options
var dstURL = NSUrl.FromFilename ("ToFile.txt");
var dstIntent = NSFileAccessIntent.CreateReadingIntent (dstURL, NSFileCoordinatorReadingOptions.ForUploading);

// Create an array
var intents = new NSFileAccessIntent[] {
    srcIntent,
    dstIntent
};

// Initialize a file coordination with intents
var queue = new NSOperationQueue ();
var fileCoordinator = new NSFileCoordinator ();
fileCoordinator.CoordinateAccess (intents, queue, (err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}",err.LocalizedDescription);
    }
});
```

## <a name="discovering-and-listing-documents"></a>Odnajdywanie i wyświetlania listy dokumentów

Sposób odnajdywania i wyświetlanie listy dokumentów jest przy użyciu istniejących `NSMetadataQuery` interfejsów API. W tej sekcji opisano nowe funkcje dodane do `NSMetadataQuery` , sprawiają, że praca z dokumentami jeszcze łatwiejsze niż przedtem.

### <a name="existing-behavior"></a>Istniejące zachowanie

Przed system iOS 8 `NSMetadataQuery` spadła do pobrania pliku lokalnego zmian takich jak: usuwa, tworzy i zmienia nazwę.

 [![](document-picker-images/image2.png "Omówienie zmiany pliku lokalnego NSMetadataQuery")](document-picker-images/image2.png#lightbox)

Na powyższym diagramie:

1.  Dla plików, które już istnieją w kontenerze aplikacji `NSMetadataQuery` ma istniejących `NSMetadata` rekordów wstępnie utworzone i buforowane, dzięki czemu są one dostępne natychmiast dla aplikacji.
1.  Aplikacja tworzy nowy plik w kontenerze aplikacji.
1.  Występuje opóźnienie przed `NSMetadataQuery` widzi modyfikację kontener aplikacji i tworzy wymagane `NSMetadata` rekordu.


Ze względu na opóźnienie do tworzenia `NSMetadata` rekordu, aplikacja musi mieć dwa danych źródła Otwórz: jeden dla zmian plików lokalnych i w chmurze na podstawie zmian.

### <a name="stitching"></a>Łączenie

W systemie iOS 8 `NSMetadataQuery` jest łatwiejszy w obsłudze bezpośrednio z nową funkcję o nazwie łączeń:

 [![](document-picker-images/image3.png "NSMetadataQuery dzięki nowej funkcji o nazwie łączeń")](document-picker-images/image3.png#lightbox)

Przy użyciu łączeń na powyższym diagramie:

1.  Tak jak poprzednio, dla plików, które już istnieją w kontenerze aplikacji `NSMetadataQuery` ma istniejących `NSMetadata` rekordów wstępnie utworzone i buforowane.
1.  Aplikacja tworzy nowy plik w kontenerze aplikacji przy użyciu pliku koordynacji.
1.  Zaczepienia w kontenerze aplikacji widzi modyfikacji i wywołania `NSMetadataQuery` można utworzyć wymaganych `NSMetadata` rekordu.
1.  `NSMetadata` Rekord został utworzony bezpośrednio po pliku i jest udostępniana aplikacja.


Za pomocą łączeń aplikacja nie ma już można otworzyć źródła danych do monitorowania lokalnego i zmiany plików w chmurze. Teraz aplikacja może polegać na `NSMetadataQuery` bezpośrednio.

> [!IMPORTANT]
> Łączenie — tylko wtedy, gdy aplikacja używa pliku koordynacji przedstawionych w powyższej sekcji. Jeśli koordynacji pliku nie jest używany, interfejsy API domyślnie istniejącego zachowania systemu iOS jest 8 wstępnie.




### <a name="new-ios-8-metadata-features"></a>Nowe funkcje metadanych dla systemu iOS 8

Następujące nowe funkcje zostały dodane do `NSMetadataQuery` w systemie iOS 8:

-   `NSMetatadataQuery` Teraz można wyświetlić listę-local dokumentów przechowywanych w chmurze.
-  Nowe interfejsy API zostały dodane do dostępu do informacji o metadanych w dokumentach oparte na chmurze. 
-  Pojawiły się nowe `NSUrl_PromisedItems` interfejsu API, który będzie można uzyskać dostępu do atrybutów pliku plików, które mogą lub nie może mieć ich zawartości, które są dostępne lokalnie.
-  Użyj `GetPromisedItemResourceValue` metodę, aby uzyskać informacje na temat danego pliku lub użyj `GetPromisedItemResourceValues` metodę, aby uzyskać informacje na więcej niż jeden plik jednocześnie.


Dwie nowe flagi koordynacji plików zostały dodane do radzenia sobie z metadanymi:

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


Przy użyciu flag powyżej zawartość pliku dokumentu nie muszą być dostępne lokalnie dla nich ma być używany.

Następujący segment kodu ilustruje sposób używania `NSMetadataQuery` zapytania istnieje określony plik i utworzyć plik, jeśli nie istnieje:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;


#region Static Properties
public const string TestFilename = "test.txt"; 
#endregion

#region Computed Properties
public bool HasiCloud { get; set; }
public bool CheckingForiCloud { get; set; }
public NSUrl iCloudUrl { get; set; }

public GenericTextDocument Document { get; set; }
public NSMetadataQuery Query { get; set; }
#endregion

#region Private Methods
private void FindDocument () {
    Console.WriteLine ("Finding Document...");

    // Create a new query and set it's scope
    Query = new NSMetadataQuery();
    Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

    // Build a predicate to locate the file by name and attach it to the query
    var pred = NSPredicate.FromFormat ("%K == %@"
        , new NSObject[] {
            NSMetadataQuery.ItemFSNameKey
            , new NSString(TestFilename)});
    Query.Predicate = pred;

    // Register a notification for when the query returns
    NSNotificationCenter.DefaultCenter.AddObserver (this,
            new Selector("queryDidFinishGathering:"),           NSMetadataQuery.DidFinishGatheringNotification,
            Query);

    // Start looking for the file
    Query.StartQuery ();
    Console.WriteLine ("Querying: {0}", Query.IsGathering);
}

[Export("queryDidFinishGathering:")]
public void DidFinishGathering (NSNotification notification) {
    Console.WriteLine ("Finish Gathering Documents.");

    // Access the query and stop it from running
    var query = (NSMetadataQuery)notification.Object;
    query.DisableUpdates();
    query.StopQuery();

    // Release the notification
    NSNotificationCenter.DefaultCenter.RemoveObserver (this
        , NSMetadataQuery.DidFinishGatheringNotification
        , query);

    // Load the document that the query returned
    LoadDocument(query);
}

private void LoadDocument (NSMetadataQuery query) {
    Console.WriteLine ("Loading Document...");    

    // Take action based on the returned record count
    switch (query.ResultCount) {
    case 0:
        // Create a new document
        CreateNewDocument ();
        break;
    case 1:
        // Gain access to the url and create a new document from
        // that instance
        NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

        // Load the document
        OpenDocument (url);
        break;
    default:
        // There has been an issue
        Console.WriteLine ("Issue: More than one document found...");
        break;
    }
}
#endregion

#region Public Methods
public void OpenDocument(NSUrl url) {

    Console.WriteLine ("Attempting to open: {0}", url);
    Document = new GenericTextDocument (url);

    // Open the document
    Document.Open ( (success) => {
        if (success) {
            Console.WriteLine ("Document Opened");
        } else
            Console.WriteLine ("Failed to Open Document");
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public void CreateNewDocument() {
    // Create path to new file
    // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
    var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
    var docPath = Path.Combine (docsFolder, TestFilename);
    var ubiq = new NSUrl (docPath, false);

    // Create new document at path 
    Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
    Document = new GenericTextDocument (ubiq);

    // Set the default value
    Document.Contents = "(default value)";

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
        Console.WriteLine ("Save completion:" + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
        } else {
            Console.WriteLine ("Unable to Save Document");
        }
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public bool SaveDocument() {
    bool successful = false;

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
        Console.WriteLine ("Save completion: " + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
            successful = true;
        } else {
            Console.WriteLine ("Unable to Save Document");
            successful=false;
        }
    });

    // Return results
    return successful;
}
#endregion

#region Events
public delegate void DocumentLoadedDelegate(GenericTextDocument document);
public event DocumentLoadedDelegate DocumentLoaded;

internal void RaiseDocumentLoaded(GenericTextDocument document) {
    // Inform caller
    if (this.DocumentLoaded != null) {
        this.DocumentLoaded (document);
    }
}
#endregion
```

### <a name="document-thumbnails"></a>Dostępność miniatur dokumentów

Apple czuje, że najlepsze środowisko użytkownika podczas wyświetlania listy dokumentów dla aplikacji jest użycie wersji zapoznawczych. Daje to kontekst użytkowników końcowych, dzięki czemu mogą szybko identyfikować dokument, który chcą pracować.

Przed system iOS 8 wyświetlane podglądy dokumentów wymagana implementacja niestandardowa. Jesteś nowym użytkownikiem z systemem iOS 8 są atrybuty systemu plików, które umożliwiają deweloperom szybko pracować z dostępność miniatur dokumentów.

#### <a name="retrieving-document-thumbnails"></a>Trwa pobieranie dostępność miniatur dokumentów 

Przez wywołanie metody `GetPromisedItemResourceValue` lub `GetPromisedItemResourceValues` metod `NSUrl_PromisedItems` interfejsu API, `NSUrlThumbnailDictionary`, jest zwracana. Jest tylko klucz obecnie w tym słowniku `NSThumbnial1024X1024SizeKey` i jego pasującą `UIImage`.

#### <a name="saving-document-thumbnails"></a>Zapisywanie dostępność miniatur dokumentów

Najprostszym sposobem, aby zapisać miniaturę polega na użyciu `UIDocument`. Przez wywołanie metody `GetFileAttributesToWrite` metody `UIDocument` i ustawienie miniatury, zostanie automatycznie zapisana po pliku dokumentu. Demon w usłudze iCloud zostaną Zobacz tej zmiany i jej są propagowane do usługi iCloud. W systemie Mac OS X miniatury są generowane automatycznie dla deweloperów przez wtyczkę szybkiego wyszukiwania.

Za pomocą podstawy pracy z dokumentami iCloud oparte na miejscu, wraz z modyfikacje do istniejących interfejsów API, jesteśmy gotowi, wdrożyć kontroler widoku selektora dokumentów na platformie Xamarin dla systemu iOS 8 aplikacji mobilnej.


## <a name="enabling-icloud-in-xamarin"></a>Włączanie usługi iCloud w środowisku Xamarin

Zanim selektora dokumentów, można użyć w aplikacji platformy Xamarin.iOS, pomocy technicznej w ramach usługi iCloud musi być włączone zarówno w aplikacji, jak i za pośrednictwem firmy Apple. 

Poniższe kroki przewodnika proces inicjowania obsługi dla usługi iCloud.

1. Tworzenie kontenera w usłudze iCloud.
2. Utwórz identyfikator aplikacji, który zawiera usługi App Service w usłudze iCloud.
3. Utwórz profil inicjowania obsługi, który zawiera ten identyfikator aplikacji.

[Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) przewodnik przeprowadzi Cię dwa pierwsze kroki. Aby utworzyć profil inicjowania obsługi administracyjnej, wykonaj kroki opisane w [profilu aprowizacji](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) przewodnik.



Poniższe kroki przewodnika proces konfigurowania aplikacji na potrzeby usługi iCloud:

Wykonaj następujące czynności:

1.  Otwórz projekt w programie Visual Studio for Mac lub Visual Studio.
2.  W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz pozycję Opcje.
3.  W oknie dialogowym Opcje wybierz **aplikacja dla systemu iOS**, upewnij się, że **identyfikatora pakietu** jest zgodna ze strukturą, która została zdefiniowana w **Identyfikatora aplikacji** utworzony powyżej dla aplikacji. 
4.  Wybierz **podpisywanie pakietu systemu iOS**, wybierz opcję **tożsamość dewelopera** i **profilu aprowizacji** utworzonego powyżej.
5.  Kliknij przycisk **OK** przycisk, aby zapisać zmiany i zamknąć okno dialogowe.
6.  Kliknij prawym przyciskiem myszy `Entitlements.plist` w **Eksploratora rozwiązań** aby go otworzyć w edytorze.

    > [!IMPORTANT]
    > W programie Visual Studio może być konieczne Otwórz Edytor uprawnień, kliknij prawym przyciskiem myszy, wybierając **Otwórz za pomocą...** i wybierając pozycję Edytor listy właściwości

7.  Sprawdź **Włącz usługę iCloud** , **dokumenty iCloud** , **magazyn kluczy i wartości** i **CloudKit** .
8.  Upewnij się, **kontenera** istnieje dla aplikacji (tak jak utworzono powyżej). Przykład: `iCloud.com.your-company.AppName`
9.  Zapisz zmiany w pliku.

Aby uzyskać więcej informacji na temat uprawnień dotyczą [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

Z powyższymi ustawieniami w miejscu aplikacji mogą teraz używać chmurowych dokumentów i nowy kontroler widoku selektora dokumentów.

## <a name="common-setup-code"></a>Wspólny kod instalacji

Przed rozpoczęciem pracy z kontrolera widoku selektora dokumentów, istnieje pewne ustawienia standardowe kod jest wymagany. Rozpoczynanie pracy od modyfikowania aplikacji `AppDelegate.cs` plików i przypisz ją wyglądać podobnie do następującego:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;

namespace DocPicker
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Static Properties
        public const string TestFilename = "test.txt"; 
        #endregion

        #region Computed Properties
        public override UIWindow Window { get; set; }
        public bool HasiCloud { get; set; }
        public bool CheckingForiCloud { get; set; }
        public NSUrl iCloudUrl { get; set; }

        public GenericTextDocument Document { get; set; }
        public NSMetadataQuery Query { get; set; }
        public NSData Bookmark { get; set; }
        #endregion

        #region Private Methods
        private void FindDocument () {
            Console.WriteLine ("Finding Document...");

            // Create a new query and set it's scope
            Query = new NSMetadataQuery();
            Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

            // Build a predicate to locate the file by name and attach it to the query
            var pred = NSPredicate.FromFormat ("%K == %@",
                new NSObject[] {NSMetadataQuery.ItemFSNameKey
                , new NSString(TestFilename)});
            Query.Predicate = pred;

            // Register a notification for when the query returns
            NSNotificationCenter.DefaultCenter.AddObserver (this
                , new Selector("queryDidFinishGathering:")
                , NSMetadataQuery.DidFinishGatheringNotification
                , Query);

            // Start looking for the file
            Query.StartQuery ();
            Console.WriteLine ("Querying: {0}", Query.IsGathering);
        }


        [Export("queryDidFinishGathering:")]
        public void DidFinishGathering (NSNotification notification) {
            Console.WriteLine ("Finish Gathering Documents.");

            // Access the query and stop it from running
            var query = (NSMetadataQuery)notification.Object;
            query.DisableUpdates();
            query.StopQuery();

            // Release the notification
            NSNotificationCenter.DefaultCenter.RemoveObserver (this
                , NSMetadataQuery.DidFinishGatheringNotification
                , query);

            // Load the document that the query returned
            LoadDocument(query);
        }

        private void LoadDocument (NSMetadataQuery query) {
            Console.WriteLine ("Loading Document...");  

            // Take action based on the returned record count
            switch (query.ResultCount) {
            case 0:
                // Create a new document
                CreateNewDocument ();
                break;
            case 1:
                // Gain access to the url and create a new document from
                // that instance
                NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
                var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

                // Load the document
                OpenDocument (url);
                break;
            default:
                // There has been an issue
                Console.WriteLine ("Issue: More than one document found...");
                break;
            }
        }
        #endregion

        #region Public Methods

        public void OpenDocument(NSUrl url) {

            Console.WriteLine ("Attempting to open: {0}", url);
            Document = new GenericTextDocument (url);

            // Open the document
            Document.Open ( (success) => {
                if (success) {
                    Console.WriteLine ("Document Opened");
                } else
                    Console.WriteLine ("Failed to Open Document");
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        public void CreateNewDocument() {
            // Create path to new file
            // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
            var docPath = Path.Combine (docsFolder, TestFilename);
            var ubiq = new NSUrl (docPath, false);

            // Create new document at path 
            Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
            Document = new GenericTextDocument (ubiq);

            // Set the default value
            Document.Contents = "(default value)";

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
                Console.WriteLine ("Save completion:" + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                } else {
                    Console.WriteLine ("Unable to Save Document");
                }
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        /// <summary>
        /// Saves the document.
        /// </summary>
        /// <returns><c>true</c>, if document was saved, <c>false</c> otherwise.</returns>
        public bool SaveDocument() {
            bool successful = false;

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
                Console.WriteLine ("Save completion: " + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                    successful = true;
                } else {
                    Console.WriteLine ("Unable to Save Document");
                    successful=false;
                }
            });

            // Return results
            return successful;
        }
        #endregion

        #region Override Methods
        public override void FinishedLaunching (UIApplication application)
        {

            // Start a new thread to check and see if the user has iCloud
            // enabled.
            new Thread(new ThreadStart(() => {
                // Inform caller that we are checking for iCloud
                CheckingForiCloud = true;

                // Checks to see if the user of this device has iCloud
                // enabled
                var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer(null);

                // Connected to iCloud?
                if (uburl == null)
                {
                    // No, inform caller
                    HasiCloud = false;
                    iCloudUrl =null;
                    Console.WriteLine("Unable to connect to iCloud");
                    InvokeOnMainThread(()=>{
                        var okAlertController = UIAlertController.Create ("iCloud Not Available", "Developer, please check your Entitlements.plist, Bundle ID and Provisioning Profiles.", UIAlertControllerStyle.Alert);
                        okAlertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        Window.RootViewController.PresentViewController (okAlertController, true, null);
                    });
                }
                else
                {   
                    // Yes, inform caller and save location the the Application Container
                    HasiCloud = true;
                    iCloudUrl = uburl;
                    Console.WriteLine("Connected to iCloud");

                    // If we have made the connection with iCloud, start looking for documents
                    InvokeOnMainThread(()=>{
                        // Search for the default document
                        FindDocument ();
                    });
                }

                // Inform caller that we are no longer looking for iCloud
                CheckingForiCloud = false;

            })).Start();
                
        }
        
        // This method is invoked when the application is about to move from active to inactive state.
        // OpenGL applications should use this method to pause.
        public override void OnResignActivation (UIApplication application)
        {
        }
        
        // This method should be used to release shared resources and it should store the application state.
        // If your application supports background execution this method is called instead of WillTerminate
        // when the user quits.
        public override void DidEnterBackground (UIApplication application)
        {
            // Trap all errors
            try {
                // Values to include in the bookmark packet
                var resources = new string[] {
                    NSUrl.FileSecurityKey,
                    NSUrl.ContentModificationDateKey,
                    NSUrl.FileResourceIdentifierKey,
                    NSUrl.FileResourceTypeKey,
                    NSUrl.LocalizedNameKey
                };

                // Create the bookmark
                NSError err;
                Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

                // Was there an error?
                if (err != null) {
                    // Yes, report it
                    Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
                }
            }
            catch (Exception e) {
                // Report error
                Console.WriteLine ("Error: {0}", e.Message);
            }
        }
        
        // This method is called as part of the transition from background to active state.
        public override void WillEnterForeground (UIApplication application)
        {
            // Is there any bookmark data?
            if (Bookmark != null) {
                // Trap all errors
                try {
                    // Yes, attempt to restore it
                    bool isBookmarkStale;
                    NSError err;
                    var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

                    // Was there an error?
                    if (err != null) {
                        // Yes, report it
                        Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
                    } else {
                        // Load document from bookmark
                        OpenDocument (srcUrl);
                    }
                }
                catch (Exception e) {
                    // Report error
                    Console.WriteLine ("Error: {0}", e.Message);
                }
            }

        }
        
        // This method is called when the application is about to terminate. Save data, if needed.
        public override void WillTerminate (UIApplication application)
        {
        }
        #endregion

        #region Events
        public delegate void DocumentLoadedDelegate(GenericTextDocument document);
        public event DocumentLoadedDelegate DocumentLoaded;

        internal void RaiseDocumentLoaded(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentLoaded != null) {
                this.DocumentLoaded (document);
            }
        }
        #endregion
    }
}

```

> [!IMPORTANT]
> Powyższy kod zawiera kod z powyższej sekcji Discovering i wyświetlania listy dokumentów. On jest przedstawiony tutaj w całości, jak będzie wyglądał w rzeczywistej aplikacji. Dla uproszczenia ten przykład działa przy użyciu pojedynczego, ustaloną pliku (`test.txt`) tylko.

Powyższy kod przedstawia kilka skróty dysku usługi iCloud, aby ułatwić pracować w pozostałej części aplikacji.

Następnie dodaj następujący kod do dowolnej widoku lub Widok kontenera, który będzie za pomocą selektora dokumentów lub Praca z dokumentami oparte na chmurze:

```csharp
using CloudKit;
...

#region Computed Properties
/// <summary>
/// Returns the delegate of the current running application
/// </summary>
/// <value>The this app.</value>
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

Spowoduje to dodanie skrótów do `AppDelegate` i dostępu do skróty iCloud utworzonego powyżej.

Przy użyciu tego kodu w miejscu Spójrzmy na wdrażanie kontrolera widoku selektora dokumentów w aplikacji platformy Xamarin dla systemu iOS 8.

## <a name="using-the-document-picker-view-controller"></a>Za pomocą kontrolera widoku selektora dokumentów

Przed system iOS 8, bardzo trudno było je na dostęp do dokumentów z innej aplikacji, ponieważ nie było możliwości odnajdywania dokumentów poza aplikacją z poziomu aplikacji.

### <a name="existing-behavior"></a>Istniejące zachowanie

 [![](document-picker-images/image31.png "Istniejące zachowanie — omówienie")](document-picker-images/image31.png#lightbox)

Spójrzmy na uzyskiwanie dostępu do dokumentu zewnętrznego przed system iOS 8:

1.  Użytkownik musi najpierw otworzyć aplikację, która utworzyła dokumentu.
1.  Dokument jest zaznaczone i `UIDocumentInteractionController` służy do wysyłania dokumentu do nowej aplikacji.
1.  Na koniec kopię oryginalnego dokumentu znajduje się w kontenerze nowej aplikacji.


W tym miejscu dokument jest drugi aplikacja może otwierać i edytować.

### <a name="discovering-documents-outside-of-an-apps-container"></a>Odnajdywanie dokumentów poza kontenerem aplikacji

W systemie iOS 8 aplikacja jest możliwość dostępu do dokumentów poza kontenerem własnej aplikacji, z łatwością:

 [![](document-picker-images/image32.png "Odnajdywanie dokumentów poza kontenerem aplikacji")](document-picker-images/image32.png#lightbox)

Przy użyciu nowej usługi iCloud selektora dokumentów ( `UIDocumentPickerViewController`), aplikacji systemu iOS bezpośrednio może odnajdywać i uzyskać dostęp poza jej kontenerem aplikacji. `UIDocumentPickerViewController` Udostępnia mechanizm do użytkownika o udzielenie dostępu i edytować te odnalezione dokumentów za pomocą uprawnień.

Aplikacja musi wyrazić zgodę na jego dokumentów, które pojawiają się w usłudze iCloud selektora dokumentów i być dostępne dla innych aplikacji odnaleźć i pracować z nimi. Zapewnienie aplikacji systemu iOS jest 8 Xamarin udostępnianie jej kontenera aplikacji, edytuj go `Info.plist` pliku w edytorze tekstu standardowego i dodaj następujące dwa wiersze do dołu słownika (między `<dict>...</dict>` znaczników):

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

`UIDocumentPickerViewController` Zapewnia doskonałą nowy interfejs użytkownika, który umożliwia użytkownikowi wybranie dokumentów. Aby wyświetlić kontroler widoku selektora dokumentów w aplikacji platformy Xamarin dla systemu iOS 8, wykonaj następujące czynności:

```csharp
using MobileCoreServices;
...

// Allow the Document picker to select a range of document types
        var allowedUTIs = new string[] {
            UTType.UTF8PlainText,
            UTType.PlainText,
            UTType.RTF,
            UTType.PNG,
            UTType.Text,
            UTType.PDF,
            UTType.Image
        };

        // Display the picker
        //var picker = new UIDocumentPickerViewController (allowedUTIs, UIDocumentPickerMode.Open);
        var pickerMenu = new UIDocumentMenuViewController(allowedUTIs, UIDocumentPickerMode.Open);
        pickerMenu.DidPickDocumentPicker += (sender, args) => {

            // Wireup Document Picker
            args.DocumentPicker.DidPickDocument += (sndr, pArgs) => {

                // IMPORTANT! You must lock the security scope before you can
                // access this file
                var securityEnabled = pArgs.Url.StartAccessingSecurityScopedResource();

                // Open the document
                ThisApp.OpenDocument(pArgs.Url);

                // IMPORTANT! You must release the security lock established
                // above.
                pArgs.Url.StopAccessingSecurityScopedResource();
            };

            // Display the document picker
            PresentViewController(args.DocumentPicker,true,null);
        };

pickerMenu.ModalPresentationStyle = UIModalPresentationStyle.Popover;
PresentViewController(pickerMenu,true,null);
UIPopoverPresentationController presentationPopover = pickerMenu.PopoverPresentationController;
if (presentationPopover!=null) {
    presentationPopover.SourceView = this.View;
    presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Down;
    presentationPopover.SourceRect = ((UIButton)s).Frame;
}
```

> [!IMPORTANT]
> Deweloper musi wywołać `StartAccessingSecurityScopedResource` metody `NSUrl` przed dokumentu zewnętrznego. `StopAccessingSecurityScopedResource` Można wywołać metody, aby zwolnić blokady zabezpieczeń, tak szybko, jak dokument został załadowany.

### <a name="sample-output"></a>Przykładowe dane wyjściowe

Poniżej przedstawiono przykładowy sposób powyższy kod zostanie wyświetlony selektor dokumentów, gdy uruchamiana na urządzeniu iPhone:

1.  Użytkownik uruchamia aplikację i pojawi się główny interfejs:   
 
    [![](document-picker-images/image33.png "Główny interfejs jest wyświetlany.")](document-picker-images/image33.png#lightbox)
1.  Podsłuchu użytkownika **akcji** znajdujący się u góry ekranu i jest proszony o wybranie **dostawcy dokumentu** z listy dostępnych dostawców:   
 
    [![](document-picker-images/image34.png "Wybierz dostawcę dokumentu z listy dostępnych dostawców")](document-picker-images/image34.png#lightbox)
1.  **Kontrolera widoku selektora dokumentów** jest wyświetlana dla wybranego **dostawcy dokumentu**:   
 
    [![](document-picker-images/image35.png "Kontroler widoku selektora dokumentów jest wyświetlana.")](document-picker-images/image35.png#lightbox)
1.  Użytkownik naciska na **Folder dokumentów** Aby wyświetlić jego zawartość:   
 
    [![](document-picker-images/image36.png "Zawartość folderu dokumentów")](document-picker-images/image36.png#lightbox)
1.  Użytkownik wybierze **dokumentu** i **selektora dokumentów** jest zamknięty.
1.  Główny interfejs zostanie wyświetlony ponownie, **dokumentu** są ładowane z zewnętrznego kontenera i jego zawartości wyświetlanej.


Rzeczywiste wyświetlanie kontrolera widoku selektora dokumentów zależy od dostawców dokumentu, że użytkownik zainstalował na urządzeniu, i który tryb selektora dokumentów została implementacji. Powyższy przykład korzysta z trybu otwartego, inne typy tryb zostanie omówiony poniżej.

## <a name="managing-external-documents"></a>Zarządzanie dokumentami zewnętrznych

Jak wspomniano powyżej, przed system iOS 8, aplikację można tylko dostęp do dokumentów, które były częścią jego kontenera aplikacji. W systemie iOS 8 aplikacja ma dostęp do dokumentów ze źródeł zewnętrznych:

 [![](document-picker-images/image37.png "Zarządzanie zewnętrzne dokumenty — omówienie")](document-picker-images/image37.png#lightbox)

Gdy użytkownik wybiera dokument z zewnętrznego źródła, dokument referencyjny dotyczący są zapisywane do kontenera aplikacji, które wskazuje w oryginalnym dokumencie.

Aby ułatwić dodawanie to nowe możliwości do istniejących aplikacji, dodano kilka nowych funkcji do `NSMetadataQuery` interfejsu API. Zazwyczaj aplikacja używa wszechobecne zakresu dokumentu do listy dokumentów, znajdujące się w jego kontenerze aplikacji. Korzystając z tego zakresu, tylko dokumenty w kontenerze aplikacji będą wyświetlane.

Za pomocą nowego wszechobecne zewnętrznego zakresu dokumentu zwróci dokumentów na żywo poza kontener aplikacji, które zwraca metadane dla nich. `NSMetadataItemUrlKey` Będą wskazywały na adres URL, w których faktycznie znajduje się dokument.

Czasami aplikacja nie chcą pracować z dokumentami wskazywany przez odwołanie th. Zamiast tego aplikacja chce pracować w dokumencie referencyjnym bezpośrednio. Na przykład aplikacja może być wyświetlenia dokumentu w folderze aplikacji w interfejsie użytkownika lub umożliwić użytkownikowi poruszać odwołania się wewnątrz folderu.

W systemie iOS 8 nowy `NSMetadataItemUrlInLocalContainerKey` dostarczono na bezpośredni dostęp w dokumencie referencyjnym. Ten klucz wskazuje rzeczywistego odwołania do dokumentu zewnętrznego w kontenerze aplikacji.

`NSMetadataUbiquitousItemIsExternalDocumentKey` Używany do sprawdzania, czy dokument jest spoza kontenera aplikacji. `NSMetadataUbiquitousItemContainerDisplayNameKey` Umożliwia dostęp do nazwy kontenera, który jest obudowie oryginalną kopię dokumentu zewnętrznego.

### <a name="why-document-references-are-required"></a>Dlaczego są wymagane odwołania do dokumentów

Głównym powodem tego system iOS 8 używa odwołań dostęp do dokumentów zewnętrznych jest zabezpieczeń. Żadna aplikacja otrzymuje dostęp do żadnej innej aplikacji kontenera. Selektor dokumentów można to zrobić, ponieważ jest uruchomionych poza procesem, a ma szeroki dostęp do systemu.

Jedynym sposobem, aby uzyskać dostęp do dokumentu poza kontenerem aplikacji jest za pomocą selektora dokumentów, a jeśli adres URL zwracany przez selektor zakresu zabezpieczeń. Adres URL zakresu zabezpieczeń zawiera po prostu wystarczająco dużo informacji wybrany dokument wraz z zakresami praw wymaganych do udzielić aplikacji dostępu do dokumentu.

Należy pamiętać, że jeśli zserializowane do ciągu adresu URL zakresu zabezpieczeń, a następnie deserializowany, informacje o zabezpieczeniach zostałyby utracone i plik będzie niedostępny z adresu URL. Funkcja odwołania dokumentu zapewnia mechanizm, aby wrócić do plików wskazywany przez te adresy URL.

Jeśli aplikacja uzyskuje `NSUrl` z jednego dokumenty referencyjne już ma zakres zabezpieczeń dołączona i może służyć do uzyskania dostępu do pliku. Z tego powodu zdecydowanie zalecane jest, deweloper użyj `UIDocument` ponieważ obsługuje on wszystkie te informacje i procesy dla nich.

### <a name="using-bookmarks"></a>Korzystanie z zakładek

Nie zawsze jest możliwe wyliczenie dokumentów aplikacji, aby wrócić do określonego dokumentu, na przykład w trakcie przywracania stanu. System iOS 8 udostępnia mechanizm do tworzenia zakładek, bezpośrednio prowadzących do danego dokumentu.

Poniższy kod tworzy zakładki z `UIDocument`firmy `FileUrl` właściwości:

```csharp
// Trap all errors
try {
    // Values to include in the bookmark packet
    var resources = new string[] {
        NSUrl.FileSecurityKey,
        NSUrl.ContentModificationDateKey,
        NSUrl.FileResourceIdentifierKey,
        NSUrl.FileResourceTypeKey,
        NSUrl.LocalizedNameKey
    };

    // Create the bookmark
    NSError err;
    Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

    // Was there an error?
    if (err != null) {
        // Yes, report it
        Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
    }
}
catch (Exception e) {
    // Report error
    Console.WriteLine ("Error: {0}", e.Message);
}
```

Istniejącego interfejsu API zakładki służy do tworzenia zakładek dla istniejącego `NSUrl` , zapisywanie i ładowane do zapewniać bezpośredni dostęp do pliku zewnętrznego. Poniższy kod zostanie przywrócona zakładki, który został utworzony powyżej:

```csharp
if (Bookmark != null) {
    // Trap all errors
    try {
        // Yes, attempt to restore it
        bool isBookmarkStale;
        NSError err;
        var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

        // Was there an error?
        if (err != null) {
            // Yes, report it
            Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
        } else {
            // Load document from bookmark
            OpenDocument (srcUrl);
        }
    }
    catch (Exception e) {
        // Report error
        Console.WriteLine ("Error: {0}", e.Message);
    }
}
```

## <a name="open-vs-import-mode-and-the-document-picker"></a>Otwórz okno programu vs. Selektor dokumentów i tryb importu

Kontroler widoku selektora dokumentów zawiera dwa różne tryby działania:

1.  **Otwórz tryb** — w tym trybie, gdy użytkownik wybierze dokumentu zewnętrznego selektora dokumentów utworzy zakładki zakresu zabezpieczeń w kontenerze aplikacji.   
 
    [![](document-picker-images/image37.png "Zakładki w kontenerze aplikacji należących do zakresu zabezpieczeń")](document-picker-images/image37.png#lightbox)
1.  **Trybu importu** — w tym trybie, gdy użytkownik wybierze dokumentu zewnętrznego, selektora dokumentów nie utworzy zakładki, ale zamiast tego, skopiuj plik do lokalizacji tymczasowej i zapewnić aplikacji dostęp do dokumentu w tej lokalizacji:   
 
    [![](document-picker-images/image38.png "Selektor dokumentów będzie skopiuj plik do lokalizacji tymczasowej i zapewniają dostęp do aplikacji w dokumencie, w tym miejscu")](document-picker-images/image38.png#lightbox)   
 Gdy aplikacja zakończy jakiegokolwiek powodu, jest opróżniany tymczasową lokalizację i plik usunięty. Jeśli aplikacja wymaga zachować dostęp do pliku, powinien utworzyć kopii i umieść go w jego kontenerze aplikacji.


Otwórz tryb jest przydatny w przypadku, gdy aplikacja chce współpracować z innej aplikacji i udostępnianie wszelkie zmiany wprowadzone do dokumentu z tej aplikacji. Tryb importu jest używany, gdy aplikacja nie chcesz udostępniać jego modyfikacji dokumentu z innymi aplikacjami.

## <a name="making-a-document-external"></a>Tworzenie dokumentu zewnętrznego

Jak wspomniano powyżej, aplikacji systemu iOS 8 nie ma dostępu do kontenerów poza kontenerem własnej aplikacji. Aplikacja może zapisu do własnego kontenera, lokalnie lub w lokalizacji tymczasowej, a następnie użyj trybu specjalnym dokumencie przenieść utworzonego dokumentu poza kontenerem aplikacji użytkownikowi wybrać lokalizację.

Aby przenieść dokument do lokalizacji zewnętrznej, wykonaj następujące czynności:

1.  Najpierw należy utworzyć nowy dokument w lokalizacji lokalnej lub tymczasowego.
1.  Utwórz `NSUrl` wskazującego na nowy dokument.
1.  Otwórz nowy kontroler widoku selektora dokumentów i przekazać go `NSUrl` przy użyciu trybu `MoveToService` . 
1.  Gdy użytkownik wybierze nowej lokalizacji, dokument zostanie przeniesiony z jego bieżącej lokalizacji do nowej lokalizacji.
1.  Dokument referencyjny dotyczący będą zapisywane do aplikacji kontenera aplikacji tak, że plik jest nadal dostępny przy tworzeniu aplikacji.


Poniższy kod może służyć do przenieść dokument do lokalizacji zewnętrznej: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

W dokumencie referencyjnym zwrócony przez proces powyżej jest dokładnie taka sama, jak rejestr utworzony przez otwarty trybu selektora dokumentów. Istnieją jednak razy, które aplikacja może chcieć przenieść dokument bez utrzymywanie odwołania do niego.

Aby przenieść dokument bez generowania odwołania, należy użyć `ExportToService` trybu. Przykład: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

Korzystając z `ExportToService` trybie dokument został skopiowany do kontenera zewnętrznych i istniejącą kopię pozostanie w jej oryginalnej lokalizacji.

## <a name="document-provider-extensions"></a>Dokument dostawcy rozszerzeń

Z systemem iOS 8 Apple chce, aby użytkownik końcowy mógł uzyskać dostępu do żadnych dokumentów oparte na chmurze, niezależnie od tego, w którym faktycznie istnieją. Aby osiągnąć ten cel, system iOS 8 zawiera nowy mechanizm rozszerzenie dostawcy dokumentu.

### <a name="what-is-a-document-provider-extension"></a>Co to jest rozszerzenie dostawcy dokumentu?

Krótko mówiąc, rozszerzenie dostawcy dokumentu jest sposób zapewnienia do przechowywania dokumentów alternatywnej aplikacji, dostępny w dokładnie taki sam sposób jak lokalizacja istniejącego magazynu usługi iCloud dla dewelopera lub innych firm.

Użytkownik może wybrać jedną z tych lokalizacji alternatywnej przechowywania z selektora dokumentów i mogą użyć dokładnie tego samego tryby dostępu (Open, importowanie, przenoszenia lub eksportu) do pracy z plikami w tej lokalizacji.

Ten sposób jest implementowany przy użyciu dwóch różnych rozszerzeń:

-  **Rozszerzenie selektora dokumentów** — zapewnia `UIViewController` podklasy, która zapewnia graficzny interfejs użytkownika, wybierz dokument z alternatywnej lokalizacji przechowywania. Tej podklasy będą wyświetlane jako część kontrolera widoku selektora dokumentów.
-  **Podaj rozszerzenie pliku** — jest to rozszerzenie bez interfejsu użytkownika, który zajmuje się faktycznie dostarczanie zawartości plików. Te rozszerzenia są dostępne za pośrednictwem pliku koordynacji ( `NSFileCoordinator` ). Jest to inny ważne przypadek, gdzie plik koordynacja jest wymagana.


Na poniższym diagramie przedstawiono przepływ danych typowe podczas pracy z dokumentu dostawcy rozszerzeń:

 [![](document-picker-images/image39.png "Ten diagram przedstawia przepływ typowych danych, podczas pracy z dokumentu dostawcy rozszerzeń")](document-picker-images/image39.png#lightbox)

Następujący proces:

1.  Aplikacja prezentuje kontrolera selektora dokumentu, aby umożliwić użytkownikowi wybrać plik do pracy z.
1.  Użytkownik wybierze alternatywną lokalizację i pliku niestandardowej `UIViewController` rozszerzenia jest wywoływana w celu wyświetlania interfejsu użytkownika.
1.  Użytkownik wybierze plik z tej lokalizacji i adres URL jest przekazywany z powrotem do selektora dokumentów.
1.  Selektor dokumentów wybiera adres URL pliku i zwraca go do aplikacji dla użytkownika pracować nad.
1.  Adres URL jest przekazywany do koordynatora pliku, aby zwrócić zawartość plików do aplikacji.
1.  Koordynator pliku wywołuje niestandardowe rozszerzenie dostawcy plików, aby pobrać plik.
1.  Zawartość pliku są zwracane z koordynatorem pliku.
1.  Zawartość pliku są zwracane do aplikacji.


### <a name="security-and-bookmarks"></a>Zabezpieczenia i zakładki

W tej sekcji spowoduje przejście krótkie omówienie jak zabezpieczeń i trwały dostęp do pliku za pośrednictwem zakładek współpracuje z dokumentu dostawcę rozszerzeń. W przeciwieństwie do usługi iCloud dostawcy dokumentu, który automatycznie zapisze zabezpieczeń i zakładki do kontenera aplikacji, dokumentu dostawca rozszerzeń nie jest, ponieważ nie są częścią systemu odwołania dokumentu.

Na przykład: w środowisku przedsiębiorstwa zawiera swój własny magazyn danych do bezpiecznej całej firmy, Administratorzy nie mają poufnych informacji firmowych dostępne lub przetworzone przez usługę iCloud publicznych serwerów. W związku z tym nie można użyć wbudowanych systemu odniesienia dokumentu.

Nadal można używać systemu zakładki i jest odpowiedzialny za rozszerzenie dostawcy plików, aby poprawnie przetworzyć adresu URL dodanego i zwraca zawartość dokumentu wskazywany przez niego.

Ze względów bezpieczeństwa systemu iOS jest 8 ma warstwę izolacji, która utrwala informacji o tym, które aplikacja ma dostęp do których identyfikator, wewnątrz której dostawca pliku. Należy zauważyć, że wszystkie dostęp do plików jest kontrolowany przez tę warstwę izolacji.

Na poniższym diagramie przedstawiono przepływ danych podczas pracy z zakładki i rozszerzenie dostawcy dokumentu:

 [![](document-picker-images/image40.png "Ten diagram przedstawia przepływ danych podczas pracy z zakładki i rozszerzenie dostawcy dokumentu")](document-picker-images/image40.png#lightbox)

Następujący proces:

1.  Aplikacja ma wprowadź tła i ma zostać zachowany po jego stan. Wywołuje `NSUrl` do utworzenia zakładki do pliku w magazynie alternatywne.
1.  `NSUrl` wywołuje rozszerzenie dostawcy plików do pobrania trwałego adresu URL do dokumentu. 
1.  Rozszerzenie dostawcy plików zwraca adres URL w formie ciągu do `NSUrl` .
1.  `NSUrl` Zawiera adres URL do zakładki i zwraca go do aplikacji.
1.  Gdy aplikacja jest przywracany z znajdujące się w tle i musi mieć w celu przywrócenia stanu, przekazuje on zakładki do `NSUrl` .
1.  `NSUrl` wywołuje rozszerzenie dostawcy plików za pomocą adresu URL pliku.
1.  Dostawcy rozszerzeń plików uzyskuje dostęp do pliku i zwraca lokalizację pliku do `NSUrl` .
1.  Lokalizacja pliku jest umieszczany w pakietach za pomocą informacji o zabezpieczeniach i zwrócone do aplikacji.


W tym miejscu aplikacji można uzyskać dostępu do pliku i pracować w zwykły.

### <a name="writing-files"></a>Zapisywanie plików

W tej sekcji spowoduje przejście krótkie omówienie sposobu pisania pliki do lokalizacji alternatywnej z działa rozszerzenie dostawcy dokumentu. Aplikacje systemu iOS użyje koordynacji pliku do zapisania informacji o dysku w kontenerze aplikacji. Wkrótce, po pomyślnym zapisany plik rozszerzenie dostawcy plików zostaną powiadomieni o zmianie.

W tym momencie rozszerzenie dostawcy plików można rozpocząć przekazywania pliku do lokalizacji alternatywnej (lub oznaczyć go jako zanieczyszczony i wymagające przekazywania).

### <a name="creating-new-document-provider-extensions"></a>Tworzenie nowego dokumentu dostawcy rozszerzeń

Tworzenie nowego dokumentu dostawcy rozszerzeń wykracza poza zakres tego artykułu wprowadzających. Te informacje są podane tutaj aby pokazać, w zależności od rozszerzenia, które użytkownik został załadowany w urządzeniu z systemem iOS, aplikacji mogą mieć dostęp do lokalizacji magazynu dokumentu poza Apple podana lokalizacja usługi iCloud.

Deweloper należy pamiętać o tym fakcie, korzystając z selektora dokumentów i Praca z dokumentami zewnętrznych. Należy nie zakładać, tych dokumentów są hostowane w usłudze iCloud.

Aby uzyskać więcej informacji na temat tworzenia dostawcy magazynu lub rozszerzenie aplikacji Document Picker, zobacz [wprowadzenie do rozszerzeń aplikacji](~/ios/platform/extensions.md) dokumentu.

## <a name="migrating-to-icloud-drive"></a>Migrowanie do dysku w usłudze iCloud

W systemie iOS 8 użytkownicy mogą być nadal przy użyciu istniejącej usługi iCloud System dokumentów jest używany system iOS 7 (i starszymi systemami) lub mogą wybrać migrację istniejących dokumentów do nowego mechanizmu dysku w usłudze iCloud.

Na Mac OS X Yosemite, firmy Apple nie zapewnia wstecznej zgodności więc wszystkie dokumenty muszą być migrowane do usługi iCloud dysku lub ich nie będą już zaktualizowane na urządzeniach.

Po przeprowadzeniu migracji konta użytkownika do usługi iCloud dysku propagowanie zmian w dokumentach na tych urządzeniach będzie można tylko urządzenia przy użyciu usługi iCloud dysku.

> [!IMPORTANT]
> Deweloperzy, należy pamiętać, że nowe funkcje omówione w tym artykule są dostępne tylko jeśli konta została zmigrowana do usługi iCloud dysku. 

## <a name="summary"></a>Podsumowanie

W tym artykule został objęty zmian do istniejących interfejsów API wymagane do obsługi dysków w usłudze iCloud i nowy kontroler widoku selektora dokumentów w usłudze iCloud. Oceniono koordynacji pliku i dlaczego jest ważne podczas pracy z dokumentami oparte na chmurze. Ma omówione ustawienia wymagane do włączenia oparte na chmurze dokumentów w aplikacji platformy Xamarin.iOS i biorąc pod uwagę wprowadzające przyjrzeć Praca z dokumentami spoza kontenera aplikacji aplikacji za pomocą kontrolera widoku selektora dokumentów.

Ponadto w tym artykule opisano skrótowo dokumentu dostawcy rozszerzeń i dlaczego Deweloper powinien wiedzieć o ich podczas pisania aplikacji, które może obsłużyć dokumentów opartych na chmurze.

## <a name="related-links"></a>Linki pokrewne

- [DocPicker (przykład)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [Wprowadzenie do systemu iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Wprowadzenie do rozszerzeń aplikacji](~/ios/platform/extensions.md)
