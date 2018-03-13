---
title: "watchOS interfejsu użytkownika"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: 0717b8484c6094bb1d9589c44df37745d9e21900
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="watchos-user-interface"></a>watchOS interfejsu użytkownika

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) przykładzie pokazano różnych watchOS formantów. Scenorysu aplikacji w tym polu jest wyświetlana (kliknij, aby powiększyć):

[![](images/storyboard-sml.png "Przykładowy układ watchOS")](images/storyboard.png#lightbox)

Programowe nazwy wszystkich kontrolek jest prefiksem `WKInterface` (np.) `WKInterfaceLabel`, `WKInterfaceButton`).


<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
        <strong>Formant</strong>
      </th>
      <th>
        <strong>Opis elementu</strong>
      </th>
      <th>
        <strong>Zrzut ekranu</strong>
      </th>
    </thead>
    <tbody>
    <tr>
      <td valign="top">
Etykieta </td>
      <td valign="top">
Użyj <code>SetText</code> i inne właściwości, aby sterować wyglądem tekstu formantu etykiety. <code>NSAttributedString</code> jest również obsługiwany.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/label.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Przycisk </td>
      <td valign="top">
Tworzenie i ustawianie właściwości w scenorysu. <kbd>CTRL + przeciągnij</kbd> można dodać <code>Action</code> do zaimplementowania obsługę, gdy zostanie kliknięty.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/button.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Przełącznik </td>
      <td valign="top">
Użyj <code>SetOn</code> kontrolować stan przełącznika.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/switch.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Suwak </td>
      <td valign="top">
Możliwe jest wiele różnych stylów.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/slider.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Obraz </td>
      <td valign="top">
Użyj <code>myImage.SetImage("MyWatchImage")</code> do ładowania obrazów na oglądanie, lub <code>WKInterfaceDevice.CurrentDevice.AddCachedImage</code> do buforowania ich do wielokrotnego użytku w czujki.
        <br />
        <a href="~/ios/watchos/user-interface/image.md">Dokumentacja formantu obrazu</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/image.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Separator </td>
      <td valign="top">
Użyj separatorów w celu tworzenia atrakcyjne Obejrzyj UI.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/separator.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
mapy </td>
      <td valign="top">
Obraz mapy statycznie jest wyświetlany na czujki można jednak sterować wiele aspektów jego wygląd, w tym dodawanie numerów PIN.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/map.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Movie & InlineMove </td>
      <td valign="top">
Filmy można albo otwórz samodzielnie lub wewnętrznej <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/movie.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Grupa </td>
      <td valign="top">
Użycie grupy w celu tworzenia atrakcyjne Obejrzyj UI.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/group.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
tabela </td>
      <td valign="top">
Uproszczone tabel w systemie iOS.
Implementowanie <code>DidSelectRow</code> odpowiadanie na wybór użytkownika (lub użyć segue).
        <br />
        <a href="~/ios/watchos/user-interface/table.md">Tabela dokumentacji Control</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TableDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/table.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Urządzenie </td>
      <td valign="top">
        <code>WKInterfaceDevice.CurrentDevice</code> zawiera właściwości, takie jak <code>ScreenBounds</code>, <code>ScreenScale</code>, i <code>PreferredContentSizeCategory</code>.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/device.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="~/ios/watchos/user-interface/menu.md">Menu</a>
      </td>
      <td valign="top">
Definiowanie menu życie naciśnij w scenorysu i wykonuje działania dla każdego przycisku w kodzie.
        <br />
        <a href="~/ios/watchos/user-interface/menu.md">Menu dokumentacji kontrolki (Wymuszaj Touch)</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/controller.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Wprowadzanie tekstu </td>
      <td valign="top">
Użyj <code>PresentTextInputController</code> i <code>WKTextInputMode</code> wyliczenia.
        <br />
        <a href="~/ios/watchos/user-interface/text-input.md">Tekst wejściowy dokumentacji</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/textinput.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Wierzchołek cyfrowych </td>
      <td valign="top">
Cyfrowe wierzchołek może służyć do dysków selektora lub jego obrotu można śledzić w kodzie.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/digital-crown.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Gestów </td>
      <td valign="top">
Istnieją cztery typy rozpoznawania gestów, które można dodać do sceny: Naciśnij, przejdź przesuwanie i LongPress.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/gestures.png" class="tableimg">
      </td>
    </tr>
    </tbody>
</table>



## <a name="related-links"></a>Linki pokrewne

- [WatchKitCatalog (przykład)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Dokumentacja interfejsu API zestawu czujki](https://developer.xamarin.com/api/namespace/WatchKit/)
