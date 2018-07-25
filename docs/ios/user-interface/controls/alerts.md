---
title: Wyświetlanie alertów w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano sposób wyświetlania alertów w rozszerzeniu Xamarin.iOS przy użyciu UIAlertController interfejsów API, wprowadzona w systemie iOS 8.
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 788e62b30dbf533df059b0c3805e04ecf7b857aa
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241343"
---
# <a name="displaying-alerts-in-xamarinios"></a>Wyświetlanie alertów w rozszerzeniu Xamarin.iOS

Począwszy od systemu iOS jest 8 UIAlertController ma ukończone UIActionSheet zastąpione i UIAlertView obu z nich są one przestarzałe.

W odróżnieniu od klas zastąpione będące podklasami UIView UIAlertController jest podklasą klasą UIViewController.

Użyj `UIAlertControllerStyle` aby wskazać typ alertu do wyświetlenia. Te typy alertów są:

- **UIAlertControllerStyleActionSheet**
    * Wstępne dla systemu iOS 8 to byłby UIActionSheet
- **UIAlertControllerStyleAlert**
    * Wstępne dla systemu iOS 8 to byłby UIAlertView 

Istnieją trzy kroki niezbędne do wykonania w przypadku tworzenia alertu kontrolera:

- Tworzenie i konfigurowanie alertu przez:
    * Tytuł
    * — komunikat
    * preferredStyle
    
- (Opcjonalnie) Dodaj pole tekstowe
- Dodaj wymagane akcje
- Przedstawia kontroler widoku

Najprostsza alertu zawiera jeden przycisk pokazany na tym zrzucie ekranu:

 ![Zgłoś alert, przy użyciu jednego przycisku](alerts-images/alert1.png)

Kod, aby wyświetlić alert proste jest następująca:

```csharp
okayButton.TouchUpInside += (sender, e) => {

    //Create Alert
    var okAlertController = UIAlertController.Create ("Title", "The message", UIAlertControllerStyle.Alert);

    //Add Action
    okAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));

    // Present Alert
    PresentViewController (okAlertController, true, null);
};
```

Wyświetlanie alertów z wieloma opcjami odbywa się w podobny sposób, ale Dodaj dwie akcje. Na przykład poniższy zrzut ekranu przedstawia alert w przypadku dwóch przycisków:

 ![ Alert o dwóch przycisków](alerts-images/alert2.png)

```csharp
okayCancelButton.TouchUpInside += ((sender, e) => {

    //Create Alert
    var okCancelAlertController = UIAlertController.Create("Alert Title", "Choose from two buttons", UIAlertControllerStyle.Alert);

    //Add Actions
    okCancelAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, alert => Console.WriteLine ("Okay was clicked")));
    okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine ("Cancel was clicked")));

    //Present Alert
    PresentViewController(okCancelAlertController, true, null);
});
```

Alerty można także wyświetlić arkusza akcji podobny do następującego zrzutu:

 ![Akcja arkusza alertu](alerts-images/alert3.png)

Przyciski są dodawane do alertu z `AddAction` metody:

```csharp
actionSheetButton.TouchUpInside += ((sender, e) => {

    // Create a new Alert Controller
    UIAlertController actionSheetAlert = UIAlertController.Create("Action Sheet", "Select an item from below", UIAlertControllerStyle.ActionSheet);

    // Add Actions
    actionSheetAlert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item One pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("custom button 1",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Two pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Three pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel, (action) => Console.WriteLine ("Cancel button pressed.")));

    // Required for iPad - You must specify a source for the Action Sheet since it is
    // displayed as a popover
    UIPopoverPresentationController presentationPopover = actionSheetAlert.PopoverPresentationController;
    if (presentationPopover!=null) {
        presentationPopover.SourceView = this.View;
        presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Up;
    }

    // Display the alert
    this.PresentViewController(actionSheetAlert,true,null);
});
```

## <a name="related-links"></a>Linki pokrewne

- [Formanty (przykład)](https://developer.xamarin.com/samples/Controls/)
- [Kontroler alertu](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
