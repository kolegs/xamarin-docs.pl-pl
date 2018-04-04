---
title: Wprowadzenie do platformy Xamarin.iOS dla programu Visual Studio
description: W tym artykule przedstawiono sposób tworzenia i testowania aplikacji systemu iOS Xamarin przy użyciu programu Visual Studio. Będzie on opisano sposób tworzenie nowych projektów dla systemu iOS, tworzenia aplikacji systemu iOS, a następnie skompilować, testowania i debugowania za pomocą sieci Mac kompilatora Apple hosta i symulator i łańcuch narzędzi kompilacji dla platformy Xamarin za pomocą programu Visual Studio.
ms.prod: xamarin
ms.assetid: bf3c779f-959f-428d-babb-428f363f7e4e
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a8264d3ebd5f294b1b77fbbafd660825d5ce5180
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-xamarinios-for-visual-studio"></a>Wprowadzenie do platformy Xamarin.iOS dla programu Visual Studio

_W tym artykule przedstawiono sposób tworzenia i testowania aplikacji systemu iOS Xamarin przy użyciu programu Visual Studio. Będzie on opisano sposób tworzenie nowych projektów dla systemu iOS, tworzenia aplikacji systemu iOS, a następnie skompilować, testowania i debugowania za pomocą sieci Mac kompilatora Apple hosta i symulator i łańcuch narzędzi kompilacji dla platformy Xamarin za pomocą programu Visual Studio._

Xamarin dla systemu Windows umożliwia iOS aplikacje mają być zapisywane i testowane w programie Visual Studio z sieci Mac świadczenie usług kompilacji i wdrożenia.

W tym artykule opisano kroki, aby zainstalować i skonfigurować narzędzia platformy Xamarin.iOS na każdym komputerze, aby tworzyć aplikacje dla systemu iOS przy użyciu programu Visual Studio.

Tworzenie aplikacji dla systemu iOS w programie Visual Studio oferuje następujące korzyści:

-  Tworzenie rozwiązań i platform dla systemu iOS, Android i Windows aplikacji.
-  Przy użyciu ulubionych narzędzi Visual Studio (takich jak **Resharper** i **Team Foundation Server**) dla wszystkich projektów i platform, łącznie z kodu źródłowego z systemem iOS.
-  Współpraca z znanych IDE, wykorzystując powiązania Xamarin.iOS Apple wszystkich interfejsów API.


<a name="Requirements_and_Installation" />

## <a name="requirements-and-installation"></a>Wymagania i instalacji

Istnieje kilka wymagań, które należy przestrzegać podczas opracowywania dla systemu iOS w programie Visual Studio. Jako krótko opisane powyżej w przeglądzie Mac jest wymagany do kompilacji pliki IPA i nie można wdrożyć aplikacje na urządzeniu bez certyfikatów firmy Apple i narzędzia podpisywania kodu.

Brak dostępnych kilka opcji konfiguracji, można zdecydować, który najlepiej pasuje do potrzeb. Są one wymienione poniżej:

-  Użyj Mac jako główny programowania maszyny i uruchomić maszynę wirtualną systemu Windows z zainstalowanego programu Visual Studio. Firma Microsoft zaleca używanie oprogramowania maszyny Wirtualnej, takich jak [równoleżników](http://www.parallels.com/products/desktop/) lub [VMWare](http://www.vmware.com/products/fusion/) .
-  Mac należy użyć tylko jako hosta usługi kompilacji. W tym scenariuszu byłoby simply podłączone do tej samej sieci co maszyna systemu Windows z [niezbędne](~/cross-platform/get-started/installation/windows.md#installation) narzędzia są zainstalowane.


W obu przypadkach należy wykonaj następujące kroki:

- [Zainstaluj narzędzia platformy Xamarin.iOS na hoście z systemem Mac](https://docs.microsoft.com/visualstudio/mac/installation)
- [Konfigurowanie komputerów Mac](~/ios/get-started/installation/windows/index.md#configuring)
- [Zainstaluj narzędzia platformy Xamarin w systemie Windows](~/cross-platform/get-started/installation/windows.md)

Aby opracować za pomocą platformy Xamarin w programie Visual Studio, należy używać **co najmniej** Visual Studio 2015 Professional lub nowszego. Zostanie Xamarin **działa** z Express wersje programu Visual Studio, ponieważ nie obsługują dodatków.

## <a name="connecting-to-the-mac"></a>Łączenie z adresem MAC

Możesz połączyć się z komputera Mac kompilacji hosta, albo za pośrednictwem ikony na pasku narzędzi programu Visual Studio (dostarczanie aplikacji systemu iOS jest otwarty):

[![](introduction-to-xamarin-ios-for-visual-studio-images/xma1a.png "Połącz ikonę Mac")](introduction-to-xamarin-ios-for-visual-studio-images/xma1a.png#lightbox)

Lub przechodząc do **Narzędzia > Opcje** w Visual Studio i wybierając **Xamarin > Ustawienia systemu iOS**:

 [![](introduction-to-xamarin-ios-for-visual-studio-images/xma-ios-options.png "Opcja systemu iOS")](introduction-to-xamarin-ios-for-visual-studio-images/xma-ios-options.png#lightbox)

Hosta kompilacji Mac można zmienić, klikając **znaleźć Xamarin Mac Agent** przycisku. Następujący ekran jest wyświetlany można zaktualizować hosta kompilacji Mac:

  [![](introduction-to-xamarin-ios-for-visual-studio-images/xma-dialog.png "Xamarin Mac Agent w oknie dialogowym")](introduction-to-xamarin-ios-for-visual-studio-images/xma-dialog.png#lightbox)


## <a name="visual-studio-toolbar-overview"></a>Omówienie narzędzi Visual Studio

Xamarin iOS dla programu Visual Studio dodaje elementy na standardowym pasku narzędzi i nowy pasek narzędzi dla systemu iOS.
Poniżej opisano funkcje te pasków narzędzi.



### <a name="standard-toolbar"></a>Standardowym pasku narzędzi

W czerwonym kółku są istotne dla systemu Xamarin iOS Programowanie formantów:

 [![](introduction-to-xamarin-ios-for-visual-studio-images/03.png "W czerwonym kółku są istotne dla systemu Xamarin iOS Programowanie formantów")](introduction-to-xamarin-ios-for-visual-studio-images/03.png#lightbox "w czerwonym kółku są istotne dla systemu Xamarin iOS Programowanie formantów")

-  **Uruchom** — uruchamia debugowania i uruchamiania aplikacji na wybranej platformie. Musi być połączony Mac (zobacz wskaźnik stanu na pasku narzędzi z systemem iOS).
-  **Konfiguracje rozwiązania** — pozwala wybrać konfigurację, aby użyć (np. debugowania, wersji).
-  **Platformy rozwiązania** — służy do wybierania iPhone lub iPhoneSimulator dla wdrożenia.


### <a name="ios-toolbar"></a>iOS paska narzędzi

Pasek narzędzi programu Visual Studio dla systemu iOS wygląda podobnie w każdej wersji programu Visual Studio. Te są podane poniżej:

[![](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png "iOS paska narzędzi")](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png#lightbox)

Każdy element znajduje się poniżej:

-  **Menedżer agenta/połączeń Mac** — okno dialogowe Xamarin Mac Agent. Pojawi się ikona *pomarańczowy* podczas nawiązywania połączenia, a *zielony* podczas połączenia.
-  **Pokaż symulatora systemu iOS** — powoduje wyświetlenie okna symulatora systemu iOS do przodu na komputerach Mac.
-  **Pokaż plik IPA na serwerze kompilacji** — plik wyjściowy otworzy wyszukiwania dla komputerów Mac do lokalizacji pliku IPA aplikacji.



## <a name="ios-output-options"></a>Opcje wyjściowe systemu iOS


### <a name="output-window"></a>Okno wyniku

Dostępne są opcje w *dane wyjściowe* okienko, w którym można wyświetlić, aby wykryć kompilacji, wdrożenia i połączenia wiadomości i błędów.

Poniższy zrzut ekranu przedstawia dostępne dane wyjściowe systemu windows, które mogą się różnić w zależności od danego typu projektu:

[![](introduction-to-xamarin-ios-for-visual-studio-images/output-sml.png "Dostępne dane wyjściowe systemu windows")](introduction-to-xamarin-ios-for-visual-studio-images/output-large.png#lightbox)

- **Xamarin** — zawiera informacje dotyczące wyłącznie Xamarin, takich jak połączenie stan Mac i aktywacji.

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output3-sml.png "Informacje dotyczące wyłącznie Xamarin, takich jak połączenie stan Mac i aktywacji")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

- **Diagnostyka Xamarin** — ta operacja wyświetla szczegółowe informacje o projekcie Xamarin, takich jak interakcji z i dla systemu Android.

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output4-sml.png "Szczegółowe informacje o projekcie Xamarin")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

Inne domyślnego okienka programu Visual Studio danych wyjściowych debugowania i kompilacji są nadal dostępne w widoku danych wyjściowych i ich używać do debugowania dane wyjściowe i dane wyjściowe programu MSBuild:

-  **Debugowania**

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output2-sml.png "Dane wyjściowe debugowania")](introduction-to-xamarin-ios-for-visual-studio-images/output2-large.png#lightbox)

- **Tworzenie** & **kolejność kompilowania**

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output1-sml.png "Dane wyjściowe programu MSBuild")](introduction-to-xamarin-ios-for-visual-studio-images/output1-large.png#lightbox)


## <a name="ios-project-properties"></a>Właściwości projektu systemu iOS

Właściwości projektu programu Visual Studio, mogą uzyskiwać przez kliknięcie prawym przyciskiem myszy nazwę projektu i wybierając *właściwości* w menu kontekstowym. Pozwoli to skonfigurować aplikację systemu iOS, jak pokazano na poniższym zrzucie ekranu:


 ![](introduction-to-xamarin-ios-for-visual-studio-images/iosproperties.png "Konfigurowanie aplikacji systemu iOS")

-  *iOS podpisywania pakietu* — nawiązuje połączenie z komputerem Mac, aby wypełnić tożsamości podpisywania kodu i profile aprowizacji:


 ![](introduction-to-xamarin-ios-for-visual-studio-images/bundlesigning.png "Wypełnij kod podpisywania tożsamości i profile aprowizacji")

-  *iOS opcje IPA* — plik IPA zostanie zapisany w systemie plików Mac:


 ![](introduction-to-xamarin-ios-for-visual-studio-images/ipaoptions.png "Opcje IPA systemu iOS")

-  *Opcje uruchamiania systemu iOS* — skonfiguruj dodatkowe parametry:

 ![](introduction-to-xamarin-ios-for-visual-studio-images/iosrunoptions.png "Opcje uruchamiania systemu iOS")



## <a name="creating-a-new-project-for-ios-applications"></a>Tworzenie nowego projektu dla aplikacji systemu iOS

Tworzenie nowego projektu systemu iOS z poziomu programu Visual Studio jest wykonywane podobnie jak inne typu projektu. Wybieranie **Plik > Nowy projekt** spowoduje otwarcie okna dialogowego pokazano poniżej, pokazujący niektóre szablony dostępne do tworzenia nowego projektu systemu iOS:


![](introduction-to-xamarin-ios-for-visual-studio-images/newproject.png "Tworzenie nowego projektu")

Pliki scenorysu i .xib można edytować w programie Visual Studio przy użyciu projektanta dla systemu iOS. Aby utworzyć scenorysu, wybierz jeden z szablonów scenorysu. Spowoduje to wygenerowanie **Main.storyboard** w pliku **Eksploratora rozwiązań** jak pokazano na poniższym zrzucie ekranu:

![](introduction-to-xamarin-ios-for-visual-studio-images/solution-explorer-new.png "Plik Main.storyboard w Eksploratorze rozwiązań")

Aby rozpocząć tworzenia lub edytowania Twojej scenorysu, kliknij dwukrotnie `Main.storyboard` aby otworzyć go w Projektancie z systemem iOS:

![](introduction-to-xamarin-ios-for-visual-studio-images/iosdesigner.png "Main.storyboard w Projektancie systemu iOS")

Aby dodać obiekty do widoku, należy użyć **przybornika** okienko, aby przeciąganie i upuszczanie elementów na Twoje powierzchnię projektu. Można dodać przybornika wybierając **Widok > przybornika**, jeśli nie został jeszcze dodany. Właściwości obiektu można modyfikować, ich układów dostosowana, i zdarzenia można utworzyć za pomocą **właściwości** okienka, jak przedstawiono poniżej:

![](introduction-to-xamarin-ios-for-visual-studio-images/properties.png "W okienku właściwości")

 Aby uzyskać więcej informacji na przy użyciu narzędzia Projektant z systemem iOS, zapoznaj się [projektanta](~/ios/user-interface/designer/index.md) przewodników.


## <a name="running--debugging-ios-applications"></a>Debugowanie aplikacji systemu iOS & uruchomiony

### <a name="device-logging"></a>Rejestrowanie urządzeń

W programie Visual Studio 2015 i nowszych systemów Android i iOS są unified tablety dziennika

Nowe okno narzędzi urządzenia dziennika dla programu Visual Studio umożliwia pokazanie dzienniki dla urządzeń z systemami Android i iOS. Mogą być wyświetlane, wykonując jedną z następujących poleceń:

- **Widok > innych okien > dziennika urządzenia**
- **Narzędzia > iOS > dziennika urządzenia**
- **pasek narzędzi dla systemu iOS > dziennika urządzenia**

Gdy okno narzędzia jest wyświetlana, użytkownik może wybrać urządzenie fizyczne z listy rozwijanej urządzeń. Po wybraniu urządzenia dzienniki zostanie automatycznie dodany do tabeli. Przełączanie między urządzeniami będzie zatrzymać i uruchomić rejestrowanie urządzenia.

Aby urządzeń, które mają być widoczne w polu kombi można załadować projektu iOS. Ponadto dla systemu iOS, Visual Studio musi być [połączone z serwerem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) w celu odnalezienia urządzeń z systemem iOS, połączony z komputerów Mac.

Udostępnia tego okna narzędzia: Tabela wpisów dziennika, listy rozwijanej w celu zaznaczenia urządzenia, sposób Wyczyść wpisy dziennika, pole wyszukiwania i Odtwórz/stop/Wstrzymaj przycisków.


### <a name="set-debugging-stops"></a>Ustaw zatrzymuje profilowanie

W dowolnym momencie w aplikacji, która sygnalizuje do debugera, aby tymczasowo zatrzymać wykonywanie programu można ustawić punktów przerwania. Aby ustawić punkt przerwania w Visual Studio, kliknij obszar marginesu edytora obok numer wiersza kodu, które mają być dzielone w:

![](introduction-to-xamarin-ios-for-visual-studio-images/image18.png "Ustawienia punktu debugowania")

Rozpocznij debugowanie i użyj symulator lub urządzenie, aby przejść przez aplikację punktu przerwania. Gdy punkt przerwania zostaje trafiony, zostanie wyróżniony wiersz i normalne zachowanie debugowania programu Visual Studio zostanie włączona: Wkrocz, lub z kodu, Sprawdź zmienne lokalne lub oknie bezpośrednim.

Ten zrzut ekranu przedstawia uruchamiania symulatora systemu iOS obok programu Visual Studio przy użyciu rozwiązań równoległych na OS X:


![](introduction-to-xamarin-ios-for-visual-studio-images/image19.png "Ten zrzut ekranu przedstawia uruchamiania symulatora systemu iOS obok programu Visual Studio na OS X przy użyciu rozwiązań równoległych")

### <a name="examine-local-variables"></a>Sprawdź zmienne lokalne

![](introduction-to-xamarin-ios-for-visual-studio-images/image20.png "Badanie zmienne lokalne z debugowaniem")

## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób użycia systemu Xamarin iOS dla programu Visual Studio. Różne funkcje dostępne do tworzenia, kompilowania i testowania aplikacji systemu iOS z poziomu programu Visual Studio na liście, a udał za pośrednictwem kompilowanie i debugowanie aplikacji iOS proste.

## <a name="related-links"></a>Linki pokrewne

- [Xamarin.iOS Installation](~/ios/get-started/installation/windows/index.md)
- [Inicjowanie obsługi administracyjnej urządzeń](~/ios/get-started/installation/device-provisioning/index.md)
- [Tworzenie interfejsu użytkownika systemu iOS w kodzie](~/ios/app-fundamentals/ios-code-only.md)
- [Łączenie Mac do środowiska Visual Studio z XMA (klip wideo)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
