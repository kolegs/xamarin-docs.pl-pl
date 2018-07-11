---
title: Rozwiązywanie problemów z program Xamarin Live Player
description: W tym dokumencie opisano znane problemy dotyczące programu Xamarin Live Player i potencjalne rozwiązania. Omówiono w nim problemów z łącznością, problemy z konfiguracją i nie tylko.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: 3db14db2c64e024ef1c04275661f610f9407dfb7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831335"
---
# <a name="troubleshooting-xamarin-live-player"></a>Rozwiązywanie problemów z program Xamarin Live Player

![Funkcja w wersji zapoznawczej](~/media/shared/preview.png)

W tym artykule opisano niektóre typowe problemy i prezentuje je poprawić.

## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Urządzenie przenośne nie łączy się po skanowanie kodów kreskowych (lub wprowadzanie kodu)

Występuje, gdy urządzenie przenośne, uruchamianie aplikacji Xamarin Live Player nie znajduje się na tej samej sieci, co komputer z uruchomionym IDE. Zapoznaj się z następujących czynności:

- Upewnij się, że urządzeniu i na komputerze znajdują się na tej samej sieci Wi-Fi.
  - Jeśli komputer, również jest podłączony do sieci przewodowej, spróbuj odłączyć połączenia przewodowego.
- Sieci, które mogą być ściśle zabezpieczone (na przykład w niektórych sieciach firmowych używających) blokuje porty wymagane przez program Xamarin Live Player.
- Zamknij aplikację Xamarin Live Player, a następnie uruchom go ponownie.

## <a name="error-while-trying-to-deploy-message-in-ide"></a>Komunikat "Wystąpił błąd podczas próby wdrożenia" w środowisku IDE

**"IOException: nie można odczytać danych z połączenia transportowego: operacja nieblokującej na poziomie gniazd mogłyby spowodować zablokowanie"**

Ten błąd często wystąpił podczas uruchamiania aplikacji Xamarin Live Player urządzenia przenośnego nie jest w tej samej sieci, jak w przypadku komputera z programem Visual Studio; Dzieje się tak często, podczas nawiązywania połączenia z urządzeniem, który wcześniej został pomyślnie powiązany.

* Sprawdź, czy urządzeniu i na komputerze znajdują się w tej samej sieci Wi-Fi.
* Sieci, które mogą być ściśle zabezpieczone (na przykład w niektórych sieciach firmowych używających) blokuje porty wymagane przez program Xamarin Live Player. Następujące porty są wymagane dla aplikacji Xamarin Live Player:
  * 37847 — dostęp do sieci wewnętrzne 
  * 8090 — dostęp do sieci zewnętrzne

## <a name="manually-configure-device"></a>Ręczne konfigurowanie urządzenia

Jeśli nie nawiązanie połączenia z urządzeniem za pośrednictwem sieci Wi-Fi można spróbować ręcznie skonfigurować swoje urządzenia za pomocą pliku konfiguracji wykonując następujące kroki:

**Krok 1. Otwórz plik konfiguracji**

Przejdź do folderu dane aplikacji:

* Windows: **%userprofile%\AppData\Roaming**
* System macOS: **~/Users/$USER/.config**

W tym folderze znajdzie **PlayerDeviceList.xml** Jeśli nie istnieje należy ją utworzyć.

**Krok 2: Uzyskaj adres IP**

W aplikacji Xamarin Live Player, przejdź do **o > Test połączenia > Uruchom Test połączenia**.

Zanotuj adres IP będzie potrzebny adres IP, podczas konfigurowania urządzenia na liście.

**Krok 3: Pobierz parowanie kodu**

Wewnątrz naciśnij aplikacji Xamarin Live Player **pary** lub **pary ponownie**, naciśnij klawisz **Wprowadź ręcznie**. Kod liczbowy zostanie wyświetlony, w którym należy zaktualizować plik konfiguracji.

**Krok 4: Generowanie identyfikatora GUID**

Przejdź do: https://www.guidgenerator.com/online-guid-generator.aspx i wygenerować nowy identyfikator guid i upewnij się, znajduje się na wielkie litery.

**Krok 5. Konfigurowanie urządzenia**

Otwórz **PlayerDeviceList.xml** w górę w edytorze, np. programu Visual Studio lub Visual Studio Code. Należy ręcznie skonfigurować urządzenie, w tym pliku. Domyślnie ten plik powinien zawierać następujące puste `Devices` — element XML:

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

**Zamknij i otwórz ponownie program Visual Studio.** Urządzenie powinno wyświetlane na liście.

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>Komunikat "typu lub przestrzeni nazw nie można odnaleźć" w środowisku IDE

Upewnij się, że wybrano **projekt startowy** odpowiadający danemu typowi urządzenia (iOS lub Android) a konfiguracji jest zgodny z tego typu urządzenia (np.) **Debugowanie | symulatora telefonu iPhone** dla systemu iOS).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>Komunikat "Konstruktor typu"InterpretedXamarin.Forms.Button"nie znaleziono" w odtwarzaczu

Niektóre klasy system nie może być zastąpiona, na przykład:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs:"Resource.Layout"nie zawiera definicji"Main""

Ten błąd występuje w przypadku projektów dla systemu Android przy użyciu interfejsów użytkownika zdefiniowane w plikach AXML.
AXML pliki nie są obecnie obsługiwane w aplikacji Xamarin Live Player.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android narzędzi i karty renderować niepoprawnie przy użyciu zestawu narzędzi Xamarin.Forms

Projekty systemu Xamarin.Forms Android, należy użyć "Toolbar.axml" i "Tabbar.axml" dla nazw plików odpowiedniego układu. Domyślny szablon używa tych nazw; zmieniając ich nazwy spowoduje, że problemy z renderowaniem.

Zgłoś wszelkie dodatkowe problemy na [bugzilla](https://aka.ms/live-player-report-issue).

## <a name="related-links"></a>Linki pokrewne

- [Ograniczenia](~/tools/live-player/limitations.md)
- [Konfiguracja](~/tools/live-player/install.md)
