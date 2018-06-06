---
title: Zażądaj przeglądu aplikacji w Xamarin.iOS
description: W tym artykule opisano metody RequestReview tego Apple dodane do systemu iOS 10 i opisano, jak ją wdrożyć w platformy Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 2b329ebad5faaa635d9a791f8760bd5f521de591
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788160"
---
# <a name="request-app-review-in-xamarinios"></a>Zażądaj przeglądu aplikacji w Xamarin.iOS

_W tym artykule opisano metody RequestReview tego Apple dodane do systemu iOS 10 i jak ją wdrożyć w platformy Xamarin.iOS._

Jesteś nowym użytkownikiem iOS 10.3 `RequestReview()` metody umożliwia aplikacji systemu iOS poprosić użytkownika o szybkości lub zapoznaj się z nim. Gdy ta metoda jest wywoływana w aplikacji wysyłki zainstalowanym ze sklepu z aplikacjami, iOS 10 będzie obsługiwać cały klasyfikacji i przejrzeć proces dla deweloperów. Ponieważ ten proces podlega zasadom sklepu z aplikacjami, alert może lub nie mogą być wyświetlane.

![](request-app-review-images/review01.png "Przykładowy wniosku o przegląd alert")

## <a name="requesting-a-rating-or-review"></a>Żąda klasyfikacji lub przeglądu

Gdy `RequestReview()` metody statycznej z `SKStoreReviewController` klasa może zostać wywołana w dowolnym momencie, w których warto środowisko użytkownika, proces przeglądu postanowieniom i obsługiwane zasadom sklepu. W związku z tym ta metoda może lub nie może wyświetlić alert i nigdy nie powinna być wywoływana w odpowiedzi na akcję użytkownika, takich jak naciśnięcie przycisku.

Na przykład aplikacja może zażądać przeglądu po uruchomieniu daną liczbę razy lub gry może zażądać przeglądu po odtwarzacz poziomu.

Do żądania przeglądu zaraz po zakończeniu aplikacji platformy Xamarin.iOS uruchamiania, wprowadź następujące zmiany do `AppDelegate.cs` pliku:

```csharp
using Foundation;
using StoreKit;
using UIKit;

namespace iOSTenThree
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        ...

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Request a review from the user
            SKStoreReviewController.RequestReview ();

            return true;
        }
        
        ...
        
    }
}
```

> [!NOTE]
> Wywoływanie `RequestReview()` w niepełnego rozwoju aplikacji będzie zawsze wyświetlania klasyfikacji i przejrzyj okna dialogowego, więc można przetestować. Dotyczy to aplikacji rozproszonych za pośrednictwem TestFlight, gdzie wywołania metody, które zostaną zignorowane.

Gdy `RequestReview()` metoda jest wywoływana w aplikacji wysyłki zainstalowanym ze sklepu z aplikacjami, iOS 10 będzie obsługiwać cały proces klasyfikacji i przejrzyj dla deweloperów. Ponownie ponieważ ten proces podlega zasadom sklepu z aplikacjami, alert może lub nie mogą być wyświetlane.

## <a name="linking-to-an-app-store-product-page"></a>Łączenie do sklepu z aplikacjami stronę produktu 

Oprócz nowe `RequestReview` metody, deweloper może nadal zapewnić link bezpośredni do aplikacji stronę produktu w sklepie z aplikacjami z poziomu aplikacji. Dodając `action=write-review` na końcu adresu URL strony produktu zostanie otwarta strona sieci, w którym użytkownik może zapisywać Przegląd aplikacji automatycznie. 

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego metody RequestReview tego Apple dodane do systemu iOS 10 i jak ją wdrożyć w platformy Xamarin.iOS.



## <a name="related-links"></a>Linki pokrewne

- [iOSTenThree próbki](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
