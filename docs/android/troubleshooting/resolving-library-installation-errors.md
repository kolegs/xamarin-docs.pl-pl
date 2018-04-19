---
title: Rozwiązywanie błędów instalacji biblioteki
description: W niektórych przypadkach mogą wystąpić błędy podczas instalowania biblioteki obsługi systemu Android. Ten przewodnik zawiera rozwiązania niektórych typowych błędów.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2018
ms.openlocfilehash: a54c69ff708ff7438ef1a8fd14c17e77b5375039
ms.sourcegitcommit: f52aa66de4d07bc00931ac8af791d4c33ee1ea04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/19/2018
---
# <a name="resolving-library-installation-errors"></a>Rozwiązywanie błędów instalacji biblioteki

_W niektórych przypadkach mogą wystąpić błędy podczas instalowania biblioteki obsługi systemu Android. Ten przewodnik zawiera rozwiązania niektórych typowych błędów._

## <a name="overview"></a>Omówienie

Podczas kompilowania projektu aplikacji platformy Xamarin.Android, mogą wystąpić błędy kompilacji, gdy program Visual Studio lub Visual Studio dla komputerów Mac próbują pobrać i zainstalować zależności bibliotek. Wiele z tych błędów są spowodowane przez problemy z połączeniem sieciowym i uszkodzenie pliku lub problemów, kontroli wersji. W tym przewodniku opisano najbardziej typowe błędy instalacji biblioteki pomocy technicznej i zawiera opis czynności, aby obejść ten problem i ponownie tworzenia projektu aplikacji. 

 
 
## <a name="errors-while-downloading-m2repository"></a>Wystąpiły błędy podczas pobierania m2Repository

Może zostać wyświetlony **m2repository** błędy podczas odwoływania się do pakietu NuGet usługi biblioteki obsługi systemu Android lub Google Play. Komunikat o błędzie jest podobny do następującego:

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

Ten przykład **android\_m2repository\_r16**, jednak może zobaczyć ten sam komunikat o błędzie dla innej wersji, takich jak **android\_m2repository\_r18**  lub **android\_m2repository\_r25**. 



### <a name="automatic-recovery-from-m2repository-errors"></a>Automatyczne odzyskiwanie w razie błędów m2repository 

Często ten problem można naprawić przez usunięcie biblioteki powodować problemy i ponownie skompilować zgodnie z następujące kroki: 

1. Przejdź do katalogu biblioteki pomocy technicznej na komputerze:

    -   W systemie Windows, bibliotek obsługi znajdują się w folderze **C:\\użytkowników\\_username_\\AppData\\lokalnego\\Xamarin**. 

    -   W systemie Mac OS X, znajdują się w pomocy technicznej biblioteki **/Users/_username_/.local/share/Xamarin**. 

2. Zlokalizuj folder biblioteki i wersji odpowiadającej komunikat o błędzie. Na przykład folder biblioteki i wersji powyżej komunikat o błędzie znajduje się w **Android.Support.v4\\22.2.1**:

    [![Przykład lokalizacji folderu dla 22.2.1 biblioteki obsługi](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. Usuń zawartość folderu wersji. Należy usunąć **.zip** plików, jak również **zawartości** i **osadzone** podkatalogów w tym folderze. Przykładowy komunikat o błędzie pokazanym powyżej pliki i podkatalogi pokazano tego zrzutu ekranu (**zawartości**, **osadzone**, i **android_m2repository_r16.zip**) mają usunięte:

    [![Przykład zawartość 22.2.1 obsługuje folder biblioteki](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   Należy pamiętać, że ważne jest, aby usunąć *cały* zawartość tego folderu. Mimo że ten folder początkowo może zawierać "Brak" **android\_m2repository\_r16.zip** plik, ten plik może zostać częściowo pobrane lub uszkodzony.

4. Skompiluj ponownie projekt &ndash; spowodowałoby proces kompilacji do ponownego pobrania Brak biblioteki.

W większości przypadków te kroki Napraw błąd kompilacji i pozwala kontynuować. Usunięcie tej biblioteki nie rozwiąże błąd kompilacji, należy ręcznie pobrać, zainstalować **android\_m2repository\_r_nn_.zip** plików zgodnie z opisem w następnej sekcji. 



### <a name="manually-downloading-m2repository"></a>Ręcznie pobrać m2repository

Jeśli wykonano za pomocą powyższe kroki automatycznego odzyskiwania i nadal występują błędy kompilacji, możesz ręcznie pobrać **android\_m2repository\_r_nn_.zip** plik (przy użyciu przeglądarki sieci web), a następnie zainstaluj go zgodnie z Aby następujące kroki. Ta procedura jest również przydatne, jeśli nie masz dostępu do Internetu na komputerze deweloperskim, ale będą mogli pobrać archiwum na innym komputerze. 

1.  Pobierz **android\_m2repository\_r_nn_.zip** pliku, który odpowiada na komunikat o błędzie &ndash; łącza znajdują się na poniższej liście (wraz z odpowiedniego skrótu MD5 każdego łącza ADRES URL):

    -   [android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    -   [android\_m2repository\_r32.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    -   [android\_m2repository\_r31.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    -   [android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    -   [android\_m2repository\_r29.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    -   [android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    -   [android\_m2repository\_r27.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    -   [android\_m2repository\_r26.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    -   [android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    -   [android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    -   [android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    -   [android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    -   [android\_m2repository\_r21.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    -   [android\_m2repository\_r20.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    -   [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    -   [android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    -   [android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    -   [android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    Jeśli **m2repository** archiwum nie ma w tej tabeli, można utworzyć adres URL pobierania, dołączając **https://dl-ssl.google.com/android/repository/** na nazwę **m2repository** do pobrania. Na przykład użyć  **https://dl-ssl.google.com/android/repository/android \_m2repository\_r10.zip** do pobrania **android\_m2repository\_r10.zip**.

2.  Zmień nazwę pliku do odpowiedniego skrótu MD5 adresu URL pobierania, jak pokazano w powyższej tabeli. Na przykład, jeśli pobrano **android\_m2repository\_r25.zip**, zmienić jego nazwę na **0B3F1796C97C707339FB13AE8507AF50.zip**. Jeśli wartość skrótu MD5 dla adresu URL pobierania pobranego pliku nie jest wyświetlany w tabeli, możesz użyć [online MD5 generator](http://www.webconfs.com/online-md5-generator.php) można przekonwertować na ciąg skrótu MD5 adresu URL. 

3.  Skopiuj plik do Xamarin **zips** folderu: 

    -   W systemie Windows, ten folder znajduje się w **C:\\użytkowników\\***username***\\AppData\\lokalnego\\Xamarin\\zips**. 

    -   W systemie Mac OS X, ten folder znajduje się w **/Users/***username***/.local/share/Xamarin/zips**. 

    Na przykład poniższy zrzut ekranu przedstawia wynik po **android\_m2repository\_r16.zip** jest pobierany i zmieniona na Skrót MD5 jej adres URL pobierania w systemie Windows:

    [![Przykład repozytorium r16.zip nazwa zostanie zmieniona na 0595E577D19D31708195A83087881EE6.zip](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)


Ta procedura nie rozwiąże błąd kompilacji, należy ręcznie pobrać **android\_m2repository\_r_nn_.zip** , rozpakowania go, a następnie zainstaluj jego zawartość, zgodnie z opisem w następnej sekcji. 


### <a name="manually-downloading-and-installing-m2repository-files"></a>Ręczne pobranie i zainstalowanie plików m2repository

Pełni ręczny proces odzyskiwania z **m2repository** powodują błędy pobierania **android\_m2repository\_r_nn_.zip** plik (przy użyciu przeglądarki sieci web), rozpakowywanie plik i kopiowania zawartości do katalogu biblioteki pomocy technicznej na tym komputerze. W poniższym przykładzie firma Microsoft będzie odzyskanie ten komunikat o błędzie: 

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

Wykonaj następujące kroki, aby pobrać **m2repository** i zainstaluj jego zawartość:

1.  Usuń zawartość folderu biblioteki odpowiadający komunikat o błędzie. Na przykład w komunikacie o błędzie powyżej należy usunąć zawartość **C:\\użytkowników\\***username***\\AppData\\lokalnego\\Xamarin\\ Android.Support.v4\\23.1.1.0**. 
    Jak opisano wcześniej, należy usunąć całą zawartość tego katalogu:

    [![Usuwanie zawartości, osadzonych i android_m2repository folderów z 23.1.1.0 folderu](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2.  Pobierz **android\_m2repository\_r_nn_.zip** pliku z Google, umożliwiająca błąd komunikatu (zobacz tabelę w poprzedniej sekcji łączy).

3.  Wyodrębnij to **.zip** archiwum do dowolnej lokalizacji (na przykład komputer stacjonarny). Powinno to utworzenie katalogu, który odpowiada nazwie **.zip** archiwum. W tym katalogu powinien znajdować się podkatalog o nazwie **m2repository**: 

    [![znaleziono w folderze m2repository wyodrębnione archiwum zip](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4.  W katalogu wersji biblioteki wyczyszczone w kroku 1, należy ponownie utworzyć **zawartości** i **osadzone** podkatalogów. Na przykład poniższy zrzut ekranu przedstawia **zawartości** i **osadzone** tworzony w podkatalogach **23.1.1.0** folder **systemu android \_m2repository\_r25.zip**: 

    [![Tworzenie zawartości i foldery osadzony w 23.1.1.0 folderu](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5.  Kopiuj **m2repository** z wyodrębnionego **.zip** do **zawartości** katalogu, który został utworzony w poprzednim kroku: 

    [![Zrzut ekranu przedstawiający m2repository kopiowane do folderu 23.1.1.0/content](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6.  W wyodrębnionego **zip** katalogu, przejdź do **m2repository\\com\\android\\obsługuje\\v4 Obsługa** , a następnie otwórz folder odpowiadającego numer wersji utworzone powyżej (w tym przykładzie **23.1.1**):

    [![Przykład listę plików znajdujących się w folderze support-v4/23.1.1](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7.  Skopiuj wszystkie pliki w tym folderze na **osadzone** katalogu utworzonego w kroku 4:

    [![Przykładowe pliki kopiowane do folderu 23.1.1.0/embedded](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8.  Upewnij się, że wszystkie pliki są kopiowane. **Osadzone** katalogu teraz powinien zawierać pliki takie jak **JAR**, **.aar**, i **.pom**.

9.  Rozpakuj zawartość każdej wyodrębnione **.aar** plików do **osadzone** katalogu. W systemie Windows, dołącz **.zip** rozszerzenia **.aar** , otwórz go, a następnie skopiuj zawartość do **osadzone** katalogu.
    Na macOS, Rozpakuj **.aar** pliku przy użyciu **Rozpakuj** w terminalu (na przykład **Rozpakuj file.aar**).

W tym momencie ręcznie zainstalowano brakujące składniki i projektu powinno utworzyć bez błędów. Jeśli nie, upewnij się, że pobrano **m2repository** **.zip** archiwum wersji, która dokładnie odpowiada wersji w komunikacie o błędzie, a następnie sprawdź, czy zostały zainstalowane jego zawartość w Popraw lokalizacje, zgodnie z opisem w powyższej procedurze. 



## <a name="summary"></a>Podsumowanie 

W tym artykule wyjaśniono, jak odzyskać z typowych błędów, które mogą mieć miejsce podczas automatyczne pobranie i zainstalowanie biblioteki zależności. Jak usunąć biblioteki powodować problemy i skompiluj ponownie projekt, aby ponownie pobrać i zainstalować ponownie biblioteki on opisany. On opisany sposób pobrać bibliotekę, a następnie zainstaluj go w **zips** folderu. On również opisany więcej wysiłku procedurę ręcznego pobierania i instalowania niezbędne pliki jako sposobu obejścia problemów, których nie można rozpoznać za pośrednictwem zautomatyzowany. 
