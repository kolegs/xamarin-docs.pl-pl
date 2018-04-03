---
title: CocosSharp
description: Ten dokument łącza do różnych artykułów na temat tworzenia gier z CocosSharp.
ms.topic: article
ms.prod: xamarin
ms.assetid: 5E72869D-3541-408B-AB64-D34C777AFB79
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: d61f74aefad09935b957b15ebb0daafb61dab8d5
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2018
---
# <a name="cocossharp"></a>CocosSharp

_CocosSharp jest biblioteki do tworzenia gier 2W przy użyciu języka C# i F #. Jest port .NET popularnych aparatu Cocos2D._

## <a name="introduction-to-cocossharp"></a>Wprowadzenie do CocosSharp

Aparat CocosSharp gier 2W zapewnia technologii do wybierania gry dla wielu platform. Aby uzyskać pełną listę obsługiwanych platform zobacz [CocosSharp stron typu wiki w witrynie GitHub](https://github.com/mono/CocosSharp/wiki).
Te przewodniki Użyj C# dla przykładów kodu, chociaż CocosSharp jest również pełni funkcjonalny i F #.

Podstawowe CocosSharp są dostarczane przez [MonoGame framework](http://www.monogame.net/)przyspieszane interfejsu API, zapewniając grafiki, zarządzanie stanem audio, gier, dane wejściowe i zawartości potoku do importowania zasobów, które jest między platformami.
CocosSharp jest warstwę abstrakcji wydajne dobrze nadaje się do gier 2W.
Ponadto większych gry może wykonywać własne optymalizacje poza ich podstawowe biblioteki wraz z rozwojem gry w złożoności. Innymi słowy CocosSharp zawiera mieszane łatwość użycia i wydajności, umożliwiają deweloperom szybkie rozpoczęcie pracy bez ograniczania gier rozmiar lub złożoność.

To praktyczne wideo przedstawia sposób tworzenia prostego CocosSharp wieloplatformowych gier:

> [!Video https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Developing-Cross-platform-2D-Games-in-C-and-CocosSharp/player]

## <a name="bouncinggamegraphics-gamescocossharpbouncing-gamemd"></a>[BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)

![BouncingGame](images/bouncing-game.png "BouncingGame")

Ten przewodnik zawiera opis BouncingGame, w tym sposób pracy z zawartości, używany do tworzenia gier, Dodawanie logiki gry i innych różnych elementów wizualnych.

## <a name="fruity-falls-gamegraphics-gamescocossharpfruity-fallsmd"></a>[Wypada owocowy gry](~/graphics-games/cocossharp/fruity-falls.md)

![Zrzut ekranu gier owocowy wypada](images/fruity-falls.png "owocowy wypada gier zrzut ekranu")

W tym przewodniku opisano gry wypada Fruity obejmujące wspólnej CocosSharp i tworzenia gier pojęcia, takie jak fizycznych, zarządzania zawartością stanu gry i gier projektu.  

## <a name="coin-time-gamegraphics-gamescocossharpcointimemd"></a>[Monety czasu gry](~/graphics-games/cocossharp/cointime.md)

![Monety czasu gry zrzut ekranu](images/cointime.png "monety czasu gry zrzut ekranu")

Czas monety jest pełna platformera gier dla systemu iOS i Android. Celem gry jest zbieranie wszystkich monet na poziomie i następnie osiągnąć drzwi zakończenia unikając wrogów i wąskich gardeł.

## <a name="drawing-geometry-with-ccdrawnodegraphics-gamescocossharpccdrawnodemd"></a>[Rysowanie geometrii z CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md)

![Kształty z CCDrawNode](images/ccdrawnode.png "narysowany CCDrawNode kształtów")

CCDrawNode udostępnia metody rysowania obiektów pierwotnych, takich jak linie, okręgi i trójkąty.

## <a name="animating-with-ccactiongraphics-gamescocossharpccactionmd"></a>[Animowanie za pomocą narzędzia CCAction](~/graphics-games/cocossharp/ccaction.md)

![Animacja CCAction](images/ccaction.png "A CCAction animacji")

`CCAction` jest klasą podstawową, która może służyć do Animowanie obiektów CocosSharp. Ten przewodnik obejmuje wbudowane `CCAction` implementacji dla typowych zadań, takich jak rozmieszczania, skalowanie i obrót. Prawdopodobnie również w sposób tworzenia niestandardowych implementacji przez dziedziczenie z `CCAction`.

## <a name="using-tiled-with-cocossharpgraphics-gamescocossharptiledmd"></a>[Stosowanie układu sąsiadującego za pomocą narzędzia CocosSharp](~/graphics-games/cocossharp/tiled.md)

![Poziom w grę](images/tiled.png "poziomu gry")

Rozmieszczany jest wydajne, elastyczne i mapowania dojrzałe aplikacji do tworzenia Kafelek prostopadły i izometryczny gier. CocosSharp zapewnia wbudowanej integracji sąsiadująco w natywny plik formatu.

## <a name="entities-in-cocossharpgraphics-gamescocossharpentitiesmd"></a>[Jednostki w narzędziu CocosSharp](~/graphics-games/cocossharp/entities.md)

![Pojazdu kosmicznego z gry](images/entities.png "pojazdu kosmicznego z gry")

Wzorzec jednostki to wydajny sposób organizowania gier kodu. Poprawia czytelność, sprawia, że kod jest łatwiejsze w obsłudze i korzysta z funkcji wbudowanych nadrzędny/podrzędny.

## <a name="handling-multiple-resolutions-in-cocossharpgraphics-gamescocossharpresolutionsmd"></a>[Obsługa wielu rozwiązań w CocosSharp](~/graphics-games/cocossharp/resolutions.md)

![Siatka reprezentujący rozdzielczość ekranu](images/resolutions.png "siatka reprezentujący rozdzielczość ekranu")

W tym przewodniku pokazano, jak pracować z CocosSharp tworzenia gier, w których wyświetlać się poprawnie na urządzeniach różnej rozdzielczości.

## <a name="cocossharp-content-pipelinegraphics-gamescocossharpcontent-pipelineindexmd"></a>[Potok zawartości w narzędziu CocosSharp](~/graphics-games/cocossharp/content-pipeline/index.md)

![XNB](images/content-pipeline.png "XNB")

Potoki zawartości są często używane w opracowywaniu gier do optymalizacji zawartości i sformatuj go w taki sposób, że może zostać załadowany na niektórych sprzętu lub niektórych struktur tworzenia gier.

## <a name="improving-frame-rate-with-ccspritesheetgraphics-gamescocossharpccspritesheetmd"></a>[Poprawa szybkość klatek z CCSpriteSheet](~/graphics-games/cocossharp/ccspritesheet.md)

![Drzewo z CCSpriteSheet](images/ccspritesheet.png "drzewa z CCSpriteSheet")

`CCSpriteSheet` udostępnia funkcję łączenia i korzystania z wielu plików obrazów w jedną teksturę. Zmniejszenie liczby tekstury można zwiększyć czas ładowania gry i szybkość klatek.

## <a name="texture-caching-using-cctexturecachegraphics-gamescocossharptexture-cachemd"></a>[Buforowanie tekstury za pomocą CCTextureCache](~/graphics-games/cocossharp/texture-cache.md)

![Reprezentacja jak CocosSharp buforuje obrazy](images/texture-cache.png "reprezentację jak CocosSharp buforuje obrazów")

W CocosSharp `CCTextureCache` klasa udostępnia standardowy sposób organizowania pamięci podręcznej i zwolnić zawartości. 

## <a name="2d-math-with-cocossharpgraphics-gamescocossharpmathmd"></a>[2D matematyczne z CocosSharp](~/graphics-games/cocossharp/math.md)

![Obraz jest obracana](images/math.png "za obrazu")

W tym przewodniku dotyczą 2D matematyce do tworzenia gier. Wykorzystuje CocosSharp aby pokazują, jak wykonywać typowe zadania tworzenia gier oraz wyjaśniono matematyczne za te zadania.

## <a name="performance-and-visual-effects-with-ccrendertexturegraphics-gamescocossharpccrendertexturemd"></a>[Wydajność i efekty wizualne z CCRenderTexture](~/graphics-games/cocossharp/ccrendertexture.md)

![Sprite z gry](images/ccrendertexture.png "sprite z gry")

`CCRenderTexture` Klasa udostępnia funkcje do renderowania wiele obiektów CocosSharp do pojedynczego tekstury. Raz utworzone, `CCRenderTexture` wystąpień może służyć do renderowania grafiki wydajne i wdrożenie efektów wizualnych.
