---
title: HealthKit w Xamarin.iOS
description: W tym dokumencie opisano HealthKit, ramy wprowadzone w systemie iOS 8, która zapewnia scentralizowane, skoordynowanej i bezpieczny magazyn danych informacji dotyczących kondycji. Omówiono jej udostępnianie aplikacji HealthKit i jak napisać kod, który używa HealthKit framework.
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 06c0231bbb9aa7b82b92e0a8c2157b8be9c8b05b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787536"
---
# <a name="healthkit-in-xamarinios"></a>HealthKit w Xamarin.iOS

Zestaw kondycji zapewnia bezpieczny magazyn danych informacji dotyczących kondycji użytkownika. Kondycji zestawu aplikacji może z jawnym uprawnieniem użytkownika do odczytu i zapisu do tego magazynu danych i otrzymywać powiadomienia, po dodaniu potrzebnych danych. Aplikacje można przedstawić dane lub użytkownika można wyświetlić pulpit nawigacyjny ich danych firmy Apple podana kondycji aplikacji.

Ponieważ dane dotyczące kondycji jest więc poufne oraz decydujące zestawu kondycji jest silnie typizowane, jednostek miary i jawne skojarzenia z typem rejestrowanych informacji (na przykład poziom glukozy krwi lub puls). Ponadto aplikacje zestawu kondycji muszą używać jawnych uprawnień, żądanie dostępu do określonego typy informacji, a użytkownik musi jawnie udzielić uprawnień dostępu do aplikacji dla tych typów danych.

W tym artykule przedstawiono:

- Wymagania dotyczące zabezpieczeń zestawu kondycji, obejmuje Inicjowanie obsługi aplikacji i żąda uprawnień dostępu do zestawu kondycji bazy danych;
- System typów kondycji zestawu, co minimalizuje możliwość stosowania nieprawidłowo lub misinterpreting danych.
- Zapisywania w magazynie danych kondycji zestaw udostępnionego, całego systemu.

W tym artykule nie opisano bardziej zaawansowanych tematów, takich jak kwerendy bazy danych, konwersja między jednostki miary lub odbieranie powiadomień nowych danych.

W tym artykule firma Microsoft będzie można tworzenia przykładowej aplikacji do rejestrowania Puls użytkownika:

[![](healthkit-images/image01.png "Przykładowa aplikacja do rejestrowania Puls użytkowników")](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>Wymagania

Poniższe elementy są wymagane do wykonania czynności przedstawionych w tym artykule:

- **Xcode 7 i iOS 8 (lub nowszego)** — najnowsze Xcode i interfejsów API systemu iOS firmy Apple muszą być zainstalowana i skonfigurowana na komputerze dewelopera.
- **Programu Visual Studio for Mac lub Visual Studio** — najnowszej wersji programu Visual Studio for Mac powinna być zainstalowana i skonfigurowana na komputerze dewelopera.
- **System iOS 8 (lub nowszego) urządzenia** — urządzenia z systemem iOS z najnowszą wersją systemu IOS 8 lub nowszego do testowania.

> [!IMPORTANT]
> Zestaw kondycji została wprowadzona w systemie iOS 8. Obecnie kondycji zestawu nie jest dostępna w symulatorze systemu iOS i debugowanie wymaga połączenia z urządzeniem fizycznych z systemem iOS.




## <a name="creating-and-provisioning-a-health-kit-app"></a>Tworzenie i obsługa aplikacji zestawu kondycji
Zanim aplikację platformy Xamarin iOS 8, można użyć interfejsu API HealthKit, musi być prawidłowo skonfigurowane i udostępnione. W tej sekcji opisano kroki wymagane do prawidłowego konfigurowania aplikacji platformy Xamarin.

Wymagaj kondycji zestawu aplikacji:

- Jawne **identyfikator aplikacji**.
- A **profilu inicjowania obsługi administracyjnej** skojarzony z tym jawne **identyfikator aplikacji** i **Kit kondycji** uprawnienia.
- `Entitlements.plist` z `com.apple.developer.healthkit` właściwości typu `Boolean` ustawioną `Yes`.
- `Info.plist` Których `UIRequiredDeviceCapabilities` klucz zawiera wpis o `String` wartość `healthkit`.
- `Info.plist` Musi mieć również wpisy odpowiednie wyjaśnienie prywatności: `String` wyjaśnienie klucza `NSHealthUpdateUsageDescription` Jeżeli aplikacji będzie można zapisać danych i `String` wyjaśnienie klucza `NSHealthShareUsageDescription` Jeżeli aplikacji będzie można odczytać zestawu kondycji dane.

Aby dowiedzieć się więcej na temat udostępniania aplikacji dla systemu iOS [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md) artykułu w jego Xamarin **wprowadzenie** serii opisuje relację między Developer certyfikaty identyfikatory aplikacji Inicjowanie obsługi administracyjnej, profile i uprawnień dla aplikacji.

<a name="explicit-appid" />

### <a name="explicit-app-id-and-provisioning-profile"></a>Identyfikator jawnej aplikacji i profilu inicjowania obsługi administracyjnej

Jawne tworzenie **identyfikator aplikacji** i odpowiednią **profilu inicjowania obsługi administracyjnej** odbywa się w obrębie firmy Apple [Centrum deweloperów systemu iOS](https://developer.apple.com/devcenter/ios/index.action). 

Bieżące **identyfikatorów aplikacji** są wymienione w [certyfikaty, identyfikatory & profile](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) sekcji Centrum deweloperów. Często, lista zawiera **identyfikator** wartości `*`, wskazujące który **identyfikator aplikacji** - **nazwa** może być używany z dowolnej liczby sufiksy. Takie *identyfikatorów aplikacji symboli wieloznacznych* nie można używać z zestawu kondycji.
 
Aby utworzyć jawne **identyfikator aplikacji**, kliknij przycisk **+** przycisk w prawym górnym umożliwiające przejście do **zarejestrować identyfikator aplikacji systemu iOS** strony:


[![](healthkit-images/image02.png "Rejestrowanie aplikacji w portalu dla deweloperów firmy Apple")](healthkit-images/image02.png#lightbox)

Jak przedstawiono na ilustracji powyżej, po utworzeniu opis aplikacji, użyj **jawny identyfikator aplikacji** sekcji, aby utworzyć identyfikator aplikacji. W **usługi aplikacji** sekcji wyboru **Kit kondycji** w **Włącz usługi** sekcji.

Gdy wszystko będzie gotowe, naciśnij klawisz **Kontynuuj** przycisk, aby zarejestrować **identyfikator aplikacji** na Twoim koncie. Zostanie wyświetlona Wstecz **certyfikaty identyfikatory i profile** strony. Kliknij przycisk **profile inicjowania obsługi** przejście do listy do bieżącego profilu inicjowania obsługi administracyjnej, a następnie kliknij przycisk **+** przycisk w prawym górnym rogu, aby przejść do **dodać systemu iOS Profil inicjowania obsługi administracyjnej** strony. Wybierz **tworzenie aplikacji dla systemu iOS** opcję i kliknij przycisk **Kontynuuj** na uzyskanie dostępu do **wybierz identyfikator aplikacji** strony. W tym miejscu, wybierz jawnych **identyfikator aplikacji** określonej wcześniej:


[![](healthkit-images/image03.png "Wybierz jawny identyfikator aplikacji")](healthkit-images/image03.png#lightbox)

Kliknij przycisk **Kontynuuj** służbowych przez pozostałe ekrany, w której będzie można określić, jak i z **certyfikaty Developer**, **urządzenia**i **nazwa** tego **profil inicjowania obsługi administracyjnej**:

[![](healthkit-images/image04.png "Generowanie profilu inicjowania obsługi administracyjnej")](healthkit-images/image04.png#lightbox)

Kliknij przycisk **Generuj** i await tworzenia profilu. Pobierz plik, a następnie kliknij dwukrotnie, aby zainstalować w środowisku Xcode. Potwierdzenie instalacji w obszarze **Xcode > Preferencje > kont > Wyświetl szczegóły...** Powinny pojawić się zainstalowanego profilu inicjowania obsługi administracyjnej, a powinien mieć ikony dla zestawu kondycji i innych usług specjalne w jego **uprawnień** wiersza:

[![](healthkit-images/image05.png "Wyświetlanie profilu w środowisku Xcode")](healthkit-images/image05.png#lightbox)

<a name="associating-appid" />

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>Kojarzenie identyfikator aplikacji i Inicjowanie obsługi profilu z aplikacji platformy Xamarin.iOS

Po utworzeniu i zainstalowany odpowiedni **profilu inicjowania obsługi administracyjnej** zgodnie z opisem, wymaga czasu tworzenie rozwiązań programu Visual Studio dla komputerów Mac lub Visual Studio. Dostęp zestawu kondycji jest dostępny dla dowolnego systemu iOS C# lub projektów F #.

Zamiast poprowadzą przez proces tworzenia projektu Xamarin iOS 8 ręcznie, otwórz przykładową aplikację dołączony do tego artykułu (która obejmuje wbudowane scenorysu i kodu). Skojarzenie przykładową aplikację z Twojego zestawu kondycji włączone **profilu inicjowania obsługi**w **konsoli rozwiązania**, kliknij prawym przyciskiem myszy projekt i wyświetlić jego **opcje** okna dialogowego. Przełącz się do **aplikacji systemu iOS** panelu, a następnie wprowadź jawnych **identyfikator aplikacji** utworzone wcześniej jako aplikację **identyfikator pakietu**:

[![](healthkit-images/image06.png "Wprowadź jawny identyfikator aplikacji")](healthkit-images/image06.png#lightbox)

Teraz przełączyć się do **iOS podpisywania pakietu** panelu. Z ostatnio zainstalowanych **profilu inicjowania obsługi administracyjnej**, z jego skojarzenie z jawnych **identyfikator aplikacji**, będą teraz dostępne jako **profilu inicjowania obsługi administracyjnej**:

[![](healthkit-images/image07.png "Wybierz profil inicjowania obsługi administracyjnej")](healthkit-images/image07.png#lightbox)

Jeśli **profilu inicjowania obsługi administracyjnej** jest niedostępny, dokładnie **identyfikator pakietu** w **aplikacji systemu iOS** panelu określonej w porównaniu z **systemu iOS Centrum deweloperów** i **profilu inicjowania obsługi administracyjnej** zainstalowano (**Xcode > Preferencje > kont > Wyświetl szczegóły** ).

Gdy obsługą zestawu kondycji **profilu inicjowania obsługi administracyjnej** jest zaznaczone, kliknij przycisk **OK** aby zamknąć okno dialogowe Opcje projektu.

### <a name="entitlementsplist-and-infoplist-values"></a>Entitlements.plist i Info.plist wartości

Przykładowa aplikacja zawiera `Entitlements.plist` pliku (która jest niezbędne do zestawu kondycji włączone aplikacji) i nie jest uwzględniony w każdym szablonie projektu. Jeśli projekt nie ma uprawnień, kliknij prawym przyciskiem myszy projekt, wybierz **Plik > Nowy plik... > iOS > Entitlements.plist** Aby ręcznie dodać.

Ostatecznie Twoje `Entitlements.plist` musi mieć następującą parą klucza i wartości:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.HealthKit</key>
    <true/>
</dict>
</plist>

```

Podobnie `Info.plist` dla aplikacji musi mieć wartość `healthkit` skojarzone z `UIRequiredDeviceCapabilities` klucza:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

Przykładowa aplikacja podaną w tym artykule obejmuje wstępnie skonfigurowane `Entitlements.plist` zawierającą wszystkie wymagane klucze.

<a name="programming" />

## <a name="programming-health-kit"></a>Zestaw kondycji programowania

Zestaw kondycji magazynu danych jest prywatny, specyficzne dla użytkownika magazyn danych użytkowany przez aplikacje. Ponieważ informacje o kondycji, są tak ważne, kroków umożliwiających dostęp do danych musi wykonać użytkownik. Dostęp może być częściowe (zapisu, ale nie odczytu, dostęp do niektórych typów danych, ale nie w innych osób, itp.) i może można odwołać w dowolnym momencie. Kondycji zestawu aplikacji powinna być zapisana pamiętać o, pamiętając, że w przypadku wielu użytkowników będzie wątpliwości dotyczące ich informacje dotyczące kondycji są przechowywane.

Kondycja zestawu danych jest ograniczona do firmy Apple określone typy. Te typy są ściśle zdefiniowana: inne, takie jak typ krwi są ograniczone do określonych wartości wyliczenia Apple dostarczony, podczas gdy inne połączyć z jednostką miary (na przykład gramów kalorii i litrach) wielkości. Nawet danych, które mają niezgodne jednostki miary można rozróżnić za ich `HKObjectType`; na przykład system typów będzie przechwytywać omyłkowo wystąpiła próba przechowywania `HKQuantityTypeIdentifier.NumberOfTimesFallen` wartość do pola Oczekiwano `HKQuantityTypeIdentifier.FlightsClimbed` mimo że oba rozwiązania używają` HKUnit.Count` Jednostka miary.

Typy możliwą w zestawu kondycji magazynu danych są wszystkie podklasy `HKObjectType`. `HKCharacteristicType` obiekty przechowywać biologicznych płeć, typ krwi i datę urodzenia. Jednak są typowe więcej `HKSampleType` obiektów, które zawierają dane, które jest próbkowany o określonej godzinie lub w danym okresie czasu. 

[![](healthkit-images/image08.png "HKSampleType obiekty wykresu")](healthkit-images/image08.png#lightbox)

`HKSampleType` jest abstrakcyjny i ma cztery konkretnych podklas. Obecnie jest tylko jeden typ `HKCategoryType` danych, który jest analiza uśpienia. Większość dużych danych w zestawie kondycji jest typu `HKQuantityType` i przechowywać swoje dane w `HKQuantitySample` obiektów, które są tworzone przy użyciu znanych wzorca projektowego fabryki:

[![](healthkit-images/image09.png "Znaczną część danych w zestawie kondycji typu HKQuantityType i dane przechowywane w obiektach HKQuantitySample")](healthkit-images/image09.png#lightbox)

`HKQuantityType` typy należeć do zakresu od `HKQuantityTypeIdentifier.ActiveEnergyBurned` do `HKQuantityTypeIdentifier.StepCount`. 

<a name="requesting-permission" />

### <a name="requesting-permission-from-the-user"></a>Żądanie uprawnienia od użytkownika

Użytkownicy końcowi muszą umożliwia aplikacji do odczytu lub zapisu danych kondycji zestaw czynności dodatnią. Jest to realizowane za pośrednictwem aplikacji kondycji, który jest wstępnie zainstalowane na urządzeniach z systemem iOS 8. Podczas pierwszego uruchomienia aplikacji zestawu kondycji, użytkownik zobaczy pod kontrolą systemu **kondycji dostęp** okna dialogowego:

[![](healthkit-images/image10.png "Zostanie wyświetlone okno dialogowe kondycji dostęp pod kontrolą systemu")](healthkit-images/image10.png#lightbox)

Później, użytkownik może zmienić uprawnień przy użyciu aplikacji kondycji **źródeł** okna dialogowego:

[![](healthkit-images/image11.png "Użytkownik może zmienić uprawnień za pomocą okna dialogowego źródeł aplikacje kondycji")](healthkit-images/image11.png#lightbox)

Ponieważ informacje o kondycji jest bardzo poufne, deweloperzy aplikacji należy pamiętać o, zapisać swoje programy przy założeniu uprawnienia zostało odrzucone i zmienić, gdy aplikacja jest uruchomiona. Najbardziej typowe idiom ma zażądać uprawnień w `UIApplicationDelegate.OnActivated` metody, a następnie zmodyfikować interfejsu użytkownika.

### <a name="permissions-walkthrough"></a>Wskazówki uprawnień

W projekcie zainicjowano obsługę administracyjną zestawu kondycji otworzyć `AppDelegate.cs` pliku. Zwróć uwagę, za pomocą instrukcji `HealthKit`; w górnej części pliku.


Poniższy kod odnosi się do kondycji zestawu uprawnień:

```csharp
private HKHealthStore healthKitStore = new HKHealthStore ();

public override void OnActivated (UIApplication application)
{
        ValidateAuthorization ();
}

private void ValidateAuthorization ()
{
        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
        var heartRateType = HKObjectType.GetQuantityType (heartRateId);
        var typesToWrite = new NSSet (new [] { heartRateType });
        var typesToRead = new NSSet ();
        healthKitStore.RequestAuthorizationToShare (
                typesToWrite, 
                typesToRead, 
                ReactToHealthCarePermissions);
}

void ReactToHealthCarePermissions (bool success, NSError error)
{
        var access = healthKitStore.GetAuthorizationStatus (HKObjectType.GetQuantityType (HKQuantityTypeIdentifierKey.HeartRate));
        if (access.HasFlag (HKAuthorizationStatus.SharingAuthorized)) {
                HeartRateModel.Instance.Enabled = true;
        } else {
                HeartRateModel.Instance.Enabled = false;
        }
}

```

Cały kod w tych metod może odbywać się wbudowany w `OnActivated`, ale Przykładowa aplikacja używa oddzielnych metod aby ich zamiar jaśniejszy: `ValidateAuthorization()` zawiera kroki niezbędne, aby zażądać dostępu dla określonych typów zapisywana (i odczytu, w razie potrzeby aplikacji) i `ReactToHealthCarePermissions()` jest wywołaniem zwrotnym, które jest aktywowana po z okna dialogowego uprawnień w Health.app ma interakcją użytkowników.

Zadaniem `ValidateAuthorization()` jest utworzenie zestawu `HKObjectTypes` czy aplikacja będzie zapisu i żądania autoryzacji, aby zaktualizować te dane. W przykładowej aplikacji `HKObjectType` jest klucza `KHQuantityTypeIdentifierKey.HeartRate`. Ten typ jest dodawane do zestawu `typesToWrite`, podczas gdy zestaw `typesToRead` jest puste. Te zestawy, a odwołania do `ReactToHealthCarePermissions()` wywołania zwrotnego, są przekazywane do `HKHealthStore.RequestAuthorizationToShare()`.

`ReactToHealthCarePermissions()` Wywołania zwrotnego będzie wywoływana po wykonaniu ma interakcji z okna dialogowego uprawnień i jest przekazywany informacje użytkownika: `bool` wartość, która będzie `true` Jeśli użytkownik ma interakcji z okna dialogowego uprawnień i `NSError`Jeśli inną niż null, wskazuje określonego rodzaju błąd związany z wyświetlania okna dialogowego uprawnień.

> [!IMPORTANT]
> Jednoznaczne argumentów dla tej funkcji: _Powodzenie_ i _błąd_ parametrów nie wskazują, czy użytkownik ma uprawnienia dostępu do danych kondycji zestawu! Tylko wskazują, że użytkownik ma możliwość zezwolenie na dostęp do danych.

Aby potwierdzić, czy aplikacja ma dostęp do danych, `HKHealthStore.GetAuthorizationStatus()` jest używana, przekazując `HKQuantityTypeIdentifierKey.HeartRate`. Na podstawie stanu zwrócony, aplikacja włącza lub wyłącza możliwość wprowadzania danych. Nie środowisko użytkownika standardowego dotyczące postępowania w przypadku odmowy dostępu, a istnieje wiele możliwych opcji. W aplikacji przykładzie stan jest ustawiony na `HeartRateModel` pojedynczego obiektu, który z kolei wywołuje istotne zdarzenia.

## <a name="model-view-and-controller"></a>Model, widok i kontroler

Aby przejrzeć `HeartRateModel` pojedynczego obiektu, otwórz `HeartRateModel.cs` pliku:

```csharp
using System;
using HealthKit;
using Foundation;

namespace HKWork
{
        public class GenericEventArgs<T> : EventArgs
        {
                public T Value { get; protected set; }
                public DateTime Time { get; protected set; }

                public GenericEventArgs (T value)
                {
                        this.Value = value;
                        Time = DateTime.Now;
                }
        }

        public delegate void GenericEventHandler<T> (object sender,GenericEventArgs<T> args);

        public sealed class HeartRateModel : NSObject
        {
                private static volatile HeartRateModel singleton;
                private static object syncRoot = new Object ();

                private HeartRateModel ()
                {
                }

                public static HeartRateModel Instance {
                        get {
                                //Double-check lazy initialization
                                if (singleton == null) {
                                        lock (syncRoot) {
                                                if (singleton == null) {
                                                        singleton = new HeartRateModel ();
                                                }
                                        }
                                }

                                return singleton;
                        }
                }

                private bool enabled = false;

                public event GenericEventHandler<bool> EnabledChanged;
                public event GenericEventHandler<String> ErrorMessageChanged;
                public event GenericEventHandler<Double> HeartRateStored;

                public bool Enabled { 
                        get { return enabled; }
                        set {
                                if (enabled != value) {
                                        enabled = value;
                                        InvokeOnMainThread(() => EnabledChanged (this, new GenericEventArgs<bool>(value)));
                                }
                        }
                }

                public void PermissionsError(string msg)
                {
                        Enabled = false;
                        InvokeOnMainThread(() => ErrorMessageChanged (this, new GenericEventArgs<string>(msg)));
                }

                //Converts its argument into a strongly-typed quantity representing the value in beats-per-minute
                public HKQuantity HeartRateInBeatsPerMinute(ushort beatsPerMinute)
                {
                        var heartRateUnitType = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        var quantity = HKQuantity.FromQuantity (heartRateUnitType, beatsPerMinute);

                        return quantity;
                }
                        
                public void StoreHeartRate(HKQuantity quantity)
                {
                        var bpm = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        //Confirm that the value passed in is of a valid type (can be converted to beats-per-minute)
                        if (! quantity.IsCompatible(bpm))
                        {
                                InvokeOnMainThread(() => ErrorMessageChanged(this, new GenericEventArgs<string> ("Units must be compatible with BPM")));
                        }

                        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
                        var heartRateQuantityType = HKQuantityType.GetQuantityType (heartRateId);
                        var heartRateSample = HKQuantitySample.FromType (heartRateQuantityType, quantity, new NSDate (), new NSDate (), new HKMetadata());

                        using (var healthKitStore = new HKHealthStore ()) {
                                healthKitStore.SaveObject (heartRateSample, (success, error) => {
                                        InvokeOnMainThread (() => {
                                                if (success) {
                                                        HeartRateStored(this, new GenericEventArgs<Double>(quantity.GetDoubleValue(bpm)));
                                                } else {
                                                        ErrorMessageChanged(this, new GenericEventArgs<string>("Save failed"));
                                                }
                                                if (error != null) {
                                                        //If there's some kind of error, disable 
                                                        Enabled = false;
                                                        ErrorMessageChanged (this, new GenericEventArgs<string>(error.ToString()));
                                                }
                                        });
                                });
                        }
                }
        }
}

```

Pierwsza sekcja jest schematyczny kod służący do tworzenia zdarzeń ogólnych i obsługi. Początkowy fragment `HeartRateModel` klasy jest również umożliwiającego tworzenia obsługującej wielowątkowość pojedynczego obiektu.

Następnie `HeartRateModel` opisuje 3 zdarzenia: 

- `EnabledChanged` — Wskazuje, że Puls magazynu została włączona lub wyłączona (należy pamiętać, że magazyn jest początkowo wyłączona). 
- `ErrorMessageChanged` — Dla tej aplikacji przykładowej, mamy bardzo prosty model obsługi błędów: ciąg znaków z ostatniego błędu. 
- `HeartRateStored` -Wywoływane, gdy Puls są przechowywane w bazie danych zestawu kondycji.

Należy pamiętać, że w każdym przypadku, gdy są uruchamiane te zdarzenia, jest wykonywane za pośrednictwem `NSObject.InvokeOnMainThread()`, dzięki czemu subskrybentów można zaktualizować interfejsu użytkownika. Alternatywnie zdarzenia może być przedstawiany jako zgłaszanych na wątki w tle i odpowiedzialność za zapewnienie zgodności może pozostać do swoich programów obsługi. Zagadnienia dotyczące wątku są ważne w zestawu kondycji aplikacji, ponieważ wiele funkcji, takich jak żądania uprawnień są asynchroniczne i wykonać ich wywołania zwrotne w wątkach-main.

Kod określonego zestawu kondycji w `HeartRateModel` znajduje się w dwóch funkcji `HeartRateInBeatsPerMinute()` i `StoreHeartRate()`. 

`HeartRateInBeatsPerMinute()` Konwertuje jej argument zestawu kondycji jednoznacznie `HKQuantity`. Typ ilość jest określona przez `HKQuantityTypeIdentifierKey.HeartRate` i jednostek ilość `HKUnit.Count` rozdzielonych `HKUnit.Minute` (innymi słowy, jest jednostka *uderzeń na minutę*). 

`StoreHeartRate()` Funkcja przyjmuje `HKQuantity` (w przykładowej aplikacji, utworzony przez `HeartRateInBeatsPerMinute()` ). Aby zweryfikować swoje dane, używa `HKQuantity.IsCompatible()` metody, która zwraca `true` Jeśli można przekonwertować obiektu jednostki do jednostki w argumencie. Jeśli ilość została utworzona z `HeartRateInBeatsPerMinute()` to oczywiście zwróci `true`, ale również zwróci `true` Jeśli ilość zostały utworzone, jak na przykład *uderzeń na godzinę*. Zazwyczaj `HKQuantity.IsCompatible()` może służyć do sprawdzania poprawności masa, odległości i energii, które użytkownik lub urządzenie może danych wejściowych lub wyświetlić w jednym systemie miary (na przykład w jednostkach Imperialnych), ale mogą być przechowywane w innym systemie (np. jednostek). 

Po zweryfikowaniu kompatybilności ilości `HKQuantitySample.FromType()` metoda fabryki umożliwiająca utworzenie silnie typizowanego `heartRateSample` obiektu. `HKSample` obiekty mają datę początkową i końcową; do natychmiastowej odczytów te wartości powinny być takie same, jak w przykładzie. Próbki nie ustawisz żadnych danych klucz wartość jego `HKMetadata` argumentu, ale co można użyć kodu na przykład następujący kod do określenia lokalizacji czujnik:

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

Raz `heartRateSample` został utworzony, kod tworzy nowe połączenie z bazą danych przy użyciu bloku. W tym bloku `HKHealthStore.SaveObject()` metody prób asynchroniczny zapis do bazy danych. Wynikowa wywołanie wyrażenia lambda albo wyzwala odpowiednie zdarzenia `HeartRateStored` lub `ErrorMessageChanged`.

Obecnie program modelu jest czas, aby zobaczyć, jak kontroler odzwierciedla stan modelu. Otwórz `HKWorkViewController.cs` pliku. Po prostu tworzącej konstruktora `HeartRateModel` pojedyncze do metody obsługi zdarzeń (ponownie, można to zrobić wbudowany z wyrażenia lambda, ale metody oddzielne upewnij zamiar nieco bardziej oczywistymi):

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

Poniżej przedstawiono istotne programów obsługi:

```csharp
void OnEnabledChanged (object sender, GenericEventArgs<bool> args)
{
        StoreData.Enabled = args.Value;
        PermissionsLabel.Text = args.Value ? "Ready to record" : "Not authorized to store data.";
        PermissionsLabel.SizeToFit ();
}

void OnErrorMessageChanged (object sender, GenericEventArgs<string> args)
{
        PermissionsLabel.Text = args.Value;
}

void OnHeartBeatStored (object sender, GenericEventArgs<double> args)
{
        PermissionsLabel.Text = String.Format ("Stored {0} BPM", args.Value);
}

```

Oczywiście w aplikacji z jednego kontrolera, byłoby możliwe w celu uniknięcia utworzenia obiektu modelu oddzielne i stosowania zdarzenia dla przepływu sterowania, ale modelu obiektów jest bardziej odpowiednia w przypadku aplikacji rzeczywistych.

## <a name="running-the-sample-app"></a>Przykładowa aplikacja uruchomiona

Symulatora systemu iOS nie obsługuje zestawu kondycji. Debugowanie musi odbywać się na urządzeniu fizycznym z systemem iOS 8.

Dołącz urządzenia programowanie prawidłowo elastycznie iOS 8 do systemu. Wybierz go jako cel wdrożenia w programie Visual Studio dla komputerów Mac i z menu wybierz **Uruchom > debugowanie**.

> [!IMPORTANT]
> Błędy dotyczące inicjowania obsługi będzie w tym momencie powierzchni. Rozwiązywanie problemów, przejrzyj tworzenie i Inicjowanie obsługi powyższej sekcji kondycji zestawu aplikacji. Dostępne są następujące składniki: 
>
> - **Centrum deweloperów systemu iOS** -jawny identyfikator aplikacji & Kit kondycji włączono profil inicjowania obsługi administracyjnej. 
> - **Opcje projektu** — identyfikator pakietu (jawny identyfikator aplikacji) i profilu inicjowania obsługi administracyjnej.
> - **Kod źródłowy** -Entitlements.plist & Info.plist

Przy założeniu, że przepisy zostały ustawione prawidłowo, aplikacja zostanie uruchomione. Po osiągnięciu jego `OnActivated` metody, będzie żądać autoryzacji zestawu kondycji. Przy pierwszym uruchomieniu występujących systemu operacyjnego użytkownika zostanie wyświetlone następujące okno dialogowe:


[![](healthkit-images/image12.png "Użytkownik zobaczy tego okna dialogowego")](healthkit-images/image12.png#lightbox)

Włącz aplikację, aby zaktualizować dane o szybkości pulsu i aplikacja pojawi się ponownie. `ReactToHealthCarePermissions` Zostanie uaktywniona asynchroniczne wywołanie zwrotne. Spowoduje to `HeartRateModel’s` `Enabled` właściwości do zmiany, które zostanie podniesiony `EnabledChanged` zdarzeń, co spowoduje `HKPermissionsViewController.OnEnabledChanged()` obsługi zdarzeń, aby uruchomić, co pozwoli `StoreData` przycisku. Na poniższym diagramie przedstawiono sekwencję:


[![](healthkit-images/image13.png "Na tym wykresie przedstawiono sekwencję zdarzeń")](healthkit-images/image13.png#lightbox)

Naciśnij klawisz **rekordu** przycisku. Spowoduje to `StoreData_TouchUpInside()` obsługi do uruchomienia, który będzie podejmować próby przeanalizować wartość `heartRate` pola tekstowego, przekształcić `HKQuantity` za pośrednictwem już wspomniano `HeartRateModel.HeartRateInBeatsPerMinute()` funkcji i przekaż ilości do `HeartRateModel.StoreHeartRate()`. Jak wspomniano wcześniej, to podejmie do przechowywania danych i zgłosi albo `HeartRateStored` lub `ErrorMessageChanged` zdarzeń.

Kliknij dwukrotnie **Home** znajdującego się na urządzeniu i otwórz kondycji aplikacji. Kliknij przycisk **źródeł** kartę i zostanie wyświetlony przykładową aplikację na liście. Wybierz go, a nie zezwalaj na uprawnień, aby zaktualizować dane o szybkości pulsu. Kliknij dwukrotnie **Home** przycisk i przełącznik z powrotem do aplikacji. Ponownie `ReactToHealthCarePermissions()` zostanie wywołany, ale tym razem, ponieważ nastąpiła odmowa dostępu, **StoreData** zostanie wyłączony przycisk (Zauważ, że przyczyną jest asynchronicznie i zmiany w interfejsie użytkownika mogą być widoczne dla użytkownika końcowego).

## <a name="advanced-topics"></a>Tematy zaawansowane

Odczytywanie danych z zestawu kondycji bazy danych jest bardzo podobny do zapisywania danych: jeden Określa typy danych, co próbujesz uzyskać dostęp, autoryzacji żądania i udzielenie autoryzacji, że dane są dostępne, z automatycznej konwersji do jednostek zgodne miary.

Istnieje szereg bardziej zaawansowane funkcje zapytań, umożliwiających zapytań na podstawie predykat i zapytań, które wykonują aktualizacje po zaktualizowaniu odpowiednie dane. 

Deweloperzy aplikacji zestawu kondycji powinni sprawdzić sekcji zestawu kondycji firmy Apple [wytyczne przeglądu aplikacji](https://developer.apple.com/app-store/review/guidelines/#healthkit).

Po zrozumieniu zabezpieczeń i system typów modeli zapisywanie i odczytywanie danych w bazie danych kondycji zestaw udostępnionego jest dość proste. Wiele funkcji w ramach zestawu kondycji działają asynchronicznie i deweloperzy aplikacji musi odpowiednio zapisać swoje programy.

Przeprowadzonych podczas opracowywania tego artykułu, nie ma obecnie odpowiednika kondycji Kit systemu Android lub Windows Phone.

## <a name="summary"></a>Podsumowanie

W tym artykule został pokazano, jak zestaw kondycji umożliwia aplikacjom przechowywanie pobieranie i kondycji udziału informacji powiązanych z, jednocześnie zapewniając standardowych aplikacji kondycji, który umożliwia użytkownikowi dostępu i kontroli nad te dane. 

Zaobserwowano również, jak prywatność, zabezpieczeń i integralność danych są zastępowanie problemów związanych z kondycją informacji i aplikacji przy użyciu zestawu kondycji musi dotyczyć zwiększenia złożoności aplikacji związanych z zarządzaniem (Inicjowanie obsługi administracyjnej) (zestaw kondycji typ kodowania System) i interfejs (kontrola użytkownika uprawnień za pomocą okien dialogowych systemu i aplikacji kondycji) użytkownika. 

Na koniec mamy biorąc przyjrzeć się proste wdrożenia przy użyciu zapisuje dane pulsu w magazynie kondycji zestawu i ma asynchroniczne obsługujący projektowania aplikacji dołączonego przykładowego zestawu kondycji.

## <a name="related-links"></a>Linki pokrewne

- [HKWork (przykład)](https://developer.xamarin.com/samples/monotouch/ios8/IntroToHealthKit/)
- [Wprowadzenie do systemu iOS 8](~/ios/platform/introduction-to-ios8.md)
