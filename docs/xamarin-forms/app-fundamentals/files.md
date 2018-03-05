---
title: Pliki
description: "Obsługa z platformy Xamarin.Forms plików może odbywać się przy użyciu zasobów osadzonych lub zapisywanie przed natywnych interfejsów API systemu plików."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/22/2017
ms.openlocfilehash: 605374c0f2bfe656e564e48d14ffe18ce5b7dfe5
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="files"></a>Pliki

_Obsługa z platformy Xamarin.Forms plików może odbywać się przy użyciu zasobów osadzonych lub zapisywanie przed natywnych interfejsów API systemu plików._

## <a name="overview"></a>Omówienie

Kod platformy Xamarin.Forms działa na wielu platformach — każdy z nich ma własny system plików. Oznacza to, że Odczyt i zapis plików większość łatwo dokonana, przy użyciu macierzystych interfejsów API plików na każdej z platform. Można również zasoby osadzone są prostsze rozwiązania do rozpowszechniania plików danych z aplikacji.

W tym dokumencie opisano następujące typowych plików obsługi scenariuszy:

-  [ **Osadzone pliki jako zasoby** ](#Loading_Files_Embedded_as_Resources) -pliki mogą być wysyłane w ramach aplikacji i załadować przy użyciu interfejsu API odbicia.
-  [ **Zapisywanie i ładowanie plików** ](#Loading_and_Saving_Files) -użytkownika zapisywalny magazynowania może być zaimplementowany w sposób macierzysty, a następnie uzyskać dostęp przy użyciu `DependencyService` .


Wywołania składników innych firm **PCLStorage** mogą służyć do odczytu i zapisu plików do użytkownika dostępny magazynowania z PCL kodu.

Informacje dotyczące obsługi plików obrazów, można znaleźć na [Praca z obrazami](~/xamarin-forms/user-interface/images.md) strony.

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Ładowanie plików osadzonych jako zasoby

Aby osadzić pliku do **PCL** zestawu, utworzyć lub dodać plik i upewnij się, że **Akcja kompilacji: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Konfigurowanie osadzonych Akcja kompilacji zasobu](files-images/vs-embeddedresource-sml.png "EmbeddedResource BuildAction ustawienie")](files-images/vs-embeddedresource.png "EmbeddedResource BuildAction ustawienie")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Plik tekstowy osadzone w PCL Konfigurowanie akcji kompilacji zasobów osadzonych](files-images/xs-embeddedresource-sml.png "EmbeddedResource BuildAction ustawienie")](files-images/xs-embeddedresource.png "EmbeddedResource BuildAction ustawienie")

-----

`GetManifestResourceStream` Umożliwia dostęp do plików osadzonych przy użyciu jego **identyfikator zasobu**. Domyślnie identyfikator zasobu jest nazwa pliku jest poprzedzony prefiksem domyślnej przestrzeni nazw dla projektu jest osadzony w — w takim przypadku zestaw jest **WorkingWithFiles** i nazwa pliku jest **PCLTextResource.txt**, Dlatego jest identyfikator zasobu `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = typeof(LoadResourceText).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text` Zmiennej następnie można używać do wyświetlania tekstu lub używania w kodzie. Ten zrzut ekranu przedstawiający [Przykładowa aplikacja](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) wyświetlany jest tekst renderowany w `Label` formantu.

 [ ![Plik tekstowy osadzone w PCL](files-images/pcltext-sml.png "osadzonego pliku tekstowego w PCL wyświetlany w aplikacji")](files-images/pcltext.png "osadzonego pliku tekstowego w PCL wyświetlany w aplikacji")

Ładowanie i deserializacji XML jest równie proste. Poniższy kod przedstawia plik XML jest załadowany i rozszeregować z zasobu, a następnie powiązany z `ListView` do wyświetlenia. Plik XML zawierający tablicę `Monkey` obiektów (klasa jest zdefiniowana w przykładowym kodzie).

```csharp
var assembly = typeof(LoadResourceText).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [ ![Plik XML osadzone w PCL wyświetlany w elemencie ListView](files-images/pclxml-sml.png "osadzonego pliku XML w PCL wyświetlany w elemencie ListView")](files-images/pclxml.png "osadzonego pliku XML w PCL wyświetlane w widoku listy.")

<a name="Embedding_in_Shared_Projects" />

### <a name="embedding-in-shared-projects"></a>Osadzanie w udostępnionych projektów

Projekty Shared może również zawierać pliki jako zasoby osadzone, jednak ponieważ zawartość projektu udostępnionego są kompilowane do odwołującego się projektów, prefiks używany do osadzonego zasobu pliku, które identyfikatory można zmienić. Oznacza to, że identyfikator zasobu dla każdego osadzonego pliku mogą być inne dla każdej platformy.

Istnieją dwa rozwiązania tego problemu z udostępnionych projektów:

-  **Synchronizacja projektów** -Edytuj właściwości projektu dla każdej platformy użyć **tego samego** przestrzeni nazw nazwa i domyślnego zestawu. Tę wartość można następnie "zapisane na stałe" jako prefiks osadzonego zasobu identyfikatorów w projekcie udostępnionym.
-  **dyrektywy kompilatora #if** -użyj dyrektywy kompilatora zasobów poprawny prefiks Identyfikatora i wartości używać dynamicznie utworzyć identyfikator zasobu poprawne.


Poniżej przedstawiono kod pokazujący drugiej opcji. Dyrektywy kompilatora są używane w celu wybrania prefiks zasobów zapisane na stałe (która jest zwykle taka sama jak domyślnej przestrzeni nazw dla projektu odwołującego się). `resourcePrefix` Zmienna jest następnie używany do tworzenia Identyfikatora prawidłowy zasób, łącząc go z nazwą zasobu osadzonego pliku.

```csharp
#if __IOS__
var resourcePrefix = "WorkingWithFiles.iOS.";
#endif
#if __ANDROID__
var resourcePrefix = "WorkingWithFiles.Droid.";
#endif
#if WINDOWS_PHONE
var resourcePrefix = "WorkingWithFiles.WinPhone.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = typeof(SharedPage).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Organizowanie zasobów

Powyższe przykłady założono, że plik jest osadzony w folderze głównym projektu PCL, w których przypadku identyfikator zasobu ma postać **Namespace.Filename.Extension**, takich jak `WorkingWithFiles.PCLTextResource.txt` i `WorkingWithFiles.iOS.SharedTextResource.txt`.

Istnieje możliwość organizowania zasobów osadzonych w folderach. Osadzony zasób znajduje się w folderze, nazwy folderu staje się częścią Identyfikatora zasobu (oddzielone kropkami), że format Identyfikatora zasobu staje się **Namespace.Folder.Filename.Extension**. Umieszczanie plików używane w przykładowej aplikacji w folderze **MójFolder** spowodowałoby, odpowiadający jej zasób identyfikatorów `WorkingWithFiles.MyFolder.PCLTextResource.txt` i `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>Debugowanie zasobów osadzonych

Ponieważ czasami trudno jest zrozumienie, dlaczego nie został załadowany określonego zasobu, poniższy kod debugowania można tymczasowo dodane do aplikacji, aby potwierdzić, że zasoby są poprawnie skonfigurowane. On dane wyjściowe obejmują wszystkie znanych zasobów osadzonych w danym zestawie do **błędy** konsoli do debugowania problemów ładowania zasobów.

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = typeof(SharedPage).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>Zapisywanie i ładowanie plików

Platformy Xamarin.Forms działa na wielu platformach, każde z nich własny system plików, nie istnieje żadne pojedynczego podejście do ładowania i zapisywania plików utworzonych przez użytkownika. Aby zademonstrować sposób zapisania i załadować plików tekstowych Przykładowa aplikacja zawiera ekranu, który zapisuje i ładuje niektóre dane wejściowe użytkownika — Zakończono ekranu przedstawiono poniżej:

 [ ![Zapisywanie i ładowanie tekstu](files-images/saveandload-sml.png "zapisywanie i ładowanie plików w aplikacji")](files-images/saveandload.png "zapisywanie i ładowanie plików w aplikacji")

Dotyczy wszystkich platform struktury katalogów nieco inne i możliwości innego systemu plików — na przykład Xamarin.iOS i Xamarin.Android obsługuje większość `System.IO` funkcji, ale Windows Phone obsługuje tylko `IsolatedStorage` i [ `Windows.Storage` ](http://msdn.microsoft.com/library/windowsphone/develop/jj681698(v=vs.105).aspx) Interfejsów API.

Aby ominąć ten problem, przykładowej aplikacji definiuje interfejs w PCL platformy Xamarin.Forms do ładowania i zapisać pliki. Zapewnia prosty interfejs API do ładowania i zapisać pliki tekstowe, które będą przechowywane na urządzeniu.

```csharp
public interface ISaveAndLoad {
    void SaveText (string filename, string text);
    string LoadText (string filename);
}
```

PCL kod używa następnie [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) uzyskać odwołanie do natywnego implementacji interfejsu. Dzięki temu kod przenośny delegować ładowania i zapisywania plików do klasy zapisane we wszystkich projektach specyficzne dla platformy. W przykładzie **zapisać** i **obciążenia** przyciski są zapisywane, jak pokazano:

```csharp
var saveButton = new Button {Text = "Save"};
saveButton.Clicked += (sender, e) => {
    DependencyService.Get<ISaveAndLoad>().SaveText("temp.txt", input.Text);
};
var loadButton = new Button {Text = "Load"};
loadButton.Clicked += (sender, e) => {
    output.Text = DependencyService.Get<ISaveAndLoad>().LoadText("temp.txt");
};
```

Następnie należy dodać wszystkie projekty specyficzne dla platformy przed plików faktycznie można zapisać i załadować implementację.

### <a name="ios-and-android"></a>iOS i Android

Implementacja interfejsu dla projektów Xamarin.iOS i Xamarin.Android mogą być identyczne. Kod poniżej pokazano, w tym `[assembly: Dependency (typeof (SaveAndLoad))]` atrybut, który jest wymagany dla `DependencyService` do pracy.

```csharp
[assembly: Dependency (typeof (SaveAndLoad))]
namespace WorkingWithFiles {
    public class SaveAndLoad : ISaveAndLoad {
        public void SaveText (string filename, string text) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            System.IO.File.WriteAllText (filePath, text);
        }
        public string LoadText (string filename) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            return System.IO.File.ReadAllText (filePath);
        }
    }
}
```

### <a name="universal-windows-platform-uwp-windows-81-and-windows-phone-81"></a>Platforma uniwersalna systemu Windows (UWP), Windows 8.1 i Windows Phone 8.1

Tych platform mają różne filesystem interfejsu API — [ `Windows.Storage` ](/windows/uwp/files/quickstart-reading-and-writing-files/) — to znaczy umożliwia zapisywanie i ładowanie plików.
`ISaveAndLoad` Interfejs może zostać zaimplementowany, jak pokazano poniżej:

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using WinApp;
using WorkingWithFiles;
using Xamarin.Forms;

[assembly: Dependency(typeof(SaveAndLoad_WinApp))]

namespace WindowsApp
{
    // https://msdn.microsoft.com/library/windows/apps/xaml/hh758325.aspx
    public class SaveAndLoad_WinApp : ISaveAndLoad
    {
        public async Task SaveTextAsync(string filename, string text)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await localFolder.CreateFileAsync(filename, CreationCollisionOption.ReplaceExisting);
            await FileIO.WriteTextAsync(sampleFile, text);
        }
        public async Task<string> LoadTextAsync(string filename)
        {
            StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await storageFolder.GetFileAsync(filename);
            string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
            return text;
        }
    }
}
```


<a name="Saving_and_Loading_in_Shared_Projects" />

### <a name="saving-and-loading-in-shared-projects"></a>Zapisywanie i ładowanie w udostępnionych projektów

Ponieważ udostępnionych projektów obsługuje dyrektywy kompilatora jest może zawierać kod specyficzne dla platformy w pliku jednej klasy wewnątrz projektu udostępnionego (bez użycia `DependencyService`).
Pojedynczy `SaveAndLoad` klasy może być zapisany zawiera zarówno implementacje powyżej selektywnie skompilowany do projektów odwołujący się przy użyciu dyrektywy kompilatora, takich jak `#if WINDOWS_PHONE`, `#if __IOS__`, i `#if __ANDROID__`.

## <a name="additional-information"></a>Dodatkowe informacje

Projekty platformy Xamarin.Forms PCL można również korzystać z [PCLStorage NuGet](http://www.nuget.org/packages/pclstorage) ([kod &amp; dokumentacji](https://pclstorage.codeplex.com/)) pomagających w realizacji operacji na plikach w sposób między platformami.


## <a name="summary"></a>Podsumowanie

Ten dokument ma pokazano niektóre operacje prostego pliku do ładowania zasobów osadzonych i zapisywanie i ładowanie tekstu na urządzeniu. Deweloperzy mogą zaimplementować własnych natywny plik interfejsów API przy użyciu `DependencyService`, ułatwiając złożonego wymagane do obsługi wymagań manipulowania pliku.


## <a name="related-links"></a>Linki pokrewne

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Tworzenia, zapisywania i odczytywania plików (UWP)](/windows/uwp/files/quickstart-reading-and-writing-files/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
- [Pliki skoroszytu](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
