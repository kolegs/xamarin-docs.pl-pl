---
title: Suwaki, przełączniki i kontrolki Segmentowane w rozszerzeniu Xamarin.iOS
description: W tym dokumencie omówiono slajdy, przełączniki i kontrolki segmentowane w rozszerzeniu Xamarin.iOS, opisujące, jak pracować z nimi programowo i w narzędziu iOS Designer.
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7df79cb6f225326dda6656fa9dfe9534e35f2457
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241994"
---
# <a name="sliders-switches-and-segmented-controls-in-xamarinios"></a>Suwaki, przełączniki i kontrolki Segmentowane w rozszerzeniu Xamarin.iOS

<a name="Sliders" />

## <a name="sliders"></a>Suwaki

Kontrolka suwaka umożliwia proste wybór wartości liczbowej w zakresie. Kontrolka wartość domyślna to wartość z zakresu od 0 do 1, ale można dostosować te limity.

 [![](slider-switch-segmented-controls-images/image25a.png "Suwak")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Poniższy zrzut ekranu przedstawia właściwości, które można edytować w Projektancie:

 [![](slider-switch-segmented-controls-images/image26a.png "Właściwości suwaka")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Wartości te można ustawiać w kodzie, jak pokazano poniżej, w tym okablowania się obsługi, aby wyświetlić obecnie wybranej wartości w `UILabel` sterowania:

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

Można również dostosować wygląd suwaka, ustawiając wartość

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

Dostosowane suwaka wygląda następująco:

 [![](slider-switch-segmented-controls-images/image27a.png "Niestandardowe suwaka")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> Ma obecnie [usterki](http://stackoverflow.com/a/19496179) powoduje `ThumbTint` nie renderowanie w czasie wykonywania, zgodnie z oczekiwaniami. Można dodać następujący wiersz kodu **przed** powyższy kod, aby uniknąć tego problemu. [[Źródła](http://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> Można użyć dowolnego obrazu, zostaną zastąpione, ale upewnij się, jest umieszczany _w_ katalog zasobów i jest wywoływana w kodzie.

<a name="Switch" />

## <a name="switch"></a>Przełącznik

dla systemu iOS używa `UISwitch` jako wartość logiczna dane wejściowe, które mogą być reprezentowane przez przycisk radiowy na innych platformach. Użytkownika można manipulować kontrolki, przenosząc *thumb* między **włączenia/wyłączenia** pozycji.

 [![](slider-switch-segmented-controls-images/image28a.png "Przełącznik")](slider-switch-segmented-controls-images/image28a.png#lightbox)

Można dostosować wygląd przełącznika w **konsoli właściwości** projektanta, które pozwalają kontrolować stan domyślny, **włączenia/wyłączenia odcień** kolorów i **obrazu/wyłączenie**. Jest to zilustrowane na poniższym obrazie:

 [![](slider-switch-segmented-controls-images/image29a.png "Właściwości przełącznika")](slider-switch-segmented-controls-images/image29a.png#lightbox)

Można również ustawić właściwości przełącznika w kodzie, na przykład poniższy kod wyświetli przełącznika z wartością domyślną `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>Kontrolki segmentowane

Kontrolki Segmentowane jest uporządkowany sposób, aby zezwolić użytkownikom na interakcję z małą liczbą opcji. Go jest rozmieszczona poziomo, a każdy z segmentów działa jako osobne przycisk. Korzystając z projektanta, kontrolki Segmentowane można znaleźć w obszarze **przybornika > kontrolki**i powinno wyglądać podobnie do poniższej ilustracji:

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "Kontrolki segmentowane")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

Dla każdego segmentu, należy wybrać indywidualnie na powierzchni projektowej umożliwia unikatowych funkcji projektanta, jak przedstawiono poniżej:

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Kontrolki segmentowane")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

Dzięki temu właściwości w konsoli umożliwiają bardziej precyzyjne sterowanie właściwości każdego segmentu. Możesz zobaczyć edytowalnych właściwości na poniższym zrzucie ekranu:

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Kontrolki segmentowane")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

Należy zauważyć, że segmenty stylu formantu jest przestarzała w systemu iOS7, a w związku z tym, dostosowując związane z tym opcje w aplikacji systemu iOS7 odniesie żadnego skutku.

## <a name="related-links"></a>Linki pokrewne

- [Formanty (przykład)](https://developer.xamarin.com/samples/Controls/)
- [Kontroler alertu](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
