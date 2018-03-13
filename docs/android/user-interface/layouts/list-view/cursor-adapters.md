---
title: "Przy użyciu CursorAdapters"
ms.topic: article
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: 5cadaf5f41d940a0255113178d018b59b780eabc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="using-cursoradapters"></a>Przy użyciu CursorAdapters


## <a name="overview"></a>Omówienie

Android udostępnia klasy karty przeznaczony do wyświetlania danych z zapytania bazy danych SQLite:

 **SimpleCursorAdapter** — jest to podobne do `ArrayAdapter` ponieważ mogą zostać użyte bez podklasy. Po prostu zapewnić wymagane parametry (takie jak informacje o kursora i układu) w konstruktorze, a następnie przypisz do `ListView`.

 **CursorAdapter** — klasa podstawowa, która może dziedziczyć po wymagają więcej kontrolować powiązania danych wartości do formantów układu (na przykład ukrywanie/pokazywanie formantów lub zmieniać ich właściwości).

Kursor kart umożliwiają wysokiej wydajności przewijać długich list dane, które są przechowywane w SQLite. Kod odbierającą Zdefiniuj kwerendę SQL w `Cursor` obiektu i opisano, jak utworzyć i wypełnić widoków dla każdego wiersza.


## <a name="creating-an-sqlite-database"></a>Tworzenie bazy danych SQLite

Aby zademonstrować kart kursora wymaga prostych implementacji bazy danych SQLite. Kod w **SimpleCursorTableAdapter/VegetableDatabase.cs** zawiera kod i SQL, aby utworzyć tabelę i wypełnić ją niektóre dane.
Pełną `VegetableDatabase` klasy jest następujący:

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
   public static readonly string create_table_sql =
       "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
   public static readonly string DatabaseName = "vegetables.db";
   public static readonly int DatabaseVersion = 1;
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
   {   // not required until second version :)
       throw new NotImplementedException();
   }
}
```

`VegetableDatabase` w będzie można utworzyć wystąpienia klasy `OnCreate` metody `HomeScreen` działania. `SQLiteOpenHelper` Klasy podstawowej zarządza instalacji pliku bazy danych i upewnia się, że SQL w jego `OnCreate` metoda jest uruchamiana tylko raz. Ta klasa jest używana w poniższych dwóch przykładach dotyczących `SimpleCursorAdapter` i `CursorAdapter`.

Zapytanie kursora *musi* ma kolumna liczb całkowitych `_id` dla `CursorAdapter` do pracy. Jeśli tabela nie ma kolumna liczb całkowitych o nazwie `_id` następnie użyć aliasów kolumny inny unikatowy całkowitą `RawQuery` składa kursora. Zapoznaj się [Android docs](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/) Aby uzyskać więcej informacji.


### <a name="creating-the-cursor"></a>Utworzenie kursora

W przykładach użyto `RawQuery` włączyć kwerendę SQL w `Cursor` obiektu. Lista kolumn, która jest zwracana z kursora definiuje kolumny danych, które są dostępne do wyświetlenia na karcie kursora. Kod, który tworzy bazę danych w **SimpleCursorTableAdapter/HomeScreen.cs** `OnCreate` metody jest następujący:

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

Każdy kod wywołujący `StartManagingCursor` także wywołać `StopManagingCursor`. W przykładach użyto `OnCreate` można uruchomić, a `OnDestroy` zamknąć kursora. `OnDestroy` Metoda zawiera ten kod:

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

Gdy aplikacja ma bazy danych SQLite i został utworzony obiekt kursora, jak pokazano, jego wykorzystywać albo `SimpleCursorAdapter` lub podklasa klasy of `CusorAdapter` na wyświetlenie wierszy w `ListView`.


## <a name="using-simplecursoradapter"></a>Przy użyciu SimpleCursorAdapter

`SimpleCursorAdapter` przypomina `ArrayAdapter`, ale specjalne do użycia z bazy danych SQLite. Nie wymagają podklasy — wystarczy ustawić niektóre parametry proste podczas tworzenia obiektu i przypisz go do `ListView`w `Adapter` właściwości.

Parametry dla konstruktora SimpleCursorAdapter są:

 **Kontekst** — odwołanie do zawierającego działania.

 **Układ** — identyfikator zasobu widoku wiersza do użycia.

 **ICursor** — kursora zawierającego zapytanie SQLite dla danych do wyświetlenia.

 **Z** tablicy ciągów — tablica ciągów odpowiadających nazwy kolumn w kursorze.

 **Aby** całkowitą tablicy — tablicę identyfikatorów układu, które odnoszą się do formantów w układzie wierszy. Wartość kolumny określone w `from` tablicy zostanie powiązany ControlID określone w tej tablicy w tym samym indeksie.

`from` i `to` tablic musi mieć taką samą liczbę wpisów, ponieważ tworzą one mapowania ze źródła danych do kontrolek układu w widoku.

**SimpleCursorTableAdapter/HomeScreen.cs** przykładowy kod przewodów się `SimpleCursorAdapter` podobnie do następującej:

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` to szybki i prosty sposób wyświetlania danych SQLite w `ListView`. Ograniczenie głównego jest że można powiązać tylko wartości kolumn, aby wyświetlić formanty, nie zezwala na zmianę innych aspektów układu (na przykład przedstawiający/ukrywanie formantów lub zmiana właściwości).


## <a name="subclassing-cursoradapter"></a>Tworzenie podklas CursorAdapter

A `CursorAdapter` podklasy ma tego samego zwiększenia wydajności jako `SimpleCursorAdapter` na wyświetlanie danych z bazy danych SQLite, ale również umożliwia pełną kontrolę nad tworzeniem i układu widoku każdego wiersza. `CursorAdapter` Implementacji bardzo różni się od podklasy `BaseAdapter` , ponieważ nie zastępuje on `GetView`, `GetItemId`, `Count` lub `this[]` indeksatora.

Podane działającego SQLite bazy danych, jest konieczne tylko w celu zastąpienia dwie metody tworzenia `CursorAdapter` podklasy:

- **BindView** — podana widoku, zaktualizuj go, aby wyświetlić dane w podanych kursora.

- **Nowy widok** — wywoływane, gdy `ListView` wymaga, aby wyświetlić nowy widok. `CursorAdapter` Zajmie się odtwarzania widoków (w przeciwieństwie do `GetView` metody na kartach regularne).

Podklasy karty w przykładach wcześniejszych mają metody do zwracania liczby wierszy oraz do pobierania bieżącego elementu — `CursorAdapter` nie wymaga tych metod, ponieważ te informacje mogą zgromadzone z samej kursora. Dzieląc tworzenia i wypełniania każdego widoku w tych dwóch metod `CursorAdapter` wymusza widoku ponownego użycia. Pozwala to z kolei regularne karty Jeśli jest to możliwe zignorować `convertView` parametr `BaseAdapter.GetView` metody.


### <a name="implementing-the-cursoradapter"></a>Implementowanie CursorAdapter

Kod w **CursorTableAdapter/HomeScreenCursorAdapter.cs** zawiera `CursorAdapter` podklasy. Przechowuje odwołanie kontekstu przekazany do konstruktora, dzięki czemu mogą uzyskać dostęp `LayoutInflater` w `NewView` metody. Pełnej klasy wygląda następująco:

```csharp
public class HomeScreenCursorAdapter : CursorAdapter {
   Activity context;
   public HomeScreenCursorAdapter(Activity context, ICursor c)
       : base(context, c)
   {
       this.context = context;
   }
   public override void BindView(View view, Context context, ICursor cursor)
   {
       var textView = view.FindViewById<TextView>(Android.Resource.Id.Text1);
       textView.Text = cursor.GetString(1); // 'name' is column 1 in the cursor query
   }
   public override View NewView(Context context, ICursor cursor, ViewGroup parent)
   {
       return this.context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, parent, false);
   }
}
```


### <a name="assigning-the-cursoradapter"></a>Przypisywanie CursorAdapter

W `Activity` który wyświetli `ListView`, Utwórz kursor i `CursorAdapter` następnie przypisać je do widoku listy.

Kod, który wykonuje tej akcji w **CursorTableAdapter/HomeScreen.cs** `OnCreate` metody jest następujący:

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

`OnDestroy` Metoda zawiera `StopManagingCursor` wywołanie metody opisanych powyżej.



## <a name="related-links"></a>Linki pokrewne

- [SimpleCursorTableAdapter (przykład)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (przykład)](https://developer.xamarin.com/samples/CursorTableAdapter/)
