---
title: Wprowadzenie do tworzenia gier z CocosSharp
description: "W tym przewodniku wieloczęściowych przedstawiono sposób tworzenia prostego gier 2W za pomocą CocosSharp. Obejmuje on typowe gier pojęcia dotyczące programowania grafiki, dane wejściowe i fizycznych."
ms.topic: article
ms.prod: xamarin
ms.assetid: BCA99A61-A48D-4207-9A35-4C62F10C9AA5
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 5ab6f68aed791dd21516d663367ac5435e92d6cc
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="introduction-to-game-development-with-cocossharp"></a>Wprowadzenie do tworzenia gier z CocosSharp

_W tym przewodniku wieloczęściowych przedstawiono sposób tworzenia prostego gier 2W za pomocą CocosSharp. Obejmuje on typowe gier pojęcia dotyczące programowania grafiki, dane wejściowe i fizycznych._

Aparat CocosSharp gier 2W zapewnia technologii do wybierania gry dla wielu platform. Aby uzyskać pełną listę obsługiwanych platform zobacz [CocosSharp stron typu wiki w witrynie GitHub](https://github.com/mono/CocosSharp/wiki). W tym samouczku będzie używać C# przykłady kodu, chociaż CocosSharp jest również pełni funkcjonalny i F #.

Podstawowe CocosSharp są dostarczane przez [MonoGame framework](http://www.monogame.net/)przyspieszane interfejsu API, zapewniając grafiki, zarządzanie stanem audio, gier, dane wejściowe i zawartości potoku do importowania zasobów, które jest między platformami. CocosSharp jest warstwę abstrakcji wydajne dobrze nadaje się do gier 2W. Ponadto większych gry może wykonywać własne optymalizacje poza ich podstawowe biblioteki wraz z rozwojem gry w złożoności. Innymi słowy CocosSharp zawiera mieszane łatwość użycia i wydajności, umożliwiają deweloperom szybkie rozpoczęcie pracy bez ograniczania gier rozmiar lub złożoność.

W pierwszej sekcji tego fokus wskazówki dotyczące konfigurowania pusty projekt.  Druga część obejmuje wszystkie gier logiki zapisywania. 

Na koniec tego przewodnika będą został utworzony proste grę, gdzie odtwarzacza celem jest slajd paletkę poziomie próbę zapobiec piłka objętych wylogowuje na ekranie. Każdy podskokiem zwiększy wyniku przez jeden punkt.

![](images/image1.png "Każdy podskokiem zwiększy wyniku przez jeden punkt")

# <a name="walkthrough-parts"></a>Części wskazówki

* [Część 1 — Tworzenie projektu CocosSharp](~/graphics-games/cocossharp/first-game/part1.md)
* [Część 2 — wdrażanie BouncingGame](~/graphics-games/cocossharp/first-game/part2.md)

## <a name="related-links"></a>Linki pokrewne

- [Zawartości (przykład)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
- [Ukończono projekt (przykład)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [PCL CocosSharp na NuGet](http://www.nuget.org/packages/CocosSharp.PCL.Shared/)
- [Dokumentacja interfejsu API CocosSharp](https://developer.xamarin.com/api/namespace/CocosSharp/)
