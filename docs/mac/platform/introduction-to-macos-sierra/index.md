---
title: Wprowadzenie do macOS Sierra
description: "W tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w macOS Sierra dla deweloperów Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 036e1ddc95f8eabec3e87c13c25cad972c29a5d1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-macos-sierra"></a>Wprowadzenie do macOS Sierra

Z nowego macOS Sierra dewelopera można korzystać z nowych interfejsów API, który Zezwalanie użytkownikom na interakcję z ich aplikacji i witryn internetowych w sposób wcześniej niedostępne. Na przykład Apple umożliwia teraz witryny, aby umożliwić klientom bezpiecznie płatności za pośrednictwem Apple Pay i ulepszenia, aby zwiększyć framework kompletnego stanu grafiki aplikacji i przetwarzania danych potencjalnych. 

Aby uzyskać więcej informacji dotyczących macOS Sierra, zobacz firmy Apple [macOS + aplikacji](https://developer.apple.com/macos/) dokumentacji.

<a name="Whats-New-in-macOS-Sierra" />

## <a name="whats-new-in-macos-sierra"></a>Nowości w macOS Sierra

Apple został dodany kilka nowych interfejsów API i usługi macOS Sierra wraz z wielu ulepszenia istniejących funkcji, w tym:

<a name="Apple-File-System" />

### <a name="apple-file-system"></a>System plików firmy Apple

Z macOS Sierra Apple wydała jako system plików modern dla systemów iOS, system macOS, systemu tvOS i watchOS nowy System plików firmy Apple. System plików Apple została zoptymalizowana pod kątem przechowywania Flash i SSD i ma następujące cechy: silne szyfrowanie, metadane kopii przy zapisie, miejsce do udostępniania, klonowanie dla plików i katalogów, migawek, rozmiaru katalogu szybkiego i atomic podstawowych Zapisz bezpieczne.

Aby uzyskać więcej informacji, zobacz firmy Apple [przewodnika System plików Apple](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999).

<a name="Apple-Pay-Enhancements" />

### <a name="apple-pay-enhancements"></a>Ulepszenia płatności firmy Apple

Apple wprowadziła kilka ulepszeń Apple Pay w macOS Sierra, które umożliwiają użytkownikom bezpieczne płatności z witryn sieci Web.

Z macOS Sierra kilka nowych interfejsów API dodano działające z macOS Sierra, iOS i watchOS do obsługi sieci dynamiczne płatności i nowego środowiska testowego piaskownicy.

System macOS Sierra zawiera nowe struktury ApplePay Javascript, która umożliwia deweloperowi dołączyć Apple Pay bezpośrednio do systemów iOS i macOS witryn skonstruowanych na podstawie Safari. W przypadku witryn sieci Web, która obsługuje Apple Pay użytkownik może autoryzować płatności za pomocą ich iPhone lub Apple Watch.

Aby uzyskać więcej informacji, zobacz firmy Apple [ApplePay JS Framework](https://developer.apple.com/reference/applepayjs) odwołania.

<a name="Building-Modern-macOS-Apps" />

### <a name="building-modern-macos-apps"></a>Tworzenie macOS nowoczesnych aplikacji

System macOS nowoczesnych aplikacji, takich jak przeglądarki sieci web Safari firmy Apple, tekstów stron i numery rozpowszechniania za pomocą otwartych wiele nowych technologii do prezentowania ujednolicone, kontekstowej interfejsu użytkownika, który wykonuje optymalizacji tradycyjnych elementów interfejsu użytkownika, takich jak ruchome panele i wielu systemu Windows.

[![Przykład okno Mac z kartami](images/content08.png)](images/content08.png#lightbox)

Nasze [macOS tworzenie nowoczesnych aplikacji](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md) przewodnik obejmuje kilka porad, funkcje i techniki deweloper może użyć do utworzenia aplikacji modern macOS w Xamarin.Mac.

<a name="CloudKit-Data-Sharing" />

### <a name="cloudkit-data-sharing"></a>Udostępnianie danych CloudKit

W ramach CloudKit została rozszerzona w macOS Sierra, aby zezwolić użytkownikowi na szybkie i łatwe udostępnianie rekordów lub zestawów rekordów z swoje prywatne iCloud bazy danych.

CloudKit zapewnia pełną interfejsu użytkownika do wysyłania i akceptowanie udostępnionego rekordu zaproszeń i użytkownik ma odczytu/zapisu pełną kontrolę nad użytkowników, którzy mają dostęp do rekordów.

<!--To find out more, please see our [CloudKit Data Sharing](~/mac/platform-features/introduction-to-macos-sierra/cloudkit-data-sharing/) guide.-->

Aby uzyskać więcej informacji, zobacz firmy Apple [odwołania Framework CloudKit](https://developer.apple.com/reference/clockkit) i [odwołania Framework JS CloudKit](https://developer.apple.com/reference/cloudkitjs).

<a name="Safari-App-Extensions-Support" />

### <a name="safari-app-extensions-support"></a>Obsługa rozszerzeń aplikacji Safari

Rozszerzenia aplikacji Safari Zezwalaj aplikacji na rozszerzyć podczas jest ściśle zintegrowany z macOS Sierra zachowanie przeglądarki Safari. Ponieważ system macOS Safari aplikacji rozszerzenia działają podobnie jak Safari rozszerzeń aplikacji systemu iOS, są one łatwe portu z jednego systemu do innego.

Aby uzyskać więcej informacji, zobacz firmy Apple [Safari przewodnik programowania w języku rozszerzenie aplikacji](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319).

<a name="Security-and-Privacy-Enhancements" />

### <a name="security-and-privacy-enhancements"></a>Bezpieczeństwo i prywatność ulepszenia

Apple wprowadziła kilka ulepszeń zarówno prywatności i bezpieczeństwa w macOS Sierra ułatwiającą aplikacji poprawy zabezpieczeń aplikacji i zapewnienia zachowania poufności użytkownika końcowego, takie jak następujące:

- Nowy `NSAllowsArbitraryLoadsInWebContent` klucza można dodać do aplikacji `Info.plist` pliku i umożliwi stron sieci web można prawidłowo załadować podczas zabezpieczeń transportu firmy Apple (ATS) jest nadal chroniona w pozostałej części aplikacji.
- Wspólne dane zabezpieczeń architektura (CDSA) interfejsu API jest przestarzała i powinna zostać zastąpiona w interfejsie API SecKey, aby wygenerować klucze asymetryczne.
- W przypadku wszystkich połączeń SSL/TLS szyfrowania symetrycznego RC4 teraz jest domyślnie wyłączona. Ponadto transportu zabezpieczyć interfejs API nie obsługuje protokołów SSLv3 i zaleca się, że aplikacja zatrzymać tak szybko, jak to możliwe przy użyciu algorytmu SHA-1 i 3DES kryptografii.
- Ponieważ nowego Schowka w systemie iOS 10 i macOS Sierra umożliwia użytkownikowi kopiowanie i wklejanie między urządzeniami, interfejsu API zostały rozszerzone umożliwia Schowka ograniczone do określonego urządzenia i oznaczony znacznikiem czasowym, aby automatycznie wyczyszczona w danym momencie. Ponadto większa nazwane nie są zachowywane i powinna zostać zastąpiona udostępnionych kontenerów stołu montażowego.
- Jeśli aplikacja uzyskuje dostęp do chronionych danych (na przykład kalendarza użytkownika), jego _musi_ zadeklarować tego celem z kluczem wartość ciągu poprawne cel w jego `Info.plist` pliku (`NSCalendarUsageDescription` w przypadku kalendarza).
- Deweloper podpisane aplikacje, które nie są dostarczane za pośrednictwem sklepu Mac App Store można teraz korzystać z CloudKit, iCloud łańcucha kluczy, iCloud dysku, powiadomienia wypychane zdalnego, uprawnień MapKit i sieci VPN.
- System macOS Sierra nie obsługuje już dostarczania zewnętrznego kodu lub danych wraz z aplikacji osoby podpisującej kodu w archiwum zip lub obraz dysku bez znaku, ścieżka środowiska uruchomieniowego nie są znane, przed uruchomieniem.

Ponadto aplikacje działające na macOS Sierra (lub nowsza) statycznie musi deklarować ich próba dostępu do określonych funkcji lub informacje użytkownika, wprowadzając jeden lub więcej określonych kluczy prywatności w ich `Info.plist` pliki, które wyjaśnić użytkownikowi przyczynę aplikacja chce uzyskać dostęp.

Ponieważ system macOS Sierra współużytkuje te zmiany z systemem iOS 10, zobacz nasze iOS 10 [zabezpieczeń i prywatności rozszerzenia](~/ios/app-fundamentals/security-privacy.md) przewodnika, aby uzyskać więcej informacji.

<a name="Smart-Card-Driver-Extension-Support" />

### <a name="smart-card-driver-extension-support"></a>Obsługa rozszerzenia sterowników kart inteligentnych

System macOS Sierra, umożliwia tworzenie aplikacji `NSExtension` na podstawie sterowniki kart inteligentnych, które umożliwia dostęp tylko do odczytu do zawartości z niektórych typów kart inteligentnych. Informacje te są następnie prezentowane w łańcuchu kluczy systemu (zastępując przestarzałe metody Architektura zabezpieczeń danych).

Aby uzyskać więcej informacji, Pleas Zobacz firmy Apple w [odwołania Framework CryptoTokenKit](https://developer.apple.com/reference/cryptotokenkit).

<a name="Unified-Logging" />

### <a name="unified-logging"></a>Ujednolicone rejestrowania

Rejestrowanie ujednoliconego zapewnia aplikacji za pośrednictwem jednego interfejsu API dla obsługi wiadomości wydajne na wszystkich poziomach systemu. Ujednolicone rejestrowania aplikacji mający precyzyjną kontrolę nad wiele poziomów rejestrowania obejmujące prywatności i działania śledzenie ułatwiają debugowanie. 

Rejestrowanie umożliwia automatyczne wiadomości korelacji podczas działania śledzenia i rejestrowania są używane razem.

System macOS Sierra obejmuje nowej konsoli aplikacji (w Applications/Utilities) umożliwiającą wyświetlanie danych dziennika z wielu źródeł, takich jak połączonych urządzeń. Również obsługuje tokenami i zapisanych wyszukiwań i wyświetla połączenia między pokrewne komunikatami wielu procesom.

Ponadto komunikaty w dzienniku można wyświetlać i aktualizować za pomocą narzędzia wiersza polecenia.

Aby uzyskać więcej informacji, zobacz firmy Apple [rejestrowania](https://developer.apple.com/reference/os/1891852-logging).

<a name="Wide-Color" />

### <a name="wide-color"></a>Kolor międzynarodowe

System macOS Sierra rozszerza do obsługi formatów pikseli rozszerzony zakres i całej gamy przestrzenie w całym systemie, w tym platform, takich jak Core grafiki, Core obrazu systemu operacyjnego i AVFoundation. Obsługa urządzeń z przedstawia szeroką kolor dalsze jest ułatwiony dostarczając to zachowanie przez cały stos całej grafiki.

Ponadto `AppKit` został zmodyfikowany do pracy w nowym rozszerzony **sRGB** kolorów, ułatwiając mieszać kolorów w one zakresami szeroki kolor bez utraty znaczących wydajności.

Apple oferuje następujące najlepsze rozwiązania podczas pracy z szeroką kolorów:

- `NSColor` teraz używa sRGB przestrzeń kolorów i nie będą clamp wartości `0.0` do `1.0` zakresu. Jeśli aplikacja opiera się na poprzednie ograniczenie zachowanie, trzeba będzie można zmodyfikować macOS Sierra.
- Podaj przetwarzania obrazów za pomocą API niskiego poziomu, takich jak grafiki rdzeni lub systemu operacyjnego, aplikacji należy używać rozszerzonej zakresu kolorów miejsca i piksela formacie, który obsługuje wartości zmiennoprzecinkowych 16-bitowych. W przypadku, gdy to konieczne, aplikacja będzie musiał ręcznie clamp wartości składnika kolorów.
- Podstawowe grafiki, Core obrazu i oferować nowych metod konwersji między dwoma przestrzenie systemu operacyjnego wydajności programów do cieniowania.

Aby dowiedzieć się więcej, zobacz nasze [wprowadzenie do szerokości kolor](~/ios/platform/wide-color.md) przewodnik.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Zmiany dodatkowe Framework

Oprócz framework najważniejszych zmian i dodatków wymienionych powyżej Apple wprowadził wiele dodatkowych framework drobne zmiany macOS Sierra.

Aby dowiedzieć się więcej, zobacz nasze [dodatkowych zmian Framework](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md) przewodnik.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>Przestarzałe interfejsy API

Następujące interfejsy API są przestarzałe w macOS Sierra:

- HFS standardowy System plików nie jest już obsługiwana.

Zobacz firmy Apple [v10.12 OS X Diffs interfejsu API](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/APIDiffsMacOS10_12/index.html) dokumentacji, aby uzyskać pełną listę deprecations i zmian.

## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla komputerów Mac](https://developer.xamarin.com/samples/mac/)
- [What's new in 10.12 X systemu operacyjnego](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
