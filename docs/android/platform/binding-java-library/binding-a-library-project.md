---
title: "Powiązanie projektu biblioteki programu Eclipse"
description: "W tym przewodniku opisano sposób korzystania z szablonów projektu platformy Xamarin.Android powiązać Eclipse Android projektu biblioteki."
ms.topic: article
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 7b1314c12bf97a2fa21911c747e3066858116a5f
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="binding-an-eclipse-library-project"></a>Powiązanie projektu biblioteki programu Eclipse

_W tym przewodniku opisano sposób korzystania z szablonów projektu platformy Xamarin.Android powiązać Eclipse Android projektu biblioteki._


## <a name="overview"></a>Omówienie

Mimo że. Pliki AAR stają się coraz bardziej normą dystrybucji biblioteki systemu Android, w niektórych przypadkach należy utworzyć powiązanie dla *projektu biblioteki systemu Android*. Biblioteka systemu android projekty są specjalne projektów systemu Android, które zawierają możliwe do udostępnienia kodu i zasobów, które mogą odwoływać się projekty aplikacji systemu Android. Zazwyczaj można powiązać projektu biblioteki systemu Android po utworzeniu biblioteki w środowisku Eclipse IDE.
Ten przewodnik zawiera przykłady sposobu tworzenia projektu biblioteki systemu Android. ZIP ze struktury katalogów projektu Eclipse.

Projekty biblioteki systemu android różnią się od standardowych projektów dla systemu Android nie są skompilowane w APK i nie są w ich własnych, można wdrożyć na urządzeniu. Zamiast tego projektu biblioteki systemu Android ma być odwołuje się projekt aplikacji systemu Android. Po utworzeniu projektu aplikacji systemu Android, najpierw kompilowania projektu biblioteki systemu Android. Projekt aplikacji systemu Android będą pochłonięta w projekcie skompilowanej biblioteki systemu Android i dołączać kodu i zasobów do APK do dystrybucji. Z powodu tej różnicy tworzenia powiązania dla projektu biblioteki systemu Android są nieco inne niż Tworzenie powiązań dla języka Java. JAR lub. Plik AAR.



## <a name="walkthrough"></a>Wskazówki

Aby korzystać z projektu biblioteki systemu Android w projekcie platformy Xamarin.Android Java — wiązanie należy skompilować projektu biblioteki systemu Android w programie Eclipse. Poniższy zrzut ekranu przedstawia przykład jednego projektu biblioteki systemu Android po kompilacji: 

[![Przykład projektu biblioteki w programie Eclipse](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Zwróć uwagę, że kod źródłowy z projektu biblioteki systemu Android została skompilowana do tymczasowej. Plik JAR o nazwie **android mapviewballoons.jar**, i zasoby skopiowane do **bin/res/Brak** folderu. 

Po projektu biblioteki systemu Android został skompilowany w środowisku Eclipse, jego mogą następnie zostać powiązany za pomocą projektu platformy Xamarin.Android Java — wiązanie. Pierwszy. Musi zostać utworzony plik ZIP, który zawiera **bin** i **res** folderów projektu biblioteki systemu Android. Należy usunąć interwencji **Brak** podkatalogu tak, aby zasoby znajdują się w **bin/res**. Poniższy zrzut ekranu przedstawia zawartość jednego takiego. Plik ZIP: 

[![Zawartość .zip projektu biblioteki systemu Android](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

To. Plik ZIP jest dodawane do projektu platformy Xamarin.Android Java — wiązanie, jak pokazano na poniższym zrzucie ekranu:

[![Dodane do projektu języka Java powiązanie zip](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

Zwróć uwagę, że działanie kompilacji. Plik ZIP został ustawiony automatycznie **LibraryProjectZip**.

Jeśli istnieją. Pliki JAR, które są wymagane przez projekt biblioteki systemu Android, powinny zostać dodane do **Jars** folderu projektu biblioteki powiązanie Java i **Akcja kompilacji** ustawioną **ReferenceJar**. Na przykład widać na poniższym zrzucie ekranu: 

[![Akcja ustawioną ReferenceJar kompilacji](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

Po zakończeniu tych czynności można użyć projektu platformy Xamarin.Android Java — wiązanie, zgodnie z opisem na wcześniej w tym dokumencie.

> [!NOTE]
> Kompilowanie projektów biblioteki systemu Android w innych IDEs nie jest obsługiwana w tej chwili. Inne IDEs nie można utworzyć w tej samej struktury katalogów lub pliki w **bin** folder jako Eclipse. 


## <a name="summary"></a>Podsumowanie

W tym artykule Rezygnacja proces powiązania projektu biblioteki systemu Android. Budujemy projektu biblioteki systemu Android w programie Eclipse następnie utworzyliśmy pliku zip z **bin** i **res** folderów projektu biblioteki systemu Android. Następnie użyliśmy tego zip do tworzenia projektu platformy Xamarin.Android Java — wiązanie. 

