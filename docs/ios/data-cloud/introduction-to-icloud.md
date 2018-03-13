---
title: iCloud
description: "Firma Apple wprowadziła iCloud w systemie iOS 5 jako usługa, aby umożliwić aplikacjom przechowywanie danych na serwerach firmy Apple i jego synchronizację wszystkie urządzenia używane przez tę samą osobę (za pośrednictwem ich identyfikator Apple ID). Ma również składnik kopii zapasowej, której dane na urządzeniach jest kopii zapasowej do serwerów firmy Apple. Ten dokument zawiera opis sposobu korzystać z niektórych iCloud interfejsów API podany przez firmę Apple do przechowywania i pobierania danych ze swoich serwerów, z C# próbki do przechowywania par klucz wartość niewielkie zbiory danych i do przechowywania dokumentów. Omówiono również, jak iCloud kopii zapasowej może mieć wpływ projektu danej aplikacji."
ms.topic: article
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/09/2016
ms.openlocfilehash: ce2130985eb954abc4b4a1f4022eec97341eb902
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="icloud"></a>iCloud

_Firma Apple wprowadziła iCloud w systemie iOS 5 jako usługa, aby umożliwić aplikacjom przechowywanie danych na serwerach firmy Apple i jego synchronizację wszystkie urządzenia używane przez tę samą osobę (za pośrednictwem ich identyfikator Apple ID). Ma również składnik kopii zapasowej, której dane na urządzeniach jest kopii zapasowej do serwerów firmy Apple. Ten dokument zawiera opis sposobu korzystać z niektórych iCloud interfejsów API podany przez firmę Apple do przechowywania i pobierania danych ze swoich serwerów, z C# próbki do przechowywania par klucz wartość niewielkie zbiory danych i do przechowywania dokumentów. Omówiono również, jak iCloud kopii zapasowej może mieć wpływ projektu danej aplikacji._

Magazyn iCloud interfejsu API w systemie iOS 5 umożliwia aplikacji, aby zapisać użytkownika dokumenty i dane specyficzne dla aplikacji w lokalizacji centralnej i uzyskiwać dostęp do tych elementów z wszystkich urządzeń użytkownika.

Dostępne są cztery typy magazynu:

- **Magazyn kluczy i wartości** — Aby udostępnić niewielkich ilości danych z aplikacji na innych urządzeniach użytkownika.

- **Magazyn UIDocument** — do przechowywania dokumentów i innych danych na koncie usługi iCloud użytkownika przy użyciu podklasą klasy UIDocument.

- **CoreData** -magazynu bazy danych SQLite.

- **Poszczególnych plików i katalogów** — do zarządzania wiele różnych plików bezpośrednio w systemie plików.

W tym dokumencie omówiono pierwsze dwa typy — pary klucz-wartość i podklasy UIDocument — oraz sposób używania tych funkcji w platformy Xamarin.iOS.

## <a name="requirements"></a>Wymagania

- Najnowsza stabilna wersja platformy Xamarin.iOS
- Xcode 8 lub nowszego
- Program Visual Studio dla komputerów Mac lub Visual Studio 2015 i nowszych.

## <a name="preparing-for-icloud-development"></a>Przygotowanie do tworzenia aplikacji w ramach usługi iCloud

Aplikacje muszą być skonfigurowane do używania iCloud zarówno w [portalu inicjowania obsługi administracyjnej Apple](https://developer.apple.com/account/ios/overview.action) i samym projekcie. Przed tworzenie aplikacji dla usługi iCloud (lub wypróbować próbek) wykonaj poniższe kroki.

Aby poprawnie skonfigurować aplikacji można uzyskać dostępu do usługi iCloud:

-   **Znajdź identyfikator Twojego zespołu** — logowanie do [developer.apple.com](http://developer.apple.com) , odwiedź **Member Center > Twoje konto > Podsumowanie konta dewelopera** Pobierz identyfikator zespołu (lub poszczególnych identyfikator dla pojedynczego deweloperów ). Będzie to ciąg znaków 10 ( **A93A5CM278** na przykład)-to jest częścią "identyfikator kontenera".

-   **Utwórz nowy identyfikator aplikacji** — Aby utworzyć identyfikator aplikacji, wykonaj czynności opisane w temacie [Inicjowanie obsługi administracyjnej dla technologii magazynu części przewodnika Inicjowanie obsługi administracyjnej urządzeń](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)i sprawdź **iCloud** jako dozwolone usługi:

 [![](introduction-to-icloud-images/icloud-sml.png "Sprawdź jako dozwolone usługi iCloud")](introduction-to-icloud-images/icloud.png#lightbox)

- **Utwórz nowy profil inicjowania obsługi administracyjnej** — Aby utworzyć profil inicjowania obsługi, wykonaj czynności opisane w temacie [Inicjowanie obsługi administracyjnej urządzeń przewodnik](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile) .

- **Dodaj identyfikator kontenera do Entitlements.plist** -format identyfikatora kontenera jest `TeamID.BundleID`. Więcej informacji można znaleźć w [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

- **Skonfiguruj właściwości projektu** — Info.plist pliku upewnij się, **identyfikator pakietu** odpowiada **identyfikator pakietu** ustawiane podczas [tworzenie identyfikator aplikacji ](~/ios/deploy-test/provisioning/capabilities/index.md); Używa podpisywania pakietu systemu iOS **profilu inicjowania obsługi administracyjnej** zawierających Identyfikatora aplikacji z iCloud usługi aplikacji i **uprawnień niestandardowych** plik wybrany. Wszystkie można to zrobić w programie Visual Studio w okienku właściwości projektu.

- **Włącz iCloud na urządzeniu** — przejdź do **Ustawienia > iCloud** i upewnij się, że urządzenie jest zalogowany.
Wybierz i Włącz **dokumentów i danych** opcji.

- **Urządzenie musi używać do testowania iCloud** — nie będzie działać w symulatorze.
W rzeczywistości naprawdę potrzebne dwa lub więcej urządzeń wszystkich zalogowany za pomocą tego samego Identyfikatora Apple, aby wyświetlić iCloud w akcji.


## <a name="key-value-storage"></a>Magazyn kluczy i wartości

Magazyn kluczy i wartości jest przeznaczony dla niewielkich ilości danych, które użytkownik może utrwalonego między urządzeniami - ostatniej strony, które są wyświetlane w książce lub magazynu. Nie można używać magazynu kluczy i wartości do tworzenia kopii zapasowej danych.

Dostępne są pod uwagę podczas korzystania z magazynu kluczy i wartości następujące ograniczenia:

- **Maksymalny rozmiar klucza** -nazwy klucza nie może być dłuższa niż 64 bajtów.

- **Maksymalna wartość rozmiaru** — nie można przechowywać więcej niż 64 kilobajtów w pojedynczej wartości.

- **Rozmiar maksymalny magazyn kluczy i wartości dla aplikacji** -aplikacji tylko może przechowywać maksymalnie 64 kilobajtów danych klucz wartość całkowita. Próbuje ustawić klucze przekracza ten limit nie powiedzie i zostanie utrzymana poprzedniej wartości.

- **Typy danych** — tylko typy podstawowe, takich jak ciągów, liczb i mogą być przechowywane wartości logiczne.

**ICloudKeyValue** przykładzie pokazano, jak to działa. Przykładowy kod tworzy klucz o nazwie dla każdego urządzenia: możesz ustawić ten klucz na jednym urządzeniu i obejrzyj wartość uzyskać propagowane do innych użytkowników. Tworzy również klucza o nazwie "Shared", który może być edytowany na dowolnym urządzeniu — w przypadku edycji na wielu urządzeniach jednocześnie, iCloud podejmie decyzję, wartość "wins" (przy użyciu sygnatury czasowej o zmianę) i pobiera propagowane.

Ten zrzut ekranu przedstawia przykład w użyciu. Po otrzymaniu powiadomienia o zmianach z usługi iCloud są drukowane w przewijania widoku tekstu w dolnej części ekranu i aktualizowane w pól wejściowych.



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "Przepływ komunikatów między urządzeniami")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>Ustawianie i pobieranie danych

Ten kod pokazuje, jak ustawić wartość ciągu.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Wywoływanie Synchronizuj gwarantuje, że wartość zostanie na stałe zapisana tylko Magazyn lokalny dysk. Synchronizacja z usługą iCloud przebiega w tle i nie może być "wymuszone" przez kod aplikacji. Z prawidłowe połączenie z siecią synchronizacji często nastąpi w ciągu 5 sekund, ale jeśli sieć jest niska (lub są rozłączone) aktualizacji może potrwać dłużej.

Można pobrać wartości o tym kodzie:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

Wartość jest pobierana z magazynu danych lokalnych — ta metoda nie próbuje skontaktować się z serwerami usługi iCloud można pobrać wartości "najnowsze". iCloud spowoduje zaktualizowanie magazynu danych lokalnych, zgodnie ze swoim własnym harmonogramem.

### <a name="deleting-data"></a>Usuwanie danych

Aby całkowicie usunąć parę klucz wartość, należy użyć metody Usuń następująco:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>Obserwowania zmiany

Aplikacja również otrzymywać powiadomienia o wartości są zmieniane przez iCloud przez dodanie obserwatora `NSNotificationCenter.DefaultCenter`.
Poniższy kod z **KeyValueViewController.cs** `ViewWillAppear` metody pokazano, jak nasłuchiwania te powiadomienia i Utwórz listę, z których zostały zmienione klucze:

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

Kod można następnie automatycznie podjąć pewne działania z listy zmienionych kluczy, takich jak aktualizowanie lokalną kopię je lub aktualizowania interfejsu użytkownika przy użyciu nowych wartości.

Zmień możliwych przyczyn należą: ServerChange (0), InitialSyncChange (1) lub QuotaViolationChange (2). Można Przyczyna dostępu i wykonywać różne przetwarzania, w razie potrzeby (na przykład może być konieczne usunięcie niektóre klucze w *QuotaViolationChange*).

## <a name="document-storage"></a>Przechowywanie dokumentów

Przechowywanie dokumentów w usłudze iCloud jest przeznaczona do zarządzania dane ważne do aplikacji (i użytkownik). Może służyć do zarządzania plikami i inne dane, które Twoja aplikacja powinna uruchomić znajduje się w tym samym czasie na podstawie iCloud tworzenia kopii zapasowej i udostępnianie funkcjonalność wszystkich urządzeń użytkownika.

Ten diagram przedstawia, jak wszystkie mieścił się ze sobą. Każde urządzenie ma dane zapisane w lokalnej pamięci masowej (UbiquityContainer) i usługi iCloud systemu operacyjnego, który demon zajmuje się wysyłania i odbierania danych w chmurze. Dostęp do wszystkich plików do UbiquityContainer musi odbywać się za pośrednictwem FilePresenter/FileCoordinator, aby zapobiec współbieżny dostęp. `UIDocument` Klasa implementuje te dla Ciebie; w tym przykładzie przedstawiono użycie UIDocument.

 [![](introduction-to-icloud-images/icloud-overview.png "Omówienie magazynu dokumentu")](introduction-to-icloud-images/icloud-overview.png#lightbox)

W przykładzie iCloudUIDoc implementuje prostą `UIDocument` podklasy, który zawiera jedno pole tekstowe. Tekst jest renderowany w `UITextView` i zmiany są propagowane przez iCloud z innymi urządzeniami z zaznaczone na czerwono komunikat z powiadomieniem. Przykładowy kod zajmuje się iCloud bardziej zaawansowane funkcje, takie jak rozwiązywania konfliktów.

Ten zrzut ekranu przedstawia przykładową aplikację — po zmianie tekstu i naciskając klawisz **UpdateChangeCount** dokumentu jest zsynchronizowany przy użyciu usługi iCloud z innymi urządzeniami.

 [![](introduction-to-icloud-images/iclouduidoc.png "Ten zrzut ekranu przedstawia przykładową aplikację po zmiana tekstu, a następnie naciskając klawisz UpdateChangeCount")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

Istnieją pięciu części próbki iCloudUIDoc:

1. **Uzyskiwanie dostępu do UbiquityContainer** -ustalić, czy włączono usługi iCloud oraz ścieżkę do obszaru magazynu usługi iCloud aplikacji.

1. **Tworzenie podklas UIDocument** — Utwórz klasę do pośredniego między magazynu usługi iCloud i obiekty modelu.

1. **Znajdowanie i otwieranie dokumentów w ramach usługi iCloud** -użyj `NSFileManager` i `NSPredicate` można znaleźć iCloud dokumentów i je otworzyć.

1. **Wyświetlanie dokumentów w ramach usługi iCloud** -udostępniają właściwości z Twojej `UIDocument` tak, aby interaktywnie kontrolek interfejsu użytkownika.

1. **Zapisywanie dokumentów w ramach usługi iCloud** — upewnij się, że zmiany wprowadzone w interfejsie użytkownika są zachowywane na dysku i usługi iCloud.

Wszystkie operacje w ramach usługi iCloud Uruchom (lub powinno być ono uruchomione) asynchronicznie, aby nie blokują podczas oczekiwania na coś niepożądane. Zostaną wyświetlone trzy różne sposoby realizacji tego zadania w przykładzie:

 **Wątki** — w `AppDelegate.FinishedLaunching` początkowej wywołanie `GetUrlForUbiquityContainer` odbywa się w innym wątku, aby uniknąć zablokowania głównego wątku.

 **NotificationCenter** — rejestrowania dla powiadomienia, gdy asynchroniczne operacje, takie jak `NSMetadataQuery.StartQuery` ukończone.

 **Programy obsługi zakończenia** — przekazując metod do uruchomienia po zakończeniu operacji asynchronicznych, takich jak `UIDocument.Open`.

### <a name="accessing-the-ubiquitycontainer"></a>Uzyskiwanie dostępu do UbiquityContainer

Pierwszym krokiem przy użyciu usługi iCloud przechowywania dokumentów ma na celu określenie, czy iCloud jest włączony, a jeśli tak lokalizacja "kontenera powszechności" (katalog, w którym włączono iCloud plików są przechowywane na urządzeniu).

Ten kod jest `AppDelegate.FinishedLaunching` metody próbki.

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

Mimo że próbki nie jest to, Firma Apple zaleca wywoływania GetUrlForUbiquityContainer zawsze, gdy aplikacja jest dostarczany do pierwszego planu.

### <a name="creating-a-uidocument-subclass"></a>Tworzenie podklas UIDocument

Wszystkich iCloud plików i katalogów (tj. niczego przechowywane w katalogu UbiquityContainer) musi być zarządzane przy użyciu metod NSFileManager, implementacja protokołu NSFilePresenter i zapisywanie za pośrednictwem NSFileCoordinator.
Najprostszym sposobem wykonania wszystkich nie ma Zapisz je samodzielnie, ale podklasy UIDocument, która obsługuje on wszystko dla Ciebie.

Istnieją tylko dwa metody, które należy zaimplementować w podklasy UIDocument do pracy z usługą iCloud:

- **LoadFromContents** -przekazuje w NSData zawartości pliku można rozpakować do Twojej klasy/es modelu.

- **ContentsForType** — żądanie podania reprezentacja NSData Twojego klasy modelu/es do zapisania na dysku (i w chmurze).

Ten przykładowy kod z **iCloudUIDoc\MonkeyDocument.cs** pokazuje, jak wdrożyć UIDocument.

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

W takim przypadku modelu danych jest bardzo prosty — jedno pole tekstowe. Modelu danych może być złożonym, wymagane, na przykład dokument Xml lub dane binarne. Podstawową rolą, jaką implementacji UIDocument jest do tłumaczenia klasach modeli i reprezentacja NSData, które mogą być zapisywane/załadowane na dysku.

### <a name="finding-and-opening-icloud-documents"></a>Znajdowanie i otwieranie dokumentów usługi iCloud

Przykładowa aplikacja dotyczy tylko jednego pliku - test.txt — tak kodu w **AppDelegate.cs** tworzy `NSPredicate` i `NSMetadataQuery` do wyszukiwania dla tej nazwy pliku. `NSMetadataQuery` Uruchamia asynchronicznie, a następnie wysyła powiadomienie po zakończeniu. `DidFinishGathering` jest wywoływana przez obserwatora powiadomień, zatrzymuje zapytania i wywołuje LoadDocument, który używa `UIDocument.Open` metody obsługi uzupełniania, aby spróbować załadować pliku i wyświetl ją w `MonkeyDocumentViewController`.

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

### <a name="displaying-icloud-documents"></a>Wyświetlanie dokumentów w ramach usługi iCloud

Wyświetlanie UIDocument nie powinien być inne dla każdej klasy modelu
- właściwości są wyświetlane w kontrolek interfejsu użytkownika, prawdopodobnie edytowane przez użytkownika, a następnie zapisywane z powrotem do modelu.

W przykładzie **iCloudUIDoc\MonkeyDocumentViewController.cs** Wyświetla tekst MonkeyDocument w `UITextView`. `ViewDidLoad` nasłuchuje powiadomienia wysyłane w `MonkeyDocument.LoadFromContents` metody. `LoadFromContents` jest wywoływane, gdy iCloud ma nowe dane do tego pliku, dzięki czemu powiadomienia wskazuje, że dokument został zaktualizowany.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

Obsługi powiadomień przykładowy kod wywołuje metodę można zaktualizować interfejsu użytkownika — w takim przypadku bez rozwiązywania konfliktów lub rozdzielczości.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>Zapisywanie dokumentów usługi iCloud

Aby dodać UIDocument można wywołać w ramach usługi icloud `UIDocument.Save` bezpośrednio (tylko w przypadku nowych dokumentów) lub przenoszenia istniejących przy użyciu pliku `NSFileManager.DefaultManager.SetUbiquitious`. Przykład kodu tworzy nowy dokument bezpośrednio w kontenerze powszechności o tym kodzie (istnieją dwa ukończenia programów obsługi, jeden dla `Save` operacji i drugi dla Otwórz):

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

Kolejne zmiany w dokumencie nie są "zapisywane" bezpośrednio, zamiast tego możemy powiedzieć `UIDocument` zmienił się z `UpdateChangeCount`, i automatycznie zaplanowane Zapisz operacji dysku:

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>Zarządzanie dokumentami iCloud

Użytkownicy mogą zarządzać iCloud dokumentów w **dokumenty** katalogu "kontenera powszechności" poza aplikację za pośrednictwem ustawień; mogą przeglądać listę plików i przejdź do usunięcia. Kod aplikacji powinno być możliwe do obsługi sytuacji, w którym zostaną one usunięte przez użytkownika. Nie należy przechowywać danych wewnętrznych aplikacji w **dokumenty** katalogu.

 [![](introduction-to-icloud-images/icloudstorage.png "Zarządzanie przepływem pracy dokumentów usługi iCloud")](introduction-to-icloud-images/icloudstorage.png#lightbox)



Ostrzeżenia różnych zostanie również wyświetlony po podjęciu próby usunięcia aplikacji z obsługą iCloud ze swojego urządzenia, aby poinformować o stan usługi iCloud dokumentów związanych z tej aplikacji.

 [![](introduction-to-icloud-images/icloud-delete1.png "Przykładowe okno dialogowe, gdy użytkownik próbuje usunąć aplikację z obsługą iCloud ze swojego urządzenia")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "Przykładowe okno dialogowe, gdy użytkownik próbuje usunąć aplikację z obsługą iCloud ze swojego urządzenia")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>Kopia zapasowa usługi iCloud

Podczas tworzenie kopii zapasowych iCloud nie jest funkcją, która jest bezpośrednio dostępny przez deweloperów, sposób projektowania aplikacji może mieć wpływ na środowisko pracy użytkownika.
Udostępnia Apple [iOS wytyczne magazynu danych](http://developer.apple.com/icloud/documentation/data-storage/) dla deweloperów, które należy wykonać w aplikacjach systemu iOS.

Najbardziej ważnym zagadnieniem jest, czy aplikacja przechowuje duże pliki, które nie są generowane przez użytkownika (na przykład aplikacji czytnika magazyn, która przechowuje zawartości na problem w megabajtach hundred-plus). Apple preferuje, nie należy przechowywać tego typu danych, których będzie można kopii zapasowej z usługą iCloud a niepotrzebnie wypełnienia przydziału iCloud użytkownika.

Aplikacje, które przechowywania dużych ilości danych, takich jak to należy przechowywać go albo w jednym z katalogów użytkownika, nie jest przechowywane w kopii zapasowej (np. Pamięci podręczne lub tmp) lub użyj `NSFileManager.SetSkipBackupAttribute` do zastosowania flagę do tych plików, dzięki czemu iCloud ignoruje je podczas operacji tworzenia kopii zapasowej.

## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono funkcję iCloud zawarte w systemie iOS 5. Kroki wymagane do skonfigurowania projektu do usługi iCloud, a następnie podano przykłady sposobu implementowania funkcji iCloud ją zbadać.

Przykład magazynu kluczy i wartości przedstawiono, jak iCloud mogą być używane do przechowywania niewielką ilość danych, podobnie jak NSUserPreferences są przechowywane. W przykładzie UIDocument pokazano, jak większej ilości danych złożonych mogą być przechowywane i synchronizowane między wieloma urządzeniami przy użyciu usługi iCloud.

Na koniec należy ono krótkie omówienie na sposób dodawania usługi iCloud kopii zapasowej powinny mieć wpływ projektu aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do usługi iCloud (przykład)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [iCloud seminarium przykładowy kod](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [Seminarium slajdów w ramach usługi iCloud](http://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [Magazynu usługi iCloud](http://support.apple.com/kb/HT4847)
