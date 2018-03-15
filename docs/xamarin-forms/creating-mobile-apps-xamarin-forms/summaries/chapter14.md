---
title: "Podsumowanie rozdział 14. Układ bezwzględne"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a3980c63c31f4fdf0297fdc9b05da3590f0cac54
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-14-absolute-layout"></a>Podsumowanie rozdział 14. Układ bezwzględne

Podobnie jak `StackLayout`, [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) pochodną `Layout<View>` i dziedziczy `Children` właściwości. `AbsoluteLayout` implementuje systemu układu, który wymaga programisty do określenia pozycji jego elementów podrzędnych i, opcjonalnie, ich rozmiar. Pozycja jest określona przez lewego górnego rogu elementu podrzędnego względem lewego górnego rogu `AbsoluteLayout` w jednostkach niezależnych od urządzenia. `AbsoluteLayout` implementuje również proporcjonalne pozycjonowanie i funkcji zmiany rozmiaru.

`AbsoluteLayout` powinny być traktowane jako specjalny układu systemu można używać tylko wtedy, gdy programista może nałożyć rozmiar na elementy podrzędne (na przykład `BoxView` elementy) lub gdy wielkość elementu nie ma wpływu na pozycjonowanie inne elementy podrzędne. `HorizontalOptions` i `VerticalOptions` właściwości nie mają wpływu na elementy podrzędne `AbsoluteLayout`.

W tym rozdziale przedstawiono również ważna cecha *dołączonych właściwości* umożliwiające właściwości zdefiniowane w jedną klasę (w tym przypadku `AbsoluteLayout`) jest dołączony do innej klasy (elementem podrzędnym `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>AbsoluteLayout w kodzie

Możesz dodać element podrzędny `Children` Kolekcja `AbsoluteLayout` przy użyciu standardu [ `Add` ](https://developer.xamarin.com/api/member/System.Collections.Generic.ICollection%3CT%3E.Add/p/T/) metody, ale `AbsoluteLayout` udostępnia rozszerzone [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Rectangle/Xamarin.Forms.AbsoluteLayoutFlags/) Metoda, która umożliwia określenie [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/). Inny [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Point/) metoda wymaga tylko [ `Point` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Point/), w takim przypadku jest nieograniczonego elementu podrzędnego, a sam rozmiar.

Można utworzyć `Rectangle` wartości z [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/System.Double/System.Double/System.Double/System.Double/) wymagający cztery wartości &mdash; dwa pierwsze wskazującą położenie lewego górnego rogu elementu podrzędnego względem jego elementu nadrzędnego, a następne dwa wskazujący rozmiar tego elementu podrzędnego. Lub użyć [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/Xamarin.Forms.Point/Xamarin.Forms.Size/) wymagającego `Point` i [ `Size` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/) wartości.

Te `Add` metod przedstawiono w części [ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo), których pozycje `BoxView` elementów za pomocą `Rectangle` wartości, a `Label` elementu za pomocą tylko `Point` wartość.

[ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) przykładowe używa 32 `BoxView` elementy, aby utworzyć wzorzec chessboard. Program udostępnia `BoxView` elementy ustalony rozmiar kwadratu 35 jednostki. `AbsoluteLayout` Ma jego `HorizontalOptions` i `VerticalOptions` ustawioną `LayoutOptions.Center`, co powoduje, że `AbsoluteLayout` mieć całkowity rozmiar kwadratowe 280 jednostki.

## <a name="attached-bindable-properties"></a>Dołączone właściwości

Istnieje również możliwość ustawienia pozycji i, opcjonalnie, rozmiar elementu podrzędnego `AbsoluteLayout` po dodaniu do `Children` kolekcji przy użyciu metody statycznej [ `AbsoluteLayout.SetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutBounds/p/Xamarin.Forms.BindableObject/Xamarin.Forms.Rectangle/). Pierwszy argument jest elementem podrzędnym; druga `Rectangle` obiektu. Można określić, że podrzędne rozmiary sam poziomie lub pionie się przez ustawienie wartości szerokości i wysokości [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) stałej.

[ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) przykładowe naraża `AbsoluteLayout` w `ContentView` z `SizeChanged` Obsługa wywołująca `AbsoluteLayout.SetLayoutBounds` na wszystkie podrzędne, aby były możliwie jak największy.  

Dołączona właściwość można powiązać który `AbsoluteLayout` definiuje jest statycznego pola tylko do odczytu typu `BindableProperty` o nazwie [ `AbsoluteLayout.LayoutBoundsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/). Statycznych `AbsoluteLayout.SetLayoutBounds` metoda jest implementowana przez wywołanie metody `SetValue` w elemencie podrzędnym z `AbsoluteLayout.LayoutBoundsProperty`. Obiekt podrzędny zawiera słownik przechowywane dołączona właściwość powiązania i jej wartość. W układzie `AbsoluteLayout` można uzyskać tę wartość, wywołując [ `AbsoluteLayout.GetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutBounds/p/Xamarin.Forms.BindableObject/), wykonywane przy `GetValue` wywołania.

## <a name="proportional-sizing-and-positioning"></a>Proporcjonalne rozmiaru i pozycjonowania

`AbsoluteLayout` implementuje proporcjonalne rozmiaru i pozycjonowania funkcji. Klasa określa drugi dołączonych właściwości możliwej do wiązania, [ `LayoutFlagsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/), z odpowiednich metod statycznych [ `AbsoluteLayout.SetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutFlags/p/Xamarin.Forms.BindableObject/Xamarin.Forms.AbsoluteLayoutFlags/) i [ `AbsoluteLayout.GetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutFlags/p/Xamarin.Forms.BindableObject/).

Argument `AbsoluteLayout.SetLayoutFlags` i wartość zwracaną `AbsoluteLayout.GetLayoutFlags` jest wartością typu [ `AbsoluteLayoutFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/), wyliczenie z następujących członków:

- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.None/) (równe 0)
- [`XProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.XProportional/) (1)
- [`YProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.YProportional/) (2)
- [`PositionProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional/) (3)
- [`WidthProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional/) (4)
- [`HeightProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional/) (8)
- [`SizeProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional/) (12)
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.All/) (\xFFFFFFFF)

Możesz łączyć je z C# bitowego operatora OR.

Z tych flagi ustawione niektórych właściwości `Rectangle` proporcjonalnie interpretowania układu granice struktura używana do położenia i rozmiaru elementu podrzędnego.

Gdy `WidthProportional` flaga jest ustawiona, `Width` wartość 1 oznacza, że szerokość jest elementem podrzędnym `AbsoluteLayout`. Podejście podobne służy do wysokości.

Pozycjonowanie proporcjonalne uwzględnia wielkość. Gdy `XProportional` flaga jest ustawiona, `X` właściwość `Rectangle` granice układ jest proporcjonalna. Wartość 0 oznacza, że obiekt podrzędny lewej krawędzi znajduje się w lewej krawędzi `AbsoluteLayout`, ale pozycji 1 oznacza, że dziecka prawej krawędzi znajduje się w prawej krawędzi `AbsoluteLayout`, nie poza prawą krawędzią `AbsoluteLayout` jako może expec t. `X` Właściwości 0,5 Wyśrodkowuje podrzędne w poziomie `AbsoluteLayout`.

[ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) przykładzie przedstawiono użycie proporcjonalne rozmiaru i pozycjonowania.

## <a name="working-with-proportional-coordinates"></a>Praca z zachowaniem proporcji współrzędnych

Czasami łatwiej wziąć pod uwagę proporcjonalna pozycjonowania inaczej niż jak jest zaimplementowana w `AbsoluteLayout`. Wolisz pracować z zachowaniem proporcji współrzędne gdzie `X` właściwości 1 umieszcza dziecka lewej krawędzi (zamiast prawej krawędzi) z prawej krawędzi `AbsoluteLayout`.

Alternatywny system pozycjonowania można wywołać "współrzędne ułamkowych podrzędnej." Możesz przekonwertować współrzędne ułamkowych podrzędnych na granice układu wymagane dla `AbsoluteLayout` przy użyciu następujących formuły:

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

[ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) przykładzie pokazano to.

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout i języka XAML

Można użyć `AbsoluteLayout` w języku XAML i set dołączonych właściwości dla elementów podrzędnych `AbsoluteLayout` za pomocą wartości atrybutów `AbsoluteLayout.LayoutBounds` i `AbsoluteLayout.LayoutFlags`. To jest przedstawiona w [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) i [ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) próbek. Ostatnie program zawiera 32 `BoxView` elementów, ale używa niejawny `Style` zawierającą `AbsoluteLayout.LayoutFlags` właściwości, aby zachować znaczników do minimum.

Atrybut w XAML, który składa się z nazwy klasy, kropkę i nazwa właściwości jest *zawsze* dołączonych właściwości możliwej do wiązania.

## <a name="overlays"></a>Nakładki

Można użyć `AbsoluteLayout` do skonstruowania *nakładki*, strony z innych kontrolek który obejmuje może chronić użytkownika przed interakcji z normalnym kontrolki na stronie. 

[ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) przykładowych pokazano to technika i prezentuje działanie również [ `ProgressBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/), które powoduje wyświetlenie zakresu, do którego program zostało zakończone zadanie.

## <a name="some-fun"></a>Niektóre fun

[ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) próbki wyświetla bieżący czas wyświetlania symulowane Mozaika 5 x 7. Każdy okrąg `BoxView` (istnieją 228 z nich) o rozmiarze i umieszczony w `AbsoluteLayout`.

[![Potrójna zrzut ekranu przedstawiający zegara Mozaika](images/ch14fg08-small.png "zegara Mozaika")](images/ch14fg08-large.png#lightbox "Mozaika zegara")

[ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) program animuje dwa `Label` obiektów Odbijanie poziomie i w pionie na ekranie.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 14 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [Przykłady rozdział 14](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
