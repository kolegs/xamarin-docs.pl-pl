---
title: Instalacja zestawu SDK systemu android
description: Visual Studio zawiera Android SDK Manager zastępujący firmy Google autonomiczny zestaw SDK Manager. W tym przewodniku wyjaśniono, jak pobrać narzędzia zestawu SDK systemu Android, platform i inne składniki potrzebne do tworzenie aplikacji platformy Xamarin.Android przy użyciu Menedżera zestawu SDK.
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 45ab1930300ac704da0a1fee25c08d40aa35ac5c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="android-sdk-setup"></a>Instalacja zestawu SDK systemu android

_Visual Studio zawiera Android SDK Manager zastępujący firmy Google autonomiczny zestaw SDK Manager. W tym przewodniku wyjaśniono, jak pobrać narzędzia zestawu SDK systemu Android, platform i inne składniki potrzebne do tworzenie aplikacji platformy Xamarin.Android przy użyciu Menedżera zestawu SDK._


## <a name="overview"></a>Omówienie

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W tym przewodniku wyjaśniono, jak zainstalować i używać Xamarin Android SDK Manager dla programu Visual Studio w systemie Windows (lub [dla komputerów Mac](?tabs=vsmac)).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W tym przewodniku wyjaśniono, jak zainstalować i używać Xamarin Android SDK Manager dla programu Visual Studio dla komputerów Mac (lub [dla systemu Windows](?tabs=vswin)).

> [!NOTE]
> Ten przewodnik dotyczy tylko programu Visual Studio 2017 i Visual Studio dla komputerów Mac.  

-----

Xamarin Android SDK Manager ułatwia Pobierz najnowsze składniki systemu Android, które są potrzebne do tworzenia aplikacji platformy Xamarin.Android.
Zastępuje firmy Google autonomiczny zestaw SDK Manager, który jest przestarzała.

Dlaczego czy chcesz użyć zamiast zestawu SDK Manager, który jest dołączony do zestawu SDK systemu Android Xamarin Android SDK Manager? W wersji 25.2.3 pakietu Narzędzia zestawu SDK systemu Android Google wprowadzono nowe narzędzie do obsługi zestawu SDK systemu Android. To nowe narzędzie  **[sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)**, to narzędzie wiersza polecenia, które zastępuje autonomiczny menedżera interfejsu użytkownika dla zestawu SDK systemu Android. W związku z tym jeśli aktualizację do wersji zestawu SDK narzędzia 26.0.1 (wymagane dla 8.0 dla systemu Android) lub nowszym i chcesz nadal zarządzać zestawu SDK systemu Android za pośrednictwem interfejsu interfejsu użytkownika, należy użyć Xamarin Android SDK Manager.

## <a name="requirements"></a>Wymagania

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby korzystać z platformy Xamarin Android SDK Manager, potrzebne następujące elementy:

- Program Visual Studio 2017 community edition lub nowszy. Wymagany jest program Visual Studio 2017 wersji 15.5 lub nowszej.

- Xamarin dla Visual Studio w wersji 4.5.0 lub nowszej. 

Xamarin Android SDK Manager nie jest zgodny z programem Visual Studio
2015. Użytkownicy programu Visual Studio 2015 powinni używać narzędzi SDK Manager dostarczanych przez firmę Google w zestawie SDK systemu Android.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Visual Studio for Mac 7.0.0.3146 (lub nowsza).

-----

Xamarin Android SDK Manager wymaga również Java Development Kit (który jest automatycznie instalowany z platformy Xamarin.Android).
Używa platformy Xamarin.Android [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), który jest wymagany, jeśli są rozwijającym się na poziomie interfejsu API, 24 lub większa (JDK 8 obsługuje również poziomy interfejsu API starszych niż 24). Można nadal używać [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Jeśli tworzenie specjalnie dla interfejsu API na poziomie 23 lub starszym.

> [!IMPORTANT]
> Xamarin.Android nie obsługuje JDK 9.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installation"></a>Instalacja

Xamarin SDK Manager można dodać do programu Visual Studio 2017 w czasie instalacji. Po zainstalowaniu programu Visual Studio, kliknij przycisk **pojedynczych składników** i przewiń w dół do **rozwoju** sekcji.
Włącz **Xamarin SDK Manager** Jeśli nie jest sprawdzana:

![Włączanie Menedżera zestaw SDK platformy Xamarin z poszczególnymi składnikami](android-sdk-images/win/01-sdk-manager-install.png)

Jeśli zainstalowano już 2017 usługi Visual Studio, zobacz [zmodyfikować 2017 Visual Studio](https://docs.microsoft.com/en-us/visualstudio/install/modify-visual-studio) dla powyższej procedury, aby włączyć program Xamarin SDK Manager postępuj instrukcje na temat sposobu modyfikowania programu Visual Studio. Jeśli zostanie wyświetlony monit o aktualizację SDK Manager, służy tę samą procedurę do zainstalowania programu Xamarin SDK Manager.

Po kliknięciu **Narzędzia > Android > Android SDK Manager** (jak wyjaśniono dalej), Xamarin Android SDK Manager zostanie uruchomiony zamiast Google Android SDK Manager. Jeśli używasz starszej wersji zestawu SDK systemu Android, obsługujący autonomiczny firmy Google Android SDK Manager instalowanie platformy Xamarin Android SDK Manager nie utworzy konflikt &ndash; autonomiczne nadal można uruchomić Menedżera zestaw SDK Google z poza Visual Studio, aby zarządzać zestawu SDK systemu Android.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
-----

 
## <a name="sdk-manager"></a>SDK Manager 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aby uruchomić Menedżera zestawu SDK programu Visual Studio, kliknij przycisk **Narzędzia > Android > Android SDK Manager**:

[![Lokalizacja elementu menu Android SDK Manager](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

**Xamarin Android SDK Manager** otworzy się w **Android SDK i narzędzia** ekranu. Ten ekran ma dwie karty &ndash; **platform** i **narzędzia**:

[![Zrzut ekranu programu Android SDK Manager, Otwórz na karcie platformy](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

**Android SDK i narzędzia** ekranu jest opisany bardziej szczegółowo w poniższych sekcjach.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Aby uruchomić Menedżera zestawu SDK programu Visual Studio dla komputerów Mac, kliknij przycisk **Narzędzia > SDK Manager**:
 
![Lokalizacja elementu menu Android SDK Manager](android-sdk-images/mac/sdkmanager-01.png )

**Android SDK Manager** otworzy się w **okna Preferencje**, który zawiera trzy karty **platform**, **narzędzia**i **Lokalizacje**:

![Zrzut ekranu programu Android SDK Manager, Otwórz na karcie platformy](android-sdk-images/mac/sdkmanager-02.png)

Karty z Xamarin Android SDK Manager są opisane w poniższych sekcjach.

-----



# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-location"></a>Lokalizacja zestawu SDK systemu android

Lokalizacja zestawu SDK systemu Android jest skonfigurowana w górnej części **Android SDK i narzędzia** ekranu, jak pokazano na poprzedniej ilustracji. Ta lokalizacja musi być skonfigurowana prawidłowo przed **platform** i **narzędzia** kart będzie działać prawidłowo. Konieczne może być Ustaw lokalizację zestawu SDK systemu Android dla co najmniej jednego z następujących powodów:

1. Xamarin SDK Manager nie może zlokalizować zestawu SDK systemu Android. 

2. Zainstalowano zestaw SDK systemu Android w innej lokalizacji (z systemem innym niż ustawienie domyślne). 

Aby ustawić lokalizacji zestawu SDK systemu Android, kliknij przycisk &hellip; przycisku do prawej krawędzi **lokalizacji zestawu SDK systemu Android**. Spowoduje to otwarcie **przeglądanie w poszukiwaniu folderu** okna dialogowego do użycia na potrzeby Przechodzenie do lokalizacji zestawu SDK systemu Android. Na poniższym zrzucie ekranu, zestaw SDK systemu Android w obszarze **Program Files (x86)\\Android** jest wybierana:

![Zrzut ekranu przedstawiający okno dialogowe systemu Windows przejdź do folderu znajdowania zestawu sdk systemu android](android-sdk-images/win/05-browse-for-folder.png)

Po kliknięciu **OK**, Xamarin Android SDK Manager będą zarządzać zestawu SDK systemu Android, która jest zainstalowana w wybranej lokalizacji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="locations-tab"></a>Karta lokalizacje

**Lokalizacje** karta ma trzy ustawienia dotyczące konfigurowania lokalizacji zestawu SDK systemu Android, zestawu Android NDK i zestawu SDK Java (JDK). Te lokalizacje, które muszą być skonfigurowane prawidłowo przed **platform** i **narzędzia** kart będzie działać prawidłowo.

Po uruchomieniu SDK Manager automatycznie określa ścieżkę dla każdego zainstalowanego pakietu i wskazuje, że nie **znaleziono** przez umieszczenie ikoną z zielonym znacznikiem wyboru obok ścieżki:

![Zrzut ekranu przedstawiający kartę lokalizacje](android-sdk-images/mac/sdkmanager-03.png)

Kliknij przycisk **resetowania do ustawień domyślnych** przycisk, aby spowodować SDK Manager do wyszukania zestawu SDK, zestawu NDK i JDK ich lokalizacji domyślnej. 

Zazwyczaj **lokalizacje** kartę, aby zmodyfikować lokalizacji zestawu SDK systemu Android i/lub Java JDK. Nie należy zainstalować zestaw NDK umożliwiające tworzenie aplikacji platformy Xamarin.Android &ndash; zestawu NDK jest używana tylko wtedy, gdy trzeba utworzyć części aplikacji przy użyciu kodu natywnego języków, takich jak C i C++.

-----


### <a name="tools-tab"></a>Karta Narzędzia

**Narzędzia** karta zawiera listę _narzędzia_ i _dodatki_. Ta karta służy do instalowania narzędzia zestawu SDK systemu Android, narzędzi platformy i narzędzi do kompilacji.
Ponadto można zainstalować emulatora systemu Android niskiego poziomu debugera (LLDB), zestawu NDK, HAXM przyspieszenia i bibliotek Google Play.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Na przykład, aby pobrać pakiet Emulator systemu Google Android, kliknij znacznik wyboru obok pozycji **emulatora systemu Android** i kliknij przycisk **Zastosuj zmiany** przycisk:

[![Instalowanie emulatora systemu Android na karcie Narzędzia](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Na przykład, aby pobrać pakiet Emulator systemu Google Android, kliknij znacznik wyboru obok pozycji **emulatora systemu Android** i kliknij przycisk **Zainstaluj aktualizacje** przycisk:

![Instalowanie emulatora systemu Android na karcie Narzędzia](android-sdk-images/mac/sdkmanager-08.png)

-----


Mogą być wyświetlane okno dialogowe z komunikatem, _niektóre składniki mogą być aktualizowane. Czy chcesz zaktualizować je teraz?_ Kliknij przycisk **Tak**. Następnie przedstawiono okno dialogowe akceptacji licencji:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ekran akceptacji licencji](android-sdk-images/win/07-license-acceptance.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Ekran akceptacji licencji](android-sdk-images/mac/sdkmanager-09.png)

-----

Kliknij przycisk **Akceptuj** Jeśli akceptujesz warunki i postanowienia. W dolnej części okna pasek postępu wskazuje postęp pobierania i instalacji. Po zakończeniu instalacji, **narzędzia** kartę wyświetli wybranych narzędzi i dodatki zostały zainstalowane.



### <a name="platforms-tab"></a>Karta platformy

**Platform** karta zawiera listę wersji zestawu SDK platformy oraz innych zasobów (takich jak obrazy systemu) dla każdej platformy.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zrzut ekranu okienka platformy](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Zrzut ekranu okienka platformy](android-sdk-images/mac/sdkmanager-11.png)

-----

Ten ekran zawiera listę wersji dla systemu Android (takich jak **Android 7.0**), Nazwa kodu (**nugacie**), poziom interfejsu API (takich jak **24**) oraz stan (**zainstalowana** po zainstalowaniu platformy). Możesz użyć **platform** kartę, aby zainstalować składniki na poziomie interfejsu API systemu Android, który ma być docelowa (Aby uzyskać więcej informacji o wersji dla systemu Android i poziomy interfejsu API, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md)).

Jeśli zainstalowano wszystkie składniki platformy, pojawi się znacznik wyboru obok nazwy platformy. Jeśli nie wszystkie składniki platformy są zainstalowane, zostanie wypełnione pole dla tej platformy. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Można rozwinąć platformy, aby wyświetlić jego składniki (i zainstalowane składniki), klikając **+** po lewej stronie platformy.
Kliknij przycisk **-** do unexpand składnika dla platformy.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Można rozwinąć platformy, aby wyświetlić jego składniki (i zainstalowane składniki), klikając **strzałka** na lewo od platformy.
Kliknij przycisk **Strzałka w dół** do unexpand składnika dla platformy.

-----

Aby dodać innej platformy w zestawie SDK, następnie kliknij przycisk kliknij pole wyboru obok platform do wyboru wydaje się zainstalować wszystkie jego składniki **Zastosuj zmiany**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Przykład dodawania składników systemu Android nugacie 7.1 do zestawu SDK systemu Android](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Przykład dodawania składników systemu Android 4.4 do zestawu SDK systemu Android](android-sdk-images/mac/sdkmanager-12.png)

-----

Aby zainstalować tylko w zestawie SDK kliknij pole wyboru obok platformy raz. Następnie można wybrać poszczególnych składników, które są potrzebne:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Przykład dodawania niektórych składników 7.1 systemu Android](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Przykład dodawania niektórych składników systemu Android 4.4](android-sdk-images/mac/sdkmanager-13.png)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Należy zauważyć, że liczba składniki do zainstalowania jest wyświetlana obok **Zastosuj zmiany** przycisku. W powyższym przykładzie sześciu składniki są gotowe do zainstalowania. Po kliknięciu **Zastosuj zmiany** przycisku, zobaczysz **akceptacji licencji** ekranu:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Należy zauważyć, że liczba składniki do zainstalowania jest wyświetlana obok **Zastosuj zmiany** przycisku. Po kliknięciu **Zastosuj zmiany** przycisku, zobaczysz **akceptacji licencji** ekranu:

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Okno dialogowe akceptacji licencji kartę platformy](android-sdk-images/win/11-license-screen.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Okno dialogowe akceptacji licencji kartę platformy](android-sdk-images/mac/sdkmanager-14.png)

-----

Kliknij przycisk **Akceptuj** Jeśli akceptujesz warunki i postanowienia. To okno dialogowe może wyświetlić więcej niż jeden raz w przypadku wielu składniki do zainstalowania. W dolnej części okna pasek postępu wskaże postępu pobierania i instalacji. Po zakończeniu procesu pobierania i instalacji (to może potrwać kilka minut w zależności od tego, jak wiele składników muszą być pobierane,) dodane składniki są oznaczone znacznikiem i wyświetlane jako **zainstalowana**.

Teraz możesz przystąpić do opracowywania aplikacji dla najnowszej, największym poziomie interfejsu API systemu Android!


 
## <a name="summary"></a>Podsumowanie

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

W tym przewodniku wyjaśniono, jak zainstalować i użyć narzędzia Xamarin Android SDK Manager w programie Visual Studio.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

W tym przewodniku wyjaśniono sposób użycia narzędzia Xamarin Android SDK Manager w programie Visual Studio dla komputerów Mac.

-----


## <a name="related-links"></a>Linki pokrewne

- [Zmiany zestawu narzędzi Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Opis poziomów interfejsu API systemu Android](~/android/app-fundamentals/android-api-levels.md)
- [Informacje o wersji (Google) narzędzia zestawu SDK](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
