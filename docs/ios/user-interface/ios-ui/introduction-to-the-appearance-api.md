---
title: Wygląd interfejsu API w Xamarin.iOS
description: iOS umożliwia zastosowanie ustawienia właściwości visual na poziomie klasy statycznej, a nie na pojedyncze obiekty, dzięki czemu dotyczy wszystkich wystąpień tego formantu w aplikacji.
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 02b33550451506ef4756f0f7d4400b4f98cef368
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790256"
---
# <a name="appearance-api-in-xamarinios"></a>Wygląd interfejsu API w Xamarin.iOS

_iOS umożliwia zastosowanie ustawienia właściwości visual na poziomie klasy statycznej, a nie na pojedyncze obiekty, dzięki czemu dotyczy wszystkich wystąpień tego formantu w aplikacji._

Ta funkcja jest widoczna w platformy Xamarin.iOS przy użyciu statycznego `Appearance` właściwości na wszystkich kontrolek UIKit, które ją obsługują. Wygląd (właściwości, takie jak jako obraz kolor i tło odcień) w związku z tym można łatwo dostosować, aby zapewnić spójny wygląd aplikacji. API wygląd została wprowadzona w systemie iOS 5 i podczas niektórych części są przestarzałe w systemie iOS 9 nadal jest to dobry sposób na osiągnięcie niektórych stylami i motywów efekty w aplikacji platformy Xamarin.iOS.

## <a name="overview"></a>Omówienie

iOS umożliwia dostosowanie wyglądu wielu formantów UIKit dokonanie standardowych formantów odpowiadają znakowania, które mają zastosowanie do tej aplikacji.

Istnieją dwa różne sposoby, aby zastosować niestandardowy wygląd:

- **Bezpośrednio na wystąpienie kontrolki** — na wiele formantów, w tym pasków narzędzi, pasków nawigacji, przycisków i suwaki można ustawić kolor odcienia, obrazu tła i pozycja tytułu (a także kilka innych atrybutów).

- **Określ ustawienia domyślne dla właściwości statycznej wygląd** — dostosowywalne atrybuty dla każdego formantu są udostępniane za pośrednictwem funkcji `Appearance` właściwości statycznej. Wszystkie dostosowania, które można zastosować do tych właściwości będzie używany jako domyślny dla każdego formantu tego typu, który jest tworzony po ustawieniu właściwości.

Wygląd przykładowej aplikacji przedstawiono wszystkie trzy metody, jak pokazano w tych zrzuty ekranu:

 [![](introduction-to-the-appearance-api-images/appearance01.png "Aplikacja przykładowa wygląd prezentuje wszystkie trzy metody")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

Począwszy od systemu iOS 8 TraitCollections wzbogacono wygląd serwera proxy.
 `AppearanceForTraitCollection` można ustawić domyślnego wyglądu w cechy określonej kolekcji. Możesz przeczytać więcej na temat [wprowadzenie do scenorysu](~/ios/user-interface/storyboards/unified-storyboards.md) przewodnik.


## <a name="setting-appearance-properties"></a>Ustawienie właściwości wyglądu

Na pierwszym ekranie klasy statycznej wygląd służy do określenia stylu przycisków i elementów żółty/pomarańczowy następująco:

```csharp
// Set the default appearance values
UIButton.Appearance.TintColor = UIColor.LightGray;
UIButton.Appearance.SetTitleColor(UIColor.FromRGB(0,127,14), UIControlState.Normal);

UISlider.Appearance.ThumbTintColor = UIColor.Red;
UISlider.Appearance.MinimumTrackTintColor = UIColor.Orange;
UISlider.Appearance.MaximumTrackTintColor = UIColor.Yellow;

UIProgressView.Appearance.ProgressTintColor = UIColor.Yellow;
UIProgressView.Appearance.TrackTintColor = UIColor.Orange;
```

Zielony elementu style są ustawione takie, w `ViewDidLoad` metodę, która zastępuje wartości domyślne i *wygląd* klasy statycznej:

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>Przy użyciu UIAppearance w platformy Xamarin.Forms

Wygląd interfejsu API mogą być przydatne, gdy [style aplikacji systemu iOS](~/xamarin-forms/platform/ios/theme.md#uiappearance) w rozwiązaniach platformy Xamarin.Forms. Kilka wierszy w `AppDelegate` ułatwiają klasy do zaimplementowania określony schemat kolorów bez konieczności tworzenia [niestandardowego modułu renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).


### <a name="custom-themes-and-uiappearance"></a>Niestandardowe kompozycje i UIAppearance

iOS umożliwia wiele atrybutów visual użytkownika formantów interfejsu jako "motywem" przy użyciu *UIAppearance* interfejsów API, aby wymusić wszystkie wystąpienia mają taki sam wygląd określonego formantu. Jest to widoczne jako właściwości Appearance na wiele klasy formantów interfejsu użytkownika, a nie na poszczególne wystąpienia formantu. Ustawienie właściwości wyświetlania statycznych `Appearance` właściwość ma wpływ na wszystkie formanty tego typu aplikacji.

Aby lepiej zrozumieć pojęcia, rozważ przykład.

Aby zmienić określony `UISegmentedControl` aby amarantowym odcień, firma Microsoft będzie odwoływać określonego formantu na naszych ekranu, tak jak poniżej w `ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

Możesz również ustawić wartość w konsoli właściwości projektanta: 

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "Tint — Konsola właściwości")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

Na poniższym obrazie przedstawiono ustawiający to odcień formantu o nazwie "grupę sg1".

 [![](introduction-to-the-appearance-api-images/image53.png "Ustawienie tint — formant")](introduction-to-the-appearance-api-images/image53.png#lightbox)

Aby ustawić wiele formantów w ten sposób będą całkowicie nieefektywne, możemy zamiast tego można ustawić statycznego `Appearance` właściwość samej klasy. Przedstawiono to w poniższym kodzie:

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

Na poniższym obrazie teraz przedstawiono oba segmentowanych formanty z ustawioną amarantowy wygląd:

 [![](introduction-to-the-appearance-api-images/image54.png "Ustawienie odcień wygląd formantu")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` właściwości jest ustawiana na początku cyklu życia aplikacji, takich jak AppDelegate `FinishedLaunching` zdarzenia lub ViewController przed dotyczy formanty są wyświetlane.


Zapoznaj się [wprowadzenie do interfejsu API wygląd](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) więcej szczegółowych informacji.


## <a name="related-links"></a>Linki pokrewne

- [Wygląd (przykład)](https://developer.xamarin.com/samples/monotouch/IntroToAppearance/)
- [Odwołanie do protokołu UIAppearance](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Wygląd w platformy Xamarin.Forms](~/xamarin-forms/platform/ios/theme.md#uiappearance)
