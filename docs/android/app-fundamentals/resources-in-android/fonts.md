---
title: Czcionki
ms.topic: article
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA$
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/30/2018
ms.openlocfilehash: 3b7c45a50ffb0748b5f63edfd444cb02af3fdc67
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="fonts"></a>Czcionki

<a name="overview" />

## <a name="overview"></a>Omówienie

Począwszy od poziom interfejsu API 26, zestaw SDK systemu Android umożliwia czcionki, które mają być traktowane jako zasoby, podobnie jak układów lub drawables. [Android NuGet 26 biblioteki obsługi](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) będzie Poprawka usterki systemu czcionki nowych interfejsów API do tych aplikacji, które docelowy poziom interfejsu API 14 lub nowszej.

Po dotyczących 26 interfejsu API lub v26 biblioteki obsługi systemu Android istnieją dwa sposoby używania czcionek w aplikacji systemu Android:

1. **Pakiet czcionki jako zasób systemu Android** &ndash; daje to pewność, że czcionki jest zawsze dostępny dla aplikacji, ale spowoduje zwiększenie rozmiaru plik APK. 
2. **Pobierz czcionki** &ndash; Android obsługuje również Pobieranie czcionki z _dostawcy czcionki_. Dostawca czcionki sprawdza, czy czcionka jest już na urządzeniu. Jeśli to konieczne, czcionka zostanie pobrany i pamięci podręcznej na urządzeniu. Wiele aplikacji może być współużytkowana tę czcionkę.

Czcionki podobne (lub czcionki, która może mieć kilka różnych stylów) mogą być łączone w _rodziny czcionek_. Umożliwia to deweloperom określenie niektóre atrybuty czcionki, takie jak jego wagę i Android automatycznie wybiera odpowiednią czcionkę z rodziny czcionek.

V26 biblioteki obsługi systemu Android zostanie Poprawka usterki systemu obsługę czcionki, które mają poziom interfejsu API 26. Jeśli systemem docelowym jest starsza poziomy interfejsu API, należy go deklarować `app` przestrzeni nazw XML, nadaj różnych atrybutów czcionek przy użyciu `android:` przestrzeni nazw i `app:` przestrzeni nazw. Jeśli tylko `android:` przestrzeni nazw jest używana, a następnie nie będą one wyświetlane urządzeń z systemem poziom interfejsu API, 25 lub mniej. Na przykład następujący fragment kodu XML deklaruje nowy [ _rodziny czcionek_ ](#font_families) zasobu, który będzie działać na poziomie interfejsu API 14 lub nowszy:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

     <font  android:font="@font/sourcesanspro_regular" 
            android:fontStyle="normal" 
            android:fontWeight="400"
            app:font="@font/sourcesanspro_regular" 
            app:fontStyle="normal" 
            app:fontWeight="400" />

</font-family>    
```

Tak długo, jak czcionki są dostarczane do aplikacji systemu Android w sposób właściwe, które można zastosować widżetu interfejsu użytkownika przez ustawienie [ `fontFamily` atrybutu](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily). Na przykład poniższy fragment kodu przedstawiono sposób wyświetlania czcionkę w element TextView:

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/caveat_bold"
    app:fontFamily="@font/caveat_bold"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

W tym przewodniku najpierw opisano, jak używać czcionek jako zasób systemu Android, a następnie przejdź do omówienia sposobu pobierania czcionek w czasie wykonywania.

<a name="fonts_as_a_resource" />

## <a name="fonts-as-a-resource"></a>Czcionki jako zasób

Pakowanie czcionkę w systemie Android APK gwarantuje, że zawsze jest dostępne dla aplikacji. Plik czcionki (albo. TTF lub. Plik OTF) jest dodawany do aplikacji platformy Xamarin.Android, podobnie jak inne zasobów przez kopiowanie plików do podkatalogu w **zasobów** folderu projektu platformy Xamarin.Android. Czcionki zasobów są przechowywane w **czcionki** podkatalogu z **zasobów** folderu projektu. 


> [!NOTE]
>  Czcionek powinien mieć **Akcja kompilacji** z **AndroidResource** lub nie będą one umieszczone w ostatnim APK. Akcja kompilacji powinien być ustawiony automatycznie IDE.

Wiele podobnych czcionki plików (na przykład czcionkę tej samej wagi różnych lub style) jest możliwy do grupowania ich w danej rodzinie czcionek.

<a name="font_families" />

### <a name="font-families"></a>Rodziny czcionek

Rodzina czcionek to zestaw czcionek, które mają różne wagi i style. Na przykład może być czcionki oddzielne pliki czcionek pogrubieniem lub kursywą. Rodzina czcionek jest definiowana za pomocą `font` elementów w pliku XML, który jest przechowywany w **zasobów/czcionki** katalogu. Każdy rodziny czcionek powinien mieć własny plik XML.

Aby utworzyć rodziny czcionek, najpierw dodać czcionki, aby **zasobów/czcionki** folderu. Następnie utwórz nowy plik XML w folderze czcionki dla rodziny czcionek. Ten plik XML będzie mieć główny `font-family` element, który zawiera co najmniej jeden `font` elementów. Każdy `font` element deklaruje atrybuty czcionki. 

Następujący kod XML jest przykładem rodziny czcionek _źródeł sieci SAN Pro_ czcionki, który definiuje wiele wag czcionkę. To jest zapisywany jako plik w **zasobów/czcionki** folder o nazwie **sourcesanspro.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto">
    <font android:font="@font/sourcesanspro_regular" 
          android:fontStyle="normal" 
          android:fontWeight="400"
          app:font="@font/sourcesanspro_" 
          app:fontStyle="normal" 
          app:fontWeight="400" />
    <font android:font="@font/sourcesanspro_bold" 
          android:fontStyle="normal" 
          android:fontWeight="800" 
          app:font="@font/sourcesanspro_bold" 
          app:fontStyle="normal" 
          app:fontWeight="800" />
    <font android:font="@font/sourcesanspro_italic" 
          android:fontStyle="italic" 
          android:fontWeight="400"
          app:font="@font/sourcesanspro_italic" 
          app:fontStyle="italic" 
          app:fontWeight="400" />
</font-family>
```

`fontStyle` Atrybut ma dwa możliwe wartości:

* **normalne** &ndash; Normalna czcionka
* **kursywą** &ndash; kursywa

`fontWeight` Atrybut odpowiada CSS `font-weight` atrybutu i odwołuje się do grubość czcionki. Jest to wartość z zakresu od 100 do 900. Na poniższej liście opisano typowe wartości wagi czcionek i ich nazwy:

* **Alokowania** &ndash; 100
* **Bardzo światła** &ndash; 200
* **Jasny** &ndash; 300
* **Normalny** &ndash; 400
* **Średnia liczba godzin** &ndash; 500
* **Częściowej Bold** &ndash; 600
* **Bold** &ndash; 700
* **Bardzo Bold** &ndash; 800
* **Czarne** &ndash; 900

Po zdefiniowaniu rodziny czcionek, można deklaratywnie przez ustawienie `fontFamily`, `textStyle`, i `fontWeight` atrybutów w pliku układu.  Na przykład następujący fragment kodu XML ustawia styl kursywa i 400 wagę czcionki (normalne):

```xml
<TextView
    android:text="Sans Source Pro semi-bold italic, 600 weight, italic"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:fontFamily="@font/sourcesanspro"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_horizontal"
    android:fontWeight="600"
    android:textStyle="italic"
    />
```

<a name="programatically_assigning_fonts" />

### <a name="programmatically-assigning-fonts"></a>Programowo Przypisywanie czcionek

Czcionki można programowo ustawić za pomocą [ `Resources.GetFont` ](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int)) metoda pobierania [ `Typeface` ](https://developer.android.com/reference/android/graphics/Typeface.html) obiektu. Wiele widoków ma `TypeFace` właściwość, która może służyć do przypisania czcionki do elementu widget. Następujący fragment kodu przedstawia sposób programowo ustawienia czcionki na element TextView:

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

`GetFont` Metody automatycznie zostanie załadowany pierwszy czcionki w danej rodzinie czcionek.  Aby załadować odpowiadający określonym styl czcionki, należy użyć `Typeface.Create` metody. Ta metoda próbuje załadować odpowiadający określonym styl czcionki. Na przykład ta Wstawka kodu będzie próbował załadować pogrubiona `Typeface` obiektu z rodziny czcionek, która jest zdefiniowana w **zasobów/czcionki**:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

<a name="downloading_fonts" />

## <a name="downloading-fonts"></a>Pobieranie czcionek

Zamiast czcionki opakowania jako zasób aplikacji systemu Android można pobrać czcionki ze źródła zdalnego. Ma to wpływu pożądane zmniejszenie rozmiaru plik APK. 

Czcionki są pobierane z pomocą _dostawcy czcionki_. Jest to specjalne dostawcy zawartości, który zarządza pobieranie i buforowanie czcionki do wszystkich aplikacji na urządzeniu. 8.0 dla systemu android zawiera dostawcy czcionki, aby pobrać czcionki z [repozytorium czcionki Google](http://fonts.google.com). Ten dostawca czcionki domyślny jest backported poziom interfejsu API 14 z v26 biblioteki obsługi systemu Android.
 
 Aplikacja wysyła żądanie do czcionki, dostawca czcionki zostanie najpierw sprawdź, czy czcionka jest już na urządzeniu. Jeśli nie, następnie spróbuje pobrać czcionkę. Jeśli czcionka nie może zostać pobrany, następnie Android użyje domyślnej czcionki systemowej. Pobrany czcionki jest dostępna we wszystkich aplikacjach na urządzeniu, a nie tylko aplikacji, który zgłosił żądanie początkowej.

Po wysłaniu żądania do pobrania czcionki, aplikacja nie bezpośrednio zbadać dostawcy czcionki. Zamiast tego użyje wystąpienia aplikacji [ `FontsContract` ](https://developer.android.com/reference/android/provider/FontsContract.html) interfejsu API (lub [ `FontsContractCompat` ](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) Jeśli 26 biblioteki obsługi jest używany).  

8.0 dla systemu android obsługuje pobieranie czcionki na dwa sposoby:

1. **Deklarowanie czcionki do pobrania jako zasób** &ndash; aplikacji mogą zadeklarować czcionki do pobrania w systemie Android za pośrednictwem plików zasobów XML. Te pliki będzie zawierać wszystkie dane meta wymagające asynchronicznie pobrać czcionek uruchamia i buforowania ich na urządzeniu aplikacji systemu Android.
2. **Programowo** &ndash; interfejsów API na poziomie interfejsu API systemu Android 26 zezwolić aplikacji na programowo, Pobierz czcionek, gdy aplikacja jest uruchomiona. Tworzy aplikacje `FontRequest` obiektu dla danego czcionki i przekaż tego obiektu w celu `FontsContract` klasy. `FontsContract` Przyjmuje `FontRequest` i pobiera czcionkę z _dostawcy czcionki_. Android synchronicznie pobierze czcionki. Przykład tworzenia `FontRequest` będą wyświetlane w dalszej części tego przewodnika.

Niezależnie od tego, które rozwiązanie jest używana można pobrać pliki zasobów, które muszą zostać dodane do aplikacji platformy Xamarin.Android przed czcionki. Po pierwsze, czcionki musi być zadeklarowana w taki sposób, w pliku XML w **zasobów/czcionki** katalogu w ramach danej rodzinie czcionek. Ta Wstawka kodu przedstawiono przykładowy sposób pobierania czcionek z [kolekcji typu Open Source czcionki Google](https://fonts.google.com) przy użyciu domyślnego dostawcę czcionki dołączoną 8.0 dla systemu Android (lub biblioteka obsługi v26):

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto"
             android:fontProviderAuthority="com.google.android.gms.fonts" 
             android:fontProviderPackage="com.google.android.gms" 
             android:fontProviderQuery="VT323" 
             android:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
             app:fontProviderAuthority="com.google.android.gms.fonts" 
             app:fontProviderPackage="com.google.android.gms" 
             app:fontProviderQuery="VT323"
             app:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
    >
</font-family>
```

`font-family` Element zawiera następujące atrybuty deklarowanie informacje, czy system Android wymaga, aby pobrać czcionki:
 
1. **fontProviderAuthority** &ndash; urzędu dostawcy czcionki do użycia dla żądania.
2. **fontPackage** &ndash; pakiet dla dostawcy czcionki do użycia dla żądania. Służy to, aby sprawdzić tożsamość dostawcy.
3. **fontQuery** &ndash; to ciąg, który pomoże dostawcy czcionki zlokalizować żądanej czcionki. Szczegóły dotyczące zapytania czcionki są specyficzne dla dostawcy czcionki. [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) Klasy w [czcionki do pobrania](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) Przykładowa aplikacja udostępniono pewne informacje o formacie zapytania czcionek z kolekcji Google czcionki Otwórz źródła.
4. **fontProviderCerts** &ndash; tablicy zasobów z listę zestawów wartości skrótów dotyczących certyfikatów, które powinny być podpisane dostawcy z.

Po zdefiniowaniu czcionek, może być konieczne podanie informacji o _certyfikaty czcionki_ związane z pobierania.

<a name="font_certificates" />

### <a name="font-certificates"></a>Certyfikaty czcionki

Jeśli dostawca czcionek nie jest preinstalowany na urządzeniu lub aplikacja używa `Xamarin.Android.Support.Compat` biblioteki, Android wymaga certyfikatów zabezpieczeń dostawcy czcionki. Te certyfikaty zostaną wyświetlone w pliku zasobów tablicy, który jest przechowywany w **zasobów/wartości** katalogu. 

Na przykład następujący kod XML ma nazwę **Resources/values/fonts_cert.xml** i magazyny certyfikatów dla dostawcy czcionki Google: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="com_google_android_gms_fonts_certs">
        <item>@array/com_google_android_gms_fonts_certs_dev</item>
        <item>@array/com_google_android_gms_fonts_certs_prod</item>
    </array>
    <string-array name="com_google_android_gms_fonts_certs_dev">
        <item>
            MIIEqDCCA5CgAwIBAgIJANWFuGx90071MA0GCSqGSIb3DQEBBAUAMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTAeFw0wODA0MTUyMzM2NTZaFw0zNTA5MDEyMzM2NTZaMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBANbOLggKv+IxTdGNs8/TGFy0PTP6DHThvbbR24kT9ixcOd9W+EaBPWW+wPPKQmsHxajtWjmQwWfna8mZuSeJS48LIgAZlKkpFeVyxW0qMBujb8X8ETrWy550NaFtI6t9+u7hZeTfHwqNvacKhp1RbE6dBRGWynwMVX8XW8N1+UjFaq6GCJukT4qmpN2afb8sCjUigq0GuMwYXrFVee74bQgLHWGJwPmvmLHC69EH6kWr22ijx4OKXlSIx2xT1AsSHee70w5iDBiK4aph27yH3TxkXy9V89TDdexAcKk/cVHYNnDBapcavl7y0RiQ4biu8ymM8Ga/nmzhRKya6G0cGw8CAQOjgfwwgfkwHQYDVR0OBBYEFI0cxb6VTEM8YYY6FbBMvAPyT+CyMIHJBgNVHSMEgcEwgb6AFI0cxb6VTEM8YYY6FbBMvAPyT+CyoYGapIGXMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbYIJANWFuGx90071MAwGA1UdEwQFMAMBAf8wDQYJKoZIhvcNAQEEBQADggEBABnTDPEF+3iSP0wNfdIjIz1AlnrPzgAIHVvXxunW7SBrDhEglQZBbKJEk5kT0mtKoOD1JMrSu1xuTKEBahWRbqHsXclaXjoBADb0kkjVEJu/Lh5hgYZnOjvlba8Ld7HCKePCVePoTJBdI4fvugnL8TsgK05aIskyY0hKI9L8KfqfGTl1lzOv2KoWD0KWwtAWPoGChZxmQ+nBli+gwYMzM1vAkP+aayLe0a1EQimlOalO762r0GXO0ks+UeXde2Z4e+8S/pf7pITEI/tP+MxJTALw9QUWEv9lKTk+jkbqxbsh8nfBUapfKqYn0eidpwq2AzVp3juYl7//fKnaPhJD9gs=
        </item>
    </string-array>
    <string-array name="com_google_android_gms_fonts_certs_prod">
        <item>
            MIIEQzCCAyugAwIBAgIJAMLgh0ZkSjCNMA0GCSqGSIb3DQEBBAUAMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDAeFw0wODA4MjEyMzEzMzRaFw0zNjAxMDcyMzEzMzRaMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBAKtWLgDYO6IIrgqWbxJOKdoR8qtW0I9Y4sypEwPpt1TTcvZApxsdyxMJZ2JORland2qSGT2y5b+3JKkedxiLDmpHpDsz2WCbdxgxRczfey5YZnTJ4VZbH0xqWVW/8lGmPav5xVwnIiJS6HXk+BVKZF+JcWjAsb/GEuq/eFdpuzSqeYTcfi6idkyugwfYwXFU1+5fZKUaRKYCwkkFQVfcAs1fXA5V+++FGfvjJ/CxURaSxaBvGdGDhfXE28LWuT9ozCl5xw4Yq5OGazvV24mZVSoOO0yZ31j7kYvtwYK6NeADwbSxDdJEqO4k//0zOHKrUiGYXtqw/A0LFFtqoZKFjnkCAQOjgdkwgdYwHQYDVR0OBBYEFMd9jMIhF1Ylmn/Tgt9r45jk14alMIGmBgNVHSMEgZ4wgZuAFMd9jMIhF1Ylmn/Tgt9r45jk14aloXikdjB0MQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEUMBIGA1UEChMLR29vZ2xlIEluYy4xEDAOBgNVBAsTB0FuZHJvaWQxEDAOBgNVBAMTB0FuZHJvaWSCCQDC4IdGZEowjTAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBBAUAA4IBAQBt0lLO74UwLDYKqs6Tm8/yzKkEu116FmH4rkaymUIE0P9KaMftGlMexFlaYjzmB2OxZyl6euNXEsQH8gjwyxCUKRJNexBiGcCEyj6z+a1fuHHvkiaai+KL8W1EyNmgjmyy8AW7P+LLlkR+ho5zEHatRbM/YAnqGcFh5iZBqpknHf1SKMXFh4dd239FJ1jWYfbMDMy3NS5CTMQ2XFI1MvcyUTdZPErjQfTbQe3aDQsQcafEQPD+nqActifKZ0Np0IS9L9kR/wbNvyz6ENwPiTrjV2KRkEjH78ZMcUQXg0L3BYHJ3lc69Vs5Ddf9uUGGMYldX3WfMBEmh/9iFBDAaTCK
        </item>
    </string-array>
</resources>
```

Z tymi plikami zasobów w miejscu aplikacji ma możliwość pobierania czcionek.

<a name="downloadable_font_resource_declaration" />

### <a name="declaring-downloadable-fonts-as-resources"></a>Deklarowanie czcionki do pobrania jako zasoby

Poprzez wyszczególnienie czcionki do pobrania w **AndroidManifest.XML**, Android asynchronicznie pobierze czcionki po pierwszym uruchomieniu aplikacji. Czcionka przez siebie są wymienione w pliku zasobów tablicy podobny do: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```        

Aby pobrać te czcionki, ich musi być zadeklarowana w **AndroidManifest.XML** przez dodanie `meta-data` jako element podrzędny `application` elementu. Na przykład, jeśli czcionki do pobrania został zadeklarowany w pliku zasobów na **Resources/values/downloadable_fonts.xml**, a następnie ta Wstawka kodu musi zostać dodany do manifestu: 

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

<a name="programatically_downloading_fonts" />

### <a name="downloading-a-font-with-the-font-apis"></a>Pobieranie czcionki z czcionek interfejsami API

Można programowo pobrać czcionkę przy uruchamianiu [ `FontRequest` ](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html) obiekt i przekazywanie do `FontContractCompat.RequestFont` metody. `FontContractCompat.RequestFont` Metody najpierw sprawdź, czy czcionka istnieje na urządzeniu, a następnie w razie potrzeby będzie asynchronicznie zapytania z czcionki dostawcy i spróbuj pobrać czcionkę dla aplikacji. Jeśli `FontRequest` nie może pobrać czcionkę, Android będzie używać domyślnej czcionki systemowej. 

A `FontRequest` obiekt zawiera informacje, które będą używane przez dostawcę czcionki do lokalizowania i Pobierz czcionki. A `FontRequest` wymaga czterech części informacji:

1. **Czcionka dostawcy urzędu** &ndash; urzędu dostawcy czcionki do użycia dla żądania.
2. **Pakiet czcionki** &ndash; pakiet dla dostawcy czcionki do użycia dla żądania. Służy to, aby sprawdzić tożsamość dostawcy.
3. **Zapytanie czcionki** &ndash; to ciąg, który pomoże dostawcy czcionki zlokalizować żądanej czcionki. Szczegóły dotyczące zapytania czcionki są specyficzne dla dostawcy czcionki. Szczegóły ciągu są specyficzne dla dostawcy czcionki. [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) Klasy w [czcionki do pobrania](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) Przykładowa aplikacja udostępniono pewne informacje o formacie zapytania czcionek z kolekcji Google czcionki Otwórz źródła.
4. **Certyfikaty dostawcy czcionki** &ndash; tablicy zasobów z listę zestawów skrótów certyfikatów, dostawca powinny być podpisane z. 

Ta Wstawka kodu jest przykładem tworzenia wystąpienia nowy `FontRequest` obiektu:

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

W poprzednich fragment `FontToDownload` zapytanie, które pomogą czcionki z kolekcji typu Open Source czcionki Google. 

Przed przekazaniem `FontRequest` do `FontContractCompat.RequestFont` metody, istnieją dwa obiekty, które muszą zostać utworzone:

* **`FontsContractCompat.FontRequestCallback`** &ndash; To jest klasą abstrakcyjną, która musi zostać rozszerzony. To wywołanie zwrotne, które będą wywoływane, gdy `RequestFont` zostało zakończone. Aplikacji platformy Xamarin.Android musi podklasy `FontsContractCompat.FontRequestCallback` i zastąpienia `OnTypefaceRequestFailed` i `OnTypefaceRetrieved`, zapewniając akcje do wykonania, kiedy pobieranie nie powiodło się lub odpowiednio zakończy się pomyślnie.
* **`Handler`** &ndash; Jest to `Handler` który będzie używany przez `RequestFont` pobrać czcionki w wątku, w razie potrzeby. Czcionek powinien **nie** pobrane w wątku interfejsu użytkownika.  

Ta Wstawka kodu jest przykładem klasy C#, która pobierze asynchronicznie czcionki z kolekcji typu Open Source czcionki Google. Implementuje `FontRequestCallback` interfejsu i zgłasza zdarzenie C# podczas `FontRequest` zostało zakończone. 


```csharp
public class FontDownloadHelper : FontsContractCompat.FontRequestCallback
{
    // A very simple font query; replace as necessary
    public static readonly String FontToDownload = "Courgette";
    
    Android.OS.Handler Handler = null;

    public event EventHandler<FontDownloadEventArg> FontDownloaded = delegate
    {
        // just an empty delegate to avoid null reference exceptions.  
    };


    public void DownloadFonts(Context context)
    {
        FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms",FontToDownload , Resource.Array.com_google_android_gms_fonts_certs);
        FontsContractCompat.RequestFont(context, request, this, GetHandlerThreadHandler());
    }

    public override void OnTypefaceRequestFailed(int reason)
    {
        base.OnTypefaceRequestFailed(reason);
        FontDownloaded(this, new FontDownloadEventArg(null));
    }

    public override void OnTypefaceRetrieved(Android.Graphics.Typeface typeface)
    {
        base.OnTypefaceRetrieved(typeface);
        FontDownloaded(this, new FontDownloadEventArg(typeface));
    }
    
    Handler GetHandlerThreadHandler()
    {
        if (Handler == null)
        {
            HandlerThread handlerThread = new HandlerThread("fonts");
            handlerThread.Start();
            Handler = new Handler(handlerThread.Looper);
        }
        return Handler;
    }
}


/// <summary>
/// EventArg when a font has been downloaded. 
/// </summary>
public class FontDownloadEventArg : EventArgs
{
    public FontDownloadEventArg(Android.Graphics.Typeface typeface)
    {
        Typeface = typeface;
    }
    public Android.Graphics.Typeface Typeface { get; private set; }
    public bool RequestFailed
    {
        get
        {
            return Typeface != null;
        }
    }
}
```



Aby użyć tego pomocnika nowy `FontDownloadHelper` jest tworzony, i program obsługi zdarzeń jest przypisany:  
```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) => 
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```

<a name="summary" />

## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano nowe interfejsy API systemu Android 8.0 do obsługi do pobrania czcionek i czcionek jako zasoby. Go omówiono sposób osadzania czcionek istniejących w APK i używać ich w układzie. Omówiono również, jak 8.0 dla systemu Android obsługuje pobieranie czcionki od dostawcy czcionki programowo lub przez zadeklarowanie czcionki meta danych zasobów plików. 


## <a name="related-links"></a>Linki pokrewne

- [FontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [FontsContractCompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources.GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [Typeface](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Obsługa systemu android biblioteki 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Używanie czcionek w systemie Android](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [Określenie wagi czcionek CSS](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Kolekcja typu Open Source czcionki Google](https://fonts.google.com/)
- [Źródło sieci SAN, Pro](https://fonts.google.com/specimen/Source+Sans+Pro)
