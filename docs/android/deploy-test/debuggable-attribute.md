---
title: Atrybut debugowalny
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: db09ebe29b6c404bac892fd76389faf0b9e03d5b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="debuggable-attribute"></a>Atrybut debugowalny

<a name="Overview" />


Aby umożliwić debugowanie, Android obsługuje protokół debugowania przewodowy Java (JDWP). Jest to technologia umożliwiająca przełączanie narzędzi, takich jak ADB do komunikowania się z maszyny wirtualnej Java. Gdy JDWP odgrywa ważną rolę w czasie projektowania, powinno zostać wyłączone przed publikowania aplikacji.

JDWP może być wartością `android:debuggable` atrybutu w aplikacji systemu Android. Xamarin.Android oferuje następujące sposoby ten atrybut:

1.  Utworzony `AndroidManifext.xml` pliku, a ustawienie `android:debuggable` Brak atrybutu.
1.  W tym `ApplicationAttribute` w `.CS` plików w następujący sposób: `[assembly: Application(Debuggable=false)]` .


Jeśli oba `AndroidManifest.xml` i `ApplicationAttribute` są Pokaż zawartość `AndroidManifest.xml` mają priorytet wyższy niż określonym przez `ApplicationAttribute`.

Jeśli żadna `AndroidManifest.xml` i `ApplicationAttribute`, następnie wartość domyślną `android:debuggable` atrybutu zależy od tego, czy są generowane symboli debugowania. Jeśli występują symboli debugowania, a następnie ustawi Xamarin.Android `android:debuggable` atrybutu `true`.

Należy pamiętać, że wartość `android:debuggable` atrybutu nie zawsze zależy od konfiguracji kompilacji. Istnieje możliwość dla wersji kompilacji mają `android:debuggable` atrybut na wartość true.


## <a name="related-links"></a>Linki pokrewne

- [Możliwością debugowania aplikacji na rynku systemu Android](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
