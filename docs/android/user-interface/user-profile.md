---
title: Profil użytkownika
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/22/2018
ms.openlocfilehash: 1eaae86ab9eacf007eca792d96e889db6f367922
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="user-profile"></a>Profil użytkownika

Android obsługiwał Wylicz kontakty z [ContactsContract](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/) dostawcy od 5 poziom interfejsu API. Na przykład wyświetlanie listy kontaktów jest tak proste, jak za pomocą [ContactContracts.Contacts](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Contacts/) klasy, jak pokazano w poniższym przykładzie:

```csharp
// Get the URI for the user's contacts:
var uri = ContactsContract.Contacts.ContentUri;

// Setup the "projection" (columns we want) for only the ID and display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id, 
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the user's contacts data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();

// Print the contact data to the console if reading back succeeds:
if (cursor != null)
{
    if (cursor.MoveToFirst())
    {
        do
        {
            Console.WriteLine("Contact ID: {0}, Contact Name: {1}",
                               cursor.GetString(cursor.GetColumnIndex(projection[0])),
                               cursor.GetString(cursor.GetColumnIndex(projection[1])));
        } while (cursor.MoveToNext());
    }
}
```

Począwszy od systemu Android 4 (14 poziom interfejsu API), [ContactsContact.Profile](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Profile/) klasy jest dostępna za pośrednictwem `ContactsContract` dostawcy. `ContactsContact.Profile` Zapewnia dostęp do osobistych profilu dla właściciela urządzenia, który zawiera dane kontaktowe, takie jak właściciel urządzenia nazwę i numer telefonu.


## <a name="required-permissions"></a>Wymagane uprawnienia

Odczytywanie i zapisywanie danych kontaktowych, należy zażądać aplikacji `READ_CONTACTS` i `WRITE_CONTACTS` uprawnienia, odpowiednio.
Ponadto do odczytu i edycji profilu użytkownika, aplikacje należy zażądać `READ_PROFILE` i `WRITE_PROFILE` uprawnienia.


## <a name="updating-profile-data"></a>Aktualizowanie danych profilu

Po ustawieniu te uprawnienia aplikacji można użyć normalne techniki systemu Android do interakcji z danymi profilu użytkownika. Na przykład, aby zaktualizować profil, nazwa wyświetlana, należy wywołać [ContentResolver.Update](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.Update) z `Uri` pobierane w drodze [ContactsContract.Profile.ContentRawContactsUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentRawContactsUri/) właściwości, jak pokazano poniżej:

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>Odczytywanie danych profilu

Uruchamianie zapytania do [ContactsContact.Profile.ContentUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentUri/) odczyty kopii danych profilu. Na przykład następujący kod odczytuje profil użytkownika, nazwa wyświetlana:

```csharp
// Read the profile
var uri = ContactsContract.Profile.ContentUri;

// Setup the "projection" (column we want) for only the display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();
if (cursor != null)
{
    if (cursor.MoveToFirst ())
    {
        Console.WriteLine(cursor.GetString (cursor.GetColumnIndex (projection [0])));
    }
}
```

## <a name="navigating-to-the-user-profile"></a>Nawigowanie do profilu użytkownika

Na koniec można przejść do profilu użytkownika, należy utworzyć celem z `ActionView` akcji i `ContactsContract.Profile.ContentUri` następnie przekaż go do `StartActivity` metody następująco:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

Podczas uruchamiania powyższej kodu, profil użytkownika jest wyświetlane, jak pokazano na poniższym zrzucie ekranu:

[![Zrzut ekranu przedstawiający profilu wyświetlanie profilu użytkownika Jan Kowalski](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

Praca z profilem użytkownika jest podobna do interakcji z innymi danymi w systemie Android, i zapewnia dodatkowy poziom personalizacji urządzenia.



## <a name="related-links"></a>Linki pokrewne

- [ContactsProviderDemo (przykład)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Wprowadzenie Sandwich lodów](http://www.android.com/about/ice-cream-sandwich/)
- [Platformy systemu android 4.0](http://developer.android.com/sdk/android-4.0.html)
