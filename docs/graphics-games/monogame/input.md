---
title: "Odwołanie do MonoGame Konsola do gier"
description: "Konsola do gier jest klasą standardowe, i platform do uzyskiwania dostępu do urządzenia wejściowe w MonoGame."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 14a38c07b2ae5552cd9fb67d0cec581eafbf61cb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="monogame-gamepad-reference"></a>Odwołanie do MonoGame Konsola do gier

_Konsola do gier jest klasą standardowe, i platform do uzyskiwania dostępu do urządzenia wejściowe w MonoGame._

`GamePad` może być użyte do odczytu danych wejściowych z urządzenia wejściowe na wielu platformach MonoGame. W tym przewodniku pokazano, jak pracować z klasy Konsola do gier. Ponieważ układ i liczba przycisków zapewnia różni się każdego urządzenia wejściowego, ten przewodnik zawiera diagramów przedstawiających mapowania różnych urządzeń.


# <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>Konsola do gier jako zamiennik Xbox360GamePad

Oryginalny API XNA podane `Xbox360GamePad` klasę dla danych wejściowych odczytu z kontrolera gier na konsoli Xbox 360 lub komputera. MonoGame zastąpił to z `GamePad` klasy, ponieważ nie można użyć konsoli Xbox 360 kontrolerów na większości platform MonoGame (np. z systemem iOS lub konsoli Xbox One). Pomimo zmiany nazwy, użycie `GamePad` klasy jest podobna do `Xbox360GamePad` klasy.


# <a name="reading-input-from-gamepad"></a>Dane wejściowe odczytu Konsola do gier

`GameController` Klasa udostępnia sposób Zestandaryzowany odczytu danych wejściowych na dowolnej platformie MonoGame. Zawiera informacje za pomocą dwóch metod:

 - `GetState` — Zwraca bieżący stan przycisków, analogowy USB i konsola d kontrolera.
 - `GetCapabilities` — Zwraca informacje na temat możliwości sprzętu, takich jak czy kontroler ma pewne przyciski lub obsługuje wibrację.


## <a name="example-moving-a-character"></a>Przykład: Przenoszenie znak

Poniższy kod przedstawia sposób USB po lewej stronie przycisku suwaka można przenieść znak przez ustawienie jej `XVelocity` i `YVelocity` właściwości. Ten kod, przy założeniu, że `characterInstance` jest wystąpieniem obiektu, który ma `XVelocity` i `YVelocity` właściwości:


```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```


## <a name="example-detecting-pushes"></a>Przykład: Wykrywanie Wypchnięć

`GamePadState` zawiera informacje o bieżącym stanie kontrolera, takie jak czy niektórych przycisk zostanie naciśnięty. Niektóre akcje, takie jak znak przejść, co wymagać wykonania sprawdzania, czy został naciśnięty przycisk (nie był w dół ostatniej ramki, ale ta ramka nie działa) lub (był dół ostatniej ramki, ale nie w dół do tej ramki). 

Aby wykonać ten rodzaj logiki, zmienne lokalne, które przechowują poprzedniej ramki `GamePadState` i bieżącej ramki `GamePadState` musi zostać utworzony. Poniższy przykład przedstawia sposób przechowywania i użyć poprzedniej ramki `GamePadState` do zaimplementowania przeskakiwanie:


```csharp
// At class scope:
// Store the last frame's and this frame's GamePadStates.
// "new" them so that code doesn't have to perform null checks:
GamePadState lastFrameGamePadState = new GamePadState();
GamePadState currentGamePadState = new GamePadState();
protected override void Update(GameTime gameTime)
{
    // store off the last state before reading the new one:
    lastFrameGamePadState = currentGamePadState;
    currentGamePadState = GamePad.GetState(PlayerIndex.One);
    bool wasAButtonPushed = 
currentGamePadState.Buttons.A == ButtonState.Pressed
        && lastFrameGamePadState.Buttons.A == ButtonState.Released;
    if(wasAButtonPushed)
    {
        MakeCharacterJump();
    }
...
}
```


## <a name="example-checking-for-buttons"></a>Przykład: Sprawdzanie przycisków

`GetCapabilities` może służyć do sprawdzenia, czy kontroler ma niektórych sprzętu, takiego jak określonego przycisku lub analogowy USB. Poniższy kod przedstawia sposób sprawdzania B i Y przycisków w kontrolerze w grę, co wymaga obecności przyciski:


```csharp
var capabilities = GamePad.GetCapabilities(PlayerIndex.One);
bool hasBButton = capabilities.HasBButton;
bool hasXButton = capabilities.HasXButton;
if(!hasBButton || !hasXButton)
{
    NotifyUserOfMissingButtons();
}
```


# <a name="ios"></a>iOS

aplikacje dla systemu iOS obsługują dane wejściowe bezprzewodowej kontroler gier.

> [!IMPORTANT]
> Pakiety NuGet dla MonoGame 3.5 nie obsługują bezprzewodowej kontrolery gier. W systemie iOS przy użyciu klasy Konsola do gier wymaga tworzenia 3.5 MonoGame ze źródła lub przy użyciu plików binarnych MonoGame 3,6 NuGet. 



## <a name="ios-game-controller"></a>iOS kontrolera gry

`GamePad` Klasa zwraca właściwości odczytywać z kontrolerów sieci bezprzewodowej. Właściwości w `GamePad` zapewnienia dobrej pokrycia standardowe iOS sprzętu kontrolera, jak pokazano na poniższym diagramie:

![](input-images/image1.png "Właściwości w Konsola do gier zapewniają dobrą pokrycia dla standardowych systemu iOS sprzętu kontrolera, jak pokazano na tym diagramie")


# <a name="apple-tv"></a>Apple TV

Gry Apple TV, można użyć zdalnego Siri lub bezprzewodowej kontrolery gier dla danych wejściowych.


## <a name="siri-remote"></a>Używanie programu Siri zdalnego

*Używanie programu Siri zdalnego* jest natywny urządzenia wejściowego dla Apple TV. Mimo że można odczytać wartości z elementu zdalnego Siri za pomocą zdarzeń (jak pokazano w [Siri zdalnego oraz kontrolerów Bluetooth przewodnik](~/ios/tvos/platform/remote-bluetooth.md)), `GamePad` klasa może zwrócić wartości z elementu zdalnego Siri.

Zwróć uwagę, że `GamePad` można tylko do odczytu danych wejściowych z przycisk Odtwórz i dotykać powierzchni: 

![](input-images/image2.png "Zwróć uwagę, konsola do gier można tylko do odczytu danych wejściowych z przycisk Odtwórz i dotykać powierzchni")

Po naciśnięciu przeczytania powierzchni przepływu `DPad` przepływu podanych przy użyciu właściwości `ButtonState` klasy. Innymi słowy, wartości są dostępne tylko jako `ButtonState.Pressed` lub `ButtonState.Released`, w przeciwieństwie do wartości liczbowe lub gestów.


## <a name="apple-tv-game-controller"></a>Apple TV gry kontrolera

Kontrolery gier dla Apple TV zachowują się tak samo, aby kontrolery gier dla aplikacji systemu iOS. Aby uzyskać więcej informacji, zobacz [iOS kontrolera gry](#iOS_Game_Controller). 


# <a name="xbox-one"></a>Xbox One

Konsoli Xbox jeden obsługuje wprowadzanie odczytu z konsoli Xbox One kontrolera gier.


## <a name="xbox-one-game-controller"></a>Kontroler gier Xbox One

Kontroler gier konsoli Xbox One jest najczęściej używane urządzenia wejściowego dla konsoli Xbox One. `GamePad` Klasa udostępnia wartości wejściowe od sprzętu kontrolera gier.

![](input-images/image3.png "Klasa Konsola do gier udostępnia wartości wejściowe od sprzętu kontrolera gier")


# <a name="summary"></a>Podsumowanie

Omówienie MonoGame w podane w tym przewodniku `GamePad` klasy implementowania logikę odczytywania danych wejściowych i diagramy wspólnych `GamePad` implementacji.

## <a name="related-links"></a>Linki pokrewne

- [MonoGame Konsola do gier](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
