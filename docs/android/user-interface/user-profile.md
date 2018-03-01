---
title: "Profil użytkownika"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2017
ms.openlocfilehash: 53ac30abea05095583fcac5ddc315f93ce7024f2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="user-profile"></a>Profil użytkownika

Android obsługiwał Wylicz kontakty z `ContactsContract` dostawcy od 5 poziom interfejsu API. Na przykład do listy kontaktów jest tak proste, jak za pomocą `ContactContracts.Contacts` klasy, jak pokazano w poniższym kodzie:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
           
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id,
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);
           
if (cursor.MoveToFirst ()) {
    do {
        Console.WriteLine ("Contact ID: {0}, Contact Name: {1}",
            cursor.GetString (cursor.GetColumnIndex (projection [0])),
            cursor.GetString (cursor.GetColumnIndex (projection [1])));
                   
    } while (cursor.MoveToNext());
}
```

Z systemem Android 4 (14 poziom interfejsu API) nowy `ContactsContact.Profile` klasy jest dostępny za pośrednictwem dostawcy ContactsContract. `ContactsContact.Profile` Zapewnia dostęp do osobistych profilu dla właściciela urządzenia, który zawiera dane kontaktowe, takie jak właściciel urządzenia nazwę i numer telefonu.

<a name="Required_Permissions" />

## <a name="required-permissions"></a>Wymagane uprawnienia

Odczytywanie i zapisywanie danych kontaktowych, należy zażądać aplikacji `Read_Contacts` i `Write_Contacts` uprawnienia, odpowiednio. Ponadto do odczytu i edycji profilu użytkownika, aplikacje należy zażądać `Read_Profile` i `Write_Profile` uprawnienia.

<a name="Updating_Profile_Data" />

## <a name="updating-profile-data"></a>Aktualizowanie danych profilu

Po ustawieniu te uprawnienia aplikacji można użyć normalne techniki systemu Android do interakcji z danymi profilu użytkownika. Na przykład, aby zaktualizować profil Nazwa wyświetlana będzie nazywa się `ContentResolver.Update` z `Uri` pobierane w drodze `ContactsContract.Profile.ContentRawContactsUri` właściwości, jak pokazano poniżej:

```csharp
var values = new ContentValues ();
          
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName,
    "John Doe");
           
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri,
    values, null, null);
```

<a name="Reading_Profile_Data" />

## <a name="reading-profile-data"></a>Odczytywanie danych profilu

Uruchamianie zapytania do `ContactsContact.Profile.ContentUri` odczyty kopii danych profilu. Na przykład następujący kod odczytuje profil użytkownika, nazwa wyświetlana:

```csharp
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);

if (cursor.MoveToFirst ()) {
    Console.WriteLine(
        cursor.GetString (cursor.GetColumnIndex (projection [0])));
}
```

<a name="Navigating_to_the_People_App" />

## <a name="navigating-to-the-people-app"></a>Nawigacja do aplikacji użytkowników

Na koniec, aby przejść do profilu użytkownika w nowej aplikacji osoby, z dołączoną do systemu Android 4, po prostu utworzyć celem z `ActionView` akcji i `ContactsContract.Profile.ContentUri`i przekaż go do `StartActivity` metody następująco:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

Podczas uruchamiania powyższej kodu, aplikacja osób załaduje profilu użytkownika, jak pokazano na poniższym zrzucie ekranu:

[![Zrzut ekranu osób aplikacji wyświetlanie profilu użytkownika Jan Kowalski](user-profile-images/15-people-app.png)](user-profile-images/15-people-app.png)

Praca z profilu użytkownika teraz jest podobny do interakcji z innymi danymi w systemie Android i zapewnia dodatkowy poziom personalizacji urządzenia.



## <a name="related-links"></a>Linki pokrewne

- [ContactsProviderDemo (przykład)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Wprowadzenie Sandwich lodów](http://www.android.com/about/ice-cream-sandwich/)
- [Platformy systemu android 4.0](http://developer.android.com/sdk/android-4.0.html)
