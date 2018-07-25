---
title: Etykiety w rozszerzeniu Xamarin.iOS
description: W tym dokumencie omówiono sposób używania etykiet w rozszerzeniu Xamarin.iOS. Go w tym artykule opisano sposób tworzenia etykiety programowo i za pomocą narzędzia iOS Designer.
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: b52bdbd41eaafbc5e6c78e1f8514b701fd78bd6b
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241916"
---
# <a name="labels-in-xamarinios"></a>Etykiety w rozszerzeniu Xamarin.iOS

`UILabel` Formant jest używany do wyświetlania pojedynczych i wielu linii, przeczytaj tylko tekst. 

## <a name="implementing-a-label"></a>Implementowanie etykietę

Nowa etykieta jest tworzona przez utworzenie wystąpienia [ `UILabel` ](https://developer.xamarin.com/api/type/UIKit.UILabel/):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Etykiety i scenorysów

Można również dodać etykietę do interfejsu użytkownika, korzystając z narzędzia iOS Designer. Wyszukaj **etykiety** w **przybornika** i przeciągnij go do widoku:

![Etykieta w przyborniku](labels-images/image3.png)

W okienku właściwości można dostosować następujące właściwości:

![Panel właściwości etykiety](labels-images/image2.png)

- **W kontekście tekstu** — zwykły lub opartego na atrybutach. Zwykły tekst pozwala na ustawienie [formatowanie atrybutów](#Formatting_Text_and_Label) na cały ciąg. Teksty opartego na atrybutach pozwala ustawić odpowiadający ustawieniom lokalnym różne znaki lub słowa w ciągu.
- **Kolor czcionki, wyrównania** — formatowanie atrybutów, które można stosować do etykiety.
- **Wiersze** — Ustawia liczbę wierszy, które mogą znajdować się na etykiecie. Ustaw tę wartość na 0, aby zezwolić na etykietę do wykorzystania jako liczbę wierszy, zgodnie z potrzebami.
- **Zachowanie** — można ustawić aktywny lub wyróżnione. Włączone jest ustawienie domyślne, wyłączonego tekstu pojawi się w jaśniejszego koloru szarego. Wyróżnione jest domyślnie wyłączona i zezwala na etykietę do narysowania wyróżniony stan, gdy zostanie wybrane przez użytkownika.
- **Baselane i podział wiersza** — 
    - Basline Określa, jak tekst będzie umieszczony Jeśli rozmiary czcionek jest inny niż ten, który został określony.
    - Podziały wierszy określają, jak opakowane lub obcięty, jeśli zawiera więcej niż jeden wiersz ciąg.
- **Pewnego** — Określa, jak czcionki o rozmiarze będzie można zminimalizować w etykiecie, jeśli to konieczne.
- **Wyróżnione w tle, przesunięcie** — pozwala ustawić kolor siatki i w tle i przesunięcia cienia.

## <a name="truncating-and-wrapping"></a>Obcinanie i opakowywanie

Informacje na temat korzystania z wiersza przerywa w systemie iOS, znaleźć [obciąć i zawijania tekstu](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text) przepisu.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>Formatowanie tekstu i etykiety

Można sformatować ciągu, którego używasz w etykiecie możesz obu zestawów atrybutów na cały ciąg formatowania, lub możesz używać parametrów opartego na atrybutach. W poniższych przykładach pokazano, jak zaimplementować te:

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

Aby uzyskać więcej informacji na temat korzystania z stylu tekstu `NSAttributedString` dotyczą [stylu tekstu](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/text_field/style_text) przepisu.

Domyślnie etykiety mają `Enabled` ustawioną na wartość true, ale jest to możliwe, aby ustawić go na wyłączone, aby przyznać użytkownikowi wskazówką wyłączenia niektórych kontrolki:

```csharp
label.Enabled = false;
```

Etykieta to ustawia jasny kolor szary, jak pokazano na poniższej przykładowej ilustracji ekranu ograniczenia w systemie iOS:

![Przycisk wyłączone w systemie iOS](labels-images/image1.png)

Można również ustawić kolory tekstu cieni i tekst etykiety dla dodatkowych efektów:

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Powoduje wyświetlenie tekstu następująco:

![Wyróżnij i Ustaw tekst w tle](labels-images/image4.png)

Aby uzyskać więcej informacji na temat Zmiana czcionki UILabel, dotyczą [Zmień czcionkę](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/change_the_font) przepisu.





