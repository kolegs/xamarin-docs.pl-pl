---
title: Dostęp do plików z rozszerzeniem Xamarin.Android
description: Ten przewodnik będzie zawierał wyjaśnienie dostęp do plików na platformie Xamarin.Android
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 07/23/2018
ms.openlocfilehash: 5a4ddf606bb71bef10cf99660c198c5a8fdb1b69
ms.sourcegitcommit: 9bb9e8297d3edd9a50585f4ba53c1b4f0bcd1d3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39212154"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>File Storage i dostęp za pomocą platformy Xamarin.Android

Typowym wymogiem dla aplikacji systemu Android jest do manipulowania plikami &ndash; zapisywanie obrazów, pobieranie dokumentów lub eksportowanie danych, aby udostępnić innych programów. Android (opartym na systemie Linux) obsługuje tę funkcję, udostępniając miejsca przechowywania plików. Android grupuje systemu plików w dwóch różnych typów magazynów:

* **Wewnętrzny magazyn** &ndash; jest część systemu plików, które mogą mieć dostęp tylko aplikacji lub systemu operacyjnego.
* **Magazyn zewnętrzny** &ndash; to partycji do przechowywania plików, który jest dostępny dla wszystkich aplikacji, użytkownika i ewentualnie inne urządzenia. Na niektórych urządzeniach magazynu zewnętrznego mogą być wymiennych (np. karta SD).

Te grupy są tylko koncepcyjne i nie musi odwoływać się do jednej partycji lub w katalogu na urządzeniu. Urządzenia z systemem Android będzie zawsze podawać partycji dla wewnętrznej i zewnętrznej usługi storage. Istnieje możliwość, że niektóre urządzenia mogą mieć wiele partycji, które są traktowane jako magazynu zewnętrznego. Niezależnie od partycji interfejsów API do odczytu zapisu lub tworzenia plików jest taka sama. Istnieją dwa zestawy interfejsów API, które aplikacji platformy Xamarin.Android może używać do uzyskiwania dostępu do plików:

1. **Interfejsów API platformy .NET (udostępniane przez narzędzie Mono i opakowane przez rozszerzenie Xamarin.Android)** &ndash; te obejmują [pomocnicy systemu plików](~/essentials/file-system-helpers.md?context=xamarin/android) dostarczone przez [Xamarin.Essentials](~/essentials/index.md?context=xamarin/android). Interfejsy API platformy .NET zapewnia najlepsze zgodności dla wielu platform i jako takie ten przewodnik koncentruje się na tych interfejsów API.
1. **Java pliku dostęp do natywnego API (dostarczonych przez Java i opakowane przez rozszerzenie Xamarin.Android)** &ndash; Java udostępnia swoje własne interfejsy API do odczytywania i zapisywania plików. Te są całkowicie dopuszczalne jest alternatywą do interfejsów API platformy .NET, ale są specyficzne dla systemów Android i nie są odpowiednie dla aplikacji, które mają być dla wielu platform.

Odczytywanie i zapisywanie w plikach jest niemal identyczne platformie Xamarin.Android, jak wszystkie inne aplikacje platformy .NET. Aplikacji platformy Xamarin.Android Określa ścieżkę do pliku, który będzie można modyfikować, następnie używa standardowych .NET idiomy przy dostępie do plików. Ponieważ rzeczywiste ścieżki do magazynu wewnętrznych i zewnętrznych mogą się różnić od urządzenia, lub z wersji systemu Android do wersji dla systemu Android, nie zaleca się kodowi twardych ścieżkę do plików. Zamiast tego należy użyć interfejsów API platformy Xamarin.Android, aby określić ścieżkę do plików. W ten sposób interfejsów API platformy .NET do odczytu i zapisu plików uwidacznia natywnych dla systemu Android interfejsów API, które może pomóc w określeniu ścieżki do plików w magazynie wewnętrznych i zewnętrznych.

Przed omówieniem interfejsy API związane z dostępem do pliku, ważne jest zrozumienie niektórych szczegółów otaczającego magazynu wewnętrznych i zewnętrznych. To zostanie dokładnie omówione w następnej sekcji.

## <a name="internal-vs-external-storage"></a>Magazyn zewnętrzny wewnętrznego programu vs

Model pamięci wewnętrznej i zewnętrznej usługi storage są bardzo podobne &ndash; są obu miejscach, w których aplikacji platformy Xamarin.Android może zapisywać pliki. Podobieństwa ten może być mylące dla deweloperów, którzy nie jest zaznajomiony z systemem Android, ponieważ nie jest jasne, gdy aplikacja powinna używać magazynu zewnętrznego vs pamięci wewnętrznej.

Wewnętrzny magazyn odwołuje się do pamięci trwałej Android przydziela do systemu operacyjnego, plików Apk i dla poszczególnych aplikacji. To miejsce nie jest dostępny z wyjątkiem systemu operacyjnego lub aplikacji. Android przyzna katalogu w pamięci wewnętrznej partycji dla każdej aplikacji. Po odinstalowaniu aplikacji również zostaną usunięte wszystkie pliki, które są przechowywane w pamięci wewnętrznej, w tym katalogu. Wewnętrzny magazyn najlepiej nadaje się dla plików, które są dostępne jedynie w aplikacji, i które nie będą udostępniane z innymi aplikacjami lub będzie mieć wartość bardzo mała, gdy aplikacja zostanie odinstalowana. W systemie Android 6.0 lub nowszym, plików w pamięci wewnętrznej mogą być automatycznie do kopii zapasowej za pomocą Google [funkcji Automatyczne kopie zapasowe systemu Android 6.0](https://developer.android.com/guide/topics/data/autobackup). Wewnętrzny magazyn ma następujące wady:

* Nie można współużytkować pliki.
* Pliki zostaną usunięte po odinstalowaniu aplikacji.
* Miejsce dostępne na wewnętrzny magazyn może być ograniczona.

Magazynu zewnętrznego, który odwołuje się do przechowywania plików, który nie jest pamięci wewnętrznej i wyłącznie nie jest dostępny do aplikacji. Głównym celem magazynu zewnętrznego jest zapewnienie miejsce dla plików, które są przeznaczone do udostępnienia między aplikacjami, lub które są zbyt duże, aby umieścić w pamięci wewnętrznej. Korzystać z magazynu zewnętrznego jest zazwyczaj ma znacznie więcej miejsca dla plików niż pamięci wewnętrznej. Jednak magazynu zewnętrznego nie zawsze musi być obecny na urządzeniu i mogą wymagać specjalnego użytkownika do niego uprawnień dostępu.

> [!NOTE]
> W przypadku urządzeń, które obsługują wielu użytkowników systemu Android zapewni każdy użytkownik własnego katalogu magazynu wewnętrznych i zewnętrznych. Ten katalog jest niedostępne dla innych użytkowników na urządzeniu. Ta separacja jest niewidoczny dla aplikacji, tak długo, jak długo wykonują nie umieszczaj ścieżek do plików w magazynie wewnętrzne lub zewnętrzne.

Jako ogólną regułę można przyjąć aplikacji platformy Xamarin.Android powinny Preferuj zapisywanie plików w pamięci wewnętrznej, gdy to stosowne, a polegają na magazynu zewnętrznego, gdy pliki muszą być udostępniane innym aplikacjom, są bardzo duże lub powinny zostać zachowane, nawet jeśli aplikacja zostanie odinstalowana. Na przykład plik konfiguracji jest najlepszym rozwiązaniem w przypadku pamięci wewnętrznej, ponieważ zawiera ona nie znaczenia z wyjątkiem do aplikacji, która tworzy go.  Z kolei zdjęcia są dobrym kandydatem do magazynu zewnętrznego. Mogą być bardzo duże, a w wielu przypadkach użytkownik może chcieć udostępniać je lub uzyskać do nich dostęp, nawet jeśli aplikacja zostanie odinstalowana.

Ten przewodnik koncentruje się na wewnętrzny magazyn. Zobacz przewodnik [magazynu zewnętrznego](~/android/platform/files/external-storage.md) Aby uzyskać szczegółowe informacje na temat korzystania z zewnętrznej usługi storage w aplikacji platformy Xamarin.Android.

## <a name="working-with-internal-storage"></a>Praca z wewnętrznych systemów pamięci masowej

Katalog pamięci wewnętrznej aplikacji jest określany przez system operacyjny i jest narażony na aplikacje dla systemu Android przez `Android.Content.Context.FilesDir` właściwości. Spowoduje to zwrócenie `Java.IO.File` obiekt reprezentujący katalogu, w którym systemu Android jest przeznaczony wyłącznie do aplikacji.  Na przykład aplikacja o nazwie pakietu **com.companyname** katalog wewnętrzny magazyn może być:

```bash
/data/user/0/com.companyname/files
```

W tym dokumencie będzie odnosił się do katalogu pamięci wewnętrznej _wewnętrzne\_MAGAZYNU_.

> [!IMPORTANT]
> Dokładnej ścieżki do katalogu pamięci wewnętrznej może różnić się od urządzenia, urządzenia oraz między wersjami systemu android. W związku z tym aplikacje należy nie ciężko kodu ścieżkę do katalogu magazynu plików wewnętrznych, a zamiast tego użyć interfejsów API platformy Xamarin.Android, takich jak `System.Environment.GetFolderPath()`.

Aby zmaksymalizować, udostępniania kodu, należy używać aplikacji platformy Xamarin.Android (lub aplikacje Xamarin.Forms, przeznaczone dla platformy Xamarin.Android) [ `System.Environment.GetFolderPath()` ](xref:System.Environment.GetFolderPath*) metody. W Xamarin.Android, ta metoda zwróci ciąg dla katalogu, który jest w tej samej lokalizacji co `Android.Content.Context.FilesDir`. Ta metoda przyjmuje wyliczenia `System.Environment.SpecialFolder`, który jest używany do identyfikowania zbiór stałych wyliczeniowych reprezentujących ścieżki do folderów specjalnych używane przez system operacyjny. Nie wszystkie `System.Environment.SpecialFolder` wartości będzie zmapowana do prawidłowego katalogu na platformy Xamarin.Android. W poniższej tabeli opisano, jakie ścieżki można oczekiwać dla danej wartości `System.Environment.SpecialFolder`:

| System.Environment.SpecialFolder | Ścieżka  |
|----------------------|---|
| `ApplicationData` | **_WEWNĘTRZNY\_MAGAZYNU_/.config** |
| `Desktop` | **_WEWNĘTRZNY\_MAGAZYNU_  /Desktop** |
| `LocalApplicationData` | **_WEWNĘTRZNY\_MAGAZYNU_/.local/share** |
| `MyComputer` | **_WEWNĘTRZNY\_MAGAZYNU_/.local/share** |
| `MyDocuments` | **_WEWNĘTRZNY\_MAGAZYNU_** |
| `MyMusic` | **_WEWNĘTRZNY\_MAGAZYNU_/Music** |
| `MyPictures` | **_WEWNĘTRZNY\_MAGAZYNU_/Music** |
| `MyVideos` | **_WEWNĘTRZNY\_MAGAZYNU_/Videos** |
| `Personal` | **_WEWNĘTRZNY\_MAGAZYNU_** |


### <a name="reading-or-writing-to-files-on-internal-storage"></a>Odczyt lub zapis do plików w pamięci wewnętrznej

Jedną z [interfejsów API języka C# do zapisu](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file) w pliku są wystarczające; wszystko, co jest niezbędne jest uzyskanie ścieżki do pliku, który znajduje się w katalogu, przydzielone do aplikacji. Zdecydowanie zaleca się, że asynchroniczne, które są używane wersje interfejsów API platformy .NET, aby zminimalizować problemy, które mogą być skojarzony z dostępu do plików blokowania wątku głównego.

Ten fragment kodu jest przykładem pisania całkowitą do pliku tekstowego UTF-8 do katalogu wewnętrzny magazyn aplikacji:

```csharp
public async Task SaveCountAsync(int count)
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");
    using (var writer = File.CreateText(backingFile))
    {
        await writer.WriteLineAsync(count.ToString());
    }
}
```

W następnym fragmencie kodu zawiera jeden ze sposobów odczytać wartość całkowitą, która została zapisana w pliku tekstowym:

```csharp
public async Task<int> ReadCountAsync()
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");

    if (backingFile == null || !File.Exists(backingFile))
    {
        return 0;
    }

    var count = 0;
    using (var reader = new StreamReader(backingFile, true))
    {
        string line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (int.TryParse(line, out var newcount))
            {
                count = newcount;
            }
        }
    }

    return count;
}
```

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Za pomocą Xamarin.Essentials &ndash; pomocnicy systemu plików

[Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android) to zbiór interfejsów API do pisania kodu zgodne dla wielu platform. [Pomocnicy systemu plików](~/essentials/file-system-helpers.md?context=xamarin/android) to klasa, która zawiera szereg pomocników, aby uprościć lokalizowanie katalogów aplikacji w pamięci podręcznej i danymi. Ten fragment kodu zawiera przykładowy sposób można znaleźć katalogu pamięci wewnętrznej i katalog pamięci podręcznej dla aplikacji:

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>Ukrywanie pliki z `MediaStore`

`MediaStore` To składnik systemu Android, które zbiera metadane dotyczące plików multimedialnych (filmy wideo, muzyka, obrazy) na urządzeniu z systemem Android. Jej celem jest uproszczenie udostępniania tych plików dla wszystkich aplikacji dla systemu Android na urządzeniu.

Pliki prywatne nie będą wyświetlane jako możliwego do udostępniania multimediów. Na przykład, jeśli aplikacja jest zapisywany obraz do jej prywatnego magazynu zewnętrznego, następnie ten plik nie będą pobierane przez skaner nośnika (`MediaStore`).

Pliki publiczne, zostanie pobrana przez `MediaStore`. Katalogi, które mają zero bajtów nazwę pliku **. NOMEDIA** nie będą skanowane przez `MediaStore`.

## <a name="related-links"></a>Linki pokrewne

* [Magazyn zewnętrzny](~/android/platform/files/external-storage.md)
* [Zapisz pliki w pamięci urządzenia](https://developer.android.com/training/data-storage/files)
* [Pomocnicy systemu plików Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android)
* [Dane kopii zapasowej użytkownika z automatycznym tworzeniem kopii zapasowej](https://developer.android.com/guide/topics/data/autobackup)
* [Adoptable magazynu](https://source.android.com/devices/storage/adoptable)
