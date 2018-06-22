---
title: Obsługa plików w platformy Xamarin.Forms
description: Obsługa z platformy Xamarin.Forms plików można osiągnąć za pomocą kodu w bibliotece .NET Standard lub przy użyciu zasobów osadzonych.
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 0be441a7be9777698212e690aca95fdd75e5050f
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310156"
---
# <a name="file-handling-in-xamarinforms"></a>Obsługa plików w platformy Xamarin.Forms

_Obsługa z platformy Xamarin.Forms plików można osiągnąć za pomocą kodu w bibliotece .NET Standard lub przy użyciu zasobów osadzonych._

## <a name="overview"></a>Omówienie

Kod platformy Xamarin.Forms działa na wielu platformach — każdy z nich ma własny system plików. Wcześniej oznacza to, Odczyt i zapis plików został najłatwiej wykonać przy użyciu macierzystych interfejsów API plików na każdej z platform. Można również zasoby osadzone są prostsze rozwiązania do rozpowszechniania plików danych z aplikacji. Jednak .NET 2.0 standardowe jest możliwe udostępnianie kodu dostępu do plików w bibliotekach .NET Standard.

Informacje dotyczące obsługi plików obrazów, można znaleźć na [Praca z obrazami](~/xamarin-forms/user-interface/images.md) strony.

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>Zapisywanie i ładowanie plików

`System.IO` Klasy mogą być używane do uzyskania dostępu do systemu plików na każdej z platform. `File` Klasa umożliwia tworzenie, usuwanie i odczytywać pliki i `Directory` klasa umożliwia tworzenie, usuwanie lub wyliczyć zawartości katalogów. Można również użyć `Stream` podklas, które zapewniają lepszej kontroli nad operacji na plikach (takie jak wyszukiwanie kompresji lub pozycji w pliku).

Plik tekstowy można pisać przy użyciu `File.WriteAllText` metody:

```csharp
File.WriteAllText(fileName, text);
```

Plik tekstowy mogą być odczytywane przy użyciu `File.ReadAllText` metody:

```csharp
string text = File.ReadAllText(fileName);
```

Ponadto `File.Exists` Metoda określa, czy istnieje określony plik:

```csharp
bool doesExist = File.Exists(fileName);
```

Ścieżka pliku w każdej platformy można ustalić na podstawie biblioteki .NET Standard przy użyciu wartości [ `Environment.SpecialFolder` ](xref:System.Environment.SpecialFolder) wyliczenie jako pierwszy argument `Environment.GetFolderPath` metody. To można połączyć z nazwy pliku `Path.Combine` metody:

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

Te operacje przedstawiono w przykładowej aplikacji, która zawiera stronę, która zapisuje i ładuje tekst:

[![Zapisywanie i ładowanie tekstu](files-images/saveandload-sml.png "zapisywanie i ładowanie plików w aplikacji")](files-images/saveandload.png#lightbox "zapisywanie i ładowanie plików w aplikacji")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Ładowanie plików osadzonych jako zasoby

Aby osadzić pliku do **.NET Standard** zestawu, utworzyć lub dodać plik i upewnij się, że **Akcja kompilacji: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Konfigurowanie osadzonych Akcja kompilacji zasobu](files-images/vs-embeddedresource-sml.png "EmbeddedResource BuildAction ustawienie")](files-images/vs-embeddedresource.png#lightbox "EmbeddedResource BuildAction ustawienie")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Plik tekstowy osadzone w PCL Konfigurowanie akcji kompilacji zasobów osadzonych](files-images/xs-embeddedresource-sml.png "EmbeddedResource BuildAction ustawienie")](files-images/xs-embeddedresource.png#lightbox "EmbeddedResource BuildAction ustawienie")

-----

`GetManifestResourceStream` Umożliwia dostęp do plików osadzonych przy użyciu jego **identyfikator zasobu**. Domyślnie identyfikator zasobu jest nazwa pliku jest poprzedzony prefiksem domyślnej przestrzeni nazw dla projektu jest osadzony w — w takim przypadku zestaw jest **WorkingWithFiles** i nazwa pliku jest **PCLTextResource.txt**, Dlatego jest identyfikator zasobu `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text` Zmiennej następnie można używać do wyświetlania tekstu lub używania w kodzie. Ten zrzut ekranu przedstawiający [Przykładowa aplikacja](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) wyświetlany jest tekst renderowany w `Label` formantu.

 [![Plik tekstowy osadzone w PCL](files-images/pcltext-sml.png "osadzonego pliku tekstowego w PCL wyświetlany w aplikacji")](files-images/pcltext.png#lightbox "osadzonego pliku tekstowego w PCL wyświetlany w aplikacji")

Ładowanie i deserializacji XML jest równie proste. Poniższy kod przedstawia plik XML jest załadowany i rozszeregować z zasobu, a następnie powiązany z `ListView` do wyświetlenia. Plik XML zawierający tablicę `Monkey` obiektów (klasa jest zdefiniowana w przykładowym kodzie).

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [![Plik XML osadzone w PCL wyświetlany w elemencie ListView](files-images/pclxml-sml.png "osadzonego pliku XML w PCL wyświetlany w elemencie ListView")](files-images/pclxml.png#lightbox "osadzonego pliku XML w PCL wyświetlane w widoku listy.")

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

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Organizowanie zasobów

Powyższe przykłady założono, że plik jest osadzony w folderze głównym projektu biblioteki .NET Standard, w których przypadku identyfikator zasobu ma postać **Namespace.Filename.Extension**, takich jak `WorkingWithFiles.PCLTextResource.txt` i `WorkingWithFiles.iOS.SharedTextResource.txt`.

Istnieje możliwość organizowania zasobów osadzonych w folderach. Osadzony zasób znajduje się w folderze, nazwy folderu staje się częścią Identyfikatora zasobu (oddzielone kropkami), że format Identyfikatora zasobu staje się **Namespace.Folder.Filename.Extension**. Umieszczanie plików używane w przykładowej aplikacji w folderze **MójFolder** spowodowałoby, odpowiadający jej zasób identyfikatorów `WorkingWithFiles.MyFolder.PCLTextResource.txt` i `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>Debugowanie zasobów osadzonych

Ponieważ czasami trudno jest zrozumienie, dlaczego nie został załadowany określonego zasobu, poniższy kod debugowania można tymczasowo dodane do aplikacji, aby potwierdzić, że zasoby są poprawnie skonfigurowane. On dane wyjściowe obejmują wszystkie znanych zasobów osadzonych w danym zestawie do **błędy** konsoli do debugowania problemów ładowania zasobów.

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

## <a name="summary"></a>Podsumowanie

W tym artykule wykazało niektóre operacje prostego pliku zapisywanie i ładowanie tekstu na urządzeniu i ładowanie zasobów osadzonych. .NET 2.0 standardowe jest możliwe udostępnianie kodu dostępu do plików w bibliotekach .NET Standard.

## <a name="related-links"></a>Linki pokrewne

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
- [Praca w systemie plików w Xamarin.iOS](~/ios/app-fundamentals/file-system.md)
- [Pliki skoroszytu](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
