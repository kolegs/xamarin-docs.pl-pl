---
title: Wprowadzenie do tworzenia gier za pomocą platformy Xamarin
description: Rodzaj tworzenia gier może znacznie się różnić od rozwoju innych typów aplikacji. Ten artykuł zawiera wprowadzenie do tworzenia gier, oferujący funkcje technologii, których można użyć platformy Xamarin.Android i Xamarin.iOS. Platformy Xamarin.Android i Xamarin.iOS zapewnia zarówno wysokiego poziomu omówienie sposobu gry zostały wprowadzone, jak i próbkowania technologie dostępne do użycia.
ms.prod: xamarin
ms.assetid: 0E3CDCD2-FBE4-49F5-A70E-8A7B937BAF1D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: b2df6d431004bbfa140b6cae1d069404af92c1df
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-game-development-with-xamarin"></a>Wprowadzenie do tworzenia gier za pomocą platformy Xamarin

_Rodzaj tworzenia gier może znacznie się różnić od rozwoju innych typów aplikacji. Ten artykuł zawiera wprowadzenie do tworzenia gier, oferujący funkcje technologii, których można użyć platformy Xamarin.Android i Xamarin.iOS. Platformy Xamarin.Android i Xamarin.iOS zapewnia zarówno wysokiego poziomu omówienie sposobu gry zostały wprowadzone, jak i próbkowania technologie dostępne do użycia._

Projektowanie gier może być bardzo atrakcyjnych szczególnie podane, jak łatwo można ją opublikować swoją pracę na platformach mobilnych. W tym artykule omówiono pojęcia i technologii związanych z tworzenia gier, które mogą pomóc tworzyć gry, czy jest utworzyć AAA wysokiej jakości, gier lub tylko do programu do zabawy.

W tym artykule omówiono następujące tematy:

- **Gry w porównaniu z systemem innym niż gry pojęcia dotyczące programowania** — firma Microsoft będzie Eksploruj niektóre pojęcia, które są unikatowe do tworzenia gier, lub są współużytkowane z innymi typami programowanie, ale wymagają wyróżnienia w tym miejscu z powodu ich znaczenie.
- **Zespół deweloperów gier** — w tej sekcji analizuje różnych ról w zespół deweloperów gier.
- **Tworzenie gier pomysł** — w tej sekcji mogą pomóc tworzyć nowe pomysł gier — pierwszy krok w podejmowaniu nową grę.
- **Technologia gier** — w tym miejscu będzie na niektóre z dostępnych technologii i platform, które może poprawić wydajność pracy jako deweloper gier listę.


# <a name="game-vs-non-game-programming-concepts"></a>Vs gier. Koncepcje programowania z systemem innym niż gry

Programiści przeniesienie do tworzenia gier są często muszą nowych pojęć i wzorce programowania. W tej sekcji przedstawiono ogólny widok niektóre z tych pojęć.


## <a name="the-game-loop"></a>Pętla gier

Typowy gry wymaga stałej ruchu lub zmiany sytuacji na ekranie w odpowiedzi do interakcji z użytkownikiem i automatycznego logiki gier. Jest to osiągane przez co to jest zwykle nazywany *gier pętli*. Gry pętla jest pewien typ w instrukcji (na przykład pętli while), w którym jest uruchomiona na bardzo wysokiej częstotliwości, na przykład 30 i 60 pętli *klatek na sekundę*.

Poniżej przedstawiono diagram prostego pętli gier:

![](images/image1.png "Jest to diagram prostego pętli gier")

Technologie, które omówiono poniżej spowoduje abstrakcyjnej optymalizacji rzeczywiste pętli while, ale mimo to Abstrakcja pojęcie aktualizacje co ramki będzie wyświetlany.

Wydajność kodu można wykonać priorytet nawet najprostszym gier. Na przykład: funkcja przyjmująca 10 MS, aby wykonać może mieć znaczący wpływ na wydajność gry — szczególnie w przypadku, gdy jest wywoływana więcej niż raz na klatkę. Jeśli Twoja Gra działa 30 klatek na sekundę następnie oznacza to, że każdej ramce musi być wykonywany w milisekundach w obszarze 33. Z kolei takich funkcji nie może nawet być zauważalne, jeśli wykonywania tylko w odpowiedzi na kliknięcie przycisku w aplikacji innej niż gry.

Popularne typy logiki, które mogą być wykonywane co ramki obejmują:

- **Odczytywanie wejściowych** — gry może być konieczne sprawdzenie, czy użytkownik ma interakcji z gry sprawdzając wprowadzania sprzętu, takiego jak ekran dotykowy, klawiatura, mysz lub kontrolera gier.
- **Przenoszenie** — obiekty, które przenoszenia z jednego miejsca do innego przeniesie zazwyczaj bardzo małych ilość co ramki dać wrażenie płynne ruchu.
- **Kolizji** — wiele gier wymagają częstego testowania czy nakładających się lub przecinające się różnych obiektów. Firma Microsoft będzie obejmować kolizji bardziej szczegółowo w dalszej części w tym artykule. Przepływu i kolizji mogą być obsługiwane przez system symulacji dedykowanych fizycznych.
- **Sprawdzanie warunków specjalistycznego** — stanu gry może być kontrolowane przez określone warunki, takie jak czy odtwarzacz został uzyskany za mało punktów lub czy skończył wyznaczonym czasie.
- **Zachowanie AI** — logika co ramki, która może być używana do sterowania zachowaniem obiektów, które nie są kontrolowane przez odtwarzacz, takich jak patrolling wroga lub przenoszenie przeciwnikowi sterowniki wokół racetrack.
- **Renderowanie** — większość gier aktualizuje informacje wyświetlane na ekranie każdej ramki. Można tego dokonać w odpowiedzi na zmiany, które mają wpływ na gry (takie jak znak Nawigacja po poziomu) lub po prostu zapewnienie visual Polski (na przykład objęte śniegu lub animowany ikony).

Należy pamiętać, że wiele działań wymienionych powyżej zmienić stan całej aplikacji, podczas gdy wiele aplikacji z systemem innym niż gry często zmiany stanu w odpowiedzi na zdarzenia są zgłaszane.


## <a name="content-loading-and-unloading"></a>Zawartości ładowanie i zwalnianie

Zawartość ręcznego ładowania i zwalniania (lub disposing) mogą być wymagane w zależności od technologii używanej do rozwoju. Ręczne ładowanie i zwalnianie zasobów mogą być niezbędne do istnieje wiele możliwych przyczyn:

 - Zasoby może zająć dużo czasu ładowania względem długość jedną ramkę. Niektórych zasobów może trwać nawet sekund obciążenia, które poważnie może zakłócić środowisko po załadowaniu mid gry. Jeśli czas ładowania jest szczególnie długie (na przykład lub dwóch więcej niż 1 sekunda) może zajść potrzeba Pokaż animowany ładowania ekranu paska postępu.
 - Zasoby mogą używać dużej ilości pamięci RAM, wymagających aktywnego zarządzania co to jest ładowany mieścić się w dostarczanych przez gry platformach docelowych.
 - Gry może być konieczne wyświetlić więcej zasobów niż można zmieścić w pamięci RAM. "Otwórz w świecie" gry często zawierają dużych środowiskach, które odtwarzaczy można poruszać się bezproblemowo — to z ekranami nie ładowania. W takim przypadku może być konieczne utworzenie niestandardowego systemu przesyłania strumieniowego zawartości w i zarządzanie użycie pamięci.

Niestandardowych formatów plików może być konieczne przetwarzanie w czasie ładowania wymagające ładowania niestandardowego kodu.


## <a name="math"></a>Math

Wiele gier wymaga bardziej zaawansowanych matematyce niż aplikacje z systemem innym niż gry. Oczywiście poziom matematyczne zależy od stopnia złożoności gry. Ogólnie rzecz biorąc grach 3W wymagają matematyczne więcej niż 2. Na szczęście zawsze rozpoczęcie pracy z prostych gier i uczenie się pracy. Tworzenie gier może być doskonałym sposobem poznawania matematyczne!

Jeśli znasz płaszczyzny kartezjański — korzystających z współrzędne X i Y do obiektów — w odpowiednim miejscu, a następnie znasz wystarczająca liczba na wprowadzenie do tworzenia gier. Poniżej przedstawiono płaszczyźnie kartezjański z dodatnią Y wskazująca w górę:

![](images/image2.png "To wskazuje na płaszczyźnie kartezjański z dodatnią Y wskazująca w górę")

> [!IMPORTANT]
> Niektóre aparaty/API Użyj układ współrzędnych gdzie zwiększenie wartości Y obiektu przeniesie go, podczas gdy inne systemy współrzędnych, w której działa Y dodatnią. Pamiętać to w przypadku przenoszenia między systemami.
Funkcje trygonometryczne (na przykład sinus i cosinus) są często używane w gier 2W, implementujące dowolnej formy obrotu.



Jeśli planowane jest tworzenie gier 3D następnie prawdopodobnie konieczne będzie należy zapoznać się z założenia z algebraiczną liniowy (dla obrotu i ruch w przestrzeni 3D), a także niektóre Calculus (stosowania przyspieszania).


## <a name="content-pipelines"></a>Potoki zawartości

Termin *zawartości potoku* odwołuje się do procesu, który przyjmuje pliku do pobrania z formatem, gdy utworzone (na przykład plik obrazu PNG) do końcowego formatu używanego w grę. Format końcowy zależy od tego, na którym jest używane typu zawartości, oraz które technologii jest używany do prezentowania zawartości.

Niektóre potoki zawartości mogą być bardzo szybko i wymagają nie nakładu pracy. Na przykład większość aparatów gier i interfejsy API mogą ładować format PNG pliku w formacie nieprzetworzonym. Z drugiej strony bardziej skomplikowanych formatów (takich jak modeli 3D) może być konieczne przetworzone przed ładowany w innym formacie, a to przetwarzanie może zająć trochę czasu, w zależności od rozmiaru i złożoność elementu zawartości.


# <a name="game-development-teams"></a>Zespoły deweloperów gier

Tworzenie gier wprowadza nowe role i tytuły dla użytkowników indywidualnych związane z procesem. Większość deweloperów gier nie będą mogli spełniają szeroką gamę umiejętności wymagane do wersji pełnej gier, więc istnieje wiele zasad. Należy pamiętać, że nie jest pełną listę obszarów programowanie — tylko niektóre z tych częściej.

- **Programista** — najczęściej odczytu, w tym artykule mieszczą się w tej kategorii. Rola programisty w opracowywaniu gier jest podobny do roli programisty w aplikacji innej niż gry. Obowiązki obejmują pisanie logiki do sterowania przepływem gry rozwijanie systemów do wykonywania typowych zadań w ramach danego projektu, dodawanie i wyświetlania zawartości i — oczywiście — naprawiania błędów.
- **2D wykonawcy** — artystów 2D są odpowiedzialne za tworzenie *2D zasoby*. Obejmują one pliki obrazów dla gry graficznego interfejsu użytkownika, cząstki środowisk i znaki. Jeśli grę, którą tworzysz 3D, artystów 2D nie można odpowiedzialny za środowisk i znaki. Można znaleźć wolnej grafiki w grę w [ http://opengameart.org/ ](http://opengameart.org/) .
- **3D artystów** — artystów 3D są odpowiedzialne za tworzenie *3D zasoby*. Obejmują one modeli 3D w środowiskach, znaków i właściwości (mebli, giełdowych i innych obiektów inanimate). Niektóre zespoły odróżnić 3D artystów i 3D twórcy animacji, w zależności od wielkości zespołu. Można znaleźć wolnej grafiki 3D w grę w [ http://opengameart.org/ ](http://opengameart.org/) .
- **Projektant gier** — projektantów gier są zobowiązani do definiowania jak gry jest odtwarzany. Może to obejmować wysokiego poziomu decyzji na przykład ustawienie grę, ogólnym celem gry i jak odtwarzacza przechodzi przez gry. Definiowanie współczynniki dla ruchu lub poziom ups i projektowanie układu poziomu projektantów gry może być również objętego bardzo szczegółowe decyzji, takich jak dane wejściowe mapowania akcji. Należy pamiętać, że termin *projektanta* może odwoływać się do gier designer lub wizualnego projektanta w zależności od kontekstu.
- **Dźwięk projektanta** — projektantów dźwięku są odpowiedzialne za zasoby audio gier. Niektóre zespołów może odróżnić osób odpowiedzialnych za tworzenie efekty i Kompozytorzy, mimo niewielkich zespołów może być jedna osoba odpowiedzialna za wszystkie audio.


# <a name="creating-a-game-idea"></a>Tworzenie gier pomysł

Projektowanie gry może pojawić się być łatwo zrobić — po wszystkich jedynym wymaganiem jest "Przechowuj coś przyjemne." Niestety wielu deweloperów znajdują się w niej kiedy nastąpi moment, aby utworzyć pomysł, w którym można uruchomić tworzenia.

Dyscypliny gier projektu nie jest łatwo wyjaśnione i wymaga praktyki zwiększające tak jak w przypadku grafik lub jest programowania w języku, ale w tej sekcji ułatwiają start do ścieżki.

Należy zacząć małych nowych deweloperów. Może być trudne się możliwość przesłania remake dużych, Nowoczesny gry wideo do, ale mniejsze gry może być lepsze środowisko uczenie i szybsze postęp sprawia, że środowisko wykorzystuje więcej.

Wiele gier, zarówno do gier uczenie się, jak również komercyjnych, stanowiące poprawy lub modyfikacji istniejących gier. Jest jednym ze sposobów generowania pomysłów spojrzeć na inne gry dla pomysłów. Na przykład można rozważyć grę, którą chcesz osobiście i spróbuj zidentyfikować jakie cechy o gry wprowadzać proste. Może to być eksploracji, nadzoru nad mechanika gry lub postęp w wątku. Nie zapomnij "światła" gry również wziąć pod uwagę podczas wyszukiwania nowe pomysły.

Innej techniki generowania nowe pomysły jest należy wziąć pod uwagę określonego rodzaju, takie jak układanki, gier strategią lub platformers. Określonego rodzaju znane deweloper może zawierać dobry punkt wyjścia.

Remaking istniejących gry jest również edukacyjnych środowisko, w chociaż może to ograniczyć żywotność komercyjnych gotowego produktu. Proces tworzenia gier, nawet taki, który jest dokładne klonowania zapewnia cenne edukacyjnych.


# <a name="game-development-technology"></a>Technologia tworzenia gier

Deweloperzy przy użyciu platformy Xamarin.Android i Xamarin.iOS ma szeroki zakres technologie umożliwiające ułatwiających tworzenie gier. W tej sekcji zostanie omówiono niektóre najbardziej popularnych rozwiązań i platform.


## <a name="cocossharp"></a>CocosSharp

CocosSharp jest aparat Cocos gier 2W w wersji open source, obsługujący wiele platform. Aparat zapewnia dostęp do systemu Android, iOS, Mac OS X, Windows Desktop, Windows RT i Windows Phone.

CocosSharp koncentruje się na programisty prosty interfejs API do tworzenia gier 2W. Wzrost gier na urządzeniach przenośnych pomogła do reignite popularne 2D tworzenia gier wprowadzania CocosSharp działało technologii zainteresowania i komercyjnych projektów podobne. Jest podana jako źródła kodu lub .dll pliki (które można uzyskać za pośrednictwem pakietu NuGet), ale nie zapewnia to edytor wizualny; w związku z tym interakcji z aparatem CocosSharp wymaga znajomości programowania.

Aby rozpocząć pracę z CocosSharp, zapoznaj się naszego [przewodniki CocosSharp](~/graphics-games/cocossharp/index.md).

Gry złych na otoczenie Ninjas jest tworzony z CocosSharp i może być dobry punkt wyjścia, jeśli szukasz już uruchomiona gry dla wielu platform:

![](images/image3.png "Gry złych na otoczenie Ninjas utworzono za pomocą CocosSharp")

Można go pobrać i uzyskać więcej informacji na [stronie AngryNinjas Github](https://github.com/xamarin/AngryNinjas).


## <a name="monogame"></a>MonoGame

MonoGame jest open source, cross platform wersja interfejsu API XNA firmy Microsoft. MonoGame może służyć do tworzenia gier dla systemu iOS, Android, Mac OS X, Linux, Windows, Windows RT i Windows Phone.

W odróżnieniu od CocosSharp MonoGame jest technicznie nie gier aparatem, ale raczej gier programowanie interfejsu API. Oznacza to, że praca z MonoGame wymaga bezpośrednio zarządzania obiektami gier, ręcznie rysowania obiektów i Implementowanie typowych obiekty, takie jak aparaty fotograficzne i *wykresy sceny* (hierarchii nadrzędny-podrzędny między obiektami gier). Aby zrozumieć różnicę, należy wziąć pod uwagę, że CocosSharp jest oparty na MonoGame. MonoGame uogólnia niektóre specyficzne dla platformy technologii, takich jak grafiki, renderowania i audio, gdy CocosSharp zawiera kod, organizowanie i wykonywania logiki gier.

MonoGame nie oferuje środowiska projektowego visual standardowe, więc pracy z MonoGame wymaga wiedzy programistycznej.

Godne gier za pomocą MonoGame przykłady:

FEZ:

![](images/image7.png "FEZ")

Bastionu:

![](images/image8.jpg "Bastionu")

Aby rozpocząć pracę z MonoGame, przejdź do naszego [przewodniki MonoGame](~/graphics-games/monogame/index.md).


## <a name="urhosharp"></a>UrhoSharp

UrhoSharp jest wieloplatformowych wysokiego poziomu 3D i 2D aparatem, który może służyć do tworzenia animowany sceny 3W i 2D aplikacji przy użyciu mają geometrię, materiałów, świateł i aparatów fotograficznych.

![](images/urhosharp.gif "UrhoSharp jest wieloplatformowych wysokiego poziomu 3D i 2D aparatem, który może służyć do tworzenia animowany sceny 3W i 2D")

Zapoznaj się z [przewodniki UrhoSharp](~/graphics-games/urhosharp/index.md) rozpocząć pracę.

## <a name="additional-technology"></a>Dodatkowe technologii

Technologie wyróżnione powyżej jest tylko przykładem technologie dostępne. Inne technologie zauważalne obejmują:

- **Zestaw Sprite** — program Xamarin obsługuje firmy Apple zestawu Sprite gier struktury, która umożliwia dostęp do wszystkich funkcji natywnego interfejsu API. Zestaw Sprite jest technologią utworzone przez firmę Apple, zapewnia głęboką integrację z resztą ekosystemu systemu iOS. Oczywiście Sprite zestawu nie jest między platformami, nie można użyć w systemie Android. Aby uzyskać więcej informacji na temat używania zestawu Sprite zobacz ten wpis:  [http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/](http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/)
- **Zestaw sceny** — Xamarin także zapewnia obsługę struktury zestawu sceny firmy Apple, która upraszcza wdrażanie grafiki 3D do aplikacji systemu iOS. Zestaw sceny jest również dostarczane przez firmę Apple, co powoduje integracji i wymienione powyżej dla zestawu Sprite zagadnienia specyficzne dla platformy technologię. Aby uzyskać więcej informacji na scenie Kit Zobacz ten wpis: [http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/](http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/)
- **OpenTK —** OpenTK (oznaczającą Otwórz zestaw narzędzi) zapewnia dostęp OpenGL niskiego poziomu z systemem iOS firmy Apple i Mac sprzętu. Aby uzyskać więcej informacji o OpenTK zobacz strony głównej na:  [http://www.opentk.com/](http://www.opentk.com/)


# <a name="summary"></a>Podsumowanie

W tym artykule opisano główne pojęcia tworzenia gier oraz informacje na temat sposobu rozpocząć tworzenie pierwszej gry. Po zakończeniu tego artykułu, kolejne kroki są pobieranie technologii i rozpocząć pracę za pośrednictwem naszego serii samouczków połączone w odpowiednich sekcjach powyżej.

## <a name="related-links"></a>Linki pokrewne

- [Przewodniki CocosSharp](~/graphics-games/cocossharp/index.md)
- [Przewodniki MonoGame](~/graphics-games/monogame/index.md)
- [Przewodniki UrhoSharp](~/graphics-games/urhosharp/index.md)
