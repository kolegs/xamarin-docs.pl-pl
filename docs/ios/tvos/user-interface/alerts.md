---
title: Praca z alertami systemu tvOS w Xamarin
description: Ten dokument zawiera opis sposobu pracy z alertami systemu tvOS w Xamarin. Tym artykule omówiono wyświetlanie alertu, dodawanie pól tekstowych i Klasa pomocy.
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: b5125f150a4d57ed27041da2944f4c161434cf93
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789086"
---
# <a name="working-with-tvos-alerts-in-xamarin"></a>Praca z alertami systemu tvOS w Xamarin

_Ten artykuł dotyczy pracy z UIAlertController, aby wyświetlić komunikat ostrzegawczy w Xamarin.tvOS do użytkownika._

Jeśli trzeba uzyskać uwagi użytkownika systemu tvOS lub poproś uprawnienia do wykonania destrukcyjnego akcji (np. usunięcie pliku), może ona za pomocą ostrzeżenie `UIAlertViewController`:

[![](alerts-images/alert01.png "Przykład UIAlertViewController")](alerts-images/alert01.png#lightbox)

Jeśli oprócz wyświetlania komunikatu, możesz dodać przycisków i pola tekstowego do alertu, aby zezwolić użytkownikowi na odpowiadanie na akcje i wyrazić swoją opinię.

<a name="About-Alerts" />

## <a name="about-alerts"></a>Alerty — informacje

Jak już wspomniano, alerty służą do pobrania uwagi użytkownika i poinformować o stanie aplikacji lub żądanie opinii użytkowników. Alerty musi przedstawić tytuł, można opcjonalnie stosować wiadomości i jeden lub więcej przycisków lub pól tekstowych.

[![](alerts-images/alert04.png "Przykład alertu")](alerts-images/alert04.png#lightbox)

Apple ma poniższe sugestie dotyczące pracy z alertami:

- **Alerty efektów** — alerty i zakłócić jego przepływ użytkownika z aplikacją przerwań środowisko użytkownika i tak, należy używać tylko ważne sytuacjach, takich jak powiadomienia o błędach, zakupy w aplikacji i destrukcyjne działania. 
- **Umożliwia wybór przydatne** — Jeśli Alert wyświetlane opcje dla użytkownika, Twoje powinien upewnić, że poszczególnych opcji oferuje krytyczne informacje i zawiera przydatne akcji dla użytkownika.

<a name="Alert-Titles-and-Messages" />

### <a name="alert-titles-and-messages"></a>Tytuły alertów i komunikatów

Apple ma poniższe uwagi prezentująca tytuł i opcjonalnie komunikat alertu:

- **Użyj tytułów Multiword** — jego tytuł alertu należy uzyskać punktu sytuacji między wyraźnie zachowując nadal proste. Tytuł pojedynczego wyrazu rzadko zapewnia wystarczającą ilość informacji.
- **Użyj opisową tytułów, które nie wymagają komunikat** — wszędzie tam, gdzie to możliwe, należy rozważyć zmianę tytuł alertu opisową wystarczającej ilości opcjonalne tekst komunikatu nie jest wymagane. 
- **Wprowadź komunikat krótki pełnym zdaniem** — Jeśli opcjonalny komunikat jest wymagane, aby uzyskać punkt alertu w, schowaj, wystarczy i zapewnić ich pełnym zdaniem z właściwego wielkość liter i znaków interpunkcyjnych.

<a name="Alert-Buttons" />

### <a name="alert-buttons"></a>Przyciski alertu

Apple ma następujące propozycję dodawanie przycisków do alertu:

- **Ograniczenie do dwóch przycisków** — wszędzie tam, gdzie to możliwe, ograniczyć Alert, aby maksymalnie dwóch przycisków. Pojedynczy alerty przycisk udostępniają informacje, ale żadne akcje. Dwa alerty przycisk zapewniają prosty wybór tak/nie akcji.
- **Użyj Succinct, logicznych tytułów przycisk** — prosty najlepiej jednej do dwóch word przycisk tytułów wyraźnie opisujące akcję przycisku. Aby uzyskać więcej informacji, zobacz nasze [Praca z przycisków](~/ios/tvos/user-interface/buttons.md) dokumentacji.
- **Wyraźnie przycisków destrukcyjnego znacznik** — w przypadku przycisków, których wykonania destrukcyjnego akcji (np. usunięcie pliku) wyraźnie oznacz je za pomocą `UIAlertActionStyle.Destructive` stylu.

<a name="Displaying-an-Alert" />

## <a name="displaying-an-alert"></a>Wyświetlanie alertu

Aby wyświetlić alert, należy utworzyć wystąpienie `UIAlertViewController` i skonfigurować go przez dodanie akcje (przyciski) i wybierając styl alertu. Na przykład poniższy kod wyświetla alert OK i Anuluj:

```csharp
const string title = "A Short Title is Best";
const string message = "A message should be a short, complete sentence.";
const string acceptButtonTitle = "OK";
const string cancelButtonTitle = "Cancel";
const string deleteButtonTitle = "Delete";
...
        
var alertController = UIAlertController.Create (title, message, UIAlertControllerStyle.Alert);

// Create the action.
var acceptAction = UIAlertAction.Create (acceptButtonTitle, UIAlertActionStyle.Default, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

var cancelAction = UIAlertAction.Create (cancelButtonTitle, UIAlertActionStyle.Cancel, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

// Add the actions.
alertController.AddAction (acceptAction);
alertController.AddAction (cancelAction);
PresentViewController (alertController, true, null);
```

Spójrzmy na ten kod szczegółowo. Najpierw utworzymy nowy Alert z danym tytuł i komunikat:

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

Następnie dla każdego przycisku, która ma być wyświetlane w alercie, utworzymy Definiowanie tytuł przycisku, jego styl i akcji, którą chcemy udostępnić w sytuacji, gdy przycisk jest naciśnięty akcji:

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ => 
    // Do something when the button is pressed
    ...
);
```

`UIAlertActionStyle` Wyliczenia umożliwiają skonfigurowanie styl przycisku jako jedną z następujących czynności:

- **Domyślne** — przycisk zostanie wybrany, gdy alert jest wyświetlany przycisk domyślny.
- **Anuluj** — przycisk to przycisk Anuluj, aby alert.
- **Destrukcyjnego** -wyróżnia przycisku destrukcyjnego akcji, takich jak usuwanie pliku. Obecnie systemu tvOS renderuje destrukcyjnego przycisk czerwonym tle.

`AddAction` Metoda dodaje danej akcji do `UIAlertViewController` i w końcu `PresentViewController (alertController, true, null)` metoda wyświetla dany alert dla użytkownika.

<a name="Adding-Text-Fields" />

## <a name="adding-text-fields"></a>Dodawanie pól tekstowych

Oprócz dodania akcje (przyciski) do alertu, można dodać pola tekstowe do alertu, aby zezwolić użytkownikowi na Wypełnij informacje, takie jak nazwy użytkowników i hasła:

[![](alerts-images/alert02.png "Pole tekst w alercie")](alerts-images/alert02.png#lightbox)

Jeśli użytkownik wybierze pola tekstowego, zostanie wyświetlony klawiatury standardowa systemu tvOS, umożliwiając wprowadź wartość w polu:

[![](alerts-images/alert03.png "Wprowadzanie tekstu")](alerts-images/alert03.png#lightbox)

Poniższy kod przedstawia alertu OK i Anuluj z jednym polem tekstowym wprowadzenie wartości:

```csharp
UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
UITextField field = null;

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;

    // Initialize field
    field.Placeholder = placeholder;
    field.Text = text;
    field.AutocorrectionType = UITextAutocorrectionType.No;
    field.KeyboardType = UIKeyboardType.Default;
    field.ReturnKeyType = UIReturnKeyType.Done;
    field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

});

// Add cancel button
alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
    // User canceled, do something
    ...
}));

// Add ok button
alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
    // User selected ok, do something
    ...
}));

// Display the alert
controller.PresentViewController(alert,true,null);
```

`AddTextField` Metoda dodaje nowe pole tekstowe do alertu, który następnie można skonfigurować przez ustawienie właściwości, takie jak symbolu zastępczego, tekst (tekst, który jest wyświetlany, gdy to pole jest puste), wartość domyślna tekst i typ klawiatury. Na przykład:

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

Tak, aby firma Microsoft może działać na wartości pola tekstowego później, firma Microsoft są także zapisanie kopii przy użyciu następującego kodu:

```csharp
UITextField field = null;
...

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;
    ...
});
```

Kiedy użytkownik wprowadzi wartość w polu tekstowym, możemy użyć `field` zmiennej można uzyskać dostępu do tej wartości.

<a name="Alert-View-Controller-Helper-Class" />

## <a name="alert-view-controller-helper-class"></a>Klasa pomocnika kontrolera widoku alertów

Ponieważ wyświetlanie prostego, wspólnych typów alertów za pomocą `UIAlertViewController` może spowodować dość nieco zduplikowany kod, można zmniejszyć ilość kodu powtarzających się klasę pomocy. Na przykład:

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;

namespace UIKit
{
    /// <summary>
    /// Alert view controller is a reusable helper class that makes working with <c>UIAlertViewController</c> alerts
    /// easier in a tvOS app.
    /// </summary>
    public class AlertViewController
    {
        #region Static Methods
        public static UIAlertController PresentOKAlert(string title, string description, UIViewController controller) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Configure the alert
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(action) => {}));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentOKCancelAlert(string title, string description, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentDestructiveAlert(string title, string description, string destructiveAction, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create(destructiveAction,UIAlertActionStyle.Destructive,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentTextInputAlert(string title, string description, string placeholder, string text, UIViewController controller, AlertTextInputDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
            UITextField field = null;

            // Add and configure text field
            alert.AddTextField ((textField) => {
                // Save the field
                field = textField;

                // Initialize field
                field.Placeholder = placeholder;
                field.Text = text;
                field.AutocorrectionType = UITextAutocorrectionType.No;
                field.KeyboardType = UIKeyboardType.Default;
                field.ReturnKeyType = UIReturnKeyType.Done;
                field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

            });

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false,"");
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null && field !=null) {
                    action(true, field.Text);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }
        #endregion

        #region Delegates
        public delegate void AlertOKCancelDelegate(bool OK);
        public delegate void AlertTextInputDelegate(bool OK, string text);
        #endregion
    }
}
```

Za pomocą tej klasy, wyświetlanie i reagowanie na alerty prosty może odbywać się w następujący sposób:

```csharp
#region Custom Actions
partial void DisplayDestructiveAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentDestructiveAlert("A Short Title is Best","The message should be a short, complete sentence.","Delete",this, (ok) => {
        Console.WriteLine("Destructive Alert: The user selected {0}",ok);
    });
}

partial void DisplayOkCancelAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKCancelAlert("A Short Title is Best","The message should be a short, complete sentence.",this, (ok) => {
        Console.WriteLine("OK/Cancel Alert: The user selected {0}",ok);
    });
}

partial void DisplaySimpleAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKAlert("A Short Title is Best","The message should be a short, complete sentence.",this);
}

partial void DisplayTextInputAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentTextInputAlert("A Short Title is Best","The message should be a short, complete sentence.","placeholder", "", this, (ok, text) => {
        Console.WriteLine("Text Input Alert: The user selected {0} and entered `{1}`",ok,text);
    });
}
#endregion
```


<a name="Summary" />

## <a name="summary"></a>Podsumowanie

Ten artykuł zawiera pasuje do pracy z `UIAlertController` do wyświetlania użytkownikowi w Xamarin.tvOS komunikat ostrzegawczy. Najpierw należy go pokazano, jak wyświetlić alert proste i dodawanie przycisków. Następnie go pokazano, jak dodać pola tekstowe do alertu. Na koniec go pokazano, jak klasa pomocy umożliwia zmniejszenie ilości powtarzających się kod wymagany do wyświetlenia alertu.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
