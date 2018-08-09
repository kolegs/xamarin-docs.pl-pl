---
title: Przekształcenia biblioteki SkiaSharp
description: W tym artykule Eksploruje przekształceń do wyświetlania grafiki SkiaSharp w aplikacjach Xamarin.Forms i przedstawia to z przykładowym kodem.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 89aa29d5bf03b1d6f9668ef2aee6ce0c1a277cc5
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615863"
---
# <a name="skiasharp-transforms"></a>Przekształcenia biblioteki SkiaSharp

_Więcej informacji na temat przekształceń do wyświetlania grafiki SkiaSharp_

Skiasharp — obsługuje przekształcenia tradycyjnych grafiki, które są implementowane jako metody [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) obiektu. Matematycznych, przekształcenia można zmieniać współrzędnych i rozmiary, które są określone w `SKCanvas` funkcji rysowania, ponieważ graficzny obiekty są renderowane. Transformacje są często wygodne rysowania grafiki powtarzające się lub podczas tworzenia animacji. Kilka technik &mdash; takich jak wymiana mapy bitowe lub tekst &mdash; nie są możliwe bez stosowania przekształceń.

Przekształcenia biblioteki SkiaSharp obsługują następujące operacje:

- *Tłumaczenie* na współrzędne przesunięcia z jednej lokalizacji do innej
- *Skala* Aby zwiększyć lub zmniejszyć współrzędnych i rozmiary
- *Obróć* wymienić współrzędne wokół punktu
- *Pochylanie* przesunięcie współrzędne poziomo lub pionowo tak, że prostokąt staje się równoległobok

Są to znane jako *przekształceniem afinicznym* przekształcenia. Affine — przekształcenia zawsze zachować równoległych i nigdy nie powodują współrzędnych lub rozmiar stają się nieskończone. Kwadrat nigdy nie jest on przekształcany w żadnych innych niż równoległobok i okrąg nigdy nie jest przekształcana na coś innego niż elipsę.

Skiasharp — obsługuje również przekształcenia nieafiniczne (nazywane również *projective* lub *perspektywy* przekształca) oparte na standardowych macierzy transformacji 3 x 3. Przekształcenia nieafiniczne umożliwia kwadrat zostać przekształcone do dowolnego kwadratowe wypukłe (pełnego rysunek przy użyciu wszystkich kąty wewnętrzne mniejsza niż 180 stopni). Inne niż affine — przekształcenia może spowodować współrzędne lub rozmiarów staną się nieskończoną, ale są istotne dla efekty 3W.

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>Różnice między SkiaSharp i przekształcenia zestawu narzędzi Xamarin.Forms

Zestaw narzędzi Xamarin.Forms obsługuje również przekształceń, które są podobne do tych w SkiaSharp. Xamarin.Forms [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) klasa definiuje następujące właściwości transformacji:

- `TranslationX` i `TranslationY`
- `Scale`
- `Rotation`, `RotationX`, i `RotationY`

`RotationX` i `RotationY` właściwości są przekształcenia perspektywy, tworzonych efekty skutkowało 3D.

Istnieje kilka różnic decydujące między przekształcenia biblioteki SkiaSharp i zestawu narzędzi Xamarin.Forms przekształcenia:

Pierwszy różnica polega na tym że przekształcenia biblioteki SkiaSharp są stosowane do całej `SKCanvas` obiektów, podczas gdy przekształcenia Xamarin.Forms są stosowane do poszczególnych `VisualElement` pochodnych. (Można zastosować przekształceń zestawu narzędzi Xamarin.Forms do `SKCanvasView` obiektu, ponieważ `SKCanvasView` pochodzi od klasy `VisualElement`, ale w obrębie którego `SKCanvasView`, zastosuj przekształcenia SkiaSkarp.)

Przekształcenia biblioteki SkiaSharp są względem lewego górnego rogu `SKCanvas` Xamarin.Forms transformacje są względem lewego górnego rogu `VisualElement` dla których są stosowane. Różnica jest ważna w przypadku stosowania skalowania i obrót przekształceń, ponieważ te przekształcenia są zawsze względem danego punktu.

Tak naprawdę istotną różnicą jest to, czy przekształcenia biblioteki SKiaSharp *metody* są przekształcenia Xamarin.Forms *właściwości*. Jest to różnica semantycznego poza syntaktycznych różnicy: przekształcenia biblioteki SkiaSharp przeprowadzić operacji, gdy stan został ustawiony przekształcenia zestawu narzędzi Xamarin.Forms. Przekształcenia biblioteki SkiaSharp mają zastosowanie do obiektów graficznych później rysowane, ale nie do obiektów grafiki, które są rysowane przed zastosowaniem transformacji. Z kolei przekształcania zestawu narzędzi Xamarin.Forms ma zastosowanie do wcześniej renderowanego elementu, jak właściwość została ustawiona. Przekształcenia biblioteki SkiaSharp kumulują się zgodnie z metody są wywoływane; Przekształcenia zestawu narzędzi Xamarin.Forms zostały zastąpione, gdy właściwość jest ustawiona na inną wartość.

Wszystkich przykładowych programów w tej sekcji są wyświetlane pod nagłówkiem **przekształca** na stronie głównej [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program, a następnie w [ **Przekształca** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms) folderu rozwiązania.

## <a name="the-translate-transformtranslatemd"></a>[Przekład — przekształcenie](translate.md)

Dowiedz się, jak użyć przekład — przekształcenie do przesunięcia SkiaSharp grafiki.

## <a name="the-scale-transformscalemd"></a>[Skalowanie — przekształcenie](scale.md)

Odkryj SkiaSharp skalowanie — przekształcenie skalowania obiektów do różnych rozmiarów.

## <a name="the-rotate-transformrotatemd"></a>[Obrót — przekształcenie](rotate.md)

Poznaj efekty i animacji, które można zrobić za pomocą SkiaSharp obrót — przekształcenie.

## <a name="the-skew-transformskewmd"></a>[Pochylenie — przekształcenie](skew.md)

Zobacz, jak utworzyć Wychylny obiektów graficznych w SkiaSharp pochylenie — przekształcenie.

## <a name="matrix-transformsmatrixmd"></a>[Przekształcenia macierzowe](matrix.md)

Szczegółowe informacje przekształcenia biblioteki SkiaSharp z macierzy transformacji wszechstronna.

## <a name="touch-manipulationstouchmd"></a>[Manipulacje za pomocą dotyku](touch.md)

Użyj macierzy przekształca do zaimplementowania manipulacje dotykowej przeciąganie, skalowanie i obrót.

## <a name="non-affine-transformsnon-affinemd"></a>[Przekształcenia nieafiniczne](non-affine.md)

Wykracza poza oridinary za pomocą przekształcenia nieafiniczne efekty.

## <a name="3d-rotation3d-rotationmd"></a>[Obrót 3D](3d-rotation.md)

Przekształcenia nieafiniczne umożliwiają obracanie 2D obiekty w przestrzeni 3D.


## <a name="related-links"></a>Linki pokrewne

- [Skiasharp — interfejsy API](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (przykład)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
