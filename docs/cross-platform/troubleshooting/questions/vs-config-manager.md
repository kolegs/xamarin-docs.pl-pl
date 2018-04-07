---
title: Dlaczego Visual Studio nie zawiera określonej w odwołaniu biblioteki projektu w mojej kompilacji?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: cb9b3689ab6a12d99f9694583cd0fd50a6f5c72c
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Dlaczego Visual Studio nie zawiera określonej w odwołaniu biblioteki projektu w mojej kompilacji

Visual Studio będzie korzystać **programu Configuration Manager** do ustalania, które projekty w rozwiązaniu automatycznie znajdują się w danej konfiguracji kompilacji lub wdrożenia.

Niektóre szablony, które są generowane z projektem określonej w odwołaniu biblioteki mają już określonej w odwołaniu biblioteki dołączony w konfiguracji. Jednak w przeciwnym razie trzeba będzie ręcznie ustawić.

## <a name="how-to-use-the-configuration-manager"></a>Jak używać programu Configuration Manager

1. Otwórz **kompilacji > programu Configuration Manager**
2. Wybierz konfigurację, aby dostosować, np. **Debug | iPhone**
3. Zaznacz pola wyboru dla projektów, które mają zostać uwzględnione.

> [!NOTE]
> Szarym pola są obsługiwane automatycznie i nie wymagają żadnych zmian.

Screencast następujące kroki: [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
