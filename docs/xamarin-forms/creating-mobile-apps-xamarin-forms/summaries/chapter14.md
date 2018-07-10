---
title: Podsumowanie rozdziałów 14. Układ bezwzględny
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdział 14. Układ bezwzględny'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 2dd94d5fb8eecc5cf4a3e376bc67c2cb6afb153b
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935442"
---
# <a name="summary-of-chapter-14-absolute-layout"></a>Podsumowanie rozdziałów 14. Układ bezwzględny

Podobnie jak `StackLayout`, [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) pochodzi od klasy `Layout<View>` i dziedziczy `Children` właściwości. `AbsoluteLayout` implementuje system układu, który wymaga programisty określić położenie jego elementów podrzędnych i, opcjonalnie, ich wielkości. Pozycja określono za pomocą lewego górnego rogu podrzędne względem lewego górnego rogu `AbsoluteLayout` w jednostkach niezależnych od urządzenia. `AbsoluteLayout` implementuje również proporcjonalna pozycjonowanie i funkcji zmiany rozmiaru.

`AbsoluteLayout` powinny być traktowane jako system układu specjalnych ma być używany tylko wtedy, gdy programista może to powodować rozmiar na elementy podrzędne (na przykład `BoxView` elementy) lub gdy wielkość elementu nie ma wpływu na pozycjonowanie inne elementy podrzędne. `HorizontalOptions` i `VerticalOptions` właściwości nie mają wpływu na elementy podrzędne `AbsoluteLayout`.

W tym rozdziale wprowadza również ważną funkcją *dołączonych właściwości możliwej do wiązania* umożliwiające właściwości zdefiniowane w jedną klasę (w tym przypadku `AbsoluteLayout`) do podłączenia do innej klasy (element podrzędny elementu `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>AbsoluteLayout w kodzie

Możesz dodać element podrzędny `Children` zbiór `AbsoluteLayout` przy użyciu standardu [ `Add` ](https://developer.xamarin.com/api/member/System.Collections.Generic.ICollection%3CT%3E.Add/p/T/) metody, ale `AbsoluteLayout` udostępnia także rozszerzone [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Rectangle/Xamarin.Forms.AbsoluteLayoutFlags/) Metoda, która umożliwia określenie [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/). Inny [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Point/) metoda wymaga tylko [ `Point` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Point/), w którym to przypadku elementu podrzędnego jest nieograniczonego rozmiarach i sam.

Możesz utworzyć `Rectangle` wartością [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/System.Double/System.Double/System.Double/System.Double/) wymagającym cztery wartości &mdash; pierwsze dwa wskazującą położenie w lewym górnym rogu podrzędne względem jego elementu nadrzędnego, a następne dwa wskazujący rozmiar elementu podrzędnego. Możesz też [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/Xamarin.Forms.Point/Xamarin.Forms.Size/) wymagającym `Point` i [ `Size` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/) wartości.

Te `Add` metody zostały przedstawione w [ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo), których stanowiska `BoxView` elementów za pomocą `Rectangle` wartości i `Label` elementu za pomocą tylko `Point` wartość.

[ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) przykładowy używa 32 `BoxView` elementy, aby utworzyć wzorzec chessboard. Program daje `BoxView` rozmiar elementów ustalone kwadratu 35 zespołów. `AbsoluteLayout` Ma jego `HorizontalOptions` i `VerticalOptions` równa `LayoutOptions.Center`, co powoduje, że `AbsoluteLayout` mieć całkowity rozmiar jednostki 280 kwadrat.

## <a name="attached-bindable-properties"></a>Dołączone właściwości możliwej do wiązania

Istnieje również możliwość ustawić położenie i, opcjonalnie, rozmiar elementu podrzędnego `AbsoluteLayout` po dodaniu do `Children` kolekcji za pomocą metody statycznej [ `AbsoluteLayout.SetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutBounds/p/Xamarin.Forms.BindableObject/Xamarin.Forms.Rectangle/). Pierwszy argument jest elementem podrzędnym; drugi to `Rectangle` obiektu. Można określić, czy element podrzędny o rozmiarach sam poziomo lub pionowo przez ustawienie wartości szerokości i wysokości [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) stałej.

[ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) przykładowy umieszcza `AbsoluteLayout` w `ContentView` z `SizeChanged` program obsługi, aby wywołać `AbsoluteLayout.SetLayoutBounds` na wszystkie elementy podrzędne, aby były możliwie jak największy.  

Dołączona właściwość może być powiązana, `AbsoluteLayout` definiuje statyczne pole tylko do odczytu tego typu jest `BindableProperty` o nazwie [ `AbsoluteLayout.LayoutBoundsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/). Statyczne `AbsoluteLayout.SetLayoutBounds` metoda jest implementowana przez wywołanie metody `SetValue` w elemencie podrzędnym przy użyciu `AbsoluteLayout.LayoutBoundsProperty`. Element podrzędny zawiera słownik, w którym przechowywane są dołączone właściwości możliwej do wiązania i jego wartość. Podczas układu `AbsoluteLayout` można uzyskać tę wartość przez wywołanie metody [ `AbsoluteLayout.GetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutBounds/p/Xamarin.Forms.BindableObject/), które jest implementowane za pomocą `GetValue` wywołania.

## <a name="proportional-sizing-and-positioning"></a>Proporcjonalna zmiana rozmiaru i pozycjonowania

`AbsoluteLayout` implementuje proporcjonalna rozmiaru i pozycjonowania funkcji. Klasa określa drugi dołączonych właściwości możliwej do wiązania, [ `LayoutFlagsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/), za pomocą odpowiednich metod statycznych [ `AbsoluteLayout.SetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutFlags/p/Xamarin.Forms.BindableObject/Xamarin.Forms.AbsoluteLayoutFlags/) i [ `AbsoluteLayout.GetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutFlags/p/Xamarin.Forms.BindableObject/).

Argument `AbsoluteLayout.SetLayoutFlags` i wartość zwracana przez `AbsoluteLayout.GetLayoutFlags` jest wartość typu [ `AbsoluteLayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayoutFlags), wyliczenie z następujących elementów członkowskich:

- [`None`](xref:Xamarin.Forms.AbsoluteLayoutFlags.None) (równe 0)
- [`XProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.XProportional) (1)
- [`YProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.YProportional) (2)
- [`PositionProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional) (3)
- [`WidthProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional) (4)
- [`HeightProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional) (8)
- [`SizeProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional) (12)
- [`All`](xref:Xamarin.Forms.AbsoluteLayoutFlags.All) (\xFFFFFFFF)

Możesz połączyć je z C# bitowy operator OR.

Przy użyciu zestawu tych flag, niektóre właściwości `Rectangle` struktury granice układ można zmieniać położenie i rozmiar elementu podrzędnego są interpretowane proporcjonalnie.

Gdy `WidthProportional` flaga jest ustawiona, `Width` wartość 1 oznacza, że element podrzędny jest szerokość `AbsoluteLayout`. Podejście podobne jest używana do wysokości.

Pozycjonowanie proporcjonalna uwzględnia wielkość. Gdy `XProportional` flaga jest ustawiona, `X` właściwość `Rectangle` granice układ jest proporcjonalna. Wartość 0 oznacza, że elementem podrzędnym lewej krawędzi znajduje się w lewej krawędzi `AbsoluteLayout`, ale pozycja 1 oznacza, że prawej krawędzi elementu podrzędnego jest umieszczony na prawej krawędzi paska `AbsoluteLayout`, nie dłuższy niż prawą krawędzią `AbsoluteLayout` jako może expec t. `X` Właściwość 0,5 centra podrzędne w poziomie w `AbsoluteLayout`.

[ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) przykład demonstruje użycie proporcjonalna rozmiaru i pozycjonowania.

## <a name="working-with-proportional-coordinates"></a>Praca z proporcjonalna współrzędnych

Czasami, łatwiej jest je traktować proporcjonalna pozycjonowania w sposób inny niż jego implementacji w `AbsoluteLayout`. Wolisz pracować z proporcjonalna współrzędne gdzie `X` właściwość 1 umieszcza lewą krawędzią elementu podrzędnego (zamiast prawej krawędzi) względem prawą krawędzią `AbsoluteLayout`.

Alternatywny pozycjonowania system może być wywoływana "coordinates ułamkowe podrzędnych". Możesz przekształcić współrzędne ułamkowe podrzędnych granice układ wymagane dla `AbsoluteLayout` przy użyciu następujących formuł:

layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

[ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) przykład pokazuje to.

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout i XAML

Możesz użyć `AbsoluteLayout` w XAML i ustaw właściwości możliwej do wiązania dołączone w elementy podrzędne `AbsoluteLayout` przy użyciu wartości atrybutu `AbsoluteLayout.LayoutBounds` i `AbsoluteLayout.LayoutFlags`. Jest to zaprezentowane w [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) i [ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) przykłady. Ostatnie program zawiera 32 `BoxView` elementów, ale używa ukrytego `Style` zawierającej `AbsoluteLayout.LayoutFlags` właściwość, aby zachować znaczników do minimum.

Atrybut w XAML, który składa się z nazwy klasy, kropkę i nazwę właściwości jest *zawsze* dołączoną właściwość może być powiązana.

## <a name="overlays"></a>Nakładki

Możesz użyć `AbsoluteLayout` do konstruowania *nakładki*, strony z innymi formantami, które obejmują może chronić użytkownika przed interakcji z normalnym formantów na stronie.

[ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) przykładowych pokazano tej techniki i przedstawia również [ `ProgressBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/), pokazujące zakres, do którego program zostało zakończone zadanie.

## <a name="some-fun"></a>Niektóre zabawa

[ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) przykład wyświetla bieżący czas za pomocą wyświetlacza symulowane Mozaika 5 7 dni w tygodniu. Każdy z dot jest `BoxView` (ma 228 z nich) o rozmiarze i umieszczony na `AbsoluteLayout`.

[![Potrójna zrzut ekranu przedstawiający zegara Mozaika](images/ch14fg08-small.png "zegara Mozaika")](images/ch14fg08-large.png#lightbox "Mozaika zegara")

[ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) program animuje dwa `Label` obiektów ze skokami w poziomie i w pionie na ekranie.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 14 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [Przykłady rozdział 14](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
