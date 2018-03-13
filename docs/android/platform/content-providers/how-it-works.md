---
title: "Jak zawartości pracy dostawców"
ms.topic: article
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 7802988833563469fcc25e03ee1bda2046591681
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="how-content-providers-work"></a>Jak zawartości pracy dostawców

Istnieją dwie klasy związane z `ContentProvider` interakcji:

- **ContentProvider** &ndash; implementuje interfejs API, który przedstawia w sposób standardowy zestaw danych. Główne sposoby są zapytania, Insert, Update i Delete.

- **Klasy ContentResolver** &ndash; statycznych serwera proxy, który komunikuje się z `ContentProvider` dostępu do danych, albo z tej samej aplikacji lub z innej aplikacji.

Dostawcy zawartości zwykle jest przechowywana w bazie danych SQLite, ale interfejsu API oznacza, że wykorzystywanie kodu nie muszą mieć żadnych informacji o źródłowy SQL. Zapytania są wykonywane za pomocą identyfikatora Uri odwołania nazwy kolumn (w celu zmniejszenia zależności na podstawowej struktury danych), za pomocą stałych i `ICursor` są zwracane do kodu odbierającą do wykonywania iteracji.


## <a name="consuming-a-contentprovider"></a>Korzystanie z ContentProvider

`ContentProviders` udostępniać swoje funkcje za pomocą identyfikatora Uri, który jest zarejestrowany w **AndroidManifest.xml** aplikacji, która publikuje dane. Brak Konwencji gdzie identyfikator Uri i kolumny danych, które są dostępne powinny być dostępne jako stałe, aby ułatwić powiązać dane. Android przez wbudowane `ContentProviders` dostarczyć wszystkie klasy wygody, aby stałe, które odwołują się do struktury danych w [ `Android.Providers` ](https://developer.xamarin.com/api/namespace/Android.Provider/) przestrzeni nazw.



### <a name="built-in-providers"></a>Dostawców wbudowanych

Android udostępnia szeroką gamę systemu i danych użytkownika przy użyciu `ContentProviders`:

- *Przeglądarka* &ndash; zakładek i historii przeglądarki (wymaga uprawnień `READ_HISTORY_BOOKMARKS` i/lub `WRITE_HISTORY_BOOKMARKS`).

- *CallLog* &ndash; wywołania ostatnie wykonane lub odebranych z urządzeniem.

- *Kontakty* &ndash; szczegółowych informacji z listy kontaktów użytkownika, w tym osób, telefonów, zdjęć i grup.

- *MediaStore* &ndash; zawartości na urządzeniu użytkownika: audio (albumów, artystów, genres, listy odtwarzania), obrazów (w tym miniatur) i wideo.

- *Ustawienia* &ndash; urządzenie systemowe ustawienia i preferencje.

- *UserDictionary* &ndash; zawartość słownika zdefiniowane przez użytkownika używane do wprowadzania tekstu predykcyjnej.

- *Poczty głosowej* &ndash; historii wiadomości poczty głosowej.



## <a name="classes-overview"></a>Przegląd klas

Klasy podstawowej, używane podczas pracy z `ContentProvider` są wyświetlane tutaj:

[![Diagram klas dostawcy zawartości aplikacji i Consuming interakcji aplikacji](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

Na tym diagramie `ContentProvider` implementuje zapytania i rejestruje identyfikator URI używanego przez inne aplikacje do lokalizowania danych. `ContentResolver` Działa jako serwer proxy do `ContentProvider` (zapytania, wstawiania, aktualizowania i usuwania metody). `SQLiteOpenHelper` Zawiera dane używane przez `ContentProvider`, ale nie jest bezpośrednio narażony na których działają aplikacje.
`CursorAdapter` Przekazuje zwrócony przez kursor `ContentResolver` do wyświetlenia w `ListView`. `UriMatcher` Jest klasa pomocy, która analizuje identyfikatorów URI podczas przetwarzania zapytania.

Poniżej opisano przeznaczenie każdej klasy:

- **ContentProvider** &ndash; implementować metod klasy abstrakcyjnej do udostępniania danych. Interfejs API jest udostępniana do innych klas i aplikacji za pomocą atrybutu identyfikatora Uri, który jest dodawany do definicji klasy.

- **SQLiteOpenHelper** &ndash; pomaga wdrożenia magazynu danych SQLite udostępnianym przez `ContentProvider`.

- **UriMatcher** &ndash; użyj `UriMatcher` w Twojej `ContentProvider` implementacji, aby ułatwić zarządzanie identyfikatory URI, które są wykorzystywane do badania zawartości.

- **Klasy ContentResolver** &ndash; wykorzystywanie kodu używa `ContentResolver` dostępu `ContentProvider` wystąpienia. Dwie klasy razem zajmie się problemy komunikacji międzyprocesowej, dzięki czemu dane można łatwo udostępniać między aplikacjami. Nigdy nie zużywa kod tworzy `ContentProvider` klasy jawnie; zamiast tego, dostęp do danych przez utworzenie kursora prowadzącego oparte na udostępnianych przez identyfikator Uri `ContentProvider` aplikacji.

- **CursorAdapter** &ndash; użyj `CursorAdapter` lub `SimpleCursorAdapter` do wyświetlania danych udostępnianych za pośrednictwem `ContentProvider`.

`ContentProvider` Interfejs API umożliwia konsumentów do wykonywania różnych operacji na danych, takich jak:

-  Zapytanie danych do zwrócenia listy lub poszczególne rekordy.
-  Zmodyfikuj poszczególne rekordy.
-  Dodaj nowe rekordy.
-  Usuwanie rekordów.

Ten dokument zawiera przykładowy używa dostarczane przez system `ContentProvider`, a także prosty przykład tylko do odczytu, który implementuje niestandardowego `ContentProvider`.

