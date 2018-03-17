---
title: Praca z ikonami
ms.topic: article
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 6d80ef6bdac7f35b282f6347a0356453a413b39c
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="working-with-icons"></a>Praca z ikonami

Apple Watch rozwiązania wymagają dwa zestawy ikon:

* Ikony aplikacji systemu iOS, które będą wyświetlane na telefonie iPhone.
* Ikony Apple Watch, które będzie renderowany w koło menu czujki i ekrany powiadomień. Ikona aplikacji monitorowania jest także wyświetlany w [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) aplikacji dla systemu iOS.

## <a name="apple-watch-icons"></a>Apple Watch ikon

| | | |
|-|-|-|
|Ikona aplikacji systemu iOS|Pojawi się na telefonie iPhone i uruchomienie aplikacji nadrzędnej|![](icons-images/icon-ios.png)|
|Ikona aplikacji monitorowania|Pojawi się Apple Watch ekranu głównego|![](icons-images/icon-home.png)|
||Pojawi się na powiadomienia czujki|![](icons-images/notification-icon.png)|
||Zostanie wyświetlony w [Apple Watch aplikacja systemu iOS](~/ios/watchos/app-fundamentals/settings.md)|![](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>Konfigurowanie rozwiązania

Aby zapewnić, że aplikacji systemu iOS i aplikacji czujki Pokaż poprawnej nazwy i ikony, wykonaj następujące instrukcje dla każdego projektu:

### <a name="ios-app"></a>Aplikacja systemu iOS

Zapoznaj się [przewodnik ikony aplikacji systemu iOS](~/ios/app-fundamentals/images-icons/app-icons.md) zapewnienie ikony aplikacji systemu iOS są poprawnie skonfigurowane.

#### <a name="infoplist"></a>Info.plist

Ciąg, który jest wyświetlany obok swoją aplikację przy użyciu czujki [Apple Watch ustawienia aplikacji](~/ios/watchos/app-fundamentals/settings.md) jest skonfigurowany w **Info.plist aplikacji dla systemu iOS**.

Upewnij się, że Twoje **Info.plist** ma `CFBundleName` kluczy i wartości (Uwaga: ten plik różni się `CFBundleDisplayName`, można mieć jednocześnie):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch aplikacji

Raz z [aplikacji nadrzędnej](~/ios/watchos/app-fundamentals/parent-app.md) skonfigurował jej ikony, konieczne jest dodanie katalogu zasobów ikona aplikacji do aplikacji czujki.

1. Kliknij prawym przyciskiem myszy projekt aplikacji czujki i wybierz **pliku > Dodaj > Nowy plik... > iOS > katalogu zasobów** można dodać katalogu zasobów do projektu.

 ![](icons-images/newasset.png "Dodaj katalog zasobów do projektu")

2. Kliknij dwukrotnie **AppIcons.appiconset/Contents.json** pliku

  ![](icons-images/xcassets-iconset-sml.png "Zawartość AppIcons")

3. Dodaj wszystkie obrazy watchOS, jak pokazano w tym zrzut ekranu:

  [![](icons-images/appicons-sml.png "Dodaj wszystkie obrazy watchOS, jak pokazano w tym zrzut ekranu")](icons-images/appicons.png#lightbox)

  Zapoznaj się [firmy Apple ikony wskazówki](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html) dla wymaganym rozmiarze (wymiary są także wyświetlane na ekranie). Należy pamiętać, że te ikony będą automatycznie przycinana mają być renderowane w koło.

  Na liście ikoną powinien wyglądać następująco:

  ![](icons-images/xcassets-complete-sml.png "Na liście ikoną w Eksploratorze rozwiązań")

4. Aby zapewnić katalogu zasobów znajduje się w aplikacji, Dodaj następujący klucz i wartość, która ma **Info.plist aplikacji czujki**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcons.appiconset</string>
```

Można sprawdzić, sprawdzając ikony są skonfigurowane poprawne [Apple Watch ustawienia aplikacji](~/ios/watchos/app-fundamentals/settings.md) w iPhone symulator, lub generowania [powiadomień](~/ios/watchos/platform/notifications.md) i potwierdzenie ikony zostanie wyświetlone powiadomienie ekran.

> [!NOTE]
> **Uwaga**: ikony nie może mieć kanału alfa (aplikacji zostaną odrzucone podczas przesyłania sklepu z aplikacjami, jeśli występuje kanał alfa). Można sprawdzić, czy kanał alfa istnieje i usuń go [w systemie Mac OS X przy użyciu aplikacji w wersji zapoznawczej](~/ios/watchos/troubleshooting.md#noalpha).


## <a name="related-links"></a>Linki pokrewne

- [Przewodnik dotyczący ikona & obrazy firmy Apple](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html)
