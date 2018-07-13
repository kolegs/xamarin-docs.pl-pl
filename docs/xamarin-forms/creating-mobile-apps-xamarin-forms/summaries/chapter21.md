---
title: Podsumowanie rozdziałów 21. Przekształcenia
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie działu 21. Przekształcenia'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 885a281d57a8ec83a949eba199667ea95679c5eb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998302"
---
# <a name="summary-of-chapter-21-transforms"></a>Podsumowanie rozdziałów 21. Przekształcenia

Widok zestawu narzędzi Xamarin.Forms pojawia się na ekranie w lokalizacji i rozmiaru określane przez jego obiektu nadrzędnego, który jest zazwyczaj `Layout` lub `Layout<View>` utworów zależnych. *Przekształcanie* to funkcja zestawu narzędzi Xamarin.Forms można zmodyfikować tej lokalizacji, rozmiaru lub nawet orientacji.

Zestaw narzędzi Xamarin.Forms obsługuje trzy podstawowe typy przekształcenia:

- *Tłumaczenie* &mdash; Przenieś element w poziomie lub pionie
- *Skala* &mdash; Zmień rozmiar elementu
- *Obrót* &mdash; włączyć element wokół punktu lub osi

W interfejsie Xamarin.Forms skalowanie jest izotropowego; ma to wpływ szerokość i wysokość równomiernie. Obrót jest obsługiwane zarówno w dwuwymiarowej powierzchni ekranu i w przestrzeni 3D. Istnieje nie przekształcenia niesymetryczność (lub znaczne zmniejszenie) i nie przekształcenia macierzy uogólniony.

Transformacje są obsługiwane z ośmiu właściwości typu `double` definicją `VisualElement` klasy:

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)
- [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)
- [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)
- [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)
- [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)
- [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

Te właściwości są wspierane przez właściwości możliwej do wiązania. One może być cele powiązanie danych i ich styl. [**Rozdział 22. Animacji** ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) pokazuje, jak te właściwości mogą być animowane, ale niektóre przykłady w tym rozdziale pokazują, jak animować można je przy użyciu zestawu narzędzi Xamarin.Forms [czasomierza](~/xamarin-forms/platform/device.md#Device_StartTimer).

Przekształcanie właściwości mogą wpłynąć na tylko sposób element jest renderowany i wykonaj *nie* mają wpływ na sposób element jest traktowany w układzie.

## <a name="the-translation-transform"></a>Przekształcanie tłumaczenia

Wartość różną od zera wartości [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) i [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) właściwości Przenieś element w poziomie lub pionie.

[ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) program pozwala na eksperymentowanie z tymi właściwościami przy użyciu dwóch `Slider` elementy, które kontrolują `TranslationX` i `TranslationY` właściwości `Frame`. Transformacja ma również wpływ na wszystkie elementy podrzędne tego `Frame`.

### <a name="text-effects"></a>Efekty tekstu

Jedno użycie wspólnej właściwości tłumaczenia jest nieco przesunięcie renderowanie tekstu. Jest to zaprezentowane w [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) próbki:

[![Potrójna zrzut ekranu przedstawiający przesunięcia tekstu](images/ch21fg03-small.png "przesunięcia tekstu")](images/ch21fg03-large.png#lightbox "przesunięcia tekstu")

Inny efekt jest do renderowania wielu kopii `Label` przypominają 3D bloku, tak jak pokazano w [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) próbki.

### <a name="jumps-and-animations"></a>Przechodzi i animacji

[ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) próbka używa tłumaczenia, aby przenieść `Button` zawsze wtedy, gdy jego dotknięcie jednak głównym celem jest, aby wykazać, że `Button` odbiera dane wejściowe użytkownika w miejscu gdzie Ten przycisk jest renderowany.

[ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) przykład jest podobny, ale korzysta z czasomierza, aby animować `Button` od jednego do drugiego miejsca.

## <a name="the-scale-transform"></a>Skalowanie — przekształcenie

[ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Przekształcenia można zwiększyć lub zmniejszyć rozmiar renderowanego elementu. Wartość domyślna to 1. Wartość 0 powoduje, że element, który ma być niewidoczna. Wartości ujemne spowodować, że element wydaje się być obracany o 180 stopni. `Scale` Właściwość nie ma wpływu na `Width` lub `Height` właściwości elementu. Te wartości pozostaną bez zmian.

Możesz eksperymentować z `Scale` właściwość za pomocą [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) próbki.

[ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) w przykładzie pokazano różnicę między animowanie `Scale` właściwość `Button` i animowanie `FontSize` właściwości. `FontSize` Właściwość ma wpływ na sposób, w jaki `Button` jest traktowana w układzie; `Scale` nie obsługuje właściwości.

[ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) oblicza przykładowe `Scale` właściwość, która jest stosowana do `Label` element, aby stał się możliwie jak największy cena na stronie.

### <a name="anchoring-the-scale"></a>Zakotwiczanie skali

Elementy skalowania w poprzednim trzy próbki mają wszystkie zwiększanie lub zmniejszanie rozmiaru względem środka elementu. Innymi słowy element zwiększa lub zmniejsza rozmiar, taka sama we wszystkich kierunkach. W czasie skalowania tylko punkt centralny element pozostaje w tej samej lokalizacji.

Możesz zmienić środek skalowanie przez ustawienie [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) i [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) właściwości. Te właściwości są względem samego elementu. Aby uzyskać `AnchorX`, wartość 0, który odwołuje się do elementu po lewej stronie i 1 odnosi się do prawej strony. Podobnie dla `AnchorY`, 0 to u góry i 1 jest dolnej. Obie te właściwości mają przypisane wartości domyślne cosinus 0,5, czyli Centrum.

[ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) próbki pozwala na eksperymentowanie z `AnchorX` i `AnchorY` właściwości, jak również `Scale` właściwości.

W systemie iOS przy użyciu wartości innych niż domyślne `AnchorX` i `AnchorY` właściwości jest zazwyczaj jest niezgodna z zmiany orientacji telefonu.

## <a name="the-rotation-transform"></a>Przekształcanie obrotu

[ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) Właściwość została określona w stopniach i wskazuje zegara obrotu wokół punktu zdefiniowany przez element `AnchorX` i `AnchorY`. [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) pozwala na eksperymentowanie z tych trzech właściwości.

### <a name="rotated-text-effects"></a>Efekty tekst obrócony

[ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) w przykładzie pokazano niezbędne do narysuj okręg przy użyciu 64 niewielki obracać matematyce `BoxView` elementów.

[ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) przykładzie wyświetlono wielu `Label` elementy ciągiem tekstowym obrócone o wyglądają jak szprychy.

[ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) przykład wyświetla ciąg tekstowy, który wydaje się zawijać się po okręgu.

### <a name="an-analog-clock"></a>Zegar analogowy

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) biblioteka zawiera [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) klasy, który oblicza kąty dla ręce zegara. Aby uniknąć zależności platformy w ViewModel, używa klasy `Task.Delay` zamiast czasomierza do znajdowania nowego `DateTime` wartość.

Dołączone do dodatków **Xamarin.FormsBook.Toolkit** jest [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) klasę, która implementuje `IValueConverter` i służy do zaokrąglenia drugi kąt do najbliższej drugiego.

[ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) używa trzech obracanie `BoxView` elementy, aby narysować zegar analogowy.

[ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) używa `BoxView` szersza grafiki, tym znaczników oznacza wokół tarczy zegara i przekazuje oznacza Obróć nieco odległości od ich zakończenia:

[![Potrójna zrzut ekranu przedstawiający zegara BoxView](images/ch21fg17-small.png "analogowy zegarową")](images/ch21fg17-large.png#lightbox "analogowy zegarową")

Ponadto [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) klasy w **Xamarin.FormsBook.Toolkit** powoduje, że używane wydaje się wycofać nieco zanim przejdziemy dalej, a następnie przenieść z powrotem do odpowiedniej pozycji.

### <a name="vertical-sliders"></a>Suwaki pionowe?

[ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) przykład pokazuje, że `Slider` elementów można obracać o 90 stopni i nadal działają. Jednak jest trudne do pozycji są obracane `Slider` elementów, ponieważ w układzie nadal mogą wydawać się odbywać się w poziomie.

## <a name="3d-ish-rotations"></a>Obroty 3D ish

[ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX) Właściwość pojawia się na Obróć element wokół 3D osi x, tak aby górnej i dolnej części elementu wydaje się, że przenoszenie kierunku lub od obserwatora. Podobnie [ `RotationY` ](xref:Xamarin.Forms.VisualElement.RotationY) wydaje się Obróć element wokół osi y, do lewej i prawej krawędzi elementu wydaje się, że przenoszenie kierunku lub od obserwatora.

`AnchorX` Właściwość ma wpływ na `RotationY` , ale nie `RotationX`. `AnchorY` Właściwość ma wpływ na `RotationX` , ale nie `RotationY`. Możesz eksperymentować z [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) przykładu, aby eksplorować interakcje z tymi właściwościami.

Leworęczni jest 3D współrzędnych też dorozumianych przez zestaw narzędzi Xamarin.Forms. Po wskazaniu palec dłoni po lewej stronie w kierunku X zwiększenie służy do koordynowania (z prawej strony) i palec środka w kierunku Y zwiększenie służy do koordynowania (w dół), następnie punktów thumb w kierunku rosnących Z współrzędnych (z ekranu).

Ponadto dla każdego z trzech osi po wskazaniu usługi po lewej stronie przycisku suwaka w kierunku rosnących wartości, następnie krzywej palcami wskazuje kierunek dodatnią rotacji kąty obrotu.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst działu 21 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [Przykłady działu 21](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
