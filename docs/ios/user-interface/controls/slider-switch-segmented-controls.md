---
title: Suwaki, przełączniki i Segmentowanych formantów
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3c98ea19b3f925e71f72b09d5356286d676a9f71
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="sliders-switches-and-segmented-controls"></a>Suwaki, przełączniki i Segmentowanych formantów

<a name="Sliders" />


## <a name="sliders"></a>Suwaki

Kontrolka suwaka umożliwia proste wybór wartość liczbową w zakresie. Formant domyślnie wartość z zakresu od 0 do 1, ale można dostosować te limity.

 [![](slider-switch-segmented-controls-images/image25a.png "Suwak")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Poniższy zrzut ekranu przedstawia właściwości, które można edytować w Projektancie:

 [![](slider-switch-segmented-controls-images/image26a.png "Właściwości suwaka")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Wartości te można ustawiać w kodzie, jak pokazano poniżej, okablowania się obsługi, aby wyświetlić wartość aktualnie wybranego w tym `UILabel` sterowania:

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

Można również dostosować wygląd suwaka przez ustawienie

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

Niestandardowych suwak wygląda następująco:

 [![](slider-switch-segmented-controls-images/image27a.png "Niestandardowe suwaka")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> Obecnie [usterki](http://stackoverflow.com/a/19496179) powoduje `ThumbTint` nie renderowania w czasie wykonywania, zgodnie z oczekiwaniami. Należy dodać następujący wiersz kodu **przed** kodu powyżej jako obejście tego problemu. [[Źródła](http://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> Można użyć dowolnego obrazu, zostaną zastąpione, ale upewnij się, że jest on umieszczany _w_ katalogu zasobów i jest wywoływana w kodzie.

<a name="Switch" />

## <a name="switch"></a>Przełącznik

iOS używa `UISwitch` zgodnie z wartością logiczną wejściowych, które mogą być reprezentowane przez przycisk radiowy na innych platformach. Użytkownika można manipulować formantu przenosząc *thumb* między **Włącz/Wyłącz** pozycji.

 [![](slider-switch-segmented-controls-images/image28a.png "Przełącznik")](slider-switch-segmented-controls-images/image28a.png#lightbox)

Można dostosować wygląd przełącznika w **konsoli właściwości** projektanta, który pozwoli kontrolować stan domyślny, **/wyłącza odcień** kolorów i **obrazu lub wyłącz**. Jest to zilustrowane na poniższej ilustracji:

 [![](slider-switch-segmented-controls-images/image29a.png "Właściwości przełącznika")](slider-switch-segmented-controls-images/image29a.png#lightbox)

Można również ustawić właściwości przełącznika w kodzie, na przykład poniższy kod przedstawia przełącznika z wartością domyślną `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>Formanty segmentu

Formant segmentu jest zorganizowany sposób Zezwalaj użytkownikom na interakcję z mniejszą liczbą opcji. Jego jest rozmieszczona poziomo i każdy z segmentów działa jako osobne przycisku. Przy użyciu projektanta, Segmentowanych formantu można znaleźć w **przybornika > formanty**i powinno wyglądać podobnie do poniższej ilustracji:

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "Formant segmentowanych")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

Unikatowy funkcji projektanta umożliwia dla każdego segmentu, należy wybrać osobno na powierzchni projektu, jak przedstawiono poniżej:

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Formant segmentowanych")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

Dzięki temu można użyć do kontrolowania dokładniej właściwości każdy z segmentów w konsoli właściwości. Można wyświetlić właściwości można edytować na poniższym zrzucie ekranu:

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Formant segmentowanych")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

Należy zauważyć, Segmentowanych stylu formantu jest przestarzała w systemie iOS7, czy w związku z tym dostosowywania opcji tego w aplikacji systemu iOS7 nie odniesie skutku.

## <a name="related-links"></a>Linki pokrewne

- [Formanty (przykład)](https://developer.xamarin.com/samples/Controls/)
- [Kontroler alertu](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
