---
title: Połączenie usługi Google Play Services składniki i NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: asb3993
ms.author: amburns
ms.openlocfilehash: cfd417f4fc01b07b4334259c45472eb24b73abd8
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919875"
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Połączenie usługi Google Play Services składniki i NuGet

### <a name="history"></a>Historia

Są używane do kilku Google Play składników usług i pakiety NuGet:

-   Usługi Google Play (Froyo)
-   Usługi Google Play (pierniki)
-   Google Play Services (ICS)
-   Usługi Google Play (JellyBean)
-   Usługi Google Play (KitKat)

Google faktycznie tylko JAR jest dostarczany dwa pliki dla usług Google Play:

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

Niezgodność istniał ponieważ naszych narzędzi nie została prawidłowo Poinformuj `aapt.exe` zasobów maksymalny poziom interfejsu API został przeznaczonych do danej aplikacji. Oznacza to, jeśli podjęto za pomocą tego powiązania usług Google Play (KitKat) na niższy poziom interfejsu API, takich jak pierniki Odebrano błędów kompilacji.

### <a name="unifying-google-play-services"></a>Połączenie usługi Google Play

W nowszych wersjach platformy Xamarin.Android, możemy teraz sprawdzić `aapt.exe` jakiej wersji maksymalnej zasobów do użycia, więc ten problem nie zniknie firmie Microsoft.

Oznacza to, że nie istnieje rzeczywista Przyczyna mieć osobne pakiety dla pierniki/ICS/JellyBean/KitKat (jednak nadal potrzebujemy oddzielne powiązania dla Froyo, ponieważ jest to plik JAR różnych pól).

Aby ułatwić dla deweloperów, firma Microsoft zostały teraz unified naszych składników i NuGet pakietów do dwóch:

-   (Froyo) usług Google Play (wiąże `google-play-services-froyo.jar`)
-   Usługi Google Play (wiąże `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>Które z nich należy użyć?

Prawie w każdym przypadku należy używać usług Google Play. Tylko przyczyny umożliwia użycie pakietu (Froyo) jest, jeśli aktywnie przeznaczonych Froyo. Tylko powodem tego pliku JAR oddzielne istnieje z Google ponieważ Froyo znajduje się na niewielkim odsetku urządzeń, ich postanowiono zaprzestać obsługi, więc ten plik JAR jest zablokowane, nieobsługiwanej migawkę usług Google Play.

### <a name="note-about-gingerbread"></a>Uwaga dotycząca pierniki

Pierniki nie ma fragmentu obsługuje domyślnie, a w związku z tym niektóre klasy w powiązaniu nie będzie można używać w aplikacji w czasie wykonywania na urządzeniu pierniki. Klas takich jak `MapFragment` nie będą działać na pierniki i ich obsługi variant należy użyć `SupportMapFragment`. Jest to deweloperom wiedzieć, który ma zostać użyty. Ta niezgodność jest odnotowany przez firmę Google w dokumentacji usług Google Play.

### <a name="what-happens-to-the-old-componentsnugets"></a>Co się dzieje na starym składniki/NuGet?

Ponieważ nie są już potrzebne, zostały wyłączone/Delisted składniki/NuGets:

-   Usługi Google Play (pierniki)
-   Usługi Google Play (JellyBean)
-   Usługi Google Play (KitKat)

Istniejące _usług Google Play (ICS)_ składnik/Nuget została zmieniona na _usług Google Play_ i będzie można zapewnić aktualności idąc dalej. Wszystkie projekty odwołujące się do jednego z pakietów wyłączone/Delisted powinny zostać uaktualnione do używania tego.

Wyłączona składników nadal istnieje i powinna być dostępnych dla projektów, które nadal występuje do nich, aby uniknąć dzielenia je. Podobnie delisted pakietów NuGet nadal istnieje i można przywrócić. Ich nie zostanie zaktualizowany idąc dalej.
