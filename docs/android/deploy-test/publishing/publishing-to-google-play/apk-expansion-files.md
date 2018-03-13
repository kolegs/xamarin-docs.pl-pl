---
title: "APK rozszerzenia plików"
ms.topic: article
ms.prod: xamarin
ms.assetid: DB7E38E8-3C4E-5191-27EA-22DE63044FE2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3431791d51858df2013634e1594ee960a10728da
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="apk-expansion-files"></a>APK rozszerzenia plików

Niektóre aplikacje (niektóre gier, na przykład) wymagają więcej zasobów i zasoby nie mogą zostać udostępnione w limit rozmiaru maksymalną aplikacji systemu Android określonych przez Google Play. To ograniczenie zależy od wersji programu APK jest przeznaczona dla systemu android:

-  100MB APKs przeznaczonych dla systemu Android 4.0 lub nowszej (poziom interfejsu API 14 lub nowszej).
-  50MB APKs, które target Android 3.2 lub niższy (poziom interfejsu API 13 lub nowszej).

Aby obejść to ograniczenie, Google Play będzie hosta i rozpowszechnić dwa *rozszerzenia plików* do go wraz z APK umożliwia aplikacji pośrednio przekroczenia tego limitu. 

Na większości urządzeń gdy aplikacja jest zainstalowana, pliki rozszerzenia zostaną pobrane wraz z APK i zostanie zapisany w lokalizacji magazynu udostępnionego (karta SD lub partycję instalację USB) na urządzeniu. W kilku starszych urządzeń pliki rozszerzeń nie może automatycznie zainstalować z APK. W takich przypadkach konieczne jest aplikacja zawiera kod, który pobierze pliki rozszerzeń, gdy użytkownik pierwszego uruchomienia aplikacji.

Rozszerzenia plików, są traktowane jako *nieprzezroczyste binarne obiektów blob (obb)* i może być rozmiar do 2 GB. Android nie przetwarzają żadnych specjalnych te pliki po ich pobraniu &ndash; pliki mogą być w dowolnym formacie, który jest odpowiedni dla aplikacji. Koncepcyjnie zalecane podejście do rozszerzenia plików jest następujący:

-   **Rozszerzenia głównego** &ndash; ten plik jest plikiem podstawowym rozszerzeń zasobów i zasoby, które nie mieści się w APK limit rozmiaru. Plik rozszerzenia głównego powinien zawierać głównej zasobów, których aplikacja potrzebuje i rzadko powinien zostać zaktualizowany.
-   **Poprawka rozszerzenia** &ndash; jest przeznaczone dla małych aktualizacji do pliku głównego rozszerzenia. Ten plik może zostać zaktualizowana. Jest odpowiedzialny za aplikacji, aby wykonać wszelkie niezbędne poprawki i aktualizacje z tego pliku.


Rozszerzenia plików, należy przekazać w tym samym czasie jako plik APK zostało przesłane.
Google play nie zezwala na rozszerzenia plików do przekazania do istniejących APK lub w przypadku istniejących APKs aktualizacji. Jeśli należy zaktualizować rozszerzenia pliku, a następnie należy przekazać nowy APK z `versionCode` aktualizacji.


## <a name="expansion-file-storage"></a>Magazyn plików rozszerzenia

Po pobraniu plików na urządzeniu, będą przechowywane w  **_udostępnionego magazynu_/Android/obb/_nazwy pakietu_**:

-   **_Magazyn udostępniony_**  &ndash; to jest katalog określony przez `Android.OS.Environment.ExternalStorageDirectory` .
-   **_Nazwa pakietu_**  &ndash; to nazwa pakietu aplikacji Java styl.


Pobrany rozszerzenia plików nie można przenieść, zmienić, zmieniono jego nazwę ani usuwać z lokalizacji na urządzeniu. Aby to zrobić spowoduje, że pliki rozszerzeń zostać pobrana ponownie, a stare pliki zostaną usunięte. Ponadto katalogu zawierającego plik rozszerzenia powinien zawierać pliki pakietu rozszerzenia.

Rozszerzenia plików oferują nie zabezpieczeń lub ochronę zawartości &ndash; inne aplikacje lub użytkowników może uzyskać dostępu do żadnych plików zapisane w magazynie udostępnionym.

Jeśli jest to niezbędne rozpakować plik rozszerzenia rozpakowane pliki powinny być przechowywane w oddzielnym katalogu, taki jak w `Android.OS.Environment.ExternalStorageDirectory`.

Alternatywą do wyodrębniania plików z pliku rozszerzenia jest do odczytu zasobów lub zasobów bezpośrednio z pliku rozszerzenia. Rozszerzenie pliku jest tylko plik zip, który może być używany z odpowiednią `ContentProvider`. [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary) zawiera zestaw [System.IO.Compression.Zip](https://github.com/mattleibow/Android.Play.ExpansionLibrary/tree/master/System.IO.Compression.Zip), która obejmuje `ContentProvider` , które pozwolą pliku bezpośredniego dostępu do niektórych plików multimedialnych. Jeśli pliki multimedialne są są pakowane w plik zip, wywołania odtwarzania multimediów bezpośrednio może używać plików zip, bez konieczności się rozpakowanie pliku zip. Pliki multimedialne nie można skompresować, po dodaniu do pliku zip. 


### <a name="filename-format"></a>FileName Format

Po pobraniu plików rozszerzenia Google Play użyje następujący plan nazwę rozszerzenia:

    [main|patch].<expansion-version>.<package-name>.obb

Trzy składniki z tego systemu są:

-   `main` lub `patch` &ndash; Określa, czy jest głównym lub poprawka rozszerzenia pliku. Może istnieć tylko jeden.
-   `<expansion-version>` &ndash; To jest liczba całkowita, która odpowiada `versionCode` z APK czy plik po raz pierwszy zostały skojarzone z.
-   `<package-name>` &ndash; Jest to nazwa pakietu aplikacji Java styl.


Na przykład, jeśli wersja APK 21, a nazwa pakietu jest `mono.samples.helloworld`, będzie miała nazwę pliku głównego rozszerzenia **main.21.mono.samples.helloworld**.


## <a name="download-process"></a>Pobierz procesu

Po zainstalowaniu aplikacji ze sklepu Google Play plików rozszerzenia należy pobrany i zapisany wraz z APK. W niektórych sytuacjach ta może się nie powtórzyła lub rozszerzenia plików, które mogą zostać usunięte. Aby obsłużyć ten warunek, aplikacja musi sprawdzić, czy rozszerzenia plików istnieje, a następnie pobrać je, jeśli to konieczne. Poniższy schemat przedstawia zalecane przepływu pracy tego procesu:

[![Schemat blokowy rozszerzenia APK](apk-expansion-files-images/apkexpansion.png)](apk-expansion-files-images/apkexpansion.png#lightbox)

Podczas uruchamiania aplikacji, należy sprawdzić aby zobaczyć, czy odpowiednie rozszerzenie znajdują się na bieżące urządzenie. Jeśli nie, a następnie aplikacji należy wysłać żądanie z witryny Google Play [Licencjonowanie aplikacji](http://developer.android.com/google/play/licensing/index.html). To sprawdzenie jest wykonywane przy użyciu *licencji weryfikacji biblioteki (LVL)*, a jednocześnie bezpłatny i licencjonowanych aplikacji. LVL jest głównie używane przez aplikacje płatną wymuszać ograniczenia licencji. Jednak Google ma rozszerzone LVL, dzięki czemu może służyć także bibliotek rozszerzenia. Bezpłatne aplikacje mają sprawdzania LVL, ale można zignorować ograniczenia licencji. Żądanie LVL jest udostępnia następujące informacje o plikach rozszerzenia, które wymaga aplikacji: 

-   **Rozmiar pliku** &ndash; rozmiaru plików rozszerzeń są używane w ramach kontroli, która określa, czy prawidłowe rozszerzenia plików zostały już pobrane.
-   **Nazwy plików** &ndash; jest to nazwa pliku (na urządzeniu bieżącej) do którego muszą zostać zapisane pakiety rozszerzeń.
-   **Adres URL do pobrania** &ndash; adres URL, które mają być używane do pobierania pakiety rozszerzeń. To jest unikatowy dla każdego pobierania i wygaśnie wkrótce po są dostarczane.


Po wykonaniu operacji wyboru LVL aplikacji powinien pobierać pliki rozszerzeń, biorąc pod uwagę następujące kwestie w ramach pobierania:

-  Urządzenie nie może mieć wystarczającej ilości miejsca do przechowywania plików rozszerzeń.
-  Jeśli sieci Wi-Fi nie jest dostępna, następnie użytkownik powinien mieć możliwość wstrzymać lub anulować pobieranie, aby uniknąć naliczania opłat niepożądanych danych.
-  Rozszerzenia plików zostaną pobrane w tle w celu unikania blokowania interakcji użytkownika.
-  Podczas pobierania przebiega w tle, powinien zostać wyświetlony wskaźnik postępu.
-  Błędów występujących podczas pobierania są bezpiecznie obsłużonych i możliwe do odzyskania.



## <a name="architectural-overview"></a>Omówienie architektury

Po uruchomieniu działania głównego sprawdza czy są pobierane pliki rozszerzeń. Jeśli pliki są pobierane, muszą zostać sprawdzone za ważności.

Jeśli nie zostały pobrane pliki rozszerzeń lub bieżące pliki są nieprawidłowe, można pobrać nowych rozszerzeń plików. Ograniczone usługi zostanie utworzona jako części aplikacji. Po uruchomieniu działania głównego aplikacji, używa usługi ograniczonego sprawdzania przed usługi Google licencjonowania można znaleźć rozszerzenia nazwy pliku i adres URL plików do pobrania. Ograniczone usługi będzie pobierać pliki wątku w tle.

Aby ułatwić działania, wymagane w celu integracji rozszerzenia plików do aplikacji, Google, należy utworzyć kilka bibliotek w języku Java. W bibliotekach są:

-   **Narzędzie do pobierania biblioteki** &ndash; jest biblioteka, która zmniejsza nakład pracy wymagany do integracji rozszerzenia plików w aplikacji. Biblioteki będzie pobierania plików rozszerzenia usługi tła, wyświetlać powiadomienia użytkowników, obsługi problemy z połączeniem sieciowym, wznowić pobieranie itp.
-   **Licencja weryfikacji biblioteki (LVL)** &ndash; biblioteki udostępniania i przetwarzania wywołania metody licencjonowania aplikacji usługi. Można go również posłużyć do wykonania licencjonowania sprawdza, czy aplikacja jest autoryzowany do użycia na urządzeniu.
-   **Biblioteka Zip rozszerzenia APK (opcjonalnie)** &ndash; w przypadku plików rozszerzeń w pliku zip, tej biblioteki będą działać jako dostawca zawartości i zezwolić aplikacji na odczytywanie zasobów i zasoby bezpośrednio z pliku zip, bez konieczności rozwiń zip plik.


Te biblioteki systemie C# i są dostępne w ramach licencji Apache 2.0. Aby szybko zintegrować rozszerzenia plików do istniejącej aplikacji, te biblioteki można dodać do istniejącej aplikacji platformy Xamarin.Android. Kod jest dostępny na [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary) w witrynie GitHub.
