---
title: "Wyświetlanie wyskakujących okienek"
description: "Platformy Xamarin.Forms zawiera dwa elementy interfejsu użytkownika podręcznego konta podobne — alert i arkusza akcji. W tym artykule przedstawiono przy użyciu alertów i akcji arkusz interfejsów API pytania użytkowników proste i prowadzą użytkowników przez zadania."
ms.topic: article
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 957db750b852b40daf1556e8dc8f7ba18e022dba
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="displaying-pop-ups"></a>Wyświetlanie wyskakujących okienek

_Platformy Xamarin.Forms zawiera dwa elementy interfejsu użytkownika podręcznego konta podobne — alert i arkusza akcji. W tym artykule przedstawiono przy użyciu alertów i akcji arkusz interfejsów API pytania użytkowników proste i prowadzą użytkowników przez zadania._

Wyświetlanie alertu lub proszenia użytkownika o dokonanie wyboru jest typowych zadań interfejsu użytkownika. Platformy Xamarin.Forms ma dwie metody na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) klasy do interakcji z użytkownikiem za pośrednictwem okno podręczne: [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) i [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/). Ich renderowaniem odpowiednie kontrolki natywne na każdej z platform.

## <a name="displaying-an-alert"></a>Wyświetlanie alertu

Wszystkie obsługiwane platformy Xamarin.Forms platformy ma modalne okno podręczne użytkownika lub prostego pytania z nich. Aby wyświetlić te alerty platformy Xamarin.Forms, należy użyć [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) metody na dowolnym [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Następujący wiersz kodu przedstawiono prosty komunikat dla użytkownika:

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Okna dialogowego alertu o jeden z przycisków")

W tym przykładzie nie zbiera informacje od użytkownika. Alert wyświetla w trybie modalnym, a po odrzucone użytkownik będzie nadal występować, interakcji z aplikacją.

[ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) Metody można również do przechwytywania przez użytkownika odpowiedzi przez umożliwienie korzystania z dwóch przycisków i zwracanie `boolean`. Aby uzyskać odpowiedzi od alert, należy podać tekst dla obu przycisków i `await` metody. Po wybraniu jednej z opcji odpowiedzi zostanie zwrócony do kodu. Uwaga `async` i `await` słów kluczowych w poniższym przykładowym kodzie:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[ ![DisplayAlert](pop-ups-images/alert2-sml.png "alertów okno dialogowe z dwóch przycisków")](pop-ups-images/alert2.png "alertów okno dialogowe z dwóch przycisków")

## <a name="guiding-users-through-tasks"></a>Wskazówki dla użytkowników za pomocą zadania

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) jest wspólnego elementu interfejsu użytkownika w systemie iOS. Platformy Xamarin.Forms [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/) metody umożliwiają uwzględnienie tego formantu w aplikacjach cross platform, renderowania natywnego alternatyw w systemach Android i Windows Phone.

Aby wyświetlić arkusz akcji `await` [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/) w żadnym [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), przekazywanie wiadomości i przycisk etykiety jako ciągi. Metoda zwraca ciąg Etykieta przycisku, który został kliknięty przez użytkownika. Prosty przykład jest następujący:

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "Okno dialogowe ActionSheet")

`destroy` Przycisk inaczej niż pozostałe renderowania i może pozostać `null` lub został określony jako trzeci parametr ciągu. W poniższym przykładzie użyto `destroy` przycisk:

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[ ![DisplayActionSheet](pop-ups-images/action2-sml.png "okna dialogowego arkusza działania przyciskiem Destroy")](pop-ups-images/action2.png "okna dialogowego arkusza działania przyciskiem Destroy")

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono przy użyciu alertów i akcji arkusz interfejsów API pytania użytkowników proste i prowadzą użytkowników przez zadania. Platformy Xamarin.Forms ma dwie metody na [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) klasy do interakcji z użytkownikiem za pośrednictwem okno podręczne: [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) i [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/), i są one zarówno z odpowiedniej kontrolki natywne na każdej z platform.



## <a name="related-links"></a>Linki pokrewne

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
