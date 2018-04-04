---
title: Rozwiązywanie problemów
description: Znane problemy z Xamarin Live Player i sposobu ich rozwiązania.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: f9d5d0e4a2217d48577c60940fb027b3a77416d2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>Rozwiązywanie problemów

![Funkcja w wersji zapoznawczej](~/media/shared/preview.png)

W tym artykule opisano niektóre typowe problemy oraz przedstawiono kroki, aby je poprawić.


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Urządzenie przenośne nie łączy się po skanowania kodów kreskowych (lub wprowadzenie kodu)

Występuje, gdy urządzenie przenośne systemem Xamarin Live Player nie znajduje się w tej samej sieci co komputer z uruchomionym IDE. Sprawdź następujące:

- Upewnij się, że urządzenie i komputer jest w tej samej sieci Wi-Fi.
  - Jeśli komputer jest również połączona z siecią przewodową, spróbuj odłączyć połączenia przewodowego.
- Sieci może być ściśle zabezpieczone (na przykład w niektórych sieciach firmowych), blokuje porty wymagane przez odtwarzacz Live Xamarin.
- Zamknij aplikację platformy Xamarin Player na żywo, a następnie uruchom go ponownie.


## <a name="error-while-trying-to-deploy-message-in-ide"></a>Komunikat "Wystąpił błąd podczas próby wdrożenia" w środowisku IDE

**"IOException: nie można odczytać danych z połączenia transportowego: operacja w gnieździe nieblokującym uniemożliwiają"**

Ten błąd często jest wystąpił, gdy urządzenie przenośne systemem Xamarin Live Player nie znajduje się w tej samej sieci co komputer z uruchomionym Visual Studio; Dzieje się tak często, gdy połączenie do urządzenia, które zostało wcześniej łączyć się pomyślnie.

* Sprawdź, czy urządzeniu i na komputerze są w tej samej sieci Wi-Fi.
* Sieci może być ściśle zabezpieczone (na przykład w niektórych sieciach firmowych), blokuje porty wymagane przez odtwarzacz Live Xamarin. Następujące porty są wymagane dla odtwarzacza Live Xamarin:
  * 37847 — dostęp do sieci wewnętrzne 
  * 8090 — dostęp do sieci zewnętrzne

## <a name="manually-configure-device"></a>Ręczne konfigurowanie urządzenia

Jeśli nie można połączyć się urządzenia za pośrednictwem sieci Wi-Fi można spróbować ręcznie skonfigurować urządzenie za pomocą pliku konfiguracji z następujących kroków:

**Krok 1: Otwórz plik konfiguracji**

Przejdź do folderu dane aplikacji:

* System Windows: **%userprofile%\AppData\Roaming**
* System macOS: **~/Users/$USER/.config**

Ten folder zawiera **PlayerDeviceList.xml** Jeśli nie istnieje należy go utworzyć.

**Krok 2: Uzyskaj adres IP**

W aplikacji platformy Xamarin Player na żywo, przejdź do **o > Test połączenia > Rozpocznij Test połączenia**.

Zwróć uwagę adresu IP, konieczne będzie podanego adresu IP podczas konfigurowania urządzenia.

**Krok 3: Pobierz parowanie kodu**

Wewnątrz naciśnij Xamarin Live Player **pary** lub **pary ponownie**, naciśnij klawisz **ręcznie wprowadź**. Kod liczbowy będą wyświetlane, który będzie konieczne zaktualizowanie pliku konfiguracji.

**Krok 4: Generowania identyfikatora GUID**

Przejdź do: https://www.guidgenerator.com/online-guid-generator.aspx wygenerować nowy identyfikator guid i upewnij się, że znajduje się na wielkie litery.


**Krok 5: Konfigurowanie urządzenia**

Otwórz **PlayerDeviceList.xml** w górę w edytorze, np. programu Visual Studio lub Visual Studio Code. Należy ręcznie skonfigurować urządzenie w tym pliku. Domyślnie plik powinien zawierać następujące pusta `Devices` — element XML:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Dodaj urządzenia z systemem iOS:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>iPhone Player</Name>
<Platform>iOS</Platform>
<AndroidApiLevel>0</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:36:03.9492291Z</LastConnectTimeUtc>
</PlayerDevice>
```


**Dodaj urządzenia z systemem Android:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>Android Player</Name>
<Platform>Android</Platform>
<AndroidApiLevel>24</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:34:42.2332328Z</LastConnectTimeUtc>
</PlayerDevice>
```

**Zamknij i otwórz ponownie program Visual Studio.** Urządzenia powinny być widoczne na liście.


## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>Komunikat "typu lub przestrzeni nazw nie można odnaleźć" w środowisku IDE

Sprawdź, czy wybrano **projekt startowy** odpowiadającego danego typu urządzenia (iOS lub Android) oraz że konfiguracji zgodny z tego urządzenia typu (np.) **Debugowanie | iPhone symulatora** dla systemu iOS).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>Komunikat "Konstruktora dla typu"InterpretedXamarin.Forms.Button"nie można odnaleźć" w Player

Niektóre klasy systemu nie można zastąpić, na przykład:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs:"Resource.Layout"nie zawiera definicji"Main""

Ten błąd występuje dla projektów dla systemu Android z interfejsami użytkownika zdefiniowane w plikach AXML.
AXML pliki nie są obecnie obsługiwane w programie Xamarin Player na żywo.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Karty i narzędzi dla systemu android renderowania niepoprawnie za pomocą platformy Xamarin.Forms

Projekty platformy Xamarin.Forms Android, należy użyć "Toolbar.axml" i "Tabbar.axml" do nazwy plików odpowiednie układu. Domyślny szablon korzysta z tych nazw; Zmiana nazwy ich powoduje problemy renderowania.


Zgłoś wszelkie dodatkowe problemy na [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Linki pokrewne

- [Ograniczenia](~/tools/live-player/limitations.md)
- [Konfiguracja](~/tools/live-player/install.md)
