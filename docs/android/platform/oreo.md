---
title: Funkcje Oreo
description: "Jak rozpocząć korzystanie z platformy Xamarin.Android do opracowywania aplikacji dla systemu android najnowszej wersji."
ms.topic: article
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 478a285dc326b62bf2fc186599bfb7515988f9ee
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="oreo-features"></a>Funkcje Oreo

_Jak rozpocząć korzystanie z platformy Xamarin.Android do opracowywania aplikacji dla systemu android najnowszej wersji._

[Android 8.0 Oreo](https://developer.android.com/index.html) jest najnowsza wersja systemu android dostępnej w sklepie Google. Android Oreo oferuje wiele nowych funkcji środowiska zainteresowań dla deweloperów platformy Xamarin.Android. Te funkcje obejmują kanały powiadomień, powiadomienie identyfikatory, niestandardowych czcionek w XML, do pobrania czcionki, automatyczne uzupełnianie i obrazów na obrazie (PIP). Android Oreo zawiera nowe interfejsy API dla tych nowych capabilties, a te interfejsy API są dostępne dla aplikacji platformy Xamarin.Android używania platformy Xamarin.Android 8.0 i nowszym.

[![Android Oreo bohater obrazu](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png)

W tym artykule ma struktury, aby pomóc Ci rozpocząć w tworzenie aplikacji platformy Xamarin.Android dla systemu Android Oreo 8.0. Wyjaśniono, jak zainstalować niezbędne aktualizacje, konfigurowania zestawu SDK i utworzyć emulatora (lub urządzenia) do testowania. Omówienie nowych funkcji w systemie Android Oreo 8.0, udostępnia również linki do aplikacji przykładowej, które ilustrują sposób korzystania z funkcji systemu Android Oreo w aplikacji platformy Xamarin.Android.


<a name="requirements" />

## <a name="requirements"></a>Wymagania

Poniżej jest wymagany do korzystania z funkcji systemu Android Oreo w aplikacji opartych na platformie Xamarin:

-   **Visual Studio** &ndash; Jeśli używasz systemu Windows w wersji 15.5 lub nowszej programu Visual Studio jest wymagana.  Jeśli używasz Mac programu Visual Studio for Mac wersji 7.2.0 jest wymagana.

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 lub nowszym musi być zainstalowana i skonfigurowana z programem Visual Studio.

-   **Zestaw SDK systemu android** &ndash; Android SDK 8.0 (26 interfejsu API) lub nowszym należy zainstalować za pomocą Menedżera zestawu SDK systemu Android.


<a name="gettingstarted" />

## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć korzystanie z platformy Xamarin.Android Oreo systemu Android, musisz pobrać i zainstalować najnowsze narzędzia i pakietów SDK przed utworzeniem projektu systemu Android Oreo:

1. Aktualizowanie do najnowszej wersji programu Visual Studio.

2. Zainstaluj **8.0.0 systemu Android (interfejs API 26)** lub nowsze pakiety i narzędzia za pośrednictwem Menedżera zestawu SDK.

3. Utwórz nowy projekt platformy Xamarin.Android którego element docelowy Oreo systemu Android (interfejs API 26).

4. Skonfiguruj emulator lub urządzenie na potrzeby testowania aplikacji systemu Android Oreo.

Każdy z tych kroków znajduje się w następujących sekcjach:


<a name="updates" />

### <a name="update-visual-studio-and-xamarinandroid"></a>Aktualizacja programu Visual Studio i platformy Xamarin.Android

Aby dodać obsługę systemu Android Oreo dla programu Visual Studio, wykonaj następujące czynności:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Jeśli używasz programu Visual Studio 2017: 

    1. Aktualizacja programu Visual Studio 2017 wersji 15.5 lub nowszej (zobacz [aktualizacji programu Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/update-visual-studio)).

    2. Użyj Menedżera SDK [instrukcje dotyczące instalacji](~/android/get-started/installation/android-sdk.md#installation) do zainstalowania programu Xamarin SDK Manager.
       Musi być zainstalowany program Xamarin SDK Manager, ponieważ Google nie zapewnia autonomicznym menedżera zestawu SDK z graficznym interfejsem użytkownika, który obsługuje interfejs API 26.0 i nowsze.

-   Jeśli używasz programu Visual Studio 2015, firma Microsoft zaleca się zmiany na starszą wersję narzędzia zestawu SDK do 25 i przy użyciu starego Menedżera Emulator systemu Google graficznego interfejsu użytkownika. Narzędzia SDK 25 nadal można używać razem 26 interfejsu API, 27 i nowszych i nie będzie mieć wpływ na projektowanie dla nowych platform. Zapewni to interfejs do zarządzania z zestawu SDK systemu Android dla wcześniejszych wersji programu VS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Aktualizacja do najnowsza stabilna wersja programu Visual Studio 2017 dla komputerów Mac, zgodnie z objaśnieniem w [aktualizacji programu Visual Studio for Mac](https://docs.microsoft.com/en-us/visualstudio/mac/update).

-----

Aby uzyskać więcej informacji na temat obsługi platformy Xamarin dla systemu Android Oreo, zobacz [wersji Xamarin.Android 8.0](https://developer.xamarin.com/releases/android/xamarin.android_8/xamarin.android_8.0/).


<a name="sdk" />

### <a name="install-the-android-sdk"></a>Zainstaluj zestaw SDK systemu Android

Aby utworzyć projekt z platformy Xamarin.Android 8.0, należy najpierw użyj Xamarin Android SDK Manager do zainstalowania platformy zestawu SDK dla **8.0 dla systemu Android - Oreo** lub nowszym. Należy również zainstalować narzędzia zestawu SDK systemu Android 26.0 lub nowszej.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Uruchom Menedżera SDK (w programie Visual Studio, kliknij przycisk **Narzędzia > Android > Android SDK Manager**).

2. Zainstaluj **8.0 dla systemu Android - Oreo** pakietów. Jeśli używasz emulatora Android SDK, należy uwzględnić **x86** obrazów systemu, które będą potrzebne:

    [![Wybranie pakietów 8.0 dla systemu Android w programie Android SDK Manager](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png)

3. Zainstaluj **narzędzia zestawu SDK systemu Android 26.0.2** lub nowszym, **Android SDK narzędzi platformy 26.0.0** lub nowszym i **Android SDK — narzędzia kompilacji 26.0.0** (lub nowsza):

    [![Wybieranie narzędzia zestawu SDK systemu Android 26 w Menedżerze zestawu SDK systemu Android](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Uruchom Menedżera SDK (w programie Visual Studio for Mac, kliknij przycisk **Narzędzia > SDK Manager**).

2. Zainstaluj **8.0 dla systemu Android - Oreo** pakietów SDK. Jeśli używasz emulatora Android SDK, należy uwzględnić **x86** obrazów systemu, które będą potrzebne:

    [![Wybranie pakietów 8.0 dla systemu Android w Menedżerze zestawu SDK](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png)

3. Zainstaluj **narzędzia zestawu SDK systemu Android 26.0.2** lub nowszym, **Android SDK narzędzi platformy 26.0.0** lub nowszym i **Android SDK — narzędzia kompilacji 26.0.0** (lub nowsza):

    [![Wybieranie narzędzia zestawu SDK systemu Android 26 w Menedżerze zestawu SDK](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png)

-----


<a name="xaproject" />

### <a name="start-a-xamarinandroid-project"></a>Uruchom projekt platformy Xamarin.Android

Utwórz nowy projekt platformy Xamarin.Android. Jeśli jesteś nowym użytkownikiem programowanie dla systemu Android za pomocą platformy Xamarin, zobacz [Hello, Android](~/android/get-started/hello-android/index.md) Aby dowiedzieć się więcej o tworzeniu projektów platformy Xamarin.Android.

Podczas tworzenia projektu systemu Android, należy skonfigurować ustawienia wersji docelowej Android 8.0 lub nowszej. Na przykład, aby skierować je do projektu dla systemu Android 8.0, należy skonfigurować docelowy poziom interfejsu API systemu Android projektu do **8.0 dla systemu Android (interfejs API 26)**. Zalecane jest również ustawić poziom docelowy framework do interfejsu API 26 lub nowszej. Aby uzyskać więcej informacji o konfigurowaniu poziomów poziom interfejsu API systemu Android, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md).

<a name="emudev" />

### <a name="configure-an-emulator-or-device"></a>Skonfiguruj Emulator lub urządzenie

Jeśli próba uruchomienia domyślne Google graficznego interfejsu użytkownika Menedżera AVD po zainstalowaniu systemu Android 26.0 narzędzia zestawu SDK lub później, mogą wystąpić następujące okna dialogowego błędu, który powoduje, że można użyć narzędzia wiersza polecenia AVD manager **avdmanager** zamiast niego :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Okno dialogowe ostrzeżenia menedżera emulatora systemu android](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Okno dialogowe ostrzeżenia menedżera emulatora systemu android](oreo-images/mac/03-avd-warning.png)

-----

Ten komunikat jest wyświetlany, ponieważ Google nie zawiera już autonomicznym Menedżera AVD graficznego interfejsu użytkownika, który obsługuje interfejs API 26.0 lub nowszy. Dla systemu Android Oreo 8.0, należy użyć programu Xamarin Android Emulator Manager lub wiersza polecenia `avdmanager` narzędzia do tworzenia urządzeń wirtualnych dla systemu Android Oreo.

Aby użyć Menedżera urządzeń Xamarin Android tworzenie i zarządzanie nimi urządzeń wirtualnych, zobacz [Menedżera urządzeń Xamarin Android](~/android/get-started/installation/android-emulator/xamarin-device-manager.md).
Aby utworzyć urządzeń wirtualnych bez Xamarin Android Emulator Manager, wykonaj czynności opisane w następnej sekcji.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Tworzenie urządzenia wirtualnego z avdmanager

Aby użyć **avdmanager** Aby utworzyć urządzenie wirtualne, wykonaj następujące kroki:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Otwórz okno wiersza polecenia i ustaw `JAVA_HOME` do lokalizacji zestawu Java SDK na komputerze. W typowej instalacji Xamarin służy następujące polecenie:

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Dodaj lokalizację zestawu SDK systemu Android `bin` folder do Twojej `PATH`.
    W typowej instalacji Xamarin służy następujące polecenie:

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  Zamknij okno wiersza polecenia i otworzyć nowe okno wiersza polecenia. Tworzenie nowego urządzenia wirtualnego za pomocą [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) polecenia. Na przykład, aby utworzyć AVD o nazwie **AVD-Oreo-8.0** przy użyciu x86 obrazu systemu na poziomie interfejsu API 26, użyj następującego polecenia:

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  Po wyświetleniu monitu o **czy chcesz utworzyć profil niestandardowy sprzęt [no]** można wprowadzić **nie** i Zaakceptuj domyślny profil sprzętu. Wówczas **tak**, **avdmanager** zostanie wyświetlony monit z listą zagadnień dostosowywania profilu sprzętu.

Po **avdmanager** Aby utworzyć urządzenie wirtualne, mają być uwzględnieni w menu rozwijanym urządzenia:

[![Nowe AVD dodany do menu rozwijanego urządzenia](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Otwórz **Terminal** okno i zmień lokalizację na lokalizację katalogu narzędzia zestawu SDK systemu Android opartym na systemie W typowej instalacji Xamarin służy następujące polecenie:

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  Tworzenie nowego urządzenia wirtualnego za pomocą [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) polecenia. Na przykład, aby utworzyć AVD o nazwie **AVD-Oreo-8.0** przy użyciu x86 obrazu systemu na poziomie interfejsu API 26, użyj następującego polecenia:

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  Po wyświetleniu monitu o **czy chcesz utworzyć profil niestandardowy sprzęt [no]** można wprowadzić **nie** i Zaakceptuj domyślny profil sprzętu. Wówczas **tak**, **avdmanager** zostanie wyświetlony monit z listą zagadnień dostosowywania profilu sprzętu.

Po użyciu **avdmanager** Aby utworzyć urządzenie wirtualne, mają być uwzględnieni w menu rozwijanym urządzenia:

[![Nowe AVD dodany do menu rozwijanego urządzenia](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png)

-----

Aby uzyskać więcej informacji o konfigurowaniu emulatorze systemu Android do testowania i debugowania, zobacz [emulatora Android SDK](~/android/deploy-test/debugging/android-sdk-emulator/index.md).

Jeśli używasz urządzenia fizycznego, takie jak węzła lub piksel można zaktualizować urządzenie do automatycznego za pośrednictwem aktualizacji lotniczego (Stachnio) lub pobranie obrazu systemu i flash urządzenia bezpośrednio. Aby uzyskać więcej informacji na temat ręcznie zaktualizować urządzenie do Oreo systemu Android, zobacz [obrazy fabryki dla węzła i pikseli urządzenia](https://developers.google.com/android/images).


<a name="newfeatures" />

## <a name="new-features"></a>Nowe funkcje

Android Oreo wprowadzono wiele nowych funkcji i możliwości, takie jak kanały powiadomień, powiadomienie identyfikatory niestandardowych czcionek w kodzie XML, czcionki do pobrania, automatyczne uzupełnianie i obraz w obrazie. W poniższych sekcjach zaznacz te funkcje i znajdują się linki ułatwiające rozpoczęcie pracy z ich w aplikacji.


<a name="notifchan" />

### <a name="notification-channels"></a>Kanały powiadomień

*Kanały powiadomień* zdefiniowana przez aplikację kategorii dla powiadomienia.
Można utworzyć kanał powiadomień dla każdego typu musisz wysłać powiadomienie, a można tworzyć kanały powiadomień w celu odzwierciedlenia wyborów dokonanych przez użytkowników aplikacji. Nowa funkcja kanały powiadomień umożliwia można umożliwić użytkownikom precyzyjną kontrolę nad różnych rodzajów powiadomienia. Na przykład w przypadku wdrażania aplikacji obsługi wiadomości, można utworzyć kanały powiadomień osobne dla każdej grupy konwersacji, który jest tworzony przez użytkownika.

[Kanały powiadomień](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) wyjaśniono, jak utworzyć kanał powiadomień i użyć go do ogłaszania lokalnego powiadomienia. Na przykład rzeczywistych kod, zobacz [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) przykładowa; tej aplikacji przykładowej zarządza dwa kanały i ustawia opcje dodatkowe powiadomień.


<a name="notifbadge" />

### <a name="notification-badges"></a>Identyfikatory powiadomień

Identyfikatory powiadomienia są kropek, które pojawiają się za pośrednictwem ikony aplikacji, jak pokazano w tym zrzut ekranu:

[![Przykład identyfikatory powiadomień na ikony aplikacji](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png)

Te punkty wskazania nowych powiadomień dla co najmniej jeden kanały powiadomień w aplikacji powiązanych z tą ikoną aplikacji &ndash; są powiadomienia, które użytkownik ma nie została jeszcze odrzucone lub reagować. Użytkownicy mogą long-naciśnij pozycję ikonę, aby przegląd na powiadomienia związane z wskaźnika powiadomień odrzuceniu lub działania dotyczące powiadomień, w menu naciśnij long tego appeaars.

Aby uzyskać więcej informacji na temat identyfikatory powiadomień, zobacz deweloperów systemu Android [identyfikatory powiadomień](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) tematu.


<a name="customfonts" />

### <a name="custom-fonts-in-xml"></a>Niestandardowe czcionki w kodzie XML

Wprowadza android Oreo *czcionek w kodzie XML*, co umożliwia można zastosować niestandardowe czcionki jako zasoby. OpenType (**.otf**) i TrueType (**.ttf**) są obsługiwane formaty czcionki. Aby dodać czcionki jako zasoby, wykonaj następujące czynności:

1. Utwórz **zasobów/czcionki** folderu.

2. Skopiuj pliki czcionki (przykład **.ttf** i **.otf** plików) do **zasobów/czcionki**. 

3. W razie potrzeby zmień nazwę każdego pliku czcionki, aby zgodne konwencji nazewnictwa plików z systemem Android (tj. Użyj tylko małe *a-z*, *0-9*i znaki podkreślenia w nazwach plików). Na przykład plik czcionki `Pacifico-Regular.ttf` można było zmienić nazwy podobną `pacifico.ttf`.

4. Zastosuj niestandardowych czcionkę przy użyciu nowej `android:fontFamily` atrybut układu XML. Na przykład następująca `TextView` dodany używa deklaracji **pacifico.ttf** czcionki zasobów:

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Można również utworzyć czcionki rodziny plik XML opisujący wiele czcionek, a także szczegóły styl i wagi. Aby uzyskać więcej informacji, zobacz Android Developer [czcionek w kodzie XML](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) tematu.

<a name="dlfonts" />

### <a name="downloadable-fonts"></a>Czcionki do pobrania

Począwszy od systemu Android Oreo aplikacje mogą żądać czcionki od dostawcy, a nie grupowania ich w plik APK. Czcionki są pobierane z sieci, tylko w razie potrzeby. Ta funkcja pozwala zmniejszyć rozmiar APK, zachowanie telefonu pamięci i sieci komórkowej danych użycia. Umożliwia także tej funkcji na interfejs API systemu Android w wersji 14 lub nowszej, instalując pakiet Android 26 biblioteki pomocy technicznej.

Jeśli Twoja aplikacja powinna czcionkę, Utwórz `FontsRequest` obiektu (Określanie czcionki do pobrania), a następnie przekaż go do `FontsContract` metodę, aby ją pobrać. W poniższych krokach opisano proces pobierania czcionki bardziej szczegółowo:

1.  Utwórz wystąpienie [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) obiektu. 

2.  Podklasy i utworzenia wystąpienia [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3.  Implementowanie [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) metodę, która jest używana do obsługi uzupełniania żądania czcionki.

4.  Implementowanie [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) metodę, która służy do informowania aplikację wszelkie błędy, które mają miejsce podczas procesu żądania czcionki.

5.  Wywołanie [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) metodę, aby pobrać czcionki z czcionek dostawcy. 

Podczas wywoływania `RequestFonts` metody, najpierw sprawdza, czy czcionka jest buforowane lokalnie (z poprzedniego wywołania `RequestFont`). Jeśli nie są buforowane, jego wywołuje metody dostawcy czcionki, asynchronicznie pobiera czcionkę i następnie przekazuje wyniki z powrotem na aplikacji za pomocą programu `OnTypeFaceRetrieved` metody.

[Czcionki do pobrania](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) przykładzie pokazano, jak korzystać z funkcji do pobrania czcionki wprowadzona w systemie Android Oreo. 

Aby uzyskać więcej informacji o pobieraniu czcionek, zobacz deweloperów systemu Android [czcionki do pobrania](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) tematu.


<a name="autofill" />

### <a name="autofill"></a>Automatyczne uzupełnianie

Nowy _Autowypełnianie_ framework w systemie Android Oreo ułatwia użytkownikom obsługi powtarzających się zadań, takich jak logowania, tworzenie konta usługi i transakcje karty kredytowej. Użytkownicy poświęcają mniej czasu ponowne wpisanie informacji (co może prowadzić do błędów wejścia). Zanim aplikacja może działać z Framework Autowypełnianie, należy włączyć usługę automatyczne uzupełnianie w ustawieniach systemu (użytkownicy można włączyć lub wyłączyć automatyczne uzupełnianie).

[AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) przykładzie przedstawiono użycie Framework Autowypełnianie. Obejmuje ona wdrożenia klienta działania z widoków, które powinny być autofilled i usługi, która może zapewnić automatyczne uzupełnianie danych do klienta działań.

Aby uzyskać więcej informacji na temat nowych funkcji automatycznego wypełniania oraz Optymalizowanie aplikacji na automatyczne uzupełnianie Zobacz deweloperów systemu Android [Framework Autowypełnianie](https://developer.android.com/guide/topics/text/autofill.html) tematu.


<a name="pip" />

### <a name="picture-in-picture-pip"></a>Obraz w obrazie (PIP)

Android Oreo umożliwia działania do uruchamiania w trybie (PIP) obraz w obrazie, zastępując ekranu innego działania. Ta funkcja służy do odtwarzania plików wideo.

Aby określić, że działanie aplikacji można użyć trybu PIP, ustaw następujące flagi na wartość true w manifestu systemu Android:

```xml
android:supportsPictureInPicture
```

Aby określić, jak działanie zachowania przypadku, gdy jest w trybie PIP, należy użyć nowego [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) obiektu. `PictureInPictureParams` reprezentuje zestaw parametrów, które służy do inicjowania i aktualizacji działania w trybie PIP (na przykład działania preferowanych współczynnik proporcji). Dodano następujące nowe metody PIP do `Activity` w Android Oreo:

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; umieszcza działania w trybie PIP. Działanie jest umieszczany w róg ekranu, a pozostałe ekranu jest wypełniony przez poprzednie działanie, które były na ekranie.

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; aktualizuje ustawienia konfiguracji PIP działania (na przykład zmiana współczynnika proporcji).

[PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) przykładzie przedstawiono podstawowe sposoby użycia trybu obraz w obrazie (PiP) dla urządzeń przenośnych, które wprowadzono w Oreo. Próbki odtwarzania wideo, który nadal nieprzerwaną podczas i z powrotem przełączanie trybów wyświetlania lub innych działań.


<a name="other" />

### <a name="other-features"></a>Inne funkcje

Android Oreo zawiera wiele innych nowe funkcje, takie jak limity tła biblioteki, lokalizacji interfejsu API, obsługa Emoji, całej gamy kolorów dla aplikacji, nowych audio koderów-dekoderów, ulepszenia widoku sieci Web, Obsługa nawigacji klawiatury ulepszone i nowy interfejs API AAudio (pro audio) dla audio małych opóźnieniach wysokiej wydajności, aby uzyskać więcej informacji o tych funkcjach, zobacz deweloperów systemu Android [Android Oreo funkcje i interfejsy API](https://developer.android.com/about/versions/oreo/android-8.0.html) tematu.


<a name="behavior" />

## <a name="behavior-changes"></a>Zmiany sposobu działania

Android Oreo zawiera szereg systemu i zmiany zachowania interfejsu API, które mogą mieć wpływ na funkcjonalność istniejące aplikacje. Te zmiany są opisane w następujący sposób.

<a name="bgsl" />

### <a name="background-execution-limits"></a>Limity wykonywania tła

Aby ulepszyć środowisko użytkownika, Android Oreo nakłada ograniczenia dotyczące aplikacji, co zrobić podczas działania w tle. Na przykład jeśli użytkownik jest obserwowanie wideo lub granie w gry, aplikacji działającej w tle może zakłócić działanie aplikacji intensywnie wideo uruchomiona na pierwszym planie. W związku z tym Android Oreo wprowadza następujące ograniczenia na aplikacje, które nie są bezpośrednio interakcji z użytkownikiem:

1.  **Ograniczenia usługi w tle** &ndash; aplikacja działa w tle, ma kilka minut, w którym jest nadal dozwolone tworzenia i używania usług okna. Na koniec tego okna, Android zatrzymuje usługi tła aplikacji i traktuje ją jako _bezczynności_.

2.  **Emisji ograniczenia** &ndash; Android 7.0 (25 interfejsu API) dotyczącymi ograniczenia emisji, które rejestruje aplikację do odbierania. Android Oreo sprawia, że te ograniczenia bardziej rygorystyczne. Na przykład aplikacji systemu Android Oreo nie będzie można zarejestrować emisji odbiorniki niejawne emisji w manifestach ich.

Aby uzyskać więcej informacji o nowych limitów wykonywania tła, zobacz deweloperów systemu Android [limity wykonywania tła](https://developer.android.com/about/versions/oreo/background.html) tematu.

<a name="breaking" />

### <a name="breaking-changes"></a>Fundamentalne zmiany

Aplikacje, które są stosowane do systemu Android Oreo lub nowszego zmodyfikować swoje aplikacje do obsługi następujących zmian, jeśli to możliwe:

- Android Oreo traktuje jako przestarzałą możliwości można ustawić priorytet poszczególnych powiadomienia. Zamiast tego poziomu ważności zalecane jest ustawiany podczas tworzenia kanału powiadomień. Poziom ważności, przypisane do kanałów powiadomień ma zastosowanie do wszystkich powiadomień wiadomości, które można post do niego.

- Dla aplikacji przeznaczonych dla systemu Android Oreo `PendingIntent.GetService()` nie działa z powodu nowych limitów dotyczącymi usługi uruchomione w tle. Jeśli ma być przeznaczona dla systemu Android Oreo, należy użyć [PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/) zamiast tego.  

<a name="sample_code" />

## <a name="sample-code"></a>Przykładowy kod

Kilka przykładów Xamarin.Android są dostępne dla pokazano, jak korzystać z funkcji systemu Android Oreo:

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) przedstawiono sposób użycia nowego systemu kanały powiadomień, wprowadzona w systemie Android Oreo. W tym przykładzie zarządza dwa kanały powiadomień: z domyślnego znaczenie i innych o wysokiej ważności.

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) przedstawiono podstawowe sposoby użycia trybu obraz w obrazie (PiP) dla urządzeń przenośnych, które wprowadzono w Oreo. Próbki odtwarzania wideo, który nadal nieprzerwaną podczas i z powrotem przełączanie trybów wyświetlania lub innych działań.

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) zademonstrowano użycie Framework Autowypełnianie. Obejmuje ona wdrożenia klienta działania z widoków, które powinny być autofilled i usługi, która może zapewnić automatyczne uzupełnianie danych do klienta działań.

-   [Do pobrania czcionki](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) zawiera przykładowy sposób korzystania z funkcji do pobrania czcionki opisany wcześniej.

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) przedstawiono użycie EmojiCompat Biblioteka obsługi. Za pomocą tej biblioteki można blokować aplikacji z przedstawiający brakujących znaków emoji jako "tofu" znaków.

-   [Celem oczekujące aktualizacje lokalizacji](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) przedstawiono sposób użycia interfejsu API lokalizacji, aby pobrać aktualizacje o lokalizacji urządzenia przy użyciu `PendingIntent`.

-   [Usługa pierwszego planu aktualizacje lokalizacji](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) pokazano, jak pobrać aktualizacje o lokalizacji do urządzenia za pomocą pierwszego planu powiązane i uruchomiono usługi za pomocą interfejsu API lokalizacji.



<a name="summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzono Android Oreo i wyjaśniono, jak zainstalować i skonfigurować najnowsze narzędzia i pakietów do tworzenia aplikacji platformy Xamarin.Android na Oreo systemu Android. Omówienie najważniejsze funkcje dostępne w Android Oreo go podać wraz z łączami do kodu źródłowego przykład kilka nowych funkcji. Znajduje się ona linki do dokumentacji interfejsu API i deweloperów systemu Android tematy ułatwiające rozpoczęcie pracy podczas tworzenia aplikacji dla systemu Android Oreo. Wyróżnione najważniejsze zmiany zachowania Oreo systemu Android, które może mieć wpływ na istniejące aplikacje.


## <a name="related-links"></a>Linki pokrewne

- [Oreo 8.0 dla systemu android](https://developer.android.com/index.html)
