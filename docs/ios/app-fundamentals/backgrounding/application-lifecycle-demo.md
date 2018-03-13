---
title: "Wersja demonstracyjna cyklem życia aplikacji"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3d68b1e38ecb5b1833b818dd2a9fb7a5c84f9206
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="application-lifecycle-demo"></a>Wersja demonstracyjna cyklem życia aplikacji

W tej sekcji zamierzamy zbadać aplikację, która przedstawia cztery stany aplikacji i roli `AppDelegate` metody powiadamiając stosowania uzyskać zmiany stanów. Aplikacja będzie drukować aktualizacje w konsoli, zmianie stan aplikacji:

 [![](application-lifecycle-demo-images/image3.png "Przykładowa aplikacja")](application-lifecycle-demo-images/image3.png#lightbox)

 [![](application-lifecycle-demo-images/image4.png "Aplikacja będzie drukować aktualizacje w konsoli, zmianie stan aplikacji")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>Wskazówki


  1. Otwórz _cyklu życia_ projektu w _LifecycleDemo_ rozwiązania.
  1. Otwórz `AppDelegate` klasy. Należy zwrócić uwagę, metod cyklu życia, aby poinformować nas, jeśli aplikacja została zmieniona stanu dodaliśmy rejestrowania:

            ```chsarp
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
  1. Kliknij przycisk Strona główna na symulator lub urządzenie, aby przenieść aplikację do tła. `OnResignActivation` i `DidEnterBackground` zostanie wywołana jako przejścia do aplikacji z `Active` do `Inactive` i do `Backgrounded` stanu. Ponieważ firma Microsoft nie spowodowały naszej aplikacji kod do wykonania w tle, aplikacja jest uznawany za _zawieszone_ w pamięci.
  1. Przejdź z powrotem do aplikacji, aby przywrócić go do na pierwszym planie. `WillEnterForeground` i `OnActivated` zarówno zostanie wywołana:

        ![](application-lifecycle-demo-images/image4.png "Zmiany stanu wypisywane w konsoli programu")

    Należy zwrócić uwagę na to, czy dodano następujący wiersz kodu do kontrolera widoku powiadamiania aplikacja została wprowadzona pierwszego planu w tle:

        ```csharp
            UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
                    label.Text = "Welcome back!";
                });
        ```

1. Naciśnij klawisz **Home** przycisk, aby przełączyć aplikację w tle. Następnie dwukrotnego **Home** przycisk, aby wyświetlić przełącznik aplikacji:
    
    ![](application-lifecycle-demo-images/app-switcher-.png "Przełącznik aplikacji")
  
1. Zlokalizuj aplikacji przełącznik aplikacji, a następnie szybko przesuń się w celu usunięcia go:
    
    ![](application-lifecycle-demo-images/app-switcher-swipe-.png "Przejdź do Usuń uruchomionej aplikacji") 
    
iOS spowoduje przerwanie aplikacji. Należy pamiętać, że `WillTerminate` nie jest wywoływany, ponieważ będziemy są przerywanie aplikacji, która jest _zawieszone_ w tle.

Teraz, gdy Rozumiemy Stany aplikacji systemu iOS i przejść, firma Microsoft będzie Spójrz na różnych opcji dostępnych dla backgrounding w systemie iOS.



## <a name="related-links"></a>Linki pokrewne

- [LifecycleDemo(Part2) (przykład)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
