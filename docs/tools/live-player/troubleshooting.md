---
title: "Rozwiązywanie problemów"
description: "Znane problemy z Xamarin Live Player i sposobu ich rozwiązania."
ms.topic: article
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: d7c5bedb03d7c869be65e3c704bac58a9cdfcbbd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
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

Ten błąd jest często wystąpił, gdy urządzenie przenośne systemem Xamarin Live Player nie znajduje się w tej samej sieci co komputer z uruchomionym IDE; Dzieje się tak często, gdy połączenie do urządzenia, które zostało wcześniej łączyć się pomyślnie.

* Sprawdź, czy urządzeniu i na komputerze są w tej samej sieci Wi-Fi.
* Sieci może być ściśle zabezpieczone (na przykład w niektórych sieciach firmowych), blokuje porty wymagane przez odtwarzacz Live Xamarin. Następujące porty są wymagane dla odtwarzacza Live Xamarin:
  * 37847 — dostęp do sieci wewnętrzne 
  * 8090 — dostęp do sieci zewnętrzne

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
