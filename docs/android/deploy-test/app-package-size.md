---
title: Rozmiar pakietu aplikacji
description: W tym artykule sprawdza elementów składowych pakiet aplikacji platformy Xamarin.Android i skojarzone strategie, których można użyć do wdrożenia pakietu wydajne podczas debugowania i wydania etapy tworzenia.
ms.prod: xamarin
ms.assetid: 8D70CDDD-3D3C-9949-8045-AB8F93D18E74
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: babe45c39f6a69dd9384f3bce8fe28ada31ebfdf
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764427"
---
# <a name="application-package-size"></a>Rozmiar pakietu aplikacji

_W tym artykule sprawdza elementów składowych pakiet aplikacji platformy Xamarin.Android i skojarzone strategie, których można użyć do wdrożenia pakietu wydajne podczas debugowania i wydania etapy tworzenia._


## <a name="overview"></a>Omówienie

Xamarin.Android używa różnych mechanizmów, aby zminimalizować rozmiar pakietów przy zachowaniu się, że wydajne debug i release procesu wdrażania. W tym artykule firma Microsoft Spójrz na wersji platformy Xamarin.Android i debugować przepływ pracy wdrożenia i jak platformy Xamarin.Android gwarantuje, że firma Microsoft kompilacji i wydania małych pakietów aplikacji.


## <a name="release-packages"></a>Pakiety wersji

Aby wysłać pełni zawartych w niej aplikacji, pakietu musi zawierać aplikacji, skojarzone biblioteki zawartości, Mono środowiska uruchomieniowego i wymaganych zestawów biblioteki klasy podstawowej (BCL). Na przykład jeśli traktujemy domyślny szablon "Hello World" zawartość kompilacji pełny pakiet będzie wyglądać następująco:

[![Rozmiar pakietu przed konsolidatora](app-package-size-images/hello-world-package-size-before-linker.png)](app-package-size-images/hello-world-package-size-before-linker.png#lightbox)

Pobieranie większe niż chcielibyśmy jest 15,8 MB. Problem jest bibliotek BCL jako obejmują one mscorlib, systemu i Mono.Android, które zawierają wiele składników niezbędnych do uruchomienia aplikacji. Jednak również udostępniają funkcje, które być może nie jest używana w aplikacji, dlatego może być wyłączenie tych składników.

Gdy mamy utworzyć aplikację do dystrybucji, możemy wykonać proces, nazywany konsolidację, który sprawdza aplikacji i usunie cały kod, który nie jest używany bezpośrednio. Ten proces jest podobny do funkcji który [wyrzucanie elementów bezużytecznych](~/android/internals/garbage-collection.md) miejsce przydzielone stosu pamięci. Jednak zamiast działają na obiektach, łączenie działa przez kod. Na przykład istnieje cały obszar nazw w System.dll do wysyłania i odbierania wiadomości e-mail, ale jeśli aplikacja nie korzystać z tej funkcji, że kod jest po prostu marnować miejsca. Po uruchomieniu konsolidator w aplikacji Hello World, naszych pakietu teraz wygląda następująco:

[![Rozmiar pakietu po konsolidatora](app-package-size-images/hello-world-package-size-after-linker.png)](app-package-size-images/hello-world-package-size-after-linker.png#lightbox)

Jak widać, spowoduje to usunięcie znaczną ilość BCL, który nie był używany. Należy pamiętać, że rozmiaru końcowego BCL zależy od tego, jakie faktycznie aplikację. Na przykład jeśli firma Microsoft Przyjrzyjmy się bardziej znaczące przykładowej aplikacji o nazwie ApiDemo, firma Microsoft Zobacz, czy składnik BCL ma zwiększać rozmiar, ponieważ ApiDemo używa więcej BCL niż Hello, World jest:

![Rozmiar pakietu ApiDemo po konsolidacji](app-package-size-images/api-demo-package-size-after-linker.png)

Jak pokazano poniżej, rozmiar pakietu aplikacji będzie zazwyczaj około 2.9 MB większym niż aplikacji i jego zależności.


## <a name="debug-packages"></a>Debugowanie pakietów

Elementy są obsługiwane inaczej dla kompilacje debugowania. Podczas ponownego wdrażania wielokrotnie do urządzenia, aplikacji musi być tak szybko, jak to możliwe, więc zoptymalizowana pakietów debugowania dla szybkości wdrożenia, a nie rozmiaru.

Android jest stosunkowo powolne skopiować i zainstalować pakiet, dlatego chcemy rozmiar pakietu, który ma być możliwie jak najmniejszy. Jak wspomniano powyżej, możliwy sposób Minimalizuj rozmiar pakietu jest za pośrednictwem konsolidator. Jednak połączenie jest powolne i chcemy zwykle można wdrożyć tylko części aplikacji, które uległy zmianie od ostatniego wdrożenia. W tym celu oddzielić naszej aplikacji z podstawowe składniki platformy Xamarin.Android.

Firma Microsoft debugowania na urządzeniu po raz pierwszy skopiowane dwóch dużych pakietów o nazwie *udostępnionych środowiska uruchomieniowego* i *udostępnionych platformy*. Udostępnione środowiska uruchomieniowego zawiera Mono środowiska uruchomieniowego i BCL, podczas udostępnionych platformy zawiera zestawy określonego poziomu interfejs API systemu Android:

[![Udostępnione rozmiar pakietu środowiska wykonawczego](app-package-size-images/shared-runtime-package-size.png)](app-package-size-images/shared-runtime-package-size.png#lightbox)

Kopiowanie te podstawowe składniki jest wykonywane tylko raz jako trwa dość nieco czasu, ale umożliwia wszystkie kolejne aplikacje uruchomione w trybie debugowania z nich korzystają. Na koniec skopiowane rzeczywistej aplikacji, która jest mała i szybko:

![Rzeczywiste aplikacji jest mały](app-package-size-images/hello-world-debug-application-no-link.png)

### <a name="fast-assembly-deployment"></a>Zestaw szybkiego wdrażania

*Szybkiego wdrożenia zestawu* opcji kompilacji może służyć do dalszych zmniejszyć rozmiar pakietu instalacji debugowania, z tym zestawy pakietu aplikacji, instalowania zestawy bezpośrednio na urządzeniu tylko raz i kopiować tylko pliki, które zostały zmienione od czasu ostatniego wdrożenia.

Aby włączyć *szybkiego wdrożenia zestawu*, wykonaj następujące czynności:

1.  Projekt systemu Android w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy i wybierz **opcje**.

2.  W oknie dialogowym Opcje projektu wybierz **Android kompilacji** :  

    ![Kompilacja Android opcje projektu](app-package-size-images/fastdev0.png)

3.  Sprawdź **użycie wspólnych wyboru Mono środowiska uruchomieniowego** i **szybkie wdrożenie zestawu** pola wyboru:  

    ![Wybrana opakowanie karcie odpowiednie pola wyboru](app-package-size-images/fastdev.png)

4.  Kliknij przycisk **OK** przycisk, aby zapisać zmiany i zamknąć okno dialogowe Opcje projektu.


Przy następnym jest wbudowana aplikacja dla debugowania, zestawy zostaną zainstalowane bezpośrednio na urządzeniu (jeśli zostały one już), a mniejszy pakietu aplikacji (który nie obejmuje zestawy) zostanie zainstalowany na urządzeniu. To skrócić czas potrzebny na szybkim rozpoczynaniu pracy związanej wprowadzanie zmian aplikacji do testowania.

Zabezpieczanie długiego najpierw wdrożyć w udostępnionych środowiska uruchomieniowego i platformy współdzielonej, za każdym razem, gdy możemy wprowadzić zmianę do aplikacji, możemy wdrożyć nową wersję szybkie i painlessly, dzięki czemu będziemy mogli cyklu fast zmiany/wdrożenia/wykonywania.


## <a name="summary"></a>Podsumowanie

W tym artykule możemy zbadać aspekty wersja platformy Xamarin.Android i tworzenia pakietów z profilu debugowania. Ponadto analizujemy strategie, których używa Mono dla platformy systemu Android do ułatwienia wdrażania pakietu wydajne podczas debugowania i wydania etapy tworzenia.
