---
title: Przy użyciu ContentProvider kontaktów
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 754b81cec7f1adbe5c7ff1c820260e162e226b15
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="using-the-contacts-contentprovider"></a>Przy użyciu ContentProvider kontaktów

Czy używa uzyskać dostęp do danych udostępnianych przez kod `ContentProvider` nie wymaga odwołania do `ContentProvider` klasy w ogóle. Zamiast tego identyfikatora Uri jest używany do tworzenia kursor nad danymi udostępnianych przez `ContentProvider`. Android używa identyfikatora Uri do wyszukiwania systemu dla aplikacji, która przedstawia `ContentProvider` z danym identyfikatorem. Identyfikator Uri jest ciągiem, zwykle w formacie wstecznego DNS `com.android.contacts/data`.

Zamiast wprowadzania deweloperzy Pamiętaj ten ciąg Android *kontaktów* Dostawca udostępnia metadanych w `android.provider.ContactsContract` klasy. Ta klasa służy do określania identyfikatora Uri `ContentProvider` oraz nazwy tabel i kolumn, które można wyszukiwać.

Niektóre typy danych również wymagać specjalne uprawnienia dostępu. Wymaga wbudowanej listy kontaktów `android.permission.READ_CONTACTS` uprawnień w **AndroidManifest.xml** pliku.

Istnieją trzy sposoby tworzenia kursora z identyfikatora Uri:

1. **ManagedQuery()** &ndash; to preferowane rozwiązanie w systemie Android 2.3 (10 poziom interfejsu API) i starszych `ManagedQuery` Zwraca kursor i automatycznie zarządza odświeżanie danych i zamykanie kursora. Ta metoda jest przestarzała w systemie Android 3.0 (11 poziom interfejsu API).

1. **ContentResolver.Query()** &ndash; Zwraca kursor niezarządzane, co oznacza muszą być odświeżane i jawnie zamknięty w kodzie.

1. **CursorLoader(). LoadInBackground()** &ndash; wprowadzone w systemie Android 3.0 (11 poziom interfejsu API), `CursorLoader` teraz jest preferowany sposób korzystać z `ContentProvider` . `CursorLoader` zapytania `ContentResolver` wątku w tle interfejsu użytkownika nie jest blokowany.
   Ta klasa są dostępne w starszych wersjach systemu Android za pomocą biblioteki zgodności v4.


Każda z tych metod ma ten sam podstawowy zestaw danych wejściowych:

-  **Identyfikator URI** &ndash; w pełni kwalifikowana nazwa `ContentProvider` .
-  **Projekcja** &ndash; specyfikacji kolumny, które można wybrać dla kursora.
-  **Wybór** &ndash; podobny do SQL `WHERE` klauzuli.
-  **SelectionArgs** &ndash; parametry mają zostać podstawione w zaznaczeniu.
-  **SortOrder** &ndash; kolumny, aby posortować według.



## <a name="creating-inputs-for-a-query"></a>Tworzenie danych wejściowych dla zapytania

`ContactsProvider` Przykładowy kod wykonuje bardzo proste zapytanie wbudowanego dostawcy kontaktów dla systemu Android. Nie trzeba znać rzeczywistej nazwy identyfikatora Uri lub kolumny — wszystkie informacje wymagane do badania kontaktów `ContentProvider` jest dostępna jako stałe udostępnianych przez `ContactsContract` klasy.

Niezależnie od tego, która metoda służy do pobierania kursora, te same obiekty są używane jako parametry, jak pokazano w *ContactsProvider/ContactsAdapter.cs* pliku:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

Na przykład `selection`, `selectionArgs` i `sortOrder` zostanie zignorowany przez ustawienie `null`.



## <a name="creating-a-cursor-from-a-content-provider-uri"></a>Utworzenie kursora prowadzącego z identyfikatora Uri dostawcy zawartości

Po utworzeniu obiektów parametru użyciem w jednej z następujących trzech sposobów:



### <a name="using-a-managed-query"></a>Przy użyciu zarządzanego zapytania

Aplikacji przeznaczonych dla systemu Android 2.3 (10 poziom interfejsu API) lub starszy, należy użyć tej metody:

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

Ten kursor będą zarządzane przez system Android, dzięki czemu nie trzeba go zamknąć.



### <a name="using-contentresolver"></a>Za pomocą klasy ContentResolver

Uzyskiwanie dostępu do `ContentResolver` bezpośrednio do pobrania kursora przed `ContentProvider` może być podobnie do następującej:

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

Tego kursora jest niezarządzany, więc musi zostać zamknięty, jeśli nie jest już wymagane.
Upewnij się, że kod zamyka kursora, która jest otwarta, w przeciwnym razie wystąpi błąd.

```csharp
cursor.Close();
```

Alternatywnie możesz wywołać `StartManagingCursor()` i `StopManagingCursor()` do zarządzania kursora. Kursory zarządzane zostaną automatycznie zdezaktywowana i ponownym uruchomieniem zapytania po zatrzymaniu i ponownym uruchomieniu działania.



### <a name="using-cursorloader"></a>Przy użyciu CursorLoader

Aplikacje utworzone dla systemu Android 3.0 (11 poziom interfejsu API) lub nowszych należy użyć tej metody:

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader` Gwarantuje, że wszystkie operacje kursora są wykonywane w wątku w tle i może inteligentnie ponownego użycia istniejącej kursora w wystąpieniach działania podczas działania ponownego uruchamiania (np. z powodu zmian w konfiguracji) zamiast który ponownie załaduj ponownie dane.

Umożliwia także wcześniejszych wersji systemu Android `CursorLoader` przy użyciu [bibliotek obsługi v4](http://developer.android.com/tools/support-library/index.html).



## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>Wyświetlanie danych kursora z niestandardowego adaptera

Aby wyświetlić obraz kontaktu użyjemy niestandardowe karty, tak, aby można rozpoznać ręcznie `PhotoId` odwołanie do ścieżki pliku obrazu.

Aby wyświetlić dane z niestandardowego adaptera, w przykładzie użyto `CursorLoader` można pobrać wszystkich danych kontaktu w kolekcji lokalnej w **FillContacts** metody z **ContactsProvider/ContactsAdapter.cs**:

```csharp
void FillContacts ()
{
   var uri = ContactsContract.Contacts.ContentUri;
   string[] projection = {
       ContactsContract.Contacts.InterfaceConsts.Id,
       ContactsContract.Contacts.InterfaceConsts.DisplayName,
       ContactsContract.Contacts.InterfaceConsts.PhotoId
  };
   // CursorLoader introduced in Honeycomb (3.0, API11)
   var loader = new CursorLoader(activity, uri, projection, null, null, null);
   var cursor = (ICursor)loader.LoadInBackground();
   contactList = new List<Contact> ();
   if (cursor.MoveToFirst ()) {
      do {
          contactList.Add (new Contact{
              Id = cursor.GetLong (cursor.GetColumnIndex (projection [0])),
              DisplayName = cursor.GetString (cursor.GetColumnIndex (projection [1])),
              PhotoId = cursor.GetString (cursor.GetColumnIndex (projection [2]))
          });
       } while (cursor.MoveToNext());
   }
}
```

Następnie wdrożyć przy użyciu metody BaseAdapter `contactList` kolekcji. Adapter jest zaimplementowany tak samo, jak możesz ją z innej kolekcji &ndash; istnieje w tym miejscu nie specjalnej obsługi, ponieważ dane są uzyskiwane z `ContentProvider`:

```csharp
Activity activity;
public ContactsAdapter (Activity activity)
{
   this.activity = activity;
   FillContacts ();
}
public override int Count {
   get { return contactList.Count; }
}
public override Java.Lang.Object GetItem (int position)
{
  return null; // could wrap a Contact in a Java.Lang.Object to return it here if needed
}
public override long GetItemId (int position)
{
   return contactList [position].Id;
}
public override View GetView (int position, View convertView, ViewGroup parent)
{
   var view = convertView ?? activity.LayoutInflater.Inflate (Resource.Layout.ContactListItem, parent, false);
   var contactName = view.FindViewById<TextView> (Resource.Id.ContactName);
   var contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
   contactName.Text = contactList [position].DisplayName;
   if (contactList [position].PhotoId == null) {
       contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
       contactImage.SetImageResource (Resource.Drawable.ContactImage);
   } else {
       var contactUri = ContentUris.WithAppendedId (ContactsContract.Contacts.ContentUri, contactList [position].Id);
       var contactPhotoUri = Android.Net.Uri.WithAppendedPath (contactUri, Contacts.Photos.ContentDirectory);
       contactImage.SetImageURI (contactPhotoUri);
   }
   return view;
}
```

Obraz jest wyświetlany (jeśli istnieje) za pomocą identyfikatora Uri do pliku obrazu na urządzeniu. Aplikacja wygląda następująco:

[![Zrzut ekranu aplikacji wyświetlanie kontaktów w elemencie ListView; Obraz jest wyświetlany na lewo od jednego wpisu](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

Przy użyciu tego samego typu kodu, aplikacja mają dostęp do różnych typów danych systemu, w tym zdjęć, wideo i muzyka przez użytkownika.
Niektóre typy danych są wymagane specjalne uprawnienia wymagane do projektu **AndroidManifest.xml**.



## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>Wyświetlanie danych kursora z SimpleCursorAdapter

Kursor można również wyświetlona z `SimpleCursorAdapter` (mimo że będą wyświetlane tylko nazwę, nie fotografii). Ten kod przedstawia sposób użycia `ContentProvider` z `SimpleCursorAdapter` (ten kod nie jest wyświetlany w próbce):

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName
};
var loader = new CursorLoader (this, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
var fromColumns = new string[] {ContactsContract.Contacts.InterfaceConsts.DisplayName};
var toControlIds = new int[] {Android.Resource.Id.Text1};
adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlsIds);
listView.Adapter = adapter;
```

Zapoznaj się [widokach listy i karty](~/android/user-interface/layouts/list-view/index.md) więcej informacji na temat implementowania `SimpleCursorAdapter`.


## <a name="related-links"></a>Linki pokrewne

- [Wersja demonstracyjna ContactsAdapter (przykład)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
