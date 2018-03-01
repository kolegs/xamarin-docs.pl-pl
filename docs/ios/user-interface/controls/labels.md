---
title: Etykiety
ms.topic: article
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 695d02c5fa0477053cd95d73e1b738332d14f0f9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="labels"></a>Etykiety

`UILabel` Formant jest używany do wyświetlania jednego oraz wiele wierszy, odczytać tylko tekst. W tym artykule omówiono następujące tematy:

- [Implementowanie etykiety](#Implementing_a_Label)
- [Obcinanie i zawijania](#Truncating_and_Wrapping)
- [Formatowanie tekstu i etykiety](#Formatting_Text_and_Label)

## <a name="implementing-a-label"></a>Implementowanie etykiety

Przy uruchamianiu utworzono nową etykietę [ `UILabel` ](https://developer.xamarin.com/api/type/UIKit.UILabel/):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Etykiety i planów

Można również dodać etykietę do interfejsu użytkownika podczas korzystania z systemem iOS projektanta. Wyszukaj **etykiety** w **przybornika** i przeciągnij go do widoku:

![Etykiety w przyborniku](labels-images/image3.png)

W konsoli właściwości można dostosować następujące właściwości:

![Panel właściwości etykiety](labels-images/image2.png)

- **W kontekście tekstu** — zwykły lub oparte na atrybutach. Zwykły tekst pozwala ustawić [atrybuty formatowania](#Formatting_Text_and_Label) na cały ciąg. Oparte na atrybutach teksty umożliwia konfigurowanie formatów na różne znaki lub słowa w ciągu.
- **Wyrównanie kolorów i czcionek,** — atrybuty formatowania, który można zastosować do etykiety.
- **Wiersze** — Ustawia liczbę wierszy, które mogą znajdować się na etykiecie. Ustaw tę wartość na 0, aby umożliwić etykietę do używania dowolną liczbę wierszy.
- **Zachowanie** — może mieć wartości włączone lub wyróżnione. Jest włączone domyślnie ustawiany wyłączonego tekstu będzie wyświetlany w jaśniejszego koloru szarego. Wyróżnione jest domyślnie wyłączona i umożliwia etykiety narysowania ze stanem wyróżnione po wybraniu przez użytkownika.
- **Baselane i podział wiersza** — 
    - Basline Określa, jak będzie tekst umieszczony Jeśli rozmiary czcionek jest inny niż określony.
    - Podziały wierszy określają sposób opakowane lub obcięty, jeśli zawiera więcej niż jednym wierszu ciąg.
- **Autoshrink** — określa sposób czcionki o rozmiarze zostanie zminimalizowane wewnątrz etykiety, jeśli to konieczne.
- **Wyróżniony, cień, przesunięcie** — pozwala ustawić kolor siatki i w tle i przesunięcie cienia.

## <a name="truncating-and-wrapping"></a>Obcinanie i zawijania

Dla informacji o korzystaniu z wiersza dzieli w systemie iOS, zobacz [Truncate i Zawijaj tekst](https://developer.xamarin.com/recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text/) przepisu.

## <a name="formatting-text-and-label"></a>Formatowanie tekstu i etykiety

Formatującej ciąg, którego używasz w etykiecie można albo zestaw atrybutów na cały ciąg formatowania, lub można użyć ciągów oparte na atrybutach. Poniższe przykłady pokazują, jak wdrożyć te:

```csharp
label = new UILabel(){
                Text = "Hello, this is a string",
                Font = UIFont.FromName("Papyrus", 20f),
                TextColor = UIColor.Magenta,
                TextAlignment = UITextAlignment.Center
            };
```

```csharp
label.AttributedText = new NSAttributedString(
                "This is some formatted text",
                font: UIFont.FromName("GillSans", 16.0f),
                foregroundColor: UIColor.Blue,
                backgroundColor: UIColor.White
            );
```

Aby uzyskać więcej informacji na style tekstu przy użyciu `NSAttributedString` dotyczą [stylu tekstu](https://developer.xamarin.com/recipes/ios/standard_controls/text_field/style_text/) przepisu.

Domyślnie etykiety mają `Enabled` ustawioną na true, ale jest to możliwe, aby ustawić ją na wyłączone w celu przyznać użytkownikowi wskazówki, że dany formant jest wyłączone:

```csharp
label.Enabled = false;
```

Etykieta to ustawienie jasny kolor szary, jak pokazano na poniższej ilustracji przykład ekranu ograniczenia w systemie iOS:

![Przycisk wyłączone w systemie iOS](labels-images/image1.png)

Można również ustawić cieni i kolor tekstu etykiety dla dodatkowych efektów:

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Która wyświetla tekst następująco:

![Cieni i ustaw w tekście](labels-images/image4.png)

Aby uzyskać więcej informacji na temat zmieniania czcionki UILabel dotyczą [Zmień czcionkę](https://developer.xamarin.com/recipes/ios/standard_controls/labels/change_the_font/) przepisu.





