---
title: Plik iTunesMetadata.plist
description: "W tym artykule omówiono pliku iTunesMetadata.plist zapewniające informacje aplikacji systemu iOS przy użyciu rozkładu Ad Hoc do testowania lub wdrożenia w przedsiębiorstwie programu iTunes."
ms.topic: article
ms.prod: xamarin
ms.assetid: 70676eba-6a99-4a3a-bccc-84359fe9c2c3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 3bdf00a9e50b2bf66f51c825306c2ba8e6365dd2
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="the-itunesmetadataplist-file"></a>Plik iTunesMetadata.plist

_W tym artykule omówiono pliku iTunesMetadata.plist zapewniające informacje aplikacji systemu iOS przy użyciu rozkładu Ad Hoc do testowania lub wdrożenia w przedsiębiorstwie programu iTunes._

Podczas tworzenia aplikacji systemu iOS w iTune Connect (albo do sprzedaży lub bezpłatnej wersji ze sklepu iTunes App Store), deweloper można określić informacje, takie jak genre, sub genre aplikacji, o prawach autorskich, urządzeń z systemem iOS obsługiwanych i wymaganych urządzeń możliwości. W aplikacjach systemu iOS, które są dostępne w ramach albo testerów lub użytkowników w firmie za pomocą dystrybucji ad hoc Brak tych informacji.

Aby podać brakujące informacje do dystrybucji Ad Hoc opcjonalny `iTunesMetadata.plist` pliku można tworzyć i zawarte w pliku IPA aplikacji. Ten plik plist jest specjalnie sformatowany plik XML (zobacz firmy Apple [przewodnik programowania w języku listy właściwości](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/PropertyLists/Introduction/Introduction.html) Aby uzyskać więcej informacji) zawierający pary klucz wartość definiowania informacji o aplikacji dla danego systemu iOS.

<a name="iTunesMetadata_contents" />

## <a name="the-itunesmetadataplist-contents"></a>Zawartość iTunesMetadata.plist

Poniżej przedstawiono przykład z typowych `iTunesMetadata.plist` plik używany do definiowania informacji iTunes Ad Hoc dystrybucji:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>UIRequiredDeviceCapabilities</key>
    <dict>
        <key>armv7</key>
        <true/>
        <key>front-facing-camera</key>
        <true/>
    </dict>
    <key>artistName</key>
    <string>Company, Inc.</string>
    <key>bundleDisplayName</key>
    <string>App Name</string>
    <key>bundleShortVersionString</key>
    <string>1.5.1</string>
    <key>bundleVersion</key>
    <string>1.5.1</string>
    <key>copyright</key>
    <string>© 2015 Company, Inc.</string>
    <key>drmVersionNumber</key>
    <integer>0</integer>
    <key>fileExtension</key>
    <string>.app</string>
    <key>gameCenterEnabled</key>
    <false/>
    <key>gameCenterEverEnabled</key>
    <false/>
    <key>genre</key>
    <string>Games</string>
    <key>genreId</key>
    <integer>6014</integer>
    <key>itemName</key>
    <string>App Name</string>
    <key>kind</key>
    <string>software</string>
    <key>playlistArtistName</key>
    <string>Company, Inc.</string>
    <key>playlistName</key>
    <string>App Name</string>
    <key>releaseDate</key>
    <string>2015-11-18T03:23:10Z</string>
    <key>s</key>
    <integer>143441</integer>
    <key>softwareIconNeedsShine</key>
    <false/>
    <key>softwareSupportedDeviceIds</key>
    <array>
        <integer>9</integer>
    </array>
    <key>softwareVersionBundleId</key>
    <string>com.company.appid</string>
    <key>subgenres</key>
    <array>
        <dict>
            <key>genre</key>
            <string>Puzzle</string>
            <key>genreId</key>
            <integer>7012</integer>
        </dict>
        <dict>
            <key>genre</key>
            <string>Word</string>
            <key>genreId</key>
            <integer>7019</integer>
        </dict>
    </array>
    <key>versionRestrictions</key>
    <integer>16843008</integer>
</dict>
</plist>

```

Wartości dla poszczególnych kluczy zostanie omówiona szczegółowo poniżej.

### <a name="uirequireddevicecapabilities"></a>UIRequiredDeviceCapabilities

`UIRequiredDeviceCapabilities` ITunes znać funkcje specyficzne dla aplikacji systemu iOS wymaga, aby można było zainstalować na urządzeniu iOS danego urządzenia, które umożliwia klucza. Podano w formie słownika (`<dict>...</dict>`) funkcji (`<key>...</key>`) i wartość logiczną, dla każdej funkcji. Jeśli wartość funkcji jest `true`, a następnie tej funkcji musi być obecny. Jeśli jest `false` funkcji nie może być obecny na urządzeniu. Na przykład:

```xml
<key>UIRequiredDeviceCapabilities</key>
<dict>
    <key>armv7</key>
    <true/>
    <key>front-facing-camera</key>
    <true/>
</dict>
```
Określa, że urządzenie iOS musi obsługiwać ARM7 instrukcji ustawić i aparatem skierowanym przed zainstalowaniem tej aplikacji na urządzeniu. Aby uzyskać pełną listę dozwolonych wartości, zobacz firmy Apple [UIRequiredDeviceCapabilities](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW3) dokumentacji.

### <a name="artistname-and-playlistartistname"></a>artistName i playlistArtistName

Użyj `artistName` i `playlistArtistName` kluczy, aby zdefiniować nazwę firmy, która utworzyła aplikację systemu iOS, który będzie wyświetlany w iTunes. Przykład:

```xml
<key>artistName</key>
<string>Company, Inc.</string>
...
<key>playlistArtistName</key>
<string>Company, Inc.</string>
```

### <a name="bundledisplayname-itemname-and-playlistname"></a>bundleDisplayName, nazwa elementu i playlistName

Użyj `bundleDisplayName`, `itemName`, i `playlistName` kluczy, aby zdefiniować nazwę aplikacji dla systemu iOS, która będzie wyświetlana wewnątrz iTunes. Przykład:

```xml
<key>bundleDisplayName</key>
<string>App Name</string>
...
<key>itemName</key>
<string>App Name</string>
...
<key>playlistName</key>
<string>App Name</string>
```

### <a name="bundleshortversionstring-and-bundleversion"></a>bundleShortVersionString i bundleVersion

Użyj `bundleShortVersionString` i `bundleVersion` klucze do definiowania numer wersji aplikacji dla systemu iOS, który będzie wyświetlany w programach iTunes. Przykład:

```xml
<key>bundleShortVersionString</key>
<string>1.5.1</string>
<key>bundleVersion</key>
<string>1.5.1</string>
```

### <a name="softwareversionbundleid"></a>softwareVersionBundleId

Użyj `softwareVersionBundleId` klawisz, aby określić identyfikator pakietu aplikacji systemu iOS. Przykład:

```xml
<key>softwareVersionBundleId</key>
<string>com.company.appid</string>
```

### <a name="copyright"></a>informacji o prawach autorskich,

Użyj `copyright` klawisz, aby zdefiniować informacje o prawach autorskich, która jest wyświetlana w programach iTunes. Przykład:

```xml
<key>copyright</key>
<string>© 2015 Company, Inc.</string>
```

### <a name="releasedate"></a>Data wersji

Użyj `releaseDate` klawisz, aby określić datę wersji dla aplikacji systemu iOS, która będzie wyświetlana w programach iTunes. Przykład:

```xml
<key>releaseDate</key>
<string>2015-11-18T03:23:10Z</string>
```

### <a name="softwareiconneedsshine"></a>softwareIconNeedsShine

Użyj `softwareIconNeedsShine` klucza iTunes stwierdzić, czy ikona aplikacji systemu iOS wymaga _highlight świecisz_ dla systemu iOS 6 (i przed). Przykład:

```xml
<key>softwareIconNeedsShine</key>
<false/>
```

### <a name="gamecenterenabled-and-gamecentereverenabled"></a>gameCenterEnabled i gameCenterEverEnabled

Użyj `gameCenterEnabled` i `gameCenterEverEnabled` klucze, aby sprawdzić iTunes jest ta aplikacja systemu iOS obsługuje Centrum gier firmy Apple. Przykład:

```xml
<key>gameCenterEnabled</key>
<false/>
<key>gameCenterEverEnabled</key>
<false/>
```

### <a name="genre-genreid-and-subgenres"></a>genre, genreId i subgenres

Użyj `genre` i `genreId` klucze mówić iTunes jakiego rodzaju aplikacje systemu iOS należy do. Przykład:

```xml
<key>genre</key>
<string>Games</string>
<key>genreId</key>
<integer>6014</integer>
```

Opcjonalnie `subgenres` klucza może służyć do zdefiniowania maksymalnie dwóch genres sub dla aplikacji systemu iOS. Przykład:

```xml
<key>subgenres</key>
<array>
    <dict>
        <key>genre</key>
        <string>Puzzle</string>
        <key>genreId</key>
        <integer>7012</integer>
    </dict>
    <dict>
        <key>genre</key>
        <string>Word</string>
        <key>genreId</key>
        <integer>7019</integer>
    </dict>
</array>
```

W aplikacjach systemu iOS firmy Apple obecnie definiuje następujące genres i genre identyfikatory:

[!include[](~/ios/includes/table-appstore.md)]

Aby uzyskać więcej informacji, zobacz firmy Apple [Genre identyfikatorów dodatku](http://www.apple.com/itunes/affiliates/resources/documentation/genre-mapping.html) dokumentacji.

### <a name="softwaresupporteddeviceids"></a>softwareSupportedDeviceIds

Użyj `softwareSupportedDeviceIds` klucza mówić iTunes jakiego urządzenia z systemem iOS, obsługuje tę aplikację systemu iOS. Przykład:

```xml
<key>softwareSupportedDeviceIds</key>
<array>
    <integer>9</integer>
</array>
```

Jeżeli dostępne są następujące wartości:

- 1 – klasycznego iPhone
- 2 — iPod Touch
- 4-iPad
- 9 — nowoczesne urządzenia iPhone

### <a name="standard-keys"></a>Standardowe klucze

Następujące klucze są zawarte w każdym `iTunesMetadata.plist` pliki w aplikacjach systemu iOS i zawsze mają takie same wartości:

```xml
<key>drmVersionNumber</key>
<integer>0</integer>
<key>fileExtension</key>
<string>.app</string>
...
<key>kind</key>
<string>software</string>
...
<key>s</key>
<integer>143441</integer>
...
<key>versionRestrictions</key>
<integer>16843008</integer>
```

<a name="iTunesMetadata_creating" />

## <a name="creating-an-itunesmetadataplist-file"></a>Tworzenie pliku iTunesMetadata.plist

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 Podczas pracy z `iTunesMetadata.plist` pliku w Visual Studio for Mac, są dostępne dwie opcje:

- Tworzenie i obsługa plików przy użyciu programu Visual Studio dla komputerów Mac visual plist edytora.
- Tworzenie i obsługa plik w edytorze zwykłego tekstu.

 Obie te opcje zostanie omówiona szczegółowo poniżej.

### <a name="using-the-visual-plist-editor"></a>Za pomocą edytora Visual Plist

Wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy plik projektu platformy Xamarin.iOS i wybierz **Dodaj** > **nowego pliku...**
2. W oknie dialogowym Nowy plik wybierz **iOS** > **listę właściwości**:

    ![](itunesmetadata-images/image01.png "Wybierz listę właściwości systemu iOS")
3. Wprowadź `iTunesMetadata` dla **nazwa** i kliknij przycisk **nowy** przycisku.
4. Kliknij dwukrotnie `iTunesMetadata.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji:

    ![](itunesmetadata-images/image02.png "Edytor iTunesMetadata.plist")
5. Kliknij zielony ** + ** Aby utworzyć nowy wpis, a następnie wprowadź `UIRequiredDeviceCapabilities` jako nazwa klucza:

    ![](itunesmetadata-images/image03.png "Utwórz nowy wpis, a następnie wprowadź UIRequiredDeviceCapabilities jako nazwa klucza")
6. Polecenie **ciąg** wartość typu i wybierz **słownika** z listy menu podręczne:

    ![](itunesmetadata-images/image04.png "Wybierz słownik z listy podręcznego")
7. Kliknij turndown po lewej stronie nazwy właściwości, aby wyświetlić wpisy słownika:

    ![](itunesmetadata-images/image05.png "Odsłoń wpisy słownika")
8. Polecenie **Dodaj nowy wpis** tekstu, następnie kliknij zielony ** + ** Aby dodać wpis do słownika:

    ![](itunesmetadata-images/image06.png "Dodaj wpis do słownika")
9. Wprowadź `armv7` jako nazwę klucza, wybierz typ **logiczna** , a następnie wprowadź **tak** jako wartość:

    ![](itunesmetadata-images/image07.png "Wprowadź nazwę klucza armv7, wybierz typ Boolean i wprowadzić wartość tak, jak wartość")
10. Powtórz powyższe kroki zostały wypełnione `iTunesMetadata.plist` plików ze wszystkimi par klucz/wartość wymagane (zobacz [zawartość iTunesMetadata.plist](#iTunesMetadata_contents) sekcji powyżej, aby uzyskać więcej informacji).

11. Zapisz zmiany w pliku plist.

### <a name="using-a-plain-text-editor"></a>Za pomocą edytora zwykłego tekstu

Wykonaj następujące czynności:

1. W edytorze zwykłego tekstu, Utwórz nowy plik tekstowy i nadaj mu nazwę `iTunesMetadata.plist`.
2. Kopiuj zawartość przykład z [zawartość iTunesMetadata.plist](#iTunesMetadata_contents) powyższej sekcji.
3. Wklej zawartość w pliku i edytować je zgodnie z potrzebami.
4. Zapisz plik i powrócić do programu Visual Studio dla komputerów Mac.
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy plik projektu platformy Xamarin.iOS i wybierz **Dodaj** > **istniejących plików... **.
6. W oknie dialogowym Otwórz plik wybierz `iTunesMetadata.plist` pliku, który został utworzony wcześniej, a następnie kliknij przycisk **OK** przycisku.
7. Pozostaw **Akcja kompilacji** tego pliku ustawioną **Brak**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dodatek plug-in Xamarin dla Visual Studio obsługuje tylko to edytor wizualny dla `Info.plist` i `Entitlement.plist` pliki, więc musisz utworzyć Twoje `iTunesMetadata.plist` plik w edytorze tekstu standardowego i ręcznie dołączyć go do projektu platformy Xamarin.iOS.

Wykonaj następujące czynności:

1. W edytorze zwykłego tekstu, Utwórz nowy plik tekstowy i nadaj mu nazwę `iTunesMetadata.plist`.
2. Kopiuj zawartość przykład z [zawartość iTunesMetadata.plist](#iTunesMetadata_contents) powyższej sekcji.
3. Wklej zawartość w pliku i edytować je zgodnie z potrzebami.
4. Zapisz plik i wróć do programu Visual Studio.
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy plik projektu platformy Xamarin.iOS i wybierz **Dodaj** > **istniejących plików... **.
6. W oknie dialogowym Otwórz plik wybierz `iTunesMetadata.plist` utworzoną wcześniej plik i kliknij przycisk **Otwórz** przycisku.
7. Pozostaw **Akcja kompilacji** tego pliku ustawioną **Brak**.

-----

Później, wybierz tę pozycję `iTunesMetadata.plist` pliku podczas przygotowania do kompilacji z IPA w IDE.

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego `iTunesMetadata.plist` pliku, który może służyć do Opisz iTunes ad hoc dostarczyć aplikacji dla systemu iOS. Został omówiony standardowe klucz w pliku plist oraz sposobu tworzenia i obsługi plików w Visual Studio i Visual Studio dla komputerów Mac.

## <a name="related-links"></a>Linki pokrewne

- [Dystrybucja w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Konfigurowanie aplikacji w iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publikowanie w sklepie App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Dystrybucji wewnętrznych](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Dystrybucja Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Obsługa IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Rozwiązywanie problemów](~/ios/deploy-test/troubleshooting.md)
