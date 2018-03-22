---
title: HomeKit
description: "HomeKit to platforma firmy Apple do kontrolowania urządzeń macierzystego automatyzacji. W tym artykule przedstawiono HomeKit i obejmuje Konfigurowanie Akcesoria testu w symulatorze akcesoriów HomeKit i zapisywanie prostej aplikacji platformy Xamarin.iOS wchodzić w interakcje z te urządzenia."
ms.topic: article
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 02116e8e11cb6ff050e2c885338777e1fd25c4cb
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="homekit"></a>HomeKit

_HomeKit to platforma firmy Apple do kontrolowania urządzeń macierzystego automatyzacji. W tym artykule przedstawiono HomeKit i obejmuje Konfigurowanie Akcesoria testu w symulatorze akcesoriów HomeKit i zapisywanie prostej aplikacji platformy Xamarin.iOS wchodzić w interakcje z te urządzenia._

[![](homekit-images/accessory01.png "Przykład HomeKit aplikacji z obsługą")](homekit-images/accessory01.png#lightbox)

Firma Apple wprowadziła HomeKit w systemie iOS 8 jako sposób bezproblemową integrację wielu urządzeń macierzystego automatyzacji pochodzących od różnych dostawców w jednej, spójne jednostkę. Promowanie wspólny protokół odnajdywania, konfigurowanie i kontrolowanie urządzeń macierzystego automatyzacji HomeKit umożliwia urządzeniom dostawców niepowiązanych współdziałają ze sobą bez poszczególnych dostawców konieczności koordynować działania.

HomeKit umożliwia tworzenie aplikacji platformy Xamarin.iOS kontrolujące dowolnego urządzenia HomeKit włączone bez przy użyciu dostawcy podane w aplikacji lub interfejsów API. Za pomocą HomeKit, można wykonać następujące czynności:

- Wykrywanie nowych urządzeń macierzystego automatyzacji HomeKit włączone i dodaj je do bazy danych, który będzie aktualny we wszystkich urządzeń z systemem iOS przez użytkownika.
- Konfiguracja konfigurowanie, wyświetlanie i kontrolować dowolnego urządzenia w HomeKit _strona główna baza danych konfiguracji_.
- Komunikować się z urządzeniem HomeKit wstępnie skonfigurowane i polecenia do wykonywania poszczególnych działań lub działać w porozumieniu, jak włączenie wszystkie kontrolki w kuchni.

Oprócz obsługująca urządzenia w domu bazy danych konfiguracji do aplikacji HomeKit włączone, HomeKit zapewnia dostęp do poleceń głosowych Siri. Biorąc pod uwagę odpowiednio skonfigurowane ustawienia HomeKit, użytkownik może wystawiać poleceń głosowych takich jak "Siri, Włącz świateł w pokoju."

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>Baza danych konfiguracji macierzystego

HomeKit organizuje wszystkie urządzenia automatyzacji w danej lokalizacji do kolekcji Home. Ta kolekcja umożliwia użytkownika do grupy urządzeń macierzystego automatyzacji do logicznie uszeregowanych jednostki z etykietami łatwy do rozpoznania, zrozumiałą dla użytkownika.

Kolekcji Home są przechowywane w domu konfiguracji bazy danych, która zostanie automatycznie utworzona kopia zapasowa i zsynchronizowane we wszystkich urządzeń z systemem iOS przez użytkownika. HomeKit zawiera następujące klasy do pracy z domu bazy danych konfiguracji:

- `HMHome` — To kontener najwyższego poziomu, który zawiera wszystkie konfiguracje dla wszystkich urządzeń automatyzacji macierzystego i informacji w jednym lokalizację fizyczną (np. pojedynczy rodziny zamieszkania). Użytkownik może mieć więcej niż jednego miejsca zamieszkania, takie jak ich głównej głównego i dom urlopu. Lub mogą mieć różnych "przechowuje" na tej samej właściwości, takie jak dom głównego i domu gościa za pośrednictwem garaż. W obu przypadkach co najmniej jeden `HMHome` obiektu _musi_ należy skonfigurować i zapisać przed można podać inne informacje HomeKit.
- `HMRoom` -Podczas opcjonalne, `HMRoom` zezwala użytkownikowi na definiowanie specjalne pomieszczenia wewnątrz domowe (`HMHome`) takie jak: kuchni, łazienkę, garaż lub pokoju. Użytkownika można grupować wszystkie urządzenia automatyzacji macierzystego w określonej lokalizacji w domu ich do `HMRoom` i działają na nich jako jednostka. Na przykład prosząc Siri, aby wyłączyć świateł garaż.
- `HMAccessory` — Ta pozycja reprezentuje osoba, urządzenie automatyzacji, zainstalowanym w zamieszkania użytkownika (na przykład inteligentne termostat) z obsługą HomeKit fizycznych. Każdy `HMAccessory` jest przypisany do `HMRoom`. Jeśli użytkownik nie skonfigurowane wszystkie pomieszczenia, HomeKit przypisuje Akcesoria sali specjalne domyślne.
- `HMService` -Reprezentuje usługi udostępniane przez dany `HMAccessory`, takie jak stan lub wyłącza światło lub jego koloru (Jeśli zmiana kolorów jest obsługiwana). Każdy `HMAccessory` może mieć więcej niż jedna usługa, takich jak obiekt otwierający drzwi garaż, która zawiera również światło. Ponadto danego `HMAccessory` może być usług, takich jak aktualizacja oprogramowania układowego, które są poza kontrolą użytkownika.
- `HMZone` — Umożliwia użytkownikowi grupy Kolekcja `HMRoom` obiektów do strefy logicznych, takich jak Upstairs, Downstairs lub piwnic. Podczas, gdy jest to opcjonalne, dzięki temu interakcji, takich jak Siri pytaniem, aby włączyć wszystkie światła downstairs off.

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>Inicjowanie obsługi aplikacji HomeKit

Ze względu na wymagania dotyczące zabezpieczeń narzuconego przez HomeKit aplikacji platformy Xamarin.iOS, który używa HomeKit framework muszą zostać prawidłowo skonfigurowane w witrynie Apple Developer Portal i w pliku projektu platformy Xamarin.iOS.

Wykonaj następujące czynności:

1. Zaloguj się do [portalu dla deweloperów firmy Apple](http://developer.apple.com).
2. Polecenie **certyfikaty, identyfikatory & profile**.
3. Jeśli jeszcze tego nie zrobiono, kliknij polecenie **identyfikatory** i Utwórz identyfikator aplikacji (np. `com.company.appname`), Edytuj else identyfikatora istniejącego
4. Upewnij się, że **HomeKit** usługi została sprawdzona dla podanego Identyfikatora: 

    [![](homekit-images/provision01.png "Włącz usługę HomeKit dla podanego Identyfikatora")](homekit-images/provision01.png#lightbox)
5. Zapisz zmiany.
4. Polecenie **profile inicjowania obsługi** > **programowanie** i tworzenie nowego profilu aplikacji inicjowania obsługi administracyjnej: 

    [![](homekit-images/provision02.png "Tworzenie nowego profilu aplikacji inicjowania obsługi administracyjnej")](homekit-images/provision02.png#lightbox)
5. Pobierz i zainstaluj nowy profil inicjowania obsługi administracyjnej albo użyj Xcode, aby pobrać i zainstalować profil.
6. Edytuj opcje projektu platformy Xamarin.iOS i upewnij się, że używasz profilu inicjowania obsługi administracyjnej, który został właśnie utworzony: 

    [![](homekit-images/provision03.png "Wybierz utworzony profil inicjowania obsługi administracyjnej")](homekit-images/provision03.png#lightbox)
7. Następnie edytuj Twojej **Info.plist** i upewnij się, że używasz identyfikator aplikacji, który został użyty do utworzenia profilu inicjowania obsługi administracyjnej: 

    [![](homekit-images/provision04.png "Ustaw identyfikator aplikacji ")](homekit-images/provision04.png#lightbox)
8. Na koniec, Edytuj Twojej **Entitlements.plist** plików i upewnij się, że **HomeKit** wybrano uprawnienia: 

    [![](homekit-images/provision05.png "Włącz uprawnień HomeKit")](homekit-images/provision05.png#lightbox)
9. Zapisz zmiany do wszystkich plików.

Przy użyciu tych ustawień w miejscu aplikacja jest teraz gotowy dostępu do interfejsów API Framework HomeKit. Aby uzyskać szczegółowe informacje o inicjowaniu obsługi, zobacz nasze [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) i [Inicjowanie obsługi aplikacji](~/ios/get-started/installation/device-provisioning/index.md) przewodniki.

> [!IMPORTANT]
> Testowanie aplikacji HomeKit włączone wymaga urządzenie iOS rzeczywistych, które zostały poprawnie zainicjowane do tworzenia aplikacji. Nie można przetestować HomeKit z symulatora systemu iOS.

## <a name="the-homekit-accessory-simulator"></a>Symulator akcesoriów HomeKit

Sposób do przetestowania wszystkich możliwych automatyzacji macierzystego urządzeń i usług, bez konieczności urządzenie fizyczne utworzone Apple _HomeKit akcesoriów symulatora_. Za pomocą tego symulator, możesz skonfigurować i konfigurowanie urządzenia wirtualnego HomeKit.

### <a name="installing-the-simulator"></a>Instalowanie symulator

Apple zapewnia symulatora akcesoriów HomeKit jako osobny plik do pobrania, z Xcode, dlatego należy go zainstalować przed kontynuowaniem.

Wykonaj następujące czynności:

1. W przeglądarce sieci web, odwiedź stronę [pliki do pobrania dla deweloperów firmy Apple](https://developer.apple.com/download/more/?name=for%20Xcode)
2. Pobierz **dodatkowych narzędzi dla Xcode xxx** (xxx w przypadku wersji Xcode, który jest zainstalowany): 

    [![](homekit-images/simulator01.png "Pobierz dodatkowe narzędzia dla środowiska Xcode")](homekit-images/simulator01.png#lightbox)
3. Otworzyć obrazu dysku i zainstalować narzędzia w Twojej **aplikacji** katalogu.

Z Symulatorem akcesoriów HomeKit zainstalowane można utworzyć wirtualnego Akcesoria do testowania.

### <a name="creating-virtual-accessories"></a>Tworzenie wirtualnego Akcesoria

Aby uruchomić symulatora akcesoriów HomeKit i utworzyć kilka wirtualnych Akcesoria, wykonaj następujące czynności:

1. Z folderu aplikacji należy uruchomić symulatora akcesoriów HomeKit: 

    [![](homekit-images/simulator02.png "Symulator akcesoriów HomeKit")](homekit-images/simulator02.png#lightbox)
2. Kliknij przycisk  **+**  i wybrać **nowe akcesoriów...** : 

    [![](homekit-images/simulator03.png "Dodaj nowy akcesoriów")](homekit-images/simulator03.png#lightbox)
3. Podaj informacje o nowych dodatek, a następnie kliknij przycisk **Zakończ** przycisk: 

    [![](homekit-images/simulator04.png "Podaj informacje o nowych akcesoriów")](homekit-images/simulator04.png#lightbox)
4. Kliknij przycisk **dodawania usługi.** przycisk, a z listy rozwijanej wybierz typ usługi: 

    [![](homekit-images/simulator05.png "Z listy rozwijanej wybierz typ usługi")](homekit-images/simulator05.png#lightbox)
5. Podaj **nazwa** usługi i kliknij **Zakończ** przycisk: 

    [![](homekit-images/simulator06.png "Wprowadź nazwę dla usługi")](homekit-images/simulator06.png#lightbox)
6. Można podać opcjonalny właściwości dla usługi, klikając **dodać cech** przycisk i konfigurowania wymaganych ustawień: 

    [![](homekit-images/simulator07.png "Konfigurowanie wymagane ustawienia")](homekit-images/simulator07.png#lightbox)
7. Powtórz powyższe kroki, aby utworzyć urządzenie wirtualne automatyzacji macierzystego obsługuje HomeKit każdego typu.

Niektóre przykładowe wirtualnego HomeKit wyposażenie utworzone i skonfigurowane może wykorzystywać i kontrolować te urządzenia z aplikacji platformy Xamarin.iOS.

## <a name="configuring-the-infoplist-file"></a>Konfigurowanie pliku Info.plist

Nowe dla systemu iOS 10 (i nowsze), deweloper należy dodać `NSHomeKitUsageDescription` klucza w aplikacji `Info.plist` plików i podać ciąg zadeklarowanie, dlaczego aplikacja potrzebuje dostępu do bazy danych HomeKit użytkownika. Ten ciąg zostanie przedstawiony użytkownikowi przy pierwszym uruchomieniu aplikacji:

[![](homekit-images/info01.png "Okno dialogowe uprawnienie HomeKit")](homekit-images/info01.png#lightbox)

Aby ustawić ten klucz, wykonaj następujące czynności:

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
2. W dolnej części ekranu, przełącz się do **źródła** widoku.
3. Dodaj nową **wpis** do listy.
4. Z listy rozwijanej wybierz **prywatność — opis użycia HomeKit**: 

    [![](homekit-images/info02.png "Wybierz prywatność — opis HomeKit użycia")](homekit-images/info02.png#lightbox)
5. Wprowadź opis dlaczego aplikacja potrzebuje dostępu do bazy danych HomeKit użytkownika: 

    [![](homekit-images/info03.png "Wprowadź opis")](homekit-images/info03.png#lightbox)
6. Zapisz zmiany w pliku.

> [!IMPORTANT]
> Nie można ustawić `NSHomeKitUsageDescription` klucza w `Info.plist` pliku spowoduje aplikacji _awarie w trybie dyskretnym_ (zamknięte przez system w czasie wykonywania) bez błąd uruchomienia w systemie iOS 10 (lub nowszego).

## <a name="connecting-to-homekit"></a>Łączenie z HomeKit

Aby komunikować się z HomeKit, musi najpierw tworzy wystąpienie aplikacji platformy Xamarin.iOS `HMHomeManager` klasy. Menedżer Home jest punkt wejścia centralnej do HomeKit i udostępnia listę dostępnych domach, aktualizacji i obsługi tej listy i zwracanie użytkownika _głównej podstawowy_.

`HMHome` Obiektu zawiera wszystkie informacje dotyczące domowe nadaj tym pomieszczenia, grupami lub stref, które może on zawierać, wraz z dowolnego Akcesoria macierzystego automatyzacji zainstalowane. Aby można było wykonać żadnych operacji w HomeKit, co najmniej jeden `HMHome` muszą być tworzone i przypisany jako głównej podstawowej.

Aplikacja jest odpowiedzialny za sprawdzanie, czy istnieje główna podstawowej i tworzenie i przypisywanie jedną, jeśli jej nie ma.

### <a name="adding-a-home-manager"></a>Dodawanie menedżera macierzystego

Aby dodać świadomości HomeKit do aplikacji platformy Xamarin.iOS **AppDelegate.cs** plik, aby go edytować i zapewnić ich wyglądać następująco:

```csharp
using HomeKit;
...

public HMHomeManager HomeManager { get; set; }
...

public override void FinishedLaunching (UIApplication application)
{
    // Attach to the Home Manager
    HomeManager = new HMHomeManager ();
    Console.WriteLine ("{0} Home(s) defined in the Home Manager", HomeManager.Homes.Count());

    // Wire-up Home Manager Events
    HomeManager.DidAddHome += (sender, e) => {
        Console.WriteLine("Manager Added Home: {0}",e.Home);
    };

    HomeManager.DidRemoveHome += (sender, e) => {
        Console.WriteLine("Manager Removed Home: {0}",e.Home);
    };
    HomeManager.DidUpdateHomes += (sender, e) => {
        Console.WriteLine("Manager Updated Homes");
    };
    HomeManager.DidUpdatePrimaryHome += (sender, e) => {
        Console.WriteLine("Manager Updated Primary Home");
    };
}
```

Po pierwszym uruchomieniu aplikacji, użytkownik zostanie poproszony o zezwolenie na dostęp do swoich informacji HomeKit:

[![](homekit-images/home01.png "Użytkownik zostanie poproszony o zezwolenie na dostęp do swoich informacji HomeKit")](homekit-images/home01.png#lightbox)

W przypadku odpowiedzi **OK**, aplikacja będzie można pracować z ich Akcesoria HomeKit w przeciwnym razie zostanie nie i wszelkie wywołania HomeKit zakończy się niepowodzeniem z powodu błędu.

Przy użyciu Narzędzia główne Menedżera w miejscu obok aplikacji należy sprawdzić, czy główna podstawowy został skonfigurowany, a jeśli nie, umożliwiają dla użytkownika utworzyć i przypisać jeden.

### <a name="accessing-the-primary-home"></a>Uzyskiwanie dostępu do głównej podstawowej

Jak już wspomniano, głównej podstawowy musi być utworzony i skonfigurowany przed HomeKit jest dostępny i jest obowiązkiem aplikacji sposób dla użytkownika utworzyć i przypisać głównej podstawowy, jeden już nie istnieje.

Podczas aplikacji najpierw uruchamiania lub zwraca z tła, należy ją monitorować `DidUpdateHomes` zdarzenie `HMHomeManager` klasy do sprawdzenia istnienia głównej podstawowej. Jeśli nie istnieje, powinien udostępniać interfejsu użytkownika go utworzyć.

Następujący kod, mogą być dodawane do kontrolera widoku w celu sprawdzenia, czy są dostępne główną podstawowej:

```csharp
using HomeKit;
...

public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
...

// Wireup events
ThisApp.HomeManager.DidUpdateHomes += (sender, e) => {

    // Was a primary home found?
    if (ThisApp.HomeManager.PrimaryHome == null) {
        // Ask user to add a home
        PerformSegue("AddHomeSegue",this);
    }
};
```

Gdy Menedżer Home nawiązaniu połączenia z HomeKit, `DidUpdateHomes` zdarzenie zostanie wyzwolone, wszelkie istniejące domach zostanie załadowany do kolekcji Menedżera domach i głównej podstawowy zostanie załadowany, jeśli jest dostępna.

### <a name="adding-a-primary-home"></a>Dodawanie głównej podstawowej

Jeśli `PrimaryHome` właściwość `HMHomeManager` jest `null` po `DidUpdateHomes` zdarzenia, należy podać sposób dla użytkownika utworzyć i przypisać głównej podstawowy przed kontynuowaniem.

Zwykle aplikacja wyświetli formularza dla użytkownika nazwę nowego głównej, która jest następnie przekazywane do macierzystego Manager można skonfigurować jako ekran główny podstawowy. Dla **HomeKitIntro** Przykładowa aplikacja modalne widoku została utworzona w Projektancie IOS i wywoływane przez `AddHomeSegue` segue z głównym interfejsu aplikacji.

Zawiera pole użytkownik musi wprowadzić nazwę nowego domu oraz przycisk służący do dodawania stronę główną. Po naciśnięciu **głównej Dodaj** przycisku następujący kod wywołuje macierzystego Menedżera można dodać głównej:

```csharp
// Add new home to HomeKit
ThisApp.HomeManager.AddHome(HomeName.Text,(home,error) =>{
    // Did an error occur
    if (error!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Add Home Error",string.Format("Error adding {0}: {1}",HomeName.Text,error.LocalizedDescription),this);
        return;
    }

    // Make the primary house
    ThisApp.HomeManager.UpdatePrimaryHome(home,(err) => {
        // Error?
        if (err!=null) {
            // Inform user of error
            AlertView.PresentOKAlert("Add Home Error",string.Format("Unable to make this the primary home: {0}",err.LocalizedDescription),this);
            return ;
        }
    });

    // Close the window when the home is created
    DismissViewController(true,null);
});
```

`AddHome` — Metoda podejmie próbę utworzenia nowego domu i przywrócić go do procedury danego wywołania zwrotnego. Jeśli `error` właściwość nie jest `null`, wystąpił błąd i powinny być prezentowane użytkownikowi. Typowe błędy są spowodowane nieunikatowej nazwie domu lub Menedżera macierzystego, nie będą mogli nawiązać połączenia z HomeKit.

Jeśli pomyślnie utworzono stronę główną, należy wywołać `UpdatePrimaryHome` metodę, aby ustawić nowy dom jako głównej podstawowej. Ponownie Jeśli `error` właściwość nie jest `null`, wystąpił błąd i powinny być prezentowane użytkownikowi.

Należy również monitorować Home Menedżera `DidAddHome` i `DidRemoveHome` zdarzenia i aktualizację interfejs użytkownika aplikacji, zgodnie z wymaganiami.

> [!IMPORTANT]
> `AlertView.PresentOKAlert` Metoda stosowana w powyższym przykładowym kodzie jest klasa pomocy w aplikacji HomeKitIntro, która sprawia, że praca z systemu iOS alerty łatwiejsze.


## <a name="finding-new-accessories"></a>Znajdowanie nowych narzędzi

Po głównej podstawowy został zdefiniowany lub ładowane z macierzystego Manager, można wywołać aplikacji platformy Xamarin.iOS `HMAccessoryBrowser` można znaleźć żadnych nowych narzędzi automatyzacji macierzystego i dodaj je do strony głównej.

Wywołanie `StartSearchingForNewAccessories` metodę, aby rozpocząć wyszukiwanie informacji dla nowych narzędzi oraz `StopSearchingForNewAccessories` metody po zakończeniu.

> [!IMPORTANT]
> `StartSearchingForNewAccessories` nie należy pozostawiać uruchomiona przez dłuższy czas, ponieważ wpływa to negatywnie na czas pracy baterii wydajności zarówno i urządzenia z systemem iOS. Apple sugeruje wywołania `StopSearchingForNewAccessories` po minucie lub wyszukiwanie tylko gdy akcesoriów znaleźć interfejsu użytkownika będzie wyświetlana użytkownikowi.

`DidFindNewAccessory` Zdarzeń zostanie wywołana po wykryciu nowych narzędzi i zostaną one dodane do `DiscoveredAccessories` listy w przeglądarce dodatek.

`DiscoveredAccessories` Lista zawiera zbiór `HMAccessory` obiekty, które określają podać HomeKit włączone macierzystego automatyzacji urządzenia i jego dostępnych usług, takich jak świateł lub garaż drzwi formantu.

Po znalezieniu nowego akcesoriów należy przedstawić użytkownika i dlatego można wybrać go i dodaj go do strony głównej. Przykład:

[![](homekit-images/accessory01.png "Znajdowanie nowych akcesoriów")](homekit-images/accessory01.png#lightbox)

Wywołanie `AddAccessory` metody, aby dodać wybrany dodatek do kolekcji głównej. Na przykład:

```csharp
// Add the requested accessory to the home
ThisApp.HomeManager.PrimaryHome.AddAccessory (_controller.AccessoryBrowser.DiscoveredAccessories [indexPath.Row], (err) => {
    // Did an error occur
    if (err !=null) {
        // Inform user of error
        AlertView.PresentOKAlert("Add Accessory Error",err.LocalizedDescription,_controller);
    }
});
```

Jeśli `err` właściwość nie jest `null`, wystąpił błąd i powinny być prezentowane użytkownikowi. W przeciwnym razie użytkownik zostanie poproszony o podanie kodu konfiguracji dla urządzeń dodać:

[![](homekit-images/accessory02.png "Wprowadź kod ustawień urządzenia dodać")](homekit-images/accessory02.png#lightbox)

Symulator akcesoriów HomeKit ten numer znajduje się w obszarze **kod ustawienia** pola:

[![](homekit-images/accessory03.png "Pole kodu ustawień w symulatorze akcesoriów HomeKit")](homekit-images/accessory03.png#lightbox)

Rzeczywiste HomeKit akcesoria są drukowane na etykiety na urządzeniu, w polu produktu lub w podręczniku użytkownika akcesoriów albo kod ustawienia.

Należy monitorować akcesoriów przeglądarki `DidRemoveNewAccessory` zdarzenia i aktualizację użytkownika interfejsu do usunięcia akcesoriów z listy dostępnych, gdy użytkownik dodał ją do ich kolekcji Home.

## <a name="working-with-accessories"></a>Praca z Akcesoria

Raz głównej podstawowej i została ustanowiona i akcesoria zostały dodane do niego, może ona listę Akcesoria (i opcjonalnie pomieszczenia) dla użytkownika do pracy.

`HMRoom` Obiekt zawiera wszystkie informacje dotyczące danego miejsca i dowolnego Akcesoria należące do niej. Opcjonalnie można podzielić pomieszczenia na co najmniej jednej strefie. A `HMZone` zawiera wszystkie informacje dotyczące danego strefy i wszystkich pomieszczeń należących do niej.

Ze względu na przykład firma Microsoft będzie można zachować rzeczy proste i pracować z głównej akcesoria bezpośrednio, zamiast je zorganizować pomieszczenia lub strefy.

`HMHome` Obiektu zawiera listę przypisanej akcesoriów, która może być prezentowana użytkownikowi w jego `Accessories` właściwości. Na przykład:

[![](homekit-images/accessory04.png "Element akcesoriów przykład")](homekit-images/accessory04.png#lightbox)

W tym miejscu formularza, użytkownik może wybrać danego akcesoriów i działa z usługami, które zapewnia.

## <a name="working-with-services"></a>Praca z usługi

Gdy użytkownik wchodzi w interakcję z danym urządzeniem włączone automatyzacji macierzystego HomeKit, jest zazwyczaj za pośrednictwem usług, które zawiera. `Services` Właściwość `HMAccessory` klasy zawiera kolekcję `HMService` oferuje urządzenia obiektów, które definiują usług.

Usługi są takie informacje jak świateł, termostaty, obiekty otwierające drzwi garaż, przełączniki czy blokad. Niektóre urządzenia (takich jak garaż drzwi obiekt otwierający) zapewni więcej niż jedna usługa, takie jak światło i możliwość otwierania lub zamykania drzwi.

Oprócz określonych usług dostępnych w danym akcesoriów, każdy dodatek zawiera `Information Service` definiuje właściwości, takie jak nazwa, producenta, Model i numer seryjny.

### <a name="accessory-service-types"></a>Typy akcesoriów usług

Następujące usługi są dostępne za pośrednictwem `HMServiceType` wyliczenia:

- **AccessoryInformation** — zawiera informacje o urządzeniu danego automatyzacji macierzystego (dodatek).
- **AirQualitySensor** — definiuje czujnik jakości udziału użytkownika.
- **Baterii** — definiuje stan akcesoriów baterii.
- **CarbonDioxideSensor** — definiuje czujnik poziom emisji dwutlenku węgla.
- **CarbonMonoxideSensor** — definiuje czujnik tlenku węgla.
- **ContactSensor** — definiuje kontaktu czujnik (na przykład okno otwarcie lub zamknięcie).
- **Drzwi** — definiuje czujnika stanu drzwi (takich jak otwarty lub zamknięty).
- **Wentylator** — definiuje zdalnego wentylator kontrolowany.
- **GarageDoorOpener** — definiuje obiekt otwierający garaż drzwi.
- **HumiditySensor** — definiuje wilgotności czujnika.
- **LeakSensor** — definiuje czujnika przeciek (jak Podgrzewacz wody aktywny lub pralki).
- **Żarówka** — definiuje autonomiczny lub światła, który jest częścią innego akcesoriów (np. garaż drzwi obiekt otwierający).
- **LightSensor** — definiuje czujnik oświetlenia.
- **LockManagement** — definiuje usługę, która zarządza drzwi zautomatyzowane blokady.
- **LockMechanism** — definiuje zdalne blokowanie kontrolowane (na przykład Blokowanie drzwi).
- **MotionSensor** — definiuje czujnika ruchu.
- **OccupancySensor** — definiuje czujnik zajęte.
- **Gniazda** — definiuje gniazda zdalnego kontrolowane tablicy.
- **SecuritySystem** — definiuje systemu macierzystego zabezpieczeń.
- **StatefulProgrammableSwitch** — definiuje programowalny przełącznik, który pozostaje w stanie podać raz wyzwalane (na przykład przerzucania przełącznika).
- **StatelessProgrammableSwitch** — definiuje programowalny przełącznik, który zwraca stan początkowy po inicjowane (takie jak przycisk polecenia).
- **SmokeSensor** — definiuje dymu czujnika.
- **Przełącz** — definiuje włącznik takie jak przełącznik standardowy tablicy.
- **Czujnik temperatury** — definiuje czujnik temperatury.
- **Termostat** — definiuje inteligentne termostacie sterować HVAC systemu.
- **Okno** — definiuje automatycznych okna trzciny można zdalnie otwarty lub zamknięty.
- **WindowCovering** — Definiuje zdalnie sterowane okna obejmujące, takich jak żaluzje, które może być otwarty lub zamknięty.

### <a name="displaying-service-information"></a>Wyświetlanie informacji o usłudze

Po załadowaniu `HMAccessory` można zbadać poszczególne `HNService` obiekty udostępnia oraz wyświetlanie tych informacji do użytkownika:

[![](homekit-images/accessory05.png "Wyświetlanie informacji o usłudze")](homekit-images/accessory05.png#lightbox)

Należy zawsze należy sprawdzić `Reachable` właściwość `HMAccessory` przed przystąpieniem do pracy z nim. Element akcesoriów może być nieosiągalny użytkownik nie jest w zasięgu urządzenia lub czy został odłączony.

Po wybraniu usługi użytkownika można wyświetlać lub modyfikować jeden lub więcej właściwości tej usługi do monitorowania lub sterowania urządzeniem danego automatyzacji macierzystego.

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>Praca z właściwości

Każdy `HMService` obiekt może zawierać Kolekcja `HMCharacteristic` obiektów, które można podać informacje o stanie usługi (na przykład drzwi otwarcie lub zamknięcie) lub umożliwiają użytkownikom dostosowanie stanu (takie jak ustawianie koloru światła).

`HMCharacteristic` nie tylko informacje na temat właściwości i jego stan, ale również zapewnia metody do pracy z stan za pośrednictwem _metadanych cech_ (`HMCharacteristisMetadata`). Te metadane zapewniają właściwości (na przykład zakres wartości minimalna i maksymalna), które są przydatne podczas wyświetlania informacji do użytkownika, dzięki czemu można zmodyfikować stanów.

`HMCharacteristicType` Wyliczenia zawiera zestaw wartości właściwości metadanych, które można zdefiniować ani zmodyfikować w następujący sposób:

 - AdminOnlyAccess
 - AirParticulateDensity
 - AirParticulateSize
 - AirQuality
 - AudioFeedback
 - BatteryLevel
 - Jasność
 - CarbonDioxideDetected
 - CarbonDioxideLevel
 - CarbonDioxidePeakLevel
 - CarbonMonoxideDetected
 - CarbonMonoxideLevel
 - CarbonMonoxidePeakLevel
 - ChargingState
 - ContactState
 - CoolingThreshold
 - CurrentDoorState
 - CurrentHeatingCooling
 - CurrentHorizontalTilt
 - CurrentLightLevel
 - CurrentLockMechanismState
 - CurrentPosition
 - CurrentRelativeHumidity
 - CurrentSecuritySystemState
 - CurrentTemperature
 - CurrentVerticalTilt
 - Wersja oprogramowania układowego
 - HardwareVersion
 - HeatingCoolingStatus
 - HeatingThreshold
 - HoldPosition
 - HUE
 - Zidentyfikuj
 - InputEvent
 - LeakDetected
 - LockManagementAutoSecureTimeout
 - LockManagementControlPoint
 - LockMechanismLastKnownAction
 - Dzienniki
 - Producent
 - Model
 - MotionDetected
 - Nazwa
 - ObstructionDetected
 - OccupancyDetected
 - OutletInUse
 - OutputState
 - PositionState
 - PowerState
 - RotationDirection
 - RotationSpeed
 - Nasycenie
 - SerialNumber
 - SmokeDetected
 - SoftwareVersion
 - StatusActive
 - StatusFault
 - StatusJammed
 - StatusLowBattery
 - StatusTampered
 - TargetDoorState
 - TargetHeatingCooling
 - TargetHorizontalTilt
 - TargetLockMechanismState
 - TargetPosition
 - TargetRelativeHumidity
 - TargetSecuritySystemState
 - TargetTemperature
 - TargetVerticalTilt
 - TemperatureUnits
 - Wersja

### <a name="working-with-a-characteristics-value"></a>Praca z wartości właściwości

Aby upewnić się, aplikacja zostanie ma najnowszy stan danej właściwości, należy wywołać `ReadValue` metody `HMCharacteristic` klasy. Jeśli `err` właściwość nie jest `null`, wystąpił błąd i może lub nie może przedstawić użytkownikowi.

Cechy `Value` właściwość zawiera bieżący stan charakterystyki jako `NSObject`i jako taki nie może pracować z bezpośrednio w języku C#.

Można odczytać wartości, dodano następujące klasy pomocy **HomeKitIntro** aplikacji przykładowej:

```csharp
using System;
using Foundation;
using System.Globalization;
using CoreGraphics;

namespace HomeKitIntro
{
    /// <summary>
    /// NS object converter is a helper class that helps to convert NSObjects into
    /// C# objects
    /// </summary>
    public static class NSObjectConverter
    {
        #region Static Methods
        /// <summary>
        /// Converts to an object.
        /// </summary>
        /// <returns>The object.</returns>
        /// <param name="nsO">Ns o.</param>
        /// <param name="targetType">Target type.</param>
        public static Object ToObject (NSObject nsO, Type targetType)
        {
            if (nsO is NSString) {
                return nsO.ToString ();
            }

            if (nsO is NSDate) {
                var nsDate = (NSDate)nsO;
                return DateTime.SpecifyKind ((DateTime)nsDate, DateTimeKind.Unspecified);
            }

            if (nsO is NSDecimalNumber) {
                return decimal.Parse (nsO.ToString (), CultureInfo.InvariantCulture);
            }

            if (nsO is NSNumber) {
                var x = (NSNumber)nsO;

                switch (Type.GetTypeCode (targetType)) {
                case TypeCode.Boolean:
                    return x.BoolValue;
                case TypeCode.Char:
                    return Convert.ToChar (x.ByteValue);
                case TypeCode.SByte:
                    return x.SByteValue;
                case TypeCode.Byte:
                    return x.ByteValue;
                case TypeCode.Int16:
                    return x.Int16Value;
                case TypeCode.UInt16:
                    return x.UInt16Value;
                case TypeCode.Int32:
                    return x.Int32Value;
                case TypeCode.UInt32:
                    return x.UInt32Value;
                case TypeCode.Int64:
                    return x.Int64Value;
                case TypeCode.UInt64:
                    return x.UInt64Value;
                case TypeCode.Single:
                    return x.FloatValue;
                case TypeCode.Double:
                    return x.DoubleValue;
                }
            }

            if (nsO is NSValue) {
                var v = (NSValue)nsO;

                if (targetType == typeof(IntPtr)) {
                    return v.PointerValue;
                }

                if (targetType == typeof(CGSize)) {
                    return v.SizeFValue;
                }

                if (targetType == typeof(CGRect)) {
                    return v.RectangleFValue;
                }

                if (targetType == typeof(CGPoint)) {
                    return v.PointFValue;
                }
            }

            return nsO;
        }

        /// <summary>
        /// Convert to string
        /// </summary>
        /// <returns>The string.</returns>
        /// <param name="nsO">Ns o.</param>
        public static string ToString(NSObject nsO) {
            return (string)ToObject (nsO, typeof(string));
        }

        /// <summary>
        /// Convert to date time
        /// </summary>
        /// <returns>The date time.</returns>
        /// <param name="nsO">Ns o.</param>
        public static DateTime ToDateTime(NSObject nsO){
            return (DateTime)ToObject (nsO, typeof(DateTime));
        }

        /// <summary>
        /// Convert to decimal number
        /// </summary>
        /// <returns>The decimal.</returns>
        /// <param name="nsO">Ns o.</param>
        public static decimal ToDecimal(NSObject nsO){
            return (decimal)ToObject (nsO, typeof(decimal));
        }

        /// <summary>
        /// Convert to boolean
        /// </summary>
        /// <returns><c>true</c>, if bool was toed, <c>false</c> otherwise.</returns>
        /// <param name="nsO">Ns o.</param>
        public static bool ToBool(NSObject nsO){
            return (bool)ToObject (nsO, typeof(bool));
        }

        /// <summary>
        /// Convert to character
        /// </summary>
        /// <returns>The char.</returns>
        /// <param name="nsO">Ns o.</param>
        public static char ToChar(NSObject nsO){
            return (char)ToObject (nsO, typeof(char));
        }

        /// <summary>
        /// Convert to integer
        /// </summary>
        /// <returns>The int.</returns>
        /// <param name="nsO">Ns o.</param>
        public static int ToInt(NSObject nsO){
            return (int)ToObject (nsO, typeof(int));
        }

        /// <summary>
        /// Convert to float
        /// </summary>
        /// <returns>The float.</returns>
        /// <param name="nsO">Ns o.</param>
        public static float ToFloat(NSObject nsO){
            return (float)ToObject (nsO, typeof(float));
        }

        /// <summary>
        /// Converts to double
        /// </summary>
        /// <returns>The double.</returns>
        /// <param name="nsO">Ns o.</param>
        public static double ToDouble(NSObject nsO){
            return (double)ToObject (nsO, typeof(double));
        }
        #endregion
    }
}
```

`NSObjectConverter` Jest używana zawsze, gdy aplikacja musi odczytać bieżący stan cechy. Na przykład:

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

Wiersz powyżej konwertuje wartość na `float` które mogą posłużyć w kodzie C# Xamarin.

Aby zmodyfikować `HMCharacteristic`, wywołaj jego `WriteValue` — metoda i zastępowania nową wartość w `NSObject.FromObject` wywołania. Na przykład:

```csharp
Characteristic.WriteValue(NSObject.FromObject(value),(err) =>{
    // Was there an error?
    if (err!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Update Error",err.LocalizedDescription,Controller);
    }
});
```

Jeśli `err` właściwość nie jest `null`, ma wystąpił błąd i powinien będą widoczne dla użytkownika.

### <a name="testing-characteristic-value-changes"></a>Testowanie zmian charakterystyczny wartość

Podczas pracy z `HMCharacteristics` i symulowane Akcesoria, modyfikacje `Value` właściwości mogą być monitorowane w symulatorze akcesoriów HomeKit.

Z **HomeKitIntro** aplikacji uruchomionej w systemie iOS rzeczywistych urządzeń sprzętowych, zmiana wartości właściwości powinny niemal natychmiast widoczne w symulatorze akcesoriów HomeKit. Na przykład zmiana stanu światła w aplikacji systemu iOS:

[![](homekit-images/test01.png "Zmiana stanu światła w aplikacji systemu iOS")](homekit-images/test01.png#lightbox)

Należy zmienić stan światła w symulatorze akcesoriów HomeKit. Jeśli wartość nie ulega zmianie, sprawdź stan komunikat o błędzie podczas zapisywania nowej wartości charakterystyczne i upewnij się, że dodatek jest nadal dostępny.

## <a name="advanced-homekit-features"></a>Funkcje zaawansowane HomeKit

W tym artykule pokrywającego podstawowych funkcji wymaganych do pracy z Akcesoria HomeKit w aplikacji platformy Xamarin.iOS. Istnieją jednak kilka zaawansowane funkcje HomeKit, które nie zostały omówione w tym wprowadzenie:

- **Pokoje** -HomeKit włączone Akcesoria można opcjonalnie podzielone na pomieszczenia przez użytkownika końcowego. Dzięki temu HomeKit Akcesoria obecne w taki sposób, który jest łatwy do zrozumienia i pracować z użytkownika. Aby uzyskać więcej informacji na temat tworzenia i utrzymywania pomieszczenia, zobacz firmy Apple [HMRoom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom) dokumentacji.
- **Strefy** -pomieszczenia Opcjonalnie można podzielić na strefy przez użytkownika końcowego. Strefy odwołuje się do kolekcji pomieszczeniach, które użytkownik może traktować jako pojedyncza jednostka. Na przykład: Upstairs, Downstairs lub piwnic. Ponownie dzięki temu HomeKit przedstawia i pracy z Akcesoria w taki sposób, który ma sens dla użytkownika końcowego. Aby uzyskać więcej informacji na temat tworzenia i utrzymywania stref, zobacz firmy Apple [HMZone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone) dokumentacji.
- **Działania i ustawia akcji** -działania Modyfikuj właściwości usługi akcesoriów i można grupować w zestawy. Ustawia akcji działa jako skrypty w celu kontroli grupy Akcesoria oraz koordynować swoje działania. Na przykład skrypt "Oglądać Telewizję" może zamknąć żaluzje, dim kontrolki i włączyć TV i dźwięku systemu. Aby uzyskać więcej informacji o tworzeniu i obsłudze działania i ustawia akcji, zobacz firmy Apple [HMAction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction) i [HMActionSet](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet) dokumentacji.
- **Wyzwalacze** — wyzwalacz uaktywnić jedną lub więcej ustawić akcji, gdy określony zestaw warunki zostały spełnione. Na przykład włączyć światła portch i Zablokuj wszystkie drzwi zewnętrznych, gdy otrzymuje ciemny poza. Aby uzyskać więcej informacji na temat tworzenia i utrzymywania wyzwalaczy Zobacz firmy Apple [HMTrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger) dokumentacji.

Ponieważ te funkcje używa tych samych metod przedstawionych powyżej, powinna być łatwa do wdrożenia przez następujących firmy Apple [przewodnik HomeKitDeveloper](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html), [HomeKit użytkowników Interface Guidelines](https://developer.apple.com/homekit/ui-guidelines/) i [ Odwołanie do platformy HomeKit](https://developer.apple.com/library/ios/home_kit_framework_ref).

## <a name="homekit-app-review-guidelines"></a>Wytyczne HomeKit aplikacji

Przed przesłaniem HomeKit włączonych aplikacji platformy Xamarin.iOS iTunes Connect zwolnienia w sklepie iTunes aplikację, upewnij się, należy wykonać wytycznymi firmy Apple dla aplikacji HomeKit włączone:

 - Głównym celem aplikacji _musi_ można macierzystego automatyzacji, jeśli przy użyciu platformy HomeKit.
 - Tekst marketing aplikacji należy powiadomić użytkowników, że HomeKit jest używany, a użytkownik musi podać zasady zachowania poufności informacji.
 - Zbieranie informacji o użytkowniku lub przy użyciu HomeKit do celów reklamowych jest zabronione.

Dla pełnego przeglądu wytycznych, zobacz firmy Apple [wytyczne przeglądu sklepu aplikacji](https://developer.apple.com/app-store/review/guidelines/).

## <a name="whats-new-in-ios-9"></a>Nowości w systemie iOS 9

Apple wprowadziła następujące zmiany i uzupełnienia HomeKit dla systemu iOS 9:

- **Obsługa istniejących obiektów** — w przypadku modyfikacji istniejącego akcesoriów, Menedżer Home (`HMHomeManager`) będzie informować o konkretny element, który został zmodyfikowany.
- **Identyfikatory trwałe** -zawierają teraz wszystkich odpowiednich klas HomeKit `UniqueIdentifier` właściwość do unikatowego identyfikowania danego elementu w HomeKit włączona aplikacji (lub wystąpienia tej samej aplikacji).
- **Zarządzanie użytkownikami** — dodano kontrolera wbudowanych widoku w celu zapewnienia zarządzania użytkownika przez użytkowników, którzy mają dostęp do urządzeń HomeKit w domu użytkownika głównego.
- **Możliwości użytkownika** — HomeKit użytkownicy mają teraz zestaw uprawnień, które kontrolują, jakie funkcje są możliwe do użycia w HomeKit i HomeKit włączone Akcesoria. Aplikację tylko powinien być wyświetlany odpowiednie funkcje do bieżącego użytkownika. Na przykład tylko administratorzy powinni mieć możliwość obsługi innych użytkowników.
- **Wstępnie zdefiniowane sceny** -wstępnie zdefiniowanych sceny zostały utworzone dla czterech typowych zdarzeń, które miało miejsce w przypadku średniej użytkownika HomeKit: Uzyskaj, pozostaw, zwróć, przejdź mają być. Nie można usunąć te wstępnie zdefiniowane sceny w domu.
- **Sceny i używanie programu Siri** -Siri ma lepszą obsługę dla sceny systemu iOS 9 i może rozpoznaje nazwy wszelkich sceny zdefiniowane w HomeKit. Użytkownik może wykonywać sceny po prostu mówiąc jego nazwę, aby Siri.
- **Kategorie akcesoriów** — zestaw wstępnie zdefiniowanych kategorii został dodany do wszystkich Akcesoria i pomaga w celu zidentyfikowania typu akcesoriów dodawany do strony głównej lub pracowano z aplikacji. Te nowe kategorie są dostępne podczas instalacji dodatek.
- **Obsługa Apple Watch** — HomeKit są teraz dostępne do watchOS i Apple Watch będą mogli kontroli urządzeń obsługujących HomeKit bez iPhone jest niemal czujki. HomeKit dla watchOS obsługuje następujące możliwości: wyświetlanie domach, kontrolowanie akcesoriów i wykonywania sceny.
- **Nowy typ wyzwalacza zdarzenia** — oprócz wyzwalaczy typu czasomierz, obsługiwane w systemie iOS 8, iOS 9 teraz obsługuje wyzwalacze zdarzeń na podstawie stanu akcesoriów (np. danych czujnika) lub używanie funkcji geolokalizacji. Wyzwalacze zdarzeń używać `NSPredicates` można ustawić warunki ich wykonanie.
- **Dostęp zdalny** — z dostępu zdalnego, użytkownik jest teraz kontrolować ich HomeKit włączone Home automatyzacji Akcesoria, gdy są one od domu w lokalizacji zdalnej. W systemie iOS 8 ta była obsługiwana tylko jeśli użytkownik ma a 3rd generation Apple TV w domu. W systemie iOS 9 to ograniczenie jest unosiło i dostępu zdalnego jest obsługiwana za pomocą usługi iCloud i protokół akcesoriów HomeKit (HAP).
- **Nowe możliwości energii małej Bluetooth (cz)** -HomeKit obsługuje teraz więcej typów akcesoriów, które mogą komunikować się za pośrednictwem protokołu energii małej Bluetooth (cz). Przy użyciu HAP Secure Tunneling, akcesoriów HomeKit mogą uwidaczniać innego akcesoriów Bluetooth za pośrednictwem sieci Wi-Fi (jeśli jest ona poza zakresem Bluetooth). W systemie iOS 9 Akcesoria cz mają pełną obsługę powiadomień i metadanych.
- **Nowe kategorie akcesoriów** -Apple dodano następujące nowe kategorie akcesoriów w systemie iOS 9: pokrycia okna, napędzane drzwi i systemu Windows, systemów alarmowych, czujników i programowalny przełączników.

Aby uzyskać więcej informacji o nowych funkcjach HomeKit w systemie iOS 9, zobacz firmy Apple [indeksu HomeKit](https://developer.apple.com/homekit/) i [What's New in HomeKit](https://developer.apple.com/videos/wwdc/2015/?id=210) wideo.

## <a name="summary"></a>Podsumowanie

W tym artykule wprowadziła framework macierzystego automatyzacji HomeKit firmy Apple. Go pokazano sposób instalacji i konfiguracji urządzeń testowych za pomocą symulatora akcesoriów HomeKit oraz sposób tworzenia prostej aplikacji platformy Xamarin.iOS do odnalezienia, komunikować się z oraz automatyzacją domu urządzenia przy użyciu HomeKit.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [Nowości w systemie iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Przewodnik HomeKitDeveloper](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [Wskazówki dotyczące interfejsu użytkownika HomeKit](https://developer.apple.com/homekit/ui-guidelines/)
- [Odwołanie HomeKit Framework](https://developer.apple.com/library/ios/home_kit_framework_ref)
