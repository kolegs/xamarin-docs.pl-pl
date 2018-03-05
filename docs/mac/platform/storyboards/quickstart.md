---
title: Scenorys Szybki Start
description: "Pobieranie macOS rozpoczęto Tworzenie interfejsów użytkownika za pomocą scenorys."
ms.topic: article
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: 559179d2618ea41bf50362f2e5eb2aa735464b33
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="starting-a-new-storyboard-based-project"></a>Uruchamianie nowego scenorysu na podstawie projektu

Jako szybkie wprowadzenie do definiowania aplikacji Xamarin.Mac interfejsu użytkownika przy użyciu Scenorys Zacznijmy nowy projekt Xamarin.Mac. Wybierz **Mac** > **aplikacji** > **aplikacji Cocoa** i kliknij przycisk **dalej** przycisk:

[ ![](quickstart-images/qs01.png "Dodawanie nowej aplikacji Cocoa")](quickstart-images/qs01.png)

Użyj **Nazwa aplikacji** z `MacStoryboard` i kliknij przycisk **dalej** przycisk:

[ ![](quickstart-images/qs02.png "Ustawienie nazwy aplikacji")](quickstart-images/qs02.png)

Użyj domyślnej **Nazwa projektu** i **Nazwa rozwiązania** i kliknij przycisk **Utwórz** przycisk:

[ ![](quickstart-images/qs03.png "Nazwy projektu i rozwiązania")](quickstart-images/qs03.png)

W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` plik, aby otworzyć do edycji w Konstruktorze interfejsu w środowisku Xcode:

[ ![](quickstart-images/qs04.png "Edytowanie scenorysu w środowisku Xcode")](quickstart-images/qs04.png)

Jak widać powyżej, domyślne scenorysu definiuje zarówno paska Menu aplikacji i jej okno główne z nim kontrolera widoku oraz widoku. Dla nasza Przykładowa aplikacja zamierzamy utworzyć interfejsu użytkownika, który ma główna _widok zawartości_ po jednej stronie i _widoku inspektora_ w ciągu sekundy.

Aby to zrobić, musimy, należy najpierw usunąć domyślną kontrolera widoku oraz widoku, który jest dostarczany z scenorysu przez wybierz go w konstruktora interfejsu i naciskając klawisz **usunąć** klucza:

[ ![](quickstart-images/qs05.png "Usuwanie kontrolera widok domyślny")](quickstart-images/qs05.png)

Następnie wpisz `split` do **filtru** obszaru, wybierz kontroler widoku podziału pionowy i przeciągnij go na _powierzchni projektowej_:

[ ![](quickstart-images/qs06.png "Wyszukiwanie kontrolera widoku podziału")](quickstart-images/qs06.png)

Zwróć uwagę, że kontroler automatycznie uwzględnione podrzędnych dwa kontrolery widoku (i ich widoki pokrewne) przewodowej w górę do lewej i prawej stronie w widoku podziału. Aby powiązać podzielony widok do swojego okna nadrzędnego, naciśnij klawisz **kontroli** klucza, kliknij na kontrolerze okna (niebieskie koło kontrolera okno ramowe) i przeciągnij linię podziału kontroler widoku. Wybierz **zawartości okna** z menu podręcznego:

[ ![](quickstart-images/qs07.png "Ustawienia systemu windows widok zawartości")](quickstart-images/qs07.png)

Spowoduje to powiązanie elementu dwa interfejs ze sobą za pomocą Segue:

[ ![](quickstart-images/qs08.png "Segue między okno i zawartości")](quickstart-images/qs08.png)

Chcemy, aby umieścić widoku tekstu z lewej strony podzielony widok i automatycznie wypełnił obszar dostępne, gdy zmieniany jest rozmiar okna lub podzielony widok. Przeciągnięcie widoku tekstu do górnej kontrolera widoku dołączone do widoku podziału, a następnie kliknij przycisk **numeru Pin** automatycznie układu ograniczenia (drugi ikona z prawej strony, u dołu powierzchnię projektu).

[ ![](quickstart-images/qs09.png "Konfigurowanie ograniczeń")](quickstart-images/qs09.png)

W tym miejscu kliknij wszystkie cztery **dwuteownik** ikon ograniczenia w górnej części Popover ograniczenia a kliknij przycisk **dodać ograniczenia 4** znajdujący się u dołu, aby dodać wymagane ograniczenia.

Jeśli firma Microsoft powrócić do programu Visual Studio dla komputerów Mac i uruchomić projekt, zwróć uwagę, że widoku tekstu zmienia rozmiar automatycznie do wypełnienia z lewej strony widoku złożonego jako okno lub podziału zmieniany jest rozmiar:

[ ![](quickstart-images/qs10.png "Przykładem aplikacji")](quickstart-images/qs10.png)

Ponieważ zamierzamy należy używać po prawej stronie w widoku złożonego jako obszar inspektora, firma Microsoft ma być mają mniejszy rozmiar i umożliwić mu zostać zwinięty. Wróć do Xcode i Edytuj widok dla prawej strony, zaznaczając go na powierzchni projektu i klikając **inspektora rozmiar**. W tym miejscu wprowadź **szerokość** z `250`:

[ ![](quickstart-images/qs11.png "Ustawienie szerokości")](quickstart-images/qs11.png)

Wybierz następny element podziału, który reprezentuje po prawej stronie, ustawić wyższej **zawierający priorytet** i kliknij przycisk **użytkownika można Zwiń** wyboru:

[ ![](quickstart-images/qs12.png "Edytowanie priorytet gospodarstwa")](quickstart-images/qs12.png)

Jeśli zostanie zwrócona do programu Visual Studio dla komputerów Mac i uruchomić projekt teraz, powiadomień po prawej stronie utrzymuje je jest mniejszy rozmiar i okno zmieni się rozmiar:

[ ![](quickstart-images/qs13.png "Przykładem aplikacji")](quickstart-images/qs13.png)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>Definiowanie prezentacji Segue

Zamierzamy układu po prawej stronie widok podzielony na działanie jako Inspektora właściwości zaznaczonego tekstu. Firma Microsoft będzie przeciągnij niektóre kontrolki widok w dolnym okienku układ interfejsu użytkownika inspektora. Ostatni formant chcemy wyświetlić popover, który umożliwia użytkownikowi wybranie z czterech znaków wstępnie zdefiniowane style.

Dodamy przycisk Inspektor i kontrolera widoku na powierzchnię projektu. Zmienimy rozmiar kontroler widoku jako rozmiaru czy chcemy naszych Popover i Dodaj do niej czterech przycisków. Następnie wyślemy wiadomość **kontroli** klucz — kliknij przycisk w widoku inspektora i przeciągnij, aby kontroler widoku reprezentujące naszych popover:

[ ![](quickstart-images/qs14.png "Przeciąganie w celu utworzenia nowego segue")](quickstart-images/qs14.png)

W menu podręcznym wybierzemy **Popover**: 

[ ![](quickstart-images/qs15.png "Wybieranie typu segue")](quickstart-images/qs15.png)

Na koniec będzie możemy wybierz Segue na powierzchni projektu i ustaw **preferowany krawędzi** do **lewej**. Następnie będzie przeciągnij linię z **widoku zakotwiczenia** do przycisku chcemy popover jest dołączony do:

[ ![](quickstart-images/qs16.png "Przeciąganie w celu utworzenia nowego segue")](quickstart-images/qs16.png)

Jeśli zostanie zwrócona do programu Visual Studio dla komputerów Mac, uruchom aplikację i wybierz polecenie **Brak** pojawi się przycisk w inspektora popover:

[ ![](quickstart-images/qs17.png "Przykład segue uruchomiona")](quickstart-images/qs17.png)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>Tworzenie aplikacji preferencji

Większość standardowych macOS aplikacje mają _okna dialogowego preferencji_ umożliwiająca użytkownikowi na definiowanie kilka opcji, które kontrolują różnych aspektów aplikacji, takich jak konta użytkowników i wyglądu.

Aby zdefiniować standardowe okno dialogowe preferencji, przeciągnij kontrolera widoku kartę na powierzchnię projektu:

[ ![](quickstart-images/qs18.png "Edytowanie scenorysu w środowisku Xcode")](quickstart-images/qs18.png)

Ponownie to zostanie automatycznie są dostarczane z dwóch elementów podrzędnych dołączonych kontrolerów widoku. Na przykład sake, dodamy etykietę do każdego widoku, który będzie Centrum wewnątrz niej:

[ ![](quickstart-images/qs19.png "Ustawienie z ograniczeniami")](quickstart-images/qs19.png)

Dalej, chcemy wyświetlić okno preferencji, gdy użytkownik wybierze **Preferencje...**  elementu menu. Na pasku Menu, wybierz element menu Preferencje **kontroli** klucza kliknij i przeciągnij linię do kontrolera widoku karty:

[ ![](quickstart-images/qs20.png "Przeciąganie w celu utworzenia segue")](quickstart-images/qs20.png)

Z menu podręcznego wybierzemy **modalne** do wyświetlenia tego okna jako modalnego okna dialogowego:

[ ![](quickstart-images/qs21.png "Wybieranie typu segue")](quickstart-images/qs21.png)

Czy możemy zapisać naszych zmiany, wróć do programu Visual Studio for Mac, uruchom aplikację i wybierz **Preferencje...**  element menu, naszej nowej preferencje, zostanie wyświetlone okno dialogowe:

[ ![](quickstart-images/qs22.png "Przykład segue uruchomiona")](quickstart-images/qs22.png)

Można zauważyć, że to nie wygląda jak aplikację sieci standardowej macOS okno dialogowe preferencji. Aby rozwiązać ten problem, należy uwzględnić dwa pliki obrazów w aplikacji Xamarin.Mac `Resources` folderu w **Eksploratora rozwiązań** i wrócić do konstruktora interfejsu w środowisku Xcode.

Wybierz kartę kontrolera widoku i przełącznik jego **styl** do **narzędzi**: 

[ ![](quickstart-images/qs23.png "Ustawienie stylu paska kartę")](quickstart-images/qs23.png)

Zaznacz każdą kartę i nadaj mu **etykiety** i wybierz jeden z obrazów do reprezentowania go:

[ ![](quickstart-images/qs24.png "Konfigurowanie każdej karcie w środowisku Xcode")](quickstart-images/qs24.png)

Czy możemy zapisać naszych zmiany, wróć do programu Visual Studio for Mac, uruchom aplikację i wybierz **Preferencje...**  element menu okna dialogowego zostaną wyświetlone takie jak aplikacja macOS standardowe:

[ ![](quickstart-images/qs25.png "Przykład okna Preferencje uruchomione")](quickstart-images/qs25.png)

Aby uzyskać więcej informacji, zobacz nasze [Praca z obrazami](~/mac/app-fundamentals/image.md), [menu](~/mac/user-interface/menu.md), [Windows](~/mac/user-interface/window.md) i [okna](~/mac/user-interface/dialog.md) dokumentacji.

## <a name="related-links"></a>Linki pokrewne

- [MacStoryboard (przykład)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Witaj, Mac](~/mac/get-started/hello-mac.md)
- [Praca z systemu Windows](~/mac/user-interface/window.md)
- [OS X człowieka Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Wprowadzenie do systemu Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
