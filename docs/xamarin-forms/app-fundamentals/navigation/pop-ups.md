---
title: Wyświetlanie wyskakujących okienek
description: Zestaw narzędzi Xamarin.Forms zawiera dwa elementy interfejsu użytkownika podręcznego dokonywania podobne — alert i arkuszu akcji. W tym artykule przedstawiono korzystania z arkusza alertów i działań interfejsów API, użytkownicy sami nie aktualizowali proste pytanie i prowadzą użytkowników przez zadania.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 156c2f9dca47a7755d4f810d7921a05662388ded
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996717"
---
# <a name="displaying-pop-ups"></a>Wyświetlanie wyskakujących okienek

_Zestaw narzędzi Xamarin.Forms zawiera dwa elementy interfejsu użytkownika podręcznego dokonywania podobne — alert i arkuszu akcji. W tym artykule przedstawiono korzystania z arkusza alertów i działań interfejsów API, użytkownicy sami nie aktualizowali proste pytanie i prowadzą użytkowników przez zadania._

Wyświetlanie alertów i monitem użytkownika o dokonanie wyboru jest typowym zadaniem interfejsu użytkownika. Zestaw narzędzi Xamarin.Forms oferuje dwie metody [ `Page` ](xref:Xamarin.Forms.Page) klasy do interakcji z użytkownikiem za pośrednictwem okno podręczne: [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) i [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*). Są one renderowane przy użyciu odpowiednich natywne kontrolki na każdej platformie.

## <a name="displaying-an-alert"></a>Wyświetlanie alertu

Wszystkie platformy obsługiwane przez zestaw narzędzi Xamarin.Forms mają modalne okno podręczne ostrzegać użytkowników lub proste pytania z nich. Aby wyświetlić te alerty w interfejsie Xamarin.Forms, użyj [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) metody na dowolnym [ `Page` ](xref:Xamarin.Forms.Page). Następujący wiersz kodu przedstawia proste komunikat dla użytkownika:

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Okna dialogowego alertu przy użyciu jednego przycisku")

W tym przykładzie nie zbiera informacji od użytkownika. Ten alert zawiera trybie modalnym i po użytkownik odrzucony interakcji z aplikacją.

[ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) Metodę można również do przechwytywania przez użytkownika odpowiedzi przez umożliwienie korzystania z dwóch przycisków i zwracanie `boolean`. Aby uzyskać odpowiedzi z poziomu alertu, podaj tekst dla obu przycisków i `await` metody. Po wybraniu jednej z opcji odpowiedzi zostanie zwrócony do kodu. Uwaga `async` i `await` słów kluczowych w poniższym przykładowym kodzie:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "Alert, okno dialogowe z dwóch przycisków")](pop-ups-images/alert2.png#lightbox "Alert, okno dialogowe z dwóch przycisków")

## <a name="guiding-users-through-tasks"></a>Wskazówki użytkowników za pomocą zadań

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) jest wspólne elementu interfejsu użytkownika w systemie iOS. Xamarin.Forms [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*) metody umożliwiają uwzględnienie tej kontrolki w aplikacji dla wielu platform, renderowanie alternatywy natywnych w systemach Android i platformy uniwersalnej systemu Windows.

Aby wyświetlić arkusz akcji `await` [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*) w dowolnym [ `Page` ](xref:Xamarin.Forms.Page), przekazywanie wiadomości i przycisk etykiety jako ciągi. Metoda zwraca ciąg znaków etykiety przycisku, który został kliknięty przez użytkownika. Prostym przykładem jest następujący:

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "Okno dialogowe ActionSheet")

`destroy` Przycisk inaczej niż pozostałe renderowania i może pozostać `null` lub został określony jako trzeci parametr ciągu. W poniższym przykładzie użyto `destroy` przycisku:

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "okno dialogowe arkusza akcji za pomocą przycisku Destroy")](pop-ups-images/action2.png#lightbox "okno dialogowe arkusza akcji za pomocą przycisku Destroy")

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, korzystania z arkusza alertów i działań interfejsów API, użytkownicy sami nie aktualizowali proste pytanie i prowadzą użytkowników przez zadania. Zestaw narzędzi Xamarin.Forms oferuje dwie metody [ `Page` ](xref:Xamarin.Forms.Page) klasy do interakcji z użytkownikiem za pośrednictwem okno podręczne: [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) i [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*), i są one zarówno renderowany przy użyciu odpowiednich natywne kontrolki na każdej platformie.



## <a name="related-links"></a>Linki pokrewne

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
