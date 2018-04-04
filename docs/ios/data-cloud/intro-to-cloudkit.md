---
title: CloudKit
description: iCloud interfejsy API umożliwia aplikacji systemu iOS 8 do przechowywania danych w iCloud o obsługę automatycznego synchronizowanie przez konto użytkownika. Przy użyciu CloudKit umożliwia użytkownikom spójne i łatwego środowisko na urządzeniach z obsługą usługi iCloud. W tym artykule omówiono włączanie CloudKit w aplikacji systemu iOS 8 przy użyciu interfejsu API wygody.
ms.prod: xamarin
ms.assetid: 66B207F2-FAA0-4551-B43B-3DB9F620C397
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/11/2016
ms.openlocfilehash: 33ceff4549e4afbb1e5fecf3bd380fdb9a3df5f2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="cloudkit"></a>CloudKit

_iCloud interfejsy API umożliwia aplikacji systemu iOS 8 do przechowywania danych w iCloud o obsługę automatycznego synchronizowanie przez konto użytkownika. Przy użyciu CloudKit umożliwia użytkownikom spójne i łatwego środowisko na urządzeniach z obsługą usługi iCloud. W tym artykule omówiono włączanie CloudKit w aplikacji systemu iOS 8 przy użyciu interfejsu API wygody._

CloudKit framework upraszcza tworzenie aplikacji tego dostępu do usługi iCloud. Obejmuje to pobieranie danych aplikacji i zasobów prawa, jak również mogą bezpiecznie przechowywać informacji o aplikacji. Ten zestaw zapewnia warstwę anonimowość zezwalając na dostęp do aplikacji przy użyciu ich identyfikatorów iCloud bez udostępniania informacji osobistych.

Deweloperzy mogą skupić się na swoich aplikacji po stronie klienta i pozwól iCloud wyeliminować konieczność pisania logiki aplikacji po stronie serwera. CloudKit zapewnia uwierzytelnianie, prywatne i publiczne baz danych i danych strukturalnych i usługi magazynu trwałego.

## <a name="requirements"></a>Wymagania

Następujące wymagane jest wykonanie czynności przedstawionych w tym artykule:

-  **Xcode i iOS SDK** — Xcode firmy Apple i iOS 8 interfejsów API muszą być zainstalowana i skonfigurowana na komputerze dewelopera.
-  **Visual Studio for Mac** — najnowszej wersji programu Visual Studio for Mac powinna być zainstalowana i skonfigurowana na urządzeniu użytkownika.
-  **System iOS 8 urządzenia** — urządzenia z systemem iOS z najnowszą wersją systemu IOS 8 do testowania.

## <a name="what-is-cloudkit"></a>Co to jest CloudKit?

CloudKit jest sposób, aby udzielić dostępu deweloperów w ramach usługi icloud serwerów. Zapewnia podstawę dla iCloud dysku i usługi iCloud biblioteka fotografii. CloudKit dotyczy zarówno systemu Mac OS X i Apple urządzenia z systemem iOS.

 [![](intro-to-cloudkit-images/image1.png "Jak CloudKit jest obsługiwana na urządzeniach z systemem iOS firmy Apple i systemu Mac OS X")](intro-to-cloudkit-images/image1.png#lightbox)

CloudKit używa infrastruktury konta usługi iCloud. Jeśli jest zalogowany do usługi iCloud konto na urządzeniu użytkownika, CloudKit użyje ich identyfikator do identyfikacji użytkownika. Jeśli żadne konto nie jest dostępny, zostanie należy podać ograniczony dostęp tylko do odczytu.

CloudKit obsługuje pojęcie baz danych prywatnych i publicznych. Publiczny baz danych Podaj "zup" wszystkie dane, których użytkownik ma dostęp. Prywatnych baz danych są przeznaczone do przechowywania danych prywatnych powiązany z określonego użytkownika.

CloudKit obsługuje zarówno strukturalnych i zbiorczo dane. To może obsługiwać bezproblemowo transferu dużych plików. CloudKit odpowiada on za wydajnie transferu dużych plików do i z iCloud serwerów w tle, zwalnianie deweloperom skoncentrować się na inne zadania.

> [!NOTE]
> Należy pamiętać, że CloudKit _technologii transportu_. Nie zapewnia żadnych trwałości; tylko umożliwia aplikacji wysyłanie i odbieranie informacji z serwerów wydajnie.

Opracowywania tego tekstu Apple początkowo dostarcza CloudKit bezpłatnie z ograniczeniem wysokiej zarówno przepustowość i pojemność pamięci masowej. Dla większych projektów lub aplikacje z podstawowej dużej liczby użytkowników Apple ma ze wskazówką dostarczenia ekonomiczny cenową skali.


## <a name="enabling-cloudkit-in-a-xamarin-application"></a>Włączanie CloudKit w aplikacji platformy Xamarin

Przed CloudKit Framework mogą korzystać z aplikacji platformy Xamarin, aplikacja musi poprawnie udostępniane szczegółowych w [Praca z funkcjami](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) i [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) prowadnic

1.  Otwórz projekt w programie Visual Studio dla komputerów Mac lub Visual Studio.
2.  W **Eksploratora rozwiązań**, otwórz **Info.plist** i upewnij się, **identyfikator pakietu** jest zgodna ze strukturą, która została zdefiniowana w **identyfikator aplikacji**utworzone jako część udostępnianie skonfigurować:
 
    [![](intro-to-cloudkit-images/image26a.png "Wprowadź identyfikator pakietu")](intro-to-cloudkit-images/image26a-orig.png#lightbox "Info.plist file displaying Bundle Identifier")

3.  Przewiń w dół do dolnej części **Info.plist** plik i wybierz **włączone trybów tła**, **aktualizacje lokalizacji** i **zdalnego powiadomienia**:

    [![](intro-to-cloudkit-images/image27a.png "Wybierz tryby tła włączone, aktualizacje lokalizacji i zdalnego powiadomień")](intro-to-cloudkit-images/image27a-orig.png#lightbox "Info.plist file displaying background modes")
4.  Kliknij prawym przyciskiem myszy projekt iOS rozwiązania i wybierz **opcje**.
5.  Wybierz **iOS podpisywania pakietu**, wybierz pozycję **tożsamości Developer** i **profilu inicjowania obsługi administracyjnej** utworzone powyżej.
6.  Upewnij się, **Entitlements.plist** obejmuje **włączyć iCloud** , **magazynu kluczy i wartości** i **CloudKit** .
7.  Upewnij się, **kontenera powszechności** istnieje dla aplikacji (jak utworzyć powyżej). Przykład: `iCloud.com.your-company.CloudKitAtlas`
8.  Zapisz zmiany w pliku.


Przy użyciu tych ustawień w miejscu aplikacja jest teraz gotowy dostępu do interfejsów API Framework CloudKit.

## <a name="cloudkit-api-overview"></a>Przegląd interfejsu API CloudKit

Przed wdrożeniem CloudKit w aplikacji platformy Xamarin dla systemu iOS, w tym artykule ma obejmować podstawowe informacje na temat platformy CloudKit, która będzie obejmować następujące tematy:

1.  **Kontenery** — izolowane silosy komunikacji w ramach usługi iCloud.
2.  **Bazy danych** — publiczny i prywatny są dostępne dla aplikacji.
3.  **Rejestruje** — mechanizm, w którym dane strukturalne jest przenoszony do i z CloudKit.
4.  **Rekord strefy** — są grupami rekordów.
5.  **Rejestruje identyfikatorów** — są w pełni znormalizowany i reprezentować określonej lokalizacji rekordu.
6.  **Odwołanie** — Podaj nadrzędny podrzędny relacje między powiązanych rekordów w danej bazie danych.
7.  **Zasoby** — Zezwalaj dla pliku dużych, bez struktury danych mają być przekazane do usługi iCloud i skojarzone z danego rekordu.


### <a name="containers"></a>Kontenery

Zawsze działa danej aplikacji uruchomionych na urządzeniu z systemem iOS wraz po stronie inne aplikacje i usługi na tym urządzeniu. Na urządzeniu klienckim aplikacja ma być ze lub w trybie piaskownicy w określony sposób. W niektórych przypadkach jest literałem piaskownicy, a w innych aplikacji jest po prostu uruchomionych w niej jest własnych pamięć.

Pojęcia pobierania aplikacji klienckiej i uruchomienie jej oddzielona od innych klientów jest bardzo zaawansowane i zapewnia następujące korzyści:

1.  **Zabezpieczenia** — jedna aplikacja nie może kolidować z innych aplikacji klienckich lub sam system operacyjny.
1.  **Stabilność** — jeśli wystąpiła awaria aplikacji klient go nie może być inne aplikacje systemu operacyjnego.
1.  **Ochrona prywatności** — każda aplikacja kliencka ma ograniczony dostęp do informacji osobistych, przechowywane w urządzeniu.


CloudKit zaprojektowano tak, aby podać te same zalety wymienionych powyżej i zastosować je do pracy z informacjami w chmurze:

 [![](intro-to-cloudkit-images/image31.png "Aplikacje CloudKit komunikacji przy użyciu kontenerów")](intro-to-cloudkit-images/image31.png#lightbox)

Podobnie jak trwa aplikacji uruchomionej na urządzeniu, jeden z wielu jest więc aplikacji komunikacja z usługą iCloud jeden z wielu. Każdy z tych silosów różnych komunikacji są określane jako kontenerów.

Kontenery są widoczne w ramach CloudKit za pośrednictwem `CKContainer` klasy. Domyślnie jednej aplikacji rozmów jeden kontener i ten kontener dzieli dane dla tej aplikacji. Oznacza to, czy kilka aplikacji można przechowywania informacji do tego samego konta usługi iCloud, ale te informacje nie będą wymieszaniu.

Przechowywanie w kontenerach iCloud danych umożliwia także CloudKit w celu hermetyzacji informacje o użytkowniku. W ten sposób aplikacja ma niektórych ograniczony dostęp do konta usługi iCloud i informacje użytkownika są przechowywane we wszystkich przy zapewnieniu ochrony prywatności i bezpieczeństwa użytkownika.

Kontenery pełni są zarządzane przez dewelopera aplikacji za pośrednictwem portalu WWDR. Przestrzeń nazw kontenera jest globalne dla wszystkich deweloperów firmy Apple, więc kontenera należy nie tylko być unikatowa dla danego dewelopera aplikacji, ale dla wszystkich deweloperów firmy Apple i aplikacji.

Apple sugeruje przy użyciu odwrotnej notacji DNS, podczas tworzenia przestrzeni nazw dla kontenerów aplikacji. Przykład:

```csharp
iCloud.com.company-name.application-name
```

Kontenery są, domyślnie powiązany jeden do jednego do danej aplikacji, może być udostępniane między aplikacjami. Dzięki wielu aplikacji, może koordynować na jeden kontener. Jedną aplikację może również komunikować się wiele kontenerów.

### <a name="databases"></a>Bazy danych

Jednym z podstawowych funkcji CloudKit ma mieć aplikacja modelu danych i replikacji modelu do serwerów usługi iCloud. Niektóre informacje są przeznaczone dla użytkownika, który go utworzył, inne informacje są publiczne dane, które może zostać utworzony przez użytkownika do użytku publicznego (na przykład przeglądu restauracji) lub może być informacje projektanta został opublikowany dla aplikacji. W obu przypadkach odbiorców nie jest jednego użytkownika, ale jest społeczność użytkowników.

 [![](intro-to-cloudkit-images/image32.png "Diagram kontenera CloudKit")](intro-to-cloudkit-images/image32.png#lightbox)

Wewnątrz kontenera najpierw jest publiczny bazy danych. Jest to gdzie znajduje się wszystkie informacje o publicznym i wspólnie mingles. Ponadto istnieją kilka pojedyncze prywatnej bazy danych dla poszczególnych użytkowników aplikacji.

Podczas uruchamiania na urządzeniu z systemem iOS, aplikację tylko mają dostęp do informacji dla iCloud aktualnie zalogowanego użytkownika. Dlatego widok aplikacji kontenera będzie wyglądać następująco:

 [![](intro-to-cloudkit-images/image33.png "Wyświetl aplikacje kontenera")](intro-to-cloudkit-images/image33.png#lightbox)

Może on zobaczyć tylko publicznego i prywatnego bazy danych skojarzone z iCloud aktualnie zalogowanego użytkownika.

Bazy danych są widoczne w ramach CloudKit za pośrednictwem `CKDatabase` klasy. Każda aplikacja ma dostęp do dwóch baz danych: bazę danych publicznego i prywatnego jednego.

Kontener jest punkt wejścia początkowej do CloudKit. Poniższy kod umożliwia dostęp do bazy danych prywatnych i publicznych z domyślnej aplikacji kontenera:

```csharp
using CloudKit;
...

public CKDatabase PublicDatabase { get; set; }
public CKDatabase PrivateDatabase { get; set; }
...

// Get the default public and private databases for
// the application
PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;
```

Poniżej przedstawiono różnice między typami bazy danych:

||Publiczny bazy danych|Prywatna baza danych|
|---|--- |--- |
|**Typ danych**|Udostępnionych danych|Bieżący użytkownik danych|
|**Quota**|Uwzględnione w przydziału dewelopera|Uwzględnione w przydziału użytkownika|
|**Domyślne uprawnienia**|World do odczytu|Użytkownik, który może zostać odczytany|
|**Edytowanie uprawnień**|iCloud ról pulpitu nawigacyjnego za pomocą poziomu rekordu — klasa|Brak|

### <a name="records"></a>Rekordy

Kontenery przechowywania baz danych i wewnątrz bazy danych znajdują się rekordy. Rekordy są mechanizm, w którym dane strukturalne jest przenoszony do i z CloudKit:

 [![](intro-to-cloudkit-images/image34.png "Kontenery przechowywania baz danych i wewnątrz bazy danych znajdują się rekordy")](intro-to-cloudkit-images/image34.png#lightbox)

Rekordy są widoczne w ramach CloudKit za pośrednictwem `CKRecord` klasy, która opakowuje pary klucz wartość. Wystąpienie obiektu w aplikacji jest odpowiednikiem `CKRecord` w CloudKit. Ponadto każdy `CKRecord` posiada typu rekordu, który jest odpowiednikiem klasy obiektu.

Rekordy mają schematu w czasie, więc opisano dane do CloudKit przed trwa przekazane do przetworzenia. Od tego momentu CloudKit interpretuje informacji i obsługi logistyki przechowywanie i pobieranie rekordu.

`CKRecord` Klasa również obsługuje szeroką gamę metadanych. Na przykład rekord zawiera informacje o podczas jego tworzenia i użytkownika, który go utworzył. Rekord zawiera także informacje o jego ostatniej modyfikacji i użytkownik, który go zmodyfikować.

Rekordy zawierają pojęcia Tag zmiany. Znajduje się poprzednia wersja poprawki danego rekordu. Tag zmian jest używany jako lekki sposób określania, czy klient i serwer ma taką samą wersję programu danego rekordu.

Jak wspomniano powyżej, `CKRecords` zawijać pary klucz wartość, a w efekcie następujące typy danych mogą być przechowywane w rekordzie:

1.   `NSString`
1.   `NSNumber`
1.   `NSData`
1.   `NSDate`
1.   `CLLocation`
1.   `CKReferences`
1.   `CKAssets`


Oprócz folderów dla typów pojedynczą wartość rekord może zawierać jednorodnego tablicę dowolnego typu wymienionych powyżej.

Poniższy kod umożliwia tworzenie nowego rekordu i zapisze go w bazie danych:

```csharp
using CloudKit;
...

private const string ReferenceItemRecordName = "ReferenceItems";
...

var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;
await CloudManager.SaveAsync (newRecord);
```

### <a name="record-zones"></a>Rekord stref

Nie istnieją rekordy samodzielnie w ramach danej bazy danych — grup rekordów ze sobą istnieje wewnątrz strefy rekordu. Rekord strefy można traktować jako tabele w tradycyjnych relacyjnych baz danych:

 [![](intro-to-cloudkit-images/image35.png "Grupy rekordów ze sobą istnieje wewnątrz strefy rekordu")](intro-to-cloudkit-images/image35.png#lightbox)

Może to być wielu rekordów w strefie danego rekordu i wielu strefach rekordu w danej bazie danych. Co baza danych zawiera domyślną strefę rekord:

 [![](intro-to-cloudkit-images/image36.png "Co baza danych zawiera strefy rekordu domyślne i niestandardowe")](intro-to-cloudkit-images/image36.png#lightbox)

Jest to, gdzie są domyślnie przechowywane rekordy. Ponadto można tworzyć niestandardowe rekordu strefy. Reprezentuje rekord strefy odbywa się podstawowej szczegółowości, na które Atomic zatwierdzeń i śledzenia zmian.

## <a name="record-identifiers"></a>Identyfikatory rekordów

Identyfikatory rekordu są prezentowane w postaci spójnej kolekcji, zawierająca zarówno klienta pod warunkiem, Nazwa rekordu i strefy, w której istnieje rekord. Identyfikatory rekordu mają następującą charakterystykę:

-  Są one tworzone przez aplikację klienta.
-  Są w pełni znormalizowany, a reprezentować określonej lokalizacji rekordu.
-  Przypisując unikatowy identyfikator rekordu w obcego bazy danych do nazwy rekordu, mogą one używane do zestawiania lokalnych baz danych, które nie są przechowywane w CloudKit.


Deweloperom tworzenia nowych rekordów, można ich przenieść identyfikator rekordu. Jeśli nie określono identyfikatora rekordu, identyfikator UUID automatycznie zostanie utworzona i przypisane do rekordu.

Podczas deweloperom tworzenie nowych identyfikatorów rekordu, można ich określenie strefy rekordu, której należy każdego rekordu. Jeśli nie zostanie określona, będzie używany Strefa domyślna rekordu.

Identyfikatory rekordów są widoczne w ramach CloudKit za pośrednictwem `CKRecordID` klasy. Poniższy kod może służyć do tworzenia nowego identyfikatora rekordu:

```csharp
var recordID =  new CKRecordID("My Record");
```

### <a name="references"></a>Odwołania

Odwołania zawierają relacje między powiązanych rekordów w danej bazie danych:

 [![](intro-to-cloudkit-images/image37.png "Odwołania zawierają relacje między powiązanych rekordów w danej bazie danych")](intro-to-cloudkit-images/image37.png#lightbox)

W powyższym przykładzie nadrzędnych własnych elementów podrzędnych, tak, aby dziecko podrzędnego rekordu rekordu nadrzędnego. Relacja przesyłany z podrzędnego rekordu do nadrzędnego rekordu i jest nazywany *ponownie odwołanie*.

Odwołania są widoczne w ramach CloudKit za pośrednictwem `CKReference` klasy. Są one sposób przez serwer iCloud zrozumieć relację rekordów.

Odwołania zawierają mechanizm za kaskadowych usuwa. Jeśli rekord nadrzędny zostanie usunięty z bazy danych, wszystkie podrzędne rekordy (jak określono w relacji) automatycznie zostaną usunięte z bazy danych również.

> [!NOTE]
> Wskaźniki delegujące są możliwość przy użyciu CloudKit. Na przykład w czasie aplikacji ma pobranych listę rekordów wskaźników, wybrać rekord i następnie monitu dla rekordu, rekord może istnieje już w bazie danych. Aplikacji musi być kodowane można bezpiecznie obsłużyć tej sytuacji.

Podczas gdy nie jest to wymagane, ponownie odwołania są preferowane, podczas pracy z CloudKit Framework. Apple ma dopracowaniu systemu, aby ustawić to najefektywniej typu odwołania.

Podczas tworzenia odwołania, deweloper może rekordu, który jest już w pamięci lub utworzyć odwołanie do identyfikatora rekordu. Jeśli przy użyciu identyfikatora rekordu i określone odwołanie nie istnieje w bazie danych, czy można utworzyć zawieszonego wskaźnika.

Poniżej przedstawiono przykład tworzenia odwołanie względem znanych rekordów:

```csharp
var reference = new CKReference(newRecord, new CKReferenceAction());
```

### <a name="assets"></a>Zasoby

Zasoby pozwala dużych, bez struktury danych mają być przekazane do usługi iCloud i skojarzony z rekordem danego pliku:

 [![](intro-to-cloudkit-images/image38.png "Zezwalaj na zasoby dla pliku dużych, bez struktury danych mają być przekazane do usługi iCloud i skojarzone z danym rekordu")](intro-to-cloudkit-images/image38.png#lightbox)

Na komputerze klienckim `CKRecord` jest tworzony, który opisuje pliku, który ma zostać przekazana do serwera usługi iCloud. A `CKAsset` zawiera plik i jest połączony z rekordem opisujące go.

Gdy plik jest przekazywany do serwera, rekord znajduje się w bazie danych i plik jest kopiowany do bazy danych magazynu masowego specjalnych. Tworzone jest połączenie między rekord wskaźnik i przekazanego pliku.

Zasoby są dostępne w ramach CloudKit za pośrednictwem `CKAsset` klasy i są używane do przechowywania dużych danych bez struktury. Ponieważ nigdy nie Deweloper chce mieć dużych, bez struktury danych w pamięci, zasoby są implementowane przy użyciu plików na dysku.

Zasoby są własnością rekordów, które umożliwia zasoby, które mają zostać pobrane przy użyciu rekordu jako wskaźnik iCloud. W ten sposób serwer może zasoby zbierania odzyskiwanie po usunięciu rekordu, który jest właścicielem zasobu.

Ponieważ `CKAssets` powinny obsługiwać duże pliki danych, Apple zaprojektowane CloudKit wydajnie przekazywania i pobierania zasoby.

Poniższy kod umożliwia utworzenie elementu zawartości i kojarzenie go z rekordem:

```csharp
var fileUrl = new NSUrl("LargeFile.mov");
var asset = new CKAsset(fileUrl);
newRecord ["name"] = asset;
```

Firma Microsoft ma teraz obejmuje wszystkie obiekty CloudKit na podstawowych. Kontenery skojarzonych aplikacji i zawierają bazy danych. Bazy danych zawiera rekordy, które są podzielone na strefy rekordu i wskazywanego przez identyfikatory rekordu. Relacje nadrzędny podrzędny są definiowane pomiędzy rekordów przy użyciu odwołania. Na koniec dużych plików może przekazać i skojarzony z rekordami korzystanie z zasobów.

## <a name="cloudkit-convenience-api"></a>CloudKit praktyczny interfejs API

Apple oferuje dwa różne zestawy interfejsu API do pracy z CloudKit:

-  **Operacyjne API** — każdego pojedynczego funkcję CloudKit. W przypadku bardziej złożonych aplikacji ten interfejs API zapewnia precyzyjną kontrolę nad CloudKit.
-  **Praktyczny interfejs API** — oferuje podzbiór funkcji CloudKit wspólnego, wstępnie skonfigurowane. W celu uwzględnienia funkcji CloudKit w aplikacji systemu iOS zapewnia rozwiązanie wygodne i łatwy dostęp.


Interfejs API wygody jest zwykle najlepszym rozwiązaniem w przypadku większości aplikacji systemu iOS i Apple sugeruje, począwszy od jej. Pozostałej części tej sekcji omówiono w następujących tematach wygody interfejsu API:

-  Zapisywanie rekordu.
-  Pobieranie rekordu.
-  Aktualizacja rekordu.


### <a name="common-setup-code"></a>Typowy kod instalacji

Przed rozpoczęciem pracy z interfejsem API wygody CloudKit, Brak kodu standardowych instalacji wymagane. Rozpocznij od modyfikowania aplikacji `AppDelegate.cs` pliku i zapewnić ich wyglądać następująco:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using UIKit;
using CloudKit;

namespace CloudKitAtlas
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Computed Properties
        public override UIWindow Window { get; set;}
        public CKDatabase PublicDatabase { get; set; }
        public CKDatabase PrivateDatabase { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            application.RegisterForRemoteNotifications ();

            // Get the default public and private databases for
            // the application
            PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
            PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;

            return true;
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            Console.WriteLine ("Registered for Push notifications with token: {0}", deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications (UIApplication application, NSError error)
        {
            Console.WriteLine ("Push subscription failed");
        }

        public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
        {
            Console.WriteLine ("Push received");
        }
        #endregion
    }
}
```

Powyższy kod przedstawia publiczne i prywatne baz danych CloudKit w postaci skrótów, aby łatwiej współpracować w pozostałej części aplikacji.

Następnie dodaj następujący kod do dowolnego widoku lub Widok kontenera, który będzie używany w CloudKit:

```csharp
using CloudKit;
...

#region Computed Properties
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

Spowoduje to dodanie skrótów na uzyskanie dostępu do `AppDelegate` i uzyskiwać dostęp do skrótów publiczne i prywatne bazy danych utworzone powyżej.

Ten kod w miejscu Spójrzmy na wdrażanie w aplikacji platformy Xamarin iOS 8 CloudKit API wygody.

### <a name="saving-a-record"></a>Zapisywanie rekordu

Przy użyciu wzorca przedstawionych powyżej Omawiając rekordów, poniższy kod utworzyć nowy rekord i zapisać go w publiczny bazy danych za pomocą interfejsu API wygody:

```csharp
private const string ReferenceItemRecordName = "ReferenceItems";
...

// Create a new record
var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;

// Save it to the database
ThisApp.PublicDatabase.SaveRecord(newRecord, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

Trzy czynności należy pamiętać o kodzie powyżej:

1.  Wywołując `SaveRecord` metody `PublicDatabase`, deweloper nie musi określić, jak dane są przesyłane, jakie strefy jest zapisywany do itp. Interfejs API wygody jest zwracając szczególną uwagę wszystkie te szczegóły samej siebie.
1.  Wywołanie jest asynchroniczne i zapewnia procedura wywołania zwrotnego, po zakończeniu wywołania, albo o powodzeniu lub niepowodzeniu. Jeśli połączenie nie powiedzie się, komunikat o błędzie będzie dostępna.
1.  CloudKit nie wymaga magazynu lokalnego/trwałości; jest pośredniczący. Dlatego po wysłaniu żądania do zapisania rekordu, natychmiast wysyłana do serwerów usługi iCloud.


> [!NOTE]
> Z powodu "stratnej" rodzaj sieci komórkowej łączności, gdzie połączeń są stale usuwane lub przerwany, jednym z pierwszego zagadnień, które deweloper musi wykonać podczas pracy z CloudKit jest obsługa błędów.

### <a name="fetching-a-record"></a>Pobieranie rekordu

Z rekordem tworzone i przechowywane pomyślnie na serwerze iCloud można pobrać rekordu należy użyć poniższego kodu:

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

Tak jak w przypadku zapisywania rekordu, powyższy kod jest asynchroniczne, prostota i wymaga obsługi błędów dużą.

### <a name="updating-a-record"></a>Aktualizacja rekordu

Po rekord zostaną pobrane z serwerów usługi iCloud, można następujący kod do modyfikowania rekordu i zapisać zmiany w bazie danych:

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {

    } else {
        // Modify the record
        record["name"] = (NSString)"New Name";

        // Save changes to database
        ThisApp.PublicDatabase.SaveRecord(record, (r, e) => {
            // Was there an error?
            if (e != null) {
                 ...
            }
        });
    }
});
```

`FetchRecord` Metody `PublicDatabase` zwraca `CKRecord` Jeśli wywołanie zakończyło się pomyślnie. Następnie aplikacja modyfikuje rekordu i wywołania `SaveRecord` ponownie, aby ponownie zapisać zmiany w bazie danych.

W tej sekcji wykazało Typowy cykl, że aplikacja będzie używać podczas pracy z interfejsem API wygody CloudKit. Aplikacja zapisywanie rekordów w usłudze iCloud, pobrać te rekordy z usługą iCloud, modyfikowania rekordów i zapisać te zmiany w ramach usługi iCloud.

## <a name="designing-for-scalability"></a>Projektowanie skalowalności

Do tej pory w tym artykule jest czyszczone przechowywania i pobierania modelu obiektu całej aplikacji z serwerów usługi iCloud za każdym razem, gdy ma to odbywać się z. Podczas tego podejścia dobrze działa z małej ilości danych i bardzo małych użytkownika podstawowego, go nie obsługuje również gdy ilość informacji i/lub użytkowników podstawowych zwiększa.

### <a name="big-data-tiny-device"></a>Dane big Data urządzenia niewielki rozmiar

Więcej popularnych aplikacji staje się, więcej danych w bazie danych i mniej możliwe jest ma pamięci podręcznej całego danych na urządzeniu. Następujące techniki może służyć do rozwiązywania tego problemu:

-  **Zachowaj dużej ilości danych w chmurze** — CloudKit zaprojektowano tak, aby efektywnie obsługiwać dużej ilości danych.
-  **Klient powinien tylko wyświetlić wycinek danych** — doprowadzić do minimum systemu od zera danych potrzebnych do obsługi dowolnego zadania w danym momencie.
-  **Widoki klientów, można zmienić** — ponieważ każdy użytkownik ma inną preferencje, wycinek danych jest wyświetlane, można zmienić od użytkownika i użytkownika poszczególnych widoku wszystkie danego wycinek mogą być różne.
-  **Klient używa zapytania, aby skupić się punktu widzenia** — zapytań Zezwalaj użytkownikom na wyświetlanie małego podzbioru większych zestawu danych, który istnieje w chmurze.


### <a name="queries"></a>Kwerendy

Jak już wspomniano, zapytania umożliwiają deweloperom wybierz mały podzbiór większy zestaw danych w chmurze. Zapytania są widoczne w ramach CloudKit za pośrednictwem `CKQuery` klasy.

Zapytanie łączy trzy różne: typu rekordu ( `RecordType`), predykat ( `NSPredicate`) i, opcjonalnie, deskryptora sortowania ( `NSSortDescriptors`). CloudKit obsługuje większość `NSPredicate`.

#### <a name="supported-predicates"></a>Predykaty obsługiwane

CloudKit obsługuje następujące typy `NSPredicates` podczas pracy z zapytania:


1. Dopasowywanie rekordów, której nazwa jest równa wartości jest przechowywana w zmiennej:
    
    ```
    NSPredicate.FromFormat(string.Format("name = '{0}'", recordName))
    ```
   
2. Umożliwia skonfigurowanie dopasowania opierać się na dynamiczne wartości klucza, dlatego, że klucz nie musi wiedzieć, w czasie kompilacji:
    
    ```
    NSPredicate.FromFormat(string.Format("{0} = '{1}'", key, value))
    ```
    
3. Dopasowywanie rekordów, gdzie wartość rekordu jest większa niż podana wartość:
   
    ```
    NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date))
    ```

4. Pasujących rekordów, gdzie rekordu lokalizacji znajduje się w 100 liczników z danej lokalizacji:
    
    ```
    var location = new CLLocation(37.783,-122.404);
    var predicate = NSPredicate.FromFormat(string.Format("distanceToLocation:fromLocation(Location,{0}) < 100", location));
    ```

5. CloudKit obsługuje tokenami wyszukiwania. To wywołanie utworzy dwa tokeny, jeden dla `after` i drugi dla `session`. Zwróci rekordu, który zawiera te dwa tokeny:
    
    ```
    NSPredicate.FromFormat(string.Format("ALL tokenize({0}, 'Cdl') IN allTokens", "after session"))
    ```
    
 6. CloudKit obsługuje złożone predykaty łączone za pomocą `AND` operatora.
    
    ```
    NSPredicate.FromFormat(string.Format("start > {0} AND name = '{1}'", (NSDate)date, recordName))
    ```
    


#### <a name="creating-queries"></a>Tworzenie zapytań

Poniższy kod może służyć do tworzenia `CKQuery` w aplikacji platformy Xamarin iOS 8:

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = '{0}'", recordName));
var query = new CKQuery("CloudRecords", predicate);
```

Najpierw tworzy predykatu, aby wybrać tylko te rekordy, które odpowiada podanej nazwie. Tworzy następnie kwerendę, która będzie wybierać rekordów podanego typu rekordu, które spełniają predykat.

#### <a name="performing-a-query"></a>Wykonywanie zapytania

Po utworzeniu zapytania, należy użyć poniższego kodu do przeprowadzenia kwerendy i przetwarzania zwróconych rekordów:

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = {0}", recordName));
var query = new CKQuery("CloudRecords", predicate);

ThisApp.PublicDatabase.PerformQuery(query, CKRecordZone.DefaultRecordZone().ZoneId, (NSArray results, NSError err) => {
    // Was there an error?
    if (err != null) {
       ...
    } else {
        // Process the returned records
        for(nint i = 0; i < results.Count; ++i) {
            var record = (CKRecord)results[i];
        }
    }
});
```

Powyższy kod pobiera zapytanie utworzone powyżej i go uruchomi w publicznej bazie danych. Ponieważ strefa nie rekord jest określona, wszystkie strefy są przeszukiwane. Jeśli nie wystąpiły żadne błędy, tablicę `CKRecords` zostanie zwrócony pasujących do parametrów zapytania.

Sposób myśleć o kwerendy jest czy są ankiety i są bardzo na fragmentowania za pośrednictwem dużych zestawów danych. Zapytania, jednak nie są dobrze nadają się w przypadku dużych, przede wszystkim statycznych zestawów danych z następujących powodów:

-  Są one uszkodzone na czas pracy baterii urządzenia.
-  Są one nieprawidłowe dla ruchu sieciowego.
-  Są one złym rozwiązaniem dla użytkowników, ponieważ informacje będą widoczne jest ograniczony przez częstotliwość aplikacja przeprowadza sondowanie w bazie danych. Użytkownicy będą dzisiaj powiadomień wypychanych przy każdej zmianie.


### <a name="subscriptions"></a>Subskrypcje

Podczas pracy z zestawami danych dużych, przede wszystkim statycznych, zapytanie nie powinien być wykonywany na urządzeniu klienckim, należy uruchomić na serwerze w imieniu klienta. Zapytanie powinno być ono uruchomione w tle i powinien być wykonywany po każdego pojedynczego rekordu, Zapisz przez bieżącego urządzenia lub innego urządzenia dotknięcie tej samej bazy danych.

Na koniec powiadomienie wypychane mają wysyłane do każdego urządzenia dołączone do bazy danych, po uruchomieniu kwerendy po stronie serwera.

Subskrypcje są widoczne w ramach CloudKit za pośrednictwem `CKSubscription` klasy. Łączą typu rekordu ( `RecordType`), predykat ( `NSPredicate`) i Apple Push Notification ( `Push`).

> [!NOTE]
> CloudKit wypchnięć są nieco rozszerzony, ponieważ zawierają one zawierających informacje dotyczące CloudKit, takie jak przyczyn wypychania nastąpić ładunku.

#### <a name="how-subscriptions-work"></a>Jak działają subskrypcji

Przed wdrożeniem subskrypcji w kodzie języka C#, Przyjrzyjmy się to szybki przegląd działanie subskrypcji:

 [![](intro-to-cloudkit-images/image39.png "Omówienie działania subskrypcji")](intro-to-cloudkit-images/image39.png#lightbox)

Wykres powyżej przedstawiono proces typowe subskrypcji w następujący sposób:

1.  Urządzenie klienckie tworzy nową subskrypcję zawierający zestaw warunków, które wyzwoli subskrypcji i powiadomienia wypychane, który będzie wysyłany, gdy występuje wyzwalacza.
2.  Subskrypcja jest wysyłany do bazy danych, gdzie został on dodany do kolekcji istniejące subskrypcje.
3.  Drugie urządzenie tworzy nowy rekord i zapisuje rekord w bazie danych.
4.  Wyszukuje bazy danych za pośrednictwem jego Lista subskrypcji, czy nowy rekord odpowiada ich warunki.
5.  W przypadku odnalezienia pasującego Push Notification są wysyłane do urządzenia, które są zarejestrowane informacje dotyczące rekordu, który spowodował wyzwalane subskrypcji.


Z tym wiedzę w miejscu Przyjrzyjmy się tworzenia subskrypcji na Xamarin iOS 8 aplikacji.

#### <a name="creating-subscriptions"></a>Tworzenie subskrypcji

Poniższy kod może służyć do tworzenia subskrypcji:

```csharp
// Create a new subscription
DateTime date;
var predicate = NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date));
var subscription = new CKSubscription("RecordType", predicate, CKSubscriptionOptions.FiresOnRecordCreation);

// Describe the type of notification
var notificationInfo = new CKNotificationInfo();
notificationInfo.AlertLocalizationKey = "LOCAL_NOTIFICATION_KEY";
notificationInfo.SoundName = "ping.aiff";
notificationInfo.ShouldBadge = true;

// Attach the notification info to the subscription
subscription.NotificationInfo = notificationInfo;
```

Najpierw tworzy predykat zawiera warunek, aby wyzwolić subskrypcji. Następnie tworzy subskrypcję dla określonego typu rekordu i ustawia opcja podczas testowania wyzwalacza. Ponadto definiuje typ powiadomień, który ma miejsce, gdy subskrypcja jest wyzwalane i dołączona subskrypcja.

### <a name="saving-subscriptions"></a>Zapisywanie subskrypcji

Z subskrypcją utworzone poniższy kod zapisze go do bazy danych:

```csharp
// Save the subscription to the database
ThisApp.PublicDatabase.SaveSubscription(subscription, (s, err) => {
    // Was there an error?
    if (err != null) {

    }
});
```

Przy użyciu interfejsu API wygody, wywołanie asynchroniczne, prostota i zapewnia obsługę błędów łatwe.

#### <a name="handling-push-notifications"></a>Obsługa powiadomień wypychanych

Jeśli projektanta został wcześniej użyty Apple Push powiadomienia (dostępu), proces zajmowanie dla powiadomień generowanych w ramach CloudKit powinny być znane.

W `AppDelegate.cs`, Zastąp `ReceivedRemoteNotification` klas w następujący sposób:

```csharp
public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
{
    // Parse the notification into a CloudKit Notification
    var notification = CKNotification.FromRemoteNotificationDictionary (userInfo);

    // Get the body of the message
    var alertBody = notification.AlertBody;

    // Was this a query?
    if (notification.NotificationType == CKNotificationType.Query) {
        // Yes, convert to a query notification and get the record ID
        var query = notification as CKQueryNotification;
        var recordID = query.RecordId;
    }
}
```

Powyższy kod zapyta CloudKit przeanalizować informacje o użytkowniku w CloudKit powiadomienie. Następnie informacje są wyodrębniane o alercie. Na koniec typ powiadomień, jest testowana i odpowiednio obsługi powiadomienia.

W tej sekcji pokazuje, jak odpowiedzieć danych Big Data, problem z urządzeniem niewielki rozmiar przedstawionych powyżej za pomocą zapytań i subskrypcje. Aplikacja pozostaw jego dużej ilości danych w chmurze i użyj tych technologii, aby zapewnić widoków do tego zestawu danych.

## <a name="cloudkit-user-accounts"></a>CloudKit kont użytkowników

Jak wspomniano w chwili rozpoczęcia tego artykułu, CloudKit jest oparty na istniejącej infrastruktury usługi iCloud. Poniższej sekcji omówiono szczegółowo, jak konta są widoczne dla dewelopera przy użyciu interfejsu API CloudKit.

### <a name="authentication"></a>Uwierzytelnianie

Podczas pracy z kontami użytkowników, pierwszym zagadnieniem jest uwierzytelnianie. CloudKit obsługuje uwierzytelnianie przy użyciu usługi iCloud aktualnie zalogowanego użytkownika na urządzeniu. Uwierzytelnianie odbywa się w tle i jest obsługiwany przez system iOS. W ten sposób deweloperzy nie musisz się martwić o szczegóły dotyczące implementowania uwierzytelniania. One tylko test, aby sprawdzić, czy użytkownik jest zalogowany.

### <a name="user-account-information"></a>Informacje o koncie użytkownika

CloudKit zawiera następujące informacje użytkownika do deweloperów:

-  **Tożsamość** — sposób unikatowej identyfikacji użytkownika.
-  **Metadane** — możliwość zapisywania i pobierania informacji o użytkownikach.
-  **Ochrona prywatności** — wszystkie informacje dotyczące obsługi w manor świadome prywatności. Nic nie jest uwidaczniana, chyba że użytkownik zgodził się do niego.
-  **Odnajdywanie** — zapewnia użytkownikom odnajdywanie swoich znajomych, którzy korzystają z tej samej aplikacji.


Następnie zajmiemy tych tematów szczegółowo.

#### <a name="identity"></a>Tożsamość

Jak już wspomniano, CloudKit umożliwia aplikacji do unikatowej identyfikacji danej użytkownika:

 [![](intro-to-cloudkit-images/image40.png "Jednoznacznie identifing danego użytkownika")](intro-to-cloudkit-images/image40.png#lightbox)

Jest uruchomiony na urządzeniach użytkownika i wszystkich baz danych prywatnego użytkownika wewnątrz kontenera CloudKit aplikacji klienckiej. Aplikacja kliencka będzie się łączyć z tych konkretnych użytkowników. To jest ustalane na podstawie użytkownika, który jest zalogowany do usługi iCloud lokalnie na urządzeniu.

Ponieważ jest to pochodzi z usługą iCloud, brak wzbogaconej kopii informacje magazynie użytkownika. I ponieważ iCloud faktycznie obsługuje kontenera, można skorelować użytkowników. Na rysunku powyżej, użytkownik, którego konto iCloud `user@icloud.com` jest połączony z bieżącego klienta.

Na podstawie kontenera przez kontener unikatowy, losowo wygenerowany identyfikator użytkownika jest utworzona i skojarzona z konta usługi iCloud (adres e-mail). Ta nazwa użytkownika jest zwracana do aplikacji i mogą być używane w dowolny sposób, które uzna za stosowne dewelopera.

> [!NOTE]
> Różnych aplikacji uruchomionych na tym samym urządzeniu dla tego samego użytkownika w ramach usługi iCloud mają różne identyfikatory użytkowników, ponieważ są one połączone z różnych kontenerów CloudKit.

Następujący kod pobiera identyfikator użytkownika CloudKit dla obecnie zalogowanego w ramach usługi iCloud użytkownika na urządzeniu:

```csharp
public CKRecordID UserID { get; set; }
...

// Get the CloudKit User ID
CKContainer.DefaultContainer.FetchUserRecordId ((recordID, err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}", err.LocalizedDescription);
    } else {
        // Save user ID
        UserID = recordID;
    }
});
```

Powyższy kod jest zapytaniem kontener CloudKit, aby dostarczyć identyfikator aktualnie zalogowanego użytkownika. Ponieważ te informacje pochodzące ze iCloud serwera, wywołanie jest asynchroniczne, a obsługa błędów jest wymagana.

#### <a name="metadata"></a>Metadane

Każdy użytkownik w CloudKit ma określonych metadanych, który opisuje je. Te metadane jest reprezentowany jako rekord CloudKit:

 [![](intro-to-cloudkit-images/image41.png "Każdy użytkownik w CloudKit ma określonych metadanych, który opisuje je")](intro-to-cloudkit-images/image41.png#lightbox)

Wyszukiwanie w bazie danych prywatnych dla określonego użytkownika kontenera jest jeden rekord, który definiuje tego użytkownika. Istnieje wiele rekordów użytkowników w publicznych bazy danych, po jednej dla każdego użytkownika kontenera. Jeden z nich będzie mieć identyfikator rekordu odpowiadającego aktualnie zalogowanego identyfikator rekordu użytkownika.

Rekordy użytkowników w publicznych bazy danych są world do odczytu. Są traktowane w większości przypadków, jako rekord zwykłe i mieć typu `CKRecordTypeUserRecord`. Te rekordy są zastrzeżone przez system i nie są dostępne dla zapytań.

Poniższy kod umożliwia dostęp do rekordu użytkownika:

```csharp
public CKRecord UserRecord { get; set; }
...

// Get the user's record
PublicDatabase.FetchRecord(UserID, (record ,er) => {
    //was there an error?
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Save the user record
        UserRecord = record;
    }
});
```

Powyższy kod jest zapytaniem publicznej bazy danych do zwrócenia rekordu użytkownika dla użytkownika, który jest Identyfikatorem możemy uzyskać dostępu do powyżej. Ponieważ te informacje pochodzące ze iCloud serwera, wywołanie jest asynchroniczne, a obsługa błędów jest wymagana.

#### <a name="privacy"></a>Ochrona prywatności

CloudKit był Projekt, domyślnie, aby chronić prywatność aktualnie zalogowanego użytkownika. Nie identyfikujących informacje o użytkowniku jest widoczne domyślnie. Istnieją przypadki, gdzie aplikacja będzie wymagać ograniczone informacje o użytkowniku.

W takich przypadkach aplikacji mogą żądać, że użytkownik ujawnić te informacje. Zostanie wyświetlone okno dialogowe użytkownikowi, prosząc ich aby wyrazić zgodę na udostępnianie swoich informacji o koncie.

#### <a name="discovery"></a>Odnajdywanie

Przy założeniu, że użytkownik jako wyłączony w celu zezwolenie aplikacji ograniczony dostęp do informacji o swoim koncie użytkownika, może być wykrywalny dla innych użytkowników aplikacji:

 [![](intro-to-cloudkit-images/image42.png "Użytkownik może być wykrywalny do innych użytkowników aplikacji")](intro-to-cloudkit-images/image42.png#lightbox)

Aplikacja kliencka rozmawia kontenera, a kontener rozmawia iCloud do dostępu do informacji o użytkowniku. Użytkownik może podać adres e-mail i odnajdywania może służyć do pobrania ponownie informacje użytkownika. Opcjonalnie identyfikator użytkownika mogą służyć do wykrywania informacji o użytkowniku.

CloudKit również umożliwia odnajdywanie informacji o każdy użytkownik, który może być znajomych aktualnie zalogowanego użytkownika iCloud badając całej książki adresowej. Proces CloudKit zostanie pobrana w książce kontaktu użytkownika i Użyj adresów e-mail, aby wyświetlić, użytkownicy mogą znaleźć innego użytkownika aplikacji, która odpowiada tych adresów.

Umożliwia to aplikacji wykorzystać książce kontaktu użytkownika bez zapewnianie dostępu do niego lub użytkownikowi dostęp do kontaktów zatwierdzenia. W żadnym momencie informacje kontaktowe były dostępne dla aplikacji, tylko proces CloudKit ma dostęp.

Aby recap, dostępne są trzy różne rodzaje danych wejściowych do odnajdowania użytkowników:

-  **Identyfikator rekordu użytkownika** — odnajdywania może odbywać się na podstawie Identyfikatora użytkownika z aktualnie zarejestrowanych w CloudKit użytkownika.
-  **Adres E-mail użytkownika** — użytkownik może podać adres e-mail i mogą być używane do odnajdywania.
-  **Skontaktuj się z książki** — służy do odnajdywania użytkowników aplikacji, które mają ten sam adres e-mail wymienionych w swoich kontaktów książce adresowej użytkownika.


Odnajdowanie użytkowników spowoduje zwrócenie następujących informacji:

-  **Identyfikator rekordu użytkownika** — Unikatowy identyfikator użytkownika w bazie danych publicznych.
-  **Pierwszy i nazwisko** — jako przechowywane w bazie danych publicznych.


Te informacje zostaną zwrócone tylko dla użytkowników, którzy mają korzystać do odnajdywania.

Poniższy kod wykryje informacje o użytkowniku aktualnie zalogowany do usługi iCloud na urządzeniu:

```csharp
public CKDiscoveredUserInfo UserInfo { get; set; }
...

// Get the user's metadata
CKContainer.DefaultContainer.DiscoverUserInfo(UserID, (info, e) => {
    // Was there an error?
    if (e != null) {
        Console.WriteLine("Error: {0}", e.LocalizedDescription);
    } else {
        // Save the user info
        UserInfo = info;
    }
});
```

Zbadać wszyscy użytkownicy w książce kontaktu, należy użyć poniższego kodu:

```csharp
// Ask CloudKit for all of the user's friends information
CKContainer.DefaultContainer.DiscoverAllContactUserInfos((info, er) => {
    // Was there an error
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Process all returned records
        for(int i = 0; i < info.Count(); ++i) {
            // Grab a user
            var userInfo = info[i];
        }
    }
});
```

W tej sekcji możemy ma obejmuje cztery główne obszary dostęp do konta użytkownika, które zapewniają CloudKit do aplikacji. Przed pobraniem metadanych i tożsamości użytkownika, aby zasady zachowania poufności informacji wbudowanych w CloudKit, a ponadto możliwość odnajdywania innych użytkowników aplikacji.

## <a name="the-development-and-production-environments"></a>Projektowanie i środowisk produkcyjnych

CloudKit zapewnia oddzielnych środowisk projektowania i produkcji dla typów rekordów i danych aplikacji. Środowisko projektowe jest bardziej elastyczne środowisko, które jest dostępne tylko dla członków zespołu deweloperów. Gdy aplikacja dodaje nowe pole do rekordu i zapisuje rekord w środowisku programistycznym, serwer automatycznie aktualizuje informacje o schemacie.

Deweloper można użyć tej funkcji, wprowadzać zmiany do schematu podczas tworzenia, która pozwala zaoszczędzić czas. Jedno zastrzeżenie: to, że po dodaniu pola do rekordu typu danych skojarzone z tym polem nie można zmienić programowo. Aby zmienić typ pola, deweloper musi usunąć to pole w [pulpitu nawigacyjnego CloudKit](https://icloud.developer.apple.com/dashboard/) i dodaj go ponownie z nowym typem.

Przed wdrożeniem aplikacji, deweloper można migrować ich schemat i dane do środowiska produkcyjnego przy użyciu **pulpitu nawigacyjnego CloudKit**. Podczas uruchamiania w środowisku produkcyjnym, serwera uniemożliwia programistyczna schematu. Deweloper można wprowadzić zmiany z **pulpitu nawigacyjnego CloudKit** , ale próbuje dodać pól do rekordu w środowisku produkcyjnym spowodować błędy.

> [!NOTE]
> IOS Simulator działa tylko w przypadku **środowisko projektowe**. Gdy dewelopera jest gotowy do testowania aplikacji w **środowiska produkcyjnego**, urządzenie fizyczne z systemem iOS jest wymagana.


## <a name="shipping-a-cloudkit-enabled-app"></a>Wysyłanie CloudKit aplikacji z obsługą

Przed dostarczeniem aplikacji, która używa CloudKit, trzeba będzie można skonfigurować do docelowego **środowiska produkcyjnego CloudKit** lub aplikacja zostanie odrzucony przez firmę Apple.

Wykonaj następujące czynności:

1. W programie Visual Studio dla agenta zarządzania, należy skompilować aplikację dla **wersji** > **iOS Device**: 

    [![](intro-to-cloudkit-images/shipping01.png "Kompilowanie aplikacji w wersji")](intro-to-cloudkit-images/shipping01.png#lightbox)

2. Z **kompilacji** menu, wybierz opcję **archiwum**: 

    [![](intro-to-cloudkit-images/shipping02.png "Wybierz archiwum")](intro-to-cloudkit-images/shipping02.png#lightbox)

3. **Archiwum** zostanie utworzona i wyświetlane w programie Visual Studio dla komputerów Mac: 

    [![](intro-to-cloudkit-images/shipping03.png "Zostanie utworzona i wyświetlane archiwum")](intro-to-cloudkit-images/shipping03.png#lightbox)

4. Uruchom **Xcode**.
5. Z **okna** menu, wybierz opcję **organizatora**: 

    [![](intro-to-cloudkit-images/shipping04.png "Wybierz organizatora.")](intro-to-cloudkit-images/shipping04.png#lightbox)

6. Wybierz archiwum aplikacji i kliknij przycisk **Eksportuj...**  przycisk: 

    [![](intro-to-cloudkit-images/shipping05.png "Archiwum aplikacji")](intro-to-cloudkit-images/shipping05.png#lightbox)
    
7. Wybierz metodę eksportu, a następnie kliknij przycisk **dalej** przycisk: 

    [![](intro-to-cloudkit-images/shipping06.png "Wybierz metodę eksportu")](intro-to-cloudkit-images/shipping06.png#lightbox)

8. Wybierz **zespół deweloperów** z listy rozwijanej i kliknij przycisk **wybierz** przycisk: 

    [![](intro-to-cloudkit-images/shipping07.png "Wybierz z listy rozwijanej zespół deweloperów")](intro-to-cloudkit-images/shipping07.png#lightbox)

9. Wybierz **produkcji** z listy rozwijanej i kliknij przycisk **dalej** przycisk: 

    [![](intro-to-cloudkit-images/shipping08.png "Wybierz z listy rozwijanej produkcji")](intro-to-cloudkit-images/shipping08.png#lightbox)

10. Przejrzyj ustawienia i kliknij przycisk **wyeksportować** przycisk: 

    [![](intro-to-cloudkit-images/shipping09.png "Przejrzyj ustawienia")](intro-to-cloudkit-images/shipping09.png#lightbox)

11. Wybierz lokalizację, aby wygenerować wynikowej aplikacji `.ipa` pliku.

Proces jest podobny do przesyłania aplikacji bezpośrednio do iTunes Connect, wystarczy kliknąć **przesyłania...**  przycisk zamiast eksportowania..., po wybraniu archiwum w oknie Organizatora.

## <a name="when-to-use-cloudkit"></a>Kiedy należy używać CloudKit

Jak możemy jak już wspomniano w tym artykule, CloudKit zapewnia prosty sposób dla aplikacji do przechowywania i pobierania informacji z serwerów usługi iCloud. Który jest nazywany CloudKit nie przestarzałe lub zastąpić istniejące narzędzia lub struktury.

### <a name="use-cases"></a>Przypadki użycia

Deweloper zdecydować, kiedy należy używać określonego iCloud framework lub technologii powinny pomóc w następujących przypadków użycia:

-  **Magazyn kluczy i wartości iCloud** — asynchronicznie śledzi aktualne niewielką ilość danych i doskonale nadaje się do pracy z preferencji aplikacji. Jednak jest ograniczony do bardzo mała ilość informacji.
-  **iCloud dysku** — wbudowane iCloud istniejących interfejsów API dokumentów i udostępnia prosty interfejs API na synchronizowanie danych bez struktury w systemie plików. Zawiera pełną w trybie offline pamięci podręcznej w systemie Mac OS X i doskonale nadaje się do perspektywy dokumenty i aplikacje.
-  **iCloud podstawowych danych** — umożliwia danych powinny być replikowane między wszystkimi urządzeniami użytkownika. Dane są jednego użytkownika i doskonałe rozwiązanie dla zachowywaniu prywatnej, strukturalnych danych synchronizacji.
-  **CloudKit** — zapewnia zarówno struktury danych publicznych i zbiorcze i może obsługiwać zarówno dużego zestawu danych, jak i dużych plików bez struktury. Wiązanej dla użytkownika na konto usługi iCloud i udostępnia klienta skierowane do transferu danych.


Zachowuje te przypadki użycia pamiętać, deweloper powinien wybierz technologii iCloud poprawne Podaj oba bieżącą funkcjonalność wymaganą aplikację i zapewnienia dobrej skalowalność rozwój w przyszłości.

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego szybkie wprowadzenie do interfejsu API CloudKit. Wykazał obsługi administracyjnej i konfigurowanie aplikacji dla systemu iOS Xamarin, aby użyć CloudKit. Oceniono funkcji API wygody CloudKit. Ma ona Pokaż sposobu projektowania CloudKit włączono aplikację skalowalności przy użyciu kwerend i subskrypcje. I na koniec wyświetlany uwidocznionego do aplikacji przez CloudKit informacje o koncie użytkownika.

## <a name="related-links"></a>Linki pokrewne

- [CloudKitAtlas (przykład)](https://developer.xamarin.com/samples/monotouch/ios8/CloudKitAtlas/)
- [Wprowadzenie do systemu iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Tworzenie profilu inicjowania obsługi administracyjnej](~/ios/get-started/installation/device-provisioning/index.md)
