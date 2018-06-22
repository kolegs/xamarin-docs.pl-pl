---
title: Jak można ręcznie zainstalować biblioteki obsługi systemu Android wymagane przez pakiety Xamarin.Android.Support?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: e760a87cbd1e0220ed5cf3a350d3539ffe29650e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766003"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Jak można ręcznie zainstalować biblioteki obsługi systemu Android wymagane przez pakiety Xamarin.Android.Support?

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Opis procedury Xamarin.Android.Support.v4 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pobierz odpowiednią pakiet Xamarin.Android.Support NuGet (na przykład instalując go z Menedżera pakietów NuGet).

Użyj `ildasm` Aby sprawdzić wersję **android_m2repository.zip** wymaga pakietu NuGet:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```
Przykład danych wyjściowych:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

Pobierz **android\_m2repository.zip** z Google przy użyciu adresu URL zwrócona z **narzędzia ildasm**. Alternatywnie możesz sprawdzić wersję _repozytorium Obsługa systemu Android_ aktualnie zainstalowany w programie Android SDK Manager:

!["Android SDK Manager przedstawiający Android repozytorium Obsługa wersji 32 zainstalowane"](install-android-support-library-images/sdk-extras.png)

Jeśli wersja zgodna ze strukturą, potrzebnych do pakietu NuGet, nie masz wszystkich nowych elementów do pobrania. Użytkownik może zamiast tego ponownie zip istniejące **m2repository** katalogu, w którym znajduje się w obszarze **dodatki\\android** w _ścieżka zestawu SDK_ (jak pokazano na początku Android Okno Menedżera zestawu SDK).

Obliczyć skrótu MD5 adresu URL zwrócone przez **narzędzia ildasm**. Format wynikowy ciąg do użycia wielkie litery i nie może zawierać spacji. Na przykład dopasować `$url` zmiennej jako wymagane, a następnie uruchom następujące wiersze 2 (na podstawie [oryginalny kod C# z platformy Xamarin.Android](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)) w programie PowerShell:

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```
Przykład danych wyjściowych:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

Kopiuj **android\_m2repository.zip** do **LOCALAPPDATA %\\Xamarin\\zips\\**  folderu. Zmień nazwę pliku, aby użyć skrótu MD5 z poprzednich skrótu MD5 obliczanie kroku. Na przykład:

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(Opcjonalnie) Rozpakuj plik do **LOCALAPPDATA %\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\zawartości\\**  (Tworzenie **zawartości\\m2repository** podkatalogu). Jeśli pominiesz ten krok, następnie pierwszej kompilacji, która korzysta z biblioteki potrwa nieco dłużej, ponieważ trzeba będzie wykonać ten krok.
Numer wersji dla podkatalogu (**23.4.0.0** w tym przykładzie) nie jest dość taki sam, jak wersja pakietu NuGet. Można użyć `ildasm` można znaleźć poprawnej wersji numer:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```
Przykład danych wyjściowych:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pobierz odpowiednią pakiet Xamarin.Android.Support NuGet (na przykład instalując go z Menedżera pakietów NuGet).

Kliknij dwukrotnie _Xamarin.Android.Support.v4_ zestawu w _odwołania_ sekcji Projekt systemu Android w programie Visual Studio dla komputerów Mac otworzyć zestaw w przeglądarce zestawu. Upewnij się, że _języka_ ma ustawioną wartość listy rozwijanej _C#_ i wybierz najwyższego poziomu _Xamarin.Android.Support.v4_ zestawu w drzewie nawigacji przeglądarkę zestawu. Zlokalizuj `SourceUrl` właściwości w jednej z `IncludeAndroidResourcesFrom` lub `JavaLibraryReference` atrybuty:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

Pobierz **android\_m2repository.zip** z przy użyciu usługi Google `SourceUrl` zwrócony z **narzędzia ildasm**. Alternatywnie możesz sprawdzić wersję _repozytorium Obsługa systemu Android_ aktualnie zainstalowany w programie Android SDK Manager:

!["Android SDK Manager przedstawiający Android repozytorium Obsługa wersji 32 zainstalowane"](install-android-support-library-images/sdk-extras.png)

Jeśli wersja zgodna ze strukturą, potrzebnych do pakietu NuGet, nie masz wszystkich nowych elementów do pobrania. Użytkownik może zamiast tego ponownie zip istniejące **m2repository** katalogu, w którym znajduje się w obszarze **dodatki języka w systemie android** w _ścieżka zestawu SDK_ (jak pokazano u góry okna Android SDK Manager) .

Obliczyć skrótu MD5 adresu URL zwrócone przez **narzędzia ildasm**. Format wynikowy ciąg do użycia wielkie litery i nie może zawierać spacji. Na przykład dopasować ciągu adresu URL, zgodnie z potrzebami, a następnie uruchom następujące polecenie **Terminal.app** wiersza polecenia:

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

Inną opcją jest użycie `csharp` interpreter do uruchomienia [tego samego języka C# kod, który korzysta z platformy Xamarin.Android sam](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208).
Aby to zrobić, Dostosuj `url` zmiennej jako wymagane, a następnie uruchom następujące polecenie **Terminal.app** wiersza polecenia:

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```
Przykład danych wyjściowych:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

Kopiuj **android\_m2repository.zip** do **$HOME/.local/share/Xamarin/zips/** folderu. Zmień nazwę pliku, aby użyć skrótu MD5 z poprzednich skrótu MD5 obliczanie kroku. Na przykład:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(Opcjonalnie) Rozpakuj plik do: 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(Tworzenie **zawartości/m2repository** podkatalogu). Jeśli pominiesz ten krok, następnie pierwszej kompilacji, która korzysta z biblioteki potrwa nieco dłużej, ponieważ trzeba będzie wykonać ten krok.

Numer wersji dla podkatalogu (**23.4.0.0** w tym przykładzie) nie jest dość taki sam, jak wersja pakietu NuGet. Podobnie jak w **narzędzia ildasm** krok wcześniej, można użyć przeglądarkę zestawu w programie Visual Studio dla komputerów Mac, aby znaleźć poprawnej wersji numer. Wyszukaj `Version` właściwości w jednej z `IncludeAndroidResourcesFrom` lub `JavaLibraryReference` atrybuty:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----


## <a name="additional-references"></a>Dodatkowe informacje

- [Błąd 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) — nieprawdziwą "pobieranie nie powiodło się. Pobierz {0} i umieszcza je w katalogu {1}." i "Zainstaluj pakiet:"{0}"dostępny w Instalatorze zestawu SDK" komunikaty o błędach związanych z Xamarin.Android.Support pakietów

### <a name="next-steps"></a>Następne kroki

W tym dokumencie omówiono bieżące zachowanie począwszy od sierpnia 2016 r. Techniki opisane w tym dokumencie nie jest częścią stabilna suite testowania dla platformy Xamarin, więc może spowodować przerwanie w przyszłości.

Aby uzyskać dodatkową pomoc, skontaktuj się z nami, lub jeśli ten problem pozostanie nawet po użyciu powyższe informacje, zobacz [jakie opcje pomocy technicznej są dostępne dla platformy Xamarin?](~/cross-platform/troubleshooting/support-options.md) informacji o opcjach kontaktu, masz sugestie, oraz jak Plik nowe usterki, w razie potrzeby.

