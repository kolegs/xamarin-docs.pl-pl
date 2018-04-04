---
title: Tworzenie niestandardowych ContentProvider
description: Poprzedniej sekcji przedstawiono sposób wykorzystywać dane z wbudowanych implementacji ContentProvider. W tej sekcji objaśnia sposób tworzenia niestandardowych ContentProvider i korzystać z jego dane.
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: 95d38fae1614bbb12ddafaeca60d50e63404ea95
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-custom-contentprovider"></a>Tworzenie niestandardowych ContentProvider

_Poprzedniej sekcji przedstawiono sposób wykorzystywać dane z wbudowanych implementacji ContentProvider. W tej sekcji objaśnia sposób tworzenia niestandardowych ContentProvider i korzystać z jego dane._

## <a name="about-contentproviders"></a>O ContentProviders

Klasa dostawcy zawartości musi dziedziczyć z `ContentProvider`. Powinien zawierać wewnętrznego magazynu danych używanego w taki sposób, aby odpowiadały na zapytania i jego powinny ujawniać identyfikatorów URI i typami MIME ponieważ stałe, aby ułatwić korzystanie z kodu prawidłowych żądań kierowanych do danych.

### <a name="uri-authority"></a>Identyfikator URI (urzędu)

`ContentProviders` są dostępne w Android przy użyciu identyfikatora Uri. Aplikacja, która udostępnia `ContentProvider` Ustawia identyfikator URI, który będzie odpowiadał na w jego **AndroidManifest.xml** pliku. Po zainstalowaniu aplikacji te identyfikatory URI są zarejestrowane, tak aby inne aplikacje mogą uzyskiwać do nich dostęp.

Mono dla systemu Android, klasa dostawcy zawartości powinno mieć `[ContentProvider]` atrybutu, aby określić identyfikator Uri (lub identyfikatorów URI) powinny zostać dodane do **AndroidManifest.xml**.


### <a name="mime-type"></a>Typ MIME

Typowy format dla typów MIME składa się z dwóch części. Android `ContentProviders` często używanych te dwa ciągi dla pierwszej części typu MIME:

1. `vnd.android.cursor.item` &ndash; Aby przedstawić pojedynczy wiersz, należy użyć `ContentResolver.CursorItemBaseType` stałej w kodzie.

1. `vnd.android.cursor.dir` &ndash; wiele wierszy, można użyć `ContentResolver.CursorDirBaseType` stałej w kodzie.

Druga część typ MIME jest specyficzne dla aplikacji i powinien użyć standardowego wstecznego DNS z `vnd.` prefiks. Przykładowy kod używa `vnd.com.xamarin.sample.Vegetables`.


### <a name="data-model-metadata"></a>Metadane modelu danych

Aplikacje odbierającą muszą tworzyć zapytania identyfikatora Uri do różnych typów danych. Podstawowy identyfikator Uri, można rozszerzyć, aby odwoływać się do konkretnej tabeli danych i mogą również obejmować parametry do filtrowania wyników. Kolumny i klauzule używanych z wynikowy kursora do wyświetlania danych, również musi być zadeklarowany.

Aby upewnić się, czy tylko prawidłowy identyfikator Uri zapytania są wykonane, jest zwyczajowe, aby zapewnić prawidłowe ciągi jako wartości stałych. To ułatwia dostępu `ContentProvider` ponieważ sprawia, że wartości wykrywalny przez uzupełnianie kodu i uniemożliwia błędów pisowni w parametrach.

W poprzednim przykładzie `android.provider.ContactsContract` klasy widoczne metadanych dla danych kontaktów. Dla naszych niestandardowe `ContentProvider` uwidaczniamy tylko stałe w samej klasy.


## <a name="implementation"></a>Implementacja

Istnieją trzy kroki do tworzenia i używania niestandardowego `ContentProvider`:

1. **Utwórz klasę bazy danych** &ndash; wdrożenie `SQLiteOpenHelper`.

2. **Utwórz `ContentProvider` klasy** &ndash; wdrożenie `ContentProvider` przy użyciu wystąpienia bazy danych metadanych udostępniony jako metody dostępu do danych i wartości stałych.

3. **Dostęp `ContentProvider` za pomocą jego identyfikatora Uri** &ndash; wypełnij `CursorAdapter` przy użyciu `ContentProvider`, dostępu za pomocą jego identyfikatora Uri.

Jak wcześniej wspomniano, `ContentProviders` mogą być używane z aplikacji innych niż gdzie są zdefiniowane. W tym przykładzie dane jest używane w tej samej aplikacji, ale należy pamiętać, że inne aplikacje także dostęp do niego tak długo, jak wiedzieli, identyfikator Uri i informacji o schemacie (która jest zwykle widoczne jako wartości stałych).


## <a name="create-a-database"></a>Tworzenie bazy danych

Większość `ContentProvider` implementacje będą oparte na `SQLite` bazy danych. Przykładowy kod bazy danych w **SimpleContentProvider/VegetableDatabase.cs** tworzy bardzo proste dwie kolumny bazy danych, jak pokazano:

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
  const string create_table_sql =
    "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
  const string DatabaseName = "vegetables.db";
  const int DatabaseVersion = 1;

  public VegetableDatabase(Context context) : base(context, DatabaseName, null, DatabaseVersion) { }
  public override void OnCreate(SQLiteDatabase db)
  {
    db.ExecSQL(create_table_sql);
    // seed with data
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Vegetables')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Fruits')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Flower Buds')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Legumes')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Bulbs')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Tubers')");
  }
  public override void OnUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
  {
    throw new NotImplementedException();
  }
}
```

Implementacja baza danych sam nie wymaga uwagi użytkownika mają być uwidaczniane z `ContentProvider`, jednak jeśli chcesz powiązać `ContentProvider's` danych `ListView` następnie kontrolować całkowitą unikatowe kolumny o nazwie `_id` musi być częścią zestaw wyników. Zobacz [widokach listy i karty](~/android/user-interface/layouts/list-view/index.md) dokumentu, aby uzyskać więcej informacji na temat używania `ListView` formantu.


## <a name="create-the-contentprovider"></a>Utwórz ContentProvider

Pozostałej części tej sekcji zwraca instrukcje krok po kroku dotyczące sposobu **SimpleContentProvider/VegetableProvider.cs** klasa przykład został skompilowany.


### <a name="initialize-the-database"></a>Inicjowanie bazy danych

Pierwszym krokiem jest podklasą `ContentProvider` i Dodaj bazy danych, które będzie używane.

```csharp
public class VegetableProvider : ContentProvider 
{
    VegetableDatabase vegeDB;
    public override bool OnCreate()
    {
       vegeDB = new VegetableDatabase(Context);
        return true;
    }
}
```

Pozostałe kod będzie stanowić implementację rzeczywiste dostawcy zawartości, która pozwala na wykrycie i zbadać danych.



## <a name="add-metadata-for-consumers"></a>Dodawanie metadanych dla konsumentów

Istnieją cztery typy metadane zamierzamy ujawnia na `ContentProvider` klasy. Uprawnień jest wymagane, tylko pozostałe są wykonywane przez Konwencję.

- **Urząd** &ndash; `ContentProvider` atrybutu *musi* dodawana do klasy, dzięki czemu jest on zarejestrowany na Android, gdy aplikacja jest zainstalowana.

- **Identyfikator URI** &ndash; `CONTENT_URI` widoczne jako stała, dzięki czemu jest łatwy w użyciu w kodzie. Powinien on zgodne urząd, ale obejmują schemat i ścieżki podstawowej.

- **Typy MIME** &ndash; listy wyników i pojedyncze wyniki są traktowane jako różne typy zawartości, aby zdefiniować dwa typy MIME do reprezentowania je.

- **InterfaceConsts** &ndash; Podaj wartości stałej dla każdej nazwy kolumny danych, aby wykorzystywanie kodu można łatwo odnaleźć i odwołać się do nich bez ryzyka błędy typograficzne.

Ten kod pokazuje, jak każdy z tych elementów jest zaimplementowana, dodanie do definicji bazy danych z poprzedniego kroku:

```csharp
[ContentProvider(new string[] { CursorTableAdapter.VegetableProvider.AUTHORITY })]
public class VegetableProvider : ContentProvider 
{
   public const string AUTHORITY = "com.xamarin.sample.VegetableProvider";
   static string BASE_PATH = "vegetables";
   public static readonly Android.Net.Uri CONTENT_URI = Android.Net.Uri.Parse("content://" + AUTHORITY + "/" + BASE_PATH);
   // MIME types used for getting a list, or a single vegetable
   public const string VEGETABLES_MIME_TYPE = ContentResolver.CursorDirBaseType + "/vnd.com.xamarin.sample.Vegetables";
   public const string VEGETABLE_MIME_TYPE = ContentResolver.CursorItemBaseType + "/vnd.com.xamarin.sample.Vegetables";
   // Column names
   public static class InterfaceConsts {
       public const string Id = "_id";
       public const string Name = "name";
   }
   VegetableDatabase vegeDB;
   public override bool OnCreate()
   {
       vegeDB = new VegetableDatabase(Context);
       return true;
   }
}
```


## <a name="implement-the-uri-parsing-helper"></a>Implementowanie URI analizowania pomocnika

Ponieważ zużywa kod używa identyfikatorów URI do wysyłania żądań do `ContentProvider`, potrzebujemy móc analizować tych żądań w celu ustalenia, jakie dane do zwrócenia. `UriMatcher` Klasy może pomóc w celu przeprowadzenia analizy identyfikatorów URI, gdy została zainicjowana z identyfikatora Uri wzorce `ContentProvider` obsługuje.

`UriMatcher` w przykładzie zostanie zainicjowana dwa identyfikatory URI:

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; żądania do zwrócenia z pełną listą warzyw.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; gdzie \# jest symbolem zastępczym dla parametru liczbowego ( `_id` wiersza w bazie danych). Symbol zastępczy gwiazdki ("\*") może również służyć do pasuje do parametru tekstu.

W kodzie używamy stałe, aby odwołać się do wartości metadanych, takich jak urząd i BASE\_ścieżki. Kody powrotne będzie używany w metodach, które wykonują analizy w celu ustalenia, jakie dane do zwracania identyfikatora Uri.

```csharp
const int GET_ALL = 0; // return code when list of Vegetables requested
const int GET_ONE = 1; // return code when a single Vegetable is requested by ID
static UriMatcher uriMatcher = BuildUriMatcher();
static UriMatcher BuildUriMatcher()
{
  var matcher = new UriMatcher(UriMatcher.NoMatch);
  // Uris to match, and the code to return when matched
  matcher.AddURI(AUTHORITY, BASE_PATH, GET_ALL); // all vegetables
  matcher.AddURI(AUTHORITY, BASE_PATH + "/#", GET_ONE); // specific vegetable by numeric ID
  return matcher;
}
```

Ten kod jest prywatnym wszystkie `ContentProvider` klasy. Zapoznaj się [dokumentacji UriMatcher firmy Google](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/) Aby uzyskać więcej informacji.


## <a name="implement-the-querymethod"></a>Implementowanie QueryMethod

Najprostszą `ContentProvider` metodą wykonania jest `Query` metody. Implementacja poniżej używa `UriMatcher` przeanalizować `uri` parametr i wywołaj metodę poprawną bazą danych. Jeśli `uri` zawiera parametr ID, a następnie liczb całkowitych jest analizowana (przy użyciu `LastPathSegment`) i używanych w zapytaniu bazy danych.

```csharp
public override Android.Database.ICursor Query(Android.Net.Uri uri, string[] projection, string selection, string[] selectionArgs, string sortOrder)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return GetFromDatabase();
  case GET_ONE:
    var id = uri.LastPathSegment;
    return GetFromDatabase(id); // the ID is the last part of the Uri
  default:
    throw new Java.Lang.IllegalArgumentException("Unknown Uri: " + uri);
  }
}
Android.Database.ICursor GetFromDatabase()
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables", null);
}
Android.Database.ICursor GetFromDatabase(string id)
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables WHERE _id = " + id, null);
}
```

`GetType` Metoda musi zostać zastąpiona. Tę metodę można wywołać w celu określenia typu zawartości, który zostanie zwrócony dla danego identyfikatora Uri.
To może stwierdzić aplikacja odbierająca komunikaty sposób obsługi tych danych.

```csharp
public override String GetType(Android.Net.Uri uri)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return VEGETABLES_MIME_TYPE; // list
  case GET_ONE:
    return VEGETABLE_MIME_TYPE; // single item
  default:
    throw new Java.Lang.IllegalArgumentExceptoin ("Unknown Uri: " + uri);
   }
}
```


## <a name="implement-the-other-overrides"></a>Implementowanie innymi zastąpieniami

Nasze prosty przykład nie zezwala na edytowanie lub usuwanie danych, ale metody Insert, Update i Delete musi być implementowana tak, dodaj je bez implementacji:

```csharp
public override int Delete(Android.Net.Uri uri, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override Android.Net.Uri Insert(Android.Net.Uri uri, ContentValues values)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override int Update(Android.Net.Uri uri, ContentValues values, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
```

Na tym kończy się podstawowe `ContentProvider` implementacji. Po zainstalowaniu aplikacji ujawnia on dane będą dostępne zarówno wewnątrz aplikacji, ale także do innych aplikacji, który zna identyfikator Uri do utworzenia odwołania.


## <a name="access-the-contentprovider"></a>Dostęp ContentProvider

Raz `VegetableProvider` został zaimplementowany, dostępu do niego odbywa się tak samo, jak dostawca kontaktów na początku tego dokumentu: uzyskać kursora przy użyciu określonego identyfikatora Uri, a następnie użyj karty do uzyskiwania dostępu do danych.


## <a name="bind-a-listview-to-a-contentprovider"></a>Powiązać ContentProvider ListView

Aby wypełnić `ListView` z danymi używamy identyfikator Uri, który odpowiada liście niefiltrowane warzyw. W kodzie używamy wartości stałej `VegetableProvider.CONTENT_URI`, który wiemy jest rozpoznawana jako `com.xamarin.sample.vegetableprovider/vegetables`. Nasze `VegetableProvider.Query` implementacja zwraca kursor, który można następnie powiązać `ListView`.

Kod w `SimpleContentProvider/HomeScreen.cs` pokazuje, jak łatwo jest wyświetlane dane z `ContentProvider`:

```csharp
listView = FindViewById<ListView>(Resource.Id.List);
string[] projection = new string[] { VegetableProvider.InterfaceConsts.Id, VegetableProvider.InterfaceConsts.Name} ;
string[] fromColumns = new string[] { VegetableProvider.InterfaceConsts.Name };
int[] toControlIds = new int[] { Android.Resource.Id.Text1 };

// CursorLoader introduced in Honeycomb (3.0, API_11)
var loader = new CursorLoader(this,
   VegetableProvider.CONTENT_URI, projection, null, null, null);
cursor = (ICursor)loader.LoadInBackground();

// Create a SimpleCursorAdapter
adapter = new SimpleCursorAdapter(this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlIds);
listView.Adapter = adapter;
```

Wynikowa aplikacji wygląda następująco:

[![Zrzut ekranu przedstawiający listę warzywa, owoców, pąków, roślin strączkowych bulw, bulw aplikacji](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)



## <a name="retrieve-a-single-item-from-a-contentprovider"></a>Pobieranie pojedynczego elementu z ContentProvider

Aplikacja odbierająca komunikaty może być również konieczne dostęp do jednego wiersze danych, można to zrobić, tworząc inny identyfikator Uri, który odwołuje się do określonego wiersza (na przykład).

Użyj `ContentResolver` bezpośrednio uzyskać dostęp do pojedynczego elementu przez utworzenie identyfikatora Uri z wymaganymi `Id`.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

Metoda wygląda następująco:

```csharp
protected void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
  var id = e.Id;
  string[] projection = new string[] { "name" };
  var uri = Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
  ICursor vegeCursor = ContentResolver.Query(uri, projection, null, new string[] { id.ToString() }, null);
  string text = "";
  if (vegeCursor.MoveToFirst()) {
    text = vegeCursor.GetInt(0) + " " + vegeCursor.GetString(1);
    Android.Widget.Toast.MakeText(this, text, Android.Widget.ToastLength.Short).Show();
  }
  vegeCursor.Close();
}
```


## <a name="related-links"></a>Linki pokrewne

- [SimpleContentProvider (przykład)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
