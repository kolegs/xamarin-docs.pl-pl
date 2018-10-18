---
title: Przyciski w rozszerzeniu Xamarin.iOS
description: Klasa obiektu klasy UIButton jest używana do reprezentowania różnych różne style przycisku na ekranach z systemem iOS. Ten przewodnik przedstawia różne opcje dotyczące pracy z przycisków w systemie iOS.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2018
ms.openlocfilehash: 35fc743944c04dd1fdb8e035ba94ad6aeb6156ea
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2018
ms.locfileid: "38986007"
---
# <a name="buttons-in-xamarinios"></a>Przyciski w rozszerzeniu Xamarin.iOS

W systemie iOS `UIButton` klasa reprezentuje kontrolkę przycisku.

Właściwości przycisku można modyfikować programowo lub za pomocą **konsoli właściwości** systemu IOS Designer:

![Konsolę właściwości w narzędziu iOS Designer](buttons-images/properties.png "konsoli właściwości z narzędzia iOS Designer")

## <a name="creating-a-button-programmatically"></a>Programowe tworzenie przycisku

Element `UIButton` można utworzyć za pomocą tylko kilku wierszy kodu.

- Utwórz wystąpienie przycisku i określić jej typ:

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  Typ przycisku jest określony przez `UIButtonType`:

  - `UIButtonType.System` -Przycisku ogólnego przeznaczenia
  - `UIButtonType.DetailDisclosure` -Wskazuje dostępność szczegółowe informacje, zwykle o określonym elemencie w tabeli
  - `UIButtonType.InfoDark` -Wskazuje dostępność informacji o konfiguracji; kolorowe ciemny
  - `UIButtonType.InfoLight` -Wskazuje dostępność informacji o konfiguracji; jasnego
  - `UIButtonType..AddContact` — Wskazuje, że można dodać kontakt
  - `UIButtonType.Custom` — Przycisk możliwe do dostosowania

  Aby uzyskać więcej informacji na temat typów inny przycisk Przyjrzyj się:
  
  - [Niestandardowe typy przycisku](#custom-button-types) części tego dokumentu
  - [Przycisk typy](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons) przepisu
  - Firmy Apple [iOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/).

- Zdefiniuj, rozmiar i położenie przycisku:

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- Ustaw tekst na przycisku. Użyj `SetTitle` metody, która wymaga tekst i `UIControlState` wartość:

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  Aby uzyskać więcej informacji na temat ustawiania stylu przycisku i ustawienie jego tekstu, zobacz:

  - [Style przycisku](#styling-a-button) części tego dokumentu
  - [Ustaw tekst przycisku](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text) przepisu.

## <a name="handling-a-button-tap"></a>Obsługa naciśnięcie przycisku

Aby reagować na naciśnięcie przycisku, należy podać program obsługi dla przycisku `TouchUpInside` zdarzeń:

```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside` nie jest zdarzeniem dostępna, tylko przycisk. `UIButton` klasy podrzędnej jest `UIControl`, która definiuje [wiele różnych zdarzeń](https://developer.xamarin.com/api/type/UIKit.UIControlEvent/).

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>Aby określić obsługę zdarzenia przycisku przy użyciu narzędzia iOS Designer

Użyj **zdarzenia** karcie **konsoli właściwości** do określenia procedury obsługi zdarzeń dla przycisku różnych zdarzeń.

Odpowiedniego zdarzenia wpisz nazwę nowego programu obsługi zdarzeń lub wybierz ją z listy. W ten sposób utworzyć program obsługi zdarzeń w kodzie dla kontrolera widoku przycisku.

![Karta zdarzeń w konsoli właściwości](buttons-images/image1.png "karty zdarzenia w konsoli właściwości")

## <a name="styling-a-button"></a>Style przycisku

`UIButton` Formanty może istnieć wiele różnych stanów, każdego określonego przez `UIControlState` wartość — `Normal`, `Disabled`, `Focused`, `Highlighted`itp. Każdy stan można podać unikatowe stylu określone programowo lub za pomocą narzędzia iOS Designer.

> [!NOTE]
> Aby uzyskać pełną listę wszystkich `UIControlState` wartości, zapoznaj się z [`UIKit.UIControlState enumeration`](https://developer.xamarin.com/api/type/UIKit.UIControlState/)
> Dokumentacja.

Na przykład, aby ustawić kolor tytułu i kolor cienia `UIControlState.Normal`:

```csharp
button.SetTitleColor(UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Poniższy kod ustawia tytuł przycisk opartego na atrybutach ciąg (stylizowane) w celu `UIControlState.Normal` i `UIControlState.Highlighted`:

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Typy przycisków niestandardowych

Przyciski `UIButtonType` z `Custom` mieć nie domyślnych stylów. Istnieje możliwość skonfigurowania wyglądu przycisku przez ustawienie obrazu dla swoich różnych stanów:

```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

W zależności od tego, czy użytkownik jest dotknięcie przycisku, czy nie, będą renderowane jako jeden z następujących obrazów (`UIControlState.Normal`, `UIControlState.Highlighted` i `UIControlState.Selected` stany odpowiednio):

![UIControlState.Normal](buttons-images/image22.png "UIControlState.Normal")
![UIControlState.Highlighted](buttons-images/image23.png "UIControlState.Highlighted") 
 ![UIControlState.Selected](buttons-images/image24.png "UIControlState.Selected")

Aby uzyskać więcej informacji na temat pracy z przyciski niestandardowe dotyczą [użyć obrazu dla przycisku](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) przepisu.

