---
title: Rozszerzenia znaczników w XAML
description: Artykule wyjaśniono, jak rozszerzyć możliwości i elastyczność XAML, umożliwiając atrybutów elementów, należy ustawić ze źródeł innych niż tekst dosłowny ciągów za pomocą rozszerzeń struktury znaczników XAML zestawu narzędzi Xamarin.Forms.
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: d507ff3c74de6bb4ea36c1a7b7dc2cd5dd60823b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996743"
---
# <a name="xaml-markup-extensions"></a>Rozszerzenia znaczników w XAML

Rozszerzeń struktury znaczników XAML pomóc rozszerzyć możliwości i elastyczność XAML, umożliwiając atrybutów elementów, należy ustawić ze źródeł, inne niż ciągi znaków literału.

Na przykład, zazwyczaj należy ustawić `Color` właściwość `BoxView` następująco:

```xaml
<BoxView Color="Blue" />
```

Lub jest on ustawiany na wartość szesnastkową kolor RGB:

```xaml
<BoxView Color="#FF0080" />
```

W obu przypadkach ciąg tekstowy jest ustawiona na `Color` atrybutu jest konwertowany na `Color` wartość przez [ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter) klasy.

Warto zamiast tego ustawić `Color` atrybutu z wartością przechowywane w słowniku zasobów, lub wartość właściwość statyczna klasy, który został utworzony lub właściwość typu `Color` innego elementu na stronie lub skonstruowany z Oddziel wartości odcień, nasycenie i jasność.

Wszystkie te opcje są możliwe przy użyciu rozszerzeń struktury znaczników XAML. Ale nie przegap frazę "rozszerzenia znaczników" przestraszyć możesz: rozszerzeń struktury znaczników XAML są *nie* rozszerzeń XML. Nawet w przypadku rozszerzeń struktury znaczników XAML XAML jest zawsze prawne XML.

Rozszerzenie znaczników jest naprawdę inny sposób określenie atrybutu elementu. XAML — rozszerzenia znaczników są zazwyczaj identyfikowane przez ustawienie atrybutu, który jest ujęty w nawiasy klamrowe:

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

Każde ustawienie atrybutów w nawiasy klamrowe są *zawsze* rozszerzenie znaczników XAML. Jednak jak zobaczysz, rozszerzeń struktury znaczników XAML można także odwoływać się bez użycia nawiasów klamrowych.

W tym artykule jest podzielona na dwie części:

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[Korzystanie z rozszerzeń struktury znaczników XAML](consuming.md)  

Użyj rozszerzenia znaczników XAML, które są zdefiniowane w interfejsie Xamarin.Forms.

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[Tworzenie rozszerzeń struktury znaczników XAML](creating.md)

Pisz własne niestandardowe rozszerzenia znaczników XAML.



## <a name="related-links"></a>Linki pokrewne

- [Rozszerzenia znaczników (przykład)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [XAML znaczników rozszerzenia rozdziału z książki zestawu narzędzi Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Słowniki zasobów](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Style dynamiczne](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Powiązanie danych](~/xamarin-forms/app-fundamentals/data-binding/index.md)
