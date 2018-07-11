---
title: Funkcje Oreo
description: Jak rozpocząć pracę, korzystając z platformy Xamarin.Android do programowania aplikacji dla najnowszej wersji systemu Android.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: mgmclemore
ms.author: mamcle
ms.date: 07/06/2018
ms.openlocfilehash: af560848240fec9558cc63969bcc269eedbd5424
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947289"
---
# <a name="oreo-features"></a>Funkcje Oreo

_Jak rozpocząć pracę, korzystając z platformy Xamarin.Android do programowania aplikacji dla najnowszej wersji systemu Android._

[System android 8.0 Oreo](https://developer.android.com/index.html) jest najnowszą wersją systemu Android dostępną w sklepie Google. Systemu android Oreo oferuje wiele nowych funkcji przydatnych deweloperom platformy Xamarin.Android. Funkcje te obejmują kanały powiadomień, powiadomienie wskaźniki, niestandardowych czcionek w XML, ładowalne, automatyczne uzupełnianie i picture in picture (PIP). System android Oreo zawiera nowe interfejsy API dla tych nowych capabilties i te interfejsy API są dostępne dla aplikacji platformy Xamarin.Android, korzystając z platformy Xamarin.Android, 8.0 i nowsze.

[![Obraz hero w usłudze systemu android Oreo](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

W tym artykule jest struktury, aby pomóc Ci rozpocząć pracę w opracowywaniu aplikacji platformy Xamarin.Android dla systemu Android Oreo 8.0. Pokazuje, jak zainstalować niezbędne aktualizacje i skonfigurować zestaw SDK utworzyć emulatora (lub urządzenia) do testowania. Umożliwia także konspektu nowych funkcji w systemie Android Oreo 8.0, wraz z łączami do przykładowych aplikacji, które ilustrują sposób korzystania z funkcji systemu Android Oreo w aplikacji platformy Xamarin.Android.


## <a name="requirements"></a>Wymagania

Korzystanie z funkcji systemu Android Oreo w aplikacjach opartych na środowisku Xamarin niezbędne jest, następujące elementy:

-   **Program Visual Studio** &ndash; korzystania z Windows w wersji 15.5 lub nowszej programu Visual Studio jest wymagany.  Jeśli używasz komputera Mac, wymagany jest program Visual Studio dla komputerów Mac w wersji 7.2.0.

-   **Platforma Xamarin.Android** &ndash; Xamarin.Android w wersji 8.0 lub nowszej, musi być zainstalowane i skonfigurowane za pomocą programu Visual Studio.

-   **Zestaw SDK systemu android** &ndash; Android SDK 8.0 (interfejs API 26) lub nowszym należy zainstalować za pośrednictwem Menedżera zestawów Android SDK.



## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć pracę, przy użyciu systemu Android Oreo z rozszerzeniem Xamarin.Android, należy pobrać i zainstalować najnowsze narzędzia i pakiety zestawu SDK, zanim będzie można utworzyć projektu systemu Android Oreo:

1. Zaktualizuj do najnowszej wersji programu Visual Studio.

2. Zainstaluj **8.0.0 dla systemu Android (interfejs API 26)** lub nowsze pakiety i narzędzi za pośrednictwem Menedżera zestawów SDK.

3. Utwórz nowy projekt platformy Xamarin.Android przeznaczonych dla systemu Android Oreo (interfejs API 26).

4. Skonfiguruj emulatora lub urządzenia do testowania aplikacji systemu Android Oreo.

Każdy z tych kroków zostało wyjaśnione w poniższych sekcjach:



### <a name="update-visual-studio-and-xamarinandroid"></a>Aktualizacja programu Visual Studio i platformy Xamarin.Android

Aby dodać obsługę systemu Android Oreo do programu Visual Studio, wykonaj następujące czynności:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Jeśli używasz programu Visual Studio 2017: 

    1. Aktualizacja programu Visual Studio 2017 w wersji 15.7 lub nowszej (zobacz [aktualizacji programu Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/update-visual-studio)).

    2. Użyj [menedżera zestawów SDK](~/android/get-started/installation/android-sdk.md) zainstalował poziom interfejsu API 26.0 lub nowszej.

-   Jeśli używasz programu Visual Studio 2015, firma Microsoft zaleca się zmiany na starszą wersję SDK Tools do 25 i przy użyciu starego Menedżera emulatora systemu Google graficznego interfejsu użytkownika. Narzędzia zestawu SDK 25 nadal można używać razem z interfejsu API 26, 27 i nowszych i nie będzie mieć wpływ na rozwój dla nowych platform. Zapewni to interfejs zarządzania zestawu SDK systemu Android dla starszych wersji programu VS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Zaktualizuj do najnowszej stabilnej wersji programu Visual Studio 2017 dla komputerów Mac, jak wyjaśniono w [aktualizowania programu Visual Studio dla komputerów Mac](https://docs.microsoft.com/visualstudio/mac/update).

-----

Aby uzyskać więcej informacji na temat obsługi platformy Xamarin dla systemu Android Oreo, zobacz [informacje o wersji platformy Xamarin.Android 8.0](https://developer.xamarin.com/releases/android/xamarin.android_8/xamarin.android_8.0/).



### <a name="install-the-android-sdk"></a>Instalowanie zestawu SDK systemu Android

Aby utworzyć projekt platformy Xamarin.Android 8.0, należy najpierw użyć platformy Xamarin Android SDK Manager do zainstalowania platformy zestawu SDK dla **system Android 8.0 - Oreo** lub nowszej. Należy również zainstalować Android SDK Tools 26.0 lub nowszej.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Uruchom Menedżer zestawów SDK (w programie Visual Studio, kliknij przycisk **Narzędzia > Android > Menedżer zestawów SDK**).

2. Zainstaluj **system Android 8.0 - Oreo** pakietów. Jeśli korzystasz z emulatora systemu Android SDK, należy uwzględnić **x86** obrazów systemów, które będą potrzebne:

    [![Wybranie pakietów dla systemu Android 8.0 w menedżera zestawów Android SDK](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. Zainstaluj **Android SDK Tools 26.0.2** lub nowszym, **Android SDK narzędzi platformy 26.0.0** lub nowszy, i **Android SDK — narzędzia kompilacji 26.0.0** (lub nowszy):

    [![Wybierając Android SDK Tools 26 Menedżer zestawu Android SDK](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom Menedżer zestawów SDK (w programie Visual Studio dla komputerów Mac, kliknij przycisk **Narzędzia > Menedżer zestawów SDK**).

2. Zainstaluj **system Android 8.0 - Oreo** pakiety zestawu SDK. Jeśli korzystasz z emulatora systemu Android SDK, należy uwzględnić **x86** obrazów systemów, które będą potrzebne:

    [![Wybranie pakietów dla systemu Android 8.0 Menedżer zestawów SDK](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. Zainstaluj **Android SDK Tools 26.0.2** lub nowszym, **Android SDK narzędzi platformy 26.0.0** lub nowszy, i **Android SDK — narzędzia kompilacji 26.0.0** (lub nowszy):

    [![Wybierając Android SDK Tools 26 menedżera zestawów SDK](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Uruchom projekt platformy Xamarin.Android

Utwórz nowy projekt platformy Xamarin.Android. Jeśli jesteś nowym użytkownikiem opracowywania aplikacji systemu Android za pomocą platformy Xamarin, zobacz [Witaj, Android](~/android/get-started/hello-android/index.md) Aby dowiedzieć się więcej o tworzeniu projekty platformy Xamarin.Android.

Kiedy tworzysz projekt systemu Android, należy skonfigurować ustawienia wersji docelowej systemu Android w wersji 8.0 lub nowszej. Na przykład, aby skierować projekcie dla systemu Android 8.0, należy skonfigurować docelowy poziom interfejsu API systemu Android projektu do **system Android 8.0 (interfejs API 26)**. Zalecane jest również ustawienie poziomie struktury docelowej do interfejsu API 26 lub nowszej. Aby uzyskać więcej informacji o konfigurowaniu poziomów poziom interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android w interpretacji](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-an-emulator-or-device"></a>Konfigurowanie emulatora lub urządzenia

Jeśli użytkownik podejmie próbę uruchomienia Google graficznego interfejsu użytkownika Menedżera AVD domyślnie po zainstalowaniu Android SDK Tools 26.0 lub później, mogą wystąpić następujące okno dialogowe błędu, który powoduje, że przy użyciu narzędzia wiersza polecenia AVD manager **avdmanager** zamiast tego :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Okno dialogowe ostrzeżenia menedżera emulatora systemu android](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Okno dialogowe ostrzeżenia menedżera emulatora systemu android](oreo-images/mac/03-avd-warning.png)

-----

Ten komunikat jest wyświetlany, ponieważ firmy Google nie zawiera już autonomiczne Menedżera AVD graficznego interfejsu użytkownika, który obsługuje interfejs API 26.0 lub nowszy. Dla systemu Android Oreo 8.0, należy użyć Menedżera emulatorów systemu Android platformy Xamarin lub wiersza polecenia `avdmanager` narzędzia do tworzenia urządzeń wirtualnych dla systemu Android Oreo.

Aby użyć Menedżera urządzeń Android do tworzenia i Zarządzaj urządzeniami wirtualnymi, zobacz [Zarządzanie urządzeń wirtualnych przy użyciu Menedżera urządzeń Android](~/android/get-started/installation/android-emulator/device-manager.md).
Aby utworzyć urządzeń wirtualnych bez Menedżer urządzeń Android, wykonaj kroki opisane w następnej sekcji.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Tworzenie przy użyciu urządzeń wirtualnych avdmanager

Aby użyć **avdmanager** Aby utworzyć urządzenie wirtualne, wykonaj następujące kroki:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Otwórz okno wiersza polecenia i ustaw `JAVA_HOME` lokalizację zestawu Java SDK na tym komputerze. W typowej instalacji Xamarin można użyć następującego polecenia:

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Dodaj lokalizację zestawu Android SDK `bin` folder do Twojej `PATH`.
    W typowej instalacji Xamarin można użyć następującego polecenia:

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  Zamknij okno wiersza polecenia i Otwórz nowe okno wiersza polecenia. Tworzenie nowego urządzenia wirtualnego przy użyciu [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) polecenia. Na przykład, aby utworzyć AVD o nazwie **AVD-Oreo — 8.0** przy użyciu x86 obraz systemu dla poziomu interfejsu API 26, użyj następującego polecenia:

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  Po wyświetleniu monitu o **czy chcesz utworzyć profil sprzętu niestandardowe [Brak]** można wprowadzić **nie** i Zaakceptuj domyślny profil sprzętu. Jeśli wybierzesz opcję **tak**, **avdmanager** zostanie wyświetlony monit z listą pytań dotyczących dostosowywania profilu sprzętu.

Po zakończeniu **avdmanager** Aby utworzyć urządzenie wirtualne, zostaną uwzględnione w menu rozwijanym urządzenia:

[![Nowe AVD dodany do menu rozwijanego urządzenia](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Otwórz **terminalu** okna i zmień lokalizację na lokalizację katalogu narzędzi zestawu SDK systemu Android na komputerze Mac. W typowej instalacji Xamarin można użyć następującego polecenia:

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  Tworzenie nowego urządzenia wirtualnego przy użyciu [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) polecenia. Na przykład, aby utworzyć AVD o nazwie **AVD-Oreo — 8.0** przy użyciu x86 obraz systemu dla poziomu interfejsu API 26, użyj następującego polecenia:

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  Po wyświetleniu monitu o **czy chcesz utworzyć profil sprzętu niestandardowe [Brak]** można wprowadzić **nie** i Zaakceptuj domyślny profil sprzętu. Jeśli wybierzesz opcję **tak**, **avdmanager** zostanie wyświetlony monit z listą zagadnień dotyczących dostosowywania profilu sprzętu.

Po użyciu **avdmanager** Aby utworzyć urządzenie wirtualne, zostaną uwzględnione w menu rozwijanym urządzenia:

[![Nowe AVD dodany do menu rozwijanego urządzenia](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

Aby uzyskać więcej informacji o konfigurowaniu emulatora systemu Android do testowania i debugowania, zobacz [debugowanie w emulatorze systemu Android](~/android/deploy-test/debugging/debug-on-emulator.md).

Jeśli używasz urządzenia fizycznego, takiego jak Nexus lub piksel, można zaktualizować urządzenie za pośrednictwem automatyczne za pośrednictwem aktualizacji air (Stachnio) lub pobranie obrazu systemu i flash urządzenia bezpośrednio. Aby uzyskać więcej informacji na temat ręcznego aktualizowania urządzenia do systemu Android Oreo zobacz [obrazów fabryki dla węzła i pikseli urządzenia](https://developers.google.com/android/images).



## <a name="new-features"></a>Nowe funkcje

System android Oreo wprowadzono wiele nowych funkcji i możliwości, takie jak kanały powiadomień, wskaźniki powiadomień, niestandardowych czcionek w formacie XML, ładowalne, automatyczne uzupełnianie i obraz w obrazie. Poniższe sekcje pokazują następujące funkcje i zapewniają łącza, aby rozpocząć pracę, ich użycie w swojej aplikacji.



### <a name="notification-channels"></a>Kanały powiadomień

*Kanały powiadomień* kategorii zdefiniowanych przez aplikację, dla powiadomienia.
Można utworzyć kanał powiadomień dla każdego typu musisz wysłać powiadomienie, i mogą tworzyć kanały powiadomień, aby odzwierciedlić wyborów dokonanych przez użytkowników aplikacji. Nowa funkcja kanały powiadomień umożliwia użytkownikom precyzyjną kontrolę nad różnego rodzaju powiadomienia. Na przykład w przypadku wdrażania aplikacji obsługi wiadomości, można utworzyć oddzielne powiadomienie kanały dla każdej grupy konwersacji, który jest tworzony przez użytkownika.

[Kanały powiadomień](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) wyjaśnia sposób tworzenia kanału powiadomień i użyć go do ogłaszania powiadomień lokalnych. Na przykład kodu w rzeczywistych warunkach, zobacz [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) przykładowe; Ta przykładowa aplikacja zarządza dwa kanały i ustawia opcje dodatkowe powiadomienia.



### <a name="notification-badges"></a>Identyfikatory powiadomień

Identyfikatory powiadomień są małe punkty, które pojawiają się za pośrednictwem ikony aplikacji, jak pokazano w tym zrzucie ekranu:

[![Przykład wskaźniki powiadomień na ikony aplikacji](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

Te punkty oznaczają, że istnieją nowe powiadomienia dla co najmniej jednego kanału powiadomień w aplikacji powiązanych z tą ikoną aplikacji &ndash; są powiadomienia, które użytkownik jeszcze nie zostało odrzucone lub nie podjęte. Użytkownicy mogą long naciśnij ikonę do Uzyskaj wgląd w powiadomienia związane ze wskaźnikiem powiadomień, odrzucanie lub działające na powiadomień z poziomu menu naciśnij długo ten appeaars.

Aby uzyskać więcej informacji na temat powiadomień wskaźniki Zobacz dewelopera systemu Android [wskaźniki powiadomień](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) tematu.



### <a name="custom-fonts-in-xml"></a>Czcionki niestandardowe w pliku XML

Wprowadzono w systemie android Oreo *czcionki w pliku XML*, co czyni go można zastosować niestandardowe czcionki jako zasoby. OpenType (**.otf**) i TrueType (**.ttf**) czcionki formaty są obsługiwane. Aby dodać czcionki jako zasoby, wykonaj następujące czynności:

1. Tworzenie **zasobów/czcionki** folderu.

2. Skopiuj pliki czcionek (przykład **.ttf** i **.otf** plików) do **zasobów/czcionki**. 

3. Jeśli to konieczne, Zmień nazwę każdego pliku czcionki tak, aby zgodne konwencji nazewnictwa plików dla systemu Android (czyli Użyj tylko małych *a – z*, *0-9*i znaki podkreślenia w nazwach plików). Na przykład plik czcionki `Pacifico-Regular.ttf` można zmienić nazwy na wartość podobną `pacifico.ttf`.

4. Zastosowanie niestandardowej czcionki za pomocą nowego `android:fontFamily` atrybut w XML układu. Na przykład następująca `TextView` deklaracji używa dodany **pacifico.ttf** zasobów czcionki:

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Można również utworzyć czcionki rodziny pliku XML, który opisuje wiele czcionek, a także styl i grubość, uzyskać szczegółowe informacje. Aby uzyskać więcej informacji, zobacz dewelopera systemu Android [czcionek w kodzie XML](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) tematu.


### <a name="downloadable-fonts"></a>Czcionki do pobrania

Począwszy od systemu Android Oreo aplikacji, można zażądać czcionki od dostawcy, a nie tworzenia ich pakietów do zestawu APK. Czcionki są pobierane z sieci, stosownie do potrzeb. Ta funkcja zmniejsza rozmiar pliku APK, zachowanie telefonu pamięć i sieć komórkowa użycia danych. Umożliwia także tę funkcję na temat interfejsu API systemu Android w wersji 14 lub nowszej, instalując pakiet Android 26 biblioteki pomocy technicznej.

Kiedy Twoja aplikacja wymaga czcionkę, tworzysz `FontsRequest` obiektu (Określanie czcionki, aby pobrać), a następnie przekazać go do `FontsContract` metodę, aby ją pobrać. W poniższych krokach opisano proces pobierania czcionki bardziej szczegółowo:

1.  Utwórz wystąpienie [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) obiektu. 

2.  Podklasy i tworzyć wystąpienia [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3.  Implementowanie [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) metody, która jest używana do obsługi zakończenia żądania czcionki.

4.  Implementowanie [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) metody, która służy do informowania aplikacji wszelkie błędy, które mają miejsce podczas procesu żądania czcionki.

5.  Wywołaj [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) metodę, aby pobrać czcionki od dostawcy czcionki. 

Gdy wywołujesz `RequestFonts` metody, najpierw sprawdza, jeśli czcionka jest buforowany lokalnie (z poprzedniego wywołania `RequestFont`). Jeśli nie są buforowane, wówczas wywołuje dostawcę czcionki, asynchronicznie pobiera czcionki i następnie przekazuje wyniki wróć do aplikacji za pomocą wywołania usługi `OnTypeFaceRetrieved` metody.

[Ładowalne](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) przykład pokazuje, jak za pomocą funkcji ładowalne wprowadzona w systemie Android Oreo. 

Aby uzyskać więcej informacji na temat pobierania czcionek, zobacz dewelopera systemu Android [ładowalne](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) tematu.



### <a name="autofill"></a>Automatyczne uzupełnianie

Nowy _Autowypełnianie_ struktura w systemie Android Oreo ułatwia użytkownikom obsługa powtarzających się zadań, takich jak logowanie, tworzenie konta usługi i transakcje kart kredytowych. Użytkownicy poświęcają mniej czasu, ponownego wpisywania informacji (co może prowadzić do wprowadzenia błędów). Zanim aplikacji może pracować w ramach automatycznego uzupełniania, usługi Autowypełnianie musi być włączona w ustawieniach systemowych (użytkowników można włączyć lub wyłączyć automatyczne wypełnianie).

[AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) w przykładzie pokazano użycie w ramach automatycznego uzupełniania. Obejmuje ona wdrożenia klienta działań za pomocą widoków, które powinny być autofilled i to usługa, która umożliwia automatyczne wypełnianie danych klienta działań.

Aby uzyskać więcej informacji o nowej funkcji automatycznego uzupełniania i zoptymalizować aplikację, aby uzyskać automatyczne uzupełnianie, zobacz dewelopera systemu Android [Autowypełnianie](https://developer.android.com/guide/topics/text/autofill.html) tematu.



### <a name="picture-in-picture-pip"></a>Picture in Picture (PIP)

Dla systemu android Oreo umożliwia dla działania do uruchamiania w trybie (PIP) obraz w obrazie nałożenie ekranu kolejnego działania. Ta funkcja jest przeznaczonych do odtwarzania wideo.

Aby określić, że działanie aplikacji można użyć trybu PIP, ustaw następujące flagi na wartość true w manifeście systemu Android:

```xml
android:supportsPictureInPicture
```

Aby określić, jak działania powinny zachowywać się w trybie PIP, możesz korzystać z nowego [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) obiektu. `PictureInPictureParams` reprezentuje zestaw parametrów, które służy do inicjowania i aktualizacji działania w trybie PIP (na przykład działania preferowanych proporcje). Dodano następujące nowe metody PIP `Activity` w systemie Android Oreo:

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; umieszcza działania w trybie PIP. Działania jest umieszczany w rogu ekranu, a pozostała część ekranu jest wypełniany poprzedniego działania, który został na ekranie.

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; aktualizuje ustawienia konfiguracji narzędzia PIP działania (na przykład zmień współczynnik proporcji).

[PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) przykład pokazuje podstawowe użycie trybu obraz w obrazie (PiP) dla urządzeń przenośnych, wprowadzona w Oreo. Próbki jest odtwarzany film wideo, który będzie nieprzerwaną podczas przełączania do i z powrotem między trybów wyświetlania lub innych działań.



### <a name="other-features"></a>Inne funkcje

System android Oreo zawiera wiele nowych funkcji takich jak ograniczeń tła Emoji obsługi biblioteki, API lokalizacji, kolor całej gamy aplikacji, nowe kodery-dekodery audio, ulepszenia WebView, klawiatura Ulepszona obsługa nawigacji i nowego interfejsu API AAudio (pro audio) dla dźwięk o małych opóźnieniach o wysokiej wydajności, aby uzyskać więcej informacji o tych funkcjach, zobacz dewelopera systemu Android [dla systemu Android Oreo funkcjach i interfejsach API](https://developer.android.com/about/versions/oreo/android-8.0.html) tematu.



## <a name="behavior-changes"></a>Zmiany zachowania

Dla systemu android Oreo oferuje różnorodne systemu oraz zmiany zachowania interfejsu API, które mogą mieć wpływ na funkcjonalność istniejących aplikacji. Zmiany te są opisane w następujący sposób.


### <a name="background-execution-limits"></a>Granice wykonania tła

Aby ulepszyć środowisko użytkownika, systemu Android Oreo nakłada ograniczenia na możliwościach aplikacji podczas uruchamiania w tle. Na przykład jeśli użytkownik jest oglądania filmu wideo lub granie w gry, aplikację uruchomioną w tle może zakłócić działanie aplikacji intensywnie korzystających z filmu wideo, uruchomiona na pierwszym planie. W rezultacie systemu Android Oreo wprowadza następujące ograniczenia na aplikacje, które nie wchodzą w bezpośrednie interakcje z użytkownikiem:

1.  **Ograniczenia dotyczące usługi w tle** &ndash; gdy aplikacja jest uruchomiona w tle, ma ona okna, w kilka minut, w którym nadal może on być tworzenia i używania usług. Na końcu tego okna, zatrzymuje usługę tło w aplikacji systemu Android i traktuje ją jako _bezczynności_.

2.  **Emisji ograniczenia** &ndash; Android 7.0 (interfejsu API 25) umieszczone ograniczenia na emisje, których aplikacja została zarejestrowana do odbierania. System android Oreo sprawia, że te ograniczenia bardziej rygorystycznych. Na przykład aplikacji systemu Android Oreo nie będzie można zarejestrować emisji odbiorniki emisji niejawne w swoich manifestach.

Aby uzyskać więcej informacji na temat nowych limitów wykonywania tła Zobacz dewelopera systemu Android [granice wykonania tła](https://developer.android.com/about/versions/oreo/background.html) tematu.


### <a name="breaking-changes"></a>Fundamentalne zmiany

Aplikacje, których platformą docelową jest system Android Oreo lub nowszym należy zmodyfikować swoje aplikacje do obsługi następujących zmian, jeśli ma to zastosowanie:

- System android Oreo traktuje jako przestarzałą możliwości można ustawić priorytet poszczególnych powiadomienia. Zamiast tego ustawiamy poziom ważności zalecane podczas tworzenia kanału powiadomień. Poziom ważności, które można przypisać do kanału powiadomień ma zastosowanie do wszystkich komunikatów powiadomień, gdy opublikujesz, które do niej.

- Dla aplikacji przeznaczonych dla systemu Android Oreo `PendingIntent.GetService()` nie działa z powodu ograniczeń nowej usługi uruchomione w tle. Jeśli są przeznaczone dla systemu Android Oreo, należy użyć [PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/) zamiast tego.  


## <a name="sample-code"></a>Przykładowy kod

Przykłady rozszerzenia Xamarin.Android kilka są dostępne dla pokazują, jak korzystać z funkcji systemu Android Oreo:

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) pokazuje, jak używać nowego systemu kanały powiadomień, wprowadzona w systemie Android Oreo. W tym przykładzie zarządza dwa kanały powiadomień: jeden z domyślną znaczenie, a druga o wysokiej ważności.

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) pokazuje podstawowe użycie trybu obraz w obrazie (PiP) dla urządzeń przenośnych, wprowadzona w Oreo. Próbki jest odtwarzany film wideo, który będzie nieprzerwaną podczas przełączania do i z powrotem między trybów wyświetlania lub innych działań.

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) zademonstrowano użycie w ramach automatycznego uzupełniania. Obejmuje ona wdrożenia klienta działań za pomocą widoków, które powinny być autofilled i to usługa, która umożliwia automatyczne wypełnianie danych klienta działań.

-   [Ładowalne](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) zawiera przykładowy sposób użycia funkcji ładowalne wcześniejszym opisem.

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) pokazuje sposób użycia EmojiCompat Biblioteka Pomocy. Przy użyciu tej biblioteki, aby uniemożliwić aplikacji na podstawie przedstawiający brakujących emoji znaków jako znaki "tofu".

-   [Lokalizacja aktualizacji oczekiwanie zamierzone](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) ilustruje sposób użycia interfejsu API lokalizacji, aby pobrać aktualizacje o lokalizacji urządzenia przy użyciu `PendingIntent`.

-   [Usługi pierwszego planu aktualizacje lokalizacji](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) ilustruje sposób korzystania z interfejsu API lokalizacji w celu pobrania aktualizacji informacje o lokalizacji urządzenia przy użyciu usługi powiązane i wprowadzenie pierwszego planu.


## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**Programistycznymi systemu android 8.0 Oreo przy użyciu języka C#**


## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono systemu Android Oreo i wyjaśniono, jak zainstalować i skonfigurować najnowsze narzędzia i pakiety do tworzenia aplikacji platformy Xamarin.Android w systemie Android Oreo. Przegląd kluczowych funkcji dostępnych w systemie Android Oreo, ona udostępniana wraz z łączami do przykładowym kodzie źródłowym, aby uzyskać kilka nowych funkcji. Zawierała linki do dokumentacji interfejsu API i tematy dewelopera systemu Android, aby pomóc Ci rozpocząć tworzenie aplikacji dla systemu Android Oreo. Wyróżnione najważniejsze zmiany zachowania systemu Android Oreo, które może mieć wpływ na istniejące aplikacje.


## <a name="related-links"></a>Linki pokrewne

- [System android 8.0 Oreo](https://developer.android.com/index.html)
