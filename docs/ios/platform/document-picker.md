---
title: Selektor dokumentu
description: "Kontroler widoku dokumentu selektora przyznaje użytkownikom dostępu do plików spoza aplikacji piaskownicy. Jest prosty mechanizm udostępniania dokumentów między aplikacjami. Umożliwia również bardziej złożonych przepływów pracy, ponieważ użytkownicy mogą edytować pojedynczego dokumentu z wieloma aplikacjami. Ten artykuł zawiera wprowadzenie do aplikacji platformy Xamarin.iOS przy użyciu selektora dokumentu i zmiany w dokumentach iCloud wymagane do jego obsługi."
ms.topic: article
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 4a8f1632076a12b1737ba8294ac8b2f28f19dc77
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="document-picker"></a>Selektor dokumentu

_Kontroler widoku dokumentu selektora przyznaje użytkownikom dostępu do plików spoza aplikacji piaskownicy. Jest prosty mechanizm udostępniania dokumentów między aplikacjami. Umożliwia również bardziej złożonych przepływów pracy, ponieważ użytkownicy mogą edytować pojedynczego dokumentu z wieloma aplikacjami. Ten artykuł zawiera wprowadzenie do aplikacji platformy Xamarin.iOS przy użyciu selektora dokumentu i zmiany w dokumentach iCloud wymagane do jego obsługi._

Selektor dokumentu umożliwia dokumenty mogą być współużytkowane przez aplikacje. Te dokumenty mogą być przechowywane w ramach usługi iCloud lub w katalogu inną aplikację. Dokumenty są udostępniane przez zestaw [dokumentu dostawcy rozszerzeń](~/ios/platform/extensions.md) użytkownik zainstalował na urządzeniu. 

Z powodu trudności utrzymania dokumenty synchronizowane między chmurą a aplikacje oni wprowadzić pewien niezbędne złożoności.

## <a name="requirements"></a>Wymagania

Następujące wymagane jest wykonanie czynności przedstawionych w tym artykule:

-  **Xcode 7 i iOS 8 lub nowszego** — Xcode 7 i iOS 8 lub nowszych interfejsów API firmy Apple muszą być zainstalowane i skonfigurowane na komputerze dewelopera.
-  **Visual Studio lub Visual Studio for Mac** — należy zainstalować najnowszą wersję programu Visual Studio for Mac.
-  **Urządzenie systemu iOS** — urządzenia z systemem iOS z systemem iOS 8 lub nowszy.

## <a name="changes-to-icloud"></a>Zmiany w ramach usługi iCloud

Aby wdrożyć nowe funkcje selektora dokumentu, iCloud firmy Apple, usługa wprowadzono następujące zmiany:

-  ICloud demon ma została napisana przy użyciu CloudKit.
-  Istniejące iCloud, funkcje zostały zmieniona iCloud dysku.
-  Dodano obsługę dla systemu Microsoft Windows z usługą iCloud.
-  Folder iCloud został dodany w Mac OS Finder.
-  urządzenia z systemem iOS można uzyskać dostępu do zawartości folderu iCloud systemu Mac OS.


## <a name="what-is-a-document"></a>Co to jest dokument?

W odniesieniu do dokumentów w ramach usługi iCloud jest jednostką jednej, autonomicznej i powinien postrzegane jako takie przez użytkownika. Użytkownik może chcieć modyfikowanie dokumentu lub udostępnić ją innym użytkownikom (przy użyciu poczty e-mail, na przykład).

Istnieje kilka typów plików, że użytkownik natychmiast rozpoznawany jako dokumentów, takich jak strony, prelekcji lub liczby plików. Jednak iCloud nie jest ograniczona do tej reguły. Na przykład stanu gry (na przykład dopasowanie gra) można traktowane jako dokument i przechowywane w ramach usługi iCloud. Ten plik może być przekazywane między urządzeniami użytkownika i zezwolić im na odebrania gry którym przerwał pracę na innym urządzeniu.

## <a name="dealing-with-documents"></a>Dotyczące dokumentów

Przed rozpoczęciem pracy z kodem musieli używać selektora dokumentu za pomocą platformy Xamarin, w tym artykule będzie obejmować najlepsze rozwiązania dotyczące pracy z dokumentami iCloud i kilka zmian wprowadzonych do istniejących interfejsów API wymagane do obsługi selektora dokumentu.

### <a name="using-file-coordination"></a>Przy użyciu pliku koordynacji

Ponieważ można zmodyfikować pliku z różnych miejsc, koordynacji należy użyć, aby zapobiec utracie danych.

 [![](document-picker-images/image1.png "Przy użyciu pliku koordynacji")](document-picker-images/image1.png#lightbox)

Spójrzmy na powyższej ilustracji:

1.  Urządzenia z systemem iOS przy użyciu pliku koordynacji tworzy nowy dokument i zapisze go w ramach usługi icloud folderu.
2.  iCloud zmodyfikowany plik jest zapisywany do chmury w celu dystrybucji do wszystkich urządzeń.
3.  Dołączone Mac widzi zmodyfikowanego pliku w ramach usługi iCloud folderu i używa koordynacji pliku Aby skopiować zmiany do pliku.
4.  Urządzenie nie za pomocą pliku koordynacji sprawia, że zmiany w pliku i zapisze go w ramach usługi icloud folderu. Te zmiany natychmiast są replikowane do innych urządzeń.

Załóżmy oryginalnej urządzenia z systemem iOS lub Mac został edytowania pliku, teraz utraty swoje zmiany i zastąpione wersji pliku z nieskoordynowane urządzenia. Aby zapobiec utracie danych, plików koordynacji jest koniecznością podczas pracy z dokumentami oparte na chmurze.

### <a name="using-uidocument"></a>Przy użyciu UIDocument

 `UIDocument` upraszcza czynności (lub `NSDocument` na macOS), wykonując wszystkie lifting ciężki dla deweloperów. Zawiera wbudowane pliku koordynacji z kolejki tła, aby zapobiec blokuje interfejsu użytkownika aplikacji.

 `UIDocument` udostępnia wiele ogólnych interfejsów API, które upraszczają tworzeniem aplikacji platformy Xamarin na cel Projektant wymaga.

Poniższy kod tworzy podklasy `UIDocument` do zaimplementowania ogólnego dokument tekstowy, który może służyć do przechowywania i pobierania tekstu w ramach usługi iCloud:

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

`GenericTextDocument` Klasy przedstawionych powyżej będzie używana w tym artykule podczas pracy z selektora dokumentu i zewnętrznych dokumentów w aplikacji platformy Xamarin.iOS 8.

## <a name="asynchronous-file-coordination"></a>Koordynacji pliku asynchroniczne

System iOS 8 zawiera kilka nowych funkcji asynchronicznych koordynacji pliku przy użyciu nowych interfejsów API koordynacji pliku. Przed iOS 8 wszystkie istniejące koordynacji interfejsy API plików zostały całkowicie synchronicznego. To oznaczało, że deweloper został odpowiedzialnych za wdrażanie własnych tła usługi kolejkowania wiadomości, aby zapobiec koordynacji pliku blokuje interfejsu użytkownika aplikacji.

Nowe `NSFileAccessIntent` klasy zawiera adres URL wskazującym plik i kilka opcji, aby kontrolować typ koordynacji wymagane. Poniższy kod ilustruje, przenoszenie plików z jednej lokalizacji do innej przy użyciu lokalizacji docelowych:

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

## <a name="discovering-and-listing-documents"></a>Wykrywanie i wyświetlanie dokumentów

Jest to sposób odnajdywania i listy dokumentów przy użyciu istniejącego `NSMetadataQuery` interfejsów API. W tej sekcji opisano nowe funkcje dodane do `NSMetadataQuery` który sprawiają, że praca z dokumentami łatwiejsze niż przed.

### <a name="existing-behavior"></a>Zachowanie istniejącej

Przed iOS 8 `NSMetadataQuery` przebiegało powoli na zmiany pliku lokalnego podnoszenia takich jak: usuwa, tworzy i zmienia jego nazwę.

 [![](document-picker-images/image2.png "Omówienie zmiany pliku lokalnego NSMetadataQuery")](document-picker-images/image2.png#lightbox)

Na powyższym diagramie:

1.  W przypadku plików, które już istnieją w kontenerze aplikacji `NSMetadataQuery` już `NSMetadata` rekordów wstępnie utworzone i buforowane tak, aby były dostępne natychmiast dla aplikacji.
1.  Aplikacja tworzy nowy plik w kontenerze aplikacji.
1.  Brak opóźnienia przed `NSMetadataQuery` widzi modyfikacji do kontenera aplikacji i tworzy wymagane `NSMetadata` rekordu.


Ze względu na opóźnienie do tworzenia `NSMetadata` rekordów, aplikacja miała ma dwie dane, otwórz źródeł: jeden dla zmiany w pliku lokalnym i w chmurze na podstawie zmiany.

### <a name="stitching"></a>Łączenia

W systemie iOS 8 `NSMetadataQuery` jest łatwiejsze do użycia bezpośrednio z nową funkcję o nazwie Stitching:

 [![](document-picker-images/image3.png "NSMetadataQuery z nową funkcję o nazwie Stitching")](document-picker-images/image3.png#lightbox)

Przy użyciu Stitching na powyższym diagramie:

1.  Jak wcześniej dla plików, które już istnieją w kontenerze aplikacji `NSMetadataQuery` już `NSMetadata` rekordów wstępnie utworzone i umieszczonych w buforze.
1.  Aplikacja tworzy nowy plik w kontenerze aplikacji przy użyciu pliku koordynacji.
1.  Punktu zaczepienia w kontenerze aplikacji widzi modyfikacji i wywołania `NSMetadataQuery` można utworzyć wymaganych `NSMetadata` rekordu.
1.  `NSMetadata` Rekord został utworzony bezpośrednio po pliku i jest udostępniana do aplikacji.


Przy użyciu Stitching aplikacji nie ma już otworzyć źródła danych, do monitorowania lokalnych i chmurze zmiany pliku. Teraz aplikacji może polegać na `NSMetadataQuery` bezpośrednio.

> [!IMPORTANT]
> **Uwaga**: łączenia działa tylko, jeśli aplikacja używa pliku koordynacji przedstawionych w powyższej sekcji. Jeśli nie jest używany plik koordynacji, interfejsy API domyślnie zachowanie istniejącej iOS 8 wstępnie.




### <a name="new-ios-8-metadata-features"></a>Nowe funkcje metadanych z systemem iOS 8

Dodano następujące nowe funkcje `NSMetadataQuery` w systemie iOS 8:

-   `NSMetatadataQuery` Teraz można wyświetlić listę dokumentów nielokalnych przechowywanych w chmurze.
-  Dostęp do informacji o metadanych w dokumentach oparte na chmurze zostały dodane nowe interfejsy API. 
-  Jest dostępna nowa `NSUrl_PromisedItems` interfejsu API, który będzie można uzyskać dostępu do atrybutów pliku pliki, które mogą lub nie ma ich zawartość dostępna lokalnie.
-  Użyj `GetPromisedItemResourceValue` metodę, aby uzyskać informacje na temat danego pliku lub użyj `GetPromisedItemResourceValues` metodę, aby uzyskać informacje na więcej niż jeden plik w czasie.


Dwie flagi koordynacji nowego pliku zostały dodane zajmujących się metadanych:

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


Powyżej flag zawartość pliku dokumentu nie muszą być dostępne lokalnie dla nich do użycia.

Następujący segment kod przedstawia sposób użycia `NSMetadataQuery` zapytań istnienie określonego pliku, utworzyć plik, jeśli nie istnieje:

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

### <a name="document-thumbnails"></a>Miniatur dokumentów

Apple tak, że najlepsze środowisko użytkownika podczas wyświetlania dokumentów dla aplikacji jest użycie podglądu. Dzięki temu kontekście użytkowników końcowych, tak szybko mogą identyfikować dokument, który chcą pracować z.

Przed iOS 8 wyświetlanie podglądu dokumentu wymagane implementacja niestandardowa. Jesteś nowym użytkownikiem z systemem iOS 8 są atrybuty systemu plików, które umożliwiają deweloperom szybkie opracowywanie z miniaturami dokumentu.

#### <a name="retrieving-document-thumbnails"></a>Trwa pobieranie miniatur dokumentów 

Wywołując `GetPromisedItemResourceValue` lub `GetPromisedItemResourceValues` metod, `NSUrl_PromisedItems` interfejsu API, `NSUrlThumbnailDictionary`, jest zwracany. Tylko klucz obecnie w tym słowniku jest `NSThumbnial1024X1024SizeKey` i jego dopasowywania `UIImage`.

#### <a name="saving-document-thumbnails"></a>Zapisywanie miniatur dokumentów

Najprostszym sposobem Zapisz miniaturę polega na użyciu `UIDocument`. Wywołując `GetFileAttributesToWrite` metody `UIDocument` i ustawienie miniaturę, zostanie automatycznie zapisany po pliku dokumentu. ICloud demon Zobacz tej zmiany, a następnie go są propagowane do usługi iCloud. W systemie Mac OS X miniatur są generowane automatycznie dla deweloperów przez wtyczkę szybkiego wyszukiwania.

Podstawy Praca z dokumentami iCloud oparte na miejscu, wraz z zmian do istniejących interfejsu API, możemy przystąpić do zaimplementowania kontroler widoku selektora dokumentu w Xamarin iOS 8 aplikacji mobilnej.


## <a name="enabling-icloud-in-xamarin"></a>Włączanie usługi iCloud w Xamarin

Przed użyciem selektora dokumentu w aplikacji platformy Xamarin.iOS, obsługa iCloud musi być włączona zarówno w aplikacji, jak i za pośrednictwem firmy Apple. 

Poniższe kroki przewodnika proces inicjowania obsługi administracyjnej dla usługi iCloud.

1. Utwórz kontener w usłudze iCloud.
2. Utwórz identyfikator aplikacji zawierający iCloud usługi aplikacji.
3. Utwórz profil inicjowania obsługi administracyjnej, który zawiera ten identyfikator aplikacji.

[Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) przewodnik przeprowadzi Cię przez pierwsze dwa kroki. Aby utworzyć profil inicjowania obsługi administracyjnej, postępuj zgodnie z instrukcjami [profilu inicjowania obsługi administracyjnej](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile) przewodnik.



Poniższe kroki przewodnika przez proces konfigurowania aplikacji do usługi iCloud:

Wykonaj następujące czynności:

1.  Otwórz projekt w programie Visual Studio dla komputerów Mac lub Visual Studio.
2.  W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz pozycję Opcje.
3.  Wybierz opcje — Okno dialogowe w **aplikacji systemu iOS**, upewnij się, że **identyfikator pakietu** jest zgodna ze strukturą, która została zdefiniowana w **identyfikator aplikacji** utworzone powyżej dla aplikacji. 
4.  Wybierz **iOS podpisywania pakietu**, wybierz pozycję **tożsamości Developer** i **profilu inicjowania obsługi administracyjnej** utworzone powyżej.
5.  Kliknij przycisk **OK** przycisk, aby zapisać zmiany i zamknąć okno dialogowe.
6.  Kliknij prawym przyciskiem myszy `Entitlements.plist` w **Eksploratora rozwiązań** aby otworzyć go w edytorze.

    > [!IMPORTANT]
> **Uwaga**: W programie Visual Studio może być konieczne Otwórz Edytor uprawnień, klikając prawym przyciskiem myszy, wybierając **Otwórz za pomocą...** i wybierając Edytor listy właściwości

7.  Sprawdź **włączyć iCloud** , **dokumenty iCloud** , **magazynu kluczy i wartości** i **CloudKit** .
8.  Upewnij się, **kontenera** istnieje dla aplikacji (jak utworzyć powyżej). Przykład: `iCloud.com.your-company.AppName`
9.  Zapisz zmiany w pliku.

Więcej informacji na temat uprawnień można znaleźć [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

Z powyższymi ustawieniami w miejscu aplikacji można teraz używać chmurowych dokumentów i nowego kontrolera widoku selektora dokumentu.

## <a name="common-setup-code"></a>Typowy kod instalacji

Przed rozpoczęciem pracy z kontrolera widoku selektora dokumentu, Brak kodu standardowych instalacji wymagane. Rozpocznij od modyfikowania aplikacji `AppDelegate.cs` pliku i zapewnić ich wyglądać następująco:

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
> **Uwaga**: powyżej kodu zawiera kod z powyższej sekcji Discovering i wyświetlanie dokumentów. On jest przedstawiony w tym miejscu w całości, podobnie jak w rzeczywistej aplikacji. Dla uproszczenia, w tym przykładzie współpracuje z pojedynczą, ustalony pliku (`test.txt`) tylko.

Powyższy kod przedstawia kilka skróty iCloud dysku, aby łatwiej współpracować w pozostałej części aplikacji.

Następnie dodaj następujący kod do dowolnego widoku lub kontener widoku, który ma być za pomocą selektora dokumentu lub Praca z dokumentami oparte na chmurze:

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

Spowoduje to dodanie skrótów na uzyskanie dostępu do `AppDelegate` i uzyskiwać dostęp do usługi iCloud skrótów utworzone powyżej.

Ten kod w miejscu Spójrzmy na wdrażanie w aplikacji platformy Xamarin iOS 8 kontroler widoku selektora dokumentu.

## <a name="using-the-document-picker-view-controller"></a>Za pomocą kontrolera widoku selektora dokumentu

Przed iOS 8 było bardzo trudne do dostęp do dokumentów z innej aplikacji, ponieważ nie można odnaleźć dokumentów lokalizację poza aplikacją z poziomu aplikacji.

### <a name="existing-behavior"></a>Zachowanie istniejącej

 [![](document-picker-images/image31.png "Istniejące zachowanie — omówienie")](document-picker-images/image31.png#lightbox)

Spójrzmy na uzyskiwanie dostępu do zewnętrznego dokumentu przed iOS 8:

1.  Użytkownik musi najpierw otworzyć aplikację, którą utworzył dokumentu.
1.  Wybrano dokumentu i `UIDocumentInteractionController` służy do wysyłania dokumentu w nowej aplikacji.
1.  Ponadto kopię oryginału jest umieszczona w kontenerze nowej aplikacji.


Z tego miejsca dokumentu jest dostępna dla drugiego aplikacji, aby otworzyć i edytować.

### <a name="discovering-documents-outside-of-an-apps-container"></a>Odnajdywanie dokumentów spoza kontenera aplikacji

W systemie iOS 8 aplikacja jest w stanie uzyskać dostęp do dokumentów poza kontener własnej aplikacji z łatwością:

 [![](document-picker-images/image32.png "Odnajdywanie dokumentów spoza kontenera aplikacji")](document-picker-images/image32.png#lightbox)

Przy użyciu nowej usługi iCloud selektora dokumentu ( `UIDocumentPickerViewController`), aplikacji systemu iOS mogą bezpośrednio odnaleźć i dostępu poza jego kontenera aplikacji. `UIDocumentPickerViewController` Udostępnia mechanizm dla użytkownika do udostępnienia i edytowania tych odnalezione dokumentów za pomocą uprawnień.

Aplikacja musi wyrazić zgodę na jego dokumentów wyświetlani w iCloud selektora dokumentu i być dostępne dla innych aplikacji na potrzeby odnajdywania i pracować z nimi. Aby aplikacja Xamarin iOS 8 udostępnić jego kontenera aplikacji, edytować go `Info.plist` w edytorze tekstu standardowego i dodaj następujące dwa wiersze w dół słownika (między `<dict>...</dict>` tagów):

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

`UIDocumentPickerViewController` Zapewnia dużą nowy interfejs użytkownika, który pozwala użytkownikowi na wybranie dokumentów. Aby wyświetlić kontroler widoku selektora dokumentu w aplikacji platformy Xamarin iOS 8, wykonaj następujące czynności:

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
> **Uwaga**: deweloper musi wywołać `StartAccessingSecurityScopedResource` metody `NSUrl` przed zewnętrznym dokumencie jest dostępny. `StopAccessingSecurityScopedResource` Można wywołać metody, aby zwolnić blokady zabezpieczeń jak dokument został załadowany.

### <a name="sample-output"></a>Przykładowe dane wyjściowe

Poniżej przedstawiono przykład sposobu powyższy kod wyświetla selektora dokumentu, gdy uruchamiana na urządzeniu iPhone:

1.  Użytkownik uruchamia aplikację i wyświetlany jest interfejs główne:   
 
    [![](document-picker-images/image33.png "Wyświetlany jest interfejs głównego")](document-picker-images/image33.png#lightbox)
1.  Podsłuchu użytkownika **akcji** w górnej części ekranu i jest wyświetlony monit o wybranie **dostawcy dokumentu** z listy dostępnych dostawców:   
 
    [![](document-picker-images/image34.png "Wybierz dostawcę dokumentu z listy dostępnych dostawców")](document-picker-images/image34.png#lightbox)
1.  **Dokumentu selektora widoku kontrolera** jest wyświetlane dla wybranego **dostawcy dokumentu**:   
 
    [![](document-picker-images/image35.png "Kontroler widoku selektora dokumentu jest wyświetlany.")](document-picker-images/image35.png#lightbox)
1.  Użytkownik naciska na **Folder dokumentów** Aby wyświetlić jego zawartość:   
 
    [![](document-picker-images/image36.png "Zawartość dokumentu")](document-picker-images/image36.png#lightbox)
1.  Użytkownik wybiera **dokumentu** i **selektora dokumentu** jest zamknięty.
1.  Interfejs głównego zostanie wyświetlony ponownie, **dokumentu** są ładowane z zewnętrznego kontenera i jego zawartość wyświetlana.


Rzeczywiste wyświetlania dokumentu selektora widoku kontrolera zależy od dostawców dokumentu, czy użytkownik zainstalował na urządzeniu, i który tryb selektora dokumentu została wdrożenie. Powyższy przykład używa trybu otwartego, innych typów tryb będzie omówionych szczegółowo poniżej.

## <a name="managing-external-documents"></a>Zarządzanie dokumentami zewnętrznych

Jak wspomniano powyżej, przed iOS 8, aplikację tylko może uzyskać dostępu do dokumentów, które były częścią jego kontenera aplikacji. W systemie iOS 8 aplikacja może uzyskiwać dostęp do dokumentów ze źródeł zewnętrznych:

 [![](document-picker-images/image37.png "Omówienie dokumentów zewnętrznych zarządzania")](document-picker-images/image37.png#lightbox)

Gdy użytkownik wybierze dokumentu ze źródła zewnętrznego, dokumentu odwołania są zapisywane do kontenera aplikacji, który wskazuje w oryginalnym dokumencie.

Aby ułatwić dodawanie tej nowej możliwości do istniejących aplikacji, dodano kilka nowych funkcji do `NSMetadataQuery` interfejsu API. Zwykle aplikacja korzysta z uniwersalnych zakresu dokumentu do listy dokumentów, których live wewnątrz jej kontenera aplikacji. Przy użyciu tego zakresu, będzie wyświetlany tylko dokumenty w kontenerze aplikacji.

Przy użyciu nowego powszechny zewnętrznego zakresu dokumentu zwróci dokumentów na żywo poza kontenerem aplikacji, które zwraca metadane dla nich. `NSMetadataItemUrlKey` Wskaże adres URL, gdzie rzeczywiście znajduje się dokument.

Czasami aplikacji nie ma do pracy z dokumentami jest wskazywana przez tą odwołania. Zamiast tego aplikacja chce w dokumencie referencyjnym bezpośrednio. Na przykład aplikacja może być do wyświetlania dokumentu w folderze aplikacji w interfejsie użytkownika lub umożliwiające użytkownikom poruszanie odniesienia znajduje się w folderze.

W systemie iOS 8 nowy `NSMetadataItemUrlInLocalContainerKey` dostarczono na bezpośredni dostęp w dokumencie referencyjnym. Klucz ten wskazuje rzeczywistego odwołania do zewnętrznego dokumentu w kontenerze aplikacji.

`NSMetadataUbiquitousItemIsExternalDocumentKey` Używany do sprawdzania, czy dokument jest zewnętrzne do aplikacji kontenera. `NSMetadataUbiquitousItemContainerDisplayNameKey` Umożliwia dostęp do nazwy kontenera, który jest obudowie oryginalną kopię dokumentu zewnętrznego.

### <a name="why-document-references-are-required"></a>Dlaczego wymagane są odwołania do dokumentów

Głównym celem tej iOS 8 używa odwołań uzyskać dostęp do dokumentów zewnętrznych jest zabezpieczeń. Żadna aplikacja nie otrzyma dostęp do kontenera żadnej innej aplikacji. Selektor dokumentu można to zrobić, ponieważ jest uruchomiona poza procesem i ma szeroki dostęp do systemu.

Jedynym sposobem na uzyskanie dostępu do dokumentu spoza kontenera aplikacji jest za pomocą selektora dokumentu, a jeśli adres URL zwrócone przez selektor zakres zabezpieczeń. Adres URL zakres zabezpieczeń zawiera wystarczającego informacje na temat wybrany dokument wraz z zakresami uprawnienia wymagane do przyznać aplikacji dostęp do dokumentu.

Należy zauważyć, że jeśli zserializowane do ciągu adresu URL zakresu zabezpieczeń, a następnie deserializowany, informacji o zabezpieczeniach, może spowodować utratę i plik będzie niedostępny z adresu URL. Funkcja odwołania dokumentu zapewnia mechanizm, aby wrócić do plików wskazywana przez te adresy URL.

Dlatego jeśli aplikacja uzyskuje `NSUrl` z jednego z dokumentów, odwołanie już ma zakres zabezpieczeń dołączona i może służyć do dostępu do pliku. Z tego powodu zdecydowanie zalecane jest aby użyć projektanta `UIDocument` ponieważ obsługuje wszystkie te informacje i procesy dla nich.

### <a name="using-bookmarks"></a>Korzystanie z zakładek

Nie zawsze jest możliwe wyliczenie dokumentów aplikacji, aby wrócić do określonego dokumentu, na przykład podczas przywracania stanu. System iOS 8 udostępnia mechanizm do tworzenia zakładek bezpośrednio prowadzących do danego dokumentu.

Następujący kod utworzy zakładki z `UIDocument`w `FileUrl` właściwości:

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

Istniejące API zakładki służy do tworzenia zakładek przed istniejące `NSUrl` czy zapisać i ładowane do zapewnienia bezpośredniego dostępu do pliku zewnętrznego. Poniższy kod spowoduje przywrócenie zakładki, która została utworzona powyżej:

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

## <a name="open-vs-import-mode-and-the-document-picker"></a>Otwórz okno programu vs. Trybu Import i selektor dokumentu

Kontroler widoku selektora dokumentu zawiera dwa różne tryby działania:

1.  **Otwórz tryb** — w tym trybie, gdy użytkownik wybierze i zewnętrznym dokumencie selektora dokumentu spowoduje utworzenie zakładki zakres zabezpieczeń w kontenerze aplikacji.   
 
    [![](document-picker-images/image37.png "Zabezpieczenia zakres zakładek w kontenerze aplikacji")](document-picker-images/image37.png#lightbox)
1.  **Trybu importu** — w tym trybie, gdy użytkownik wybierze i zewnętrznym dokumencie, selektora dokumentu zostanie nie utworzyć zakładki, lecz skopiuj plik do lokalizacji tymczasowej i zapewnić dostęp aplikacji do dokumentu w tej lokalizacji:   
 
    [![](document-picker-images/image38.png "Selektor dokumentu zostanie skopiuj plik do lokalizacji tymczasowej i zapewnienia dostępu aplikacji do dokumentu w tej lokalizacji")](document-picker-images/image38.png#lightbox)   
 Gdy aplikacja zakończy jakiejkolwiek przyczyny, lokalizacji tymczasowej jest opróżniany i usunąć pliku. Jeśli aplikacja musi do obsługi dostępu do pliku, jego wykonanie kopii i umieścić go w jego kontenera aplikacji.


Trybu otwartego jest przydatne, gdy aplikacja chce współpracę z innymi aplikacjami i udostępniać wszelkie zmiany wprowadzone do dokumentu z daną aplikacją. Tryb importu jest używany, gdy aplikacja nie chce udostępnić jego modyfikacji dokumentu z innymi aplikacjami.

## <a name="making-a-document-external"></a>Tworzenie zewnętrznego dokumentu

Jak wspomniano powyżej, aplikacji systemu iOS 8 nie ma dostępu do kontenerów poza jego własnej kontenera aplikacji. Aplikacji można zapisać do jego własnej kontenera lokalnie lub do lokalizacji tymczasowej, a następnie użyj trybu specjalnego dokumentu, aby przenieść dokument wynikowy spoza kontenera aplikacji użytkownika wybrana lokalizacja.

Aby przenieść dokument w lokalizacji zewnętrznej, należy wykonać następujące czynności:

1.  Najpierw należy utworzyć nowy dokument w lokalizacji lokalnej lub tymczasowych.
1.  Utwórz `NSUrl` wskazującego na nowy dokument.
1.  Otwórz nowy kontroler widoku selektora dokumentu i przekaż go `NSUrl` z trybu `MoveToService` . 
1.  Po wybraniu nowej lokalizacji, dokument zostanie przeniesiony z jego bieżącej lokalizacji do nowej lokalizacji.
1.  Odwołania dokumentu będą zapisywane do aplikacji kontenera aplikacji tak, że plik nadal będą dostępne przez tworzenie aplikacji.


Poniższy kod umożliwia przeniesienie dokumentu do lokalizacji zewnętrznej: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

W dokumencie referencyjnym zwrócony przez proces powyżej jest dokładnie taka sama utworzonej przez Otwórz tryb selektora dokumentu. Istnieją jednak, ile razy aplikacja może chcieć przenieść dokumentu bez aktualizowania odwołanie do niej.

Aby przenieść dokument bez generowania odwołanie, użyj `ExportToService` tryb. Przykład: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

Korzystając z `ExportToService` trybie dokument został skopiowany do zewnętrznego kontenera i istniejącą kopię pozostanie w jej oryginalnej lokalizacji.

## <a name="document-provider-extensions"></a>Dokument dostawcy rozszerzeń

Z systemem iOS 8 Apple chce, aby użytkownicy mogli uzyskać dostępu do żadnych dokumentów oparte na chmurze, niezależnie od tego, gdzie faktycznie istnieją. Na osiągnięcie tego celu, iOS 8 zapewnia mechanizm nowy dokument dostawcy rozszerzenia.

### <a name="what-is-a-document-provider-extension"></a>Co to jest rozszerzenie dostawcy dokumentu?

Krótko mówiąc, rozszerzenie dostawcy dokumentu jest sposób deweloperem lub innych firm, aby zapewnić magazynu dokument aplikacji, który jest dostępny w dokładnie sam sposób jak istniejącej lokalizacji magazynu usługi iCloud.

Użytkownik może wybrać jedną z tych lokalizacji alternatywnej magazynu z selektora dokumentów i mogą użyć dokładnie tego samego tryby dostępu (Open, Import, Przenieś lub eksportu), Praca z plikami w tej lokalizacji.

Ten sposób jest implementowany przy użyciu dwóch różnych rozszerzeń:

-  **Dokument rozszerzenia selektora** — zapewnia `UIViewController` podklasy, która udostępnia graficzny interfejs użytkownika do wyboru alternatywnej lokalizacji przechowywania dokumentu. To podklasa będą wyświetlane jako część kontroler widoku selektora dokumentu.
-  **Podaj rozszerzenie pliku** — jest to rozszerzenie bez interfejsu użytkownika, która zajmuje się faktycznie dostarczanie zawartości plików. Te rozszerzenia są realizowane za pośrednictwem pliku koordynacji ( `NSFileCoordinator` ). Jest to inny ważne gdzie koordynacji pliku jest wymagana.


Na poniższym diagramie przedstawiono przepływ typowych danych podczas pracy z dokumentu dostawcy rozszerzeń:

 [![](document-picker-images/image39.png "Ten diagram przedstawia przepływ typowych danych, podczas pracy z rozszerzeniami dostawcy dokumentu")](document-picker-images/image39.png#lightbox)

Odbywa się następujący proces:

1.  Kontrolera selektora dokumentu, aby zezwolić użytkownikowi na wybranie plików do pracy z przedstawia informacje o aplikacji.
1.  Użytkownik wybiera alternatywną lokalizację i pliku niestandardowego `UIViewController` rozszerzenia jest wywoływana w celu wyświetlania interfejsu użytkownika.
1.  Użytkownik wybiera pliku z tej lokalizacji i adres URL jest przekazywane z powrotem do selektora dokumentu.
1.  Selektor dokumentu wybiera adres URL pliku i zwraca go do aplikacji dla użytkownika do pracy nad.
1.  Adres URL jest przekazywany do koordynatora plik, aby zwrócić zawartość plików w aplikacji.
1.  Koordynator plik wywołuje niestandardowe rozszerzenie dostawcy do pobierania pliku.
1.  Zawartość pliku są zwracane do koordynatora pliku.
1.  Zawartość pliku są zwracane do aplikacji.


### <a name="security-and-bookmarks"></a>Bezpieczeństwo i zakładki

W tej sekcji potrwa krótki przegląd sposobu zabezpieczeń i plik trwałego uzyskiwać dostęp do działania zakładki z rozszerzeniami dostawcy dokumentu. W przeciwieństwie do usługi iCloud dostawcy dokumentu, który automatycznie zapisuje zabezpieczeń i zakładek do kontenera aplikacji, dokumentu dostawca rozszerzeń nie ponieważ nie są częścią systemu odniesienia dokumentu.

Na przykład: w ustawieniu przedsiębiorstwa, który zapewnia bezpieczny magazyn danych własnej firmie, Administratorzy nie mają poufnych informacji firmowych przetworzone przez publiczny iCloud serwerów lub uzyskać dostępu do. W związku z tym nie można użyć wbudowanych systemu odniesienia dokumentu.

System zakładki można nadal stosować i jest obowiązkiem dostawcy rozszerzenie, aby poprawnie przetworzyć adres URL zakładką i zwraca zawartość dokumentu wskazywanego przez nią.

Ze względów bezpieczeństwa z systemem iOS 8 ma warstwy izolacji, która będzie nadal występował, informacje o tym, które aplikacja ma już dostępu do których identyfikator, wewnątrz którego dostawcy plików. Należy zauważyć, że dostęp do wszystkich plików jest kontrolowany przez tę warstwę izolacji.

Na poniższym diagramie przedstawiono przepływ danych podczas pracy z zakładek i rozszerzenie dostawcy dokumentu:

 [![](document-picker-images/image40.png "Ten diagram przedstawia przepływ danych podczas pracy z zakładek i rozszerzenie dostawcy dokumentu")](document-picker-images/image40.png#lightbox)

Odbywa się następujący proces:

1.  Aplikacja ma wprowadź tła i musi być zachowany jego stanu. Wywołuje `NSUrl` do tworzenia zakładek do pliku w magazynie alternatywnych.
1.  `NSUrl` wywołuje rozszerzenie dostawcy pliku do pobrania adresu URL trwałych do dokumentu. 
1.  Rozszerzenie pliku dostawcy zwraca adres URL jako ciąg, aby `NSUrl` .
1.  `NSUrl` Zawiera adres URL do zakładki i zwraca go do aplikacji.
1.  Gdy aplikacja jest przywracany z znajdujące się w tle i wymaga przywrócić stan, przekazuje on zakładkę do `NSUrl` .
1.  `NSUrl` wywołuje rozszerzenie pliku dostawcy z adresu URL pliku.
1.  Dostawca rozszerzeń plików uzyskuje dostęp do pliku i zwraca lokalizację pliku do `NSUrl` .
1.  Lokalizacja pliku jest dołączany do informacji o zabezpieczeniach i zwrócony do aplikacji.


W tym miejscu aplikacji można uzyskać dostępu do pliku i pracować z nim w normalnym.

### <a name="writing-files"></a>Zapisywanie plików

W tej sekcji potrwa krótki przegląd sposobu pisania pliki do lokalizacji alternatywnej z działa dokumentu dostawcy rozszerzenia. Aplikacje systemu iOS użyje koordynacji pliku do zapisania informacji o dysku wewnątrz kontenera aplikacji. Wkrótce po pomyślnie zapisany plik z rozszerzeniem dostawcy zostanie powiadomiony o zmianę.

W tym momencie rozszerzenie dostawcy można rozpocząć przekazywania pliku do lokalizacji alternatywnej (lub oznaczyć go jako zakłócone i wymaganie przekazywania).

### <a name="creating-new-document-provider-extensions"></a>Tworzenie nowych dokumentów dostawcy rozszerzeń

Tworzenie nowych dokumentów dostawcy rozszerzeń znajduje się poza zakres tego artykułu wstępne. Informacje są podane tutaj wyświetlane, na podstawie na rozszerzenia, które użytkownik został załadowany w urządzeniu z systemem iOS, aplikacji może mieć dostęp do lokalizacji magazynu dokumentu poza Apple podana lokalizacja usługi iCloud.

Deweloper musi mieć pamiętać o tym, gdy za pomocą selektora dokumentu i Praca z dokumentami zewnętrznych. Nie należy ich zakładać tych dokumentów są obsługiwane w ramach usługi iCloud.

Aby uzyskać więcej informacji o tworzeniu dostawcy magazynu lub dokument selektora rozszerzenia, zobacz [wprowadzenie do rozszerzeń aplikacji](~/ios/platform/extensions.md) dokumentu.

## <a name="migrating-to-icloud-drive"></a>Migracja do usługi iCloud dysku

W systemie iOS 8 użytkownicy mogą wybierać kontynuować przy użyciu istniejącej usługi iCloud dokumenty System używany w systemów iOS 7 (i starszymi systemami) lub ich można przeprowadzić migrację istniejących dokumentów do nowego mechanizmu dysku iCloud.

Na Mac OS X Yosemite, Apple nie wymaga zapewnienia zgodności wszystkie dokumenty muszą być migrowane z usługą iCloud dysku lub ich nie będzie można zaktualizować na urządzeniach.

Po przeprowadzeniu migracji konta użytkownika do usługi iCloud dysku tylko na urządzeniach przy użyciu usługi iCloud dysku będzie można propagować zmian do dokumentów na tych urządzeniach.

> [!IMPORTANT]
> **Uwaga**: deweloperzy należy zwrócić uwagę, że nowe funkcje omówione w tym artykule są dostępne tylko jeśli przeprowadzono migrację konta użytkownika do usługi iCloud dysku. 

## <a name="summary"></a>Podsumowanie

W tym artykule ma opisano zmiany w usłudze iCloud istniejących interfejsów API wymagane do obsługi iCloud dysku i nowego kontrolera widoku selektora dokumentu. Pokrywającego koordynacji pliku i dlaczego jest ważne podczas pracy z dokumentami oparte na chmurze. Ma ona pasuje do ustawienia wymagane do włączenia usługi opartej na chmurze dokumentów w aplikacji platformy Xamarin.iOS i podane wprowadzające przyjrzeć Praca z dokumentami spoza kontenera aplikacji aplikacji przy użyciu kontrolera widoku selektora dokumentu.

Ponadto w tym artykule krótko opisano rozszerzenia dostawcy dokumentów i dlaczego dewelopera należy zwrócić uwagę ich podczas pisania aplikacji, które może obsłużyć dokumenty oparte na chmurze.

## <a name="related-links"></a>Linki pokrewne

- [DocPicker (przykład)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [Wprowadzenie do systemu iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Wprowadzenie do rozszerzeń aplikacji](~/ios/platform/extensions.md)
