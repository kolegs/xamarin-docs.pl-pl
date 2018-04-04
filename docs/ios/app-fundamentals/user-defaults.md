---
title: Praca z domyślnych ustawień użytkownika
description: Ten artykuł dotyczy pracy z NSUserDefault, aby zapisać ustawienia domyślne w Xamarin iOS aplikacji lub rozszerzenia.
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: aa28e7d5636b06c8ab1e46457537431b5d1c7f1a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-user-defaults"></a>Praca z domyślnych ustawień użytkownika

_Ten artykuł dotyczy pracy z NSUserDefault, aby zapisać ustawienia domyślne w aplikacji platformy Xamarin.iOS lub rozszerzenia._


`NSUserDefaults` Klasa udostępnia sposób dla systemu iOS aplikacje i rozszerzenia do interakcji programowo w systemie systemowe ustawienia domyślne. Przy użyciu ustawienia domyślne systemu, użytkownik może konfigurować zachowanie aplikacji lub style w celu spełnienia preferencji (oparte na projekt aplikacji). Na przykład, aby przedstawiać dane w programie vs metryki angielskie pomiary, lub wybierz motyw danego interfejsu użytkownika.

W przypadku użycia z grup aplikacji `NSUserDefaults` także sposób komunikacji między aplikacjami (lub rozszerzenia) w ramach danej grupy.

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>O domyślnych ustawień użytkownika

Jak wspomniano powyżej, domyślne ustawienia użytkownika (`NSUserDefaults`) można dodać do aplikacji (lub rozszerzenia) i umożliwia zapewnienie konfigurowalne opcje, które użytkownik końcowy może modyfikować można dostosować wygląd i działanie aplikacji w czasie wykonywania.

Gdy aplikacja najpierw wykonuje, `NSUserDefaults` odczytuje klucze i wartości z bazy danych aplikacji użytkownika domyślne i przechowuje je w pamięci, aby uniknąć otwieranie i Odczyt bazy danych zawsze wymagana jest wartość. 

> [!IMPORTANT]
> Apple już zaleca wywołania developer `Synchronize` metodę, aby zsynchronizować bezpośrednio w pamięci podręcznej z bazą danych. Zamiast tego zostanie automatycznie wywołany okresowo, aby zapewnić synchronizację z bazą danych domyślne ustawienia użytkownika w pamięci podręcznej.

`NSUserDefaults` Klasa zawiera kilka metod wygody do odczytywania i zapisywania preferencji wartości dla typowych danych, takich jak: ciąg, liczbę całkowitą, float, boolean i adresów URL. Inne typy danych mogą być archiwizowane za pomocą `NSData`, następnie odczytu lub zapisu w bazie danych użytkownika ustawienia domyślne. Aby uzyskać więcej informacji, zobacz firmy Apple [preferencji i ustawień przewodnik programowania w języku](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i).

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>Uzyskiwanie dostępu do wystąpienia NSUserDefaults udostępnionego 

Wystąpienie domyślne użytkownika udostępnionego zapewnia dostęp do domyślne ustawienia użytkownika dla bieżącego użytkownika urządzenia. Jeśli domyślnie przyjmowana jest udostępniony obiekt nie istnieje, jest ona tworzona po raz pierwszy jest dostępne i zainicjowany z następującymi informacjami:

- `NSArgumentDomain` Składające się z wartości domyślne przeanalizować z bieżącej aplikacji.
- Domena identyfikator pakietu aplikacji.
- `NSGlobalDomain` Składające się z wartości domyślnych, współużytkowana przez wszystkie aplikacje.
- Osobnej domenie dla każdego z języków preferowanym przez użytkownika.
- `NSRegistrationDomain` z zestawem tymczasowej wartości domyślnych, które mogą być modyfikowane przez aplikację, aby upewnić się, są zawsze pomyślnych wyszukiwań.

Aby uzyskać dostęp do wystąpienia domyślne użytkownika udostępnionego, należy użyć poniższego kodu:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>Uzyskiwanie dostępu do wystąpienia NSUserDefaults grupy aplikacji

Jak już wspomniano, korzystając z grup aplikacji `NSUserDefaults` może służyć do komunikacji między aplikacjami (lub rozszerzenia) w ramach danej grupy. Najpierw należy upewnić się, że grupy aplikacji i wymagane identyfikatorów aplikacji zostały poprawnie skonfigurowane w **certyfikaty, identyfikatory & profile** sekcji na [Centrum deweloperów systemu iOS](https://developer.apple.com/devcenter/ios/) i zostały zainstalowane w środowisku programistycznym.

Następnie projektów aplikacji i/lub rozszerzenie musi być jedną z prawidłową identyfikatorów aplikacji utworzone powyżej i `Entitlements.plist` pliku musi zostać uwzględniony w pakiecie aplikacji przy użyciu grup aplikacji włączone i określone.

Z tym wszystkie w miejscu udostępnionych aplikacji grupy domyślne ustawienia użytkownika jest możliwy przy użyciu następującego kodu:

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

Gdzie `group.com.xamarin.todaysharing` utworzeniu grupy aplikacji w **certyfikaty, identyfikatory & profile** którego chcesz uzyskać dostęp. Aby uzyskać więcej informacji, zobacz [możliwości grupy aplikacji](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) dokumentacji.

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>Odczytywanie wartości domyślnych

Po możesz uzyskać dostęp do żądanego domyślna baza danych użytkownika, można odczytywać wartości ustawienia domyślne przy użyciu pary klucz-wartość i kilka metod wygody na podstawie typu odczytywane dane:

- `ArrayForKey` -Zwraca tablicę `NSObjects` dla danej wartości klucza.
- `BoolForKey` -Zwraca wartość typu boolean dla danego klucza.
- `DataForKey` -Zwraca `NSData` obiektu dla danego klucza.
- `DictionaryForKey` -Zwraca `NSDictionary` dla danego klucza.
- `DoubleForKey` -Zwraca wartość typu double dla danego klucza.
- `FloatForKey` -Zwraca wartość typu float dla danego klucza.
- `IntForKey` -Zwraca wartość całkowitą dla danego klucza.
- `StringArrayForKey` -Zwraca tablicę `String` obiektów z danej wartości klucza.
- `StringForKey` -Zwraca wartość ciągu dla danego klucza.
- `URLForKey` -Zwraca `NSUrl` wartość dla danego klucza.

Na przykład następujący kod będzie odczytywać wartość logiczną domyślne ustawienia użytkownika:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values" />

## <a name="writing-default-values"></a>Zapisywanie wartości domyślne

Podobnie jak odczytywania powyższych wartości, po możesz uzyskać dostęp do żądanego domyślnej bazy danych użytkowników, można napisać wartości domyślne przy użyciu pary klucz-wartość i kilka metod wygody na podstawie typu danych:

- `SetBool` -Zapisuje wartość logiczną do danego klucza.
- `SetDouble` -Zapisuje podanej wartości double do danego klucza.
- `SetFloat` -Zapisuje wartość danego typu float do danego klucza.
- `SetString` -Zapisuje wartość ciągu do danego klucza.
- `SetURL` -Zapisuje podany adres URL (`NSUrl`) wartość do danego klucza.

Na przykład następujący kod zapisać wartość logiczną na domyślne ustawienia użytkownika:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> Gdy aplikacja najpierw wykonuje, `NSUserDefaults` odczytuje klucze i wartości z bazy danych aplikacji użytkownika domyślne i przechowuje je w pamięci, aby uniknąć otwieranie i Odczyt bazy danych zawsze wymagana jest wartość.



<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego `NSUserDefaults` klasy i jak może służyć do zapewnienia zestaw opcji, które użytkownik końcowy służy do konfigurowania aplikacji platformy Xamarin.iOS. Ponadto opisano korzystania z grup aplikacji do komunikacji między rozszerzenie i jego nadrzędnego aplikacji lub aplikacji w grupie.


## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Przewodnik programowania w języku ustawienia i preferencje](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
