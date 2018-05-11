---
title: Poprawa szybkość klatek z CCSpriteSheet
description: CCSpriteSheet udostępnia funkcję łączenia i korzystania z wielu plików obrazów w jedną teksturę. Zmniejszenie liczby tekstury można zwiększyć czas ładowania gry i szybkość klatek.
ms.prod: xamarin
ms.assetid: A1334030-750C-4C60-8B84-1A8A54B0D00E
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 9b0f58554b26b1a5334970b8c1288234acbf8db7
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="improving-frame-rate-with-ccspritesheet"></a>Poprawa szybkość klatek z CCSpriteSheet

_CCSpriteSheet udostępnia funkcję łączenia i korzystania z wielu plików obrazów w jedną teksturę. Zmniejszenie liczby tekstury można zwiększyć czas ładowania gry i szybkość klatek._

Wiele gier wymaga optymalizacji wysiłków bezproblemowe działanie i szybkość ładowania na sprzęcie przenośnym. `CCSpriteSheet` Klasy mogą pomóc w pokonywaniu wielu typowych problemów z wydajnością występujących CocosSharp gier. W tym przewodniku przedstawiono typowe problemy z wydajnością i sposobu ich rozwiązania przy użyciu `CCSpriteSheet` klasy.


## <a name="what-is-a-sprite-sheet"></a>Co to jest arkusz sprite?

A *arkusza sprite*, które mogą być również określany jako *atlas tekstury*, jest obrazem, który łączy wiele obrazów w jednym pliku. Może to poprawić wydajność środowiska uruchomieniowego, a także czasy ładowania zawartości.

Na przykład poniższy obraz jest arkusz proste sprite utworzone przez trzy oddzielne obrazy. Poszczególne obrazy może być dowolnym rozmiarze, a wynikowy arkusza sprite nie jest wymagane do można całkowicie wypełnione:

![](ccspritesheet-images/image1.png "Poszczególne obrazy może być dowolnym rozmiarze, a wynikowy arkusza sprite nie jest wymagane do można całkowicie wypełnione")


### <a name="render-states"></a>Renderowanie stanów

Visual CocosSharp obiektów (takich jak `CCSprite`) uprościć renderowania kodu za pośrednictwem tradycyjnych graficznego interfejsu API renderowania kodu na przykład MonoGame lub OpenGL, które wymagają utworzenia buforów wierzchołków (zgodnie z opisem w [rysowania grafiki 3D Wierzchołków w MonoGame](~/graphics-games/monogame/3d/part2.md) przewodniku). Niezależnie od jego prostota CocosSharp nie eliminuje koszty ustawienie *renderowania stanów*, liczbę razy, że kod renderowania musi się przestawić tekstury lub innych stanów związanych z renderowania, które są.

Wewnętrzny kod w CocosSharp renderuje każdego elementu wizualnego sekwencyjnie, przy przechodzenie drzewa wizualnego, począwszy od bieżącej `CCScene`. Na przykład wziąć pod uwagę następujące sceny:

![](ccspritesheet-images/image2.png "Należy wziąć pod uwagę to sceny")

CocosSharp będzie renderować czterech gwiazdek w kolejności:

![](ccspritesheet-images/image3.png "CocosSharp będzie renderować czterech gwiazdek w sekwencji")

Ponieważ każdy `CCSprite` używa tego samego tekstury CocosSharp można pogrupować wszystkie cztery gwiazdki. Ten kod wymaga tylko jeden renderowania stanu przypisania (przypisanie gwiazdy tekstury) na klatkę. Ten scenariusz jest bardzo wydajny.

Oczywiście bardzo mało gry użycie tylko jednego obrazu. Następujące sceny wprowadza grafiki planety:

![](ccspritesheet-images/image4.png "Grafika planety wprowadza to sceny")

W idealnym przypadku CocosSharp powinien być rysowany wszystkie ikony przy użyciu jednego obrazu najpierw (na przykład gwiazdek), a następnie w pozostałej części ikony przy użyciu innego obrazu (świecie):

![](ccspritesheet-images/image5.png "W idealnym przypadku CocosSharp powinien być rysowany wszystkie ikony najpierw przy użyciu jednego obrazu na przykład gwiazdek, a następnie w pozostałej części ikony przy użyciu innego obrazu świecie")

Kolejność powyżej wymaga renderowania dwa stany: na pierwszą gwiazdek, na świecie pierwszy.

Jeśli wszystkie `CCSprite` wystąpienia są elementy podrzędne tego samego `CCNode`, a następnie CocosSharp zoptymalizuje kolejność rysowania, aby zmniejszyć renderowania zmiany stanu. Jeśli z drugiej strony, `CCSprite` wystąpienia są zorganizowane w taki sposób, że CocosSharp jest w stanie zoptymalizować renderowania (np. Jeśli są one częścią inną jednostkę `CCNode` wystąpień), następnie kolejność mogą nie być optymalne. Poniżej przedstawiono kolejność rysowania możliwe najgorszy, co pięć doszło do renderowania:

![](ccspritesheet-images/image6.png "Pokazuje najgorszy kolejność rysowania możliwa, co pięć doszło do renderowania")

Stany renderowania może być trudne do optymalizacji, ponieważ kolejność rysowania musi przestrzegać drzewie wizualnym `CCNode` wystąpień. Tego drzewa jest często strukturę, aby były łatwe do pracy z (na przykład jednostek zawierających ich elementów podrzędnych obiektów visual) lub zorganizowane ze względu na żądany układu wizualnego, zgodnie z definicją wykonawcy.

Oczywiście sytuacji idealnej ma stan renderowania pojedynczego, pomimo mających wiele obrazów. Gry CocosSharp można to zrobić przez łączenie wszystkich obrazów w jednym pliku, a następnie ładowania tego jeden plik (wraz z jego towarzyszące **.plist** pliku) do `CCSpriteSheet`. Przy użyciu `CCSpriteSheet` klasa staje się jeszcze bardziej istotne, gier, który ma dużą liczbę obrazów, lub które mają bardzo skomplikowane układów. 

### <a name="load-times"></a>Czas ładowania

Łączenie wielu obrazów w jednym pliku zwiększa również czas ładowania gry z kilku powodów:

 - Łączenie wielu obrazów w pojedynczym pliku można zmniejszyć ogólną liczbę pikseli używane za pośrednictwem wydajne pakowania
 - Ładowanie plików mniej oznacza mniejsze koszty każdego pliku, np. podczas analizowania nagłówków .png
 - Ładowanie plików mniej wymaga mniej czasu, co jest ważne dla opartej na dyskach nośniki, takie jak dyski DVD tradycyjnych dyskach twardych przeszukiwania

## <a name="using-ccspritesheet-in-code"></a>Przy użyciu CCSpriteSheet w kodzie

Aby utworzyć `CCSpriteSheet` wystąpienia, należy podać kod, obrazu i pliku, który definiuje regionów obrazu do użycia dla każdej ramce. Obraz mogą być ładowane jako **.png** lub **.xnb** plik (Jeśli za pomocą [zawartości potoku](~/graphics-games/cocossharp/content-pipeline/index.md)). Jest plik definiujący ramek **.plist** plików, które mogą być tworzone ręcznie lub *TexturePacker* (co omówiono poniżej).

Przykładowa aplikacja, która [można pobrać tutaj](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/), tworzy `CCSpriteSheet` z **.png** i **.plist** plików, używając następującego kodu:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

Po załadowaniu `CCSpriteSheet` zawiera `List` z `CCSpriteFrame` wystąpienia — każde wystąpienie jednego z obrazów źródłowych użyte do utworzenia cały arkusz. W przypadku liczby **SpriteSheetDemo** projektu, `CCSpriteSheet` zawiera trzy obrazy. **.Plist** pliku mogą być kontrolowane w programie Visual Studio dla komputerów Mac lub w edytorze tekstu, obrazów, które są dostępne. Jeśli firma Microsoft wyświetlić **.plist** plik w edytorze tekstów przedstawiono trzy ramki (pominięty, aby wyróżnić nazwy kluczy sekcji):


```csharp
...
<dict>
    <key>frames</key>
    <dict>
        <key>farBackground.png</key>
        ...
        <key>foreground.png</key>
        ...
<key>forestBackground.png</key>
...
```

Możemy użyć `Find` metody do znalezienia ramki według nazwy, w następujący sposób (pominięty Kod wyróżnić `CCSpriteFrame` użycia):


```csharp
CCSpriteFrame frame;
...
frame = sheet.Frames.Find(item=>item.TextureFilename == "farBackground.png"); 
CCSprite sprite = new CCSprite (frame); 
...
```

Ponieważ `CCSprite` Konstruktor może zająć `CCSpriteFrame` parametru kod nigdy nie sprawdzić szczegóły `CCSpriteFrame`, takie jak tekstury, który używa lub obszaru obrazu w arkuszu sprite wzorca.


## <a name="creating-a-sprite-sheet-plist"></a>Tworzenie .plist arkusza sprite

Plik .plist jest plik opartych na języku xml, który można tworzyć i edytować ręcznie. Podobnie programy do edycji obrazów mogą być używane do łączenia wielu plików w jeden rozmiar pliku. Od czasu utworzenia i utrzymywania arkusze sprite może być bardzo czasochłonne, zajmiemy program TexturePacker, który można eksportować pliki w formacie CocosSharp. TexturePacker oferuje bezpłatny i wersji "Zaawansowany" i jest dostępna dla systemów Windows i Mac OS. W dalszej części tego podręcznika można wykonać przy użyciu bezpłatnej wersji. 

Może być TexturePacker [pobrane z witryny sieci Web TexturePacker](https://www.codeandweb.com/texturepacker). Po otwarciu TexturePacker nie ma załadować projektu. Początkowy ekranie można dodać ikony, otwórz ostatnich projektów (jeśli zostały utworzone innych projektów), a następnie wybierz format używany w arkuszu sprite. CocosSharp używa formatu danych Cocos2D:

![](ccspritesheet-images/image7.png "CocosSharp używa formatu danych Cocos2D")

Pliki obrazów (takich jak **.png**) można dodać do TexturePacker przeciągania usunięcie ich z Eksploratora Windows w systemie Windows lub wyszukiwania na komputerach Mac. Zawsze, gdy zostanie dodany plik TexturePacker automatycznie aktualizuje Podgląd arkusza sprite:

![](ccspritesheet-images/image8.png "Zawsze, gdy zostanie dodany plik TexturePacker automatycznie aktualizuje sprite Podgląd arkusza")

Aby wyeksportować arkusz sprite, kliknij przycisk **publikowania arkusza sprite** i wybrać lokalizację arkusza sprite. TexturePacker zapisze plik .plist i plikiem obrazu.

Aby użyć pliki wynikowe, należy dodać do projektu CocosSharp .png i plist. Aby uzyskać informacje dotyczące dodawania plików do projektów CocosSharp, zobacz [przewodnik BouncingGame](~/graphics-games/cocossharp/bouncing-game.md). Po dodaniu plików, może być załadowany do `CCSpriteSheet` pokazany został wcześniej w powyższym kodzie:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

### <a name="considerations-for-maintaining-a-texturepacker-sprite-sheet"></a>Zagadnienia dotyczące obsługi arkusz sprite TexturePacker

Opracowanej gry artystów może dodać, usunąć lub zmodyfikować kompozycji. Każda zmiana wymaga arkusza sprite zaktualizowane. Przedstawione poniżej zagadnienia może ułatwić sprite arkusza konserwacji:

 - Zachowaj oryginalne pliki (pliki używane do tworzenia arkusze sprite) w folderze projektu i upewnij się, że są one dodawane do kontroli wersji. Te pliki będą potrzebne do odtworzenia arkusza sprite zawsze, gdy zostaną zmienione.
 - Nie Dodaj oryginalnych plików do programu Visual Studio for Mac/Visual Studio, lub jeśli są one dodawane **Akcja kompilacji** do **Brak**. Jeśli pliki są dodawane i mieć specyficzne dla platformy **Akcja kompilacji**, a następnie ich niepotrzebnemu ponownemu spowoduje zwiększenie rozmiaru pliku wynikowego aplikacji.
 - Należy rozważyć użycie *inteligentne folderów* w TexturePacker. Inteligentne folderów automatycznie dodać żadnych obrazów zawartych w arkuszu sprite. Ta funkcja zapisać dużo czasu, gdy tworzenie gier dużą liczbę obrazów. 

    ![](ccspritesheet-images/image9.png "Tej funkcji można zapisywać wiele czasu podczas tworzenia gier z dużą liczbę obrazów")
 - Śledzą sprite rozmiary tekstury. Niektóre starsze wersje sprzętu telefonu nie obsługuje większa niż 2048 x 2048 rozmiary tekstury. Ponadto 32-bitowego obrazu 2048 x 2048 używa prawie 17 (MB) pamięci RAM — znaczną ilość pamięci.
 - TexturePacker nie zawiera foldery w nazwach sprite domyślnie, więc możliwe są nazwa powoduje konflikt. Najlepiej można zdecydować, czy aby zawierają folder nazwy lub nie na początku programowanie. Gry większych należy rozważyć przy użyciu nazwy folderów, aby zapobiec konfliktom. Aby dołączyć ścieżki folderu, kliknij opcję **Pokaż zaawansowane** w **danych** sekcji i sprawdź **nazwa folderu Prepend**. 

    ![](ccspritesheet-images/image10.png "Aby dołączyć ścieżki folderu, kliknij przycisk Pokaż zaawansowane w sekcji danych i sprawdź nazwę folderu Prepend")

## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano sposób tworzenia i używania `CCSpriteSheet` klasy. Obejmuje ona również sposób tworzenia plików, które mogą być ładowane do `CCSpriteSheet` wystąpienia przy użyciu programu TexturePacker.

## <a name="related-links"></a>Linki pokrewne

- [CCSpriteSheet](https://developer.xamarin.com/api/type/CocosSharp.CCSpriteSheet/)
- [Pełna wersja demonstracyjna (przykład)](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)
