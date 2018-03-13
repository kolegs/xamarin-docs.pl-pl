---
title: "Podsumowanie działu 21. Przekształcenia"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: afb8e2fff58583dc8648c55839649c96cb68b6ba
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-21-transforms"></a>Podsumowanie działu 21. Przekształcenia

Widok platformy Xamarin.Forms pojawia się na ekranie położenie i rozmiar określony przez jego element nadrzędny, który jest zazwyczaj `Layout` lub `Layout<View>` zależnych. *Przekształcenie* funkcja platformy Xamarin.Forms można modyfikować tej lokalizacji, rozmiaru lub nawet orientacji.

Platformy Xamarin.Forms obsługuje trzy podstawowe typy transformacji:

- *Tłumaczenie* & #x 2014; przesunięcie elementu w poziomie lub pionie
- *Skala* & #x 2014; Zmień rozmiar elementu
- *Obracanie* & #x 2014; Włącz element wokół punktu lub osi

W przypadku platformy Xamarin.Forms skalowanie jest izotropowego; wpływa na szerokość i wysokość jednakowo. Obracanie jest obsługiwana zarówno na powierzchni dwuwymiarowa ekranu i w przestrzeni 3D. Brak nie przekształcenia pochylenia (lub znaczne zmniejszenie), a nie przekształcenia macierzy uogólniony.

Przekształceń są obsługiwane z ośmioma właściwości typu `double` zdefiniowane przez `VisualElement` klasy:

- [`TranslationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)
- [`TranslationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)
- [`Scale`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)
- [`Rotation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)
- [`RotationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/)
- [`RotationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/)
- [`AnchorX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/)
- [`AnchorY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)

Te właściwości są wspierany przez właściwości. Może być cele wiązania z danymi, a stylem. [**Rozdział 22. Animacja** ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) pokazano, jak można animować właściwości, ale niektóre przykłady w tym rozdziale Pokaż animowania je za pomocą platformy Xamarin.Forms [czasomierza](~/xamarin-forms/platform/device.md#Device_StartTimer).

Przekształć właściwości wpływ tylko sposób renderowania elementu i wykonaj *nie* mają wpływ na sposób element jest traktowany w układzie.

## <a name="the-translation-transform"></a>Przekształcanie tłumaczenia

Podano niezerowe wartości [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) i [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) właściwości przesunięcie elementu w poziomie lub pionie.

[ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) program umożliwia Poeksperymentuj z tymi właściwościami przy użyciu dwóch `Slider` elementów, które kontrolują `TranslationX` i `TranslationY` właściwości `Frame`. Transformacja wpływa na wszystkie elementy podrzędne tego `Frame`.

### <a name="text-effects"></a>Efektów tekstowych

Jedno użycie wspólnej właściwości tłumaczenia jest nieco przesunięcie renderowanie tekstu. To jest przedstawiona w [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) próbki:

[![Potrójna zrzut ekranu przedstawiający powoduje przesunięcie tekstu](images/ch21fg03-small.png "przesunięcia tekst")](images/ch21fg03-large.png#lightbox "przesunięcia tekstu")

Inny powoduje renderowanie wiele kopii `Label` tak, aby przypominały 3D bloku, takich jak zostało to pokazane w [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) próbki.

### <a name="jumps-and-animations"></a>Skok i animacji

[ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) próbki używa tłumaczenia, aby przenieść `Button` zawsze, gdy jest on dotknięciu, ale podstawowym celem jest wykazać, że `Button` odbiera dane wejściowe użytkownika w lokalizacji gdzie Ten przycisk jest renderowany.

[ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) próbki jest podobny, ale używa czasomierza do animowania `Button` z jednego miejsca do innego.

## <a name="the-scale-transform"></a>Przekształcanie skali

[ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Przekształcenia można zwiększyć lub zmniejszyć rozmiar renderowanego elementu. Wartość domyślna to 1. Wartość 0 powoduje, że element ma być niewidoczna. Wartości ujemne spowodować element wydaje się być obracany o 180 stopni. `Scale` Właściwość nie ma wpływu na `Width` lub `Height` właściwości elementu. Wartości te pozostają takie same.

Możesz eksperymentować z `Scale` za pomocą właściwości [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) próbki.

[ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) przykładzie pokazano różnicę między animacji `Scale` właściwość `Button` i animowanie `FontSize` właściwości. `FontSize` Właściwość ma wpływ na sposób `Button` jest traktowany w układzie; `Scale` nie obsługuje właściwości.

[ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) oblicza przykładowe `Scale` właściwość, która jest stosowana do `Label` element, aby stał się możliwie jak największy cena na stronie.

### <a name="anchoring-the-scale"></a>Zakotwiczanie skali

Skalowany w poprzednich przykładach trzy elementy mają wszystkie zwiększyć lub zmniejszyć rozmiar względem Centrum elementu. Innymi słowy element zwiększa lub zmniejsza rozmiar taka sama we wszystkich kierunkach. W tej samej lokalizacji w czasie skalowanie pozostaje tylko punkt w środku elementu.

Możesz zmienić Centrum skalowania przez ustawienie [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) i [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) właściwości. Te właściwości są względem samego elementu. Aby uzyskać `AnchorX`, wartość 0, który odwołuje się do lewej strony elementu i 1 odwołuje się do prawej strony. Podobnie dla `AnchorY`0 jest górnej i dolnej jest 1. Obie właściwości mają przypisane wartości domyślne 0,5, która jest Centrum.

[ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) próbki pozwala na wypróbowanie `AnchorX` i `AnchorY` właściwości, jak również `Scale` właściwości.

W systemach iOS, przy użyciu wartości innych niż domyślne `AnchorX` i `AnchorY` właściwości jest zazwyczaj niezgodny z telefonu zmiany orientacji.

## <a name="the-rotation-transform"></a>Przekształcanie obrotu

[ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Właściwości określona w stopniach, określa obrót wokół punktu zdefiniowany przez element `AnchorX` i `AnchorY`. [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) służy do eksperymentów z tych trzech właściwości.

### <a name="rotated-text-effects"></a>Efekty obracany tekst

[ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) w przykładzie pokazano, należy zwrócić koło przy użyciu 64 niewielki rozmiar obracać matematyce `BoxView` elementów.

[ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) przykładzie wyświetlono wielu `Label` elementy o tej samej ciąg tekstowy obracać się pojawiać, takich jak klienci.

[ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) próbki wyświetla ciąg tekstowy, który pojawi się do zakodowania w okręgu.

### <a name="an-analog-clock"></a>Zegar analogowy

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka zawiera [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) klasy, która oblicza kąty dla ręce zegara. Aby uniknąć zależności platformy w ViewModel, używa klasy `Task.Delay` zamiast czasomierza do znajdowania nowy `DateTime` wartość.

Zawarte w **Xamarin.FormsBook.Toolkit** jest [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) klasa implementująca `IValueConverter` i służy do zaokrąglenia drugi kąt najbliższej sekundy.

[ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) używa trzech obracanie `BoxView` elementy do rysowania zegar analogowy.

[ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) używa `BoxView` dla bardziej dużą ilością grafiki, w tym znaczników oznacza wokół tarczy zegara i przekazuje to obracania małego odległości od ich końce:

[![Potrójna zrzut ekranu przedstawiający zegara BoxView](images/ch21fg17-small.png "tarczy zegara analogowy")](images/ch21fg17-large.png#lightbox "analogowy tarczy zegara")

Ponadto [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) klasy w **Xamarin.FormsBook.Toolkit** powoduje, że używane wydaje się wycofać nieco przed przeskok w przód, a następnie przenieść z powrotem do odpowiedniej pozycji.

### <a name="vertical-sliders"></a>Suwaki pionowe?

[ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) próbki wykaże, że `Slider` elementów może być obracany o 90 stopni i nadal działają. Jednak trudno umieść je obracać `Slider` elementów, ponieważ w układzie one nadal wydają się być poziomej.

## <a name="3d-ish-rotations"></a>Obrotów 3D ish

[ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) Właściwości wydaje się Obróć element wokół 3D osi x, tak aby górny i dolny elementu wydają się przenieść kierunku lub od obserwatora. Podobnie [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) wydaje się Obróć element wokół osi y, do lewej i prawej krawędzi elementu, prawdopodobnie można przenieść do lub od obserwatora.

`AnchorX` Właściwość ma wpływ na `RotationY` , ale nie `RotationX`. `AnchorY` Właściwość ma wpływ na `RotationX` , ale nie `RotationY`. Możesz eksperymentować z [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) przykład, aby eksplorować interakcji z tych właściwości.

Dla leworęcznych jest 3D współrzędnych implikowana przez platformy Xamarin.Forms. Jeśli punkt wskazującym lewej ręki w kierunku X zwiększa współrzędne (z prawej strony) i palca środka w kierunku Y zwiększa współrzędne (wyłączone), następnie punkty przycisku przewijania w kierunek narastania Z współrzędne (poza ekranu).

Ponadto dla każdego z trzech osi Jeśli punkt użytkownika po lewej stronie przycisku suwaka w kierunek narastania wartości, następnie krzywej palcami wskazuje kierunek dodatnią obracania kąty obrotu.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 21 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [Przykłady działu 21](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
