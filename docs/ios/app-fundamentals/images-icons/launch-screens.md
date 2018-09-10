---
title: Ekrany uruchamiania dla aplikacji platformy Xamarin.iOS
description: W tym artykule opisano sposób tworzenia aplikacji ekranu uruchamiania dla wszystkich urządzeń z systemem iOS, o dowolnej rozdzielczości i orientację, za pomocą pojedynczego ujednoliconego scenorysu.
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2018
ms.openlocfilehash: 40b8c38e89e96223bbf657ff06356d9fb2e9d9b3
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780598"
---
# <a name="launch-screens-for-xamarinios-apps"></a>Ekrany uruchamiania dla aplikacji platformy Xamarin.iOS

_W tym artykule opisano sposób tworzenia aplikacji ekranu uruchamiania dla wszystkich urządzeń z systemem iOS, o dowolnej rozdzielczości i orientację, za pomocą pojedynczego ujednoliconego scenorysu._

Przed system iOS 8 Tworzenie ekranu uruchamiania dla aplikacji systemu iOS wymagany deweloperowi, podaj zasób obrazu dla każdego z różnych czynników formularzy urządzenia i rozwiązania, w których można uruchomić aplikacji. Od czasu wydania systemu IOS jest 8 jednak było można użyć pojedynczego ujednoliconego scenorysu, aby utworzyć ekran uruchamiania, który wydaje się prawidłowe we wszystkich przypadkach.

Ten krótki przewodnik opisuje sposób tworzenia ekranu uruchamiania, za pomocą scenorysu zapewniany domyślnie w nowy projekt lub za pomocą scenorysu dodane ręcznie do istniejącego projektu. Pokazuje sposób użycia narzędzia iOS Designer, aby dodać wyświetlanie obrazów i etykietę do scenorysu, aby ustawić ograniczenia dotyczące widoków i upewnić się, że scenorysu wydaje się prawidłowe dla różnych urządzeń i orientacje.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>Zarządzanie ekrany uruchamiania, za pomocą scenorysów

W systemie iOS 8 (lub nowszy) Deweloper można utworzyć specjalny scenorysu Unified zapewnienie ekranu uruchamiania, zamiast korzystać z jednego lub więcej obrazów statycznych uruchamiania. Podczas tworzenia uruchomienia scenorysu w narzędziu iOS Designer, należy użyć klas rozmiaru i automatycznego układu do definiowania różnych układów środowiska do innego ekranu. Za pomocą klasy rozmiaru i automatycznego układu, deweloper można utworzyć ekran uruchamiania jednego, które wyglądają dobrze na wszystkich urządzeniach i wyświetlić środowisk.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. W programie Visual Studio dla komputerów Mac, należy utworzyć nowy projekt, wybierając **Plik > nowe rozwiązanie** , a następnie wybierając **aplikacja pojedynczego widoku**: 

    ![Okno nowy projekt, za pomocą wybrana aplikacja pojedynczego widoku](launch-screens-images/launch01.png)

    - Domyślnie nowy projekt zawiera **LaunchScreen.storyboard** pliku, który definiuje interfejsu ekranu uruchamiania. 
    - Aby zamiast tego dodać scenorysu ekranu uruchamiania do istniejącego projektu, kliknij prawym przyciskiem myszy nazwę projektu w **konsoli rozwiązania** i wybierz polecenie **Dodaj > Nowy plik...**  , a następnie wybierz **ekranu uruchamiania**:

    ![Okno nowego pliku, z systemem iOS wybrany ekran uruchamiania](launch-screens-images/launch01b.png)

    - Nadaj plikowi nazwę **LaunchScreen** lub inną nazwę.

2. Konfigurowanie projektu na potrzeby odpowiedni Storyboard ekran uruchamiania:

    - Kliknij dwukrotnie **Info.plist** w pliku **konsoli rozwiązania** aby go otworzyć do edycji.
    - W **obrazy uruchomieniowe** sekcji, upewnij się, że **ekranu uruchamiania** jest ustawiona na nazwę odpowiedniej scenorysu:

    ![Selektor ekranu uruchamiania w pliku Info.plist](launch-screens-images/launch02.png)

    - Domyślnie nowy projekt jest skonfigurowany do używania **LaunchScreen.storyboard** jako ekran jego uruchomienia.

3. Dodawanie obrazu do **Assets.xcassets** zasobów katalogu, aby była dostępna do użycia na ekranie uruchamiania. Aby uzyskać więcej informacji, zobacz [Dodawanie obrazów do zasobu katalogu obrazu zestawie](~/ios/app-fundamentals/images-icons/displaying-an-image.md) części [wyświetlanie obrazów](~/ios/app-fundamentals/images-icons/displaying-an-image.md) przewodnik.

4. Otwórz **LaunchScreen.storyboard** do edycji przez dwukrotne kliknięcie go w **konsoli rozwiązania**.

5. Wybierz urządzenie i orientację, na którym ma zostać scenorysu ekranu uruchamiania w narzędziu iOS Designer w wersji zapoznawczej. Otwórz panel wyboru urządzenia na dolny pasek narzędzi i wybierz **iPhone 4S** i **pionowa**.

    ![Na pasku narzędzi wybór urządzenia](launch-screens-images/launch05.png)

    - Należy pamiętać, wybierając urządzenie i orientację zmienia tylko, jak narzędzia iOS Designer podglądy projektu. Niezależnie od tego wyboru wprowadzone w tym miejscu, nowo dodane ograniczenia są stosowane na wszystkich urządzeniach oraz orientacje, chyba że **Edytuj cechy** przycisk został użyty do określono inaczej. 

6. Ustaw **tła** kolor Widok główny kontroler widoku. Wybierz widok, klikając przycisk na środku kontrolera widoku, a następnie dostosować przy użyciu koloru tła **konsoli właściwości**:

    ![Pojedynczy widok kolorem tła purpurowy](launch-screens-images/launch06.png)

7. Dodaj **widoku obrazu** do ekranu uruchamiania i ustaw jego źródło **obraz**:

    - Przeciągnij **widoku obrazu** z **konsoli do przybornika** do środka widoku.
    - Za pomocą **widoku obrazu** wybrane w **widżet** części **konsoli właściwości** ustaw **obraz** właściwości obrazu już skonfigurowane dodane do **Assets.xcassets** katalog zasobów. Zmienia położenie i rozmiar **widoku obrazu** zgodnie z wymaganiami:
    
    ![Widok obrazu z jej zestaw właściwości obrazu](launch-screens-images/launch07.png)

8. Dodaj **etykiety** poniżej **widoku obrazu** i użyj **konsoli właściwości** można ustawić jego atrybuty: 

    ![Etykieta z zestawem jego tekstu i kolorów](launch-screens-images/launch08.png)

9. Przełącz do trybu edycji ograniczenia przy użyciu przycisku po prawej stronie w **pasek narzędzi ograniczeń**:
    
    ![Przycisk tryb edytowania ograniczenia](launch-screens-images/launch09.png)

10. Dodawanie ograniczenia do **widoku obrazu**, ustawiając jego wysokości i szerokości i jego wyśrodkowania w poziomie i w pionie:

    ![Wyświetl obraz z ograniczeniami układu](launch-screens-images/launch10.png)

    - Aby uzyskać szczegółowe informacje na temat sposobu dodawania ograniczeń, zobacz [Autoukład za pomocą projektanta platformy Xamarin dla systemu iOS](~/ios/user-interface/designer/designer-auto-layout.md).

11. Dodawanie ograniczenia do **etykiety**, przygotuj poziomo, nadając mu wysokości i szerokości i pozycjonowanie na stałym pionowo odległości od **widoku obrazu**:

    ![Etykieta z ograniczenia układu](launch-screens-images/launch11.png)

12. Przetestuj inne urządzenia i orientacje, aby sprawdzić, czy projekt wygląda zgodnie z oczekiwaniami we wszystkich scenariuszach. W przypadkach, w którym korekt należy podjąć, dla określonego urządzenia lub orientację, użyj **Edytuj cechy** przycisk, aby dodać ograniczeniami dla określonego rozmiaru klas:

    ![Na ekranie uruchamiania renderowane jako dla telefonu iPhone X przy użyciu orientacji poziomej](launch-screens-images/launch12.png)

13. Zapisz zmiany do scenorysu. Uruchom aplikację w symulatorze lub na urządzeniu i na ekranie uruchamiania będzie widoczny jako uruchamia aplikację.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Utwórz nowy projekt. W programie Visual Studio, wybierz **Plik > Nowy > Projekt > Visual C# > iPhone & iPad > aplikacji (Xamarin) dla systemu iOS**:

    ![Okno nowy projekt, z systemem iOS (Xamarin) aplikacji wybrane](launch-screens-images/launch01.w157.png)

    Wybierz **aplikacja pojedynczego widoku** szablonu, a następnie kliknij **OK**:

    ![Pojedynczy szablon widoku aplikacji](launch-screens-images/launch01-2.w157.png)

2. Jeśli **zasobów > LaunchScreen.xib** istnieje w **Eksploratora rozwiązań**, usuń go, klikając prawym przyciskiem myszy plik i wybierając pozycję **Usuń**. Ten plik zostanie zastąpione scenorysu w następnym kroku.

3. Utwórz Scenorys do użycia jako ekran uruchamiania. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nad projektem i wybierz **Dodaj > Nowy element...**  następuje **pusty Scenorys**. Nazwa tego scenorysu **LaunchScreen.storyboard** i kliknij przycisk **Dodaj**:

    ![Okno Dodaj nowy element, przy pusty Scenorys wybrane](launch-screens-images/launch03.w157.png)

4. Konfigurowanie projektu do użycia **LaunchScreen.storyboard** jako jego scenorysu ekranu uruchamiania:

    - Kliknij dwukrotnie **Info.plist** w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji. 
    - Na **wizualne elementy zawartości** kartę, należy ustawić **ekranu uruchamiania** do **LaunchScreen**.

    ![Selektor ekranu uruchamiania w pliku Info.plist](launch-screens-images/launch04-vs.png)

5. Dodawanie obrazu do katalogu zasobów w projekcie, aby była dostępna do użycia na ekranie uruchamiania:

    - W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **wykazy zasobów** i wybierz **Dodaj wykaz zasobów**. Nazwa tego nowy katalog zasobów **zasoby**:

    ![Okno Dodaj nowy element, z katalogu zasobów wybrane](launch-screens-images/launch05.w157.png)

    - Dodaj nowy zestaw obrazów do **zasoby** katalog zasobów, zgodnie z opisem w [Dodawanie obrazów do zasobu katalogu obrazu zestawie](~/ios/app-fundamentals/images-icons/displaying-an-image.md) części [wyświetlanie obrazów](~/ios/app-fundamentals/images-icons/displaying-an-image.md) przewodnik.

6. Otwórz **LaunchScreen.storyboard** do edycji przez dwukrotne kliknięcie go w **Eksploratora rozwiązań**.

    - Aby edytować plik scenorysu, program Visual Studio wymaga aktywnego połączenia z hostem kompilacji Mac. Zobacz [nawiązywania połączenia z komputerem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) przewodnika, aby uzyskać szczegółowe informacje.

7. Wybierz urządzenie i orientację, na którym ma zostać scenorysu ekranu uruchamiania w narzędziu iOS Designer w wersji zapoznawczej. Otwórz panel wyboru urządzenia na dolny pasek narzędzi i wybierz **iPhone 4S** i **pionowa**: 
 
    ![Na pasku narzędzi wybór urządzenia](launch-screens-images/launch07-vs.png)

    - Należy pamiętać, wybierając urządzenie i orientację zmienia tylko, jak narzędzia iOS Designer podglądy projektu. Niezależnie od tego wyboru wprowadzone w tym miejscu, nowo dodane ograniczenia są stosowane na wszystkich urządzeniach oraz orientacje, chyba że **Edytuj cechy** przycisk został użyty do określono inaczej. 

8. Dodaj **kontrolera widoku** do scenorysu, przeciągając je z **przybornika** na powierzchnię projektu: 

    ![Pusty kontroler widoku dodany do powierzchni projektowej](launch-screens-images/launch08-vs.png)

9. Ustaw **tła** kolor Widok główny kontroler widoku. Wybierz widok, klikając przycisk na środku kontrolera widoku, a następnie dostosować przy użyciu koloru tła **okno właściwości**:
    
    ![Pojedynczy widok kolorem tła purpurowy](launch-screens-images/launch09-vs.png)

10. Dodaj **widoku obrazu** do ekranu uruchamiania i ustaw jego źródło **obraz**:

    - Przeciągnij **widoku obrazu** z **przybornika** do środka widoku.
    - Za pomocą **widoku obrazu** nadal zaznaczone w **widżet** części **okno właściwości** ustaw **obraz** właściwość umożliwiająca ustawienie obrazu już dodany do **zasoby** katalog zasobów. Zmienia położenie i rozmiar **widoku obrazu** zgodnie z wymaganiami:
    
    ![Widok obrazu z jej zestaw właściwości obrazu](launch-screens-images/launch10-vs.png)

11. Dodaj **etykiety** poniżej **obraz widoku**:

    - Przeciągnij **etykiety** z **przybornika** na powierzchnię projektową, umieszczając go poniżej **widoku obrazu**.
    - Ustaw atrybuty dla **etykiety** przy użyciu **okno właściwości**:

    ![Etykieta z zestawem jego tekstu i kolorów](launch-screens-images/launch11-vs.png) 

12. Przełącz do trybu edycji ograniczenia przy użyciu przycisku po prawej stronie w **pasek narzędzi ograniczeń**:
    
    ![Przycisk tryb edytowania ograniczenia](launch-screens-images/launch12-vs.png) 

13. Dodawanie ograniczenia do **widoku obrazu**, ustawiając jego wysokości i szerokości i jego wyśrodkowania w poziomie i w pionie:

    ![Wyświetl obraz z ograniczeniami układu](launch-screens-images/launch13-vs.png) 

    - Aby dowiedzieć się, jak dodawanie ograniczeń, zobacz [Autoukład za pomocą projektanta platformy Xamarin dla systemu iOS](~/ios/user-interface/designer/designer-auto-layout.md).

14. Dodawanie ograniczenia do **etykiety**, przygotuj poziomo, nadając mu wysokości i szerokości i pozycjonowanie na stałym pionowo odległości od **widoku obrazu**:
    
    ![Etykieta z ograniczenia układu](launch-screens-images/launch14-vs.png) 

15. Przetestuj inne urządzenia i orientacje, aby sprawdzić, czy projekt wygląda zgodnie z oczekiwaniami we wszystkich scenariuszach. W przypadkach, w którym korekt należy podjąć, dla określonego urządzenia lub orientację, użyj **Edytuj cechy** przycisk, aby dodać ograniczeniami dla określonego rozmiaru klas:

    ![Na ekranie uruchamiania renderowane jako dla telefonu iPhone X przy użyciu orientacji poziomej](launch-screens-images/launch15-vs.png) 

16. Zapisz zmiany do scenorysu. Uruchom aplikację w symulatorze lub na urządzeniu i na ekranie uruchamiania będzie widoczny jako uruchamia aplikację.

-----

> [!NOTE]
> Scenorysu, używane jako ekran uruchamiania _musi_ uwzględnić tylko prostego, wbudowanego elementy interfejsu użytkownika i **nie** wykonaj wszelkie obliczenia lub pochodzić od klasy niestandardowej.

Aby uzyskać więcej informacji na temat tworzenia ekranu uruchamiania za pomocą scenorysu Unified zobacz [dynamiczne ekrany uruchamiania](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) części [ujednolicone Scenorysy](~/ios/user-interface/storyboards/unified-storyboards.md) przewodnik.

## <a name="migrating-to-launch-screen-storyboards"></a>Migrowanie do uruchomienia scenorysów ekranu

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Podczas aktualizowania istniejącej aplikacji do użytku scenorysów jego ekrany uruchamiania, kliknij prawym przyciskiem myszy **Nazwa projektu** w **Eksploratora rozwiązań** i wybierz **Dodaj**  >  **Nowy plik...** . Wybierz **iOS** > **ekranu uruchamiania** i kliknij przycisk **New** przycisku:

![](launch-screens-images/storyboard02.png "Wybierz ekran uruchamiania systemu iOS")

Następnie kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji. W obszarze **ekranu uruchamiania**, wybierz nowy plik scenorysu utworzonego powyżej.

![](launch-screens-images/storyboard09.png "Wybierz nowy plik scenorysu utworzonego powyżej")


Aby użyć nowego scenorysu jako ekran startowy, wykonaj następujące czynności:

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** aby go otworzyć do edycji.
2. Przewiń do **Universal obrazy uruchomieniowe** części edytora, otwórz **ekranu uruchamiania** listy rozwijanej i wybierz nazwę scenorysu utworzonego powyżej: 

    ![](launch-screens-images/storyboard08.png "Ustawianie ekranu startowego do scenorysu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierz **Dodaj** > **nowy plik...** : 

    ![](launch-screens-images/image012.png "Dodaj nowy plik")
2. Wprowadź nazwę dla ekranu startowego, a następnie kliknij przycisk **Dodaj** przycisku: 

    ![](launch-screens-images/image013.png "Wprowadź nazwę dla ekranu startowego")
3. W **Eksploratora rozwiązań**, kliknij dwukrotnie plik scenorysu nowo utworzony, aby go otworzyć do edycji.
4. Upewnij się, że **klasy rozmiaru** jest ustawiona na **wszelkie: wszelkie** i **Wyświetl jako** jest **ogólny**: 

    ![](launch-screens-images/image016.png "Upewnij się, że klasa rozmiaru jest ustawiona na dowolne: wszelkie i Wyświetl jako jest ogólny")
5. Zestaw ekranu startowego z klas rozmiaru, proste elementy interfejsu użytkownika (takie jak `UIImageView`) i obrazów, które zostały uwzględnione w zbiorze aplikacji: 

    ![](launch-screens-images/image017.png "Zestaw ekranu startowego w narzędziu iOS Designer")
6. Zapisz zmiany do scenorysu.

-----

## <a name="related-links"></a>Linki pokrewne

- [Dynamiczne ekrany uruchamiania (przykład)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Ujednolicone scenorysy](~/ios/user-interface/storyboards/unified-storyboards.md)
- [Podstawy narzędzia iOS Designer](~/ios/user-interface/designer/index.md)
- [Dodawanie obrazów do obrazu wykazu zasobów zestawu](~/ios/app-fundamentals/images-icons/displaying-an-image.md#adding-images-to-an-asset-catalog-image-set)
- [Automatyczne rozmieszczanie za pomocą projektanta platformy Xamarin dla systemu iOS](~/ios/user-interface/designer/designer-auto-layout.md)
- [Human Interface wytyczne: Ekran startowy](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
