---
title: Praca w systemie watchOS ikony w środowisku Xamarin
description: W tym dokumencie opisano różne ikony niezbędne dla aplikacji systemu watchOS oraz sposób konfigurowania rozwiązania, aby uwzględnić te ikony.
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/26/2018
ms.openlocfilehash: e46ecc9d78ccc5dcfbe571c9ec5350fe6c391b7e
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39275927"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>Praca w systemie watchOS ikony w środowisku Xamarin

Apple Watch rozwiązania wymagają dwa zestawy ikon:

* Ikony aplikacji dla systemu iOS, które pojawią się na telefonie iPhone.
* Ikony Apple Watch, które będą renderowane w okręgu w menu wyrażenie kontrolne i ekrany powiadomień. Ikona aplikacji watch pojawia się również w [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) aplikacji dla systemu iOS.

## <a name="apple-watch-icons"></a>Ikony Apple Watch

| | | |
|-|-|-|
|Ikona aplikacji systemu iOS|Pojawi się na telefonie iPhone i rozpoczyna się aplikacja nadrzędna|![Ikona aplikacji systemu iOS](icons-images/icon-ios.png)|
|Obejrzyj ikonę aplikacji|Zostanie wyświetlone na ekranie głównym Apple Watch|![Ikona aplikacji systemu watchOS](icons-images/icon-home.png)|
||Pojawi się w powiadomieniach wyrażenie kontrolne|![ikona powiadomienia systemu watchOS](icons-images/notification-icon.png)|
||Pojawia się w [aplikacji Apple Watch dla systemu iOS](~/ios/watchos/app-fundamentals/settings.md)|![Ikona aplikacji Watch dla systemu iOS](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>Konfigurowanie rozwiązania

Aby upewnić się, że aplikacja dla systemu iOS oraz aplikacji watch pokazują poprawne nazwy i ikony, wykonaj te instrukcje dla każdego projektu:

### <a name="ios-app"></a>Aplikacja systemu iOS

Zapoznaj się [przewodnik ikony aplikacji dla systemu iOS](~/ios/app-fundamentals/images-icons/app-icons.md) zapewnienie ikony aplikacji systemu iOS są poprawnie skonfigurowane.

#### <a name="infoplist"></a>Info.plist

Ciąg, który pojawia się obok aplikacji watch w [aplikacji ustawienia Apple Watch](~/ios/watchos/app-fundamentals/settings.md) jest skonfigurowana w **pliku Info.plist aplikacji dla systemu iOS**.

Upewnij się, że Twoje **Info.plist** ma `CFBundleName` klucza i wartości (Uwaga: Ta zmienna różni się `CFBundleDisplayName`, może mieć jednocześnie):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch aplikacji

Raz swoje [aplikacja nadrzędna](~/ios/watchos/app-fundamentals/parent-app.md) skonfigurował jej ikony, należy dodać katalog zasobów ikon aplikacji do aplikacji na zegarek.

1. Kliknij prawym przyciskiem myszy na projekt aplikacji Watch i wybierz **Plik > Dodaj > Nowy plik... > iOS > Katalog zasobów** można dodać katalog zasobów do projektu.

 ![](icons-images/newasset.png "Dodaj katalog zasobów do projektu")

2. Kliknij dwukrotnie **AppIcon.appiconset/Contents.json** pliku

  ![](icons-images/xcassets-iconset-sml.png "Zawartość AppIcon")

3. Dodaj wszystkie obrazy systemu watchOS, jak pokazano w tym zrzucie ekranu:

  [![](icons-images/appicons-sml.png "Dodaj wszystkie obrazy systemu watchOS, jak pokazano w tym zrzucie ekranu")](icons-images/appicons.png#lightbox)

  Zapoznaj się [firmy Apple ikonę wskazówki](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/menu-icons/) dla wymaganych rozmiarów (wymiary są także wyświetlane na ekranie). Należy pamiętać, że te ikony będą automatycznie przycinane do renderowania w okręgu.

  Na liście ikon powinna wyglądać mniej więcej tak:

  ![](icons-images/xcassets-complete-sml.png "Lista ikon w Eksploratorze rozwiązań")

4. Aby upewnić się, katalog zasobów znajduje się w aplikacji, Dodaj następujący klucz i wartość, która **pliku Info.plist aplikacji Watch**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcon.appiconset</string>
```

Możesz sprawdzić ikony są skonfigurowane poprawna, sprawdzając [aplikacji ustawienia Apple Watch](~/ios/watchos/app-fundamentals/settings.md) w symulatorze dla telefonu iPhone lub generowania [powiadomień](~/ios/watchos/platform/notifications.md) i Potwierdzanie ikona pojawia się powiadomienie ekran.

> [!NOTE]
> Ikony nie może mieć kanał alfa (aplikacja zostanie odrzucone podczas przesyłania App Store Jeśli kanał alfa jest obecna). Możesz sprawdzić, czy istnieje kanał alfa i usuń go [przy użyciu aplikacji (wersja zapoznawcza) w systemie Mac OS X](~/ios/watchos/troubleshooting.md#noalpha).


## <a name="related-links"></a>Linki pokrewne

- [Przewodnik ikonę & obrazy systemu watchOS firmy Apple](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/)
