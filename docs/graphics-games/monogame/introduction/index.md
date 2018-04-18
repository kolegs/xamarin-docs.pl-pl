---
title: Wprowadzenie do tworzenia gier z MonoGame
description: W tym przewodniku wieloczęściowych pokazano, jak utworzyć prostą aplikację 2D przy użyciu MonoGame.  Obejmuje on typowe gry pojęcia dotyczące programowania, takich jak grafiki, wprowadzania, gier, jednostki i fizycznych.
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: c94ed2e06ee57b67745b6a02692df2360aeb9754
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="introduction-to-game-development-with-monogame"></a>Wprowadzenie do tworzenia gier z MonoGame

_W tym przewodniku wieloczęściowych pokazano, jak utworzyć prostą aplikację 2D przy użyciu MonoGame.  Obejmuje on typowe gry pojęcia dotyczące programowania, takich jak grafiki, wprowadzania, gier, jednostki i fizycznych._

W tym artykule opisano MonoGame interfejsu API technologii do tworzenia gier i platform. Aby uzyskać pełną listę platform, zobacz [MonoGame witryny sieci Web](http://www.monogame.net/). W tym samouczku będzie używać C# przykłady kodu, chociaż MonoGame jest również pełni funkcjonalny i F #.

MonoGame jest między platformami, przyspieszane interfejsu API, zapewniając grafiki, zarządzanie stanem audio, gier, dane wejściowe i zawartości potoku do importowania zasobów. W przeciwieństwie do większości aparaty gier MonoGame Podaj lub nie nakładają żadnych struktury wzorzec lub projekt.  Gdy oznacza to, że deweloperzy mogą organizowanie ich kodu według potrzeb, oznacza to również, że potrzebne są nieco kod instalacji najpierw rozpoczęcia nowego projektu.

W pierwszej sekcji tego przewodnika koncentruje się na konfigurowaniu pusty projekt. Ostatnia sekcja obejmuje zapisywania wszystkich naszych logiki gier i zawartości, większość z nich będzie cross platform.

Na koniec tego przewodnika będzie został utworzony proste grę, gdzie odtwarzacza można kontrolować animowany znak z wprowadzaniem dotykowym.  Chociaż nie jest to technicznie pełne gier (ponieważ go nie win lub utratę warunki), pokazano wiele pojęcia do tworzenia gier i może służyć jako podstawa dla wielu typów gier. 

Poniższy kod przedstawia wynik tego przewodnika:

![Animacja próbki znak gier następujący przycisk myszy](images/image1.gif)

## <a name="monogame-and-xna"></a>Monogame i XNA

Biblioteka MonoGame ma na celu naśladować biblioteki XNA firmy Microsoft w składni i funkcjonalność.  Wszystkie obiekty MonoGame istnieje w przestrzeni nazw Microsoft.Xna — dzięki czemu większość kodu XNA do użycia w MonoGame bez żadnych modyfikacji. 

Deweloperzy zapoznać się z XNA już będą zapoznać się ze składnią MonoGame firmy, a deweloperzy wyszukiwania, aby uzyskać dodatkowe informacje na temat pracy z MonoGame będzie mógł odwoływać się do istniejącego wskazówki XNA online, dokumentacja interfejsu API i dyskusji.


## <a name="walkthrough-parts"></a>Części wskazówki

- [Część 1 — Tworzenie projektu MonoGame Międzyplatformowego](~/graphics-games/monogame/introduction/part1.md)
- [Część 2 — wdrażanie WalkingGame](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>Linki pokrewne

- [WalkingGame MonoGame projekt (przykład)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
- [IOS XNB czcionek](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [XNB Fonts Android](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [MonoGame Android na NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [IOS MonoGame na NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [Dokumentacja interfejsu API MonoGame](http://www.monogame.net/documentation/?page=main)
