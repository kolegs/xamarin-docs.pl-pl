---
title: Praca z wątku interfejsu użytkownika platformy Xamarin.iOS
description: Ten dokument zawiera opis sposobu pracy z wątku interfejsu użytkownika w Xamarin.iOS. W tym artykule omówiono wykonywanie wątków interfejsu użytkownika, zawiera przykład wątku tła i sprawdza async/await.
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 4328b84625aff4c92d6e97029ced7dde747d4fc4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790412"
---
# <a name="working-with-the-ui-thread-in-xamarinios"></a>Praca z wątku interfejsu użytkownika platformy Xamarin.iOS

Interfejsy użytkownika aplikacji są zawsze jednowątkowe, nawet w przypadku urządzeń wielowątkowych — istnieje tylko jeden reprezentację ekranu i wszelkie zmiany wyświetlanych muszą być skoordynowane za pośrednictwem jednego "punkt dostępu". Zapobiega to próby zaktualizowania tej samej piksel w tym samym czasie (na przykład) przez wiele wątków.

Kod należy wykonać tylko wątku zmiany formantów interfejsu z głównej użytkownika (lub interfejsu użytkownika). Wszelkie aktualizacje interfejsu użytkownika, które występują w innym wątku (np. wątek wywołania zwrotnego lub w tle) nie może pobrać wyświetlony na ekranie lub nawet może spowodować awarię.

## <a name="ui-thread-execution"></a>Wykonanie wątku interfejsu użytkownika

Podczas tworzenia formantów w widoku, lub Obsługa zdarzenia zainicjowane przez użytkownika, takie jak touch, kod jest już wykonywane w kontekście wątku interfejsu użytkownika.

Jeśli kod jest wykonywany w wątku w tle, zadania lub wywołania zwrotnego następnie prawdopodobnie nie może być wykonane w głównym wątku interfejsu użytkownika. W takim przypadku zawijania kod w wywołaniu `InvokeOnMainThread` lub `BeginInvokeOnMainThread` podobnie do następującej:

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

`InvokeOnMainThread` Metoda jest zdefiniowana na `NSObject` , może zostać wywołane z wnętrza metody zdefiniowane dowolnego obiektu UIKit (np. widok lub View Controller).

Podczas debugowania aplikacji platformy Xamarin.iOS, zostanie zgłoszony błąd jeśli kod spróbuje dostęp do formantów interfejsu użytkownika z niewłaściwego wątku. Dzięki temu można wyśledzić i rozwiązać te problemy przy użyciu metody InvokeOnMainThread. To tylko występuje podczas debugowania i nie zgłasza błąd w kompilacjach wydania. Takie pojawi się komunikat o błędzie:

 ![](ui-thread-images/image10.png "Wykonanie wątku interfejsu użytkownika")

 <a name="Background_Thread_Example" />


## <a name="background-thread-example"></a>Przykład wątku w tle

Oto przykład, który próbuje uzyskać dostęp kontrolki interfejsu użytkownika ( `UILabel`) z wątku w tle przy użyciu prostego wątku:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

Zgłosi kodu `UIKitThreadAccessException` podczas debugowania. Aby rozwiązać ten problem (i upewnij się, że formant interfejsu użytkownika jest dostępne tylko z wątku interfejsu użytkownika głównego), zawijać każdy kod odwołujący się do formantów interfejsu użytkownika wewnątrz `InvokeOnMainThread` wyrażenie w następujący sposób:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

Nie należy jak używana w pozostałej części Przykłady w tym dokumencie, ale jest ważne pojęcia do zapamiętania, gdy aplikacja zgłasza żądania dotyczące sieci, korzysta z Centrum powiadomień lub inne metody, które wymagają wykonania — program obsługi, który zostanie uruchomiony na innym Wątek.

 <a name="Async_Await_Example" />


## <a name="asyncawait-example"></a>Przykład Async/Await

Gdy słów kluczowych async/await C# 5 `InvokeOnMainThread` nie jest wymagane, ponieważ po zakończeniu wykonywania zadań Oczekiwano metody będzie kontynuowane w wątku wywołującym.

Ten przykład kodu (który oczekiwanie na wywołania metody opóźnienie wyłącznie w celach demonstracyjnych) zawiera to metoda asynchroniczna, która jest wywoływana w wątku interfejsu użytkownika (jest obsługi TouchUpInside). Ponieważ zawierającego metoda jest wywoływana w wątku interfejsu użytkownika, operacje interfejsu użytkownika, takich jak ustawienie tekstu na `UILabel` lub przedstawiający `UIAlertView` można bezpiecznie wywołać po zakończeniu operacji asynchronicznych na wątki w tle.

```csharp
async partial void button2_TouchUpInside (UIButton sender)
{
    textfield1.ResignFirstResponder ();
    textfield2.ResignFirstResponder ();
    textview1.ResignFirstResponder ();
    label1.Text = "async method started";
    await Task.Delay(1000); // example purpose only
    label1.Text = "1 second passed";
    await Task.Delay(2000);
    label1.Text = "2 more seconds passed";
    await Task.Delay(1000);
    new UIAlertView("Async method complete", "This method", 
               null, "Cancel", null)
        .Show();
    label1.Text = "async method completed";
}
```

Jeśli metoda asynchroniczna jest wywoływana z wątku w tle (nie głównym wątku interfejsu użytkownika) następnie `InvokeOnMainThread` nadal będą wymagane.


## <a name="related-links"></a>Linki pokrewne

- [Formanty (przykład)](https://developer.xamarin.com/samples/Controls/)
- [Wątkowość](~/ios/app-fundamentals/threading.md)
