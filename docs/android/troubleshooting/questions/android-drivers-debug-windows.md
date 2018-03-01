---
title: "Jakie sterowniki USB należy do debugowania dla systemu Android w systemie Windows?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/19/2017
ms.openlocfilehash: 97803c0496c7f5f6ea40e86023caad66c675037e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Jakie sterowniki USB należy do debugowania dla systemu Android w systemie Windows?

## <a name="finding-usb-drivers"></a>Znajdowanie sterowniki USB

Debugowanie na urządzeniu z systemem Android, wdrażając w systemie Windows. Musisz zainstalować zgodny sterownik USB. Domyślnie, który dodaje obsługę urządzeń węzła, zgodnie z opisem w tym miejscu Android SDK Manager zawiera "Sterownik USB Google": [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

Inne urządzenia wymagają sterowników USB, w szczególności opublikowane przez producenta urządzenia. Niektóre linki do typowych producentów znajdują się w tym przewodniku: [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Alternatywne

W zależności od manfacturer może być trudne do śledzenia dokładne sterownik USB potrzebne. Niektóre alternatyw do testowania aplikacji systemu Android opracowany w systemu Windows w tym przy użyciu emulatora systemu Android lub testowania usług zewnętrznych. Oto niektóre z nich:

- [Chmury testowej Xamarin](https://xamarin.com/test-cloud) — usługi uruchamiane na setki rzeczywistym Android urządzeń testowania chmury.

- [Emulator programu Visual Studio dla systemu Android](https://www.visualstudio.com/en-us/features/msft-android-emulator-vs.aspx)

- [Emulator systemu Google Android SDK](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

