---
title: Dostęp do plików w pamięci zewnętrznej, za pomocą platformy Xamarin.Android
description: Ten przewodnik omówi dostęp do plików w pamięci zewnętrznej, na platformie Xamarin.Android
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 07/23/2018
ms.openlocfilehash: 380100d38febf567fde94096455fd846d9d3d2d3
ms.sourcegitcommit: 9bb9e8297d3edd9a50585f4ba53c1b4f0bcd1d3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39212155"
---
# <a name="external-storage"></a>Magazyn zewnętrzny

Magazyn zewnętrzny odnosi się do przechowywania plików, który jest nie w pamięci wewnętrznej i wyłącznie nie jest dostępny do aplikacji, która jest odpowiedzialna za pliku. Głównym celem magazynu zewnętrznego jest zapewnienie miejsce dla plików, które są przeznaczone do udostępnienia między aplikacjami, lub które są zbyt duże, aby umieścić w pamięci wewnętrznej.

W przeszłości wypowiedzi magazynu zewnętrznego określonej partycji dysku, na nośniku wymiennym, np. karta SD (znane równie jako _przenośnego urządzenia pamięci masowej_). Ta różnica nie jest już tak istotny usprawniły urządzeń z systemem Android i wielu urządzeń z systemem Android nie obsługują już magazynu wymiennego. Zamiast tego niektóre urządzenia przyzna niektóre z ich wewnętrznej pamięci trwałej Android, które do wykonania tych samych funkcji nośnik wymienny. Jest to nazywane _emulowanej_ magazynu i nadal jest uznawany za magazynu zewnętrznego. Alternatywnie niektóre urządzenia z systemem Android może mieć wiele partycji magazynu zewnętrznego. Na przykład Android tablet (oprócz wewnętrznej pamięci masowej) może być emulowane magazynu i jeden lub więcej miejsc. karta SD. Wszystkie te partycje są traktowane przez system Android magazynu zewnętrznego.

Na urządzeniach, które mają wielu użytkowników każdy użytkownik będzie miał dedykowanych katalogu na partycji głównej zewnętrznej usługi storage dla swojego magazynu zewnętrznego. Aplikacje działające jako jeden użytkownik nie będzie dostępu do plików przez innego użytkownika na urządzeniu. Pliki dla wszystkich użytkowników są nadal czytelny na świecie i świata zapisywalny; Jednak system Android będzie piaskownicy profile wszystkich użytkowników, od innych.

Odczytywanie i zapisywanie w plikach jest niemal identyczne platformie Xamarin.Android, jak wszystkie inne aplikacje platformy .NET. Aplikacji platformy Xamarin.Android Określa ścieżkę do pliku, który będzie można modyfikować, następnie używa standardowych .NET idiomy przy dostępie do plików. Ponieważ rzeczywiste ścieżki do magazynu wewnętrznych i zewnętrznych mogą się różnić od urządzenia, lub z wersji systemu Android do wersji dla systemu Android, nie zaleca się kodowi twardych ścieżkę do plików. Zamiast tego rozszerzenie Xamarin.Android uwidacznia natywnych dla systemu Android interfejsów API, które może pomóc w określeniu ścieżki do plików w magazynie wewnętrznych i zewnętrznych.

Ten przewodnik przedstawimy pojęć i interfejsów API w systemu Android, które są specyficzne dla magazynu zewnętrznego.

## <a name="public-and-private-files-on-external-storage"></a>Pliki publicznych i prywatnych w pamięci zewnętrznej

Istnieją dwa różne typy plików, które aplikacji mogą przechowywać w pamięci zewnętrznej:

* **Prywatne** pliki &ndash; pliki prywatne są pliki, które są specyficzne dla aplikacji (ale są nadal czytelne dla świata z możliwością zapisu świata). Android oczekuje, że pliki prywatne są przechowywane w określonym katalogu w pamięci zewnętrznej. Nawet jeśli pliki są nazywane "private", są one nadal widoczne i dostępne przez inne aplikacje na urządzeniu, nie mają przyznane specjalne ochrony przez system Android.

* **Publiczne** pliki &ndash; są to pliki, które nie są uważane za jest specyficzny dla aplikacji i są przeznaczone do udostępnienia za darmo.

Różnice między tych plików jest w głównej mierze koncepcyjna. Pliki prywatne są prywatne w tym sensie, są wówczas uważane za publiczne pliki są inne pliki, które istnieją w pamięci zewnętrznej, może być częścią grupy aplikacji,. System android oferuje dwa różne interfejsy API rozpoznawania ścieżek do plików prywatnych i publicznych, ale w przeciwnym razie tych samych interfejsów API platformy .NET są używane do odczytu i zapisu do tych plików. Są to tych samych interfejsów API, które zostały omówione w sekcji na [odczytywanie i zapisywanie](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage).

### <a name="private-external-files"></a>Prywatne plików zewnętrznych

Prywatne plików zewnętrznych są traktowane jako specyficzna dla aplikacji (podobne do plików wewnętrznych), ale są przechowywane w pamięci zewnętrznej, dowolna liczba przyczyn (na przykład jest zbyt duży dla magazynu wewnętrznego). Podobnie jak pliki wewnętrzne, te pliki zostaną usunięte, gdy aplikacja zostanie odinstalowana przez użytkownika.

Lokalizacji głównej dla prywatnych zewnętrznych plików zostanie znaleziony, wywołując metodę `Android.Content.Context.GetExternalFilesDir(string type)`. Ta metoda zwróci `Java.IO.File` obiekt, który reprezentuje katalog prywatnej zewnętrznej usługi storage dla aplikacji. Przekazywanie `null` do tej metody zwróci ścieżkę do katalogu magazynu użytkownika dla aplikacji. Na przykład dla aplikacji z nazwą pakietu `com.companyname.app`, będzie katalog "root" prywatnej plików zewnętrznych:

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

W tym dokumencie będzie odnosił się do katalogu magazynu pliki prywatne w pamięci zewnętrznej, jak _PRYWATNEJ\_zewnętrznych\_MAGAZYNU_.


Parametr `GetExternalFilesDir()` jest ciąg, który określa _katalogu aplikacji_. To jest katalog mają na celu dostarczenie standardowe miejsce dla logicznej struktury plików. Wartości ciągu są dostępne za pośrednictwem stałe na `Android.OS.Environment` klasy:

| `Android.OS.Environment` | Katalog |
|-|-|
| DirectoryAlarms | **_PRYWATNE\_zewnętrznych\_MAGAZYNU_  /alarmów** |
| DirectoryDcim | **_PRYWATNE\_ZEWNĘTRZNYCH\_MAGAZYNU_/DCIM** |
| DirectoryDownloads | **_PRYWATNE\_zewnętrznych\_MAGAZYNU_  /Pobierz** |
| DirectoryDocuments | **_PRYWATNE\_zewnętrznych\_MAGAZYNU_  /dokumentów** |
| DirectoryMovies | **_PRYWATNE\_zewnętrznych\_MAGAZYNU_/Movies** |
| DirectoryMusic | **_PRYWATNE\_zewnętrznych\_MAGAZYNU_/Music** |
| DirectoryNotifications | **_PRYWATNE\_zewnętrznych\_MAGAZYNU_/Notifications** |
| DirectoryPodcasts | **_PRYWATNE\_zewnętrznych\_MAGAZYNU_/Podcasts** |
| DirectoryRingtones | **_PRYWATNE\_zewnętrznych\_MAGAZYNU_/Ringtones** |
| DirectoryPictures | **_PRYWATNE\_zewnętrznych\_MAGAZYNU_  /zdjęcia** |

Dla urządzeń, które mają wiele partycji magazynu zewnętrznego każda partycja będzie mieć katalogu, który jest przeznaczony dla pliki prywatne. Metoda `Android.Content.Context.GetExternalFilesDirs(string type)` zwraca tablicę `Java.IO.Files`. Każdy obiekt będzie reprezentować prywatnego katalogu specyficzny dla aplikacji na wszystkich urządzeniach magazyn udostępniony/zewnętrzną, gdzie umieścić pliki aplikacji jest właścicielem.

> [!IMPORTANT]
> Dokładnej ścieżki do katalogu magazynu prywatnego exteral może różnić się od urządzenia, urządzenia oraz między wersjami systemu android. W związku z tym aplikacje muszą nie ciężko kodu ścieżka do tego katalogu i zamiast tego użyć interfejsów API platformy Xamarin.Android, takich jak `Android.Content.Context.GetExternalFilesDir()`.

### <a name="public-external-files"></a>Pliki zewnętrzne publiczne

Pliki publiczne są pliki znajdujące się w pamięci zewnętrznej, które nie są przechowywane w katalogu, który przydziela Android pliki prywatne. Pliki publiczne nie zostaną usunięte, gdy aplikacja zostanie odinstalowana. Aplikacje dla systemu android muszą mieć uprawnienie przed ich może odczytać lub zapisać wszystkie pliki publiczne. Istnieje możliwość, że pliki publiczne istniał w dowolnym miejscu w pamięci zewnętrznej, jednak zgodnie z Konwencją Android oczekuje, że pliki publiczne istnieje w katalogu określonego we właściwości `Android.OS.Environment.ExternalStorageDirectory`. Właściwość ta zwróci `Java.IO.File` obiekt, który reprezentuje katalog podstawowy magazynu zewnętrznego. Na przykład `Android.OS.Environment.ExternalStorageDirectory` mogą odwoływać się do następującego katalogu:

```bash
/storage/emulated/0/
```

W tym dokumencie będzie odnosił się do katalogu magazynu dla publicznych plików w pamięci zewnętrznej, jak _publicznych\_zewnętrznych\_MAGAZYNU_.


System android obsługuje również pojęcie katalogów aplikacji na _publicznych\_zewnętrznych\_MAGAZYNU_. Te katalogi są dokładnie takie same jak diretories aplikacji dla `_PRIVATE\_EXTERNAL\_STORAGE_` i są opisane w tabeli w poprzedniej sekcji. Metoda `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` zwróci `Java.IO.File` obiektu, które odnoszą się do katalogu aplikacji publicznych. `directoryType` Parametr jest obowiązkowy i nie może być `null`.

Na przykład, wywołanie `Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath` zwróci to ciąg, który będzie przypominał:

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> Dokładnej ścieżki do katalogu publicznego magazynu zewnętrznego mogą się różnić od urządzenia, urządzenia oraz między wersjami systemu android. W związku z tym aplikacje muszą nie ciężko kodu ścieżka do tego katalogu i zamiast tego użyć interfejsów API platformy Xamarin.Android, takich jak `Android.OS.Environment.ExternalStorageDirectory`.

## <a name="working-with-external-storage"></a>Praca z zewnętrznej usługi storage

Po uzyskaniu pełną ścieżkę do pliku aplikacji platformy Xamarin.Android go powinien korzystać z wszystkich standardowych interfejsów API platformy .NET do tworzenia, odczytu, zapisu lub usuwania plików. To maksymalizuje stopień cross platform zgodnego kodu dla aplikacji. Jednak przed podjęciem próby uzyskania dostępu do pliku aplikacji platformy Xamarin.Android upewnij się, że można uzyskać dostępu do tego pliku jest.

1. **Sprawdź magazynu zewnętrznego** &ndash; w zależności od charakteru magazynu zewnętrznego, jest to możliwe, że nie może być on zainstalowany i może być używany przez aplikację. Wszystkie aplikacje, należy sprawdzić stan magazynu zewnętrznego, przed podjęciem próby jej używać.
2. **Wykonaj sprawdzenie uprawnień środowiska uruchomieniowego** &ndash; aplikacji dla systemu Android musi żądać uprawnienia od użytkownika w celu uzyskania dostępu do magazynu zewnętrznego. Oznacza to, że czas działania, które uprawnienia żądanie powinno być wykonywane przed dostępu do żadnych plików. Przewodnik [uprawnień w Xamarin.Android](~/android/app-fundamentals/permissions.md) zawiera szczegółowe informacje na temat uprawnień systemu Android.

Każda z tych dwóch zadań zostanie dokładnie omówione poniżej.

### <a name="verifying-that-external-storage-is-available"></a>Weryfikowanie, czy zewnętrznej usługi storage jest dostępna

Pierwszym krokiem przed zapisaniem do magazynu zewnętrznego jest Sprawdź, czy jest do odczytu lub zapisywalnego. `Android.OS.Environment.ExternalStorageState` Właściwości zawiera ciąg, który określa stan magazynu zewnętrznego. Ta właściwość zwraca ciąg, który reprezentuje stan. Ta tabela znajduje się lista `ExternalStorageState` wartości, które mogą być zwrócone przez `Environment.ExternalStorageState`:

| ExternalStorageState | Opis  |
|----------------------|---|
| MediaBadRemoval      | Nagle usunięto nośnik bez właściwie trwająca Dezinstalacja. |
| MediaChecking        | Znajduje się nośnik, ale w trakcie dysku sprawdź.  |
| MediaEjecting        | Nośnik jest w trakcie odinstalować, a wysunięty.  |
| MediaMounted         | Nośnik jest zainstalowany i można je odczytać lub zapisywane.  |
| MediaMountedReadOnly | Nośnik jest zainstalowany, ale mogą być odczytane tylko z. |
| MediaNofs            | Nośnik jest obecny, ale nie zawiera systemem plików, które są odpowiednie dla systemu Android. |
| MediaRemoved         | Brak nośnika. |
| MediaShared          | Nośnik jest obecny, ale nie jest zainstalowany. To są udostępniane przy użyciu kabla USB za pomocą innego urządzenia.|
| MediaUnknown         | Stan nośnika jest nierozpoznana przez system Android. |
| MediaUnmountable     | Nośnik jest zainstalowany, ale nie może być zainstalowany przez system Android. |
| MediaUnmounted       | Nośnik jest zainstalowany, ale nie jest zainstalowany. |


Większość aplikacji systemu Android musi się jedynie do sprawdzania, jeśli jest zainstalowany magazynu zewnętrznego. Poniższy fragment kodu pokazuje, jak sprawdzić, czy Magazyn zewnętrzny jest zainstalowany dostęp tylko do odczytu lub odczytu i zapisu:

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>Uprawnienia do magazynu zewnętrznego

Android uwzględnia, uzyskiwanie dostępu do magazynu zewnętrznego za _niebezpieczne uprawnienia_, co zwykle wymaga użytkownika udzielić uprawnień, ich dostęp do zasobu. Użytkownik może cofnąć to uprawnienie w dowolnym momencie.  Oznacza to, że czas działania, które uprawnienia żądanie powinno być wykonywane przed dostępu do żadnych plików. Aplikacje są automatycznie uprawnień, aby odczytywać i zapisywać swoje własne pliki prywatne. Jest możliwe w przypadku aplikacji odczytywać i zapisywać pliki prywatne, które należą do innych aplikacji, po jego [uprawnienie](~/android/app-fundamentals/permissions.md) przez użytkownika.

Wszystkie aplikacje dla systemu Android musi deklarować jeden z dwóch uprawnień do magazynu zewnętrznego w **AndroidManifest.xml** . Aby zidentyfikować uprawnień, jedną z następujących dwóch `uses-permission` elementów, należy dodać do **AndroidManifest.xml**:

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> Jeśli użytkownik udziela `WRITE_EXTERNAL_STORAGE`, następnie `READ_EXTERNAL_STORAGE` jest również niejawnie przyznane. Nie jest konieczne w celu zażądania uprawnień, zarówno w **AndroidManifest.xml**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Uprawnienia mogą być dodawane przy użyciu **manifestu systemu Android** karcie **właściwości rozwiązania**:

![Eksplorator rozwiązań — wymagane uprawnienia dla programu Visual Studio 2017](./images/required-permissions.w157.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Uprawnienia mogą być dodawane przy użyciu **manifestu systemu Android** karcie **okienku właściwości rozwiązania**:

[![Konsola rozwiązania - wymaganych uprawnień dla programu Visual Studio dla komputerów Mac](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

Ogólnie rzecz biorąc wszystkie niebezpieczne uprawnienia muszą zostać zatwierdzone przez użytkownika. Uprawnienia do magazynu zewnętrznego są anomalii, istnieją wyjątki od tej reguły, w zależności od wersji systemu Android, która aplikacja jest uruchomiona:

![Schemat blokowy sprawdza uprawnienia magazynu zewnętrznego](./images/external-permission-check-flowchart.png)

Aby uzyskać więcej informacji na temat wykonywania żądań uprawnień środowiska uruchomieniowego, sprawdź w przewodniku [uprawnień w Xamarin.Android](~/android/app-fundamentals/permissions.md). **Monodroid-sample** [Pliki_lokalne](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles) również pokazano jeden ze sposobów wykonania testy uprawnienie środowiska uruchomieniowego.

#### <a name="granting-and-revoking-permissions-with-adb"></a>Przyznanie i odwoływanie uprawnień za pomocą ADB

W trakcie opracowywania aplikacji systemu Android, może być konieczne udzielać i odmawiać uprawnień do testowania różnych przepływów pracy związanych z testy uprawnienie środowiska uruchomieniowego. Jest to możliwe, w tym celu w wierszu polecenia przy użyciu ADB. Poniższe fragmenty kodu w wierszu polecenia pokazują, jak udzielić lub odwołać uprawnienia dla aplikacji systemu Android o nazwie pakietu przy użyciu ADB **com.companyname.app**:

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>Usuwanie plików

Standardowe interfejsy API języka C# może służyć do usunięcia pliku z magazynu zewnętrznego, takie jak [ `System.IO.File.Delete` ](xref:System.IO.File.Delete*). Istnieje również możliwość użycia interfejsów API języka Java, kosztem przenośność kodu. Na przykład:

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>Linki pokrewne

* [Pliki lokalne Xamarin.Android przykładowy na **monodroid — przykłady**](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Uprawnienia w platformy Xamarin.Android](~/android/app-fundamentals/permissions.md)
