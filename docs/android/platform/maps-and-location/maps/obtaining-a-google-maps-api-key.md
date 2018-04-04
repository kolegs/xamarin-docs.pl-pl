---
title: Uzyskiwanie Google mapuje klucz interfejsu API
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c37fce491b2e6f5e0211fcc6aa7906643a1bac2a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="obtaining-a-google-maps-api-key"></a>Uzyskiwanie Google mapuje klucz interfejsu API

Aby korzystać z funkcji map programu Google w systemie Android, należy zarejestrować dla klucza interfejsu API map z serwisem Google. Przed wykonaniem to widoczne tylko pusta siatka zamiast mapę w aplikacji. Należy uzyskać klucz interfejsu API systemu Android map Google v2 — kluczy z starsze v1 klucza interfejsu API systemu Android map Google nie będą działać.

Uzyskanie klucza interfejsu API map v2 obejmuje następujące kroki:

1.  Pobieranie odcisku palca algorytmu SHA-1 z magazynu kluczy, który został użyty do podpisania aplikacji.
2.  Utwórz projekt w konsoli interfejsów API firmy Google.
3.  Uzyskiwanie klucz interfejsu API.


## <a name="obtaining-your-signing-key-fingerprint"></a>Uzyskiwanie odcisku palca klucza podpisywania

Aby zrealizować klucz interfejsu API map z Google, musisz znać odcisku palca algorytmu SHA-1 z magazynu kluczy, który został użyty do podpisania aplikacji.
Zwykle oznacza to, będzie musiał określić odcisku palca algorytmu SHA-1 dla magazynu kluczy debugowania, a następnie SHA-1 odcisków palców dla magazynu kluczy, który został użyty do podpisania aplikacji w wersji.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Domyślnie magazynu kluczy, który jest używany do podpisywania wersje do debugowania Xamarin.Android, aplikacja może być znalezione w następującej lokalizacji:

**C:\\użytkowników\\[USERNAME]\\AppData\\lokalnego\\Xamarin\\Mono dla systemu Android\\debug.keystore**

Informacje o magazynie kluczy są uzyskiwane przez uruchomienie `keytool` polecenie zestaw JDK. To narzędzie jest zwykle znajdują się w katalogu bin języka Java:

**C:\\Program Files (x86)\\Java\\jdk [wersja]\\bin\\keytool.exe**

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Domyślnie magazynu kluczy, który jest używany do podpisywania wersje do debugowania Xamarin.Android, aplikacja może być znalezione w następującej lokalizacji:

**/Users/[USERNAME]/.Local/share/Xamarin/mono dla Android/debug.keystore**

Informacje o magazynie kluczy są uzyskiwane przez uruchomienie `keytool` polecenie zestaw JDK. To narzędzie jest zwykle znajdują się w katalogu bin języka Java:

**/System/Library/Java/JavaVirtualMachines/[Version].JDK/Contents/Home/bin/keytool**

-----


Uruchom narzędzie klucza za pomocą następującego polecenia (przy użyciu ścieżek plików pokazanym powyżej):

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Przykład Debug.KeyStore

Klucz debugowania domyślne (który jest automatycznie tworzony dla debugowania) Użyj tego polecenia:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----


### <a name="production-keys"></a>Kluczy produkcji

W przypadku wdrażania aplikacji do witryny Google Play, musi być [podpisany przy użyciu klucza prywatnego](~/android/deploy-test/signing/index.md).
`keytool` Można uruchomić szczegółów klucza prywatnego i używany do tworzenia klucza interfejsu API map Google produkcji wynikowy odcisku palca algorytmu SHA-1. Pamiętaj, aby zaktualizować **AndroidManifest.xml** pliku przy użyciu poprawnego klucza interfejsu API map Google, przed przystąpieniem do wdrożenia.

### <a name="keytool-output"></a>Dane wyjściowe Keytool

Powinien zostać wyświetlony ekran podobny do następujących danych wyjściowych w oknie konsoli:

```shell
Alias name: androiddebugkey
Creation date: Jan 01, 2016
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 4aa9b300
Valid from: Mon Jan 01 08:04:04 UTC 2013 until: Mon Jan 01 18:04:04 PST 2033
Certificate fingerprints:
    MD5:  AE:9F:95:D0:A6:86:89:BC:A8:70:BA:34:FF:6A:AC:F9
    SHA1: BB:0D:AC:74:D3:21:E1:43:07:71:9B:62:90:AF:A1:66:6E:44:5D:75
    Signature algorithm name: SHA1withRSA
    Version: 3
```

Użyje odcisku palca algorytmu SHA-1 (wymienionych po **SHA1**) dalszej części tego przewodnika.


## <a name="creating-an-api-project"></a>Tworzenie projektu interfejsu API

Po pobraniu odcisku palca algorytmu SHA-1 z podpisywania magazynu kluczy jest wymagane do utworzenia nowego projektu w konsoli interfejsów API firmy Google (lub Dodaj usługę interfejsu API Google Maps systemu Android w wersji 2 do istniejącego projektu).

1. W przeglądarce przejdź do [konsoli deweloperów Google](https://console.developers.google.com/): i kliknij przycisk **tworzenia projektu**:

   [![Przycisk tworzenia projektu Google Developer konsoli](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. W **nowy projekt** okno dialogowe zostanie wyświetlone, wprowadź nazwę projektu.
   Okno dialogowe będzie produkcji Identyfikatora unikatowego projektu, który jest oparta na nazwę projektu, jak pokazano w poniższym przykładzie:

   [![Nowy projekt o nazwie XamarinMapsDemo](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. Kliknij przycisk **Utwórz** przycisku. Po minucie lub to, projekt zostanie utworzony i zostają przeniesieni do **Menedżer interfejsu API** strony. W **biblioteki** kliknij **interfejsu API systemu Android map Google**:

   [![Kliknięcie przycisku Google Maps interfejsu API systemu Android w sekcji biblioteki](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. W górnej części **interfejsu API systemu Android map Google** kliknij przycisk **włączyć** Aby włączyć usługę dla tego projektu:

   [![Klikając przycisk Włącz w sekcji pulpitu nawigacyjnego](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)


W tym momencie został utworzony projekt interfejsu API i v2 interfejsu API Google Maps systemu Android został dodany do niego. Jednak nie można używać tego interfejsu API w projekcie, dopóki nie zostaną utworzone dla niego poświadczeń. Następnie przedstawiono, jak utworzyć klucz interfejsu API i białe listy aplikacji platformy Xamarin.Android, dzięki czemu są autoryzowane do używania tego klucza.


## <a name="obtaining-the-api-key"></a>Uzyskiwanie klucz interfejsu API

Po **konsoli dla deweloperów Google** projekt interfejsu API został utworzony, może się tworzenie klucza interfejsu API systemu Android. Aplikacji platformy Xamarin.Android musi mieć klucz interfejsu API przed udzieleniem im dostępu do interfejsu API systemu Android mapy v2.

1. W **interfejsu API systemu Android map Google** wyświetlana strona (po kliknięciu przycisku **włączyć** w poprzednim kroku), kliknij przycisk **przejdź do poświadczeń** przycisk:

   [![Ten interfejs API jest włączona wiadomości](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. W **poświadczenia** kliknij przycisk **poświadczeniami, które są potrzebne?** przycisk:

   [![Dodawanie poświadczeń do okna dialogowego z projektu](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. Po kliknięciu tego przycisku, jest generowany klucz interfejsu API. Następnie należy ograniczyć ten klucz, tak aby tylko aplikacji mogą wywoływać interfejsy API przy użyciu tego klucza. Kliknij przycisk **klucza Ogranicz**:

   [![Kliknięcie przycisku ograniczenia klucza na stronie poświadczenia](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. Zmień **nazwa** pola **1 klucz interfejsu API** na nazwę przypominającą klucz służy do (**XamarinMapsDemoKey** jest używany w tym przykładzie). Następnie kliknij przycisk **aplikacji systemu Android** przycisku radiowego:

   [![Wybór aplikacji dla systemu Android na stronie poświadczenia](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. Aby dodać odcisku palca algorytmu SHA-1, kliknij przycisk **+ Dodaj nazwę pakietu i odcisk palca**:

   [![Klikając przycisk Dodaj nazwę pakietu i odcisk palca](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. Wprowadź nazwę pakietu aplikacji, a następnie wprowadź odcisk palca certyfikatu SHA-1 (uzyskanych za pośrednictwem `keytool` jak opisano wcześniej w tym przewodniku). W poniższym przykładzie nazwy pakietu `XamarinMapsDemo` jest wprowadzona, a następnie odcisk palca certyfikatu SHA-1, które zostały uzyskane z **debug.keystore**:

   [![Wprowadzona nazwa pakietu jest com.xamarin.docs.android.map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. Należy pamiętać, że aby Twoje APK dostęp do map Google, użytkownik musi obejmować odciski palców SHA-1 i pakietu nazwy dla każdego keystore (debug i release), którego używasz do logowania się użytkownika APK. Na przykład jeśli używasz jednego komputera do debugowania i inny komputer w celu generowania wersji APK powinien być dołączany odcisk palca certyfikatu SHA-1 z magazynu kluczy debugowania pierwszego komputera i odcisk palca certyfikatu SHA-1 z magazynu kluczy wersji programu drugi komputer. Kliknij przycisk **+ Dodaj nazwę pakietu i odcisk palca** Aby dodać inną nazwę odcisk palca i pakietu, jak pokazano w poniższym przykładzie:

   [![Dodawanie innego odcisk palca tworzy innego certyfikatu SHA-1](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. Kliknij przycisk **zapisać** przycisk, aby zapisać zmiany. Następnie nastąpi powrót do listy kluczy interfejsu API. Jeśli masz inne klucze interfejsu API, które zostały utworzone wcześniej, zostaną również wyświetlone tutaj. W tym przykładzie wyświetlane jest tylko jeden klucz interfejsu API (utworzone w poprzednich krokach):

   [![XamarinMapsDemoKey jest wyświetlane na liście kluczy interfejsu API](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)



## <a name="adding-the-key-to-your-project"></a>Dodawanie klucza do projektu

Na koniec należy dodać ten klucz interfejsu API, aby **AndroidManifest.XML** pliku aplikacji platformy Xamarin.Android. W poniższym przykładzie `YOUR_API_KEY` ma zostać zastąpiony przy użyciu klucza interfejsu API wygenerowany w poprzednich krokach:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionName="4.10" package="com.xamarin.docs.android.mapsandlocationdemo"
    android:versionCode="10">
...

  <application android:label="@string/app_name">
    <!-- Put your Google Maps V2 API Key here. -->
    <meta-data android:name="com.google.android.geo.API_KEY" android:value="YOUR_API_KEY" />
    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
  </application>
</manifest>
```


## <a name="related-links"></a>Linki pokrewne

- [Konsoli interfejsów API firmy Google](https://code.google.com/apis/console/)
- [Klucz interfejsu API map Google](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
