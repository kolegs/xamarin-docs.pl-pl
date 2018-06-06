---
title: Praca z watchOS grupy aplikacji w programie Xamarin
description: W tym dokumencie opisano grupy aplikacji i ich użycia w aplikacji watchOS. Zawarto informacje, jak skonfigurować grupę aplikacji, inicjowanie obsługi wymagań, Entitlements.plist zagadnienia i wdrożenia.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 5736b25af3993e2da794422a1a6f040461532497
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790682"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>Praca z watchOS grupy aplikacji w programie Xamarin


Grupa aplikacji umożliwia różnych aplikacji (lub aplikacji i jej rozszerzenia) dostęp do lokalizacji magazynu udostępnionego pliku. Grupy aplikacji mogą być używane dla danych, takich jak:

- Apple Watch [ustawienia](~/ios/watchos/app-fundamentals/settings.md).
- Udostępnione [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Udostępnione [pliki](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Skonfiguruj grupy aplikacji

Lokalizacji udostępnionej została skonfigurowana przy użyciu [grupy aplikacji](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), która jest skonfigurowana w **certyfikaty, identyfikatory & profile** sekcji na [Centrum deweloperów systemu iOS](https://developer.apple.com/devcenter/ios/). Ta wartość musi się też odwoływać w każdym projekcie **Entitlements.plist**.

### <a name="provisioning"></a>Inicjowanie obsługi administracyjnej

Grupa aplikacji ma identyfikator, który zazwyczaj jest Identyfikatorem pakietu z `group.` prefiks. Na przykład można użyć Identyfikatora pakietu `com.xamarin.WatchSettings` i grupy aplikacji `group.com.xamarin.WatchSettings`.

[![](app-groups-images/app-group-sml.png "Użyj com.xamarin.WatchSettings identyfikator pakietu i group.com.xamarin.WatchSettings grupy aplikacji")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

A także Konfigurowanie profilu inicjowania obsługi administracyjnej **włączyć grupy aplikacji** w **Entitlements.plist** i podaj identyfikator wybrano:

[![](app-groups-images/entitlements-sml.png "Konfigurowanie właściwości i podaj identyfikator")](app-groups-images/entitlements.png#lightbox)


### <a name="deployment"></a>wdrażania

Upewnij się, skonfiguruj grupy aplikacji poprawnie w Twojej [wdrożenia](~/ios/watchos/deploy-test/index.md#App_Groups) inicjowania obsługi administracyjnej.


Aby uzyskać więcej informacji, zobacz [możliwości grupy aplikacji](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) dokumentacji.


## <a name="related-links"></a>Linki pokrewne

- [Firmy Apple udostępnianie danych zawierającego aplikacji](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Doc grupy aplikacji firmy Apple](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
