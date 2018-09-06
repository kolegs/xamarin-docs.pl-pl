---
title: Definiowanie zestawu Android SDK dla platformy Xamarin.Android
description: Visual Studio zawiera Menedżer zestawów SDK systemu Android, która umożliwia pobieranie narzędzi zestawu SDK systemu Android, platform i inne składniki, które są potrzebne do opracowywania aplikacji platformy Xamarin.Android.
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/09/2018
ms.openlocfilehash: 55b6c10a9014b89534c37faeac2faf84f02339cd
ms.sourcegitcommit: b6f3e55d4f3dcdc505abc8dc9241cff0bb5bd154
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780599"
---
# <a name="setting-up-the-android-sdk-for-xamarinandroid"></a>Definiowanie zestawu Android SDK dla platformy Xamarin.Android

_Visual Studio zawiera Menedżer zestawów SDK systemu Android, która umożliwia pobieranie narzędzi zestawu SDK systemu Android, platform i inne składniki, które są potrzebne do opracowywania aplikacji platformy Xamarin.Android._


## <a name="overview"></a>Omówienie

Ten przewodnik wyjaśnia, jak używać platformy Xamarin Android SDK Manager w programie Visual Studio i Visual Studio dla komputerów Mac.

> [!NOTE]
> Ten przewodnik dotyczy tylko programu Visual Studio 2017 i Visual Studio dla komputerów Mac.  

Menedżer zestawów Xamarin Android SDK (instalowany jako część **opracowywania aplikacji mobilnych przy użyciu platformy .NET** obciążenia) ułatwia pobieranie najnowszych składników systemu Android, które są potrzebne do tworzenia aplikacji platformy Xamarin.Android. Zastępuje ona autonomiczny firmy Google menedżera zestawów SDK, która została wycofana.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="requirements"></a>Wymagania

Aby korzystać z platformy Xamarin Android SDK Manager, potrzebne następujące elementy:

- Program Visual Studio 2017 (Community, Professional lub Enterprise edition). Visual Studio 2017 w wersji 15.7 lub nowszej jest wymagana.

- Visual Studio Tools for Xamarin w wersji 4.10.0 lub nowszej (instalowany jako część **opracowywania aplikacji mobilnych przy użyciu platformy .NET** obciążenia). 

Menedżer zestawów Xamarin Android SDK nie jest zgodny z programem Visual Studio 2015. Użytkownicy programu Visual Studio 2015 należy używać narzędzi Menedżera zestawów SDK dostarczanych przez firmę Google w zestawie SDK dla systemu Android.

Program Xamarin Android SDK Manager wymaga również zestawu Java Development Kit (który jest instalowany automatycznie z rozszerzeniem Xamarin.Android). Istnieje kilka alternatyw JDK do wyboru:

-   Domyślnie używa platformy Xamarin.Android [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), co jest wymagane, jeśli opracowywanie na poziomie interfejsu API 24 lub nowszej (JDK 8 obsługuje również poziomy interfejsu API starszych niż 24).

-   Będzie można kontynuować używanie [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) przypadku opracowywania specjalnie dla poziom 23 interfejsu API lub starszym.

-   Jeśli używasz programu Visual Studio 15.8 w wersji zapoznawczej 5 lub później, spróbuj przy użyciu dystrybucji Microsoft [dystrybucja Microsoft OpenJDK](openjdk.md) (obecnie w wersji zapoznawczej) zamiast JDK 8.

> [!IMPORTANT]
> Platforma Xamarin.Android nie obsługuje zestaw JDK 9.

 
## <a name="sdk-manager"></a>Menedżer zestawów SDK 

Aby uruchomić Menedżera zestawów SDK w programie Visual Studio, kliknij przycisk **Narzędzia > Android > Menedżer zestawów SDK**:

[![Lokalizacja elementu menu Menedżer zestawów Android SDK](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

Menedżer zestawów SDK, który zostanie otwarty w **zestawy Android SDK i narzędzia** ekranu. Ten ekran ma dwie karty &ndash; **platform** i **narzędzia**:

[![Zrzut ekranu z Menedżera zestawów Android SDK, Otwórz na karcie platformy](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

**Zestawy Android SDK i narzędzia** ekranu jest opisany bardziej szczegółowo w poniższych sekcjach.


### <a name="android-sdk-location"></a>Lokalizacja zestawu SDK systemu android

Lokalizacja zestawu Android SDK jest skonfigurowany w górnej części **zestawy Android SDK i narzędzia** ekranu, jak pokazano na poprzednim zrzucie ekranu. Ta lokalizacja musi być skonfigurowany poprawnie przed **platform** i **narzędzia** kart będzie działać prawidłowo. Konieczne może być Ustaw lokalizację zestawu Android SDK dla co najmniej jednej z następujących powodów:

1. Menedżer zestawów SDK nie może zlokalizować zestawu Android SDK. 

2. Zainstalowano zestaw SDK systemu Android w alternatywnej lokalizacji (innych niż domyślne). 

Aby ustawić lokalizację zestawu Android SDK, kliknij przycisk wielokropka (&hellip;) przycisku z prawej strony **Lokalizacja zestawu SDK systemu Android**. Spowoduje to otwarcie **przeglądanie w poszukiwaniu folderu** okna dialogowego do użycia przejdź do lokalizacji zestawu Android SDK. Poniższy zrzut ekranu zestawu Android SDK w ramach **Program Files (x86)\\Android** jest wybierana:

![Zrzut ekranu przedstawiający okno dialogowe Windows przeglądanie w poszukiwaniu folderu lokalizowanie zestawu sdk systemu android](android-sdk-images/win/05-browse-for-folder.png)

Po kliknięciu **OK**, Menedżer zestawu SDK będą zarządzać zestawu Android SDK, który jest zainstalowany w wybranej lokalizacji.


### <a name="tools-tab"></a>Karta Narzędzia do testowania

**Narzędzia** karta zawiera listę _narzędzia_ i _dodatki_. Ta karta służy do instalacji zestawu Android SDK tools, narzędzia platformy i narzędzia tworzenia.
Ponadto można zainstalować Emulator systemu Android niskiego poziomu debugera (LLDB) zestawu NDK, aparatu HAXM przyspieszenie transmisji i bibliotek sklepu Google Play.


Na przykład, aby pobrać pakiet Emulator systemu Google Android, kliknij znacznik wyboru obok pozycji **emulatora systemu Android** i kliknij przycisk **Zastosuj zmiany** przycisku:

[![Instalowania emulatora systemu Android na karcie Narzędzia](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)

Mogą być wyświetlane okno dialogowe z komunikatem, _następujący pakiet wymaga zaakceptowania jego postanowień licencyjnych przed zainstalowaniem_:

![Ekranu akceptacji licencji](android-sdk-images/win/07-license-acceptance.png)

Kliknij przycisk **Akceptuj** Jeśli akceptujesz warunki i postanowienia. W dolnej części okna pasek postępu wskazuje postęp pobierania i instalacji. Po zakończeniu instalacji, **narzędzia** kartę pokażą, że wybranych narzędzi i dodatki zostały zainstalowane.

### <a name="platforms-tab"></a>Karta platformy

**Platform** karta zawiera listę wersji zestawu SDK platformy wraz z innych zasobów (np. obrazów systemów) dla każdej platformy:

[![Zrzut ekranu przedstawiający okienko platformy](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)

Ten ekran zawiera listę wersji systemu Android (takie jak **system Android 8.0**), Nazwa kodu (**Oreo**), poziom interfejsu API (takie jak **26**) i rozmiary składników dla danej platformy (np. jako **1 GB**). Możesz użyć **platform** kartę, aby zainstalować składniki na poziomie interfejsu API systemu Android, która ma pod kątem. Aby uzyskać więcej informacji o wersji systemu Android i poziomy interfejsu API, zobacz [poziomy interfejsu API systemu Android w interpretacji](~/android/app-fundamentals/android-api-levels.md).

Po zainstalowaniu wszystkich składników platformy znacznik wyboru pojawia się obok nazwy platformy. Jeśli nie wszystkie składniki platformy są zainstalowane, zostanie wypełnione pole dla danej platformy. Można rozwinąć platformy, aby wyświetlić jego składniki (i składniki, które są zainstalowane), klikając **+** po lewej stronie platformy.
Kliknij przycisk **-** do unexpand składnika ofercie platformy.

Aby dodać inną platformą do zestawu SDK, następnie kliknij przycisk kliknij pole wyboru obok platformy do momentu znacznik wyboru pojawia się umożliwia zainstalowanie wszystkich składników, **Zastosuj zmiany**:

[![Przykład dodawania składników dnia Android 7.1 określić wersję Nougat do zestawu SDK systemu Android](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)

Aby zainstalować tylko te elementy, przeznaczonych dla określonej, kliknij pole wyboru obok platformy raz. Następnie możesz wybrać poszczególne składniki, które są potrzebne:

[![Przykład dodawania niektórych składników systemu Android 7.1](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)

Należy zauważyć, że liczba składników do zainstalowania pojawia się obok **Zastosuj zmiany** przycisku. Po kliknięciu **Zastosuj zmiany** przycisku, zostanie wyświetlony **akceptacja licencji** ekranu, jak pokazano wcześniej.
Kliknij przycisk **Akceptuj** Jeśli akceptujesz warunki i postanowienia. To okno dialogowe może zostać wyświetlony w więcej niż jeden raz w przypadku wielu składników do zainstalowania. W dolnej części okna pasek postępu wskazuje postęp pobierania i instalacji. Po zakończeniu procesu pobierania i instalacji (może to potrwać kilka minut, w zależności od tego, jak wiele składników, które muszą zostać pobrane,) dodane składniki są oznaczone znacznikiem i wyświetlane jako **zainstalowane**.

### <a name="respository-selection"></a>Wybieranie repozytorium

Domyślnie Menedżer zestawów SDK pobiera składniki platformy i narzędzi z zarządzanych przez firmę Microsoft repozytorium. Jeśli potrzebujesz dostępu do eksperymentalne alfa/beta platformami i narzędziami, które nie są jeszcze dostępne w repozytorium firmy Microsoft, możesz przełączyć menedżera zestawów SDK, aby używać repozytorium firmy Google. Aby tego przełącznika, kliknij ikonę koła zębatego w prawym dolnym rogu, a następnie wybierz **repozytorium > Google (nieobsługiwany)**:

[![Wybieranie repozytorium firmy Google](android-sdk-images/win/11-google-repo-w157-sml.png)](android-sdk-images/win/11-google-repo-w157.png#lightbox)

Po wybraniu usługa Google repository dodatkowe pakiety mogą być wyświetlane w **platform** karty, które nie były wcześniej dostępne. (W powyższym zrzucie ekranu **Android SDK platformy 28** został dodany, przełączając się usługa Google repository.) Należy pamiętać, że użycie usługa Google repository nie jest obsługiwane i dlatego nie zaleca się codziennym tworzeniu oprogramowania.

Aby przełączyć się do repozytorium obsługiwanych platform i narzędzi, kliknij przycisk **firmy Microsoft (zalecane)**. Spowoduje to przywrócenie listę pakietów i narzędzia do wybór domyślny.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="requirements"></a>Wymagania

Aby korzystać z platformy Xamarin Android SDK Manager, potrzebne następujące elementy:

-   Program Visual Studio dla komputerów Mac w wersji 7.5 (lub nowszego).

Program Xamarin Android SDK Manager wymaga również zestawu Java Development Kit (który jest instalowany automatycznie z rozszerzeniem Xamarin.Android). Istnieje kilka alternatyw JDK do wyboru:

-   Domyślnie używa platformy Xamarin.Android [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), co jest wymagane, jeśli opracowywanie na poziomie interfejsu API 24 lub nowszej (JDK 8 obsługuje również poziomy interfejsu API starszych niż 24).

-   Będzie można kontynuować używanie [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) przypadku opracowywania specjalnie dla poziom 23 interfejsu API lub starszym.

-   Jeśli używasz programu Visual Studio dla komputerów Mac 7.7 lub nowszego, spróbuj użyć dystrybucji Microsoft [dystrybucja Microsoft OpenJDK](openjdk.md) (obecnie w wersji zapoznawczej) zamiast JDK 8.

> [!IMPORTANT]
> Platforma Xamarin.Android nie obsługuje zestaw JDK 9.
 
## <a name="sdk-manager"></a>Menedżer zestawów SDK 

Aby uruchomić Menedżera zestawów SDK w programie Visual Studio dla komputerów Mac, kliknij przycisk **Narzędzia > Menedżer zestawów SDK**:
 
[![Lokalizacja elementu menu Menedżer zestawów Android SDK](android-sdk-images/mac/01-sdk-manager-menu-item-m75-sml.png)](android-sdk-images/mac/01-sdk-manager-menu-item-m75.png#lightbox)

**Menedżer zestawów SDK** zostanie otwarty w **okno preferencji**, który zawiera trzy karty **platform**, **narzędzia**i **Lokalizacje**:

[![Zrzut ekranu z Menedżera zestawów Android SDK, Otwórz na karcie platformy](android-sdk-images/mac/02-sdk-manager-platforms-m75-sml.png)](android-sdk-images/mac/02-sdk-manager-platforms-m75.png#lightbox)

Karty z Menedżera zestawów Android SDK zostały opisane w poniższych sekcjach.


### <a name="locations-tab"></a>Karta lokalizacje

**Lokalizacje** karta ma trzy ustawienia do lokalizacji zestawu Android SDK, zestawu Android NDK i zestawu SDK Java (JDK). Te lokalizacje, które muszą być skonfigurowane prawidłowo przed **platform** i **narzędzia** kart będzie działać prawidłowo.

Podczas uruchamiania Menedżera zestawów SDK automatycznie określa ścieżkę dla każdego zainstalowanego pakietu i wskazuje, że było **znaleziono** , umieszczając Ikona zielonego znacznika wyboru obok ścieżki:

[![Zrzut ekranu przedstawiający kartę lokalizacje](android-sdk-images/mac/03-locations-tab-m75-sml.png)](android-sdk-images/mac/03-locations-tab-m75.png#lightbox)

Kliknij przycisk **Resetuj na domyślne** przycisk, aby spowodować, że Menedżer zestawów SDK do wyszukania w zestawie SDK, zestaw NDK i JDK w lokalizacjach domyślne. 

Zazwyczaj można użyć **lokalizacje** kartę, aby zmodyfikować lokalizację zestawu Android SDK i/lub Java JDK. Nie musisz zainstalować zestaw NDK do opracowywania aplikacji platformy Xamarin.Android &ndash; zestawu NDK jest używana tylko wtedy, gdy jest potrzebne do opracowania części aplikacji przy użyciu kodu natywnego języków, takich jak C i C++.

### <a name="tools-tab"></a>Karta Narzędzia do testowania

**Narzędzia** karta zawiera listę _narzędzia_ i _dodatki_. Ta karta służy do instalacji zestawu Android SDK tools, narzędzia platformy i narzędzia tworzenia.
Ponadto można zainstalować Emulator systemu Android niskiego poziomu debugera (LLDB) zestawu NDK, aparatu HAXM przyspieszenie transmisji i bibliotek sklepu Google Play.

Na przykład, aby pobrać pakiet Emulator systemu Google Android, kliknij znacznik wyboru obok pozycji **emulatora systemu Android** i kliknij przycisk **Zastosuj zmiany** przycisku:

[![Instalowania emulatora systemu Android na karcie Narzędzia](android-sdk-images/mac/04-tools-tab-m75-sml.png)](android-sdk-images/mac/04-tools-tab-m75.png#lightbox)

Mogą być wyświetlane okno dialogowe z komunikatem, _następujący pakiet wymaga zaakceptowania jego postanowień licencyjnych przed zainstalowaniem_:

[![Ekranu akceptacji licencji](android-sdk-images/mac/05-license-acceptance-m75-sml.png)](android-sdk-images/mac/05-license-acceptance-m75.png#lightbox)

Kliknij przycisk **Akceptuj** Jeśli akceptujesz warunki i postanowienia. W dolnej części okna pasek postępu wskazuje postęp pobierania i instalacji. Po zakończeniu instalacji, **narzędzia** kartę pokażą, że wybranych narzędzi i dodatki zostały zainstalowane.


### <a name="platforms-tab"></a>Karta platformy

**Platform** karta zawiera listę wersji zestawu SDK platformy wraz z innych zasobów (np. obrazów systemów) dla każdej platformy:

[![Zrzut ekranu przedstawiający okienko platformy](android-sdk-images/mac/06-platforms-tab-m75-sml.png)](android-sdk-images/mac/06-platforms-tab-m75.png#lightbox)

Ten ekran zawiera listę wersji systemu Android (takie jak **dla systemu Android 8.1**), Nazwa kodu (**Oreo**), poziom interfejsu API (takie jak **27**) i rozmiary składników dla danej platformy (np. jako **1 GB**). Możesz użyć **platform** kartę, aby zainstalować składniki na poziomie interfejsu API systemu Android, która ma pod kątem. Aby uzyskać więcej informacji o wersji systemu Android i poziomy interfejsu API, zobacz [poziomy interfejsu API systemu Android w interpretacji](~/android/app-fundamentals/android-api-levels.md).

Po zainstalowaniu wszystkich składników platformy znacznik wyboru pojawia się obok nazwy platformy. Jeśli nie wszystkie składniki platformy są zainstalowane, zostanie wypełnione pole dla danej platformy. Można rozwinąć platformy, aby wyświetlić jego składniki (i składniki, które są zainstalowane), klikając **strzałkę** na lewo od platformy.
Kliknij przycisk **Strzałka w dół** do unexpand składnika ofercie platformy.

Aby dodać inną platformą do zestawu SDK, następnie kliknij przycisk kliknij pole wyboru obok platformy do momentu znacznik wyboru pojawia się umożliwia zainstalowanie wszystkich składników, **Zastosuj zmiany**:

[![Przykład dodawania wszystkie składniki platformy](android-sdk-images/mac/07-install-all-m75-sml.png)](android-sdk-images/mac/07-install-all-m75.png#lightbox)

Aby zainstalować tylko niektóre składniki, kliknij pole wyboru obok platformy raz. Następnie możesz wybrać poszczególne składniki, które są potrzebne:

[![Przykład dodawania niektórych składników](android-sdk-images/mac/08-individual-components-m75-sml.png)](android-sdk-images/mac/08-individual-components-m75.png#lightbox)

Należy zauważyć, że liczba składników do zainstalowania pojawia się obok **Zastosuj zmiany** przycisku. Po kliknięciu **Zastosuj zmiany** przycisku, zostanie wyświetlony **akceptacja licencji** ekranu, jak pokazano wcześniej.
Kliknij przycisk **Akceptuj** Jeśli akceptujesz warunki i postanowienia. To okno dialogowe może zostać wyświetlony w więcej niż jeden raz w przypadku wielu składników do zainstalowania. W dolnej części okna pasek postępu wskazuje postęp pobierania i instalacji. Po zakończeniu procesu pobierania i instalacji (może to potrwać kilka minut, w zależności od tego, jak wiele składników, które muszą zostać pobrane,) dodane składniki są oznaczone znacznikiem i wyświetlane jako **zainstalowane**.

### <a name="respository-selection"></a>Wybieranie repozytorium

Domyślnie Menedżer zestawów SDK pobiera składniki platformy i narzędzi z zarządzanych przez firmę Microsoft repozytorium. Jeśli potrzebujesz dostępu do eksperymentalne alfa/beta platformami i narzędziami, które nie są jeszcze dostępne w repozytorium firmy Microsoft, możesz przełączyć menedżera zestawów SDK, aby używać repozytorium firmy Google. Aby tego przełącznika, kliknij ikonę koła zębatego w prawym dolnym rogu, a następnie wybierz **repozytorium > Google (nieobsługiwany)**:

[![Wybieranie repozytorium firmy Google](android-sdk-images/mac/09-google-repo-m75-sml.png)](android-sdk-images/mac/09-google-repo-m75.png#lightbox)

Po wybraniu usługa Google repository dodatkowe pakiety mogą być wyświetlane w **platform** karty, które nie były wcześniej dostępne. (W powyższym zrzucie ekranu **Android SDK platformy 28** został dodany, przełączając się usługa Google repository.) Należy pamiętać, że użycie usługa Google repository nie jest obsługiwane i dlatego nie zaleca się codziennym tworzeniu oprogramowania.

Aby przełączyć się do repozytorium obsługiwanych platform i narzędzi, kliknij przycisk **firmy Microsoft (zalecane)**. Spowoduje to przywrócenie listę pakietów i narzędzia do wybór domyślny.

-----

 
## <a name="summary"></a>Podsumowanie

W przewodniku wyjaśniono, jak zainstalować i korzystać z narzędzia Menedżer zestawów SDK platformy Xamarin w programie Visual Studio i Visual Studio dla komputerów Mac.


## <a name="related-links"></a>Linki pokrewne

- [Opis poziomów interfejsu API systemu Android](~/android/app-fundamentals/android-api-levels.md)
- [Zmiany zestawu narzędzi Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
