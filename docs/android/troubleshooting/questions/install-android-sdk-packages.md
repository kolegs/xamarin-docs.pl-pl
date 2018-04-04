---
title: Pakiety zestawu SDK systemu Android, które należy zainstalować?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/02/2018
ms.openlocfilehash: d1fc8a1c511e86615108d1df841c28256f23191f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="which-android-sdk-packages-should-i-install"></a>Pakiety zestawu SDK systemu Android, które należy zainstalować?

Instalowanie zestawu SDK systemu Android automatycznie nie zawiera wszystkich minimalnych wymaganych pakietów do opracowywania. Podczas poszczególnych deweloperów musi być inna, następujące pakiety zazwyczaj jest wymagana w przypadku programowania z użyciem platformy Xamarin.Android:

## <a name="tools"></a>Narzędzia

Zainstaluj najnowsze narzędzia w folderze Tools w Menedżerze SDK:

- Narzędzia zestawu SDK systemu android
- Narzędzia platformy zestawu SDK systemu android
- Narzędzia kompilacji zestawu SDK systemu android

## <a name="android-platforms"></a>Następujące platformy systemu android

Zainstaluj "Zestawu SDK platformy" dla wersji systemu Android ustawione jako minimalna i docelowej. 

Przykłady:

- Docelowego interfejsu API 23
- Minimalny interfejs API 23

Tylko należy zainstalować zestaw SDK platformy dla interfejsu API 23

- Docelowego interfejsu API 23
- Minimalny interfejs API 15

Należy zainstalować zestaw SDK platformy dla interfejsu API 15 i 23. Należy pamiętać, że nie należy zainstalować poziomy interfejsu API między minimalna i docelowa (nawet jeśli jesteś backporting te poziomy interfejsu API).

## <a name="system-images"></a>Obrazy systemu
Są to tylko wymagane, jeśli chcesz użyć dla systemu Android emulatorów poza pole z Google. 

- [Jak skonfigurować emulatorem domyślnym](~/android/get-started/installation/android-emulator/index.md)

- [Sposób przyspieszenia emulatorem domyślnym](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>Dodatki
Zazwyczaj dodatki do zestawu SDK systemu Android nie są wymagane; jednak zaleca się z nimi zapoznać, ponieważ może być wymagane w zależności od Twojego przypadek użycia.

## <a name="further-reading"></a>Dalsze informacje
Poniższym przewodniku opisano te opcje i przechodzi do szczegółowe informacje o różnych pakietów SDK manager ma niedostępne: [przewodnik konfiguracji systemu Android SDK Manager](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)

