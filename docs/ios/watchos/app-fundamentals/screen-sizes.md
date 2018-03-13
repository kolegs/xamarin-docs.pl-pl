---
title: Praca z rozmiaru ekranu
ms.topic: article
ms.prod: xamarin
ms.assetid: 840DF939-2F59-4ABA-87D8-92AAC8A92BC4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 81e0a9c8d65e354704f01ce70b8eb0b1b67f993f
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="working-with-screen-sizes"></a>Praca z rozmiaru ekranu

Apple Watch jest dostępny w dwóch rozmiaru ekranu:

- **38mm**
  - 136 x 170 logicznej pikseli (w pikselach fizycznych 272 x 340)

- **42mm**
  - 156 x 195 logicznej pikseli (w pikselach fizycznych 312 x 390).

Rozmiar ekranu należy wziąć pod uwagę podczas projektowania i testowania aplikacji.

## <a name="watchos-interface-designer"></a>watchOS Projektant interfejsu

Domyślnie wyświetli programu Visual Studio for Mac projektanta Obejrzyj kontrolery interfejsu w **Any Apple Watch**.

![](screen-sizes-images/screen-any-sml.png "Wyświetla projektanta Obejrzyj kontrolery interfejsu na dowolny Apple Watch")

Edytuj i Wyświetl podgląd Twojej scenorysu w jednej z rozmiaru ekranu dostępne za pomocą menu Rozmiar: **38mm** lub **42mm**:

![](screen-sizes-images/screen-menu-sml.png "Zaznaczając rozmiar 38mm lub 42mm")

Większy rozmiar ekranu czasami spowoduje, że zawartość, które zostałyby obcięte/ukryte na ekranie mniejsze.
Należy przetestować zarówno rozmiarów.


### <a name="interface-design"></a>Projekt interfejsu

Aplikację należy wyświetlić tę samą zawartość na ekranie, niezależnie od rozmiaru i powinien rozwiń lub Zwiń elementy zależnie od potrzeb. W programie Visual Studio dla projektanta Mac, inspektora atrybutu należy używać **względnym w stosunku do kontenera** lub **rozmiar, aby dopasować zawartości** zamiast stałym rozmiarze.

![](screen-sizes-images/sizeattributepanel-sml.png "Użyj względem kontenera lub rozmiar, aby dopasować zawartości zamiast stałym rozmiarze")

Ponieważ ekranu wyrażenie kontrolne jest otoczone czarna obudowa, zapewniając wypełnienia wokół interfejsu nie jest zalecane. Let elementy rest względem krawędzi ekranu i pozwól ramką tworzą naturalnych obramowania wokół aplikacji.


## <a name="watchos-simulator"></a>watchOS Simulator

Podczas testowania w symulatorze można łatwo przełączać się między rozmiar dwóch ekranu przy użyciu **sprzętu > urządzenia** menu.

![](screen-sizes-images/simulator.png "Symulator można przełączać się między rozmiary dwóch ekranu przy użyciu menu urządzenia sprzętowego")


## <a name="image-resources"></a>Zasoby obrazów

Należy używać wielu zasobów obrazu, gdy pojedynczego zasobu nie wyglądać w różnych rozmiarach. Katalogi zasobów obrazu pozwala oddzielne map bitowych może być określony dla każdego rozmiaru:

![](screen-sizes-images/images-xcassets.png "Katalog zasobów edytora")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

Można również użyć kodu do określenia rozmiaru ekranu i całkowicie załadować różnych obrazów:

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

Dowiedz się więcej o użyciu [Kontrola obrazu](~/ios/watchos/user-interface/image.md).



## <a name="related-links"></a>Linki pokrewne

- [Wprowadzenie do systemu watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
