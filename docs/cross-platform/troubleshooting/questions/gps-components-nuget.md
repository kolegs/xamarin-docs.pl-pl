---
title: Google Play, ujednolicając naukę usługi składniki i NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 3f5c5f75ae1c7a44537afa59ff4a15d54b1df50b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351486"
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Google Play, ujednolicając naukę usługi składniki i NuGet

## <a name="history"></a>Historia

Używany kilka składników usług Google Play Services i pakiety NuGet:

-   Usługi Google Play (Froyo)
-   Usługi Google Play (Gingerbread)
-   Usługi Google Play (ICS)
-   Usługi Google Play (JellyBean)
-   Usługi Google Play (KitKat)

Google faktycznie tylko JAR dostarczany dwóch plików dla usług Google Play:

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

Rozbieżność istniał, ponieważ naszych narzędzi nie został prawidłowo Poinformuj `aapt.exe` zasobów maksymalny poziom interfejsu API została służący do danej aplikacji. Oznacza to, jeśli próbowaliśmy do Ciebie za pomocą powiązania usług Google Play (KitKat) na niższy poziom interfejsu API, takich jak Gingerbread Odebrano błędy kompilacji.

## <a name="unifying-google-play-services"></a>Ujednolicenie usługi Google Play

W nowszych wersjach platformy Xamarin.Android, możemy teraz sprawdzić `aapt.exe` jakiej wersji maksymalnej zasobów do użycia, więc ten problem nie zniknie dla nas.

Oznacza to, że nie ma rzeczywistych mieć osobne pakiety dla Gingerbread/usługi Udostępnianie połączenia internetowego/JellyBean/KitKat (jednak wciąż potrzebujemy oddzielne powiązania dla Froyo ponieważ jest to plik JAR różnych całkowicie).

W celu ułatwienia deweloperom, firma Microsoft została teraz unified naszych NuGet i składniki pakietów do dwóch:

-   Usługi Google Play (Froyo) (wiąże `google-play-services-froyo.jar`)
-   Usługi Google Play (wiąże `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>Który z nich należy użyć?

Prawie w każdym przypadku należy używać usług Google Play. Jedyny przypadek, za pomocą pakietu (Froyo) jest, jeśli aktywnie przeznaczonych do pracy Froyo. Jedyny przypadek, że ten plik JAR oddzielne istnieje od firmy Google ponieważ Froyo odsetkowi takich urządzeń, oni sami zamierzasz obsługiwać, więc ten plik JAR jest migawką zamrożone, nieobsługiwanych usług Google Play.

### <a name="note-about-gingerbread"></a>Uwaga dotycząca Gingerbread

Gingerbread nie ma domyślnie obsługują fragmentu, a w związku z tym niektóre z klas w powiązaniu nie będzie można używać w aplikacji w czasie wykonywania na urządzeniu Gingerbread. Klasy, takie jak `MapFragment` nie będzie działać w Gingerbread i ich wariant pomocy technicznej, które powinny być używane zamiast `SupportMapFragment`. To Ty dla deweloperów, aby dowiedzieć się, której mają zostać użyte. Ta niezgodność zauważyć przez firmę Google w dokumentacji usług Google Play.

### <a name="what-happens-to-the-old-componentsnugets"></a>Co się dzieje stare składniki/NuGet?

Ponieważ nie są już potrzebne, mamy wyłączone/Delisted następujące składniki/rozszerzeń Nuget:

-   Usługi Google Play (Gingerbread)
-   Usługi Google Play (JellyBean)
-   Usługi Google Play (KitKat)

Istniejące _Google Play Services (usługi Udostępnianie połączenia internetowego)_ Nuget/składnika została zmieniona na _usług Google Play_ i jest przechowywany na bieżąco przyszłości. Wszystkie projekty, które odwołuje się do jednego z pakietów wyłączone/Delisted powinien zostać zaktualizowany do z niego korzystać.

Składniki wyłączone nadal istnieje i powinna być umożliwiająca przywrócenie dla projektów, które nadal występuje do nich, aby uniknąć ich. Podobnie delisted pakiety NuGet nadal istnieje i można go przywrócić. Nie zostaną one zaktualizowane w przyszłości.
