---
title: Visual języku systemu Xamarin iOS i Android
description: Wskazówki XThis demonstruje sposób tworzenia natywnych aplikacji platformy Xamarin.iOS i platformy Xamarin.Android wykorzystujące reguły biznesowe napisane w języku Visual.
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 3b92442668f6bd86c1c521dd6b476e4bbb43c1ec
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="visual-basicnet-in-xamarin-ios-and-android"></a>Visual języku systemu Xamarin iOS i Android

[TaskyPortable](/samples/mobile/VisualBasic/TaskyPortableVB/) przykładowej aplikacji pokazano, jak kod Visual Basic skompilowany w przenośnej biblioteki klas może służyć za pomocą platformy Xamarin. Poniżej przedstawiono niektóre zrzuty ekranu wynikowy aplikacje uruchomione w systemach iOS, Android i Windows Phone:

 [![](native-apps-images/image5.png "iOS, Android i Windows, telefony uruchamianie aplikacji skompilowanej za pomocą języka Visual Basic")](native-apps-images/image5.png#lightbox)

IOS, Android i Windows Phone, które projekty w tym przykładzie są napisane w języku C#. Wbudowana w macierzystym technologii interfejsu użytkownika dla każdej aplikacji (Scenorys, Xml i Xaml odpowiednio), podczas `TodoItem` zarządzania są dostarczane przez przenośną bibliotekę klas języka Visual Basic przy użyciu `IXmlStorage` dostarczonych przez implementację Projekt natywnej.

## <a name="sample-walkthrough"></a>Przykładowe wskazówki

W tym przewodniku opisano, jak zostało zaimplementowane w języku Visual Basic [TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB) przykładowych program Xamarin dla systemów iOS i Android.

> [!NOTE]
> Przeczytaj instrukcje na [Visual PCLs języku](/guides/cross-platform/application_fundamentals/pcl/portable_visual_basic_net/) przed kontynuowaniem w tym przewodniku.

## <a name="visualbasicportablelibrary"></a>VisualBasicPortableLibrary

Biblioteki klas przenośnych Visual Basic można tworzyć tylko w programie Visual Studio.
Biblioteka przykład zawiera podstawowe informacje o aplikacji w czterech plikach języka Visual Basic:

-  IXmlStorage.vb
-  TodoItem.vb
-  TodoItemManager.vb
-  TodoItemRepositoryXML.vb


### <a name="ixmlstoragevb"></a>IXmlStorage.vb

Ponieważ zachowania dostępu do pliku się więc znacznie różnić między platformami, nie podawaj przenośnej biblioteki klas `System.IO` magazyn interfejsów API w żadnym profilu plików. Oznacza to, że jeśli chcemy bezpośrednią interakcję z systemu plików w naszym kodzie przenośny, musimy wywołania zwrotnego naszych projektów natywnych na każdej z platform.  Zapisywanie naszego kodu języka Visual Basic przed prosty interfejs, który można zaimplementować w języku C# na każdej platformie, można mamy możliwe do udostępnienia kodu języka Visual Basic, który ma nadal dostęp do systemu plików.

Przykładowy kod używa tego bardzo prosty interfejs, który zawiera tylko dwie metody: Odczyt i zapis serializacji pliku Xml.

```vb
Public Interface IXmlStorage
    Function ReadXml(filename As String) As List(Of TodoItem)
    Sub WriteXml(tasks As List(Of TodoItem), filename As String)
End Interface
```

systemy iOS, Android i Windows Phone implementacje tego interfejsu będą wyświetlane w dalszej części tego przewodnika.

### <a name="todoitemvb"></a>TodoItem.vb

Ta klasa zawiera obiektu biznesowego, można użyć w całej aplikacji. On są definiowane w języku Visual Basic i udostępnionych z systemem iOS, Android i Windows Phone projektów, które zostały napisane w języku C#.

Definicja klasy jest następujący:

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

Przykład używa XML serializacji i deserializacji do ładowania i zapisania obiektów TodoItem.

### <a name="todoitemmanagervb"></a>TodoItemManager.vb

Klasa Manager przedstawia "Interfejsu API" dla przenośnego kodu. Zapewnia podstawowe operacje CRUD na `TodoItem` klasy, ale nie wykonywania tych operacji.

```vb
Public Class TodoItemManager
    Private _repository As TodoItemRepositoryXML
    Public Sub New(filename As String, storage As IXmlStorage)
        _repository = New TodoItemRepositoryXML(filename, storage)
    End Sub
    Public Function GetTask(id As Integer) As TodoItem
        Return _repository.GetTask(id)
    End Function
    Public Function GetTasks() As List(Of TodoItem)
        Return New List(Of TodoItem)(_repository.GetTasks())
    End Function
    Public Function SaveTask(item As TodoItem) As Integer
        Return _repository.SaveTask(item)
    End Function
    Public Function DeleteTask(item As TodoItem) As Integer
        Return _repository.DeleteTask(item.ID)
    End Function
End Class
```

Konstruktor przyjmuje wystąpienia IXmlStorage jako parametr. Umożliwia to zapewnienie własną implementację pracy, a jednocześnie zezwalając na kod przenośny opisano inne funkcje, które mogą być udostępniane każdej platformy.

### <a name="todoitemrepositoryvb"></a>TodoItemRepository.vb

Klasa repozytorium zawiera logikę do zarządzania listę obiektów TodoItem. Kompletny kod poniżej przedstawiono — logika istnieje głównie w celu zarządzania unikatowa wartość Identyfikatora w TodoItems wraz z dodawaniem i usunięty z kolekcji.

```vb
Public Class TodoItemRepositoryXML
    Private _filename As String
    Private _storage As IXmlStorage
    Private _tasks As List(Of TodoItem)

    ''' <summary>Constructor</summary>
    Public Sub New(filename As String, storage As IXmlStorage)
        _filename = filename
        _storage = storage
        _tasks = _storage.ReadXml(filename)
    End Sub
    ''' <summary>Inefficient search for a Task by ID</summary>
    Public Function GetTask(id As Integer) As TodoItem
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                Return _tasks(t)
            End If
        Next
        Return New TodoItem() With {.ID = id}
    End Function
    ''' <summary>List all the Tasks</summary>
    Public Function GetTasks() As IEnumerable(Of TodoItem)
        Return _tasks
    End Function
    ''' <summary>Save a Task to the Xml file
    ''' Calculates the ID as the max of existing IDs</summary>
    Public Function SaveTask(item As TodoItem) As Integer
        Dim max As Integer = 0
        If _tasks.Count > 0 Then
            max = _tasks.Max(Function(t As TodoItem) t.ID)
        End If
        If item.ID = 0 Then
            item.ID = ++max
            _tasks.Add(item)
        Else
            Dim j = _tasks.Where(Function(t) t.ID = item.ID).First()
            j = item
        End If
        _storage.WriteXml(_tasks, _filename)
        Return max
    End Function
    ''' <summary>Removes the task from the XMl file</summary>
    Public Function DeleteTask(id As Integer) As Integer
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                _tasks.RemoveAt(t)
                _storage.WriteXml(_tasks, _filename)
                Return 1
            End If
        Next
        Return -1
    End Function
End Class
```

> [!NOTE]
> Ten kod jest przykładem mechanizm bardzo podstawowe magazynu danych.
> Zapewnia on pokazują, jak przenośnej biblioteki klas może kodu interfejsu do korzystania z funkcji specyficznych dla platformy (w tym przypadku ładowania i zapisywania pliku Xml). Go go nie mają być alternatywą wysokiej jakości bazy danych.

## <a name="ios-android-and-windows-phone-application-projects"></a>systemy iOS, Android i projekty aplikacji Windows Phone

Ta sekcja zawiera implementacje specyficzne dla platformy dla interfejsu IXmlStorage i pokazuje, jak jest używane w każdej aplikacji. Projekty aplikacji są napisane w języku C#.

### <a name="ios-and-android-ixmlstorage"></a>iOS i Android IXmlStorage

Xamarin.iOS i Xamarin.Android zapewniają pełną `System.IO` funkcji, aby można było łatwo obciążenia i Zapisz plik Xml przy użyciu następującej klasy:

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        if (File.Exists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new FileStream(filename, FileMode.Open))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(filename))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

W aplikacji systemu iOS `TodoItemManager` i `XmlStorageImplementation` są tworzone w **AppDelegate.cs** plików, jak pokazano w poniższym przykładzie. Pierwsze cztery wiersze po prostu budowania ścieżkę do pliku, w którym będą przechowywane dane; ostatnie dwie linie Pokaż dwie klasy uruchamianiu.

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

W aplikacji systemu Android `TodoItemManager` i `XmlStorageImplementation` są tworzone w **Application.cs** plików, jak pokazano w poniższym przykładzie. Pierwsze trzy wiersze po prostu budowania ścieżkę do pliku, w którym będą przechowywane dane; ostatnie dwie linie Pokaż dwie klasy uruchamianiu.

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new AndroidTodo.XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

Pozostała część kod aplikacji jest głównie dane przy użyciu interfejsu użytkownika i użycie `TaskMgr` klasy przy ładowaniu i zapisywaniu `TodoItem` klasy.

### <a name="windows-phone-ixmlstorage"></a>Windows Phone IXmlStorage

Windows Phone nie zapewnia pełny dostęp do urządzenia systemu plików, zamiast tego udostępnianie IsolatedStorage API. `IXmlStorage` Implementacji dla Windows Phone wygląda następująco:

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        if (fileStorage.FileExists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new StreamReader(new IsolatedStorageFileStream(filename, FileMode.Open, fileStorage)))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(new IsolatedStorageFileStream(filename, FileMode.OpenOrCreate, fileStorage)))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

`TodoItemManager` i `XmlStorageImplementation` są tworzone w **App.xaml.cs** plików, jak pokazano w poniższym przykładzie.

```csharp
var filename = "TodoList.xml";
var xmlStorage = new XmlStorageImplementation();
TodoMgr = new TodoItemManager(filename, xmlStorage);
```

Pozostała część aplikacji Windows Phone składa się z Xaml i C# do tworzenia interfejsu użytkownika i użyj `TodoMgr` klasy przy ładowaniu i zapisywaniu `TodoItem` obiektów.

## <a name="visual-basic-pcl-in-visual-studio-for-mac"></a>Visual Basic PCL w programie Visual Studio dla komputerów Mac

Visual Studio for Mac nie obsługuje języka Visual Basic — nie można utworzyć ani Kompiluj projekty Visual Basic w programie Visual Studio dla komputerów Mac.

Programu Visual Studio for Mac na obsługę bibliotek klas przenośnych oznacza, że można się odwoływać PCL zestawy, które zostały skompilowane z języka Visual Basic.

W tej sekcji wyjaśniono, jak skompilować zestawem PCL w programie Visual Studio i upewnij się, że zostanie ono przechowywane w systemie kontroli wersji i odwołuje się innych projektów.

### <a name="keeping-the-pcl-output-from-visual-studio"></a>Utrzymywanie PCL dane wyjściowe z programu Visual Studio

Domyślnie większość systemów kontroli wersji (w tym TFS i Git) zostanie skonfigurowana do ignorowania **/bin/** katalogu, co oznacza w skompilowanym zestawie PCL nie będą przechowywane. Oznacza to, że trzeba ręcznie skopiować go na komputerach z systemem programu Visual Studio for Mac dodać odwołanie do niej.

Aby zapewnić, że system kontroli wersji można przechowywać dane wyjściowe zestawu PCL, można utworzyć skrypt po kompilacji, który kopiuje go do katalogu głównym projektu. Ten krok postkompilacyjnego zapewnia zestawu można łatwo dodać do kontroli źródła i współużytkowane z innymi projektami.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

1. Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **właściwości > zdarzeń kompilacji** sekcji.

2. Dodaj _postkompilacyjnego_ skrypt, który kopiuje dane wyjściowe biblioteki DLL z tego projektu do katalogu głównego projektu (która jest poza **/bin/**). W zależności od konfiguracji kontroli wersji biblioteki DLL powinno być teraz możliwe do dodania do kontroli źródła.

  [![](native-apps-images/image6-vs-sml.png "Zdarzenia kompilacji skryptu używanego po kompilacji do skopiowania VB DLL")](native-apps-images/image6-vs.png#lightbox)

#### <a name="visual-studio-2015"></a>Visual Studio 2015

1.  Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **właściwości > Skompiluj** , upewnij się, wszystkie konfiguracje jest zaznaczona w polu linie oddzielające lewego górnego. Kliknij przycisk **zdarzenia kompilacji...**  przycisk w prawym dolnym rogu.

    [![](native-apps-images/image6.png "Sekcja kompilacji właściwości projektu")](native-apps-images/image6.png#lightbox)

1.  Dodawanie skryptu mające miejsce po kompilacji, który kopiuje dane wyjściowe biblioteki DLL z tego projektu do katalogu głównego projektu (która jest poza **/bin/** ). W zależności od konfiguracji kontroli wersji biblioteki DLL powinno być teraz możliwe do dodania do kontroli źródła.

    [![](native-apps-images/image7.png "Tworzenie okna zdarzeń")](native-apps-images/image7.png#lightbox)

#### <a name="all-versions"></a>Wszystkie wersje

Następnym Skompiluj projekt, zestawu przenośnej biblioteki klas zostaną skopiowane do katalogu głównego projektu, a podczas wyboru — w/commit/wypchnąć zmiany plik DLL, który będzie przechowywany (tak, aby można go pobrać na komputerze Mac z programem Visual Studio dla komputerów Mac).

  [![](native-apps-images/image8-sml.png "Lokalizacja pliku w Visual Basic zestawu wyjściowego")](native-apps-images/image8.png#lightbox)


Ten zestaw może następnie można dodać do projektów platformy Xamarin w programie Visual Studio dla komputerów Mac, nawet jeśli sam języka Visual Basic nie jest obsługiwane w Xamarin iOS lub Android projektów.

### <a name="referencing-the-pcl-in-visual-studio-for-mac"></a>Odwołanie do PCL w programie Visual Studio dla komputerów Mac

Ponieważ Xamarin nie obsługuje języka Visual Basic nie można załadować projektu PCL (ani aplikacji Windows Phone) opisane w tym zrzut ekranu:

 [![](native-apps-images/image9.png "Programu Visual Studio for Mac rozwiązania")](native-apps-images/image9.png#lightbox)

Będziemy nadal zawierają zestawu PCL Visual Basic DLL w projektach Xamarin.iOS i Xamarin.Android:

1.  Kliknij prawym przyciskiem myszy **odwołania** a następnie wybierz węzeł **odwołuje się do edycji...**

    [![](native-apps-images/image10.png "Menu odwołania do edycji projektu")](native-apps-images/image10.png#lightbox)

1.  Wybierz **.Net zestawu** karcie i przejdź do pliku DLL w katalogu projektu Visual Basic. Mimo że program Visual Studio dla komputerów Mac nie można otworzyć projektu, wszystkie pliki z kontroli źródła powinna być istnieje. Kliknij przycisk **Dodaj** następnie **OK** można dodać tego zestawu z systemami iOS i Android aplikacje.

    [![](native-apps-images/image11-sml.png "Kliknij przycisk Dodaj, a następnie OK aby dodać ten zestaw z systemami iOS i Android aplikacje")](native-apps-images/image11.png#lightbox)

1.  IOS i Android aplikacje teraz mogą obejmować logiki aplikacji udostępniane przez przenośną bibliotekę klas języka Visual Basic. Ten zrzut ekranu przedstawia odwołuje się do PCL Visual Basic, który zawiera kod, który korzysta z funkcji z tej biblioteki aplikacji systemu iOS.

    [![](native-apps-images/image12-sml.png "Edytuj odwołania do dodania okna zestawu .NET")](native-apps-images/image12.png#lightbox)


Jeśli wprowadzono zmiany do projektu Visual Basic w programie Visual Studio Pamiętaj, aby skompilować projekt, przechowywać wynikowego zestawu biblioteki DLL w kontroli źródła, a następnie ściągnięcia tej nowej biblioteki DLL z kontroli źródła na komputerze Mac, dzięki czemu kompilacji programu Visual Studio for Mac zawiera najnowsze funkcje.


## <a name="summary"></a>Podsumowanie

W tym artykule wykazała jak korzystać z kodu języka Visual Basic w aplikacjach platformy Xamarin.IOS przy użyciu programu Visual Studio i przenośnej biblioteki klas. Mimo że Xamarin nie obsługuje bezpośredniego Visual Basic, kompilowania kodu Visual Basic w PCL umożliwia kod napisany w języku Visual Basic do uwzględnienia w systemów iOS i Android aplikacje.

## <a name="related-links"></a>Linki pokrewne

- [TaskyPortableVB (przykład)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [Programowanie wieloplatformowych aplikacji za pomocą programu .NET Framework (Microsoft)](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
