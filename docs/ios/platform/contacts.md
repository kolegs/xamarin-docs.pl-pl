---
title: Kontakty i ContactsUI
description: W tym artykule omówiono, Praca z nowych kontaktów i interfejsu użytkownika kontakty platform w aplikacji platformy Xamarin.iOS. Te struktury Zastąp istniejące książki adresowej i interfejsu użytkownika książki adresowej używane w poprzednich wersjach systemu IOS.
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4d963bbefce2b4564c3f352be5768df77b45b34d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="contacts-and-contactsui"></a>Kontakty i ContactsUI

_W tym artykule omówiono, Praca z nowych kontaktów i interfejsu użytkownika kontakty platform w aplikacji platformy Xamarin.iOS. Te struktury Zastąp istniejące książki adresowej i interfejsu użytkownika książki adresowej używane w poprzednich wersjach systemu IOS._

Wraz z wprowadzeniem systemu iOS 9, Apple wydała dwóch nowych struktur, `Contacts` i `ContactsUI`, książka adresowa istniejących tego Zamień i interfejsu użytkownika książki adresowej struktury używane przez system iOS 8 i wcześniejszych.

Dwa nowe struktury zawiera następujące funkcje:

- [**Kontakty** ](#contacts) — zapewnia dostęp do danych listy kontaktów użytkownika.
    Ponieważ większość aplikacji wymaga tylko dostęp tylko do odczytu, platforma został zoptymalizowany dla wątku dostępu bezpiecznej, tylko do odczytu.

- [**ContactsUI** ](#contactsui) — udostępnia interfejs użytkownika platformy Xamarin.iOS elementów do wyświetlenia, edytować, wybierz i tworzenie kontaktów na urządzeniach z systemem iOS.

[![](contacts-images/add01.png "Przykład arkusza skontaktuj się z urządzenia z systemem iOS")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> Istniejące `AddressBook` i `AddressBookUI` struktury używane przez system iOS 8 (przed) są przestarzałe w systemie iOS 9 i powinna zostać zastąpiona nową `Contacts` i `ContactsUI` struktury możliwie jak dla wszystkich istniejących aplikacji platformy Xamarin.iOS. Nowe aplikacje mają być zapisywane względem nowych struktur.




W poniższych sekcjach firma Microsoft będzie Spójrz na tych nowych struktur i sposobu wdrażania ich w aplikacji platformy Xamarin.iOS.

<a name="contacts" />

## <a name="the-contacts-framework"></a>Framework kontaktów

Framework kontaktów udostępnia Xamarin.iOS informacje kontaktowe użytkownika. Ponieważ większość aplikacji wymaga tylko dostęp tylko do odczytu, platforma został zoptymalizowany dla wątku dostępu bezpiecznej, tylko do odczytu.

<a name="Contact_Objects" />

### <a name="contact-objects"></a>Skontaktuj się z pomocą obiektów

`CNContact` Klasy zapewnia wątku bezpieczne, tylko do odczytu dostęp do właściwości kontaktu, takie jak nazwisko, adres lub numerów telefonów. `CNContact` funkcji, takich jak `NSDictionary` i zawiera wiele kolekcji tylko do odczytu właściwości (takie jak adresy i numery telefonów):

[![](contacts-images/contactobjects.png "Skontaktuj się z obiektu ― omówienie")](contacts-images/contactobjects.png#lightbox)

Dla właściwości, która może mieć wielu wartości (takie jak wiadomości e-mail adres lub numerów telefonu), będą one reprezentowane jako tablica `NSLabeledValue` obiektów. `NSLabeledValue` jest krotka awaryjny wątku składającą się tylko do odczytu zestawu etykiety i wartości, których etykiety definiuje wartość dla użytkownika (na przykład e-mail domu lub pracy). Framework kontaktów zawiera szereg wstępnie zdefiniowanych etykiety (za pośrednictwem `CNLabelKey` i `CNLabelPhoneNumberKey` klasy statyczne) można używać w aplikacji lub istnieje możliwość definiowania niestandardowych etykiet do własnych potrzeb.

Dla żadnych aplikacji platformy Xamarin.iOS, które należy dopasować wartości istniejącego (lub utworzyć nowe), użyj `NSMutableContact` wersji klas i jej klas podrzędnych (takich jak `CNMutablePostalAddress`).

Na przykład kod wykonaj Utwórz nowy kontakt i dodać go do kolekcji użytkowników, kontaktów:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Add email addresses
var homeEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@mac.com"));
var workEmail = new CNLabeledValue<NSString>(CNLabelKey.Work, new NSString("john.appleseed@apple.com"));
contact.EmailAddresses = new CNLabeledValue<NSString>[] { homeEmail, workEmail };

// Add phone numbers
var cellPhone = new CNLabeledValue<CNPhoneNumber>(CNLabelPhoneNumberKey.iPhone, new CNPhoneNumber("713-555-1212"));
var workPhone = new CNLabeledValue<CNPhoneNumber>("Work", new CNPhoneNumber("408-555-1212"));
contact.PhoneNumbers = new CNLabeledValue<CNPhoneNumber>[] { cellPhone, workPhone };

// Add work address
var workAddress = new CNMutablePostalAddress()
{
    Street = "1 Infinite Loop",
    City = "Cupertino",
    State = "CA",
    PostalCode = "95014"
};
contact.PostalAddresses = new CNLabeledValue<CNPostalAddress>[] { new CNLabeledValue<CNPostalAddress>(CNLabelKey.Work, workAddress) };

// Add birthday
var birthday = new NSDateComponents()
{
    Day = 1,
    Month = 4,
    Year = 1984
};
contact.Birthday = birthday;

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

// Attempt to save changes
NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error))
{
    Console.WriteLine("New contact saved");
}
else
{
    Console.WriteLine("Save error: {0}", error);
}
```

Jeśli ten kod jest uruchamiana na urządzeniu z systemem iOS 9, nowy kontakt zostanie dodany do kolekcji użytkowników. Na przykład:

[![](contacts-images/add01.png "Nowy kontakt dodany do kolekcji użytkowników")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>Skontaktuj się z pomocą formatowanie i lokalizacja

Framework kontaktów zawiera niektóre obiekty i metody, które mogą pomóc Ci sformatowanie i zlokalizować zawartość wyświetlaną dla użytkownika. Na przykład następujący kod będzie poprawnie format nazwy kontaktów i adres do wyświetlenia:

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

Właściwości etykiet, które będą wyświetlane w Interfejsie użytkownika aplikacji framework skontaktuj się z ma metody lokalizowania także te ciągi. Ponownie jest oparta na urządzenia z systemem iOS, które aplikacja jest uruchamiana na bieżące ustawienia regionalne. Na przykład:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOption.Nickname));
Console.WriteLine(CNLabeledValue.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>Pobieranie istniejących kontaktów

Przy użyciu wystąpienia `CNContactStore` klasy, można pobrać informacji kontaktowych z bazy danych z kontaktów użytkownika. `CNContactStore` Zawiera wszystkie metody, wymagane do pobierania lub aktualizowania kontaktów i grup z bazy danych. Ponieważ te metody są synchroniczne, zaleca się uruchomić je w wątku tła, aby zapobiec blokuje interfejsu użytkownika.

Za pomocą predykatów (zbudowane na podstawie `CNContact` klasy), można filtrować wyniki zwracane podczas pobierania kontakty z bazy danych. Można pobrać tylko kontakty, które zawierają ciąg `Appleseed`, użyj następującego kodu:

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> Rodzajowa i złożone predykaty nie są obsługiwane przez platformę kontaktów.

Na przykład, aby ograniczyć pobieranie tylko **GivenName** i **FamilyName** właściwości kontaktu, użyj następującego kodu:

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

Na koniec można wyszukiwać bazy danych i zwracają wyniki, należy użyć poniższego kodu:

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

Jeśli ten kod został uruchomiony po próbki utworzonej w **obiektu Kontakty** powyższej sekcji go zwróci kontaktu "Jan Appleseed", który właśnie utworzyliśmy.

### <a name="contact-access-privacy"></a>Skontaktuj się z pomocą dostępu prywatności

Ponieważ użytkownicy końcowi można udzielić lub odmówić dostępu do informacje kontaktowe dla poszczególnych aplikacji, po raz pierwszy należy wywoływania `CNContactStore`, okno dialogowe zostanie wyświetlone z pytaniem, aby umożliwić dostęp do aplikacji.

Żądanie uprawnienia będzie być podane tylko raz, aplikacja jest uruchamiania i kolejne po raz pierwszy uruchamia lub wywołań `CNContactStore` użyje uprawnień, którą użytkownik wybrał w tym czasie.

Aplikację należy zaprojektować tak, aby bezpiecznie obsługuje użytkownika zezwalających na dostęp do swoich kontaktów bazy danych.

#### <a name="fetching-partial-contacts"></a>Pobieranie z częściowa kontaktów

A _skontaktuj się z częściowa_ jest kontakt tylko niektóre z dostępnych właściwości pobrano z kontaktu w sklepie. Próba dostępu do właściwości, która nie została wcześniej dołączone spowoduje Wystąpił wyjątek.

Łatwo można sprawdzić, czy dany kontakt ma żądanej właściwości przy użyciu `IsKeyAvailable` lub `AreKeysAvailable` metody `CNContact` wystąpienia. Na przykład:

```csharp
// Does the contact contain the requested key?
if (!contact.IsKeyAvailable(CNContactOption.PostalAddresses)) {
    // No, re-request to pull required info
    var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName, CNContactKey.PostalAddresses};
    var store = new CNContactStore();
    NSError error;
    contact = store.GetUnifiedContact(contact.Identifier, fetchKeys, out error);
}
```

> [!IMPORTANT]
> `GetUnifiedContact` i `GetUnifiedContacts` metody `CNContactStore` klasy _tylko_ zwracać skontaktuj się z częściowa ograniczone do właściwości zażąda podane klucze pobierania.

### <a name="unified-contacts"></a>Ujednolicone kontaktów

Użytkownik może mieć różnych źródeł informacji kontaktowych dla pojedynczej osoby w swoich kontaktów bazy danych (takiej jak iCloud, Facebook lub poczty Google). IOS i OS X aplikacje tych informacji kontaktowych zostanie automatycznie połączone ze sobą i wyświetlane użytkownikowi jako pojedyncza, _Unified skontaktuj się z_:

[![](contacts-images/unified01.png "Ujednolicone Kontakty — omówienie")](contacts-images/unified01.png#lightbox)

Kontaktu Unified jest tymczasowy, w pamięci widok informacji kontaktowych łącza, które będzie mieć własny unikatowy identyfikator (które mają być używane do kontaktu refetch, jeśli jest to wymagane). Domyślnie framework kontaktów zwróci Unified kontaktu, jeśli to możliwe.

### <a name="creating-and-updating-contacts"></a>Tworzenie i aktualizowanie kontaktów

Jak widzieliśmy w [skontaktuj się z pomocą obiektów](#Contact_Objects) powyższej sekcji, użyj `CNContactStore` i wystąpienie `CNMutableContact` do tworzenia nowych kontaktów, które następnie są zapisywane na użytkownika skontaktuj się z bazą danych przy użyciu `CNSaveRequest`:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("New contact saved");
} else {
    Console.WriteLine("Save error: {0}", error);
}
```

A `CNSaveRequest` może również służyć do pamięci podręcznej wielu zmian kontaktów i grup w ramach jednej operacji i partii te modyfikacje `CNContactStore`.

Aby zaktualizować niemodyfikowalnym kontaktu uzyskane z operacji pobrania, możesz poprosić modyfikowalną kopii, która następnie zmodyfikuj i Zapisz do magazynu kontaktów. Na przykład:

```csharp
// Get mutable copy of contact
var mutable = contact.MutableCopy() as CNMutableContact;
var newEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@xamarin.com"));

// Append new email
var emails = new NSObject[mutable.EmailAddresses.Length+1];
mutable.EmailAddresses.CopyTo(emails,0);
emails[mutable.EmailAddresses.Length+1] = newEmail;
mutable.EmailAddresses = emails;

// Update contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.UpdateContact(mutable);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("Contact updated.");
} else {
    Console.WriteLine("Update error: {0}", error);
}
```

### <a name="contact-change-notifications"></a>Powiadomienia o zmianie kontaktów

Modyfikacjach kontaktu, skontaktuj się z magazynu ogłoszeń `CNContactStoreDidChangeNotification` do domyślnego centrum powiadomień. Jeśli mają być buforowane lub aktualnie są wyświetlane wszystkie kontakty, należy odświeżyć te obiekty z magazynu skontaktuj się z pomocą (`CNContactStore`).

### <a name="containers-and-groups"></a>Kontenery i grup

Kontaktów użytkownika może istnieć albo lokalnie na urządzeniu użytkownika jako kontakty synchronizowane z urządzeniem z co najmniej jednego konta serwera (takich jak Facebook lub Google). Każda pula kontaktów ma własną _kontenera_ i danego kontaktu może istnieć tylko w jeden kontener.

[![](contacts-images/containers01.png "Kontenery i grupy — omówienie")](contacts-images/containers01.png#lightbox)

Niektóre kontenery umożliwiają dla kontaktów, które mają być ułożone w co najmniej jeden _grup_ lub _podgrupy_. To zachowanie jest zależna od magazynu zapasowego dla danego kontenera. Na przykład iCloud ma tylko jeden kontener, ale może mieć wiele grup (ale nie podgrupy). Microsoft Exchange z drugiej strony, nie obsługuje grup, ale może mieć wiele kontenerów (po jednej dla każdego folderu programu Exchange).

[![](contacts-images/containers02.png "Nakłada kontenery i grup")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>W ramach ContactsUI

W sytuacjach, w którym aplikacja musi przedstawiać niestandardowego interfejsu użytkownika ContactsUI framework służy do prezentowania elementy interfejsu użytkownika do wyświetlania, edytowania, wybierz i utworzyć kontakty w aplikacji platformy Xamarin.iOS.

Za pomocą formantów wbudowanych firmy Apple nie tylko zmniejszyć ilość kodu, który należy utworzyć na kontakt z pomocą techniczną w Twojej aplikacji platformy Xamarin.iOS, ale istnieje spójny interfejs dla użytkowników aplikacji.

### <a name="the-contact-picker-view-controller"></a>Skontaktuj się z pomocą selektora kontroler widoku

Skontaktuj się z pomocą selektora widoku kontrolera (`CNContactPickerViewController`) zarządza standardowy widok skontaktuj się z selektora umożliwia użytkownikowi wybranie kontaktu lub skontaktuj się z właściwości z bazy danych z kontaktów użytkownika. Użytkownik może wybrać co najmniej jeden kontaktu (na podstawie jego użycia) i skontaktuj się z pomocą selektora widoku kontrolera nie jest wyświetlany monit o zgodę przed wyświetleniem selektora.

Przed wywołaniem `CNContactPickerViewController` klasy, należy zdefiniować właściwości, które użytkownik może wybrać i zdefiniuj predykaty kontrolować wyświetlania i wybranych właściwości skontaktuj się z.

Użyj wystąpienia klasy, która dziedziczy `CNContactPickerDelegate` odpowiedzieć na interakcję użytkownika z selektora. Na przykład:

```csharp
using System;
using System.Linq;
using UIKit;
using Foundation;
using Contacts;
using ContactsUI;

namespace iOS9Contacts
{
    public class ContactPickerDelegate: CNContactPickerDelegate
    {
        #region Constructors
        public ContactPickerDelegate ()
        {
        }

        public ContactPickerDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ContactPickerDidCancel (CNContactPickerViewController picker)
        {
            Console.WriteLine ("User canceled picker");

        }

        public override void DidSelectContact (CNContactPickerViewController picker, CNContact contact)
        {
            Console.WriteLine ("Selected: {0}", contact);
        }

        public override void DidSelectContactProperty (CNContactPickerViewController picker, CNContactProperty contactProperty)
        {
            Console.WriteLine ("Selected Property: {0}", contactProperty);
        }
        #endregion
    }
}
```

Aby zezwolić użytkownikowi na wybranie adres e-mail kontaktów w ich bazy danych, można użyć poniższego kodu:

```csharp
// Create a new picker
var picker = new CNContactPickerViewController();

// Select property to pick
picker.DisplayedPropertyKeys = new NSString[] {CNContactKey.EmailAddresses};
picker.PredicateForEnablingContact = NSPredicate.FromFormat("emailAddresses.@count > 0");
picker.PredicateForSelectionOfContact = NSPredicate.FromFormat("emailAddresses.@count == 1");

// Respond to selection
picker.Delegate = new ContactPickerDelegate();

// Display picker
PresentViewController(picker,true,null);
```

### <a name="the-contact-view-controller"></a>Kontroler widoku kontaktów

Skontaktuj się z widoku kontrolera (`CNContactViewController`) Klasa udostępnia kontrolera do prezentowania standardowy widok kontaktu dla użytkownika końcowego. Widok kontaktu można wyświetlać nowych kontaktów nowy, nieznane lub istniejące i typ musi być określona, przed wyświetleniem widoku wywołując poprawne Konstruktor statyczny (`FromNewContact`, `FromUnknownContact`, `FromContact`). Na przykład:

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>Podsumowanie

W tym artykule trwało szczegółowe przyjrzeć się praca z platform kontaktów i skontaktuj się z interfejsu użytkownika w aplikacji platformy Xamarin.iOS. Po pierwsze objętych usługą różnych typów obiektów, które zapewnia platformę kontaktu i jak używać do tworzenia nowych lub istniejących kontakty. Uwzględniła framework skontaktuj się z interfejsu użytkownika, aby wybrać istniejące kontakty i wyświetlić informacje kontaktowe.


## <a name="related-links"></a>Linki pokrewne

- [Przykładowe QuickContacts](https://developer.xamarin.com/samples/monotouch/ios9/QuickContacts/)
- [Nowości w systemie iOS 9](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html#//apple_ref/doc/uid/TP40016198-DontLinkElementID_14))
- [Odwołanie do platformy kontaktów](https://developer.apple.com/library/prerelease/ios/documentation/Contacts/Reference/Contacts_Framework/index.html#//apple_ref/doc/uid/TP40015328)
- [Odwołanie ContactsUI Framework](https://developer.apple.com/library/prerelease/ios/documentation/ContactsUI/Reference/ContactsUI_Framework/index.html#//apple_ref/doc/uid/TP40016207)
