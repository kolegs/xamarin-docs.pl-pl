---
title: "watchOS interfejsu użytkownika"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: f7a7b565372970cc487b664ed7415cf876e290b6
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="watchos-user-interface"></a>watchOS interfejsu użytkownika

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) przykładzie pokazano różnych watchOS formantów. Scenorysu aplikacji w tym polu jest wyświetlana (kliknij, aby powiększyć):

[![](images/storyboard-sml.png "Przykładowy układ watchOS")](images/storyboard.png#lightbox)

Programowe nazwy wszystkich kontrolek jest prefiksem `WKInterface` (np.) `WKInterfaceLabel`, `WKInterfaceButton`).

|Formant|Opis|Zrzut ekranu|
|---|---|---|
|Etykieta|Użyj `SetText` i inne właściwości, aby sterować wyglądem tekstu formantu etykiety. `NSAttributedString` jest również obsługiwany.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Przycisk|Tworzenie i ustawianie właściwości w scenorysu. CTRL + przeciągnij, aby dodać `Action` do zaimplementowania obsługę, gdy zostanie kliknięty.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Przełącznik|Użyj `SetOn` kontrolować stan przełącznika.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Suwak|Możliwe jest wiele różnych stylów.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Obraz|Użyj `myImage.SetImage("MyWatchImage")` do ładowania obrazów na oglądanie, lub `WKInterfaceDevice.CurrentDevice.AddCachedImage` do buforowania ich do wielokrotnego użytku w czujki.<br />[Dokumentacja formantu obrazu](~/ios/watchos/user-interface/image.md)<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Separator|Użyj separatorów w celu tworzenia atrakcyjne Obejrzyj UI.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|mapy|Obraz mapy statycznie jest wyświetlany na czujki można jednak sterować wiele aspektów jego wygląd, w tym dodawanie numerów PIN.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Movie & InlineMove|Filmy można albo otwórz samodzielnie lub wewnętrznej<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Grupa|Użycie grupy w celu tworzenia atrakcyjne Obejrzyj UI.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|tabela|Uproszczone tabel w systemie iOS. Implementowanie `DidSelectRow` odpowiadanie na wybór użytkownika (lub użyć segue).<br />[Tabela dokumentacji Control](~/ios/watchos/user-interface/table.md)<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Urządzenie|`WKInterfaceDevice.CurrentDevice` zawiera właściwości, takie jak `ScreenBounds`, `ScreenScale`, i `PreferredContentSizeCategory`.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menu](~/ios/watchos/user-interface/menu.md)|Definiowanie menu życie naciśnij w scenorysu i wykonuje działania dla każdego przycisku w kodzie.<br />[Menu dokumentacji kontrolki (Wymuszaj Touch)](~/ios/watchos/user-interface/menu.md)<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Wprowadzanie tekstu|Użyj `PresentTextInputController` i `WKTextInputMode` wyliczenia.<br />[Tekst wejściowy dokumentacji](~/ios/watchos/user-interface/text-input.md)<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Wierzchołek cyfrowych|Cyfrowe wierzchołek może służyć do dysków selektora lub jego obrotu można śledzić w kodzie.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Gestów|Istnieją cztery typy rozpoznawania gestów, które można dodać do sceny: Naciśnij, przejdź przesuwanie i LongPress.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## <a name="related-links"></a>Linki pokrewne

- [WatchKitCatalog (przykład)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Dokumentacja interfejsu API zestawu czujki](https://developer.xamarin.com/api/namespace/WatchKit/)
