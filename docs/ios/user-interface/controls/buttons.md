---
title: Przyciski
description: Klasa UIButton jest używana do reprezentowania różnych inny styl przycisku na ekranach systemu iOS. W tej sekcji przedstawiono różne opcje do pracy z przycisków w systemie iOS.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c2c33103c005a5ed567b1c4703846f887d824ac4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="buttons"></a>Przyciski

_Klasa UIButton jest używana do reprezentowania różnych inny styl przycisku na ekranach systemu iOS. W tej sekcji przedstawiono różne opcje do pracy z przycisków w systemie iOS._

`UIButton`Klasa reprezentuje kontrolkę przycisku w systemie iOS. 

Przycisk Właściwości można edytować w `Properties Pad` projektanta iOS:


![](buttons-images/properties.png "Konsola właściwości projektanta dla systemu iOS")

## <a name="creating-a-button"></a>Utworzenie przycisku

UIButton mogą być tworzone w za pomocą tylko kilka wierszy kodu.

Najpierw utwórz wystąpienie przycisk Nowy i określić typ przycisku, które są potrzebne:

```csharp
UIButton myButton = new UIButton(UIButtonType.System);
```

UIButtonType powinny być określone jako jedną z następujących czynności:

- **System** — jest to standardowy typ używane przez system iOS, typu, która będzie używana najczęściej.
- **DetailDisclosure** — przedstawia "Zmniejsz" typ przycisku używane do ukrywania lub pokazywania szczegółowych informacji.
- **InfoDark** -ciemnego szczegółowe informacje "i" wyświetlany przycisk w okręgu.
- **InfoLight** -światło szczegółowe informacje "i" wyświetlany przycisk w okręgu.
- **AddContact** — przycisk wyświetlany jako przycisk Dodaj kontakt.
- **Niestandardowe** — umożliwia dostosowanie kilka cech przycisku.

Więcej informacji na temat typów przycisk znajdują się w [przycisku typy](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/create_different_types_of_buttons/) przepisu.

Następnie określ na ekranie rozmiar i położenie przycisku. Przykład:

```csharp
myButton.Frame = new CGRect (25, 25, 300, 150);
```

Aby zmienić tekst przycisku, użyj `SetTitle` właściwości na przycisku, należy ustawić ciąg tekstu i `UIControlStyle`. Na przykład:

```csharp
myButton.SetTitle("Hello, World!", UIControlState.Normal);
```

Ustawianie właściwości różne dla każdego stanu umożliwia użytkownikowi komunikowanie się więcej informacji na temat użytkownika (np.) kolor tekstu szarego stanu wyłączone). Można przełączać się między każdy stan za pomocą projektanta dla systemu iOS, lub możesz zrobić to programowo. Aby uzyskać więcej informacji na tekst przycisku Ustawienia i stan dotyczą [ustawić tekst przycisku](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/set_button_text/) przepisu.

## <a name="dealing-with-user-interactions"></a>Zajmujących się interakcji użytkowników


Przyciski nie są bardzo przydatne, chyba że ich czymś po kliknięciu! 

W systemie iOS zdarzenia na przyciskach są prawie zawsze zdarzenia touch, zgodnie z użyciem współdziała z przycisku na ekranie ich przez dotknięcie go. Lista wszystkich możliwych zdarzeń kontrolne znajduje się [tutaj](https://developer.apple.com/documentation/uikit/uicontrolevents), ale najczęściej używane zdarzenie w systemie iOS jest `TouchUpInside`. Następnie możesz utworzyć program obsługi zdarzeń po naciśnięciu przycisku coś zrobić:


```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

### <a name="adding-events-in-the-ios-designer"></a>Dodawanie zdarzeń w systemie iOS projektanta
 
Karta zdarzeń w konsoli właściwości umożliwia dodawanie zdarzeń do formantów.

Wybierz zdarzenie, a następnie wpisz nazwę nowego obsługi zdarzeń lub wybierz jedną z listy. W ten sposób spowoduje utworzenie nowej metody częściowej klasy kontrolera widoku.

![Na karcie zdarzenia](buttons-images/image1.png)

## <a name="styling-a-button"></a>Style przycisku

UIButtons są inne niż większość UIKit kontrolki, w tym mają stan, więc nie można zmienić tylko po prostu tytuł, należy je zmienić w każdej `UIControlState`. Ustawianie koloru tytuł i kolor cienia odbywa się w podobny sposób:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Ponadto można użyć oparte na atrybutach tekstu jako tytuł przycisku. Na przykład:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Przycisk niestandardowych typów


Podczas ustawiania `Custom` typ przycisku, obiekt nie ma żadnych odwzorowanie domyślne. Ustawianie obrazów dla różnych stanów można skonfigurować wygląd przycisku. Na przykład poniższy kod przedstawia sposób dodawania różnych obrazy dla `Normal`, `Highlighted` i `Selected` stany:


```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```


W zależności od tego, czy użytkownik jest dotknięcie przycisku lub nie, będzie renderowane jako jeden z następujących obrazów (`Normal`, `Highlighted` i `Selected` stany odpowiednio):


![](buttons-images/image22.png "Stan UIButton normalny")
![](buttons-images/image23.png "stanu UIButton wyróżnione")
![](buttons-images/image24.png "wybrany stan UIButton")

Aby uzyskać więcej informacji na temat pracy z przycisków niestandardowych dotyczą [użyć obrazu dla przycisku](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/use_an_image_for_a_button/).


## <a name="related-links"></a>Linki pokrewne

- [UIButton skoroszytu](https://developer.xamarin.com/workbooks/ios/user-interface/UIbutton/uibutton.workbook)
