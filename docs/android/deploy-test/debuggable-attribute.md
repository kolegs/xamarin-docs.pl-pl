---
title: Atrybut debugowalny
ms.prod: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 85b2462605f76e3be8bae589e9fe6cf655741746
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762691"
---
# <a name="debuggable-attribute"></a>Atrybut debugowalny



Aby umożliwić debugowanie, Android obsługuje protokół debugowania przewodowy Java (JDWP). Jest to technologia umożliwiająca przełączanie narzędzi, takich jak ADB do komunikowania się z maszyny wirtualnej Java. Gdy JDWP odgrywa ważną rolę w czasie projektowania, powinno zostać wyłączone przed publikowania aplikacji.

JDWP może być wartością `android:debuggable` atrybutu w aplikacji systemu Android. Xamarin.Android oferuje następujące sposoby ten atrybut:

1.  Utworzony `AndroidManifext.xml` pliku, a ustawienie `android:debuggable` Brak atrybutu.
2.  W tym `ApplicationAttribute` w `.CS` plików w następujący sposób: `[assembly: Application(Debuggable=false)]` .


Jeśli oba `AndroidManifest.xml` i `ApplicationAttribute` są Pokaż zawartość `AndroidManifest.xml` mają priorytet wyższy niż określonym przez `ApplicationAttribute`.

Jeśli żadna `AndroidManifest.xml` ani `ApplicationAttribute` jest obecny, a następnie wartość domyślną `android:debuggable` atrybutu zależy od tego, czy są generowane symboli debugowania. Jeśli występują symboli debugowania, a następnie ustawi Xamarin.Android `android:debuggable` atrybutu `true`.

Należy pamiętać, że wartość `android:debuggable` atrybutu nie zawsze zależy od konfiguracji kompilacji. Istnieje możliwość dla wersji kompilacji mają `android:debuggable` atrybut na wartość true.


## <a name="related-links"></a>Linki pokrewne

- [Możliwością debugowania aplikacji na rynku systemu Android](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
