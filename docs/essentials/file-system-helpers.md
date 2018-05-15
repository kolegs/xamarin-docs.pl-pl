---
title: Pomocnicy System plików Xamarin.Essentials
description: Klasa System plików zawiera szereg pomocników można znaleźć pamięci podręcznej aplikacji i danych katalogów i otwierać pliki wewnątrz pakietu aplikacji.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 871d164df982d1d170e8ba5bffd3bd6600a4cdda
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-file-system-helpers"></a>Pomocnicy System plików Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**FileSystem** klasy zawiera szereg pomocników katalogi pamięci podręcznej i dane aplikacji i otwierać pliki wewnątrz pakietu aplikacji.

## <a name="using-file-system-helpers"></a>Przy użyciu pomocników systemu plików

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Można uzyskać katalogu aplikacji do przechowywania **dane z pamięci podręcznej**. Dane w pamięci podręcznej może służyć do żadnych danych, który ma zostać zachowany dłuższy niż dane tymczasowe, ale nie powinien być dane wymagane do poprawnego funkcjonowania.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Aby uzyskać diredctory najwyższego poziomu aplikacji dla wszystkie pliki, które nie są plikami danych użytkownika. Kopii zapasowej tych plików w systemie operacyjnym synchronizowanie framework. Zobacz szczegóły implementacji platformy poniżej.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

Aby otworzyć plik pakietu aplikacji jest umieszczany w pakietach:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Szczegóły implementacji platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** — zwraca [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) bieżącego kontekstu.
- **AppDataDirectory** — zwraca [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) bieżącego kontekstu i czy kopii zapasowej za pomocą [kopii zapasowej Autu](https://developer.android.com/guide/topics/data/autobackup.html) uruchamiania na 23 interfejsu API i nowszym.

Dodaj dowolny plik w **zasoby** folderu w Android projektu i Oznacz jako akcji kompilacji **AndroidAsset** jej używać z `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** — zwraca [Biblioteka/pamięci podręcznych](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) katalogu.
- **AppDataDirectory** — zwraca [biblioteki](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) katalogu, w którym została skopiowana przez program iTunes i iCloud.

Dodaj dowolny plik w **zasobów** folder w systemie iOS projektu i Oznacz jako akcji kompilacji **BundledResource** jej używać z `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

- **CacheDirectory** — zwraca [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) katalogu...
- **AppDataDirectory** — zwraca [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) katalogu, w którym jest wykonywana kopia zapasowa w chmurze.

Dodaj dowolny plik w katalogu głównym projektu platformy uniwersalnej systemu Windows i oznacz Akcja kompilacji jako **zawartości** jej używać z `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>interfejs API

- [Kod źródłowy pomocników systemu plików](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Dokumentacja interfejsu API systemu plików](xref:Xamarin.Essentials.FileSystem)
