---
title: Podsumowanie rozdziale 12. Style
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: bb26c5d93bc9945ad43ed62d7feba2bc851e510e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-12-styles"></a>Podsumowanie rozdziale 12. Style

Style platformy Xamarin.Forms, Zezwól wiele widoków udostępnić zbiór właściwości. Zmniejsza znaczników i umożliwia utrzymywanie spójne motywy wizualne.

Style prawie zawsze są zdefiniowane i używane w znaczniku. Obiekt typu [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) jest uruchomiony w słowniku zasobów, a następnie ustaw [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) właściwości przy użyciu elementu wizualnego `StaticResource` lub `DyanamicResource` znaczników rozszerzenie.

## <a name="the-basic-style"></a>Podstawowy styl

A `Style` wymaga, aby jego [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) należy ustawić typ obiektu visual dotyczy. Gdy `Style` zostanie uruchomiony w słowniku zasobów (jak jest często) wymagany jest również `x:Key` atrybutu.

`Style` Ma właściwość content typu [ `Setters` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Setters/), która jest kolekcją [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) obiektów. Każdy `Setter` kojarzy [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) z [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/).

W języku XAML `Property` ustawienie jest nazwą właściwości CLR (takich jak `Text` właściwość `Button`), ale właściwość styl musi był zabezpieczony za pomocą właściwości możliwej do wiązania. Również, właściwość musi być zdefiniowana w klasie wskazywanym przez `TargetType` ustawienie lub dziedziczone przez tę klasę.

Można określić `Value` ustawienie za pomocą elementu właściwości `<Setter.Value>`. Dzięki temu można ustawić `Value` do obiektu, którego nie można wyrazić w ciągu tekstowym lub do `OnPlatform` obiektu lub do obiektu wdrożyć przy użyciu `x:Arguments` lub `x:FactoryMethod`. `Value` Można również ustawić właściwość z `StaticResource` wyrażenia do innego elementu w słowniku.

[ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) program przedstawia podstawowe składnię i pokazano sposób odwoływania się do `Style` z `StaticResource` — rozszerzenie znaczników:

[![Potrójna zrzut ekranu przedstawiający styl podstawowy](images/ch12fg01-small.png "style podstawowe")](images/ch12fg01-large.png#lightbox "style podstawowe")

`Style` Obiekt i żaden z obiektów utworzonych w `Style` obiekt jako `Value` ustawienia są współużytkowane przez wszystkie widoki, który odwołuje się do `Style`. `Style` Nie może zawierać wszystko, co nie może zostać udostępniony, takich jak `View` zależnych.

Nie można ustawić obsługi zdarzeń w `Style`. `GestureRecognizers` Nie można ustawić właściwości `Style` , ponieważ nie jest obsługiwana przez właściwości możliwej do wiązania.

## <a name="styles-in-code"></a>Style w kodzie

Mimo że nie jest często, można utworzyć wystąpienia i zainicjuj `Style` obiektów w kodzie. Wskazuje na [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) próbki.

## <a name="style-inheritance"></a>Styl dziedziczenia

`Style` ma [ `BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) właściwości, które można ustawić `StaticResource` odwołuje się do innego stylu — rozszerzenie znaczników. Dzięki temu style, aby dziedziczyć z poprzednich style i dodania lub zamiany ustawienia właściwości. [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) przykładzie pokazano to.

Jeśli `Style2` opiera się na `Style1`, `TargetType` z `Style2` musi być taka sama jak `Style1` lub typ pochodzący od `Style1`. Słownik zasobów, w którym `Style1` jest przechowywana musi być tym samym słownika zasobów jako `Style2` lub wyżej w drzewie wizualnym słownika zasobów.

## <a name="implicit-styles"></a>Niejawne style

Jeśli `Style` w zasobie słownika nie ma `x:Key` atrybutu ustawienie przypisano klucza słownika automatycznie i `Style` obiekt staje się *niejawne styl*. Widok bez `Style` ustawienie, którego typ odpowiada `TargetType` dokładnie znajdzie tego stylu jako [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) przykładzie pokazano.

Niejawne styl może pochodzić od `Style` z `x:Key` ustawienia, ale nie odwrotnie. Nie można jawnie odwoływać się niejawne stylu.

Można zaimplementować trzy typy hierarchii przy użyciu stylów i `BasedOn`:

- Z style zdefiniowane na `Application` i `Page` dół style zdefiniowane na układów niższe w drzewie wizualnym.
- Z style zdefiniowane dla klas podstawowych, takich jak `VisualElement` i `View` style zdefiniowane dla określonej klasy.
- Przy użyciu stylów z kluczami jawne słownika niejawne style.

Te hierarchie przedstawiono w części [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) próbki.

## <a name="dynamic-styles"></a>Style dynamiczne

Styl ze słownika zasobów może odwoływać się `DynamicResource` zamiast `StaticResource`. Dzięki temu styl *dynamiczne styl*. Jeśli dany styl zastępuje w słowniku zasobów innym stylu z tym samym kluczem, widoki odwołujące się do stylu z `DynamicResource` automatycznie zmienić. Ponadto spowoduje braku wpis z określonym kluczem w słowniku `StaticResource` Aby zgłosić wyjątek, ale nie `DynamicResource`.

Ta metoda umożliwia dynamiczną zmianę style lub kompozycje jako [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) przykładzie pokazano.

Jednak nie można ustawić `BasedOn` właściwości `DynamicResource` w skład rozszerzenia ponieważ `BasedOn` nie jest obsługiwana przez właściwości możliwej do wiązania. Do uzyskania stylu dynamicznie, nie należy ustawiać `BasedOn`. Zamiast tego należy ustawić [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) właściwości klucza słownika stylu, aby dziedziczyć. [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) przykładzie pokazano tej metody.

## <a name="device-styles"></a>Style urządzenia

[ `Device.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) Zagnieżdżona klasa definiuje dwanaście statycznego pola tylko do odczytu dla sześciu style o `TargetType` z `Label` używanego do typowych użycia tekstu.

Sześć te pola są typu `Style` że można ustawić bezpośrednio do `Style` właściwości w kodzie:

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)

Sześć pól są typu `string` i mogą być używane jako klucze słownika style dynamicznych:

- [`BodyStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyleKey/) taki sam, jak "BodyStyle"
- [`TitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyleKey/) taki sam, jak "TitleStyle"
- [`SubtitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyleKey/) taki sam, jak "SubtitleStyle"
- [`CaptionStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyleKey/) taki sam, jak "CaptionStyle"
- [`ListItemTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyleKey/) taki sam, jak "ListItemTextStyle"
- [`ListItemDetailTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyleKey/) taki sam, jak "ListItemDetailTextStyle"

Te style są zilustrowane [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) próbki.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdziale 12 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [Przykłady rozdziale 12](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Style](~/xamarin-forms/user-interface/styles/index.md)
