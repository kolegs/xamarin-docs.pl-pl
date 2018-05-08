---
title: Uruchamianie ekrany dla aplikacji platformy Xamarin.iOS
description: W tym artykule opisano sposób tworzenia aplikacji ekranu uruchamiania w przypadku urządzeń z systemem iOS o dowolnej rozdzielczość i orientacji, za pomocą pojedynczego scenorysu Unified.
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2018
ms.openlocfilehash: d5a267bfa8655a9b9c6d4dba9d8cf9d16624ba9b
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="launch-screens"></a>Uruchamianie ekranów

_W tym artykule opisano sposób tworzenia aplikacji ekranu uruchamiania w przypadku urządzeń z systemem iOS o dowolnej rozdzielczość i orientacji, za pomocą pojedynczego scenorysu Unified._

Przed iOS 8 Tworzenie ekranu uruchamiania dla aplikacji systemu iOS wymagane deweloperowi Podaj zasób obrazu dla każdego z różnych rozmiarach urządzenia i rozwiązania, w których można uruchomić aplikacji. Od czasu wydania systemu iOS 8 jednak jest możliwe do pojedynczego scenorysu Unified umożliwia tworzenie ekranu uruchamiania, który wygląda poprawnie we wszystkich przypadkach.

Ten krótki przewodnik opisuje sposób tworzenia ekranu uruchamiania, scenorysu domyślne w nowym projekcie lub scenorysu, dodać ręcznie do istniejącego projektu. Go następnie pokazuje, jak dodać widok obrazu i etykietę do scenorysu, można ustawić ograniczenia dotyczące widoków i upewnić się, że scenorysu poprawne dla różnych urządzeń i orientacji za pomocą projektanta dla systemu iOS.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>Zarządzanie uruchamiania ekrany z Scenorys

W systemie iOS 8 (lub nowszy) dewelopera można tworzyć specjalne scenorysu Unified zapewnienie ekranu uruchamiania zamiast co najmniej jeden obraz statyczny uruchamiania. Podczas tworzenia uruchomienia scenorysu w systemie iOS projektanta, rozmiar klasy i układu automatycznego można użyć do zdefiniowania różnych układów ekranu różnych środowiskach. Przy użyciu klasy wielkości i układ automatyczny, deweloper można utworzyć osłonę pojedynczego uruchomienia, która wygląda dobrze na wszystkich urządzeniach i wyświetlić środowisk.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. W programie Visual Studio dla komputerów Mac, Utwórz nowy projekt, wybierając **Plik > nowe rozwiązanie** , a następnie wybierając **jednej aplikacji widoku**: 

    ![Okno nowy projekt z jednej aplikacji widoku wybrane](launch-screens-images/launch01.png)

    - Domyślnie nowy projekt zawiera **LaunchScreen.storyboard** pliku, który definiuje interfejs ekranu uruchamiania. 
    - Zamiast tego dodać scenorysu ekranu uruchamiania do istniejącego projektu, kliknij prawym przyciskiem myszy nazwę projektu w **konsoli rozwiązania** i wybierz polecenie **Dodaj > Nowy plik...**  , a następnie wybierz **uruchomić ekranu**:

    ![W oknie nowy plik, z systemem iOS ekranu Uruchom wybrane](launch-screens-images/launch01b.png)

    - Nadaj nazwę plikowi **LaunchScreen** lub inną nazwę.

2. Konfigurowanie projektu do scenorysu odpowiednią dla jego uruchomienie ekranu:

    - Kliknij dwukrotnie **Info.plist** w pliku **konsoli rozwiązania** go otworzyć do edycji.
    - W **uruchomić obrazów** sekcji, upewnij się, że **uruchomić ekranu** jest ustawiona na nazwę scenorysu odpowiednie:

    ![Selektor uruchomić ekranu w pliku Info.plist](launch-screens-images/launch02.png)

    - Domyślnie nowy projekt jest skonfigurowany do używania **LaunchScreen.storyboard** jako ekran jego uruchomienia.

3. Dodawanie obrazu do **Assets.xcassets** zasobów katalogu, aby była ona dostępna do użycia na ekranie uruchamiania. Aby uzyskać więcej informacji, zobacz [Dodawanie obrazów do zasobu katalogu obrazu ustawić](~/ios/app-fundamentals/images-icons/displaying-an-image.md) sekcji [wyświetlania obrazu](~/ios/app-fundamentals/images-icons/displaying-an-image.md) przewodnik.

4. Otwórz **LaunchScreen.storyboard** do edycji przez dwukrotne kliknięcie w **konsoli rozwiązania**.

5. Wybierz urządzenie i orientacji, na których chcesz wyświetlić podgląd scenorysu ekranu uruchamiania w systemie iOS projektanta. Otwórz panel wyboru urządzeń na dolnym pasku narzędzi i wybierz **iPhone 4S** i **pionowa**.

    ![Na pasku narzędzi wybór urządzenia](launch-screens-images/launch05.png)

    - Należy pamiętać, że wybór urządzenia i orientację tylko zmienia sposób iOS projektanta Wyświetla podgląd projektu. Niezależnie od opcji wybranej w tym miejscu, nowo dodanych ograniczenia są stosowane we wszystkich urządzeń i orientacji, chyba że **Edytuj cech** przycisk został użyty do określenia, w przeciwnym razie wartość. 

6. Ustaw **tła** kolor Widok główny kontroler widoku. Wybierz widok, klikając w środku kontroler widoku i dostosować przy użyciu kolor tła **konsoli właściwości**:

    ![Kolor tła purpurowa pojedynczego widoku](launch-screens-images/launch06.png)

7. Dodaj **widoku obrazu** do ekranu uruchomienia i ustawić jej źródła **obrazu**:

    - Przeciągnij **widoku obrazu** z **konsoli przybornika** do środka widoku.
    - Z **widoku obrazu** wybrane w **elementu Widget** sekcji **konsoli właściwości** ustawić **obrazu** obrazu zdefiniowane dla właściwości dodane do **Assets.xcassets** katalogu zasobów. Zmienia położenie i rozmiar **widoku obrazu** zgodnie z wymaganiami:
    
    ![Wyświetl obraz z zestawem właściwości obrazu](launch-screens-images/launch07.png)

8. Dodaj **etykiety** poniżej **widoku obrazu** i użyj **konsoli właściwości** można ustawić jego atrybuty: 

    ![Etykiety z zestawem kolor tekstu i](launch-screens-images/launch08.png)

9. Przełącz do trybu edycji ograniczenia za pomocą przycisku po prawej stronie w **narzędzi ograniczenia**:
    
    ![Przycisk trybu edycji ograniczenia](launch-screens-images/launch09.png)

10. Dodawanie ograniczeń do **widoku obrazu**, ustawienie jej wysokości i szerokości i przygotuj poziomie i w pionie:

    ![Wyświetl obraz z ograniczeniami układu](launch-screens-images/launch10.png)

    - Aby uzyskać więcej informacji na temat dodawania ograniczenia, zobacz [automatycznego układu projektanta Xamarin dla systemu iOS](~/ios/user-interface/designer/designer-auto-layout.md).

11. Dodawanie ograniczeń do **etykiety**, przygotuj poziomie nadanie mu wysokości i szerokości i umieszczając go ustalonego pionowo odległości od **widoku obrazu**:

    ![Etykiety z ograniczeniami układu](launch-screens-images/launch11.png)

12. Przetestuj innymi urządzeniami i orientacje, aby sprawdzić, czy projekt wygląda zgodnie z założeniami we wszystkich scenariuszach. W przypadkach, w którym należy wykonać dla określonego urządzenia lub orientacji dostosowań, użycie **Edytuj cech** przycisk, aby dodać ograniczeniami dla klas określony rozmiar:

    ![Na ekranie uruchamiania renderowane jako iPhone X przy użyciu orientacji poziomej](launch-screens-images/launch12.png)

13. Zapisz zmiany do scenorysu. Uruchom aplikację na urządzeniach lub symulatorze, i uruchomić ekranu będzie widoczny jako aplikacja jest uruchamiana.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Utwórz nowy projekt. W programie Visual Studio, wybierz **Plik > Nowy > Projekt > Visual C# > iPhone & iPad > iOS App (Xamarin)**:

    ![Okno nowy projekt, z systemem iOS App (Xamarin) wybrane](launch-screens-images/launch01.w157.png)

    Wybierz **jednej aplikacji widoku** szablonu, a następnie kliknij przycisk **OK**:

    ![Jeden szablon widoku aplikacji](launch-screens-images/launch01-2.w157.png)

2. Jeśli **zasobów > LaunchScreen.xib** istnieje w **Eksploratora rozwiązań**, usuń go prawym przyciskiem myszy plik i wybierając pozycję **usunąć**. Ten plik zostanie zastąpiony scenorysu w następnym kroku.

3. Utwórz scenorysu do użycia jako ekranu uruchamiania. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj > Nowy element...**  następuje **pusty scenorysu**. Nazwa tego scenorysu **LaunchScreen.storyboard** i kliknij przycisk **Dodaj**:

    ![Okno Dodawanie nowego elementu z pustą scenorysu wybrane](launch-screens-images/launch03.w157.png)

4. Konfigurowanie projektu do użycia **LaunchScreen.storyboard** jako jego scenorysu ekranu uruchamiania:

    - Kliknij dwukrotnie **Info.plist** w pliku **Eksploratora rozwiązań** go otworzyć do edycji. 
    - Na **zasoby Visual** ustaw **uruchomić ekranu** do **LaunchScreen**.

    ![Selektor uruchomić ekranu w pliku Info.plist](launch-screens-images/launch04-vs.png)

5. Dodawanie obrazu do katalogu zasobów w projekcie, aby była ona dostępna do użycia na ekranie Uruchom:

    - W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **katalogi zasobów** i wybierz **Dodaj katalog zasobów**. Nazwa tego nowego katalogu zasobów **zasoby**:

    ![Okno Dodawanie nowego elementu z katalogu zasobów wybrane](launch-screens-images/launch05.w157.png)

    - Dodaj nowy zestaw obrazu do **zasoby** katalogu zasobów, zgodnie z opisem w [Dodawanie obrazów do zasobu katalogu obrazu ustawić](~/ios/app-fundamentals/images-icons/displaying-an-image.md) sekcji [wyświetlania obrazu](~/ios/app-fundamentals/images-icons/displaying-an-image.md) przewodnik.

6. Otwórz **LaunchScreen.storyboard** do edycji przez dwukrotne kliknięcie w **Eksploratora rozwiązań**.

    - Aby edytować plik scenorysu, Visual Studio wymaga aktywnego połączenia z hostem Mac kompilacji. Zobacz [połączenie z komputerem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) przewodnika, aby uzyskać szczegółowe informacje.

7. Wybierz urządzenie i orientacji, na których chcesz wyświetlić podgląd scenorysu ekranu uruchamiania w systemie iOS projektanta. Otwórz panel wyboru urządzeń na dolnym pasku narzędzi i wybierz **iPhone 4S** i **pionowa**: 
 
    ![Na pasku narzędzi wybór urządzenia](launch-screens-images/launch07-vs.png)

    - Należy pamiętać, że wybór urządzenia i orientację tylko zmienia sposób iOS projektanta Wyświetla podgląd projektu. Niezależnie od opcji wybranej w tym miejscu, nowo dodanych ograniczenia są stosowane we wszystkich urządzeń i orientacji, chyba że **Edytuj cech** przycisk został użyty do określenia, w przeciwnym razie wartość. 

8. Dodaj **kontrolera widoku** do scenorysu, przeciągając go od **przybornika** na powierzchnię projektu: 

    ![Pusty kontroler widoku dodany na powierzchnię projektu](launch-screens-images/launch08-vs.png)

9. Ustaw **tła** kolor Widok główny kontroler widoku. Wybierz widok, klikając w środku kontroler widoku i dostosować przy użyciu kolor tła **okna właściwości**:
    
    ![Kolor tła purpurowa pojedynczego widoku](launch-screens-images/launch09-vs.png)

10. Dodaj **widoku obrazu** do ekranu uruchomienia i ustawić jej źródła **obrazu**:

    - Przeciągnij **widoku obrazu** z **przybornika** do środka widoku.
    - Z **widoku obrazu** nadal wybrany w **elementu Widget** sekcji **okna właściwości** ustawić **obrazu** właściwości do ustawienia obrazu już dodany do **zasoby** katalogu zasobów. Zmienia położenie i rozmiar **widoku obrazu** zgodnie z wymaganiami:
    
    ![Wyświetl obraz z zestawem właściwości obrazu](launch-screens-images/launch10-vs.png)

11. Dodaj **etykiety** poniżej **obrazu widoku**:

    - Przeciągnij **etykiety** z **przybornika** na powierzchnię projektu umieszczenia go poniżej **widoku obrazu**.
    - Ustaw atrybuty **etykiety** przy użyciu **okna właściwości**:

    ![Etykiety z zestawem kolor tekstu i](launch-screens-images/launch11-vs.png) 

12. Przełącz do trybu edycji ograniczenia za pomocą przycisku po prawej stronie w **narzędzi ograniczenia**:
    
    ![Przycisk trybu edycji ograniczenia](launch-screens-images/launch12-vs.png) 

13. Dodawanie ograniczeń do **widoku obrazu**, ustawienie jej wysokości i szerokości i przygotuj poziomie i w pionie:

    ![Wyświetl obraz z ograniczeniami układu](launch-screens-images/launch13-vs.png) 

    - Informacje o dodanie ograniczeń, zobacz [automatycznego układu projektanta Xamarin dla systemu iOS](~/ios/user-interface/designer/designer-auto-layout.md).

14. Dodawanie ograniczeń do **etykiety**, przygotuj poziomie nadanie mu wysokości i szerokości i umieszczając go ustalonego pionowo odległości od **widoku obrazu**:
    
    ![Etykiety z ograniczeniami układu](launch-screens-images/launch14-vs.png) 

15. Przetestuj innymi urządzeniami i orientacje, aby sprawdzić, czy projekt wygląda zgodnie z założeniami we wszystkich scenariuszach. W przypadkach, w którym należy wykonać dla określonego urządzenia lub orientacji dostosowań, użycie **Edytuj cech** przycisk, aby dodać ograniczeniami dla klas określony rozmiar:

    ![Na ekranie uruchamiania renderowane jako iPhone X przy użyciu orientacji poziomej](launch-screens-images/launch15-vs.png) 

16. Zapisz zmiany do scenorysu. Uruchom aplikację na urządzeniach lub symulatorze, i uruchomić ekranu będzie widoczny jako aplikacja jest uruchamiana.

-----

> [!NOTE]
> Scenorysu używane jako ekran uruchamianie _musi_ obejmują tylko prostego, wbudowanego elementy interfejsu użytkownika i **nie** nie wszystkie obliczenia ani nie pochodzi od klasy niestandardowej.

Aby uzyskać więcej informacji o tworzeniu ekranu uruchamiania Unified scenorysu, zobacz [dynamiczne ekrany uruchamianie](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) sekcji [Unified Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) przewodnik.

## <a name="migrating-to-launch-screen-storyboards"></a>Migrowanie do uruchomienia Scenorys ekranu

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Kliknij prawym przyciskiem myszy podczas aktualizowania istniejących aplikacji przy użyciu Scenorys dla ekranów jego uruchomienia, **Nazwa projektu** w **Eksploratora rozwiązań** i wybierz **Dodaj**  >  **Nowego pliku...** . Wybierz **iOS** > **uruchomić ekranu** i kliknij przycisk **nowy** przycisk:

![](launch-screens-images/storyboard02.png "Wybierz uruchamianie ekranu systemu iOS")

Następnie kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji. W obszarze **uruchomić ekranu**, wybierz plik scenorysu utworzone powyżej.

![](launch-screens-images/storyboard09.png "Wybierz plik scenorysu utworzone powyżej")


Aby użyć nowego scenorysu jako ekran startowy, wykonaj następujące czynności:

1. Kliknij dwukrotnie `Info.plist` w pliku **Eksploratora rozwiązań** go otworzyć do edycji.
2. Przewiń do **uniwersalnych obrazy uruchamianie** części edytora, otwórz **uruchomić ekranu** listy rozwijanej, a następnie wybierz nazwę scenorysu utworzone powyżej: 

    ![](launch-screens-images/storyboard08.png "Ustawianie ekranu startowego do scenorysu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierz **Dodaj** > **nowego pliku...** : 

    ![](launch-screens-images/image012.png "Dodaj nowy plik")
2. Wprowadź nazwę ekran startowy, a następnie kliknij przycisk **Dodaj** przycisk: 

    ![](launch-screens-images/image013.png "Wprowadź nazwę dla ekranu startowego")
3. W **Eksploratora rozwiązań**, kliknij dwukrotnie plik scenorysu nowo utworzony, aby otworzyć do edycji.
4. Upewnij się, że **klasy wielkości** ustawiono **żadnych: wszystkie** i **widoku jako** jest **ogólnego**: 

    ![](launch-screens-images/image016.png "Upewnij się, że klasa rozmiar jest ustawiona na dowolną: wszelkie i widoku jako jest ogólny")
5. Ekran startowy z klas rozmiar, elementy Interfejsu użytkownika prostego zestawu (takie jak `UIImageView`) i obrazów, które zostały uwzględnione w pakiecie aplikacji: 

    ![](launch-screens-images/image017.png "Zestaw ekranu startowego w systemie iOS projektanta")
6. Zapisz zmiany do scenorysu.

-----

## <a name="related-links"></a>Linki pokrewne

- [Ekrany dynamicznego uruchamiania (przykład)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Ujednolicone scenorysy](~/ios/user-interface/storyboards/unified-storyboards.md)
- [Podstawy narzędzia iOS Designer](~/ios/user-interface/designer/index.md)
- [Ustawianie obrazów dodanie do obrazu w katalogu zasobów](~/ios/app-fundamentals/images-icons/displaying-an-image.md#asset-catalogs)
- [Automatycznie Rozmieść przy użyciu projektanta Xamarin dla systemu iOS](~/ios/user-interface/designer/designer-auto-layout.md)
- [Human Interface Guidelines: Ekran startowy](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
