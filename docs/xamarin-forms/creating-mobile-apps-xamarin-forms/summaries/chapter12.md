---
title: Podsumowanie rozdziałów 12. Style
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdziałów 12. Style'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 3bab1d4b61f146f8caa005e9148df745c91a3ebd
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998178"
---
# <a name="summary-of-chapter-12-styles"></a>Podsumowanie rozdziałów 12. Style

W interfejsie Xamarin.Forms style, Zezwalaj na wiele widoków, aby udostępnić zbiór ustawień właściwości. Zmniejsza znaczników i umożliwia utrzymanie spójne motywy wizualne.

Style prawie zawsze są zdefiniowane i używane w znacznikach. Obiekt typu [ `Style` ](xref:Xamarin.Forms.Style) jest tworzone w słowniku zasobów, a następnie ustaw [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) właściwości przy użyciu elementu wizualnego `StaticResource` lub `DyanamicResource` znaczników rozszerzenie.

## <a name="the-basic-style"></a>Podstawowy styl

A `Style` wymaga, aby jego [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) można ustawić typ obiektu visual dotyczy. Gdy `Style` zostanie uruchomiony w słowniku zasobów (co jest typowe) wymaga również `x:Key` atrybutu.

`Style` Ma właściwość zawartości typu [ `Setters` ](xref:Xamarin.Forms.Style.Setters), który jest kolekcją [ `Setter` ](xref:Xamarin.Forms.Setter) obiektów. Każdy `Setter` kojarzy [ `Property` ](xref:Xamarin.Forms.Setter.Property) z [ `Value` ](xref:Xamarin.Forms.Setter.Value).

W XAML `Property` ustawienie jest nazwą właściwości środowiska CLR (takie jak `Text` właściwość `Button`), ale ze stylem właściwości muszą być chronione przez właściwości możliwej do wiązania. Ponadto właściwość musi być zdefiniowana w taki sposób, w klasie, wskazywanym przez `TargetType` ustawienie lub dziedziczone przez tę klasę.

Można określić `Value` ustawienie przy użyciu elementu właściwości `<Setter.Value>`. Dzięki temu można ustawić `Value` do obiektu, którego nie można wyrazić w ciągu tekstowym lub do `OnPlatform` obiektu lub do obiektu tworzone przy użyciu `x:Arguments` lub `x:FactoryMethod`. `Value` Właściwość można ustawić za pomocą `StaticResource` wyrażenia z innym elementem w słowniku.

[ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) program pokazuje podstawową składnię i pokazuje, jak odwołać się do `Style` z `StaticResource` — rozszerzenie znaczników:

[![Potrójna zrzut ekranu przedstawiający podstawowe style](images/ch12fg01-small.png "podstawowe style")](images/ch12fg01-large.png#lightbox "podstawowe style")

`Style` Obiektu i dowolny obiekt utworzony w `Style` obiektu jako `Value` ustawienia są współużytkowane przez wszystkie widoki, które odwołuje się do `Style`. `Style` Nie może zawierać wszystko, co nie może być współużytkowana, takich jak `View` utworów zależnych.

Nie można ustawić programy obsługi zdarzeń w `Style`. `GestureRecognizers` Nie można ustawić właściwości `Style` , ponieważ nie jest obsługiwana przez właściwości możliwej do wiązania.

## <a name="styles-in-code"></a>Style w kodzie

Chociaż nie jest powszechne, możesz utworzyć wystąpienia i zainicjuj `Style` obiektów w kodzie. Wskazuje na [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) próbki.

## <a name="style-inheritance"></a>Dziedziczenie stylów

`Style` ma [ `BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) właściwości, które można ustawić `StaticResource` odwołuje się do innego stylu — rozszerzenie znaczników. Dzięki temu style, które dziedziczą z poprzednim stylów i dodania lub zastąpienia ustawień właściwości. [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) przykład pokazuje to.

Jeśli `Style2` opiera się na `Style1`, `TargetType` z `Style2` musi być taka sama jak `Style1` lub pochodzić od `Style1`. Słownik zasobów, w którym `Style1` są przechowywane muszą być tego samego słownik zasobów jako `Style2` lub wyższej w drzewie wizualnym słownika zasobów.

## <a name="implicit-styles"></a>Style niejawne

Jeśli `Style` w zasobie słownika nie ma `x:Key` atrybutu ustawienie, jest automatycznie przypisany klucz ze słownika i `Style` obiekt staje się *styl niejawny*. Widok bez `Style` ustawienie i którego typ jest zgodny `TargetType` dokładnie znajdzie stylu, jako [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) w przykładzie pokazano.

Styl niejawny może pochodzić z `Style` z `x:Key` ustawienie, ale nie odwrotnie. Nie można jawnie odwoływać stylu niejawnego.

Możesz wdrożyć trzy typy hierarchii przy użyciu stylów i `BasedOn`:

- Przy użyciu stylów zdefiniowany w `Application` i `Page` dół style zdefiniowane na układy znajdujące się w drzewie wizualnym.
- Przy użyciu stylów zdefiniowany dla klas podstawowych, takich jak `VisualElement` i `View` style zdefiniowane dla określonej klasy.
- Przy użyciu stylów za pomocą kluczy słownika jawne style niejawne.

Te hierarchie zostały przedstawione w [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) próbki.

## <a name="dynamic-styles"></a>Style dynamiczne

Styl w słowniku zasobów mogą być przywoływane przez `DynamicResource` zamiast `StaticResource`. To sprawia, że styl *style dynamiczne*. Jeśli stylu został zastąpiony w słowniku zasobów innym stylu z tym samym kluczem widoki odwołujące się do tego stylu z `DynamicResource` automatyczną zmianę. Ponadto spowoduje, że braku wpis z określonym kluczem w słowniku `StaticResource` Aby zgłosić wyjątek, ale nie `DynamicResource`.

Ta technika umożliwia dynamiczną zmianę stylu lub kompozycje jako [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) w przykładzie pokazano.

Jednak nie można ustawić `BasedOn` właściwości `DynamicResource` rozszerzenia korzeń ponieważ `BasedOn` nie jest obsługiwana przez właściwości możliwej do wiązania. Do uzyskania stylu dynamicznie, nie należy ustawiać `BasedOn`. Zamiast tego należy ustawić [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) właściwości stylu, aby dziedziczyć klucz ze słownika. [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) w przykładzie pokazano tej techniki.

## <a name="device-styles"></a>Style urządzenia

[ `Device.Styles` ](xref:Xamarin.Forms.Device.Styles) Zagnieżdżona klasa definiuje dwunastu statycznego pola tylko do odczytu dla sześciu stylów z `TargetType` z `Label` używanego do często spotykanych rodzajów użycia tekstu.

Sześć te pola są typu `Style` , można ustawić bezpośrednio do `Style` właściwości w kodzie:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)

Sześć pól są typu `string` i mogą być używane jako klucze słownikowe do style dynamiczne:

- [`BodyStyleKey`](xref:Xamarin.Forms.Device.Styles.BodyStyleKey) równa wartość Wrapped "dla"
- [`TitleStyleKey`](xref:Xamarin.Forms.Device.Styles.TitleStyleKey) równa się "TitleStyle"
- [`SubtitleStyleKey`](xref:Xamarin.Forms.Device.Styles.SubtitleStyleKey) równa się "SubtitleStyle"
- [`CaptionStyleKey`](xref:Xamarin.Forms.Device.Styles.CaptionStyleKey) równa się "CaptionStyle"
- [`ListItemTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyleKey) równa się "ListItemTextStyle"
- [`ListItemDetailTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyleKey) równa się "ListItemDetailTextStyle"

Te style są zilustrowane [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) próbki.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdziale 12 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [Przykłady rozdziale 12](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Style](~/xamarin-forms/user-interface/styles/index.md)
