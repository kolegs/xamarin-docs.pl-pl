---
title: Praca z watchOS układ Xamarin
description: Ten dokument zawiera opis sposobu tworzenia układu watchOS za pomocą platformy Xamarin. Zawarto informacje kontrolery interfejsu, grup, separatory i formanty zawartości.
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 11ff5ec2fc8fe99a780a3d728d3d84af59794cea
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790617"
---
# <a name="working-with-watchos-layout-in-xamarin"></a>Praca z watchOS układ Xamarin

Projektowanie układu dla Apple Watch [ekranu rozmiary](~/ios/watchos/app-fundamentals/screen-sizes.md) przedstawia wyjątkowe wyzwanie.

## <a name="design-tips"></a>Wskazówki dotyczące projektowania

Punkt klucza jest: należy interfejsu użytkownika do odczytu i użyteczny na ekranie małych czujki palcem duże. Nie można podzielić na pułapki projektowania dla **symulatora systemu iOS** (która jest wyświetlana bardzo duże) i **wskaźnik myszy** (który działa z obiektami docelowymi touch niewielki rozmiar)!

- Korzystanie z czarnym tle — tworzy wrażenie większym ekranie z czarnym ramką czujki.

- Nie konsoli wokół układu ekranu — ramką stanowi naturalne dopełnienie visual.

- Skoncentruj się na czytelność. Umożliwia rozmiary czcionek i kolorów rozważnie upewnij się, że tekst jest możliwy do odczytu. Umożliwia automatyczne Pomoc typu dynamicznego style wbudowane tekstu.

![](layout-images/type.png "Przykład obsługi typu dynamicznego")

- Skoncentruj się na touch rozmiarów obiektu docelowego. Przyciski/tappable wiersze tabeli z etykiet tekstowych powinna obejmować cały ekran. Apple mówi "nigdy nie umieścić więcej niż trzy elementy obok siebie", a jeśli używasz ikony i etykiety nie.

- Użyj [ `Menu` kontroli](~/ios/watchos/user-interface/menu.md) do funkcji Uwidacznianie rzadziej używane jasny i zwięzły projektu aplikacji.


## <a name="implementation"></a>Implementacja

Obejrzyj zestawu są dostępne następujące kontrolki ułatwiają tworzenie atrakcyjnych czujki układy aplikacji:

### <a name="interface-controller"></a>Kontroler interfejsu

`WKInterfaceController` Jest podstawowym klasy wszystkie Twoje sceny.

Powierzchnię projektu dla kontrolera interfejsu zachowuje się jak pionowym **grupy**: inne formanty można przeciągnięcie do kontrolera interfejsu i zostaną one automatycznie określić limit jeden nad drugim:

![](layout-images/controller-scene.png "Formanty są automatycznie określić limit jeden nad drugim")

Można ustawić **pozycji** i **rozmiar** właściwości każdego formantu w celu kontrolowania sposobu ich wyświetlania:

![](layout-images/positionsize-attributes.png "Ustaw właściwości pozycji i rozmiaru w każdej kontrolki")

Jeśli jest równa rozmiarowi **względnym w stosunku do kontenera** można podać wartości proporcjonalnych i dostosowanie przesunięcia. Ten zrzut ekranu przedstawia przycisk, który został ustawiony na 80% szerokości ekranu czujki Użyj (**0,8**):

![](layout-images/button-attributes.png "Podaj wartość proporcjonalne i dostosowanie przesunięcia")


### <a name="group"></a>Grupa

`WKInterfaceGroup` jest kontenera układ prosty, w którym można skonfigurować do stosu formanty w pionie lub poziomie. Obejmuje on odstępy między każdego formantu domyślnie, ale można zmodyfikować odstępy (i marginesy) w **atrybuty** inspektora.

![](layout-images/group-attributes.png "Zmodyfikuj odstępy i marginesy inspektora atrybutów")

Grupy można się być o rozmiarze znajduje się względem kontrolki wokół nich i grupy zagnieżdżone, aby utworzyć złożone układów.

![](layout-images/group-scene.png "Grupy mogą być zagnieżdżane do tworzenia złożonych układów")


### <a name="separator"></a>Separator

Formant separatora ma na celu pomoc w układzie wytyczne visual. Użyj separatorów (lub kolorów tła lub obrazów) pomóc użytkownikowi zrozumieć zawartość, która jest powiązana z ekranu.

![](layout-images/separator-scene.png "Przykład użycia separatora")

Należy pamiętać, separatory niebieskiego i zielonego, których nie należy używać pełnej szerokości ekranu zostały skonfigurowane przy użyciu jednej **stałe** lub **względnym w stosunku do kontenera** rozmiary.

### <a name="content-controls"></a>Formanty zawartości

Układ nie może być niekompletne bez `Label`, `Image`, `Button`, `Switch`, `Slider`, `Map`, i [inne formanty](~/ios/watchos/user-interface/index.md).
Może być umieszczony w układów przy użyciu **grup** lub ustawienia pozycji i rozmiaru na każdej kontrolki.



## <a name="related-links"></a>Linki pokrewne

- [WatchKitCatalog (przykład)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Odwołanie do układu firmy Apple](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Kolor & typografii firmy Apple odwołania](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
