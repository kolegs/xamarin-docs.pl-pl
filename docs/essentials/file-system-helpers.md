---
title: 'Xamarin.Essentials: Pomocnicy systemu plików'
description: Klasa systemu plików w Xamarin.Essentials zawiera szereg pomocników, aby znaleźć pamięci podręcznej aplikacji i danych katalogów i otworzyć pliki wewnątrz pakietu aplikacji.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 13293ec05261cbdc1e70fd278002d1af18654851
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815621"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials: Pomocnicy systemu plików

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**FileSystem** klasa zawiera szereg pomocnicy do znajdowania aplikacji w pamięci podręcznej i danymi katalogów i otwierać pliki wewnątrz pakietu aplikacji.

## <a name="using-file-system-helpers"></a>Za pomocą pomocnicy systemu plików

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Można pobrać katalogu aplikacji do przechowywania **dane z pamięci podręcznej**. Dane w pamięci podręcznej może służyć do żadnych danych, która ma zostać zachowany dłużej niż dane tymczasowe, ale nie powinny być dane, które są wymagane do poprawnego funkcjonowania.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Aby uzyskać katalog najwyższego poziomu aplikacji dla wszystkich plików, które nie są plikami danych użytkownika. Kopię zapasową tych plików z systemem operacyjnym, synchronizowanie framework. Zobacz szczegóły implementacji platformy poniżej.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

Aby otworzyć plik, który pakiet aplikacji jest zawarte w pakiecie:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Funkcje specyficzne dla implementacji platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** — zwraca [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) bieżącego kontekstu.
- **AppDataDirectory** — zwraca [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) bieżącego kontekstu i kopii zapasowej przy użyciu [automatyczne kopie zapasowe](https://developer.android.com/guide/topics/data/autobackup.html) uruchamianie w 23 interfejsu API i nowszych.

Dodaj dowolny plik w **zasoby** folder w systemie Android projektu i Oznacz jako akcji kompilacji **AndroidAsset** do użycia jej wraz z `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** — zwraca [biblioteki/pamięci podręcznych](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) katalogu.
- **AppDataDirectory** — zwraca [biblioteki](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) katalogu, w którym jest wykonywana kopia zapasowa w programach iTunes i iCloud.

Dodaj dowolny plik w **zasobów** folder w narzędziu iOS projektu i Oznacz jako akcji kompilacji **BundledResource** do użycia jej wraz z `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

- **CacheDirectory** — zwraca [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) katalogu...
- **AppDataDirectory** — zwraca [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) katalog, w którym jest wykonywana kopia zapasowa w chmurze.

Dodaj dowolny plik w katalogu głównym projektu platformy uniwersalnej systemu Windows i Oznacz jako akcji kompilacji **zawartości** do użycia jej wraz z `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>interfejs API

- [Kod źródłowy pomocnicy systemu plików](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Dokumentacja interfejsu API systemu plików](xref:Xamarin.Essentials.FileSystem)
