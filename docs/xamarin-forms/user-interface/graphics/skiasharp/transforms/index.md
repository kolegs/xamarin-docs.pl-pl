---
title: Transformacje SkiaSharp
description: Dowiedz się więcej o przekształceń do wyświetlania SkiaSharp grafiki
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: a94e1011557a5c7487315681e6e7c4d106ae4ba1
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/05/2018
---
# <a name="skiasharp-transforms"></a>Transformacje SkiaSharp

_Dowiedz się więcej o przekształceń do wyświetlania SkiaSharp grafiki_

SkiaSharp obsługuje transformacji tradycyjnych grafiki, które są wdrażane jako metody [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) obiektu. Matematycznie, transformacje alter współrzędnych i rozmiary, które określisz w `SKCanvas` funkcji rysowania jako obiektów graficznych są renderowane. Przekształceń są często wygodne rysowania powtarzających się grafiki lub animacji. Niektóre techniki &mdash; takich jak obracanie map bitowych lub tekst &mdash; nie jest możliwe bez korzystania z transformacji.

Transformacje SkiaSharp obsługuje następujące operacje:

- *Tłumaczenie* na współrzędne shift z jednej lokalizacji do innej
- *Skala* Aby zwiększyć lub zmniejszyć współrzędnych i rozmiary
- *Obróć* współrzędne wokół punktu obrotu
- *Pochylanie* przesunięcie koordynuje poziomo czy pionowo czego równoległobok zmieni się w prostokącie

Są one nazywane *podobne* przekształca. Affine — przekształcenia zawsze zachować równoległych i nigdy nie spowodować współrzędnych lub rozmiar stają się nieskończona. Kwadrat nigdy nie jest on przekształcany w innym niż równoległobok i koło nigdy nie jest on przekształcany w innym niż elipsy.

SkiaSharp obsługuje również-affine — przekształcenia (nazywane również *projective* lub *perspektywy* przekształca) oparte na standardowych macierzy transformacji 3 x 3. Affine — przekształcenia umożliwia kwadrat mają zostać przetworzone na dowolnym wypukłych Czworokąt (pełnego rysunku z wszystkich kąty wewnętrzne mniej niż 180 stopni). Inne niż affine — przekształcenia może spowodować współrzędne lub rozmiary stać się nieskończona, ale są istotne dla efekty 3W.

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>Różnice między SkiaSharp i transformacje platformy Xamarin.Forms

Platformy Xamarin.Forms obsługuje również transformacje, które są podobne do SkiaSharp. Platformy Xamarin.Forms [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) klasa definiuje następujące właściwości transform:

- `TranslationX` I `TranslationY`
- `Scale`
- `Rotation`, `RotationX`, i `RotationY`

`RotationX` i `RotationY` właściwości są przekształceniami perspektywy, tworzonych przez efekty skutkowało 3W.

Istnieje kilka ważnych różnic między transformacje SkiaSharp i transformacje platformy Xamarin.Forms:

Pierwsza różnica polega na SkiaSharp transformacje są stosowane do całej `SKCanvas` obiektu podczas przekształcenia platformy Xamarin.Forms są stosowane do poszczególnych `VisualElement` pochodnych. (Można zastosować do przekształcenia platformy Xamarin.Forms `SKCanvasView` obiektu, ponieważ `SKCanvasView` pochodzi z `VisualElement`, ale w tym `SKCanvasView`, zastosować przekształcenia SkiaSkarp.)

Przekształcenia SkiaSharp są względem lewego górnego rogu `SKCanvas` platformy Xamarin.Forms przekształceń są względem lewego górnego rogu `VisualElement` dla których są stosowane. Ta różnica jest ważne podczas stosowania skalowanie i obrót przekształca, ponieważ te przekształceń są zawsze względem danego punktu.

Naprawdę istotną różnicą jest to, że SKiaSharp przekształceń są *metody* podczas przekształcenia platformy Xamarin.Forms *właściwości*. Jest semantycznego różnica poza syntaktycznych różnica: transformacje SkiaSharp wykonać operacji, gdy transformacje platformy Xamarin.Forms ustawić stanu. Transformacje SkiaSharp mają zastosowanie do obiektów graficznych następnie narysowanego, ale nie do obiektów graficznych, które są rysowane przed stosowana jest transformacja. Z kolei transformacji platformy Xamarin.Forms dotyczy wcześniej renderowanego elementu jak właściwość jest ustawiona. Transformacje SkiaSharp kumulują się jako metody są wywoływane; Transformacje platformy Xamarin.Forms zostaną zastąpione, gdy właściwość jest ustawiona na inną wartość.

Wszystkie przykładowe programy w tej sekcji są wyświetlane pod nagłówkiem **przekształca** na stronie głównej [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programu, a następnie w [ **Przekształca** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms) folderu rozwiązania.

## <a name="the-translate-transformtranslatemd"></a>[Przekład — przekształcenie](translate.md)

Dowiedz się, jak używać przekształcania Przetłumacz przesunięcie SkiaSharp grafiki.

## <a name="the-scale-transformscalemd"></a>[Skalowanie — przekształcenie](scale.md)

Wykryj przekształcenia skali SkiaSharp skalowania obiektów do różnych rozmiarów.

## <a name="the-rotate-transformrotatemd"></a>[Obrót — przekształcenie](rotate.md)

Poznaj efekty i możliwe za pomocą przekształcenie obracania SkiaSharp animacji.

## <a name="the-skew-transformskewmd"></a>[Pochylenie — przekształcenie](skew.md)

Zobacz, jak utworzyć Wychylny obiektów graficznych w SkiaSharp pochylenia transformacji.

## <a name="matrix-transformsmatrixmd"></a>[Przekształcenia macierzowe](matrix.md)

Poznaj głębiej transformacje SkiaSharp z macierzy transformacji uniwersalny.

## <a name="touch-manipulationstouchmd"></a>[Manipulacje za pomocą dotyku](touch.md)

Użyj macierzy przekształca do zaimplementowania manipulacje touch przeciąganie, skalowanie i obrót.

## <a name="non-affine-transformsnon-affinemd"></a>[Przekształcenia nieafiniczne](non-affine.md)

Wykracza poza oridinary o skutki-affine — przekształcenia.

## <a name="3d-rotation3d-rotationmd"></a>[Obrót 3D](3d-rotation.md)

Affine — przekształcenia umożliwia obracanie 2D obiektów w przestrzeni 3D.


## <a name="related-links"></a>Linki pokrewne

- [Interfejsy API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
