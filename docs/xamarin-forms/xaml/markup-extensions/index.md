---
title: Rozszerzenia znaczników XAML
description: Rozszerz zakres źródła, z których XAML atrybuty są ustawiane
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: b81bc4b31edd1d8b8f5f43f97885c38e889dd32c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-markup-extensions"></a>Rozszerzenia znaczników XAML

Rozszerzenia znaczników XAML wydłużenia możliwościach i elastyczności XAML, zezwalając atrybuty elementu ustawiono ze źródeł innych niż ciągi literału.

Na przykład, zazwyczaj należy ustawić `Color` właściwość `BoxView` podobnie do następującej:

```xaml
<BoxView Color="Blue" />
```

Lub może go ustawić na wartość szesnastkowa kolor RGB:

```xaml
<BoxView Color="#FF0080" />
```

W obu przypadkach ciąg tekstowy ustawioną `Color` atrybutu jest konwertowany na `Color` wartość o [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) klasy.

Warto zamiast tego ustaw `Color` atrybutu z wartością przechowywaną w słowniku zasobów, lub wartość właściwości statycznej klasy, które zostały utworzone lub właściwość typu `Color` innego elementu na stronie lub utworzone z Oddzielanie wartości odcień, nasycenie i jasność.

Wszystkie te opcje są możliwe przy użyciu rozszerzenia znaczników XAML. Ale nie pozwól frazę "rozszerzenia znaczników" przestraszyć możesz: rozszerzenia znaczników XAML są *nie* rozszerzeń XML. Nawet w przypadku rozszerzenia znaczników dla XAML XAML jest zawsze prawne XML. 

Rozszerzenie znaczników jest naprawdę inny sposób Express atrybut elementu. Zazwyczaj do zidentyfikowania przez ustawienia atrybutów ujętych w nawiasy klamrowe są rozszerzenia znaczników XAML:

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

Wszystkie ustawienia atrybutów w nawiasach klamrowych jest *zawsze* rozszerzenie znaczników w XAML. Jednakże jak można zauważyć, rozszerzenia znaczników XAML mogą się też odwoływać bez użycia nawiasów klamrowych.

W tym artykule jest podzielony na dwie części:

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[Korzystanie z rozszerzeń struktury znaczników XAML](consuming.md)  

Użyj rozszerzenia znaczników XAML, które są zdefiniowane w platformy Xamarin.Forms.

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[Tworzenie rozszerzeń struktury znaczników XAML](creating.md) 

Pisanie własnych niestandardowych rozszerzeń znaczników XAML.



## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia znaczników (przykład)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Rozdział rozszerzeń znaczników XAML z książki platformy Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Słowniki zasobów](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Style dynamiczne](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Powiązanie danych](~/xamarin-forms/app-fundamentals/data-binding/index.md)
