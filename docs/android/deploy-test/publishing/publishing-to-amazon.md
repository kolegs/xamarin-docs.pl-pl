---
title: Publikowanie do sklepu z aplikacjami firmy Amazon
ms.prod: xamarin
ms.assetid: A3E9EAC7-2968-8891-CDF2-B73FC0013EC9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: 4805b9685328c5848bcdeab6a0fbaa0c5ed67844
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762529"
---
# <a name="publishing-to-the-amazon-app-store"></a>Publikowanie do sklepu z aplikacjami firmy Amazon

Program dystrybucji aplikacji mobilnej Amazon umożliwia deweloperom aplikacji mobilnej publikowanie ich aplikacji w portalu Amazon. W tej sekcji omówiono pokrótce Amazon sklepu z aplikacjami dla systemu Android. 

[![Sklep z aplikacjami firmy Amazon ekranu](publishing-to-amazon-images/amazon-app-store.png)](publishing-to-amazon-images/amazon-app-store.png#lightbox)

Amazon limit rozmiaru APKs. Jednak jeśli APK jest większy niż 30MB, następnie użyje FTP dla dystrybucji zamiast portalu dystrybucji aplikacji mobilnych firmy Amazon.


## <a name="submitting-apps-binary-info"></a>Przesyłanie aplikacji: Informacje o binarne

Przesyłanie aplikacji do sklepu z aplikacjami firmy Amazon jest procesem podobne do przesyłania aplikacji do witryny Google Play. Aplikacje rozproszone przez firmy Amazon wymagają następujące zasoby: 

-   **Ikona** &ndash; to 114 x 114 PNG z przeźroczystym tłem. Jest wymagane.
-   **Miniatury** &ndash; jest większy wersja ikony powyżej. To 512 x 512 pikseli na przezroczystym tle. Ta ikona jest również obowiązkowe.
-   **Zrzuty ekranu** &ndash; Amazon wymaga co najmniej trzech i maksymalnie 10 zrzutów ekranu. Zrzuty ekranu musi być 1024w x 600h pikseli lub 800 x 480h pikseli. Zarówno w formacie PNG, jak i .jpg formaty są dozwolone.
-   **Promocyjna obrazu** &ndash; w kolejności dla aplikacji, aby zostać umieszczony w rozmieszczanie promocyjne, takie jak strona główna, można opcjonalnie składać promocyjna obrazu. Należy go 1024w x 500h pikseli .png lub .jpg pliku w orientacji poziomej. Nie może mieć żadnych animacji.
-  Mogą być udostępniane aktualizacje do pięciu wideo.



## <a name="approval-process"></a>Proces zatwierdzania

Po przesłaniu aplikacja przechodzi ona przez proces zatwierdzania.
Amazon przejrzeć aplikacji do zapewnienia, że jej działa w sposób opisany w opisie produktu, nie zagrożenie danych klienta i nie utrudni działanie urządzenia. Po zakończeniu procesu zatwierdzania Amazon spowoduje wysłanie powiadomienia i dystrybucji aplikacji.
