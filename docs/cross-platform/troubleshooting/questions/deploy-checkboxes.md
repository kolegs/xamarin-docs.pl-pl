---
title: Wdrażanie pola wyboru wyłączone w programie Configuration Manager
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: c0f116565a2741c62a00ed2a255cfde8c57b8569
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Wdrażanie pola wyboru wyłączone w programie Configuration Manager

Od Xamarin 3.5 projektów platformy Xamarin.iOS wdrażane automatycznie po naciśnięciu **Start** przycisku paska narzędzi lub pobrania **Debuguj > Rozpocznij debugowanie** elementu menu. Należy także ustawić żądany projekt aplikacji platformy Xamarin.iOS jako **projekt startowy** przed uruchomieniem jednym z tych poleceń.

W związku z tym **Wdróż** pola wyboru celowo są wyłączone w programie Visual Studio Configuration Manager dla platformy Xamarin.iOS projektów:

![](deploy-checkboxes-images/configuration.png "Wyświetlane pole wyboru "Wdróż" wyłączone dla projektu platformy Xamarin.iOS w wersji 3.5 Xamarin programu Visual Studio Configuration Manager")

Ta zmiana eliminuje błąd, który może być w starszych wersjach programu Xamarin (wersja 3.3 i starszych), gdy projekt aplikacji platformy Xamarin.iOS nie został ustawiony na wdrażanie:

![](deploy-checkboxes-images/error.png "Okno dialogowe błędu: iPhoneApp1 projekt musi zostać wdrożony, aby można było go uruchomić. Sprawdź, czy projekt jest wybrany do wdrożenia w Menedżerze konfiguracji rozwiązania.")
