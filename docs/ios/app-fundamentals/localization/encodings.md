---
title: Internationalization Encodings
ms.topic: article
ms.prod: xamarin
ms.assetid: F5117294-28BB-4583-B6A0-A339B050FDE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 3829d8ed94bab482b0da9e52e5ee6e1f488e3992
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="internationalization-encodings"></a>Internationalization Encodings

Nie wszystkie kodowania są domyślnie dołączone w bibliotece klas platformy Xamarin.iOS.

Aby zmniejszyć rozmiar aplikacji, Xamarin.iOS nie zawiera określonego kodowania i masz nakazać programowi mtouch uwzględnienie zestawy zawierające kodowania, które są potrzebne do obsługi.

Można to zrobić, wybierając dodatkowe kodowania z okienka kompilacji/Zaawansowane z systemem iOS w programie Visual Studio dla komputerów Mac lub Visual Studio:

 [![](encodings-images/00.png "Wybranie kodowania dodatkowe")](encodings-images/00.png#lightbox)

 [![](encodings-images/00a.png "Wybranie kodowania dodatkowe")](encodings-images/00a.png#lightbox)

Możesz wybrać jeden z nich:

-  CJK: Chineese, japońskim i koreańskim
-  mideast: arabskiego, hebrajskiego i turecki Latin5.
-  inne: cyrylica, Bałtyckiego wietnamski, ukraiński i tajski
-  rzadkie: EBCDIC kodowania i innych rzadkich strony kodowe
-  zachodnie: języki alfabetu łacińskiego, Wielkanoc i Europa Zachodnia
-  wszystkie


 <a name="cjk" />


## <a name="cjk"></a>cjk

-  CP51932
-  CP932
-  CP936
-  CP949
-  CP950
-  CP54936


 <a name="mideast" />


## <a name="mideast"></a>mideast

-  CP1254
-  CP1255
-  CP1256
-  CP28596
-  CP28598
-  CP28599
-  CP38598


 <a name="other" />


## <a name="other"></a>Inne

-  CP1251
-  CP1257
-  CP1258
-  CP20866
-  CP21866
-  CP28594
-  CP28595
-  CP57002
-  CP874


 <a name="rare" />


## <a name="rare"></a>rzadkie

-  CP1026
-  CP1047
-  CP1140
-  CP1141
-  CP1142
-  CP1143
-  CP1144
-  CP1145
-  CP1146
-  CP1147
-  CP1148
-  CP1149
-  CP20273
-  CP20277
-  CP20278
-  CP20280
-  CP20284
-  CP20285
-  CP20290
-  CP20297
-  CP20420
-  CP20424
-  CP20871
-  CP21025
-  CP37
-  CP500
-  CP708
-  CP852
-  CP855
-  CP857
-  CP858
-  CP862
-  CP864
-  CP866
-  CP869
-  CP870
-  CP875


 <a name="west" />


## <a name="west"></a>Zachodnie

-  CP10000
-  CP10079
-  CP1250
-  CP1252
-  CP1253
-  CP28592
-  CP28593
-  CP28597
-  CP28605
-  CP437
-  CP850
-  CP860
-  CP861
-  CP863
-  CP865

