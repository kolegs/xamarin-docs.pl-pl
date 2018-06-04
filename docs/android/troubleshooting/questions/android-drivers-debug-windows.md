---
title: Jakie sterowniki USB należy do debugowania dla systemu Android w systemie Windows?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: 7b3a4c2f807839897a099959fe3a6ea9ec25df78
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732752"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Jakie sterowniki USB należy do debugowania dla systemu Android w systemie Windows?

## <a name="finding-usb-drivers"></a>Znajdowanie sterowniki USB

Debugowanie na urządzeniu z systemem Android, wdrażając w systemie Windows. Musisz zainstalować zgodny sterownik USB. Android SDK Manager obejmuje "Sterownik USB Google" Domyślnie, który dodaje obsługę urządzeń węzła zgodnie z opisem w tym miejscu: [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

Inne urządzenia wymagają sterowników USB, w szczególności opublikowane przez producenta urządzenia. Niektóre linki do typowych producentów znajdują się w tym przewodniku: [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Alternatywne

W zależności od manfacturer może być trudne do śledzenia dokładne sterownik USB potrzebne. Niektóre alternatyw do testowania aplikacji systemu Android opracowany w systemu Windows w tym przy użyciu emulatora systemu Android lub testowania usług zewnętrznych. Oto niektóre z nich:

- [Test aplikacji Centrum](https://docs.microsoft.com/appcenter/test-cloud/) — usługi uruchamiane na setki rzeczywistym Android urządzeń testowania chmury.

- [Emulator programu Visual Studio dla systemu Android](https://www.visualstudio.com/en-us/features/msft-android-emulator-vs.aspx)

- [Debugowanie za pomocą emulatora systemu Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

