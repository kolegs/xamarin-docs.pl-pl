---
title: Za pomocą platformy Xamarin.iOS przy użyciu usługi iCloud
description: W tym dokumencie opisano, w usłudze iCloud i jego użycia w aplikacji platformy Xamarin.iOS. Omówiono w nim magazyn kluczy i wartości, do przechowywania dokumentów i kopii zapasowych w usłudze iCloud.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/09/2016
ms.openlocfilehash: b72ecc40994d9336c4941f3db700796edd80e81f
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353220"
---
# <a name="using-icloud-with-xamarinios"></a>Za pomocą platformy Xamarin.iOS przy użyciu usługi iCloud

Magazyn iCloud interfejsu API w systemie iOS 5 umożliwia aplikacji, aby zapisać dokumenty użytkowników i dane specyficzne dla aplikacji do centralnej lokalizacji i mieć dostęp do tych elementów ze wszystkich urządzeń użytkownika.

Dostępne są cztery typy magazynów:

- **Magazyn kluczy i wartości** — do udostępnienia małe ilości danych za pomocą aplikacji w innych urządzeń użytkownika.

- **Magazyn UIDocument** — do przechowywania dokumentów i innych danych na koncie usługi iCloud użytkownika przy użyciu podklasa UIDocument.

- **CoreData** -Magazyn bazy danych SQLite.

- **Pojedyncze pliki i katalogi** — Zarządzanie wiele różnych plików bezpośrednio w systemie plików.

W tym dokumencie omówiono pierwsze dwa typy — pary klucz-wartość i podklasy UIDocument — i jak korzystać z tych funkcji rozszerzenia Xamarin.iOS.

> [!IMPORTANT]
> Apple [udostępnia narzędzia](https://developer.apple.com/support/allowing-users-to-manage-data/) pomagające deweloperom poprawnie obsługiwać Unii Europejskiej ogólnego danych (GDPR Protection Regulation).

## <a name="requirements"></a>Wymagania

- Najnowsza stabilna wersja rozszerzenia Xamarin.iOS
- Narzędzia Xcode 8 lub nowszego
- Program Visual Studio for Mac lub Visual Studio 2015 i nowszych.

## <a name="preparing-for-icloud-development"></a>Trwa przygotowywanie do tworzenia aplikacji w usłudze iCloud

Aplikacje muszą być skonfigurowane do użycia w usłudze iCloud zarówno w [portalu aprowizacji firmy Apple](https://developer.apple.com/account/ios/overview.action) i samym projekcie. Zanim tworzeniu aplikacji w usłudze iCloud (lub wypróbowania przykładów) wykonaj następujące czynności.

Aby poprawnie skonfigurować aplikację do dostępu do usługi iCloud:

-   **Znajdź identyfikator Twojego zespołu** — Zaloguj się do [developer.apple.com](http://developer.apple.com) i odwiedź stronę **Member Center przeznaczonej > konta > Podsumowanie konta dewelopera** identyfikator zespołu (lub poszczególne identyfikator dla pojedynczego deweloperów ). Będzie on ciąg znaków 10 ( **A93A5CM278** na przykład) — ta stanowi część "identyfikator kontenera".

-   **Utwórz nowy identyfikator aplikacji** — Aby utworzyć identyfikator aplikacji, wykonaj czynności opisane w temacie [Inicjowanie obsługi administracyjnej technologii Store części przewodnika Device Provisioning](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)i należy koniecznie sprawdzić **iCloud** jako dozwolone usługi:

 [![](introduction-to-icloud-images/icloud-sml.png "Sprawdź iCloud jako dozwolone usługi")](introduction-to-icloud-images/icloud.png#lightbox)

- **Utwórz nowy profil inicjowania obsługi administracyjnej** — Aby utworzyć profil inicjowania obsługi administracyjnej, należy wykonać czynności opisane w temacie [Device Provisioning guide](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) .

- **Dodaj identyfikator kontenera do pliku Entitlements.plist** — format identyfikator kontenera to `TeamID.BundleID`. Aby uzyskać więcej informacji zobacz [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

- **Konfigurowanie właściwości projektu** — w pliku Info.plist, upewnij się, plik **identyfikatora pakietu** odpowiada **identyfikator pakietu** ustawiane podczas [tworzenie Identyfikatora aplikacji ](~/ios/deploy-test/provisioning/capabilities/index.md); Używa podpisywanie zbiorów systemu iOS **profilu aprowizacji** zawierające identyfikator aplikacji za pomocą usługi App Service w usłudze iCloud i **niestandardowe uprawnienia** wybrano plik. Wszystkie można to zrobić w programie Visual Studio w okienku właściwości projektu.

- **Włącz usługę iCloud na urządzenie** — przejdź do **Ustawienia > iCloud** i upewnij się, że urządzenie jest zalogowany.
Wybierz i Włącz **dokumenty i dane** opcji.

- **Korzystasz z urządzenia do przetestowania iCloud** — nie będą działać w symulatorze.
W rzeczywistości tak naprawdę potrzebne dwie lub większą liczbę urządzeń wszystkie zalogowany za pomocą tego samego identyfikatora Apple ID, aby zobaczyć działanie w usłudze iCloud.


## <a name="key-value-storage"></a>Magazyn kluczy i wartości

Magazyn kluczy i wartości jest przeznaczony dla niewielkich ilości danych, który użytkownik może być utrwalona różnych urządzeń —, takich jak ostatniej strony, które są wyświetlane w książce lub magazyn. Magazyn kluczy i wartości nie można używać do tworzenia kopii zapasowych danych.

Istnieją pewne ograniczenia, w których trzeba pamiętać podczas korzystania z magazynu kluczy i wartości:

- **Maksymalny rozmiar klucza** -nazw klucz nie może być dłuższa niż 64 bajtów.

- **Maksymalna wartość rozmiaru** — nie można zapisać więcej niż 64 kilobajtów w postaci pojedynczej wartości.

- **Rozmiar maksymalny parach klucz wartość dla aplikacji** — aplikacje tylko może przechowywać maksymalnie 64 kilobajtów danych klucz wartość w sumie. Próbuje ustawić klucze po przekroczeniu tego limitu zakończy się niepowodzeniem i utrwali poprzedniej wartości.

- **Typy danych** — tylko typy podstawowe, takie jak ciągi, liczby i wartości logiczne, które mogą być przechowywane.

**ICloudKeyValue** przykład pokazuje, jak to działa. Przykładowy kod tworzy klucz o nazwie dla każdego urządzenia: można ustawić tego klucza na jednym urządzeniu i oglądać wartości Pobierz propagowane do innych osób. Tworzy wartość dla kucza zwanego "Udostępnione", który może być edytowany na dowolnym urządzeniu — Jeśli edytujesz na wielu urządzeniach na raz, w usłudze iCloud zdecyduje wartość "wins" (przy użyciu znacznika czasu na zmianie), która pobiera propagowane.

Ten zrzut ekranu pokazuje przykład używany. Po odebraniu powiadomienia o zmianach z usługi iCloud są drukowane w przewijania widoku tekstu w dolnej części ekranu i aktualizowane w pól wejściowych.



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "Przepływ komunikatów między urządzeniami")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>Ustawianie i pobieranie danych

Ten kod pokazuje, jak można ustawić wartość ciągu.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Wywołanie synchronizacji gwarantuje, że wartość jest trwały, aby tylko magazyn na dysku lokalnym. Synchronizacja do usługi iCloud odbywa się w tle i nie może być "wymuszone" przez kod aplikacji. Za pomocą prawidłowe połączenie z siecią synchronizacji często nastąpi w ciągu 5 sekund, ale jeśli sieć jest niska (lub odłączona) aktualizacja może trwać znacznie dłużej.

Możesz pobrać wartość przy użyciu tego kodu:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

Wartość jest pobierana z magazynu danych lokalnych — ta metoda nie jest podejmowana próba kontaktuje się z serwerami usługi iCloud, aby uzyskać wartość "najnowsza". w usłudze iCloud zostaną zaktualizowane lokalnym magazynie danych zgodnie z harmonogramem swój własny.

### <a name="deleting-data"></a>Usuwanie danych

Aby całkowicie usunąć pary klucz wartość, należy użyć metody Usuń następująco:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>Monitorowanie zmian

Aplikacji mogą także otrzymywać powiadomienia, gdy wartości są zmieniane przez usługi iCloud, dodając obserwatora `NSNotificationCenter.DefaultCenter`.
Poniższy kod z **KeyValueViewController.cs** `ViewWillAppear` metoda pokazuje, jak do nasłuchiwania tych powiadomień i Utwórz listę, z których klucze zostały zmienione:

```csharp
keyValueNotification =
NSNotificationCenter.DefaultCenter.AddObserver (
    NSUbiquitousKeyValueStore.DidChangeExternallyNotification, notification => {
    Console.WriteLine ("Cloud notification received");
    NSDictionary userInfo = notification.UserInfo;

    var reasonNumber = (NSNumber)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangeReasonKey);
    nint reason = reasonNumber.NIntValue;

    var changedKeys = (NSArray)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangedKeysKey);
    var changedKeysList = new List<string> ();
    for (uint i = 0; i < changedKeys.Count; i++) {
        var key = changedKeys.GetItem<NSString> (i); // resolve key to a string
        changedKeysList.Add (key);
    }
    // now do something with the list...
});
```

Kodzie następnie może potrwać niektóre akcje, z listą zmienionych kluczy, takie jak aktualizowanie lokalną kopię je lub aktualizowania interfejsu użytkownika z nowymi wartościami.

Zmiana możliwe przyczyny: ServerChange (0), InitialSyncChange (1) lub QuotaViolationChange (2). Masz dostęp przyczynę i wykonać inne przetwarzanie, jeśli jest to wymagane (na przykład może być konieczne usunięcie niektórych kluczy na *QuotaViolationChange*).

## <a name="document-storage"></a>Przechowywanie dokumentów

Przechowywanie dokumentów w usłudze iCloud jest przeznaczona do zarządzania danymi, które są ważne do aplikacji (i użytkownik). Może służyć do zarządzania plikami i inne dane, które Twoja aplikacja wymaga do uruchomienia, a jednocześnie zapewniając kopie zapasowe oparte na usłudze iCloud i udostępniania funkcji między wszystkich urządzeń użytkownika.

Ten diagram przedstawia, jak to wszystko dopasowuje się ze sobą. Każde urządzenie ma dane zapisane w lokalnej pamięci masowej (UbiquityContainer) i usługi iCloud systemu operacyjnego, który demona dba o wysyłania i odbierania danych w chmurze. Dostęp do wszystkich plików do UbiquityContainer musi odbywać się za pośrednictwem FilePresenter/FileCoordinator, aby zapobiec równoczesny dostęp. `UIDocument` Klasa implementuje te dla Ciebie; w tym przykładzie pokazano, jak używać UIDocument.

 [![](introduction-to-icloud-images/icloud-overview.png "Omówienie magazynu dokumentów")](introduction-to-icloud-images/icloud-overview.png#lightbox)

W przykładzie iCloudUIDoc implementuje prosty `UIDocument` podklasy, który zawiera jedno pole tekstowe. Tekst jest renderowany w `UITextView` i zmiany są propagowane przez z innymi urządzeniami w usłudze iCloud z komunikatem powiadomienia, pokazane na czerwono. Przykładowy kod dotyczy bardziej zaawansowane funkcje usługi iCloud, takie jak rozwiązywanie konfliktów.

Ten zrzut ekranu pokazuje Przykładowa aplikacja — po zmiana tekstu, a następnie naciskając klawisz **UpdateChangeCount** dokumentu są synchronizowane za pomocą z innymi urządzeniami w usłudze iCloud.

 [![](introduction-to-icloud-images/iclouduidoc.png "Ten zrzut ekranu pokazuje Przykładowa aplikacja po zmiana tekstu, a następnie naciskając klawisz UpdateChangeCount")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

Dostępne są przykładowe iCloudUIDoc, składa się z pięciu części:

1. **Uzyskiwanie dostępu do UbiquityContainer** -ustalić, czy jest włączone w usłudze iCloud i jeśli tak jest ścieżką do obszaru magazynu usługi iCloud Twojej aplikacji.

1. **Tworzenie podklas UIDocument** — Utwórz klasę w celu pośrednich kont usługi storage w usłudze iCloud i obiekty modelu.

1. **Znajdowanie i otwieranie dokumentów w usłudze iCloud** — użyj `NSFileManager` i `NSPredicate` można znaleźć dokumenty iCloud i otworzyć je.

1. **Wyświetlanie dokumentów w usłudze iCloud** — udostępnianie właściwości z usługi `UIDocument` tak, aby interaktywnie kontrolek interfejsu użytkownika.

1. **Zapisywanie dokumentów w usłudze iCloud** — upewnij się, czy zmiany wprowadzone w interfejsie użytkownika są zachowywane na dysku i usługi iCloud.

Wszystkie operacje w usłudze iCloud uruchomić (lub powinno być ono uruchomione) asynchronicznie, aby nie powodują one blokadę podczas oczekiwania na coś, co ma być wykonywana. Zostaną wyświetlone trzy różne sposoby realizacji tego zadania w przykładzie:

 **Wątki** — w `AppDelegate.FinishedLaunching` początkowe wywołanie `GetUrlForUbiquityContainer` odbywa się w innym wątku, aby zapobiec blokowaniu wątku głównego.

 **NotificationCenter** — rejestrowania powiadomień, gdy asynchroniczne operacje takie jak `NSMetadataQuery.StartQuery` ukończone.

 **Programy obsługi zakończenia** — przekazując metody służące do uruchomienia po zakończeniu operacji asynchronicznych, takich jak `UIDocument.Open`.

### <a name="accessing-the-ubiquitycontainer"></a>Uzyskiwanie dostępu do UbiquityContainer

Pierwszym krokiem przy użyciu usługi iCloud przechowywania dokumentów ma na celu określenie, czy jest włączony w usłudze iCloud, a jeżeli tak lokalizację "container powszechność" (katalog przechowywania plików włączone usługi iCloud na urządzenie).

Ten kod znajduje się w `AppDelegate.FinishedLaunching` metoda próbki.

```csharp
// GetUrlForUbiquityContainer is blocking, Apple recommends background thread or your UI will freeze
ThreadPool.QueueUserWorkItem (_ => {
    CheckingForiCloud = true;
    Console.WriteLine ("Checking for iCloud");
    var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer (null);
    // OR instead of null you can specify "TEAMID.com.your-company.ApplicationName"

    if (uburl == null) {
        HasiCloud = false;
        Console.WriteLine ("Can't find iCloud container, check your provisioning profile and entitlements");

        InvokeOnMainThread (() => {
            var alertController = UIAlertController.Create ("No \uE049 available",
            "Check your Entitlements.plist, BundleId, TeamId and Provisioning Profile!", UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Destructive, null));
            viewController.PresentViewController (alertController, false, null);
        });
    } else { // iCloud enabled, store the NSURL for later use
        HasiCloud = true;
        iCloudUrl = uburl;
        Console.WriteLine ("yyy Yes iCloud! {0}", uburl.AbsoluteUrl);
    }
    CheckingForiCloud = false;
});
```

Mimo, że plik nie jest to, Firma Apple zaleca wywołanie GetUrlForUbiquityContainer zawsze wtedy, gdy aplikacja przejdzie do pierwszego planu.

### <a name="creating-a-uidocument-subclass"></a>Tworzenie podklas UIDocument

Wszystkie pliki w usłudze iCloud i katalogi (tj. nic przechowywane w katalogu UbiquityContainer) muszą być zarządzane przy użyciu metod NSFileManager, implementacja protokołu NSFilePresenter i zapisywanie za pośrednictwem NSFileCoordinator.
Najprostszym sposobem, aby zrobić to wszystko nie należy napisać ją samodzielnie, ale podklasy UIDocument, która zajmuje się.

Istnieją tylko dwie metody, które należy zaimplementować podklasy UIDocument pracować w usłudze iCloud:

- **LoadFromContents** -przekazuje NSData treści pliku dla Ciebie do rozpakowania do swojej klasy modelu/es.

- **ContentsForType** — trzeba będzie podać NSData reprezentacja swojej klasy modelu/es żądania do zapisania na dysku (i w chmurze).

Ten przykładowy kod z **iCloudUIDoc\MonkeyDocument.cs** przedstawia sposób implementowania UIDocument.

```csharp
public class MonkeyDocument : UIDocument
{
    // the 'model', just a chunk of text in this case; must easily convert to NSData
    NSString dataModel;
    // model is wrapped in a nice .NET-friendly property
    public string DocumentString {
        get {
            return dataModel.ToString ();
        }
        set {
            dataModel = new NSString (value);
        }
    }
    public MonkeyDocument (NSUrl url) : base (url)
    {
        DocumentString = "(default text)";
    }
    // contents supplied by iCloud to display, update local model and display (via notification)
    public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("LoadFromContents({0})", typeName);

        if (contents != null)
            dataModel = NSString.FromData ((NSData)contents, NSStringEncoding.UTF8);

        // LoadFromContents called when an update occurs
        NSNotificationCenter.DefaultCenter.PostNotificationName ("monkeyDocumentModified", this);
        return true;
    }
    // return contents for iCloud to save (from the local model)
    public override NSObject ContentsForType (string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("ContentsForType({0})", typeName);
        Console.WriteLine ("DocumentText:{0}",dataModel);

        NSData docData = dataModel.Encode (NSStringEncoding.UTF8);
        return docData;
    }
}
```

Model danych w tym przypadku jest bardzo proste — jedno pole tekstowe. Modelu danych może być złożonym, wymagane, na przykład dokumentu Xml lub dane binarne. Podstawową rolą implementacji UIDocument jest tłumaczenie klasach modeli i reprezentację w postaci NSData można zapisać/załadować na dysku.

### <a name="finding-and-opening-icloud-documents"></a>Znajdowanie i otwieranie dokumentów w usłudze iCloud

Dotyczy tylko przykładową aplikację z pojedynczym plikiem — jako — więc kod w **AppDelegate.cs** tworzy `NSPredicate` i `NSMetadataQuery` do wyszukiwania specjalnie dla tej nazwy pliku. `NSMetadataQuery` Jest uruchamiane asynchronicznie, a następnie wysyła powiadomienie po zakończeniu tej operacji. `DidFinishGathering` jest wywoływane przez obserwatora powiadomień, zatrzymuje zapytania i wywołuje LoadDocument, który używa `UIDocument.Open` metody za pomocą procedury obsługi zakończenia, aby spróbować załadować pliku i wyświetlić ją w `MonkeyDocumentViewController`.

```csharp
string monkeyDocFilename = "test.txt";
void FindDocument ()
{
    Console.WriteLine ("FindDocument");
    query = new NSMetadataQuery {
        SearchScopes = new NSObject [] { NSMetadataQuery.UbiquitousDocumentsScope }
    };

    var pred = NSPredicate.FromFormat ("%K == %@", new NSObject[] {
        NSMetadataQuery.ItemFSNameKey, new NSString (MonkeyDocFilename)
    });

    Console.WriteLine ("Predicate:{0}", pred.PredicateFormat);
    query.Predicate = pred;

    NSNotificationCenter.DefaultCenter.AddObserver (
        this,
        new Selector ("queryDidFinishGathering:"),
        NSMetadataQuery.DidFinishGatheringNotification,
        query
    );

    query.StartQuery ();
}

[Export ("queryDidFinishGathering:")]
void DidFinishGathering (NSNotification notification)
{
    Console.WriteLine ("DidFinishGathering");
    var metadataQuery = (NSMetadataQuery)notification.Object;
    metadataQuery.DisableUpdates ();
    metadataQuery.StopQuery ();

    NSNotificationCenter.DefaultCenter.RemoveObserver (this, NSMetadataQuery.DidFinishGatheringNotification, metadataQuery);
    LoadDocument (metadataQuery);
}

void LoadDocument (NSMetadataQuery metadataQuery)
{
    Console.WriteLine ("LoadDocument");

    if (metadataQuery.ResultCount == 1) {
        var item = (NSMetadataItem)metadataQuery.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);
        doc = new MonkeyDocument (url);

        doc.Open (success => {
            if (success) {
                Console.WriteLine ("iCloud document opened");
                Console.WriteLine (" -- {0}", doc.DocumentString);
                viewController.DisplayDocument (doc);
            } else {
                Console.WriteLine ("failed to open iCloud document");
            }
        });
    } // TODO: if no document, we need to create one
}
```

### <a name="displaying-icloud-documents"></a>Wyświetlanie dokumentów w usłudze iCloud

Wyświetlanie UIDocument nie powinny mieć inny do każdej klasy modelu
- właściwości są wyświetlane w kontrolek interfejsu użytkownika, prawdopodobnie edytowane przez użytkownika, a następnie zapisywane z powrotem do modelu.

W przykładzie **iCloudUIDoc\MonkeyDocumentViewController.cs** Wyświetla tekst MonkeyDocument w `UITextView`. `ViewDidLoad` nasłuchuje powiadomienie wysłane `MonkeyDocument.LoadFromContents` metody. `LoadFromContents` jest wywoływana, gdy w usłudze iCloud ma nowe dane do tego pliku, dzięki czemu powiadomienia oznacza, że dokument został zaktualizowany.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

Obsługi powiadomień przykładowy kod wywołuje metodę, aby zaktualizować interfejs użytkownika — w tym przypadku bez żadnych konfliktów bądź rozpoznawanie nazw.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>Zapisywanie dokumentów w usłudze iCloud

Aby dodać UIDocument można wywołać w ramach usługi icloud `UIDocument.Save` bezpośrednio (tylko w przypadku nowych dokumentów) lub przenieść istniejący plik przy użyciu `NSFileManager.DefaultManager.SetUbiquitious`. Przykładowy kod tworzy nowy dokument bezpośrednio w kontenerze powszechność przy użyciu tego kodu (istnieją dwa zakończenia obsługi w tym miejscu, jeden dla `Save` operacji i inny wpis dla Otwórz):

```csharp
var docsFolder = Path.Combine (iCloudUrl.Path, "Documents"); // NOTE: Documents folder is user-accessible in Settings
var docPath = Path.Combine (docsFolder, MonkeyDocFilename);
var ubiq = new NSUrl (docPath, false);
var monkeyDoc = new MonkeyDocument (ubiq);
monkeyDoc.Save (monkeyDoc.FileUrl, UIDocumentSaveOperation.ForCreating, saveSuccess => {
Console.WriteLine ("Save completion:" + saveSuccess);
if (saveSuccess) {
    monkeyDoc.Open (openSuccess => {
        Console.WriteLine ("Open completion:" + openSuccess);
        if (openSuccess) {
            Console.WriteLine ("new document for iCloud");
            Console.WriteLine (" == " + monkeyDoc.DocumentString);
            viewController.DisplayDocument (monkeyDoc);
        } else {
            Console.WriteLine ("couldn't open");
        }
    });
} else {
    Console.WriteLine ("couldn't save");
}
```

Kolejne zmiany w dokumencie nie są "zapisywane" bezpośrednio, zamiast tego o `UIDocument` o zmianie za pomocą `UpdateChangeCount`, i automatycznie zaplanowane Zapisz na dysku operację:

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>Zarządzanie dokumentami w usłudze iCloud

Użytkownicy mogą zarządzać dokumenty iCloud w **dokumenty** katalogu "container powszechność" poza aplikację za pomocą ustawień; można wyświetlić listę plików i przejdź do usunięcia. Kod aplikacji powinien być w stanie obsługiwać sytuację, w którym zostaną one usunięte przez użytkownika. Nie należy przechowywać dane aplikacji wewnętrznej w **dokumenty** katalogu.

 [![](introduction-to-icloud-images/icloudstorage.png "Zarządzanie przepływem pracy dokumenty iCloud")](introduction-to-icloud-images/icloudstorage.png#lightbox)



Użytkownicy otrzymają również różne ostrzeżenia podczas próby usunięcia aplikacji z obsługą usługi iCloud ze swojego urządzenia, aby poinformować o stanie dokumenty iCloud związane z tej aplikacji.

 [![](introduction-to-icloud-images/icloud-delete1.png "Przykładowe okno dialogowe, gdy użytkownik próbuje usunąć aplikację z obsługą usługi iCloud ze swojego urządzenia")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "Przykładowe okno dialogowe, gdy użytkownik próbuje usunąć aplikację z obsługą usługi iCloud ze swojego urządzenia")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>Kopia zapasowa w usłudze iCloud

Podczas wykonywania kopii zapasowych usługi iCloud nie jest funkcją, która jest bezpośrednio dostępny przez deweloperów, sposób projektowania aplikacji może mieć wpływ na środowisko pracy użytkownika.
Udostępnia Apple [iOS wytyczne dotyczące magazynu danych](http://developer.apple.com/icloud/documentation/data-storage/) dla deweloperów w swoich aplikacjach dla systemu iOS.

Najważniejszą kwestią jest, czy Twoja aplikacja przechowuje duże pliki, które nie są generowane przez użytkownika (na przykład aplikację czytelnik czasopisma, która przechowuje zawartości na problem w megabajtach hundred-plus). Apple preferuje, nie należy przechowywać tego rodzaju danych, którym będzie można kopii zapasowej w chmurze iCloud i niepotrzebnie wypełnienia przydział w usłudze iCloud użytkownika.

Aplikacje, które przechowywania dużych ilości danych, takich jak to należy przechowywać go albo w jednym z katalogów użytkownika, nie jest kopii zapasowej (np. Pamięci podręczne lub tmp) lub użyj `NSFileManager.SetSkipBackupAttribute` do zastosowania flagi do tych plików, tak, aby w usłudze iCloud będzie je ignorować podczas operacji tworzenia kopii zapasowej.

## <a name="summary"></a>Podsumowanie

Ten artykuł zawiera wprowadzenie nowej funkcji w usłudze iCloud zawarte w systemie iOS 5. Kroki wymagane do skonfigurowania projektu do użycia w usłudze iCloud i następnie podane przykłady sposobu implementowania funkcji w usłudze iCloud go zbadać.

W przykładzie magazyn kluczy i wartości pokazano, jak iCloud może służyć do przechowywania niewielką ilość danych, podobnie jak NSUserPreferences są przechowywane. W przykładzie UIDocument pokazano, jak więcej złożonych danych może być przechowywane i synchronizowane na wielu urządzeniach za pośrednictwem usługi iCloud.

Na koniec dołączone krótkie Omówiliśmy sposób dodawania kopii zapasowej w usłudze iCloud powinny mieć wpływ na projekt aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do usługi iCloud (przykład)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [Seminarium przykładowego kodu w usłudze iCloud](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [Slajdy seminarium w usłudze iCloud](http://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [Magazyn w usłudze iCloud](http://support.apple.com/kb/HT4847)
