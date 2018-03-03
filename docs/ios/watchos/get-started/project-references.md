---
title: "Odwołania do projektu"
description: "Wyjaśnienie relacji między aplikacji dla systemu iOS, obejrzyj aplikacji i obejrzyj rozszerzenia."
ms.topic: article
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 2fd39ed46a070d091f0fd5480cc782d319ec15bd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="project-references"></a>Odwołania do projektu

_Wyjaśnienie relacji między aplikacji dla systemu iOS, obejrzyj aplikacji i obejrzyj rozszerzenia._

Trzy projekty w rozwiązaniu watchOS *automatycznie skonfigurowana* można odwoływać się siebie w określony sposób. w przypadku aplikacji watchOS 3 wbudowane i powiązane poprawnie. Te odwołania do projektu i ustawienia identyfikatora pakietu są opisane poniżej odwołania.

## <a name="project-references"></a>Odwołania do projektu

Dwukrotne kliknięcie na węzłach odwołań dla każdego projektu, aby wyświetlić odwołania:

- **aplikacja iPhone** odwołania **aplikacji czujki**

![](project-references-images/catalog-reference1.png "iPhone aplikacja odwołuje się do aplikacji czujki")

- **Obejrzyj aplikacji** odwołania **rozszerzenia aplikacji czujki**

![](project-references-images/catalog-reference2.png "iPhone aplikacja odwołuje się do aplikacji czujki")


 - **Rozszerzenia aplikacji czujki** nie odwołuje się do jednego z innych projektów

![](project-references-images/catalog-reference3.png "Rozszerzenie aplikacji czujki nie odwołuje się do innych projektów")



## <a name="bundle-identifiers"></a>Identyfikatory pakietu

Należy również upewnić się, że Twoje **identyfikatorów pakietu** są poprawne.
Wszystkie trzy projekty powinny mieć *tego samego* prefiks identyfikatora, z projektami dwóch czujki o wstępnie zdefiniowanych rozszerzeń `watchkitextension` i `watchkitapp`w następujący sposób (dla **WatchKitCatalog** przykład:)

 - Projekt Unified Xamarin.iOS — `com.xamarin.WatchKitCatalog`

 - Projekt rozszerzenia WatchKit- `com.xamarin.WatchKitCatalog.watchkitextension`

 - Projekt aplikacji Czujka — `com.xamarin.WatchKitCatalog.watchkitapp`

Ponadto upewnij się, że te **Info.plist** ustawienia są prawidłowe:

 - Projekt aplikacji czujki `WKCompanionAppBundleIdentifier` odpowiada identyfikator pakietu aplikacji kontenera nadrzędnego / (ie. ten, który jest uruchamiany na telefonie iPhone);

 - Projekt rozszerzenia zestawu czujki **identyfikator pakietu WKApp** odpowiada identyfikator projektu aplikacji czujki pakietu.

Identyfikatory można edytować, klikając dwukrotnie **Info.plist** pliku w każdym projekcie.

Ten zrzut ekranu **rozszerzenia czujki** plik Info.plist, przedstawiający **czujki aplikacji** również identyfikator:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
![](project-references-images/infoplist-extension.png "Ten zrzut ekranu jest plik Info.plist rozszerzenia czujki")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
![](project-references-images/infoplist-extension-vs.png "Ten zrzut ekranu jest plik Info.plist rozszerzenia czujki")

-----

Ten zrzut ekranu **aplikacji czujki** pliku Info.plist.
Bieżący **czujki systemu operacyjnego** wersja jest 8.2, więc **cel wdrożenia** aplikacji czujki powinna być **8.2**. Należy pamiętać, że jeśli Xcode 6.3 jest zainstalowany, ta wartość może być ustawiony na 8.3 — należy ją zmienić 8.2.

![](project-references-images/infoplist-watchapp.png "Obejrzyj plik Info.plist")

Cel wdrożenia dla aplikacji czujki może różnić się od rozszerzenie czujki i aplikacja systemu iOS.

