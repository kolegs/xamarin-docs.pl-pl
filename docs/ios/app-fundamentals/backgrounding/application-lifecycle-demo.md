---
title: Pokaz dotyczący cyklu życia aplikacji dla platformy Xamarin.iOS
description: W tym dokumencie sprawdza, czy różne zdarzenia cyklu życia, obsługiwane przez delegata aplikacji w aplikacji systemu iOS, demonstrując, kiedy i jak te zdarzenia są obsługiwane.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/17/2018
ms.openlocfilehash: 53ae6947cf1483fabe415d6f6521d9384bddb46f
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111176"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Pokaz dotyczący cyklu życia aplikacji dla platformy Xamarin.iOS

W tym artykule i [przykładowego kodu](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/) pokazuje stanów cztery aplikacji w systemie iOS, a rola `AppDelegate` metod powiadamiania aplikacji podczas pobierania zmienionych stanów. Aplikacja zostanie wydrukowany aktualizacje w konsoli, zmianie stan aplikacji:

[![](application-lifecycle-demo-images/image3-sml.png "Przykładowa aplikacja")](application-lifecycle-demo-images/image3.png#lightbox)

[![](application-lifecycle-demo-images/image4.png "Aplikacja zostanie wydrukowany aktualizacje w konsoli, zmianie stan aplikacji")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>Przewodnik

1. Otwórz **cyklu życia** projektu w **LifecycleDemo** rozwiązania.
1. Otwórz `AppDelegate` klasy. Rejestrowanie został dodany do metod cyklu życia, aby wskazać, kiedy zmienił się stan aplikacji:

    ```csharp
    public override void OnActivated(UIApplication application)
    {
        Console.WriteLine("OnActivated called, App is active.");
    }
    public override void WillEnterForeground(UIApplication application)
    {
        Console.WriteLine("App will enter foreground");
    }
    public override void OnResignActivation(UIApplication application)
    {
        Console.WriteLine("OnResignActivation called, App moving to inactive state.");
    }
    public override void DidEnterBackground(UIApplication application)
    {
        Console.WriteLine("App entering background state.");
    }
    // not guaranteed that this will run
    public override void WillTerminate(UIApplication application)
    {
        Console.WriteLine("App is terminating.");
    }
    ```

1. Uruchom aplikację w symulatorze lub na urządzeniu. `OnActivated` zostanie wywołana po uruchomieniu aplikacji. Aplikacja jest teraz w _Active_ stanu.
1. Naciśnij przycisk Strona główna na urządzeniu, aby wyświetlić ją w tle lub symulatora. `OnResignActivation` i `DidEnterBackground` zostanie wywołana jako przejścia do aplikacji z `Active` do `Inactive` do `Backgrounded` stanu. Ponieważ nie ustawiono kod aplikacji, można wykonać w tle, aplikacja jest uznawana za _zawieszone_ w pamięci.
1. Przejdź z powrotem do aplikacji, aby przywrócić je na pierwszym planie. `WillEnterForeground` i `OnActivated` zarówno zostanie wywołana metoda:

    ![](application-lifecycle-demo-images/image4.png "Zmiany stanu drukowane w konsoli")

    Następujący wiersz kodu na kontrolerze widoku jest wykonywany, gdy aplikacja została wprowadzona na pierwszym planie z tła i zmieni tekst wyświetlany na ekranie:

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. Naciśnij klawisz **Home** przycisk, aby umieścić aplikację w tle. Następnie dotknij **Home** przycisk, aby wyświetlić przełącznik aplikacji. Na telefonie iPhone X Przesuń w górę w dolnej części ekranu:

    [![Przełącznik aplikacji](application-lifecycle-demo-images/app-switcher-sml.png "przełącznik aplikacji")](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. Odszukaj aplikację w przełączniku aplikacji, a następnie przesuń w górę można usunąć ją (dla systemu iOS 11, długie naciśnięcie aż ikony czerwonego pojawiają się w prawym górnym rogu):

    [![Przesuń do uruchomionej aplikacji Usuń](application-lifecycle-demo-images/app-switcher-swipe-sml.png "przesunięcia do usuwania uruchomionej aplikacji")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

dla systemu iOS zostanie zakończona aplikacja. Należy pamiętać, że `WillTerminate` nie jest wywoływana, ponieważ aplikacja jest już _zawieszone_ w tle.

## <a name="related-links"></a>Linki pokrewne

- [LifecycleDemo (przykład)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
