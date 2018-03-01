---
title: "Wyświetlanie alertów"
ms.topic: article
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 09f4178d5d6e12388771fa63875fbe1f489c959a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="displaying-alerts"></a>Wyświetlanie alertów

Począwszy od systemu iOS 8, UIAlertController ma UIActionSheet zastąpionego ukończone i UIAlertView zarówno których obecnie są przestarzałe.

W odróżnieniu od klasy zastąpione będące podklasami UIView UIAlertController jest podklasą klasy UIViewController.

Użyj `UIAlertControllerStyle` wskazująca typ alertu do wyświetlenia. Te typy alertów są:

- **UIAlertControllerStyleActionSheet**
    * Wstępne iOS 8 to byłby UIActionSheet
- **UIAlertControllerStyleAlert**
    * Wstępne iOS 8 to byłby UIAlertView 

Istnieją trzy kroki niezbędne do wykonania podczas tworzenia kontrolera alertu:

- Tworzenie i konfigurowanie alertu przez:
    * Tytuł
    * — komunikat
    * preferredStyle
    
- (Opcjonalnie) Dodaj pole tekstowe
- Dodaj wymagane akcje
- Przedstawia kontroler widoku

Najprostsza alertu zawiera jeden przycisk opisane w tym zrzut ekranu:

 ![Alert dotyczący z jednego przycisku](alerts-images/alert1.png)

Kod, aby wyświetlić alert proste wygląda następująco:

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

Wyświetlanie alertu wiele opcji dotyczących odbywa się w podobny sposób, ale dodaje dwie akcje. Na przykład poniższy zrzut ekranu przedstawia alertu o dwa przyciski:

 ![ Alert dotyczący z dwóch przycisków](alerts-images/alert2.png)

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

Alerty można także wyświetlić arkusza akcji, podobnie jak na poniższym zrzucie ekranu:

 ![Alert arkusza akcji](alerts-images/alert3.png)

Przyciski są dodawane do alertu o `AddAction` metody:

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
- [Kontroler alertu](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
